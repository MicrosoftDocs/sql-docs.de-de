---
title: Datenbankprüfpunkte (SQL Server) | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über Prüfpunkte, die gute Startpunkte für die SQL Server-Datenbank-Engine darstellen, um während der Wiederherstellung Änderungen aus dem Protokoll anzuwenden.
ms.date: 04/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- automatic checkpoints
- transaction logs [SQL Server], checkpoints
- logs [SQL Server], active
- pages [SQL Server], dirty
- MinLSN
- checkpoints [SQL Server]
- pages [SQL Server], flushing
- dirty pages
- transaction logs [SQL Server], active logs
- recovery interval option [SQL Server]
- buffer cache [SQL Server]
- logs [SQL Server], checkpoints
- Minimum Recovery LSN
- flushing pages
- active logs
ms.assetid: 98a80238-7409-4708-8a7d-5defd9957185
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9277d551762afcff062a6a1e1fc389009e5ea2ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351206"
---
# <a name="database-checkpoints-sql-server"></a>Datenbankprüfpunkte (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
 Ein *Prüfpunkt* erstellt einen bekannten fehlerfreien Punkt, von dem aus [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] Änderungen übernehmen kann, die im Protokoll während der Wiederherstellung nach einem unerwarteten Herunterfahren oder einem Absturz enthalten sind.

##  <a name="overview"></a><a name="Overview"></a> Übersicht   
Aus Leistungsgründen führt [!INCLUDE[ssDE](../../includes/ssde-md.md)] Änderungen an Datenbankseiten im Arbeitsspeicher aus (im Puffercache) und schreibt diese Seiten nicht nach jeder Änderung auf den Datenträger. Vielmehr gibt [!INCLUDE[ssDE](../../includes/ssde-md.md)] in regelmäßigen Abständen einen Prüfpunkt auf jeder Datenbank aus. Ein *Prüfpunkt* schreibt die aktuellen, speicherintern geänderten Seiten (auch bekannt als *modifizierte Seiten*) sowie Transaktionsprotokollinformationen vom Arbeitsspeicher auf den Datenträger und erfasst diese Informationen im Transaktionsprotokoll.  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] unterstützt mehrere Typen von Prüfpunkten. Dazu gehören "automatisch", "indirekt", "manuell" und "intern". In der folgenden Tabelle werden die **Prüfpunkttypen** zusammengefasst:
  
