---
description: CREATE TABLE (Transact-SQL)
title: CREATE TABLE (Transact-SQL)
ms.custom: ''
ms.date: 09/22/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FILESTREAM_TSQL
- TABLE
- CREATE_TABLE_TSQL
- CREATE TABLE
- FILESTREAM
- TABLE_TSQL
- FILESTREAM_ON
- FILESTREAM_ON_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CHECK constraints
- global temporary tables [SQL Server]
- local temporary tables [SQL Server]
- size [SQL Server], tables
- UNIQUE constraints [SQL Server], creating
- constraints [SQL Server], columns
- maximum number of indexes per table
- number of tables per database
- number of indexes per table
- table creation [SQL Server], CREATE TABLE
- temporary tables [SQL Server], creating
- table size [SQL Server]
- nullability [SQL Server]
- partitioned tables [SQL Server], creating
- DEFAULT definition
- null values [SQL Server], columns
- number of rows per table
- maximum number of columns per table
- FOREIGN KEY constraints, creating
- PRIMARY KEY constraints
- number of bytes per row
- CREATE TABLE statement
- number of columns per table
- maximum number of bytes per row
- data retention policy
ms.assetid: 1e068443-b9ea-486a-804f-ce7b6e048e8b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 807508fcd1d89abcd37bee18177c63ba9b13bde4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340010"
---
# <a name="create-table-transact-sql"></a>CREATE TABLE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Erstellt eine neue Tabelle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

> [!NOTE]
> Informationen zur [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]-Syntax finden Sie unter [CREATE TABLE ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/statements/create-table-azure-sql-data-warehouse.md).

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="simple-syntax"></a>Einfache Syntax

```syntaxsql
-- Simple CREATE TABLE Syntax (common if not using options)
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( { <column_definition> } [ ,...n ] )
[ ; ]
```

## <a name="full-syntax"></a>Vollständige Syntax

```syntaxsql
-- Disk-Based CREATE TABLE Syntax
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    [ AS FileTable ]
    ( {   <column_definition>
        | <computed_column_definition>
        | <column_set_definition>
        | [ <table_constraint> ] [ ,... n ]
        | [ <table_index> ] }
          [ ,...n ]
          [ PERIOD FOR SYSTEM_TIME ( system_start_time_column_name
             , system_end_time_column_name ) ]
      )
    [ ON { partition_scheme_name ( partition_column_name )
           | filegroup
           | "default" } ]
    [ TEXTIMAGE_ON { filegroup | "default" } ]
    [ FILESTREAM_ON { partition_scheme_name
           | filegroup
           | "default" } ]
    [ WITH ( <table_option> [ ,...n ] ) ]
[ ; ]
  
<column_definition> ::=
column_name <data_type>
    [ FILESTREAM ]
    [ COLLATE collation_name ]
    [ SPARSE ]
    [ MASKED WITH ( FUNCTION = ' mask_function ') ]
    [ [ CONSTRAINT constraint_name ] DEFAULT constant_expression ]
    [ IDENTITY [ ( seed,increment ) ]
    [ NOT FOR REPLICATION ]
    [ GENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] ]
    [ NULL | NOT NULL ]
    [ ROWGUIDCOL ]
    [ ENCRYPTED WITH
        ( COLUMN_ENCRYPTION_KEY = key_name ,
          ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } ,
          ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ) ]
    [ <column_constraint> [, ...n ] ]
    [ <column_index> ]
  
<data_type> ::=
[ type_schema_name . ] type_name
    [ ( precision [ , scale ] | max |
        [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]
  
<column_constraint> ::=
[ CONSTRAINT constraint_name ]
{     { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        [
            WITH FILLFACTOR = fillfactor
          | WITH ( < index_option > [ , ...n ] )
        ]
        [ ON { partition_scheme_name ( partition_column_name )
            | filegroup | "default" } ]
  
  | [ FOREIGN KEY ]
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ NOT FOR REPLICATION ]
  
  | CHECK [ NOT FOR REPLICATION ] ( logical_expression )
}
  
<column_index> ::=
 INDEX index_name [ CLUSTERED | NONCLUSTERED ]
    [ WITH ( <index_option> [ ,... n ] ) ]
    [ ON { partition_scheme_name (column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
<computed_column_definition> ::=
column_name AS computed_column_expression
[ PERSISTED [ NOT NULL ] ]
[
    [ CONSTRAINT constraint_name ]
    { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        [
            WITH FILLFACTOR = fillfactor
          | WITH ( <index_option> [ , ...n ] )
        ]
        [ ON { partition_scheme_name ( partition_column_name )
        | filegroup | "default" } ]
  
    | [ FOREIGN KEY ]
        REFERENCES referenced_table_name [ ( ref_column ) ]
        [ ON DELETE { NO ACTION | CASCADE } ]
        [ ON UPDATE { NO ACTION } ]
        [ NOT FOR REPLICATION ]
  
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )
]
  
<column_set_definition> ::=
column_set_name XML COLUMN_SET FOR ALL_SPARSE_COLUMNS
  
< table_constraint > ::=
[ CONSTRAINT constraint_name ]
{
    { PRIMARY KEY | UNIQUE }
        [ CLUSTERED | NONCLUSTERED ]
        (column [ ASC | DESC ] [ ,...n ] )
        [
            WITH FILLFACTOR = fillfactor
           |WITH ( <index_option> [ , ...n ] )
        ]
        [ ON { partition_scheme_name (partition_column_name)
            | filegroup | "default" } ]
    | FOREIGN KEY
        ( column [ ,...n ] )
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]
        [ ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT } ]
        [ NOT FOR REPLICATION ]
    | CHECK [ NOT FOR REPLICATION ] ( logical_expression )

< table_index > ::=
{  
    {  
      INDEX index_name  [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ]
         (column_name [ ASC | DESC ] [ ,... n ] )
    | INDEX index_name CLUSTERED COLUMNSTORE
    | INDEX index_name [ NONCLUSTERED ] COLUMNSTORE (column_name [ ,... n ] )
    }
    [ WITH ( <index_option> [ ,... n ] ) ]
    [ ON { partition_scheme_name (column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
}

<table_option> ::=
{  
    [DATA_COMPRESSION = { NONE | ROW | PAGE }
      [ ON PARTITIONS ( { <partition_number_expression> | <range> }
      [ , ...n ] ) ]]
    [ FILETABLE_DIRECTORY = <directory_name> ]
    [ FILETABLE_COLLATE_FILENAME = { <collation_name> | database_default } ]
    [ FILETABLE_PRIMARY_KEY_CONSTRAINT_NAME = <constraint_name> ]
    [ FILETABLE_STREAMID_UNIQUE_CONSTRAINT_NAME = <constraint_name> ]
    [ FILETABLE_FULLPATH_UNIQUE_CONSTRAINT_NAME = <constraint_name> ]
    [ SYSTEM_VERSIONING = ON [ ( HISTORY_TABLE = schema_name . history_table_name
        [, DATA_CONSISTENCY_CHECK = { ON | OFF } ] ) ] ]
    [ REMOTE_DATA_ARCHIVE =
      {
          ON [ ( <table_stretch_options> [,...n] ) ]
        | OFF ( MIGRATION_STATE = PAUSED )
      }
    ]   
    [ DATA_DELETION = ON  
          {( 
             FILTER_COLUMN = column_name,   
             RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS 
                              | MONTH | MONTHS | YEAR | YEARS }
        )}  
     ]
}
  
<table_stretch_options> ::=
{  
    [ FILTER_PREDICATE = { null | table_predicate_function } , ]
      MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }
 }   
  
<index_option> ::=
{
    PAD_INDEX = { ON | OFF }
  | FILLFACTOR = fillfactor
  | IGNORE_DUP_KEY = { ON | OFF }
  | STATISTICS_NORECOMPUTE = { ON | OFF }
  | STATISTICS_INCREMENTAL = { ON | OFF }
  | ALLOW_ROW_LOCKS = { ON | OFF }
  | ALLOW_PAGE_LOCKS = { ON | OFF }
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
  | COMPRESSION_DELAY= {0 | delay [Minutes]}
  | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }
       [ ON PARTITIONS ( { <partition_number_expression> | <range> }
       [ , ...n ] ) ]
}
<range> ::=
<partition_number_expression> TO <partition_number_expression>
```

