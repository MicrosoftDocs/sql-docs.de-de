---
description: Verschieben von Benutzerdatenbanken
title: Verschieben von Benutzerdatenbanken | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- disaster recovery [SQL Server], moving database files
- database files [SQL Server], moving
- data files [SQL Server], moving
- editions [SQL Server], moving databases between
- moving full-text catalogs
- scheduled disk maintenace [SQL Server]
- moving databases
- full-text catalogs [SQL Server], moving
- moving database files
- moving user databases
- relocating database files
- planned database relocations [SQL Server]
- databases [SQL Server], moving
ms.assetid: ad9a4e92-13fb-457d-996a-66ffc2d55b79
author: stevestein
ms.author: sstein
ms.openlocfilehash: 64b8416375cdc61c43e8799b33891096858a79f7
ms.sourcegitcommit: 059722ff78a6061b801807416b312ae9f721ec7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100636715"
---
# <a name="move-user-databases"></a>Verschieben von Benutzerdatenbanken
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]können Sie die Daten-, Protokoll- und Volltextkatalogdateien einer Benutzerdatenbank an einen neuen Speicherort verschieben, indem Sie den neuen Dateispeicherort in der FILENAME-Klausel der [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) -Anweisung angeben. Diese Methode ermöglicht das Verschieben von Datenbankdateien innerhalb derselben Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Wenn Sie eine Datenbank auf eine andere Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder einen anderen Server verschieben möchten, verwenden Sie [Sicherungs- und Wiederherstellungs-](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md) oder [Trennungs- und Anfügungsoperationen](../../relational-databases/databases/move-a-database-using-detach-and-attach-transact-sql.md).  
  
