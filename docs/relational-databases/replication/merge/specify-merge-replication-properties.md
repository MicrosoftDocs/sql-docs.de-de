---
description: Angeben von Mergereplikationseigenschaften
title: Angeben von Mergereplikationseigenschaften | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- merge replication [SQL Server replication], download-only articles
- articles [SQL Server replication], download-only
- download-only articles
ms.assetid: 14839cec-6dbf-49c2-aa27-56847b09b4db
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f53a43fa102505121cd176eca6c94d4697a25372
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336188"
---
# <a name="specify-merge-replication-properties"></a>Angeben von Mergereplikationseigenschaften
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
In diesem Artikel wird beschrieben, wie Sie verschiedene Eigenschaften für Ihre Mergereplikationen angeben. 

## <a name="merge-article-is-download-only"></a>Der Artikel zu Mergereplikationen ist nur herunterladbar
Nur herunterladbare Artikel sind für Anwendungen vorgesehen, deren Daten nicht auf den Abonnenten aktualisiert werden. Weitere Informationen finden Sie unter [Optimieren der Leistung der Mergereplikation durch nur herunterladbare Artikel](../../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).  
   
###  <a name="considerations"></a>Überlegungen   
-   Wenn Sie nach der Initialisierung von Abonnements angeben, dass ein Artikel nur herunterladbar sein soll, müssen sämtliche Clientabonnements, die den Artikel erhalten, erneut initialisiert werden. Serverabonnements müssen nicht erneut initialisiert werden. Weitere Informationen über die Auswirkungen von Eigenschaftsänderungen finden Sie unter [Ändern von Veröffentlichungs- und Artikeleigenschaften](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
### <a name="use-sql-server-management-studio"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="on-the-articles-page"></a>Auf der Seite „Artikel“
Wählen Sie im Assistenten für neue Veröffentlichung auf der Seite **Artikel** eine Tabelle aus, und aktivieren Sie dann das Kontrollkästchen **Die durch die Hervorhebung markierte Tabelle ist nur herunterladbar**.  
  
#### <a name="on-the-properties-tab-of-the-article-properties"></a>Auf der Registerkarte „Eigenschaften“ der Artikeleigenschaften  
  
1.  Wählen Sie im Assistenten für neue Veröffentlichung auf der Seite **Artikel** bzw. im Dialogfeld **Veröffentlichungseigenschaften - \<Publication>** eine Tabelle aus, und klicken anschließend auf **Artikeleigenschaften**.    
2.  Klicken Sie auf **Eigenschaften des hervorgehobenen Tabelle-Artikels festlegen** oder **Eigenschaften aller Tabellenartikel festlegen**.  
  
3.  Geben Sie im Abschnitt **Zielobjekt** der Registerkarte **Eigenschaften** des Dialogfelds **Artikeleigenschaften - \<Article>** einen der folgenden Werte für **Synchronisierungsrichtung** an:    
    -   **Nur herunterladbar auf Abonnenten, Abonnentenänderungen nicht zulassen**    
    -   **Nur herunterladbar auf Abonnenten, Abonnentenänderungen zulassen**    
4.  Wenn Sie sich im Dialogfeld **Veröffentlichungseigenschaften - \<Publication>** befinden, klicken Sie auf **OK**, um die Einstellungen zu speichern und das Dialogfeld zu schließen.  

###  <a name="use-transact-sql"></a>Verwenden von Transact-SQL  
  
#### <a name="new-article"></a>Neuer Artikel  
  
1.  Führen Sie [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) aus, und geben Sie dabei den Wert **1** oder **2** für den Parameter `@subscriber_upload_options` an. Die Werte entsprechen dem folgenden Verhalten:  
  
    -   **0** &ndash; Keine Einschränkungen (Standard). Auf dem Abonnenten vorgenommene Änderungen werden auf den Verleger hochgeladen.    
    -   **1** &ndash; Änderungen sind auf dem Abonnenten zulässig, werden aber nicht auf den Verleger hochgeladen.    
    -   **2** &ndash; Änderungen sind auf dem Abonnenten nicht zulässig.  
  
       > [!NOTE]  
       > Wenn die Quelltabelle für einen Artikel bereits in einer anderen Veröffentlichung veröffentlicht wurde, muss der Wert für `@subscriber_upload_options` für beide Artikel identisch sein.  
  
#### <a name="existing-article"></a>Bereits vorhandener Artikel
  
1.  Führen Sie den Befehl [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) aus, und überprüfen Sie den Wert **upload_options** für den Artikel im Resultset, um zu bestimmen, ob ein Artikel nur herunterladbar ist. 
  
2.  Wenn in Schritt 1 der Wert **0** zurückgegeben wird, führen Sie [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) aus, und geben Sie dabei den Wert **subscriber_upload_options** für `@property`, den Wert **1** für `@force_invalidate_snapshot` und `@force_reinit_subscription` sowie den Wert **1** oder **2** für `@value` an, was folgendem Verhalten entspricht:  
  
    -   **1** &ndash; Änderungen sind auf dem Abonnenten zulässig, werden aber nicht auf den Verleger hochgeladen.    
    -   **2** &ndash; Änderungen sind auf dem Abonnenten nicht zulässig.  
  
        > [!NOTE]  
        >  Wenn die Quelltabelle eines Artikels bereits in einer anderen Veröffentlichung veröffentlicht wurde, muss das Verhalten nur herunterladbarer Artikel gleich sein.  
  
## <a name="interactive-conflict-resolution"></a>Interactive Conflict Resolution 
  
 Bei der Microsoft SQL Server-Replikation steht ein interaktiver Konfliktlöser zur Verfügung, mit dem Sie Konflikte bei einer bedarfsgesteuerten Synchronisierung in der Synchronisierungsverwaltung für Microsoft Windows manuell lösen können. Wenn Sie die interaktive Konfliktlösung aktiviert haben, können Sie Konflikte mithilfe des interaktiven Konfliktlösers interaktiv lösen. Der interaktive Konfliktlöser ist über die Synchronisierungsverwaltung für Microsoft Windows verfügbar. Weitere Informationen finden Sie unter [Synchronisieren eines Abonnements mithilfe der Synchronisierungsverwaltung von Windows &#40;Synchronisierungsverwaltung von Windows&#41;](../../../relational-databases/replication/synchronize-a-subscription-using-windows-synchronization-manager.md).  
  
  
### <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
  
-   Wenn die Synchronisierung nicht im Rahmen der Synchronisierungsverwaltung von Windows erfolgt (sondern als geplante Synchronisierung oder als bedarfsgesteuerte Synchronisierung in SQL Server Management Studio oder im Replikationsmonitor), werden Konflikte ohne Benutzereingriff automatisch entsprechend der Standardkonfliktlösung gelöst, die für den Artikel angegeben ist. Weitere Informationen finden Sie unter [Interactive Conflict Resolution](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md).  
  
### <a name="use-sql-server-management-studio"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="enable-interactive-conflict-resolution-for-an-article"></a>Aktivieren der interaktiven Konfliktlösung für einen Artikel  
  
1.  Wählen Sie auf der Seite **Artikel** des Assistenten für neue Veröffentlichung bzw. des Dialogfelds **Veröffentlichungseigenschaften - \<Publication>** eine Tabelle aus. Weitere Informationen zum Verwenden des Assistenten sowie Zugriff auf das Dialogfeld finden Sie unter [Erstellen einer Veröffentlichung](../../../relational-databases/replication/publish/create-a-publication.md) und [Anzeigen und Ändern von Veröffentlichungseigenschaften](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).   
2.  Klicken Sie auf **Artikeleigenschaften** und anschließend auf **Eigenschaften des hervorgehobenen Tabellenartikels festlegen** bzw. **Eigenschaften aller Tabellenartikel festlegen**.    
3.  Klicken Sie auf der Seite **Artikeleigenschaften - \<Article>** oder **Artikeleigenschaften - \<ArticleType>** auf die Registerkarte **Konfliktlöser**.    
4.  Aktivieren Sie die Option **Zulassen, dass der Abonnent Konflikte während bedarfsgesteuerter Synchronisierungen interaktiv löst**.    
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]    
6.  Wenn Sie sich im Dialogfeld **Veröffentlichungseigenschaften - \<Publication>** befinden, klicken Sie auf **OK**, um die Einstellungen zu speichern und das Dialogfeld zu schließen.  
  
#### <a name="specify-that-a-subscription-should-use-interactive-conflict-resolution"></a>Angeben, dass ein Abonnement die interaktive Konfliktlösung verwendet  
  
1.  Geben Sie im Dialogfeld **Abonnementeigenschaften - \<Subscriber>: \<SubscriptionDatabase>** für die Option **Konflikte interaktiv lösen** den Wert **TRUE** an. Weitere Informationen zum Zugreifen auf dieses Dialogfeld finden Sie unter [View and Modify Push Subscription Properties](../../../relational-databases/replication/view-and-modify-push-subscription-properties.md) und [View and Modify Pull Subscription Properties](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).   
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
###  <a name="use-transact-sql"></a>Verwenden von Transact-SQL  
 Sie können programmgesteuert angeben, dass ein Abonnent diese grafische Benutzeroberfläche zur Auflösung von Artikelkonflikten verwendet, wenn ein Pullabonnement für eine Mergeveröffentlichung erstellt wird. Im interaktiven Konfliktlöser werden nur Konflikte in Artikeln, die diese Option unterstützen, angezeigt.  
  
#### <a name="create-a-merge-pull-subscription-that-uses-the-interactive-resolver"></a>Erstellen eines Mergepullabonnements, das den interaktiven Konfliktlöser verwendet  
  
1.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md) aus, und geben Sie dabei `@publication` an. Betrachten Sie den Wert von **allow_interactive_resolver** für jeden Artikel im Resultset, für das der interaktive Konfliktlöser verwendet wird.   
    -   Wenn dieser Wert **1** lautet, wird der Interaktive Konfliktlöser verwendet.    
    -   Wenn dieser Wert **0** lautet, müssen Sie zuerst den interaktiven Konfliktlöser für jeden Artikel aktivieren. Führen Sie hierzu [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) aus, und geben Sie dabei `@publication`, `@article`, den Wert **allow_interactive_resolver** für `@property` sowie den Wert **TRUE** für `@value` an.    
