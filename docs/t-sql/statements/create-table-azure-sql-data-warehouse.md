---
description: CREATE TABLE (Azure Synapse Analytics)
title: CREATE TABLE (Azure Synapse Analytics) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/03/2019
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: ea21c73c-40e8-4c54-83d4-46ca36b2cf73
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 3358f3d7393b1afa250cfc5f0a953c1ef178f820
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186567"
---
# <a name="create-table-azure-synapse-analytics"></a>CREATE TABLE (Azure Synapse Analytics)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Erstellt eine neue Tabelle in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] oder [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  

Informationen zu Tabellen und deren Verwendung finden Sie unter [Tabellen in[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-overview/).

> [!NOTE]
>  Die Erläuterungen zu [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] in diesem Artikel gelten sowohl für [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] als auch für [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], sofern nicht anders angegeben.

[!INCLUDE[synapse-analytics-od-supported-tables](../../includes/synapse-analytics-od-supported-tables.md)]

 ![Artikellinksymbol](../../database-engine/configure-windows/media/topic-link.gif "Artikellinksymbol") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

<a name="Syntax"></a>

## <a name="syntax"></a>Syntax
  
```syntaxsql
-- Create a new table.
CREATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( 
      { column_name <data_type>  [ <column_options> ] } [ ,...n ]
    )  
    [ WITH ( <table_option> [ ,...n ] ) ]  
[;]  

<column_options> ::=
    [ COLLATE Windows_collation_name ]  
    [ NULL | NOT NULL ] -- default is NULL  
    [ <column_constraint> ]

<column_constraint>::=
    {
        DEFAULT DEFAULT constant_expression
        | PRIMARY KEY NONCLUSTERED  NOT ENFORCED -- Applies to Azure Synapse Analytics only
        | UNIQUE NOT ENFORCED -- Applies to Azure Synapse Analytics only
    }

<table_option> ::=
    {
       CLUSTERED COLUMNSTORE INDEX --default for Azure Synapse Analytics 
      | CLUSTERED COLUMNSTORE INDEX ORDER (column [,...n])  
      | HEAP --default for Parallel Data Warehouse
      | CLUSTERED INDEX ( { index_column_name [ ASC | DESC ] } [ ,...n ] ) -- default is ASC
    }  
    {
        DISTRIBUTION = HASH ( distribution_column_name )
      | DISTRIBUTION = ROUND_ROBIN -- default for Azure Synapse Analytics
      | DISTRIBUTION = REPLICATE -- default for Parallel Data Warehouse
    }
    | PARTITION ( partition_column_name RANGE [ LEFT | RIGHT ] -- default is LEFT  
        FOR VALUES ( [ boundary_value [,...n] ] ) )

<data type> ::=
      datetimeoffset [ ( n ) ]  
    | datetime2 [ ( n ) ]  
    | datetime  
    | smalldatetime  
    | date  
    | time [ ( n ) ]  
    | float [ ( n ) ]  
    | real [ ( n ) ]  
    | decimal [ ( precision [ , scale ] ) ]   
    | numeric [ ( precision [ , scale ] ) ]   
    | money  
    | smallmoney  
    | bigint  
    | int   
    | smallint  
    | tinyint  
    | bit  
    | nvarchar [ ( n | max ) ]  -- max applies only to Azure Synapse Analytics 
    | nchar [ ( n ) ]  
    | varchar [ ( n | max )  ] -- max applies only to Azure Synapse Analytics  
    | char [ ( n ) ]  
    | varbinary [ ( n | max ) ] -- max applies only to Azure Synapse Analytics  
    | binary [ ( n ) ]  
    | uniqueidentifier  
```  

<a name="Arguments"></a>
## <a name="arguments"></a>Argumente

 *database_name*  
 Der Name der Datenbank, die die neue Tabelle enthält. Gemäß Standardeinstellung die aktuelle Datenbank.  
  
 *schema_name*  
 Das Schema der Tabelle. Die Angabe von *schema* ist optional. Wenn keine Angabe gemacht wird, wird das Standardschema verwendet.  
  
 *table_name*  
 Der Name der neuen Tabelle. Stellen Sie dem Tabellennamen das Zeichen # voran, um eine temporäre lokale Tabelle zu erstellen.  Erläuterungen und einen Leitfaden zu temporären Tabellen finden Sie unter [Temporäre Tabellen in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-temporary/). 

 *column_name*  
 Der Name einer Tabellenspalte.

### <a name="column-options"></a><a name="ColumnOptions"></a> Spaltenoptionen

 `COLLATE` *Windows_collation_name*  
 Gibt die Sortierung für den Ausdruck an. Bei der Sortierung muss es sich um eine von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützte Windows-Sortierung handeln. Eine Liste mit von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützten Windows-Sortierungen finden Sie unter [Name der Windows-Sortierung (Transact-SQL)](windows-collation-name-transact-sql.md).  
  
 `NULL` | `NOT NULL`  
 Gibt an, ob `NULL`-Werte in der Spalte zulässig sind. Der Standardwert lautet `NULL`.  
  
 [`CONSTRAINT` *constraint_name*] `DEFAULT` *constant_expression*  
 Gibt den Standardspaltenwert an.  
  
 | Argument | Erklärung |
 | -------- | ----------- |
 | *constraint_name* | Der optionale Name für die Einschränkung. Der Einschränkungsname ist innerhalb der Datenbank eindeutig. Der Name kann in anderen Datenbanken wiederverwendet werden. |
 | *constant_expression* | Der Standardwert für die Spalte. Bei dem Ausdruck muss es sich um einen Literalwert oder eine Konstante handeln. Folgende konstanten Ausdrücke sind beispielsweise zulässig: `'CA'`, `4`. Folgende konstante Ausdrücke sind unzulässig: `2+3`, `CURRENT_TIMESTAMP`. |
  
### <a name="table-structure-options"></a><a name="TableOptions"></a> Tabellenstrukturoptionen

Einen Leitfaden zum Auswählen des Tabellentyps finden Sie unter [Indizieren von Tabellen in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-index/).
  
 `CLUSTERED COLUMNSTORE INDEX` 
 
Speichert die Tabelle als gruppierten Columnstore-Index. Der gruppierte Columnstore-Index gilt für alle Tabellendaten. Dies ist das Standardverhalten für [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].
 
 `HEAP` Speichert die Tabelle als Heap. Dies ist das Standardverhalten für [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
 `CLUSTERED INDEX` ( *index_column_name* [ ,...*n* ] )  
 Speichert die Tabelle als gruppierten Index mit mindestens einer Schlüsselspalte. Durch dieses Verhalten werden die Daten zeilenweise gespeichert. Verwenden Sie *index_column_name*, um den Namen einer oder mehrerer Schlüsselspalten im Index anzugeben.  Weitere Informationen finden Sie im Abschnitt über Rowstore-Tabellen unter den allgemeinen Hinweisen.
 
 `LOCATION = USER_DB` Diese Option ist veraltet. Sie ist syntaktisch zulässig, aber nicht mehr erforderlich, und hat keine Auswirkungen auf das Verhalten.   
  
### <a name="table-distribution-options"></a><a name="TableDistributionOptions"></a> Tabellenverteilungsoptionen

Informationen zum Auswählen der besten Verteilungsmethode und zur Verwendung von verteilten Tabellen finden Sie unter [Verteilen von Tabellen in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-distribute/).

`DISTRIBUTION = HASH` (*distribution_column_name*) Weist jede Zeile einer Verteilung zu, indem für den in *distribution_column_name* gespeicherten Wert ein Hashvorgang durchgeführt wird. Der Algorithmus ist deterministisch. Das bedeutet, er erzeugt für gleiche Verteilungen immer die gleichen Hashwerte.  Die Verteilungsspalte muss als NOT NULL definiert sein, weil alle Zeilen, die NULL enthalten, derselben Verteilung zugewiesen werden.

`DISTRIBUTION = ROUND_ROBIN` Verteilt die Zeilen im Roundrobinverfahren gleichmäßig auf alle Verteilungen. Dies ist das Standardverhalten für [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].

`DISTRIBUTION = REPLICATE` Speichert in jedem Computeknoten eine Kopie der Tabelle. Bei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] wird die Tabelle in einer Verteilungsdatenbank auf den einzelnen Computeknoten gespeichert. Bei [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] wird die Tabelle in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dateigruppe gespeichert, die sich über den gesamten Computeknoten erstreckt. Dies ist das Standardverhalten für [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].
  
### <a name="table-partition-options"></a><a name="TablePartitionOptions"></a> Tabellenpartitionsoptionen
Einen Leitfaden zur Verwendung von Tabellenpartitionen finden Sie unter [Partitionieren von Tabellen in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-partition/).

 `PARTITION` (*partition_column_name*`RANGE` [`LEFT` | `RIGHT`] `FOR VALUES` ([*boundary_value* [,...*n*]]))   
Erstellt eine oder mehrere Tabellenpartitionen. Diese Partitionen sind horizontale Tabellenslices, mit deren Hilfe Sie Vorgänge für Teilmengen von Zeilen ausführen können, unabhängig davon, ob die Tabelle als Heap, gruppierter Index oder gruppierter Columnstore-Index gespeichert ist. Im Gegensatz zur Verteilungsspalte bestimmen Tabellenpartitionen nicht die Verteilung für den Speicherort der einzelnen Zeilen. Vielmehr bestimmten Tabellenpartitionen, wie die Zeilen in den einzelnen Verteilungen gruppiert und gespeichert werden.  

| Argument | Erklärung |
| -------- | ----------- |
|*partition_column_name*| Gibt die Spalte an, die von [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] zum Partitionieren der Zeilen verwendet wird. Diese Spalte kann einen beliebigen Datentyp aufweisen. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] sortiert die Werte der Partitionsspalte in aufsteigender Reihenfolge. Die Sortierung vom niedrigsten zum höchsten Wert erfolgt in der `LEFT`-Spezifikation von `RIGHT` nach `RANGE`. |  
| `RANGE LEFT` | Gibt den Begrenzungswert an, der zur Partition auf der linken Seite (niedrigere Werte) gehört. Die Standardeinstellung ist LEFT. |
| `RANGE RIGHT` | Gibt den Begrenzungswert an, der zur Partition auf der rechten Seite (höhere Werte) gehört. | 
| `FOR VALUES` ( *boundary_value* [,...*n*] ) | Gibt die Begrenzungswerte für die Partition an. *boundary_value* ist ein konstanter Ausdruck. Er darf nicht NULL sein. Er muss entweder dem Datentyp *partition_column_name* entsprechen oder implizit in diesen Datentyp konvertierbar sein. Er darf bei der impliziten Konvertierung nicht abgeschnitten werden, sodass die Größe und Dezimalstellen des Werts nicht mehr dem Datentyp von *partition_column_name* entsprechen.<br></br><br></br>Wenn Sie die `PARTITION`-Klausel, aber keinen Begrenzungswert angeben, erstellt [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] eine partitionierte Tabelle mit einer Partition. Ggf. können Sie die Tabelle später in zwei Partitionen teilen.<br></br><br></br>Wenn Sie einen Begrenzungswert angeben, weist die resultierende Tabelle zwei Partitionen auf, eine für die im Vergleich zum Begrenzungswert niedrigeren Werte und eine für die im Vergleich zum Begrenzungswert höheren Werte. Wenn Sie eine Partition in eine nicht partitionierte Tabelle verschieben, empfängt die nicht partitionierte Tabelle die Daten, jedoch sind in den Metadaten keine Partitionsbegrenzungen enthalten.| 

 Informationen hierzu finden Sie unter [Erstellen einer partitionierten Tabelle](#PartitionedTable) im Abschnitt mit den Beispielen.

### <a name="ordered-clustered-columnstore-index-option"></a>Option für sortierten gruppierten Columnstore-Index 

Gruppierter Columnstore-Index (Clustered Columnstore Index, CCI) ist der Standardwert für das Erstellen von Tabellen in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  Daten im CCI werden nicht sortiert, bevor sie in Spaltensegmente komprimiert werden.  Beim Erstellen eines CCI mit ORDER werden die Daten vor dem Hinzufügen zu Indexsegmenten sortiert, und die Abfrageleistung kann verbessert werden. Weitere Informationen finden Sie unter [Leistungsoptimierung mit einem gruppierten Columnstore-Index](/azure/sql-data-warehouse/performance-tuning-ordered-cci?view=azure-sqldw-latest&preserve-view=true).  

Ein geordneter gruppierter CCI kann für Spalten aller Datentypen (außer Zeichenfolgenspalten) erstellt werden, die in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] unterstützt werden.  

