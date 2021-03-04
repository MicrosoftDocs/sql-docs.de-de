---
description: sys.dm_os_wait_stats (Transact-SQL)
title: sys.dm_os_wait_stats (Transact-SQL)
ms.custom: contperf-fy21q3
ms.date: 02/10/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_wait_stats_TSQL
- dm_os_wait_stats
- sys.dm_os_wait_stats
- sys.dm_os_wait_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_wait_stats dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 86b3be2a5a1dad9c27f9b55b90d4a079ec6ee1e4
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837257"
---
# <a name="sysdm_os_wait_stats-transact-sql"></a>sys.dm_os_wait_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Gibt Informationen zu allen Wartevorgängen in den Threads zurück, die ausgeführt wurden. In dieser aggregierten Sicht können Sie Leistungsprobleme bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sowie bei bestimmten Abfragen und Batches diagnostizieren. [sys.dm_exec_session_wait_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-session-wait-stats-transact-sql.md) von der Sitzung ähnliche Informationen bereit.  
  
> [!NOTE] 
> Um dies von **[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] oder** aus aufzurufen, verwenden Sie den Namen **sys.dm_pdw_nodes_os_wait_stats**.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|wait_type|**nvarchar(60)**|Der Name des Wartetyps. Weitere Informationen finden Sie unter [Wartetypen](#WaitTypes) weiter unten in diesem Thema.|  
|waiting_tasks_count|**bigint**|Anzahl von Wartevorgängen für diesen Wartetyp. Dieser Leistungsindikator wird beim Starten eines Wartevorgangs inkrementiert.|  
|wait_time_ms|**bigint**|Gesamtwartezeit für diesen Wartetyp (in Millisekunden). Diese Zeit beinhaltet signal_wait_time_ms.|  
|max_wait_time_ms|**bigint**|Maximale Wartezeit für diesen Wartetyp.|  
|signal_wait_time_ms|**bigint**|Differenz zwischen dem Zeitpunkt der Signalisierung des wartenden Threads und dem Beginn der Ausführung.|  
|pdw_node_id|**int**|Der Bezeichner für den Knoten, auf dem sich diese Distribution befindet. <br/> **Gilt für**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] |  
  
## <a name="permissions"></a>Berechtigungen

In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei den Dienst Zielen "Basic", "S0" und "S1" in SQL-Datenbank ist für Datenbanken in Pools für elastische Datenbanken das [Server Administrator](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) Konto oder das [Azure Active Directory Administrator](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) Konto erforderlich. Für alle anderen SQL-Datenbank-Dienst Ziele `VIEW DATABASE STATE` ist die Berechtigung in der Datenbank erforderlich.   

##  <a name="types-of-waits"></a><a name="WaitTypes"></a> Warte Typen  
 **Ressourcen warte** Vorgänge treten auf, wenn ein Arbeits Thread den Zugriff auf eine Ressource anfordert, die nicht verfügbar ist, weil die Ressource von einem anderen Arbeits Thread verwendet wird oder noch nicht verfügbar ist. Beispiele für Ressourcen warte Vorgänge sind sperren, Latches, Netzwerk-und Datenträger-e/a-warte Vorgänge. Sperren und Latchwartevorgänge sind Vorgänge, die auf Synchronisierungsobjekte warten.  
  
**Warteschlangen** -warte Vorgänge treten auf, wenn sich ein Worker im Leerlauf befindet und auf die Zuweisung von Arbeit wartet. Warteschlangen-Wartevorgänge treten am häufigsten bei Hintergrundtasks des Systems auf, wie z. B. der Deadlocküberwachung und dem Cleanup gelöschter Datensätze. Diese Tasks warten, dass Arbeitsanforderungen in einer Arbeitswarteschlange platziert werden. Warteschlangen-Wartevorgänge können in regelmäßigen Abständen selbst dann aktiviert werden, wenn keine neuen Pakete in die Warteschlange übertragen wurden.  
  
 **Externe warte** Vorgänge treten auf, wenn ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Worker darauf wartet, dass ein externes Ereignis, z. b. ein aufrufter gespeicherter Prozeduren oder eine Verbindungs Server Abfrage, abgeschlossen wird. Wenn Sie Probleme mit Blockierungen diagnostizieren, sollten Sie bedenken, dass externe Wartevorgänge nicht immer auf den Leerlauf eines Arbeitsthreads hinweisen, da der Arbeitsthread möglicherweise aktiv externen Code ausführt.  
  
 `sys.dm_os_wait_stats` zeigt die Zeit für abgeschlossene Wartevorgänge an. Aktuelle Wartevorgänge werden in dieser dynamischen Verwaltungssicht nicht angezeigt.  
  
 Ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Arbeitsthread wird nicht als wartend eingestuft, wenn eine der folgenden Aussagen zutrifft:  
  
-   Eine Ressource wird verfügbar.  
  
-   Eine Warteschlange ist nicht leer.  
  
-   Ein externer Prozess wird abgeschlossen.  
  
 Obwohl der Thread nicht mehr wartet, muss er nicht sofort mit der Ausführung beginnen. Ein solcher Thread wird zunächst in die Warteschlange der ausführbaren Arbeitsthreads eingereiht und muss warten, dass ein Quantum auf dem Zeitplanungsmodul ausgeführt wird.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] den Warte Zeit Indikatoren sind **bigint** -Werte und somit nicht so anfällig für einen Rollover, wie die entsprechenden Leistungsindikatoren in früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Bestimmte Arten von Wartezeiten während der Abfrageausführung können auf Engpässe oder Stillstände während der Abfrage hinweisen. Entsprechend können serverweite lange Wartezeiten oder hohe Wartevorgangsanzahlen auf Engpässe oder Hotspots im Zusammenhang mit Abfrageinteraktionen innerhalb der Serverinstanz hinweisen. So weisen beispielsweise Sperrenwartevorgänge auf Datenkonflikte durch Abfragen, Seiten-E/A-Latchwartevorgänge auf langsame E/A-Antwortzeiten und Seitenlatch-Updatewartevorgänge auf ein fehlerhaftes Dateilayout hin.  
  
 Der Inhalt dieser dynamischen Verwaltungssicht kann durch Ausführen des folgenden Befehls zurückgesetzt werden:  
  
```sql  
DBCC SQLPERF ('sys.dm_os_wait_stats', CLEAR);  
GO  
```  
  
Dieser Befehl setzt alle Leistungsindikatoren auf 0 zurück.  
  
> [!NOTE]
> Diese Statistiken sind bei einem Neustart von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht permanent. Alle Daten stellen einen Gesamtwert seit dem letzten Zurücksetzen der Statistiken oder dem Neustarten des Servers dar.  
  
 In der folgenden Tabelle werden die Wartetypen für Tasks in einer Liste aufgeführt.  

