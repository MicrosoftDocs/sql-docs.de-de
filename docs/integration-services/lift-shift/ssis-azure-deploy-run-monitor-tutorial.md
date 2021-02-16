---
title: Bereitstellen und Ausführen von SSIS-Paketen in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein SSIS-Projekt (SQL Server Integration Services) für den SSIS-Katalog auf einer Azure SQL-Datenbank bereitstellen und ein Paket ausführen.
ms.date: 05/22/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: f603b138773ea76fe73ea3d6929ece535779e23c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351991"
---
# <a name="tutorial-deploy-and-run-a-sql-server-integration-services-ssis-package-in-azure"></a>Tutorial: Bereitstellen und Ausführen eines SSIS-Pakets in Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


In diesem Tutorial wird gezeigt, wie ein SSIS-Projekt für den SSIS-Katalog in einer Azure SQL-Datenbank bereitgestellt wird, ein Paket in Azure SSIS Integration Runtime ausgeführt wird und das ausgeführte Paket überwacht wird.

## <a name="prerequisites"></a>Voraussetzungen

Prüfen Sie, ob Sie über die Version 17.2 oder höher von SQL Server Management Studio verfügen, bevor Sie beginnen. Die neueste Version von SSMS können Sie unter [Herunterladen von SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) herunterladen.

Vergewissern Sie sich auch, dass Sie die SSIS-Datenbank in Azure eingerichtet und die Azure SSIS Integration Runtime bereitgestellt haben. Informationen dazu, wie SSIS in Azure bereitgestellt wird, finden Sie unter [Bereitstellen von SQL Server Integration Services-Paketen in Azure](/azure/data-factory/tutorial-deploy-ssis-packages-azure).

## <a name="for-azure-sql-database-get-the-connection-info"></a>Abrufen der Verbindungsinformationen für Azure SQL-Datenbank

Um das Paket auf Azure SQL-Datenbank auszuführen, rufen Sie die Verbindungsinformationen ab, die für eine Verbindungsherstellung mit der SSIS-Katalogdatenbank (SSISDB) benötigt werden. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen für die folgenden Prozeduren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie aus dem Menü auf der linken Seite **SQL-Datenbanken** aus, und klicken Sie auf der Seite **SQL-Datenbanken** auf die SSISDB-Datenbank. 
3. Überprüfen Sie auf der **Übersichtsseite** Ihrer Datenbank den vollqualifizierten Servernamen. Zeigen Sie auf den Servernamen, damit die Option **Klicken Sie zum Kopieren** angezeigt wird. 
4. Wenn Sie die Anmeldeinformationen für Ihren Azure SQL-Datenbank-Server vergessen, navigieren Sie zur Seite „SQL Datenbank-Server“, damit der Serveradministratorname angezeigt wird. Sie können das Kennwort falls erforderlich zurücksetzen.

## <a name="connect-to-the-ssisdb-database"></a>Herstellen einer Verbindung mit SSISDB

Verwenden Sie SQL Server Management Studio, um eine Verbindung mit dem SSIS-Katalog auf Ihrem Azure SQL-Datenbank-Server herzustellen. Weitere Informationen und Screenshots finden Sie unter [Herstellen einer Verbindung mit der SSISDB-Katalogdatenbank in Azure](ssis-azure-connect-to-catalog-database.md).

Beachten Sie diese beiden wichtigen Punkte. Diese Schritte werden in der folgenden Prozedur beschrieben.
-   Geben Sie den vollqualifizierten Namen des Azure SQL-Datenbank-Servers im Format **mysqldbserver.database.windows.net** ein.
-   Wählen Sie `SSISDB` als Datenbank für die Verbindung.

> [!IMPORTANT]
> Ein Azure SQL-Datenbank-Server überwacht Port 1433. Wenn Sie versuchen, eine Verbindung zu einem Azure SQL-Datenbank-Server innerhalb einer Unternehmensfirewall herzustellen, muss dieser Port in der Unternehmensfirewall geöffnet sein, damit Sie eine Verbindung herstellen können.

1. Öffnen Sie SQL Server Management Studio.

