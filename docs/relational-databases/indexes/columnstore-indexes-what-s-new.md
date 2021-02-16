---
description: Columnstore-Indizes - Neuigkeiten
title: Columnstore-Indizes - Neuigkeiten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/11/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a82a85c87fd1b5c9b2625ed0d8a7c51bee11e21
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351234"
---
# <a name="columnstore-indexes---what39s-new"></a>Columnstore-Indizes - Neuigkeiten
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Zusammenfassung der Columnstore-Funktionen, die für die einzelnen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und die aktuellen Releases von [!INCLUDE[ssSDS](../../includes/sssds-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] verfügbar sind.  

 > [!NOTE]
 > Für [!INCLUDE[ssSDS](../../includes/sssds-md.md)] sind Columnstore-Indizes in den [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]-Tarifen Premium und Standard (S3 und höher) sowie in allen V-Kern-Tarifen verfügbar. Für [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 und höher sind Columnstore-Indizes in allen Editionen verfügbar. Für [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] (vor SP1) und frühere Versionen sind Columnstore-Indizes nur in der Enterprise Edition verfügbar.
 
## <a name="feature-summary-for-product-releases"></a>Zusammenfassung der Funktionen für Produktversionen  
 In dieser Tabelle sind die wichtigsten Funktionen für Columnstore-Indizes sowie die Produkte zusammengefasst, in denen sie verfügbar sind.  

|Columnstore-Index-Funktion|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]|[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]|[!INCLUDE[ssSDW](../../includes/sssdw-md.md)]|  
|-------------------------------|---------------------------|---------------------------|---------------------------|---------------------------|--------------------------------------------|-------------------------|---|  
|Batchmodusausführung für Multithread-Abfragen|ja|ja|ja|ja|ja|ja|ja| 
|Batchmodusausführung für Singlethread-Abfragen|||ja|ja|ja|ja|ja|  
|Option für die Archivierungskomprimierung||ja|ja|ja|ja|ja|ja|  
|Momentaufnahmeisolation und Read Commited-Momentaufnahmeisolation|||ja|ja|ja|ja|ja| 
|Geben Sie den Columnstore-Index bei der Erstellung einer Tabelle an.|||ja|ja|ja|ja|ja|  
|Always On unterstützt Columnstore-Indizes.|ja|ja|ja|ja|ja|ja|ja| 
|Lesbare sekundäre Always On-Datenbanken unterstützen schreibgeschützte nicht gruppierte Columnstore-Indizes.|ja|ja|ja|ja|ja|ja|ja|  
|Lesbare sekundäre Always On-Datenbanken unterstützen aktualisierbare Columnstore-Indizes.|||ja||ja|||  
|Schreibgeschützter nicht gruppierter Columnstore-Index auf Heap- oder B-Struktur|ja|ja|ja<sup>1</sup>|ja<sup>1</sup>|ja<sup>1</sup>|ja<sup>1</sup>|ja<sup>1</sup>|  
|Aktualisierbarer nicht gruppierter Columnstore-Index auf Heap- oder B-Struktur|||ja|ja|ja|ja|ja|  
|Zusätzliche B-Strukturindizes sind für eine Heap- oder B-Struktur mit einem nicht gruppierten Columnstore-Index zulässig|ja|ja|ja|ja|ja|ja|ja|  
|Aktualisierbarer gruppierter Columnstore-Index||ja|ja|ja||ja|ja|  
|B-Strukturindex in einem gruppierten Columnstore-Index|||ja|ja||ja|ja|  
|Columnstore-Index für eine speicheroptimierte Tabelle|||ja|ja||ja|ja|  
|Die Definition von nicht gruppierten Columnstore-Indizes unterstützt gefilterte Bedingungen.|||ja|ja|ja|ja|ja|  
|Option für die Komprimierungsverzögerung von Columnstore-Indizes in `CREATE TABLE` und `ALTER TABLE`|||ja|ja|ja|ja|ja|
|Der Columnstore-Index kann über eine nicht permanent berechnete Spalte verfügen.||||ja|ja|||   
|Unterstützung des Tupelverschiebungsvorgangs durch Zusammenführungstask im Hintergrund||||||ja|ja|ja|
  
 <sup>1</sup> Speichern Sie den Index in einer schreibgeschützten Dateigruppe, um einen schreibgeschützten nicht gruppierten Columnstore-Index zu erstellen.  
 
> [!NOTE]
> Der Grad an Parallelität (DOP) für Vorgänge im [Batchmodus](../../relational-databases/query-processing-architecture-guide.md#batch-mode-execution) sind für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard Edition auf „2“ und für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Web und Express Edition auf „1“ beschränkt. Dies gilt für Columnstore-Indizes, die über datenträgerbasierte Tabellen und speicheroptimierte Tabellen erstellt wurden.

## [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 
 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] fügt diese neuen Funktionen hinzu.

### <a name="functional"></a>Funktionen
- Seit [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] wird der Tupelverschiebungsvorgang von einem Zusammenführungstask im Hintergrund unterstützt. Dieser komprimiert automatisch kleinere OPEN-Deltazeilengruppen, die für einen bestimmten durch einen internen Schwellenwert definierten Zeitraum vorhanden waren, oder führt COMPRESSED-Zeilengruppen zusammen, aus denen eine große Anzahl von Zeilen gelöscht wurde. Zuvor war ein Vorgang zum Neuorganisieren eines Index erforderlich, um Zeilengruppen mit teilweise gelöschten Daten zusammenzuführen. Dies verbessert die Qualität des Columnstore-Index im Lauf der Zeit. 

## [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 
 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] fügt diese neuen Funktionen hinzu.

