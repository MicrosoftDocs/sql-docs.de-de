---
description: Verteilerinformationen, Veröffentlichungen
title: Verteilerinformationen, Veröffentlichungen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.monitor.Distributor.Publications.f1
- sql13.rep.monitor.Distributor.commonjobs..f1
- sql13.rep.monitor.Distributor.SubscriptionSummary.merge.f1
- sql13.rep.monitor.Distributor.SubscriptionSummary.snapshot.f1
- sql13.rep.monitor.Distributor.SubscriptionSummary.tran.f1
ms.assetid: 1f499277-7f12-42ba-8cf4-52b683434944
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: b5fb4f1788ed3812490ca0e730f854b18c64e6ad
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464791"
---
# <a name="distributor-information-publications"></a>Verteilerinformationen, Veröffentlichungen
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Die Registerkarte **Veröffentlichungen** enthält Zusammenfassungsinformationen zu allen Veröffentlichungen auf dem Verteiler, die Sie im linken Bereich ausgewählt haben.  
  
Die Informationen, die bezüglich der vom Verteiler unterstützten Veröffentlichungen angezeigt werden, umfassen eine Spalte mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz des Verlegers. Ansonsten stimmten die Veröffentlichungsinformationen mit den Informationen überein, die beim Anzeigen von Veröffentlichungen in der Verlegeransicht des Replikationsmonitors bereitgestellt werden. Weitere Informationen zu den Spalten auf der Registerkarte **Veröffentlichungen** finden Sie unter [Publisher Information, Publications](../../relational-databases/replication/publisher-information-publications.md).  

## <a name="subscription-watch-list"></a>Überwachungsliste für Abonnements

