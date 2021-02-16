---
description: 'Tutorial: Verwenden des Microsoft Azure BLOB-Speicherdiensts mit SQL Server 2016'
title: 'Tutorial: Verwenden des Microsoft Azure BLOB-Speicherdiensts mit SQL Server 2016'
ms.custom: seo-dt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.technology: ''
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server 2016
ms.assetid: e69be67d-da1c-41ae-8c9a-6b12c8c2fb61
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4a87a9842ac5fdf05de192d1fc562fe241441c75
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340039"
---
# <a name="tutorial-use-azure-blob-storage-service-with-sql-server-2016"></a>Tutorial: Verwenden des Microsoft Azure BLOB-Speicherdiensts mit SQL Server 2016

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
Willkommen zum Tutorial über das Arbeiten mit SQL Server 2016 im Dienst Microsoft Azure Blob Storage. Dieses Tutorial hilft Ihnen dabei, zu verstehen, wie der Dienst Microsoft Azure Blob Storage für SQL Server-Datendateien und SQL Server-Sicherungen verwendet wird.  
  
Die Unterstützung der SQL Server-Integration für den Dienst Microsoft Azure Blob Storage startete als eine Erweiterung von SQL Server 2012 Servicepack 1 CU2 und wurde zudem mit SQL Server 2014 und SQL Server 2016 erweitert. Eine Übersicht der Funktionen und derer Vorteile finden Sie unter [SQL Server-Datendateien in Microsoft Azure](../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md). Eine Livedemo finden Sie unter [Demo of Point in Time Restore](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo)(in englischer Sprache).  

In diesem Tutorial erfahren Sie in mehreren Abschnitten, wie Sie im Dienst Microsoft Azure Blob Storage mit SQL Server-Datendateien arbeiten. Jeder Abschnitt ist auf eine bestimmte Aufgabe fokussiert, und die Abschnitte sollten nacheinander ausgeführt werden. Zunächst erfahren Sie, wie Sie in Blob Storage einen neuen Container mit einer gespeicherten Zugriffsrichtlinie und einer Shared Access Signature (SAS) erstellen. Danach erfahren Sie, wie Sie SQL Server-Anmeldeinformationen erstellen, um SQL Server in Azure Blob Storage zu integrieren. Als Nächstes sichern Sie eine Datenbank in Blob Storage und stellen sie in einem virtuellen Azure-Computer wieder her. Anschließend verwenden Sie die Protokollsicherung für die Übertragung von Dateimomentaufnahmen von SQL Server 2016, um den Stand eines bestimmten Zeitpunkts in einer neuen Datenbank wiederherzustellen. Das Tutorial wird schließlich die Verwendung der gespeicherten Meta Data-Prozeduren und -Funktionen demonstrieren, um Ihnen dabei zu helfen, Dateimomentaufnahme-Sicherungen zu verstehen und mit ihnen zu arbeiten.
  
## <a name="prerequisites"></a>Voraussetzungen

Um dieses Tutorial abzuschließen, müssen Sie mit den Sicherungs- und Wiederherstellungskonzepten in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] und der T-SQL-Syntax vertraut sein. Zur Verwendung dieses Tutorials benötigen Sie ein Azure-Speicherkonto, SQL Server Management Studio (SSMS), Zugriff auf eine Instanz eines lokalen SQL Servers, Zugriff auf einen virtuellen Azure-Computer (VM), auf dem SQL Server 2016 ausgeführt wird, und eine AdventureWorks2016-Datenbank. Darüber hinaus sollte das zum Ausgeben von BACKUP- oder RESTORE-Befehlen verwendete Benutzerkonto Mitglied der Datenbankrolle **db_backupoperator** sein und über Berechtigungen zum **Ändern beliebiger Anmeldeinformationen** verfügen. 

