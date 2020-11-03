---
title: Hinzufügen von Datenraster zu mobilen Berichten | Reporting Services | Microsoft-Dokumentation
description: Sie können Daten mit dem Publisher für mobile Berichte von Microsoft SQL Server in einem Raster darstellen. Dazu können Sie ein einfaches Datenraster, ein Indikatordatenraster oder ein Diagrammdatenraster verwenden.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.topic: conceptual
ms.assetid: fe98a970-90d3-44d1-9189-9141c237f141
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e346ff2f6d5b4951e7ce0e3af81914b15b34ac15
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907318"
---
# <a name="add-data-grids-to-mobile-reports--reporting-services"></a>Hinzufügen von Datenrastern zu mobilen Berichten | Reporting Services
Mitunter sind die Daten selbst die beste Visualisierung. Erfahren Sie mehr über die drei *Datenraster* (bzw. Tabellen) zum Anzeigen von Daten in [!INCLUDE[SS_MobileReptPub_Long](../../includes/ss-mobilereptpub-long.md)]:
* Einfaches Datenraster
* Indikatordatenraster
* Diagrammdatenraster

## <a name="simple-data-grid"></a>Einfaches Datenraster
Das grundlegende bzw. einfache Datenraster kann mehrere Spalten mit Daten mit benutzerdefinierten Formaten und Kopfzeilen anzeigen. 

![Screenshot eines einfachen Datenrasters in einem mobilen Bericht](../../reporting-services/mobile-reports/media/mobile-report-simple-data-grid.png)

Nachdem Sie der Entwurfsoberfläche ein Datenraster hinzugefügt haben, können Sie es mit realen Daten verbinden.

1. Ziehen Sie von der Registerkarte **Layout** ein einfaches Datenraster in den Entwurfsbereich, und stellen Sie es auf die gewünschte Größe ein.

2. Rufen Sie [Daten aus Excel oder einem freigegebenen Dataset](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md)ab.

3. Wählen Sie die Registerkarte **Daten** und im Bereich **Dateneigenschaften** unter **Daten für die Rasteransicht** eine Datentabelle aus.

4. Wählen Sie im Bereich **Spalten** die gewünschten Spalten aus. Sie können sie neu anordnen und umbenennen sowie ihr Format und ihre Aggregation festlegen. 

 
##  <a name="indicator-data-grid"></a>Indikatordatenraster
Sie können einem Indikatordatenraster Spalten mit Messgeräten hinzufügen.

![Screenshot eines Indikatordatenrasters in einem mobilen Bericht](../../reporting-services/mobile-reports/media/mobile-report-indicator-data-grid.png)

1. Ziehen Sie von der Registerkarte **Layout** ein Indikatordatenraster in den Entwurfsbereich, und stellen Sie es auf die gewünschte Größe ein.

2. Wählen Sie auf der Registerkarte **Daten** im Bereich **Spalten** die Option **Messspalte hinzufügen** aus. 

3. Wählen Sie **Optionen** und und dann einen **Messgerättyp** aus. 

4. Legen Sie wie bei **Messgeräten, die Sie Ihrem mobilen Bericht direkt hinzufügen** die Felder **Wert** und **Vergleich** sowie [Wertrichtung](../../reporting-services/mobile-reports/add-gauges-to-mobile-reports-reporting-services.md)fest.

Das Datenraster gibt im Messgerät automatisch nur die Daten für die jeweilige Zeile des Datenrasters ein.  

## <a name="chart-data-grid"></a>Diagrammdatenraster
Einem Diagrammdatenraster können Sie Spalten mit Messgeräten oder Diagrammen hinzufügen. 

![Screenshot eines Diagrammdatenrasters in einem mobilen Bericht](../../reporting-services/mobile-reports/media/mobile-report-chart-data-grid.png)

Wenn Sie einem Datenraster eine Diagrammspalte hinzufügen, müssen Sie eine getrennte Datentabelle hinzufügen, um in jeder Zeile Daten für das Diagramm bereitzustellen. Diese zweite Datentabelle muss ein Feld mit der Hauptdatentabelle gemeinsam nutzen, um jede Zeile mit ihren dazugehörigen Diagrammdaten zu verknüpfen. 

1. Ziehen Sie von der Registerkarte **Layout** ein Diagrammdatenraster in den Entwurfsbereich, und stellen Sie es auf die gewünschte Größe ein.

2. Wählen Sie auf der Registerkarte **Daten** im Bereich **Spalten** die Option **Diagrammpalte hinzufügen** aus. 

3. Rufen Sie [Daten aus Excel oder einem freigegebenen Dataset](../../reporting-services/mobile-reports/data-for-reporting-services-mobile-reports.md) ab, um eine zweite Datentabelle hinzuzufügen, die ein Feld mit der Hauptdatentabelle gemeinsam nutzt, falls noch nicht geschehen.

4. Wählen Sie unter **Dateneigenschaften** in **Daten für die Rasteransicht** die Hauptdatentabelle und anschließend in **Referenzdaten für Diagrammvisualisierungen** die zweite Tabelle aus.

5. Wählen Sie **Optionen** und dann **Diagrammtyp** aus.
 
6. Wählen Sie **Diagrammdatenfeld** , **Quellensuche** und **Zielsuche** aus. 
   Diese drei Eigenschaften bestimmen, wie das Datenraster Daten für jedes Diagramm in der Spalte bereitstellt.
   
   *   **Quellensuche** ist auf ein Feld in der Datentabelle in **Daten für die Rasteransicht** festgelegt. Dieses Feld fungiert als Zeilenfilter, der auf die Referenzdatentabelle des Diagramms angewendet wird, um dem eingebetteten Diagramm für jede Zeile Daten bereitzustellen. 
   * **Zielsuche** ist das Feld in der Datentabelle **Referenzdaten für Diagrammvisualisierungen**. Die Daten für das Diagramm in den einzelnen Zeilen werden in diesen beiden Feldern verknüpft.   
   * **Diagrammdatenfeld** bestimmt, welche Metrik in der Datentabelle **Referenzdaten für Diagrammvisualisierungen** als Wert oder Reihe der Y-Achse im Diagramm in den einzelnen Zeilen verwendet wird.  

## <a name="see-also"></a>Weitere Informationen 
* [Karten in mobilen Reporting Services-Berichten](../../reporting-services/mobile-reports/maps-in-reporting-services-mobile-reports.md)
* [Navigationen in mobilen Reporting Services-Berichten](../../reporting-services/mobile-reports/add-navigators-to-reporting-services-mobile-reports.md)
* [Visualisierungen in mobilen Reporting Services-Berichten](../../reporting-services/mobile-reports/add-visualizations-to-reporting-services-mobile-reports.md)
* [Messgeräte in mobilen Reporting Services-Berichten](../../reporting-services/mobile-reports/add-gauges-to-mobile-reports-reporting-services.md)  
 
  
