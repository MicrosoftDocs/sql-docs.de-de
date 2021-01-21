---
title: XML-Verbindungstyp | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über den XML-Verbindungstyp zum Herstellen einer Verbindung mit und Abrufen von Daten aus XML-Dokumenten, Webdiensten oder in die Abfrage eingebetteten XML-Daten.
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 5b55fff2-1b15-4156-83ef-15ad9cf9f509
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b6ca1df39a5aeed8b57de5c6eff7ab66e725062d
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596534"
---
# <a name="xml-connection-type-ssrs"></a>XML-Verbindungstyp (SSRS)
  Wenn Sie Daten aus einer XML-Datenquelle in den Bericht einschließen möchten, benötigen Sie ein Dataset, das auf einer Berichtsdatenquelle vom Typ "XML" basiert. Dieser integrierte Datenquellentyp basiert auf der XML-Datenerweiterung. Verwenden Sie diesen Datenquellentyp, um eine Verbindung mit XML-Dokumenten, Webdiensten oder in die Abfrage eingebetteten XML-Daten herzustellen und Daten abzurufen.  
  
 Diese Datenerweiterung unterstützt Parameter und Anmeldeinformationen, die getrennt von der Verbindungszeichenfolge verwaltet werden.  
  
 Verwenden Sie die Informationen in diesem Thema, um eine Datenquelle zu erstellen. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Hinzufügen und Prüfen einer Datenverbindung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="connection-string"></a><a name="Connection"></a> Verbindungszeichenfolge  
 Bei der Verbindungszeichenfolge muss es sich um eine URL handeln, von der auf den Webdienst, die webbasierte Anwendung oder das per HTTP verfügbare XML-Dokument verwiesen wird. XML-Dokumente müssen die Erweiterung XML aufweisen. Für in der Datasetabfrage eingebettete XML-Daten können Sie auch eine leere Verbindungszeichenfolge verwenden.  
  
 Im folgenden Beispiel wird die Syntax der Verbindungszeichenfolge für einen Webdienst bzw. für ein XML-Dokument angegeben. Das `file://` -Protokoll wird nicht unterstützt.  
  
