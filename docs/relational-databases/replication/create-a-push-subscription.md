---
title: Erstellen eines Pushabonnements | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie ein Pushabonnement in SQL Server mithilfe von SQL Server Management Studio, Transact-SQL oder Replikationsverwaltungsobjekten erstellen.
ms.custom: ''
ms.date: 08/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- push subscriptions [SQL Server replication], creating
- merge replication subscribing [SQL Server replication], push subscriptions
- subscriptions [SQL Server replication], push
- snapshot replication [SQL Server], subscribing
- transactional replication, subscribing
ms.assetid: adfbbc61-58d1-4330-9ad6-b14ab1142e2b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 4710900a9a33de2be669ddf1204080068aad0220
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91869251"
---
# <a name="create-a-push-subscription"></a>Erstellen eines Pushabonnements
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  In diesem Thema wird beschrieben, wie ein Pushabonnement in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]oder Replikationsverwaltungsobjekten (RMO) erstellt wird. Informationen zum Erstellen eines Pushabonnements für einen Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abonnenten finden Sie unter [Erstellen eines Abonnements für einen Nicht-SQL Server-Abonnenten](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md).  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
  
 
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
Erstellen Sie mit dem Assistenten für neue Abonnements ein Pushabonnement auf dem Verleger oder Abonnenten. Folgen Sie den Seiten im Assistenten für folgende Aufgaben:  
  
- Angeben des Verlegers und der Veröffentlichung.  
  
- Auswählen, wo die Replikations-Agents ausgeführt werden. Wählen Sie für ein Pushabonnement je nach Veröffentlichungstyp auf der Seite **Speicherort des Verteilungs-Agents** bzw. **Speicherort des Merge-Agents** die Option **Alle Agents auf dem Verteiler ausführen (Pushabonnements)** aus.  
  
- Angeben der Abonnenten und Abonnentendatenbanken.  
  
- Angeben der Anmeldenamen und Kennwörter für Verbindungen zwischen den Replikations-Agents:  
  
  - Bei Abonnements für Momentaufnahme- und Transaktionsveröffentlichungen geben Sie die Anmeldeinformationen auf der Seite **Sicherheit für den Verteilungs-Agent** an.  
  
  - Bei Abonnements für Mergeveröffentlichungen geben Sie die Anmeldeinformationen auf der Seite **Sicherheit für den Merge-Agent** an.  
  
    Informationen zu den erforderlichen Berechtigungen für die jeweiligen Agents finden Sie unter [Sicherheitsmodell des Replikations-Agents](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
- Angeben eines Synchronisierungszeitplans und wann der Abonnent initialisiert werden soll.  
  
- Angeben weiterer Optionen für Mergeveröffentlichungen. Diese Optionen umfassen den Abonnementtyp und Werte für parametrisierte Filter.  
  
- Angeben von zusätzlichen Optionen für Transaktionsveröffentlichungen, die Updates von Abonnements zulassen. Eine Option besteht darin, zu entscheiden, ob Abonnenten bei Änderungen auf dem Verleger sofort einen Commit ausführen oder diese in eine Warteschlange schreiben sollen. Eine weitere Option besteht darin, Anmeldeinformationen einzurichten, mit denen eine Verbindung zwischen dem Abonnenten und dem Verleger hergestellt wird.  
  
- Optionales Erstellen eines Skripts für das Abonnement.  
  
#### <a name="to-create-a-push-subscription-from-the-publisher"></a>So erstellen Sie ein Pushabonnement auf dem Verleger  
  
1. Stellen Sie in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Herausgeber her, und erweitern Sie den Serverknoten.  
  
2. Erweitern Sie den Ordner **Replikation** , und erweitern Sie dann den Ordner **Lokale Veröffentlichungen** .  
  
3. Klicken Sie mit der rechten Maustaste auf die Veröffentlichung, für die Sie ein oder mehrere Abonnements erstellen möchten, und klicken Sie dann auf **Neue Abonnements**.  
  
4. Schließen Sie die Seiten im Assistenten für neue Abonnements ab.  
  
#### <a name="to-create-a-push-subscription-from-the-subscriber"></a>So erstellen Sie ein Pushabonnement auf dem Abonnenten  
  
1. Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]eine Verbindung mit dem Abonnenten her, und erweitern Sie dann den Serverknoten.  
  
