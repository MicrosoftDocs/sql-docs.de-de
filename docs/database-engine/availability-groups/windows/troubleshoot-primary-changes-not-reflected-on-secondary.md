---
title: Nicht sichtbare Änderungen an sekundären Verfügbarkeitsgruppenreplikaten
description: Erfahren Sie, wie Sie bestimmen, warum Änderungen an einem primären Replikat nicht im sekundären Replikat einer Always On-Verfügbarkeitsgruppe angezeigt werden.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: c602fd39-db93-4717-8f3a-5a98b940f9cc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 48b31c951cc74f1dc848cb5705b0fd3d756291db
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337887"
---
# <a name="determine-why-changes-from-primary-replica-are-not-reflected-on-secondary-replica-for-an-always-on-availability-group"></a>Bestimmen, warum Änderungen an primären Replikaten nicht in sekundären Replikat einer Always On-Verfügbarkeitsgruppe angezeigt werden
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Die Clientanwendung führt erfolgreich ein Update für das primäre Replikat durch, wobei jedoch die Abfrage des sekundären Replikats ergibt, dass die Änderung nicht widergespiegelt wird. In diesem Fall wird davon ausgegangen, dass hinsichtlich Ihrer Verfügbarkeit ein fehlerfreier Synchronisierungsstatus vorliegt. In den meisten Fällen löst sich dieses Problem nach einigen Minuten von selbst.  
  
 Wenn sich die Änderungen jedoch auch nach wenigen Minuten nicht im sekundäre Replikat widerspiegeln, liegt möglicherweise ein Engpass in der Synchronisierungsworkflow vor. Die Position des Engpasses hängt davon ab, ob das sekundäre Replikat auf den synchronen oder den asynchronen Commit festgelegt ist.  
  
 **Synchroner Commit**  
  
 Jedes erfolgreiche Update für das primäre Replikat wurde bereits mit dem sekundären Replikat synchronisiert oder die Protokolldatensätze wurden bereits zur Festschreibung auf dem sekundären Replikat geleert. Aus diesem Grund sollte sich der Engpass im Wiederholungsprozess befinden, der nach der Protokollleerung auf dem sekundären Replikat erfolgt.  
  
 Sobald die Wiederholung jedoch nachgeholt wurde, stellen alle Leseworkloads auf dem sekundären Replikat eine Momentaufnahme-Isolation dar:  
  
  -   Lang andauernde Transaktionen auf dem primären Replikat  
  
  -   Wiederholung auf dem sekundären Replikat  


**Asynchroner Commit**  
 
 Da der asynchrone Commit eine Transaktion bestätigt, sobald diese auf dem lokalen Datenträger geleert wurde, kann sich der Engpass an einer beliebigen Stelle nach diesem Punkt befinden:  
 
  -   Lang andauernde Transaktionen auf dem primären Replikat  
  
  -   Netzwerklatenz oder -durchsatz  
  
  -   Protokollfestschreibung auf dem sekundären Replikat  
  
  -   Wiederholung auf dem sekundären Replikat  


In den folgenden Abschnitten werden die häufigsten Ursachen für das Problem beschrieben, dass sich Änderungen am primären Replikat bei schreibgeschützten Abfragen nicht im sekundären Replikat widerspiegeln.  


##  <a name="long-running-active-transactions"></a><a name="BKMK_OLDTRANS"></a> Lang andauernde aktive Transaktionen  
 Eine lang andauernde Transaktion auf dem primären Replikat verhindert, dass die Updates für das sekundäre Replikat gelesen werden.  
  
### <a name="explanation"></a>Erklärung  
 Alle Leseworkloads auf dem sekundären Replikat stellen Momentaufnahme-Isolationsabfragen dar. Bei der Momentaufnahmeisolation wird schreibgeschützten Clients die Verfügbarkeitsdatenbank auf dem sekundären Replikat angezeigt, und zwar am Anfangspunkt der ältesten aktiven Transaktion im wiederholten Protokoll. Wenn eine Transaktion mehrere Stunden lang keinen Commit ausgeführt hat, verhindert die geöffnete Transaktion die Anzeige neuer Updates für alle schreibgeschützten Abfragen.  
  
### <a name="diagnosis-and-resolution"></a>Diagnose und Lösung  
 Verwenden Sie auf dem primären Replikat [DBCC OPENTRAN &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-opentran-transact-sql.md), um die ältesten aktiven Transaktionen anzuzeigen und herauszufinden, ob ein Rollback für diese ausgeführt werden kann. Sobald ein Rollback für die ältesten aktiven Transaktionen und eine Synchronisierung mit dem sekundären Replikat ausgeführt wurde, können Leseworkloads auf dem sekundären Replikat Updates in der Verfügbarkeitsdatenbank bis zum Anfang der zum jeweiligen Zeitpunkt ältesten aktiven Transaktion sehen.  
  
