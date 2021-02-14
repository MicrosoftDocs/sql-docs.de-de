---
title: Sequenz Ausdrücke (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über XQuery-Sequenz Ausdrücke, die eine Sequenz von Elementen erstellen, Filtern und kombinieren.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sequence [XQuery]
- expressions [XQuery], sequence
- filtering sequences [XQuery]
ms.assetid: 41e18b20-526b-45d2-9bd9-e3b7d7fbce4e
author: rothja
ms.author: jroth
ms.openlocfilehash: ea654a45894791fa22ecfa843c44cdd6214a5181
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351064"
---
# <a name="sequence-expressions-xquery"></a>Sequenzausdrücke (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützt die XQuery-Operatoren, die zum Konstruieren, Filtern und Kombinieren einer Sequenz von Elementen verwendet werden. Bei einem Element kann es sich um einen atomaren Wert oder einen Knoten handeln.  
  
## <a name="constructing-sequences"></a>Konstruieren von Sequenzen  
 Mithilfe des Kommaoperators können Sie eine Sequenz konstruieren, die Items zu einer einzigen Sequenz verkettet.  
  
 Eine Sequenz kann doppelte Werte enthalten. Geschachtelte Sequenzen, also eine Sequenz innerhalb einer Sequenz, werden reduziert. So wird z. B. die Sequenz (1, 2, (3, 4, (5))) zu (1, 2, 3, 4, 5). Es folgen Beispiele für das Konstruieren von Sequenzen.  
  
### <a name="example-a"></a>Beispiel A  
 Die folgende Abfrage gibt eine Sequenz aus fünf atomaren Werten zurück:  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1,2,3,4,5)')  
go  
-- result 1 2 3 4 5  
```  
  
 Die folgende Abfrage gibt eine Sequenz aus zwei Knoten zurück:  
  
```  
-- sequence of 2 nodes  
declare @x xml  
set @x=''  
select @x.query('(<a/>, <b/>)')  
go  
-- result  
<a />  
<b />  
```  
  
 Die folgende Abfrage gibt einen Fehler zurück, weil Sie eine Sequenz aus atomaren Werten und Knoten konstruieren. Dies ist eine heterogene Sequenz, die nicht unterstützt wird.  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1, 2, <a/>, <b/>)')  
go  
```  
  
### <a name="example-b"></a>Beispiel B  
 Die folgende Abfrage konstruiert eine Sequenz aus atomaren Werten, indem vier Sequenzen mit unterschiedlicher Länge zu einer einzigen Sequenz kombiniert werden.  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1,2),10,(),(4, 5, 6)')  