```
-- Memory optimized CREATE TABLE Syntax
CREATE TABLE
    { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( { <column_definition>
    | [ <table_constraint> ] [ ,... n ]
    | [ <table_index> ]
      [ ,... n ] }
      [ PERIOD FOR SYSTEM_TIME ( system_start_time_column_name
        , system_end_time_column_name ) ]
)
    [ WITH ( <table_option> [ ,... n ] ) ]
 [ ; ]

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ GENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] ]
    [ NULL | NOT NULL ]
[
    [ CONSTRAINT constraint_name ] DEFAULT memory_optimized_constant_expression ]
    | [ IDENTITY [ ( 1, 1 ) ]
]
    [ <column_constraint> ]
    [ <column_index> ]
  
<data_type> ::=
 [type_schema_name . ] type_name [ (precision [ , scale ]) ]

<column_constraint> ::=
 [ CONSTRAINT constraint_name ]
{
  { PRIMARY KEY | UNIQUE }
      {   NONCLUSTERED
        | NONCLUSTERED HASH WITH (BUCKET_COUNT = bucket_count)
      }
  | [ FOREIGN KEY ]
        REFERENCES [ schema_name . ] referenced_table_name [ ( ref_column ) ]
  | CHECK ( logical_expression )
}
  
< table_constraint > ::=
 [ CONSTRAINT constraint_name ]
{
   { PRIMARY KEY | UNIQUE }
     {
       NONCLUSTERED (column [ ASC | DESC ] [ ,... n ])
       | NONCLUSTERED HASH (column [ ,... n ] ) WITH ( BUCKET_COUNT = bucket_count )
                    }
    | FOREIGN KEY
        ( column [ ,...n ] )
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]
    | CHECK ( logical_expression )
}

<column_index> ::=
  INDEX index_name
{ [ NONCLUSTERED ] | [ NONCLUSTERED ] HASH WITH (BUCKET_COUNT = bucket_count)}

<table_index> ::=
  INDEX index_name
{   [ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count)
  | [ NONCLUSTERED ] (column [ ASC | DESC ] [ ,... n ] )
      [ ON filegroup_name | default ]
  | CLUSTERED COLUMNSTORE [WITH ( COMPRESSION_DELAY = {0 | delay [Minutes]})]
      [ ON filegroup_name | default ]
  
}
  
<table_option> ::=
{  
    MEMORY_OPTIMIZED = ON
  | DURABILITY = {SCHEMA_ONLY | SCHEMA_AND_DATA}
  | SYSTEM_VERSIONING = ON [ ( HISTORY_TABLE = schema_name . history_table_name
        [, DATA_CONSISTENCY_CHECK = { ON | OFF } ] ) ]
  
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

*database_name* Der Name der Datenbank, in der die Tabelle erstellt wird. *database_name* muss dem Namen einer vorhandenen Datenbank entsprechen. Wird *database_name* nicht angegeben, wird standardmäßig die aktuelle Datenbank verwendet. Der Anmeldename für die aktuelle Verbindung muss einer vorhandenen Benutzer-ID in der durch *database_name* angegebenen Datenbank zugeordnet sein. Diese Benutzer-ID muss über CREATE TABLE-Berechtigungen verfügen.

*schema_name* Der Name des Schemas, zu dem die neue Tabelle gehört.

*table_name* Der Name der neuen Tabelle. Tabellennamen müssen die Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) erfüllen. *table_name* kann höchstens 128 Zeichen aufweisen, ausgenommen lokale temporäre Tabellennamen (Namen mit einem Nummernzeichen (#) als Präfix), bei denen maximal 116 Zeichen zulässig sind.

AS-Dateitabelle **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher).

Erstellt die neue Tabelle als FileTable. Sie geben keine Spalten an, da eine FileTable über ein festes Schema verfügt. Weitere Informationen finden Sie unter [FileTables](../../relational-databases/blob/filetables-sql-server.md).

*column_name*
*computed_column_expression* Ein Ausdruck, der den Wert einer berechneten Spalte definiert. Eine berechnete Spalte ist eine virtuelle Spalte, die nicht physisch in der Tabelle gespeichert ist, es sei denn, die Spalte wurde (mit PERSISTED) als persistente Spalte markiert. Die Spalte wird anhand eines Ausdrucks berechnet, der andere Spalten in derselben Tabelle verwendet. Eine berechnete Spalte kann beispielsweise die Definition **cost** AS **price** \* **qty** aufweisen. Der Ausdruck kann der Name einer nicht berechneten Spalte, eine Konstante, eine Funktion, eine Variablen oder eine beliebige durch einen oder mehrere Operatoren verbundene Kombination der genannten Möglichkeiten sein. Der Ausdruck darf keine Unterabfrage sein oder Aliasdatentypen enthalten.

Berechnete Spalten können in SELECT-Listen, WHERE-Klauseln, ORDER BY-Klauseln oder an anderen Stellen verwendet werden, an denen reguläre Ausdrücke verwendet werden können. Dabei gelten folgende Ausnahmen:

- Berechnete Spalten müssen als PERSISTED gekennzeichnet sein, um Teil einer FOREIGN KEY- oder CHECK-Einschränkung zu sein.
- Eine berechnete Spalte kann als Schlüsselspalte in einem Index oder als Teil einer PRIMARY KEY- oder UNIQUE-Einschränkung verwendet werden, wenn der Wert der berechneten Spalte durch einen deterministischen Ausdruck definiert ist und der Datentyp des Ergebnisses in Indexspalten zulässig ist.

   Wenn eine Tabelle beispielsweise die beiden ganzzahligen Spalten **a** und **b** enthält, kann für die berechnete Spalte **a+b** ein Index erstellt werden. Für die berechnete Spalte **a+DATEPART(dd, GETDATE())** ist dies jedoch nicht möglich, da sich der Wert in späteren Aufrufen möglicherweise ändert.

- Eine berechnete Spalte kann nicht das Ziel einer INSERT- oder UPDATE-Anweisung sein.

> [!NOTE]
> Jede Zeile in einer Tabelle kann unterschiedliche Werte in den Spalten aufweisen, die für eine berechnete Spalte herangezogen werden, daher enthält die berechnete Spalte möglicherweise nicht in jeder Zeile den gleichen Wert.

Die NULL-Zulässigkeit berechneter Spalten wird automatisch von [!INCLUDE[ssDE](../../includes/ssde-md.md)] auf Grundlage der verwendeten Ausdrücke bestimmt. Für das Ergebnis der meisten Ausdrücke wird die NULL-Zulässigkeit angenommen, und zwar auch dann, wenn nur Spalten vorhanden sind, die keine NULL-Werte zulassen, da mögliche Unter- oder Überläufe ebenfalls zu NULL-Ergebnissen führen. Verwenden Sie die Funktion `COLUMNPROPERTY` mit der Eigenschaft **AllowsNull**, um die NULL-Zulässigkeit von berechneten Spalten in einer Tabelle zu untersuchen. Ein Ausdruck, der NULL-Werte zulässt, kann nur durch die Angabe von `ISNULL` mit der Konstante *check_expression* in einen Ausdruck umgewandelt werden, der keine NULL-Werte zulässt. Die Konstante ist ein Wert ungleich NULL, durch den jedes NULL-Ergebnis ersetzt wird. Für berechnete Spalten, die auf CLR-benutzerdefinierten Typausdrücken (Common Language Runtime) basieren, ist die REFERENCES-Berechtigung für den Typ erforderlich.

PERSISTED Gibt an, dass das [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] die berechneten Werte physisch in der Tabelle speichert und die Werte aktualisiert, wenn Spalten, von denen die berechnete Spalte abhängt, aktualisiert werden. Wenn Sie eine berechnete Spalte mit `PERSISTED` als persistente Spalte markieren, können Sie einen Index für eine berechnete Spalte erstellen, die deterministisch, jedoch nicht genau ist. Weitere Informationen finden Sie unter [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md). Sämtliche berechnete Spalten, die als Partitionierungsspalten einer partitionierten Tabelle verwendet werden, müssen ausdrücklich mit `PERSISTED` gekennzeichnet sein. *computed_column_expression* muss deterministisch sein, wenn `PERSISTED` angegeben wird.

ON { *partition_scheme* | *filegroup* |  **"default"** } Gibt das Partitionsschema oder die Dateigruppe an, in der die Tabelle gespeichert wird. Wenn *partition_scheme* angegeben wird, soll die Tabelle eine partitionierte Tabelle sein, deren Partitionen in einem Satz aus einer oder mehreren in *partition_scheme* angegebenen Dateigruppen gespeichert werden. Wenn *filegroup* angegeben ist, wird die Tabelle in der genannten Dateigruppe gespeichert. Die Dateigruppe muss in der Datenbank vorhanden sein. Wenn **"default"** angegeben oder ON nicht angegeben ist, wird die Tabelle in der Standarddateigruppe gespeichert. Der in CREATE TABLE angegebene Speichermechanismus einer Tabelle kann nachfolgend nicht mehr geändert werden.

ON {*partition_scheme* | *filegroup* |  **"default"** } kann auch in den Einschränkungen PRIMARY KEY und UNIQUE angegeben werden. Diese Einschränkungen erstellen Indizes. Wenn *filegroup* angegeben ist, wird der Index in der genannten Dateigruppe gespeichert. Wenn **"default"** angegeben oder ON überhaupt nicht angegeben ist, wird der Index in derselben Dateigruppe wie die Tabelle gespeichert. Wenn die Einschränkungen `PRIMARY KEY` oder `UNIQUE` einen gruppierten Index erstellt, werden die Datenseiten für die Tabelle in derselben Dateigruppe wie der Index gespeichert. Wenn `CLUSTERED` angegeben oder ein gruppierter Index anderweitig durch die Einschränkung erstellt wird, und ein Wert für *partition_scheme* angegeben wird, der von der Angabe für *partition_scheme* oder *filegroup* der Tabellendefinition abweicht (oder umgekehrt), wird nur die Einschränkungsdefinition berücksichtigt. Der andere Wert wird ignoriert.

> [!NOTE]
> In diesem Zusammenhang ist *default* kein Schlüsselwort. Es handelt sich dabei um einen Bezeichner für die Standarddateigruppe, der wie bei ON **"default"** oder ON **[** default **]** begrenzt sein muss. Wenn **"default"** angegeben wird, muss die Option `QUOTED_IDENTIFIER` für die aktuelle Sitzung auf ON festgelegt sein. Dies ist die Standardeinstellung. Weitere Informationen finden Sie unter [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).
>
> Nachdem Sie eine partitionierte Tabelle erstellt haben, erwägen Sie, die `LOCK_ESCALATION`-Option für die Tabelle auf `AUTO` festzulegen. Dies kann die Parallelität verbessern, indem die Sperren auf Partitionsebene (HoBT) statt auf Tabellenebene aktiviert werden. Weitere Informationen finden Sie unter [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).

TEXTIMAGE_ON { *filegroup*|  **"default"** } Gibt an, dass die Spalten vom Typ **text**, **ntext**, **image**, **xml**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** und eines CLR-benutzerdefinierten Typs (einschließlich „geometry“ und „geography“) in der angegebenen Dateigruppe gespeichert werden.

`TEXTIMAGE_ON` ist nicht zulässig, wenn die Tabelle keine Spalten für umfangreiche Werte enthält. `TEXTIMAGE_ON` darf nicht angegeben werden, wenn *partition_scheme* angegeben wird. Wenn **"default"** angegeben oder `TEXTIMAGE_ON` nicht angegeben wird, werden die Spalten für umfangreiche Werte in der Standarddateigruppe gespeichert. Die in `CREATE TABLE` angegebene Speicherung von Spaltendaten mit großen Werten kann nachfolgend nicht mehr geändert werden.

> [!NOTE]
> Varchar(max), nvarchar(max), varbinary(max), XML und große UDT-Werte werden bis zu einem Höchstwert von 8.000 Bytes direkt in der Datenzeile gespeichert, sofern der Wert die Größe des Datensatzes nicht überschreitet. Überschreitet der Wert die Größe des Datensatzes, wird ein Zeiger innerhalb der Zeilen gespeichert, während der Rest außerhalb der Zeilen im LOB-Speicherbereich gespeichert wird. Null (0) ist der Standardwert, der angibt, dass alle Werte direkt in der Datenzeile gespeichert werden.
>
> `TEXTIMAGE_ON` ändert nur den Speicherort des LOB-Speicherbereichs. Dies beeinträchtigt in Zeilen gespeicherte Daten nicht. Verwenden Sie große Werttypen von „sp_tableoption“ außerhalb der Zeilen, um den gesamten LOB-Wert außerhalb der Zeile zu speichern.
>
> In diesem Zusammenhang ist DEFAULT kein Schlüsselwort. Dabei handelt es sich um einen Bezeichner für die Standarddateigruppe, der wie in `TEXTIMAGE_ON "default"` oder `TEXTIMAGE_ON [default]` abgegrenzt sein muss. Wenn **"default"** angegeben wird, muss die Option `QUOTED_IDENTIFIER` für die aktuelle Sitzung auf ON festgelegt sein. Dies ist die Standardeinstellung. Weitere Informationen finden Sie unter [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

FILESTREAM_ON { *partition_scheme_name* | filegroup | **"** default **"** } **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] und höher) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] unterstützt `FILESTREAM` nicht.

Gibt die Dateigruppe für FILESTREAM-Daten an.

Wenn die Tabelle FILESTREAM-Daten enthält und partitioniert ist, muss die FILESTREAM_ON-Klausel eingeschlossen werden und ein Partitionsschema von FILESTREAM-Dateigruppen angeben. Dieses Partitionsschema muss die gleiche Partitionsfunktion und die gleichen Partitionsspalten wie das Partitionsschema der Tabelle verwenden. Andernfalls wird ein Fehler ausgelöst.

Wenn die Tabelle nicht partitioniert ist, kann die FILESTREAM-Spalte nicht partitioniert werden. Die FILESTREAM-Daten für die Tabelle müssen in einer einzigen Dateigruppe gespeichert werden. Diese Dateigruppe wird in der FILESTREAM_ON-Klausel angegeben.

Wenn die Tabelle nicht partitioniert und die `FILESTREAM_ON`-Klausel nicht angegeben ist, wird die FILESTREAM-Dateigruppe mit dem `DEFAULT`-Eigenschaftensatz verwendet. Wenn keine FILESTREAM-Dateigruppe vorhanden ist, wird ein Fehler ausgelöst.

Wie auch bei ON und `TEXTIMAGE_ON` kann der mit `CREATE TABLE` für `FILESTREAM_ON` festgelegte Wert nur in den folgenden Fällen geändert werden:

- Eine [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)-Anweisung konvertiert einen Heap in einen gruppierten Index. In diesem Fall kann eine andere FILESTREAM-Dateigruppe, ein anderes Partitionsschema oder NULL angegeben werden.
- Eine [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md)-Anweisung konvertiert einen gruppierten Index in einen Heap. In diesem Fall können eine andere FILESTREAM-Dateigruppe, ein anderes Partitionsschema oder **"default"** angegeben werden.

Für die Dateigruppe in der Klausel `FILESTREAM_ON <filegroup>` bzw. für die einzelnen FILESTREAM-Dateigruppen im Partitionsschema muss jeweils eine Datei definiert sein. Diese Datei muss mit einer [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)- oder [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)-Anweisung definiert werden, andernfalls wird ein Fehler ausgelöst.

Verwandte Themen zu FILESTREAM finden Sie unter [Binary Large Object (BLOB)-Daten](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md).

[ _type\_schema\_name_ **.** ] *type_name* Gibt den Datentyp der Spalte sowie das Schema an, zu dem er gehört. Für datenträgerbasierte Tabellen kann der Datentyp einer der folgenden sein:

- Ein Systemdatentyp.
- Ein Aliastyp, der auf einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemdatentyp basiert. Aliasdatentypen werden mit der `CREATE TYPE`-Anweisung erstellt, bevor sie in einer Tabellendefinition verwendet werden können. Die NULL- oder NOT NULL-Zuweisung für einen Aliasdatentyp lässt sich durch die entsprechende Angabe in einer `CREATE TABLE`-Anweisung überschreiben. Die Längenangabe kann jedoch nicht geändert werden, da die Länge eines Aliasdatentyps nicht in einer `CREATE TABLE`-Anweisung angegeben werden kann.
- Ein CLR-benutzerdefinierter Typ. CLR-benutzerdefinierte Typen werden mit der `CREATE TYPE`-Anweisung erstellt, bevor sie in einer Tabellendefinition verwendet werden können. Zum Erstellen einer Spalte, die auf einem CLR-benutzerdefinierten Typ basiert, ist die REFERENCES-Berechtigung für den Typ erforderlich.

Wenn *type_schema_name* nicht angegeben ist, verweist [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] auf *type_name* in der folgenden Reihenfolge:

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemdatentyp.
- Das Standardschema des aktuellen Benutzers in der aktuellen Datenbank
- Das **dbo** -Schema in der aktuellen Datenbank

Eine Liste unterstützter Systemtypen für speicheroptimierte Tabellen finden Sie unter [Unterstützte Datentypen für In-Memory OLTP](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md).

*precision* Die Genauigkeit für den angegebenen Datentyp. Weitere Informationen über gültige Genauigkeitswerte finden Sie unter [Genauigkeit, Dezimalstellen und Länge](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

*scale* Die Dezimalstellen für den angegebenen Datentyp. Weitere Informationen zu gültigen Dezimalstellenwerten finden Sie unter [Genauigkeit, Dezimalstellen und Länge](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

**max** Gilt nur für die Datentypen **varchar**, **nvarchar** und **varbinary** zum Speichern von 2^31 Byte an Zeichen- und Binärdaten sowie von 2^30 Byte an Unicode-Daten.

CONTENT Gibt an, dass jede Instanz des **xml**-Datentyps in *column_name* mehrere allgemeine Elemente enthalten kann. CONTENT gilt nur für den **xml**-Datentyp und kann nur angegeben werden, wenn *xml_schema_collection* ebenfalls angegeben ist. Wird der Parameter nicht angegeben, entspricht CONTENT dem Standardverhalten.

DOCUMENT Gibt an, dass jede Instanz des **xml**-Datentyps in *column_name* nur ein allgemeines Element enthalten kann. DOCUMENT gilt nur für den **xml**-Datentyp und kann nur angegeben werden, wenn *xml_schema_collection* ebenfalls angegeben ist.

*xml_schema_collection* Gilt nur für den **xml**-Datentyp zum Verknüpfen einer XML-Schemaauflistung mit diesem Typ. Vor der Typisierung einer **xml**-Spalte mit einem Schema muss das Schema zuerst mithilfe von [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md) in der Datenbank erstellt werden.

DEFAULT Gibt den Wert an, der für die Spalte bereitgestellt wird, wenn während des Einfügens kein Wert explizit angegeben wurde. DEFAULT-Definitionen können auf alle Spalten angewendet werden, mit Ausnahme der als **timestamp** definierten Spalten sowie von Spalten mit der `IDENTITY`-Eigenschaft. Wenn ein Standardwert für einen benutzerdefinierten Spaltentyp angegeben wird, sollte dieser Typ eine implizite Konvertierung von *constant_expression* in den benutzerdefinierten Typ unterstützen. DEFAULT-Definitionen werden entfernt, wenn die Tabelle gelöscht wird. Es kann nur ein konstanter Wert (z. B. eine Zeichenfolge), eine Skalarfunktion (entweder eine System-, eine benutzerdefinierte oder eine CLR-Funktion) oder NULL als Standardwert verwendet werden. Um die Kompatibilität mit früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aufrechtzuerhalten, ist es möglich, einer DEFAULT-Definition einen Einschränkungsnamen zuzuweisen.

*constant_expression* Eine Konstante, ein NULL-Wert oder eine Systemfunktion, die bzw. der als Standardwert für die Spalte verwendet wird.

*memory_optimized_constant_expression* Eine Konstante, ein NULL-Wert oder eine Systemfunktion, die bzw. der als Standardwert für die Spalte verwendet wird. Muss in systemintern kompilierten gespeicherten Prozeduren unterstützt werden. Weitere Informationen zu den integrierten Features in nativ kompilierten gespeicherten Prozeduren finden Sie unter [Unterstützte Features für nativ kompilierte T-SQL-Module](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).

IDENTITY Gibt an, dass es sich bei der neuen Spalte um eine Identitätsspalte handelt. Wenn eine neue Zeile zur Tabelle hinzugefügt wird, stellt [!INCLUDE[ssDE](../../includes/ssde-md.md)] einen eindeutigen, inkrementellen Wert für die Spalte bereit. Identitätsspalten werden üblicherweise zusammen mit PRIMARY KEY-Einschränkungen verwendet, um als eindeutiger Zeilenbezeichner für die Tabelle zu dienen. Die Eigenschaft `IDENTITY` kann folgenden Spalten zugewiesen werden: **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** oder **numeric(p,0)** . Es kann nur eine Identitätsspalte pro Tabelle erstellt werden. Gebundene Standardwerte und DEFAULT-Einschränkungen können nicht mit einer Identitätsspalte verwendet werden. Entweder müssen sowohl Ausgangswert als auch Schrittweite oder keines von beiden angegeben werden. Wurden Ausgangswert und inkrementeller Wert nicht angegeben, ist der Standardwert (1,1).

*seed* Der Wert, der für die erste in die Tabelle geladene Zeile verwendet wird.

*increment* Der Inkrementwert, der zum Identitätswert der zuvor geladenen Zeile addiert wird.

NOT FOR REPLICATION In der Anweisung `CREATE TABLE` kann die Klausel `NOT FOR REPLICATION` für die IDENTITY-Eigenschaft, für FOREIGN KEY-Einschränkungen und für CHECK-Einschränkungen angegeben werden. Wenn diese Klausel für die `IDENTITY`-Eigenschaft angegeben wird, werden Werte in Identitätsspalten nicht inkrementiert, wenn Replikations-Agents Einfügungen ausführen. Wenn diese Klausel für eine Einschränkung angegeben wird, wird die Einschränkung nicht erzwungen, wenn Replikations-Agents Einfüge-, Update- oder Löschvorgänge ausführen.

GENERATED ALWAYS AS ROW { START | END } [ HIDDEN ] [ NOT NULL ] **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt an, dass eine angegebene `datetime2`-Spalte vom System verwendet wird, um entweder die Startzeit oder die Endzeit aufzuzeichnen, für die ein Datensatz gültig ist. Die Spalte muss als `NOT NULL` definiert werden. Wenn Sie versuchen, die Spalte als `NULL` zu definieren, löst das System einen Fehler aus. Wenn Sie NOT NULL nicht explizit für eine Zeitraumspalte angeben, definiert das System die Spalte standardmäßig als `NOT NULL`. Verwenden Sie dieses Argument mit den Argumenten `PERIOD FOR SYSTEM_TIME` und `WITH SYSTEM_VERSIONING = ON`, um die Systemversionsverwaltung für eine Tabelle zu aktivieren. Weitere Informationen finden Sie unter [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

Sie können eine oder beide Zeitraumspalten mit dem Flag **HIDDEN** kennzeichnen, um diese Spalten implizit auszublenden, sodass **SELECT \* FROM** _`<table>`_ für diese Spalten keinen Wert zurückgibt. Standardmäßig sind Zeitraumspalten nicht ausgeblendet. Damit sie verwendet werden können, müssen ausgeblendete Spalten explizit in allen Abfragen eingeschlossen werden, die direkt auf die temporale Tabelle verweisen. Zum Ändern des Attributs **HIDDEN** einer vorhandenen Zeitraumspalte, muss **PERIOD** gelöscht und mit einem anderen Flag neu erstellt werden.

INDEX *index_name* [CLUSTERED | NONCLUSTERED] (*column_name* [ASC | DESC] [,... *n*] ) **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt an, dass ein Index in der Tabelle erstellt werden soll. Dies kann ein gruppierter oder ein nicht gruppierter Index sein. Der Index enthält die aufgelisteten Spalten und sortiert die Daten in aufsteigender oder absteigender Reihenfolge.

INDEX *index_name* CLUSTERED COLUMNSTORE **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt an, dass die gesamte Tabelle im Spaltenformat mit einem Columnstore-Index gespeichert werden soll. Dies beinhaltet immer alle Spalten in der Tabelle. Die Daten werden nicht in alphabetischer oder numerischer Reihenfolge sortiert, da die Zeilen zugunsten der Vorteile der Columnstore-Komprimierung organisiert sind.

INDEX *index_name* [ NONCLUSTERED ] COLUMNSTORE (*column_name* [ ,... *n* ] ) **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt an, dass ein nicht gruppierter Columnstore-Index in der Tabelle erstellt werden soll. Die zugrunde liegende Tabelle kann ein Rowstore-Heap, gruppierter Index oder gruppierter Columnstore-Index sein. Eine zweite Kopie der Daten für die Spalten im Index wird immer gespeichert, wenn ein nicht gruppierter Columnstore-Index erstellt wird.

Der nicht gruppierte Columnstore-Index wird als gruppierter Columnstore-Index gespeichert und verwaltet. Er wird „nicht gruppierter Columnstore-Index“ genannt, da er als sekundärer Index in einer Tabelle fungiert und seine Spalten begrenzt sein können.

ON _partition\_scheme\_name_ **(** _column\_name_ **)** Gibt das Partitionsschema an, das die Dateigruppen definiert, denen die Partitionen eines partitionierten Index zugeordnet werden. Das Partitionsschema muss bereits durch Ausführen von [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) oder [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md) in der Datenbank vorhanden sein. *column_name* gibt die Spalte an, auf deren Grundlage ein partitionierter Index partitioniert wird. Diese Spalte muss mit dem Datentyp, der Länge und der Genauigkeit des Arguments der Partitionsfunktion übereinstimmen, die *partition_scheme_name* verwendet. *column_name* ist nicht auf Spalten in der Indexdefinition beschränkt. Es können beliebige Spalten der Basistabelle angegeben werden, mit der Ausnahme, dass *column_name* beim Partitionieren von UNIQUE-Indizes aus den Spalten ausgewählt werden muss, die als eindeutige Schlüssel verwendet werden. Mit dieser Einschränkung kann [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Eindeutigkeit der Schlüsselwerte in nur einer einzigen Partition überprüfen.

> [!NOTE]
> Beim Partitionieren eines nicht eindeutigen gruppierten Index fügt [!INCLUDE[ssDE](../../includes/ssde-md.md)] standardmäßig die Partitionierungsspalte zu der Liste der gruppierten Indexschlüssel hinzu, sofern sie dort noch nicht angegeben wurde. Beim Partitionieren eines nicht eindeutigen, nicht gruppierten Indexes fügt [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Partitionierungsspalte als (eingeschlossene) Nichtschlüsselspalte des Indexes hinzu, sofern sie noch nicht angegeben wurde.

Wenn *partition_scheme_name* oder *filegroup* bei einer partitionierten Tabelle nicht angegeben werden, wird der Index in demselben Partitionsschema platziert und verwendet dieselbe Partitionierungsspalte wie die zugrunde liegende Tabelle.

> [!NOTE]
> Sie können kein Partitionierungsschema für einen XML-Index angeben. Beim Partitionieren der Basistabelle verwendet der XML-Index dasselbe Partitionsschema wie die Tabelle.

Weitere Informationen zur Partitionierung von Indizes finden Sie unter [Partitionierte Tabellen und Indizes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

ON *filegroup_name* Erstellt den angegebenen Index für die angegebene Dateigruppe. Wenn kein Speicherort angegeben und die Tabelle oder Sicht nicht partitioniert ist, verwendet der Index dieselbe Dateigruppe wie die zugrunde liegende Tabelle oder Sicht. Die Dateigruppe muss bereits vorhanden sein.

ON **"default"** Erstellt den angegebenen Index für die Standarddateigruppe

Die Benennung default ist in diesem Kontext kein Schlüsselwort. Es handelt sich dabei um einen Bezeichner für die Standarddateigruppe, der wie bei ON **"default"** oder ON **[default]** begrenzt sein muss. Wenn "default" angegeben wird, muss die Option `QUOTED_IDENTIFIER` für die aktuelle Sitzung auf ON festgelegt sein. Dies ist die Standardeinstellung. Weitere Informationen finden Sie unter [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

[ FILESTREAM_ON { *filestream_filegroup_name* | *partition_scheme_name* | "NULL" } ] **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] und höher).

Gibt die Platzierung der FILESTREAM-Daten für die Tabelle an, wenn ein gruppierter Index erstellt wird. Die FILESTREAM_ON-Klausel lässt zu, dass FILESTREAM-Daten in eine andere FILESTREAM-Dateigruppe oder ein anderes Partitionsschema verschoben werden.

*filestream_filegroup_name* ist der Name einer FILESTREAM-Dateigruppe. Für die Dateigruppe muss eine Datei mit einer [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)-Anweisung oder einer [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)-Anweisung definiert worden sein, andernfalls wird ein Fehler ausgelöst.

Wenn die Tabelle partitioniert ist, muss die `FILESTREAM_ON`-Klausel eingeschlossen werden und ein Partitionsschema von FILESTREAM-Dateigruppen angeben, das die gleiche Partitionsfunktion und die gleichen Partitionsspalten wie das Partitionsschema der Tabelle enthält. Andernfalls wird ein Fehler ausgelöst.

Wenn die Tabelle nicht partitioniert ist, kann die FILESTREAM-Spalte nicht partitioniert werden. Die FILESTREAM-Daten für die Tabelle müssen in einer einzigen Dateigruppe gespeichert werden, die in der `FILESTREAM_ON`-Klausel angegeben wird.

`FILESTREAM_ON NULL` kann in einer `CREATE INDEX`-Anweisung angegeben werden, wenn ein gruppierter Index erstellt wird und die Tabelle keine FILESTREAM-Spalte enthält.

Weitere Informationen finden Sie unter [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md).

ROWGUIDCOL Gibt an, dass die neue Spalte eine Spalte mit Zeilen-GUIDs ist. Nur eine **uniqueidentifier**-Spalte pro Tabelle kann als ROWGUIDCOL-Spalte gekennzeichnet werden. Nach der Anwendung der ROWGUIDCOL-Eigenschaft kann mit `$ROWGUID` auf die Spalte verwiesen werden. Die ROWGUIDCOL-Eigenschaft kann nur einer **uniqueidentifier**-Spalte zugewiesen werden. Spalten eines benutzerdefinierten Datentyps können nicht mit ROWGUIDCOL gekennzeichnet werden.

Die ROWGUIDCOL-Eigenschaft erzwingt keine Eindeutigkeit der in der Spalte gespeicherten Werte. ROWGUIDCOL erzeugt auch nicht automatisch Werte für neue Zeilen, die in die Tabelle eingefügt werden. Zum Generieren eindeutiger Werte für jede Spalte verwenden Sie entweder die Funktion [NEWID](../../t-sql/functions/newid-transact-sql.md) oder [NEWSEQUENTIALID](../../t-sql/functions/newsequentialid-transact-sql.md) in [INSERT](../../t-sql/statements/insert-transact-sql.md)-Anweisungen, oder verwenden Sie diese Funktionen als Standardwert für die Spalte.

ENCRYPTED WITH Gibt Verschlüsselungsspalten mit dem Feature [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) an.

COLUMN_ENCRYPTION_KEY = *key_name* Gibt Spaltenverschlüsselungsschlüssel an. Weitere Informationen finden Sie unter [CREATE COLUMN ENCRYPTION KEY](../../t-sql/statements/create-column-encryption-key-transact-sql.md).

ENCRYPTION_TYPE = { DETERMINISTIC | RANDOMIZED } Bei der **deterministischen Verschlüsselung** wird eine Methode verwendet, die immer denselben verschlüsselten Wert für jeden angegebenen Nur-Text-Wert generiert. Die Verwendung der deterministischen Verschlüsselung ermöglicht die Suche mit einer Gleichheitsüberprüfung, das Gruppieren und das Verknüpfen von Tabellen mit Gleichheitsjoins, basierend auf verschlüsselten Werten. Jedoch erlaubt sie nicht autorisierten Benutzern möglicherweise, Informationen zu verschlüsselten Werten zu erraten, indem sie die Muster in den verschlüsselten Spalten untersuchen. Das Verknüpfen zweier Tabellen mit deterministisch verschlüsselten Spalten ist nur möglich, wenn die Spalten mit demselben Spaltenverschlüsselungsschlüssel verschlüsselt sind. Die deterministische Verschlüsselung muss eine Spaltensortierung mit einer binary2-Sortierreihenfolge für Zeichenspalten verwenden.

Die **zufällige Verschlüsselung** verwendet eine Methode, die Daten in einer weniger vorhersagbaren Weise verschlüsselt. Die Verschlüsselung nach dem Zufallsprinzip ist zwar sicherer, verhindert jedoch jegliche Berechnungen und Indizierungen in verschlüsselten Spalten, es sei denn, Ihre SQL Server-Instanz unterstützt Always Encrypted mit Secure Enclaves. Weitere Einzelheiten finden Sie unter [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

Wenn Sie Always Encrypted (ohne Secure Enclaves) verwenden, verwenden Sie die deterministische Verschlüsselung für Spalten, die mit Parametern oder Gruppierungsparametern durchsucht werden sollen (z. B. eine Behörden-ID-Nummer). Verwenden Sie die zufällige Datenverschlüsselung für Daten (z. B. Kreditkartennummern), die nicht mit anderen Datensätzen gruppiert oder in Jointabellen verwendet werden und nach denen nicht gesucht wird, wenn andere Spalten verwendet werden (z. B. Transaktionsnummern), um die Zeile zu suchen, die die betreffende verschlüsselte Spalte enthält.

Wenn Sie Always Encrypted mit Secure Enclaves verwenden, wird eine Verschlüsselung nach dem Zufallsprinzip empfohlen.

Spalten müssen einen qualifizierenden Datentyp aufweisen.

ALGORITHM **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher)

Muss **'AEAD_AES_256_CBC_HMAC_SHA_256'** sein.

Weitere Informationen einschließlich Featureeinschränkungen finden Sie unter [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md).

SPARSE Gibt an, dass die Spalte eine Sparsespalte ist. Der Speicher für Sparsespalten ist für NULL-Werte optimiert. Spalten mit geringer Dichte können nicht als NOT NULL festgelegt werden. Weitere Einschränkungen und Informationen zu Sparsespalten finden Sie unter [Verwenden von Sparsespalten](../../relational-databases/tables/use-sparse-columns.md).

ADD MASKED WITH ( FUNCTION = ' *mask_function* ') **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher)

Gibt eine dynamische Datenmaske an. *mask_function* ist der Name der Maskierungsfunktion mit den entsprechenden Parametern. Es stehen vier Optionen zur Verfügung:

- default()
- email()
- partial()
- random()

Weitere Informationen zu Funktionsparametern finden Sie im Artikel zur [dynamischen Datenmaskierung](../../relational-databases/security/dynamic-data-masking.md).

FILESTREAM **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] und höher)

Gilt nur für **varbinary(max)** -Spalten. Gibt den FILESTREAM-Speicher für die **varbinary(max)** -BLOB-Daten an.

Die Tabelle muss auch eine Spalte mit dem Datentyp **uniqueidentifier** aufweisen, der das ROWGUIDCOL-Attribut enthält. Diese Spalte darf keine NULL-Werte zulassen und muss eine UNIQUE- oder eine PRIMARY KEY-Einschränkung für einzelne Spalten enthalten. Der GUID-Wert für die Spalte muss entweder beim Einfügen von Daten von einer Anwendung oder durch eine DEFAULT-Einschränkung mit der NEWID ()-Funktion bereitgestellt werden.

Die Spalte ROWGUIDCOL kann nicht gelöscht, und die zugehörigen Einschränkungen können nicht geändert werden, wenn für die Tabelle eine FILESTREAM-Spalte definiert ist. Die Spalte ROWGUIDCOL kann nur gelöscht werden, nachdem die letzte FILESTREAM-Spalte gelöscht wurde.

Wenn das FILESTREAM-Speicherattribut für eine Spalte angegeben wird, werden alle Werte dieser Spalte in einem FILESTREAM-Datencontainer des Dateisystems gespeichert.

COLLATE *collation_name* Gibt die Sortierung für die Spalte an. Als Sortierungsname kann entweder der Name einer Windows-Sortierreihenfolge oder ein SQL-Sortierungsname verwendet werden. *collation_name* ist nur für Spalten mit den folgenden Datentypen anwendbar: **char**, **varchar**, **text**, **nchar**, **nvarchar** und **ntext**. Wenn collation_name nicht angegeben ist, wird der Spalte die Sortierung des benutzerdefinierten Datentyps zugewiesen, wenn es sich um eine Spalte von einem benutzerdefinierten Datentyp handelt, oder es wird die Standardsortierung der Datenbank zugewiesen.

Weitere Informationen zu den Windows- und SQL-Sortierungsnamen finden Sie unter [Name der Windows-Sortierung](../../t-sql/statements/windows-collation-name-transact-sql.md) und [SQL Server-Sortierungsname](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

Weitere Informationen finden Sie unter [COLLATE](~/t-sql/statements/collations.md).

CONSTRAINT Ein optionales Schlüsselwort, das den Anfang der Definition einer PRIMARY KEY-, NOT NULL-, UNIQUE-, FOREIGN KEY- oder CHECK-Einschränkung anzeigt.

*constraint_name* Der Name einer Einschränkung. Einschränkungsnamen müssen innerhalb des Schemas, zu dem die Tabelle gehört, eindeutig sein.

NULL | NOT NULL Bestimmt, ob NULL-Werte in der Spalte zulässig sind. NULL ist genau genommen keine Einschränkung, kann jedoch wie NOT NULL verwendet werden. NOT NULL kann nur für berechnete Spalten angegeben werden, wenn auch PERSISTED angegeben ist.

PRIMARY KEY Eine Einschränkung, die die Entitätsintegrität für eine bestimmte Spalte (oder Spalten) durch einen eindeutigen Index erzwingt. Es kann nur eine PRIMARY KEY-Einschränkung pro Tabelle erstellt werden.

UNIQUE Eine Einschränkung, die Entitätsintegrität für eine angegebene Spalte (oder Spalten) durch einen eindeutigen Index bereitstellt. Eine Tabelle kann mehrere UNIQUE-Einschränkungen haben.

CLUSTERED | NONCLUSTERED Gibt an, dass ein gruppierter oder nicht gruppierter Index für die PRIMARY KEY- oder UNIQUE-Einschränkung erstellt wird. Für PRIMARY KEY-Einschränkungen wird standardmäßig CLUSTERED verwendet; für UNIQUE-Einschränkungen wird standardmäßig NONCLUSTERED verwendet.

In einer `CREATE TABLE`-Anweisung kann CLUSTERED nur für eine einzige Einschränkung angegeben werden. Wenn Sie CLUSTERED für eine UNIQUE-Einschränkung angeben und außerdem eine PRIMARY KEY-Einschränkung angeben, wird für PRIMARY KEY standardmäßig NONCLUSTERED verwendet.

FOREIGN KEY REFERENCES Eine Einschränkung, die referenzielle Integrität für die Daten in der Spalte oder den Spalten bereitstellt. FOREIGN KEY-Einschränkungen erfordern, dass jeder Wert in der Spalte in den entsprechenden Spalten, auf die verwiesen wird, in der Tabelle, auf die verwiesen wird, vorhanden ist. FOREIGN KEY-Einschränkungen können nur auf Spalten verweisen, die PRIMARY KEY- oder UNIQUE-Einschränkungen in der Tabelle sind, auf die verwiesen wird; oder auf Spalten, auf die in einer UNIQUE INDEX-Einschränkung in der Tabelle, auf die verwiesen wird, verwiesen wird. Fremdschlüssel für berechnete Spalten müssen auch als PERSISTED markiert werden.

[ _schema\_name_ **.** ] *referenced_table_name*] Der Name der Tabelle, auf die in der FOREIGN KEY-Einschränkung verwiesen wird, sowie das Schema, zu dem sie gehört.

**(** *ref_column* [ **,** ... *n* ] **)** Eine Spalte oder Liste von Spalten aus der Tabelle, auf die die FOREIGN KEY-Einschränkung verweist

ON DELETE { **NO ACTION** \| CASCADE \| SET NULL \| SET DEFAULT } Gibt an, welche Aktion für Zeilen in der erstellten Tabelle ausgeführt werden soll, wenn diese Zeilen eine referenzielle Beziehung aufweisen und die Zeile, auf die verwiesen wird, aus der übergeordneten Tabelle gelöscht wird. Der Standardwert ist NO ACTION.

NO ACTION Die [!INCLUDE[ssDE](../../includes/ssde-md.md)] löst einen Fehler aus, und für die Aktion zum Löschen der Zeile in der übergeordneten Tabelle wird ein Rollback ausgeführt.

CASCADE Wenn diese Zeile aus der übergeordneten Tabelle gelöscht wird, werden die entsprechenden Zeilen aus der verweisenden Tabelle gelöscht.

SET NULL Alle Werte, aus denen sich der Fremdschlüssel zusammensetzt, werden auf NULL festgelegt, wenn die entsprechende Zeile in der übergeordneten Tabelle gelöscht wird. Die Fremdschlüsselspalten müssen NULL-Werte zulassen, um diese Einschränkung auszuführen.

SET DEFAULT Alle Werte, aus denen sich der Fremdschlüssel zusammensetzt, werden auf ihre Standardwerte festgelegt, wenn die entsprechende Zeile in der übergeordneten Tabelle gelöscht wird. Alle Fremdschlüsselspalten müssen Standarddefinitionen aufweisen, damit diese Einschränkung ausgeführt wird. Wenn eine Spalte NULL-Werte zulässt, und es ist kein expliziter Standardwert festgelegt, wird NULL als der implizite Standardwert für die Spalte verwendet.

Geben Sie CASCADE nicht an, wenn die Tabelle in eine Mergeveröffentlichung einbezogen werden soll, bei der logische Datensätze verwendet werden. Weitere Informationen zu logischen Datensätzen finden Sie unter [Gruppieren von Änderungen an verknüpften Zeilen mithilfe von logischen Datensätzen](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).

`ON DELETE CASCADE` kann nicht definiert werden, wenn bereits ein `INSTEAD OF`-Trigger für `ON DELETE` für die Tabelle vorhanden ist.

In der Datenbank [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] verfügt die Tabelle **ProductVendor** beispielsweise über eine referenzielle Beziehung zu der Tabelle **Vendor**. Der Fremdschlüssel **ProductVendor.BusinessEntityID** verweist auf den Primärschlüssel **Vendor.BusinessEntityID**.

Wenn eine `DELETE`-Anweisung für eine Zeile in der **Vendor**-Tabelle ausgeführt wird und eine `ON DELETE CASCADE`-Aktion für **ProductVendor.BusinessEntityID** festgelegt ist, sucht [!INCLUDE[ssDE](../../includes/ssde-md.md)] nach mindestens einer abhängigen Zeile in der **ProductVendor**-Tabelle. Falls eine solche Zeile vorhanden ist, werden die abhängigen Zeilen in der Tabelle **ProductVendor** sowie die Zeile, auf die in der Tabelle **Vendor** verwiesen wird, gelöscht.

Ist hingegen `NO ACTION` angegeben, löst [!INCLUDE[ssDE](../../includes/ssde-md.md)] einen Fehler aus und führt ein Rollback für die Löschaktion der **Vendor**-Zeile aus, wenn in der **ProductVendor**-Tabelle mindestens eine Zeile vorhanden ist, die auf diese Zeile verweist.

ON UPDATE { **NO ACTION** \| CASCADE \ SET NULL \| SET DEFAULT } Gibt an, welche Aktion für eine Zeile der geänderten Tabelle ausgeführt werden soll, wenn diese Zeile eine referenzielle Beziehung hat und die Zeile, auf die verwiesen wird, in der übergeordneten Tabelle aktualisiert wird. Der Standardwert ist NO ACTION.

NO ACTION Die [!INCLUDE[ssDE](../../includes/ssde-md.md)] löst einen Fehler aus, und für die Updateaktion der Zeile in der übergeordneten Tabelle wird ein Rollback ausgeführt.

CASCADE Wenn diese Zeile in der übergeordneten Tabelle aktualisiert wird, werden die entsprechenden Zeilen in der verweisenden Tabelle aktualisiert.

SET NULL Alle Werte, aus denen sich der Fremdschlüssel zusammensetzt, werden auf NULL festgelegt, wenn die entsprechende Zeile in der übergeordneten Tabelle aktualisiert wird. Die Fremdschlüsselspalten müssen NULL-Werte zulassen, um diese Einschränkung auszuführen.

SET DEFAULT Alle Werte, aus denen sich der Fremdschlüssel zusammensetzt, werden auf die Standardwerte festgelegt, wenn die entsprechende Zeile in der übergeordneten Tabelle aktualisiert wird. Alle Fremdschlüsselspalten müssen Standarddefinitionen aufweisen, damit diese Einschränkung ausgeführt wird. Wenn eine Spalte NULL-Werte zulässt, und es ist kein expliziter Standardwert festgelegt, wird NULL als der implizite Standardwert für die Spalte verwendet.

Geben Sie `CASCADE` nicht an, wenn die Tabelle in eine Mergeveröffentlichung einbezogen werden soll, bei der logische Datensätze verwendet werden. Weitere Informationen zu logischen Datensätzen finden Sie unter [Gruppieren von Änderungen an verknüpften Zeilen mithilfe von logischen Datensätzen](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).

`ON UPDATE CASCADE`, `SET NULL` und `SET DEFAULT` können nicht definiert werden, wenn für `ON UPDATE` bereits ein `INSTEAD OF`-Trigger für die Tabelle vorhanden ist, die geändert wird.

In der Datenbank [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] verfügt die Tabelle **ProductVendor** beispielsweise über eine referenzielle Beziehung zur Tabelle **Vendor**: Der Fremdschlüssel **ProductVendor.BusinessEntity** verweist auf den Primärschlüssel **Vendor.BusinessEntityID**.

Wenn eine UPDATE-Anweisung für eine Zeile in der Tabelle **Vendor** ausgeführt wird, und eine ON UPDATE CASCADE-Aktion für **ProductVendor.BusinessEntityID** festgelegt ist, sucht [!INCLUDE[ssDE](../../includes/ssde-md.md)] nach mindestens einer abhängigen Zeile in der Tabelle **ProductVendor**. Falls eine solche Zeile vorhanden ist, werden die abhängigen Zeilen in der Tabelle **ProductVendor** sowie die Zeile, auf die in der Tabelle **Vendor** verwiesen wird, aktualisiert.

Ist hingegen NO ACTION angegeben, löst [!INCLUDE[ssDE](../../includes/ssde-md.md)] einen Fehler aus und führt für die Updateaktion der **Vendor**-Zeile ein Rollback aus, wenn in der **ProductVendor**-Tabelle mindestens eine Zeile vorhanden ist, die auf diese Zeile verweist.

CHECK Eine Einschränkung, die Domänenintegrität erzwingt, indem die möglichen Eingabewerte für eine oder mehrere Spalten beschränkt wird. CHECK-Einschränkungen für berechnete Spalten müssen auch als PERSISTED markiert werden.

*logical_expression* Ein logischer Ausdruck, der TRUE oder FALSE zurückgibt. Aliasdatentypen können nicht Teil des Ausdrucks sein.

*column* Eine Spalte oder Liste von Spalten in Klammern, die in Tabelleneinschränkungen verwendet wird, um anzuzeigen, welche Spalten in der Einschränkungsdefinition verwendet werden.

[ **ASC** | DESC ] Gibt die Reihenfolge an, in der die Spalte oder die Spalten, die in der Tabelleneinschränkung enthalten sind, sortiert werden. Die Standardeinstellung ist ASC.

*partition_scheme_name* Der Name des Partitionsschemas, das die Dateigruppen definiert, denen die Partitionen einer partitionierten Tabelle zugeordnet werden. Das Partitionsschema muss in der Datenbank vorhanden sein.

[ _partition\_column\_name_ **.** ] Gibt die Spalte an, auf deren Grundlage eine partitionierte Tabelle partitioniert wird. Die Spalte muss in Bezug auf Datentyp, Länge und Genauigkeit mit der Spalte übereinstimmen, die in *partition_scheme_name* verwendet wird. Berechnete Spalten, die in eine Partitionsfunktion einbezogen werden, müssen explizit als PERSISTED gekennzeichnet sein.

> [!IMPORTANT]
> Es wird empfohlen, NOT NULL für die Partitionierungsspalte von partitionierten Tabellen sowie von nicht partitionierten Tabellen anzugeben, die als Quelle oder Ziel für ALTER TABLE...SWITCH-Vorgänge fungieren. Damit stellen Sie sicher, dass mit CHECK-Einschränkungen für Partitionierungsspalten keine Überprüfung auf NULL-Werte ausgeführt werden muss.

WITH FILLFACTOR **=** _fillfactor_ Gibt an, wie weit die [!INCLUDE[ssDE](../../includes/ssde-md.md)] die einzelnen Indexseiten füllen soll, die zum Speichern der Indexdaten verwendet werden. Vom Benutzer angegebene *fillfactor*-Werte können Zahlen von 1 bis 100 sein. Wenn kein Wert angegeben ist, lautet der Standardwert 0. Die Füllfaktorwerte 0 und 100 sind in jeder Hinsicht identisch.

> [!IMPORTANT]
> Das Verwenden von WITH FILLFACTOR = *fillfactor* als einzige Indexoption, die für die PRIMARY KEY- oder UNIQUE-Einschränkungen gilt, wird hier aus Gründen der Abwärtskompatibilität weiterhin dokumentiert. In zukünftigen Releases wird dies jedoch nicht mehr der Fall sein.

*column_set_name* XML COLUMN_SET FOR ALL_SPARSE_COLUMNS Der Name des Spaltensatzes. Bei einem Spaltensatz handelt es sich um eine nicht typisierte XML-Darstellung, die alle Sparsespalten einer Tabelle in einer strukturierten Ausgabe kombiniert. Weitere Informationen zu Spaltensätzen finden Sie unter [Verwenden von Spaltensätzen](../../relational-databases/tables/use-column-sets.md).

PERIOD FOR SYSTEM_TIME ( *system_start_time_column_name*, *system_end_time_column_name* ) **Gilt für:**  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt die Namen der Spalten an, die das System verwendet, um den Zeitraum aufzuzeichnen, für den ein Datensatz gültig ist. Verwenden Sie dieses Argument zusammen mit den Argumenten GENERATED ALWAYS AS ROW { START | END } und WITH SYSTEM_VERSIONING = ON, um die Versionsverwaltung durch das System für eine Tabelle zu aktivieren. Weitere Informationen finden Sie unter [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

COMPRESSION_DELAY **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Diese Verzögerung gibt bei einer speicheroptimierten Tabelle die minimale Anzahl von Minuten an, die eine Zeile in der Tabelle unverändert bleiben muss, bevor sie für die Komprimierung in den Columnstore-Index geeignet ist. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wählt spezifische Zeilen für die Komprimierung gemäß dem Zeitpunkt des letzten Updates aus. Wenn Zeilen innerhalb von zwei Stunden häufig geändert werden, können Sie beispielsweise `COMPRESSION_DELAY = 120 Minutes` festlegen, um sicherzustellen, dass Zeilen vor der Komprimierung durch SQL Server aktualisiert werden.

DELAY gibt bei einer datenträgerbasierten Tabelle die minimale Anzahl von Minuten an, die eine Deltazeilengruppe im Zustand CLOSED in der Delta-Zeilengruppe verbringen muss, bevor [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sie in die komprimierte Zeilengruppe komprimieren kann. Da Einfügungs- und Aktualisierungszeiten in datenträgerbasierten Tabellen nicht für einzelne Zeilen nachverfolgt werden, wird die Verzögerung in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf Delta-Zeilengruppen im CLOSED-Status angewendet.

Die Standardeinstellung beträgt 0 Minuten.

Empfehlungen zur Verwendung von `COMPRESSION_DELAY` finden Sie unter [Erste Schritte mit Columnstore für operative Echtzeitanalyse](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).

\< table_option> ::= Gibt eine oder mehrere Tabellenoptionen an.

DATA_COMPRESSION Gibt die Datenkomprimierungsoption für die angegebene Tabelle, die Partitionsnummer oder den Bereich von Partitionen an. Die folgenden Optionen sind verfügbar:

NONE Die Tabelle oder die angegebenen Partitionen werden nicht komprimiert.

ROW Die Tabelle oder die angegebenen Partitionen werden mit Zeilenkomprimierung komprimiert.

PAGE Die Tabelle oder die angegebenen Partitionen werden mit Seitenkomprimierung komprimiert.

COLUMNSTORE

**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gilt nur für columnstore-Indizes, einschließlich nicht gruppierter und gruppierter columnstore-Indizes. COLUMNSTORE gibt an, dass die Komprimierung mit der leistungsfähigsten Columnstore-Komprimierung ausgeführt werden soll. Dies ist die gängige Methode.

COLUMNSTORE_ARCHIVE **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gilt nur für columnstore-Indizes, einschließlich nicht gruppierter und gruppierter columnstore-Indizes. Durch COLUMNSTORE_ARCHIVE wird die Tabelle oder Partition weiter in eine geringere Größe komprimiert. Dies empfiehlt sich bei der Archivierung und in Situationen, in denen es auf eine geringere Speichergröße und nicht auf den zusätzlichen Zeitaufwand für das Speichern und Abrufen ankommt.

Weitere Informationen finden Sie unter [Data Compression](../../relational-databases/data-compression/data-compression.md).

ON PARTITIONS **(** { `<partition_number_expression>` | [ **,** ...*n* ] **)** Gibt die Partitionen an, für die die DATA_COMPRESSION-Einstellung gilt. Wenn die Tabelle nicht partitioniert ist, erzeugt das Argument `ON PARTITIONS` einen Fehler. Wenn die `ON PARTITIONS`-Klausel nicht angegeben wird, gilt die `DATA_COMPRESSION`-Option für alle Partitionen einer partitionierten Tabelle.

*partition_number_expression* kann auf folgenden Weisen angegeben werden:

- Geben Sie die Partitionsnummer einer Partition an, z. B. `ON PARTITIONS (2)`
- Geben Sie die Partitionsnummern mehrerer einzelner Partitionen durch Kommas getrennt an, beispielsweise: `ON PARTITIONS (1, 5)`
- Geben Sie sowohl Bereiche als auch einzelne Partitionen an, beispielsweise: `ON PARTITIONS (2, 4, 6 TO 8)`

Für `<range>` können durch das Wort „TO“ getrennte Partitionsnummern angegeben werden, z. B. `ON PARTITIONS (6 TO 8)`.

Wenn Sie für verschiedene Partitionen unterschiedliche Datenkomprimierungstypen festlegen möchten, geben Sie die Option `DATA_COMPRESSION` mehrmals an, beispielsweise:

```sql
WITH
(
    DATA_COMPRESSION = NONE ON PARTITIONS (1),
    DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),
    DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)
)
```

\<index_option> ::= Gibt eine oder mehrere Indexoptionen an. Eine vollständige Beschreibung dieser Optionen finden Sie unter [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md).

PAD_INDEX = { ON | **OFF** } Bei der Einstellung ON wird der durch FILLFACTOR angegebene Prozentsatz des freien Speicherplatzes auf die Zwischenebenenseiten des Indexes angewendet. Wenn die Einstellung OFF verwendet wird oder kein FILLFACTOR-Wert angegeben wurde, werden die Zwischenebenenseiten fast bis zu ihrer Kapazitätsgrenze gefüllt, wobei ausreichend Speicherplatz für mindestens eine Zeile mit der maximal für diesen Index möglichen Größe frei bleibt; diese ergibt sich aus der Schlüsselmenge auf den Zwischenseiten. Der Standardwert ist OFF.

FILLFACTOR **=** _fillfactor_ Gibt einen Prozentsatz an, der anzeigt, wie weit die [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Blattebene jeder Indexseite während der Indexerstellung oder -änderung füllen soll. *fillfactor* muss ein ganzzahliger Wert zwischen 1 und 100 sein. Die Standardeinstellung ist 0. Die Füllfaktorwerte 0 und 100 sind in jeder Hinsicht identisch.

IGNORE_DUP_KEY = { ON | **OFF** } Diese Option gibt die Fehlermeldung an, wenn ein Einfügevorgang versucht, doppelte Schlüsselwerte in einen eindeutigen Index einzufügen. Die IGNORE_DUP_KEY-Option gilt nur für Einfügevorgänge nach dem Erstellen oder Neuerstellen des Index. Beim Ausführen von [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) oder [UPDATE](../../t-sql/queries/update-transact-sql.md) hat die Option keine Auswirkungen. Der Standardwert ist OFF.

ON Eine Warnmeldung wird ausgegeben, wenn doppelte Schlüsselwerte in einen eindeutigen Index eingefügt werden. Es schlagen nur die Zeilen fehl, die gegen die Eindeutigkeitseinschränkung verstoßen.

OFF Eine Fehlermeldung wird ausgegeben, wenn doppelte Schlüsselwerte in einen eindeutigen Index eingefügt werden. Für den gesamten INSERT-Vorgang wird ein Rollback ausgeführt.

`IGNORE_DUP_KEY` kann für Indizes, die für eine Sicht erstellt werden, nicht eindeutige Indizes, XML-Indizes, räumliche und gefilterte Indizes nicht auf ON festgelegt werden.

Verwenden Sie [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md), um `IGNORE_DUP_KEY` anzuzeigen.

In abwärtskompatibler Syntax entspricht `WITH IGNORE_DUP_KEY``WITH IGNORE_DUP_KEY = ON`.

STATISTICS_NORECOMPUTE **=** { ON | **OFF** } Bei der Einstellung ON werden veraltete Indexstatistiken nicht automatisch neu berechnet. Bei der Einstellung OFF sind automatische Statistikupdates aktiviert. Der Standardwert ist OFF.

ALLOW_ROW_LOCKS **=** { **ON** | OFF } Bei der Einstellung ON sind Zeilensperren beim Zugriff auf den Index zulässig. Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] bestimmt, wann Zeilensperren verwendet werden. Bei OFF werden Zeilensperren nicht verwendet. Der Standardwert ist ON.

ALLOW_PAGE_LOCKS **=** { **ON** | OFF } Bei der Einstellung ON sind Seitensperren beim Zugriff auf den Index zulässig. Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] bestimmt, wann Seitensperren verwendet werden. Bei der Einstellung OFF werden Seitensperren nicht verwendet. Der Standardwert ist ON.

OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | **OFF** } **Gilt für** : [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] und höher. <BR>
Gibt an, ob der Konflikt beim Einfügen der letzten Seite optimiert werden soll. Der Standardwert ist OFF. Weitere Informationen finden Sie im Abschnitt [Sequenzielle Schlüssel](./create-index-transact-sql.md#sequential-keys) auf der CREATE INDEX-Seite.

FILETABLE_DIRECTORY = *directory_name*

**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher).

Gibt den Windows-kompatiblen FileTable-Verzeichnisnamen an. Dieser Name sollte für alle FileTable-Verzeichnisnamen in der Datenbank eindeutig sein. Bei Eindeutigkeitsvergleichen wird unabhängig von den Sortiereinstellungen die Groß-/Kleinschreibung nicht beachtet. Wenn dieser Wert nicht angegeben ist, wird Name der Dateitabelle verwendet.

FILETABLE_COLLATE_FILENAME = { *collation_name* | database_default }

**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher). [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] unterstützt `FILETABLE` nicht.

Gibt den Namen der Sortierung an, die auf die **Name** -Spalte in der FileTable angewendet werden soll. Zur Einhaltung der Windows-Dateinamensemantik darf bei der Sortierung die Groß-/Kleinschreibung nicht beachtet werden. Wenn dieser Wert nicht angegeben ist, wird die Standardsortierung für die Datenbank verwendet. Wenn bei der Datenbank-Standardsortierung die Groß-/Kleinschreibung beachtet wird, wird ein Fehler ausgelöst, und der CREATE TABLE-Vorgang kann nicht durchgeführt werden.

*collation_name* Der Name einer Sortierung, bei der die Groß-/Kleinschreibung nicht beachtet wird.

database_default Gibt an, dass die Standardsortierung für die Datenbank verwendet werden soll. Bei dieser Sortierung darf die Groß-/Kleinschreibung nicht beachtet werden.

FILETABLE_PRIMARY_KEY_CONSTRAINT_NAME = *constraint_name*
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher)

Gibt den Namen an, der für die Primärschlüsseleinschränkung verwendet werden soll, die automatisch für die FileTable erstellt wird. Wenn dieser Wert nicht angegeben ist, wird ein Name für die Einschränkung generiert.

FILETABLE_STREAMID_UNIQUE_CONSTRAINT_NAME = *constraint_name*
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher)

Gibt den Namen an, der für die eindeutige Einschränkung verwendet werden soll, die automatisch für die Spalte **stream_id** der FileTable erstellt wird. Wenn dieser Wert nicht angegeben ist, wird ein Name für die Einschränkung generiert.

FILETABLE_FULLPATH_UNIQUE_CONSTRAINT_NAME = *constraint_name*
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher)

Gibt den Namen an, der für die eindeutige Einschränkung verwendet werden soll, die automatisch für die Spalten **parent_path_locator** und **name** in der FileTable erstellt wird. Wenn dieser Wert nicht angegeben ist, wird ein Name für die Einschränkung generiert.

SYSTEM_VERSIONING **=** ON [ ( HISTORY_TABLE **=** *schema_name* .*history_table_name* [, DATA_CONSISTENCY_CHECK **=** { **ON** | OFF } ] ) ]  **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)])

Ermöglicht die Systemversionsverwaltung der Tabelle, wenn die Einschränkungsanforderungen für den Datentyp, die NULL-Zulässigkeit und den Primärschlüssel erfüllt werden. Wenn das Argument `HISTORY_TABLE` nicht verwendet wird, generiert das System eine neue Verlaufstabelle, die dem Schema der aktuellen Tabelle in der gleichen Dateigruppe der aktuellen Tabelle entspricht, erstellt eine Verbindung zwischen den beiden Tabellen und ermöglicht dem System, den Verlauf von jedem Datensatz der aktuellen Tabelle in der Verlaufstabelle aufzuzeichnen. Der Name dieser Verlaufstabelle ist dann `MSSQL_TemporalHistoryFor<primary_table_object_id>`. Standardmäßig ist die Verlaufstabelle **PAGE** -komprimiert. Wenn das Argument `HISTORY_TABLE` verwendet wird, um eine Verbindung zu einer vorhandenen Verlaufstabelle zu erstellen und diese zu verwenden, wird die Verbindung zwischen der aktuellen Tabelle und der angegebenen Tabelle hergestellt. Falls die aktuelle Tabelle partitioniert wurde, kann die Verlaufstabelle auf der Standarddateigruppe erstellt werden, da die Partitionierungskonfiguration nicht automatisch von der aktuellen auf die Verlaufstabelle repliziert wird. Falls der Name einer Verlaufstabelle während der Erstellung der Verlaufstabelle angegeben wird, müssen Sie das Schema und den Tabellennamen angeben. Wenn Sie einen Link zu einer vorhandenen Verlaufstabelle erstellen, können Sie eine Datenkonsistenzprüfung durchführen. Diese Datenkonsistenzprüfung stellt sicher, dass vorhandene Datensätze nicht überlappen. Die Datenkonsistenzprüfung ist standardmäßig aktiviert. Verwenden Sie dieses Argument mit den Argumenten `PERIOD FOR SYSTEM_TIME` und `GENERATED ALWAYS AS ROW { START | END }`, um die Systemversionsverwaltung für eine Tabelle zu aktivieren. Weitere Informationen finden Sie unter [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

REMOTE_DATA_ARCHIVE = { ON [ ( *table_stretch_options* [,...n] ) ] | OFF ( MIGRATION_STATE = PAUSED ) }   
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher).

Erstellt eine neue Tabelle, für die Stretch Database aktiviert oder deaktiviert ist. Weitere Informationen finden Sie unter [Stretch Database](../../sql-server/stretch-database/stretch-database.md).

**Aktivieren von Stretch Database für eine Tabelle**

Wenn Sie Stretch durch Angeben von `ON` für eine Tabelle aktivieren, können Sie optional `MIGRATION_STATE = OUTBOUND` festlegen, um sofort mit dem Migrieren von Daten zu beginnen, oder Sie legen `MIGRATION_STATE = PAUSED` fest, um die Datenmigration zu verzögern. Standardwert: `MIGRATION_STATE = OUTBOUND`. Weitere Informationen über das Aktivieren von Stretch für eine Tabelle finden Sie unter [Aktivieren von Stretch Database für eine Tabelle](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).

**Voraussetzungen** Sie müssen Stretch auf dem Server und auf der Datenbank aktivieren, bevor Sie Stretch für eine Tabelle aktivieren können. Weitere Informationen finden Sie unter [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).

**Berechtigungen** Zum Aktivieren von Stretch für eine Datenbank oder eine Tabelle benötigen Sie die „db_owner“-Berechtigungen. Zum Aktivieren von Stretch für eine Tabelle benötigen Sie auch die ALTER-Berechtigungen für die Tabelle.

[ FILTER_PREDICATE = { null | *predicate* } ] **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher)

Gibt optional ein Filterprädikat zum Auswählen der Zeilen an, die aus einer Tabelle migriert werden sollen, die sowohl Verlaufsdaten als auch aktuelle Daten enthält. Das Prädikat muss eine deterministische Inline-Tabellenwertfunktion aufrufen. Weitere Informationen finden Sie unter [Aktivieren von Stretch Database für eine Tabelle](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) und [Auswählen zu migrierender Zeilen mithilfe einer Filterfunktion](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).

> [!IMPORTANT]
> Wenn Sie ein schwaches Filterprädikat angeben, wird die Datenmigration ebenfalls unzureichend ausgeführt. Stretch-Datenbank wendet das Filterprädikat über den CROSS APPLY-Operator auf die Tabelle an.

Wenn Sie kein Filterprädikat angeben, wird die gesamte Tabelle migriert.

Wenn Sie ein Filterprädikat angeben, müssen Sie auch *MIGRATION_STATE* angeben.

MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED } **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

- Geben Sie `OUTBOUND` an, um Daten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] zu migrieren.
- Geben Sie `INBOUND` an, um Remotedaten für die Tabelle aus [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] zurück zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu migrieren und Stretch für die Tabelle zu deaktivieren. Weitere Informationen finden Sie unter [Deaktivieren von Stretch Database und Zurückholen von Remotedaten](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

   Dieser Vorgang verursacht Datenübertragungskosten und kann nicht abgebrochen werden.

- Geben Sie `PAUSED` an, um die Datenmigration zu pausieren oder nach hinten zu verschieben. Weitere Informationen finden Sie unter [Anhalten und Fortsetzen der Datenmigration – Stretch Database](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).

[DATA_DELETION = ON  
    {( FILTER_COLUMN = Spaltenname,   
           RETENTION_PERIOD = { INFINITE | Anzahl {DAY | DAYS | WEEK | WEEKS | MONTH | MONTHS | YEAR | YEARS } )}] **Gilt für:** *nur* Azure SQL Edge

Aktiviert die auf Aufbewahrungsrichtlinien basierende Bereinigung von alten oder veralteten Daten aus Tabellen innerhalb einer Datenbank. Weitere Informationen finden Sie unter [Aktivieren und Deaktivieren von Datenaufbewahrung](/azure/azure-sql-edge/data-retention-enable-disable). Die folgenden Parameter müssen angegeben werden, damit Datenaufbewahrung aktiviert wird. 

- FILTER_COLUMN = { spaltenname }  
Gibt die Spalte an, die verwendet werden soll, um zu bestimmen, ob die Zeilen in der Tabelle veraltet sind. Die folgenden Datentypen sind für die Filterspalte zulässig.
  - Date
  - Datetime
  - DateTime2
  - SmallDateTime
  - DateTimeOffset

- RETENTION_PERIOD = { INFINITE \| Anzahl {DAY \| DAYS \| WEEK \| WEEKS \| MONTH \| MONTHS \| YEAR \| YEARS }}       
  Gibt die Richtlinie für den Aufbewahrungszeitraum an. Der Aufbewahrungszeitraum wird als eine Kombination aus einem positiven Integerwert und der Datumsteileinheit angegeben.   

MEMORY_OPTIMIZED **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]) Azure SQL Managed Instance unterstützt speicheroptimierte Tabellen nicht.

Der Wert ON gibt an, dass die Tabelle arbeitsspeicheroptimiert ist. Speicheroptimierte Tabellen sind Teil des Features „In-Memory OLTP“, das verwendet wird, um die Leistung der Transaktionsverarbeitung zu optimieren. Informationen zu den ersten Schritten mit In-Memory-OLTP finden Sie unter [Schnellstart 1: In-Memory-OLTP-Technologien für höhere Transact-SQL-Leistung](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md). Ausführliche Informationen zu speicheroptimierten Tabellen finden Sie unter [Speicheroptimierte Tabellen](../../relational-databases/in-memory-oltp/sample-database-for-in-memory-oltp.md).

Der Standardwert OFF gibt an, dass die Tabelle auf einem Datenträger basiert.

DURABILITY **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Der Wert von `SCHEMA_AND_DATA` gibt an, dass die Tabelle dauerhaft ist, d. h. Änderungen werden auf dem Datenträger beibehalten und überstehen Neustarts und Failover. SCHEMA_AND_DATA ist der Standardwert.

Der Wert von `SCHEMA_ONLY` gibt an, dass die Tabelle nicht dauerhaft ist. Das Tabellenschema wird beibehalten, aber Datenupdates werden nach einem Neustart oder Failover der Datenbank nicht beibehalten. `DURABILITY = SCHEMA_ONLY` ist nur mit `MEMORY_OPTIMIZED = ON` zulässig.

> [!WARNING]
> Wenn eine Tabelle mit **DURABILITY = SCHEMA_ONLY** erstellt wird und **READ_COMMITTED_SNAPSHOT** anschließend mithilfe von **ALTER DATABASE** geändert wird, gehen Daten in der Tabelle verloren.

BUCKET_COUNT **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt die Anzahl der Buckets an, die im Hashindex erstellt werden sollen. Der maximale Wert für BUCKET_COUNT in Hashindizes beträgt 1.073.741.824. Weitere Informationen zu Indizes für speicheroptimierte Tabellen finden Sie unter [Indexes for Memory-Optimized Tables (Indizes für speicheroptimierte Tabellen)](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md).

Bucket_count ist ein erforderliches Argument.

INDEX **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]).

Spalten- und Tabellenindizes können als Teil der Anweisung CREATE TABLE angeben werden. Ausführliche Informationen zum Hinzufügen und Entfernen von Indizes bei speicheroptimierten Tabellen finden Sie unter [Ändern von speicheroptimierten Tabellen](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md)

HASH **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt an, dass ein HASH-Index erstellt wird.

Hashindizes werden nur für speicheroptimierte Tabellen unterstützt.

## <a name="remarks"></a>Bemerkungen

Informationen zur Anzahl zulässiger Tabellen, Spalten, Einschränkungen und Indizes finden Sie unter [Spezifikationen der maximalen Kapazität für SQL Server](../../sql-server/maximum-capacity-specifications-for-sql-server.md).

Der Speicherplatz für Tabellen und Indizes wird i. A. jeweils blockweise zugeordnet. Wenn die Option `SET MIXED_PAGE_ALLOCATION` von `ALTER DATABASE` beim Erstellen einer Tabelle oder eines Index auf TRUE festgelegt ist (bzw. „always“ bei [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]), werden so lange Seiten aus gemischten Erweiterungen zugewiesen, bis ausreichend Seiten zum Füllen einer einheitlichen Erweiterung vorhanden sind. Wenn genügend Seiten für einen einheitlichen Block vorhanden sind, wird jedes Mal dann ein weiterer Block zugeordnet, wenn die bereits zugeordneten Blöcke voll sind. Führen Sie `sp_spaceused` aus, um einen Bericht darüber zu erhalten, wie viel Speicherplatz für eine Tabelle zugeordnet ist und von dieser verwendet wird.

Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] erzwingt keine Reihenfolge für die Angabe von DEFAULT, IDENTITY, ROWGUIDCOL oder Spalteneinschränkungen in einer Spaltendefinition.

Beim Erstellen einer Tabelle wird die Option QUOTED IDENTIFIER immer mit der Einstellung ON in den Metadaten der Tabelle gespeichert, und zwar auch dann, wenn die Option beim Erstellen der Tabelle auf OFF festgelegt ist.

## <a name="temporary-tables"></a>Temporäre Tabellen

Sie können sowohl lokale als auch globale temporäre Tabellen erstellen. Lokale temporäre Tabellen sind nur während der aktuellen Sitzung sichtbar; globale temporäre Tabellen sind von allen Sitzungen aus sichtbar. Temporäre Tabellen können nicht partitioniert werden.

Stellen Sie lokalen temporären Tabellennamen ein einzelnes Nummernzeichen (#*table_name*) und globalen temporären Tabellennamen ein doppeltes Nummernzeichen voran (##*table_name*).

[!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen verweisen auf die temporäre Tabelle, indem sie den in der `CREATE TABLE`-Anweisung angegebenen Wert für *table_name* verwenden. Zum Beispiel:

```sql
CREATE TABLE #MyTempTable (
    col1 INT PRIMARY KEY
);

