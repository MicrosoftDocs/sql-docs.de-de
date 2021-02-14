---
title: 'SQL: Variable ()-Funktion (XQuery) | Microsoft-Dokumentation'
description: 'Erfahren Sie, wie Sie die XQuery-Erweiterungs Funktion SQL: Variable () verwenden, um eine Variable verfügbar zu machen, die einen relationalen SQL-Wert in einem XQuery-Ausdruck enthält.'
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sql:variable() function
- sql:variable function
ms.assetid: 6e2e5063-c1cf-4b5a-b642-234921e3f4f7
author: rothja
ms.author: jroth
ms.openlocfilehash: d879088a98bfa7e826c53e4b90822bc65caff57a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350384"
---
# <a name="xquery-extension-functions---sqlvariable"></a>XQuery-Erweiterungsfunktionen – sql:variable()
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Macht eine Variable verfügbar, die einen relationalen SQL-Wert in einem XQuery-Ausdruck enthält.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sql:variable("variableName") as xdt:anyAtomicType?  
```  
  
## <a name="remarks"></a>Bemerkungen  
 Wie im Thema [binden relationaler Daten in XML](../t-sql/xml/binding-relational-data-inside-xml-data.md)beschrieben, können Sie diese Funktion verwenden, wenn Sie [XML-Datentyp Methoden](../t-sql/xml/xml-data-type-methods.md) zum verfügbar machen eines relationalen Werts in XQuery verwenden.  
  
 Beispielsweise wird die [Query ()-Methode](../t-sql/xml/query-method-xml-data-type.md) verwendet, um eine Abfrage für eine XML-Instanz anzugeben, die in einer Variablen oder Spalte des **XML** -Datentyps gespeichert ist. Manchmal sollen in einer Abfrage auch Werte aus einer [!INCLUDE[tsql](../includes/tsql-md.md)]-Variablen oder einem -Parameter verwendet werden, um relationale und XML-Daten zu verbinden. Zu diesem Zweck verwenden Sie die **SQL: Variable** -Funktion.  
  
 Der SQL-Wert wird einem entsprechenden XQuery-Wert zugeordnet, und sein Typ ist ein XQuery-Basistyp, der dem entsprechenden SQL-Typ entspricht.  
  
 Sie können nur im Kontext des Quell Ausdrucks einer XML-DML-INSERT-Anweisung auf eine **XML** -Instanz verweisen. Andernfalls können Sie nicht auf Werte vom Typ **XML** oder einen Common Language Runtime (CLR)-benutzerdefinierten Typ verweisen.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-the-sqlvariable-function-to-bring-a-transact-sql-variable-value-into-xml"></a>A. Einfügen einer Transact-SQL-Variablen in XML mit der sql:variable()-Funktion  
 Im folgenden Beispiel wird eine XML-Instanz erstellt, die aus Folgendem besteht:  
  
-   Einem Wert (`ProductID`) aus einer Nicht-XML-Spalte. Die [SQL: column ()-Funktion](../xquery/xquery-extension-functions-sql-column.md) wird verwendet, um diesen Wert in der XML-Datei zu binden.  
  
-   Einem Wert (`ListPrice`) aus einer Nicht-XML-Spalte aus einer anderen Tabelle. Die `sql:column()`-Funktion wird auch hier zum Binden dieses Werts im XML-Code verwendet.  
  
-   Einem Wert (`DiscountPrice`) aus einer [!INCLUDE[tsql](../includes/tsql-md.md)]-Variablen. Zum Binden dieses Werts im XML-Code wird die `sql:variable()`-Methode verwendet.  
  
-   Ein Wert ( `ProductModelName` ) aus einer Spalte vom Typ **XML** , um die Abfrage interessanter zu gestalten.  
  
 Im Folgenden wird die Abfrage aufgeführt:  
  
```sql
DECLARE @price money  
  
SET @price=2500.00  
SELECT ProductID, Production.ProductModel.ProductModelID,CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  
       <Product   
           ProductID="{ sql:column("Production.Product.ProductID") }"  
           ProductModelID= "{ sql:column("Production.Product.ProductModelID") }"  
           ProductModelName="{/pd:ProductDescription[1]/@ProductModelName }"  
           ListPrice="{ sql:column("Production.Product.ListPrice") }"  
           DiscountPrice="{ sql:variable("@price") }"  
        />')   
FROM Production.Product   
JOIN Production.ProductModel  
ON Production.Product.ProductModelID = Production.ProductModel.ProductModelID  
WHERE ProductID=771  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Der XML-Code wird durch die XQuery-Abfrage in der `query()`-Methode erstellt.  
  
-   Das- `namespace` Schlüsselwort wird verwendet, um ein Namespace Präfix im [XQuery-Prolog](../xquery/modules-and-prologs-xquery-prolog.md)zu definieren. Dies geschieht, weil der `ProductModelName`-Attributwert aus der Spalte des `CatalogDescription xml`-Typs abgerufen wird, der ein Schema zugeordnet ist.  
  
 Dies ist das Ergebnis:  
  
```xml
<Product ProductID="771" ProductModelID="19"   
         ProductModelName="Mountain 100"   
         ListPrice="3399.99" DiscountPrice="2500" />  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server XQuery-Erweiterungsfunktionen](./xquery-extension-functions-sql-column.md)   
 [Vergleichen von typisiertem XML mit nicht typisiertem XML](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [XML-Daten &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Erstellen von Instanzen der XML-Daten](../relational-databases/xml/create-instances-of-xml-data.md)   
 [XML-Datentypmethoden](../t-sql/xml/xml-data-type-methods.md)   
 [XML DML &#40;Data Modification Language&#41;](../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
