---
description: sys.dm_os_workers (Transact-SQL)
title: sys.dm_os_workers (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_workers_TSQL
- sys.dm_os_workers_TSQL
- dm_os_workers
- sys.dm_os_workers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_workers dynamic management view
ms.assetid: 4d5d1e52-a574-4bdd-87ae-b932527235e8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5fdbf16415df10f596ae9cf636d943fff3c3c4ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347621"
---
# <a name="sysdm_os_workers-transact-sql"></a>sys.dm_os_workers (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile für jeden Arbeitsthread im System zurück. Weitere Informationen zu Workern finden Sie im [Handbuch zur Thread-und Task Architektur](../../relational-databases/thread-and-task-architecture-guide.md). 
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_workers**.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|worker_address|**varbinary(8)**|Speicheradresse des Arbeitsthreads.|  
|status|**int**|Nur zur internen Verwendung.|  
|is_preemptive|**bit**|1 = Arbeitsthread wird mit präemptiver Zeitplanung ausgeführt. Jeder Arbeitsthread mit externem Code wird unter präemptiver Zeitplanung ausgeführt.|  
|is_fiber|**bit**|1 = Arbeitsthread wird mit Lightweightpooling ausgeführt. Weitere Informationen finden Sie weiter unten in diesem Thema unter [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)noch nicht kennen.|  
|is_sick|**bit**|1 = Arbeitsthread versucht fortlaufend, einen Spinlock abzurufen. Wenn dieses Bit festgelegt ist, kann ein Problem im Zusammenhang mit einem Konflikt bei einem Objekt vorliegen, auf das häufig zugegriffen wird.|  
|is_in_cc_exception|**bit**|1 = Arbeitsthread verarbeitet zurzeit eine Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ausnahme.|  
|is_fatal_exception|**bit**|Gibt an, ob dieser Arbeitsthread eine schwerwiegende Ausnahme empfangen hat.|  
|is_inside_catch|**bit**|1 = Arbeitsthread verarbeitet zurzeit eine Ausnahme.|  
|is_in_polling_io_completion_routine|**bit**|1 = Arbeitsthread führt zurzeit eine E/A-Abschlussroutine für einen ausstehenden E/A-Vorgang aus. Weitere Informationen finden Sie unter [sys.dm_io_pending_io_requests &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-pending-io-requests-transact-sql.md).|  
|context_switch_count|**int**|Die Anzahl der von diesem Arbeitsthread ausgeführten Kontextwechsel des Zeitplanungsmoduls.|  
|pending_io_count|**int**|Anzahl der physischen E/A-Vorgänge, die dieser Arbeitsthread ausgeführt hat.|  
|pending_io_byte_count|**bigint**|Gesamtanzahl von Bytes aller ausstehenden physischen E/A-Vorgänge für diesen Arbeitsthread.|  
|pending_io_byte_average|**int**|Durchschnittliche Anzahl von Bytes aller physischen E/A-Vorgänge für diesen Arbeitsthread.|  
|wait_started_ms_ticks|**bigint**|Der Zeitpunkt in [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), zu dem dieser worker in den Zustand "angehalten" wechselt. Wenn Sie diesen Wert von ms_ticks in [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) subtrahieren, wird die Anzahl der Millisekunden zurückgegeben, die der Worker gewartet hat.|  
|wait_resumed_ms_ticks|**bigint**|Der Zeitpunkt in [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), zu dem dieser Worker den ausführbaren Zustand eingegeben hat. Wenn Sie diesen Wert von ms_ticks in [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) subtrahieren, wird die Anzahl der Millisekunden zurückgegeben, in denen sich der Worker in der ausführbaren Warteschlange befand.|  
|task_bound_ms_ticks|**bigint**|Der Zeitpunkt in [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), in dem eine Aufgabe an diesen Worker gebunden ist.|  
|worker_created_ms_ticks|**bigint**|Der Zeitpunkt in [ms_ticks](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md), in dem ein Worker erstellt wird.|  
|exception_num|**int**|Die Fehlernummer der letzten Ausnahme, die bei diesem Arbeitsthread aufgetreten ist.|  
|exception_severity|**int**|Der Schweregrad der letzten Ausnahme, die bei diesem Arbeitsthread aufgetreten ist.|  
|exception_address|**varbinary(8)**|Die Codeadresse, von der die Ausnahme ausgelöst wurde.|  
|affinity|**bigint**|Die Threadaffinität des Arbeitsthreads. Entspricht der Affinität des Threads in [sys.dm_os_threads &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md).|  
|state|**nvarchar(60)**|Der Status des Arbeitsthreads. Folgenden Werte sind möglich:<br /><br /> INIT = Der Arbeitsthread wird zurzeit initialisiert.<br /><br /> RUNNING = Arbeitsthread wird derzeit nicht präemptiv oder präemptiv ausgeführt.<br /><br /> RUNNABLE = Arbeitsthread kann im Zeitplanungsmodul ausgeführt werden.<br /><br /> SUSPENDED = Arbeitsthread wurde angehalten und wartet darauf, dass ein Ereignis ein Signal sendet.|  
|start_quantum|**bigint**|Zeit in Millisekunden zu Beginn der aktuellen Ausführung dieses Arbeitsthreads.|  
|end_quantum|**bigint**|Zeit in Millisekunden am Ende der aktuellen Ausführung dieses Arbeitsthreads.|  
|last_wait_type|**nvarchar(60)**|Typ des letzten Wartevorgangs. Eine Liste der Warte Typen finden Sie unter [sys.dm_os_wait_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|  
|return_code|**int**|Rückgabewert des letzten Wartevorgangs. Folgenden Werte sind möglich:<br /><br /> 0 =SUCCESS<br /><br /> 3 = DEADLOCK<br /><br /> 4 = PREMATURE_WAKEUP<br /><br /> 258 = TIMEOUT|  
|quantum_used|**bigint**|Nur zur internen Verwendung.|  
|max_quantum|**bigint**|Nur zur internen Verwendung.|  
|boost_count|**int**|Nur zur internen Verwendung.|  
|tasks_processed_count|**int**|Anzahl der von diesem Arbeitsthread verarbeiteten Tasks.|  
|fiber_address|**varbinary(8)**|Speicheradresse der Fiber, der dieser Arbeitsthread zugeordnet ist.<br /><br /> NULL = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist nicht konfiguriert für Lightweightpooling.|  
|task_address|**varbinary(8)**|Speicheradresse des aktuellen Tasks. Weitere Informationen finden Sie unter [sys.dm_os_tasks &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md).|  
|memory_object_address|**varbinary(8)**|Speicheradresse des Arbeitsthread-Speicherobjekts. Weitere Informationen finden Sie unter [sys.dm_os_memory_objects &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).|  
|thread_address|**varbinary(8)**|Speicheradresse des Threads, der diesem Arbeitsthread zugeordnet ist. Weitere Informationen finden Sie unter [sys.dm_os_threads &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md).|  
|signal_worker_address|**varbinary(8)**|Speicheradresse des Arbeitsthreads, der dieses Objekt zuletzt signalisiert hat. Weitere Informationen finden Sie unter [sys.dm_os_workers](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|scheduler_address|**varbinary(8)**|Speicheradresse des Zeitplanungsmoduls. Weitere Informationen finden Sie unter [sys.dm_os_schedulers &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).|  
|processor_group|**smallint**|Speichert die Prozessorgruppen-ID, die diesem Thread zugewiesen ist.|  
|pdw_node_id|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn sich der Arbeitsthread im Status RUNNING befindet und der Arbeitsthread nicht präemptiv ausgeführt wird, stimmt die Adresse des Arbeitsthreads mit active_worker_address in sys.dm_os_schedulers überein.  
  
 Wird ein auf ein Ereignis wartender Arbeitsthread signalisiert, wird der Arbeitsthread an der vordersten Stelle in der ausführbaren Warteschlange platziert. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ermöglicht diesen Vorgang tausendmal nacheinander. Danach wird der Arbeitsthread an das Ende der Warteschlange verschoben. Wenn ein Arbeitsthread an das Ende der Warteschlange verschoben wird, wirkt sich dies negativ auf die Leistung aus.  
  
## <a name="permissions"></a>Berechtigungen
In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] Premium-Tarifen ist die- `VIEW DATABASE STATE` Berechtigung in der Datenbank erforderlich. In [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] den Tarifen "Standard" und "Basic" ist die Mitgliedschaft in der  `Server Admin` Rolle oder ein `Azure Active Directory admin` Konto erforderlich.   

## <a name="examples"></a>Beispiele  
 Sie können die folgende Abfrage verwenden, um herauszufinden, wie lange ein Arbeitsthread bereits im Status SUSPENDED oder RUNNABLE ausgeführt wird.  
  
```sql
SELECT   
    t1.session_id,  
    CONVERT(varchar(10), t1.status) AS status,  
    CONVERT(varchar(15), t1.command) AS command,  
    CONVERT(varchar(10), t2.state) AS worker_state,  
    w_suspended =   
      CASE t2.wait_started_ms_ticks  
        WHEN 0 THEN 0  
        ELSE   
          t3.ms_ticks - t2.wait_started_ms_ticks  
      END,  
    w_runnable =   
      CASE t2.wait_resumed_ms_ticks  
        WHEN 0 THEN 0  
        ELSE   
          t3.ms_ticks - t2.wait_resumed_ms_ticks  
      END  
  FROM sys.dm_exec_requests AS t1  
  INNER JOIN sys.dm_os_workers AS t2  
    ON t2.task_address = t1.task_address  
  CROSS JOIN sys.dm_os_sys_info AS t3  
  WHERE t1.scheduler_id IS NOT NULL;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
 session_id status     command         worker_state w_suspended w_runnable  
 ---------- ---------- --------------- ------------ ----------- --------------------  
 4          background LAZY WRITER     SUSPENDED    688         688  
 6          background LOCK MONITOR    SUSPENDED    4657        4657
 19         background BRKR TASK       SUSPENDED    603820344   603820344  
 14         background BRKR EVENT HNDL SUSPENDED    63583641    63583641  
 51         running    SELECT          RUNNING      0           0  
 2          background RESOURCE MONITO RUNNING      0           603825954  
 3          background LAZY WRITER     SUSPENDED    422         422  
 7          background SIGNAL HANDLER  SUSPENDED    603820485   603820485  
 13         background TASK MANAGER    SUSPENDED    603824704   603824704  
 18         background BRKR TASK       SUSPENDED    603820407   603820407  
 9          background TRACE QUEUE TAS SUSPENDED    454         454  
 52         suspended  SELECT          SUSPENDED    35094       35094  
 1          background RESOURCE MONITO RUNNING      0           603825954  
```

 Wenn `w_runnable` und `w_suspended` gleich sind, stellt dies in der Ausgabe die Zeit dar, die sich der Arbeitsthread im Status SUSPENDED befindet. Andernfalls stellt `w_runnable` die Zeit dar, die der Arbeitsthread im Status RUNNABLE verbringt. In der Ausgabe befindet sich die Sitzung `52``SUSPENDED` Millisekunden lang im Status `35,094`.  
  
## <a name="see-also"></a>Weitere Informationen  
[SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)       
[Leitfaden zur Architektur der Abfrageverarbeitung](../../relational-databases/query-processing-architecture-guide.md#DOP)       
[Handbuch zur Thread- und Taskarchitektur](../../relational-databases/thread-and-task-architecture-guide.md)    
