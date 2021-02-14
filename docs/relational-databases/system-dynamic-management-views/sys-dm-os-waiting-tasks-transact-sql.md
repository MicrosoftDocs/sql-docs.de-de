---
description: sys.dm_os_waiting_tasks (Transact-SQL)
title: sys.dm_os_waiting_tasks (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_waiting_tasks
- sys.dm_os_waiting_tasks_TSQL
- dm_os_waiting_tasks_TSQL
- sys.dm_os_waiting_tasks
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_waiting_tasks dynamic management view
ms.assetid: ca5e6844-368c-42e2-b187-6e5f5afc8df3
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c44fe0aed3c77809a1134bf34e5897a15cae4067
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100347968"
---
# <a name="sysdm_os_waiting_tasks-transact-sql"></a>sys.dm_os_waiting_tasks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt Informationen zur Warteschlange von Tasks zurück, die auf eine Ressource warten. Weitere Informationen zu Aufgaben finden Sie im [Handbuch zur Thread-und Task Architektur](../../relational-databases/thread-and-task-architecture-guide.md).
   
> [!NOTE]  
> Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_waiting_tasks**.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**waiting_task_address**|**varbinary(8)**|Adresse des wartenden Tasks.|  
|**session_id**|**smallint**|ID der Sitzung, die dem Task zugeordnet ist.|  
|**exec_context_id**|**int**|ID des Ausführungskontexts, der dem Task zugeordnet ist.|  
|**wait_duration_ms**|**bigint**|Gesamtwartezeit für diesen Wartetyp (in Millisekunden). Diese Zeit ist **signal_wait_time**.|  
|**wait_type**|**nvarchar(60)**|Der Name des Wartetyps.|  
|**resource_address**|**varbinary(8)**|Adresse der Ressource, auf die der Task wartet.|  
|**blocking_task_address**|**varbinary(8)**|Task, der derzeit diese Ressource verwendet.|  
|**blocking_session_id**|**smallint**|ID der Sitzung, die die Anforderung blockiert. Wenn diese Spalte den Wert NULL aufweist, wird die Anforderung nicht blockiert, oder die Sitzungsinformationen der blockierenden Sitzung sind nicht verfügbar (bzw. können nicht identifiziert werden).<br /><br /> -2 = Der Besitzer der blockierenden Ressource ist eine verwaiste verteilte Transaktion.<br /><br /> -3 = Der Besitzer der blockierenden Ressource ist eine verzögerte Wiederherstellungstransaktion.<br /><br /> -4 = Die Sitzungs-ID des Besitzers des blockierenden Latches konnte aufgrund interner Latchstatusübergänge nicht bestimmt werden.|  
|**blocking_exec_context_id**|**int**|ID des Ausführungskontexts des blockierenden Tasks.|  
|**resource_description**|**nvarchar (3072)**|Beschreibung der verwendeten Ressource. Weitere Informationen finden Sie in der unten stehenden Liste.|  
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="resource_description-column"></a>resource_description-Spalte  
 Für die resource_description-Spalte sind folgende Werte möglich.  
  
 **Ressourcenbesitzer des Threadpools:**  
  
-   Thread Pool-ID = Scheduler\<hex-address>  
  
 **Ressourcenbesitzer der parallelen Abfrage:**  
  
-   exchangeevent-ID = {Port | Pipe} \<hex-address> waittype = \<exchange-wait-type> NodeId =\<exchange-node-id>  
  
 **Exchange-wait-type:**  
  
-   e_waitNone  
  
-   e_waitPipeNewRow  
  
-   e_waitPipeGetRow  
  
-   e_waitSynchronizeConsumerOpen  
  
-   e_waitPortOpen  
  
-   e_waitPortClose  
  
-   e_waitRange  
  
 **Besitzer der LOCK-Ressource:**  
  
-   \<type-specific-description> ID = Sperr \<lock-hex-address> Modus = \<mode> AssociatedObjectId =\<associated-obj-id>  
  
     **\<type-specific-description> folgende Aktionen sind möglich:**  
  
    -   Für Datenbank: databaselock subresource = \<databaselock-subresource> DBID =\<db-id>  
  
    -   Für file: FILELOCK fileid = \<file-id> subresource = \<filelock-subresource> DBID =\<db-id>  
  
    -   Für Objekt: objectlock lockpartition = \<lock-partition-id> objID = \<obj-id> subresource = \<objectlock-subresource> DBID =\<db-id>  
  
    -   Für page: PAGELOCK fleid = \<file-id> pageID = \<page-id> DBID = \<db-id> subresource =\<pagelock-subresource>  
  
    -   Für Key: Keylock Hobtid = \<hobt-id> DBID =\<db-id>  
  
    -   Für Block: extentlock-Datei = \<file-id> pageID = \<page-id> DBID =\<db-id>  
  
    -   Für RID: ridlock fleid = \<file-id> pageID = \<page-id> DBID =\<db-id>  
  
    -   Für Anwendung: applicationlock Hash = \<hash> databaseprincipalid = \<role-id> DBID =\<db-id>  
  
    -   Für Metadaten: metadatalock subresource = \<metadata-subresource> ClassID = \<metadatalock-description> DBID =\<db-id>  
  
    -   Für HoBT: hobtlock Hobtid = \<hobt-id> subresource = \<hobt-subresource> DBID =\<db-id>  
  
    -   Für ALLOCATION_UNIT: Zuordnung von "-ID", "-ID" = " \<hobt-id> subresource = \<alloc-unit-subresource> DBID ="\<db-id>  
  
     **\<mode> folgende Aktionen sind möglich:**  
  
     Sch-S, Sch-M, S, U, X, IS, IU, IX, SIU, SIX, UIX, BU, RangeS-S, RangeS-U, RangeI-N, RangeI-S, RangeI-U, RangeI-X, RangeX-, RangeX-U, RangeX-X  
  
 **Besitzer der externen Ressource:**  
  
-   Extern ExternalResource =\<wait-type>  
  
 **Besitzer der allgemeinen Ressource:**  
  
-   Transaktionmutex transaktioninfo Workspace =\<workspace-id>  
  
-   Mutex  
  
-   CLRTaskJoin  
  
-   CLRMonitorEvent  
  
-   CLRRWLockEvent  
  
-   resourceWait  
  
 **Besitzer der Latchressource:**  
  
-   \<db-id>:\<file-id>:\<page-in-file>  
  
-   \<GUID>  
  
-   \<latch-class> (\<latch-address>)  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
 
## <a name="example"></a>Beispiel
### <a name="a-identify-tasks-from-blocked-sessions"></a>A. Identifizieren Sie Tasks aus blockierten Sitzungen. 

```sql
SELECT * FROM sys.dm_os_waiting_tasks 
WHERE blocking_session_id IS NOT NULL; 
```   

### <a name="b-view-waiting-tasks-per-connection"></a>B. Wartende Tasks pro Verbindung anzeigen

```sql
SELECT st.text AS [SQL Text], c.connection_id, w.session_id, 
  w.wait_duration_ms, w.wait_type, w.resource_address, 
  w.blocking_session_id, w.resource_description, c.client_net_address, c.connect_time
FROM sys.dm_os_waiting_tasks AS w
INNER JOIN sys.dm_exec_connections AS c ON w.session_id = c.session_id 
CROSS APPLY (SELECT * FROM sys.dm_exec_sql_text(c.most_recent_sql_handle)) AS st 
              WHERE w.session_id > 50 AND w.wait_duration_ms > 0
ORDER BY c.connection_id, w.session_id
GO
```

### <a name="c-view-waiting-tasks-for-all-user-processes-with-additional-information"></a>C. Anzeigen von wartenden Aufgaben für alle Benutzer Prozesse mit zusätzlichen Informationen

```sql
SELECT 'Waiting_tasks' AS [Information], owt.session_id,
    owt.wait_duration_ms, owt.wait_type, owt.blocking_session_id,
    owt.resource_description, es.program_name, est.text,
    est.dbid, eqp.query_plan, er.database_id, es.cpu_time,
    es.memory_usage*8 AS memory_usage_KB
FROM sys.dm_os_waiting_tasks owt
INNER JOIN sys.dm_exec_sessions es ON owt.session_id = es.session_id
INNER JOIN sys.dm_exec_requests er ON es.session_id = er.session_id
OUTER APPLY sys.dm_exec_sql_text (er.sql_handle) est
OUTER APPLY sys.dm_exec_query_plan (er.plan_handle) eqp
WHERE es.is_user_process = 1
ORDER BY owt.session_id;
GO
```
  
## <a name="see-also"></a>Weitere Informationen  
[SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)      
[Handbuch zur Thread- und Taskarchitektur](../../relational-databases/thread-and-task-architecture-guide.md)     
   
 
