---
title: Erstellen eines Pullabonnements | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie ein Pullabonnement in SQL Server mithilfe von SQL Server Management Studio, Transact-SQL oder Replikationsverwaltungsobjekten erstellen.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- pull subscriptions [SQL Server replication], creating
- merge replication subscribing [SQL Server replication], pull subscriptions
- subscriptions [SQL Server replication], pull
- snapshot replication [SQL Server], subscribing
- transactional replication, subscribing
ms.assetid: 41d1886d-59c9-41fc-9bd6-a59b40e0af6e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 069c63b27714d9d0428b84846a19f2a31687c82e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347687"
---
# <a name="create-a-pull-subscription"></a>Create a Pull Subscription

<!--
2019-10-24 , GeneMi:
This article .md file exists in the so-called "2016+" section of repo 'sql-docs-pr'.
No article in 2016+ should ever have the moniker 'sql-server-2014' on its metadata line 'monikerRange:', I think.
Presently 'sql-server-2014' moniker is on this 'monikerRange'. This situation deserves further investigation.
-->

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  In diesem Thema wird beschrieben, wie ein Pullabonnement in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]oder Replikationsverwaltungsobjekten (RMO) erstellt wird.  
  
 Das Einrichten eines Pullabonnements für die P2P-Replikation ist mit einem Skript möglich, aber nicht über den Assistenten.  
 
  ##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 Erstellen Sie mit dem Assistenten für neue Abonnements ein Pullabonnement auf dem Verleger oder dem Abonnenten. Folgen Sie den Seiten im Assistenten für folgende Aufgaben:  
  
-   Angeben des Verlegers und der Veröffentlichung.  
  
-   Auswählen, wo die Replikations-Agents ausgeführt werden. Wählen Sie für ein Pullabonnement je nach Veröffentlichungstyp auf der Seite **Speicherort des Verteilungs-Agents** bzw. **Speicherort des Merge-Agents** die Option **Jeden Agent auf seinem Abonnenten ausführen (Pullabonnements)** aus.  
  
-   Angeben der Abonnenten und Abonnentendatenbanken.  
  
-   Angeben der Anmeldenamen und Kennwörter für Verbindungen zwischen den Replikations-Agents:  
  
    -   Bei Abonnements für Momentaufnahme- und Transaktionsveröffentlichungen geben Sie die Anmeldeinformationen auf der Seite **Sicherheit für den Verteilungs-Agent** an.  
  
    -   Bei Abonnements für Mergeveröffentlichungen geben Sie die Anmeldeinformationen auf der Seite **Sicherheit für den Merge-Agent** an.  
  
     Informationen zu den für die jeweiligen Agents erforderlichen Berechtigungen finden Sie unter [Sicherheitsmodell des Replikations-Agents](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
-   Angeben eines Synchronisierungszeitplans und wann der Abonnent initialisiert werden soll.  
  
-   Angeben weiterer Optionen für Mergeveröffentlichungen: Abonnementtyp; Werte für parametrisierte Filter; Informationen für die Synchronisierung über HTTP, wenn die Veröffentlichung für die Websynchronisierung aktiviert ist.  
  
-   Angeben weiterer Optionen für Transaktionsveröffentlichungen, die aktualisierbare Abonnements zulassen: ob Abonnenten sofort ein Commit für Änderungen auf dem Verleger ausführen oder die Änderungen in eine Warteschlange schreiben sollen; die Anmeldeinformationen zum Herstellen einer Verbindung zwischen Abonnenten und Verleger.  
  
-   Optionale Skripterstellung für das Abonnement.  
  
#### <a name="to-create-a-pull-subscription-from-the-publisher"></a>So erstellen Sie ein Pullabonnement auf dem Verleger  
  
1.  Stellen Sie in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten.  
  
2.  Erweitern Sie den Ordner **Replikation** , und erweitern Sie dann den Ordner **Lokale Veröffentlichungen** .  
  
3.  Klicken Sie mit der rechten Maustaste auf die Veröffentlichung, für die Sie eine oder mehrere Abonnements erstellen möchten, und klicken Sie dann auf **Neue Abonnements**.  
  
4.  Schließen Sie die Seiten im Assistenten für neue Abonnements ab.  

#### <a name="to-create-a-pull-subscription-from-the-subscriber"></a>So erstellen Sie ein Pullabonnement auf dem Abonnenten  
  
1.  Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]eine Verbindung mit dem Abonnenten her, und erweitern Sie dann den Serverknoten.  
  
2.  Erweitern Sie den Ordner **Replikation** .  
  
3.  Klicken Sie mit der rechten Maustaste auf den Ordner **Lokale Abonnements** , und klicken Sie dann auf **Neue Abonnements**.  
  
4.  Wählen Sie auf der Seite **Veröffentlichung** des Assistenten für neue Abonnements **\<Find SQL Server Publisher>** oder **\<Find Oracle Publisher>** aus der Dropdownliste **Verleger** aus.  
  
5.  Stellen Sie im Dialogfeld **Verbindung mit Server herstellen** eine Verbindung mit dem Verleger her.  
  
6.  Wählen Sie auf der Seite **Veröffentlichung** eine Veröffentlichung aus.  
  
7.  Schließen Sie die Seiten im Assistenten für neue Abonnements ab.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 Pullabonnements können mithilfe von gespeicherten Replikationsprozeduren programmgesteuert erstellt werden. Die verwendeten gespeicherten Prozeduren hängen vom Typ der Veröffentlichung ab, zu der das Abonnement gehört.  
  
#### <a name="to-create-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>So erstellen Sie ein Pullabonnement für eine Momentaufnahme- oder Transaktionsveröffentlichung  
  
1.  Überprüfen Sie auf dem Verleger, ob die Veröffentlichung Pullabonnements unterstützt, indem Sie [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md) ausführen.  
  
    -   Wenn **allow_pull** im Resultset den Wert **1** hat, dann unterstützt die Veröffentlichung Pullabonnements.  
  
    -   Wenn **allow_pull** den Wert **0** hat, führen Sie [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) aus, und geben Sie **allow_pull** für **\@property** und **true** für **\@value** an.  
  