|Type |BESCHREIBUNG| 
|-------------------------- |--------------------------| 
|ABR |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| | 
|AM_INDBUILD_ALLOCATION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|AM_SCHEMAMGR_UNSHARED_CACHE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|ASSEMBLY_FILTER_HASHTABLE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|ASSEMBLY_LOAD |Tritt während des exklusiven Zugriffs auf das Laden einer Assembly auf.| 
|ASYNC_DISKPOOL_LOCK |Tritt beim Versuch auf, parallele Threads zu synchronisieren, die Tasks wie das Erstellen oder Initialisieren einer Datei ausführen.| 
|ASYNC_IO_COMPLETION |Tritt auf, wenn ein Task darauf wartet, dass asynchrone nicht-Daten-e/a-Vorgängen beendet werden. Beispiele hierfür sind die e/a-Vorgänge, die an einem betriebsbereiten Standby-Protokoll Versand, der Daten Bank Spiegelung, einigen Massen Import| 
|ASYNC_NETWORK_IO |Tritt bei Netzwerk Schreibvorgängen auf, wenn der Task blockiert wird und darauf wartet, dass die Client Anwendung bestätigt, dass alle an ihn gesendeten Daten verarbeitet wurden. Überprüfen Sie, ob der Client Daten vom Server verarbeitet oder weniger häufig auftritt, dass das Netzwerk erwartungsgemäß funktioniert. Gründe, warum der Client Daten nicht schnell genug verarbeiten kann, sind z. b. Anwendungs Entwurfs Probleme wie das Schreiben von Ergebnissen in eine Datei, während die Ergebnisse eintreffen, auf Benutzereingaben warten oder eine künstliche Wartezeit eingeführt wurde. Außerdem kommt es möglicherweise zu einer langsamen Reaktion des Client Systems aufgrund von Problemen wie niedrigem virtuellem und physischem Arbeitsspeicher, 100% CPU-Auslastung usw. Netzwerkverzögerungen können auch zu diesem warte Vorgang führen.|
|ASYNC_OP_COMPLETION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|ASYNC_OP_CONTEXT_READ |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|ASYNC_OP_CONTEXT_WRITE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|ASYNC_SOCKETDUP_IO |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|AUDIT_GROUPCACHE_LOCK |Tritt bei einem Wartevorgang auf eine Sperre auf, die den Zugriff auf einen speziellen Cache steuert. Der Cache enthält Informationen zu den jeweiligen Überwachungen, mit denen die einzelnen Überwachungsaktionsgruppen überwacht werden sollen.| 
|AUDIT_LOGINCACHE_LOCK |Tritt bei einem Wartevorgang auf eine Sperre auf, die den Zugriff auf einen speziellen Cache steuert. Der Cache enthält Informationen zu den jeweiligen Überwachungen, die zum Überwachen der Überwachungsaktionsgruppen für die Anmeldung verwendet werden.| 
|AUDIT_ON_DEMAND_TARGET_LOCK |Tritt bei einem Wartevorgang auf eine Sperre auf, mit der eine einmalige Initialisierung überwachungsbezogener Ziele von erweiterten Ereignissen sichergestellt werden soll.| 
|AUDIT_XE_SESSION_MGR |Tritt bei einem Wartevorgang auf eine Sperre auf, mit der das Starten und Beenden überwachungsbezogener Ziele von erweiterten Ereignissen synchronisiert werden soll.| 
|BACKUP |Tritt auf, wenn ein Task im Rahmen der Sicherungsverarbeitung blockiert wird.| 
|BACKUP_OPERATOR |Tritt auf, wenn ein Task auf das Einlegen eines Bands wartet. Sie können den Status eines Bands mithilfe von sys.dm_io_backup_tapes abfragen. Wenn keine Bandeinlegung aussteht, kann dieser Wartetyp auf ein Hardwareproblem mit dem Bandlaufwerk hinweisen.| 
|BACKUPBUFFER |Tritt auf, wenn ein Sicherungstask auf Daten oder auf einen Puffer wartet, in dem Daten gespeichert werden sollen. Dies ist nur dann ein Standardwartetyp, wenn ein Task auf die Einlegung eines Bands wartet.| 
|BACKUPIO |Tritt auf, wenn ein Sicherungstask auf Daten oder auf einen Puffer wartet, in dem Daten gespeichert werden sollen. Dies ist nur dann ein Standardwartetyp, wenn ein Task auf die Einlegung eines Bands wartet.| 
|BACKUPTHREAD |Tritt auf, wenn ein Task auf das Ende eines Sicherungstasks wartet. Die Wartezeiten können lang sein (von einigen Minuten bis zu mehreren Stunden). Wenn der Task, auf den gewartet wird, Teil eines E/A-Prozesses ist, weist dieser Wartetyp nicht auf ein Problem hin.| 
|BAD_PAGE_PROCESS |Tritt auf, wenn die Hintergrundprotokollierung fehlerverdächtiger Seiten eine häufigere Ausführung als alle fünf Sekunden zu vermeiden versucht. Eine übermäßige Anzahl von fehlerverdächtigen Seiten bewirkt, dass die Protokollierung häufig ausgeführt wird.| 
|BLOB_METADATA |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|Bmpallocation |Tritt bei parallelen Batches im Batch Modus auf, wenn die Zuordnung eines großen bitmapfilters synchronisiert wird. Wenn die Wartezeit zu lang ist und durch eine Abfrageoptimierung (beispielsweise durch das Hinzufügen von Indizes) nicht verkürzt werden kann, sollten Sie erwägen, den Kostenschwellenwert für Parallelität anzupassen oder den Grad an Parallelität zu verringern.<br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|Bmpbuild |Tritt beim Synchronisieren der Erstellung eines großen bitmapfilters bei parallelen Batch Modus-Plänen auf. Wenn die Wartezeit zu lang ist und durch eine Abfrageoptimierung (beispielsweise durch das Hinzufügen von Indizes) nicht verkürzt werden kann, sollten Sie erwägen, den Kostenschwellenwert für Parallelität anzupassen oder den Grad an Parallelität zu verringern. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|Bmprepartition |Tritt beim Synchronisieren der Neupartitionierung eines großen bitmapfilters bei parallelen Batch Modus-Plänen auf. Wenn die Wartezeit zu lang ist und durch eine Abfrageoptimierung (beispielsweise durch das Hinzufügen von Indizes) nicht verkürzt werden kann, sollten Sie erwägen, den Kostenschwellenwert für Parallelität anzupassen oder den Grad an Parallelität zu verringern. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|Bmpreplikation |Tritt bei parallelen Batches im Batch Modus auf, wenn die Replikation eines großen bitmapfilters über Arbeitsthreads hinweg synchronisiert wird. Wenn die Wartezeit zu lang ist und durch eine Abfrageoptimierung (beispielsweise durch das Hinzufügen von Indizes) nicht verkürzt werden kann, sollten Sie erwägen, den Kostenschwellenwert für Parallelität anzupassen oder den Grad an Parallelität zu verringern. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|Bpsort |Tritt beim Synchronisieren der Sortierung eines Datasets über mehrere Threads bei parallelen Batch Modus-Plänen auf. Wenn die Wartezeit zu lang ist und durch eine Abfrageoptimierung (beispielsweise durch das Hinzufügen von Indizes) nicht verkürzt werden kann, sollten Sie erwägen, den Kostenschwellenwert für Parallelität anzupassen oder den Grad an Parallelität zu verringern. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|BROKER_CONNECTION_RECEIVE_TASK |Tritt auf, wenn auf einen Zugriff für den Empfang einer Nachricht an einem Verbindungsendpunkt gewartet wird. Der Empfangszugriff auf den Endpunkt wird serialisiert.| 
|BROKER_DISPATCHER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|BROKER_ENDPOINT_STATE_MUTEX |Tritt auf, wenn beim Zugriff auf den Status eines Service Broker Verbindungs Endpunkts ein Konflikt vorliegt. Der Zugriff auf den Status für Änderungen wird serialisiert.| 
|BROKER_EVENTHANDLER |Tritt auf, wenn ein Task auf den primären Ereignishandler der Service Broker wartet. Dieser Wartetyp sollte nur sehr kurz auftreten.| 
|BROKER_FORWARDER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|BROKER_INIT |Tritt beim Initialisieren von Service Broker in jeder aktiven Datenbank auf. Dieser Wartetyp sollte nicht häufig auftreten.| 
|BROKER_MASTERSTART |Tritt auf, wenn ein Task darauf wartet, dass der primäre Ereignishandler des-Service Broker gestartet wird. Dieser Wartetyp sollte nur sehr kurz auftreten.| 
|BROKER_RECEIVE_WAITFOR |Tritt auf, wenn RECEIVE WAITFOR wartet. Dies bedeutet möglicherweise, dass entweder keine Nachrichten in der Warteschlange empfangen werden können oder ein Sperr Konflikt verhindert, dass er Nachrichten aus der Warteschlange empfängt.| 
|BROKER_REGISTERALLENDPOINTS |Tritt während der Initialisierung eines Service Broker Verbindungs Endpunkts auf. Dieser Wartetyp sollte nur sehr kurz auftreten.| 
|BROKER_SERVICE |Tritt auf, wenn die mit einem Ziel Dienst verknüpfte Service Broker Zielliste aktualisiert wird oder die Priorität erneut priorisiert wird.| 
|BROKER_SHUTDOWN |Tritt bei einem geplanten Herunterfahren von Service Broker ein. Dieser Wartetyp sollte, wenn überhaupt, nur sehr kurz auftreten.| 
|BROKER_START |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|BROKER_TASK_SHUTDOWN |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|BROKER_TASK_STOP |Tritt auf, wenn der Task Handler für die Service Broker Warteschlange versucht, den Task zu beenden. Die Statusüberprüfung wird serialisiert und muss vorher ausgeführt werden.| 
|BROKER_TASK_SUBMIT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|BROKER_TO_FLUSH |Tritt auf, wenn der Service Broker Lazy Speicherung von geleert die in-Memory-Übertragungs Objekte in eine Arbeits Tabelle leert.| 
|BROKER_TRANSMISSION_OBJECT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|BROKER_TRANSMISSION_TABLE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|BROKER_TRANSMISSION_WORK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|BROKER_TRANSMITTER |Tritt auf, wenn der Service Broker Sender auf Arbeit wartet. Service Broker verfügt über eine Komponente, die als Sender bezeichnet wird, der Nachrichten aus mehreren Dialogfeldern plant, die über das Netzwerk über eine oder mehrere Verbindungs Endpunkte gesendet werden sollen. Der Sender verfügt zu diesem Zweck über zwei dedizierte Threads. Dieser Wartetyp wird in Rechnung gestellt, wenn diese senderthreads darauf warten, dass Dialog Meldungen mithilfe der Transportverbindungen gesendet werden. Hohe Werte waiting_tasks_count für diesen Wartetyp zeigen auf zeitweilig auftretende Arbeit für diese senderthreads und sind keine Anzeichen für Leistungsprobleme. Wenn Service Broker überhaupt nicht verwendet wird, sollte waiting_tasks_count 2 (für die zwei Übermittlers) und wait_time_ms die doppelte Dauer seit dem Instanzstart sein. Siehe [Service Broker-warte Statistik](/archive/blogs/sql_service_broker/service-broker-wait-types).|
|BUILTIN_HASHKEY_MUTEX |Kann nach dem Start einer Instanz auftreten, während interne Datenstrukturen initialisiert werden. Tritt nach dem Initialisieren der Datenstrukturen nicht erneut auf.| 
|CHANGE_TRACKING_WAITFORCHANGES |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|CHECK_PRINT_RECORD |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|CHECK_SCANNER_MUTEX |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|CHECK_TABLES_INITIALIZATION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|CHECK_TABLES_SINGLE_SCAN |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|CHECK_TABLES_THREAD_BARRIER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|CHECKPOINT_QUEUE |Tritt auf, während der Prüfpunkttask auf die nächste Prüfpunktanforderung wartet.| 
|CHKPT |Tritt beim Starten des Servers auf, um dem Prüfpunktthread mitzuteilen, dass er beginnen kann.| 
|CLEAR_DB |Tritt während Vorgängen auf, die den Status einer Datenbank ändern, z. B. beim Öffnen oder Schließen einer Datenbank.| 
|CLR_AUTO_EVENT |Tritt bei der CLR (Common Language Runtime)-Ausführung eines Tasks auf, wenn dieser auf das Initialisieren eines bestimmten automatischen Ereignisses (autoevent) wartet. Lange Wartezeiten sind typisch und zeigen kein Problem an.| 
|CLR_CRST |Tritt bei der CLR-Ausführung (Common Language Runtime) eines Tasks auf, wenn dieser auf den Eintritt in einen kritischen Abschnitt des Tasks wartet, der zurzeit von einem anderen Task verwendet wird.| 
|CLR_JOIN |Tritt bei der CLR-Ausführung (Common Language Runtime) eines Tasks auf, wenn dieser auf das Ende eines anderen Tasks wartet. Dieser Wartestatus tritt auf, wenn ein Join zwischen Tasks besteht.| 
|CLR_MANUAL_EVENT |Tritt bei der CLR (Common Language Runtime)-Ausführung eines Tasks auf, wenn dieser auf das Initiieren eines bestimmten manuellen Ereignisses wartet.| 
|CLR_MEMORY_SPY |Tritt während eines Wartevorgangs auf den Erhalt einer Sperre für eine Datenstruktur auf, die zum Aufzeichnen aller virtuellen Speicherbelegungen aus der CLR verwendet wird. Die Datenstruktur wird gesperrt, um bei Parallelzugriffen die Integrität beizubehalten.| 
|CLR_MONITOR |Tritt bei der CLR (Common Language Runtime)-Ausführung eines Tasks auf, wenn dieser darauf wartet, eine Sperre für die Überwachung abrufen zu können.| 
|CLR_RWLOCK_READER |Tritt bei der CLR-Ausführung (Common Language Runtime) eines Tasks auf, wenn dieser auf eine Sperre des Lesers wartet.| 
|CLR_RWLOCK_WRITER |Tritt bei der CLR-Ausführung (Common Language Runtime) eines Tasks auf, wenn dieser auf eine Sperre des Schreibers wartet.| 
|CLR_SEMAPHORE |Tritt bei der CLR-Ausführung (Common Language Runtime) eines Tasks auf, wenn dieser auf eine Semaphore wartet.| 
|CLR_TASK_START |Tritt auf, während auf den Abschluss des Startvorgangs eines CLR-Tasks gewartet wird.| 
|CLRHOST_STATE_ACCESS |Tritt bei einem Wartevorgang auf den Erhalt von exklusivem Zugriff auf die CLR Hosting-Datenstrukturen auf. Dieser Wartetyp tritt auf, während die CLR-Laufzeit eingerichtet oder beendet wird.| 
|Cmempartitioniert |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|CMEMTHREAD |Tritt auf, wenn ein Task auf ein threadsicheres Speicherobjekt wartet. Die Wartezeit kann bei Konflikten zunehmen, die entstehen, wenn mehrere Tasks Speicher vom gleichen Speicherobjekt zuordnen.| 
|COLUMNSTORE_BUILD_THROTTLE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|COLUMNSTORE_COLUMNDATASET_SESSION_LIST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|COMMIT_TABLE |Nur zur internen Verwendung.| 
|CONNECTION_ENDPOINT_LOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|Countrytrecoverymgr |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|CREATE_DATINISERVICE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|CXCONSUMER <a name="cxconsumer"></a>|Tritt bei parallelen Abfrage Plänen auf, wenn ein Consumerthread (übergeordnetes Element) darauf wartet, dass ein Producer-Thread Zeilen sendet. Cxconsumer-warte Vorgänge werden von einem Exchange-Iterator verursacht, der außerhalb der Zeilen aus dem zugehörigen Producer-Thread läuft. Dies ist ein normaler Bestandteil der parallelen Abfrage Ausführung. <br /><br /> **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2, [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]|
|CXPACKET <a name="cxpacket"></a>|Tritt bei parallelen Abfrage Plänen auf, wenn darauf gewartet wird, dass der [Exchange-Iterator](../../relational-databases/showplan-logical-and-physical-operators-reference.md)des Abfrage Prozessors synchronisiert wird und wenn Zeilen erzeugt und genutzt werden. Wenn der Warte Vorgang übermäßig groß ist und nicht durch Optimieren der Abfrage (z. b. Hinzufügen von Indizes) reduziert werden kann, sollten Sie den Kosten Schwellenwert für Parallelität oder den maximalen Grad an Parallelität (MAXDOP) verringern. <br /><br /> **Hinweis:** Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 bezieht sich cxpacket nur auf die Synchronisierung des Exchange-Iterators und das Generieren von Zeilen. Threads, die Zeilen verarbeiten, werden separat im cxconsumer-Wartetyp nachverfolgt. Wenn die Consumerthreads zu langsam sind, kann der Exchange-iteratorpuffer voll werden und cxpacket-warte Vorgänge verursachen. <br /><br /> **Hinweis:** In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] bezieht sich cxpacket nur auf das warten auf Threads, die Zeilen erzeugen. Die Exchange-iteratorsynchronisierung wird in den CXSYNC_PORT-und CXSYNC_CONSUMER-warte Typen separat nachverfolgt. Threads, die Zeilen verarbeiten, werden separat im cxconsumer-Wartetyp nachverfolgt.<br /> | 
|CXSYNC_PORT|Tritt bei parallelen Abfrage Plänen auf, wenn auf das Öffnen, schließen und Synchronisieren von [Exchange-iteratorports](../../relational-databases/showplan-logical-and-physical-operators-reference.md) zwischen Producer-und Consumerthreads gewartet wird. Wenn ein Abfrageplan z. b. über einen langen Sortiervorgang verfügt, kann CXSYNC_PORT Wartezeiten höher sein, da die Sortierung vor dem Synchronisieren des Exchange-iteratorports erfolgen muss. <br /><br /> **Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]| 
|CXSYNC_CONSUMER|Tritt bei parallelen Abfrage Plänen auf, wenn auf das Erreichen eines [Exchange-iteratorsynchronisierungs](../../relational-databases/showplan-logical-and-physical-operators-reference.md) Punkts zwischen allen Consumerthreads gewartet wird. <br /><br /> **Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]| 
|CXROWSET_SYNC |Tritt während eines parallelen Bereichsscans auf.| 
|DAC_INIT |Tritt während des Initialisierungsvorgangs der dedizierten Administratorverbindung auf.| 
|DBCC_SCALE_OUT_EXPR_CACHE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|DBMIRROR_DBM_EVENT |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|DBMIRROR_DBM_MUTEX |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|DBMIRROR_EVENTS_QUEUE |Tritt auf, wenn die Datenbankspiegelung auf die Verarbeitung von Ereignissen wartet.| 
|DBMIRROR_SEND |Tritt auf, wenn ein Task auf die Beseitigung eines Kommunikationsrückstands auf der Netzwerkebene wartet, um Nachrichten senden zu können. Weist darauf hin, dass die Kommunikationsebene möglicherweise überlastet ist, was sich negativ auf den Datendurchsatz bei der Datenbankspiegelung auswirken kann.| 
|DBMIRROR_WORKER_QUEUE |Zeigt an, dass der Arbeitstask der Datenbankspiegelung auf weitere Arbeit wartet.| 
|DBMIRRORING_CMD |Tritt auf, wenn ein Task darauf wartet, dass Protokolldatensätze auf den Datenträger geleert werden. Dieser Wartestatus dauert erwartungsgemäß einige Zeit an.| 
|DBSEEDING_FLOWCONTROL |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|DBSEEDING_OPERATION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|DEADLOCK_ENUM_MUTEX |Tritt auf, wenn die Deadlocküberwachung und sys.dm_os_waiting_tasks sicherstellen, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht mehrere Deadlocksuchvorgänge gleichzeitig ausführt.| 
|DEADLOCK_TASK_SEARCH |Eine lange Wartezeit für diese Ressource zeigt an, dass der Server Abfragen zusätzlich zu sys.dm_os_waiting_tasks ausführt und diese Abfragen die Ausführung einer Deadlocksuche durch die Deadlocküberwachung blockieren. Dieser Wartetyp wird nur von der Deadlocküberwachung verwendet. Für Abfragen zusätzlich zu sys.dm_os_waiting_tasks wird DEADLOCK_ENUM_MUTEX verwendet.| 
|DEBUG |Tritt bei Transact-SQL-und CLR-Debuggen für die interne Synchronisierung auf| 
|DIRECTLOGCONSUMER_LIST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|DIRTY_PAGE_POLL |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|DIRTY_PAGE_SYNC |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|DIRTY_PAGE_TABLE_LOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|DISABLE_VERSIONING |Tritt auf, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] den Versionstransaktions-Manager abruft, um festzustellen, ob der Zeitstempel der ersten aktiven Transaktion nach dem Zeitstempel liegt, an dem der Statuswechsel begonnen hat. Wenn dies der Fall ist, sind alle Momentaufnahmetransaktionen abgeschlossen, die vor dem Ausführen der ALTER DATABASE-Anweisung begonnen wurden. Dieser Wartestatus wird verwendet, wenn von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Versionsverwaltung mithilfe der ALTER DATABASE-Anweisung deaktiviert wird.| 
|DISKIO_SUSPEND |Tritt auf, wenn ein Task auf den Zugriff auf eine Datei wartet, wenn eine externe Sicherung aktiviert ist. Dieser Wartetyp wird für jeden wartenden Benutzerprozess gemeldet. Ein Wert über fünf pro Benutzerprozess kann darauf hinweisen, dass die externe Sicherung zu lange dauert.| 
|DISPATCHER_PRIORITY_QUEUE_SEMAPHORE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|DISPATCHER_QUEUE_SEMAPHORE |Tritt auf, wenn ein Thread aus dem Verteilerpool auf weitere Arbeit wartet. Die Wartezeit für diesen Wartetyp steigt voraussichtlich, wenn der Verteiler im Leerlauf ist.| 
|DLL_LOADING_MUTEX |Tritt einmal auf, während auf das Laden der DLL des XML-Parsers gewartet wird.| 
|DPT_ENTRY_LOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|DROP_DATABASE_TIMER_TASK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|DROPTEMP |Tritt zwischen den Versuchen, ein temporäres Objekt zu löschen, auf, wenn beim vorherigen Versuch ein Fehler aufgetreten ist. Die Wartedauer nimmt mit jedem fehlerhaften Löschversuch exponentiell zu.| 
|DTC |Tritt auf, wenn ein Task auf ein Ereignis wartet, das für die Verwaltung des Statusübergangs verwendet wird. Mit diesem Status wird gesteuert, wann die Wiederherstellung von MS DTC-Transaktionen (Microsoft Distributed Transaction Coordinator) erfolgt, nachdem von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] benachrichtigt wird, dass der MS DTC-Dienst nicht mehr verfügbar ist.| 
|DTC_ABORT_REQUEST |Tritt in einer MS DTC-Arbeitsthreadsitzung auf, wenn die Sitzung darauf wartet, eine MS DTC-Transaktion in Besitz zu nehmen. Sobald MS DTC die Transaktion besitzt, kann die Sitzung ein Rollback der Transaktion ausführen. Im Allgemeinen wartet die Sitzung auf eine andere Sitzung, die die Transaktion verwendet.| 
|DTC_RESOLVE |Tritt auf, wenn ein Wiederherstellungstask auf die master-Datenbank in einer datenbankübergreifenden Transaktion wartet, damit der Task das Ergebnis der Transaktion abfragen kann.| 
|DTC_STATE |Tritt auf, wenn ein Task auf ein Ereignis wartet, das Änderungen am internen globalen Statusobjekt für MS DTC schützt. Dieser Status sollte nur sehr kurze Zeit andauern.| 
|DTC_TMDOWN_REQUEST |Tritt in einer MS DTC-Arbeitsthreadsitzung auf, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] benachrichtigt wird, dass der MS DTC-Dienst nicht zur Verfügung steht. Zunächst wartet der Arbeitsthread auf den Beginn des MS DTC-Wiederherstellungsprozesses. Dann wartet der Arbeitsthread darauf, das Ergebnis der verteilten Transaktion, an der der Arbeitsthread arbeitet, abrufen zu können. Dies kann so lange fortgesetzt werden, bis die Verbindung mit dem MS DTC-Dienst wiederhergestellt ist.| 
|DTC_WAITFOR_OUTCOME |Tritt auf, wenn Wiederherstellungstasks darauf warten, dass MS DTC aktiviert wird, damit vorbereitete Transaktionen aufgelöst werden können.| 
|DTCNEW_ENLIST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|DTCNEW_PREPARE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|DTCNEW_RECOVERY |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|DTCNEW_TM |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|DTCNEW_TRANSACTION_ENLISTMENT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|Dtcpntsync |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|DUMP_LOG_COORDINATOR |Tritt auf, wenn ein Haupttask darauf wartet, dass ein untergeordneter Task Daten generiert. Normalerweise tritt dieser Status nicht auf. Eine lange Wartezeit weist auf eine unerwartete Blockierung hin. In diesem Fall sollte der untergeordnete Task überprüft werden.| 
|DUMP_LOG_COORDINATOR_QUEUE |Nur zur internen Verwendung.| 
|DUMPTRIGGER |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|EC |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|EE_PMOLOCK |Tritt bei der Synchronisierung bestimmter Typen von Speicherbelegungen während der Ausführung von Anweisungen auf.| 
|EE_SPECPROC_MAP_INIT |Tritt bei der Synchronisierung der Erstellung der internen Prozedurhashtabelle auf. Dieser Wartevorgang kann nur beim ersten Zugriff auf die Hashtabelle nach dem Start der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz auftreten.| 
|ENABLE_EMPTY_VERSIONING |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|ENABLE_VERSIONING |Tritt auf, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf das Beenden aller Updatetransaktionen in dieser Datenbank wartet, bevor die Datenbank als bereit für den Übergang in den Status der Zulässigkeit der Momentaufnahmeisolation deklariert werden kann. Dieser Status wird verwendet, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Momentaufnahmeisolation mithilfe der ALTER DATABASE-Anweisung aktiviert.| 
|ERROR_REPORTING_MANAGER |Tritt bei der Synchronisierung mehrerer gleichzeitiger Fehlerprotokollinitialisierungen auf.| 
|EXCHANGE |Tritt während der Synchronisierung des Austauschiterators des Abfrageprozessors während paralleler Abfragen auf.| 
|EXECSYNC |Tritt während paralleler Abfragen bei der Synchronisierung im Abfrageprozessor in Bereichen auf, die nicht mit dem Austauschiterator verbunden sind. Beispiele für solche Bereiche sind Bitmaps, LOBs (Large Objects) und der Spooliterator. Dieser Wartestatus kann von LOBs häufig verwendet werden.| 
|EXECUTION_PIPE_EVENT_INTERNAL |Tritt während der Synchronisierung zwischen den Producer- und Consumerteilen der Batchausführung auf, die über den Verbindungskontext gesendet werden.| 
|EXTERNAL_RG_UPDATE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|EXTERNAL_SCRIPT_NETWORK_IO |Nur zur internen Verwendung. <br /><br /> **Gilt für**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] bis aktuelle.| 
|EXTERNAL_SCRIPT_PREPARE_SERVICE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|EXTERNAL_SCRIPT_SHUTDOWN |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|EXTERNAL_WAIT_ON_LAUNCHER, |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|FABRIC_HADR_TRANSPORT_CONNECTION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|FABRIC_REPLICA_CONTROLLER_LIST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|FABRIC_REPLICA_CONTROLLER_STATE_AND_CONFIG |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|FABRIC_REPLICA_PUBLISHER_EVENT_PUBLISH |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|FABRIC_REPLICA_PUBLISHER_SUBSCRIBER_LIST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|FABRIC_WAIT_FOR_BUILD_REPLICA_EVENT_PROCESSING |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|FAILPOINT |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|FCB_REPLICA_READ |Tritt bei der Synchronisierung der Lesevorgänge einer Sparsedatei der Momentaufnahme (oder einer von DBCC erstellten temporären Momentaufnahme) auf.| 
|FCB_REPLICA_WRITE |Tritt beim Synchronisieren der Push- oder Pullvorgänge einer Seite in eine Sparsedatei der Momentaufnahme (oder einer von DBCC erstellten temporären Momentaufnahme) auf.| 
|FEATURE_SWITCHES_UPDATE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|FFT_NSO_DB_KILL_FLAG |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_NSO_DB_LIST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_NSO_FCB |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_NSO_FCB_FIND |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_NSO_FCB_PARENT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_NSO_FCB_RELEASE_CACHED_ENTRIES |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_NSO_FCB_STATE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|FFT_NSO_FILEOBJECT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_NSO_TABLE_LIST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_NTFS_STORE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_RECOVERY |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_RSFX_COMM |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_RSFX_WAIT_FOR_MEMORY |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_STARTUP_SHUTDOWN |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_STORE_DB |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_STORE_ROWSET_LIST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FFT_STORE_TABLE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FILE_VALIDATION_THREADS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|FILESTREAM_CACHE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FILESTREAM_CHUNKER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FILESTREAM_CHUNKER_INIT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FILESTREAM_FCB |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FILESTREAM_FILE_OBJECT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FILESTREAM_WORKITEM_QUEUE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FILETABLE_SHUTDOWN |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FOREIGN_REDO |Nur zur internen Verwendung. <br /><br /> **Gilt für**: [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] bis aktuelle.| 
|FORWARDER_TRANSITION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|FS_FC_RWLOCK |Tritt bei einem Wartevorgang auf die Durchführung einer der beiden folgenden Aktionen durch den FILESTREAM-Garbage Collector auf:| 
|FS_GARBAGE_COLLECTOR_SHUTDOWN |Tritt auf, wenn der FILESTREAM-Garbage Collector auf das Abschließen von Cleanuptasks wartet.| 
|FS_HEADER_RWLOCK |Tritt bei einem Wartevorgang auf den Erhalt von Zugriff auf den FILESTREAM-Header eines FILESTREAM-Datencontainers auf, um entweder Inhalte in der FILESTREAM-Headerdatei (Filestream.hdr) zu lesen oder zu aktualisieren.| 
|FS_LOGTRUNC_RWLOCK |Tritt bei einem Wartevorgang auf den Erhalt von Zugriff auf die FILESTREAM-Protokollkürzung auf, um eine der beiden folgenden Aktionen auszuführen:| 
|FSA_FORCE_OWN_XACT |Tritt auf, wenn ein FILESTREAM-Datei-E/A-Vorgang an die zugeordnete Transaktion gebunden werden muss, die Transaktion jedoch gerade im Besitz einer anderen Sitzung ist.| 
|FSAGENT |Tritt auf, wenn ein FILESTREAM-Datei-E/A-Vorgang auf eine FILESTREAM-Agent-Ressource wartet, die gerade von einem anderen Datei-E/A-Vorgang verwendet wird.| 
|FSTR_CONFIG_MUTEX |Tritt bei einem Wartevorgang auf eine andere Neukonfiguration eines FILESTREAM-Funktionen auf, die abgeschlossen werden soll.| 
|FSTR_CONFIG_RWLOCK |Tritt bei einem Wartevorgang auf die Serialisierung des Zugriffs auf die FILESTREAM-Konfigurationsparameter auf.| 
|FT_COMPROWSET_RWLOCK |Volltext wartet auf Metadatenvorgang für Fragment. Wird nur für Informationszwecke dokumentiert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|FT_IFTS_RWLOCK |Volltext wartet auf interne Synchronisierung. Wird nur für Informationszwecke dokumentiert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|FT_IFTS_SCHEDULER_IDLE_WAIT |Volltext-Wartetyp für den Ruhezustand des Zeitplanungsmoduls. Das Zeitplanungsmodul befindet sich im Leerlauf.| 
|FT_IFTSHC_MUTEX |Volltext wartet auf einen fdhost-Steuerungsvorgang. Wird nur für Informationszwecke dokumentiert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|FT_IFTSISM_MUTEX |Volltext wartet auf einen Kommunikationsvorgang. Wird nur für Informationszwecke dokumentiert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|FT_MASTER_MERGE |Volltext wartet auf Masterzusammenführungsvorgang. Wird nur für Informationszwecke dokumentiert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|FT_MASTER_MERGE_COORDINATOR |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FT_METADATA_MUTEX |Wird nur für Informationszwecke dokumentiert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|FT_PROPERTYLIST_CACHE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|FT_RESTART_CRAWL |Tritt auf, wenn ein Volltextcrawl von einem letzten bekannten fehlerfreien Punkt neu gestartet werden muss, um nach einem vorübergehenden Fehler wiederhergestellt zu werden. Durch die Wartezeit können die Arbeitstasks, die zurzeit an der jeweiligen Auffüllung arbeiten, abgeschlossen werden oder den aktuellen Schritt beenden.| 
|FULLTEXT GATHERER |Tritt bei der Synchronisierung von Volltextvorgängen auf.| 
|GDMA_GET_RESOURCE_OWNER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|GHOSTCLEANUP_UPDATE_STATS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|Ghostcleanupsyncmgr |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|GLOBAL_QUERY_CANCEL |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|GLOBAL_QUERY_CLOSE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|GLOBAL_QUERY_CONSUMER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|GLOBAL_QUERY_PRODUCER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|GLOBAL_TRAN_CREATE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|GLOBAL_TRAN_UCS_SESSION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|GUARDIAN |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|HADR_AG_MUTEX |Tritt auf, wenn eine Always on DDL-Anweisung oder ein Windows Server-Failoverclustering-Befehl auf exklusiven Lese-/Schreibzugriff auf die Konfiguration einer Verfügbarkeits Gruppe wartet. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_AR_CRITICAL_SECTION_ENTRY |Tritt auf, wenn eine Always on DDL-Anweisung oder ein Windows Server-Failoverclustering-Befehl auf exklusiven Lese-/Schreibzugriff auf den Lauf Zeit Status des lokalen Replikats der zugeordneten Verfügbarkeits Gruppe wartet. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_AR_MANAGER_MUTEX |Tritt auf, wenn das Herunterfahren eines Verfügbarkeitsreplikats auf das Abschließen des Startvorgangs wartet oder wenn der Start eines Verfügbarkeitsreplikats auf das Abschließen des Herunterfahrens wartet. Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_AR_UNLOAD_COMPLETED |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_ARCONTROLLER_NOTIFICATIONS_SUBSCRIBER_LIST |Der Verleger für ein Verfügbarkeitsreplikatereignis (z. B. eine Statusänderung oder eine Konfigurationsänderung) wartet auf exklusiven Lese-/Schreibzugriff für die Liste der Ereignisabonnenten. Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_BACKUP_BULK_LOCK |Die Always on primäre Datenbank hat eine Sicherungs Anforderung von einer sekundären Datenbank empfangen und wartet darauf, dass der Hintergrund Thread die Verarbeitung der Anforderung zum Abrufen oder Freigeben der bulkop-Sperre abgeschlossen hat. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_BACKUP_QUEUE |Der Sicherungs Hintergrund Thread der Always on primären Datenbank wartet auf eine neue Arbeits Anforderung von der sekundären Datenbank. (In der Regel tritt dies auf, wenn die primäre Datenbank das bulkop-Protokoll speichert und darauf wartet, dass die sekundäre Datenbank angibt, dass die primäre Datenbank die Sperre freigeben kann.) <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_CLUSAPI_CALL |Ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Thread wartet darauf, vom nicht präemptiven Modus (geplant durch [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ) in den präemptiven Modus (geplant durch das Betriebssystem) zu wechseln, um Windows Server-Failoverclustering-APIs aufzurufen. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_COMPRESSED_CACHE_SYNC |Warten auf Zugriff auf den Cache mit komprimierten Protokollblöcken, die verwendet werden, um eine redundante Komprimierung der Protokollblöcke zu verhindern, die an mehrere sekundäre Datenbanken gesendet werden. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_CONNECTIVITY_INFO |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_DATABASE_FLOW_CONTROL |Warten, dass Meldungen an den Partner gesendet werden, wenn die maximale Anzahl von Meldungen in der Warteschlange erreicht wurde. Gibt an, dass die Protokollscans schneller ausgeführt werden als Netzwerksendevorgänge. Dies ist nur ein Problem, wenn Netzwerksendevorgänge langsamer als erwartet sind. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_DATABASE_VERSIONING_STATE |Tritt bei der Versions Zustandsänderung einer Always on sekundären Datenbank auf. Diese Wartezeit erfolgt für interne Datenstrukturen und ist in der Regel sehr kurz, ohne direkte Auswirkung auf den Datenzugriff. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_DATABASE_WAIT_FOR_RECOVERY |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|HADR_DATABASE_WAIT_FOR_RESTART |Warten auf den Neustart der Datenbank unter Always on Verfügbarkeits Gruppen-Steuerelement. Unter normalen Bedingungen ist dies kein Problem für Kunden, da Wartezeiten hier erwartet werden. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING |Eine Abfrage von Objekten in einer lesbaren sekundären Datenbank einer Always on Verfügbarkeits Gruppe wird bei der Zeilen Versionsverwaltung blockiert, während auf einen Commit oder ein Rollback aller Transaktionen gewartet wird, die aktiv waren, als das sekundäre Replikat für Lese Arbeits Auslastungen aktiviert war. Dieser Wartetyp garantiert, dass Zeilenversionen vor der Ausführung einer Abfrage unter der Momentaufnahmeisolation verfügbar sind. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_DB_COMMAND |Warten auf Antworten auf konverterende Nachrichten (die eine explizite Antwort von der anderen Seite erfordern, mithilfe der Always on konverational Message Infrastructure). Eine Reihe von unterschiedlichen Nachrichtentypen verwendet diesen Wartetyp. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_DB_OP_COMPLETION_SYNC |Warten auf Antworten auf konverterende Nachrichten (die eine explizite Antwort von der anderen Seite erfordern, mithilfe der Always on konverational Message Infrastructure). Eine Reihe von unterschiedlichen Nachrichtentypen verwendet diesen Wartetyp. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_DB_OP_START_SYNC |Eine Always on DDL-Anweisung oder ein Windows Server-Failoverclustering-Befehl wartet auf den serialisierten Zugriff auf eine Verfügbarkeits Datenbank und deren Lauf Zeit Status. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_DBR_SUBSCRIBER |Der Verleger für ein Verfügbarkeitsreplikatereignis (z. B. eine Statusänderung oder eine Konfigurationsänderung) wartet auf exklusiven Lese-/Schreibzugriff auf den Laufzeitstatus eines Ereignisabonnenten, der einer Verfügbarkeitsdatenbank entspricht. Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_DBR_SUBSCRIBER_FILTER_LIST |Der Verleger für ein Verfügbarkeitsreplikatereignis (z. B. eine Statusänderung oder eine Konfigurationsänderung) wartet auf exklusiven Lese-/Schreibzugriff auf die Liste von Ereignisabonnenten, die Verfügbarkeitsdatenbanken entsprechen. Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_DBSEEDING |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|HADR_DBSEEDING_LIST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|HADR_DBSTATECHANGE_SYNC |Parallelitätssteuerungs-Wartevorgang zum Aktualisieren des internen Status des Datenbankreplikats. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_FABRIC_CALLBACK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|HADR_FILESTREAM_BLOCK_FLUSH |Der FileStream-Always on Transport-Manager wartet, bis die Verarbeitung eines Protokoll Blocks abgeschlossen ist. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_FILESTREAM_FILE_CLOSE |Der FileStream-Always on Transport-Manager wartet, bis die nächste FILESTREAM-Datei verarbeitet und das zugehörige Handle geschlossen wird. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_FILESTREAM_FILE_REQUEST |Ein Always on sekundäres Replikat wartet darauf, dass das primäre Replikat alle angeforderten FILESTREAM-Dateien während der rückgängig- <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_FILESTREAM_IOMGR |Der FileStream-Always on Transport-Manager wartet auf die R/W-Sperre, die den FILESTREAM Always on e/a-Manager während des Starts oder herunter Fahrens schützt. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_FILESTREAM_IOMGR_IOCOMPLETION |Der FileStream-Always on e/a-Manager wartet auf den Abschluss des e/a-Vorgangs. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_FILESTREAM_MANAGER |Der FileStream-Always on Transport-Manager wartet auf die R/W-Sperre, die den FILESTREAM Always on Transport-Manager während des Starts oder herunter Fahrens schützt. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_FILESTREAM_PREPROC |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|HADR_GROUP_COMMIT |Die Transaktionscommitverarbeitung wartet auf das Zulassen eines Gruppencommits, damit mehrere Protokolldatensätze für den Commit in einen einzelnen Protokollblock gesetzt werden können. Dieser Wartevorgang ist eine erwartete Bedingung, durch die der Protokoll-E/A, die Aufzeichnung sowie Sendevorgänge optimiert werden. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_LOGCAPTURE_SYNC |Parallelitätssteuerung um die Protokollaufzeichnung oder Anwendungsobjekt beim Erstellen von Löschen von Scans. Dies ist ein erwarteter Wartevorgang, wenn Partner ihren Status oder Verbindungsstatus ändern. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_LOGCAPTURE_WAIT |Warten, bis Protokolldatensätze verfügbar werden. Kann beim Warten auf die Erstellung von neuen Protokolldatensätzen durch Verbindungen oder auf den E/A-Abschluss generiert werden, wenn das Lesen des Protokolls nicht im Cache stattfindet. Dies ist ein erwarteter Wartevorgang, wenn der Protokollscan das Ende des Protokolls auf der primären Datenbank erreicht hat oder vom Datenträger liest. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_LOGPROGRESS_SYNC |Parallelitätssteuerungs-Wartevorgang beim Aktualisieren des Protokollstatus von Datenbankreplikaten. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_NOTIFICATION_DEQUEUE |Ein Hintergrundtask, der Windows Server-Failoverclusteringbenachrichtigungen verarbeitet und auf die nächste Benachrichtigung wartet. Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_NOTIFICATION_WORKER_EXCLUSIVE_ACCESS |Der Always on Verfügbarkeits Replikat-Manager wartet auf serialisierten Zugriff auf den Lauf Zeit Status eines Hintergrund Tasks, der Windows Server-Failoverclustering-Benachrichtigungen verarbeitet. Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_NOTIFICATION_WORKER_STARTUP_SYNC |Ein Hintergrundtask wartet auf den Abschluss des Starts eines Hintergrundtasks, der Windows Server-Failoverclusteringbenachrichtigungen verarbeitet. Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_NOTIFICATION_WORKER_TERMINATION_SYNC |Ein Hintergrundtask wartet auf den Abschluss eines Hintergrundtasks, der Windows Server-Failoverclusteringbenachrichtigungen verarbeitet. Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_PARTNER_SYNC |Parallelitätssteuerungs-Wartevorgang für der Partnerliste. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_READ_ALL_NETWORKS |Wartet auf Lese- oder Schreibzugriff für die List mit WSFC-Netzwerken. Nur zur internen Verwendung. Hinweis: die Engine speichert eine Liste mit wsfc-Netzwerken, die in dynamischen Verwaltungs Sichten (z. b. sys.dm_hadr_cluster_networks) verwendet werden, oder um Always on Transact-SQL-Anweisungen zu validieren, die auf wsfc-Netzwerkinformationen verweisen. Diese Liste wird nach dem Start der Engine, den wsfc-bezogenen Benachrichtigungen und dem internen Always on Neustart aktualisiert (z. b. Verlust und Wiederherstellung des wsfc-Quorums). Tasks werden normalerweise blockiert, wenn ein Update in dieser Liste ausgeführt wird. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_RECOVERY_WAIT_FOR_CONNECTION |Warten, bis die sekundäre Datenbank vor dem Ausführen der Wiederherstellung eine Verbindung mit der primären Datenbank herstellt. Dies ist ein erwarteter Wartevorgang, der länger dauern kann, wenn die Verbindung zur primären Datenbank nur langsam aufgebaut wird. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_RECOVERY_WAIT_FOR_UNDO |Die Datenbankwiederherstellung wartet, bis die sekundäre Datenbank die Wiederherstellungs- und Initialisierungsphase beendet hat, um diese mit der primären Datenbank auf den allgemeinen Protokollpunkt zurückzubringen. Dies ist ein erwarteter warte Vorgang nach einem Failover. Der Rückgängig-Status kann über den Windows-System Monitor (perfmon.exe) und dynamische Verwaltungs Sichten nachverfolgt werden. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_REPLICAINFO_SYNC |Warten, bis die Parallelitätssteuerung den aktuellen Replikatstatus aktualisiert. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_SEEDING_CANCELLATION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|HADR_SEEDING_FILE_LIST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|HADR_SEEDING_LIMIT_BACKUPS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|HADR_SEEDING_SYNC_COMPLETION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|HADR_SEEDING_TIMEOUT_TASK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|HADR_SEEDING_WAIT_FOR_COMPLETION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|HADR_SYNC_COMMIT |Warten auf die Transaktionscommitverarbeitung, bis die synchronisierten sekundären Datenbanken das Protokoll härten. Dieser Wartevorgang wird auch durch den Leistungsindikator für die Transaktionsverzögerung wiedergegeben. Dieser Wartetyp wird für synchronisierte Verfügbarkeitsgruppen erwartet und gibt die Zeit zum Senden, Schreiben und Bestätigen des Protokolls an die sekundären Datenbanken an. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_SYNCHRONIZING_THROTTLE |Warten, bis die Transaktionscommitverarbeitung erlaubt, dass eine synchronisierende sekundäre Datenbank das primäre Ende des Protokolls erreicht, um in den synchronisierten Status überzugehen. Dies ist ein erwarteter Wartevorgang, wenn eine sekundäre Datenbank im Rückstand ist. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_TDS_LISTENER_SYNC |Entweder das interne Always on System oder der wsfc-Cluster fordert an, dass Listener gestartet oder beendet werden. Die Verarbeitung dieser Anforderung ist immer asynchron, und es gibt einen Mechanismus zum Entfernen von redundanten Anforderungen. Es gibt auch Momente, in denen dieser Prozess aufgrund von Konfigurationsänderungen angehalten wird. Alle Wartevorgänge, die mit diesem Listener-Synchronisierungsmechanismus verknüpft sind, verwenden diesen Wartetyp. Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_TDS_LISTENER_SYNC_PROCESSING |Wird am Ende einer Always on Transact-SQL-Anweisung verwendet, die einen verfügbarkeitsgruppenlistener starten und/oder beenden muss. Da der Start-/Beendigungsvorgang asynchron erfolgt, wird der Benutzerthread mit diesem Wartetyp blockiert, bis die Situation des Listeners bekannt ist. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_THROTTLE_LOG_RATE_GOVERNOR |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|HADR_THROTTLE_LOG_RATE_MISMATCHED_SLO | Tritt auf, wenn eine sekundäre georeplikation mit niedrigerer computegröße (niedrigerer SLO) als die primäre Datenbank konfiguriert ist. Eine primäre Datenbank wird aufgrund eines verzögerten Protokoll Verbrauchs durch die sekundäre Datenbank gedrosselt. Dies wird dadurch verursacht, dass die sekundäre Datenbank nicht über genügend computekapazität verfügt, um mit der Änderungs Rate der primären Datenbank Schritt zu halten. <br /><br /> **Gilt für**: Azure SQL-Datenbank| 
|HADR_THROTTLE_LOG_RATE_LOG_SIZE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|HADR_THROTTLE_LOG_RATE_SEEDING |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|HADR_THROTTLE_LOG_RATE_SEND_RECV_QUEUE_SIZE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|HADR_TIMER_TASK |Warten auf die Sperre für das Zeitgebertaskobjekt; wird auch für die tatsächlichen Wartevorgänge zwischen Zeiten verwendet, in denen Arbeiten ausgeführt werden. Beispielsweise wird für eine Aufgabe, die alle 10 Sekunden nach einer Ausführung ausgeführt wird, Always on Verfügbarkeits Gruppe etwa 10 Sekunden auf das erneute Planen der Aufgabe warten, und der Warte Vorgang ist hier enthalten. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_TRANSPORT_DBRLIST |Warten auf Zugriff auf die Datenbankreplikatliste der Transportschicht. Wird für den Spinlock verwendet, der Zugriff darauf gewährt. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_TRANSPORT_FLOW_CONTROL |Warten, wenn sich die Anzahl der ausstehenden nicht bestätigten Always on Nachrichten über dem Schwellenwert für die ausgehenden Datenflusssteuerung befindet. Dies erfolgt auf einer Replikat-zu-Replikat-Basis (nicht auf einer Datenbank-zu-Datenbank-Basis). <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_TRANSPORT_SESSION |Always on Verfügbarkeits Gruppen warten, während Sie den zugrunde liegenden Transport Status ändern oder darauf zugreifen. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_WORK_POOL |Parallelitäts Steuerungs-warte Vorgang auf das Always on Verfügbarkeits Gruppen Hintergrund Arbeitsaufgaben Objekt. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_WORK_QUEUE |Always on Verfügbarkeits Gruppen hintergrundworkerthread, der auf die Zuweisung neuer Aufgaben wartet. Dies ist ein erwarteter Wartevorgang, wenn es Arbeitsthreads gibt, die auf neue Arbeit warten (normaler Status). <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HADR_XRF_STACK_ACCESS |Zugreifen auf (suchen, hinzufügen und löschen) des erweiterten Wiederherstellungs Verzweigungs Stapels für eine Always on Verfügbarkeits Datenbank. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HCCO_CACHE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|HK_RESTORE_FILEMAP |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|HKCS_PARALLEL_MIGRATION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|HKCS_PARALLEL_RECOVERY |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|Htbuild |Tritt auf, wenn beim Synchronisieren der Erstellung der Hash Tabelle auf der Eingabe Seite eines Hashjoins/einer Aggregation paralleler Batches im Batch Modus ausgeführt werden. Wenn die Wartezeit zu lang ist und durch eine Abfrageoptimierung (beispielsweise durch das Hinzufügen von Indizes) nicht verkürzt werden kann, sollten Sie erwägen, den Kostenschwellenwert für Parallelität anzupassen oder den Grad an Parallelität zu verringern. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|Htdelete |Tritt auf, wenn beim Synchronisieren am Ende eines Hashjoins/einer-Aggregation parallele Batches im Batch Modus ausgeführt werden. Wenn die Wartezeit zu lang ist und durch eine Abfrageoptimierung (beispielsweise durch das Hinzufügen von Indizes) nicht verkürzt werden kann, sollten Sie erwägen, den Kostenschwellenwert für Parallelität anzupassen oder den Grad an Parallelität zu verringern. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|Htmemo |Tritt auf, wenn bei der Synchronisierung paralleler Batches im Batch Modus ausgeführt werden, bevor die Hash Tabelle gescannt wird, um Übereinstimmungen/nicht-Übereinstimmungen in einem HashJoin Wenn die Wartezeit zu lang ist und durch eine Abfrageoptimierung (beispielsweise durch das Hinzufügen von Indizes) nicht verkürzt werden kann, sollten Sie erwägen, den Kostenschwellenwert für Parallelität anzupassen oder den Grad an Parallelität zu verringern. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|Htreinit |Tritt bei der Synchronisierung mit parallelen batchmodusplänen auf, bevor ein Hashjoin/Aggregation für den nächsten Teil Join zurückgesetzt wird. Wenn die Wartezeit zu lang ist und durch eine Abfrageoptimierung (beispielsweise durch das Hinzufügen von Indizes) nicht verkürzt werden kann, sollten Sie erwägen, den Kostenschwellenwert für Parallelität anzupassen oder den Grad an Parallelität zu verringern. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|Htrepartition |Tritt beim Synchronisieren der Neupartitionierung der Hash Tabelle auf der Eingabe Seite eines Hashjoins/der Aggregation auf, wenn parallele Batches im Batch Modus ausgeführt werden. Wenn die Wartezeit zu lang ist und durch eine Abfrageoptimierung (beispielsweise durch das Hinzufügen von Indizes) nicht verkürzt werden kann, sollten Sie erwägen, den Kostenschwellenwert für Parallelität anzupassen oder den Grad an Parallelität zu verringern.<br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|HTTP_ENUMERATION |Tritt beim Starten auf, wenn die HTTP-Endpunkte zum Starten von HTTP aufgezählt werden sollen.| 
|HTTP_START |Tritt auf, wenn eine Verbindung auf den Abschluss der HTTP-Initialisierung wartet.| 
|HTTP_STORAGE_CONNECTION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|IMPPROV_IOWAIT |Tritt auf, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf das Fertigstellen eines E/A-Massenladevorgang wartet.| 
|INSTANCE_LOG_RATE_GOVERNOR |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|INTERNAL_TESTING |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|IO_AUDIT_MUTEX |Tritt während der Synchronisierung von Ereignispuffern der Ablaufverfolgung auf.| 
|IO_COMPLETION |Tritt auf, während auf den Abschluss von E/A-Vorgängen gewartet wird. Dieser Wartetyp stellt in der Regel Nicht-Datenseiten-E/A-Vorgänge dar. E/a-Abschluss Wartezeiten für Datenseiten werden als PAGEIOLATCH-warte Vorgänge angezeigt \_ \* .| 
|IO_QUEUE_LIMIT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|IO_RETRY |Tritt auf, wenn ein E/A-Vorgang, z. B. das Lesen von oder Schreiben auf einen Datenträger, aufgrund unzureichender Ressourcen scheitert und dann wiederholt wird.| 
|IOAFF_RANGE_QUEUE |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|KSOURCE_WAKEUP |Wird vom Dienststeuerungstask verwendet, während auf Anforderungen vom Dienstkontroll-Manager gewartet wird. Lange Wartezeiten werden erwartet und zeigen kein Problem an.| 
|KTM_ENLISTMENT |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|KTM_RECOVERY_MANAGER |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|KTM_RECOVERY_RESOLUTION |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|LATCH_DT |Tritt beim Warten auf einen DT-Latch (Löschlatch) auf. Dazu gehören keine Pufferlatches oder Transaktionsmarkierungslatches. Eine Auflistung von Latchwartezeiten \_ \* ist in sys.dm_os_latch_stats verfügbar. Beachten Sie, dass sys.dm_os_latch_stats die Wartevorgänge LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX und LATCH_DT in einer Gruppe zusammenfasst.| 
|LATCH_EX |Tritt beim Warten auf einen EX-Latch (exklusiven Latch) auf. Dazu gehören keine Pufferlatches oder Transaktionsmarkierungslatches. Eine Auflistung von Latchwartezeiten \_ \* ist in sys.dm_os_latch_stats verfügbar. Beachten Sie, dass sys.dm_os_latch_stats die Wartevorgänge LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX und LATCH_DT in einer Gruppe zusammenfasst.| 
|LATCH_KP |Tritt beim Warten auf einen KP-Latch (Beibehaltungslatch) auf. Dazu gehören keine Pufferlatches oder Transaktionsmarkierungslatches. Eine Auflistung von Latchwartezeiten \_ \* ist in sys.dm_os_latch_stats verfügbar. Beachten Sie, dass sys.dm_os_latch_stats die Wartevorgänge LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX und LATCH_DT in einer Gruppe zusammenfasst.| 
|LATCH_NL |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|LATCH_SH |Tritt beim Warten auf einen SH-Latch (gemeinsamen Latch) auf. Dazu gehören keine Pufferlatches oder Transaktionsmarkierungslatches. Eine Auflistung von Latchwartezeiten \_ \* ist in sys.dm_os_latch_stats verfügbar. Beachten Sie, dass sys.dm_os_latch_stats die Wartevorgänge LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX und LATCH_DT in einer Gruppe zusammenfasst.| 
|LATCH_UP |Tritt beim Warten auf einen UP-Latch (Updatelatch) auf. Dazu gehören keine Pufferlatches oder Transaktionsmarkierungslatches. Eine Auflistung von Latchwartezeiten \_ \* ist in sys.dm_os_latch_stats verfügbar. Beachten Sie, dass sys.dm_os_latch_stats die Wartevorgänge LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX und LATCH_DT in einer Gruppe zusammenfasst.| 
|LAZYWRITER_SLEEP |Tritt auf, wenn Lazy Writer-Tasks angehalten werden. Dieser Wartetyp gibt die Zeitdauer von wartenden Hintergrundtasks an. Wenn Sie nach durch den Benutzer bedingtem Hängen des Computers suchen, sollten Sie diesen Status nicht verwenden.| 
|LCK_M_BU |Tritt auf, wenn ein Task darauf wartet, eine Massenupdatesperre (BU-Sperre) abzurufen. Weitere Informationen finden Sie unter [Massen Update sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#bulk_update) .| 
|LCK_M_BU_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine Massenupdatesperre (BU-Sperre) mit Abbruchs-Blockern abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter [Massen Update sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#bulk_update) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_BU_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine Massenupdatesperre (BU-Sperre) mit niedriger Priorität abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter [Massen Update sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#bulk_update) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_IS |Tritt auf, wenn ein Task darauf wartet, eine beabsichtigte gemeinsame Sperre (IS-Sperre) abzurufen. Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent)| 
|LCK_M_IS_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine beabsichtigte gemeinsame Sperre (IS-Sperre) mit Abbruchs-Blockern abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.) Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_IS_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine beabsichtigte gemeinsame Sperre (IS-Sperre) mit niedriger Priorität abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_IU |Tritt auf, wenn ein Task darauf wartet, eine beabsichtigte Updatesperre (IU-Sperre) abzurufen. Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent)| 
|LCK_M_IU_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine beabsichtigte Updatesperre (IU-Sperre) mit Abbruchs-Blockern abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.) Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_IU_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine beabsichtigte Updatesperre (IU-Sperre) mit niedriger Priorität abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.). Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_IX |Tritt auf, wenn ein Task darauf wartet, eine beabsichtigte exklusive Sperre (IX-Sperre) abzurufen. Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent)| 
|LCK_M_IX_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine beabsichtigte exklusive Sperre (IX-Sperre) mit Abbruchs-Blockern abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_IX_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine beabsichtigte exklusive Sperre (IX-Sperre) mit niedriger Priorität abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RIn_NL |Tritt auf, wenn ein Task darauf wartet, eine NULL-Sperre für den aktuellen Schlüsselwert und eine Einfügungssperre zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. Eine NULL-Sperre für den Schlüssel ist eine Sperre, die sofort aufgehoben wird.| 
|LCK_M_RIn_NL_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine NULL-Sperre mit Abbruchs-Blockern für den aktuellen Schlüsselwert und eine Einfügungssperre mit Abbruchs-Blockern zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. Eine NULL-Sperre für den Schlüssel ist eine Sperre, die sofort aufgehoben wird. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RIn_NL_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine NULL-Sperre mit niedriger Priorität für den aktuellen Schlüsselwert und eine Einfügungssperre mit niedriger Priorität zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. Eine NULL-Sperre für den Schlüssel ist eine Sperre, die sofort aufgehoben wird. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RIn_S |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre für den aktuellen Schlüsselwert und eine Einfügungssperre zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen.| 
|LCK_M_RIn_S_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre mit Abbruchs-Blockern für den aktuellen Schlüsselwert und eine Einfügungssperre mit Abbruchs-Blockern zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RIn_S_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre mit niedriger Priorität für den aktuellen Schlüsselwert und eine Einfügungssperre mit niedriger Priorität zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RIn_U |Ein Task wartet darauf, eine Updatesperre für den aktuellen Schlüsselwert und eine Einfügungssperre zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen.| 
|LCK_M_RIn_U_ABORT_BLOCKERS |Task wartet darauf, eine Updatesperre mit Abbruchs-Blockern für den aktuellen Schlüsselwert und eine Einfügungssperre mit Abbruchs-Blockern zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RIn_U_LOW_PRIORITY |Task wartet darauf, eine Updatesperre mit niedriger Priorität für den aktuellen Schlüsselwert und eine Einfügungssperre mit niedriger Priorität zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RIn_X |Tritt auf, wenn ein Task darauf wartet, eine exklusive Sperre für den aktuellen Schlüsselwert und eine Einfügungssperre zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen.| 
|LCK_M_RIn_X_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine exklusive Sperre mit Abbruchs-Blockern für den aktuellen Schlüsselwert und eine Einfügungssperre mit Abbruchs-Blockern zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RIn_X_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine exklusive Sperre mit niedriger Priorität für den aktuellen Schlüsselwert und eine Einfügungssperre mit niedriger Priorität zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RS_S |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre für den aktuellen Schlüsselwert und eine freigegebene Bereichssperre zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen.| 
|LCK_M_RS_S_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre mit Abbruchs-Blockern für den aktuellen Schlüsselwert und eine freigegebene Bereichssperre mit Abbruchs-Blockern zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RS_S_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre mit niedriger Priorität für den aktuellen Schlüsselwert und eine freigegebene Bereichssperre mit niedriger Priorität zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RS_U |Tritt auf, wenn ein Task darauf wartet, eine Updatesperre für den aktuellen Schlüsselwert und eine Bereichsupdatesperre zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen.| 
|LCK_M_RS_U_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine Updatesperre mit Abbruchs-Blockern für den aktuellen Schlüsselwert und eine Updatebereichssperre mit Abbruchs-Blockern zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RS_U_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine exklusive Sperre mit niedriger Priorität für den aktuellen Schlüsselwert und eine Einfügungssperre mit niedriger Priorität zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RX_S |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre für den aktuellen Schlüsselwert und eine exklusive Bereichssperre zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen.| 
|LCK_M_RX_S_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre mit Abbruchs-Blockern für den aktuellen Schlüsselwert und eine exklusive Bereichssperre mit Abbruchs-Blockern zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RX_S_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre mit niedriger Priorität für den aktuellen Schlüsselwert und eine exklusive Bereichssperre mit niedriger Priorität zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RX_U |Tritt auf, wenn ein Task darauf wartet, eine Updatesperre für den aktuellen Schlüsselwert und eine exklusive Bereichssperre zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen.| 
|LCK_M_RX_U_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine Updatesperre mit Abbruchs-Blockern für den aktuellen Schlüsselwert und eine exklusive Bereichssperre mit Abbruchs-Blockern zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RX_U_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine Updatesperre mit niedriger Priorität für den aktuellen Schlüsselwert und eine exklusive Bereichssperre mit niedriger Priorität zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RX_X |Tritt auf, wenn ein Task darauf wartet, eine exklusive Sperre für den aktuellen Schlüsselwert und eine exklusive Bereichssperre zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen.| 
|LCK_M_RX_X_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine exklusive Sperre mit Abbruchs-Blockern für den aktuellen Schlüsselwert und eine exklusive Bereichssperre mit Abbruchs-Blockern zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_RX_X_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine exklusive Sperre mit niedriger Priorität für den aktuellen Schlüsselwert und eine exklusive Bereichssperre mit niedriger Priorität zwischen dem aktuellen und dem vorherigen Schlüssel abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index.), <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_S |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre abzurufen. Weitere Informationen finden Sie unter frei [gegebene Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#shared) .| 
|LCK_M_S_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre mit Abbruchs-Blockern abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter frei [gegebene Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#shared) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_S_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre mit niedriger Priorität abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter frei [gegebene Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#shared) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_SCH_M |Tritt auf, wenn ein Task darauf wartet, eine Schemaänderungssperre abzurufen. Weitere Informationen finden Sie unter [Schema Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#schema) .| 
|LCK_M_SCH_M_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine Schemaänderungssperre Abbruchs-Blockern abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter [Schema Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#schema) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_SCH_M_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine Schemaänderungssperre mit niedriger Priorität abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter [Schema Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#schema) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_SCH_S |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Schemasperre abzurufen. Weitere Informationen finden Sie unter [Schema Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#schema) .| 
|LCK_M_SCH_S_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Schemasperre Abbruchs-Blockern abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter [Schema Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#schema) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_SCH_S_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Schemasperre mit niedriger Priorität abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index) Weitere Informationen finden Sie unter [Schema Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#schema) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_SIU |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre mit beabsichtigter Updatesperre abzurufen. Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) .| 
|LCK_M_SIU_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre mit beabsichtigter Updatesperre mit Abbruchs-Blockern abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_SIU_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre mit beabsichtigter Updatesperre mit niedriger Priorität abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_SIX |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre mit beabsichtigter exklusiver Sperre abzurufen. Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) .| 
|LCK_M_SIX_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre mit beabsichtigter exklusiver Sperre mit Abbruchs-Blockern abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_SIX_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine gemeinsame Sperre mit beabsichtigter exklusiver Sperre mit niedriger Priorität abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_U |Tritt auf, wenn ein Task darauf wartet, eine Updatesperre abzurufen. Weitere Informationen finden Sie unter [Update sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#update) .| 
|LCK_M_U_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine Updatesperre mit Abbruchs-Blockern abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter [Update sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#update) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_U_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine Updatesperre mit niedriger Priorität abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter [Update sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#update) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_UIX |Tritt auf, wenn ein Task darauf wartet, eine Updatesperre mit beabsichtigter exklusiver Sperre abzurufen. Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) .| 
|LCK_M_UIX_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine Updatesperre mit beabsichtigter exklusiver Sperre mit Abbruchs-Blockern abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_UIX_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine Updatesperre mit beabsichtigter exklusiver Sperre mit niedriger Priorität abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter beabsichtigte [Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#intent) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_X |Tritt auf, wenn ein Task darauf wartet, eine exklusive Sperre abzurufen. Weitere Informationen finden Sie unter [exklusive Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#exclusive) .| 
|LCK_M_X_ABORT_BLOCKERS |Tritt auf, wenn ein Task darauf wartet, eine exklusive Sperre mit Abbruchs-Blockern abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter [exklusive Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#exclusive) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LCK_M_X_LOW_PRIORITY |Tritt auf, wenn ein Task darauf wartet, eine exklusive Sperre mit niedriger Priorität abzurufen. (Im Zusammenhang mit der Wait-Option mit niedriger Priorität von ALTER TABLE und Alter Index). Weitere Informationen finden Sie unter [exklusive Sperren](../sql-server-transaction-locking-and-row-versioning-guide.md#exclusive) . <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|LOG_POOL_SCAN |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|LOG_RATE_GOVERNOR |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|LOGBUFFER |Tritt auf, wenn ein Task auf Speicherplatz im Protokollpuffer zum Speichern eines Protokolldatensatzes wartet. Durchgehend hohe Werte können darauf hinweisen, dass die Protokollgeräte die Menge der vom Server generierten Protokolldaten nicht bewältigen können.| 
|LOGCAPTURE_LOGPOOLTRUNCPOINT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|LOGGENERATION |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|LOGMGR |Tritt auf, wenn ein Task beim Schließen der Datenbank vor dem Beenden des Protokolls auf das Fertigstellen ausstehender Protokoll-E/A-Vorgänge wartet.| 
|LOGMGR_FLUSH |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|LOGMGR_PMM_LOG |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|LOGMGR_QUEUE |Tritt auf, während der Protokollschreibertask auf Arbeitsanforderungen wartet.| 
|LOGMGR_RESERVE_APPEND |Tritt auf, wenn ein Task darauf wartet, feststellen zu können, ob durch das Abschneiden von Protokollen Speicherplatz freigegeben wird, damit der Task einen neuen Protokolldatensatz schreiben kann. Erwägen Sie, die Größe der Protokolldatei(en) für die betroffene Datenbank zu erhöhen, um diese Wartezeit zu reduzieren.| 
|LOGPOOL_CACHESIZE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|LOGPOOL_CONSUMER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|LOGPOOL_CONSUMERSET |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|LOGPOOL_FREEPOOLS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|LOGPOOL_MGRSET |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|LOGPOOL_REPLACEMENTSET |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|LOGPOOLREFCOUNTEDOBJECT_REFDONE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|LOWFAIL_MEMMGR_QUEUE |Tritt auf, während darauf gewartet wird, dass Arbeitsspeicher zur Verwendung verfügbar ist.| 
|MD_AGENT_YIELD |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|MD_LAZYCACHE_RWLOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|MEMORY_ALLOCATION_EXT |Tritt beim belegen von Speicher aus dem internen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Speicherpool oder dem Betriebssystem auf. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|MEMORY_GRANT_UPDATE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|METADATA_LAZYCACHE_RWLOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] nur. |  
|Migrationbuffer |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|MISCELLANEOUS |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|MISCELLANEOUS |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|MSQL_DQ |Tritt auf, wenn ein Task auf das Ende eines verteilten Abfragevorgangs wartet. Dieser Wartetyp wird verwendet, um mögliche MARS-Anwendungsdeadlocks (Multiple Active Result Sets) zu erkennen. Die Wartezeit wird mit dem Ende des Aufrufs der verteilten Abfrage beendet.| 
|MSQL_XACT_MGR_MUTEX |Tritt auf, wenn ein Task darauf wartet, in den Besitz des Sitzungstransaktions-Managers zu gelangen, um einen Transaktionsvorgang auf Sitzungsebene auszuführen.| 
|MSQL_XACT_MUTEX |Tritt während der Synchronisierung der Transaktionsverwendung auf. Eine Anforderung muss den Mutex abrufen, bevor sie die Transaktion verwenden kann.| 
|MSQL_XP |Tritt auf, wenn ein Task auf das Ende einer erweiterten gespeicherten Prozedur wartet. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet diesen Wartestatus, um mögliche MARS-Anwendungsdeadlocks zu erkennen. Die Wartezeit wird mit dem Ende des Aufrufs der erweiterten gespeicherten Prozedur beendet.| 
|MSSEARCH |Tritt während Aufrufen der Volltextsuche auf. Diese Wartezeit endet, wenn der Volltextvorgang abgeschlossen ist. Sie zeigt keinen Konflikt an, sondern die Dauer von Volltextvorgängen.| 
|NET_WAITFOR_PACKET |Tritt auf, wenn eine Verbindung während eines Netzwerklesevorgangs auf ein Netzwerkpaket wartet.| 
|Networksxmlmgrload |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|NODE_CACHE_MUTEX |Nur zur internen Verwendung.| 
|OLEDB |Tritt auf, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] den SNAC-OLE DB Anbieter (SQLNCLI) oder den Microsoft OLE DB-Treiber für SQL Server (msoledbsql) aufruft. Dieser Wartetyp wird nicht für die Synchronisierung verwendet. Er zeigt vielmehr die Dauer von Aufrufen des OLE DB-Anbieters an.| 
|ONDEMAND_TASK_QUEUE |Tritt auf, während ein Hintergrundtask auf Systemtaskanforderungen mit hoher Priorität wartet. Lange Wartezeiten zeigen an, dass keine Anforderungen mit hoher Priorität zu verarbeiten waren, und sollten kein Problem darstellen.| 
|PAGEIOLATCH_DT |Tritt auf, wenn ein Task auf einen Latch für einen Puffer in einer E/A-Anforderung wartet. Die Latchanforderung erfolgt im Löschmodus. Lange Wartezeiten können Probleme mit dem Datenträgersubsystem anzeigen.| 
|PAGEIOLATCH_EX |Tritt auf, wenn ein Task auf einen Latch für einen Puffer in einer E/A-Anforderung wartet. Die Latchanforderung erfolgt im exklusiven Modus-ein Modus, der verwendet wird, wenn der Puffer auf den Datenträger geschrieben wird. Lange Wartezeiten können Probleme mit dem Datenträgersubsystem anzeigen. <br /><br /> Weitere Informationen finden Sie in diesem [SQL Server Blog zu langsamen e/a-Problemen](https://techcommunity.microsoft.com/t5/sql-server-support/slow-i-o-sql-server-and-disk-i-o-performance/ba-p/333983) .| 
|PAGEIOLATCH_KP |Tritt auf, wenn ein Task auf einen Latch für einen Puffer in einer E/A-Anforderung wartet. Die Latchanforderung erfolgt im Beibehaltungsmodus. Lange Wartezeiten können Probleme mit dem Datenträgersubsystem anzeigen.| 
|PAGEIOLATCH_NL |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|PAGEIOLATCH_SH |Tritt auf, wenn ein Task auf einen Latch für einen Puffer in einer E/A-Anforderung wartet. Die Latchanforderung erfolgt im freigegebenen Modus-ein Modus, der verwendet wird, wenn der Puffer von der Festplatte gelesen wird. Lange Wartezeiten können Probleme mit dem Datenträgersubsystem anzeigen.<br /><br /> Weitere Informationen finden Sie in diesem [SQL Server Blog zu langsamen e/a-Problemen](https://techcommunity.microsoft.com/t5/sql-server-support/slow-i-o-sql-server-and-disk-i-o-performance/ba-p/333983) .| 
|PAGEIOLATCH_UP |Tritt auf, wenn ein Task auf einen Latch für einen Puffer in einer E/A-Anforderung wartet. Die Latchanforderung erfolgt im Updatemodus. Lange Wartezeiten können Probleme mit dem Datenträgersubsystem anzeigen.<br /><br /> Weitere Informationen finden Sie in diesem [SQL Server Blog zu langsamen e/a-Problemen](https://techcommunity.microsoft.com/t5/sql-server-support/slow-i-o-sql-server-and-disk-i-o-performance/ba-p/333983) .| 
|PAGELATCH_DT |Tritt auf, wenn ein Task auf einen Latch für einen Puffer außerhalb einer E/A-Anforderung wartet. Die Latchanforderung erfolgt im Löschmodus. Der zerstörungsmodus muss vor dem Löschen des Inhalts einer Seite abgerufen werden. Weitere Informationen finden Sie unter [latchmodi](../diagnose-resolve-latch-contention.md#sql-server-latch-modes-and-compatibility) .| 
|PAGELATCH_EX |Tritt auf, wenn ein Task auf einen Latch für einen Puffer außerhalb einer E/A-Anforderung wartet. Die Latchanforderung erfolgt im exklusiven Modus-Sie blockiert das Schreiben oder Lesen von anderen Threads in die Seite (Buffer). </br></br> Ein gängiges Szenario, das zu diesem Latch führt, ist der Puffer Latchkonflikt der letzten Seite. Um dieses Problem zu verstehen und zu beheben, verwenden Sie die [PAGELATCH_EX Konflikt mit der letzten Seite auflösen](/troubleshoot/sql/performance/resolve-pagelatch-ex-contention) , und [diagnostizieren und beheben Sie die Latchkonflikte der letzten Seiten Einfügung auf SQL Server](../diagnose-resolve-latch-contention.md#last-pagetrailing-page-insert-contention). Ein anderes Szenario sind [Latchkonflikte bei kleinen Tabellen mit einem nicht gruppierten Index und zufälligen Einfügungen (Warteschlangen Tabelle)](../diagnose-resolve-latch-contention.md#latch-contention-on-small-tables-with-a-non-clustered-index-and-random-inserts-queue-table).| 
|PAGELATCH_KP |Tritt auf, wenn ein Task auf einen Latch für einen Puffer außerhalb einer E/A-Anforderung wartet. Die Latchanforderung befindet sich im Modus "beibehalten", wodurch verhindert wird, dass die Seite von einem anderen Thread zerstört wird. Weitere Informationen finden Sie unter [latchmodi](../diagnose-resolve-latch-contention.md#sql-server-latch-modes-and-compatibility) .| 
|PAGELATCH_NL |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|PAGELATCH_SH |Tritt auf, wenn ein Task auf einen Latch für einen Puffer außerhalb einer E/A-Anforderung wartet. Die Latchanforderung erfolgt im freigegebenen Modus, sodass mehrere Threads einen Puffer (Seite) lesen, aber nicht ändern können. Weitere Informationen finden Sie unter [latchmodi](../diagnose-resolve-latch-contention.md#sql-server-latch-modes-and-compatibility) .| 
|PAGELATCH_UP |Tritt auf, wenn ein Task auf einen Latch für einen Puffer außerhalb einer E/A-Anforderung wartet. Die Latchanforderung erfolgt im Updatemodus. Dieser Wartetyp kann häufig beobachtet werden, wenn eine System Seite (Puffer) wie PFS, GAM, SGAM in einem Latchzustand ist. Weitere Informationen finden Sie unter [latchmodi](../diagnose-resolve-latch-contention.md#sql-server-latch-modes-and-compatibility) . </br></br> Informationen zur Problembehandlung für ein häufiges Szenario mit diesem Latch finden Sie unter [reduzieren von Zuweisungs Konflikten in SQL Server tempdb-Datenbank](/troubleshoot/sql/performance/recommendations-reduce-allocation-contention).| 
|PARALLEL_BACKUP_QUEUE |Tritt beim Serialisieren der Ausgabe auf, die von RESTORE HEADERONLY, RESTORE FILELISTONLY oder RESTORE LABELONLY erstellt wurde.| 
|PARALLEL_REDO_DRAIN_WORKER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PARALLEL_REDO_FLOW_CONTROL |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PARALLEL_REDO_LOG_CACHE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PARALLEL_REDO_TRAN_LIST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PARALLEL_REDO_TRAN_TURN |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PARALLEL_REDO_WORKER_SYNC |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PARALLEL_REDO_WORKER_WAIT_WORK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PERFORMANCE_COUNTERS_RWLOCK |Nur zur internen Verwendung.| 
|PHYSICAL_SEEDING_DMV |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|POOL_LOG_RATE_GOVERNOR |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PREEMPTIVE_ABR |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|PREEMPTIVE_AUDIT_ACCESS_EVENTLOG |Tritt auf, wenn das Zeitplanungsmodul für das [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] -Betriebssystem (SQLOS) in den präemptiven Modus wechselt, um ein Überwachungsereignis in das Windows-Ereignisprotokoll zu schreiben. <br /><br /> **Gilt für**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] nur. |  
|PREEMPTIVE_AUDIT_ACCESS_SECLOG |Tritt auf, wenn das SQLOS-Zeitplanungsmodul in den präemptiven Modus wechselt, um ein Überwachungsereignis in das Windows-Sicherheitsprotokoll zu schreiben. <br /><br /> **Gilt für**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] nur. |  
|PREEMPTIVE_CLOSEBACKUPMEDIA |Tritt auf, wenn das SQLOS-Zeitplanungsmodul in den präemptiven Modus wechselt, um Sicherungsmedien zu schließen.| 
|PREEMPTIVE_CLOSEBACKUPTAPE |Tritt auf, wenn das SQLOS-Zeitplanungsmodul in den präemptiven Modus wechselt, um ein Bandsicherungsmedium zu schließen.| 
|PREEMPTIVE_CLOSEBACKUPVDIDEVICE |Tritt auf, wenn das SQLOS-Zeitplanungsmodul in den präemptiven Modus wechselt, um ein virtuelles Sicherungsmedium zu schließen.| 
|PREEMPTIVE_CLUSAPI_CLUSTERRESOURCECONTROL |Tritt auf, wenn das SQLOS-Zeitplanungsmodul in den präemptiven Modus wechselt, um Windows-Failoverclustervorgänge auszuführen.| 
|PREEMPTIVE_COM_COCREATEINSTANCE |Tritt auf, wenn das SQLOS-Zeitplanungsmodul in den präemptiven Modus wechselt, um ein COM-Objekt zu erstellen.| 
|PREEMPTIVE_COM_COGETCLASSOBJECT |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_CREATEACCESSOR |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_DELETEROWS |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_GETCOMMANDTEXT |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_GETDATA |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_GETNEXTROWS |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_GETRESULT |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_GETROWSBYBOOKMARK |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_LBFLUSH |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_LBLOCKREGION |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_LBREADAT |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_LBSETSIZE |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_LBSTAT |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_LBUNLOCKREGION |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_LBWRITEAT |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_QUERYINTERFACE |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_RELEASE |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_RELEASEACCESSOR |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_RELEASEROWS |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_RELEASESESSION |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_RESTARTPOSITION |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_SEQSTRMREAD |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_SEQSTRMREADANDWRITE |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_SETDATAFAILURE |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_SETPARAMETERINFO |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_SETPARAMETERPROPERTIES |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_STRMLOCKREGION |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_STRMSEEKANDREAD |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_STRMSEEKANDWRITE |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_STRMSETSIZE |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_STRMSTAT |Nur zur internen Verwendung.| 
|PREEMPTIVE_COM_STRMUNLOCKREGION |Nur zur internen Verwendung.| 
|PREEMPTIVE_CONSOLEWRITE |Nur zur internen Verwendung.| 
|PREEMPTIVE_CREATEPARAM |Nur zur internen Verwendung.| 
|PREEMPTIVE_DEBUG |Nur zur internen Verwendung.| 
|PREEMPTIVE_DFSADDLINK |Nur zur internen Verwendung.| 
|PREEMPTIVE_DFSLINKEXISTCHECK |Nur zur internen Verwendung.| 
|PREEMPTIVE_DFSLINKHEALTHCHECK |Nur zur internen Verwendung.| 
|PREEMPTIVE_DFSREMOVELINK |Nur zur internen Verwendung.| 
|PREEMPTIVE_DFSREMOVEROOT |Nur zur internen Verwendung.| 
|PREEMPTIVE_DFSROOTFOLDERCHECK |Nur zur internen Verwendung.| 
|PREEMPTIVE_DFSROOTINIT |Nur zur internen Verwendung.| 
|PREEMPTIVE_DFSROOTSHARECHECK |Nur zur internen Verwendung.| 
|PREEMPTIVE_DTC_ABORT |Nur zur internen Verwendung.| 
|PREEMPTIVE_DTC_ABORTREQUESTDONE |Nur zur internen Verwendung.| 
|PREEMPTIVE_DTC_BEGINTRANSACTION |Nur zur internen Verwendung.| 
|PREEMPTIVE_DTC_COMMITREQUESTDONE |Nur zur internen Verwendung.| 
|PREEMPTIVE_DTC_ENLIST |Nur zur internen Verwendung.| 
|PREEMPTIVE_DTC_PREPAREREQUESTDONE |Nur zur internen Verwendung.| 
|PREEMPTIVE_FILESIZEGET |Nur zur internen Verwendung.| 
|PREEMPTIVE_FSAOLEDB_ABORTTRANSACTION |Nur zur internen Verwendung.| 
|PREEMPTIVE_FSAOLEDB_COMMITTRANSACTION |Nur zur internen Verwendung.| 
|PREEMPTIVE_FSAOLEDB_STARTTRANSACTION |Nur zur internen Verwendung.| 
|PREEMPTIVE_FSRECOVER_UNCONDITIONALUNDO |Nur zur internen Verwendung.| 
|PREEMPTIVE_GETRMINFO |Nur zur internen Verwendung.| 
|PREEMPTIVE_HADR_LEASE_MECHANISM |Always on Verfügbarkeits Gruppen-Lease-Manager-Planung für die Microsoft-Support Diagnose. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PREEMPTIVE_HTTP_EVENT_WAIT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PREEMPTIVE_HTTP_REQUEST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PREEMPTIVE_LOCKMONITOR |Nur zur internen Verwendung.| 
|PREEMPTIVE_MSS_RELEASE |Nur zur internen Verwendung.| 
|PREEMPTIVE_ODBCOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OLE_UNINIT |Nur zur internen Verwendung.| 
|PREEMPTIVE_OLEDB_ABORTORCOMMITTRAN |Nur zur internen Verwendung.| 
|PREEMPTIVE_OLEDB_ABORTTRAN |Nur zur internen Verwendung.| 
|PREEMPTIVE_OLEDB_GETDATASOURCE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OLEDB_GETLITERALINFO |Nur zur internen Verwendung.| 
|PREEMPTIVE_OLEDB_GETPROPERTIES |Nur zur internen Verwendung.| 
|PREEMPTIVE_OLEDB_GETPROPERTYINFO |Nur zur internen Verwendung.| 
|PREEMPTIVE_OLEDB_GETSCHEMALOCK |Nur zur internen Verwendung.| 
|PREEMPTIVE_OLEDB_JOINTRANSACTION |Nur zur internen Verwendung.| 
|PREEMPTIVE_OLEDB_RELEASE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OLEDB_SETPROPERTIES |Nur zur internen Verwendung.| 
|PREEMPTIVE_OLEDBOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_ACCEPTSECURITYCONTEXT |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_ACQUIRECREDENTIALSHANDLE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_AUTHENTICATIONOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_AUTHORIZATIONOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_AUTHZGETINFORMATIONFROMCONTEXT |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_AUTHZINITIALIZECONTEXTFROMSID |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_AUTHZINITIALIZERESOURCEMANAGER |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_BACKUPREAD |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_CLOSEHANDLE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_CLUSTEROPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_COMOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_COMPLETEAUTHTOKEN |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_COPYFILE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_CREATEDIRECTORY |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_CREATEFILE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_CRYPTACQUIRECONTEXT |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_CRYPTIMPORTKEY |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_CRYPTOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_DECRYPTMESSAGE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_DELETEFILE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_DELETESECURITYCONTEXT |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_DEVICEIOCONTROL |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_DEVICEOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_DIRSVC_NETWORKOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_DISCONNECTNAMEDPIPE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_DOMAINSERVICESOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_DSGETDCNAME |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_DTCOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_ENCRYPTMESSAGE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_FILEOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_FINDFILE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_FLUSHFILEBUFFERS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_FORMATMESSAGE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_FREECREDENTIALSHANDLE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_FREELIBRARY |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_GENERICOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_GETADDRINFO |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_GETCOMPRESSEDFILESIZE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_GETDISKFREESPACE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_GETFILEATTRIBUTES |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_GETFILESIZE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_GETFINALFILEPATHBYHANDLE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PREEMPTIVE_OS_GETLONGPATHNAME |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_GETPROCADDRESS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_GETVOLUMENAMEFORVOLUMEMOUNTPOINT |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_GETVOLUMEPATHNAME |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_INITIALIZESECURITYCONTEXT |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_LIBRARYOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_LOADLIBRARY |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_LOGONUSER |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_LOOKUPACCOUNTSID |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_MESSAGEQUEUEOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_MOVEFILE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_NETGROUPGETUSERS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_NETLOCALGROUPGETMEMBERS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_NETUSERGETGROUPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_NETUSERGETLOCALGROUPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_NETUSERMODALSGET |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_NETVALIDATEPASSWORDPOLICY |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_NETVALIDATEPASSWORDPOLICYFREE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_OPENDIRECTORY |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_PDH_WMI_INIT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PREEMPTIVE_OS_PIPEOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_PROCESSOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_QUERYCONTEXTATTRIBUTES |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PREEMPTIVE_OS_QUERYREGISTRY |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_QUERYSECURITYCONTEXTTOKEN |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_REMOVEDIRECTORY |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_REPORTEVENT |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_REVERTTOSELF |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_RSFXDEVICEOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_SECURITYOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_SERVICEOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_SETENDOFFILE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_SETFILEPOINTER |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_SETFILEVALIDDATA |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_SETNAMEDSECURITYINFO |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_SQLCLROPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_SQMLAUNCH |Nur zur internen Verwendung. <br /><br /> **Gilt für**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] bis [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)]. |  
|PREEMPTIVE_OS_VERIFYSIGNATURE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_VERIFYTRUST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PREEMPTIVE_OS_VSSOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_WAITFORSINGLEOBJECT |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_WINSOCKOPS |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_WRITEFILE |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_WRITEFILEGATHER |Nur zur internen Verwendung.| 
|PREEMPTIVE_OS_WSASETLASTERROR |Nur zur internen Verwendung.| 
|PREEMPTIVE_REENLIST |Nur zur internen Verwendung.| 
|PREEMPTIVE_RESIZELOG |Nur zur internen Verwendung.| 
|PREEMPTIVE_ROLLFORWARDREDO |Nur zur internen Verwendung.| 
|PREEMPTIVE_ROLLFORWARDUNDO |Nur zur internen Verwendung.| 
|PREEMPTIVE_SB_STOPENDPOINT |Nur zur internen Verwendung.| 
|PREEMPTIVE_SERVER_STARTUP |Nur zur internen Verwendung.| 
|PREEMPTIVE_SETRMINFO |Nur zur internen Verwendung.| 
|PREEMPTIVE_SHAREDMEM_GETDATA |Nur zur internen Verwendung.| 
|PREEMPTIVE_SNIOPEN |Nur zur internen Verwendung.| 
|PREEMPTIVE_SOSHOST |Nur zur internen Verwendung.| 
|PREEMPTIVE_SOSTESTING |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|PREEMPTIVE_SP_SERVER_DIAGNOSTICS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PREEMPTIVE_STARTRM |Nur zur internen Verwendung.| 
|PREEMPTIVE_STREAMFCB_CHECKPOINT |Nur zur internen Verwendung.| 
|PREEMPTIVE_STREAMFCB_RECOVER |Nur zur internen Verwendung.| 
|PREEMPTIVE_STRESSDRIVER |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|PREEMPTIVE_TESTING |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|PREEMPTIVE_TRANSIMPORT |Nur zur internen Verwendung.| 
|PREEMPTIVE_UNMARSHALPROPAGATIONTOKEN |Nur zur internen Verwendung.| 
|PREEMPTIVE_VSS_CREATESNAPSHOT |Nur zur internen Verwendung.| 
|PREEMPTIVE_VSS_CREATEVOLUMESNAPSHOT |Nur zur internen Verwendung.| 
|PREEMPTIVE_XE_CALLBACKEXECUTE |Nur zur internen Verwendung.| 
|PREEMPTIVE_XE_CX_FILE_OPEN |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|PREEMPTIVE_XE_CX_HTTP_CALL |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|PREEMPTIVE_XE_DISPATCHER |Nur zur internen Verwendung.| 
|PREEMPTIVE_XE_ENGINEINIT |Nur zur internen Verwendung.| 
|PREEMPTIVE_XE_GETTARGETSTATE |Nur zur internen Verwendung.| 
|PREEMPTIVE_XE_SESSIONCOMMIT |Nur zur internen Verwendung.| 
|PREEMPTIVE_XE_TARGETFINALIZE |Nur zur internen Verwendung.| 
|PREEMPTIVE_XE_TARGETINIT |Nur zur internen Verwendung.| 
|PREEMPTIVE_XE_TIMERRUN |Nur zur internen Verwendung.| 
|PREEMPTIVE_XETESTING |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|PRINT_ROLLBACK_PROGRESS |Wird verwendet, um zu warten, während Benutzerprozesse in einer Datenbank beendet werden, die mithilfe der ALTER DATABASE-Beendigungsklausel von einem Status in einen anderen versetzt wurde. Weitere Informationen finden Sie unter ALTER DATABASE (Transact-SQL).| 
|PRU_ROLLBACK_DEFERRED |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_ALL_COMPONENTS_INITIALIZED |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_COOP_SCAN |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_DIRECTLOGCONSUMER_GETNEXT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PWAIT_EVENT_SESSION_INIT_MUTEX |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_FABRIC_REPLICA_CONTROLLER_DATA_LOSS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PWAIT_HADR_ACTION_COMPLETED |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_HADR_CHANGE_NOTIFIER_TERMINATION_SYNC |Tritt auf, wenn ein Hintergrundtask auf die Beendigung des Hintergrundtasks wartet, der (über Abruf) Windows Server-Failoverclusteringbenachrichtigungen empfängt. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_HADR_CLUSTER_INTEGRATION |Ein Anfüge-, Ersetzungs-und/oder Entfernungs Vorgang wartet darauf, eine Schreibsperre für eine Always on interne Liste (z. b. eine Liste von Netzwerken, Netzwerkadressen oder verfügbarkeitsgruppenlistener) zu erfassen. Nur interner Gebrauch, <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_HADR_FAILOVER_COMPLETED |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_HADR_JOIN |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|PWAIT_HADR_OFFLINE_COMPLETED |Ein Always on Drop Availability Group-Vorgang wartet darauf, dass die Ziel Verfügbarkeits Gruppe offline geschaltet wird, bevor Windows Server-Failoverclustering-Objekte zerstört werden. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_HADR_ONLINE_COMPLETED |Ein Always on Vorgang zum Erstellen oder Failover von Verfügbarkeits Gruppen wartet darauf, dass die Ziel Verfügbarkeits Gruppe online geschaltet wird. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_HADR_POST_ONLINE_COMPLETED |Eine Always on Drop Availability Group-Vorgang wartet auf die Beendigung eines Hintergrund Tasks, der als Teil eines vorherigen Befehls geplant wurde. Es darf z. B. einen Hintergrundtask geben, der Verfügbarkeitsdatenbanken an die primäre Rolle übergibt. Die DROP AVAILABILITY GROUP-DDL muss auf den Abschluss dieses Hintergrundtasks warten, um Racebedingungen zu verhindern. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_HADR_SERVER_READY_CONNECTIONS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_HADR_WORKITEM_COMPLETED |Interner Wartevorgang von einem Thread, der auf den Abschluss eines asynchronen Arbeitstasks wartet. Dies ist ein erwarteter Wartevorgang und wird in CSS verwendet. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_HADRSIM |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|PWAIT_LOG_CONSOLIDATION_IO |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|PWAIT_LOG_CONSOLIDATION_POLL |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|PWAIT_MD_LOGIN_STATS |Tritt während der internen Synchronisierung in Metadaten zu Anmeldestatistiken auf. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_MD_RELATION_CACHE |Tritt während der internen Synchronisierung in Metadaten zu Tabelle oder Index auf. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_MD_SERVER_CACHE |Tritt während der internen Synchronisierung in Metadaten zu Verbindungsservern auf. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_MD_UPGRADE_CONFIG |Tritt während der internen Synchronisierung beim Aktualisieren von serverweiten Konfigurationen auf. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_PREEMPTIVE_APP_USAGE_TIMER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|PWAIT_PREEMPTIVE_AUDIT_ACCESS_WINDOWSLOG |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_QRY_BPMEMORY |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_REPLICA_ONLINE_INIT_MUTEX |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_RESOURCE_SEMAPHORE_FT_PARALLEL_QUERY_SYNC |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|PWAIT_SBS_FILE_OPERATION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|PWAIT_XTP_FSSTORAGE_MAINTENANCE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|PWAIT_XTP_HOST_STORAGE_WAIT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_ASYNC_CHECK_CONSISTENCY_TASK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_ASYNC_PERSIST_TASK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_ASYNC_PERSIST_TASK_START |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_ASYNC_QUEUE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|QDS_BCKG_TASK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_BLOOM_FILTER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|QDS_CLEANUP_STALE_QUERIES_TASK_MAIN_LOOP_SLEEP |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_CTXS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_DB_DISK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_DYN_VECTOR |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_EXCLUSIVE_ACCESS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|QDS_HOST_INIT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|QDS_LOADDB |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_PERSIST_TASK_MAIN_LOOP_SLEEP |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_QDS_CAPTURE_INIT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|QDS_SHUTDOWN_QUEUE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_STMT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_STMT_DISK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_TASK_SHUTDOWN |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QDS_TASK_START |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QE_WARN_LIST_SYNC |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|QPJOB_KILL |Weist darauf hin, dass das asynchrone automatische Update der Statistik durch den Aufruf von KILL beim Starten des Updates abgebrochen wurde. Der Abschlussthread wird angehalten, und es wird darauf gewartet, dass er mit dem Überwachen auf KILL-Befehle beginnt. Ein guter Wert liegt unter einer Sekunde.| 
|QPJOB_WAITFOR_ABORT |Weist darauf hin, dass das asynchrone automatische Update der Statistik durch den Aufruf von KILL während des Updates abgebrochen wurde. Das Update wurde jetzt beendet und wird so lange angehalten, bis die Nachrichtenkoordination des Abschlussthreads abgeschlossen ist. Dies ist ein gewöhnlicher, jedoch selten vorkommender Status, der sehr kurz sein sollte. Ein guter Wert liegt unter einer Sekunde.| 
|QRY_MEM_GRANT_INFO_MUTEX |Tritt auf, wenn die Arbeitsspeicherverwaltung für die Abfrageausführung den Zugriff auf statische Listen mit Informationen zu zugewiesenen Arbeitsspeichern steuert. Dieser Status listet Informationen zu den aktuellen zugewiesenen und wartenden Speicheranforderungen auf. Dieser Status ist ein einfacher Zugriffssteuerungsstatus. In diesem Status sollte es nie zu langen Wartezeiten kommen. Wird dieser Mutex nicht freigegeben, antworten alle neuen speicherbeanspruchenden Abfragen nicht mehr.| 
|QRY_PARALLEL_THREAD_MUTEX |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|QRY_PROFILE_LIST_MUTEX |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|QUERY_ERRHDL_SERVICE_DONE |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. <br /><br /> **Gilt für**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] nur. |  
|QUERY_WAIT_ERRHDL_SERVICE |Nur für Informationszwecke identifiziert.  Wird nicht unterstützt. <br /><br /> **Gilt für**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] nur.  |  
|QUERY_EXECUTION_INDEX_SORT_EVENT_OPEN |Tritt in bestimmten Fällen auf, wenn eine Offlineindexerstellung parallel ausgeführt wird und die unterschiedlichen Arbeitsthreads, die die Sortierung ausführen, den Zugriff auf die Sortierungsdateien synchronisieren.| 
|QUERY_NOTIFICATION_MGR_MUTEX |Tritt während der Synchronisierung der Garbage Collection-Warteschlange im Abfragebenachrichtigungs-Manager auf.| 
|QUERY_NOTIFICATION_SUBSCRIPTION_MUTEX |Tritt während der Statussynchronisierung für Transaktionen in Abfragebenachrichtigungen auf.| 
|QUERY_NOTIFICATION_TABLE_MGR_MUTEX |Tritt während der internen Synchronisierung im Abfragebenachrichtigungs-Manager auf.| 
|QUERY_NOTIFICATION_UNITTEST_MUTEX |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|QUERY_OPTIMIZER_PRINT_MUTEX |Tritt während der Synchronisierung der Erstellung der Diagnoseausgabe des Abfrageoptimierers auf. Dieser Wartetyp tritt nur auf, wenn Diagnose Einstellungen Unterrichtung des Microsoft-Produkt Supports aktiviert wurden.| 
|QUERY_TASK_ENQUEUE_MUTEX |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|QUERY_TRACEOUT |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|RBIO_WAIT_VLF |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|RBIO_RG_STORAGE |Tritt auf, wenn ein hyperskalierungsdatenbankcomputeknoten aufgrund von verzögertem Protokoll Verbrauch auf Seiten Servern gedrosselt wird. <br /><br /> **Gilt für**: hyperskalierung von Azure SQL-Datenbank.|
|RBIO_RG_DESTAGE |Tritt auf, wenn ein hyperskalierungsdatenbankcomputeknoten aufgrund von verzögertem Protokoll Verbrauch durch den langfristigen Protokoll Speicher gedrosselt wird. <br /><br /> **Gilt für**: hyperskalierung von Azure SQL-Datenbank.|
|RBIO_RG_REPLICA |Tritt auf, wenn ein hyperskalierungsdatenbankcomputeknoten aufgrund von verzögertem Protokoll Verbrauch durch die lesbaren sekundären Replikat Knoten gedrosselt wird. <br /><br /> **Gilt für**: hyperskalierung von Azure SQL-Datenbank.|
|RBIO_RG_LOCALDESTAGE |Tritt auf, wenn ein hyperskalierungsdatenbankcomputeknoten aufgrund eines verzögerten Protokoll Verbrauchs durch den Protokolldienst gedrosselt wird. <br /><br /> **Gilt für**: hyperskalierung von Azure SQL-Datenbank.|
|RECOVER_CHANGEDB |Tritt während der Synchronisierung des Datenbankstatus in einer Datenbank im betriebsbereiten Standbymodus auf.| 
|RECOVERY_MGR_LOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|REDO_THREAD_PENDING_WORK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|REDO_THREAD_SYNC |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|REMOTE_BLOCK_IO |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|REMOTE_DATA_ARCHIVE_MIGRATION_DMV |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|REMOTE_DATA_ARCHIVE_SCHEMA_DMV |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|REMOTE_DATA_ARCHIVE_SCHEMA_TASK_QUEUE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|REPL_CACHE_ACCESS |Tritt während der Synchronisierung für einen Replikationsartikelcache auf. Während dieser Wartezeiten wird der Replikationsprotokollleser angehalten, und Anweisungen der Datendefinitionssprache (Data Definition Language, DDL) für eine veröffentlichte Tabelle werden blockiert.| 
|REPL_HISTORYCACHE_ACCESS |Nur zur internen Verwendung.| 
|REPL_SCHEMA_ACCESS |Tritt während der Synchronisierung von Versionsinformationen des Replikationsschemas auf. Dieser Status ist vorhanden, wenn DDL-Anweisungen für das replizierte Objekt ausgeführt werden und wenn der Protokollleser ein versionsspezifisches Schema auf der Basis des Auftretens von DDL erstellt oder verwendet. Konflikte können für diesen Wartetyp erkannt werden, wenn Sie über viele veröffentlichte Datenbanken auf einem einzelnen Verleger mit Transaktions Replikation verfügen und die veröffentlichten Datenbanken sehr aktiv sind.| 
|REPL_TRANFSINFO_ACCESS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|REPL_TRANHASHTABLE_ACCESS |Nur zur internen Verwendung.| 
|REPL_TRANTEXTINFO_ACCESS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|REPLICA_WRITES |Tritt auf, während ein Task auf den Abschluss von Seitenschreibvorgängen in Datenbankmomentaufnahmen oder DBCC-Replikaten wartet.| 
|REQUEST_DISPENSER_PAUSE |Tritt auf, wenn ein Task auf den Abschluss aller ausstehenden E/A-Vorgänge wartet, damit E/A-Vorgänge in eine Datei für eine Momentaufnahmesicherung eingefroren werden können.| 
|REQUEST_FOR_DEADLOCK_SEARCH |Tritt auf, während die Deadlocküberwachung darauf wartet, die nächste Deadlocksuche zu starten. Dieser Wartetyp wird zwischen Deadlockerkennungen erwartet, und eine lange Gesamtwartezeit für diese Ressource zeigt kein Problem an.| 
|RESERVED_MEMORY_ALLOCATION_EXT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|RESMGR_THROTTLED |Tritt auf, wenn eine neue Anforderung eingeht und auf der Basis der Einstellung GROUP_MAX_REQUESTS eingeschränkt wird.| 
|RESOURCE_GOVERNOR_IDLE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|RESOURCE_QUEUE |Tritt während der Synchronisierung verschiedener interner Ressourcenwarteschlangen auf.| 
|RESOURCE_SEMAPHORE |Tritt auf, wenn eine Abfrage Speicher Anforderung während der Abfrage Ausführung aufgrund von anderen gleichzeitigen Abfragen nicht sofort erteilt werden kann. Lange Wartevorgänge und Wartezeiten zeigen möglicherweise eine überhöhte Anzahl gleichzeitiger Abfragen oder eine überhöhte Menge angeforderten Arbeitsspeichers an. Übermäßige warte Vorgänge dieses Typs können den SQL- [Fehler 8645](../errors-events/mssqlserver-8645-database-engine-error.md)ergeben, dass beim Warten auf Speicherressourcen zum Ausführen der Abfrage ein Timeout aufgetreten ist. Führen Sie die Abfrage erneut aus. "<br /><br /> Ausführliche Informationen und Informationen zur Problembehandlung bei Wartezeiten für Arbeitsspeicher Zuweisungen finden Sie in diesem [Blogbeitrag](https://techcommunity.microsoft.com/t5/sql-server-support/memory-grants-the-mysterious-sql-server-memory-consumer-with/ba-p/333994) .| 
|RESOURCE_SEMAPHORE_MUTEX |Tritt auf, während eine Abfrage darauf wartet, dass ihre Anforderung einer Threadreservierung erfüllt wird. Der Wartetyp tritt außerdem beim Synchronisieren von Abfragekompilierungs- und Arbeitsspeicherzuweisungsanforderungen auf.| 
|RESOURCE_SEMAPHORE_QUERY_COMPILE |Tritt auf, wenn die Anzahl gleichzeitiger Abfragekompilierungen einen Drosselungsgrenzwert erreicht. Hohe Wartezeiten und Wartezeiten können auf übermäßige Kompilierungen, Neukompilierungen oder nicht zwischen speicherbare Pläne hindeuten.| 
|RESOURCE_SEMAPHORE_SMALL_QUERY |Tritt auf, wenn einer Arbeitsspeicheranforderung einer kleinen Abfrage aufgrund von anderen gleichzeitigen Abfragen nicht sofort entsprochen werden kann. Die Wartezeit sollte wenige Sekunden nicht überschreiten, da der Server die Anforderung an den Hauptspeicherpool für Abfragen überträgt, wenn er den angeforderten Arbeitsspeicher nicht innerhalb weniger Sekunden erteilen kann. Lange Wartezeiten zeigen möglicherweise eine übermäßige Anzahl gleichzeitiger kleiner Abfragen bei gleichzeitiger Blockierung des Hauptspeicherpools durch wartende Abfragen an. <br /><br /> **Gilt für**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] nur. |  
|RESTORE_FILEHANDLECACHE_ENTRYLOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|RESTORE_FILEHANDLECACHE_LOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|RG_RECONFIG |Nur zur internen Verwendung.| 
|ROWGROUP_OP_STATS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|ROWGROUP_VERSION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|RTDATA_LIST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|SATELLITE_CARGO |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|SATELLITE_SERVICE_SETUP |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|SATELLITE_TASK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|SBS_DISPATCH |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|SBS_RECEIVE_TRANSPORT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|SBS_TRANSPORT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|SCAN_CHAR_HASH_ARRAY_INITIALIZATION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SEC_DROP_TEMP_KEY |Tritt nach einem fehlerhaftem Versuch, einen temporären Sicherheitsschlüssel zu löschen, vor einem Wiederholungsversuch auf.| 
|SECURITY_CNG_PROVIDER_MUTEX |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|SECURITY_CRYPTO_CONTEXT_MUTEX |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SECURITY_DBE_STATE_MUTEX |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|SECURITY_KEYRING_RWLOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SECURITY_MUTEX |Tritt bei einem Wartevorgang auf Mutexe auf, die den Zugriff auf die globale Liste von EKM (Extensible Key Management)-Kryptografieanbietern und die Sitzungsbereichsliste der EKM-Sitzungen steuern.| 
|SECURITY_RULETABLE_MUTEX |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SEMPLAT_DSI_BUILD |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SEQUENCE_GENERATION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SEQUENTIAL_GUID |Tritt auf, während eine neue sequenzielle GUID empfangen wird.| 
|SERVER_IDLE_CHECK |Tritt während der Synchronisierung des Leerlaufstatus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz auf, wenn ein Ressourcenmonitor versucht, eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz als im Leerlauf oder in einem Reaktivierungsversuch befindlich zu deklarieren.| 
|SERVER_RECONFIGURE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SESSION_WAIT_STATS_CHILDREN |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|SHARED_DELTASTORE_CREATION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|SHUTDOWN |Tritt auf, während eine Anweisung zum Herunterfahren darauf wartet, dass aktive Verbindungen beendet werden.| 
|SLEEP_BPOOL_FLUSH |Tritt auf, wenn ein Prüfpunkt die Ausgabe neuer E/A-Vorgänge drosselt, um eine Überflutung des Datenträgersubsystems zu vermeiden.| 
|SLEEP_BUFFERPOOL_HELPLW |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|SLEEP_DBSTARTUP |Tritt beim Datenbankstart auf, während darauf gewartet wird, dass alle Datenbanken wiederhergestellt werden.| 
|SLEEP_DCOMSTARTUP |Tritt höchstens einmal beim Start einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz auf, während auf den Abschluss der DCOM-Initialisierung gewartet wird.| 
|SLEEP_MASTERDBREADY |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SLEEP_MASTERMDREADY |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SLEEP_MASTERUPGRADED |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SLEEP_MEMORYPOOL_ALLOCATEPAGES |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|SLEEP_MSDBSTARTUP |Tritt auf, wenn die SQL-Ablaufverfolgung auf den Abschluss des Startvorgangs der msdb-Datenbank wartet.| 
|SLEEP_RETRY_VIRTUALALLOC |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|SLEEP_SYSTEMTASK |Tritt beim Start eines Hintergrundtasks auf, während auf den Abschluss des Startvorgangs von tempdb gewartet wird.| 
|SLEEP_TASK |Tritt auf, wenn ein Task ruht, während auf das Auftreten eines generischen Ereignisses gewartet wird.| 
|SLEEP_TEMPDBSTARTUP |Tritt auf, während ein Task auf den Abschluss des Startvorgangs von tempdb wartet.| 
|SLEEP_WORKSPACE_ALLOCATEPAGE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|SLO_UPDATE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|Smsync |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|SNI_CONN_DUP |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|SNI_CRITICAL_SECTION |Tritt während der internen Synchronisierung innerhalb von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Netzwerkkomponenten auf.| 
|SNI_HTTP_WAITFOR_0_DISCON |Tritt während des Herunterfahrens von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf, während darauf gewartet wird, dass ausstehende HTTP-Verbindungen beendet werden.| 
|SNI_LISTENER_ACCESS |Tritt auf, während darauf gewartet wird, dass nicht einheitliche Speicherzugriffsknoten (Non-Uniform Memory Access, NUMA) die Statusänderung aktualisieren. Der Zugriff auf die Statusänderung wird serialisiert.| 
|SNI_TASK_COMPLETION |Tritt bei einem Wartevorgang auf das Beenden aller Tasks während der Statusänderung eines NUMA-Knotens auf.| 
|SNI_WRITE_ASYNC |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|SOAP_READ |Tritt während eines Wartevorgangs auf den Abschluss eines HTTP-Netzwerklesevorgangs auf.| 
|SOAP_WRITE |Tritt auf, während auf den Abschluss eines HTTP-Netzwerkschreibvorgangs gewartet wird.| 
|SOCKETDUPLICATEQUEUE_CLEANUP |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|SOS_CALLBACK_REMOVAL |Tritt beim Ausführen einer Synchronisierung für eine Rückrufliste auf, um einen Rückruf zu entfernen. Es wird nicht erwartet, dass dieser Leistungsindikator nach dem Abschluss der Serverinitialisierung geändert wird.| 
|SOS_DISPATCHER_MUTEX |Tritt während der internen Synchronisierung des Verteilerpools auf. Dies schließt Anpassungsvorgänge des Pools mit ein.| 
|SOS_LOCALALLOCATORLIST |Tritt während der internen Synchronisierung im Speicher-Manager von [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] auf. <br /><br /> **Gilt für**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] nur. |  
|SOS_MEMORY_TOPLEVELBLOCKALLOCATOR |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SOS_MEMORY_USAGE_ADJUSTMENT |Tritt auf, wenn die Speicherauslastung zwischen Pools angepasst wird.| 
|SOS_OBJECT_STORE_DESTROY_MUTEX |Tritt während der internen Synchronisierung in Speicherpools auf, wenn Objekte im Pool gelöscht werden.| 
|SOS_PHYS_PAGE_CACHE |Bezieht sich auf die Zeit, die ein Thread beim Abrufen des Mutex wartet, das erforderlich ist, bevor physische Seiten zugeordnet oder diese Seiten an das Betriebssystem zurückgegeben werden. Warte Vorgänge für diesen Typ werden nur angezeigt, wenn die Instanz von AWE-Arbeits [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Speicher verwendet. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SOS_PROCESS_AFFINITY_MUTEX |Tritt während der Synchronisierung des Zugriffs für die Verarbeitung von Affinitätseinstellungen auf.| 
|SOS_RESERVEDMEMBLOCKLIST |Tritt während der internen Synchronisierung im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Speicher-Manager auf. <br /><br /> **Gilt für**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] nur. |  
|SOS_SCHEDULER_YIELD |Tritt auf, wenn ein Task freiwillig das Zeitplanungsmodul freigibt, damit andere Tasks ausgeführt werden können. Während dieses Warte Vorgangs wartet der Task in einer ausführbaren Warteschlange darauf, dass sein Quantum erneuert wird. das bedeutet, dass die Ausführung auf der CPU erneut geplant werden soll. Längere warte Vorgänge für diesen Wartetyp weisen am häufigsten auf Möglichkeiten zur Optimierung von Abfragen hin, die Index-oder Tabellenscans ausführen. Konzentrieren Sie sich auf die Plan Regression, den fehlenden Index, Statistik Aktualisierungen, neuschreib Vorgänge von Abfragen. Die Optimierung von Laufzeiten reduziert die Notwendigkeit, dass die Aufgaben nicht mehrmals durchlaufen werden müssen. Wenn Abfrage Zeiten für solche CPU-nutzbaren Tasks akzeptabel sind, wird dieser Wartetyp erwartet und kann ignoriert werden. | 
|SOS_SMALL_PAGE_ALLOC |Tritt während der Zuordnung und Freigabe von Arbeitsspeicher auf, der von einigen Arbeitsspeicherobjekten verwaltet wird.| 
|SOS_STACKSTORE_INIT_MUTEX |Tritt während der Synchronisierung der Initialisierung des internen Speichers auf.| 
|SOS_SYNC_TASK_ENQUEUE_EVENT |Tritt auf, wenn ein Task synchron gestartet wird. Die meisten Tasks in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden asynchron gestartet. Dabei kehrt die Steuerung sofort zum Starter zurück, nachdem die Taskanforderung in der Arbeitswarteschlange angeordnet wurde.| 
|SOS_VIRTUALMEMORY_LOW |Tritt auf, wenn eine Speicher Belegung darauf wartet, dass ein Ressourcen-Manager virtuellen Arbeitsspeicher freigibt.| 
|SOSHOST_EVENT |Tritt auf, wenn eine gehostete Komponente, wie z. B. CLR, auf ein Ereignissynchronisierungsobjekt von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wartet.| 
|SOSHOST_INTERNAL |Tritt während der Synchronisierung von Rückrufen des Speicher-Managers auf, die von gehosteten Komponenten, wie z. B. CLR, verwendet werden.| 
|SOSHOST_MUTEX |Tritt auf, wenn eine gehostete Komponente, wie z. B. CLR, auf ein Mutexsynchronisierungsobjekt von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wartet.| 
|SOSHOST_RWLOCK |Tritt auf, wenn eine gehostete Komponente, wie z. B. CLR, auf ein Leser-/Schreibersynchronisierungsobjekt von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wartet.| 
|SOSHOST_SEMAPHORE |Tritt auf, wenn eine gehostete Komponente, wie z. B. CLR, auf ein Semaphorensynchronisierungsobjekt von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wartet.| 
|SOSHOST_SLEEP |Tritt auf, wenn ein gehosteter Task ruht, während auf das Auftreten eines generischen Ereignisses gewartet wird. Gehostete Tasks werden von gehosteten Komponenten, wie z. B. CLR, verwendet.| 
|SOSHOST_TRACELOCK |Tritt während der Synchronisierung des Zugriffs auf Ablaufverfolgungsdatenströme auf.| 
|SOSHOST_WAITFORDONE |Tritt auf, wenn eine gehostete Komponente, wie z. B. CLR, auf den Abschluss eines Tasks wartet.| 
|SP_PREEMPTIVE_SERVER_DIAGNOSTICS_SLEEP |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SP_SERVER_DIAGNOSTICS_BUFFER_ACCESS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SP_SERVER_DIAGNOSTICS_INIT_MUTEX |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SP_SERVER_DIAGNOSTICS_SLEEP |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SQLCLR_APPDOMAIN |Tritt auf, während CLR auf den Abschluss des Startvorgangs einer Anwendungsdomäne wartet.| 
|SQLCLR_ASSEMBLY |Tritt auf, während auf den Zugriff auf die Liste der geladenen Assemblys in der Anwendungsdomäne gewartet wird.| 
|SQLCLR_DEADLOCK_DETECTION |Tritt auf, während CLR auf den Abschluss der Deadlockerkennung wartet.| 
|SQLCLR_QUANTUM_PUNISHMENT |Tritt auf, wenn ein CLR-Task gedrosselt wird, weil er sein Ausführungsquantum überschritten hat. Diese Drosselung wird ausgeführt, um die Auswirkungen dieses ressourcenintensiven Tasks auf andere Tasks zu reduzieren.| 
|SQLSORT_NORMMUTEX |Tritt bei der internen Synchronisierung auf, während interne Sortierungsstrukturen initialisiert werden.| 
|SQLSORT_SORTMUTEX |Tritt bei der internen Synchronisierung auf, während interne Sortierungsstrukturen initialisiert werden.| 
|SQLTRACE_BUFFER_FLUSH |Tritt auf, wenn ein Task darauf wartet, dass ein Hintergrundtask Ablaufverfolgungspuffer alle vier Sekunden auf den Datenträger leert. <br /><br /> **Gilt für**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] nur. |  
|SQLTRACE_FILE_BUFFER |Tritt während der Synchronisierung für Ablaufverfolgungspuffer während einer Dateiablaufverfolgung auf. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SQLTRACE_FILE_READ_IO_COMPLETION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SQLTRACE_FILE_WRITE_IO_COMPLETION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SQLTRACE_INCREMENTAL_FLUSH_SLEEP |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SQLTRACE_LOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] nur. |  
|SQLTRACE_PENDING_BUFFER_WRITERS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|SQLTRACE_SHUTDOWN |Tritt auf, während das Herunterfahren der Ablaufverfolgung auf den Abschluss ausstehender Ablaufverfolgungsereignisse wartet.| 
|SQLTRACE_WAIT_ENTRIES |Tritt auf, während eine Ereigniswarteschlange der SQL-Ablaufverfolgung auf das Eintreffen von Paketen in der Warteschlange wartet.| 
|SRVPROC_SHUTDOWN |Tritt auf, während der Prozess des Herunterfahrens auf die Freigabe interner Ressourcen wartet, damit ein ordnungsgemäßes Herunterfahren erfolgen kann.| 
|STARTUP_DEPENDENCY_MANAGER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|TDS_BANDWIDTH_STATE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|TDS_INIT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|TDS_PROXY_CONTAINER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|TEMPOBJ |Tritt auf, wenn Löschvorgänge temporärer Objekte synchronisiert werden. Dieser Wartetyp ist selten und tritt nur auf, wenn ein Task einen exklusiven Zugriff für temp-Tabellenlöschvorgänge angefordert hat.| 
|TEMPORAL_BACKGROUND_PROCEED_CLEANUP |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|TERMINATE_LISTENER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|THREADPOOL |Tritt auf, wenn ein Task auf die weitere Ausführung eines Arbeitsthreads wartet. Dies kann anzeigen, dass die Einstellung für die maximale Anzahl von Arbeitsthreads zu niedrig ist oder die Ausführung von Batches ungewöhnlich lange dauert, wodurch die Anzahl der Arbeitsthreads reduziert wird, die für das Ausführen anderer Batches verfügbar sind.| 
|TIMEPRIV_TIMEPERIOD |Tritt während der internen Synchronisierung des Zeitgebers für erweiterte Ereignisse auf.| 
|TRACE_EVTNOTIF |Nur zur internen Verwendung.| 
|TRACEWRITE |Tritt auf, wenn der Rowset-Ablaufverfolgungsanbieter der SQL-Ablaufverfolgung entweder auf einen freien Puffer oder auf einen Puffer mit zu verarbeitenden Ereignissen wartet.| 
|TRAN_MARKLATCH_DT |Tritt auf, wenn auf einen Latch im Löschmodus für einen Transaktionsmarkierungslatch gewartet wird. Transaktionsmarkierungslatches werden für die Synchronisierung von Commit-Vorgängen mit markierten Transaktionen verwendet.| 
|TRAN_MARKLATCH_EX |Tritt auf, wenn auf einen Latch im exklusiven Modus für eine markierte Transaktion gewartet wird. Transaktionsmarkierungslatches werden für die Synchronisierung von Commit-Vorgängen mit markierten Transaktionen verwendet.| 
|TRAN_MARKLATCH_KP |Tritt auf, wenn auf einen Latch im Beibehaltungsmodus für eine markierte Transaktion gewartet wird. Transaktionsmarkierungslatches werden für die Synchronisierung von Commit-Vorgängen mit markierten Transaktionen verwendet.| 
|TRAN_MARKLATCH_NL |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|TRAN_MARKLATCH_SH |Tritt auf, wenn auf einen Latch im freigegebenen Modus für eine markierte Transaktion gewartet wird. Transaktionsmarkierungslatches werden für die Synchronisierung von Commit-Vorgängen mit markierten Transaktionen verwendet.| 
|TRAN_MARKLATCH_UP |Tritt auf, wenn auf einen Latch im Updatemodus für eine markierte Transaktion gewartet wird. Transaktionsmarkierungslatches werden für die Synchronisierung von Commit-Vorgängen mit markierten Transaktionen verwendet.| 
|TRANSACTION_MUTEX |Tritt während der Synchronisierung des Zugriffs auf eine Transaktion durch mehrere Batches auf.| 
|UCS_ENDPOINT_CHANGE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|UCS_MANAGER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|UCS_MEMORY_NOTIFICATION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|UCS_SESSION_REGISTRATION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|UCS_TRANSPORT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|UCS_TRANSPORT_STREAM_CHANGE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|UTIL_PAGE_ALLOC |Tritt auf, wenn Transaktionsprotokollscans darauf warten, dass Arbeitsspeicher bei unzureichendem Arbeitsspeicher verfügbar ist.| 
|VDI_CLIENT_COMPLETECOMMAND |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|VDI_CLIENT_GETCOMMAND |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|VDI_CLIENT_OPERATION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|VDI_CLIENT_OTHER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|VERSIONING_COMMITTING |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|VIA_ACCEPT |Tritt auf, wenn eine VIA (Virtual Interface Adapter)-Anbieterverbindung während des Startvorgangs abgeschlossen wird.| 
|VIEW_DEFINITION_MUTEX |Tritt während der Synchronisierung für den Zugriff auf zwischengespeicherte Sichtdefinitionen auf.| 
|WAIT_FOR_RESULTS |Tritt auf, wenn auf das Auslösen einer Abfragebenachrichtigung gewartet wird.| 
|WAIT_ON_SYNC_STATISTICS_REFRESH |Tritt auf, wenn auf den Abschluss eines synchronen Statistik Updates gewartet wird, bevor die Abfrage Kompilierung und Ausführung fortgesetzt<br /><br /> **Gilt für**: Seit [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]|
|WAIT_SCRIPTDEPLOYMENT_REQUEST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WAIT_SCRIPTDEPLOYMENT_WORKER |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WAIT_XLOGREAD_SIGNAL |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|WAIT_XTP_ASYNC_TX_COMPLETION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WAIT_XTP_CKPT_AGENT_WAKEUP |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WAIT_XTP_CKPT_CLOSE |Tritt ein, wenn auf den Abschluss eines Prüfpunkts gewartet wird. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WAIT_XTP_CKPT_ENABLED |Tritt ein, wenn die Prüfpunktausführung deaktiviert ist, und das Warten auf die Prüfpunktausführung aktiviert ist. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WAIT_XTP_CKPT_STATE_LOCK |Tritt beim Synchronisieren der Überprüfung des Prüfpunktstatus auf. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WAIT_XTP_COMPILE_WAIT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|WAIT_XTP_GUEST |Tritt ein, wenn die Datenbankspeicherbelegung keine Benachrichtigungen über niedrigen Arbeitsspeicher mehr empfangen darf. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|WAIT_XTP_HOST_WAIT |Tritt ein, wenn Wartevorgänge von der Datenbank-Engine ausgelöst und vom Host implementiert werden. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WAIT_XTP_OFFLINE_CKPT_BEFORE_REDO |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WAIT_XTP_OFFLINE_CKPT_LOG_IO |Tritt ein, wenn ein Offline-Prüfpunkt auf den Abschluss einer Protokolllese-E/A wartet. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WAIT_XTP_OFFLINE_CKPT_NEW_LOG |Tritt ein, wenn ein Offline-Prüfpunkt auf Scanvorgänge für neue Protokolldatensätze wartet. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WAIT_XTP_PROCEDURE_ENTRY |Tritt ein, wenn eine Löschprozedur auf den Abschluss aller aktuellen Ausführungen dieser Prozedur wartet. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WAIT_XTP_RECOVERY |Tritt auf, wenn die Daten Bank Wiederherstellung darauf wartet, dass Speicher optimierte Objekte wieder hergestellt werden. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WAIT_XTP_SERIAL_RECOVERY |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|WAIT_XTP_SWITCH_TO_INACTIVE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|WAIT_XTP_TASK_SHUTDOWN |Tritt ein, wenn auf den Abschluss eines In-Memory OLTP-Threads gewartet wird. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|WAIT_XTP_TRAN_DEPENDENCY |Tritt ein, wenn auf Transaktionsabhängigkeiten gewartet wird. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WAITFOR |Tritt als Ergebnis einer WAITFOR Transact-SQL-Anweisung auf. Die Dauer des Wartevorgangs wird durch die Parameter der Anweisung bestimmt. Hierbei handelt es sich um einen vom Benutzer initiierten Wartevorgang.| 
|WAITFOR_PER_QUEUE |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|WAITFOR_TASKSHUTDOWN |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|WAITSTAT_MUTEX |Tritt während der Synchronisierung des Zugriffs auf die Statistikauflistung auf, die zum Auffüllen von sys.dm_os_wait_stats verwendet wird.| 
|WCC |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|WINDOW_AGGREGATES_MULTIPASS |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|WINFAB_API_CALL |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WINFAB_REPLICA_BUILD_OPERATION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|WINFAB_REPORT_FAULT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|WORKTBL_DROP |Tritt während des Anhaltens vor einem erneuten Versuch nach einem fehlerhaften Löschvorgang einer Arbeitstabelle auf.| 
|WRITE_COMPLETION |Tritt während der Ausführung eines Schreibvorgangs auf.| 
|WRITELOG |Tritt auf, während auf den Abschluss einer Protokollleerung gewartet wird. Häufig verwendete Vorgänge, die Protokollleerungen verursachen, sind Prüfpunkte und Transaktionscommits.| 
|XACT_OWN_TRANSACTION |Tritt auf, während auf das Abrufen des Besitzes einer Transaktion gewartet wird.| 
|XACT_RECLAIM_SESSION |Tritt auf, während darauf gewartet wird, dass der aktuelle Besitzer einer Sitzung den Besitz der Sitzung freigibt.| 
|XACTLOCKINFO |Tritt während der Synchronisierung des Zugriffs auf die Liste von Sperren für eine Transaktion auf. Zusätzlich zur Transaktion selbst erfolgt der Zugriff auf die Liste von Sperren durch Vorgänge, wie z. B. Deadlockerkennung und Sperrenmigration, während Seitenteilungen.| 
|XACTWORKSPACE_MUTEX |Tritt während der Synchronisierung von Austritten aus einer Transaktion sowie der Anzahl von Datenbanksperren zwischen eingetragenen Mitgliedern einer Transaktion auf.| 
|XDB_CONN_DUP_HASH |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|XDES_HISTORY |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|XDES_OUT_OF_ORDER_LIST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|XDES_SNAPSHOT |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|Xdestsvermgr |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|XE_BUFFERMGR_ALLPROCESSED_EVENT |Tritt auf, wenn Puffer einer Sitzung für erweiterte Ereignisse an Ziele gesendet werden. Dieser Wartetyp tritt in einem Hintergrundthread auf.| 
|XE_BUFFERMGR_FREEBUF_EVENT |Tritt auf, wenn eine der folgenden Bedingungen zutrifft:| 
|XE_CALLBACK_LIST |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|XE_CX_FILE_READ |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|XE_DISPATCHER_CONFIG_SESSION_LIST |Tritt auf, wenn eine Sitzung für erweiterte Ereignisse, die asynchrone Ziele verwendet, begonnen oder beendet wird. Dieser Wartetyp weist auf eines der folgenden Ereignisse hin:| 
|XE_DISPATCHER_JOIN |Tritt auf, wenn ein für Sitzungen für erweiterte Ereignisse verwendeter Hintergrundthread beendet wird.| 
|XE_DISPATCHER_WAIT |Tritt auf, wenn ein für Sitzungen für erweiterte Ereignisse verwendeter Hintergrundthread auf die Verarbeitung von Ereignispuffern wartet.| 
|XE_FILE_TARGET_TVF |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|XE_LIVE_TARGET_TVF |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.| 
|XE_MODULEMGR_SYNC |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|XE_OLS_LOCK |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. Zukünftige Kompatibilität wird nicht sichergestellt.| 
|XE_PACKAGE_LOCK_BACKOFF |Nur für Informationszwecke identifiziert. Wird nicht unterstützt. <br /><br /> **Gilt für**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] nur. |  
|XE_SERVICES_EVENTMANUAL |Nur zur internen Verwendung.| 
|XE_SERVICES_MUTEX |Nur zur internen Verwendung.| 
|XE_SERVICES_RWLOCK |Nur zur internen Verwendung.| 
|XE_SESSION_CREATE_SYNC |Nur zur internen Verwendung.| 
|XE_SESSION_FLUSH |Nur zur internen Verwendung.| 
|XE_SESSION_SYNC |Nur zur internen Verwendung.| 
|XE_STM_CREATE |Nur zur internen Verwendung.| 
|XE_TIMER_EVENT |Nur zur internen Verwendung.| 
|XE_TIMER_MUTEX |Nur zur internen Verwendung.| 
|XE_TIMER_TASK_DONE |Nur zur internen Verwendung.| 
|XIO_CREDENTIAL_MGR_RWLOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|XIO_CREDENTIAL_RWLOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|XIO_EDS_MGR_RWLOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|XIO_EDS_RWLOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|XIO_IOSTATS_BLOBLIST_RWLOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|XIO_IOSTATS_FCBLIST_RWLOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und höher.| 
|XIO_LEASE_RENEW_MGR_RWLOCK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|XTP_HOST_DB_COLLECTION |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|XTP_HOST_LOG_ACTIVITY |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|XTP_HOST_PARALLEL_RECOVERY |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|XTP_PREEMPTIVE_TASK |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|XTP_TRUNCATION_LSN |Nur zur internen Verwendung. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] und höher.| 
|XTPPROC_CACHE_ACCESS |Tritt beim Zugriff auf alle Cacheobjekte der systemintern kompilierten gespeicherten Prozedur auf. <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher.| 
|XTPPROC_PARTITIONED_STACK_CREATE |Tritt ein, wenn Cachestrukturen der systemintern kompilierten gespeicherten Prozedur pro NUMA-Knoten für eine angegebene Prozedur zugewiesen werden (muss als Singlethread erfolgen). <br /><br /> **Gilt für**:  [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] und höher.|

  
 Die folgenden xevents beziehen sich auf den Partitions **Wechsel** und die Online indexneu Erstellung. Weitere Informationen zur Syntax finden Sie unter [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) und [Alter Index &#40;Transact-SQL-&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
-   lock_request_priority_state  
  
-   process_killed_by_abort_blockers  
  
-   ddl_with_wait_at_low_priority  
  
 Eine Sperr Kompatibilitäts Matrix finden Sie unter [sys.dm_tran_locks &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).  
  
## <a name="see-also"></a>Weitere Informationen  
    
 [SQL Server dynamischen Verwaltungs Sichten im Zusammenhang mit dem Betriebs System &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_exec_session_wait_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-session-wait-stats-transact-sql.md)   
 [sys.dm_db_wait_stats &#40;Azure SQL-Datenbank&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database.md)