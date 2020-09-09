---
description: sys.dm_os_memory_cache_counters (Transact-SQL)
title: sys. dm_os_memory_cache_counters (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_memory_cache_counters_TSQL
- dm_os_memory_cache_counters_TSQL
- sys.dm_os_memory_cache_counters
- dm_os_memory_cache_counters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_counters dynamic management view
ms.assetid: ca7bd036-d661-4c17-b00a-e1a975bd8932
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c2455d20419ebb8f23b2146ca25637ac689a3c9b
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89536972"
---
# <a name="sysdm_os_memory_cache_counters-transact-sql"></a>sys.dm_os_memory_cache_counters (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Momentaufnahme des Zustands eines Caches in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zurück. **sys. dm_os_memory_cache_counters** stellt Laufzeitinformationen zu den zugeordneten Cache Einträgen, deren Verwendung und die Speicher Quelle für die Cache Einträge bereit.  
  
> **Hinweis:** Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys. dm_pdw_nodes_os_memory_cache_counters**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8)**|Gibt die Adresse (Primärschlüssel) der Leistungsindikatoren an, die einem bestimmten Cache zugeordnet sind. Lässt keine NULL-Werte zu.|  
|**name**|**nvarchar(256)**|Gibt den Namen des Caches an. Lässt keine NULL-Werte zu.|  
|**type**|**nvarchar(60)**|Gibt den Typ des Caches an, der diesem Eintrag zugeordnet ist. Lässt keine NULL-Werte zu.|  
|**single_pages_kb**|**bigint**|**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Der Umfang (in KB) des zugeordneten Einzelseiten-Arbeitsspeichers. Hierbei handelt es sich um den Arbeitsspeicher, der mithilfe der Einzelseitenzuordnung zugeordnet wird. Dies bezieht sich auf die 8-KB-Seiten, die direkt aus dem Pufferpool für diesen Cache verwendet werden. Lässt keine NULL-Werte zu.|  
|**pages_kb**|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Gibt die dem Cache zugeordnete Arbeitsspeichermenge in Kilobyte an. Lässt keine NULL-Werte zu.|  
|**multi_pages_kb**|**bigint**|**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Der Umfang (in KB) des zugeordneten mehrseitigen Arbeitsspeichers. Hierbei handelt es sich um den Arbeitsspeicher, der mithilfe der Zuordnung für mehrere Seiten des Arbeitsspeicherknotens zugeordnet wird. Dieser Arbeitsspeicher wird außerhalb des Pufferpools zugeordnet und nutzt die virtuelle Zuordnung der Arbeitsspeicherknoten. Lässt keine NULL-Werte zu.|  
|**pages_in_use_kb**|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Gibt die dem Cache zugeordnete und vom Cache verwendete Arbeitsspeichermenge in Kilobyte an. Lässt NULL-Werte zu.  Werte für Objekte vom Typ `USERSTORE_<*>` werden nicht nachverfolgt.  NULL wird gemeldet.|  
|**single_pages_in_use_kb**|**bigint**|**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Der Umfang (in KB) des verwendeten Einzelseiten-Arbeitsspeichers. Lässt NULL-Werte zu. Diese Informationen werden für Objekte vom Typ USERSTORE_ nicht nachverfolgt, \<*> und diese Werte sind NULL.|  
|**multi_pages_in_use_kb**|**bigint**|**Gilt für**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)].<br /><br /> Der Umfang (in Kilobytes) des verwendeten mehrseitigen Arbeitsspeichers. Lässt NULL-Werte zu. Diese Informationen werden für Objekte vom Typ USERSTORE_ nicht nachverfolgt \<*> , und diese Werte sind NULL.|  
|**entries_count**|**bigint**|Gibt die Anzahl der Einträge im Cache an. Lässt keine NULL-Werte zu.|  
|**entries_in_use_count**|**bigint**|Gibt die Anzahl der Einträge im Cache an, der verwendet wird. Lässt keine NULL-Werte zu.|  
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen 

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] Premium-Tarifen ist die- `VIEW DATABASE STATE` Berechtigung in der Datenbank erforderlich. In [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] den Tarifen "Standard" und "Basic" ist der  **Server Administrator** oder ein **Azure Active Directory Administrator** Konto erforderlich.   

## <a name="see-also"></a>Weitere Informationen  
  [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


