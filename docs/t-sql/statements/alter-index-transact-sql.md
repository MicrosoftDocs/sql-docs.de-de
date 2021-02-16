---
description: ALTER INDEX (Transact-SQL)
title: ALTER INDEX (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER INDEX
- ALTER_INDEX_TSQL
dev_langs:
- t-sql
helpviewer_keywords:
- indexes [SQL Server], reorganizing
- ALTER INDEX statement
- indexes [SQL Server], disabling
- online index operations
- index reorganization [SQL Server]
- ALLOW_ROW_LOCKS option
- ALL keyword
- reorganizing indexes
- constraints [SQL Server], indexes
- row locks [SQL Server]
- index rebuilding [SQL Server]
- rebuilding indexes
- locking [SQL Server], indexes
- partitioned indexes [SQL Server], rebuilding
- defragmenting indexes
- disabling indexes
- XML indexes [SQL Server], modifying
- index modifications [SQL Server]
- indexes [SQL Server], modifying
- index options [SQL Server]
- modifying indexes
- index disabling [SQL Server]
- MAXDOP index option, ALTER INDEX statement
- spatial indexes [SQL Server], modifying
- indexes [SQL Server], options
- ALLOW_PAGE_LOCKS option
- page locks [SQL Server]
- index rebuild [SQL Server]
- index reorganize [SQL Server]
ms.assetid: b796c829-ef3a-405c-a784-48286d4fb2b9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2ec8737d01b52db828dbe95cd1252d8913128c90
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339317"
---
# <a name="alter-index-transact-sql"></a>ALTER INDEX (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Ändert einen vorhandenen Tabellen- oder Sichtindex (Rowstore, Columnstore oder XML), indem der Index deaktiviert, neu erstellt oder neu organisiert wird oder indem Optionen für den Index festgelegt werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database
  
ALTER INDEX { index_name | ALL } ON <object>  
{  
      REBUILD {  
            [ PARTITION = ALL ] [ WITH ( <rebuild_index_option> [ ,...n ] ) ]   
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_index_option> ) [ ,...n ] ]  
      }  
    | DISABLE  
    | REORGANIZE  [ PARTITION = partition_number ] [ WITH ( <reorganize_option>  ) ]  
    | SET ( <set_index_option> [ ,...n ] )   
    | RESUME [WITH (<resumable_index_options>,[...n])]
    | PAUSE
    | ABORT
}  
[ ; ]  
  
<object> ::=   
{  
    { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
}  
  
<rebuild_index_option > ::=  
{  
      PAD_INDEX = { ON | OFF }  
    | FILLFACTOR = fillfactor   
    | SORT_IN_TEMPDB = { ON | OFF }  
    | IGNORE_DUP_KEY = { ON | OFF }  
    | STATISTICS_NORECOMPUTE = { ON | OFF }  
    | STATISTICS_INCREMENTAL = { ON | OFF }  
    | ONLINE = {   
          ON [ ( <low_priority_lock_wait> ) ]   
        | OFF } 
    | RESUMABLE = { ON | OFF } 
    | MAX_DURATION = <time> [MINUTES}     
    | ALLOW_ROW_LOCKS = { ON | OFF }  
    | ALLOW_PAGE_LOCKS = { ON | OFF }  
    | MAXDOP = max_degree_of_parallelism  
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }   
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]  
}  
  
<single_partition_rebuild_index_option> ::=  
{  
      SORT_IN_TEMPDB = { ON | OFF }  
    | MAXDOP = max_degree_of_parallelism  
    | RESUMABLE = { ON | OFF } 
    | MAX_DURATION = <time> [MINUTES}     
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE } }  
    | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<reorganize_option>::=  
{  
       LOB_COMPACTION = { ON | OFF }  
    |  COMPRESS_ALL_ROW_GROUPS =  { ON | OFF}  
}  
  
<set_index_option>::=  
{  
      ALLOW_ROW_LOCKS = { ON | OFF }  
    | ALLOW_PAGE_LOCKS = { ON | OFF }  
    | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
    | IGNORE_DUP_KEY = { ON | OFF }  
    | STATISTICS_NORECOMPUTE = { ON | OFF }  
    | COMPRESSION_DELAY= { 0 | delay [Minutes] }  
}  

<resumable_index_option> ::=
 { 
    MAXDOP = max_degree_of_parallelism
    | MAX_DURATION =<time> [MINUTES]
    | <low_priority_lock_wait>  
 }
 
<low_priority_lock_wait>::=  
{  
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ] ,   
                          ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )  
}  

