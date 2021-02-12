---
title: Anzeigen einer Reihe mit mehreren Datenbereichen in einem Diagramm | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie die Lesbarkeit von Diagrammen mithilfe von Skalierungsunterbrechungslinien und Filtern sowie durch Trennen von Datenbereichen im Berichts-Generator erhöhen.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 45da3d39-278e-4760-a4b3-9932c9547cf2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b808f4b7641d11f98fe8a2be877b0485fbdff5b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042045"
---
# <a name="displaying-a-series-with-multiple-data-ranges-on-a-chart"></a>Anzeigen einer Reihe mit mehreren Datenbereichen in einem Diagramm

  Ein Diagramm verwendet die niedrigsten und die höchsten Werte einer Reihe, um die Achsenskalierung zu berechnen. Wenn eine Reihe im Diagramm mehr als einen Datenbereich enthält, sind ggf. nicht mehr alle Datenpunkte sichtbar, und es werden nur einige Datenpunkte im Diagramm deutlich angezeigt. Angenommen, ein Bericht zeigt die täglichen Umsatzgesamtbeträge für einen Zeitraum von 30 Tagen an.  
  
 ![Diagramm mit mehreren Datenbereichen](../../reporting-services/report-design/media/rs-multipledatarangeschart.gif "Diagramm mit mehreren Datenbereichen")  
  
 Der Umsatz liegt für den Monat meist zwischen den Werten 10 und 40. Eine einwöchige Marketingkampagne hat Anfang April jedoch einen abrupten Umsatzanstieg bewirkt. Diese Änderung der Umsatzdaten führt zu einer ungleichmäßigen Verteilung von Datenpunkten, sodass die Lesbarkeit des gesamten Diagramms verringert wird.  
  
 Es gibt verschiedene Möglichkeiten, die Lesbarkeit zu verbessern.  
  
-   **Skalierungsunterbrechungen aktivieren**: Wenn die Daten zwei oder mehr Gruppen von Datenbereichen bilden, können Sie die Lücke zwischen den Bereichen mit einer Skalierungsunterbrechung entfernen. Eine Skalierungsunterbrechung ist ein Streifen, der über den Zeichnungsbereich gezogen wird, um eine Unterbrechung zwischen den hohen und niedrigen Werten einer Reihe zu kennzeichnen.  
  
-   **Filter für unnötige Werte**: Wenn Datenpunkte vorhanden sind, die den wichtigen Datenbereich eines Diagramms verdecken, können Sie die nicht erwünschten Punkte mit einem Berichtsfilter entfernen. Weitere Informationen zum Hinzufügen eines Filters zum Diagramm in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] finden Sie unter [Hinzufügen von Datasetfiltern, Datenbereichsfiltern und Gruppenfiltern (Berichts-Generator und SSRS)](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md).  
  
-   **Plotten jedes Datenbereichs als separate Reihe, um mehrere Reihen vergleichen zu können**: Wenn Sie über mehr als zwei Datenbereiche verfügen, sollten Sie erwägen, die Datenbereiche in separate Reihen zu unterteilen. Weitere Informationen hierzu finden Sie unter [Mehrere Reihen in einem Diagramm &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/multiple-series-on-a-chart-report-builder-and-ssrs.md):  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="displaying-multiple-data-ranges-using-scale-breaks"></a>Anzeigen von mehreren Datenbereichen mit Skalierungsunterbrechungen  
 Wenn Sie eine Skalierungsunterbrechung aktivieren, berechnet das Diagramm, an welcher Stelle im Diagramm eine Linie gezeichnet werden soll. Zwischen den Bereichen muss eine ausreichende Trennung bestehen, damit eine Skalierungsunterbrechung gezeichnet werden kann. Standardmäßig kann eine Skalierungsunterbrechung nur hinzugefügt werden, wenn eine Trennung zwischen den Datenbereichen eines Diagramms mindestens 25 % ausmacht.  
  
 ![Diagramm mit Skalierungsunterbrechungslinie](../../reporting-services/report-design/media/rs-multipledatarangeschart-scalebreak.gif "Diagramm mit Skalierungsunterbrechungslinie")  
  
> [!NOTE]  
>  Sie können nicht angeben, an welcher Stelle im Diagramm eine Skalierungsunterbrechung platziert werden soll. Sie können jedoch ändern, wie die Skalierungsunterbrechung berechnet wird. Dies wird weiter unten in diesem Thema beschrieben.  
  
 Wenn Sie eine Skalierungsunterbrechung aktivieren, diese jedoch auch dann nicht angezeigt wird, wenn zwischen den Datenbereichen ein ausreichender Abstand herrscht, können Sie die Eigenschaft CollapsibleSpaceThreshold auf einen niedrigeren Wert als 25 festlegen. CollapsibleSpaceThreshold gibt den erforderlichen reduzierbaren Zwischenraum an, der zwischen den Datenbereichen vorhanden sein muss. Weitere Informationen hierzu finden Sie unter [Hinzufügen von Skalierungsunterbrechungen zu einem Diagramm (Berichts-Generator und SSRS)](../../reporting-services/report-design/add-scale-breaks-to-a-chart-report-builder-and-ssrs.md).  
  
 Diagramme unterstützen bis zu fünf Skalierungsunterbrechungen pro Diagramm. Ein Diagramm wird jedoch ggf. unleserlich, wenn mehr als eine Skalierungsunterbrechung angezeigt wird. Wenn Sie mehr als zwei Datenbereiche haben, sollten Sie erwägen, zum Anzeigen der Daten eine andere Methode zu verwenden. Weitere Informationen hierzu finden Sie unter [Mehrere Reihen in einem Diagramm &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/multiple-series-on-a-chart-report-builder-and-ssrs.md):  
  
## <a name="unsupported-scale-break-scenarios"></a>Nicht unterstützte Skalierungsunterbrechungsszenarios  
 Skalierungsunterbrechungen werden für die folgenden Diagrammszenarios nicht unterstützt:  
  
-   Das Diagramm weist eine 3D-Aktivierung auf.  
  
-   Eine logarithmische Wertachse wurde angegeben.  
  
-   Für die Wertachse wurden der Mindestwert und der Höchstwert explizit festgelegt.  
  
-   Das Diagramm hat den folgenden Typ: Polar, Netz, Kreis, Ring, Trichter, Pyramide oder Gestapelt.  
  
 Ein Beispiel eines Diagramms mit Skalierungsunterbrechungen ist als Beispielbericht verfügbar. Weitere Informationen zum Herunterladen des Beispielberichts und anderer Berichte finden Sie unter [Beispielberichte zu Berichts-Generator und Berichts-Designer](https://go.microsoft.com/fwlink/?LinkId=198283).  

## <a name="next-steps"></a>Nächste Schritte

[Mehrere Reihen in einem Diagramm](../../reporting-services/report-design/multiple-series-on-a-chart-report-builder-and-ssrs.md)   
[Formatieren eines Diagramms](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
[3D, Abschrägungen und andere Diagrammeffekte](../../reporting-services/report-design/chart-effects-3d-bevel-and-other-report-builder.md)   
[Diagramme](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
[Achseneigenschaften (Dialogfeld), Achsenoptionen](/previous-versions/sql/)   
[Zusammenfassen von kleinen Slices in einem Kreisdiagramm](../../reporting-services/report-design/collect-small-slices-on-a-pie-chart-report-builder-and-ssrs.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)