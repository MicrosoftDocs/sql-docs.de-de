---
title: Anzeigen von Informationen und Ausführen von Aufgaben (Replikationsmonitor)
description: In diesem Artikel erfahren Sie, wie Sie in SQL Server Management Studio (SSMS) mithilfe des Replikationsmonitors Informationen anzeigen und verschiedene Aufgaben ausführen.
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- viewing publication information
- publications [SQL Server replication], viewing information
- publications [SQL Server replication], Replication Monitor tasks
ms.assetid: 92e28a07-d6a7-461b-a0b3-bd9bc6afcbe5
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 24eb21c5907e343ec433316fbdceebfc3864cedc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340752"
---
# <a name="view-information-and-perform-tasks-using-replication-monitor"></a>Anzeigen von Informationen und Ausführen von Aufgaben mit dem Replikationsmonitor
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
Im Replikationsmonitor finden Sie mehrere Registerkarten und Optionen, mit denen Sie Informationen anzeigen und verschiedene Aufgaben ausführen können. In diesem Artikel werden unterschiedliche Informationen und Aufgaben beschrieben, die Sie mithilfe des Replikationsmonitor aufrufen bzw. ausführen können. 


## <a name="for-a-publication"></a>Veröffentlichungen

### <a name="view-information"></a>Anzeigen von Informationen
Der Replikationsmonitor stellt die folgenden Registerkarten mit Informationen zur ausgewählten Veröffentlichung bereit:  
  
-   **Alle Abonnements:** Auf dieser Registerkarte werden Informationen zu allen Abonnements für die ausgewählte Veröffentlichung angezeigt.  
  
-   **Agents:** Auf dieser Registerkarte werden Informationen zu allen Agents angezeigt, die von einer Veröffentlichung verwendet werden:   
    -   Momentaufnahme-Agent, der von allen Veröffentlichungen verwendet wird.   
    -   Protokolllese-Agent, der von allen Transaktionsveröffentlichungen verwendet wird.   
    -   Dem Warteschlangenlese-Agent, der von Transaktionsveröffentlichungen verwendet wird, die Abonnements mit verzögertem Update über eine Warteschlange aufweisen.  
  
-   **Warnungen:** Auf dieser Registerkarte können Sie Warnungen und Benachrichtigungen für Agents angeben.  
  
