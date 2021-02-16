---
title: Ausführen eines Failovers für eine Verfügbarkeitsgruppe
description: Hier wird beschrieben, wie Sie ein geplantes oder erzwungenes manuelles Failover für eine Always On-Verfügbarkeitsgruppe mit SQL Server Management Studio, Transact-SQL oder SQL PowerShell ausführen.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.failoverwizard.connecttoreplicas.f1
- sql13.swb.failoverwizard.progress.f1
- sql13.swb.failoverwizard.selectnewprimary.f1
- sql13.swb.failoverwizard.f1
- sql13.swb.failoverwizard.confirmdataloss.f1
helpviewer_keywords:
- failover [SQL Server], failover
- Availability Groups [SQL Server], wizards
- Availability Groups [SQL Server], configuring
ms.assetid: 4a602584-63e4-4322-aafc-5d715b82b834
author: cawrites
ms.author: chadam
ms.openlocfilehash: c432d109bebee0a7a1574018e50f03b1c99127aa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355147"
---
# <a name="use-the-fail-over-availability-group-wizard-sql-server-management-studio"></a>Verwenden des Assistenten für Failover-Verfügbarkeitsgruppen (SQL Server Management Studio)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie ein geplantes manuelles Failover oder ein erzwungenes manuelles Failover (ein erzwungenes Failover) in einer Always On-Verfügbarkeitsgruppe mit [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]oder PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]ausgeführt wird. Eine Verfügbarkeitsgruppe führt auf der Ebene eines Verfügbarkeitsreplikats ein Failover aus. Bei einem Failover zu einem sekundären Replikat im Status SYNCHRONIZED führt der Assistent ein geplantes manuelles Failover (ohne Datenverlust) aus. Bei einem Failover zu einem sekundären Replikat im Status UNSYNCHRONIZED oder NOT SYNCHRONIZING führt der Assistent ein erzwungenes manuelles Failover aus, ein sogenanntes *erzwungenes Failover* (mit möglichem Datenverlust). Bei beiden Formen des manuellen Failovers geht das sekundäre Replikat, mit dem die Verbindung besteht, in die primäre Rolle über. Bei einem geplanten manuellen Failover wird aktuell das frühere primäre Replikat in die sekundäre Rolle überführt. Nach einem erzwungenen Failover geht das frühere primäre Replikat, sobald es online geschaltet wird, in die sekundäre Rolle über.  

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
 Bevor Ihrem ersten geplanten manuellen Failovers, gehen Sie zum Abschnitt „Vorbereitungen“ unter [Ausführen eines geplanten manuellen Failovers einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)ausgeführt wird.  
  
 Lesen Sie vor dem ersten erzwungenen Failover die Abschnitte „Vorbereitungen“ und „Follow Up: Essential Tasks After a Forced Failover“ (Nächste Schritte: wichtige Aufgaben nach einem erzwungenen Failover) im Artikel [Ausführen eines erzwungenen manuellen Failovers einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md).  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Ein Failoverbefehl gibt einen Wert zurück, sobald das sekundäre Zielreplikat den Befehl akzeptiert hat. Die Datenbankwiederherstellung tritt jedoch asynchron auf, nachdem die Verfügbarkeitsgruppe aufgehört hat, ein Failover auszuführen.  
    
###  <a name="prerequisites-for-using-the-failover-availability-group-wizard"></a><a name="Prerequisites"></a> Voraussetzungen für die Verwendung des Assistenten für das Failover von Verfügbarkeitsgruppen  
  
-   Es muss eine Verbindung mit der Serverinstanz bestehen, die ein aktuell verfügbares Verfügbarkeitsreplikat hostet.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So verwenden Sie den Assistenten für das Failover von Verfügbarkeitsgruppen**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die ein sekundäres Replikat der Verfügbarkeitsgruppe hostet, für die ein Failover ausgeführt werden muss, und erweitern Sie die Serverstruktur.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Um den Assistenten für das Failover von Verfügbarkeitsgruppen zu starten, klicken Sie mit der rechten Maustaste auf die Verfügbarkeitsgruppe, für die ein Failover ausgeführt werden soll, und wählen **Failover** aus.  
  
4.  Welche Informationen auf der Seite **Einführung** angezeigt werden, hängt davon ab, ob ein sekundäres Replikat für ein geplantes Failover geeignet ist. Wenn diese Seite den Text "**Geplantes Failover für diese Verfügbarkeitsgruppe ausführen**" enthält, dann können Sie ein Failover für die Verfügbarkeitsgruppe ohne Datenverlust durchführen.  
  
