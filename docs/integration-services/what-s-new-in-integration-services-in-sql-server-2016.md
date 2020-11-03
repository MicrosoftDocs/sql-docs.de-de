---
description: Neuigkeiten in Integration Services in SQL Server 2016
title: Neuigkeiten in Integration Services in SQL Server 2016 | Microsoft-Dokumentation
ms.custom:
- SQL2016_New_Updated
ms.date: 09/28/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, what's new
- what's new [Integration Services]
ms.assetid: da6999c7-e5e3-4a59-a284-1da635995af1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a458d0bcf700341951d79c775fb06fde37585362
ms.sourcegitcommit: 894c1a23e922dc29b82c1d2c34c7b0ff28b38654
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2020
ms.locfileid: "92193733"
---
# <a name="what39s-new-in-integration-services-in-sql-server-2016"></a>Neuigkeiten in Integration Services in SQL Server 2016

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



In diesem Artikel werden Funktionen beschrieben, die in SQL Server 2016 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] hinzugefügt oder aktualisiert wurden. Es werden außerdem Funktionen erwähnt, die für SQL Server 2016 dem [Azure Feature Pack für SQL Server Integration Services (SSIS)](../integration-services/azure-feature-pack-for-integration-services-ssis.md) hinzugefügt oder in diesem aktualisiert wurden.  

## <a name="new-for-ssis-in-azure-data-factory"></a>SSIS-Neuerungen in Azure Data Factory

In der öffentlichen Vorschauversion von Azure Data Factory Version 2, die seit September 2017 zur Verfügung steht, ist nun Folgendes möglich:
-   Bereitstellen von Paketen in der SSIS-Katalogdatenbank (SSISDB) für Azure SQL-Datenbank
-   Ausführen von Paketen, die in Azure in Azure SSIS Integration Runtime bereitgestellt wurden. Hierbei handelt es sich um eine Komponente von Azure Data Factory Version 2.