|XML-Dokumenttyp|Beispiel für Verbindungszeichenfolge|  
|-----------------------|-------------------------------|  
|Webdienst|`https://adventure-works.com/results.aspx`|  
|XML-Dokument|`https://localhost/XML/Customers.xml`|  
|Eingebettetes XML-Dokument|*Leer*|  
  
 Weitere Beispiele für Verbindungszeichenfolgen finden Sie unter [Datenverbindungen, Datenquellen und Verbindungszeichenfolgen in Berichts-Generator](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
##  <a name="credentials"></a><a name="Credentials"></a> Anmeldeinformationen  
 Anmeldeinformationen sind erforderlich, um Abfragen auszuführen und den Bericht lokal oder vom Berichtsserver aus in der Vorschau anzuzeigen.  
  
 Nachdem Sie den Bericht veröffentlicht haben, müssen Sie eventuell die Anmeldeinformationen für die Datenquelle ändern, sodass die Berechtigungen zum Abrufen der Daten beim Ausführen des Berichts auf dem Berichtsserver gültig sind.  
  
 Auf einem Berichterstellungsclient sind die folgenden Optionen zum Angeben von Anmeldeinformationen verfügbar:  
  
-   Aktueller Windows-Benutzer (auch bekannt als integrierte Sicherheit).  
  
-   Anmeldeinformationen sind nicht erforderlich. Wenn Sie keine Anmeldeinformationen auswählen, wird der anonyme Zugriff verwendet. Stellen Sie sicher, dass für die Verbindung des Berichtsservers mit einer externen Datenquelle ein Konto für die unbeaufsichtigte Ausführung definiert ist. Die XML-Datenverarbeitungserweiterung übergibt keine Anmeldeinformationen an die Ziel-URL oder den Webdienst. Die Verbindung wird nur dann hergestellt, wenn Sie das Konto für die unbeaufsichtigte Ausführung definiert haben. Weitere Informationen finden Sie unter [Konfigurieren des Kontos für die unbeaufsichtigte Ausführung &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
 Gespeicherte Anmeldeinformationen oder Aufforderungen zur Eingabe von Anmeldeinformationen werden nicht unterstützt. Wenn Sie die integrierte Sicherheit von Windows deaktiviert haben, können Sie sie nicht zum Abrufen von Daten verwenden. Wenn Sie gespeicherte Anmeldeinformationen oder auf Anforderung einzugebende Anmeldeinformationen angeben, tritt ein Laufzeitfehler auf.  
  
 Weitere Informationen finden Sie unter [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) oder [Angeben der Anmeldeinformationen und Verbindungsinformationen für Berichtsdatenquellen](specify-credential-and-connection-information-for-report-data-sources.md).  
  
##  <a name="queries"></a><a name="Query"></a> Abfragen  
 Mit einer Abfrage wird angegeben, welche Daten für ein Berichtsdataset abgerufen werden sollen. Die Feldauflistung für ein Dataset wird mit den Spalten aus dem Resultset einer Abfrage aufgefüllt. In Berichten wird nur das erste Resultset verarbeitet, das von einer Abfrage abgerufen wird.  
  
 Sie müssen den textbasierten Abfrage-Designer verwenden, um die Abfrage zu erstellen. Die Abfrage muss XML-Daten zurückgeben.  
  
 Weitere Informationen über den textbasierten Abfrage-Designer finden Sie unter [Benutzeroberfläche des textbasierten Abfrage-Designers (Berichts-Generator)](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md).  
  
 Die möglichen Werte einer Datasetabfrage für eine Datenquelle vom Typ "XML" sind unten dargestellt.  
  
-   *Leer*  
  
     Verwenden Sie eine leere Abfrage, um ein Standardresultset zu erstellen. Die Standardabfrage wird erstellt, indem die Datenquelle gelesen und die XML-Knotenhierarchie bis zur ersten Blattauflistung durchsucht wird. Das Resultset enthält alle Knoten mit Textwerten und alle Knotenattribute unter diesem Pfad. Die Spalten im Resultset werden den Feldern für das Dataset zugeordnet.  
  
-   *Elementpfad*  
  
     Gibt die Sequenz von Knoten an, die beim Abrufen von XML-Daten aus der Datenquelle verwendet wird.  
  
-   *XML-Abfrageelement*  
  
     Eine XML-Abfragespezifikation mit den folgenden optionalen Elementen:  
  
    -   **Die XML-Datenquelle stellt einen Webdienst dar**  
  
         Erforderliche XML-Elemente:  
  
         `<Method Namespace=` *"Namespace"*  `Name="MethodName" />`  
  
         `-- or --`  
  
         `<SoapAction>` *SOAP-Aktion* `</SoapAction>`  
  
         Optionale XML-Elemente:  
  
         `<ElementPath>`  *Elementpfad*  `</ElementPath>`  
  
         `<Method Namespace=` *"Namespace"*  `Name="MethodName" />`  
  
         `-- or --`  
  
         `<SoapAction>` *SOAP-Aktion* `</SoapAction>`  
  
    -   **Die XML-Datenquelle stellt ein XML-Dokument dar**  
  
         Erforderliche XML-Elemente: Keine  
  
         Optionale XML-Elemente:  
  
         `<ElementPath>`  *Elementpfad*  `</ElementPath>`  
  
    -   **Die XML-Datenquelle stellt ein eingebettetes XML-Dokument dar**  
  
         Erforderliche XML-Elemente:  
  
         `<XmlData>` inneres XML-Element `</XmlData>`  
  
         Optionale XML-Elemente:  
  
         `<ElementPath>`  *Elementpfad*  `</ElementPath>`  
  
         `-- or --`  
  
         `<ElementPath IgnoreNamespaces="true">`  *Elementpfad*  `</ElementPath>`  
  
 Weitere Informationen zur Abfragesyntax finden Sie unter [XML-Abfragesyntax für XML-Berichtsdaten &#40;SSRS&#41;](../../reporting-services/report-data/xml-query-syntax-for-xml-report-data-ssrs.md).  
  
 Beispiele finden Sie unter [Reporting Services: Verwenden von XML und Webdienst-Datenquellen](/previous-versions/sql/sql-server-2005/administrator/aa964129(v=sql.90)).  
  
### <a name="requirements-for-retrieving-xml-web-service-data"></a>Anforderungen für das Abrufen von XML-Webdienstdaten  
 Die XML-Datenverarbeitungserweiterung kann das Schema nicht selbstständig erkennen. Daher müssen Sie über eine Möglichkeit verfügen, die SOAP-Methoden zu ermitteln, mit denen die gewünschten Daten abgerufen werden können. Sie müssen außerdem wissen, welches Adressierungsschema oder welchen Namespace der Webdienst für seine Daten verwendet.  
  
 Für einen Webdienst können Sie ein \<**Query**>-Element angeben, das eine aufzurufende Methode oder eine SOAP-Aktion angibt. Sie können die Abfrage leer lassen und die Standardabfrage verwenden, wenn die XML-Datenquelle eine hierarchische Struktur besitzt, die die im Bericht zu verwendenden Daten bereitstellt. Mit der Abfrage abgerufene Werte und Attribute von XML-Elementknoten werden den im Bericht verwendeten Datasetfeldern zugeordnet.  
  
### <a name="requirements-for-retrieving-xml-document-data"></a>Anforderungen für das Abrufen von XML-Dokumentdaten  
 Bei Verwendung von HTTP muss der Server XML-Daten zurückgeben, oder die XML-Daten müssen im XML- **Query** -Element eingebettet sein. Wenn Sie über HTTP direkt auf ein XML-Dokument verweisen, muss die Erweiterung .xml verwendet werden.  
  
 Sie müssen wissen, wie Sie eine XML-Abfrage erstellen, die alle benötigten Daten abruft. Wenn Sie keinen Elementpfad angeben, besteht das Standardverhalten zum Analysieren eines XML-Dokuments darin, dass der erste verfügbare Pfad zu einer Blattknotenauflistung im XML-Dokument ausgewählt wird. Wenn das XML-Dokument zusätzliche Pfade zu anderen Auflistungen gleichgeordneter Blattknoten enthält, werden diese Knoten ignoriert, es sei denn, Sie geben in der Abfrage einen Pfad ein.  
  
 Zum Angeben eines Elementpfads können Sie eine XQuery-ähnliche XML-Syntax verwenden.  
  
 Weitere Informationen finden Sie unter [Syntax für Elementpfade für XML-Berichtsdaten &#40;SSRS&#41;](../../reporting-services/report-data/element-path-syntax-for-xml-report-data-ssrs.md).  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parameter  
 Die Abfrage wird nicht analysiert, um Parameter zu identifizieren.  
  
 Zum Hinzufügen von Parametern müssen diese im Dialogfeld **Dataseteigenschaften** auf der Seite [Parameter](/previous-versions/sql/) manuell erstellt werden.  
  
##  <a name="remarks"></a><a name="Remarks"></a> Hinweise  
 Die XML-Datenerweiterung unterstützt das Erstellen von Berichten auf Basis von tabellarischen (nicht hierarchischen) XML-Daten. Weitere Informationen finden Sie unter [Add Data from External Data Sources (SSRS) (Hinzufügen von Daten aus externen Datenquellen)](../../reporting-services/report-data/add-data-from-external-data-sources-ssrs.md).  
  
 Es ist keine integrierte Unterstützung zum Abrufen von XML-Dokumenten aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank vorhanden.  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> Themen zur Vorgehensweise  
 Dieser Abschnitt enthält schrittweise Anweisungen zum Arbeiten mit Datenverbindungen, Datenquellen und Datasets.  
  
 [Hinzufügen und Prüfen einer Datenverbindung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
 [Erstellen eines freigegebenen Datasets oder eingebetteten Datasets &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
 [Hinzufügen eines Filters zu einem Dataset &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
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
  
 [Von Reporting Services unterstützte Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)  
 Enthält ausführliche Informationen zur Plattform- und Versionsunterstützung für die einzelnen Datenerweiterungen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Berichtsparameter &#40;Berichts-Generator und Berichts-Designer&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Filtern, Gruppieren und Sortieren von Daten &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Ausdrücke &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
