---
title: Profilerstellungsinfrastruktur für Abfragen | Microsoft-Dokumentation
description: Erfahren Sie, wie die SQL Server-Datenbank-Engine auf Laufzeitinformationen zu Abfrageausführungsplänen zugreift, um den Workload und die Art und Ursache der Ressourcennutzung zu verstehen.
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- query plans [SQL Server]
- execution plans [SQL Server]
- query profiling
- lightweight query profiling
- lightweight profiling
- lwp
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 39e32e8715c92a14faf68fd22464a50fed349997
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347697"
---
# <a name="query-profiling-infrastructure"></a>Profilerstellungsinfrastruktur für Abfragen
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Die [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] bietet die Möglichkeit, auf Laufzeitinformationen für Abfrageausführungspläne zuzugreifen. Eine der wichtigsten Aktionen beim Auftreten eines Leistungsproblems besteht darin, ein genaues Verständnis der Workload zu erlangen, die ausgeführt wird, und zu ermitteln, wie die Ressourcenauslastung gesteuert wird. Für diese Erkenntnisse ist Zugriff auf den [tatsächlichen Ausführungsplan](../../relational-databases/performance/display-an-actual-execution-plan.md) wichtig.

Während der Abschluss der Abfrage eine Voraussetzung für die Verfügbarkeit eines aktuellen Abfrageplans ist, können [Live-Abfragestatistiken](../../relational-databases/performance/live-query-statistics.md) Einblicke in Echtzeit in den Abfrageausführungsprozess gewähren, während die Daten von einem [Abfrageplanoperator](../../relational-databases/showplan-logical-and-physical-operators-reference.md) zu einem anderen fließen. Der Live-Abfrageplan zeigt den gesamten Abfragestatus und die Laufzeit-Ausführungsstatistik auf Operatorebene an, wie z.B. die Anzahl der erzeugten Zeilen, die verstrichene Zeit, den Operatorstatus usw. Da diese Daten in Echtzeit verfügbar sind, ohne auf den Abschluss der Abfrage warten zu müssen, sind diese Ausführungsstatistiken äußerst nützlich für das Debuggen von Leistungsproblemen bei Abfragen, beispielsweise bei Abfragen mit langer Ausführungszeit und Abfragen, die unbegrenzt ausgeführt und nie abgeschlossen werden.

## <a name="the-standard-query-execution-statistics-profiling-infrastructure"></a>Standard-Profilerstellungsinfrastruktur für Abfrageausführungsstatistiken

Die *Profilerstellungsinfrastruktur für Abfrageausführungsstatistiken* (Standardprofilerstellung) muss aktiviert sein, um Informationen über Ausführungspläne zu erfassen, nämlich Zeilenanzahl, CPU- und E/A-Auslastung. Die folgenden Methoden zum Erfassen von Ausführungsplaninformationen für eine **Zielsitzung** nutzen die Standard-Profilerstellungsinfrastruktur:

- [SET STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md) 
- [SET STATISTICS PROFILE](../../t-sql/statements/set-statistics-profile-transact-sql.md)
- [Live-Abfragestatistik](../../relational-databases/performance/live-query-statistics.md)