INSERT INTO #MyTempTable
VALUES (1);
```

Wenn mehr als eine temporäre Tabelle innerhalb einer einzigen gespeicherten Prozedur oder innerhalb eines Batches erstellt wird, müssen verschiedene Namen für die temporären Tabellen verwendet werden.

Wenn Sie eine temporäre Tabelle erstellen oder auf eine temporäre Tabelle zugreifen und dabei einen Schemanamen (*schema_name*) angeben, wird dieser ignoriert. Alle temporären Tabellen werden im dbo-Schema erstellt.

Wenn eine lokale temporäre Tabelle in einer gespeicherten Prozedur oder einer Anwendung erstellt wird, die von mehreren Benutzern gleichzeitig ausgeführt werden kann, muss es dem [!INCLUDE[ssDE](../../includes/ssde-md.md)] möglich sein, die von den verschiedenen Benutzern erstellten Tabellen zu unterscheiden. Zu diesem Zweck fügt das [!INCLUDE[ssDE](../../includes/ssde-md.md)] intern ein numerisches Suffix an alle Namen lokaler temporärer Tabellen an. Der vollständige, in der Tabelle **sysobjects** von **tempdb** gespeicherte Name einer temporären Tabelle besteht aus dem in der CREATE TABLE-Anweisung angegebenen Tabellennamen und dem vom System generierten numerischen Suffix. Damit das Suffix angefügt werden kann, darf *table_name* für eine lokale temporäre Tabelle nicht länger als 116 Zeichen sein.

Temporäre Tabellen werden automatisch gelöscht, wenn sie nicht mehr gültig sind, es sei denn, sie wurden bereits explizit mithilfe von DROP TABLE gelöscht:

- Eine lokale temporäre Tabelle, die in einer gespeicherten Prozedur erstellt wurde, wird bei Beendigung der gespeicherten Prozedur automatisch gelöscht. Auf die Tabelle kann durch geschachtelte gespeicherte Prozeduren verwiesen werden, die von der gespeicherten Prozedur ausgeführt werden, die die Tabelle erstellt hat. Auf die Tabelle kann nicht durch den Vorgang verwiesen werden, der die gespeicherte Prozedur, die die Tabelle erstellt hat, aufgerufen hat.
- Alle anderen lokalen temporären Tabellen werden am Ende der aktuellen Sitzung automatisch gelöscht.
- Eine globale temporäre Tabelle wird automatisch gelöscht, wenn die Sitzung, die die betreffende Tabelle erstellt hat, beendet wird und kein Task mehr auf die Tabelle verweist. Die Zuordnung zwischen einem Task und einer Tabelle wird nur für die Dauer einer einzelnen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung aufrechterhalten. Das bedeutet, dass eine globale temporäre Tabelle bei Beendigung der letzten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung gelöscht wird, die aktiv auf die Tabelle verwiesen hat, als die Sitzung, die die Tabelle erstellt hat, beendet wurde.

Eine lokale temporäre Tabelle, die in einer gespeicherten Prozedur oder einem Trigger erstellt wurde, kann den gleichen Namen wie eine temporäre Tabelle haben, die vor dem Aufruf der gespeicherten Prozedur oder des Triggers erstellt wurde. Wenn jedoch eine Abfrage auf eine temporäre Tabelle verweist und zu diesem Zeitpunkt zwei temporäre Tabellen mit demselben Namen vorhanden sind, ist nicht definiert, anhand welcher Tabelle die Abfrage aufgelöst werden soll. Eine geschachtelte gespeicherte Prozedur kann ebenfalls eine temporäre Tabelle mit demselben Namen wie eine temporäre Tabelle erstellen, die von der gespeicherten Prozedur erstellt wurde, die die geschachtelte gespeicherte Prozedur aufgerufen hat. Damit jedoch Änderungsanweisungen anhand der Tabelle aufgelöst werden können, die in der geschachtelten Prozedur erstellt wurde, muss die Tabelle die gleiche Struktur mit den gleichen Spaltennamen wie die Tabelle aufweisen, die in der aufrufenden Prozedur erstellt wurde. Dies wird im folgenden Beispiel gezeigt.

```sql
CREATE PROCEDURE dbo.Test2
AS
    CREATE TABLE #t (x INT PRIMARY KEY);
    INSERT INTO #t VALUES (2);
    SELECT Test2Col = x FROM #t;