Benutzer können die Spalte **column_store_order_ordinal** in **sys.index_columns** für die Spalte(n) nach der die Tabelle sortiert ist und nach der Sequenz der Sortierung abfragen.  

Weitere Informationen finden Sie unter [Leistungsoptimierung mit einem gruppierten Columnstore-Index](/azure/sql-data-warehouse/performance-tuning-ordered-cci).   

### <a name="data-type"></a><a name="DataTypes"></a> Datentyp

[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] unterstützt die am häufigsten verwendeten Datentypen. Im Folgenden finden Sie eine Liste der unterstützten Datentypen mit entsprechenden Informationen und Angaben zum Speicherplatz in Byte. Ausführlichere Informationen zu Datentypen und deren Verwendung finden Sie unter [Tabellendatentypen in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-data-types).

Eine Tabelle mit Datentypkonvertierungen finden Sie im Abschnitt über implizite Konvertierungen unter [CAST und CONVERT (Transact-SQL)](../functions/cast-and-convert-transact-sql.md).

>[!NOTE]
>Weitere Detailinformationen finden Sie unter [Datums- und Uhrzeitdatentypen und zugehörige Funktionen &#40;Transact-SQL&#41;](../functions/date-and-time-data-types-and-functions-transact-sql.md).

`datetimeoffset` [ ( *n* ) ]  
 Der Standardwert für *n* ist 7.  
  
 `datetime2` [ ( *n* ) ]  
Entspricht `datetime`, jedoch mit der Ausnahme, dass die Anzahl von Sekundenbruchteilen angegeben werden kann. Der Standardwert für *n* ist `7`.  
  
|Wert *n*|Precision|Skalieren|  
|--:|--:|-:|  
|`0`|19|0|  
|`1`|21|1|  
|`2`|22|2|  
|`3`|23|3|  
|`4`|24|4|  
|`5`|25|5|  
|`6`|26|6|  
|`7`|27|7|  
  
 `datetime`  
 Speichert das Datum und die Uhrzeit mit 19 bis 23 Zeichen entsprechend dem gregorianischen Kalender. Das Datum kann das Jahr, den Monat und den Tag enthalten. Die Uhrzeit enthält die Stunde, die Minute und die Sekunde. Optional können drei Ziffern für die Sekundenbruchteile angezeigt werden. Die Speichergröße beträgt 8 Byte.  
  
 `smalldatetime`  
 Speichert ein Datum und eine Uhrzeit. Die Speichergröße beträgt 4 Byte.  
  
 `date`  
 Speichert ein Datum mit maximal 10 Zeichen für das Jahr, den Monat und den Tag gemäß dem gregorianischen Kalender. Die Speichergröße beträgt 3 Byte. Das Datum wird als ganze Zahl gespeichert.  
  
 `time` [ ( *n* ) ]  
 Der Standardwert für *n* ist `7`.  
  
 `float` [ ( *n* ) ]  
 Ungefähre Zahlendatentypen für numerische Gleitkommadaten. Gleitkommadaten sind Näherungswerte. Deshalb können nicht alle Werte im Bereich des Datentyps exakt dargestellt werden. *n* gibt die Anzahl der Bits zum Speichern der Mantisse von `float` in wissenschaftlicher Schreibweise an. Somit gibt *n* die Genauigkeit und die Speichergröße vor. Wenn *n* angegeben ist, muss es sich um einen Wert zwischen `1` und `53` handeln. Der Standardwert von *n* lautet `53`.  
  
| Wert *n* | Precision | Speichergröße |  
| --------: | --------: | -----------: |  
| 1-24   | 7 Stellen  | 4 Byte      |  
| 25-53  | 15 Stellen | 8 Byte      |  
  
 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] verarbeitet *n* als einen von zwei möglichen Werten. Wenn `1`<= *n* <= `24` gegeben ist, wird *n* als `24` behandelt. Wenn `25` <= *n* <= `53` gegeben ist, wird *n* als `53` behandelt.  
  
 Der [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] `float`-Datentyp entspricht dem ISO-Standard für alle Werte von *n* zwischen `1` und `53`. Das Synonym für double precision lautet `float(53)`.  
  
 `real` [ ( *n* ) ]  
 Die Definition von „real“ entspricht der von „float“. Das ISO-Synonym für `real` ist `float(24)`.  
  
 `decimal` [ ( *precision* [ *, scale* ] ) ] | `numeric` [ ( *precision* [ *, scale* ] ) ]  
 Speichert Zahlen mit fester Genauigkeit und mit fester Anzahl von Dezimalstellen.  
  
 *precision*  
 Die maximal speicherbare Gesamtzahl an Dezimalstellen, sowohl links als auch rechts vom Dezimalkomma. Die Genauigkeit muss ein Wert zwischen `1` und der maximalen Genauigkeit von `38` sein. Die Standardgenauigkeit beträgt `18`.  
  
 *scale*  
 Die maximal speicherbare Zahl an Dezimalstellen rechts vom Dezimalkomma. *Scale* muss in einem Bereich zwischen `0` und *precision* liegen. *scale* kann nur angegeben werden, wenn *precision* angegeben wird. Der Standardwert lautet `0`; daher gilt: `0` <= *scale* <= *precision*. Die maximalen Speichergrößen variieren abhängig von der Genauigkeit.  
  
