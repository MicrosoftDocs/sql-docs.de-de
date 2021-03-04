---
description: sys.dm_os_memory_cache_clock_hands (Transact-SQL)
title: sys.dm_os_memory_cache_clock_hands (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 12/21/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_memory_cache_clock_hands_TSQL
- dm_os_memory_cache_clock_hands
- dm_os_memory_cache_clock_hands_TSQL
- sys.dm_os_memory_cache_clock_hands
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_clock_hands dynamic management view
ms.assetid: 0660eddc-691c-425f-9d43-71151d644de7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c5bb5a19d77934a1d37a643d08d7b3e06175ea8d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839247"
---
# <a name="sysdm_os_memory_cache_clock_hands-transact-sql"></a>sys.dm_os_memory_cache_clock_hands (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt den Status der Zeiger für eine bestimmte Cacheclock zurück.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_memory_cache_clock_hands**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8)**|Adresse des Caches, der der Clock zugeordnet ist. Lässt keine NULL-Werte zu.|  
|**name**|**nvarchar(256)**|Name des Caches. Lässt keine NULL-Werte zu.|  
|**type**|**nvarchar(60)**|Typ des Cachespeichers. Es können mehrere Caches desselben Typs vorhanden sein. Lässt keine NULL-Werte zu.|  
|**clock_hand**|**nvarchar(60)**|Zeigertyp. Folgende Werte sind möglich:<br /><br /> Extern<br /><br /> Intern<br /><br /> Lässt keine NULL-Werte zu.|  
|**clock_status**|**nvarchar(60)**|Clockstatus. Folgende Werte sind möglich:<br /><br /> Ausgesetzt<br /><br /> Wird ausgeführt<br /><br /> Lässt keine NULL-Werte zu.|  
|**rounds_count**|**bigint**|Anzahl der Sweeps innerhalb des Caches zum Entfernen von Einträgen. Lässt keine NULL-Werte zu.|  
|**removed_all_rounds_count**|**bigint**|Anzahl der durch alle Sweeps entfernten Einträge. Lässt keine NULL-Werte zu.|  
|**updated_last_round_count**|**bigint**|Anzahl der während des letzten Sweeps aktualisierten Einträge. Lässt keine NULL-Werte zu.|  
|**removed_last_round_count**|**bigint**|Anzahl der während des letzten Sweeps entfernten Einträge. Lässt keine NULL-Werte zu.|  
|**last_tick_time**|**bigint**|Letzter Zeitpunkt, in Millisekunden, zu dem sich der Uhrzeiger bewegt hat. Lässt keine NULL-Werte zu.|  
|**round_start_time**|**bigint**|Zeitpunkt des letzten Sweeps in Millisekunden. Lässt keine NULL-Werte zu.|  
|**last_round_start_time**|**bigint**|Gesamtzeit in Millisekunden, die die Uhr für die letzte Umdrehung benötigt hat. Lässt keine NULL-Werte zu.|  
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
  
## <a name="remarks"></a>Bemerkungen  
 Die Informationen werden von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im Arbeitsspeicher in einer Struktur gespeichert, die als Arbeitsspeichercache bezeichnet wird. Die Informationen im Cache können Daten, Indexeinträge, kompilierte Prozedurpläne und eine Vielzahl anderer Typen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Informationen sein. Damit vermieden wird, dass die Informationen neu erstellt werden müssen, werden diese solange wie möglich im Arbeitsspeichercache beibehalten und erst dann aus dem Cache entfernt, wenn sie zu alt sind, um noch hilfreich zu sein, oder wenn der Arbeitsspeicherplatz für neue Informationen benötigt wird. Der Vorgang, bei dem alte Informationen entfernt werden, wird als Arbeitsspeichersweep bezeichnet. Der Arbeitsspeichersweep ist eine häufige, jedoch keine kontinuierliche Aktivität. Der Sweep des Arbeitsspeichercaches wird von einem Taktalgorithmus gesteuert. Jeder Takt kann mehrere Arbeitsspeichersweeps steuern, die als Zeiger bezeichnet werden. Der Taktzeiger des Arbeitsspeichercaches stellt die aktuelle Position eines der Zeiger eines Arbeitsspeichersweeps dar.  

## <a name="see-also"></a>Weitere Informationen  
 [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)    
 [sys.dm_os_memory_cache_counters &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-counters-transact-sql.md)
