---
description: OPTION-Klausel (Transact-SQL)
title: OPTION-Klausel (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPTION clause
- OPTION_TSQL
- OPTION
- OPTION_clause_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- clauses [SQL Server], OPTION
- OPTION clause
ms.assetid: f47e2f3f-9302-4711-9d66-16b1a2a7ffe3
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 559b9572d95347d4ef14dfe9664cbaf6fb39bbed
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186577"
---
# <a name="option-clause-transact-sql"></a>OPTION-Klausel (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt an, dass der angezeigte Abfragehinweis in der gesamten Abfrage verwendet werden soll. Jeder Abfragehinweis kann nur einmal angegeben werden, obwohl mehrere Abfragehinweise zulässig sind. Es kann nur eine OPTION-Klausel pro Anweisung angegeben werden.  
  
 Diese Klausel kann in den Anweisungen SELECT, DELETE, UPDATE und MERGE angegeben werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
### <a name="syntax-for-ssnoversion-mdmd-and-ssazure_mdmd"></a>Syntax für [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssazure_md.md](../../includes/ssazure_md.md)]

```syntaxsql

[ OPTION ( <query_hint> [ ,...n ] ) ]   
```  
  
### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Syntax für [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] und [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]

```syntaxsql
  
OPTION ( <query_option> [ ,...n ] )  
  
<query_option> ::=  
    LABEL = label_name |  
    <query_hint>  
  
<query_hint> ::=  
    HASH JOIN   
    | LOOP JOIN   
    | MERGE JOIN  
    | FORCE ORDER  
    | { FORCE | DISABLE } EXTERNALPUSHDOWN  
```  
  
### <a name="syntax-for-sssodfull-mdmd"></a>Syntax für [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)]

```syntaxsql

OPTION ( <query_option> [ ,...n ] )

<query_option> ::=
    LABEL = label_name
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *query_hint*  
 Schlüsselwörter, die angeben, dass Hinweise für den Optimierer verwendet werden, um die Verarbeitung der Anweisung durch die Datenbank-Engine anzupassen. Weitere Informationen finden Sie unter [Abfragehinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-an-option-clause-with-a-group-by-clause"></a>A. Verwenden einer OPTION-Klausel mit einer GROUP BY-Klausel  
 Im folgenden Beispiel wird gezeigt, wie die `OPTION`-Klausel in Verbindung mit einer `GROUP BY`-Klausel verwendet wird.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (HASH GROUP, FAST 10);  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-select-statement-with-a-label-in-the-option-clause"></a>B. SELECT-Anweisung mit einer Bezeichnung in der OPTION-Klausel  
 Im folgenden Beispiel wird eine einfache [!INCLUDE[ssDW](../../includes/ssdw-md.md)]-SELECT-Anweisung mit einer Bezeichnung in der OPTION-Klausel dargestellt.  
  
```sql
-- Uses AdventureWorks  
  
SELECT * FROM FactResellerSales  
  OPTION ( LABEL = 'q17' );  
```  
  
### <a name="c-select-statement-with-a-query-hint-in-the-option-clause"></a>C. SELECT-Anweisung mit einem Abfragehinweis in der OPTION-Klausel  
 Im folgenden Beispiel wird eine SELECT-Anweisung in der OPTION-Klausel dargestellt, die einen HASH JOIN-Abfragehinweis verwendet.  
  
```sql
-- Uses AdventureWorks  
  
SELECT COUNT (*) FROM dbo.DimCustomer a  
INNER JOIN dbo.FactInternetSales b   
ON (a.CustomerKey = b.CustomerKey)  
OPTION (HASH JOIN);  
```  
  
### <a name="d-select-statement-with-a-label-and-multiple-query-hints-in-the-option-clause"></a>D. SELECT-Anweisung mit einer Bezeichnung und mehreren Abfragehinweisen in der OPTION-Klausel  
 Beim folgenden Beispiel handelt es sich um eine [!INCLUDE[ssDW](../../includes/ssdw-md.md)]-SELECT-Anweisung, die eine Bezeichnung und mehrere Abfragehinweise enthält. Bei der Ausführung der Abfrage auf den Computeknoten wendet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen Hashjoin oder Merge Join an, je nachdem, welche Strategie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als optimal auswählt.  
  
```sql
-- Uses AdventureWorks  
  
SELECT COUNT (*) FROM dbo.DimCustomer a  
INNER JOIN dbo.FactInternetSales b   
ON (a.CustomerKey = b.CustomerKey)  
OPTION ( Label = 'CustJoin', HASH JOIN, MERGE JOIN);  
```  
  
### <a name="e-using-a-query-hint-when-querying-a-view"></a>E. Verwenden eines Abfragehinweises beim Abfragen einer Sicht  
 Im folgenden Beispiel wird eine Sicht mit dem Namen CustomerView erstellt und anschließend ein HASH JOIN-Abfragehinweis in einer Abfrage verwendet, die auf eine Sicht und eine Tabelle verweist.  
  
```sql
-- Uses AdventureWorks  
  
CREATE VIEW CustomerView  
AS  
SELECT CustomerKey, FirstName, LastName FROM ssawPDW..DimCustomer;  
  
SELECT COUNT (*) FROM dbo.CustomerView a  
INNER JOIN dbo.FactInternetSales b  
ON (a.CustomerKey = b.CustomerKey)  
OPTION (HASH JOIN);  
  
DROP VIEW CustomerView;
```  
  
### <a name="f-query-with-a-subselect-and-a-query-hint"></a>F. Abfrage mit einer untergeordneten SELECT-Anweisung und einem Abfragehinweis  
 Im folgenden Beispiel wird eine Abfrage dargestellt, die sowohl eine untergeordnete SELECT-Anweisung als auch einen Abfragehinweis enthält. Der Abfragehinweis wird global angewendet. Abfragehinweise dürfen nicht an die untergeordnete SELECT-Anweisung angefügt werden.  
  
```sql
-- Uses AdventureWorks  
  
CREATE VIEW CustomerView AS  
SELECT CustomerKey, FirstName, LastName FROM ssawPDW..DimCustomer;  
  
SELECT * FROM (  
SELECT COUNT (*) AS a FROM dbo.CustomerView a  
INNER JOIN dbo.FactInternetSales b  
ON ( a.CustomerKey = b.CustomerKey )) AS t  
OPTION (HASH JOIN);  
```  
  
### <a name="g-force-the-join-order-to-match-the-order-in-the-query"></a>G. Erzwingen der Übereinstimmung der Joinreihenfolge mit der Reihenfolge in der Abfrage  
 Im folgenden Beispiel wird der FORCE ORDER-Hinweis verwendet, um zu erzwingen, dass der Abfrageplan die von der Abfrage angegebene Joinreihenfolge verwendet. Dies verbessert die Leistung einiger Abfragen, jedoch nicht aller.  
  
```sql
-- Uses AdventureWorks  
  
-- Obtain partition numbers, boundary values, boundary value types, and rows per boundary  
-- for the partitions in the ProspectiveBuyer table of the ssawPDW database.  
SELECT sp.partition_number, prv.value AS boundary_value, lower(sty.name) AS boundary_value_type, sp.rows   
FROM sys.tables st JOIN sys.indexes si ON st.object_id = si.object_id AND si.index_id <2  
JOIN sys.partitions sp ON sp.object_id = st.object_id AND sp.index_id = si.index_id  
JOIN sys.partition_schemes ps ON ps.data_space_id = si.data_space_id   
JOIN sys.partition_range_values prv ON prv.function_id = ps.function_id   
JOIN sys.partition_parameters pp ON pp.function_id = ps.function_id   
JOIN sys.types sty ON sty.user_type_id = pp.user_type_id AND prv.boundary_id = sp.partition_number   
WHERE st.object_id = (SELECT object_id FROM sys.objects WHERE name = 'FactResellerSales')   
ORDER BY sp.partition_number  
OPTION ( FORCE ORDER )  
;  
```  
  
### <a name="h-using-externalpushdown"></a>H. Verwenden von EXTERNALPUSHDOWN  
 Im folgenden Beispiel wird die Weitergabe der WHERE-Klausel an den MapReduce-Auftrag in der externen Hadoop-Tabelle erzwungen.  
  
```sql
SELECT ID FROM External_Table_AS A   
WHERE ID < 1000000  
OPTION (FORCE EXTERNALPUSHDOWN);  
```  
  
 Im folgenden Beispiel wird die Weitergabe der WHERE-Klausel an den MapReduce-Auftrag in der externen Hadoop-Tabelle verhindert. Wo die WHERE-Klausel angewendet wird, werden alle Zeilen an PDW zurückgegeben.  
  
```sql
SELECT ID FROM External_Table_AS A   
WHERE ID < 10  
OPTION (DISABLE EXTERNALPUSHDOWN);  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Hints &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)   
 [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)  
  
  

