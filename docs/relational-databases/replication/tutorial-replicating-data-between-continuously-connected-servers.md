---
title: 'Tutorial: Konfigurieren der Transaktionsreplikation'
description: In diesem Tutorial erfahren Sie, wie Sie die Transaktionsreplikation zwischen zwei vollständig verbundenen SQL Server-Instanzen konfigurieren.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- tutorials [SQL Server replication]
- replication [SQL Server], tutorials
- wizards [SQL Server replication]
ms.assetid: 7b18a04a-2c3d-4efe-a0bc-c3f92be72fd0
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e2a27d4fccfb24aff29668651ea914e11429910f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347120"
---
# <a name="tutorial-configure-replication-between-two-fully-connected-servers-transactional"></a>Tutorial: Konfigurieren der Replikation zwischen zwei Servern mit kontinuierlicher Verbindung (Transaktionsreplikation)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Die Transaktionsreplikation stellt eine bewährte Lösung für das Problem des Verschiebens von Daten zwischen Servern mit kontinuierlicher Verbindung dar. Mithilfe des Replikations-Assistenten können Sie eine Replikationstopologie auf einfache Weise konfigurieren und verwalten. 

In diesem Tutorial wird die Konfiguration einer Topologie für Transaktionsreplikationen für Server mit kontinuierlicher Verbindung erläutert. Weitere Informationen zur Funktionsweise der Transaktionsreplikation finden Sie unter [Übersicht über die Transaktionsreplikation](./transactional/transactional-replication.md). 
  
## <a name="what-you-will-learn"></a>Lernziele  
In diesem Tutorial erfahren Sie, wie Sie Daten aus einer Datenbank mithilfe der Transaktionsreplikation in einer anderen Datenbank veröffentlichen.  

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines Verlegers über die Transaktionsreplikation
> * Erstellen eines Abonnenten für den Verleger der Transaktion
> * Überprüfen des Abonnements und Messen der Wartezeit
  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Tutorial richtet sich an Benutzer, die zwar mit grundlegenden Datenbankvorgängen vertraut sind, aber nur über begrenzte Kenntnisse in Bezug auf die Replikation verfügen. Bevor Sie dieses Tutorial starten, sollten Sie Folgendes abgeschlossen haben: [Tutorial: Vorbereiten von SQL Server auf die Replikation](../../relational-databases/replication/tutorial-preparing-the-server-for-replication.md).  
  
Für dieses Tutorial benötigen Sie SQL Server, SQL Server Management Studio (SSMS) und eine AdventureWorks-Datenbank:  
  
