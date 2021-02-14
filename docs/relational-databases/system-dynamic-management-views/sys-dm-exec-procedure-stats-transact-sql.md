---
description: sys.dm_exec_procedure_stats (Transact-SQL)
title: sys.dm_exec_procedure_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_procedure_stats_TSQL
- dm_exec_procedure_stats_TSQL
- dm_exec_procedure_stats
- sys.dm_exec_procedure_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_procedure_stats dynamic management view
ms.assetid: ab8ddde8-1cea-4b41-a7e4-697e6ddd785a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 14292947fb8ff77df9c00d28eb35de8f55ce8b33
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342992"
---
# <a name="sysdm_exec_procedure_stats-transact-sql"></a>sys.dm_exec_procedure_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt die zusammengefasste Leistungsstatistik für zwischengespeicherte gespeicherte Prozeduren zurück. Diese Sicht gibt eine Zeile für jeden Plan der zwischengespeicherten gespeicherten Prozedur zurück, und die Lebensdauer der Zeile entspricht der Verweildauer der gespeicherten Prozedur im Cache. Bei Entfernung einer gespeicherten Prozedur aus dem Cache wird die entsprechende Zeile aus dieser Sicht gelöscht. Zu diesem Zeitpunkt wird ein Leistungsstatistik-SQL-Ablaufverfolgungsereignis ausgelöst, das **sys.dm_exec_query_stats** entspricht.  
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]können dynamische Verwaltungssichten keine Informationen verfügbar machen, die sich auf die Datenbankkapselung auswirken würden oder die sich auf andere Datenbanken beziehen, auf die der Benutzer Zugriff hat. Um zu vermeiden, dass diese Informationen verfügbar gemacht werden, wird jede Zeile, die Daten enthält, die nicht zum verbundenen Mandanten gehören, herausgefiltert.  
  
> [!NOTE]
> Die Ergebnisse von **sys.dm_exec_procedure_stats**  können bei jeder Ausführung variieren, da die Daten nur abgeschlossene Abfragen widerspiegeln, und keine, die noch aktiv sind.
> Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_exec_procedure_stats**. 

  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------| 
