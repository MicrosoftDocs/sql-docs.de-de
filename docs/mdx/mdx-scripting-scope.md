---
description: SCOPE-Anweisung (MDX)
title: SCOPE-Anweisung (MDX) | Microsoft-Dokumentation
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 4c9f6738b2d7e0764e750b25f09001b7e9d3864a
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88483863"
---
# <a name="mdx-scripting---scope"></a>MDX-Skripts – SCOPE


  Beschränkt den Bereich angegebener MDX-Anweisungen (Multidimensional Expressions) auf einen angegebenen Teilcube.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
SCOPE(Subcube_Expression)   
   [ MDX_Statement ]  
END SCOPE  
  
Subcube_Expression ::=(Auxiliary_Subcube [, Auxiliary_Subcube,...n])  
  
Auxiliary_Subcube ::=   
        Limited_Set   
    | Root([dimension_name])   
    | Leaves([dimension_name])  
  
Limited_Set ::=   
        single_tuple   
    | member   
    | Common_Grain_Members   
    | hierarchy.members   
    | level.members   
    | {}   
    | Descendants  
            (  
                  Member  
         , [level  
         [  
            , SELF   
             | AFTER   
                          | BEFORE   
                          | SELF_AND_AFTER   
                          | SELF_AND_BEFORE   
                          | SELF_BEFORE_AFTER   
                          | LEAVES  
                  ]  
            )   
[* <limited set>]  
```  
  
## <a name="arguments"></a>Argumente  
 *Subcube_Expression*  
 Ein gültiger MDX-Teilcubeausdruck.  
  
 *MDX_Statement*  
 Eine gültige MDX-Anweisung.  
  
 *Common_Grain_Members*  
 Eine gültige MDX-Anweisung, die zu Elementen derselben Granularität ausgewertet wird.  
  
 *single_tuple*  
 Ein einzelnes Tupel.  
  
## <a name="remarks"></a>Bemerkungen  
 Die SCOPE-Anweisung bestimmt den Teilcube, auf den sich die Ausführung mindestens einer MDX-Anweisung auswirkt. Wenn eine MDX-Anweisung nicht in eine SCOPE-Anweisung eingeschlossen ist, besteht der implizite Bereich der MDX-Anweisung im gesamten Cube.  
  
> [!NOTE]  
>  Ausgeblendete Elemente werden in SCOPE-Anweisungen verfügbar gemacht.  
  
 SCOPE-Anweisungen erstellen Teilcubes, die unabhängig von der **MDX-Kompatibilitäts** Einstellung "Löcher" verfügbar machen. So kann z. B. die Anweisung `Scope( Customer.State.members )` die Staaten in Ländern oder Regionen einschließen, die keine Staaten enthalten, für die jedoch unsichtbare Platzhalterelemente eingefügt wurden.  
  
 Berechnete Elemente und benannte Mengen, die in einer SCOPE-Anweisung erstellt wurden, sind von der SCOPE-Anweisung nicht betroffen.  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird aus dem MDX-Berechnungs Skript in der Adventure Works-Beispiellösung der aktuelle Bereich als Geschäftsquartal im Geschäftsjahr 2005 und das Sales Amount Quota-Measure definiert. Anschließend wird den Zellen im aktuellen Bereich mithilfe der **ParallelPeriod** -Funktion ein Wert zugewiesen. Anschließend ändert das Beispiel den Bereich mithilfe einer anderen SCOPE-Anweisung und führt dann mithilfe der [this (MDX)](../mdx/this-mdx.md) -Funktion eine weitere Zuweisung durch.  
  
```  
Scope   
 (   
    [Date].[Fiscal Year].&[2005],  
    [Date].[Fiscal].[Fiscal Quarter].Members,  
    [Measures].[Sales Amount Quota]  
 ) ;     
  
   This = ParallelPeriod                               
          (   
             [Date].[Fiscal].[Fiscal Year], 1,  
             [Date].[Fiscal].CurrentMember   
          ) * 1.35 ;  
  
/*-- Allocate equally to months in FY 2002 -----------------------------*/  
  
  Scope   
  (   
     [Date].[Fiscal Year].&[2002],  
     [Date].[Fiscal].[Month].Members   
  ) ;     
  
    This = [Date].[Fiscal].CurrentMember.Parent / 3 ;     
  
  End Scope ;     
End Scope ;     
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [MDX-Skriptanweisungen &#40;MDX&#41;](../mdx/mdx-scripting-statements-mdx.md)  
  
  
