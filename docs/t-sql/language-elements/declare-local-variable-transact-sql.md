---
title: DECLARE @local_variable (Transact-SQL) | Microsoft-Dokumentation
description: Transact-SQL-Referenz für die Verwendung der DECLARE-Anweisung, um lokale Variablen für die Verwendung in einem Batch oder einer Prozedur zu definieren.
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECLARE
- DECLARE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table-valued parameters
- variables [SQL Server], declaring
- DECLARE statement
- declaring variables
ms.assetid: d1635ebb-f751-4de1-8bbc-cae161f90821
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 46551c65a37fb4e2c5b98806835f9b0b0fb3701d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347564"
---
# <a name="declare-local_variable-transact-sql"></a>DECLARE @local_variable (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Variablen werden im Hauptteil eines Batchs oder einer Prozedur mit einer DECLARE-Anweisung deklariert. Die Werte werden mithilfe einer SET- oder SELECT-Anweisung zugewiesen. Cursorvariablen können mit dieser Anweisung deklariert und mit anderen cursorspezifischen Anweisungen verwendet werden. Nach der Deklaration werden alle Variablen mit NULL initialisiert, es sei denn, ein Wert wurde als Teil der Deklaration angegeben.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database  
  
DECLARE   
{   
    { @local_variable [AS] data_type  [ = value ] }  
  | { @cursor_variable_name CURSOR }  
} [,...n]   
| { @table_variable_name [AS] <table_type_definition> }   
  
<table_type_definition> ::=   
     TABLE ( { <column_definition> | <table_constraint> } [ ,...n] )   
  
<column_definition> ::=   
     column_name { scalar_data_type | AS computed_column_expression }  
     [ COLLATE collation_name ]   
     [ [ DEFAULT constant_expression ] | IDENTITY [ (seed ,increment ) ] ]   
     [ ROWGUIDCOL ]   
     [ <column_constraint> ]   
  
<column_constraint> ::=   
     { [ NULL | NOT NULL ]   
     | [ PRIMARY KEY | UNIQUE ]   
     | CHECK ( logical_expression )   
     | WITH ( <index_option > )  
     }   
  
<table_constraint> ::=   
     { { PRIMARY KEY | UNIQUE } ( column_name [ ,...n] )   
     | CHECK ( search_condition )   
     }   
  
<index_option> ::=  
See CREATE TABLE for index option syntax.  
  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DECLARE   
{{ @local_variable [AS] data_type } [ =value [ COLLATE <collation_name> ] ] } [,...n]  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
@*local_variable*  
 Der Name einer Variablen. Variablennamen müssen mit einem at-Zeichen (@) beginnen. Die Namen lokaler Variablen müssen den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen.  
  
*data_type*  
 Ein vom System bereitgestellter, CLR-benutzerdefinierter Tabellentyp (Common Language Runtime) oder Aliasdatentyp. Eine Variable kann nicht den Datentyp **text**, **ntext** oder **image** haben.  
  
 Weitere Informationen zu Systemdatentypen finden Sie unter [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md). Weitere Informationen zu CLR-benutzerdefinierten Typen oder Aliasdatentypen finden Sie unter [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md).  
  
 =*value*  
 Weist der Variablen inline einen Wert zu. Der Wert kann eine Konstante oder ein Ausdruck sein; auf jeden Fall muss er mit dem Typ der Variablendeklaration übereinstimmen oder implizit in diesen Typ konvertiert werden können. Weitere Informationen finden Sie unter [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).  
  
@*cursor_variable_name*  
 Der Name einer Cursorvariablen. Cursorvariablennamen müssen mit einem at-Zeichen (@) beginnen und den Regeln für Bezeichner entsprechen.  
  
CURSOR  
 Gibt an, dass die Variable eine lokale Cursorvariable ist.  
  
@*table_variable_name*  
 Der Name einer Variablen vom Typ **table**. Variablennamen müssen mit einem at-Zeichen (@) beginnen und den Regeln für Bezeichner entsprechen.  
  
<table_type_definition>  
Definiert den Datentyp **table**. Die Tabellendeklaration schließt Spaltendefinitionen, Namen, Datentypen und Einschränkungen ein. Die einzigen zulässigen Einschränkungstypen sind PRIMARY KEY, UNIQUE, NULL und CHECK. Ein Aliasdatentyp kann nicht als Skalar-Spaltendatentyp verwendet werden, wenn eine Regel oder Standarddefinition an den Typ gebunden ist.
  
