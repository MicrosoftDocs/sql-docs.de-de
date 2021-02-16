---
title: Wiederherstellen einer Transaktionsprotokollsicherung (SQL Server) | Microsoft-Datenbank
description: In diesem Artikel wird beschrieben, wie Sie eine Transaktionsprotokollsicherung in SQL Server mithilfe von SQL Server Management Studio oder Transact-SQL wiederherstellen.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.restoretlog.general.f1
- sql13.swb.restoretlog.options.f1
helpviewer_keywords:
- restore log
- backing up transaction logs [SQL Server], restoring
- transaction log backups [SQL Server], restoring
- restoring transaction logs [SQL Server], restoring backups
- transaction log restores [SQL Server], SQL Server Management Studio
ms.assetid: 1de2b888-78a6-4fb2-a647-ba4bf097caf3
author: cawrites
ms.author: chadam
ms.openlocfilehash: f4f4e5448a01ce9e4d644aec6dd50eaea2c31297
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100346768"
---
# <a name="restore-a-transaction-log-backup-sql-server"></a>Wiederherstellen einer Transaktionsprotokollsicherung (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird beschrieben, wie Sie in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] eine Transaktionsprotokollsicherung mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]wiederherstellen.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Voraussetzungen](#Prerequisites)  
  
     [Security](#Security)  
  
-   **Wiederherstellen einer Transaktionsprotokollsicherung mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [Verwandte Aufgaben](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Sicherungen müssen in der Reihenfolge wiederhergestellt werden, in der sie erstellt wurden. Bevor Sie eine bestimmte Transaktionsprotokollsicherung wiederherstellen können, müssen Sie zuerst die folgenden vorherigen Sicherungen wiederherstellen, ohne für Transaktionen ohne Commit ein Rollback auszuführen, also mit der Option WITH NORECOVERY:  
  
    -   Die vollständige Datenbanksicherung und die letzte, differenzielle Sicherung, die ggf. vor der betreffenden Transaktionsprotokollsicherung durchgeführt wurde. Bevor die letzte vollständige oder differenzielle Datenbanksicherung erstellt wurde, muss die Datenbank das vollständige Wiederherstellungsmodell oder das massenprotokollierte Wiederherstellungsmodell verwendet haben.  
  
    -   Alle Transaktionsprotokollsicherungen, die nach der vollständigen Datenbanksicherung oder der differenziellen Sicherung (falls Sie eine solche Sicherung wiederherstellen) und vor der betreffenden Transaktionsprotokollsicherung durchgeführt wurden. Protokollsicherungen müssen ohne Lücken in der Protokollkette in der Reihenfolge angewendet werden, in der sie erstellt wurden.  
  
         Weitere Informationen zu Transaktionsprotokollsicherungen finden Sie unter [ Transaktionsprotokollsicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md) und [Anwenden von Transaktionsprotokollsicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 RESTORE-Berechtigungen werden Rollen erteilt, in denen Mitgliedsinformationen immer für den Server verfügbar sind. Da die Mitgliedschaft in einer festen Datenbankrolle nur bei unbeschädigten und zugänglichen Datenbanken geprüft werden kann (was beim Ausführen von RESTORE nicht immer der Fall ist), verfügen Mitglieder der festen Datenbankrolle **db_owner** nicht über RESTORE-Berechtigungen.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
> [!WARNING]  
>  Beim üblichen Prozess der Wiederherstellung wählen Sie im Dialogfeld **Datenbank wiederherstellen** die Protokollsicherungen zusammen mit den Datensicherungen und den differenziellen Sicherungen aus.  
  
#### <a name="to-restore-a-transaction-log-backup"></a>So stellen Sie eine Transaktionsprotokollsicherung wieder her  
  
1.  Klicken Sie nach dem Herstellen einer Verbindung mit der entsprechenden Instanz von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] im Objekt-Explorer auf den Servernamen, um die Serverstruktur zu erweitern.  
  
2.  Erweitern Sie **Datenbanken**, und wählen Sie je nach Datenbank eine Benutzerdatenbank aus, oder erweitern Sie **Systemdatenbanken** , und wählen Sie eine Systemdatenbank aus.  
  
3.  Klicken Sie mit der rechten Maustaste auf die Datenbank, zeigen Sie auf **Aufgaben**, zeigen Sie auf **Wiederherstellen**, und klicken Sie dann auf **Transaktionsprotokoll**. Daraufhin wird das Dialogfeld **Transaktionsprotokoll wiederherstellen** geöffnet.  
  
    > [!NOTE]  
    >  Ist **Transaktionsprotokoll** ausgegraut, muss ggf. eine vollständige oder differenzielle Sicherung wiederhergestellt werden. Verwenden Sie das Sicherungsdialogfeld **Datenbank** .  
  
4.  Wählen Sie auf der Seite **Allgemein** im Listenfeld **Datenbank** den Namen einer Datenbank aus. Es werden nur Datenbanken im Wiederherstellungsstatus aufgeführt.  
  
5.  Zum Festlegen von Quelle und Speicherort der wiederherzustellenden Sicherungssätze klicken Sie auf eine der folgenden Optionen:  
  
    -   **Von vorherigen Sicherungen der Datenbank**  
  
         Wählen Sie die wiederherzustellende Datenbank aus der Dropdownliste aus. Die Liste enthält nur Datenbanken, die entsprechend dem Sicherungsverlauf von **msdb** gesichert wurden.  
  
    -   **Aus Datei oder von Band**  
  
         Klicken Sie auf die Schaltfläche zum Durchsuchen ( **...** ), um das Dialogfeld **Sicherungsmedien auswählen** zu öffnen. Wählen Sie im Feld **Sicherungsmedientyp** einen der aufgeführten Medientypen aus. Wenn Sie ein oder mehrere Medien für das Feld **Sicherungsmedien** auswählen möchten, klicken Sie auf **Hinzufügen**.  
  
         Klicken Sie nach dem Hinzufügen der gewünschten Medien zum Listenfeld **Sicherungsmedien** auf **OK** , um zur Seite **Allgemein** zurückzukehren.  
  
6.  Wählen Sie im Raster **Wählen Sie die wiederherzustellenden Transaktionsprotokollsicherungen aus** die wiederherzustellenden Sicherungen aus. In diesem Raster werden die für die ausgewählte Datenbank zur Verfügung stehenden Transaktionsprotokollsicherungen aufgeführt. Eine Protokollsicherung ist nur verfügbar, wenn der entsprechende Wert für **Erste LSN** größer als der Wert für **Letzte LSN** der Datenbank ist. Protokollsicherungen werden in der Reihenfolge der enthaltenen Protokollsequenznummern (Log Sequence Number, LSN) angezeigt und müssen in dieser Reihenfolge wiederhergestellt werden.  
  
     In der folgenden Tabelle werden die Spaltenheader des Rasters aufgelistet und deren Werte beschrieben.  
  
    |Header|Wert|  
    |------------|-----------|  
    |**Wiederherstellen**|Aktivierte Kontrollkästchen zeigen die wiederherzustellenden Sicherungssätze an.|  
    |**Name**|Name des Sicherungssatzes.|  
    |**Komponente**|Gesicherte Komponente: **Datenbank**, **Datei** oder \<blank> (bei Transaktionsprotokollen).|  
    |**Datenbank**|Name der an dem Sicherungsvorgang beteiligten Datenbank.|  
    |**Startdatum**|Datum und Uhrzeit des Sicherungsbeginns, entsprechend den Ländereinstellungen des Clients.|  
    |**Beendigungsdatum**|Datum und Uhrzeit des Sicherungsabschlusses, entsprechend den Ländereinstellungen des Clients.|  
    |**Erste LSN**|Protokollsequenznummer der ersten Transaktion im Sicherungssatz. Bei Dateisicherungen leer.|  
    |**Erste LSN**|Protokollsequenznummer der letzten Transaktion im Sicherungssatz. Bei Dateisicherungen leer.|  
    |**Prüfpunkt-LSN**|Protokollsequenznummer des letzten Prüfpunkts zum Zeitpunkt der Erstellung der Sicherung.|  
    |**Vollständige LSN**|Protokollsequenznummer der neuesten vollständigen Datenbanksicherung.|  
    |**Server**|Name der Instanz der Datenbank-Engine, durch die der Sicherungsvorgang ausgeführt wurde.|  
    |**Benutzername**|Name des Benutzers, der den Sicherungsvorgang ausgeführt hat.|  
    |**Größe**|Größe des Sicherungssatzes in Byte.|  
    |**Position**|Position des Sicherungssatzes auf dem Volume.|  
    |**Ablauf**|Datum und Uhrzeit des Zeitpunkts, zu dem die Gültigkeit des Sicherungssatzes endet.|  
  
7.  Wählen Sie eines der folgenden Szenarien aus:  
  
    -   **Zeitpunkt**  
  
         Behalten Sie entweder die Standardeinstellung bei (**Aktuellster möglicher Status**), oder wählen Sie Datum und Uhrzeit aus, indem Sie auf die Schaltfläche zum Durchsuchen klicken. Daraufhin wird das Dialogfeld **Zeitpunktwiederherstellung** geöffnet.  
  
    -   **Markierte Transaktion**  
  
         Stellen Sie für die Datenbank den Zustand zum Zeitpunkt einer zuvor markierten Transaktion wieder her. Nach Auswahl dieser Option wird das Dialogfeld **Markierte Transaktion auswählen** geöffnet, in dem ein Raster mit den in den ausgewählten Transaktionsprotokollsicherungen verfügbaren markierten Transaktionen angezeigt wird.  
  
         Standardmäßig erfolgt die Wiederherstellung bis zur markierten Transaktion (die jedoch nicht eingeschlossen wird). Um die markierte Transaktion ebenfalls wiederherzustellen, wählen Sie **Markierte Transaktion einschließen** aus.  
  
         In der folgenden Tabelle werden die Spaltenheader des Rasters aufgelistet und deren Werte beschrieben.  
  
        |Header|Wert|  
        |------------|-----------|  
        |\<blank>|Zeigt ein Kontrollkästchen zur Auswahl der Markierung an.|  
        |**Transaktionsmarkierung**|Name der markierten Transaktion, der vom Benutzer zugewiesen wurde, als für die Transaktion der Commit ausgeführt wurde.|  
        |**Date**|Datum und Uhrzeit, zu der für die Transaktion der Commit ausgeführt wurde. Als Transaktionsdatum und -uhrzeit werden das Datum und die Uhrzeit angezeigt, die in der **msdbgmarkhistory** -Tabelle aufgezeichnet wurden, nicht das Datum und die Uhrzeit des Clientcomputers.|  
        |**Beschreibung**|Die Beschreibung der markierten Transaktion, die der Benutzer angegeben hat, als für die Transaktion ein Commit ausgeführt wurde (sofern zutreffend).|  
        |**LSN**|Die Protokollfolgenummer (LSN, Log Sequence Number) der markierten Transaktion.|  
        |**Datenbank**|Der Name der Datenbank, in der für die markierte Transaktion ein Commit ausgeführt wird.|  
        |**Benutzername**|Der Name des Datenbankbenutzers, der für die markierte Transaktion ein Commit ausgeführt hat.|  
  
8.  Zum Anzeigen oder Auswählen der erweiterten Optionen klicken Sie auf **Optionen** im Bereich **Seite auswählen** .  
  
9. Im Abschnitt **Wiederherstellungsoptionen** stehen folgende Optionen zur Verfügung:  
  
    -   **Replikationseinstellungen beibehalten (WITH KEEP_REPLICATION)**  
  
         Behält die Replikationseinstellungen bei, wenn eine veröffentlichte Datenbank auf einem Server wiederhergestellt wird, auf dem die Datenbank nicht erstellt wurde.  
  
         Diese Option ist nur in Verbindung mit der Option **Datenbank betriebsbereit belassen, indem für Transaktionen ohne Commit ein Rollback ausgeführt wird ...** (wird weiter unten beschrieben) verfügbar und entspricht der Wiederherstellung einer Sicherungskopie mit der Option **RECOVERY** .  
  
         Das Überprüfen dieser Option entspricht der Verwendung der Option **KEEP_REPLICATION** in einer [!INCLUDE[tsql](../../includes/tsql-md.md)]**RESTORE** -Anweisung.  
  
    -   **Bestätigung vor Wiederherstellen jeder einzelnen Sicherung**  
  
         Bei Auswahl dieser Option wird vor dem Wiederherstellen jedes Sicherungssatzes (nach dem ersten) das Dialogfeld **Wiederherstellung fortsetzen** angezeigt, in dem Sie angeben müssen, ob Sie die Wiederherstellungssequenz fortsetzen möchten. Das Dialogfeld enthält den Namen des nächsten Mediensatzes (sofern vorhanden), den Namen des Sicherungssatzes und die Beschreibung des Sicherungssatzes.  
  
         Diese Option ist besonders dann hilfreich, wenn Sie für verschiedene Mediensätze Bänder austauschen müssen. Sie können die Option beispielsweise dann verwenden, wenn der Server nur über ein Bandlaufwerk verfügt. Klicken Sie erst auf **OK**, wenn Sie soweit sind, den Vorgang fortzusetzen.  
  
         Wenn Sie auf **Nein** klicken, verbleibt die Datenbank im Wiederherstellungsstatus. Sie können die Wiederherstellungssequenz nach der letzten abgeschlossenen Wiederherstellung fortsetzen. Verwenden Sie den Task **Datenbank wiederherstellen** erneut, wenn die nächste Sicherung eine Datensicherung oder differenzielle Sicherung ist. Wenn die nächste Sicherung eine Protokollsicherung ist, verwenden Sie den Task **Transaktionsprotokoll wiederherstellen** .  
  
    -   **Zugriff auf die wiederhergestellte Datenbank einschränken (WITH RESTRICTED_USER)**  
  
         Macht die wiederhergestellte Datenbank nur Mitgliedern von **db_owner**, **dbcreator** oder **sysadmin** verfügbar.  
  
         Das Überprüfen dieser Option entspricht der Verwendung der Option **RESTRICTED_USER** in einer [!INCLUDE[tsql](../../includes/tsql-md.md)]**RESTORE** -Anweisung.  
  
10. Geben Sie für die Optionen zum **Wiederherstellungsstatus** den Status der Datenbank nach dem Wiederherstellungsvorgang an.  
  
    -   **Belassen Sie die Datenbank betriebsbereit, indem für Transaktionen ohne Commit ein Rollback ausgeführt wird. Zusätzliche Transaktionsprotokolle können nicht wiederhergestellt werden. (RESTORE WITH RECOVERY)**  
  
         Stellt die Datenbank wieder her. Diese Option entspricht der Option **RECOVERY** in einer [!INCLUDE[tsql](../../includes/tsql-md.md)]**RESTORE** -Anweisung.  
  
         Wählen Sie diese Option nur dann aus, wenn Sie keine Protokolldateien besitzen, die Sie wiederherstellen möchten.  
  
    -   **Belassen Sie die Datenbank nicht betriebsbereit, und führen Sie kein Rollback für Transaktionen ohne Commit aus. Zusätzliche Transaktionsprotokolle können wiederhergestellt werden. (RESTORE WITH NORECOVERY)**  
  
         Belässt die Datenbank im nicht wiederhergestellten Status **RESTORING** . Diese Option entspricht der Verwendung der Option **NORECOVERY** in einer [!INCLUDE[tsql](../../includes/tsql-md.md)]**RESTORE** -Anweisung.  
  
         Wenn Sie diese Option auswählen, ist die Option **Replikationseinstellungen beibehalten** nicht verfügbar.  
  
        > [!IMPORTANT]  
        >  Aktivieren Sie diese Option stets bei einer Spiegelung oder sekundären Datenbank.  
  
    -   **Datenbank im schreibgeschützten Modus belassen. Transaktionen ohne Commit werden rückgängig gemacht, die Rückgängigaktionen werden jedoch in einer Datei gespeichert, sodass die Auswirkungen der Wiederherstellung umgekehrt werden können. (RESTORE WITH STANDBY)**  
  
         Belässt die Datenbank in einem Standbystatus. Diese Option entspricht der Verwendung der Option **STANDBY** in einer [!INCLUDE[tsql](../../includes/tsql-md.md)]**RESTORE** -Anweisung.  
  
         Bei Auswahl dieser Option müssen Sie eine Standbydatei angeben.  
  
11. Geben Sie optional im Textfeld **Standbydatei** einen Dateinamen für die Standbydatei an. Diese Option ist erforderlich, wenn Sie die Datenbank im schreibgeschützten Modus belassen. Sie können nach der Standbydatei suchen oder den Pfadnamen im Textfeld eingeben.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
> [!IMPORTANT]  
>  Es ist empfehlenswert, entweder WITH NORECOVERY oder WITH RECOVERY in jeder RESTORE-Anweisung immer explizit anzugeben, um Mehrdeutigkeit zu vermeiden. Dies ist besonders beim Schreiben von Skripts wichtig.  
  
#### <a name="to-restore-a-transaction-log-backup"></a>So stellen Sie eine Transaktionsprotokollsicherung wieder her  
  
1.  Führen Sie die RESTORE LOG-Anweisung aus, um die Transaktionsprotokollsicherung anzuwenden, und geben Sie dabei Folgendes an:  
  
    -   Den Namen der Datenbank, auf die das zu sichernde Transaktionsprotokoll angewendet wird.  
  
    -   Das Sicherungsmedium, von dem die Transaktionsprotokollsicherung wiederhergestellt wird.  
  
    -   Die NORECOVERY-Klausel.  
  
     Diese Anweisung weist die folgende Basissyntax auf:  
  
     RESTORE LOG *Datenbankname* FROM <Sicherungsgerät> WITH NORECOVERY.  
  
     Dabei ist *Datenbankname* der Name der Datenbank und <Sicherungsgerät> der Name des Mediums, auf dem sich die wiederherzustellende Protokollsicherung befindet.  
  
2.  Wiederholen Sie Schritt 1 für jede anzuwendende Transaktionsprotokollsicherung.  
  
3.  Verwenden Sie nach dem Wiederherstellen der letzten Sicherung in der Wiederherstellungssequenz die folgenden Anweisungen, um die Datenbank wiederherzustellen:  
  
    -   Wiederherstellen der Datenbank als Teil der letzten RESTORE LOG-Anweisung:  
  
        ```  
        RESTORE LOG <database_name> FROM <backup_device> WITH RECOVERY;  
        GO  
        ```  
  
    -   Warten, bis die Datenbank durch Verwendung einer getrennten RESTORE DATABASE-Anweisung wiederhergestellt wird:  
  
        ```  
        RESTORE LOG <database_name> FROM <backup_device> WITH NORECOVERY;   
        RESTORE DATABASE <database_name> WITH RECOVERY;  
        GO  
        ```  
  
         Das Warten mit der Wiederherstellung der Datenbank ermöglicht eine Überprüfung, ob alle erforderlichen Protokollsicherungen wiederhergestellt wurden. Diese Vorgehensweise ist oft ratsam, wenn Sie eine Wiederherstellung bis zu einem bestimmten Zeitpunkt ausführen.  
  
    > [!IMPORTANT]  
    >  Wenn Sie eine Spiegeldatenbank erstellen, lassen Sie den Wiederherstellungsschritt aus. Eine Spiegeldatenbank muss im Status RESTORING verbleiben.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Beispiele (Transact-SQL)  
 Standardmäßig verwendet die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Datenbank das einfache Wiederherstellungsmodell. Für die folgenden Beispiele ist es erforderlich, dass die Datenbank folgendermaßen für die Verwendung des vollständigen Wiederherstellungsmodells geändert wird:  
  
```sql  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
```  
  
#### <a name="a-applying-a-single-transaction-log-backup"></a>A. Anwenden einer einzelnen Transaktionsprotokollsicherung  
 Im folgenden Beispiel wird mit der Wiederherstellung der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Datenbank mithilfe einer vollständigen Datenbanksicherung begonnen, die sich auf einem Sicherungsmedium mit dem Namen `AdventureWorks2012_1`befindet. Dann wird die erste Transaktionsprotokollsicherung angewendet, die sich auf einem Sicherungsmedium mit dem Namen `AdventureWorks2012_log`befindet. Schließlich wird die Datenbank im Beispiel wiederhergestellt.  
  
```sql  
RESTORE DATABASE AdventureWorks2012  
   FROM AdventureWorks2012_1  
   WITH NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 1,  
   NORECOVERY;  
GO  
RESTORE DATABASE AdventureWorks2012  
   WITH RECOVERY;  
GO  
```  
  
#### <a name="b-applying-multiple-transaction-log-backups"></a>B. Anwenden mehrerer Transaktionsprotokollsicherungen  
 Im folgenden Beispiel wird mit der Wiederherstellung der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Datenbank mithilfe einer vollständigen Datenbanksicherung begonnen, die sich auf einem Sicherungsmedium mit dem Namen `AdventureWorks2012_1`befindet. Dann werden nacheinander die ersten drei Transaktionsprotokollsicherungen angewendet, die sich auf einem Sicherungsmedium mit dem Namen `AdventureWorks2012_log`befinden. Schließlich wird die Datenbank im Beispiel wiederhergestellt.  
  
```sql  
RESTORE DATABASE AdventureWorks2012  
   FROM AdventureWorks2012_1  
   WITH NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 1,  
   NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 2,  
   NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 3,  
   NORECOVERY;  
GO  
RESTORE DATABASE AdventureWorks2012  
   WITH RECOVERY;  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Sichern eines Transaktionsprotokolls &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Wiederherstellen einer Datenbank bis zum Fehlerzeitpunkt im vollständigen Wiederherstellungsmodell &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [Wiederherstellen einer SQL Server-Datenbank zu einem Zeitpunkt &#40;vollständiges Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
  
-   [Wiederherstellen einer Datenbank bis zu einer markierten Transaktion &#40;SQL Server Management Studio&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Anwenden von Transaktionsprotokollsicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)  
  
  
