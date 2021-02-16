---
description: Ausführen eines SSIS-Pakets aus SSMS mit Transact-SQL (SSMS)
title: Ausführen eines SSIS-Pakets mit Transact-SQL (SSMS) | Microsoft-Dokumentation
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5b45f89b10e9e5405e7f85c662120f2cf04d7d02
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336327"
---
# <a name="run-an-ssis-package-from-ssms-with-transact-sql"></a>Ausführen eines SSIS-Pakets aus SSMS mit Transact-SQL (SSMS)

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


In diesem Schnellstart wird erläutert, wie Sie mit SQL Server Management Studio (SSMS) eine Verbindung mit der SSIS-Katalogdatenbank herstellen und anschließend mit Transact-SQL-Anweisungen ein im SSIS-Katalog gespeichertes SSIS-Paket ausführen.

SQL Server Management Studio ist eine integrierte Umgebung zum Verwalten jeder beliebigen SQL-Infrastruktur, von SQL Server bis hin zur SQL-Datenbank. Weitere Informationen zu SSMS finden Sie unter [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md).

## <a name="prerequisites"></a>Voraussetzungen

Prüfen Sie, ob Sie über die neueste Version von SQL Server Management Studio (SSMS) verfügen, bevor Sie beginnen. Wie Sie SSMS herunterladen, erfahren Sie unter [Herunterladen von SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).

Ein Azure SQL-Datenbank-Server überwacht Port 1433. Wenn Sie versuchen, eine Verbindung mit einem Azure SQL-Datenbank-Server innerhalb einer Unternehmensfirewall herzustellen, muss dieser Port in der Unternehmensfirewall geöffnet sein, damit Sie eine Verbindung herstellen können.

## <a name="supported-platforms"></a>Unterstützte Plattformen

Mithilfe der Informationen in diesem Schnellstart können Sie auf den folgenden Plattformen SSIS-Pakete ausführen:

-   SQL Server unter Windows

-   Azure SQL-Datenbank. Weitere Informationen zum Bereitstellen und Ausführen von Paketen in Azure finden Sie unter [Migration von SQL Server Integration Services-Workloads in die Cloud per Lift & Shift](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Anhand der Informationen in diesem Schnellstart können Sie unter Linux keine SSIS-Pakete ausführen. Weitere Informationen zum Ausführen von Paketen finden Sie unter [Extrahieren, Transformieren und Laden von Daten unter Linux mit SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="for-azure-sql-database-get-the-connection-info"></a>Abrufen der Verbindungsinformationen für Azure SQL-Datenbank

Um das Paket auf Azure SQL-Datenbank auszuführen, rufen Sie die Verbindungsinformationen ab, die für eine Verbindungsherstellung mit der SSIS-Katalogdatenbank (SSISDB) benötigt werden. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen für die folgenden Prozeduren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie aus dem Menü auf der linken Seite **SQL-Datenbanken** aus, und klicken Sie auf der Seite **SQL-Datenbanken** auf die SSISDB-Datenbank. 
3. Überprüfen Sie auf der **Übersichtsseite** Ihrer Datenbank den vollqualifizierten Servernamen. Zeigen Sie auf den Servernamen, damit die Option **Klicken Sie zum Kopieren** angezeigt wird. 
4. Wenn Sie die Anmeldeinformationen für Ihren Azure SQL-Datenbank-Server vergessen, navigieren Sie zur Seite „SQL Datenbank-Server“, damit der Serveradministratorname angezeigt wird. Sie können das Kennwort falls erforderlich zurücksetzen.

## <a name="connect-to-the-ssisdb-database"></a>Herstellen einer Verbindung mit SSISDB

Verwenden Sie SQL Server Management Studio, um eine Verbindung mit dem SSIS-Katalog auf Ihrem Azure SQL-Datenbank-Server einzurichten. 

1. Öffnen Sie SQL Server Management Studio.

2. Geben Sie im Dialogfeld **Mit Server verbinden** die folgenden Informationen ein:

   | Einstellung       | Vorgeschlagener Wert | Weitere Informationen | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servertyp** | Datenbank-Engine | Dieser Wert ist erforderlich. |
   | **Servername** | Der vollqualifizierte Servername | Wenn Sie eine Verbindung mit einem Azure SQL-Datenbank-Server herstellen, ist der Name im Format `<server_name>.database.windows.net`. |
   | **Authentifizierung** | SQL Server-Authentifizierung | Mit der SQL Server-Authentifizierung können Sie eine Verbindung zu SQL Server oder Azure SQL-Datenbank herstellen. Wenn Sie eine Verbindung mit einem Azure SQL-Datenbank-Server herstellen, können Sie keine Windows-Authentifizierung verwenden. |
   | **Anmeldung** | Das Serveradministratorkonto | Dabei handelt es sich um das Konto, das Sie beim Erstellen des Servers angegeben haben. |
   | **Kennwort** | Das Kennwort für das Serveradministratorkonto | Dabei handelt es sich um das Kennwort, das Sie beim Erstellen des Servers angegeben haben. |

3.  Klicken Sie auf **Verbinden**. Das Fenster „Objekt-Explorer“ wird in SSMS geöffnet.

4. Erweitern Sie im Objekt-Explorer **Integration Services-Kataloge** und dann **SSISDB**, um die Objekte in der SSIS-Katalogdatenbank anzuzeigen.

## <a name="run-a-package"></a>Ausführen eines Pakets
Führen Sie den folgenden Transact-SQL-Code aus, um ein SSIS-Paket auszuführen.

1.  Öffnen Sie in SSMS ein neues Abfragefenster, und fügen Sie den folgenden Code ein. (Dabei handelt es sich um den Code, der durch die Option **Skript** im Dialogfeld **Paket ausführen** in SSMS generiert wurde.)

2.  Aktualisieren Sie die Parameterwerte in der für das System gespeicherten `catalog.create_execution`-Prozedur.

3.  Prüfen Sie, ob es sich bei SSISDB um die aktuelle Datenbank handelt.

4.  Führen Sie das Skript aus.

5. Aktualisieren Sie im Objekt-Explorer ggf. die Inhalte von **SSISDB**, und überprüfen Sie das Projekt, das Sie bereitgestellt haben.

```sql
Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx',
    @execution_id=@execution_id OUTPUT,
    @folder_name=N'Deployed Projects',
      @project_name=N'Integration Services Project1',
    @use32bitruntime=False,
      @reference_id=Null
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,
    @object_type=50,
      @parameter_name=N'LOGGING_LEVEL',
      @parameter_value=@var0
EXEC [SSISDB].[catalog].[start_execution] @execution_id
GO
```

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über weitere Möglichkeiten, ein Paket auszuführen.
    - [Run an SSIS package with SSMS](./ssis-quickstart-run-ssms.md) (Ausführen eines SSIS-Pakets mit SSMS)
    - [Run an SSIS package with Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md) (Ausführen eines SSIS-Pakets mit Transact-SQL [VS Code])
    - [Run an SSIS package from the command prompt](./ssis-quickstart-run-cmdline.md) (Ausführen eines SSIS-Pakets über die Eingabeaufforderung)
    - [Run an SSIS package with PowerShell](ssis-quickstart-run-powershell.md) (Ausführen eines SSIS-Pakets mit PowerShell)
    - [Run an SSIS package with C#](./ssis-quickstart-run-dotnet.md) (Ausführen eines SSIS-Pakets mit C#)