go  
-- result = 1 2 10 4 5 6  
```  
  
 Sie können die Sequenz mithilfe von FLOWR und ORDER BY sortieren:  
  
```  
declare @x xml  
set @x=''  
select @x.query('for $i in ((1,2),10,(),(4, 5, 6))  
                  order by $i  
                  return $i')  
go  
```  
  
 Sie können die Elemente in der Sequenz zählen, indem Sie die **FN: count ()** -Funktion verwenden.  
  
```  
declare @x xml  
set @x=''  
select @x.query('count( (1,2,3,(),4) )')  
go  
-- result = 4  
```  
  
### <a name="example-c"></a>Beispiel C  
 Die folgende Abfrage wird für die AdditionalContactInfo-Spalte des **XML** -Typs in der Contact-Tabelle angegeben. Diese Spalte speichert zusätzliche Kontaktinformationen, z. B. eine oder mehrere zusätzliche Telefonnummern, Pagernummern und Adressen. Die \<telephoneNumber> \<pager> Knoten, und können an einer beliebigen Stelle im Dokument angezeigt werden. Die Abfrage erstellt eine Sequenz, die alle untergeordneten Elemente \<telephoneNumber> des Kontext Knotens, gefolgt von den untergeordneten Elementen, enthält \<pager> . Beachten Sie die Verwendung des Kommasequenzoperators im zurückgegebenen Ausdruck `($a//act:telephoneNumber, $a//act:pager)`.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS act,  
 'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS aci)  
  
SELECT AdditionalContactInfo.query('  
   for $a in /aci:AdditionalContactInfo   
   return ($a//act:telephoneNumber, $a//act:pager)  
') As Result  
FROM Person.Contact  
WHERE ContactID=3  
```  
  
 Dies ist das Ergebnis:  
  
```  
<act:telephoneNumber xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>333-333-3333</act:number>  
</act:telephoneNumber>  
<act:telephoneNumber xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>333-333-3334</act:number>  
</act:telephoneNumber>  
<act:pager xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>999-555-1244</act:number>  
  <act:SpecialInstructions>  
Page only in case of emergencies.  
</act:SpecialInstructions>  
</act:pager>  
```  
  
## <a name="filtering-sequences"></a>Filtern von Sequenzen  
 Sie können die durch einen Ausdruck zurückgegebene Sequenz filtern, indem Sie dem Ausdruck ein Prädikat hinzufügen. Weitere Informationen finden Sie unter [Path Expressions &#40;XQuery&#41;](../xquery/path-expressions-xquery.md). Die folgende Abfrage gibt z. b. eine Sequenz von drei <`a`> Elementknoten zurück:  
  
```  
declare @x xml  
set @x = '<root>  
<a attrA="1">111</a>  
<a></a>  
<a></a>  
</root>'  
SELECT @x.query('/root/a')  
```  
  
 Dies ist das Ergebnis:  
  
```  
<a attrA="1">111</a>  
<a />  
<a />  
```  
  
 Wenn Sie nur <`a`> Elemente mit dem Attribut attrA abrufen möchten, können Sie im Prädikat einen Filter angeben. Die resultierende Sequenz weist nur eine <`a`>-Element auf.  
  
```  
declare @x xml  
set @x = '<root>  
<a attrA="1">111</a>  
<a></a>  
<a></a>  
</root>'  
SELECT @x.query('/root/a[@attrA]')  
```  
  
 Dies ist das Ergebnis:  
  
```  
<a attrA="1">111</a>  
```  
  
 Weitere Informationen zum Angeben von Prädikaten in einem Pfad Ausdruck finden Sie unter [Angeben von Prädikaten in einem Pfad Ausdrucks Schritt](../xquery/path-expressions-specifying-predicates.md).  
  
 Das folgende Beispiel erstellt einen Sequenzausdruck aus Teilbäumen und wendet dann einen Filter auf die Sequenz an.  
  
```  
declare @x xml  
set @x = '  
<a>  
  <c>C under a</c>  
</a>  
<b>    
   <c>C under b</c>  
</b>  
<c>top level c</c>  
<d></d>  
'  
```  
  
 Der Ausdruck in `(/a, /b)` konstruiert eine Sequenz mit den Teilbäumen `/a` und `/b`, und aus der resultierenden Sequenz filtert der Ausdruck das Element `<c>`.  
  
```  
SELECT @x.query('  
  (/a, /b)/c  
')  
```  
  
 Dies ist das Ergebnis:  
  
```  
<c>C under a</c>  
<c>C under b</c>  
```  
  
 Das folgende Beispiel wendet einen Prädikatfilter an. Der Ausdruck findet Elemente <`a`> und <`b`>, die Element <`c`> enthalten.  
  
```  
declare @x xml  
set @x = '  
<a>  
  <c>C under a</c>  
</a>  
<b>    
   <c>C under b</c>  
</b>  
  
<c>top level c</c>  
<d></d>  
'  
SELECT @x.query('  
  (/a, /b)[c]  
')  
```  
  
 Dies ist das Ergebnis:  
  
```  
<a>  
  <c>C under a</c>  
</a>  
<b>  
  <c>C under b</c>  
</b>  
```  
  
### <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Der XQuery-range-Ausdruck wird nicht unterstützt.  
  
-   Die Sequenzen müssen homogen sein. Das heißt, dass alle Items in einer Sequenz entweder Knoten oder atomare Werte sein müssen. Dies wird statisch geprüft.  
  
-   Das Kombinieren von Sequenzen mit dem union-, intersect- oder except-Operator wird nicht unterstützt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Ausdrücke](../xquery/xquery-expressions.md)  
  
  
