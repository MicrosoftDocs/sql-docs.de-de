---
description: SELECT - INTO-Klausel (Transact-SQL)
title: INTO-Klausel (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- INTO_TSQL
- INSERT_INTO_TSQL
- INSERT INTO
- INTO
- INTO clause
- INTO_clause_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- copying data [SQL Server], into a new table
- INTO clause
- moving data, to a new table
- table creation [SQL Server], INTO clause
- SELECT INTO statement
- inserting rows
- clauses [SQL Server], INTO
- row additions [SQL Server], INTO clause
ms.assetid: b48d69e8-5a00-48bf-b2f3-19278a72dd88
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a12bbc7b4f8d4178217b09b0a3a08233ab6892c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347924"
---
# <a name="select---into-clause-transact-sql"></a>SELECT - INTO-Klausel (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Mit SELECT…INTO wird eine neue Tabelle in der Standarddateigruppe erstellt, und die Ergebniszeilen aus der Abfrage werden darin eingefügt. Die vollständige SELECT-Syntax finden Sie unter [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
[ INTO new_table ]
[ ON filegroup ]
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *new_table*   
 Gibt den Namen einer neuen Tabelle an, die mithilfe der Spalten in der Auswahlliste und der aus der Datenquelle ausgewählten Zeilen erstellt wird.  
 
 Das Format von *new_table* wird bestimmt, indem die Ausdrücke in der Auswahlliste ausgewertet werden. Die Spalten in *new_table* werden in der durch die Auswahlliste angegebenen Reihenfolge erstellt. Jede Spalte in *new_table* besitzt den gleichen Namen, Datentyp, NULL-Zulässigkeit und Wert wie der entsprechende Ausdruck in der Auswahlliste. Die IDENTITY-Eigenschaft einer Spalte wird übertragen. Dies gilt mit Ausnahme der unter "Arbeiten mit Identitätsspalten" im Abschnitt "Hinweise" angegebenen Bedingungen.  
  
 Um die Tabelle in einer anderen Datenbank für die gleiche Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu erstellen, geben Sie *new_table* als vollqualifizierten Namen in der Form *database.schema.table_name* an.  
  
 *new_table* kann nicht für einen Remoteserver erstellt werden; Sie können *new_table* jedoch anhand einer Remotedatenquelle auffüllen. Um *new_table* anhand einer Remotequelltabelle zu erstellen, geben Sie die Quelltabelle als vierteiligen Namen in der Form *linked_server*.*catalog*.*schema*.*object* in der FROM-Klausel der SELECT-Anweisung an. Alternativ können Sie die [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md)-Funktion oder die [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md)-Funktion in der FROM-Klausel verwenden, um die Remotedatenquelle anzugeben.  
 
 *filegroup*    
 Gibt den Namen der Dateigruppe an, in der die neue Tabelle erstellt wird. Die angegebene Dateigruppe muss in der Datenbank vorhanden sein, andernfalls löst die SQL Server-Engine einen Fehler aus.   
 
 **Anwendungsbereich:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und höher.
  
## <a name="data-types"></a>Datentypen  
 Beim FILESTREAM-Attribut werden keine Daten in die neue Tabelle übertragen. FILESTREAM-BLOBs werden kopiert und in der neuen Tabelle als **varbinary(max)** -BLOBs gespeichert. Ohne das FILESTREAM-Attribut verfügt der **varbinary(max)** -Datentyp über eine Einschränkung von 2 GB. Wenn ein FILESTREAM-BLOB diesen Wert überschreitet, wird Fehler 7119 ausgelöst, und die Anweisung wird beendet.  
  
 Bei der Auswahl einer vorhandenen Identitätsspalte in einer neuen Tabelle erbt die neue Spalte die IDENTITY-Eigenschaft, es sein denn, eine der folgenden Bedingungen trifft zu:  
  
-   Die SELECT-Anweisung enthält einen Join.  
  
-   Mehrere SELECT-Anweisungen sind mit UNION verknüpft.  
  
-   Die Identitätsspalte ist mehrfach in der Auswahlliste aufgeführt.  
  
-   Die Identitätsspalte ist Teil eines Ausdrucks.  
  
-   Die Identitätsspalte stammt aus einer Remotedatenquelle.  
  
Falls eine dieser Bedingungen erfüllt ist, wird die Spalte mit NOT NULL erstellt, anstatt die IDENTITY-Eigenschaft zu erben. Wenn eine Identitätsspalte in der neuen Tabelle erforderlich, aber nicht verfügbar ist oder wenn Sie einen Ausgangs- oder Inkrementwert benötigen, der sich von der Quellidentitätsspalte unterscheidet, definieren Sie die Spalte in der Auswahlliste mithilfe der IDENTITY-Funktion. Weitere Informationen finden Sie unter "Erstellen einer Identitätsspalte mithilfe der IDENTITY-Funktion" im Abschnitt mit den Beispielen unten.  

## <a name="remarks"></a>Bemerkungen  
Die `SELECT...INTO`-Anweisung wird in zwei Schritten ausgeführt: Zunächst wird die neue Tabelle erstellt, dann werden die Zeilen eingefügt.  Das bedeutet, dass ein Rollback ausgeführt wird, wenn die Einfügevorgänge fehlschlagen; die neue (leere) Tabelle bleibt jedoch bestehen.  Wenn der gesamte Vorgang als Ganzes erfolgreich sein oder fehlschlagen muss, verwenden Sie eine [explizite Transaktion](../language-elements/begin-transaction-transact-sql.md).
  
## <a name="limitations-and-restrictions"></a>Einschränkungen  
 Tabellenvariablen und Tabellenwertparameter können nicht als neue Tabelle angegeben werden.  
  
 Sie können mit `SELECT...INTO` keine partitionierte Tabelle erstellen, auch dann nicht, wenn die Quelltabelle partitioniert ist. Für `SELECT...INTO` wird nicht das Partitionsschema der Quelltabelle verwendet. Stattdessen wird die neue Tabelle in der Standarddateigruppe erstellt. Erstellen Sie zum Einfügen von Zeilen in eine partitionierte Tabelle zuerst die partitionierte Tabelle, und verwenden Sie anschließend die `INSERT INTO...SELECT...FROM`-Anweisung.  
  
 Indizes, Einschränkungen und Trigger, die in der Quelltabelle definiert wurden, werden nicht in die neue Tabelle übertragen. Sie können auch nicht in der `SELECT...INTO`-Anweisung angegeben werden. Wenn diese Objekte erforderlich sind, können Sie sie nach dem Ausführen der `SELECT...INTO`-Anweisung erstellen.  
  
 Die Angabe einer `ORDER BY`-Klausel gewährleistet nicht, dass die Zeilen in der angegebenen Reihenfolge eingefügt werden.  
  
 Wenn eine Sparsespalte in die Auswahlliste eingeschlossen ist, wird die Eigenschaft der Sparsespalte nicht an die neue Tabelle übertragen. Wenn diese Eigenschaft in der neuen Tabelle erforderlich ist, ändern Sie die Spaltendefinition, nachdem Sie die SELECT...INTO-Anweisung ausgeführt haben, um diese Eigenschaft einzuschließen.  
  
 Wenn eine berechnete Spalte in die Auswahlliste eingeschlossen ist, ist die entsprechende Spalte in der neuen Tabelle keine berechnete Spalte. Die Werte in der neuen Spalte entsprechen den Werten, die zum Zeitpunkt der Ausführung der `SELECT...INTO`-Anweisung berechnet wurden.  
  
## <a name="logging-behavior"></a>Protokollierungsverhalten  
 Der Grad der Protokollierung für `SELECT...INTO` hängt von dem Wiederherstellungsmodell ab, das für die Datenbank aktiv ist. Unter dem einfachen Wiederherstellungsmodell und dem massenprotokollierten Wiederherstellungsmodell werden Massenvorgänge minimal protokolliert. Bei minimaler Protokollierung kann es effizienter sein, die `SELECT...INTO`-Anweisung zu verwenden, anstatt eine Tabelle zu erstellen und diese dann mithilfe einer INSERT-Anweisung aufzufüllen. Weitere Informationen finden Sie unter [Das Transaktionsprotokoll &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).
 
`SELECT...INTO`-Anweisungen, die benutzerdefinierte Funktionen (UDFs) enthalten, sind vollständig protokollierte Vorgänge. Wenn die benutzerdefinierten Funktionen, die in der `SELECT...INTO`-Anweisung verwendet werden, keine Datenzugriffsvorgänge ausführen, können Sie die SCHEMABINDING-Klausel für die benutzerdefinierten Funktionen angeben. Dadurch wird die abgeleitete UserDataAccess-Eigenschaft für diese benutzerdefinierten Funktionen auf 0 (Null) festgelegt. Nach dieser Änderung werden `SELECT...INTO`-Anweisungen minimal protokolliert. Wenn die `SELECT...INTO`-Anweisung weiterhin auf mindestens eine benutzerdefinierte Funktion verweist, bei der diese Eigenschaft auf 1 festgelegt ist, wird der Vorgang vollständig protokolliert.
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die CREATE TABLE-Berechtigung in der Zieldatenbank.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-creating-a-table-by-specifying-columns-from-multiple-sources"></a>A. Erstellen einer Tabelle durch Angeben von Spalten aus mehreren Quellen  
 Im folgenden Beispiel wird die `dbo.EmployeeAddresses`-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank erstellt, indem sieben Spalten aus verschiedenen mitarbeiter- und adressbezogenen Tabellen ausgewählt werden.  
  
```sql  
SELECT c.FirstName, c.LastName, e.JobTitle, a.AddressLine1, a.City,   
    sp.Name AS [State/Province], a.PostalCode  
INTO dbo.EmployeeAddresses  
FROM Person.Person AS c  
    JOIN HumanResources.Employee AS e   
    ON e.BusinessEntityID = c.BusinessEntityID  
    JOIN Person.BusinessEntityAddress AS bea  
    ON e.BusinessEntityID = bea.BusinessEntityID  
    JOIN Person.Address AS a  
    ON bea.AddressID = a.AddressID  
    JOIN Person.StateProvince as sp   
    ON sp.StateProvinceID = a.StateProvinceID;  
GO  
```  
  
### <a name="b-inserting-rows-using-minimal-logging"></a>B. Einfügen von Zeilen bei minimaler Protokollierung  
 Im folgenden Beispiel wird die `dbo.NewProducts`-Tabelle erstellt, und Zeilen aus der `Production.Product`-Tabelle werden eingefügt. Im Beispiel wird davon ausgegangen, dass das Wiederherstellungsmodell der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank auf FULL festgelegt wird. Um sicherzustellen, dass die minimale Protokollierung verwendet wird, wird das Wiederherstellungsmodell der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank auf BULK_LOGGED festgelegt, bevor Zeilen eingefügt und nach der SELECT...INTO-SELECT-Anweisung auf FULL zurückgesetzt werden. Dadurch wird sichergestellt, dass die SELECT…INTO-Anweisung minimalen Speicherplatz im Transaktionsprotokoll belegt und effektiv ausgeführt wird.  
  
```sql  
ALTER DATABASE AdventureWorks2012 SET RECOVERY BULK_LOGGED;  
GO  
  
SELECT * INTO dbo.NewProducts  
FROM Production.Product  
WHERE ListPrice > $25   
AND ListPrice < $100;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
GO  
```  
  
### <a name="c-creating-an-identity-column-using-the-identity-function"></a>C. Erstellen einer Identitätsspalte mithilfe der IDENTITY-Funktion  
 Im folgenden Beispiel wird die IDENTITY-Funktion verwendet, um eine Identitätsspalte in der neuen `Person.USAddress`-Tabelle der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank zu erstellen. Dies ist erforderlich, da die SELECT-Anweisung, durch die die Tabelle definiert wird, einen Join enthält. Dieser Join bewirkt, dass die IDENTITY-Eigenschaft nicht an die neue Tabelle übertragen wird. Beachten Sie, dass sich der in der IDENTITY-Funktion angegebene Ausgangs- und Inkrementwert von dem der `AddressID`-Spalte in der `Person.Address`-Quelltabelle unterscheidet.  
  
```sql  
-- Determine the IDENTITY status of the source column AddressID.  
SELECT OBJECT_NAME(object_id) AS TableName, name AS column_name, 
  is_identity, seed_value, increment_value  
FROM sys.identity_columns  
WHERE name = 'AddressID';  
  
-- Create a new table with columns from the existing table Person.Address. 
-- A new IDENTITY column is created by using the IDENTITY function.  
SELECT IDENTITY (int, 100, 5) AS AddressID,   
       a.AddressLine1, a.City, b.Name AS State, a.PostalCode  
INTO Person.USAddress   
FROM Person.Address AS a  
INNER JOIN Person.StateProvince AS b 
  ON a.StateProvinceID = b.StateProvinceID  
WHERE b.CountryRegionCode = N'US';   
  
-- Verify the IDENTITY status of the AddressID columns in both tables.  
SELECT OBJECT_NAME(object_id) AS TableName, name AS column_name, 
  is_identity, seed_value, increment_value  
FROM sys.identity_columns  
WHERE name = 'AddressID';  
```  
  
### <a name="d-creating-a-table-by-specifying-columns-from-a-remote-data-source"></a>D: Erstellen einer Tabelle durch Angeben von Spalten aus einer Remotedatenquelle  
 Im folgenden Beispiel werden drei Methoden beschrieben, um eine neue Tabelle für den lokalen Server von einer Remotedatenquelle aus zu erstellen. Zunächst wird im Beispiel ein Link zur Remotedatenquelle erstellt. Der Name des Verbindungsservers `MyLinkServer,` wird dann in der FROM-Klausel der ersten SELECT...INTO-Anweisung und der OPENQUERY-Funktion der zweiten SELECT...INTO-Anweisung angegeben. Die dritte SELECT...INTO-Anweisung verwendet die OPENDATASOURCE-Funktion, die die Remotedatenquelle direkt angibt, anstatt den Namen des Verbindungsservers zu verwenden.  
  
 **Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
```sql
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' 
-- or 'server_name\instance_name'.  
EXEC sp_addlinkedserver @server = N'MyLinkServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI',   
    @datasrc = N'server_name',  
    @catalog = N'AdventureWorks2012';  
GO  

USE AdventureWorks2012;  
GO  
-- Specify the remote data source in the FROM clause using a four-part name   
-- in the form linked_server.catalog.schema.object.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.Departments  
FROM MyLinkServer.AdventureWorks2012.HumanResources.Department  
GO  
-- Use the OPENQUERY function to access the remote data source.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.DepartmentsUsingOpenQuery  
FROM OPENQUERY(MyLinkServer, 'SELECT *  
               FROM AdventureWorks2012.HumanResources.Department');   
GO  
-- Use the OPENDATASOURCE function to specify the remote data source.  
-- Specify a valid server name for Data Source using the format 
-- server_name or server_name\instance_name.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.DepartmentsUsingOpenDataSource  
FROM OPENDATASOURCE('SQLNCLI',  
    'Data Source=server_name;Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Department;  
GO  
```  
  
### <a name="e-import-from-an-external-table-created-with-polybase"></a>E. Importieren aus einer externen mit PolyBase erstellten Tabelle  
 Importieren Sie Daten aus Hadoop oder Azure Storage in SQL Server für den beständigen Speicher. Verwenden Sie `SELECT INTO`, um Daten, auf die eine externe Tabelle verweist, zu importieren und dauerhaft in SQL Server zu speichern. Erstellen Sie dynamisch eine relationale Tabelle, und erstellen Sie dann in einem zweiten Schritt einen Columnstore-Index über die Tabelle.  
  
 **Gilt für:** [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]  
  
```sql
-- Import data for car drivers into SQL Server to do more in-depth analysis.  
SELECT DISTINCT   
        Insured_Customers.FirstName, Insured_Customers.LastName,   
        Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
INTO Fast_Customers from Insured_Customers INNER JOIN   
(  
        SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
ORDER BY YearlyIncome;  
```  
### <a name="f-copying-the-data-from-one-table-to-another-and-create-the-new-table-on-a-specified-filegroup"></a>F. Kopieren der Daten aus einer der Tabellen in eine andere und Erstellen der neuen Tabelle in einer angegebenen Dateigruppe
Das folgende Beispiel zeigt, wie eine neue Tabelle als Kopie einer anderen Tabelle erstellt und in eine angegebene Dateigruppe geladen wird, bei der es sich nicht um die Standarddateigruppe des Benutzers handelt.

 **Anwendungsbereich:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und höher.

```sql
ALTER DATABASE [AdventureWorksDW2016] ADD FILEGROUP FG2;
ALTER DATABASE [AdventureWorksDW2016]
ADD FILE
(
NAME='FG2_Data',
FILENAME = '/var/opt/mssql/data/AdventureWorksDW2016_Data1.mdf'
)
TO FILEGROUP FG2;
GO
SELECT * INTO [dbo].[FactResellerSalesXL] ON FG2 FROM [dbo].[FactResellerSales];
```
  
## <a name="see-also"></a>Weitere Informationen  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Beispiele für SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-examples-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [IDENTITY &#40;Funktion&#41; &#40;Transact-SQL&#41;](../../t-sql/functions/identity-function-transact-sql.md)  
  
  