```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse 
  
ALTER INDEX { index_name | ALL }  
    ON   [ schema_name. ] table_name  
{  
      REBUILD {  
            [ PARTITION = ALL [ WITH ( <rebuild_index_option> ) ] ] 
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_index_option> )] ] 
      }  
    | DISABLE  
    | REORGANIZE [ PARTITION = partition_number ]  
}  
[;]  

<rebuild_index_option > ::=   
{  
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]   
}

<single_partition_rebuild_index_option > ::=   
{  
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
}  
  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

 *index_name*  
 Der Name des Indexes. Indexnamen müssen für eine Tabelle oder Sicht eindeutig sein, können aber innerhalb einer Datenbank mehrfach vorkommen. Indexnamen müssen den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen.  
  
 ALL  
 Gibt alle Indizes an, die unabhängig vom Indextyp der Tabelle oder Sicht zugeordnet sind. Die Angabe von ALL verursacht bei der Anweisung einen Fehler, wenn mindestens ein Index in einer Offlinedateigruppe oder schreibgeschützten Dateigruppe enthalten ist oder der angegebene Vorgang für mindestens einen Indextyp nicht zulässig ist. In der folgenden Tabelle werden die Indexvorgänge und die nicht zulässigen Indextypen aufgelistet.  
  
|Verwendet bei diesem Vorgang das Schlüsselwort ALL|Erzeugt einen Fehler, wenn mindestens einer dieser Indextypen in der Tabelle enthalten ist|  
|----------------------------------------|----------------------------------------|  
|REBUILD WITH ONLINE = ON|XML-Index<br /><br /> Räumlicher Index<br /><br /> Columnstore-Index: **Anwendungsbereich:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|REBUILD PARTITION = *partition_number*|Nicht partitionierter Index, XML-Index, räumlicher Index oder deaktivierter Index|  
|REORGANIZE|Indizes, für die ALLOW_PAGE_LOCKS auf OFF festgelegt wurde|  
|REORGANIZE PARTITION = *partition_number*|Nicht partitionierter Index, XML-Index, räumlicher Index oder deaktivierter Index|  
|IGNORE_DUP_KEY = ON|XML-Index<br /><br /> Räumlicher Index<br /><br /> Columnstore-Index: **Anwendungsbereich:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|ONLINE = ON|XML-Index<br /><br /> Räumlicher Index<br /><br /> Columnstore-Index: **Anwendungsbereich:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|RESUMABLE = ON| Fortsetzbare Indizes werden nicht unterstützt für Schlüsselwort **ALL**. <br /><br /> **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |   
  
> [!WARNING]
> Ausführlichere Informationen zu Indexvorgängen, die online ausgeführt werden können, finden Sie unter [Richtlinien für Onlineindexvorgänge](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

 Wenn ALL mit PARTITION = *partition_number* angegeben ist, müssen alle Indizes ausgerichtet sein. Das bedeutet, dass sie auf der Grundlage der entsprechenden Partitionsfunktionen partitioniert sind. Das Verwenden von ALL mit PARTITION hat zur Folge, dass alle Indexpartitionen mit demselben Wert für *partition_number* neu erstellt oder neu organisiert werden. Weitere Informationen zu partitionierten Indizes finden Sie unter [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 *database_name*  
 Der Name der Datenbank.  
  
 *schema_name*  
 Der Name des Schemas, zu dem die Tabelle oder Sicht gehört.  
  
 *table_or_view_name*  
 Der Name der Tabelle oder Sicht, die dem Index zugeordnet ist. Verwenden Sie zum Anzeigen eines Berichts über die Indizes zu einem Objekt die [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)-Katalogsicht.  
  
 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] unterstützt das dreiteilige Namensformat „database_name.[schema_name].table_or_view_name“, wenn „database_name“ die aktuelle Datenbank bzw. „tempdb“ ist und „table_or_view_name“ mit # beginnt.  
  
 REBUILD [ WITH **(** \<rebuild_index_option> [ **,** ... *n*] **)** ]  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Gibt an, dass der Index mit denselben Spalten, demselben Indextyp, demselben Eindeutigkeitsattribut und derselben Sortierreihenfolge neu erstellt wird. Diese Klausel entspricht [DBCC DBREINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md). Mit REBUILD wird ein deaktivierter Index aktiviert. Durch das Neuerstellen eines gruppierten Indexes erfolgt nicht die Neuerstellung von zugeordneten nicht gruppierten Indizes, es sei denn, das Schlüsselwort ALL ist angegeben. Wenn keine Indexoptionen angegeben sind, werden die vorhandenen Werte der Indexoptionen angewendet, die in [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) gespeichert sind. Für alle Indexoptionen, deren Werte nicht in **sys.indexes** gespeichert sind, wird der Standardwert angewendet, der in der Argumentdefinition der Option angegeben ist.  
  
 Wenn ALL angegeben ist und die zugrunde liegende Tabelle ein Heap ist, hat die Neuerstellung keine Auswirkungen auf die Tabelle. Alle nicht gruppierten Indizes, die der Tabelle zugeordnet sind, werden neu erstellt.  
  
 Der Neuerstellungsvorgang kann minimal protokolliert werden, wenn die Datenbankwiederherstellung auf das massenprotokollierte oder einfache Wiederherstellungsmodell festgelegt ist.  
  
> [!NOTE]
> Wenn Sie einen primären XML-Index neu erstellen, ist die zugrunde liegende Benutzertabelle während des Indexvorgangs nicht verfügbar.  
 
Für Columnstore-Indizes wird durch den Neuerstellungsvorgang Folgendes ausgelöst:  
  
-  Sortierreihenfolge wird nicht verwendet.  
-  Abrufen einer exklusiven Sperre für die Tabelle oder Partition, während die Neuerstellung ausgeführt wird.  Die Daten sind während der Neuerstellung „offline“ und nicht verfügbar, sogar wenn Sie NOLOCK, RCSI oder SI verwenden.  
-  Neukomprimierung aller Daten im Columnstore. Während die Neuerstellung ausgeführt wird, gibt es zwei Kopien des columnstore-Indexes. Nach Abschluss der Neuerstellung wird der ursprüngliche columnstore-Index in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gelöscht.  

Weitere Informationen finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
PARTITION  

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Gibt an, dass nur eine Partition eines Indexes neu erstellt oder neu organisiert wird. PARTITION kann nicht angegeben werden, wenn *index_name* kein partitionierter Index ist.  
  
 PARTITION = ALL erstellt alle Partitionen neu.  
  
> [!WARNING]
> Das Erstellen bzw. Neuerstellen von nicht ausgerichteten Indizes für eine Tabelle mit mehr als 1.000 Partitionen ist möglich, wird aber nicht unterstützt. Dies hätte Leistungseinbußen oder eine zu hohe Speicherauslastung während der Vorgänge zur Folge. Microsoft empfiehlt, bei mehr als 1.000 Partitionen nur ausgerichtete Indizes zu verwenden.  
  
 *partition_number*  
   
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Die Partitionsnummer eines partitionierten Indexes, der neu erstellt oder neu organisiert werden soll. *partition_number* ist ein konstanter Ausdruck, der auf Variablen verweisen kann. Bei diesen Variablen kann es sich um Funktionen und benutzerdefinierte Variablen oder Funktionen handeln, die jedoch nicht auf eine [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung verweisen können. *partition_number* muss vorhanden sein. Andernfalls schlägt die Anweisung fehl.  
  
 WITH **(** \<single_partition_rebuild_index_option> **)**  
   
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Die Optionen, die für das Neuerstellen einer einzelnen Partition (PARTITION = `SORT_IN_TEMPDB`n`MAXDOP`) angegeben werden können, lauten `DATA_COMPRESSION`, *und*. XML-Indizes können nicht bei der Neuerstellung einer einzelnen Partition angegeben werden.  
  
 DISABLE  
 Markiert den Index als deaktiviert und als nicht verfügbar für das [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Jeder Index kann deaktiviert werden. Die Indexdefinition eines deaktivierten Indexes bleibt weiterhin im Systemkatalog ohne zugrunde liegende Indexdaten bestehen. Durch das Deaktivieren eines gruppierten Indexes wird der Benutzerzugriff auf die zugrunde liegenden Tabellendaten verhindert. Verwenden Sie ALTER INDEX REBUILD oder CREATE INDEX WITH DROP_EXISTING, um einen Index zu aktivieren. Weitere Informationen finden Sie unter [Deaktivieren von Indizes und Einschränkungen](../../relational-databases/indexes/disable-indexes-and-constraints.md) und [Aktivieren von Indizes und Einschränkungen](../../relational-databases/indexes/enable-indexes-and-constraints.md).  
  
 REORGANIZE bei einem **Rowstore**-Index  
 Gibt für Rowstore-Indizes an, dass die Indexblattebene neu organisiert wird. Für den REORGANIZE-Vorgang gilt:  
  
-   Wird immer online ausgeführt. Das bedeutet, dass blockierende Langzeitsperren für Tabellen nicht aufrechterhalten werden und Abfragen oder Updates der zugrunde liegenden Tabelle während der ALTER INDEX REORGANIZE-Transaktion fortgesetzt werden können.
-   Nicht zulässig für einen deaktivierten Index
-   Nicht zulässig, wenn ALLOW_PAGE_LOCKS auf OFF festgelegt ist
-   Es wird kein Rollback durchgeführt, wenn er innerhalb einer Transaktion ausgeführt wird, für die ein Rollback durchgeführt wird.

> [!NOTE]
> Wenn ALTER INDEX REORGANIZE explizite Transaktionen (z. B. ALTER INDEX in BEGIN TRAN ... COMMIT/ROLLBACK) anstelle des impliziten Standardtransaktionsmodus verwendet, ist das Sperrverhalten von REORGANIZE einschränkender, was möglicherweise zu Blockierungen führt. Weitere Informationen über implizite Transaktionen finden Sie unter [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../t-sql/statements/set-implicit-transactions-transact-sql.md).

Weitere Informationen finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 

REORGANIZE WITH **(** LOB_COMPACTION = { **ON** | OFF } **)**  
 Gilt für Rowstore-Indizes.  
  
LOB_COMPACTION = ON  
  
-   Gibt an, dass alle Seiten komprimiert werden, die Daten der folgenden LOB-Datentypen (Large Objects) enthalten: image, text, ntext, varchar(max), nvarchar(max), varbinary(max) und xml. Durch das Komprimieren dieser Daten kann die Datenmenge auf der Festplatte verringert werden.  
-   Bei einem gruppierten Index werden dadurch alle LOB-Spalten komprimiert, die in der Tabelle enthalten sind.  
-   Bei einem nicht gruppierten Index werden alle LOB-Spalten komprimiert, die (eingeschlossene) Nichtschlüsselspalten im Index sind.  
-   Mit REORGANIZE ALL wird LOB_COMPACTION für alle Indizes ausgeführt. Bei jedem Index werden alle LOB-Spalten im gruppierten Index, in der zugrunde liegenden Tabelle oder in eingeschlossenen Spalten in einem nicht gruppierten Index komprimiert.  
  
LOB_COMPACTION = OFF  
  
-   Seiten, die LOB-Daten enthalten, werden nicht komprimiert.  
-   Die Einstellung OFF hat keine Auswirkungen auf einen Heap.  
  
 REORGANIZE bei einem **Columnstore**-Index  
 Bei Columnstore-Indizes wird durch REORGANIZE jede CLOSED-Deltazeilengruppe im Columnstore als eine komprimierte Zeilengruppe komprimiert. Der REORGANIZE-Vorgang wird immer online ausgeführt. Das bedeutet, dass blockierende Langzeitsperren für Tabellen nicht aufrechterhalten werden und Abfragen oder Updates der zugrunde liegenden Tabelle während der ALTER INDEX REORGANIZE-Transaktion fortgesetzt werden können. Weitere Informationen finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
-   REORGANIZE ist für das Verschieben von CLOSED-Delta-Zeilengruppen in komprimierte Zeilengruppen nicht erforderlich. Der im Hintergrund ausgeführte Tupelverschiebungsvorgang (TM, Tuple Mover) wird in bestimmten Zeitabständen reaktiviert, um CLOSED-Delta-Zeilengruppen zu komprimieren. Es wird empfohlen, REORGANIZE zu verwenden, wenn TM im Rückstand ist. Mit REORGANIZE können Zeilengruppen aggressiver komprimiert werden.  
-   Informationen zum Komprimieren aller OPEN- und CLOSED-Zeilengruppen finden Sie unter der `REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS)`-Option in diesem Abschnitt.  
  
Bei Columnstore-Indizes in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] werden mit REORGANIZE die folgenden Optimierungen für eine zusätzliche Defragmentierung online ausgeführt:  
  
-   Es werden Zeilen physisch aus der Zeilengruppe entfernt, wenn mindestens 10 % der Zeilen logisch gelöscht wurden. Die gelöschten Bytes werden auf den physischen Medien freigegeben. Bei einer komprimierten Zeilengruppe mit 1 Million Zeilen, in der beispielsweise 100.000 Zeilen gelöscht wurden, werden von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die gelöschten Zeilen entfernt, und die Zeilengruppe wird mit 900.000 Zeilen neu komprimiert. Durch das Entfernen gelöschter Zeilen wird Speicherplatz eingespart.  
  
-   Eine oder mehrere komprimierte Zeilengruppen werden kombiniert, um die Anzahl der Zeilen pro Zeilengruppe auf maximal 1.048.576 Zeilen zu erhöhen. Bei einem Massenexport von 5 Batches mit 102.400 Zeilen erhalten Sie beispielsweise 5 komprimierte Zeilengruppen. Wenn Sie REORGANIZE ausführen, werden diese Zeilengruppen in eine komprimierte Zeilengruppe mit 512.000 Zeilen zusammengeführt. Dies setzt voraus, dass keine Wörterbuchumfangsbegrenzungen oder Arbeitsspeichereinschränkungen vorhanden sind.  
  
-   Zeilengruppen, in denen mindestens 10 % der Zeilen logisch gelöscht wurden, versucht [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit einer oder mehreren Zeilengruppen zu kombinieren. Beispiel: Zeilengruppe 1 ist mit 500.000 Zeilen komprimiert und Zeilengruppe 21 mit dem Maximalwert von 1.048.576 Zeilen. In Zeilengruppe 21 wurden 60 % der Zeilen gelöscht, wodurch noch 409.830 Zeilen vorhanden sind. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden diese beiden Zeilengruppen vorzugsweise kombiniert, um eine neue Zeilengruppe mit 909.830 Zeilen zu komprimieren.  
  
REORGANIZE WITH ( COMPRESS_ALL_ROW_GROUPS = { ON | **OFF** } )  
 Gilt für Columnstore-Indizes. 

 **Anwendungsbereich:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Mit COMPRESS_ALL_ROW_GROUPS kann die Übernahme von OPEN- oder CLOSED-Delta-Zeilengruppen in den Columnstore erzwungen werden. Mit dieser Option ist es nicht notwendig, den Columnstore-Index zum Leeren der Delta-Zeilengruppe neu zu erstellen.  Damit sowie durch die anderen Defragmentierungsfeatures zum Entfernen und Zusammenfügen von Zeilengruppen ist es in den meisten Fällen nicht mehr erforderlich, den Index neu zu erstellen.    

-   Mit ON wird das Einfügen aller Zeilengruppen in den Columnstore erzwungen, unabhängig von ihrer Größe und ihrem Status (CLOSED oder OPEN).  
-   Mit der Einstellung OFF wird die Übernahme aller CLOSED-Zeilengruppen in den Columnstore erzwungen.  

Weitere Informationen finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 

SET **(** \<set_index option> [ **,** ... *n*] **)**  
 Gibt Indexoptionen ohne das Neuerstellen oder Neuorganisieren des Indexes an. SET kann für einen deaktivierten Index nicht angegeben werden.  
  
PAD_INDEX = { ON | OFF }  
   
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  

 Gibt die Auffüllung von Indizes an. Der Standardwert ist OFF.  
  
 EIN  
 Der Prozentsatz des mit FILLFACTOR angegebenen freien Speicherplatzes wird für die Zwischenebenenseiten des Indexes angewendet. Wenn FILLFACTOR nicht zu dem Zeitpunkt angegeben wird, an dem PAD_INDEX auf ON festgelegt wird, wird der in [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) gespeicherte Füllfaktorwert verwendet.  
  
 OFF oder *fillfactor* ist nicht angegeben  
 Die Zwischenebenenseiten werden nahezu vollständig gefüllt. Dabei bleibt genügend Platz für mindestens eine Zeile der maximal zulässigen Größe eines Indexes erhalten. Dies erfolgt auf der Grundlage des Schlüsselsatzes in den Zwischenseiten.  
  
 Weitere Informationen finden Sie unter [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
FILLFACTOR = *fillfactor*  
 
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Gibt einen Prozentwert an, der dem Füllfaktor entspricht. Dieser Faktor legt fest, wie weit die [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Blattebene jeder Indexseite während der Indexerstellung oder -änderung auffüllen soll. *fillfactor* muss ein ganzzahliger Wert zwischen 1 und 100 sein. Die Standardeinstellung ist 0. Die Füllfaktorwerte 0 und 100 sind in jeder Hinsicht identisch.  
  
 Eine explizite FILLFACTOR-Einstellung gilt nur bei der erstmaligen Erstellung oder bei der Neuerstellung des Indexes. [!INCLUDE[ssDE](../../includes/ssde-md.md)] hält den angegebenen Prozentsatz des Speicherplatzes nicht dynamisch auf den Seiten frei. Weitere Informationen finden Sie unter [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
 Verwenden Sie zum Anzeigen der Füllfaktoreinstellung **sys.indexes**.  
  
> [!IMPORTANT]
> Das Erstellen oder Ändern eines gruppierten Indexes mit einem FILLFACTOR-Wert wirkt sich auf den Speicherplatz aus, den die Daten belegen, da das [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Daten beim Erstellen des gruppierten Indexes neu verteilt.  
  
 SORT_IN_TEMPDB = { ON | **OFF** }  
 
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Gibt an, ob die Ergebnisse der Sortierung in **tempdb** gespeichert werden sollen. Außer für Azure SQL-Datenbank Hyperscale ist der Standardwert OFF. Für alle Indexerstellungsvorgänge in Hyperscale ist SORT_IN_TEMPDB unabhängig von der angegebenen Option immer ON, sofern nicht die fortsetzbare Indexneuerstellung verwendet wird.  
  
 EIN  
 Die Zwischenergebnisse von Sortierungen, mit denen der Index erstellt wird, werden in **tempdb** gespeichert. Wenn sich **tempdb** auf einem anderen Datenträgersatz befindet als die Benutzerdatenbank, kann die zum Erstellen eines Index erforderliche Zeit reduziert werden. Sie erhöht jedoch den Betrag an Speicherplatz, der während der Indexerstellung verwendet wird.  
  
 OFF  
 Die Zwischenergebnisse der Sortierung werden in derselben Datenbank gespeichert wie der Index.  
  
 Wenn kein Sortierungsvorgang erforderlich ist oder wenn die Sortierung im Arbeitsspeicher ausgeführt werden kann, wird die SORT_IN_TEMPDB-Option ignoriert.  
  
 Weitere Informationen finden Sie unter [SORT_IN_TEMPDB-Option für Indizes](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).  
  
 IGNORE_DUP_KEY **=** { ON | OFF }  
 Gibt die Fehlermeldung an, wenn ein Einfügevorgang versucht, doppelte Schlüsselwerte in einen eindeutigen Index einzufügen. Die IGNORE_DUP_KEY-Option gilt nur für Einfügevorgänge nach dem Erstellen oder Neuerstellen des Index. Der Standardwert ist OFF.  
  
 EIN  
 Eine Warnmeldung wird ausgegeben, wenn doppelte Schlüsselwerte in einen eindeutigen Index eingefügt werden. Es schlagen nur die Zeilen fehl, die gegen die Eindeutigkeitseinschränkung verstoßen.  
  
 OFF  
 Eine Fehlermeldung wird ausgegeben, wenn doppelte Schlüsselwerte in einen eindeutigen Index eingefügt werden. Für den gesamten INSERT-Vorgang wird ein Rollback ausgeführt.  
  
 IGNORE_DUP_KEY kann für Indizes, die für eine Sicht erstellt werden, für nicht eindeutige Indizes, XML-Indizes sowie für räumliche und gefilterte Indizes nicht auf ON festgelegt werden.  
  
 Um IGNORE_DUP_KEY anzuzeigen, verwenden Sie [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 In abwärtskompatibler Syntax ist WITH IGNORE_DUP_KEY gleichwertig mit WITH IGNORE_DUP_KEY = ON.  
  
 STATISTICS_NORECOMPUTE **=** { ON | OFF }  
 Gibt an, ob Verteilungsstatistiken neu berechnet werden. Der Standardwert ist OFF.  
  
 EIN  
 Veraltete Indexstatistiken werden nicht automatisch neu berechnet.  
  
 OFF  
 Die automatischen Updates der Statistiken sind aktiviert.  
  
 Um das automatische Aktualisieren von Statistiken wiederherzustellen, müssen Sie STATISTICS_NORECOMPUTE auf OFF festlegen oder die UPDATE STATISTICS-Anweisung ohne die NORECOMPUTE-Klausel ausführen.  
  
> [!IMPORTANT]
> Wenn Sie die automatische Neuberechnung von Verteilungsstatistiken deaktivieren, wählt der Abfrageoptimierer möglicherweise nicht die optimalen Ausführungspläne für Abfragen, an denen die Tabelle beteiligt ist.  
  
STATISTICS_INCREMENTAL = { ON | **OFF** }  

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Bei **ON** wird die Statistik pro Partition erstellt. Bei **OFF** wird die Statistikstruktur gelöscht und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] berechnet die Statistiken erneut. Der Standardwert ist **OFF**.  
  
 Wenn Statistiken pro Partition nicht unterstützt werden, wird die Option ignoriert und eine Warnung generiert. Inkrementelle Statistiken werden für folgende Statistiktypen nicht unterstützt:  
  
-   Statistiken, die mit Indizes erstellt wurden, die über keine Partitionsausrichtung mit der Basistabelle verfügen  
-   Statistiken, die für lesbare sekundäre Always On-Datenbanken erstellt wurden  
-   Statistiken, die für schreibgeschützte Datenbanken erstellt wurden  
-   Statistiken, die für gefilterte Indizes erstellt wurden  
-   Statistiken, die für Sichten erstellt wurden  
-   Statistiken, die für interne Tabellen erstellt wurden  
-   Statistiken, die mit räumlichen Indizes oder XML-Indizes erstellt wurden  
  
 ONLINE **=** { ON | **OFF** } \<as applies to rebuild_index_option>  
 Gibt an, ob die zugrunde liegenden Tabellen und zugeordneten Indizes für Abfragen und Datenänderungen während des Indexvorgangs verfügbar sind. Der Standardwert ist OFF.  
  
 Bei XML-Indizes oder räumlichen Indizes wird nur `ONLINE = OFF` unterstützt, und wenn ONLINE auf ON festgelegt wird, wird ein Fehler ausgelöst.  
  
> [!IMPORTANT]
> Onlineindexvorgänge sind nicht in jeder Edition von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verfügbar. Eine Liste der Features, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Editionen und unterstützte Features von [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]](../../sql-server/editions-and-supported-features-for-sql-server-2016.md) und [Editionen und unterstützte Features von SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
 EIN  
 Lang andauernde Sperren werden nicht für die Dauer des Indexvorgangs aufrechterhalten. Während der Hauptphase des Indexvorgangs wird nur eine beabsichtigte freigegebene Sperre für die Quelltabelle aufrechterhalten. Auf diese Weise können Abfragen oder Updates der zugrunde liegenden Tabelle und Indizes fortgesetzt werden. Zu Beginn des Vorgangs wird das Quellobjekt für sehr kurze Zeit mit einer gemeinsamen Sperre (S) belegt. Am Ende des Vorgangs wird für kurze Zeit eine S-Sperre für die Quelle eingerichtet, wenn ein nicht gruppierter Index erstellt wird, oder es wird eine Sch-M-Sperre (Schema Modification, Schemaänderung) eingerichtet, wenn ein gruppierter Index online erstellt oder gelöscht wird oder wenn ein gruppierter oder nicht gruppierter Index neu erstellt wird. ONLINE kann nicht auf ON festgelegt werden, wenn ein Index auf einer lokalen temporären Tabelle erstellt wird.  
  
 OFF  
 Die Tabellensperren werden für die Dauer des Indexvorgangs angewendet. Ein Offlineindexvorgang, durch den ein gruppierter, räumlicher oder XML-Index erstellt, neu erstellt oder gelöscht wird bzw. ein nicht gruppierter Index neu erstellt oder gelöscht wird, aktiviert eine Sch-M-Sperre für die Tabelle. Dadurch wird verhindert, dass Benutzer für die Dauer des Vorgangs auf die zugrunde liegende Tabelle zugreifen können. Ein Offlineindexvorgang, bei dem ein nicht gruppierter Index erstellt wird, aktiviert eine freigegebene Sperre (S) für die Tabelle. Dadurch werden Updates der zugrunde liegenden Tabelle verhindert. Lesevorgänge, wie SELECT-Anweisungen, sind jedoch zulässig.  
  
Weitere Informationen finden Sie unter [Ausführen von Onlineindexvorgängen](../../relational-databases/indexes/perform-index-operations-online.md) .  
  
Indizes, einschließlich Indizes globaler temporärer Tabellen, können online neu erstellt werden. Es gelten folgende Ausnahmen:
- XML-Index
- Index für eine lokale temp-Tabelle
- Eindeutiger gruppierter Ausgangsindex für eine Sicht
- Columnstore-Indizes
- Gruppierter Index, wenn die zugrunde liegende Tabelle LOB-Datentypen (**image**, **ntext**, **text**) und räumliche Datentypen enthält.
- **varchar(max)** - und **varbinary(max)** -Spalten können nicht Teil eines Index sein. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] kann ein gruppierter Index mit der Option **ONLINE** erstellt oder neu erstellt werden, wenn eine Tabelle Spalten vom Typ **varchar(max)** oder **varbinary(max)** enthält. [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] lässt die **ONLINE**-Option nicht zu, wenn die Basistabelle Spalten vom Typ **varchar(max)** oder **varbinary(max)** enthält.

Weitere Informationen finden Sie unter [Funktionsweise von Onlineindexvorgängen](../../relational-databases/indexes/how-online-index-operations-work.md).

RESUMABLE **=** { ON | **OFF**}

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   

 Gibt an, ob ein Onlineindexvorgang fortsetzbar ist.

 Der Indexvorgang ON ist fortsetzbar.

 Der Indexvorgang OFF ist nicht fortsetzbar.

MAX_DURATION **=** *time* [**MINUTES**] verwendet mit **RESUMABLE = ON** (erfordert **ONLINE = ON**).

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Gibt die Zeitspanne an (als ganzzahligen Wert in Minuten), in der ein fortsetzbarer Onlineindexvorgang ausgeführt wird, bevor er angehalten wird. 

> [!IMPORTANT]
> Ausführlichere Informationen zu Indexvorgängen, die online ausgeführt werden können, finden Sie unter [Richtlinien für Onlineindexvorgänge](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

> [!NOTE] 
> Fortsetzbare Neuerstellungen von Onlineindizes werden für Columnstore-Indizes nicht unterstützt.

ALLOW_ROW_LOCKS **=** { **ON** | OFF }  

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Gibt an, ob Zeilensperren zulässig sind. Der Standardwert ist ON.  
  
 EIN  
 Zeilensperren sind beim Zugriff auf den Index zulässig. Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] bestimmt, wann Zeilensperren verwendet werden.  
  
 OFF  
 Zeilensperren werden nicht verwendet.  
  
ALLOW_PAGE_LOCKS **=** { **ON** | OFF }  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Gibt an, ob Seitensperren zulässig sind. Der Standardwert ist ON.  
  
 EIN  
 Seitensperren sind beim Zugriff auf den Index zulässig. Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] bestimmt, wann Seitensperren verwendet werden.  
  
 OFF  
 Seitensperren werden nicht verwendet.  
  
> [!NOTE]
> Ein Index kann nicht neu organisiert werden, wenn ALLOW_PAGE_LOCKS auf OFF festgelegt ist.  

 OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | **OFF** }

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Gibt an, ob der Konflikt beim Einfügen der letzten Seite optimiert werden soll. Der Standardwert ist OFF. Weitere Informationen finden Sie im Abschnitt [Sequenzielle Schlüssel](./create-index-transact-sql.md#sequential-keys) auf der CREATE INDEX-Seite.

 MAXDOP **=** max_degree_of_parallelism  
 
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Überschreibt die Konfigurationsoption **max degree of parallelism** (Max. Grad an Parallelität) für die Dauer des Indexvorgangs. Weitere Informationen finden Sie unter [Konfigurieren der Serverkonfigurationsoption Max. Grad an Parallelität](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Sie können mit MAXDOP die Anzahl der Prozessoren begrenzen, die bei der Ausführung paralleler Pläne verwendet werden. Maximal sind 64 Prozessoren zulässig.  
  
> [!IMPORTANT]
>  Obwohl die MAXDOP-Option syntaktisch für alle XML-Indizes unterstützt wird, verwendet ALTER INDEX gegenwärtig für einen räumlichen Index oder einen primären XML-Index nur einen einzelnen Prozessor.  
  
 *max_degree_of_parallelism* kann folgende Werte haben:  
  
 1  
 Unterdrückt das Generieren paralleler Pläne.  
  
 \>1  
 Begrenzt die Höchstzahl von Prozessoren in einem parallelen Indexvorgang auf die angegebene Zahl  
  
 0 (Standard)  
 Verwendet abhängig von der aktuellen Systemarbeitsauslastung die tatsächliche Anzahl von Prozessoren oder weniger Prozessoren.  
  
 Weitere Informationen finden Sie unter [Konfigurieren von Parallelindexvorgängen](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
> Parallele Indexvorgänge sind nicht in jeder Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verfügbar. Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Editionen und unterstütze Funktionen für den SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
COMPRESSION_DELAY **=** { **0** |*Dauer [Minuten]* }  

**Anwendungsbereich:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])  
  
 DELAY gibt bei einer datenträgerbasierten Tabelle die minimale Anzahl von Minuten an, die eine Deltazeilengruppe im Zustand CLOSED in der Delta-Zeilengruppe verbringen muss, bevor [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sie in die komprimierte Zeilengruppe komprimieren kann. Da Einfügungs- und Aktualisierungszeiten in datenträgerbasierten Tabellen nicht für einzelne Zeilen nachverfolgt werden, wird die Verzögerung in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf Delta-Zeilengruppen im CLOSED-Status angewendet.  
  
 Die Standardeinstellung beträgt 0 Minuten.  
  
 Empfehlungen zur Verwendung von COMPRESSION_DELAY finden Sie unter [Erste Schritte mit Columnstore für operative Echtzeitanalyse](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
 DATA_COMPRESSION  
 Gibt die Datenkomprimierungsoption für den angegebenen Index, die Partitionsnummer oder den Bereich von Partitionen an. Die folgenden Optionen sind verfügbar:  
  
 Keine  
 Der Index oder die angegebenen Partitionen werden nicht komprimiert. Gilt nicht für columnstore-Indizes.  
  
 ROW  
 Der Index oder die angegebenen Partitionen werden mit Zeilenkomprimierung komprimiert. Gilt nicht für columnstore-Indizes.  
  
 PAGE  
 Der Index oder die angegebenen Partitionen werden mit Seitenkomprimierung komprimiert. Gilt nicht für columnstore-Indizes.  
  
 COLUMNSTORE  
   
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Gilt nur für columnstore-Indizes, einschließlich nicht gruppierter und gruppierter columnstore-Indizes. COLUMNSTORE gibt an, dass der Index oder angegebene Partitionen, die mit der COLUMNSTORE_ARCHIVE-Option komprimiert wurden, dekomprimiert werden sollen. Nachdem die Daten wiederhergestellt wurden, sind sie weiterhin mit der columnstore-Komprimierung komprimiert, die für alle columnstore-Indizes verwendet wird.  
  
 COLUMNSTORE_ARCHIVE  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Gilt nur für columnstore-Indizes, einschließlich nicht gruppierter und gruppierter columnstore-Indizes. Durch COLUMNSTORE_ARCHIVE wird die angegebene Partition weiter in eine geringere Größe komprimiert. Dies empfiehlt sich bei der Archivierung und in Situationen, in denen es auf eine geringere Speichergröße und nicht auf den zusätzlichen Zeitaufwand für das Speichern und Abrufen ankommt.  
  
 Weitere Informationen zur Datenkomprimierung finden Sie unter [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md).  
  
 ON PARTITIONS **(** { \<partition_number_expression> | \<range> } [ **,** ...n] **)**  
    
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Gibt die Partitionen an, für die die DATA_COMPRESSION-Einstellung gilt. Wenn der Index nicht partitioniert ist, erzeugt das ON PARTITIONS-Argument einen Fehler. Wenn die ON PARTITIONS-Klausel nicht angegeben wird, gilt die DATA_COMPRESSION-Option für alle Partitionen eines partitionierten Indexes.  
  
 \<partition_number_expression> kann wie folgt angegeben werden:  
  
-   Geben Sie die Nummer für eine Partition an, beispielsweise: `ON PARTITIONS (2)`.  
-   Geben Sie die Partitionsnummern für mehrere einzelne Partitionen durch Kommas getrennt an, beispielsweise: `ON PARTITIONS (1, 5)`.  
-   Geben Sie sowohl Bereiche als auch einzelne Partitionen an: `ON PARTITIONS (2, 4, 6 TO 8)`.  
  
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
  
 ONLINE **=** { ON  | **OFF** } \<as applies to single_partition_rebuild_index_option>  
 Gibt an, ob ein Index oder eine Indexpartition einer zugrunde liegenden Tabelle online oder offline neu erstellt werden kann. Wenn **REBUILD** online ausgeführt wird (**ON**), sind die Daten in dieser Tabelle für Abfragen und Datenänderungen während des Indexvorgangs verfügbar.  Der Standardwert ist **OFF**.  
  
 EIN  
 Lang andauernde Sperren werden nicht für die Dauer des Indexvorgangs aufrechterhalten. Während der Hauptphase des Indexvorgangs wird nur eine beabsichtigte freigegebene Sperre für die Quelltabelle aufrechterhalten. Es sind eine S-Sperre für die Tabelle am Anfang der Indexneuerstellung und eine Sch-M-Sperre für die Tabelle am Ende der Onlineneuerstellung des Index erforderlich. Obwohl beide Sperren kurze Metadatensperren sind, muss insbesondere die Sch-M-Sperre auf den Abschluss aller blockierenden Transaktionen warten. Während der Wartezeit sperrt die Sch-M-Sperre alle anderen Transaktionen, die an dieser Sperre warten, wenn sie auf die gleiche Tabelle zugreifen.  
  
> [!NOTE]
>  Durch Neuerstellung von Onlineindizes können die *low_priority_lock_wait*-Optionen festgelegt werden, die weiter unten in diesem Abschnitt beschrieben werden.  
  
 OFF  
 Die Tabellensperren werden für die Dauer des Indexvorgangs angewendet. Dadurch wird verhindert, dass Benutzer für die Dauer des Vorgangs auf die zugrunde liegende Tabelle zugreifen können.  
  
 WAIT_AT_LOW_PRIORITY ausschließlich kombiniert mit **ONLINE=ON**  
 
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Bei der Onlineindexneuerstellung muss auf blockierende Vorgänge für diese Tabelle gewartet werden. **WAIT_AT_LOW_PRIORITY** gibt an, dass der Onlineneuerstellungsvorgang für den Index auf Sperren mit niedriger Priorität wartet und die weitere Ausführung anderer Vorgänge ermöglicht, während der Onlineerstellungsvorgang für den Index wartet. Das Auslassen der Option **WAIT AT LOW PRIORITY** entspricht WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 Minuten, ABORT_AFTER_WAIT = NONE). Weitere Informationen finden Sie unter [WAIT_AT_LOW_PRIORITY](alter-index-transact-sql.md). 
  
 MAX_DURATION = *time* [**MINUTES**]  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Die Wartezeit (ein ganzzahliger Wert in Minuten), während der die Sperren der Onlineindexneuerstellung mit niedriger Priorität warten, wenn der DDL-Befehl ausgeführt wird. Wenn der Vorgang während des **MAX_DURATION**-Zeitraums blockiert wird, wird eine der **ABORT_AFTER_WAIT**-Aktionen ausgeführt. **MAX_DURATION** wird immer in Minuten angegeben, und das Wort **MINUTES** kann ausgelassen werden.  
 
 ABORT_AFTER_WAIT = [**NONE** | **SELF** | **BLOCKERS** } ]  
   
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Keine  
 Es wird weiterhin mit normaler (regulärer) Priorität auf die Sperre gewartet.  
  
 SELF  
 Beendet den DDL-Vorgang zur Onlineindexneuerstellung, der derzeit ausgeführt wird, ohne weitere Aktionen auszuführen.  
  
 BLOCKERS  
 Bricht alle Benutzertransaktionen ab, die den DDL-Vorgang zur Onlineindexneuerstellung blockieren, sodass der Vorgang fortgesetzt werden kann. Die **BLOCKERS**-Option erfordert für die **ALTER ANY CONNECTION**-Berechtigung eine Anmeldung.  
 
 RESUME 
 
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  

Fortsetzen eines Indexvorgangs, der manuell oder aufgrund eines Fehlers angehalten wurde.

MAX_DURATION kombiniert mit **RESUMABLE=ON**
 
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Die Zeitspanne (als ganzzahliger Wert in Minuten), die ein fortsetzbarer Onlineindexvorgang ausgeführt wird, nachdem er fortgesetzt wurde. Nach Ablauf dieser Zeitspanne wird der fortsetzbare Vorgang angehalten, falls er noch ausgeführt wird.

WAIT_AT_LOW_PRIORITY kombiniert mit **RESUMABLE=ON** und **ONLINE = ON**  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Beim Fortsetzen einer Onlineindexneuerstellung nach dem Anhalten muss auf blockierende Vorgänge für diese Tabelle gewartet werden. **WAIT_AT_LOW_PRIORITY** gibt an, dass der Onlineneuerstellungsvorgang für den Index auf Sperren mit niedriger Priorität wartet und die weitere Ausführung anderer Vorgänge ermöglicht, während der Onlineerstellungsvorgang für den Index wartet. Das Auslassen der Option **WAIT AT LOW PRIORITY** entspricht WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 Minuten, ABORT_AFTER_WAIT = NONE). Weitere Informationen finden Sie unter [WAIT_AT_LOW_PRIORITY](alter-index-transact-sql.md). 

PAUSE
 
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
Anhalten eines fortsetzbaren Onlineneuerstellungsvorgangs für einen Index.

ABORT

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   

Abbrechen eines ausgeführten oder angehaltenen Indexvorgangs, der als fortsetzbar deklariert wurde. Zum Beenden eines fortsetzbaren Indexneuerstellungsvorgangs müssen Sie einen **ABORT**-Befehl explizit ausführen. Durch das Auftreten eines Fehlers oder durch Anhalten eines fortsetzbaren Indexvorgangs wird dessen Ausführung nicht beendet. Der Vorgang befindet sich stattdessen in einem unbestimmten Pausezustand.
  
## <a name="remarks"></a>Bemerkungen  
ALTER INDEX kann nicht verwendet werden, um einen Index neu zu partitionieren oder ihn in eine andere Dateigruppe zu verschieben. Diese Anweisung kann nicht verwendet werden, um die Indexdefinition, wie z. B. das Hinzufügen oder Löschen von Spalten oder das Ändern der Spaltenreihenfolge, zu ändern. Verwenden Sie CREATE INDEX mit der DROP_EXISTING-Klausel zum Ausführen dieser Vorgänge.  
  
Wenn eine Option nicht explizit angegeben ist, wird die aktuelle Einstellung angewendet. Wenn in der REBUILD-Klausel beispielsweise keine FILLFACTOR-Einstellung angegeben ist, wird der im Systemkatalog gespeicherte Füllfaktorwert während der Neuerstellung verwendet. Verwenden Sie zum Anzeigen der aktuellen Indexoptionseinstellungen [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
Die Werte für `ONLINE`, `MAXDOP` und `SORT_IN_TEMPDB` werden nicht im Systemkatalog gespeichert. Der Standardwert der Option wird verwendet, sofern die Option nicht in der Indexanweisung angegeben ist.
  
Auf Mehrprozessorcomputern werden für `ALTER INDEX REBUILD`, wie bei anderen Abfragen auch, automatisch weitere Prozessoren verwendet, um die Scan- und Sortierungsvorgänge auszuführen, die mit einem Ändern des Index verbunden sind. Wenn Sie `ALTER INDEX REORGANIZE` mit oder ohne LOB_COMPACTION ausführen, entspricht der Wert von **Max. Grad an Parallelität** einem einzelnen Threadvorgang. Weitere Informationen finden Sie unter [Konfigurieren von Parallelindexvorgängen](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!IMPORTANT]
> Ein Index kann nicht neu organisiert oder neu erstellt werden, wenn die Dateigruppe, in der er enthalten ist, eine Offline- oder schreibgeschützte Dateigruppe ist. Wenn das Schlüsselwort ALL angegeben ist und mindestens ein Index in einer Offline- oder schreibgeschützten Dateigruppe enthalten ist, erzeugt die Anweisung einen Fehler.  
  
## <a name="rebuilding-indexes"></a><a name="rebuilding-indexes"></a> Neuerstellen von Indizes  
Beim Neuerstellen eines Indexes wird der Index gelöscht und neu erstellt. Bei diesem Vorgang wird die Fragmentierung entfernt, Speicherplatz wird freigegeben, indem die Seiten auf der Grundlage der angegebenen oder vorhandenen Füllfaktoreinstellung komprimiert werden, und die Indexzeilen werden in aufeinanderfolgenden Seiten neu geordnet. Wenn ALL angegeben ist, werden alle Indizes der Tabelle in einer einzelnen Transaktion gelöscht und neu erstellt. Fremdschlüsseleinschränkungen müssen nicht im Voraus gelöscht werden. Wenn Indizes mit mindestens 128 Blöcken neu erstellt werden, verzögert das [!INCLUDE[ssDE](../../includes/ssde-md.md)] die tatsächlichen aufgehobenen Seitenzuordnungen sowie deren zugeordnete Sperren, bis für die Transaktion ein Commit ausgeführt wird.  
 
Weitere Informationen finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
## <a name="reorganizing-indexes"></a><a name="reorganizing-indexes"></a> Neuorganisieren von Indizes
Das Neuorganisieren eines Indexes beansprucht minimale Systemressourcen. Dabei wird die Blattebene von gruppierten und nicht gruppierten Indizes in Tabellen und Sichten defragmentiert, indem die Blattebenenseiten physisch neu geordnet werden, damit sie mit der logischen Reihenfolge der Blattknoten von links nach rechts übereinstimmen. Durch das Neuorganisieren werden die Indexseiten auch komprimiert. Die Komprimierung basiert auf dem vorhandenen Füllfaktorwert. 
  
Wenn `ALL` angegeben ist, werden relationale Indizes, sowohl gruppierte als auch nicht gruppierte, und XML-Indizes der Tabelle neu organisiert. Bei Angabe von ALL gelten einige Einschränkungen, die Sie in der Definition für ALL in diesem Artikel im Abschnitt „Argumente“ finden.  
  
Weitere Informationen finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  

> [!IMPORTANT]
> Für eine [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]-Tabelle mit einem sortierten, gruppierten Columnstore-Index sortiert `ALTER INDEX REORGANIZE` die Daten nicht neu. Verwenden Sie `ALTER INDEX REBUILD` zum Neusortieren der Daten.
  
## <a name="disabling-indexes"></a><a name="disabling-indexes"></a> Deaktivieren von Indizes  
Durch das Deaktivieren eines Indexes wird der Benutzerzugriff auf den Index sowie auf die zugrunde liegenden Tabellendaten gruppierter Indizes verhindert. Die Indexdefinition bleibt im Systemkatalog erhalten. Beim Deaktivieren eines nicht gruppierten oder gruppierten Indexes in einer Sicht werden die Indexdaten physisch gelöscht. Durch das Deaktivieren eines gruppierten Indexes wird der Benutzerzugriff auf die Daten verhindert; die Daten bleiben jedoch in der B-Struktur unverwaltet, bis der Index gelöscht oder neu erstellt wird. Führen Sie eine Abfrage für die **is_disabled**-Spalte in der **sys.indexes**-Katalogsicht aus, um den Status eines aktivierten oder deaktivierten Index anzuzeigen.  
  
Befindet sich eine Tabelle in einer Transaktionsreplikationsveröffentlichung, können die Indizes, die mit Primärschlüsselspalten verknüpft sind, nicht deaktiviert werden, weil diese Indizes von der Replikation benötigt werden. Wenn Sie einen Index deaktivieren möchten, müssen Sie zuerst die Tabelle aus der Veröffentlichung löschen. Weitere Informationen finden Sie unter [Veröffentlichen von Daten und Datenbankobjekten](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
Verwenden Sie die `ALTER INDEX REBUILD`-Anweisung oder die `CREATE INDEX WITH DROP_EXISTING`-Anweisung, um den Index zu aktivieren. Das Neuerstellen eines deaktivierten gruppierten Indexes kann nicht durchgeführt werden, wenn die ONLINE-Option auf ON festgelegt ist. Weitere Informationen finden Sie unter [Deaktivieren von Indizes und Einschränkungen](../../relational-databases/indexes/disable-indexes-and-constraints.md).  
  
## <a name="setting-options"></a>Festlegen von Optionen  
Sie können die Optionen `ALLOW_ROW_LOCKS`, `ALLOW_PAGE_LOCKS`, `OPTIMIZE_FOR_SEQUENTIAL_KEY`, `IGNORE_DUP_KEY` und `STATISTICS_NORECOMPUTE` für einen angegebenen Index festlegen, ohne diesen Index neu zu erstellen oder neu zu organisieren. Die geänderten Werte werden sofort auf den Index angewendet. Verwenden Sie zum Anzeigen dieser Einstellungen **sys.indexes**. Weitere Informationen finden Sie unter [Festlegen von Indexoptionen](../../relational-databases/indexes/set-index-options.md).  
  
### <a name="row-and-page-locks-options"></a>Zeilen- und Seitensperren (Optionen)  
Wenn `ALLOW_ROW_LOCKS = ON` und `ALLOW_PAGE_LOCK = ON` angegeben sind, sind Sperren auf Zeilen-, Seiten- und Tabellenebene zulässig, wenn auf den Index zugegriffen wird. Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] wählt die geeignete Sperre aus und kann die Sperre von einer Zeilen- oder Seitensperre auf eine Tabellensperre ausweiten.  
  
Wenn `ALLOW_ROW_LOCKS = OFF` und `ALLOW_PAGE_LOCK = OFF` angegeben sind, ist nur eine Sperre auf Tabellenebene zulässig, wenn auf den Index zugegriffen wird.  
  
Wenn beim Festlegen der Optionen für Zeilen- oder Seitensperren ALL angegeben ist, werden die Einstellungen auf alle Indizes angewendet. Wenn es sich bei der zugrunde liegenden Tabelle um einen Heap handelt, werden die Einstellungen folgendermaßen angewendet:  
  
|Option|Details|
|------|-------|
|ALLOW_ROW_LOCKS = ON oder OFF|Für den Heap und alle zugeordneten nicht gruppierten Indizes.|  
|ALLOW_PAGE_LOCKS = ON|Für den Heap und alle zugeordneten nicht gruppierten Indizes.|  
|ALLOW_PAGE_LOCKS = OFF|Vollständig für die nicht gruppierten Indizes. Dies bedeutet, dass für die nicht gruppierten Indizes keine Seitensperren zulässig sind. Beim Heap sind nur gemeinsame Sperren (S, Shared), Updatesperren (U, Update) und exklusive Sperren (X, Exclusive) für die Seite unzulässig. Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] kann weiterhin eine beabsichtigte Seitensperre (IS, IU oder IX) für interne Zwecke abrufen.|  
  
## <a name="online-index-operations"></a><a name="online-index-operations"></a> Onlineindexvorgänge  
Wenn Sie einen Index neu erstellen, und die ONLINE-Option ist auf ON festgelegt, sind die zugrunde liegenden Objekte, die Tabellen und zugeordneten Indizes für Abfragen und Datenänderungen verfügbar. Sie können auch einen Teil eines Indexes online neu erstellen, der sich in einer einzelnen Partition befindet. Exklusive Tabellensperren werden während des Änderungsprozesses nur für einen kurzen Zeitraum aufrechterhalten.  
  
Das Neuorganisieren eines Indexes wird stets online durchgeführt. Bei dem Prozess werden Sperren nicht dauerhaft aufrechterhalten; daher werden Abfragen oder Updates, die ausgeführt werden, nicht blockiert.  
  
Gleichzeitige Onlineindexvorgänge können in derselben Tabelle oder Tabellenpartition nur bei den folgenden Aktionen ausgeführt werden:  
  
-   Erstellen mehrerer nicht gruppierter Indizes.  
-   Neuorganisieren unterschiedlicher Indizes in derselben Tabelle.  
-   Neuorganisieren unterschiedlicher Indizes während der Neuerstellung von nicht überlappenden Indizes derselben Tabelle.  
  
Alle anderen gleichzeitig durchgeführten Onlineindexvorgänge erzeugen einen Fehler. Sie können beispielsweise nicht zwei oder mehr Indizes zur gleichen Zeit für dieselbe Tabelle neu erstellen bzw. beim Neuerstellen eines vorhandenen Indexes keinen neuen Index für dieselbe Tabelle erstellen.  

### <a name="resumable-index-operations"></a><a name="resumable-indexes"></a>Fortsetzbare Indexvorgänge

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Die Neuerstellung von Onlineindizes wird als fortsetzbar angegeben, wenn die Option RESUMABLE = ON verwendet wird. 
-  Die RESUMABLE-Option wird in den Metadaten nicht für einen bestimmten Index beibehalten und gilt nur für die Dauer der aktuellen DDL-Anweisung. Zum Aktivieren der Fortsetzungsmöglichkeit muss die Klausel RESUMABLE = ON daher explizit angegeben werden.
-  Die MAX_DURATION-Option wird für die Option RESUMABLE = ON oder die Argumentoption **low_priority_lock_wait** unterstützt. 
   -  MAX_DURATION gibt bei der RESUMABLE-Option das Zeitintervall an, in dem ein Index neu erstellt wird. Sobald dieses Zeitintervall beendet ist, wird die Indexneuerstellung entweder angehalten oder in ihrer Ausführung beendet. Der Benutzer entscheidet, wann die Neuerstellung eines angehaltenen Index fortgesetzt werden kann. Die **Zeitspanne** für MAX_DURATION in Minuten muss größer als 0 Minuten und kleiner oder gleich einer Woche (7 \* 24 \* 60 = 10.080 Minuten) sein. Das lange Anhalten eines Indexvorgangs kann Auswirkungen auf die DML-Leistung für eine bestimmte Tabelle sowie die Datenträgerkapazität der Datenbank haben, da sowohl der ursprüngliche Index als auch der neu erstellte Index Speicherplatz benötigen und während der DML-Vorgänge aktualisiert werden müssen. Wird die MAX_DURATION-Option ausgelassen, dann wird der Indexvorgang bis zum vollständigen Abschluss oder bis ein Fehler auftritt fortgeführt. 
   -  Mit der Argumentoption \<low_priority_lock_wait> können Sie entscheiden, wie der Indexvorgang fortgesetzt werden kann, wenn er für die SCH-M-Sperre blockiert wird.
 
-  Durch das erneute Ausführen der ursprünglichen ALTER INDEX REBUILD-Anweisung mit denselben Parametern wird ein angehaltener Indexneuerstellungsvorgang fortgesetzt. Auch durch Ausführen der ALTER INDEX RESUME-Anweisung kann ein angehaltener Indexneuerstellungsvorgang fortgesetzt werden.
-  Die Option SORT_IN_TEMPDB=ON wird für den fortsetzbaren Index nicht unterstützt. 
-  Der DDL-Befehl kann mit RESUMABLE = ON nicht innerhalb einer expliziten Transaktion ausgeführt werden (kann nicht Teil des Blocks „tran ... commit  block“ sein).
-  Nur Indexvorgänge, die angehalten wurden, sind fortsetzbar.
-  Beim Fortsetzen eines Indexvorgangs, der angehalten wurde, können Sie den MAXDOP-Wert in einen neuen Wert ändern.  Wird beim Fortsetzen eines angehaltenen Indexvorgangs kein MAXDOP-Wert angegeben, wird der letzte MAXDOP-Wert übernommen. Wird überhaupt keine MAXDOP-Option für Indexneuerstellungsvorgänge angegeben, wird der Standardwert übernommen.
- Wenn Sie den Indexvorgang sofort anhalten möchten, können Sie den laufenden Befehl beenden (STRG+C) oder die Befehle ALTER INDEX PAUSE oder KILL *session_id* ausführen. Ein angehaltener Befehl kann mit der RESUME-Option fortgesetzt werden.
-  Mit dem ABORT-Befehl wird die Sitzung beendet, die die ursprüngliche Indexneuerstellung gehostet hat, und der Indexvorgang wird abgebrochen.  
-  Für die fortsetzbare Indexneuerstellung werden keine zusätzlichen Ressourcen benötigt, mit Ausnahme von:
   -    zusätzlichem Speicherplatz, damit der Index weiter erstellt wird, einschließlich der Zeit, wenn der Index angehalten wird
   -    DDL-Status zur Verhinderung von DDL-Änderungen
-  Der Cleanup inaktiver Datensätze wird ausgeführt, während der Index angehalten wird. Er wird jedoch angehalten, während der Index ausgeführt wird.   
Die folgenden Funktionen sind für Indexneuerstellungsvorgänge deaktiviert:
   -    Neuerstellen eines deaktivierten Index nicht unterstützt bei RESUMABLE=ON
   -    ALTER INDEX REBUILD ALL-Befehl
   -    ALTER TABLE-Befehl mithilfe von Indexneuerstellung  
   -    Der DDL-Befehl kann mit „RESUMEABLE = ON“ nicht innerhalb einer expliziten Transaktion ausgeführt werden (kann nicht Teil des Blocks „tran ... commit  block“ sein).
   -    Erstellen eines Index, der mindestens eine berechnete Spalte oder TIMESTAMP-Spalte als Schlüsselspalte besitzt
-   Sollte die Basistabelle mindestens eine LOB-Spalte besitzen, dann ist für die Indexneuerstellung eines fortsetzbaren gruppierten Index eine Sch-M-Sperre zu Beginn dieses Vorgangs erforderlich. 

> [!NOTE]
> Der DDL-Befehl wird so lange ausgeführt, bis er entweder abgeschlossen ist, angehalten wird oder ein Fehler auftritt. Wenn der Befehl angehalten wird, wird ein Fehler ausgelöst, der meldet, dass der Vorgang angehalten wurde und dass die Indexerstellung nicht abgeschlossen wurde. Weitere Informationen zum aktuellen Indexstatus finden Sie unter [sys.index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md). Tritt ein Fehler auf, wird auch hier eine Fehlermeldung ausgegeben. 

 Weitere Informationen finden Sie unter [Ausführen von Onlineindexvorgängen](../../relational-databases/indexes/perform-index-operations-online.md) .  
  
 ### <a name="wait_at_low_priority-with-online-index-operations"></a>WAIT_AT_LOW_PRIORITY bei Onlineindexvorgängen  
  
 Um die DDL-Anweisung für eine Onlineindexneuerstellung auszuführen, müssen alle aktiven blockierenden Transaktionen, die für eine bestimmte Tabelle ausgeführt werden, abgeschlossen sein. Wenn die Onlineindexneuerstellung ausgeführt wird, werden alle neuen Transaktionen, die zur Ausführung in dieser Tabelle bereit sind, blockiert. Obwohl die Sperre für die Onlineindexneuerstellung nur kurz dauert, kann das Warten auf den Abschluss aller noch offenen Transaktionen und das Blockieren aller neuen, zu startenden Transaktionen für eine bestimmte Tabelle den Durchsatz beeinträchtigen, eine Verlangsamung oder einen Ausfall der Arbeitsauslastung verursachen und den Zugriff auf die zugrunde liegende Tabelle deutlich einschränken. Mit der **WAIT_AT_LOW_PRIORITY**-Option können Datenbankadministratoren die S-Sperre sowie Sch-M-Sperren, die für die Onlineneuerstellung von Indizes erforderlich sind, verwalten und eine von drei Optionen auswählen. In allen drei Fällen gilt: Sind während der Wartezeit (MAX_DURATION = n [Minuten]) keine blockierenden Aktivitäten vorhanden, wird die Onlineindexneuerstellung ohne Wartezeit sofort ausgeführt, und die DDL-Anweisung wird abgeschlossen.  
  
## <a name="spatial-index-restrictions"></a>Einschränkungen für räumliche Indizes  
 Wenn Sie einen räumlichen Index neu erstellen, ist die zugrunde liegende Benutzertabelle während des Indexvorgangs nicht verfügbar, weil der räumliche Index eine Schemasperre eingerichtet hat.  
  
 Die PRIMARY KEY-Einschränkung der Benutzertabelle kann nicht geändert werden, solange ein räumlicher Index für eine Spalte der betreffenden Tabelle definiert ist. Die PRIMARY KEY-Einschränkung kann erst geändert werden, nachdem alle räumlichen Indizes aus der Tabelle gelöscht wurden. Nach der Änderung der PRIMARY Key-Einschränkung können die einzelnen räumlichen Indizes neu erstellt werden.  
  
 Räumliche Indizes können in einem Neuerstellungsvorgang einer einzelnen Partition nicht angegeben werden. Sie können räumliche Indizes jedoch bei einer vollständigen Neuerstellung der Partition angeben.  
  
 Zum Ändern von Optionen, die einem bestimmten räumlichen Index eigen sind, beispielsweise BOUNDING_BOX oder GRID, können Sie entweder eine CREATE SPATIAL INDEX-Anweisung mit der Angabe DROP_EXISTING = ON verwenden, oder Sie löschen den räumlichen Index und erstellen einen neuen räumlichen Index. Für ein Beispiel gehen Sie unter [CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md).  
  
## <a name="data-compression"></a>Datenkomprimierung  
 Weitere Informationen zur Datenkomprimierung finden Sie unter [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md).  
  
 Mithilfe der gespeicherten Prozedur [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) können Sie einschätzen, wie sich eine Änderung der PAGE- und ROW-Komprimierung auf eine Tabelle, einen Index oder eine Partition auswirkt.  
  
Für partitionierte Indizes gelten die folgenden Einschränkungen:  
  
-   Bei Verwendung von ALTER INDEX ALL ... können Sie die Komprimierungseinstellung einer einzelnen Partition nicht ändern, wenn die Tabelle nicht ausgerichtete Indizes aufweist.  
-   Mit der ALTER INDEX \<index> … REBUILD PARTITION ...-Syntax wird die angegebene Partition des Indexes neu erstellt.  
-   Mit der ALTER INDEX \<index> … REBUILD WITH ...-Syntax werden alle Partitionen des Indexes neu erstellt.  
  
## <a name="statistics"></a>Statistik  
 Wenn Sie **ALTER INDEX ALL ...** in einer Tabelle ausführen, werden nur die Statistiken mit zugeordneten Indizes aktualisiert. Automatische oder manuelle Statistiken, die statt eines Indexes in der Tabelle erstellt wurden, werden nicht aktualisiert.  
  
## <a name="permissions"></a>Berechtigungen  
 Zum Ausführen von ALTER INDEX benötigen Sie mindestens die ALTER-Berechtigung auf der Tabelle bzw. Sicht.  
  
## <a name="version-notes"></a>Versionshinweise  
  
-  [!INCLUDE[ssSDS](../../includes/sssds-md.md)] verwendet die Optionen „Dateigruppe“ und „Filestream“ nicht.  
-  Columnstore-Indizes sind erst ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] verfügbar. 
-  Fortsetzbare Indexvorgänge sind ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] verfügbar.   
  
## <a name="basic-syntax-example"></a>Einfaches Syntaxbeispiel:   
  
```sql 
ALTER INDEX index1 ON table1 REBUILD;  
  
