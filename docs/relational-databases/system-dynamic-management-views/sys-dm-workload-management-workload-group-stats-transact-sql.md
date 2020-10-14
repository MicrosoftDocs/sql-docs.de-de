---
description: sys.dm_workload_management_workload_groups_stats (Transact-SQL)
title: sys.dm_workload_management_workload_groups_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/02/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
author: ronortloff
ms.author: rortloff
monikerRange: = azure-sqldw-latest||= sqlallproducts-allversions
ms.openlocfilehash: bc36846a62b8b71e0e21d7ea61d088a0f16c674d
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92035179"
---
# <a name="sysdm_workload_management_workload_groups_stats-transact-sql"></a>sys.dm_workload_management_workload_groups_stats (Transact-SQL)
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Gibt Statistiken zu Arbeits Auslastungs Gruppen und die effektiven Werte der Arbeits Auslastungs [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] Gruppe in zurück  
  
|Spaltenname|Datentyp|BESCHREIBUNG|Range|  
|-----------------|---------------|-----------------|-----------|  
|group_id|**int**|Eindeutige ID der Arbeitsauslastungsgruppe||
|name|**sysname**|Name der Arbeitsauslastungsgruppe.||
|statistics_start_time|**datetime**|Zeitpunkt, zu dem die Statistik Sammlung für die Arbeits Auslastungs Gruppe begann  Der Wert ist entweder beim Erstellen der Arbeits Auslastungs Gruppe oder beim Anhalten oder Skalieren der Instanz.||
|total_request_count|**bigint**|Kumulierte Anzahl vervollständigter Anforderungen in der Arbeitsauslastungsgruppe.||
|total_shared_resource_reqeusts|**bigint**|Kumulierte Anzahl der abgeschlossenen Anforderungen in der Arbeits Auslastungs Gruppe, die Ressourcen aus dem freigegebenen Pool verwendet haben.||
|total_queued_request_count|**bigint**|Kumulierte Anzahl von Anforderungen in der Warteschlange, nachdem das max_concurrency Limit erreicht wurde.||
|total_request_execution_timeouts|**bigint**|Kumulierte Anzahl von Anforderungen in der Arbeits Auslastungs Gruppe, für die vor Abschluss ein Timeout aufgetreten ist, basierend auf der query_execution_timeout_sec Einstellung||
|effective_min_percentage_resource|**tinyint**|Die effektive min_percentage_resource Einstellung, die die Einstellungen für die Dienst Ebene und die Arbeits Auslastungs Gruppe zulässt. Der effektive min_percentage_resource kann auf niedrigeren Dienst Ebenen höher angepasst werden.  Beispielsweise ist auf DW100c der niedrigste min_percentage_resource zulässiger Wert 25%.  Der min_percentage_resource wird auf 0% angepasst, wenn der Wert nicht auf Dienst Ebene erteilt werden kann.  Min_percentage_resource z. b. auf "DW6000c" auf 10% festgelegt, hätte beim horizontalen Herunterskalieren auf DW100c einen effective_min_percentage_resource von 0%.||
|effective_cap_percentage_resource|**tinyint**|Der effektive cap_percentage_resource für die Arbeits Auslastungs Gruppe.  Wenn eine andere Arbeits Auslastungs Gruppe mit min_percentage_resource > 0 vorhanden ist, wird die effective_cap_percentage_resource proportional gesenkt.||
|effective_request_min_resource_grant_percent|**Dezimalzahl (5, 2)**|Der effektive Lauf Zeitwert für request_min_resource_grant_percent der Arbeits Auslastungs Gruppe. Der effektive Wert in Bezug auf die Dienst Ebene und die Konfiguration der Arbeits Auslastungs Gruppe.  Wenn min_percentage_resource aufgrund der Dienst Ebene angepasst wird, werden effective_request_min_resource_grant_percent entsprechend angepasst.||
|effective_request_max_resource_grant_percent|**Dezimalzahl (5, 2)**|Der effektive Lauf Zeitwert für request_max_resource_grant_percent der Arbeits Auslastungs Gruppe, der die Konfiguration aller Arbeits Auslastungs Gruppen berücksichtigt.||
|||||

## <a name="see-also"></a>Siehe auch

 [Azure Synapse Analytics und parallele Data Warehouse dynamische Verwaltungs Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
