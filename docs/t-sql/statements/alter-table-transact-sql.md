---
description: ALTER TABLE (Transact-SQL)
title: ALTER TABLE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WAIT_AT_LOW_PRIORITY
- ABORT_AFTER_WAIT
- ABORT_AFTER_WAIT_TSQL
- ALTER_TABLE_TSQL
- ALTER TABLE
- WAIT_AT_LOW_PRIORITY_TSQL
- ALTER_COLUMN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- columns [SQL Server], resizing
- changing column size
- MAXDOP index option, ALTER TABLE statement
- table modifications [SQL Server], ALTER TABLE
- ALTER TABLE statement
- modifying tables
- partitioned tables [SQL Server], lock escalation
- resizing columns
- removing columns
- switching partitions
- reassigning partitions
- removing constraints
- triggers [SQL Server], disabling
- columns [SQL Server], adding
- LOCK_ESCALATION option of ALTER TABLE
- constraints [SQL Server], deleting
- constraints [SQL Server], disabling
- triggers [SQL Server], enabling
- re-enabling constraints
- index modifications [SQL Server]
- disabling constraints
- columns [SQL Server], removing
- max degree of parallelism option
- locking [SQL Server], tables
- ONLINE option
- disabling triggers
- constraints [SQL Server], adding
- deleting constraints
- adding constraints
- adding columns
- SWITCH partitions
- partitioned tables [SQL Server], switching
- lock escalation [SQL Server], option of ALTER TABLE
- constraints [SQL Server], enabling
- dropping constraints
- dropping columns
- data retention policy
- table changes [SQL Server]
ms.assetid: f1745145-182d-4301-a334-18f799d361d1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6815d21b6a75216f4fc4a8bfccf2d2d3b0aa585e
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186522"
---
# <a name="alter-table-transact-sql"></a>ALTER TABLE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Ändert eine Tabellendefinition durch Ändern, Hinzufügen oder Löschen von Spalten und Einschränkungen. Mit ALTER TABLE können Sie zudem Partitionen neu zuweisen und erstellen oder Einschränkungen und Trigger deaktivieren und aktivieren.

