---
description: sys.dm_broker_queue_monitors (Transact-SQL)
title: sys. dm_broker_queue_monitors (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_broker_queue_monitors
- sys.dm_broker_queue_monitors_TSQL
- dm_broker_queue_monitors_TSQL
- sys.dm_broker_queue_monitors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_broker_queue_monitors dynamic management view
ms.assetid: 401207dc-ef4a-4a3f-879c-76dcbb52d6bc
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 8290b399d91bb196c818ba61fc7b685fcc23b383
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88498349"
---
# <a name="sysdm_broker_queue_monitors-transact-sql"></a>sys.dm_broker_queue_monitors (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile für jede Warteschlangenüberwachung in der Instanz zurück. Eine Warteschlangenüberwachung verwaltet die Aktivierung einer Warteschlange.  
  

|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Objekt-ID für die Datenbank mit der Warteschlange, die überwacht wird. Lässt NULL-Werte zu.|  
|**queue_id**|**int**|Objekt-ID der überwachten Warteschlange. Lässt NULL-Werte zu.|  
|**Status**|**nvarchar(32)**|Status des Überwachungsservers. Lässt NULL-Werte zu. Folgende Werte sind möglich:<br /><br /> **INACTIVE**<br /><br /> **NOTIFIED**<br /><br /> **RECEIVES_OCCURRING**|  
|**last_empty_rowset_time**|**datetime**|Zeitpunkt, zu dem bei einer RECEIVE-Anweisung aus der Warteschlange zuletzt ein leeres Ergebnis zurückgegeben wurde. Lässt NULL-Werte zu.|  
|**last_activated_time**|**datetime**|Zeitpunkt, zu dem die Warteschlangenüberwachung zuletzt eine gespeicherte Prozedur aktiviert hat. Lässt NULL-Werte zu.|  
|**tasks_waiting**|**int**|Anzahl von Sitzungen, die zurzeit in einer RECEIVE-Anweisung auf diese Warteschlange warten. Lässt NULL-Werte zu.<br /><br /> Hinweis: Diese Zahl umfasst alle Sitzungen, die eine RECEIVE-Anweisung ausführen, unabhängig davon, ob die Sitzung von der Warteschlangen Überwachung gestartet wurde. Dies gilt beim Verwenden von WAITFOR zusammen mit RECEIVE. Im Wesentlichen warten diese Tasks darauf, dass Nachrichten in der Warteschlange eintreffen.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-current-status-queue-monitor"></a>A. Aktueller Status der Warteschlangen  
 In diesem Szenario wird der aktuelle Status aller Nachrichtenwarteschlangen wiedergegeben.  
  
```  
SELECT t1.name AS [Service_Name],  t3.name AS [Schema_Name],  t2.name AS [Queue_Name],    
CASE WHEN t4.state IS NULL THEN 'Not available'   
ELSE t4.state   
END AS [Queue_State],    
CASE WHEN t4.tasks_waiting IS NULL THEN '--'   
ELSE CONVERT(VARCHAR, t4.tasks_waiting)   
END AS tasks_waiting,   
CASE WHEN t4.last_activated_time IS NULL THEN '--'   
ELSE CONVERT(varchar, t4.last_activated_time)   
END AS last_activated_time ,    
CASE WHEN t4.last_empty_rowset_time IS NULL THEN '--'   
ELSE CONVERT(varchar,t4.last_empty_rowset_time)   
END AS last_empty_rowset_time,   
(   
SELECT COUNT(*)   
FROM sys.transmission_queue t6   
WHERE (t6.from_service_name = t1.name) ) AS [Tran_Message_Count]   
FROM sys.services t1    INNER JOIN sys.service_queues t2   
ON ( t1.service_queue_id = t2.object_id )     
INNER JOIN sys.schemas t3 ON ( t2.schema_id = t3.schema_id )    
LEFT OUTER JOIN sys.dm_broker_queue_monitors t4   
ON ( t2.object_id = t4.queue_id  AND t4.database_id = DB_ID() )    
INNER JOIN sys.databases t5 ON ( t5.database_id = DB_ID() );  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungssichten in Verbindung mit Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
  

