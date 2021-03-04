---
description: sys.dm_os_process_memory (Transact-SQL)
title: sys.dm_os_process_memory (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_process_memory_TSQL
- dm_os_process_memory_TSQL
- dm_os_process_memory
- sys.dm_os_process_memory
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_process_memory dynamic management view
ms.assetid: e838130c-95d4-4605-9e3b-eb0ab71cd250
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8d8f329db1572b27f1496ffb58c3bc493fd34ca6
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837977"
---
# <a name="sysdm_os_process_memory-transact-sql"></a>sys.dm_os_process_memory (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Die meisten Speicherbelegungen, die für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozessraum attributiert sind, werden über Schnittstellen gesteuert, die eine Nachverfolgung und Berücksichtigung dieser Zuordnungen ermöglichen. Speicherbelegungen werden jedoch eventuell in dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Adressraum ausgeführt, der die internen Arbeitsspeicherverwaltungsroutinen umgeht. Die Werte werden durch Aufrufe des Basisbetriebssystems erhalten. Sie werden nicht durch interne Methoden von manipuliert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , es sei denn, Sie passen sich für gesperrte oder große Seiten Zuordnungen an.  
  
 Alle zurückgegebenen Werte mit Angaben zu den Arbeitsspeichergrößen werden in Kilobytes (KB) angezeigt. Der Spalten **total_virtual_address_space_reserved_kb** ist ein Duplikat von **virtual_memory_in_bytes** von **sys.dm_os_sys_info**.  
  
 In der folgenden Tabelle wird ein vollständiges Bild des Prozessadressraums angegeben.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_process_memory**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**physical_memory_in_use_kb**|**bigint**|Gibt das Prozessworkingset in KB an, wie vom Betriebssystem gemeldet, sowie nachverfolgte Zuordnungen, die über APIs umfangreicher Seiten und AWE-APIs durchgeführt wurden. Lässt keine NULL-Werte zu.|  
|**large_page_allocations_kb**|**bigint**|Gibt den physischen Arbeitsspeicher an, der über APIs umfangreicher Seiten zugeordnet wird. Lässt keine NULL-Werte zu.|  
|**locked_page_allocations_kb**|**bigint**|Gibt im Arbeitsspeicher gesperrte Speicherseiten an. Lässt keine NULL-Werte zu.|  
|**total_virtual_address_space_kb**|**bigint**|Gibt die Gesamtgröße des Benutzermodusteils im virtuellen Adressraum an. Lässt keine NULL-Werte zu.|  
|**virtual_address_space_reserved_kb**|**bigint**|Gibt die Gesamtmenge des vom Prozess reservierten virtuellem Adressraums an. Lässt keine NULL-Werte zu.|  
|**virtual_address_space_committed_kb**|**bigint**|Gibt die Menge des reservierten virtuellen Adressraums an, für die ein Commit oder eine Zuordnung zu physischen Seiten besteht. Lässt keine NULL-Werte zu.|  
|**virtual_address_space_available_kb**|**bigint**|Gibt die Menge an virtuellen Adressräumen an, die gerade frei sind. Lässt keine NULL-Werte zu.<br /><br /> **Hinweis:** Freie Regionen, die kleiner als die Zuordnungs Granularität sind, können vorhanden sein. Diese Bereiche sind für Zuordnungen nicht verfügbar.|  
|**page_fault_count**|**bigint**|Gibt die Anzahl der Seitenfehler an, die vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozess verursacht wurden. Lässt keine NULL-Werte zu.|  
|**memory_utilization_percentage**|**int**|Gibt den Prozentwert des Arbeitsspeichers an, für den ein Commit ausgeführt wurde und der sich im Workingset befindet. Lässt keine NULL-Werte zu.|  
|**available_commit_limit_kb**|**bigint**|Gibt den Arbeitsspeicher an, der für den Commit durch den Prozess verfügbar ist. Lässt keine NULL-Werte zu.|  
|**process_physical_memory_low**|**bit**|Gibt an, dass der Prozess auf Benachrichtigung zu nicht genügend physischem Arbeitsspeicher reagiert. Lässt keine NULL-Werte zu.|  
|**process_virtual_memory_low**|**bit**|Gibt an, dass eine Bedingung nicht genügenden virtuellen Arbeitsspeichers erkannt wurde. Lässt keine NULL-Werte zu.|  
|**pdw_node_id**|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
