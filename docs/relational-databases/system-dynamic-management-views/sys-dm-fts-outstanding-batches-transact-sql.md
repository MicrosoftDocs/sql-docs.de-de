---
description: sys.dm_fts_outstanding_batches (Transact-SQL)
title: sys.dm_fts_outstanding_batches (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_fts_outstanding_batches
- dm_fts_outstanding_batches_TSQL
- sys.dm_fts_outstanding_batches_TSQL
- sys.dm_fts_outstanding_batches
dev_langs:
- TSQL
helpviewer_keywords:
- troubleshooting [SQL Server], full-text search
- sys.dm_fts_outstanding_batches dynamic management view
ms.assetid: c4d697ed-c906-4c28-b137-036a25e13c84
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2932f123ac2b7f1125d187c943645c15d09e52d0
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100347935"
---
# <a name="sysdm_fts_outstanding_batches-transact-sql"></a>sys.dm_fts_outstanding_batches (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt Informationen zu den einzelnen Volltext-Indizierungsbatches zurück.  
  
  |Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|database_id|**int**|ID der Datenbank|  
|catalog_id|**int**|ID des Volltextkatalogs|  
|table_id|**int**|ID der Tabellen-ID, die den Volltextindex enthält|  
|batch_id|**int**|Batch-ID|  
|memory_address|**varbinary(8)**|Die Speicheradresse des Batchobjekts|  
|crawl_memory_address|**varbinary(8)**|Speicheradresse des Durchforstungsobjekts (übergeordnetes Objekt)|  
|memregion_memory_address|**varbinary(8)**|Arbeitsspeicherbereichs-Speicheradresse des ausgehenden freigegebenen Speichers des Filterdaemonhosts (fdhost.exe)|  
|hr_batch|**int**|Zuletzt aufgetretener Fehlercode für den Batch|  
|is_retry_batch|**bit**|Gibt an, ob dies ein Wiederholungsbatch ist:<br /><br /> 0 = Nein<br /><br /> 1 = Ja|  
|retry_hints|**int**|Typ der für den Batch benötigten Wiederholung:<br /><br /> 0 = Keine Wiederholung<br /><br /> 1 = Multithreadwiederholung<br /><br /> 2 = Einzelthreadwiederholung<br /><br /> 3 = Einzel- und Multithreadwiederholung<br /><br /> 5 = Letzte Multithreadwiederholung<br /><br /> 6 = Letzte Einzelthreadwiederholung<br /><br /> 7 = Letzte Einzel- und Multithreadwiederholung|  
|retry_hints_description|**nvarchar(120)**|Beschreibung des benötigten Wiederholungstyps:<br /><br /> NO RETRY<br /><br /> MULTI THREAD RETRY<br /><br /> SINGLE THREAD RETRY<br /><br /> SINGLE AND MULTI THREAD RETRY<br /><br /> MULTI THREAD FINAL RETRY<br /><br /> SINGLE THREAD FINAL RETRY<br /><br /> SINGLE AND MULTI THREAD FINAL RETRY|  
|doc_failed|**bigint**|Anzahl der fehlgeschlagenen Dokumente im Batch|  
|batch_timestamp|**timestamp**|Der Timestampwert, der bei der Erstellung des Batches erhalten wurde|  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird in Erfahrung gebracht, wieviele Batches derzeit für jede Tabelle in der Serverinstanz verarbeitet werden.  
  
```  
SELECT database_id, table_id, COUNT(*) AS batch_count FROM sys.dm_fts_outstanding_batches GROUP BY database_id, table_id ;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungs Sichten und Funktionen für die voll Text Suche und die semantische Suche &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Volltextsuche](../../relational-databases/search/full-text-search.md)  
  
  
