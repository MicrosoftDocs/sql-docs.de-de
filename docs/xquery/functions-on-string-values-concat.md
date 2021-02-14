---
title: Concat-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die XQuery-Funktion Concat (), die eine Zeichenfolge zurückgibt, die durch Verkettung von NULL oder mehr Zeichen folgen, die als Argumente angegeben
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
- fn:concat function
- concat function [XQuery]
ms.assetid: d50afd20-a297-445e-be9e-13b48017e7ca
author: rothja
ms.author: jroth
ms.openlocfilehash: 3ad92315e20ae314fb9a22ddea1241f379ecc90f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341922"
---
# <a name="functions-on-string-values---concat"></a>Funktionen für Zeichenfolgenwerte – concat
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Nimmt null oder mehr Zeichenfolgen als Argumente an und gibt eine Zeichenfolge zurück, die durch Verketten der Werte der einzelnen Argumente erstellt wird.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:concat ($string as xs:string?  
           ,$string as xs:string?  
           [, ...]) as xs:string  
```  
  
## <a name="arguments"></a>Argumente  
 *$string*  
 Optionale zu verkettende Zeichenfolge.  
  
## <a name="remarks"></a>Bemerkungen  
 Die Funktion erfordert mindestens zwei Argumente. Wenn ein Argument eine leere Sequenz ist, wird diese als eine Zeichenfolge mit der Länge Null behandelt.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Ergänzende Zeichen (Ersatzpaare)  
 Das Verhalten von Ersatzzeichenpaaren in XQuery-Funktionen hängt vom Kompatibilitätsgrad der Datenbank ab und in einigen Fällen vom Standardnamespace-URI für Funktionen. Weitere Informationen finden Sie im Abschnitt "XQuery-Funktionen sind Ersatz Zeichen Unterstützung" im Thema " [Breaking Changes to Datenbank-Engine Features in SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). Siehe auch [ALTER DATABASE-Kompatibilitäts Grad &#40;Transact-SQL-&#41;](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) und [Sortierung und Unicode-Unterstützung](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der AdventureWorks-Beispieldatenbank gespeichert sind.  
  
### <a name="a-using-the-concat-xquery-function-to-concatenate-strings"></a>A. Verwenden der concat()-Funktion von XQuery zum Verketten von Zeichenfolgen  
 Diese Abfrage gibt für ein bestimmtes Produktmodell eine Zeichenfolge zurück, die durch Verketten des Garantiezeitraumes und der Garantiebeschreibung erstellt wird. Im Katalog Beschreibungs Dokument besteht das <`Warranty`>-Element aus <`WarrantyPeriod`> und <> untergeordneten `Description` Elementen.  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
SELECT CatalogDescription.query('  
    <Product   
        ProductModelID= "{ (/pd:ProductDescription/@ProductModelID)[1] }"  
        ProductModelName = "{ sql:column("PD.Name") }" >  
        {   
          concat( string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:WarrantyPeriod)[1]), "-",  
                  string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:Description)[1]))   
         }   
     </Product>  
 ') as Result  
FROM Production.ProductModel PD  
WHERE  PD.ProductModelID=28  
  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   In der SELECT-Klausel ist CatalogDescription eine Spalte vom Typ **XML** . Daher wird die [Query ()-Methode (XML-Datentyp)](../t-sql/xml/query-method-xml-data-type.md), instructions. Query (), verwendet. Die XQuery-Anweisung wird als Argument der query-Methode angegeben.  
  
-   Das Dokument, für das die Abfrage ausgeführt wird, verwendet Namespaces. Daher wird das **Namespace** -Schlüsselwort verwendet, um das Präfix für den Namespace zu definieren. Weitere Informationen finden Sie unter [XQuery-Prolog](../xquery/modules-and-prologs-xquery-prolog.md).  
  
 Dies ist das Ergebnis:  
  
```  
<Product ProductModelID="28" ProductModelName="Road-450">1 year-parts and labor</Product>  
```  
  
 Die vorherige Abfrage ruft Informationen für ein bestimmtes Produkt ab. Die folgende Abfrage ruft die gleichen Informationen für alle Produkte ab, für die XML-Katalogbeschreibungen gespeichert sind. Die **exist ()** -Methode des **XML** -Datentyps in der WHERE-Klausel gibt true zurück, wenn das XML-Dokument in den Zeilen über ein <`ProductDescription`>-Element verfügt.  
  
```  
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS wm)  
  
SELECT CatalogDescription.query('  
    <Product   
        ProductModelID= "{ (/pd:ProductDescription/@ProductModelID)[1] }"   
        ProductName = "{ sql:column("PD.Name") }" >  
        {   
          concat( string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:WarrantyPeriod)[1]), "-",  
                  string((/pd:ProductDescription/pd:Features/wm:Warranty/wm:Description)[1]))   
         }   
     </Product>  
 ') as Result  
FROM Production.ProductModel PD  
WHERE CatalogDescription.exist('//pd:ProductDescription ') = 1  
  
```  
  
 Beachten Sie, dass der boolesche Wert, der von der **exist ()** -Methode des **XML** -Typs zurückgegeben wird, mit 1 verglichen wird.  
  
### <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Die **Concat ()** -Funktion in SQL Server akzeptiert nur Werte vom Typ xs: String. Andere Werte müssen explizit in xs:string oder xdt:untypedAtomic umgewandelt werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
