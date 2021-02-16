---
description: Ausführen eines SSIS-Pakets mit C#-Code in einer .NET-App
title: Ausführen eines SSIS-Projekts mit .NET-Code (C#) | Microsoft-Dokumentation
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ba99a313c4d446bdb07030355d7efce0f2d8e862
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336312"
---
# <a name="run-an-ssis-package-with-c-code-in-a-net-app"></a>Ausführen eines SSIS-Pakets mit C#-Code in einer .NET-App

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


In diesem Schnellstart wird dargestellt, wie C#-Code geschrieben wird, um eine Verbindung mit einem Datenbankserver herzustellen und ein SSIS-Paket auszuführen.

Sie können Visual Studio, Visual Studio Code oder ein anderes Tool Ihrer Wahl verwenden, um eine C#-App zu erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, stellen Sie sicher, dass Sie Visual Studio oder Visual Studio Code installiert haben. Laden Sie die kostenlose Community Edition von Visual Studio oder Visual Studio Code (ebenfalls kostenlos) unter [Visual Studio-Downloads](https://www.visualstudio.com/downloads/) herunter.

Ein Azure SQL-Datenbank-Server überwacht Port 1433. Wenn Sie versuchen, eine Verbindung mit einem Azure SQL-Datenbank-Server innerhalb einer Unternehmensfirewall herzustellen, muss dieser Port in der Unternehmensfirewall geöffnet sein, damit Sie eine Verbindung herstellen können.

## <a name="for-azure-sql-database-get-the-connection-info"></a>Abrufen der Verbindungsinformationen für Azure SQL-Datenbank

Um das Paket auf Azure SQL-Datenbank auszuführen, rufen Sie die Verbindungsinformationen ab, die für eine Verbindungsherstellung mit der SSIS-Katalogdatenbank (SSISDB) benötigt werden. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen für die folgenden Prozeduren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie aus dem Menü auf der linken Seite **SQL-Datenbanken** aus, und klicken Sie auf der Seite **SQL-Datenbanken** auf die SSISDB-Datenbank. 
3. Überprüfen Sie auf der **Übersichtsseite** Ihrer Datenbank den vollqualifizierten Servernamen. Zeigen Sie auf den Servernamen, damit die Option **Klicken Sie zum Kopieren** angezeigt wird. 
4. Wenn Sie die Anmeldeinformationen für Ihren Azure SQL-Datenbank-Server vergessen, navigieren Sie zur Seite „SQL Datenbank-Server“, damit der Serveradministratorname angezeigt wird. Sie können das Kennwort falls erforderlich zurücksetzen.
5. Klicken Sie auf **Datenbank-Verbindungszeichenfolgen anzeigen**.
6. Überprüfen Sie die vollständige **ADO.NET**-Verbindungszeichenfolge. Der Beispielcode kann mithilfe von `SqlConnectionStringBuilder` die Verbindungszeichenfolge mit den von Ihnen angegebenen individuellen Parameterwerten neu erstellen.

## <a name="create-a-new-visual-studio-project"></a>Erstellen eines neuen Visual Studio-Projekts

1. Klicken Sie in Visual Studio auf **Datei** > **Neu** > **Projekt**. 
2. Erweitern Sie **Visual C#** im Dialogfeld **Neues Projekt**.
3. Klicken Sie auf **Konsolenanwendung**, und geben Sie für den Projektnamen *run_ssis_project* an.
4. Klicken Sie auf **OK**, um das neue Projekt in Visual Studio zu erstellen und zu öffnen.

## <a name="add-references"></a>Hinzufügen von Verweisen
1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner **Verweise**, und wählen Sie **Verweis hinzufügen** aus. Das Dialogfeld **Verweis-Manager** wird geöffnet.
2. Erweitern Sie **Assemblys** im **Verweis-Manager**, und klicken Sie auf **Erweiterungen**.
3. Wählen Sie die folgenden beiden Verweise aus, die hinzugefügt werden sollen:
    -   Microsoft.SqlServer.Management.Sdk.Sfc
    -   Microsoft.SqlServer.Smo
4. Klicken Sie auf die Schaltfläche **Durchsuchen**, um **Microsoft.SqlServer.Management.IntegrationServices** einen Verweis hinzuzufügen. (Diese Assembly ist nur im globalen Assemblycache (Global Assembly Cache, GAC) installiert.) Das Dialogfeld **Zu referenzierende Dateien auswählen** wird geöffnet.
5. Navigieren Sie im Dialogfeld **Zu referenzierende Dateien auswählen** zum GAC-Ordner, der die Assembly enthält. In der Regel handelt es sich dabei um den Ordner `C:\Windows\assembly\GAC_MSIL\Microsoft.SqlServer.Management.IntegrationServices\14.0.0.0__89845dcd8080cc91`.
6. Wählen Sie die Assembly (d.h. die DLL-Datei) in dem Ordner aus, und klicken Sie auf **Hinzufügen**.
7. Klicken Sie auf **OK**, um das Dialogfeld **Verweis-Manager** zu schließen und die drei Verweise hinzuzufügen. Überprüfen Sie die Liste **Verweise** im Projektmappen-Explorer, um sicherzustellen, dass die Verweise hinzugefügt wurden.

## <a name="add-the-c-code"></a>Hinzufügen von C#-Code 
1. Öffnen Sie **Program.cs**.

2. Ersetzen Sie die Inhalte von **Program.cs** durch den folgenden Code. Fügen Sie die entsprechenden Werte für Ihren Server, die Datenbank, den Benutzer und das Kennwort hinzu.

> [!NOTE]
> In diesem Beispiel wird die Windows-Authentifizierung verwendet. Ersetzen Sie das Argument `Integrated Security=SSPI;` durch `User ID=<user name>;Password=<password>;`, um die SQL Server-Authentifizierung zu verwenden. Wenn Sie eine Verbindung mit einem Azure SQL-Datenbank-Server herstellen, können Sie keine Windows-Authentifizierung verwenden.


```csharp
using Microsoft.SqlServer.Management.IntegrationServices;
using System.Data.SqlClient;

namespace run_ssis_package
{
    class Program
    {
        static void Main(string[] args)
        {
            // Variables
            string targetServerName = "localhost";
            string folderName = "Project1Folder";
            string projectName = "Integration Services Project1";
            string packageName = "Package.dtsx";

            // Create a connection to the server
            string sqlConnectionString = "Data Source=" + targetServerName +
                ";Initial Catalog=master;Integrated Security=SSPI;";
            SqlConnection sqlConnection = new SqlConnection(sqlConnectionString);

            // Create the Integration Services object
            IntegrationServices integrationServices = new IntegrationServices(sqlConnection);

            // Get the Integration Services catalog
            Catalog catalog = integrationServices.Catalogs["SSISDB"];

            // Get the folder
            CatalogFolder folder = catalog.Folders[folderName];

            // Get the project
            ProjectInfo project = folder.Projects[projectName];

            // Get the package
            PackageInfo package = project.Packages[packageName];

            // Run the package
            package.Execute(false, null);

        }
    }
}
```

## <a name="run-the-code"></a>Ausführen des Codes

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Überprüfen Sie, ob das Paket wie erwartet ausgeführt wurde, und schließen Sie dann das Anwendungsfenster.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über weitere Möglichkeiten, ein Paket auszuführen.
    - [Run an SSIS package with SSMS](./ssis-quickstart-run-ssms.md) (Ausführen eines SSIS-Pakets mit SSMS)
    - [Run an SSIS package with Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md) (Ausführen eines SSIS-Pakets mit Transact-SQL [SSMS])
    - [Run an SSIS package with Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md) (Ausführen eines SSIS-Pakets mit Transact-SQL [VS Code])
    - [Run an SSIS package from the command prompt](./ssis-quickstart-run-cmdline.md) (Ausführen eines SSIS-Pakets über die Eingabeaufforderung)
    - [Run an SSIS package with PowerShell](ssis-quickstart-run-powershell.md) (Ausführen eines SSIS-Pakets mit PowerShell)
