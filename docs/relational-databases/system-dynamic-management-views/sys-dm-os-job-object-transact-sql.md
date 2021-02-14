---
description: sys.dm_os_job_object (Azure SQL-Datenbank)
title: sys.dm_os_job_object (Azure SQL-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/03/2020
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.dm_db_resource_stats
- sys.dm_db_resource_stats_TSQL
- dm_db_resource_stats
- dm_db_resource_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_resource_stats
- dm_db_resource_stats
ms.assetid: 6e76b39f-236e-4bbf-b0b5-38be190d81e8
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: c9dbbe97b010e5fb47dd58f60f5863a63a5a36fd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342144"
---
# <a name="sysdm_os_job_object-azure-sql-database"></a>sys.dm_os_job_object (Azure SQL-Datenbank)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Gibt eine einzelne Zeile zurück, die die Konfiguration des Auftrags Objekts beschreibt, das den SQL Server Prozess verwaltet, sowie bestimmte Statistiken zum Ressourcenverbrauch auf Auftrags Objektebene. Gibt eine leere Menge zurück, wenn SQL Server nicht in einem Job-Objekt ausgeführt wird.

Ein Auftrags Objekt ist ein Windows-Konstrukt, das CPU-, Arbeitsspeicher-und e/A-Ressourcenkontrolle auf Betriebssystemebene implementiert. Weitere Informationen zu Auftrags Objekten finden Sie unter [Auftrags Objekte](/windows/desktop/ProcThread/job-objects).
  
|Spalten|Datentyp|Beschreibung|  
|-------------|---------------|-----------------|  
|cpu_rate|**int**|Gibt den Teil der Prozessor Zyklen an, den SQL Server Threads während jedes Zeit Planungs Intervalls verwenden können. Der Wert wird als Prozentsatz der verfügbaren Zyklen innerhalb eines 10000-Zyklus-Planungs Intervalls, multipliziert mit der Anzahl logischer CPUs, angezeigt. Beispielsweise bedeutet der Wert 800 für eine SQL Server Instanz mit 8 logischen CPUs, dass die Verwendung von CPUs die volle Kapazität von Threads verwenden kann.|
|cpu_affinity_mask|**bigint**|Eine Bitmaske, die beschreibt, welche logischen Prozessoren der SQL Server Prozess innerhalb der Prozessor Gruppe verwenden kann. Beispielsweise bedeutet cpu_affinity_mask 255 (1111 1111 in Binary), dass die ersten acht logischen Prozessoren verwendet werden können. <br /><br />Diese Spalte wird aus Gründen der Abwärtskompatibilität bereitgestellt. Die Prozessor Gruppe wird nicht gemeldet, und der gemeldete Wert ist möglicherweise falsch, wenn eine Prozessor Gruppe mehr als 64 logische Prozessoren enthält. Verwenden Sie die- `process_physical_affinity` Spalte, um stattdessen die Prozessor Affinität zu bestimmen.|
|cpu_affinity_group|**int**|Die Nummer der Prozessor Gruppe, die von SQL Server verwendet wird.|
|memory_limit_mb|**bigint**|Die maximale Menge an zugespeichertem Speicher (in MB), die alle Prozesse im Auftrags Objekt, einschließlich SQL Server, kumulativ verwenden können.| 
|process_memory_limit_mb |**bigint**|Die maximale Menge an zugespeichertem Speicher (in MB), die ein einzelner Prozess im Auftrags Objekt (z. b. SQL Server) verwenden kann.|
|workingset_limit_mb |**bigint**|Die maximale Arbeitsspeicher Menge in MB, die der SQL Server Workingset verwenden kann.|
|non_sos_mem_gap_mb|**bigint**|Die Größe des Arbeitsspeichers in MB, der für Thread Stapel, DLLs und andere nicht-SOS-Speicher Belegungen reserviert ist. SOS-Ziel Arbeitsspeicher ist der Unterschied zwischen `process_memory_limit_mb` und `non_sos_mem_gap_mb` .| 
|low_mem_signal_threshold_mb|**bigint**|Ein Arbeitsspeicher Schwellenwert (in MB). Wenn die Menge an verfügbarem Arbeitsspeicher für das Auftrags Objekt unter diesem Schwellenwert liegt, wird ein Benachrichtigungs Signal mit geringem Arbeitsspeicher an den SQL Server Prozess gesendet. |
|total_user_time|**bigint**|Die Gesamtanzahl der 100 ns-Ticks, die Threads innerhalb des Auftrags Objekts im Benutzermodus aufgewendet haben, seit das Auftrags Objekt erstellt wurde. |
|total_kernel_time |**bigint**|Die Gesamtanzahl der 100 ns-Ticks, die Threads innerhalb des Auftrags Objekts im Kernel Modus aufgewendet haben, seit das Auftrags Objekt erstellt wurde. |
|write_operation_count |**bigint**|Die Gesamtanzahl der Schreibvorgänge auf lokalen Datenträgern, die von SQL Server seit der Erstellung des Auftrags Objekts ausgegeben wurden. |
|read_operation_count |**bigint**|Die Gesamtanzahl der Lese-e/a-Vorgänge auf lokalen Datenträgern, die von SQL Server seit der Erstellung des Auftrags Objekts ausgegeben wurden. |
|peak_process_memory_used_mb|**bigint**|Die maximale Arbeitsspeicher Menge in MB, die ein einzelner Prozess im Auftrags Objekt, wie z. b. SQL Server, seit der Erstellung des Auftrags Objekts verwendet hat.| 
|peak_job_memory_used_mb|**bigint**|Die Höchstmenge an Arbeitsspeicher in MB, die alle Prozesse im Auftrags Objekt kumulativ seit der Erstellung des Auftrags Objekts verwendet haben.|
|process_physical_affinity|**nvarchar (3072)**|Bitmasken, die beschreiben, welche logischen Prozessoren der SQL Server Prozess in den einzelnen Prozessor Gruppen verwenden kann. Der Wert in dieser Spalte wird durch ein oder mehrere Wertpaare gebildet, die jeweils in geschweiften Klammern eingeschlossen sind. In jedem Paar ist der erste Wert die Prozessor Gruppennummer, und der zweite Wert ist die Affinitäts Bitmaske für diese Prozessor Gruppe. Der-Wert bedeutet beispielsweise `{{0,a}{1,2}}` , dass die Affinitäts Maske für die Prozessor Gruppe `0` `a` ( `1010` in Binär Weise, die angibt, dass die Prozessoren 2 und 4 verwendet werden) und die Affinitäts Maske für die Prozessor Gruppe `1` (in Binär Weise, die `2` `10` angibt, dass Prozessor 2 verwendet wird) ist.|
  
## <a name="permissions"></a>Berechtigungen  
In SQL verwaltete Instanz ist die- `VIEW SERVER STATE` Berechtigung erforderlich. In der SQL-Datenbank ist die `VIEW DATABASE STATE`-Berechtigung für die Datenbank erforderlich.  
 
## <a name="see-also"></a>Weitere Informationen  

Informationen zu verwalteten Instanzen finden Sie unter [SQL verwaltete Instanz](/azure/sql-database/sql-database-managed-instance).
