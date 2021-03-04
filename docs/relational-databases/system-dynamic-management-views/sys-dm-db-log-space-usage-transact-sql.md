---
description: sys.dm_db_log_space_usage (Transact-SQL)
title: sys.dm_db_log_space_usage (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_db_log_space_usage
- sys.dm_db_log_space_usage_TSQL
- dm_db_log_space_usage
- dm_db_log_space_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_log_space_usage dynamic management view
ms.assetid: f6b40060-c17d-472f-b0a3-3b350275d487
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9057a03c6a747184c8a4f884e9bd73404726cf41
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839382"
---
# <a name="sysdm_db_log_space_usage-transact-sql"></a>sys.dm_db_log_space_usage (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Gibt Informationen zur Speicherplatz Verwendung für das Transaktionsprotokoll zurück. 
  
> [!NOTE]
> Alle Transaktionsprotokoll Dateien werden kombiniert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|database_id|**smallint**|Datenbank-ID|  
|total_log_size_in_bytes |**bigint** |Die Größe des Protokolls  |
|used_log_space_in_bytes |**bigint** |Die besetzte Größe des Protokolls.  |     
|used_log_space_in_percent |**real** |Die besetzte Größe des Protokolls als Prozentsatz der gesamten Protokoll Größe |
|log_space_in_bytes_since_last_backup |**bigint** |Der Speicherplatz, der seit der letzten Protokoll Sicherung verwendet wurde. <br />**Gilt für:** [!INCLUDE[sssql14-md](../../includes/sssql14-md.md)] bis [!INCLUDE[sscurrent-md](../../includes/ssnoversion-md.md)] ,  [!INCLUDE[ssSDS](../../includes/sssds-md.md)] .|
    
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-determine-the-amount-of-free-log-space-in-tempdb"></a>A. Ermitteln der Menge an freiem Protokoll Speicherplatz in tempdb   
Mit der folgenden Abfrage wird der gesamte freie Protokoll Speicherplatz in Megabyte (MB), der in tempdb verfügbar ist, zurückgegeben.

```sql
USE tempdb;  
GO  

SELECT (total_log_size_in_bytes - used_log_space_in_bytes)*1.0/1024/1024 AS [free log space in MB]  
FROM sys.dm_db_log_space_usage;  
```
  
## <a name="see-also"></a>Weitere Informationen  
[Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
[Dynamische Verwaltungs Sichten im Zusammenhang mit der Datenbank &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
[sys.dm_db_file_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql.md)    
[sys.dm_db_task_space_usage &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md)   
[sys.dm_db_session_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)  
[sys.dm_db_log_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-info-transact-sql.md)    
[sys.dm_db_log_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-log-stats-transact-sql.md)