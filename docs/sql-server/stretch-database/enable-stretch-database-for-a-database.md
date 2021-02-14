---
description: Aktivieren von Stretch Database für eine Datenbank
title: Aktivieren von Stretch Database für eine Datenbank
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, enabling database
- enabling database for Stretch Database
ms.assetid: 37854256-8c99-4566-a552-432e3ea7c6da
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 0b93f34e5a5b716c649f0902dabefe651253c9ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079981"
---
# <a name="enable-stretch-database-for-a-database"></a>Aktivieren von Stretch Database für eine Datenbank
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


  Um eine vorhandene Datenbank für Stretch Database zu konfigurieren, wählen Sie **Aufgaben &gt; Stretch &gt; Aktivieren** für eine Datenbank in SQL Server Management Studio aus, um den Assistenten zum **Aktivieren einer Datenbank für Stretch** zu öffnen. Sie können auch Transact-SQL verwenden, um Stretch Database für eine Datenbank zu aktivieren.  
  
 Wenn Sie **Aufgaben | Stretch | Aktivieren** für eine einzelne Tabelle auswählen und Sie die Datenbank noch nicht für Stretch Database aktiviert haben, konfiguriert der Assistent die Datenbank für Stretch Database und ermöglicht Ihnen im Zuge dieses Vorgangs auch die Auswahl von Tabellen. Führen Sie die Schritte in diesem Artikel statt der Schritte in [Aktivieren von Stretch Database für eine Tabelle](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) aus.  
  
 Um Stretch Database für eine Datenbank oder eine Tabelle zu aktivieren, benötigen Sie „db_owner“-Berechtigungen. Für das Aktivieren von Stretch Database für eine Datenbank sind ebenfalls CONTROL DATABASE-Berechtigungen erforderlich.  

> [!NOTE]
> Bedenken Sie später beim Deaktivieren von Stretch Database Folgendes: Wenn Sie Stretch Database für eine Tabelle oder eine Datenbank deaktivieren, wird das Remoteobjekt nicht gelöscht. Wenn Sie die Remotetabelle oder Remotedatenbank löschen möchten, müssen Sie sie mithilfe des Azure-Verwaltungsportals löschen. Die Remoteobjekte erzeugen weiterhin Azure-Kosten, bis Sie die Objekte manuell löschen. 
 
## <a name="before-you-get-started"></a>Bevor Sie beginnen  
  
-   Bevor Sie eine Datenbank für Stretch konfigurieren, empfiehlt es sich, den Stretch Database-Ratgeber auszuführen, um für Stretch geeignete Datenbanken und Tabellen zu identifizieren. Der Stretch Database-Ratgeber ermittelt auch Blockierungsprobleme. Weitere Informationen finden Sie unter [Identifizieren von Datenbanken und Tabellen für Stretch Database durch Ausführen des Ratgebers für Stretch Database](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md).  
  
-   Lesen Sie [Einschränkungen für Stretch Database](../../sql-server/stretch-database/limitations-for-stretch-database.md).  
  