| Precision | Speicherplatz in Bytes  |  
| ---------: |-------------: |  
|  1-9       |             5 |  
| 10–19      |             9 |  
| 20–28      |            13 |  
| 29–38      |            17 |  
  
 `money` | `smallmoney`  
 Datentypen zur Darstellung von Währungswerten.  
  
| Datentyp | Speicherplatz in Bytes |  
| --------- | ------------: |  
| `money`|8|  
| `smallmoney` |4|  
  
 `bigint` \| `int` \| `smallint` \| `tinyint`  
 Exakte Zahlendatentypen für ganzzahlige Daten. Der Speicherplatz wird wie in der folgenden Tabelle dargestellt.  
  
| Datentyp | Speicherplatz in Bytes |  
| --------- | ------------: |  
| `bigint`|8|  
| `int` |4|  
| `smallint` |2|  
| `tinyint` |1|  
  
 `bit`  
 Ein ganzzahliger Datentyp, der den Wert `1`, `0` oder NULL annehmen kann. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] optimiert das Speichern von bit-Spalten. Wenn in einer Tabelle 8 oder weniger bit-Spalten vorhanden sind, werden die Spalten als 1 Byte gespeichert. Sind zwischen 9 und 16 bit-Spalten vorhanden, werden diese als 2 Byte gespeichert usw.  
  
 `nvarchar` [ ( *n* | `max` ) ]  -- `max` gilt nur für [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
 Unicode-Daten variabler Länge. *n* muss ein Wert zwischen 1 und 4000 sein. `max` gibt an, dass die maximale Speichergröße 2^31-1 Byte (2 GB) beträgt. Die Speichergröße in Byte ist doppelt so groß wie die Anzahl eingegebener Zeichen + 2 Byte. Die eingegebenen Daten können null Zeichen lang sein.  
  
 `nchar` [ ( *n* ) ]  
 Unicode-Zeichendaten mit einer festen Länge von *n* Zeichen. *n* muss ein Wert zwischen `1` und `4000` sein. Die Speichergröße beträgt zweimal *n* Byte.  
  
 `varchar` [ ( *n*  | `max` ) ]  -- `max` gilt nur für [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].   
 Nicht-Unicode-Zeichendaten mit einer variablen Länge von *n* Byte. *n* muss ein Wert zwischen `1` und `8000` sein. `max` gibt an, dass die maximale Speichergröße 2^31-1 Byte (2 GB) beträgt. Die Speichergröße ist die tatsächliche Länge der eingegebenen Daten + 2 Byte.  
  
 `char` [ ( *n* ) ]  
 Nicht-Unicode-Zeichendaten mit einer festen Länge von *n* Byte. *n* muss ein Wert zwischen `1` und `8000` sein. Die Speichergröße beträgt *n* Byte. Der Standardwert für *n* lautet `1`.  
  
 `varbinary` [ ( *n*  | `max` ) ]  -- `max` gilt nur für [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
 Binärdaten mit variabler Länge. *n* kann ein Wert zwischen `1` und `8000` sein. `max` gibt an, dass die maximale Speichergröße 2^31-1 Byte (2 GB) beträgt. Die Speichergröße ist die tatsächliche Länge der eingegebenen Daten + 2 Byte. Der Standardwert für *n* ist 7.  
  
 `binary` [ ( *n* ) ]  
 Binärdaten mit einer festen Länge von *n* Byte. *n* kann ein Wert zwischen `1` und `8000` sein. Die Speichergröße beträgt *n* Byte. Der Standardwert für *n* ist `7`.  
  
 `uniqueidentifier`  
 Ein 16-Byte-GUID.  
   
<a name="Permissions"></a>  
## <a name="permissions"></a>Berechtigungen  
 Zum Erstellen einer Tabelle sind Berechtigungen in der festen Datenbankrolle `db_ddladmin` oder folgende Berechtigungen erforderlich:
 - `CREATE TABLE`-Berechtigung für die Datenbank
 - `ALTER SCHEMA`-Berechtigung für das Schema, das die Tabelle enthält. 

Zum Erstellen einer partitionierten Tabelle sind Berechtigungen in der festen Datenbankrolle `db_ddladmin` oder folgende Berechtigungen erforderlich:

- `ALTER ANY DATASPACE`-Berechtigung
  
 Der Anmeldename, der eine lokale temporäre Tabelle erstellt, erhält die Berechtigungen `CONTROL`, `INSERT`, `SELECT` und `UPDATE` für die Tabelle.  
 
<a name="GeneralRemarks"></a>  
## <a name="general-remarks"></a>Allgemeine Hinweise  
 
Informationen zu minimalen und maximalen Limits finden Sie unter [[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]-Kapazitätslimits](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-service-capacity-limits/). 
 
### <a name="determining-the-number-of-table-partitions"></a>Bestimmen der Anzahl der Tabellenpartitionen
Jede benutzerdefinierte Tabelle ist in mehrere kleinere Tabellen aufgeteilt, die in getrennten entfernten Speicherorten, so genannten Verteilungen, gespeichert sind. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] verwendet 60 Verteilungen. Bei [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] hängt die Anzahl der Verteilungen von der Anzahl der Computeknoten ab.
 
Jede Verteilung enthält alle Tabellenpartitionen. Bei 60 Verteilungen und vier Tabellenpartitionen, zuzüglich einer leeren Partition, sind beispielsweise 300 Partitionen (5 x 60 = 300) vorhanden. Wenn es sich bei der Tabelle um einen gruppierten columnstore-Index handelt, gibt es einen columnstore-Index pro Partition und somit 300 columnstore-Indizes.

Es wird empfohlen, weniger Tabellenpartitionen zu verwenden, um sicherzustellen, dass jeder Columnstore-Index genügend Zeilen aufweist, um von den Vorteilen der Columnstore-Indizes zu profitieren. Weitere Informationen finden Sie unter [Partitionieren von Tabellen in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-partition/) und [Indizieren von Tabellen in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-index/).  

### <a name="rowstore-table-heap-or-clustered-index"></a>Rowstore-Tabelle (Heap oder gruppierter Index)

Eine Rowstore-Tabelle ist eine in zeilenweiser Reihenfolge gespeicherte Tabelle. Es handelt sich um einen Heap oder gruppierten Index. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] erstellt alle Rowstore-Tabellen mit Seitenkomprimierung. Dieses Verhalten kann vom Benutzer nicht konfiguriert werden.

