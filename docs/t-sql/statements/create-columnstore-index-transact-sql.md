---
description: CREATE COLUMNSTORE INDEX (Transact-SQL)
title: CREATE COLUMNSTORE INDEX (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_COLUMNSTORE_INDEX_TSQL
- COLUMNSTORE INDEX
- COLUMNSTORE_INDEX_TSQL
- CREATE CLUSTERED COLUMNSTORE INDEX
- COLUMNSTORE_TSQL
- CREATE NONCLUSTERED COLUMNSTORE INDEX
- CREATE_NONCLUSTERED_COLUMNSTORE_INDEX_TSQL
- CREATE COLUMNSTORE INDEX
- CREATE_CLUSTERED_COLUMNSTORE_INDEX_TSQL
- COLUMNSTORE
dev_langs:
- TSQL
helpviewer_keywords:
- index creation [SQL Server], columnstore indexes
- columnstore index, creating
- CREATE COLUMNSTORE INDEX statement
- CREATE INDEX statement
ms.assetid: 7e1793b3-5383-4e3d-8cef-027c0c8cb5b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 47759238673fa5e88c02421e27d0e12177072605
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347041"
---
# <a name="create-columnstore-index-transact-sql"></a>CREATE COLUMNSTORE INDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Konvertiert eine Rowstore-Tabelle in einen gruppierten Columnstore-Index oder erstellt einen nicht gruppierten Columnstore-Index. Verwendet einen Columnstore-Index, um eine operative Echtzeitanalyse für eine OLTP-Arbeitslast effizient auszuführen oder um die Datenkomprimierung und Abfrageleistung für Data Warehouse-Arbeitslasten zu verbessern.  
  
> [!NOTE]
> Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] können Sie die Tabelle als gruppierten Columnstore-Index erstellen.   Es ist nun nicht mehr erforderlich, zuerst eine Rowstore-Tabelle zu erstellen und diese dann in einen gruppierten Columnstore-Index zu konvertieren.  

> [!TIP]
> Weitere Informationen zu den Richtlinien für das Entwerfen von Indizes finden Sie im [Handbuch zum SQL Server-Indexentwurf](../../relational-databases/sql-server-index-design-guide.md).

Zu den Beispielen wechseln:  
-   [Beispiele zum Konvertieren einer Rowstore-Tabelle in Columnstore](../../t-sql/statements/create-columnstore-index-transact-sql.md#convert)  
-   [Beispiele für nicht gruppierte Columnstore-Indizes](../../t-sql/statements/create-columnstore-index-transact-sql.md#nonclustered)  
  
Zu den Szenarios wechseln:  
-   [Erste Schritte mit Columnstore für operative Echtzeitanalyse](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)  
-   [Columnstore-Indizes: Data Warehouse](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)  
  
Weitere Informationen:  
-   [Columnstore-Indizes: Übersicht](../../relational-databases/indexes/columnstore-indexes-overview.md)  
-   [Columnstore-Indizes: Zusammenfassung der Features](../../relational-databases/indexes/columnstore-indexes-what-s-new.md)  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Create a clustered columnstore index on disk-based table.  
CREATE CLUSTERED COLUMNSTORE INDEX index_name  
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }  
    [ WITH ( < with_option> [ ,...n ] ) ]  
    [ ON <on_option> ] 
[ ; ]  
  
--Create a nonclustered columnstore index on a disk-based table.  
CREATE [NONCLUSTERED]  COLUMNSTORE INDEX index_name   
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }
        ( column  [ ,...n ] )  
    [ WHERE <filter_expression> [ AND <filter_expression> ] ]
    [ WITH ( < with_option> [ ,...n ] ) ]  
    [ ON <on_option> ]
[ ; ]  
  
<with_option> ::=  
      DROP_EXISTING = { ON | OFF } -- default is OFF  
    | MAXDOP = max_degree_of_parallelism 
    | ONLINE = { ON | OFF } 
    | COMPRESSION_DELAY  = { 0 | delay [ Minutes ] }  
    | DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
      [ ON PARTITIONS ( { partition_number_expression | range } [ ,...n ] ) ]  
  
<on_option>::=  
      partition_scheme_name ( column_name )
    | filegroup_name
    | "default"
  
<filter_expression> ::=  
      column_name IN ( constant [ ,...n ]  
    | column_name { IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !< } constant  
  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE CLUSTERED COLUMNSTORE INDEX index_name
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name } 
    [ORDER (column [,...n] ) ]  
    [ WITH ( DROP_EXISTING = { ON | OFF } ) ] --default is OFF  
[;]  

```
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

Einige Optionen sind nicht in allen Datenbank-Engine-Versionen verfügbar. Die folgende Tabelle enthält die Versionen, in denen die Optionen in GRUPPIERTEN COLUMNSTORE- und NICHT GRUPPIERTEN COLUMNSTORE-INDIZES eingeführt wurden:

|Option| CLUSTERED | NONCLUSTERED |
|---|---|---|
| COMPRESSION_DELAY | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] |
| DATA_COMPRESSION | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] | 
| ONLINE | [!INCLUDE[ssSQLv15_md](../../includes/sssql19-md.md)] | [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] |
| WHERE-Klausel | – | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] |

Alle Optionen sind in Azure SQL-Datenbank verfügbar.

### <a name="create-clustered-columnstore-index"></a>CREATE CLUSTERED COLUMNSTORE INDEX

Erstellt einen gruppierten Columnstore-Index, in dem alle Daten komprimiert und nach Spalten gespeichert werden. Der Index beinhaltet alle Spalten der Tabelle und speichert die gesamte Tabelle. Wenn die vorhandene Tabelle ein Heap oder ein gruppierter Index ist, wird die Tabelle in einen gruppierten Columnstore-Index konvertiert. Wenn die Tabelle bereits als gruppierter Columnstore-Index gespeichert ist, wird der vorhandene Index gelöscht und neu erstellt.  
  
*index_name*  
Gibt den Namen für den neuen Index an.  
  
Wenn die Tabelle bereits einen gruppierten Columnstore-Index enthält, können Sie denselben Namen wie für den vorhandenen Index angeben oder die DROP EXISTING-Option zum Angeben eines neuen Namens verwenden.  
  
ON [*database_name*. [*schema_name* ] . | *schema_name* . ] *table_name*

Gibt den ein-, zwei- oder dreiteiligen Namen der Tabelle an, die als gruppierter Columnstore-Index gespeichert werden soll. Wenn die Tabelle ein Heap oder ein gruppierter Index ist, wird die Tabelle von einem Rowstore in einen Columnstore konvertiert. Wenn die Tabelle bereits ein Columnstore ist, wird mit dieser Anweisung der gruppierte Columnstore-Index neu erstellt. Zum Konvertieren in einen sortierten gruppierten Columnstore-Index muss es sich bei dem vorhandenen Index um einen gruppierten Columnstore-Index handeln.
  
#### <a name="with-options"></a>WITH-Optionen

##### <a name="drop_existing--off--on"></a>DROP_EXISTING = [OFF] | ON

   `DROP_EXISTING = ON` gibt an, dass der vorhandene Index gelöscht und ein neuer Columnstore-Index erstellt werden soll.  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH (DROP_EXISTING = ON);
```
   Bei Verwendung der Standardeinstellung DROP_EXISTING = OFF wird erwartet, dass der Indexname mit dem vorhandenen Namen übereinstimmt. Wenn der angegebene Indexname bereits vorhanden ist, tritt ein Fehler auf.  
  
