---
title: Path-Ausdrücke (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie, wie XQuery-Pfad Ausdrücke in einem Dokument Knoten wie Element-, Attribut-und Textknoten finden.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- axis step [XQuery]
- path expressions [XQuery]
- expressions [XQuery], path
ms.assetid: b93fa36c-bf69-46b9-b137-f597d66fd0c0
author: rothja
ms.author: jroth
ms.openlocfilehash: 6d69a916d19c519e1b313a7244a6a07feba36a67
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339989"
---
# <a name="path-expressions-xquery"></a>Pfadausdrücke (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Über XQuery-Pfadausdrücke werden in einem Dokument enthaltene Knoten gesucht, z. B. Element-, Attribut- und Textknoten. Das Ergebnis eines Pfadausdrucks erscheint immer in der Dokumentreihenfolge ohne Duplikatknoten in der Ergebnissequenz. Beim Angeben eines Pfads können Sie entweder die vollständige oder die abgekürzte Syntax verwenden. Die folgenden Informationen beziehen sich auf die ungekürzte Syntax. Die abgekürzte Syntax wird anschließend beschrieben.  
  
> [!NOTE]  
>  Da die Beispielabfragen in diesem Thema anhand der Spalten des **XML** -Typs **CatalogDescription** und **instructions** in der **ProductModel** -Tabelle angegeben werden, sollten Sie sich mit dem Inhalt und der Struktur der in diesen Spalten gespeicherten XML-Dokumente vertraut machen.  
  
 Ein Pfadausdruck kann relativ oder absolut sein. Beide sind im Folgenden beschrieben:  
  
-   Ein relativer Pfadausdruck besteht aus einem oder mehreren Schritten, die durch einen oder zwei Schrägstriche (/ oder //) getrennt sind. Beispiel: `child::Features` ist ein relativer Pfadausdruck, in dem `Child` nur auf die untergeordneten Knoten des Kontextknotens verweist. Dies ist der Knoten der gegenwärtig verarbeitet wird. Der Ausdruck ruft die untergeordneten \<Features> Elementknoten des Kontext Knotens ab.  
  
-   Ein absoluter Pfadausdruck beginnt mit einem oder zwei Schrägstrichen (/ oder //), auf die ein optionaler relativer Pfad folgt. Beispiel: Der erste Schrägstrich des Ausdrucks `/child::ProductDescription` gibt an, dass es sich um einen absoluten Pfadausdruck handelt. Da ein Schrägstrich am Anfang eines Ausdrucks den Stamm Knoten des Dokuments des Kontext Knotens zurückgibt, gibt der Ausdruck alle untergeordneten Elementknoten des Stamm Knotens zurück \<ProductDescription> .  
  
     Wenn ein absoluter Pfad mit einem einzelnen Schrägstrich beginnt, kann darauf ein relativer Pfad folgen. Wenn Sie nur einen Schrägstrich angeben, gibt der Ausdruck den Stammknoten des Kontextknotens zurück. Bei einem XML-Datentyp ist dies der Dokumentknoten.  
  
 Ein typischer Pfadausdruck besteht aus Schritten. Der absolute Pfad Ausdruck enthält z. b `/child::ProductDescription/child::Summary` . zwei durch einen Schrägstrich getrennte Schritte.  
  
-   Der erste Schritt ruft die untergeordneten \<ProductDescription> Elementknoten des Dokument Stamms ab.  
  
-   Der zweite Schritt ruft die untergeordneten \<Summary> Elementknoten für jeden abgerufenen \<ProductDescription> Elementknoten ab, der wiederum zum Kontext Knoten wird.  
  
 Bei einem Schritt eines Pfadausdrucks kann es sich um einen Achsen- oder um einen allgemeinen Schritt handeln.  
  
## <a name="axis-step"></a>Achsenschritt  
 Ein Achsenschritt in einem Pfadausdruck besteht aus den folgenden Teilen.  
  
 [sigen](../xquery/path-expressions-specifying-axis.md)  
 Definiert die Bewegungsrichtung. Ein Achsenschritt in einem Pfadausdruck beginnt beim Kontextknoten und navigiert zu den in der durch die Achse angegebenen Richtung erreichbaren Knoten.  
  
 [Knotentest](../xquery/path-expressions-specifying-node-test.md)  
 Gibt an, welcher Knotentyp oder welche Knotennamen ausgewählt werden sollen.  
  
 Keines oder beliebig viele optionale Prädikate  
 Filtert die Knoten, indem einige ausgewählt und andere verworfen werden.  
  
 In den folgenden Beispielen wird ein **axisstep** in den path-Ausdrücken verwendet:  
  
-   Der absolute Pfadausdruck `/child::ProductDescription` enthält nur einen Schritt. Er gibt eine Achse (`child`) und einen Knotentest (`ProductDescription`) an.  
  
-   Der relative Pfadausdruck `child::ProductDescription/child::Features` enthält zwei durch einen Schrägstrich getrennte Schritte. Beide Schritte geben eine untergeordnete Achse an. ProductDescription und Features sind Knotentests.  
  
-   Der relative Pfad Ausdruck `child::root/child::Location[attribute::LocationID=10]` enthält zwei durch einen Schrägstrich getrennte Schritte. Der erste Schritt gibt eine Achse (`child`) und einen Knotentest (`root`) an. Der zweite Schritt gibt alle drei Komponenten eines Achsenschritts an: eine Achse (child), einen Knotentest (`Location`) und ein Prädikat (`[attribute::LocationID=10]`).  
  
 Weitere Informationen zu den Komponenten eines Achsen Schritts finden Sie unter [angeben der Achse in einem Pfad Ausdrucks Schritt](../xquery/path-expressions-specifying-axis.md), [Angeben eines Knoten Tests in einem Pfad Ausdrucks](../xquery/path-expressions-specifying-node-test.md)Schritt und [Angeben von Prädikaten in einem Pfad Ausdrucks](../xquery/path-expressions-specifying-predicates.md)Schritt.  
  
## <a name="general-step"></a>Allgemeiner Schritt  
 Ein allgemeiner Schritt ist lediglich ein Ausdruck, der eine Knotensequenz auswerten muss.  
  
 Die XQuery-Implementierung in SQL Server unterstützt einen allgemeinen Schritt als ersten Schritt eines Pfadausdrucks. In den folgenden Beispielen werden Pfadausdrücke mit allgemeinen Schritten verwendet:  
  
```  
(/a, /b)/c  
id(/a/b)  
```  
  
 Weitere Informationen zur ID-Funktion finden Sie unter [ID Function &#40;XQuery&#41;](../xquery/functions-on-sequences-id.md).  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 [Angeben einer Achse in einem Pfadausdrucksschritt](../xquery/path-expressions-specifying-axis.md)  
 Beschreibt das Arbeiten mit dem Achsenschritt in einem Pfadausdruck.  
  
 [Angeben eines Knotentests in einem Pfadausdrucksschritt](../xquery/path-expressions-specifying-node-test.md)  
 Beschreibt das Arbeiten mit Knotentests in einem Pfadausdruck.  
  
 [Angeben von Prädikaten in einem Pfadausdrucksschritt](../xquery/path-expressions-specifying-predicates.md)  
 Beschreibt das Arbeiten mit Prädikaten in einem Pfadausdruck.  
  
 [Verwenden abgekürzter Syntax in einem Pfadausdruck](../xquery/path-expressions-using-abbreviated-syntax.md)  
 Beschreibt das Arbeiten mit einer abgekürzten Syntax in einem Pfadausdruck.  
  
  