### <a name="columnstore-table-columnstore-index"></a>Columnstore-Tabelle (Columnstore Index)

Eine Columnstore-Tabelle ist eine in spaltenweiser Reihenfolge gespeicherte Tabelle. Dabei stellt der Columnstore-Index eine Technologie zum Verwalten von Daten dar, die in einer Columnstore-Tabelle gespeichert sind.  Der gruppierte Columnstore-Index hat keine Auswirkungen auf die Verteilung der Daten. Er bestimmt vielmehr, wie die Daten in den einzelnen Verteilungen gespeichert werden.

Um aus einer Rowstore-Tabelle eine Columnstore-Tabelle zu machen, müssen alle in der Tabelle vorhandenen Indizes gelöscht und ein gruppierter Columnstore-Index erstellt werden. Ein Beispiel hierzu finden Sie unter [CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md).

Weitere Informationen und Beispiele finden Sie in diesen Artikeln:
- [Columnstore-Indizes: Zusammenfassung der Features für Produktversionen](../../relational-databases/indexes/columnstore-indexes-what-s-new.md)
- [Indizieren von Tabellen in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-index/)
- [Beschreibung von Columnstore-Indizes](~/relational-databases/indexes/columnstore-indexes-overview.md) 

<a name="LimitationsRestrictions"></a>  
## <a name="limitations-and-restrictions"></a>Einschränkungen  
 Für eine Verteilungsspalte darf keine DEFAULT-Einschränkung definiert werden.  
  