- Erstellen Sie ein kostenloses [Azure-Konto](https://azure.microsoft.com/offers/ms-azr-0044p/).
- Erstellen Sie ein [Azure-Speicherkonto](/azure/storage/common/storage-quickstart-create-account?tabs=portal).
- Installieren Sie die [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Bereitstellen einer [Azure-VM, die SQL Server ausführt](https://azure.microsoft.com/services/virtual-machines/sql-server/)
- Installieren Sie [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md).
- Laden Sie die [AdventureWorks 2016-Beispieldatenbank](../samples/adventureworks-install-configure.md) herunter.
- Weisen Sie das Benutzerkonto der Rolle des [db_backupoperator](./security/authentication-access/database-level-roles.md) zu und gewähren Sie die Berechtigung zum [Ändern beliebiger Anmeldeinformationen](../t-sql/statements/alter-credential-transact-sql.md). 

## <a name="1---create-stored-access-policy-and-shared-access-storage"></a>1: Erstellen einer gespeicherten Zugriffsrichtlinie und von Speicher mit freigegebenem Zugriff

In diesem Abschnitt verwenden Sie ein [Azure PowerShell](/powershell/azure/)-Skript zum Erstellen einer SAS für einen Azure-Blobcontainer mithilfe einer gespeicherten Zugriffsrichtlinie.  
  
> [!NOTE]  
> Dieses Skript wurde mit Azure PowerShell 5.0.10586 geschrieben.  
  
Eine SAS ist ein URI, der eingeschränkte Zugriffsrechte für Container, Blobs, Warteschlangen oder Tabellen gewährt. Eine gespeicherte Zugriffsrichtlinie stellt eine zusätzliche Steuerungsebene für Shared Access Signatures auf Serverseite bereit, einschließlich das Aufheben, Ablaufen oder Erweitern des Zugriffs. Wenn Sie diese neue Erweiterung verwenden, müssen Sie eine Richtlinie für einen Container mit mindestens Lese-, Schreib- und Auflistungsrechten erstellen.  
  
Sie können eine gespeicherte Zugriffsrichtlinie und eine SAS mithilfe von Azure PowerShell, mithilfe des Azure Storage SDKs, der Azure-REST-API oder einem Hilfsprogramm von Drittanbietern erstellen. Dieses Tutorial veranschaulicht, wie ein Azure PowerShell-Skript verwendet wird, um diesen Vorgang abzuschließen. Das Skript verwendet das Bereitstellungsmodell des Ressourcen-Managers und erstellt die nachstehenden neuen Ressourcen.  
  
-   Resource group   
-   Speicherkonto  
-   Azure-Blobcontainer   
-   SAS-Richtlinie    

Dieses Skript beginnt mit der Deklarierung einer Reihe von Variablen, um die Namen für die oben genannten Ressourcen und die Namen der folgenden erforderlichen Eingabewerte anzugeben:  
  
-   Ein Präfixname, der zur Benennung von anderen Ressourcenobjekten verwendet wird    
-   Abonnementname    
-   Standort des Rechenzentrums  

  
Das Skript wird durch die Generierung der erforderlichen CREATE CREDENTIAL-Anweisung abgeschlossen, die Sie in [2: Erstellen von SQL Server-Anmeldeinformationen mit einer Shared Access Signature (SAS)](#2---create-a-sql-server-credential-using-a-shared-access-signature)verwenden werden. Diese Anweisung wird für Sie in die Zwischenablage kopiert und wird an der Konsole ausgegeben, damit Sie sie anzeigen können.  
  
Befolgen Sie folgende Schritte, um eine Richtlinie für einen Container zu erstellen und einen SAS-Schlüssel zu erstellen:  
  
1.  Öffnen Sie Windows PowerShell oder Windows PowerShell ISE (siehe oben genannte Versionsanforderungen).  
  
2.  Bearbeiten Sie das nachstehende Skript, und führen Sie es anschließend aus:  
  
    ```powershell
    # Define global variables for the script  
    $prefixName = '<a prefix name>'  # used as the prefix for the name for various objects  
    $subscriptionID = '<your subscription ID>'   # the ID  of subscription name you will use  
    $locationName = '<a data center location>'  # the data center region you will use  
    $storageAccountName= $prefixName + 'storage' # the storage account name you will create or use  
    $containerName= $prefixName + 'container'  # the storage container name to which you will attach the SAS policy with its SAS token  
    $policyName = $prefixName + 'policy' # the name of the SAS policy 

    # Set a variable for the name of the resource group you will create or use  
    $resourceGroupName=$prefixName + 'rg'   
  
    # Add an authenticated Azure account for use in the session   
    Connect-AzAccount    
  
    # Set the tenant, subscription and environment for use in the rest of   
    Set-AzContext -SubscriptionId $subscriptionID   
  
    # Create a new resource group - comment out this line to use an existing resource group  
    New-AzResourceGroup -Name $resourceGroupName -Location $locationName   
  
    # Create a new Azure Resource Manager storage account - comment out this line to use an existing Azure Resource Manager storage account  
    New-AzStorageAccount -Name $storageAccountName -ResourceGroupName $resourceGroupName -Type Standard_RAGRS -Location $locationName   
  
    # Get the access keys for the Azure Resource Manager storage account  
    $accountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName  
  
    # Create a new storage account context using an Azure Resource Manager storage account  
    $storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $accountKeys[0].Value

    # Creates a new container in blob storage  
    $container = New-AzStorageContainer -Context $storageContext -Name $containerName  
  
    # Sets up a Stored Access Policy and a Shared Access Signature for the new container  
    $policy = New-AzStorageContainerStoredAccessPolicy -Container $containerName -Policy $policyName -Context $storageContext -StartTime $(Get-Date).ToUniversalTime().AddMinutes(-5) -ExpiryTime $(Get-Date).ToUniversalTime().AddYears(10) -Permission rwld

    # Gets the Shared Access Signature for the policy  
    $sas = New-AzStorageContainerSASToken -name $containerName -Policy $policyName -Context $storageContext
    Write-Host 'Shared Access Signature= '$($sas.Substring(1))''  

    # Sets the variables for the new container you just created
    $container = Get-AzStorageContainer -Context $storageContext -Name $containerName
    $cbc = $container.CloudBlobContainer 
  
    # Outputs the Transact SQL to the clipboard and to the screen to create the credential using the Shared Access Signature  
    Write-Host 'Credential T-SQL'  
    $tSql = "CREATE CREDENTIAL [{0}] WITH IDENTITY='Shared Access Signature', SECRET='{1}'" -f $cbc.Uri,$sas.Substring(1)   
    $tSql | clip  
    Write-Host $tSql 

    # Once you're done with the tutorial, remove the resource group to clean up the resources. 
    # Remove-AzResourceGroup -Name $resourceGroupName  
    ```  

3.  Nach Abschluss des Skripts wir die CREATE CREDENTIAL-Anweisung in Ihrer Zwischenablage zur Verwendung im nächsten Abschnitt abgelegt.  


## <a name="2---create-a-sql-server-credential-using-a-shared-access-signature"></a>2: Erstellen von SQL Server-Anmeldeinformationen mit einer Shared Access Signature (SAS)

In diesem Abschnitt erstellen Sie Anmeldeinformationen zum Speichern der Sicherheitsinformationen, die von SQL Server verwendet werden, um in einen Azure-Container zu schreiben und aus diesem zu lesen, den Sie im vorherigen Abschnitt erstellt haben.  
  
SQL Server-Anmeldeinformationen sind ein Objekt zum Speichern von Authentifizierungsinformationen, die für die Verbindung mit einer Ressource außerhalb von SQL Server erforderlich sind. In den Anmeldeinformationen werden der URI-Pfad des Speichercontainers und die Shared Access Signature für diesen Container gespeichert.  

Führen Sie die folgenden Schritte aus, um SQL Server-Anmeldeinformationen zu erstellen:  
  
1.  Stellen Sie eine Verbindung mit SQL Server Management Studio her.  
2.  Öffnen Sie ein neues Abfragefenster und stellen Sie eine Verbindung mit der SQL Server-Instanz der Datenbank-Engine in Ihrer lokalen Umgebung.  
  
3.  Fügen Sie in das neue Abfragefenster die CREATE CREDENTIAL-Anweisung mit der SAS aus Abschnitt 1 ein, und führen Sie dieses Skript aus.  
  
    Das Skript wird wie der folgende Code aussehen.  
  
    ```sql   
    /* Example:
    USE master  
    CREATE CREDENTIAL [https://msfttutorial.blob.core.windows.net/containername] 
    WITH IDENTITY='SHARED ACCESS SIGNATURE'   
    , SECRET = 'sharedaccesssignature' 
    GO */
    
    USE master  
    CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>] 
      -- this name must match the container path, start with https and must not contain a forward slash at the end
    WITH IDENTITY='SHARED ACCESS SIGNATURE' 
      -- this is a mandatory string and should not be changed   
     , SECRET = 'sharedaccesssignature' 
       -- this is the shared access signature key that you obtained in section 1.   
    GO    
    ```  
  
4.  Um alle verfügbaren Anmeldeinformationen anzuzeigen, können Sie die folgende Anweisung in einem Abfragefenster ausführen, das mit Ihrer Instanz verbunden ist:  
  
    ```sql  
    SELECT * from sys.credentials  
    ```  
  
5.  Öffnen Sie ein neues Abfragefenster und stellen Sie eine Verbindung mit der SQL Server-Instanz der Datenbank-Engine auf Ihrem virtuellen Azure-Computer her.  
  
6.  Fügen Sie in das neue Abfragefenster die CREATE CREDENTIAL-Anweisung mit der SAS aus Abschnitt 1 ein, und führen Sie dieses Skript aus.  
  
7.  Wiederholen Sie die Schritte 5 und 6 für alle zusätzlichen SQL Server-Instanzen, die auf den Azure-Container zugreifen sollen.  

## <a name="3---database-backup-to-url"></a>3: Datenbanksicherung über URLs

In diesem Abschnitt werden Sie die AdventureWorks2016-Datenbank in Ihrer lokalen SQL Server 2016-Instanz in dem Azure-Container sichern, den Sie in [Abschnitt 1](#1---create-stored-access-policy-and-shared-access-storage) erstellt haben.
  
> [!NOTE]  
> Wenn Sie eine SQL Server 2012 SP1 CU2 oder höher Datenbank oder eine SQL Server 2014-Datenbank in diesem Azure-Container sichern möchten, können Sie die [hier](/previous-versions/sql/2014/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2014&preserve-view=true) dokumentierte veraltete Syntax verwenden, um die URL mit der WITH CREDENTIAL-Syntax zu sichern.  
  
So sichern Sie eine Datenbank in Blob Storage:  
  
1.  Stellen Sie eine Verbindung mit SQL Server Management Studio her.  
2.  Öffnen Sie ein neues Abfragefenster und stellen Sie eine Verbindung mit der SQL Server 2016-Instanz der Datenbank-Engine auf Ihrem virtuellen Azure-Computer her.  
3.  Kopieren Sie das folgende Transact-SQL-Skript in das Abfragefenster. Ändern Sie die URL gemäß Ihres Speicherkontonamens und des Containers, den Sie in Abschnitt 1 angegeben haben, und führen Sie dieses Skript anschließend aus.  
  
    ```sql  
  
    -- To permit log backups, before the full database backup, modify the database to use the full recovery model.  
    USE master;  
    ALTER DATABASE AdventureWorks2016  
       SET RECOVERY FULL;  
  
    -- Back up the full AdventureWorks2016 database to the container that you created in section 1  
    BACKUP DATABASE AdventureWorks2016   
       TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_onprem.bak'  
  
    ```  
  
4.  Öffnen Sie den Objekt-Explorer öffnen und stellen Sie eine Verbindung mit dem Azure-Speicher mit Ihrem Speicherkonto und Schlüssel her. 
    1.  Erweitern Sie die Container, erweitern Sie den Container, den Sie in Abschnitt 1 erstellt haben und überprüfen Sie, ob die Sicherung aus Schritt 3 oben in diesem Container angezeigt wird.  
  
   ![Verbinden mit dem Azure-Speicherkonto](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/connect-to-azure-storage.png)


## <a name="4----restore-database-to-virtual-machine-from-url"></a>4: Wiederherstellen einer Datenbank auf einem virtuellen Computer über URLs

In diesem Abschnitt werden Sie die AdventureWorks2016-Datenbank für Ihre SQL Server 2016-Instanz auf Ihrem virtuellen Azure-Computer wiederherstellen.
  
> [!NOTE]  
> Der Einfachheit halber verwenden wir in diesem Tutorial den gleichen Container für die Daten und Protokolldateien, die wir für die Datenbanksicherung verwenden. In einer Produktionsumgebung würden Sie wahrscheinlich mehrere Container und häufig auch mehrere Datendateien verwenden. Mit SQL Server 2016 könnten Sie auch ein Striping der Sicherung über mehrere Blobs erwägen, um die Backup-Leistung zu steigern, wenn Sie eine große Datenbank sichern.  
  
Zum Wiederherstellen der AdventureWorks2016-Datenbank von Azure Blob Storage auf Ihre SQL Server 2016-Instanz auf dem virtuellen Computer in Microsoft Azure gehen Sie folgendermaßen vor:  
  
1.  Stellen Sie eine Verbindung mit SQL Server Management Studio her.   
2.  Öffnen Sie ein neues Abfragefenster und stellen Sie eine Verbindung mit der SQL Server 2016-Instanz der Datenbank-Engine auf Ihrem virtuellen Azure-Computer her.   
3.  Kopieren Sie das folgende Transact-SQL-Skript in das Abfragefenster. Ändern Sie die URL gemäß Ihres Speicherkontonamens und des Containers, den Sie in Abschnitt 1 angegeben haben, und führen Sie dieses Skript anschließend aus.  
  
    ```sql  
    -- Restore AdventureWorks2016 from URL to SQL Server instance using Azure blob storage for database files  
    RESTORE DATABASE AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_onprem.bak'   
       WITH  
          MOVE 'AdventureWorks2016_data' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Data.mdf'  
         ,MOVE 'AdventureWorks2016_log' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Log.ldf'  
    --, REPLACE  
  
    ```  
  
4.  Öffnen Sie den Objekt-Explorer und erstellen Sie eine Verbindung mit Ihrer Azure SQL Server 2016-Instanz.   
5.  Erweitern Sie im Objekt-Explorer den Knoten Datenbanken und überprüfen Sie, ob die AdventureWorks2016-Datenbank wiederhergestellt wurde (Aktualisieren Sie den Knoten nach Bedarf).    
    1. Klicken Sie mit der rechten Maustaste auf „AdventureWorks2016“, und wählen Sie „Eigenschaften“ aus.
    1. Wählen Sie „Dateien“ aus, und überprüfen Sie, ob die Pfade für die zwei Datenbankdateien URLs sind, die auf Blobs in Ihrem Azure-Blog-Container verweisen (wenn Sie fertig sind, wählen Sie „Abbrechen“ aus).
    
     ![AdventureWorks DB auf Azure-VM](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/adventureworks-on-azure-vm.png)

    
8.  Stellen Sie im Objekt-Explorer eine Verbindung mit dem Azure-Speicher her.
    1. Container erweitern – erweitern Sie den Container, den Sie in Abschnitt 1 erstellt haben, und überprüfen Sie, ob „AdventureWorks2016_Data.mdf“ und „AdventureWorks2016_Log.ldf“ aus Schritt 3 oben in diesem Container erscheinen, zusammen mit der Backup-Datei aus Abschnitt 3 (aktualisieren Sie den Knoten nach Bedarf).  
  
   ![Datendateien im Container in Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/data-files-in-container.png)

## <a name="5---backup-database-using-file-snapshot-backup"></a>5: Backup einer Datenbank mit Dateimomentaufnahme-Sicherung

In diesem Abschnitt werden Sie in Ihrem virtuellen Azure-Computer Ihre AdventureWorks2016-Datenbank per Dateimomentaufnahme-Sicherung zum Ausführen einer nahezu sofortigen Sicherung mithilfe von Azure-Momentaufnahmen sichern. Weitere Informationen zu Momentaufnahme-Sicherungen finden Sie unter [Dateimomentaufnahme-Sicherungen für Datenbankdateien in Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)  
  
So sichern Sie die AdventureWorks2016-Datenbank mithilfe der Dateimomentaufnahme-Sicherung:  
  
1.  Stellen Sie eine Verbindung mit SQL Server Management Studio her.   
2.  Öffnen Sie ein neues Abfragefenster und stellen Sie eine Verbindung mit der SQL Server 2016-Instanz der Datenbank-Engine auf Ihrem virtuellen Azure-Computer her.  
3.  Kopieren Sie das folgende Transact-SQL-Skript in das Abfragefenster und führen Sie es aus (schließen Sie dieses Abfragefenster nicht – Sie werden dieses Skript in Schritt 5 erneut ausführen). Diese vom System gespeicherte Prozedur ermöglicht Ihnen, die vorhandenen Dateimomentaufnahme-Sicherungen für jede Datei anzuzeigen, die eine angegebene Datenbank enthält. Sie werden feststellen, dass es keine Dateimomentaufnahme-Sicherungen für diese Datenbank gibt.  
  
    ```sql  
    -- Verify that no file snapshot backups exist  
    SELECT * FROM sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
    ```  
  
4.  Kopieren Sie das folgende Transact-SQL-Skript in das Abfragefenster. Ändern Sie die URL gemäß Ihres Speicherkontonamens und des Containers, den Sie in Abschnitt 1 angegeben haben, und führen Sie dieses Skript anschließend aus. Beachten Sie, wie schnell diese Sicherung ausgeführt wird.  
  
    ```sql  
    -- Backup the AdventureWorks2016 database with FILE_SNAPSHOT  
    BACKUP DATABASE AdventureWorks2016   
       TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2016_Azure.bak'   
       WITH FILE_SNAPSHOT;    
    ```  
  
5.  Nachdem Sie überprüft haben, dass das Skript in Schritt 4 erfolgreich ausgeführt wurde, führen Sie das folgende Skript erneut aus. Beachten Sie, dass die Dateimomentaufnahme-Sicherung in Schritt 4 Dateimomentaufnahmen der Daten und der Protokolldatei generiert hat.  
  
    ```sql  
    -- Verify that two file-snapshot backups exist  
    SELECT * FROM sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
  
    ```  
  
    ![Ergebnisse von fn_db_backup_file_snapshots mit Anzeige von Momentaufnahmen](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/results-showing-snapshot.png) 
  
6.  Im Objekt-Explorer in Ihrer SQL Server 2016-Instanz in Ihrem virtuellen Azure-Computer erweitern Sie den Knoten Datenbanken, und überprüfen Sie, ob die AdventureWorks2016-Datenbank in dieser Instanz wiederhergestellt wurde (aktualisieren Sie den Knoten nach Bedarf).  
7.  Stellen Sie im Objekt-Explorer eine Verbindung mit dem Azure-Speicher her.   
8.  Container erweitern – erweitern Sie den Container, den Sie in Abschnitt 1 erstellt haben, und überprüfen Sie, ob die AdventureWorks2016_Azure.bak aus Schritt 4 oben in diesem Container erscheint, zusammen mit der Backup-Datei aus Abschnitt 3 und den Datenbank-Dateien aus Abschnitt 4 (aktualisieren Sie den Knoten nach Bedarf).  
  
    ![Momentaufnahme-Sicherung in Azure](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/snapshot-backup-on-azure.PNG)

## <a name="6----generate-activity-and-backup-log-using-file-snapshot-backup"></a>6: Generieren von Aktivität und Erstellen einer Sicherung eines Protokolls mithilfe der Dateimomentaufnahme-Sicherung

In diesem Abschnitt generieren Sie Aktivität in der AdventureWorks2016-Datenbank und erstellen regelmäßig Transaktionsprotokollsicherungen mithilfe von Dateimomentaufnahme-Sicherungen. Weitere Informationen zur Verwendung von Momentaufnahme-Sicherungen finden Sie unter [Dateimomentaufnahme-Sicherungen für Datenbankdateien in Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
Um Aktivität in der AdventureWorks2016-Datenbank zu generieren und regelmäßig Transaktionsprotokollsicherungen mithilfe von Dateimomentaufnahme-Sicherungen zu erstellen, befolgen Sie folgende Schritte:  
  
1.  Stellen Sie eine Verbindung mit SQL Server Management Studio her.   
2.  Öffnen Sie zwei neue Abfragefenster und stellen Sie von jedem Abfragefenster aus eine Verbindung mit der SQL Server 2016-Instanz der Datenbank-Engine auf Ihrem virtuellen Azure-Computer her.   
3.  Kopieren Sie das folgende Transact-SQL-Skript, fügen Sie es in ein Abfragefenster ein und führen Sie es aus. Beachten Sie, dass die Production.Location-Tabelle über 14 Zeilen verfügt, bevor wir neue Zeilen in Schritt 4 hinzufügen.  
  
    ```sql 
    -- Verify row count at start  
    SELECT COUNT (*) from AdventureWorks2016.Production.Location;    
    ```  
  
4.  Kopieren Sie die folgenden zwei Transact-SQL-Skripts, und geben Sie sie in die beiden separaten Abfragefenster ein. Ändern Sie die URL gemäß Ihres Speicherkontonamens und des Containers, den Sie in Abschnitt 1 angegeben haben, und führen Sie diese Skript anschließend gleichzeitig in separaten Abfragefenstern aus. Die Ausführung dieser Skripts nimmt etwa sieben Minuten in Anspruch.  
  
    ```sql  
    -- Insert 30,000 new rows into the Production.Location table in the AdventureWorks2014 database in batches of 75  
    DECLARE @count INT=1, @inner INT;  
    WHILE @count < 400  
       BEGIN  
          BEGIN TRAN;  
             SET @inner =1;  
                WHILE @inner <= 75  
                   BEGIN;  
                      INSERT INTO AdventureWorks2016.Production.Location    
                         (Name, CostRate, Availability, ModifiedDate)   
                            VALUES (NEWID(), .5, 5.2, GETDATE());  
                      SET @inner = @inner + 1;  
                   END;  
          COMMIT;  
       WAITFOR DELAY '00:00:01';   
       SET @count = @count + 1;  
       END;  
    SELECT COUNT (*) from AdventureWorks2014.Production.Location;    
    ```  
  
    ```sql  
    --take 7 transaction log backups with FILE_SNAPSHOT, one per minute, and include the row count and the execution time in the backup file name   
    DECLARE @count INT=1, @device NVARCHAR(120), @numrows INT;  
    WHILE @count <= 7  
       BEGIN  
             SET @numrows = (SELECT COUNT (*) FROM AdventureWorks2016.Production.Location);  
             SET @device = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/tutorial-' + CONVERT (varchar(10),@numrows) + '-' + FORMAT(GETDATE(), 'yyyyMMddHHmmss') + '.bak';  
             BACKUP LOG AdventureWorks2016 TO URL = @device WITH FILE_SNAPSHOT;  
             SELECT * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016');  
          WAITFOR DELAY '00:1:00';   
             SET @count = @count + 1;  
       END;  
    ```  
  
5.  Überprüfen Sie die Ausgabe des ersten Skripts, und beachten Sie, dass die endgültige Zeilenanzahl jetzt 29.939 beträgt.  
  
    ![Die Zeilenanzahl 29.939 wird angezeigt.](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/29-thousand-rows.png)
  
6.  Überprüfen Sie die Ausgabe des zweiten Skripts und beachten Sie, dass jedes Mal, wenn die BACKUP LOG-Anweisung ausgeführt wird, zwei neue Dateimomentaufnahmen erstellt werden: eine Dateimomentaufnahme der Protokolldatei und eine Dateimomentaufnahme der Datendatei – insgesamt also zwei Dateimomentaufnahmen für jede Datenbankdatei. Wenn das zweite Skript abgeschlossen ist, beachten Sie, dass es jetzt insgesamt 16 Dateimomentaufnahmen gibt, also 8 für jede Datenbankdatei: eine von der BACKUP DATABASE-Anweisung und eine für jede Ausführung der BACKUP LOG-Anweisung.  
  
   ![Ergebnisse der Sicherungsmomentaufnahme](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/snapshot-back-results.png)
  
7.  Stellen Sie im Objekt-Explorer eine Verbindung mit dem Azure-Speicher her.  
  
8.  Erweitern Sie „Container“, erweitern Sie den Container, den Sie in Abschnitt 1 erstellt haben, und überprüfen Sie, ob sieben neue Sicherungsdateien zusammen mit den Datendateien des vorangegangenen Abschnitts erscheinen (Aktualisieren Sie, falls notwendig).  
  
    ![Mehrere Momentaufnahmen in Azure-Container](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/tutorial-snapshots-in-container.png)

## <a name="7---restore-a-database-to-a-point-in-time"></a>7: Wiederherstellen einer Datenbank bis zu einem Zeitpunkt

In diesem Abschnitt stellen Sie eine AdventureWorks2016-Datenbank zu einem Zeitpunkt zwischen zwei Transaktionsprotokollsicherungen wieder her.  
  
Um bei herkömmlichen Sicherungen einen bestimmten Zeitpunkt wiederherzustellen zu können, müssen Sie über eine vollständige Sicherung der Datenbank, z.B. eine differenzielle Sicherung, sowie über alle Transaktionsprotokolldateien bis zu diesem und direkt nach diesem Zeitpunkt verfügen, den Sie wiederherstellen möchten. Mit Dateimomentaufnahme-Sicherungen benötigen Sie nur die zwei angrenzenden Protokollsicherungsdateien als Zeitrahmen, den Sie wiederherstellen möchten. Sie benötigen nur zwei Momentaufnahmesicherungssätze, weil jede Momentaufnahmesicherung einer Protokolldatei eine Dateimomentaufnahme jeder Datenbankdatei (jede Datendatei und die Protokolldatei) erstellt.  
  
Zum Wiederherstellen einer Datenbank bis zu einem bestimmten Zeitpunkt per Dateimomentaufnahmesicherungssätzen gehen Sie folgendermaßen vor:  
  
1.  Stellen Sie eine Verbindung mit SQL Server Management Studio her.  
2.  Öffnen Sie ein neues Abfragefenster und stellen Sie eine Verbindung mit der SQL Server 2016-Instanz der Datenbank-Engine auf Ihrem virtuellen Azure-Computer her.   
3.  Kopieren Sie das folgende Transact-SQL-Skript, fügen Sie es in das Abfragefenster ein und führen Sie es aus. Überprüfen Sie, ob die Production.Location-Tabelle 29.939 Zeilen hat, bevor wir sie zu einem bestimmten Zeitpunkt wiederherstellen, wenn in Schritt 5 weniger Zeilen vorhanden sind.  
  
    ```sql
    -- Verify row count at start  
    SELECT COUNT (*) from AdventureWorks2016.Production.Location   
    ```  
  
    ![Die Zeilenanzahl 29.939 wird angezeigt.](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/29-thousand-rows.png) 
  
4.  Kopieren Sie das folgende Transact-SQL-Skript in das Abfragefenster. Wählen Sie zwei aufeinander folgende Backup-Log-Dateien und konvertieren Sie den Dateinamen in das Datum und die Zeit, die Sie für dieses Skript benötigen. Ändern Sie die URL gemäß Ihres Speicherkontonamens und des Containers, den Sie in Abschnitt 1 angegeben haben, geben Sie die Namen der ersten und zweiten Protokollsicherungsdatei an, geben Sie die STOPAT-Zeit im Format „June 26, 2018 01:48 PM“ an und führen Sie dieses Skript anschließend aus. Dieses Skript wird einige Minuten in Anspruch nehmen.  
  
    ```sql  
    -- restore and recover to a point in time between the times of two transaction log backups, and then verify the row count  
    ALTER DATABASE AdventureWorks2016 SET SINGLE_USER WITH ROLLBACK IMMEDIATE;  
    RESTORE DATABASE AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<firstbackupfile>.bak'   
       WITH NORECOVERY,REPLACE;  
    RESTORE LOG AdventureWorks2016   
       FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<secondbackupfile>.bak'    
       WITH RECOVERY, STOPAT = 'June 26, 2018 01:48 PM';  
    ALTER DATABASE AdventureWorks2016 set multi_user;  
    -- get new count  
    SELECT COUNT (*) FROM AdventureWorks2016.Production.Location ;
    ```  
  
5.  Überprüfen Sie die Ausgabe. Beachten Sie, dass nach der Wiederherstellung die Anzahl der Zeilen 18.389 ist, also eine Zeilenzahl zwischen Sicherung 5 und 6 ist (die Zeilenanzahl variiert).  
  
    ![18-thousand-rows.JPG](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/18-thousand-rows.png)

## <a name="8----restore-as-new-database-from-log-backup"></a>8: Wiederherstellen als neue Datenbank aus der Protokollsicherung

In diesem Abschnitt stellen Sie eine AdventureWorks2016-Datenbank als eine neue Datenbank aus einer Sicherung einer Protokolldatei für Datei-Momentaufnahmen wieder her.  
  
In diesem Szenario führen Sie eine Wiederherstellung auf eine SQL Server-Instanz auf einem anderen virtuellen Computer für Geschäftsanalysen und Berichterstellung aus. Die Wiederherstellung auf eine andere Instanz auf einem anderen virtuellen Computer verlagert die Arbeitsauslastung auf einen virtuellen Computer, der für diesen Zweck dediziert und der Größe angepasst ist und entfernt dessen Ressourcenanforderungen vom Transaktionssystem.  
  
Die Wiederherstellung von einer Transaktionsprotokollsicherung mit Dateimomentaufnahme-Sicherung ist sehr schnell und ist erheblich schneller als die traditionelle Streamingsicherung. Mit dem herkömmlichen Streaming von Sicherungen müssten Sie die vollständige Datenbanksicherung, vielleicht eine differenzielle Sicherung und einige oder alle Transaktionsprotokollsicherungen (oder eine neue vollständige Datenbanksicherung) verwenden. Jedoch benötigen Sie bei Dateimomentaufnahme-Sicherungen nur die letzte Protokollsicherung (oder jede andere Protokollsicherung oder zwei angrenzende Protokollsicherungen für die Point-in-Time-Wiederherstellung auf einen Zeitpunkt zwischen zwei Zeitpunkten von Protokollsicherungen). Um es einfacher auszudrücken: Sie benötigen nur einen Momentaufnahmen-Sicherungssatz einer Protokolldatei, weil jede Momentaufnahmesicherung einer Protokolldatei eine Dateimomentaufnahme jeder Datenbankdatei (jede Datendatei und die Protokolldatei) erstellt.  
  
Befolgen Sie folgende Schritte, um eine Datenbank auf eine neue Datenbank einer Transaktionsprotokollsicherung mithilfe einer Dateimomentaufnahme-Sicherung wiederherzustellen:  
  
1.  Stellen Sie eine Verbindung mit SQL Server Management Studio her.   
2.  Öffnen Sie ein neues Abfragefenster, und stellen Sie eine Verbinden mit der SQL Server 2016-Instanz der Datenbank-Engine auf einem virtuellen Azure-Computer her.  
  
    > [!NOTE]  
    > Wenn es sich hierbei um einen anderen virtuellen Azure-Computer handelt, als denjenigen, den Sie für die vorherigen Abschnitte verwendet haben, stellen Sie sicher, dass Sie die Schritte in [2: Erstellen von SQL Server-Anmeldeinformationen mit einer Shared Access Signature (SAS)](#2---create-a-sql-server-credential-using-a-shared-access-signature)befolgt haben. Wenn Sie in einen anderen Container wiederherstellen möchten, führen Sie die Schritte in [1: Erstellen einer gespeicherten Zugriffsrichtlinie und von Speicher mit freigegebenem Zugriff](#1---create-stored-access-policy-and-shared-access-storage) für den neuen Container aus.  
  
3.  Kopieren Sie das folgende Transact-SQL-Skript in das Abfragefenster. Wählen Sie die Protokollsicherungsdatei aus, die Sie verwenden möchten. Ändern Sie die URL gemäß Ihres Speicherkontonamens und des Containers, den Sie in Abschnitt 1 angegeben haben, geben Sie den Namen der Protokollsicherungsdatei an, und führen Sie dieses Skript anschließend aus.  
  
    ```sql  
    -- restore as a new database from a transaction log backup file  
    RESTORE DATABASE AdventureWorks2016_EOM   
        FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/<logbackupfile.bak>'    
        WITH MOVE 'AdventureWorks2016_data' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_EOM_Data.mdf'  
       , MOVE 'AdventureWorks2016_log' to 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/AdventureWorks2014_EOM_Log.ldf'  
       , RECOVERY  
    --, REPLACE   
    ```  
  
4.  Überprüfen Sie die Ausgabe, um sicherzustellen, dass die Wiederherstellung erfolgreich war.   
5.  Stellen Sie im Objekt-Explorer eine Verbindung mit dem Azure-Speicher her.    
6.  Erweitern Sie „Container“, erweitern Sie den Container, den Sie in Abschnitt 1 erstellt haben (aktualisieren Sie, falls notwendig), und überprüfen Sie, ob die neuen Daten- und Protokolldateien zusammen mit den Blobs der vorangegangenen Abschnitte im Container erscheinen.  
  
    ![Azure-Container mit den Daten- und Protokolldateien für die neue Datenbank](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/new-db-in-azure-container.png)

## <a name="9---manage-backup-sets-and-file-snapshot-backups"></a>9: Verwalten von Sicherungssätzen und Dateimomentaufnahme-Sicherungen

In diesem Abschnitt löschen Sie einen Sicherungssatz mithilfe der gespeicherten Systemprozedur [sp_delete_backup &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md). Diese gespeicherte Systemprozedur löscht die Sicherungsdatei und die Dateimomentaufnahme in jeder Datenbankdatei, die diesem Sicherungssatz zugeordnet ist.  
  
> [!NOTE]  
> Wenn Sie versuchen, einen Sicherungssatz zu löschen, indem Sie einfach die Sicherungsdatei aus dem Azure-Blobcontainer löschen, löschen Sie nur die Sicherungsdatei selbst und die zugehörigen Dateimomentaufnahmen bleiben bestehen. Wenn Sie vor diesem Problem stehen, verwenden Sie die Systemfunktion [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md) , um die URL der verwaisten Dateimomentaufnahmen zu identifizieren und anschließend die gespeicherte Systemprozedur [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) , um jede verwaiste Dateimomentaufnahme zu löschen. Weitere Informationen finden Sie unter  [Dateimomentaufnahme-Sicherungen für Datenbankdateien in Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
Befolgen Sie folgende Schritte, um einen Dateimomentaufnahme-Sicherungssatz zu löschen:  
  
1.  Stellen Sie eine Verbindung mit SQL Server Management Studio her.  
2.  Öffnen Sie ein neues Abfragefenster, und stellen Sie eine Verbinden mit der SQL Server 2016-Instanz der Datenbank-Engine auf Ihrem virtuellen Azure-Computer her (oder mit einer beliebigen SQL Server 2016-Instanz mit Lese-/Schreibberechtigungen für diesen Container).   
3.  Kopieren Sie das folgende Transact-SQL-Skript in das Abfragefenster. Wählen Sie die Protokollsicherung aus, die Sie zusammen mit den zugehörigen Dateimomentaufnahmen löschen möchten. Ändern Sie die URL gemäß Ihres Speicherkontonamens und des Containers, den Sie in Abschnitt 1 angegeben haben, geben Sie den Namen der Protokollsicherungsdatei an, und führen Sie dieses Skript anschließend aus.  
  
  ```sql
  sys.sp_delete_backup 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>/tutorial-21764-20181003205236.bak';  
  ```  
  
4.  Stellen Sie im Objekt-Explorer eine Verbindung mit dem Azure-Speicher her.  
5.  Erweitern Sie die Container, erweitern Sie den Container, den Sie in Abschnitt 1 erstellt haben und stellen Sie sicher, dass die Sicherung aus Schritt 3 nicht länger im Container angezeigt wird (aktualisieren Sie den Knoten bei Bedarf).  
  
    ![Azure-Container zeigt das Löschen des Protokollsicherungsblobs](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/deleted-backup-snapshot.png)
  
6.  Kopieren Sie das folgende Transact-SQL-Skript, fügen Sie es in das Abfragefenster ein, und führen Sie es anschließend aus, um sicherzustellen, dass zwei Dateimomentaufnahmen gelöscht wurden.  
  
    ```sql  
    -- verify that two file snapshots have been removed  
    SELECT * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016');   
    ```  
    ![Bereich „Ergebnisse“ mit 2 gelöschten Dateimomentaufnahmen](media/tutorial-use-azure-blob-storage-service-with-sql-server-2016/results-of-two-deleted-snapshot-files.png)

## <a name="10---remove-resources"></a>10: Entfernen von Ressourcen

Sobald Sie dieses Tutorial beendet haben, und um Ressourcen zu sparen, stellen Sie sicher, dass Sie die in diesem Tutorial erstellte Ressourcengruppe löschen. 

Um die Ressourcengruppe zu löschen, führen Sie den folgenden PowerShell-Code aus:

  ```powershell
  # Define global variables for the script  
  $prefixName = '<prefix name>'  # should be the same as the beginning of the tutorial
  
  # Set a variable for the name of the resource group you will create or use  
  $resourceGroupName=$prefixName + 'rg'   
  
  # Adds an authenticated Azure account for use in the session   
  Connect-AzAccount    
  
  # Set the tenant, subscription and environment for use in the rest of   
  Set-AzContext -SubscriptionId $subscriptionID    
    
  # Remove the resource group
  Remove-AzResourceGroup -Name $resourceGroupName   
  ```


  
## <a name="see-also"></a>Weitere Informationen

[SQL Server-Datendateien in Microsoft Azure](../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md)  
[Dateimomentaufnahme-Sicherungen für Datenbankdateien in Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)  
[SQL Server-Sicherung in URL](../relational-databases/backup-restore/sql-server-backup-to-url.md) 
[Shared Access Signatures, Teil 1: Grundlagen zum SAS-Modell](/azure/storage/common/storage-sas-overview)  
[Erstellen eines Containers](/rest/api/storageservices/Create-Container)  
[Set Container ACL](/rest/api/storageservices/Set-Container-ACL)  
[Abrufen der Container-ACL](/rest/api/storageservices/Get-Container-ACL)
[Anmeldeinformationen &#40;Datenbank-Engine&#41;](../relational-databases/security/authentication-access/credentials-database-engine.md)  
[CREATE CREDENTIAL &#40;Transact-SQL&#41;](../t-sql/statements/create-credential-transact-sql.md)  
[sys.credentials &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)  
[sp_delete_backup &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md)  
[sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md)  
[sp_delete_backup_file_snapshot (Transact-SQL)](../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) [Dateimomentaufnahme-Sicherungen für Datenbankdateien in Azure](../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md)