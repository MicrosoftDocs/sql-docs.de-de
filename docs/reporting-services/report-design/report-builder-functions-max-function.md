---
title: Max-Funktion (Berichts-Generator) | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie mehr über die Funktion „Max“, die den höchsten Wert aller numerischen Werte ungleich null zurückgibt, die von einem Ausdruck im Berichts-Generator angegeben werden.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 61c4d6ff-6435-456a-9cbd-5113d2113e8a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7b218b63de094ef11d04af70b8492b8fbe3522c0
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596085"
---
# <a name="report-builder-functions---max-function"></a>Funktionen des Berichts-Generators: Max-Funktion
  Gibt den Maximalwert aller numerischen Werte ungleich NULL aus dem angegebenen Ausdruck im Kontext des angegebenen Bereichs zurück.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="syntax"></a>Syntax  
  
```  
  
Max(expression, scope, recursive)  
```  
  
#### <a name="parameters"></a>Parameter  
 *expression*  
 (**Variant**) Der Ausdruck, für den die Aggregation ausgeführt werden soll.  
  
 *scope*  
 (**Zeichenfolge**) Optional. Der Name eines Datasets, einer Gruppe oder eines Datenbereichs mit den Berichtselementen, auf die die Aggregatfunktion anzuwenden ist. Wenn *scope* nicht angegeben ist, wird der aktuelle Bereich verwendet.  
  
 *recursive*  
 (**Enumerationstyp**) Optional. **Simple** (Standard) oder **RdlRecursive**. Gibt an, ob die Aggregation rekursiv auszuführen ist.  
  
## <a name="return-type"></a>Rückgabetyp  
 Wird durch den Typ des Ausdrucks bestimmt.  
  
## <a name="remarks"></a>Bemerkungen  
 Die im Ausdruck angegebene Gruppe von Daten muss über den gleichen Datentyp verfügen. Um Daten mit mehreren numerischen Datentypen in den gleichen Datentyp zu konvertieren, verwenden Sie Konvertierungsfunktionen wie **CInt**, **CDbl** oder **CDec**. Weitere Informationen finden Sie unter [Funktionen für die Typkonvertierung](/dotnet/visual-basic/language-reference/functions/type-conversion-functions).  
  
 Der Wert des *scope* -Objekts muss eine Zeichenfolgenkonstante sein und darf kein Ausdruck sein. Für äußere Aggregate oder Aggregate, die keine anderen Aggregate angeben, muss das *scope* -Objekt auf den aktuellen Bereich oder einen enthaltenen Bereich verweisen. Bei Aggregaten von Aggregaten können geschachtelte Aggregate einen untergeordneten Bereich angeben.  
  
 Das *Expression* -Objekt kann Aufrufe von geschachtelten Aggregatfunktionen enthalten. Dabei gelten folgende Ausnahmen und Bedingungen:  
  
-   Das *Scope* -Objekt für geschachtelte Aggregate muss dem Bereich des äußeren Aggregats entsprechen oder darin enthalten sein. In allen eindeutigen Bereichen des Ausdrucks muss ein Bereich eine untergeordnete Beziehung zu allen anderen Bereichen haben.  
  
-   Das *Scope* -Objekt für geschachtelte Aggregate darf nicht der Name eines Datasets sein.  
  
-   Das *Expression* -Objekt darf die Funktionen **First**, **Last**, **Previous** oder **RunningValue** nicht enthalten.  
  
-   Das *Expression* -Objekt darf keine geschachtelten Aggregate enthalten, die ein *recursive*-Objekt angeben.  
  
 Weitere Informationen finden Sie in der [Aggregatfunktionsreferenz (Berichts-Generator und SSRS)](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md) und unter [Ausdrucksbereich für Gesamtwerte, Aggregate und integrierte Auflistungen (Berichts-Generator und SSRS)](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
 Weitere Informationen zu rekursiven Aggregaten finden Sie unter [Creating Recursive Hierarchy Groups (Report Builder and SSRS) (Erstellen von rekursiven Hierarchiegruppen (Berichts-Generator und SSRS))](../../reporting-services/report-design/creating-recursive-hierarchy-groups-report-builder-and-ssrs.md).  
  
## <a name="example"></a>Beispiel  
 Mit dem folgenden Codebeispiel wird der höchste Gesamtwert in der Gruppierung bzw. dem Datenbereich `Year` bereitgestellt.  
  
```  
=Max(Fields!OrderTotal.Value, "Year")  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ausdrucksverwendungen in Berichten &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Beispiele für Ausdrücke &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Datentypen in Ausdrücken (Berichts-Generator und SSRS)](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)   
 [Ausdrucksbereich für Gesamtwerte, Aggregate und integrierte Sammlungen &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)  
  
