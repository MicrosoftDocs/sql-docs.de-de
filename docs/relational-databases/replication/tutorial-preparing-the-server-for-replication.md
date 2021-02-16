---
title: 'Tutorial: Vorbereiten der Replikation'
description: In diesem Tutorial erfahren Sie, wie Sie Ihren Verleger, Verteiler und Abonnenten für die Replikation vorbereiten, indem Sie Windows-Konten erstellen, den Momentaufnahmeordner vorbereiten und die Verteilung konfigurieren.
ms.custom: seo-lt-2019
ms.date: 04/02/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: quickstart
helpviewer_keywords:
- replication [SQL Server], tutorials
ms.assetid: ce30a095-2975-4387-9377-94a461ac78ee
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7506b3b57d81187269c26fae3cafc2d3cdbc6628
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348624"
---
# <a name="tutorial-prepare-sql-server-for-replication-publisher-distributor-subscriber"></a>Tutorial: Vorbereiten von SQL Server auf die Replikation (Verleger, Verteiler und Abonnent)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Es ist wichtig, einen Sicherheitsplan zu erstellen, bevor Sie die Replikationstopologie konfigurieren. In diesem Tutorial erfahren Sie, wie Sie eine Replikationstopologie besser absichern. Außerdem lernen Sie, wie Sie die Verteilung konfigurieren, die der erste Schritt beim Replizieren von Daten ist. Es ist erforderlich, dieses Lernprogramm vor allen anderen Lernprogrammen abzuschließen.  
  
> [!NOTE]  
> Für das sichere Replizieren von Daten zwischen Servern empfiehlt es sich, alle Empfehlungen unter [Bewährte Methoden für die Replikationssicherheit](../../relational-databases/replication/security/replication-security-best-practices.md) zu implementieren.  
  
## <a name="what-you-will-learn"></a>Lernziele  
In diesem Tutorial erfahren Sie, wie Sie einen Server vorbereiten, sodass die Replikation sicher und mit den niedrigsten Berechtigungen ausgeführt werden kann.  

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen von Windows-Konten für die Replikation
> * Vorbereiten des Momentaufnahmeordners
> * Konfigurieren der Verteilung

## <a name="prerequisites"></a>Voraussetzungen
Dieses Tutorial richtet sich an Benutzer, die zwar mit grundlegenden Datenbankvorgängen vertraut sind, aber nur über begrenzte Erfahrungen in Bezug auf die Replikation verfügen. 

Für dieses Tutorial benötigen Sie SQL Server, SQL Server Management Studio (SSMS) und eine AdventureWorks-Datenbank:  
  