ALTER INDEX ALL ON table1 REBUILD;  
  
ALTER INDEX ALL ON dbo.table1 REBUILD;  
```

## <a name="examples-columnstore-indexes"></a>Beispiele: Columnstore-Indizes  
 Diese Beispiele gelten für Columnstore-Indizes.  
  
### <a name="a-reorganize-demo"></a>A. REORGANIZE-Demo  
 In diesem Beispiel wird die Funktionsweise des ALTER INDEX REORGANIZE-Befehls veranschaulicht.  Es wird eine Tabelle mit mehreren Zeilengruppen erstellt, die anschließend mithilfe von REORGANIZE zusammengeführt werden.  
  
```sql  
-- Create a database   
CREATE DATABASE [ columnstore ];  
GO  
  
-- Create a rowstore staging table  
CREATE TABLE [ staging ] (  
     AccountKey              int NOT NULL,  
     AccountDescription      nvarchar (50),  
     AccountType             nvarchar(50),  
     AccountCodeAlternateKey     int  
     )  
  
-- Insert 10 million rows into the staging table.   
DECLARE @loop int  
DECLARE @AccountDescription varchar(50)  
DECLARE @AccountKey int  
DECLARE @AccountType varchar(50)  
DECLARE @AccountCode int  
  
SELECT @loop = 0  
BEGIN TRAN  
    WHILE (@loop < 300000)   
      BEGIN  
        SELECT @AccountKey = CAST (RAND()*10000000 as int);  
        SELECT @AccountDescription = 'accountdesc ' + CONVERT(varchar(20), @AccountKey);  
        SELECT @AccountType = 'AccountType ' + CONVERT(varchar(20), @AccountKey);  
        SELECT @AccountCode =  CAST (RAND()*10000000 as int);  
  
        INSERT INTO  staging VALUES (@AccountKey, @AccountDescription, @AccountType, @AccountCode);  
  
        SELECT @loop = @loop + 1;  
    END  