\<table_type_definiton> Ist eine Teilmenge von Informationen, die zum Definieren einer Tabelle in CREATE TABLE verwendet werden. Darin sind Elemente und wichtige Definitionen eingeschlossen. Weitere Informationen finden Sie unter [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 *n*  
 Ein Platzhalter, der angibt, dass mehrere Variablen angegeben und ihnen Werte zugewiesen werden können. Beim Deklarieren von **table**-Variablen muss die **table**-Variable die einzige Variable sein, die in der DECLARE-Anweisung deklariert wird.  
  
 *column_name*  
 Der Name der Spalte in der Tabelle.  
  
 *scalar_data_type*  
 Gibt an, dass die Spalte ein skalarer Datentyp ist.  
  
 *computed_column_expression*  
 Ein Ausdruck, der den Wert einer berechneten Spalte definiert. Sie wird mithilfe anderer Spalten derselben Tabelle mit einem Ausdruck berechnet. Eine berechnete Spalte kann beispielsweise die Definition **cost** AS **price \* qty** aufweisen. Der Ausdruck kann der Name einer nicht berechneten Spalte, eine Konstante, eine integrierte Funktion, eine Variable oder eine beliebige durch einen oder mehrere Operatoren verbundene Kombination der genannten Möglichkeiten sein. Der Ausdruck kann keine Unterabfrage oder benutzerdefinierte Funktion sein. Der Ausdruck kann nicht auf einen CLR-benutzerdefinierten Typ verweisen.  
  
 [ COLLATE *collation_name*]  
 Gibt die Sortierung für die Spalte an. Als *collation_name* kann entweder der Name einer Windows-Sortierung oder ein SQL-Sortierungsname verwendet werden. Dieses Element gilt nur für Spalten der Datentypen **char**, **varchar**, **text**, **nchar**, **nvarchar** und **ntext**. Wenn collation_name nicht angegeben ist, wird der Spalte die Sortierung des benutzerdefinierten Datentyps zugewiesen, wenn es sich um eine Spalte von einem benutzerdefinierten Datentyp handelt, oder es wird die Sortierung der aktuellen Datenbank zugewiesen.  
  
 Weitere Informationen zu den Windows- und SQL-Sortierungsnamen finden Sie unter [COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md).  
  
 DEFAULT  
 Gibt den Wert an, der für die Spalte bereitgestellt wird, wenn kein Wert explizit angegeben wurde. DEFAULT-Definitionen können auf alle Spalten angewendet werden, mit Ausnahme der als **timestamp** definierten Spalten sowie von Spalten mit der IDENTITY-Eigenschaft. DEFAULT-Definitionen werden entfernt, wenn die Tabelle gelöscht wird. Es kann nur ein konstanter Wert wie eine Zeichenfolge, eine Systemfunktion, z. B. SYSTEM_USER(), oder NULL als Standardwert verwendet werden. Um die Kompatibilität mit früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aufrechtzuerhalten, ist es möglich, einer DEFAULT-Definition einen Einschränkungsnamen zuzuweisen.  
  
 *constant_expression*  
 Eine Konstante, ein NULL-Wert oder eine Systemfunktion, die bzw. der als Standardwert für die Spalte verwendet wird.  
  
 IDENTITY  
 Gibt an, dass es sich bei der neuen Spalte um eine Identitätsspalte handelt. Wenn der Tabelle eine neue Zeile hinzugefügt wird, stellt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen eindeutigen, inkrementellen Wert für die Spalte bereit. Identitätsspalten werden in der Regel in Verbindung mit PRIMARY KEY-Einschränkungen verwendet, um als eindeutiger Zeilenbezeichner für die Tabelle zu dienen. Die IDENTITY-Eigenschaft kann folgenden Spalten zugewiesen werden: **tinyint**, **smallint**, **int**, **decimal(p,0)** oder **numeric(p,0)** . Es kann nur eine Identitätsspalte pro Tabelle erstellt werden. Gebundene Standardwerte und DEFAULT-Einschränkungen können nicht mit einer Identitätsspalte verwendet werden. Sie müssen entweder den Ausgangswert und den Schrittweitenwert oder keinen von beiden angeben. Wurden Ausgangswert und inkrementeller Wert nicht angegeben, ist der Standardwert (1,1).  
  
 *seed*  
 Der Wert, der für die erste in die Tabelle geladene Zeile verwendet wird.  
  
 *increment*  
 Der Schrittweitenwert, der zum Identitätswert der zuvor geladenen Zeile addiert wird.  
  
 ROWGUIDCOL  
 Gibt an, dass die neue Spalte eine Spalte mit für alle Zeilen global eindeutigen Bezeichnern ist. Nur eine **uniqueidentifier**-Spalte pro Tabelle kann als ROWGUIDCOL-Spalte gekennzeichnet werden. Die ROWGUIDCOL-Eigenschaft kann nur einer **uniqueidentifier**-Spalte zugewiesen werden.  
  
 NULL | NOT NULL  
 Gibt an, ob NULL in der Variablen zulässig ist. Die Standardeinstellung ist NULL.  
  
 PRIMARY KEY  
 Eine Einschränkung, die Entitätsintegrität für eine bestimmte Spalte (oder Spalten) durch einen eindeutigen Index erzwingt. Es kann nur eine PRIMARY KEY-Einschränkung pro Tabelle erstellt werden.  
  
 UNIQUE  
 Eine Einschränkung, die Entitätsintegrität für eine bestimmte Spalte (oder Spalten) durch einen eindeutigen Index bereitstellt. Eine Tabelle kann mehrere UNIQUE-Einschränkungen haben.  
  
 CHECK  
 Eine Einschränkung, die Domänenintegrität erzwingt, indem die möglichen Eingabewerte für eine oder mehrere Spalten beschränkt wird.  
  
 *logical_expression*  
 Ein logischer Ausdruck, der TRUE oder FALSE zurückgibt.  
  
## <a name="remarks"></a>Bemerkungen  
 Variablen werden oft in einem Batch oder einer Prozedur als Zähler für WHILE, LOOP oder für IF...ELSE-Blöcke verwendet.  
  
 Variablen können nur in Ausdrücken verwendet werden und stellen keinen Ersatz für Objektnamen oder Schlüsselwörter dar. Um dynamische SQL-Anweisungen zu erstellen, verwenden Sie EXECUTE.  
  
 Der Gültigkeitsbereich einer lokalen Variablen ist der Batch, in dem sie deklariert ist.  
 
 Eine Tabellenvariable ist nicht zwingend speicherresident. Wenn nicht genügend Speicherplatz vorhanden ist, können die Seiten, die einer Tabellenvariablen zugehörig sind, an tempdb gesendet werden.
  
 Auf eine Cursorvariable, der aktuell ein Cursor zugewiesen ist, kann in folgenden Anweisungen als Quelle verwiesen werden:  
  
-   CLOSE-Anweisung.  
  
-   DEALLOCATE-Anweisung.  
  
-   FETCH-Anweisung.  
  
-   OPEN-Anweisung.  
  
-   Positionierte DELETE- oder UPDATE-Anweisung.  
  
-   SET CURSOR VARIABLE-Anweisung (auf der rechten Seite).  
  
 Bei all diesen Anweisungen wird von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ein Fehler ausgelöst, wenn eine Cursorvariable vorhanden ist, auf die verwiesen wird, für die aber aktuell kein Cursor zugeordnet ist. Ist keine Cursorvariable vorhanden, auf die verwiesen wird, wird von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] der gleiche Fehler ausgelöst wie für eine nicht deklarierte Variable eines anderen Typs.  
  
 Eine Cursorvariable hat folgende Eigenschaften:  
  