## <a name="considerations"></a>Überlegungen  
 Wenn Sie eine Datenbank auf eine andere Serverinstanz verschieben, müssen Sie möglicherweise einen Teil oder auch alle Metadaten für die Datenbank erneut erstellen, um Benutzern und Anwendungen ein konsistentes Verhalten bereitzustellen. Weitere Informationen finden Sie unter [Verwalten von Metadaten beim Bereitstellen einer Datenbank auf einer anderen Serverinstanz &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
 Einige Funktionen von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ändern die Art und Weise, wie [!INCLUDE[ssDE](../../includes/ssde-md.md)] Informationen in den Datenbankdateien speichert. Diese Funktionen sind nicht in allen Editionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verfügbar. Eine Datenbank, die diese Funktionen enthält, kann nicht in eine Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verschoben werden, die sie nicht unterstützt. Verwenden Sie die dynamische Verwaltungssicht sys.dm_db_persisted_sku_features, um alle editionsspezifischen Funktionen aufzulisten, die in der aktuellen Datenbank aktiviert sind.  
  
 Für die Prozeduren in diesem Thema ist der logische Name der Datenbankdateien erforderlich. Zum Abrufen des Namens führen Sie eine Abfrage für die Namensspalte in der [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md) -Katalogsicht aus.  
  
 Ab [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]sind Volltextkataloge in die Datenbank integriert, statt im Dateisystem gespeichert. Die Volltextkataloge werden jetzt automatisch verschoben, wenn Sie eine Datenbank verschieben.  
  
## <a name="planned-relocation-procedure"></a>Prozedur zur geplanten Verschiebung  
 Zum Verschieben einer Daten- oder Protokolldatei im Rahmen einer geplanten Verschiebung müssen Sie die folgenden Schritte ausführen:  
  
1.  Führen Sie für jede zu verschiebende Datei die folgende Anweisung aus.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE ( NAME = logical_name, FILENAME = 'new_path\os_file_name' );  
    ```  
  
2.  Führen Sie die folgende Anweisung aus.  
  
    ```  
    ALTER DATABASE database_name SET OFFLINE;  
    ```  
  
3.  Verschieben Sie die Datei(en) an den neuen Speicherort.  
  
4.  Führen Sie die folgende Anweisung aus.  
  
    ```  
    ALTER DATABASE database_name SET ONLINE;  
    ```  
  
5.  Überprüfen Sie die Dateiänderung durch Ausführen der folgenden Abfrage.  

    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="relocation-for-scheduled-disk-maintenance"></a>Verschiebung aufgrund planmäßiger Datenträgerwartung  
 Zum Verschieben einer Datei im Rahmen eines planmäßigen Datenträgerwartungsprozesses müssen Sie die folgenden Schritte ausführen:  
  
1.  Führen Sie für jede zu verschiebende Datei die folgende Anweisung aus.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE ( NAME = logical_name , FILENAME = 'new_path\os_file_name' );  
    ```  
  
2.  Beenden Sie die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , oder fahren Sie das System für die Wartungsarbeiten herunter. Weitere Informationen finden Sie unter [Starten, Beenden, Anhalten, Fortsetzen und Neustarten von SQL Server-Diensten](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
3.  Verschieben Sie die Datei(en) an den neuen Speicherort.  
  
4.  Starten Sie die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder den Server neu. Weitere Informationen finden Sie unter [Starten, Beenden, Anhalten, Fortsetzen und Neustarten der Datenbank-Engine, SQL Server-Agent oder des SQL Server-Browsers](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
5.  Überprüfen Sie die Dateiänderung durch Ausführen der folgenden Abfrage.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="failure-recovery-procedure"></a>Prozedur zur Wiederherstellung nach Fehlern  
 Wenn eine Datei aufgrund eines Hardwarefehlers verschoben werden muss, müssen Sie die folgenden Schritte ausführen, um die Datei an einen neuen Speicherort zu verschieben:  
  
> [!IMPORTANT]  
>  Wenn die Datenbank nicht gestartet werden kann, d. h., wenn sie als fehlerverdächtig eingestuft wurde oder sich in einem nicht wiederhergestellten Status befindet, können nur Mitglieder der festen Rolle sysadmin die Datei verschieben.  
  
1.  Beenden Sie die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , wenn sie gestartet ist.  
  
2.  Starten Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz im ausschließlichen Wiederherstellungsmodus der master-Datenbank durch Eingeben der folgenden Befehle an der Eingabeaufforderung.  
  
    -   Führen Sie für die Standardinstanz (MSSQLSERVER) den folgenden Befehl aus.  
  
        ```  
        NET START MSSQLSERVER /f /T3608  
        ```  
  
    -   Führen Sie für eine benannte Instanz den folgenden Befehl aus.  
  
        ```  
        NET START MSSQL$instancename /f /T3608  
        ```  
  
     Weitere Informationen finden Sie unter [Starten, Beenden, Anhalten, Fortsetzen und Neustarten von SQL Server-Diensten](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
3.  Verwenden Sie für jede zu verschiebende Datei die **sqlcmd** -Befehle oder [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , um die folgende Anweisung auszuführen:  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE( NAME = logical_name , FILENAME = 'new_path\os_file_name' );  
    ```  
  
     Weitere Informationen zum Verwenden des **sqlcmd** -Hilfsprogramms finden Sie unter [Verwenden des Hilfsprogramms „sqlcmd“](../../ssms/scripting/sqlcmd-use-the-utility.md).  
  
4.  Starten Sie das Hilfsprogramm **sqlcmd** oder [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
5.  Beenden Sie die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
6.  Verschieben Sie die Datei(en) an den neuen Speicherort.  
  
7.  Starten Sie die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Führen Sie z. B. folgenden Befehl aus: `NET START MSSQLSERVER`  
  
8.  Überprüfen Sie die Dateiänderung durch Ausführen der folgenden Abfrage.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Protokolldatei im Rahmen einer geplanten Verschiebung an einen neuen Speicherort verschoben.  
  
```  
USE master;  
GO  
-- Return the logical file name.  
SELECT name, physical_name AS CurrentLocation, state_desc  
FROM sys.master_files  
WHERE database_id = DB_ID(N'AdventureWorks2012')  
    AND type_desc = N'LOG';  
GO  
ALTER DATABASE AdventureWorks2012 SET OFFLINE;  
GO  
-- Physically move the file to a new location.  
-- In the following statement, modify the path specified in FILENAME to  
-- the new location of the file on your server.  
ALTER DATABASE AdventureWorks2012   
    MODIFY FILE ( NAME = AdventureWorks2012_Log,   
                  FILENAME = 'C:\NewLoc\AdventureWorks2012_Log.ldf');  
GO  
ALTER DATABASE AdventureWorks2012 SET ONLINE;  
GO  
--Verify the new location.  
SELECT name, physical_name AS CurrentLocation, state_desc  
FROM sys.master_files  
WHERE database_id = DB_ID(N'AdventureWorks2012')  
    AND type_desc = N'LOG';  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)   
 [Anfügen und Trennen von Datenbanken &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [Verschieben von Systemdatenbanken](../../relational-databases/databases/move-system-databases.md)   
 [Verschieben von Datenbankdateien](../../relational-databases/databases/move-database-files.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Starten, Beenden, Anhalten, Fortsetzen und Neustarten der Datenbank-Engine, SQL Server-Agent oder des SQL Server-Browsers](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
