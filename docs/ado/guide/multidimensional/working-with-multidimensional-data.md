---
description: Arbeiten mit mehrdimensionalen Daten
title: Arbeiten mit mehrdimensionalen Daten | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- multidimensional data [ADO]
ms.assetid: 84387746-aa3e-44fd-ad6c-a8214a6966dc
author: rothja
ms.author: jroth
ms.openlocfilehash: 33ae3c1ca1cb8f89378b4f2ea468b41d6d3a4ac5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036620"
---
# <a name="working-with-multidimensional-data"></a>Arbeiten mit mehrdimensionalen Daten
Ein *Cellset* ist das Ergebnis einer Abfrage für mehrdimensionale Daten. Sie besteht aus einer Auflistung von Achsen, in der Regel nicht mehr als vier Achsen und in der Regel nur zwei oder drei Achsen. Eine *Achse* ist eine Auflistung von Membern aus einer oder mehreren Dimensionen, die zum Suchen oder Filtern bestimmter Werte in einem Cube verwendet wird.  
  
 Eine *Position* ist ein Punkt entlang einer Achse. Bei einer Achse, die aus einer einzelnen Dimension besteht, sind diese Positionen eine Teilmenge der Dimensions Elemente. Wenn eine Achse aus mehr als einer Dimension besteht, ist jede Position eine Verbund Entität, die *n* Teile hat, wobei *n* die Anzahl der Dimensionen ist, die auf dieser Achse ausgerichtet sind. Jeder Teil der Position ist ein Element aus einer einzelnen konstituierenden Dimension.  
  
 Wenn z. b. die Geography-und Product-Dimensionen eines Cubes, der Umsatzdaten enthält, entlang der x-Achse eines Cellsets ausgerichtet sind, kann eine Position entlang dieser Achse die Elemente "USA" und "Computer" enthalten. In diesem Beispiel ist es erforderlich, dass Elemente aus jeder Dimension auf der Achse ausgerichtet sind, wenn Sie eine Position entlang der x-Achse ermitteln.  
  
 Eine *Zelle* ist ein Objekt, das an der Schnittmenge der Achsen Koordinaten positioniert ist. Jeder Zelle sind mehrere Informationen zugeordnet, darunter die Daten selbst, eine formatierte Zeichenfolge (die anzeigbare Form von Zelldaten) und der Wert für die Ordnungszahl der Zelle. (Jede Zelle ist ein eindeutiger Ordinalwert im Cellset. Der Ordinalwert der ersten Zelle im Cellset ist 0 (null), während die linke Zelle in der zweiten Zeile eines Cellsets mit acht Spalten einen Ordinalwert von 8 hat.)  
  
 Ein Cube hat z. b. die folgenden sechs Dimensionen (Beachten Sie, dass sich dieses Cubeschema geringfügig von dem Beispiel unter [Übersicht über mehrdimensionale Schemas und Daten](./overview-of-multidimensional-schemas-and-data.md)unterscheidet):  
  
-   Salesperson  
  
-   Geography (natürliche Hierarchie): Kontinente, Länder, Bundesstaaten usw.  
  
-   Quartale, Monate, Tage  
  
-   Jahre  
  
-   Measures-Sales, Prozentänderung, BudgetedSales  
  
-   Produkte  
  
 Das folgende Cellset repräsentiert Verkäufe für 1991 für alle Produkte:  
  
> [!NOTE]
>  Die Zellwerte im Beispiel können als geordnete Paare von Achsen Positions Ordinalzahlen angezeigt werden, wobei die erste Ziffer die Position der x-Achse und die zweite Ziffer der Position der y-Achse darstellt.  
  
 Die Merkmale dieses Cellsets lauten wie folgt:  
  
-   Achsen Dimensionen: Quartale, Vertriebsmitarbeiter, Geografie  
  
-   Filter Dimensionen: Measures, Jahre, Produkte  
  
-   Zwei Achsen: column (x, Achse 0) und Zeile (y oder Achse 1)  
  
-   x-Achse: zwei unter-und Geografiedimensionen, SalesPerson und geography  
  
-   y-Achse: Quartale-Dimension  
  
 Die x-Achse verfügt über zwei eingefügte Dimensionen: "SalesPerson" und "geography". Aus geography werden vier Mitglieder ausgewählt: Seattle, Boston, USA-Süd und Japan. Zwei Mitglieder werden von SalesPerson ausgewählt: Valentine und Nash. Dies ergibt insgesamt acht Positionen auf dieser Achse (8 = 4 * 2).  
  
 Jede Koordinate wird als Position mit zwei Membern dargestellt: eine von der SalesPerson-Dimension und eine andere aus der Geography-Dimension:  
  
```console
(Valentine, Seattle), (Valentine, Boston), (Valentine, USA_North),  
(Valentine, Japan), (Nash, Seattle), (Nash, Boston), (Nash, USA_North),  
(Nash, Japan)  
```  
  
 Die y-Achse verfügt nur über eine Dimension mit den folgenden acht Positionen:  
  
```console
Jan, Feb, Mar, Qtr2, Qtr3, Oct, Nov, Dec  
```  
  
 Cellsets, Zellen, Achsen und Positionen werden alle in ADO MD durch die entsprechenden Objekte dargestellt: [Cellset](../../reference/ado-md-api/cellset-object-ado-md.md), [Cell](../../reference/ado-md-api/cell-object-ado-md.md), [Axis](../../reference/ado-md-api/axis-object-ado-md.md)und [Position](../../reference/ado-md-api/position-object-ado-md.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [ADO MD-Objektmodell](../../reference/ado-md-api/ado-md-object-model.md)   
 [ADO (mehrdimensional) (ADO MD)](./ado-multidimensional-ado-md.md)   
 [Übersicht über mehrdimensionale Schemas und Daten](./overview-of-multidimensional-schemas-and-data.md)   
 [Programmieren mit ADO MD](./programming-with-ado-md.md)   
 [Verwenden von ADO mit ADO MD](./using-ado-with-ado-md.md)