---
title: Data-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die XQuery-Funktionsdaten () verwenden, um den typisierten Wert für jedes Element in einer angegebenen Sequenz von Elementen zurückzugeben.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:data function
- data function [XQuery]
ms.assetid: 511b5d7d-c679-4cb2-a3dd-170cc126f49d
author: rothja
ms.author: jroth
ms.openlocfilehash: 4e20c00d93082994cd5dc230bfe1b4356b3ed031
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338506"
---
# <a name="data-accessor-functions---data-xquery"></a>Data Accessor-Funktionen – data (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Gibt den typisierten Wert für jedes Element zurück, das durch *$arg* angegeben wird.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:data ($arg as item()*) as xdt:untypedAtomic*  
```  
  
## <a name="arguments"></a>Argumente  
 *$arg*  
 Sequenz der Items, deren typisierte Werte zurückgegeben werden.  
  
## <a name="remarks"></a>Bemerkungen  
 Für typisierte Werte gilt Folgendes:  
  
-   Der typisierte Wert eines atomaren Werts ist der atomare Wert.  
  
-   Der typisierte Wert eines Textknotens ist der Zeichenfolgenwert des Textknotens.  
  
-   Der typisierte Wert eines Kommentars ist der Zeichenfolgenwert des Kommentars.  
  
-   Der typisierte Wert einer Verarbeitungsanweisung ist der Inhalt der Verarbeitungsanweisung, ohne den Namen des Verarbeitungsanweisungsziels.  
  
-   Der typisierte Wert eines Dokumentknotens ist dessen Zeichenfolgenwert.  
  
 Für Attribut- und Elementknoten gilt Folgendes:  
  
-   Wenn ein Attributknoten mit einem XML-Schematyp typisiert wird, ist dessen typisierter Wert der entsprechende typisierte Wert.  
  
-   Wenn der Attribut Knoten nicht typisiert ist, ist der typisierte Wert gleich seinem Zeichen folgen Wert, der als Instanz von **xdt: untypedAtomic** zurückgegeben wird.  
  
-   Wenn der Elementknoten nicht typisiert wurde, ist der typisierte Wert gleich seinem Zeichen folgen Wert, der als Instanz von **xdt: untypedAtomic** zurückgegeben wird.  
  
 Für typisierte Elementknoten gilt Folgendes:  
  
-   Wenn das-Element einen einfachen Inhaltstyp aufweist, gibt **Data ()** den typisierten Wert des-Elements zurück.  
  
-   Wenn der Knoten einen komplexen Typ hat, einschließlich xs: anyType, gibt **Data ()** einen statischen Fehler zurück.  
  
 Obwohl die Verwendung der **Data ()** -Funktion häufig optional ist, wie in den folgenden Beispielen gezeigt, erhöht die Angabe der **Data ()** -Funktion die Lesbarkeit der Abfrage explizit. Weitere Informationen finden Sie unter [Grundlagen zu XQuery](../xquery/xquery-basics.md).  
  
 Sie können keine **Daten ()** auf konstruiertem XML-Code angeben, wie im folgenden dargestellt:  
  
```  
declare @x xml  
set @x = ''  
select @x.query('data(<SomeNode>value</SomeNode>)')  
```  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der AdventureWorks-Datenbank gespeichert sind.  
  
### <a name="a-using-the-data-xquery-function-to-extract-typed-value-of-a-node"></a>A. Verwenden der XQuery-Funktion data() XQuery zum Extrahieren des typisierten Werts eines Knotens.  
 Die folgende Abfrage veranschaulicht, wie die **Data ()** -Funktion verwendet wird, um Werte eines Attributs, eines Elements und eines Text Knotens abzurufen:  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query(N'  
 for $pd in //p1:ProductDescription  
 return   
    <Root   
      ProductID = "{ data( ($pd//@ProductModelID)[1] ) }"   
      Feature =   "{ data( ($pd/p1:Features/wm:Warranty/wm:Description)[1] ) }" >  
    </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Dies ist das Ergebnis:  
  
```  
<Root ProductID="19" Feature="parts and labor"/>  
```  
  
 Wie bereits erwähnt, ist die **Data ()** -Funktion beim Konstruieren von Attributen optional. Wenn Sie die **Data ()** -Funktion nicht angeben, wird Sie implizit angenommen. Die folgende Abfrage führt zu denselben Ergebnissen wie die vorherige Abfrage:  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
      for $pd in //p1:ProductDescription  
         return   
          <Root    
                ProductID = "{ ($pd/@ProductModelID)[1] }"    
                Feature =   "{ ($pd/p1:Features/wm:Warranty/wm:Description)[1] }" >  
           </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 In den folgenden Beispielen werden-Instanzen veranschaulicht, in denen die **Data ()** -Funktion erforderlich ist.  
  
 In der folgenden Abfrage gibt **$pd/p1: Spezifikationen/Material** das <`Material`>-Element zurück. Außerdem gibt **Data ($PD/P1: Spezifikationen/Material)** Zeichendaten zurück, die als xdt: untypedAtomic typisiert sind, da <`Material`> nicht typisiert ist. Wenn die Eingabe nicht typisiert ist, wird das Ergebnis der **Daten ()** als **xdt: untypedAtomic** typisiert.  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      for $pd in //p1:ProductDescription  
         return   
          <Root>  
             { $pd/p1:Specifications/Material }  
             { data($pd/p1:Specifications/Material) }  
           </Root>  
 ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Dies ist das Ergebnis:  
  
```  
<Root>  
  <Material>Almuminum Alloy</Material>Almuminum Alloy  
</Root>  
```  
  
 In der folgenden Abfrage gibt **Data ($PD/P1: Features/WM: Garantie)** einen statischen Fehler zurück, da <`Warranty`> ein komplexes Typelement ist.  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS p1,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
 <Root>  
   {     /p1:ProductDescription/p1:Features/wm:Warranty }  
   { data(/p1:ProductDescription/p1:Features/wm:Warranty) }  
 </Root>  
 ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID = 23  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
