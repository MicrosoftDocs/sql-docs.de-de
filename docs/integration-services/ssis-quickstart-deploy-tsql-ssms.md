---
description: Bereitstellen eines SSIS-Projekts aus SSMS mit Transact-SQL
title: Bereitstellen eines SSIS-Projekts mit Transact-SQL (SSMS) | Microsoft-Dokumentation
ms.date: 05/21/2018
ms.topic: quickstart
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 609090873281197875eba8cfeb54292df9f79ba9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354232"
---
# <a name="deploy-an-ssis-project-from-ssms-with-transact-sql"></a>Bereitstellen eines SSIS-Projekts aus SSMS mit Transact-SQL

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



In diesem Schnellstart wird erläutert, wie Sie mit SQL Server Management Studio (SSMS) eine Verbindung mit der SSIS-Katalogdatenbank herstellen und anschließend mit Transact-SQL-Anweisungen ein im SSIS-Katalog gespeichertes SSIS-Projekt bereitstellen. 

SQL Server Management Studio ist eine integrierte Umgebung zum Verwalten jeder beliebigen SQL-Infrastruktur, von SQL Server bis hin zur SQL-Datenbank. Weitere Informationen zu SSMS finden Sie unter [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md).

## <a name="prerequisites"></a>Voraussetzungen

Prüfen Sie, ob Sie über die neueste Version von SQL Server Management Studio verfügen, bevor Sie beginnen. Wie Sie SSMS herunterladen, erfahren Sie unter [Herunterladen von SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).

## <a name="supported-platforms"></a>Unterstützte Plattformen

Mithilfe der Informationen in diesem Schnellstart können Sie auf den folgenden Plattformen SSIS-Projekte bereitstellen:

-   SQL Server unter Windows

Mithilfe der Informationen in diesem Schnellstart können Sie keine SSIS-Pakete in Azure SQL-Datenbank bereitstellen. Die gespeicherte Prozedur `catalog.deploy_project` erwartet den Pfad zur Datei `.ispac` im lokalen Dateisystem. Weitere Informationen zum Bereitstellen und Ausführen von Paketen in Azure finden Sie unter [Migration von SQL Server Integration Services-Workloads in die Cloud per Lift & Shift](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Dieser Schnellstart enthält keine Anleitung zum Bereitstellen von SSIS-Paketen in SQL Server unter Linux. Weitere Informationen zum Ausführen von Paketen finden Sie unter [Extrahieren, Transformieren und Laden von Daten unter Linux mit SSIS](../linux/sql-server-linux-migrate-ssis.md).

## <a name="supported-authentication-method"></a>Unterstützte Authentifizierungsmethode

Weitere Informationen finden Sie unter [Authentifizierungsmethoden für die Bereitstellung](ssis-quickstart-deploy-ssms.md#authentication-methods-for-deployment).

## <a name="connect-to-the-ssis-catalog-database"></a>Herstellen einer Verbindung mit der SSIS-Katalogdatenbank

Verwenden Sie SQL Server Management Studio, um eine Verbindung mit dem SSIS-Katalog einzurichten. 

1. Öffnen Sie SQL Server Management Studio.

2. Geben Sie im Dialogfeld **Mit Server verbinden** die folgenden Informationen ein:

   | Einstellung       | Vorgeschlagener Wert | Weitere Informationen | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servertyp** | Datenbank-Engine | Dieser Wert ist erforderlich. |
   | **Servername** | Der vollqualifizierte Servername |  |
   | **Authentifizierung** | SQL Server-Authentifizierung | |
   | **Anmeldung** | Das Serveradministratorkonto | Dabei handelt es sich um das Konto, das Sie beim Erstellen des Servers angegeben haben. |
   | **Kennwort** | Das Kennwort für das Serveradministratorkonto | Dabei handelt es sich um das Kennwort, das Sie beim Erstellen des Servers angegeben haben. |

3. Klicken Sie auf **Verbinden**. Das Fenster „Objekt-Explorer“ wird in SSMS geöffnet. 

4. Erweitern Sie im Objekt-Explorer **Integration Services-Kataloge** und dann **SSISDB**, um die Objekte in der SSIS-Katalogdatenbank anzuzeigen.


## <a name="run-the-t-sql-code"></a>Ausführen des T-SQL-Codes
Führen Sie den folgenden Transact-SQL-Code aus, um ein SSIS-Projekt bereitzustellen.

1.  Öffnen Sie in SSMS ein neues Abfragefenster, und fügen Sie den folgenden Code ein.

2.  Aktualisieren Sie die Parameterwerte in der für das System gespeicherten `catalog.deploy_project`-Prozedur.

3.  Achten Sie darauf, dass es sich bei **SSISDB** um die aktuelle Datenbank handelt.

4.  Führen Sie das Skript aus.

5. Aktualisieren Sie im Objekt-Explorer ggf. die Inhalte von **SSISDB**, und überprüfen Sie das Projekt, das Sie bereitgestellt haben.

```sql
DECLARE @ProjectBinary AS varbinary(max)
DECLARE @operation_id AS bigint
SET @ProjectBinary =
    (SELECT * FROM OPENROWSET(BULK '<project_file_path>.ispac', SINGLE_BLOB) AS BinaryData)

EXEC catalog.deploy_project @folder_name = '<target_folder>',
    @project_name = '<project_name',
    @Project_Stream = @ProjectBinary,
    @operation_id = @operation_id out
```

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über weitere Möglichkeiten, ein Paket bereitzustellen.
    - [Deploy an SSIS package with SSMS (Bereitstellen eines SSIS-Pakets mit SSMS)](./ssis-quickstart-deploy-ssms.md)
    - [Deploy an SSIS package with Transact-SQL (VS Code) (Bereitstellen eines SSIS-Pakets mit Transact-SQL [VS Code])](ssis-quickstart-deploy-tsql-vscode.md)
    - [Deploy an SSIS package from the command prompt (Bereitstellen eines SSIS-Pakets von der Befehlszeile aus)](./ssis-quickstart-deploy-cmdline.md)
    - [Deploy an SSIS package with PowerShell (Bereitstellen eines SSIS-Pakets mit PowerShell)](ssis-quickstart-deploy-powershell.md)
    - [Deploy an SSIS package with C#](./ssis-quickstart-deploy-dotnet.md) (Bereitstellen eines SSIS-Pakets mit C#) 
- Führen Sie ein bereitgestelltes Paket aus. Für die Ausführung eines Pakets können Sie aus mehreren Tools und Sprachen auswählen. Weitere Informationen finden Sie in den folgenden Artikeln:
    - [Run an SSIS package with SSMS](./ssis-quickstart-run-ssms.md) (Ausführen eines SSIS-Pakets mit SSMS)
    - [Run an SSIS package with Transact-SQL (SSMS)](./ssis-quickstart-run-tsql-ssms.md) (Ausführen eines SSIS-Pakets mit Transact-SQL [SSMS])
    - [Run an SSIS package with Transact-SQL (VS Code)](ssis-quickstart-run-tsql-vscode.md) (Ausführen eines SSIS-Pakets mit Transact-SQL [VS Code])
    - [Run an SSIS package from the command prompt](./ssis-quickstart-run-cmdline.md) (Ausführen eines SSIS-Pakets über die Eingabeaufforderung)
    - [Run an SSIS package with PowerShell](ssis-quickstart-run-powershell.md) (Ausführen eines SSIS-Pakets mit PowerShell)
    - [Run an SSIS package with C#](./ssis-quickstart-run-dotnet.md) (Ausführen eines SSIS-Pakets mit C#)