> [!NOTE]
> Wenn Sie auf die Schaltfläche *Live-Abfragestatistik einschließen* in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] klicken, wird die Standard-Profilerstellungsinfrastruktur verwendet.    
> Ist die [Lightweight-Infrastruktur zur Profilerstellung](#lwp) in höheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aktiviert, wird sie von Live-Abfragestatistiken anstelle der Standardprofilerstellung verwendet, wenn diese über den [Aktivitätsmonitor](../../relational-databases/performance-monitor/activity-monitor.md) oder durch direktes Abfragen der [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)-DMV angezeigt werden. 

Die folgenden Methoden zum globalen Erfassen von Ausführungsplaninformationen für **alle Sitzungen** nutzen die Standard-Profilerstellungsinfrastruktur:

-  Das erweiterte Ereignis ***query_post_execution_showplan***. Informationen zum Aktivieren von erweiterten Ereignissen finden Sie unter [Überwachen der Systemaktivität mit erweiterten Ereignissen](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md).  
- Das Ablaufverfolgungsereignis **Showplan XML** in der [SQL-Ablaufverfolgung](../../relational-databases/sql-trace/sql-trace.md) und in [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md). Weitere Informationen zu diesem Ablaufverfolgungsereignis finden Sie unter [Showplan XML-Ereignisklasse](../../relational-databases/event-classes/showplan-xml-event-class.md).

Wenn Sie eine erweiterte Ereignissitzung ausführen, die das Ereignis *query_post_execution_showplan* verwendet, dann wird die [sys.dm_exec_query_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)-DMV ebenfalls mit Daten aufgefüllt. Dies ermöglicht Live-Abfragestatistiken für alle Sitzungen, indem der [Aktivitätsmonitor](../../relational-databases/performance-monitor/activity-monitor.md) verwendet oder die DMV direkt abgefragt wird. Weitere Informationen finden Sie unter [Live Query Statistics](../../relational-databases/performance/live-query-statistics.md).

## <a name="the-lightweight-query-execution-statistics-profiling-infrastructure"></a><a name="lwp"></a> Die einfache Profilerstellungsinfrastruktur für die Abfrageausführungsstatistik

Beginnend mit [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 und [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] wurde eine neue *einfache Profilerstellungsinfrastruktur für die Abfrageausführungsstatistik* (oder **einfache Profilerstellung**) eingeführt. 

> [!NOTE]
> Nativ kompilierte gespeicherte Prozeduren werden bei der einfachen Profilerstellung nicht unterstützt.  

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v1"></a>Einfache Profilerstellungsinfrastruktur für die Abfrageausführungsstatistik v1

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 bis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) 
  
Beginnend mit [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 und [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] wurde der Leistungsmehraufwand für die Erfassung von Informationen zu Ausführungsplänen durch die Einführung von einfacher Profilerstellung verringert. Im Gegensatz zur Standardprofilerstellung erfasst die einfache Profilerstellung keine CPU-Laufzeitinformationen. Allerdings erfasst die einfache Profilerstellung weiterhin die Zeilenanzahl und Informationen zur E/A-Verwendung.

Ein neues erweitertes Ereignis ***query_thread_profile*** wurde ebenfalls eingeführt, das einfache Profilerstellung nutzt. Dieses erweiterte Ereignis stellt Statistiken zur Abfrageausführung pro Operator bereit und ermöglicht einen besseren Einblick in die Leistung der einzelnen Knoten und Threads. Eine Beispielsitzung mit diesem erweiterten Ereignis kann wie im folgenden Beispiel konfiguriert werden:

```sql
CREATE EVENT SESSION [NodePerfStats] ON SERVER
ADD EVENT sqlserver.query_thread_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

> [!NOTE]
> Weitere Informationen zum Leistungsmehraufwand bei der Abfrageprofilerstellung finden Sie im Blogbeitrag [Developers Choice: Query progress - anytime, anywhere (Von Entwicklern inspiriert: Abfragestatus – jederzeit und überall)](/archive/blogs/sql_server_team/query-progress-anytime-anywhere) 

Wenn Sie eine erweiterte Ereignissitzung ausführen, die das Ereignis *query_thread_profile* verwendet, dann wird die [sys.dm_exec_query_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)-DMV ebenfalls unter Verwendung von einfacher Profilerstellung mit Daten aufgefüllt. Dies ermöglicht Live-Abfragestatistiken für alle Sitzungen, indem der [Aktivitätsmonitor](../../relational-databases/performance-monitor/activity-monitor.md) verwendet oder die DMV direkt abgefragt wird.

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v2"></a>Einfache Profilerstellungsinfrastruktur für die Abfrageausführungsstatistik v2

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 bis [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) 

[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 enthält eine überarbeitete Version der einfachen Profilerstellung mit minimalem Mehraufwand. Einfache Profilerstellung kann auch global über das [Ablaufverfolgungsflag 7412](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) für die Versionen aktiviert werden, die oben unter *Gilt für* angegeben werden. Eine neue DMF [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md) wurde eingeführt, um den Abfrageausführungsplan für In-Flight-Anforderungen zurückzugeben.

Beginnend mit [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU3 und [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU11 gilt Folgendes: Wenn einfache Profilerstellung nicht global aktiviert ist, kann das Argument **QUERY_PLAN_PROFILE** des neuen [USE HINT-Abfragehinweises](../../t-sql/queries/hints-transact-sql-query.md#use_hint) verwendet werden, um einfache Profilerstellung auf Abfrageebene für jede beliebige Sitzung zu aktivieren. Wenn eine Abfrage abgeschlossen wird, die diesen neuen Hinweis enthält, wird auch ein neues erweitertes Ereignis **_query_plan_profile_* _ ausgegeben, das XML für einen tatsächlichen Ausführungsplan ähnlich dem erweiterten Ereignis _query_post_execution_showplan* bereitstellt. 

> [!NOTE]
> Das erweiterte Ereignis *query_plan_profile* nutzt zudem die einfache Profilerstellung, auch wenn der Abfragehinweis nicht verwendet wird. 

Eine einfache Sitzung mit dem erweiterten Ereignis *query_plan_profile* kann wie im unten stehenden Beispiel konfiguriert werden:

```sql
CREATE EVENT SESSION [PerfStats_LWP_Plan] ON SERVER
ADD EVENT sqlserver.query_plan_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

### <a name="lightweight-query-execution-statistics-profiling-infrastructure-v3"></a>Einfache Profilerstellungsinfrastruktur für die Abfrageausführungsstatistik v3

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] enthalten eine neu überarbeitete Version der einfachen Profilerstellung, die Informationen zur Anzahl der Zeilen für alle Ausführungen erfasst. Einfache Profilerstellung ist in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] standardmäßig aktiviert. Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] hat das Ablaufverfolgungsflag 7412 keine Auswirkungen. Die Lightweight-Profilerstellung kann mithilfe der [datenbankweit gültigen Konfiguration](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) LIGHTWEIGHT_QUERY_PROFILING auf Datenbankebene deaktiviert werden: `ALTER DATABASE SCOPED CONFIGURATION SET LIGHTWEIGHT_QUERY_PROFILING = OFF;`.