2. **Stellen Sie eine Verbindung mit dem Server her**. Geben Sie im Dialogfeld **Mit Server verbinden** die folgenden Informationen ein:

   | Einstellung       | Vorgeschlagener Wert | BESCHREIBUNG | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servertyp** | Datenbank-Engine | Dieser Wert ist erforderlich. |
   | **Servername** | Der vollqualifizierte Servername | Der Name muss das folgende Format aufweisen: **mysqldbserver.database.windows.net**. Falls Sie den Servernamen benötigen, finden Sie ihn mithilfe der Anweisungen unter [Connect to the SSISDB Catalog database on Azure (Herstellen einer Verbindung mit der SSIS-Katalogdatenbank in Azure)](ssis-azure-connect-to-catalog-database.md). |
   | **Authentifizierung** | SQL Server-Authentifizierung | Sie können über die Windows-Authentifizierung keine Verbindung zu Azure SQL-Datenbank herstellen. |
   | **Anmeldung** | Das Serveradministratorkonto | Hierbei handelt es sich um das Konto, das Sie bei der Servererstellung angegeben haben. |
   | **Kennwort** | Das Kennwort für das Serveradministratorkonto | Das Kennwort, das Sie beim Erstellen des Servers angegeben haben |

3. **Stellen Sie eine Verbindung mit der SSIS-Datenbank her**. Wählen Sie **Optionen**  aus, um das Dialogfeld **Mit Server verbinden** zu erweitern. Wählen Sie im erweiterten Dialogfeld **Mit Server verbinden** die Registerkarte **Verbindungseigenschaften** aus. Wählen Sie `SSISDB` im Feld **Mit Datenbank verbinden** aus, oder geben Sie es ein.

4. Wählen Sie dann **Verbinden** aus. Das Fenster „Objekt-Explorer“ wird in SSMS geöffnet. 

5. Erweitern Sie im Objekt-Explorer **Integration Services-Kataloge** und dann **SSISDB**, um die Objekte in der SSIS-Katalogdatenbank anzuzeigen.

## <a name="deploy-a-project-with-the-deployment-wizard"></a>Bereitstellen eines Projekts mit dem Bereitstellungs-Assistenten

