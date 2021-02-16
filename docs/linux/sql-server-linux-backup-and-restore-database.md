---
title: Sichern und Wiederherstellen von SQL Server-Datenbanken für Linux
description: Erfahren Sie, wie Sie SQL Server-Datenbanken für Linux sichern und wiederherstellen. Finden Sie auch heraus, wie die Sicherung und Wiederherstellung mit SQL Server Management Studio (SSMS) funktioniert.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 11/14/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: d30090fb-889f-466e-b793-5f284fccc4e6
ms.openlocfilehash: 92210c7d111f44a965383a8c2099068b2030227e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338282"
---
# <a name="backup-and-restore-sql-server-databases-on-linux"></a>Sichern und Wiederherstellen von SQL Server-Datenbanken für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Es stehen viele verschiedene Optionen bereit, um Sicherungen von Datenbanken über SQL Server 2017 für Linux zu erstellen. Auf einem Linux-Server können Sie mit **sqlcmd** eine Verbindung mit dem SQL Server herstellen und Sicherungen erstellen. Von Windows aus können Sie eine Verbindung mit SQL Server für Linux herstellen und Sicherungen über die Benutzeroberfläche erstellen. Die Sicherungsfunktionalität ist plattformübergreifend identisch. Beispielsweise können Sie Datenbanken lokal, auf Remotelaufwerken oder im [Microsoft Azure Blob-Speicherdienst](../relational-databases/backup-restore/sql-server-backup-to-url.md) sichern.

