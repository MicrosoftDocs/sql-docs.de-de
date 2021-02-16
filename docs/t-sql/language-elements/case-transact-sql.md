---
title: CASE (Transact-SQL)
description: Transact-SQL-Referenz für den Ausdruck CASE Mit dem CASE-Ausdruck wird eine Liste von Bedingungen für die Rückgabe bestimmter Ergebnisse ausgewertet.
ms.date: 01/26/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CASE_TSQL
- CASE
dev_langs:
- TSQL
helpviewer_keywords:
- CASE expression [Transact-SQL]
- simple CASE expression
- comparing expressions
- searched CASE expression
ms.assetid: 658039ec-8dc2-4251-bc82-30ea23708cee
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5647234be5f758f1b4ff27f2f11e5338d67635da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352382"
---
# <a name="case-transact-sql"></a>CASE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Wertet eine Liste von Bedingungen aus und gibt einen von mehreren möglichen Ergebnisausdrücken zurück.  
  
 Der CASE-Ausdruck verfügt über zwei Formate:  
  
-   Der einfache CASE-Ausdruck vergleicht einen Ausdruck mit mehreren einfachen Ausdrücken, um das Ergebnis zu bestimmen.  
  
-   Der komplexe CASE-Ausdruck wertet eine Menge boolescher Ausdrücke aus, um das Ergebnis zu bestimmen.  
  
 Bei beiden Formaten wird ein optionales ELSE-Argument unterstützt.  
  
 CASE kann in einer beliebigen Anweisung oder Klausel verwendet werden, die einen gültigen Ausdruck zulässt. Beispielsweise können CASE-Anweisungen wie SELECT, UPDATE, DELETE und SET und Klauseln wie select_list, IN, WHERE, ORDER BY und HAVING verwendet werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server, Azure SQL Database and Azure Synapse Analytics
  
--Simple CASE expression:   
CASE input_expression   
     WHEN when_expression THEN result_expression [ ...n ]   
     [ ELSE else_result_expression ]   
END   

--Searched CASE expression:  
CASE  
     WHEN Boolean_expression THEN result_expression [ ...n ]   
     [ ELSE else_result_expression ]   
END  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
CASE  
     WHEN when_expression THEN result_expression [ ...n ]   
     [ ELSE else_result_expression ]   
