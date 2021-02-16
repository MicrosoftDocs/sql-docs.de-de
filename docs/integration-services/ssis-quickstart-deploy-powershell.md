---
description: Bereitstellen eines SSIS-Projekts mit PowerShell
title: Bereitstellen eines SSIS-Projekts mit PowerShell | Microsoft- Dokumentation
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e4cbdedce72ad96da140930dacdfb8955bfc8b60
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354244"
---
# <a name="deploy-an-ssis-project-with-powershell"></a>Bereitstellen eines SSIS-Projekts mit PowerShell

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


In diesem Schnellstart wird dargestellt, wie Sie mit einem PowerShell-Skript eine Verbindung zu einem Datenbankserver herstellen und SSIS-Projekte im SSIS-Katalog bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

Ein Azure SQL-Datenbank-Server überwacht Port 1433. Wenn Sie versuchen, eine Verbindung mit einem Azure SQL-Datenbank-Server innerhalb einer Unternehmensfirewall herzustellen, muss dieser Port in der Unternehmensfirewall geöffnet sein, damit Sie eine Verbindung herstellen können.

## <a name="supported-platforms"></a>Unterstützte Plattformen

Mithilfe der Informationen in diesem Schnellstart können Sie auf den folgenden Plattformen SSIS-Projekte bereitstellen:

-   SQL Server unter Windows

