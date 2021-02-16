---
title: Übersicht über Wiederherstellungsvorgänge (SQL Server) | Microsoft-Dokumentation
description: Dieser Artikel stellt eine Übersicht über die Vorgänge dar, die eine Rolle spielen, wenn zum Wiederherstellen einer SQL Server-Datenbank nach einem Ausfall mehrere SQL Server-Sicherungen nacheinander wiederhergestellt werden.
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- restoring tables [SQL Server]
- backups [SQL Server], restore scenarios
- database backups [SQL Server], restore scenarios
- database restores [SQL Server]
- restoring [SQL Server]
- restores [SQL Server]
- table restores [SQL Server]
- restoring databases [SQL Server], about restoring databases
- database restores [SQL Server], scenarios
- accelerated database recovery
ms.assetid: e985c9a6-4230-4087-9fdb-de8571ba5a5f
author: cawrites
ms.author: chadam
ms.openlocfilehash: edc2a92b260e85c2de1ff5b4fc1758748643d174
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346767"
---
# <a name="restore-and-recovery-overview-sql-server"></a>Übersicht über Wiederherstellungsvorgänge (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Um eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank nach einem Ausfall wiederherzustellen, muss ein Datenbankadministrator einen Satz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherungen im Rahmen einer logisch folgerichtigen und sinnvollen Wiederherstellungssequenz wiederherstellen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -RESTORE WITH RECOVERY unterstützt folgendermaßen die Wiederherstellung von Daten aus Sicherungskopien einer ganzen Datenbank, einer Datendatei oder einer Datenseite:  
  
-   Datenbank ( *Vollständige Datenbankwiederherstellung*)  
  
     Die gesamte Datenbank wird wiederhergestellt. Während des Wiederherstellungsvorgangs ist die Datenbank offline.  
  
-   Datendatei ( *Dateiwiederherstellung*)  
  
     Mindestens eine Datendatei wird wiederhergestellt. Während einer Dateiwiederherstellung sind die Dateigruppen, die die Dateien enthalten, automatisch offline. Wenn Sie versuchen, auf eine Offlinedateigruppe zuzugreifen, verursacht dies einen Fehler.  
  
-   Datenseite ( *Seitenwiederherstellung*)  
  
     Mit dem vollständigen Wiederherstellungsmodell oder dem massenprotokollierten Wiederherstellungsmodell können Sie einzelne Datenbanken wiederherstellen. Die Seitenwiederherstellung kann unabhängig von der Anzahl der Dateigruppen für jede Datenbank ausgeführt werden.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherung und -Wiederherstellung funktioniert unter allen unterstützten Betriebssystemen. Informationen zu den unterstützten Betriebssystemen finden Sie unter [Hardware- und Softwareanforderungen für die Installation von SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md). Informationen zur Unterstützung von Sicherungskopien früherer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Versionen finden Sie im Kapitel [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)im Abschnitt „Kompatibilitätsunterstützung“.  
  
##  <a name="overview-of-restore-scenarios"></a><a name="RestoreScenariosOv"></a> Übersicht über Wiederherstellungsszenarien  
 Ein *Wiederherstellungsszenario* in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist ein Prozess, der die Datenbank aus Daten von mindestens einer Sicherungskopie wiederherstellt. Die unterstützten Wiederherstellungsszenarien sind vom Wiederherstellungsmodell der Datenbank und der Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]abhängig.  
  
 In der folgenden Tabelle werden die für die verschiedenen Wiederherstellungsmodelle unterstützten Wiederherstellungsszenarien eingeführt.  
  
