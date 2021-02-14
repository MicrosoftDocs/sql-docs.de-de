---
title: enthält Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe der enthält-Funktion in einer XQuery ermitteln, ob ein angegebener Zeichen folgen Wert den angegebenen Teil Zeichenfolgen-Wert enthält
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- contains function (XQuery)
- fn:contains function
ms.assetid: 2c88c015-04fc-429b-84b2-835596a28b65
author: rothja
ms.author: jroth
ms.openlocfilehash: 18f57d57e1a944a1be66a880231533e6cf34493e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344975"
---
# <a name="functions-on-string-values---contains"></a>Funktionen für Zeichenfolgenwerte – contains
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Gibt einen Wert vom Typ xs: Boolean zurück, der angibt, ob der Wert von *$arg 1* einen Zeichen folgen Wert enthält, der durch *$Arg 2* angegeben wird.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:contains ($arg1 as xs:string?, $arg2 as xs:string?) as xs:boolean?  
```  
  
## <a name="arguments"></a>Argumente  
 *$arg 1*  
 Zu testender Zeichenfolgenwert.  
  
 *$Arg 2*  
 Abzurufende Unterzeichenfolge.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn der Wert von *$Arg 2* eine Zeichenfolge der Länge 0 (null) ist, gibt die Funktion **true** zurück. Wenn der Wert von *$arg 1* eine Zeichenfolge der Länge 0 (null) ist und der Wert von *$Arg 2* keine Zeichenfolge der Länge 0 (null) ist, gibt die Funktion **false** zurück.  
  
 Wenn der Wert *$arg 1* oder *$Arg 2* die leere Sequenz ist, wird das Argument als Zeichenfolge der Länge 0 (null) behandelt.  
  
 Die contains()-Funktion verwendet die Unicode-Codepunkt-Standardsortierung von XQuery für den Zeichenfolgenvergleich.  
  
 Der für *$Arg 2* angegebene Teil Zeichenfolgen-Wert muss kleiner oder gleich 4000 Zeichen sein. Wenn der angegebene Wert größer als 4000 Zeichen ist, tritt eine dynamische Fehlerbedingung auf, und die enthält ()-Funktion gibt eine leere Sequenz anstelle eines booleschen Werts von **true** oder **false** zurück. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] löst keine dynamischen Fehler bei XQuery-Ausdrücken aus.  
  
 Um Vergleiche ohne Berücksichtigung der Groß-/Kleinschreibung zu erhalten, können die groß [-](../xquery/functions-on-string-values-upper-case.md) oder Kleinbuchstaben verwendet werden.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Ergänzende Zeichen (Ersatzpaare)  
 Das Verhalten von Ersatzzeichenpaaren in XQuery-Funktionen hängt vom Kompatibilitätsgrad der Datenbank ab und in einigen Fällen vom Standardnamespace-URI für Funktionen. Weitere Informationen finden Sie im Abschnitt "XQuery-Funktionen sind Ersatz Zeichen Unterstützung" im Thema " [Breaking Changes to Datenbank-Engine Features in SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Siehe auch [ALTER DATABASE-Kompatibilitäts Grad &#40;Transact-SQL-&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) und [Sortierung und Unicode-Unterstützung](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ XML in der AdventureWorks-Datenbank gespeichert sind.  
  
### <a name="a-using-the-contains-xquery-function-to-search-for-a-specific-character-string"></a>A. Verwenden der contains()-Funktion von XQuery zum Suchen nach einer bestimmten Zeichenfolge  
 Die folgende Abfrage sucht nach Produkten, die das Wort Aerodynamic in den Zusammenfassungsbeschreibungen enthalten. Die Abfrage gibt die ProductID und das <`Summary`>-Element für solche Produkte zurück.  
  
```  
--The product model description document uses  
--namespaces. The WHERE clause uses the exit()  
--method of the xml data type. Inside the exit method,  
--the XQuery contains()function is used to  
--determine whether the <Summary> text contains the word  
--Aerodynamic.   
  
USE AdventureWorks  
GO  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
      <Prod>  
         { /pd:ProductDescription/@ProductModelID }  
         { /pd:ProductDescription/pd:Summary }  
      </Prod>  
 ') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('  
   /pd:ProductDescription/pd:Summary//text()  
    [contains(., "Aerodynamic")]') = 1  
```  
  
 Ergebnisse  
  
 `ProductModelID Result`  
  
 `-------------- ---------`  
  
 `28     <Prod ProductModelID="28">`  
  
 `<pd:Summary xmlns:pd=`  
  
 `"https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">`  
  
 `<p1:p xmlns:p1="http://www.w3.org/1999/xhtml">`  
  
 `A TRUE multi-sport bike that offers streamlined riding and`  
  
 `a revolutionary design. Aerodynamic design lets you ride with`  
  
 `the pros, and the gearing will conquer hilly roads.</p1:p>`  
  
 `</pd:Summary>`  
  
 `</Prod>`  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
