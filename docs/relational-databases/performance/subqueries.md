---
title: Unterabfragen (SQL Server) | Microsoft-Dokumentation
description: Sehen Sie sich ein Beispiel für eine Unterabfrage an, wobei es sich um eine Abfrage handelt, die in einer SELECT-, INSERT-, UPDATE- oder DELETE-Anweisung bzw. in einer anderen Unterabfrage in SQL Server geschachtelt ist.
ms.custom: ''
ms.date: 02/18/2018
ms.prod: sql
ms.technology: performance
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Subquery
- Subqueries
- subqueries [SQL Server], fundamentals
- subqueries [SQL Server], correlated
- subqueries [SQL Server], types
ms.assetid: bfc97432-c14c-4768-9dc5-a9c512f6b2bd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6ce8c74858311d125d8053272f0cff3e14917cf6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345528"
---
# <a name="subqueries-sql-server"></a>Unterabfragen (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 
Eine Unterabfrage ist eine Abfrage, die in einer `SELECT`-, `INSERT`-, `UPDATE`- oder `DELETE`-Anweisung bzw. in einer anderen Unterabfrage geschachtelt ist. Eine Unterabfrage kann überall dort verwendet werden, wo ein Ausdruck zulässig ist. In diesem Beispiel wird eine Unterabfrage als Spaltenausdruck namens „MaxUnitPrice“ in einer SELECT-Anweisung verwendet.

```sql
USE AdventureWorks2016;
GO
SELECT Ord.SalesOrderID, Ord.OrderDate,
    (SELECT MAX(OrdDet.UnitPrice)
     FROM Sales.SalesOrderDetail AS OrdDet
     WHERE Ord.SalesOrderID = OrdDet.SalesOrderID) AS MaxUnitPrice
FROM Sales.SalesOrderHeader AS Ord;
GO
```

## <a name="subquery-fundamentals"></a><a name="fundamentals"></a> Grundlegende Informationen zu Unterabfragen
Eine Unterabfrage wird auch innere Abfrage oder innere SELECT-Anweisung genannt, während die Anweisung mit einer Unterabfrage als äußere Abfrage oder äußere SELECT-Anweisung bezeichnet wird.   

