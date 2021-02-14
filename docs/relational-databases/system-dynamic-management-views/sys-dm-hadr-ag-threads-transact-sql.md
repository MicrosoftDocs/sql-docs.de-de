---
description: sys.dm_hadr_ag_threads (Transact-SQL)
title: sys.dm_hadr_ag_threads (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/08/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_hadr_ag_threads_TSQL
- sys.dm_hadr_ag_threads
- dm_hadr_ag_threads_TSQL
- dm_hadr_ag_threads
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_ag_threads catalog view
ms.assetid: ''
author: kfarlee
ms.author: kfarlee
monikerRange: ''
ms.openlocfilehash: c8487dd07e424b7edccc17b8a9587cfc91f74268
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100354128"
---
# <a name="sysdm_hadr_ag_threads-transact-sql"></a>sys.dm_hadr_ag_threads (Transact-SQL)

Mithilfe der DMVs für die HADR-Thread Telemetrie (**sys.dm_hadr_ag_threads** und [sys.dm_hadr_db_threads](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-db-threads-transact-sql.md)) können Benutzer schnell Einblicke in die Thread Verwendung durch eine Verfügbarkeits Gruppe und eine hoch Verfügbarkeits Datenbank erhalten. Das Verständnis dieser Thread Verwendung ist ein wichtiger Benchmark zum Optimieren von Verfügbarkeits Gruppen.

Diese DMV meldet die Thread Verwendung auf Verfügbarkeits Gruppenebene.

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Der Bezeichner der Verfügbarkeits Gruppe.|
|**name**|**nvarchar(128)**|Der Name der Verfügbarkeitsgruppe.|
|**num_databases**|**int**|Anzahl der Datenbanken in der Verfügbarkeits Gruppe.|
|**num_capture_threads**|**int**|Anzahl der Protokoll Erfassungs Threads, die von allen Datenbanken in dieser Verfügbarkeits Gruppe verwendet werden.|
|**num_redo_threads**|**int**|Anzahl der Redo-Threads, die von allen Datenbanken in dieser Verfügbarkeits Gruppe verwendet werden.|
|**num_parallel_redo_threads**|**int**|Anzahl paralleler Redo-Threads, die von allen Datenbanken in dieser Verfügbarkeits Gruppe verwendet werden.|
|**num_hadr_threads**|**int**|Anzahl aller Always on-Threads, die von allen Datenbanken in dieser Verfügbarkeits Gruppe verwendet werden.|

## <a name="permissions"></a>Berechtigungen  

 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="see-also"></a>Weitere Informationen  

 [Always on Verfügbarkeits gruppendynamische Verwaltungs Sichten und Funktionen &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Katalogsichten Always On-Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Überwachen von Verfügbarkeits Gruppen &#40;Transact-SQL-&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  