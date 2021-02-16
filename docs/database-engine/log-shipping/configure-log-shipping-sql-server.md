---
title: Konfigurieren des Protokollversands (SQL Server) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von SQL Server Management Studio oder Transact-SQL in SQL Server den Protokollversand konfigurieren.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], enabling
- log shipping [SQL Server], configuring
ms.assetid: c42aa04a-4945-4417-b4c7-50589d727e9c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 519377db5732e17d3301f76a189405b2af11aaa7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348325"
---
# <a name="configure-log-shipping-sql-server"></a>Konfigurieren des Protokollversands (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie der Protokollversand in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]konfiguriert wird.  
  
> [!NOTE]  
>  Von [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] und höheren Versionen wird die Sicherungskomprimierung unterstützt. Wenn Sie eine Protokollversandkonfiguration erstellen, können Sie das Verhalten der Sicherungskomprimierung von Protokollsicherungen steuern. Weitere Informationen finden Sie unter [Sicherungskomprimierung &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-compression-sql-server.md).  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Voraussetzungen](#Prerequisites)  
  
     [Security](#Security)  
  
-   **So konfigurieren Sie den Protokollversand mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [Verwandte Aufgaben](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Die primäre Datenbank muss das vollständige oder massenprotokollierte Wiederherstellungsmodell verwenden. Durch Umstellen der Datenbank auf die einfache Wiederherstellung ist der Protokollversand nicht mehr funktionsfähig.  
  
-   Vor der Konfiguration des Protokollversands müssen Sie eine Freigabe erstellen, um die Transaktionsprotokollsicherungen dem sekundären Server zur Verfügung zu stellen. Es handelt sich dabei um eine Freigabe des Verzeichnisses, in dem die Transaktionsprotokollsicherungen generiert werden. Wenn Sie z.B. die Transaktionsprotokolle im Verzeichnis C:\data\tlogs\\sichern, können Sie die Freigabe \\\\*primaryserver*\tlogs dieses Verzeichnisses erstellen.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Die gespeicherten Prozeduren für den Protokollversand erfordern die Mitgliedschaft in der festen Serverrolle **sysadmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-configure-log-shipping"></a>So konfigurieren Sie den Protokollversand  
  
1.  Klicken Sie mit der rechten Maustaste auf die Datenbank, die Sie als primäre Datenbank in der Protokollversandkonfiguration verwenden möchten, und klicken Sie dann auf **Eigenschaften**.  
  
2.  Klicken Sie unter **Seite auswählen** auf **Transaktionsprotokollversand**.  
  
3.  Aktivieren Sie das Kontrollkästchen vor **Diese Datenbank als primäre Datenbank in einer Protokollversandkonfiguration aktivieren** .  
  
4.  Klicken Sie unter **Transaktionsprotokollsicherungen** auf **Sicherungseinstellungen**.  
  
5.  Geben Sie in das Feld **Geben Sie den Netzwerkpfad zum Sicherungsordner an** den Netzwerkpfad für die Freigabe ein, die Sie für den Ordner der Transaktionsprotokollsicherung erstellt haben.  
  
6.  **Wenn sich der Sicherungsordner auf dem primären Server befindet, geben Sie einen lokalen Pfad in das Feld „Sicherungsordner“ ein.** (Wenn sich der Sicherungsordner nicht auf dem primären Server befindet, können Sie dieses Feld leer lassen.)  
  
    > [!IMPORTANT]  
    >  Falls das Konto für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst auf dem primären Server unter dem lokalen Systemkonto ausgeführt wird, müssen Sie den Sicherungsordner auf dem primären Server erstellen und einen lokalen Pfad zu diesem Ordner angeben.  
  
7.  Konfigurieren Sie die Parameter **Dateien löschen, die älter sind als** und **Warnen, wenn keine Sicherung erfolgt in** .  
  
8.  Beachten Sie den Sicherungszeitplan im Feld **Zeitplan** unter **Sicherungsauftrag**. Wenn Sie den Zeitplan für Ihre Installation anpassen möchten, klicken Sie auf **Zeitplan** , und passen Sie den Zeitplan des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents bei Bedarf an.  
  
9. [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] wird die [Sicherungskomprimierung](../../relational-databases/backup-restore/backup-compression-sql-server.md). Wenn Sie eine Protokollversandkonfiguration erstellen, können Sie das Verhalten der Sicherungskomprimierung von Protokollsicherungen steuern, indem Sie eine der folgenden Optionen auswählen: **Standardservereinstellung verwenden**, **Sicherung komprimieren** oder **Sicherung nicht komprimieren**. Weitere Informationen finden Sie unter [Log Shipping Transaction Log Backup Settings](../../relational-databases/databases/log-shipping-transaction-log-backup-settings.md).  
  
10. Klicken Sie auf **OK**.  
  
11. Klicken Sie unter **Sekundäre Serverinstanzen und Datenbanken** auf **Hinzufügen**.  
  
12. Klicken Sie auf **Verbinden** , und stellen Sie eine Verbindung mit der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] her, die Sie als sekundären Server verwenden möchten.  
  
13. Wählen Sie im Feld **Sekundäre Datenbank** eine Datenbank aus der Liste aus, oder geben Sie den Namen der Datenbank ein, die Sie erstellen möchten.  
  
14. Wählen Sie auf der Registerkarte **Sekundäre Datenbank initialisieren** die Option aus, die Sie zum Initialisieren der sekundären Datenbank verwenden möchten.  
  
    > [!NOTE]  
    >  Wenn Sie veranlassen, dass [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] die sekundäre Datenbank von einer Datenbanksicherung initialisiert, werden die Daten- und Protokolldateien der sekundären Datenbank am gleichen Speicherort abgelegt wie die Daten- und Protokolldateien der **master** -Datenbank. Dieser Speicherort unterscheidet sich wahrscheinlich vom Speicherort der Daten- und Protokolldateien der primären Datenbank.  
  
15. Geben Sie auf der Registerkarte **Dateien kopieren** im Ordner **Zielordner für kopierte Dateien** den Pfad des Ordners ein, in den die Transaktionsprotokollsicherungen kopiert werden sollen. Dieser Ordner befindet sich oft auf dem sekundären Server.  
  
16. Beachten Sie den Zeitplan für das Kopieren im Feld **Zeitplan** unter **Kopierauftrag**. Wenn Sie den Zeitplan für Ihre Installation anpassen möchten, klicken Sie auf **Zeitplan** , und passen Sie den Zeitplan des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents bei Bedarf an. Dieser Zeitplan sollte ungefähr mit dem Sicherungszeitplan übereinstimmen.  
  
17. Wählen Sie auf der Registerkarte **Wiederherstellen** unter **Datenbankstatus beim Wiederherstellen von Sicherungen** die Option **Kein Wiederherstellungsmodus** oder **Standbymodus** aus.  
    > [!IMPORTANT]  
    > **Standby-Modus** ist nur eine Option, wenn die Versionen des primären und sekundären Servers identisch sind. Wenn die Hauptversion des sekundären Servers höher ist als die des primären, ist nur **Kein Wiederherstellungsmodus** zulässig.
  
18. Wenn Sie die Option **Standbymodus** auswählen, legen Sie fest, ob die Benutzer von der sekundären Datenbank getrennt werden sollen, während der Wiederherstellungsvorgang ausgeführt wird.  
  
19. Wenn Sie den Wiederherstellungsprozess auf dem sekundären Server verzögern möchten, wählen Sie unter **Wiederherstellen von Sicherungen verzögern um mindestens** eine Verzögerungszeit aus.  
  
20. Wählen Sie unter **Warnen, wenn keine Wiederherstellung erfolgt in** einen Warnschwellenwert aus.  
  
21. Beachten Sie den Wiederherstellungszeitplan im Feld **Zeitplan** unter **Wiederherstellungsauftrag**. Wenn Sie den Zeitplan für Ihre Installation anpassen möchten, klicken Sie auf **Zeitplan** , und passen Sie den Zeitplan des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents bei Bedarf an. Dieser Zeitplan sollte ungefähr mit dem Sicherungszeitplan übereinstimmen.  
  
22. Klicken Sie auf **OK**.  
  
23. Aktivieren Sie unter **Überwachungsserverinstanz** das Kontrollkästchen **Überwachungsserverinstanz verwenden** , und klicken Sie dann auf **Einstellungen**.  
  
    > [!IMPORTANT]  
    >  Wenn Sie diese Protokollversandkonfiguration überwachen möchten, müssen Sie nun den Überwachungsserver hinzufügen. Zum späteren Hinzufügen des Überwachungsservers müssten Sie diese Protokollversandkonfiguration entfernen und sie dann durch eine neue Konfiguration ersetzen, die einen Überwachungsserver umfasst.  
  
24. Klicken Sie auf **Verbinden** , und stellen Sie eine Verbindung mit der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] her, die Sie als Überwachungsserver verwenden möchten.  
  
25. Wählen Sie unter **Überwachungsverbindungen** die Verbindungsmethode aus, die von den Sicherungs-, Kopier- und Wiederherstellungsaufträgen zum Herstellen einer Verbindung mit dem Überwachungsserver verwendet werden soll.  
  
26. Wählen Sie unter **Verlaufsbeibehaltung** aus, wie lang ein Datensatz des Protokollversandverlaufs beibehalten werden soll.  
  
27. Klicken Sie auf **OK**.  
  
28. Klicken Sie im Dialogfeld **Datenbankeigenschaften** auf **OK** , um den Konfigurationsprozess zu starten.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-configure-log-shipping"></a>So konfigurieren Sie den Protokollversand  
  
1.  Initialisieren Sie die sekundäre Datenbank, indem Sie eine vollständige Sicherung der primären Datenbank auf dem sekundären Server wiederherstellen.  
  
2.  Führen Sie auf dem primären Server [sp_add_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md) aus, um eine primäre Datenbank hinzuzufügen. Von der gespeicherten Prozedur wird die Sicherungsauftrags-ID und die primäre ID zurückgegeben.  
  
3.  Führen Sie auf dem primären Server [sp_add_jobschedule](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) aus, um einen Zeitplan für den Sicherungsauftrag hinzuzufügen.  
  
4.  Führen Sie auf dem Überwachungsserver [sp_add_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-add-log-shipping-alert-job-transact-sql.md) aus, um den Warnungsauftrag hinzuzufügen.  
  
5.  Aktivieren Sie auf dem primären Server den Sicherungsauftrag.  
  
6.  Führen Sie auf dem sekundären Server [sp_add_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-primary-transact-sql.md) aus, um die Details des primären Servers und der Datenbank zur Verfügung zu stellen. Diese gespeicherte Prozedur gibt die sekundäre ID sowie die IDs des Kopier- und des Wiederherstellungsauftrags zurück.  
  
7.  Führen Sie auf dem sekundären Server [sp_add_jobschedule](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) aus, um den Zeitplan für den Kopier- und den Wiederherstellungsauftrag festzulegen.  
  
8.  Führen Sie auf dem sekundären Server [sp_add_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md) aus, um eine sekundäre Datenbank hinzuzufügen.  
  
9. Führen Sie auf dem primären Server [sp_add_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-secondary-transact-sql.md) aus, um die erforderlichen Informationen über die neue sekundäre Datenbank dem primären Server hinzuzufügen.  
  
10. Aktivieren Sie auf dem sekundären Server den Kopier- und den Wiederherstellungsauftrag. Weitere Informationen finden sie unter [Disable or Enable a Job](../../ssms/agent/disable-or-enable-a-job.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Aktualisieren des Protokollversands auf SQL Server 2016 &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Hinzufügen einer sekundären Datenbank zu einer Protokollversandkonfiguration &#40;SQL Server&#41;](../../database-engine/log-shipping/add-a-secondary-database-to-a-log-shipping-configuration-sql-server.md)  
  
-   [Entfernen einer sekundären Datenbank aus einer Protokollversandkonfiguration &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-a-secondary-database-from-a-log-shipping-configuration-sql-server.md)  
  
-   [Entfernen des Protokollversands &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   [Anzeigen des Protokollversandberichts &#40;SQL Server Management Studio&#41;](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)  
  
-   [Überwachen des Protokollversands &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
-   [Failover zu einer sekundären Datenbank für den Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Informationen zum Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Protokollversandtabellen und gespeicherte Prozeduren](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
