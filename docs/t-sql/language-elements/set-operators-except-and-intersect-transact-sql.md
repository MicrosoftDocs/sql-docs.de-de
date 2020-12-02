---
description: Mengenoperatoren – EXCEPT und INTERSECT (Transact-SQL)
title: EXCEPT und INTERSECT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- INTERSECT_TSQL
- EXCEPT_TSQL
- INTERSECT
- EXCEPT
dev_langs:
- TSQL
helpviewer_keywords:
- EXCEPT operator [Transact-SQL]
- queries [SQL Server], comparing
- comparing queries
- INTERSECT operator
ms.assetid: b1019300-171a-4a1a-854f-e1e751de3565
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 06029c531fbdebfd74d3a2314221725a41647853
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128211"
---
# <a name="set-operators---except-and-intersect-transact-sql"></a>Mengenoperatoren – EXCEPT und INTERSECT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Gibt durch den Vergleich zweier Abfragen eindeutige Zeilen zurück.  
  
EXCEPT gibt eindeutige Zeilen aus der linken Eingabeabfrage zurück, die nicht von der rechten Eingabeabfrage ausgegeben werden.  
 
INTERSECT gibt eindeutige Zeilen zurück, die durch die linken und rechten Eingabeabfragen ausgegeben werden.  
  
Für das Kombinieren der Resultsets zweier Abfragen, die EXCEPT oder INTERSECT verwenden, gelten folgende Grundregeln:  
  
-   Die Anzahl und die Reihenfolge der Spalten müssen für alle Abfragen identisch sein.  
  
-   Die Datentypen müssen kompatibel sein.  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
{ <query_specification> | ( <query_expression> ) }   
{ EXCEPT | INTERSECT }  
{ <query_specification> | ( <query_expression> ) }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
\<_query\_specification_> | ( \<_query\_expression_> )  
Eine Abfrageangabe oder ein Abfrageausdruck, die bzw. der Daten zurückgibt, die mit den Daten aus einer anderen Abfrageangabe oder einem anderen Abfrageausdruck zu vergleichen sind. Die Definitionen der Spalten, die Bestandteil eines EXCEPT- oder INTERSECT-Vorgangs sind, müssen nicht identisch sein. Aber sie müssen mithilfe impliziter Konvertierung vergleichbar sein. Wenn sich die Datentypen unterscheiden, bestimmen die Regeln für die [Rangfolge der Datentypen](../../t-sql/data-types/data-type-precedence-transact-sql.md) den für den Vergleich verwendeten Datentyp.  
  
Das Ergebnis basiert auf denselben Regeln wie für das Kombinieren von Ausdrücken, wenn die Typen identisch sind, sich aber in Genauigkeit, Dezimalstellen oder Länge unterscheiden. Weitere Informationen finden Sie unter [Genauigkeit, Dezimalstellen und Länge &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
Die Abfrageangabe bzw. der Abfrageausdruck kann keine Spalten des Datentyps **xml**, **text**, **ntext**, **image** bzw. keine nichtbinären benutzerdefinierten CLR-Typspalten zurückgeben, da diese Datentypen nicht vergleichbar sind.  
  
EXCEPT  
Gibt alle eindeutigen Werte aus der Abfrage links vom EXCEPT-Operator zurück. Diese Werte werden zurückgeben, solange die rechte Abfrage diese Werte nicht auch zurückgibt.  
  
INTERSECT  
Gibt alle eindeutigen Werte zurück, die von den Abfragen auf der linken und rechten Seite des INTERSECT-Operators zurückgegeben werden.  
  
## <a name="remarks"></a>Hinweise  
Die Datentypen vergleichbarer Spalten werden von den Abfragen links und rechts vom EXCEPT- oder INTERSECT-Operator zurückgegeben. Diese Datentypen können Zeichendatentypen mit unterschiedlichen Sortierungen enthalten. Wenn dies der Fall, wird der erforderliche Vergleich gemäß den Regeln der [Rangfolge von Sortierungen](../../t-sql/statements/collation-precedence-transact-sql.md) ausgeführt. Wenn Sie diese Konvertierung nicht ausführen können, gibt die [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] einen Fehler zurück.  
  
Wenn Sie zum Bestimmen EINDEUTIGER Zeilen Spaltenwerte vergleichen, werden zwei NULL-Werte als identisch betrachtet.  
  
EXCEPT und INTERSECT geben die Spaltennamen des Resultsets zurück, die mit den Spaltennamen identisch sind, die die Abfrage links vom Operator zurückgibt.  
  
Spaltennamen oder -aliasse in ORDER BY-Klauseln müssen auf Spaltennamen verweisen, die von der linken Abfrage zurückgegeben werden.  
  
Die NULL-Zulässigkeit aller Spalten des Resultsets, die von EXCEPT oder INTERSECT zurückgegeben wird, entspricht der NULL-Zulässigkeit der entsprechenden Spalte, die von der linken Abfrage des Operators zurückgegeben wird.  
  
Wenn EXCEPT oder INTERSECT zusammen mit anderen Operatoren in einem Ausdruck verwendet wird, wird dieser in der folgenden Rangfolge ausgewertet:  
  
1.  Ausdrücke in Klammern  
  
2.  Der INTERSECT-Operator  
  
3.  EXCEPT und UNION werden auf der Grundlage ihrer Position im Ausdruck von links nach rechts ausgewertet.  
  
Sie können mit EXCEPT oder INTERSECT mehr als zwei Sätze von Abfragen vergleichen. Wenn Sie dies tun, wird die Datentypkonvertierung bestimmt, indem zwei Abfragen nacheinander verglichen werden. Dies erfolgt gemäß der zuvor erwähnten Regeln der Ausdrucksauswertung.  
  
EXCEPT und INTERSECT können nicht in verteilten partitionierten Sichtdefinitionen und Abfragebenachrichtigungen verwendet werden.  
 
EXCEPT und INTERSECT können in verteilten Abfragen verwendet werden. Sie werden jedoch nur auf dem lokalen Server ausgeführt und nicht mithilfe eines Push-Vorgangs an den Verbindungsserver übertragen. Daher kann sich das Verwenden von EXCEPT und INTERSECT in verteilten Abfragen auf die Leistung auswirken.  
  
Sie können Vorwärtscursor und statische Cursor im Resultset verwenden, wenn sie mit einem EXCEPT- oder INTERSECT-Vorgang verwendet werden. Sie können auch einen keysetgesteuerten oder dynamischen Cursor zusammen mit einem EXCEPT- oder INTERSECT-Vorgang verwenden. Wenn Sie dies tun, wird der Cursor des Resultsets des Vorgangs in einen statischen Cursor konvertiert.  
  
Wenn ein EXCEPT-Vorgang mithilfe dem Feature des grafischen Showplans von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] angezeigt wird, wird der Vorgang als [Left Anti Semi Join](../../relational-databases/showplan-logical-and-physical-operators-reference.md) angezeigt, und ein INTERSECT-Vorgang wird als [Left Semi Join](../../relational-databases/showplan-logical-and-physical-operators-reference.md) angezeigt.  
  