Weitere Informationen zu Syntaxkonventionen finden Sie unter [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

> [!IMPORTANT]
> Die Syntax für ALTER TABLE unterscheidet sich für datenträgerbasierte und speicheroptimierte Tabellen. Über die folgenden Links gelangen Sie direkt zu dem für Ihre Tabellentypen geeigneten Syntaxblock sowie zu den Syntaxbeispielen.
>
> - Datenträgerbasierte Tabellen:
>
>   - [Syntax](#syntax-for-disk-based-tables)
>   - [Beispiele](#Example_Top)
> - Speicheroptimierte Tabellen
>
>   - [Syntax](#syntax-for-memory-optimized-tables)
>   - [Beispiele](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md)

## <a name="syntax-for-disk-based-tables"></a>Syntax für datenträgerbasierte Tabellen

```syntaxsql
ALTER TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
{
    ALTER COLUMN column_name
    {
        [ type_schema_name. ] type_name
            [ (
                {
                   precision [ , scale ]
                 | max
                 | xml_schema_collection
                }
            ) ]
        [ COLLATE collation_name ]
        [ NULL | NOT NULL ] [ SPARSE ]
      | { ADD | DROP }
          { ROWGUIDCOL | PERSISTED | NOT FOR REPLICATION | SPARSE | HIDDEN }
      | { ADD | DROP } MASKED [ WITH ( FUNCTION = ' mask_function ') ]
    }
    [ WITH ( ONLINE = ON | OFF ) ]
    | [ WITH { CHECK | NOCHECK } ]
  
    | ADD
    {
        <column_definition>
      | <computed_column_definition>
      | <table_constraint>
      | <column_set_definition>
    } [ ,...n ]
      | [ system_start_time_column_name datetime2 GENERATED ALWAYS AS ROW START
                [ HIDDEN ] [ NOT NULL ] [ CONSTRAINT constraint_name ]
            DEFAULT constant_expression [WITH VALUES] ,
                system_end_time_column_name datetime2 GENERATED ALWAYS AS ROW END
                   [ HIDDEN ] [ NOT NULL ][ CONSTRAINT constraint_name ]
            DEFAULT constant_expression [WITH VALUES] ,
        ]
       PERIOD FOR SYSTEM_TIME ( system_start_time_column_name, system_end_time_column_name )
    | DROP
     [ {
         [ CONSTRAINT ][ IF EXISTS ]
         {
              constraint_name
              [ WITH
               ( <drop_clustered_constraint_option> [ ,...n ] )
              ]
          } [ ,...n ]
          | COLUMN [ IF EXISTS ]
          {
              column_name
          } [ ,...n ]
          | PERIOD FOR SYSTEM_TIME
     } [ ,...n ]
    | [ WITH { CHECK | NOCHECK } ] { CHECK | NOCHECK } CONSTRAINT
        { ALL | constraint_name [ ,...n ] }
  
    | { ENABLE | DISABLE } TRIGGER
        { ALL | trigger_name [ ,...n ] }
  
    | { ENABLE | DISABLE } CHANGE_TRACKING
        [ WITH ( TRACK_COLUMNS_UPDATED = { ON | OFF } ) ]
  
    | SWITCH [ PARTITION source_partition_number_expression ]
        TO target_table
        [ PARTITION target_partition_number_expression ]
        [ WITH ( <low_priority_lock_wait> ) ]

    | SET
        (
            [ FILESTREAM_ON =
                { partition_scheme_name | filegroup | "default" | "NULL" } ]
            | SYSTEM_VERSIONING =
                  {
                      OFF
                  | ON
                      [ ( HISTORY_TABLE = schema_name . history_table_name
                          [, DATA_CONSISTENCY_CHECK = { ON | OFF } ]
                          [, HISTORY_RETENTION_PERIOD =
                          {
                              INFINITE | number {DAY | DAYS | WEEK | WEEKS
                  | MONTH | MONTHS | YEAR | YEARS }
                          }
                          ]
                        )
                      ]
                  }
            | DATA_DELETION =  
                {
                      OFF 
                    | ON  
                        [(  [ FILTER_COLUMN = column_name ]   
                            [, RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS 
                                    | MONTH | MONTHS | YEAR | YEARS }}]   
                        )]
                   }
    | REBUILD
      [ [PARTITION = ALL]
        [ WITH ( <rebuild_option> [ ,...n ] ) ]
      | [ PARTITION = partition_number
           [ WITH ( <single_partition_rebuild_option> [ ,...n ] ) ]
        ]
      ]
  
    | <table_option>
    | <filetable_option>
    | <stretch_configuration>
}
[ ; ]
  
-- ALTER TABLE options
  
<column_set_definition> ::=
    column_set_name XML COLUMN_SET FOR ALL_SPARSE_COLUMNS

<drop_clustered_constraint_option> ::=
    {
        MAXDOP = max_degree_of_parallelism
      | ONLINE = { ON | OFF }
      | MOVE TO
         { partition_scheme_name ( column_name ) | filegroup | "default" }
    }
<table_option> ::=
    {
        SET ( LOCK_ESCALATION = { AUTO | TABLE | DISABLE } )
    }
  
<filetable_option> ::=
    {
       [ { ENABLE | DISABLE } FILETABLE_NAMESPACE ]
       [ SET ( FILETABLE_DIRECTORY = directory_name ) ]
    }
  
<stretch_configuration> ::=
    {
      SET (
        REMOTE_DATA_ARCHIVE
        {
            = ON (<table_stretch_options>)
          | = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED )
          | ( <table_stretch_options> [, ...n] )
        }
            )
    }
  
<table_stretch_options> ::=
    {
     [ FILTER_PREDICATE = { null | table_predicate_function } , ]
       MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }
    }
  
<single_partition_rebuild__option> ::=
{
      SORT_IN_TEMPDB = { ON | OFF }
    | MAXDOP = max_degree_of_parallelism
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE} }
    | ONLINE = { ON [( <low_priority_lock_wait> ) ] | OFF }
}
  
<low_priority_lock_wait>::=
{
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ],
        ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )
}
```

> [!NOTE]
> Weitere Informationen finden Sie hier:
>
> - [ALTER TABLE column_constraint](alter-table-column-constraint-transact-sql.md)
> - [ALTER TABLE column_definition](alter-table-column-definition-transact-sql.md)
> - [ALTER TABLE computed_column_definition](alter-table-computed-column-definition-transact-sql.md)
> - [ALTER TABLE index_option](alter-table-index-option-transact-sql.md)
> - [ALTER TABLE table_constraints](alter-table-table-constraint-transact-sql.md)

## <a name="syntax-for-memory-optimized-tables"></a>Syntax für speicheroptimierte Tabellen

```syntaxsql
ALTER TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
{
    ALTER COLUMN column_name
    {
        [ type_schema_name. ] type_name
            [ (
                {
                   precision [ , scale ]
                }
            ) ]
        [ COLLATE collation_name ]
        [ NULL | NOT NULL ]
    }

    | ALTER INDEX index_name
    {
        [ type_schema_name. ] type_name
        REBUILD
        [ [ NONCLUSTERED ] WITH ( BUCKET_COUNT = bucket_count )
        ]
    }

    | ADD
    {
        <column_definition>
      | <computed_column_definition>
      | <table_constraint>
      | <table_index>
      | <column_index>
    } [ ,...n ]
  
    | DROP
     [ {
         CONSTRAINT [ IF EXISTS ]
         {
              constraint_name
          } [ ,...n ]
        | INDEX [ IF EXISTS ]
      {
         index_name
       } [ ,...n ]
          | COLUMN [ IF EXISTS ]
          {
              column_name
          } [ ,...n ]
          | PERIOD FOR SYSTEM_TIME
     } [ ,...n ]
    | [ WITH { CHECK | NOCHECK } ] { CHECK | NOCHECK } CONSTRAINT
        { ALL | constraint_name [ ,...n ] }

    | { ENABLE | DISABLE } TRIGGER
        { ALL | trigger_name [ ,...n ] }
  
    | SWITCH [ [ PARTITION ] source_partition_number_expression ]
        TO target_table
        [ PARTITION target_partition_number_expression ]
        [ WITH ( <low_priority_lock_wait> ) ]
    
}
[ ; ]

-- ALTER TABLE options

< table_constraint > ::=
 [ CONSTRAINT constraint_name ]
{
   {PRIMARY KEY | UNIQUE }
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
{[ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count)
  | [ NONCLUSTERED ] (column [ ASC | DESC ] [ ,... n ] )
      [ ON filegroup_name | default ]
  | CLUSTERED COLUMNSTORE [WITH ( COMPRESSION_DELAY = {0 | delay [Minutes]})]
      [ ON filegroup_name | default ]
}

```

## <a name="syntax-for-azure-synapse-analytics-and-parallel-data-warehouse"></a>Syntax für Azure Synapse Analytics und Parallel Data Warehouse

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

ALTER TABLE { database_name.schema_name.source_table_name | schema_name.source_table_name | source_table_name }
{
    ALTER COLUMN column_name
        {
            type_name [ ( precision [ , scale ] ) ]
            [ COLLATE Windows_collation_name ]
            [ NULL | NOT NULL ]
        }
    | ADD { <column_definition> | <column_constraint> FOR column_name} [ ,...n ]
    | DROP { COLUMN column_name | [CONSTRAINT] constraint_name } [ ,...n ]
    | REBUILD {
            [ PARTITION = ALL [ WITH ( <rebuild_option> ) ] ]
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_option> ] ]
      }
    | { SPLIT | MERGE } RANGE (boundary_value)
    | SWITCH [ PARTITION source_partition_number
        TO target_table_name [ PARTITION target_partition_number ] [ WITH ( TRUNCATE_TARGET = ON | OFF )
}
[;]

<column_definition>::=
{
    column_name
    type_name [ ( precision [ , scale ] ) ]
    [ <column_constraint> ]
    [ COLLATE Windows_collation_name ]
    [ NULL | NOT NULL ]
}

<column_constraint>::=
    [ CONSTRAINT constraint_name ] 
    {
        DEFAULT constant_expression
        | PRIMARY KEY NONCLUSTERED (column_name [ ,... n ]) NOT ENFORCED -- Applies to Azure Synapse Analytics only
        | UNIQUE (column_name [ ,... n ]) NOT ENFORCED -- Applies to Azure Synapse Analytics only
    }
<rebuild_option > ::=
{
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]
}

<single_partition_rebuild_option > ::=
{
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
}
```

[!INCLUDE[synapse-analytics-od-supported-tables](../../includes/synapse-analytics-od-supported-tables.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

*database_name*  
Der Name der Datenbank, in der die Tabelle erstellt wurde.

*schema_name*  
Der Name des Schemas, zu dem die Tabelle gehört.

*table_name*  
Der Name der Tabelle, die geändert werden soll. Wenn die Tabelle nicht in der aktuellen Datenbank oder in dem Schema enthalten ist, das dem aktuellen Benutzer gehört, müssen Sie die Datenbank und das Schema explizit angeben.

ALTER COLUMN  
Gibt an, dass die benannte Spalte geändert werden soll.

Für die geänderte Spalte gilt Folgendes:

- Eine Spalte mit dem **timestamp**-Datentyp.
- Die Spalte darf nicht die ROWGUIDCOL-Spalte der Tabelle sein.
- Die Spalte darf keine berechnete Spalte sein und nicht in einer berechneten Spalte verwendet werden.
- Die Spalte darf nicht in mit der CREATE STATISTICS-Anweisung generierten Statistiken verwendet werden. Benutzer müssen DROP STATISTICS ausführen, um die Statistiken zu löschen, bevor ALTER COLUMN erfolgreich ausgeführt werden kann.  Führen Sie diese Abfrage aus, um alle vom Benutzer erstellten Statistiken und Statistikspalten für eine Tabelle abzurufen.

``` sql

SELECT s.name AS statistics_name  
      ,c.name AS column_name  
      ,sc.stats_column_id  
FROM sys.stats AS s  
INNER JOIN sys.stats_columns AS sc   
    ON s.object_id = sc.object_id AND s.stats_id = sc.stats_id  
INNER JOIN sys.columns AS c   
    ON sc.object_id = c.object_id AND c.column_id = sc.column_id  
WHERE s.object_id = OBJECT_ID('<table_name>'); 

```
   > [!NOTE]
   > Vom Abfrageoptimierer automatisch generierte Statistiken werden von ALTER COLUMN automatisch gelöscht.

- Die Spalte darf nicht in einer PRIMARY KEY- oder [FOREIGN KEY] REFERENCES-Einschränkung verwendet werden.
- Die Spalte darf nicht in einer CHECK- oder UNIQUE-Einschränkung verwendet werden. Das Ändern der Länge einer Spalte mit variabler Länge, die in einer CHECK- oder UNIQUE-Einschränkung verwendet wird, ist dagegen zulässig.
- Der Spalte darf keine Standarddefinition zugeordnet sein. Die Länge, die Genauigkeit oder die Dezimalstellen einer Spalte können jedoch geändert werden, sofern der Datentyp nicht geändert wird.

Der Datentyp von **text**-, **ntext**- und **image**-Spalten kann nur auf die folgende Weise geändert werden:

- **text** in **varchar(max)** , **nvarchar(max)** oder **xml**
- **ntext** in **varchar(max)** , **nvarchar(max)** oder **xml**
- **image** in **varbinary(max)**

Änderungen des Datentyps können Datenänderungen zur Folge haben. Beispielsweise kann die Änderung einer Spalte vom Datentyp **nchar** oder **nvarchar** in **char** oder **varchar** zur Konvertierung erweiterter Zeichen führen. Weitere Informationen finden Sie unter [CAST und CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md). Das Reduzieren der Genauigkeit und der Dezimalstellen einer Spalte kann zum Abschneiden von Daten führen.

> [!NOTE]
> Der Datentyp einer Spalte einer partitionierten Tabelle kann nicht geändert werden.
>
> Der Datentyp der Spalten, die in einem Index enthalten sind, kann nicht geändert werden, es sei denn, die Spalte ist vom Datentyp **varchar**, **nvarchar** oder **varbinary**, und die neue Größe ist größer oder gleich der alten Größe.
>
> Spalten, die in einer PRIMARY KEY-Einschränkung enthalten sind, können nicht von **NOT NULL** in **NULL** geändert werden.

Wenn Sie Always Encrypted (ohne Secure Enclaves) verwenden, können Sie, falls die Spalte, die modifiziert wird, mit ENCRYPTED WITH verschlüsselt ist, den Datentyp in einen kompatiblen Datentyp (wie etwa INT oder BIGINT) ändern, aber Sie können die Verschlüsselungseinstellungen nicht anpassen.

Wenn Sie Always Encrypted mit Secure Enclaves verwenden, können Sie alle Verschlüsselungseinstellungen ändern, solange der Spaltenverschlüsselungsschlüssel, der die Spalte schützt (und der neue Spaltenverschlüsselungsschlüssel, wenn Sie den Schlüssel ändern), Enclave-Berechnungen unterstützt (verschlüsselt mit Enclave-fähigen Spaltenhauptschlüsseln). Weitere Einzelheiten finden Sie unter [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

*column_name*  
Der Name der Spalte, die geändert, hinzugefügt oder gelöscht werden soll. *column_name* darf maximal 128 Zeichen lang sein. Bei neuen Spalten, die mit einem **timestamp**-Datentyp erstellt wurden, ist *column_name* nicht erforderlich. Der Name **timestamp** wird verwendet, wenn Sie *column_name* nicht für eine Spalte vom Datentyp **timestamp** angeben.

> [!NOTE]
> Neue Spalten werden hinzugefügt, nachdem alle vorhandenen Spalten in der Tabelle geändert wurden.

[ _type\_schema\_name_ **.** ] _type\_name_  
Der neue Datentyp für die geänderte Spalte oder der Datentyp für die hinzugefügte Spalte. *type_name* kann für vorhandene Spalten von partitionierten Tabellen nicht angegeben werden. *type_name* kann einen der folgenden Typen aufweisen:

- Ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemdatentyp.
- Ein Aliasdatentyp, der auf einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemdatentyp basiert. Aliasdatentypen werden mit der CREATE TYPE-Anweisung erstellt, bevor sie in einer Tabellendefinition verwendet werden können.
- Ein benutzerdefinierter [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]-Datentyp und das Schema, zu dem er gehört. Benutzerdefinierte Typen werden mit der CREATE TYPE-Anweisung erstellt, bevor sie in einer Tabellendefinition verwendet werden können.

Es gelten folgende Kriterien für *type_name* in einer geänderten Spalte:

- Der vorherige Datentyp muss implizit in den neuen Datentyp konvertiert werden können.
- *type_name* darf nicht **timestamp** sein.
- ANSI NULL DEFAULT ist für ALTER COLUMN immer aktiviert. Fehlt die Angabe, so lässt die Spalte NULL-Werte zu.
- ANSI_PADDING ist für ALTER COLUMN immer auf ON festgelegt.
- Wenn die geänderte Spalte eine Identitätsspalte ist, muss *new_data_type* ein Datentyp sein, der die IDENTITY-Eigenschaft unterstützt.
- Die aktuelle Einstellung für SET ARITHABORT wird ignoriert. ALTER TABLE wird ausgeführt, als sei ARITHABORT aktiviert.

> [!NOTE]
> Falls die COLLATE-Klausel nicht angegeben wird, führt das Ändern des Datentyps einer Spalte dazu, dass die Sortierung in die Standardsortierung der Datenbank geändert wird.

*precision*  
Die Genauigkeit für den angegebenen Datentyp. Weitere Informationen über gültige Genauigkeitswerte finden Sie unter [Genauigkeit, Dezimalstellen und Länge](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

*scale*  
Die Dezimalstellen für den angegebenen Datentyp. Weitere Informationen zu gültigen Dezimalstellenwerten finden Sie unter [Genauigkeit, Dezimalstellen und Länge](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

**max**  
Gilt nur für die Datentypen **varchar**, **nvarchar**, und **varbinary** zum Speichern von 2^31-1 Bytes an Zeichen- und Binärdaten sowie von Unicode-Daten.

*xml_schema_collection*  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gilt nur für den **xml**-Datentyp zum Zuordnen eines XML-Schemas zum Typ. Bevor Sie eine **xml**-Spalte mit einer Schemaauflistung typisieren können, muss die Schemaauflistung mithilfe von [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md) in der Datenbank erstellt werden.

COLLATE \< *collation_name* >  
Gibt die neue Sortierung für die geänderte Spalte an. Wenn keine Sortierung angegeben ist, wird der Spalte die Standardsortierung der Datenbank zugewiesen. Als Sortierungsname kann entweder der Name einer Windows-Sortierreihenfolge oder ein SQL-Sortierungsname verwendet werden. Eine Liste und weitere Informationen finden Sie unter [Name der Windows-Sortierung](../../t-sql/statements/windows-collation-name-transact-sql.md) und [SQL Server-Sortierungsname](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

Mit der COLLATE-Klausel werden nur die Sortierungen von Spalten der Datentypen **char**, **varchar**, **nchar** und **nvarchar** geändert. Um die Sortierung einer benutzerdefinierten Aliasdatentyp-Spalte zu ändern, verwenden Sie separate ALTER TABLE-Anweisungen zum Ändern der Spalte in einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemdatentyp. Ändern Sie dann die Sortierung, und ändern Sie die Spalte zurück in einen Aliasdatentyp.

Mit ALTER COLUMN kann die Sortierung nicht geändert werden, wenn mindestens eine der folgenden Bedingungen zutrifft:

- Wenn eine CHECK-Einschränkung, eine FOREIGN KEY-Einschränkung oder berechnete Spalten auf die geänderte Spalte verweisen.
- Wenn ein Index, eine Statistik oder ein Volltextindex für die Spalte erstellt werden. Statistiken, die automatisch für die geänderte Spalte erstellt wurden, werden gelöscht, wenn die Spaltensortierung geändert wird.
- Wenn eine schemagebundene Sicht oder Funktion auf die Spalte verweist.

Weitere Informationen finden Sie unter [COLLATE](~/t-sql/statements/collations.md).

NULL | NOT NULL  
Gibt an, ob die Spalte NULL-Werte akzeptiert. Spalten, die keine NULL-Werte zulassen, können mit ALTER TABLE nur hinzugefügt werden, wenn für sie ein Standardwert angegeben ist oder wenn die Tabelle leer ist. NOT NULL kann nur für berechnete Spalten angegeben werden, wenn auch PERSISTED angegeben wird. Wenn die neue Spalte NULL-Werte zulässt und kein Standardwert angegeben wird, enthält sie einen NULL-Wert für jede Zeile in der Tabelle. Wenn die neue Spalte NULL-Werte zulässt und eine Standarddefinition mit der neuen Spalte hinzugefügt wird, können Sie WITH VALUES verwenden, um den Standardwert in der neuen Spalte für jede vorhandene Zeile in der Tabelle zu speichern.

Wenn die neue Spalte keine NULL-Werte zulässt und die Tabelle nicht leer ist, müssen Sie eine DEFAULT-Definition mit der neuen Spalte hinzufügen. Die neue Spalte wird zudem automatisch mit dem Standardwert in den neuen Spalten in jeder vorhandenen Zeile geladen.

Durch die Angabe von NULL in ALTER COLUMN kann erzwungen werden, dass eine NOT NULL-Spalte, mit Ausnahme von Spalten in PRIMARY KEY-Einschränkungen, NULL-Werte zulässt. NOT NULL kann nur in ALTER COLUMN angegeben werden, wenn die Spalte keine NULL-Werte enthält. Die NULL-Werte müssen auf einen beliebigen Wert aktualisiert werden, damit ALTER COLUMN NOT NULL zulässig ist. Beispiel:

```sql
UPDATE MyTable SET NullCol = N'some_value' WHERE NullCol IS NULL;
ALTER TABLE MyTable ALTER COLUMN NullCOl NVARCHAR(20) NOT NULL;
```

Wenn Sie eine Tabelle mit der CREATE TABLE- oder ALTER TABLE-Anweisung erstellen bzw. ändern, beeinflussen die Datenbank- und Sitzungseinstellungen die NULL-Zulässigkeit des in einer Spaltendefinition verwendeten Datentyps und setzen diese möglicherweise außer Kraft. Stellen Sie sicher, dass Sie eine nicht berechnete Spalte stets explizit als NULL oder NOT NULL definieren.

Wenn Sie eine Spalte mit einem benutzerdefinierten Datentyp hinzufügen, stellen Sie sicher, dass Sie die Spalte mit der gleichen NULL-Zulässigkeit wie der des benutzerdefinierten Datentyps definieren. Geben Sie zudem einen Standardwert für die Spalte an. Weitere Informationen finden Sie unter [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md).

> [!NOTE]
> Wenn NULL oder NOT NULL mit ALTER COLUMN angegeben werden, muss auch *new_data_type* [(*precision* [, *scale* ])] angegeben werden. Wenn Datentyp, Genauigkeit und Dezimalstellen nicht geändert werden, geben Sie die aktuellen Spaltenwerte an.

[ {ADD | DROP} ROWGUIDCOL ]  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gibt an, dass die ROWGUIDCOL-Eigenschaft der angegebenen Spalte hinzugefügt oder daraus gelöscht wird. ROWGUIDCOL zeigt an, dass die Spalte eine GUID-Spalte für eine Zeile darstellt. Sie können nur eine **uniqueidentifier**-Spalte pro Tabelle als ROWGUIDCOL-Spalte kennzeichnen. Die ROWGUIDCOL-Eigenschaft kann nur einer **uniqueidentifier**-Spalte zugewiesen werden. ROWGUIDCOL kann keiner Spalte eines benutzerdefinierten Datentyps zugewiesen werden.

ROWGUIDCOL erzwingt keine Eindeutigkeit der in der Spalte gespeicherten Werte und generiert nicht automatisch Werte für neue Zeilen, die in die Tabelle eingefügt werden. Um für jede Spalte eindeutige Werte zu generieren, verwenden Sie entweder die NEWID- oder die NEWSEQUENTIALID-Funktion in INSERT-Anweisungen. Geben Sie alternativ dazu die NEWID- oder die NEWSEQUENTIALID-Funktion als Standardwert für die Spalte an.

[ {ADD | DROP} PERSISTED ]  
Gibt an, dass die PERSISTED-Eigenschaft der angegebenen Spalte hinzugefügt oder aus ihr gelöscht wird. Die Spalte muss eine berechnete Spalte sein, die durch einen deterministischen Ausdruck definiert ist. Für Spalten, die als PERSISTED angegeben werden, speichert das [!INCLUDE[ssDE](../../includes/ssde-md.md)] die berechneten Werte physisch in der Tabelle und aktualisiert die Werte, wenn andere Spalten, von denen die berechnete Spalte abhängt, aktualisiert werden. Durch Kennzeichnen einer berechneten Spalte als PERSISTED können Sie Indizes für berechnete Spalten erstellen, die durch deterministische, aber nicht genaue Ausdrücke definiert sind. Weitere Informationen finden Sie unter [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md).

Jede berechnete Spalte, die als Partitionierungsspalte einer partitionierten Tabelle verwendet wird, muss explizit als PERSISTED gekennzeichnet sein.

DROP NOT FOR REPLICATION  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gibt an, dass Werte in Identitätsspalten inkrementiert werden, wenn Replikations-Agents Einfügevorgänge ausführen. Diese Klausel kann nur angegeben werden, wenn *column_name* eine Identitätsspalte ist.

SPARSE  
Gibt an, dass die Spalte eine Sparsespalte ist. Der Speicher für Sparsespalten ist für NULL-Werte optimiert. Sparsespalten können nicht als NOT NULL festgelegt werden. Beim Umwandeln einer Sparsespalte in eine Nicht-Sparsespalte oder umgekehrt wird die Tabelle für die Dauer der Befehlsausführung gesperrt. Sie müssen möglicherweise die REBUILD-Klausel verwenden, um Speicherplatzeinsparungen freizugeben. Weitere Einschränkungen und Informationen zu Sparsespalten finden Sie unter [Verwenden von Sparsespalten](../../relational-databases/tables/use-sparse-columns.md).

ADD MASKED WITH ( FUNCTION = ' *mask_function* ')  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gibt eine dynamische Datenmaske an. *mask_function* ist der Name der Maskierungsfunktion mit den entsprechenden Parametern. Drei Optionen stehen zur Verfügung:

- default()
- email()
- partial()
- random()

Verwenden Sie `DROP MASKED`, um eine Maske zu löschen. Weitere Informationen zu Funktionsparametern finden Sie im Artikel zur [dynamischen Datenmaskierung](../../relational-databases/security/dynamic-data-masking.md).

WITH ( ONLINE = ON | OFF) \<as applies to altering a column>  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Ermöglicht verschiedene Aktionen zum Ändern einer Spalte, während die Tabelle verfügbar bleibt. Die Standardeinstellung ist OFF. ALTER COLUMN kann online für Spaltenänderungen in Bezug auf Datentyp, Spaltenlänge, Genauigkeit, NULL-Zulässigkeit, geringe Dichte und Sortierung ausgeführt werden.

Die Onlineänderung ermöglicht, dass vom Benutzer und automatisch erstellte Statistiken weiterhin während des ALTER COLUMN-Vorgangs auf die geänderte Spalte verweisen. So können Abfragen wie gewohnt ausgeführt werden. Nach dem Beenden des Vorgangs werden automatisch erstellte Statistiken, die auf die Spalte verweisen, gelöscht, und vom Benutzer erstellte Statistiken werden ungültig. Der Benutzer muss benutzergenerierte Statistiken manuell aktualisieren, nachdem der Vorgang abgeschlossen wurde. Wenn die Spalte Teil eines Filterausdrucks für Statistiken oder Indizes ist, kann kein Spaltenänderungsvorgang ausgeführt werden.

- Während des Onlineänderungsvorgangs werden alle Vorgänge blockiert, die von der Spalte abhängig sein könnten (Index, Ansichten usw.), oder es tritt ein Fehler mit einer entsprechenden Fehlermeldung auf. Durch dieses Verhalten wird sichergestellt, dass bei der Onlineänderung der Spalte kein Fehler aufgrund von Abhängigkeiten auftritt, die während des Vorgangs entstanden sind.
- Das Ändern einer Spalte von NOT NULL auf NULL ist bei einem Onlinevorgang nicht möglich, wenn nicht gruppierte Indizes auf die geänderte Spalte verweisen.
- Die Onlineänderung wird nicht unterstützt, wenn auf die Spalte mit einer CHECK-Einschränkung verwiesen wird und der Änderungsvorgang die Genauigkeit der Spalte („numeric“ oder „datetime“) einschränkt.
- Die `WAIT_AT_LOW_PRIORITY`-Option kann für die Onlinespaltenänderung nicht verwendet werden.
- `ALTER COLUMN ... ADD/DROP PERSISTED` wird bei der Onlinespaltenänderung nicht unterstützt.
- `ALTER COLUMN ... ADD/DROP ROWGUIDCOL/NOT FOR REPLICATION` ist von der Onlinespaltenänderung nicht betroffen.
- Die Onlinespaltenänderung von Tabellen mit Änderungsnachverfolgung und von Tabellen, die ein Mergereplikationsverleger sind, wird nicht unterstützt.
- Die Onlinespaltenänderung unterstützt nicht das Ändern von oder in CLR-Datentypen.
- Die Änderung in einen XML-Datentyp mit einer von der aktuellen Schemaauflistung abweichenden Auflistung wird von der Onlinespaltenänderung nicht unterstützt.
- Durch die Onlinespaltenänderung werden die Einschränkungen, die festlegen, wann eine Spalte geändert werden kann, nicht verringert. Durch Verweise von Index/Statistiken usw. tritt möglicherweise ein Fehler beim Änderungsvorgang auf.
- Mit der Onlinespaltenänderung können nicht mehrere Spalten gleichzeitig geändert werden.
- Die Onlinespaltenänderung hat keine Auswirkungen, wenn es sich um eine temporale Tabelle mit Systemversionsverwaltung handelt. ALTER COLUMN wird nicht im Modus „online“ ausgeführt. Dies gilt unabhängig vom Wert, der für die Option ONLINE festgelegt wurde.

Für die Onlinespaltenänderung gelten ähnliche Anforderungen, Einschränkungen und Funktionen wie für die Online-Indexneuerstellung. Dazu gehören Folgende:

- Die Online-Indexneuerstellung wird nicht unterstützt, wenn die Tabelle ältere LOB- oder Filestream-Spalten enthält oder die Tabelle über einen Columnstore-Index verfügt. Die gleichen Einschränkungen gelten für die Onlinespaltenänderung.
- Eine vorhandene Spalte, die geändert wird, erfordert die doppelte Speicherplatzzuordnung: für die ursprüngliche Spalte und für die neu erstellte, ausgeblendete Spalte.
- Die Sperrstrategie während eines Onlinevorgangs zur Spaltenänderung folgt demselben Sperrmuster wie die Onlineindexerstellung.

WITH CHECK | WITH NOCHECK  
Gibt an, ob die Daten in der Tabelle bei einer neu hinzugefügten oder erneut aktivierten FOREIGN KEY- oder CHECK-Einschränkung überprüft werden. Fehlt die Angabe, so wird WITH CHECK für neue Einschränkungen und WITH NOCHECK für erneut aktivierte Einschränkungen angenommen.

Verwenden Sie WITH NOCHECK, wenn neue CHECK- oder FOREIGN KEY-Einschränkungen nicht für vorhandene Daten überprüft werden sollen. Diese Vorgehensweise wird nur in seltenen Fällen empfohlen. Die neue Einschränkung wird bei allen späteren Datenupdates ausgewertet. Einschränkungsverletzungen, die beim Hinzufügen der Einschränkung durch WITH NOCHECK unterdrückt werden, können zu Fehlern bei zukünftigen Updates führen, wenn Zeilen mit Daten aktualisiert werden, die der Einschränkung nicht entsprechen.

> [!NOTE]
> Der Abfrageoptimierer berücksichtigt mit WITH NOCHECK definierte Einschränkungen nicht. Diese Einschränkungen werden ignoriert, bis sie mithilfe von `ALTER TABLE table WITH CHECK CHECK CONSTRAINT ALL` erneut aktiviert werden.

ALTER INDEX *index_name*  
Gibt an, dass die Bucketanzahl von *index_name* geändert werden muss.

Die Syntax ALTER TABLE... ADD/DROP/ALTER INDEX wird nur für speicheroptimierte Tabelle unterstützt.

> [!IMPORTANT]
> Wenn keine ALTER TABLE-Anweisung verwendet wird, werden die Anweisungen [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) und [PAD_INDEX](alter-table-index-option-transact-sql.md) nicht für Indizes auf speicheroptimierten Tabellen unterstützt.

ADD  
Gibt an, dass eine oder mehrere Spaltendefinitionen, Definitionen berechneter Spalten oder Tabelleneinschränkungen hinzugefügt werden. Alternativ dazu werden die Spalten hinzugefügt, die das System zur Systemversionsverwaltung verwendet. Für speicheroptimierte Tabellen kann ein Index hinzugefügt werden.

> [!NOTE]
> Neue Spalten werden hinzugefügt, nachdem alle vorhandenen Spalten in der Tabelle geändert wurden.

> [!IMPORTANT]
> Wenn keine ALTER TABLE-Anweisung verwendet wird, werden die Anweisungen [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) und [PAD_INDEX](alter-table-index-option-transact-sql.md) nicht für Indizes auf speicheroptimierten Tabellen unterstützt.

PERIOD FOR SYSTEM_TIME ( system_start_time_column_name, system_end_time_column_name )  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gibt die Namen der Spalten an, die das System zum Aufzeichnen des Zeitraums verwendet, für den ein Datensatz gültig ist. Sie können vorhandene Spalten angeben oder neue Spalten im Rahmen des ADD PERIOD FOR SYSTEM_TIME-Arguments erstellen. Richten Sie Spalten mit dem Datentyp „datetime2“ ein, und definieren Sie sie als NOT NULL. Wenn Sie eine Zeitraumspalte als NULL definieren, tritt ein Fehler auf. Sie können für die Spalten system_start_time und system_end_time [column_constraint](../../t-sql/statements/alter-table-column-constraint-transact-sql.md) definieren und/oder [Standardwerte für Spalten angeben](../../relational-databases/tables/specify-default-values-for-columns.md). Sehen Sie sich Beispiel A in den folgenden [Beispielen zur Systemversionsverwaltung](#system_versioning) an, in dem veranschaulicht wird, wie Sie Standardwerte für system_end_time-Spalten einsetzen können.

Verwenden Sie dieses Argument mit dem Argument SET SYSTEM_VERSIONING, um die Systemversionsverwaltung für eine vorhandene Tabelle zu ermöglichen. Weitere Informationen finden Sie im Artikel zu [temporalen Tabelle](../../relational-databases/tables/temporal-tables.md) und unter [Getting Started with Temporal Tables in Azure SQL Database (Erste Schritte mit temporalen Tabellen in Azure SQL-Datenbank)](/azure/azure-sql/temporal-tables).

Ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] können Benutzer eine oder beide Zeitraumspalten mit dem Flag **HIDDEN** kennzeichnen, um diese Spalten implizit auszublenden, sodass **SELECT \* FROM\<table_name>** für diese Spalten keinen Wert zurückgibt. Standardmäßig sind Zeitraumspalten nicht ausgeblendet. Damit sie verwendet werden können, müssen ausgeblendete Spalten explizit in allen Abfragen eingeschlossen werden, die direkt auf die temporale Tabelle verweisen.

DROP  
Gibt an, dass eine oder mehrere Spaltendefinitionen, Definitionen berechneter Spalten oder Tabelleneinschränkungen oder die Spezifikation der Spalten, die das System zur Systemversionsverwaltung verwendet, gelöscht werden.

CONSTRAINT *constraint_name*  
Gibt an, dass *constraint_name* aus der Tabelle entfernt wird. Es können mehrere Einschränkungen aufgeführt werden.

Der benutzerdefinierte oder vom System bereitgestellte Name der Einschränkung kann durch Abfragen der Katalogsichten **sys.check_constraint**, **sys.default_constraints**, **sys.key_constraints** und **sys.foreign_keys** ermittelt werden.

Eine PRIMARY KEY-Einschränkung kann nicht gelöscht werden, wenn ein XML-Index für die Tabelle vorhanden ist.

INDEX *index_name*  
Gibt an, dass *index_name* aus der Tabelle entfernt wird.

Die Syntax ALTER TABLE... ADD/DROP/ALTER INDEX wird nur für speicheroptimierte Tabelle unterstützt.

> [!IMPORTANT]
> Wenn keine ALTER TABLE-Anweisung verwendet wird, werden die Anweisungen [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) und [PAD_INDEX](alter-table-index-option-transact-sql.md) nicht für Indizes auf speicheroptimierten Tabellen unterstützt.

COLUMN *column_name*  
Gibt an, dass *contraint_name* oder *column_name* aus der Tabelle gelöscht wird. Es können mehrere Spalten aufgeführt werden.

 Unter folgenden Umständen kann eine Spalte nicht gelöscht werden:

- Wenn sie in einem Index als Schlüsselspalte oder als INCLUDE-Element verwendet wird.
- Wenn sie in einer CHECK-, FOREIGN KEY-, UNIQUE- oder PRIMARY KEY-Einschränkung verwendet wird.
- Wenn ihr ein mit dem DEFAULT-Schlüsselwort definierter Standardwert zugeordnet ist oder wenn sie an ein Standardobjekt gebunden ist.
- Wenn sie an eine Regel gebunden ist.

> [!NOTE]
> Durch Löschen einer Spalte wird der Speicherplatz der Spalte nicht freigegeben. Unter Umständen müssen Sie den Speicherplatz einer gelöschten Spalte freigeben, wenn das Limit der Zeilengröße einer Tabelle fast erreicht oder überschritten ist. Zum Freigeben des Speicherplatzes erstellen Sie einen gruppierten Index für die Tabelle oder erstellen einen vorhandenen gruppierten Index mithilfe von [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) neu. Weitere Informationen zu den Auswirkungen gelöschter LOB-Datentypen finden Sie in [diesem CSS-Blogbeitrag](/archive/blogs/psssql/how-it-works-gotcha-varcharmax-caused-my-queries-to-be-slower).

PERIOD FOR SYSTEM_TIME  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Löscht die Spezifikation der Spalten, die das System zur Systemversionierung verwendet.

WITH \<drop_clustered_constraint_option>  
Gibt an, dass mindestens eine Option zum Löschen einer gruppierten Einschränkung festgelegt wurde.

MAXDOP = *max_degree_of_parallelism*  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Überschreibt die Konfigurationsoption **Max. Grad an Parallelität** nur für die Dauer des Vorgangs. Weitere Informationen finden Sie unter [Konfigurieren der Serverkonfigurationsoption Max. Grad an Parallelität](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

Verwenden Sie die MAXDOP-Option, um die Anzahl der Prozessoren zu beschränken, die für die Ausführung paralleler Pläne verwendet werden. Maximal sind 64 Prozessoren zulässig.

*max_degree_of_parallelism* kann einer der folgenden Werte sein:

1  
Unterdrückt das Generieren paralleler Pläne.

\>1  
Begrenzt die Höchstzahl von Prozessoren in einem parallelen Indexvorgang auf die angegebene Zahl

0 (Standard)  
Verwendet abhängig von der aktuellen Systemarbeitsauslastung die tatsächliche Anzahl von Prozessoren oder weniger Prozessoren.

Weitere Informationen finden Sie unter [Konfigurieren von Parallelindexvorgängen](../../relational-databases/indexes/configure-parallel-index-operations.md).

> [!NOTE]
> Parallele Indexvorgänge sind nicht in jeder Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verfügbar. Weitere Informationen finden Sie unter [Editionen und unterstützte Features von SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) und unter [Editionen und unterstützte Features von SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

ONLINE **=** { ON | **OFF** } \<as applies to drop_clustered_constraint_option>  
Gibt an, ob die zugrunde liegenden Tabellen und zugeordneten Indizes für Abfragen und Datenänderungen während des Indexvorgangs verfügbar sind. Der Standardwert ist OFF. Sie können REBUILD als ONLINE-Vorgang ausführen.

EIN  
Lang andauernde Tabellensperren werden nicht für die Dauer des Indexvorgangs aufrechterhalten. Während der Hauptphase des Indexvorgangs wird nur eine beabsichtigte freigegebene Sperre für die Quelltabelle aufrechterhalten. Durch dieses Verhalten können Abfragen oder Updates der zugrunde liegenden Tabelle und Indizes fortgesetzt werden. Zu Beginn des Vorgangs wird das Quellobjekt für kurze Zeit mit einer gemeinsamen Sperre (Shared, S) belegt. Am Ende des Vorgangs wird für kurze Zeit eine gemeinsame Sperre (S) für die Quelle aktiviert, wenn ein nicht gruppierter Index erstellt wird. Alternativ dazu wird eine Schemaänderungssperre (Schema Modification, SCH-M) aktiviert, wenn ein gruppierter Index online erstellt oder gelöscht wird und wenn ein gruppierter oder nicht gruppierter Index neu erstellt wird. ONLINE kann nicht auf ON festgelegt werden, wenn ein Index für eine lokale temporäre Tabelle erstellt wird. Nur ein Heap-Neuerstellungsvorgang mit einem einzelnen Thread ist zulässig.

Um die DDL für **SWITCH** oder für eine Onlineneuerstellung eines Indexes auszuführen, müssen alle aktiven blockierenden Transaktionen, die in einer bestimmten Tabelle ausgeführt werden, abgeschlossen sein. Bei der Ausführung verhindert **SWITCH** oder der Neuerstellungsvorgang das Starten neuer Transaktionen. Dies kann sich auf den Arbeitsauslastungsdurchsatz auswirken und den Zugriff auf die zugrunde liegende Tabelle vorübergehend deutlich einschränken.

OFF  
Die Tabellensperren werden für die Dauer des Indexvorgangs angewendet. Ein Offlineindexvorgang, bei dem ein gruppierter Index erstellt, neu erstellt oder gelöscht bzw. ein nicht gruppierter Index neu erstellt oder gelöscht wird, aktiviert eine Schemaänderungssperre (SCH-M) für die Tabelle. Durch diese Sperre wird verhindert, dass Benutzer für die Dauer des Vorgangs auf die zugrunde liegende Tabelle zugreifen können. Ein Offlineindexvorgang, bei dem ein nicht gruppierter Index erstellt wird, aktiviert eine freigegebene Sperre (S) für die Tabelle. Durch diese Sperre werden Updates der zugrunde liegenden Tabelle verhindert. Lesevorgänge wie SELECT-Anweisungen sind jedoch zulässig. Heap-Neuerstellungsvorgänge mit mehreren Threads sind zulässig.

Weitere Informationen finden Sie unter [Funktionsweise von Onlineindexvorgängen](../../relational-databases/indexes/how-online-index-operations-work.md).

> [!NOTE]
> Onlineindexvorgänge sind nicht in jeder Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verfügbar. Weitere Informationen finden Sie unter [Editionen und unterstützte Features von SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) und unter [Editionen und unterstützte Features von SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

MOVE TO { _Partition\_schema\_name_ **(** _Spalaten\_name_ [ ,...*n* ] **)**  | *Dateigruppe* |  **"** Standard **"** }  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gibt einen Speicherort an, an den die Datenzeilen verschoben werden sollen, die sich aktuell auf der Blattebene des gruppierten Indexes befinden. Die Tabelle wird an den neuen Speicherort verschoben. Diese Option gilt nur für Einschränkungen, durch die ein gruppierter Index erstellt wird.

> [!NOTE]
> In diesem Zusammenhang ist „default“ kein Schlüsselwort. Es ist ein Bezeichner für die Standarddateigruppe und muss begrenzt werden, wie in MOVE TO **"** default **"** oder MOVE TO **[** default **]** . Wenn **"** default **"** angegeben ist, muss die QUOTED_IDENTIFIER-Option in der aktuellen Sitzung auf ON festgelegt sein. Dies ist die Standardeinstellung. Weitere Informationen finden Sie unter [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

{ CHECK | NOCHECK } CONSTRAINT  
Gibt an, dass *contraint_name* aktiviert oder deaktiviert wird. Diese Option kann nur mit FOREIGN KEY- und CHECK-Einschränkungen verwendet werden. Wenn NOCHECK angegeben wird, wird die Einschränkung deaktiviert, und zukünftige Einfügungen oder Updates der Spalte werden nicht anhand der Einschränkungsbedingungen überprüft. DEFAULT-, PRIMARY KEY- und UNIQUE-Einschränkungen können nicht deaktiviert werden.

ALL  
Gibt an, dass alle Einschränkungen entweder mit der NOCHECK-Option deaktiviert oder mit der CHECK-Option aktiviert werden.

{ ENABLE | DISABLE } TRIGGER  
Gibt an, dass *trigger_name* aktiviert oder deaktiviert wird. Wenn ein Trigger deaktiviert wird, bleibt er dennoch weiterhin für die Tabelle definiert. Bei der Ausführung von INSERT-, UPDATE- oder DELETE-Anweisungen für die Tabelle werden die Aktionen im Trigger jedoch erst ausgeführt, wenn der Trigger wieder aktiviert wird.

ALL  
Gibt an, dass alle Trigger in der Tabelle aktiviert oder deaktiviert werden.

*trigger_name*  
Gibt den Namen des Triggers an, der deaktiviert oder aktiviert werden soll.

{ ENABLE | DISABLE } CHANGE_TRACKING  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gibt an, ob die Änderungsnachverfolgung für die Tabelle deaktiviert bzw. aktiviert wurde. Standardmäßig ist die Änderungsnachverfolgung deaktiviert.

Diese Option ist nur dann verfügbar, wenn die Änderungsnachverfolgung für die Datenbank aktiviert ist. Weitere Informationen finden Sie unter [ALTER DATABASE SET-Optionen](../../t-sql/statements/alter-database-transact-sql-set-options.md).

Um die Änderungsnachverfolgung zu aktivieren, muss die Tabelle über einen Primärschlüssel verfügen.

WITH **(** TRACK_COLUMNS_UPDATED **=** { ON | **OFF** } **)**  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gibt an, ob die [!INCLUDE[ssDE](../../includes/ssde-md.md)] verfolgt, welche Spalten mit Änderungsnachverfolgung aktualisiert wurden. Der Standardwert ist OFF.

SWITCH [ PARTITION *source_partition_number_expression* ] TO [ _schema\_name_ **.** ] *target_table* [ PARTITION *target_partition_number_expression* ]  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Verlagert einen Datenblock auf eine der folgenden Arten:

- Weist alle Daten einer Tabelle als Partition einer bereits vorhandenen partitionierten Tabelle neu zu.
- Wechselt eine Partition von einer partitionierten Tabelle zu einer anderen.
- Weist alle Daten aus einer Partition einer partitionierten Tabelle einer bereits vorhandenen nicht partitionierten Tabelle neu zu.

Wenn *table* eine partitionierte Tabelle ist, muss *source_partition_number_expression* angegeben werden. Wenn *target_table* partitioniert ist, muss *target_partition_number_expression* angegeben werden. Wenn die Daten einer Tabelle als Partition einer vorhandenen partitionierten Tabelle neu zugewiesen werden oder eine Partition von einer partitionierten Tabelle zu einer anderen gewechselt wird, muss die Zielpartition vorhanden und leer sein.

Wenn die Daten einer Partition neu zugewiesen werden, sodass sie eine einzelne Tabelle bilden, muss die Zieltabelle bereits vorhanden und leer sein. Die Quelltabelle oder -partition und die Zieltabelle oder -partition müssen sich in derselben Dateigruppe befinden. Die entsprechenden Indizes oder Indexpartitionen müssen sich ebenfalls in derselben Dateigruppe befinden. Darüber hinaus gelten weitere Einschränkungen für das Wechseln von Partitionen. *table* und *target_table* dürfen nicht gleich sein. *target_table* kann ein mehrteiliger Bezeichner sein.

*source_partition_number_expression* und *target_partition_number_expression* sind konstante Ausdrücke, die auf Variablen und Funktionen verweisen können. Diese enthalten benutzerdefinierte Typvariablen und benutzerdefinierte Funktionen. Sie können nicht auf [!INCLUDE[tsql](../../includes/tsql-md.md)]-Ausdrücke verweisen.

Eine partitionierte Tabelle mit einem gruppierten Columnstore-Index verhält sich wie ein partitionierter Heap:

- Der Primärschlüssel muss den Partitionsschlüssel beinhalten.
- Der eindeutige Index muss den Partitionsschlüssel beinhalten. Durch das Einbeziehen des Partitionsschlüssels in einen eindeutigen Bezeichner kann sich jedoch die Eindeutigkeit verändern.
- Alle nicht gruppierten Indizes müssen den Partitionsschlüssel enthalten, damit Partitionen gewechselt werden können.

Informationen zu **SWITCH**-Einschränkungen beim Verwenden von Replikaten finden Sie unter [Replizieren partitionierter Tabellen und Indizes](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).

Nicht gruppierte Columnstore-Indizes für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016 CTP1 und für Versionen von SQL-Datenbank vor V12 waren schreibgeschützt. Nicht gruppierte Columnstore-Indizes müssen im aktuellen Format erneut erstellt werden (das aktualisiert werden kann), bevor PARTITION-Vorgänge ausgeführt werden können.

SET **(** FILESTREAM_ON = { *partition_scheme_name* \| *filestream_filegroup_name* \| **"** default **"** \| **"** NULL **"** } **)**  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher). `FILESTREAM` wird von [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] nicht unterstützt.

Gibt an, wo FILESTREAM-Daten gespeichert werden.

ALTER TABLE mit der SET FILESTREAM_ON-Klausel ist nur erfolgreich, wenn die Tabelle keine FILESTREAM-Spalten enthält. Die FILESTREAM-Spalten können mit einer zweiten ALTER TABLE-Anweisung hinzugefügt werden.

Wenn *partition_scheme_name* angegeben wurde, gelten die Regeln für [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md). Stellen Sie sicher, dass die Tabelle bereits für Zeilendaten partitioniert ist und dass das Partitionsschema die gleiche Partitionsfunktion und die gleichen Partitionsspalten wie das FILESTREAM-Partitionsschema verwendet.

*filestream_filegroup_name* gibt den Namen einer FILESTREAM-Dateigruppe an. Für die Dateigruppe muss eine Datei mit einer [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)-Anweisung oder einer [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)-Anweisung definiert worden sein. Andernfalls tritt ein Fehler auf.

**"** default **"** gibt die FILESTREAM-Dateigruppe mit dem DEFAULT-Eigenschaftensatz an. Wenn keine FILESTREAM-Dateigruppe vorhanden ist, tritt ein Fehler auf.

**"** NULL **"** gibt an, dass alle Verweise auf FILESTREAM-Dateigruppen für die Tabelle entfernt werden. Alle FILESTREAM-Spalten müssen zuerst gelöscht werden. Verwenden Sie SET FILESTREAM_ON = "**NULL**", um alle FILESTREAM-Daten zu löschen, die mit einer Tabelle verknüpft sind.

SET **(** SYSTEM_VERSIONING **=** { OFF | ON [ ( HISTORY_TABLE = schema_name . history_table_name [ , DATA_CONSISTENCY_CHECK = { **ON** | OFF } ] ) ] } **)**  
 **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Deaktiviert oder aktiviert die Systemversionsverwaltung einer Tabelle. Das System überprüft, ob die Einschränkungsanfoderungen des Datentyps, der NULL-Zulässigkeit und des Primärschlüssels für die Systemversionierung eingehalten wurden, um die Systemversionierung einer Tabelle zu ermöglichen. Wenn das Argument HISTORY_TABLE nicht verwendet wird, generiert das System eine neue Verlaufstabelle, die dem Schema der aktuellen Tabelle entspricht, erstellt eine Verknüpfung zwischen den beiden Tabellen und ermöglicht dem System, den Verlauf von jedem Datensatz der aktuellen Tabelle in der Verlaufstabelle aufzuzeichnen. Der Name dieser Verlaufstabelle ist dann `MSSQL_TemporalHistoryFor<primary_table_object_id>`. Wenn Sie das Argument HISTORY_TABLE verwenden, um eine Verknüpfung mit einer vorhandenen Verlaufstabelle zu erstellen und diese zu verwenden, erstellt das System eine Verknüpfung zwischen der aktuellen Tabelle und der angegebenen Tabelle. Wenn Sie eine Verknüpfung mit einer vorhandenen Verlaufstabelle erstellen, können Sie eine Datenkonsistenzprüfung durchführen. Diese Datenkonsistenzprüfung stellt sicher, dass vorhandene Datensätze nicht überlappen. Die Datenkonsistenzprüfung ist standardmäßig aktiviert. Weitere Informationen finden Sie unter [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

HISTORY_RETENTION_PERIOD = { **INFINITE** \| number {DAY \| DAYS \| WEEK \| WEEKS \| MONTH \| MONTHS \| YEAR \| YEARS} }  
**Gilt für**: [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gibt die endliche oder unendliche Aufbewahrungsdauer für Verlaufsdaten in temporalen Tabellen an. Wenn sie weggelassen wird, wird von einer unendlichen Vermerkdauer ausgegangen.

SET (DATA_DELETION =  
      { OFF | ON  
                [(  [ FILTER_COLUMN = Spaltenname ]   
                    [, RETENTION_PERIOD = { INFINITE | Anzahl {DAY | DAYS | WEEK | WEEKS | MONTH | MONTHS | YEAR | YEARS }}]   
                )] }   
**Anwendungsbereich:** *Nur* Azure SQL Edge

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

SET **(** LOCK_ESCALATION = { AUTO \| TABLE \| DISABLE } **)**  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gibt die zulässigen Methoden der Sperrenausweitung für eine Tabelle an.

AUTO  
Mit dieser Option kann die [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] die für das Tabellenschema geeignete Granularität der Sperrenausweitung auswählen.

- Wenn die Tabelle partitioniert ist, ist die Sperrenausweitung bis zur Heap- oder B-Struktur (HoBT)-Granularität zulässig. Dies bedeutet, dass eine Ausweitung auf Partitionsebene zulässig ist. Nach der Ausweitung der Sperre auf die HoBT-Ebene wird die Sperrenausweitung nicht bis zur TABLE-Granularität fortgeführt.
- Wenn die Tabelle nicht partitioniert ist, wird die Sperre bis zur TABLE-Granularität ausgeweitet.

TABLE  
Die Sperrenausweitung wird immer mit der Granularität der Tabellenebene ausgeführt, unabhängig davon, ob die Tabelle partitioniert ist. TABLE ist der Standardwert.

DISABLE  
Verhindert die Sperrenausweitung in den meisten Fällen. Sperren auf Tabellenebene sind jedoch nicht völlig ausgeschlossen. Wenn Sie z.B. eine Tabelle scannen, die keinen gruppierten Index unter der Serializable-Isolationsstufe aufweist, muss die [!INCLUDE[ssDE](../../includes/ssde-md.md)] eine Tabellensperre zulassen, damit die Datenintegrität gewahrt wird.

REBUILD  
Verwenden Sie die REBUILD WITH-Syntax, um eine gesamte Tabelle neu zu erstellen, einschließlich aller Partitionen in einer partitionierten Tabelle. Wenn die Tabelle einen gruppierten Index besitzt, erstellt die REBUILD-Option den gruppierten Index neu. REBUILD kann als ONLINE-Vorgang ausgeführt werden.

Verwenden Sie die REBUILD PARTITION-Syntax, um eine einzelne Partition in einer partitionierten Tabelle neu zu erstellen.

PARTITION = ALL  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Erstellt alle Partitionen neu, wenn die Komprimierungseinstellungen für die Partition geändert werden.

REBUILD WITH ( \<rebuild_option> )  
Alle Optionen gelten für eine Tabelle mit einem gruppierten Index. Wenn die Tabelle nicht über einen gruppierten Index verfügt, wird die Heapstruktur nur von einigen der Optionen beeinflusst.

Wenn mit dem REBUILD-Vorgang keine bestimmte Komprimierungseinstellung angegeben wird, wird die aktuelle Komprimierungseinstellung für die Partition verwendet. Um die aktuelle Einstellung zurückzugeben, fragen Sie die **data_compression**-Spalte in der **sys.partitions**-Katalogsicht ab.

Die vollständigen Beschreibungen dieser Optionen zum Neuerstellen finden Sie unter [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md).

DATA_COMPRESSION  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gibt die Datenkomprimierungsoption für die angegebene Tabelle, die Partitionsnummer oder den Bereich von Partitionen an. Die folgenden Optionen sind verfügbar:

NONE Die Tabelle oder die angegebenen Partitionen werden nicht komprimiert. Diese Option gilt nicht für Columnstore-Tabellen.

ROW Die Tabelle oder die angegebenen Partitionen werden mit Zeilenkomprimierung komprimiert. Diese Option gilt nicht für Columnstore-Tabellen.

PAGE Die Tabelle oder die angegebenen Partitionen werden mit Seitenkomprimierung komprimiert. Diese Option gilt nicht für Columnstore-Tabellen.

COLUMNSTORE  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gilt nur für columnstore-Tabellen. COLUMNSTORE gibt an, dass eine Partition, die mit der COLUMNSTORE_ARCHIVE-Option komprimiert wurde, dekomprimiert werden soll. Nachdem die Daten wiederhergestellt wurden, sind sie weiterhin mit der Columnstore-Komprimierung komprimiert, die für alle Columnstore-Tabellen verwendet wird.

COLUMNSTORE_ARCHIVE  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Gilt nur für columnstore-Tabellen. Dies sind Tabellen, die mit einem gruppierten columnstore-Index gespeichert wurden. Durch COLUMNSTORE_ARCHIVE wird die angegebene Partition weiter in eine geringere Größe komprimiert. Verwenden Sie diese Option bei der Archivierung und in anderen Situationen, in denen es auf eine geringere Speicherbelegung und nicht auf den zusätzlichen Zeitaufwand für das Speichern und Abrufen ankommt.

Informationen zur gleichzeitigen Neuerstellung mehrerer Partitionen finden Sie unter [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md). Wenn die Tabelle nicht über einen gruppierten Index verfügt, werden bei Änderungen an der Datenkomprimierung der Heap und die nicht gruppierten Indizes neu erstellt. Weitere Informationen zur Datenkomprimierung finden Sie unter [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md).

ONLINE **=** { ON | **OFF** } \<as applies to single_partition_rebuild_option>  
Gibt an, ob eine einzelne Partition der zugrunde liegenden Tabellen und der zugeordneten Indizes für Abfragen und Datenänderungen während des Indexvorgangs verfügbar ist. Der Standardwert ist OFF. Sie können REBUILD als ONLINE-Vorgang ausführen.

EIN  
Lang andauernde Tabellensperren werden nicht für die Dauer des Indexvorgangs aufrechterhalten. Erfordert eine S-Sperre für die Tabelle am Anfang der Indexneuerstellung und eine Sch-M-Sperre für die Tabelle am Ende der Onlineneuerstellung des Indexes. Obwohl beide Sperren kurze Metadatensperren sind, muss die Sch-M-Sperre auf den Abschluss aller blockierenden Transaktionen warten. Während der Wartezeit sperrt die Sch-M-Sperre alle anderen Transaktionen, die an dieser Sperre warten, wenn sie auf die gleiche Tabelle zugreifen.

> [!NOTE]
> Durch Neuerstellung von Onlineindizes können die *low_priority_lock_wait*-Optionen festgelegt werden, die weiter unten in diesem Abschnitt beschrieben werden.

OFF  
Die Tabellensperren werden für die Dauer des Indexvorgangs angewendet. Dadurch wird verhindert, dass Benutzer für die Dauer des Vorgangs auf die zugrunde liegende Tabelle zugreifen können.

*column_set_name* XML COLUMN_SET FOR ALL_SPARSE_COLUMNS  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Der Name des Spaltensatzes. Bei einem Spaltensatz handelt es sich um eine nicht typisierte XML-Darstellung, die alle Sparsespalten einer Tabelle in einer strukturierten Ausgabe kombiniert. Sie können einer Tabelle, die Sparsespalten enthält, keinen Spaltensatz hinzufügen. Weitere Informationen zu Spaltensätzen finden Sie unter [Verwenden von Spaltensätzen](../../relational-databases/tables/use-column-sets.md).

{ ENABLE | DISABLE } FILETABLE_NAMESPACE  
 **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher).

Aktiviert oder deaktiviert die systemdefinierten Einschränkungen für eine FileTable. Kann nur mit einer FileTable verwendet werden.

SET ( FILETABLE_DIRECTORY = *directory_name* )  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher). `FILETABLE` wird von [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] nicht unterstützt.

Gibt den Windows-kompatiblen FileTable-Verzeichnisnamen an. Dieser Name sollte für alle FileTable-Verzeichnisnamen in der Datenbank eindeutig sein. Bei Eindeutigkeitsvergleichen wird die Groß-/Kleinschreibung unabhängig von den SQL-Sortiereinstellungen nicht beachtet. Kann nur mit einer FileTable verwendet werden.

```syntaxsql
 SET (
        REMOTE_DATA_ARCHIVE
        {
            = ON (<table_stretch_options> )
          | = OFF_WITHOUT_DATA_RECOVERY
          ( MIGRATION_STATE = PAUSED ) | ( <table_stretch_options> [, ...n] )
        } )
```

**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] und höher).

Aktiviert bzw. deaktiviert Stretch Database für die Datenbank. Weitere Informationen finden Sie unter [Stretch Database](../../sql-server/stretch-database/stretch-database.md).

**Aktivieren von Stretch Database für eine Tabelle**

Wenn Sie durch Angeben von `ON` Stretch für eine Tabelle aktivieren, können Sie optional `MIGRATION_STATE = OUTBOUND` festlegen, um sofort mit dem Migrieren von Daten zu beginnen, oder Sie können `MIGRATION_STATE = PAUSED` festlegen, um die Datenmigration zu verzögern. Standardwert: `MIGRATION_STATE = OUTBOUND`. Weitere Informationen über das Aktivieren von Stretch für eine Tabelle finden Sie unter [Aktivieren von Stretch Database für eine Tabelle](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).

**Voraussetzungen** Sie müssen Stretch auf dem Server und auf der Datenbank aktivieren, bevor Sie Stretch für eine Tabelle aktivieren können. Weitere Informationen finden Sie unter [Aktivieren von Stretch Database für eine Datenbank](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).

**Berechtigungen** Zum Aktivieren von Stretch für eine Datenbank oder eine Tabelle benötigen Sie die „db_owner“-Berechtigungen. Zum Aktivieren von Stretch für eine Tabelle benötigen Sie auch die ALTER-Berechtigungen für die Tabelle.

**Deaktivieren von Stretch Database für eine Tabelle**

Wenn Sie Stretch für eine Tabelle deaktivieren, haben Sie zwei Möglichkeiten für die Remotedaten, die bereits zu Azure migriert wurden. Weitere Informationen finden Sie unter [Deaktivieren von Stretch Database und Zurückholen von Remotedaten](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

- Führen Sie den folgenden Befehl aus, um Stretch für eine Tabelle zu deaktivieren und die Remotedaten einer Tabelle aus Azure zurück zu SQL Server zu kopieren. Dieser Befehl kann nicht abgebrochen werden.

    ```sql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    ```

Dieser Vorgang verursacht Datenübertragungskosten und kann nicht abgebrochen werden. Weitere Informationen finden Sie unter [Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/).

Nachdem die Remotedaten von Azure zurück zu SQL Server kopiert wurden, wird Stretch für die Tabelle deaktiviert.

- Führen Sie den folgenden Befehl aus, um Stretch für eine Tabelle zu deaktivieren und die Remotedaten zu verwerfen.

    ```sql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

Nach dem Deaktivieren von Stretch Database für eine Tabelle wird die Datenmigration beendet und die Abfrageergebnisse enthalten nicht mehr die Ergebnisse aus der Remotetabelle.

Auch wenn Stretch deaktiviert wird, wird die Remotetabelle nicht entfernt. Wenn Sie die Remotetabelle löschen möchten, muss dieser Vorgang über das Azure-Portal erfolgen.

[ FILTER_PREDICATE = { null | *predicate* } ]  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] und höher).

Gibt optional ein Filterprädikat zum Auswählen der Zeilen an, die aus einer Tabelle migriert werden sollen, die sowohl Verlaufsdaten als auch aktuelle Daten enthält. Das Prädikat muss eine deterministische Inline-Tabellenwertfunktion aufrufen. Weitere Informationen finden Sie unter [Aktivieren von Stretch Database für eine Tabelle](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) und [Auswählen zu migrierender Zeilen mithilfe einer Filterfunktion – Stretch Database](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).

> [!IMPORTANT]
> Wenn Sie ein schwaches Filterprädikat angeben, wird die Datenmigration ebenfalls unzureichend ausgeführt. Stretch-Datenbank wendet das Filterprädikat über den CROSS APPLY-Operator auf die Tabelle an.

Wenn Sie kein Filterprädikat angeben, wird die gesamte Tabelle migriert.

Wenn Sie ein Filterprädikat angeben, müssen Sie auch *MIGRATION_STATE* angeben.

MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] und höher).

