---
description: sys.dm_fts_fdhosts (Transact-SQL)
title: sys.dm_fts_fdhosts (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_fts_fdhosts
- dm_fts_fdhosts_TSQL
- sys.dm_fts_fdhosts
- sys.dm_fts_fdhosts_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_fdhosts dynamic management view
- troubleshooting [SQL Server], full-text search
ms.assetid: d42a6334-4362-4361-83da-f8324fe55ec7
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d88fd09206405e9cc277f83d42a385a7e1542b5c
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100345341"
---
# <a name="sysdm_fts_fdhosts-transact-sql"></a>sys.dm_fts_fdhosts (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt Informationen über die aktuelle Aktivität des oder der Filterdaemonhosts auf der Serverinstanz zurück.  
  
 
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**fdhost_id**|**int**|ID des Filterdämonhosts.|  
|**fdhost_name**|**nvarchar(120)**|Name des Filterdämonhosts.|  
|**fdhost_process_id**|**int**|Windows-Prozess-ID des Filterdämonhosts.|  
|**fdhost_type**|**nvarchar(120)**|Typ des Dokuments, das vom Filterdämonhost verarbeitet wird. Ist einer der folgenden Typen:<br /><br /> Einzelthread<br /><br /> Multithread<br /><br /> Umfangreiches Dokument|  
|**max_thread**|**int**|Maximale Anzahl von Threads im Filterdämonhost.|  
|**batch_count**|**int**|Anzahl von Batches, die derzeit im Filterdämonhost verarbeitet werden.|  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   

## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden der Name des Filterdämonhosts und die maximale Anzahl der in ihm enthaltenen Threads zurückgegeben. Außerdem wird überwacht, wie viele Batches derzeit im Filterdämon verarbeitet werden. Diese Informationen können bei der Leistungsdiagnose verwendet werden.  
  
```  
SELECT fdhost_name, batch_count, max_thread FROM sys.dm_fts_fdhosts;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungs Sichten und Funktionen für die voll Text Suche und die semantische Suche &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Volltextsuche](../../relational-databases/search/full-text-search.md)  
  
  
