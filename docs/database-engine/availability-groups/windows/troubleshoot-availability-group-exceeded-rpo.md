---
title: Verfügbarkeitsgruppe hat RPO überschritten
description: Häufige Probleme und Lösungen für den Fall, dass die Always On-Verfügbarkeitsgruppe die Recovery Point Objective (RPO) überschreitet.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: 38de1841-9c99-435a-998d-df81c7ca0f1e
author: rothja
ms.author: jroth
ms.openlocfilehash: f70696592cae8f486e7ff7b52e417bb67edfa8d8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337926"
---
# <a name="troubleshoot-availability-group-exceeded-rpo"></a>Problembehandlung: Verfügbarkeitsgruppe hat RPO überschritten
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Nachdem Sie ein erzwungenes manuelles Failover für eine Verfügbarkeitsgruppe für ein sekundäres Replikat im asynchronen Commitmodus ausgeführt haben, stellen Sie eventuell fest, dass der Datenverlust Ihre Recovery Point Objective (RPO) überschreitet. Ein anderer Fall: Wenn Sie den möglichen Datenverlust eines sekundären Replikats im asynchronen Commit mit der Methode unter [Überwachen der Leistung für Always On-Verfügbarkeitsgruppen](monitor-performance-for-always-on-availability-groups.md) berechnen, stellen Sie fest, dass dieser Ihre RPO überschreitet.  
  
 Bei einem sekundären Replikat für synchrone Commits wird sichergestellt, dass keine Daten verloren gehen. Der mögliche Datenverlust eines sekundären Replikats im asynchronen Commitmodus hängt jedoch davon ab, wie groß der Protokollanteil ist, der auf eine Festschreibung im sekundären Replikat wartet.  
  
 In den folgenden Abschnitten werden häufige Ursachen für einen hohen möglichen Datenverlust eines sekundären Replikats im asynchronen Commitmodus beschrieben. Dabei wird davon ausgegangen, dass kein systemisches Leistungsproblem in Ihrer Serverinstanz besteht, das nicht im Zusammenhang mit Verfügbarkeitsgruppen steht.  
  
