---
title: Vergleichsausdrücke (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie XQuery-Vergleichsausdrücke verwenden, die allgemeine, Wert-, Knoten-und Knoten Reihen folgen Vergleichs Operatoren enthalten.
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
- node comparison operators [XQuery]
- comparison expressions [XQuery]
- node order comparison operators [XQuery]
- expressions [XQuery], comparison
- comparison operators [XQuery]
- value comparison operators
ms.assetid: dc671348-306f-48ef-9e6e-81fc3c7260a6
author: rothja
ms.author: jroth
ms.openlocfilehash: 4824cd52001305a05c00e197e8c4140598267d4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349345"
---
# <a name="comparison-expressions-xquery"></a>Vergleichsausdrücke (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  XQuery stellt die folgenden Typen von Vergleichsoperatoren bereit:  
  
-   Allgemeine Vergleichsoperatoren  
  
-   Wert Vergleichs Operatoren  
  
-   Knotenvergleichsoperatoren  
  
-   Knoten Reihen folgen Vergleichs Operatoren  
  
## <a name="general-comparison-operators"></a>Allgemeine Vergleichs Operatoren  
 Allgemeine Vergleichsoperatoren können zum Vergleichen von atomaren Werten, Sequenzen oder einer beliebigen Kombination daraus verwendet werden.  
  
 Die allgemeinen Operatoren werden in der folgenden Tabelle definiert.  
  
|Operator|Beschreibung|  
|--------------|-----------------|  
|=|Gleich|  
|!=|Ungleich|  
|\<|Kleiner als|  
|>|Größer als|  
|\<=|Kleiner als oder gleich|  
|>=|Größer als oder gleich|  
  
 Wenn Sie zwei Sequenzen mithilfe von Vergleichsoperatoren vergleichen, und in der zweiten Sequenz ist ein Wert vorhanden, der True mit einem Wert in der ersten Sequenz vergleicht, ist das Gesamtergebnis True. Anderenfalls ist es False. Beispiel: (1, 2, 3) = (3, 4) ist True, weil der Wert 3 in beiden Sequenzen auftritt.  
  
```  
declare @x xml  
set @x=''  
select @x.query('(1,2,3) = (3,4)')    
```  
  
 Der Vergleich erwartet, dass die Werte vergleichbare Typen sind. Dies wird statisch überprüft. Bei numerischen Vergleichen kann eine Höherstufung des numeric-Typs auftreten. Wenn ein decimal-Wert von 10 z. B. mit einem double-Wert 1e1 verglichen wird, wird der decimal-Wert in double geändert. Beachten Sie, dass dies zu ungenauen Ergebnissen führen kann, weil double-Vergleiche nicht genau sein können.  
  
 Wenn einer der Werte nicht typisiert ist, wird er in den Datentyp des anderen Werts umgewandelt. Im folgenden Beispiel wird der Wert 7 als ganzzahliger Wert behandelt. Vor dem Vergleich wird der nicht typisierte Wert von /a[1] in einen integer-Wert konvertiert. Der integer-Vergleich gibt True zurück.  
  
```  
declare @x xml  
set @x='<a>6</a>'  
select @x.query('/a[1] < 7')  
```  
  
 Wenn hingegen der nicht typisierte Wert mit einer Zeichenfolge oder einem anderen nicht typisierten Wert verglichen wird, wird er in xs:string umgewandelt. In der folgenden Abfrage wird der string-Wert 6 mit dem string-Wert "17" verglichen. Die folgende Abfrage gibt aufgrund des Zeichenfolgenvergleichs False zurück.  
  
```  
declare @x xml  
set @x='<a>6</a>'  
select @x.query('/a[1] < "17"')  
```  
  
 Die folgende Abfrage gibt kleinformatige Bilder eines Produktmodells aus dem Produktkatalog zurück, der in der AdventureWorks-Beispieldatenbank bereitgestellt wird. Die Abfrage vergleicht eine Sequenz von atomic-Werten, die von `PD:ProductDescription/PD:Picture/PD:Size` zurückgegeben wird, mit einer Singletonsequenz "small". Wenn der Vergleich true ist, wird das <Bildelement zurückgegeben \> .  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD)  
