---
title: Hinzufügen von Daten aus externen Datenquellen | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie Daten aus externen Datenquellen zu Berichten hinzufügen und wie Berichte mit Technologien für den Datenzugriff funktionieren.
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
reviewer: ''
ms.custom: ''
ms.date: 03/17/2017
ms.openlocfilehash: bf02fc6f015b16b708e78d73f763d7adc758bc1d
ms.sourcegitcommit: 822d4b3cfa53269535500a3db5877a82b5076728
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87988407"
---
# <a name="add-data-from-external-data-sources-ssrs"></a>Hinzufügen von Daten aus externen Datenquellen (SSRS)
  Daten werden mithilfe einer Datenverbindung aus einer externen Datenquelle abgerufen. Datenverbindungsinformationen werden normalerweise vom Besitzer der externen Datenquelle bereitgestellt, der für das Gewähren der Berechtigungen und Festlegen der erforderlichen Anmeldeinformationstypen zuständig ist. Datenverbindungsinformationen werden als Berichtsdatenquelle gespeichert. Der Datenquellentyp bestimmt, welche Datenerweiterung zum Abrufen der Daten verwendet wird.  
  
 Weitere Informationen zu Datenquellentypen finden Sie unter [In diesem Abschnitt](#InThisSection).  
  
##  <a name="understanding-data-access-technology"></a><a name="DataAccess"></a> Grundlegendes zur Datenzugriffstechnologie  
 Zum Abrufen von Daten für ein Berichtsdataset sind mehrere Ebenen von Datenzugriffssoftware erforderlich. In der folgenden Liste wird die Verwendung von Datenzugriffstechnologien in Berichten kurz erläutert:  
  
-   **Anwendung und Benutzeroberfläche:** Die Anwendung Berichts-Generator dient zum Erstellen einer Datenquelle und zum Hinzufügen eines Verweises auf eine freigegebene Datenquelle, eines freigegebenes Datasets oder eines Berichtsteils, der die Datenquellen und die Datasets enthält, von denen er abhängt.  
  
-   **Berichtsdefinitionselemente:** Datenquellen und Datasets sind Teil der Berichtsdefinition. Nachdem ein Bericht auf einem Berichtsserver veröffentlicht wurde, werden freigegebene Datenquellen und freigegebene Datasets unabhängig von der Berichtsdefinition verwaltet.  
  
    -   **Datenquelle und freigegebene Datenquelle:** Dies ist der Teil einer Berichtsdefinition, der die Informationen zum Typ der Datenverarbeitungserweiterung sowie die Verbindungs- und Authentifizierungsinformationen enthält.  
  
    -   **Dataset und Feldauflistung:** Dies ist der Teil einer Berichtsdefinition, der die Abfrage, die Feldauflistung und die Felddatentypen enthält.  
  
-   **Reporting Services-Datenerweiterungen:** Diese integrierten Datenerweiterungen werden mit dem Berichts-Generator installiert. Eine Datenerweiterung stellt Funktionen zur Behandlung der Authentifizierung, von Serveraggregaten und mehrwertigen Parametern bereit.  
  
-   **Datenanbieter:** Diese Software verwaltet die Verbindung und den Abruf der Daten aus der externen Datenquelle. Der Datenanbieter definiert die Verbindungszeichenfolgensyntax. Die meisten Datenerweiterungen werden basierend auf einer Datenanbieterebene erstellt.  
  
-   **Externe Datenquelle:** Aus externen Datenquellen wie Datenbanken, Dateien, Cubes oder Webdiensten werden Berichtsdaten abgerufen.  
  
> [!NOTE]  
>  Wenn Sie nicht mit einem Berichtsserver verbunden sind, stehen Ihnen die mit Berichts-Generator installierten Datenerweiterungen zur Verfügung. Sie greifen im Einzelbenutzermodus von Ihrem Computer aus auf die Daten zu (mit Anmeldeinformationen). Wenn Sie mit einem Berichtsserver verbunden sind, können Sie die auf dem Berichtsserver installierten Datenerweiterungen auswählen. Der Zugriff auf die Daten erfolgt im Mehrbenutzermodus (der Bericht wird von mehreren Benutzern ausgeführt), und Sie verwenden Anmeldeinformationen für den Berichtsserver. Weitere Informationen finden Sie unter [Angeben der Anmeldeinformationen und Verbindungsinformationen für Berichtsdatenquellen](specify-credential-and-connection-information-for-report-data-sources.md).  
  
##  <a name="understanding-report-data"></a><a name="ReportData"></a> Grundlegendes zu Berichtsdaten  
 In der einfachsten Form zeigt ein Bericht Daten aus einem Berichtsdataset in einem Datenbereich auf der Berichtsseite an, d. h. in nur einer Tabelle, einem Diagramm, einer Matrix oder einem anderen Berichtsdatenbereich. Die Daten in einem Berichtsdataset stammen aus dem ersten Resultset, das für einen mit schreibgeschütztem Zugriff in einer externen Datenquelle ausgeführten Abfragebefehl zurückgegeben wird. Jeder Datenbereich wird bei Bedarf erweitert, um alle Daten aus dem Dataset anzuzeigen.  
  
 Daten in einem Dataset sind im Wesentliche tabellarische Daten. Spalten sind die Felder aus der Datasetabfrage. Zeilen sind die Zeilen aus dem Resultset. Die folgenden verallgemeinerten Datentypen können in einem Bericht verwendet werden:  
  
-   Rechteckige Daten. Daten aus einem Resultset, das in jeder Zeile gleiche Anzahl von Spalten aufweist.  
  
-   Hierarchische Daten werden als vereinfachtes Rowset unterstützt.  
  
    -   Unregelmäßige Hierarchien mit einer unterschiedlichen Anzahl von Spalten für jede Datenzeile werden nicht unterstützt. Dies hat für einige Datenerweiterungen Auswirkungen.  
  
    -   Datenerweiterungen, die mit mehrdimensionalen Datenquellen arbeiten, verwenden das XML for Analysis-Protokoll und rufen Daten nicht als Cellset, sondern als vereinfachtes Rowset ab.  
  
    -   Die XML-Datenerweiterung vereinfacht XML-Daten automatisch zur Verwendung in einem Bericht. Wenn die erste Instanz eines XML-Elements nicht alle Attribute oder Unterelemente enthält, sind die Daten u. U. nicht als Berichtsdaten verfügbar.  
  
-   Rekursive Daten werden unterstützt. Ein Resultset mit einer rekursiven Datenhierarchie enthält alle Informationen zur Hierarchiestruktur in einem rechteckigen Resultset. Die Mitarbeiterstruktur in einem Unternehmen kann z. B. durch eine Tabelle mit zwei Spalten dargestellt werden: ein Mitarbeiter und ein Manager. Jeder Manager ist auch ein Mitarbeiter mit einem Manager. Der Manager auf der obersten Ebene enthält normalerweise NULL oder einen anderen Bezeichner, der angibt, dass dieser Mitarbeiter keinen Manager hat.  
  
  
##  <a name="working-with-data-types"></a><a name="DataTypes"></a> Arbeiten mit Datentypen  
 Beim Erstellen eines Datasets werden die Datentypen der Felder einer Teilmenge der CLR-Datentypen (Common Language Runtime) von [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]zugeordnet. Datentypen, die nicht eindeutig zugeordnet werden können, werden als Zeichenfolgen zurückgegeben. Weitere Informationen zum Arbeiten mit Felddatentypen finden Sie unter [Datasetfeld-Sammlung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md). Wenn Sie einen Parameter erstellen, muss es sich beim Datentyp um einen unterstützten Berichtsdefinitions-Datentyp handeln. Weitere Informationen zur Zuordnung von Datentypen des Datenanbieters zu einem Berichtsparameter finden Sie unter [Datentypen in Ausdrücken &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md).  
  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> Themen zur Vorgehensweise  
 Dieser Abschnitt enthält schrittweise Anweisungen zum Arbeiten mit Datenverbindungen, Datenquellen und Datasets.  
  
 [Hinzufügen und Prüfen einer Datenverbindung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
 [Erstellen eines freigegebenen Datasets oder eingebetteten Datasets &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
 [Hinzufügen eines Filters zu einem Dataset &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
  
##  <a name="in-this-section"></a><a name="InThisSection"></a> In diesem Abschnitt  
 Die folgenden Themen enthalten Informationen zu jeder integrierten Datenerweiterung.  
  
|Thema|Datenquellentyp|  
|-----------|----------------------|  
|[SQL Server-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/sql-server-connection-type-ssrs.md)|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|[Analysis Services-Verbindungstyp für MDX &#40;SSRS&#41;](../../reporting-services/report-data/analysis-services-connection-type-for-mdx-ssrs.md)|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|  
|[PowerPivot-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/power-pivot-connection-type-ssrs.md)|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|  
|[SharePoint-Listenverbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/sharepoint-list-connection-type-ssrs.md)|[!INCLUDE[msCoName](../../includes/msconame-md.md)] SharePoint-Liste|  
|[Azure SQL-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/sql-azure-connection-type-ssrs.md)|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|  
|[SQL Server Parallel Data Warehouse-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/sql-server-parallel-data-warehouse-connection-type-ssrs.md)|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDWfull](../../includes/ssdwfull-md.md)]|  
|[SAP NetWeaver BI-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/sap-netweaver-bi-connection-type-ssrs.md)|SAP NetWeaver BI|  
|[Hyperion Essbase-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/hyperion-essbase-connection-type-ssrs.md)|Hyperion Essbase|  
|[OLE DB-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/ole-db-connection-type-ssrs.md)|OLE DB|  
|[ODBC-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/odbc-connection-type-ssrs.md)|ODBC|  
|[XML-Verbindungstyp &#40;SSRS&#41;](../../reporting-services/report-data/xml-connection-type-ssrs.md)|XML|  
  
##  <a name="related-sections"></a><a name="Related"></a> Verwandte Abschnitte

 Diese Abschnitte der Dokumentation enthalten umfassende grundlegende Informationen zu Berichtsdaten sowie Informationen zum Definieren, Anpassen und Verwenden der mit Daten zusammenhängenden Teile eines Berichts.  
  
|Thema|BESCHREIBUNG|  
|-----------|-----------------|  
|[Berichtsdatasets &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)|Bietet eine Übersicht über den Zugriff auf Daten für den Bericht.|  
|[Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)|Enthält Informationen zu Datenverbindungen und Datenquellen.|  
|[Erstellen von Berichten zu eingebetteten und freigegebenen Datasets &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)|Enthält Informationen zu eingebetteten und freigegebenen Datasets.|  
|[Datasetfelder-Sammlung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)|Enthält Informationen zur von der Abfrage generierten Datasetfeldauflistung.|  
|[Von Reporting Services unterstützte Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)|Enthält ausführliche Informationen zur Plattform- und Versionsunterstützung für die einzelnen Datenerweiterungen.|  
|[Data Processing Extensions Overview (Übersicht über Datenverarbeitungserweiterungen)](../../reporting-services/extensions/data-processing/data-processing-extensions-overview.md)|Enthält ausführliche Informationen zu Datenerweiterungen für erfahrene Benutzer.|  
  
  
## <a name="see-also"></a>Weitere Informationen  
 [Berichtsdatasets &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)   
 [Abfrageentwurfstools &#40;SSRS&#41;](query-design-tools-ssrs.md)  
  
  
