---
title: Referenz zu Aggregatfunktionen (Berichts-Generator) | Microsoft-Dokumentation
description: Verwenden Sie in Ausdrücken im Berichts-Generator integrierte Aggregatfunktionen, um in Ihrem Bericht aggregierte Werte zu verwenden.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: db6542ee-02d0-4073-90e6-cba8f9510fbb
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8338e122e08ddf03417875c6a0436c16d1e3ff34
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91935278"
---
# <a name="report-builder-functions---aggregate-functions-reference"></a>Funktionen des Berichts-Generators: Referenz zu Aggregatfunktionen
  Um aggregierte Werte in den Bericht einzuschließen, können Sie integrierte Aggregatfunktionen in Ausdrücken verwenden. Die Standardaggregatfunktion für numerische Felder ist SUM. Sie können den Ausdruck bearbeiten und eine andere integrierte Aggregatfunktion verwenden oder einen anderen Bereich angeben. Mit dem Bereich wird angegeben, welcher Satz an Daten für die Berechnung verwendet werden soll.  
  
 Der Berichtsprozessor kombiniert Berichtsdaten und das Berichtslayout, und die Ausdrücke für jedes Berichtselement werden ausgewertet. Beim Anzeigen der einzelnen Seiten des Berichts sehen Sie die Ergebnisse für jeden Ausdruck in den gerenderten Berichtselementen.  
  
 In der folgenden Tabelle sind Kategorien von integrierten Funktionen aufgeführt, die Sie in einen Ausdruck einschließen können:  
  
