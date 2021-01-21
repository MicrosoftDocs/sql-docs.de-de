---
title: RunningValue-Funktion (Berichts-Generator) | Microsoft-Dokumentation
description: In diesem Artikel erhalten Sie Informationen zur Funktion „RunningValue“, die ein aktuelles Aggregat aller numerischen Nicht-NULL-Werte zurückgibt, die vom Ausdruck im Berichts-Generator angegeben werden.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 6bee2f15-0e69-49c8-9689-b04544063b1d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 75b25a7cb08a7473e6a725a841e53011c6967756
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596019"
---
# <a name="report-builder-functions---runningvalue-function"></a>Funktionen des Berichts-Generators: RunningValue-Funktion
  Gibt ein laufendes Aggregat aller numerischen Werte ungleich NULL aus dem angegebenen Ausdruck für den Kontext des angegebenen Bereichs ausgewertet zurück.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="syntax"></a>Syntax  
  
```  
  
RunningValue(expression, function, scope)  
```  
  
#### <a name="parameters"></a>Parameter  
 *expression*  
 Der Ausdruck, für den die Aggregation auszuführen ist, z.B. `[Quantity]`.  
  
 *Funktion*  
 (**Enum**) Der Name der Aggregatfunktion, die auf den Ausdruck angewendet werden soll. Beispiel: **Sum**. Diese Funktion kann nicht **RunningValue**, **RowNumber** oder **Aggregate** sein.  
  
 *scope*  
 (**String**) Eine Zeichenfolgenkonstante als Name eines Datasets, eines Datenbereichs oder einer Gruppe oder NULL (**Nothing** in [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]), der den Kontext angibt, in dem die Aggregation ausgewertet wird. Durch **Nothing** wird der äußerste Kontext angegeben, normalerweise das Berichtsdataset.  
  
## <a name="return-type"></a>Rückgabetyp  
 Wird durch die im *function* -Parameter angegebene Aggregatfunktion bestimmt.  
  
## <a name="remarks"></a>Bemerkungen  
 Der Wert für **RunningValue** wird für jede neue Instanz des Bereichs auf 0 zurückgesetzt. Wenn eine Gruppe angegeben wird, wird der laufende Wert zurückgesetzt, wenn sich der Gruppenausdruck ändert. Wenn ein Datenbereich angegeben wird, wird der laufende Wert für jede neue Instanz des Datenbereichs zurückgesetzt. Wenn ein Dataset angegeben wird, wird der laufende Wert für das gesamte Dataset nicht zurückgesetzt.  
  
 **RunningValue** darf nicht in einem Filter- oder Sortierausdruck verwendet werden.  
  
 Der Datensatz, für den der ausgeführte Wert berechnet wird, muss den gleichen Datentyp aufweisen. Um Daten mit mehreren numerischen Datentypen in den gleichen Datentyp zu konvertieren, verwenden Sie Konvertierungsfunktionen wie **CInt**, **CDbl** oder **CDec**. Weitere Informationen finden Sie unter [Funktionen für die Typkonvertierung](/dotnet/visual-basic/language-reference/functions/type-conversion-functions).  
  
 *Scope* darf kein Ausdruck sein.  
  
 Das *Expression* -Objekt kann Aufrufe von geschachtelten Aggregatfunktionen enthalten. Dabei gelten folgende Ausnahmen und Bedingungen:  
  
-   Der Bereich für geschachtelte Aggregate muss dem Bereich des äußeren Aggregats entsprechen oder darin enthalten sein. In allen eindeutigen Bereichen des Ausdrucks muss ein Bereich eine untergeordnete Beziehung zu allen anderen Bereichen haben.  
  
-   Der Bereich für geschachtelte Aggregate darf nicht der Name eines Datasets sein.  
  
-   Das *Expression* -Objekt darf die Funktionen **First**, **Last**, **Previous** oder **RunningValue** nicht enthalten.  
  
-   Das *Expression* -Objekt darf keine geschachtelten Aggregate enthalten, die ein *recursive*-Objekt angeben.  
  
 Verwenden Sie **RowNumber** zur Berechnung des laufenden Werts für die Zeilenanzahl. Weitere Informationen finden Sie unter [RowNumber-Funktion (Berichts-Generator und SSRS)](../../reporting-services/report-design/report-builder-functions-rownumber-function.md).  
  
 Weitere Informationen finden Sie in der [Aggregatfunktionsreferenz (Berichts-Generator und SSRS)](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md) und unter [Ausdrucksbereich für Gesamtwerte, Aggregate und integrierte Auflistungen (Berichts-Generator und SSRS)](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
 Weitere Informationen zu rekursiven Aggregaten finden Sie unter [Creating Recursive Hierarchy Groups (Report Builder and SSRS) (Erstellen von rekursiven Hierarchiegruppen (Berichts-Generator und SSRS))](../../reporting-services/report-design/creating-recursive-hierarchy-groups-report-builder-and-ssrs.md).  
  
## <a name="examples"></a>Beispiele  
 Das folgende Codebeispiel generiert eine laufende Summe für das Feld mit dem Namen `Cost` im äußersten Bereich, den das Dataset darstellt.  
  
```  
=RunningValue(Fields!Cost.Value, Sum, Nothing)  
```  
  
 Das folgende Codebeispiel generiert eine laufende Summe für das Feld mit dem Namen `Score` im Dataset mit dem Namen `DataSet1`.  
  
```  
=RunningValue(Fields!Score.Value,sum,"DataSet1")  
```  
  
 Das folgende Codebeispiel generiert eine laufende Summe für das Feld mit dem Namen `Traffic Charges` im äußersten Bereich.  
  
```  
=RunningValue(Fields!Traffic Charges.Value, Sum, Nothing)  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ausdrucksverwendungen in Berichten &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Beispiele für Ausdrücke &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Datentypen in Ausdrücken (Berichts-Generator und SSRS)](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)   
 [Ausdrucksbereich für Gesamtwerte, Aggregate und integrierte Sammlungen &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)  
  
