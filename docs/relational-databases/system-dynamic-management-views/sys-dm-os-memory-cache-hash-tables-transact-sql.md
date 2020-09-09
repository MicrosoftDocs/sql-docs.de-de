---
description: sys.dm_os_memory_cache_hash_tables (Transact-SQL)
title: sys. dm_os_memory_cache_hash_tables (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_memory_cache_hash_tables_TSQL
- sys.dm_os_memory_cache_hash_tables
- dm_os_memory_cache_hash_tables
- dm_os_memory_cache_hash_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_hash_tables dynamic management view
ms.assetid: 68b94f35-8f80-4d2b-bcde-7a21934219af
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0013d45519560f6bda0437f2977a1d071dd1b424
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89546539"
---
# <a name="sysdm_os_memory_cache_hash_tables-transact-sql"></a>sys.dm_os_memory_cache_hash_tables (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile für jeden aktiven Cache in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz zurück.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys. dm_pdw_nodes_os_memory_cache_hash_tables**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8)**|Die Adresse (Primärschlüssel) des Cacheeintrags. Lässt keine NULL-Werte zu.|  
|**name**|**nvarchar(256)**|Name des Caches. Lässt keine NULL-Werte zu.|  
|**type**|**nvarchar(60)**|Typ des Caches. Lässt keine NULL-Werte zu.|  
|**table_level**|**int**|Die Hashtabellennummer. Ein bestimmter Cache kann mehrere Hashtabellen besitzen, die unterschiedlichen Hashfunktionen entsprechen. Lässt keine NULL-Werte zu.|  
|**buckets_count**|**int**|Die Anzahl der Buckets in der Hashtabelle. Lässt keine NULL-Werte zu.|  
|**buckets_in_use_count**|**int**|Die Anzahl der Buckets, die zurzeit verwendet werden. Lässt keine NULL-Werte zu.|  
|**buckets_min_length**|**int**|Die minimale Anzahl von Cacheeinträgen in einem Bucket. Lässt keine NULL-Werte zu.|  
|**buckets_max_length**|**int**|Die maximale Anzahl von Cacheeinträgen in einem Bucket. Lässt keine NULL-Werte zu.|  
|**buckets_avg_length**|**int**|Die durchschnittliche Anzahl von Cacheeinträgen in jedem Bucket. Lässt keine NULL-Werte zu.|  
|**buckets_max_length_ever**|**int**|Die maximale Anzahl der Cacheinträge in einem Hashbucket für diese Hashtabelle seit dem Start des Servers. Lässt keine NULL-Werte zu.|  
|**hits_count**|**bigint**|Die Anzahl von Cachetreffern. Lässt keine NULL-Werte zu.|  
|**misses_count**|**bigint**|Die Anzahl von Cachefehlversuchen. Lässt keine NULL-Werte zu.|  
|**buckets_avg_scan_hit_length**|**int**|Die durchschnittliche Anzahl der untersuchten Einträge in einem Bucket, bevor das gesuchte Element gefunden wurde. Lässt keine NULL-Werte zu.|  
|**buckets_avg_scan_miss_length**|**int**|Die durchschnittliche Anzahl der untersuchten Einträge in einem Bucket, bevor die Suche ohne Erfolg beendet wurde. Lässt keine NULL-Werte zu.|  
|**pdw_node_id**|**int**|Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.<br /><br /> **Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]|  
  
## <a name="permissions"></a>Berechtigungen 

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] Premium-Tarifen ist die- `VIEW DATABASE STATE` Berechtigung in der Datenbank erforderlich. In [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] den Tarifen "Standard" und "Basic" ist der  **Server Administrator** oder ein **Azure Active Directory Administrator** Konto erforderlich.   

## <a name="see-also"></a>Weitere Informationen  
 
  [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


