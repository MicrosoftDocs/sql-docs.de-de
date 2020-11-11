---
title: 'Tutorial: Hinzufügen einer KPI zu einem Bericht (Berichts-Generator) | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie einem paginierten Bericht der Reporting Services im Berichts-Generator einen Key Performance Indicator (KPI) hinzufügen.
ms.date: 06/15/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 1bf77859-0b33-4f40-abaf-ebeeb6ebb1f8
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f4802ee3beb72e34ed2b85e8857ac69c61557018
ms.sourcegitcommit: 9e2c682929ee64c051dc62f8917d147861f7c635
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043708"
---
# <a name="tutorial-adding-a-kpi-to-your-report-report-builder"></a>Tutorial: Hinzufügen eines KPIs zu einem Bericht (Berichts-Generator)
In diesem [!INCLUDE[ssRBnoversion_md](../includes/ssrbnoversion.md)]-Tutorial fügen Sie eine Leistungskennzahl (Key Performance Indicator; KPI) einem paginierten [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)]-Bericht hinzu.  

KPIs sind für Unternehmen bedeutende, messbare Werte. In diesem Szenario ist die Verkaufszusammenfassung nach Produktunterkategorien der KPI. Der aktuelle Status der KPI wird mithilfe von Farben, Messgeräten und Indikatoren angezeigt.
  
Die folgende Abbildung ähnelt dem Bericht, den Sie erstellen werden.  
  
![Screenshot: KPI-Bericht im Berichts-Generator](../reporting-services/media/report-builder-kpi-report.png)
    
> [!NOTE]  
> In diesem Lernprogramm werden die Schritte für den Assistenten in zwei Verfahren zusammengefasst: ein Verfahren zum Erstellen des Datasets und ein Verfahren zum Erstellen einer Tabelle. Im ersten Tutorial dieser Reihe erhalten Sie ausführliche Anweisungen zum Navigieren zu einem Berichtsserver, Auswählen einer Datenquelle, Erstellen eines Datasets und Ausführen des Assistenten: [Tutorial: Erstellen eines einfachen Tabellenberichts (Berichts-Generator)](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md).  
  
Geschätzte Zeit zum Bearbeiten dieses Tutorials: 15 Minuten  
  
