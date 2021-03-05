---
description: CREATE TABLE AS SELECT (Azure Synapse Analytics)
title: CREATE TABLE AS SELECT (Azure Synapse Analytics) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/07/2016
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: d1e08f88-64ef-4001-8a66-372249df2533
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 6af11a4ffc62f8311f31cf378c9a65f67ae58a2b
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186547"
---
# <a name="create-table-as-select-azure-synapse-analytics"></a>CREATE TABLE AS SELECT (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

CREATE TABLE AS SELECT (CTAS) ist eins der wichtigsten verfügbaren T-SQL-Features. Es handelt sich um einen vollständig parallelisierten Vorgang, der basierend auf der Ausgabe einer SELECT-Anweisung eine neue Tabelle erstellt. CTAS ist die schnellste und einfachste Möglichkeit, eine Kopie einer Tabelle zu erstellen.   
 
 Verwenden Sie CTAS z.B., um:  
  
-   eine Tabelle mit einer anderen Hashverteilungsspalte neu zu erstellen.
-   eine Tabelle als Replikat neu zu erstellen.   
-   einen Columnstore-Index für ausgewählte Spalten in der Tabelle zu erstellen.  
-   externe Daten abzufragen oder zu importieren.  

> [!NOTE]  
> Da CTAS auf den Funktionen zum Erstellen einer Tabelle aufbaut, wird in diesem Artikel nicht der Artikel CREATE TABLE wiederholt. Stattdessen werden die Unterschiede zwischen den Anweisungen CTAS und CREATE TABLE beschrieben. Weitere Informationen zu CREATE TABLE finden Sie im Artikel zur Anweisung [CREATE TABLE (Azure Synapse Analytics)](./create-table-azure-sql-data-warehouse.md). 
  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
<a name="syntax-bk"></a>

## <a name="syntax"></a>Syntax   

```syntaxsql
CREATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    [ ( column_name [ ,...n ] ) ]  
    WITH ( 
      <distribution_option> -- required
      [ , <table_option> [ ,...n ] ]    
    )  
    AS <select_statement>  
    OPTION <query_hint> 
[;]  

<distribution_option> ::=
    { 
        DISTRIBUTION = HASH ( distribution_column_name ) 
      | DISTRIBUTION = ROUND_ROBIN 
      | DISTRIBUTION = REPLICATE
    }   

<table_option> ::= 
    {   
        CLUSTERED COLUMNSTORE INDEX --default for Synapse Analytics 
      | CLUSTERED COLUMNSTORE INDEX ORDER (column[,...n])
      | HEAP --default for Parallel Data Warehouse   
      | CLUSTERED INDEX ( { index_column_name [ ASC | DESC ] } [ ,...n ] ) --default is ASC 
    }  
      | PARTITION ( partition_column_name RANGE [ LEFT | RIGHT ] --default is LEFT  
        FOR VALUES ( [ boundary_value [,...n] ] ) ) 
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT select_criteria  

<query_hint> ::=
    {
        MAXDOP 
    }
```  

<a name="arguments-bk"></a>
  
