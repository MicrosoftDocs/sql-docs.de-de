---
title: Konfigurieren der Serverkonfigurationsoption „Maximale Anzahl von Arbeitsthreads“ | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Option „max worker threads“ zum Konfigurieren der Anzahl von Arbeitsthreads verwenden, die SQL Server zum Verarbeiten bestimmter Anforderungen zur Verfügung stehen.
ms.custom: contperf-fy20q4
ms.date: 04/14/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- worker threads [SQL Server]
- max worker threads option
ms.assetid: abeadfa4-a14d-469a-bacf-75812e48fac1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c5291b502ec70be8b86c6806e14404664d0b0831
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100347003"
---
# <a name="configure-the-max-worker-threads-server-configuration-option"></a>Konfigurieren der Serverkonfigurationsoption Maximale Anzahl von Arbeitsthreads
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Thema wird beschrieben, wie die Serverkonfigurationsoption **Max. Anzahl von Arbeitsthreads** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]konfiguriert wird. Mit der Option **max worker threads** wird die Anzahl von Arbeitsthreads konfiguriert, die in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zum Verarbeiten von Abfrageanforderungen, Anmelde-, Abmelde- und ähnlichen Anwendungsanforderungen verfügbar sind.


[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet die nativen Threaddienste der Betriebssysteme, um die folgenden Bedingungen sicherzustellen:

- Jedes Netzwerk, das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt, wird von einem Thread oder mehreren Threads gleichzeitig unterstützt.

- Ein Thread verarbeitet Datenbankprüfpunkte.

- Ein Threadpool verarbeitet alle Benutzer.

Der Standardwert für **Max. Anzahl von Arbeitsthreads** ist 0. Auf diese Weise kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Anzahl der Arbeitsthreads beim Starten automatisch konfigurieren. Die Standardeinstellung ist für die meisten Systeme am besten geeignet. Abhängig von der Konfiguration des Systems kann durch Festlegen von **Max. Anzahl von Arbeitsthreads** auf einen bestimmten Wert manchmal die Leistung verbessert werden.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Die tatsächliche Anzahl von Abfrageanforderungen kann den für **max worker threads** festgelegten Wert überschreiten. In diesem Fall erstellt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen Arbeitsthreadpool, damit die Anforderung vom nächsten verfügbaren Arbeitsthread verarbeitet werden kann. Ein Arbeitsthread wird nur aktiven Anforderungen zugewiesen und nach Verarbeitung der Anforderung freigegeben. Dies geschieht selbst dann, wenn die Benutzersitzung bzw. die Verbindung, für die die Anforderung erfolgt ist, offen bleibt. 

-   Durch die Serverkonfigurationsoption **max worker threads** werden nicht alle Threads begrenzt, die innerhalb der Engine generiert werden können. Systemthreads, die für Tasks wie LazyWriter, Checkpoint, Log Writer, Service Broker, Lock Manager usw. erforderlich sind, werden außerhalb dieses Limits generiert. Verfügbarkeitsgruppen verwenden einige der Arbeitsthreads innerhalb der Begrenzung **max worker threads**, aber auch Systemthreads (siehe [Threadverwendung durch Verfügbarkeitsgruppen](../availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md#ThreadUsage)). Wenn die konfigurierte maximale Anzahl von Threads überschritten wird, erhalten Sie über die folgende Abfrage Informationen zu den Systemtasks, die die zusätzlichen Threads generiert haben.  
  
 ```sql  
 SELECT  s.session_id, r.command, r.status,  
    r.wait_type, r.scheduler_id, w.worker_address,  
    w.is_preemptive, w.state, t.task_state,  
    t.session_id, t.exec_context_id, t.request_id  
 FROM sys.dm_exec_sessions AS s  
 INNER JOIN sys.dm_exec_requests AS r  
    ON s.session_id = r.session_id  
 INNER JOIN sys.dm_os_tasks AS t  
    ON r.task_address = t.task_address  
 INNER JOIN sys.dm_os_workers AS w  
    ON t.worker_address = w.worker_address  
 WHERE s.is_user_process = 0;  
 ```  
  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
  
-   Diese Option ist eine erweiterte Option und sollte ausschließlich von einem erfahrenen Datenbankadministrator oder einem zertifizierten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Experten geändert werden. Wenn Sie ein Leistungsproblem vermuten, ist es wahrscheinlich nicht die Verfügbarkeit von Arbeitsthreads. Die Ursache hängt eher mit Aktivitäten zusammen, die die Arbeitsthreads belegen und nicht freigeben. Beispiele hierfür sind Abfragen mit langer Ausführungszeit oder Engpässe im System (E/A, Blockierungen, Latchwartevorgänge, Netzwerkwartevorgänge), die Abfragen mit langer Wartezeit verursachen. Sinnvollerweise sollten Sie die Grundursache eines Leistungsproblems ermitteln, bevor Sie die Einstellung für die maximale Anzahl der Arbeitsthreads ändern. Weitere Informationen zur Leistungsbewertung finden Sie unter [Überwachen und Optimieren der Leistung](../../relational-databases/performance/monitor-and-tune-for-performance.md).
  
-   Threadpools erleichtern das Optimieren der Leistung, wenn sehr viele Clients eine Verbindung mit dem Server herstellen. Üblicherweise wird ein separater Betriebssystemthread für jede Abfrageanforderung erstellt. Wenn jedoch bei Hunderten von Verbindungen mit dem Server weiterhin ein Thread pro Abfrageanforderung verwendet wird, kann dabei eine große Menge an Systemressourcen verbraucht werden. Die Option **Max. Anzahl von Arbeitsthreads** ermöglicht [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das Erstellen eines Pools mit Arbeitsthreads, der eine große Anzahl von Abfrageanforderungen versorgen kann und so zur Verbesserung der Leistung beiträgt.  
  
-   Die folgende Tabelle zeigt die automatisch konfigurierte Anzahl von maximalen Arbeitsthreads (bei einem Wert von 0) auf der Basis verschiedener Kombinationen aus CPUs, Computerarchitektur und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Versionen. Folgende Formel wird verwendet: **_Standardwert für maximale Anzahl von Workern_ + ((* logische CPUs* – 4) * *Worker pro CPU*)**.  
  
    |Anzahl von CPUs|32-Bit-Computer (bis [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)])|64-Bit-Computer (bis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1)|64-Bit-Computer (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])|   
    |------------|------------|------------|------------|  
    |\<= 4|256|512|512|   
    |8|288|576|576|   
    |16|352|704|704|   
    |32|480|960|960|   
    |64|736|1472|1472|   
    |128|1248|2496|4480|   
    |256|2272|4544|8576|   
    
    Bis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 hängt der Wert für *Worker pro CPU* nur von der Architektur ab (32 oder 64 Bit):
    
    |Anzahl von CPUs|32-Bit-Computer <sup>1</sup>|64-Bit-Computer|   
    |------------|------------|------------|   
    |\<= 4|256|512|   
    |\> 4|256 + ((logische CPUs - 4) * 8)|512 <sup>2</sup> + ((logische CPUs - 4) * 16)|   
    
    Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 und [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] hängt der Wert für *Worker pro CPU* von der Architektur und der Anzahl von Prozessoren ab (zwischen 4 und 64 oder mehr als 64):
    
    |Anzahl von CPUs|32-Bit-Computer <sup>1</sup>|64-Bit-Computer|   
    |------------|------------|------------|   
    |\<= 4|256|512|   
    |\> 4 und \<= 64|256 + ((logische CPUs - 4) * 8)|512 <sup>2</sup> + ((logische CPUs - 4) * 16)|   
    |\> 64|256 + ((logische CPUs – 4) * 32)|512 <sup>2</sup> + ((logische CPUs - 4) * 32)|   
  
    <sup>1</sup> Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht mehr unter einem 32-Bit-Betriebssystem installiert werden. Werte für 32-Bit-Computer sind zur Unterstützung von Kunden aufgeführt, die [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und früher nutzen. Als maximale Anzahl von Arbeitsthreads auf einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz, die auf einem 32-Bit-Computer ausgeführt wird, wird der Wert 1.024 empfohlen.
    
    <sup>2</sup> Ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] wird der *Standardwert für maximale Anzahl von Workern* für Computer mit weniger als 2 GB Arbeitsspeicher durch 2 dividiert.
  
    > [!TIP]  
    > Empfehlungen zur Verwendung von mehr als 64 CPUs finden Sie unter [Bewährte Methoden zum Ausführen von SQL Server auf Computern mit mehr als 64 CPUs](../../relational-databases/thread-and-task-architecture-guide.md#best-practices-for-running-sql-server-on-computers-that-have-more-than-64-cpus).  
  
-   Wenn alle Arbeitsthreads aktiviert sind, kann es sein, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bei Abfragen mit langer Ausführungszeit scheinbar nicht mehr reagiert, bis ein Arbeitsthread abgeschlossen wird und verfügbar ist. Dies ist zwar kein Fehler, aber in bestimmten Situationen unerwünscht. Wenn ein Prozess scheinbar nicht mehr reagiert und keine neuen Abfragen verarbeitet werden können, stellen Sie mithilfe der dedizierten Administratorverbindung (DAC) eine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] her, und brechen Sie den Prozess ab. Um diese Situation zu verhindern, erhöhen Sie den Wert für die maximale Anzahl von Arbeitsthreads.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Die Ausführungsberechtigungen für **sp_configure** ohne Parameter oder nur mit dem ersten Parameter werden standardmäßig allen Benutzern erteilt. Um **sp_configure** mit beiden Parametern auszuführen und eine Konfigurationsoption zu ändern oder die `RECONFIGURE`-Anweisung auszuführen, benötigt ein Benutzer die `ALTER SETTINGS`-Berechtigung auf Serverebene. Die `ALTER SETTINGS`-Berechtigung ist implizit in den festen Serverrollen **sysadmin** und **serveradmin** enthalten.  
  
##  <a name="using-ssmanstudiofull"></a><a name="SSMSProcedure"></a> Bei Verwendung von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
  
#### <a name="to-configure-the-max-worker-threads-option"></a>So konfigurieren Sie die Option Max. Anzahl von Arbeitsthreads  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf einen Server, und wählen Sie **Eigenschaften** aus.  
  
2.  Klicken Sie auf den Knoten **Prozessoren** .  
  
3.  Geben Sie im Feld **Max. Anzahl von Arbeitsthreads** einen Wert zwischen 128 und 32.767 ein, oder wählen Sie einen Wert aus.  
  
> [!TIP]
> Sie können mithilfe der Option **Max. Anzahl von Arbeitsthreads** die Anzahl von Arbeitsthreads konfigurieren, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Prozessen zur Verfügung stehen. Die Standardeinstellung für **Max. Anzahl von Arbeitsthreads** eignet sich für die meisten Systeme am besten. Abhängig von der Konfiguration des Systems kann die Leistung manchmal verbessert werden, indem **Max. Anzahl von Arbeitsthreads** auf einen niedrigeren Wert festgelegt wird.
> Unter [Empfehlungen](#Recommendations) auf dieser Seite finden Sie weitere Informationen.
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-configure-the-max-worker-threads-option"></a>So konfigurieren Sie die Option Max. Anzahl von Arbeitsthreads  
  
1.  Stellen Sie eine Verbindung mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)]her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. In diesem Beispiel wird gezeigt, wie [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) verwendet wird, um die Option `max worker threads` auf `900`festzulegen.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'max worker threads', 900 ;  
GO  
RECONFIGURE;  
GO  
```  
  
##  <a name="follow-up-after-you-configure-the-max-worker-threads-option"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Konfigurieren der Option „Max. Anzahl von Arbeitsthreads“  
 Die Änderung wird nach dem Ausführen von [RECONFIGURE](../../t-sql/language-elements/reconfigure-transact-sql.md) sofort wirksam, ohne dass ein Neustart von [!INCLUDE[ssDE](../../includes/ssde-md.md)] erforderlich ist.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Diagnoseverbindung für Datenbankadministratoren](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)
