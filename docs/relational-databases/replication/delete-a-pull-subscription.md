---
description: Löschen eines Pullabonnements
title: Löschen eines Pullabonnements |Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- removing subscriptions
- deleting subscriptions
- pull subscriptions [SQL Server replication], deleting
- subscriptions [SQL Server replication], pull
ms.assetid: 997c0b8e-d8d9-4eed-85b1-6baa1f8594ce
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 1da4312a4eb0f25b82b820ef9004bb9c64e5fff7
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076392"
---
# <a name="delete-a-pull-subscription"></a>Löschen eines Pullabonnements
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  In diesem Thema wird beschrieben, wie ein Pullabonnement in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]oder Replikationsverwaltungsobjekten (RMO) gelöscht wird.  
  
 **In diesem Thema**  
  
-   **So löschen Sie ein Pullabonnement mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replikationsverwaltungsobjekte (RMO)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 Löschen Sie ein Pullabonnement auf dem Verleger (aus dem Ordner **Lokale Veröffentlichungen** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]) oder dem Abonnenten (aus dem Ordner **Lokale Abonnenten** ). Beim Löschen eines Abonnements werden keine Objekte oder Daten aus dem Abonnement entfernt, diese müssen manuell entfernt werden.  
  
#### <a name="to-delete-a-pull-subscription-at-the-publisher"></a>So löschen Sie ein Pullabonnement auf dem Verleger  
  
1.  Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten.  
  
2.  Erweitern Sie den Ordner **Replikation** , und erweitern Sie dann den Ordner **Lokale Veröffentlichungen** .  
  
3.  Erweitern Sie die Veröffentlichung, der das zu löschende Abonnement zugeordnet ist.  
  
4.  Klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Löschen**.  
  
5.  Wählen Sie im Bestätigungsdialogfeld aus, ob zum Löschen der Abonnementinformationen eine Verbindung mit dem Abonnenten hergestellt werden soll. Wenn Sie das Kontrollkästchen **Verbindung mit Abonnenten herstellen** deaktivieren, müssen Sie zum Löschen der Informationen später eine Verbindung mit dem Abonnenten herstellen.  
  
#### <a name="to-delete-a-pull-subscription-at-the-subscriber"></a>So löschen Sie ein Pullabonnement auf dem Abonnenten  
  
1.  Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]eine Verbindung mit dem Abonnenten her, und erweitern Sie dann den Serverknoten.  
  
2.  Erweitern Sie den Ordner **Replikation** , und erweitern Sie dann den Ordner **Lokale Abonnements** .  
  
3.  Klicken Sie mit der rechten Maustaste auf das Abonnement, das Sie löschen möchten, und klicken Sie dann auf **Löschen**.  
  
4.  Wählen Sie im Bestätigungsdialogfeld aus, ob zum Löschen der Abonnementinformationen eine Verbindung mit dem Verleger hergestellt werden soll. Wenn Sie das Kontrollkästchen **Verbindung mit Verleger herstellen** deaktivieren, müssen Sie zum Löschen der Informationen später eine Verbindung mit dem Verleger herstellen.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 Pullabonnements können mithilfe von gespeicherten Replikationsprozeduren programmgesteuert gelöscht werden. Die verwendeten gespeicherten Prozeduren hängen vom Typ der Veröffentlichung ab, zu der das Abonnement gehört.  
  
#### <a name="to-delete-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>So löschen Sie ein Pullabonnement für eine Momentaufnahme- oder Transaktionsveröffentlichung  
  
1.  Führen Sie auf dem Abonnenten für die Abonnementdatenbank [sp_droppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md) aus. Geben Sie **\@publication**, **\@publisher** und **\@publisher_db** an.  
  
2.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md) aus. Geben Sie **\@publication** und **\@subscriber** an. Geben Sie für **\@article** den Wert **all** an. (Optional) Wenn auf den Verteiler nicht zugegriffen werden kann, geben Sie für **\@ignore_distributor** den Wert **1** an, um das Abonnement ohne die damit verbundenen Objekte auf dem Verteiler zu löschen.  
  
#### <a name="to-delete-a-pull-subscription-to-a-merge-publication"></a>So löschen Sie ein Pullabonnement für eine Mergeveröffentlichung  
  
1.  Führen Sie auf dem Abonnenten für die Abonnementdatenbank [sp_dropmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md) aus. Geben Sie **\@publication**, **\@publisher** und **\@publisher_db** an.  
  