### <a name="functional"></a>Funktionen
- [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] unterstützt nichtpersistierte berechnete Spalten in gruppierten Columnstore-Indexen. Persistierte berechnete Spalten werden in gruppierten Columnstore-Indizes nicht unterstützt. Sie können keinen nicht gruppierten Index für einen Columnstore-Index erstellen, der berechnete Spalten umfasst. 

## [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]  
 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] fügt wichtige Verbesserungen hinzu, um die Leistung und Flexibilität von Columnstore-Indizes zu erhöhen. Dies verbessert die Data Warehouse-Szenarios und ermöglicht operative Echtzeitanalysen.  
  
### <a name="functional"></a>Funktionen  
  
-   Eine Rowstore-Tabelle kann über einen aktualisierbaren nicht gruppierten Columnstore-Index verfügen. Bisher war der nicht gruppierte Columnstore-Index schreibgeschützt.  
  
-   Die Definition des nicht gruppierten Columnstore-Index unterstützt gefilterte Bedingungen. Um die Auswirkung auf die Leistung beim Hinzufügen eines Columnstore-Indexes in eine OLTP-Tabelle zu verringern, verwenden Sie eine gefilterte Bedingung, um einen nicht gruppierten Columnstore-Index anhand der kalten Daten Ihrer Betriebsworkload zu erstellen. 
  
-   Eine In-Memory-Tabelle kann nur über einen Columnstore-Index verfügen. Sie können ihn bei Erstellung der Tabelle generieren oder später mit [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) hinzufügen. Früher konnte nur eine datenträgerbasierte Tabelle über einen Columnstore-Index verfügen.  
  
-   Ein gruppierter Columnstore-Index kann über eine oder mehrere nicht gruppierte Rowstore-Indizes verfügen. Früher hat der Columnstore-Index keine nicht gruppierten Indizes unterstützt. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwaltet die nicht gruppierten Indizes für DML-Vorgänge automatisch.  
  
-   Unterstützung für Primär- und Fremdschlüssel unter Verwendung eines B-Strukturindexes zum Erzwingen dieser Einschränkungen für einen gruppierten Columnstore-Index  
  