2.  Führen Sie auf dem Abonnenten [sp_addpullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md) aus. Geben Sie **\@publisher** und **\@publication** an. Informationen zum Aktualisieren von Abonnements finden Sie unter [Erstellen eines aktualisierbaren Abonnements für eine Transaktionsveröffentlichung](publish/create-an-updatable-subscription-to-a-transactional-publication.md).   
  
3.  Führen Sie auf dem Abonnenten [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md) aus. Geben Sie Folgendes an:  
  
    -   Die Parameter **\@publisher**, **\@publisher_db** und **\@publication** .  
  
    -   Die [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Anmeldeinformationen, unter denen der Verteilungs-Agent auf dem Abonnenten ausgeführt wird, für **\@job_login** und **\@job_password**.  
  
        > [!NOTE]  
        >  Für Verbindungen, die mit der integrierten Windows-Authentifizierung hergestellt werden, werden immer die mit **\@job_login** und **\@job_password** angegebenen Windows-Anmeldeinformationen verwendet. Der Verteilungs-Agent stellt die lokale Verbindung mit dem Abonnenten immer mithilfe der integrierten Windows-Authentifizierung her. Standardmäßig stellt der Agent mithilfe der integrierten Windows-Authentifizierung eine Verbindung mit dem Verteiler her.  
  
    -   (Optional) Den Wert **0** für **\@distributor_security_mode** und die Anmeldeinformationen für SQL Server für **\@distributor_login** und **\@distributor_password**, wenn die SQL-Authentifizierung für den Verbindungsaufbau mit dem Verteiler verwendet werden muss.  
  
    -   Einen Zeitplan für den Verteilungs-Agentauftrag für dieses Abonnement. Weitere Informationen finden Sie unter [Angeben von Synchronisierungszeitplänen](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
4.  Führen Sie auf dem Verleger [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) aus, um das Pullabonnement zu registrieren. Geben Sie **\@publication**, **\@subscriber** und **\@destination_db** an. Geben Sie den Wert **pull** für **\@subscription_type** an.  
  
#### <a name="to-create-a-pull-subscription-to-a-merge-publication"></a>So erstellen Sie ein Pullabonnement für eine Mergeveröffentlichung  
  
1.  Überprüfen Sie auf dem Verleger, ob die Veröffentlichung Pullabonnements unterstützt, indem Sie [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md) ausführen.  
  
    -   Wenn **allow_pull** im Resultset den Wert **1** hat, dann unterstützt die Veröffentlichung Pullabonnements.  
  
    -   Wenn **allow_pull** den Wert **0** hat, führen Sie [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) aus, und geben Sie **allow_pull** für **\@property** und **true** für **\@value** an.  
  
2.  Führen Sie auf dem Abonnenten [sp_addmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md) aus. Geben Sie **\@publisher**, **\@publisher_db**, **\@publication** und die folgenden Parameter an:  
  
    -   **\@subscriber_type**: Geben Sie für ein Clientabonnement **local** und für ein Serverabonnement **global** an.  
  
    -   **\@subscription_priority**: Legen Sie für das Abonnement eine Priorität fest (**0.00** bis **99.99**). Dies ist nur für Serverabonnements erforderlich.  
  
         Weitere Informationen finden Sie unter [Erweiterte Konflikterkennung und -lösung bei der Mergereplikation](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)angegeben wird.  
  
3.  Führen Sie auf dem Abonnenten [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md) aus. Geben Sie die folgenden Parameter an:  
  
    -   **\@publisher**, **\@publisher_db** und **\@publication**.  
  
    -   Die Windows-Anmeldeinformationen, unter denen der Merge-Agent auf dem Abonnenten ausgeführt wird, für **\@job_login** und **\@job_password**.  
  
        > [!NOTE]  
        >  Für Verbindungen, die mit der integrierten Windows-Authentifizierung hergestellt werden, werden immer die mit **\@job_login** und **\@job_password** angegebenen Windows-Anmeldeinformationen verwendet. Der Merge-Agent stellt die lokale Verbindung mit dem Abonnenten immer mithilfe der Windows-Authentifizierung her. Standardmäßig stellt der Agent mithilfe der integrierten Windows-Authentifizierung eine Verbindung mit dem Verteiler und dem Verleger her.  
  
    -   (Optional) Den Wert **0** für **\@distributor_security_mode** und die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldeinformationen für **\@distributor_login** und **\@distributor_password**, wenn die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung für den Verbindungsaufbau mit dem Verteiler verwendet werden muss.  
  
    -   (Optional) Den Wert **0** für **\@publisher_security_mode** und die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldeinformationen für **\@publisher_login** und **\@publisher_password**, wenn die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung für den Verbindungsaufbau mit dem Verleger verwendet werden muss.  
  
    -   Einen Zeitplan für den Merge-Agentauftrag für dieses Abonnement. Weitere Informationen finden Sie unter [Erstellen eines aktualisierbaren Abonnements für eine Transaktionsveröffentlichung](publish/create-an-updatable-subscription-to-a-transactional-publication.md).  
  
4.  Führen Sie auf dem Verleger [sp_addmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md) aus. Geben Sie **\@publication**, **\@subscriber**, **\@subscriber_db** und den Wert **pull** für **\@subscription_type** an. Damit wird das Pullabonnement registriert.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Beispiele (Transact-SQL)  
 Im folgenden Beispiel wird ein Pullabonnement für eine Transaktionsveröffentlichung erstellt. Der erste Batch wird auf dem Abonnenten ausgeführt, und der zweite Batch wird auf dem Verleger ausgeführt. Die Werte für den Anmeldenamen und das Kennwort werden zur Laufzeit mithilfe von sqlcmd-Skriptvariablen bereitgestellt.  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Execute this batch at the Subscriber.  
DECLARE @publication AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @publicationDB AS sysname;  
SET @publication = N'AdvWorksProductTran';  
SET @publisher = $(PubServer);  
SET @publicationDB = N'AdventureWorks';  
  
-- At the subscription database, create a pull subscription   
-- to a transactional publication.  
USE [AdventureWorksReplica]  
EXEC sp_addpullsubscription   
  @publisher = @publisher,   
  @publication = @publication,   
  @publisher_db = @publicationDB;  
  
-- Add an agent job to synchronize the pull subscription.  
EXEC sp_addpullsubscription_agent   
  @publisher = @publisher,   
  @publisher_db = @publicationDB,   
  @publication = @publication,   
  @distributor = @publisher,   
  @job_login = $(Login),   
  @job_password = $(Password);  
GO  
```  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Execute this batch at the Publisher.  
DECLARE @publication AS sysname;  
DECLARE @subscriber AS sysname;  
DECLARE @subscriptionDB AS sysname;  
SET @publication = N'AdvWorksProductTran';  
SET @subscriber = $(SubServer);  
SET @subscriptionDB = N'AdventureWorksReplica';  
  
-- At the Publisher, register the subscription, using the defaults.  
EXEC sp_addsubscription   
  @publication = @publication,   
  @subscriber = @subscriber,   
  @destination_db = @subscriptionDB,   
  @subscription_type = N'pull',  
  @status = N'subscribed';  
GO  
  
```  
  
 Im folgenden Beispiel wird ein Pullabonnement für eine Mergeveröffentlichung erstellt. Der erste Batch wird auf dem Abonnenten ausgeführt, und der zweite Batch wird auf dem Verleger ausgeführt. Die Werte für den Anmeldenamen und das Kennwort werden zur Laufzeit mithilfe von **sqlcmd** -Skriptvariablen bereitgestellt.  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Execute this batch at the Subscriber.  
DECLARE @publication AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @publicationDB AS sysname;  
DECLARE @hostname AS sysname;  
SET @publication = N'AdvWorksSalesOrdersMerge';  
SET @publisher = $(PubServer);  
SET @publicationDB = N'AdventureWorks';  
SET @hostname = N'adventure-works\david8';  
  
-- At the subscription database, create a pull subscription   
-- to a merge publication.  
USE [AdventureWorksReplica]  
EXEC sp_addmergepullsubscription   
  @publisher = @publisher,   
  @publication = @publication,   
  @publisher_db = @publicationDB;  
  
-- Add an agent job to synchronize the pull subscription.   
EXEC sp_addmergepullsubscription_agent   
  @publisher = @publisher,   
  @publisher_db = @publicationDB,   
  @publication = @publication,   
  @distributor = @publisher,   
  @job_login = $(Login),   
  @job_password = $(Password),  
  @hostname = @hostname;  
GO  
```  
  
```  
-- Execute this batch at the Publisher.  
DECLARE @myMergePub  AS sysname;  
DECLARE @mySub       AS sysname;  
DECLARE @mySubDB     AS sysname;  
  
SET @myMergePub = N'AdvWorksSalesOrdersMerge';  
SET @mySub = N'MYSUBSERVER';  
SET @mySubDB = N'AdventureWorksReplica';  
  
-- At the Publisher, register the subscription, using the defaults.  
USE [AdventureWorks]  
EXEC sp_addmergesubscription @publication = @myMergePub,   
@subscriber = @mySub, @subscriber_db = @mySubDB,   
@subscription_type = N'pull';  
GO  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Verwenden von Replikationsverwaltungsobjekten (RMO)  
 Die RMO-Klassen, die zum Erstellen eines Pullabonnements verwendet werden, richten sich nach dem Typ der Veröffentlichung, zu der das Abonnement gehört.  
  
#### <a name="to-create-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>So erstellen Sie ein Pullabonnement für eine Momentaufnahme- oder Transaktionsveröffentlichung  
  
1.  Stellen Sie die Verbindung zum Abonnenten und zum Verleger her, indem Sie die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Klasse verwenden.  
  
2.  Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.TransPublication> -Klasse, indem Sie die Verlegerverbindung aus Schritt 1 verwenden. Geben Sie <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> und <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>an.  
  
3.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> -Methode auf. Wenn diese Methode **false** zurückgibt, sind entweder die in Schritt 2 angegebenen Eigenschaften falsch definiert, oder die Veröffentlichung ist auf dem Server nicht vorhanden.  
  
4.  Führen Sie ein bitweises logisches AND ( **&** in Visual C# und **And** in Visual Basic) zwischen der <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> -Eigenschaft und <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPull>. Falls das Ergebnis <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>lautet, legen Sie für <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> das Ergebnis eines bitweisen logischen OR ( **|** in Visual C# und **Or** in Visual Basic) zwischen <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> und <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPull>. Rufen Sie dann <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> auf, um Pullabonnements zu ermöglichen.  
  
5.  Falls die Abonnementdatenbank nicht vorhanden ist, erstellen Sie sie mithilfe der <xref:Microsoft.SqlServer.Management.Smo.Database> -Klasse. Weitere Informationen finden Sie unter [Erstellen, Ändern und Löschen von Datenbanken](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md).  
  
6.  Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.TransPullSubscription>-Klasse.  
  
7.  Legen Sie folgende Eigenschaften für das Abonnement fest:  
  
    -   Die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> zu dem in Schritt 1 erstellten Abonnenten für <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>  
  
    -   Name der Abonnementdatenbank für <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>.  
  
    -   Name des Verlegers für <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>.  
  
    -   Name der Veröffentlichungsdatenbank für <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A>.  
  
    -   Name der Veröffentlichung für <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>.  
  
    -   Die Parameter <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> und <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> von <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A> , um die Anmeldeinformationen für das [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Konto bereitzustellen, unter dem der Verteilungs-Agent auf dem Abonnenten ausgeführt wird. Mit diesem Konto werden lokale Verbindungen mit dem Abonnenten sowie Remoteverbindungen mithilfe der Windows-Authentifizierung hergestellt.  
  
        > [!NOTE]  
        >  <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A> muss nicht festgelegt werden, wenn das Abonnement von einem Mitglied der festen Serverrolle **sysadmin** erstellt wurde. Die Verwendung dieser Eigenschaft wird jedoch empfohlen. In diesem Fall nimmt der Agent die Identität des SQL Server-Agent-Kontos an. Weitere Informationen finden Sie unter [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
    -   (Optional) Den Wert **true** für <xref:Microsoft.SqlServer.Replication.PullSubscription.CreateSyncAgentByDefault%2A> , um einen Agentauftrag zu erstellen, mit dem das Abonnement synchronisiert wird. Wenn Sie **false** angeben (Standard), kann das Abonnement nur programmgesteuert synchronisiert werden, und Sie müssen zusätzliche Eigenschaften für <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent> angeben, wenn Sie von der <xref:Microsoft.SqlServer.Replication.TransPullSubscription.SynchronizationAgent%2A> -Eigenschaft auf dieses Objekt zugreifen. Weitere Informationen finden Sie unter [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
        > [!NOTE]  
        >  Der SQL Server-Agent ist nicht in jeder Edition von [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Von den SQL Server 2016-Editionen unterstützte Funktionen](../../sql-server/editions-and-components-of-sql-server-2016.md). Wenn Sie den Wert **true** für Express-Abonnenten angeben, wird der Agentauftrag nicht erstellt. Jedoch werden wichtige abonnementbezogene Metadaten auf dem Abonnenten gespeichert.  
  
    -   (Optional) Legen Sie die Werte der Felder <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> und <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> oder <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> für <xref:Microsoft.SqlServer.Replication.PullSubscription.DistributorSecurity%2A> fest, wenn Sie die SQL Server-Authentifizierung verwenden, um die Verbindung zum Verteiler herzustellen.  
  
8.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A> -Methode auf.  
  
9. Rufen Sie mit der Instanz der <xref:Microsoft.SqlServer.Replication.TransPublication> -Klasse aus Schritt 2 die <xref:Microsoft.SqlServer.Replication.TransPublication.MakePullSubscriptionWellKnown%2A> -Methode auf, um das Pullabonnement beim Verleger zu registrieren. Wenn diese Registrierung bereits vorhanden ist, tritt eine Ausnahme auf.  
  
#### <a name="to-create-a-pull-subscription-to-a-merge-publication"></a>So erstellen Sie ein Pullabonnement für eine Mergeveröffentlichung  
  
1.  Stellen Sie die Verbindung zum Abonnenten und zum Verleger her, indem Sie die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Klasse verwenden.  
  
2.  Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.MergePublication> -Klasse, indem Sie die Verlegerverbindung aus Schritt 1 verwenden. Geben Sie <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>und <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>an.  
  
3.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> -Methode auf. Wenn diese Methode **false** zurückgibt, sind entweder die in Schritt 2 angegebenen Eigenschaften falsch definiert, oder die Veröffentlichung ist auf dem Server nicht vorhanden.  
  
4.  Führen Sie ein bitweises logisches AND ( **&** in Visual C# und **And** in Visual Basic) zwischen der <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> -Eigenschaft und <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPull>. Falls das Ergebnis <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>lautet, legen Sie für <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> das Ergebnis eines bitweisen logischen OR ( **|** in Visual C# und **Or** in Visual Basic) zwischen <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> und <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPull>. Rufen Sie dann <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> auf, um Pullabonnements zu ermöglichen.  
  
5.  Falls die Abonnementdatenbank nicht vorhanden ist, erstellen Sie sie mithilfe der <xref:Microsoft.SqlServer.Management.Smo.Database> -Klasse. Weitere Informationen finden Sie unter [Erstellen, Ändern und Löschen von Datenbanken](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md).  
  
6.  Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.MergePullSubscription>-Klasse.  
  
7.  Legen Sie folgende Eigenschaften für das Abonnement fest:  
  
    -   Die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> zu dem in Schritt 1 erstellten Abonnenten für <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>  
  
    -   Name der Abonnementdatenbank für <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>.  
  
    -   Name des Verlegers für <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>.  
  
    -   Name der Veröffentlichungsdatenbank für <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A>.  
  
    -   Name der Veröffentlichung für <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>.  
  
    -   Die Parameter <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> und <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> von <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A> , um die Anmeldeinformationen für das [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Konto bereitzustellen, unter dem der Merge-Agent auf dem Abonnenten ausgeführt wird. Mit diesem Konto werden lokale Verbindungen mit dem Abonnenten sowie Remoteverbindungen mithilfe der Windows-Authentifizierung hergestellt.  
  
        > [!NOTE]  
        >  <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A> muss nicht festgelegt werden, wenn das Abonnement von einem Mitglied der festen Serverrolle **sysadmin** erstellt wurde. Die Verwendung dieser Eigenschaft wird jedoch empfohlen. In diesem Fall nimmt der Agent die Identität des SQL Server-Agent-Kontos an. Weitere Informationen finden Sie unter [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
    -   (Optional) Den Wert **true** für <xref:Microsoft.SqlServer.Replication.PullSubscription.CreateSyncAgentByDefault%2A> , um einen Agentauftrag zu erstellen, mit dem das Abonnement synchronisiert wird. Wenn Sie **false** angeben (Standard), kann das Abonnement nur programmgesteuert synchronisiert werden, und Sie müssen zusätzliche Eigenschaften für <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent> angeben, wenn Sie von der <xref:Microsoft.SqlServer.Replication.MergePullSubscription.SynchronizationAgent%2A> -Eigenschaft auf dieses Objekt zugreifen. Weitere Informationen finden Sie unter [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
    -   (Optional) Legen Sie die Werte der Felder <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> und <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> oder <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> für <xref:Microsoft.SqlServer.Replication.PullSubscription.DistributorSecurity%2A> fest, wenn Sie die SQL Server-Authentifizierung verwenden, um die Verbindung zum Verteiler herzustellen.  
  
    -   (Optional) Legen Sie die Felder <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> und <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> oder <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> von <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherSecurity%2A> fest, wenn Sie die SQL Server-Authentifizierung zum Herstellen einer Verbindung mit dem Verleger verwenden.  
  
8.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A> -Methode auf.  
  
9. Rufen Sie mit der Instanz der <xref:Microsoft.SqlServer.Replication.MergePublication> -Klasse aus Schritt 2 die <xref:Microsoft.SqlServer.Replication.MergePublication.MakePullSubscriptionWellKnown%2A> -Methode auf, um das Pullabonnement beim Verleger zu registrieren. Wenn diese Registrierung bereits vorhanden ist, tritt eine Ausnahme auf.  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> Beispiel (RMO)  
 Im folgenden Beispiel wird ein neues Pullabonnement für eine Transaktionsveröffentlichung erstellt. Die Anmeldeinformationen für das [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Konto, mit dem der Verteilungs-Agentauftrag erstellt wird, werden zur Laufzeit übergeben.  
  
```csharp  
// Define the Publisher, publication, and databases.  
string publicationName = "AdvWorksProductTran";  
string publisherName = publisherInstance;  
string subscriberName = subscriberInstance;  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
  
//Create connections to the Publisher and Subscriber.  
ServerConnection subscriberConn = new ServerConnection(subscriberName);  
ServerConnection publisherConn = new ServerConnection(publisherName);  
  
// Create the objects that we need.  
TransPublication publication;  
TransPullSubscription subscription;  
  
try  
{  
    // Connect to the Publisher and Subscriber.  
    subscriberConn.Connect();  
    publisherConn.Connect();  
  
    // Ensure that the publication exists and that   
    // it supports pull subscriptions.  
    publication = new TransPublication();  
    publication.Name = publicationName;  
    publication.DatabaseName = publicationDbName;  
    publication.ConnectionContext = publisherConn;  
  
    if (publication.IsExistingObject)  
    {  
        if ((publication.Attributes & PublicationAttributes.AllowPull) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowPull;  
        }  
  
        // Define the pull subscription.  
        subscription = new TransPullSubscription();  
        subscription.ConnectionContext = subscriberConn;  
        subscription.PublisherName = publisherName;  
        subscription.PublicationName = publicationName;  
        subscription.PublicationDBName = publicationDbName;  
        subscription.DatabaseName = subscriptionDbName;  
  
        // Specify the Windows login credentials for the Distribution Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin;  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword;  
  
        // Make sure that the agent job for the subscription is created.  
        subscription.CreateSyncAgentByDefault = true;  
  
        // By default, subscriptions to transactional publications are synchronized   
        // continuously, but in this case we only want to synchronize on demand.  
        subscription.AgentSchedule.FrequencyType = ScheduleFrequencyType.OnDemand;  
  
        // Create the pull subscription at the Subscriber.  
        subscription.Create();  
  
        Boolean registered = false;  
  
        // Verify that the subscription is not already registered.  
        foreach (TransSubscription existing  
            in publication.EnumSubscriptions())  
        {  
            if (existing.SubscriberName == subscriberName  
                && existing.SubscriptionDBName == subscriptionDbName)  
            {  
                registered = true;  
            }  
        }  
        if (!registered)  
        {  
            // Register the subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown(  
                subscriberName, subscriptionDbName,  
                SubscriptionSyncType.Automatic,  
                TransSubscriberType.ReadOnly);  
        }  
    }  
    else  
    {  
        // Do something here if the publication does not exist.  
        throw new ApplicationException(String.Format(  
            "The publication '{0}' does not exist on {1}.",  
            publicationName, publisherName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement the appropriate error handling here.  
    throw new ApplicationException(String.Format(  
        "The subscription to {0} could not be created.", publicationName), ex);  
}  
finally  
{  
    subscriberConn.Disconnect();  
    publisherConn.Disconnect();  
}  
  
```  
  
```vb  
' Define the Publisher, publication, and databases.  
Dim publicationName As String = "AdvWorksProductTran"  
Dim publisherName As String = publisherInstance  
Dim subscriberName As String = subscriberInstance  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
  
'Create connections to the Publisher and Subscriber.  
Dim subscriberConn As ServerConnection = New ServerConnection(subscriberName)  
Dim publisherConn As ServerConnection = New ServerConnection(publisherName)  
  
' Create the objects that we need.  
Dim publication As TransPublication  
Dim subscription As TransPullSubscription  
  
Try  
    ' Connect to the Publisher and Subscriber.  
    subscriberConn.Connect()  
    publisherConn.Connect()  
  
    ' Ensure that the publication exists and that   
    ' it supports pull subscriptions.  
    publication = New TransPublication()  
    publication.Name = publicationName  
    publication.DatabaseName = publicationDbName  
    publication.ConnectionContext = publisherConn  
  
    If publication.IsExistingObject Then  
        If (publication.Attributes And PublicationAttributes.AllowPull) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowPull  
        End If  
  
        ' Define the pull subscription.  
        subscription = New TransPullSubscription()  
        subscription.ConnectionContext = subscriberConn  
        subscription.PublisherName = publisherName  
        subscription.PublicationName = publicationName  
        subscription.PublicationDBName = publicationDbName  
        subscription.DatabaseName = subscriptionDbName  
        subscription.Description = "Pull subscription to " + publicationDbName _  
        + " on " + subscriberName + "."  
  
        ' Specify the Windows login credentials for the Distribution Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword  
  
        ' Make sure that the agent job for the subscription is created.  
        subscription.CreateSyncAgentByDefault = True  
  
        ' By default, subscriptions to transactional publications are synchronized   
        ' continuously, but in this case we only want to synchronize on demand.  
        subscription.AgentSchedule.FrequencyType = ScheduleFrequencyType.OnDemand  
  
        ' Create the pull subscription at the Subscriber.  
        subscription.Create()  
  
        Dim registered As Boolean = False  
  
        ' Verify that the subscription is not already registered.  
        For Each existing As TransSubscription In publication.EnumSubscriptions()  
            If existing.SubscriberName = subscriberName And _  
                existing.SubscriptionDBName = subscriptionDbName Then  
                registered = True  
            End If  
        Next existing  
        If Not registered Then  
            ' Register the subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown( _  
             subscriberName, subscriptionDbName, _  
             SubscriptionSyncType.Automatic, _  
             TransSubscriberType.ReadOnly)  
        End If  
    Else  
        ' Do something here if the publication does not exist.  
        Throw New ApplicationException(String.Format( _  
         "The publication '{0}' does not exist on {1}.", _  
         publicationName, publisherName))  
    End If  
Catch ex As Exception  
    ' Implement the appropriate error handling here.  
    Throw New ApplicationException(String.Format( _  
        "The subscription to {0} could not be created.", publicationName), ex)  
Finally  
    subscriberConn.Disconnect()  
    publisherConn.Disconnect()  
End Try  
  
```  
  
 Im folgenden Beispiel wird ein Pullabonnement für eine Mergeveröffentlichung erstellt. Die Anmeldeinformationen für das Windows-Konto, mit dem der Merge-Agentauftrag erstellt wird, werden zur Laufzeit übergeben.  
  
```cpp#  
// Define the Publisher, publication, and databases.  
string publicationName = "AdvWorksSalesOrdersMerge";  
string publisherName = publisherInstance;  
string subscriberName = subscriberInstance;  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
string hostname = @"adventure-works\garrett1";  
  
//Create connections to the Publisher and Subscriber.  
ServerConnection subscriberConn = new ServerConnection(subscriberName);  
ServerConnection publisherConn = new ServerConnection(publisherName);  
  
// Create the objects that we need.  
MergePublication publication;  
MergePullSubscription subscription;  
  
try  
{  
    // Connect to the Subscriber.  
    subscriberConn.Connect();  
  
    // Ensure that the publication exists and that   
    // it supports pull subscriptions.  
    publication = new MergePublication();  
    publication.Name = publicationName;  
    publication.DatabaseName = publicationDbName;  
    publication.ConnectionContext = publisherConn;  
  
    if (publication.LoadProperties())  
    {  
        if ((publication.Attributes & PublicationAttributes.AllowPull) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowPull;  
        }  
  
        // Define the pull subscription.  
        subscription = new MergePullSubscription();  
        subscription.ConnectionContext = subscriberConn;  
        subscription.PublisherName = publisherName;  
        subscription.PublicationName = publicationName;  
        subscription.PublicationDBName = publicationDbName;  
        subscription.DatabaseName = subscriptionDbName;  
        subscription.HostName = hostname;  
  
        // Specify the Windows login credentials for the Merge Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin;  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword;  
  
        // Make sure that the agent job for the subscription is created.  
        subscription.CreateSyncAgentByDefault = true;  
  
        // Create the pull subscription at the Subscriber.  
        subscription.Create();  
  
        Boolean registered = false;  
  
        // Verify that the subscription is not already registered.  
        foreach (MergeSubscription existing  
            in publication.EnumSubscriptions())  
        {  
            if (existing.SubscriberName == subscriberName  
                && existing.SubscriptionDBName == subscriptionDbName  
                && existing.SubscriptionType == SubscriptionOption.Pull)  
            {  
                registered = true;  
            }  
        }  
        if (!registered)  
        {  
            // Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown(  
                subscriberName, subscriptionDbName,  
                SubscriptionSyncType.Automatic,  
                MergeSubscriberType.Local, 0);  
        }  
    }  
    else  
    {  
        // Do something here if the publication does not exist.  
        throw new ApplicationException(String.Format(  
            "The publication '{0}' does not exist on {1}.",  
            publicationName, publisherName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement the appropriate error handling here.  
    throw new ApplicationException(String.Format(  
        "The subscription to {0} could not be created.", publicationName), ex);  
}  
finally  
{  
    subscriberConn.Disconnect();  
    publisherConn.Disconnect();  
}  
  
```  
  
```vb  
' Define the Publisher, publication, and databases.  
Dim publicationName As String = "AdvWorksSalesOrdersMerge"  
Dim publisherName As String = publisherInstance  
Dim subscriberName As String = subscriberInstance  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
Dim hostname As String = "adventure-works\garrett1"  
  
'Create connections to the Publisher and Subscriber.  
Dim subscriberConn As ServerConnection = New ServerConnection(subscriberName)  
Dim publisherConn As ServerConnection = New ServerConnection(publisherName)  
  
' Create the objects that we need.  
Dim publication As MergePublication  
Dim subscription As MergePullSubscription  
  
Try  
    ' Connect to the Subscriber.  
    subscriberConn.Connect()  
  
    ' Ensure that the publication exists and that   
    ' it supports pull subscriptions.  
    publication = New MergePublication()  
    publication.Name = publicationName  
    publication.DatabaseName = publicationDbName  
    publication.ConnectionContext = publisherConn  
  
    If publication.LoadProperties() Then  
        If (publication.Attributes And PublicationAttributes.AllowPull) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowPull  
        End If  
  
        ' Define the pull subscription.  
        subscription = New MergePullSubscription()  
        subscription.ConnectionContext = subscriberConn  
        subscription.PublisherName = publisherName  
        subscription.PublicationName = publicationName  
        subscription.PublicationDBName = publicationDbName  
        subscription.DatabaseName = subscriptionDbName  
        subscription.HostName = hostname  
  
        ' Specify the Windows login credentials for the Merge Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword  
  
        ' Make sure that the agent job for the subscription is created.  
        subscription.CreateSyncAgentByDefault = True  
  
        ' Create the pull subscription at the Subscriber.  
        subscription.Create()  
  
        Dim registered As Boolean = False  
  
        ' Verify that the subscription is not already registered.  
        For Each existing As MergeSubscription In _  
        publication.EnumSubscriptions()  
            If existing.SubscriberName = subscriberName Then  
                registered = True  
            End If  
        Next  
        If Not registered Then  
            ' Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown( _  
             subscriberName, subscriptionDbName, _  
             SubscriptionSyncType.Automatic, _  
             MergeSubscriberType.Local, 0)  
        End If  
    Else  
        ' Do something here if the publication does not exist.  
        Throw New ApplicationException(String.Format( _  
         "The publication '{0}' does not exist on {1}.", _  
         publicationName, publisherName))  
    End If  
Catch ex As Exception  
    ' Implement the appropriate error handling here.  
    Throw New ApplicationException(String.Format( _  
        "The subscription to {0} could not be created.", publicationName), ex)  
Finally  
    subscriberConn.Disconnect()  
    publisherConn.Disconnect()  
End Try  
```  
  
 In diesem Beispiel wird ein Pullabonnement für eine Mergeveröffentlichung erstellt, ohne dass ein zugehöriger Agentauftrag und Abonnementmetadaten in [MSsubscription_properties](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md)erstellt werden. Die Anmeldeinformationen für das Windows-Konto, mit dem der Merge-Agentauftrag erstellt wird, werden zur Laufzeit übergeben.  
  
```csharp  
// Define the Publisher, publication, and databases.  
string publicationName = "AdvWorksSalesOrdersMerge";  
string publisherName = publisherInstance;  
string subscriberName = subscriberInstance;  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
  
//Create connections to the Publisher and Subscriber.  
ServerConnection subscriberConn = new ServerConnection(subscriberName);  
ServerConnection publisherConn = new ServerConnection(publisherName);  
  
// Create the objects that we need.  
MergePublication publication;  
MergePullSubscription subscription;  
  
try  
{  
    // Connect to the Subscriber.  
    subscriberConn.Connect();  
  
    // Ensure that the publication exists and that   
    // it supports pull subscriptions.  
    publication = new MergePublication();  
    publication.Name = publicationName;  
    publication.DatabaseName = publicationDbName;  
    publication.ConnectionContext = publisherConn;  
  
    if (publication.LoadProperties())  
    {  
        if ((publication.Attributes & PublicationAttributes.AllowPull) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowPull;  
        }  
  
        // Define the pull subscription.  
        subscription = new MergePullSubscription();  
        subscription.ConnectionContext = subscriberConn;  
        subscription.PublisherName = publisherName;  
        subscription.PublicationName = publicationName;  
        subscription.PublicationDBName = publicationDbName;  
        subscription.DatabaseName = subscriptionDbName;  
  
        // Specify that an agent job not be created for this subscription. The  
        // subscription can only be synchronized by running the Merge Agent directly.  
        // Subscripition metadata stored in MSsubscription_properties will not  
        // be available and must be specified at run time.  
        subscription.CreateSyncAgentByDefault = false;  
  
        // Create the pull subscription at the Subscriber.  
        subscription.Create();  
  
        Boolean registered = false;  
  
        // Verify that the subscription is not already registered.  
        foreach (MergeSubscription existing  
            in publication.EnumSubscriptions())  
        {  
            if (existing.SubscriberName == subscriberName  
                && existing.SubscriptionDBName == subscriptionDbName  
                && existing.SubscriptionType == SubscriptionOption.Pull)  
            {  
                registered = true;  
            }  
        }  
        if (!registered)  
        {  
            // Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown(  
                subscriberName, subscriptionDbName,  
                SubscriptionSyncType.Automatic,  
                MergeSubscriberType.Local, 0);  
        }  
    }  
    else  
    {  
        // Do something here if the publication does not exist.  
        throw new ApplicationException(String.Format(  
            "The publication '{0}' does not exist on {1}.",  
            publicationName, publisherName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement the appropriate error handling here.  
    throw new ApplicationException(String.Format(  
        "The subscription to {0} could not be created.", publicationName), ex);  
}  
finally  
{  
    subscriberConn.Disconnect();  
    publisherConn.Disconnect();  
}  
```  
  
```vb  
' Define the Publisher, publication, and databases.  
Dim publicationName As String = "AdvWorksSalesOrdersMerge"  
Dim publisherName As String = publisherInstance  
Dim subscriberName As String = subscriberInstance  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
  
'Create connections to the Publisher and Subscriber.  
Dim subscriberConn As ServerConnection = New ServerConnection(subscriberName)  
Dim publisherConn As ServerConnection = New ServerConnection(publisherName)  
  
' Create the objects that we need.  
Dim publication As MergePublication  
Dim subscription As MergePullSubscription  
  
Try  
    ' Connect to the Subscriber.  
    subscriberConn.Connect()  
  
    ' Ensure that the publication exists and that   
    ' it supports pull subscriptions.  
    publication = New MergePublication()  
    publication.Name = publicationName  
    publication.DatabaseName = publicationDbName  
    publication.ConnectionContext = publisherConn  
  
    If publication.LoadProperties() Then  
        If (publication.Attributes And PublicationAttributes.AllowPull) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowPull  
        End If  
  
        ' Define the pull subscription.  
        subscription = New MergePullSubscription()  
        subscription.ConnectionContext = subscriberConn  
        subscription.PublisherName = publisherName  
        subscription.PublicationName = publicationName  
        subscription.PublicationDBName = publicationDbName  
        subscription.DatabaseName = subscriptionDbName  
  
        ' Specify that an agent job not be created for this subscription. The  
        ' subscription can only be synchronized by running the Merge Agent directly.  
        ' Subscripition metadata stored in MSsubscription_properties will not  
        ' be available and must be specified at run time.  
        subscription.CreateSyncAgentByDefault = False  
  
        ' Create the pull subscription at the Subscriber.  
        subscription.Create()  
  
        Dim registered As Boolean = False  
  
        ' Verify that the subscription is not already registered.  
        For Each existing As MergeSubscription In _  
        publication.EnumSubscriptions()  
            If existing.SubscriberName = subscriberName Then  
                registered = True  
            End If  
        Next  
        If Not registered Then  
            ' Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown( _  
             subscriberName, subscriptionDbName, _  
             SubscriptionSyncType.Automatic, _  
             MergeSubscriberType.Local, 0)  
        End If  
    Else  
        ' Do something here if the publication does not exist.  
        Throw New ApplicationException(String.Format( _  
         "The publication '{0}' does not exist on {1}.", _  
         publicationName, publisherName))  
    End If  
Catch ex As Exception  
    ' Implement the appropriate error handling here.  
    Throw New ApplicationException(String.Format( _  
     "The subscription to {0} could not be created.", publicationName), ex)  
Finally  
    subscriberConn.Disconnect()  
    publisherConn.Disconnect()  
End Try  
```  
  
 In diesem Beispiel wird ein Pullabonnement für eine Mergeveröffentlichung erstellt, das mithilfe der Websynchronisierung über das Internet synchronisiert werden kann. Die Anmeldeinformationen für das Windows-Konto, mit dem der Merge-Agentauftrag erstellt wird, werden zur Laufzeit übergeben. Weitere Informationen finden Sie unter [Configure Web Synchronization](../../relational-databases/replication/configure-web-synchronization.md).  
  
```csharp  
// Define the Publisher, publication, and databases.  
string publicationName = "AdvWorksSalesOrdersMerge";  
string publisherName = publisherInstance;  
string subscriberName = subscriberInstance;  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
string hostname = @"adventure-works\garrett1";  
string webSyncUrl = "https://" + publisherInstance + "/WebSync/replisapi.dll";  
  
//Create connections to the Publisher and Subscriber.  
ServerConnection subscriberConn = new ServerConnection(subscriberName);  
ServerConnection publisherConn = new ServerConnection(publisherName);  
  
// Create the objects that we need.  
MergePublication publication;  
MergePullSubscription subscription;  
  
try  
{  
    // Connect to the Subscriber.  
    subscriberConn.Connect();  
  
    // Ensure that the publication exists and that   
    // it supports pull subscriptions and Web synchronization.  
    publication = new MergePublication();  
    publication.Name = publicationName;  
    publication.DatabaseName = publicationDbName;  
    publication.ConnectionContext = publisherConn;  
  
    if (publication.LoadProperties())  
    {  
        if ((publication.Attributes & PublicationAttributes.AllowPull) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowPull;  
        }  
        if ((publication.Attributes & PublicationAttributes.AllowWebSynchronization) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowWebSynchronization;  
        }  
  
        // Define the pull subscription.  
        subscription = new MergePullSubscription();  
        subscription.ConnectionContext = subscriberConn;  
        subscription.PublisherName = publisherName;  
        subscription.PublicationName = publicationName;  
        subscription.PublicationDBName = publicationDbName;  
        subscription.DatabaseName = subscriptionDbName;  
        subscription.HostName = hostname;  
  
        // Specify the Windows login credentials for the Merge Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin;  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword;  
  
        // Enable Web synchronization.  
        subscription.UseWebSynchronization = true;  
        subscription.InternetUrl = webSyncUrl;  
  
        // Specify the same Windows credentials to use when connecting to the  
        // Web server using HTTPS Basic Authentication.  
        subscription.InternetSecurityMode = AuthenticationMethod.BasicAuthentication;  
        subscription.InternetLogin = winLogin;  
        subscription.InternetPassword = winPassword;  
  
        // Ensure that we create a job for this subscription.  
        subscription.CreateSyncAgentByDefault = true;  
  
        // Create the pull subscription at the Subscriber.  
        subscription.Create();  
  
        Boolean registered = false;  
  
        // Verify that the subscription is not already registered.  
        foreach (MergeSubscription existing  
            in publication.EnumSubscriptions())  
        {  
            if (existing.SubscriberName == subscriberName  
                && existing.SubscriptionDBName == subscriptionDbName  
                && existing.SubscriptionType == SubscriptionOption.Pull)  
            {  
                registered = true;  
            }  
        }  
        if (!registered)  
        {  
            // Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown(  
                subscriberName, subscriptionDbName,  
                SubscriptionSyncType.Automatic,  
                MergeSubscriberType.Local, 0);  
        }  
    }  
    else  
    {  
        // Do something here if the publication does not exist.  
        throw new ApplicationException(String.Format(  
            "The publication '{0}' does not exist on {1}.",  
            publicationName, publisherName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement the appropriate error handling here.  
    throw new ApplicationException(String.Format(  
        "The subscription to {0} could not be created.", publicationName), ex);  
}  
finally  
{  
    subscriberConn.Disconnect();  
    publisherConn.Disconnect();  
}  
```  
  
```vb  
' Define the Publisher, publication, and databases.  
Dim publicationName As String = "AdvWorksSalesOrdersMerge"  
Dim publisherName As String = publisherInstance  
Dim subscriberName As String = subscriberInstance  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
Dim hostname As String = "adventure-works\garrett1"  
Dim webSyncUrl As String = "https://" + publisherInstance + "/WebSync/replisapi.dll"  
  
'Create connections to the Publisher and Subscriber.  
Dim subscriberConn As ServerConnection = New ServerConnection(subscriberName)  
Dim publisherConn As ServerConnection = New ServerConnection(publisherName)  
  
' Create the objects that we need.  
Dim publication As MergePublication  
Dim subscription As MergePullSubscription  
  
Try  
    ' Connect to the Subscriber.  
    subscriberConn.Connect()  
  
    ' Ensure that the publication exists and that   
    ' it supports pull subscriptions and Web synchronization.  
    publication = New MergePublication()  
    publication.Name = publicationName  
    publication.DatabaseName = publicationDbName  
    publication.ConnectionContext = publisherConn  
  
    If publication.LoadProperties() Then  
        If (publication.Attributes And PublicationAttributes.AllowPull) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowPull  
        End If  
        If (publication.Attributes And PublicationAttributes.AllowWebSynchronization) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowWebSynchronization  
        End If  
  
        ' Define the pull subscription.  
        subscription = New MergePullSubscription()  
        subscription.ConnectionContext = subscriberConn  
        subscription.PublisherName = publisherName  
        subscription.PublicationName = publicationName  
        subscription.PublicationDBName = publicationDbName  
        subscription.DatabaseName = subscriptionDbName  
        subscription.HostName = hostname  
        subscription.CreateSyncAgentByDefault = True  
  
        ' Specify the Windows login credentials for the Merge Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword  
  
        ' Enable Web synchronization.  
        subscription.UseWebSynchronization = True  
        subscription.InternetUrl = webSyncUrl  
  
        ' Specify the same Windows credentials to use when connecting to the  
        ' Web server using HTTPS Basic Authentication.  
        subscription.InternetSecurityMode = AuthenticationMethod.BasicAuthentication  
        subscription.InternetLogin = winLogin  
        subscription.InternetPassword = winPassword  
  
        ' Create the pull subscription at the Subscriber.  
        subscription.Create()  
  
        Dim registered As Boolean = False  
  
        ' Verify that the subscription is not already registered.  
        For Each existing As MergeSubscription In _  
        publication.EnumSubscriptions()  
            If existing.SubscriberName = subscriberName Then  
                registered = True  
            End If  
        Next  
        If Not registered Then  
            ' Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown( _  
             subscriberName, subscriptionDbName, _  
             SubscriptionSyncType.Automatic, _  
             MergeSubscriberType.Local, 0)  
        End If  
    Else  
        ' Do something here if the publication does not exist.  
        Throw New ApplicationException(String.Format( _  
         "The publication '{0}' does not exist on {1}.", _  
         publicationName, publisherName))  
    End If  
Catch ex As Exception  
    ' Implement the appropriate error handling here.  
    Throw New ApplicationException(String.Format( _  
     "The subscription to {0} could not be created.", publicationName), ex)  
Finally  
    subscriberConn.Disconnect()  
    publisherConn.Disconnect()  
End Try  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Anzeigen und Ändern der Eigenschaften von Pullabonnements](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)   
 [Konfigurieren der Websynchronisierung](../../relational-databases/replication/configure-web-synchronization.md)   
 [Abonnieren von Veröffentlichungen](../../relational-databases/replication/subscribe-to-publications.md)   
 [Bewährte Methoden für die Replikationssicherheit](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
