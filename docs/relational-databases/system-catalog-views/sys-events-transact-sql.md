---
description: sys.events (Transact-SQL)
title: sys. Events (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.events_TSQL
- sys.events
- events_TSQL
- events
dev_langs:
- TSQL
helpviewer_keywords:
- sys.events catalog view
ms.assetid: f245a97a-80fc-43fb-a6e4-139420c9a47a
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3383217a22167b5255f748792952949396138fe4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97413051"
---
# <a name="sysevents-transact-sql"></a>sys.events (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Enthält eine Zeile für jedes Ereignis, durch das ein Trigger oder eine Ereignisbenachrichtigung ausgelöst wird. Diese Ereignisse stellen die Ereignistypen dar, die beim Erstellen des Triggers oder der Ereignisbenachrichtigung mit [CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md) oder [CREATE EVENT NOTIFICATION](../../t-sql/statements/create-event-notification-transact-sql.md)angegeben werden.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID des Triggers oder der Ereignisbenachrichtigung. Dieser Wert kennzeichnet zusammen mit **type** die Zeile eindeutig.|  
|**type**|**int**|Ereignis, das den Trigger auslöst.|  
|**type_desc**|**nvarchar(60)**|Beschreibung des Ereignisses, das einen Trigger auslöst.|  
|**is_trigger_event**|**bit**|1 = Triggerereignis.<br /><br /> 0 = Benachrichtigungsereignis.|  
|**event_group_type**|**int**|Ereignisgruppe, für die der Trigger oder die Ereignisbenachrichtigung erstellt wurde, bzw. NULL, wenn sie nicht für eine Ereignisgruppe erstellt wurden.|  
|**event_group_type_desc**|**nvarchar(60)**|Beschreibung der Ereignisgruppe, für die der Trigger oder die Ereignisbenachrichtigung erstellt wurde, bzw. NULL, wenn sie nicht für eine Ereignisgruppe erstellt wurden.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
