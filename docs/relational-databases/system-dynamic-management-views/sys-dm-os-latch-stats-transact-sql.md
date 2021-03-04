---
description: sys.dm_os_latch_stats (Transact-SQL)
title: sys.dm_os_latch_stats (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_latch_stats_TSQL
- dm_os_latch_stats_TSQL
- dm_os_latch_stats
- sys.dm_os_latch_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_latch_stats dynamic management view
ms.assetid: 2085d9fc-828c-453e-82ec-b54ed8347ae5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d1a8d34673b28e1da4fb15b0f469740aab378391
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839233"
---
# <a name="sysdm_os_latch_stats-transact-sql"></a>sys.dm_os_latch_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Gibt Informationen zu allen nach Klassen sortierten Latchwartevorgängen zurück. 
  
> [!NOTE]  
> Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_latch_stats**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|latch_class|**nvarchar(60)**|Name der Latchklasse.|  
|waiting_requests_count|**bigint**|Anzahl der Wartevorgänge auf Latches in dieser Klasse. Dieser Leistungsindikator wird beim Starten eines Latchwartevorgangs erhöht.|  
|wait_time_ms|**bigint**|Gesamtwartezeit auf Latches in dieser Klasse (in Millisekunden).<br /><br /> **Hinweis:** Diese Spalte wird alle fünf Minuten während eines latchwartens und am Ende eines Latchwartevorgangs aktualisiert.|  
|max_wait_time_ms|**bigint**|Maximale Zeitdauer, die ein Speicherobjekt auf diesen Latch gewartet hat. Wenn dieser Wert ungewöhnlich hoch ist, kann dies ein Hinweis auf einen internen Deadlock sein.|  
|pdw_node_id|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen  
In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
  
## <a name="remarks"></a>Bemerkungen  
 sys.dm_os_latch_stats kann zum Identifizieren der Quelle von Latchkonflikten verwendet werden, indem die relative Anzahl der Wartevorgänge und der Wartezeiten für die verschiedenen Latchklassen überprüft wird. In einigen Fällen können Sie Latchkonflikte möglicherweise lösen oder reduzieren. Es kann jedoch Situationen geben, in denen Sie sich mit [!INCLUDE[msCoName](../../includes/msconame-md.md)] Support Services in Verbindung setzen müssen.  
  
Sie können den Inhalt sys.dm_os_latch_stats mithilfe von `DBCC SQLPERF` wie folgt zurücksetzen:  
  
```sql  
DBCC SQLPERF ('sys.dm_os_latch_stats', CLEAR);  
GO  
```  
  
 Dadurch werden alle Leistungsindikatoren auf 0 zurückgesetzt.  
  
> [!NOTE]  
>  Diese Statistiken werden nicht persistent gespeichert, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] neu gestartet wird. Alle Daten stellen einen Gesamtwert seit dem letzten Zurücksetzen der Statistiken oder dem Starten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dar.  
  
## <a name="latches"></a><a name="latches"></a> Latches  
 Ein Latch ist ein internes Lightweight-Synchronisierungs Objekt, das einer Sperre ähnlich ist, die von verschiedenen-Komponenten verwendet wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ein Latch wird hauptsächlich zum Synchronisieren von Datenbankseiten bei Vorgängen wie dem Puffer-oder Dateizugriff verwendet. Jedem Latch wird eine einzelne Zuordnungseinheit zugeordnet. 
  
 Ein Latchwartevorgang findet dann statt, wenn der Latch nicht sofort erteilt werden kann, da er von einem anderen Thread in einem in Konflikt stehenden Modus beansprucht wird. Im Gegensatz zu Sperren wird ein Latch unmittelbar nach dem Vorgang freigegeben, selbst bei Schreibvorgängen.  
  
 Latches werden nach Klassen gruppiert, die auf Komponenten und der Verwendung basieren. 0 oder mehr Latches einer bestimmten Klasse können zu jeder Zeit in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz vorhanden sein.  
  
