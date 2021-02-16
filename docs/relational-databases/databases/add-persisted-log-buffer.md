---
description: Hinzufügen von persistentem Protokollpuffer zu einer Datenbank
title: Hinzufügen von persistentem Protokollpuffer zu einer Datenbank
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- PMEM
- persistent memory
- persisted log buffer
- add log file
- create log buffer
- remove log buffer
ms.assetid: 8ead516a-1334-4f40-84b2-509d0a8ffa45
author: briancarrig
ms.author: brcarrig
manager: amitban
ms.openlocfilehash: ab2554561abb1590db33249a0ade4e8458b0fd54
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272371"
---
# <a name="add-persisted-log-buffer-to-a-database"></a>Hinzufügen von persistentem Protokollpuffer zu einer Datenbank
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Thema wird beschrieben, wie Sie einer Datenbank in [!INCLUDE[sqlv15](../../includes/sssql19-md.md)] mithilfe von [!INCLUDE[tsql](../../includes/tsql-md.md)] einen persistenten Protokollpuffer hinzufügen.  
  
## <a name="permissions"></a>Berechtigungen

Erfordert die ALTER-Berechtigung für die Datenbank.  

## <a name="configure-persistent-memory-device-linux"></a>Konfigurieren des persistenten Speichergeräts (Linux)

Hier erfahren Sie mehr zum Konfigurieren eines persistenten Speichergeräts unter [Linux](../../linux/sql-server-linux-configure-pmem.md).

## <a name="configure-persistent-memory-device-windows"></a>Konfigurieren des persistenten Speichergeräts (Windows)

Hier erfahren Sie mehr zum Konfigurieren eines persistenten Speichergeräts unter [Windows](/windows-server/storage/storage-spaces/deploy-pmem/).
  
## <a name="add-a-persisted-log-buffer-to-a-database"></a>Hinzufügen eines persistenten Protokollpuffers zu einer Datenbank  

In den folgenden Beispielen wird ein persistenter Protokollpuffer hinzugefügt.

```sql
ALTER DATABASE <MyDB> 
  ADD LOG FILE 
  (
    NAME = <DAXlog>, 
    FILENAME = '<Filepath to DAX Log File>', 
    SIZE = 20MB
  );
```

Das Volume oder die Bereitstellung der neuen Protokolldatei muss mit DAX (NTFS) formatiert oder mit der DAX-Option (XFS/EXT4) bereitgestellt werden.

## <a name="remove-a-persisted-log-buffer"></a>Entfernen eines persistenten Protokollpuffers

Die Datenbank muss zum sicheren Entfernen eines persistenten Protokollpuffers im Einzelbenutzermodus abgelegt werden, um den persistenten Protokollpuffer zu leeren.

Im folgenden Beispiel wird ein persistenter Protokollpuffer entfernt.

```sql
ALTER DATABASE <MyDB> SET SINGLE_USER;
ALTER DATABASE <MyDB> REMOVE FILE <DAXlog>;
ALTER DATABASE <MyDB> SET MULTI_USER;
```

## <a name="limitations"></a>Einschränkungen

[Transparent Data Encryption (TDE)](../security/encryption/transparent-data-encryption.md) ist nicht mit dem persistenten Protokollpuffer kompatibel.

[Verfügbarkeitsgruppen](../../t-sql/statements/create-availability-group-transact-sql.md) können dieses Feature nur auf sekundären Replikaten verwenden, weil eine normale Protokollschreibsemantik auf dem primären Replikat erforderlich ist. Allerdings muss die kleine Protokolldatei auf allen Knoten erstellt werden (idealerweise auf DAX-Volumes oder -Bereitstellungen).

## <a name="backup-and-restore-operations"></a>Sicherungs- und Wiederherstellungsvorgänge

Es gelten normale Wiederherstellungsbedingungen. Wenn persistenter Protokollpuffer in einem DAX-Volume wieder hergestellt oder bereitgestellt wird, kann er weiterhin verwendet oder andernfalls sicher entfernt werden.
  
## <a name="next-steps"></a>Nächste Schritte

- [Funktionsweise (schnellere Ausführung): Nicht flüchtiger SQL Server-Speicher: Protokollfragmentzwischenspeicherung auf NVDIMM](/archive/blogs/bobsql/how-it-works-it-just-runs-faster-non-volatile-memory-sql-server-tail-of-log-caching-on-nvdimm)
- [Verfügbar gemachte Daten: Wartezeit und Dauerhaftigkeit mit SQL Server 2016](https://channel9.msdn.com/Shows/Data-Exposed/Latency-and-Durability-with-SQL-Server-2016)
- [Beschleunigung der Transaktionscommitwartezeit mit Speicherklassenspeicher in Windows Server 2016/SQL Server 2016 SP1](/archive/blogs/sqlserverstorageengine/transaction-commit-latency-acceleration-using-storage-class-memory-in-windows-server-2016sql-server-2016-sp1)