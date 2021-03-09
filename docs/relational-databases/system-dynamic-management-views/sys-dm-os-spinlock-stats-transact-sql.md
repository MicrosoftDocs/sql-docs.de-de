---
description: sys.dm_os_spinlock_stats (Transact-SQL)
title: sys.dm_os_spinlock_stats (Transact-SQL)
ms.custom: ''
ms.date: 02/10/2021
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: ''
ms.suite: sql
ms.technology: database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
f1_keywords:
- sys.dm_os_spinlock_stats_TSQL
- dm_os_spinlock_stats_TSQL
- dm_os_spinlock_stats
- sys.dm_os_spinlock_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_spinlock_stats dynamic management view
author: bluefooted
ms.author: pamela
ms.reviewer: wiassaf
manager: amitban
ms.openlocfilehash: 2e48f40003412a2db5340a32e5408f649bf5bb18
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464985"
---
# <a name="sysdm_os_spinlock_stats-transact-sql"></a>sys.dm_os_spinlock_stats (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Gibt Informationen zu allen Spinlock-warte Vorgängen nach Typ organisiert zurück.  
  

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|name|**nvarchar(256)**|Der Name des Spinlock-Typs.|  
|Stöße|**bigint**|Gibt an, wie oft ein Thread versucht, die Spinlock abzurufen, und wird blockiert, da ein anderer Thread derzeit die Spinlock-Sperre besitzt.|  
|HS|**bigint**|Gibt an, wie oft ein Thread eine Schleife ausführt, während versucht wird, das Spinlock abzurufen.|  
|spins_per_collision|**real**|Das Verhältnis von Drehungen zu Kollisionen|  
|sleep_time|**bigint**|Die Zeitspanne in Millisekunden, die Threads im Fall eines Backoff im Standbymodus verbracht haben.|  
|Backoffs auch|**int**|Gibt an, wie oft ein Thread, der "spinnt" ist, den Spinlock nicht abrufen kann und den Scheduler ergibt.|  


## <a name="permissions"></a>Berechtigungen  
In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.    
  
## <a name="remarks"></a>Bemerkungen  
 
 sys.dm_os_spinlock_stats kann verwendet werden, um die Quelle von Spinlock-Konflikten zu identifizieren. In einigen Situationen können Sie möglicherweise Spinlock-Konflikte auflösen oder verringern. Es kann jedoch Situationen geben, in denen Sie sich mit [!INCLUDE[msCoName](../../includes/msconame-md.md)] Support Services in Verbindung setzen müssen.  
  
 Sie können den Inhalt sys.dm_os_spinlock_stats mithilfe von `DBCC SQLPERF` wie folgt zurücksetzen:  
  
```  
DBCC SQLPERF ('sys.dm_os_spinlock_stats', CLEAR);  
GO  
```  
  
 Dadurch werden alle Leistungsindikatoren auf 0 zurückgesetzt.  
  
> [!NOTE]  
>  Diese Statistiken werden nicht persistent gespeichert, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] neu gestartet wird. Alle Daten stellen einen Gesamtwert seit dem letzten Zurücksetzen der Statistiken oder dem Starten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dar.  
  
## <a name="spinlocks"></a>Spinlocks  
 Ein Spinlock ist ein schlankes Synchronisierungs Objekt, das verwendet wird, um den Zugriff auf Datenstrukturen zu serialisieren, die in der Regel für kurze Zeit aufbewahrt werden. Wenn ein Thread versucht, auf eine Ressource zuzugreifen, die durch einen Spinlock geschützt ist, der von einem anderen Thread aufbewahrt wird, führt der Thread eine Schleife aus, und versucht erneut, auf die Ressource zuzugreifen, anstatt sofort den Scheduler wie bei einem Latch oder einer anderen Ressourcen Wartezeit bereitzustellen. Der Thread wird fortgesetzt, bis die Ressource verfügbar ist, oder die Schleife wird beendet. zu diesem Zeitpunkt führt der Thread den Scheduler aus und wechselt zurück in die ausführbare Warteschlange. Durch diese Vorgehensweise wird die übermäßige Thread Kontext Umschaltung verringert. Wenn jedoch ein Konflikt zwischen Spinlock vorliegt, wird möglicherweise eine beträchtliche CPU-Auslastung festgestellt.