Die neue dynamische Verwaltungsfunktion [sys.dm_exec_query_plan_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md) wird eingeführt, um das Äquivalent des letzten bekannten, tatsächlichen Ausführungsplans für die meisten Abfragen zurückzugeben. Diese heißt *Abfrageplanstatistik*. Die letzte Abfrageplanstatistik kann auf Datenbankebene mithilfe der [datenbankweit gültigen Konfiguration](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) LAST_QUERY_PLAN_STATS deaktiviert werden: `ALTER DATABASE SCOPED CONFIGURATION SET LAST_QUERY_PLAN_STATS = ON;`.

Im Gegensatz zum Ereignis *query_post_execution_showplan*, das die Standardprofilerstellung nutzt, erfasst das neue erweiterte Ereignis *query_post_execution_plan_profile* das Äquivalent eines tatsächlichen Ausführungsplans mithilfe einfacher Profilerstellung. In [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] ist dieses Ereignis ab CU14 ebenfalls enthalten. Sie können eine Beispielsitzung wie im folgenden Beispiel mithilfe des erweiterten Ereignisses *query_post_execution_plan_profile* konfigurieren:

```sql
CREATE EVENT SESSION [PerfStats_LWP_All_Plans] ON SERVER
ADD EVENT sqlserver.query_post_execution_plan_profile(
  ACTION(sqlos.scheduler_id,sqlserver.database_id,sqlserver.is_system,
    sqlserver.plan_handle,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,
    sqlserver.server_instance_name,sqlserver.session_id,sqlserver.session_nt_username,
    sqlserver.sql_text))
ADD TARGET package0.ring_buffer(SET max_memory=(25600))
WITH (MAX_MEMORY=4096 KB,
  EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
  MAX_DISPATCH_LATENCY=30 SECONDS,
  MAX_EVENT_SIZE=0 KB,
  MEMORY_PARTITION_MODE=NONE,
  TRACK_CAUSALITY=OFF,
  STARTUP_STATE=OFF);
```

