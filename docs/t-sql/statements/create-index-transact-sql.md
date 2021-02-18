---
description: CREATE INDEX (Transact-SQL)
title: CREATE INDEX (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/17/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE INDEX
- INDEX
- INDEX_TSQL
- CREATE_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE XML INDEX statement
- PRIMARY XML INDEX statement
- indexes [SQL Server], creating
- online index operations
- index keys [SQL Server]
- PROPERTY INDEX statement
- computed columns, index creation
- clustered indexes, creating
- indexed views [SQL Server], index creation
- partitioned indexes [SQL Server], creating
- VALUE INDEX statement
- index creation [SQL Server], CREATE INDEX statement
- DROP_EXISTING clause
- row locks [SQL Server]
- composite indexes
- MAXDOP index option, CREATE INDEX statement
- filtered indexes [SQL Server], creating
- PATH INDEX statement
- locking [SQL Server], indexes
- unique indexes, creating
- primary indexes [SQL Server]
- CREATE PRIMARY XML INDEX statement
- SET statement, index creation
- index options [SQL Server]
- included columns
- ONLINE option
- nonclustered indexes [SQL Server], creating
- CREATE INDEX statement
- IGNORE_DUP_KEY option
- deterministic functions
- keys [SQL Server], index
- SECONDARY XML INDEX statement
- page locks [SQL Server]
- secondary indexes [SQL Server]
- XML indexes [SQL Server], creating
ms.assetid: d2297805-412b-47b5-aeeb-53388349a5b9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e02d3676b1b303ef6dbbae4a509ed0db0c608071
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525230"
---
# <a name="create-index-transact-sql"></a>CREATE INDEX (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Erstellt einen eindeutigen Index für eine Tabelle oder Sicht. Wird auch als Rowstore-Index bezeichnet, da es sich entweder um einen gruppierten oder nicht gruppierten B-Strukturindex handelt. Sie können noch bevor die Tabelle mit Daten aufgefüllt wird, einen Rowstore-Index erstellen. Verwenden Sie einen Rowstore-Index, um die Abfrageleistung zu verbessern, insbesondere, wenn die Abfragen aus bestimmten Spalten auswählen oder Werte erfordern, die in einer bestimmten Reihenfolge sortiert werden sollen.

> [!NOTE]
> [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] unterstützen Unique-Einschränkungen derzeit nicht. Jegliche Beispiele, die auf Unique-Einschränkungen verweisen, gelten nur für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDS](../../includes/sssds-md.md)].
> [!TIP]
> Weitere Informationen zu den Richtlinien für das Entwerfen von Indizes finden Sie im [Handbuch zum SQL Server-Indexentwurf](../../relational-databases/sql-server-index-design-guide.md).

 **Einfache Beispiele:**

```sql
-- Create a nonclustered index on a table or view
CREATE INDEX i1 ON t1 (col1);

-- Create a clustered index on a table and use a 3-part name for the table
CREATE CLUSTERED INDEX i1 ON d1.s1.t1 (col1);

-- Syntax for SQL Server and Azure SQL Database
-- Create a nonclustered index with a unique constraint
-- on 3 columns and specify the sort order for each column
CREATE UNIQUE INDEX i1 ON t1 (col1 DESC, col2 ASC, col3 DESC);
```

**Wichtiges Szenario:**

Verwenden Sie ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und [!INCLUDE[ssSDS](../../includes/sssds-md.md)] einen nicht gruppierten Index für einen Columnstore-Index, um die Abfrageleistung von Data Warehouse zu verbessern. Weitere Informationen finden Sie unter [Columnstore-Indizes: Data Warehouse](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md).

Informationen zu weiteren Indextypen finden Sie unter:

- [CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md)
- [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)
- [CREATE COLUMNSTORE INDEX](../../t-sql/statements/create-columnstore-index-transact-sql.md)

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

### <a name="syntax-for-sql-server-and-azure-sql-database"></a>Syntax für SQL Server und Azure SQL-Datenbank

```syntaxsql
CREATE [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON <object> ( column [ ASC | DESC ] [ ,...n ] )
    [ INCLUDE ( column_name [ ,...n ] ) ]
    [ WHERE <filter_predicate> ]
    [ WITH ( <relational_index_option> [ ,...n ] ) ]
    [ ON { partition_scheme_name ( column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
[ ; ]
  
<object> ::=
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }

<relational_index_option> ::=
{
    PAD_INDEX = { ON | OFF }
  | FILLFACTOR = fillfactor
  | SORT_IN_TEMPDB = { ON | OFF }
  | IGNORE_DUP_KEY = { ON | OFF }
  | STATISTICS_NORECOMPUTE = { ON | OFF }
  | STATISTICS_INCREMENTAL = { ON | OFF }
  | DROP_EXISTING = { ON | OFF }
  | ONLINE = { ON | OFF }
  | RESUMABLE = { ON | OFF }
  | MAX_DURATION = <time> [MINUTES]
  | ALLOW_ROW_LOCKS = { ON | OFF }
  | ALLOW_PAGE_LOCKS = { ON | OFF }
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
  | MAXDOP = max_degree_of_parallelism
  | DATA_COMPRESSION = { NONE | ROW | PAGE }
     [ ON PARTITIONS ( { <partition_number_expression> | <range> }
     [ , ...n ] ) ]
}

<filter_predicate> ::=
    <conjunct> [ AND <conjunct> ]

<conjunct> ::=
    <disjunct> | <comparison>

<disjunct> ::=
        column_name IN (constant ,...n)

<comparison> ::=
        column_name <comparison_op> constant

<comparison_op> ::=
    { IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !< }

<range> ::=
<partition_number_expression> TO <partition_number_expression>
```

### <a name="backward-compatible-relational-index"></a>Abwärtskompatibler relationaler Index

> [!IMPORTANT]
> Die abwärtskompatible Syntaxstruktur für den relationalen Index wird in einer zukünftigen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entfernt.
> Verwenden Sie diese Syntaxstruktur beim Entwickeln neuer Anwendungen nicht, und planen Sie das Ändern von Anwendungen, in denen die Funktion aktuell verwendet wird.
> Verwenden Sie stattdessen die in <relational_index_option> angegebene Syntaxstruktur.

```syntaxsql
CREATE [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON <object> ( column_name [ ASC | DESC ] [ ,...n ] )
    [ WITH <backward_compatible_index_option> [ ,...n ] ]
    [ ON { filegroup_name | "default" } ]

<object> ::=
{
    [ database_name. [ owner_name ] . | owner_name. ]
    table_or_view_name
}

<backward_compatible_index_option> ::=
{
    PAD_INDEX
  | FILLFACTOR = fillfactor
  | SORT_IN_TEMPDB
  | IGNORE_DUP_KEY
  | STATISTICS_NORECOMPUTE
  | DROP_EXISTING
}
```

### <a name="syntax-for-azure-synapse-analytics-and-parallel-data-warehouse"></a>Syntax für Azure Synapse Analytics und Parallel Data Warehouse

```syntaxsql

CREATE CLUSTERED COLUMNSTORE INDEX index_name
    ON [ database_name . [ schema ] . | schema . ] table_name
    [ORDER (column[,...n])]
    [WITH ( DROP_EXISTING = { ON | OFF } )]
[;]


CREATE [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON [ database_name . [ schema ] . | schema . ] table_name
        ( { column [ ASC | DESC ] } [ ,...n ] )
    WITH ( DROP_EXISTING = { ON | OFF } )
[;]


```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

UNIQUE      
Erstellt einen eindeutigen Index für eine Tabelle oder Sicht. Ein eindeutiger Index ist ein Index, bei dem zwei Zeilen nicht den gleichen Indexschlüsselwert haben dürfen. Ein gruppierter Index für eine Sicht muss eindeutig sein.

[!INCLUDE[ssDE](../../includes/ssde-md.md)] lässt das Erstellen eines eindeutigen Index für Spalten, die bereits doppelte Werte enthalten, nicht zu. Dies ist unabhängig davon, ob IGNORE_DUP_KEY auf ON festgelegt ist. Wenn Sie dies versuchen, wird in [!INCLUDE[ssDE](../../includes/ssde-md.md)] eine Fehlermeldung angezeigt. Doppelte Werte müssen entfernt werden, bevor ein eindeutiger Index für die Spalte oder Spalten erstellt werden kann. In einem eindeutigen Index verwendete Spalten müssen auf NOT NULL festgelegt werden, da mehrere NULL-Werte beim Erstellen eines eindeutigen Index als Duplikate angesehen werden.

CLUSTERED      
Erstellt einen Index, in dem die logische Reihenfolge der Schlüsselwerte die physische Reihenfolge der entsprechenden Zeilen in einer Tabelle bestimmt. Die unterste Ebene oder Blattebene des gruppierten Indexes enthält die eigentlichen Datenzeilen der Tabelle. Eine Tabelle oder Sicht kann nur über jeweils einen gruppierten Index verfügen.

Eine Sicht mit einem eindeutigen gruppierten Index wird als indizierte Sicht bezeichnet. Durch das Erstellen eines eindeutigen gruppierten Index für eine Sicht wird die Sicht physisch materialisiert. Ein eindeutiger gruppierter Index muss für eine Sicht erstellt werden, bevor ein anderer Index für dieselbe Sicht definiert werden kann. Weitere Informationen finden Sie unter [Erstellen von indizierten Sichten](../../relational-databases/views/create-indexed-views.md).

Erstellen Sie den gruppierten Index, bevor Sie irgendeinen nicht gruppierten Index erstellen. Für eine Tabelle vorhandene nicht gruppierte Indizes werden neu erstellt, wenn ein gruppierter Index erstellt wird.

Ist `CLUSTERED` nicht angegeben, wird ein nicht gruppierter Index erstellt.

> [!NOTE]
> Da die Blattebene eines gruppierten Index und seine Datenseiten per Definition identisch sind, bewirkt das Erstellen eines gruppierten Index und das Verwenden der ON *partition_scheme_name*- oder ON *filegroup_name*-Klausel effektiv, dass eine Tabelle aus der Dateigruppe, in der die Tabelle erstellt wurde, in das neue Partitionsschema oder die neue Dateigruppe verschoben wird. Bevor Sie Tabellen oder Indizes für bestimmte Dateigruppen erstellen, überprüfen Sie, welche Dateigruppen verfügbar sind, und stellen Sie sicher, dass in ihnen ausreichend freier Speicherplatz für den Index vorhanden ist.

In einigen Fällen können durch das Erstellen eines gruppierten Indexes zuvor deaktivierte Indizes aktiviert werden. Weitere Informationen finden Sie unter [Aktivieren von Indizes und Einschränkungen](../../relational-databases/indexes/enable-indexes-and-constraints.md) und [Deaktivieren von Indizes und Einschränkungen](../../relational-databases/indexes/disable-indexes-and-constraints.md).

NONCLUSTERED      
Erstellt einen Index, der die logische Reihenfolge einer Tabelle angibt. Bei einem nicht gruppierten Index ist die physische Reihenfolge der Datenzeilen unabhängig von deren indizierter Reihenfolge.

Jede Tabelle kann über bis zu 999 nicht gruppierte Indizes verfügen, unabhängig davon, wie die Indizes erstellt werden: implizit mit PRIMARY KEY- und UNIQUE-Einschränkungen oder explizit mit CREATE INDEX.

Für indizierte Sichten können nicht gruppierte Indizes nur erstellt werden, wenn bereits ein eindeutiger gruppierter Index für die entsprechende Sicht definiert ist.

Wenn nicht anders angegeben, ist NONCLUSTERED der Standardindextyp.

