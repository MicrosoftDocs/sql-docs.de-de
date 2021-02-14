---
title: Herunterladen von SQL Server Data Tools (SSDT)
description: Informationen zu SQL Server Data Tools (SSDT) Hier erfahren Sie, wie Sie diese Tools zur Datenbankbereitstellung mit Visual Studio 2019 und Visual Studio 2017 installieren.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssdt
ms.topic: conceptual
keywords: SSDT installieren, SSDT herunterladen, aktuelle SSDT-Version
ms.assetid: b0fc4987-d260-4d0a-9dd1-98099835b361
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 02/20/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: cf088c407a95d32f3cfc7413b55ec41661126e78
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077236"
---
# <a name="download-sql-server-data-tools-ssdt-for-visual-studio"></a>Herunterladen von SQL Server Data Tools (SSDT) für Visual Studio 2015

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

**SQL Server Data Tools (SSDT)** ist ein modernes Entwicklungstool für relationale SQL Server-Datenbanken, Datenbanken in Azure SQL, IS-Pakete (Integration Services), AS-Datenmodelle (Analysis Services) und RS-Berichte (Reporting Services). Mit SSDT lassen sich Datenbanken und andere Inhaltstypen für SQL Server entwerfen und bereitstellen – und zwar ebenso einfach wie eine Anwendung in Visual Studio.

## <a name="ssdt-for-visual-studio-2019"></a>SSDT für Visual Studio 2019

### <a name="changes-in-ssdt-for-visual-studio-2019"></a>Änderungen in SSDT für Visual Studio 2019

Die wichtigsten SSDT-Funktionen zum Erstellen von Datenbankprojekten sind weiterhin in Visual Studio enthalten.

In Visual Studio 2019 wurde die erforderliche Funktionalität zur Unterstützung von Analysis Services-, Integration Services- und Reporting Services-Projekten ausschließlich in die jeweiligen Visual Studio-Erweiterungen (VSIX) verlagert.

> [!NOTE]
> Es gibt kein eigenständiges SSDT-Installationsprogramm für Visual Studio 2019.

### <a name="install-ssdt-with-visual-studio-2019"></a>Installieren von SSDT mit Visual Studio 2019

