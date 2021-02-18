---
description: Wiederherstellen einer SQL Server-Datenbank zu einem Zeitpunkt (vollständiges Wiederherstellungsmodell)
title: Wiederherstellen einer SQL Server-Datenbank zu einem Zeitpunkt (vollständiges Wiederherstellungsmodell) | Microsoft-Dokumentation
decription: Learn how to restore a database to a point in time in SQL Server using SQL Server Management Studio or Transact-SQL in the full or bulk-logged recovery models.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- STOPAT clause [RESTORE LOG statement]
- point in time recovery [SQL Server]
- restoring databases [SQL Server], point in time
ms.assetid: 3a5daefd-08a8-4565-b54f-28ad01a47d32
author: cawrites
ms.author: chadam
ms.openlocfilehash: 86f23acdc6dd82210c5747915d78aafc2344fc6b
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100348102"
---
# <a name="restore-a-sql-server-database-to-a-point-in-time-full-recovery-model"></a>Wiederherstellen einer SQL Server-Datenbank zu einem Zeitpunkt (vollständiges Wiederherstellungsmodell)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird erläutert, wie in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] eine Datenbank bis zu einem bestimmten Zeitpunkt mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]wiederhergestellt wird. Dieses Thema ist nur für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbanken relevant, die das vollständige oder massenprotokollierte Wiederherstellungsmodell verwenden.  
  
> [!IMPORTANT]  
>  Wenn jedoch beim massenprotokollierten Wiederherstellungsmodell die Protokollsicherung massenprotokollierte Änderungen enthält, ist die Wiederherstellung bis zu einem bestimmten Zeitpunkt nicht möglich. Die Datenbank muss bis zum Ende der Transaktionsprotokollsicherung wiederhergestellt werden.  
  
