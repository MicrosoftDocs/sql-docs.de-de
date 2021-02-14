---
description: sys.dm_fts_index_population (Transact-SQL)
title: sys.dm_fts_index_population (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_index_population
- dm_fts_index_population
- sys.dm_fts_index_population_TSQL
- dm_fts_index_population_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_population dynamic management view
ms.assetid: 82d1c102-efcc-4b60-9a5e-3eee299bcb2b
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d7cb0887afa481cebcc5190ab0a28ddf307eb2c2
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100345298"
---
# <a name="sysdm_fts_index_population-transact-sql"></a>sys.dm_fts_index_population (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt Informationen zu den aktuell ausgeführten Auffüllungen des Volltextindexes und semantischen Schlüsselausdrucks zurück, die gerade in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt werden.  
 
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID der Datenbank, die den aufzufüllenden Volltextindex enthält.|  
|**catalog_id**|**int**|ID des Volltextkatalogs, in dem dieser Volltextindex enthalten ist.|  
|**table_id**|**int**|ID der Tabelle, für die der Volltextindex aufgefüllt wird.|  
|**memory_address**|**varbinary(8)**|Speicheradresse der internen Datenstruktur, mit der ein aktiver Auffüllprozess dargestellt wird.|  
|**population_type**|**int**|Typ der Auffüllung. Einer der folgenden:<br /><br /> 1 = Vollständiges Auffüllen<br /><br /> 2 = Inkrementelles, auf Timestamps basierendes Auffüllen<br /><br /> 3 = Manuelles Aktualisieren von Überarbeitungen<br /><br /> 4 = Aktualisieren von Überarbeitungen im Hintergrund.|  
|**population_type_description**|**nvarchar(120)**|Beschreibung des Auffüllungstyps.|  
|**is_clustered_index_scan**|**bit**|Gibt an, ob die Auffüllung einen Scanvorgang im gruppierten Index umfasst.|  
|**range_count**|**int**|Anzahl der Teilbereiche, in die diese Auffüllung parallelisiert wurde.|  
|**completed_range_count**|**int**|Anzahl der Bereiche, für die die Verarbeitung abgeschlossen ist.|  
|**outstanding_batch_count**|**int**|Aktuelle Anzahl ausstehender Batches für diese Auffüllung. Weitere Informationen finden Sie unter [sys.dm_fts_outstanding_batches &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-outstanding-batches-transact-sql.md).|  
|**status**|**int**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Status dieser Auffüllung. Hinweis: Einige Status sind vorübergehend. Einer der folgenden:<br /><br /> 3 = Wird gestartet<br /><br /> 5 = Normal verarbeiten<br /><br /> 7 = Verarbeitung wurde angehalten<br /><br /> Dieser Status tritt zum Beispiel auf, wenn eine automatische Zusammenführung ausgeführt wird.<br /><br /> 11 = Auffüllen abgebrochen<br /><br /> 12 = Verarbeiten einer semantischen Ähnlichkeitsextraktion|  
|**status_description**|**nvarchar(120)**|Beschreibung des Status der Auffüllung.|  
|**completion_type**|**int**|Status des Abschlusses dieser Auffüllung.|  
|**completion_type_description**|**nvarchar(120)**|Beschreibung des Abschlusstyps.|  
|**worker_count**|**int**|Dieser Wert ist immer 0.|  
|**queued_population_type**|**int**|Typ der Auffüllung, basierend auf Überarbeitungen, die ggf. nach der aktuellen Auffüllung folgt.|  
|**queued_population_type_description**|**nvarchar(120)**|Beschreibung der Auffüllung, die ggf. folgt. Wenn beispielsweise CHANGE TRACKING = AUTO und die erste vollständige Auffüllung durchgeführt wird, wird in dieser Spalte "Autom. Auffüllen" angezeigt.|  
|**start_time**|**datetime**|Zeit des Starts der Auffüllung.|  
|**incremental_timestamp**|**timestamp**|Stellt den Timestamp des Starts einer vollständigen Auffüllung dar. Für alle anderen Auffüllungstypen ist dieser Wert der letzte Prüfpunkt, für den ein Commit ausgeführt wurde, der den Fortschritt der Auffüllung darstellt.|  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn die statistische semantische Indizierung zusätzlich zur Volltextindizierung aktiviert ist, erfolgen die semantischen Extraktion und Auffüllung von Schlüsselausdrücken sowie die Extraktion von Dokumentähnlichkeitsdaten gleichzeitig mit der Volltextindizierung. Die Auffüllung des Dokumentähnlichkeitsindexes erfolgt später in einer zweiten Phase. Weitere Informationen finden Sie unter [Verwalten und Überwachen der semantischen Suche](../../relational-databases/search/manage-and-monitor-semantic-search.md).  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
  
## <a name="physical-joins"></a>Physische Joins  
 ![Wesentliche Joins dieser dynamischen Verwaltungssicht](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-index-population-1.gif "Wesentliche Joins dieser dynamischen Verwaltungssicht")  
  
## <a name="relationship-cardinalities"></a>Kardinalität der Beziehungen  
  
|From|Beschreibung|Relationship|  
|----------|--------|------------------|  
|dm_fts_active_catalogs.database_id|dm_fts_index_population.database_id|1:1|  
|dm_fts_active_catalogs.catalog_id|dm_fts_index_population.catalog_id|1:1|  
|dm_fts_population_ranges.parent_memory_address|dm_fts_index_population.memory_address|n:1|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten und Funktionen für die voll Text Suche und die semantische Suche &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  