Wenn Visual Studio 2019 bereits installiert ist, können Sie [die Liste der Workloads bearbeiten](/visualstudio/install/install-visual-studio?preserve-view=true&view=vs-2019), um SSDT einzuschließen. Falls Sie Visual Studio 2019 nicht installiert haben, können Sie [Visual Studio 2019 Community](https://visualstudio.microsoft.com/downloads/) herunterladen und installieren.

Verwenden Sie den Visual Studio-Installer, um die installierten Visual Studio-Workloads so zu ändern, dass sie SSDT enthalten.

1. Starten Sie den Visual Studio-Installer. Hierzu können Sie im Windows-Startmenü nach „Installer“ suchen.

   ![Visual Studio-Installer im Windows-Startmenü für Visual Studio 2019](../ssdt/media/visual-studio-installer.png)

2. Wählen Sie im Installer die Edition von Visual Studio aus, der Sie SSDT hinzufügen möchten, und klicken Sie dann auf **Anpassen**.

3. Klicken Sie in der Workloadliste unter **Datenspeicherung und -verarbeitung** auf **SQL Server Data Tools**.

   ![Workload „Datenspeicherung und -verarbeitung“ für Visual Studio 2019](../ssdt/media/download-sql-server-data-tools-ssdt/data-workload-2019.png)

Für Analysis Services-, Integration Services- oder Reporting Services-Projekte können Sie die entsprechenden [Erweiterungen](/visualstudio/ide/finding-and-using-visual-studio-extensions) in Visual Studio über **Erweiterungen** > **Erweiterungen verwalten** oder über den [Marketplace](https://marketplace.visualstudio.com/search?term=services&target=VS&category=All%20categories&vsVersion=&sortBy=Relevance) installieren.

* [Analysis Services](https://marketplace.visualstudio.com/items?itemName=ProBITools.MicrosoftAnalysisServicesModelingProjects)
* [Integrationsdienste](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects)
* [Reporting Services](https://marketplace.visualstudio.com/items?itemName=ProBITools.MicrosoftReportProjectsforVisualStudio)

## <a name="ssdt-for-visual-studio-2017"></a>SSDT für Visual Studio 2017

### <a name="changes-in-ssdt-for-visual-studio-2017"></a>Änderungen in SSDT für Visual Studio 2017

Ab Visual Studio 2017 wurde die Funktionalität zum Erstellen von Datenbankprojekten in die Visual Studio-Installation integriert. Es ist nicht erforderlich, den eigenständigen SSDT-Installer zu installieren, um die Kernoberfläche von SSDT zu nutzen.

Zum Erstellen von Analysis Services-, Integration Services- oder Reporting Services-Projekten benötigen Sie jedoch weiterhin das eigenständige SSDT-Installationsprogramm.

### <a name="install-ssdt-with-visual-studio-2017"></a>Installieren von SSDT mit Visual Studio 2017

Wählen Sie für die Installation von SSDT während der [Visual Studio-Installation](/visualstudio/install/install-visual-studio) die Workload **Datenspeicherung und -verarbeitung** und anschließend **SQL Server Data Tools** aus.

Wenn Visual Studio bereits installiert ist, verwenden Sie den Visual Studio-Installer, um die installierten Workloads so zu ändern, dass sie SSDT enthalten.

1. Starten Sie den Visual Studio-Installer. Hierzu können Sie im Windows-Startmenü nach „Installer“ suchen.

   ![Visual Studio-Installer im Windows-Startmenü für Visual Studio 2017](../ssdt/media/visual-studio-installer.png)

2. Wählen Sie im Installer die Edition von Visual Studio aus, der Sie SSDT hinzufügen möchten, und klicken Sie dann auf **Anpassen**.

3. Klicken Sie in der Workloadliste unter **Datenspeicherung und -verarbeitung** auf **SQL Server Data Tools**.

   ![Workload „Datenspeicherung und -verarbeitung“ für Visual Studio 2017](../ssdt/media/download-sql-server-data-tools-ssdt/data-workload-2017.png)

### <a name="install-analysis-services-integration-services-and-reporting-services-tools"></a>Installieren der Tools Analysis Services, Integration Services und Reporting Services

Führen Sie den [eigenständigen SSDT-Installer](#ssdt-for-vs-2017-standalone-installer) aus, um Unterstützungspakete für Analysis Services-, Integration Services- und Reporting Services-Projekte zu installieren.

Der Installer listet verfügbare Visual Studio-Instanzen auf, denen die SSDT-Tools hinzugefügt werden können. Wenn Visual Studio nicht bereits installiert ist, wird SSDT durch Auswahl der Option **Neue SQL Server Data Tools-Instanz installieren** mit einer Mindestversion von Visual Studio installiert. Zur Erzielung optimaler Ergebnisse wird jedoch empfohlen, SSDT mit der [aktuellen Version von Visual Studio](https://www.visualstudio.com/downloads) zu verwenden.

![Auswählen von AS, IS, RS](../ssdt/media/download-sql-server-data-tools-ssdt/select-services.png)

## <a name="ssdt-for-vs-2017-standalone-installer"></a>SSDT für VS 2017 (eigenständiger Installer)

:::image type="icon" source="media/download.png" border="false"::: **[SSDT für Visual Studio 2017 (15.9.7) herunterladen](https://go.microsoft.com/fwlink/?linkid=2151759)**

> [!IMPORTANT]
> * Deinstallieren Sie vor der Installation von SSDT für Visual Studio 2017 (15.9.7) die Erweiterungen für *Analysis Services-* und *Reporting Services-Projekte*, wenn diese bereits installiert wurden, und schließen Sie alle VS-Instanzen. 
> * Die Power Query-Quelle für SQL Server 2017 wurde in ihrer bisherigen Form als bereits enthaltene Komponente entfernt. Wir haben die Power Query-Quelle für SQL Server 2017 und 2019 nun als separate Komponente angekündigt, die [hier](https://www.microsoft.com/download/details.aspx?id=100619) heruntergeladen werden kann.
> * Um Pakete mit Oracle- und Teradata-Connectors zu entwerfen, die auf eine frühere Version von SQL Server vor SQL Server 2019 abzielen, müssen Sie zusätzlich zum [Microsoft Oracle-Connector für SQL Server 2019](https://www.microsoft.com/download/details.aspx?id=58228) und [Microsoft Teradata-Connector für SQL Server 2019](https://www.microsoft.com/download/details.aspx?id=100599) auch die entsprechende Version des Microsoft-Connectors für Oracle und Teradata von Attunity installieren.
>    * [Microsoft Connector-Version 5.0 für Oracle von Attunity für SQL Server 2017](https://www.microsoft.com/download/details.aspx?id=55179)
>    * [Microsoft Connector-Version 4.0 für Oracle von Attunity für SQL Server 2016](https://www.microsoft.com/download/details.aspx?id=52950)
>    * [Microsoft Connector-Version 3.0 für Oracle von Attunity für SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=44582)
>    * [Microsoft Connector-Version 2.0 für Oracle von Attunity für SQL Server 2012](https://www.microsoft.com/download/details.aspx?id=29283)

### <a name="release-notes"></a>Versionsinformationen

Eine vollständige Liste der Änderungen finden Sie in den [Versionsanmerkungen für SQL Server Data Tools (SSDT)](release-notes-ssdt.md).

### <a name="system-requirements"></a>Systemanforderungen

SSDT für Visual Studio 2017 hat die gleichen [Systemanforderungen](/visualstudio/productinfo/vs2017-system-requirements-vs) wie Visual Studio.

### <a name="available-languages---ssdt-for-vs-2017"></a>Verfügbare Sprachen: SSDT für Visual Studio 2017

Diese Version von **SSDT für Visual Studio 2017** kann in folgenden Sprachen installiert werden:

* [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x804)
* [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x404)
* [Englisch (USA)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x409)
* [Französisch](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x40c)
* [Deutsch](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x407)
* [Italienisch](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x410)
* [Japanisch](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x411)
* [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x412)
* [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x416)
* [Russisch](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x419)
* [Spanisch](https://go.microsoft.com/fwlink/?linkid=2151759&clcid=0x40a)

### <a name="considerations-and-limitations"></a>Überlegungen und Einschränkungen

* Sie können die Community-Version nicht offline installieren.

* Zum Aktualisieren von SSDT müssen Sie denselben Pfad befolgen, der zum Installieren von SSDT verwendet wurde. Wenn Sie SSDT beispielsweise mithilfe der VSIX-Erweiterungen hinzugefügt haben, müssen Sie Upgrades über die VSIX-Erweiterungen durchführen. Wenn Sie SSDT separat installiert haben, müssen Sie mithilfe dieser Methode ein Upgrade durchführen.

## <a name="offline-install"></a>Offlineinstallation

Befolgen Sie die Anweisungen in diesem Abschnitt, um SSDT zu installieren, wenn Sie nicht mit dem Internet verbunden sind. Weitere Informationen finden Sie unter [Erstellen einer Netzwerkinstallation von Visual Studio 2017](/visualstudio/install/create-a-network-installation-of-visual-studio).

Führen Sie zunächst die folgenden Schritte aus, während Sie **online** sind:

1. [Installieren Sie den eigenständigen SSDT-Installer](#ssdt-for-vs-2017-standalone-installer).

2. [Laden Sie „vs_sql.exe“ herunter](https://aka.ms/vs/15/release/vs_sql.exe).

3. Führen Sie, während Sie online sind, einen der folgenden Befehle aus, um alle Dateien herunterzuladen, die für die Offlineinstallation erforderlich sind. Dabei ist die Verwendung der Option `--layout` wichtig. Mit ihr werden die tatsächlichen Dateien für die Offlineinstallation heruntergeladen. Ersetzen Sie `<filepath>` durch den tatsächlichen Layoutpfad, um die Dateien zu speichern.
   1. Übergeben Sie das folgende Gebietsschema für eine bestimmte Sprache: `vs_sql.exe --layout c:\<filepath> --lang en-us` (eine einzelne Sprache hat etwa 1 GB).
   1. Lassen Sie für alle Sprachen das Argument `--lang` aus: `vs_sql.exe --layout c:\<filepath>` (alle Sprachen haben etwa 3,9 GB).

Nach Abschluss der vorherigen Schritte sind folgende Schritte **offline** möglich:

1. Führen Sie `vs_setup.exe --NoWeb` aus, um die Visual Studio 2017-Shell sowie das SQL Server-Datenprojekt zu installieren.

2. Führen Sie `SSDT-Setup-ENU.exe /install` über dem Layoutordner aus, und wählen Sie SSIS/SSRS/SSAS aus.
   a. Führen Sie alternativ für eine unbeaufsichtigte Installation `SSDT-Setup-ENU.exe /INSTALLALL[:vsinstances] /passive` aus.

Führen Sie für verfügbare Optionen `SSDT-Setup-ENU.exe /help` aus.

> [!NOTE]
> Wenn Sie eine Vollversion von Visual Studio 2017 verwenden, erstellen Sie offline einen Ordner nur für SSDT, und führen Sie über diesen `SSDT-Setup-ENU.exe` aus. (fügen Sie SSDT nicht einem anderen Offlinelayout für Visual Studio 2017 hinzu). Wenn Sie das SSDT-Layout einem bereits vorhandenen Offlinelayout für Visual Studio hinzufügen, werden die benötigten Runtimekomponenten (EXE-Dateien) nicht erstellt.

## <a name="supported-sql-versions"></a>Unterstützte SQL-Versionen

|Projektvorlagen|Unterstützte SQL-Plattformen|
|-------------------|--------------------|
|Relationale Datenbanken| SQL Server 2005\* – SQL Server 2017<br> (Verwenden Sie SSDT 17.x oder SSDT für Visual Studio 2017, um eine Verbindung mit [SQL Server für Linux](../linux/sql-server-linux-overview.md) herzustellen)<br /><br />Azure SQL-Datenbank<br /><br />Azure Synapse Analytics (unterstützt nur Abfragen, Datenbankprojekte werden noch nicht unterstützt)<br /><br /> \* SQL Server 2005 wird nicht mehr unterstützt,<br /><br /> wechseln Sie zu einer offiziell unterstützten SQL-Version|
|Analysis Services-Modelle<br /><br />Reporting Services-Berichte | SQL Server 2008 – SQL Server 2017|
|Integration Services-Pakete| SQL Server 2012 – SQL Server 2019 |

## <a name="dacfx"></a>DacFX

SSDT für Visual Studio 2015 und Visual Studio 2017 verwendet DacFx 17.4.1: [Download Data-Tier Application Framework (DacFx) 17.4.1 (Herunterladen von Data-Tier Application Framework (DacFx) 17.4.1)](https://www.microsoft.com/download/details.aspx?id=56508).

## <a name="previous-versions"></a>Vorgängerversionen

In den [vorherigen Releases von SQL Server Data Tools (SSDT und SSDT-BI)](previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi.md) können Sie SSDT für Visual Studio 2015 oder eine frühere Version von SSDT herunterladen und installieren.

## <a name="see-also"></a>Weitere Informationen

* [SSDT MSDN-Forum](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=ssdt) 

* [SSDT-Team-Blog](/archive/blogs/ssdt/)

* [DACFx-API-Referenz](/previous-versions/sql/sql-server-2014/dn645454(v=sql.120))

* [Herunterladen von SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)

## <a name="next-steps"></a>Nächste Schritte

Gehen Sie nach der Installation von SSDT die folgenden Tutorials durch, um zu erfahren, wie Sie mithilfe von SSDT Datenbanken, Pakete, Datenmodelle und Berichte erstellen.

* [Projektorientierte Offlinedatenbankentwicklung](project-oriented-offline-database-development.md)

* [SSIS-Tutorial: Erstellen eines einfachen ETL-Pakets](../integration-services/ssis-how-to-create-an-etl-package.md)

* [Analysis Services-Tutorials](/analysis-services/analysis-services-tutorials-ssas)

* [Erstellen eines einfachen Tabellenberichts (SSRS-Lernprogramm)](../reporting-services/create-a-basic-table-report-ssrs-tutorial.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
