---
description: sys.dm_exec_query_profiles (Transact-SQL)
title: sys.dm_exec_query_profiles (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_query_profiles_TSQL
- sys.dm_exec_query_profiles_TSQL
- dm_exec_query_profiles
- sys.dm_exec_query_profiles
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_profiles dynamic management view
ms.assetid: 54efc6cb-eea8-4f6d-a4d0-aa05eeb54081
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 313603678820b1f28c7854507befc69520653fc7
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839319"
---
# <a name="sysdm_exec_query_profiles-transact-sql"></a>sys.dm_exec_query_profiles (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

Überwacht den Abfragestatus einer ausgeführten Abfrage in Echtzeit. Verwenden Sie beispielsweise diese DMV, um zu ermitteln, welcher Teil der Abfrage langsam ausgeführt wird. Verknüpfen Sie diese DMV mit anderen System-DMVs, indem Sie die im Beschreibungsfeld angegebenen Spalten verwenden. Sie können diese DMV aber auch mit anderen Leistungsindikatoren (z. B. Systemmonitor, xperf) verknüpfen, indem Sie die timestamp-Spalten verwenden.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
Die zurückgegebenen Leistungsindikatoren gelten pro Operator und pro Thread. Die Ergebnisse sind dynamisch und stimmen nicht mit den Ergebnissen vorhandener Optionen, wie z `SET STATISTICS XML ON` . b. der Ausgabe der Ausgabe ab, wenn die Abfrage abgeschlossen ist.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|Identifiziert die Sitzung, in der die Abfrage ausgeführt wird. Verweist auf dm_exec_sessions.session_id.|  
|request_id|**int**|Identifiziert die Zielanforderung. Verweist auf dm_exec_sessions.request_id.|  
|sql_handle|**varbinary(64)**|Ein Token, das den Batch oder die gespeicherte Prozedur eindeutig identifiziert, zu der die Abfrage gehört. Verweist auf dm_exec_query_stats.sql_handle.|  
|plan_handle|**varbinary(64)**|Ein Token, das einen Abfrage Ausführungsplan für einen Batch eindeutig identifiziert, der ausgeführt wurde und dessen Plan sich im Plancache befindet oder gerade ausgeführt wird. Verweist auf dm_exec_query_stats. plan_handle.|  
|physical_operator_name|**nvarchar(256)**|Der Name des physischen Operators.|  
|node_id|**int**|Identifiziert einen Operatorknoten in der Abfragestruktur.|  
|thread_id|**int**|Unterscheidet die Threads (für eine parallele Abfrage), die zu demselben Abfrageoperatorknoten gehören.|  
|task_address|**varbinary(8)**|Identifiziert den SQLOS-Task, den dieser Thread verwendet. Verweist auf dm_os_tasks.task_address.|  
|row_count|**bigint**|Anzahl der bisher vom Operator zurückgegebenen Zeilen.|  
|rewind_count|**bigint**|Anzahl der bisherigen Zurückspulvorgänge.|  
|rebind_count|**bigint**|Anzahl der bisherigen erneuten Bindungen.|  
|end_of_scan_count|**bigint**|Anzahl der bisherigen Scanenden.|  
|estimate_row_count|**bigint**|Geschätzte Anzahl von Zeilen. Es kann nützlich sein, "estimated_row_count" mit dem tatsächlichen "row_count" zu vergleichen.|  
|first_active_time|**bigint**|Die Zeit in Millisekunden, zu der der Operator zuerst aufgerufen wurde.|  
|last_active_time|**bigint**|Die Zeit in Millisekunden, zu der der Operator zuletzt aufgerufen wurde.|  
|open_time|**bigint**|Zeitstempel beim Öffnen (in Millisekunden).|  
|first_row_time|**bigint**|Zeitstempel beim Öffnen der ersten Zeile (in Millisekunden).|  
|last_row_time|**bigint**|Zeitstempel beim Öffnen der letzten Zeile (in Millisekunden).|  
|close_time|**bigint**|Zeitstempel beim Schließen (in Millisekunden).|  
|elapsed_time_ms|**bigint**|Die gesamte verstrichene Zeit (in Millisekunden), die bisher durch die Vorgänge des Ziel Knotens verwendet wurde.|  
|cpu_time_ms|**bigint**|Die CPU-Gesamtzeit (in Millisekunden), die bisher durch die Vorgänge des Ziel Knotens verwendet wurde.|  
|database_id|**smallint**|ID der Datenbank, die das Objekt enthält, für das die Lese- und Schreibvorgänge ausgeführt werden.|  
|object_id|**int**|Der Bezeichner für das Objekt, für das die Lese- und Schreibvorgänge ausgeführt werden. Verweist auf "sys.objects.object_id".|  
|index_id|**int**|Der Index (sofern vorhanden), für den das Rowset geöffnet wird.|  
|scan_count|**bigint**|Anzahl der bisherigen Tabellen-/Indexscans.|  
|logical_read_count|**bigint**|Anzahl der bisherigen logischen Lesevorgänge.|  
|physical_read_count|**bigint**|Anzahl der bisherigen physischen Lesevorgänge.|  
|read_ahead_count|**bigint**|Anzahl der bisherigen Read-Ahead-Lesevorgänge.|  
|write_page_count|**bigint**|Anzahl der bisherigen page-writes-Schreibvorgänge aufgrund eines Überlaufs.|  
|lob_logical_read_count|**bigint**|Anzahl der bisherigen logischen LOB-Lesevorgänge.|  
|lob_physical_read_count|**bigint**|Anzahl der bisherigen physischen LOB-Lesevorgänge.|  
|lob_read_ahead_count|**bigint**|Anzahl der bisherigen Read-Ahead-LOB-Lesevorgänge.|  
|segment_read_count|**int**|Anzahl der bisherigen Segment-Read-Ahead-Lesevorgänge.|  
|segment_skip_count|**int**|Anzahl der bisher übersprungenen Segmente.| 
|actual_read_row_count|**bigint**|Anzahl der von einem Operator gelesenen Zeilen, bevor das Rest-Prädikat angewendet wurde.| 
|estimated_read_row_count|**bigint**|**Gilt für:** Ab [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] SP1. <br/>Anzahl der Zeilen, die von einem Operator vor dem Anwenden des Rest-Prädikats gelesen werden sollen.|  
  
## <a name="general-remarks"></a>Allgemeine Hinweise  
 Wenn der Abfrageplan Knoten keine e/a-Vorgänge hat, werden alle e/a-bezogenen Leistungsindikatoren auf NULL festgelegt.  
  
 Die e/a-bezogenen Leistungsindikatoren, die von dieser DMV gemeldet werden, sind `SET STATISTICS IO` in den folgenden zwei Punkten präziser als die, die von gemeldet werden:  
  
-   `SET STATISTICS IO` gruppiert die Zähler für alle e/a-Vorgänge für eine bestimmte Tabelle. Mit dieser DMV erhalten Sie separate Leistungsindikatoren für jeden Knoten im Abfrageplan, der e/a-Vorgänge für die Tabelle ausführt.  
  
-   Bei einem parallelen Scan meldet diese DMV Leistungsindikatoren für jeden der parallelen Threads für den Scan.
 
Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 ist die *Standard Infrastruktur für die Abfrage Ausführungs Statistik-Profilerstellung* parallel mit einer vereinfachten Profil Erstellungs Infrastruktur für die *Abfrage Ausführungs Statistik* vorhanden. `SET STATISTICS XML ON` und `SET STATISTICS PROFILE ON` verwenden stets die *Standardprofil Erstellungs Infrastruktur für die Abfrage Ausführungs Statistik*. Um aufgefüllt `sys.dm_exec_query_profiles` zu werden, muss eine der Abfrage Profil Erstellungs Infrastrukturen aktiviert werden. Weitere Informationen finden Sie unter [Profilerstellungsinfrastruktur für Abfragen](../../relational-databases/performance/query-profiling-infrastructure.md).    

>[!NOTE]
> Die Abfrage, die untersucht wird, muss gestartet werden, **nachdem** die Infrastruktur für die Abfrage Profilerstellung aktiviert wurde, sodass Sie nach dem Start der Abfrage keine Ergebnisse in erzeugt `sys.dm_exec_query_profiles` . Weitere Informationen zum Aktivieren der Abfrage Profil Erstellungs Infrastrukturen finden Sie unter [Infrastruktur für die Abfrage Profilerstellung](../../relational-databases/performance/query-profiling-infrastructure.md).

## <a name="permissions"></a>Berechtigungen  
Unter [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] und Azure SQL verwaltete Instanz sind `VIEW DATABASE STATE` Berechtigungen und die Mitgliedschaft in der `db_owner` Daten Bank Rolle erforderlich.   
Bei [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Premium-Tarifen ist die- `VIEW DATABASE STATE` Berechtigung in der Datenbank erforderlich. Unter für [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] die Dienst Ziele "Basic", "S0" und "S1" in SQL-Datenbank und für Datenbanken in Pools für elastische Datenbanken ist das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
   
## <a name="examples"></a>Beispiele  
 Schritt 1: Melden Sie sich bei einer Sitzung an, in der Sie die Abfrage ausführen möchten, die Sie analysieren möchten `sys.dm_exec_query_profiles` . So konfigurieren Sie die Abfrage für die Profilerstellung `SET STATISTICS PROFILE ON` Führen Sie Ihre Abfrage in derselben Sitzung aus.  
  
```sql  
--Configure query for profiling with sys.dm_exec_query_profiles  
SET STATISTICS PROFILE ON;  
GO  

--Or enable query profiling globally under SQL Server 2016 SP1 or above (not needed in SQL Server 2019)  
DBCC TRACEON (7412, -1);  
GO 
  
--Next, run your query in this session, or in any other session if query profiling has been enabled globally 
```  
  
 Schritt 2: Melden Sie sich bei einer zweiten Sitzung an, die sich von der Sitzung unterscheidet, in der die Abfrage ausgeführt wird.  
  
 Die folgende Anweisung fasst den Fortschritt der Abfrage zusammen, die derzeit in Sitzung 54 ausgeführt wird. Zu diesem Zweck wird die Gesamtzahl der Ausgabezeilen aller Threads für jeden Knoten berechnet und mit der geschätzten Anzahl an Ausgabezeilen für diesen Knoten verglichen.  
  
```sql  
--Run this in a different session than the session in which your query is running. 
--Note that you may need to change session id 54 below with the session id you want to monitor.
SELECT node_id,physical_operator_name, SUM(row_count) row_count, 
  SUM(estimate_row_count) AS estimate_row_count, 
  CAST(SUM(row_count)*100 AS float)/SUM(estimate_row_count)  
FROM sys.dm_exec_query_profiles   
WHERE session_id=54
GROUP BY node_id,physical_operator_name  
ORDER BY node_id;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Execution Related Dynamic Management Views and Functions &#40;Transact-SQL&#41; (Dynamische Verwaltungssichten und Funktionen im Zusammenhang mit der Ausführung (Transact-SQL))](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
