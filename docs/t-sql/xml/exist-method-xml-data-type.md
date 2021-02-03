---
description: exist()-Methode (XML-Datentyp)
title: exist()-Methode (XML-Datentyp)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- exist() method
- exist method
ms.assetid: a55b75e0-0a17-4787-a525-9b095410f7af
author: MightyPen
ms.author: genemi
ms.openlocfilehash: b29a0143b2fca6bbcafaa94acd60e096a121480d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206883"
---
# <a name="exist-method-xml-data-type"></a>exist()-Methode (XML-Datentyp)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt ein **Bit** mit folgenden Bedeutungen zurück:  
  
-   1 bedeutet True, wenn der XQuery-Ausdruck in einer Abfrage ein nicht leeres Ergebnis zurückgibt, d. h. wenn er mindestens einen XML-Knoten zurückgibt.  
  
-   0 bedeutet False, wenn er ein leeres Ergebnis zurückgibt.  
  
-   NULL, wenn die Instanz vom Datentyp **xml** , für die die Abfrage ausgeführt wurde, NULL enthält.  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
exist (XQuery)   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 XQuery  
 Ein XQuery-Ausdruck, ein Zeichenfolgenliteral.  
  
## <a name="remarks"></a>Bemerkungen  
  
> [!NOTE]  
>  Die **exist()** -Methode gibt 1 für den XQuery-Ausdruck zurück, der ein nicht leeres Ergebnis zurückgibt. Wenn Sie die **true()** - oder **false()** -Funktionen in der **exist()** -Methode angeben, gibt die **exist()** -Methode 1 zurück, da die Funktionen **true()** und **false()** den booleschen Wert True bzw. False zurückgeben. D. h., sie geben ein nicht leeres Ergebnis zurück. Daher gibt **exist()** den Wert 1 (True) zurück, wie im folgenden Beispiel gezeigt:  
  
```sql
DECLARE @x XML;  
SET @x='';  
SELECT @x.exist('true()');   
```  
  
## <a name="examples"></a>Beispiele  
 In den folgenden Beispielen wird veranschaulicht, wie die **exist()** -Methode angegeben wird.  
  
### <a name="example-specifying-the-exist-method-against-an-xml-type-variable"></a>Beispiel: Angeben der exist()-Methode für eine Variable vom Typ xml  
 Im folgenden Beispiel stellt @x eine Variable vom Typ **XML** dar (nicht typisiertes XML) und @f eine Variable vom Typ „Integer“, die den von der **exist()** -Methode zurückgegebenen Wert speichert. Die **exist()** -Methode gibt True zurück (1), wenn der in der XML-Instanz gespeicherte Datumswert `2002-01-01`ist.  
  
```sql  
DECLARE @x XML;  
DECLARE @f BIT;  
SET @x = '<root Somedate = "2002-01-01Z"/>';  
SET @f = @x.exist('/root[(@Somedate cast as xs:date?) eq xs:date("2002-01-01Z")]');  
SELECT @f;  
```  
  
 Beachten Sie beim Vergleichen von Datumswerten mit der **exist()** -Methode Folgendes:  
  
-   Zum Vergleichen der Werte wird der Code `cast as xs:date?` verwendet, um die Werte in Werte vom Typ **xs:date** umzuwandeln.  
  
-   Der Wert des **\@Somedate**-Attributs ist nicht typisiert. Beim Vergleichen wird der Wert implizit in den Typ auf der rechten Seite des Vergleichs, den **xs:date** -Datentyp, umgewandelt.  
  