> [!IMPORTANT]
> SQL Server für Linux unterstützt nur das Sichern im Azure Blob Storage mit Blockblobs. Die Verwendung eines Speicherschlüssels für die Sicherung und Wiederherstellung führt zur Verwendung eines Seitenblobs, was nicht unterstützt wird. Verwenden Sie stattdessen eine Shared Access Signature. Weitere Informationen zu Unterschieden zwischen Blockblobs und Seitenblobs finden Sie unter [Sicherung: Blockblobs vs. Seitenblobs](../relational-databases/backup-restore/sql-server-backup-to-url.md#blockbloborpageblob).

## <a name="backup-a-database"></a>Sichern einer Datenbank

Im folgenden Beispiel stellt **sqlcmd** eine Verbindung mit der lokalen SQL Server-Instanz her und führt eine vollständige Sicherung einer Benutzerdatenbank mit dem Namen `demodb` her.

```bash
sqlcmd -S localhost -U SA -Q "BACKUP DATABASE [demodb] TO DISK = N'/var/opt/mssql/data/demodb.bak' WITH NOFORMAT, NOINIT, NAME = 'demodb-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
```

Wenn Sie den Befehl ausführen, fordert SQL Server Sie auf, ein Kennwort einzugeben. Nachdem Sie das Kennwort eingegeben haben, gibt die Shell die Ergebnisse des Sicherungsstatus zurück. Beispiel:

```
Password:
10 percent processed.
21 percent processed.
32 percent processed.
40 percent processed.
51 percent processed.
61 percent processed.
72 percent processed.
80 percent processed.
91 percent processed.
Processed 296 pages for database 'demodb', file 'demodb' on file 1.
100 percent processed.
Processed 2 pages for database 'demodb', file 'demodb_log' on file 1.
BACKUP DATABASE successfully processed 298 pages in 0.064 seconds (36.376 MB/sec).
```

### <a name="backup-the-transaction-log"></a>Sichern des Transaktionsprotokolls

Wenn die Datenbank zum vollständigen Wiederherstellungsmodell gehört, können Sie auch Transaktionsprotokollsicherungen für differenziertere Wiederherstellungsoptionen erstellen. Im folgenden Beispiel stellt **sqlcmd** eine Verbindung mit der lokalen SQL Server-Instanz her und führt eine Transaktionsprotokollsicherung durch.

```bash
sqlcmd -S localhost -U SA -Q "BACKUP LOG [demodb] TO DISK = N'/var/opt/mssql/data/demodb_LogBackup.bak' WITH NOFORMAT, NOINIT, NAME = N'demodb_LogBackup', NOSKIP, NOREWIND, NOUNLOAD, STATS = 5"
```

## <a name="restore-a-database"></a>Wiederherstellen einer Datenbank

Im folgenden Beispiel stellt **sqlcmd** eine Verbindung mit der lokalen Instanz von SQL Server her und stellt die demodb-Datenbank wieder her. Beachten Sie, dass die `NORECOVERY`-Option für zusätzliche Wiederherstellungen von Protokolldateisicherungen verwendet wird. Wenn Sie keine weiteren Protokolldateien wiederherstellen möchten, entfernen Sie die `NORECOVERY`-Option.

```bash
sqlcmd -S localhost -U SA -Q "RESTORE DATABASE [demodb] FROM DISK = N'/var/opt/mssql/data/demodb.bak' WITH FILE = 1, NOUNLOAD, REPLACE, NORECOVERY, STATS = 5"
```

> [!TIP]
> Wenn Sie versehentlich NORECOVERY verwenden, aber keine zusätzlichen Protokolldateisicherungen haben, führen Sie den `RESTORE DATABASE demodb`-Befehl ohne zusätzliche Parameter aus. Dadurch wird die Wiederherstellung abgeschlossen, und die Datenbank bleibt funktionstüchtig.

### <a name="restore-the-transaction-log"></a>Wiederherstellen des Transaktionsprotokolls

Der folgende Befehl stellt die vorherige Transaktionsprotokollsicherung wieder her.

```bash
sqlcmd -S localhost -U SA -Q "RESTORE LOG demodb FROM DISK = N'/var/opt/mssql/data/demodb_LogBackup.bak'"
```

## <a name="backup-and-restore-with-sql-server-management-studio-ssms"></a>Sichern und Wiederherstellen mit SSMS (SQL Server Management Studio)

Sie können SSMS auf einem Windows-Computer verwenden, um eine Verbindung mit einer Linux-Datenbank herzustellen und eine Sicherung über die Benutzeroberfläche durchführen.

>[!NOTE] 
> Verwenden Sie die neueste Version von SSMS, um eine Verbindung mit SQL Server herzustellen. Die aktuelle Version können Sie unter [Herunterladen von SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) herunterladen. Weitere Informationen zum Verwenden von SSMS finden Sie unter [Verwenden von SSMS zum Verwalten von SQL Server für Linux](sql-server-linux-manage-ssms.md).

Die folgenden Schritte führen Sie durch das Erstellen einer Sicherung mit SSMS. 

1. Starten Sie SSMS, und stellen Sie eine Verbindung mit Ihrem Server in SQL Server 2017 für Linux her.

1. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Datenbank, klicken Sie auf **Aufgaben** und dann auf **Sichern**.

1. Überprüfen Sie im Dialogfeld **Datenbank sichern** die Parameter und Optionen, und klicken Sie auf **OK**.
 
SQL Server schließt die Datenbanksicherung ab.

### <a name="restore-with-sql-server-management-studio-ssms"></a>Wiederherstellen mit SSMS (SQL Server Management Studio) 

Die folgenden Schritte führen Sie durch die Wiederherstellung einer Datenbank mit SSMS.

1. Klicken Sie in SSMS mit der rechten Maustaste auf **Datenbanken**, und klicken Sie anschließend auf **Datenbanken wiederherstellen**. 

1. Klicken Sie unter **Quelle** auf **Gerät:** und dann auf die Auslassungspunkte (...).

1. Suchen Sie die Datenbanksicherungsdatei, und klicken Sie auf **OK**. 

1. Überprüfen Sie unter **Wiederherstellungsplan** die Sicherungsdatei und die Einstellungen. Klicken Sie auf **OK**. 

1. SQL Server stellt die Datenbank wieder her. 

## <a name="see-also"></a>Siehe auch

* [Erstellen einer vollständigen Datenbanksicherung (SQL Server)](../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)
* [Sichern eines Transaktionsprotokolls (SQL Server)](../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)
* [BACKUP (Transact-SQL)](../t-sql/statements/backup-transact-sql.md)
* [SQL Server-Sicherung über URL](../relational-databases/backup-restore/sql-server-backup-to-url.md)
