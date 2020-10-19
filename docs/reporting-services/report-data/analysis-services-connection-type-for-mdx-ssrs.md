---
title: Analysis Services-Verbindungstyp für MDX | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über den Analysis Services-Verbindungstyp für MDX (SSRS), sodass Sie Metadaten zur Verwendung als Berichtsdaten abrufen können.
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: bd2e7148-3124-4e07-9734-22333127c3be
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 3f8b6fb8aa45357318c398773708ecd2c66dba30
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91935074"
---
# <a name="analysis-services-connection-type-for-mdx-ssrs"></a>Analysis Services-Verbindungstyp für MDX (SSRS)
  Wenn Sie Daten aus einem [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Cube in den Bericht einschließen möchten, benötigen Sie ein Dataset, das auf einer Berichtsdatenquelle des Typs „[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]“ basiert. Dieser integrierte Datenquellentyp basiert auf der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Datenerweiterung. Sie können Metadaten über Dimensionen, Hierarchien, Ebenen, Key Performance Indicators (KPIs), Measures und Attribute aus einem [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Cube abrufen, um sie als Berichtsdaten zu verwenden.  
  
 Diese Datenverarbeitungserweiterung unterstützt mehrwertige Parameter, Serveraggregate und getrennt von der Verbindungszeichenfolge verwaltete Anmeldeinformationen.  
  
 Verwenden Sie die Informationen in diesem Thema, um eine Datenquelle zu erstellen. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Hinzufügen und Prüfen einer Datenverbindung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="connection-string"></a><a name="Connection"></a> Verbindungszeichenfolge  
 Wenn Sie eine Verbindung mit einem [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Cube herstellen, stellen Sie eine Verbindung mit dem Datenbankobjekt in einer Analysis Services-Instanz auf einem Server her. Die Datenbank kann mehrere Cubes enthalten. Sie geben den Cube beim Erstellen der Abfrage im Abfrage-Designer an. Das folgenden Beispiel zeigt eine Verbindungszeichenfolge:  
  
```  
data source=<server name>;initial catalog=<database name>  
```  
  
 Weitere Beispiele für Verbindungszeichenfolgen finden Sie unter [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
  
##  <a name="credentials"></a><a name="Credentials"></a> Anmeldeinformationen  
 Anmeldeinformationen sind erforderlich, um Abfragen auszuführen und den Bericht lokal oder vom Berichtsserver aus in der Vorschau anzuzeigen.  
  
 Nachdem Sie den Bericht veröffentlicht haben, müssen Sie eventuell die Anmeldeinformationen für die Datenquelle ändern, sodass die Berechtigungen zum Abrufen der Daten beim Ausführen des Berichts auf dem Berichtsserver gültig sind.  
  
 Auf einem Berichterstellungsclient sind die folgenden Optionen zum Angeben von Anmeldeinformationen verfügbar:  
  
-   Aktueller Windows-Benutzer (auch bekannt als integrierte Sicherheit).  
  
-   Verwendung eines gespeicherten Benutzernamens und eines gespeicherten Kennworts.  
  
-   Aufforderung zur Eingabe der Anmeldeinformationen. Diese Option unterstützt nur die integrierte Windows-Sicherheit.  
  
-   Anmeldeinformationen sind nicht erforderlich. Zur Verwendung dieser Option müssen Sie zuvor das Konto für die unbeaufsichtigte Ausführung auf dem Berichtsserver konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren des Kontos für die unbeaufsichtigte Ausführung &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).
  
 Weitere Informationen finden Sie unter [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) oder [Angeben der Anmeldeinformationen und Verbindungsinformationen für Berichtsdatenquellen](specify-credential-and-connection-information-for-report-data-sources.md).  
  
  
##  <a name="queries"></a><a name="Query"></a> Abfragen  
 Nachdem Sie eine Datenverbindung mit einer [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Datenquelle hergestellt haben, erstellen Sie ein Dataset, und Sie definieren eine MDX-Abfrage (Multidimensional Expressions), die die aus dem Cube abzurufenden Daten angibt. Verwenden Sie den grafischen MDX-Abfrage-Designer, um die zugrunde liegenden Datenstrukturen der Datenquelle zu durchsuchen und auszuwählen.  
  
 Zum Angeben einer Abfrage stehen Ihnen folgende Methoden zur Auswahl:  
  
-   Erstellen Sie eine Abfrage interaktiv. Der Analysis Services-MDX-Abfrage-Designer unterstützt die folgenden Ansichten:  
  
    -   **Entwurfsansicht** Ziehen Sie Dimensionen, Elemente, Elementeigenschaften, Measures und KPIs aus dem Metadatenbrowser in den Bereich **Daten** , um eine MDX-Abfrage zu erstellen. Ziehen Sie berechnete Elemente aus dem Bereich „Berechnete Elemente“ in den Datenbereich, um zusätzliche Datasetfelder zu definieren.  
  
    -   **Abfrageansicht** Ziehen Sie Dimensionen, Elemente, Elementeigenschaften, Measures und KPIs aus dem Metadatenbrowser in den Bereich "Abfrage", um eine MDX-Abfrage zu erstellen. Sie können MDX-Text direkt im Abfragebereich bearbeiten. Ziehen Sie berechnete Elemente aus dem Bereich „Berechnete Elemente“ in den Abfragebereich, um zusätzliche Datasetfelder zu definieren.  
  
     Weitere Informationen finden Sie unter [Benutzeroberfläche des MDX-Abfrage-Designers für Analysis Services (Berichts-Generator)](../../reporting-services/report-data/analysis-services-mdx-query-designer-user-interface.md).  
  
-   Importieren Sie eine vorhandene MDX-Abfrage aus einem Bericht. Verwenden Sie die Schaltfläche **Abfrage importieren** , um eine RDL-Datei auszuwählen und eine Abfrage zu importieren. Sie können eine Abfrage aus einem Bericht importieren, der ein eingebettetes, auf einer [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Datenquelle beruhendes Dataset enthält. Das direkte Importieren einer MDX-Abfrage aus einer MDX-Datei wird nicht unterstützt.  
  
 Führen Sie die Abfrage zur Entwurfszeit aus, um ein Resultset anzuzeigen. Die Abfrageergebnisse werden automatisch als vereinfachtes Rowset abgerufen. Die Feldauflistung für ein Dataset wird mit den Spalten aus dem Resultset einer Abfrage aufgefüllt. Nachdem Sie die Abfrage erstellt haben, zeigen Sie die aus den Metadaten generierte Datasetfeldauflistung im Berichtsdatenbereich an. Bei der Ausführung des Berichts werden die tatsächlichen Daten aus der externen Datenquelle zurückgegeben.  
  
 Die [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Datenverarbeitungserweiterung unterstützt erweiterte Datasetfeldeigenschaften. Bei diesen Eigenschaften handelt es sich um Werte, die in der externen Datenquelle verfügbar sind, aber nicht im Berichtsdatenbereich angezeigt werden. Über die Sammlung [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Felder **können Sie in Ihrem Bericht erweiterte Feldeigenschaften verwenden, die von der** -Datenverarbeitungserweiterung unterstützt werden. Für Eigenschaften, die Werte für die Datenquelle besitzen, können Sie auf vordefinierte Eigenschaftswerte wie **FormattedValue**, **Color**oder **UniqueName**zugreifen. Weitere Informationen finden Sie unter [Erweiterte Feldeigenschaften für eine Analysis Services-Datenbank &#40;SSRS&#41;](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md).  
  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parameter  
 Erstellen Sie im Filterbereich des Abfrage-Designers einen Filter, und markieren Sie ihn als Parameter, um Abfrageparameter einzuschließen. Für jeden Filter wird automatisch ein Dataset erstellt, um die verfügbaren Werte bereitzustellen. Diese Datasets werden standardmäßig nicht im Berichtsdatenbereich angezeigt. Weitere Informationen finden Sie unter [Definieren von Parametern im MDX-Abfrage-Designer für Analysis Services (Berichts-Generator und SSRS)](../../reporting-services/report-data/define-parameters-in-the-mdx-query-designer-for-analysis-services.md) und [Anzeigen von ausgeblendeten Datasets für Parameterwerte für mehrdimensionale Daten (Berichts-Generator und SSRS)](../../reporting-services/report-data/show-hidden-datasets-for-parameter-values-multidimensional-data.md).  
  
 Alle Berichtsparameter sind standardmäßig vom Datentyp **Text**. Die Standardwerte müssen möglicherweise nach dem Erstellen der Berichtsparameter geändert werden. Weitere Informationen finden Sie unter [Berichtsparameter &#40;Berichts-Generator und Berichts-Designer&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)" basiert.  
  
  
##  <a name="remarks"></a><a name="Remarks"></a> Hinweise  
 Die Analysis Services-Datenerweiterung basiert auf dem XMLA-Protokoll (XML for Analysis). Resultsets von Cubes werden durch das XMLA-Protokoll als vereinfachtes Rowset abgerufen. Unregelmäßige Hierarchien werden nicht unterstützt. Weitere Informationen finden Sie unter [Unregelmäßige Hierarchien](/analysis-services/multidimensional-models/user-defined-hierarchies-ragged-hierarchies).  
  
 Sie können auch mit dem OLE DB-Datenquellentyp Daten aus einem [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Cube abrufen. Weitere Informationen finden Sie unter [OLE DB-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/ole-db-connection-type-ssrs.md).  
  
 Weitere Informationen zur Versionsunterstützung finden Sie unter [Von Reporting Services unterstützte Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
  
##  <a name="related-sections"></a><a name="Related"></a> Verwandte Abschnitte  
 Diese Abschnitte der Dokumentation enthalten umfassende grundlegende Informationen zu Berichtsdaten sowie Informationen zum Definieren, Anpassen und Verwenden der mit Daten zusammenhängenden Teile eines Berichts.  
  
 [Berichtsdatasets &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)  
 Bietet eine Übersicht über den Zugriff auf Daten für den Bericht.  
  
 [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
 Enthält Informationen zu Datenverbindungen und Datenquellen.  
  
 [Erstellen von Berichten zu eingebetteten und freigegebenen Datasets &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
 Enthält Informationen zu eingebetteten und freigegebenen Datasets.  
  
 [Datasetfelder-Sammlung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
 Enthält Informationen zur von der Abfrage generierten Datasetfeldauflistung.  
  
 [Erweiterte Feldeigenschaften für eine Analysis Services-Datenbank &#40;SSRS&#41;](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md)  
 Enthält Informationen zu zusätzlichen Feldern, die über den XMLA-Datenanbieter zur Verfügung stehen.  
  
 [Von Reporting Services unterstützte Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)  
 Enthält ausführliche Informationen zur Plattform- und Versionsunterstützung für die einzelnen Datenerweiterungen.  
  
  
## <a name="see-also"></a>Weitere Informationen  
 [Berichtsparameter &#40;Berichts-Generator und Berichts-Designer&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Filtern, Gruppieren und Sortieren von Daten &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Ausdrücke &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