|Wiederherstellungsszenario|Mit dem einfachen Wiederherstellungsmodell|Mit dem vollständigen/massenprotokollierten Wiederherstellungsmodell|  
|----------------------|---------------------------------|----------------------------------------------|  
|Vollständige Datenbankwiederherstellung|Dies ist die grundlegende Wiederherstellungsstrategie. Eine vollständige Datenbankwiederherstellung besteht möglicherweise nur im Wiederherstellen einer vollständigen Datenbanksicherung. Alternativ kann eine vollständige Datenbankwiederherstellung das Wiederherstellen einer vollständigen Datenbanksicherung, gefolgt vom Wiederherstellen einer differenziellen Sicherung, umfassen.<br /><br /> Weitere Informationen finden Sie unter [Vollständige Datenbankwiederherstellungen &#40;einfaches Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md).|Dies ist die grundlegende Wiederherstellungsstrategie. Eine vollständige Datenbankwiederherstellung bedeutet die Wiederherstellung einer vollständigen Datenbanksicherung und, optional, einer differenziellen Sicherung (soweit vorhanden), gefolgt von der Wiederherstellung aller darauffolgenden Protokollsicherungen (in chronologischer Reihenfolge). Um die vollständige Datenbankwiederherstellung abzuschließen, wird die letzte Protokollsicherung wiederhergestellt (RESTORE WITH RECOVERY).<br /><br /> Weitere Informationen finden Sie unter [Vollständige Datenbankwiederherstellungen &#40;vollständiges Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).|  
|File restore **\***|Stellen Sie mindestens eine beschädigte schreibgeschützte Datei wieder her, ohne die gesamte Datenbank wiederherzustellen. Die Dateiwiederherstellung ist nur verfügbar, wenn die Datenbank mindestens eine schreibgeschützte Dateigruppe aufweist.|Wiederherstellen einer oder mehrerer Dateien ohne Wiederherstellung der gesamten Datenbank. Die Dateiwiederherstellung kann ausgeführt werden, während die Datenbank offline ist oder, bei einigen Editionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], während die Datenbank online bleibt. Während einer Dateiwiederherstellung sind die Dateigruppen, die die wiederherzustellenden Dateien enthalten, immer offline.|  
|Seitenwiederherstellung|Nicht verfügbar|Stellt mindestens eine beschädigte Seite wieder her. Die Seitenwiederherstellung kann ausgeführt werden, während die Datenbank offline ist oder, bei einigen Editionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], während die Datenbank online bleibt. Während einer Seitenwiederherstellung sind die wiederherzustellenden Seiten immer offline.<br /><br /> Es muss eine fortlaufende Kette von Protokollsicherungen bis zur aktuellen Protokolldatei vorhanden sein, und alle Protokollsicherungen müssen angewendet werden, um die Seite auf den Stand der aktuellen Protokolldatei zu bringen.<br /><br /> Weitere Informationen finden Sie unter [Wiederherstellung von Seiten &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md).|  
|Schrittweise Wiederherstellung **\***|Stellen Sie die Datenbank in Phasen auf Dateigruppenebene wieder her, beginnend mit der primären Dateigruppe und allen sekundären Dateigruppen mit Lese-/Schreibzugriff.|Stellen Sie die Datenbank phasenweise auf Dateigruppenebene wieder her, beginnend mit der primären Dateigruppe.<br /><br /> Weitere Informationen finden Sie unter [Schrittweise Wiederherstellungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).|  
  
 **\*** Die Onlinewiederherstellung wird nur in der Enterprise Edition unterstützt.  

### <a name="steps-to-restore-a-database"></a>Schritte zum Wiederherstellen einer Datenbank
Zum Ausführen einer Dateiwiederherstellung führt das [!INCLUDE[ssde_md](../../includes/ssde_md.md)] zwei Schritte aus: 

-   Erstellen aller fehlenden Datenbankdateien.

-   Kopieren der Daten aus den Sicherungsmedien in die Datenbankdateien.

Zum Ausführen einer Datenbankwiederherstellung führt das [!INCLUDE[ssde_md](../../includes/ssde_md.md)] drei Schritte aus: 

-   Erstellen der Datenbank- und Transaktionsprotokolldateien, wenn sie noch nicht vorhanden sind.

-   Kopieren aller Daten-, Protokoll- und Indexseiten aus den Sicherungsmedien einer Datenbank in die Datenbankdateien. 