-   [Integrierte Aggregatfunktionen](#CalculatingAggregates)  
  
-   [Einschränkungen bei integrierten Feldern, Auflistungen und Aggregatfunktionen](#Restrictions)  
  
-   [Beschränkungen bei geschachtelten Aggregaten](#NestedRestrictions)  
  
-   [Berechnen von ausgeführten Werten](#CalculatingRunningValues)  
  
-   [Abrufen von Zeilenanzahlen](#RetrievingRowCounts)  
  
-   [Nachschlagen von Werten aus einem anderen Dataset](#LookupFunctions)  
  
-   [Abrufen von sortierungsabhängigen Werten](#RetrievingPostsortValues)  
  
-   [Abrufen von Serveraggregaten](#RetrievingServerAggregates)  
  
-   [Abrufen von rekursiven Ebenen](#RetrievingRecursiveLevel)  
  
-   [Testen für Bereich](#TestingforScope)  
  
 Zur Bestimmung der gültigen Bereiche für eine Funktion lesen Sie im entsprechenden Funktionsreferenzthema nach. Weitere Informationen und Beispiele finden Sie unter [Ausdrucksbereich für Gesamtwerte, Aggregate und integrierte Sammlungen &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="built-in-aggregate-functions"></a><a name="CalculatingAggregates"></a> Integrierte Aggregatfunktionen  
 Die folgenden integrierten Funktionen berechnen Summenwerte für einen Satz von numerischen Daten ungleich null im Standardbereich oder im benannten Bereich.  
  
|**Function**|**Beschreibung**|  
|------------------|---------------------|  
|[Avg](../../reporting-services/report-design/report-builder-functions-avg-function.md)|Gibt den Durchschnitt aller numerischen Werte ungleich NULL aus dem angegebenen Ausdruck im Kontext des festgelegten Bereichs ausgewertet zurück.|  
|[Count](../../reporting-services/report-design/report-builder-functions-count-function.md)|Gibt die Anzahl der Werte ungleich NULL aus dem angegebenen Ausdruck im Kontext des festgelegten Bereichs ausgewertet zurück.|  
|[CountDistinct](../../reporting-services/report-design/report-builder-functions-countdistinct-function.md)|Gibt die Anzahl aller unterschiedlichen Werte ungleich NULL aus dem angegebenen Ausdruck im Kontext des angegebenen Bereichs ausgewertet zurück.|  
|[Max](../../reporting-services/report-design/report-builder-functions-max-function.md)|Gibt den Maximalwert aller numerischen Werte ungleich NULL aus dem angegebenen Ausdruck im Kontext des angegebenen Bereichs zurück. Sie können diese Funktion verwenden, um einen Diagrammachsen-Höchstwert zur Steuerung der Skala anzugeben.|  
|[Min](../../reporting-services/report-design/report-builder-functions-min-function.md)|Gibt den Minimalwert aller numerischen Werte ungleich NULL aus dem angegebenen Ausdruck im Kontext des angegebenen Bereichs zurück. Sie können diese Funktion verwenden, um einen Diagrammachsen-Mindestwert zur Steuerung der Skala anzugeben.|  
|[StDev](../../reporting-services/report-design/report-builder-functions-stdev-function.md)|Gibt die Standardabweichung aller numerischen Werte ungleich NULL aus dem angegebenen Ausdruck im Kontext des angegebenen Bereichs ausgewertet zurück.|  
|[StDevP](../../reporting-services/report-design/report-builder-functions-stdevp-function.md)|Gibt die Standardabweichung der Auffüllung aller numerischen Werte ungleich NULL aus dem angegebenen Ausdruck im Kontext des festgelegten Bereichs ausgewertet zurück.|  
|[Sum](../../reporting-services/report-design/report-builder-functions-sum-function.md)|Gibt die Summe aller numerischen Werte ungleich NULL aus dem angegebenen Ausdruck im Kontext des festgelegten Bereichs ausgewertet zurück.|  
|[Union](../../reporting-services/report-design/report-builder-functions-union-function.md)|Gibt die Vereinigung aller räumlichen Datenwerte ungleich NULL vom Typ **SqlGeometry** oder **SqlGeography** aus dem angegebenen Ausdruck im Kontext des festgelegten Bereichs ausgewertet zurück.|  
|[Var](../../reporting-services/report-design/report-builder-functions-var-function.md)|Gibt die Varianz aller numerischen Werte ungleich NULL aus dem angegebenen Ausdruck im Kontext des festgelegten Bereichs ausgewertet zurück.|  
|[VarP](../../reporting-services/report-design/report-builder-functions-varp-function.md)|Gibt die Auffüllungsvarianz aller numerischen Werte ungleich NULL aus dem angegebenen Ausdruck im Kontext des angegebenen Bereichs ausgewertet zurück.|  
  
 ![Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird")Zurück zum Anfang  
  
##  <a name="restrictions-on-built-in-fields-collections-and-aggregate-functions"></a><a name="Restrictions"></a> Einschränkungen bei integrierten Feldern, Auflistungen und Aggregatfunktionen  
 In der folgenden Tabelle werden Einschränkungen für Berichtspositionen zusammengefasst, auf deren Grundlage Sie Ausdrücke hinzufügen können, die Verweise auf globale integrierte Auflistungen enthalten.  
  
|Position im Bericht|Felder|Parameter|Berichtselemente|PageNumber<br /><br /> TotalPages|DataSource<br /><br /> Dataset|Variables|RenderFormat|  
|------------------------|------------|----------------|-----------------|-------------------------------|----------------------------|---------------|------------------|  
|Seitenheader<br /><br /> Seitenfuß|Ja|Ja|Höchstens eins<br /><br /> Hinweis 1|Ja|Ja|Ja|Ja|  
|Body|Ja<br /><br /> Hinweis 2|Ja|Nur Elemente im aktuellen Bereich oder einem enthaltenen Bereich<br /><br /> Hinweis 3|Nein|Ja|Ja|Ja|  
|Berichtsparameter|Nein|Nur Parameter am Anfang der Liste<br /><br /> Hinweis 4|Nein|Nein|Nein|Nein|Nein|  
|Feld|Ja|Ja|Nein|Nein|Nein|Nein|Nein|  
|Abfrageparameter|Nein|Ja|Nein|Nein|Nein|Nein|Nein|  
|Gruppierungsausdruck|Ja|Ja|Nein|Nein|Ja|Nein|Nein|  
|Sortierungsausdruck|Ja|Ja|Nein|Nein|Ja|Ja<br /><br /> Hinweis 5|Nein|  
|Filterausdruck|Ja|Ja|Nein|Nein |Ja|Ja<br /><br /> Hinweis 6|Nein|  
|Code|Nein|Ja<br /><br /> Hinweis 7|Nein|Nein|Nein|Nein|Nein|  
|Berichtssprache|Nein|Ja|Nein|Nein|Nein|Nein|Nein|  
|Variables|Ja|Ja|Nein|Nein |Ja|Aktueller oder enthaltener Bereich|Nein|  
|Aggregate|Ja|Ja|Nur in Seitenkopf/Seitenfuß|Nur in Berichtselementaggregaten|Ja|Nein|Nein|  
|Suchfunktionen|Ja|Ja|Ja|Nein|Ja|Nein|Nein|  
  
-   **Hinweis 1.** Berichtselemente müssen in der gerenderten Berichtsseite vorhanden sein, oder der Wert ist NULL. Wenn die Sichtbarkeit eines Berichtselements von einem Ausdruck abhängt, der False ergibt, ist das Berichtselement auf der Seite nicht vorhanden.  
  
-   **Hinweis 2.** Wenn ein Feldverweis in einem Gruppenbereich verwendet wird und der Feldverweis nicht im Gruppenausdruck enthalten ist, dann wird die Definition des Werts für das Feld aufgehoben, außer wenn es nur einen Wert im Bereich gibt. Verwenden Sie Erste oder Letzte und den Gruppenbereich, um einen Wert anzugeben.  
  
-   **Hinweis 3.** Ausdrücke, die einen Verweis auf Berichtselemente einschließen, können Werte für andere Berichtselemente im gleichen Gruppenbereich oder einem enthaltenen Gruppenbereich angeben.  
  
-   **Hinweis 4.** Die Eigenschaftswerte für frühere Parameter sind u. U. NULL.  
  
-   **Hinweis 5.** Nur in Mitgliedssortierungen. Nicht zu verwenden in Ausdrücken für die Datenbereichssortierung.  
  
-   **Hinweis 6.** Nur in Mitgliedsfiltern. Nicht zu verwenden in Datenbereichs- oder Datasetfilterausdrücken.  
  
-   **Hinweis 7.** Die Parametersammlung wird erst initialisiert, nachdem der Codeblock verarbeitet wurde, daher können keine Methoden zum Steuern von Parametern während der Initialisierung verwendet werden.  
  
-   **Hinweis 8.** Der Datentyp für alle Aggregate außer Count und CountDistinct muss für alle Werte der gleiche Datentyp oder NULL sein.  
  
 ![Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird")Zurück zum Anfang  
  
##  <a name="restrictions-on-nested-aggregates"></a><a name="NestedRestrictions"></a> Beschränkungen bei geschachtelten Aggregaten  
 In der folgenden Tabelle finden Sie Einschränkungen, auf deren Grundlage Aggregatfunktionen andere Aggregatfunktionen als geschachtelte Aggregate angeben können.  
  
|Kontext|RunningValue|RowNumber|First (Erster)<br /><br /> Last (Letzter)|Previous|Sum und andere Vorsortierungsfunktionen|ReportItem-Aggregate|Suchfunktionen|Aggregatfunktion|  
|-------------|------------------|---------------|--------------------|--------------|-------------------------------------|---------------------------|----------------------|------------------------|  
|Ausgeführter Wert|Nein|Nein|Nein|Nein |Ja|Nein|Ja|Nein|  
|First (Erster)<br /><br /> Last (Letzter)|Nein|Nein|Nein|Nein |Ja|Nein|Nein|Nein|  
|Previous|Ja|Ja|Ja|Nein|Ja|Nein|Ja|Nein|  
|Sum und andere Vorsortierungsfunktionen|Nein|Nein|Nein|Nein |Ja|Nein|Ja|Nein|  
|ReportItem-Aggregate|Nein|Nein|Nein|Nein|Nein|Nein|Nein|Nein|  
|Suchfunktionen|Ja|Ja<br /><br /> Hinweis 1|Ja<br /><br /> Hinweis 1|Ja<br /><br /> Hinweis 1|Ja<br /><br /> Hinweis 1|Ja<br /><br /> Hinweis 1|Nein|Nein|  
|Aggregatfunktion|Nein|Nein|Nein|Nein|Nein|Nein|Nein|Nein|  
  
-   **Hinweis 1.** Aggregatfunktionen sind nur im *Source* -Ausdruck einer Suchfunktion zulässig, wenn die Suchfunktion nicht in einem Aggregat enthalten ist. Aggregatfunktionen sind im *Destination* -Ausdruck oder *Result* -Ausdruck einer Suchfunktion nicht zulässig.  
  
 ![Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird")Zurück zum Anfang  
  
##  <a name="calculating-running-values"></a><a name="CalculatingRunningValues"></a> Berechnen von ausgeführten Werten  
 Die folgenden integrierten Funktionen berechnen ausgeführte Werte für einen Satz von Daten. **RowNumber** gleicht **RunningValue** darin, dass der ausgeführte Wert für eine Anzahl zurückgegeben wird, die für jede Zeile innerhalb des enthaltenen Bereichs inkrementiert wird. Der Bereichsparameter für diese Funktionen muss einen enthaltenden Bereich angeben, der steuert, wann der Zähler neu gestartet wird.  
  
|**Function**|**Beschreibung**|  
|------------------|---------------------|  
|[RowNumber](../../reporting-services/report-design/report-builder-functions-rownumber-function.md)|Gibt eine laufende Zählung der Zeilenanzahl für den angegebenen Bereich zurück. Die **RowNumber** -Funktion startet den Zähler bei 1 neu und nicht bei 0.|  
|[RunningValue](../../reporting-services/report-design/report-builder-functions-runningvalue-function.md)|Gibt ein laufendes Aggregat aller numerischen Werte ungleich NULL aus dem angegebenen Ausdruck für den Kontext des angegebenen Bereichs ausgewertet zurück.|  
  
 ![Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird")Zurück zum Anfang  
  
##  <a name="retrieving-row-counts"></a><a name="RetrievingRowCounts"></a> Abrufen von Zeilenanzahlen  
 Die folgende integrierte Funktion berechnet die Anzahl von Zeilen im angegebenen Bereich. Verwenden Sie diese Funktion, um alle Zeilen zu zählen, einschließlich Zeilen mit NULL-Werten.  
  
|**Function**|**Beschreibung**|  
|------------------|---------------------|  
|[CountRows](../../reporting-services/report-design/report-builder-functions-countrows-function.md)|Gibt die Anzahl der Zeilen im angegebenen Bereich zurück, einschließlich der Zeilen mit NULL-Werten.|  
  
 ![Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird")Zurück zum Anfang  
  
##  <a name="looking-up-values-from-another-dataset"></a><a name="LookupFunctions"></a> Nachschlagen von Werten aus einem anderen Dataset  
 Die folgenden Suchfunktionen rufen Werte aus einem angegebenen Dataset ab.  
  
|**Function**|**Beschreibung**|  
|------------------|---------------------|  
|[Lookup-Funktion](../../reporting-services/report-design/report-builder-functions-lookup-function.md)|Gibt für einen angegebenen Ausdruck einen Wert aus einem Dataset zurück.|  
|[LookupSet-Funktion](../../reporting-services/report-design/report-builder-functions-lookupset-function.md)|Gibt für einen angegebenen Ausdruck einen Satz von Werten aus einem Dataset zurück.|  
|[Multilookup-Funktion](../../reporting-services/report-design/report-builder-functions-multilookup-function.md)|Gibt den Satz der ersten übereinstimmenden Werte für einen Satz von Namen aus einem Dataset mit Name-Wert-Paaren zurück.|  
  
 ![Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird")Zurück zum Anfang  
  
##  <a name="retrieving-sort-dependent-values"></a><a name="RetrievingPostsortValues"></a> Abrufen von sortierungsabhängigen Werten  
 Die folgenden integrierten Funktionen geben den ersten, letzten oder vorherigen Wert innerhalb eines gegebenen Bereichs zurück. Diese Funktionen hängen von der Sortierreihenfolge der Datenwerte ab. Verwenden Sie diese Funktionen, um beispielsweise den ersten und den letzten Wert auf einer Seite zu suchen, um Seitenkopfzeilen im Wörterbuchformat zu erstellen. Verwenden Sie **Previous** , um einen Wert in einer Zeile mit dem Wert der vorherigen Zeile innerhalb eines bestimmten Bereichs zu vergleichen, beispielsweise, um jährliche Prozentwerte in einer Tabelle zu finden.  
  
|**Function**|**Beschreibung**|  
|------------------|---------------------|  
|[First](../../reporting-services/report-design/report-builder-functions-first-function.md)|Gibt den ersten Wert im festgelegten Bereich des angegebenen Ausdrucks zurück.|  
|[Last](../../reporting-services/report-design/report-builder-functions-last-function.md)|Gibt den letzten Wert im festgelegten Bereich des angegebenen Ausdrucks zurück.|  
|[Previous](../../reporting-services/report-design/report-builder-functions-previous-function.md)|Gibt den Wert oder den angegebenen Aggregatwert für die vorherige Instanz eines Elements innerhalb des angegebenen Bereichs zurück.|  
  
 ![Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird")Zurück zum Anfang  
  
##  <a name="retrieving-server-aggregates"></a><a name="RetrievingServerAggregates"></a> Abrufen von Serveraggregaten  
 Die folgende integrierte Funktion ruft benutzerdefinierte Aggregate vom Datenanbieter ab. Mithilfe eines [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Datenquellentyps können Sie beispielsweise Aggregate, die auf dem Datenquellenserver berechnet wurden, für die Verwendung in einer Gruppenkopfzeile abrufen.  
  
|**Function**|**Beschreibung**|  
|------------------|---------------------|  
|[Aggregat](../../reporting-services/report-design/report-builder-functions-aggregate-function.md)|Gibt ein benutzerdefiniertes Aggregat des angegebenen Ausdrucks gemäß der Definition durch den Datenanbieter zurück.|  
  
 ![Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird")Zurück zum Anfang  
  
##  <a name="testing-for-scope"></a><a name="TestingforScope"></a> Testen für Bereich  
 Die folgende integrierte Funktion testet den aktuellen Kontext eines Berichtselements, um festzustellen, ob dieses Mitglied eines bestimmten Bereichs ist.  
  
|Funktion|BESCHREIBUNG|  
|--------------|-----------------|  
|[InScope](../../reporting-services/report-design/report-builder-functions-inscope-function.md)|Gibt an, ob sich die aktuelle Instanz eines Elements innerhalb des angegebenen Bereichs befindet.|  
  
 ![Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird")Zurück zum Anfang  
  
##  <a name="retrieving-recursive-level"></a><a name="RetrievingRecursiveLevel"></a> Abrufen von rekursiven Ebenen  
 Die folgende integrierte Funktion ruft die aktuelle Ebene ab, wenn eine rekursive Hierarchie verarbeitet wird. Verwenden Sie das Ergebnis dieser Funktion mit der Eigenschaft **Padding** in einem Textfeld, um für eine rekursive Gruppe die Einzugsebene in einer visuellen Hierarchie zu steuern. Weitere Informationen finden Sie unter [Erstellen von rekursiven Hierarchiegruppen &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/creating-recursive-hierarchy-groups-report-builder-and-ssrs.md).  
  
|Funktion|BESCHREIBUNG|  
|--------------|-----------------|  
|[Level](../../reporting-services/report-design/report-builder-functions-level-function.md)|Gibt die aktuelle Ebene in einer rekursiven Hierarchie zurück.|  
  
 ![Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird")Zurück zum Anfang  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ausdrucksverwendungen in Berichten &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Beispiele für Ausdrücke &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Ausdrucksbereich für Gesamtwerte, Aggregate und integrierte Sammlungen &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)  
  