-   Sie kann das Ziel eines Cursortyps oder einer anderen Cursorvariablen sein. Weitere Informationen finden Sie unter [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md).  
  
-   Auf sie kann als Ziel eines Ausgabecursorparameters in einer EXECUTE-Anweisung verwiesen werden, wenn der Cursorvariablen aktuell kein Cursor zugewiesen ist.  
  
-   Sie sollte als Zeiger auf den Cursor verstanden werden.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-declare"></a>A. Verwenden von DECLARE  
 Im folgenden Beispiel werden mithilfe der lokalen Variablen `@find` Kontaktinformationen für alle Nachnamen abgerufen, die mit `Man` beginnen.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @find VARCHAR(30);   
/* Also allowed:   
DECLARE @find VARCHAR(30) = 'Man%';   
*/  
SET @find = 'Man%';   
SELECT p.LastName, p.FirstName, ph.PhoneNumber  
FROM Person.Person AS p   
JOIN Person.PersonPhone AS ph ON p.BusinessEntityID = ph.BusinessEntityID  
WHERE LastName LIKE @find;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName            FirstName               Phone
------------------- ----------------------- -------------------------
Manchepalli         Ajay                    1 (11) 500 555-0174
Manek               Parul                   1 (11) 500 555-0146
Manzanares          Tomas                   1 (11) 500 555-0178
  
