---
title: Angeben von Prädikaten in einem Pfad Ausdrucks Schritt | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Prädikate im Achsen Schritt eines XQuery-Pfad Ausdrucks angeben, um eine XML-Knoten Sequenz zu filtern.
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
- predicates [XQuery]
- qualifiers [XQuery]
- path expressions [XQuery]
ms.assetid: 2660ceca-b8b4-4a1f-98a0-719ad5f89f81
author: rothja
ms.author: jroth
ms.openlocfilehash: ef2625be70c2af44f4a45ba00302b3d26afc101d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339945"
---
# <a name="path-expressions---specifying-predicates"></a>Pfadausdrücke – Angeben von Prädikaten
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Wie im Thema [Path Expressions in XQuery](../xquery/path-expressions-xquery.md)beschrieben, umfasst ein Achsen Schritt in einem Pfad Ausdruck die folgenden Komponenten:  
  
-   [Eine-Achse](../xquery/path-expressions-specifying-axis.md).  
  
-   Einen Knotentest. Weitere Informationen finden Sie unter [Angeben eines Knoten Tests in einem Pfad Ausdrucks Schritt](../xquery/path-expressions-specifying-node-test.md).  
  
-   Null oder mehr Prädikate. Dies ist optional.  
  
 Das optionale Prädikat ist der dritte Teil des Achsenschritts in einem Pfadausdruck.  
  
## <a name="predicates"></a>Prädikate  
 Ein Prädikat wird zum Filtern einer Knotensequenz durch Anwenden eines angegebenen Tests verwendet. Der Prädikatausdruck ist in eckige Klammern eingeschlossen und an den letzten Knoten in einem Pfadausdruck gebunden.  
  
 Nehmen Sie beispielsweise an, dass ein SQL-Parameterwert (x) des **XML** -Datentyps deklariert ist, wie im folgenden Beispiel gezeigt:  
  
```  
declare @x xml  
set @x = '  
<People>  
  <Person>  
    <Name>John</Name>  
    <Age>24</Age>  
  </Person>  
  <Person>  
    <Name>Goofy</Name>  
    <Age>54</Age>  
  </Person>  
  <Person>  
    <Name>Daffy</Name>  
    <Age>30</Age>  
  </Person>  
</People>  
'  
```  
  
 In diesem Fall sind die folgenden Ausdrücke gültig, die einen Prädikatwert von [1] für jede der drei unterschiedlichen Knotenebenen verwenden:  
  
```  
select @x.query('/People/Person/Name[1]')  
select @x.query('/People/Person[1]/Name')  
select @x.query('/People[1]/Person/Name')  
```  
  
 Beachten Sie, dass in jedem Fall das Prädikat an den Knoten im Pfadausdruck gebunden wird, für den es angewendet wird. Beispielsweise wählt der erste Pfad Ausdruck das erste <`Name`>-Element in jedem/People/Person-Knoten aus, und gibt mit der bereitgestellten XML-Instanz Folgendes zurück:  
  
```  
<Name>John</Name><Name>Goofy</Name><Name>Daffy</Name>  
```  
  
 Der zweite Pfad Ausdruck wählt jedoch alle <`Name`> Elemente aus, die unter dem ersten/People/Person-Knoten liegen. Deshalb gibt er das folgende Ergebnis zurück:  
  
```  
<Name>John</Name>  
```  
  
 Es können auch Klammern verwendet werden, um die Auswertungsreihenfolge des Prädikats zu ändern. So wird z. B. im folgenden Ausdruck ein Klammernsatz verwendet, um den Pfad von (/People/Person/Name) vom Prädikat [1] zu trennen:  
  
```  
select @x.query('(/People/Person/Name)[1]')  
```  
  
 In diesem Beispiel ändert sich die Reihenfolge, in der das Prädikat angewendet wird. Das liegt daran, dass der in Klammern eingeschlossene Pfad (/People/Person/Name) zuerst ausgewertet und anschließend der Prädikatoperator [1] für das Set angewendet wird, das alle mit dem eingeschlossenen Pfad übereinstimmenden Knoten enthält. Ohne die Klammern wäre die Reihenfolge des Vorgangs anders: Der Prädikatoperator [1] würde dann als ein `child::Name`-Knotentest angewendet werden, wie das im ersten Beispiel für den Pfadausdruck der Fall war.  
  