-   Stretch Database migriert Daten in Azure. Daher benötigen Sie ein Azure-Konto und ein Abonnement für die Abrechnung. [Klicken Sie hier](https://azure.microsoft.com/pricing/free-trial/), um ein Azure-Konto zu erstellen.  
  
-   Erhalten Sie die erforderlichen Informationen zu Verbindungen und Anmeldungen, um einen neuen Azure-Server zu erstellen oder einen vorhandenen Azure-Server auszuwählen.  
  
##  <a name="prerequisite-enable-stretch-database-on-the-server"></a><a name="EnableTSQLServer"></a> Voraussetzung: Aktivieren von Stretch Database auf dem Server  
 Bevor Sie Stretch Database für eine Datenbank oder eine Tabelle aktivieren können, müssen Sie sie auf dem lokalen Server aktivieren. Dieser Vorgang erfordert die Berechtigung „Systemadministrator“ oder „Severadministrator“.  
  
-   Wenn Sie über die erforderlichen Administratorberechtigungen verfügen, konfiguriert der Assistent **Datenbank für Stretch aktivieren** den Server für Stretch.  
  
-   Wenn Sie nicht über die erforderlichen Berechtigungen verfügen, muss ein Administrator die Option manuell durch Ausführen von **sp_configure** aktivieren, bevor Sie den Assistenten ausführen. Alternativ muss ein Administrator den Assistenten ausführen.  
  
 Führen Sie zum manuellen Aktivieren von Stretch Database auf dem Server **sp_configure** aus, und aktivieren Sie die Option **remote data archive** . Im folgenden Beispiel wird die Option **remote data archive** aktiviert, indem ihr Wert auf 1 festgelegt wird.  
  
```sql
EXEC sp_configure 'remote data archive' , '1';  
GO

RECONFIGURE;  
GO  
```  
  
 Weitere Informationen finden Sie unter [Configure the remote data archive Server Configuration Option](../../database-engine/configure-windows/configure-the-remote-data-archive-server-configuration-option.md) (Konfigurieren der Serverkonfigurationsoption „Remotedatenarchiv“) und [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
##  <a name="use-the-wizard-to-enable-stretch-database-on-a-database"></a><a name="Wizard"></a> Verwenden des Assistenten zum Aktivieren von Stretch Database für eine Datenbank  
 Informationen zum Assistenten „Datenbank für Stretch aktivieren“ (einschließlich der von Ihnen einzugebenden Informationen und der auszuwählenden Angaben) finden Sie unter [Get started by running the Enable Database for Stretch Wizard](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md) (Erste Schritte mit dem Assistenten „Datenbank für Stretch aktivieren“).  
  
##  <a name="use-transact-sql-to-enable-stretch-database-on-a-database"></a><a name="EnableTSQLDatabase"></a> Verwenden von Transact-SQL zum Aktivieren von Stretch Database für eine Datenbank  
 Bevor Sie Stretch Database für einzelne Tabellen aktivieren können, müssen Sie sie auf dem lokalen Server aktivieren.  
  
 Um Stretch Database für eine Datenbank oder eine Tabelle zu aktivieren, benötigen Sie „db_owner“-Berechtigungen. Für das Aktivieren von Stretch Database für eine Datenbank sind ebenfalls CONTROL DATABASE-Berechtigungen erforderlich.  
  
1.  Wählen Sie vor Beginn einen vorhandenen Azure-Server für die Daten aus, die Stretch Database migriert, oder erstellen Sie einen neuen Azure-Server.  
  
2.  Erstellen Sie auf dem Azure-Server eine Firewallregel mit dem IP-Adressbereich der SQL Server-Instanz, sodass SQL Server mit dem Remoteserver kommunizieren kann.  

    Sie können die erforderlichen Werte problemlos finden und die Firewallregel erstellen, indem Sie versuchen, über den Objekt-Explorer in SQL Server Management Studio (SSMS) eine Verbindung mit dem Azure-Server herzustellen. SSMS unterstützt Sie beim Erstellen der Regel, indem das folgende Dialogfeld geöffnet wird, das die erforderlichen IP-Adresswerte bereits enthält.
    
    ![Firewallregel für Stretch](../../sql-server/stretch-database/media/firewall-rule-for-stretch.png)
  
3.  Zum Konfigurieren einer SQL Server-Datenbank für Stretch Database muss die Datenbank über einen Datenbankhauptschlüssel verfügen. Der Datenbankhauptschlüssel sichert die Anmeldeinformationen, die Stretch Database für die Verbindung mit der Remotedatenbank verwendet. Nachstehend finden Sie ein Beispiel, in dem ein neuer Datenbank-Hauptschlüssel erstellt wird.  
  
    ```sql  
    USE <database>; 
    GO  
  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD='<password>'; 
    GO
    ```  
    Weitere Informationen zum Datenbank-Hauptschlüssel finden Sie unter [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md) und [Erstellen eines Datenbank-Hauptschlüssels](../../relational-databases/security/encryption/create-a-database-master-key.md).
    
4.  Wenn Sie eine Datenbank für Stretch Database konfigurieren, müssen Sie die Anmeldeinformationen für Stretch Database angeben, um die Kommunikation zwischen dem lokalen SQL Server-Computer und dem Azure-Remoteserver zu ermöglichen. Sie haben zwei Möglichkeiten.  
  
    -   Sie können die Anmeldeinformationen eines Administrators angeben.  
  
        -   Wenn Sie Stretch Database durch Ausführen des Assistenten aktivieren, können Sie die Anmeldeinformationen zu diesem Zeitpunkt erstellen.  
  
        -   Wenn Sie Stretch Database mithilfe von **ALTER DATABASE** aktivieren möchten, müssen Sie die Anmeldeinformationen manuell erstellen, bevor Sie **ALTER DATABASE** zum Aktivieren von Stretch Database ausführen. 
        
        Nachstehend finden Sie ein Beispiel, in dem neue Anmeldeinformationen erstellt werden.
  
        ```sql  
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>  
            WITH IDENTITY = '<identity>' , SECRET = '<secret>' ;
        GO   
        ```  

         Weitere Informationen zu Anmeldeinformationen finden Sie unter[CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md). Zum Erstellen von Anmeldeinformationen ist die ALTER ANY CREDENTIAL-Berechtigung erforderlich.  

    -   Sie können ein Verbunddienstkonto verwenden, damit SQL Server mit dem Azure-Remoteserver kommunizieren kann, wenn alle der folgenden Bedingungen zutreffen.  
  
        -   Das Dienstkonto, unter dem die SQL Server-Instanz ausgeführt wird, ist ein Domänenkonto.  
  
        -   Das Domänenkonto gehört zu einer Domäne, deren Active Directory mit Azure Active Directory verbunden ist.  
  
        -   Der Azure-Remoteserver wird konfiguriert, um die Azure Active Directory-Authentifizierung zu unterstützen.  
  
        -   Das Dienstkonto, unter dem die SQL Server-Instanz ausgeführt wird, muss auf dem Azure-Remoteserver als ein dbmanager- oder sysadmin-Konto konfiguriert worden sein.  
  
5.  Um eine Datenbank für Stretch Database zu konfigurieren, führen Sie den Befehl ALTER DATABASE aus.  
  
    1.  Stellen Sie für das SERVER-Argument den Namen eines vorhandenen Azure-Servers bereit, einschließlich des Namensteils `.database.windows.net` , z.B. `MyStretchDatabaseServer.database.windows.net`.  
  
    2.  Stellen Sie vorhandene Administratoranmeldeinformationen mit dem Argument CREDENTIAL bereit, oder geben Sie FEDERATED_SERVICE_ACCOUNT = ON an. Das folgenden Beispiel enthält vorhandene Anmeldeinformationen:  
  
    ```sql  
    ALTER DATABASE <database name>  
        SET REMOTE_DATA_ARCHIVE = ON  
            (  
                SERVER = '<server_name>' ,  
                CREDENTIAL = <db_scoped_credential_name>  
            ) ;  
    GO
    ```  
  
## <a name="next-steps"></a>Nächste Schritte  
-   [Aktivieren von Stretch Database für eine Tabelle](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) zum Aktivieren zusätzlicher Tabellen  
  
-   [Überwachung und Problembehandlung bei der Datenmigration &#40;Stretch Database&#41;](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md) zum Anzeigen des Status der Datenmigration.  
  
-   [Anhalten und Fortsetzen der Datenmigration &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)  
  
-   [Verwalten von Stretch Database und Behandeln von Problemen](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)  
  
-   [Sichern von Stretch-fähigen Datenbanken](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
  
-   [Wiederherstellen von Stretch-fähigen Datenbanken](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Identifizieren von Datenbanken und Tabellen für Stretch Database durch Ausführen des Ratgebers für Stretch Database](../../sql-server/stretch-database/stretch-database-databases-and-tables-stretch-database-advisor.md)   
 [ALTER DATABASE SET-Optionen &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
  
  
