---
description: sys.dm_os_dispatcher_pools (Transact-SQL)
title: sys.dm_os_dispatcher_pools (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_dispatcher_pools_TSQL
- dm_os_dispatcher_pools
- sys.dm_os_dispatcher_pools
- sys.dm_os_dispatcher_pools_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- extended events [SQL Server], views
- sys.dm_os_dispatcher_pools DMV
ms.assetid: b9edbc83-c6bc-4753-9bb5-a454cfe7d6bf
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: dcd6c612f406e3c4ae10c7ab0aacfeb3a71fd9c9
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100338875"
---
# <a name="sysdm_os_dispatcher_pools-transact-sql"></a>sys.dm_os_dispatcher_pools (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt Informationen zu Sitzungsverteilerpools zurück. Verteilerpools sind von Systemkomponenten verwendete Threadpools für die Hintergrundverarbeitung.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_dispatcher_pools**.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|dispatcher_pool_address|**varbinary(8)**|Die Adresse des Verteilerpools. dispatcher_pool_address ist eindeutig. Lässt keine NULL-Werte zu.|  
|Typ|**nvarchar(256)**|Der Typ des Verteilerpools. Lässt keine NULL-Werte zu. Es gibt zwei Typen von Verteilerpools:<br /><br /> DISP_POOL_XE_ENGINE<br /><br /> DISP_POOL_XE_SESSION<br /><br /> Abfragen der DMV für die vollständige Liste|  
|name|**nvarchar(256)**|Der Name des Verteilerpools Lässt keine NULL-Werte zu.|  
|dispatcher_count|**int**|Die Anzahl aktiver Verteilerthreads Lässt keine NULL-Werte zu.|  
|dispatcher_ideal_count|**int**|Die Anzahl zu verwendender Verteilerthreads, die der Verteilerpool erhöhen kann. Lässt keine NULL-Werte zu.|  
|dispatcher_timeout_ms|**int**|Die Zeit in Millisekunden, die ein Verteiler auf neue Arbeit wartet, bevor er beendet wird. Lässt keine NULL-Werte zu.|  
|dispatcher_waiting_count|**int**|Die Anzahl von Verteilerthreads im Leerlauf. Lässt keine NULL-Werte zu.|  
|queue_length|**int**|Die Anzahl von Arbeitselementen, die auf die Verarbeitung durch den Verteilerpool warten. Lässt keine NULL-Werte zu.|  
|pdw_node_id|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   

## <a name="see-also"></a>Siehe auch  
  
  


