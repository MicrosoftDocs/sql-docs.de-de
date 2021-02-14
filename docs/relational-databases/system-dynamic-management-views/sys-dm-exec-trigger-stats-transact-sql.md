---
description: sys.dm_exec_trigger_stats (Transact-SQL)
title: sys.dm_exec_trigger_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_trigger_stats
- dm_exec_trigger_stats_TSQL
- sys.dm_exec_trigger_stats_TSQL
- sys.dm_exec_trigger_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_trigger_stats dynamic management function
ms.assetid: 863498b4-849c-434d-b748-837411458738
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bd36c570985d5b8df124a8ff793cdeee9330e128
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342887"
---
# <a name="sysdm_exec_trigger_stats-transact-sql"></a>sys.dm_exec_trigger_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt die Aggregatleistungsstatistik für zwischengespeicherte Trigger zurück. Diese Sicht enthält eine Zeile pro Trigger, und die Lebensdauer der Zeile entspricht der Verweildauer des Triggers im Cache. Bei Entfernung eines Triggers aus dem Cache wird die entsprechende Zeile aus dieser Sicht gelöscht. Zu diesem Zeitpunkt wird ein Leistungsstatistik-SQL-Ablaufverfolgungsereignis ausgelöst, das **sys.dm_exec_query_stats** entspricht.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID der Datenbank, in der sich der Trigger befindet.|  
|**object_id**|**int**|Objekt-ID des Triggers.|  
|**type**|**char(2)**|Der Objekttyp:<br /><br /> TA = Assembly (CLR) Trigger<br /><br /> TR = SQL-Trigger|  
|**Type_desc**|**nvarchar(60)**|Beschreibung des Objekttyps:<br /><br /> CLR_TRIGGER<br /><br /> SQL_TRIGGER|  
|**sql_handle**|**varbinary(64)**|Kann zur Korrelation mit Abfragen in **sys.dm_exec_query_stats** verwendet werden, die aus diesem Trigger ausgeführt wurden.|  
|**plan_handle**|**varbinary(64)**|Bezeichner für den speicherinternen Plan. Dieser Bezeichner ist vorübergehend und bleibt nur für die Dauer der Speicherung des Plans im Cache konstant. Dieser Wert kann mit der dynamischen Verwaltungssicht **sys.dm_exec_cached_plans** verwendet werden.|  
|**cached_time**|**datetime**|Der Zeitpunkt, zu dem der Trigger dem Cache hinzugefügt wurde.|  
|**last_execution_time**|**datetime**|Der Zeitpunkt, zu dem der Trigger zuletzt ausgeführt wurde.|  
|**execution_count**|**bigint**|Die Anzahl der Ausführungen des Auslösers seit der letzten Kompilierung.|  
|**total_worker_time**|**bigint**|Die Gesamtmenge der CPU-Zeit (in Mikrosekunden), die von Ausführungen dieses Auslösers seit der Kompilierung verbraucht wurde.|  
|**last_worker_time**|**bigint**|CPU-Zeit (in Mikrosekunden) für die letzte Ausführung des Triggers.|  
|**min_worker_time**|**bigint**|Die maximale CPU-Zeit (in Mikrosekunden), die dieser Triggern während einer einzelnen Ausführung verbraucht hat.|  
|**max_worker_time**|**bigint**|Die maximale CPU-Zeit (in Mikrosekunden), die dieser Triggern während einer einzelnen Ausführung verbraucht hat.|  
|**total_physical_reads**|**bigint**|Die Gesamtanzahl der physischen Lesevorgänge, die von Ausführungen dieses Auslösers seit der Kompilierung durchgeführt wurden.|  
|**last_physical_reads**|**bigint**|Die Anzahl physischer Lesevorgänge bei der letzten Ausführung des Auslösers.|  
|**min_physical_reads**|**bigint**|Die minimale Anzahl physischer Lesevorgänge für eine einzelne Ausführung dieses Auslösers.|  
|**max_physical_reads**|**bigint**|Die maximal zulässige Anzahl physischer Lesevorgänge für eine einzelne Ausführung dieses Auslösers.|  
|**total_logical_writes**|**bigint**|Die Gesamtanzahl logischer Schreibvorgänge für Ausführungen dieses Auslösers seit der Kompilierung.|  
|**last_logical_writes**|**bigint**|Die Anzahl der logischen Schreibvorgänge, die bei der letzten Ausführung des Auslösers ausgeführt wurden.|  
|**min_logical_writes**|**bigint**|Die minimale Anzahl logischer Schreibvorgänge für eine einzelne Ausführung dieses Auslösers.|  
|**max_logical_writes**|**bigint**|Die maximale Anzahl logischer Schreibvorgänge für eine einzelne Ausführung dieses Auslösers.|  
|**total_logical_reads**|**bigint**|Die Gesamtanzahl logischer Lesevorgänge für Ausführungen dieses Auslösers seit der Kompilierung.|  
|**last_logical_reads**|**bigint**|Die Anzahl logischer Lesevorgänge bei der letzten Ausführung des Auslösers.|  
|**min_logical_reads**|**bigint**|Die Mindestanzahl logischer Lesevorgänge für eine einzelne Ausführung dieses Auslösers.|  
|**max_logical_reads**|**bigint**|Die maximale Anzahl logischer Lesevorgänge für eine einzelne Ausführung dieses Auslösers.|  
|**total_elapsed_time**|**bigint**|Die insgesamt verstrichene Zeit (in Mikrosekunden) für abgeschlossene Ausführungen dieses Auslösers.|  
|**last_elapsed_time**|**bigint**|Verstrichene Zeit (in Mikrosekunden) für die letzte abgeschlossene Ausführung dieses Triggers.|  
|**min_elapsed_time**|**bigint**|Die mindestens verstrichene Zeit (in Mikrosekunden) für eine beliebige abgeschlossene Ausführung dieses Auslösers.|  
|**max_elapsed_time**|**bigint**|Die maximal verstrichene Zeit (in Mikrosekunden) für eine beliebige abgeschlossene Ausführung dieses Auslösers.| 
|**total_spills**|**bigint**|Die Gesamtanzahl der Seiten, die durch die Ausführung dieses Auslösers seit der Kompilierung übergegangen sind.<br /><br /> **Gilt für**: ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**last_spills**|**bigint**|Die Anzahl der Seiten, die beim letzten Ausführen des Auslösers übergegangen sind.<br /><br /> **Gilt für**: ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**min_spills**|**bigint**|Die Mindestanzahl von Seiten, die dieser Triggern während einer einzelnen Ausführung übergegangen ist.<br /><br /> **Gilt für**: ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**max_spills**|**bigint**|Die maximale Anzahl von Seiten, die dieser Triggern während einer einzelnen Ausführung übergegangen ist.<br /><br /> **Gilt für**: ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**total_page_server_reads**|**bigint**|Die Gesamtanzahl von Seiten Server Lesevorgängen, die von Ausführungen dieses Auslösers seit der Kompilierung ausgeführt wurden.<br /><br /> **Gilt für**: hyperskalierung von Azure SQL-Datenbank|  
|**last_page_server_reads**|**bigint**|Die Anzahl der Seiten Server Lesevorgänge bei der letzten Ausführung des Auslösers.<br /><br /> **Gilt für**: hyperskalierung von Azure SQL-Datenbank|  
|**min_page_server_reads**|**bigint**|Die Mindestanzahl von Seiten Server Lesevorgängen, die dieser Triggern während einer einzelnen Ausführung ausgeführt hat.<br /><br /> **Gilt für**: hyperskalierung von Azure SQL-Datenbank|  
|**max_page_server_reads**|**bigint**|Die maximale Anzahl von Seiten Server Lesevorgängen, die dieser Triggern während einer einzelnen Ausführung ausgeführt hat.<br /><br /> **Gilt für**: hyperskalierung von Azure SQL-Datenbank|  

  
## <a name="remarks"></a>Bemerkungen  
 In [!INCLUDE[ssSDS](../../includes/sssds-md.md)]können dynamische Verwaltungssichten keine Informationen verfügbar machen, die sich auf die Datenbankkapselung auswirken würden oder die sich auf andere Datenbanken beziehen, auf die der Benutzer Zugriff hat. Um zu vermeiden, dass diese Informationen verfügbar gemacht werden, wird jede Zeile, die Daten enthält, die nicht zum verbundenen Mandanten gehören, herausgefiltert.  

Statistiken in der Sicht werden nach Abschluss einer Abfrage aktualisiert.  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel gibt Informationen zu den fünf Triggern mit der höchsten durchschnittlichen verstrichenen Zeit zurück.  
  
```sql  
SELECT TOP 5 d.object_id, d.database_id, DB_NAME(database_id) AS 'database_name',   
    OBJECT_NAME(object_id, database_id) AS 'trigger_name', d.cached_time,  
    d.last_execution_time, d.total_elapsed_time,   
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],   
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_trigger_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[Dynamische Verwaltungs Sichten und-Funktionen im Zusammenhang mit der Ausführung &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
[sys.dm_exec_sql_text &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
[sys.dm_exec_query_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
[sys.dm_exec_procedure_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)   
[sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
  