> [!NOTE]  
>  Wenn Sie ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf Intel skylake-Prozessoren installiert haben, lesen Sie [diesen Artikel](https://support.microsoft.com/topic/kb4538688-fix-severe-spinlock-contention-occurs-in-sql-server-2019-43faea65-fdcb-6835-f7fe-93abdb235837) , um das erforderliche Update anzuwenden und das Ablaufverfolgungsflag 8101 zu aktivieren.

 Die folgende Tabelle enthält kurze Beschreibungen einiger der gängigsten Spinlock-Typen.  
  
|SpinLock-Typ|BESCHREIBUNG|  
|-----------------|-----------------|  
|ABR|Nur zur internen Verwendung.|
|ADB_CACHE|Nur zur internen Verwendung.|
|ALLOC_CACHES_HASH|Nur zur internen Verwendung.|
|APPENDONLY_STORAGE|Nur zur internen Verwendung.|
|APRC_BACK_OFF_STATS|Nur zur internen Verwendung.|
|APRC_EVENT_LIST|Nur zur internen Verwendung.|
|APRC_QUEUE_LIST|Nur zur internen Verwendung.|
|APRC_VALIDATION_QUEUE_LIST|Nur zur internen Verwendung.|
|ASYNC_OP_ADMIN_CLIENT_REGISTRATION_LIST|Nur zur internen Verwendung.|
|ASYNC_OP_ADMIN_WORK_REGISTRATION_HASH_TABLE|Nur zur internen Verwendung.|
|Asyncstatuslist|Nur zur internen Verwendung.|
|BACKUP|Nur zur internen Verwendung.|
|BACKUP_COPY_CONTEXT|Nur zur internen Verwendung.|
|BACKUP_CTX|Nur zur internen Verwendung.|
|BASE_XACT_HASH|Nur zur internen Verwendung.|
|BLOCKER_ENUM|Nur zur internen Verwendung.|
|Bprepartition|Nur zur internen Verwendung.|
|Bpworkfile|Nur zur internen Verwendung.|
|BUF_HASH|Nur zur internen Verwendung.|
|BUF_LINK|Nur zur internen Verwendung.|
|BUF_WRITE_LOG|Nur zur internen Verwendung.|
|CACHEOBJ_DBG|Nur zur internen Verwendung.|
|Channelforceclosemanager|Nur zur internen Verwendung.|
|CHECK_AGGREGATE_STATE|Nur zur internen Verwendung.|
|CLR_HOSTTASK|Nur zur internen Verwendung.|
|CLR_SPIN_LOCK|Nur zur internen Verwendung.|
|CMED_DATABASE|Nur zur internen Verwendung.|
|CMED_HASH_SET|Nur zur internen Verwendung.<br><br>**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ( [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] über [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] CU1)<br><br> **Hinweis:** dieser Spinlock-Name ändert sich in LOCK_RW_CMED_HASH_SET, nachdem Sie [SQL Server 2016 Cu2](https://support.microsoft.com/topic/kb3195888-fix-high-cpu-usage-causes-performance-issues-in-sql-server-2016-and-2017-9514b80d-938f-e179-3131-74e6c757c4d5)angewendet haben.|
|Columndatasetsessionlist|Nur zur internen Verwendung.|
|COLUMNSTORE_HASHTABLE|Nur zur internen Verwendung.|
|COLUMNSTOREBUILDSTATE_LIST|Nur zur internen Verwendung.|
|COM_INIT|Nur zur internen Verwendung.|
|Commit ausgeführt|Nur zur internen Verwendung.|
|COMPPLAN_SKELETON|Nur zur internen Verwendung.|
|CONNECTION_MANAGER|Nur zur internen Verwendung.|
|Stan|Nur zur internen Verwendung.|
|Csibuildmem|Nur zur internen Verwendung.|
|CURSOR|Nur zur internen Verwendung.|
|Cursor|Nur zur internen Verwendung.|
|Dataportconsumer|Nur zur internen Verwendung.|
|Dataportsourceinfocredit|Nur zur internen Verwendung.|
|Dataportsourceinfoqueue|Nur zur internen Verwendung.|
|DATASET_FREELIST|Nur zur internen Verwendung.|
|DBCC_CHECK|Nur zur internen Verwendung.|
|DBSEEDING_OPERATION|Nur zur internen Verwendung.|
|DBT_HASH|Nur zur internen Verwendung.|
|DBT_IO_LIST|Nur zur internen Verwendung.|
|DBTABLE|Steuert den Zugriff auf eine Datenstruktur im Arbeitsspeicher für jede Datenbank in einer [!INCLUDE[ssde_md](../../includes/ssde_md.md)] , die die Eigenschaften dieser Datenbank enthält. Weitere Informationen finden Sie in [diesem Artikel](https://techcommunity.microsoft.com/t5/SQL-Server/Improving-Concurrency-Scalability-of-SQL-Server-workload-by/ba-p/384789). |
|DEFERRED_WF_EXT_DROP|Nur zur internen Verwendung.|
|DEK_INSTANCE|Nur zur internen Verwendung.|
|DELAYED_PARTITIONED_STACK|Nur zur internen Verwendung.|
|Deletebitmap|Nur zur internen Verwendung.|
|DIAG_MANAGER|Nur zur internen Verwendung.|
|DIAG_OBJECT|Nur zur internen Verwendung.|
|DIGEST_CACHE|Nur zur internen Verwendung.|
|Dinpbuf|Nur zur internen Verwendung.|
|Directlogconsumer|Nur zur internen Verwendung.|
|DP_LIST|Steuert den Zugriff auf die Liste der geänderten Seiten für eine Datenbank, für die ein indirekter Prüfpunkt aktiviert ist. Anwenden von Korrekturen aus [Artikel 4497928](https://support.microsoft.com/kb/4497928), [Artikel 4040276](https://support.microsoft.com/kb/4040276)oder Verwenden des Ablaufverfolgungsflags [3468](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). Weitere Informationen finden Sie in [diesem Artikel](https://techcommunity.microsoft.com/t5/SQL-Server/Indirect-Checkpoint-and-tempdb-8211-the-good-the-bad-and-the-non/ba-p/385510).|
|DROP|Nur zur internen Verwendung.|
|DROP_TEMPO|Nur zur internen Verwendung.|
|DROPPED_ALLOC_UNIT|Nur zur internen Verwendung.|
|DTC_HASHTABLE|Nur zur internen Verwendung.|
|DTT_LIST|Nur zur internen Verwendung.|
|ENDD_LIST|Nur zur internen Verwendung.|
|EXT_CACHE|Nur zur internen Verwendung.|
|EXTENT_ACTIVATION|Nur zur internen Verwendung.|
|FABRIC_DB_MGR_PTR|Nur zur internen Verwendung.|
|FABRIC_LOG_MANAGEMENT_INPUT_VALUE|Nur zur internen Verwendung.|
|FABRIC_REPLICA_TRANSPORT|Nur zur internen Verwendung.|
|FABRIC_TVF_DATA_CONSUMER_LIST|Nur zur internen Verwendung.|
|FABRIC_TVF_LOAD_LIB|Nur zur internen Verwendung.|
|FCB_REPLICA_SYNC|Nur zur internen Verwendung.|
|FGCB_PRP_FILL|Nur zur internen Verwendung.|
|FILE_HANDLE_CACHE|Nur zur internen Verwendung.|
|FILE_TABLE|Nur zur internen Verwendung.|
|FILESTREAM_CHUNKER|Nur zur internen Verwendung.|
|FREE_SPACE_CACHE_ENTRY|Nur zur internen Verwendung.|
|FS_CONTAINER_LIST_WITH_DELETE|Nur zur internen Verwendung.|
|FS_DELETED_FOLDER_CLEANUP|Nur zur internen Verwendung.|
|FSAGENT|Nur zur internen Verwendung.|
|FSGHOST_STATUS|Nur zur internen Verwendung.|
|FT_INIT|Nur zur internen Verwendung.|
|GHOST_FREE|Nur zur internen Verwendung.|
|GHOST_HASH|Nur zur internen Verwendung.|
|GLOBAL_SCHEDULER_LIST|Nur zur internen Verwendung.|
|GLOBAL_TRACE_FLAGS|Nur zur internen Verwendung.|
|Globaltrans|Nur zur internen Verwendung.|
|GROUP_COMMIT_FEEDBACK_LOOP|Nur zur internen Verwendung.|
|GUARDIAN|Nur zur internen Verwendung.|
|HADR_AGH_X_ACCESS|Nur zur internen Verwendung.|
|HADR_AR_CONTROLLER_COLLECTION|Nur zur internen Verwendung.|
|HADR_AR_DB_MGR|Nur zur internen Verwendung.|
|HADR_AR_TRANSPORT|Nur zur internen Verwendung.|
|HADR_COMPRESSION_MGR_POOL|Nur zur internen Verwendung.|
|HADR_FABRIC_FACTORY|Nur zur internen Verwendung.|
|HADR_PRIORITY_QUEUE|Nur zur internen Verwendung.|
|HADR_TRANSPORT_CONTROL|Nur zur internen Verwendung.|
|HADR_TRANSPORT_LIST|Nur zur internen Verwendung.|
|Hadrseedinglist|Nur zur internen Verwendung.|
|HOBT_DROPPED|Nur zur internen Verwendung.|
|HOBT_HASH|Nur zur internen Verwendung.|
|HTTP|Nur zur internen Verwendung.|
|HTTP_CONNCACHE|Nur zur internen Verwendung.|
|HTTP_ENDPOINT|Nur zur internen Verwendung.|
|IDENTITY|Nur zur internen Verwendung.|
|INDEX_CREATE|Nur zur internen Verwendung.|
|IO_DISPENSER_PAUSE|Nur zur internen Verwendung.|
|IO_RG_VOLUME_HASHTABLE|Nur zur internen Verwendung.|
|Ioreq|Nur zur internen Verwendung.|
|Issresource|Nur zur internen Verwendung.|
|KTM_ENLISTMENT|Nur zur internen Verwendung.|
|LANG_RES_LOAD|Nur zur internen Verwendung.|
|LIVE_TARGET_TVF|Nur zur internen Verwendung.|
|LOCK_FREE_LIST|Nur zur internen Verwendung.|
|LOCK_HASH|Schützt den Zugriff auf die Hash Tabelle des Sperren-Managers, in der Informationen zu den Sperren gespeichert werden, die in einer Datenbank gespeichert sind. Weitere Informationen finden Sie in [diesem Artikel](https://support.microsoft.com/kb/2926217) und im [Handbuch zu Transaktions Sperren und Zeilen Versions](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md#Lock_Engine)Verwaltung.|
|LOCK_NOTIFICATION|Nur zur internen Verwendung.|
|LOCK_RESOURCE_ID|Nur zur internen Verwendung.|
|LOCK_RW_ABTX_HASH_SET|Nur zur internen Verwendung.|
|LOCK_RW_AGDB_HEALTH_DIAG|Nur zur internen Verwendung.|
|LOCK_RW_CMED_HASH_SET|Nur zur internen Verwendung.<br><br>**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (Beginnend mit [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] Cu2), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]|
|LOCK_RW_DPT_TABLE|Nur zur internen Verwendung.|
|LOCK_RW_IN_ROW_TRACKER|Nur zur internen Verwendung.|
|LOCK_RW_LOGIN_RATE_STATS|Nur zur internen Verwendung.|
|LOCK_RW_PVS_PAGE_TRACKER|Nur zur internen Verwendung.|
|LOCK_RW_RBIO_REQ|Nur zur internen Verwendung.|
|LOCK_RW_SECURITY_CACHE|Schützt die Cache Einträge im Zusammenhang mit Sicherheits Token und Zugriffs Überprüfungen. <br><br>**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (Beginnend mit [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] Cu2), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]<br><br> Wenn sich die Einträge im Cache Speicher "TokenAndPermUserStore" kontinuierlich vergrößern, bemerken Sie möglicherweise große Drehungen für diese Spinlock. Wertet die [](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) Verwendung der Ablaufverfolgungsflags 4610 und 4618 aus Weitere Informationen finden Sie unter [Zugriffs Überprüfungs Cache-Server Konfigurationsoptionen](../../database-engine/configure-windows/access-check-cache-server-configuration-options.md), in diesem [Artikel](https://support.microsoft.com/topic/queries-take-a-longer-time-to-finish-running-when-the-size-of-the-tokenandpermuserstore-cache-grows-in-sql-server-2005-ad1622e7-3bb5-7902-19a0-5d0e6271033d)und in diesem [Blog](https://techcommunity.microsoft.com/t5/sql-server-support/query-performance-issues-associated-with-a-large-sized-security/ba-p/315494).|
|LOCK_RW_TEST|Nur zur internen Verwendung.|
|LOCK_RW_WPR_BUCKET|Nur zur internen Verwendung.|
|LOCK_SORT_STREAM|Nur zur internen Verwendung.|
|LOCK_SQLSATELLITE_MESSAGE|Nur zur internen Verwendung.|
|LOG_CONSOLIDATION|Nur zur internen Verwendung.|
|LOG_RG_GOVERNOR|Nur zur internen Verwendung.|
|LOGCACHE_ACCESS|Nur zur internen Verwendung.|
|Logflushq|Nur zur internen Verwendung.|
|Logioabq|Nur zur internen Verwendung.|
|Logioseqmappdingmessagequeue|Nur zur internen Verwendung.|
|Loglc|Nur zur internen Verwendung.|
|Loglfm|Nur zur internen Verwendung.|
|LOGON_TRIGGER_CACHE|Nur zur internen Verwendung.|
|LOGPOOL_HASHBUCKET|Nur zur internen Verwendung.|
|LOGPOOL_REFCOUNTEDOBJECT|Nur zur internen Verwendung.|
|LOGPOOL_SHAREDCACHEBUFFER|Nur zur internen Verwendung.|
|LOGPOOL_SIZEPERRESOURCEPOOL|Nur zur internen Verwendung.|
|LPE_BATCH|Nur zur internen Verwendung.|
|LPE_SESSION|Nur zur internen Verwendung.|
|LPE_SXTP|Nur zur internen Verwendung.|
|Lsid|Nur zur internen Verwendung.|
|Lslist|Nur zur internen Verwendung.|
|Lsnreflist|Nur zur internen Verwendung.|
|LSS_SYNC_DTC|Nur zur internen Verwendung.|
|MD_CHANGE_NOTIFICATION|Nur zur internen Verwendung.|
|MDB_REMOTE_BATCH_STATS_HASH_TABLE|Nur zur internen Verwendung.|
|MDB_REMOTE_SESSION_HASH_TABLE|Nur zur internen Verwendung.|
|MEM_MGR|Nur zur internen Verwendung.|
|MGR_CACHE|Nur zur internen Verwendung.|
|MIGRATION_BUF_LIST|Nur zur internen Verwendung.|
|Mutex|Schützt die Cache Einträge im Zusammenhang mit Sicherheits Token und Zugriffs Überprüfungen. <br><br>**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (Bis [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)] )<br><br>Wenn sich die Einträge im Cache Speicher "TokenAndPermUserStore" kontinuierlich vergrößern, bemerken Sie möglicherweise große Drehungen für diese Spinlock. Wertet die [](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) Verwendung der Ablaufverfolgungsflags 4610 und 4618 aus Weitere Informationen finden Sie unter [Zugriffs Überprüfungs Cache-Server Konfigurationsoptionen](../../database-engine/configure-windows/access-check-cache-server-configuration-options.md), in diesem [Artikel](https://support.microsoft.com/topic/queries-take-a-longer-time-to-finish-running-when-the-size-of-the-tokenandpermuserstore-cache-grows-in-sql-server-2005-ad1622e7-3bb5-7902-19a0-5d0e6271033d)und in diesem [Blog](https://techcommunity.microsoft.com/t5/sql-server-support/query-performance-issues-associated-with-a-large-sized-security/ba-p/315494).|
|NETCONN_ADDRESS|Nur zur internen Verwendung.|
|ONDEMAND_TASK|Nur zur internen Verwendung.|
|ONE_PROC_SIM_NODE_CONTEXT|Nur zur internen Verwendung.|
|ONE_PROC_SIM_NODE_CONTEXT_LIST|Nur zur internen Verwendung.|
|ONE_PROC_SIM_REPLICA_CONTEXT|Nur zur internen Verwendung.|
|ONE_PROC_SIM_SERVICE_PARTITION|Nur zur internen Verwendung.|
|OPT_IDX_MISS_ID|Nur zur internen Verwendung.|
|OPT_IDX_MISS_KEY|Nur zur internen Verwendung.|
|OPT_IDX_STATS|Nur zur internen Verwendung.|
|OPT_INFO_MGR|Nur zur internen Verwendung.|
|PAGE_WORKITEMLIST|Nur zur internen Verwendung.|
|Pagecopier|Nur zur internen Verwendung.|
|Parallelredocache|Nur zur internen Verwendung.|
|PARTITIONED_HEAP_FREE_LIST|Nur zur internen Verwendung.|
|PROGRESS_REPORT|Nur zur internen Verwendung.|
|QE_SHUTDOWN|Nur zur internen Verwendung.|
|QSCAN_CACHE|Nur zur internen Verwendung.|
|QUERY_EXEC_STATS|Nur zur internen Verwendung.|
|QUERY_STORE_ASYNC_PERSIST|Nur zur internen Verwendung.|
|QUERY_STORE_ASYNC_QUEUE_TLIST|Nur zur internen Verwendung.|
|QUERY_STORE_CAPTURE_POLICY_INTERVAL|Nur zur internen Verwendung.|
|QUERY_STORE_CAPTURE_POLICY_STATS|Nur zur internen Verwendung.|
|QUERY_STORE_CAPTURE_POLICY_THRESHOLD|Nur zur internen Verwendung.|
|QUERY_STORE_CURRENT_INTERVAL|Nur zur internen Verwendung.|
|QUERY_STORE_HT_CACHE|Nur zur internen Verwendung.|
|QUERY_STORE_LIST|Nur zur internen Verwendung.|
|QUERY_STORE_PLAN_COMP_AGG|Nur zur internen Verwendung.|
|QUERY_STORE_PLAN_LIST|Nur zur internen Verwendung.|
|QUERY_STORE_READ_ONLY_FLAGS|Nur zur internen Verwendung.|
|QUERY_STORE_SELF_AGG|Nur zur internen Verwendung.|
|QUERY_STORE_STMT_COMP_AGG|Nur zur internen Verwendung.|
|QueryExec|Nur zur internen Verwendung.|
|Queryscan|Nur zur internen Verwendung.|
|RANGE_GENERATION|Nur zur internen Verwendung.|
|READ_AHEAD|Nur zur internen Verwendung.|
|Redomgrstate|Nur zur internen Verwendung.|
|REMOTE_SESSION_CACHE|Nur zur internen Verwendung.|
|Remoteblockio|Nur zur internen Verwendung.|
|Remoteop|Nur zur internen Verwendung.|
|REPL_LOGREADER_HISTORY_CACHE|Nur zur internen Verwendung.|
|REPL_LOGREADER_PERDB_HISTORY_CACHE|Nur zur internen Verwendung.|
|Resmanager|Nur zur internen Verwendung.|
|RESSOURCE|Nur zur internen Verwendung.|
|ResQueue|Nur zur internen Verwendung.|
|RFS_THREAD_QUEUE|Nur zur internen Verwendung.|
|RG_TIMER|Nur zur internen Verwendung.|
|ROWGROUP_VERSIONS|Nur zur internen Verwendung.|
|Rpcchannelpool|Nur zur internen Verwendung.|
|Rpcpackage|Nur zur internen Verwendung.|
|Rpkrequestorcontext|Nur zur internen Verwendung.|
|RWLOCK_LAST|Nur zur internen Verwendung.|
|SATELLITE_CONNECTION|Nur zur internen Verwendung.|
|SBS_CLIENT_ENDPOINTS|Nur zur internen Verwendung.|
|SBS_CLIENT_REQUESTS|Nur zur internen Verwendung.|
|SBS_DISPATCH|Nur zur internen Verwendung.|
|SBS_PENDING|Nur zur internen Verwendung.|
|SBS_SERVER_XACT_TASK_PROXY|Nur zur internen Verwendung.|
|SBS_TRANSPORT|Nur zur internen Verwendung.|
|SBS_UCS_DISPATCH|Nur zur internen Verwendung.|
|SICHERHEIT|Nur zur internen Verwendung.|
|SECURITY_CACHE|Schützt die Cache Einträge im Zusammenhang mit Sicherheits Token und Zugriffs Überprüfungen. <br><br>**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ( [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] über [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] CU1)<br><br> Wenn sich die Einträge im Cache Speicher "TokenAndPermUserStore" kontinuierlich vergrößern, bemerken Sie möglicherweise große Drehungen für diese Spinlock. Wertet die [](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) Verwendung der Ablaufverfolgungsflags 4610 und 4618 aus Weitere Informationen finden Sie unter [Zugriffs Überprüfungs Cache-Server Konfigurationsoptionen](../../database-engine/configure-windows/access-check-cache-server-configuration-options.md), in diesem [Artikel](https://support.microsoft.com/topic/queries-take-a-longer-time-to-finish-running-when-the-size-of-the-tokenandpermuserstore-cache-grows-in-sql-server-2005-ad1622e7-3bb5-7902-19a0-5d0e6271033d)und in diesem [Blog](https://techcommunity.microsoft.com/t5/sql-server-support/query-performance-issues-associated-with-a-large-sized-security/ba-p/315494).<br><br>**Hinweis:** dieser Spinlock-Name ändert sich in LOCK_RW_SECURITY_CACHE, nachdem Sie [SQL Server 2016 Cu2](https://support.microsoft.com/topic/kb3195888-fix-high-cpu-usage-causes-performance-issues-in-sql-server-2016-and-2017-9514b80d-938f-e179-3131-74e6c757c4d5)angewendet haben.|
|SECURITY_FEDAUTH_AAD_BECWSCONNS|Nur zur internen Verwendung.|
|SEMANTIC_TICACHE|Nur zur internen Verwendung.|
|SEQUENCED_OBJECT|Nur zur internen Verwendung.|
|SEQUEUE_SIZED_THREADSAFE|Nur zur internen Verwendung.|
|SESSION_KILLER|Nur zur internen Verwendung.|
|SESSION_MANAGER|Nur zur internen Verwendung.|
|SESSION_SEC_CONTEXT|Nur zur internen Verwendung.|
|SETRANGE_SYNC|Nur zur internen Verwendung.|
|SHARABLE_SESSION_OBJECTS|Nur zur internen Verwendung.|
|SLO_INFO_LIST|Nur zur internen Verwendung.|
|SNI|Nur zur internen Verwendung.|
|SNI_NODE_PENDING_IO_QUEUE|Nur zur internen Verwendung.|
|Soapsessions|Nur zur internen Verwendung.|
|SOS_ABORT_TASK|Nur zur internen Verwendung.|
|SOS_ACTIVEDESCRIPTOR|Nur zur internen Verwendung.|
|SOS_BLOCKALLOCPARTIALLIST|Nur zur internen Verwendung.|
|SOS_BLOCKDESCRIPTORBUCKET|Nur zur internen Verwendung.|
|SOS_CACHESTORE|Synchronisiert den Zugriff auf verschiedene in-Memory-Caches in der [!INCLUDE[ssde_md](../../includes/ssde_md.md)] , z. b. im Plancache oder temporären Tabellencache. Ein starker Konflikt bei diesem Spinlock-Typ kann viele verschiedene Dinge bedeuten, abhängig vom spezifischen Cache, der Konflikte verursacht. Wenden Sie sich an Microsoft [!INCLUDE[msCoName](../../includes/msconame-md.md)] Support Services, um Hilfe bei der Behebung dieses Spinlock-Typs |
|SOS_CACHESTORE_CLOCK|Nur zur internen Verwendung.|
|SOS_CLOCKALG_INTERNODE_SYNC|Nur zur internen Verwendung.|
|SOS_DEBUG_HOOK|Nur zur internen Verwendung.|
|SOS_DESCDATABUFFERLIST|Nur zur internen Verwendung.|
|SOS_LARGEPAGE_ALLOCATOR|Nur zur internen Verwendung.|
|SOS_MINITHREAD|Nur zur internen Verwendung.|
|SOS_NODE|Nur zur internen Verwendung.|
|SOS_OBJECT_POOL|Nur zur internen Verwendung.|
|SOS_OBJECT_STORE|Nur zur internen Verwendung.|
|SOS_OOM_CHECK|Nur zur internen Verwendung.|
|SOS_PHYS_PAGE_CACHE|Nur zur internen Verwendung.|
|SOS_RESOURCE_CLERK_LIST|Nur zur internen Verwendung.|
|SOS_RINGBUFFER_RECORD|Nur zur internen Verwendung.|
|SOS_RW|Nur zur internen Verwendung.|
|SOS_SATELLITE_USER_POOL|Nur zur internen Verwendung.|
|SOS_SCHEDULER|Nur zur internen Verwendung.|
|SOS_SELIST_SIZED_SLOCK|Nur zur internen Verwendung.|
|SOS_SUSPEND_QUEUE|Nur zur internen Verwendung.|
|SOS_SYSTHREAD|Nur zur internen Verwendung.|
|SOS_SYSTHREAD_DISPATCHER|Nur zur internen Verwendung.|
|SOS_TASK|Nur zur internen Verwendung.|
|SOS_TLIST|Nur zur internen Verwendung.|
|SOS_VM_LOW|Nur zur internen Verwendung.|
|SOS_WAIT_STATS|Nur zur internen Verwendung.|
|SOS_WAITABLE_ADDRESS_HASHBUCKET|Nur zur internen Verwendung.|
|SPIN_EVENT_MUTEX|Nur zur internen Verwendung.|
|SPL_DISPATCHER_LIST|Nur zur internen Verwendung.|
|SPL_DISPATCHER_QUEUE|Nur zur internen Verwendung.|
|SPL_NONYIELD_ANALYSIS|Nur zur internen Verwendung.|
|SPL_QUERY_STORE_CTX_INITIALIZED|Nur zur internen Verwendung.|
|SPL_QUERY_STORE_EXEC_STATS_AGG|Nur zur internen Verwendung.|
|SPL_QUERY_STORE_EXEC_STATS_READ|Nur zur internen Verwendung.|
|SPL_QUERY_STORE_STATS_COOKIE_CACHE|Nur zur internen Verwendung.|
|SPL_SOS_DISPATCHER|Nur zur internen Verwendung.|
|SPL_TDS_PKT_QUEUE|Nur zur internen Verwendung.|
|SPL_XE_BUFFER_MGR|Nur zur internen Verwendung.|
|SPL_XE_DISPATCHER_QUEUE|Nur zur internen Verwendung.|
|SPL_XE_NOTIFICATION_CALLBACK_LIST|Nur zur internen Verwendung.|
|SPL_XE_SESSION_EVENT_MGR|Nur zur internen Verwendung.|
|SPL_XE_SESSION_MGR|Nur zur internen Verwendung.|
|SPL_XE_SESSION_TARGET_MGR|Nur zur internen Verwendung.|
|SPT_PROFILE|Nur zur internen Verwendung.|
|SQL_MGR|Nur zur internen Verwendung.|
|SQL_NORM|Nur zur internen Verwendung.|
|SQLTRACE_FILE_BUFFER|Nur zur internen Verwendung.|
|SRVPROC|Nur zur internen Verwendung.|
|STACK_HASHER|Nur zur internen Verwendung.|
|Sublatch|Nur zur internen Verwendung.|
|Subpabsc|Nur zur internen Verwendung.|
|SUBPDESC_LIST|Nur zur internen Verwendung.|
|SVC_BROKER_CTRL|Nur zur internen Verwendung.|
|SVC_BROKER_DEBUG_LIST|Nur zur internen Verwendung.|
|SVC_BROKER_LIST|Nur zur internen Verwendung.|
|SVC_BROKER_OBJECT|Nur zur internen Verwendung.|
|SYNCPOINT_RESOURCE|Nur zur internen Verwendung.|
|Taskelapandexecutionmonitor|Nur zur internen Verwendung.|
|TDS_TVP|Nur zur internen Verwendung.|
|Testteam|Nur zur internen Verwendung.|
|Testteamexponentielle|Nur zur internen Verwendung.|
|Testteamexponentialtastas|Nur zur internen Verwendung.|
|Testteamtastas|Nur zur internen Verwendung.|
|TMP_SESS_KEY|Nur zur internen Verwendung.|
|TSQL_DEBUG|Nur zur internen Verwendung.|
|TXFRM_REPL|Nur zur internen Verwendung.|
|VDI_OPERATION|Nur zur internen Verwendung.|
|WINFAB_REPORT_FAULT|Nur zur internen Verwendung.|
|WRITE_PAGE_RECORDER|Nur zur internen Verwendung.|
|X_PACKET_LIST|Nur zur internen Verwendung.|
|X_PIPE|Nur zur internen Verwendung.|
|X_PIPE_DEMAND|Nur zur internen Verwendung.|
|X_PORT|Nur zur internen Verwendung.|
|XACT_LOCK_INFO|Nur zur internen Verwendung.|
|XACT_LOCKINFO_TASK|Nur zur internen Verwendung.|
|XACT_WORKSPACE|Nur zur internen Verwendung.|
|Xcb|Nur zur internen Verwendung.|
|XCB_FREE_LIST|Nur zur internen Verwendung.|
|XCB_HASH|Nur zur internen Verwendung.|
|XCHNG_TRACE|Nur zur internen Verwendung.|
|XDES|Nur zur internen Verwendung.|
|XDES_HASH|Nur zur internen Verwendung.|
|XDE smgr|Nur zur internen Verwendung.|
|Xdestablelist|Nur zur internen Verwendung.|
|XE_RATE_LIMITER_STRETCHDB|Nur zur internen Verwendung.|
|XE_SESSION_STORAGE|Nur zur internen Verwendung.|
|XID_ARRAY|Nur zur internen Verwendung.|
|XIO_BLOCKLIST|Nur zur internen Verwendung.|
|XIO_REQSTR|Nur zur internen Verwendung.|
|XIO_SEQNUMBUMP|Nur zur internen Verwendung.|
|Xiostats|Nur zur internen Verwendung.|
|XTP_RT_DATA_LIST|Nur zur internen Verwendung.|
|XTS_MGR|Nur zur internen Verwendung.|
|XVB_CSN|Nur zur internen Verwendung.|
|XVB_LIST|Nur zur internen Verwendung.|
 
## <a name="see-also"></a>Weitere Informationen  
 
 [DBCC SQLPERF &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md)   
 
 [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  

 [Wann ist Spinlock ein bedeutender Treiber der CPU-Auslastung in SQL Server?](https://techcommunity.microsoft.com/t5/SQL-Server-Support/When-is-Spinlock-a-Significant-Driver-of-CPU-utilization-in-SQL/ba-p/530142)

 [Diagnostizieren und Beheben von Spinlock-Konflikten auf SQL Server](../diagnose-resolve-spinlock-contention.md)
  