- Geben Sie `OUTBOUND` an, um Daten von SQL Server zu Azure zu migrieren.
- Geben Sie `INBOUND` an, um Remotedaten für die Tabelle aus Azure zurück zu SQL Server zu migrieren und Stretch für die Tabelle zu deaktivieren. Weitere Informationen finden Sie unter [Deaktivieren von Stretch Database und Zurückholen von Remotedaten](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

    Dieser Vorgang verursacht Datenübertragungskosten und kann nicht abgebrochen werden.

- Geben Sie `PAUSED` an, um die Datenmigration zu pausieren oder nach hinten zu verschieben. Weitere Informationen finden Sie unter [Anhalten und Fortsetzen der Datenmigration – Stretch Database](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).

WAIT_AT_LOW_PRIORITY  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Bei der Onlineindexneuerstellung muss auf blockierende Vorgänge für diese Tabelle gewartet werden. **WAIT_AT_LOW_PRIORITY** gibt an, dass der Onlineneuerstellungsvorgang für den Index auf Sperren mit niedriger Priorität wartet und die weitere Ausführung anderer Vorgänge ermöglicht, während der Onlineerstellungsvorgang für den Index wartet. Die Option **WAIT AT LOW PRIORITY** auszulassen, entspricht `WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE)`.

MAX_DURATION = *time* [**MINUTES** ]  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Die Wartezeit (ein ganzzahliger Wert in Minuten), für die **SWITCH** oder die Sperren der Onlineindexneuerstellung mit niedriger Priorität warten, wenn der DDL-Befehl ausgeführt wird. Wenn der Vorgang während des **MAX_DURATION**-Zeitraums blockiert wird, wird eine der **ABORT_AFTER_WAIT**-Aktionen ausgeführt. **MAX_DURATION** wird immer in Minuten angegeben, und das Wort **MINUTES** kann ausgelassen werden.

ABORT_AFTER_WAIT = [**NONE** | **SELF** | **BLOCKERS** } ]  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Keine  
Es wird weiterhin mit normaler (regulärer) Priorität auf die Sperre gewartet.

