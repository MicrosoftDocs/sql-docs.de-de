---
description: sys.dm_os_sys_info (Transact-SQL)
title: sys.dm_os_sys_info (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_sys_info_TSQL
- dm_os_sys_info
- dm_os_sys_info_TSQL
- sys.dm_os_sys_info
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_sys_info dynamic management view
- time [SQL Server], instance started
- starting time
ms.assetid: 20f6bc9c-839a-4fa4-b3f3-a6c47d1b69af
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a4f43c899f066713d3a2b4277e3c000c7d5bcb35
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100344344"
---
# <a name="sysdm_os_sys_info-transact-sql"></a>sys.dm_os_sys_info (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt verschiedene nützliche Informationen zum Computer und den Ressourcen zurück, die für [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] zur Verfügung stehen und verwendet werden.  
  
> **Hinweis:** Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_sys_info**.  
  
|Spaltenname|Datentyp|Beschreibung und versionsspezifische Notizen |  
|-----------------|---------------|-----------------|  
|**cpu_ticks**|**bigint**|Gibt die aktuelle CPU-Taktanzahl an. Die CPU-Takte stammen vom RDTSC-Leistungsindikator des Prozessors. Es handelt sich um eine monoton steigende Zahl. Lässt keine NULL-Werte zu.|  
|**ms_ticks**|**bigint**|Gibt die Anzahl der Millisekunden seit dem Starten des Computers an. Lässt keine NULL-Werte zu.|  
|**cpu_count**|**int**|Gibt die Anzahl der logischen CPUs im System an. Lässt keine NULL-Werte zu.|  
|**hyperthread_ratio**|**int**|Gibt das Verhältnis der Anzahl von logischen oder physischen Kernen an, die von einem physischen Prozessorpaket verfügbar gemacht werden. Lässt keine NULL-Werte zu.|  
|**physical_memory_in_bytes**|**bigint**|**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Gibt die Gesamtmenge des physischem Speichers auf dem Computer an. Lässt keine NULL-Werte zu.|  
|**physical_memory_kb**|**bigint**|**Gilt für:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Gibt die Gesamtmenge des physischem Speichers auf dem Computer an. Lässt keine NULL-Werte zu.|  
|**virtual_memory_in_bytes**|**bigint**|**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Umfang des virtuellen Arbeitsspeichers, der dem Prozess im Benutzermodus zur Verfügung steht. Damit kann bestimmt werden, ob SQL Server mithilfe eines 3-GB-Schalters gestartet wurde.|  
|**virtual_memory_kb**|**bigint**|**Gilt für:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Gibt die Gesamtmenge des virtuellem Adressraums für den Prozess im Benutzermodus an. Lässt keine NULL-Werte zu.|  
|**bpool_committed**|**int**|**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Stellt den Arbeitsspeicher im Speicher-Manager in Kilobyte (KB) dar, für den ein Commit ausgeführt wurde. Reservierter Arbeitsspeicher im Speicher-Manager ist nicht eingeschlossen. Lässt keine NULL-Werte zu.|  
|**committed_kb**|**int**|**Gilt für:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Stellt den Arbeitsspeicher im Speicher-Manager in Kilobyte (KB) dar, für den ein Commit ausgeführt wurde. Reservierter Arbeitsspeicher im Speicher-Manager ist nicht eingeschlossen. Lässt keine NULL-Werte zu.|  
|**bpool_commit_target**|**int**|**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Stellt den Arbeitsspeicher in Kilobytes (KB) dar, der von SQL Server-Speicher-Manager genutzt werden kann.|  
|**committed_target_kb**|**int**|**Gilt für:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Stellt den Arbeitsspeicher in Kilobytes (KB) dar, der von SQL Server-Speicher-Manager genutzt werden kann. Die Zielmenge wird anhand einer Vielzahl von Eingaben berechnet, darunter die folgenden:<br /><br /> -der aktuelle Zustand des Systems einschließlich der Auslastung<br /><br /> -der von den aktuellen Prozessen angeforderte Arbeitsspeicher<br /><br /> -die Menge an Arbeitsspeicher, die auf dem Computer installiert ist<br /><br /> -Konfigurationsparameter<br /><br /> Wenn **committed_target_kb** größer als **committed_kb** ist, wird vom Speicher-Manager versucht, zusätzlichen Arbeitsspeicher zu erhalten. Wenn **committed_target_kb** kleiner als **committed_kb** ist, versucht der Speicher-Manager, den Umfang des zugesicherte Arbeitsspeichers zu verringern. Der **committed_target_kb** enthält immer gestohlenen und reservierten Arbeitsspeicher. Lässt keine NULL-Werte zu.|  
|**bpool_visible**|**int**|**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] bis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .<br /><br /> Anzahl von 8-KB-Puffern im Pufferpool, die im virtuellen Prozessadressraum direkt adressierbar sind. Wenn AWE (Address Windowing Extensions) nicht verwendet wird und wenn der Pufferpool sein Arbeitsspeicherziel erreicht hat (bpool_committed = bpool_commit_target), entspricht der Wert von bpool_visible dem Wert von bpool_committed. Wenn AWE in einer 32-Bit-Version von SQL Server verwendet wird, stellt bpool_visible die Größe des AWE-Zuordnungsfensters dar, das verwendet wird, um auf den vom Pufferpool zugewiesenen physischen Speicher zuzugreifen. Da die Größe des Zuordnungsfensters durch den Prozessadressraum gebunden ist, ist der sichtbare Umfang geringer als der zugesicherte Umfang, und er kann durch interne Komponenten weiter reduziert werden, die zu anderen Zwecken als für Datenbankseiten Arbeitsspeicher belegen. Ist der Wert von bpool_visible zu niedrig, treten möglicherweise Fehler aufgrund von nicht genügend Arbeitsspeicher auf.|  
|**visible_target_kb**|**int**|**Gilt für:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.<br /><br /> Ist identisch mit **committed_target_kb**. Lässt keine NULL-Werte zu.|  
|**stack_size_in_bytes**|**int**|Gibt die Größe der Aufrufliste für jeden von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellten Thread an. Lässt keine NULL-Werte zu.|  
|**os_quantum**|**bigint**|Stellt das Quantum für einen nicht präemptiven Task dar, gemessen in Millisekunden. Quantum (in Sekunden) = **os_quantum** /CPU-Taktfrequenz. Lässt keine NULL-Werte zu.|  
|**os_error_mode**|**int**|Gibt den Fehlermodus für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozess an. Lässt keine NULL-Werte zu.|  
|**os_priority_class**|**int**|Gibt die Prioritätsklasse für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozess an. NULL-Werte sind zulässig.<br /><br /> 32 = normal (Fehlerprotokoll besagt, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bei normaler Prioritätsbasis (= 7) startet.)<br /><br /> 128 = Hoch (Fehlerprotokoll besagt, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf hoher Prioritätsbasis ausgeführt wird.)  (=13).)<br /><br /> Weitere Informationen finden Sie unter [Configure the priority boost Server Configuration Option](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md).|  
|**max_workers_count**|**int**|Stellt die maximale Anzahl von Arbeitsthreads dar, die erstellt werden können. Lässt keine NULL-Werte zu.|  
|**scheduler_count**|**int**|Stellt die Anzahl der im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozess konfigurierten Benutzer-Zeitplanungsmodule dar. Lässt keine NULL-Werte zu.|  
|**scheduler_total_count**|**int**|Stellt die Gesamtanzahl der Zeitplanungsmodule in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dar. Lässt keine NULL-Werte zu.|  
|**deadlock_monitor_serial_number**|**int**|Gibt die ID der aktuellen Deadlocküberwachungssequenz an. Lässt keine NULL-Werte zu.|  
|**sqlserver_start_time_ms_ticks**|**bigint**|Stellt die **ms_tick** Nummer beim [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] letzten Start dar. Vergleicht diesen Wert mit dem aktuellen Wert in der ms_ticks-Spalte. Lässt keine NULL-Werte zu.|  
|**sqlserver_start_time**|**datetime**|Gibt das lokale Systemdatum und die Uhrzeit des letzten Starts an [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Lässt keine NULL-Werte zu.|  
|**affinity_type**|**int**|**Gilt für:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] und höher.<br /><br /> Gibt den Typ der Server-CPU-Prozessaffinität an, die derzeit verwendet wird. Lässt keine NULL-Werte zu. Weitere Informationen finden Sie unter [Alter Server Configuration &#40;Transact-SQL-&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md).<br /><br /> 1 = MANUELL<br /><br /> 2 = AUTO|  
|**affinity_type_desc**|**varchar(60)**|**Gilt für:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] und höher.<br /><br /> Beschreibt die **affinity_type** Spalte. Lässt keine NULL-Werte zu.<br /><br /> MANUELL = Die Affinität wurde für mindestens eine CPU festgelegt.<br /><br /> AUTO = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann Threads zwischen CPUs frei verschieben.|  
|**process_kernel_time_ms**|**bigint**|**Gilt für:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] und höher.<br /><br /> Benötigte Gesamtzeit in Millisekunden für alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Threads im Kernelmodus. Dieser Wert kann größer als eine einzelne Prozessoruhr sein, da er die Zeit für alle Prozessoren auf dem Server enthält. Lässt keine NULL-Werte zu.|  
|**process_user_time_ms**|**bigint**|**Gilt für:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] und höher.<br /><br /> Benötigte Gesamtzeit in Millisekunden für alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Threads im Benutzermodus. Dieser Wert kann größer als eine einzelne Prozessoruhr sein, da er die Zeit für alle Prozessoren auf dem Server enthält. Lässt keine NULL-Werte zu.|  
|**time_source**|**int**|**Gilt für:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] und höher.<br /><br /> Gibt die API an, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet wird, um die Wanduhrzeit abzurufen. Lässt keine NULL-Werte zu.<br /><br /> 0 = QUERY_PERFORMANCE_COUNTER<br /><br /> 1 = MULTIMEDIA_TIMER|  
|**time_source_desc**|**nvarchar(60)**|**Gilt für:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] und höher.<br /><br /> Beschreibt die **time_source** Spalte. Lässt keine NULL-Werte zu.<br /><br /> QUERY_PERFORMANCE_COUNTER = die [QueryPerformanceCounter](/windows/win32/api/profileapi/nf-profileapi-queryperformancecounter) -API ruft die Uhrzeit der Wall-Uhr ab.<br /><br /> MULTIMEDIA_TIMER = die [Multimediazeitgeber](/previous-versions//ms713418(v=vs.85)) -API, die die Uhrzeit der Wand Zeit abruft.|  
|**virtual_machine_type**|**int**|**Gilt für:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] und höher.<br /><br /> Gibt an, ob [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in einer virtualisierten Umgebung ausgeführt wird.  Lässt keine NULL-Werte zu.<br /><br /> 0 = NONE<br /><br /> 1 = HYPERVISOR<br /><br /> 2 = OTHER|  
|**virtual_machine_type_desc**|**nvarchar(60)**|**Gilt für:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] und höher.<br /><br /> Beschreibt die **virtual_machine_type** Spalte. Lässt keine NULL-Werte zu.<br /><br /> None = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird nicht auf einem virtuellen Computer ausgeführt.<br /><br /> HYPERVISOR = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird auf einem virtuellen Computer ausgeführt, der von einem Betriebssystem gehostet wird, auf dem HYPERVISOR ausgeführt wird (ein Host Betriebssystem, das eine Hardware gestützte Virtualisierung nutzt).<br /><br /> Other = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird in einem virtuellen Computer ausgeführt, der von einem Betriebssystem gehostet wird, das keinen Hardware-Assistenten wie Microsoft Virtual PC verwendet.|  
|**softnuma_configuration**|**int**|**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher.<br /><br /> Gibt an, wie NUMA-Knoten konfiguriert werden. Lässt keine NULL-Werte zu.<br /><br /> 0 = off zeigt Hardware Standard an<br /><br /> 1 = automatisierter Soft-NUMA<br /><br /> 2 = Manuelle Soft-NUMA über Registrierung|  
|**softnuma_configuration_desc**|**nvarchar(60)**|**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher.<br /><br /> Off = Soft-NUMA-Feature ist Off<br /><br /> ON = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bestimmt automatisch die NUMA-Knoten Größen für Soft-NUMA<br /><br /> Manuell = manuell konfigurierte Soft-NUMA|
|**process_physical_affinity**|**nvarchar (3072)** |**Gilt für:** Ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] .<br /><br />Weitere Informationen. |
|**sql_memory_model**|**int**|**Gilt für:** [!INCLUDE[sssql11](../../includes/sssql11-md.md)] SP4, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 und höher.<br /><br />Gibt das Speichermodell [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an, das von zum Zuweisen von Speicher verwendet wird. Lässt keine NULL-Werte zu.<br /><br />1 = herkömmliches Speichermodell<br />2 = Sperren von Seiten im Speicher<br /> 3 = große Seiten im Speicher|
|**sql_memory_model_desc**|**nvarchar(120)**|**Gilt für:** [!INCLUDE[sssql11](../../includes/sssql11-md.md)] SP4, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 und höher.<br /><br />Gibt das Speichermodell [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an, das von zum Zuweisen von Speicher verwendet wird. Lässt keine NULL-Werte zu.<br /><br />  =  Konventionelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet herkömmliches Speichermodell, um Arbeitsspeicher zuzuweisen. Dabei handelt es sich um ein Standardmäßiges SQL-Speichermodell, wenn das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Dienst Konto während des Starts keine Sperren von Seiten im Speicher hat.<br />  =  LOCK_PAGES [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet Sperr Seiten im Speicher, um Arbeitsspeicher zuzuweisen. Dies ist der standardmäßige SQL-Speicher-Manager, wenn SQL Server-Dienst Konto während SQL Server Starts Berechtigungen zum Sperren von Seiten im Arbeitsspeicher besitzt.<br />   =  LARGE_PAGES [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet große Seiten im Speicher, um Arbeitsspeicher zuzuweisen. SQL Server verwendet die Zuordnung von großen Seiten, um Speicher nur mit der Enterprise Edition zuzuweisen, wenn SQL Server Dienst Konto beim Start des Servers Sperren von Seiten im Arbeitsspeicher besitzt und wenn das Ablaufverfolgungsflag 834 aktiviert ist.|
|**pdw_node_id**|**int**|**Gilt für:** [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
|**socket_count** |**int** | **Anwendungsbereich:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und höher.<br /><br />Gibt die Anzahl der auf dem System verfügbaren Prozessorsockets an. |  
|**cores_per_socket** |**int** | **Anwendungsbereich:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und höher.<br /><br />Gibt die Anzahl der Prozessoren pro Socket an, die auf dem System verfügbar sind. |  
|**numa_node_count** |**int** | **Anwendungsbereich:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und höher.<br /><br />Gibt die Anzahl der auf dem System verfügbaren NUMA-Knoten an. Diese Spalte enthält sowohl physische NUMA-Knoten als auch weiche NUMA-Knoten. |  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   

## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
