---
description: sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)
title: sys.dm_db_column_store_row_group_physical_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_column_store_row_group_physical_stats_TSQL
- sys.dm_db_column_store_row_group_physical_stats
- dm_db_column_store_row_group_physical_stats
- dm_db_column_store_row_group_physical_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_db_column_store_row_group_physical_stats
- sys.dm_db_column_store_row_group_physical_stats dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 020cfa1e2f1f60064f21063e0766213ab44aaa25
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464832"
---
# <a name="sysdm_db_column_store_row_group_physical_stats-transact-sql"></a>sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Stellt aktuelle Informationen zu allen columnstore--Indizes in der aktuellen Datenbank auf Zeilen Gruppenebene bereit.  

Dadurch wird die Katalog Sicht [sys.column_store_row_groups &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)erweitert.  

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID der zugrunde liegenden Tabelle.|  
|**index_id**|**int**|ID dieses columnstore--Indexes für *object_id* Tabelle.|  
|**partition_number**|**int**|ID der Tabellen Partition, die *row_group_id* enthält. Sie können partition_number verwenden, um diese DMV mit sys.partitions zu verknüpfen.|  
|**row_group_id**|**int**|ID dieser Zeilen Gruppe. Bei partitionierten Tabellen ist value innerhalb der Partition eindeutig.<br /><br /> -1 für einen in-Memory-Tail.|  
|**delta_store_hobt_id**|**bigint**|Der hobt_id für eine Zeilen Gruppe im Delta Speicher.<br /><br /> NULL, wenn die Zeilen Gruppe nicht im Delta Speicher gespeichert ist.<br /><br /> NULL für das Ende einer in-Memory-Tabelle.|  
|**state**|**tinyint**|Die *state_description* zugeordnete ID-Nummer.<br /><br /> 0 = INVISIBLE<br /><br /> 1 = OPEN<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED<br /><br /> 4 = Tombstone<br /><br /> Die Komprimierung ist der einzige Status, der für Tabellen im Arbeitsspeicher gilt.|  
|**state_desc**|**nvarchar(60)**|Beschreibung des Zeilen Gruppenstatus:<br /><br /> 0-unsichtbar: eine Zeilen Gruppe, die erstellt wird. Beispiel: <br />Eine Zeilen Gruppe im columnstore-ist unsichtbar, während die Daten komprimiert werden. Wenn die Komprimierung abgeschlossen ist, ändert ein metadatenschalter den Status der columnstore--Zeilen Gruppe von unsichtbar in komprimiert und den Status der Delta Store-Zeilen Gruppe von Closed in Tombstone.<br /><br /> 1-öffnen-eine Delta Store-Zeilen Gruppe, die neue Zeilen akzeptiert. Eine offene Zeilengruppe befindet sich weiterhin im rowstore-Format und wurde nicht in das columnstore-Format komprimiert.<br /><br /> 2-geschlossen-eine Zeilen Gruppe im Delta Speicher, die die maximale Anzahl von Zeilen enthält, und wartet darauf, dass der tupelverschiebungsprozess Sie in den columnstore komprimiert.<br /><br /> 3-komprimiert: eine Zeilen Gruppe, die mit der columnstore--Komprimierung komprimiert und im columnstore-gespeichert wird.<br /><br /> 4-Tombstone: eine Zeilen Gruppe, die sich zuvor im Delta Store befand und nicht mehr verwendet wird.|  
|**total_rows**|**bigint**|Anzahl von Zeilen, die physisch in der Zeilen Gruppe gespeichert sind. Für komprimierte Zeilen Gruppen. Schließt die Zeilen ein, die als gelöscht markiert sind.|  
|**deleted_rows**|**bigint**|Die Anzahl von Zeilen, die in einer komprimierten Zeilengruppe physisch gespeichert und zum Löschen markiert sind.<br /><br /> Für Zeilengruppen im Deltastore lautet der Wert 0.|  
|**size_in_bytes**|**bigint**|Kombinierte Größe (in Bytes) aller Seiten in dieser Zeilen Gruppe. Diese Größe enthält nicht die Größe, die zum Speichern von Metadaten oder freigegebenen Wörterbüchern erforderlich ist.|  
|**trim_reason**|**tinyint**|Der Grund dafür, dass die komprimierte Zeilen Gruppe die maximale Anzahl von Zeilen überschreitet.<br /><br /> 0-UNKNOWN_UPGRADED_FROM_PREVIOUS_VERSION<br /><br /> 1-NO_TRIM<br /><br /> 2-Bulkload<br /><br /> 3-erneute Organisation<br /><br /> 4-DICTIONARY_SIZE<br /><br /> 5-MEMORY_LIMITATION<br /><br /> 6-RESIDUAL_ROW_GROUP<br /><br /> 7-STATS_MISMATCH<br /><br /> 8-Überlauf<br /><br /> 9-AUTO_MERGE|  
|**trim_reason_desc**|**nvarchar(60)**|Beschreibung der *trim_reason*.<br /><br /> 0-UNKNOWN_UPGRADED_FROM_PREVIOUS_VERSION: ist beim Upgrade von der vorherigen Version von aufgetreten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> 1-NO_TRIM: die Zeilen Gruppe wurde nicht gekürzt. Die Zeilen Gruppe wurde mit dem Maximum von 1.048.576 Zeilen komprimiert.  Die Anzahl der Zeilen kann kleiner sein, wenn eine Teilmenge der Zeilen nach dem Schließen der Delta-Zeilen Gruppe gelöscht wurde.<br /><br /> 2-Bulkload: die Batch Größe für den Massen Ladevorgang beschränkte die Anzahl der Zeilen.<br /><br /> 3-Reorg: erzwungene Komprimierung als Teil des Befehls "Reorg".<br /><br /> 4-DICTIONARY_SIZE: die Wörterbuch Größe ist zu groß geworden, um alle Zeilen zu komprimieren.<br /><br /> 5-MEMORY_LIMITATION: nicht genügend Arbeitsspeicher, um alle Zeilen zu komprimieren.<br /><br /> 6-RESIDUAL_ROW_GROUP: als Teil der letzten Zeilen Gruppe mit Zeilen < 1 Million während des Index Erstellungs Vorgangs geschlossen<br /><br /> 7-STATS_MISMATCH: nur für columnstore-in der in-Memory-Tabelle. Wenn die Statistik >= 1 Million qualifizierten Zeilen im Ende falsch angegeben wurde, aber weniger gefunden wurde, weist die komprimierte Zeilen Gruppe < 1 Million Zeilen auf.<br /><br /> 8-Spillover: nur für columnstore-in der in-Memory-Tabelle. Wenn das Ende > 1 Million qualifizierten Zeilen aufweist, werden die letzten Batch Zeilen komprimiert, wenn die Anzahl zwischen 100 KB und 1 Million liegt.<br /><br /> 9-AUTO_MERGE: ein Merge-Vorgang des tupelverschiebungsvorgangs, der im Hintergrund ausgeführt wird, hat eine oder mehrere Zeilen Gruppen in diese Zeilen Gruppe konsolidiert.|  
|**transition_to_compressed_state**|TINYINT|Zeigt, wie diese Zeilen Gruppe aus dem Delta Store in einen komprimierten Zustand im columnstore verschoben wurde.<br /><br /> 1-NOT_APPLICABLE<br /><br /> 2-INDEX_BUILD<br /><br /> 3-TUPLE_MOVER<br /><br /> 4-REORG_NORMAL<br /><br /> 5-REORG_FORCED<br /><br /> 6-Bulkload<br /><br /> 7-zusammenführen|  
|**transition_to_compressed_state_desc**|nvarchar(60)| 1-NOT_APPLICABLE-der Vorgang gilt nicht für den Delta Store. Oder die Zeilen Gruppe wurde vor dem Upgrade auf komprimiert [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] . in diesem Fall wird der Verlauf nicht beibehalten.<br /><br /> 2-INDEX_BUILD-Indexerstellung oder indexneu Erstellung komprimierte Zeilen Gruppe.<br /><br /> 3-TUPLE_MOVER: der tupelverschiebungsvorgang, der im Hintergrund ausgeführt wird, hat die Zeilen Gruppe komprimiert. Der tupelverschiebungsvorgang erfolgt, nachdem der Status der Zeilen Gruppe von geöffnet in geschlossen geändert wurde.<br /><br /> 4-REORG_NORMAL-der neuorganisations Vorgang, Alter Index... Reorg: die geschlossene Zeilen Gruppe wurde aus dem Delta Store in den columnstore verschoben. Dieser Fehler ist aufgetreten, bevor die Zeilen Gruppe vom tupelverschiebungsverschiebungs Vorgang verschoben werden konnte.<br /><br /> 5-REORG_FORCED: diese Zeilen Gruppe war im Delta Store geöffnet und wurde im columnstore-erzwungen, bevor Sie eine vollständige Anzahl von Zeilen besaß.<br /><br /> 6-Bulkload: bei einem Massen Ladevorgang wurde die Zeilen Gruppe direkt ohne Verwendung des Delta Stores komprimiert.<br /><br /> 7-Merge-ein Mergevorgang hat eine oder mehrere Zeilen Gruppen in diese Zeilen Gruppe konsolidiert und dann die columnstore--Komprimierung ausgeführt.|  
|**has_vertipaq_optimization**|bit|Vertipq-Optimierung verbessert die columnstore--Komprimierung durch Neuanordnen der Reihenfolge der Zeilen in der Zeilen Gruppe, um eine höhere Komprimierung zu erreichen. Diese Optimierung erfolgt in den meisten Fällen automatisch. Es gibt zwei Fälle, in denen vertipq-Optimierung nicht verwendet wird:<br/>  a. Wenn eine Delta-Zeilen Gruppe in den columnstore-verschoben wird und mindestens ein nicht gruppierter Index für den columnstore--Index vorhanden ist, wird die vertigtq-Optimierung übersprungen, um die Änderungen am Zuordnungsindex zu minimieren.<br/> b. für columnstore--Indizes für Speicher optimierte Tabellen. <br /><br /> 0 = Nein<br /><br /> 1 = Ja|  
|**Stro**|BIGINT|Die Zeilen Gruppen Generierung, die dieser Zeilen Gruppe zugeordnet ist.|  
|**created_time**|datetime2|Uhrzeit, zu der die Zeilen Gruppe erstellt wurde.<br /><br /> NULL: bei einem columnstore--Index für eine in-Memory-Tabelle.|  
|**closed_time**|datetime2|Uhrzeit, zu der die Zeilen Gruppe geschlossen wurde.<br /><br /> NULL: bei einem columnstore--Index für eine in-Memory-Tabelle.|  
| &nbsp; | &nbsp; | &nbsp; |

