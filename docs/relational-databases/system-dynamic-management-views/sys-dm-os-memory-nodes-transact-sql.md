---
description: sys.dm_os_memory_nodes (Transact-SQL)
title: sys.dm_os_memory_nodes (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_nodes_TSQL
- sys.dm_os_memory_nodes
- sys.dm_os_memory_nodes_TSQL
- dm_os_memory_nodes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_nodes dynamic management view
ms.assetid: bf4032fe-7db1-40e9-a62e-d69cebff4b44
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4fde4a08d0ed1a0fdc5f3a8be6b70b43faa0b557
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100339464"
---
# <a name="sysdm_os_memory_nodes-transact-sql"></a>sys.dm_os_memory_nodes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Interne Zuordnungen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwenden den Speicher-Manager von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Die Nachverfolgung des Unterschieds zwischen Prozess Speicher-Leistungsindikatoren von **sys.dm_os_process_memory** und internen Indikatoren kann auf die Arbeitsspeicher Nutzung von externen Komponenten im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Speicherbereich hindeuten.  
  
 Die Knoten werden einzeln für physische NUMA-Arbeitsspeicherknoten erstellt. Diese können sich von den CPU-Knoten in **sys.dm_os_nodes** unterscheiden.  
  
 Zuordnungen, die direkt durch Windows-Routinen für die Speicherbelegung vorgenommen wurden, werden nicht nachverfolgt. Die folgende Tabelle enthält Informationen über Arbeitsspeicherbelegungen, die ausschließlich über die Speicher-Manager-Schnittstellen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] durchgeführt werden.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_memory_nodes**.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**memory_node_id**|**smallint**|Gibt die ID des Speicherknotens an. Im Zusammenhang mit **memory_node_id** **sys.dm_os_memory_clerks**. Lässt keine NULL-Werte zu.|  
|**virtual_address_space_reserved_kb**|**bigint**|Gibt die Anzahl der virtuellen Adressreservierungen in Kilobytes (KB) an, für die weder ein Commit noch eine Zuordnung zu physischen Seiten besteht. Lässt keine NULL-Werte zu.|  
|**virtual_address_space_committed_kb**|**bigint**|Gibt die Menge virtueller Adressen in KB an, für die ein Commit oder eine Zuordnung zu physischen Seiten besteht. Lässt keine NULL-Werte zu.|  
|**locked_page_allocations_kb**|**bigint**|Gibt die Menge an physischem Speicher in KB an, der von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gesperrt wurde. Lässt keine NULL-Werte zu.|  
|**single_pages_kb**|**bigint**|**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Speichermenge in KB, für die ein Commit ausgeführt wurde und die mithilfe der Einzelseitenzuordnung durch Threads, die auf diesem Knoten ausgeführt werden, zugeordnet wird. Dieser Speicher wird aus dem Pufferpool zugeordnet. Dieser Wert gibt den Knoten an, auf dem die Zuordnungen angefordert wurden, und nicht den physischen Speicherort, an dem die Zuordnungsanforderung erfüllt wurde.|  
|**pages_kb**|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Gibt die Menge des zugesicherten Arbeitsspeichers in KB an, der diesem NUMA-Knoten von der Seitenzuordnung im Speicher-Manager zugeordnet wird. Lässt keine NULL-Werte zu.|  
|**multi_pages_kb**|**bigint**|**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Speichermenge in KB, für die ein Commit ausgeführt wurde und die mithilfe der Mehrfachseitenzuordnung durch Threads, die auf diesem Knoten ausgeführt werden, zugeordnet wird. Dieser Speicher wird von außerhalb des Pufferpools zugeordnet. Dieser Wert gibt den Knoten an, in dem die Zuordnungs Anforderungen aufgetreten sind, nicht den physischen Speicherort, an dem die Zuordnungs Anforderung erfüllt wurde|  
|**shared_memory_reserved_kb**|**bigint**|Gibt die Menge an gemeinsam genutzten Speicher in KB an, die auf diesem Knoten reserviert wurde. Lässt keine NULL-Werte zu.|  
|**shared_memory_committed_kb**|**bigint**|Gibt die Menge an gemeinsam genutzten Speicher in KB an, für die auf diesem Knoten ein Commit ausgeführt wurde. Lässt keine NULL-Werte zu.|  
|**cpu_affinity_mask**|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Nur zur internen Verwendung. Lässt keine NULL-Werte zu.|  
|**online_scheduler_mask**|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Nur zur internen Verwendung. Lässt keine NULL-Werte zu.|  
|**processor_group**|**smallint**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Nur zur internen Verwendung. Lässt keine NULL-Werte zu.|  
|**foreign_committed_kb**|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Gibt die Menge an zugesichertem Arbeitsspeicher von anderen Arbeitsspeicherknoten in KB an. Lässt keine NULL-Werte zu.|  
|**target_kb** |**bigint** |**Gilt für:** [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Gibt das Speicher Ziel für den Speicher Knoten in KB an. |   
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   

## <a name="see-also"></a>Weitere Informationen  
  [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