SELF  
**SWITCH** oder der aktuell ausgeführte DDL-Vorgang zur Neuerstellung des Onlineindexes wird beendet, ohne Aktionen durchzuführen.

BLOCKERS  
Alle Benutzertransaktionen, die derzeit **SWITCH** oder den DDL-Vorgang zur Neuerstellung des Onlineindexes blockieren, werden beendet, sodass der Vorgang fortgesetzt werden kann.

Erfordert die **ALTER ANY CONNECTION**-Berechtigung.

IF EXISTS  
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Löscht die Spalte oder Einschränkung nur, wenn diese bereits vorhanden ist.

## <a name="remarks"></a>Bemerkungen

Verwenden Sie [INSERT](../../t-sql/statements/insert-transact-sql.md), um neue Datenzeilen hinzuzufügen. Verwenden Sie [DELETE](../../t-sql/statements/delete-transact-sql.md) oder [TRUNCATE TABLE](../../t-sql/statements/truncate-table-transact-sql.md), um Datenzeilen zu entfernen. Verwenden Sie [UPDATE](../../t-sql/queries/update-transact-sql.md), um die Werte in vorhandenen Zeilen zu ändern.

Wenn der Prozedurcache Ausführungspläne enthält, die auf die Tabelle verweisen, kennzeichnet ALTER TABLE diese für die erneute Kompilierung bei der nächsten Ausführung.

