---
title: Notfallwiederherstellung durch erzwungenes Quorum
description: Zur Beseitigung eines Quorumfehlers ist ein manueller Eingriff erforderlich. In diesem Artikel wird erläutert, wie Sie im Falle einer ausgefallenen SQL Server-Failoverclusterinstanz ein Quorum erzwingen.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
- failover clustering [SQL Server], AlwaysOn Availability Groups
ms.assetid: 6cefdc18-899e-410c-9ae4-d6080f724046
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2358f1f57a7d6b72f97daf4c27963436a40adfca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355231"
---
# <a name="wsfc-disaster-recovery-through-forced-quorum-sql-server"></a>WSFC-Notfallwiederherstellung durch erzwungenes Quorum (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Quorumfehler werden normalerweise durch eine systemische Katastrophe, einen persistenten Kommunikationsfehler oder eine fehlerhafte Konfiguration, die mehrere Knoten im WSFC-Cluster betreffen, verursacht.  Zur Beseitigung eines Quorumfehlers ist ein manueller Eingriff erforderlich.  
  
-   **Vorbereitung:**  [Voraussetzungen](#Prerequisites), [Sicherheit](#Security)  
  
-   **WSFC-Notfallwiederherstellung durch die Prozedur für erzwungene Quoren** [WSFC-Notfallwiederherstellung durch die Prozedur für erzwungene Quoren](#Main)  
  
-   [Verwandte Aufgaben](#RelatedTasks)  
  
-   [Verwandte Inhalte](#RelatedContent)  
  
##  <a name="before-you-start"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
 Bei der Prozedur für erzwungene Quoren wird davon ausgegangen, dass vor dem Quorumfehler ein fehlerfreies Quorum vorhanden war.  
  
> [!WARNING]  
>  Der Benutzer sollte mit den Begriffen und Wechselwirkungen von Windows Server Failover Clustering, WSFC-Quorummodellen, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]und der spezifischen Bereitstellungskonfiguration der Umgebung vertraut sein.  
>   
>  Weitere Informationen finden Sie unter  [Windows Server-Failoverclustering (WSFC) mit SQL Server](https://msdn.microsoft.com/library/hh270278\(v=SQL.110\).aspx) und [WSFC-Quorummodi und Abstimmungskonfiguration (SQL Server)](https://msdn.microsoft.com/library/hh270280\(v=SQL.110\).aspx)  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
 Der Benutzer muss einem Domänenkonto entsprechen, das Mitglied der lokalen Administratorgruppe an jedem Knoten des WSFC-Clusters ist.  
  
##  <a name="wsfc-disaster-recovery-through-the-forced-quorum-procedure"></a><a name="Main"></a> WSFC-Notfallwiederherstellung durch die Prozedur für erzwungene Quoren  
 Vergessen Sie nicht, dass bei einem Quorumfehler alle gruppierten Dienste, SQL Server-Instanzen und [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]im WSFC-Cluster offline geschaltet werden, da der Cluster in der aktuellen Konfiguration keine Fehlertoleranz auf Knotenebene gewährleisten kann.  Ein Quorumfehler bedeutet, dass fehlerfreie Abstimmungsknoten im WSFC-Cluster nicht mehr dem Quorummodell entsprechen. Einige Knoten sind möglicherweise völlig ausgefallen, und andere haben den WSFC-Dienst möglicherweise nur heruntergefahren und sind, abgesehen vom Verlust der Fähigkeit, mit einem Quorum zu kommunizieren, möglicherweise fehlerfrei.  
  
 Um den WSFC-Cluster wieder online zu schalten, müssen Sie die Ursache für den Quorumfehler in der vorhandenen Konfiguration beheben, die betroffenen Datenbanken nach Bedarf wiederherstellen und die übrigen Knoten im WSFC-Cluster neu konfigurieren, um die verbleibende Clustertopologie widerzuspiegeln.  
  
 Sie können die *erzwungene Quorumprozedur* für einen WSFC-Clusterknoten verwenden, um die Sicherheitskontrollen zu überschreiben, die den Cluster offline geschaltet haben.  Damit wird der Cluster im Endeffekt angewiesen, die Quorumssstimmenprüfungen anzuhalten, und Sie erhalten damit die Möglichkeit, die WSFC-Clusterressourcen und SQL Server auf beliebigen Knoten im Cluster wieder online schalten.  
  
 Dieser Typ von Notfallwiederherstellungsprozess sollte die folgenden Schritte umfassen:  
  
#### <a name="to-recover-from-quorum-failure"></a>So stellen Sie das System nach Quorumfehler wieder her:  
  
1.  **Bestimmen Sie den Fehlerumfang.** Identifizieren Sie, welche Verfügbarkeitsgruppen oder SQL Server-Instanzen nicht mehr reagieren, welche Clusterknoten online sind und zur Verwendung nach der Katastrophe verfügbar sind, und untersuchen Sie die Windows-Ereignisprotokolle und die SQL Server-Systemprotokolle.  Wo praktikabel, sollten Sie forensische Daten und Systemprotokolle für spätere Analysen beibehalten.  
  
    > [!TIP]  
    >  Auf einer reagierenden Instanz von [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]können Sie Informationen zum Zustand von Verfügbarkeitsgruppen, die auf der lokalen Serverinstanz ein Verfügbarkeitsreplikat besitzen, abrufen, indem Sie die dynamische Verwaltungssicht (DMV, Dynamic Management View) [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md) abfragen.  
  
2.  **Starten Sie den WSFC-Cluster mit erzwungenem Quorum auf einem einzelnen Knoten.** Identifizieren Sie einen Knoten mit einer minimalen Anzahl von Komponentenfehlern, auf dem der WSFC-Clusterdienst nicht heruntergefahren wurde.  Überprüfen Sie, ob dieser Knoten mit den meisten anderen Knoten kommunizieren kann.  
  
     Versetzen Sie den Cluster von diesem Knoten aus unter Verwendung der erzwungenen Quorumprozedur manuell in den Onlinemodus.  Um potenzielle Datenverluste zu minimieren, wählen Sie einen Knoten aus, der zuletzt ein primäres Replikat der Verfügbarkeitsgruppe gehostet hat.  
  
     Weitere Informationen finden Sie unter  [Erzwingen des Starts eines Clusters ohne Quorum](https://msdn.microsoft.com/library/hh270275\(v=SQL.110\).aspx)  
  
    > [!NOTE]  
    >  Die erzwungene Quorumeinstellung bewirkt, dass im gesamten Cluster die Quorumüberprüfungen blockiert werden, bis der logische WSFC-Cluster die Mehrheit der Abstimmungsknoten erreicht und automatisch zu einem regulären Quorumbetriebsmodus übergeht.  
  
3.  **Starten Sie den WSFC-Dienst auf jedem einzelnen ansonsten fehlerfreien Knoten normal.** Sie müssen die erzwungene Quorumoption nicht angeben, wenn Sie den Clusterdienst auf den anderen Knoten starten.  
  
     Sobald der WSFC-Dienst auf den einzelnen Knoten wieder online ist, verhandelt er mit den anderen fehlerfreien Knoten, um den neuen Clusterkonfigurationszustand zu synchronisieren.  Denken Sie daran, diesen Schritt für jeden Knoten getrennt auszuführen, um potenzielle Racebedingungen beim Auflösen des letzten bekannten Clusterstatus zu verhindern.  
  
    > [!WARNING]  
    >  Stellen Sie sicher, dass jeder Knoten, den Sie starten, mit den anderen neu online geschalteten Knoten kommunizieren kann.  Erwägen Sie, den WSFC-Dienst bei den anderen Knoten zu deaktivieren.  Andernfalls laufen Sie Gefahr, mehr als eine Quorumknotengruppe zu erstellen. Das ist ein Split-Brain-Szenario. Wenn Ihre Ermittlungsergebnisse in Schritt 1 richtig waren, sollte dieser Fall nicht eintreten.  
  
4.  **Übernehmen der neuen Quorummodus- und Knotenabstimmungskonfiguration.** Wenn durch das Erzwingen des Quorums alle Knoten im Cluster erfolgreich neu gestartet wurden und die Ursache für den Quorumfehler korrigiert wurde, sind keine Änderungen am ursprünglichen Quorummodus und der Knotenabstimmungskonfiguration erforderlich.  
  
     Andernfalls sollten Sie den neu wiederhergestellten Clusterknoten und Verfügbarkeitsreplikattopologie auswerten und bei Bedarf den Quorummodus und die Abstimmungszuweisungen für jeden Knoten ändern. Nicht wiederhergestellte Knoten sollten offline geschaltet werden oder einen Knotenabstimmungswert von 0 (Null) erhalten.  
  
    > [!TIP]  
    >  Zu diesem Zeitpunkt hat es möglicherweise den Anschein, als wären die Knoten und SQL Server-Instanzen im Cluster in ihrem normalen Betriebszustand wiederhergestellt.  Allerdings ist unter Umständen immer noch kein fehlerfreies Quorum vorhanden.  Vergewissern Sie sich mithilfe des Failovercluster-Managers oder des AlwaysOn-Dashboards in SQL Server Management Studio oder der entsprechenden DMVs, dass ein Quorum wiederhergestellt wurde.  
  
5.  **Stellen Sie Datenbankreplikate der Verfügbarkeitsgruppen bei Bedarf wieder her.** Datenbanken, die mit keiner Verfügbarkeitsgruppe verknüpft sind, sollten im Rahmen des regulären SQL Server-Startprozess von allein wiederhergestellt und online geschaltet werden.  
  
     Sie können potenzielle Datenverluste und den Zeitaufwand des Wiederherstellungsvorgangs für die Verfügbarkeitsgruppenreplikate minimieren, indem Sie diese in der folgenden Reihenfolge wieder online schalten: primäres Replikat, synchrone sekundäre Replikate, asynchrone sekundäre Replikate.  
  
6.  **Reparatur oder Austausch fehlerhafter Komponenten und erneute Überprüfung des Clusters.** Nachdem Sie das System nach dem Auftreten der ursprünglichen Katastrophe und des Quorumfehlers wiederhergestellt haben, sollten Sie die fehlerhaften Knoten reparieren oder austauschen und die zugehörigen WSFC- und AlwaysOn-Konfigurationen entsprechend anpassen.  Hierzu kann es erforderlich sein, Verfügbarkeitsgruppenreplikate zu löschen, Knoten aus dem Cluster zu entfernen oder die Software eines Knotens zu vereinfachen oder neu zu installieren.  
  
     Sie müssen alle fehlerhaften Verfügbarkeitsreplikate reparieren oder entfernen.  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] schneidet das Transaktionsprotokoll nicht am letzten bekannten Punkt des Verfügbarkeitsreplikats mit den am weitesten zurückliegenden Daten ab.   Wenn ein fehlerhaftes Replikat nicht repariert oder aus der Verfügbarkeitsgruppe entfernt wird, wachsen die Transaktionsprotokolle, und es besteht das Risiko, dass nicht mehr ausreichend Speicherplatz für die Transaktionsprotokolle der anderen Replikate verfügbar ist.  
  
    > [!NOTE]  
    >  Wenn Sie den WSFC-Konfigurationsüberprüfungs-Assistenten ausführen, wenn im WSFC-Cluster ein Verfügbarkeitsgruppenlistener vorhanden ist, generiert der Assistent die folgende falsche Warnmeldung:  
    >   
    >  „Die RegisterAllProviderIP-Eigenschaft für Netzwerkname 'Name:&lt;Netzwerkname' ist auf 1 festgelegt. Für die aktuelle Clusterkonfiguration muss dieser Wert auf 0 festgelegt werden.“  
    >   
    >  Ignorieren Sie diese Meldung.  
  
7.  **Wiederholen Sie Schritt 4 nach Bedarf.** Das Ziel besteht darin, erneut das für einen fehlerfreien Betrieb angemessene Maß an Fehlertoleranz und Hochverfügbarkeit wiederherzustellen.  
  
8.  **Ausführen einer RPO/RTO-Analyse.** Sie sollten SQL Server-Systemprotokolle, Datenbank-Timestamps und Windows-Ereignisprotokolle analysieren, um die Fehlerursache zu bestimmen und die tatsächlichen Wiederherstellungspunkt- und Wiederherstellungszeitdaten zu dokumentieren.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Erzwingen des Starts eines Clusters ohne Quorum](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md)  
  
-   [Ausführen eines erzwungenen manuellen Failovers einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Anzeigen von Cluster-Quorum-NodeWeight-Einstellungen](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
-   [Konfigurieren von Cluster-Quorum-NodeWeight-Einstellungen](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
-   [Verwenden des AlwaysOn-Dashboards &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   [Anzeigen von Ereignissen und Protokollen für einen Failovercluster](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Get-ClusterLog-Failovercluster-Cmdlet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
## <a name="see-also"></a>Weitere Informationen  
 [Windows Server-Failoverclustering &#40;WSFC&#41; mit SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)  
  
