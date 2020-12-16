---
title: Überwachen der Replikations-Agents | Microsoft-Dokumentation
description: Der Replikationsmonitor von SQL Server bietet einen systemischen Überblick über die Replikationsaktivität und erleichtert die Suche nach Informationen zu einem bestimmten Agent.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- monitoring performance [SQL Server replication], agents
- Log Reader Agent, monitoring
- Replication Monitor, agents
- Merge Agent, monitoring
- Queue Reader Agent, monitoring
- Snapshot Agent, monitoring
- agents [SQL Server replication], monitoring
- Distribution Agent, monitoring
ms.assetid: d06ed24f-82d7-4b9e-9e40-cc9780476a71
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: ddaa4698033f06db589ca550f39d434173e012e7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477841"
---
# <a name="monitor-replication-agents"></a>Überwachen der Replikations-Agents
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Der Replikationsmonitor von [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] bietet einen systemischen Überblick über die Replikationsaktivität und erleichtert gleichzeitig die Suche nach Informationen zu einem bestimmten Agent. Die folgende Liste enthält alle Agents, die Registerkarten im Replikationsmonitor, auf denen die Agents zu finden sind, und einen Verweis darauf, wo Sie Informationen zum Zugreifen auf die jeweilige Registerkarte finden:  
  
-   Die folgenden Agents sind Veröffentlichungen im Replikationsmonitor zugeordnet:  
  
    -   Momentaufnahme-Agent  
  
    -   Protokolllese-Agent  
  
    -   Warteschlangenlese-Agent  
  
     Auf die Informationen und Aufgaben zu diesen Agents können Sie über die folgenden Registerkarten zugreifen: **Agents** (verfügbar für jeden Verleger und jede Veröffentlichung) und **Warnungen** (verfügbar für jede Veröffentlichung). Weitere Informationen finden Sie unter [View information and perform tasks using Replication Monitor (Anzeigen von Informationen und Ausführen von Aufgaben mit dem Replikationsmonitor)](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
-   Die folgenden Agents sind Abonnements im Replikationsmonitor zugeordnet:  
  
    -   Verteilungs-Agent  
  
    -   Merge-Agent  
  
     Auf die Informationen und Aufgaben zu diesen Agents können Sie über die folgenden Registerkarten zugreifen: **Überwachungsliste für Abonnements** (verfügbar für jeden Verleger) oder **Alle Abonnements** (verfügbar für jede Veröffentlichung). Weitere Informationen finden Sie unter [View information and perform tasks using Replication Monitor (Anzeigen von Informationen und Ausführen von Aufgaben mit dem Replikationsmonitor)](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
## <a name="using-sql-server-management-studio-to-monitor-replication-agents"></a>Überwachen der Replikations-Agents mit SQL Server Management Studio  
 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] stellt die folgenden Dialogfelder zum Überwachen von Replikations-Agents bereit:  
  
-   **Status des Momentaufnahme-Agents anzeigen** (bei allen Veröffentlichungen)  
  
-   **Status des Protokolllese-Agents anzeigen** (bei allen Transaktionsveröffentlichungen)  
  
-   **Synchronisierungsstatus anzeigen** (bei allen Abonnements; von diesem Dialogfeld aus können Sie auf den Verteilungs-Agent und den Merge-Agent zugreifen)  
  
 Der Replikationsmonitor stellt darüber hinaus zusätzliche Informationen zu den einzelnen Agents bereit und bietet außerdem die Möglichkeit der Überwachung des Warteschlangenlese-Agents, sofern dieser verwendet wird. Weitere Informationen finden Sie unter [View information and perform tasks using Replication Monitor (Anzeigen von Informationen und Ausführen von Aufgaben mit dem Replikationsmonitor)](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).
  
#### <a name="to-monitor-the-snapshot-agent-and-log-reader-agent"></a>So überwachen Sie den Momentaufnahme-Agent und den Protokokolllese-Agent  
  
1.  Stellen Sie in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten.  
  
2.  Erweitern Sie den Ordner **Replikation** , und erweitern Sie dann den Ordner **Lokale Veröffentlichungen** .  
  
3.  Klicken Sie mit der rechten Maustaste auf eine Veröffentlichung, und klicken Sie dann auf **Status des Protokolllese-Agents anzeigen** bzw. **Status des Momentaufnahme-Agents anzeigen**.  
  
4.  Führen Sie im Dialogfeld **Status des Protokolllese-Agents anzeigen** bzw. **Status des Momentaufnahme-Agents anzeigen** einen oder mehrere der folgenden Schritte aus:  
  
    -   Zeigen Sie den Agentstatus an.  
  
    -   Starten oder beenden Sie den Agent bei Bedarf.  
  
    -   Klicken Sie auf **Überwachen** , um den **Replikationsmonitor** zu starten.  
  
5.  Klicken Sie auf **Schließen**.  
  
#### <a name="to-monitor-the-distribution-agent-and-merge-agent-from-the-publisher"></a>So überwachen Sie den Verteilungs-Agent und den Merge-Agent (vom Verleger aus)  
  
1.  Stellen Sie in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten.  
  
2.  Erweitern Sie den Ordner **Replikation** , und erweitern Sie dann den Ordner **Lokale Veröffentlichungen** .  
  
3.  Erweitern Sie die Veröffentlichung für das Abonnement, das überwacht werden soll.  
  
4.  Klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Synchronisierungsstatus anzeigen**.  
  
5.  Führen Sie im Dialogfeld **Synchronisierungsstatus anzeigen** einen oder mehrere der folgenden Schritte aus:  
  
    -   Zeigen Sie den Agentstatus an.  
  
    -   Starten oder beenden Sie den Agent bei Bedarf.  
  
    -   Klicken Sie bei Pushabonnements auf **Überwachen** , um den **Replikationsmonitor** zu starten.  
  
    -   Klicken Sie bei Pullabonnements auf **Auftragsverlauf anzeigen** , um den **Protokolldatei-Viewer** zu starten, in dem das Ergebnis aus dem Agentprotokoll angezeigt wird.  
  
6.  Klicken Sie auf **Schließen**.  
  
#### <a name="to-monitor-the-distribution-agent-and-merge-agent-from-the-subscriber"></a>So überwachen Sie den Verteilungs-Agent und den Merge-Agent (vom Abonnenten aus)  
  
1.  Stellen Sie in [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]eine Verbindung mit dem Abonnenten her, und erweitern Sie dann den Serverknoten.  
  
2.  Erweitern Sie den Ordner **Replikation** , und erweitern Sie dann den Ordner **Lokale Abonnements** .  
  
3.  Klicken Sie mit der rechten Maustaste auf das zu überwachende Abonnement, und klicken Sie dann auf **Synchronisierungsstatus anzeigen**.  
  
4.  Führen Sie im Dialogfeld **Synchronisierungsstatus anzeigen** einen oder mehrere der folgenden Schritte aus:  
  
    -   Zeigen Sie den Agentstatus an.  
  
    -   Starten oder beenden Sie den Agent bei Bedarf.  
  
    -   Klicken Sie auf **Auftragsverlauf anzeigen** , um den **Protokolldatei-Viewer** zu starten, in dem das Ergebnis aus dem Agentprotokoll angezeigt wird.  
  
5.  Klicken Sie auf **Schließen**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über Replikations-Agents](../../../relational-databases/replication/agents/replication-agents-overview.md)  
  
  
