---
description: sys.dm_fts_active_catalogs (Transact-SQL)
title: sys.dm_fts_active_catalogs (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_active_catalogs_TSQL
- dm_fts_active_catalogs
- dm_fts_active_catalogs_TSQL
- sys.dm_fts_active_catalogs
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_active_catalogs dynamic management view
ms.assetid: 40ab5453-040c-4d2e-bb49-e340cf90c3ee
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 581dcf8dc3dace674f7112680981da638cf658ca
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838576"
---
# <a name="sysdm_fts_active_catalogs-transact-sql"></a>sys.dm_fts_active_catalogs (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt Informationen zu den Volltextkatalogen zurück, für die zurzeit Auffüllungsaktivitäten auf dem Server ausgeführt werden.  
  
> [!NOTE]
>  Die folgenden Spalten werden in einer zukünftigen Version von entfernt [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : is_paused, previous_status, previous_status_description, row_count_in_thousands, Status, status_description und worker_count. Verwenden Sie diese Spalten in Neuentwicklungen nicht. Planen Sie die Änderung von Anwendungen, die diese Spalten derzeit verwenden.  
  
 
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID der Datenbank, die den aktiven Volltextkatalog enthält.|  
|**catalog_id**|**int**|ID des aktiven Volltextkatalogs.|  
|**memory_address**|**varbinary(8)**|Adresse von Speicherpuffern, die für Auffüllungsaktivitäten im Zusammenhang mit diesem Volltextkatalog zugeordnet wurden.|  
|**name**|**nvarchar(128)**|Name des aktiven Volltextkatalogs.|  
|**is_paused**|**bit**|Gibt an, ob das Auffüllen des aktiven Volltextkatalogs angehalten wurde.|  
|**status**|**int**|Aktueller Status des Volltextkatalogs. Einer der folgenden:<br /><br /> 0 = Wird initialisiert<br /><br /> 1 = Bereit<br /><br /> 2 = Angehalten<br /><br /> 3 = Temporärer Fehler<br /><br /> 4 = Erneute Einbindung erforderlich<br /><br /> 5 = Herunterfahren<br /><br /> 6 = Übernehmen von Änderungen zu Sicherungszwecken<br /><br /> 7 = Sicherung erfolgt über Katalog<br /><br /> 8 = Katalog ist beschädigt|  
|**status_description**|**nvarchar(120)**|Beschreibung des aktuellen Status des aktiven Volltextkatalogs.|  
|**previous_status**|**int**|Vorhergehender Status des Volltextkatalogs. Einer der folgenden:<br /><br /> 0 = Wird initialisiert<br /><br /> 1 = Bereit<br /><br /> 2 = Angehalten<br /><br /> 3 = Temporärer Fehler<br /><br /> 4 = Erneute Einbindung erforderlich<br /><br /> 5 = Herunterfahren<br /><br /> 6 = Übernehmen von Änderungen zu Sicherungszwecken<br /><br /> 7 = Sicherung erfolgt über Katalog<br /><br /> 8 = Katalog ist beschädigt|  
|**previous_status_description**|**nvarchar(120)**|Beschreibung des vorhergehenden Status des aktiven Volltextkatalogs.|  
|**worker_count**|**int**|Anzahl der zurzeit aktiven Arbeitsthreads für diesen Volltextkatalog.|  
|**active_fts_index_count**|**int**|Anzahl der Volltextindizes, die aufgefüllt werden.|  
|**auto_population_count**|**int**|Anzahl von Tabellen, bei denen dieser Volltextkatalog automatisch aufgefüllt wird.|  
|**manual_population_count**|**int**|Anzahl von Tabellen, bei denen das manuelle Auffüllen für diesen Volltextkatalog ausgeführt wird.|  
|**full_incremental_population_count**|**int**|Anzahl von Tabellen, bei denen der Volltextkatalog vollständig oder inkrementell aufgefüllt wird.|  
|**row_count_in_thousands**|**int**|Geschätzte Zeilenanzahl (in Tausenden) in allen Volltextindizes in diesem Volltextkatalog.|  
|**is_importing**|**bit**|Gibt an, ob der Volltextkatalog importiert wird:<br /><br /> 1 = Der Katalog wird importiert.<br /><br /> 2 = Der Katalog wird nicht importiert.|  
  
## <a name="remarks"></a>Bemerkungen  
 Die is_importing Spalte war neu in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
   
## <a name="physical-joins"></a>Physische Joins  
 ![Wesentliche Joins dieser dynamischen Verwaltungssicht](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-active-catalogs-1.gif "Wesentliche Joins dieser dynamischen Verwaltungssicht")  
  
## <a name="relationship-cardinalities"></a>Kardinalität der Beziehungen  
  
|Von|Beschreibung|Relationship|  
|----------|--------|------------------|  
|dm_fts_active_catalogs.database_id|dm_fts_index_population.database_id|1:1|  
|dm_fts_active_catalogs.catalog_id|dm_fts_index_population.catalog_id|1:1|  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden Informationen zu den aktiven Volltextkatalogen in der aktuellen Datenbank zurückgegeben.  
  
```  
SELECT catalog.name, catalog.is_importing, catalog.auto_population_count,  
  OBJECT_NAME(population.table_id) AS table_name,  
  population.population_type_description, population.is_clustered_index_scan,  
  population.status_description, population.completion_type_description,  
  population.queued_population_type_description, population.start_time,  
  population.range_count   
FROM sys.dm_fts_active_catalogs catalog   
CROSS JOIN sys.dm_fts_index_population population   
WHERE catalog.database_id = population.database_id   
AND catalog.catalog_id = population.catalog_id   
AND catalog.database_id = (SELECT dbid FROM sys.sysdatabases WHERE name = DB_NAME());  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 
 [Dynamische Verwaltungs Sichten und Funktionen für die voll Text Suche und die semantische Suche &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
