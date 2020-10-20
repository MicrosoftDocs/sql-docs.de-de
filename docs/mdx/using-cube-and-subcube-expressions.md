---
description: Verwenden von Cube- und Teilcubeausdrücken
title: Verwenden von Cube-und Teilcubeausdrücken | Microsoft-Dokumentation
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 37f23c75d8a947e52daf47ef9648ccbd1587b9a8
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192300"
---
# <a name="using-cube-and-subcube-expressions"></a>Verwenden von Cube- und Teilcubeausdrücken


  Sie verwenden Cube- und Teilcubeausdrücke in MDX-Anweisungen (Multidimensional Expressions), um Daten für einen Cube oder Teilcube zu definieren oder zu bearbeiten oder um Daten aus einem Cube oder Teilcube abzurufen.  
  
## <a name="cube-expressions"></a>Cubeausdrücke  
 Ein Cubeausdruck enthält entweder einen Cubebezeichner oder das CURRENTCUBE-Schlüsselwort. Er kann deshalb nur ein einfacher Ausdruck sein. In vielen MDX-Anweisung wird, statt einen Cubebezeichner zu verlangen, das CURRENTCUBE-Schlüsselwort verwendet, um den aktuellen Cubekontext anzugeben.  
  
 Ein Cubebezeichner wird als *CUBE_NAME* in den BNF-Notation-Beschreibungen von MDX-Anweisungen angezeigt.  
  
 Cubeausdrücke können an mehreren Stellen angezeigt werden. In einer MDX SELECT-Anweisung geben sie den Cube an, von dem Daten abgerufen werden sollen. In der folgenden Beispielabfrage verweist der Ausdruck [Adventure Works] auf den gleichnamigen Cube:  
  
 `SELECT [Measures].[Internet Sales Amount] ON COLUMNS`  
  
 `FROM [Adventure Works]`  
  
 In der CREATE MEMBER-Anweisung gibt der Cube-Ausdruck den Cube an, in dem das von Ihnen erstellte berechnete Element angezeigt werden soll. Im folgenden Beispiel wird durch die Anweisung ein berechnetes Measure in der Measures-Dimension des Adventure Works-Cube erstellt:  
  
 `CREATE MEMBER [Adventure Works].[Measures].[Test] AS 1`  
  
 Wenn Sie eine CREATE MEMBER-Anweisung innerhalb eines MDX-Scripts verwenden, kann der Name des Cube durch das Schlüsselwort CURRENTCUBE ersetzt werden, da der Cube, in dem das berechnete Element erstellt wird, derselbe sein muss, zu dem auch das MDX-Script gehört, wie das folgende Beispiel zeigt:  
  
 `CREATE MEMBER CURRENTCUBE.[Measures].[Test] AS 1;`  
  
 Dies macht es einfacher, Definitionen berechneter Elemente von einem Cube zum anderen zu kopieren, da der Name des Cube nicht mehr hartcodiert ist.  
  
## <a name="subcube-expressions"></a>Teilcubeausdrücke  
 Ein Teilcubeausdruck kann einen Teilcubebezeichner oder eine MDX-Anweisung enthalten, die einen Teilcube zurückgibt. Wenn der Teilcubeausdruck einen Teilcubebezeichner enthält, ist es ein einfacher Ausdruck. Wenn er eine MDX-Anweisung enthält, die einen Teilcube zurückgibt, ist er eine komplexe Anweisung. Die MDX-SELECT-Anweisung gibt z. B. einen Teilcube zurück und kann an jeder Stelle verwendet werden, an der Teilcubeausdrücke zulässig sind, wie das folgende Beispiel zeigt:  
  
 `SELECT [Measures].MEMBERS ON COLUMNS,`  
  
 `[Date].[Calendar Year].MEMBERS ON ROWS`  
  
 `FROM`  
  
 `(SELECT [Measures].[Internet Sales Amount] ON COLUMNS,`  
  
 `[Date].[Calendar Year].&[2004] ON ROWS`  
  
 `FROM [Adventure Works])`  
  
 Diese Verwendung einer SELECT-Anweisung in der FROM-Klausel wird auch als untergeordneter SELECT-Ausdruck bezeichnet.  
  
 Ein weiteres gängiges Szenario für die Verwendung von Teilcubeausdrücken sind Zuweisungen mit definiertem Bereich in einem MDX-Skript. Im folgenden Beispiel wird die SCOPE-Anweisung verwendet, um eine Zuweisung auf einen Teilcube zu beschränken, der aus [Measures].[Internet Sales Amount] besteht:  
  
 `SCOPE([Measures].[Internet Sales Amount]);`  
  
 `This=1;`  
  
 `END SCOPE;`  
  
 Ein Teilcubebezeichner wird als *Subcube_Name*angezeigt. in BNF-Schreibweisebeschreibungen von MDX-Anweisungen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Die grundlegende MDX-Abfrage &#40;MDX-&#41;](/analysis-services/multidimensional-models/mdx/mdx-query-the-basic-query)   
 [Entwickeln von Teilcubes in MDX-&#40;MDX&#41;](/analysis-services/multidimensional-models/mdx/building-subcubes-in-mdx-mdx)   
 [CREATE SUBCUBE-Anweisung &#40;MDX-&#41;](../mdx/mdx-data-definition-create-subcube.md)   
 [Ausdrücke &#40;MDX-&#41;](../mdx/expressions-mdx.md)   
 [SCOPE-Anweisung &#40;MDX&#41;](../mdx/mdx-scripting-scope.md)  
  
