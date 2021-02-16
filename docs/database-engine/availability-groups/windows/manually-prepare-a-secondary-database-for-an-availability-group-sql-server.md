---
title: Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe
description: In diesem Artikel wird beschrieben, wie Sie eine sekundäre Datenbank manuell auf die Verknüpfung mit einer Always On-Verfügbarkeitsgruppe vorbereiten.
ms.custom: seodec18
ms.date: 07/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.preparedbs.f1
- sql13.swb.availabilitygroup.configsecondarydbs.f1
helpviewer_keywords:
- secondary databases [SQL Server], in availability group
- secondary databases [SQL Server]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: 9f2feb3c-ea9b-4992-8202-2aeed4f9a6dd
author: cawrites
ms.author: chadam
ms.openlocfilehash: 772c020101b8dd5252e3c5c6971b2129d9c93863
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348404"
---
# <a name="prepare-a-secondary-database-for-an-always-on-availability-group"></a>Vorbereiten einer sekundären Datenbank auf eine Always On-Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
In diesem Thema wird erläutert, wie eine Datenbank in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)] oder PowerShell für eine Always On-Verfügbarkeitsgruppe in vorbereitet wird. Das Vorbereiten einer Datenbank erfolgt in zwei Schritten: 

1. Stellen Sie mit RESTORE WITH NORECOVERY die neuesten Datenbank- und Protokollsicherungen für jede primäre Datenbank und nachfolgende Protokollsicherungen auf jeder Serverinstanz wieder her, die das sekundäre Replikat hostet.
2. Verknüpfen Sie die wiederhergestellte Datenbank mit der Verfügbarkeitsgruppe.  
  
> [!TIP]  
>  Wenn Sie eine vorhandene Protokollversandkonfiguration haben, können Sie möglicherweise die primäre Datenbank für den Protokollversand zusammen mit mindestens einer sekundären Datenbank in ein primäres Replikat einer Verfügbarkeitsgruppe und mindestens ein sekundäres Replikat konvertieren. Weitere Informationen finden Sie weiter unten in diesem Thema im Abschnitt [Voraussetzungen für das Migrieren vom Protokollversand zu Always On-Verfügbarkeitsgruppen (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-migrating-log-shipping-to-always-on-availability-groups.md).  

##  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Voraussetzungen und Einschränkungen  
  
-   Stellen Sie sicher, dass das System, auf dem die Datenbank gespeichert werden soll, einen Datenträger mit ausreichend Speicherplatz für die sekundären Datenbanken besitzt.  
  
-   Der Name der sekundären Datenbank muss dem Namen der primären Datenbank entsprechen.  
  
-   Verwenden Sie RESTORE WITH NORECOVERY für jeden Wiederherstellungsvorgang.  
  
-   Wenn sich die sekundäre Datenbank unter einem anderen Dateipfad (einschließlich des Laufwerkbuchstabens) als die primäre Datenbank befinden muss, muss vom Wiederherstellungsbefehl auch die WITH MOVE-Option für alle Datenbankdateien verwendet werden, um für sie den Pfad der sekundären Datenbank anzugeben.  
  
-   Wenn Sie die Datenbank dateigruppenweise wiederherstellen, stellen Sie sicher, dass Sie die vollständige Datenbank wiederherstellen.  
  
-   Nach dem Wiederherstellen der Datenbank müssen Sie alle seit der letzten wiederhergestellten Datensicherung erstellten Protokollsicherungen wiederherstellen (WITH NORECOVERY).  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
  
-   Bei eigenständigen Instanzen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]sollte der Dateipfad (einschließlich des Laufwerkbuchstabens) einer sekundären Datenbank nach Möglichkeit mit dem Pfad der entsprechenden primären Datenbank übereinstimmen. Grund: Wenn beim Erstellen einer sekundären Datenbank die Datenbankdateien verschoben werden, tritt beim späteren Hinzufügen einer Datei auf der sekundären Datenbank möglicherweise ein Fehler auf und bewirkt, dass die sekundäre Datenbank angehalten wird.  
  
