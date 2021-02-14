---
title: Teil Zeichenfolge-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die XQuery-Funktion SUBSTRING (), die den angegebenen Teil einer Quell Zeichenfolge zurückgibt.
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
- substring function [XQuery]
- fn:substring function
ms.assetid: 2b3b8651-de51-46dc-af82-c86c45eac871
author: rothja
ms.author: jroth
ms.openlocfilehash: 7457341a5489485973869e46c2ba721b86352217
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345008"
---
# <a name="functions-on-string-values---substring"></a>Funktionen für Zeichenfolgenwerte – substring
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Gibt einen Teil des Werts *$sourceString* zurück, beginnend an der durch den Wert von $startingLoc gekennzeichneten Position und setzt die Anzahl der Zeichen fort, die durch den Wert von *$length* angegeben *werden* .  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:substring($sourceString as xs:string?,  
                          $startingLoc as xs:decimal?) as xs:string?  
  
fn:substring($sourceString as xs:string?,  
                          $startingLoc as xs:decimal?,  
                          $length as xs:decimal?) as xs:string?  
```  
  
## <a name="arguments"></a>Argumente  
 *$sourceString*  
 Quellzeichenfolge.  
  
 *$startingLoc*  
 Ausgangspunkt in der Quellzeichenfolge, an dem die Unterzeichenfolge beginnt. Wenn dieser Wert negativ oder 0 ist, werden nur die Zeichen an Positionen größer null zurückgegeben. Wenn Sie größer als die Länge des *$sourceString* ist, wird die Zeichenfolge der Länge 0 (null) zurückgegeben.  
  
 *$length*  
 [optional] Anzahl der abzurufenden Zeichen. Wenn kein Wert angegeben wird, werden alle Zeichen von der in *$startingLoc* bis zum Ende der Zeichenfolge angegebenen Position zurückgegeben.  
  
## <a name="remarks"></a>Bemerkungen  
 Die Version der Funktion mit drei Argumenten gibt die Zeichen in `$sourceString` zurück, deren Position `$p` genügt:  
  
 `fn:round($startingLoc) <= $p < fn:round($startingLoc) + fn:round($length)`  
  
 Der Wert von *$length* kann größer als die Anzahl der Zeichen im Wert *$sourceString* nach der Startposition sein. In diesem Fall gibt die Teil Zeichenfolge die Zeichen bis zum Ende der *$sourceString* zurück.  
  
 Das erste Zeichen einer Zeichenfolge befindet sich an Position 1.  
  
 Wenn der Wert von *$sourceString* die leere Sequenz ist, wird er als Zeichenfolge der Länge 0 (null) behandelt. Andernfalls wird die leere Sequenz zurückgegeben, wenn entweder *$startingLoc* oder *$length* die leere Sequenz ist.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Ergänzende Zeichen (Ersatzpaare)  
 Das Verhalten von Ersatzzeichenpaaren in XQuery-Funktionen hängt vom Kompatibilitätsgrad der Datenbank ab und in einigen Fällen vom Standardnamespace-URI für Funktionen. Weitere Informationen finden Sie im Abschnitt "XQuery-Funktionen sind Ersatz Zeichen Unterstützung" im Thema " [Breaking Changes to Datenbank-Engine Features in SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Siehe auch [ALTER DATABASE-Kompatibilitäts Grad &#40;Transact-SQL-&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) und [Sortierung und Unicode-Unterstützung](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 SQL Server erfordert, dass die *Parameter* *$startingLoc* und $length vom Typ xs: Decimal anstelle von xs: Double sind.  
  
 SQL Server ermöglicht, dass *$startingLoc* und *$length* die leere Sequenz sind, weil die leere Sequenz ein möglicher Wert ist, weil dynamische Fehler () zugeordnet werden.  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der-Datenbank gespeichert sind [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] .  
  
### <a name="a-using-the-substring-xquery-function-to-retrieve-partial-summary-product-model-descriptions"></a>A. Verwenden der substring()-Funktion von XQuery zum Abrufen von Teilzusammenfassungsbeschreibungen der Produktmodelle  
 Die Abfrage ruft die ersten 50 Zeichen des Texts ab, der das Produktmodell beschreibt, das <`Summary`> Element im Dokument.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
    <Prod>{ substring(string((/pd:ProductDescription/pd:Summary)[1]), 1, 50) }</Prod>  
 ') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('/pd:ProductDescription')  = 1;  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Die **String ()** -Funktion gibt den Zeichen folgen Wert des<`Summary`>-Elements zurück. Diese Funktion wird verwendet, da das <`Summary`>-Element sowohl den Text als auch unter Elemente (HTML-Formatierungs Elemente) enthält, und da Sie diese Elemente überspringen und den gesamten Text abrufen.  
  
-   Die **SUBSTRING ()** -Funktion Ruft die ersten 50 Zeichen aus dem von der **Zeichenfolge ()** abgerufenen Zeichen folgen Wert ab.  
  
 Dies ist ein Teilergebnis:  
  
```  
ProductModelID Result  
-------------- ----------------------------------------------------  
19      <Prod>Our top-of-the-line competition mountain bike.</Prod>   
23      <Prod>Suitable for any type of riding, on or off-roa</Prod>  
...  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
