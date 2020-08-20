---
description: sys.dm_resource_governor_workload_groups_history_ex (Azure SQL-Datenbank)
title: sys. dm_resource_governor_workload_groups_history_ex (Azure SQL-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_resource_governor_workload_groups_history_ex_TSQL
- sys.dm_resource_governor_workload_groups_history_ex
- dm_resource_governor_workload_groups_history_ex
- dm_resource_governor_workload_groups_history_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_resource_governor_workload_groups_history_ex dynamic management view
author: joesackmsft
ms.author: josack
ms.openlocfilehash: eee98e604fad74b58db9acd5e1f5b20a0189f216
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88489742"
---
# <a name="sysdm_resource_governor_workload_groups_history_ex-azure-sql-database"></a>sys.dm_resource_governor_workload_groups_history_ex (Azure SQL-Datenbank)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Gibt eine Momentaufnahme für die letzten 32 Minuten (insgesamt 128 Sekunden) der Ressourcenpool Statistik für eine Azure SQL-Datenbank zurück.
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**pool_id**| INT |ID des Ressourcenpools. Lässt keine NULL-Werte zu.|
|**group_id**| INT |ID der Arbeitsauslastungsgruppe. Lässt keine NULL-Werte zu.|
|**name**| nvarchar(256) |Name der Arbeitsauslastungsgruppe. Lässt keine NULL-Werte zu.|
|**snapshot_time**| datetime |DateTime der erstellten Ressourcengruppen-Statistik Momentaufnahme.|
|**duration_ms**| INT |Dauer zwischen aktueller und vorheriger Momentaufnahme.|
|**active_worker_count**| INT |Gesamtanzahl der Worker in der aktuellen Momentaufnahme.|
|**active_request_count**| INT |Die aktuelle Anforderungsanzahl. Lässt keine NULL-Werte zu.|
|**active_session_count**| INT |Gesamtanzahl aktiver Sitzungen in der aktuellen Momentaufnahme.|
|**total_request_count**| BIGINT |Kumulierte Anzahl vervollständigter Anforderungen in der Arbeitsauslastungsgruppe. Lässt keine NULL-Werte zu.|
|**delta_request_count**| INT |Anzahl der abgeschlossenen Anforderungen in der Arbeits Auslastungs Gruppe seit der letzten Momentaufnahme. Lässt keine NULL-Werte zu.|
|**total_cpu_usage_ms**| BIGINT |Kumulierte CPU-Verwendung dieser Arbeitsauslastungsgruppe in Millisekunden. Lässt keine NULL-Werte zu.|
|**delta_cpu_usage_ms**| INT |CPU-Auslastung in Millisekunden seit der letzten Momentaufnahme. Lässt keine NULL-Werte zu.|
|**delta_cpu_usage_preemptive_ms**| INT |Präemptiv Win32-Aufrufe, die seit der letzten Momentaufnahme nicht von der SQL-CPU-RG-|
|**delta_reads_reduced_memgrant_count**| INT |Die Anzahl der Arbeitsspeicher Zuweisungen, die den maximalen Grenzwert für die Abfrage Größe seit der letzten Momentaufnahme erreicht haben. Lässt keine NULL-Werte zu.|
|**reads_throttled**| INT |Die Gesamtanzahl der gelesenen Lesevorgänge.|
|**delta_reads_queued**| INT |Die Gesamtanzahl der gelesenen e/As seit der letzten Momentaufnahme. Lässt NULL-Werte zu. NULL, wenn die Ressourcengruppe nicht für e/a-Vorgänge bestimmt ist.|
|**delta_reads_issued**| INT |Der seit der letzten Momentaufnahme ausgegebene Lesevorgang insgesamt. Lässt NULL-Werte zu. NULL, wenn die Ressourcengruppe nicht für e/a-Vorgänge bestimmt ist.|
|**delta_reads_completed**| INT |Die Gesamtanzahl der gelesenen IOS-Lesevorgänge seit der letzten Lässt keine NULL-Werte zu.|
|**delta_read_bytes**| BIGINT |Die Gesamtanzahl der seit der letzten Momentaufnahme gelesenen Bytes. Lässt keine NULL-Werte zu.|
|**delta_read_stall_ms**| INT |Gesamtzeit (in Millisekunden) zwischen Lese-e/a-Empfang und-Abschluss seit der letzten Momentaufnahme. Lässt keine NULL-Werte zu.|
|**delta_read_stall_queued_ms**| INT |Gesamtzeit (in Millisekunden) zwischen Lese-e/a-Empfang und Problem seit der letzten Momentaufnahme. Lässt NULL-Werte zu. NULL, wenn die Ressourcengruppe nicht für e/a-Vorgänge bestimmt ist. Delta_read_stall_queued_ms bedeutet, dass die e/a von der RG betroffen ist.|
|**delta_writes_queued**| INT |Die Gesamtanzahl der in die Warteschlange eingereihten e/a-Vorgänge seit Lässt NULL-Werte zu. NULL, wenn die Ressourcengruppe nicht für e/a-Vorgänge bestimmt ist.|
|**delta_writes_issued**| INT |Die seit der letzten Momentaufnahme ausgegebene IOS-Summe. Lässt NULL-Werte zu. NULL, wenn die Ressourcengruppe nicht für e/a-Vorgänge bestimmt ist.|
|**delta_writes_completed**| INT |Die Summe der seit der letzten Momentaufnahme abgeschlossenen IOS-Schreibvorgänge. Lässt keine NULL-Werte zu.|
|**delta_writes_bytes**| BIGINT |Die Gesamtanzahl von Bytes, die seit der letzten Momentaufnahme geschrieben wurden. Lässt keine NULL-Werte zu.|
|**delta_write_stall_ms**| INT |Gesamtzeit (in Millisekunden) zwischen Schreib-e/a-Eingang und-Abschluss seit der letzten Momentaufnahme. Lässt keine NULL-Werte zu.|
|**delta_background_writes**| INT |Die Gesamtanzahl der Schreibvorgänge, die seit der letzten Momentaufnahme von Hintergrundaufgaben|
|**delta_background_write_bytes**| BIGINT |Die Gesamt Schreibgröße, die seit der letzten Momentaufnahme von Hintergrundaufgaben ausgeführt wurde (in Bytes).|
|**delta_log_bytes_used**| BIGINT |Das Protokoll wurde seit der letzten Momentaufnahme in Bytes verwendet.|
|**delta_log_temp_db_bytes_used**| BIGINT |Das tempdb-Protokoll wurde seit der letzten Momentaufnahme in Bytes verwendet.|
|**delta_query_optimizations**| BIGINT |Gibt die Anzahl von Abfrage Optimierungen in dieser Arbeits Auslastungs Gruppe seit der letzten Momentaufnahme an. Lässt keine NULL-Werte zu.|
|**delta_suboptimal_plan_generations**| BIGINT |Die Anzahl der suboptimalen Plan Generierungen, die in dieser Arbeits Auslastungs Gruppe aufgrund von Speicherauslastung seit der letzten Momentaufnahme aufgetreten sind. Lässt keine NULL-Werte zu.
|**max_memory_grant_kb**| BIGINT |Maximale Arbeitsspeicher Zuweisung für die Gruppe in KB.|
|**max_request_cpu_msec**| BIGINT |Maximale CPU-Nutzung für eine einzelne Anforderung in Millisekunden. Lässt keine NULL-Werte zu.|
|**max_concurrent_request**| INT |Aktuelle Einstellung für die maximale Anzahl gleichzeitiger Anforderungen. Lässt keine NULL-Werte zu.|
|**max_io**| INT |Maximale e/a-Beschränkung für die Gruppe.|
|**max_global_io**| INT |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.
|**max_queued_io**| INT |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.|
|**max_log_rate_kb**| BIGINT |Maximale Protokoll Rate (Kilobytes pro Sekunde) auf Ressourcengruppen Ebene.|
|**max_session**| INT |Sitzungs Limit für die Gruppe.|
|**max_worker**| INT |Workerlimit für die Gruppe.|
|||

## <a name="permissions"></a>Berechtigungen

Diese Sicht erfordert die View Server State-Berechtigung.

## <a name="remarks"></a>Bemerkungen

Benutzer können auf diese dynamische Verwaltungs Sicht zugreifen, um den Ressourcenverbrauch nahezu in Echtzeit für den benutzerworkloadpool sowie systeminterne Pools der Azure SQL-Daten Bank Instanz zu überwachen.

> [!IMPORTANT]
> Die meisten Daten, die von dieser DMV festgestellt werden, sind für den internen Gebrauch vorgesehen und können geändert werden.

## <a name="examples"></a>Beispiele

Im folgenden Beispiel werden die maximalen Protokoll Raten Daten und der Verbrauch bei den einzelnen Momentaufnahmen nach Benutzer Pool zurückgegeben:

```sql
SELECT snapshot_time,
       name,
       max_log_rate_kb,
       delta_log_bytes_used
FROM sys.dm_resource_governor_workload_groups_history_ex
WHERE name LIKE 'User%'
ORDER BY snapshot_time DESC;
```

## <a name="see-also"></a>Siehe auch

- [Governanceprotokoll Raten-Governance](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server#transaction-log-rate-governance)
- [Pool für elastische Datenbanken: DTU-Ressourcenlimits](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools)
- [Pool für elastische Datenbanken: V-Kern-Ressourcenlimits](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)