- Installieren Sie auf dem Verlegerserver (Quelle) Folgendes:  
  
   - Eine beliebige [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Edition mit Ausnahme von SQL Server Express und SQL Server Compact. Diese Editionen können nicht als Verleger für Replikationen verwendet werden.   
   - Die [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] -Beispieldatenbank Aus Sicherheitsgründen werden die Beispieldatenbanken standardmäßig nicht installiert.  
  
- Installieren Sie auf dem Abonnentenserver (Ziel) eine beliebige Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit Ausnahme von [!INCLUDE[ssEW](../../includes/ssew-md.md)]. [!INCLUDE[ssEW](../../includes/ssew-md.md)] kann nicht als Abonnent einer Transaktionsreplikation verwendet werden.  
  
- Installieren Sie [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).
- Installieren Sie die [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Laden Sie die [AdventureWorks-Beispieldatenbank](https://github.com/Microsoft/sql-server-samples/releases) herunter. Weitere Informationen zum Wiederherstellen einer Datenbank in SSMS finden Sie unter [Wiederherstellen einer Datenbank](../backup-restore/restore-a-database-backup-using-ssms.md). 
    
>[!NOTE]
> - Die Replikation wird für SQL Server-Instanzen, zwischen denen mehr als zwei Versionen liegen, nicht unterstützt. Weitere Informationen finden Sie unter [In der Replikationstopologie unterstützte SQL Server-Versionen](replication-backward-compatibility.md).
> - Sie müssen in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger und dem Abonnenten herstellen. Dazu verwenden Sie einen Anmeldenamen eines Mitglieds der festen Serverrolle **sysadmin** ist. Weitere Informationen zu dieser Rolle finden Sie unter [Rollen auf Serverebene](../security/authentication-access/server-level-roles.md).  


**Ungefähre Dauer dieses Tutorials: 30 Minuten**
  
## <a name="create-windows-accounts-for-replication"></a>Erstellen von Windows-Konten für die Replikation
In diesem Abschnitt erstellen Sie Windows-Konten zum Ausführen von Replikations-Agents. Sie erstellen für die folgenden Agents ein separates Windows-Konto auf dem lokalen Server:  
  
|Agent|Position|Kontoname|  
|---------|------------|----------------|  
|Momentaufnahme-Agent|Herausgeber|<*machine_name*>\repl_snapshot|  
|Protokolllese-Agent|Herausgeber|<*machine_name*>\repl_logreader|  
|Verteilungs-Agent|Verleger und Abonnent|<*machine_name*>\repl_distribution|  
|Merge-Agent|Verleger und Abonnent|<*machine_name*>\repl_merge|  
  
> [!NOTE]  
> In den Replikationstutorials wird für Verleger und Verteiler dieselbe Instanz (NODE1\SQL2016) von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gemeinsam verwendet. Die Abonnenteninstanz (NODE2\SQL2016) ist eine Remoteinstanz. Verleger und Abonnent können dieselbe Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gemeinsam verwenden. Dies ist jedoch nicht obligatorisch. Wenn der Verleger und Abonnent dieselbe Instanz verwenden, sind die Schritte zum Erstellen von Konten auf dem Verleger nicht erforderlich.  

### <a name="create-local-windows-accounts-for-replication-agents-at-the-publisher"></a>Erstellen lokaler Windows-Konten für Replikations-Agents auf dem Verleger
  
1. Öffnen Sie auf dem Verleger in der Systemsteuerung unter **Verwaltung** die Anwendung **Computerverwaltung**.  
  
2. Erweitern Sie unter **System** den Eintrag **Lokale Benutzer und Gruppen**.  
  
3. Klicken Sie mit der rechten Maustaste auf **Benutzer**, und klicken Sie dann auf **Neuer Benutzer**.  
     
4. Geben Sie **repl_snapshot** im Feld **Benutzername** ein, geben Sie das Kennwort und weitere relevante Informationen an, und klicken Sie anschließend auf **Erstellen**, um das Konto „repl_snapshot“ zu erstellen: 

   ![Dialogfeld „Neuer Benutzer“](media/tutorial-preparing-the-server-for-replication/newuser.png)
  
5. Wiederholen Sie den letzten Schritt, um die Konten „repl_logreader“, „repl_distribution“ und „repl_merge“ zu erstellen:  
 
   ![Liste der Replikationsbenutzer](media/tutorial-preparing-the-server-for-replication/replusers.png)
  
6. Klicken Sie auf **Schließen**.  
  
### <a name="create-local-windows-accounts-for-replication-agents-at-the-subscriber"></a>Erstellen lokaler Windows-Konten für Replikations-Agents auf dem Abonnenten  
  
1. Öffnen Sie auf dem Abonnenten in der Systemsteuerung unter **Verwaltung** die Anwendung **Computerverwaltung**.  
  
2. Erweitern Sie unter **System** den Eintrag **Lokale Benutzer und Gruppen**.  
  
3. Klicken Sie mit der rechten Maustaste auf **Benutzer**, und klicken Sie dann auf **Neuer Benutzer**.  
  
4. Geben Sie **repl_distribution** im Feld **Benutzername** ein, geben Sie das Kennwort und weitere relevante Informationen an, und klicken Sie anschließend auf **Erstellen**, um das Konto „repl_distribution“ zu erstellen.  
  
5. Wiederholen Sie den letzten Schritt, um das Konto repl_merge zu erstellen.  
  
6. Klicken Sie auf **Schließen**.  

Weitere Informationen finden Sie unter [Replikations-Agents (Übersicht)](../../relational-databases/replication/agents/replication-agents-overview.md).  

## <a name="prepare-the-snapshot-folder"></a>Vorbereiten des Momentaufnahmeordners
In diesem Abschnitt konfigurieren Sie den Momentaufnahmeordner, in dem die Veröffentlichungsmomentaufnahme erstellt und gespeichert wird. 

### <a name="create-a-share-for-the-snapshot-folder-and-assign-permissions"></a>Erstellen einer Freigabe für den Momentaufnahmeordner und Zuweisen von Berechtigungen  
  
1. Navigieren Sie im Datei-Explorer zum [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenordner. Der Standardspeicherort lautet C:\Programme\Microsoft SQL Server\MSSQL.X\MSSQL\Data.  
  
2. Erstellen Sie einen neuen Ordner mit dem Namen **repldata**.  
  
3. Klicken Sie mit der rechten Maustaste auf diesen Ordner, und wählen Sie **Eigenschaften** aus.  
  
   a. Klicken Sie auf der Registerkarte **Freigabe** im Dialogfeld **repldata Properties** (Eigenschaften von repldata) auf **Erweiterte Freigabe**.  
  
   b. Klicken Sie im Dialogfeld **Erweiterte Freigabe** auf **Diesen Ordner freigeben**, und klicken Sie dann auf **Berechtigungen**.  

   ![Optionsauswahl zur Freigabe des Ordners „repldata“](media/tutorial-preparing-the-server-for-replication/repldata.png)

6. Klicken Sie im Dialogfeld **Berechtigungen für repldata** auf **Hinzufügen**. Geben Sie im Dialogfeld **Select Users, Computers, Service Accounts, or Groups** (Benutzer, Computer, Dienstkonten oder Gruppen auswählen) den Namen des zuvor erstellten Momentaufnahmen-Agent-Kontos ein: <*Name_des_Verlegercomputers*> **\repl_snapshot**. Klicken Sie auf **Namen überprüfen** und dann auf **OK**.  

   ![Optionsauswahl zum Hinzufügen von Freigabeberechtigungen](media/tutorial-preparing-the-server-for-replication/addshareperms.png)

7. Wiederholen Sie Schritt 6, um die anderen beiden Konten hinzuzufügen, die Sie zuvor erstellt haben: <*Name_des_Verlegercomputers*> **\repl_merge** und <*Name_des_Verlegercomputers*> **\repl_distribution**.

8. Weisen Sie nach dem Hinzufügen der drei Konten die folgenden Berechtigungen zu:      
   - repl_distribution: **Lesen**  
   - repl_merge: **Lesen**  
   - repl_snapshot: **Vollzugriff**    

   ![Freigabeberechtigungen für die jeweiligen Konten](media/tutorial-preparing-the-server-for-replication/sharedpermissions.png)

9. Klicken Sie nach der ordnungsgemäßen Konfiguration der Freigabeberechtigungen auf **OK**, um das Dialogfeld **Permissions for repldata** (Berechtigungen für „repldata“) zu schließen. Klicken Sie auf **OK**, um das Dialogfeld **Erweiterte Freigabe** zu schließen. 

10. Klicken Sie im Dialogfeld **repldata Properties** (Eigenschaften von „repldata“) zuerst auf die Registerkarte **Sicherheit** und anschließend auf **Bearbeiten**:  

    ![Schaltfläche „Bearbeiten“ auf der Registerkarte „Sicherheit“](media/tutorial-preparing-the-server-for-replication/editsecurity.png)   

11. Klicken Sie im Dialogfeld **Berechtigungen für repldata** auf **Hinzufügen**. Geben Sie im Dialogfeld **Select Users, Computers, Service Accounts, or Groups** (Benutzer, Computer, Dienstkonten oder Gruppen auswählen) den Namen des zuvor erstellten Momentaufnahmen-Agent-Kontos ein: <*Name_des_Verlegercomputers*> **\repl_snapshot**. Klicken Sie auf **Namen überprüfen** und dann auf **OK**.  

    ![Optionsauswahl zum Hinzufügen von Sicherheitsberechtigungen](media/tutorial-preparing-the-server-for-replication/addsecuritypermissions.png)

  
12. Wiederholen Sie den vorherigen Schritt, um Berechtigungen für den Verteilungs-Agent <*Name_des_Verlegercomputers*> **\repl_distribution** und für den Merge-Agent <*Name_des_Verlegercomputers*> **\repl_merge** hinzuzufügen.  
    
  
13. Überprüfen Sie, ob die folgenden Berechtigungen gewährt wurden:  
  
    - repl_distribution: **Lesen**
    - repl_merge: **Lesen**
    - repl_snapshot: **Vollzugriff**   
 
    ![Benutzerberechtigungen für Replikationsdaten](media/tutorial-preparing-the-server-for-replication/replpermissions.png) 

14. Klicken Sie erneut auf die Registerkarte **Freigabe**, und notieren Sie sich den **Netzwerkpfad** für die Freigabe. Sie benötigen diesen Pfad später, wenn Sie den Momentaufnahmeordner konfigurieren.  

    ![Netzwerkpfad auf der Registerkarte „Freigabe“](media/tutorial-replicating-data-between-continuously-connected-servers/networkpath.png)

15. Klicken Sie auf **OK**, um das Dialogfeld **repldata Properties** zu schließen. 
 
Weitere Informationen finden Sie unter [Secure the snapshot folder](../../relational-databases/replication/security/secure-the-snapshot-folder.md) (Schützen des Momentaufnahmeordners).  
  

## <a name="configure-distribution"></a>Konfigurieren der Verteilung
In diesem Abschnitt konfigurieren Sie die Verteilung auf dem Verleger und legen die erforderlichen Berechtigungen für die Verteilungs- und Veröffentlichungsdatenbank fest. Wenn Sie den Verteiler bereits konfiguriert haben, müssen Sie die Veröffentlichung und die Verteilung deaktivieren, bevor Sie mit dem Lesen des folgenden Abschnitts beginnen. Führen Sie diesen Schritt nicht aus, falls Sie eine vorhandene Replikationstopologie beibehalten müssen. Dies gilt insbesondere für die Produktionsumgebung.   
  
Das Konfigurieren eines Verlegers mit einem Remoteverteiler wird in diesem Tutorial nicht behandelt.  

### <a name="configure-distribution-at-the-publisher"></a>Konfigurieren der Verteilung auf dem Verleger  
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten.  
  
2. Klicken Sie mit der rechten Maustaste auf den Ordner **Replikation**, und klicken Sie anschließend auf **Verteilung konfigurieren**:  

   ![Menüelement „Verteilung konfigurieren“ im Kontextmenü](media/tutorial-preparing-the-server-for-replication/configuredistribution.png)
  
   > [!NOTE]  
   > - Wenn Sie **localhost** anstelle des tatsächlichen Servernamens verwendet haben, um eine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herzustellen, werden Sie in einer Warnmeldung darauf hingewiesen, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht in der Lage ist, eine Verbindung mit **localhost oder der IP-Adresse** herzustellen. Klicken Sie im Warnungsdialogfeld auf **OK**. Ändern Sie im Dialogfeld **Verbindung mit Server herstellen** die Angabe für **Servername** von **localhost oder der IP-Adresse** in den Namen des Servers. Wählen Sie dann **Verbinden** aus.  
   > - Zurzeit liegt ein bekanntes Problem mit SQL Server Management Studio 18.0 und höher (SSMS) vor, bei dem eine Warnmeldung _nicht_ angezeigt wird, wenn eine Verbindung zum Verteiler über die IP-Adresse hergestellt wird, diese jedoch weiterhin ungültig ist. Der tatsächliche Servername sollte beim Herstellen einer Verbindung zum Verteiler verwendet werden. 
   
   Der Verteilungskonfigurations-Assistent wird gestartet.  
  
3. Aktivieren Sie auf der Seite **Verteiler** das Optionsfeld neben < *'Servername'* >  **als seinen eigenen Verteiler verwenden. SQL Server erstellt eine Verteilungsdatenbank und ein Protokoll**. Wählen Sie **Weiter** aus.  

   ![Option, mit der festgelegt wird, dass der Server selbst als Verteiler agiert](media/tutorial-preparing-the-server-for-replication/serverdistributor.png)
  
4. Wenn der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent nicht ausgeführt wird, klicken Sie auf der Seite **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Start** auf **Ja, den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent zum automatischen Starten konfigurieren**. Wählen Sie **Weiter** aus.  

     
5. Geben Sie den Pfad \\\\<*Name_des_Verlegercomputers*> **\repldata** im Textfeld **Momentaufnahmeordner** ein, und klicken Sie anschließend auf **Weiter**. Dieser Pfad sollte mit dem übereinstimmen, der zuvor im Eigenschaftenordner von „repldata“ unter **Netzwerkpfad** nach dem Konfigurieren der Freigabeeigenschaften angezeigt wurde. 

   ![Vergleich der Netzwerkpfade im Dialogfeld „Eigenschaften von ‚repldata‘“ und im Verteilungskonfigurations-Assistenten](media/tutorial-preparing-the-server-for-replication/repldatasnapshot.png)
  
6. Übernehmen Sie die Standardwerte auf den restlichen Seiten des Assistenten.  
    
   ![Letzte Seite des Assistenten](media/tutorial-preparing-the-server-for-replication/distributionwizarddefaults.png)
  
7. Klicken Sie auf **Fertig stellen**, um die Verteilung zu aktivieren. 

Beim Konfigurieren des Verteilers kann die unten abgebildete Fehlermeldung angezeigt werden. Sie weist darauf hin, dass das Konto, mit dem das SQL Server-Agent-Konto gestartet wurde, kein Administratorkonto im System ist. Sie müssen den SQL Server-Agent daher entweder manuell starten und dem bestehenden Konto diese Berechtigungen gewähren oder das Konto ändern, das der SQL Server-Agent nutzt. 

![Fehlermeldung beim Konfigurieren des SQL Server-Agents](media/tutorial-preparing-the-server-for-replication/startingagenterror.png)

Wenn Ihre SSMS-Instanz mit Administratorrechten ausgeführt wird, können Sie den SQL-Agent manuell innerhalb von SSMS starten:  
    
![Klicken auf „Starten“ im Kontextmenü für den Agent in SSMS](media/tutorial-preparing-the-server-for-replication/ssmsstartagent.png) 

>[!NOTE]
> Wenn der SQL-Agent nicht sichtbar gestartet wird, klicken Sie in SSMS mit der rechten Maustaste auf den SQL Server-Agent und anschließend auf **Aktualisieren**. Wenn er weiterhin nicht gestartet wird, müssen Sie ihn manuell über den SQL Server-Konfigurations-Manager starten.    
  
## <a name="set-database-permissions"></a>Festlegen von Datenbankberechtigungen  
  
1. Erweitern Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] den Knoten **Sicherheit**, klicken Sie mit der rechten Maustaste auf **Anmeldungen**, und wählen Sie anschließend **Neue Anmeldung** aus:  

   ![Menüelement „Neue Anmeldung“ im Kontextmenü](media/tutorial-preparing-the-server-for-replication/newlogin.png)
  
2. Klicken Sie auf der Seite **Allgemein** auf **Suchen**. Geben Sie in das Feld **Namen des auszuwählenden Objekts eingeben** die Zeichenfolge <*Name_des_Verlegercomputers*> **\repl_snapshot** ein, und klicken Sie zuerst auf **Namen überprüfen** und anschließend auf **OK**.  

   ![Optionsauswahl zur Eingabe des Objektnamens](media/tutorial-preparing-the-server-for-replication/addsnapshotlogin.png)
  
3. Wählen Sie auf der Seite **Benutzerzuordnung** in der Liste **Benutzer, die dieser Anmeldung zugeordnet sind** die Datenbanken **distribution** und [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] aus.  
  
   Wählen Sie in der Liste „Mitgliedschaft in Datenbankrolle für“ die Rolle **db_owner** für die Anmeldung bei beiden Datenbanken aus.  

   ![Auswählen von Datenbanken und der zugehörigen Rolle](media/tutorial-preparing-the-server-for-replication/dbowner.png)
  
4. Klicken Sie auf **OK**, um die Anmeldung zu erstellen.  
  
5. Wiederholen Sie die Schritte 1 bis 4, um eine Anmeldung für die anderen lokalen Konten (repl_distribution, repl_logreader und repl_merge) zu erstellen. Diese Anmeldungen müssen auch den Benutzern zugeordnet werden, die Mitglieder der festen Datenbankrolle **db_owner** in den Datenbanken **distribution** und **AdventureWorks** sind.  

   ![Ansicht mit allen vier Konten im Objekt-Explorer](media/tutorial-preparing-the-server-for-replication/usersinssms.png)
   
  
Weitere Informationen finden Sie unter
- [Konfigurieren der Verteilung](../../relational-databases/replication/configure-distribution.md) 
- [Sicherheitsmodell des Replikations-Agents](../../relational-databases/replication/security/replication-agent-security-model.md)  

## <a name="next-steps"></a>Nächste Schritte
Sie haben Ihren Server jetzt erfolgreich für die Replikation vorbereitet. Im nächsten Artikel erfahren Sie, wie Sie die Transaktionsreplikation konfigurieren: 

> [!div class="nextstepaction"]
> [Tutorial: Konfigurieren der Replikation zwischen zwei Servern mit kontinuierlicher Verbindung (Transaktionsreplikation)](tutorial-replicating-data-between-continuously-connected-servers.md)

  
