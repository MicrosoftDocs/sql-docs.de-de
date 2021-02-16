---
description: Ausführen eines SSIS-Pakets mit PowerShell
title: Ausführen eines SSIS-Pakets mit PowerShell | Microsoft-Dokumentation
ms.date: 09/17/2020
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0f82c5746f456fe03b3fdca21b0297fbcc63d0d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348209"
---
# <a name="run-an-ssis-package-with-powershell"></a>Ausführen eines SSIS-Pakets mit PowerShell

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


In diesem Schnellstart wird dargestellt, wie Sie mit einem PowerShell-Skript eine Verbindung zu einem Datenbankserver herstellen und ein SSIS-Paket ausführen.

## <a name="prerequisites"></a>Voraussetzungen

Ein Azure SQL-Datenbank-Server überwacht Port 1433. Wenn Sie versuchen, eine Verbindung mit einem Azure SQL-Datenbank-Server innerhalb einer Unternehmensfirewall herzustellen, muss dieser Port in der Unternehmensfirewall geöffnet sein, damit Sie eine Verbindung herstellen können.

## <a name="supported-platforms"></a>Unterstützte Plattformen

Mithilfe der Informationen in diesem Schnellstart können Sie auf den folgenden Plattformen SSIS-Pakete ausführen:

-   SQL Server unter Windows

-   SSIS Integration Runtime (IR) in Azure Data Factory (ADF), wenn der SSIS-Katalog (SSISDB) von Azure SQL Managed Instance (MI) gehostet wird. Weitere Informationen zum Bereitstellen und Ausführen von Paketen in Azure finden Sie unter [Migration von SQL Server Integration Services-Workloads in die Cloud per Lift & Shift](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Anhand der Informationen in diesem Schnellstart können Sie unter Linux keine SSIS-Pakete ausführen. Weitere Informationen zum Ausführen von Paketen finden Sie unter [Extrahieren, Transformieren und Laden von Daten unter Linux mit SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="for-azure-sql-database-get-the-connection-info"></a>Abrufen der Verbindungsinformationen für Azure SQL-Datenbank

Um das Paket auf Azure SQL-Datenbank auszuführen, rufen Sie die Verbindungsinformationen ab, die für eine Verbindungsherstellung mit der SSIS-Katalogdatenbank (SSISDB) benötigt werden. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen für die folgenden Prozeduren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie aus dem Menü auf der linken Seite **SQL-Datenbanken** aus, und klicken Sie auf der Seite **SQL-Datenbanken** auf die SSISDB-Datenbank. 
3. Überprüfen Sie auf der **Übersichtsseite** Ihrer Datenbank den vollqualifizierten Servernamen. Zeigen Sie auf den Servernamen, damit die Option **Klicken Sie zum Kopieren** angezeigt wird. 
4. Wenn Sie die Anmeldeinformationen für Ihren Azure SQL-Datenbank-Server vergessen, navigieren Sie zur Seite „SQL Datenbank-Server“, damit der Serveradministratorname angezeigt wird. Sie können das Kennwort falls erforderlich zurücksetzen.
5. Klicken Sie auf **Datenbank-Verbindungszeichenfolgen anzeigen**.
6. Überprüfen Sie die vollständige **ADO.NET**-Verbindungszeichenfolge.

## <a name="ssis-powershell-provider"></a>SSIS-PowerShell-Anbieter
Sie können den SSIS-PowerShell-Anbieter verwenden, um eine Verbindung mit dem SSIS-Katalog herzustellen und Pakete in diesem auszuführen.

Nachfolgend finden Sie ein grundlegendes Beispiel zum Ausführen von SSIS-Paketen in einem Paketkatalog mithilfe des SSIS-PowerShell-Anbieters.

```powershell
(Get-ChildItem SQLSERVER:\SSIS\localhost\Default\Catalogs\SSISDB\Folders\Project1Folder\Projects\'Integration Services Project1'\Packages\ |
WHERE { $_.Name -eq 'Package.dtsx' }).Execute("false", $null)
```

## <a name="powershell-script"></a>PowerShell-Skript
Geben Sie am Anfang des folgenden Skripts geeignete Werte für die Variablen an, und führen Sie das Skript aus, um das SSIS-Paket auszuführen.

> [!NOTE]
> In diesem Beispiel wird die Windows-Authentifizierung verwendet. Ersetzen Sie das Argument `Integrated Security=SSPI;` durch `User ID=<user name>;Password=<password>;`, um die SQL Server-Authentifizierung zu verwenden. Wenn Sie eine Verbindung mit einem Azure SQL-Datenbank-Server herstellen, können Sie keine Windows-Authentifizierung verwenden. 

```powershell
# Variables
$SSISNamespace = "Microsoft.SqlServer.Management.IntegrationServices"
$TargetServerName = "localhost"
$TargetFolderName = "Project1Folder"
$ProjectName = "Integration Services Project1"
$PackageName = "Package.dtsx"

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

# Get the folder
$folder = $catalog.Folders[$TargetFolderName]

# Get the project
$project = $folder.Projects[$ProjectName]

# Get the package
$package = $project.Packages[$PackageName]

Write-Host "Running " $PackageName "..."

$result = $package.Execute("false", $null)

Write-Host "Done."
```

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über weitere Möglichkeiten, ein Paket auszuführen.
    - [Run an SSIS package with SSMS](./ssis-quickstart-run-ssms.md) (Ausführen eines SSIS-Pakets mit SSMS)
    - [Run an SSIS package with Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md) (Ausführen eines SSIS-Pakets mit Transact-SQL [SSMS])
    - [Run an SSIS package with Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md) (Ausführen eines SSIS-Pakets mit Transact-SQL [VS Code])
    - [Run an SSIS package from the command prompt](./ssis-quickstart-run-cmdline.md) (Ausführen eines SSIS-Pakets über die Eingabeaufforderung)
    - [Run an SSIS package with C#](./ssis-quickstart-run-dotnet.md) (Ausführen eines SSIS-Pakets mit C#) 