## <a name="changing-the-size-of-a-column"></a>Ändern der Größe einer Spalte

Sie können Länge, Genauigkeit oder Dezimalstellen einer Spalte ändern, indem Sie die neue Größe für den Spaltendatentyp angeben. Verwenden Sie die ALTER COLUMN-Klausel. Wenn die Spalte Daten enthält, darf die neue Größe nicht unter der maximalen Datenmenge liegen. Außerdem kann die Spalte nicht in einem Index definiert werden, es sei denn, die Spalte ist vom Datentyp **varchar**, **nvarchar** oder **varbinary** und der Index ist nicht das Ergebnis einer PRIMARY KEY-Einschränkung. Sehen Sie sich hierzu das Beispiel in dem kurzen Abschnitt mit dem Titel [Ändern einer Spaltendefinition](#alter_column) an.

## <a name="locks-and-alter-table"></a>Sperren und ALTER TABLE

Änderungen, die Sie in ALTER TABLE angeben, werden sofort implementiert. Wenn die Änderungen Änderungen der Zeilen in der Tabelle erfordern, aktualisiert ALTER TABLE die Zeilen. ALTER TABLE belegt die Tabelle mit einer Schemaänderungssperre (SCH-M), um sicherzustellen, dass andere Verbindungen während der Änderung noch nicht einmal auf die Metadaten der Tabelle verweisen können. Eine Ausnahme sind Onlineindexvorgänge, die am Ende eine kurze SCH-M-Sperre erfordern. Bei einem `ALTER TABLE...SWITCH`-Vorgang werden sowohl die Quell- als auch die Zieltabelle mit der Sperre belegt. Die an der Tabelle vorgenommenen Änderungen werden protokolliert und sind vollständig wiederherstellbar. Änderungen, die sich auf sämtliche Zeilen einer großen Tabelle auswirken (z.B. das Löschen einer Spalte oder in einigen Editionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das Hinzufügen einer NOT NULL-Spalte mit einem Standardwert) kann viel Zeit in Anspruch nehmen und dazu führen, dass viele Protokolldatensätze generiert werden. Diese ALTER TABLE-Anweisungen sollten ebenso vorsichtig ausgeführt werden wie jede INSERT-, UPDATE- oder DELETE-Anweisung, die sich auf viele Zeilen auswirkt.

### <a name="adding-not-null-columns-as-an-online-operation"></a>Hinzufügen von NOT NULL-Spalten als Onlinevorgang

Ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Enterprise Edition ist das Hinzufügen einer NOT NULL-Spalte mit einem Standardwert ein Onlinevorgang, wenn der Standardwert eine *Laufzeitkonstante* darstellt. Dies bedeutet, dass der Vorgang trotz der Anzahl von Zeilen in der Tabelle nahezu sofort abgeschlossen wird. Dies liegt daran, dass die vorhandenen Zeilen in der Tabelle während des Vorgangs nicht aktualisiert werden. Stattdessen wird der Standardwert nur in den Metadaten der Tabelle gespeichert und der Wert in Abfragen, die auf diese Zeilen zugreifen, nur nach Bedarf gesucht. Dieses Verhalten erfolgt automatisch. Es ist keine zusätzliche Syntax erforderlich, um den Onlinevorgang außerhalb der ADD COLUMN-Syntax zu implementieren. Eine Laufzeitkonstante ist ein Ausdruck, der zur Laufzeit trotz Determinismus den gleichen Wert für jede Zeile in der Tabelle erzeugt. Der konstante Ausdruck "My temporary data" oder die GETUTCDATETIME()-Systemfunktion sind z. B. Laufzeitkonstanten. Im Gegensatz dazu sind die Funktionen `NEWID()` oder `NEWSEQUENTIALID()` keine Laufzeitkonstanten, da für jede Zeile in der Tabelle ein eindeutiger Wert erzeugt wird. Das Hinzufügen einer NOT NULL-Spalte mit einem Standardwert, der keine Laufzeitkonstante ist, wird immer offline ausgeführt, und dabei wird eine exklusive SCH-M-Sperre für die Dauer des Vorgangs aktiviert.

Während die vorhandenen Zeilen auf den in Metadaten gespeicherten Wert verweisen, wird der Standardwert für alle neu eingefügten Zeilen in der Zeile gespeichert, ohne einen anderen Wert für die Spalte anzugeben. Der in Metadaten gespeicherte Standardwert wird in eine vorhandene Zeile verschoben, wenn die Zeile aktualisiert wird (auch wenn die tatsächliche Spalte nicht in der UPDATE-Anweisung angegeben wird) oder wenn die Tabelle oder der gruppierte Index neu erstellt wird.

Spalten vom Typ **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **xml**, **text**, **ntext**, **image**, **hierarchyid**, **geometry**, **geography** oder CLR UDTS können nicht in einem Onlinevorgang hinzugefügt werden. Eine Spalte kann nicht online hinzugefügt werden, wenn dies dazu führt, dass die maximal mögliche Zeilengröße den Grenzwert von 8.060 Byte überschreitet. Die Spalte wird in diesem Fall als Offlinevorgang hinzugefügt.

## <a name="parallel-plan-execution"></a>Ausführung paralleler Pläne

In [!INCLUDE[ssEnterpriseEd11](../../includes/ssenterpriseed11-md.md)] und höher wird die Anzahl von Prozessoren, die zur Ausführung einer einzelnen ALTER TABLE ADD CONSTRAINT-Anweisung (indexbasiert) oder einer ALTER TABLE DROP CONSTRAINT-Anweisung (gruppierter Index) verwendet werden, durch die Konfigurationsoption **Max. Grad an Parallelität** und durch die aktuelle Arbeitsauslastung bestimmt. Wenn das [!INCLUDE[ssDE](../../includes/ssde-md.md)] erkennt, dass das System ausgelastet ist, wird der Grad an Parallelität für den Vorgang automatisch reduziert, bevor mit der Ausführung der Anweisung begonnen wird. Sie können die Anzahl der Prozessoren, die zur Ausführung der Anweisung verwendet werden, durch Angeben der MAXDOP-Option manuell konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren der Serverkonfigurationsoption Max. Grad an Parallelität](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

## <a name="partitioned-tables"></a>Partitionierte Tabellen

Neben dem Ausführen von SWITCH-Vorgängen mit partitionierten Tabellen können mit ALTER TABLE der Status der Spalten, Einschränkungen und Trigger einer partitionierten Tabelle genau wie bei nicht partitionierten Tabellen geändert werden. Die Partitionierung der Tabelle selbst kann jedoch mit der Anweisung nicht geändert werden. Verwenden Sie zum Neupartitionieren einer partitionierten Tabelle [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md) und [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md). Zudem können Sie den Datentyp einer Spalte in einer partitionierten Tabelle nicht ändern.

## <a name="restrictions-on-tables-with-schema-bound-views"></a>Einschränkungen für Tabellen mit schemagebundenen Sichten

Die Einschränkungen, die für ALTER TABLE-Anweisungen für Tabellen mit schemagebundenen Sichten gelten, sind dieselben, die derzeit beim Ändern von Tabellen mit einem einfachen Index angewendet werden. Das Hinzufügen einer Spalte ist zulässig. Das Entfernen oder Ändern einer Spalte, die Bestandteil einer schemagebundenen Sicht ist, ist dagegen nicht zulässig. Wenn für die ALTER TABLE-Anweisung das Ändern einer in einer schemagebundenen Sicht verwendeten Spalte erforderlich ist, schlägt ALTER TABLE fehl, und das [!INCLUDE[ssDE](../../includes/ssde-md.md)] löst eine Fehlermeldung aus. Weitere Informationen zu Schemabindung und indizierten Sichten finden Sie unter [CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md).

Das Hinzufügen oder Entfernen von Triggern für Basistabellen wird durch das Erstellen einer schemagebundenen Sicht, die auf die Tabellen verweist, nicht beeinflusst.

## <a name="indexes-and-alter-table"></a>Indizes und ALTER TABLE

Als Teil einer Einschränkung erstellte Indizes werden gelöscht, wenn die Einschränkung gelöscht wird. Mit CREATE INDEX erstellte Indizes müssen mit DROP INDEX gelöscht werden. Verwenden Sie die ALTER INDEX-Anweisung, um einen Index neu zu erstellen, der Teil einer Einschränkungsdefinition ist. Die Einschränkung muss nicht gelöscht und mit ALTER TABLE erneut hinzugefügt werden.

Alle auf einer Spalte basierenden Indizes und Einschränkungen müssen entfernt werden, bevor die Spalte entfernt werden kann.

Wenn Sie eine Einschränkung löschen, für die ein gruppierter Index erstellt wurde, werden die Datenzeilen, die auf der Blattebene des gruppierten Indexes gespeichert waren, in einer nicht gruppierten Tabelle gespeichert. Sie können den gruppierten Index löschen und die daraus resultierende Tabelle in einer einzigen Transaktion in eine andere Dateigruppe oder in ein anderes Partitionsschema verschieben, indem Sie die MOVE TO-Option angeben. Die MOVE TO-Option weist die folgenden Einschränkungen auf:

- MOVE TO ist für indizierte Sichten oder nicht gruppierte Indizes nicht gültig.
- Das Partitionsschema oder die Dateigruppe muss bereits vorhanden sein.
- Wird MOVE TO nicht angegeben, wird die Tabelle in demselben Partitionsschema oder derselben Dateigruppe platziert, das bzw. die für den gruppierten Index definiert war.

Beim Löschen eines gruppierten Indexes können Sie die ONLINE **=** ON-Option angeben, sodass die DROP INDEX-Transaktion keine Abfragen und Änderungen der zugrunde liegenden Daten und zugeordneten nicht gruppierten Indizes blockiert.

Für ONLINE **=** ON gelten folgende Einschränkungen:

- ONLINE **=** ON ist nicht gültig für gruppierte Indizes, die auch deaktiviert sind. Deaktivierte Indizes müssen mit ONLINE **=** OFF gelöscht werden.
- Es können nicht mehrere Indizes gleichzeitig gelöscht werden.
- ONLINE **=** ON ist nicht gültig für indizierte Sichten, nicht gruppierte Indizes oder Indizes für lokale temporäre Tabellen.
- ONLINE **=** ON ist für Columnstore-Indizes nicht gültig.

Zum Löschen eines gruppierten Indexes ist temporärer Speicherplatz im Umfang des vorhandenen gruppierten Indexes erforderlich. Dieser zusätzliche Speicherplatz wird nach Abschluss des Vorgangs freigegeben.

> [!NOTE]
> Die unter *\<drop_clustered_constraint_option>* aufgeführten Optionen gelten für gruppierte Indizes für Tabellen und können nicht auf gruppierte Indizes für Sichten oder nicht gruppierte Indizes angewendet werden.

## <a name="replicating-schema-changes"></a>Replizieren von Schemaänderungen

Wenn Sie ALTER TABLE für eine veröffentlichte Tabelle auf einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verleger ausführen, wird diese Änderung standardmäßig an alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abonnenten weitergegeben. Für diese Funktionalität bestehen einige Einschränkungen. Sie können sie deaktivieren. Weitere Informationen finden Sie unter [Vornehmen von Schemaänderungen in Veröffentlichungsdatenbanken](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).

## <a name="data-compression"></a>Datenkomprimierung

Für Systemtabellen ist die Komprimierung nicht verfügbar. Wenn die Tabelle ein Heap ist, erfolgt der Neuerstellungsvorgang für den ONLINE-Modus mit einem einzelnen Thread. Verwenden Sie den OFFLINE-Modus für einen Multithreaded-Neuerstellungsvorgang von Heaps. Weitere Informationen zur Datenkomprimierung finden Sie unter [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md).

Mit der gespeicherten Prozedur [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) können Sie einschätzen, wie sich eine Änderung des Komprimierungsstatus auf eine Tabelle, einen Index oder eine Partition auswirkt.

Für partitionierte Tabellen gelten die folgenden Einschränkungen:

- Sie können die Komprimierungseinstellung einer einzelnen Partition nicht ändern, wenn die Tabelle nicht ausgerichtete Indizes aufweist.
- Mit der Syntax ALTER TABLE \<table> REBUILD PARTITION... wird die angegebene Partition neu erstellt.
- Mit der Syntax ALTER TABLE \<table> REBUILD WITH... werden alle Partitionen neu erstellt.

## <a name="dropping-ntext-columns"></a>Löschen von NTEXT-Spalten

Wenn NTEXT-Spalten gelöscht werden, wird das Cleanup der gelöschten Daten als serialisierter Vorgang für alle Zeilen durchgeführt. Die Bereinigung kann lange Zeit in Anspruch nehmen. Wenn Sie eine NTEXT-Spalte in einer Tabelle mit einer großen Zeilenanzahl löschen, aktualisieren Sie die NTEXT-Spalte zunächst auf den Wert NULL, und löschen Sie dann die Spalte. Sie können diese Option mit parallelen Vorgängen und damit wesentlich schneller ausführen.

## <a name="online-index-rebuild"></a>Onlineneuerstellung von Indizes

Um die DDL-Anweisung für eine Onlineindexneuerstellung auszuführen, müssen alle aktiven blockierenden Transaktionen, die für eine bestimmte Tabelle ausgeführt werden, abgeschlossen sein. Wenn die Onlineindexneuerstellung gestartet wird, werden alle neuen Transaktionen blockiert, die zur Ausführung in dieser Tabelle bereit sind. Obwohl die Sperre für die Onlineindexneuerstellung nur kurz dauert, kann das Warten auf den Abschluss aller noch offenen Transaktionen und das Blockieren des Starts aller neuen Transaktionen für eine bestimmte Tabelle den Durchsatz beeinträchtigen. Dies kann zur Verlangsamung oder Ausfall einer Arbeitsauslastung führen und den Zugriff auf die zugrunde liegende Tabelle deutlich einschränken. Mit der **WAIT_AT_LOW_PRIORITY**-Option können Datenbankadministratoren die S-Sperre sowie Sch-M-Sperren, die für die Onlineneuerstellung von Indizes erforderlich sind, verwalten und eine von drei Optionen auswählen. In allen drei Fällen gilt: Sind während der Wartezeit (`(MAX_DURATION =n [minutes])`) keine blockierenden Aktivitäten vorhanden, wird die Onlineindexneuerstellung ohne Wartezeit sofort ausgeführt, und die DDL-Anweisung wird abgeschlossen.

## <a name="compatibility-support"></a>Kompatibilitätsunterstützung

Die ALTER TABLE-Anweisung unterstützt nur zweiteilige Tabellennamen (schema.object). In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] schlägt die Angabe eines Tabellennamens mit den folgenden Formaten zur Kompilierzeit mit Fehler 117 fehl.