## <a name="requirements"></a>Requirements (Anforderungen)  
Weitere Informationen zu den Anforderungen finden Sie unter [Voraussetzungen für Tutorials &#40;Berichts-Generator&#41;](../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
## <a name="1-create-a-table-report-and-dataset-from-the-table-or-matrix-wizard"></a><a name="Table"></a>1. Erstellen eines Tabellenberichts und eines Datasets mit dem Tabellen- oder Matrix-Assistenten  
In diesem Abschnitt wählen Sie eine freigegebene Datenquelle aus, erstellen ein eingebettetes Dataset und zeigen die Daten in einer Tabelle an.  
 
### <a name="to-create-a-table-with-an-embedded-dataset"></a>So erstellen Sie eine Tabelle mit einem eingebetteten Dataset  
  
1.  [Starten Sie den Berichts-Generator](../reporting-services/report-builder/start-report-builder.md) entweder von Ihrem Computer, über das [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] -Webportal oder über den integrierten SharePoint-Modus.  
  
    Das Dialogfeld **Neuer Bericht oder neues Dataset** wird geöffnet.  
  
    Wenn das Dialogfeld **Neuer Bericht oder neues Dataset** nicht angezeigt wird, wählen Sie im Menü **Datei** die Option **Neu**.  
  
2.  Vergewissern Sie sich, dass im linken Bereich **Neuer Bericht** ausgewählt ist.  
  
3.  Klicken Sie im rechten Bereich auf **Tabellen- oder Matrix-Assistent**.  
  
4.  Klicken Sie auf der Seite **Dataset auswählen** auf **Dataset erstellen**.  
  
5.  Klicken Sie auf **Weiter**.  
  
6.  Wählen Sie auf der Seite **Verbindung mit einer Datenquelle auswählen** eine vorhandene Datenquelle aus, oder navigieren Sie zum Berichtsserver, und wählen Sie eine Datenquelle aus. Falls dort keine Datenquelle verfügbar ist oder Sie keinen Zugriff auf einen Berichtsserver haben, können Sie stattdessen eine eingebettete Datenquelle verwenden. Weitere Informationen finden Sie im [Tutorial: Erstellen eines einfachen Tabellenberichts &#40;Berichts-Generator&#41;](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md).  
  
7.  Klicken Sie auf **Weiter**.  
  
8.  Klicken Sie auf der Seite **Abfrage entwerfen** auf **Als Text bearbeiten**.  
  
9. Kopieren Sie die folgende Abfrage, und fügen Sie sie in den Abfragebereich ein:  

    > [!NOTE]  
    > In diesem Lernprogramm sind die Datenwerte in der Abfrage enthalten, sodass keine externe Datenquelle benötigt wird. Die Abfrage ist daher relativ lang. In einer Geschäftsumgebung wären die Daten nicht in der Abfrage enthalten. Dieses Szenario dient nur zu Lernzwecken.  
  
    ```  
    SELECT CAST('2015-01-05' AS date) as SalesDate, 'Accessories' as Subcategory,   
       'Carrying Case' as Product, CAST(16996.60 AS money) AS Sales, 68 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Tripod' as Product, CAST(1350.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2015-01-11' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Lens Adapter' as Product, CAST(1147.50 AS money) AS Sales, 17 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Accessories' as Subcategory,  
       'Mini Battery Charger' as Product, CAST(1056.00 AS money) AS Sales, 44 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate,  'Accessories' as Subcategory,  
       'Telephoto Conversion Lens' as Product, CAST(1380.00 AS money) AS Sales, 18 as Quantity  
    UNION SELECT CAST('2015-01-06' AS date) as SalesDate,'Accessories' as Subcategory,    
       'USB Cable' as Product, CAST(780.00 AS money) AS Sales, 26 as Quantity  
    UNION SELECT CAST('2015-01-08' AS date) as SalesDate, 'Accessories' as Subcategory,   
       'Budget Movie-Maker' as Product, CAST(3798.00 AS money) AS Sales, 9 as Quantity  
    UNION SELECT CAST('2015-01-09' AS date) as SalesDate, 'Camcorders' as Subcategory,   
       'Business Videographer' as Product, CAST(10400.00 AS money) AS Sales, 13 as Quantity  
    UNION SELECT CAST('2015-01-10' AS date) as SalesDate, 'Camcorders' as Subcategory,   
       'Social Videographer' as Product, CAST(3000.00 AS money) AS Sales, 60 as Quantity  
    UNION SELECT CAST('2015-01-11' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Advanced Digital' as Product, CAST(7234.50 AS money) AS Sales, 39 as Quantity  
    UNION SELECT CAST('2015-01-07' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Compact Digital' as Product, CAST(10836.00 AS money) AS Sales, 84 as Quantity  
    UNION SELECT CAST('2015-01-08' AS date) as SalesDate,  'Digital' as Subcategory,   
       'Consumer Digital' as Product, CAST(2550.00 AS money) AS Sales, 17 as Quantity  
    UNION SELECT CAST('2015-01-05' AS date) as SalesDate, 'Digital' as Subcategory,   
       'Slim Digital' as Product, CAST(8357.80 AS money) AS Sales, 44 as Quantity  
    UNION SELECT CAST('2015-01-09' AS date) as SalesDate, 'Digital SLR' as Subcategory,   
       'SLR Camera 35mm' as Product, CAST(18530.00 AS money) AS Sales, 34 as Quantity  
    UNION SELECT CAST('2015-01-07' AS date) as SalesDate, 'Digital SLR' as Subcategory,   
       'SLR Camera' as Product, CAST(26576.00 AS money) AS Sales, 88 as Quantity  
    ```  
  
10. Klicken Sie auf der Symbolleiste des Abfrage-Designers auf „Ausführen“ ( **!** ).

11. Klicken Sie auf **Weiter**.  
  
## <a name="2-organize-data-and-choose-layout-in-the-wizard"></a><a name="CompleteWizard"></a>2. Organisieren von Daten und Auswählen des Layouts im Assistenten  
Der Tabellen- oder Matrix-Assistent stellt ein erstes Design für die Darstellung von Daten bereit. Im Vorschaufenster des Assistenten können Sie das Ergebnis der Datengruppierung visualisieren, bevor Sie den Tabellen- oder Matrixentwurf abschließen.  
  
### <a name="to-organize-data-into-groups-and-choose-a-layout"></a>So organisieren Sie Daten in Gruppen und wählen ein Layout aus 
  
1.  Ziehen Sie auf der Seite „Felder anordnen“ das Feld „Product“ in **Werte**.  
  
2.  Ziehen Sie „Quantity“ in **Werte** , und platzieren Sie es unter „Product“.  
  
    Die Menge wird mit der Sum-Funktion zusammengefasst, der Standardfunktion zum Summieren numerischer Felder.  
  
3.  Ziehen Sie „Sales“ in **Werte** , und fügen Sie dieses Feld unter „Quantity“ ein.  
  
    In Schritt 1, 2 und 3 werden die Daten angegeben, die in der Tabelle angezeigt werden sollen.  
  
4.  Ziehen Sie „SalesDate“ in **Zeilengruppen**.  
  
5.  Ziehen Sie „Subcategory“ in **Zeilengruppen** , und fügen Sie dieses Feld unter „SalesDate“ ein.  
  
    Durch die Schritte 4 und 5 werden die Werte für die Felder zuerst nach Datum und dann nach allen Umsätzen für dieses Datum angeordnet.  
  
6.  Klicken Sie auf **Weiter**.  
  
    Bei der Ausführung des Berichts werden in der Tabelle jedes Datum, alle Aufträge für jedes Datum sowie alle Produkte, Mengen und Umsatzsummen für jeden Auftrag angezeigt.  
  
7.  Vergewissern Sie sich auf der Seite „Layout auswählen“, dass unter **Optionen** die Option **Teil- und Gesamtergebnisse anzeigen** ausgewählt ist.  
  
8.  Überprüfen Sie, ob **Als Block, Teilergebnis unterhalb** ausgewählt ist.  
  
9. Deaktivieren Sie die Option **Gruppen erweitern/reduzieren**.  
  
    In diesem Lernprogramm enthält der erstellte Bericht keine Drilldownfunktion, mit der ein Benutzer eine übergeordnete Gruppenhierarchie einblenden kann, um untergeordnete Gruppenzeilen und Detailzeilen anzuzeigen.  
  
10. Klicken Sie auf **Weiter**.  
  
11. Klicken Sie auf **Fertig stellen**.  
  
      Die Tabelle wird der Entwurfsoberfläche hinzugefügt. Die Tabelle enthält fünf Spalten und fünf Zeilen. Der Bereich „Zeilengruppen“ umfasst drei Zeilengruppen: SalesDate, Subcategory und Details. Detaildaten sind alle Daten, die von der Datasetabfrage abgerufen werden. Der Bereich „Spaltengruppen“ ist leer.  
      
      ![Screenshot von Zeilengruppen](../reporting-services/media/report-builder-kpi-row-groups.png)
  
12. Klicken Sie auf **Ausführen** , um eine Vorschau des Berichts anzuzeigen.  
  
Für jedes an einem bestimmten Datum verkaufte Produkt werden in der Tabelle der Produktname, die verkaufte Menge und der Gesamtumsatz angezeigt. Die Daten sind zuerst nach Verkaufsdatum und dann nach Unterkategorie organisiert. 

![Screenshot: einfache KPI-Tabelle im Berichts-Generator](../reporting-services/media/report-builder-kpi-basic-table.png)
    
### <a name="format-dates-and-currency"></a>Formatieren von Datumsangaben und Währung
Erweitern Sie die Spalten, und legen Sie das Format für die Datums- und Währungsangaben fest.

1. Klicken Sie auf **Entwurf** , um wieder zur Entwurfsansicht zurückzuwechseln.

2. Die Produktnamen könnten mehr Platz benötigen. Um die Spalte „Product“ breiter zu gestalten, wählen Sie die gesamte Tabelle aus, und ziehen Sie den rechten Rand des Spaltenziehpunkts am oberen Rand der Product-Spalte.

3. Drücken Sie die STRG-TASTE, und wählen Sie anschließend die vier Zellen aus, die [Sum(Sales)] enthalten.

4. Klicken Sie auf der Registerkarte **Start** unter **Number** (Zahl) auf  > **Currency** (Währung). Die Zellen ändern sich, um die formatierte Währung anzuzeigen.

   Wenn Sie das Gebietsschema „Deutsch (Deutschland)“ verwenden, lautet der Standardbeispieltext [12.345,00€]. Falls kein Beispielwert für die Währung angezeigt wird, klicken Sie in der Gruppe **Zahlen** auf **Platzhalterformate** > **Beispielwerte**.
    
    ![Screenshot: Option „Beispielwerte“ im Berichts-Generator ausgewählt](../reporting-services/media/report-builder-placeholder-value-button.png)

5. (Optional) Klicken Sie auf der Registerkarte **Stamm** in der Gruppe **Zahl** zweimal auf die Schaltfläche **Dezimalstellen verringern** , um volle Dollarbeträge ohne Centangaben anzuzeigen.

6. Klicken Sie auf die Zelle, die [SalesDate] enthält.

6. Klicken Sie in der Gruppe **Number** (Zahl) auf **Date** (Datum).

   In der Zelle wird das Beispieldatum [1/31/2000] angezeigt. 

12. Klicken Sie auf **Ausführen** , um eine Vorschau des Berichts anzuzeigen.  
 
![Screenshot: KPI-Bericht mit formatierten Zahlen im Berichts-Generator](../reporting-services/media/report-builder-kpi-format-numbers.png)

## <a name="3-use-background-colors-to-display-a-kpi"></a><a name="BackgroundColors"></a>3. Anzeigen eines KPI mithilfe von Hintergrundfarben  
Hintergrundfarben können für einen Ausdruck festgelegt werden, der beim Ausführen des Berichts ausgewertet wird.  
  
### <a name="to-display-the-present-state-of-a-kpi-by-using-background-colors"></a>So zeigen Sie den aktuellen Status eines KPI mit Hintergrundfarben an  
  
1.  Klicken Sie in der Tabelle mit der rechten Maustaste die `[Sum(Sales)]` -Zelle (die Teilergebniszeile mit dem Umsatz für eine Unterkategorie), und klicken Sie anschließend auf **Textfeldeigenschaften**. 

    Stellen Sie sicher, dass Sie die Zelle (und nicht den darin enthaltenen Text) ausgewählt haben, um die **Textfeldeigenschaften** anzuzeigen. 
    
    ![Screenshot der Option „Textfeldeigenschaften“ im Berichts-Generator](../reporting-services/media/report-builder-text-box-properties.png)
  
2.  Klicken Sie auf der Registerkarte **Ausfüllen** auf die Schaltfläche **fx** neben der Option **Füllfarbe** , und geben Sie den folgenden Ausdruck in das Feld **Ausdruck festlegen für: BackgroundColor** ein:  
  
    `=IIF(Sum(Fields!Sales.Value) >= 5000 ,"Lime", IIF(Sum(Fields!Sales.Value) < 2500, "Red","Yellow"))`  
  
     Dadurch wird die Hintergrundfarbe für jede Zelle in Limonengrün geändert, die eine aggregierte Summe von `[Sum(Sales)]` enthält, die größer gleich 5.000 ist. Werte von `[Sum(Sales)]` zwischen 2.500 und 5.000 werden gelb eingefärbt. Werte kleiner als 2.500 werden rot eingefärbt.  
  
1.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
2.  Klicken Sie auf **Ausführen** , um eine Vorschau des Berichts anzuzeigen.  
  
In der Teilergebniszeile, die den Umsatz für eine Unterkategorie anzeigt, ist die Hintergrundfarbe der Zelle abhängig vom Wert der Umsatzsumme rot, gelb oder grün.  

![Screenshot: KPI-Bericht im Berichts-Generator mit Farben in bestimmten Zellen](../reporting-services/media/report-builder-kpi-colors.png)
  
## <a name="4-display-a-kpi-by-using-a-gauge"></a><a name="Gauge"></a>4. Anzeigen eines KPI mit einem Messgerät  
Ein Messgerät stellt einen einzelnen Wert in einem Dataset dar. In diesem Tutorial wird ein horizontales lineares Messgerät verwendet, da es aufgrund seiner Form und Einfachheit auch dann leicht zu lesen ist, wenn es klein ist und innerhalb einer Tabellenzelle verwendet wird. Weitere Informationen finden Sie unter [Messgeräte &#40;Berichts-Generator und SSRS&#41;](../reporting-services/report-design/gauges-report-builder-and-ssrs.md).  
  
### <a name="to-display-the-present-state-of-a-kpi-using-a-gauge"></a>So zeigen Sie den aktuellen Status eines KPI mit einem Messgerät an  
  
1.  Wechseln Sie zurück in die Entwurfsansicht.  
  
2.  Klicken Sie in der Tabelle mit der rechten Maustaste auf den Spaltenziehpunkt für die Spalte „Sales“ (Vertrieb) und anschließend auf **Spalte einfügen** > **Rechts**. Eine neue Spalte wird der Tabelle hinzugefügt.  

    ![Screenshot: Einfügen einer Spalte in den KPI-Bericht des Berichts-Generators](../reporting-services/media/report-builder-kpi-insert-column.png)
  
3.  Geben Sie in der Spaltenüberschrift **Lineare KPI** ein.  
  
4.  Klicken Sie auf der Registerkarte **Einfügen** auf **Datenvisualisierungen** > **Messgerät** und anschließend außerhalb der Tabelle auf die Entwurfsoberfläche.   
  
5.  Wählen Sie im Dialogfeld **Messgerättyp auswählen** den ersten linearen Messgerättyp ( **Horizontal** ) aus.  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    Der Entwurfsoberfläche wird ein Messgerät hinzugefügt.  
  
7.  Ziehen Sie im Bereich „Berichtsdaten“ das `Sales`-Feld in das Messgerät. Der Bereich **Messgerätdaten** wird geöffnet.  
  
    Wenn Sie das `Sales` -Feld auf dem Messgerät ablegen, wird es in der Liste **Werte** hinzugefügt und anhand der integrierten Sum-Funktion aggregiert.  
   
    ![Screenshot: Ziehen des Felds „Sales“ in einen KPI-Bericht des Berichts-Generators](../reporting-services/media/report-builder-kpi-drag-sales-field.png)
   
9. Klicken Sie im Bereich **Messgerätdaten** auf den Pfeil neben **LinearPointer1** > **Pointer Properties** (Zeigereigenschaften).  
  
10. Stellen Sie im Dialogfeld **Lineare Zeigereigenschaften** in der Registerkarte **Zeigeroptionen** unter **Zeigertyp** sicher, dass **Leiste** ausgewählt ist. 
 
11. Klicken Sie auf **OK**.  
  
12. Klicken Sie mit der rechten Maustaste auf die Skala im Messgerät, und klicken Sie auf **Skalierungseigenschaften**.  
  
13. Legen Sie im Dialogfeld **Lineare Skalierungseigenschaften** auf der Registerkarte **Allgemein** das **Maximum** auf 25.000 fest.  

    > [!NOTE]  
    > Anstelle einer Konstante wie 25.000 können Sie den Wert der Option **Maximum** auch mithilfe eines Ausdrucks dynamisch berechnen. Der Ausdruck würde das Aggregat der Aggregatfunktion verwenden und dem Ausdruck `=Max(Sum(Fields!Sales.value), "Tablix1")`ähneln.  

14. Aktivieren Sie auf der Registerkarte **Bezeichnungen** das Kontrollkästchen **Skalabezeichnungen ausblenden**.

15. Klicken Sie auf **OK**.
  
14. Ziehen Sie das Messgerät innerhalb der Tabelle in die zweite leere Zelle der Spalte „Lineare KPI“, und zwar in die Zeile, die das Teilergebnis „Vertrieb“ für das `Subcategory` -Feld anzeigt, und neben das Feld, in dem Sie die Formel für die Hintergrundfarbe hinzugefügt haben.  
  
    > [!NOTE]  
    > Möglicherweise müssen Sie die Größe der Spalte ändern, damit das horizontale lineare Messgerät in die Zelle passt. Um die Größe einer Spalte anzupassen, wählen Sie die Tabelle aus und ziehen den Spaltenziehpunkt. Daraufhin passt die Berichtsentwurf-Oberfläche die Größe an die Tabelle an.  
  
15. Klicken Sie auf **Ausführen** , um eine Vorschau des Berichts anzuzeigen.  
  
    Die horizontale Länge der grünen Leiste im Messgerät ändert sich je nach Wert der KPI.  
  
![Screenshot: eine lineare KPI-Spalte, die dem KPI-Bericht des Berichts-Generators hinzugefügt wurde](../reporting-services/media/report-builder-linear-kpi.png) 
  
## <a name="5-display-a-kpi-by-using-an-indicator"></a><a name="Indicator"></a>5. Anzeigen eines KPI mit einem Indikator  
Indikatoren sind kleine einfache Messgeräte, die Datenwerte auf einen Blick darstellen. Aufgrund ihrer Größe und Einfachheit werden Indikatoren oft in Tabellen und Matrizen verwendet. Weitere Informationen finden Sie unter [Indikatoren (Berichts-Generator und SSRS)](../reporting-services/report-design/indicators-report-builder-and-ssrs.md).  
  
### <a name="to-display-the-present-state-of-a-kpi-using-an-indicator"></a>So zeigen Sie den aktuellen Status eines KPI mit einem Indikator an  
  
1.  Wechseln Sie in die Entwurfsansicht.  
  
2.  Klicken Sie in der Tabelle mit der rechten Maustaste auf den Spaltenziehpunkt für die Spalte „Lineare KPI“, die Sie in der vorherigen Prozedur hinzugefügt haben, und anschließend auf **Spalte einfügen** > **Rechts**. Eine neue Spalte wird der Tabelle hinzugefügt.  
  
3.  Geben Sie in der Spaltenüberschrift **Ampel-KPI** ein.  
  
4.  Klicken Sie auf die Zelle für die Unterkategorie „Teilergebnis“, die sich neben dem linearen Messgerät befindet, das Sie in der vorherigen Prozedur hinzugefügt haben.  
  
5.  Doppelklicken Sie auf der Registerkarte **Einfügen** unter **Datenvisualisierungen** auf **Indikator**.  
  
6.  Wählen Sie im Dialogfeld **Indikatortyp auswählen** unter **Formen** den ersten Formtyp **3 Ampeln (ohne Rand)** aus.  
  
7.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
    Der Indikator wird der Zelle in der neuen Spalte „Stoplight KPI“ hinzugefügt.  
  
8.  Klicken Sie mit der rechten Maustaste auf den Indikator, und klicken Sie auf **Indikatoreigenschaften**.  
  
9. Wählen Sie auf der Registerkarte **Werte und Status** im Feld **Wert** **[Sum(Sales)]** aus. Ändern Sie keine weiteren Optionen.  
  
    Standardmäßig findet eine Datensynchronisierung im Datenbereich statt, und der Wert **Tablix1** , der Name des Tabellendatenbereichs im Bericht, wird im Feld **Synchronisierungsbereich** angezeigt.  
  
    In diesem Bericht können Sie auch den Bereich eines Indikators ändern, der in der Zelle für das Teilergebnis der Unterkategorie eingefügt wurde, um das Feld "SalesDate" zu synchronisieren.  
  
11. Klicken Sie auf **OK**.

11. Klicken Sie auf **Ausführen** , um eine Vorschau des Berichts anzuzeigen.  

![Screenshot: Spalte mit einem Ampel-KPI im KPI-Bericht des Berichts-Generators](../reporting-services/media/report-builder-kpi-stoplight.png)
  
## <a name="6-add-a-report-title"></a><a name="Title"></a>6. Hinzufügen eines Berichtstitels  
Ein Berichtstitel wird oben im Bericht angezeigt. Sie können den Berichtstitel in eine Berichtskopfzeile einfügen oder, wenn der Bericht keine Kopfzeile enthält, in einem Textfeld am oberen Rand des Berichtshauptteils. In diesem Abschnitt verwenden Sie das Textfeld, das automatisch am oberen Rand des Berichtshauptteils platziert wird.  
  
Sie können die Textdarstellung weiter verbessern, indem Sie verschiedene Schriftschnitte, Größen und Farben für Ausdrücke und einzelne Zeichen des Texts anwenden. Weitere Informationen finden Sie unter [Formatieren von Text in einem Textfeld (Berichts-Generator und SSRS)](../reporting-services/report-design/format-text-in-a-text-box-report-builder-and-ssrs.md).  
  
### <a name="to-add-a-report-title"></a>So fügen Sie einen Berichtstitel hinzu  
  
1.  Klicken Sie auf der Entwurfsoberfläche auf **Zum Hinzufügen eines Titels klicken**.  
  
2.  Geben Sie **Produktvertriebs-KPI** ein, und klicken Sie in den Bereich außerhalb des Textfelds.  
  
3.  Klicken Sie optional mit der rechten Maustaste auf das Textfeld mit dem Eintrag **Product Sales KPI** , klicken Sie auf **Textfeldeigenschaften** , und wählen Sie auf der Registerkarte „Schriftart“ andere Schriftschnitte, Größen und Farben aus.  
  
4.  Klicken Sie auf **Ausführen** , um eine Vorschau des Berichts anzuzeigen.  
  
## <a name="7-save-the-report"></a><a name="Save"></a>7. Speichern des Berichts  
Speichern Sie den Bericht auf einem Berichtsserver oder auf Ihrem Computer. Wenn Sie den Bericht nicht auf dem Berichtsserver speichern, ist eine Reihe von [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] -Funktionen nicht verfügbar, z. B. Berichtsteile und Unterberichte.  
  
### <a name="to-save-the-report-on-a-report-server"></a>So speichern Sie den Bericht auf einem Berichtsserver  
  
1.  Klicken Sie auf die Schaltfläche **Berichts-Generator** und anschließend auf **Speichern unter**.  
  
2.  Klicken Sie auf **Letzte Sites und Server**.  
  
3.  Wählen Sie den Namen des Berichtsservers aus, auf dem Sie zum Speichern von Berichten berechtigt sind, oder geben Sie ihn ein.  
  
    Die Meldung "Verbindung mit Berichtsserver wird hergestellt" wird angezeigt. Nachdem die Verbindung hergestellt wurde, sehen Sie den Inhalt des Berichtsordners, den der Berichtsserveradministrator als Standardspeicherort für Berichte angegeben hat.  
  
4.  Ersetzen Sie im Feld **Name** den Standardnamen durch **Produktumsatz-KPI**.  
  
5.  Klicken Sie auf **Speichern**.  
  
Der Bericht wird auf dem Berichtsserver gespeichert. Der Name des Berichtsservers, mit dem Sie verbunden sind, wird in der Statusleiste unten im Fenster angezeigt.  
  
### <a name="to-save-the-report-on-your-computer"></a>So speichern Sie den Bericht auf dem Computer  
  
1.  Klicken Sie auf die Schaltfläche **Berichts-Generator** und anschließend auf **Speichern unter**.  
  
2.  Klicken Sie auf **Desktop** , **Eigene Dokumente** oder **Computer** , und navigieren Sie zu dem Ordner, in dem Sie den Bericht speichern möchten.  
  
> [!NOTE]  
> Wenn Sie keinen Zugriff auf einen Berichtsserver haben, klicken Sie auf **Desktop** &gt; **Eigene Dokumente** oder **Arbeitsplatz** , und speichern Sie den Bericht auf dem Computer.  
  
1.  Ersetzen Sie im Feld **Name** den Standardnamen durch **Produktumsatz-KPI**.  
  
2.  Klicken Sie auf **Speichern**.  
  
## <a name="next-steps"></a>Nächste Schritte  
Sie haben das Lernprogramm "Hinzufügen eines KPI zu einem Bericht" erfolgreich abgeschlossen. Weitere Informationen finden Sie unter
*  [Gauges (Messgeräte)](../reporting-services/report-design/gauges-report-builder-and-ssrs.md)
* [Indicators (Indikatoren)](../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
  
## <a name="see-also"></a>Weitere Informationen  
* [Lernprogramme für den Berichts-Generator](../reporting-services/report-builder-tutorials.md)
* [Berichts-Generator in SQL Server](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
  