-   **Überwachungstoken** (nur bei Transaktionsreplikationen): Auf dieser Registerkarte können Sie die Latenz messen, also die Zeitspanne zwischen dem Ausführen des Commit für eine Transaktion auf dem Verleger und dem Ausführen des Commit für die entsprechende Aktion auf dem Abonnenten.  
  
 Weitere Informationen zu den Optionen der einzelnen Registerkarten finden Sie, wenn Sie auf die Registerkarte im rechten Bereich und dann auf **Hilfe** auf der Menüleiste klicken. Informationen zum Starten des Replikationsmonitors finden Sie unter [Starten des Replikationsmonitors](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Ausführen von Aufgaben
  
1.  Erweitern Sie im linken Bereich eine Verlegergruppe, erweitern Sie einen Verleger, und klicken Sie dann auf eine Veröffentlichung.   
2.  Klicken Sie zum Anzeigen und Ändern von Veröffentlichungseigenschaften mit der rechten Maustaste auf die Veröffentlichung anschließend auf **Eigenschaften**.    
3.  Wenn Sie Informationen zu Abonnements aufrufen möchten, klicken Sie auf die Registerkarte **Alle Abonnements** und anschließend mit der rechten Maustaste auf das Abonnement. Klicken Sie dann auf **Eigenschaften**. Sie können auf dieser Registerkarte auch auf detailliertere Informationen zugreifen und Aufgaben ausführen. 
4.  Klicken Sie auf die Registerkarte **Agents**, um Informationen zu Agents anzuzeigen. Sie können auf dieser Registerkarte auch auf detailliertere Informationen zugreifen und Aufgaben ausführen. 
5.  Klicken Sie auf die Registerkarte **Warnungen**, um Informationen zu Agent-Warnungen und Schwellenwerten anzuzeigen. Weitere Informationen finden Sie unter [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md).  
6.  Klicken Sie auf die Registerkarte **Überwachungstoken**, um Informationen zu Überwachungstoken anzuzeigen. Weitere Informationen zu Überwachungstoken finden Sie unter [Messen der Latenzzeit und Überprüfen der Verbindungen bei Transaktionsreplikationen](../../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md).  
  
## <a name="for-a-publisher"></a>Verleger 

### <a name="view-information"></a>Anzeigen von Informationen
Der Replikationsmonitor bietet folgende Registerkarten, auf denen Informationen zu dem ausgewählten Verleger angezeigt werden:   
-   **Veröffentlichungen:** Auf dieser Registerkarte werden Informationen zu allen Veröffentlichungen auf dem ausgewählten Verleger angezeigt.   
-   **Überwachungsliste für Abonnements:** Auf dieser Registerkarte werden Informationen zu Abonnements von allen auf dem ausgewählten Verleger vorhandenen Veröffentlichungen angezeigt, für die Fehlermeldungen oder Warnungen ausgegeben wurden bzw. bei denen Leistungsprobleme aufgetreten sind. Für Verteiler, auf denen Versionen vor [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] ausgeführt werden, wird diese Registerkarte nicht angezeigt.    
-   **Agents:** Auf dieser Registerkarte werden ausführliche Informationen zu den Agents und Aufträgen angezeigt, die von allen Replikationstypen verwendet werden. Über diese Registerkarte können Sie zudem alle Agents und Aufträge starten und beenden. Wenn Sie weitere Informationen zu den Optionen auf den einzelnen Registerkarten anzeigen möchten, klicken Sie im rechten Bereich auf die Registerkarte, und klicken Sie anschließend auf der Menüleiste auf **Hilfe** . Informationen zum Starten des Replikationsmonitors finden Sie unter [Starten des Replikationsmonitors](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Ausführen von Aufgaben
  
1.  Erweitern Sie im linken Bereich eine Verlegergruppe, und klicken Sie dann auf einen Verleger.   
2.  Um Informationen zu allen Veröffentlichungen anzuzeigen, klicken Sie auf die Registerkarte **Veröffentlichungen** .    
3.  Um Informationen zu Abonnements anzuzeigen, klicken Sie auf die Registerkarte **Überwachungsliste für Abonnements** . Über diese Registerkarte können Sie auch auf ausführliche Informationen zugreifen und Aufgaben ausführen:   
    -   Wenn Sie detaillierte Informationen zum Agent angezeigt bekommen möchten, der dem jeweiligen Abonnement zugeordnet ist, klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Details anzeigen**.    
    -   Wenn Sie die Eigenschaften eines Abonnements anzeigen möchten, klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie anschließend auf **Eigenschaften**.    
    -   Wenn Sie ein Pushabonnement synchronisieren möchten, klicken Sie mit der rechten Maustaste auf das betreffende Abonnement, und klicken Sie dann auf **Synchronisierung starten**.   
    -   Wenn Sie ein Abonnement erneut initialisieren möchten, klicken Sie mit der rechten Maustaste auf das betreffende Abonnement, und klicken Sie dann auf **Abonnement erneut initialisieren**.   
4.  Um Informationen zu Agents anzuzeigen, klicken Sie auf die Registerkarte **Agents** . Sie können von dieser Registerkarte aus auch auf detailliertere Informationen zugreifen und Aufgaben ausführen:    
    -   Wenn Sie ausführliche Informationen zu einem Agent (z. B. Informationsmeldungen und Fehlermeldungen) anzeigen möchten, klicken Sie mit der rechten Maustaste auf den Agent, und klicken Sie dann auf **Details anzeigen**.  
    -   Wenn Sie detaillierte Informationen zu dem Auftrag anzeigen möchten, durch den der Agent ausgeführt wird (z. B. Zeitplan, Details zu den Auftragsschritten usw.), klicken Sie mit der rechten Maustaste auf den Agent, und klicken Sie anschließend auf **Eigenschaften**.    
    -   Wenn Sie die Profile für den Agent verwalten möchten, klicken Sie mit der rechten Maustaste auf den Agent, und klicken Sie dann auf **Agentprofil**. Weitere Informationen finden Sie unter [Arbeiten mit Replikations-Agent-Profilen](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).    
    -   Um einen Agent zu starten, der nicht ausgeführt wird, klicken Sie mit der rechten Maustaste auf den Agent, und klicken Sie dann auf **Agent starten**.  
    -   Um einen Agent zu beenden, der ausgeführt wird, klicken Sie mit der rechten Maustaste auf den Agent, und klicken Sie dann auf **Agent beenden**.  


## <a name="for-a-subscription"></a>Abonnements 

### <a name="view-information"></a>Anzeigen von Informationen
  Der Replikationsmonitor umfasst folgende Registerkarten mit Informationen zu Abonnements:    
-   **Alle Abonnements:** Auf dieser Registerkarte werden Informationen zu allen Abonnements für die ausgewählte Veröffentlichung angezeigt.   
-   **Überwachungsliste für Abonnements:** Auf dieser Registerkarte werden Informationen zu Abonnements von allen auf dem ausgewählten Verleger vorhandenen Veröffentlichungen angezeigt, für die Fehlermeldungen oder Warnungen ausgegeben wurden bzw. bei denen Leistungsprobleme aufgetreten sind. Für Verteiler, auf denen Versionen vor [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] ausgeführt werden, wird diese Registerkarte nicht angezeigt. Weitere Informationen zu den Optionen der einzelnen Registerkarten finden Sie, wenn Sie auf die Registerkarte im rechten Bereich und dann auf **Hilfe** auf der Menüleiste klicken. Informationen zum Starten des Replikationsmonitors finden Sie unter [Starten des Replikationsmonitors](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Ausführen von Aufgaben
  
1.  Erweitern Sie im linken Bereich eine Verlegergruppe, erweitern Sie einen Verleger, und klicken Sie dann auf eine Veröffentlichung.   
2.  Um Informationen zu Abonnements anzuzeigen, klicken Sie auf die Registerkarte **Alle Abonnements** . Um nur die Abonnements anzuzeigen, die sich in einem bestimmten Status befinden, wie z. B. diejenigen, die gerade synchronisiert werden, wählen Sie die entsprechende Option aus der Dropdownliste **Anzeigen** aus.    
3.  Zum Anzeigen und Ändern von Abonnementeigenschaften klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Eigenschaften**. Sie können auf dieser Registerkarte auch auf detailliertere Informationen zugreifen und Aufgaben ausführen. 
  
### <a name="to-view-information-and-perform-tasks-for-subscriptions-in-the-subscription-watch-list-tab"></a>So zeigen Sie Informationen für Abonnements in der Registerkarte "Überwachungsliste für Abonnements" an und führen Aufgaben aus  
  
1.  Erweitern Sie im linken Bereich eine Verlegergruppe, und klicken Sie dann auf einen Verleger.    
2.  Um Informationen zu Abonnements anzuzeigen, klicken Sie auf die Registerkarte **Überwachungsliste für Abonnements** .    
3.  Wählen Sie den anzuzeigenden Abonnementtyp aus der Dropdownliste **\<SubscriptionType>-Abonnements anzeigen** aus. Um nur die Abonnements anzuzeigen, die sich in einem bestimmten Status befinden, wie z. B. diejenigen, die gerade synchronisiert werden, wählen Sie die entsprechende Option aus der Dropdownliste **Anzeigen** aus.    
4.  Zum Anzeigen und Ändern von Abonnementeigenschaften klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Eigenschaften**. Sie können auf dieser Registerkarte auch auf detailliertere Informationen zugreifen und Aufgaben ausführen. 
  
  
## <a name="for-publication-agents"></a>Veröffentlichungs-Agents
Der Replikationsmonitor stellt die Registerkarte **Agents** bereit, auf der Informationen zu den Agents enthalten sind, die der ausgewählten Veröffentlichung zugeordnet sind. Der Verteilungs-Agent und der Merge-Agent sind Abonnements zugeordnet. 
  
### <a name="view-information"></a>Anzeigen von Informationen
 Diese Registerkarte enthält Informationen zu folgenden Agents:  
  
-   Momentaufnahme-Agent, der von allen Veröffentlichungen verwendet wird.  
-   Protokolllese-Agent, der von allen Transaktionsveröffentlichungen verwendet wird.  
-   Warteschlangenlese-Agent, der von Transaktionsveröffentlichungen verwendet wird, die für Abonnements mit verzögertem Update über eine Warteschlange aktiviert wurden. 
  
 Wenn Sie weitere Informationen zu den Optionen dieser Registerkarte erhalten möchten, klicken Sie auf der Menüleiste auf **Hilfe** . Informationen zum Starten des Replikationsmonitors finden Sie unter [Starten des Replikationsmonitors](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="to-view-information-and-perform-tasks-for-the-agents-associated-with-a-publication"></a>So zeigen Sie Informationen für die einer Veröffentlichung zugeordneten Agents an und führen Aufgaben aus  
  
1.  Erweitern Sie im linken Bereich eine Verlegergruppe, erweitern Sie einen Verleger, und klicken Sie dann auf eine Veröffentlichung.    
2.  Klicken Sie auf die Registerkarte **Agents** , um Informationen zu Agents anzuzeigen. Sie können von dieser Registerkarte aus auch auf detailliertere Informationen zugreifen und Aufgaben ausführen:    
    -   Wenn Sie ausführliche Informationen zu einem Agent (z. B. Informationsmeldungen und Fehlermeldungen) anzeigen möchten, klicken Sie mit der rechten Maustaste auf den Agent, und klicken Sie dann auf **Details anzeigen**.  
    -   Wenn Sie detaillierte Informationen zu dem Auftrag anzeigen möchten, durch den der Agent ausgeführt wird (z. B. Zeitplan, Details zu den Auftragsschritten usw.), klicken Sie mit der rechten Maustaste auf den Agent, und klicken Sie anschließend auf **Eigenschaften**.    
    -   Wenn Sie die Profile für den Agent verwalten möchten, klicken Sie mit der rechten Maustaste auf den Agent, und klicken Sie dann auf **Agentprofil**. Weitere Informationen finden Sie unter [Arbeiten mit Replikations-Agent-Profilen](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).   
    -   Um einen Agent zu starten, der nicht ausgeführt wird, klicken Sie mit der rechten Maustaste auf den Agent, und klicken Sie dann auf **Agent starten**.   
    -   Um einen Agent zu beenden, der ausgeführt wird, klicken Sie mit der rechten Maustaste auf den Agent, und klicken Sie dann auf **Agent beenden**.  
  
## <a name="for-subscription-agents"></a>Abonnement-Agents

### <a name="view-information"></a>Anzeigen von Informationen
-   **Alle Abonnements:** Auf dieser Registerkarte werden Informationen zu allen Abonnements für die ausgewählte Veröffentlichung angezeigt.  
  
-   **Überwachungsliste für Abonnements:** Auf dieser Registerkarte werden Informationen zu Abonnements von allen auf dem ausgewählten Verleger vorhandenen Veröffentlichungen angezeigt, für die Fehlermeldungen oder Warnungen ausgegeben wurden bzw. bei denen Leistungsprobleme aufgetreten sind. Für Verteiler, auf denen Versionen vor [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] ausgeführt werden, wird diese Registerkarte nicht angezeigt. Weitere Informationen zu den Optionen der einzelnen Registerkarten finden Sie, wenn Sie auf die Registerkarte im rechten Bereich und dann auf **Hilfe** auf der Menüleiste klicken. Informationen zum Starten des Replikationsmonitors finden Sie unter [Starten des Replikationsmonitors](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Ausführen von Aufgaben
  
1.  Erweitern Sie im linken Bereich eine Verlegergruppe, erweitern Sie einen Verleger, und klicken Sie dann auf eine Veröffentlichung.    
2.  Klicken Sie auf die Registerkarte **Alle Abonnements** , um sich Informationen zu den Abonnements anzeigen zu lassen. Sie können von dieser Registerkarte aus auch auf detailliertere Informationen zugreifen und Aufgaben ausführen:   
    -   Wenn Sie detaillierte Informationen zum Agent angezeigt bekommen möchten, der dem jeweiligen Abonnement zugeordnet ist, klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Details anzeigen**. Zu den Detailinformationen gehören: Agentverlauf und Fehlermeldungen, bei der Transaktionsreplikation eine Leistungsstatistik und bei der Mergereplikation eine Synchronisierungsstatistik auf Artikelebene.  
  
         Welche Registerkarten das Detailfenster enthält, hängt von der Art des Abonnements ab: Bei Momentaufnahmeabonnements wird die Registerkarte **Verlauf Verteiler zu Abonnent** angezeigt, bei Transaktionsabonnements enthält das Fenster die Registerkarten **Verlauf Verleger zu Verteiler**, **Verlauf Verteiler zu Abonnent** und **Nicht verteilte Befehle**, und bei Mergeabonnements finden Sie hier die Registerkarte **Synchronisierungsverlauf**.  
  
    -   Wenn Sie ein Pushabonnement synchronisieren möchten, klicken Sie mit der rechten Maustaste auf das betreffende Abonnement, und klicken Sie dann auf **Synchronisierung starten**.    
    -   Wenn Sie ein Abonnement erneut initialisieren möchten, klicken Sie mit der rechten Maustaste auf das betreffende Abonnement, und klicken Sie dann auf **Abonnement erneut initialisieren**.    
    -   Wenn Sie ein einzelnes Mergeabonnement überprüfen möchten, klicken Sie mit der rechten Maustaste auf das betreffende Abonnement, und klicken Sie dann auf **Abonnement überprüfen**. Wenn Sie alle Abonnements für eine Mergeveröffentlichung überprüfen möchten, klicken Sie mit der rechten Maustaste auf die entsprechende Veröffentlichung, und klicken Sie dann auf **Alle Abonnements überprüfen**. Wenn alle Abonnements für eine Transaktionsveröffentlichung überprüft werden sollen, klicken Sie mit der rechten Maustaste auf die betreffende Veröffentlichung, und klicken Sie dann auf **Abonnements überprüfen**.    
    -   Wenn Sie die Profile für den Agent verwalten möchten, klicken Sie mit der rechten Maustaste auf den Agent, und klicken Sie dann auf **Agentprofil**. Weitere Informationen finden Sie unter [Arbeiten mit Replikations-Agent-Profilen](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).  
  
### <a name="subscription-watch-list-tab"></a>Registerkarte „Überwachungsliste für Abonnements“ 
  
1.  Erweitern Sie im linken Bereich eine Verlegergruppe, und klicken Sie dann auf einen Verleger.    
2.  Klicken Sie auf die Registerkarte **Überwachungsliste für Abonnements** , um sich Informationen zu den Abonnements anzeigen zu lassen. Sie können von dieser Registerkarte aus auch auf detailliertere Informationen zugreifen und Aufgaben ausführen:   
    -   Wenn Sie detaillierte Informationen zum Agent angezeigt bekommen möchten, der dem jeweiligen Abonnement zugeordnet ist, klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Details anzeigen**. Zu den Detailinformationen gehören: Agentverlauf und Fehlermeldungen, bei der Transaktionsreplikation eine Leistungsstatistik und bei der Mergereplikation eine Synchronisierungsstatistik auf Artikelebene.    
         Welche Registerkarten das Detailfenster enthält, hängt von der Art des Abonnements ab: Bei Momentaufnahmeabonnements wird die Registerkarte **Verlauf Verteiler zu Abonnent** angezeigt, bei Transaktionsabonnements enthält das Fenster die Registerkarten **Verlauf Verleger zu Verteiler**, **Verlauf Verteiler zu Abonnent** und **Leistung**, und bei Mergeabonnements finden Sie hier die Registerkarte **Synchronisierungsverlauf**.  
  
    -   Wenn Sie ein Pushabonnement synchronisieren möchten, klicken Sie mit der rechten Maustaste auf das betreffende Abonnement, und klicken Sie dann auf **Synchronisierung starten**.    
    -   Wenn Sie ein Abonnement erneut initialisieren möchten, klicken Sie mit der rechten Maustaste auf das betreffende Abonnement, und klicken Sie dann auf **Abonnement erneut initialisieren**.    
    -   Wenn Sie ein einzelnes Mergeabonnement überprüfen möchten, klicken Sie mit der rechten Maustaste auf das betreffende Abonnement, und klicken Sie dann auf **Abonnement überprüfen**. Wenn Sie alle Abonnements für eine Mergeveröffentlichung überprüfen möchten, klicken Sie mit der rechten Maustaste auf die entsprechende Veröffentlichung, und klicken Sie dann auf **Alle Abonnements überprüfen**. Wenn alle Abonnements für eine Transaktionsveröffentlichung überprüft werden sollen, klicken Sie mit der rechten Maustaste auf die betreffende Veröffentlichung, und klicken Sie dann auf **Abonnements überprüfen**.    
    -   Wenn Sie die Profile für den Agent verwalten möchten, klicken Sie mit der rechten Maustaste auf den Agent, und klicken Sie dann auf **Agentprofil**. Weitere Informationen finden Sie unter [Arbeiten mit Replikations-Agent-Profilen](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).  

    


## <a name="see-also"></a>Weitere Informationen  
 [Anzeigen und Ändern von Veröffentlichungseigenschaften](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Anzeigen und Ändern der Eigenschaften von Pushabonnements](../../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [Anzeigen und Ändern der Eigenschaften von Pullabonnements](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)  
 [Festlegen von Schwellenwerten und Warnungen im Replikationsmonitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)   
 [Replikations-Agent-Verwaltung](../../../relational-databases/replication/agents/replication-agent-administration.md)    
  
  
