---
title: Leere und NULL-Datenpunkte in Diagrammen (Berichts-Generator) | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die Verarbeitung von leeren Werten in Diagrammen, die Verwendung von Platzhaltern und wie leere Datenpunkte und NULL-Datenpunkte im Berichts-Generator angezeigt werden.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: faddd29d-4cc1-4c2c-8e29-d3d9918fe22a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4ed8a71adea4b57d96137cc94168ca1810956bdd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073784"
---
# <a name="empty-and-null-data-points-in-charts-report-builder-and-ssrs"></a>Leere und NULL-Datenpunkte in Diagrammen (Berichts-Generator und SSRS)

  Wenn Sie Felder mit leeren oder NULL-Werte in einem Diagramm im paginierten [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Bericht anzeigen, wird das Diagramm möglicherweise nicht wie erwartet angezeigt. Je nach angegebenem Diagrammtyp werden leere Werte in Diagrammen unterschiedlich verarbeitet:  
  
-   Bei linearen Diagrammtypen (Balken-, Säulen-, Punkt-, Linien-, Flächen- oder Bereichsdiagramme) werden leere Werte als leere Bereiche oder Lücken im Diagramm dargestellt. Wenn Sie leere Punkte angeben möchten, müssen Sie Platzhalter für leere Punkte hinzufügen. Weitere Informationen finden Sie unter [Hinzufügen von leeren Punkten zu einem Diagramm (Berichts-Generator und SSRS)](../../reporting-services/report-design/add-empty-points-to-a-chart-report-builder-and-ssrs.md).  
  
-   Bei fortlaufenden, linearen Diagrammtypen (Bereich, Balken, Spalte, Zeile und Punkte) werden dem Diagramm leere Datenpunkte hinzugefügt, um die Kontinuität der Reihe zu erhalten.  
  
-   Bei nichtlinearen Diagrammtypen (Polar-, Kreis-, Ring-, Trichter- oder Pyramidendiagramme) werden leere Werte nicht im Diagramm dargestellt.  
  
-   In Formdiagrammtypen werden NULL-Werte weggelassen.  
  
 Ein Beispiel eines Diagramms mit leeren Datenpunkten ist als Beispielbericht verfügbar. Weitere Informationen zum Herunterladen des Beispielberichts und anderer Berichte finden Sie unter [Beispielberichte zu Berichts-Generator und Berichts-Designer](https://go.microsoft.com/fwlink/?LinkId=198283).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="removing-empty-or-null-values"></a>Entfernen von leeren Werten oder NULL-Werten  
 Wenn Sie vermeiden möchten, dass wichtige Daten verdeckt werden, entfernen Sie ggf. leere Werte aus dem Dataset. Zum Filtern von NULL-Werten können Sie die NOT IS NULL-Klausel in der Abfrage verwenden. Alternativ dazu können Sie einen Filterausdruck hinzufügen, der angibt, dass nur Werte ungleich null (0) angezeigt werden sollen. Weitere Informationen finden Sie unter [Hinzufügen von Datasetfiltern, Datenbereichsfiltern und Gruppenfiltern &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md).  
  
## <a name="fields-with-no-values-in-a-chart"></a>Felder ohne Werte in einem Diagramm  
 Wenn ein Feld im zurückgegebenen Dataset keine Werte enthält, wird ein leeres Diagramm ohne Datenpunkte angezeigt, der Reihenname (normalerweise der Feldname) wird jedoch als Legendenelement hinzugefügt.  
  
 Befinden sich hingegen im zurückgegebenen Dataset gar keine Datenzeilen, ist das Verhalten anders. Dies kann vorkommen, wenn der Bericht parametrisiert wurde und der ausgewählte Wert ein leeres Resultset zurückgibt. Wenn die Datasetabfrage null Datenzeilen zurückgibt, wird zur Laufzeit eine Meldung eingeblendet, dass keine Daten angezeigt werden können. Sie können diese Meldung anpassen, indem Sie die NoDataMessage-Beschriftung für den Bericht im Bereich **Eigenschaften** ändern. Weitere Informationen finden Sie unter [Erstellen von Berichten zu eingebetteten und freigegebenen Datasets &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md).  

## <a name="next-steps"></a>Nächste Schritte

[Diagramme](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
[Formatieren eines Diagramms](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
[Hinzufügen eines Diagramms zu einem Bericht](../../reporting-services/report-design/add-a-chart-to-a-report-report-builder-and-ssrs.md)   
[Problembehandlung bei Diagrammen](../../reporting-services/report-design/troubleshoot-charts-report-builder-and-ssrs.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