2.  Führen Sie auf dem Abonnenten für die Abonnementdatenbank [sp_addmergepullsubscription](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)aus. Weitere Informationen finden Sie unter [Create a Pull Subscription](../../../relational-databases/replication/create-a-pull-subscription.md).    
3.  Führen Sie auf dem Abonnenten für die Abonnentendatenbank [sp_addmergepullsubscription_agent](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)aus, und geben Sie die folgenden Parameter an:    
    -   `@publisher`, `@publisher_db` (die veröffentlichte Datenbank) und `@publication`.    
    -   Den Wert **TRUE** für `@enabled_for_syncmgr`.    
    -   Den Wert **TRUE** für `@use_interactive_resolver`.    
    -   Die für den Merge-Agent erforderlichen Sicherheitskontoinformationen. Weitere Informationen finden Sie unter [Create a Pull Subscription](../../../relational-databases/replication/create-a-pull-subscription.md).    
4.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_addmergesubscription](../../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)aus.  
  
#### <a name="define-an-article-that-supports-the-interactive-resolver"></a>Definieren eines Artikels, der den interaktiven Konfliktlöser unterstützt  
  
1.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)aus. Geben Sie den Namen der Veröffentlichung, zu der der Artikel gehört, für `@publication`, den Namen des Artikels für `@article`, das Datenbankobjekt, das veröffentlicht wird, für `@source_object` und den Wert **TRUE** für `@allow_interactive_resolver` an. Weitere Informationen finden Sie unter [Definieren eines Artikels](../../../relational-databases/replication/publish/define-an-article.md).  
 