1.  [Eine hohe Netzwerklatenz oder ein niedriger Netzwerkdurchsatz führt zur Protokollanhäufung beim primären Replikat.](#BKMK_LATENCY)  
  
2.  [Ein Datenträger-E/A-Engpass verlangsamt die Protokollfestschreibung beim sekundären Replikat.](#BKMK_IO_BOTTLENECK)  
  
##  <a name="high-network-latency-or-low-network-throughput-causes-log-build-up-on-the-primary-replica"></a><a name="BKMK_LATENCY"></a> Eine hohe Netzwerklatenz oder ein niedriger Netzwerkdurchsatz führt zur Protokollanhäufung beim primären Replikat.  
 Die häufigste Ursache für die Überschreitung der RPO bei Datenbanken besteht darin, dass sie nicht schnell genug an das sekundäre Replikat gesendet werden können.  
  
### <a name="explanation"></a>Erklärung  
 Das primäre Replikat aktiviert die Flusssteuerung für die Protokollsendung, wenn es die maximal zulässige Anzahl von unbestätigten Nachrichten, die über das sekundäre Replikat gesendet werden, überschritten hat. Erst, wenn einige dieser Nachrichten bestätigt wurden, können weitere Protokollblöcke an das sekundäre Replikat gesendet werden. Da ein Datenverlust nur dann verhindert werden kann, wenn dieser auf dem sekundären Replikat festgeschrieben wurde, vergrößert sich durch die Anhäufung nicht gesendeter Protokollnachrichten der mögliche Datenverlust.  
  
### <a name="diagnosis-and-resolution"></a>Diagnose und Lösung  
 Eine große Anzahl von Nachrichten, die erneut an das sekundäre Replikat gesendet werden, kann auf eine hohe Netzwerklatenz und auf Netzwerkrauschen zurückzuführen sein. Sie können auch den DMV-Wert **log_send_rate** mit dem Leistungsobjekt „Geleerte Protokollbytes/Sekunde“ abgleichen. Wenn Protokolle schneller auf den Datenträgern geleert werden, als sie gesendet werden, kann der mögliche Datenverlust ins Unermessliche steigen.  
  
 Es ist außerdem hilfreich, die zwei Leistungsobjekte `SQL Server:Availability Replica > Flow Control Time (ms/sec)` und `SQL Server:Availability Replica > Flow Control/sec` zu überprüfen. Durch Multiplikation beider Werte erfahren Sie bis auf die Sekunde genau, wie viel Zeit dafür aufgewendet wurde, auf die Verarbeitung der Flusssteuerung zu warten. Je länger die Wartezeit der Flusssteuerung, desto niedriger ist die Senderate.  
  
 Die folgenden Metriken sind hilfreich bei der Diagnose von Netzwerklatenz und -durchsatz. Sie können andere Windows-Tools wie **ping.exe** und [Netzwerkmonitor](https://www.microsoft.com/p/network-monitor-pro-free-edition/9n8gdvj32gp7) verwenden, um Latenz und Netzwerkauslastung auszuwerten.  
  
-   DMV `sys.dm_hadr_database_replica_states, log_send_queue_size`  
  
-   DMV `sys.dm_hadr_database_replica_states, log_send_rate`  
  
-   Leistungsindikator `SQL Server:Database > Log Bytes Flushed/sec`  
  
-   Leistungsindikator `SQL Server:Database Mirroring > Send/Receive Ack Time`  
  
-   Leistungsindikator `SQL Server:Availability Replica > Bytes Sent to Replica/sec`  
  
-   Leistungsindikator `SQL Server:Availability Replica > Bytes Sent to Transport/sec`  
  
-   Leistungsindikator `SQL Server:Availability Replica > Flow Control Time (ms/sec)`  
  
-   Leistungsindikator `SQL Server:Availability Replica > Flow Control/sec`  
  
-   Leistungsindikator `SQL Server:Availability Replica > Resent Messages/sec`  

Versuchen Sie zur Behandlung dieses Problems, ein Upgrade für Ihre Netzwerkbandbreite durchzuführen oder unnötigen Netzwerkdatenverkehr zu beseitigen oder zu verringern.  


##  <a name="disk-io-bottleneck-slows-down-log-hardening-on-the-secondary-replica"></a><a name="BKMK_IO_BOTTLENECK"></a> Ein Datenträger-E/A-Engpass verlangsamt die Protokollfestschreibung beim sekundären Replikat.  
 Abhängig von der Bereitstellung der Datenbankdateien kann sich die Protokollfestschreibung aufgrund eines E/A-Konflikts mit der meldenden Workload verlangsamen.  
  
### <a name="explanation"></a>Erklärung  
 Ein Datenverlust wird verhindert, sobald der Protokollblock in die Protokolldatei festgeschrieben wurde. Aus diesem Grund ist es wichtig, die Protokolldatei von der Datendatei zu isolieren. Wenn die Protokoll- und Datendatei beide derselben Festplatte zugeordnet werden, verbraucht die meldende Workload mit intensiven Lesevorgängen in der Datendatei die gleichen E/A-Ressourcen, die vom Vorgang zur Protokollfestschreibung benötigt werden. Eine langsame Protokollfestschreibung kann zu einer langsamen Bestätigung gegenüber dem primären Replikat führen, das eine übermäßige Aktivierung der Flusssteuerung und lange Wartezeiten bei der Flusssteuerung führen kann.  
  
### <a name="diagnosis-and-resolution"></a>Diagnose und Lösung  
 Wenn Sie überprüft haben, ob im Netzwerk eine hohe Latenz oder ein geringer Durchsatz auftreten, sollten Sie das sekundäre Replikat auf E/A-Konflikte untersuchen. Abfragen von [SQL Server: Minimieren von Datenträger-E/As](/previous-versions/technet-magazine/jj643251(v=msdn.10)) sind nützlich, um Konflikte zu identifizieren. Beispiele aus diesem Artikel werden der Übersichtlichkeit halber unten aufgeführt.  
  
 Mit dem folgenden Skript können Sie die Anzahl der Lese- und Schreibvorgänge zu allen Daten- und Protokolldateien für jede Verfügbarkeitsdatenbank anzeigen, die auf einer Instanz von SQL Server ausgeführt wird. Diese ist nach der durchschnittlichen E/A-Verzögerungszeit in Millisekunden sortiert. Beachten Sie, dass die Zahlen kumulativ ausgehend vom letzten Zeitpunkt sind, an dem die Serverinstanz gestartet wurde. Aus diesem Grund sollten Sie nach einiger Weile die Differenz zwischen zwei Messungen messen.  
  
```sql  
SELECT DB_NAME(database_id) AS   
   [Database Name] ,   
   file_id ,   
   io_stall_read_ms ,   
   num_of_reads ,   
   CAST(io_stall_read_ms / ( 1.0 + num_of_reads ) AS NUMERIC(10, 1)) AS [avg_read_stall_ms] ,   
   io_stall_write_ms ,   
   num_of_writes ,  
   CAST(io_stall_write_ms / ( 1.0 + num_of_writes ) AS NUMERIC(10, 1)) AS [avg_write_stall_ms] ,   
   io_stall_read_ms + io_stall_write_ms AS [io_stalls] ,   
   num_of_reads + num_of_writes AS [total_io] ,   
   CAST(( io_stall_read_ms + io_stall_write_ms ) / ( 1.0 + num_of_reads  
+ num_of_writes) AS NUMERIC(10,1)) AS [avg_io_stall_ms]  
FROM sys.dm_io_virtual_file_stats(NULL, NULL)  
WHERE DB_NAME(database_id) IN (SELECT DISTINCT database_name FROM sys.dm_hadr_database_replica_cluster_states)  
ORDER BY avg_io_stall_ms DESC;  
```  
  
 Die nächste Abfrage stellt eine Point-in-Time-Momentaufnahme (nicht kumulativ) der auf Ihrem System ausstehenden E/A-Anforderungen bereit.  
  
```sql  
SELECT DB_NAME(mf.database_id) AS [Database] ,   
   mf.physical_name ,  
   r.io_pending ,   
   r.io_pending_ms_ticks ,   
   r.io_type ,   
   fs.num_of_reads ,   
   fs.num_of_writes  
FROM sys.dm_io_pending_io_requests AS r   
INNER JOIN sys.dm_io_virtual_file_stats(NULL, NULL) AS fs ON r.io_handle = fs.file_handle   
INNER JOIN sys.master_files AS mf ON fs.database_id = mf.database_id  
AND fs.file_id = mf.file_id  
ORDER BY r.io_pending , r.io_pending_ms_ticks DESC;  
```  
  
 Sie können vergleichen, wie groß die E/A-Anteile von Lese- und Schreibvorgängen zur Identifizierung von E/A-Konflikten sind.  
  
 Im Folgenden werden einige weitere Leistungsindikatoren aufgeführt, mit denen Sie E/A-Engpässe diagnostizieren können:  
  
-   **Physischer Datenträger: Alle Indikatoren**  
  
-   **Physischer Datenträger: Durchschn. Sek./Übertragung**  
  
-   **SQL Server: Datenbanken > Wartezeit für Protokollleerungen**  
  
-   **SQL Server: Datenbanken > Wartevorgänge für Protokollleerungen/Sekunde**  
  
-   **SQL Server: Datenbanken > Protokollpool-Datenträgerlesevorgänge/Sekunde**  
  
 Wenn Sie einen E/A-Engpass identifizieren und die Protokoll- und Datendatei auf derselben Festplatte platziert haben, sollten Sie im ersten Schritt die Daten- und die Protokolldatei auf separaten Datenträgern platzieren. Durch diese bewährte Vorgehensweise wird verhindert, dass die meldende Workload den Übertragungsprotokollpfad vom primären Replikat zum Protokollpuffer sowie dessen Fähigkeit zur Festschreibung der Transaktion auf dem sekundären Replikat beeinträchtigt.  
  
## <a name="next-steps"></a>Nächste Schritte  
 [Behandlung von Leistungsproblemen in SQL Server (gilt für SQL Server 2012)](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100))  
  