#### <a name="example-1---extended-event-session-using-standard-profiling"></a>Beispiel 1: Erweiterte Ereignissitzung mit der Standardprofilerstellung

```sql
CREATE EVENT SESSION [QueryPlanOld] ON SERVER 
ADD EVENT sqlserver.query_post_execution_showplan(
    ACTION(sqlos.task_time, sqlserver.database_id, 
    sqlserver.database_name, sqlserver.query_hash_signed, 
    sqlserver.query_plan_hash_signed, sqlserver.sql_text))
ADD TARGET package0.event_file(SET filename = N'C:\Temp\QueryPlanStd.xel')
WITH (MAX_MEMORY=4096 KB, EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS, 
    MAX_DISPATCH_LATENCY=30 SECONDS, MAX_EVENT_SIZE=0 KB, 
    MEMORY_PARTITION_MODE=NONE, TRACK_CAUSALITY=OFF, STARTUP_STATE=OFF);
```

#### <a name="example-2---extended-event-session-using-lightweight-profiling"></a>Beispiel 2: Erweiterte Ereignissitzung mit der einfachen Profilerstellung

```sql
CREATE EVENT SESSION [QueryPlanLWP] ON SERVER 
ADD EVENT sqlserver.query_post_execution_plan_profile(
    ACTION(sqlos.task_time, sqlserver.database_id, 
    sqlserver.database_name, sqlserver.query_hash_signed, 
    sqlserver.query_plan_hash_signed, sqlserver.sql_text))
ADD TARGET package0.event_file(SET filename=N'C:\Temp\QueryPlanLWP.xel')
WITH (MAX_MEMORY=4096 KB, EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS, 
    MAX_DISPATCH_LATENCY=30 SECONDS, MAX_EVENT_SIZE=0 KB, 
    MEMORY_PARTITION_MODE=NONE, TRACK_CAUSALITY=OFF, STARTUP_STATE=OFF);
```

## <a name="query-profiling-infrastruture-usage-guidance"></a>Leitfaden für die Verwendung der Infrastruktur zur Abfrageprofilerstellung
In der folgenden Tabelle werden die Aktionen zum Aktivieren der Standard- oder Lightweight-Profilerstellung zusammengefasst, sowohl global (auf Serverebene) als auch in einer einzelnen Sitzung. Schließt auch die niedrigste Version ein, für die die Aktion verfügbar ist. 

