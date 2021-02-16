---
title: 'Schnellstart: Sichern und Wiederherstellen einer Datenbank'
titleSuffix: SQL Server
description: In diesem Artikel erfahren Sie, wie Sie eine neue Datenbank sowie eine Sicherung der Datenbank erstellen und die Sicherung in SQL Server wiederherstellen.
author: cawrites
ms.author: chadam
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: backup-restore
ms.prod_service: backup-restore
ms.assetid: ''
ms.openlocfilehash: 66b4277674c1144a716e5153a9fa062f988c75c0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340208"
---
# <a name="quickstart-backup-and-restore-a-sql-server-database-on-premises"></a>Schnellstart: Lokales Sichern und Wiederherstellen einer SQL Server-Datenbank
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Schnellstart erstellen Sie eine neue Datenbank, legen eine einfache Sicherung dieser Datenbank an und stellen sie dann wieder her. 

Eine ausführlichere Anleitung finden Sie unter [Erstellen einer vollständigen Datenbanksicherung](create-a-full-database-backup-sql-server.md) und [Wiederherstellen einer Sicherung mithilfe von SSMS](restore-a-database-backup-using-ssms.md).

## <a name="prerequisites"></a>Voraussetzungen
Um diesen Schnellstart abzuschließen, benötigen Sie Folgendes: 

- [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)
- [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)

## <a name="create-a-test-database"></a>Erstellen einer Testdatenbank 

1. Starten Sie [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md), und stellen Sie eine Verbindung mit Ihrer SQL Server-Instanz her.
1. Öffnen Sie das Fenster **Neue Abfrage**. 
1. Führen Sie den folgenden Transact-SQL-Code (T-SQL-Code) aus, um die Testdatenbank zu erstellen. Aktualisieren Sie den Knoten **Datenbanken** im **Objekt-Explorer**, um Ihre neue Datenbank anzuzeigen. 

```sql
USE [master]
GO

CREATE DATABASE [SQLTestDB]
GO

USE [SQLTestDB]
GO
CREATE TABLE SQLTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO


USE [SQLTestDB]
GO

INSERT INTO SQLTest (ID, c1) VALUES (1, 'test1')
INSERT INTO SQLTest (ID, c1) VALUES (2, 'test2')
INSERT INTO SQLTest (ID, c1) VALUES (3, 'test3')
INSERT INTO SQLTest (ID, c1) VALUES (4, 'test4')
INSERT INTO SQLTest (ID, c1) VALUES (5, 'test5')
GO

SELECT * FROM SQLTest
GO
```
 
## <a name="take-a-backup"></a>Erstellen einer Sicherung
Um eine Sicherung der Datenbank zu erstellen, führen Sie die folgenden Schritte aus: 

1. Starten Sie [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md), und stellen Sie eine Verbindung mit Ihrer SQL Server-Instanz her.
1. Erweitern Sie den Knoten **Datenbanken** im **Objekt-Explorer**.  
1. Klicken Sie mit der rechten Maustaste auf die Datenbank, zeigen Sie auf **Aufgaben**, und wählen Sie dann **Sichern...** aus. 
1. Bestätigen Sie unter **Ziel**, dass der Pfad für die Sicherung richtig ist. Wenn Sie ihn ändern müssen, wählen Sie **Entfernen** aus, um den vorhandenen Pfad zu entfernen, und klicken Sie dann auf **Hinzufügen**, um einen neuen Pfad einzugeben. Sie können die Auslassungspunkte verwenden, um zu einer bestimmten Datei zu navigieren. 
1. Klicken Sie auf **OK**, um eine Sicherung der Datenbank zu erstellen. 

![Erstellen einer SQL-Sicherung](media/quickstart-backup-restore-database/backup-db-ssms.png)

Alternativ können Sie den folgenden Transact-SQL-Befehl zum Sichern Ihrer Datenbank ausführen: 

```sql
BACKUP DATABASE [SQLTestDB] 
TO DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Backup\SQLTestDB.bak' 
WITH NOFORMAT, NOINIT,  
NAME = N'SQLTestDB-Full Database Backup', SKIP, NOREWIND, NOUNLOAD,  STATS = 10
GO
```


## <a name="restore-a-backup"></a>Wiederherstellen einer Sicherung
Gehen Sie zum Wiederherstellen Ihrer Datenbank wie folgt vor: 

1. Starten Sie [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md), und stellen Sie eine Verbindung mit Ihrer SQL Server-Instanz her.
1. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf den Knoten **Datenbanken**, und wählen Sie dann **Datenbank wiederherstellen...** aus.

    ![Wiederherstellen einer Datenbank](media/quickstart-backup-restore-database/restore-db-ssms1.png)

1. Wählen Sie **Gerät:** und dann die Auslassungspunkte (...) aus, um nach der Sicherungsdatei zu suchen. 
1. Wählen Sie **Hinzufügen** aus, und navigieren Sie zum Speicherort Ihrer `.bak`-Datei. Wählen Sie die `.bak`-Datei aus, und klicken Sie dann auf **OK**. 
1. Klicken Sie auf **OK**, um das Dialogfeld **Sicherungsmedien auswählen** zu schließen. 
1. Klicken Sie auf **OK**, um die Sicherung Ihrer Datenbank wiederherzustellen. 

    ![Wiederherstellen der Datenbank](media/quickstart-backup-restore-database/restore-db-ssms2.png)

Alternativ können Sie das folgende Transact-SQL-Skript zum Wiederherstellen Ihrer Datenbank ausführen:

```sql
USE [master]
RESTORE DATABASE [SQLTestDB] 
FROM DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Backup\SQLTestDB.bak' WITH  FILE = 1,  NOUNLOAD,  STATS = 5
GO
```

### <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Führen Sie den folgenden Transact-SQL-Befehl aus, um die von Ihnen erstellte Datenbank mitsamt ihres Sicherungsverlaufs in der MSDB-Datenbank zu entfernen:

```sql
EXEC msdb.dbo.sp_delete_database_backuphistory @database_name = N'SQLTestDB'
GO

USE [master]
DROP DATABASE [SQLTestDB]
GO
```

## <a name="see-more"></a>Weitere Informationen
[Sichern und Wiederherstellen: Übersicht](back-up-and-restore-of-sql-server-databases.md)
[Sichern in URL](sql-server-backup-to-url.md)
[Erstellen einer vollständigen Sicherung](create-a-full-database-backup-sql-server.md)
[Wiederherstellen einer Datenbanksicherung](restore-a-database-backup-using-ssms.md)