##  <a name="high-network-latency-or-low-network-throughput-causes-log-build-up-on-the-primary-replica"></a><a name="BKMK_LATENCY"></a> Eine hohe Netzwerklatenz oder ein niedriger Netzwerkdurchsatz führt zur Protokollanhäufung beim primären Replikat.  
 Eine hohe Netzwerklatenz oder ein niedriger Durchsatz kann dazu führen, dass Protokolle nicht schnell genug an das sekundäre Replikat gesendet werden.  
  
### <a name="explanation"></a>Erklärung  
 Das primäre Replikat aktiviert die Flusssteuerung für die Protokollsendung, wenn es die maximal zulässige Anzahl von unbestätigten Nachrichten, die über das sekundäre Replikat gesendet werden, überschritten hat. Erst, wenn einige dieser Nachrichten bestätigt wurden, können weitere Protokollblöcke an das sekundäre Replikat gesendet werden. Diese Situation kann schwerwiegendere Auswirkungen hinsichtlich des möglichen Datenverlusts zur Folge haben, wodurch möglicherweise Ihre Recovery Point Objective (RPO) beeinträchtigt wird.  
  
### <a name="diagnosis-and-resolution"></a>Diagnose und Lösung  
 Ein hoher DMV-Wert [log_send_queue_size](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) kann darauf hinweisen, dass Protokolle auf dem primären Replikat zurückgehalten werden. Durch Dividieren dieses Werts mit [log_send_rate](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) können Sie einen ungefähren Schätzwert erhalten, wie schnell Daten auf dem sekundären Replikat nachgeholt werden können.  
  
 Zudem ist es hilfreich, die zwei Leistungsobjekte [SQL Server: Verfügbarkeitsreplikat > Flusssteuerungszeit (ms/Sekunde)](~/relational-databases/performance-monitor/sql-server-availability-replica.md) und [SQL Server: Verfügbarkeitsreplikat > Flusssteuerung/s](~/relational-databases/performance-monitor/sql-server-availability-replica.md) zu überprüfen. Durch Multiplikation beider Werte erfahren Sie bis auf die Sekunde genau, wie viel Zeit dafür aufgewendet wurde, auf die Verarbeitung der Flusssteuerung zu warten. Je länger die Wartezeit der Flusssteuerung, desto niedriger ist die Senderate.  
  
 Nachfolgend finden Sie eine Liste mit nützlichen Metriken für die Diagnose von Netzwerklatenz und -durchsatz. Sie können andere Windows-Tools wie **ping.exe** verwenden, um die Netzwerkauslastung auszuwerten.  
  
