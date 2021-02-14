---
title: String-Funktion (XQuery) | Microsoft-Dokumentation
description: Informationen zur XQuery-Funktions Zeichenfolge (), die den Wert des Arguments zurückgibt, das als Zeichenfolge dargestellt wird.
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
- string function
- fn:string function
ms.assetid: 7baa2959-9340-429b-ad53-3df03d8e13fc
author: rothja
ms.author: jroth
ms.openlocfilehash: 9c334f2f78ae79b9a1af3a268728d6fcc5454f05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351079"
---
# <a name="data-accessor-functions---string-xquery"></a>Data Accessor-Funktionen – string (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Gibt den Wert *$arg* zurück, der als Zeichenfolge dargestellt wird.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:string() as xs:string  
fn:string($arg as item()?) as xs:string  
```  
  
## <a name="arguments"></a>Argumente  
 *$arg*  
 Ist ein Knoten oder ein atomarer Wert.  
  
## <a name="remarks"></a>Bemerkungen  
  
-   Wenn *$arg* die leere Sequenz ist, wird die Zeichenfolge der Länge 0 (null) zurückgegeben.  
  
-   Wenn *$arg* ein Knoten ist, gibt die Funktion den Zeichen folgen Wert des Knotens zurück, der mit dem String-value-Accessor abgerufen wird. Dies wurde in der W3C XQuery 1.0- und XPath 2.0-Datenmodellspezifikation definiert.  
  
-   Wenn *$arg* ein atomarer Wert ist, gibt die Funktion die gleiche Zeichenfolge zurück, die von der Umwandlung des Ausdrucks in **xs: String**, *$arg* zurückgegeben wird, sofern nicht anders angegeben.  
  
-   Wenn der Typ der *$arg* **xs: anyURI** ist, wird der URI in eine Zeichenfolge konvertiert, ohne Sonderzeichen zu vermeiden.  
  
-   In dieser Implementierung kann **FN: String ()** ohne Argument nur im Kontext eines kontextabhängigen Prädikats verwendet werden. Insbesondere kann die Funktion nur innerhalb von Klammern ([ ]) verwendet werden.  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der AdventureWorks-Datenbank gespeichert sind.  
  
### <a name="a-using-the-string-function"></a>A. Verwenden der Zeichenfolgenfunktion  
 Die folgende Abfrage ruft den <> untergeordneten `Features` Elementknoten des <`ProductDescription`> Elements ab.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
 /PD:ProductDescription/PD:Features  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Dies ist das Teilergebnis:  
  
```  
<PD:Features xmlns:PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
   These are the product highlights.   
   <p1:Warranty xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p1:WarrantyPeriod>3 years</p1:WarrantyPeriod>  
    <p1:Description>parts and labor</p1:Description>  
   </p1:Warranty>  
       ...  
</PD:Features>  
```  
  
 Wenn Sie die **String ()** -Funktion angeben, erhalten Sie den Zeichen folgen Wert des angegebenen Knotens.  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
 string(/PD:ProductDescription[1]/PD:Features[1])  
')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Dies ist das Teilergebnis.  
  
```  
These are the product highlights.   
3 yearsparts and labor...    
```  
  
### <a name="b-using-the-string-function-on-various-nodes"></a>B. Verwenden der Zeichenfolgenfunktion für verschiedene Knoten  
 Im folgenden Beispiel wird eine XML-Instanz einer Variablen des Typs xml zugewiesen. Abfragen werden angegeben, um das Ergebnis der Anwendung von **String ()** auf verschiedene Knoten zu veranschaulichen.  
  
```  
declare @x xml  
set @x = '<?xml version="1.0" encoding="UTF-8" ?>  
<!--  This is a comment -->  
<root>  
  <a>10</a>  
just text  
  <b attr="x">20</b>  
</root>  
'  
```  
  
 Die folgende Abfrage ruft den Zeichenfolgenwert des Dokumentknotens ab. Dieser Wert wird generiert, indem die Zeichenfolgenwerte aller seiner Nachfolgertextknoten verkettet werden.  
  
```  
select @x.query('string(/)')  
```  
  
 Dies ist das Ergebnis:  
  
```  
This is a comment 10  
just text  
 20  
```  
  
 Die folgende Abfrage versucht, den Zeichenfolgenwert eines Verarbeitungsanweisungsknotens abzurufen. Das Ergebnis ist eine leere Sequenz, weil kein Textknoten enthalten ist.  
  
```  
select @x.query('string(/processing-instruction()[1])')  
```  
  
 Die folgende Abfrage ruft den Zeichenfolgenwert des Kommentarknotens ab und gibt den Textknoten zurück.  
  
```  
select @x.query('string(/comment()[1])')  
```  
  
 Dies ist das Ergebnis:  
  
```  
This is a comment   
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
