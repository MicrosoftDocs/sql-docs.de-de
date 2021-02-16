---
description: sys.dm_db_missing_index_group_stats_query (Transact-SQL)
title: sys.dm_db_missing_index_group_stats_query (Transact-SQL)
ms.custom: ''
ms.date: 02/10/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_missing_index_group_stats_query_TSQL
- sys.dm_db_missing_index_group_stats_query
- dm_db_missing_index_group_stats_query_TSQL
- dm_db_missing_index_group_stats_query
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_group_stats_query dynamic management view
- missing indexes feature [SQL Server], sys.dm_db_missing_index_group_stats_query dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current
ms.openlocfilehash: f54143b965847c83cd07ee07543873fcd7359900
ms.sourcegitcommit: c6cc0b669b175ae290cf5b08952010661ebd03c3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2021
ms.locfileid: "100531048"
---
# <a name="sysdm_db_missing_index_group_stats_query-transact-sql"></a>sys.dm_db_missing_index_group_stats_query (Transact-SQL)
[!INCLUDE [SQL Server 2019 SQL Database](../../includes/applies-to-version/sqlserver2019-asdb-asdbmi.md)]

  Gibt Informationen zu Abfragen zurück, die einen fehlenden Index aus Gruppen fehlender Indizes benötigt haben, ausgenommen räumliche Indizes. Pro fehlende Indexgruppe kann mehr als eine Abfrage zurückgegeben werden. Eine fehlende Indexgruppe kann über mehrere Abfragen verfügen, die denselben Index benötigen.
  
 In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]können dynamische Verwaltungssichten keine Informationen verfügbar machen, die sich auf die Datenbankkapselung auswirken würden oder die sich auf andere Datenbanken beziehen, auf die der Benutzer Zugriff hat. Um zu vermeiden, dass diese Informationen verfügbar gemacht werden, wird jede Zeile, die Daten enthält, die nicht zum verbundenen Mandanten gehören, herausgefiltert.  
    
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**group_handle**|**int**|Identifiziert eine Gruppe fehlender Indizes. Dieser Bezeichner ist innerhalb des Servers eindeutig.<br /><br /> Die anderen Spalten stellen Informationen zu allen Abfragen bereit, für die der Index in der Gruppe als fehlend betrachtet wird.<br /><br /> Eine Indexgruppe enthält nur einen Index.<BR><BR>Kann mit **index_group_handle** in [sys.dm_db_missing_index_groups](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)verknüpft werden.|  
|**query_hash**|**Binär (8)**|Binärer Hashwert, der in der Abfrage berechnet wird und zum Identifizieren von Abfragen mit ähnlicher Logik verwendet wird. Sie können den Abfragehash verwenden, um die aggregierte Ressourcennutzung für Abfragen zu ermitteln, die sich nur durch Literalwerte unterscheiden.|  
|**query_plan_hash**|**Binär (8)**|Binärer Hashwert, der im Abfrageausführungsplan wird und zum Identifizieren ähnlicher Abfrageausführungspläne verwendet wird. Sie können diesen Abfrageplan-Hashwert verwenden, um die kumulierten Kosten für Abfragen mit ähnlichen Ausführungsplänen zu suchen.<br /><br /> Ist immer 0x000, wenn eine systemintern kompilierte gespeicherte Prozedur eine speicheroptimierte Tabelle abfragt.|  
|**last_sql_handle**|**varbinary(64)**|Ein Token, das den Batch oder die gespeicherte Prozedur der zuletzt kompilierten Anweisung, die diesen Index benötigte, eindeutig identifiziert.<BR><BR>**last_sql_handle** kann verwendet werden, um den SQL-Text der Abfrage abzurufen, indem die dynamische Verwaltungsfunktion [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) aufgerufen wird.|
|**last_statement_start_offset**|**int**|Gibt die Anfangsposition der Abfrage, die die Zeile innerhalb des Texts des Batches oder des persistenten Objekts für die letzte kompilierte Anweisung, die diesen Index benötigt, im SQL-Batch in Bytes an.|
|**last_statement_end_offset**|**int**|Gibt die Endposition der Abfrage, die die Zeile beschreibt, innerhalb des Texts des Batches oder des persistenten Objekts für die zuletzt kompilierte Anweisung, die diesen Index benötigt, im SQL-Batch in Bytes an.|
|**last_statement_sql_handle**|**varbinary(64)**|Ein Token, das den Batch oder die gespeicherte Prozedur der zuletzt kompilierten Anweisung, die diesen Index benötigte, eindeutig identifiziert.<BR><BR>**last_statement_sql_handle** können mit **last_statement_start_offset** und **last_statement_end_offset** zum Abrufen des SQL-Texts der Abfrage verwendet werden, indem die dynamische Verwaltungsfunktion [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) aufgerufen wird.<BR><BR>Wenn Abfragespeicher bei der Kompilierung der Abfrage nicht aktiviert war, wird 0 zurückgegeben.|
|**user_seeks**|**bigint**|Die Anzahl von durch Benutzerabfragen verursachten Suchvorgängen, für die der empfohlene Index in der Gruppe hätte verwendet werden können.|  
|**user_scans**|**bigint**|Die Anzahl von durch Benutzerabfragen verursachten Scanvorgängen, für die der empfohlene Index in der Gruppe hätte verwendet werden können.|  
|**last_user_seek**|**datetime**|Das Datum und die Uhrzeit des letzten durch Benutzerabfragen verursachten Suchvorgangs, für den der empfohlene Index in der Gruppe hätte verwendet werden können.|  
|**last_user_scan**|**datetime**|Das Datum und die Uhrzeit des letzten durch Benutzerabfragen verursachten Scanvorgangs, für den der empfohlene Index in der Gruppe hätte verwendet werden können.|  
|**avg_total_user_cost**|**float**|Die durchschnittlichen Kosten der Benutzerabfragen, die durch den Index in der Gruppe reduziert werden könnten.|  
|**avg_user_impact**|**float**|Durchschnittlicher prozentualer Nutzen, der für Benutzerabfragen entstünde, wenn diese Gruppe fehlender Indizes implementiert würde. Der Wert bedeutet, dass die Abfragekosten durchschnittlich um diesen Prozentsatz verringert würden, wenn diese Gruppe fehlender Indizes implementiert würde.|  
|**system_seeks**|**bigint**|Die Anzahl von durch Systemabfragen, beispielsweise Auto Stats-Abfragen, verursachten Suchvorgängen, für die der empfohlene Index in der Gruppe hätte verwendet werden können. Weitere Informationen finden Sie unter [Auto Stats-Ereignisklasse](../../relational-databases/event-classes/auto-stats-event-class.md).|  
|**system_scans**|**bigint**|Die Anzahl von durch Systemabfragen verursachten Scanvorgängen, für die der empfohlene Index in der Gruppe hätte verwendet werden können.|  
|**last_system_seek**|**datetime**|Das Datum und die Uhrzeit des letzten durch Systemabfragen verursachten Systemsuchvorgangs, für den der empfohlene Index in der Gruppe hätte verwendet werden können.|  
|**last_system_scan**|**datetime**|Das Datum und die Uhrzeit des letzten durch Systemabfragen verursachten Systemscanvorgangs, für den der empfohlene Index in der Gruppe hätte verwendet werden können.|  
|**avg_total_system_cost**|**float**|Die durchschnittlichen Kosten der Systemabfragen, die durch den Index in der Gruppe reduziert werden könnten.|  
|**avg_system_impact**|**float**|Durchschnittlicher prozentualer Nutzen, der für Systemabfragen entstünde, wenn diese Gruppe fehlender Indizes implementiert würde. Der Wert bedeutet, dass die Abfragekosten durchschnittlich um diesen Prozentsatz verringert würden, wenn diese Gruppe fehlender Indizes implementiert würde.|  
  