|**database_id**|**int**|ID der Datenbank, in der sich die gespeicherte Prozedur befindet.|  
|**object_id**|**int**|Objekt-ID der gespeicherten Prozedur.|  
|**type**|**char(2)**|Der Objekttyp:<br /><br /> P = Gespeicherte SQL-Prozedur<br /><br /> PC = Gespeicherte Assemblyprozedur (CLR)<br /><br /> X = Erweiterte gespeicherte Prozedur|  
|**type_desc**|**nvarchar(60)**|Beschreibung des Objekttyps:<br /><br /> SQL_STORED_PROCEDURE<br /><br /> CLR_STORED_PROCEDURE<br /><br /> EXTENDED_STORED_PROCEDURE|  
|**sql_handle**|**varbinary(64)**|Dies kann verwendet werden, um mit Abfragen in **sys.dm_exec_query_stats** zu korrelieren, die in dieser gespeicherten Prozedur ausgeführt wurden.|  
|**plan_handle**|**varbinary(64)**|Bezeichner für den speicherinternen Plan. Dieser Bezeichner ist vorübergehend und bleibt nur für die Dauer der Speicherung des Plans im Cache konstant. Dieser Wert kann mit der dynamischen Verwaltungssicht **sys.dm_exec_cached_plans** verwendet werden.<br /><br /> Ist immer 0x000, wenn eine systemintern kompilierte gespeicherte Prozedur eine speicheroptimierte Tabelle abfragt.|  
|**cached_time**|**datetime**|Der Zeitpunkt, zu dem die gespeicherte Prozedur dem Cache hinzugefügt wurde.|  
|**last_execution_time**|**datetime**|Der Zeitpunkt, zu dem die gespeicherte Prozedur zuletzt ausgeführt wurde.|  
|**execution_count**|**bigint**|Die Anzahl der Ausführungen der gespeicherten Prozedur seit der letzten Kompilierung.|  
|**total_worker_time**|**bigint**|Die Gesamtmenge an CPU-Zeit (in Mikrosekunden), die von Ausführungen dieser gespeicherten Prozedur seit der Kompilierung verbraucht wurde.<br /><br /> Wenn zahlreiche Ausführungen weniger als 1 Millisekunde dauern, wird **total_worker_time** bei nativ kompilierten gespeicherten Prozeduren u.U. nicht exakt angegeben.|  
|**last_worker_time**|**bigint**|Die CPU-Zeit (in Mikrosekunden) für die letzte Ausführung der gespeicherten Prozedur. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Die minimale CPU-Zeit (in Mikrosekunden), die diese gespeicherte Prozedur bei einer einzelnen Ausführung jemals verbraucht hat. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Die maximale CPU-Zeit (in Mikrosekunden), die diese gespeicherte Prozedur bei einer einzelnen Ausführung jemals verbraucht hat. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Die Gesamtanzahl der physischen Lesevorgänge, die von Ausführungen dieser gespeicherten Prozedur seit der Kompilierung durchgeführt wurden.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**last_physical_reads**|**bigint**|Die Anzahl der physischen Lesevorgänge, die bei der letzten Ausführung der gespeicherten Prozedur ausgeführt wurden.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**min_physical_reads**|**bigint**|Die minimale Anzahl physischer Lesevorgänge für eine einzelne Ausführung dieser gespeicherten Prozedur.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**max_physical_reads**|**bigint**|Die maximal zulässige Anzahl physischer Lesevorgänge für eine einzelne Ausführung dieser gespeicherten Prozedur.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**total_logical_writes**|**bigint**|Die Gesamtanzahl logischer Schreibvorgänge für Ausführungen dieser gespeicherten Prozedur seit der Kompilierung.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**last_logical_writes**|**bigint**|Die Anzahl der Pufferpool Seiten, die bei der letzten Ausführung des Plans dirgebunden wurden. Wenn eine Seite bereits modifiziert (geändert) wurde, werden keine Schreibvorgänge gezählt.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**min_logical_writes**|**bigint**|Die minimale Anzahl logischer Schreibvorgänge für eine einzelne Ausführung dieser gespeicherten Prozedur.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**max_logical_writes**|**bigint**|Die maximale Anzahl logischer Schreibvorgänge für eine einzelne Ausführung dieser gespeicherten Prozedur.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**total_logical_reads**|**bigint**|Die Gesamtanzahl logischer Lesevorgänge für Ausführungen dieser gespeicherten Prozedur seit der Kompilierung.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**last_logical_reads**|**bigint**|Die Anzahl logischer Lesevorgänge bei der letzten Ausführung der gespeicherten Prozedur.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**min_logical_reads**|**bigint**|Die Mindestanzahl logischer Lesevorgänge für eine einzelne Ausführung dieser gespeicherten Prozedur.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**max_logical_reads**|**bigint**|Die maximale Anzahl logischer Lesevorgänge für eine einzelne Ausführung dieser gespeicherten Prozedur.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**total_elapsed_time**|**bigint**|Die insgesamt verstrichene Zeit (in Mikrosekunden) für abgeschlossene Ausführungen dieser gespeicherten Prozedur.|  
|**last_elapsed_time**|**bigint**|Die verstrichene Zeit (in Mikrosekunden) für die zuletzt abgeschlossene Ausführung dieser gespeicherten Prozedur.|  
|**min_elapsed_time**|**bigint**|Die mindestens verstrichene Zeit (in Mikrosekunden) für eine beliebige abgeschlossene Ausführung dieser gespeicherten Prozedur.|  
|**max_elapsed_time**|**bigint**|Die maximal verstrichene Zeit (in Mikrosekunden) für eine beliebige abgeschlossene Ausführung dieser gespeicherten Prozedur.|  
|**total_spills**|**bigint**|Die Gesamtanzahl der Seiten, die durch die Ausführung dieser gespeicherten Prozedur seit der Kompilierung übergegangen sind.<br /><br /> **Gilt für**: ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**last_spills**|**bigint**|Die Anzahl der Seiten, die bei der letzten Ausführung der gespeicherten Prozedur übergegangen sind.<br /><br /> **Gilt für**: ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**min_spills**|**bigint**|Die Mindestanzahl von Seiten, die diese gespeicherte Prozedur während einer einzelnen Ausführung jemals übergegangen ist.<br /><br /> **Gilt für**: ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**max_spills**|**bigint**|Die maximale Anzahl von Seiten, die diese gespeicherte Prozedur während einer einzelnen Ausführung jemals übergegangen ist.<br /><br /> **Gilt für**: ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**pdw_node_id**|**int**|Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.<br /><br />**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]|  
|**total_page_server_reads**|**bigint**|Die Gesamtanzahl von Seiten Server Lesevorgängen, die von Ausführungen dieser gespeicherten Prozedur seit der Kompilierung ausgeführt wurden.<br /><br /> **Gilt für**: hyperskalierung von Azure SQL-Datenbank|  
|**last_page_server_reads**|**bigint**|Die Anzahl von Seiten Lesevorgängen, die bei der letzten Ausführung der gespeicherten Prozedur ausgeführt wurden.<br /><br /> **Gilt für**: hyperskalierung von Azure SQL-Datenbank|  
|**min_page_server_reads**|**bigint**|Die Mindestanzahl von Seiten Server Lesevorgängen, die diese gespeicherte Prozedur jemals während einer einzelnen Ausführung ausgeführt hat.<br /><br /> **Gilt für**: hyperskalierung von Azure SQL-Datenbank|  
|**max_page_server_reads**|**bigint**|Die maximale Anzahl von Seiten Server Lesevorgängen, die diese gespeicherte Prozedur jemals während einer einzelnen Ausführung ausgeführt hat.<br /><br /> **Gilt für**: hyperskalierung von Azure SQL-Datenbank|  
  
 <sup>1</sup> wenn die Statistik Sammlung für System intern kompilierte gespeicherte Prozeduren aktiviert ist, wird die workerzeit in Millisekunden erfasst. Wird die Abfrage in weniger als einer Millisekunde ausgeführt, lautet der Wert 0.  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
   
## <a name="remarks"></a>Bemerkungen  
 Die Statistik in der Sicht wird aktualisiert, wenn die Ausführung einer gespeicherten Prozedur abgeschlossen ist.  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel gibt Informationen zu den 10 gespeicherten Prozeduren mit der höchsten durchschnittlich verstrichenen Zeit zurück.  
  
```sql  
SELECT TOP 10 d.object_id, d.database_id, OBJECT_NAME(object_id, database_id) 'proc name',   
    d.cached_time, d.last_execution_time, d.total_elapsed_time,  
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],  
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_procedure_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[Dynamische Verwaltungs Sichten und-Funktionen im Zusammenhang mit der Ausführung &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
[sys.dm_exec_sql_text &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
[sys.dm_exec_query_plan &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_query_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)    
[sys.dm_exec_trigger_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)    
[sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)    
  
  