|Name|[!INCLUDE[tsql](../../includes/tsql-md.md)] -Schnittstelle|BESCHREIBUNG|  
|----------|----------------------------------|-----------------|  
|Automatic|EXEC sp_configure **'** Wiederherstellungsintervall **','** _seconds_ **'**|Wird automatisch im Hintergrund ausgegeben, um das obere, mittels Serverkonfigurationsoption **Wiederherstellungsintervall** vorgeschlagene Zeitlimit zu erfüllen. Automatische Prüfpunkte werden vollständig ausgeführt.  Automatische Prüfpunkte werden auf Basis der Anzahl an ausstehenden Schreibvorgängen gedrosselt. Zudem hängt die Drosselung auch davon ab, ob [!INCLUDE[ssDE](../../includes/ssde-md.md)] eine Erhöhung der Schreiblatenz auf über 50 Millisekunden erkennt.<br /><br /> Weitere Informationen finden Sie unter [Configure the recovery interval Server Configuration Option](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md).|  
|Indirekt|ALTER DATABASE ... SET TARGET_RECOVERY_TIME **=** _Zielwiederherstellungszeit_ { SECONDS &#124; MINUTES }|Wird im Hintergrund ausgegeben, um eine benutzerdefinierte Zielwiederherstellungszeit für eine bestimmte Datenbank zu erfüllen. Ab [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)]ist der Standardwert gleich 1 Minute. Der Standard für ältere Versionen ist 0 und gibt an, dass die Datenbank automatische Prüfpunkte verwendet, deren Frequenz von der Einstellung für das Wiederherstellungsintervall der Serverinstanz abhängt.<br /><br /> Weitere Informationen finden Sie unter [Ändern der Zielwiederherstellungszeit einer Datenbank &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md)konfiguriert wird.|  
|Manuell|CHECKPOINT [*checkpoint_duration*]|Wird ausgegeben, wenn Sie einen [!INCLUDE[tsql](../../includes/tsql-md.md)] -CHECKPOINT-Befehl ausführen. Der manuelle Prüfpunkt tritt in der aktuellen Datenbank für die Verbindung auf. Standardmäßig werden manuelle Prüfpunkte vollständig ausgeführt. Das Drosseln erfolgt auf die gleiche Weise wie für automatische Prüfpunkte.  Optional gibt der *checkpoint_duration* -Parameter die Anforderung an, welchen Zeitraum in Sekunden ein Prüfpunkt benötigen darf, bis er abgeschlossen ist.<br /><br /> Weitere Informationen finden Sie unter [CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md).|  
|Intern|Keine.|Wird von verschiedenen Servervorgängen wie Sicherung und Erstellung einer Datenbank-Momentaufnahme ausgegeben. So wird gewährleistet, dass Datenträgerabbilder dem aktuellen Protokollstatus entsprechen.|  
  
> [!NOTE]
> Die erweiterte Setupoption **-k** von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ermöglicht Datenbankadministratoren das Drosseln des E/A-Prüfpunktverhaltens basierend auf dem Durchsatz des E/A-Teilsystems für einige Arten von Prüfpunkten. Die Setupoption **-k** gilt für automatische Prüfpunkte sowie für andere nicht gedrosselte manuelle und interne Prüfpunkte.  
  
 Für automatische, manuelle und interne Prüfpunkte muss nur für Änderungen, die nach dem letzten Prüfpunkt erfolgt sind, während der Datenbankwiederherstellung ein Rollforward ausgeführt werden. Dadurch verringert sich die Zeit, die zur Wiederherstellung einer Datenbank erforderlich ist.  
  
> [!IMPORTANT]
> Lang andauernde Transaktionen ohne Commit erhöhen die Wiederherstellungszeit für alle Prüfpunkttypen.   
  
##  <a name="interaction-of-the-target_recovery_time-and-recovery-interval-options"></a><a name="InteractionBwnSettings"></a> Interaktion der Optionen "TTARGET_RECOVERY_TIME" und "Wiederherstellungsintervall"  
 In der folgenden Tabelle wird die Interaktion zwischen der serverweiten Einstellung **sp_configure '** Wiederherstellungsintervall **'** und der datenbankspezifischen `ALTER DATABASE ... TARGET_RECOVERY_TIME`-Einstellung zusammengefasst.  
  
|Zielwiederherstellungszeit|'Wiederherstellungsintervall'|Verwendeter Prüfpunkttyp|  
|----------------------------|-------------------------|-----------------------------|  
|0|0|Automatische Prüfpunkte, deren Zielwiederherstellungsintervall 1 Minute beträgt.|  
|0|>0|Automatische Prüfpunkte, deren Zielwiederherstellungsintervall anhand der benutzerdefinierten Einstellung der Option **sp_configure 'recovery interval'** angegeben wurde.|  
|>0|Nicht zutreffend|Indirekte Prüfpunkte, deren Zielwiederherstellungszeit anhand der TARGET_RECOVERY_TIME-Einstellung definiert ist (in Sekunden ausgedrückt).|  
  
##  <a name="automatic-checkpoints"></a><a name="AutomaticChkpt"></a> Automatische Prüfpunkte  
Ein automatischer Prüfpunkt tritt auf, sobald die Anzahl von Protokolldatensätzen die Anzahl der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Schätzungen erreicht, die während der Zeit verarbeitet werden können, die in der Serverkonfigurationsoption **Wiederherstellungsintervall** angegeben ist. Weitere Informationen finden Sie unter [Configure the recovery interval Server Configuration Option](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md).
 
In jeder Datenbank ohne benutzerdefinierte Zielwiederherstellungszeit generiert [!INCLUDE[ssDE](../../includes/ssde-md.md)] automatische Prüfpunkte. Die Frequenz richtet sich nach der erweiterten Serverkonfigurationsoption **Wiederherstellungsintervall** , mit der die maximale Zeit angegeben wird, die von einer bestimmten Serverinstanz zur Datenbankwiederherstellung während eines Systemneustarts benötigt werden sollte. [!INCLUDE[ssDE](../../includes/ssde-md.md)] schätzt die maximale Anzahl an Protokolldatensätzen, die innerhalb des Wiederherstellungsintervalls verarbeitet werden können. Erreicht eine Datenbank, die automatische Prüfpunkte verwendet, diese maximale Anzahl an Protokolldatensätzen, gibt [!INCLUDE[ssDE](../../includes/ssde-md.md)] einen Prüfpunkt in der Datenbank aus. 
 
Das Zeitintervall zwischen automatischen Prüfpunkten kann **in hohem Maß** variieren. Bei einer Datenbank mit einer beträchtlichen Transaktionsarbeitsauslastung treten häufiger Prüfpunkte auf als bei einer Datenbank für hauptsächlich schreibgeschützte Vorgänge. Im einfachen Wiederherstellungsmodell wird ein automatischer Prüfpunkt auch in die Warteschlange aufgenommen, wenn das Protokoll zu 70 Prozent gefüllt ist.  
  
Unter dem einfachen Wiederherstellungsmodell wird der ungenutzte Abschnitt des Transaktionsprotokolls durch einen automatischen Prüfpunkt abgeschnitten, sofern nicht gewisse Faktoren die Protokollkürzung verzögern. Im Gegensatz dazu lösen automatische Prüfpunkte im vollständigen und im massenprotokollierten Wiederherstellungsmodell keine Protokollkürzung aus, sobald eine Protokollsicherungskette eingerichtet wurde. Weitere Informationen finden Sie unter [Das Transaktionsprotokoll &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
Nach einem Systemabsturz richtet sich die Zeitdauer, die für die Wiederherstellung einer bestimmten Datenbank erforderlich ist, in hohem Maß nach der Menge zufälliger E/A-Vorgänge, die zur Wiederherstellung der zum Zeitpunkt des Absturzes modifizierten Seiten erforderlich sind. Dies bedeutet, dass die Einstellung **Wiederherstellungsintervall** nicht zuverlässig ist. Sie ermöglicht keine genaue Bestimmung der Wiederherstellungsdauer. Zudem erhöht sich die allgemeine E/A-Aktivität für Daten erheblich und eher unvorhersehbar, wenn ein automatischer Prüfpunkt ausgeführt wird.  
   
###  <a name="impact-of-recovery-interval-on-recovery-performance"></a><a name="PerformanceImpact"></a> Auswirkungen des Wiederherstellungsintervalls auf die Wiederherstellungsleistung  
Bei einem System zur Onlinetransaktionsverarbeitung (Online Transaction Processing, OLTP), das Transaktionen mit kurzer Ausführungszeit verwendet, ist das **Wiederherstellungsintervall** der wichtigste Faktor für die Bestimmung der Wiederherstellungszeit. Die Option **Wiederherstellungsintervall** wirkt sich jedoch nicht auf die Zeit aus, die für das Rückgängigmachen einer Transaktion mit langer Ausführungszeit erforderlich ist. Die Wiederherstellung einer Datenbank mit einer Transaktion mit langer Ausführungszeit kann deutlich länger dauern als mit der Einstellung **Wiederherstellungsintervall** angegeben. 
 
Wenn beispielsweise eine Transaktion mit langer Ausführungszeit zwei Stunden für Updates benötigt hat, bevor die Serverinstanz deaktiviert wurde, dauert die tatsächliche Wiederherstellung erheblich länger als im Wert für das **Wiederherstellungsintervall** zur Wiederherstellung der Transaktion mit langer Ausführungszeit angegeben. Weitere Informationen zu den Auswirkungen einer Transaktion mit langer Ausführungszeit auf die Wiederherstellungsdauer finden Sie unter [Das Transaktionsprotokoll &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md). Informationen zum Wiederherstellungsprozess finden Sie unter [ (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery).
  
In der Regel gewährleisten die Standardwerte eine optimale Wiederherstellungsleistung. Durch Ändern des Wiederherstellungsintervalls kann sich jedoch die Leistung in folgenden Fällen verbessern:  
  
-   Die Wiederherstellung dauert routinemäßig bedeutend länger als 1 Minute, wenn kein Rollback für Transaktionen mit langer Laufzeit ausgeführt wird.  
  
-   Sie stellen fest, dass häufige Prüfpunkte die Datenbankleistung beeinträchtigen.  
  
Wenn Sie die Einstellung **recovery interval** erhöhen möchten, empfehlen wir eine schrittweise Erhöhung des entsprechenden Werts. Werten Sie zudem die Auswirkungen der jeweiligen stufenweisen Erhöhung auf die Wiederherstellungsleistung aus. Diese Vorgehensweise ist wichtig, da mit der Erhöhung des Werts für die Einstellung **Wiederherstellungsintervall** die Ausführung der Datenbankwiederherstellung entsprechend länger dauert. Ändern Sie beispielsweise den Wert für das **Wiederherstellungsintervall** in 10 Minuten, dauert die Wiederherstellung ungefähr 10-mal länger als bei einem **Wiederherstellungsintervall**-Wert von 1 Minute.  
  
##  <a name="indirect-checkpoints"></a><a name="IndirectChkpt"></a> Indirekte Prüfpunkte
Indirekte Prüfpunkte (eingeführt in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) stellen auf Datenbankebene eine konfigurierbare Alternative zu automatischen Prüfpunkten dar. Dies kann durch Festlegen der Datenbankkonfigurationsoption **Wiederherstellungszeit für das Ziel** konfiguriert werden. Weitere Informationen finden Sie unter [Ändern der Zielwiederherstellungszeit einer Datenbank &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md)konfiguriert wird.
Im Fall eines Systemabsturzes ermöglichen indirekte Prüfpunkte eine potenziell schnellere und besser vorhersehbare Wiederherstellungszeit als automatische Prüfpunkte. Indirekte Prüfpunkte bieten folgende Vorteile:  
  
-   Im Fall einer Arbeitsauslastung für Onlinetransaktionen bei einer Datenbank, die für indirekte Prüfpunkte konfiguriert ist, kann eine Verringerung der Leistung auftreten. Indirekte Prüfpunkte stellen sicher, dass die Anzahl der modifizierten Seiten unter einem bestimmten Schwellenwert liegt, sodass die Datenbankwiederherstellung innerhalb der Zielwiederherstellungszeit abgeschlossen wird. 

  Die Konfigurationsoption **Wiederherstellungsintervall** ermittelt die Wiederherstellungszeit über die Anzahl der Transaktionen. Im Gegensatz dazu greifen **indirekte Prüfpunkte** auf die Anzahl der modifizierten Seiten zurück. Wenn für eine Datenbank, die eine große Anzahl von DML-Vorgängen empfängt, indirekte Prüfpunkte aktiviert sind, können beim Schreiben im Hintergrund leere modifizierte Puffer aggressiv auf den Datenträger geleert werden. Dadurch wird sichergestellt, dass der Zeitaufwand für die Wiederherstellung innerhalb der Zielwiederherstellungszeit der Datenbank liegt. Dies kann auf bestimmten Systemen zusätzliche E/A-Aktivitäten verursachen, die zu einem Leistungsengpass beitragen können, wenn das Datenträgersubsystem über oder nahe dem E/A-Schwellenwert arbeitet.  
  
-   Indirekte Prüfpunkte ermöglichen Ihnen eine zuverlässige Kontrolle der Datenbankwiederherstellungszeit, indem die Kosten für das zufällige E/A-Volumen während des REDO-Vorgangs berücksichtigt werden. Dadurch überschreitet eine Serverinstanz nicht den oberen Grenzwert der Wiederherstellungszeiten für eine bestimmte Datenbank, sofern eine Transaktion mit langer Laufzeit keine übermäßig langen UNDO-Vorgänge verursacht.  
  
-   Indirekte Prüfpunkte reduzieren prüfpunktbezogene E/A-Spitzen, indem modifizierte Seiten im Hintergrund kontinuierlich auf den Datenträger geschrieben werden.  
  
Im Fall einer Arbeitsauslastung für Onlinetransaktionen bei einer Datenbank, die für indirekte Prüfpunkte konfiguriert ist, kann jedoch eine Verringerung der Leistung auftreten. Dies ist darauf zurückzuführen, dass der Hintergrundschreiber, der von indirekten Prüfpunkten verwendet wird, manchmal die Gesamtarbeitsauslastung zum Schreiben für eine Serverinstanz erhöht.  
 
> [!IMPORTANT]
> Indirekte Prüfpunkte sind das Standardverhalten für neue Datenbanken, die in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] erstellt wurden, einschließlich der Modelldatenbank und der TempDB-Datenbank.          
> Datenbanken, die direkt aktualisiert oder aus einer früheren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wiederhergestellt wurden, verwenden das frühere Verhalten der automatischen Prüfpunkte, sofern nicht explizit die Verwendung indirekter Prüfpunkte angegeben wurde.       

### <a name="improved-indirect-checkpoint-scalability"></a><a name="ctp23"></a> Verbesserte Skalierbarkeit indirekter Prüfpunkte
Vor [!INCLUDE[ssNoVersion](../../includes/sssql19-md.md)] konnten Scheduler-Fehler ohne Ergebnis auftreten, wenn eine Datenbank viele modifizierte Seiten generiert hat (z. B. `tempdb`). Mit [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] wird die Skalierbarkeit für indirekte Prüfpunkte verbessert, um diese Fehler in Datenbanken zu vermeiden, die Workloads mit vielen `UPDATE`/`INSERT`-Vorgängen enthalten.
  
##  <a name="internal-checkpoints"></a><a name="EventsCausingChkpt"></a> Interne Prüfpunkte  
Interne Prüfpunkte werden von verschiedenen Serverkomponenten generiert, um so gewährleisten zu können, dass Datenträgerabbilder dem aktuellen Protokollstatus entsprechen. Interne Prüfpunkte werden als Antwort auf die folgenden Ereignisse erstellt:  
  
-   Datenbankdateien wurden mit ALTER DATABASE hinzugefügt oder entfernt.  
  
-   Eine vollständige Datenbanksicherung wird ausgeführt.  
  
-   Eine Datenbank-Momentaufnahme wird erstellt, entweder explizit oder intern für DBCC CHECKDB.  
  
-   Eine Aktivität wird ausgeführt, für die das Herunterfahren einer Datenbank erforderlich ist. Beispielsweise besitzt AUTO_CLOSE den Status ON, und die letzte Benutzerverbindung mit der Datenbank wird geschlossen, oder eine Änderung einer Datenbankoption wird vorgenommen, für die ein Neustart der Datenbank erforderlich ist.  
  
-   Eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz wird beendet, indem der Dienst [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER) beendet wird. Durch jede der Aktionen wird ein Prüfpunkt in jeder Datenbank der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ausgelöst.  
  
-   Aktivieren des Offlinemodus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Failoverclusterinstanz (FCI).      
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks  
 **So ändern Sie das Wiederherstellungsintervall auf einer Serverinstanz**  
  
-   [Konfigurieren des Wiederherstellungsintervalls (Serverkonfigurationsoption)](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md)  
  
 **So konfigurieren Sie indirekte Prüfpunkte auf einer Datenbank**  
  
-   [Ändern der Zielwiederherstellungszeit einer Datenbank &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md)  
  
 **So geben Sie einen manuellen Prüfpunkt auf einer Datenbank aus**  
  
-   [CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)  

## <a name="see-also"></a>Weitere Informationen  
[Das Transaktionsprotokoll &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)            
[Handbuch zur Architektur und Verwaltung von Transaktionsprotokollen in SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)      
 
