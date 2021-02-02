---
title: Wechsel zwischen Modi (aktualisierbare Transaktion)
description: Hier wird beschrieben, wie Sie in SQL Server Management Studio (SSMS) oder Transact-SQL (T-SQL) zwischen den Updatemodi für eine aktualisierbare Transaktionsveröffentlichung wechseln.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, updatable subscriptions
- updatable subscriptions, update modes
- subscriptions [SQL Server replication], updatable
ms.assetid: ab5ebab1-7ee4-41f4-999b-b4f0c420c921
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 50c586c6f8dc2b6bc4f9a4b41f99f8403ce84b71
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076848"
---
# <a name="switch-between-update-modes-for-an-updatable-transactional-subscription"></a>Umschalten zwischen Updatemodi für ein aktualisierbares Transaktionsabonnement
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie zwischen Updatemodi für ein aktualisierbares Transaktionsabonnement in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../../includes/tsql-md.md)]umgeschaltet wird. Geben Sie im Assistenten für neue Abonnements den Modus für aktualisierbare Abonnements an. Weitere Informationen zum Festlegen des Modus bei der Verwendung dieses Assistenten finden Sie unter [Anzeigen und Ändern der Eigenschaften von Pullabonnements](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
     [Empfehlungen](#Recommendations)  
  
-   **So schalten Sie zwischen Updatemodi für ein aktualisierbares Transaktionsabonnement um mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Sie können jederzeit ein Failover vom sofortigen Aktualisieren zum verzögerten Aktualisieren ausführen. Danach können Sie erst wieder zum sofortigen Aktualisieren wechseln, wenn der Abonnent und der Verleger verbunden sind und der Warteschlangenlese-Agent alle ausstehenden Nachrichten in der Warteschlange auf den Verleger angewendet hat.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
  
-   Wenn ein Abonnement mit Aktualisierung mit einer Transaktionsveröffentlichung ein Failover von einem Aktualisierungsmodus zu einem anderen unterstützt, können Sie programmgesteuert den Aktualisierungsmodus wechseln, um Situationen zu bewältigen, in denen sich die Verbindung für eine kurze Zeitdauer ändert. Der Updatemodus kann mithilfe gespeicherter Replikationsprozeduren programm- und bedarfsgesteuert festgelegt werden. Weitere Informationen finden Sie unter [Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
> [!NOTE]  
>  Damit der Updatemodus nach dem Erstellen des Abonnements geändert werden kann, muss beim Erstellen des Abonnements die **update_mode** -Eigenschaft auf **failover** (ermöglicht das Umschalten vom sofortigen Update auf das verzögerte Update) oder auf **queued failover** (ermöglicht das Umschalten vom verzögerten Update auf das sofortige Update) festgelegt werden. Diese Eigenschaften werden im Assistenten für neue Abonnements automatisch festgelegt.  
  
#### <a name="to-set-the-updating-mode-for-a-push-subscription"></a>So legen Sie den Update für ein Pushabonnement fest  
  
1.  Stellen Sie in [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]eine Verbindung mit dem Abonnenten her, und erweitern Sie dann den Serverknoten.  
  
2.  Erweitern Sie den Ordner **Replikation** , und erweitern Sie dann den Ordner **Lokale Abonnements** .  
  
3.  Klicken Sie mit der rechten Maustaste auf das Abonnement, für das Sie den Updatemodus festlegen möchten, und klicken Sie dann auf **Updatemethode festlegen**.  
  
4.  Wählen Sie im Dialogfeld **Updatemethode festlegen - \<Subscriber>: \<SubscriptionDatabase>** die Option **Sofortiges Aktualisieren** oder **Verzögertes Aktualisieren über eine Warteschlange** aus.  
  
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

#### <a name="to-set-the-updating-mode-for-a-pull-subscription"></a>So legen Sie den Updatemodus für ein Pullabonnement fest  
  
1.  Wählen Sie im Dialogfeld **Abonnementeigenschaften - \<Publisher>: \<PublicationDatabase>** für die Option **Updatemethode für Abonnent** den Wert **Änderungen sofort replizieren** oder **Änderungen in Warteschlange einreihen** aus.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
 Weitere Informationen zum Zugreifen auf das Dialogfeld **Abonnementeigenschaften - \<Publisher>: \<PublicationDatabase>** finden Sie unter [Anzeigen und Ändern der Eigenschaften von Pullabonnements](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-switch-between-update-modes"></a>So wechseln Sie den Updatemodus  
  
1.  Stellen Sie sicher, dass die Veröffentlichung das Failover unterstützt, indem Sie bei Pullabonnements [sp_helppullsubscription](../../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md) und bei Pushabonnements [sp_helpsubscription](../../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md) ausführen. Wenn der Wert des **Updatemodus** im Resultset **3** oder **4** ist, wird das Failover unterstützt.  
  
2.  Führen Sie auf dem Abonnenten für die Abonnementdatenbank [sp_setreplfailovermode](../../../relational-databases/system-stored-procedures/sp-setreplfailovermode-transact-sql.md)aus. Geben Sie `@publisher`, `@publisher_db`, `@publication`und einen der folgenden Werte für `@failover_mode`fest:  
  
    -   **queued** - Failover zum verzögerten Aktualisieren, wenn die Verbindung vorübergehend unterbrochen wurde.  
  
    -   **immediate** - Failover zum sofortigen Aktualisieren, wenn die Verbindung wiederhergestellt wurde.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)  
  
  