### <a name="partitions"></a>Partitionen
Bei Verwendung von Partitionen darf die Partitionsspalte keine reine Unicode-Sortierung aufweisen. So schlägt die folgende Anweisung beispielsweise fehl.  
  
 ```sql
CREATE TABLE t1 ( c1 varchar(20) COLLATE Divehi_90_CI_AS_KS_WS) WITH (PARTITION (c1 RANGE FOR VALUES (N'')))
```  
 
 Wenn *boundary_value* ein Literalwert ist, der implizit in den Datentyp in *partition_column_name* konvertiert werden muss, ergibt sich eine Abweichung. Zwar wird über die [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]-Systemsichten der Literalwert angezeigt, für [!INCLUDE[tsql](../../includes/tsql-md.md)]-Vorgänge wird jedoch der konvertierte Wert verwendet. 

### <a name="temporary-tables"></a>Temporäre Tabellen

 Globale temporäre Tabellen, die mit ## beginnen, werden nicht unterstützt.  
  
 Lokale temporäre Tabellen weisen die folgenden Einschränkungen auf:  
  
-   Sie sind nur für die aktuelle Sitzung sichtbar. Am Ende der Sitzung werden sie von [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] automatisch gelöscht. Verwenden Sie zum Löschen die DROP TABLE-Anweisung.   
-   Sie können nicht umbenannt werden. 
-   Sie dürfen keine Partitionen oder Sichten enthalten.  
-   Ihre Berechtigungen können nicht geändert werden. Im Zusammenhang mit lokalen temporären Tabellen können die Anweisungen `GRANT`, `DENY` und `REVOKE` nicht verwendet werden.   
-   Datenbankkonsolenbefehle sind für temporäre Tabellen gesperrt.   
-   Wenn in einem Batch mehrere lokale temporäre Tabellen verwendet werden, muss jede einen eindeutigen Namen aufweisen. Wenn ein Batch von mehreren Sitzungen ausgeführt und dabei jeweils eine lokale temporäre Tabelle erstellt wird, wird von [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] intern ein numerisches Suffix an den Namen der lokalen temporären Tabelle angehängt, sodass jede lokale temporäre Tabelle einen eindeutigen Name aufweist.  
    
