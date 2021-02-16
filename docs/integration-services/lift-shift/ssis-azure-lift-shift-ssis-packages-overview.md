---
title: Bereitstellen und Ausführen von SSIS-Paketen in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre SQL Server Integration Services-Projekte, -Pakete und -Workloads (SSIS) in die Microsoft Azure-Cloud verschieben können.
ms.date: 09/23/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 12b4e6c71698845314844e4cf0bcd5b371209641
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352004"
---
# <a name="lift-and-shift-sql-server-integration-services-workloads-to-the-cloud"></a>Migration von SQL Server Integration Services-Workloads in die Cloud per Lift & Shift

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Sie können Ihre SQL Server Integration Services-Projekte, -Pakete und -Workloads (SSIS) nun in die Azure-Cloud verschieben. SSIS-Projekte und -Pakete in der SSIS-Katalogdatenbank (SSISDB) können Sie in Azure SQL-Datenbank oder SQL Managed Instance mit gängigen Tools wie SQL Server Management Studio (SSMS) bereitstellen, ausführen und verwalten.

## <a name="benefits"></a>Vorteile
Das Verschieben Ihrer SSIS-Workload in Azure hat folgende potenzielle Vorteile:
-   **Verringern Sie Betriebskosten** und den Aufwand der Verwaltung der Infrastruktur, der anfällt, wenn Sie SSIS lokal oder auf virtuellen Azure-Computern ausführen.
-   **Erhöhen Sie die Hochverfügbarkeit** und die Hochverfügbarkeitsfeatures von Azure und Azure SQL-Datenbank durch die Möglichkeit, mehrere Knoten pro Cluster anzugeben.
-   **Erhöhen Sie die Skalierbarkeit** durch die Möglichkeit, mehrere Kerne pro Knoten (Hochskalieren) und mehrere Knoten pro Cluster (Aufskalieren) anzugeben.

## <a name="architecture-of-ssis-on-azure"></a>Architektur von SSIS in Azure
In der folgenden Tabelle werden die Unterschiede zwischen der lokalen Ausführung von SSIS und SSIS in Azure hervorgehoben.

Der wichtigste Unterschied ist die Trennung des Speichers von Runtime. Azure Data Factory hostet die Runtime-Engine für SSIS-Pakete in Azure. Die Runtime-Engine wird als Azure SSIS Integration Runtime (Azure SSIS IR) bezeichnet. Weitere Informationen finden Sie unter [Azure SSIS Integration Runtime](/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime).

| Position | Storage | Typ | Skalierbarkeit |
|---|---|---|---|
| Lokal | SQL Server | Von SQL Server gehostete SSIS-Runtime | SSIS Scale Out (in SQL Server 2017 und höher)<br/><br/>Benutzerdefinierte Projektmappen (in früheren Versionen von SQL Server) |
| In Azure | SQL-Datenbank oder verwaltete SQL-Instanz | Azure SSIS Integration Runtime, eine Komponente von Azure Data Factory | Skalierungsoptionen für die Azure SSIS Integration Runtime |
| | | | |

## <a name="provision-ssis-on-azure"></a>Bereitstellen von SSIS in Azure

**Bereitstellen:** Bevor Sie SSIS-Pakete in Azure bereitstellen und ausführen können, müssen Sie die SSIS-Katalogdatenbank (SSISDB) und Azure SSIS Integration Runtime bereitstellen.

-   Führen Sie zum Bereitstellen von SSIS in Azure im Azure-Portal die erforderlichen Schritte in diesem Artikel aus: [Bereitstellen der Azure-SSIS Integration Runtime in Azure Data Factory](/azure/data-factory/tutorial-deploy-ssis-packages-azure). 

-   Führen Sie zum Bereitstellen von SSIS in Azure mit PowerShell die erforderlichen Schritte in diesem Artikel aus: [Bereitstellen der Azure-SSIS Integration Runtime in Azure Data Factory mit PowerShell](/azure/data-factory/tutorial-deploy-ssis-packages-azure-powershell).