COMMIT  
  
-- Create a table for the clustered columnstore index  
  
CREATE TABLE cci_target (  
     AccountKey              int NOT NULL,  
     AccountDescription      nvarchar (50),  
     AccountType             nvarchar(50),  
     AccountCodeAlternateKey int  
     )  
  
-- Convert the table to a clustered columnstore index named inxcci_cci_target;  
CREATE CLUSTERED COLUMNSTORE INDEX idxcci_cci_target ON cci_target;  
```  
  
 Mit der TABLOCK-Option können Sie Zeilen parallel einfügen. Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] können der INSERT INTO-Vorgang und die TABLOCK-Option parallel ausgeführt werden.  
  
```sql  
INSERT INTO cci_target WITH (TABLOCK) 
SELECT TOP 300000 * FROM staging;  
```  
  
 Führen Sie diesen Befehl aus, um die OPEN-Delta-Zeilengruppen anzuzeigen. Die Anzahl der Zeilengruppen hängt vom Grad der Parallelität ab.  
  
```sql  
SELECT *   
FROM sys.dm_db_column_store_row_group_physical_stats   
WHERE object_id  = object_id('cci_target');  
```  
  
 Führen Sie diesen Befehl aus, um die Übernahme aller CLOSED- und OPEN-Zeilengruppen in den Columnstore zu erzwingen.  
  
```sql  
ALTER INDEX idxcci_cci_target ON cci_target REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
 Führen Sie diesen Befehl erneut aus, um kleinere Zeilengruppen in eine komprimierte Zeilengruppe zusammenzuführen.  
  
