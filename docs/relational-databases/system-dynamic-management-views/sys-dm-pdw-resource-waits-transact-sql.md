---
description: sys.dm_pdw_resource_waits (Transact-SQL)
title: sys.dm_pdw_resource_waits (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/26/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: a43ce9a2-5261-41e3-97f0-555ba05ebed9
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: b8ab07f9c8b990b7d002de070ece8717fb90b97a
ms.sourcegitcommit: 32135463a8494d9ed1600a58f51819359e3c09dc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2020
ms.locfileid: "91834116"
---
# <a name="sysdm_pdw_resource_waits-transact-sql"></a>sys.dm_pdw_resource_waits (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Zeigt die Warte Informationen für alle Ressourcentypen in an [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] .  
  
|Spaltenname|Datentyp|BESCHREIBUNG|Range|  
|-----------------|---------------|-----------------|-----------|  
|wait_id|**bigint**|Die Position der Anforderung in der Warteliste.|0-basierte Ordnungszahl. Dies ist nicht über alle warte Einträge hinweg eindeutig.|  
|session_id|**nvarchar(32)**|Die ID der Sitzung, in der der Wartezustand aufgetreten ist.|Weitere Informationen finden Sie unter session_id in [sys.dm_pdw_exec_sessions &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).|  
|type|**nvarchar(255)**|Der Typ des warte Vorgangs, den dieser Eintrag darstellt.|Mögliche Werte:<br /><br /> Verbindung<br /><br /> Parallelität für lokale Abfragen<br /><br /> Parallelität verteilter Abfragen<br /><br /> DMS-Parallelität<br /><br /> Parallelität der Sicherung|  
|object_type|**nvarchar(255)**|Der Typ des Objekts, das vom warte Vorgang betroffen ist.|Mögliche Werte:<br /><br /> **Objekt**<br /><br /> **DATABASE**<br /><br /> **Anlage**<br /><br /> **SCHEMA**<br /><br /> **Asyl**|  
|object_name|**nvarchar (386)**|Der Name oder die GUID des angegebenen Objekts, das von der Wartezeit betroffen war.|Tabellen und Sichten werden mit dreiteiligen Namen angezeigt.<br /><br /> Indizes und Statistiken werden mit vierteiligen Namen angezeigt.<br /><br /> Namen, Prinzipale und Datenbanken sind Zeichen folgen Namen.|  
|request_id|**nvarchar(32)**|ID der Anforderung, für die der Wartezustand aufgetreten ist.|Der QID-Bezeichner der Anforderung.<br /><br /> GUID-Bezeichner für Ladeanforderungen.|  
|request_time|**datetime**|Der Zeitpunkt, zu dem die Sperre oder Ressource angefordert wurde.||  
|acquire_time|**datetime**|Der Zeitpunkt, zu dem die Sperre oder Ressource abgerufen wurde.||  
|state|**nvarchar(50)**|Status des warte Zustands.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|priority|**int**|Priorität des wartenden Elements.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|concurrency_slots_used|**int**|Intern|Weitere Informationen finden Sie unter [Monitor Ressourcen-warte](#monitor-resource-waits) Vorgänge|  
|resource_class|**nvarchar (20)**|Intern |Weitere Informationen finden Sie unter [Monitor Ressourcen-warte](#monitor-resource-waits) Vorgänge|  
  
## <a name="monitor-resource-waits"></a>Überwachen von Ressourcen Verzögerungen 
Mit der Einführung von [Arbeits](/azure/sql-data-warehouse/sql-data-warehouse-workload-isolation)Auslastungs Gruppen sind Parallelitäts Slots nicht mehr anwendbar.  Verwenden Sie die folgende Abfrage und die- `resources_requested` Spalte, um die Ressourcen zu verstehen, die zum Ausführen der Anforderung erforderlich sind.

```sql
select rw.wait_id
      ,rw.session_id
      ,rw.type
      ,rw.object_type
      ,rw.object_name
      ,rw.request_id
      ,rw.request_time
      ,rw.acquire_time
      ,rw.state
      ,resources_requested = s.effective_request_min_resource_grant_percent
      ,r.group_name
  from sys.dm_workload_management_workload_groups_stats s
  join sys.dm_pdw_exec_requests r on r.group_name = s.name collate SQL_Latin1_General_CP1_CI_AS
  join sys.dm_pdw_resource_waits rw on rw.request_id = r.request_id
```

## <a name="see-also"></a>Weitere Informationen  
 [SQL Data Warehouse und parallele Data Warehouse dynamischen Verwaltungs Sichten &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
