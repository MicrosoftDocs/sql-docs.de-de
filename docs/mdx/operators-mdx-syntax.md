---
description: Operatoren (MDX-Syntax)
title: Operatoren (MDX-Syntax) | Microsoft-Dokumentation
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: b9ad1f77a8e023d55a34e64d6c40ad956b0dac92
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92193490"
---
# <a name="operators-mdx-syntax"></a>Operatoren (MDX-Syntax)


  In MDX (Multidimensional Expressions) können Sie mit Operatoren die folgenden Aktionen ausführen:  
  
-   Ändern von Daten, temporär oder dauerhaft.  
  
-   Suchen nach Werten oder Objekten, die mit einer angegebenen Bedingung übereinstimmen.  
  
-   Implementieren einer Entscheidung zwischen Werten oder Ausdrücken.  
  
-   Testen hinsichtlich bestimmter Bedingungen vor dem Beginn einer Transaktion oder dem Ausführen eines Commits für eine Transaktion bzw. vor dem Ausführen bestimmter Anweisungen.  
  
 MDX unterstützt die Operatoren, die in der folgenden Tabelle aufgelistet sind:  
  
|Art der auszuführenden Operation|Verwendung|  
|---------------------------------------|---------|  
|Weist einer Variablen einen Wert zu oder ordnet eine Resultsetspalte einem Alias zu.|[Zuweisungsoperatoren](../mdx/assignment-operators.md)|  
|Addition, Subtraktion, Multiplikation, Division.|[Arithmetic Operators (Arithmetische Operatoren)](../mdx/arithmetic-operators.md)|  
|Testen, ob eine Bedingung wahr ist (z. B. AND, OR, NOT oder XOR).|[Bitwise Operators (Bitweise Operatoren)](../mdx/bitwise-operators.md)|  
|Vergleichen eines Werts mit einem anderen Wert oder einem Ausdruck.|[Comparison Operators (Vergleichsoperatoren)](../mdx/comparison-operators.md)|  
|Dauerhaftes oder temporäres Kombinieren von zwei Zeichenfolgen zu einer Zeichenfolge.|[Verkettungsoperatoren](../mdx/concatenation-operators.md)|  
|Dauerhaftes oder temporäres Kombinieren von zwei Mengenausdrücken zu einer Menge.|[Mengenoperatoren](../mdx/set-operators.md)|  
|Ausführen einer Operation für einen Operanden.|[Unary Operators (Unäre Operatoren)](../mdx/unary-operators.md)|  
  
> [!NOTE]  
>  In Abfragen kann jeder Benutzer Operationen ausführen, sofern die Daten in dem Cube, der mit einem Operator verwendet werden soll, für diesen Benutzer sichtbar sind. Sie benötigen allerdings die entsprechenden Berechtigungen, um die Daten erfolgreich ändern zu können.  
  
 Wenn mehrere Operatoren verwendet werden, spielt die Reihenfolge eine Rolle, in der MDX die Operatoren auswertet. Darüber hinaus kann es für das Verwenden von Operatoren erforderlich sein, dass ein Datentyp in einen anderen Datentyp konvertiert wird, bevor die Operatoren ausgewertet werden können.  
  
## <a name="evaluating-complex-expressions"></a>Auswerten von komplexen Ausdrücken  
 Sie können einen Ausdruck erstellen, indem Sie Operatoren dazu verwenden, mehrere kleinere Ausdrücke zu kombinieren. In diesen komplexen Ausdrücken wertet MDX die Operatoren entsprechend der Definition der Operator Rangfolge aus, die von verwendet wird [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] . MDX führt Operatoren mit einer höheren Position in der Rangfolge vor Operatoren mit einer niedrigeren Position in der Rangfolge aus.  
  
### <a name="understanding-operator-precedence"></a>Grundlegendes zur Rangfolge von Operatoren  
 Die folgende Liste zeigt die Operatorenrangfolge (vom höchsten bis zum niedrigsten Operator). Operatoren, die in derselben Zeile stehen, sind in der Rangfolge gleichwertig und werden von links nach rechts ausgewertet, es sei denn, durch Klammern wird eine andere Reihenfolge erzwungen:  
  
-   IS  
  
-   AS  
  
-   DISTINCT  
  
-   :  
  
-   ^  
  
-   /, *  
  
-   +, -  
  
-   EXISTING  
  
-   <>, >=, =, \<=, > , <  
  
-   NICHT  
  
-   AND  
  
-   XOR  
  
-   oder  
  
 Weitere Informationen zu Operatoren in MDX finden Sie unter [MDX-Operator Verweis &#40;MDX-&#41;](../mdx/mdx-operator-reference-mdx.md).  
  
### <a name="determining-results"></a>Bestimmen von Ergebnissen  
 Wenn Sie einfache Ausdrücke zu einem komplexen Ausdruck kombinieren, wird der Datentyp des sich ergebenden Werts bestimmt, indem die Regeln für die Operatoren mit den Regeln für die Rangfolge der Datentypen kombiniert werden.  
  
 Wenn das Ergebnis ein Zeichen- oder ein Unicode-Wert ist, wird die Sortierung des Ergebnisses bestimmt, indem die Regeln für die Operatoren mit den Regeln für die Sortierungsrangfolge kombiniert werden. Weitere Informationen zu Sortierungen finden Sie unter [Sprachen und Sortierungen &#40;Analysis Services&#41;](/analysis-services/languages-and-collations-analysis-services).  
  
 Außerdem gibt es Regeln, mit denen die Genauigkeit, die Dezimalstellen und die Länge des Ergebnisses basierend auf der Genauigkeit, der Skala und der Länge der einfachen Ausdrücke bestimmt werden.  
  
## <a name="converting-data-types"></a>Konvertieren von Datentypen  
 MDX konvertiert den Datentyp eines Objekts implizit in einen anderen Datentyp, wenn das Objekt in einem Ausdruck verwendet wird, der einen anderen Typ erfordert. In der folgenden Tabelle werden die Konvertierungsregeln für jedes Objekt aufgelistet.  
  
|Ursprünglicher Typ|Benötigter Typ|Konvertierung|  
|-------------------|-----------------|----------------|  
|Ebene|Set|\<level>. Members|  
|Hierarchy|Member|\<hierarchy>DefaultMember|  
|Member|Tupel|(\<Member>)|  
|Tupel|Member|\<tuple>. Item (0)|  
|Tupel|Skalar|\<tuple>. Wert|  
  
## <a name="see-also"></a>Weitere Informationen  
 [MDX-Operator Verweis &#40;MDX-&#41;](../mdx/mdx-operator-reference-mdx.md)   
 [MDX-Syntaxelemente &#40;MDX&#41;](../mdx/mdx-syntax-elements-mdx.md)  
  
