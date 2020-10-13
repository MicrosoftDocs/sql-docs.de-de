---
title: Aktivieren aktualisierbarer Abonnements für Transaktionsveröffentlichungen
description: In diesem Artikel erhalten Sie Informationen zum Aktivieren aktualisierbarer Abonnements für Transaktionsveröffentlichungen in SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, updatable subscriptions
- updatable subscriptions, enabling
- subscriptions [SQL Server replication], updatable
ms.assetid: 539d5bb0-b808-4d8c-baf4-cb6d32d2c595
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1c838513ab9f179034f085aa251db3213d894e07
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91869079"
---
# <a name="enable-updating-subscriptions-for-transactional-publications"></a>Aktivieren des Aktualisierens von Abonnements für Transaktionsveröffentlichungen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie das Aktualisieren von Abonnements für Transaktionsveröffentlichungen in [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] mit [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../../includes/tsql-md.md)]aktiviert wird.  
  
> **HINWEIS!** [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
 Benutzer sollten nach Möglichkeit dazu aufgefordert werden, Anmeldeinformationen zur Laufzeit anzugeben. Wenn Anmeldeinformationen in einer Skriptdatei gespeichert werden müssen, muss die Datei an einem sicheren Ort gespeichert werden, um unberechtigten Zugriff zu vermeiden.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 Aktivieren Sie aktualisierbare Abonnements für Transaktionsveröffentlichungen auf der Seite **Veröffentlichungstyp** des Assistenten für neue Veröffentlichung.  
  
 Um aktualisierbare Abonnements verwenden zu können, müssen auch im Assistenten für neue Abonnements Optionen konfiguriert werden.  
  
#### <a name="to-enable-updating-subscriptions"></a>So aktivieren Sie aktualisierbare Abonnements  
  
1.  Wählen Sie im Assistenten für neue Veröffentlichung auf der Seite **Veröffentlichungstyp** die Option **Transaktionsveröffentlichung mit aktualisierbaren Abonnements**aus.  
  
2.  Geben Sie auf der Seite **Agentsicherheit** neben den Sicherheitseinstellungen für den Momentaufnahme-Agent und den Protokolllese-Agent auch die Sicherheitseinstellungen für den Warteschlangenlese-Agent an. Weitere Informationen zu den Berechtigungen, die für das Konto erforderlich sind, unter dem der Warteschlangenlese-Agent ausgeführt wird, finden Sie unter [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md).  

    > **HINWEIS:** Der Warteschlangenlese-Agent wird auch dann konfiguriert, wenn Sie lediglich Abonnements mit sofortigem Update verwenden.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 Wenn Sie mithilfe von gespeicherten Replikationsprozeduren programmgesteuert eine Transaktionsveröffentlichung erstellen, können Sie das sofortige oder das verzögerte Aktualisieren der Abonnements aktivieren.  
  
#### <a name="to-create-a-publication-that-supports-immediate-updating-subscriptions"></a>So erstellen Sie eine Veröffentlichung, die Abonnements mit sofortigem Update unterstützt  
  
1.  Erstellen Sie, falls notwendig, einen Protokolllese-Agentauftrag für die Veröffentlichungsdatenbank.  
  
    -   Wenn bereits ein Protokolllese-Agentauftrag für die Veröffentlichungsdatenbank vorhanden ist, fahren Sie mit Schritt 2 fort.  
  
    -   Wenn Sie sich nicht sicher sind, ob ein Protokolllese-Agentauftrag für die Veröffentlichungsdatenbank vorhanden ist, dann führen Sie [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) auf dem Verleger für die Veröffentlichungsdatenbank aus. Wenn das Resultset leer ist, muss ein Protokolllese-Agentauftrag erstellt werden.  
  
    -   Führen Sie auf dem Verleger [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md) aus. Geben Sie für **\@job_name** und **\@password** die [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows-Anmeldeinformationen an, unter denen der Agent ausgeführt wird. Wenn der Agent zum Herstellen der Verbindung mit dem Verleger die SQL Server-Authentifizierung verwendet, müssen Sie zudem für **\@publisher_security_mode** den Wert **0** und für **\@publisher_login** und **\@publisher_password** die [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Anmeldeinformationen angeben.  
  
2.  Führen Sie [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) aus. Geben Sie dabei für den Parameter **\@allow_sync_tran** den Wert **true** an.  
  
3.  Führen Sie auf dem Verleger [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md) aus. Geben Sie den in Schritt 2 für **\@publication** verwendeten Veröffentlichungsnamen und für **\@job_name** und **\@password** die Windows-Anmeldeinformationen an, unter denen der Momentaufnahmen-Agent ausgeführt wird. Wenn der Agent zum Herstellen der Verbindung mit dem Verleger die SQL Server-Authentifizierung verwendet, müssen Sie zudem für **\@publisher_security_mode** den Wert **0** und für **\@publisher_login** und **\@publisher_password** die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Anmeldeinformationen angeben. Dadurch wird ein Auftrag des Momentaufnahme-Agents für die Veröffentlichung erstellt.  
  
4.  Fügen Sie der Veröffentlichung Artikel hinzu. Weitere Informationen finden Sie unter [Definieren eines Artikels](../../../relational-databases/replication/publish/define-an-article.md).  
  
5.  Erstellen Sie auf dem Abonnenten ein Abonnement mit Update für diese Veröffentlichung.   
  
#### <a name="to-create-a-publication-that-supports-queued-updating-subscriptions"></a>So erstellen Sie eine Veröffentlichung, die Abonnements mit verzögertem Update über eine Warteschlange unterstützt  
  
1.  Erstellen Sie, falls notwendig, einen Protokolllese-Agentauftrag für die Veröffentlichungsdatenbank.  
  
    -   Wenn bereits ein Protokolllese-Agentauftrag für die Veröffentlichungsdatenbank vorhanden ist, fahren Sie mit Schritt 2 fort.  
  
    -   Wenn Sie sich nicht sicher sind, ob ein Protokolllese-Agentauftrag für die Veröffentlichungsdatenbank vorhanden ist, dann führen Sie [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) auf dem Verleger für die Veröffentlichungsdatenbank aus. Wenn das Resultset leer ist, muss ein Protokolllese-Agentauftrag erstellt werden.  
  
    -   Führen Sie auf dem Verleger [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md) aus. Geben Sie für **\@job_name** und **\@password** die Windows-Anmeldeinformationen an, unter denen der Agent ausgeführt wird. Wenn der Agent zum Herstellen der Verbindung mit dem Verleger die SQL Server-Authentifizierung verwendet, müssen Sie zudem für **\@publisher_security_mode** den Wert **0** und für **\@publisher_login** und **\@publisher_password** die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Anmeldeinformationen angeben.  
  
2.  Erstellen Sie, falls notwendig, einen Warteschlangenlese-Agentauftrag für den Verteiler.  
  
    -   Wenn bereits ein Warteschlangenlese-Agentauftrag für die Verteilungsdatenbank vorhanden ist, fahren Sie mit Schritt 3 fort.  
  
    -   Wenn Sie sich nicht sicher sind, ob ein Warteschlangenlese-Agentauftrag für die Verteilungsdatenbank vorhanden ist, dann führen Sie [sp_helpqreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpqreader-agent-transact-sql.md) auf dem Verteiler für die Verteilungsdatenbank aus. Wenn das Resultset leer ist, muss ein Warteschlangenlese-Agentauftrag erstellt werden.  
  
    -   Führen Sie auf dem Verteiler [sp_addqreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md) aus. Geben Sie für **\@job_name** und **\@password** die Windows-Anmeldeinformationen an, unter denen der Agent ausgeführt wird. Diese Anmeldeinformationen werden verwendet, wenn der Warteschlangenlese-Agent eine Verbindung mit dem Verleger und dem Abonnenten herstellt. Weitere Informationen finden Sie unter [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
3.  Führen Sie [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) aus. Geben Sie dabei den Wert **true** für den Parameter **\@allow_queued_tran** und einen der Werte **pub wins**, **sub reinit** oder **sub wins** für **\@conflict_policy** an.  
  
4.  Führen Sie auf dem Verleger [sp_addpublication_snapshot (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md) aus. Geben Sie für **\@publication** den in Schritt 3 verwendeten Veröffentlichungsnamen und für **\@snapshot_job_name** und **\@password** die Windows-Anmeldeinformationen an, unter denen der Momentaufnahme-Agent ausgeführt wird. Wenn der Agent zum Herstellen der Verbindung mit dem Verleger die SQL Server-Authentifizierung verwendet, müssen Sie zudem für **\@publisher_security_mode** den Wert **0** und für **\@publisher_login** und **\@publisher_password** die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Anmeldeinformationen angeben. Dadurch wird ein Auftrag des Momentaufnahme-Agents für die Veröffentlichung erstellt.  
  
5.  Fügen Sie der Veröffentlichung Artikel hinzu. Weitere Informationen finden Sie unter [Definieren eines Artikels](../../../relational-databases/replication/publish/define-an-article.md).  
  
6.  Erstellen Sie auf dem Abonnenten ein Abonnement mit Update für diese Veröffentlichung.  
  
#### <a name="to-change-the-conflict-policy-for-a-publication-that-allows-queued-updating-subscriptions"></a>So ändern Sie die Konfliktrichtlinie für eine Veröffentlichung, die Abonnements mit verzögertem Update über eine Warteschlange zulässt  
  
1.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_changepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) aus. Geben Sie den Wert **conflict_policy** für **\@property** sowie für die gewünschte Konfliktrichtlinie einen der Werte **pub wins**, **sub reinit** oder **sub wins** für **\@value** an.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Beispiel (Transact-SQL)  
 In diesem Beispiel wird eine Veröffentlichung erstellt, die sowohl sofortige als auch verzögerte Updates von Pullabonnements unterstützt.  
  
 [!code-sql[HowTo#sp_createtranupdatingpub](../../../relational-databases/replication/codesnippet/tsql/enable-updating-subscrip_1.sql)]  
  
## <a name="see-also"></a>Weitere Informationen  
 [Festlegen der Konfliktlösungsoptionen für verzögerte Updates über eine Warteschlange &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/create-an-updatable-subscription-to-a-transactional-publication.md)   
 [Transaktionsreplikation](../../../relational-databases/replication/transactional/transactional-replication.md)   
 [Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)   
 [Create an Updatable Subscription to a Transactional Publication](create-an-updatable-subscription-to-a-transactional-publication.md)   
 [Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [Verwenden von sqlcmd mit Skriptvariablen](../../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)  
  
