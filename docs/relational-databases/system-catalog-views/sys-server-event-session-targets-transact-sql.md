---
description: sys.server_event_session_targets (Transact-SQL)
title: sys.server_event_session_targets (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- server_event_session_targets_TSQL
- sys.server_event_session_targets_TSQL
- sys.server_event_session_targets
- server_event_session_targets
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_event_session_targets catalog view
- xe
ms.assetid: dda4879d-57ae-4267-b410-1ef5c37404c7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d69b9273cbcf67484568300793ece0cb8f0c05e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172633"
---
# <a name="sysserver_event_session_targets-transact-sql"></a>sys.server_event_session_targets (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Gibt für eine Ereignissitzung eine Zeile für jedes Ereignisziel zurück.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|Die ID der Ereignissitzung. Lässt keine NULL-Werte zu.|  
|target_id|**int**|Die ID des Ziels. Die ID ist innerhalb des Ereignissitzungsobjekts eindeutig. Lässt keine NULL-Werte zu.|  
|name|**sysname**|Der Name des Ereignisziels. Lässt keine NULL-Werte zu.|  
|Paket|**sysname**|Der Name des Ereignispakets, das das Ereignisziel enthält. Lässt keine NULL-Werte zu.|  
|module|**sysname**|Der Name des Moduls, das das Ereignisziel enthält. Lässt keine NULL-Werte zu.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="remarks"></a>Bemerkungen  
 Diese Sicht hat die folgende Kardinalität der Beziehungen.  
  
| Von | Beschreibung | Beziehung |
| ---- | -- | ------------ |
|sys.server_event_session_targets.event_session_id|sys.server_event_sessions sys.server_event_sessions.event_session_id|n:1|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Katalog Sichten für erweiterte Ereignisse &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Erweiterte Ereignisse](../../relational-databases/extended-events/extended-events.md)  
  
  