GO

CREATE PROCEDURE dbo.Test1
AS
    CREATE TABLE #t (x INT PRIMARY KEY);
    INSERT INTO #t VALUES (1);
    SELECT Test1Col = x FROM #t;
    EXEC Test2;
GO

CREATE TABLE #t(x INT PRIMARY KEY);
INSERT INTO #t VALUES (99);
GO

EXEC Test1;
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
(1 row(s) affected)
Test1Col
-----------
1

(1 row(s) affected)
 Test2Col
 -----------
 2
 ```

Beim Erstellen lokaler oder globaler temporärer Tabellen unterstützt die Syntax der `CREATE TABLE`-Anweisung Einschränkungsdefinitionen mit Ausnahme von FOREIGN KEY-Einschränkungen. Wenn eine FOREIGN KEY-Einschränkung in einer temporären Tabelle angegeben wird, gibt die Anweisung ein Warnmeldung zurück, die besagt, dass die Einschränkung nicht berücksichtigt wurde. Die Tabelle wird dennoch, jedoch ohne die FOREIGN KEY-Einschränkungen erstellt. In FOREIGN KEY-Einschränkungen kann nicht auf temporäre Tabellen verwiesen werden.

Wenn eine temporäre Tabelle mit einer benannten Einschränkung und innerhalb des Bereichs einer benutzerdefinierten Transaktion erstellt wird, kann jeweils nur ein Benutzer die Anweisung ausführen, mit der die temporäre Tabelle erstellt wird. Wenn zum Beispiel eine gespeicherte Prozedur eine temporäre Tabelle mit einer benannten Primärschlüsseleinschränkung erstellt, kann die gespeicherte Prozedur nicht von mehreren Benutzern gleichzeitig ausgeführt werden.

## <a name="database-scoped-global-temporary-tables-azure-sql-database"></a>Datenbankweit gültige globale temporäre Tabellen (Azure SQL-Datenbank)

Globale temporäre Tabellen für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (mit „## table_name“ initiiert) werden in „tempdb“ gespeichert und für alle Benutzersitzungen in der gesamten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz freigegeben. Informationen zu SQL-Tabellentypen finden Sie im obigen Abschnitt zum Erstellen von Tabellen.

In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] werden globale temporäre Tabellen unterstützt, die in „tempdb“ gespeichert werden und auf Datenbankebene gelten. Das heißt, dass globale temporäre Tabellen für alle Benutzersitzungen innerhalb derselben [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] freigegeben werden. Benutzersitzungen von anderen Datenbanken können nicht auf globale temporäre Tabellen zugreifen.

Globale temporäre Tabellen für [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] befolgen die gleiche Syntax und Semantik, die bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für temporäre Tabellen verwendet werden. Auf ähnliche Weise gelten globale temporäre gespeicherte Prozeduren in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] auf Datenbankebene. Lokale temporäre Tabellen (mit „# table_name“ initiiert) werden auch für [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] unterstützt und befolgen die gleiche Syntax und Semantik, die bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden. Informationen dazu finden Sie im obigen Abschnitt [Temporäre Tabellen](#temporary-tables).  

> [!IMPORTANT]
> Diese Funktion ist verfügbar für [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

### <a name="troubleshooting-global-temporary-tables-for-azure-sql-database"></a>Problembehandlung bei globalen temporären Tabellen für Azure SQL-Datenbank

Informationen zur Problembehandlung für tempdb finden Sie unter [Überwachen der Speicherplatzverwendung in tempdb](../../relational-databases/databases/tempdb-database.md#monitoring-tempdb-use).

> [!NOTE]
> Nur ein Serveradministrator kann auf die Problembehandlungs-DMVs in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] zugreifen.

### <a name="permissions"></a>Berechtigungen

Jeder Benutzer ist berechtigt, globale temporäre Objekte zu erstellen. Benutzer haben nur Zugriff auf ihre eigenen Objekte, es sei denn, ihnen wurden zusätzliche Berechtigungen zugewiesen.

## <a name="partitioned-tables"></a>Partitionierte Tabellen

Bevor eine partitionierte Tabelle mithilfe von CREATE TABLE erstellt wird, müssen Sie zuerst eine Partitionsfunktion erstellen, um anzugeben, wie die Tabelle partitioniert werden soll. Eine Partitionsfunktion wird mit [CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md) erstellt. Darüber hinaus müssen Sie ein Partitionsschema erstellen, um die Dateigruppen anzugeben, die die von der Partitionsfunktion angegebenen Partitionen aufnehmen. Ein Partitionsschema wird mit [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) erstellt. Das Platzieren von PRIMARY KEY- oder UNIQUE-Einschränkungen in verschiedenen Dateigruppen ist bei partitionierten Tabellen nicht möglich. Weitere Informationen finden Sie unter [partitionierte Tabellen und Indizes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

## <a name="primary-key-constraints"></a>PRIMARY KEY-Einschränkungen

- Eine Tabelle kann nur eine PRIMARY KEY-Einschränkung enthalten.
- Der durch eine PRIMARY KEY-Einschränkung generierte Index kann nicht dazu führen, dass die Anzahl der Indizes der Tabelle 999 nicht gruppierte Indizes und 1 gruppierten Index übersteigt.
- Wenn CLUSTERED oder NONCLUSTERED für eine PRIMARY KEY-Einschränkung nicht angegeben ist, wird CLUSTERED verwendet, sofern keine gruppierten Indizes für UNIQUE-Einschränkungen angegeben sind.
- Alle Spalten, für die eine PRIMARY KEY-Einschränkung definiert wurde, müssen als NOT NULL definiert sein. Falls weder NULL noch NOT NULL angegeben ist, wird für alle Spalten, auf die eine PRIMARY KEY-Einschränkung angewendet wird, die NULL-Zulässigkeit auf NOT NULL festgelegt.

    > [!NOTE]
    > In speicheroptimierten Tabellen sind Schlüsselspalten mit NULL-Zulässigkeit zulässig.

- Wenn ein Primärschlüssel für eine Spalte eines CLR-benutzerdefinierten Typs definiert wird, muss die Implementierung des Typs eine binäre Sortierreihenfolge unterstützen. Weitere Informationen finden Sie unter [Benutzerdefinierte CLR-Typen](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).

## <a name="unique-constraints"></a>UNIQUE-Einschränkungen

- Wenn CLUSTERED oder NONCLUSTERED für eine UNIQUE-Einschränkung nicht angegeben ist, wird standardmäßig NONCLUSTERED verwendet.
- Jede UNIQUE-Einschränkung erzeugt einen Index. Die Anzahl der UNIQUE-Einschränkungen kann nicht dazu führen, dass die Anzahl der Indizes der Tabelle 999 nicht gruppierte Indizes und 1 gruppierten Index übersteigt.
- Wenn eine UNIQUE-Einschränkung für eine Spalte eines CLR-benutzerdefinierten Typs definiert wird, muss die Implementierung des Typs eine binäre oder operatorbasierte Sortierreihenfolge unterstützen. Weitere Informationen finden Sie unter [Benutzerdefinierte CLR-Typen](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).

## <a name="foreign-key-constraints"></a>Fremdschlüsseleinschränkungen

- Wenn ein anderer Wert als NULL in die Spalte einer FOREIGN KEY-Einschränkung eingegeben wird, muss der Wert in der Spalte vorhanden sein, auf die verwiesen wird; andernfalls wird eine Fremdschlüsselverletzungs-Fehlermeldung zurückgegeben.
- FOREIGN KEY-Einschränkungen werden auf die vorangegangene Spalte angewendet, es sei denn, es werden Quellspalten angegeben.
- FOREIGN KEY-Einschränkungen können nur auf Tabellen verweisen, die sich innerhalb derselben Datenbank auf demselben Server befinden. Datenbankübergreifende referenzielle Integrität muss durch Trigger implementiert werden. Weitere Informationen finden Sie unter [CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md).
- FOREIGN KEY-Einschränkungen können auf eine andere Spalte in derselben Tabelle verweisen. Ein solcher Verweis wird als Eigenverweis bezeichnet.
- Die REFERENCES-Klausel einer FOREIGN KEY-Einschränkung auf Spaltenebene kann nur eine Verweisspalte auflisten. Diese Spalte muss denselben Datentyp aufweisen wie die Spalte, für die die Einschränkung definiert wurde.
- Die REFERENCES-Klausel einer FOREIGN KEY-Einschränkung auf Tabellenebene muss ebenso viele Verweisspalten haben, wie sich Spalten in der Einschränkungsspaltenliste befinden. Der Datentyp jeder Verweisspalte muss ebenfalls mit dem der entsprechenden Spalte in der Spaltenliste übereinstimmen. Die Verweisspalten müssen in derselben Reihenfolge angegeben werden, die verwendet wurde, als die Spalten des Primärschlüssels oder spezielle Einschränkungen für die Verweistabelle angegeben wurden.
- CASCADE, SET NULL oder SET DEFAULT können nicht angegeben werden, wenn eine Spalte vom Typ **timestamp** Teil des Fremdschlüssels oder des Schlüssels ist, auf den verwiesen wird.
- CASCADE, SET NULL, SET DEFAULT und NO ACTION können für Tabellen kombiniert werden, die referenzielle Beziehungen untereinander aufweisen. Wenn das [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Einstellung NO ACTION feststellt, wird die Verarbeitung beendet und ein Rollback für verbundene CASCADE-, SET NULL- und SET DEFAULT-Aktionen ausgeführt. Wenn eine DELETE-Anweisung eine Kombination aus CASCADE-, SET NULL-, SET DEFAULT- und NO ACTION-Aktionen bewirkt, werden alle CASCADE-, SET NULL- und SET DEFAULT-Aktionen angewendet, bevor das [!INCLUDE[ssDE](../../includes/ssde-md.md)] nach der möglichen Angabe von NO ACTION sucht.
- Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] verfügt über keine vordefinierte Grenze hinsichtlich der Anzahl von FOREIGN KEY-Einschränkungen, die eine Tabelle, die auf andere Tabellen verweist, enthalten kann, oder hinsichtlich der Anzahl von FOREIGN KEY-Einschränkungen im Besitz anderer Tabellen, die auf eine bestimmte Tabelle verweisen.

  Nichtsdestotrotz ist die tatsächliche Anzahl von FOREIGN KEY-Einschränkungen , die verwendet werden können, durch die Hardwarekonfiguration und den Entwurf der Datenbank und der Anwendung begrenzt. Als Empfehlung gilt, dass eine Tabelle maximal 253 FOREIGN KEY-Einschränkungen enthalten sollte und dass maximal 253 FOREIGN KEY-Einschränkungen auf eine Tabelle verweisen sollten. Die in Ihrem Fall tatsächlich gültige Grenze kann je nach Anwendung und Hardware darüber oder darunter liegen. Beim Entwerfen von Datenbank und Anwendungen sollten Sie die Kosten für das Erzwingen von FOREIGN KEY-Einschränkungen berücksichtigen.

- FOREIGN KEY-Einschränkungen werden nicht auf temporäre Tabellen angewendet.
- FOREIGN KEY-Einschränkungen können nur auf Spalten in PRIMARY KEY- oder UNIQUE-Einschränkungen in der Tabelle, auf die verwiesen wird, oder auf eine UNIQUE INDEX-Einschränkung für die Tabelle, auf die verwiesen wird, verweisen.
- Wenn ein Fremdschlüssel für eine Spalte eines CLR-benutzerdefinierten Typs definiert wird, muss die Implementierung des Typs eine binäre Sortierreihenfolge unterstützen. Weitere Informationen finden Sie unter [Benutzerdefinierte CLR-Typen](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).
- Für Spalten, die an einer Fremdschlüsselbeziehung beteiligt sind, muss die gleiche Länge und Skala definiert sein.

## <a name="default-definitions"></a>DEFAULT-Definitionen

- Eine Spalte kann nur eine DEFAULT-Definition haben.
- Eine DEFAULT-Definition kann konstante Werte, Funktionen, standardmäßige NILADIC-SQL-Funktionen oder NULL enthalten. Die folgende Tabelle zeigt die Funktionen ohne Argumente und die Werte, die sie während einer INSERT-Anweisung für den Standardwert zurückgeben.

    |SQL-92-Funktion ohne Argumente|Zurückgegebener Wert|
    |------------------------------|--------------------|
    |CURRENT_TIMESTAMP|Aktuelles Datum und aktuelle Uhrzeit.|
    |CURRENT_USER|Name des Benutzers, der einen Einfügevorgang ausführt.|
    |SESSION_USER|Name des Benutzers, der einen Einfügevorgang ausführt.|
    |SYSTEM_USER|Name des Benutzers, der einen Einfügevorgang ausführt.|
    |USER|Name des Benutzers, der einen Einfügevorgang ausführt.|
- *constant_expression* in einer DEFAULT-Definition kann nicht auf eine andere Spalte in der Tabelle oder auf andere Tabellen, Sichten oder gespeicherte Prozeduren verwiesen werden.
- DEFAULT-Definitionen können nicht für Spalten mit einem **timestamp**-Datentyp oder für Spalten mit IDENTITY-Eigenschaft erstellt werden.
- DEFAULT-Definitionen können nicht für Spalten mit Aliasdatentypen erstellt werden, wenn der Aliasdatentyp an ein Standardobjekt gebunden ist.

## <a name="check-constraints"></a>CHECK-Einschränkungen

- Eine Spalte kann beliebig viele CHECK-Einschränkungen haben, und die Bedingung kann mehrere logische Ausdrücke enthalten, die mit AND und OR verknüpft sind. Mehrere CHECK-Einschränkungen für eine Spalte werden in der Reihenfolge überprüft, in der sie erstellt wurden.
- Die Suchbedingung muss einen booleschen Ausdruck ergeben und darf nicht auf eine andere Tabelle verweisen.
- Eine CHECK-Einschränkung auf Spaltenebene kann nur auf die von der Einschränkung betroffene Spalte verweisen, und eine CHECK-Einschränkung auf Tabellenebene kann nur auf Spalten derselben Tabelle verweisen.

  CHECK-Einschränkungen und Regeln dienen beide zur Überprüfung der Daten während INSERT- und UPDATE-Anweisungen.

- Sobald eine Regel und mindestens eine CHECK-Einschränkung für eine oder mehrere Spalten vorhanden sind, werden alle Einschränkungen ausgewertet.
- CHECK-Einschränkungen können nicht für Spalten vom Typ **text**, **ntext** oder **image** definiert werden.

## <a name="additional-constraint-information"></a>Weitere Informationen zu Einschränkungen

- Ein für eine Einschränkung erstellter Index kann nicht mit `DROP INDEX` gelöscht werden. Die Einschränkung muss mithilfe von `ALTER TABLE` gelöscht werden. Ein Index, der für eine Einschränkung erstellt wurde und von ihr verwendet wird, kann mithilfe von `ALTER INDEX ... REBUILD` neu erstellt werden. Weitere Informationen finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).
- Einschränkungsnamen müssen den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen, wobei der Name nicht mit einem Nummernzeichen (#) beginnen darf. Wenn *constraint_name* nicht angegeben ist, vergibt das System einen Namen für die Einschränkung. Der Einschränkungsname wird in jeder Fehlermeldung über Einschränkungsverletzungen angezeigt.
- Wenn eine Einschränkung in einer `INSERT`-, `UPDATE`- oder `DELETE`-Anweisung verletzt wird, wird die Anweisung beendet. Wenn `SET XACT_ABORT` jedoch auf OFF festgelegt ist, wird die Verarbeitung der Transaktion fortgesetzt, wenn die Anweisung Teil einer expliziten Transaktion ist. Wenn `SET XACT_ABORT` auf ON festgelegt ist, wird für die ganze Transaktion ein Rollback ausgeführt. Sie können auch die `ROLLBACK TRANSACTION`-Anweisung mit der Transaktionsdefinition verwenden, indem Sie die `@@ERROR`-Systemfunktion prüfen.
- Wenn `ALLOW_ROW_LOCKS = ON` und `ALLOW_PAGE_LOCK = ON` angegeben sind, sind Sperren auf Zeilen-, Seiten- und Tabellenebene zulässig, wenn auf den Index zugegriffen wird. Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] wählt die geeignete Sperre aus und kann die Sperre von einer Zeilen- oder Seitensperre auf eine Tabellensperre ausweiten. Wenn `ALLOW_ROW_LOCKS = OFF` und `ALLOW_PAGE_LOCK = OFF` angegeben sind, ist nur eine Sperre auf Tabellenebene zulässig, wenn auf den Index zugegriffen wird.
- Wenn eine Tabelle FOREIGN KEY- oder CHECK-Einschränkungen und Trigger hat, werden die Einschränkungsbedingungen ausgewertet, bevor der Trigger ausgeführt wird.

Verwenden Sie `sp_help` oder `sp_helpconstraint`, um einen Bericht über eine Tabelle und deren Spalten zu erhalten. Verwenden Sie `sp_rename`, um eine Tabelle umzubenennen. Verwenden Sie für einen Bericht zu den Sichten und gespeicherten Prozeduren, die von einer Tabelle abhängen, [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md) und [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md).

## <a name="nullability-rules-within-a-table-definition"></a>NULL-Zulässigkeitsregeln in einer Tabellendefinition

Die NULL-Zulässigkeit einer Spalte bestimmt, ob diese Spalte NULL als Datenwert enthalten kann. NULL ist nicht null oder leer: NULL bedeutet, dass kein Eintrag vorgenommen oder explizit NULL angegeben wurde, und impliziert üblicherweise, dass der Wert entweder unbekannt oder nicht anwendbar ist.

Wenn Sie `CREATE TABLE` oder `ALTER TABLE` verwenden, beeinflussen Datenbank- und Sitzungseinstellungen die NULL-Zulässigkeit des in einer Spaltendefinition verwendeten Datentyps und überschreiben diese möglicherweise. Es empfiehlt sich, bei nicht berechneten Spalten stets explizit NULL oder NOT NULL für die Spalte anzugeben oder, im Falle eines benutzerdefinierten Datentyps, zuzulassen, dass die Spalte die standardmäßige NULL-Zulässigkeit des Datentyps verwendet. Spalten mit geringer Dichte müssen immer NULL zulassen.

Wenn die NULL-Zulässigkeit der Spalte nicht explizit angegeben ist, wird sie gemäß den in der folgenden Tabelle aufgeführten Regeln hergeleitet.

|Spaltendatentyp|Regel|
|----------------------|----------|
|Aliasdatentyp|Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] verwendet die NULL-Zulässigkeit, die beim Erstellen des Datentyps angegeben wurde. Verwenden Sie **sp_help** zum Bestimmen der standardmäßigen NULL-Zulässigkeit des Datentyps.|
|CLR-benutzerdefinierter Typ|Die NULL-Zulässigkeit wird gemäß der Spaltendefinition bestimmt.|
|Vom System bereitgestellter Datentyp|Wenn es für den vom System bereitgestellten Datentyp nur eine Option gibt, hat diese Vorrang. Für **timestamp**-Datentypen muss NOT NULL angegeben werden. Wenn Sitzungseinstellungen mithilfe von SET auf ON festgelegt werden, gilt Folgendes:<br />Wenn **ANSI_NULL_DFLT_ON** auf ON festgelegt ist, wird NULL zugewiesen. <br />Wenn **ANSI_NULL_DFLT_OFF** auf ON festgelegt ist, wird NOT NULL zugewiesen.<br /><br /> Wenn Datenbankeinstellungen mithilfe von ALTER DATABASE konfiguriert werden, gilt Folgendes:<br />Wenn **ANSI_NULL_DEFAULT_ON** auf ON festgelegt ist, wird NULL zugewiesen. <br />Wenn **ANSI_NULL_DEFAULT_OFF** auf ON festgelegt ist, wird NOT NULL zugewiesen.<br /><br /> Zum Anzeigen der Datenbankeinstellungen für ANSI_NULL_DEFAULT verwenden Sie die Katalogsicht **sys.databases**.|

Wenn keine der ANSI_NULL_DFLT-Optionen für die Sitzung festgelegt wurde und für die Datenbank die Standardeinstellung gilt (ANSI_NULL_DEFAULT ist auf OFF festgelegt), wird der Standardwert NOT NULL zugewiesen.

Wenn es sich bei der Spalte um eine berechnete Spalte handelt, wird die NULL-Zulässigkeit stets automatisch durch das [!INCLUDE[ssDE](../../includes/ssde-md.md)] bestimmt. Zum Bestimmen der NULL-Zulässigkeit dieser Art von Spalte verwenden Sie die Funktion `COLUMNPROPERTY` mit der Eigenschaft **AllowsNull**.

> [!NOTE]
> Für den SQL Server-ODBC-Treiber und den SQL Server OLE DB-Treiber ist ANSI_NULL_DFLT_ON standardmäßig auf ON festgelegt. ODBC- und OLE DB-Benutzer können dies in ODBC-Datenquellen oder mit von der Anwendung festgelegten Verbindungsattributen oder -eigenschaften konfigurieren.

## <a name="data-compression"></a>Datenkomprimierung

Für Systemtabellen ist die Komprimierung nicht verfügbar. Bei Erstellung einer Tabelle wird die Datenkomprimierung auf NONE festgelegt, falls nicht anders angegeben. Wenn Sie eine Partitionsliste bzw. eine Partition außerhalb des zulässigen Bereichs angeben, wird ein Fehler generiert. Weitere Informationen zur Datenkomprimierung finden Sie unter [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md).

Mit der gespeicherten Prozedur [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) können Sie einschätzen, wie sich eine Änderung des Komprimierungsstatus auf eine Tabelle, einen Index oder eine Partition auswirkt.

## <a name="permissions"></a>Berechtigungen

Erfordert die `CREATE TABLE`-Berechtigung in der Datenbank und die `ALTER`-Berechtigung für das Schema, in dem die Tabelle erstellt wird.

Wenn in der `CREATE TABLE`-Anweisung eine Spalte als Spalte eines benutzerdefinierten Typs definiert wird, ist die `REFERENCES`-Berechtigung für den benutzerdefinierten Typ erforderlich.

Wenn in der `CREATE TABLE`-Anweisung eine Spalte als Spalte eines CLR-benutzerdefinierten Typs definiert wird, ist entweder der Besitz des Typs oder die `REFERENCES`-Berechtigung für den Typ erforderlich.

Wenn einer Spalte in der `CREATE TABLE`-Anweisung eine XML-Schemaauflistung zugeordnet ist, ist entweder der Besitz der XML-Schemaauflistung oder die entsprechende `REFERENCES`-Berechtigung erforderlich.

Jeder Benutzer ist berechtigt, temporäre Tabellen in tempdb zu erstellen.

## <a name="examples"></a>Beispiele

### <a name="a-create-a-primary-key-constraint-on-a-column"></a>A. Erstellen einer PRIMARY KEY-Einschränkung für eine Spalte

Im folgenden Beispiel ist die Spaltendefinition für eine PRIMARY KEY-Einschränkung dargestellt, die über einen gruppierten Index für die `EmployeeID`-Spalte der `Employee`-Tabelle verfügt. Da kein Einschränkungsname angegeben ist, wird der Einschränkungsname vom System angegeben.

```sql
CREATE TABLE dbo.Employee (
    EmployeeID INT PRIMARY KEY CLUSTERED
);
```

### <a name="b-using-foreign-key-constraints"></a>B. Verwenden von FOREIGN KEY-Einschränkungen

Eine FOREIGN KEY-Einschränkung wird zum Verweisen auf eine andere Tabelle verwendet. Fremdschlüssel können einspaltige oder mehrspaltige Schlüssel sein. Dieses Beispiel zeigt eine einspaltige FOREIGN KEY-Einschränkung für die `SalesOrderHeader`-Tabelle, die auf die `SalesPerson`-Tabelle verweist. Für eine einspaltige FOREIGN KEY-Einschränkung wird nur die REFERENCES-Klausel benötigt.

```sql
SalesPersonID INT NULL REFERENCES SalesPerson(SalesPersonID)
```

Sie können auch explizit die FOREIGN KEY-Klausel verwenden und das Spaltenattribut nochmals nennen. Beachten Sie, dass der Spaltenname in den beiden Tabellen nicht identisch sein muss.

```sql
FOREIGN KEY (SalesPersonID) REFERENCES SalesPerson(SalesPersonID)
```

Einschränkungen für mehrspaltige Schlüssel werden als Tabelleneinschränkungen erstellt. Die `SpecialOfferProduct`-Tabelle in der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank enthält einen mehrspaltigen Primärschlüssel. Das folgende Beispiel zeigt, wie von einer anderen Tabelle aus auf diesen Schlüssel verwiesen wird; die Angabe eines expliziten Einschränkungsnamens ist optional.

```sql
CONSTRAINT FK_SpecialOfferProduct_SalesOrderDetail
    FOREIGN KEY (ProductID, SpecialOfferID)
    REFERENCES SpecialOfferProduct (ProductID, SpecialOfferID)
