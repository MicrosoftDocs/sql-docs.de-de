---
title: 'SQL: column ()-Funktion (XQuery) | Microsoft-Dokumentation'
description: 'Erfahren Sie, wie die XQuery-Funktion "SQL: column ()" zum Binden von relationalen XML-Daten in XML und zum Zusammenführen von relationalen und XML-Daten verwendet werden kann.'
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
- sql:column function
- sql:column() function
ms.assetid: e8f67bdf-b489-49a9-9d0f-2069c1750467
author: rothja
ms.author: jroth
ms.openlocfilehash: 3d2f550e3516594a269257786e109df9c84a1082
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341873"
---
# <a name="xquery-extension-functions---sqlcolumn"></a>XQuery-Erweiterungsfunktionen – sql:column()
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Wie im Thema [binden relationaler Daten in XML](../t-sql/xml/binding-relational-data-inside-xml-data.md)beschrieben, können Sie die **SQL: column (()** -Funktion verwenden, wenn Sie [XML-Datentyp Methoden](../t-sql/xml/xml-data-type-methods.md) zum verfügbar machen eines relationalen Werts in XQuery verwenden.  
  
 Beispielsweise wird die [Query ()-Methode (XML-Datentyp)](../t-sql/xml/query-method-xml-data-type.md) verwendet, um eine Abfrage für eine XML-Instanz anzugeben, die in einer Variablen oder Spalte vom Typ **XML** gespeichert ist. Manchmal kann es auch wünschenswert sein, dass eine Abfrage Werte aus einer anderen Nicht-XML-Spalte verwendet, um relationale und XML-Daten zu verbinden. Zu diesem Zweck verwenden Sie die **SQL: column ()** -Funktion.  
  
 Der SQL-Wert wird einem entsprechenden XQuery-Wert zugeordnet, und sein Datentyp ist ein XQuery-Basistyp, der mit dem entsprechenden SQL-Typ äquivalent ist.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sql:column("columnName")  
```  
  
## <a name="remarks"></a>Bemerkungen  
 Beachten Sie, dass der Verweis auf eine Spalte, die in der **SQL: column ()** -Funktion in einer XQuery angegeben ist, auf eine Spalte in der Zeile verweist, die verarbeitet wird.  
  
 In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] können Sie nur auf eine **XML** -Instanz im Kontext des Quell Ausdrucks einer XML-DML-INSERT-Anweisung verweisen. andernfalls können Sie nicht auf Spalten verweisen, die vom Typ **XML** oder von einem benutzerdefinierten CLR-Typ sind.  
  
 Die **SQL: column ()** -Funktion wird in joinvorgängen nicht unterstützt. Stattdessen kann der APPLY-Vorgang verwendet werden.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-sqlcolumn-to-retrieve-the-relational-value-inside-xml"></a>A. Abrufen des relationalen Werts in XML mit sql:column()  
 Das folgende Beispiel zeigt beim Erstellen von XML, wie Werte aus einer relationalen Nicht-XML-Spalte abgerufen werden, um XML und relationale Daten zu binden.  
  
 Die Abfrage erstellt XML in der folgenden Form:  
  
```xml
<Product ProductID="771" ProductName="Mountain-100 Silver, 38" ProductPrice="3399.99" ProductModelID="19"   
  ProductModelName="Mountain 100" />  
```  
  
 Beachten Sie im erstellten XML Folgendes:  
  
-   Die Attributwerte **ProductID**, **ProductName** und **ProductPrice** werden aus der **Product** -Tabelle abgerufen.  
  
-   Der **ProductModelID** -Attribut Wert wird aus der **ProductModel** -Tabelle abgerufen.  
  
-   Um die Abfrage interessanter zu gestalten, wird der **ProductModelName** -Attribut Wert aus der **CatalogDescription** -Spalte vom **XML-Typ** abgerufen. Da die XML-Produktmodell-Kataloginformationen nicht für alle Produktmodelle gespeichert werden, wird die `if`-Anweisung nur zum Abrufen des Werts verwendet, wenn dieser vorhanden ist.  
  
    ```sql
    SELECT P.ProductID, CatalogDescription.query('  
    declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
           <Product   
               ProductID=       "{ sql:column("P.ProductID") }"  
               ProductName=     "{ sql:column("P.Name") }"  
               ProductPrice=    "{ sql:column("P.ListPrice") }"  
               ProductModelID= "{ sql:column("PM.ProductModelID") }" >  
               { if (not(empty(/pd:ProductDescription))) then  
                 attribute ProductModelName { /pd:ProductDescription[1]/@ProductModelName }  
                else   
                   ()  
    }  
            </Product>  
    ') as Result  
    FROM Production.ProductModel PM, Production.Product P  
    WHERE PM.ProductModelID = P.ProductModelID  
    AND   CatalogDescription is not NULL  
    ORDER By PM.ProductModelID  
    ```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Da die Werte aus zwei verschiedenen Tabellen abgerufen werden, gibt die FROM-Klausel zwei Tabellen an. Die Bedingung in der WHERE-Klausel filtert das Ergebnis und ruft nur Produkte ab, deren Produktmodelle über Katalogbeschreibungen verfügen.  
  
-   Das **Namespace** -Schlüsselwort im [XQuery-Prolog](../xquery/modules-and-prologs-xquery-prolog.md) definiert das XML-Namespace Präfix "PD", das im Abfragetext verwendet wird. Beachten Sie, dass die Tabellenaliasse "P" und "PM" in der FROM-Klausel der Abfrage selbst definiert werden.  
  
-   Die **SQL: column ()** -Funktion wird verwendet, um nicht-XML-Werte in XML zu verwenden.  
  
 Dies ist das Teilergebnis:  
  
```  
ProductID               Result  
-----------------------------------------------------------------  
771         <Product ProductID="771"                   ProductName="Mountain-100 Silver, 38"   
                  ProductPrice="3399.99" ProductModelID="19"   
                  ProductModelName="Mountain 100" />  
...  
```  
  
 Die folgende Abfrage erstellt XML, das produktspezifische Informationen enthält. Diese Informationen umfassen die Werte ProductID, ProductName, ProductPrice und, wenn verfügbar, ProductModelName für alle Produkte, die zu einem bestimmten Produktmodell, ProductModelID=19, gehören. Der XML-Code wird dann der @x Variablen vom Typ **XML** zugewiesen.  
  
```sql
declare @x xml  
SELECT @x = CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
       <Product   
           ProductID=       "{ sql:column("P.ProductID") }"  
           ProductName=     "{ sql:column("P.Name") }"  
           ProductPrice=    "{ sql:column("P.ListPrice") }"  
           ProductModelID= "{ sql:column("PM.ProductModelID") }" >  
           { if (not(empty(/pd:ProductDescription))) then  
             attribute ProductModelName { /pd:ProductDescription[1]/@ProductModelName }  
            else   
               ()  
}  
        </Product>  
')   
FROM Production.ProductModel PM, Production.Product P  
WHERE PM.ProductModelID = P.ProductModelID  
And P.ProductModelID = 19  
select @x  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server XQuery-Erweiterungsfunktionen]()   
 [Vergleichen von typisiertem XML mit nicht typisiertem XML](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [XML-Daten &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Erstellen von Instanzen der XML-Daten](../relational-databases/xml/create-instances-of-xml-data.md)   
 [XML-Datentypmethoden](../t-sql/xml/xml-data-type-methods.md)   
 [XML DML &#40;Data Modification Language&#41;](../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
