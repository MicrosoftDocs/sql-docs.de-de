---
title: Angeben eines Knoten Tests in einem Pfad Ausdrucks Schritt | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen Knoten Test im Achsen Schritt eines XQuery-Pfad Ausdrucks angeben.
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
- node test [XQuery]
ms.assetid: ffe27a4c-fdf3-4c66-94f1-7e955a36cadd
author: rothja
ms.author: jroth
ms.openlocfilehash: 578d10bc078edf886eb7980c3b7a099c99ef59a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338544"
---
# <a name="path-expressions---specifying-node-test"></a>Pfadausdrücke – Angeben eines Knotentests
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Ein Achsenschritt in einem Pfadausdruck besteht aus den folgenden Komponenten:  
  
-   [Einer Achse](../xquery/path-expressions-specifying-axis.md)  
  
-   Einem Knotentest  
  
-   [Null oder mehr Schrittqualifizierern (optional)](../xquery/path-expressions-specifying-predicates.md)  
  
 Weitere Informationen finden Sie unter [Path Expressions &#40;XQuery&#41;](../xquery/path-expressions-xquery.md).  
  
 Ein Knotentest ist eine Bedingung und die zweite Komponente eines Achsenschritts in einem Pfadausdruck. Alle von einem Schritt ausgewählten Knoten müssen diese Bedingung erfüllen. Für den Pfadausdruck `/child::ProductDescription` ist `ProductDescription` der Knotentest. Dieser Schritt ruft nur die untergeordneten Elementknoten ab, deren Name ProductDescription lautet.  
  
 Eine Knotentestbedingung kann Folgendes umfassen:  
  
-   Einen Knotennamen. Nur Knoten der Hauptknotenart mit dem angegebenen Namen werden zurückgegeben.  
  
-   Einen Knotentyp. Nur Knoten des angegebenen Typs werden zurückgegeben.  
  
> [!NOTE]  
>  Für Knotennamen, die in XQuery Pfadausdrücken angegeben werden, gelten nicht die gleichen sortierungsabhängigen Regeln, die für Transact-SQL-Abfragen gelten. Außerdem wird immer Groß- und Kleinschreibung unterschieden.  
  
## <a name="node-name-as-node-test"></a>Knotenname als Knotentest  
 Wenn Sie einen Knotennamen als Knotentest in einem Schritt eines Pfadausdrucks angeben, sollten Sie das Konzept der Hauptknotenart verstanden haben. Jede Achse (child, parent, attribute und self) verfügt über eine Hauptknotenart. Beispiel:  
  
-   Eine attribute-Achse kann nur Attribute enthalten. Daher ist der attribute-Knoten die Hauptknotenart der attribute-Achse.  
  
-   Für andere Achsen ist element die Hauptknotenart für die betreffende Achse, wenn die von der Achse ausgewählten Knoten element-Knoten enthalten können.  
  
 Wenn Sie einen Knotennamen als Knotentest angeben, gibt der Schritt die folgenden Knotentypen zurück:  
  
-   Knoten, die der Hauptknotenart der Achse entsprechen.  
  
-   Knoten, die den gleichen Namen wie im Knotentest angegeben besitzen.  
  
 Angenommen, der folgende Pfadausdruck liegt vor:  
  
```  
child::ProductDescription   
```  
  
 Dieser Ausdruck mit nur einem Schritt gibt eine `child`-Achse und den Knotennamen `ProductDescription` als Knotentest an. Der Ausdruck gibt nur die Knoten zurück, die der Hauptknotenart der untergeordneten Achse angehören, element-Knoten und Knoten, die ProductDescription als Namen tragen.  
  
 Der Pfadausdruck `/child::PD:ProductDescription/child::PD:Features/descendant::*,` weist drei Schritte auf. Diese Schritte geben child- und descendant-Achsen an. In jedem Schritt wird der Knotenname als Knotentest angegeben. Das Platzhalterzeichen (`*`) im dritten Schritt gibt alle Knoten von der Hauptknotenart für die descendant-Achse an. Die Hauptknotenart der Achse bestimmt den Typ der ausgewählten Knoten sowie die Knotennamenfilter, die die Knoten ausgewählt haben.  
  
 Wenn dieser Ausdruck für Produktkatalog-XML-Dokumente in der **ProductModel** -Tabelle ausgeführt wird, werden daher alle untergeordneten Elementknoten des untergeordneten \<Features> Element Knotens des- \<ProductDescription> Elements abgerufen.  
  
 Der Pfad Ausdruck `/child::PD:ProductDescription/attribute::ProductModelID` besteht aus zwei Schritten. Beide Schritte geben einen Knotennamen als Knotentest an. Der zweite Schritt verwendet außerdem die attribute-Achse. Daher wählt jeder Schritt Knoten der Hauptknotenart seiner Achse aus, die den Namen als Knotentest angegeben hat. Folglich gibt der Ausdruck den **ProductModelID** -Attribut Knoten des \<ProductDescription> Element Knotens zurück.  
  
 Wenn Sie die Namen von Knoten für Knotentests angeben, können Sie auch das Platzhalterzeichen (*) zum Angeben des lokalen Namens eines Knotens oder seines Namespacepräfixes verwenden, wie im folgenden Beispiel gezeigt wird:  
  
```  
declare @x xml  
set @x = '  
<greeting xmlns="ns1">  
   <salutation>hello</salutation>  
</greeting>  
<greeting xmlns="ns2">  
   <salutation>welcome</salutation>  
</greeting>  
<farewell xmlns="ns1" />'  
select @x.query('//*:greeting')  
select @x.query('declare namespace ns="ns1"; /ns:*')  
```  
  
## <a name="node-type-as-node-test"></a>Knotentyp als Knotentest  
 Um andere Knotentypen als element-Knoten abzufragen, verwenden Sie einen Knotentyptest. Wie in der folgenden Tabelle gezeigt, sind vier Knotentyptests verfügbar.  
  
|Knotentyp|Gibt zurück|Beispiel|  
|---------------|-------------|-------------|  
|`comment()`|True für einen Kommentarknoten.|`following::comment()` wählt alle Kommentarknoten aus, die nach dem Kontextknoten auftreten.|  
|`node()`|True für einen beliebigen Knoten.|`preceding::node()` wählt alle Kommentarknoten aus, die vor dem Kontextknoten auftreten.|  
|`processing-instruction()`|True für einen Verarbeitungsanweisungsknoten.|`self::processing instruction()` wählt alle Verarbeitungsanweisungsknoten im Kontextknoten aus.|  
|`text()`|True für einen Textknoten.|`child::text()` wählt alle Textknoten aus, die untergeordnete Elemente des Kontextknotens sind.|  
  
 Wenn der Knotentyp, z. B. text() oder comment() ..., als Knotentest angegeben wird, gibt der Schritt unabhängig von der Hauptknotenart der Achse nur Knoten der angegebenen Art zurück. Der folgende Pfadausdruck gibt z. B. nur die untergeordneten Kommentarknoten des Kontextknotens zurück:  
  
```  
child::comment()  
```  
  
 Ebenso `/child::ProductDescription/child::Features/child::comment()` Ruft die untergeordneten Kommentar Knoten des untergeordneten \<Features> Element Knotens des \<ProductDescription> Element Knotens ab.  
  
## <a name="examples"></a>Beispiele  
 Die folgenden Beispiele vergleichen den Knotennamen und die Knotenart.  
  
### <a name="a-results-of-specifying-the-node-name-and-the-node-type-as-node-tests-in-a-path-expression"></a>A. Ergebnisse der Angabe des Knotennamens und des Knotentyps als Knotentests in einem Pfadausdruck  
 Im folgenden Beispiel wird ein einfaches XML-Dokument einer Variablen vom Typ **XML** zugewiesen. Das Dokument wird mithilfe verschiedener Pfadausdrücke abgefragt. Anschließend werden die Ergebnisse verglichen.  
  
```  
declare @x xml  
set @x='  
<a>  
 <b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
 </b>  
</a>'  
select @x.query('  
/child::a/child::b/descendant::*  
')  
```  
  
 Dieser Ausdruck fragt die nachfolgenden Elementknoten des `<b>`-Elementknotens ab.  
  
 Das Sternchen (`*`) im Knotentest zeigt ein Platzhalterzeichen für den Knotennamen an. Die descendant-Achse besitzt den element-Knoten als Hauptknotenart. Daher gibt der Ausdruck alle nachfolgenden Elementknoten des `<b>`-Elementknotens zurück. Wie im folgenden Beispiel gezeigt, werden die Elementknoten `<c>` und `<d>` zurückgegeben:  
  
```  
<c>text2  
     <d>text3</d>  
</c>  
<d>text3</d>  
```  
  
 Wenn Sie eine descendant-or-self-Achse statt einer descendant-Achse angeben, werden der Kontextknoten und seine Nachfolger zurückgegeben:  
  
```  
/child::a/child::b/descendant-or-self::*  
```  
  
 Dieser Ausdruck gibt den Elementknoten `<b>` und seine nachfolgenden Elementknoten zurück. Durch Zurückgeben der nachfolgenden Knoten bestimmt die Hauptknotenart der descendant-or-self-Achse, der Elementknotentyp, welche Arten von Knoten zurückgegeben werden.  
  
 Dies ist das Ergebnis:  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
  
<c>text2  
     <d>text3</d>  
</c>  
  
<d>text3</d>   
```  
  
 Der vorherige Ausdruck verwendete ein Platzhalterzeichen als Knotennamen. Sie können jedoch auch die `node()`-Funktion wie im folgenden Ausdruck gezeigt verwenden:  
  
```  
/child::a/child::b/descendant::node()  
```  
  
 Da `node()` ein Knotentyp ist, erhalten Sie alle Knoten der nachfolgenden Achse. Dies ist das Ergebnis:  
  
```  
text1  
<c>text2  
     <d>text3</d>  
</c>  
text2  
<d>text3</d>  
text3  
```  
  
 Wenn Sie erneut die descendant-or-self-Achse und `node()` als Knotentest angeben, erhalten Sie alle Nachfolger, Elemente und Textknoten sowie den Kontextknoten, das `<b>`-Element.  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
text1  
<c>text2  
     <d>text3</d>  
</c>  
text2  
<d>text3</d>  
text3  
```  
  
### <a name="b-specifying-a-node-name-in-the-node-test"></a>B. Angeben eines Knotennamens im Knotentest  
 Das folgende Beispiel gibt einen Knotennamen als Knotentest in allen Pfadausdrücken an. Als Ergebnis geben alle Ausdrücke Knoten der Hauptknotenart der Achse zurück, für die der Knotenname im Knotentest angegeben wurde.  
  
 Der folgende Abfrage Ausdruck gibt das <`Warranty`>-Element aus dem in der-Tabelle gespeicherten Produktkatalog-XML-Dokument zurück `Production.ProductModel` :  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::wm:Warranty  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Das `namespace`-Schlüsselwort im XQuery-Prolog definiert ein Präfix, das im Abfragetext verwendet wird. Weitere Informationen zum XQuery-Prolog finden Sie unter [XQuery-Prolog](../xquery/modules-and-prologs-xquery-prolog.md) .  
  
-   Alle drei Schritte im Pfadausdruck geben die untergeordnete Achse und einen Knotennamen als Knotentest an.  
  
-   Der optionale Schrittqualifiziereranteil des Achsenschritts wird in keinem der Schritte im Ausdruck angegeben.  
  
 Die Abfrage gibt die <`Warranty`>-Elemente der untergeordneten Elemente des <> Elements zurück, das `Features` dem <> Element untergeordnet ist `ProductDescription` .  
  
 Dies ist das Ergebnis:  
  
```  
<wm:Warranty xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
  <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
  <wm:Description>parts and labor</wm:Description>  
</wm:Warranty>     
```  
  
 In der folgenden Abfrage gibt der Pfadausdruck ein Platzhalterzeichen (`*`) in einem Knotentest an.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::*  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Das Platzhalterzeichen wird für den Knotennamen angegeben. Folglich gibt die Abfrage alle untergeordneten Elementknoten des untergeordneten Knotens <`Features`> Element des `ProductDescription` Knotens <> Element zurück.  
  
 Die folgende Abfrage ähnelt der vorherigen Abfrage; es wird nur ein Namespace zusammen mit dem Platzhalterzeichen angegeben. Als Ergebnis werden alle untergeordneten Elementknoten im betreffenden Namespace zurückgegeben. Beachten Sie, dass das <`Features`>-Element Elemente aus unterschiedlichen Namespaces enthalten kann.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::wm:*  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Sie können das Platzhalterzeichen als Namespacepräfix verwenden, wie in der folgenden Abfrage gezeigt wird:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::*:Maintenance  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Diese Abfrage gibt die <untergeordneten `Maintenance`> Elementknoten in allen Namespaces aus dem Produktkatalog-XML-Dokument zurück.  
  
### <a name="c-specifying-node-kind-in-the-node-test"></a>C. Angeben der Knotenart im Knotentest  
 Das folgende Beispiel gibt die Knotenart als Knotentest in allen path-Ausdrücken an. Als Ergebnis geben alle Ausdrücke Knoten der Art zurück, die im Knotentest angegeben wurde.  
  
 In der folgenden Abfrage geben die path-Ausdrücke eine Knotenart im dritten Schritt zurück:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::PD:Features/child::text()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 In der nächsten Abfrage wird Folgendes angegeben:  
  
-   Der Pfadausdruck enthält drei durch einen Schrägstrich (`/`) getrennte Schritte.  
  
-   Jeder dieser Schritte gibt eine untergeordnete Achse an.  
  
-   Die ersten beiden Schritte geben einen Knotennamen als Knotentest an, und der dritte Schritt gibt eine Knotenart als Knotentest an.  
  
-   Der Ausdruck gibt die untergeordneten Textknoten des <>-Elements zurück, `Features` das dem <> Element-Knoten untergeordnet ist `ProductDescription` .  
  
 Es wird nur ein Textknoten zurückgegeben. Dies ist das Ergebnis:  
  
```  
These are the product highlights.   
```  
  
 Die folgende Abfrage gibt die untergeordneten Kommentar Knoten des <`ProductDescription`>-Elements zurück:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::PD:ProductDescription/child::comment()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Der zweite Schritt gibt eine Knotenart als Knotentest an.  
  
-   Folglich gibt der Ausdruck die untergeordneten Kommentar Knoten des <`ProductDescription`> Element-Knoten zurück.  
  
 Dies ist das Ergebnis:  
  
```  
<!-- add one or more of these elements... one for each specific product in this product model -->  
<!-- add any tags in <specifications> -->      
```  
  
 Die folgende Abfrage ruft die Verarbeitungsanweisungsknoten der obersten Ebene ab:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::processing-instruction()  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Dies ist das Ergebnis:  
  
```  
<?xml-stylesheet href="ProductDescription.xsl" type="text/xsl"?>   
```  
  
 Sie können einen Zeichenfolgenliteral-Parameter an den `processing-instruction()`-Knotentest übergeben. In diesem Fall gibt die Abfrage die Verarbeitungsanweisungen zurück, deren Attributwert das Zeichenfolgenliteral ist, das im Argument angegeben wird.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
 /child::processing-instruction("xml-stylesheet")  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
## <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden spezifischen Einschränkungen sind zu beachten:  
  
-   Die erweiterten SequenceType-Knotentests werden nicht unterstützt.  
  
-   processing-instruction(name) wird nicht unterstützt. Schließen Sie den Namen stattdessen in Anführungszeichen ein.  
  
  