## <a name="remarks"></a>Bemerkungen  
 Die von **sys.dm_db_missing_index_group_stats_query** zurückgegebenen Informationen werden bei jeder Abfrage Ausführung aktualisiert, nicht bei jeder Abfrage Kompilierung oder Neukompilierung. Statistiken zur Verwendung sind nicht persistent und werden nur bis zum Neustart von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beibehalten. Datenbankadministratoren sollten regelmäßig Sicherungskopien der Informationen zu fehlenden Indizes erstellen, wenn Sie die Verwendungsstatistiken nach dem Wiederverwenden des Servers beibehalten möchten.  
 
  >[!NOTE]
  >Das Resultset für diese DMV ist auf 600 Zeilen beschränkt. Jede Zeile enthält einen fehlenden Index. Wenn Sie über mehr als 600 fehlende Indizes verfügen, sollten Sie die vorhandenen fehlenden Indizes ansprechen, damit Sie die neueren Indizes anzeigen können.

## <a name="permissions"></a>Berechtigungen  
 Zum Abfragen dieser dynamischen Verwaltungssicht muss den Benutzern die VIEW SERVER STATE-Berechtigung oder eine Berechtigung, die die VIEW SERVER STATE-Berechtigung impliziert, erteilt werden.  
  
## <a name="examples"></a>Beispiele  
 In den folgenden Beispielen wird die Verwendung der dynamischen Verwaltungs Sicht **sys.dm_db_missing_index_group_stats_query** veranschaulicht.  
  
  
### <a name="a-find-the-latest-query-text-for-the-top-10-highest-anticipated-improvements-for-user-queries"></a>A. Finden Sie den neuesten Abfragetext für die 10 wichtigsten Verbesserungen für Benutzer Abfragen. 
 Die folgende Abfrage gibt den zuletzt aufgezeichneten Abfragetext für die 10 fehlenden Indizes zurück, die die höchste erwartete kumulative Verbesserung in absteigender Reihenfolge bewirken würden.  

```sql
SELECT TOP 10 
    SUBSTRING
    (
            sql_text.text,
            misq.last_statement_start_offset / 2 + 1,
            (
            CASE misq.last_statement_Start_offset
                WHEN -1 THEN datalength(sql_text.text)
                ELSE misq.last_statement_end_offset
            END - misq.last_statement_start_offset
            ) / 2 + 1
    ),
    misq.*
FROM sys.dm_db_missing_index_group_stats_query AS misq
CROSS APPLY sys.dm_exec_sql_text(last_sql_handle) AS sql_text
ORDER BY avg_total_user_cost * avg_user_impact * (user_seeks + user_scans) DESC; 
```
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.dm_db_missing_index_columns &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