5.  Auf der Seite **Neues primäres Replikat auswählen** können Sie den Status des aktuellen primären Replikats und des WSFC-Quorums überprüfen, bevor Sie das sekundäre Replikat auswählen, das das neue primäre Replikat (das *Failoverziel*) wird. Für ein geplantes manuelles Failover müssen Sie ein sekundäres Replikat auswählen, das für **Failoverbereitschaft** den Wert "**Kein Datenverlust**" aufweist. Bei einem erzwungenen Failover lautet dieser Wert für alle möglichen Failoverziele „**Data loss, Warnings(** _#_ **)** “ (Datenverlust, Warnungen (#)), wobei *#* die Anzahl der Warnungen angibt, die für ein angegebenes sekundäres Replikat vorhanden sind. Um die Warnungen für ein angegebenes Failoverziel anzuzeigen, klicken Sie auf dessen „Failoverbereitschaft“-Wert.  
  
     Weitere Informationen finden Sie weiter unten in diesem Thema unter [Seite 'Neues primäres Replikat auswählen'](#SelectNewPrimaryReplica).  
  
6.  Stellen Sie auf der Seite **Verbindung mit Replikat herstellen** eine Verbindung mit dem Failoverziel her. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Seite 'Verbindung mit Replikat herstellen'](#ConnectToReplica).  
  
7.  Wenn Sie ein erzwungenes Failover ausführen, zeigt der Assistent die Seite **Potenziellen Datenverlust bestätigen** an. Um mit dem Failover fortzufahren, müssen Sie **Hier klicken, um Failover mit potenziellem Datenverlust zu bestätigen** auswählen. Weitere Informationen finden Sie nachfolgend in diesem Thema unter[Seite 'Potenziellen Datenverlust bestätigen'](#ConfirmPotentialDataLoss).  
  
8.  Überprüfen Sie auf der Seite **Zusammenfassung** die Auswirkungen eines Failovers zum ausgewählten sekundären Replikat.  
  
     Wenn Sie mit der Auswahl zufrieden sind, können Sie optional auf **Skript** klicken, um ein Skript der Schritte zu erstellen, die der Assistent ausführt. Um ein Failover für die Verfügbarkeitsgruppe zum ausgewählten sekundären Replikat durchzuführen, klicken Sie auf **Fertig stellen**.  
  
9. Die Seite **Status** zeigt den Status des Failovervorgangs für die Verfügbarkeitsgruppe an.  
  
10. Bei der Beendigung des Failovervorgangs wird das Ergebnis auf der Seite **Ergebnisse** angezeigt. Klicken Sie nach Abschluss des Assistenten auf **Schließen** , um den Assistenten zu beenden.  
  
     Weitere Informationen finden Sie unter [Seite „Ergebnisse“ &#40;Always On-Verfügbarkeitsgruppen-Assistenten&#41;](../../../database-engine/availability-groups/windows/results-page-always-on-availability-group-wizards.md)ausgeführt wird.  
  
11. Lesen Sie nach einem erzwungenen Failover den Abschnitt „Follow Up: Essential Tasks After a Forced Failover“ (Nächste Schritte: wichtige Aufgaben nach einem erzwungenen Failover) im Artikel [Ausführen eines erzwungenen manuellen Failovers einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md).  
  
## <a name="help-for-pages-that-are-exclusive-to-this-wizard"></a>Hilfe zu Seiten, die diesem Assistenten eigen sind  
 In diesem Abschnitt werden die Seiten beschrieben, die dem [!INCLUDE[ssAoFoAgWiz](../../../includes/ssaofoagwiz-md.md)]eigen sind.  
  
 **In diesem Abschnitt**  
  
-   [Seite 'Neues primäres Replikat auswählen'](#SelectNewPrimaryReplica)  
  
-   [Seite 'Verbindung mit Replikat herstellen'](#ConnectToReplica)  
  
-   [Seite 'Potenziellen Datenverlust bestätigen'](#ConfirmPotentialDataLoss)  
  
 Die Hilfe zu anderen Seiten dieses Assistenten gilt auch für einen oder mehrere andere Assistenten für Always On-Verfügbarkeitsgruppen und wird in separaten F1-Hilfethemen dokumentiert.  
  
###  <a name="select-new-primary-replica-page"></a><a name="SelectNewPrimaryReplica"></a> Select New Primary Replica Page  
 In diesem Abschnitt werden die Optionen der Seite **Neues primäres Replikat auswählen** beschrieben. Auf dieser Seite wählen Sie das sekundäre Replikat (Failoverziel) aus, zu dem ein Failover für die Verfügbarkeitsgruppe durchgeführt wird. Dieses Replikat wird das neue primäre Replikat.  
  
#### <a name="page-options"></a>Seitenoptionen  
 **Aktuelles primäres Replikat**  
 Zeigt den Namen des aktuellen primären Replikats an, wenn es online ist.  
  
 **Status des primären Replikats**  
 Zeigt den Status des aktuellen primären Replikats an, wenn es online ist.  
  
 **Quorum-Status**  
 Zeigt den Quorum-Status des Verfügbarkeitsreplikats für den WSFC-Clustertyp an, der einen der folgenden Werte haben kann:  
  
   |Wert|BESCHREIBUNG|  
   |-----------|-----------------|  
   |**Normales Quorum**|Der Cluster hat mit einem normalem Quorum begonnen.|  
   |**Erzwungenes Quorum**|Der Cluster hat mit einem erzwungenem Quorum begonnen.|  
   |**Unbekanntes Quorum**|Der Clusterquorumstatus ist nicht verfügbar.|  
   |**Nicht zutreffend**|Der Knoten, der das Verfügbarkeitsreplikat hostet, verfügt über kein Quorum.|  
  
 Weitere Informationen finden Sie unter [WSFC-Quorummodi und Abstimmungskonfiguration &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  

 Der Quorum-Status gilt nicht für den Clustertyp „NONE“.

 Der Quorum-Status für den Clustertyp „EXTERNAL“ wird vom Cluster-Manager verwaltet und ist in SQL Server nicht sichtbar.
  
 **Auswählen eines neuen primären Replikats**  
 Wählen Sie mit diesem Raster ein sekundäres Zielreplikat aus, das zum neuen primären Replikat werden soll. Das Raster verfügt über folgende Spalten:  
  
 **Serverinstanz**  
 Zeigt den Namen der Serverinstanz an, auf der das sekundäre Replikat gehostet wird.  
  
 **Verfügbarkeitsmodus**  
 Zeigt den Verfügbarkeitsmodus der Serverinstanz an und kann einen der folgenden Werte haben:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Synchroner Commit**|Im Modus für synchrone Commits wartet ein primäres Replikat mit synchronen Commits vor dem Commit für Transaktionen auf ein sekundäres Replikat mit synchronen Commits, um zu bestätigen, dass das Schreiben des Protokolls abgeschlossen wurde. Im Modus für synchrone Commits wird sichergestellt, dass Transaktionen, für die ein Commit ausgeführt wird, vollständig geschützt sind, sobald eine angegebene sekundäre Datenbank mit der primären Datenbank synchronisiert wird.|  
|**Asynchroner Commit**|Im Modus für asynchrone Commits führt das primäre Replikat einen Commit für Transaktionen aus, ohne auf die Bestätigung zu warten, dass ein sekundäres Replikat mit asynchronem Commit das Protokoll geschrieben hat. Im Modus für asynchrone Commits wird die Transaktionswartezeit auf den sekundären Datenbanken minimiert. Dabei liegen sie jedoch möglicherweise hinter den primären Datenbanken zurück, was Datenverluste zur Folge haben kann.|  
  
 Weitere Informationen finden Sie unter [Verfügbarkeitsmodi &#40;Always On-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)ausgetauscht werden.  
  
 **Failovermodus**  
 Zeigt den Failovermodus der Serverinstanz an und kann einen der folgenden Werte haben:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Automatisch**|Ein sekundäres Replikat, das für ein automatisches Failover konfiguriert wurde, unterstützt auch ein geplantes manuelles Failover, wenn das sekundäre Replikat mit dem primären Replikat synchronisiert wird.|  
|**Manuell**|Man unterscheidet zwei manuelle Failovertypen: geplantes Failover (ohne Datenverlust) und erzwungenes Failover (mit möglichem Datenverlust). Die Unterstützung für ein erzwungenes Failover hängt vom Verfügbarkeitsmodus und für den Modus mit synchronem Commit vom Synchronisierungsstatus des sekundären Replikats wie folgt ab: Welche Form von manuellem Failover gegenwärtig von einem angegebenen sekundären Replikat unterstützt wird, können Sie der Spalte **Failoverbereitschaft** dieses Rasters entnehmen.|  
  
 Weitere Informationen finden Sie weiter unten in diesem Thema unter [Failover und Failovermodi &#40;Always On-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
 **Failoverbereitschaft**  
 Zeigt die Failoverbereitschaft des sekundären Replikats an und kann einen der folgenden Werte enthalten:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Kein Datenverlust**|Dieses sekundäre Replikat unterstützt gegenwärtig geplante Failover. Dieser Wert tritt nur dann auf, wenn ein für den Verfügbarkeitsmodus mit synchronem Commit konfiguriertes sekundäres Replikat mit dem primären Replikat synchronisiert wird.|  
|**Datenverlust, Warnungen (** _#_ **)**|Dieses sekundäre Replikat unterstützt nur erzwungene manuelle Failover (mit möglichem Datenverlust). Dieser Wert tritt immer dann auf, wenn das sekundäre Replikat nicht mit dem primären Replikat synchronisiert ist. Klicken Sie den Link für Datenverlust-Warnungen, um Informationen zum möglichen Datenverlust zu erhalten.|  
  
 **Aktualisieren**  
 Klicken Sie, um das Raster zu aktualisieren.  
  
 **Abbrechen**  
 Klicken Sie, um den Assistenten abzubrechen. Wenn Sie den Assistenten auf der Seite **Neues primäres Replikat auswählen** abbrechen, wird dieser beendet, ohne dass Aktionen ausgeführt werden.  
  
###  <a name="confirm-potential-data-loss-page"></a><a name="ConfirmPotentialDataLoss"></a> Confirm Potential Data Loss Page  
 In diesem Abschnitt werden die Optionen der Seite **Potenziellen Datenverlust bestätigen** beschrieben, die nur angezeigt wird, wenn Sie ein erzwungenes Failover ausführen. Dieses Thema wird nur für den [!INCLUDE[ssAoFoAgWiz](../../../includes/ssaofoagwiz-md.md)]verwendet. Auf dieser Seite können Sie angeben, ob Sie einen möglichen Datenverlust in Kauf nehmen möchten, um ein Failover für die Verfügbarkeitsgruppe zu erzwingen.  
  
#### <a name="confirm-potential-data-loss-options"></a>Optionen für 'Potenziellen Datenverlust bestätigen'  
 Wenn das ausgewählte sekundäre Replikat nicht mit dem primären Replikat synchronisiert wird, zeigt der Assistent eine Warnung an, die besagt, dass ein Failover zu diesem sekundären Replikat bei einer oder mehreren Datenbanken zu einem Datenverlust führen könnte.  
  
 **Klicken Sie hier, um ein Failover mit potenziellem Datenverlust zu bestätigen.**  
 Wenn Sie bereit sind, einen Datenverlust zu riskieren, um die Datenbanken in dieser Verfügbarkeitsgruppe für die Benutzer verfügbar zu machen, klicken Sie auf dieses Kontrollkästchen. Wenn Sie keinen Datenverlust riskieren möchten, können Sie entweder auf **Zurück** klicken, um zur Seite **Neues primäres Replikat auswählen** zurückzukehren, oder auf **Abbrechen** klicken, um den Assistenten zu beenden, ohne ein Failover für die Verfügbarkeitsgruppe auszuführen.  
  
 **Abbrechen**  
 Klicken Sie, um den Assistenten abzubrechen. Wenn Sie den Assistenten auf der Seite **Potenziellen Datenverlust bestätigen** abbrechen, wird dieser beendet, ohne dass eine Aktion ausgeführt wird.  
  
###  <a name="connect-to-replica-page"></a><a name="ConnectToReplica"></a> Connect to Replica Page  
 In diesem Abschnitt werden die Optionen der Seite **Verbindung mit Replikat herstellen** des [!INCLUDE[ssAoFoAgWiz](../../../includes/ssaofoagwiz-md.md)]beschrieben. Diese Seite wird nur angezeigt, wenn keine Verbindung mit dem sekundären Zielreplikat besteht. Verwenden Sie diese Seite, um eine Verbindung mit dem sekundären Replikat herzustellen, das Sie als neues primäres Replikat ausgewählt haben.  
  
#### <a name="page-options"></a>Seitenoptionen  
 **Rasterspalten:**  
 **Serverinstanz**  
 Zeigt den Namen der Serverinstanz an, die als Host für das Verfügbarkeitsreplikat fungiert.  
  
 **Verbunden als**  
 Zeigt das Konto an, das mit der Serverinstanz verbunden ist, nachdem die Verbindung hergestellt wurde. Wenn in dieser Spalte für eine bestimmte Serverinstanz "**Nicht verbunden**" angezeigt wird, müssen Sie auf die Schaltfläche **Verbinden** klicken.  
  
 **Herstellen einer Verbindung**  
 Klicken Sie auf diese Schaltfläche, wenn diese Serverinstanz unter einem anderen Konto als andere Serverinstanzen ausgeführt wird, mit denen Sie eine Verbindung herstellen müssen.  
  
 **Abbrechen**  
 Klicken Sie, um den Assistenten abzubrechen. Wenn Sie den Assistenten auf der Seite **Verbindung mit Replikat herstellen** abbrechen, wird dieser beendet, ohne dass Aktionen ausgeführt werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verfügbarkeitsmodi &#40;Always On-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Failover und Failovermodi (Always On-Verfügbarkeitsgruppen)](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)   
 [Ausführen eines geplanten manuellen Failovers einer Verfügbarkeitsgruppe (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)   
 [Ausführen eines erzwungenen manuellen Failovers einer Verfügbarkeitsgruppe (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)   
 [WSFC-Notfallwiederherstellung durch erzwungenes Quorum &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)  
  