<a name="LockingBehavior"></a>   
## <a name="locking-behavior"></a>Sperrverhalten  
 Wendet auf die Tabelle eine exklusive Sperre an. Wendet auf die Objekte DATABASE, SCHEMA und SCHEMARESOLUTION eine gemeinsame Sperre an.  
 

<a name="ExamplesColumn"></a>   
## <a name="examples-for-columns"></a>Beispiele für Spalten

### <a name="a-specify-a-column-collation"></a><a name="ColumnCollation"></a> A. Festlegen einer Spaltensortierung 
 Im folgenden Beispiel wird die Tabelle `MyTable` mit zwei verschiedenen Spaltensortierungen erstellt. Standardmäßig weist die Spalte `mycolumn1` die Standardsortierung Latin1_General_100_CI_AS_KS_WS auf. Die Spalte `mycolumn2` weist die Sortierung Frisian_100_CS_AS auf.  
  
```sql
CREATE TABLE MyTable   
  (  
    mycolumnnn1 nvarchar,  
    mycolumn2 nvarchar COLLATE Frisian_100_CS_AS )  
WITH ( CLUSTERED COLUMNSTORE INDEX )  
;  
```  
  
### <a name="b-specify-a-default-constraint-for-a-column"></a><a name="DefaultConstraint"></a> B. Festlegen einer DEFAULT-Einschränkung für eine Spalte

 Im folgenden Beispiel ist die Syntax zum Festlegen eines Standardwerts für eine Spalte dargestellt. Die Spalte colA weist eine Standardeinschränkung mit dem Namen constraint_colA und den Standardwert 0 auf.  
  