```sql  
ALTER INDEX idxcci_cci_target ON cci_target REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
### <a name="b-compress-closed-delta-rowgroups-into-the-columnstore"></a>B. Komprimieren von CLOSED-Delta-Zeilengruppen im Columnstore  
 In diesem Beispiel wird durch die REORGANIZE-Option jede CLOSED-Delta-Zeilengruppe im Columnstore als eine komprimierte Zeilengruppe komprimiert.   Dies ist zwar nicht erforderlich, kann aber nützlich sein, wenn CLOSED-Zeilengruppen im TM-Vorgang (Tuple Mover) nicht schnell genug komprimiert werden.  
  
```sql  
-- Uses AdventureWorksDW  
-- REORGANIZE all partitions  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE;  
  
-- REORGANIZE a specific partition  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE PARTITION = 0;  
```  
  
### <a name="c-compress-all-open-and-closed-delta-rowgroups-into-the-columnstore"></a>C. Komprimieren aller OPEN- und CLOSED-Delta-Zeilengruppen im Columnstore  
 **Anwendungsbereich:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Mit dem Befehl REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON) werden alle OPEN- und CLOSED-Delta-Zeilengruppen als eine komprimierte Zeilengruppe im Columnstore komprimiert. Dadurch wird der Deltastore geleert, und es werden alle Zeilen im Columnstore komprimiert. Dies ist insbesondere nach dem Ausführen einer Vielzahl von Einfügevorgängen nützlich, da die Zeilen bei diesen Vorgängen in einer oder mehreren Delta-Zeilengruppen gespeichert werden.  
  
 Mit REORGANIZE werden Zeilengruppen bis zur einer maximalen Anzahl von \<= 1.024.576 Zeilen kombiniert. Nach dem Komprimieren aller OPEN- und CLOSED-Zeilengruppen werden nicht mehr viele komprimierte Zeilengruppen übrig bleiben, die nur wenige Zeilen enthalten. Bestmöglich gefüllte Zeilengruppen verringern die komprimierte Größe und verbessern die Abfrageleistung.  
  
```sql  
-- Uses AdventureWorksDW2016  
-- Move all OPEN and CLOSED delta rowgroups into the columnstore.  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
  
