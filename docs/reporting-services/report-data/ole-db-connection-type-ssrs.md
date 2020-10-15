---
title: OLE DB-Verbindungstyp | Microsoft-Dokumentation
description: Mithilfe der Informationen in diesem Artikel zum OLE DB-Verbindungstyp erfahren Sie, wie Sie eine Datenquelle erstellen.
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: d00cb13b-e1c2-4300-a195-3da1430a2df1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bff06284226c67a0f784b9e77bbe85dd056edb09
ms.sourcegitcommit: 783b35f6478006d654491cb52f6edf108acf2482
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91891140"
---
# <a name="ole-db-connection-type-ssrs"></a>OLE-Datenbank-Verbindungstyp (SSRS)
  Wenn Sie Daten von einem OLE DB-Datenanbieter einschließen möchten, benötigen Sie ein Dataset, das auf einer Berichtsdatenquelle vom Typ "OLE DB" basiert. Dieser integrierte Datenquellentyp basiert auf der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-OLE DB-Datenverarbeitungserweiterung.  
  
 OLE DB ist eine Datenzugriffstechnologie, die es Clients ermöglicht, eine Verbindung mit einer Vielzahl von Datenanbietern herzustellen. Nachdem Sie den Datenquellentyp OLE DB ausgewählt haben, müssen Sie einen bestimmten Datenanbieter auswählen. Die Unterstützung für Funktionen wie Parameter und Anmeldeinformationen hängt vom ausgewählten Datenanbieter ab.  
  
 Verwenden Sie die Informationen in diesem Thema, um eine Datenquelle zu erstellen. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Hinzufügen und Prüfen einer Datenverbindung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="connection-string"></a><a name="Connection"></a> Verbindungszeichenfolge  
 Die Verbindungszeichenfolge für die OLE DB-Datenverarbeitungserweiterung hängt vom gewünschten Datenanbieter ab. Eine typische Verbindungszeichenfolge enthält Name-Wert-Paare, die vom Datenanbieter unterstützt werden. Die folgende Verbindungszeichenfolge gibt z. B. den OLE DB-Anbieter für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client und die AdventureWorks-Datenbank an:  
  
```  
Provider=SQLNCLI10.1;Data Source=server; Initial Catalog=AdventureWorks  
```  
  
 Die verwendete Verbindungszeichenfolge hängt von der externen Datenquelle ab, mit der Sie eine Verbindung herstellen. Wenn Sie die für einen Datenanbieter spezifischen Verbindungszeichenfolgen-Eigenschaften festlegen möchten, klicken Sie im Dialogfeld **Datenquelleneigenschaften** auf der Registerkarte **Allgemein** auf die Schaltfläche **Erstellen** , um das Dialogfeld **Verbindungseigenschaften** zu öffnen. Legen Sie erweiterte Datenquelleneigenschaften im Dialogfeld **Datenlinkeigenschaften** fest.  
  
 Beispiele für Verbindungszeichenfolgen finden Sie unter [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
  
##  <a name="credentials"></a><a name="Credentials"></a> Anmeldeinformationen  
 Anmeldeinformationen sind erforderlich, um Abfragen auszuführen und den Bericht lokal oder vom Berichtsserver aus in der Vorschau anzuzeigen.  
  
 Nachdem Sie den Bericht veröffentlicht haben, müssen Sie eventuell die Anmeldeinformationen für die Datenquelle ändern, sodass die Berechtigungen zum Abrufen der Daten beim Ausführen des Berichts auf dem Berichtsserver gültig sind.  
  
 Weitere Informationen finden Sie unter [Angeben der Anmeldeinformationen und Verbindungsinformationen für Berichtsdatenquellen](specify-credential-and-connection-information-for-report-data-sources.md).  
  
###### <a name="special-characters-in-a-password"></a>Sonderzeichen in Kennwörtern  
 Wenn Sie die OLE DB-Datenquelle so konfigurieren, dass ein Kennwort angegeben werden muss oder das Kennwort in der Verbindungszeichenfolge enthalten ist, und Benutzer das Kennwort mit Sonderzeichen (z. B. Satzzeichen) eingeben, können einige zugrunde liegende Datenquellentreiber die Sonderzeichen nicht überprüfen. Wenn Sie den Bericht verarbeiten, ist die Meldung "Kein zulässiges Kennwort" möglicherweise ein Anzeichen für dieses Problem.  
  
> [!NOTE]  
>  Es wird empfohlen, der Verbindungszeichenfolge keine Anmeldeinformationen (z. B. Kennwörter) hinzuzufügen. Der Berichts-Generator enthält eine separate Registerkarte im Dialogfeld **Datenquelle** , auf der Sie Anmeldeinformationen eingeben können.  
  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parameter  
 Einige OLE DB-Anbieter unterstützen unbenannte Parameter und nicht benannte Parameter. Parameter werden anhand der Position übergeben, indem in der Abfrage ein Platzhalter verwendet wird. Das Platzhalterzeichen hängt von der vom Datenanbieter unterstützten Syntax ab.  
  
  
##  <a name="remarks"></a><a name="Remarks"></a> Hinweise  
 OLE DB ist eine systemeigene Technologie zum Erstellen von Datenanbietern für bestimmte Datenquellen. OLE DB basiert auf COM-Schnittstellen (Component Object Model). Die OLE DB-Technologie wurde nach ODBC- und vor ADO.NET-Datenanbietern entwickelt. OLE DB-Datenanbieter werden wie andere COM-Komponenten im Betriebssystem registriert. OLE DB-Datenanbieter sind von Microsoft und Drittanbietern verfügbar. Microsoft stellt auch MSDASQL bereit, einen OLE DB-Datenanbieter, der die Kommunikation mit ODBC-Treibern überbrückt. Weitere Informationen finden Sie unter [ODBC-Verbindungstyp (SSRS)](../../reporting-services/report-data/odbc-connection-type-ssrs.md).  
  
 Zum erfolgreichen Abrufen der gewünschten Daten muss eine vom Datenanbieter unterstützte Abfragesyntax angegeben werden. Die Parameterunterstützung variiert abhängig vom Datenanbieter. Weitere Informationen finden Sie in den spezifischen Themen für den ausgewählten Datenanbieter. Beispiel:  
  
-   [OLE DB-Anbieter für Analysis Services &#40;Analysis Services – mehrdimensionale Daten&#41;](/analysis-services/instances/data-providers-used-for-analysis-services-connections
)  
   
  
-   [SQL Server Native Client &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  
 Weitere Informationen zu bestimmten OLE DB-Datenanbietern finden Sie unter [Von Reporting Services unterstützte Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
  
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
  