Weitere Informationen finden Sie unter [Migration von SQL Server Integration Services-Workloads in die Cloud per Lift und Shift](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Diese neuen Funktionen benötigen SQL Server Data Tools (SSDT) Version 17.2 oder höher, nicht jedoch SQL Server 2017 oder SQL Server 2016. Wenn Sie Pakete in Azure bereitstellen, aktualisiert der Assistent für die Paketbereitstellung die Pakete immer auf das aktuelle Paketformat.

## <a name="2016-improvements-by-category"></a>SQL Server 2016: Verbesserungen nach Kategorie  
  
-   **Verwaltbarkeit**  
  
    -   Bessere Bereitstellung  
  
        -   [SSISDB-Upgrade-Assistent](#ssisdbupgrwiz)  
  
        -   [Unterstützung für Always On im SSIS-Katalog](#AlwaysOn)  
  
        -   [Inkrementelle Paketbereitstellung](#IncrementalDeployment)  
  
        -   [Unterstützung für Always Encrypted im SSIS-Katalog](#encrypted)  
  
    -   Besseres Debuggen  
  
        -   [Neue „ssis_logreader“-Rolle auf Datenbankebene im SSIS-Katalog](#LogReader)  
  
        -   [Neuer Protokolliergrad „RuntimeLineage“ im SSIS-Katalog](#RuntimeLineage)  
  
        -   [Neuer benutzerdefinierter Protokolliergrad im SSIS-Katalog](#CustomLogging)  
  
        -   [Spaltennamen für Fehler im Datenfluss](#ErrorColumn)  
  
        -   [Umfassendere Unterstützung der Fehlerspaltennamen](#getidstring)  
  
        -   [Unterstützung für den serverweiten Standardprotokolliergrad](#ServerLogLevel)  
  
        -   [Neue IDTSComponentMetaData130-Schnittstelle in der API](#CMD130)  
  
    -   Bessere Paketverwaltung  
  
        -   [Benutzerfreundlicheres Upgraden von Projekten](#ProjectUpgrade)  
  
        -   [Die Eigenschaft „AutoAdjustBufferSize“ berechnet automatisch die Puffergröße für den Datenfluss](#BufferSize)  
  
        -   [Wiederverwendbare Vorlagen der Ablaufsteuerung](#Templates)  
  
        -   [Neue Vorlagen, zu Teilen umbenannt](#Parts)  
  
-   **Konnektivität**  
  
    -   Erweiterte lokale Konnektivität  
  
        -   [Unterstützung für OData V4-Datenquellen](#ODatav4)  
  
        -   [Explizite Unterstützung für Excel 2013-Datenquellen](#Excel2013)  
  
        -   [Unterstützung für das Hadoop File System (HDFS)](#HDFS)  
  
        -   [Erweiterte Unterstützung für Hadoop und HDFS](#more_hadoop)  
  
        -   [Die Komponente HDFS File Destination (HDFS-Dateispeicherort) unterstützt nun das Dateiformat ORC](#hdfsORC)  
  
        -   [ODBC-Komponenten für SQL Server 2016 aktualisiert](#odbc2016)  
  
        -   [Explizite Unterstützung für Excel 2016-Datenquellen](#Excel2016)  
  
        -   [Veröffentlichung von Microsoft Connector für SAP BW für SQL Server 2016](#SAPBW)
        
        -   [Connectors, Version 4.0, für Oracle und Teradata veröffentlicht](#oracleteradata)
        
        -   [Connectors für Analytics Platform System (PDW) Appliance Update 5 veröffentlicht](#pdwau5)
  
    -   Erweiterte Konnektivität in der Cloud  
  
        -   Azure Storage-Connectors und Hive- und Pig-Tasks für HDInsight – [Azure Feature Pack für SSIS für SQL Server 2016 veröffentlicht](#AFP2016)
        
        -   [Veröffentlichung der Unterstützung für Onlineressourcen von Microsoft Dynamics in Service Pack 1](#dynamics)
        
        -   [Support für Azure Data Lake Store freigegeben](#datalakestore)
        
        -   [Unterstützung für Azure Synapse Analytics freigegeben](#sqldwupload)
  
-   **Nutzbarkeit und Produktivität**  
  
    -   Benutzerfreundlicheres Installieren  
  
        -   [Das Upgrade wird blockiert, wenn SSISDB zu einer Verfügbarkeitsgruppe gehört](#Upgrade)  
  
    -   Ansprechenderes Designerlebnis  
  
        -   [SSIS-Designer erstellt und verwaltet Pakete für SQL Server 2016, 2014 und 2012](#OneDesigner)  
  
        -   Zahlreiche Verbesserungen am Designer und Programmfehlerbehebungen  
  
    -   Bessere Verwaltungsfunktionen in SQL Server Management Studio
  
        -   [Verbesserte Leistung für SSIS-Katalogsichten](#CatViews)  
  
    -   Weitere Verbesserungen  
  
        -   [Die Balanced Data Distributor-Transformation ist jetzt Teil von SSIS](#BDDinbox)  
  
        -   [Data Feed Publishing-Komponenten sind jetzt Bestandteil von SSIS](#ComplexFeedinbox)  
  
        -   [Unterstützung für Azure Blob Storage im SQL Server-Import/Export-Assistenten](#AzureBlob)  
  
        -   [Veröffentlichung des Change Data Capture Designers für Oracle von Attunity und des dazugehörigen Diensts für SQL Server 2016](#CDCOracle)  
  
        -   [CDC-Komponenten für SQL Server 2016 aktualisiert](#cdc2016)  
  
        -   [Analysis Services-Task „DDL ausführen“ aktualisiert](#ASDDL)  
  
        -   [Analysis Services-Tasks unterstützen Tabellenmodelle](#ssasrc0)  
  
        -   [Unterstützung für integrierte R-Dienste](#builtinR)  
  
        -   [Umfangreiche Ausgabe der XML-Validierung im XML-Task](#ValidateXML)  
  
## <a name="manageability"></a>Verwaltbarkeit  

### <a name="better-deployment"></a>Bessere Bereitstellung

####  <a name="ssisdb-upgrade-wizard"></a><a name="ssisdbupgrwiz"></a> SSISDB-Upgrade-Assistent  
 Führen Sie den SSISDB-Upgrade-Assistenten aus, um die Datenbank SSIS-Katalogdatenbank (SSISDB) zu aktualisieren, falls diese älter ist als die aktuelle Version der SQL Server-Instanz. Dies tritt auf, wenn eine der folgenden Bedingungen zutrifft.  
  
-   Sie haben die Datenbank aus einer älteren Version von SQL Server wiederhergestellt.  
  
-   Sie haben die Datenbank vor der Aktualisierung der SQL Server-Instanz nicht aus einer Always On-Verfügbarkeitsgruppe entfernt. Dies verhindert die automatische Aktualisierung der Datenbank. Weitere Informationen finden Sie unter [Upgrading SSISDB in an availability group](../integration-services/catalog/ssis-catalog.md#Upgrade).  
  
 Weitere Informationen finden Sie unter [SSIS Catalog (SSISDB) (SSIS-Katalog (SSISDB))](../integration-services/catalog/ssis-catalog.md). 

####  <a name="support-for-always-on-in-the-ssis-catalog"></a><a name="AlwaysOn"></a> Unterstützung für Always On im SSIS-Katalog  
 Das Feature der Always On-Verfügbarkeitsgruppen ist eine Lösung für hohe Verfügbarkeit und Notfallwiederherstellung, die eine Alternative zur Datenbankspiegelung auf Unternehmensebene bietet. Eine Verfügbarkeitsgruppe unterstützt eine Failoverumgebung für einen diskreten Satz von Benutzerdatenbanken. Diese werden auch als Verfügbarkeitsdatenbanken bezeichnet, die zusammen ein Failover ausführen. Weitere Informationen finden Sie unter [AlwaysOn-Verfügbarkeitsgruppen](../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md).  
  
 In SQL Server 2016 stellt SSIS neue Funktionen zur Verfügung, mit denen Sie problemlos Bereitstellungen im zentralisierten SSIS-Katalog (d.h. in der SSISDB-Benutzerdatenbank) vornehmen können. Um Hochverfügbarkeit für die SSISDB-Datenbank und ihren Inhalt – Projekte, Pakete, Ausführungsprotokolle usw. – zu gewährleisten, können Sie die SSISDB-Datenbank wie jede andere Datenbank zu einer Always On-Verfügbarkeitsgruppe hinzufügen. Wenn ein Failover auftritt, übernimmt einer der sekundären Knoten automatisch die Rolle eines primären Knoten.  
  
 Eine umfassende Übersicht und eine ausführliche Anleitung für die Aktivierung von Always On für SSISDB finden Sie unter [SSIS Catalog (SSIS-Katalog)](../integration-services/catalog/ssis-catalog.md).  

####  <a name="incremental-package-deployment"></a><a name="IncrementalDeployment"></a> Inkrementelle Paketbereitstellung  
Mit der Funktion für inkrementelle Paketbereitstellung können Sie ein oder mehrere Pakete in einem vorhandenen oder neuen Projekt bereitstellen, ohne das gesamte Projekt bereitzustellen. Sie können Pakete schrittweise (inkrementell) mithilfe der folgenden Tools bereitstellen.  
  
-   Bereitstellungs-Assistent  
  
-   SQL Server Management Studio (verwendet den Bereitstellungs-Assistenten)  
  
-   SQL Server Data Tools (Visual Studio)(verwendet ebenfalls den Bereitstellungs-Assistenten)  
  
-   Gespeicherte Prozeduren  
  
-   Die API des Management Object Model (MOM)  
  
 Weitere Informationen finden Sie unter [Bereitstellen von SQL Server Integration Services-Projekten und Paketen (SSIS)](../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  

####  <a name="support-for-always-encrypted-in-the-ssis-catalog"></a><a name="encrypted"></a> Unterstützung für Always Encrypted im SSIS-Katalog  
 SSIS unterstützt bereits die Funktion Always Encrypted in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Weitere Informationen finden Sie im folgenden Blogbeitrag.  
  
-   [SSIS mit Always Encrypted](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-with-always-encrypted/ba-p/388272)  
  
-   [Suchtransformation mit Always Encrypted](https://techcommunity.microsoft.com/t5/sql-server-integration-services/lookup-transformation-with-always-encrypted/ba-p/388282)  

### <a name="better-debugging"></a>Besseres Debuggen

####  <a name="new-ssis_logreader-database-level-role-in-the-ssis-catalog"></a><a name="LogReader"></a> Neue „ssis_logreader“-Rolle auf Datenbankebene im SSIS-Katalog  
 In früheren Versionen des SSIS-Katalogs konnten nur Benutzer in der **ssis_admin** -Rolle auf die Anzeige von Protokollausgaben zugreifen. Mit der neuen **ssis_logreader** -Rolle auf Datenbankebene können Sie Benutzern, die keine Administratoren sind, Zugriffsberechtigungen auf die Anzeige von Protokollausgaben gewähren.  
  
 Es gibt auch die neue **ssis_monitor** -Rolle. Diese Rolle unterstützt Always On und dient dem SSIS-Katalog nur zur internen Verwendung.  

####  <a name="new-runtimelineage-logging-level-in-the-ssis-catalog"></a><a name="RuntimeLineage"></a> Neuer Protokolliergrad „RuntimeLineage“ im SSIS-Katalog  
 Der neue Protokolliergrad **RuntimeLineage** des SSIS-Katalogs sammelt die zur Nachverfolgung der Herkunftsinformationen im Datenfluss erforderlichen Daten. Sie können diese Herkunftsinformationen analysieren, um die Herkunftsbeziehung zwischen Tasks zu bestimmen. Unabhängige Softwareentwickler (ISVs) und Entwickler können mit diesen Informationen benutzerdefinierte Herkunftszuordnungstools erstellen. 

####  <a name="new-custom-logging-level-in-the-ssis-catalog"></a><a name="CustomLogging"></a> Neuer benutzerdefinierter Protokolliergrad im SSIS-Katalog  
 Vorgängerversionen des SSIS-Katalogs boten Ihnen für die Ausführung eines Pakets die Wahl zwischen vier Protokolliergraden: **None, Basic, Performance und Verbose**. SQL Server 2016 fügt den Protokolliergrad **RuntimeLineage** hinzu. Darüber hinaus können Sie jetzt zahlreiche benutzerdefinierte Protokolliergrade im SSIS-Katalog erstellen und speichern und den Standardprotokolliergrad für jede Paketausführung bestimmen. Wählen Sie für jeden benutzerdefinierten Protokolliergrad nur die Statistiken und Ereignisse aus, die Sie erfassen möchten. Optional können Sie den Ereigniskontext mit aufnehmen, um variable Werte, Verbindungszeichenfolgen und die Eigenschaften von Tasks anzeigen zu lassen. Weitere Informationen finden Sie unter [Aktivieren der Protokollierung für die Paketausführung auf dem SSIS-Server](../integration-services/performance/integration-services-ssis-logging.md#server_logging). 

####  <a name="column-names-for-errors-in-the-data-flow"></a><a name="ErrorColumn"></a> Spaltennamen für Fehler im Datenfluss  
 Wenn Sie die Zeilen im Datenfluss, die einen Fehler enthalten umleiten, enthält die Ausgabe einen numerischen Bezeichner für die Spalte, in der der Fehler aufgetreten ist, sondern zeigt den Namen der Spalte nicht. Der Name der fehlerhaften Spalte kann auf verschiedenen Wegen gesucht oder angezeigt werden.  
  
-   Wenn Sie die Protokollierung konfigurieren, wählen Sie das Ereignis **DiagnosticEx** für die Protokollierung. Dieses Ereignis schreibt eine Spaltenzuordnung für den Datenfluss in das Protokoll. Sie können den Spaltennamen dann in dieser Spaltenzuordnung nachschlagen, und zwar mithilfe des von einer Fehlerausgabe erfassten Spaltenbezeichners. Weitere Informationen finden Sie unter [Fehlerbehandlung in Daten](../integration-services/data-flow/error-handling-in-data.md).  
  
-   Im erweiterten Editor wird in den Eigenschaften einer Eingabe- oder Ausgabespalte einer Datenflusskomponente auch der Name der Upstreamspalte angezeigt.  
  
-   Fügen Sie den Daten-Viewer an eine Fehlerausgabe an, um die Namen der fehlerhaften Spalten anzuzeigen.  Der Daten-Viewer zeigt jetzt sowohl die Beschreibung des Fehlers und den Namen der fehlerhaften Spalte an.  
  
-   Rufen Sie in der Skriptkomponente oder einer benutzerdefinierten Datenflusskomponente die neue <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A> -Methode der „IDTSComponentMetadata100“-Schnittstelle auf.  
  
 Weitere Informationen zu dieser Verbesserung finden Sie im folgenden Blogbeitrag von SSIS-Entwickler Bo Fan: [Error Column Improvements for SSIS Data Flow](https://techcommunity.microsoft.com/t5/sql-server-integration-services/error-column-improvements-for-ssis-data-flow-updated-for-rc2/ba-p/388253)(Verbesserungen der Fehlerspalte für SSIS-Datenfluss).  
  
> [!NOTE]  
>  (Diese Unterstützung wurde in darauffolgenden Versionen erweitert. Weitere Informationen finden Sie unter [Umfassendere Unterstützung des Fehlerspaltennamens](#getidstring) und [Neue IDTSComponentMetaData130-Schnittstelle in der API](#CMD130).)  

####  <a name="expanded-support-for-error-column-names"></a><a name="getidstring"></a> Umfassendere Unterstützung der Fehlerspaltennamen  
 Das Ereignis **DiagnosticEx** protokolliert seit neuestem nicht mehr nur noch Spalteninformationen für Herkunftsspalten, sondern auch für alle Eingabe- und Ausgabespalten. Daher heißt die Ausgabe nicht mehr Pipeline-Herkunftszuordnung, sondern Pipeline-Spaltenzuordnung.  
  
 Die Methode „GetIdentificationStringByLineageID“ wurde in <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A>hinzugefügt oder aktualisiert wurden. Weitere Informationen finden Sie unter [Spaltennamen für Fehler im Datenfluss](#ErrorColumn).  
  
 Weitere Informationen zu dieser Änderung und zu den Verbesserungen der Fehlerspalte finden Sie im folgenden aktualisierten Blogbeitrag. [Error Column Improvements for SSIS Data Flow (Updated for CTP3.3)](https://techcommunity.microsoft.com/t5/sql-server-integration-services/error-column-improvements-for-ssis-data-flow-updated-for-rc2/ba-p/388253)  
  
> [!NOTE]  
>  (In RC0 wurde diese Methode zur neuen <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130> -Schnittstelle verschoben. Weitere Informationen finden Sie unter [Neue IDTSComponentMetaData130-Schnittstelle in der API](#CMD130).)  

####  <a name="support-for-server-wide-default-logging-level"></a><a name="ServerLogLevel"></a> Unterstützung für den serverweiten Standardprotokolliergrad  
 Sie können nun in SQL Server unter **Servereigenschaften** mithilfe der Eigenschaft **Serverweiter Protokolliergrad** einen Standardwert für den serverweiten Protokolliergrad festlegen. Sie können zwischen einem der integrierten Protokolliergrade (None, Standard, Verbose, Performance oder RuntimeLineage) oder einem vorhandenen benutzerdefinierten entscheiden. Der ausgewählte Protokolliergrad wird auf alle im SSIS-Katalog bereitgestellten Pakete angewendet. Er gilt standardmäßig auch für SQL Agent-Auftragsschritte, die ein SSIS-Paket ausführen.  

####  <a name="new-idtscomponentmetadata130-interface-in-the-api"></a><a name="CMD130"></a> Neue IDTSComponentMetaData130-Schnittstelle in der API  
 Der neue Protokolliergrad <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130> -Schnittstelle fügt in SQL Server 2016 der vorhandenen <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> -Schnittstelle neue Funktionalität hinzu, insbesondere der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A> -Methode. (Die **GetIdentificationStringByID** -Methode wird von der <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> -Schnittstelle zur neuen Schnittstelle verschoben.)Es gibt auch die neuen Schnittstellen <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInputColumn130> und <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn130> , die beide die **LineageIdentificationString** -Eigenschaft unterstützen. Weitere Informationen finden Sie unter [Spaltennamen für Fehler im Datenfluss](#ErrorColumn).  

### <a name="better-package-management"></a>Bessere Paketverwaltung

####  <a name="improved-experience-for-project-upgrade"></a><a name="ProjectUpgrade"></a> Benutzerfreundlicheres Upgraden von Projekten  
 Wenn Sie SSIS-Projekte aus früheren Versionen auf die aktuelle Version upgraden, funktionieren die auf Projektebene ausgeführten Verbindungs-Manager weiterhin wie erwartet, und das Paketlayout und die Anmerkungen werden beibehalten.  

####  <a name="autoadjustbuffersize-property-automatically-calculates-buffer-size-for-data-flow"></a><a name="BufferSize"></a> Die Eigenschaft „AutoAdjustBufferSize“ berechnet automatisch die Puffergröße für den Datenfluss  
 Wenn Sie den Wert der Eigenschaft **AutoAdjustBufferSize** auf **true** festlegen, berechnet die Datenfluss-Engine automatisch die Puffergröße für den Datenfluss. Weitere Informationen finden Sie unter [Data Flow Performance Features](../integration-services/data-flow/data-flow-performance-features.md).  

####  <a name="reusable-control-flow-templates"></a><a name="Templates"></a> Wiederverwendbare Vorlagen der Ablaufsteuerung  
 Sie können nun einen häufig verwendeten Ablaufsteuerungstask oder Container in einer eigenständigen Vorlagendatei speichern. Mithilfe Ablaufsteuerungsvorlagen kann diese Datei in einem oder mehreren Paketen in einem Projekt mehrmalig wiederverwendet werden. Die Wiederverwendbarkeit erleichtert das Design und die Verwaltung von SSIS-Paketen. Weitere Informationen finden Sie unter [Wiederverwenden der Ablaufsteuerung für Pakete mithilfe von Ablaufsteuerungs-Paketteilen](../integration-services/reuse-control-flow-across-packages-by-using-control-flow-package-parts.md).  

####  <a name="new-templates-renamed-as-parts"></a><a name="Parts"></a> Neue Vorlagen, zu Teilen umbenannt  
 Die neuen wiederverwendbaren Vorlagen zur Ablaufsteuerung, die in der CTP-Version 3.0 veröffentlicht wurden, wurden zu Teilen der Ablaufsteuerung oder zu Paketteilen umbenannt. Weitere Informationen zu diesem Feature finden Sie unter [Wiederverwenden der Ablaufsteuerung für Pakete mithilfe von Ablaufsteuerungs-Paketteilen](../integration-services/reuse-control-flow-across-packages-by-using-control-flow-package-parts.md).  

## <a name="connectivity"></a>Konnektivität  

### <a name="expanded-connectivity-on-premises"></a>Erweiterte lokale Konnektivität

####  <a name="support-for-odata-v4-data-sources"></a><a name="ODatav4"></a> Unterstützung für OData V4-Datenquellen  
 Die OData-Quelle und der OData-Verbindungs-Manager unterstützen seit neuem OData V3- und V4-Protokolle.  
  
-   Für OData V3-Protokolle unterstützt die Komponente das ATOM- und das JSON-Datenformat.  
  
-   Für OData V4-Protokolle unterstützt die Komponente das ATOM- und das JSON-Datenformat.  
  
 Weitere Informationen finden Sie unter [OData Source](../integration-services/data-flow/odata-source.md).  

####  <a name="explicit-support-for-excel-2013-data-sources"></a><a name="Excel2013"></a> Explizite Unterstützung für Excel 2013-Datenquellen  
 Der Excel-Verbindungs-Manager, die Excel-Quelle und das Excel-Ziel sowie der SQL Server-Import/Export-Assistent bieten jetzt explizite Unterstützung für Excel 2013-Datenquellen. 

####  <a name="support-for-the-hadoop-file-system-hdfs"></a><a name="HDFS"></a> Unterstützung für das Hadoop File System (HDFS)  
 Die Unterstützung für das Hadoop File System enthält Verbindungs-Manager für die Verbindung mit Hadoop-Clustern und Tasks für allgemeine HDFS-Vorgänge. Weitere Informationen finden Sie unter [Hadoop- und HDFS-Unterstützung in Integration Services &#40;SSIS&#41;](../integration-services/hadoop-and-hdfs-support-in-integration-services-ssis.md).  

####  <a name="expanded-support-for-hadoop-and-hdfs"></a><a name="more_hadoop"></a> Erweiterte Unterstützung für Hadoop und HDFS  
  
-   Der Hadoop-Verbindungs-Manager unterstützt jetzt die Standard- und Kerberos-Authentifizierung. Weitere Informationen finden Sie unter [Hadoop-Verbindungs-Manager](../integration-services/connection-manager/hadoop-connection-manager.md).  
  
-   Die Komponenten HDFS File Source (HDFS-Dateiquelle) und HDFS File Destination (HDFS-Dateispeicherort) unterstützen nun sowohl das Text- als auch das Avro-Format. Weitere Informationen finden Sie unter  [HDFS File Source](../integration-services/data-flow/hdfs-file-source.md) und  [HDFS File Destination](../integration-services/data-flow/hdfs-file-destination.md).  
  
-   Der Task „Hadoop-Dateisystem“ unterstützt neuerdings zusätzlich zu den Optionen „CopyToHadoop“ und „CopyFromHadoop“ auch „CopyWithinHadoop“. Weitere Informationen finden Sie unter [Hadoop File System Task](../integration-services/control-flow/hadoop-file-system-task.md).  

####  <a name="hdfs-file-destination-now-supports-orc-file-format"></a><a name="hdfsORC"></a> Die Komponente HDFS File Destination (HDFS-Dateispeicherort) unterstützt nun das Dateiformat ORC  
 Die Komponente „HDFS-Dateiziel“ unterstützt nun zusätzlich zu Text und Avro das Dateiformat ORC. (Die Komponente HDFS File Source (HDFS-Dateiquelle) unterstützt nur Text und Avro.) Weitere Informationen zu dieser Komponente finden Sie unter [HDFS File Destination](../integration-services/data-flow/hdfs-file-destination.md).  

####  <a name="odbc-components-updated-for-sql-server-2016"></a><a name="odbc2016"></a> ODBC-Komponenten für SQL Server 2016 aktualisiert  
 Die Quell- und Zielkomponenten von ODBC wurden aktualisiert und sind nun vollständig mit SQL Server 2016 kompatibel. Es wurden keine neuen Funktionen hinzugefügt, und das Verhalten hat sich nicht verändert.  

####  <a name="explicit-support-for-excel-2016-data-sources"></a><a name="Excel2016"></a> Explizite Unterstützung für Excel 2016-Datenquellen  
 Der Excel-Verbindungs-Manager, die Excel-Quelle und das Excel-Ziel unterstützen jetzt explizit Excel 2016-Datenquellen.  

####  <a name="connector-for-sap-bw-for-sql-server-2016-released"></a><a name="SAPBW"></a> Veröffentlichung von Microsoft Connector für SAP BW für SQL Server 2016  
 Der MicrosoftÂ® Connector für SAP BW für Microsoft SQL ServerÂ® 2016 wurde als Teil des SQL Server 2016 Feature Pack veröffentlicht. Die Komponenten des Feature Packs können Sie unter [MicrosoftÂ® SQL ServerÂ® 2016 Feature Pack](https://go.microsoft.com/fwlink/?LinkID=746297)herunterladen.
 
#### <a name="connectors-v40-for-oracle-and-teradata-released"></a><a name="oracleteradata"></a> Connectors, Version 4.0, für Oracle und Teradata veröffentlicht
Die Microsoft-Connectors, Version 4.0, für Oracle und Teradata wurden veröffentlicht. Die Connectors können unter [Microsoft Connectors v4.0 für Oracle and Teradata](https://www.microsoft.com/download/details.aspx?id=52950)heruntergeladen werden.

### <a name="connectors-for-analytics-platform-system-pdw-appliance-update-5-released"></a><a name="pdwau5"></a> Connectors für Analytics Platform System (PDW) Appliance Update 5 veröffentlicht
Die Zieladapter zum Laden von Daten in PDW mit AU5 wurden veröffentlicht. Die Adapter können unter [Analytics Platform System Appliance Update 5 Documentation and Client Tools](https://www.microsoft.com/download/details.aspx?id=51610)heruntergeladen werden.

### <a name="expanded-connectivity-to-the-cloud"></a>Erweiterte Konnektivität in der Cloud

####  <a name="azure-feature-pack-for-ssis-released-for-sql-server-2016"></a><a name="AFP2016"></a> Azure Feature Pack für SSIS für SQL Server 2016 veröffentlicht  
 Das Azure Feature Pack für Integration Services wurde für SQL Server 2016 veröffentlicht. Das Feature Pack enthält Verbindungs-Manager für Verbindungen zu Azure-Datenquellen und Tasks für allgemeine Azure-Vorgänge. Weitere Informationen finden Sie unter [Azure Feature Pack für Integration Services &#40;SSIS&#41;](../integration-services/azure-feature-pack-for-integration-services-ssis.md).  

#### <a name="support-for-microsoft-dynamics-online-resources-released-in-service-pack-1"></a><a name="dynamics"></a> Veröffentlichung der Unterstützung für Onlineressourcen von Microsoft Dynamics in Service Pack 1

Wenn SQL Server 2016 Service Pack 1 installiert ist, unterstützen die OData-Quelle und der OData-Verbindungs-Manager jetzt Verbindungen mit den OData-Feeds von Microsoft Dynamics AX Online und Microsoft Dynamics CRM Online.

#### <a name="support-for-azure-data-lake-store-released"></a><a name="datalakestore"></a> Support für Azure Data Lake Store freigegeben

Die neueste Version von Azure Feature Pack enthält einen Verbindungs-Manager, eine Quelle und ein Ziel, um Daten zu und von Azure Data Lake Store zu verschieben. Weitere Informationen finden Sie unter [Azure Feature Pack für Integration Services &#40;SSIS&#41;](../integration-services/azure-feature-pack-for-integration-services-ssis.md).

#### <a name="support-for-azure-synapse-analytics-released"></a><a name="sqldwupload"></a>Unterstützung für Azure Synapse Analytics freigegeben

Die neueste Version von Azure Feature Pack enthält den Azure SQL DW Uploadtask, um SQL Data Warehouse mit Daten aufzufüllen. Weitere Informationen finden Sie unter [Azure Feature Pack für Integration Services &#40;SSIS&#41;](../integration-services/azure-feature-pack-for-integration-services-ssis.md).

## <a name="usability-and-productivity"></a>Nutzbarkeit und Produktivität  
 
### <a name="better-install-experience"></a>Benutzerfreundlicheres Installieren

####  <a name="upgrade-blocked-when-ssisdb-belongs-to-an-availability-group"></a><a name="Upgrade"></a> Das Upgrade wird blockiert, wenn SSISDB zu einer Verfügbarkeitsgruppe gehört  
 Wenn die SSIS-Katalogdatenbank (SSISDB) zu einer Always On-Verfügbarkeitsgruppe gehört, müssen Sie die SSISDB aus der Verfügbarkeitsgruppe entfernen, SQL Server upgraden und die SSISDB erneut zur Verfügbarkeitsgruppe hinzufügen. Weitere Informationen finden Sie unter [Upgrading SSISDB in an availability group](../integration-services/catalog/ssis-catalog.md#Upgrade).  

### <a name="better-design-experience"></a>Ansprechenderes Designerlebnis

####  <a name="multi-targeting-and-multi-version-support-in-ssis-designer"></a><a name="OneDesigner"></a> Festlegung mehrerer Zielversionen und Unterstützung mehrerer Versionen im SSIS-Designer  
 Sie können den SSIS-Designer in den SQL Server Data Tools (SSDT) nun in Visual Studio 2015 verwenden, um Pakete zu erstellen, zu verwalten und auszuführen, die auf SQL Server 2016, 2014 oder 2012 ausgerichtet sind. Die neuesten SSDT können Sie unter [Download der neuesten SQL Server-Datatools](../ssdt/download-sql-server-data-tools-ssdt.md)herunterladen. 

 Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf ein Integration Services-Projekt, und wählen Sie **Eigenschaften** aus, um die Eigenschaftsseiten für das Projekt zu öffnen. Klicken Sie in der Registerkarte **Allgemein** in den **Konfigurationseigenschaften** auf die Eigenschaft **TargetServerVersion** , und wählen Sie dann SQL Server 2016, 2014 oder 2012 aus.  
   
 ![TargetServerVersion-Eigenschaft im Dialogfeld „Projekteigenschaften“](../integration-services/media/targetserverversion2.png "TargetServerVersion-Eigenschaft im Dialogfeld „Projekteigenschaften“")  

> [!IMPORTANT]
> Wenn Sie benutzerdefinierte Erweiterungen für SSIS entwickeln, siehe [Unterstützung der Festlegung von Zielversionen in Ihren benutzerdefinierten Komponenten](../integration-services/extending-packages-custom-objects/support-multi-targeting-in-your-custom-components.md) und den Blogbeitrag [Getting your SSIS custom extensions to be supported by the multi-version support of SSDT 2015 for SQL Server 2016](https://blogs.msdn.microsoft.com/ssis/2016/04/19/getting-your-ssis-custom-extensions-to-be-supported-by-the-multi-version-support-of-ssdt-2015-for-sql-server-2016/)(Unterstützung benutzerdefinierter SSIS-Erweiterungen dank der Unterstützung mehrerer Versionsn von SSDT für SQL Server 2016).  

### <a name="better-management-experience-in-sql-server-management-studio"></a>Bessere Verwaltungsfunktionen in SQL Server Management Studio

####  <a name="improved-performance-for-ssis-catalog-views"></a><a name="CatViews"></a> Verbesserte Leistung für SSIS-Katalogsichten  
 Der Großteil der SSIS-Katalogsichten funktionieren seit neuestem besser, wenn sie nicht von einem Mitglied der Rolle „ssis_admin“ausgeführt werden.  

### <a name="other-enhancements"></a>Weitere Verbesserungen

####  <a name="balanced-data-distributor-transformation-is-now-part-of-ssis"></a><a name="BDDinbox"></a> Die Balanced Data Distributor-Transformation ist jetzt Teil von SSIS  
 Die Balanced Data Distributor-Transformation, die in früheren Versionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]einen separaten Download erforderte, wird jetzt bei der Installation [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]automatisch mitgeliefert. Weitere Informationen finden Sie unter [Balanced Data Distributor Transformation](../integration-services/data-flow/transformations/balanced-data-distributor-transformation.md).  
  
####  <a name="data-feed-publishing-components-are-now-part-of-ssis"></a><a name="ComplexFeedinbox"></a> Data Feed Publishing-Komponenten sind jetzt Bestandteil von SSIS  
 Die Data Feed Publishing-Komponenten, die in früheren Versionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]einen separaten Download erforderten, werden jetzt bei der Installation [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]automatisch mitgeliefert. Weitere Informationen finden Sie unter [Data Streaming Destination](../integration-services/data-flow/data-streaming-destination.md).  

####  <a name="support-for-azure-blob-storage-in-the-sql-server-import-and-export-wizard"></a><a name="AzureBlob"></a> Unterstützung für Azure Blob Storage im SQL Server-Import/Export-Assistenten  
 Der SQL Server-Import/Export-Assistent kann neuerdings Daten aus Azure Blob Storage importieren und auch dort speichern. Weitere Informationen finden Sie unter [Datenquelle wählen &#40;SQL Server-Import/Export-Assistent&#41;](../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md) und [Ziel wählen &#40;SQL Server-Import/Export-Assistent&#41;](../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md). 

####  <a name="change-data-capture-designer-and-service-for-oracle-for-microsoft-sql-server-2016-released"></a><a name="CDCOracle"></a> Veröffentlichung des Change Data Capture Designers für Oracle von Attunity und des dazugehörigen Diensts für SQL Server 2016  
 MicrosoftÂ® Change Data Capture-Designer und -Dienst für Oracle von Attunity für Microsoft SQL ServerÂ® 2016 wurde als Teil des SQL Server 2016 Feature Pack veröffentlicht.  Diese Komponenten unterstützen jetzt Oracle 12c in der klassischen Installation. (Eine mehrinstanzenfähige Installation wird nicht unterstützt.) Die Komponenten des Feature Packs können Sie unter [MicrosoftÂ® SQL ServerÂ® 2016 Feature Pack](https://go.microsoft.com/fwlink/?LinkID=746297)herunterladen.  
  
####  <a name="cdc-components-updated-for-sql-server-2016"></a><a name="cdc2016"></a> CDC-Komponenten für SQL Server 2016 aktualisiert  
 Die Komponenten CDC-Steuerungstask (Change Data Capture), CDC-Quelle und CDC-Splittertransformation wurden aktualisiert, sodass sie vollständig mit SQL Server 2016 kompatibel sind. Es wurden keine neuen Funktionen hinzugefügt, und das Verhalten hat sich nicht verändert.  
  
####  <a name="analysis-services-execute-ddl-task-updated"></a><a name="ASDDL"></a> Analysis Services-Task „DDL ausführen“ aktualisiert  
 Der Analysis Services-Task „DDL ausführen“ akzeptiert nun auch Befehle der Skriptsprache für tabellarische Modelle.

####  <a name="analysis-services-tasks-support-tabular-models"></a><a name="ssasrc0"></a> Analysis Services-Tasks unterstützen Tabellenmodelle  
 Sie können nun alle SSIS-Tasks und Ziele verwenden, die SQL Server Analysis Services (SSAS) mit SQL Server 2016-Tabellenmodellen unterstützen. Die SSIS-Tasks wurden aktualisiert, sodass sie Tabellenobjekte anstelle von mehrdimensionalen Objekten darstellen. Wenn Sie beispielsweise Objekte zur Verarbeitung auswählen, erkennt der Verarbeitungstask automatisch, dass es sich um ein Tabellenmodell handelt, und zeigt eine Auflistung tabellarischer Objekte anstelle von Measuregruppen und Dimensionen an. Der Speicherort der Partitionsverarbeitung zeigt nun auch tabellarische Objekte an und unterstützt die Übertragung von Daten in eine Partition.  
  
 Das Speicherort für die Dimensionsverarbeitung funktioniert nicht für tabellarische Modelle mit dem Kompatibilitätsgrad „SQL Server 2016“.  Für die Verarbeitung von Tabellen benötigen Sie lediglich den Analysis Services-Verarbeitungstask und das Ziel der Partitionsverarbeitung. 

####  <a name="support-for-built-in-r-services"></a><a name="builtinR"></a> Unterstützung für integrierte R-Dienste  
 SSIS unterstützt bereits die in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]integrierten R-Dienste. Mit SSIS können Sie nicht nur Daten extrahieren und die Ausgabe von Analysen laden, sondern auch R-Modelle erstellen, ausführen und in regelmäßigen Abständen aufbewahren. Weitere Informationen finden Sie im folgenden Blogbeitrag. [Operationalize your machine learning project using SQL Server 2016 SSIS and R Services](https://techcommunity.microsoft.com/t5/sql-server-integration-services/operationalize-your-machine-learning-project-using-sql-server/ba-p/388296)(Operationalisieren Ihres Machine Learning-Projekts mithilfe von SQL Server 2016 Integration Services (SSIS) und R Services). 

####  <a name="rich-xml-validation-output-in-the-xml-task"></a><a name="ValidateXML"></a> Umfangreiche Ausgabe der XML-Validierung im XML-Task  
 Validieren Sie XML-Dokumente und erhalten Sie eine umfangreiche Fehlerausgabe durch die Aktivierung der Eigenschaft **ValidationDetails** des XML-Tasks. Bevor die Eigenschaft **ValidationDetails** verfügbar war, gab die XML-Validierung durch den XML-Task nur „true“ oder „false“ als Ergebnis zurück, ohne Informationen zu Fehlern oder wo diese auftraten. Wenn Sie jetzt die Eigenschaft **ValidationDetails** auf „true“ festlegen, enthält die Ausgabedatei ausführliche Informationen zu jedem Fehler, einschließlich der Zeilennummer und der Position. Sie können diese Informationen verwenden, um Fehler in XML-Dokumenten zu verstehen, zu finden und zu beheben. Weitere Informationen finden Sie unter [Validate XML with the XML Task](../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
 [!INCLUDE[ssIS](../includes/ssis-md.md)] führte die Eigenschaft **ValidationDetails** im [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] Service Pack 2 ein. Diese neue Eigenschaft wurde zu diesem Zeitpunkt nicht angekündigt oder dokumentiert. Die Eigenschaft **ValidationDetails** ist auch in [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] und in [!INCLUDE[ssSQL15](../includes/sssql15-md.md)]verfügbar.   

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

## <a name="see-also"></a>Weitere Informationen  
 [Neues in SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md)   
 [Editionen und unterstützten Funktionen von SQL Server 2016](../sql-server/editions-and-components-of-sql-server-2016.md)