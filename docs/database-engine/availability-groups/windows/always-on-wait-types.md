---
title: Identifizieren von Wartevorgängen von Verfügbarkeitsgruppen
description: Identifizieren von Wartevorgängen von Always On-Verfügbarkeitsgruppen mit Transact-SQL (T-SQL) und erweiterten Ereignissen.
ms.custom: ag-guide, seodec18
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: afa8caff-f325-48d9-a8ef-a30beab60389
author: rothja
ms.author: jroth
ms.openlocfilehash: 4be170149181263e7d904f5f820a9dd50f6d3801
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347492"
---
# <a name="identify-waits-associated-with-availability-groups"></a>Identifizieren von Wartevorgängen von Verfügbarkeitsgruppen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Bei der Problembehandlung der Latenz von Always On-Verfügbarkeitsgruppen, können Wartestatistiken mithilfe von Verfügbarkeitsgruppen-spezifischen Wartetypen in der dynamischen Verwaltungssicht (DMV) [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) auf Zuwachs überwacht werden.  
  
 Allgemeine Informationen zur Verwendung von Wartestatistiken finden Sie unter [SQL Server 2005 Waits and Queues (Wartezeiten und Warteschlangen in SQL Server 2005)](/previous-versions/sql/sql-server-2005/administrator/cc966413(v=technet.10)). Zwar wurde das Dokument für SQL Server 2005 geschrieben, dennoch sind die enthaltenen Informationen noch für die neueren Versionen von SQL Server anwendbar.  
  
## <a name="query-for-availability-groups-wait-types"></a>Abfragen von Wartetypen von Always On-Verfügbarkeitsgruppen  
 Verwenden Sie die folgende T-SQL-Abfrage, um alle Wartestatistiken mit den Wartetypen der Verfügbarkeitsgruppe abzurufen:  
  
```sql  
SELECT * FROM sys.dm_os_wait_stats   
WHERE wait_type LIKE '%hadr%'  
ORDER BY wait_time_ms DESC  
```  
  
 Verwenden Sie den folgenden T-SQL-Befehl, um die Wartestatistiken durch Erfassen erweiterter Ereignisse zu überwachen.  
  
```sql
CREATE EVENT SESSION [alwayson] ON SERVER   
ADD EVENT sqlos.wait_info(  
    WHERE ([wait_type]=(758) OR [wait_type]=(776) OR [wait_type]=(853) OR [wait_type]=(833)))  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,  
MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)  
GO  
```  
  
 Sie können die Zuordnung der Schlüsselwerte anzeigen, indem Sie die folgende Abfrage ausführen:  
  
```sql
SELECT * FROM sys.dm_xe_map_values   
WHERE name='wait_types' AND map_value LIKE '%hadr%'   
ORDER BY map_key ASC  
```  
  
## <a name="next-steps"></a>Nächste Schritte  
 [Wartetypen](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md#WaitTypes)  
  