-   Anstelle von **cast as xs:date()** können Sie die **xs:date()** -Konstruktorfunktion verwenden. Weitere Informationen finden Sie unter [Konstruktorfunktionen &#40;XQuery&#41;](../../xquery/constructor-functions-xquery.md).  
  
 Das folgende Beispiel ähnelt dem vorherigen Beispiel, verwendet aber ein <`Somedate`>-Element.  
  
```sql
DECLARE @x XML;  
DECLARE @f BIT;  
SET @x = '<Somedate>2002-01-01Z</Somedate>';  
SET @f = @x.exist('/Somedate[(text()[1] cast as xs:date ?) = xs:date("2002-01-01Z") ]')  
SELECT @f;  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Die **text()** -Methode gibt einen Textknoten zurück, der den nicht typisierten Wert `2002-01-01`enthält (Der XQuery-Typ lautet **xdt:untypedAtomic**.) Sie müssen diesen Typ explizit von **x** in **xsd:date** umwandeln, da das implizite Umwandeln in diesem Fall nicht unterstützt wird.  
  
### <a name="example-specifying-the-exist-method-against-a-typed-xml-variable"></a>Beispiel: Angeben der exist()-Methode für eine typisierte xml-Variable  
 Das folgende Beispiel veranschaulicht die Verwendung der **exist()** -Methode für eine Variable vom Typ **xml** . Es handelt sich hier um eine typisierte XML-Variable, da sie den Namen des Namespace der Schemaauflistung angibt: `ManuInstructionsSchemaCollection`.  
  
 Im Beispiel wird dieser Variablen zunächst ein Dokument mit Produktionsanweisungen zugewiesen und dann die **exist()** -Methode aufgerufen, um zu ermitteln, ob das Dokument ein <`Location`>-Element enthält, dessen Attributwert für die **LocationID** 50 beträgt.  
  
 Die für die @x-Variable angegebene **exist()** -Methode gibt 1 (TRUE) zurück, wenn das Dokument mit den Produktionsanweisungen ein <`Location`>-Element mit `LocationID=50` enthält. Anderenfalls gibt sie 0 (False) zurück.  
  
```sql
DECLARE @x XML (Production.ManuInstructionsSchemaCollection);  
SELECT @x=Instructions  
FROM Production.ProductModel  
WHERE ProductModelID=67;  
--SELECT @x  
DECLARE @f INT;  
SET @f = @x.exist(' declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
    /AWMI:root/AWMI:Location[@LocationID=50]  
');  
SELECT @f;  
```  
  
### <a name="example-specifying-the-exist-method-against-an-xml-type-column"></a>Beispiel: Angeben der exist()-Methode für eine Spalte vom Typ xml  
 Die folgende Abfrage ruft Produktmodell-IDs ab, deren Katalogbeschreibungen nicht das Spezifikationselement <`Specifications`> umfassen:  
  
```sql
SELECT ProductModelID, CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
    <Product   
        ProductModelID= "{ sql:column("ProductModelID") }"   
        />  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
    declare namespace  pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     /pd:ProductDescription[not(pd:Specifications)]'  
    ) = 1;  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Die WHERE-Klausel wählt nur diejenigen Zeilen aus der **ProductDescription** -Tabelle aus, die die für die **CatalogDescription** -Spalte vom Typ XML angegebene Bedingung erfüllen.  
  
-   Die **exist()** -Methode in der WHERE-Klausel gibt 1 (TRUE) zurück, wenn das XML kein <`Specifications`>-Element enthält. Beachten Sie die Verwendung der [not()-Funktion (XQuery)](../../xquery/functions-on-boolean-values-not-function.md).  
  
-   Die [sql:column()-Funktion (XQuery)](../../xquery/xquery-extension-functions-sql-column.md) wird zum Einbinden eines Werts aus einer Spalte verwendet, die nicht vom Typ XML ist.  
  
-   Diese Abfrage gibt ein leeres Rowset zurück.  
  
 Die Abfrage gibt die **query()** - und die **exist()** -Methode vom XML-Datentyp an. Beide Methoden deklarieren denselben Namespace im Abfrageprolog. In diesem Fall bietet es sich an, WITH XMLNAMESPACES zu verwenden, um das Präfix zu deklarieren und in der Abfrage zu verwenden.  
  
```sql
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
    <Product   
        ProductModelID= "{ sql:column("ProductModelID") }"   
        />  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
     /pd:ProductDescription[not(pd:Specifications)]'  
    ) = 1;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Hinzufügen von Namespaces zu Abfragen mit WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Vergleichen von typisiertem XML mit nicht typisiertem XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Erstellen von Instanzen der XML-Daten](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [XML-Datentypmethoden](../../t-sql/xml/xml-data-type-methods.md)   
 [XML DML &#40;Data Modification Language&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