## <a name="conflict-tracking-and-resolution-level-for-merge-articles"></a>Konfliktnachverfolgungs- und -lösungsebene für Mergeartikel
In diesem Thema wird beschrieben, wie die Konfliktnachverfolgung und die Auflösungsebene für Mergeartikel in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../../includes/tsql-md.md)]angegeben werden.  
  
 Wenn ein Abonnement für eine Mergeveröffentlichung synchronisiert wird, prüft die Replikation auf Konflikte, die sich ergeben, wenn der Verleger und der Abonnent die gleichen Daten ändern. Sie können angeben, ob Konflikte auf Zeilenebene erkannt werden, wobei jede Änderung in der Zeile als Konflikt eingestuft wird, oder auf Spaltenebene, wobei nur die Änderungen in derselben Spalte und derselben Zeile als Konflikte eingestuft werden. Die Konfliktlösung für Artikel wird auf Zeilenebene ausgeführt. Weitere Informationen zur Konflikterkennung und -lösung bei logischen Datensätzen finden Sie unter [Detecting and Resolving Conflicts in Logical Records](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-resolving-in-logical-record.md).  
 
### <a name="limitations-and-restrictions"></a>Einschränkungen  
  
-   Wenn Sie die Nachverfolgungsebene nach dem Initialisieren von Abonnements ändern, müssen diese Abonnements erneut initialisiert werden. Weitere Informationen über die Auswirkungen von Eigenschaftsänderungen finden Sie unter [Ändern von Veröffentlichungs- und Artikeleigenschaften](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).   
-   Bei der Nachverfolgung auf Zeilen- oder Spaltenebene erfolgt die Konfliktlösung immer auf Zeilenebene: die gewinnende Zeile überschreibt die verlierende Zeile. Bei der Mergereplikation können Sie auch angeben, dass Konflikte auf der Ebene logischer Datensätze nachverfolgt und gelöst werden. Diese Optionen sind in [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]jedoch nicht verfügbar. Weitere Informationen zum Festlegen dieser Optionen aus gespeicherten Replikations Prozeduren finden [Sie unter Definieren einer logischen Daten Satz Beziehung zwischen Mergetabellenartikeln.](../../../relational-databases/replication/publish/define-a-logical-record-relationship-between-merge-table-articles.md)  
  