- server.database.schema.table
- .database.schema.table
- ..schema.table

Bei früheren Versionen wurde durch die Angabe des Formats „server.database.schema.table“ der Fehler 4902 zurückgegeben. Die Angabe des Formats ".database.schema.table" oder "..schema.table" war erfolgreich.

Um das Problem zu beheben, vermeiden Sie die Verwendung eines vierteiligen Präfixes.

## <a name="permissions"></a>Berechtigungen

Erfordert die ALTER-Berechtigung für die Tabelle.

ALTER TABLE-Berechtigungen gelten für beide an einer ALTER TABLE SWITCH-Anweisung beteiligten Tabellen. Alle verschobenen Daten erben die Sicherheitseinstellungen der Zieltabelle.

Falls Sie Spalten in der ALTER TABLE-Anweisung mit einem benutzerdefinierten CLR-Typ (Common Language Runtime) oder Aliasdatentyp definieren, ist die REFERENCES-Berechtigung für den Typ erforderlich.

Für das Hinzufügen oder Ändern einer Spalte, durch die die Zeilen der Tabelle aktualisiert werden, ist die **UPDATE**-Berechtigung für die Tabelle erforderlich. Dies gilt beispielsweise für das Hinzufügen einer **NOT NULL**-Spalte mit einem Standardwert oder für das Hinzufügen einer Identitätsspalte zu einer nicht leeren Tabelle.

## <a name="examples"></a><a name="Example_Top"></a> Beispiele