Weitere Informationen zum Bereitstellen von Paketen und zum Bereitstellungs-Assistenten finden Sie unter [Bereitstellen von SQL Server Integration Services-Projekten und Paketen (SSIS)](../packages/deploy-integration-services-ssis-projects-and-packages.md) und [Bereitstellungs-Assistent für Integration Services](../packages/deploy-integration-services-ssis-projects-and-packages.md#integration-services-deployment-wizard).

> [!NOTE]
> Für die Bereitstellung in Azure wird nur das Projektbereitstellungsmodell unterstützt.

### <a name="start-the-integration-services-deployment-wizard"></a>Starten des Bereitstellungs-Assistenten für Integration Services
1. Erweitern Sie im Objekt-Explorer in SSMS einen Projektorder mit den Knoten **Integration Services-Kataloge** und dem erweiterten **SSISDB**-Knoten.

2.  Wählen Sie den Knoten **Projekte** aus.

3.  Klicken Sie mit der rechten Maustaste auf den Knoten **Projekte**, und wählen Sie **Projekt bereitstellen** aus. Der Bereitstellungs-Assistent für Integration Services wird geöffnet. Sie können ein Projekt aus der SSIS-Katalogdatenbank oder dem Dateisystem bereitstellen.

    ![Bereitstellen eines Projekts aus SSMS](media/ssis-azure-deploy-run-monitor-tutorial/ssisdb-deploy-project1.png)

    ![Das Dialogfeld des SSIS-Bereitstellungs-Assistenten wird geöffnet](media/ssis-azure-deploy-run-monitor-tutorial/ssisdb-deploy-project2.png)

### <a name="deploy-a-project-with-the-deployment-wizard"></a>Bereitstellen eines Projekts mit dem Bereitstellungs-Assistenten
1. Lesen Sie auf der Seite **Einführung** des Bereitstellungs-Assistenten die Einführung. Klicken Sie auf **Weiter**, um zur Seite **Quelle auswählen** zu wechseln.

2. Wählen Sie auf der Seite **Quelle auswählen** das vorhandene SSIS-Projekt aus, das bereitgestellt werden soll.
    -   Um eine von Ihnen erstellte Projektbereitstellungsdatei bereitzustellen, wählen Sie **Projektbereitstellungsdatei** aus, und geben Sie den Pfad für die ISPAC-Datei ein.
    -   Um ein Projekt bereitzustellen, das sich in einem SSIS-Katalog befindet, wählen Sie **Integration Services-Katalog** aus und geben dann den Servernamen und den Pfad zum Projekt im Katalog ein. In diesem Schritt können nur Projekte erneut bereitgestellt werden, die sich in der von SQL Server gehosteten SSISDB befinden.
    -   Klicken Sie auf **Weiter**, um die Seite **Ziel auswählen** zu sehen.
  
3.  Wählen Sie auf der Seite **Ziel auswählen** das Ziel für das Projekt aus.
    -   Geben Sie den vollqualifizierten Servernamen im Format `<server_name>.database.windows.net` ein.
    -   Stellen Sie die Authentifizierungsinformationen bereit, und klicken Sie dann auf **Verbinden**.
    -   Klicken Sie dann auf **Durchsuchen**, um den Zielordner in SSISDB auszuwählen.
    -   Klicken Sie dann auf **Weiter**, um die Seite **Überprüfen** zu öffnen. (Die Schaltfläche **Weiter** ist nur nach der Auswahl von **Verbinden** aktiviert.)
  
4.  Überprüfen Sie auf der Seite **Überprüfen** die Einstellungen, die Sie ausgewählt haben.
    -   Sie können Ihre Auswahl ändern, indem Sie auf **Vorherige** klicken, oder indem Sie auf einen der Schritte im linken Bereich klicken.
    -   Klicken Sie auf **Bereitstellen**, um den Bereitstellungsprozess zu starten.

    > [!NOTE]
    > Wenn Sie die Fehlermeldung **Es liegt kein aktiver Worker-Agent vor (.Net SqlClient-Datenanbieter)** erhalten, stellen Sie sicher, dass die Azure-SSIS Integration Runtime ausgeführt wird. Dieser Fehler tritt auf, wenn Sie versuchen, eine Bereitstellung durchzuführen, während die Azure-SSIS IR beendet ist.

5.  Nachdem der Bereitstellungsvorgang abgeschlossen ist, wird die Seite **Ergebnisse** geöffnet. Diese Seite zeigt an, ob die einzelnen Aktionen erfolgreich ausgeführt wurden oder ob Fehler aufgetreten sind.
    -   Ist die Aktion fehlerhaft, klicken Sie in der Spalte **Ergebnis** auf **Fehlgeschlagen**, um eine Erklärung über den Fehler anzuzeigen.
    -   Sie können auf **Bericht speichern** klicken, um die Ergebnisse in einer XML-Datei zu speichern.
    -   Klicken Sie auf **Schließen**, um den Assistenten zu beenden.

## <a name="deploy-a-project-with-powershell"></a>Bereitstellen eines Projekts mit PowerShell

Um ein Projekt mit PowerShell für SSISDB in Azure SQL-Datenbank bereitzustellen, passen Sie das folgende Skript an Ihre Anforderungen an. Das Skript zählt die untergeordneten Ordner unter `$ProjectFilePath` sowie die Projekte in jedem untergeordneten Ordner auf, erstellt daraufhin die gleichen Ordner in SSISDB und stellt die Projekte in diesen Ordnern bereit.

Dieses Skript erfordert, dass SQL Server Data Tools Version 17.x oder SQL Server Management Studio auf dem Computer installiert ist, auf dem das Skript ausgeführt wird.

```powershell
# Variables
$ProjectFilePath = "C:\<folder>"
$SSISDBServerEndpoint = "<servername>.database.windows.net"
$SSISDBServerAdminUserName = "<username>"
$SSISDBServerAdminPassword = "<password>"

# Load the IntegrationServices Assembly
[System.Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.Management.IntegrationServices") | Out-Null;

# Store the IntegrationServices Assembly namespace to avoid typing it every time
$ISNamespace = "Microsoft.SqlServer.Management.IntegrationServices"

Write-Host "Connecting to server ..."

# Create a connection to the server
$sqlConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID="+ $SSISDBServerAdminUserName +";Password="+ $SSISDBServerAdminPassword + ";Initial Catalog=SSISDB"
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $sqlConnectionString

# Create the Integration Services object
$integrationServices = New-Object $ISNamespace".IntegrationServices" $sqlConnection

# Get the catalog
$catalog = $integrationServices.Catalogs['SSISDB']

write-host "Enumerating all folders..."

$folders = ls -Path $ProjectFilePath -Directory

if ($folders.Count -gt 0)
{
    foreach ($filefolder in $folders)
    {
        Write-Host "Creating Folder " + $filefolder.Name + " ..."

        # Create a new folder
        $folder = New-Object $ISNamespace".CatalogFolder" ($catalog, $filefolder.Name, "Folder description")
        $folder.Create()

        $projects = ls -Path $filefolder.FullName -File -Filter *.ispac
        if ($projects.Count -gt 0)
        {
            foreach($projectfile in $projects)
            {
                $projectfilename = $projectfile.Name.Replace(".ispac", "")
                Write-Host "Deploying " + $projectfilename + " project ..."

                # Read the project file, and deploy it to the folder
                [byte[]] $projectFileContent = [System.IO.File]::ReadAllBytes($projectfile.FullName)
                $folder.DeployProject($projectfilename, $projectFileContent)
            }
        }
    }
}

Write-Host "All done." 
```

## <a name="run-a-package"></a>Ausführen eines Pakets

1. Wählen Sie im Objekt-Explorer von SSMS das Paket aus, das Sie ausführen möchten.

2. Klicken Sie mit der rechten Maustaste auf das ausgewählte Paket, und klicken Sie auf **Ausführen**, um das Dialogfeld **Paket ausführen** zu öffnen.

3.  Konfigurieren Sie im Dialogfeld **Paket ausführen** die Paketausführung, indem Sie die Einstellungen auf den Registerkarten **Parameter**, **Verbindungs-Manager** und **Erweitert** nutzen.

4.  Klicken Sie auf **OK**, um das Paket auszuführen.

## <a name="monitor-the-running-package-in-ssms"></a>Überwachen des ausgeführten Pakets in SSMS

Um die Status der ausgeführten Integration Services-Vorgänge, wie Bereitstellung, Überprüfung und Paketausführung, auf dem Integration Services-Server anzuzeigen, verwenden Sie das Dialogfeld **Aktive Vorgänge** in SSMS. Klicken Sie mit der rechten Maustaste auf **SSISDB**, und klicken Sie dann auf **Aktive Vorgänge**, um das Dialogfeld **Aktive Vorgänge** zu öffnen.

Sie können auch im Objekt-Explorer ein Paket auswählen, mit der rechten Maustaste darauf klicken, und dann auf **Berichte** > **Standardberichte** > **Alle Ausführungen** klicken.

Weitere Informationen zum Überwachen von ausgeführten Paketen in SSMS finden Sie unter [Monitor Running Packages and Other Operations (Überwachen ausgeführter Pakete und anderer Vorgänge)](../performance/monitor-running-packages-and-other-operations.md).

## <a name="monitor-the-execute-ssis-package-activity"></a>Überwachen der SSIS-Paketaktivität

Wenn Sie ein Paket als Teil einer Azure Data Factory-Pipeline mit der Aktivität „SSIS-Paket ausführen“ ausführen, können Sie die Pipelineausführungen auf der Data Factory-Benutzeroberfläche überwachen. Anschließend können Sie Ausführungs-ID von SSISDB aus der Ausgabe der Aktivitätsausführung abrufen und diese ID dann zum Überprüfen verständlicherer Ausführungsprotokolle und Fehlermeldungen in SSMS verwenden.

![Abrufen der Paketausführungs-ID in Data Factory](media/ssis-azure-deploy-run-monitor-tutorial/get-execution-id.png)

## <a name="monitor-the-azure-ssis-integration-runtime"></a>Überwachen der Azure SSIS Integration Runtime

Um die Statusinformationen über die Azure SSIS Integration Runtime-Komponente abzurufen, in der Pakete ausgeführt werden, verwenden Sie die folgenden PowerShell-Befehle. Stellen Sie für jeden Befehl die Namen der Data Factory, Azure SSIS IR sowie die Ressourcengruppe bereit.

Weitere Informationen finden Sie unter [Monitor Azure-SSIS integration runtime (Überwachen der Azure SSIS Integration Runtime)](/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime).

### <a name="get-metadata-about-the-azure-ssis-integration-runtime"></a>Abrufen von Metadaten über Azure SSIS Integration Runtime

```powershell
Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSsisIRName -ResourceGroupName $ResourceGroupName
```

### <a name="get-the-status-of-the-azure-ssis-integration-runtime"></a>Abrufen des Status von Azure SSIS Integration Runtime

```powershell
Get-AzDataFactoryV2IntegrationRuntime -Status -DataFactoryName $DataFactoryName -Name $AzureSsisIRName -ResourceGroupName $ResourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte
- Lernen Sie, wie Sie die Paketausführung zeitlich planen. Weitere Informationen finden Sie unter [Schedule SSIS package execution on Azure (Planen der Ausführung von SSIS-Paketen in Azure)](ssis-azure-schedule-packages.md)