## <a name="examples"></a>Beispiele  
In den folgenden Beispielen wird die Verwendung der Operatoren `INTERSECT` und `EXCEPT` veranschaulicht. Die erste Abfrage gibt alle Werte aus der `Production.Product`-Tabelle zum Vergleich mit den Ergebnissen mit `INTERSECT` und `EXCEPT` zurück.  
  
```sql
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product ;  
--Result: 504 Rows  
```  
  
Die folgende Abfrage gibt alle eindeutigen Werte zurück, die von den Abfragen auf der linken und rechten Seite des `INTERSECT`-Operators zurückgegeben werden.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product  
INTERSECT  
SELECT ProductID   
FROM Production.WorkOrder ;  
--Result: 238 Rows (products that have work orders)  
```  
  
Die folgende Abfrage gibt alle eindeutigen Werte aus der Abfrage links vom `EXCEPT`-Operator zurück, die nicht auch in der rechten Abfrage gefunden werden.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product  
EXCEPT  
SELECT ProductID   
FROM Production.WorkOrder ;  
--Result: 266 Rows (products without work orders)  
```  
  
Die folgende Abfrage gibt alle eindeutigen Werte aus der Abfrage links vom `EXCEPT`-Operator zurück, die nicht auch in der rechten Abfrage gefunden werden. Die Tabellen sind die Umkehrung des vorherigen Beispiels.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.WorkOrder  
EXCEPT  
SELECT ProductID   
FROM Production.Product ;  
--Result: 0 Rows (work orders without products)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
In den folgenden Beispielen wird die Verwendung der Operatoren `INTERSECT` und `EXCEPT` veranschaulicht. Die erste Abfrage gibt alle Werte aus der `FactInternetSales`-Tabelle zum Vergleich mit den Ergebnissen mit `INTERSECT` und `EXCEPT` zurück.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales;  
--Result: 60398 Rows  
```  
  
Die folgende Abfrage gibt alle eindeutigen Werte zurück, die von den Abfragen auf der linken und rechten Seite des `INTERSECT`-Operators zurückgegeben werden.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
INTERSECT   
SELECT CustomerKey   
FROM DimCustomer   
WHERE DimCustomer.Gender = 'F'  
ORDER BY CustomerKey;  
--Result: 9133 Rows (Sales to customers that are female.)  
```  
  
Die folgende Abfrage gibt alle eindeutigen Werte aus der Abfrage links vom `EXCEPT`-Operator zurück, die nicht auch in der rechten Abfrage gefunden werden.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
EXCEPT   
SELECT CustomerKey   
FROM DimCustomer   
WHERE DimCustomer.Gender = 'F'  
ORDER BY CustomerKey;  
--Result: 9351 Rows (Sales to customers that are not female.)  
```  