### <a name="use-sql-server-management-studio"></a>Verwenden von SQL Server Management Studio  
 Geben Sie die Nachverfolgung auf Zeilen- oder Spaltenebene für Mergeartikel im Dialogfeld **Artikeleigenschaften** auf der Registerkarte **Eigenschaften** an. Dieses Dialogfeld ist im Assistenten für neue Veröffentlichung und im Dialogfeld **Veröffentlichungseigenschaften - \<Publication>** verfügbar. Weitere Informationen zum Verwenden des Assistenten sowie Zugriff auf das Dialogfeld finden Sie unter [Erstellen einer Veröffentlichung](../../../relational-databases/replication/publish/create-a-publication.md) und [Anzeigen und Ändern von Veröffentlichungseigenschaften](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="specify-row--or-column-level-tracking"></a>Angeben der Nachverfolgung auf Zeilen- oder Spaltenebene  
  
1.  Wählen Sie auf der Seite **Artikel** des Assistenten für neue Veröffentlichung bzw. des Dialogfelds **Veröffentlichungseigenschaften - \<Publication>** eine Tabelle aus.  
2.  Klicken Sie auf **Artikeleigenschaften** und anschließend auf **Eigenschaften des hervorgehobenen Tabellenartikels festlegen** bzw. **Eigenschaften aller Tabellenartikel festlegen**.   
3.  Wählen Sie im Dialogfeld **Artikeleigenschaften \<Article>** auf der Registerkarte **Eigenschaften** einen der folgenden Werte für die Eigenschaft **Nachverfolgungsebene** aus: **Nachverfolgung auf Zeilenebene** oder **Nachverfolgung auf Spaltenebene**.   
4.  Wenn Sie sich im Dialogfeld **Veröffentlichungseigenschaften - \<Publication>** befinden, klicken Sie auf **OK**, um die Einstellungen zu speichern und das Dialogfeld zu schließen.  
  
### <a name="use-transact-sql"></a>Verwenden von Transact-SQL  
  
#### <a name="to-specify-conflict-tracking-options-for-a-new-merge-article"></a>So geben Sie Konfliktverfolgungsoptionen für einen neuen Mergeartikel an  
  
1.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) aus, und geben Sie dabei einen der folgenden Werte für `@column_tracking` an:  
  
    -   **true** &ndash; Nachverfolgung auf Spaltenebene für den Artikel verwenden.    
    -   **false** &ndash; Nachverfolgung auf Zeilenebene verwenden (Standard).  
  
