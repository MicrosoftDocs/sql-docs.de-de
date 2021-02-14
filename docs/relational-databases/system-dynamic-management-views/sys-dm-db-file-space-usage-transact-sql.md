---
description: sys.dm_db_file_space_usage (Transact-SQL)
title: sys.dm_db_file_space_usage (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_file_space_usage
- sys.dm_db_file_space_usage_TSQL
- sys.dm_db_file_space_usage
- dm_db_file_space_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_file_space_usage dynamic management view
ms.assetid: 148a5276-a8d5-49d2-8146-3c63d24c2144
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 495e70e426f4a015c1d789526e4ad7292e2503e4
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343126"
---
# <a name="sysdm_db_file_space_usage-transact-sql"></a>sys.dm_db_file_space_usage (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt Informationen zur Speicherplatz Verwendung für jede Datendatei in der Datenbank zurück.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_db_file_space_usage**.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|database_id|**smallint**|Datenbank-ID|  
|file_id|**smallint**|Die Datei-ID<br /><br /> file_id wird file_id in [sys.dm_io_virtual_file_stats](../../relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md) und in [sys.sysDateien](../../relational-databases/system-compatibility-views/sys-sysfiles-transact-sql.md).|  
|filegroup_id|**smallint**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Die Dateigruppen-ID.|  
|total_page_count|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Die Gesamtanzahl der Seiten in der Datendatei.|  
|allocated_extent_page_count|**bigint**|**Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Gesamtanzahl der Seiten in den zugeordneten Blöcken in der Datendatei.|  
|unallocated_extent_page_count|**bigint**|Die Gesamtanzahl der Seiten in den nicht zugeordneten Blöcken in der Datendatei.<br /><br /> Nicht verwendete Seiten in nicht zugeordneten Blöcken sind nicht enthalten.|  
|version_store_reserved_page_count|**bigint**|Gesamtzahl der Seiten in den gleichartigen Blöcken, die dem Versionsspeicher zugeordnet werden. Versionsspeicherseiten werden nie aus gemischten Blöcken zugeordnet.<br /><br /> IAM-Seiten sind nicht enthalten, da sie immer aus gemischten Blöcken zugeordnet werden. PFS-Seiten sind dann enthalten, wenn sie aus einem einheitlichen Block zugeordnet werden.<br /><br /> Weitere Informationen finden Sie unter [sys.dm_tran_version_store &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-transact-sql.md).|  
|user_object_reserved_page_count|**bigint**|Gesamtzahl der Seiten, die aus gleichartigen Blöcken für Benutzerobjekte in der Datenbank zugeordnet werden. Nicht verwendete Seiten aus einem zugeordneten Block sind in der Gesamtzahl enthalten.<br /><br /> IAM-Seiten sind nicht enthalten, da sie immer aus gemischten Blöcken zugeordnet werden. PFS-Seiten sind dann enthalten, wenn sie aus einem einheitlichen Block zugeordnet werden.<br /><br /> Mit der Spalte total_pages in der [sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md) -Katalog Sicht können Sie die Anzahl der reservierten Seiten jeder Zuordnungs Einheit im Benutzerobjekt zurückgeben. Die total_pages-Spalte enthält jedoch IAM-Seiten.|  
|internal_object_reserved_page_count|**bigint**|Gesamtzahl der Seiten in gleichartigen Blöcken, die für interne Objekte in der Datei zugeordnet werden. Nicht verwendete Seiten aus einem zugeordneten Block sind in der Gesamtzahl enthalten.<br /><br /> IAM-Seiten sind nicht enthalten, da sie immer aus gemischten Blöcken zugeordnet werden. PFS-Seiten sind dann enthalten, wenn sie aus einem einheitlichen Block zugeordnet werden.<br /><br /> Es ist keine Katalogsicht bzw. kein dynamisches Verwaltungsobjekt vorhanden, die bzw. das die Seitenanzahl für jedes interne Objekt zurückgibt.|  
|mixed_extent_page_count|**bigint**|Gesamtzahl der zugeordneten und nicht zugeordneten Seiten in zugeordneten gemischten Blöcken in der Datei. Gemischte Blöcke enthalten Seiten, die verschiedenen Objekten zugeordnet werden. Diese Gesamtzahl enthält alle IAM-Seiten in der Datei.|
|modified_extent_page_count|**bigint**|**Gilt für**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und höher.<br /><br />Gesamtanzahl der Seiten, die seit der letzten vollständigen Datenbanksicherung in den zugeordneten Blöcken der Datei geändert wurden. Mithilfe der geänderten Seitenanzahl kann die Menge der differenziellen Änderungen in der Datenbank seit der letzten vollständigen Sicherung nachverfolgt werden, um zu entscheiden, ob eine differenzielle Sicherung erforderlich ist.|
|pdw_node_id|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
|distribution_id|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Die eindeutige, der Verteilung zugeordnete numerische ID.|  
  
## <a name="remarks"></a>Bemerkungen  
 Die Anzahl von Seiten wird immer auf Blockebene angegeben. Deshalb sind die Seitenanzahlen immer ein Vielfaches von acht (8). Die Blöcke, die GAM-Zuordnungsseiten (Global Allocation Map) und SGAM-Zuordnungsseiten (Shared Global Allocation Map) enthalten, werden gleichartigen Blöcken zugeordnet. Sie sind nicht in den zuvor beschriebenen Seitenanzahlen enthalten. Weitere Informationen zu Seiten und Blöcken finden Sie im Handbuch für die [Architektur von Seiten und Blöcken](../../relational-databases/pages-and-extents-architecture-guide.md). 
  
 Der Inhalt des aktuellen Versionsspeicher befindet sich in [sys.dm_tran_version_store](../../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-transact-sql.md). Versionsspeicherseiten werden auf der Dateiebene anstelle der Sitzungs- und Taskebene nachverfolgt, da es sich bei ihnen um globale Ressourcen handelt. Eine Sitzung kann Versionen generieren, doch können die Versionen nach dem Sitzungsende nicht entfernt werden. Beim Cleanup des Versionsspeichers muss die am längsten ausgeführte Transaktion, die Zugriff auf die bestimmte Version benötigt, berücksichtigt werden. Die am längsten laufende Transaktion im Zusammenhang mit der Bereinigung des Versionsspeicher kann durch Anzeigen der Spalte elapsed_time_seconds in [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md)ermittelt werden.  
  
 Häufige Änderungen in der mixed_extent_page_count-Spalte können auf die umfangreiche Verwendung von SGAM-Seiten hinweisen. In diesem Fall sind zahlreiche PAGELATCH_UP-Wartevorgänge enthalten, bei denen die Warteressource eine SGAM-Seite ist. Weitere Informationen finden Sie unter [sys.dm_os_waiting_tasks &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql.md), [sys.dm_os_wait_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)und [sys.dm_os_latch_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md).  
  
## <a name="user-objects"></a>Benutzerobjekte  
 Die folgenden Objekte sind in den Seitenzählern für Benutzerobjekte enthalten:  
  
-   Benutzerdefinierte Tabellen und Indizes  
  
-   Systemtabellen und -indizes  
  
-   Globale temporäre Tabellen und Indizes  
  
-   Lokale temporäre Tabellen und Indizes  
  
-   Tabellenvariablen  
  
-   In Tabellenwertfunktionen zurückgegebene Tabellen  
  
## <a name="internal-objects"></a>Interne Objekte  
 Interne Objekte gibt es nur in tempdb. Die folgenden Objekte sind in den Seitenzählern für interne Objekte enthalten:  
  
-   Arbeitstabellen für Cursor- oder Spoolvorgänge und temporären LOB-Speicher (Large Object)  
  
-   Arbeitsdateien für Vorgänge wie z. B. Hashjoins  
  
-   Sortierläufe  
  
## <a name="relationship-cardinalities"></a>Kardinalität der Beziehungen  
  
|From|Beschreibung|Relationship|  
|----------|--------|------------------|  
|sys.dm_db_file_space_usage.database_id, file_id|sys.dm_io_virtual_file_stats.database_id, file_id|1:1|  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   

## <a name="examples"></a>Beispiele  
  
### <a name="determing-the-amount-of-free-space-in-tempdb"></a>Ermitteln des in tempdb verfügbaren freien Speicherplatzes  
 Die folgende Abfrage gibt die Gesamtanzahl der freien Seiten und den freien Gesamt Speicherplatz in Megabyte (MB) zurück, die in allen Datendateien in **tempdb** verfügbar sind.  
  
```sql
USE tempdb;  
GO  
SELECT SUM(unallocated_extent_page_count) AS [free pages],   
(SUM(unallocated_extent_page_count)*1.0/128) AS [free space in MB]  
FROM sys.dm_db_file_space_usage;  
```  

### <a name="determining-the-amount-of-space-used-by-user-objects"></a>Ermitteln des von Benutzerobjekten belegten Speicherplatzes  
 Die folgende Abfrage gibt die Gesamtzahl der von den Benutzerobjekten in tempdb verwendeten Seiten sowie den von den Benutzerobjekten belegten Gesamtspeicherplatz (in MB) zurück.  
  
```sql  
USE tempdb;  
GO  
SELECT SUM(user_object_reserved_page_count) AS [user object pages used],  
(SUM(user_object_reserved_page_count)*1.0/128) AS [user object space in MB]  
FROM sys.dm_db_file_space_usage;
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten im Zusammenhang mit der Datenbank &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_db_task_space_usage &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md)   
 [sys.dm_db_session_space_usage &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)  