-- For a specific partition, move all OPEN AND CLOSED delta rowgroups into the columnstore  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE PARTITION = 0 WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
### <a name="d-defragment-a-columnstore-index-online"></a>D: Online-Defragmentieren eines Columnstore-Index  
 Gilt nicht für: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
 Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] können Sie mit REORGANIZE mehr tun, als Delta-Zeilengruppen im Columnstore zu komprimieren. Sie können auch eine Onlinedefragmentierung durchführen. Zunächst wird die Größe des Columnstores verringert, indem gelöschte Zeilen physisch entfernt werden, wenn mindestens 10 % der Zeilen in einer Zeilengruppe gelöscht wurden.  Anschließend werden Zeilengruppen zu größeren Zeilengruppen bis maximal 1.024.576 Zeilen pro Zeilengruppe zusammengeführt.  Alle geänderten Zeilengruppen werden erneut komprimiert.  
  
> [!NOTE]
> Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] ist das Neuerstellen eines Columnstore-Index in den meisten Fällen nicht mehr erforderlich, da mit REORGANIZE gelöschte Zeilen physisch entfernt und Zeilengruppen zusammengeführt werden. Mit der Option COMPRESS_ALL_ROW_GROUPS wird die Übernahme aller OPEN- oder CLOSED-Delta-Zeilengruppen in den Columnstore erzwungen. Dies konnte zuvor nur mit einer Neuerstellung ausgeführt werden. REORGANIZE wird online im Hintergrund ausgeführt, wodurch Abfragen parallel dazu erfolgen können.  
  
