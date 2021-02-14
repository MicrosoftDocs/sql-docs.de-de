---
title: Lower-case-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die XQuery-Funktion in Kleinbuchstaben (), die jedes Zeichen in einer angegebenen Zeichenfolge in den entsprechenden Kleinbuchstaben konvertiert.
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
- lower-case Function (XQuery)
- lower-case
ms.assetid: 5222c4ff-890c-4d57-8506-c065a5ebfd3e
author: rothja
ms.author: jroth
ms.openlocfilehash: b832484a1edd7177617cdd9ad6de05b15b603ba2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341896"
---
# <a name="functions-on-string-values---lower-case"></a>Funktionen für Zeichenfolgenwerte – lower-case
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Die Kleinbuchstaben-Funktion konvertiert jedes Zeichen in *$arg* in den entsprechenden Kleinbuchstaben. Die binäre Konvertierung der Groß-/Kleinschreibung für Unicode-Codepunkte von Microsoft Windows gibt an, wie Zeichen in Kleinbuchstaben konvertiert werden. Dieser Standard unterscheidet sich vom Unicode-Standard zum Zuordnen von Codepunkten.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:lower-case($arg as xs:string?) as xs:string  
```  
  
## <a name="arguments"></a>Argumente  
  
|Begriff|Definition|  
|-|-|
|*$arg*|Der Zeichenfolgenwert, der in Kleinbuchstaben konvertiert werden soll.|  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn der Wert *$arg* leer ist, wird eine Zeichenfolge der Länge 0 (null) zurückgegeben.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-changing-a-string-to-upper-case"></a>A. Ändern einer Zeichenfolge in Großbuchstaben  
 Im folgenden Beispiel wird die Eingabe Zeichenfolge ' abcdef! ' geändert. @4 in Kleinbuchstaben.  
  
```  
DECLARE @x xml = N'abcDEF!@4';  
SELECT @x.value('fn:lower-case(/text()[1])', 'nvarchar(10)');  
```  
  
 [!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
 `abcdef!@4`  
  
### <a name="b-search-for-a-specific-character-string"></a>B. Suchen nach einer bestimmten Zeichenfolge  
 In diesem Beispiel wird gezeigt, wie die lower-case-Funktion in einer Suche verwendet werden kann, bei der nicht zwischen Groß- und Kleinschreibung unterschieden werden soll.  
  
```  
USE AdventureWorks  
GO  
--WITH XMLNAMESPACES clause specifies the namespace prefix  
--to use.   
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
--The XQuery contains() function is used to determine whether  
--any of the text nodes below the <Summary> element contain  
--the word 'frame'. The lower-case() function makes the   
--case insensitive.  
  
SELECT ProductModelID, CatalogDescription.query('  
      <Prod>  
         { /pd:ProductDescription/@ProductModelID }  
         { /pd:ProductDescription/pd:Summary }  
      </Prod>  
 ') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('  
/pd:ProductDescription/pd:Summary//text()[  
          contains(lower-case(.), "FRAME")]')  = 1  
```  
  
 [!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
 `ProductModelID Result`  
  
 `-------------- ---------`  
  
 `19     <Prod ProductModelID="19">`  
  
 `<pd:Summary xmlns:pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">`  
  
 `<p1:p xmlns:p1="http://www.w3.org/1999/xhtml">Our top-of-the-line competition mountain bike.`  
  
 `Performance-enhancing options include the innovative HL Frame,`  
  
 `super-smooth front suspension, and traction for all terrain.`  
  
 `</p1:p>`  
  
 `</pd:Summary>`  
  
 `</Prod>`  
  
 `25     <Prod ProductModelID="25">`  
  
 `<pd:Summary xmlns:pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">`  
  
 `<p1:p xmlns:p1="http://www.w3.org/1999/xhtml">This bike is ridden by race winners. Developed with the`  
  
 `Adventure Works Cycles professional race team, it has a extremely light`  
  
 `heat-treated aluminum frame, and steering that allows precision control.`  
  
 `</p1:p>`  
  
 `</pd:Summary>`  
  
 `</Prod>`  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
