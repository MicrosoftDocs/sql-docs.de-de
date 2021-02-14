---
description: sys.dm_exec_function_stats (Transact-SQL)
title: sys.dm_exec_function_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/10/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_function_stats
- sys.dm_exec_function_stats_tsql
- dm_exec_function_stats
- dm_exec_function_stats_tsql
helpviewer_keywords:
- sys.dm_exec_function_stats dynamic management view
ms.assetid: 4c3d6a02-08e4-414b-90be-36b89a0e5a3a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: be2d102b81bf5535354b99cc3405d50a0bf94915
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342985"
---
# <a name="sysdm_exec_function_stats-transact-sql"></a>sys.dm_exec_function_stats (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Gibt aggregierte Leistungsstatistiken für zwischengespeicherte Funktionen zurück. Die Sicht gibt eine Zeile für jeden zwischengespeicherten Funktionsplan zurück, und die Lebensdauer der Zeile ist so lange, wie die Funktion zwischengespeichert bleibt. Wenn eine Funktion aus dem Cache entfernt wird, wird die entsprechende Zeile aus dieser Sicht gelöscht. Zu diesem Zeitpunkt wird ein Leistungsstatistik-SQL-Ablaufverfolgungsereignis ausgelöst, das **sys.dm_exec_query_stats** entspricht. Gibt Informationen zu skalaren Funktionen zurück, einschließlich in-Memory-Funktionen und CLR-Skalarfunktionen. Gibt keine Informationen zu Tabellenwert Funktionen und zu Skalarfunktionen zurück, die mit [skalaren UDF-Inlining](../../relational-databases/user-defined-functions/scalar-udf-inlining.md)Inline sind.
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]können dynamische Verwaltungssichten keine Informationen verfügbar machen, die sich auf die Datenbankkapselung auswirken würden oder die sich auf andere Datenbanken beziehen, auf die der Benutzer Zugriff hat. Um zu vermeiden, dass diese Informationen verfügbar gemacht werden, wird jede Zeile, die Daten enthält, die nicht zum verbundenen Mandanten gehören, herausgefiltert.  
  
> [!NOTE]
> Die Ergebnisse von **sys.dm_exec_function_stats**  können bei jeder Ausführung variieren, da die Daten nur abgeschlossene Abfragen widerspiegeln, und keine, die noch aktiv sind. 

  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Datenbank-ID, in der sich die Funktion befindet.|  