```sql  
-- Uses AdventureWorks  
-- Defragment by physically removing rows that have been logically deleted from the table, and merging rowgroups.  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE;  
```  
  
### <a name="e-rebuild-a-clustered-columnstore-index-offline"></a>E. Neuerstellen eines gruppierten Columnstore-Index im Offline-Modus  
Gilt für: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])   
  
> [!TIP]
> Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] wird empfohlen, anstelle von ALTER INDEX REBUILD die Option ALTER INDEX REORGANIZE zu verwenden.  
  
> [!NOTE]
> In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] werden mit REORGANIZE nur CLOSED-Zeilengruppen im Columnstore komprimiert. Die einzige Möglichkeit, Defragmentierungsvorgänge auszuführen und die Übernahme aller Delta-Zeilengruppen in den Columnstore zu erzwingen, ist das Neuerstellen des Index.  
  
 In diesem Beispiel wird veranschaulicht, wie Sie einen gruppierten Columnstore-Index neu erstellen und die Übernahme aller Delta-Zeilengruppen in den Columnstore erzwingen. Im ersten Schritt wird die FactInternetSales2-Tabelle mit einem gruppierten columnstore-Index vorbereitet, und es werden Daten aus den ersten vier Spalten eingefügt.  
  
```sql  
-- Uses AdventureWorksDW  
  
CREATE TABLE dbo.FactInternetSales2 (  
    ProductKey [int] NOT NULL,   
    OrderDateKey [int] NOT NULL,   
    DueDateKey [int] NOT NULL,   
    ShipDateKey [int] NOT NULL);  
  
CREATE CLUSTERED COLUMNSTORE INDEX cci_FactInternetSales2  
ON dbo.FactInternetSales2;  
  
INSERT INTO dbo.FactInternetSales2  
SELECT ProductKey, OrderDateKey, DueDateKey, ShipDateKey  
FROM dbo.FactInternetSales;  
  
SELECT * FROM sys.column_store_row_groups;  
```  
  
 Aus den Ergebnissen ist ersichtlich, dass eine OPEN-Zeilengruppe vorhanden ist. Dies bedeutet, dass in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gewartet wird, bis weitere Zeilen hinzugefügt werden, bevor die Zeilengruppe geschlossen wird und die Daten in den Columnstore verschoben werden. Mit der nächsten Anweisung wird der gruppierte Columnstore-Index neu erstellt und die Übernahme aller Zeilen in den Columnstore erzwungen.  
  
```sql  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REBUILD;  
SELECT * FROM sys.column_store_row_groups;  
```  
  
 Die Ergebnisse der SELECT-Anweisung zeigen, dass die Zeilengruppe als COMPRESSED gekennzeichnet ist. Dies bedeutet, dass die Spaltensegmente der Zeilengruppe jetzt komprimiert und im Columnstore gespeichert sind.  
  
### <a name="f-rebuild-a-partition-of-a-clustered-columnstore-index-offline"></a>F. Neuerstellen einer Partition eines gruppierten Columnstore-Index im Offline-Modus  
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])  
 
 Verwenden Sie ALTER INDEX REBUILD mit der Partitionsoption, um eine Partition eines großen gruppierten Columnstore-Index neu zu erstellen. In diesem Beispiel wird die Partition 12 neu erstellt. Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] wird empfohlen, REBUILD durch REORGANIZE zu ersetzen.  
  
```sql  
ALTER INDEX cci_fact3   
ON fact3  
REBUILD PARTITION = 12;  
```  
  
### <a name="g-change-a-clustered-columstore-index-to-use-archival-compression"></a>G. Ändern eines gruppierten Columnstore-Index zur Verwendung der Archivierungskomprimierung  
 Gilt nicht für: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
 Mit der Datenkomprimierungsoption COLUMNSTORE_ARCHIVE können Sie die Größe eines gruppierten Columnstore-Index noch weiter verringern. Dies kann bei älteren Daten nützlich sein, die Sie kostengünstiger speichern möchten. Es wird empfohlen, diese Option nur bei selten verwendeten Daten anzuwenden, da der Dekomprimierungsvorgang länger dauert als bei der üblichen COLUMNSTORE-Komprimierung.  
  
 Im folgenden Beispiel wird ein gruppierter columnstore-Index für die Verwendung der Archivierungskomprimierung neu erstellt. Anschließend wird gezeigt, wie die Archivierungskomprimierung entfernt wird. Letztendlich wird nur die columnstore-Komprimierung verwendet.  
  