*index_name*      
 Der Name des Indexes. Indexnamen müssen für eine Tabelle oder Ansicht eindeutig sein, können aber innerhalb einer Datenbank mehrfach vorkommen. Indexnamen müssen den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen.

*column*      
 Gibt die Spalte(n) an, auf der bzw. denen der Index basiert. Geben Sie zwei oder mehr Spaltennamen an, um einen zusammengesetzten Index für die kombinierten Werte der angegebenen Spalten zu erstellen. Führen Sie die Spalten, die im zusammengesetzten Index enthalten sein sollen, in der Reihenfolge der Sortierpriorität in den Klammern hinter *table_or_view_name* auf.

Es können bis zu 32 Spalten in einem einzigen zusammengesetzten Indexschlüssel kombiniert werden. Alle Spalten in einem zusammengesetzten Indexschlüssel müssen sich in derselben Tabelle oder Sicht befinden. Die maximal zulässige Größe der kombinierten Indexwerte liegt bei 900 Byte für gruppierte Indizes oder 1700 Byte für nicht gruppierte Indizes. Für Versionen vor [!INCLUDE[ssSDS](../../includes/sssds-md.md)] und [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] liegen die Höchstwerte bei 16 Spalten und 900 Byte.

Spalten, die von den Large Object-Datentypen (LOB-Datentyp) **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **xml** oder **image** sind, können nicht als Schlüsselspalten für einen Index angegeben werden. Darüber hinaus darf eine Sichtdefinition keine Spalten der Datentypen **ntext**, **text** oder **image** enthalten, auch wenn in der CREATE INDEX-Anweisung nicht auf diese Spalten verwiesen wird.

Sie können Indizes für Spalten mit dem CLR-benutzerdefinierten Typ erstellen, wenn durch den Typ Binärreihenfolgen unterstützt werden. Außerdem können Sie Indizes für berechnete Spalten erstellen, die als Methodenaufrufe aus einer Spalte mit dem benutzerdefinierten Typ definiert sind, vorausgesetzt, die Methoden sind als deterministisch markiert und führen keine Datenzugriffe durch. Weitere Informationen zum Indizieren von Spalten mit dem CLR-benutzerdefinierten Typ finden Sie unter [CLR-benutzerdefinierte Typen](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).

[ **ASC** | DESC ]      
Bestimmt für die entsprechende Indexspalte die aufsteigende oder absteigende Sortierreihenfolge. Die Standardeinstellung ist ASC.

INCLUDE **(** _column_ [ **,** ... *n* ] **)**       
Gibt die Nichtschlüsselspalten an, die zur Blattebene des nicht gruppierten Index hinzugefügt werden sollen. Der nicht gruppierte Index kann eindeutig oder nicht eindeutig sein.

Die Spaltennamen dürfen in der INCLUDE-Liste nicht wiederholt und nicht gleichzeitig als Schlüssel- und Nichtschlüsselspalten verwendet werden. Nicht gruppierte Indizes enthalten stets die Spalten des gruppierten Index, wenn ein gruppierter Index für die Tabelle definiert ist. Weitere Informationen finden Sie unter [Create Indexes with Included Columns](../../relational-databases/indexes/create-indexes-with-included-columns.md).

Mit Ausnahme von **text**, **ntext** und **image** sind alle Datentypen zulässig. Der Index muss offline erstellt oder neu erstellt werden (ONLINE = OFF), wenn eine der angegebenen Nichtschlüsselspalten den Datentyp **varchar(max)** , **nvarchar(max)** oder **varbinary(max)** aufweist.

Bei berechneten Spalten, die deterministisch und präzise oder unpräzise sind, kann es sich um eingeschlossene Spalten handeln. Berechnete Spalten, die aus den Datentypen **image**, **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** und **xml** abgeleitet wurden, können, solange der Datentyp der berechneten Spalte als Indexschlüsselspalte zulässig ist, in Spalten eingefügt werden, bei denen es sich nicht um Schlüsselspalten handelt. Weitere Informationen finden Sie unter [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md).

Weitere Informationen zum Erstellen eines XML-Index finden Sie unter [CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md).

WHERE \<filter_predicate>      
Erstellt einen gefilterten Index, wobei angegeben wird, welche Zeilen in den Index aufgenommen werden sollen. Der gefilterte Index muss ein nicht gruppierter Index für eine Tabelle sein. Erstellt gefilterte Statistikdaten für die Datenzeilen im gefilterten Index.

Im Filterprädikat werden einfache Vergleichsoperatoren verwendet. Es darf darin nicht auf eine berechnete Spalte, eine UDT-Spalte, eine Spalte mit einem räumlichen Datentyp oder eine Spalten mit dem hierarchyID-Datentyp verwiesen werden. Vergleiche mit NULL-Literalen sind mit den Vergleichsoperatoren nicht zulässig. Verwenden Sie stattdessen den IS NULL-Operator und den IS NOT NULL-Operator.

Es folgen einige Beispiele für Filterprädikate für die `Production.BillOfMaterials`-Tabelle:

```sql
WHERE StartDate > '20000101' AND EndDate <= '20000630'

WHERE ComponentID IN (533, 324, 753)

WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL
```

Gefilterte Indizes gelten nicht für XML-Indizes und Volltextindizes. Bei UNIQUE-Indizes müssen nur die ausgewählten Zeilen über eindeutige Indexwerte verfügen. Bei gefilterten Indizes ist die IGNORE_DUP_KEY-Option nicht zulässig.

