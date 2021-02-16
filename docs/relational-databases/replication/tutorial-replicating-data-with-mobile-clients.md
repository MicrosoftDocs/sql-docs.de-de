---
title: 'Tutorial: Konfigurieren der Mergereplikation'
description: In diesem Tutorial erfahren Sie, wie Sie die Mergereplikation zwischen einem SQL Server und einem mobilen Client konfigurieren.
ms.custom: seo-lt-2019
ms.date: 04/03/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: quickstart
helpviewer_keywords:
- replication [SQL Server], tutorials
ms.assetid: af673514-30c7-403a-9d18-d01e1a095115
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3f29e39d8b66b5adafb4c84de6a8f1df99ce95bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348615"
---
# <a name="tutorial-configure-replication-between-a-server-and-mobile-clients-merge"></a>Tutorial: Konfigurieren der Replikation zwischen einem Server und mobilen Clients (Mergereplikation)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Die Mergereplikation stellt eine geeignete Lösung für das Problem des Verschiebens von Daten zwischen einem zentralen Server und mobilen Clients dar, die nur gelegentlich miteinander verbunden sind. Mithilfe der Replikations-Assistenten können Sie eine Mergereplikationstopologie auf einfache Weise konfigurieren und verwalten. 

In diesem Lernprogramm wird die Konfiguration einer Replikationstopologie für mobile Clients erläutert. Weitere Informationen zur Mergereplikation finden Sie unter [Mergereplikation](./merge/merge-replication.md).
  
## <a name="what-you-will-learn"></a>Lernziele  
In diesem Tutorial werden mithilfe einer Mergereplikation Daten aus einer zentralen Datenbank für mindestens einen mobilen Benutzer veröffentlicht, sodass jeder Benutzer eine eindeutig gefilterte Teilmenge von Daten erhält. 

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Konfigurieren eines Verlegers für die Mergereplikation
> * Hinzufügen eines mobilen Abonnenten für die Mergeveröffentlichung
> * Synchronisieren des Abonnements für die Mergeveröffentlichung
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Tutorial richtet sich an Benutzer, die mit grundlegenden Datenbankvorgängen vertraut sind, aber nur über begrenzte Kenntnisse in Bezug auf die Replikation verfügen. Bevor Sie dieses Tutorial starten, sollten Sie Folgendes abgeschlossen haben: [Tutorial: Vorbereiten von SQL Server auf die Replikation](../../relational-databases/replication/tutorial-preparing-the-server-for-replication.md).  
  
Für dieses Tutorial benötigen Sie SQL Server, SQL Server Management Studio (SSMS) und eine AdventureWorks-Datenbank: 
  
- Installieren Sie auf dem Verlegerserver (Quelle) Folgendes:  
  
   - Eine beliebige Version von SQL Server (SQL Server Express und SQL Server Compact ausgeschlossen) Diese Editionen können nicht als Replikationsverleger fungieren.   
   - Die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Beispieldatenbank Aus Sicherheitsgründen werden die Beispieldatenbanken standardmäßig nicht installiert.  
  
- Installieren Sie auf dem Abonnentenserver (Ziel) eine beliebige Edition von SQL Server mit Ausnahme von SQL Server Express oder SQL Server Compact. Die Veröffentlichung, die in diesem Tutorial erstellt wird, unterstützt weder SQL Server Express noch SQL Server Compact. 

