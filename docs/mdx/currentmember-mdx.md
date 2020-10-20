---
description: CurrentMember (MDX)
title: CurrentMember (MDX) | Microsoft-Dokumentation
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 5778a8b1d56fa568fe97dba104c1b46da1a005cf
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92196044"
---
# <a name="currentmember-mdx"></a>CurrentMember (MDX)


  Gibt das aktuelle Element entlang einer angegebenen Hierarchie während einer Iteration zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
Hierarchy_Expression.CurrentMember  
```  
  
## <a name="arguments"></a>Argumente  
 *Hierarchy_Expression*  
 Ein gültiger MDX-Ausdruck (Multidimensional Expressions), der eine Hierarchie zurückgibt.  
  
## <a name="remarks"></a>Bemerkungen  
 Bei der Iteration durch eine Menge von Hierarchieelementen stellt bei jedem Iterationsschritt das jeweils bearbeitete Element das aktuelle Element dar. Die **CurrentMember** -Funktion gibt dieses Element zurück.  
  
> [!IMPORTANT]  
>  Wenn eine Dimension nur eine einzige sichtbare Hierarchie enthält, kann auf die Hierarchie entweder mit dem Dimensionsnamen oder mit dem Hierarchienamen verwiesen werden, weil der Dimensionsname in seine einzige sichtbare Hierarchie aufgelöst wird. `Measures.CurrentMember` ist z. B. ein gültiger MDX-Ausdruck, weil er in die einzige vorhandene Hierarchie in der Measures-Dimension aufgelöst wird.  
  
## <a name="examples"></a>Beispiele  
 Die folgende Abfrage zeigt, wie " **CurrentMember** " verwendet werden kann, um den aktuellen Member aus Hierarchien in den Spalten, Zeilen und slicenachse zu finden:  
  
 `WITH MEMBER MEASURES.CURRENTDATE AS`  
  
 `[Date].[Calendar].CURRENTMEMBER.NAME`  
  
 `MEMBER MEASURES.CURRENTPRODUCT AS`  
  
 `[Product].[Product Categories].CURRENTMEMBER.NAME`  
  
 `MEMBER MEASURES.CURRENTMEASURE AS`  
  
 `MEASURES.CURRENTMEMBER.NAME`  
  
 `MEMBER MEASURES.CURRENTCUSTOMER AS`  
  
 `[Customer].[Customer Geography].CURRENTMEMBER.NAME`  
  
 `SELECT`  
  
 `[Product].[Product Categories].[Category].MEMBERS`  
  
 `*`  
  
 `{MEASURES.CURRENTDATE, MEASURES.CURRENTPRODUCT,MEASURES.CURRENTMEASURE, MEASURES.CURRENTCUSTOMER}`  
  
 `ON 0,`  
  
 `[Date].[Calendar].MEMBERS`  
  
 `ON 1`  
  
 `FROM [Adventure Works]`  
  
 `WHERE([Customer].[Customer Geography].[Country].&[Australia])`  
  
 Das aktuelle Element ändert sich in einer Hierarchie, die auf einer Achse in einer Abfrage verwendet wird. Daher kann sich der aktuelle Member in anderen Hierarchien in derselben Dimension, die nicht auf einer Achse verwendet werden, auch ändern; Dieses Verhalten wird als "Auto-vorhanden" bezeichnet. Weitere Informationen finden Sie unter [Schlüsselkonzepte in MDX-&#40;Analysis Services&#41;](/analysis-services/multidimensional-models/mdx/key-concepts-in-mdx-analysis-services). Die folgende Abfrage zeigt beispielsweise, wie sich das aktuelle Element in der Calendar Year-Hierarchie der Date-Dimension mit dem aktuellen Element der Calendar-Hierarchie ändert, wenn dieses auf der ROWS-Achse angezeigt wird:  
  
 `WITH MEMBER MEASURES.CURRENTYEAR AS`  
  
 `[Date].[Calendar Year].CURRENTMEMBER.NAME`  
  
 `SELECT`  
  
 `{MEASURES.CURRENTYEAR}`  
  
 `ON 0,`  
  
 `[Date].[Calendar].MEMBERS`  
  
 `ON 1`  
  
 `FROM [Adventure Works]`  
  
 **CurrentMember** ist äußerst wichtig, damit Berechnungen den Kontext der Abfrage erkennen, in der Sie verwendet werden. Im folgenden Beispiel wird die Bestellmenge jedes Produkts und der Prozentsatz der Bestellmengen nach Kategorie und Modell aus dem **Adventure Works** -Cube zurückgegeben. Die **CurrentMember** -Funktion identifiziert das Produkt, dessen Bestellmenge während der Berechnung verwendet werden soll.  
  
```  
WITH   
   MEMBER [Measures].[Order Percent by Category] AS  
   CoalesceEmpty  
(   
      ([Product].[Product Categories].CurrentMember,  
        Measures.[Order Quantity]) /   
          (  
           Ancestor  
           ( [Product].[Product Categories].CurrentMember,   
             [Product].[Product Categories].[Category]  
           ), Measures.[Order Quantity]  
       ), 0  
   ), FORMAT_STRING='Percent'  
SELECT   
   {Measures.[Order Quantity],  
      [Measures].[Order Percent by Category]} ON COLUMNS,  
{[Product].[Product].Members} ON ROWS  
FROM [Adventure Works]  
WHERE {[Date].[Calendar Year].[Calendar Year].&[2003]}  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [MDX-Funktionsreferenz &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