(3 row(s) affected)
```  
  
### <a name="b-using-declare-with-two-variables"></a>B. Verwenden von DECLARE mit zwei Variablen  
 Im folgenden Beispiel werden die Namen von Vertriebsmitarbeitern von [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] abgerufen, die in der Vertriebsregion Nordamerika tätig sind und im laufenden Jahr bereits einen Umsatz von mindestens 2.000.000 USD erzielt haben.  
  
```sql  
USE AdventureWorks2012;  
GO  
SET NOCOUNT ON;  
GO  
DECLARE @Group nvarchar(50), @Sales MONEY;  
SET @Group = N'North America';  
SET @Sales = 2000000;  
SET NOCOUNT OFF;  
SELECT FirstName, LastName, SalesYTD  
FROM Sales.vSalesPerson  
WHERE TerritoryGroup = @Group and SalesYTD >= @Sales;  
```  
  
### <a name="c-declaring-a-variable-of-type-table"></a>C. Deklarieren einer Variablen vom Typ "table"  
 Im folgenden Beispiel wird eine `table`-Variable erstellt, die die in der OUTPUT-Klausel der UPDATE-Anweisung angegebenen Werte speichert. Es folgen zwei `SELECT`-Anweisungen, die die Werte in `@MyTableVar` und die Ergebnisse des Updatevorgangs in der `Employee`-Tabelle zurückgeben. Beachten Sie, dass sich die Ergebnisse in der `INSERTED.ModifiedDate`-Spalte von den Werten in der `ModifiedDate`-Spalte in der `Employee`-Tabelle unterscheiden. Der Grund dafür ist, dass der `AFTER UPDATE`-Trigger, der den Wert von `ModifiedDate` auf das aktuelle Datum aktualisiert, in der `Employee`-Tabelle definiert wird. Die von `OUTPUT` zurückgegebenen Spalten spiegeln jedoch die Daten wider, bevor Trigger ausgelöst werden. Weitere Informationen finden Sie unter [OUTPUT-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar TABLE(  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    ModifiedDate DATETIME);  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25   
OUTPUT INSERTED.BusinessEntityID,  
       DELETED.VacationHours,  
       INSERTED.VacationHours,  
       INSERTED.ModifiedDate  
INTO @MyTableVar;  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours, ModifiedDate  
FROM @MyTableVar;  
GO  
--Display the result set of the table.  
--Note that ModifiedDate reflects the value generated by an  
--AFTER UPDATE trigger.  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO  
```  
  
### <a name="d-declaring-a-variable-of-user-defined-table-type"></a>D: Deklarieren einer Variablen vom Typ 'user-defined table'  
 Im folgenden Beispiel wird ein Tabellenwertparameter oder eine Tabellenvariable mit dem Namen `@LocationTVP` erstellt. Dies erfordert einen entsprechenden benutzerdefinierten Tabellentyp mit dem Namen `LocationTableType`. Weitere Informationen zum Erstellen eines benutzerdefinierten Tabellentyps finden Sie unter [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md). Weitere Informationen zu Tabellenwertparametern finden Sie unter [Use Table-Valued Parameters &#40;Database Engine&#41; (Verwenden von Tabellenwertparametern &#40;Datenbank-Engine&#41;)](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
```sql  
DECLARE @LocationTVP   
AS LocationTableType;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-declare"></a>E. Verwenden von DECLARE  
 Im folgenden Beispiel werden mithilfe der lokalen Variablen `@find` Kontaktinformationen für alle Nachnamen abgerufen, die mit `Walt` beginnen.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @find VARCHAR(30);  
/* Also allowed:   
DECLARE @find VARCHAR(30) = 'Man%';  
*/  
SET @find = 'Walt%';  
  
SELECT LastName, FirstName, Phone  
FROM DimEmployee   
WHERE LastName LIKE @find;  
```  
  
### <a name="f-using-declare-with-two-variables"></a>F. Verwenden von DECLARE mit zwei Variablen  
 Das folgende Beispiel ruft Variablen ab, um die Vor- und Nachnamen von Mitarbeitern in der `DimEmployee`-Tabelle anzugeben.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @lastName VARCHAR(30), @firstName VARCHAR(30);  
  
SET @lastName = 'Walt%';  
SET @firstName = 'Bryan';  
  
SELECT LastName, FirstName, Phone  
FROM DimEmployee   
WHERE LastName LIKE @lastName AND FirstName LIKE @firstName;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [Integrierte Funktionen &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [table &#40;Transact-SQL&#41;](../../t-sql/data-types/table-transact-sql.md)   
 [Vergleichen von typisiertem XML mit nicht typisiertem XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)  
  
  




