---
description: Verkettungsoperatoren
title: Verkettungs Operatoren | Microsoft-Dokumentation
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: b53f5d79124a86e8748a473af5b371152932514e
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192555"
---
# <a name="concatenation-operators"></a>Verkettungsoperatoren


  Der Operator für Verkettungen ist das Pluszeichen (+). Sie können zwei oder mehr Zeichenfolgen zu einer einzelnen Zeichenfolge kombinieren oder verketten. Sie können auch binäre Zeichenfolgen verketten.  
  
 Der folgende Code ist ein Beispiel für den Operator für Verkettungen, der hier den Produktnamen mit dem eindeutigen Namen des Produkts kombiniert.  
  
```  
WITH MEMBER Measures.ProductName AS   
   Product.Product.CurrentMember.Name + " (" + Product.Product.CurrentMember.UniqueName + ")"  
SELECT   
   {Measures.ProductName} ON COLUMNS,  
   Product.Product.Members ON ROWS  
FROM [Adventure Works]  
```  
  
## <a name="language-considerations"></a>Sprachbezogene Aspekte  
 Haben die beiden in einer Verkettung verwendeten Zeichenfolgen dieselbe Sortierung, hat die sich ergebende verkettete Zeichenfolge dieselbe Sortierung wie die Eingaben. Haben die in einer Verkettung verwendeten Zeichenfolgen unterschiedliche Sortierungen, wird die Sortierung der sich ergebenden verketteten Zeichenfolge durch die Regeln für die Sortierungsrangfolge bestimmt. Weitere Informationen finden Sie unter [Sprachen und Sortierungen &#40;Analysis Services&#41;](/analysis-services/languages-and-collations-analysis-services).  
  
## <a name="see-also"></a>Weitere Informationen  
 [MDX-Operator Verweis &#40;MDX-&#41;](../mdx/mdx-operator-reference-mdx.md)   
 [Operatoren &#40;MDX-Syntax&#41;](../mdx/operators-mdx-syntax.md)  
  
