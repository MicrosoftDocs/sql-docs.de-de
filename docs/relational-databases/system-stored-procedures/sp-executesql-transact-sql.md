---
description: sp_executesql (Transact-SQL)
title: sp_executesql (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_executesql
- sp_executesql_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_executesql
- dynamic SQL
ms.assetid: a8d68d72-0f4d-4ecb-ae86-1235b962f646
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6eac2107c22781c278e173992d8994fc68fea981
ms.sourcegitcommit: a5398f107599102af7c8cda815d8e5e9a367ce7e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "92005756"
---
# <a name="sp_executesql-transact-sql"></a>sp_executesql (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Führt eine [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung oder einen -Batch aus, die bzw. der mehrfach wiederverwendet werden kann oder dynamisch erstellt wurde. Die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung oder der -Batch können eingebettete Parameter enthalten.  
  
> [!IMPORTANT]  
>  Durch zur Laufzeit kompilierte [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen können Anwendungen böswilligen Angriffen ausgesetzt werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_executesql [ @stmt = ] statement  
[   
  { , [ @params = ] N'@parameter_name data_type [ OUT | OUTPUT ][ ,...n ]' }   
     { , [ @param1 = ] 'value1' [ ,...n ] }  
]  
```  
  
## <a name="arguments"></a>Argumente  
 [ \@ stmt =]- *Anweisung*  
 Eine Unicode-Zeichenfolge, die eine- [!INCLUDE[tsql](../../includes/tsql-md.md)] Anweisung oder einen-Batch enthält. \@stmt muss entweder eine Unicode-Konstante oder eine Unicode-Variable sein. Komplexere Unicodeausdrücke, wie z. B. die Verkettung von zwei Zeichenfolgen mit dem +-Operator, sind nicht zulässig. Zeichenkonstanten sind nicht zulässig. Wenn eine Unicode-Konstante angegeben wird, muss Ihr ein **N**vorangestellt werden. Beispielsweise ist die Unicode-Konstante **N ' sp_who '** gültig, aber die Zeichen Konstante **' sp_who '** ist nicht. Die Länge der Zeichenfolge wird nur durch den verfügbaren Arbeitsspeicher des Datenbankservers begrenzt. Auf 64-Bit-Servern ist die Größe der Zeichenfolge auf 2 GB beschränkt, die maximale Größe von **nvarchar (max)**.  
  
> [!NOTE]  
>  \@stmt kann Parameter enthalten, die dieselbe Form wie ein Variablenname aufweisen, z. b.: `N'SELECT * FROM HumanResources.Employee WHERE EmployeeID = @IDParameter'`  
  
 Jeder in \@ stmt enthaltene Parameter muss über einen entsprechenden Eintrag in der \@ Parameter Definitionsliste params und in der Parameterwerte Liste verfügen.  
  
 [Parameter \@ =] N ' \@ *parameter_name* *data_type* [,... *n* ] '  
 Ist eine Zeichenfolge, die die Definitionen aller in stmt eingebetteten Parameter enthält \@ . Die Zeichenfolge muss entweder eine Unicode-Konstante oder eine Unicode-Variable sein. Jede Parameterdefinition besteht aus einem Parameternamen und einem Datentyp. *n* ist ein Platzhalter, der zusätzliche Parameter Definitionen angibt. Jeder in \@ stmt angegebene Parameter muss in \@ params definiert werden. Wenn die [!INCLUDE[tsql](../../includes/tsql-md.md)] Anweisung oder der Batch in \@ stmt keine Parameter enthält, \@ ist kein Parameter erforderlich. Der Standardwert für diesen Parameter ist NULL.  
  
 [ \@ param1 =] '*value1*'  
 Der Wert für den ersten Parameter, der in der Parameterzeichenfolge definiert ist. Bei diesem Wert kann es sich um eine Unicode-Konstante oder eine Unicode-Variable handeln. Für jeden Parameter, der in stmt enthalten ist, muss ein Parameterwert angegeben werden \@ . Die Werte sind nicht erforderlich, wenn die [!INCLUDE[tsql](../../includes/tsql-md.md)] Anweisung oder der Batch in \@ stmt keine Parameter enthält.  
  
 [ OUT | OUTPUT ]  
 Gibt an, dass es sich bei dem Parameter um einen Ausgabeparameter handelt. die Parameter " **Text**", " **ntext**" und " **Image** " können als Ausgabeparameter verwendet werden, es sei denn, die Prozedur ist eine Common Language Runtime (CLR)-Prozedur Ein Ausgabeparameter, der das Schlüsselwort OUTPUT verwendet, kann ein Cursorplatzhalter sein, es sei denn, bei der Prozedur handelt es sich um eine CLR-Prozedur (Common Language Runtime).  
  
 *n*  
 Ein Platzhalter für die Werte zusätzlicher Parameter. Werte können nur Konstanten oder Variablen sein. Werte können keine komplexeren Ausdrücke sein, wie z. B. Funktionen oder Ausdrücke, die mithilfe von Operatoren erstellt werden.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 0 (Erfolg) oder ungleich 0 (Fehler)  
  
## <a name="result-sets"></a>Resultsets  
 Gibt die Resultsets von allen SQL-Anweisungen der SQL-Zeichenfolge zurück.  
  
## <a name="remarks"></a>Bemerkungen  
 sp_executesql Parameter müssen in der jeweiligen Reihenfolge eingegeben werden, wie im Abschnitt "Syntax" weiter oben in diesem Thema beschrieben. Wenn die Parameter nicht in der vorgegebenen Reihenfolge eingegeben werden, wird eine Fehlermeldung ausgegeben.  
  
 sp_executesql verhält sich hinsichtlich Batches, Namensbereichen und Datenbankkontext wie EXECUTE. Die- [!INCLUDE[tsql](../../includes/tsql-md.md)] Anweisung oder der-Batch im sp_executesql \@ stmt-Parameter wird erst kompiliert, wenn die sp_executesql-Anweisung ausgeführt wird. Der Inhalt von \@ stmt wird dann kompiliert und als Ausführungsplan ausgeführt, der separat vom Ausführungsplan des Batches ist, der sp_executesql aufgerufen hat. Der sp_executesql-Batch kann nicht auf Variablen verweisen, die in dem Batch deklariert werden, der sp_executesql aufruft. Lokale Cursor oder Variablen im sp_executesql-Batch sind für den Batch, der sp_executesql aufruft, nicht sichtbar. Änderungen am Datenbankkontext sind nur bis zum Ende der sp_executesql-Anweisung vorhanden.  
  
 sp_executesql kann anstelle von gespeicherten Prozeduren verwendet werden, um eine [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung mehrere Male auszuführen, wenn sich nur die Parameterwerte in der Anweisung ändern. Da die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung selbst unverändert bleibt und nur die Parameterwerte geändert werden, wird der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abfrageoptimierer wahrscheinlich den Ausführungsplan wiederverwenden, der für die erste Ausführung erstellt wird.  
  
> [!NOTE]  
>  Verwenden Sie zur Verbesserung der Leistung vollqualifizierte Objektnamen in der Anweisungszeichenfolge.  
  
 sp_executesql unterstützt das von der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Zeichenfolge getrennte Festlegen von Parameterwerten, wie dem folgenden Beispiel zu entnehmen ist.  
  
```sql  
DECLARE @IntVariable INT;  
DECLARE @SQLString NVARCHAR(500);  
DECLARE @ParmDefinition NVARCHAR(500);  
  
/* Build the SQL string one time.*/  
SET @SQLString =  
     N'SELECT BusinessEntityID, NationalIDNumber, JobTitle, LoginID  
       FROM AdventureWorks2012.HumanResources.Employee   
       WHERE BusinessEntityID = @BusinessEntityID';  
SET @ParmDefinition = N'@BusinessEntityID tinyint';  
/* Execute the string with the first parameter value. */  
SET @IntVariable = 197;  
EXECUTE sp_executesql @SQLString, @ParmDefinition,  
                      @BusinessEntityID = @IntVariable;  
/* Execute the same string with the second parameter value. */  
SET @IntVariable = 109;  
EXECUTE sp_executesql @SQLString, @ParmDefinition,  
                      @BusinessEntityID = @IntVariable;  
```  
  
 Die Verwendung von Ausgabeparametern mit sp_executesql ist ebenfalls möglich. Im folgenden Beispiel wird eine Berufsbezeichnung aus der `AdventureWorks2012.HumanResources.Employee`-Tabelle abgerufen und im Ausgabeparameter `@max_title` zurückgegeben.  
  
```sql  
DECLARE @IntVariable INT;  
DECLARE @SQLString NVARCHAR(500);  
DECLARE @ParmDefinition NVARCHAR(500);  
DECLARE @max_title VARCHAR(30);  
  
SET @IntVariable = 197;  
SET @SQLString = N'SELECT @max_titleOUT = max(JobTitle)   
   FROM AdventureWorks2012.HumanResources.Employee  
   WHERE BusinessEntityID = @level';  
SET @ParmDefinition = N'@level TINYINT, @max_titleOUT VARCHAR(30) OUTPUT';  
  
EXECUTE sp_executesql @SQLString, @ParmDefinition, @level = @IntVariable, @max_titleOUT=@max_title OUTPUT;  
SELECT @max_title;  
```  
  
 Die Möglichkeit, Parameter in sp_executesql zu ersetzen, bietet die folgenden Vorteile gegenüber der Verwendung der EXECUTE-Anweisung, um eine Zeichenfolge auszuführen:  
  
-   Da sich der tatsächliche Text der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung in der sp_executesql-Zeichenfolge für die verschiedenen Ausführungen nicht ändert, findet der Abfrageoptimierer wahrscheinlich für die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung bei der zweiten Ausführung die Übereinstimmung mit dem Ausführungsplan, der für die erste Ausführung generiert wurde. Deshalb muss [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die zweite Anweisung nicht kompilieren.  
  
-   Die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Zeichenfolge wird nur einmal erstellt.  
  
-   Der integer-Parameter wird im systemeigenen Format angegeben. Die Konvertierung in Unicode ist nicht erforderlich.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der public-Rolle.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-executing-a-simple-select-statement"></a>A. Ausführen einer einfachen SELECT-Anweisung  
 In diesem Beispiel wird eine einfache `SELECT`-Anweisung erstellt und ausgeführt, die den eingebetteten Parameter `@level` enthält.  
  
```sql  
EXECUTE sp_executesql   
          N'SELECT * FROM AdventureWorks2012.HumanResources.Employee   
          WHERE BusinessEntityID = @level',  
          N'@level TINYINT',  
          @level = 109;  
```  
  
### <a name="b-executing-a-dynamically-built-string"></a>B. Ausführen einer dynamisch erstellten Zeichenfolge  
 In folgenden Beispiel wird veranschaulicht, wie mithilfe von `sp_executesql` eine dynamisch erstellte Zeichenfolge ausgeführt wird. Mit der gespeicherten Prozedur im Beispiel werden Daten in mehrere Tabellen eingefügt, die zum Partitionieren der Jahresverkaufszahlen verwendet werden. Für jeden Monat des Jahres ist eine Tabelle mit dem folgenden Format vorhanden:  
  
```sql  
CREATE TABLE May1998Sales  
    (OrderID INT PRIMARY KEY,  
    CustomerID INT NOT NULL,  
    OrderDate  DATETIME NULL  
        CHECK (DATEPART(yy, OrderDate) = 1998),  
    OrderMonth INT  
        CHECK (OrderMonth = 5),  
    DeliveryDate DATETIME NULL,  
        CHECK (DATEPART(mm, OrderDate) = OrderMonth)  
    )  
```  
  
 Die gespeicherte Prozedur in diesem Beispiel erstellt eine `INSERT`-Anweisung dynamisch und führt sie aus, um neue Aufträge in die entsprechende Tabelle einzufügen. Im Beispiel wird das Bestelldatum verwendet, um den Namen der Tabelle zu erstellen, die die Daten enthalten soll. Anschließend wird dieser Name in eine `INSERT`-Anweisung integriert.  
  
> [!NOTE]  
>  Dies ist ein einfaches Beispiel für sp_executesql. Das Beispiel enthält keine Fehlerprüfung und keine Überprüfung etwaiger Geschäftsregeln, wie z. B. um sicherzustellen, dass Bestellnummern in den Tabellen nicht doppelt vorhanden sind.  
  
```sql  
CREATE PROCEDURE InsertSales @PrmOrderID INT, @PrmCustomerID INT,  
                 @PrmOrderDate DATETIME, @PrmDeliveryDate DATETIME  
AS  
DECLARE @InsertString NVARCHAR(500)  
DECLARE @OrderMonth INT  
  
-- Build the INSERT statement.  
SET @InsertString = 'INSERT INTO ' +  
       /* Build the name of the table. */  
       SUBSTRING( DATENAME(mm, @PrmOrderDate), 1, 3) +  
       CAST(DATEPART(yy, @PrmOrderDate) AS CHAR(4) ) +  
       'Sales' +  
       /* Build a VALUES clause. */  
       ' VALUES (@InsOrderID, @InsCustID, @InsOrdDate,' +  
       ' @InsOrdMonth, @InsDelDate)'  
  
/* Set the value to use for the order month because  
   functions are not allowed in the sp_executesql parameter  
   list. */  
SET @OrderMonth = DATEPART(mm, @PrmOrderDate)  
  
EXEC sp_executesql @InsertString,  
     N'@InsOrderID INT, @InsCustID INT, @InsOrdDate DATETIME,  
       @InsOrdMonth INT, @InsDelDate DATETIME',  
     @PrmOrderID, @PrmCustomerID, @PrmOrderDate,  
     @OrderMonth, @PrmDeliveryDate  
  
GO  
```  
  
 Das Verwenden von sp_executesql in dieser Prozedur ist effizienter als das Verwenden von EXECUTE zum Ausführen einer Zeichenfolge. Beim Verwenden von sp_executesql werden nur 12 Versionen der INSERT-Zeichenfolge generiert, nämlich eine Version für jede Monatstabelle. Mit EXECUTE ist jede INSERT-Zeichenfolge eindeutig, weil die Parameterwerte unterschiedlich sind. Obwohl beide Methoden die gleiche Anzahl von Batches generieren, ist es wegen der Ähnlichkeit der von sp_executesql generierten INSERT-Zeichenfolgen wahrscheinlicher, dass der Abfrageoptimierer die Ausführungspläne wiederverwendet.  
  
### <a name="c-using-the-output-parameter"></a>C. Verwenden des OUTPUT-Parameters  
 Im folgenden Beispiel wird ein- `OUTPUT` Parameter verwendet, um das von der- `SELECT` Anweisung im-Parameter generierte Resultset zu speichern `@SQLString` . `SELECT` Anschließend werden zwei-Anweisungen ausgeführt, die den Wert des- `OUTPUT` Parameters verwenden.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SQLString NVARCHAR(500);  
DECLARE @ParmDefinition NVARCHAR(500);  
DECLARE @SalesOrderNumber NVARCHAR(25);  
DECLARE @IntVariable INT;  
SET @SQLString = N'SELECT @SalesOrderOUT = MAX(SalesOrderNumber)  
    FROM Sales.SalesOrderHeader  
    WHERE CustomerID = @CustomerID';  
SET @ParmDefinition = N'@CustomerID INT,  
    @SalesOrderOUT NVARCHAR(25) OUTPUT';  
SET @IntVariable = 22276;  
EXECUTE sp_executesql  
    @SQLString  
    ,@ParmDefinition  
    ,@CustomerID = @IntVariable  
    ,@SalesOrderOUT = @SalesOrderNumber OUTPUT;  
-- This SELECT statement returns the value of the OUTPUT parameter.  
SELECT @SalesOrderNumber;  
-- This SELECT statement uses the value of the OUTPUT parameter in  
-- the WHERE clause.  
SELECT OrderDate, TotalDue  
FROM Sales.SalesOrderHeader  
WHERE SalesOrderNumber = @SalesOrderNumber;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-executing-a-simple-select-statement"></a>D: Ausführen einer einfachen SELECT-Anweisung  
 In diesem Beispiel wird eine einfache `SELECT`-Anweisung erstellt und ausgeführt, die den eingebetteten Parameter `@level` enthält.  
  
```sql  
-- Uses AdventureWorks  
  
EXECUTE sp_executesql   
          N'SELECT * FROM AdventureWorksPDW2012.dbo.DimEmployee   
          WHERE EmployeeKey = @level',  
          N'@level TINYINT',  
          @level = 109;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