END
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente  
 *input_expression*  
 Der Ausdruck, der ausgewertet wird, wenn das einfache CASE-Format verwendet wird. *input_expression* ist ein gültiger [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md).  
  
 WHEN *when_expression*  
 Ein einfacher Ausdruck, mit dem *input_expression* verglichen wird, wenn das einfache CASE-Format verwendet wird. *when_expression* ist ein gültiger Ausdruck. Die Datentypen von *input_expression* und allen *when_expression*-Ausdrücken müssen gleich sein, oder es muss eine implizite Konvertierung vorliegen.  
  
 THEN *result_expression*  
 Der Ausdruck, der zurückgegeben wird, wenn *input_expression* gleich *when_expression* TRUE ergibt oder wenn *boolean_expression* TRUE ergibt. *result expression* ist ein gültiger [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md).  
  
 ELSE *else_result_expression*  
 Der Ausdruck, der zurückgegeben wird, wenn keine Vergleichsoperation TRUE ergibt. Wenn dieses Argument weggelassen wird und keine Vergleichsoperation als TRUE ausgewertet wird, gibt die CASE-Funktion NULL zurück. *else_result_expression* ist ein gültiger Ausdruck. Die Datentypen von *else_result_expression* und allen *result_expression*-Ausdrücken müssen gleich sein, oder es muss eine implizite Konvertierung vorliegen.  
  
 WHEN *Boolean_expression*  
 Der boolesche Ausdruck, der ausgewertet wird, wenn das komplexe CASE-Format verwendet wird. *Boolean_expression* ist in gültiger boolescher Ausdruck.  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 Gibt den Typ mit höchsten Priorität in *result_expressions* und in dem optionalen *else_result_expression*-Ausdruck zurück. Weitere Informationen finden Sie unter [Rangfolge der Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
### <a name="return-values"></a>Rückgabewerte  
 **Einfacher CASE-Ausdruck:**  
  
 Beim einfachen CASE-Ausdruck wird verglichen, ob der erste Ausdruck mit dem Ausdruck in den einzelnen WHEN-Klauseln gleichwertig ist. Wenn diese Ausdrücke gleichwertig sind, wird der Ausdruck in der THEN-Klausel zurückgegeben.  
  
-   Lässt nur eine Gleichheitsüberprüfung zu.  
  
-   Wertet input_expression = when_expression in der angegebenen Reihenfolge für jede WHEN-Klausel aus.  
  
-   Gibt für die erste *input_expression* = *when_expression*, die TRUE ergibt, *result_expression* zurück.  
  
-   Wird kein *input_expression* = *when_expression*-Ausdruck TRUE ergibt, gibt [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] den *else_result_expression*-Ausdruck zurück, falls eine ELSE-Klausel angegeben ist, oder einen NULL-Wert, falls keine ELSE-Klausel angegeben ist.  
  
 **Komplexe CASE-Ausdrücke:**  
  
-   Für jede WHEN-Klausel werden die *Boolean_expression*-Ausdrücke in der angegebenen Reihenfolge ausgewertet.  
  
-   Gibt den *result_expression*-Ausdruck des ersten *Boolean_expression*-Ausdruck zurück, der TRUE ergibt.  
  
-   Wird kein *Boolean_expression*-Ausdruck zu TRUE ausgewertet, gibt [!INCLUDE[ssDE](../../includes/ssde-md.md)] den *else_result_expression*-Ausdruck zurück, falls eine ELSE-Klausel angegeben ist, oder einen NULL-Wert, falls keine ELSE-Klausel angegeben ist.  
  
## <a name="remarks"></a>Bemerkungen  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist für CASE-Ausdrücke nur eine Schachtelung von 10 Ebenen zulässig.  
  
 Der CASE-Ausdruck kann nicht verwendet werden, um den Ablauf bei der Ausführung von Transact-SQL-Anweisungen, Anweisungsblöcken, benutzerdefinierten Funktionen und gespeicherten Prozeduren zu steuern. Eine Liste der Methoden zur Ablaufsteuerung finden Sie unter [Control-of-Flow Language &#40;Transact-SQL&#41; (Sprachkonstrukte zur Ablaufsteuerung (Transact-SQL))](~/t-sql/language-elements/control-of-flow.md).  
  
 Der CASE-Ausdruck bewertet ihre Bedingungen sequenziell und hält bei der ersten Bedingung an, deren Bedingung erfüllt ist. In einigen Situationen wird ein Ausdruck bewertet, bevor ein CASE-Ausdruck die Ergebnisse des Ausdrucks als Eingabe empfängt. Bei der Bewertung dieser Ausdrücke sind Fehler möglich. Aggregierte Ausdrücke, die in WHEN-Argumenten für einen CASE-Ausdruck angezeigt werden, werden zuerst ausgewertet und dann dem CASE-Ausdruck bereitgestellt. Beim Erzeugen des MAX-Aggregat-Werts erzeugt die folgende Abfrage beispielsweise einen Fehler aufgrund einer Division durch Null. Dies erfolgt vor dem Auswerten des CASE-Ausdrucks.  
  
```sql  
WITH Data (value) AS   
(   
SELECT 0   
UNION ALL   
SELECT 1   
)   
SELECT   
   CASE   
      WHEN MIN(value) <= 0 THEN 0   
      WHEN MAX(1/value) >= 100 THEN 1   
   END   
FROM Data ;  
```  
  
 Sie sollten nur von der Reihenfolge der Bewertung der WHEN-Bedingungen für skalare Ausdrücke abhängig sein (einschließlich nicht korrelierter Unterabfragen, die Skalare zurückgeben), nicht für aggregierte Ausdrücke.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-a-select-statement-with-a-simple-case-expression"></a>A. Verwenden einer SELECT-Anweisung mit einem einfachen CASE-Ausdruck  
 Innerhalb einer `SELECT`-Anweisung ermöglicht ein einfacher `CASE`-Ausdruck nur eine Überprüfung auf Gleichheit. Andere Vergleiche werden nicht angestellt. Im folgenden Beispiel wird ein `CASE`-Ausdruck verwendet, um die Anzeige von Produktkategorien so zu ändern, dass sie leichter verständlich werden.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT   ProductNumber, Category =  
      CASE ProductLine  
         WHEN 'R' THEN 'Road'  
         WHEN 'M' THEN 'Mountain'  
         WHEN 'T' THEN 'Touring'  
         WHEN 'S' THEN 'Other sale items'  
         ELSE 'Not for sale'  
      END,  
   Name  
FROM Production.Product  
ORDER BY ProductNumber;  
GO  
```  
  
### <a name="b-using-a-select-statement-with-a-searched-case-expression"></a>B. Verwenden einer SELECT-Anweisung mit einem komplexen CASE-Ausdruck  
 Innerhalb einer `SELECT`-Anweisung können mit dem komplexen `CASE`-Ausdruck Werte im Resultset basierend auf den Vergleichsergebnissen ersetzt werden. Im folgenden Beispiel wird anstelle des Listenpreises ein Kommentar angezeigt, der vom Preisbereich der einzelnen Produkte abhängt.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT   ProductNumber, Name, "Price Range" =   
      CASE   
         WHEN ListPrice =  0 THEN 'Mfg item - not for resale'  
         WHEN ListPrice < 50 THEN 'Under $50'  
         WHEN ListPrice >= 50 and ListPrice < 250 THEN 'Under $250'  
         WHEN ListPrice >= 250 and ListPrice < 1000 THEN 'Under $1000'  
         ELSE 'Over $1000'  
      END  
FROM Production.Product  
ORDER BY ProductNumber ;  
GO  
```  
  
### <a name="c-using-case-in-an-order-by-clause"></a>C. Verwenden von CASE in einer ORDER BY-Klausel  
 In den folgenden Beispielen wird der CASE-Ausdruck in einer ORDER BY-Klausel verwendet, um die Sortierreihenfolge der Zeilen auf Grundlage eines angegebenen Spaltenwerts zu bestimmen. Im ersten Beispiel wird der Wert in der `SalariedFlag`-Spalte der `HumanResources.Employee`-Tabelle ausgewertet. Mitarbeiter, deren `SalariedFlag` auf 1 festgelegt wurde, werden nach `BusinessEntityID` in absteigender Folge zurückgegeben. Mitarbeiter, deren `SalariedFlag` auf 0 festgelegt wurde, werden nach `BusinessEntityID` in aufsteigender Folge zurückgegeben. Im zweiten Beispiel wird das Resultset nach der `TerritoryName`-Spalte sortiert, wenn die `CountryRegionName`-Spalte gleich 'United States' ist, und bei allen anderen Zeilen nach `CountryRegionName`.  
  
```sql  
SELECT BusinessEntityID, SalariedFlag  
FROM HumanResources.Employee  
ORDER BY CASE SalariedFlag WHEN 1 THEN BusinessEntityID END DESC  
        ,CASE WHEN SalariedFlag = 0 THEN BusinessEntityID END;  
GO    
```  
  
```sql  
SELECT BusinessEntityID, LastName, TerritoryName, CountryRegionName  
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL  
ORDER BY CASE CountryRegionName WHEN 'United States' THEN TerritoryName  
         ELSE CountryRegionName END;  
```  
  
### <a name="d-using-case-in-an-update-statement"></a>D: Verwenden von CASE in einer UPDATE-Anweisung  
 Im folgenden Beispiel wird der CASE-Ausdruck in einer UPDATE-Anweisung verwendet, um den Wert zu bestimmen, der für die `VacationHours`-Spalte für Mitarbeiter mit `SalariedFlag` gleich 0 festgelegt wurde. Wenn von `VacationHours` 10 Stunden subtrahiert werden, und dies einen negativen Wert ergibt, wird `VacationHours` um 40 Stunden erhöht; andernfalls wird `VacationHours` um 20 Stunden erhöht. Die OUTPUT-Klausel wird verwendet, um die Werte vor und nach dem Urlaub anzuzeigen.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE HumanResources.Employee  
SET VacationHours =   
    ( CASE  
         WHEN ((VacationHours - 10.00) < 0) THEN VacationHours + 40  
         ELSE (VacationHours + 20.00)  
       END  
    )  
OUTPUT Deleted.BusinessEntityID, Deleted.VacationHours AS BeforeValue,   
       Inserted.VacationHours AS AfterValue  
WHERE SalariedFlag = 0;   
```  
  
### <a name="e-using-case-in-a-set-statement"></a>E. Verwenden von CASE in einer SET-Anweisung  
 Im folgenden Beispiel wird der CASE-Ausdruck in einer SET-Anweisung in der Tabellenwertfunktion `dbo.GetContactInfo` verwendet. In der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank werden alle Daten zu Personen in der `Person.Person`-Tabelle gespeichert. Die Person kann z.B. ein Mitarbeiter, Herstellerkontakt oder Kunde sein. Die Funktion gibt den Vor- und Nachnamen einer bestimmten `BusinessEntityID` und den Kontakttyp für diese Person zurück. Der CASE-Ausdruck in der SET-Anweisung bestimmt den Wert, der für die `ContactType`-Spalte basierend auf dem Vorhandensein der `BusinessEntityID`-Spalte in der Tabelle `Employee`, `Vendor` oder `Customer` angezeigt werden soll.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE FUNCTION dbo.GetContactInformation(@BusinessEntityID INT)  
    RETURNS @retContactInformation TABLE   
(  
BusinessEntityID INT NOT NULL,  
FirstName NVARCHAR(50) NULL,  
LastName NVARCHAR(50) NULL,  
ContactType NVARCHAR(50) NULL,  
    PRIMARY KEY CLUSTERED (BusinessEntityID ASC)  
)   
AS   
-- Returns the first name, last name and contact type for the specified contact.  
BEGIN  
    DECLARE   
        @FirstName NVARCHAR(50),   
        @LastName NVARCHAR(50),   
        @ContactType NVARCHAR(50);  
  
    -- Get common contact information  
    SELECT   
        @BusinessEntityID = BusinessEntityID,   
@FirstName = FirstName,   
        @LastName = LastName  
    FROM Person.Person   
    WHERE BusinessEntityID = @BusinessEntityID;  
  
    SET @ContactType =   
        CASE   
            -- Check for employee  
            WHEN EXISTS(SELECT * FROM HumanResources.Employee AS e   
                WHERE e.BusinessEntityID = @BusinessEntityID)   
                THEN 'Employee'  
  
            -- Check for vendor  
            WHEN EXISTS(SELECT * FROM Person.BusinessEntityContact AS bec  
                WHERE bec.BusinessEntityID = @BusinessEntityID)   
                THEN 'Vendor'  
  
            -- Check for store  
            WHEN EXISTS(SELECT * FROM Purchasing.Vendor AS v            
                WHERE v.BusinessEntityID = @BusinessEntityID)   
                THEN 'Store Contact'  
  
            -- Check for individual consumer  
            WHEN EXISTS(SELECT * FROM Sales.Customer AS c   
                WHERE c.PersonID = @BusinessEntityID)   
                THEN 'Consumer'  
        END;  
  
    -- Return the information to the caller  
    IF @BusinessEntityID IS NOT NULL   
    BEGIN  
        INSERT @retContactInformation  
        SELECT @BusinessEntityID, @FirstName, @LastName, @ContactType;  
    END;  
  
    RETURN;  
END;  
GO  
  
SELECT BusinessEntityID, FirstName, LastName, ContactType  
FROM dbo.GetContactInformation(2200);  
GO  
SELECT BusinessEntityID, FirstName, LastName, ContactType  
FROM dbo.GetContactInformation(5);  
```  
  
### <a name="f-using-case-in-a-having-clause"></a>F. Verwenden von CASE in einer HAVING-Klausel  
 Im folgenden Beispiel wird der CASE-Ausdruck in einer HAVING-Klausel verwendet, um die von der SELECT-Anweisung zurückgegebenen Zeilen einzuschränken. Die Anweisung gibt den maximalen Stundensatz für jede Berufsbezeichnung in der `HumanResources.Employee`-Tabelle zurück. Die HAVING-Klausel beschränkt die Bezeichnungen auf diejenigen Positionen, die von Männern mit einem maximalen Stundensatz von über 40 Dollar bzw. von Frauen mit einem maximalen Stundensatz von über 42 Dollar besetzt werden.  
  
```sql 
USE AdventureWorks2012;  
GO  
SELECT JobTitle, MAX(ph1.Rate)AS MaximumRate  
FROM HumanResources.Employee AS e  
JOIN HumanResources.EmployeePayHistory AS ph1 ON e.BusinessEntityID = ph1.BusinessEntityID  
GROUP BY JobTitle  
HAVING (MAX(CASE WHEN Gender = 'M'   
        THEN ph1.Rate   
        ELSE NULL END) > 40.00  
     OR MAX(CASE WHEN Gender  = 'F'   
        THEN ph1.Rate    
        ELSE NULL END) > 42.00)  
ORDER BY MaximumRate DESC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="g-using-a-select-statement-with-a-case-expression"></a>G. Verwenden einer SELECT-Anweisung mit einem CASE-Ausdruck  
 Innerhalb einer SELECT-Anweisung können mit dem komplexen CASE-Ausdruck Werte im Resultset basierend auf den Vergleichsergebnissen ersetzt werden. Im folgenden Beispiel wird ein CASE-Ausdruck verwendet, um die Anzeige von Produktkategorien so zu ändern, dass sie leichter verständlich werden. Wenn ein Wert nicht vorhanden ist, wird der Text „Not for sale“ (Nicht zu verkaufen) angezeigt.  
  
```sql 
-- Uses AdventureWorks  
  
SELECT   ProductAlternateKey, Category =  
      CASE ProductLine  
         WHEN 'R' THEN 'Road'  
         WHEN 'M' THEN 'Mountain'  
         WHEN 'T' THEN 'Touring'  
         WHEN 'S' THEN 'Other sale items'  
         ELSE 'Not for sale'  
      END,  
   EnglishProductName  
FROM dbo.DimProduct  
ORDER BY ProductKey;  
```  
  
### <a name="h-using-case-in-an-update-statement"></a>H. Verwenden von CASE in einer UPDATE-Anweisung  
 Im folgenden Beispiel wird der CASE-Ausdruck in einer UPDATE-Anweisung verwendet, um den Wert zu bestimmen, der für die `VacationHours`-Spalte für Mitarbeiter mit `SalariedFlag` gleich 0 festgelegt wurde. Wenn von `VacationHours` 10 Stunden subtrahiert werden, und dies einen negativen Wert ergibt, wird `VacationHours` um 40 Stunden erhöht; andernfalls wird `VacationHours` um 20 Stunden erhöht.  
  
```sql 
-- Uses AdventureWorks   
  
UPDATE dbo.DimEmployee  
SET VacationHours =   
    ( CASE  
         WHEN ((VacationHours - 10.00) < 0) THEN VacationHours + 40  
         ELSE (VacationHours + 20.00)   
       END  
    )   
WHERE SalariedFlag = 0;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [COALESCE &#40;Transact-SQL&#41;](../../t-sql/language-elements/coalesce-transact-sql.md)   
 [IIF &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-iif-transact-sql.md)   
 [CHOOSE &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-choose-transact-sql.md)  
  
  