-   DMV [log_send_queue_size](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   DMV [log_send_rate](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   Leistungsindikator `SQL Server:Database > Log Bytes Flushed/sec`  
  
-   Leistungsindikator `SQL Server:Database Mirroring > Send/Receive Ack Time`  
  
-   Leistungsindikator `SQL Server:Availability Replica > Bytes Sent to Replica/sec`  
  
-   Leistungsindikator `SQL Server:Availability Replica > Bytes Sent to Transport/sec`  
  
-   Leistungsindikator `SQL Server:Availability Replica > Flow Control Time (ms/sec)`  
  
-   Leistungsindikator `SQL Server:Availability Replica > Flow Control/sec`  
  
-   Leistungsindikator `SQL Server:Availability Replica > Resent Messages/sec`  
  
 Versuchen Sie zur Behandlung dieses Problems, ein Upgrade für Ihre Netzwerkbandbreite durchzuführen oder unnötigen Netzwerkdatenverkehr zu beseitigen oder zu verringern.  
  
##  <a name="another-reporting-workload-blocks-the-redo-thread-from-running"></a><a name="BKMK_REDOBLOCK"></a> Eine andere meldende Workload verhindert die Ausführung des Wiederholungsthreads.  
 Der Wiederholungsthread auf dem sekundären Replikat wird durch eine lang andauernde schreibgeschützte Abfrage an der Durchführung von Änderungen an der Datendefinitionssprache (DDL) gehindert. Bevor weitere Updates für die Leseworkload verfügbar gemacht werden können, muss die Blockierung des Wiederholungsthreads aufgehoben werden.  
  
### <a name="explanation"></a>Erklärung  
 Auf dem sekundären Replikat rufen die schreibgeschützten Abfragen Schemastabilitätssperren (`Sch-S`) ab. Diese `Sch-S`-Sperren können den Wiederholungsthread am Abruf von Schemaänderungssperren (`Sch-M`) zur Durchführung von DDL-Änderungen hindern. Ein blockierter Wiederholungsthread kann erst Protokolldatensätze anwenden, wenn die Blockierung aufgehoben wurde.  
  
### <a name="diagnosis-and-resolution"></a>Diagnose und Lösung  
 Wenn der Wiederholungsthread blockiert wurde, wird ein erweitertes Ereignis namens `sqlserver.lock_redo_blocked` generiert. Darüber hinaus können Sie die DMV „sys.dm_exec_request“ für das sekundäre Replikat abfragen, um herauszufinden, welche Sitzung den REDO-Thread blockiert, und dann entsprechende Korrekturmaßnahmen treffen. Die folgende Abfrage gibt die Sitzungs-ID der meldenden Workload zurück, die den Wiederholungsthread blockiert.  
  
```sql  
select session_id, command, blocking_session_id, wait_time, wait_type, wait_resource   
from sys.dm_exec_requests where command = 'DB STARTUP'  
```  
  
 Sie können die Ausführung der meldenden Workload abschließen. An diesem Punkt ist die Blockierung des Wiederholungsthreads aufgehoben. Alternativ können Sie die Blockierung des Wiederholungsthreads sofort aufheben, indem Sie den Befehl [KILL &#40;Transact-SQL&#41;](~/t-sql/language-elements/kill-transact-sql.md) für die blockierende Sitzungs-ID ausführen.  
  
##  <a name="redo-thread-falls-behind-due-to-resource-contention"></a><a name="BKMK_REDOBEHIND"></a> Ein Wiederholungsthread gerät aufgrund von Ressourcenkonflikten in den Rückstand.  
 Eine umfangreiche meldende Workload auf dem sekundären Replikat hat das sekundäre Replikat verlangsamt, weshalb der Wiederholungsthread in den Rückstand geraten ist.  
  
### <a name="explanation"></a>Erklärung  
 Bei der Anwendung von Protokolldatensätzen auf das sekundäre Replikat liest der Wiederholungsthread die Protokolldatensätze vom Protokolldatenträger und greift dann für jeden Protokolldatensatz auf die Datenseiten zu, um den Protokolldatensatz anzuwenden. Der Seitenzugriff kann E/A-gebunden sein (beim Zugriff auf den physischen Datenträger), wenn sich die Seite noch nicht im Pufferpool befindet. Wenn eine E/A-gebundene meldende Workload vorhanden ist, konkurriert die meldende Workload mit dem Wiederholungsthread um E/A-Ressourcen und kann den Wiederholungsthread verlangsamen. Dies wirkt sich nicht nur auf andere meldende Workloads hinsichtlich der Anzeige aktualisierter Daten aus, sondern auch auf die RTO.  
  
### <a name="diagnosis-and-resolution"></a>Diagnose und Lösung  
 Anhand der folgenden DMV-Abfrage können Sie erkennen, wie weit der Wiederholungsthread im Rückstand liegt, indem Sie die Diskrepanz zwischen `last_redone_lsn` und `last_received_lsn` messen.  
  
```sql  
select recovery_lsn, truncation_lsn, last_hardened_lsn, last_received_lsn,   
   last_redone_lsn, last_redone_time  
from sys.dm_hadr_database_replica_states  
  
```  
  
 Wenn der Wiederholungsthread tatsächlich im Rückstand liegt, müssen Sie der Ursache für die Leistungsbeeinträchtigung beim sekundären Replikat auf den Grund gehen. Falls ein E/A-Konflikt bei der meldenden Workload besteht, können Sie mithilfe des [Resource Governor](~/relational-databases/resource-governor/resource-governor.md) bis zu einem gewissen Grad die von der meldenden Workload verwendeten CPU-Zyklen und so indirekt die durchgeführten E/A-Zyklen steuern. Wenn die meldende Workload 10 Prozent der CPU verbraucht, die Workload jedoch E/A-gebunden ist, können Sie zur Drosselung von Leseworkloads den CPU-Ressourceneinsatz mithilfe des Resource Governor auf 5 % beschränken, wodurch die Auswirkungen auf die E/A minimiert werden.  
  
## <a name="next-steps"></a>Nächste Schritte  
 [Behandeln von Leistungsproblemen in SQL Server 2008](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100))
  