#### <a name="change-conflict-tracking-options-for-a-merge-article"></a>Ändern der Konfliktverfolgungsoptionen für einen Mergeartikel  
  
1.  Um die Konfliktverfolgungsoptionen für einen Mergeartikel zu bestimmen, führen Sie [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)aus. Achten Sie auf den Wert der **column_tracking** -Option im Resultset für den Artikel. Der Wert **1** bedeutet, dass die Nachverfolgung auf Spaltenebene verwendet wird, und der Wert **0** bedeutet, dass die Nachverfolgung auf Zeilenebene verwendet wird.    
2.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)aus. Geben Sie den Wert **column_tracking** für `@property` und einen der folgenden Werte für `@value` an:  
  
    -   **true** &ndash; Nachverfolgung auf Spaltenebene für den Artikel verwenden.    
    -   **false** &ndash; Nachverfolgung auf Zeilenebene verwenden (Standard).  
  
     Geben Sie den Wert **1** sowohl für `@force_invalidate_snapshot` als auch für `@force_reinit_subscription` an. 

## <a name="manage-tracking--deletes"></a>Verwalten von Löschvorgängen bei der Nachverfolgung
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
 Standardmäßig synchronisiert die Mergereplikation DELETE-Befehle zwischen dem Verleger und dem Abonnenten. Die Mergereplikation ermöglicht Ihnen, Zeilen in der Abonnementdatenbank auch dann beizubehalten, wenn sie aus der Veröffentlichung gelöscht wurden und umgekehrt. Sie können bei der Erstellung eines neuen Artikels programmgesteuert festlegen, dass der DELETE-Befehl ignoriert wird, oder Sie können diese Funktionalität zu einem späteren Zeitpunkt mithilfe von gespeicherten Replikationsprozeduren aktivieren.  
  
> [!IMPORTANT]  
>  Die Aktivierung dieser Funktionalität führt zu Nichtkonvergenz, was bedeutet, dass die dem Abonnenten verfügbaren Daten nicht genau den Daten des Verlegers entsprechen. Sie müssen einen eigenen Mechanismus implementieren, um die gelöschten Zeilen zu entfernen.  
  
### <a name="specify-that-deletes-be-ignored-for-a-new-merge-article"></a>Angeben, dass Löschvorgänge für neue Mergeartikel ignoriert werden sollen  
  
Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) aus. Geben Sie den Wert **FALSE** für `@delete_tracking` an. Weitere Informationen finden Sie unter [Definieren eines Artikels](../../../relational-databases/replication/publish/define-an-article.md).
  
> [!NOTE]  
>  Wenn die Quelltabelle eines Artikels bereits in einer anderen Veröffentlichung veröffentlicht wurde, muss der Wert von **delete_tracking** für beide Artikel gleich sein.  
  