|`Scope`|Standardprofilerstellung|Lightweight-Profilerstellung|
|---------------|---------------|---------------|
|Global|xEvent-Sitzung mit `query_post_execution_showplan` XE, ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|Ablaufverfolgungsflag 7412, ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1|
|Global|SQL-Ablaufverfolgung und SQL Server Profiler mit Ablaufverfolgungsereignis `Showplan XML`, ab SQL Server 2000|xEvent-Sitzung mit `query_thread_profile` XE, ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2|
|Global|-|xEvent-Sitzung mit `query_post_execution_plan_profile` XE, ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU14 und [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]|
|Sitzung|Verwendung von `SET STATISTICS XML ON`, ab SQL Server 2000|Verwendung des `QUERY_PLAN_PROFILE`-Abfragehinweises zusammen mit einer xEvent-Sitzung mit `query_plan_profile` XE, ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU3 und [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU11|
|Sitzung|Verwendung von `SET STATISTICS PROFILE ON`, ab SQL Server 2000|-|
|Sitzung|Klick auf die Schaltfläche [Live-Abfragestatistik](../../relational-databases/performance/live-query-statistics.md) in SSMS, ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2|-|

## <a name="remarks"></a>Bemerkungen

> [!IMPORTANT]
> Aufgrund einer möglichen zufälligen AV bei der Ausführung einer gespeicherten Überwachungsprozedur, die auf [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md) verweist, müssen Sie sicherstellen, dass [KB 4078596](https://support.microsoft.com/help/4078596) in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] installiert ist.

Beginnend mit der einfachen Profilerstellung v2 und ihrem geringen Mehraufwand kann jeder Server, der nicht bereits CPU-gebunden ist, einfache Profilerstellung **kontinuierlich** ausführen und es Datenbankexperten ermöglichen, jederzeit auf jede aktuell ausgeführte Ausführung zuzugreifen (z.B. mit dem Aktivitätsmonitor oder durch direktes Abfragen von `sys.dm_exec_query_profiles`) und den Abfrageplan mit Laufzeitstatistiken abzurufen.

Weitere Informationen zum Leistungsmehraufwand bei der Abfrageprofilerstellung finden Sie im Blogbeitrag [Developers Choice: Query progress - anytime, anywhere (Von Entwicklern inspiriert: Abfragestatus – jederzeit und überall)](https://techcommunity.microsoft.com/t5/SQL-Server/Developers-Choice-Query-progress-anytime-anywhere/ba-p/385004) 

> [!NOTE]
> Erweiterte Ereignisse, die die einfache Profilerstellung nutzen, verwenden die Informationen der Standardprofilerstellung, wenn die Standardprofilerstellungsinfrastruktur bereits aktiviert ist. Dies tritt beispielsweise auf, wenn eine Sitzung mit dem erweiterten Ereignis `query_post_execution_showplan` ausgeführt und eine weitere Sitzung mit `query_post_execution_plan_profile` gestartet wird. Die zweite Sitzung verwendet weiterhin die Informationen der Standardprofilerstellung.

> [!NOTE]
> In [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] ist die Lightweight-Profilerstellung standardmäßig deaktiviert, sie wird jedoch aktiviert, wenn eine XEvent-Ablaufverfolgung mit `query_post_execution_plan_profile` gestartet wird. Sie wird wieder deaktiviert, wenn die Ablaufverfolgung beendet wird. Daher wird dringend empfohlen, die Lightweight-Profilerstellung mit dem Ablaufverfolgungsflag 7412 global zu aktivieren, wenn auf `query_post_execution_plan_profile` basierende XEvent-Ablaufverfolgungen häufig für eine [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]-Instanz gestartet und beendet werden, um den Mehraufwand durch die wiederholte Aktivierung/Deaktivierung zu vermeiden. 

## <a name="see-also"></a>Weitere Informationen  
 [Überwachen und Optimieren der Leistung](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Tools für die Leistungsüberwachung und -optimierung](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Öffnen des Aktivitätsmonitors &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [Aktivitätsmonitor](../../relational-databases/performance-monitor/activity-monitor.md)     
 [Überwachen der Leistung mit dem Abfragespeicher](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
 [Überwachen der Systemaktivität mit erweiterten Ereignissen](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)      
 [sys.dm_exec_query_statistics_xml](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-statistics-xml-transact-sql.md)     
 [sys.dm_exec_query_profiles](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-profiles-transact-sql.md)     
 [Ablaufverfolgungsflags](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)    
 [Referenz zu logischen und physischen Showplanoperatoren](../../relational-databases/showplan-logical-and-physical-operators-reference.md)    
 [Tatsächlicher Ausführungsplan](../../relational-databases/performance/display-an-actual-execution-plan.md)    
 [Live-Abfragestatistik](../../relational-databases/performance/live-query-statistics.md)