```

### <a name="c-using-unique-constraints"></a>C. Verwenden von UNIQUE-Einschränkungen

UNIQUE-Einschränkungen werden verwendet, um Eindeutigkeit für Nicht-Primärschlüsselspalten zu erzwingen. Im folgenden Beispiel wird eine Einschränkung erzwungen, durch die festgelegt wird, dass die `Name`-Spalte der `Product`-Tabelle eindeutig sein muss.

```sql
Name NVARCHAR(100) NOT NULL
UNIQUE NONCLUSTERED
```

### <a name="d-using-default-definitions"></a>D: Verwenden von DEFAULT-Definitionen

Standardwerte stellen jeweils einen Wert bereit (in INSERT- und UPDATE-Anweisungen), wenn kein Wert angegeben ist. Die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank könnte beispielsweise eine Nachschlagetabelle enthalten, in der die verschiedenen Tätigkeiten aufgelistet sind, die Mitarbeiter in dem Unternehmen ausüben können. In einer Spalte, in der jede Tätigkeit beschrieben wird, könnte ein Zeichenfolgen-Standardwert eine Beschreibung bereitstellen, falls keine explizite Angabe einer Beschreibung erfolgt.

```sql
DEFAULT 'New Position - title not formalized yet'
```

Neben Konstanten können DEFAULT-Definitionen auch Funktionen enthalten. Verwenden Sie das folgende Beispiel, um das aktuelle Datum für einen Eintrag zu erhalten.

```sql
DEFAULT (GETDATE())
```

Eine Funktion ohne Argumente kann ebenfalls zur Verbesserung der Datenintegrität beitragen. Verwenden Sie die Funktion ohne Argumente für USER, um den Benutzer nachzuverfolgen, der eine Zeile einfügt. Schließen Sie die Funktionen ohne Argumente nicht in Klammern ein.

```sql
DEFAULT USER
```

### <a name="e-using-check-constraints"></a>E. Verwenden von CHECK-Einschränkungen

Im folgenden Beispiel wird eine Einschränkung gezeigt, die für Werte gilt, die in die `CreditRating`-Spalte der `Vendor`-Tabelle eingegeben werden. Die Einschränkung ist nicht benannt.

```sql
CHECK (CreditRating >= 1 and CreditRating <= 5)
```

Dieses Beispiel zeigt eine benannte Einschränkung mit Mustereinschränkung für die Zeichendaten, die in eine Spalte einer Tabelle eingegeben werden.

```sql
CONSTRAINT CK_emp_id CHECK (
    emp_id LIKE '[A-Z][A-Z][A-Z][1-9][0-9][0-9][0-9][0-9][FM]'
    OR emp_id LIKE '[A-Z]-[A-Z][1-9][0-9][0-9][0-9][0-9][FM]'
)
```

Dieses Beispiel gibt an, dass die Werte in einer speziellen Liste enthalten sein müssen oder einem bestimmten Muster entsprechen müssen.

```sql
CHECK (
    emp_id IN ('1389', '0736', '0877', '1622', '1756')
    OR emp_id LIKE '99[0-9][0-9]'
)
```

### <a name="f-showing-the-complete-table-definition"></a>F. Anzeigen der vollständigen Tabellendefinition

Im folgenden Beispiel werden die vollständigen Tabellendefinitionen mit allen Einschränkungsdefinitionen für die in der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank erstellte `PurchaseOrderDetail`-Tabelle angezeigt. Beachten Sie, dass zum Ausführen des Beispiels das Tabellenschema nach `dbo` geändert wird.

```sql
CREATE TABLE dbo.PurchaseOrderDetail
(
    PurchaseOrderID int NOT NULL
        REFERENCES Purchasing.PurchaseOrderHeader(PurchaseOrderID),
    LineNumber smallint NOT NULL,
    ProductID int NULL
        REFERENCES Production.Product(ProductID),
    UnitPrice money NULL,
    OrderQty smallint NULL,
    ReceivedQty float NULL,
    RejectedQty float NULL,
    DueDate datetime NULL,
    rowguid uniqueidentifier ROWGUIDCOL NOT NULL
        CONSTRAINT DF_PurchaseOrderDetail_rowguid DEFAULT (NEWID()),
    ModifiedDate datetime NOT NULL
        CONSTRAINT DF_PurchaseOrderDetail_ModifiedDate DEFAULT (GETDATE()),
    LineTotal AS ((UnitPrice*OrderQty)),
    StockedQty AS ((ReceivedQty-RejectedQty)),
    CONSTRAINT PK_PurchaseOrderDetail_PurchaseOrderID_LineNumber
               PRIMARY KEY CLUSTERED (PurchaseOrderID, LineNumber)
               WITH (IGNORE_DUP_KEY = OFF)
)
ON PRIMARY;
```

### <a name="g-creating-a-table-with-an-xml-column-typed-to-an-xml-schema-collection"></a>G. Erstellen einer Tabelle mit einer XML-Spalte, die mit einer XML-Schemaauflistung typisiert wird

Im folgenden Beispiel wird eine Tabelle mit einer `xml`-Spalte erstellt, die mit der XML-Schemaauflistung `HRResumeSchemaCollection` typisiert wird. Das Schlüsselwort `DOCUMENT` gibt an, dass jede Instanz des Datentyps `xml` in *column_name* nur ein allgemeines Element enthalten kann.

```sql
CREATE TABLE HumanResources.EmployeeResumes
(
    LName nvarchar(25),
    FName nvarchar(25),
    Resume xml(DOCUMENT HumanResources.HRResumeSchemaCollection)
);
```

### <a name="h-creating-a-partitioned-table"></a>H. Erstellen einer partitionierten Tabelle

Im folgenden Beispiel wird eine Partitionsfunktion zum Partitionieren einer Tabelle oder eines Indexes in vier Partitionen erstellt. Anschließend wird im Beispiel ein Partitionsschema erstellt, das die Dateigruppen angibt, die jede der vier Partitionen aufnehmen sollen. Schließlich wird eine Tabelle erstellt, die das Partitionsschema verwendet. In diesem Beispiel wird davon ausgegangen, dass die Dateigruppen bereits in der Datenbank vorhanden sind.

```sql
CREATE PARTITION FUNCTION myRangePF1 (int)
    AS RANGE LEFT FOR VALUES (1, 100, 1000);