### <a name="specify-that-deletes-be-ignored-for-an-existing-merge-article"></a>Angeben, dass Löschvorgänge für einen vorhandenen Mergeartikel ignoriert werden sollen  
  
1.  Führen Sie [sp_helpmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) aus, um zu ermitteln, ob die Fehlerkompensierung für einen Artikel aktiviert ist, und achten Sie im Resultset auf den Wert von **delete_tracking**. Ist dieser Wert **0**, werden Löschvorgänge bereits ignoriert.  
  
2.  Ist der in Schritt 1 ermittelte Wert **1** ist, führen Sie [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) auf dem Verleger für die Veröffentlichungsdatenbank aus. Geben Sie den Wert `delete_tracking` für `@property` und den Wert **FALSE** für `@value` an.  
  
    > [!NOTE]  
    >  Wenn die Quelltabelle eines Artikels bereits in einer anderen Veröffentlichung veröffentlicht wurde, muss der Wert von **delete_tracking** für beide Artikel gleich sein.  
  

## <a name="processing-order"></a>Verarbeitungsreihenfolge
  Die Mergereplikation ermöglicht es Ihnen, die Reihenfolge anzugeben, in der Artikel während des Synchronisierungsprozesses vom Merge-Agent verarbeitet werden. Sie können den Artikeln bei ihrer Erstellung mithilfe gespeicherter Replikationsprozeduren programmgesteuert eine Reihenfolge zuweisen. Artikel werden in der Reihenfolge vom niedrigsten zum höchsten Wert verarbeitet. Wenn zwei Artikel denselben Wert haben, werden sie gleichzeitig verarbeitet. 

