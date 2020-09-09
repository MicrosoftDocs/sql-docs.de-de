---
description: sys.dm_hadr_name_id_map (Transact-SQL)
title: sys. dm_hadr_name_id_map (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_hadr_name_id_map
- sys.dm_hadr_name_id_map_TSQL
- dm_hadr_name_id_map
- dm_hadr_name_id_map_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_name_id_map dynamic management view
ms.assetid: e07bb8a9-37de-4a39-a257-950d7c3ae8fb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cdd98f7eaa120814a8f615738cc0cee284b60269
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89533094"
---
# <a name="sysdm_hadr_name_id_map-transact-sql"></a>sys.dm_hadr_name_id_map (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Zeigt die Zuordnung von Always on-Verfügbarkeitsgruppen an, die die aktuelle Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit drei eindeutigen IDs verknüpft hat: eine Verfügbarkeits Gruppen-ID, eine wsfc-Ressourcen-ID und eine wsfc-Gruppen-ID. Der Zweck dieser Zuordnung ist, das Szenario zu behandeln, in dem die WSFC-Ressource/Gruppe umbenannt wird.  
   
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**ag_name**|**nvarchar(256)**|Der Name der Verfügbarkeitsgruppe. Dies ist ein vom Benutzer angegebener Name, der innerhalb des Windows Server-Failoverclusters (WSFC) eindeutig sein muss.|  
|**ag_id**|**uniqueidentifier**|Eindeutiger Bezeichner (GUID) der Verfügbarkeitsgruppe.|  
|**ag_resource_id**|**nvarchar(256)**|Eindeutige ID der Verfügbarkeitsgruppe als Ressource im WSFC-Cluster.|  
|**ag_group_id**|**nvarchar(256)**|Eindeutiger WSFC-Gruppen-ID der Verfügbarkeitsgruppe.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Always on Verfügbarkeits gruppendynamische Verwaltungs Sichten und Funktionen &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Katalogsichten Always On-Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Überwachen von Verfügbarkeits Gruppen &#40;Transact-SQL-&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
