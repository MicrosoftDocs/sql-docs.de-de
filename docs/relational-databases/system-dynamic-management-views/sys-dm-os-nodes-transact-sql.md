---
description: sys.dm_os_nodes (Transact-SQL)
title: sys.dm_os_nodes (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/13/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_nodes
- dm_os_nodes_TSQL
- dm_os_nodes
- sys.dm_os_nodes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_nodes dynamic management view
ms.assetid: c768b67c-82a4-47f5-850b-0ea282358d50
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 38707e178005049f4afe5cb7aaac806e486fc196
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100344353"
---
# <a name="sysdm_os_nodes-transact-sql"></a>sys.dm_os_nodes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Eine interne Komponente mit der Bezeichnung SQLOS erstellt Knotenstrukturen, die die Lage des Hardwareprozessors imitieren. Diese Strukturen können mithilfe von [Soft-NUMA](../../database-engine/configure-windows/soft-numa-sql-server.md) geändert werden, um benutzerdefinierte Knoten Layouts zu erstellen.  

> [!NOTE]
> Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] wird von automatisch Soft-NUMA für bestimmte Hardware Konfigurationen verwendet. Weitere Informationen finden Sie unter [Automatische Soft-NUMA](../../database-engine/configure-windows/soft-numa-sql-server.md#automatic-soft-numa).
  
Die folgende Tabelle enthält Informationen zu diesen Knoten.  
  
> [!NOTE]
> Um diese DMV von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_nodes**.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|node_id|**smallint**|ID des Knotens.|  
|node_state_desc|**nvarchar(256)**|Beschreibung des Knotenzustands. Die Werte werden zuerst mit den sich gegenseitig ausschließenden Werten angezeigt, gefolgt von den kombinierbaren Werten. Beispiel:<br /> Online, Thread Resources Low, Lazy Preemptive<br /><br />Es gibt vier sich gegenseitig ausschließende node_state_desc-Werte. Sie sind unten mit ihren Beschreibungen aufgeführt.<br /><ul><li>Online: Knoten ist online.<li>Offline: Knoten ist offline.<li>Im Leerlauf: der Knoten verfügt über keine ausstehenden Arbeitsanforderungen und befindet sich in einem Leerlaufzustand.<li>IDLE_READY: der Knoten verfügt über keine ausstehenden Arbeitsanforderungen und kann in den Leerlauf versetzt werden.</li></ul><br />Es gibt drei kombinierbare node_state_desc Werte, die unten mit ihren Beschreibungen aufgeführt sind.<br /><ul><li>DAC: dieser Knoten ist für die [dedizierte administrative Verbindung](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)reserviert.<li>THREAD_RESOURCES_LOW: auf diesem Knoten können keine neuen Threads erstellt werden, weil nicht genügend Arbeitsspeicher verfügbar ist.<li>Hot Added: gibt an, dass die Knoten als Reaktion auf ein Hot Add CPU-Ereignis hinzugefügt wurden.</li></ul>|  
|memory_object_address|**varbinary(8)**|Adresse des Speicherobjekts ist diesem Knoten zugeordnet. Eine eins-zu-eins-Beziehung zu [sys.dm_os_memory_objects](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).memory_object_address.|  
|memory_clerk_address|**varbinary(8)**|Adresse des Speicherclerks ist diesem Knoten zugeordnet. Eine eins-zu-eins-Beziehung zu [sys.dm_os_memory_clerks](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).memory_clerk_address.|  
|io_completion_worker_address|**varbinary(8)**|Adresse des Arbeitsthreads ist dem E/A-Abschluss für diesen Knoten zugewiesen. Eine eins-zu-eins-Beziehung zu [sys.dm_os_workers](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).worker_address.|  
|memory_node_id|**smallint**|ID des Arbeitsspeicherknotens, zu dem dieser Knoten gehört. N:1-Beziehung zu [sys.dm_os_memory_nodes](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-nodes-transact-sql.md).memory_node_id.|  
|cpu_affinity_mask|**bigint**|Bitmap, das die CPUs identifiziert, die diesem Knoten zugeordnet sind.|  
|online_scheduler_count|**smallint**|Anzahl der von diesem Knoten verwalteten Online Planer.|  
|idle_scheduler_count|**smallint**|Anzahl der Onlinescheduler, die über keinen aktiven Arbeitsthread verfügen.|  
|active_worker_count|**int**|Anzahl der Arbeitsthreads, die auf allen von diesem Knoten verwalteten Zeitplanungsmodulen aktiv sind.|  
|avg_load_balance|**int**|Durchschnittliche Anzahl von Tasks pro Zeitplanungsmodul auf diesem Knoten.|  
|timer_task_affinity_mask|**bigint**|Bitmap, das die Zeitplanungsmodule identifiziert, denen Zeitgebertasks zugewiesen sein können.|  
|permanent_task_affinity_mask|**bigint**|Bitmap, das die Zeitplanungsmodule identifiziert, denen permanente Zeitgebertasks zugewiesen sein können.|  
|resource_monitor_state|**bit**|Jeder Knoten verfügt über einen zugewiesenen Ressourcenmonitor. Der Ressourcenmonitor kann ausgeführt werden oder sich im Leerlauf befinden. Der Wert 1 gibt an, dass der Monitor ausgeführt wird, der Wert 0 gibt an, dass er sich im Leerlauf befindet.|  
|online_scheduler_mask|**bigint**|Identifiziert die Prozessaffinitätsmaske für diesen Knoten.|  
|processor_group|**smallint**|Identifiziert die Gruppe von Prozessoren für diesen Knoten.|  
|cpu_count |**int** |Anzahl der verfügbaren CPUs für diesen Knoten. |
|pdw_node_id|**int**|Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.<br /><br /> **Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]|  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   

## <a name="see-also"></a>Weitere Informationen    
 [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md)  
  