### <a name="how-processing-order-is-determined"></a>Funktionsweise bei der Festlegung der Verarbeitungsreihenfolge  
 Während der Mergesynchronisierung werden die Artikel standardmäßig entsprechend den Abhängigkeiten zwischen den Objekten geordnet. Zu diesen Abhängigkeiten gehören auch die in den Basistabellen definierten DRI-Einschränkungen (Deklarative referenzielle Integrität). Bei der Verarbeitung werden die Änderungen in einer Tabelle aufgezählt und dann angewendet. Wenn es keine deklarative referenzielle Integrität gibt, zwischen den Tabellenartikeln aber Joinfilter oder logische Datensätze vorhanden sind, werden die Artikel entsprechend der in den Filtern und logischen Datensätzen festgelegten Reihenfolge verarbeitet. Artikel, für die keine Abhängigkeiten per DRI, Joinfiltern, logischen Datensätzen oder anderen Methoden bestehen, werden anhand ihres in der [sysmergearticles &#40;Transact-SQL&#41;](../../../relational-databases/system-tables/sysmergearticles-transact-sql.md)-Systemtabelle verzeichneten Artikelspitznamens verarbeitet.  
  
 Angenommen, es gibt eine Veröffentlichung mit der **SalesOrderHeader** - und der **SalesOrderDetail** -Tabelle mit einer **SalesOrderID** -Primärschlüsselspalte in der **SalesOrderHeader** -Tabelle und einer zugehörigen **SalesOrderID** -Fremdschlüsselspalte in der **SalesOrderDetail** -Tabelle. Bei der Synchronisierung verhindert die Mergereplikation Fremdschlüsselverletzungen, indem alle neuen Zeilen zuerst in **SalesOrderHeader** und erst dann die entsprechenden Zeilen in **SalesOrderDetail** eingefügt werden. Genauso werden erst Zeilen aus der **SalesOrderDetail** -Tabelle gelöscht, bevor die entsprechenden Zeilen auch aus der **SalesOrderHeader**-Tabelle gelöscht werden.  
  
 In einigen Anwendungen wird die referenzielle Integrität durch Datenbanktrigger oder auf Anwendungsebene und nicht per DRI erzwungen. So könnte z. B. die oben beschriebene Veröffentlichung statt DRI in der **SalesOrderDetail** -Tabelle einen INSERT-Trigger besitzen, der sicherstellt, dass die zugehörige Zeile in der **SalesOrderHeader** -Tabelle vorhanden ist, bevor eine Einfügung zugelassen wird. Die **SalesOrderHeader** -Tabelle könnte einen DELETE-Trigger enthalten, der sicherstellt, dass in **SalesOrderDetail** keine zugehörigen Zeilen vorhanden sind, bevor eine Löschung zugelassen wird. Bei der Mergereplikation werden zur Bestimmung der Verarbeitungsreihenfolge der Artikel keine Trigger berücksichtigt, da dieser Replikationstyp das Ergebnis des Triggers nicht vorhersehen kann. Darüber hinaus kann diese Replikation auch keine auf Anwendungsebene definierten Einschränkungen berücksichtigen.  
  
 Wenn die referenzielle Integrität durch Trigger oder auf Anwendungsebene gewahrt wird, müssen Sie die Reihenfolge angeben, in der die Artikel verarbeitet werden sollen. Im Beispiel mit den Triggern würden Sie dann angeben, dass die **SalesOrderHeader** -Tabelle vor der **SalesOrderDetail**-Tabelle zu verarbeiten ist, weil die Artikelreihenfolge auf der Einfügereihenfolge basiert. Bei der Mergereplikation wird die Reihenfolge der Löschungen automatisch umgekehrt. Das Nichtfestlegen einer Artikelverarbeitungsreihenfolge führt nicht zu einem Scheitern der Mergereplikation, weil der Merge-Agent mit der Verarbeitung der Artikel fortfährt, wenn es zu einer Einschränkungsverletzung kommt. Wenn die anderen Artikel verarbeitet sind, versucht der Agent, alle Operationen, die fehlgeschlagen sind, erneut auszuführen. Durch das Angeben einer Artikelverarbeitungsreihenfolge werden lediglich solche Neuversuche und die damit verbundene zusätzliche Verarbeitung verhindert. Wenn Sie eine falsche Reihenfolge angeben (z. B. eine, bei der die Detaildatensätze vor den Headerdatensätzen verarbeitet werden), versucht die Mergereplikation die Verarbeitung so lange, bis sie gelingt.   
  
### <a name="new-article"></a>Neuer Artikel
  
1.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) aus. Geben Sie für `@processing_order` einen ganzzahligen Wert an, der die Verarbeitungsreihenfolge für den Artikel darstellt. Weitere Informationen finden Sie unter [Definieren eines Artikels](../../../relational-databases/replication/publish/define-an-article.md).  
  
    > [!NOTE]  
    >  Wenn Sie sortierte Artikel erstellen, sollten Sie Lücken zwischen den Werten für die Artikelreihenfolge lassen. Dadurch wird das Festlegen neuer Wert zu einem späteren Zeitpunkt erleichtert. Wenn Sie beispielsweise für drei Artikel eine bestimmte Verarbeitungsreihenfolge angeben möchten, legen Sie den Wert für `@processing_order` auf 10, 20 und 30 anstatt auf 1, 2 und 3 fest.  
  
### <a name="existing-article"></a>Bereits vorhandener Artikel
  
1.  Um die Verarbeitungsreihenfolge eines Artikels zu ermitteln, führen Sie [sp_helpmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) aus, und betrachten den Wert von **processing_order** im Resultset.   
2.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) aus. Geben Sie für `@property` den Wert **processing_order** und für `@value` einen ganzzahligen Wert an, der die Verarbeitungsreihenfolge darstellt.  


## <a name="see-also"></a>Weitere Informationen  
 [Optimieren der Leistung der Mergereplikation durch nur herunterladbare Artikel](../../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md)   
 [Define an Article](../../../relational-databases/replication/publish/define-an-article.md)   
 [Anzeigen und Ändern von Artikeleigenschaften](../../../relational-databases/replication/publish/view-and-modify-article-properties.md)  
  
  