### <a name="quantifiers-and-predicates"></a>Quantifizierer und Prädikate  
 Quantifizierer können innerhalb der Klammern des Prädikats mehrfach verwendet und hinzugefügt werden. So wäre z. B. unter Nutzung des vorigen Beispiels folgende Verwendung von mehreren Quantifizierern innerhalb eines komplexen Prädikatunterausdrucks gültig.  
  
```  
select @x.query('/People/Person[contains(Name[1], "J") and xs:integer(Age[1]) < 40]/Name/text()')  
```  
  
 Das Ergebnis eines Prädikatausdrucks wird in einen booleschen Wert konvertiert und wird als Wahrheitswert des Prädikats bezeichnet. Im Ergebnis werden nur die Knoten in der Sequenz zurückgegeben, deren Prädikatwahrheitswert True ist. Alle anderen Knoten werden verworfen.  
  
 Beim folgenden Pfadausdruck ist z. B. ein Prädikat in den zweiten Schritt eingeschlossen:  
  
```  
/child::root/child::Location[attribute::LocationID=10]  
```  
  
 Die von diesem Prädikat angegebene Bedingung wird auf alle untergeordneten `Location` Knoten des Knotens <> Elements angewendet. Das führt dazu, dass als Ergebnis nur solche Arbeitsplatzstandorte zurückgegeben werden, bei denen der Attributwert der Standortkennung (LocationID) 10 ist.  
  
 Der vorige Pfadausdruck wird in der folgenden SELECT-Anweisung ausgeführt:  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
 /child::AWMI:root/child::AWMI:Location[attribute::LocationID=10]  
')  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
### <a name="computing-predicate-truth-values"></a>Berechnen des Prädikatwahrheitswerts  
 Gemäß den XQuery-Spezifikationen werden die folgenden Regeln angewendet, um den Prädikatwahrheitswert zu ermitteln:  
  
1.  Wenn der Wert des Prädikatausdrucks eine leere Sequenz ist, ist der Prädikatwahrheitswert False.  
  
     Beispiel:  
  
    ```  
    SELECT Instructions.query('  
    declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
     /child::AWMI:root/child::AWMI:Location[attribute::LotSize]  
    ')  
    FROM Production.ProductModel  
    WHERE ProductModelID=7  
    ```  
  
     Der Pfad Ausdruck in dieser Abfrage gibt nur die <`Location`> Elementknoten zurück, für die ein LotSize-Attribut angegeben ist. Wenn das Prädikat eine leere Sequenz für eine bestimmte <> zurückgibt `Location` , wird der Arbeitsplatz Speicherort nicht im Ergebnis zurückgegeben.  
  
2.  Prädikat Werte können nur xs: Integer, xs: Boolean oder Node lauten \* . Für Node \* wird das Prädikat als true ausgewertet, wenn es Knoten gibt, und false für eine leere Sequenz. Alle anderen numerischen Typen wie z. B. double und float generieren einen statischen Typisierungsfehler. Der Prädikatwahrheitswert eines Ausdrucks ist ausschließlich dann True, wenn die resultierende ganze Zahl gleich dem Wert der Kontextposition ist. Außerdem verringern nur ganzzahlige Literalwerte und die **Last ()** -Funktion die Kardinalität des gefilterten Schritt Ausdrucks auf 1.  
  
     Die folgende Abfrage ruft z. b. den dritten untergeordneten Elementknoten des <`Features`>-Elements ab.  
  
    ```  
    SELECT CatalogDescription.query('  
    declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
    declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
     /child::PD:ProductDescription/child::PD:Features/child::*[3]  
    ')  
    FROM Production.ProductModel  
    WHERE ProductModelID=19  
    ```  
  
     Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
    -   Der dritte Schritt im Ausdruck gibt einen Prädikatausdruck mit dem Wert 3 an. Daher ist der Prädikatwahrheitswert dieses Ausdrucks nur für die Knoten True, deren Kontextposition 3 ist.  
  
    -   Der dritte Schritt gibt auch ein Platzhalterzeichen (*) an, das alle Knoten im Knotentest anzeigt. Das Prädikat filtert jedoch die Knoten und gibt nur den Knoten in der dritten Position zurück.  
  
    -   Die Abfrage gibt den dritten untergeordneten Elementknoten des <`Features`> untergeordneten Elementen des <`ProductDescription`> Elements des Dokument Stamms zurück.  
  