GO

CREATE PARTITION SCHEME myRangePS1
    AS PARTITION myRangePF1
    TO (test1fg, test2fg, test3fg, test4fg);
GO  
  
CREATE TABLE PartitionTable (col1 int, col2 char(10))
    ON myRangePS1 (col1);
GO
```

Basierend auf den Werten der `col1`-Spalte von `PartitionTable` werden die Partitionen folgendermaßen zugewiesen.

|Dateigruppe|test1fg|test2fg|test3fg|test4fg|
|---------------|-------------|-------------|-------------|-------------|
|**Partition**|1|2|3|4|
|**Werte**|col1 \<= 1|col1 > 1 AND col1 \<= 100|col1 > 100 AND col1 \<= 1,000|col1 > 1000|

### <a name="i-using-the-uniqueidentifier-data-type-in-a-column"></a>I. Verwenden des uniqueidentifier-Datentyps in einer Spalte

Im folgenden Beispiel wird eine Tabelle mit einer `uniqueidentifier`-Spalte erstellt. In dem Beispiel wird eine PRIMARY KEY-Einschränkung verwendet, um zu verhindern, dass Benutzer doppelte Werte in die Tabelle einfügen. Mithilfe der `NEWSEQUENTIALID()`-Funktion in der `DEFAULT`-Einschränkung werden Werte für neue Zeilen bereitgestellt. Die ROWGUIDCOL-Eigenschaft wird auf die `uniqueidentifier`-Spalte angewendet, sodass mit dem $ROWGUID-Schlüsselwort auf sie verwiesen werden kann.

```sql
CREATE TABLE dbo.Globally_Unique_Data
(
    GUID UNIQUEIDENTIFIER
        CONSTRAINT Guid_Default DEFAULT
        NEWSEQUENTIALID() ROWGUIDCOL,
    Employee_Name VARCHAR(60)
    CONSTRAINT Guid_PK PRIMARY KEY (GUID)
);
```

### <a name="j-using-an-expression-for-a-computed-column"></a>J. Verwenden eines Ausdrucks für eine berechnete Spalte

Im folgenden Beispiel wird die Verwendung eines Ausdrucks (`(low + high)/2`) zum Berechnen der berechneten Spalte `myavg` verwendet.

```sql
CREATE TABLE dbo.mytable
(
    low INT,
    high INT,
    myavg AS (low + high)/2
);
```

### <a name="k-creating-a-computed-column-based-on-a-user-defined-type-column"></a>K. Erstellen einer berechneten Spalte basierend auf einer Spalte eines benutzerdefinierten Typs

Im folgenden Beispiel wird eine Tabelle mit einer Spalte erstellt, die als Spalte des benutzerdefinierten Typs `utf8string` definiert ist. Hierbei wird vorausgesetzt, dass die Assembly des Typs und der Typ selbst bereits in der aktuellen Datenbank erstellt wurden. Eine zweite Spalte wird basierend auf `utf8string` definiert. Hierbei wird die `ToString()`-Methode von **type(class)** `utf8string` verwendet, um einen Wert für die Spalte zu berechnen.

```sql
CREATE TABLE UDTypeTable
(
    u UTF8STRING,
    ustr AS u.ToString() PERSISTED
);
```

### <a name="l-using-the-user_name-function-for-a-computed-column"></a>L. Verwenden der USER_NAME-Funktion für eine berechnete Spalte

Im folgenden Beispiel wird die `USER_NAME()`-Funktion in der `myuser_name`-Spalte verwendet.

```sql
CREATE TABLE dbo.mylogintable
(
    date_in DATETIME,
    user_id INT,
    myuser_name AS USER_NAME()
);
```

### <a name="m-creating-a-table-that-has-a-filestream-column"></a>M. Erstellen einer Tabelle mit einer FILESTREAM-Spalte

Im folgenden Beispiel wird eine Tabelle mit der `FILESTREAM`-Spalte `Photo` erstellt. Eine Tabelle mit einer oder mehreren `FILESTREAM`-Spalten muss eine `ROWGUIDCOL`-Spalte enthalten.

```sql
CREATE TABLE dbo.EmployeePhoto
(
    EmployeeId INT NOT NULL PRIMARY KEY,
    Photo VARBINARY(MAX) FILESTREAM NULL,
    MyRowGuidColumn UNIQUEIDENTIFIER NOT NULL ROWGUIDCOL UNIQUE DEFAULT NEWID()
);
```

### <a name="n-creating-a-table-that-uses-row-compression"></a>N. Erstellen einer Tabelle, in der Zeilenkomprimierung verwendet wird

Im folgenden Beispiel wird eine Tabelle erstellt, in der Zeilenkomprimierung verwendet wird.

```sql
CREATE TABLE dbo.T1
(
    c1 INT,
    c2 NVARCHAR(200)
)
WITH (DATA_COMPRESSION = ROW);
```

Weitere Beispiele für die Datenkomprimierung finden Sie unter [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md).

### <a name="o-creating-a-table-that-has-sparse-columns-and-a-column-set"></a>O. Erstellen einer Tabelle mit Sparsespalten und einem Spaltensatz

Anhand der folgenden Beispiele wird gezeigt, wie Sie eine Tabelle mit einer Sparsespalte und eine Tabelle mit zwei Sparsespalten und einem Spaltensatz erstellen. In den Beispielen wird die grundlegende Syntax verwendet. Komplexere Beispiele finden Sie unter [Verwenden von Spalten mit geringer Dichte](../../relational-databases/tables/use-sparse-columns.md) und [Verwenden von Spaltensätzen](../../relational-databases/tables/use-column-sets.md).

In diesem Beispiel wird eine Tabelle erstellt, die eine Sparsespalte enthält.

```sql
CREATE TABLE dbo.T1
(
    c1 INT PRIMARY KEY,
    c2 VARCHAR(50) SPARSE NULL
);
```

In diesem Beispiel wird eine Tabelle erstellt, die zwei Sparsespalten und einen Spaltensatz mit dem Namen `CSet` enthält.

```sql
CREATE TABLE T1
(
    c1 INT PRIMARY KEY,
    c2 VARCHAR(50) SPARSE NULL,
    c3 INT SPARSE NULL,
    CSet XML COLUMN_SET FOR ALL_SPARSE_COLUMNS
);
```

### <a name="p-creating-a-system-versioned-disk-based-temporal-table"></a>P. Erstellen einer datenträgerbasierten temporalen Tabelle mit Systemversionsverwaltung

**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

In den folgenden Beispielen wird gezeigt, wie Sie temporale Tabellen erstellen, die mit einer neuen Verlaufstabelle verbunden sind, und wie Sie temporale Tabellen erstellen, die mit einer vorhandenen Verlaufstabelle verbunden sind. Beachten Sie, dass der Primärschlüssel der temporalen Tabelle definiert sein muss, damit die Versionsverwaltung durch das System für die Tabelle aktiviert werden kann. Beispiele für das Hinzufügen und Entfernen der Versionsverwaltung durch das System bei einer vorhandenen Tabelle finden Sie unter [Beispiele für die Versionsverwaltung durch das System](../../t-sql/statements/alter-table-transact-sql.md#Example_Top). Anwendungsfälle finden Sie unter [Temporale Tabellen](../../relational-databases/tables/temporal-tables.md).

In diesem Beispiel wird eine neue temporale Tabelle mit einer neuen Verlaufstabelle verbunden.

```sql
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON);
```

In diesem Beispiel wird eine neue temporale Tabelle mit einer vorhandenen Verlaufstabelle verbunden.

```sql
-- Existing table
CREATE TABLE Department_History
(
    DepartmentNumber CHAR(10) NOT NULL,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 NOT NULL,
    SysEndTime DATETIME2 NOT NULL
);

