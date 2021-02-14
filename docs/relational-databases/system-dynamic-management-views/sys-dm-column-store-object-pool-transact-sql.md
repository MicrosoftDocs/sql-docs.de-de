---
description: sys.dm_column_store_object_pool (Transact-SQL)
title: sys.dm_column_store_object_pool (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: a8a58ca7-0a7d-4786-bfd9-e8894bd345dd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bad20aaf35fd0ddc53627648166e560cedc06ae3
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100345334"
---
# <a name="sysdm_column_store_object_pool-transact-sql"></a>sys.dm_column_store_object_pool (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

 Gibt die Anzahl der unterschiedlichen Typen der Objektspeicher Pool-Verwendung für columnstore--Index Objekte zurück.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**database_id**|INT|Die ID der Datenbank. Dies ist innerhalb einer Instanz einer SQL Server-Datenbank oder eines Azure SQL-Datenbankservers eindeutig. |  
|**object_id**|INT|ID des Objekts. Bei dem Objekt handelt es sich um einen der object_types. | 
|**index_id**|INT|ID des columnstore-Indexes.|  
|**partition_number**|BIGINT|Auf 1 basierende Partitionsnummer im Index oder Heap. Jede Tabelle oder Sicht weist mindestens eine Partition auf.| 
|**column_id**|INT|ID der columnstore-Spalte. Dies ist für DELETE_BITMAP NULL.| 
|**row_group_id**|INT|ID der Zeilen Gruppe.|
|**object_type**|SMALLINT|1 = COLUMN_SEGMENT<br /><br /> 2 = COLUMN_SEGMENT_PRIMARY_DICTIONARY<br /><br /> 3 = COLUMN_SEGMENT_SECONDARY_DICTIONARY<br /><br /> 4 = COLUMN_SEGMENT_BULKINSERT_DICTIONARY<br /><br /> 5 = COLUMN_SEGMENT_DELETE_BITMAP|  
|**object_type_desc**|nvarchar(60)|COLUMN_SEGMENT-ein Spalten Segment. `object_id` die Segment-ID. Ein Segment speichert alle Werte für eine Spalte innerhalb einer Zeilen Gruppe. Wenn eine Tabelle z. b. 10 Spalten enthält, gibt es 10 Spalten Segmente pro Zeilen Gruppe. <br /><br /> COLUMN_SEGMENT_PRIMARY_DICTIONARY: ein globales Wörterbuch, das Suchinformationen für alle Spalten Segmente in der Tabelle enthält.<br /><br /> COLUMN_SEGMENT_SECONDARY_DICTIONARY: ein lokales Wörterbuch, das einer Spalte zugeordnet ist.<br /><br /> COLUMN_SEGMENT_BULKINSERT_DICTIONARY: eine weitere Darstellung des globalen Wörterbuchs. Dies ermöglicht eine umgekehrte Suche nach Werten, die dictionary_id werden können. Wird zum Erstellen komprimierter Segmente als Teil von tupelverschiebungs-oder Massen Laden verwendet.<br /><br /> COLUMN_SEGMENT_DELETE_BITMAP: eine Bitmap, die Segment Löschungen nachverfolgt. Es gibt eine DELETE-Bitmap pro Partition.|  
|**access_count**|INT|Anzahl der Lese-oder Schreibzugriffe auf dieses Objekt.|  
|**memory_used_in_bytes**|BIGINT|Von diesem Objekt im Objekt Pool verwendeter Arbeitsspeicher.|  
|**object_load_time**|datetime|Uhrzeit, zu der object_id in den Objekt Pool eingefügt wurde.|  
  
## <a name="permissions"></a>Berechtigungen  

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
 
## <a name="see-also"></a>Weitere Informationen  
  
 [Index Related Dynamic Management Views and Functions (Transact-SQL) (Indexbezogene dynamische Verwaltungssichten und -funktionen (Transact-SQL))](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [Überwachen und Optimieren der Leistung](../../relational-databases/performance/monitor-and-tune-for-performance.md)  
 [Columnstore-Indizes: Übersicht](../../relational-databases/indexes/columnstore-indexes-overview.md) 
  