-   Columnstore-Indizes können über eine Komprimierungsverzögerungsoption verfügen, die die Auswirkungen minimiert, die die Transaktionsarbeitsauslastung auf betriebliche Echtzeitanalysen hat.  Mit dieser Option können häufig geänderte Zeilen vor der Komprimierung in den Columnstore stabilisiert werden. Weitere Informationen finden Sie unter [CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md) und [Erste Schritte mit Columnstore für operative Echtzeitanalyse](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
### <a name="performance-for-database-compatibility-level-120-or-130"></a>Leistung für Datenbankkompatibilitätsgrad 120 oder 130  
  
-   Columnstore-Indizes unterstützen die RCSI-Stufe (Read Committed Snapshot Isolation) sowie die Momentaufnahmeisolation (Snapshot Isolation, SI). Dies ermöglicht transaktionale konsistente Analyseabfragen ohne Sperren.  
  
-   Columnstore unterstützt Indexdefragmentierung durch Entfernen von gelöschten Zeilen, ohne dass der Index explizit neu erstellt werden muss. Mit der `ALTER INDEX ... REORGANIZE`-Anweisung werden gelöschte Zeilen basierend auf einer intern definierten Richtlinie in Form eines Onlinevorgangs aus dem Columnstore entfernt.  
  
-   Columnstore-Indizes können über Zugriff auf ein lesbares sekundäres Always On-Replikat verfügen. Sie können die Leistung von Betriebsanalysen verbessern, indem Sie die Analyseabfragen in ein sekundäres Always On-Replikat auslagern.  
  
-   Die Aggregatweitergabe berechnet die Aggregatfunktionen `MIN`, `MAX`, `SUM`, `COUNT` und `AVG` während Tabellenscans, wenn der Datentyp 8 Byte oder weniger nutzt und es sich nicht um einen String-Datentyp handelt. Die Aggregatweitergabe wird mit oder ohne `GROUP BY`-Klausel für gruppierte und nicht gruppierte Columnstore-Indizes unterstützt. Bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist diese Erweiterung für die Enterprise Edition vorbehalten.
  
-   Die Zeichenfolgenprädikatweitergabe beschleunigt Abfragen, bei denen Zeichenfolgen der Typen VARCHAR/CHAR oder NVARCHAR/NCHAR verglichen werden. Dies gilt für die allgemeinen Vergleichsoperatoren und schließt Operatoren wie `LIKE` ein, die Bitmapfilter verwenden. Dies funktioniert mit allen unterstützten Sortierungen. Bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist diese Erweiterung für die Enterprise Edition vorbehalten. 

-   Erweiterungen für Batchmodusvorgänge können mithilfe vektorbasierter Hardwarefunktionen genutzt werden. [!INCLUDE[ssde_md](../../includes/ssde_md.md)] erkennt die CPU-Unterstützung für AVX 2- (Erweiterte Vektorerweiterungen) und SSE 4-Hardwareerweiterungen (Streaming SIMD Extensions 4) und nutzt diese, wenn sie unterstützt werden. Bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist diese Erweiterung für die Enterprise Edition vorbehalten.
  
### <a name="performance-for-database-compatibility-level-130"></a>Leistung für Datenbankkompatibilitätsgrad 130  
  
-   Unterstützung der neuen Batchmodusausführung für Abfragen, die diese Vorgänge verwenden:  
    -   `SORT`  
    -   Aggregate mit mehrere verschiedenen Funktionen. Beispiele: `COUNT/COUNT`, `AVG/SUM`, `CHECKSUM_AGG` und `STDEV/STDEVP`  
    -   Fensteraggregatfunktionen: `COUNT`, `COUNT_BIG`, `SUM`, `AVG`, `MIN`, `MAX` und `CLR`  
    -   Benutzerdefinierte Fensteraggregate: `CHECKSUM_AGG`, `STDEV`, `STDEVP`, `VAR`, `VARP` und `GROUPING`  
    -   Analysefunktionen für Fensteraggregate: `LAG`, `LEAD`, `FIRST_VALUE`, `LAST_VALUE`, `PERCENTILE_CONT`, `PERCENTILE_DISC`, `CUME_DIST` und `PERCENT_RANK`  

-   Singlethread-Abfragen unter `MAXDOP 1` oder mit der seriellen Abfrageplanausführung im Batchmodus. Früher konnten nur Multithread-Abfragen im Batchmodus ausgeführt werden.  

-   Speicheroptimierte Tabellenabfragen können sowohl beim Zugriff auf Daten im Rowstore- als auch beim Zugriff auf Daten im Columnstore-Index parallele Pläne im SQL InterOp-Modus verwenden.
  
### <a name="supportability"></a>Unterstützbarkeit  
Diese Systemsichten sind für Columnstore neu:  
  
:::row:::
    :::column:::
        [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_column_store_object_pool &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-column-store-object-pool-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_column_store_row_group_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_column_store_row_group_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.internal_partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
  
In-Memory-OLTP-basierte DMVs enthalten Updates für den Columnstore:  

:::row:::
    :::column:::
        [sys.dm_db_xtp_hash_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-hash-index-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_xtp_memory_consumers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-memory-consumers-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_nonclustered_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-nonclustered-index-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_xtp_object_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-object-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_table_memory_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-table-memory-stats-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="limitations"></a>Einschränkungen    
-   Für In-Memory-Tabellen muss ein Columnstore-Index alle Spalten enthalten. Der Columnstore-Index kann keine gefilterte Bedingung haben.  
-   Für In-Memory-Tabellen können Abfragen für Columnstore-Indizes nur im InterOP-Modus und nicht im einheitlichen In-Memory-Modus ausgeführt werden. Parallele Ausführung wird unterstützt.  
  
## [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 In [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] wurde der gruppierten Columnstore-Index als primäres Speicherformat eingeführt. Dieser ermöglichte reguläre Ladevorgänge sowie Aktualisierungs-, Lösch- und Einfügevorgänge.  
  
-   Die Tabelle kann einen gruppierten Columnstore-Index als primären Tabellenspeicher verwenden. Für die Tabelle sind keine anderen Indizes zulässig, aber der gruppierte Columnstore-Index ist aktualisierbar, sodass Sie reguläre Ladevorgänge ausführen und Änderungen an einzelnen Zeilen vornehmen können.  
-   Der nicht gruppierte Columnstore-Index weist weiterhin die gleiche Funktionalität wie in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] auf, ermöglicht nun jedoch die Ausführung zusätzlicher Operatoren im Batchmodus. Er ist weiterhin nur durch erneutes Erstellen und mithilfe eines Partitionswechsels aktualisierbar. Der nicht gruppierte Columnstore-Index wird nur in datenträgerbasierten Tabellen und nicht in In-Memory-Tabellen unterstützt.  
-   Sowohl der gruppierte als auch der nicht gruppierte Columnstore-Index verfügt über eine Archivierungskomprimierungsoption, die die Daten weiter komprimiert. Die Archivierungsoption eignet sich zur Verringerung der Datengröße sowohl im Arbeitsspeicher als auch auf dem Datenträger, verlangsamt jedoch die Leistung der Abfrage. Dies funktioniert gut für Daten, auf die nur selten zugegriffen wird.  
-   Der gruppierte und der nicht gruppierte Columnstore-Index funktionieren auf sehr ähnliche Weise. Sie verwenden das gleiche Format für spaltenweise Speicherung, dieselbe Abfrageverarbeitungs-Engine und den gleichen Satz von dynamischen Verwaltungssichten. Der Unterschied besteht in den primären bzw. sekundären Indextypen, und darin, dass der nicht gruppierte Columnstore-Index schreibgeschützt ist.  
-   Diese Operatoren werden für Multithread-Abfragen im Batchmodus ausgeführt: SCAN, FILTER, PROJECT, JOIN, GROUP BY und UNION ALL.  
  
## [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
 In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] wurden der nicht gruppierte Columnstore-Index als weiterer Indextyp für Rowstore-Tabellen sowie die Batchverarbeitung für Abfragen für Columnstore-Daten eingeführt.  
  
-   Eine Rowstore-Tabelle kann über einen nicht gruppierten Columnstore-Index verfügen.  
-   Der Columnstore-Index ist schreibgeschützt. Nachdem Sie den Columnstore-Index erstellt haben, können Sie die Tabelle nicht mehr mit `INSERT`-, `DELETE`- und `UPDATE`-Vorgängen aktualisieren. Um diese Vorgänge auszuführen, müssen Sie den Index löschen,die Tabelle aktualisieren und den Columnstore-Index neu erstellen. Sie können zusätzliche Daten mithilfe von Partitionswechseln in die Tabelle laden. Der Vorteil der Partitionswechsel besteht darin, dass Sie Daten laden können, ohne den Columnstore-Index löschen und neu erstellen zu müssen.  
-   Der Columnstore-Index erfordert stets zusätzlichen Speicherplatz (in der Regel zusätzliche 10 % gegenüber Rowstore), da eine Kopie der Daten gespeichert wird.  
-   Die Batchverarbeitung bietet eine mindestens doppelt so gute Leistung, ist aber nur für die parallele Abfrageausführung verfügbar.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Columnstore Indexes Design Guidance (Leitfaden zum Entwerfen von Columnstore-Indizes)](../../relational-databases/indexes/columnstore-indexes-design-guidance.md)   
 [Columnstore Indexes Data Loading Guidance (Leitfaden zum Laden von Daten in Columnstore-Indizes)](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Abfrageleistung für Columnstore-Indizes](../../relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Erste Schritte mit Columnstore für operative Echtzeitanalyse](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)   
 [Columnstore-Indizes für Data Warehousing](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Neuorganisieren und Neuerstellen von Indizes](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)
  