3.  Wenn der Wert des Prädikatausdrucks ein einfacher Werttyp des booleschen Datentyps ist, entspricht der Prädikatwahrheitswert dem Wert des Prädikatausdrucks.  
  
     Beispielsweise wird die folgende Abfrage für eine Variable vom Typ **XML** angegeben, die eine XML-Instanz enthält, die XML-Instanz der Kundenumfrage. Die Abfrage ruft solche Kunden ab, die über untergeordnete Elemente verfügen. In dieser Abfrage lautet der Wert \<HasChildren> 1 \</HasChildren> .  
  
    ```  
    declare @x xml  
    set @x='  
    <Survey>  
      <Customer CustomerID="1" >  
      <Age>27</Age>  
      <Income>20000</Income>  
      <HasChildren>1</HasChildren>  
      </Customer>  
      <Customer CustomerID="2" >  
      <Age>27</Age>  
      <Income>20000</Income>  
      <HasChildren>0</HasChildren>  
      </Customer>  
    </Survey>  
    '  
    declare @y xml  
    set @y = @x.query('  
      for $c in /child::Survey/child::Customer[( child::HasChildren[1] cast as xs:boolean ? )]  
      return   
          <CustomerWithChildren>  
              { $c/attribute::CustomerID }  
          </CustomerWithChildren>  
    ')  
    select @y  
    ```  
  
     Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
    -   Der Ausdruck in der **for** -Schleife umfasst zwei Schritte, und der zweite Schritt gibt ein Prädikat an. Der Wert dieses Prädikats ist ein Wert des booleschen Datentyps. Wenn dieser Wert True ist, so ist auch der Wahrheitswert des Prädikats True.  
  
    -   Die Abfrage gibt die <`Customer`> untergeordneten Elementen zurück, deren Prädikat Wert true ist, der untergeordneten \<Survey> Elemente des Dokument Stamms. Dies ist das Ergebnis:  
  
        ```  
        <CustomerWithChildren CustomerID="1"/>   
        ```  
  
4.  Wenn der Wert des Prädikatausdrucks eine Sequenz ist, die mindestens einen Knoten enthält, so ist der Wahrheitswert des Prädikats True.  
  
 Die folgende Abfrage ruft beispielsweise ProductModelID für Produktmodelle ab, deren XML-Katalogbeschreibung mindestens eine Funktion enthält, ein untergeordnetes Element des <`Features`>-Elements aus dem Namespace, der dem **WM** -Präfix zugeordnet ist.  
  
```  
SELECT ProductModelID  
FROM   Production.ProductModel  
WHERE CatalogDescription.exist('  
             declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
             declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
             /child::PD:ProductDescription/child::PD:Features[wm:*]  
             ') = 1  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Die WHERE-Klausel gibt die [exist ()-Methode (XML-Datentyp)](../t-sql/xml/exist-method-xml-data-type.md)an.  
  
-   Der Pfad Ausdruck in der **exist ()** -Methode gibt ein Prädikat im zweiten Schritt an. Wenn der Prädikatausdruck eine Sequenz von mindestens einer Funktion zurückgibt, ist der Wahrheitswert dieses Prädikatausdrucks True. In diesem Fall wird die ProductModelID zurückgegeben, da die **exist ()** -Methode "true" zurückgibt.  
  
## <a name="static-typing-and-predicate-filters"></a>Statische Typisierung und Prädikatfilter  
 Die Prädikate können sich auch auf den statisch abgeleiteten Typ eines Ausdrucks auswirken. Ganzzahlige Literalwerte und die **Last ()** -Funktion verringern die Kardinalität des gefilterten Schritt Ausdrucks auf höchstens einen Wert.  
  
  