-- Temporal table
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.Department_History, DATA_CONSISTENCY_CHECK = ON));
```

### <a name="q-creating-a-system-versioned-memory-optimized-temporal-table"></a>Q. Erstellen einer speicheroptimierten temporalen Tabelle mit Systemversionsverwaltung

**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Im folgenden Beispiel wird gezeigt, wie eine speicheroptimierte temporale Tabelle mit Versionsverwaltung durch das System erstellt wird, die mit einer neuen datenträgerbasierten Verlaufstabelle verbunden ist.

In diesem Beispiel wird eine neue temporale Tabelle mit einer neuen Verlaufstabelle verbunden.

```sql
CREATE SCHEMA History;
GO

CREATE TABLE dbo.Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY NONCLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH
(
    MEMORY_OPTIMIZED = ON,
    DURABILITY = SCHEMA_AND_DATA,
    SYSTEM_VERSIONING = ON (HISTORY_TABLE = History.DepartmentHistory)
);
```

In diesem Beispiel wird eine neue temporale Tabelle mit einer vorhandenen Verlaufstabelle verbunden.

```sql
-- Existing table
CREATE TABLE Department_History
(
    DepartmentNumber CHAR(10) NOT NULL,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 NOT NULL,
    SysEndTime DATETIME2 NOT NULL
);

