---
title: 'Problembehandlung bei Berichten: Kartenberichte (Berichts-Generator) | Microsoft-Dokumentation'
description: Informieren Sie sich über die Idenn für das Behandeln von Problemen, die beim Anpassen, Hinzufügen einer Ebene oder Veröffentlichen eines Berichts mit einer Karte im Berichts-Generator auftreten können.
ms.date: 07/22/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: a690aec2-056b-40bc-8cab-c694bd2d6d62
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a4580559356869ffb52d75241219e42c8511ba3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081161"
---
# <a name="troubleshoot-reports-map-reports-report-builder-and-ssrs"></a>Problembehandlung bei Berichten: Kartenberichte (Berichts-Generator und SSRS)
  Probleme mit Karten in einem paginierten [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] -Bericht können auftreten, wenn Sie dem Bericht eine Karte oder eine Kartenebene hinzufügen, wenn Sie im Bericht eine vorhandene Karte oder eine Kartenebene anpassen, wenn Sie eine Karte in einem Bericht in der Vorschau anzeigen oder wenn Sie einen Bericht mit einer Karte veröffentlichen. Dieses Thema soll Ihnen beim Behandeln der folgenden Probleme helfen.  
    
   ## <a name="need-more-help"></a>Benötigen Sie weitere Hilfe?  
   
  Versuchen Sie Folgendes:  
 * [SQL Server Reporting Services](https://stackoverflow.com/questions/tagged/reporting-services) auf Stack Overflow  
 * Melden Sie ein Problem, oder machen Sie einen Vorschlag unter [Microsoft SQL Server UserVoice](https://feedback.azure.com/forums/908035-sql-server).  

  
##  <a name="report-definition-size-issues"></a><a name="Embedded"></a> Probleme bei der Berichtsdefinitionsgröße  
 Verwenden Sie diesen Abschnitt, um Probleme zu lösen, die sich auf die Berichtsdefinitionsgröße beziehen.  
  
## <a name="how-do-i-reduce-the-report-definition-size"></a>Wie reduziere ich die Berichtsdefinitionsgröße?  
 Eine Kartenebene enthält Kartenelemente, die aus räumlichen Daten erstellt werden. In einigen Fällen werden Kartenelemente in die Berichtsdefinition eingebettet. Dies wird wie folgt durchgeführt:  
  
-   Wenn die Quelle räumlicher Daten eine Karte im Kartenkatalog oder eine ESRI-Shape-Datei auf dem lokalen Computer ist, werden Kartenelemente automatisch in die Berichtsdefinition eingebettet.  
  
     Wenn Sie einen Bericht auf dem Berichtsserver veröffentlichen und ein Verweis auf eine lokale Datei als räumliche Datenquelle vorhanden ist, können die räumlichen Daten nicht zur Verarbeitungszeit des Berichts abgerufen werden. Um dieses Problem zu vermeiden, werden die Kartendaten in die Berichtsdefinition eingebettet.  
  
-   Wenn Sie im Karten-Assistenten oder Ebenen-Assistenten die Option aktivieren, räumliche Daten einzubetten, werden Kartenelemente auf der Grundlage der räumlichen Daten in die Kartenebene in der Berichtsdefinition eingebettet.  
  
-   Wenn Sie im Bereich „Karte“ mit der rechten Maustaste auf die Ebene klicken und anschließend auf eine der Optionen unter **Räumliche Daten einbetten** klicken, werden Kartenelemente auf der Grundlage der räumlichen Daten in die Kartenebene in der Berichtsdefinition eingebettet.  
  
-   Um auf einer ESRI-Shape-Datei basierende eingebettete Daten aus einer Berichtsdefinition zu entfernen, gehen Sie wie folgt vor:  
  
1.  Laden Sie die SHP-Datei und die DBF-Datei von ESRI auf den Berichtsserver hoch, oder veröffentlichen Sie sie.  
  
2.  Wählen Sie im Bericht im Bereich Karte in der Entwurfsansicht die Ebene aus, die eingebettete Daten enthält, und öffnen Sie das Eigenschaften für die **Ebenendaten** . Wählen Sie unter **Räumliche Daten verwenden** den Eintrag **Mit ESRI-Shape-Datei (.shp) verknüpfen** aus, und wechseln Sie dann zu dem Ordner auf dem Berichtsserver, der die ESRI-Shape-Dateien enthält, wählen Sie sie aus, und klicken Sie auf OK.  
  
3.  Speichern Sie den Bericht. Die eingebetteten Daten für die Ebene, die Sie geändert haben, wurden aus der Berichtsdefinition entfernt.  
  
 Kartenelemente aus einem Bericht im Kartenkatalog werden immer in eine Kartenebene eingebettet.  
  
##  <a name="spatial-data-issues"></a><a name="Spatial"></a> Probleme bei räumlichen Daten  
 Verwenden Sie diesen Abschnitt, um Probleme zu lösen, die sich auf räumliche Daten beziehen.  
  
## <a name="on-the-design-surface-i-see-sample-spatial-data"></a>Auf der Entwurfsoberfläche werden räumliche Beispieldaten angezeigt  
 Zur Entwurfszeit kann auf der Entwurfsoberfläche die Meldung zu räumlichen Beispieldaten aus den folgenden Gründen angezeigt werden:  
  
-   Räumliche Daten stammen aus einer SHP-Datei von ESRI, aber die entsprechende DBF-Datei ist nicht verfügbar. ESRI-Shape-Dateien schließen im Allgemeinen eine SHP-Datei mit räumlichen Daten und eine DBF-Unterstützungsdatei ein. Überprüfen Sie, ob sich die DBF-Datei im selben Verzeichnis befindet wie die SHP-Datei.  
  
-   Räumliche Daten stammen aus einem Dataset, und die Datenverbindung für die Abfrage ist ungültig, oder die aktuellen Anmeldeinformationen sind ungültig.  
  
-   Die Kartenebene enthält eine Eigenschaft mit einem Ausdruck. Ausdrücke werden erst ausgewertet, wenn der Bericht ausgeführt wird. Um die Karte anzuzeigen, müssen Sie den Bericht in der Vorschau anzeigen.  
  
-   Räumliche Daten stammen aus einem Dataset, das einen bestimmten Bereich aufweist. Wenn eine Karte beispielsweise in einem Tablix-Datenbereich geschachtelt ist oder denselben Datasetbereich für analytische und für räumliche Daten verwendet, wird der Datenbereich erst berechnet, wenn der Bericht ausgeführt wird.  
  
## <a name="when-i-set-an-offset-for-an-individual-map-element-a-cluster-of-map-elements-move"></a>Beim Festlegen eines Offsets für ein einzelnes Kartenelement wird eine Gruppe von Kartenelementen verschoben  
 Räumliche Daten definieren die Kartenelemente, die auf jeder Kartenebene angezeigt werden. Ein Kartenelement kann auf räumlichen Daten basieren, die ein einzelner Punkt sind, ein Satz von Punkten, eine einzelne Linie, ein Satz von Linien, ein einzelnes Polygon oder ein Satz von Polygonen. Jedes Kartenelement ist eine Einheit. Wenn ein Kartenelement mehrere Punkte enthält und Sie das Element verschieben, werden alle Punkte für dieses Kartenelement ebenfalls verschoben.  
  
 Die Daten für jedes Kartenelement werden vom Format der räumlichen Daten aus der externen Quelle bestimmt. Wenn eine Abfrage z. B. räumliche Daten aus einer SQL Server-Datenbank angibt, kann jede Zeile im Resultset mehrere Sätze von Punkt-, Linien- oder Polygonkoordinaten enthalten. Alle Kartenelemente, die von einer einzelnen Zeile im Resultset definiert werden, werden als Einheit behandelt. Wenn Sie die Anzeige von bestimmten Sätzen von Koordinaten verändern möchten, führen Sie einen der folgenden Schritte aus:  
  
-   Ändern Sie die Abfrage so, dass die Koordinatensätze im Resultset als separate Zeilen zurückgegeben werden.  
  
-   Wählen Sie die zu verändernden Kartenelemente aus, und legen Sie die Eigenschaften des entsprechenden eingebetteten Punkts, der Linie oder des Polygons fest, indem Sie die Standardanzeigeeigenschaften für den entsprechenden Ebenentyp überschreiben.  
  
## <a name="my-layer-that-uses-spatial-data-from-an-esri-shapefile-always-has-embedded-data"></a>In eine Ebene, die räumliche Daten aus einer ESRI-Shape-Datei verwendet, sind immer Daten eingebettet  
 Um sicherzustellen, dass Berichte mit Karten auf einem Berichtsserver ausgeführt werden können, müssen ESRI-Shape-Dateien als Ressourcen auf dem Berichtsserver verfügbar sein. Wenn Sie einer Karte eine Ebene hinzufügen und eine Shape-Datei im lokalen Dateisystem angeben, werden die räumlichen Daten automatisch in den Bericht eingebettet.  
  
 Um die eingebetteten Daten durch einen Link zur ESRI-Shape-Datei zu ersetzen, müssen Sie die SHP-Datei von ESRI und die zugehörige DBF-Datei auf den Berichtsserver hochladen und dann die Quelle räumlicher Daten für die Ebene ändern.  
  
## <a name="i-renamed-a-data-source-or-dataset-to-a-friendly-name-and-now-no-data-appears-in-my-map"></a>Nach dem Umbenennen einer Datenquelle oder eines Dataset auf einen Anzeigenamen werden keine Daten auf der Karte angezeigt.  
 Die Berichtsdefinition wird nicht automatisch aktualisiert, wenn Sie den Namen eines Berichtselements manuell ändern.  
  
 Wenn Sie den Namen eines Datasets ändern, müssen alle Datenbereiche oder Kartenebenen, die auf dieses Dataset verweisen, manuell aktualisiert werden. Um ein Tablix, Diagramm oder Messgerät erneut an ein Dataset zu binden, wählen Sie das Element auf der Entwurfsoberfläche aus, öffnen Sie die Datenbereichseigenschaften, und wählen Sie den Namen des entsprechenden Datasets aus. Um eine Kartenebene erneut an ein Dataset zu binden, wählen Sie die Ebene aus, öffnen Sie die Ebeneneigenschaften, und wählen Sie den Namen des entsprechenden Datasets aus.  
  
## <a name="my-spatial-data-contains-nulls-and-empty-strings"></a>Räumliche Daten enthalten NULL-Werte und leere Zeichenfolgen.  
 In räumlichen Daten für das Kartenberichtselement werden NULL-Werte als 0 (null) und leeren Zeichenfolge als Leerwert ("") festgelegt.  
  
 Um dieses Verhalten für räumliche Daten aus einer SQL Server-Datenbank zu ändern, ändern Sie die Abfrage, die die räumlichen Daten zurückgibt.  
  
## <a name="my-map-exceeds-the-maximum-number-of-spatial-elements"></a>Die Karte enthält mehr als die maximale Anzahl räumlicher Elemente  
 Standardmäßig kann eine Karte 20.000 Kartenelemente oder 1.000.000 Punkte enthalten. Wenn die Karte diese Grenzen überschreitet, können Sie einen der folgenden Ansätze verwenden:  
  
-   Entfernen einer Ebene  
  
-   Verringern der Kartenauflösung  
  
-   Reduzieren der Kartenviewportkoordinaten, um einen kleineren Bereich anzuzeigen  
  
-   Wenn die räumlichen Daten aus einem Berichtsdataset stammen, legen Sie einen Filter fest, um die Daten aus dem Dataset einzuschränken. Der Filter muss für ein Feld festgelegt werden, das keinen räumlichen Datentyp aufweist.  
  
-   Wenn die räumlichen Daten aus einer SQL Server-Datenbank stammen, ändern Sie die Abfrage so, dass sie räumliche Funktionen verwendet, die die Daten auf einen kleineren Bereich beschränken.  
  
##  <a name="viewport-center-and-view-issues"></a><a name="Viewport"></a> Probleme beim Zentrieren und Anzeigen des Viewports  
 Verwenden Sie diesen Abschnitt, um Probleme zu lösen, die sich auf Viewportoptionen beziehen.  
  
## <a name="i-cannot-set-the-center-and-view-on-an-embedded-map-element"></a>Zentrierung und Ansicht können nicht auf ein eingebettetes Kartenelement festgelegt werden  
 Um einen Viewport auf ein bestimmtes Kartenelement zu zentrieren, müssen Sie die räumlichen Daten in einer Ebene analytischen Daten zugeordnet haben.  
  
## <a name="i-set-the-map-center-and-view-in-my-report-when-i-reopen-the-report-why-isnt-the-map-view-the-same"></a>Im Bericht sind Zentrierung und Ansicht für die Karte festgelegt. Beim erneuten Öffnen des Berichts ist die Kartenansicht anders.  
 Wenn die Benutzeranmeldeinformationen, die zum Lesen räumlicher Daten benötigt werden, beim Öffnen des Berichts nicht verfügbar sind, werden räumliche Platzhalterdaten verwendet. Abhängig von den für den Kartenviewport festgelegten Zentrier- und Zoomoptionen ist die Kartenansicht möglicherweise auf eine andere Ebene zentriert.  
  
 Klicken Sie mit der rechten Maustaste auf den Kartenviewport, und klicken Sie anschließend auf **Neu laden**, um die räumlichen Daten erneut zu laden und den im Bericht gespeicherten Kartenansicht-Mittelpunkt zu verwenden. Nach dem Eingeben der Anmeldeinformationen für die räumliche Datenquelle werden die räumlichen Daten in die Ebene geladen, und die Kartenansicht wird wiederhergestellt.  
  
## <a name="the-center-and-view-for-a-map-layer-option-does-not-work"></a>Zentrierung und Ansicht für eine Kartenebenenoption zeigen kein erwartungsgemäßes Verhalten  
 Wenn der Mittelpunkt des Viewports auf die räumlichen Daten für eine bestimmte Ebene festgelegt ist und der Mittelpunkt der Ansicht augenscheinlich nicht Mittelpunkt der Ebene ist, sind wahrscheinlich kleine Inseln oder Bereiche in den räumlichen Daten enthalten, die zu klein sind, um im Viewport gesehen zu werden. Beispielsweise können räumliche Daten für ein Land kleine Inseln oder andere kleine Gebiete enthalten. Der Viewport berechnet den Mittelpunkt aller räumlichen Daten für die Ebene.  
  
 Um Berechnungen für die Ebene zu überschreiben, können Sie eine der folgenden Aktionen ausführen:  
  
-   Geben Sie einen benutzerdefinierten Mittelpunkt für den Viewport an.  
  
-   Ändern Sie die Zoomstufe, damit der Viewport die Orte ausschließt, die Sie nicht einschließen möchten.  
  
-   Betten Sie die räumlichen Daten in den Bericht ein, und löschen Sie die Orte, die Sie nicht einschließen möchten.  
  
##  <a name="layer-issues"></a><a name="Layers"></a> Probleme bei Ebenen  
 Verwenden Sie diesen Abschnitt, um Probleme zu lösen, die sich auf Ebenenoptionen beziehen.  
  
## <a name="i-do-not-see-one-or-more-layers-in-my-map"></a>Mindestens eine Ebene wird nicht in der Karte angezeigt  
 Ob eine Kartenebene in einem Bericht angezeigt wird, hängt von der Verfügbarkeit der räumlichen Daten, der Beziehung zwischen den räumlichen Daten und den analytischen Daten, dem Typ der räumlichen Daten und dem Typ der entsprechenden Ebene, den Sichtbarkeits- und Transparenzoptionen für die Ebene sowie von der Zeichnungsreihenfolge der Ebene ab. Wenn Daten aus einer Ebene nicht angezeigt werden, überprüfen Sie die folgenden Optionen:  
  
-   **Ebenentyp und räumlicher Datentyp.** Der Ebenentyp bestimmt, welche räumlichen Daten angezeigt werden. Wenn der Ebenentyp beispielsweise Punkt lautet, aber die räumlichen Daten Linien sind, werden keine Daten angezeigt.  
  
-   **Werte von Übereinstimmungsfeldern.** Die Werte in den Feldern, die Sie angeben, um analytische Daten und räumliche Daten zu verknüpfen, müssen jedes Kartenelement eindeutig identifizieren. Die Felder müssen den gleichen Datentyp aufweisen. Die Werte in den Feldern müssen identisch sein. Weitere Informationen finden Sie unter [Probleme bei Legende, Farbskala und Entfernungsskala](#Legend).  
  
-   **Ebenenreihenfolge.** Die Reihenfolge der Ebenen im Bereich Karte ist die Reihenfolge, in der die Ebenen im Berichtsrenderer gezeichnet werden. Räumliche Daten auf Ebenen, die zuerst gezeichnet werden, könnten von räumlichen Daten für später gezeichnete Ebenen überschrieben werden. Ebenen, die in der Liste oben angezeigt werden, werden zuerst gezeichnet. Wenn Sie die Reihenfolge der Ebenen in der Liste ändern, ändern Sie die Zeichnungsreihenfolge der Ebenen.  
  
-   **Transparenz.** Sie können die Transparenz für jede Kartenebene unabhängig angeben. Die Standardwerte für die Transparenz hängen davon ab, wie Sie eine Ebene hinzufügen. Eine Transparenz von 0 % bedeutet, dass die Ebene nicht transparent ist und keine Daten anderer Ebenen durch sie hindurchscheinen. Damit andere Daten durch eine vorhandene Ebene durchscheinen können, legen Sie den Wert auf einen höheren Prozentsatz fest, der den gewünschten Effekt ergibt.  
  
-   **Sichtbarkeit.** Die Sichtbarkeit für eine Ebene ist entweder **Sichtbar**, **Ausgeblendet** oder **Zoombasiert**, abhängig von der Zoomstufe des Kartenviewports. Der maximale und minimale Bereich für die Zoomstufe kann ebenfalls angegeben werden. Die Sichtbarkeit kann auf einem Ausdruck basieren, der einen dieser Werte ergibt.  
  
    > [!TIP]  
    >  Sie können die Sichtbarkeit für jede Ebene im Bereich Karte umschalten. Während Sie eine Ebene entwerfen, schalten Sie die Sichtbarkeit aller anderen Ebenen aus, um zu bestimmen, ob das Problem eine einzelne Ebene oder die Transparenz zwischen Ebenen betrifft.  
  
## <a name="i-set-a-filter-on-the-map-layer-and-it-has-no-effect"></a>Ein auf der Kartenebene festgelegter Filter hat keine Auswirkungen.  
 Um Daten für eine Ebene zu filtern, muss der Datentyp im Filterausdruck angegeben werden. Überprüfen Sie, ob Sie den richtigen zugrunde liegenden Datentyp angegeben haben, damit die Filterformel die angegebene Bedingung ordnungsgemäß auswertet. Weitere Informationen finden Sie unter [Beispiele für Filtergleichungen &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md).  
  
##  <a name="legend-color-scale-and-rule-issues"></a><a name="Legend"></a> Probleme bei Legende, Farbskala und Regeln  
 Verwenden Sie diesen Abschnitt, um Probleme zu lösen, die sich auf Optionen für Regeln, Legende und Farbskala beziehen.  
  
## <a name="how-do-i-control-the-values-in-the-map-legend"></a>Wie können die Werte in der Kartenlegende gesteuert werden?  
 Legendenwerte werden automatisch von Kartenelementtyp-Regeln bestimmt, die Sie für jede Kartenebene angeben, und von Verteilungsregeln, die Sie für die Legende angeben.  
  
 Standardmäßig werden alle von allen Regeln generierten Elemente in der ersten Legende angezeigt. Werte für alle Polygon-, Linien- und Punktregeln für jede Ebene tragen zum kombinierten Legendenbereich bei. Um Elemente in unterschiedlichen Legenden anzuzeigen, müssen Sie zuerst mehrere Legenden erstellen und dann für jede Regel angeben, in welcher Legende die entsprechenden Elemente angezeigt werden sollen.  
  
 Um einer bestimmten Legende eine Regel zuzuordnen, öffnen Sie die Regeleigenschaften, und wählen Sie auf der Seite "Legende" den Namen der zu verwendenden Legende aus. Um Elemente aus einer Legende zu entfernen, wählen Sie in den Legendenoptionen für den Namen der Legende die Leerzeile aus. Wenn Sie Legendenelemente im Bericht umbenennen, müssen Sie jede Ebene dem entsprechenden Legendenelement manuell zuordnen.  
  
 Um den Titel und den Inhalt für jede Legende zu steuern, verwenden Sie die Legendeneigenschaften für die Regel. Sie können angeben, wie viele Teile erstellt werden sollen, die Berechnungen ändern, mit denen jedem Teil Werte zugewiesen werden, den minimalen und maximalen Bereichswert festlegen sowie das Format des Legendentexts ändern.  
  
 Weitere Informationen finden Sie unter [Ändern der Kartenlegenden, Farbskala und zugeordneten Regeln &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md).  
  
## <a name="the-rules-that-i-set-do-not-give-the-results-that-i-expect"></a>Die festgelegten Regeln ergeben nicht die erwarteten Ergebnisse.  
 Regeln werden auf die analytischen Daten angewendet, die Kartenelementen auf einer Ebene zugeordnet sind. Verwenden Sie die folgende Liste, um Probleme mit allen Farb-, Größen-, Breiten- und Markertypregeln zu identifizieren:  
  
-   Die Rangfolge zum Anwenden des Formats auf jedes Kartenelement (Polygon, Linie, Punkt) lautet (vom niedrigsten zum höchsten Rang): Ebeneneigenschaften, Kartenelementeigenschaften für alle Kartenelemente auf der Ebene, Regeln, die Sie angeben, und schließlich für eingebettete Kartenelemente mit aktivierter Überschreibungsoption die angegebenen Werte. Sobald Sie die Überschreibungsoption für ein eingebettetes Element aktivieren, gelten Regeln nicht mehr, auch wenn Sie später Werte auf ihre ursprüngliche Einstellung zurückändern.  
  
-   Probleme bei Übereinstimmungsfeldern. Übereinstimmungsfelder ermöglichen die Datenbindung zwischen Kartenelementen und analytischen Daten. Die Felder für räumliche und analytische Daten, die Übereinstimmungsfeldern entsprechen, müssen denselben Datentyp und dasselbe Format aufweisen. Wenn das Übereinstimmungsfeld nicht genau mit den räumlichen und analytischen Daten übereinstimmt, hat die Regel keine Auswirkungen. Wenn das Übereinstimmungsfeld für räumliche Daten im Vergleich zum Übereinstimmungsfeld für die analytischen Daten beispielsweise zusätzliche Leerzeichen oder zusätzliche Interpunktionszeichen aufweist, tritt keine Übereinstimmung auf.  
  
-   Weitere Informationen finden Sie unter [Unterschiedliche Polygon-, Linien- und Punktanzeigen bei der Verwendung von Regeln und analytischen Daten &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md).  
  
## <a name="what-is-the-value-nan-on-the-color-scale"></a>Was bedeutet der Wert "NaN" auf der Farbskala?  
 **NaN** steht für "Not a Number" (keine Zahl). Als Farbskalawerte werden numerische Werte erwartet. Überprüfen Sie die Verteilungseinstellungen und den Legendentextwert für die der Farbskala zugeordneten Regeln. Wenn Sie benutzerdefinierte Verteilungsbereiche erstellt haben, überprüfen Sie, ob Sie die Untergrenze für den ersten Bereich und die Obergrenze für den letzten Bereich angegeben haben.  
  
## <a name="my-color-scale-does-not-appear-when-i-run-the-report"></a>Die Farbskala wird beim Ausführen des Berichts nicht angezeigt.  
 Auf der Farbskala werden Informationen für den Benutzer angezeigt, wenn eine Kartenebene Farbregeln für Polygone, Linien oder Punkte für die gesamte Ebene oder für eingebettete Kartenelemente angibt. Wenn kein Kartenelement eine Farbregel angibt oder wenn die Farbregeln mit einer Legende statt der Farbzuordnung angegeben werden, dann wird die Farbzuordnung im gerenderten Bericht nicht angezeigt.  
  
 Um die Farbskala anzuzeigen, geben Sie Farbregeln für eine Ebene oder ein eingebettetes Kartenelement an. Weitere Informationen finden Sie unter [Ändern der Kartenlegenden, Farbskala und zugeordneten Regeln &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md).  
  
##  <a name="tile-issues"></a><a name="Tile"></a> Probleme bei Kacheln  
 Verwenden Sie diesen Abschnitt, um Probleme zu lösen, die sich auf Optionen für den Kachelhintergrund beziehen.  
  
## <a name="i-cannot-see-the-bing-maps-tile-background"></a>Der Hintergrund aus Bing Map-Kacheln wird nicht angezeigt.  
 Die folgenden Einstellungen beeinflussen, ob ein Hintergrund aus Bing Map-Kacheln in der lokalen Vorschau oder für einen Bericht, der vom Berichtsserver ausgeführt wird, angezeigt wird:  
  
-   Die Kartenkachelebene muss vorhanden sein. Wählen Sie im Karten-Assistenten oder Ebenen-Assistenten **Bing Maps-Hintergrund für diese Kartenansicht hinzufügen** aus. Dies fügt eine Kachelebene für den aktuellen Kartenviewport-Ansichtmittelpunkt und die aktuelle Zoomstufe hinzu. Sie können auch eine Kachelebene über die Symbolleiste des Bereichs Karte hinzufügen.  
  
-   Das Kartenkoordinatensystem für den Viewport muss **Geografisch** lauten, nicht **Planar**.  
  
-   Die Kartenprojektion muss **Mercator** sein.  
  
-   Um die lokale Vorschau anzuzeigen, müssen Sie über Internetzugriff verfügen. Für einen Bericht, der vom Berichtsserver ausgeführt wird, muss der Berichtsserver so konfiguriert sein, dass er den Kachelhintergrund unterstützt. Weitere Informationen finden Sie unter [Planen eines Kartenberichts](plan-a-map-report-report-builder-and-ssrs.md).  
  
 Weitere Informationen zum Hinzufügen einer Kachelebene finden Sie unter [Hinzufügen, Ändern oder Löschen einer Karte oder einer Kartenebene (Berichts-Generator und SSRS)](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md).  
  
## <a name="how-do-i-control-the-text-on-a-tile-layer"></a>Wie kann der Text in einer Kachelebene gesteuert werden?  
 Die Ansichten **Straße** und **Hybrid** enthalten Text. Der Text ist Teil der Kacheln, die von Bing Maps Web Services stammen.  
  
 Um eine Kachelebene ohne Text einzuschließen, wählen Sie die Ansicht **Luftbild** aus.  
  
##  <a name="tooltip-and-label-issues"></a><a name="Tooltip"></a> Probleme bei QuickInfo und Beschriftung  
 Verwenden Sie diesen Abschnitt, um Probleme zu behandeln, die sich auf Bezeichnungs- oder QuickInfo-Optionen beziehen.  
  
## <a name="i-get-an-expression-error-about-dataset-scope-when-i-set-a-label-or-tooltip-to-an-expression"></a>Wenn eine Bezeichnung oder eine QuickInfo auf einen Ausdruck festgelegt wird, wird ein Ausdrucksfehler bezüglich eines Datasetbereichs angezeigt.  
 Wenn die räumlichen Daten aus einem Kartenkatalog oder einer ESRI-Shape-Datei stammen, sind die zugeordneten Daten nicht Teil eines Berichtsdatasets. Sie können keine Ausdruckssyntax für einen Dataset-Feldverweis verwenden, um diese Daten für eine Bezeichnung oder eine QuickInfo anzugeben.  
  
 Um Daten anzugeben, die sich auf räumliche Daten beziehen, die nicht Teil eines Berichtsdatasets sind, müssen Sie das Symbol # gefolgt von einer Bezeichnung verwenden, die den Namen der Daten angibt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Karten &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)   
 [Problembehandlung in Berichts-Generator](/previous-versions/sql/2014/reporting-services/troubleshoot-report-builder?view=sql-server-2014&preserve-view=true)  
  
  