```sql  
--Prepare the example by creating a table with a clustered columnstore index.  
CREATE TABLE SimpleTable (  
    ProductKey [int] NOT NULL,   
    OrderDateKey [int] NOT NULL,   
    DueDateKey [int] NOT NULL,   
    ShipDateKey [int] NOT NULL  
);  
  
CREATE CLUSTERED INDEX cci_SimpleTable ON SimpleTable (ProductKey);  
  
CREATE CLUSTERED COLUMNSTORE INDEX cci_SimpleTable  
ON SimpleTable  
WITH (DROP_EXISTING = ON);  
  
--Compress the table further by using archival compression.  
ALTER INDEX cci_SimpleTable ON SimpleTable  
REBUILD  
WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE);  
  
--Remove the archive compression and only use columnstore compression.  
ALTER INDEX cci_SimpleTable ON SimpleTable  
REBUILD  
WITH (DATA_COMPRESSION = COLUMNSTORE);  
GO  
```  
  
## <a name="examples-rowstore-indexes"></a>Beispiele: Rowstore-Indizes  
  
### <a name="a-rebuilding-an-index"></a>A. Neuerstellen eines Index  
 Im folgenden Beispiel wird ein einzelner Index für die `Employee`-Tabelle der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank neu erstellt.  
  
```sql  
ALTER INDEX PK_Employee_EmployeeID ON HumanResources.Employee REBUILD;  
```  
  
### <a name="b-rebuilding-all-indexes-on-a-table-and-specifying-options"></a>B. Neuerstellen aller Indizes einer Tabelle und Angeben von Optionen  
 Im folgenden Beispiel wird das Schlüsselwort ALL angegeben. Dadurch werden alle Indizes neu erstellt, die der Production.Product-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank zugeordnet sind. Es werden drei Optionen angegeben.  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
ALTER INDEX ALL ON Production.Product  
REBUILD WITH (FILLFACTOR = 80, SORT_IN_TEMPDB = ON, STATISTICS_NORECOMPUTE = ON);  
```  
  
Im folgenden Beispiel werden die ONLINE-Option einschließlich der Option für Sperren mit niedriger Priorität sowie die Zeilenkomprimierungsoption hinzugefügt.  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
ALTER INDEX ALL ON Production.Product  
REBUILD WITH   
(  
    FILLFACTOR = 80,   
    SORT_IN_TEMPDB = ON,  
    STATISTICS_NORECOMPUTE = ON,  
    ONLINE = ON ( WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 4 MINUTES, ABORT_AFTER_WAIT = BLOCKERS ) ),   
    DATA_COMPRESSION = ROW  
);  
```  
  
### <a name="c-reorganizing-an-index-with-lob-compaction"></a>C. Neuorganisieren eines Indexes mit LOB-Komprimierung  
 Im folgenden Beispiel wird ein einzelner gruppierter Index in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank neu organisiert. Da der Index einen LOB-Datentyp in der Blattebene enthält, komprimiert die Anweisung auch alle Seiten, die die LOB-Daten enthalten. Beachten Sie, dass die Angabe der Option WITH (LOB_COMPACTION) nicht erforderlich ist, da der Standardwert auf ON festgelegt ist.  
  
```sql  
ALTER INDEX PK_ProductPhoto_ProductPhotoID ON Production.ProductPhoto REORGANIZE WITH (LOB_COMPACTION = ON);  
```  
  
### <a name="d-setting-options-on-an-index"></a>D: Festlegen von Optionen für einen Index  
 Im folgenden Beispiel werden mehrere Optionen für den `AK_SalesOrderHeader_SalesOrderNumber`-Index in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank festgelegt.  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
ALTER INDEX AK_SalesOrderHeader_SalesOrderNumber ON  
    Sales.SalesOrderHeader  
SET (  
    STATISTICS_NORECOMPUTE = ON,  
    IGNORE_DUP_KEY = ON,  
    ALLOW_PAGE_LOCKS = ON  
    ) ;  
GO
```  
  
### <a name="e-disabling-an-index"></a>E. Deaktivieren eines Indexes  
 Im folgenden Beispiel wird ein nicht gruppierter Index für die `Employee`-Tabelle der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank deaktiviert.  
  
```sql  
ALTER INDEX IX_Employee_ManagerID ON HumanResources.Employee DISABLE;
```  
  
### <a name="f-disabling-constraints"></a>F. Deaktivieren von Einschränkungen  
 Im folgenden Beispiel wird eine PRIMARY KEY-Einschränkung deaktiviert, indem der PRIMARY KEY-Index in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank deaktiviert wird. Die FOREIGN KEY-Einschränkung für die zugrunde liegende Tabelle wird automatisch deaktiviert, und eine Warnmeldung wird angezeigt.  
  
```sql  
ALTER INDEX PK_Department_DepartmentID ON HumanResources.Department DISABLE;  
```  
  
Das Resultset gibt diese Warnmeldung zurück.  
  
```  
Warning: Foreign key 'FK_EmployeeDepartmentHistory_Department_DepartmentID'  
on table 'EmployeeDepartmentHistory' referencing table 'Department'  
was disabled as a result of disabling the index 'PK_Department_DepartmentID'.
```  
  
### <a name="g-enabling-constraints"></a>G. Aktivieren von Einschränkungen  
 Im folgenden Beispiel werden die in Beispiel F deaktivierten PRIMARY KEY- und FOREIGN KEY-Einschränkungen aktiviert.  
  
Die PRIMARY KEY-Einschränkung wird aktiviert, indem der PRIMARY KEY-Index neu erstellt wird.  
  
```sql  
ALTER INDEX PK_Department_DepartmentID ON HumanResources.Department REBUILD;  
```  
  
Anschließend wird die FOREIGN KEY-Einschränkung aktiviert.  
  
```sql  
ALTER TABLE HumanResources.EmployeeDepartmentHistory  
CHECK CONSTRAINT FK_EmployeeDepartmentHistory_Department_DepartmentID;  
GO  
```  
  
### <a name="h-rebuilding-a-partitioned-index"></a>H. Neuerstellen eines partitionierten Indexes  
 Im folgenden Beispiel wird eine einzelne Partition mit der Partitionsnummer `5` des partitionierten `IX_TransactionHistory_TransactionDate`-Indexes in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank neu erstellt. Partition 5 wird online neu erstellt, und die zehnminütige Wartezeit für die Sperre mit niedriger Priorität gilt für jede einzelne Sperre, die durch die Indexneuerstellung abgerufen wird. Wenn während dieser Zeit die Sperre nicht für die komplette Neuerstellung des Indexes reicht, wird die Anweisung für die Neuerstellung abgebrochen.  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
-- Verify the partitioned indexes.  
SELECT *  
FROM sys.dm_db_index_physical_stats (DB_ID(),OBJECT_ID(N'Production.TransactionHistory'), NULL , NULL, NULL);  
GO  
--Rebuild only partition 5.  
ALTER INDEX IX_TransactionHistory_TransactionDate  
ON Production.TransactionHistory  
REBUILD Partition = 5   
   WITH (ONLINE = ON (WAIT_AT_LOW_PRIORITY (MAX_DURATION = 10 minutes, ABORT_AFTER_WAIT = SELF)));  
GO  
```  
  
### <a name="i-changing-the-compression-setting-of-an-index"></a>I. IÄndern der Komprimierungseinstellung eines Index  
 Im folgenden Beispiel wird ein Index für eine nicht partitionierte rowstore-Tabelle neu erstellt.  
  
```sql
ALTER INDEX IX_INDEX1   
ON T1  
REBUILD   
WITH (DATA_COMPRESSION = PAGE);  
GO  
```  
  
Weitere Beispiele für die Datenkomprimierung finden Sie unter [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md).  
 
### <a name="j-online-resumable-index-rebuild"></a>J. Onlineneuerstellung von fortsetzbaren Indizes

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   

 In den folgenden Beispielen wird veranschaulicht, wie fortsetzbare Onlineindizes neu erstellt werden. 

1. Führen Sie eine Onlineindexneuerstellung als fortsetzbaren Vorgang mit MAXDOP=1 aus.

   ```sql
   ALTER INDEX test_idx on test_table REBUILD WITH (ONLINE=ON, MAXDOP=1, RESUMABLE=ON) ;
   ```

2. Wenn Sie denselben Befehl ein zweites Mal ausführen (siehe oben), nachdem ein Indexvorgang angehalten wurde, wird der Indexneuerstellungsvorgang automatisch fortgesetzt.

3. Führen Sie eine Onlineindexneuerstellung als fortsetzbaren Vorgang aus, und legen Sie MAX_DURATION auf 240 Minuten fest.

   ```sql
   ALTER INDEX test_idx on test_table REBUILD WITH (ONLINE=ON, RESUMABLE=ON, MAX_DURATION=240) ; 
   ```
4. Halten Sie einen fortsetzbaren Onlineneuerstellungsvorgangs für einen Index an.

   ```sql
   ALTER INDEX test_idx on test_table PAUSE ;
   ```   
5. Setzen Sie eine Onlineindexneuerstellung für eine Indexneuerstellung fort, die als fortsetzbarer Vorgang ausgeführt wurde, und geben Sie für MAXDOP den Wert 4 an.

   ```sql
   ALTER INDEX test_idx on test_table RESUME WITH (MAXDOP=4) ;
   ```
6. Setzen Sie einen Vorgang zur Onlineindexneuerstellung für eine fortsetzbar ausgeführte Onlineindexneuerstellung fort. Legen Sie MAXDOP auf den Wert 2 fest, und legen Sie die Ausführungszeit für den fortsetzbar ausgeführten Index auf 240 Minuten fest. Sollte ein Index in der Sperre blockiert werden, warten Sie 10 Minuten, bevor Sie alle Blocker entfernen. 

   ```sql
      ALTER INDEX test_idx on test_table  
         RESUME WITH (MAXDOP=2, MAX_DURATION= 240 MINUTES, 
         WAIT_AT_LOW_PRIORITY (MAX_DURATION=10, ABORT_AFTER_WAIT=BLOCKERS)) ;
   ```      
7. Brechen Sie einen fortsetzbaren Indexneuerstellungsvorgang ab, der ausgeführt wird oder angehalten wurde.

   ```sql
   ALTER INDEX test_idx on test_table ABORT ;
   ``` 
  
## <a name="see-also"></a>Weitere Informationen  
[Leitfaden zur Architektur und zum Design von SQL Server-Indizes](../../relational-databases/sql-server-index-design-guide.md)     
[Ausführen von Onlineindexvorgängen](../../relational-databases/indexes/perform-index-operations-online.md)    
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)     
[CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md)   
[CREATE XML INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-index-transact-sql.md)   
[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)   
[Deaktivieren von Indizes und Einschränkungen](../../relational-databases/indexes/disable-indexes-and-constraints.md)   
[XML-Indizes &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)   
[Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)   
[sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
[EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)    