-- Temporal table
CREATE TABLE Department
(
    DepartmentNumber CHAR(10) NOT NULL PRIMARY KEY CLUSTERED,
    DepartmentName VARCHAR(50) NOT NULL,
    ManagerID INT NULL,
    ParentDepartmentNumber CHAR(10) NULL,
    SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL,
    SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL,
    PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH
(
    SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.Department_History, DATA_CONSISTENCY_CHECK = ON)
);
```

### <a name="r-creating-a-table-with-encrypted-columns"></a>R. Erstellen einer Tabelle mit verschlüsselten Spalten

Im folgenden Beispiel wird eine Tabelle mit zwei verschlüsselten Spalten erstellt. Weitere Informationen hierzu finden Sie unter [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md).

```sql
CREATE TABLE Customers (
    CustName NVARCHAR(60)
        ENCRYPTED WITH (
            COLUMN_ENCRYPTION_KEY = MyCEK,
            ENCRYPTION_TYPE = RANDOMIZED,
            ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ),
    SSN VARCHAR(11) COLLATE Latin1_General_BIN2
        ENCRYPTED WITH (
            COLUMN_ENCRYPTION_KEY = MyCEK,
            ENCRYPTION_TYPE = DETERMINISTIC ,
            ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'
        ),
    Age INT NULL
);
```

### <a name="s-create-an-inline-filtered-index"></a>S. Erstellen eines inlinegefilterten Index

Erstellt eine Tabelle mit einem inlinegefilterten Index.

```sql
CREATE TABLE t1
(
    c1 INT,
    index IX1 (c1) WHERE c1 > 0
);
```

### <a name="t-create-an-inline-index"></a>T. Erstellen eines Inlineindex

Im Folgenden wird veranschaulicht, wie ein NONCLUSTERED-Inlineindex in einer datenträgerbasierten Tabelle verwendet wird:

```sql
CREATE TABLE t1
(
    c1 INT,
    INDEX ix_1 NONCLUSTERED (c1)
);

CREATE TABLE t2
(
    c1 INT,
    c2 INT INDEX ix_1 NONCLUSTERED
);

CREATE TABLE t3
(
    c1 INT,
    c2 INT,
    INDEX ix_1 NONCLUSTERED (c1,c2)
);
```

### <a name="u-create-a-temporary-table-with-an-anonymously-named-compound-primary-key"></a>U. Erstellen einer temporären Tabelle mit einem anonym benannten zusammengesetzten Primärschlüssel

Erstellt eine temporäre Tabelle mit einem anonym benannten zusammengesetzten Primärschlüssel. Dies ist nützlich, um Laufzeitkonflikte zu vermeiden, bei denen zwei, für die ganze Sitzung gültigen temporären Tabellen, jede in einem separaten Modus, den gleichen Namen für eine Einschränkung verwenden.

```sql
CREATE TABLE #tmp
(
    c1 INT,
    c2 INT,
    PRIMARY KEY CLUSTERED ([c1], [c2])
);
GO
```

Wenn Sie die Einschränkung explizit benennen, generiert die zweite Sitzung einen Fehler, wie z.B.:

```
Msg 2714, Level 16, State 5, Line 1
There is already an object named 'PK_#tmp' in the database.
Msg 1750, Level 16, State 1, Line 1
Could not create constraint or index. See previous errors.
```

Das Problem ergibt sich aus der Tatsache, dass der Name der temporären Tabelle zwar eindeutig ist, die Einschränkungsnamen aber nicht.

### <a name="v-using-global-temporary-tables-in-azure-sql-database"></a>V. Verwenden globaler temporärer Tabellen in Azure SQL-Datenbank

Sitzung A erstellt die globale temporäre Tabelle „##test“ in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] „testdb1“und fügt eine Zeile hinzu.

```sql
CREATE TABLE ##test (
    a INT,
    b INT
);

INSERT INTO ##test
VALUES (1, 1);

-- Obtain object ID for temp table ##test
SELECT OBJECT_ID('tempdb.dbo.##test') AS 'Object ID';
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
1253579504
```

Abrufen eines globalen Namens einer temporären Tabelle für eine spezifische Objekt-ID (1253579504) in tempdb (2)

```sql
SELECT name FROM tempdb.sys.objects WHERE object_id = 1253579504;
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```sql
##test
```

Sitzung B stellt eine Verbindung zu „testdb1“ in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] her und kann auf die von Sitzung A erstellte Tabelle „##test“ zugreifen.

```sql
SELECT * FROM ##test;
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```sql
1, 1
```

Sitzung C stellt eine Verbindung zu einer anderen Datenbank in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] namens „testdb2“ her und möchte auf die Tabelle „##test“ in der Datenbank „testdb1“ zugreifen. SELECT schlägt aufgrund des Datenbankbereichs für die globalen temporären Tabellen fehl.

```sql
SELECT * FROM ##test
```

Dies generiert den folgenden Fehler:

```
Msg 208, Level 16, State 0, Line 1
Invalid object name '##test'
```

Adressieren des Systemobjekts in „tempdb“ in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] aus der aktuellen Benutzerdatenbank „testdb1“.

```sql
SELECT * FROM tempdb.sys.objects;
SELECT * FROM tempdb.sys.columns;
SELECT * FROM tempdb.sys.database_files;
```   

### <a name="w-enable-data-retention-policy-on-a-table"></a>W. Aktivieren der Richtlinie für Datenaufbewahrung für eine Tabelle

Im folgenden Beispiel wird eine Tabelle erstellt, in der Datenaufbewahrung aktiviert ist, und es wird ein Aufbewahrungszeitraum von einer Woche angegeben. Dieses Beispiel gilt nur für **Azure SQL Edge**.

```sql
CREATE TABLE [dbo].[data_retention_table] 
(
  [dbdatetime2] datetime2(7), 
  [product_code] int, 
  [value] char(10)
) 
WITH (DATA_DELETION = ON ( FILTER_COLUMN = [dbdatetime2], RETENTION_PERIOD = 1 WEEKS ))
```

## <a name="next-steps"></a>Nächste Schritte

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md)
- [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)
- [CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)
- [Datentypen](../../t-sql/data-types/data-types-transact-sql.md)
- [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md)
- [sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)
- [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)
- [DROP TABLE](../../t-sql/statements/drop-table-transact-sql.md)
- [CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md)
- [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md)
- [CREATE TYPE](../../t-sql/statements/create-type-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [sp_help](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)
- [sp_helpconstraint](../../relational-databases/system-stored-procedures/sp-helpconstraint-transact-sql.md)
- [sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)
- [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