## <a name="results"></a>Ergebnisse  
 Gibt eine Zeile für jede Zeilen Gruppe in der aktuellen Datenbank zurück.  
  
## <a name="permissions"></a>Berechtigungen  
Erfordert `CONTROL` die-Berechtigung für die-Tabelle und `VIEW DATABASE STATE` die-Berechtigung für die Datenbank.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-calculate-fragmentation-to-decide-when-to-reorganize-or-rebuild-a-columnstore-index"></a>A. Berechnen Sie die Fragmentierung, um zu entscheiden, wann Sie einen columnstore--Index neu organisieren oder neu erstellen.  
 Bei columnstore--Indizes ist der Prozentsatz gelöschter Zeilen ein gutes Maß für die Fragmentierung in einer Zeilen Gruppe. Wenn die Fragmentierung 20% oder mehr beträgt, entfernen Sie die gelöschten Zeilen. Weitere Beispiele finden Sie unter [Neuorganisieren und Neuerstellen von Indizes](~/relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
 In diesem Beispiel wird **sys.dm_db_column_store_row_group_physical_stats** mit anderen Systemtabellen verbunden, und anschließend wird die `Fragmentation` Spalte als Schätzung der Effizienz der einzelnen Zeilen Gruppen in der aktuellen Datenbank berechnet. Wenn Sie Informationen zu einer einzelnen Tabelle suchen möchten, entfernen Sie die Kommentar-Bindestriche vor der **Where** -Klausel, und geben Sie einen Tabellennamen an.  
  
```sql  
SELECT i.object_id,   
    object_name(i.object_id) AS TableName,   
    i.name AS IndexName,   
    i.index_id,   
    i.type_desc,   
    CSRowGroups.*,  
    100*(ISNULL(deleted_rows,0))/NULLIF(total_rows,0) AS 'Fragmentation'
FROM sys.indexes AS i  
JOIN sys.dm_db_column_store_row_group_physical_stats AS CSRowGroups  
    ON i.object_id = CSRowGroups.object_id AND i.index_id = CSRowGroups.index_id   
-- WHERE object_name(i.object_id) = 'table_name'   
ORDER BY object_name(i.object_id), i.name, row_group_id;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)      
 [Columnstore Index Architecture (Columnstore-Indizes: Architektur)](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)         
 [Abfragen der SQL Server System Katalog-FAQ](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.ALL_COLUMNS &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)  
 [sys.column_store_dictionaries &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)   
 [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
  