|**object_id**|**int**|Objekt-ID der Funktion.|  
|**type**|**char(2)**|Objekttyp: FN = skalare Wert Funktionen|  
|**type_desc**|**nvarchar(60)**|Beschreibung des Objekt Typs: SQL_SCALAR_FUNCTION|  
|**sql_handle**|**varbinary(64)**|Dies kann verwendet werden, um mit Abfragen in **sys.dm_exec_query_stats** zu korrelieren, die in dieser Funktion ausgeführt wurden.|  
|**plan_handle**|**varbinary(64)**|Bezeichner für den speicherinternen Plan. Dieser Bezeichner ist vorübergehend und bleibt nur für die Dauer der Speicherung des Plans im Cache konstant. Dieser Wert kann mit der dynamischen Verwaltungssicht **sys.dm_exec_cached_plans** verwendet werden.<br /><br /> Ist immer 0x000, wenn eine nativ kompilierte Funktion eine Speicher optimierte Tabelle abfragt.|  
|**cached_time**|**datetime**|Der Zeitpunkt, zu dem die Funktion dem Cache hinzugefügt wurde.|  
|**last_execution_time**|**datetime**|Der Zeitpunkt, zu dem die Funktion zuletzt ausgeführt wurde.|  
|**execution_count**|**bigint**|Gibt an, wie oft die Funktion seit der letzten Kompilierung ausgeführt wurde.|  
|**total_worker_time**|**bigint**|Die Gesamtmenge der CPU-Zeit (in Mikrosekunden), die von Ausführungen dieser Funktion seit der Kompilierung verbraucht wurde.<br /><br /> Bei nativ kompilierten Funktionen sind **total_worker_time** möglicherweise nicht genau, wenn viele Ausführungen weniger als 1 Millisekunde benötigen.|  
|**last_worker_time**|**bigint**|Die CPU-Zeit (in Mikrosekunden), die bei der letzten Ausführung der Funktion verbraucht wurde. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Die minimale CPU-Zeit (in Mikrosekunden), die diese Funktion seit einer einzelnen Ausführung verbraucht hat. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Maximale CPU-Zeit (in Mikrosekunden), die diese Funktion seit einer einzelnen Ausführung verbraucht hat. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Die Gesamtanzahl der physischen Lesevorgänge, die von Ausführungen dieser Funktion seit der Kompilierung durchgeführt wurden.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**last_physical_reads**|**bigint**|Anzahl physischer Lesevorgänge bei der letzten Ausführung der Funktion.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**min_physical_reads**|**bigint**|Minimale Anzahl physischer Lesevorgänge für eine einzelne Ausführung dieser Funktion.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**max_physical_reads**|**bigint**|Maximale Anzahl physischer Lesevorgänge für eine einzelne Ausführung dieser Funktion.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**total_logical_writes**|**bigint**|Die Gesamtanzahl logischer Schreibvorgänge für Ausführungen dieser Funktion seit der Kompilierung.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**last_logical_writes**|**bigint**|Die Anzahl der Pufferpoolseiten, die seit der letzten Planausführung modifiziert wurden. Wenn eine Seite bereits modifiziert (geändert) wurde, werden keine Schreibvorgänge gezählt.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**min_logical_writes**|**bigint**|Die minimale Anzahl logischer Schreibvorgänge, die diese Funktion während einer einzelnen Ausführung durchgeführt hat.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**max_logical_writes**|**bigint**|Maximale Anzahl logischer Schreibvorgänge für eine einzelne Ausführung dieser Funktion.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**total_logical_reads**|**bigint**|Die Gesamtanzahl logischer Lesevorgänge für Ausführungen dieser Funktion seit der Kompilierung.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**last_logical_reads**|**bigint**|Anzahl logischer Lesevorgänge bei der letzten Ausführung der Funktion.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**min_logical_reads**|**bigint**|Bisherige Mindestanzahl logischer Lesevorgänge für eine einzelne Ausführung dieser Funktion.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**max_logical_reads**|**bigint**|Maximale Anzahl logischer Lesevorgänge für eine einzelne Ausführung dieser Funktion.<br /><br /> Ist immer 0, wenn eine speicheroptimierte Tabelle abgefragt wird.|  
|**total_elapsed_time**|**bigint**|Insgesamt verstrichene Zeit (in Mikrosekunden) für abgeschlossene Ausführungen dieser Funktion.|  
|**last_elapsed_time**|**bigint**|Verstrichene Zeit (in Mikrosekunden) für die zuletzt abgeschlossene Ausführung dieser Funktion.|  
|**min_elapsed_time**|**bigint**|Mindestens verstrichene Zeit (in Mikrosekunden) für eine beliebige abgeschlossene Ausführung dieser Funktion.|  
|**max_elapsed_time**|**bigint**|Maximal verstrichene Zeit (in Mikrosekunden) für eine beliebige abgeschlossene Ausführung dieser Funktion.|  
|**total_page_server_reads**|**bigint**|Die Gesamtanzahl von Seiten Server Lesevorgängen, die von Ausführungen dieser Funktion seit der Kompilierung ausgeführt wurden.<br /><br /> **Gilt für:** Hyperskalierung von Azure SQL-Datenbank.|  
|**last_page_server_reads**|**bigint**|Anzahl von Seiten Server Lesevorgängen, die bei der letzten Ausführung der Funktion ausgeführt wurden.<br /><br /> **Gilt für:** Hyperskalierung von Azure SQL-Datenbank.|  
|**min_page_server_reads**|**bigint**|Die minimale Anzahl von Seiten Server Lesevorgängen, die diese Funktion jemals während einer einzelnen Ausführung ausgeführt hat.<br /><br /> **Gilt für:** Hyperskalierung von Azure SQL-Datenbank.|  
|**max_page_server_reads**|**bigint**|Maximale Anzahl von Seiten Server Lesevorgängen, die diese Funktion jemals während einer einzelnen Ausführung ausgeführt hat.<br /><br /> **Gilt für:** Hyperskalierung von Azure SQL-Datenbank.|
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden Informationen zu den zehn wichtigsten Funktionen zurückgegeben, die nach durchschnittlich verstrichener Zeit identifiziert werden.  
  
```sql  
SELECT TOP 10 d.object_id, d.database_id, OBJECT_NAME(object_id, database_id) 'function name',   
    d.cached_time, d.last_execution_time, d.total_elapsed_time,  
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],  
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_function_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungs Sichten und-Funktionen im Zusammenhang mit der Ausführung &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 
 [sys.dm_exec_trigger_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)   
 [sys.dm_exec_procedure_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  
  
  
