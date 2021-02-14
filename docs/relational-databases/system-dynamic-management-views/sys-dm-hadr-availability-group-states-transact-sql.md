---
description: sys.dm_hadr_availability_group_states (Transact-SQL)
title: sys.dm_hadr_availability_group_states (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_hadr_availability_group_states
- sys.dm_hadr_availability_group_states_TSQL
- dm_hadr_availability_group_states_TSQL
- dm_hadr_availability_group_states
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_availability_group_states dynamic management view
ms.assetid: d18019dd-f8dc-4492-b035-b1a639369b65
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ee24a962698e2a661b4859c846a748a5b8517e7d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354163"
---
# <a name="sysdm_hadr_availability_group_states-transact-sql"></a>sys.dm_hadr_availability_group_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile für jede Always on Verfügbarkeits Gruppe zurück, die ein Verfügbarkeits Replikat in der lokalen Instanz von besitzt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . In jede Zeile werden die Statuswerte angezeigt, die den Zustand einer angegebenen Verfügbarkeitsgruppe definieren.  
  
> [!NOTE]  
>  Um die komplette Liste von zu erhalten, Fragen Sie die [sys.availability_groups](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) -Katalog Sicht ab.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Eindeutiger Bezeichner der Verfügbarkeitsgruppe.|  
|**primary_replica**|**varchar(128)**|Name der Serverinstanz, die das aktuelle primäre Replikat hostet.<br /><br /> NULL = Nicht das primäre Replikat, oder mit dem WSFC-Failovercluster kann nicht kommuniziert werden.|  
|**primary_recovery_health**|**tinyint**|Gibt den Wiederherstellungszustand des primären Replikats an. Folgende Werte sind möglich:<br /><br /> 0 = in Bearbeitung<br /><br /> 1 = Online<br /><br /> NULL<br /><br /> Auf sekundären Replikaten ist die **primary_recovery_health** Spalte NULL.|  
|**primary_recovery_health_desc**|**nvarchar(60)**|Beschreibung der **primary_replica_health**, eine der folgenden:<br /><br /> ONLINE_IN_PROGRESS<br /><br /> ONLINE<br /><br /> NULL|  
|**secondary_recovery_health**|**tinyint**|Gibt den Wiederherstellungs Zustand eines sekundären Replikat Replikats an. folgende:<br /><br /> 0 = in Bearbeitung<br /><br /> 1 = Online<br /><br /> NULL<br /><br /> Auf dem primären Replikat ist die **secondary_recovery_health** Spalte NULL.|  
|**secondary_recovery_health_desc**|**nvarchar(60)**|Beschreibung der **secondary_recovery_health**, eine der folgenden:<br /><br /> ONLINE_IN_PROGRESS<br /><br /> ONLINE<br /><br /> NULL|  
|**synchronization_health**|**tinyint**|Gibt einen Rollup des **synchronization_health** aller Verfügbarkeits Replikate in der Verfügbarkeits Gruppe wieder. Unten sind die möglichen Werte und ihre Beschreibungen aufgeführt.<br /><br /> 0: nicht fehlerfrei. Keines der Verfügbarkeits Replikate hat einen fehlerfreien **synchronization_health** (2 = fehlerfrei).<br /><br /> 1: teilweise fehlerfrei. Der Synchronisierungsstatus einiger, aber nicht aller Verfügbarkeitsreplikate ist fehlerfrei.<br /><br /> 2: fehlerfrei. Der Synchronisierungsstatus jedes Verfügbarkeitsreplikats ist fehlerfrei.<br /><br /> Informationen zum Integritäts Status der Replikat Synchronisierung finden Sie in der Spalte **synchronization_health** in [sys.dm_hadr_availability_replica_states &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md).|  
|**synchronization_health_desc**|**nvarchar(60)**|Beschreibung der **synchronization_health**, eine der folgenden:<br /><br /> NOT_HEALTHY<br /><br /> PARTIALLY_HEALTHY<br /><br /> HEALTHY|  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Überwachen von Verfügbarkeits Gruppen &#40;Transact-SQL-&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Dynamische Verwaltungssichten und -funktionen für Always On-Verfügbarkeitsgruppen (Transact-SQL)](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)  
  
  
