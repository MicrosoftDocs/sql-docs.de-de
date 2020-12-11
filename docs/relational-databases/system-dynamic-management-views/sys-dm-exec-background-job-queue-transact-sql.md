---
description: sys.dm_exec_background_job_queue (Transact-SQL)
title: sys.dm_exec_background_job_queue (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_background_job_queue
- sys.dm_exec_background_job_queue_TSQL
- dm_exec_background_job_queue_TSQL
- sys.dm_exec_background_job_queue
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_background_job_queue dynamic management function
ms.assetid: 05d9884f-b74c-4e3c-a23b-c90c1ea5ef02
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b60bed7d0e0126b8ac765c2fc910d75da6475a01
ms.sourcegitcommit: 2991ad5324601c8618739915aec9b184a8a49c74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "97334567"
---
# <a name="sysdm_exec_background_job_queue-transact-sql"></a>sys.dm_exec_background_job_queue (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile für jeden Abfrageprozessorauftrag zurück, der für die asynchrone Ausführung (im Hintergrund) geplant ist.  
  
> **HINWEIS!** Um dies von oder aus aufzurufen **[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]** **[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]** , verwenden Sie den Namen **sys.dm_pdw_nodes_exec_background_job_queue**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**time_queued**|**datetime**|Zeitpunkt, zu dem der Auftrag der Warteschlange hinzugefügt wurde.|  
|**job_id**|**int**|Auftragsbezeichner.|  
|**database_id**|**int**|Datenbank, für die der Auftrag ausgeführt werden soll.|  
|**object_id1**|**int**|Wert hängt vom Auftragstyp ab. Weitere Informationen finden Sie im Abschnitt "Hinweise".|  
|**object_id2**|**int**|Wert hängt vom Auftragstyp ab. Weitere Informationen finden Sie im Abschnitt "Hinweise".|  
|**object_id3**|**int**|Wert hängt vom Auftragstyp ab. Weitere Informationen finden Sie im Abschnitt "Hinweise".|  
|**object_id4**|**int**|Wert hängt vom Auftragstyp ab. Weitere Informationen finden Sie im Abschnitt "Hinweise".|  
|**error_code**|**int**|Fehlercode, wenn der Auftrag aufgrund eines Fehlers wieder eingefügt wurde. NULL, wenn der Auftrag angehalten, nicht entnommen oder abgeschlossen wurde.|  
|**request_type**|**smallint**|Typ der Auftragsanforderung.|  
|**retry_count**|**smallint**|Häufigkeit, mit der der Auftrag aufgrund mangelnder Ressourcen oder sonstiger Gründe aus der Warteschlange entnommen und wieder eingefügt wurde.|  
|**in_progress**|**smallint**|Gibt an, ob der Auftrag mit der Ausführung begonnen hat.<br /><br /> 1 = Gestartet<br /><br /> 0 = Wartet|  
|**session_id**|**smallint**|Sitzungsbezeichner.|  
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken `Server admin` oder ein `Azure Active Directory admin` Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
  
## <a name="remarks"></a>Bemerkungen  
 Diese Sicht gibt nur Informationen für Aufträge zum asynchronen Aktualisieren von Statistiken zurück. Weitere Informationen zu asynchronen Update Statistiken finden Sie unter [Statistics](../../relational-databases/statistics/statistics.md).  
  
 Die Werte **object_id1** , die über **object_id4** sind, hängen vom Typ der Auftrags Anforderung ab. In der folgenden Tabelle wird die Bedeutung dieser Spalten für die verschiedenen Auftragstypen zusammengefasst.  
  
|Anforderungstyp|object_id1|object_id2|object_id3|object_id4|  
|------------------|-----------------|-----------------|-----------------|-----------------|  
|Asynchrones Statistikupdate|Tabellen- oder Sicht-ID|Statistik-ID|Nicht verwendet|Nicht verwendet|  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die Anzahl der aktiven asynchronen Aufträge in der Hintergrundwarteschlange für die einzelnen Datenbanken in der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zurückgegeben.  
  
```  
SELECT DB_NAME(database_id) AS [Database], COUNT(*) AS [Active Async Jobs]  
FROM sys.dm_exec_background_job_queue  
WHERE in_progress = 1  
GROUP BY database_id;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten und-Funktionen im Zusammenhang mit der Ausführung &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Statistiken](../../relational-databases/statistics/statistics.md)   
 [KILL STATS JOB &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-stats-job-transact-sql.md)  
  
  