2. Erweitern Sie den Ordner **Replikation** .  
  
3. Klicken Sie mit der rechten Maustaste zuerst auf den Ordner **Lokale Abonnements** und anschließend auf **Neue Abonnements**.  
  
4. Wählen Sie auf der Seite **Veröffentlichung** des Assistenten für neue Abonnements **\<Find SQL Server Publisher>** oder **\<Find Oracle Publisher>** aus der Dropdownliste **Verleger** aus.  
  
5. Stellen Sie im Dialogfeld **Verbindung mit Server herstellen** eine Verbindung mit dem Verleger her.  
  
6. Wählen Sie auf der Seite **Veröffentlichung** eine Veröffentlichung aus.  
  
7. Schließen Sie die Seiten im Assistenten für neue Abonnements ab.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
Sie können Pushabonnements mithilfe von gespeicherten Replikationsprozeduren programmgesteuert erstellen. Die verwendeten gespeicherten Prozeduren hängen vom Typ der Veröffentlichung ab, zu der das Abonnement gehört.  
  
> [!IMPORTANT]
> Benutzer sollten nach Möglichkeit dazu aufgefordert werden, Anmeldeinformationen zur Laufzeit anzugeben. Wenn Anmeldeinformationen in einer Skriptdatei gespeichert werden müssen, muss die Datei an einem sicheren Ort gespeichert werden, um unberechtigten Zugriff zu vermeiden.  
  
#### <a name="to-create-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>So erstellen Sie ein Pushabonnement für eine Momentaufnahme- oder Transaktionsveröffentlichung.  
  
1. Überprüfen Sie für die Veröffentlichungsdatenbank auf dem Verleger, ob die Veröffentlichung Pushabonnements unterstützt, indem Sie [sp_helppublication](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md) ausführen.  
  
   - Wenn der Wert von **allow_push** **1**ist, werden Pushabonnements unterstützt.  
  
   - Wenn der Wert von **allow_push** **0** ist, führen Sie [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) aus. Legen Sie **allow_push** für **\@property** und **true** für **\@value** fest.  
  
2. Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_addsubscription](../system-stored-procedures/sp-addsubscription-transact-sql.md) aus. Geben Sie **\@publication**, **\@subscriber** und **\@destination_db** an. Geben Sie den Wert **push** für **\@subscription_type** an. Weitere Informationen zum Aktualisieren von Abonnements finden Sie unter [Erstellen eines aktualisierbaren Abonnements für eine Transaktionsveröffentlichung](publish/create-an-updatable-subscription-to-a-transactional-publication.md).  
  
3. Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_addpushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md) aus. Geben Sie Folgendes an:  
  
   - Die Parameter **\@subscriber**, **\@subscriber_db** und **\@publication**.  
  
   - Die [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Anmeldeinformationen, unter denen der Verteilungs-Agent auf dem Verteiler für **\@job_login** und **\@job_password** ausgeführt wird.  
  
     > [!NOTE]
     > Für Verbindungen, die über die integrierte Windows-Authentifizierung hergestellt werden, werden immer die mit **\@job_login** und **\@job_password** angegebenen Windows-Anmeldeinformationen verwendet. Der Verteilungs-Agent stellt die lokale Verbindung mit dem Verteiler immer mithilfe der integrierten Windows-Authentifizierung her. Standardmäßig stellt der Agent mithilfe der integrierten Windows-Authentifizierung eine Verbindung mit dem Abonnenten her.  
  
   - (Optional) Der Wert **0** (null) für **\@subscriber_security_mode** und die [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldeinformationen für **\@subscriber_login** und **\@subscriber_password**. Geben Sie diese Parameter an, falls Sie beim Herstellen einer Verbindung mit dem Abonnenten die SQL Server-Authentifizierung verwenden müssen.  
  
   - Einen Zeitplan für den Verteilungs-Agentauftrag für dieses Abonnement. Weitere Informationen finden Sie unter [Angeben von Synchronisierungszeitplänen](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
> [!IMPORTANT]
> Wenn Sie ein Pushabonnement auf einem Verleger mit einem Remoteverteiler erstellen, werden die angegebenen Werte für alle Parameter, einschließlich *job_login* und *job_password*, als Nur-Text an den Verteiler gesendet. Sie sollten die Verbindung zwischen dem Verleger und dem zugehörigen Remoteverteiler verschlüsseln, bevor Sie diese gespeicherte Prozedur ausführen. Weitere Informationen finden Sie unter [Aktivieren von verschlüsselten Verbindungen zur Datenbank-Engine &#40;SQL Server-Konfigurations-Manager&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-create-a-push-subscription-to-a-merge-publication"></a>So erstellen Sie ein Pushabonnement für eine Mergeveröffentlichung  
  
1. Überprüfen Sie für die Veröffentlichungsdatenbank auf dem Verleger, ob die Veröffentlichung Pushabonnements unterstützt, indem Sie [sp_helpmergepublication](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md) ausführen.  
  
   - Wenn der Wert von **allow_push** **1**ist, werden Pushabonnements von der Veröffentlichung unterstützt.  
  
   - Wenn der Wert von **allow_push** nicht **1** ist, führen Sie [sp_changemergepublication](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) aus. Legen Sie **allow_push** für **\@property** und **true** für **\@value** fest.  
  
2. Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_addmergesubscription](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md) aus. Geben Sie die folgenden Parameter an:  
  
   - **\@publication**. Das ist der Name der Veröffentlichung.  
  
   - **\@subscriber_type**. Geben Sie für ein Clientabonnement **local** an. Geben Sie für ein Serverabonnement **global** an.  
  
   - **\@subscription_priority**. Geben Sie für ein Serverabonnement eine Priorität für das Abonnement (**0.00** bis **99.99**) an.  
  
   Weitere Informationen finden Sie unter [Erweiterte Konflikterkennung und -lösung bei der Mergereplikation](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md).  
  
3. Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md) aus. Geben Sie Folgendes an:  
  
   - Die Parameter **\@subscriber**, **\@subscriber_db** und **\@publication**.  
  
   - Die Windows-Anmeldeinformationen, unter denen der Merge-Agent auf dem Verteiler für **\@job_login** und **\@job_password** ausgeführt wird.  
  
     > [!NOTE]
     > Für Verbindungen, die über die integrierte Windows-Authentifizierung hergestellt werden, werden immer die mit **\@job_login** und **\@job_password** angegebenen Windows-Anmeldeinformationen verwendet. Der Merge-Agent stellt die lokale Verbindung mit dem Verteiler immer mithilfe der integrierten Windows-Authentifizierung her. Standardmäßig stellt der Agent mithilfe der integrierten Windows-Authentifizierung eine Verbindung mit dem Abonnenten her.  
  
   - (Optional) Den Wert **0** für **\@subscriber_security_mode** und die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldeinformationen für **\@subscriber_login** und **\@subscriber_password**. Geben Sie diese Parameter an, falls Sie beim Herstellen einer Verbindung mit dem Abonnenten die SQL Server-Authentifizierung verwenden müssen.  
  
   - (Optional) Den Wert **0** für **\@publisher_security_mode** und die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldeinformationen für **\@publisher_login** und **\@publisher_password**. Geben Sie diese Werte an, falls Sie beim Herstellen einer Verbindung mit dem Verleger die SQL Server-Authentifizierung verwenden müssen.  
  
   - Einen Zeitplan für den Merge-Agentauftrag für dieses Abonnement. Weitere Informationen finden Sie unter [Angeben von Synchronisierungszeitplänen](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
> [!IMPORTANT]
> Wenn Sie ein Pushabonnement auf einem Verleger mit einem Remoteverteiler erstellen, werden die angegebenen Werte für alle Parameter, einschließlich *job_login* und *job_password*, als Nur-Text an den Verteiler gesendet. Sie sollten die Verbindung zwischen dem Verleger und dem zugehörigen Remoteverteiler verschlüsseln, bevor Sie diese gespeicherte Prozedur ausführen. Weitere Informationen finden Sie unter [Aktivieren von verschlüsselten Verbindungen zur Datenbank-Engine &#40;SQL Server-Konfigurations-Manager&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Beispiele (Transact-SQL)  
 Im folgenden Beispiel wird ein Pushabonnement für eine Transaktionsveröffentlichung erstellt. Die Werte für den Anmeldenamen und das Kennwort werden zur Laufzeit mithilfe von **sqlcmd**-Skriptvariablen bereitgestellt.  
  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/create-a-push-subscription_1.sql)]  
  
 Im folgenden Beispiel wird ein Pushabonnement für eine Mergeveröffentlichung erstellt. Die Werte für den Anmeldenamen und das Kennwort werden zur Laufzeit mithilfe von **sqlcmd**-Skriptvariablen bereitgestellt.  
  
 [!code-sql[HowTo#sp_addmergepushsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/create-a-push-subscription_2.sql)]  
  
##  <a name="using-replication-management-objects"></a><a name="RMOProcedure"></a> Verwenden von Replikationsverwaltungsobjekten  
 Sie können Pushabonnements mithilfe von Replikationsverwaltungsobjekten (RMO) programmgesteuert erstellen. Die RMO-Klassen, die Sie zum Erstellen eines Pushabonnements verwenden, hängen vom Typ der Veröffentlichung ab, für die das Abonnement erstellt wird.  
  
> [!IMPORTANT]
> Benutzer sollten nach Möglichkeit dazu aufgefordert werden, Anmeldeinformationen zur Laufzeit anzugeben. Wenn Sie Anmeldeinformationen speichern müssen, verwenden Sie die [Kryptografiedienste](/previous-versions/aa719848(v=vs.71)) von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework.  
  
#### <a name="to-create-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>So erstellen Sie ein Pushabonnement für eine Momentaufnahme- oder Transaktionsveröffentlichung.  
  
1. Erstellen Sie eine Verbindung mit dem Verleger, indem Sie die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Klasse verwenden.  
  
2. Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.TransPublication> -Klasse, indem Sie die Verlegerverbindung aus Schritt 1 verwenden. Geben Sie <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>und <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>an.  
  
3. Rufen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> -Methode auf. Wenn diese Methode **false**zurückgibt, sind entweder die in Schritt 2 angegebenen Eigenschaften falsch definiert, oder die Veröffentlichung ist auf dem Server nicht vorhanden.  
  
4. Führen Sie ein bitweises logisches AND ( **&** in Visual C# und **And** in Visual Basic) zwischen der <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> -Eigenschaft und <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Falls das Ergebnis <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>lautet, legen Sie für <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> das Ergebnis eines bitweisen logischen OR ( **|** in Visual C# und **Or** in Visual Basic) zwischen <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> und <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Rufen Sie dann <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> auf, um Pushabonnements zu aktivieren.  
  
5. Falls die Abonnementdatenbank nicht vorhanden ist, erstellen Sie sie mithilfe der <xref:Microsoft.SqlServer.Management.Smo.Database> -Klasse. Weitere Informationen finden Sie unter [Erstellen, Ändern und Löschen von Datenbanken](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md).  
  
6. Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.TransSubscription>-Klasse.  
  
7. Legen Sie folgende Eigenschaften für das Abonnement fest:  
  
   - <xref:Microsoft.SqlServer.Management.Common.ServerConnection> auf den in Schritt 1 erstellten Verleger für <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
   - Name der Abonnementdatenbank für <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>.  
  
   - Name des Abonnenten für <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>. 
  
   - Name der Veröffentlichungsdatenbank für <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>.  
  
   - Name der Veröffentlichung für <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>.
  
   - Die Parameter <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> und <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> von <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> , um die Anmeldeinformationen für das [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Konto bereitzustellen, unter dem der Verteilungs-Agent auf dem Verteiler ausgeführt wird. Mit diesem Konto werden lokale Verbindungen mit dem Verteiler sowie Remoteverbindungen mithilfe der Windows-Authentifizierung hergestellt.  
  
     > [!NOTE]
     > <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> muss nicht festgelegt werden, wenn das Abonnement von einem Mitglied der festen Serverrolle **sysadmin** erstellt wurde. Die Verwendung dieser Eigenschaft wird jedoch empfohlen. In diesem Fall nimmt der Agent die Identität des SQL Server-Agent-Kontos an. Weitere Informationen finden Sie unter [Sicherheitsmodell des Replikations-Agents](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
   - (Optional) Den Wert **true** (Standard) für <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A> , um einen Agentauftrag zu erstellen, mit dem das Abonnement synchronisiert wird. Wenn Sie **false**angeben, kann das Abonnement nur programmgesteuert synchronisiert werden.  
  
   - (Optional) Legen Sie die Felder <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> und <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> oder <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> von <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> fest, wenn Sie die SQL Server-Authentifizierung zum Herstellen einer Verbindung mit dem Abonnenten verwenden.  
  
8. Rufen Sie die <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> -Methode auf.  
  
> [!IMPORTANT]
> Wenn Sie ein Pushabonnement auf einem Verleger mit einem Remoteverteiler erstellen, werden die angegebenen Werte für alle Eigenschaften (einschließlich <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>) als Nur-Text an den Verteiler gesendet. Sie sollten die Verbindung zwischen dem Verleger und dem zugehörigen Remoteverteiler verschlüsseln, bevor Sie die <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A>-Methode aufrufen. Weitere Informationen finden Sie unter [Aktivieren von verschlüsselten Verbindungen zur Datenbank-Engine &#40;SQL Server-Konfigurations-Manager&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-create-a-push-subscription-to-a-merge-publication"></a>So erstellen Sie ein Pushabonnement für eine Mergeveröffentlichung  
  
1. Erstellen Sie eine Verbindung mit dem Verleger, indem Sie die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Klasse verwenden.  
  
2. Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.MergePublication> -Klasse, indem Sie die Verlegerverbindung aus Schritt 1 verwenden. Geben Sie <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>und <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>an.  
  
3. Rufen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> -Methode auf. Wenn diese Methode **false**zurückgibt, sind entweder die in Schritt 2 angegebenen Eigenschaften falsch definiert, oder die Veröffentlichung ist auf dem Server nicht vorhanden.  
  
4. Führen Sie ein bitweises logisches AND ( **&** in Visual C# und **And** in Visual Basic) zwischen der <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> -Eigenschaft und <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Falls das Ergebnis <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>lautet, legen Sie für <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> das Ergebnis eines bitweisen logischen OR ( **|** in Visual C# und **Or** in Visual Basic) zwischen <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> und <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Rufen Sie dann <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> auf, um Pushabonnements zu aktivieren.  
  
5. Falls die Abonnementdatenbank nicht vorhanden ist, erstellen Sie sie mithilfe der <xref:Microsoft.SqlServer.Management.Smo.Database> -Klasse. Weitere Informationen finden Sie unter [Erstellen, Ändern und Löschen von Datenbanken](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md).  
  
6. Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.MergeSubscription>-Klasse.  
  
7. Legen Sie folgende Eigenschaften für das Abonnement fest:  
  
   - <xref:Microsoft.SqlServer.Management.Common.ServerConnection> auf den in Schritt 1 erstellten Verleger für <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
   - Name der Abonnementdatenbank für <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>.  
  
   - Name des Abonnenten für <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>.    
   - Name der Veröffentlichungsdatenbank für <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>.  
  
   - Name der Veröffentlichung für <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>.    
   - Die Parameter <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> und <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> von <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> , um die Anmeldeinformationen für das [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Konto bereitzustellen, unter dem der Merge-Agent auf dem Verteiler ausgeführt wird. Mit diesem Konto werden lokale Verbindungen mit dem Verteiler sowie Remoteverbindungen über die Windows-Authentifizierung hergestellt.  
  
     > [!NOTE]
     > <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> muss nicht festgelegt werden, wenn das Abonnement von einem Mitglied der festen Serverrolle **sysadmin** erstellt wurde. Die Verwendung dieser Eigenschaft wird jedoch empfohlen. In diesem Fall nimmt der Agent die Identität des SQL Server-Agent-Kontos an. Weitere Informationen finden Sie unter [Sicherheitsmodell des Replikations-Agents](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
   - (Optional) Den Wert **true** (Standard) für <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A> , um einen Agentauftrag zu erstellen, mit dem das Abonnement synchronisiert wird. Wenn Sie **false**angeben, kann das Abonnement nur programmgesteuert synchronisiert werden.  
  
   - (Optional) Legen Sie die Felder <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> und <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> oder <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> von <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> fest, wenn Sie die SQL Server-Authentifizierung zum Herstellen einer Verbindung mit dem Abonnenten verwenden.  
  
   - (Optional) Legen Sie die Felder <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> und <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> oder <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> von <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherSecurity%2A> fest, wenn Sie die SQL Server-Authentifizierung zum Herstellen einer Verbindung mit dem Verleger verwenden.  
  
8. Rufen Sie die <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> -Methode auf.  
  
> [!IMPORTANT]  
> Wenn Sie ein Pushabonnement auf einem Verleger mit einem Remoteverteiler erstellen, werden die angegebenen Werte für alle Eigenschaften (einschließlich <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>) als Nur-Text an den Verteiler gesendet. Sie sollten die Verbindung zwischen dem Verleger und dem zugehörigen Remoteverteiler verschlüsseln, bevor Sie die <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A>-Methode aufrufen. Weitere Informationen finden Sie unter [Aktivieren von verschlüsselten Verbindungen zur Datenbank-Engine &#40;SQL Server-Konfigurations-Manager&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Beispiele (RMO)  
 Im folgenden Beispiel wird ein neues Pushabonnement für eine Transaktionsveröffentlichung erstellt. Die Anmeldeinformationen für das Windows-Konto, mit denen Sie den Verteilungs-Agent-Auftrag ausführen, werden zur Laufzeit übergeben.  
  
 [!code-cs[HowTo#rmo_CreateTranPushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createtranpushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateTranPushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createtranpushsub)]  
  
 Im folgenden Beispiel wird ein neues Pushabonnement für eine Mergeveröffentlichung erstellt. Die Anmeldeinformationen für das Windows-Konto, mit denen Sie den Merge-Agent-Auftrag ausführen, werden zur Laufzeit übergeben.  
  
 [!code-cs[HowTo#rmo_CreateMergePushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepushsub)]  
  
## <a name="see-also"></a>Weitere Informationen  
 [Anzeigen und Ändern der Eigenschaften von Pushabonnements](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [Bewährte Methoden für die Replikationssicherheit](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Erstellen einer Veröffentlichung](../../relational-databases/replication/publish/create-a-publication.md)   
 [Grundlegendes zu Replikationsverwaltungsobjekten](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Synchronisieren eines Pushabonnements](../../relational-databases/replication/synchronize-a-push-subscription.md)   
 [Abonnieren von Veröffentlichungen](../../relational-databases/replication/subscribe-to-publications.md)   
 [Verwenden von sqlcmd mit Skriptvariablen](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)  
  