```sql
CREATE TABLE MyTable
  (  
    colA int CONSTRAINT constraint_colA DEFAULT 0,  
    colB nvarchar COLLATE Frisian_100_CS_AS
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX )  
;  
```

<a name="ExamplesTemporaryTables"></a> 
## <a name="examples-for-temporary-tables"></a>Beispiele für temporäre Tabellen

### <a name="c-create-a-local-temporary-table"></a><a name="TemporaryTable"></a> C. Erstellen einer lokalen temporären Tabelle  
 Im folgenden Beispiel wird eine lokale temporäre Tabelle namens #myTable erstellt. In der Tabelle wird mit einem dreiteiligen Namen angegeben, der mit einem # beginnt.
  
```sql
CREATE TABLE AdventureWorks.dbo.#myTable
  (  
   id int NOT NULL,  
   lastName varchar(20),  
   zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = HASH (id),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```

<a name="ExTableStructure"></a>  
## <a name="examples-for-table-structure"></a>Beispiele für die Tabellenstruktur

### <a name="d-create-a-table-with-a-clustered-columnstore-index"></a><a name="ClusteredColumnstoreIndex"></a> D. Erstellen einer Tabelle mit einem gruppierten Columnstore-Index  
 Im folgende Beispiel wird eine verteilte Tabelle mit einem gruppierten Columnstore-Index erstellt. Jede Verteilung wird als Columnstore gespeichert.  
  
 Der gruppierte Columnstore-Index hat keine Auswirkung auf die Verteilung der Daten, da die Daten immer zeilenweise verteilt werden. Der gruppierte Columnstore-Index bestimmt jedoch, wie die Daten in den einzelnen Verteilungen gespeichert werden.  
  
```sql
  CREATE TABLE MyTable
  (  
    colA int CONSTRAINT constraint_colA DEFAULT 0,  
    colB nvarchar COLLATE Frisian_100_CS_AS
  )  
WITH   
  (   
    DISTRIBUTION = HASH ( colB ),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```  

### <a name="e-create-an-ordered-clustered-columnstore-index"></a><a name="OrderedClusteredColumnstoreIndex"></a> E. Erstellen eines sortierten gruppierten Columnstore-Index

Das folgende Beispiel zeigt, wie ein sortierter gruppierter Columnstore-Index erstellt wird. Der Index ist nach SHIPDATE sortiert.

```sql
CREATE TABLE Lineitem  
WITH (DISTRIBUTION = ROUND_ROBIN, CLUSTERED COLUMNSTORE INDEX ORDER(SHIPDATE))  
AS  
SELECT * FROM ext_Lineitem
```

<a name="ExTableDistribution"></a> 
## <a name="examples-for-table-distribution"></a>Beispiele für die Tabellenverteilung

### <a name="f-create-a-round_robin-table"></a><a name="RoundRobin"></a> F. Erstellen einer ROUND_ROBIN-Tabelle  
 Im folgenden Beispiel wird eine ROUND_ROBIN-Tabelle mit drei Spalten und ohne Partitionen erstellt. Die Daten werden in allen Verteilungen verteilt. Die Tabelle wird mit einem gruppierten Columnstore-Index erstellt, der im Vergleich zu einem Heap oder einem gruppierten Rowstore-Index eine bessere Leistung und Datensicherung gewährleistet.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );  
```  
  
### <a name="g-create-a-hash-distributed-table"></a><a name="HashDistributed"></a> G. Erstellen einer mittels Hash verteilten Tabelle

 Im folgenden Beispiel wird dieselbe Tabelle wie im vorherigen Beispiel erstellt. Allerdings werden bei dieser Tabelle die Zeilen (in der `id`-Spalte) gezielt verteilt statt wie bei einer ROUND_ROBIN-Tabelle zufällig verteilt. Die Tabelle wird mit einem gruppierten Columnstore-Index erstellt, der im Vergleich zu einem Heap oder einem gruppierten Rowstore-Index eine bessere Leistung und Datensicherung gewährleistet.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = HASH (id),   
    CLUSTERED COLUMNSTORE INDEX  
  );  
```  
  