-   Anwenden des Transaktionsprotokolls beim so genanten [Wiederherstellungsprozess](#TlogAndRecovery).

 Unabhängig davon, wie die Daten hergestellt werden, stellt [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] sicher, dass die gesamte Datenbank vor dem Wiederherstellen logisch konsistent ist. Beispielsweise können Sie eine Datei erst wiederherstellen und online schalten, wenn sie durch ein Rollforward auf einen Stand gebracht wurde, in dem sie mit der Datenbank konsistent ist.  
  
### <a name="advantages-of-a-file-or-page-restore"></a>Vorteile von Datei- oder Seitenwiederherstellungen  
 Das Wiederherstellen von Dateien oder Seiten anstelle der vollständigen Datenbank bietet folgende Vorteile:  
  
-   Bei der Wiederherstellung geringerer Datenmengen wird weniger Zeit zum Kopieren und Wiederherstellen benötigt.  
  
-   In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist es u. U. möglich, dass während des Datei- oder Seitenwiederherstellungsprozesses andere Daten der Datenbank online bleiben.  

## <a name="recovery-and-the-transaction-log"></a><a name="TlogAndRecovery"></a> Wiederherstellung und das Transaktionsprotokoll
Für die meisten Wiederherstellungsszenarien ist es erforderlich, eine Transaktionsprotokollsicherung anzuwenden und dem [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] die Ausführung des **Wiederherstellungsprozesses** zu ermöglichen, damit die Datenbank online geschaltet wird. Wiederherstellung ist der Prozess, der von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für alle Datenbanken verwendet wird, damit diese im Hinblick auf Transaktionen in einem konsistenten bzw. fehlerfreien Zustand starten.

Im Falle eines Ausfalls oder bei einem sonstigen nicht ordnungsgemäßen Herunterfahren, bleiben die Datenbanken möglicherweise in einem Status, in dem einige Änderungen nicht vom Puffercache in die Datendateien geschrieben wurden, einige Änderungen von unvollständigen Transaktionen jedoch bereits in den Datendateien vorgenommen wurden. Wenn eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gestartet wird, führt sie basierend auf dem letzten [Datenbankprüfpunkt](../../relational-databases/logs/database-checkpoints-sql-server.md) eine Wiederherstellung der einzelnen Datenbanken aus, die drei Phasen umfasst:

-   In der **Analysephase** wird das Transaktionsprotokoll analysiert, um zu bestimmen, welches der letzte Prüfpunkt ist, und es werden die Tabelle der modifizierten Seiten (Dirty Page Table, DPT) und die Tabelle der aktiven Transaktionen (Active Transaction Table, ATT) erstellt. Die DPT enthält Einträge der Seiten, die beim Herunterfahren der Datenbank in einem modifizierten Zustand vorlagen. Die DPT enthält Einträge der Transaktionen, die beim nicht ordnungsgemäßen Herunterfahren der Datenbank aktiv waren.

-   Die **Rollforwardphase** wiederholt alle im Protokoll aufgezeichneten Änderungen, die beim Herunterfahren der Datenbank möglicherweise nicht in die Datendateien geschrieben wurden. Die für eine erfolgreiche datenbankweite Wiederherstellung erforderliche [Mindest-Protokollfolgenummer](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md#minlsn) (minLSN) finden Sie in der DPT. Sie markiert den Anfang der für alle modifizierten Seiten erforderlichen Wiederholungsvorgänge. In dieser Phase schreibt das [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] alle modifizierten Seiten, die zu Transaktionen mit Commit gehören, auf den Datenträger.

-   In der **Rollbackphase** wird ein Rollback aller in der ATT gefundenen unvollständigen Transaktionen ausgeführt, um die Integrität der Datenbank sicherzustellen. Nach dem Rollback wird die Datenbank online geschaltet, und es können keine weiteren Transaktionsprotokollsicherungen auf die Datenbank angewendet werden.

Informationen zum Fortschritt der einzelnen Phasen der Datenbankwiederherstellung werden im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-[Fehlerprotokoll](../../tools/configuration-manager/viewing-the-sql-server-error-log.md) erfasst. Der Fortschritt der Datenbankwiederherstellung kann auch mit erweiterten Ereignissen nachverfolgt werden. Weitere Informationen finden Sie im Blogbeitrag [New extended events for database recovery progress](/archive/blogs/sql_server_team/new-extended-events-for-database-recovery-progress) (Neue erweiterte Ereignisse für den Fortschritt der Datenbankwiederherstellung).

> [!NOTE]
> Wenn in einem Szenario einer schrittweisen Wiederherstellung eine schreibgeschützte Dateigruppe bereits vor der Erstellung der Dateisicherung schreibgeschützt war, ist die Anwendung von Protokollsicherungen auf die Dateigruppe unnötig und wird von der Dateiwiederherstellung ausgelassen. 

<a name="FastRecovery"></a>
> [!NOTE]
> Um die Verfügbarkeit von Datenbanken in einer Unternehmensumgebung zu maximieren, kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition eine Datenbank nach der Rollforwardphase online schalten, während die Rollbackphase noch ausgeführt wird. Dies wird als schnelle Wiederherstellung bezeichnet.

##  <a name="recovery-models-and-supported-restore-operations"></a><a name="RMsAndSupportedRestoreOps"></a> Wiederherstellungsmodelle und unterstützte Wiederherstellungsvorgänge  
 Die für eine Datenbank verfügbaren Wiederherstellungsvorgänge hängen vom Wiederherstellungsmodell ab. In der folgenden Tabelle finden Sie eine Zusammenfassung, ob und in welchem Ausmaß die verschiedenen Wiederherstellungsmodelle ein bestimmtes Wiederherstellungsszenario unterstützen.  
  
|Wiederherstellungsvorgang|Vollständiges Wiederherstellungsmodell|Massenprotokolliertes Wiederherstellungsmodell|Einfaches Wiederherstellungsmodell|  
|-----------------------|-------------------------|---------------------------------|---------------------------|  
|Datenwiederherstellung|Vollständige Wiederherstellung (falls das Protokoll verfügbar ist).|Gefahr des Datenverlusts.|Alle Daten seit der letzten vollständigen Sicherung oder differenziellen Sicherung gehen verloren.|  
|Wiederherstellung bis zu einem bestimmten Zeitpunkt|Jeder von den Protokollsicherungen abgedeckte Zeitpunkt.|Nicht zulässig, wenn die Protokollsicherung massenprotokollierte Änderungen enthält.|Wird nicht unterstützt.|  
|Dateiwiederherstellung * *\** _|Vollständige Unterstützung.|Manchmal_ *\*\** *|Verfügbar nur für schreibgeschützte sekundäre Dateien.|  
|Seitenwiederherstellung * *\** _|Vollständige Unterstützung.|Manchmal_ *\*\** *|Keine.|  
|Schrittweise Wiederherstellung (Dateigruppenebene) * *\** _|Vollständige Unterstützung.|Manchmal_ *\*\** *|Verfügbar nur für schreibgeschützte sekundäre Dateien.|  
  
 **\*** Verfügbar nur in der Enterprise Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
 **\*\*** Die erforderlichen Bedingungen finden Sie in [Einschränkungen bei der Wiederherstellung mit dem einfachen Wiederherstellungsmodell](#RMsimpleScenarios)weiter unten in diesem Thema.  
  
> [!IMPORTANT]  
> Unabhängig vom Wiederherstellungsmodell einer Datenbank kann eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherung nicht in eine [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]-Version wiederhergestellt werden, die älter als die Version ist, mit der die Sicherung erstellt wurde.  
  
## <a name="restore-scenarios-under-the-simple-recovery-model"></a><a name="RMsimpleScenarios"></a> Wiederherstellungsszenarien mit dem einfachen Wiederherstellungsmodell  
 Bei Verwendung des einfachen Wiederherstellungsmodells unterliegen Wiederherstellungsvorgänge den folgenden Einschränkungen:  
  
-   Die Dateiwiederherstellung und die schrittweise Wiederherstellung stehen nur für schreibgeschützte sekundäre Dateigruppen zur Verfügung. Informationen zu diesen Wiederherstellungsszenarien finden Sie unter [Dateiwiederherstellungen &#40;einfaches Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md) und [Schrittweise Wiederherstellungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).  
  
-   Die Seitenwiederherstellung ist nicht zulässig.  
  
-   Die Wiederherstellung bis zu einem bestimmten Zeitpunkt ist nicht zulässig.  
  
 Wenn die genannten Einschränkungen für Ihre Anforderungen nicht geeignet sind, sollten Sie die Verwendung des vollständigen Wiederherstellungsmodells in Betracht ziehen. Weitere Informationen finden Sie unter [Übersicht über Sicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
> [!IMPORTANT]  
> Unabhängig vom Wiederherstellungsmodell einer Datenbank kann eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherung nicht mit einer Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wiederhergestellt werden, die älter als die Version ist, mit der die Sicherung erstellt wurde.  
  
##  <a name="restore-under-the-bulk-logged-recovery-model"></a><a name="RMblogRestore"></a> Wiederherstellen mit dem massenprotokollierten Wiederherstellungsmodell  
 In diesem Abschnitt werden Aspekte der Wiederherstellung behandelt, die sich nur auf das massenprotokollierte Wiederherstellungsmodell beziehen, das als Zusatz zum vollständigen Wiederherstellungsmodell gedacht ist.  
  
> [!NOTE]  
> Eine Einführung in das massenprotokollierte Wiederherstellungsmodell finden Sie unter [Das Transaktionsprotokoll &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
 Im Allgemeinen ähnelt das massenprotokollierte Wiederherstellungsmodell dem vollständigen Wiederherstellungsmodell, und die für das vollständige Wiederherstellungsmodell beschriebenen Informationen gelten zudem für beide Modelle. Die Zeitpunktwiederherstellung und die Onlinewiederherstellung werden jedoch durch das massenprotokollierte Wiederherstellungsmodell beeinflusst.  
  
### <a name="restrictions-for-point-in-time-recovery"></a>Einschränkungen für die Zeitpunktwiederherstellung  
 Wenn eine Protokollsicherung, die im massenprotokollierten Wiederherstellungsmodell vorgenommen wurde, massenprotokollierte Änderungen enthält, ist eine Zeitpunktwiederherstellung nicht zulässig. Wenn versucht wird, eine Wiederherstellung bis zu einem bestimmten Zeitpunkt für eine Protokollsicherung auszuführen, die Massenänderungen enthält, treten beim Wiederherstellungsvorgang Fehler auf.  
  
### <a name="restrictions-for-online-restore"></a>Einschränkungen für die Onlinewiederherstellung  
 Eine Onlinewiederherstellungssequenz funktioniert nur, wenn folgende Bedingungen erfüllt werden:  
  
-   Alle erforderlichen Protokollsicherungen müssen vorgenommen werden, bevor die Wiederherstellungssequenz gestartet wird.  
  
-   Massenänderungen müssen gesichert werden, bevor die Onlinewiederherstellungssequenz gestartet wird.  
  
-   Wenn in der Datenbank Massenänderungen vorhanden sind, müssen alle Dateien online oder[defunct](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md)sein. (Dies bedeutet, dass die Datei kein Bestandteil der Datenbank mehr ist.)  
  
 Wenn diese Bedingungen nicht erfüllt werden, treten bei der Onlinewiederherstellungssequenz Fehler auf.  
  
> [!NOTE]  
>  Es empfiehlt sich, in das vollständige Wiederherstellungsmodell zu wechseln, bevor eine Onlinewiederherstellung gestartet wird. Weitere Informationen finden Sie unter [Wiederherstellungsmodelle &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md).  
  
 Informationen zum Ausführen einer Onlinewiederherstellung finden Sie unter [Onlinewiederherstellungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md).  
  
##  <a name="database-recovery-advisor-sql-server-management-studio"></a><a name="DRA"></a> Datenbankwiederherstellungsberater (SQL Server Management Studio)  
Der Datenbankwiederherstellungsberater erleichtert das Erstellen von Wiederherstellungsplänen, durch die optimale folgerichtige Wiederherstellungssequenzen implementiert werden. Viele bekannte Probleme mit der Datenbankwiederherstellung und von Kunden angeforderte Erweiterungen wurden behoben. Mit dem Datenbankwiederherstellungsberater werden u. a. folgende wichtige Erweiterungen eingeführt:  
  
-   **Wiederherstellungsplan-Algorithmus:**  Der zum Erstellen von Wiederherstellungsplänen verwendete Algorithmus wurde erheblich verbessert, insbesondere bei komplexen Wiederherstellungsszenarios. Viele Grenzfälle, einschließlich Verzweigungszenarien in Zeitpunktwiederherstellungen, werden effizienter als in früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]behandelt.  
  
-   **Zeitpunktwiederherstellungen:**  Der Wiederherstellungsberater für Datenbanken vereinfacht erheblich das Wiederherstellen von Datenbanken zu einem bestimmten Zeitpunkt. Die Unterstützung für Zeitpunktwiederherstellungen wird durch eine visuelle Sicherungszeitachse deutlich verbessert. Diese visuelle Zeitachse macht es möglich, einen geeigneten Zeitpunkt als Zielwiederherstellungspunkt zum Wiederherstellen einer Datenbank zu ermitteln. Die Zeitachse erleichtert das Durchlaufen eines verzweigten Wiederherstellungspfads (ein Pfad, der Wiederherstellungsverzweigungen umfasst). Ein angegebener Zeitpunktwiederherstellungsplan schließt automatisch die Sicherungen ein, die für das Wiederherstellen zum Zielzeitpunkt (Datum und Uhrzeit) relevant sind. Informationen finden Sie unter [Wiederherstellen einer SQL Server-Datenbank auf einen Zeitpunkt &#40;vollständiges Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).  
  
Weitere Informationen zum Datenbankwiederherstellungsberater finden Sie in den folgenden Blogs zur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Verwaltbarkeit:  
  
-   [Wiederherstellungsberater: Einführung](/archive/blogs/managingsql/recovery-advisor-an-introduction)  
  
-   [Wiederherstellungsberater: Verwenden von SSMS zum Erstellen/Wiederherstellen von Teilungssicherungen](/archive/blogs/managingsql/recovery-advisor-using-ssms-to-createrestore-split-backups)  

## <a name="accelerated-database-recovery"></a><a name="adr"></a> Verbesserte Wiederherstellung von Datenbanken
Die [verbesserte Wiederherstellung von Datenbanken](/azure/sql-database/sql-database-accelerated-database-recovery/) ist in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] verfügbar. Durch die verbesserte Wiederherstellung von Datenbanken wird die Verfügbarkeit von Datenbanken enorm verbessert, insbesondere bei zeitintensiven Transaktionen. Hierfür wurde der [Wiederherstellungsprozess](#TlogAndRecovery) der [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] vollständig überarbeitet. Eine Datenbank, für die die verbesserte Wiederherstellung von Datenbanken aktiviert wurde, wird nach einem Failover oder einem nicht sauberen Herunterfahren deutlich schneller wiederhergestellt. Ist diese Option aktiviert, wird bei der verbesserten Wiederherstellung von Datenbanken auch das Rollback von abgebrochenen Transaktionen mit langer Ausführungszeit deutlich schneller.

Sie können die datenbankbasierte beschleunigte Wiederherstellung für [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] mithilfe der folgenden Syntax aktivieren:

```sql
ALTER DATABASE <db_name> SET ACCELERATED_DATABASE_RECOVERY = ON;
```

> [!NOTE]
> Die verbesserte Wiederherstellung von Datenbanken bei [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] standardmäßig aktiviert.

## <a name="see-also"></a><a name="RelatedContent"></a> Weitere Ressourcen  
 [Übersicht über Sicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)      
 [Das Transaktionsprotokoll &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)     
 [Handbuch zur Architektur und Verwaltung von Transaktionsprotokollen in SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)     
 [Sichern und Wiederherstellen von SQL Server-Datenbanken](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)     
 [Anwenden von Transaktionsprotokollsicherungen (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)