## <a name="arguments"></a>Argumente  
Einzelheiten finden Sie im Abschnitt [Arguments (Argumente)](./create-table-azure-sql-data-warehouse.md#Arguments) im Artikel CREATE TABLE.  

<a name="column-options-bk"></a>

### <a name="column-options"></a>Spaltenoptionen
`column_name` [ ,...`n` ]   
 Spaltennamen lassen die in CREATE TABLE erwähnten [Spaltenoptionen](./create-table-azure-sql-data-warehouse.md#ColumnOptions) nicht zu.  Sie können stattdessen eine optionale Liste mit mindestens einem Spaltennamen für die neue Tabelle bereitstellen. Die Spalten in der neuen Tabelle verwenden die von Ihnen angegebenen Namen. Wenn Sie Spaltennamen angeben, muss die Anzahl der Spalten in der Spaltenliste mit der Anzahl der Spalten in den SELECT-Ergebnissen übereinstimmen. Wenn Sie keine Spaltennamen angeben, verwendet die neue Zieltabelle die Spaltennamen, die in den Ergebnissen einer SELECT-Anweisung verwendet werden. 
  
 Sie können keine anderen Spaltenoptionen wie z.B. Datentypen, Sortierung oder NULL-Zulässigkeit angeben. Jedes dieser Attribute wird aus den Ergebnissen der `SELECT`-Anweisung abgeleitet. Sie können die SELECT-Anwendung allerdings zum Ändern der Attribute verwenden. Ein Beispiel finden Sie unter [Verwenden von CTAS zum Ändern von Spaltenattributen](#ctas-change-column-attributes-bk).   

<a name="table-distribution-options-bk"></a>

### <a name="table-distribution-options"></a>Tabellenverteilungsoptionen

`DISTRIBUTION` = `HASH` ( *distribution_column_name* ) | ROUND_ROBIN | REPLICATE      
Die CTAS-Anweisung erfordert eine Verteilungsoption und verfügt nicht über Standardwerte. Hierin unterscheidet sie sich von CREATE TABLE, da letztere Standardwerte aufweist. 

Weitere Informationen und Hilfe bei der Auswahl der besten Verteilungsspalte finden Sie im Abschnitt [Table distribution options (Tabellenverteilungsoptionen)](./create-table-azure-sql-data-warehouse.md#TableDistributionOptions) im Artikel CREATE TABLE. 

<a name="table-partition-options-bk"></a>

### <a name="table-partition-options"></a>Tabellenpartitionsoptionen
Standardmäßig erstellt die CTAS-Anweisung eine nicht partitionierte Tabelle, selbst wenn die Quelltabelle partitioniert ist. Sie müssen die Partitionsoptionen angeben, um mit der CTAS-Anweisung eine partitionierte Tabelle zu erstellen. 

Einzelheiten finden Sie im Abschnitt [Table partition options (Tabellenpartitionsoptionen)](./create-table-azure-sql-data-warehouse.md#TablePartitionOptions) im Artikel CREATE TABLE.

<a name="select-options-bk"></a>

### <a name="select-statement"></a>Select-Anweisung
Die SELECT-Anweisung ist der wesentliche Unterschied zwischen CTAS und CREATE TABLE.  

 `WITH` *common_table_expression*  
 Gibt ein temporäres benanntes Resultset an, das als allgemeiner Tabellenausdruck (CTE, Common Table Expression) bezeichnet wird. Weitere Informationen finden Sie unter [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 `SELECT` *select_criteria*  
 Füllt die neue Tabelle mit den Ergebnissen einer SELECT-Anweisung auf. *Select_criteria* ist der Hauptteil der SELECT-Anweisung, der bestimmt, welche Daten in die neue Tabelle kopiert werden sollen. Informationen zu SELECT-Anweisungen finden Sie unter [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
 
### <a name="query-hint"></a>Abfragetipp
Benutzer können MAXDOP auf eine ganze Zahl festlegen, um den maximalen Grad an Parallelität zu steuern.  Wenn MAXDOP auf 1 festgelegt ist, wird die Abfrage von einem einzelnen Thread ausgeführt.

 
<a name="permissions-bk"></a>  
  
## <a name="permissions"></a>Berechtigungen  
CTAS erfordert eine `SELECT`-Berechtigung für alle Objekte, auf die in *select_criteria* verwiesen wird.

Berechtigungen zum Erstellen einer Tabelle finden Sie unter [Permissions (Berechtigungen)](./create-table-azure-sql-data-warehouse.md#Permissions) im Artikel CREATE TABLE. 
  
<a name="general-remarks-bk"></a>
  
## <a name="general-remarks"></a>Allgemeine Hinweise
Weitere Informationen finden Sie unter [Allgemeine Hinweise](./create-table-azure-sql-data-warehouse.md#GeneralRemarks) im Artikel CREATE TABLE.

<a name="limitations-bk"></a>

## <a name="limitations-and-restrictions"></a>Einschränkungen  

Ein sortierter, gruppierter Columnstore-Index kann für Spalten beliebiger in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] unterstützter Datentypen erstellt werden, mit Ausnahme von Zeichenfolgenspalten.  

[SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md) hat keine Auswirkung auf CTAS. Verwenden Sie [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md), um ein ähnliches Verhalten zu erzielen.  
 
Weitere Informationen finden Sie unter [Limitations and Restrictions (Einschränkungen)](./create-table-azure-sql-data-warehouse.md#LimitationsRestrictions) im Artikel CREATE TABLE.

<a name="locking-behavior-bk"></a>
  
## <a name="locking-behavior"></a>Sperrverhalten  
 Weitere Informationen finden Sie unter [Locking Behavior (Sperrverhalten)](./create-table-azure-sql-data-warehouse.md#LockingBehavior) im Artikel CREATE TABLE.
 
<a name="performance-bk"></a>
 
 ## <a name="performance"></a>Leistung 

Bei einer Tabelle mit Hashverteilung können Sie CTAS verwenden, um eine andere Verteilungsspalte auszuwählen. Hierdurch kann die Leistung für Joins und Aggregationen verbessert werden. Wenn Sie keine andere Verteilungsspalte auswählen möchten, erreichen Sie die beste CTAS-Leistung, wenn Sie dieselbe Verteilungsspalte auswählen. Hierdurch wird die Neuverteilung von Zeilen vermieden. 

Wenn Sie CTAS verwenden, um eine Tabelle zu erstellen und die Leistung unwichtig ist, können Sie `ROUND_ROBIN` angeben, damit Sie sich nicht für eine Verteilungsspalte entscheiden müssen.

Um die Datenverschiebung in nachfolgenden Abfragen zu vermeiden, können Sie `REPLICATE` angeben. Dies erfolgt auf Kosten der höheren Speicherkapazität für das Laden einer vollständigen Kopie der Tabelle auf jedem Computeknoten.  


<a name="examples-copy-table-bk"></a>

## <a name="examples-for-copying-a-table"></a>Beispiele für das Kopieren einer Tabelle

<a name="ctas-copy-table-bk"></a>

### <a name="a-use-ctas-to-copy-a-table"></a>A. Kopieren einer Tabelle mithilfe von CTAS 
Gilt für: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Eine der häufigsten Anwendungen von `CTAS` ist wahrscheinlich das Erstellen einer Kopie einer Tabelle, damit Sie die DLL verändern können. Wenn Sie Ihre Tabelle z.B. ursprünglich als `ROUND_ROBIN` erstellt haben und sie jetzt in eine Tabelle ändern wollen, die auf eine Spalte verteilt wird, können Sie mit `CTAS` die Verteilungsspalte ändern. `CTAS` kann auch zur Änderung der Partitionierung, des Index oder der Spaltentypen verwendet werden.

Nehmen wir an, dass Sie diese Tabelle mithilfe des Standardverteilungstyps von `ROUND_ROBIN` erstellt haben, da in `CREATE TABLE` keine Verteilungsspalte angegeben wurde.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey INT NOT NULL,
    OrderDateKey INT NOT NULL,
    DueDateKey INT NOT NULL,
    ShipDateKey INT NOT NULL,
    CustomerKey INT NOT NULL,
    PromotionKey INT NOT NULL,
    CurrencyKey INT NOT NULL,
    SalesTerritoryKey INT NOT NULL,
    SalesOrderNumber NVARCHAR(20) NOT NULL,
    SalesOrderLineNumber TINYINT NOT NULL,
    RevisionNumber TINYINT NOT NULL,
    OrderQuantity SMALLINT NOT NULL,
    UnitPrice MONEY NOT NULL,
    ExtendedAmount MONEY NOT NULL,
    UnitPriceDiscountPct FLOAT NOT NULL,
    DiscountAmount FLOAT NOT NULL,
    ProductStandardCost MONEY NOT NULL,
    TotalProductCost MONEY NOT NULL,
    SalesAmount MONEY NOT NULL,
    TaxAmt MONEY NOT NULL,
    Freight MONEY NOT NULL,
    CarrierTrackingNumber NVARCHAR(25),
    CustomerPONumber NVARCHAR(25)
);
```

Jetzt möchten Sie eine neue Kopie dieser Tabelle mit einem gruppierten Columnstore-Index erstellen, damit Sie die Leistungsvorteile von gruppierten Columnstore-Tabellen nutzen können. Außerdem soll diese Tabelle über ProductKey verteilt werden, da Sie für diese Spalte Joins erwarten und Datenverschiebungen während Joins auf ProductKey vermeiden möchten. Außerdem soll eine Partitionierung für OrderDateKey hinzugefügt werden, damit Sie alte Daten schnell durch Ablegen alter Partitionen löschen können. Hier finden Sie die CTAS-Anweisung, mit der die Kopie Ihrer alten Tabelle in eine neue Tabelle kopiert wird.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Schließlich können Sie Ihre Tabellen umbenennen, um die Namen mit Ihrer neuen Tabelle zu tauschen. Anschließend können Sie die alte Tabelle ablegen.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

<a name="examples-column-bk"></a>

## <a name="examples-for-column-options"></a>Beispiele für Spaltenoptionen

<a name="ctas-change-column-attributes-bk"></a>

### <a name="b-use-ctas-to-change-column-attributes"></a>B. Verwenden von CTAS zum Ändern von Spaltenattributen 
Gilt für: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Dieses Beispiel verwendet CTAS, um die Datentypen, NULL-Zulässigkeit und Sortierung für mehrere Spalten in der Tabelle DimCustomer2 zu ändern.  
  
```sql  
-- Original table 
CREATE TABLE [dbo].[DimCustomer2] (  
    [CustomerKey] INT NOT NULL,  
    [GeographyKey] INT NULL,  
    [CustomerAlternateKey] nvarchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL  
)  
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH([CustomerKey]));  
  
-- CTAS example to change data types, nullability, and column collations  
CREATE TABLE test  
WITH (HEAP, DISTRIBUTION = ROUND_ROBIN)  
AS  
SELECT  
    CustomerKey AS CustomerKeyNoChange,  
    CustomerKey*1 AS CustomerKeyChangeNullable,  
    CAST(CustomerKey AS DECIMAL(10,2)) AS CustomerKeyChangeDataTypeNullable,  
    ISNULL(CAST(CustomerKey AS DECIMAL(10,2)),0) AS CustomerKeyChangeDataTypeNotNullable,  
    GeographyKey AS GeographyKeyNoChange,  
    ISNULL(GeographyKey,0) AS GeographyKeyChangeNotNullable,  
    CustomerAlternateKey AS CustomerAlternateKeyNoChange,  
    CASE WHEN CustomerAlternateKey = CustomerAlternateKey 
        THEN CustomerAlternateKey END AS CustomerAlternateKeyNullable,  
    CustomerAlternateKey COLLATE Latin1_General_CS_AS_KS_WS AS CustomerAlternateKeyChangeCollation  
FROM [dbo].[DimCustomer2]  
  
-- Resulting table 
CREATE TABLE [dbo].[test] (
    [CustomerKeyNoChange] INT NOT NULL, 
    [CustomerKeyChangeNullable] INT NULL, 
    [CustomerKeyChangeDataTypeNullable] DECIMAL(10, 2) NULL, 
    [CustomerKeyChangeDataTypeNotNullable] DECIMAL(10, 2) NOT NULL, 
    [GeographyKeyNoChange] INT NULL, 
    [GeographyKeyChangeNotNullable] INT NOT NULL, 
    [CustomerAlternateKeyNoChange] NVARCHAR(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL, 
    [CustomerAlternateKeyNullable] NVARCHAR(15) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
    [CustomerAlternateKeyChangeCollation] NVARCHAR(15) COLLATE Latin1_General_CS_AS_KS_WS NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN);
```
 
Als letzten Schritt können Sie [RENAME &#40;Transact-SQL&#41;](../../t-sql/statements/rename-transact-sql.md) verwenden, um die Tabellennamen zu tauschen. Dadurch wird DimCustomer2 zur neuen Tabelle.

```sql
RENAME OBJECT DimCustomer2 TO DimCustomer2_old;
RENAME OBJECT test TO DimCustomer2;

DROP TABLE DimCustomer2_old;
```

<a name="examples-table-distribution-bk"></a>

## <a name="examples-for-table-distribution"></a>Beispiele für die Tabellenverteilung

<a name="ctas-change-distribution-method-bk"></a>

### <a name="c-use-ctas-to-change-the-distribution-method-for-a-table"></a>C. Verwenden von CTAS zum Ändern der Verteilungsmethode für eine Tabelle
Gilt für: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Dieses einfache Beispiel zeigt, wie die Verteilungsmethode für eine Tabelle geändert wird. Um die Funktionsweise hinter diesem Vorgang zu zeigen, wird eine Tabelle mit Hashverteilung in RoundRobin geändert. Anschließend wird die RoundRobin-Tabelle wieder in eine Tabelle mit Hashverteilung geändert. Die finale Tabelle entspricht der ursprünglichen Tabelle. 

In den meisten Fällen müssen Sie keine Tabelle mit Hashverteilung in eine RoundRobin-Tabelle ändern. Es kommt häufiger vor, dass Sie eine RoundRobin-Tabelle in eine Tabelle mit Hashverteilung ändern müssen. Dies ist z.B. der Fall, wenn Sie eine neue Tabelle ursprünglich als RoundRobin laden und sie später in eine Tabelle mit Hashverteilung ändern möchten, um eine Leistungssteigerung bei Joins zu erzielen.

In diesem Beispiel wird die AdventureWorksDW-Beispieldatenbank verwendet. Unter [Laden von Beispieldaten in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-load-sample-databases/) erfahren Sie, wie Sie die [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]-Version laden.
 
```sql
-- DimSalesTerritory is hash-distributed.
-- Copy it to a round-robin table.
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  
Ändern Sie sie nun wieder zurück in eine Tabelle mit Hashverteilung.

```sql
-- You just made DimSalesTerritory a round-robin table.
-- Change it back to the original hash-distributed table. 
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH(SalesTerritoryKey) 
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```
 
<a name="ctas-change-to-replicated-bk"></a>

### <a name="d-use-ctas-to-convert-a-table-to-a-replicated-table"></a>D: Verwenden von CTAS zur Konvertierung in eine replizierte Tabelle  
Gilt für: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Dieses Beispiel gilt für die Konvertierung von RoundRobin-Tabellen oder Tabellen mit Hashverteilung in eine replizierte Tabelle. In diesem Beispiel geht die vorherige Methode zum Ändern des Verteilungstyps noch einen Schritt weiter.  Da es sich bei DimSalesTerritory um eine Dimension und vermutlich um eine kleinere Tabelle handelt, können Sie die Tabelle als repliziert neu erstellen, um Datenverschiebungen bei der Verknüpfung zu anderen Tabellen zu vermeiden. 

```sql
-- DimSalesTerritory is hash-distributed.
-- Copy it to a replicated table.
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE 
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```
 
### <a name="e-use-ctas-to-create-a-table-with-fewer-columns"></a>E. Verwenden von CTAS zum Erstellen einer Tabelle mit weniger Spalten
Gilt für: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Das folgende Beispiel erstellt eine verteilte RoundRobin-Tabelle mit dem Namen `myTable (c, ln)`. Die neue Tabelle hat nur zwei Spalten. Sie verwendet die Spaltenaliase in der SELECT-Anweisung für die Spaltennamen.  
  
```sql  
CREATE TABLE myTable  
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN  
  )  
AS SELECT CustomerKey AS c, LastName AS ln  
    FROM dimCustomer; 
```  

<a name="examples-query-hints-bk"></a>

## <a name="examples-for-query-hints"></a>Beispiele für Abfragehinweise

<a name="ctas-query-hint-bk"></a>

### <a name="f-use-a-query-hint-with-create-table-as-select-ctas"></a>F. Verwenden eines Abfragehinweises mit CREATE TABLE AS SELECT (CTAS)  
Gilt für: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]
  
Diese Abfrage zeigt die grundlegende Syntax für die Verwendung eines Join-Abfragehinweis mit der CTAS-Anweisung. Nach dem Senden der Abfrage verwendet [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] die Hashjoinstrategie beim Generieren des Abfrageplans für jede einzelne Verteilung. Weitere Informationen zum Hashjoin-Abfragehinweis finden Sie unter [OPTION-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md).  
  
```sql  
CREATE TABLE dbo.FactInternetSalesNew  
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN   
  )  
AS SELECT T1.* FROM dbo.FactInternetSales T1 JOIN dbo.DimCustomer T2  
ON ( T1.CustomerKey = T2.CustomerKey )  
OPTION ( HASH JOIN );  
```  

<a name="examples-external-tables"></a>

## <a name="examples-for-external-tables"></a>Beispiele für externe Tabellen

<a name="ctas-azure-blob-storage-bk"></a>

### <a name="g-use-ctas-to-import-data-from-azure-blob-storage"></a>G. Verwenden von CTAS zum Importieren von Daten aus Azure Blob Storage  
Gilt für: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Verwenden Sie zum Importieren von Daten aus einer externen Tabelle einfach CREATE TABLE AS SELECT, um die Daten aus der externen Tabelle auszuwählen. Die Syntax zum Auswählen von Daten aus einer externen Tabelle in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] entspricht der Syntax zum Auswählen von Daten aus einer regulären Tabelle.  
  
 Das folgende Beispiel definiert eine externe Tabelle basierend auf Daten in einem Azure Blob Storage-Konto. Anschließend wird CREATE TABLE AS SELECT verwendet, um Daten aus der externen Tabelle auszuwählen. Dadurch werden die Daten aus Dateien mit Texttrennzeichen aus Azure Blob Storage importiert und in einer neuen [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]-Tabelle gespeichert.  
  
```sql  
--Use your own processes to create the text-delimited files on Azure blob storage.  
--Create the external table called ClickStream.  
CREATE EXTERNAL TABLE ClickStreamExt (   
    url VARCHAR(50),  
    event_date DATE,  
    user_IP VARCHAR(50)  
)  
WITH (  
    LOCATION='/logs/clickstream/2015/',  
    DATA_SOURCE = MyAzureStorage,  
    FILE_FORMAT = TextFileFormat)  
;  
  
--Use CREATE TABLE AS SELECT to import the Azure blob storage data into a new   
--Synapse Analytics table called ClickStreamData  
CREATE TABLE ClickStreamData   
WITH  
  (  
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH (user_IP)  
  )  
AS SELECT * FROM ClickStreamExt  
;  
```  

<a name="ctas-import-Hadoop-bk"></a>
  
### <a name="h-use-ctas-to-import-hadoop-data-from-an-external-table"></a>H. Verwenden von CTAS zum Importieren von Hadoop-Daten aus einer externen Tabelle  
Gilt für: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
Verwenden Sie zum Importieren von Daten aus einer externen Tabelle einfach CREATE TABLE AS SELECT, um die Daten aus der externen Tabelle auszuwählen. Die Syntax zum Auswählen von Daten aus einer externen Tabelle in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] entspricht der Syntax zum Auswählen von Daten aus einer regulären Tabelle.  
  
 Das folgende Beispiel definiert eine externe Tabelle auf einem Hadoop-Cluster. Anschließend wird CREATE TABLE AS SELECT verwendet, um Daten aus der externen Tabelle auszuwählen. Dadurch werden die Daten aus Dateien mit Texttrennzeichen aus Hadoop importiert und in einer neuen [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Tabelle gespeichert.  
  
```sql  
-- Create the external table called ClickStream.  
CREATE EXTERNAL TABLE ClickStreamExt (   
    url VARCHAR(50),  
    event_date DATE,  
    user_IP VARCHAR(50)  
)  
WITH (  
    LOCATION = 'hdfs://MyHadoop:5000/tpch1GB/employee.tbl',  
    FORMAT_OPTIONS ( FIELD_TERMINATOR = '|')  
)  
;  
  
-- Use your own processes to create the Hadoop text-delimited files 
-- on the Hadoop Cluster.  
  
-- Use CREATE TABLE AS SELECT to import the Hadoop data into a new 
-- table called ClickStreamPDW  
CREATE TABLE ClickStreamPDW   
WITH  
  (  
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH (user_IP)  
  )  
AS SELECT * FROM ClickStreamExt  
;   
```  
 
<a name="examples-workarounds-bk"></a>
 
## <a name="examples-using-ctas-to-replace-sql-server-code"></a>Beispiele für die Verwendung von CTAS zum Ersetzen von SQL Server-Code

Verwenden Sie CTAS, um einige nicht unterstützte Features zu umgehen. Sie können nicht nur Code für das Data Warehouse ausführen – das Umschreiben von vorhandenem Code für die Verwendung von CTAS verbessert normalerweise auch die Leistung. Das liegt am vollständig parallelisierten Design dieser Anweisung. 

> [!NOTE]
> Versuchen Sie, die Denkweise „CTAS zuerst“ zu verinnerlichen. Wenn Sie denken, dass Sie ein Problem mithilfe von `CTAS` lösen können, ist es in der Regel der beste Weg, das Problem anzugehen, selbst wenn Sie dann möglicherweise mehr Daten schreiben müssen.
>

<a name="ctas-replace-select-into-bk"></a>

### <a name="i-use-ctas-instead-of-selectinto"></a>I. Verwenden von CTAS anstelle von SELECT..INTO  
Gilt für: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

SQL Server-Code verwendet in der Regel SELECT..INTO, um eine Tabelle mit den Ergebnissen einer SELECT-Anweisung aufzufüllen. Dies ist ein Beispiel für eine SELECT..INTO-Anweisung von SQL Server.

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Diese Syntax wird in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und Parallel Data Warehouse nicht unterstützt. Dieses Beispiel stellt das erneute Generieren der vorherigen SELECT..INTO-Anweisung als CTAS-Anweisung dar. Sie können eine der DISTRIBUTION-Optionen verwenden, die in der CTAS-Syntax beschrieben werden. Dieses Beispiel verwendet die Verteilungsmethode ROUND_ROBIN.

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

<a name="ctas-replace-implicit-joins-bk"></a>

### <a name="j-use-ctas-and-implicit-joins-to-replace-ansi-joins-in-the-from-clause-of-an-update-statement"></a>J. Verwenden von CTAS und implizierten Joins zum Ersetzen von ANSI-Joins in der `FROM`-Klausel einer `UPDATE`-Anweisung  
Gilt für: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Möglicherweise liegt ein komplexes Update vor, das mithilfe der ANSI-Verknüpfungssyntax mehr als zwei Tabellen verknüpft, um UPDATE oder DELETE durchzuführen.

Stellen Sie sich vor, dass Sie die folgende Tabelle aktualisieren müssen:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]          SMALLINT    NOT NULL
,   [TotalSalesAmount]      MONEY       NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Die ursprüngliche Abfrage könnte in etwa wie folgt ausgesehen haben:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Da [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] keine ANSI-Joins in der `FROM`-Klausel einer `UPDATE`-Anweisung unterstützt, können Sie diesen SQL Server-Code nicht verwenden, ohne ihn leicht zu ändern.

Sie können eine Kombination aus einer `CTAS`-Anweisung und einem impliziten Join verwenden, um diesen Code zu ersetzen:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

<a name="ctas-replace-ansi-joins-bk"></a>

### <a name="k-use-ctas-to-specify-which-data-to-keep-instead-of-using-ansi-joins-in-the-from-clause-of-a-delete-statement"></a>K. Verwenden von CTAS zum Angeben der beizubehaltenden Daten statt der Verwendung von ANSI-Joins in der FROM-Klausel einer DELETE-Anweisung  
Gilt für: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Manchmal ist die Verwendung von `CTAS` der beste Ansatz zum Löschen von Daten. Anstatt die Daten zu löschen, wählen Sie einfach die Daten aus, die Sie behalten möchten. Dies gilt insbesondere für `DELETE`-Anweisungen, die die ANSI-Verknüpfungssyntax verwenden, da [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] keine ANSI-Joins in der `FROM`-Klausel einer `DELETE`-Anweisung unterstützt.

Ein Beispiel für eine konvertierte DELETE-Anweisung sehen Sie hier:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

<a name="ctas-simplify-merge-bk"></a>

### <a name="l-use-ctas-to-simplify-merge-statements"></a>L. Verwenden von CTAS zur Vereinfachung von MERGE-Anweisungen  
Gilt für: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

MERGE-Anweisungen können zumindest teilweise durch `CTAS` ersetzt werden. Sie können `INSERT` und `UPDATE` in eine einzige Anweisung konsolidieren. Alle gelöschten Datensätze müssten in einer zweiten Anweisung geschlossen werden.

Hier sehen Sie ein Beispiel für `UPSERT`:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimProduct_upsert]  TO [DimProduct];

```

<a name="ctas-data-type-and-nullability-bk"></a>

### <a name="m-explicitly-state-data-type-and-nullability-of-output"></a>M. Explizites Angeben des Datentyps und der NULL-Zulässigkeit der Ausgabe  
Gilt für: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Beim Migrieren von SQL Server-Code zu [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] stoßen Sie möglicherweise auf das folgende Programmierungsmuster:

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Sie denken vermutlich instinktiv, dass Sie diesen Code zu einer CTAS-Anweisung migrieren sollten und damit richtig liegen. Hier gibt es jedoch ein verstecktes Problem.

Der folgende Code erzielt NICHT das gleiche Ergebnis:

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Beachten Sie, dass die Spalte „result“ (Ergebnis) die Werte des Datentyps und der NULL-Zulässigkeit des Ausdrucks übernimmt. Dies kann zu geringfügigen Abweichungen bei Werten führen, wenn Sie nicht vorsichtig sind.

Versuchen Sie als Beispiel einmal Folgendes:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Der für „result“ gespeicherte Wert unterscheidet sich. Da der gespeicherte Wert in der Spalte „result“ auch in anderen Ausdrücken verwendet wird, hat dieser Fehler eine noch größere Bedeutung.

![CREATE TABLE AS SELECT-Ergebnisse](../../t-sql/statements/media/create-table-as-select-results.png)

Dies ist besonders wichtig für die Datenmigration. Obwohl die zweite Abfrage wohl genauer ist, gibt es hier ein Problem. Die Daten würden sich zum Quellsystem unterscheiden, was Fragen zur Integrität in der Migration aufwirft. Dies ist einer der seltenen Fällen, in denen die „falsche“ Antwort tatsächlich die richtige ist!

Der Grund für die Abweichung zwischen beiden Ergebnissen ist die implizite Typumwandlung. Im ersten Beispiel definiert die Tabelle die Spaltendefinition. Wenn die Zeile eingefügt wird, tritt eine implizite Typkonvertierung ein. Im zweiten Beispiel tritt keine implizite Typkonvertierung ein, da der Ausdruck den Datentyp der Spalte definiert. Beachten Sie außerdem, dass die Spalte im zweiten Beispiel als Spalte definiert wurde, für die NULL zulässig ist. Dies ist im ersten Beispiel nicht der Fall. Bei der Erstellung der Tabelle im ersten Beispiel wurde die NULL-Zulässigkeit der Spalte explizit definiert. Im zweiten Beispiel wurde es dem Ausdruck überlassen, was standardmäßig zur Definition von NULL führt.  

Zum Beheben dieser Probleme müssen Sie die Typkonvertierung und NULL-Zulässigkeit im Teil `SELECT` der `CTAS`-Anweisung explizit festlegen. Diese Eigenschaften können nicht im Teil CREATE TABLE festgelegt werden.

Im folgenden Beispiel wird veranschaulicht, wie Sie den Code korrigieren:

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Beachten Sie Folgendes:
- CAST oder CONVERT hätten verwendet werden können
- ISNULL wird anstelle von COALESCE verwendet, um die NULL-Zulässigkeit zu erzwingen
- ISNULL ist die äußerste Funktion
- Der zweite Teil von ISNULL ist eine Konstante, d.h. 0 (null)

> [!NOTE]
> Damit die NULL-Zulässigkeit korrekt festgelegt wird, ist es wichtig, `ISNULL` und nicht `COALESCE` zu verwenden. `COALESCE` ist keine deterministische Funktion. Daher lässt das Ergebnis des Ausdrucks immer NULL-Werte zu. `ISNULL` ist anders. ISNULL ist deterministisch. Wenn der zweite Teil der `ISNULL`-Funktion eine Konstante oder ein Literal ist, ergibt sich daraus der Wert NOT NULL.

Dieser Tipp eignet sich nicht nur zum Sicherstellen der Integrität von Berechnungen. Er ist auch wichtig für den Partitionswechsel von Tabellen. Angenommen, Sie haben diese Tabelle als Fakt definiert:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Allerdings ist das Wertfeld ein berechneter Ausdruck und kein Teil der Quelldaten.

Um Ihr partitioniertes Dataset zu erstellen, sollten Sie Folgendes tun:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

Die Abfrage sollte ganz normal ausgeführt werden. Das Problem tritt auf, wenn Sie versuchen, den Partitionswechsel durchzuführen. Die Tabellendefinitionen stimmen nicht überein. Damit die Tabellendefinitionen übereinstimmen, muss die CTAS-Anweisung geändert werden.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Sie sehen, dass die Typkonsistenz und das Aufrechterhalten von NULL-Zulässigkeitseigenschaften für CTAS bewährte Methoden für gute Softwareentwicklung sind. Sie können somit die Integrität in Ihren Berechnungen aufrecht erhalten und gleichzeitig sicherstellen, dass Partitionswechsel möglich sind.

### <a name="n-create-an-ordered-clustered-columnstore-index-with-maxdop-1"></a>N. Erstellen eines sortierten gruppierten Columnstore-Index mit MAXDOP 1  
```sql
CREATE TABLE Table1 WITH (DISTRIBUTION = HASH(c1), CLUSTERED COLUMNSTORE INDEX ORDER(c1) )
AS SELECT * FROM ExampleTable
OPTION (MAXDOP 1);
```

 
## <a name="see-also"></a>Weitere Informationen  
 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)   
 [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md)   
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)   
 [CREATE EXTERNAL TABLE AS SELECT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-as-select-transact-sql.md)   
 [CREATE TABLE &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-azure-sql-data-warehouse.md) [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
 [DROP EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-table-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER EXTERNAL TABLE &#40;Transact-SQL&#41;](./create-external-table-transact-sql.md)  