|Category|Funktionssyntaxelemente|
|--------------|------------------------------|
|[Hinzufügen von Spalten und Einschränkungen](#add)|ADD * PRIMARY KEY mit Indexoptionen * Sparsespalten und Spaltensätze *|
|[Löschen von Spalten und Einschränkungen](#Drop)|DROP|
|[Ändern einer Spaltendefinition](#alter_column)|Ändern des Datentyps * Ändern der Spaltengröße * Sortierung|
|[Ändern einer Tabellendefinition](#alter_table)|DATA_COMPRESSION * SWITCH PARTITION * LOCK ESCALATION • Änderungsnachverfolgung|
|[Deaktivieren und Aktivieren von Einschränkungen und Triggern](#disable_enable)|CHECK * NO CHECK * ENABLE TRIGGER * DISABLE TRIGGER|
| &nbsp; | &nbsp; |

### <a name="adding-columns-and-constraints"></a><a name="add"></a>Hinzufügen von Spalten und Einschränkungen

Die Beispiele in diesem Abschnitt veranschaulichen das Hinzufügen von Spalten und Einschränkungen zu einer Tabelle.

#### <a name="a-adding-a-new-column"></a>A. Hinzufügen einer neuen Spalte

Im folgenden Beispiel wird eine Spalte hinzugefügt, die NULL-Werte zulässt und für die keine Werte durch eine DEFAULT-Definition bereitgestellt werden. Jede Zeile in der neuen Spalte erhält einen `NULL`-Wert.

```sql
CREATE TABLE dbo.doc_exa (column_a INT) ;
GO
ALTER TABLE dbo.doc_exa ADD column_b VARCHAR(20) NULL ;
GO
```

#### <a name="b-adding-a-column-with-a-constraint"></a>B. Hinzufügen einer Spalte mit einer Einschränkung

Im folgenden Beispiel wird eine neue Spalte mit einer `UNIQUE`-Einschränkung hinzugefügt.

```sql
CREATE TABLE dbo.doc_exc (column_a INT) ;
GO
ALTER TABLE dbo.doc_exc ADD column_b VARCHAR(20) NULL
    CONSTRAINT exb_unique UNIQUE ;
GO
EXEC sp_help doc_exc ;
GO
DROP TABLE dbo.doc_exc ;
GO
```

#### <a name="c-adding-an-unverified-check-constraint-to-an-existing-column"></a>C. Hinzufügen einer nicht überprüften CHECK-Einschränkung zu einer vorhandenen Spalte

Im folgenden Beispiel wird einer vorhandenen Spalte in der Tabelle eine Einschränkung hinzugefügt. Die Spalte hat einen Wert, der die Einschränkung verletzt. Deshalb wird `WITH NOCHECK` verwendet, um zu verhindern, dass die Einschränkung für vorhandene Zeilen überprüft wird, und um das Hinzufügen der Einschränkung zu ermöglichen.

```sql
CREATE TABLE dbo.doc_exd (column_a INT) ;
GO
INSERT INTO dbo.doc_exd VALUES (-1) ;
GO
ALTER TABLE dbo.doc_exd WITH NOCHECK
ADD CONSTRAINT exd_check CHECK (column_a > 1) ;
GO
EXEC sp_help doc_exd ;
GO
DROP TABLE dbo.doc_exd ;
GO
```

#### <a name="d-adding-a-default-constraint-to-an-existing-column"></a>D: Hinzufügen einer DEFAULT-Einschränkung zu einer vorhandenen Spalte

Im folgenden Beispiel wird eine Tabelle mit zwei Spalten erstellt und ein Wert in die erste Spalte eingefügt, während die andere Spalte NULL bleibt. Anschließend wird der zweiten Spalte eine `DEFAULT`-Einschränkung hinzugefügt. Um zu überprüfen, ob der Standardwert angewendet wird, wird ein weiterer Wert in die erste Spalte eingefügt und die Tabelle abgefragt.

```sql
CREATE TABLE dbo.doc_exz (column_a INT, column_b INT) ;
GO
INSERT INTO dbo.doc_exz (column_a) VALUES (7) ;
GO
ALTER TABLE dbo.doc_exz
  ADD CONSTRAINT col_b_def
  DEFAULT 50 FOR column_b ;
GO
INSERT INTO dbo.doc_exz (column_a) VALUES (10) ;
GO
SELECT * FROM dbo.doc_exz ;
GO
DROP TABLE dbo.doc_exz ;
GO
```

#### <a name="e-adding-several-columns-with-constraints"></a>E. Hinzufügen mehrerer Spalten mit Einschränkungen

Im folgenden Beispiel werden mehrere Spalten mit Einschränkungen hinzugefügt, die mit der neuen Spalte definiert werden. Die erste neue Spalte weist eine `IDENTITY`-Eigenschaft auf. Jede Zeile in der Tabelle besitzt neue inkrementelle Werte in der Identitätsspalte.

```sql
CREATE TABLE dbo.doc_exe (column_a INT CONSTRAINT column_a_un UNIQUE) ;
GO
ALTER TABLE dbo.doc_exe ADD

-- Add a PRIMARY KEY identity column.
column_b INT IDENTITY
CONSTRAINT column_b_pk PRIMARY KEY,

-- Add a column that references another column in the same table.
column_c INT NULL
CONSTRAINT column_c_fk
REFERENCES doc_exe(column_a),

-- Add a column with a constraint to enforce that
-- nonnull data is in a valid telephone number format.
column_d VARCHAR(16) NULL
CONSTRAINT column_d_chk
CHECK
(column_d LIKE '[0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]' OR
column_d LIKE
'([0-9][0-9][0-9]) [0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]'),

-- Add a nonnull column with a default.
column_e DECIMAL(3,3)
CONSTRAINT column_e_default
DEFAULT .081 ;
GO
EXEC sp_help doc_exe ;
GO
DROP TABLE dbo.doc_exe ;
GO
```

#### <a name="f-adding-a-nullable-column-with-default-values"></a>F. Hinzufügen einer Spalte, die NULL-Werte zulässt, mit Standardwerten

Im folgenden Beispiel wird eine Spalte, die NULL-Werte zulässt, mit einer `DEFAULT`-Definition hinzugefügt und `WITH VALUES` verwendet, um Werte für jede vorhandene Zeile in der Tabelle bereitzustellen. Ohne WITH VALUES hat jede Zeile in der neuen Spalte den Wert NULL.

```sql
CREATE TABLE dbo.doc_exf (column_a INT) ;
GO
INSERT INTO dbo.doc_exf VALUES (1) ;
GO
ALTER TABLE dbo.doc_exf
ADD AddDate smalldatetime NULL
CONSTRAINT AddDateDflt
DEFAULT GETDATE() WITH VALUES ;
GO
DROP TABLE dbo.doc_exf ;
GO
```

#### <a name="g-creating-a-primary-key-constraint-with-index-or-data-compression-options"></a>G. Erstellen einer PRIMARY KEY-Einschränkung mit Index- oder Datenkomprimierungsoptionen

Im folgenden Beispiel wird die PRIMARY KEY-Einschränkung `PK_TransactionHistoryArchive_TransactionID` erstellt, und es werden die Optionen `FILLFACTOR`, `ONLINE` und `PAD_INDEX` festgelegt. Der entstehende gruppierte Index hat denselben Namen wie die Einschränkung.

**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
USE AdventureWorks;
GO
ALTER TABLE Production.TransactionHistoryArchive WITH NOCHECK
ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
WITH (FILLFACTOR = 75, ONLINE = ON, PAD_INDEX = ON);
GO
```

In diesem ähnlichen Beispiel werden Seitenkomprimierung und der gruppierte Primärschlüssel angewandt.

```sql
USE AdventureWorks;
GO
ALTER TABLE Production.TransactionHistoryArchive WITH NOCHECK
ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
WITH (DATA_COMPRESSION = PAGE);
GO
```

#### <a name="h-adding-a-sparse-column"></a>H. Hinzufügen einer Sparsespalte

In den folgenden Beispielen wird gezeigt, wie Sparsespalten der Tabelle T1 hinzugefügt und geändert werden. Der Code zum Erstellen der Tabelle `T1` lautet wie folgt.

```sql
CREATE TABLE T1 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) SPARSE NULL,
  C3 INT SPARSE NULL,
  C4 INT) ;
GO
```

Um eine zusätzliche Sparsespalte `C5` hinzuzufügen, führen Sie die folgende Anweisung aus.

```sql
ALTER TABLE T1
ADD C5 CHAR(100) SPARSE NULL ;
GO
```

Um die Nicht-Sparsespalte `C4` in eine Sparsespalte umzuwandeln, führen Sie die folgende Anweisung aus.

```sql
ALTER TABLE T1
ALTER COLUMN C4 ADD SPARSE ;
GO
```

Um die `C4`-Sparsespalte in eine Nicht-Sparsespalte umzuwandeln, führen Sie die folgende Anweisung aus.

```sql
ALTER TABLE T1
ALTER COLUMN C4 DROP SPARSE ;
GO
```

#### <a name="i-adding-a-column-set"></a>I. Hinzufügen eines Spaltensatzes

In den folgenden Beispielen wird veranschaulicht, wie eine Spalte der Tabelle `T2` hinzugefügt wird. Sie können einer Tabelle, die bereits Sparsespalten enthält, keinen Spaltensatz hinzufügen. Der Code zum Erstellen der Tabelle `T2` lautet wie folgt.

```sql
CREATE TABLE T2 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

Durch die folgenden drei Anweisungen werden der Spaltensatz `CS` hinzugefügt und dann die Spalten `C2` und `C3` in `SPARSE` geändert.

```sql
ALTER TABLE T2
ADD CS XML COLUMN_SET FOR ALL_SPARSE_COLUMNS ;
GO

ALTER TABLE T2
ALTER COLUMN C2 ADD SPARSE ;
GO

ALTER TABLE T2
ALTER COLUMN C3 ADD SPARSE ;
GO
```

#### <a name="j-adding-an-encrypted-column"></a>J. Hinzufügen einer verschlüsselten Spalte

Die folgende Anweisung fügt eine verschlüsselte Spalte mit dem Namen `PromotionCode` hinzu.

```sql
ALTER TABLE Customers ADD
    PromotionCode nvarchar(100)
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,
    ENCRYPTION_TYPE = RANDOMIZED,
    ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') ;
```

### <a name="dropping-columns-and-constraints"></a><a name="Drop"></a>Löschen von Spalten und Einschränkungen

In den Beispielen in diesem Abschnitt wird das Löschen von Spalten und Einschränkungen veranschaulicht.

#### <a name="a-dropping-a-column-or-columns"></a>A. Löschen einer oder mehrerer Spalten

Im ersten Beispiel wird eine Tabelle durch Entfernen einer Spalte geändert. Im zweiten Beispiel werden mehrere Spalten entfernt.

```sql
CREATE TABLE dbo.doc_exb (
     column_a INT,
     column_b VARCHAR(20) NULL,
     column_c DATETIME,
     column_d INT) ;
GO  
-- Remove a single column.
ALTER TABLE dbo.doc_exb DROP COLUMN column_b ;
GO
-- Remove multiple columns.
ALTER TABLE dbo.doc_exb DROP COLUMN column_c, column_d;
```

#### <a name="b-dropping-constraints-and-columns"></a>B. Löschen von Einschränkungen und Spalten

Im ersten Beispiel wird eine `UNIQUE`-Einschränkung aus einer Tabelle entfernt. Im zweiten Beispiel werden zwei Einschränkungen und eine einzelne Spalte entfernt.

```sql
CREATE TABLE dbo.doc_exc (column_a INT NOT NULL CONSTRAINT my_constraint UNIQUE) ;
GO

-- Example 1. Remove a single constraint.
ALTER TABLE dbo.doc_exc DROP my_constraint ;
GO

DROP TABLE dbo.doc_exc;
GO

CREATE TABLE dbo.doc_exc ( column_a INT
                          NOT NULL CONSTRAINT my_constraint UNIQUE
                          ,column_b INT
                          NOT NULL CONSTRAINT my_pk_constraint PRIMARY KEY) ;
GO

-- Example 2. Remove two constraints and one column
-- The keyword CONSTRAINT is optional. The keyword COLUMN is required.
ALTER TABLE dbo.doc_exc
DROP CONSTRAINT my_constraint, my_pk_constraint, COLUMN column_b ;
GO
```

#### <a name="c-dropping-a-primary-key-constraint-in-the-online-mode"></a>C. Löschen einer PRIMARY KEY-Einschränkung im ONLINE-Modus

Im folgenden Beispiel wird eine PRIMARY KEY-Einschränkung gelöscht, wobei die `ONLINE`-Option auf `ON` festgelegt wird.

```sql
ALTER TABLE Production.TransactionHistoryArchive
DROP CONSTRAINT PK_TransactionHistoryArchive_TransactionID
WITH (ONLINE = ON) ;
GO
```

#### <a name="d-adding-and-dropping-a-foreign-key-constraint"></a>D: Hinzufügen und Löschen einer FOREIGN KEY-Einschränkung

Im folgenden Beispiel wird die `ContactBackup`-Tabelle erstellt und dann geändert, indem zuerst eine `FOREIGN KEY`-Einschränkung hinzugefügt wird, die auf die `Person.Person`-Tabelle verweist, und dann die `FOREIGN KEY`-Einschränkung gelöscht wird.

```sql
CREATE TABLE Person.ContactBackup
    (ContactID INT) ;
GO

ALTER TABLE Person.ContactBackup
ADD CONSTRAINT FK_ContactBackup_Contact FOREIGN KEY (ContactID)
    REFERENCES Person.Person (BusinessEntityID) ;
GO

ALTER TABLE Person.ContactBackup
DROP CONSTRAINT FK_ContactBackup_Contact ;
GO

DROP TABLE Person.ContactBackup ;
```

![Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Pfeilsymbol, das mit dem Link „Zurück zum Anfang“ verwendet wird")[Beispiele](#Example_Top)

### <a name="altering-a-column-definition"></a><a name="alter_column"></a> Ändern einer Spaltendefinition

#### <a name="a-changing-the-data-type-of-a-column"></a>A. Ändern des Datentyps einer Spalte

Im folgenden Beispiel wird eine Spalte einer Tabelle von `INT` in `DECIMAL` geändert.

```sql
CREATE TABLE dbo.doc_exy (column_a INT) ;
GO
INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;
GO
ALTER TABLE dbo.doc_exy ALTER COLUMN column_a DECIMAL (5, 2) ;
GO
DROP TABLE dbo.doc_exy ;
GO
```

#### <a name="b-changing-the-size-of-a-column"></a>B. Ändern der Größe einer Spalte

Im folgenden Beispiel werden die Größe einer **varchar**-Spalte sowie die Genauigkeit und die Dezimalstellen einer **decimal**-Spalte geändert. Da die Spalten Daten enthalten, kann die Spaltengröße nur erhöht werden. Beachten Sie auch, dass `col_a` in einem eindeutigen Index definiert ist. Die Größe von `col_a` kann erhöht werden, da die Spalte vom Datentyp **varchar** und der Index nicht das Ergebnis einer PRIMARY KEY-Einschränkung ist.

```sql
-- Create a two-column table with a unique index on the varchar column.
CREATE TABLE dbo.doc_exy (col_a varchar(5) UNIQUE NOT NULL, col_b decimal (4,2)) ;
GO
INSERT INTO dbo.doc_exy VALUES ('Test', 99.99) ;
GO
-- Verify the current column size.
SELECT name, TYPE_NAME(system_type_id), max_length, precision, scale
FROM sys.columns WHERE object_id = OBJECT_ID(N'dbo.doc_exy') ;
GO
-- Increase the size of the varchar column.
ALTER TABLE dbo.doc_exy ALTER COLUMN col_a varchar(25) ;
GO
-- Increase the scale and precision of the decimal column.
ALTER TABLE dbo.doc_exy ALTER COLUMN col_b decimal (10,4) ;
GO
-- Insert a new row.
INSERT INTO dbo.doc_exy VALUES ('MyNewColumnSize', 99999.9999) ;
GO
-- Verify the current column size.
SELECT name, TYPE_NAME(system_type_id), max_length, precision, scale
FROM sys.columns WHERE object_id = OBJECT_ID(N'dbo.doc_exy') ;
```

#### <a name="c-changing-column-collation"></a>C. Ändern von Spaltensortierungen

Im folgenden Beispiel wird gezeigt, wie die Sortierung einer Spalte geändert wird. Zuerst wird eine Tabelle mit der Standardbenutzersortierung erstellt.

```sql
CREATE TABLE T3 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

Anschließend wird die Sortierung der Spalte `C2` in Latin1_General_BIN geändert. Der Datentyp ist erforderlich, auch wenn er nicht geändert wird.

```sql
ALTER TABLE T3
ALTER COLUMN C2 varchar(50) COLLATE Latin1_General_BIN ;
GO
```

#### <a name="d-encrypting-a-column"></a>D: Verschlüsseln einer Spalte

Das folgende Beispiel zeigt, wie Sie eine Spalte mit [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md) verschlüsseln.

Zunächst wird eine Tabelle ohne jegliche verschlüsselte Spalten erstellt.

```sql
CREATE TABLE T3 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

Als Nächstes wird die Spalte „C2“ mit einem Spaltenverschlüsselungsschlüssel namens „CEK1“ und Verschlüsselung nach dem Zufallsprinzip verschlüsselt. Für die erfolgreiche Ausführung der Anweisung ist Folgendes erforderlich:

- Der Spaltenverschlüsselungsschlüssel muss Enclave-fähig sein. Das bedeutet, dass er mit einem Spaltenhauptschlüssel verschlüsselt sein muss, der Enclave-Berechnungen zulässt.
- Die SQL Server-Zielinstanz muss Always Encrypted mit Secure Enclaves unterstützen.
- Die Anweisung muss über eine Verbindung übermittelt werden, die für Always Encrypted mit Secure Enclaves eingerichtet ist, und unter Verwendung eines unterstützten Clienttreibers.
- Außerdem benötigt die aufrufende Anwendung Zugriff auf den Spaltenhauptschlüssel, der „CEK1“ schützt.

```sql
ALTER TABLE T3
ALTER COLUMN C2 VARCHAR(50) ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NULL;
GO
```

### <a name="altering-a-table-definition"></a><a name="alter_table"></a> Ändern einer Tabellendefinition

Die Beispiele in diesem Abschnitt veranschaulichen, wie die Definition einer Tabelle geändert wird.

#### <a name="a-modifying-a-table-to-change-the-compression"></a>A. Ändern einer Tabelle, um die Komprimierung zu ändern

Im folgenden Beispiel wird die Komprimierung einer nicht partitionierten Tabelle geändert. Der Heap oder der gruppierte Index wird neu erstellt. Wenn die Tabelle ein Heap ist, werden alle nicht gruppierten Indizes neu erstellt.

```sql
ALTER TABLE T1
REBUILD WITH (DATA_COMPRESSION = PAGE) ;
```

Im folgenden Beispiel wird die Komprimierung einer partitionierten Tabelle geändert. Die `REBUILD PARTITION = 1`-Syntax bewirkt, dass nur die Partition `1` neu erstellt wird.

**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =NONE) ;
GO
```

Mit der folgenden alternativen Syntax werden im gleichen Vorgang alle Partitionen in der Tabelle neu erstellt.

**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = ALL
WITH (DATA_COMPRESSION = PAGE ON PARTITIONS(1)) ;
```

Weitere Beispiele für die Datenkomprimierung finden Sie unter [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md).

#### <a name="b-modifying-a-columnstore-table-to-change-archival-compression"></a>B. Ändern einer columnstore-Tabelle, um die Archivierungskomprimierung zu ändern

Im folgenden Beispiel wird eine columnstore-Tabellenpartition weiter komprimiert, indem ein zusätzlicher Komprimierungsalgorithmus angewendet wird. Durch diese Komprimierung wird zwar die Tabellengröße reduziert, aber die zum Speichern und Abrufen benötigte Zeit wird verlängert. Dies empfiehlt sich bei der Archivierung und in Situationen, in denen es auf eine geringere Speicherbelegung und nicht auf den zusätzlichen Zeitaufwand für das Speichern und Abrufen ankommt.

**Gilt für:** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE) ;
GO
```

Im folgenden Beispiel wird eine columnstore-Tabellenpartition, die mit der COLUMNSTORE_ARCHIVE-Option komprimiert wurde, dekomprimiert. Nachdem die Daten wiederhergestellt wurden, sind sie weiterhin mit der Columnstore-Komprimierung komprimiert, die für alle Columnstore-Tabellen verwendet wird.

**Gilt für:** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION = COLUMNSTORE) ;
GO
```

#### <a name="c-switching-partitions-between-tables"></a>C. Wechseln von Partitionen zwischen Tabellen

Im folgenden Beispiel wird eine partitionierte Tabelle erstellt, wobei vorausgesetzt wird, dass das `myRangePS1`-Partitionsschema bereits in der Datenbank erstellt wurde. Anschließend wird eine nicht partitionierte Tabelle mit derselben Struktur wie die partitionierte Tabelle und in derselben Dateigruppe wie `PARTITION 2` der `PartitionTable`-Tabelle erstellt. Die Daten von `PARTITION 2` der `PartitionTable`-Tabelle werden dann in die `NonPartitionTable`-Tabelle verschoben.

```sql
CREATE TABLE PartitionTable (col1 INT, col2 CHAR(10))
ON myRangePS1 (col1) ;
GO
CREATE TABLE NonPartitionTable (col1 INT, col2 CHAR(10))
ON test2fg ;
GO
ALTER TABLE PartitionTable SWITCH PARTITION 2 TO NonPartitionTable ;
GO
```

#### <a name="d-allowing-lock-escalation-on-partitioned-tables"></a>D: Zulassen der Sperrenausweitung in partitionierten Tabellen

Im folgenden Beispiel wird die Sperrenausweitung auf die Partitionsebene einer partitionierten Tabelle ermöglicht. Wenn die Tabelle nicht partitioniert ist, wird die Sperrenausweitung auf der TABLE-Ebene festgelegt.

**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE dbo.T1 SET (LOCK_ESCALATION = AUTO) ;
GO
```

#### <a name="e-configuring-change-tracking-on-a-table"></a>E. Konfigurieren der Änderungsnachverfolgung in einer Tabelle

Im folgenden Beispiel wird die Änderungsnachverfolgung für die `Person.Person`-Tabelle aktiviert.

**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
USE AdventureWorks;
ALTER TABLE Person.Person
ENABLE CHANGE_TRACKING ;
```

Im folgenden Beispiel werden die Änderungsnachverfolgung sowie die Verfolgung von Spalten aktiviert, die während einer Änderung aktualisiert werden.

**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.

```sql
USE AdventureWorks;
GO
ALTER TABLE Person.Person
ENABLE CHANGE_TRACKING
WITH (TRACK_COLUMNS_UPDATED = ON)
```

Im folgenden Beispiel wird die Änderungsnachverfolgung für die `Person.Person`-Tabelle deaktiviert.

**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
USE AdventureWorks;
GO
ALTER TABLE Person.Person
DISABLE CHANGE_TRACKING ;
```

### <a name="disabling-and-enabling-constraints-and-triggers"></a><a name="disable_enable"></a>Deaktivieren und Aktivieren von Einschränkungen und Triggern

#### <a name="a-disabling-and-re-enabling-a-constraint"></a>A. Deaktivieren und erneutes Aktivieren einer Einschränkung

Im folgenden Beispiel wird eine Einschränkung deaktiviert, die die in den Daten akzeptierten Gehälter begrenzt. `NOCHECK CONSTRAINT` wird mit `ALTER TABLE` verwendet, um die Einschränkung zu deaktivieren und eine Einfügung zuzulassen, die die Einschränkung normalerweise verletzen würde. Mit `CHECK CONSTRAINT` wird die Einschränkung wieder aktiviert.

```sql
CREATE TABLE dbo.cnst_example (
  id INT NOT NULL,
  name VARCHAR(10) NOT NULL,
  salary MONEY NOT NULL
  CONSTRAINT salary_cap CHECK (salary < 100000)) ;

-- Valid inserts
INSERT INTO dbo.cnst_example VALUES (1,'Joe Brown',65000) ;
INSERT INTO dbo.cnst_example VALUES (2,'Mary Smith',75000) ;

-- This insert violates the constraint.
INSERT INTO dbo.cnst_example VALUES (3,'Pat Jones',105000) ;

-- Disable the constraint and try again.
ALTER TABLE dbo.cnst_example NOCHECK CONSTRAINT salary_cap;
INSERT INTO dbo.cnst_example VALUES (3,'Pat Jones',105000) ;

-- Re-enable the constraint and try another insert; this will fail.
ALTER TABLE dbo.cnst_example CHECK CONSTRAINT salary_cap;
INSERT INTO dbo.cnst_example VALUES (4,'Eric James',110000) ;
```

#### <a name="b-disabling-and-re-enabling-a-trigger"></a>B. Deaktivieren und erneutes Aktivieren eines Triggers

Im folgenden Beispiel wird die `DISABLE TRIGGER`-Option von `ALTER TABLE` verwendet, um den Trigger zu deaktivieren und eine Einfügung zuzulassen, die den Trigger normalerweise verletzen würde. Anschließend wird der Trigger mithilfe von `ENABLE TRIGGER` wieder aktiviert.

```sql
CREATE TABLE dbo.trig_example (
  id INT,
  name VARCHAR(12),
  salary MONEY) ;
GO
-- Create the trigger.
CREATE TRIGGER dbo.trig1 ON dbo.trig_example FOR INSERT
AS
IF (SELECT COUNT(*) FROM INSERTED
WHERE salary > 100000) > 0
BEGIN
    print 'TRIG1 Error: you attempted to insert a salary > $100,000'
    ROLLBACK TRANSACTION
END ;
GO
-- Try an insert that violates the trigger.
INSERT INTO dbo.trig_example VALUES (1,'Pat Smith',100001) ;
GO
-- Disable the trigger.
ALTER TABLE dbo.trig_example DISABLE TRIGGER trig1 ;
GO
-- Try an insert that would typically violate the trigger.
INSERT INTO dbo.trig_example VALUES (2,'Chuck Jones',100001) ;
GO
-- Re-enable the trigger.
ALTER TABLE dbo.trig_example ENABLE TRIGGER trig1 ;
GO
-- Try an insert that violates the trigger.
INSERT INTO dbo.trig_example VALUES (3,'Mary Booth',100001) ;
GO
```

### <a name="online-operations"></a><a name="online"></a>Onlinevorgänge

#### <a name="a-online-index-rebuild-using-low-priority-wait-options"></a>A. Onlineindexneuerstellung mit LOW_PRIORITY_WAIT-Optionen

Im folgenden Beispiel wird gezeigt, wie eine Onlineindexneuerstellung mithilfe der LOW_PRIORITY_WAIT-Optionen ausgeführt wird.

**Gilt für:** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
ALTER TABLE T1
REBUILD WITH
(
    PAD_INDEX = ON,
    ONLINE = ON ( WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 4 MINUTES,
                                         ABORT_AFTER_WAIT = BLOCKERS ) )
) ;
```

#### <a name="b-online-alter-column"></a>B. Onlinespaltenänderung

Das folgende Beispiel zeigt, wie Sie einen ALTER COLUMN-Vorgang mit der Option ONLINE ausführen.

**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

```sql
CREATE TABLE dbo.doc_exy (column_a INT) ;
GO
INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;
GO
ALTER TABLE dbo.doc_exy
    ALTER COLUMN column_a DECIMAL (5, 2) WITH (ONLINE = ON) ;
GO
sp_help doc_exy;
DROP TABLE dbo.doc_exy ;
GO
```

### <a name="system-versioning"></a><a name="system_versioning"></a> Zeilenversionsverwaltung

Mit den folgenden vier Beispielen können Sie sich mit der Syntax für die Systemversionierung vertraut machen. Weitere Informationen erhalten Sie unter [Erste Schritte mit temporalen Tabellen mit Systemversionsverwaltung](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md).

**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

#### <a name="a-add-system-versioning-to-existing-tables"></a>A. Hinzufügen der Systemversionierung zu vorhandenen Tabellen

Im Folgenden wird veranschaulicht, wie Sie die Systemversionierung einer vorhandenen Tabelle hinzufügen und eine neue Verlaufstabelle erstellen. In diesem Beispiel wird davon ausgegangen, dass eine Tabelle mit dem Namen `InsurancePolicy` mit einem festgelegten Primärschlüssel bereits vorhanden ist. Dieses Beispiel füllt die neu erstellten Zeitraumspalten für die Systemversionsverwaltung mit Standardwerten für Start- und Endzeitpunkt auf, da diese Werte nicht NULL sein dürfen. In diesem Beispiel wird die HIDDEN-Klausel verwendet, um sicherzustellen, dass vorhandene Anwendungen, die mit der aktuellen Tabelle interagieren, nicht beeinträchtigt werden. Außerdem wird HISTORY_RETENTION_PERIOD verwendet, das nur in [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] verfügbar ist.

```sql
--Alter non-temporal table to define periods for system versioning
ALTER TABLE InsurancePolicy
ADD PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime),
SysStartTime datetime2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL
    DEFAULT SYSUTCDATETIME(),
SysEndTime datetime2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL
    DEFAULT CONVERT(DATETIME2, '9999-12-31 23:59:59.99999999') ;

--Enable system versioning with 1 year retention for historical data
ALTER TABLE InsurancePolicy
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 1 YEAR)) ;
```

#### <a name="b-migrate-an-existing-solution-to-use-system-versioning"></a>B. Migrieren einer vorhandenen Projektmappe zur Systemversionierung

In folgendem Beispiel wird gezeigt, wie Sie die Migration zur Systemversionierung von einer Projektmappe durchführen, die Trigger verwendet, um die Unterstützung für temporale Tabellen nachzuahmen. Im Beispiel wird davon ausgegangen, dass eine Projektmappe vorhanden ist, die eine `ProjectTask`- und eine `ProjectTaskHistory`-Tabelle für ihre vorhandene Projektmappe verwendet, dass sie die Spalten `Changed Date` und `Revised Date` für ihren Zeitraum verwendet, dass diese Zeitraumspalten nicht den Datentyp `datetime2` verwenden und dass für die `ProjectTask`-Tabelle ein Primärschlüssel festgelegt ist.

```sql
-- Drop existing trigger
DROP TRIGGER ProjectTask_HistoryTrigger;

-- Adjust the schema for current and history table
-- Change data types for existing period columns
ALTER TABLE ProjectTask ALTER COLUMN [Changed Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTask ALTER COLUMN [Revised Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [Changed Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [Revised Date] datetime2 NOT NULL ;

-- Add SYSTEM_TIME period and set system versioning with linking two existing tables
-- (a certain set of data checks happen in the background)
ALTER TABLE ProjectTask
ADD PERIOD FOR SYSTEM_TIME ([Changed Date], [Revised Date])

ALTER TABLE ProjectTask
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProjectTaskHistory, DATA_CONSISTENCY_CHECK = ON))
```

#### <a name="c-disabling-and-re-enabling-system-versioning-to-change-table-schema"></a>C. Deaktivieren und erneutes Aktivieren der Systemversionierung zum Ändern des Tabellenschemas

In diesem Beispiel wird gezeigt, wie Sie die Systemversionierung in der `Department`-Tabelle deaktivieren, eine Spalte hinzufügen und die Systemversionierung erneut aktivieren können. Das Deaktivieren der Systemversionsverwaltung ist nötig, um das Tabellenschema ändern zu können. Führen Sie diese Schritte während einer Transaktion durch, um Aktualisierungen beider Tabellen zu verhindern, während das Tabellenschema aktualisiert wird. Dadurch kann der Datenbankadministrator die Datenkonsistenzprüfung überspringen, wenn die Systemversionsverwaltung erneut aktiviert wird. So wird die Leistung verbessert. Für Aufgaben wie das Erstellen von Statistiken, das Wechseln von Partitionen oder das Anwenden der Komprimierung auf eine oder beide Tabellen ist das Deaktivieren der Systemversionsverwaltung nicht erforderlich.

```sql
BEGIN TRAN
/* Takes schema lock on both tables */
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = OFF) ;
/* expand table schema for temporal table */
ALTER TABLE Department  
     ADD Col5 int NOT NULL DEFAULT 0 ;
/* Expand table schema for history table */
ALTER TABLE DepartmentHistory
    ADD Col5 int NOT NULL DEFAULT 0 ;
/* Re-establish versioning again*/
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE=dbo.DepartmentHistory,
                                 DATA_CONSISTENCY_CHECK = OFF)) ;
COMMIT
```

#### <a name="d-removing-system-versioning"></a>D: Entfernen der Systemversionierung

In diesem Beispiel wird gezeigt, wie Sie die Systemversionierung aus der Tabelle „Department“ entfernen und die `DepartmentHistory`-Tabelle vollständig löschen. Sie können auch die Zeitraumspalten löschen, die vom System zum Erfassen von Systemversionierungsinformationen verwendet werden. Die Tabellen `Department` und `DepartmentHistory` können nicht gelöscht werden, solange die Systemversionsverwaltung aktiviert ist.

```sql
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = OFF) ;
ALTER TABLE Department
DROP PERIOD FOR SYSTEM_TIME ;
DROP TABLE DepartmentHistory ;
```

## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

In allen folgenden Beispielen A bis C wird die `FactResellerSales`-Tabelle der [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)]-Datenbank verwendet.

### <a name="a-determining-if-a-table-is-partitioned"></a>A. Bestimmen, ob eine Tabelle partitioniert ist

Die folgende Abfrage gibt mindestens eine Zeile zurück, wenn die `FactResellerSales` -Tabelle partitioniert ist. Wenn die Tabelle nicht partitioniert ist, werden keine Zeilen zurückgegeben.

```sql
SELECT * FROM sys.partitions AS p
JOIN sys.tables AS t
    ON p.object_id = t.object_id
WHERE p.partition_id IS NOT NULL
    AND t.name = 'FactResellerSales' ;
```

### <a name="b-determining-boundary-values-for-a-partitioned-table"></a>B. Bestimmen von Begrenzungswerte für eine partitionierte Tabelle

Die folgende Abfrage gibt die Begrenzungswerte für jede Partition in der `FactResellerSales` -Tabelle zurück.

```sql
SELECT t.name AS TableName, i.name AS IndexName, p.partition_number,
    p.partition_id, i.data_space_id, f.function_id, f.type_desc,
    r.boundary_id, r.value AS BoundaryValue
FROM sys.tables AS t
JOIN sys.indexes AS i
    ON t.object_id = i.object_id
JOIN sys.partitions AS p
    ON i.object_id = p.object_id AND i.index_id = p.index_id
JOIN  sys.partition_schemes AS s
    ON i.data_space_id = s.data_space_id
JOIN sys.partition_functions AS f
    ON s.function_id = f.function_id
LEFT JOIN sys.partition_range_values AS r
    ON f.function_id = r.function_id and r.boundary_id = p.partition_number
WHERE t.name = 'FactResellerSales' AND i.type <= 1
ORDER BY p.partition_number ;
```

### <a name="c-determining-the-partition-column-for-a-partitioned-table"></a>C. Bestimmen der Partitionsspalte für eine partitionierte Tabelle

Die folgende Abfrage gibt den Namen der Partitionierungsspalte für die Tabelle zurück. `FactResellerSales`.

```sql
SELECT t.object_id AS Object_ID, t.name AS TableName,
    ic.column_id as PartitioningColumnID, c.name AS PartitioningColumnName
FROM sys.tables AS t
JOIN sys.indexes AS i
    ON t.object_id = i.object_id
JOIN sys.columns AS c
    ON t.object_id = c.object_id
JOIN sys.partition_schemes AS ps
    ON ps.data_space_id = i.data_space_id
JOIN sys.index_columns AS ic
    ON ic.object_id = i.object_id
    AND ic.index_id = i.index_id AND ic.partition_ordinal > 0
WHERE t.name = 'FactResellerSales'
AND i.type <= 1
AND c.column_id = ic.column_id ;
```

### <a name="d-merging-two-partitions"></a>D: Zusammenführen zweier Partitionen

In folgendem Beispiel werden zwei Partitionen in einer Tabelle zusammengeführt.

Die `Customer`-Tabelle verfügt über die folgende Struktur:

```sql
CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 10, 25, 50, 100))) ;
```

Der folgende Befehl kombiniert die Partitionsgrenzen 10 und 25.

```sql
ALTER TABLE Customer MERGE RANGE (10);
```

Die neue DLL für die Tabelle ist:

```sql
CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 25, 50, 100))) ;
```

### <a name="e-splitting-a-partition"></a>E. Teilen einer Partition

In folgendem Beispiel wird eine Partition in einer Tabelle geteilt.

Die `Customer`-Tabelle verfügt über die folgende Struktur:

```sql
DROP TABLE Customer;

CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 10, 25, 50, 100 ))) ;
```

Der folgende Befehl erstellt eine neue Partitionsgrenze mit dem Wert 75, in der Mitte zwischen 50 und 100.

```sql
ALTER TABLE Customer SPLIT RANGE (75);
```

Die neue DLL für die Tabelle ist:

```sql
CREATE TABLE Customer (
   id INT NOT NULL,
   lastName VARCHAR(20),
   orderCount INT,
   orderDate DATE)
   WITH DISTRIBUTION = HASH(id),
   PARTITION ( orderCount (RANGE LEFT
      FOR VALUES (1, 5, 10, 25, 50, 75, 100))) ;
```

### <a name="f-using-switch-to-move-a-partition-to-a-history-table"></a>F. Verwenden von SWITCH zum Verschieben einer Partition in eine Verlaufstabelle

In folgendem Beispiel werden die Daten in einer Partition der `Orders`-Tabelle in eine Partition der `OrdersHistory`-Tabelle verschoben.

Die `Orders`-Tabelle verfügt über die folgende Struktur:

```sql
CREATE TABLE Orders (
    id INT,
    city VARCHAR (25),
    lastUpdateDate DATE,
    orderDate DATE)
WITH
    (DISTRIBUTION = HASH (id),
    PARTITION ( orderDate RANGE RIGHT
    FOR VALUES ('2004-01-01', '2005-01-01', '2006-01-01', '2007-01-01'))) ;
```

In diesem Beispiel verfügt die `Orders`-Tabelle über die folgenden Partitionen. Jede Partition enthält Daten.

|Partition|Enthält Daten?|Begrenzungsbereich|
|---------------|---------------|--------------------|
|1|Ja|OrderDate < '2004-01-01'|
|2|Ja|'2004-01-01' <= OrderDate < '2005-01-01'|
|3|Ja|'2005-01-01' <= OrderDate< '2006-01-01'|
|4|Ja|'2006-01-01'<= OrderDate < '2007-01-01'|
|5|Ja|'2007-01-01' <= OrderDate|
| &nbsp; | &nbsp; | &nbsp; |

- Partition 1 (enthält Daten): OrderDate < '2004-01-01'
- Partition 2 (enthält Daten): '2004-01-01' <= OrderDate < '2005-01-01'
- Partition 3 (enthält Daten): '2005-01-01' <= OrderDate< '2006-01-01'
- Partition 4 (enthält Daten): '2006-01-01'<= OrderDate < '2007-01-01'
- Partition 5 (enthält Daten): '2007-01-01' <= OrderDate

Die `OrdersHistory`-Tabelle verfügt über die folgende DLL, die die gleichen Spalten und Spaltennamen wie die `Orders`-Tabelle aufweist. Diese werden in der `id`-Spalte mittels Hash verteilt.

```sql
CREATE TABLE OrdersHistory (
   id INT,
   city VARCHAR (25),
   lastUpdateDate DATE,
   orderDate DATE)
WITH
    (DISTRIBUTION = HASH (id),
    PARTITION ( orderDate RANGE RIGHT
    FOR VALUES ('2004-01-01'))) ;
```

Obwohl die Spalten und Spaltennamen identisch sein müssen, müssen die Partitionsbegrenzungen nicht übereinstimmen. In diesem Beispiel verfügt die `OrdersHistory`-Tabelle über zwei Partitionen, die beide leer sind:

- Partition 1 (keine Daten): OrderDate < '2004-01-01'
- Partition 2 (leer): '2004-01-01' <= OrderDate

Für die letzten beiden Tabellen verschiebt der Befehl alle Zeilen mit `OrderDate < '2004-01-01'` von der `Orders`-Tabelle in die `OrdersHistory`-Tabelle.

```sql
ALTER TABLE Orders SWITCH PARTITION 1 TO OrdersHistory PARTITION 1;
```

Dadurch ist die erste Partition in `Orders` leer, während die erste Partition in `OrdersHistory` Daten enthält. Die Tabellen sehen nun wie folgt aus:

 `Orders`-Tabelle

- Partition 1 (leer): OrderDate < '2004-01-01'
- Partition 2 (enthält Daten): '2004-01-01' <= OrderDate < '2005-01-01'
- Partition 3 (enthält Daten): '2005-01-01' <= OrderDate< '2006-01-01'
- Partition 4 (enthält Daten): '2006-01-01'<= OrderDate < '2007-01-01'
- Partition 5 (enthält Daten): '2007-01-01' <= OrderDate

`OrdersHistory`-Tabelle

- Partition 1 (enthält Daten): OrderDate < '2004-01-01'
- Partition 2 (leer): '2004-01-01' <= OrderDate

Bereinigen Sie die `Orders`-Tabelle, indem Sie die leere Partition entfernen, indem Sie Partition 1 und 2 folgendermaßen zusammenführen:

```sql
ALTER TABLE Orders MERGE RANGE ('2004-01-01');
```

Nach dem Merge verfügt die `Orders`-Tabelle über die folgenden Partitionen:

`Orders`-Tabelle

- Partition 1 (enthält Daten): OrderDate < '2005-01-01'
- Partition 2 (enthält Daten): '2005-01-01' <= OrderDate< '2006-01-01'
- Partition 3 (enthält Daten): '2006-01-01'<= OrderDate < '2007-01-01'
- Partition 4 (enthält Daten): '2007-01-01' <= OrderDate

Angenommen, es vergeht ein weiteres Jahr, und Sie möchten das Jahr 2005 archivieren. Sie können dem Jahr 2005 eine leere Partition in der `OrdersHistory`-Tabelle zuweisen, indem Sie die leere Partition folgendermaßen teilen:

```sql
ALTER TABLE OrdersHistory SPLIT RANGE ('2005-01-01');
```

Nach dem Teilen verfügt die `OrdersHistory`-Tabelle über die folgenden Partitionen:

 `OrdersHistory`-Tabelle

- Partition 1 (enthält Daten): OrderDate < '2004-01-01'
- Partition 2 (leer): '2004-01-01' < '2005-01-01'
- Partition 3 (leer): '2005-01-01' <= OrderDate

## <a name="see-also"></a>Weitere Informationen

- [sys.tables](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)
- [sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DROP TABLE](../../t-sql/statements/drop-table-transact-sql.md)
- [sp_help](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)
- [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md)
- [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