- Installieren Sie [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Installieren Sie die [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Laden Sie die [AdventureWorks-Beispieldatenbank](https://github.com/Microsoft/sql-server-samples/releases) herunter. Weitere Informationen zum Wiederherstellen einer Datenbank in SSMS finden Sie unter [Wiederherstellen einer Datenbank](../backup-restore/restore-a-database-backup-using-ssms.md).  
 
  
>[!NOTE]
> - Die Replikation wird für SQL Server-Instanzen, zwischen denen mehr als zwei Versionen liegen, nicht unterstützt. Weitere Informationen finden Sie unter [In der Replikationstopologie unterstützte SQL Server-Versionen](https://blogs.msdn.microsoft.com/repltalk/2016/08/12/suppported-sql-server-versions-in-replication-topology/).
> - Sie müssen in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger und dem Abonnenten herstellen. Dazu verwenden Sie einen Anmeldenamen eines Mitglieds der festen Serverrolle **sysadmin** ist. Weitere Informationen zu dieser Rolle finden Sie unter [Rollen auf Serverebene](../security/authentication-access/server-level-roles.md).  
  
  
**Ungefähre Dauer dieses Tutorials: 60 Minuten**  
  
## <a name="configure-a-publisher-for-merge-replication"></a>Konfigurieren eines Verlegers für die Mergereplikation
In diesem Abschnitt erfahren Sie, wie Sie mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Mergeveröffentlichung erstellen, um eine Teilmenge der Tabellen **Employee**, **SalesOrderHeader** und **SalesOrderDetail** in der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Beispieldatenbank zu veröffentlichen. Diese Tabellen werden mit parametrisierten Zeilenfiltern gefiltert, sodass in den einzelnen Abonnements jeweils eine eindeutige Teilmenge der Daten enthalten ist. Außerdem fügen Sie der Veröffentlichungszugriffsliste (Publication Access List, PAL) die vom Merge-Agent verwendete [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldung hinzu.  
  
### <a name="create-merge-publication-and-define-articles"></a>Erstellen einer Mergeveröffentlichung und Definieren von Artikeln  
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten.  
  
2. Starten Sie den SQL Server-Agent, indem Sie im Objekt-Explorer erst mit der rechten Maustaste auf den Agent und anschließend mit der linken auf **Starten** klicken. Wenn mit diesem Schritt der Agent nicht gestartet wird, müssen Sie ihn manuell über den SQL Server-Konfigurations-Manager starten.  
3. Erweitern Sie den Ordner **Replikation**, und klicken Sie mit der rechten Maustaste auf **Lokale Veröffentlichungen**. Klicken Sie anschließend auf **Neue Veröffentlichung**. Der Assistent für neue Veröffentlichung wird gestartet:  

   ![Optionsauswahl zum Starten des Assistenten für neue Veröffentlichung](media/tutorial-replicating-data-between-continuously-connected-servers/newpublication.png)
  
3. Wählen Sie auf der Seite **Veröffentlichungsdatenbank** die Datenbank [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] aus, und klicken Sie anschließend auf **Weiter**. 

      
4. Wählen Sie auf der Seite **Veröffentlichungstyp** die Option **Mergeveröffentlichung** aus, und klicken Sie anschließend auf **Weiter**.  
   
5. Stellen Sie auf der Seite **Abonnententypen** sicher, dass [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] oder höher ausgewählt ist, und klicken Sie anschließend auf **Weiter**: 

    ![Seiten „Veröffentlichungstyp“ und „Abonnententypen“](media/tutorial-replicating-data-with-mobile-clients/mergerpl.png)
  
   
6. Erweitern Sie auf der Seite **Artikel** den Knoten **Tabellen**. Wählen Sie die folgenden drei Tabellen aus: **Employee**, **SalesOrderHeader** und **SalesOrderDetail**. Wählen Sie **Weiter** aus.  

   ![Tabellenauswahl auf der Seite „Artikel“](media/tutorial-replicating-data-with-mobile-clients/mergearticles.png)

   >[!NOTE]
   > Die Tabelle **Employee** enthält eine Spalte (**OrganizationNode**) vom Datentyp **hierarchyid**. Dieser Datentyp wird nur für die Replikation in SQL Server 2017 unterstützt. 
   >
   > Wenn Sie einen früheren Build als SQL Server 2017 verwenden, wird unten auf dem Bildschirm eine Meldung angezeigt, in der Sie darüber informiert werden, dass möglicherweise Daten verloren gehen können, wenn Sie diese Spalte in einer bidirektionalen Replikation verwenden. Sie können diese Meldung für dieses Tutorial ignorieren. Trotzdem sollte dieser Datentyp nur in einer Produktionsumgebung repliziert werden, wenn Sie den unterstützten Build verwenden.
   > 
   > Weitere Informationen zum Replizieren des Datentyps **hierarchyid** finden Sie unter [Verwenden von hierarchyid-Spalten in replizierten Tabellen](../../t-sql/data-types/hierarchyid-data-type-method-reference.md#using-hierarchyid-columns-in-replicated-tables).
    
  
7. Klicken Sie auf der Seite **Tabellenzeilen filtern** auf **Hinzufügen**, und klicken Sie anschließend auf **Filter hinzufügen**.  
  
8. Klicken Sie im Dialogfeld **Filter hinzufügen** auf **Employee (HumanResources)** unter **Wählen Sie die zu filternde Tabelle aus**. Klicken Sie erst auf die Spalte **LoginID** und dann auf den Pfeil nach rechts, um der WHERE-Klausel der Filterabfrage die Spalte hinzuzufügen, und ändern Sie die WHERE-Klausel wie folgt:  
  
   ```sql 
    WHERE [LoginID] = HOST_NAME()  
   ```  
  
   Klicken Sie erst auf **Eine Zeile aus dieser Tabelle wird nur an ein Abonnement gesendet** und anschließend auf **OK**.  
 
   ![Optionsauswahl zum Hinzufügen eines Filters](media/tutorial-replicating-data-with-mobile-clients/mergeaddfilter.png)

    
  
10. Klicken Sie erst auf der Seite **Tabellenzeilen filtern** auf **Employee (Human Resources)** , dann auf **Hinzufügen** und anschließend auf **Join hinzufügen, um den ausgewählten Filter zu erweitern**.  
  
    a. Wählen Sie im Dialogfeld **Join hinzufügen** unter **Verknüpfte Tabelle** den Eintrag **Sales.SalesOrderDetail** aus. Klicken Sie auf **Write the join statement manually** (Joinanweisung manuell schreiben), und schließen Sie den Vorgang wie folgt ab:  
  
    ```sql  
    ON [Employee].[BusinessEntityID] =  [SalesOrderHeader].[SalesPersonID] 
    ```  
  
    b. Klicken Sie unter **Geben Sie Joinoptionen an** auf die Option **Eindeutiger Schlüssel**, und klicken Sie anschließend auf **OK**.

    ![Optionsauswahl zum Hinzufügen eines Joins zum Filter](media/tutorial-replicating-data-with-mobile-clients/mergeaddjoin.png)

  
13. Klicken Sie erst auf der Seite **Tabellenzeilen filtern** auf **SalesOrderHeader**, dann auf **Hinzufügen** und anschließend auf **Join hinzufügen, um den ausgewählten Filter zu erweitern**.  
  
    a. Wählen Sie im Dialogfeld **Join hinzufügen** unter **Verknüpfte Tabelle** den Eintrag **Sales.SalesOrderDetail** aus.    
    b. Klicken Sie auf **Anweisung mit dem Generator erstellen**.  
    c. Überprüfen Sie im Feld **Vorschau**, dass die Joinanweisung wie folgt lautet:  
  
    ```sql  
    ON [SalesOrderHeader].[SalesOrderID] = [SalesOrderDetail].[SalesOrderID] 
    ```  
  
    d. Klicken Sie unter **Geben Sie Joinoptionen an** auf die Option **Eindeutiger Schlüssel**, und klicken Sie anschließend auf **OK**. Wählen Sie **Weiter** aus. 

    ![Optionsauswahl zum Hinzufügen eines weiteren Joins für Verkaufsaufträge](media/tutorial-replicating-data-with-mobile-clients/joinsalestables.png)
  
21. Klicken Sie auf **Momentaufnahme sofort erstellen**, deaktivieren Sie **Ausführung des Momentaufnahme-Agents zu folgenden Zeitpunkten planen**, und klicken Sie auf **Weiter**:  

    ![Optionsauswahl zum sofortigen Erstellen einer Momentaufnahme](media/tutorial-replicating-data-with-mobile-clients/snapshotagent.png)
  
22. Wählen Sie auf der Seite **Agentsicherheit** die Option **Sicherheitseinstellungen** aus. Geben Sie zuerst <*Name_des_Verlegercomputers*> **\repl_snapshot** in das Feld **Prozesskonto** und anschließend das Kennwort für dieses Konto ein. Klicken Sie anschließend auf **OK**. Wählen Sie **Weiter** aus.  

    ![Optionsauswahl zum Festlegen der Sicherheit für den Momentaufnahme-Agent](media/tutorial-replicating-data-with-mobile-clients/snapshotagentsecurity.png)
  
23. Geben Sie auf der Seite **Assistenten abschließen** im Feld **Veröffentlichungsname** den Namen **AdvWorksSalesOrdersMerge** ein, und klicken Sie auf **Fertig stellen**:  

    ![Seite „Assistenten abschließen“ mit dem Veröffentlichungsnamen](media/tutorial-replicating-data-with-mobile-clients/namemergerepl.png)
  
24. Klicken Sie auf **Schließen**, nachdem die Veröffentlichung erstellt wurde. Klicken Sie unter dem Knoten **Replikation** im **Objekt-Explorer** mit der rechten Maustaste auf **Lokale Veröffentlichungen**, und wählen Sie **Aktualisieren** aus, um Ihre neue Mergereplikation anzuzeigen.  
  
### <a name="view-the-status-of-snapshot-generation"></a>Anzeigen des Status der Momentaufnahmeerstellung  
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten und den Ordner **Replikation**.  
  
2. Klicken Sie im Ordner **Lokale Veröffentlichungen** erst mit der rechten Maustaste auf **AdvWorksSalesOrdersMerge** und anschließend mit der linken auf **Status des Momentaufnahme-Agents anzeigen**:  

   ![Optionsauswahl zum Anzeigen des Status des Momentaufnahme-Agents](media/tutorial-replicating-data-with-mobile-clients/viewsnapshotagentstatus.png)
  
3. Der aktuelle Status des Auftrags des Momentaufnahmen-Agents für die Veröffentlichung wird angezeigt. Stellen Sie sicher, dass der Momentaufnahmeauftrag erfolgreich war, bevor Sie zur nächsten Lektion wechseln.  
  
### <a name="add-the-merge-agent-login-to-the-pal"></a>Hinzufügen der Anmeldung des Merge-Agents zur PAL  
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten und den Ordner **Replikation**.  
  
2. Klicken Sie im Ordner **Lokale Veröffentlichungen** erst mit der rechten Maustaste auf **AdvWorksSalesOrdersMerge** und anschließend mit der linken auf **Eigenschaften**.  
  
   a. Wählen Sie die Seite **Veröffentlichungszugriffsliste** und anschließend **Hinzufügen** aus. 
  
   b. Klicken Sie im Dialogfeld **Veröffentlichungszugriff hinzufügen** auf <*Name_des_Verlegercomputers*> **\repl_merge** und anschließend auf **OK**. Wählen Sie erneut **OK** aus. 

   ![Optionsauswahl zum Hinzufügen der Anmeldung des Merge-Agents](media/tutorial-replicating-data-with-mobile-clients/mergepal.png) 

  
Weitere Informationen finden Sie unter  
- [Filtern von veröffentlichten Daten](../../relational-databases/replication/publish/filter-published-data.md) 
- [Parametrisierte Zeilenfilter](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
- [Definieren eines Artikels](../../relational-databases/replication/publish/define-an-article.md)  
  
  
## <a name="create-a-subscription-to-the-merge-publication"></a>Erstellen eines Abonnements für die Mergeveröffentlichung
In diesem Abschnitt erfahren Sie, wie Sie der zuvor erstellten Mergeveröffentlichung ein Abonnement hinzufügen. In diesem Tutorial wird der Remoteabonnent (NODE2\SQL2016) verwendet. Anschließend erstellen Sie Berechtigungen für die Abonnementdatenbank und generieren die gefilterte Datenmomentaufnahme für das neue Abonnement manuell.   
  
### <a name="add-a-subscriber-for-merge-publication"></a>Hinzufügen eines Abonnenten für die Mergeveröffentlichung
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Abonnenten her, und erweitern Sie den Serverknoten. Erweitern Sie den Ordner **Replikation**, klicken Sie erst mit der rechten Maustaste auf den Ordner **Lokale Abonnements** und dann mit der linken auf **Neue Abonnements**. Der Assistent für neue Abonnements wird gestartet:

   ![Optionsauswahl zum Starten des Assistenten für neue Abonnements](media/tutorial-replicating-data-with-mobile-clients/newsub.png)
  
2. Klicken Sie auf der Seite **Veröffentlichung** in der Liste **Verleger** auf **SQL Server-Verleger** suchen.  
  
   Geben Sie im Dialogfeld **Verbindung mit Server herstellen** im Feld **Servername** den Namen der Verlegerinstanz ein, und klicken Sie auf **Verbinden**. 

   ![Optionsauswahl zum Hinzufügen eines Verlegers](media/tutorial-replicating-data-with-mobile-clients/publication.png)
  
4. Klicken Sie erst auf **AdvWorksSalesOrdersMerge** und dann auf **Weiter**.  
  
5. Klicken Sie auf der Seite **Speicherort des Merge-Agents** auf **Jeden Agent auf seinem Abonnenten ausführen**, und klicken Sie anschließend auf **Weiter**:  

   ![Option „Jeden Agent auf seinem Abonnenten ausführen“](media/tutorial-replicating-data-with-mobile-clients/pullsub.png)
  
6. Wählen Sie auf der Seite **Abonnenten** den Instanznamen des Abonnentenservers aus. Wählen Sie unter **Abonnementdatenbank** die Option **Neue Datenbank** in der Liste aus.  
  
   Geben Sie im Dialogfeld **Neue Datenbank** den Namen **SalesOrdersReplica** in das Feld **Datenbankname** ein. Wählen Sie **OK** und anschließend **Weiter** aus. 

   ![Optionsauswahl zum Hinzufügen einer Datenbank zum Abonnenten](media/tutorial-replicating-data-with-mobile-clients/addsubdb.png)
  
8. Klicken Sie auf der Seite **Sicherheit für den Merge-Agent** auf die Schaltfläche mit den Auslassungspunkten ( **…** ). Geben Sie zuerst <*Name_des_Abonnentencomputers*> **\repl_merge** in das Feld **Prozesskonto** und anschließend das Kennwort für dieses Konto ein. Wählen Sie nacheinander **OK**, **Weiter** und anschließend noch einmal **Weiter** aus.  

   ![Optionsauswahl für die Sicherheit für den Merge-Agent](media/tutorial-replicating-data-with-mobile-clients/mergeagentsecurity.png)

9. Legen Sie auf der Seite **Synchronisierungszeitplan** den **Agentzeitplan** auf **Nur bedarfsgesteuert ausführen** fest. Wählen Sie **Weiter** aus.  

   ![Auswahl "Nur bedarfsgesteuert ausführen" für den Agent](media/tutorial-replicating-data-with-mobile-clients/mergesyncschedule.png)
  
9. Wählen Sie auf der Seite **Abonnements initialisieren** aus der Liste **Initialisierungszeitpunkt** die Option **Bei der ersten Synchronisierung** aus. Wählen Sie **Weiter** aus, um zur Seite **Abonnementtyp** zu wechseln, und wählen Sie den entsprechenden Abonnementtyp aus. In diesem Tutorial wird **Client** verwendet. Nachdem Sie den Abonnementtyp ausgewählt haben, wählen Sie erneut **Weiter** aus. 

   ![Optionsauswahl zum Initialisieren von Abonnements bei der ersten Synchronisierung](media/tutorial-replicating-data-with-mobile-clients/firstsync.png)

10. Geben Sie auf der Seite **HOST_NAME-Werte** im Feld **HOST_NAME-Wert** den Wert **adventure-works\pamela0** ein. Wählen Sie dann **Fertig stellen** aus.  

    ![Seite „HOST_NAME-Werte“](media/tutorial-replicating-data-with-mobile-clients/hostname.png)
  
11. Wählen Sie erneut **Fertig stellen** aus. Klicken Sie auf **Schließen**, nachdem das Abonnement erstellt wurde.  

### <a name="set-server-permissions-at-the-subscriber"></a>Festlegen der Serverberechtigungen auf dem Abonnenten  
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Abonnenten her. Erweitern Sie den Knoten **Sicherheit**, klicken Sie mit der rechten Maustaste auf **Anmeldungen**, und klicken Sie anschließend auf **Neue Anmeldung**.  
  
   Klicken Sie auf der Seite **Allgemein** auf **Suchen**, und geben Sie dann im Feld **Geben Sie den Objektnamen ein** die Zeichenfolge <*Name_des_Abonnentencomputers*> **\repl_merge** ein. Klicken Sie auf **Namen überprüfen** und dann auf **OK**. 
    
   ![Optionsauswahl zum Festlegen der Anmeldung](media/tutorial-replicating-data-with-mobile-clients/sublogin.png)
  
1. Wählen Sie auf der Seite **Benutzerzuordnung** die Datenbank **SalesOrdersReplica** und die Rolle **db_owner** aus. Erteilen Sie auf der Seite **Sicherungsfähige Elemente** die Berechtigung **Explizit** für **Ablaufverfolgung ändern**. Klicken Sie auf **OK**.

   ![Seiten „Benutzerzuordnung“ und „Sicherungsfähige Elemente“](media/tutorial-replicating-data-with-mobile-clients/setdbo.png)
  
### <a name="create-the-filtered-data-snapshot-for-the-subscription"></a>Erstellen der gefilterten Datenmomentaufnahme für das Abonnement  
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten und den Ordner **Replikation**.  
  
2. Klicken Sie im Ordner **Lokale Veröffentlichungen** erst mit der rechten Maustaste auf die **AdvWorksSalesOrdersMerge**-Veröffentlichung und anschließend mit der linken auf **Eigenschaften**.  
   
   a. Klicken Sie erst auf die Seite **Datenpartitionen** und dann auf **Hinzufügen**.   
   b. Geben Sie im Dialogfeld **Datenpartition hinzufügen** im Feld **HOST_NAME-Wert** den Wert **adventure-works\pamela0** ein, und klicken Sie anschließend auf **OK**.  
   c. Wählen Sie die neu hinzugefügte Partition aus, klicken Sie erst auf **Die ausgewählten Momentaufnahmen jetzt generieren** und anschließend auf **OK**. 

   ![Optionsauswahl zum Hinzufügen einer Partition](media/tutorial-replicating-data-with-mobile-clients/partition.png)
  
  
Weitere Informationen finden Sie unter  
- [Abonnieren von Veröffentlichungen](../../relational-databases/replication/subscribe-to-publications.md)  
- [Erstellen eines Pullabonnements](../../relational-databases/replication/create-a-pull-subscription.md)  
- [Momentaufnahmen für Mergeveröffentlichungen mit parametrisierten Filtern](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)  

## <a name="synchronize-the-subscription-to-the-merge-publication"></a>Synchronisieren des Abonnements für die Mergeveröffentlichung

In diesem Abschnitt erfahren Sie, wie Sie den Merge-Agent starten können, um das Abonnement mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] zu initialisieren. Mithilfe dieser Prozedur können Sie außerdem eine Synchronisierung mit dem Verleger ausführen.   
  
### <a name="start-synchronization-and-initialize-the-subscription"></a>Starten der Synchronisierung und Initialisieren des Abonnements  
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Abonnenten her.  
2. Stellen Sie sicher, dass der SQL Server-Agent ausgeführt wird. Wenn das nicht der Fall ist, klicken Sie im Objekt-Explorer erst mit der rechten Maustaste auf den SQL Server-Agent und anschließend mit der linken auf **Starten**. Wenn mit diesem Schritt der Agent nicht gestartet wird, müssen Sie ihn manuell über den SQL Server-Konfigurations-Manager starten. 
  
2. Erweitern Sie den Knoten **Replikation**. Klicken Sie im Ordner **Lokale Abonnements** mit der rechten Maustaste auf das Abonnement in der Datenbank **SalesOrdersReplica**, und klicken Sie anschließend auf **Synchronisierungsstatus anzeigen**.  
  
   Klicken Sie auf **Starten**, um das Abonnement zu initialisieren. 

   ![Synchronisierungsstatus mit Schaltfläche „Starten“](media/tutorial-replicating-data-with-mobile-clients/mergesyncstatus.png)
    
  
  
## <a name="next-steps"></a>Nächste Schritte  
Nun wissen Sie, wie Sie ihren Verleger und Ihren Abonnenten erfolgreich für Ihre Mergereplikation konfigurieren können. Sie können außerdem:

1. Daten in der Tabelle **SalesOrderHeader** oder **SalesOrderDetail** auf dem Verleger oder Abonnenten einfügen, aktualisieren oder löschen
2. Dieses Verfahren wiederholen, wenn Netzwerkkonnektivität zum Synchronisieren von Daten zwischen Verleger und Abonnent verfügbar ist
3. Die Tabelle **SalesOrderHeader** oder **SalesOrderDetail** auf dem anderen Server abfragen, um die replizierten Änderungen anzuzeigen  
  
Weitere Informationen finden Sie unter   
- [Initialisieren eines Abonnements mit einer Momentaufnahme](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
- [Synchronisieren von Daten](../../relational-databases/replication/synchronize-data.md)  
- [Synchronisieren eines Pullabonnements](../../relational-databases/replication/synchronize-a-pull-subscription.md)  
  
  
  
