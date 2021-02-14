---
description: sys.dm_os_memory_pools (Transact-SQL)
title: sys.dm_os_memory_pools (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_memory_pools_TSQL
- dm_os_memory_pools
- dm_os_memory_pools_TSQL
- sys.dm_os_memory_pools
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_pools dynamic management view
ms.assetid: 1ef053f3-c6f3-456e-82b6-26e4bd630d46
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5ea86721a9a9b0543bf32f0c2aba17ac5be66368
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100339451"
---
# <a name="sysdm_os_memory_pools-transact-sql"></a>sys.dm_os_memory_pools (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile für jeden Objektspeicher in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz zurück. Mit dieser Sicht kann die Cachespeichernutzung überwacht und schlechtes Cacheverhalten identifiziert werden.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_memory_pools**.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**memory_pool_address**|**varbinary(8)**|Speicheradresse des Eintrags, der für den Speicherpool steht. Lässt keine NULL-Werte zu.|  
|**pool_id**|**int**|ID eines bestimmten Pools in einer Gruppe von Pools. Lässt keine NULL-Werte zu.|  
|**type**|**nvarchar(60)**|Typ des Objektpools. Lässt keine NULL-Werte zu. Weitere Informationen finden Sie unter [sys.dm_os_memory_clerks &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).|  
|**name**|**nvarchar(256)**|Vom System zugewiesener Name des Speicherobjekts. Lässt keine NULL-Werte zu.|  
|**max_free_entries_count**|**bigint**|Maximale Anzahl freier Einträge, die ein Pool haben kann. Lässt keine NULL-Werte zu.|  
|**free_entries_count**|**bigint**|Anzahl der derzeit im Pool befindlichen freien Einträge. Lässt keine NULL-Werte zu.|  
|**removed_in_all_rounds_count**|**bigint**|Anzahl der seit dem Starten der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz aus dem Pool entfernten Einträge. Lässt keine NULL-Werte zu.|  
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   

## <a name="remarks"></a>Bemerkungen  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Komponenten verwenden gelegentlich ein gemeinsames Poolframework zum Zwischenspeichern homogener, statusfreier Datentypen. Das Poolframework ist einfacher als das Cacheframework. Alle Einträge in den Pools werden als gleichwertig betrachtet. Pools sind in interner Hinsicht Arbeitsspeicherclerks und können an den Stellen verwendet werden, an denen Arbeitsspeicherclerks verwendet werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 
  [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