-   **Vorbereitungen:**  
  
     [Empfehlungen](#Recommendations)  
  
     [Security](#Security)  
  
-   **Wiederherstellen einer SQL Server-Datenbank zu einem bestimmten Zeitpunkt mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
  
-   Verwenden von STANDBY zum Finden eines unbekannten Zeitpunkts  
  
-   Frühes Angeben des Zeitpunkts in einer Wiederherstellungssequenz  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Ist die wiederherzustellende Datenbank nicht vorhanden, muss der Benutzer über CREATE DATABASE-Berechtigungen verfügen, um RESTORE ausführen zu können. Ist die Datenbank vorhanden, werden RESTORE-Berechtigungen standardmäßig den Mitgliedern der festen Serverrollen **sysadmin** und **dbcreator** sowie dem Besitzer (**dbo**) der Datenbank erteilt (für die Option FROM DATABASE_SNAPSHOT ist die Datenbank immer vorhanden).  
  
 RESTORE-Berechtigungen werden Rollen erteilt, in denen Mitgliedsinformationen immer für den Server verfügbar sind. Da die Mitgliedschaft in einer festen Datenbankrolle nur bei unbeschädigten und zugänglichen Datenbanken geprüft werden kann (was beim Ausführen von RESTORE nicht immer der Fall ist), verfügen Mitglieder der festen Datenbankrolle **db_owner** nicht über RESTORE-Berechtigungen.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So stellen Sie eine Datenbank bis zu einem Zeitpunkt wieder her**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der entsprechenden Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]her, und erweitern Sie die Serverstruktur.  
  
2.  Erweitern Sie **Datenbanken**. Wählen Sie je nach Datenbank entweder eine Benutzerdatenbank aus, oder erweitern Sie **Systemdatenbanken**, und wählen Sie eine Systemdatenbank aus.  
  
3.  Klicken Sie mit der rechten Maustaste auf die Datenbank, zeigen Sie auf **Tasks** und **Wiederherstellung**, und klicken Sie anschließend auf **Datenbank**.  
  
4.  Legen Sie Quelle und Speicherort der wiederherzustellenden Sicherungssätze auf der Seite **Allgemein** mithilfe des Abschnitts **Quelle** fest. Wählen Sie eine der folgenden Optionen aus:  
  
    -   **Datenbank**  
  
         Wählen Sie die wiederherzustellende Datenbank aus der Dropdownliste aus. Die Liste enthält nur Datenbanken, die entsprechend dem Sicherungsverlauf von **msdb** gesichert wurden.  
  
    > [!NOTE]  
    >  Wenn die Sicherung von einem anderen Server abgerufen wird, verfügt der Zielserver über keine Sicherungsverlaufsinformationen für die angegebene Datenbank. Wählen Sie in diesem Fall **Sicherungsmedium** aus, um die wiederherzustellende Datei oder das Medium manuell anzugeben.  
  
    -   **Device**  
  
         Klicken Sie auf die Schaltfläche zum Durchsuchen ( **...** ), um das Dialogfeld **Sicherungsmedien auswählen** zu öffnen. Wählen Sie im Feld **Sicherungsmedientyp** einen der aufgeführten Medientypen aus. Wenn Sie ein oder mehrere Medien für das Feld **Sicherungsmedien** auswählen möchten, klicken Sie auf **Hinzufügen**.  
  
         Klicken Sie nach dem Hinzufügen der gewünschten Medien zum Listenfeld **Sicherungsmedien** auf **OK** , um zur Seite **Allgemein** zurückzukehren.  
  
         Wählen Sie im Listenfeld **Quelle: Gerät: Datenbank** den Namen der Datenbank aus, die wiederhergestellt werden soll.  
  
         **Hinweis** Diese Liste ist nur verfügbar, wenn **Sicherungsmedium** ausgewählt wird. Nur Datenbanken mit Sicherungen auf dem ausgewählten Medium stehen zur Verfügung.  
  
5.  Im Abschnitt **Ziel** wird das Feld **Datenbank** automatisch mit dem Namen der Datenbank aufgefüllt, die wiederhergestellt werden soll. Geben Sie zum Ändern des Datenbanknamens den neuen Namen ins Feld **Datenbank** ein.  
  
6.  Klicken Sie auf **Zeitachse** , um auf das Dialogfeld **Sicherungszeitachse** zuzugreifen.  
  
7.  Klicken Sie im Abschnitt **Wiederherstellen in** auf **Bestimmtes Datum und bestimmte Uhrzeit**.  
  
8.  Verwenden Sie die Felder **Datum** und **Uhrzeit** oder den Schieberegler, um ein bestimmtes Datum und eine bestimmte Uhrzeit anzugeben, bis zu dem die Wiederherstellung ausgeführt werden soll. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
    > [!NOTE]  
    >  Ändern Sie im Feld **Zeitachsenintervall** die auf der Zeitachse angezeigte Zeitdauer.  
  
9. Nachdem Sie den gewünschten Zeitpunkt für die Wiederherstellung angegeben haben, stellt der Datenbankwiederherstellungsberater sicher, dass die Spalte **Wiederherstellen** des Rasters **Wiederherzustellende Sicherungssätze** nur die Sicherungen angezeigt, die für die Wiederherstellung bis zu diesem Zeitpunkt benötigt werden. Die ausgewählten Sicherungen machen den empfohlenen Wiederherstellungsplan für Ihre Zeitpunktwiederherstellung aus. Verwenden Sie nach Möglichkeit nur die für diesen Wiederherstellungsvorgang ausgewählten Sicherungen.  
  
     Weitere Informationen zu den Spalten des Rasters **Wiederherzustellende Sicherungssätze** finden Sie unter [Datenbank wiederherstellen &#40;Seite „Allgemein“&#41;](../../relational-databases/backup-restore/restore-database-general-page.md)bezeichnet) gesichert werden. Informationen zum Datenbankwiederherstellungsberater finden Sie unter [Übersicht über Wiederherstellungsvorgänge &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).  
  
10. Auf der Seite **Optionen** im Bereich **Wiederherstellungsoptionen** können Sie entsprechend Ihren Anforderungen die folgenden Optionen auswählen:  
  
    -   **Vorhandene Datenbank überschreiben (WITH REPLACE)**  
  
    -   **Replikationseinstellungen beibehalten (WITH KEEP_REPLICATION)**  
  
    -   **Zugriff auf die wiederhergestellte Datenbank einschränken (WITH RESTRICTED_USER)**  
  
     Weitere Informationen zu diesen Optionen finden Sie unter [Datenbank wiederherstellen &#40;Seite „Optionen“&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  
  
11. Aktivieren Sie eine Option für das Feld **Wiederherstellungsstatus** . In diesem Feld wird der Status der Datenbank nach dem Wiederherstellungsvorgang bestimmt.  
  
    -   **RESTORE WITH RECOVERY** ist das Standardverhalten, das die Datenbank betriebsbereit belässt, indem für Transaktionen ohne Commit ein Rollback ausgeführt wird. Zusätzliche Transaktionsprotokolle können nicht wiederhergestellt werden. Wählen Sie diese Option nur aus, wenn Sie alle benötigten Sicherungen jetzt wiederherstellen möchten.  
  
    -   **RESTORE WITH NORECOVERY** belässt die Datenbank nicht betriebsbereit und führt kein Rollback für Transaktionen ohne Commit aus. Zusätzliche Transaktionsprotokolle können wiederhergestellt werden. Die Datenbank kann erst verwendet werden, wenn sie wiederhergestellt wurde.  
  
    -   **RESTORE WITH STANDBY** belässt die Datenbank im schreibgeschützten Modus. Diese Option macht Transaktionen rückgängig, für die noch kein Commit ausgeführt wurde, speichert die Umkehraktionen aber in einer Standbydatei, damit die Auswirkungen der Wiederherstellung rückgängig gemacht werden können.  
  
     Beschreibungen der Optionen für den Bereich finden Sie unter [Datenbank wiederherstellen &#40;Seite Optionen&#41;](../../relational-databases/backup-restore/restore-database-options-page.md).  
  
12. **Erstellen der Sicherung des Protokollfragments vor dem Wiederherstellen** wird ausgewählt, wenn es für den ausgewählten Zeitpunkt erforderlich ist. Sie müssen diese Einstellung nicht ändern, können das Protokollfragment jedoch sichern, auch wenn es nicht erforderlich ist.  
  
13. Bei Wiederherstellungsvorgängen treten möglicherweise Fehler auf, wenn aktive Verbindungen zur Datenbank bestehen. Aktivieren Sie die Option **Bestehende Verbindungen schließen** , um sicherzustellen, dass alle aktiven Verbindungen zwischen [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] und der Datenbank geschlossen werden. Durch die Aktivierung dieses Kontrollkästchens wechselt die Datenbank in einen Einzelbenutzermodus, bevor Wiederherstellungsvorgänge ausgeführt werden. Außerdem wird dadurch die Datenbank auf einen Multibenutzermodus festgelegt, wenn der Vorgang abgeschlossen ist.  
  
14. Wählen Sie **Bestätigung vor Wiederherstellen jeder einzelnen Sicherung** aus, wenn Sie zwischen jedem Wiederherstellungsvorgang zur Bestätigung aufgefordert werden möchten. Dies ist in der Regel nur bei großen Datenbanken und bei der gewünschten Überwachung des Status des Wiederherstellungsvorgangs erforderlich.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **Voraussetzungen**  
  
 Die Wiederherstellung zu einem bestimmten Zeitpunkt erfolgt immer aus einer Protokollsicherung. In jeder RESTORE LOG-Anweisung der Wiederherstellungssequenz müssen Sie den Zielzeitpunkt oder die Transaktion in einer identischen STOPAT-Klausel angeben. Als Voraussetzung für eine Zeitpunktwiederherstellung müssen Sie zuerst eine vollständige Datenbanksicherung wiederherstellen, deren Endpunkt vor dem Zielwiederherstellungszeitpunkt liegt. Diese vollständige Datenbanksicherung kann älter als die letzte vollständige Datenbanksicherung sein, solange Sie dann jede nachfolgende Protokollsicherung wiederherstellen, bis zu und einschließlich der Protokollsicherung, die den Zielzeitpunkt enthält.  
  
 Damit Sie besser ermitteln können, welche Datenbanksicherung wiederhergestellt werden soll, können Sie in der RESTORE DATABASE-Anweisung die WITH STOPAT-Klausel angeben, um einen Fehler auszulösen, wenn eine Datensicherung für den angegebenen Zielzeitpunkt zu aktuell ist. Die vollständige Datensicherung wird immer wiederhergestellt, auch wenn sie den Zielzeitpunkt enthält.  
  
 **Grundlegende [!INCLUDE[tsql](../../includes/tsql-md.md)] -Syntax**  
  
 RESTORE LOG *Datenbankname* FROM <Sicherungsgerät> WITH STOPAT **=** _Zeit_ **,** RECOVERY...  
  
 Der Wiederherstellungszeitpunkt ist der Transaktionscommit, der zuletzt vor oder genau zu dem gegebenen **datetime** -Wert erfolgte, der für *time* angegeben wird.  
  
 Wenn Sie nur die Änderungen vor einem bestimmten Zeitpunkt wiederherstellen möchten, geben Sie für die einzelnen Sicherungen, die Sie wiederherstellen, WITH STOPAT **=** _time_ an. Damit stellen Sie sicher, dass der Zielzeitpunkt nicht überschritten wird.  
  
 **So stellen Sie eine Datenbank bis zu einem Zeitpunkt wieder her**  
  
> [!NOTE]  
>  Ein Beispiel für diese Prozedur finden Sie weiter unten in diesem Abschnitt unter [Beispiel (Transact-SQL)](#TsqlExample).  
  
1.  Stellen Sie eine Verbindung mit der Serverinstanz her, auf der Sie die Datenbank wiederherstellen möchten.  
  
2.  Führen Sie die RESTORE DATABASE-Anweisung mithilfe der Option NORECOVERY aus.  
  
    > [!NOTE]  
    >  Wenn in einer Teilwiederherstellungssequenz eine [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md) -Dateigruppe ausgeschlossen wird, wird die Wiederherstellung bis zu einem bestimmten Zeitpunkt nicht unterstützt. Sie können das Fortsetzen der Wiederherstellungssequenz erzwingen. Die FILESTREAM-Dateigruppen, die nicht in die RESTORE-Anweisung eingeschlossen werden, können jedoch zu keinem Zeitpunkt wiederhergestellt werden. Wenn Sie eine Wiederherstellung bis zu einem bestimmten Zeitpunkt erzwingen möchten, geben Sie die CONTINUE_AFTER_ERROR-Option zusammen mit der Option STOPAT, STOPATMARK oder STOPBEFOREMARK an. Diese müssen Sie auch in den folgenden RESTORE LOG-Anweisungen angeben. Wenn Sie CONTINUE_AFTER_ERROR angeben, ist die Teilwiederherstellungssequenz erfolgreich, und die FILESTREAM-Dateigruppe wird nicht mehr wiederherstellbar.  
  
3.  Stellen Sie die letzte differenzielle Datenbanksicherung wieder her und – sofern vorhanden –  ohne dabei die Datenbank wiederherzustellen (RESTORE DATABASE *database_name* FROM *backup_device* WITH NORECOVERY).  
  
4.  Wenden Sie jede einzelne Transaktionsprotokollsicherung in derselben Reihenfolge an, in der sie erstellt wurde, und geben Sie dabei den Zeitpunkt an, zu dem die Wiederherstellung des Protokolls beendet werden soll (RESTORE DATABASE *Datenbankname* FROM <Sicherungsgerät> WITH STOPAT **=** _Zeit_ **,** RECOVERY).  
  
    > [!NOTE]  
    >  Die Optionen RECOVERY und STOPAT. Wenn die Transaktionsprotokollsicherung den geforderten Zeitpunkt nicht enthält (z. B. wenn der angegebene Zeitpunkt hinter dem Zeitpunkt liegt, bis zu dem das Transaktionsprotokoll reicht), wird eine Warnung erzeugt, und die Datenbank wird nicht wiederhergestellt.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Beispiel (Transact-SQL)  
 Im folgenden Beispiel wird eine Datenbank in den am `12:00 AM` um `April 15, 2020` bestehenden Status wiederhergestellt und ein Wiederherstellungsvorgang gezeigt, der mehrere Protokollsicherungen umfasst. Auf dem Sicherungsmedium ( `AdventureWorksBackups`) ist die wiederherzustellende vollständige Datenbanksicherung der dritte Sicherungssatz (`FILE = 3`), die erste Protokollsicherung ist der vierte Sicherungssatz (`FILE = 4`), und die zweite Protokollsicherung ist der fünfte Sicherungssatz (`FILE = 5`).  
  
> [!IMPORTANT]  
>  Für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Datenbank wird das einfache Wiederherstellungsmodell verwendet. Um Protokollsicherungen zu ermöglichen, wurde für die Datenbank vor dem Erstellen einer vollständigen Datenbanksicherung mithilfe von `ALTER DATABASE AdventureWorks SET RECOVERY FULL`die Verwendung des vollständigen Wiederherstellungsmodells festgelegt.  
  
```  
RESTORE DATABASE AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=3, NORECOVERY;  
  
RESTORE LOG AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=4, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';  
  
RESTORE LOG AdventureWorks  
   FROM AdventureWorksBackups  
   WITH FILE=5, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';  
RESTORE DATABASE AdventureWorks WITH RECOVERY;   
GO  
  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Sichern eines Transaktionsprotokolls &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Wiederherstellen einer Datenbank bis zum Fehlerzeitpunkt im vollständigen Wiederherstellungsmodell &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [Wiederherstellen einer Datenbank bis zu einer markierten Transaktion &#40;SQL Server Management Studio&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
-   [Wiederherstellen zu einer Protokollfolgenummer &#40;SQL Server&#41;](../../relational-databases/backup-restore/recover-to-a-log-sequence-number-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Restore.ToPointInTime%2A> (SMO)  
  
## <a name="see-also"></a>Weitere Informationen  
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)  
  
  