SELECT CatalogDescription.query('         
    for $P in /PD:ProductDescription/PD:Picture[PD:Size = "small"]         
    return $P') as Result         
FROM   Production.ProductModel         
WHERE  ProductModelID=19         
```  
  
 Mit der folgenden Abfrage wird eine Sequenz von Telefonnummern in <Number- \> Elementen mit dem Zeichenfolgenliterals "112-111-1111" verglichen. Die Abfrage vergleicht die Sequenz der Rufnummerelemente in der AdditionalContactInfo-Spalte, um zu bestimmen, ob eine bestimmte Rufnummer für einen bestimmten Kunden im Dokument vorhanden ist.  
  
```  
WITH XMLNAMESPACES (  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS act,  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS aci)  
  
SELECT AdditionalContactInfo.value('         
   /aci:AdditionalContactInfo//act:telephoneNumber/act:number = "112-111-1111"', 'nvarchar(10)') as Result         
FROM Person.Contact         
WHERE ContactID=1         
```  
  
 Diese Abfrage gibt True zurück. Dies zeigt an, dass die Rufnummer im Dokument vorhanden ist. Die folgende Abfrage ist eine geringfügig geänderte Version der vorherigen Abfrage. In dieser Abfrage werden die aus dem Dokument abgerufenen Rufnummerwerte mit einer Sequenz aus zwei Rufnummerwerten verglichen. Wenn der Vergleich true ist, wird das <Number- \> Element zurückgegeben.  
  
```  
WITH XMLNAMESPACES (  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS act,  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS aci)  
  
SELECT AdditionalContactInfo.query('         
  if (/aci:AdditionalContactInfo//act:telephoneNumber/act:number = ("222-222-2222","112-111-1111"))         
  then          
     /aci:AdditionalContactInfo//act:telephoneNumber/act:number         
  else         
    ()') as Result         
FROM Person.Contact         
WHERE ContactID=1  
  
```  
  
 Dies ist das Ergebnis:  
  
```  
\<act:number   
  xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
    111-111-1111  
\</act:number>  
\<act:number   
  xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
    112-111-1111  
\</act:number>   
```  
  
## <a name="value-comparison-operators"></a>Wertvergleichsoperatoren  
 Wertvergleichsoperatoren werden zum Vergleichen atomarer Werte verwendet. Beachten Sie, dass Sie allgemeine Vergleichsoperatoren anstelle von Wertvergleichsoperatoren in Ihren Abfragen verwenden können.  
  
 Die Wertvergleichsoperatoren werden in der folgenden Tabelle definiert.  
  
|Operator|BESCHREIBUNG|  
|--------------|-----------------|  
|eq|Gleich|  
|ne|Ungleich|  
|lt|Kleiner als|  
|gt|Größer als|  
|le|Kleiner als oder gleich|  
|ge|Größer als oder gleich|  
  
 Wenn die beiden Werte gemäß dem ausgewählten Operator das Gleiche vergleichen, gibt der Ausdruck True zurück. Anderenfalls wird False zurückgegeben. Wenn einer der Werte eine leere Sequenz ergibt, ist das Ergebnis des Ausdrucks False.  
  
 Diese Operatoren funktionieren nur für atomare Singletonwerte. Dies bedeutet, dass Sie eine Sequenz nicht als einen der Operanden angeben können.  
  
 Die folgende Abfrage ruft z. b. \<Picture> Elemente für ein Produktmodell ab, bei dem die Bildgröße "Small" ist:  
  
```  
SELECT CatalogDescription.query('         
              declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";         
              for $P in /PD:ProductDescription/PD:Picture[PD:Size eq "small"]         
              return         
                    $P         
             ') as Result         
FROM Production.ProductModel         
WHERE ProductModelID=19         
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   `declare namespace` definiert das Namespacepräfix, das anschließend in der Abfrage verwendet wird.  
  
-   Der \<Size> Elementwert wird mit dem angegebenen atomaren Wert "Small" verglichen.  
  
-   Beachten Sie, dass die **Data ()** -Funktion implizit zum Abrufen des Knoten Werts verwendet wird, da die Wert Operatoren nur für atomarische Werte verwendet werden. Das heißt, `data($P/PD:Size) eq "small"` generiert das gleiche Ergebnis.  
  
 Dies ist das Ergebnis:  
  
```  
\<PD:Picture   
  xmlns:PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
  \<PD:Angle>front\</PD:Angle>  
  \<PD:Size>small\</PD:Size>  
  \<PD:ProductPhotoID>31\</PD:ProductPhotoID>  
\</PD:Picture>  
```  
  
 Beachten Sie, dass die Typhöherstufungsregeln für Wertvergleiche mit den Regeln für allgemeine Vergleiche identisch sind. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] verwendet außerdem bei Wertvergleichen die gleichen Typumwandlungsregeln für nicht typisierte Werte wie bei allgemeinen Vergleichen. Im Gegensatz dazu wandeln die Regeln in der XQuery-Spezifikation bei Wertvergleichen immer den nicht typisierten Wert in xs:string um.  
  
## <a name="node-comparison-operator"></a>Knotenvergleichsoperator  
 Der Knoten Vergleichs Operator **ist**, gilt nur für Knoten Typen. Das zurückgegebene Ergebnis zeigt an, ob zwei Knoten, die als Operanden übergeben werden, den gleichen Knoten im Quelldokument darstellen. Dieser Operator gibt True zurück, wenn die beiden Operanden den gleichen Knoten bezeichnen. Andernfalls wird false zurückgegeben.  
  
 Die folgende Abfrage überprüft, ob Arbeitsplatzstandort 10 der erste Arbeitsplatzstandort im Fertigungsvorgang eines bestimmten Produktmodells ist.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions' AS AWMI)  
  
SELECT ProductModelID, Instructions.query('         
    if (  (//AWMI:root/AWMI:Location[@LocationID=10])[1]         
          is          
          (//AWMI:root/AWMI:Location[1])[1] )          
    then         
          <Result>equal</Result>         
    else         
          <Result>Not-equal</Result>         
         ') as Result         
FROM Production.ProductModel         
WHERE ProductModelID=7           
```  
  
 Dies ist das Ergebnis:  
  
```  
ProductModelID       Result          
-------------- --------------------------  
7              <Result>equal</Result>      
```  
  
## <a name="node-order-comparison-operators"></a>Knotenreihenfolge-Vergleichsoperatoren  
 Knotenreihenfolge-Vergleichsoperatoren vergleichen Knotenpaare basierend auf ihren Positionen in einem Dokument.  
  
 Diese Vergleiche werden auf der Grundlage der Dokumentreihenfolge erstellt:  
  
-   `<<` : Führt **Operand 1** vor **Operand 2** in der Dokument Reihenfolge aus.  
  
-   `>>` : Der **Operand 1** folgt **Operand 2** in der Dokument Reihenfolge.  
  
 Die folgende Abfrage gibt true zurück, wenn das-Element in der Produktkatalog Beschreibung \<Warranty> vor dem- \<Maintenance> Element in der Dokument Reihenfolge für ein bestimmtes Produkt angezeigt wird.  
  
```  
WITH XMLNAMESPACES (  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD,  
  'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS WM)  
  
SELECT CatalogDescription.value('  
     (/PD:ProductDescription/PD:Features/WM:Warranty)[1] <<   
           (/PD:ProductDescription/PD:Features/WM:Maintenance)[1]', 'nvarchar(10)') as Result  
FROM  Production.ProductModel  
where ProductModelID=19  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Die **value ()** -Methode des **XML**-Datentyps wird in der Abfrage verwendet.  
  
-   Das boolesche Ergebnis der Abfrage wird in **nvarchar (10)** konvertiert und zurückgegeben.  
  
-   Diese Abfrage gibt True zurück.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Geben Sie System &#40;XQuery ein&#41;](../xquery/type-system-xquery.md)   
 [XQuery-Ausdrücke](../xquery/xquery-expressions.md)  
  
  
