---
title: Deaktivieren der Veröffentlichung und Verteilung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Veröffentlichung und Verteilung in SQL Server mithilfe von SQL Server Management Studio, Transact-SQL oder Replikationsverwaltungsobjekten deaktivieren.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- disabling publishing
- publishing [SQL Server replication], disabling
- distribution disabling [SQL Server replication]
- removing replication
- replication [SQL Server], removing
- disabling replication
- disabling distribution
ms.assetid: 6d4a1474-4d13-4826-8be2-80050fafa8a5
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: c5ac700e65cc251f43afecaa910c3af67fcbdbdd
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076482"
---
# <a name="disable-publishing-and-distribution"></a>Deaktivieren der Veröffentlichung und Verteilung
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  In diesem Thema wird beschrieben, wie die Veröffentlichung und die Verteilung in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]oder Replikationsverwaltungsobjekten (RMO) deaktiviert werden.  
  
 Sie können folgendermaßen vorgehen:  
  
-   Löschen aller Verteilungsdatenbanken auf dem Verteiler.  
  
-   Deaktivieren aller Verleger, die den Verteiler verwenden, und Löschen aller Veröffentlichungen auf diesen Verlegern.  
  
-   Löschen aller Abonnements für die Veröffentlichungen. Daten in den Veröffentlichungs- und Abonnementdatenbanken werden nicht gelöscht. Allerdings geht die Synchronisierungsbeziehung zu allen Veröffentlichungsdatenbanken verloren. Falls Sie die Daten auf dem Abonnenten löschen möchten, müssen Sie diese manuell löschen.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Voraussetzungen](#Prerequisites)  
  
-   **So deaktivieren Sie die Veröffentlichung und Verteilung mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replikationsverwaltungsobjekte (RMO)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Zum Deaktivieren der Veröffentlichung und Verteilung müssen sämtliche Verteilungs- und Veröffentlichungsdatenbanken online sein. Wenn für Verteilungs- oder Veröffentlichungsdatenbanken *Datenbankmomentaufnahmen* vorhanden sind, müssen diese gelöscht werden, bevor die Veröffentlichung und Verteilung deaktiviert werden kann. Eine Datenbankenmomentaufnahme ist eine schreibgeschützte Offlinekopie einer Datenbank, die in keinem Bezug zu einer Replikationsmomentaufnahme steht. Weitere Informationen finden Sie unter [Datenbankmomentaufnahmen &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 Sie können die Veröffentlichung und die Verteilung mithilfe des Veröffentlichungs- und Verteilungsdeaktivierungs-Assistenten deaktivieren.  
  
#### <a name="to-disable-publishing-and-distribution"></a>So deaktivieren Sie die Veröffentlichung und Verteilung  
  
1.  Stellen Sie in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] eine Verbindung mit dem Verleger oder Verteiler her, den Sie deaktivieren möchten, und erweitern Sie dann den Serverknoten.  
  
2.  Klicken Sie mit der rechten Maustaste auf den Ordner **Replikation** , und klicken Sie dann auf **Veröffentlichung und Verteilung deaktivieren**.  
  
3.  Führen Sie die Schritte des Veröffentlichungs- und Verteilungsdeaktivierungs-Assistenten vollständig aus.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 Die Veröffentlichung und die Verteilung können mit gespeicherten Replikationsprozeduren programmgesteuert deaktiviert werden.  
  
#### <a name="to-disable-publishing-and-distribution"></a>So deaktivieren Sie die Veröffentlichung und Verteilung  
  
1.  Beenden Sie alle replikationsbezogenen Aufträge. Eine Auflistung der Auftragsnamen finden Sie im Abschnitt "Agentsicherheit mit SQL Server-Agent" unter [Sicherheitsmodell des Replikations-Agents](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
2.  Führen Sie für jeden Abonnenten der Abonnementdatenbank [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) aus, um Replikationsobjekte aus der Datenbank zu entfernen. Diese gespeicherte Prozedur entfernt keine Replikationsaufträge vom Verteiler.  
  
3.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) aus, um Replikationsobjekte aus der Datenbank zu entfernen.  
  
4.  Wenn der Verleger einen Remoteverteiler verwendet, führen Sie [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md)aus.  
  
5.  Führen Sie auf dem Verteiler [sp_dropdistpublisher](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)aus. Diese gespeicherte Prozedur sollte für jeden auf dem Verteiler registrierten Verleger einmal ausgeführt werden.  
  
6.  Führen Sie auf dem Verteiler [sp_dropdistributiondb](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md) aus, um die Verteilerdatenbank zu löschen. Diese gespeicherte Prozedur sollte für jede Verteilungsdatenbank auf dem Verteiler einmal ausgeführt werden. Dadurch werden außerdem alle der Verteilungsdatenbank zugeordnete Warteschlangenlese-Agentaufträge entfernt.  
  
7.  Führen Sie auf dem Verteiler [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md) aus, um die Verteilerbezeichnung vom Server zu entfernen.  
  
    > [!NOTE]  
    > Wenn nicht alle Replikationsveröffentlichungs- und Verteilungsobjekte gelöscht wurden, bevor Sie [sp_dropdistpublisher](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md) und [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md)ausführen, geben diese Prozeduren einen Fehler zurück. Damit alle replikationsbezogenen Objekte gelöscht werden, wenn ein Verleger oder ein Verteiler gelöscht wird, muss der Parameter `@no_checks` auf **1** festgelegt werden. Wenn ein Verleger oder Verteiler offline oder nicht erreichbar ist, kann der Parameter `@ignore_distributor` auf **1** festgelegt werden, damit der Verleger/Verteiler gelöscht werden kann. Alle verbleibenden Veröffentlichungs- und Verteilungsobjekte müssen jedoch manuell entfernt werden.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Beispiele (Transact-SQL)  
 Dieses Beispielskript entfernt Replikationsobjekte aus der Abonnementdatenbank.  
  
 [!code-sql[HowTo#sp_removedbreplication](../../relational-databases/replication/codesnippet/tsql/disable-publishing-and-d_1.sql)]  
  
 Dieses Beispielskript deaktiviert die Veröffentlichung und die Verteilung auf einem Server, der ein Verleger und Verteiler ist, und löscht die Verteilungsdatenbank.  
  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/disable-publishing-and-d_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Verwenden von Replikationsverwaltungsobjekten (RMO)  
  
#### <a name="to-disable-publishing-and-distribution"></a>So deaktivieren Sie die Veröffentlichung und Verteilung  
  
1.  Entfernen Sie alle Abonnements von Veröffentlichungen, für die der Verteiler verwendet wird. Weitere Informationen finden Sie unter [Delete a Pull Subscription](../../relational-databases/replication/delete-a-pull-subscription.md) und [Delete a Push Subscription](../../relational-databases/replication/delete-a-push-subscription.md).  
  
2.  Entfernen Sie alle Veröffentlichungen, für die der Verteiler verwendet wird, und deaktivieren Sie die Veröffentlichung für alle Datenbanken, wenn sich Verleger und Verteiler auf dem gleichen Server befinden. Weitere Informationen finden Sie unter [Delete a Publication](../../relational-databases/replication/publish/delete-a-publication.md).  
  
3.  Erstellen Sie eine Verbindung mit dem Verteiler, indem Sie die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Klasse verwenden.  
  
4.  Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.DistributionPublisher>-Klasse. Geben Sie die <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Name%2A> -Eigenschaft an, und übergeben Sie das <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Objekt aus Schritt 3.  
  
5.  (Optional) Rufen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> -Methode auf, um die Eigenschaften des Objekts abzurufen und zu verifizieren, dass der Verleger vorhanden ist. Wenn diese Methode **false** zurückgibt, war der in Schritt 4 festgelegte Verlegername falsch, oder der Verleger wird von diesem Verteiler nicht verwendet.  
  
6.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Remove%2A> -Methode auf. Übergeben Sie den Wert **true** für *force* , wenn sich Verleger und Verteiler auf verschiedenen Servern befinden, und wenn der Verleger auf dem Verteiler deinstalliert werden soll, ohne dass zuvor überprüft wurde, ob die Veröffentlichungen auf dem Verleger gelöscht wurden.  
  
7.  Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.ReplicationServer>-Klasse. Übergeben Sie das <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Objekt aus Schritt 3.  
  
8.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationServer.UninstallDistributor%2A> -Methode auf. Übergeben Sie den Wert **true** für *force* , um alle Replikationsobjekte auf dem Verteiler zu löschen, ohne zuvor zu überprüfen, ob die lokalen Veröffentlichungsdatenbanken deaktiviert und die Verteilerdatenbanken deinstalliert wurden.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Beispiele (RMO)  
 In diesem Beispiel werden die Verlegerregistrierung auf dem Verteiler entfernt, die Verteilungsdatenbank gelöscht und der Verteiler deinstalliert.  
  
 [!code-cs[HowTo#rmo_DropDistPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropdistpub)]  
  
 [!code-vb[HowTo#rmo_vb_DropDistPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropdistpub)]  
  
 In diesem Beispiel wird der Verteiler deinstalliert, ohne dass zuvor die lokalen Veröffentlichungsdatenbanken deaktiviert oder die Verteilungsdatenbank gelöscht wurde.  
  
 [!code-cs[HowTo#rmo_DropDistPubForce](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropdistpubforce)]  
  
 [!code-vb[HowTo#rmo_vb_DropDistPubForce](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropdistpubforce)]  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
  
  