### <a name="h-create-a-replicated-table"></a><a name="Replicated"></a> H. Erstellen einer replizierten Tabelle  
 Im folgenden Beispiel wird eine replizierte Tabelle ähnlich wie im vorherigen Beispiel erstellt. Replizierte Tabellen werden vollständig auf alle Computeknoten kopiert. Dank dieser Kopie auf allen Computeknoten wird die Anzahl der Datenverschiebungen für Abfragen reduziert. Dieses Beispiel wird mit einem CLUSTERED INDEX erstellt, wodurch eine bessere Datenkomprimierung als bei einem Heap erzielt wird. Ein Heap enthält möglicherweise nicht genügend Zeilen, um eine gute CLUSTERED COLUMNSTORE INDEX-Komprimierung zu erreichen.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = REPLICATE,
    CLUSTERED INDEX (lastName)  
  );  
```  

<a name="ExTablePartitions"></a> 
## <a name="examples-for-table-partitions"></a>Beispiele für Tabellenpartitionen

###  <a name="i-create-a-partitioned-table"></a><a name="PartitionedTable"></a> I. Erstellen einer partitionierten Tabelle

 Im folgenden Beispiel wird dieselbe Tabelle wie in Beispiel A erstellt, jedoch mit einer RANGE LEFT-Partitionierung für die `id`-Spalte. Es werden vier Partitionsbegrenzungswerte angegeben, sodass sich fünf Partitionen ergeben.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode int)  
WITH
  (
  
    PARTITION ( id RANGE LEFT FOR VALUES (10, 20, 30, 40 )),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```  
  
 In diesem Beispiel werden die Daten in die folgenden Partitionen sortiert:  
  
- Partition 1: col <= 10
- Partition 2: 10 < col <= 20
- Partition 3: 20 < col <= 30
- Partition 4: 30 < col <= 40
- Partition 5: 40 < col  
  
 Wenn diese Tabelle statt mit RANGE LEFT (Standardeinstellung) mit RANGE RIGHT partitioniert wird, werden die Daten in die folgenden Partitionen sortiert:  
  
- Partition 1: col < 10  
- Partition 2: 10 <= col < 20
- Partition 3: 20 <= col < 30
- Partition 4: 30 <= col < 40
- Partition 5: 40 <= col  
  
### <a name="j-create-a-partitioned-table-with-one-partition"></a><a name="OnePartition"></a> J. Erstellen einer partitionierten Tabelle mit einer Partition

 Im folgende Beispiel wird eine partitionierte Tabelle mit einer Partition erstellt. Es wird kein Begrenzungswert angegeben, sodass sich nur eine Partition ergibt.  
  
```sql
CREATE TABLE myTable (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode int)  
WITH
    (
      PARTITION ( id RANGE LEFT FOR VALUES ( )),  
      CLUSTERED COLUMNSTORE INDEX  
    )  
;  
```  
  
### <a name="k-create-a-table-with-date-partitioning"></a><a name="DatePartition"></a> K. Erstellen einer Tabelle mit Datumspartitionierung

 Im folgenden Beispiel wird eine neue Tabelle mit dem Namen `myTable` mit Partitionierung in einer `date`-Spalte erstellt. Durch die Verwendung von RANGE RIGHT und Datumsangaben für die Begrenzungswerte werden in alle Partitionen die Daten eines Monats eingefügt.  
  
```sql
CREATE TABLE myTable (  
    l_orderkey      bigint,
    l_partkey       bigint,
    l_suppkey       bigint,
    l_linenumber    bigint,
    l_quantity      decimal(15,2),  
    l_extendedprice decimal(15,2),  
    l_discount      decimal(15,2),  
    l_tax           decimal(15,2),  
    l_returnflag    char(1),  
    l_linestatus    char(1),  
    l_shipdate      date,  
    l_commitdate    date,  
    l_receiptdate   date,  
    l_shipinstruct  char(25),  
    l_shipmode      char(10),  
    l_comment       varchar(44))  
WITH
  (
    DISTRIBUTION = HASH (l_orderkey),  
    CLUSTERED COLUMNSTORE INDEX,  
    PARTITION ( l_shipdate  RANGE RIGHT FOR VALUES
      (  
        '1992-01-01','1992-02-01','1992-03-01','1992-04-01','1992-05-01',
        '1992-06-01','1992-07-01','1992-08-01','1992-09-01','1992-10-01',
        '1992-11-01','1992-12-01','1993-01-01','1993-02-01','1993-03-01',
        '1993-04-01','1993-05-01','1993-06-01','1993-07-01','1993-08-01',
        '1993-09-01','1993-10-01','1993-11-01','1993-12-01','1994-01-01',
        '1994-02-01','1994-03-01','1994-04-01','1994-05-01','1994-06-01',
        '1994-07-01','1994-08-01','1994-09-01','1994-10-01','1994-11-01',
        '1994-12-01'  
      ))
  );  
```  
  
<a name="SeeAlso"></a>
## <a name="see-also"></a>Weitere Informationen
 
[CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)   
[DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
[sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md) 
