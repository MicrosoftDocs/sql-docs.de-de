---
description: DataMember (MDX)
title: DataMember (MDX) | Microsoft-Dokumentation
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: ea373dc4ffe7eb01b19969288afce45489ab4e8c
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92196034"
---
# <a name="datamember-mdx"></a>DataMember (MDX)


  Gibt das systemgenerierte Datenelement zurück, das einem Nicht-Blattelement einer Dimension zugeordnet ist.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
Member_Expression.DataMember  
```  
  
## <a name="arguments"></a>Argumente  
 *Member_Expression*  
 Ein gültiger MDX-Ausdruck (Multidimensional Expressions), der ein Element zurückgibt.  
  
## <a name="remarks"></a>Bemerkungen  
 Diese Funktion wird für nicht Blatt Elemente in einer beliebigen Hierarchie verwendet und kann vom Befehl " [Update CUBE-Anweisung (MDX)](../mdx/mdx-data-manipulation-update-cube.md) " verwendet werden, um Daten direkt an ein nicht Blatt Element zurückzuschreiben, anstatt in die Nachfolger Elemente des Blatt Elements.  
  
> [!NOTE]  
>  Gibt das angegebene Element zurück, wenn es ein Blattelement ist, oder wenn das Nichtblattelement über keine dazugehörigen Datenelemente verfügt.  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird die **DataMember** -Funktion in einem berechneten Measure verwendet, um das Vertriebs Kontingent für jeden einzelnen Mitarbeiter anzuzeigen:  
  
```  
WITH MEMBER measures.InvidualQuota AS   
([Employee].[Employees].currentmember.datamember, [Measures].[Sales Amount Quota])  
,FORMAT_STRING='Currency'  
SELECT {[Measures].[Sales Amount Quota],[Measures].InvidualQuota} ON COLUMNS,  
[Employee].[Employees].MEMBERS ON ROWS  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [MDX-Funktionsreferenz &#40;MDX-&#41;](../mdx/mdx-function-reference-mdx.md)   
 [Schlüsselkonzepte in MDX &#40;Analysis Services&#41;](/analysis-services/multidimensional-models/mdx/key-concepts-in-mdx-analysis-services)  
  