Viele [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen, die Unterabfragen einschließen, können auch als Joins formuliert werden. Andere Fragestellungen können nur mithilfe von Unterabfragen formuliert werden. In [!INCLUDE[tsql](../../includes/tsql-md.md)] gibt es normalerweise keinen Leistungsunterschied zwischen einer Anweisung, die eine Unterabfrage enthält, und einer semantisch gleichbedeutenden Version ohne Unterabfrage. In manchen Fällen, in denen das Vorhandensein bestimmter Daten überprüft werden muss, wird mit einem Join jedoch eine bessere Leistung erzielt. Denn ansonsten muss die geschachtelte Abfrage für jedes einzelne Ergebnis der äußeren Abfrage verarbeitet werden, damit die Entfernung von Duplikaten sichergestellt ist. In solchen Fällen erzielt ein Join bessere Ergebnisse. Das folgende Beispiel zeigt eine `SELECT`-Anweisung, die mit einer Unterabfrage erstellt wurde, und eine `SELECT`-Anweisung, die mit einem Join erstellt wurde. Beide geben dasselbe Resultset zurück:

```sql
USE AdventureWorks2016;
GO

/* SELECT statement built using a subquery. */
SELECT Name
FROM Production.Product
WHERE ListPrice =
    (SELECT ListPrice
     FROM Production.Product
     WHERE Name = 'Chainring Bolts' );
GO

/* SELECT statement built using a join that returns
   the same result set. */
SELECT Prd1. Name
FROM Production.Product AS Prd1
     JOIN Production.Product AS Prd2
       ON (Prd1.ListPrice = Prd2.ListPrice)
WHERE Prd2. Name = 'Chainring Bolts';
GO
```

Eine Unterabfrage, die in einer äußeren SELECT-Anweisung geschachtelt ist, besitzt folgende Komponenten:    
-   Eine reguläre `SELECT`-Abfrage mit den normalen Auswahllistenkomponenten.   
-   Eine reguläre `FROM`-Klausel mit einem oder mehreren Tabellen- oder Sichtnamen.   
-   Einer optionalen `WHERE`-Klausel.   
-   Einer optionalen `GROUP BY`-Klausel.   
-   Einer optionalen `HAVING`-Klausel.   

Die SELECT-Abfrage einer Unterabfrage wird immer in Klammern eingeschlossen. Sie kann keine `COMPUTE`- oder `FOR BROWSE`-Klausel enthalten und darf nur dann eine `ORDER BY`-Klausel einschließen, wenn auch eine TOP-Klausel angegeben ist.   

Eine Unterabfrage kann in der `WHERE`- oder `HAVING`-Klausel einer äußeren `SELECT`-, `INSERT`-, `UPDATE`- oder `DELETE`-Anweisung oder in einer anderen Unterabfrage geschachtelt sein. Bis zu 32 Schachtelungsebenen sind möglich. Allerdings variiert das Limit in Abhängigkeit vom verfügbaren Arbeitsspeicher und der Komplexität anderer Ausdrücke in der Abfrage. Einzelne Abfragen unterstützen möglicherweise keine Schachtelung bis zu 32 Ebenen. Sofern eine Unterabfrage einen einzelnen Wert zurückgibt, kann sie in allen Fällen auftreten, in denen auch ein Ausdruck verwendet werden kann.   

Wenn eine Tabelle nur in einer Unterabfrage, jedoch nicht in der äußeren Abfrage verwendet wird, können Spalten aus dieser Tabelle nicht in die Ausgabe (die Auswahlliste der äußeren Abfrage) eingeschlossen werden.   

Anweisungen, die eine Unterabfrage einschließen, besitzen in der Regel eines der folgenden Formate:   
-   WHERE-Ausdruck \[NOT] IN (Unterabfrage)
-   WHERE-Ausdruck comparison_operator \[ANY | ALL] (Unterabfrage)
-   WHERE \[NOT] EXISTS (Unterabfrage)   

In manchen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen kann die Unterabfrage wie eine unabhängige Abfrage ausgewertet werden. Grundsätzlich werden die Ergebnisse der Unterabfrage in die äußere Abfrage eingesetzt (auch wenn [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen mit Unterabfragen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht unbedingt auf diese Weise verarbeitet werden).    

Es gibt drei grundlegende Arten von Unterabfragen. Arten: 
-   Unterabfragen, die Listen bearbeiten und mit `IN` eingeleitet werden, oder Unterabfragen, die mit einem durch `ANY` oder `ALL` geänderten Vergleichsoperator eingeleitet werden.
-   Unterabfragen, die mit einem nicht geänderten Vergleichsoperator eingeleitet werden und einen einzelnen Wert zurückgeben müssen.
-   Unterabfragen, die mit `EXISTS` eingeleitete Tests auf Vorhandensein bestimmter Daten darstellen.

## <a name="subquery-rules"></a><a name="rules"></a> Regeln für Unterabfragen
Eine Unterabfrage unterliegt den folgenden Beschränkungen: 
-   Die Auswahlliste einer mit einem Vergleichsoperator eingeleiteten Unterabfrage darf nur einen einzigen Ausdruck oder Spaltennamen einschließen (außer bei `EXISTS` und `IN`, die sich auf Anweisungen mit `SELECT *` bzw. auf eine Liste beziehen).   
-   Wenn die `WHERE`-Klausel einer äußeren Abfrage einen Spaltennamen einschließt, muss sie mit der Spalte in der Auswahlliste der Unterabfrage verknüpfbar sein (kompatible Datentypen).   
-   Die Datentypen **ntext**, **text** und **image** können nicht in der Auswahlliste von Unterabfragen verwendet werden.   
-   Da sie einen einzelnen Wert zurückgeben müssen, können Unterabfragen, die mit einem nicht geänderten Vergleichsoperator (dem nicht das Schlüsselwort ANY oder ALL folgt) eingeleitet werden, keine `GROUP BY`- und `HAVING`-Klauseln enthalten.   
-   Das `DISTINCT`-Schlüsselwort darf nicht bei Unterabfragen verwendet werden, die GROUP BY einschließen.
-   Die `COMPUTE`- und `INTO`-Klauseln können nicht angegeben werden.   
-   `ORDER BY` kann nur angegeben werden, wenn auch `TOP` angegeben wird.   
-   Eine mit einer Unterabfrage erstellte Sicht kann nicht aktualisiert werden.   
-   Die Auswahlliste einer mit `EXISTS` eingeleiteten Unterabfrage besitzt laut Vereinbarung ein Sternchen (\*) statt eines einzelnen Spaltennamens. Die Regeln für eine mit `EXISTS` eingeleitete Unterabfrage entsprechen den Regeln für eine standardmäßige Auswahlliste, da eine mit `EXISTS` eingeleitete Unterabfrage einen Test auf das Vorhandensein bestimmter Daten erstellt und keine Daten, sondern TRUE oder FALSE zurückgibt.   

## <a name="qualifying-column-names-in-subqueries"></a><a name="qualifying"></a> Qualifizieren von Spaltennamen in Unterabfragen
Im folgenden Beispiel wird die *BusinessEntityID*-Spalte in der `WHERE`-Klausel der äußeren Abfrage implizit durch den Tabellennamen in der `FROM`-Klausel der äußeren Abfrage (*Sales.Store*) qualifiziert. Der Verweis auf *CustomerID* in der Auswahlliste der Unterabfrage wird durch die `FROM`-Klausel der Unterabfrage qualifiziert, also durch die *Sales.Customer*-Tabelle.

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Sales.Store
WHERE BusinessEntityID NOT IN
    (SELECT CustomerID
     FROM Sales.Customer
     WHERE TerritoryID = 5);
GO
```

Allgemein gilt die Regel, dass Spaltennamen in einer Anweisung implizit durch die Tabelle qualifiziert werden, auf die in der `FROM`-Klausel derselben Ebene verwiesen wird. Wenn eine Spalte in der Tabelle nicht vorhanden ist, auf die in einer `FROM`-Klausel einer Unterabfrage verwiesen wird, wird sie implizit durch die Tabelle qualifiziert, auf die in der `FROM`-Klausel der äußeren Abfrage verwiesen wird.   

Werden diese impliziten Annahmen angegeben, lautet die Abfrage folgendermaßen:

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Sales.Store
WHERE Sales.Store.BusinessEntityID NOT IN
    (SELECT Sales.Customer.CustomerID
     FROM Sales.Customer
     WHERE TerritoryID = 5);
GO
```

Es empfiehlt sich immer, den Tabellennamen explizit anzugeben, und es ist immer möglich, implizite Annahmen zu Tabellennamen durch explizite Qualifizierungen zu überschreiben.   

> [!IMPORTANT]
> Wenn in einer Unterabfrage auf eine Spalte verwiesen wird, die nicht in der Tabelle vorhanden ist, auf die in der `FROM`-Klausel der Unterabfrage verwiesen wird, die jedoch in einer Tabelle vorhanden ist, auf die durch die `FROM`-Klausel der äußeren Abfrage verwiesen wird, wird die Abfrage ohne Fehler ausgeführt. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qualifiziert die Spalte in der Unterabfrage implizit mit dem Tabellennamen in der äußeren Abfrage.   

## <a name="multiple-levels-of-nesting"></a><a name="nesting"></a> Mehrere Schachtelungsebenen
Eine Unterabfrage kann selbst wiederum eine oder mehrere Unterabfragen beinhalten. In einer Anweisung können beliebig viele Unterabfragen geschachtelt sein.   

Die folgende Abfrage sucht die Namen aller Mitarbeiter, die im Vertrieb arbeiten.   

```sql
USE AdventureWorks2016;
GO
SELECT LastName, FirstName
FROM Person.Person
WHERE BusinessEntityID IN
    (SELECT BusinessEntityID
     FROM HumanResources.Employee
     WHERE BusinessEntityID IN
        (SELECT BusinessEntityID
         FROM Sales.SalesPerson)
    );
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
LastName                                           FirstName
-------------------------------------------------- -----------------------
Jiang                                              Stephen
Abbas                                              Syed
Alberts                                            Amy
Ansman-Wolfe                                       Pamela
Campbell                                           David
Carson                                             Jillian
Ito                                                Shu
Mitchell                                           Linda
Reiter                                             Tsvi
Saraiva                                            Jos
Vargas                                             Garrett
Varkey Chudukatil                                  Ranjit
Valdez                                             Rachel
Tsoflias                                           Lynn
Pak                                                Jae
Blythe                                             Michael
Mensa-Annan                                        Tete

(17 row(s) affected)
```

Die innerste Abfrage gibt die IDs der Vertriebsmitarbeiter zurück. Die Abfrage auf der nächsthöheren Ebene wird mit diesen Vertriebsmitarbeiter-IDs ausgewertet und gibt die Kontakt-ID-Nummern der Mitarbeiter zurück. Zuletzt ermittelt die äußerste Abfrage anhand der Kontakt-IDs die Namen der Mitarbeiter.   

Sie können diese Abfrage auch als Join ausdrücken:

```sql
USE AdventureWorks2016;
GO
SELECT LastName, FirstName
FROM Person.Person c
INNER JOIN HumanResources.Employee e
ON c.BusinessEntityID = e.BusinessEntityID
JOIN Sales.SalesPerson s 
ON e.BusinessEntityID = s.BusinessEntityID;
GO
```

## <a name="correlated-subqueries"></a><a name="correlated"></a> Korrelierte Unterabfragen
Viele Abfragen können ausgewertet werden, indem die Unterabfrage einmal ausgeführt wird und der Ergebniswert oder die -werte in die `WHERE`-Klausel der äußeren Abfrage eingesetzt werden. In Abfragen mit einer korrelierten Unterabfrage (auch wiederholte Unterabfrage genannt) hängt die Unterabfrage für ihre Werte von der äußeren Abfrage ab. Das bedeutet, dass die Unterabfrage wiederholt ausgeführt wird, und zwar einmal für jede Zeile, die von der äußeren Abfrage ausgewählt werden könnte.
Diese Abfrage ruft eine Instanz des Vor- und Nachnamens der einzelnen Mitarbeiter ab, für die die Prämie in der *SalesPerson*-Tabelle 5000 beträgt und für die die Mitarbeiter-IDs in der *Employee*-Tabelle und der *SalesPerson*-Tabelle übereinstimmen.

```sql
USE AdventureWorks2016;
GO
SELECT DISTINCT c.LastName, c.FirstName, e.BusinessEntityID 
FROM Person.Person AS c JOIN HumanResources.Employee AS e
ON e.BusinessEntityID = c.BusinessEntityID 
WHERE 5000.00 IN
    (SELECT Bonus
    FROM Sales.SalesPerson sp
    WHERE e.BusinessEntityID = sp.BusinessEntityID) ;
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
LastName FirstName BusinessEntityID
-------------------------- ---------- ------------
Ansman-Wolfe Pamela 280
Saraiva José 282

(2 row(s) affected)
```

Die vorherige Unterabfrage in dieser Anweisung kann nicht unabhängig von der äußeren Abfrage ausgewertet werden. Sie benötigt einen Wert für *Employee.BusinessEntityID*, wobei sich dieser Wert jedoch ändert, während [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterschiedliche Zeilen in *Employee* untersucht.   
Auf die gleiche Weise wird diese Abfrage ausgewertet: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] untersucht für jede Zeile der Employee-Tabelle die Aufnahme in die Ergebnisse, indem in der inneren Abfrage der Wert jeder Zeile ersetzt wird.
Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beispielsweise zunächst die Zeile für `Syed Abbas` überprüft, nimmt die Variable *Employee.BusinessEntityID* den Wert 285 an, den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in die innere Abfrage einsetzt.

```sql
USE AdventureWorks2016;
GO
SELECT Bonus
FROM Sales.SalesPerson
WHERE BusinessEntityID = 285;
GO
```

Das Ergebnis ist 0 (`Syed Abbas` erhielt keine Prämie, weil er kein Vertriebsmitarbeiter ist), sodass die äußere Abfrage ausgewertet wird zu:

```sql
USE AdventureWorks2016;
GO
SELECT LastName, FirstName
FROM Person.Person AS c JOIN HumanResources.Employee AS e
ON e.BusinessEntityID = c.BusinessEntityID 
WHERE 5000 IN (0.00);
GO
```

Da dies falsch ist, wird die Zeile zu `Syed Abbas` nicht in die Ergebnisse eingeschlossen. Verfahren Sie mit der Zeile zu `Pamela Ansman-Wolfe` auf die gleiche Weise. Sie werden feststellen, dass diese Zeile in den Ergebnissen vorhanden ist.     

Korrelierte Unterabfragen können auch Tabellenwertfunktionen in die `FROM`-Klausel einschließen, indem ein Verweis auf Spalten aus einer Tabelle in der äußeren Abfrage als ein Argument der Tabellenwertfunktion erfolgt. In diesem Fall wird die Tabellenwertfunktion für jede Zeile in der äußeren Abfrage entsprechend der Unterabfrage bewertet.    
  
## <a name="subquery-types"></a><a name="types"></a> Arten von Unterabfragen
Unterabfragen können an vielen Stellen angegeben werden: 
-   Mit Aliasnamen. Weitere Informationen finden Sie unter [Unterabfragen mit Aliasnamen](#aliases).
-   Mit `IN` oder `NOT IN`. Weitere Informationen finden Sie unter [Unterabfragen mit IN](#in) und [Unterabfragen mit NOT IN](#notin).
-   In `UPDATE`-, `DELETE`- und `INSERT`-Anweisungen. Weitere Informationen finden Sie unter [Unterabfragen in den Anweisungen UPDATE, DELETE und INSERT](#upsert).
-   Mit Vergleichsoperatoren. Weitere Informationen finden Sie unter [Unterabfragen mit Vergleichsoperatoren](#comparison).
-   Mit `ANY`, `SOME` oder `ALL`. Weitere Informationen finden Sie unter [Mit ANY, SOME oder ALL modifizierte Vergleichsoperatoren](#comparison_modified).
-   Mit `EXISTS` oder `NOT EXISTS`. Weitere Informationen finden Sie unter [Unterabfragen mit EXISTS](#exists) und [Unterabfragen mit NOT EXISTS](#notexists).
-   Anstelle von Ausdrücken. Weitere Informationen finden Sie unter [Anstelle von Ausdrücken verwendete Unterabfragen](#expression).

### <a name="subqueries-with-aliases"></a><a name="aliases"></a> Unterabfragen mit Aliasnamen
Viele Anweisungen, in denen die Unterabfrage und die äußere Abfrage auf dieselbe Tabelle verweisen, können als Selbstjoin (Verknüpfungen einer Tabelle mit sich selbst) ausgedrückt werden. Beispielsweise können die Adressen von Mitarbeitern aus einem bestimmten Bundesstaat mit einer Unterabfrage gesucht werden:   

```sql
USE AdventureWorks2016;
GO
SELECT StateProvinceID, AddressID
FROM Person.Address
WHERE AddressID IN
    (SELECT AddressID
     FROM Person.Address
     WHERE StateProvinceID = 39);
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
StateProvinceID AddressID
----------- -----------
39 942
39 955
39 972
39 22660

(4 row(s) affected)
```   

Sie können auch einen Selbstjoin verwenden:   

```sql
USE AdventureWorks2016;
GO
SELECT e1.StateProvinceID, e1.AddressID
FROM Person.Address AS e1
INNER JOIN Person.Address AS e2
ON e1.AddressID = e2.AddressID
AND e2.StateProvinceID = 39;
GO
```

Tabellenaliasnamen sind erforderlich, weil die mit sich selbst verknüpfte Tabelle zwei verschiedene Funktionen erfüllt. Aliasnamen können auch in geschachtelten Abfragen verwendet werden, in denen sowohl die innere als auch die äußere Abfrage auf dieselbe Tabelle verweisen.    

```sql
USE AdventureWorks2016;
GO
SELECT e1.StateProvinceID, e1.AddressID
FROM Person.Address AS e1
WHERE e1.AddressID IN
    (SELECT e2.AddressID
     FROM Person.Address AS e2
     WHERE e2.StateProvinceID = 39);
GO
```

Explizite Aliasnamen machen deutlich, dass ein Verweis auf *Person.Address* in der Unterabfrage eine andere Bedeutung als der Verweis in der äußeren Abfrage hat.   

### <a name="subqueries-with-in"></a><a name="in"></a> Unterabfragen mit IN
Das Ergebnis einer mit `IN` (oder mit `NOT IN`) eingeleiteten Unterabfrage entspricht einer Liste aus 0 oder mehr Werten. Sobald die Unterabfrage Ergebnisse zurückgibt, werden diese von der äußeren Abfrage verwendet.    
Die folgende Abfrage sucht die Namen aller Wheel-Produkte, die Adventure Works Cycles herstellt.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]     

```
Name
----------------------------
LL Mountain Front Wheel
ML Mountain Front Wheel
HL Mountain Front Wheel
LL Road Front Wheel
ML Road Front Wheel
HL Road Front Wheel
Touring Front Wheel
LL Mountain Rear Wheel
ML Mountain Rear Wheel
HL Mountain Rear Wheel
LL Road Rear Wheel
ML Road Rear Wheel
HL Road Rear Wheel
Touring Rear Wheel

(14 row(s) affected)
```

Diese Anweisung wird in zwei Schritten ausgewertet. Die innere Abfrage gibt zunächst die Unterkategorie-ID zurück, die dem Namen "Wheel" entspricht (17). Danach wird dieser Wert in die äußere Abfrage eingesetzt, die die zu den Unterkategorie-IDs gehörenden Produktnamen in „Product“ findet.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN ('17');
GO
```

Beim Verwenden eines Joins statt einer Unterabfrage zeigt sich u. a. folgender Unterschied: Wenn Sie für dieses und ähnliche Probleme einen Join statt einer Unterabfrage verwenden, können Sie im Ergebnis die Spalten aus mehreren Tabellen anzeigen. Wenn Sie beispielsweise den Namen der Produktunterkategorie in die Ergebnisse einschließen möchten, müssen Sie die Variante mit dem Join verwenden.    

```sql
USE AdventureWorks2016;
GO
SELECT p.Name, s.Name
FROM Production.Product p
INNER JOIN Production.ProductSubcategory s
ON p.ProductSubcategoryID = s.ProductSubcategoryID
AND s.Name = 'Wheels';
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]    

```
Name
LL Mountain Front Wheel Wheels
ML Mountain Front Wheel Wheels
HL Mountain Front Wheel Wheels
LL Road Front Wheel Wheels
ML Road Front Wheel Wheels
HL Road Front Wheel Wheels
Touring Front Wheel Wheels
LL Mountain Rear Wheel Wheels
ML Mountain Rear Wheel Wheels
HL Mountain Rear Wheel Wheels
LL Road Rear Wheel Wheels
ML Road Rear Wheel Wheels
HL Road Rear Wheel Wheels
Touring Rear Wheel Wheels

(14 row(s) affected)
```    

Die folgende Abfrage sucht die Namen aller Hersteller, deren Bonität gut ist, bei denen Adventure Works Cycles mindestens 20 Artikel bestellt und deren durchschnittliche Vorlaufzeit bei Lieferungen 16 Tage beträgt.    

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Purchasing.Vendor
WHERE CreditRating = 1
AND BusinessEntityID IN
    (SELECT BusinessEntityID
     FROM Purchasing.ProductVendor
     WHERE MinOrderQty >= 20
     AND AverageLeadTime < 16);
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
Name
--------------------------------------------------
Compete Enterprises, Inc
International Trek Center
First National Sport Co.
Comfort Road Bicycles
Circuit Cycles
First Rate Bicycles
Jeff's Sporting Goods
Competition Bike Training Systems
Electronic Bike Repair & Supplies
Crowley Sport
Expert Bike Co
Team Athletic Co.
Compete, Inc.   

(13 row(s) affected)
```   

Die innere Abfrage wird ausgewertet und gibt die IDs der Hersteller zurück, die den Bedingungen der Unterabfrage entsprechen. Danach wird die äußere Abfrage ausgewertet. Beachten Sie, dass Sie in den WHERE-Klauseln der inneren und äußeren Abfrage mehrere Bedingungen einschließen können.   

Mit einem Join kann dieselbe Abfrage folgendermaßen ausgedrückt werden:

```sql
USE AdventureWorks2016;
GO
SELECT DISTINCT Name
FROM Purchasing.Vendor v
INNER JOIN Purchasing.ProductVendor p
ON v.BusinessEntityID = p.BusinessEntityID
WHERE CreditRating = 1
  AND MinOrderQty >= 20
  AND AverageLeadTime < 16;
GO
```

Ein Join kann immer als Unterabfrage ausgedrückt werden. Demgegenüber kann eine Unterabfrage zwar häufig, jedoch nicht immer als Join ausgedrückt werden. Die Ursache hierfür liegt in der Symmetrie von Joins: Sie können die Tabellen A und B in beliebiger Reihenfolge verknüpfen und erhalten immer dieselben Ergebnisse. Dies gilt nicht, wenn eine Unterabfrage verwendet wird.    

### <a name="subqueries-with-not-in"></a><a name="notin"></a> Unterabfragen mit NOT IN
Auch Unterabfragen, die mit dem NOT IN-Schlüsselwort eingeleitet werden, geben eine Liste aus null oder mehr Werten zurück.   
Die folgende Abfrage sucht die Namen aller Produkte, die keine fertigen Fahrräder sind.   

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID NOT IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Mountain Bikes' 
        OR Name = 'Road Bikes'
        OR Name = 'Touring Bikes');
GO
```

Diese Anweisung kann nicht in einen Join konvertiert werden. Der analoge Join mit Ungleich hat eine andere Bedeutung: Sie sucht die Namen von Produkten, die sich in einer Unterkategorie befinden, die nicht fertige Fahrräder sind.      

### <a name="subqueries-in-update-delete-and-insert-statements"></a><a name="upsert"></a> Unterabfragen in den Anweisungen UPDATE, DELETE und INSERT
Unterabfragen können in den Anweisungen `UPDATE`, `DELETE`, `INSERT` und `SELECT` der Datenbearbeitungssprache (Data Manipulation Language, DML) geschachtelt werden.    

Das folgende Beispiel verdoppelt den Wert in der *ListPrice*-Spalte der *Production.Product*-Tabelle. Die Unterabfrage in der `WHERE`-Klausel verweist auf die *Purchasing.ProductVendor*-Tabelle, um die in der *Product*-Tabelle aktualisierten Zeilen auf die zu beschränken, die von *BusinessEntity* 1540 angegeben wurden.

```sql
USE AdventureWorks2016;
GO 
UPDATE Production.Product
SET ListPrice = ListPrice * 2
WHERE ProductID IN
    (SELECT ProductID 
     FROM Purchasing.ProductVendor
     WHERE BusinessEntityID = 1540);
GO
```

Nachfolgend ist eine gleichwertige `UPDATE`-Anweisung aufgeführt, die einen Join verwendet:

```sql
USE AdventureWorks2016;
GO 
UPDATE Production.Product
SET ListPrice = ListPrice * 2
FROM Production.Product AS p
INNER JOIN Purchasing.ProductVendor AS pv
    ON p.ProductID = pv.ProductID AND BusinessEntityID = 1540;
GO   
```

### <a name="subqueries-with-comparison-operators"></a><a name="comparison"></a> Unterabfragen mit Vergleichsoperatoren
Unterabfragen können mit einem der folgenden Vergleichsoperatoren eingeleitet werden: (=, < >, >, > =, <, ! >, ! < oder < =).   

Eine Unterabfrage, die mit einem unveränderten Vergleichsoperator (dem nicht `ANY` oder `ALL` folgt) eingeleitet wird, darf keine Werteliste zurückgeben, wie Unterabfragen mit `IN`, sondern muss einen einzelnen Wert zurückgeben. Wenn eine solche Unterabfrage mehrere Werte zurückgibt, wird von SQL Server eine Fehlermeldung angezeigt.    

Sie sollten mit nicht geänderten Vergleichsoperatoren eingeleitete Unterabfragen nur verwenden, wenn Sie bei den Daten und dem vorliegenden Problem sicher sein können, dass die Unterabfrage genau einen Wert zurückgibt.     

Unter der Annahme, dass jeder Vertriebsmitarbeiter nur für eine Vertriebsregion zuständig ist, möchten Sie beispielsweise die Namen aller Kunden finden, die in der Region ansässig sind, die `Linda Mitchell` betreut. Hierzu können Sie eine Anweisung mit einer Unterabfrage schreiben, die mit dem einfachen Vergleichsoperator = eingeleitet wird.    

```sql
USE AdventureWorks2016;
GO
SELECT CustomerID
FROM Sales.Customer
WHERE TerritoryID =
    (SELECT TerritoryID
     FROM Sales.SalesPerson
     WHERE BusinessEntityID = 276);
GO
```

Wenn `Linda Mitchell` jedoch mehrere Vertriebsregionen betreut, wird eine Fehlermeldung ausgegeben. Statt des Vergleichsoperators = könnte die Abfrage mit `IN` formuliert werden (= ANY wäre ebenfalls möglich).    

Mit nicht geänderten Vergleichsoperatoren eingeleitete Unterabfragen schließen häufig Aggregatfunktionen ein, da diese einen einzelnen Wert zurückgeben. Die folgende Anweisung ermittelt z. B. die Namen aller Produkte, deren Listenpreis höher als der durchschnittliche Listenpreis ist.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ListPrice >
    (SELECT AVG (ListPrice)
     FROM Production.Product);
GO
```     

Da mit einem nicht geänderten Vergleichsoperator eingeleitete Unterabfragen einen einzelnen Wert zurückgeben müssen, dürfen sie `GROUP BY`- oder `HAVING`-Klauseln nur dann einschließen, wenn sichergestellt ist, dass die GROUP BY- oder HAVING-Klausel selbst nur einen einzelnen Wert zurückgibt. Die folgende Abfrage findet z. B. die Produkte, deren Preis über dem des Produkts mit dem niedrigsten Preis in der Unterkategorie 14 liegt.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ListPrice >
    (SELECT MIN (ListPrice)
     FROM Production.Product
     GROUP BY ProductSubcategoryID
     HAVING ProductSubcategoryID = 14);
GO
```

### <a name="comparison-operators-modified-by-any-some-or-all"></a><a name="comparison_modified"></a> Mit ANY, SOME oder ALL modifizierte Vergleichsoperatoren
Vergleichsoperatoren, die eine Unterabfrage einleiten, können mit den Schlüsselwörtern ALL oder ANY geändert werden. SOME ist eine ISO-Standard-Entsprechung für `ANY`.     

Mit einem geänderten Vergleichsoperator eingeleitete Unterabfragen geben eine Liste aus 0 oder mehr Werten zurück und können eine `GROUP BY`- oder `HAVING`-Klausel einschließen. Diese Unterabfragen können auch mit `EXISTS` ausgedrückt werden.     

Verwenden wir als Beispiel den Vergleichsoperator >: `>ALL` bedeutet „größer als jeder Wert“. Mit anderen Worten: "größer als der Maximalwert". `>ALL (1, 2, 3)` bedeutet beispielsweise „größer als 3“. `>ANY` bedeutet „größer als mindestens ein Wert“, d.h. „größer als das Minimum“. `>ANY (1, 2, 3)` bedeutet demnach „größer als 1“.
Eine Zeile in einer Unterabfrage mit `>ALL` muss die in der äußeren Abfrage angegebene Bedingung nur erfüllen, wenn der Wert in der Spalte, die die Unterabfrage einleitet, größer als jeder Wert aus der Werteliste ist, die von der Unterabfrage zurückgegeben wird.    

Dementsprechend bedeutet `>ANY`, dass eine Zeile die Bedingung in der äußeren Abfrage nur erfüllt, wenn der Wert in der Spalte, die die Unterabfrage einleitet, größer als mindestens einer der Werte in der Werteliste ist, die von der Unterabfrage zurückgegeben wird.    

Die folgende Abfrage stellt ein Beispiel für eine Unterabfrage dar, die mit einem durch ANY geänderten Vergleichsoperator eingeleitet wird. Sie findet die Produkte, deren Listenpreise größer oder gleich dem maximalen Listenpreis aller Produkt-Unterkategorien sind.    

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ListPrice >= ANY
    (SELECT MAX (ListPrice)
     FROM Production.Product
     GROUP BY ProductSubcategoryID);
GO
```    

Für jede Produkt-Unterkategorie findet die innere Abfrage den maximalen Listenpreis. Die äußere Abfrage betrachtet all diese Werte und ermittelt, welche Listenpreise einzelner Produkte größer oder gleich dem maximalen Listenpreis in allen Produkt-Unterkategorien sind. Wenn `ANY` zu `ALL` geändert wird, gibt die Abfrage nur solche Produkte zurück, deren Listenpreis größer oder gleich allen Listenpreisen ist, die in der inneren Abfrage zurückgegeben wurden.    

Wenn die Unterabfrage keine Werte zurückgibt, gibt auch die Gesamtabfrage keine Werte zurück.    

Der `=ANY`-Operator entspricht `IN`. Um beispielsweise die Namen aller Wheel-Produkte zu ermitteln, die von Adventure Works Cycles hergestellt werden, können Sie entweder `IN` oder `=ANY` verwenden.

```sql
--Using =ANY
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID =ANY
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO

--Using IN
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO
```

Im Folgenden wird das Resultset der beiden Abfragen aufgeführt:

```
Name
--------------------------------------------------
LL Mountain Front Wheel
ML Mountain Front Wheel
HL Mountain Front Wheel
LL Road Front Wheel
ML Road Front Wheel
HL Road Front Wheel
Touring Front Wheel
LL Mountain Rear Wheel
ML Mountain Rear Wheel
HL Mountain Rear Wheel
LL Road Rear Wheel
ML Road Rear Wheel
HL Road Rear Wheel
Touring Rear Wheel

(14 row(s) affected)
```    

Der `<>ANY`-Operator unterscheidet sich jedoch von `NOT IN`: `<>ANY` bedeutet nicht = a, nicht = b oder nicht = c. `NOT IN` bedeutet nicht = a, nicht = b und nicht = c. `<>ALL` ist identisch mit `NOT IN`.     

Die folgende Abfrage findet z. B. die Kunden, die sich in einem Gebiet befinden, das nicht von Vertriebsmitarbeitern abgedeckt ist.     

```sql
USE AdventureWorks2016;
GO
SELECT CustomerID
FROM Sales.Customer
WHERE TerritoryID <> ANY
    (SELECT TerritoryID
     FROM Sales.SalesPerson);
GO
```    

Die Ergebnisse schließen alle Kunden ein, mit Ausnahme der Kunden, deren Vertriebsregionen NULL sind, da jede Region, die einem Kunden zugeordnet ist, von einem Vertriebsmitarbeiter betreut wird. Die innere Abfrage findet alle Vertriebsregionen, die von Vertriebsmitarbeitern betreut werden. Dann findet die äußere Abfrage für jede Region die Kunden, die sich nicht in einer dieser Regionen befinden.    

Aus demselben Grund enthalten die Ergebnisse keinen der Kunden, wenn Sie in dieser Abfrage `NOT IN` verwenden.      

Sie erhalten dieselben Ergebnisse mit dem Operator `<>ALL`, der mit `NOT IN` identisch ist.   

### <a name="subqueries-with-exists"></a><a name="exists"></a> Unterabfragen mit EXISTS
Unterabfragen, die mit dem `EXISTS`-Schlüsselwort eingeleitet werden, dienen als Test auf das Vorhandensein bestimmter Daten. Die `WHERE`-Klausel der äußeren Abfrage testet, ob die von der Unterabfrage zurückgegebenen Zeilen vorhanden sind. Die Unterabfrage gibt keine tatsächlichen Daten zurück, sondern lediglich den Wert TRUE oder FALSE.   

Die Syntax einer mit EXISTS eingeleiteten Unterabfrage lautet wie folgt:   

`WHERE [NOT] EXISTS (subquery)`    

Die folgende Abfrage sucht die Namen aller Produkte, die sich in der Wheels-Unterkategorie befinden:    

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE EXISTS
    (SELECT * 
     FROM Production.ProductSubcategory
     WHERE ProductSubcategoryID = 
            Production.Product.ProductSubcategoryID
        AND Name = 'Wheels');
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]    

```
Name
--------------------------------------------------
LL Mountain Front Wheel
ML Mountain Front Wheel
HL Mountain Front Wheel
LL Road Front Wheel
ML Road Front Wheel
HL Road Front Wheel
Touring Front Wheel
LL Mountain Rear Wheel
ML Mountain Rear Wheel
HL Mountain Rear Wheel
LL Road Rear Wheel
ML Road Rear Wheel
HL Road Rear Wheel
Touring Rear Wheel

(14 row(s) affected)
```    

Sehen Sie sich die Namen der einzelnen Produkte der Reihe nach an, um die Ergebnisse dieser Abfrage zu verstehen. Veranlasst dieser Wert die Unterabfrage zur Rückgabe mindestens einer Zeile? Bewirkt die Abfrage also, dass der Test auf Vorhandensein zu TRUE ausgewertet wird?   

Beachten Sie, dass sich mit EXISTS eingeleitete Unterabfragen in folgender Hinsicht geringfügig von anderen Unterabfragen unterscheiden: 
-   Vor dem `EXISTS`-Schlüsselwort befindet sich weder ein Spaltenname noch eine Konstante oder ein anderer Ausdruck.     
-   Die Auswahlliste einer mit `EXISTS` eingeleiteten Unterabfrage besteht fast immer aus einem Sternchen (*). Spaltennamen müssen nicht aufgelistet werden, da lediglich getestet wird, ob Zeilen vorhanden sind, die die in der Unterabfrage angegebenen Bedingungen erfüllen.    

Das `EXISTS`-Schlüsselwort ist wichtig, da es häufig keine alternative Formulierung ohne Unterabfragen gibt. Manche mit EXISTS erstellten Abfragen können nicht auf andere Weise ausgedrückt werden. Viele Abfragen können jedoch mithilfe von IN oder einem durch `ANY` oder `ALL` geänderten Vergleichsoperator ähnliche Ergebnisse erzielen.     

Die oben gezeigte Abfrage kann z.B. mithilfe von IN ausgedrückt werden:   

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO
```   

### <a name="subqueries-with-not-exists"></a><a name="notexists"></a> Unterabfragen mit NOT EXISTS
`NOT EXISTS` funktioniert auf dieselbe Weise wie `EXISTS`, mit der Ausnahme, dass die umgebende `WHERE`-Klausel nur erfüllt wird, wenn von der Unterabfrage keine Zeilen zurückgegeben werden.    

Um beispielsweise die Namen von Produkten zu finden, die sich nicht in der Unterkategorie Wheels befinden:   

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE NOT EXISTS
    (SELECT * 
     FROM Production.ProductSubcategory
     WHERE ProductSubcategoryID = 
            Production.Product.ProductSubcategoryID
        AND Name = 'Wheels');
GO
```   

### <a name="subqueries-used-in-place-of-an-expression"></a><a name="expression"></a> Anstelle von Ausdrücken verwendete Unterabfragen
In [!INCLUDE[tsql](../../includes/tsql-md.md)] kann eine Unterabfrage überall dort in `SELECT`-, `UPDATE`-, `INSERT`- und `DELETE`-Anweisungen verwendet werden, wo auch ein Ausdruck verwendet werden kann. Eine Ausnahme stellen `ORDER BY`-Listen dar.    

Das folgende Beispiel veranschaulicht, wie Sie diese Erweiterung verwenden können. Diese Abfrage ermittelt die Preise aller Mountainbike-Produkte, ihren Durchschnittspreis sowie die Differenz zwischen dem Preis jedes einzelnen Mountainbikes und dem Durchschnittspreis.    

```sql
USE AdventureWorks2016;
GO
SELECT Name, ListPrice, 
(SELECT AVG(ListPrice) FROM Production.Product) AS Average, 
    ListPrice - (SELECT AVG(ListPrice) FROM Production.Product)
    AS Difference
FROM Production.Product
WHERE ProductSubcategoryID = 1;
GO
```   

## <a name="see-also"></a>Weitere Informationen  
[IN &#40;Transact-SQL&#41;](../../t-sql/language-elements/in-transact-sql.md)       
[EXISTS &#40;Transact-SQL&#41;](../../t-sql/language-elements/exists-transact-sql.md)     
[ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md)     
[SOME | ANY &#40;Transact-SQL&#41;](../../t-sql/language-elements/some-any-transact-sql.md)     
[Joins](../../relational-databases/performance/joins.md)    
[Vergleichsoperatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md)       
  
