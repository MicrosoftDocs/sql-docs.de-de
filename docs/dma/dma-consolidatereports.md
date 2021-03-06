---
title: Bewerten eines Unternehmens und Konsolidieren von Bewertungsberichten mit Datenmigrations-Assistent
description: Erfahren Sie, wie Sie mit DMA ein Unternehmen bewerten und Bewertungsberichte konsolidieren, bevor Sie ein Upgrade SQL Server ausführen oder zu Azure SQL-Datenbank migrieren.
ms.date: 06/21/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: c82b5cf03b5db429903dd90fdf8a5fa5d6ec3772
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247503"
---
# <a name="assess-an-enterprise-and-consolidate-assessment-reports-with-dma"></a>Bewerten eines Unternehmens und Konsolidieren der Bewertungsberichte mit DMA

Die folgenden Schritt-für-Schritt-Anweisungen helfen Ihnen bei der Verwendung der Datenmigrations-Assistent, um eine erfolgreiche skalierte Bewertung für das Upgrade von lokalen SQL Server oder SQL Server, die auf Azure-VMS ausgeführt werden, oder für die Migration zu Azure SQL-Datenbank auszuführen.

[!INCLUDE [online-offline](../includes/azure-migrate-to-assess-sql-data-estate.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Legen Sie einen Tools-Computer in Ihrem Netzwerk fest, von dem DMA initiiert wird. Stellen Sie sicher, dass dieser Computer über eine Verbindung mit Ihren SQL Server Zielen verfügt.
- Sie müssen Folgendes herunterladen und installieren:
  - [Datenmigrations-Assistent](https://www.microsoft.com/download/details.aspx?id=53595) v 3.6 oder höher.
  - [PowerShell](https://aka.ms/wmf5download) Version 5.0 oder höher.
  - [.NET Framework](https://www.microsoft.com/download/details.aspx?id=30653) v 4.5 oder höher.
  - [SSMS](../ssms/download-sql-server-management-studio-ssms.md) 17,0 oder höher.
  - [Power BI Desktop](/power-bi/fundamentals/desktop-get-the-desktop).
  - [Azure PowerShell-Module](/powershell/azure/install-az-ps)
- Herunterladen und extrahieren:
  - Der [DMA-Bericht Power BI Vorlage](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/4/PowerBI-Reports.zip).
  - Das [loadwarehouse-Skript](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/3/LoadWarehouse1.zip).

## <a name="loading-the-powershell-modules"></a>Laden der PowerShell-Module

Wenn Sie die PowerShell-Module im PowerShell-Modul Verzeichnis speichern, können Sie die Module aufzurufen, ohne Sie explizit vor der Verwendung laden zu müssen.

Führen Sie die folgenden Schritte aus, um die Module zu laden:

1. Navigieren Sie zu c:\programme\windowspowershell\modules, und erstellen Sie dann einen Ordner mit dem Namen **datamigrationassistant**.
2. Öffnen Sie die [PowerShell-Module](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/161/1/PowerShell-Modules2.zip), und speichern Sie Sie in dem Ordner, den Sie erstellt haben.

      ![PowerShell-Module](../dma/media//dma-consolidatereports/dma-powershell-modules.png)

    Jeder Ordner enthält die zugehörige psm1-Datei, wie in der folgenden Abbildung dargestellt:

   ![PowerShell-Module psm1 Dateien](../dma/media//dma-consolidatereports/dma-powershell-modules-psm1-files.png)

   > [!NOTE]
   > Der Ordner und die psm1-Datei, die er enthält, müssen denselben Namen aufweisen.

   > [!IMPORTANT]
   > Möglicherweise müssen Sie die Blockierung der PowerShell-Dateien entsperren, nachdem Sie Sie im Verzeichnis WindowsPowerShell gespeichert haben, um sicherzustellen, dass die Module ordnungsgemäß geladen werden. Um die Blockierung einer PowerShell-Datei zu entsperren, klicken Sie mit der rechten Maustaste auf die Datei, wählen Sie **Eigenschaften** aus, aktivieren Sie das Textfeld **Sperre entsperren** , und wählen Sie dann **OK**

   ![psm1-Dateieigenschaften](../dma/media//dma-consolidatereports/dma-psm1-file-properties.png)

    PowerShell sollte diese Module jetzt automatisch laden, wenn eine neue PowerShell-Sitzung gestartet wird.

## <a name="create-an-inventory-of-sql-servers"></a><a name="create-inventory"></a> Erstellen eines Inventars von SQL-Servern

Bevor Sie das PowerShell-Skript zur Bewertung Ihrer SQL Server-Computer ausführen, müssen Sie eine Inventur der SQL Server-Computer erstellen, die Sie bewerten möchten.

Diese Inventur kann in einer von zwei Formen vorliegen:

- Excel-CSV-Datei
- SQL Server-Tabelle

### <a name="if-using-a-csv-file"></a>Bei Verwendung einer CSV-Datei

> [!IMPORTANT]
> Stellen Sie sicher, dass die Inventur Datei als durch Trennzeichen getrennte Datei (CSV) gespeichert wird.
>
> Legen Sie für Standard Instanzen den Instanznamen auf MSSQLSERVER fest.

Wenn Sie eine CSV-Datei verwenden, um die Daten zu importieren, stellen Sie sicher, dass nur zwei Spalten mit dem **dateninstanznamen** und dem Daten **Banknamen** vorhanden sind und dass die Spalten keine Header Zeilen enthalten.

 ![Inhalt der CSV-Datei](../dma/media//dma-consolidatereports/dma-csv-file-contents.png)

### <a name="if-using-a-sql-server-table"></a>Bei Verwendung einer SQL Server Tabelle

> [!IMPORTANT]
> Legen Sie für Standard Instanzen den Instanznamen auf MSSQLSERVER fest.

Erstellen Sie eine Datenbank mit dem Namen **estatueingeventory** und eine Tabelle namens **databaseingeventory**. Die Tabelle, in der diese Inventur Daten enthalten sind, kann beliebig viele Spalten enthalten, solange die folgenden vier Spalten vorhanden sind:

- ServerName
- InstanceName
- DatabaseName
- Bewermentflag

![Screenshot eines SQL Server Tabellen Inhalts, wenn Sie eine SQL Server Tabelle verwenden.](../dma/media//dma-consolidatereports/dma-sql-server-table-contents-database-inventory.png)

Wenn sich diese Datenbank nicht auf dem Tools-Computer befindet, stellen Sie sicher, dass der Computer Computer über eine Netzwerkverbindung zu dieser SQL Server Instanz verfügt.

Der Vorteil der Verwendung einer SQL Server Tabelle für eine CSV-Datei besteht darin, dass Sie mit der bewertungsflag-Spalte die Instanz/Datenbank steuern können, die für die Bewertung übernommen wird, was das Aufteilen von Bewertungen in kleinere Blöcke vereinfacht.  Sie können dann mehrere Bewertungen überspannen (Weitere Informationen finden Sie im Abschnitt zum Ausführen einer Bewertung weiter unten in diesem Artikel), der einfacher als die Verwaltung mehrerer CSV-Dateien ist.

Beachten Sie, dass eine Bewertung in Abhängigkeit von der Anzahl der Objekte und ihrer Komplexität sehr viel Zeit in Anspruch nehmen kann (Stunden +). Daher ist es ratsam, die Bewertung in verwaltbare Blöcke aufzuteilen.

### <a name="if-using-an-instance-inventory"></a>Bei Verwendung einer instanzinventur

Erstellen Sie eine Datenbank mit dem Namen **estatueingeventory** und eine Tabelle mit dem Namen **instanceingeventory**. Die Tabelle, in der diese Inventur Daten enthalten sind, kann beliebig viele Spalten enthalten, solange die folgenden vier Spalten vorhanden sind:

- ServerName
- InstanceName
- Port
- Bewermentflag

![Screenshot eines SQL Server Tabellen Inhalts bei Verwendung einer instanzinventur.](../dma/media//dma-consolidatereports/dma-sql-server-table-contents-instance-inventory.png)

## <a name="running-a-scaled-assessment"></a>Ausführen einer skalierten Bewertung

Nachdem Sie die PowerShell-Module in das Verzeichnis "modules" geladen und eine Inventur erstellt haben, müssen Sie eine skalierte Bewertung durchführen, indem Sie PowerShell öffnen und die dmadatacollector-Funktion ausführen.

  ![dmadatacollector-Funktions Auflistungen](../dma/media//dma-consolidatereports/dma-dmaDataCollector-function-listing.png)

Die der dmadatacollector-Funktion zugeordneten Parameter werden in der folgenden Tabelle beschrieben.

|Parameter  |BESCHREIBUNG |
|---------|---------|
|**getserverlistfrom** | Ihre Inventur. Mögliche Werte sind **SQLServer** und **CSV**.<br/>Weitere Informationen finden Sie unter [Erstellen eines Inventars von SQL-Servern](#create-inventory). |
|**csvpath** | Der Pfad zu Ihrer CSV-Inventur Datei.  Wird nur verwendet, wenn **getserverlistfrom** auf  **CSV** festgelegt ist. |
|**serverName** | Der SQL Server Instanzname des Inventars, wenn **SQLServer** im **getserverlistfrom** -Parameter verwendet wird. |
|**databaseName** | Die Datenbank, in der die Inventur Tabelle gehostet wird. |
|**nur ""** | Bitflag zum angeben, ob eine Liste von Instanzen zur Bewertung verwendet werden soll.  Wenn der Wert auf 0 festgelegt ist, wird die Tabelle databaseinventory verwendet, um die Liste der Bewertungs Ziele zu erstellen. |
|**AssessmentName** | Der Name der DMA-Bewertung. |
|**TargetPlatform** | Der Zieltyp der Bewertung, den Sie ausführen möchten.  Mögliche Werte sind **azuresqldatabase**, **managedsqlserver**, **SQLServer2012**, **SQLServer2014**, **SQLServer2016**, **SQLServerLinux2017**, **SQLServerWindows2017**,  **SqlServerWindows2019** und **SqlServerLinux2019**.  |
|**AuthenticationMethod** | Die Authentifizierungsmethode für das Herstellen einer Verbindung mit den SQL Server Zielen, die Sie bewerten möchten. Mögliche Werte sind **SQLAuth** und **windowsauth**. |
|**OutputLocation** | Das Verzeichnis, in dem die JSON-Bewertungs Ausgabedatei gespeichert werden soll. Abhängig von der Anzahl der Datenbanken, die bewertet werden, und der Anzahl der Objekte in den Datenbanken kann die Bewertung sehr lange dauern. Nachdem alle Bewertungen abgeschlossen sind, wird die Datei geschrieben. |

Wenn ein unerwarteter Fehler auftritt, wird das Befehlsfenster beendet, das von diesem Prozess initiiert wird.  Überprüfen Sie das Fehlerprotokoll, um zu ermitteln, warum es fehlschlug

  ![Speicherort des Fehlerprotokolls](../dma/media//dma-consolidatereports/dma-error-log-file-location.png)

## <a name="consuming-the-assessment-json-file"></a>Verwenden der JSON-Bewertungs Datei

Nachdem Ihre Bewertung abgeschlossen ist, können Sie die Daten für die Analyse in SQL Server importieren. Um die JSON-Bewertungs Datei zu nutzen, öffnen Sie PowerShell, und führen Sie die dmaprocessor-Funktion aus.

  ![dmaprocessor-Funktions Auflistung](../dma/media//dma-consolidatereports/dma-dmaProcessor-function-listing.png)

Die der dmaprocessor-Funktion zugeordneten Parameter werden in der folgenden Tabelle beschrieben.

|Parameter  |BESCHREIBUNG |
|---------|---------|
|**processto** | Der Speicherort, an dem die JSON-Datei verarbeitet wird. Mögliche Werte sind **SQLServer** und **azuresqldatabase**. |
|**serverName** | Die SQL Server Instanz, in die die Daten verarbeitet werden.  Wenn Sie für den **processto** -Parameter **azuresqldatabase** angeben, schließen Sie nur den SQL Server Namen ein (nicht include. Database.Windows.net). Sie werden zur Eingabe von zwei Anmeldungen aufgefordert, wenn Sie die Azure SQL-Datenbank als Ziel haben. die erste ist Ihre Azure-Mandanten-Anmelde Informationen, während die zweite die Administrator Anmeldung für den Azure-SQL Server ist. |
|**"Kreatedmareporting"** | Die Stagingdatenbank, die für die Verarbeitung der JSON-Datei erstellt werden soll.  Wenn die angegebene Datenbank bereits vorhanden ist und Sie diesen Parameter auf einen Wert festlegen, werden die Objekte nicht erstellt.  Dieser Parameter ist hilfreich, um ein einzelnes Objekt neu zu erstellen, das gelöscht wurde. |
|**"Kreatedatawarehouse"** | Erstellt die Data Warehouse, die vom Power BI Bericht verwendet werden. |
|**databaseName** | Der Name der dmareporting-Datenbank. |
|**Warehouse-Sender Name** | Name der Data Warehouse-Datenbank. |
|**jsondirectory** | Das Verzeichnis, das die JSON-Bewertungs Datei enthält.  Wenn mehrere JSON-Dateien im Verzeichnis vorhanden sind, werden Sie nacheinander verarbeitet. |

Die dmaprocessor-Funktion sollte nur einige Sekunden in Anspruch nehmen, um eine einzelne Datei zu verarbeiten.

## <a name="loading-the-data-warehouse"></a>Laden der Data Warehouse

Nachdem der dmaprocessor die Bearbeitung der Bewertungs Dateien abgeschlossen hat, werden die Daten in die dmareporting-Datenbank in der Report Data-Tabelle geladen. An diesem Punkt müssen Sie den Data Warehouse laden.

1. Verwenden Sie das loadwarehouse-Skript, um fehlende Werte in den Dimensionen aufzufüllen.

    Das Skript nimmt die Daten aus der Tabelle Report Data in der dmareporting-Datenbank und lädt Sie in das Warehouse.  Wenn während dieses Ladevorgangs Fehler auftreten, ist dies wahrscheinlich auf fehlende Einträge in den Dimensions Tabellen zurückzuführen.

2. Laden Sie die Data Warehouse.

  ![Loadwarehouse-Inhalte geladen](../dma/media//dma-consolidatereports/dma-load-warehouse-loaded.png)

## <a name="set-your-database-owners"></a>Festlegen der Datenbankbesitzer

Obwohl es nicht obligatorisch ist, den größten Nutzen aus den Berichten zu erzielen, wird empfohlen, dass Sie die Datenbankbesitzer in der **dimdbowner** -Dimension festlegen und dann **dbownerkey** in der **factassessment** -Tabelle aktualisieren.  Nach diesem Vorgang wird das aufteilen und Filtern des Power BI Berichts basierend auf bestimmten Daten Bank Besitzern ermöglicht.

Sie können auch das loadwarehouse-Skript verwenden, um die grundlegenden TSQL-Anweisungen zum Festlegen der Datenbankbesitzer bereitzustellen.

  ![Loadwarehouse-Einstellungs Besitzer](../dma/media//dma-consolidatereports/dma-load-warehouse-set-owners.png)

## <a name="dma-reports"></a>DMA-Berichte

1. Öffnen Sie die Power BI Vorlage DMA-Berichte in der Power BI Desktop.
2. Geben Sie Server Details an, die auf Ihre **dmawarehouse** -Datenbank verweisen, und wählen Sie dann **Laden** aus.

   ![DMA-Berichte Power BI Vorlage geladen](../dma/media//dma-consolidatereports/dma-reports-powerbi-template-loaded.png)

   Nachdem der Bericht die Daten aus der **dmawarehouse** -Datenbank aktualisiert hat, wird ein Bericht angezeigt, der dem folgenden ähnelt.

   ![Dmawarehouse-Berichtsansicht](../dma/media//dma-consolidatereports/dma-DMAWarehouse-report1.png)

   > [!TIP]
   > Wenn die erwarteten Daten nicht angezeigt werden, versuchen Sie, das aktive Lesezeichen zu ändern.  Weitere Informationen finden Sie in den Details im folgenden Abschnitt.

## <a name="working-with-dma-reports"></a>Arbeiten mit DMA-Berichten

Verwenden Sie zum Arbeiten mit DMA-Berichten Lesezeichen und Slicer, um nach folgenden Filtern zu filtern:

- Bewertungs Typen (Azure SQL-Datenbank, Azure SQL verwaltete Instanz SQL Server) 
- Instanzname
- Datenbankname
- Teamname

Um auf das Blatt Lesezeichen und Filter zuzugreifen, wählen Sie das Lesezeichen Filter auf der Hauptberichts Seite aus:

![DMA-Berichts Lesezeichen und-Filter](../dma/media//dma-consolidatereports/dma-report-bookmarks-filters.png)

Wenn Sie das Lesezeichen Filter auswählen, wird das folgende Blatt aktiviert:

![Blatt "DMA-Berichts Ansichten"](../dma/media//dma-consolidatereports/dma-report-views-blade.png)

Sie können Lesezeichen verwenden, um den Berichts Kontext zwischen folgenden zu wechseln:

- Azure SQL-Datenbank-cloudbewertungen
- Azure SQL verwaltete Instanz Cloud-Bewertungen
- Lokale Bewertungen

![Lesezeichen für DMA-Berichts Ansichten](../dma/media//dma-consolidatereports/dma-report-bookmarks1.png)

Um das Blatt "Filter" auszublenden, klicken Sie auf die Schaltfläche "zurück":

![DMA-Berichts Ansichten zurück-Schaltfläche](../dma/media//dma-consolidatereports/dma-report-bookmarks-back.png)

Unten links auf der Berichtsseite finden Sie eine Eingabeaufforderung, die anzeigt, ob ein Filter aktuell auf eines der folgenden Elemente angewendet wird:

- Factassessment – instanceName
- Factassessment – DatabaseName
- dimdbowner-dbowner

![Angewendete Aufforderung Filtern](../dma/media//dma-consolidatereports/dma-filter-applied-prompt.png)

> [!NOTE]
> Wenn Sie nur eine Bewertung der Azure SQL-Datenbank durchführen, werden nur cloudberichte aufgefüllt. Wenn Sie hingegen nur eine lokale Bewertung durchführen, werden nur lokale Berichte aufgefüllt. Wenn Sie jedoch sowohl eine Azure-als auch eine lokale Bewertung durchführen und dann beide Bewertungen in Ihr Warehouse laden, können Sie zwischen Cloud-Berichten und lokalen Berichten wechseln, indem Sie mit der STRG-Taste auf das zugehörige Symbol klicken.

## <a name="reports-visuals"></a>Berichte mit Visualisierungen

Die Details, die in den Power BI Berichten angezeigt werden, werden in den folgenden Abschnitten dargestellt.

### <a name="readiness-"></a>Bereit

  ![DMA-Bereitschaft in Prozent](../dma/media//dma-consolidatereports/dma-readiness-percentage.png)

Dieses visuelle Element wird basierend auf dem Auswahl Kontext aktualisiert (alles, Instanz, Datenbank [Vielfache von]).

### <a name="readiness-count"></a>Bereitschafts Anzahl

  ![DMA-Bereitschafts Anzahl](../dma/media//dma-consolidatereports/dma-readiness-count.png)

Diese Visualisierung zeigt die Anzahl von Datenbanken, die bereit sind, die Anzahl der Datenbanken zu migrieren, die noch nicht für die Migration bereit sind.

### <a name="readiness-bucket"></a>Bereitschafts Bucket

  ![DMA-Bereitschafts Bucket](../dma/media//dma-consolidatereports/dma-readiness-bucket.png)

Diese Visualisierung zeigt eine Aufschlüsselung der Datenbanken durch die folgenden Bereitschafts Buden:

- 100% bereit
- 75-99% bereit
- 50-75% bereit
- nicht bereit

### <a name="issues-word-cloud"></a>Ausgibt Word-Cloud

  ![DMA-Probleme wordcloud](../dma/media//dma-consolidatereports/dma-issues-word-cloud.png)

Dieses visuelle Element zeigt die Probleme, die derzeit im Auswahl Kontext auftreten (alles, Instanz, Database [Vielfache von]). Je größer das Wort auf dem Bildschirm angezeigt wird, desto größer ist die Anzahl der Probleme in dieser Kategorie. Wenn Sie mit dem Mauszeiger auf ein Wort zeigen, wird die Anzahl der in dieser Kategorie auftretenden Probleme angezeigt.

### <a name="database-readiness"></a>Daten Bank Bereitschaft

  ![Bericht zur DMA-Daten Bank Bereitschaft](../dma/media//dma-consolidatereports/dma-database-readiness-report.png)

Dieser Abschnitt ist der primäre Teil des Berichts, der die Bereitschaft einer Instanz-Database anzeigt. Dieser Bericht enthält eine Drilldownhierarchie von:

- InstanceDatabase
- ChangeCategory
- Titel
- ObjektType
- Impactedobjectname

 ![Bericht zur DMA-Daten Bank Bereitschaft, Drilldown](../dma/media//dma-consolidatereports/dma-database-readiness-report-drilldown.png)

Dieser Bericht dient auch als Filter Punkt zum Erstellen des Berichts für den Wiederherstellungs Plan.

Um einen Drilldown in den Bericht zum Wartungs Plan auszuführen, klicken Sie mit der rechten Maustaste auf einen Datenpunkt in diesem Diagramm, zeigen Sie auf **Drillthrough**, und wählen Sie Wiederherstellungs **Pläne**.

Mit diesem Task wird der Bericht des Wiederherstellungs Plans basierend auf dem Punkt, an dem Sie die Drillthrough-Option auswählen, auf die aktuelle Hierarchieebene gefiltert.

  ![Bericht "Übersicht über DMA-Daten Bank Bereitschaft" gefiltert](../dma/media//dma-consolidatereports/dma-database-readiness-report-drilldown-filtered.png)

  ![Bericht zu DMA-Wiederherstellungs Plänen](../dma/media//dma-consolidatereports/dma-remediation-plan-report.png)

Sie können den Bericht zum Wartungsplan auch eigenständig verwenden, um einen benutzerdefinierten Wiederherstellungs Plan mithilfe der Filter auf dem Blatt **Visualisierungen Filter** zu erstellen.

  ![Bericht Filteroptionen für den DMA-Wiederherstellungs Plan](../dma/media//dma-consolidatereports/dma-remediation-plan-report-filter-options.png)

### <a name="script-disclaimer"></a>Skript Ausschluss

*Die in diesem Artikel bereitgestellten Beispiel Skripts werden von keinem Microsoft-Standard Support Programm oder-Dienst unterstützt. Alle Skripts werden ohne jegliche Gewährleistung bereitgestellt. Microsoft lehnt alle impliziten Gewährleistungen ab, einschließlich, ohne Einschränkung, implizierter Gewährleistungen der Handels Üblichkeit oder Eignung für einen bestimmten Zweck. Das gesamte Risiko, das sich aus der Verwendung oder Leistung der Beispiel Skripts und der Dokumentation ergibt, bleibt bei Ihnen bestehen. In keinem Fall müssen Microsoft, seine Autoren oder andere Personen an der Erstellung beteiligt sein. die Produktion oder die Übermittlung der Skripts ist für jegliche Schäden haftbar (einschließlich, ohne Einschränkung, Schäden für den Verlust von Geschäfts Gewinn, Geschäfts Unterbrechung, Verlust von Geschäftsinformationen oder anderer peerarer Verlust), die sich aus der Verwendung oder der Unfähigkeit der Verwendung der Beispiel Skripts oder der Dokumentation ergeben, auch wenn Microsoft auf die Möglichkeit solcher Schäden hingewiesen wurde.  Aktivieren Sie die Berechtigung, bevor Sie diese Skripts für andere Sites/Depots/Blogs erneut veröffentlichen.*