> [!NOTE]  
> `sys.dm_os_latch_stats` verfolgt Latchanforderungen nicht nach, die sofort erteilt werden oder die ohne Warten einen Fehler erzeugen.  
  
 Die folgende Tabelle enthält kurze Beschreibungen der verschiedenen Latchklassen.  
  
|Latchklasse|BESCHREIBUNG|  
|-----------------|-----------------|  
|ALLOC_CREATE_RINGBUF|Wird intern von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zum Initialisieren der Synchronisierung beim Erstellen eines Zuordnungsringpuffers verwendet.|  
|ALLOC_CREATE_FREESPACE_CACHE|Wird zum Initialisieren der Synchronisierung interner Caches für freien Speicher für Heaps verwendet.|  
|ALLOC_CACHE_MANAGER|Wird zum Synchronisieren interner Kohärenztests verwendet.|  
|ALLOC_FREESPACE_CACHE|Wird zum Synchronisieren des Zugriffs auf einen Seitencache mit verfügbarem Speicher für Heaps und BLOBs (Binary Large Objects) verwendet. Konflikte bei Latches dieser Klasse können auftreten, wenn mehrere Verbindungen versuchen, gleichzeitig Zeilen in einen Heap oder ein BLOB einzufügen. Durch Partitionieren des Objekts sinkt das Konfliktrisiko. Jede Partition verfügt über einen eigenen Latch. Durch das Partitionieren werden die Einfügevorgänge auf mehrere Latches verteilt.|  
|ALLOC_EXTENT_CACHE|Wird zum Synchronisieren des Zugriffs auf einen Cache mit Blöcken verwendet, die nicht zugeordnete Seiten enthalten. Konflikte bei Latches dieser Klasse können auftreten, wenn mehrere Verbindungen versuchen, gleichzeitig in derselben Zuordnungseinheit Datenseiten zuzuordnen. Das Konfliktrisiko kann durch Partitionieren des Objekts, zu dem diese Zuordnungseinheit gehört, gesenkt werden.|  
|ACCESS_METHODS_DATASET_PARENT|Wird zum Synchronisieren des Zugriffs des untergeordneten Datasets auf das übergeordnete Dataset während paralleler Vorgänge verwendet.|  
|ACCESS_METHODS_HOBT_FACTORY|Wird zum Synchronisieren des Zugriffs auf eine interne Hashtabelle verwendet.|  
|ACCESS_METHODS_HOBT|Wird zum Synchronisieren des Zugriffs auf die speicherinterne Darstellung eines HoBts verwendet.|  
|ACCESS_METHODS_HOBT_COUNT|Wird zum Synchronisieren des Zugriffs auf HoBt-Seiten- und Zeilenleistungsindikatoren verwendet.|  
|ACCESS_METHODS_HOBT_VIRTUAL_ROOT|Wird zum Synchronisieren des Zugriffs auf die Stammseitenabstraktion einer internen B-Struktur verwendet.|  
|ACCESS_METHODS_CACHE_ONLY_HOBT_ALLOC|Wird zum Synchronisieren des Zugriffs auf Arbeitstabellen verwendet.|  
|ACCESS_METHODS_BULK_ALLOC|Wird zum Synchronisieren des Zugriffs innerhalb von Massenzuordnungen verwendet.|  
|ACCESS_METHODS_SCAN_RANGE_GENERATOR|Wird zum Synchronisieren des Zugriffs auf einen Bereichgenerator während paralleler Scans verwendet.|  
|ACCESS_METHODS_KEY_RANGE_GENERATOR|Wird zum Synchronisieren des Zugriffs auf Read-Ahead-Vorgänge während paralleler Scans in Schlüsselbereichen verwendet.|  
|APPEND_ONLY_STORAGE_INSERT_POINT|Wird zum Synchronisieren von Einfügungen in schnellen Nur anhängen-Speichereinheiten verwendet.|  
|APPEND_ONLY_STORAGE_FIRST_ALLOC|Wird zum Synchronisieren der ersten Zuordnung für eine Nur anhängen-Speichereinheit verwendet.|  
|APPEND_ONLY_STORAGE_UNIT_MANAGER|Wird zum Synchronisieren des Zugriffs auf die interne Datenstruktur innerhalb der Verwaltung schneller Nur anhängen-Speichereinheiten verwendet.|  
|APPEND_ONLY_STORAGE_MANAGER|Wird zum Synchronisieren von Verkleinerungsvorgängen bei der Verwaltung schneller Nur anhängen-Speichereinheiten verwendet.|  
|BACKUP_RESULT_SET|Wird zum Synchronisieren paralleler Sicherungsresultsets verwendet.|  
|BACKUP_TAPE_POOL|Wird zum Synchronisieren von Pools mit Sicherungsbändern verwendet.|  
|BACKUP_LOG_REDO|Wird zum Synchronisieren von Wiederholungsvorgängen für Sicherungsprotokolle verwendet.|  
|BACKUP_INSTANCE_ID|Wird zum Synchronisieren der Generierung von Instanz-IDs für Sicherungsleistungsindikatoren verwendet.|  
|BACKUP_MANAGER|Wird zum Synchronisieren des internen Sicherungs-Managers verwendet.|  
|BACKUP_MANAGER_DIFFERENTIAL|Wird zum Synchronisieren differenzieller Sicherungsvorgänge mit DBCC verwendet.|  
|BACKUP_OPERATION|Wird für die interne Datenstruktursynchronisierung in einem Sicherungsvorgang verwendet, wie z. B. in einer Datenbank-, Protokoll- oder Dateisicherung.|  
|BACKUP_FILE_HANDLE|Wird zum Synchronisieren von Vorgängen zum Öffnen von Dateien während einer Wiederherstellung verwendet.|  
|BUFFER|Wird zum Synchronisieren des kurzfristigen Zugriffs auf Datenbankseiten verwendet. Vor dem Lesen oder Ändern von Datenbankseiten ist ein Pufferlatch erforderlich. Pufferlatchkonflikte können ein Hinweis auf unterschiedliche Probleme sein, darunter Hotpages und langsame E/A-Vorgänge.<br /><br /> Diese Latchklasse umfasst alle möglichen Verwendungen von Seitenlatches. sys.dm_os_wait_stats unterscheidet zwischen Seitenlatch-warte Vorgängen, die durch e/a-Vorgänge verursacht werden, und Lese-und Schreibvorgängen auf der Seite.|  
|BUFFER_POOL_GROW|Wird für die Synchronisierung des internen Puffer-Managers während Erweiterungen des Pufferpools verwendet.|  
|DATABASE_CHECKPOINT|Wird für die Serialisierung von Prüfpunkten in einer Datenbank verwendet.|  
|CLR_PROCEDURE_HASHTABLE|Nur zur internen Verwendung.|  
|CLR_UDX_STORE|Nur zur internen Verwendung.|  
|CLR_DATAT_ACCESS|Nur zur internen Verwendung.|  
|CLR_XVAR_PROXY_LIST|Nur zur internen Verwendung.|  
|DBCC_CHECK_AGGREGATE|Nur zur internen Verwendung.|  
|DBCC_CHECK_RESULTSET|Nur zur internen Verwendung.|  
|DBCC_CHECK_TABLE|Nur zur internen Verwendung.|  
|DBCC_CHECK_TABLE_INIT|Nur zur internen Verwendung.|  
|DBCC_CHECK_TRACE_LIST|Nur zur internen Verwendung.|  
|DBCC_FILE_CHECK_OBJECT|Nur zur internen Verwendung.|  
|DBCC_PERF|Wird zum Synchronisieren interner Leistungsindikatoren verwendet.|  
|DBCC_PFS_STATUS|Nur zur internen Verwendung.|  
|DBCC_OBJECT_METADATA|Nur zur internen Verwendung.|  
|DBCC_HASH_DLL|Nur zur internen Verwendung.|  
|EVENTING_CACHE|Nur zur internen Verwendung.|  
|FCB|Wird zum Synchronisieren des Zugriffs auf den Dateikontrollblock verwendet.|  
|FCB_REPLICA|Nur zur internen Verwendung.|  
|FGCB_ALLOC|Wird zum Synchronisieren des Zugriffs auf Roundrobin-Zuordnungsinformationen in einer Dateigruppe verwendet.|  
|FGCB_ADD_REMOVE|Verwenden Sie, um den Zugriff auf Dateigruppen für Vorgänge zum Hinzufügen, löschen, vergrößern und Verkleinern von Dateien zu synchronisieren.|  
|FILEGROUP_MANAGER|Nur zur internen Verwendung.|  
|FILE_MANAGER|Nur zur internen Verwendung.|  
|FILESTREAM_FCB|Nur zur internen Verwendung.|  
|FILESTREAM_FILE_MANAGER|Nur zur internen Verwendung.|  
|FILESTREAM_GHOST_FILES|Nur zur internen Verwendung.|  
|FILESTREAM_DFS_ROOT|Nur zur internen Verwendung.|  
|LOG_MANAGER|Nur zur internen Verwendung.|  
|FULLTEXT_DOCUMENT_ID|Nur zur internen Verwendung.|  
|FULLTEXT_DOCUMENT_ID_TRANSACTION|Nur zur internen Verwendung.|  
|FULLTEXT_DOCUMENT_ID_NOTIFY|Nur zur internen Verwendung.|  
|FULLTEXT_LOGS|Nur zur internen Verwendung.|  
|FULLTEXT_CRAWL_LOG|Nur zur internen Verwendung.|  
|FULLTEXT_ADMIN|Nur zur internen Verwendung.|  
|FULLTEXT_AMDIN_COMMAND_CACHE|Nur zur internen Verwendung.|  
|FULLTEXT_LANGUAGE_TABLE|Nur zur internen Verwendung.|  
|FULLTEXT_CRAWL_DM_LIST|Nur zur internen Verwendung.|  
|FULLTEXT_CRAWL_CATALOG|Nur zur internen Verwendung.|  
|FULLTEXT_FILE_MANAGER|Nur zur internen Verwendung.|  
|DATABASE_MIRRORING_REDO|Nur zur internen Verwendung.|  
|DATABASE_MIRRORING_SERVER|Nur zur internen Verwendung.|  
|DATABASE_MIRRORING_CONNECTION|Nur zur internen Verwendung.|  
|DATABASE_MIRRORING_STREAM|Nur zur internen Verwendung.|  
|QUERY_OPTIMIZER_VD_MANAGER|Nur zur internen Verwendung.|  
|QUERY_OPTIMIZER_ID_MANAGER|Nur zur internen Verwendung.|  
|QUERY_OPTIMIZER_VIEW_REP|Nur zur internen Verwendung.|  
|RECOVERY_BAD_PAGE_TABLE|Nur zur internen Verwendung.|  
|RECOVERY_MANAGER|Nur zur internen Verwendung.|  
|SECURITY_OPERATION_RULE_TABLE|Nur zur internen Verwendung.|  
|SECURITY_OBJPERM_CACHE|Nur zur internen Verwendung.|  
|SECURITY_CRYPTO|Nur zur internen Verwendung.|  
|SECURITY_KEY_RING|Nur zur internen Verwendung.|  
|SECURITY_KEY_LIST|Nur zur internen Verwendung.|  
|SERVICE_BROKER_CONNECTION_RECEIVE|Nur zur internen Verwendung.|  
|SERVICE_BROKER_TRANSMISSION|Nur zur internen Verwendung.|  
|SERVICE_BROKER_TRANSMISSION_UPDATE|Nur zur internen Verwendung.|  
|SERVICE_BROKER_TRANSMISSION_STATE|Nur zur internen Verwendung.|  
|SERVICE_BROKER_TRANSMISSION_ERRORS|Nur zur internen Verwendung.|  
|SSBXmitWork|Nur zur internen Verwendung.|  
|SERVICE_BROKER_MESSAGE_TRANSMISSION|Nur zur internen Verwendung.|  
|SERVICE_BROKER_MAP_MANAGER|Nur zur internen Verwendung.|  
|SERVICE_BROKER_HOST_NAME|Nur zur internen Verwendung.|  
|SERVICE_BROKER_READ_CACHE|Nur zur internen Verwendung.|  
|SERVICE_BROKER_WAITFOR_MANAGER| Wird verwendet, um eine Zuordnung auf Instanzebene von kellnerwarteschlangen Pro Datenbank-ID, Daten Bank Version und Warteschlangen-ID-Tupel ist eine Warteschlange vorhanden. Konflikte bei Latches dieser Klasse können auftreten, wenn viele Verbindungen bestehen: in einem WAITFOR (Receive)-Wartestatus; Aufrufen von WAITFOR (Receive); Überschreiten des WAITFOR-Timeouts Empfangen einer Nachricht; Commit oder Rollback der Transaktion, die WAITFOR (Receive) enthält; Sie können die Konflikte verringern, indem Sie die Anzahl der Threads in einem WAITFOR-Wartestatus (Receive) verringern. |  
|SERVICE_BROKER_WAITFOR_TRANSACTION_DATA|Nur zur internen Verwendung.|  
|SERVICE_BROKER_TRANSMISSION_TRANSACTION_DATA|Nur zur internen Verwendung.|  
|SERVICE_BROKER_TRANSPORT|Nur zur internen Verwendung.|  
|SERVICE_BROKER_MIRROR_ROUTE|Nur zur internen Verwendung.|  
|TRACE_ID|Nur zur internen Verwendung.|  
|TRACE_AUDIT_ID|Nur zur internen Verwendung.|  
|TRACE|Nur zur internen Verwendung.|  
|TRACE_CONTROLLER|Nur zur internen Verwendung.|  
|TRACE_EVENT_QUEUE|Nur zur internen Verwendung.|  
|TRANSACTION_DISTRIBUTED_MARK|Nur zur internen Verwendung.|  
|TRANSACTION_OUTCOME|Nur zur internen Verwendung.|  
|NESTING_TRANSACTION_READONLY|Nur zur internen Verwendung.|  
|NESTING_TRANSACTION_FULL|Nur zur internen Verwendung.|  
|MSQL_TRANSACTION_MANAGER|Nur zur internen Verwendung.|  
|DATABASE_AUTONAME_MANAGER|Nur zur internen Verwendung.|  
|UTILITY_DYNAMIC_VECTOR|Nur zur internen Verwendung.|  
|UTILITY_SPARSE_BITMAP|Nur zur internen Verwendung.|  
|UTILITY_DATABASE_DROP|Nur zur internen Verwendung.|  
|UTILITY_DYNAMIC_MANAGER_VIEW|Nur zur internen Verwendung.|  
|UTILITY_DEBUG_FILESTREAM|Nur zur internen Verwendung.|  
|UTILITY_LOCK_INFORMATION|Nur zur internen Verwendung.|  
|VERSIONING_TRANSACTION|Nur zur internen Verwendung.|  
|VERSIONING_TRANSACTION_LIST|Nur zur internen Verwendung.|  
|VERSIONING_TRANSACTION_CHAIN|Nur zur internen Verwendung.|  
|VERSIONING_STATE|Nur zur internen Verwendung.|  
|VERSIONING_STATE_CHANGE|Nur zur internen Verwendung.|  
|KTM_VIRTUAL_CLOCK|Nur zur internen Verwendung.|  
  
## <a name="see-also"></a>Weitere Informationen  
[DBCC SQLPERF &#40;Transact-SQL-&#41;](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md)       
[SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)       
[SQL Server, Latches-Objekt](../../relational-databases/performance-monitor/sql-server-latches-object.md)