-   Vor dem Vorbereiten der sekundären Datenbanken sollten Sie unbedingt geplante Protokollsicherungen auf den Datenbanken in der Verfügbarkeitsgruppe anhalten, bis die Initialisierung sekundärer Replikate abgeschlossen ist.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
 Beim Sichern einer Datenbank wird die [TRUSTWORTHY-Datenbankeigenschaft](../../../relational-databases/security/trustworthy-database-property.md) auf OFF festgelegt. Deshalb ist TRUSTWORTHY bei einer neu wiederhergestellten Datenbank immer auf OFF festgelegt.  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Mitglieder der festen Serverrolle **sysadmin** und der festen Datenbankrollen **db_owner** und **db_backupoperator** verfügen standardmäßig über BACKUP DATABASE- und BACKUP LOG-Berechtigungen. Weitere Informationen finden Sie unter [BACKUP &#40;Transact-SQL&#41;](../../../t-sql/statements/backup-transact-sql.md).  
  
 Wenn die Datenbank, die wiederhergestellt wird, auf der Serverinstanz nicht vorhanden ist, erfordert die RESTORE-Anweisung CREATE DATABASE-Berechtigungen. Weitere Informationen finden Sie unter [RESTORE &#40;Transact-SQL&#41;](../../../t-sql/statements/restore-statements-transact-sql.md)nicht wiederhergestellt werden.  
  
##  <a name="use-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
> [!NOTE]  
>  Wenn die Sicherungs- und Wiederherstellungsdateipfade sowohl auf der Serverinstanz, auf der das primäre Replikat gehostet wird, als auch auf jeder Instanz identisch sind, auf der ein sekundäres Replikat gehostet wird, können Sie sekundäre Replikatdatenbanken mithilfe des [Assistenten für neue Verfügbarkeitsgruppen](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md), des [Assistenten zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)oder des [Assistenten zum Hinzufügen von Datenbanken zu Verfügbarkeitsgruppen](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)erstellen.  
  
 **So bereiten Sie eine sekundäre Datenbank vor**  
  
1.  Wenn Sie noch keine aktuelle Sicherung der primären Datenbank besitzen, erstellen Sie neue vollständige oder differenzielle Datenbanksicherung. Es wird empfohlen, diese Sicherung und nachfolgende Protokollsicherungen auf der empfohlenen Netzwerkfreigabe zu speichern.  
  
2.  Erstellen Sie mindestens eine neue Protokollsicherung der primären Datenbank.

   >[!NOTE]
   >Eine Transaktionsprotokollsicherung ist möglicherweise nicht erforderlich, wenn eine Transaktionsprotokollsicherung noch nicht in der Datenbank im primären Replikat erfasst wurde. Microsoft empfiehlt, dass Sie jedes Mal eine Transaktionsprotokollsicherung durchführen, wenn eine neue Datenbank mit der Verfügbarkeitsgruppe verknüpft wird. 
  
3.  Stellen Sie auf der Serverinstanz, die das sekundäre Replikat hostet, die vollständige Datenbanksicherung der primären (und optional eine differenzielle Sicherung) und anschließend nachfolgende Protokollsicherungen wieder her.  
  
     Aktivieren Sie auf der Seite **RESTORE DATABASE-Optionen** die Option **Datenbank nicht betriebsbereit belassen und kein Rollback für Transaktionen ohne Commit ausführen. Zusätzliche Transaktionsprotokolle können wiederhergestellt werden. (RESTORE WITH NORECOVERY)** .  
  
     Wenn sich die Dateipfade der primären Datenbank und der sekundären Datenbank unterscheiden, z. B. wenn sich die primäre Datenbank auf Laufwerk F: befindet, bei der Serverinstanz, die das sekundäre Replikat hostet, jedoch das Laufwerk F: fehlt, schließen Sie die MOVE-Option in die WITH-Klausel ein.  
  
4.  Um die Konfiguration der sekundären Datenbank abzuschließen, müssen Sie die sekundäre Datenbank mit der Verfügbarkeitsgruppe verknüpfen. Weitere Informationen finden Sie unter [Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
> [!NOTE]  
>  Informationen zum Ausführen dieser Sicherungs- und Wiederherstellungsoptionen finden Sie weiter unten in diesem Abschnitt unter [Verwandte Sicherungs- und Wiederherstellungsaufgaben](#RelatedTasks).  
  
###  <a name="related-backup-and-restore-tasks"></a><a name="RelatedTasks"></a> Verwandte Sicherungs- und Wiederherstellungsaufgaben  
 **So erstellen Sie eine Datenbanksicherung**  
  
-   [Erstellen einer vollständigen Datenbanksicherung &#40;SQL Server&#41;](../../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Erstellen einer differenziellen Datenbanksicherung &#40;SQL Server&#41;](../../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
 **So erstellen Sie eine Protokollsicherung**  
  
-   [Sichern eines Transaktionsprotokolls &#40;SQL Server&#41;](../../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
 **So stellen Sie Sicherungen wieder her**  
  
-   [Restore a Database Backup Using SSMS](../../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Wiederherstellen einer differenziellen Datenbanksicherung &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-differential-database-backup-sql-server.md)  
  
-   [Wiederherstellen einer Transaktionsprotokollsicherung &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
-   [Wiederherstellen einer Datenbank an einem neuen Speicherort &#40;SQL Server&#41;](../../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So bereiten Sie eine sekundäre Datenbank vor**  
  
> [!NOTE]  
>  Ein Beispiel für diese Prozedur finden Sie weiter oben in diesem Thema [Beispiel (Transact-SQL)](#ExampleTsql).  
  
1.  Wenn Sie keine aktuelle vollständige Sicherung der primären Datenbank besitzen, stellen Sie eine Verbindung mit der Serverinstanz her, die das primäre Replikat hostet, und erstellen Sie eine vollständige Datenbanksicherung. Es wird empfohlen, diese Sicherung und nachfolgende Protokollsicherungen auf der empfohlenen Netzwerkfreigabe zu speichern.  
  
2.  Stellen Sie auf der Serverinstanz, die das sekundäre Replikat hostet, die vollständige Datenbanksicherung der primären (und optional eine differenzielle Sicherung) und anschließend alle nachfolgenden Protokollsicherungen wieder her. Verwenden Sie WITH NORECOVERY für jeden Wiederherstellungsvorgang.  
  
     Wenn sich die Dateipfade der primären Datenbank und der sekundären Datenbank unterscheiden, z. B. wenn sich die primäre Datenbank auf Laufwerk F: befindet, bei der Serverinstanz, die das sekundäre Replikat hostet, jedoch das Laufwerk F: fehlt, schließen Sie die MOVE-Option in die WITH-Klausel ein.  
  
3.  Wurden seit der erforderlichen Protokollsicherung zusätzliche Protokollsicherungen in der primären Datenbank vorgenommen, müssen Sie diese ebenfalls auf die Serverinstanz kopieren, die das sekundäre Replikat hostet, und alle Protokollsicherungen auf die sekundäre Datenbank anwenden, beginnend mit der frühesten und mithilfe von RESTORE WITH NORECOVERY.  
  
    > [!NOTE]  
    >  Eine Protokollsicherung ist nicht vorhanden, wenn die primäre Datenbank erst kürzlich erstellt wurde und bisher keine Protokollsicherung vorgenommen wurde oder wenn das Wiederherstellungsmodell soeben von SIMPLE in FULL geändert wurde.  
  
4.  Um die Konfiguration der sekundären Datenbank abzuschließen, müssen Sie die sekundäre Datenbank mit der Verfügbarkeitsgruppe verknüpfen. Weitere Informationen finden Sie unter [Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
> [!NOTE]  
>  Informationen zum Ausführen dieser Sicherungs- und Wiederherstellungsvorgänge finden Sie weiter unten in diesem Thema unter [Verwandte Sicherungs- und Wiederherstellungsaufgaben](#RelatedTasks).  
  
###  <a name="transact-sql-example"></a><a name="ExampleTsql"></a> Beispiel für Transact-SQL  
 Im folgenden Beispiel wird eine sekundäre Datenbank vorbereitet. In diesem Beispiel wird die [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] -Beispieldatenbank verwendet, in der standardmäßig das einfache Wiederherstellungsmodell verwendet wird.  
  
1.  Damit die [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] -Datenbank verwendet werden kann, ändern Sie sie so, dass das vollständige Wiederherstellungsmodell verwendet wird.  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE MyDB1   
    SET RECOVERY FULL;  
    GO  
    ```  
  
2.  Nach dem Ändern des Wiederherstellungsmodells der Datenbank von SIMPLE in FULL erstellen Sie eine vollständige Sicherung, die zum Erstellen der sekundären Datenbank verwendet werden kann. Da das Wiederherstellungsmodell soeben geändert wurde, wird die Option WITH FORMAT angegeben, um einen neuen Mediensatz zu erstellen. Dies ist hilfreich, um die Sicherungen unter dem vollständigen Wiederherstellungsmodell von vorherigen Sicherungen zu trennen, die unter dem einfachen Wiederherstellungsmodell erstellt wurden. Im Rahmen dieses Beispiels wird die Sicherungsdatei (C:\\[!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)].bak) auf dem gleichen Laufwerk wie die Datenbank erstellt.  
  
    > [!NOTE]  
    >  Bei einer Produktionsdatenbank sollten Sie die Sicherung stets auf einem separaten Medium erstellen.  
  
     Erstellen Sie auf der Serverinstanz, die das primäre Replikat (`INSTANCE01`) hostet, folgendermaßen eine vollständige Sicherung der primären Datenbank:  
  
    ```  
    BACKUP DATABASE MyDB1   
        TO DISK = 'C:\MyDB1.bak'   
        WITH FORMAT  
    GO  
    ```  
  
3.  Kopieren Sie die vollständige Sicherung auf die Serverinstanz, die das sekundäre Replikat hostet.  
  
4.  Stellen Sie mit RESTORE WITH NORECOVERY die vollständige Sicherung auf der Serverinstanz wieder her, auf der das sekundäre Replikat gehostet wird. Der Wiederherstellungsbefehl hängt davon ab, ob die Pfade der primären und sekundären Datenbanken identisch sind.  
  
    -   **Wenn die Pfade identisch sind, führen Sie Folgendes aus:**  
  
         Stellen Sie folgendermaßen die vollständige Sicherung auf dem Computer wieder her, der das sekundäre Replikat hostet:  
  
        ```  
        RESTORE DATABASE MyDB1   
            FROM DISK = 'C:\MyDB1.bak'   
            WITH NORECOVERY  
        GO  
        ```  
  
    -   **Wenn die Pfade unterschiedlich sind, führen Sie Folgendes aus:**  
  
         Wenn sich der Pfad der sekundären Datenbank vom Pfad der primären Datenbank unterscheidet (z. B. wenn die Laufwerkbuchstaben unterschiedlich sind), ist es für das Erstellen der sekundären Datenbank erforderlich, dass der Wiederherstellungsvorgang eine MOVE-Klausel einschließt.  
  
        > [!IMPORTANT]  
        >  Wenn die Pfadnamen der primären und sekundären Datenbank unterschiedlich sind, können Sie keine Datei hinzufügen. Der Grund hierfür besteht darin, dass die Serverinstanz des sekundären Replikats beim Empfangen des Protokolls für das Hinzufügen einer Datei versucht, die neue Datei unter demselben Pfad abzulegen, der von der primären Datenbank verwendet wird.  
  
         Der folgende Befehl stellt z. B. eine Sicherung einer primären Datenbank wieder her, die sich im Datenverzeichnis der Standardinstanz von [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)](C:\Programme\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\DATA) befindet. Bei der Datenbankwiederherstellung muss die Datenbank in das Datenverzeichnis einer Remoteinstanz von [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] (*Always On1*) verschoben werden, die das sekundäre Replikat auf einem anderen Clusterknoten hostet. Dort werden die Daten und-Protokolldateien im Verzeichnis *C:\Programme\Microsoft SQL Server\MSSQL13.Always On1\MSSQL\DATA* wiederhergestellt. Der Wiederherstellungsvorgang verwendet WITH NORECOVERY, um die sekundäre Datenbank in der wiederhergestellten Datenbank zu belassen.  
  
        ```  
        RESTORE DATABASE MyDB1  
          FROM DISK='C:\MyDB1.bak'  
         WITH NORECOVERY,   
            MOVE 'MyDB1_Data' TO   
             'C:\Program Files\Microsoft SQL Server\MSSQL13.Always On1\MSSQL\DATA\MyDB1_Data.mdf',   
            MOVE 'MyDB1_Log' TO  
             'C:\Program Files\Microsoft SQL Server\MSSQL13.Always On1\MSSQL\DATA\MyDB1_Data.ldf';  
        GO  
        ```  
  
5.  Nach dem Wiederherstellen der vollständigen Sicherung müssen Sie eine Protokollsicherung für die primäre Datenbank erstellen. Beispielsweise wird das Protokoll mit der folgenden [!INCLUDE[tsql](../../../includes/tsql-md.md)]-Anweisung in der Sicherungsdatei *E:\MyDB1_log.trn* gesichert:  
  
    ```  
    BACKUP LOG MyDB1   
      TO DISK = 'E:\MyDB1_log.trn'   
    GO  
    ```  
  
6.  Sie können die Datenbank erst mit dem sekundären Replikat verknüpfen, nachdem Sie die erforderliche Protokollsicherung (und alle nachfolgenden Protokollsicherungen) angewendet haben.  
  
     So wird beispielsweise mit der folgenden [!INCLUDE[tsql](../../../includes/tsql-md.md)]-Anweisung das erste Protokoll von *C:\MyDB1.trn* wiederhergestellt:  
  
    ```  
    RESTORE LOG MyDB1   
      FROM DISK = 'E:\MyDB1_log.trn'   
        WITH FILE=1, NORECOVERY  
    GO  
    ```  
  
7.  Wenn weitere Protokollsicherungen erfolgen, bevor die Datenbank mit dem sekundären Replikat verknüpft wird, müssen Sie mit RESTORE WITH NORECOVERY auch alle Protokollsicherungen nacheinander auf der Serverinstanz wiederherstellen, die das sekundäre Replikat hostet.  
  
     So werden beispielsweise mit der folgenden [!INCLUDE[tsql](../../../includes/tsql-md.md)]-Anweisung zwei zusätzliche Protokolle von *E:\MyDB1_log.bak* wiederhergestellt:  
  
    ```  
    RESTORE LOG MyDB1   
      FROM DISK = 'E:\MyDB1_log.trn'   
        WITH FILE=2, NORECOVERY  
    GO  
    RESTORE LOG MyDB1   
      FROM DISK = 'E:\MyDB1_log.trn'   
        WITH FILE=3, NORECOVERY  
    GO  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> PowerShell  
 **So bereiten Sie eine sekundäre Datenbank vor**  
  
1.  Wenn Sie eine aktuelle Sicherung der primären Datenbank erstellen müssen, wechseln Sie mit **cd** in das Verzeichnis der Serverinstanz, die das primäre Replikat hostet.  
  
2.  Verwenden Sie das Cmdlet **Backup-SqlDatabase** , um jede Sicherung zu erstellen.  
  
3.  Wechseln Sie mit **cd** in das Verzeichnis der Serverinstanz, die das sekundäre Replikat hostet.  
  
4.  Stellen Sie die Datenbank und die Protokollsicherungen aller primären Datenbanken mit dem Cmdlet **restore-SqlDatabase** wieder her, und geben Sie dabei den Wiederherstellungsparameter **NoRecovery** an. Wenn sich die Dateipfade zwischen den Computern unterscheiden, die das primäre Replikat und das sekundäre Zielreplikat hosten, verwenden Sie ebenfalls den Wiederherstellungsparameter **RelocateFile** .  
  
    > [!NOTE]  
    >  Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
5.  Um die Konfiguration der sekundären Datenbank abzuschließen, müssen Sie sie mit der Verfügbarkeitsgruppe verknüpfen. Weitere Informationen finden Sie unter [Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
 **Einrichten und Verwenden des SQL Server PowerShell-Anbieters**  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
###  <a name="sample-backup-and-restore-script-and-command"></a><a name="ExamplePSscript"></a> Beispiele für Sicherung, Wiederherstellungsskript und Befehl  
 Mit den folgenden PowerShell-Befehlen werden eine vollständige Datenbanksicherung und ein Transaktionsprotokoll auf einer Netzwerkfreigabe gesichert und diese Sicherungen von dieser Freigabe wiederhergestellt. In diesem Beispiel wird davon ausgegangen, dass der Dateipfad, unter dem die Datenbank wiederhergestellt wird, mit dem Dateipfad identisch ist, unter dem die Datenbank gesichert wurde.  
  
```  
# Create database backup  
Backup-SqlDatabase -Database "MyDB1" -BackupFile "\\share\backups\MyDB1.bak" -ServerInstance "SourceMachine\Instance"  
# Create log backup  
Backup-SqlDatabase -Database "MyDB1" -BackupAction "Log" -BackupFile "\\share\backups\MyDB1.trn" -ServerInstance "SourceMachine\Instance"  
# Restore database backup   
Restore-SqlDatabase -Database "MyDB1" -BackupFile "\\share\backups\MyDB1.bak" -NoRecovery -ServerInstance "DestinationMachine\Instance"  
# Restore log backup   
Restore-SqlDatabase -Database "MyDB1" -BackupFile "\\share\backups\MyDB1.trn" -RestoreAction "Log" -NoRecovery -ServerInstance "DestinationMachine\Instance"  
  
```  
  
##  <a name="next-steps"></a><a name="FollowUp"></a> Nächste Schritte  
 Um die Konfiguration der sekundären Datenbank abzuschließen, müssen Sie die neu wiederhergestellte Datenbank mit der Verfügbarkeitsgruppe verknüpfen. Weitere Informationen finden Sie unter [Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)aktiviert sind, eine Always On-Verfügbarkeitsgruppe zu erstellen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE-Argumente &#40;Transact-SQL&#41;](../../../t-sql/statements/restore-statements-arguments-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../../t-sql/statements/restore-statements-transact-sql.md)   
 [Problembehandlung bei einem fehlgeschlagenen Vorgang zum Hinzufügen einer Datei &#40;AlwaysOn-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