-   Azure SQL-Datenbank. Weitere Informationen zum Bereitstellen und Ausführen von Paketen in Azure finden Sie unter [Migration von SQL Server Integration Services-Workloads in die Cloud per Lift & Shift](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Dieser Schnellstart enthält keine Anleitung zum Bereitstellen von SSIS-Paketen in SQL Server unter Linux. Weitere Informationen zum Ausführen von Paketen finden Sie unter [Extrahieren, Transformieren und Laden von Daten unter Linux mit SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="for-azure-sql-database-get-the-connection-info"></a>Abrufen der Verbindungsinformationen für Azure SQL-Datenbank

Um das Paket in Azure SQL-Datenbank bereitzustellen, rufen Sie die Verbindungsinformationen ab, die für die Verbindungsherstellung mit der SSIS-Katalogdatenbank (SSISDB) benötigt werden. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen für die folgenden Prozeduren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie aus dem Menü auf der linken Seite **SQL-Datenbanken** aus, und klicken Sie auf der Seite **SQL-Datenbanken** auf die SSISDB-Datenbank. 
3. Überprüfen Sie auf der **Übersichtsseite** Ihrer Datenbank den vollqualifizierten Servernamen. Zeigen Sie auf den Servernamen, damit die Option **Klicken Sie zum Kopieren** angezeigt wird. 
4. Wenn Sie die Anmeldeinformationen für Ihren Azure SQL-Datenbank-Server vergessen, navigieren Sie zur Seite „SQL Datenbank-Server“, damit der Serveradministratorname angezeigt wird. Sie können das Kennwort falls erforderlich zurücksetzen.
5. Klicken Sie auf **Datenbank-Verbindungszeichenfolgen anzeigen**.
6. Überprüfen Sie die vollständige **ADO.NET**-Verbindungszeichenfolge.

## <a name="supported-authentication-method"></a>Unterstützte Authentifizierungsmethode

Weitere Informationen finden Sie unter [Authentifizierungsmethoden für die Bereitstellung](ssis-quickstart-deploy-ssms.md#authentication-methods-for-deployment).

## <a name="ssis-powershell-provider"></a>SSIS-PowerShell-Anbieter
Geben Sie am Anfang des folgenden Skripts geeignete Werte für die Variablen an, und führen Sie das Skript aus, um das SSIS-Projekt bereitzustellen.

> [!NOTE]
> Im folgenden Beispiel wird mit der Windows-Authentifizierung eine SQL Server-Infrastruktur bereitgestellt. Verwenden Sie das Cmdlet `New-PSDive`, um eine Verbindung über die SQL Server-Authentifizierung herzustellen. Wenn Sie eine Verbindung mit einem Azure SQL-Datenbank-Server herstellen, können Sie keine Windows-Authentifizierung verwenden.

```powershell
# Variables
$TargetInstanceName = "localhost\default"
$TargetFolderName = "Project1Folder"
$ProjectFilePath = "C:\Projects\Integration Services Project1\Integration Services Project1\bin\Development\Integration Services Project1.ispac"
$ProjectName = "Integration Services Project1"

# Get the Integration Services catalog
$catalog = Get-Item SQLSERVER:\SSIS\$TargetInstanceName\Catalogs\SSISDB\

# Create the target folder
New-Object "Microsoft.SqlServer.Management.IntegrationServices.CatalogFolder" ($catalog, 
$TargetFolderName,"Folder description") -OutVariable folder
$folder.Create()

# Read the project file and deploy it
[byte[]] $projectFile = [System.IO.File]::ReadAllBytes($ProjectFilePath)
$folder.DeployProject($ProjectName, $projectFile)

# Verify packages were deployed.
dir "$($catalog.PSPath)\Folders\$TargetFolderName\Projects\$ProjectName\Packages" | 
SELECT Name, DisplayName, PackageId
```

## <a name="powershell-script"></a>PowerShell-Skript
Geben Sie am Anfang des folgenden Skripts geeignete Werte für die Variablen an, und führen Sie das Skript aus, um das SSIS-Projekt bereitzustellen.

> [!NOTE]
> Im folgenden Beispiel wird mit der Windows-Authentifizierung eine SQL Server-Infrastruktur bereitgestellt. Ersetzen Sie das Argument `Integrated Security=SSPI;` durch `User ID=<user name>;Password=<password>;`, um die SQL Server-Authentifizierung zu verwenden. Wenn Sie eine Verbindung mit einem Azure SQL-Datenbank-Server herstellen, können Sie keine Windows-Authentifizierung verwenden.

```powershell
# Variables
$SSISNamespace = "Microsoft.SqlServer.Management.IntegrationServices"
$TargetServerName = "localhost"
$TargetFolderName = "Project1Folder"
$ProjectFilePath = "C:\Projects\Integration Services Project1\Integration Services Project1\bin\Development\Integration Services Project1.ispac"
$ProjectName = "Integration Services Project1"

# Load the IntegrationServices assembly
$loadStatus = [System.Reflection.Assembly]::Load("Microsoft.SQLServer.Management.IntegrationServices, "+
    "Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91, processorArchitecture=MSIL")

# Create a connection to the server
$sqlConnectionString = `
    "Data Source=" + $TargetServerName + ";Initial Catalog=master;Integrated Security=SSPI;"
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $sqlConnectionString

# Create the Integration Services object
$integrationServices = New-Object $SSISNamespace".IntegrationServices" $sqlConnection

# Get the Integration Services catalog
$catalog = $integrationServices.Catalogs["SSISDB"]

# Create the target folder
$folder = New-Object $SSISNamespace".CatalogFolder" ($catalog, $TargetFolderName,
    "Folder description")
$folder.Create()

Write-Host "Deploying " $ProjectName " project ..."

# Read the project file and deploy it
[byte[]] $projectFile = [System.IO.File]::ReadAllBytes($ProjectFilePath)
$folder.DeployProject($ProjectName, $projectFile)

Write-Host "Done."
```

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über weitere Möglichkeiten, ein Paket bereitzustellen.
    - [Deploy an SSIS package with SSMS (Bereitstellen eines SSIS-Pakets mit SSMS)](./ssis-quickstart-deploy-ssms.md)
    - [Deploy an SSIS package with Transact-SQL (SSMS) (Bereitstellen eines SSIS-Pakets mit Transact-SQL [SSMS])](./ssis-quickstart-deploy-tsql-ssms.md)
    - [Deploy an SSIS package with Transact-SQL (VS Code) (Bereitstellen eines SSIS-Pakets mit Transact-SQL [VS Code])](ssis-quickstart-deploy-tsql-vscode.md)
    - [Deploy an SSIS package from the command prompt (Bereitstellen eines SSIS-Pakets von der Befehlszeile aus)](./ssis-quickstart-deploy-cmdline.md)
    - [Deploy an SSIS package with C#](./ssis-quickstart-deploy-dotnet.md) (Bereitstellen eines SSIS-Pakets mit C#) 
- Führen Sie ein bereitgestelltes Paket aus. Für die Ausführung eines Pakets können Sie aus mehreren Tools und Sprachen auswählen. Weitere Informationen finden Sie in den folgenden Artikeln:
    - [Run an SSIS package with SSMS](./ssis-quickstart-run-ssms.md) (Ausführen eines SSIS-Pakets mit SSMS)
    - [Run an SSIS package with Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md) (Ausführen eines SSIS-Pakets mit Transact-SQL [SSMS])
    - [Run an SSIS package with Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md) (Ausführen eines SSIS-Pakets mit Transact-SQL [VS Code])
    - [Run an SSIS package from the command prompt](./ssis-quickstart-run-cmdline.md) (Ausführen eines SSIS-Pakets über die Eingabeaufforderung)
    - [Run an SSIS package with PowerShell](ssis-quickstart-run-powershell.md) (Ausführen eines SSIS-Pakets mit PowerShell)
    - [Run an SSIS package with C#](./ssis-quickstart-run-dotnet.md) (Ausführen eines SSIS-Pakets mit C#) 