### <a name="transactional-replication"></a>Transaktionsreplikation
  Informationen zu Transaktionsabonnements enthalten den Namen des Verlegers. Davon abgesehen entsprechen die bereitgestellten Funktionen und Informationen in diesem Dialogfeld denen in der Verlegeransicht. Weitere Informationen zur Verwendung dieses Dialogfelds finden Sie unter [Verlegerinformationen, Überwachungsliste für Abonnements &#40;Transaktionsveröffentlichung, SQL Server 2005 und später&#41;](../../relational-databases/replication/publisher-information-subscription-watch-list-transactional.md). 

### <a name="merge-replication"></a>Mergereplikation
  Informationen zu Abonnements von Mergeveröffentlichungen enthalten den Namen des Verlegers. Davon abgesehen entsprechen die bereitgestellten Funktionen und Informationen in diesem Dialogfeld denen in der Verlegeransicht. Weitere Informationen zur Verwendung dieses Dialogfelds finden Sie unter [Verlegerinformationen, Überwachungsliste für Abonnements &#40;Mergeveröffentlichung, SQL Server 2005 und später&#41;](../../relational-databases/replication/publisher-information-subscription-watch-list-merge-publication.md).  

### <a name="snapshot-replication"></a>Momentaufnahmereplikation 
  Informationen zu Abonnements von Momentaufnahmeveröffentlichungen enthalten den Namen des Verlegers. Davon abgesehen entsprechen die bereitgestellten Funktionen und Informationen in diesem Dialogfeld denen in der Verlegeransicht. Weitere Informationen zur Verwendung dieses Dialogfelds finden Sie unter [Verlegerinformationen, Überwachungsliste für Abonnements &#40;Momentaufnahmeveröffentlichung, SQL Server 2005 und später&#41;](../../relational-databases/replication/publisher-information-subscription-watch-list-snapshot.md).  

## <a name="agents"></a>Agents
 Auf der Registerkarte **Agents** werden Informationen zu den Agents und Wartungsaufträgen angezeigt, die dem Verleger und dem Abonnenten zugeordnet sind.  
  
 Die Agents, die auf der Registerkarte **Agents** in der Verteileransicht für einen Verteiler verfügbar sind, umfassen alle Agents, die auf der Registerkarte **Agents** für einen Verleger verfügbar sind. Die Registerkarte **Agents** für einen Verteiler in der Verteileransicht enthält jedoch auch einen Verteiler-Agent und einen Merge-Agent.  
  
 Weitere Informationen zu Momentaufnahme-, Protokollleser- und Warteschlangenlese-Agents sowie Wartungsaufträgen finden Sie unter [Publisher Information, Agents](../../relational-databases/replication/publisher-information-agents.md). Beachten Sie, dass beim Anzeigen von Agent-Informationen für einen Verteiler auf der Registerkarte **Agents** Verlegerinformationen für den Momentaufnahme- und den Protokollleser-Agent vorhanden sind. Auf der Registerkarte **Agents** für einen Verteiler in der Verteileransicht können Sie auch jedoch **Verteiler-Agent** und **Merge-Agent** auswählen.  
  
### <a name="options"></a>Optionen  
 In den folgenden Abschnitten werden die Daten beschrieben, die auf dieser Registerkarte für den Verteiler-Agent und den Merge-Agent angezeigt werden.  
  
### <a name="distributor-agent"></a>Verteiler-Agent  
 **Status**  
 Der Status des Agents. In der folgenden Liste sind die möglichen Statuswerte aufgeführt:  
  
-   Fehler    
-   Wiederholen    
-   Wird ausgeführt    
-   Wird nicht ausgeführt   
-   Nie gestartet  
  
 **Publisher**  
 Der Name einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz des Verlegers.  
  
 **Veröffentlichung**  
 Der Name der Veröffentlichung, der der Agent zugeordnet ist.  
  
 **Abonnement**  
 Der Name des Abonnements im Format [*SubscriberName*].[*Database*].  
  
 **Type**  
 Der Typ der Replikation: push, pull oder anonymous.  
  
 **Letzte Startzeit**  
 Zeitpunkt, zu dem der Agent beim letzten Mal gestartet wurde.  
  
 **Duration**  
 Die Zeitdauer, für die der Agent ausgeführt wurde. Dieser Wert gibt entweder die verstrichene Zeit eines zurzeit ausgeführten Agents oder die Gesamtzeit des zuvor ausgeführten Agents an.  
  
 **Letzte Aktion**  
 Die letzte Aktion, die bei der letzten Ausführung des Agents ausgeführt wurde.  
  
 **Übermittlungsrate**  
 Die Rate (in Befehlen pro Sekunde), mit der bei der letzten Ausführung des Agents für Initialisierungsbefehle ein Commit in der Verteilungsdatenbank ausgeführt wurde.  
  
 **Latenz**  
 Die verstrichene Zeit in Sekunden zwischen dem Commit der letzten Änderung in der Veröffentlichungsdatenbank und dem Commit des zugehörigen Befehls in der Verteilungsdatenbank.  
  
 **#Trans**  
 Die Anzahl von Transaktionen, für die bei der letzten Ausführung des Agents ein Commit in der Verteilungsdatenbank ausgeführt wurde.  
  
 **#Cmds**  
 Die Anzahl von Befehlen, für die bei der letzten Ausführung des Agents ein Commit in der Verteilungsdatenbank ausgeführt wurde. Ein Befehl entspricht einer Datenänderung, z. B. einem Update.  
  
 **Durchschn. Anzahl der Befehle**  
 Die durchschnittliche Anzahl von Befehlen pro Transaktion bei der letzten Ausführung des Agents.  
  
### <a name="merge-agent"></a>Merge-Agent  
 **Status**  
 Der Status des Agents. In der folgenden Liste sind die möglichen Statuswerte aufgeführt:  
  
-   Fehler  
  
-   Wiederholen  
  
-   Wird ausgeführt  
  
-   Wird nicht ausgeführt  
  
-   Nie gestartet  
  
 **Publisher**  
 Der Name einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz des Verlegers.  
  
 **Veröffentlichung**  
 Der Name der Veröffentlichung, der der Agent zugeordnet ist.  
  
 **Abonnement**  
 Der Name des Abonnements im Format [*SubscriberName*].[*Database*].  
  
 **Type**  
 Der Typ der Replikation: push, pull oder anonymous.  
  
 **Letzte Startzeit**  
 Zeitpunkt, zu dem der Agent beim letzten Mal gestartet wurde.  
  
 **Duration**  
 Die Zeitdauer, für die der Agent ausgeführt wurde. Dieser Wert gibt entweder die verstrichene Zeit eines zurzeit ausgeführten Agents oder die Gesamtzeit des zuvor ausgeführten Agents an.  
  
 **Letzte Aktion**  
 Die letzte Aktion, die bei der letzten Ausführung des Agents ausgeführt wurde.  
  
 **Übermittlungsrate**  
 Die Rate (in Befehlen pro Sekunde), mit der für Änderungen ein Commit in der Verteilungsdatenbank ausgeführt wird.  
  
 **Verlegereinfügungen**  
 Die Anzahl der auf dem Verleger angewendeten INSERT-Befehle.  
  
 **Verlegerupdates**  
 Die Anzahl der auf dem Verleger angewendeten UPDATE-Befehle.  
  
 **Verlegerlöschungen**  
 Die Anzahl der auf dem Verleger angewendeten DELETE-Befehle.  
  
 **Verlegerkonflikte**  
 Die Anzahl der Konflikte beim Verleger, die im Rahmen des Mergevorgangs aufgetreten sind.  
  
 **Abonnenteneinfügungen**  
 Die Anzahl der auf dem Abonnenten angewendeten INSERT-Befehle.  
  
 **Abonnentenupdates**  
 Die Anzahl der auf dem Abonnenten angewendeten UPDATE-Befehle.  
  
 **Abonnentenlöschungen**  
 Die Anzahl der auf dem Abonnenten angewendeten DELETE-Befehle.  
  
 **Abonnentenkonflikte**  
 Die Anzahl der Konflikte beim Abonnenten, die im Rahmen des Mergevorgangs aufgetreten sind.  

  
## <a name="see-also"></a>Weitere Informationen  
 [Starten des Replikationsmonitors](../../relational-databases/replication/monitor/start-the-replication-monitor.md)   
 [Überwachen der Replikation](../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)  
  
  
