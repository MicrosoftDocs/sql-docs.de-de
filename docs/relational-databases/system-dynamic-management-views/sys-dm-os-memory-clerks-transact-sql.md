---
description: sys.dm_os_memory_clerks (Transact-SQL)
title: sys.dm_os_memory_clerks (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_clerks
- sys.dm_os_memory_clerks
- dm_os_memory_clerks_TSQL
- sys.dm_os_memory_clerks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_clerks dynamic management view
ms.assetid: 1d556c67-5c12-46d5-aa8c-7ec1bb858df7
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 87d26cf7e64e509f811b80b954984217430ef831
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100338865"
---
# <a name="sysdm_os_memory_clerks-transact-sql"></a>sys.dm_os_memory_clerks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt die Gruppe aller derzeit in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz aktiven Arbeitsspeicherclerks zurück.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_memory_clerks**.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**memory_clerk_address**|**varbinary(8)**|Gibt die eindeutige Speicheradresse des Arbeitsspeicherclerks an. Dies ist die Primärschlüsselspalte. Lässt keine NULL-Werte zu.|  
|**type**|**nvarchar(60)**|Gibt den Typ des Arbeitsspeicherclerks an. Jeder Clerk entspricht einem bestimmten Typ, wie z. B. dem CLR-Clerk MEMORYCLERK_SQLCLR. Lässt keine NULL-Werte zu.|  
|**name**|**nvarchar(256)**|Gibt den intern zugewiesenen Namen des Arbeitsspeicherclerks an. Eine Komponente kann über mehrere Arbeitsspeicherclerks eines bestimmten Typs verfügen. Eine Komponente hat die Möglichkeit, bestimmte Namen zum Identifizieren von Arbeitsspeicherclerks desselben Typs zu verwenden. Lässt keine NULL-Werte zu.|  
|**memory_node_id**|**smallint**|Gibt die ID des Speicherknotens an. Lässt keine NULL-Werte zu.|  
|**single_pages_kb**|**bigint**|**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].|  
|**pages_kb**|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Gibt an, wie viel Arbeitsspeicher für Seiten diesem Arbeitsspeicherclerk (in Kilobyte) zugewiesen wurde. Lässt keine NULL-Werte zu.|  
|**multi_pages_kb**|**bigint**|**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Umfang des zugeordneten Arbeitsspeichers für mehrere Seiten in KB. Dies entspricht dem Umfang des Arbeitsspeichers, der mithilfe der Mehrfachseitenzuordnung der Speicherknoten zugeordnet wurde. Dieser Arbeitsspeicher wird außerhalb des Pufferpools zugeordnet und nutzt die virtuelle Zuordnung der Arbeitsspeicherknoten. Lässt keine NULL-Werte zu.|  
|**virtual_memory_reserved_kb**|**bigint**|Gibt den Umfang des von einem Arbeitsspeicherclerk reservierten virtuellen Arbeitsspeichers an. Lässt keine NULL-Werte zu.|  
|**virtual_memory_committed_kb**|**bigint**|Gibt den Umfang des virtuellen Arbeitsspeichers an, für den ein Arbeitsspeicherclerk ein Commit ausgeführt hat. Der Umfang des Arbeitsspeichers, für den ein Commit ausgeführt wurde, sollte immer geringer als der Umfang des reservierten Arbeitsspeichers sein. Lässt keine NULL-Werte zu.|  
|**awe_allocated_kb**|**bigint**|Gibt den Arbeitsspeicher in Kilobyte (KB) an, der im physischen Speicher gesperrt und nicht vom Betriebssystem ausgelagert ist. Lässt keine NULL-Werte zu.|  
|**shared_memory_reserved_kb**|**bigint**|Gibt den Umfang des von einem Arbeitsspeicherclerk reservierten gemeinsamen Arbeitsspeichers an. Der Umfang des Arbeitsspeichers, der als freigegebener Speicherbereich und für die Dateizuordnung reserviert ist. Lässt keine NULL-Werte zu.|  
|**shared_memory_committed_kb**|**bigint**|Gibt den Umfang des freigegebenen Speicherbereichs an, für den vom Arbeitsspeicherclerk ein Commit ausgeführt wurde. Lässt keine NULL-Werte zu.|  
|**page_size_in_bytes**|**bigint**|Gibt die Granularität der Seitenzuordnung für diesen Arbeitsspeicherclerk an. Lässt keine NULL-Werte zu.|  
|**page_allocator_address**|**varbinary(8)**|Gibt die Adresse der Seitenzuordnung an. Diese Adresse ist für einen Arbeitsspeicher Clerk eindeutig und kann in **sys.dm_os_memory_objects** verwendet werden, um Speicher Objekte zu suchen, die an diesen Clerk gebunden sind. Lässt keine NULL-Werte zu.|  
|**host_address**|**varbinary(8)**|Gibt die Arbeitsspeicheradresse des Hosts für diesen Arbeitsspeicherclerk an. Weitere Informationen finden Sie unter [sys.dm_os_hosts &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-hosts-transact-sql.md). Komponenten, z. b. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, greifen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] über die Host Schnittstelle auf Speicherressourcen zu.<br /><br /> 0x00000000 = Arbeitsspeicherclerk gehört zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Lässt keine NULL-Werte zu.|  
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen 

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
  
## <a name="remarks"></a>Bemerkungen  
 Der Speicher-Manager von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] besteht aus einer Hierarchie mit drei Ebenen. Die unterste Ebene der Hierarchie bilden Speicherknoten. Die mittlere Ebene besteht aus Arbeitsspeicherclerks, Arbeitsspeichercaches und Arbeitsspeicherpools. Die obere Ebene besteht aus Speicherobjekten. Diese Objekte werden im Allgemeinen für die Zuordnung von Arbeitsspeicher in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz verwendet.  
  
 Speicherknoten stellen die Schnittstelle und die Implementierung für Zuordnungen auf unterer Ebene bereit. Innerhalb von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] haben nur Arbeitsspeicherclerks Zugriff auf Speicherknoten. Arbeitsspeicherclerks greifen für die Belegung von Arbeitsspeicher auf Speicherknotenschnittstellen zu. Zudem können Speicherknoten den mithilfe des Clerks zugeordneten Arbeitsspeicher zu Diagnosezwecken nachverfolgen. Jede Komponente, die eine beträchtliche Speichermenge zuordnet, muss einen eigenen Arbeitsspeicherclerk erstellen und ihren gesamten Arbeitsspeicher mithilfe der Clerkschnittstellen zuordnen. Komponenten erstellen häufig ihre entsprechenden Clerks, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gestartet wird.  
  
## <a name="see-also"></a>Weitere Informationen  

 [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_query_plan &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)  
  
  