Sie müssen Azure SSIS IR nur einmal bereitstellen. Danach können Sie vertraute Tools wie SQL Server Data Tools (SSDT) und SQL Server Management Studio (SSMS) für das Bereitstellen, Konfigurieren, Ausführen, Überwachen, Planen und Verwalten von Paketen verwenden.

> [!NOTE]
> Die Azure-SSIS Integration Runtime ist noch nicht in allen Azure-Regionen verfügbar. Weitere Informationen zu den unterstützten Regionen finden Sie unter [Verfügbare Produkte nach Region – Microsoft Azure](https://azure.microsoft.com/regions/services/).

**Hoch- und Aufskalieren:** Wenn Sie Azure SSIS IR bereitstellen, können Sie hoch- und aufskalieren, indem Sie Werte für die folgenden Optionen angeben:
-   Die Knotengröße (einschließlich der Anzahl von Kernen) und die Anzahl von Knoten im Cluster.
-   Die vorhandene Instanz von Azure SQL-Datenbank, auf der die SSIS-Katalogdatenbank (SSISDB) gehostet werden soll, und die Dienstebene der Datenbank.
-   Die maximalen parallelen Ausführungen pro Knoten.

**Verbessern der Leistung:** Weitere Informationen finden Sie unter [Konfigurieren von Azure SSIS Integration Runtime für hohe Leistung](/azure/data-factory/configure-azure-ssis-integration-runtime-performance).

**Senken der Kosten:** Um Kosten zu senken, führen Sie die Azure-SSIS IR nur bei Bedarf aus. Weitere Informationen finden Sie unter [Starten und Beenden der Azure SSIS Integration Runtime nach einem Zeitplan](/azure/data-factory/how-to-schedule-azure-ssis-integration-runtime).

## <a name="design-packages"></a>Entwerfen von Paketen

Sie können Pakete weiterhin lokal in SSDT oder in Visual Studio (wenn SSDT installiert ist) **entwerfen und erstellen**.

### <a name="connect-to-data-sources"></a>Verbindung mit Datenquellen herstellen

Weitere Informationen zur Verbindung mit lokalen Datenquellen aus der Cloud mithilfe der **Windows-Authentifizierung** finden Sie unter [Herstellen einer Verbindung mit Datenquellen und Dateifreigaben mit der Windows-Authentifizierung in SSIS-Paketen in Azure](/azure/data-factory/ssis-azure-connect-with-windows-auth).

Weitere Informationen zur Verbindung mit Dateien und Dateifreigaben finden Sie unter [Öffnen und Speichern von Dateien lokal und in Azure mit in Azure bereitgestellten SSIS-Paketen](/azure/data-factory/ssis-azure-files-file-shares).

### <a name="available-ssis-components"></a>Verfügbare SSIS-Komponenten

Wenn Sie eine Instanz der SQL-Datenbank zum Hosten der SSISDB bereitstellen, werden auch das Azure Feature Pack für SSIS und die Access Redistributable-Komponente installiert. Diese Komponenten stellen zusätzlich zu den Datenquellen, die von den integrierten Komponenten unterstützt werden, die Konnektivität zu **Excel- und Access-Dateien** und zu verschiedenen **Azure**-Datenquellen bereit.

Sie können auch weitere Komponenten installieren, wie z.B. einen Treiber, der standardmäßig nicht installiert ist. Weitere Informationen finden Sie unter [Customize setup for the Azure-SSIS integration runtime (Anpassen des Setups von Azure SSIS Integration Runtime)](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

Wenn Sie über eine Lizenz für die Enterprise Edition verfügen, stehen zusätzliche Komponenten zur Verfügung. Weitere Informationen finden Sie unter [Bereitstellen der Enterprise-Edition für die Azure-SSIS Integration Runtime](/azure/data-factory/how-to-configure-azure-ssis-ir-enterprise-edition).

Wenn Sie ein unabhängiger Softwareanbieter sind, können Sie die Installation Ihrer lizensierten Komponente aktualisieren, um sie in Azure zur Verfügung zu stellen. Weitere Informationen finden Sie unter [Installieren kostenpflichtiger oder lizenzierter benutzerdefinierter Komponenten für Azure SSIS Integration Runtime](/azure/data-factory/how-to-develop-azure-ssis-ir-licensed-components).

### <a name="transaction-support"></a>Transaktionsunterstützung

Sie können mit SQL Server auf virtuellen Computern in Azure und lokal Microsoft Distributed Transaction Coordinator-Transaktionen (MS DTC) verwenden. Verwenden Sie die Funktion für die benutzerdefinierte Installation, um MS DTC auf jedem Knoten von Azure SSIS IR zu konfigurieren. Weitere Informationen finden Sie unter [Benutzerdefiniertes Setup von Azure-SSIS Integration Runtime](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

Mit Azure SQL-Datenbank können Sie nur elastische Transaktionen verwenden. Weitere Informationen finden Sie unter [Verteilte Transaktionen über Clouddatenbanken](/azure/sql-database/sql-database-elastic-transactions-overview).

## <a name="deploy-and-run-packages"></a>Bereitstellen und Ausführen von Paketen

Informationen zu den ersten Schritten finden Sie unter [Tutorial: Bereitstellen und Ausführen eines SSIS-Pakets in Azure](ssis-azure-deploy-run-monitor-tutorial.md).

### <a name="prerequisites"></a>Voraussetzungen

Zum Bereitstellen von SSIS-Paketen in Azure benötigen Sie eine der folgenden Versionen von SQL Server Data Tools (SSDT):
-   Version 15.3 oder höher für Visual Studio 2017.
-   Version 17.2 oder höher für Visual Studio 2015.

### <a name="connect-to-ssisdb"></a>Herstellen einer Verbindung mit SSISDB

Der **Name der SQL-Datenbank**, die SSISDB hostet, stellt den ersten Teil des vierteiligen Namens dar, den Sie beim Bereitstellen und Ausführen von Paketen von SSDT und SSMS im folgenden Format verwenden müssen: `<sql_database_name>.database.windows.net`. Weitere Informationen darüber, wie Sie eine Verbindung zur SSIS-Katalogdatenbank in Azure herstellen, finden Sie unter [Herstellen einer Verbindung mit dem SSIS-Katalog (SSISDB) in Azure](ssis-azure-connect-to-catalog-database.md).

### <a name="deploy-projects-and-packages"></a>Bereitstellen von Projekten und Paketen

Sie müssen das **Projektbereitstellungsmodell** verwenden und nicht das Paketbereitstellungsmodell, wenn Sie Projekte für SSISDB in Azure bereitstellen.

Zum Bereitstellen von Projekten in Azure können Sie folgende vertraute Tools und Skriptoptionen verwenden:
-   SQL Server Management Studio (SSMS)
-   Transact-SQL (von SSMS, Visual Studio Code oder einem anderen Tool)
-   Ein Befehlszeilentool
-   PowerShell oder C# und das Objektmodell der SSIS-Verwaltung

Der Bereitstellungsprozess überprüft Pakete, um sicherzustellen, dass sie auf der Integration Runtime von Azure SSIS ausgeführt werden können. Weitere Informationen finden Sie unter [Überprüfen von in Azure bereitgestellten SSIS-Paketen](ssis-azure-validate-packages.md).

Ein Bereitstellungsbeispiel, in dem SSMS und der Bereitstellungs-Assistent für Integration Services verwendet werden, finden Sie unter [Tutorial: Bereitstellen und Ausführen eines SSIS-Pakets in Azure](ssis-azure-deploy-run-monitor-tutorial.md).

### <a name="version-support"></a>Versionsunterstützung

Sie können ein Paket in Azure bereitstellen, das mit einer beliebigen Version von SSIS erstellt wurde. Wenn Sie ein Paket in Azure bereitstellen, wird das Paket automatisch in das aktuelle Paketformat geändert, sofern es zu keinen Überprüfungsfehlern kam. Das heißt, es wird immer auf die aktuelle Version von SSIS aktualisiert.

### <a name="run-packages"></a>Ausführen von Paketen

Zum Ausführen von in Azure bereitgestellten SSIS-Paketen gibt es eine Vielzahl von Methoden. Weitere Informationen finden Sie unter [Ausführen von in Azure bereitgestellten SSIS-Paketen](ssis-azure-run-packages.md).

### <a name="run-packages-in-an-azure-data-factory-pipeline"></a>Ausführen von Paketen in einer Azure Data Factory-Pipeline

Um ein SSIS-Paket in einer Azure Data Factory-Pipeline auszuführen, verwenden Sie die Aktivität „SSIS-Paket ausführen“. Weitere Informationen finden Sie unter [Ausführen eines SSIS-Pakets mit der Aktivität „SSIS-Paket ausführen“ in Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

Beim Ausführen eines Pakets in einer Data Factory-Pipeline mit der Aktivität „SSIS-Paket ausführen“ können Sie Werte an das Paket zur Runtime übergeben. Wenn Sie mindestens einen Runtimewert an Pakete übergeben möchten, müssen Sie mit SQL Server Management Studio (SSMS) SSIS-Ausführungsumgebungen in SSISDB erstellen. Erstellen Sie in jeder Umgebung Variablen, und weisen Sie Werte zu, die den Parametern für Ihre Projekte oder Pakete entsprechen. Konfigurieren Sie Ihre SSIS-Pakete in SSMS, um Ihren Projekt- oder Paketparametern diese Umgebungsvariablen zuzuordnen. Wenn Sie die Pakete in der Pipeline ausführen, können Sie zwischen Umgebungen wechseln, indem Sie auf der Registerkarte „Einstellungen“ der Benutzeroberfläche der Aktivität „SSIS-Paket ausführen“ verschiedene Umgebungspfade angeben. Weitere Informationen zu SSIS-Umgebungen finden Sie unter [Erstellen und Zuordnen einer Serverumgebung](../packages/deploy-integration-services-ssis-projects-and-packages.md#create-and-map-a-server-environment).

## <a name="monitor-packages"></a>Überwachen von Paketen

Sie können die folgenden Berichtsoptionen in SSMS verwenden, um ausgeführte Pakete zu überwachen.
-   Klicken Sie mit der rechten Maustaste auf **SSISDB**, und klicken Sie dann auf **Aktive Vorgänge**, um das Dialogfeld **Aktive Vorgänge** zu öffnen.
-   Wählen Sie im Objekt-Explorer ein Paket aus, klicken Sie mit der rechten Maustaste, und klicken Sie dann auf **Berichte** > **Standardberichte** > **Alle Ausführungen**.

Informationen zum Überwachen der Azure SSIS Integration Runtime finden Sie unter [Monitor the Azure-SSIS integration runtime (Überwachen der Azure SSIS Integration Runtime)](/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime).

## <a name="schedule-packages"></a>Planen von Paketen
Sie können eine Vielzahl von Tools verwenden, um die Ausführung von in Azure bereitgestellten Paketen zu planen. Weitere Informationen finden Sie unter [Planen der Ausführung von in Azure bereitgestellten SSIS-Paketen](ssis-azure-schedule-packages.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den ersten Schritten mit SSIS-Workloads in Azure finden Sie in folgenden Artikeln:
-   [Tutorial: Bereitstellen und Ausführen eines SSIS-Pakets in Azure](ssis-azure-deploy-run-monitor-tutorial.md)
-   [Bereitstellen der Azure-SSIS Integration Runtime in Azure Data Factory](/azure/data-factory/tutorial-deploy-ssis-packages-azure)