- Installieren Sie auf dem Verlegerserver (Quelle) Folgendes:  
  
   - Eine beliebige [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Edition mit Ausnahme von SQL Server Express und SQL Server Compact. Diese Editionen können nicht als Verleger für Replikationen verwendet werden.   
   - Die [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] -Beispieldatenbank Aus Sicherheitsgründen werden die Beispieldatenbanken standardmäßig nicht installiert.  
  
- Installieren Sie auf dem Abonnentenserver (Ziel) eine beliebige Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit Ausnahme von [!INCLUDE[ssEW](../../includes/ssew-md.md)]. [!INCLUDE[ssEW](../../includes/ssew-md.md)] kann nicht als Abonnent einer Transaktionsreplikation verwendet werden.  
  
- Installieren Sie [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Installieren Sie die [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Laden Sie die [AdventureWorks-Beispieldatenbank](https://github.com/Microsoft/sql-server-samples/releases) herunter. Weitere Informationen zum Wiederherstellen einer Datenbank in SSMS finden Sie unter [Wiederherstellen einer Datenbank](../backup-restore/restore-a-database-backup-using-ssms.md). 
 
>[!NOTE]
> - Die Replikation wird für SQL Server-Instanzen, zwischen denen mehr als zwei Versionen liegen, nicht unterstützt. Weitere Informationen finden Sie unter [In der Replikationstopologie unterstützte SQL Server-Versionen](https://blogs.msdn.microsoft.com/repltalk/2016/08/12/suppported-sql-server-versions-in-replication-topology/).
> - Sie müssen in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger und dem Abonnenten herstellen. Dazu verwenden Sie einen Anmeldenamen eines Mitglieds der festen Serverrolle **sysadmin** ist. Weitere Informationen zu dieser Rolle finden Sie unter [Rollen auf Serverebene](../security/authentication-access/server-level-roles.md).  
  
  
**Ungefähre Dauer dieses Tutorials: 60 Minuten**  
  
## <a name="configure-the-publisher-for-transactional-replication"></a>Konfigurieren des Verlegers für die Transaktionsreplikation
In diesem Abschnitt erstellen Sie mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Transaktionsveröffentlichung, um eine gefilterte Teilmenge der **Product**-Tabelle in der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Beispieldatenbank zu veröffentlichen. Außerdem fügen Sie der Veröffentlichungszugriffsliste (PAL) die vom Verteilungs-Agent verwendete SQL Server-Anmeldung hinzu.


### <a name="create-a-publication-and-define-articles"></a>Erstellen einer Veröffentlichung und Definieren von Artikeln
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten.  
  
2. Klicken Sie mit der rechten Maustaste auf **SQL Server-Agent**, und klicken Sie anschließend auf **Starten**. Der SQL Server-Agent muss ausgeführt werden, bevor Sie die Veröffentlichung erstellen. Wenn der Agent nicht gestartet wird, müssen Sie ihn manuell über den SQL Server-Konfigurations-Manager starten. 
3. Erweitern Sie den Ordner **Replikation**, und klicken Sie mit der rechten Maustaste auf den Ordner **Lokale Veröffentlichungen**. Klicken Sie anschließend auf **Neue Veröffentlichung**. Dadurch wird der Assistent für neue Veröffentlichungen gestartet:  

   ![Optionsauswahl zum Starten des Assistenten für neue Veröffentlichungen](media/tutorial-replicating-data-between-continuously-connected-servers/newpublication.png)
  
  
3. Wählen Sie auf der Seite **Veröffentlichungsdatenbank** die Datenbank [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] aus, und klicken Sie anschließend auf **Weiter**.  
  
4. Wählen Sie auf der Seite **Veröffentlichungstyp** den Eintrag **Transaktionsveröffentlichung** aus, und klicken Sie anschließend auf **Weiter**:  

   ![Seite „Veröffentlichungstyp“ mit ausgewähltem Veröffentlichungstyp](media/tutorial-replicating-data-between-continuously-connected-servers/tranrepl.png)
  
5. Erweitern Sie auf der Seite **Artikel** den Knoten **Tabellen**, und aktivieren Sie das Kontrollkästchen neben **Product**. Erweitern Sie anschließend den Knoten **Product**, und deaktivieren Sie die Kontrollkästchen neben **ListPrice** und **StandardCost**. Wählen Sie **Weiter** aus.  

   ![Seite „Artikel“ mit ausgewählten Artikeln, die veröffentlicht werden sollen](media/tutorial-replicating-data-between-continuously-connected-servers/replarticles.png)
  
6. Klicken Sie auf der Seite **Tabellenzeilen filtern** auf **Hinzufügen**.   
  
7. Wählen Sie im Dialogfeld **Filter hinzufügen** die Spalte **SafetyStockLevel** aus. Klicken Sie anschließend auf den Pfeil nach rechts, um die Spalte zur WHERE-Klausel der Filteranweisung in der Filterabfrage hinzuzufügen. Geben Sie anschließend den WHERE-Klauselmodifizierer wie folgt ein:  
  
   ```sql  
   WHERE [SafetyStockLevel] < 500  
   ```
  
   ![Seite „Tabellenzeilen filtern“ und Dialogfeld „Filter hinzufügen“](media/tutorial-replicating-data-between-continuously-connected-servers/filter.png)
  
8. Wählen Sie **OK** und anschließend **Weiter** aus.  
  
9. Aktivieren Sie das Kontrollkästchen **Momentaufnahme sofort erstellen und zum Initialisieren von Abonnements verfügbar halten**, und wählen Sie **Weiter** aus:  

   ![Seite „Momentaufnahmen-Agent“ mit aktiviertem Kontrollkästchen](media/tutorial-replicating-data-between-continuously-connected-servers/snapshot.png)
  
10. Deaktivieren Sie auf der Seite **Agentsicherheit** das Kontrollkästchen neben **Use the security settings from the Snapshot Agent** (Sicherheitseinstellungen des Momentaufnahme-Agents verwenden).   
  
    Legen Sie für den Momentaufnahmen-Agent die **Sicherheitseinstellungen** fest, indem Sie auf die entsprechende Schaltfläche klicken. Geben Sie zuerst <*Name_des_Verlegercomputers*> **\repl_snapshot** in das Feld **Prozesskonto** und anschließend das Kennwort für dieses Konto ein. Klicken Sie anschließend auf **OK**.  

    ![Seite „Agentsicherheit“ und Dialogfeld „Sicherheit für den Momentaufnahme-Agent“](media/tutorial-replicating-data-between-continuously-connected-servers/snapshotagentsecurity.png)
  
12. Wiederholen Sie den vorherigen Schritt, um <*Name_des_Verlegercomputers*> **\repl_logreader** als Prozesskonto für den Protokolllese-Agent festzulegen. Klicken Sie anschließend auf **OK**.  

    ![Dialogfeld „Sicherheit für den Protokolllese-Agent“ und Seite „Agentsicherheit“](media/tutorial-replicating-data-between-continuously-connected-servers/logreaderagentsecurity.png)   

  
13. Geben Sie auf der Seite **Assistenten abschließen** im Feld **Veröffentlichungsname** den Namen **AdvWorksProductTrans** ein, und klicken Sie auf **Fertig stellen**:  

    ![Seite „Assistenten abschließen“ mit dem Veröffentlichungsnamen](media/tutorial-replicating-data-between-continuously-connected-servers/advworksproducttrans.png)
  
14. Klicken Sie nach Erstellung der Veröffentlichung auf **Schließen**, um den Assistenten zu beenden. 

Wenn Ihr SQL Server-Agent bei dem Versuch, die Veröffentlichung zu erstellen, nicht ausgeführt wird, wird möglicherweise die unten abgebildete Fehlermeldung angezeigt. Sie weist darauf hin, dass Ihre Veröffentlichung erfolgreich erstellt wurde, Ihr Momentaufnahmen-Agent jedoch nicht gestartet werden konnte. In diesem Fall müssen Sie den SQL Server-Agent starten und den Momentaufnahmen-Agent anschließend manuell starten. Die Anweisungen für diesen Vorgang finden Sie im folgenden Abschnitt. 

![Warnung, dass der Momentaufnahmen-Agent nicht gestartet werden konnte](media/tutorial-replicating-data-between-continuously-connected-servers/snapshotagenterror.png)
    
  
### <a name="view-the-status-of-snapshot-generation"></a>Anzeigen des Status der Momentaufnahmeerstellung  
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten und den Ordner **Replikation**.  
  
2. Klicken Sie im Ordner **Lokale Veröffentlichungen** mit der rechten Maustaste auf **AdvWorksProductTrans**, und wählen Sie anschließend **Status des Momentaufnahmen-Agents anzeigen** aus:  
   ![Menüelement im Kontextmenü zur Anzeige des Status des Momentaufnahmen-Agents](media/tutorial-replicating-data-between-continuously-connected-servers/viewsnapshot.png)
  
3. Der aktuelle Status des Auftrags des Momentaufnahmen-Agents für die Veröffentlichung wird angezeigt. Überprüfen Sie, ob der Auftrag für die Momentaufnahme erfolgreich war, bevor Sie mit dem nächsten Abschnitt fortfahren.
          
Anhand des Status des Momentaufnahmen-Agents für Ihre Veröffentlichung können Sie nachvollziehen, ob Ihr SQL Server-Agent beim Erstellen der Veröffentlichung ausgeführt wurde. Wurde er niemals ausgeführt, wählen Sie zum Starten Ihres Momentaufnahmen-Agents die Option **Starten** aus: 

![Schaltfläche „Starten“ und Statusmeldungsänderung, durch die darauf aufmerksam gemacht wird, dass der Momentaufnahmen-Agent ausgeführt wurde](media/tutorial-replicating-data-between-continuously-connected-servers/startsnapshotagent.png)
     
Wenn Ihnen hier ein Fehler angezeigt wird, finden Sie unter [Troubleshooting Snapshot Agent errors (Problembehandlung für den Momentaufnahmen-Agent)](troubleshoot-tran-repl-errors.md#find-errors-with-the-snapshot-agent) weitere Informationen.


  
### <a name="add-the-distribution-agent-login-to-the-pal"></a>Hinzufügen der Verteilungs-Agent-Anmeldung zur PAL  
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten und den Ordner **Replikation**.  
  
2. Klicken Sie im Ordner **Lokale Veröffentlichungen** mit der rechten Maustaste auf **AdvWorksProductTrans**, und wählen Sie anschließend **Eigenschaften** aus.  Das Dialogfeld **Veröffentlichungseigenschaften** wird angezeigt.    
  
   a. Wählen Sie die Seite **Veröffentlichungszugriffsliste** und anschließend **Hinzufügen** aus.  
   b. Klicken Sie im Dialogfeld **Veröffentlichungszugriff hinzufügen** zuerst auf <*Name_des_Verlegercomputers*> **\repl_distribution** und anschließend auf **OK**.
   
   ![Optionsauswahl zum Hinzufügen einer Anmeldung zur Veröffentlichungszugriffsliste](media/tutorial-replicating-data-between-continuously-connected-servers/tranreplproperties.png)

Weitere Informationen finden Sie unter [Konzepte für die Replikationsprogrammierung](../../relational-databases/replication/concepts/replication-programming-concepts.md).  
  

## <a name="create-a-subscription-to-the-transactional-publication"></a>Erstellen eines Abonnements für die Transaktionsveröffentlichung
In diesem Abschnitt fügen Sie der zuvor erstellten Veröffentlichung einen Abonnenten hinzu. In diesem Tutorial wird ein Remote-Abonnent (NODE2\SQL2016) verwendet. Sie können ein Abonnement jedoch auch lokal zum Verleger hinzufügen. 

### <a name="create-the-subscription"></a>Erstellen des Abonnements  
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten und den Ordner **Replikation**.  
  
2. Klicken Sie im Ordner **Lokale Veröffentlichungen** mit der rechten Maustaste auf die Veröffentlichung **AdvWorksProductTrans**, und wählen Sie anschließend **Neue Abonnements** aus. Der Assistent für neue Abonnements wird gestartet: 
 
   ![Optionsauswahl zum Starten des Assistenten für neue Abonnements](media/tutorial-replicating-data-between-continuously-connected-servers/newsub.png)     
  
3. Wählen Sie auf der Seite **Veröffentlichung** die Veröffentlichung **AdvWorksProductTrans** aus, und klicken Sie anschließend auf **Weiter**:  

   ![Seite „Veröffentlichung“ mit ausgewählter Veröffentlichung](media/tutorial-replicating-data-between-continuously-connected-servers/selectpub.png)
  
4. Aktivieren Sie auf der Seite **Speicherort des Verteilungs-Agents** das Optionsfeld neben **Alle Agents auf dem Verteiler ausführen**, und klicken Sie anschließend auf **Weiter**.  Weitere Informationen zu Push- und Pullabonnements finden Sie unter [Abonnieren von Veröffentlichungen](./subscribe-to-publications.md).

   ![Seite „Speicherort des Verteilungs-Agents“ mit aktivierter Option zur Ausführung aller Agents auf dem Verteiler](media/tutorial-replicating-data-between-continuously-connected-servers/runagentsatdist.png)
  
5. Klicken Sie auf der Seite **Abonnenten** auf die Schaltfläche **Abonnent hinzufügen** und anschließend in der Dropdownliste auf **SQL Server-Abonnent hinzufügen**, wenn der Name der Abonnenteninstanz nicht angezeigt wird. Dadurch wird das Dialogfeld **Verbindung mit Server herstellen** geöffnet. Geben Sie den Namen der Abonnenteninstanz ein, und klicken Sie anschließend auf **Verbinden**.  
    
   Aktivieren Sie das Kontrollkästchen neben dem Namen Ihrer Abonnenteninstanz, nachdem der Abonnent hinzugefügt wurde. Wählen Sie anschließend unter **Abonnementdatenbank** die Option **Neue Datenbank** aus.   

   ![Seite „Abonnenten“ mit Optionsauswahl zum Hinzufügen eines Abonnentenservers](media/tutorial-replicating-data-between-continuously-connected-servers/addsub.png)

7. Das Dialogfeld **Neue Datenbank** wird angezeigt. Geben Sie **ProductReplica** in das Feld **Datenbankname** ein, und wählen Sie **OK** und anschließend **Weiter** aus: 
  
   ![Eingeben eines Namens für die Abonnementdatenbank](media/tutorial-replicating-data-between-continuously-connected-servers/productreplica.png)
  
8. Klicken Sie auf der Seite **Distribution Agent Security** (Verteilungs-Agent-Sicherheit) auf die Schaltfläche mit den Auslassungspunkten ( **…** ). Geben Sie <*Publisher_Machine_Name*> **\repl_distribution** in das Feld **Prozesskonto** und anschließend das Kennwort für dieses Konto ein. Klicken Sie erst auf **OK** und anschließend auf **Weiter**.

   ![Informationen zum Verteilungskonto im Dialogfeld „Sicherheit für den Verteilungs-Agent“](media/tutorial-replicating-data-between-continuously-connected-servers/adddistaccount.png)
  
9. Wählen Sie **Fertig stellen** aus, um auf den verbleibenden Seiten die Standardwerte zu übernehmen und den Assistenten abzuschließen.  
  
### <a name="set-database-permissions-at-the-subscriber"></a>Festlegen der Datenbankberechtigungen auf dem Abonnenten  
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Abonnenten her. Erweitern Sie den Knoten **Sicherheit**, klicken Sie mit der rechten Maustaste auf **Anmeldungen**, und klicken Sie anschließend auf **Neue Anmeldung**.     
  
   a. Klicken Sie auf der Seite **Allgemein** unter **Anmeldename** auf **Suche**, und fügen Sie <*Subscriber_Machine_Name>* > **\repl_distribution** die Anmeldung hinzu.

   b. Weisen Sie auf der Seite **Benutzerzuordnungen** die **db_owner**-Anmeldungsmitgliedschaft der Datenbank **ProductReplica** zu. 

   ![Optionsauswahl zum Konfigurieren der Anmeldung beim Abonnenten](media/tutorial-replicating-data-between-continuously-connected-servers/loginforsub.png)

2. Wählen Sie **OK** aus, um das Dialogfeld **Neue Anmeldung** zu schließen. 

  
### <a name="view-the-synchronization-status-of-the-subscription"></a>Anzeigen des Synchronisierungsstatus des Abonnements  
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger her. Erweitern Sie zuerst den Serverknoten und anschließend den Ordner **Replikation**.  
  
2. Erweitern Sie im Ordner **Lokale Veröffentlichungen** die Veröffentlichung **AdvWorksProductTrans**, klicken Sie mit der rechten Maustaste auf das Abonnement in der Datenbank **ProductReplica**, und wählen Sie anschließend **Synchronisierungsstatus anzeigen** aus. Der aktuelle Synchronisierungsstatus des Abonnements wird angezeigt:

   ![Optionsauswahl zum Öffnen des Dialogfelds „Synchronisierungsstatus anzeigen“](media/tutorial-replicating-data-between-continuously-connected-servers/viewsyncstatus.png)
3. Wenn das Abonnement unter **AdvWorksProductTrans** nicht angezeigt wird, drücken Sie F5, um die Liste zu aktualisieren.  
  
Weitere Informationen finden Sie unter  
- [Initialisieren eines Abonnements mit einer Momentaufnahme](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
- [Erstellen eines Pushabonnements](../../relational-databases/replication/create-a-push-subscription.md)  
- [Abonnieren von Veröffentlichungen](../../relational-databases/replication/subscribe-to-publications.md)  

## <a name="measure-replication-latency"></a>Messen der Wartezeit
In diesem Abschnitt verwenden Sie Überwachungstoken, um zu überprüfen, ob die Änderungen auf dem Abonnenten repliziert werden, und um die Wartezeit zu ermitteln. Die Wartezeit ist die Zeit, die dafür benötigt wird, dem Abonnenten eine am Verleger vorgenommene Änderung anzuzeigen.
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger her. Erweitern Sie den Serverknoten, klicken Sie mit der rechten Maustaste auf den Ordner **Replikation**, und klicken Sie anschließend auf **Replikationsmonitor starten**:

   ![Menüelement „Replikationsmonitor starten“ im Kontextmenü](media/tutorial-replicating-data-between-continuously-connected-servers/launchreplmonitor.png)

2. Erweitern Sie im linken Bereich zuerst eine Verlegergruppe und dann die Verlegerinstanz. Klicken Sie anschließend auf die Veröffentlichung **AdvWorksProductTrans**.  
  
   a. Wählen Sie die Registerkarte **Überwachungstoken** aus.  
   b. Wählen Sie **Überwachung einfügen** aus.    
   c. Zeigen Sie die verstrichene Zeit für das Überwachungstoken in den folgenden Spalten an: **Verleger zu Verteiler**, **Verteiler zu Abonnent**, **Gesamtlatenzzeit**. Der Wert **Ausstehend** gibt an, dass das Token einen bestimmten Punkt noch nicht erreicht hat.

   ![Informationen zum Überwachungstoken](media/tutorial-replicating-data-between-continuously-connected-servers/tracertoken.png)


Weitere Informationen finden Sie unter 
- [Messen der Wartezeit und Überprüfen der Verbindungen bei Transaktionsreplikationen](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)
- [Suchen nach Fehlern bei Transaktionsreplikation-Agents](troubleshoot-tran-repl-errors.md)


## <a name="next-steps"></a>Nächste Schritte
Sie haben Ihren Verleger und Ihren Abonnenten erfolgreich für die Transaktionsreplikation konfiguriert. Nun können Sie in der **Product**-Tabelle auf dem Verleger Daten einfügen, aktualisieren oder löschen. Anschließend können Sie die **Product**-Tabelle auf dem Abonnenten abfragen, um die replizierten Änderungen anzuzeigen. 

Im nächsten Artikel lernen Sie, wie eine Mergereplikation konfiguriert wird:  

> [!div class="nextstepaction"]
> [Tutorial: Konfigurieren der Replikation zwischen einem Server und mobilen Clients (Mergereplikation)](tutorial-replicating-data-with-mobile-clients.md)