##### <a name="maxdop--max_degree_of_parallelism"></a>MAXDOP = *max_degree_of_parallelism*  
   Überschreibt die Serverkonfiguration für den maximalen Grad an Parallelität für die Dauer des Indexvorgangs. Sie können mit MAXDOP die Anzahl der Prozessoren begrenzen, die bei der Ausführung paralleler Pläne verwendet werden. Maximal sind 64 Prozessoren zulässig.  
  
   *max_degree_of_parallelism* kann folgende Werte haben:  
   - 1 - Unterdrückt die Generierung paralleler Pläne.  
   - \>1 - Beschränkt die maximale Anzahl der Prozessoren, die bei einem parallelen Indexvorgang verwendet werden, je nach aktueller Systemauslastung auf die angegebene Zahl oder einen niedrigeren Wert. Beispiel: Wenn MAXDOP = 4, beträgt die Anzahl der verwendeten Prozessoren 4 oder weniger.  
   - 0 (Standard) - Verwendet abhängig von der aktuellen Systemarbeitsauslastung die tatsächliche Anzahl von Prozessoren oder weniger Prozessoren.  
  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH (MAXDOP = 2);
```

   Weitere Informationen finden Sie unter [Konfigurieren der Serverkonfigurationsoption Max. Grad an Parallelität](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) und [Konfigurieren von Parallelindexvorgängen](../../relational-databases/indexes/configure-parallel-index-operations.md).  
 
###### <a name="compression_delay--0--delay--minutes-"></a>COMPRESSION_DELAY = **0** | *delay* [ Minutes ]  
   Bei einer datenträgerbasierten Tabelle gibt *delay* die minimale Anzahl von Minuten an, die eine Delta-Zeilengruppe im Zustand CLOSED in der Delta-Zeilengruppe verbringen muss, bevor SQL Server sie in die komprimierte Zeilengruppe komprimieren kann. Da Einfügungs- und Aktualisierungszeiten in datenträgerbasierten Tabellen nicht für einzelne Zeilen nachverfolgt werden, wird in SQL Server die Verzögerung auf Delta-Zeilengruppen im Zustand CLOSED angewendet.  
   Die Standardeinstellung beträgt 0 Minuten.  
   
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( COMPRESSION_DELAY = 10 Minutes );
```

   Empfehlungen zur Verwendung von COMPRESSION_DELAY finden Sie unter [Erste Schritte mit Columnstore für operative Echtzeitanalyse](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
##### <a name="data_compression--columnstore--columnstore_archive"></a>DATA_COMPRESSION = COLUMNSTORE | COLUMNSTORE_ARCHIVE  
   Gibt die Datenkomprimierungsoption für die angegebene Tabelle, die Partitionsnummer oder den Bereich von Partitionen an. Die folgenden Optionen sind verfügbar:   
- `COLUMNSTORE` ist die Standardeinstellung und gibt an, dass die Komprimierung mit der leistungsstärksten Columnstore-Komprimierung ausgeführt werden soll. Dies ist die gängige Methode.  
- Durch `COLUMNSTORE_ARCHIVE` wird die Tabelle oder Partition weiter in eine geringere Größe komprimiert. Verwenden Sie diese Option für Situationen wie etwa die Archivierung, bei denen es auf eine geringere Datenspeichergröße und nicht auf den zusätzlichen Zeitaufwand für das Speichern und Abrufen ankommt.  
  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( DATA_COMPRESSION = COLUMNSTORE_ARCHIVE );
```
   Weitere Informationen zur Datenkomprimierung finden Sie unter [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md).  

###### <a name="online--on--off"></a>ONLINE = [ON | OFF]
- `ON` gibt an, dass der Columnstore-Index online und verfügbar bleibt, während die neue Kopie des Indexes erstellt wird.
- `OFF` gibt an, dass der Index nicht verwendet werden kann, während die neue Kopie erstellt wird.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( ONLINE = ON );
```

#### <a name="on-options"></a>ON-Optionen 
   Mit den ON-Optionen können Sie Optionen für die Datenspeicherung, wie ein Partitionsschema, eine bestimmte Dateigruppe oder die Standarddateigruppe, angeben. Wenn die ON-Option nicht angegeben ist, verwendet der Index die Einstellungspartition oder Dateigruppeneinstellungen der vorhandenen Tabelle.  
  
   *partition_scheme_name* **(** _column_name_ **)**  
   Gibt das Partitionsschema für die Tabelle an. Das Partitionsschema muss in der Datenbank bereits vorhanden sein. Informationen zum Erstellen des Partitionsschemas finden Sie unter [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md).  
 
   *column_name* gibt die Spalte an, auf deren Grundlage ein partitionierter Index partitioniert wird. Diese Spalte muss mit dem Datentyp, der Länge und der Genauigkeit des Arguments der Partitionsfunktion übereinstimmen, die *partition_scheme_name* verwendet.  

   *filegroup_name*  
   Gibt die Dateigruppe zum Speichern des gruppierten Columnstore-Indexes an. Wenn kein Speicherort angegeben und die Tabelle nicht partitioniert ist, verwendet der Index die gleiche Dateigruppe wie die zugrunde liegende Tabelle oder Sicht. Die Dateigruppe muss bereits vorhanden sein.  

   **"** default **"**  
   Um den Index in der Standarddateigruppe zu erstellen, verwenden Sie "default" oder [ default ].  
  
   Wenn "default" angegeben wird, muss die Option QUOTED_IDENTIFIER für die aktuelle Sitzung auf ON festgelegt sein. QUOTED_IDENTIFIER hat standardmäßig den Wert ON. Weitere Informationen finden Sie unter [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
### <a name="create-nonclustered-columnstore-index"></a>CREATE [NONCLUSTERED] COLUMNSTORE INDEX  
Erstellt einen nicht gruppierten In-Memory-Columnstore-Index in einer als Heap oder gruppierter Index gespeicherten Rowstore-Tabelle. Der Index kann gefiltert sein und muss nicht alle Spalten der zugrunde liegenden Tabelle enthalten. Der Columnstore-Index benötigt genügend Platz zum Speichern einer Kopie der Daten. Er kann aktualisiert werden und wird bei einer Änderung der zugrunde liegenden Tabelle aktualisiert. Der nicht gruppierte Columnstore-Index in einem gruppierten Index ermöglicht eine Echtzeitanalyse.  
  
*index_name*  
   Gibt den Namen des Indexes an. *index_name* muss innerhalb der Tabelle eindeutig sein, kann aber innerhalb der Datenbank mehrfach vorkommen. Indexnamen müssen den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen.  
  
 **(** _column_  [ **,** ...*n* ] **)**  
    Gibt die zu speichernden Spalten an. Ein nicht gruppierter Columnstore-Index ist auf 1024 Spalten beschränkt.  
   Jede Spalte muss ein unterstützter Datentyp für columnstore-Indizes sein. Eine Liste der unterstützten Datentypen finden Sie unter [Einschränkungen](../../t-sql/statements/create-columnstore-index-transact-sql.md#LimitRest).  

ON [*database_name*. [*schema_name* ] . | *schema_name* . ] *table_name*  
   Gibt den ein-, zwei- oder dreiteiligen Name der Tabelle an, die den Index enthält.  

#### <a name="with-options"></a>WITH-Optionen
##### <a name="drop_existing--off--on"></a>DROP_EXISTING = [OFF] | ON  
   DROP_EXISTING = ON Der vorhandene Index wird gelöscht und neu erstellt. Der angegebene Indexname muss mit dem Namen eines derzeit vorhandenen Index übereinstimmen. Die Indexdefinition kann jedoch geändert werden. Sie können z. B. andere Spalten oder Indexoptionen angeben.
  
   DROP_EXISTING = OFF Es wird ein Fehler angezeigt, wenn der angegebene Indexname bereits vorhanden ist. Der Indextyp kann nicht mithilfe von DROP_EXISTING geändert werden. In abwärtskompatibler Syntax ist WITH DROP_EXISTING gleichwertig mit WITH DROP_EXISTING = ON.  

###### <a name="maxdop--max_degree_of_parallelism"></a>MAXDOP = *max_degree_of_parallelism*  
   Überschreibt die Konfigurationsoption [Max. Grad an Parallelität](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) für die Dauer des Indexvorgangs. Sie können mit MAXDOP die Anzahl der Prozessoren begrenzen, die bei der Ausführung paralleler Pläne verwendet werden. Maximal sind 64 Prozessoren zulässig.  
  
   *max_degree_of_parallelism* kann folgende Werte haben:  
   - 1 - Unterdrückt die Generierung paralleler Pläne.  
   - \>1 - Beschränkt die maximale Anzahl der Prozessoren, die bei einem parallelen Indexvorgang verwendet werden, je nach aktueller Systemauslastung auf die angegebene Zahl oder einen niedrigeren Wert. Beispiel: Wenn MAXDOP = 4, beträgt die Anzahl der verwendeten Prozessoren 4 oder weniger.  
   - 0 (Standard) - Verwendet abhängig von der aktuellen Systemarbeitsauslastung die tatsächliche Anzahl von Prozessoren oder weniger Prozessoren.  
  
   Weitere Informationen finden Sie unter [Konfigurieren von Parallelindexvorgängen](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
>  Parallele Indexvorgänge sind nicht in jeder Edition von [!INCLUDE[msC](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verfügbar. Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Editionen und unterstütze Funktionen für den SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
###### <a name="online--on--off"></a>ONLINE = [ON | OFF]   
- `ON` gibt an, dass der Columnstore-Index online und verfügbar bleibt, während die neue Kopie des Indexes erstellt wird.
- `OFF` gibt an, dass der Index nicht verwendet werden kann, während die neue Kopie erstellt wird. In einem nicht gruppierten Index bleibt die Basistabelle verfügbar, nur der nicht gruppierte Columnstore-Index wird erst für Abfragen verwendet, wenn der neue Index erstellt ist. 

```sql
CREATE COLUMNSTORE INDEX ncci ON Sales.OrderLines (StockItemID, Quantity, UnitPrice, TaxRate) WITH ( ONLINE = ON );
```

##### <a name="compression_delay--0--delayminutes"></a>COMPRESSION_DELAY = **0** | \<delay>[Minutes]  
   Gibt eine Untergrenze für die Zeitdauer an, für die eine Zeile in der Deltazeilengruppe bleibt, bevor sie in die komprimierte Zeilengruppe migriert werden kann. Ein Kunde kann beispielsweise angeben, dass eine Zeile für die Komprimierung in das Spaltenspeicherformat freigegeben werden soll, wenn an ihr 120 Minuten lang keine Änderungen vorgenommen wurden. Bei Columnstore-Indizes in datenträgerbasierten Tabellen wird die Zeit nicht nachverfolgt, wenn eine Zeile eingefügt oder aktualisiert wird. Vielmehr wird die Zeit, in der die Deltazeilengruppe geschlossen war, als Proxy für die Zeile verwendet. Die Standardeinstellung beträgt 0 Minuten. Eine Zeile wird in einen Speicher im Spaltenformat migriert, nachdem sich 1 Million Zeilen in der Deltazeilengruppe angesammelt haben und die Zeile als geschlossen gekennzeichnet wurde.  
  
###### <a name="data_compression"></a>DATA_COMPRESSION  
   Gibt die Datenkomprimierungsoption für die angegebene Tabelle, die Partitionsnummer oder den Bereich von Partitionen an. Gilt nur für columnstore-Indizes, einschließlich nicht gruppierter und gruppierter columnstore-Indizes. Die folgenden Optionen sind verfügbar:
   
- `COLUMNSTORE`: Dies ist die Standardeinstellung und gibt an, dass die Komprimierung mit der leistungsstärksten Columnstore-Komprimierung ausgeführt werden soll. Dies ist die gängige Methode.  
- `COLUMNSTORE_ARCHIVE`: Durch COLUMNSTORE_ARCHIVE wird die Tabelle oder Partition weiter in eine geringere Größe komprimiert. Dies empfiehlt sich bei der Archivierung und in Situationen, in denen es auf eine geringere Speichergröße und nicht auf den zusätzlichen Zeitaufwand für das Speichern und Abrufen ankommt.  
  
 Weitere Informationen zur Datenkomprimierung finden Sie unter [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md).  
  
##### <a name="where-filter_expression--and-filter_expression-"></a>WHERE \<filter_expression> [AND \<filter_expression>]
  
   Gibt nach dem Aufruf eines Filterprädikats an, welche Zeilen in den Index aufgenommen werden sollen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellt gefilterte Statistikdaten für die Datenzeilen im gefilterten Index.  
  
   Für das Filterprädikat wird eine einfache Vergleichslogik verwendet. Vergleiche mit NULL-Literalen sind mit den Vergleichsoperatoren nicht zulässig. Verwenden Sie stattdessen den IS NULL-Operator und den IS NOT NULL-Operator.  
  
   Es folgen einige Beispiele für Filterprädikate für die `Production.BillOfMaterials`-Tabelle:  
   `WHERE StartDate > '20000101' AND EndDate <= '20000630'`    
   `WHERE ComponentID IN (533, 324, 753)`  
   `WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL`  
   
   Informationen zu gefilterten Indizes finden Sie unter [Erstellen gefilterter Indizes](../../relational-databases/indexes/create-filtered-indexes.md).  
  
#### <a name="on-options"></a>ON-Optionen  
   Diese Optionen geben die Dateigruppen an, für die der Index erstellt wird.  
  
*partition_scheme_name* **(** _column_name_ **)**  
   Gibt das Partitionsschema an, das die Dateigruppen definiert, denen die Partitionen eines partitionierten Index zugeordnet werden. Das Partitionsschema muss bereits durch Ausführen von [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) in der Datenbank vorhanden sein. 
   *column_name* gibt die Spalte an, auf deren Grundlage ein partitionierter Index partitioniert wird. Diese Spalte muss mit dem Datentyp, der Länge und der Genauigkeit des Arguments der Partitionsfunktion übereinstimmen, die *partition_scheme_name* verwendet. *column_name* ist nicht auf Spalten in der Indexdefinition beschränkt. Wenn Sie einen columnstore-Index partitionieren, fügt [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Partitionierungsspalte als Spalte des Index hinzu, wenn sie noch nicht angegeben ist.  
   Wenn *partition_scheme_name* oder *filegroup* bei einer partitionierten Tabelle nicht angegeben werden, wird der Index in demselben Partitionsschema platziert und verwendet dieselbe Partitionierungsspalte wie die zugrunde liegende Tabelle.  
   Ein columnstore-Index einer partitionierten Tabelle muss über eine Partitionsausrichtung verfügen.  
   Weitere Informationen zu partitionierten Indizes finden Sie unter [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  

*filegroup_name*  
   Gibt den Namen einer Dateigruppe an, für die der Index erstellt werden soll. Wenn *filegroup_name* nicht angegeben und die Tabelle nicht partitioniert ist, verwendet der Index die gleiche Dateigruppe wie die zugrunde liegende Tabelle. Die Dateigruppe muss bereits vorhanden sein.  
 
**"** default **"**  
Erstellt den angegebenen Index für die Standarddateigruppe.  
  
Die Benennung default ist in diesem Kontext kein Schlüsselwort. Es handelt sich dabei um einen Bezeichner für die Standarddateigruppe. Dieser muss wie in ON **"** default **"** or ON **[** default **]** durch Trennzeichen getrennt werden. Wenn "default" angegeben wird, muss die Option QUOTED_IDENTIFIER für die aktuelle Sitzung auf ON festgelegt sein. Dies ist die Standardeinstellung. Weitere Informationen finden Sie unter [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER-Berechtigung für die Tabelle.  
  
##  <a name="general-remarks"></a><a name="GenRemarks"></a> Allgemeine Hinweise  
Ein Columnstore-Index kann für eine temporäre Tabelle erstellt werden. Wenn die Tabelle gelöscht oder die Sitzung beendet wird, wird der Index ebenfalls gelöscht.  

Ein sortierter, gruppierter Columnstore-Index kann für Spalten beliebiger in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] unterstützter Datentypen erstellt werden, mit Ausnahme von Zeichenfolgenspalten.  
 
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
  
     *Durch Festlegen von ANSI_WARNINGS auf ON wird implizit ARITHABORT auf ON festgelegt, wenn der Kompatibilitätsgrad der Datenbank auf 90 oder höher festgelegt ist. Wird der Kompatibilitätsgrad der Datenbank auf 80 oder niedriger festgelegt, muss die ARITHABORT-Option explizit auf ON festgelegt werden.  
  
 Wenn die SET-Optionen falsch sind, können die folgenden Bedingungen auftreten:  
  
-   Der gefilterte Index wird nicht erstellt.  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] generiert einen Fehler und führt ein Rollback aller INSERT-, UPDATE-, DELETE- oder MERGE-Anweisungen aus, die im Index gespeicherte Daten ändern.  
  
-   Der Abfrageoptimierer berücksichtigt den Index des Abfrageausführungsplans von Transact-SQL-Anweisungen nicht.  
  
 Weitere Informationen zu gefilterten Indizes finden Sie unter [Erstellen gefilterter Indizes](../../relational-databases/indexes/create-filtered-indexes.md). 
  
##  <a name="limitations-and-restrictions"></a><a name="LimitRest"></a> Einschränkungen  

**Jede Spalte in einem Columnstore-Index muss von einem der folgenden allgemeinen Geschäftsdatentypen sein:** 
-   datetimeoffset [ ( *n* ) ]  
-   datetime2 [ ( *n* ) ]  
-   datetime  
-   smalldatetime  
-   date  
-   time [ ( *n* ) ]  
-   float [ ( *n* ) ]  
-   real [ ( *n* ) ]  
-   decimal [ ( *precision* [ *, scale* ] **)** ]
-   numeric [ ( *precision* [ *, scale* ] **)** ]    
-   money  
-   SMALLMONEY  
-   BIGINT  
-   INT  
-   SMALLINT  
-   TINYINT  
-   bit  
-   nvarchar [ ( *n* ) ] 
-   nvarchar(max) (Gilt für [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)], den Premium-Tarif und den Standard-Tarif (S3 und höher) sowie alle Tarife der vCore-Angebote, ausschließlich in gruppierten Columnstore-Indizes)   
-   nchar [ ( *n* ) ]  
-   varchar [ ( *n* ) ]  
-   varchar(max) (Gilt für [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)], den Premium-Tarif und den Standard-Tarif (S3 und höher) sowie alle Tarife der vCore-Angebote, ausschließlich in gruppierten Columnstore-Indizes)
-   char [ ( *n* ) ]  
-   varbinary [ ( *n* ) ] 
-   varbinary (max) (Gilt für [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und Azure SQL Database zum Premium- und Standard-Tarif (S3 und höher) sowie alle Tarife der vCore-Angebote, ausschließlich in gruppierten Columnstore-Indizes)
-   binary [ ( *n* ) ]  
-   uniqueidentifier  (Gilt für [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher)
  
Wenn die zugrunde liegende Tabelle eine Spalte enthält, die einen Datentyp aufweist, der für Columnstore-Indizes nicht unterstützt wird, müssen Sie diese Spalte aus dem nicht gruppierten Columnstore-Index ausschließen.  
  
**Spalten, die einen der folgenden Datentypen verwenden, können nicht in einem Columnstore-Index enthalten sein:**
-   ntext, text und image  
-   nvarchar(max), varchar(max) und varbinary(max) (Gilt für [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und frühere Versionen und nicht gruppierte Columnstore-Indizes) 
-   rowversion (und timestamp)  
-   sql_variant  
-   CLR-Typen (hierarchyid- und räumliche Typen)  
-   Xml  
-   uniqueidentifier (gilt für [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])  

**Nicht gruppierte Columnstore-Indizes:**
-   Kann nicht mehr als 1024 Spalten enthalten.
-   Kann nicht als einschränkungsbasierter Index erstellt werden. Es ist möglich, dass Tabellen mit einem Columnstore-Index UNIQUE-, PRIMARY KEY- oder FOREIGN KEY-Einschränkungen enthalten. Einschränkungen werden immer mit einem Zeilenspeicherindex durchgesetzt. Sie können nie mit einem Columnstore-Index erzwungen werden, egal ob geclustert oder nicht.
-   Kann keine Sparsespalte enthalten.  
-   Kann nicht mithilfe der **ALTER INDEX**-Anweisung geändert werden. Um den nicht gruppierten Index zu ändern, müssen Sie stattdessen den columnstore-Index löschen und neu erstellen. Sie können einen Columnstore-Index mithilfe von **ALTER INDEX** deaktivieren und neu erstellen.  
-   Kann nicht mit dem Schlüsselwort **INCLUDE** erstellt werden.  
-   Darf nicht das Schlüsselwort **ASC** oder das Schlüsselwort **DESC** zum Sortieren des Index enthalten. Columnstore-Indizes werden gemäß den Komprimierungsalgorithmen sortiert. Durch die Sortierung würden viele der Leistungsvorteile entfernt werden.  
-   Darf keine LOB-Spalten (Large Object) vom Typ nvarchar(max), varchar(max) bzw. varbinary(max) in gruppierten Columnstore-Indizes enthalten. LOB-Datentypen werden in gruppierten Columnstore-Indizes nur von Versionen ab [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und von der Azure SQL-Datenbank mit konfiguriertem Premium- und Standard-Tarif (S3 und höher) sowie allen Angebotstarifen unterstützt. Frühere Versionen unterstützen LOB-Datentypen in gruppierten und nicht gruppierten Columnstore-Indizes nicht.


> [!NOTE]  
> Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] können Sie einen nicht gruppierten Columnstore-Index für eine indizierte Sicht erstellen.  


 **Columnstore-Indizes können mit den folgenden Features nicht kombiniert werden:**  
-   Berechnete Spalten. Ab SQL Server 2017 darf ein gruppierter Columnstore-Index eine nicht persistierte berechnete Spalte enthalten. In SQL Server 2017 dürfen gruppierte Columnstore-Indizes jedoch keine persistierten berechneten Spalten enthalten, und es können keine nicht gruppierten Indizes für berechnete Spalten erstellt werden. 
-   Seiten- und Zeilenkomprimierung und **vardecimal**-Speicherformat (ein Columnstore-Index ist bereits in einem anderen Format komprimiert)  
-   Replikation  
-   Filestream

In einer Tabelle mit einem gruppierten Columnstore-Index können keine Cursor oder Trigger verwendet werden. Diese Einschränkung gilt nicht für nicht gruppierte Columnstore-Indizes; in einer Tabelle mit einem nicht gruppierten Columnstore-Index können Cursor und Trigger verwendet werden.

**[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]-spezifische Einschränkungen**  
Diese Einschränkungen gelten nur für [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. Mit diesem Release wurden aktualisierbare gruppierte Columnstore-Indizes eingeführt. Nicht gruppierte Columnstore-Indizes waren noch schreibgeschützt.  

-   Änderungsnachverfolgung. Für Columnstore-Indizes kann keine Änderungsnachverfolgung verwendet werden.  
-   Change Data Capture. Change Data Capture kann nicht für Tabellen mit einem gruppierten Columnstore-Index aktiviert werden. Ab SQL Server 2016 kann das Feature für Tabellen mit einem nicht gruppierten Columnstore-Index aktiviert werden.  
-   Lesbares sekundäres Replikat. Auf einen gruppierten Columnstore-Index (CCI) kann nicht über ein lesbares sekundäres Replikat einer lesbaren Always On-Verfügbarkeitsgruppe zugegriffen werden.  Auf einen nicht gruppierten Columnstore-Index (NCCI) kann über ein lesbares sekundäres Replikat zugegriffen werden.  
-   Mehrere aktive Resultsets (MARS). [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] verwendet MARS für schreibgeschützte Verbindungen für Tabellen mit einem Columnstore-Index. [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] unterstützt MARS jedoch nicht für gleichzeitige DML-Vorgänge (Datenbearbeitungssprache) für eine Tabelle mit einem Columnstore-Index. In diesem Fall beendet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Verbindung und bricht die Transaktionen ab.  
-  Nicht gruppierte Columnstore-Indizes können nicht für eine Ansicht oder indizierte Sicht erstellt werden.
  
 Informationen zu den Leistungsvorteilen und Einschränkungen von Columnstore-Indizes finden Sie unter [Columnstore-Indizes: Übersicht](../../relational-databases/indexes/columnstore-indexes-overview.md).
  
##  <a name="metadata"></a><a name="Metadata"></a> Metadaten  
 Alle Spalten in einem Columnstore-Index werden in den Metadaten als eingeschlossene Spalten gespeichert. Der Columnstore-Index weist keine Schlüsselspalten auf. Diese Systemsichten enthalten Informationen zu Columnstore-Indizes.  
  
-   [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
-   [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
-   [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)  
-   [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
-   [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)  
-   [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)  

##  <a name="examples-for-converting-a-rowstore-table-to-columnstore"></a><a name="convert"></a> Beispiele zum Konvertieren einer Rowstore-Tabelle in Columnstore  
  
### <a name="a-convert-a-heap-to-a-clustered-columnstore-index"></a>A. Konvertieren eines Heaps in einen gruppierten Columnstore-Index  
 In diesem Beispiel wird eine Tabelle als Heap erstellt und anschließend in einen gruppierten Columnstore-Index mit dem Namen cci_Simple konvertiert. Dadurch wird der Speicher für die gesamte Tabelle von rowstore in columnstore geändert.  
  
```sql  
CREATE TABLE SimpleTable(  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED COLUMNSTORE INDEX cci_Simple ON SimpleTable;  
GO  
```  
  
### <a name="b-convert-a-clustered-index-to-a-clustered-columnstore-index-with-the-same-name"></a>B. Konvertiert einen gruppierten Index in einen gruppierten Columnstore-Index mit demselben Namen.  
 In diesem Beispiel wird eine Tabelle mit einem gruppierten Index erstellt. Anschließend wird die Syntax zum Konvertieren des gruppierten Indexes in einen gruppierten Columnstore-Index veranschaulicht. Dadurch wird der Speicher für die gesamte Tabelle von rowstore in columnstore geändert.  
  
```sql  
CREATE TABLE SimpleTable (  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED INDEX cl_simple ON SimpleTable (ProductKey);  
GO  
CREATE CLUSTERED COLUMNSTORE INDEX cl_simple ON SimpleTable  
WITH (DROP_EXISTING = ON);  
GO  
```  
  
### <a name="c-handle-nonclustered-indexes-when-converting-a-rowstore-table-to-a-columnstore-index"></a>C. Verarbeitung von nicht gruppierten Indizes beim Konvertieren einer Rowstore-Tabelle in einen Columnstore-Index.  
 In diesem Beispiel wird gezeigt, wie nicht gruppierte Indizes beim Konvertieren einer Rowstore-Tabelle in einen Columnstore-Index verarbeitet werden. Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] ist keine spezielle Aktion erforderlich. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definiert die nicht gruppierten Indizes automatisch und erstellt sie neu im neuen gruppierten Columnstore-Index.  
  
 Wenn Sie die nicht gruppierten Indizes löschen möchten, verwenden Sie vor dem Erstellen des Columnstore-Indexes die DROP INDEX-Anweisung. Mit der DROP EXISTING-Option wird nur der konvertierte gruppierte Index gelöscht. Die nicht gruppierten Indizes werden nicht gelöscht.  
  
 In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] konnte für einen Columnstore-Index kein nicht gruppierter Index erstellt werden. Im folgenden Beispiel wird gezeigt, dass in früheren Releases vor dem Erstellen des Columnstore-Indexes die nicht gruppierten Indizes gelöscht werden mussten.  
  
```sql  
--Create the table for use with this example.  
CREATE TABLE SimpleTable (  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
  
--Create two nonclustered indexes for use with this example  
CREATE INDEX nc1_simple ON SimpleTable (OrderDateKey);  
CREATE INDEX nc2_simple ON SimpleTable (DueDateKey);   
GO  
  
--SQL Server 2012 and SQL Server 2014: you need to drop the nonclustered indexes  
--in order to create the columnstore index.   
  
DROP INDEX SimpleTable.nc1_simple;  
DROP INDEX SimpleTable.nc2_simple;  
  
--Convert the rowstore table to a columnstore index.  
CREATE CLUSTERED COLUMNSTORE INDEX cci_simple ON SimpleTable;   
GO  
```  
  
### <a name="d-convert-a-large-fact-table-from-rowstore-to-columnstore"></a>D: Konvertieren einer großen Faktentabelle von einem Rowstore in einen Columnstore  
 In diesem Beispiel wird erläutert, wie eine große Faktentabelle von einer Rowstore-Tabelle in eine Columnstore-Tabelle konvertiert wird.  
  
 So konvertieren Sie eine Rowstore-Tabelle in eine Columnstore-Tabelle:  
  
1.  Erstellen Sie zunächst eine kleine Tabelle, die für das Beispiel verwendet werden kann.  
  
    ```sql  
    --Create a rowstore table with a clustered index and a nonclustered index.  
    CREATE TABLE MyFactTable (  
        ProductKey [INT] NOT NULL,  
        OrderDateKey [INT] NOT NULL,  
         DueDateKey [INT] NOT NULL,  
         ShipDateKey [INT] NOT NULL )  
    )  
    WITH (  
        CLUSTERED INDEX ( ProductKey )  
    );  
  
    --Add a nonclustered index.  
    CREATE INDEX my_index ON MyFactTable ( ProductKey, OrderDateKey );  
    ```  
  
2.  Löschen Sie alle nicht gruppierten Indizes aus der Rowstore-Tabelle.  
  
    ```sql  
    --Drop all nonclustered indexes  
    DROP INDEX my_index ON MyFactTable;  
    ```  
  
3.  Löschen Sie den gruppierten Index.  
  
    -   Führen Sie diesen Schritt nur aus, wenn Sie beim Konvertieren in einen gruppierten Columnstore-Index einen neuen Namen für den Index angeben möchten. Wenn Sie den gruppierten Index nicht löschen, weist der neue gruppierte Columnstore-Index den gleichen Namen auf.  
  
        > [!NOTE]  
        > Der Indexname ist möglicherweise einprägsamer, wenn Sie einen eigenen Namen angeben. Alle gruppierten Rowstore-Indizes weisen den Standardnamen auf, der „ClusteredIndex_\<GUID>“ lautet.  
  
    ```sql  
    --Process for dropping a clustered index.  
    --First, look up the name of the clustered rowstore index.  
    --Clustered rowstore indexes always use the DEFAULT name 'ClusteredIndex_<GUID>'.  
    SELECT i.name   
    FROM sys.indexes i   
    JOIN sys.tables t  
    ON ( i.type_desc = 'CLUSTERED' ) WHERE t.name = 'MyFactTable';  
  
    --Drop the clustered rowstore index.  
    DROP INDEX ClusteredIndex_d473567f7ea04d7aafcac5364c241e09 ON MyDimTable;  
    ```  
  
4.  Konvertieren Sie die Rowstore-Tabelle in eine Columnstore-Tabelle mit einem gruppierten Columnstore-Index.  
  
    ```sql  
    --Option 1: Convert to columnstore and name the new clustered columnstore index MyCCI.  
    CREATE CLUSTERED COLUMNSTORE INDEX MyCCI ON MyFactTable;  
  
    --Option 2: Convert to columnstore and use the rowstore clustered   
    --index name for the columnstore clustered index name.  
    --First, look up the name of the clustered rowstore index.  
    SELECT i.name   
    FROM sys.indexes i  
    JOIN sys.tables t   
    ON ( i.type_desc = 'CLUSTERED' )  
    WHERE t.name = 'MyFactTable';  
  
    --Second, create the clustered columnstore index and   
    --Replace ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
    --with the name of your clustered index.  
    CREATE CLUSTERED COLUMNSTORE INDEX   
    ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
     ON MyFactTable  
    WITH DROP_EXISTING = ON;  
    ```  
  
### <a name="e-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index"></a>E. Konvertieren einer Columnstore-Tabelle in eine Rowstore-Tabelle mit einem gruppierten Index  
 Verwenden Sie die CREATE INDEX-Anweisung mit der DROP_EXISTING-Option, um eine Columnstore-Tabelle in eine Rowstore-Tabelle mit einem gruppierten Index zu konvertieren.  
  
```sql  
CREATE CLUSTERED INDEX ci_MyTable   
ON MyFactTable  
WITH ( DROP EXISTING = ON );  
```  
  
### <a name="f-convert-a-columnstore-table-to-a-rowstore-heap"></a>F. Konvertieren einer Columnstore-Tabelle in einen Rowstore-Heap  
 Löschen Sie zum Konvertieren einer Columnstore-Tabelle in einen Rowstore-Heap einfach den gruppierten Columnstore-Index.  
  
```sql  
DROP INDEX MyCCI   
ON MyFactTable;  
```  
  

### <a name="g-defragment-by-rebuilding-the-entire-clustered-columnstore-index"></a>G. Defragmentieren durch Neuerstellen des gesamten gruppierten Columnstore-Indexes  
   Gilt für: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
 Es gibt zwei Möglichkeiten, den gesamten gruppierten Columnstore-Index neu zu erstellen. Sie können CREATE CLUSTERED COLUMNSTORE INDEX oder [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) und die REBUILD-Option verwenden. Mit beiden Methoden werden die gleichen Ergebnisse erzielt.  
  
> [!NOTE]  
> Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] können Sie `ALTER INDEX...REORGANIZE` verwenden, anstatt den gesamten Index mit diesen beispielhaften Methoden neu zu erstellen.  
  
```sql  
--Determine the Clustered Columnstore Index name of MyDimTable.  
SELECT i.object_id, i.name, t.object_id, t.name   
FROM sys.indexes i   
JOIN sys.tables t  
ON (i.type_desc = 'CLUSTERED COLUMNSTORE')  
WHERE t.name = 'RowstoreDimTable';  
  
--Rebuild the entire index by using CREATE CLUSTERED INDEX.  
CREATE CLUSTERED COLUMNSTORE INDEX my_CCI   
ON MyFactTable  
WITH ( DROP_EXISTING = ON );  
  
--Rebuild the entire index by using ALTER INDEX and the REBUILD option.  
ALTER INDEX my_CCI  
ON MyFactTable  
REBUILD PARTITION = ALL  
WITH ( DROP_EXISTING = ON );  
```  
  
##  <a name="examples-for-nonclustered-columnstore-indexes"></a><a name="nonclustered"></a> Beispiele für nicht gruppierte Columnstore-Indizes  
  
### <a name="a-create-a-columnstore-index-as-a-secondary-index-on-a-rowstore-table"></a>A. Erstellen eines Columnstore-Indexes als sekundärer Index für eine Rowstore-Tabelle  
 In diesem Beispiel wird ein nicht gruppierter Columnstore-Index für eine Rowstore-Tabelle erstellt. In diesem Fall kann nur ein Columnstore-Index erstellt werden. Der Columnstore-Index erfordert zusätzlichen Speicher, da er eine Kopie der Daten in der Rowstore-Tabelle enthält. In diesem Beispiel wird eine einfache Tabelle und ein gruppierter Index erstellt. Anschließend wird die Syntax zum Erstellen eines nicht gruppierten Columnstore-Indexes beschrieben.  
  
```sql  
CREATE TABLE SimpleTable  
(ProductKey [INT] NOT NULL,   
OrderDateKey [INT] NOT NULL,   
DueDateKey [INT] NOT NULL,   
ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED INDEX cl_simple ON SimpleTable (ProductKey);  
GO  
CREATE NONCLUSTERED COLUMNSTORE INDEX csindx_simple  
ON SimpleTable  
(OrderDateKey, DueDateKey, ShipDateKey);  
GO  
```  
  
### <a name="b-create-a-simple-nonclustered-columnstore-index-using-all-options"></a>B. Erstellen eines einfachen nicht gruppierten Columnstore-Indexes mit allen Optionen  
 Im folgenden Beispiel wird die Syntax zum Erstellen eines nicht gruppierten Columnstore-Indexes unter Verwendung aller Optionen veranschaulicht.  
  
```sql  
CREATE NONCLUSTERED COLUMNSTORE INDEX csindx_simple  
ON SimpleTable  
(OrderDateKey, DueDateKey, ShipDateKey)  
WITH (DROP_EXISTING =  ON,   
    MAXDOP = 2)  
ON "default"  
GO  
```  
  
 Ein komplexeres Beispiel mit partitionierten Tabellen finden Sie unter [Columnstore-Indizes: Übersicht](../../relational-databases/indexes/columnstore-indexes-overview.md).  
  
### <a name="c-create-a-nonclustered-columnstore-index-with-a-filtered-predicate"></a>C. Erstellen eines nicht gruppierten Columnstore-Indexes mit einem gefilterten Prädikat  
 Im folgenden Beispiel wird ein gefilterter nicht gruppierter Columnstore-Index für die Production.BillOfMaterials-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank erstellt. Das Filterprädikat kann Spalten einschließen, die keine Schlüsselspalten im gefilterten Index sind. Das Prädikat in diesem Beispiel wählt nur die Zeilen aus, in denen {1}EndDate{2} nicht NULL ist.  
  
```sql  
IF EXISTS (SELECT name FROM sys.indexes  
    WHERE name = N'FIBillOfMaterialsWithEndDate'   
    AND object_id = OBJECT_ID(N'Production.BillOfMaterials'))  
DROP INDEX FIBillOfMaterialsWithEndDate  
    ON Production.BillOfMaterials;  
GO  
CREATE NONCLUSTERED COLUMNSTORE INDEX "FIBillOfMaterialsWithEndDate"  
    ON Production.BillOfMaterials (ComponentID, StartDate)  
    WHERE EndDate IS NOT NULL;  
```  
  
###  <a name="d-change-the-data-in-a-nonclustered-columnstore-index"></a><a name="ncDML"></a> D. Ändern der Daten in einem nicht gruppierten Columnstore-Index  
   Gilt für: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] bis [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].
  
 Wenn Sie einen nicht gruppierten Columnstore-Index für eine Tabelle erstellen, können Sie die Daten in dieser Tabelle nicht mehr direkt ändern. Eine Abfrage mit INSERT, UPDATE, MERGE oder DELETE schlägt fehl und gibt eine Fehlermeldung zurück. Um Daten in der Tabelle hinzuzufügen oder zu ändern, können Sie eine der folgenden Aktionen ausführen:  
  
-   Deaktivieren oder löschen Sie den Columnstore-Index. Anschließend können Sie die Daten in der Tabelle aktualisieren. Wenn Sie den Columnstore-Index deaktivieren, können Sie den Columnstore-Index nach dem Aktualisieren der Daten neu erstellen. Beispiel:  
  
    ```sql  
    ALTER INDEX mycolumnstoreindex ON mytable DISABLE;  
    -- update mytable --  
    ALTER INDEX mycolumnstoreindex on mytable REBUILD  
    ```  
  
-   Laden Sie Daten in eine Stagingtabelle ohne Columnstore-Index. Erstellen Sie einen Columnstore-Index für die Stagingtabelle. Wechseln Sie für die Stagingtabelle in eine leere Partition der Haupttabelle.  
  
-   Wechseln Sie für eine Partition in der Tabelle mit dem Columnstore-Index in eine leere Stagingtabelle. Wenn die Stagingtabelle über einen Columnstore-Index verfügt, deaktivieren Sie den Columnstore-Index. Nehmen Sie die gewünschten Updates vor. Erstellen bzw. erstellen Sie den Columnstore-Index neu. Wechseln Sie für die Stagingtabelle zurück in die (nun leere) Partition der Haupttabelle.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-change-a-clustered-index-to-a-clustered-columnstore-index"></a>A. Ändern eines gruppierten Indexes in einen gruppierten Columnstore-Index  
 Mit der CREATE CLUSTERED COLUMNSTORE INDEX-Anweisung und DROP_EXISTING = ON haben Sie folgende Möglichkeiten:  
  
-   Ändern eines gruppierten Indexes in einen gruppierten Columnstore-Index  
  
-   Neuerstellen eines gruppierten Columnstore-Indexes  
  
 In diesem Beispiel wird die xDimProduct-Tabelle als Rowstore-Tabelle mit einem gruppierten Index erstellt. Anschließend wird die Rowstore-Tabelle mit CREATE CLUSTERED COLUMNSTORE INDEX in eine Columnstore-Tabelle konvertiert.  
  
```sql  
-- Uses AdventureWorks  
  
IF EXISTS (SELECT name FROM sys.tables  
    WHERE name = N'xDimProduct'  
    AND object_id = OBJECT_ID (N'xDimProduct'))  
DROP TABLE xDimProduct;  
  
--Create a distributed table with a clustered index.  
CREATE TABLE xDimProduct (ProductKey, ProductAlternateKey, ProductSubcategoryKey)  
WITH ( DISTRIBUTION = HASH(ProductKey),  
    CLUSTERED INDEX (ProductKey) )  
AS SELECT ProductKey, ProductAlternateKey, ProductSubcategoryKey FROM DimProduct;  
  
--Change the existing clustered index   
--to a clustered columnstore index with the same name.  
--Look up the name of the index before running this statement.  
CREATE CLUSTERED COLUMNSTORE INDEX <index_name>   
ON xdimProduct   
WITH ( DROP_EXISTING = ON );  
```  
  
### <a name="b-rebuild-a-clustered-columnstore-index"></a>B. Neuerstellen eines gruppierten Columnstore-Indexes  
 Aufbauend auf dem vorherigen Beispiel wird in diesem Beispiel der vorhandene gruppierte Columnstore-Index namens „cci_xDimProduct“ mit CREATE CLUSTERED COLUMNSTORE INDEX neu erstellt.  
  
```sql  
--Rebuild the existing clustered columnstore index.  
CREATE CLUSTERED COLUMNSTORE INDEX cci_xDimProduct   
ON xdimProduct   
WITH ( DROP_EXISTING = ON );  
```  
  
### <a name="c-change-the-name-of-a-clustered-columnstore-index"></a>C. Ändern des Namens eines gruppierten Columnstore-Indexes  
 Um den Namen eines gruppierten Columnstore-Indexes zu ändern, löschen Sie den vorhandenen gruppierten Columnstore-Index. Erstellen Sie anschließend den Index mit einem neuen Namen neu.  
  
 Es wird empfohlen, diesen Vorgang nur bei einer kleinen oder leeren Tabelle durchzuführen. Einen umfangreichen gruppierten Columnstore-Index zu löschen und mit einem anderen Namen neu zu erstellen, nimmt viel Zeit in Anspruch.  
  
 In diesem Beispiel wird der gruppierte Columnstore-Index „cci_xDimProduct“ aus dem vorherigen Beispiel gelöscht. Anschließend wird der gruppierte Columnstore-Index mit dem Namen „mycci_xDimProduct“ neu erstellt.  
  
```sql  
--For illustration purposes, drop the clustered columnstore index.   
--The table continues to be distributed, but changes to a heap.  
DROP INDEX cci_xdimProduct ON xDimProduct;  
  
--Create a clustered index with a new name, mycci_xDimProduct.  
CREATE CLUSTERED COLUMNSTORE INDEX mycci_xDimProduct  
ON xdimProduct  
WITH ( DROP_EXISTING = OFF );  
```  
  
### <a name="d-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index"></a>D: Konvertieren einer Columnstore-Tabelle in eine Rowstore-Tabelle mit einem gruppierten Index  
 Es kann vorkommen, dass ein gruppierter Columnstore-Index gelöscht und ein gruppierter Index erstellt werden soll. Dadurch wird die Tabelle im Rowstore-Format gespeichert. In diesem Beispiel wird eine Columnstore-Tabelle in eine Rowstore-Tabelle mit einem gruppierten Index mit demselben Namen konvertiert. Dabei gehen keine Daten verloren. Alle Daten werden in die Rowstore-Tabelle verschoben, und aus den aufgelisteten Spalten werden die Schlüsselspalten im gruppierten Index.  
  
```sql  
--Drop the clustered columnstore index and create a clustered rowstore index.   
--All of the columns are stored in the rowstore clustered index.   
--The columns listed are the included columns in the index.  
CREATE CLUSTERED INDEX cci_xDimProduct    
ON xdimProduct (ProductKey, ProductAlternateKey, ProductSubcategoryKey, WeightUnitMeasureCode)  
WITH ( DROP_EXISTING = ON);  
```  
  
### <a name="e-convert-a-columnstore-table-back-to-a-rowstore-heap"></a>E. Konvertieren einer Columnstore-Tabelle in einen Rowstore-Heap  
 Verwenden Sie [DROP INDEX (SQL Server PDW)](drop-index-transact-sql.md), um den gruppierten Columnstore-Index zu verwerfen und die Tabelle in einen Rowstore-Heap zu konvertieren. In diesem Beispiel wird die Tabelle „cci_xDimProduct“ in einen Rowstore-Heap konvertiert. Die Tabelle wird weiterhin verteilt, jedoch als Heap gespeichert.  
  
```sql  
--Drop the clustered columnstore index. The table continues to be distributed, but changes to a heap.  
DROP INDEX cci_xdimProduct ON xdimProduct;  
```  

### <a name="f-create-an-ordered-clustered-columnstore-index-on-a-table-with-no-index"></a>F. Erstellen eines sortierten gruppierten Columnstore-Index für eine Tabelle ohne Index

```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE );
```

### <a name="g-convert-a-clustered-columnstore-index-to-an-ordered-clustered-columnstore-index"></a>G. Konvertieren eines gruppierten Columnstore-Index in einen sortierten gruppierten Columnstore-Index

```sql  
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE );
WITH (DROP_EXISTING = ON)
```

### <a name="h-add-a-column-to-the-ordering-of-an-ordered-clustered-columnstore-index"></a>H. Hinzufügen einer Spalte zur Reihenfolge eines sortierten gruppierten Columnstore-Index

```sql
-- The original ordered clustered columnstore index was ordered on SHIPDATE column only.  Add PRODUCTKEY column to the ordering.
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE, PRODUCTKEY );
WITH (DROP_EXISTING = ON)
```
### <a name="i-change-the-ordinal-of-ordered-columns"></a>I. Ändern der Ordnungszahl von sortierten Spalten  
```sql
-- The original ordered clustered columnstore index was ordered on SHIPDATE, PRODUCTKEY.  Change the ordering to PRODUCTKEY, SHIPDATE.  
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( PRODUCTKEY,SHIPDATE );
WITH (DROP_EXISTING = ON)
```