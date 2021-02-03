---
description: MSSQLSERVER_15581
title: MSSQLSERVER_15581
ms.custom: ''
ms.date: 09/03/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha, VenCher
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 15581 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: e2308e1cbdd139983df14f7620bd4a6b9168ac92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196887"
---
# <a name="mssqlserver_15581"></a>MSSQLSERVER_15581
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|15581|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|SEC_NODBMASTERKEYERR|
|Meldungstext|Erstellen Sie einen Hauptschlüssel in der Datenbank, oder öffnen Sie den Hauptschlüssel in der Sitzung, bevor Sie diesen Vorgang ausführen.|
||

## <a name="explanation"></a>Erklärung

Der Fehler 15581 wird ausgelöst, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine Datenbank nicht wiederherstellen kann, für die TDE (Transparent Data Encryption) aktiviert ist. Im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll wird eine Fehlermeldung wie die folgende angezeigt:

> 2020-01-14 22:16:26.47 spid20s Error: 15581, Schweregrad: 16, Status: 3.  
2020-01-14 22:16:26.47 spid20s Erstellen Sie einen Hauptschlüssel in der Datenbank, oder öffnen Sie den Hauptschlüssel in der Sitzung, bevor Sie diesen Vorgang ausführen.

## <a name="possible-cause"></a>Mögliche Ursache

Dieses Problem tritt auf, wenn die Verschlüsselung durch den Hauptschlüssel des Diensts für den Hauptschlüssel der Datenbank in der Masterdatenbank entfernt wird, wenn der folgende Befehl ausgeführt wird:

```sql
Use master
go
alter master key drop encryption by service master key
```

Der Hauptschlüssel des Dienste wird verwendet, um das Zertifikat zu verschlüsseln, das von dem Hauptschlüssel der Datenbank verwendet wird. Jeder Versuch, die Datenbank mit TDE-Aktivierung zu verwenden, benötigt Zugriff auf den Hauptschlüssel der Datenbank in der Masterdatenbank. Ein Hauptschlüssel, der nicht vom Hauptschlüssel des Diensts verschlüsselt ist, muss geöffnet werden, indem in jeder Sitzung, für die Zugriff auf den Hauptschlüssel erforderlich ist, die Anweisung [OPEN MASTER KEY (Transact-SQL)](../../t-sql/statements/open-master-key-transact-sql.md) zusammen mit einem Kennwort verwendet wird. Da dieser Befehl nicht für Systemsitzungen ausgeführt werden kann, kann die Wiederherstellung für Datenbanken mit TDE-Aktivierung nicht abgeschlossen werden.

## <a name="user-action"></a>Benutzeraktion

Zur Lösung dieses Problems aktivieren Sie die automatische Entschlüsselung des Hauptschlüssels. Führen Sie zu diesem Zweck die folgenden Befehle aus:

```sql
Use master
go
open master key DECRYPTION BY PASSWORD = 'password'
alter master key add encryption by service master key
```

Verwenden Sie die folgende Abfrage, um zu bestimmen, ob die automatische Entschlüsselung des Hauptschlüssels durch den Hauptschlüssel des Diensts für die Masterdatenbank deaktiviert wurde:

```sql
select is_master_key_encrypted_by_server from sys.databases where name = 'master'
```

Wenn diese Abfrage einen Wert von 0 zurückgibt, wurde die automatische Entschlüsselung des Hauptschlüssels durch den Hauptschlüssel des Diensts deaktiviert.

## <a name="more-information"></a>Weitere Informationen

In manchen Fällen scheint die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht zu reagieren. Wenn Sie die dynamische Verwaltungssicht `sys.dm_exec_requests` abfragen, können Sie feststellen, dass der `LogWriter`-Thread und weitere Threads, die DML-Vorgänge durchführen, mit WRITELOG wait_type auf unbestimmte Zeit warten. Andere Sitzungen warten möglicherweise ebenfalls, während sie versuchen, Sperren abzurufen.