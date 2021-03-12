---
description: sys.dm_exec_external_work (Transact-SQL)
title: sys.dm_exec_external_work (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/10/2021
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DM_EXEC_EXTERNAL_WORK
- DM_EXEC_EXTERNAL_WORK_TSQL
- SYS.DM_EXEC_EXTERNAL_WORK_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_external_work management view
- dm_exec_external_work management view
- PolyBase,views
- PolyBase
ms.assetid: 7597d97b-1fde-4135-ac35-4af12968f300
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba2068fa58bdf711deecf90612ee4bf1e47d374b
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622882"
---
# <a name="sysdm_exec_external_work-transact-sql"></a>sys.dm_exec_external_work (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

Gibt Informationen über die Arbeitsauslastung pro Worker auf jedem Computeknoten zurück.  
  
Abfrage `sys.dm_exec_external_work` zur Identifizierung der für die Kommunikation mit der externen Datenquelle aufgedrehten Arbeit (z. b. Hadoop oder MongoDB).  
  
|Spaltenname|Datentyp|BESCHREIBUNG|Range|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|`nvarchar(32)`|Eindeutiger Bezeichner für zugeordnete polybase-Abfrage.|Weitere Informationen finden Sie unter *request_ID* in [sys.dm_exec_requests &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|step_index|`int`|Die Anforderung, die dieser Worker ausführt.|Weitere Informationen finden Sie unter *step_index* in  [sys.dm_exec_requests &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|dms_step_index|`int`|Schritt in den DMS-Plan, der von diesem Worker ausgeführt wird.|Weitere Informationen finden Sie unter [sys.dm_exec_dms_workers &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-workers-transact-sql.md).|  
|compute_node_id|`int`|Der Knoten, auf dem der Worker ausgeführt wird.|Weitere Informationen finden Sie unter [sys.dm_exec_compute_nodes &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).|  
|type|`nvarchar(60)`|Der Typ der externen Arbeit.|' Datei Teilung ' (für Hadoop und Azure Storage)<br/><br/>"ODBC-Daten Aufteilung" (für andere externe Datenquellen) |  
|work_id|`int`|ID der tatsächlichen Teilung.|Größer oder gleich 0 (null).|  
|input_name|`nvarchar(4000)`|Name der zu lesenden Eingabe|Dateiname (mit Pfad), wenn Hadoop oder Azure Storage verwendet wird. Bei anderen externen Datenquellen ist dies die Verkettung des Standorts der externen Datenquelle und des Speicherort der externen Tabelle: `scheme://DataSourceHostname[:port]/[DatabaseName.][SchemaName.]TableName`|  
|read_location|`bigint`|Offset des Lese Speicher Orts.| `0` Gibt die Anzahl der Bytes in der Datei minus 1 an.<br/><br/>`NULL` für nicht-Hadoop-oder nicht-Azure-Speicher. |  
|read_command|`nvarchar(4000)`|Die Abfrage, die an die externe Datenquelle gesendet wird. In [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] eingeführt.|Text, der die Abfrage darstellt. Für Hadoop und Azure Storage gibt zurück `NULL` .|
|bytes_processed|`bigint`|Gesamtanzahl der für die Verarbeitung von Daten durch diesen Worker zugeordneten Bytes. Dieser Wert kann nicht notwendigerweise die Gesamtmenge der Daten darstellen, die von der Abfrage zurückgegeben werden. |Größer oder gleich 0 (null).|  
|length|`bigint`|Länge des Split-oder HDFS-Blocks für Hadoop|Benutzerdefinierbar. Der Standardwert ist 64M.|  
|status|`nvarchar(32)`|Status des Workers|Ausstehend, verarbeitet, abgeschlossen, fehlgeschlagen, abgebrochen|  
|start_time|`datetime`|Beginn der Arbeit||  
|end_time|`datetime`|Ende der Arbeit||  
|total_elapsed_time|`int`|Gesamtzeit in Millisekunden||
|compute_pool_id|`int`|Eindeutiger Bezeichner für den Pool, in dem der Worker ausgeführt wird. Gilt nur für SQL Server Big Data-Cluster. Weitere Informationen finden Sie unter [sys.dm_exec_compute_pools (Transact-SQL)](sys-dm-exec-compute-pools.md).|Gibt `0` für [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] unter Windows und Linux zurück.|

## <a name="see-also"></a>Weitere Informationen  
 [Problembehandlung bei polybase mit dynamischen Verwaltungs Sichten](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten im Zusammenhang mit der Datenbank &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