ON *partition_scheme_name* **( _column_name_ )**      

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt das Partitionsschema an, das die Dateigruppen definiert, denen die Partitionen eines partitionierten Index zugeordnet werden. Das Partitionsschema muss bereits durch Ausführen von [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) oder [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md) in der Datenbank vorhanden sein. *column_name* gibt die Spalte an, auf deren Grundlage ein partitionierter Index partitioniert wird. Diese Spalte muss mit dem Datentyp, der Länge und der Genauigkeit des Arguments der Partitionsfunktion übereinstimmen, die *partition_scheme_name* verwendet. *column_name* ist nicht auf Spalten in der Indexdefinition beschränkt. Es können beliebige Spalten der Basistabelle angegeben werden, mit der Ausnahme, dass *column_name* beim Partitionieren von UNIQUE-Indizes aus den Spalten ausgewählt werden muss, die als eindeutige Schlüssel verwendet werden. Mit dieser Einschränkung kann [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Eindeutigkeit der Schlüsselwerte in nur einer einzigen Partition überprüfen.

> [!NOTE]
> Beim Partitionieren eines nicht eindeutigen gruppierten Index fügt [!INCLUDE[ssDE](../../includes/ssde-md.md)] standardmäßig die Partitionierungsspalte zu der Liste der gruppierten Indexschlüssel hinzu, sofern sie dort noch nicht angegeben wurde. Beim Partitionieren eines nicht eindeutigen, nicht gruppierten Indexes fügt [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Partitionierungsspalte als (eingeschlossene) Nichtschlüsselspalte des Indexes hinzu, sofern sie noch nicht angegeben wurde.

Wenn _partition_scheme_name_ oder _filegroup_ bei einer partitionierten Tabelle nicht angegeben werden, wird der Index in demselben Partitionsschema platziert und verwendet dieselbe Partitionierungsspalte wie die zugrunde liegende Tabelle.

> [!NOTE]
> Sie können kein Partitionierungsschema für einen XML-Index angeben. Beim Partitionieren der Basistabelle verwendet der XML-Index dasselbe Partitionsschema wie die Tabelle.

Weitere Informationen zur Partitionierung von Indizes finden Sie unter [Partitionierte Tabellen und Indizes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

ON _filegroup_name_      

**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher)

Erstellt den angegebenen Index für die angegebene Dateigruppe. Wenn kein Speicherort angegeben und die Tabelle oder Sicht nicht partitioniert ist, verwendet der Index dieselbe Dateigruppe wie die zugrunde liegende Tabelle oder Sicht. Die Dateigruppe muss bereits vorhanden sein.

ON **"** default **"**      

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Erstellt den angegebenen Index in derselben Dateigruppe oder in demselben Partitionsschema wie die Tabelle oder Sicht.

Die Benennung default ist in diesem Kontext kein Schlüsselwort. Es handelt sich dabei um einen Bezeichner für die Standarddateigruppe. Dieser muss wie in ON **"** default **"** or ON **[** default **]** durch Trennzeichen getrennt werden. Wenn "default" angegeben wird, muss die Option QUOTED_IDENTIFIER für die aktuelle Sitzung auf ON festgelegt sein. Dies ist die Standardeinstellung. Weitere Informationen finden Sie unter [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

> [!NOTE]
> „default“ gibt nicht die Standarddateigruppe der Datenbank im Kontext von CREATE INDEX an. Dies unterscheidet sich von CREATE TABLE, wo „default“ die Tabelle in der Standarddateigruppe der Datenbank platziert.

[ FILESTREAM_ON { *filestream_filegroup_name* | *partition_scheme_name* | "NULL" } ]      

**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher)

Gibt die Platzierung der FILESTREAM-Daten für die Tabelle an, wenn ein gruppierter Index erstellt wird. Die FILESTREAM_ON-Klausel lässt zu, dass FILESTREAM-Daten in eine andere FILESTREAM-Dateigruppe oder ein anderes Partitionsschema verschoben werden.

_filestream_filegroup_name_ ist der Name einer FILESTREAM-Dateigruppe. Für die Dateigruppe muss eine Datei mit einer [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)-Anweisung oder einer [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)-Anweisung definiert worden sein, andernfalls wird ein Fehler ausgelöst.

Wenn die Tabelle partitioniert ist, muss die FILESTREAM_ON-Klausel eingeschlossen werden und ein Partitionsschema von FILESTREAM-Dateigruppen angeben, das die gleiche Partitionsfunktion und die gleichen Partitionsspalten wie das Partitionsschema der Tabelle enthält. Andernfalls wird ein Fehler ausgelöst.

Wenn die Tabelle nicht partitioniert ist, kann die FILESTREAM-Spalte nicht partitioniert werden. Die FILESTREAM-Daten für die Tabelle müssen in einer einzigen Dateigruppe gespeichert werden, die in der FILESTREAM_ON-Klausel angegeben wird.

`FILESTREAM_ON NULL` kann in einer CREATE INDEX-Anweisung angegeben werden, wenn ein gruppierter Index erstellt wird und die Tabelle keine FILESTREAM-Spalte enthält.

Weitere Informationen finden Sie unter [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).

**\<object>::=**      

Gibt das vollqualifizierte oder nicht vollqualifizierte Objekt an, das indiziert werden soll.

_database_name_      
Der Name der Datenbank.

*schema_name*      
Der Name des Schemas, zu dem die Tabelle oder Sicht gehört.

*table_or_view_name*      
Gibt den Namen der zu indizierenden Tabelle oder Sicht an.

Die Sicht muss mit SCHEMABINDING definiert werden, um einen Index für sie zu erstellten. Ein eindeutiger gruppierter Index muss für eine Sicht erstellt werden, bevor ein nicht gruppierter Index erstellt wird. Weitere Informationen zu indizierten Sichten finden Sie im Abschnitt mit den Hinweisen.

Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] kann es sich bei dem Objekt um eine Tabelle handeln, die gemeinsam mit einem gruppierten Columnstore-Index gespeichert wird.

[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] unterstützt das aus drei Teilen bestehende Namensformat _database_name_.[_schema_name_]._object_name_, wenn *database_name* die aktuelle Datenbank bzw. _database_name_`tempdb` ist und _object_name_ mit # beginnt.

**\<relational_index_option\>::=**       
Gibt die Optionen an, die beim Erstellen des Indexes verwendet werden sollen.

PAD_INDEX = { ON | **OFF** }      

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt die Auffüllung von Indizes an. Der Standardwert ist OFF.

EIN      
Der Prozentsatz des mit *fillfactor* angegebenen freien Speicherplatzes wird für die Zwischenebenenseiten des Indexes angewendet.

OFF oder _fillfactor_ ist nicht angegeben      
Die Zwischenebenenseiten sind nahezu vollständig aufgefüllt. Allerdings ist ausreichend Speicherplatz vorhanden, um mindestens eine Zeile in der maximal für den Index möglichen Größe aufzunehmen, wenn der Schlüsselsatz auf den Zwischenseiten berücksichtigt wird.

Die Option PAD_INDEX ist nur dann hilfreich, wenn FILLFACTOR angegeben ist, da PAD_INDEX den durch FILLFACTOR angegebenen Prozentsatz verwendet. Wenn der für FILLFACTOR angegebene Prozentsatz nicht groß genug ist, um eine Zeile aufzunehmen, überschreibt [!INCLUDE[ssDE](../../includes/ssde-md.md)] diesen Prozentsatz intern, um das Minimum zuzulassen. Auf jeder Zwischenindexseite befinden sich unabhängig vom angegebenen _fillfactor_-Wert nie weniger als zwei Zeilen.

In abwärtskompatibler Syntax ist WITH PAD_INDEX gleichwertig mit WITH PAD_INDEX = ON.

FILLFACTOR **=** _fillfactor_      

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt einen Prozentsatz an, der angibt, wie weit das [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Blattebene jeder Indexseite während der Indexerstellung oder -neuerstellung füllen soll. *fillfactor* muss ein ganzzahliger Wert zwischen 1 und 100 sein. Wenn *fillfactor* 100 ist, erstellt das [!INCLUDE[ssDE](../../includes/ssde-md.md)] Indizes mit vollständig aufgefüllten Blattseiten.

Die FILLFACTOR-Einstellung gilt nur, wenn der Index erstellt oder neu erstellt wird. [!INCLUDE[ssDE](../../includes/ssde-md.md)] hält den angegebenen Prozentsatz des Speicherplatzes nicht dynamisch auf den Seiten frei. Zum Anzeigen der Füllfaktoreinstellung verwenden Sie die Katalogsicht [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

> [!IMPORTANT]
> Das Erstellen eines gruppierten Indexes mit einem FILLFACTOR-Wert unter 100 wirkt sich auf den Speicherplatz aus, den die Daten belegen, da [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Daten beim Erstellen des gruppierten Indexes neu verteilt.

Weitere Informationen finden Sie unter [Angeben des Füllfaktors für einen Index](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).

SORT_IN_TEMPDB = { ON | **OFF** }      

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt an, ob temporäre Ergebnisse des Sortierens in **tempdb** gespeichert werden sollen. Außer für Hyperscale von Azure SQL-Datenbank ist der Standardwert OFF. Für alle Indexerstellungsvorgänge in Hyperscale ist SORT_IN_TEMPDB unabhängig von der angegebenen Option immer ON, sofern nicht die fortsetzbare Indexneuerstellung verwendet wird.

EIN      
Die Zwischenergebnisse von Sortierungen, mit denen der Index erstellt wird, werden in **tempdb** gespeichert. Dadurch kann sich die zum Erstellen eines Index erforderliche Zeit verringern, wenn sich **tempdb** in anderen Datenträgersätzen befindet als die Benutzerdatenbank. Sie erhöht jedoch den Betrag an Speicherplatz, der während der Indexerstellung verwendet wird.

OFF      
Die Zwischenergebnisse der Sortierung werden in derselben Datenbank gespeichert wie der Index.

Zusätzlich zu dem Speicherplatz, der in der Benutzerdatenbank zum Erstellen des Index erforderlich ist, muss **tempdb** ungefähr die gleiche Menge an zusätzlichem Speicherplatz aufweisen, um die Zwischenergebnisse der Sortierung zu speichern. Weitere Informationen finden Sie unter [SORT_IN_TEMPDB-Option für Indizes](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).

In abwärtskompatibler Syntax ist WITH SORT_IN_TEMPDB gleichwertig mit WITH SORT_IN_TEMPDB = ON.

IGNORE_DUP_KEY = { ON | **OFF** }      
Gibt die Fehlermeldung an, wenn ein Einfügevorgang versucht, doppelte Schlüsselwerte in einen eindeutigen Index einzufügen. Die IGNORE_DUP_KEY-Option gilt nur für Einfügevorgänge nach dem Erstellen oder Neuerstellen des Index. Beim Ausführen von [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) oder [UPDATE](../../t-sql/queries/update-transact-sql.md) hat die Option keine Auswirkungen. Der Standardwert ist OFF.

EIN      
Eine Warnmeldung wird ausgegeben, wenn doppelte Schlüsselwerte in einen eindeutigen Index eingefügt werden. Es schlagen nur die Zeilen fehl, die gegen die Eindeutigkeitseinschränkung verstoßen.

OFF      
Eine Fehlermeldung wird ausgegeben, wenn doppelte Schlüsselwerte in einen eindeutigen Index eingefügt werden. Für den gesamten INSERT-Vorgang wird ein Rollback ausgeführt.

IGNORE_DUP_KEY kann für Indizes, die für eine Sicht erstellt werden, nicht eindeutige Indizes, XML-Indizes, räumliche und gefilterte Indizes nicht auf ON festgelegt werden.

Um IGNORE_DUP_KEY anzuzeigen, verwenden Sie [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

In abwärtskompatibler Syntax ist WITH IGNORE_DUP_KEY gleichwertig mit WITH IGNORE_DUP_KEY = ON.

STATISTICS_NORECOMPUTE = { ON | **OFF**}      
Gibt an, ob Verteilungsstatistiken neu berechnet werden. Der Standardwert ist OFF.

EIN      
Veraltete Indexstatistiken werden nicht automatisch neu berechnet.

OFF      
Die automatischen Updates der Statistiken sind aktiviert.

Um das automatische Aktualisieren von Statistiken wiederherzustellen, müssen Sie STATISTICS_NORECOMPUTE auf OFF festlegen oder die UPDATE STATISTICS-Anweisung ohne die NORECOMPUTE-Klausel ausführen.

> [!IMPORTANT]
> Wenn Sie die automatische Neuberechnung von Verteilungsstatistiken deaktivieren, wählt der Abfrageoptimierer möglicherweise nicht die optimalen Ausführungspläne für Abfragen, an denen die Tabelle beteiligt ist.

In abwärtskompatibler Syntax ist WITH STATISTICS_NORECOMPUTE gleichwertig mit WITH STATISTICS_NORECOMPUTE = ON.

STATISTICS_INCREMENTAL = { ON | **OFF** }     

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Bei **ON** wird die Statistik pro Partition erstellt. Bei **OFF** wird die Statistikstruktur gelöscht und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] berechnet die Statistiken erneut. Der Standardwert ist **OFF**.

Wenn Statistiken pro Partition nicht unterstützt werden, wird die Option ignoriert und eine Warnung generiert. Inkrementelle Statistiken werden für folgende Statistiktypen nicht unterstützt:

- Statistiken, die mit Indizes erstellt wurden, die über keine Partitionsausrichtung mit der Basistabelle verfügen.
- Statistiken, die für lesbare sekundäre Always On-Datenbanken erstellt wurden.
- Statistiken, die für schreibgeschützte Datenbanken erstellt wurden.
- Statistiken, die für gefilterte Indizes erstellt wurden.
- Statistiken, die für Sichten erstellt wurden.
- Statistiken, die für interne Tabellen erstellt wurden.
- Statistiken, die mit räumlichen Indizes oder XML-Indizes erstellt wurden.

DROP_EXISTING = { ON | **OFF** }      
Eine Option zum Entfernen und erneutem Erstellen eines vorhandenen gruppierten oder nicht gruppierten Index mit veränderten Spaltenspezifikationen, die den Namen für den Index beibehält. Der Standardwert ist OFF.

EIN      
Gibt an, dass der vorhandene Index entfernt und neu erstellt werden soll. Der Index muss über denselben Namen wie der Parameter *index_name* verfügen.

OFF      
Gibt an, dass der vorhandene Index nicht entfernt und neu erstellt werden soll. SQL Server zeigt einen Fehler an, wenn der angegebene Indexname bereits vorhanden ist.

Mit DROP_EXISTING können Sie folgende Änderung vornehmen:

- Umwandlung eines nicht gruppierten Rowstore-Index in einen gruppierten Rowstore-Index.

Mit DROP_EXISTING können Sie folgende Änderung nicht vornehmen:

- Umwandlung eines gruppierten Rowstore-Index in einen nicht gruppierten Rowstore-Index.
- Umwandlung eines gruppierten Columnstore-Index in einen nicht gruppierten Rowstore-Index.

In abwärtskompatibler Syntax ist WITH DROP_EXISTING gleichwertig mit WITH DROP_EXISTING = ON.

ONLINE = { ON | **OFF** }      
Gibt an, ob die zugrunde liegenden Tabellen und zugeordneten Indizes für Abfragen und Datenänderungen während des Indexvorgangs verfügbar sind. Der Standardwert ist OFF.

> [!IMPORTANT]
> Onlineindexvorgänge sind nicht in jeder Edition von [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verfügbar. Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Editionen und unterstütze Funktionen für den SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).

EIN      
Lang andauernde Sperren werden nicht für die Dauer des Indexvorgangs aufrechterhalten. Während der Hauptphase des Indexvorgangs wird nur eine beabsichtigte freigegebene Sperre für die Quelltabelle aufrechterhalten. Dadurch können Abfragen oder Updates für die zugrunde liegende Tabelle und die Indizes fortgesetzt werden. Zu Beginn des Vorgangs wird für sehr kurze Zeit eine freigegebene Sperre (S) für das Quellobjekt aufrechterhalten. Am Ende des Vorgangs wird für die Quelle für kurze Zeit eine freigegebene Sperre (S) aktiviert, wenn ein nicht gruppierter Index erstellt wird. Eine Schemaänderungssperre (SCH-M) wird aktiviert, wenn ein gruppierter Index online erstellt oder gelöscht und wenn ein gruppierter oder nicht gruppierter Index neu erstellt wird. ONLINE kann nicht auf ON festgelegt werden, wenn ein Index auf einer lokalen temporären Tabelle erstellt wird.

OFF      
Die Tabellensperren werden für die Dauer des Indexvorgangs angewendet. Ein Offlineindexvorgang, bei dem ein gruppierter Index erstellt, neu erstellt oder gelöscht bzw. ein nicht gruppierter Index neu erstellt oder gelöscht wird, aktiviert eine Schemaänderungssperre (SCH-M) für die Tabelle. Dadurch wird verhindert, dass Benutzer für die Dauer des Vorgangs auf die zugrunde liegende Tabelle zugreifen können. Ein Offlineindexvorgang, bei dem ein nicht gruppierter Index erstellt wird, aktiviert eine freigegebene Sperre (S) für die Tabelle. Dadurch werden Updates der zugrunde liegenden Tabelle verhindert. Lesevorgänge, wie SELECT-Anweisungen, sind jedoch zulässig.

Weitere Informationen finden Sie unter [Ausführen von Onlineindexvorgängen](../../relational-databases/indexes/perform-index-operations-online.md) .  

Indizes, einschließlich Indizes globaler temporärer Tabellen, können online erstellt werden. Es gelten folgende Ausnahmen:

- XML-Index
- Index für eine lokale temp-Tabelle
- Eindeutiger gruppierter Ausgangsindex für eine Sicht
- Deaktivierte gruppierte Indizes
- Columnstore-Indizes
- Gruppierter Index, wenn die zugrunde liegende Tabelle LOB-Datentypen (**image**, **ntext**, **text**) und räumliche Datentypen enthält.
- **varchar(max)** - und **varbinary(max)** -Spalten können nicht Teil eines Index sein. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] kann ein gruppierter Index mit der Option **ONLINE** erstellt oder neu erstellt werden, wenn eine Tabelle Spalten vom Typ **varchar(max)** oder **varbinary(max)** enthält. [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] lässt die **ONLINE**-Option nicht zu, wenn die Basistabelle Spalten vom Typ **varchar(max)** oder **varbinary(max)** enthält.

Weitere Informationen finden Sie unter [Funktionsweise von Onlineindexvorgängen](../../relational-databases/indexes/how-online-index-operations-work.md).

RESUMABLE **=** { ON | **OFF**}      

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

 Gibt an, ob ein Onlineindexvorgang fortsetzbar ist.

 EIN      
Der Indexvorgang ist fortsetzbar.

 OFF      
Der Indexvorgang ist nicht fortsetzbar.

MAX_DURATION **=** *time* [**MINUTES**] verwendet mit **RESUMABLE = ON** (erfordert **ONLINE = ON**)   

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt die Zeitspanne an (als ganzzahligen Wert in Minuten), in der ein fortsetzbarer Onlineindexvorgang ausgeführt wird, bevor er angehalten wird.

> [!IMPORTANT]
> Ausführlichere Informationen zu Indexvorgängen, die online ausgeführt werden können, finden Sie unter [Richtlinien für Onlineindexvorgänge](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

> [!NOTE] 
> Fortsetzbare Neuerstellungen von Onlineindizes werden für Columnstore-Indizes nicht unterstützt.

ALLOW_ROW_LOCKS = { **ON** | OFF }      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt an, ob Zeilensperren zulässig sind. Der Standardwert ist ON.

EIN      
Zeilensperren sind beim Zugriff auf den Index zulässig. Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] bestimmt, wann Zeilensperren verwendet werden.

OFF      
Zeilensperren werden nicht verwendet.

ALLOW_PAGE_LOCKS = { **ON** | OFF }      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt an, ob Seitensperren zulässig sind. Der Standardwert ist ON.

EIN      
Seitensperren sind beim Zugriff auf den Index zulässig. Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] bestimmt, wann Seitensperren verwendet werden.

OFF      
Seitensperren werden nicht verwendet.

OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | **OFF** }      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt an, ob der Konflikt beim Einfügen der letzten Seite optimiert werden soll. Der Standardwert ist OFF. Weitere Informationen finden Sie im Abschnitt [Sequenzielle Schlüssel](#sequential-keys).

MAXDOP = *max_degree_of_parallelism*      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Überschreibt die Konfigurationsoption **max degree of parallelism** (Max. Grad an Parallelität) für die Dauer des Indexvorgangs. Weitere Informationen finden Sie unter [Konfigurieren der Serverkonfigurationsoption Max. Grad an Parallelität](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Sie können mit MAXDOP die Anzahl der Prozessoren begrenzen, die bei der Ausführung paralleler Pläne verwendet werden. Maximal sind 64 Prozessoren zulässig.

*max_degree_of_parallelism* kann folgende Werte haben:

1      
Unterdrückt das Generieren paralleler Pläne.

\>1      
Beschränkt die maximale Anzahl der Prozessoren, die bei einem parallelen Indexvorgang verwendet werden, je nach aktueller Systemauslastung auf die angegebene Zahl oder einen niedrigeren Wert.

0 (Standard)      
Verwendet abhängig von der aktuellen Systemarbeitsauslastung die tatsächliche Anzahl von Prozessoren oder weniger Prozessoren.

 Weitere Informationen finden Sie unter [Konfigurieren von Parallelindexvorgängen](../../relational-databases/indexes/configure-parallel-index-operations.md).

> [!NOTE]
> Parallele Indexvorgänge sind nicht in jeder Edition von [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verfügbar. Eine Liste der Features, die von den Editionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt werden, finden Sie unter [Editionen und unterstütze Features für SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) und [Editionen und unterstützte Features von SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

DATA_COMPRESSION      
Gibt die Datenkomprimierungsoption für den angegebenen Index, die Partitionsnummer oder den Bereich von Partitionen an. Die folgenden Optionen sind verfügbar:

Keine      
Der Index oder die angegebenen Partitionen werden nicht komprimiert.

ROW      
Der Index oder die angegebenen Partitionen werden mit Zeilenkomprimierung komprimiert.

PAGE      
Der Index oder die angegebenen Partitionen werden mit Seitenkomprimierung komprimiert.

Weitere Informationen zur Datenkomprimierung finden Sie unter [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md).

ON PARTITIONS **(** { \<partition_number_expression> | \<range> } [ **,** ..._n_ ] **)**       
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt die Partitionen an, für die die DATA_COMPRESSION-Einstellung gilt. Wenn der Index nicht partitioniert ist, erzeugt das ON PARTITIONS-Argument einen Fehler. Wenn die ON PARTITIONS-Klausel nicht angegeben wird, gilt die DATA_COMPRESSION-Option für alle Partitionen eines partitionierten Indexes.

\<partition_number_expression> kann wie folgt angegeben werden:

- Geben Sie die Nummer der Partition an, beispielsweise: ON PARTITIONS (2).
- Geben Sie die Partitionsnummern mehrerer einzelner Partitionen durch Kommas getrennt an, beispielsweise: ON PARTITIONS (1, 5).
- Geben Sie sowohl Bereiche als auch einzelne Partitionen an, beispielsweise: ON PARTITIONS (2, 4, 6 TO 8).

Für \<range> können durch das Wort „TO“ getrennte Partitionsnummern angegeben werden, z. B. `ON PARTITIONS (6 TO 8)`.

 Wenn Sie für verschiedene Partitionen unterschiedliche Datenkomprimierungstypen festlegen möchten, geben Sie die Option DATA_COMPRESSION mehrmals an, beispielsweise:

```sql
REBUILD WITH
(
  DATA_COMPRESSION = NONE ON PARTITIONS (1),
  DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),
  DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)
);
```

## <a name="remarks"></a>Bemerkungen
Die CREATE INDEX-Anweisung wird so optimiert wie jede andere Abfrage. Um weniger E/A-Vorgänge zu benötigen, entscheidet der Abfrageprozessor möglicherweise, einen anderen Index zu scannen, statt einen Tabellenscan auszuführen. Der Sortiervorgang wird in einigen Situationen möglicherweise umgangen. Auf einem Multiprozessorcomputer kann CREATE INDEX mehr Prozessoren verwenden, um die mit dem Erstellen des Index zusammenhängenden Scan- und Sortiervorgänge auszuführen. Dies geschieht in gleicher Weise wie für andere Abfragen. Weitere Informationen finden Sie unter [Konfigurieren von Parallelindexvorgängen](../../relational-databases/indexes/configure-parallel-index-operations.md).

Der Indexerstellungsvorgang kann minimal protokolliert werden, wenn das Wiederherstellungsmodell der Datenbank auf die massenprotokollierte oder einfache Wiederherstellung festgelegt ist.

Indizes können für temporäre Tabellen erstellt werden. Wenn die Tabelle gelöscht oder die Sitzung beendet wird, werden die Indizes gelöscht.

Ein gruppierter Index kann auf einer Tabellenvariable erstellt werden, wenn ein Primärschlüssel generiert wird. Wenn die Abfrage abgeschlossen wird oder die Sitzung beendet wird, wird der Index gelöscht.

Durch Indizes werden erweiterte Eigenschaften unterstützt.

## <a name="clustered-indexes"></a>Gruppierte Indizes
Für das Erstellen eines gruppierten Index für eine Tabelle (Heap) oder das Löschen und Neuerstellen eines vorhandenen gruppierten Index muss zusätzlicher Arbeitsbereich in der Datenbank verfügbar sein, um das Sortieren von Daten und das Speichern einer temporären Kopie der ursprünglichen Tabelle oder von vorhandenen gruppierten Indexdaten zu ermöglichen. Weitere Informationen zu gruppierten Indizes finden Sie unter [Erstellen gruppierter Indizes](../../relational-databases/indexes/create-clustered-indexes.md) und im [Leitfaden zur Architektur und zum Design von SQL Server-Indizes](../../relational-databases/sql-server-index-design-guide.md).

## <a name="nonclustered-indexes"></a>Nicht gruppierte Indizes
Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] können Sie nicht gruppierte Indizes für eine Tabelle erstellen, die als gruppierter Columnstore-Index gespeichert wurde. Wenn Sie zuerst einen nicht gruppierten Index für eine Tabelle erstellen, die als Head oder gruppierter Index gespeichert ist, bleibt der Index erhalten, wenn Sie später die Tabelle in einen gruppierten Columnstore-Index konvertieren. Außerdem ist es nicht notwendig, den nicht gruppierten Index zu löschen, wenn Sie den gruppierten Columnstore-Index neu erstellen.

Einschränkungen:

- Die `FILESTREAM_ON`-Option ist nicht gültig, wenn Sie einen nicht gruppierten Index für eine Tabelle erstellen, die als gruppierter Columnstore-Index gespeichert wurde.

## <a name="unique-indexes"></a>Eindeutige Indizes
Wenn ein eindeutiger Index vorhanden ist, prüft [!INCLUDE[ssDE](../../includes/ssde-md.md)] jedes Mal, wenn Daten mithilfe von Einfügevorgängen hinzugefügt werden, auf doppelte Werte. Für Einfügevorgänge, die doppelte Schlüsselwerte generieren würden, wird ein Rollback ausgeführt. In [!INCLUDE[ssDE](../../includes/ssde-md.md)] wird in diesem Fall eine Fehlermeldung angezeigt. Dies trifft auch dann zu, wenn beim Einfügevorgang viele Zeilen geändert werden, aber nur ein doppelter Wert verursacht wird. Wenn versucht wird, Daten einzugeben, für die ein eindeutiger Index vorhanden ist, und die IGNORE_DUP_KEY-Klausel auf ON festgelegt ist, schlagen nur die Zeilen fehl, die den UNIQUE-Index verletzen.

## <a name="partitioned-indexes"></a>Partitionierte Indizes
Partitionierte Indizes werden ähnlich wie partitionierte Tabellen erstellt und verwaltet. Aber wie gewöhnliche Indizes werden sie wie separate Datenobjekte behandelt. Sie können einen partitionierten Index für eine nicht partitionierte Tabelle erstellen, und Sie können einen nicht partitionierten Index für eine partitionierte Tabelle erstellen.

Wenn Sie einen Index für eine partitionierte Tabelle erstellen und keine Dateigruppe angeben, in die der Index platziert werden soll, wird der Index auf die gleiche Weise partitioniert wie die zugrunde liegende Tabelle. Der Grund hierfür ist, dass Indizes standardmäßig in dieselben Dateigruppen wie die zugrunde liegenden Tabellen platziert werden. Bei partitionierten Tabellen werden Indizes in dasselbe Partitionsschema platziert, das dieselben Partitionierungsspalten verwendet. Wenn der Index das gleiche Partitionsschema und die gleiche Partitionierungsspalte wie die Tabelle verwendet, wird der Index auf die Tabelle *ausgerichtet*.

> [!WARNING]
> Das Erstellen bzw. Neuerstellen von nicht ausgerichteten Indizes für eine Tabelle mit mehr als 1.000 Partitionen ist möglich, wird aber nicht unterstützt. Dies hätte Leistungseinbußen oder eine zu hohe Speicherauslastung während der Vorgänge zur Folge. Es empfiehlt sich, bei mehr als 1.000 Partitionen nur ausgerichtete Indizes zu verwenden.

Beim Partitionieren eines nicht eindeutigen gruppierten Index fügt [!INCLUDE[ssDE](../../includes/ssde-md.md)] standardmäßig alle Partitionierungsspalten zu der Liste der gruppierten Indexschlüssel hinzu, sofern sie dort noch nicht angegeben wurden.

Indizierte Sichten können für partitionierte Tabellen auf die gleiche Weise wie Indizes für Tabellen erstellt werden. Weitere Informationen zu gruppierten Indizes finden Sie unter [Partitionierte Tabellen und Indizes](../../relational-databases/partitions/partitioned-tables-and-indexes.md) und im [Leitfaden zur Architektur und zum Design von SQL Server-Indizes](../../relational-databases/sql-server-index-design-guide.md).

In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] werden Statistiken nicht durch das Scannen aller Zeilen in der Tabelle erstellt, wenn ein partitionierter Index erstellt oder neu erstellt wird. Der Abfrageoptimierer generiert stattdessen Statistiken mithilfe des Standardalgorithmus zur Stichprobenentnahme. Um Statistiken zu partitionierten Indizes durch das Scannen aller Zeilen in der Tabelle abzurufen, verwenden Sie `CREATE STATISTICS` oder `UPDATE STATISTICS` mit der `FULLSCAN`-Klausel.

## <a name="filtered-indexes"></a>Gefilterten Indizes
Ein gefilterter Index ist ein optimierter nicht gruppierter Index, der sich für Abfragen eignet, mit denen ein kleiner Prozentsatz von Zeilen in einer Tabelle ausgewählt wird. Es wird ein Filterprädikat verwendet, um einen Teil der Daten in der Tabelle zu indizieren. Ein gut entworfener gefilterter Index kann die Abfrageleistung verbessern, den Speicheraufwand verringern und Wartungskosten reduzieren.

### <a name="required-set-options-for-filtered-indexes"></a>Erforderliche SET-Optionen für gefilterte Indizes

Die SET-Optionen in der Spalte Erforderlicher Wert sind immer dann erforderlich, wenn eine der folgenden Bedingungen auftritt:

- Es wird ein gefilterter Index erstellt.
- Ein INSERT-, UPDATE-, DELETE- oder MERGE-Vorgang ändert die Daten in einem gefilterten Index.
- Der gefilterte Index wird vom Abfrageoptimierer verwendet, um den Abfrageplan zu erstellen.

    |SET-Optionen|Erforderlicher Wert|Standardserverwert|Standard<br /><br /> OLE DB- und ODBC-Wert|Standard<br /><br /> DB-Library-Wert|
    |-----------------|--------------------|--------------------------|---------------------------------------|-----------------------------------|
    |ANSI_NULLS|EIN|EIN|EIN|OFF|
    |ANSI_PADDING|EIN|EIN|EIN|OFF|
    |ANSI_WARNINGS*|EIN|EIN|EIN|OFF|
    |ARITHABORT|EIN|EIN|OFF|OFF|
    |CONCAT_NULL_YIELDS_NULL|EIN|EIN|EIN|OFF|
    |NUMERIC_ROUNDABORT|OFF|OFF|OFF|OFF|
    |QUOTED_IDENTIFIER|EIN|EIN|EIN|OFF|
  
     * Durch Festlegen von ANSI_WARNINGS auf ON wird implizit ARITHABORT auf ON festgelegt, wenn der Kompatibilitätsgrad der Datenbank auf 90 oder höher festgelegt ist. Wird der Kompatibilitätsgrad der Datenbank auf 80 oder niedriger festgelegt, muss die ARITHABORT-Option explizit auf ON festgelegt werden.

Wenn die SET-Optionen falsch sind, können die folgenden Bedingungen auftreten:

- Der gefilterte Index wird nicht erstellt.
- [!INCLUDE[ssDE](../../includes/ssde-md.md)] generiert einen Fehler und führt ein Rollback aller INSERT-, UPDATE-, DELETE- oder MERGE-Anweisungen aus, die im Index gespeicherte Daten ändern.
- Der Abfrageoptimierer berücksichtigt den Index des Abfrageausführungsplans von Transact-SQL-Anweisungen nicht.

 Weitere Informationen zu gruppierten Indizes finden Sie unter [Erstellen gefilterter Indizes](../../relational-databases/indexes/create-filtered-indexes.md) und im [Leitfaden zur Architektur und zum Design von SQL Server-Indizes](../../relational-databases/sql-server-index-design-guide.md).

## <a name="spatial-indexes"></a>Räumliche Indizes
Weitere Informationen zu räumlichen Indizes finden Sie unter [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md) und [Übersicht über räumliche Indizes](../../relational-databases/spatial/spatial-indexes-overview.md).

## <a name="xml-indexes"></a>XML-Indizes
Weitere Informationen zu XML-Indizes finden Sie unter [CREATE XML INDEX;](../../t-sql/statements/create-xml-index-transact-sql.md) und [XML-Indizes &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md).

## <a name="index-key-size"></a>Indexschlüsselgröße
Die maximale Größe für einen Indexschlüssel beträgt 900 Byte für gruppierte Indizes und 1700 Byte für nicht gruppierte Indizes. (Vor [!INCLUDE[ssSDS](../../includes/sssds-md.md)] und [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] lag die maximale Größe bei 900 Byte.) Indizes für Spalten des Datentyps **varchar**, deren Größe das Limit überschreitet, können erstellt werden, wenn die in den Spalten vorhandenen Daten zum Zeitpunkt der Indexerstellung das Größenlimit nicht überschreiten. Allerdings treten bei nachfolgenden Einfüge- oder Updateaktionen für die Spalten Fehler auf, wenn dadurch das Limit überschritten wird. Der Indexschlüssel eines gruppierten Indexes kann keine Spalten des Datentyps **varchar** enthalten, bei denen Daten in der Zuordnungseinheit ROW_OVERFLOW_DATA vorhanden sind. Wird ein gruppierter Index für eine **varchar**-Spalte erstellt, bei der in der Zuordnungseinheit IN_ROW_DATA Daten vorhanden sind, erzeugen alle nachfolgenden Einfügungen und Updates der Spalte einen Fehler, bei der diese Daten aus der Zeile entfernt werden.

Nicht gruppierte Indizes können Nichtschlüsselspalten auf der Blattebene des Indexes enthalten. Diese Spalten werden von [!INCLUDE[ssDE](../../includes/ssde-md.md)] beim Berechnen der Indexschlüsselgröße nicht berücksichtigt. Weitere Informationen finden Sie unter [Erstellen von Indizes mit eingeschlossenen Spalten](../../relational-databases/indexes/create-indexes-with-included-columns.md) und im [Leitfaden zur Architektur und zum Design von SQL Server-Indizes](../../relational-databases/sql-server-index-design-guide.md).

> [!NOTE]
> Falls beim Partitionieren der Tabellen die Partitionierungsschlüsselspalten nicht bereits in einem nicht eindeutigen gruppierten Index vorhanden sind, werden Sie mithilfe von [!INCLUDE[ssDE](../../includes/ssde-md.md)] dem Index hinzugefügt. Die kombinierte Größe der indizierten Spalten (eingeschlossene Spalten werden nicht gezählt), zzgl. beliebiger hinzugefügter Partitionierungsspalten dürfen 1800 Byte in einem nicht eindeutigen gruppierten Index nicht übersteigen.

## <a name="computed-columns"></a>Berechnete Spalten
Indizes können für berechnete Spalten erstellt werden. Zudem können berechnete Spalten die Eigenschaft PERSISTED besitzen. Das bedeutet, dass [!INCLUDE[ssDE](../../includes/ssde-md.md)] die berechneten Werte in der Tabelle speichert und sie aktualisiert, wenn andere Spalten, von denen die berechnete Spalte abhängt, aktualisiert werden. [!INCLUDE[ssDE](../../includes/ssde-md.md)] verwendet diese persistenten Werte, wenn ein Index für die Spalte erstellt wird und wenn in einer Abfrage auf den Index verwiesen wird.

Zum Indizieren einer berechneten Spalte muss diese deterministisch und präzise sein. Allerdings wird mithilfe der PERSISTED-Eigenschaft der Typ der indizierbaren berechneten Spalten um Folgendes erweitert:

- Auf [!INCLUDE[tsql](../../includes/tsql-md.md)] und CLR-Funktionen basierte berechnete Spalten und Methoden des CLR-benutzerdefinierten Typs, die vom Benutzer als deterministisch markiert sind.
- Berechnete Spalten, die auf Ausdrücken basieren, die gemäß der Definition von [!INCLUDE[ssDE](../../includes/ssde-md.md)] deterministisch, aber unpräzise sind.

Für persistente berechnete Spalten müssen die folgenden SET-Optionen wie im vorherigen Abschnitt [Erforderliche SET-Optionen für gefilterte Indizes](#required-set-options-for-filtered-indexes) gezeigt festgelegt werden.

Die jeweilige UNIQUE- oder PRIMARY KEY-Einschränkung kann eine berechnete Spalte enthalten, sofern diese alle Bedingungen für das Indizieren erfüllt. Die berechnete Spalte muss insbesondere deterministisch und präzise oder deterministisch und persistent sein. Weitere Informationen zu deterministischen Funktionen finden Sie unter [Deterministische und nicht deterministische Funktionen](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).

Berechnete Spalten, die aus den Datentypen **image**, **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** und **xml** abgeleitet wurden, können entweder als Schlüssel oder eingefügte Spalte indiziert werden, bei der es sich nicht um eine Schlüsselspalte handelt, solange der Datentyp der berechneten Spalte als Indexschlüsselspalte oder Nichtschlüsselspalte zulässig ist. Sie können beispielsweise keinen primären XML-Index für eine berechnete **xml**-Spalte erstellen. Wenn der Indexschlüssel die zulässige Größe von 900 Byte überschreitet, wird eine Warnmeldung angezeigt.

Das Erstellen eines Indexes für eine berechnete Spalte kann bei einem Einfüge- oder Updatevorgang einen Fehler erzeugen, wenn der Einfüge- oder Updatevorgang zuvor funktioniert hat. Ein solcher Fehler tritt möglicherweise auf, wenn die berechnete Spalte einen arithmetischen Fehler zur Folge hat. In der folgenden Tabelle wird zum Beispiel die INSERT-Anweisung erfolgreich ausgeführt, obwohl die berechnete Spalte `c` einen arithmetischen Fehler zur Folge hat.

```sql
CREATE TABLE t1 (a INT, b INT, c AS a/b);
INSERT INTO t1 VALUES (1, 0);
```

Wenn Sie allerdings nach dem Erstellen der Tabelle einen Index für die berechnete Spalte `c` erstellen, meldet die gleiche `INSERT`-Anweisung einen Fehler.

```sql
CREATE TABLE t1 (a INT, b INT, c AS a/b);
CREATE UNIQUE CLUSTERED INDEX Idx1 ON t1(c);
INSERT INTO t1 VALUES (1, 0);
```

Weitere Informationen finden Sie unter [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md).

## <a name="included-columns-in-indexes"></a>Eingeschlossene Spalten in Indizes
Nichtschlüsselspalten werden als eingeschlossene Spalten bezeichnet und können zur Blattebene eines nicht gruppierten Index hinzugefügt werden, um die Abfrageleistung durch Abdecken der Abfrage zu verbessern. Das heißt, alle Spalten, auf die in der Abfrage verwiesen wird, sind im Index als Schlüssel- oder Nichtschlüsselspalten enthalten. Dadurch kann der Abfrageoptimierer alle erforderlichen Informationen über einen Indexscan suchen. Es erfolgt kein Zugriff auf die Daten der Tabelle oder des gruppierten Index. Weitere Informationen finden Sie unter [Erstellen von Indizes mit eingeschlossenen Spalten](../../relational-databases/indexes/create-indexes-with-included-columns.md) und im [Leitfaden zur Architektur und zum Design von SQL Server-Indizes](../../relational-databases/sql-server-index-design-guide.md).

## <a name="specifying-index-options"></a>Angeben von Indexoptionen
Mit [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] wurden neue Indexoptionen eingeführt. Außerdem werden die Optionen auf andere Weise angegeben. In der abwärtskompatiblen Syntax ist WITH *option_name* gleichbedeutend mit WITH **(** \<option_name>  **= ON )** . Beim Festlegen von Indexoptionen gelten folgende Regeln:

- Neue Indexoptionen können nur mithilfe von WITH (**_option\_name_ = ON | OFF**) angegeben werden.
- Optionen können nicht mithilfe der abwärtskompatiblen und der neuen Syntax in derselben Anweisung angegeben werden. Wenn Sie beispielsweise WITH (**DROP_EXISTING, ONLINE = ON**) angeben, schlägt die Anweisung fehl.
- Beim Erstellen eines XML-Index müssen die Optionen mithilfe von WITH (**_option_name_= ON | OFF**) angegeben werden.

## <a name="drop_existing-clause"></a>DROP_EXISTING (Klausel)
Mit der DROP_EXISTING-Klausel können Sie den Index neu erstellen, Spalten hinzufügen oder löschen, Optionen ändern, die Sortierreihenfolge für Spalten ändern sowie das Partitionsschema oder die Dateigruppe ändern.

Wenn der Index eine PRIMARY KEY- oder UNIQUE-Einschränkung erzwingt und die Indexdefinition in keiner Weise geändert wurde, wird der Index gelöscht und neu erstellt. Dabei wird die vorhandene Einschränkung beibehalten. Wenn die Indexdefinition jedoch geändert wird, schlägt die Anweisung fehl. Zum Ändern der Definition einer PRIMARY KEY- oder UNIQUE-Einschränkung müssen Sie die Einschränkung löschen und eine Einschränkung mit der neuen Definition hinzufügen.

Die DROP_EXISTING-Klausel erhöht die Leistung beim Neuerstellen eines gruppierten Index (mit der gleichen oder einer anderen Schlüsselmenge) für eine Tabelle, die auch nicht gruppierte Indizes besitzt. Die DROP_EXISTING-Klausel ersetzt die Ausführung einer DROP INDEX-Anweisung für den alten gruppierten Index mit anschließender Ausführung einer CREATE INDEX-Anweisung für den neuen gruppierten Index. Die nicht gruppierten Indizes werden einmal neu erstellt, dies aber nur dann, wenn die Indexdefinition geändert wurde. Die DROP_EXISTING-Klausel erstellt die nicht gruppierten Indizes nicht neu, wenn die Indexdefinition denselben Indexnamen, dieselben Schlüssel- und Partitionsspalten, dasselbe Eindeutigkeitsattribut und dieselbe Sortierreihenfolge wie der ursprüngliche Index aufweist.

Unabhängig davon, ob die nicht gruppierten Indizes neu erstellt werden, verbleiben sie immer in ihren ursprünglichen Dateigruppen oder Partitionsschemas und verwenden die ursprünglichen Partitionsfunktionen. Wenn ein gruppierter Index in einer anderen Dateigruppe oder einem anderen Partitionsschema neu erstellt wird, werden die nicht gruppierten Indizes nicht an den neuen Standort des gruppierten Index verschoben. Daher können auch die nicht gruppierten Indizes, die zuvor mit dem gruppierten Index ausgerichtet waren, möglicherweise nicht mehr damit ausgerichtet sein. Weitere Informationen zu Ausrichtung von partitionierten Indizes finden Sie unter [Partitionierte Tabellen und Indizes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

Die DROP_EXISTING-Klausel sortiert die Daten nicht erneut, wenn dieselben Indexschlüsselspalten in derselben Reihenfolge und mit derselben aufsteigenden oder absteigenden Reihenfolge verwendet werden, es sei denn, in der Indexanweisung ist ein nicht gruppierter Index angegeben und die ONLINE-Option ist auf OFF festgelegt. Wenn der gruppierte Index deaktiviert ist, muss der CREATE INDEX WITH DROP_EXISTING-Vorgang mit der ONLINE-Einstellung OFF durchgeführt werden. Wenn ein nicht gruppierter Index deaktiviert ist und keinem deaktivierten gruppierten Index zugeordnet ist, kann der CREATE INDEX WITH DROP_EXISTING-Vorgang mit der ONLINE-Einstellung OFF oder ON durchgeführt werden.

> [!NOTE]
> Wenn Indizes mit 128 oder mehr Blöcken gelöscht oder neu erstellt werden, verzögert [!INCLUDE[ssDE](../../includes/ssde-md.md)] die eigentlichen Seitenzuordnungsaufhebungen und die zugehörigen Sperren bis zu einem Zeitpunkt nach dem Transaktionscommit.

## <a name="online-option"></a>ONLINE (Option)
Die folgenden Regeln gelten für das Durchführen von Onlineindexvorgängen:

- Die zugrunde liegende Tabelle kann nicht geändert, abgeschnitten oder gelöscht werden, wenn ein Onlineindexvorgang verarbeitet wird.
- Beim Indexvorgang ist zusätzlicher temporärer Speicherplatz erforderlich.
- Onlinevorgänge können für partitionierte Indizes und Indizes durchgeführt werden, die persistente berechnete Spalten oder eingeschlossene Spalten enthalten.

Weitere Informationen finden Sie unter [Ausführen von Onlineindexvorgängen](../../relational-databases/indexes/perform-index-operations-online.md) .

### <a name="resumable-index-operations"></a><a name="resumable-indexes"></a>Fortsetzbare Indexvorgänge
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Die folgenden Richtlinien gelten für fortsetzbare Indexvorgänge:

- Eine Onlineneuerstellung eines Indexes wird mit der `RESUMABLE = ON`-Option als fortsetzbar angegeben.
- Die RESUMABLE-Option wird in den Metadaten nicht für einen bestimmten Index beibehalten und gilt nur für die Dauer der aktuellen DDL-Anweisung. Daher muss die `RESUMABLE = ON`-Klausel explizit angegeben werden, wenn Fortsetzbarkeit aktiviert werden soll.
- Die MAX_DURATION-Option wird nur für die `RESUMABLE = ON`-Option unterstützt.
- MAX_DURATION gibt bei der RESUMABLE-Option das Zeitintervall an, in dem ein Index erstellt wird. Sobald dieses Zeitintervall beendet ist, wird die Indexerstellung entweder angehalten oder in ihrer Ausführung beendet. Der Benutzer entscheidet, wann die Erstellung eines angehaltenen Index fortgesetzt werden kann. Die **Zeitspanne** für MAX_DURATION in Minuten muss größer als 0 Minuten und kleiner oder gleich einer Woche (7 \* 24 \* 60 = 10.080 Minuten) sein. Das lange Anhalten eines Indexvorgangs kann Auswirkungen auf die DML-Leistung für eine bestimmte Tabelle sowie die Datenträgerkapazität der Datenbank haben, da sowohl der ursprüngliche Index als auch der neu erstellte Index Speicherplatz benötigen und während der DML-Vorgänge aktualisiert werden müssen. Wird die MAX_DURATION-Option ausgelassen, dann wird der Indexvorgang bis zum vollständigen Abschluss oder bis ein Fehler auftritt fortgeführt.
- Wenn Sie den Indexvorgang sofort anhalten möchten, können Sie den laufenden Befehl beenden (STRG+C) oder die Befehle [ALTER INDEX](alter-index-transact-sql.md) PAUSE oder `KILL <session_id>` ausführen. Ein angehaltener Befehl kann mit dem Befehl [ALTER INDEX](alter-index-transact-sql.md) fortgesetzt werden.
- Das erneute Ausführen der ursprünglichen CREATE INDEX-Anweisung für den fortsetzbaren Index führt dazu, dass ein angehaltener Indexvorgang automatisch fortgesetzt wird.
- Die Option `SORT_IN_TEMPDB = ON` wird für den fortsetzbaren Index nicht unterstützt.
- Der DDL-Befehl kann mit `RESUMABLE = ON` nicht innerhalb einer expliziten Transaktion ausgeführt werden (kann nicht Teil des Blocks „Begin TRAN ... COMMIT“ sein).
- Zum Fortsetzen bzw. Abbrechen einer Indexerstellung bzw. -neuerstellung verwenden Sie die [ALTER INDEX](alter-index-transact-sql.md)-T-SQL-Syntax.

> [!NOTE]
> Der DDL-Befehl wird so lange ausgeführt, bis er entweder abgeschlossen ist, angehalten wird oder ein Fehler auftritt. Wenn der Befehl angehalten wird, wird ein Fehler ausgelöst, der meldet, dass der Vorgang angehalten wurde und dass die Indexerstellung nicht abgeschlossen wurde. Weitere Informationen zum aktuellen Indexstatus finden Sie unter [sys.index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md). Tritt ein Fehler auf, wird auch hier eine Fehlermeldung ausgegeben.

Informationen zur Anzeige, dass eine Indexerstellung als fortsetzbarer Vorgang ausgeführt wird, und zur Überprüfung des aktuellen Ausführungsstatus finden Sie unter [sys.index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md).

#### <a name="resources"></a>Ressourcen
Die folgenden Ressourcen sind für die Onlineerstellung des fortsetzbaren Indexes erforderlich:

- zusätzlichem Speicherplatz, damit der Index weiter erstellt wird, einschließlich der Zeit, wenn der Index angehalten wird
- Zusätzlicher Protokolldurchsatz während der Sortierungsphase Der insgesamt durch Protokolle belegte Speicherplatz für den fortsetzbaren Index ist geringer als bei der Erstellung eines regulären Onlineindexes. Zudem ist eine Protokollkürzung während dieses Vorgangs möglich.
- DDL-Status zur Verhinderung von DDL-Änderungen
- Ein Cleanup inaktiver Datensätze wird für den Index innerhalb des Builds für die Dauer des Vorgangs blockiert, während dieser angehalten und auch während der Vorgang ausgeführt wird.

#### <a name="current-functional-limitations"></a>Aktuelle funktionale Einschränkungen
Die folgenden Funktionen sind für Vorgänge zur Erstellung fortsetzbarer Indizes deaktiviert:

- Nachdem der Vorgang zum Erstellen eines fortsetzbaren Onlineindexes angehalten wurde, kann der ursprüngliche Wert von MAXDOP nicht mehr geändert werden.
- Die Erstellung eines Indexes, der Folgendes enthält:

  - Berechnete Spalte oder TIMESTAMP-Spalte als Schlüsselspalte
  - LOB-Spalte als enthaltene Spalte für die Erstellung des fortsetzbaren Indexes
  - Gefilterter Index

## <a name="row-and-page-locks-options"></a>Zeilen- und Seitensperren (Optionen)
Wenn `ALLOW_ROW_LOCKS = ON` und `ALLOW_PAGE_LOCK = ON` angegeben sind, sind Sperren auf Zeilen-, Seiten- und Tabellenebene zulässig, wenn auf den Index zugegriffen wird. Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] wählt die geeignete Sperre aus und kann die Sperre von einer Zeilen- oder Seitensperre auf eine Tabellensperre ausweiten.

Wenn `ALLOW_ROW_LOCKS = OFF` und `ALLOW_PAGE_LOCK = OFF` angegeben sind, ist nur eine Sperre auf Tabellenebene zulässig, wenn auf den Index zugegriffen wird.

## <a name="sequential-keys"></a>Sequenzielle Schlüssel
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Das Leistungsproblem von Speicherkonflikten bei Einfügevorgängen für die letzte Seite tritt häufig auf, wenn eine große Anzahl von gleichzeitigen Threads versucht, Zeilen in einen Index mit einem sequentiellen Schlüssel einzufügen. Ein Index wird als sequentiell betrachtet, wenn die führende Schlüsselspalte Werte enthält, die immer größer (oder kleiner) werden, wie beispielsweise eine Identitätsspalte oder ein Datum, das auf das aktuelle Datum/Uhrzeit voreingestellt ist. Da die einzufügenden Schlüssel sequentiell sind, werden alle neuen Zeilen am Ende der Indexstruktur eingefügt – also auf der gleichen Seite. Dies führt zu Konflikten für die Seite im Speicher, die als mehrere Threads betrachtet werden kann, die auf PAGELATCH_EX für die betreffende Seite warten.

Diese Indexoption OPTIMIZE_FOR_SEQUENTIAL_KEY aktiviert eine Optimierung in der Datenbank-Engine, die den Durchsatz für Einfügevorgänge mit hoher Parallelität in den Index verbessert. Dies ist für Indizes vorgesehen, die einen sequenziellen Schlüssel enthalten und bei denen es daher verstärkt zu Speicherkonflikten bei Einfügevorgängen für die letzte Seite kommen kann, kann aber auch für Indizes verwendet werden, die Hotspots in anderen Bereichen des B-Strukturindex aufweisen.

## <a name="viewing-index-information"></a>Anzeigen von Indexinformationen
Informationen zu Indizes können Sie mithilfe von Katalogsichten, Systemfunktionen und gespeicherten Systemprozeduren zurückgeben.

## <a name="data-compression"></a>Datenkomprimierung
Die Datenkomprimierung wird im Artikel [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md) beschrieben. Die folgenden wichtigen Punkte sind zu berücksichtigen:

- Die Komprimierung kann es ermöglichen, dass mehr Zeilen auf einer Seite gespeichert werden, die maximale Zeilengröße wird durch sie allerdings nicht geändert.
- Nicht-Blattseiten eines Index sind nicht seitenkomprimiert, können jedoch zeilenkomprimiert sein.
- Jeder nicht gruppierte Index verfügt über eine eigenen Komprimierungseinstellung und erbt die Komprimierungseinstellung nicht von der zugrunde liegenden Tabelle.
- Wenn ein gruppierter Index auf einem Heap erstellt wird, erbt der gruppierte Index den Komprimierungsstatus des Heaps, sofern kein anderer Komprimierungsstatus angegeben wird.

Für partitionierte Indizes gelten die folgenden Einschränkungen:

- Sie können die Komprimierungseinstellung einer einzelnen Partition nicht ändern, wenn die Tabelle nicht ausgerichtete Indizes aufweist.
- Mit der ALTER INDEX \<index> … REBUILD PARTITION ...-Syntax wird die angegebene Partition des Indexes neu erstellt.
- Mit der ALTER INDEX \<index> … REBUILD WITH ...-Syntax werden alle Partitionen des Indexes neu erstellt.

Mit der gespeicherten Prozedur [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) können Sie einschätzen, wie sich eine Änderung des Komprimierungsstatus auf eine Tabelle, einen Index oder eine Partition auswirkt.

## <a name="permissions"></a>Berechtigungen
Erfordert die `ALTER`-Berechtigung für die Sicht oder die Mitgliedschaft in der festen Datenbankrolle `db_ddladmin`.

## <a name="limitations-and-restrictions"></a>Einschränkungen
In [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] können Sie Folgendes nicht erstellen:

- Einen gruppierten oder nicht gruppierten Rowstore-Index für eine Data Warehouse-Tabelle, wenn ein Columnstore-Index bereits vorhanden ist. Dieses Verhalten unterscheidet sich von SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], da dieser Dienst es zulässt, dass Rowstore- und Columnstore-Indizes beide in derselben vorhanden sind.
- Sie können keinen Index für eine Sicht erstellen.

## <a name="metadata"></a>Metadaten
Informationen zu vorhandenen Indizes erhalten Sie, wenn Sie die [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)-Katalogsicht abfragen.

## <a name="version-notes"></a>Versionshinweise

[!INCLUDE[ssSDS](../../includes/sssds-md.md)] unterstützt die Optionen „Dateigruppe“ und „Filestream“ nicht.

## <a name="examples-all-versions-uses-the-adventureworks-database"></a>Beispiele: Alle Versionen. Verwendet die AdventureWorks-Datenbank.

### <a name="a-create-a-simple-nonclustered-rowstore-index"></a>A. Erstellen eines einfachen nicht gruppierten Rowstore-Index
Im folgenden Beispiel wird ein nicht gruppierter Index für die Spalte `VendorID` der Tabelle `Purchasing.ProductVendor` erstellt.

```sql
CREATE INDEX IX_VendorID ON ProductVendor (VendorID);
CREATE INDEX IX_VendorID ON dbo.ProductVendor (VendorID DESC, Name ASC, Address DESC);
CREATE INDEX IX_VendorID ON Purchasing..ProductVendor (VendorID);
```

### <a name="b-create-a-simple-nonclustered-rowstore-composite-index"></a>B. Erstellen eines einfachen nicht gruppierten zusammengesetzten Rowstore-Index
Im folgenden Beispiel wird ein nicht gruppierter zusammengesetzter Index für die Spalten `SalesQuota` und `SalesYTD` der Tabelle `Sales.SalesPerson` erstellt.

```sql
CREATE NONCLUSTERED INDEX IX_SalesPerson_SalesQuota_SalesYTD ON Sales.SalesPerson (SalesQuota, SalesYTD);
```

### <a name="c-create-an-index-on-a-table-in-another-database"></a>C. Erstellen eines Index für eine Tabelle in einer anderen Datenbank
Im folgenden Beispiel wird ein gruppierter Index für die `VendorID`-Spalte der `ProductVendor`-Tabelle in der `Purchasing`-Datenbank erstellt.

```sql
CREATE CLUSTERED INDEX IX_ProductVendor_VendorID ON Purchasing..ProductVendor (VendorID);
```

### <a name="d-add-a-column-to-an-index"></a>D: Hinzufügen einer Spalte zu einem Index
Im folgenden Beispiel wird der Index X_FF mit zwei Spalten der dbo.FactFinance-Tabelle erstellt. Die nächste Anweisung erstellt den Index mit zwei weiteren Spalten neu und behält den bereits vorhandenen Namen bei.

```sql
CREATE INDEX IX_FF ON dbo.FactFinance (FinanceKey ASC, DateKey ASC);

-- Rebuild and add the OrganizationKey
CREATE INDEX IX_FF ON dbo.FactFinance (FinanceKey, DateKey, OrganizationKey DESC)
  WITH (DROP_EXISTING = ON);
```

## <a name="examples-sql-server-azure-sql-database"></a>Beispiele: SQL Server, Azure SQL-Datenbank

### <a name="e-create-a-unique-nonclustered-index"></a>E. Erstellen eines eindeutigen nicht gruppierten Index
Im folgenden Beispiel wird ein eindeutiger nicht gruppierter Index für die `Name`-Spalte der `Production.UnitMeasure`-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank erstellt. Der Index erzwingt Eindeutigkeit für die Daten, die in die Spalte `Name` eingefügt werden.

```sql
CREATE UNIQUE INDEX AK_UnitMeasure_Name
  ON Production.UnitMeasure(Name);
```

In der folgenden Abfrage wird die Eindeutigkeitseinschränkung getestet, indem eine Zeile mit demselben Wert wie in einer vorhandenen Zeile eingefügt wird.

```sql
-- Verify the existing value.
SELECT Name FROM Production.UnitMeasure WHERE Name = N'Ounces';
GO

INSERT INTO Production.UnitMeasure (UnitMeasureCode, Name, ModifiedDate)
  VALUES ('OC', 'Ounces', GETDATE());
```

Die folgende Fehlermeldung wird angezeigt:

```
Server: Msg 2601, Level 14, State 1, Line 1
Cannot insert duplicate key row in object 'UnitMeasure' with unique index 'AK_UnitMeasure_Name'. The statement has been terminated.
```

### <a name="f-use-the-ignore_dup_key-option"></a>F. Verwenden der IGNORE_DUP_KEY-Option
Das folgende Beispiel veranschaulicht die Wirkung der Option `IGNORE_DUP_KEY`, indem mehrere Zeilen zunächst mit dem Optionswert `ON` und anschließend mit dem Optionswert `OFF` in eine temporäre Tabelle eingefügt werden. Eine einzelne Zeile wird in die `#Test`-Tabelle eingefügt, die absichtlich einen doppelten Wert erzeugt, wenn die zweite mehrzeilige `INSERT`-Anweisung ausgeführt wird. Eine Zählung der Zeilen in der Tabelle gibt die Anzahl der eingefügten Zeilen zurück.

```sql
CREATE TABLE #Test (C1 NVARCHAR(10), C2 NVARCHAR(50), C3 DATETIME);
GO

CREATE UNIQUE INDEX AK_Index ON #Test (C2)
  WITH (IGNORE_DUP_KEY = ON);
GO

INSERT INTO #Test VALUES (N'OC', N'Ounces', GETDATE());
INSERT INTO #Test SELECT * FROM Production.UnitMeasure;
GO

SELECT COUNT(*) AS [Number of rows] FROM #Test;
GO

DROP TABLE #Test;
GO
```

Im Folgenden werden die Ergebnisse der zweiten `INSERT`-Anweisung aufgeführt.

```cmd
Server: Msg 3604, Level 16, State 1, Line 5 Duplicate key was ignored.

Number of rows
--------------
38
```

Beachten Sie, dass die aus der Tabelle `Production.UnitMeasure` eingefügten Zeilen, die die Eindeutigkeitseinschränkung nicht verletzten, erfolgreich eingefügt wurden. Es wurde eine Warnung ausgegeben, und die doppelte Zeile wurde ignoriert, aber es wurde kein Rollback für die gesamte Transaktion ausgeführt.

Dieselben Anweisungen werden erneut ausgeführt. Dabei ist die Option `IGNORE_DUP_KEY` allerdings auf `OFF` festgelegt.

```sql
CREATE TABLE #Test (C1 NVARCHAR(10), C2 NVARCHAR(50), C3 DATETIME);
GO

CREATE UNIQUE INDEX AK_Index ON #Test (C2)
  WITH (IGNORE_DUP_KEY = OFF);
GO

INSERT INTO #Test VALUES (N'OC', N'Ounces', GETDATE());
INSERT INTO #Test SELECT * FROM Production.UnitMeasure;
GO

SELECT COUNT(*) AS [Number of rows] FROM #Test;
GO

DROP TABLE #Test;
GO
```

Im Folgenden werden die Ergebnisse der zweiten `INSERT`-Anweisung aufgeführt.

```
Server: Msg 2601, Level 14, State 1, Line 5
Cannot insert duplicate key row in object '#Test' with unique index
'AK_Index'. The statement has been terminated.

Number of rows
--------------
1
```

Beachten Sie, dass keine Zeilen aus der `Production.UnitMeasure`-Tabelle in die Tabelle eingefügt wurden, obwohl nur eine Zeile in der Tabelle die `UNIQUE`-Einschränkung für den Index verletzte.

### <a name="g-using-drop_existing-to-drop-and-re-create-an-index"></a>G. Verwenden von DROP_EXISTING zum Löschen und Neuerstellen eines Index
Im folgenden Beispiel wird ein vorhandener Index für die `ProductID`-Spalte der `Production.WorkOrder`-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank mithilfe der `DROP_EXISTING`-Option gelöscht und neu erstellt. Die Optionen `FILLFACTOR` und `PAD_INDEX` sind ebenfalls festgelegt.

```sql
CREATE NONCLUSTERED INDEX IX_WorkOrder_ProductID
  ON Production.WorkOrder(ProductID)
    WITH (FILLFACTOR = 80,
      PAD_INDEX = ON,
      DROP_EXISTING = ON);
GO
```

### <a name="h-create-an-index-on-a-view"></a>H. Erstellen eines Index für eine Sicht
Im folgenden Beispiel werden eine Sicht und ein Index für diese Sicht erstellt. Dies beinhaltet zwei Abfragen, in denen die indizierte Sicht verwendet wird.

```sql
-- Set the options to support indexed views
SET NUMERIC_ROUNDABORT OFF;
SET ANSI_PADDING, ANSI_WARNINGS, CONCAT_NULL_YIELDS_NULL, ARITHABORT,
  QUOTED_IDENTIFIER, ANSI_NULLS ON;
GO

-- Create view with schemabinding
IF OBJECT_ID ('Sales.vOrders', 'view') IS NOT NULL
  DROP VIEW Sales.vOrders;
GO

CREATE VIEW Sales.vOrders
  WITH SCHEMABINDING
AS
  SELECT SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Revenue,
    OrderDate, ProductID, COUNT_BIG(*) AS COUNT
  FROM Sales.SalesOrderDetail AS od, Sales.SalesOrderHeader AS o
  WHERE od.SalesOrderID = o.SalesOrderID
  GROUP BY OrderDate, ProductID;
GO

-- Create an index on the view
CREATE UNIQUE CLUSTERED INDEX IDX_V1
  ON Sales.vOrders (OrderDate, ProductID);
GO

-- This query can use the indexed view even though the view is
-- not specified in the FROM clause.
SELECT SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Rev,
  OrderDate, ProductID
FROM Sales.SalesOrderDetail AS od
  JOIN Sales.SalesOrderHeader AS o ON od.SalesOrderID = o.SalesOrderID
    AND ProductID BETWEEN 700 AND 800
    AND OrderDate >= CONVERT(DATETIME, '05/01/2002', 101)
GROUP BY OrderDate, ProductID
ORDER BY Rev DESC;
GO

-- This query can use the above indexed view
SELECT OrderDate, SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Rev
FROM Sales.SalesOrderDetail AS od
  JOIN Sales.SalesOrderHeader AS o ON od.SalesOrderID = o.SalesOrderID
    AND DATEPART(mm, OrderDate) = 3
  AND DATEPART(yy, OrderDate) = 2002
GROUP BY OrderDate
ORDER BY OrderDate ASC;
GO
```

### <a name="i-create-an-index-with-included-non-key-columns"></a>I. Erstellen eines Index mit eingeschlossenen (Nichtschlüssel-)Spalten
Im folgenden Beispiel wird ein nicht gruppierter Index mit einer Schlüsselspalte (`PostalCode`) und vier Nichtschlüsselspalten (`AddressLine1`, `AddressLine2`, `City`, `StateProvinceID`) erstellt. Es folgt eine Abfrage, die vom Index abgedeckt wird. Wenn Sie den vom Abfrageoptimierer ausgewählten Index anzeigen möchten, wählen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] im Menü **Abfrage** die Option **Display Actual Execution Plan** (Tatsächlichen Ausführungsplan einschließen) aus.

```sql
CREATE NONCLUSTERED INDEX IX_Address_PostalCode
  ON Person.Address (PostalCode)
  INCLUDE (AddressLine1, AddressLine2, City, StateProvinceID);
GO

SELECT AddressLine1, AddressLine2, City, StateProvinceID, PostalCode
FROM Person.Address
WHERE PostalCode BETWEEN N'98000' and N'99999';
GO
```

### <a name="j-create-a-partitioned-index"></a>J. Erstellen eines partitionierten Index
Im folgenden Beispiel wird ein nicht gruppierter partitionierter Index für `TransactionsPS1` (ein vorhandenes Partitionsschema in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank) erstellt. Dieses Beispiel setzt voraus, dass das Beispiel für einen partitionierten Index installiert wurde.

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
CREATE NONCLUSTERED INDEX IX_TransactionHistory_ReferenceOrderID
  ON Production.TransactionHistory (ReferenceOrderID)
  ON TransactionsPS1 (TransactionDate);
GO
```

### <a name="k-creating-a-filtered-index"></a>K. Erstellen eines gefilterten Index
Im folgenden Beispiel wird ein gefilterter Index für die Production.BillOfMaterials[!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Tabelle in der -Datenbank erstellt. Das Filterprädikat kann Spalten einschließen, die keine Schlüsselspalten im gefilterten Index sind. Das Prädikat in diesem Beispiel wählt nur die Zeilen aus, in denen EndDate nicht NULL ist.

```sql
CREATE NONCLUSTERED INDEX "FIBillOfMaterialsWithEndDate"
  ON Production.BillOfMaterials (ComponentID, StartDate)
  WHERE EndDate IS NOT NULL;
```

### <a name="l-create-a-compressed-index"></a>L. Erstellen eines komprimierten Index
Im folgenden Beispiel wird ein Index für eine nicht partitionierte Tabelle unter Verwendung der Zeilenkomprimierung erstellt.

```sql
CREATE NONCLUSTERED INDEX IX_INDEX_1
  ON T1 (C2)
  WITH (DATA_COMPRESSION = ROW);
GO
```

Im folgenden Beispiel wird ein Index für eine partitionierte Tabelle unter Verwendung der Zeilenkomprimierung für alle Partitionen des Indexes erstellt.

```sql
CREATE CLUSTERED INDEX IX_PartTab2Col1
  ON PartitionTable1 (Col1)
  WITH (DATA_COMPRESSION = ROW);
GO
```

 Im folgenden Beispiel wird ein Index für eine partitionierte Tabelle erstellt, wobei die Seitenkomprimierung für Partition `1` des Indexes und die Zeilenkomprimierung für die Partitionen `2` bis `4` des Indexes verwendet wird.

```sql
CREATE CLUSTERED INDEX IX_PartTab2Col1
  ON PartitionTable1 (Col1)
  WITH (
    DATA_COMPRESSION = PAGE ON PARTITIONS(1),
    DATA_COMPRESSION = ROW ON PARTITIONS (2 TO 4)
  );
GO
```

### <a name="m-create-resume-pause-and-abort-resumable-index-operations"></a>M. Erstellen, Fortsetzen, Anhalten und Abbrechen von Vorgängen fortsetzbarer Indizes
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
-- Execute a resumable online index create statement with MAXDOP=1
CREATE INDEX test_idx1 ON test_table (col1) WITH (ONLINE = ON, MAXDOP = 1, RESUMABLE = ON);

-- Executing the same command again (see above) after an index operation was paused, resumes automatically the index create operation.

-- Execute a resumable online index creates operation with MAX_DURATION set to 240 minutes. After the time expires, the resumable index create operation is paused.
CREATE INDEX test_idx2 ON test_table (col2) WITH (ONLINE = ON, RESUMABLE = ON, MAX_DURATION = 240);

-- Pause a running resumable online index creation
ALTER INDEX test_idx1 ON test_table PAUSE;
ALTER INDEX test_idx2 ON test_table PAUSE;

-- Resume a paused online index creation
ALTER INDEX test_idx1 ON test_table RESUME;
ALTER INDEX test_idx2 ON test_table RESUME;

-- Abort resumable index create operation which is running or paused
ALTER INDEX test_idx1 ON test_table ABORT;
ALTER INDEX test_idx2 ON test_table ABORT;
```

## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="n-basic-syntax"></a>N. Grundlegende Syntax
Erstellen, Fortsetzen, Anhalten und Abbrechen von Vorgängen fortsetzbarer Indizes       

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
-- Execute a resumable online index create statement with MAXDOP=1
CREATE INDEX test_idx ON test_table WITH (ONLINE = ON, MAXDOP = 1, RESUMABLE = ON);

-- Executing the same command again (see above) after an index operation was paused, resumes automatically the index create operation.

-- Execute a resumable online index creates operation with MAX_DURATION set to 240 minutes. After the time expires, the resumable index create operation is paused.
CREATE INDEX test_idx ON test_table WITH (ONLINE = ON, RESUMABLE = ON, MAX_DURATION = 240);

-- Pause a running resumable online index creation
ALTER INDEX test_idx ON test_table PAUSE;

-- Resume a paused online index creation
ALTER INDEX test_idx ON test_table RESUME;

-- Abort resumable index create operation which is running or paused
ALTER INDEX test_idx ON test_table ABORT;
```

### <a name="o-create-a-nonclustered-index-on-a-table-in-the-current-database"></a>O. Erstellen eines nicht gruppierten Index für eine Tabelle in der aktuellen Datenbank
Im folgenden Beispiel wird ein nicht gruppierter Index für die Spalte `VendorID` der Tabelle `ProductVendor` erstellt.

```sql
CREATE INDEX IX_ProductVendor_VendorID
  ON ProductVendor (VendorID);
```

### <a name="p-create-a-clustered-index-on-a-table-in-another-database"></a>P. Erstellen eines nicht gruppierten Index für eine Tabelle in einer anderen Datenbank
Im folgenden Beispiel wird ein nicht gruppierter Index für die `VendorID`-Spalte der `ProductVendor`-Tabelle in der `Purchasing`-Datenbank erstellt.

```sql
CREATE CLUSTERED INDEX IX_ProductVendor_VendorID
  ON Purchasing..ProductVendor (VendorID);
```
### <a name="q-create-an-ordered-clustered-index-on-a-table"></a>Q. Erstellen eines geordneten gruppierten Index für eine Tabelle  
Im folgenden Beispiel wird ein geordneter gruppierter Index für die `c1`-Spalte und die `c2`-Spalte der `T1`-Tabelle in der `MyDB`-Datenbank erstellt.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX MyOrderedCCI ON MyDB.dbo.T1 
ORDER (c1, c2);

```

### <a name="r-convert-a-cci-to-an-ordered-clustered-index-on-a-table"></a>R. Umwandeln eines gruppierten Columnstore-Index in einen geordneten gruppierten Index für eine Tabelle  
Im folgenden Beispiel wird der vorhandene gruppierte Columnstore-Index in einen geordneten gruppierten Columnstore-Index mit dem Namen `MyOrderedCCI` für die Spalten `c1` und `c2` der Tabelle `T2` in der Datenbank `MyDB` umgewandelt.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX MyOrderedCCI ON MyDB.dbo.T2
ORDER (c1, c2)
WITH (DROP_EXISTING = ON);

```

## <a name="see-also"></a>Weitere Informationen
[Leitfaden zur Architektur und zum Design von SQL Server-Indizes](../../relational-databases/sql-server-index-design-guide.md)     
[Ausführen von Onlineindexvorgängen](../../relational-databases/indexes/perform-index-operations-online.md)  
[Indizes und ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md#indexes-and-alter-table)     
[ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md)     
[CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md)     
[CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md)     
[CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)      
[CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md)     
[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)    
[CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md)     
[Datentypen](../../t-sql/data-types/data-types-transact-sql.md)    
[DBCC SHOW_STATISTICS](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)    
[DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md)    
[XML-Indizes &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)     
[sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)     
[sys.index_columns](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)    
[sys.xml_indexes](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md)     
[EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
