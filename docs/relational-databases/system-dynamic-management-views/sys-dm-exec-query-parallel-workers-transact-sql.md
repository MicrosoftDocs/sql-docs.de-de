---
description: sys.dm_exec_query_parallel_workers (Transact-SQL)
title: sys.dm_exec_query_parallel_workers (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_query_parallel_workers_TSQL
- dm_exec_query_parallel_workers
- sys.dm_exec_query_parallel_workers_TSQL
- sys.dm_exec_query_parallel_workers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_parallel_workers dynamic management view
ms.assetid: 1d72cef1-22d8-4ae0-91db-6694fe918c9f
author: pelopes
ms.author: pelopes
manager: ajayj
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 30ea6f1642da78754560b26a8772c33ab341d2b4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477241"
---
# <a name="sysdm_exec_query_parallel_workers-transact-sql"></a>sys.dm_exec_query_parallel_workers (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Gibt workerverfügbarkeits-Informationen pro Knoten zurück.  
  
|Name|Datentyp|Beschreibung|  
|----------|---------------|-----------------|  
|**node_id**|**int**|NUMA-Knoten-ID.|  
|**scheduler_count**|**int**|Anzahl der Planer auf diesem Knoten.|  
|**max_worker_count**|**int**|Maximale Anzahl von Workern für parallele Abfragen.|  
|**reserved_worker_count**|**int**|Anzahl der Arbeitsthreads, die von parallelen Abfragen reserviert werden, sowie die Anzahl der Haupt Arbeitsthreads, die von allen Anforderungen verwendet| 
|**free_worker_count**|**int**|Anzahl der Worker, die für Aufgaben verfügbar sind.<br /><br />**Hinweis:** jede eingehende Anforderung beansprucht mindestens einen Worker, der von der Anzahl der freien Worker subtrahiert wird.  Es ist möglich, dass die Anzahl der freien Worker auf einem stark ausgelasteten Server eine negative Zahl sein kann.| 
|**used_worker_count**|**int**|Anzahl von Workern, die von parallelen Abfragen verwendet werden.|  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken `Server admin` oder ein `Azure Active Directory admin` Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
 
## <a name="examples"></a>Beispiele  
  
### <a name="a-viewing-current-parallel-worker-availability"></a>A. Anzeigen der aktuellen Verfügbarkeit paralleler Worker  

```sql 
SELECT * FROM sys.dm_exec_query_parallel_workers;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten und-Funktionen im Zusammenhang mit der Ausführung &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_os_workers &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md)