2.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_dropmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)aus. Geben Sie **\@publication**, **\@subscriber** und **\@subscriber_db** an. Geben Sie den Wert **pull** für **\@subscription_type** an. (Optional) Wenn auf den Verteiler nicht zugegriffen werden kann, geben Sie für **\@ignore_distributor** den Wert **1** an, um das Abonnement ohne die damit verbundenen Objekte auf dem Verteiler zu löschen.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Beispiele (Transact-SQL)  
 Im folgenden Beispiel wird ein Pullabonnement für eine Transaktionsveröffentlichung gelöscht. Der erste Batch wird auf dem Abonnenten ausgeführt und der zweite wird auf dem Verleger ausgeführt.  
  
 [!code-sql[HowTo#sp_droptranpullsubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-pull-subscription_1.sql)]  
  
 [!code-sql[HowTo#sp_droptransubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-pull-subscription_2.sql)]  
  
 Im folgenden Beispiel wird ein Pullabonnement für eine Mergeveröffentlichung gelöscht. Der erste Batch wird auf dem Abonnenten ausgeführt und der zweite wird auf dem Verleger ausgeführt.  
  
 [!code-sql[HowTo#sp_dropmergepullsubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-pull-subscription_3.sql)]  
  
 [!code-sql[HowTo#sp_dropmergesubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-pull-subscription_4.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Verwenden von Replikationsverwaltungsobjekten (RMO)  
 Sie können Pullabonnements mithilfe von Replikationsverwaltungsobjekten (RMO) programmgesteuert löschen. Die RMO-Klassen, mit denen Sie ein Pullabonnement löschen, hängen vom Typ der Veröffentlichung ab, für die das Pullabonnement abonniert wird.  
  
#### <a name="to-delete-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>So löschen Sie ein Pullabonnement für eine Momentaufnahme- oder Transaktionsveröffentlichung  
  
1.  Stellen Sie die Verbindung zum Abonnenten und zum Verleger her, indem Sie die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Klasse verwenden.  
  
2.  Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.TransPullSubscription> -Klasse, und legen Sie die Eigenschaften <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>und <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> fest: Verwenden Sie die Verbindung zum Abonnenten aus Schritt 1, um die <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> -Eigenschaft festzulegen.  
  
3.  Überprüfen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> -Eigenschaft, um festzustellen, ob das Abonnement vorhanden ist. Wenn der Wert dieser Eigenschaft **false** ist, wurden entweder die Abonnementeigenschaften in Schritt 2 falsch definiert, oder das Abonnement ist nicht vorhanden.  
  
4.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.PullSubscription.Remove%2A> -Methode auf.  
  
5.  Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.TransPublication> -Klasse, indem Sie die Verlegerverbindung aus Schritt 1 verwenden. Geben Sie <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> und <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>an.  
  
6.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> -Methode auf. Wenn diese Methode **false** zurückgibt, sind entweder die in Schritt 5 angegebenen Eigenschaften falsch definiert, oder die Veröffentlichung ist auf dem Server nicht vorhanden.  
  
7.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.TransPublication.RemovePullSubscription%2A> -Methode auf. Geben Sie den Namen des Abonnenten und der Abonnementdatenbank für die Parameter *subscriber* und *subscriberDB* an.  
  
#### <a name="to-delete-a-pull-subscription-to-a-merge-publication"></a>So löschen Sie ein Pullabonnement für eine Mergeveröffentlichung  
  
1.  Stellen Sie die Verbindung zum Abonnenten und zum Verleger her, indem Sie die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Klasse verwenden.  
  
2.  Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.MergePullSubscription> -Klasse, und legen Sie die Eigenschaften <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>und <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> fest: Verwenden Sie die Verbindung aus Schritt 1, um die <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> -Eigenschaft festzulegen.  
  
3.  Überprüfen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> -Eigenschaft, um festzustellen, ob das Abonnement vorhanden ist. Wenn der Wert dieser Eigenschaft **false** ist, wurden entweder die Abonnementeigenschaften in Schritt 2 falsch definiert, oder das Abonnement ist nicht vorhanden.  
  
4.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.PullSubscription.Remove%2A> -Methode auf.  
  
5.  Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.MergePublication> -Klasse, indem Sie die Verlegerverbindung aus Schritt 1 verwenden. Geben Sie <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> und <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>an.  
  
6.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> -Methode auf. Wenn diese Methode **false** zurückgibt, sind entweder die in Schritt 5 angegebenen Eigenschaften falsch definiert, oder die Veröffentlichung ist auf dem Server nicht vorhanden.  
  
7.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.MergePublication.RemovePullSubscription%2A> -Methode auf. Geben Sie den Namen des Abonnenten und der Abonnementdatenbank für die Parameter *subscriber* und *subscriberDB* an.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Beispiele (RMO)  
 In diesem Beispiel wird ein Pullabonnement für eine Transaktionsveröffentlichung gelöscht und die Registrierung des Abonnements auf dem Verleger entfernt.  
  
 [!code-cs[HowTo#rmo_DropTranPullSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_droptranpullsub)]  
  
 [!code-vb[HowTo#rmo_vb_DropTranPullSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_droptranpullsub)]  
  
 In diesem Beispiel wird ein Pullabonnement für eine Mergeveröffentlichung gelöscht und die Registrierung des Abonnements auf dem Verleger entfernt.  
  
 [!code-cs[HowTo#rmo_DropMergePullSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropmergepullsub)]  
  
 [!code-vb[HowTo#rmo_vb_DropMergePullSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropmergepullsub)]  
  
## <a name="see-also"></a>Weitere Informationen  
 [Abonnieren von Veröffentlichungen](../../relational-databases/replication/subscribe-to-publications.md)   
 [Bewährte Methoden für die Replikationssicherheit](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
