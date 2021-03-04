---
description: sys.dm_os_threads (Transact-SQL)
title: sys.dm_os_threads (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_threads_TSQL
- sys.dm_os_threads
- dm_os_threads
- sys.dm_os_threads_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_threads dynamic management view
ms.assetid: a5052701-edbf-4209-a7cb-afc9e65c41c1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bd05ea40dd38053b30e75323e888689fc903f8f1
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837192"
---
# <a name="sysdm_os_threads-transact-sql"></a>sys.dm_os_threads (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Liste aller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Betriebssystemthreads zurück, die unter dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozess ausgeführt werden.  
  
> [!NOTE]  
>  Um dies von oder aus aufzurufen [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , verwenden Sie den Namen **sys.dm_pdw_nodes_os_threads**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|thread_address|**varbinary(8)**|Speicheradresse (Primärschlüssel) des Threads.|  
|started_by_sqlservr|**bit**|Gibt den Threadinitiator an.<br /><br /> 1 = Der Thread wurde von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gestartet.<br /><br /> 0 = Der Thread wurde von einer anderen Komponente gestartet, z. B. von einer erweiterten gespeicherten Prozedur innerhalb von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|os_thread_id|**int**|ID des vom Betriebssystem zugewiesenen Threads.|  
|status|**int**|Internes Statusflag.|  
|instruction_address|**varbinary(8)**|Adresse der zurzeit ausgeführten Anweisung.|  
|creation_time|**datetime**|Zeit, zu der dieser Thread erstellt wurde.|  
|kernel_time|**bigint**|Menge der von diesem Thread verwendeten Kernelzeit.|  
|usermode_time|**bigint**|Menge der von diesem Thread verwendeten Benutzerzeit.|  
|stack_base_address|**varbinary(8)**|Speicheradresse der höchsten Stapeladresse für diesen Thread.|  
|stack_end_address|**varbinary(8)**|Speicheradresse der niedrigsten Stapeladresse für diesen Thread.|  
|stack_bytes_committed|**int**|Anzahl von Bytes, für die im Stapel ein Commit ausgeführt wurde.|  
|stack_bytes_used|**int**|Anzahl von Bytes, die aktiv im Thread verwendet werden.|  
|affinity|**bigint**|CPU-Maske, in der dieser Thread ausgeführt wird. Dies hängt von dem Wert ab, der von der **Alter Server Configuration Set Process-Affinitäts** Anweisung konfiguriert wurde. Kann sich bei weicher Affinität vom Zeitplanungsmodul unterscheiden.|  
|Priorität|**int**|Prioritätswert dieses Threads.|  
|Gebietsschema|**int**|Zwischengespeicherter Gebietsschemabezeichner (LCID) für den Thread.|  
|Token|**varbinary(8)**|Zwischengespeichertes Identitätswechsel-Tokenhandle für den Thread.|  
|is_impersonating|**int**|Gibt an, ob dieser Thread den Win32-Identitätswechsel verwendet.<br /><br /> 1 = Der Thread verwendet Sicherheitsanmeldeinformationen, die von der Standardeinstellung des Prozesses abweichen. Dieser Wert gibt an, dass der Thread die Identität einer Entität annimmt, die nicht mit der Entität übereinstimmt, die den Prozess erstellt hat.|  
|is_waiting_on_loader_lock|**int**|Betriebssystemstatus, der angibt, ob der Thread in der Loadersperre wartet.|  
|fiber_data|**varbinary(8)**|Aktuelle Win32-Fiber, die im Thread ausgeführt wird. Dies gilt nur, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für Lightweightpooling konfiguriert ist.|  
|thread_handle|**varbinary(8)**|Nur zur internen Verwendung.|  
|event_handle|**varbinary(8)**|Nur zur internen Verwendung.|  
|scheduler_address|**varbinary(8)**|Speicheradresse des Zeitplanungsmoduls, das diesem Thread zugeordnet ist. Weitere Informationen finden Sie unter [sys.dm_os_schedulers &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).|  
|worker_address|**varbinary(8)**|Speicheradresse des Arbeitsthreads, der an diesen Thread gebunden ist. Weitere Informationen finden Sie unter [sys.dm_os_workers &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|fiber_context_address|**varbinary(8)**|Interne Fiberkontextadresse. Dies gilt nur, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für Lightweightpooling konfiguriert ist.|  
|self_address|**varbinary(8)**|Interner Konsistenzzeiger.|  
|processor_group|**smallint**|**Gilt für**:  [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] und höher.<br /><br /> Prozessorgruppen-ID.|  
|pdw_node_id|**int**|**Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet.|  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   

## <a name="notes-on-linux-version"></a>Hinweise zur Linux-Version

Aufgrund der Funktionsweise der SQL-Engine unter Linux entsprechen einige dieser Informationen nicht den Linux-Diagnosedaten. Beispielsweise entspricht `os_thread_id` nicht dem Ergebnis von Tools wie `ps` `top` oder dem procfs (/proc/ `pid` ).  Dies liegt an der Plattform-Abstraktionsschicht (sqlpal), einer Ebene zwischen SQL Server Komponenten und dem Betriebssystem.

## <a name="examples"></a>Beispiele  
 Beim Start werden von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Threads gestartet, denen anschließend Arbeitsthreads zugeordnet werden. Externe Komponenten, z. B. eine erweiterte gespeicherte Prozedur, können jedoch Threads unter dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozess starten. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hat keine Kontrolle über diese Threads. sys.dm_os_threads können Informationen zu nicht autorisierten Threads bereitstellen, die Ressourcen im-Prozess verbrauchen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Mit der folgenden Abfrage werden Arbeitsthreads zusammen mit der jeweiligen Ausführungszeit ermittelt, die nicht von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gestartete Threads ausführen.  
  
> [!NOTE]
>  Aus Gründen der Kürze wird in der folgenden Abfrage ein Sternchen (`*`) in der `SELECT`-Anweisung verwendet. Vermeiden Sie die Verwendung des Sternchens (*) insbesondere für Katalogsichten, dynamische Verwaltungssichten und Systemfunktionen mit Tabellenrückgabe. Zukünftige Upgrades und Releases von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] können Spalten hinzufügen und die Reihenfolge der Spalten in diese Sichten und Funktionen ändern. Diese Änderungen könnten zur Funktionsunfähigkeit von Anwendungen führen, die eine bestimmte Reihenfolge und Anzahl von Spalten erwarten.  
  
```  
SELECT *  
  FROM sys.dm_os_threads  
  WHERE started_by_sqlservr = 0;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
  [sys.dm_os_workers &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md)   
 [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
