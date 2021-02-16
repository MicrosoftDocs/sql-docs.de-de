---
title: 'Richtlinienbasierte Verwaltung: Verfügbarkeitsgruppen'
description: Das Zustandsmodell für Always On-Verfügbarkeitsgruppen wertet eine Reihe vordefinierter Richtlinien der richtlinienbasierten Verwaltung (Policy Bases Management, PBM) aus. Sie können Thesen verwenden, um den Zustand einer Verfügbarkeitsgruppe sowie deren Replikate und Datenbanken in SQL Server anzuzeigen.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
helpviewer_keywords:
- Availability Groups [SQL Server], troubleshooting
- Availability Groups [SQL Server], policies
ms.assetid: afa5289c-641a-4c03-8749-44862384ec5f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0e5208846958a09f53425bf42bd9e4058a9cfa8d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347500"
---
# <a name="policy-based-management-for-operational-issues-with-always-on-availability-groups"></a>Richtlinienbasierte Verwaltung von Betriebsproblemen mit Always On-Verfügbarkeitsgruppen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Das Zustandsmodell für Always On-Verfügbarkeitsgruppen wertet eine Reihe vordefinierter Richtlinien der richtlinienbasierten Verwaltung (Policy Bases Management, PBM) aus. Sie können Thesen verwenden, um den Zustand einer Verfügbarkeitsgruppe sowie deren Verfügbarkeitsreplikate und Datenbanken in SQL Server anzuzeigen.  
  
  
##  <a name="terms-and-definitions"></a><a name="TermsAndDefinitions"></a> Begriffe und Definitionen  
 Vordefinierte Always On-Richtlinien  
 Ein Satz integrierter Richtlinien, die es Datenbankadministratoren ermöglichen, eine Verfügbarkeitsgruppe und die zugehörigen Verfügbarkeitsreplikate und -datenbanken auf Konformität mit den Zuständen zu prüfen, die durch die Always On-Richtlinien definiert werden.  
  
 [AlwaysOn-Verfügbarkeitsgruppen](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
 Eine Lösung für hohe Verfügbarkeit und Notfallwiederherstellung, die eine Alternative zur Datenbankspiegelung auf Unternehmensebene bietet.  
  
 Verfügbarkeitsgruppe  
 Ein Container für jeden diskreten Satz von Benutzerdatenbanken (auch *Verfügbarkeitsdatenbanken* genannt), für die zusammen ein Failover ausgeführt wird.  
  
 Verfügbarkeitsreplikat  
 Eine Instanziierung einer Verfügbarkeitsgruppe, die von einer bestimmten Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] gehostet wird und eine lokale Kopie jeder Verfügbarkeitsdatenbank beibehält, die zur Verfügbarkeitsgruppe gehört. Zwei Typen von Verfügbarkeitsreplikaten sind vorhanden: ein einzelnes *primäres Replikat* und ein bis vier *sekundäre Replikate*. Die Serverinstanzen, die die Verfügbarkeitsreplikate für eine angegebene Verfügbarkeitsgruppe hosten, müssen sich auf verschiedenen Konten eines einzelnen WSFC-Clusters (Windows Server-Failoverclustering) befinden.  
  
 Verfügbarkeitsdatenbank  
 Eine Datenbank, die zu einer Verfügbarkeitsgruppe gehört. Für jede Verfügbarkeitsdatenbank verwaltet die Verfügbarkeitsgruppe eine einzelne Lese-/Schreibkopie (die *primäre Datenbank*) und eine bis vier schreibgeschützte Kopien (*sekundäre Datenbanken*).  
  
 Das Always On-Dashboard  
 Ein [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] -Dashboard, das eine Übersicht über die Integrität einer Verfügbarkeitsgruppe bereitstellt. Weitere Informationen hierzu finden Sie unter [Always On-Dashboard](#Dashboard)weiter unten in diesem Thema.  
  
##  <a name="predefined-policies-and-issues"></a><a name="Always OnPBM"></a> Vordefinierte Richtlinien und Probleme  
 In der folgenden Tabelle sind die vordefinierten Richtlinien zusammengefasst.  
  
|Richtlinienname|Problem|Kategorie **&#42;**|Facet|  
|-----------------|-----------|--------------------|-----------|  
|WSFC-Clusterstatus|[WSFC-Clusterdienst ist offline](../../../database-engine/availability-groups/windows/wsfc-cluster-service-is-offline.md).|Kritisch|SQL Server-Instanz|  
|Onlinezustand der Verfügbarkeitsgruppe|[Verfügbarkeitsgruppe ist offline](../../../database-engine/availability-groups/windows/availability-group-is-offline.md).|Kritisch|Verfügbarkeitsgruppe|  
|Bereitschaft der Verfügbarkeitsgruppe für automatisches Failover|[Verfügbarkeitsgruppe nicht bereit für automatischen Failover](../../../database-engine/availability-groups/windows/availability-group-is-not-ready-for-automatic-failover.md).|Kritisch|Verfügbarkeitsgruppe|  
|Datensynchronisierungsstatus der Verfügbarkeitsreplikate|[Einige Verfügbarkeitsreplikate synchronisieren keine Daten](../../../database-engine/availability-groups/windows/some-availability-replicas-are-not-synchronizing-data.md).|Warnung|Verfügbarkeitsgruppe|  
|Datensynchronisierungsstatus synchroner Replikate|[Einige synchrone Replikate wurden nicht synchronisiert](../../../database-engine/availability-groups/windows/some-synchronous-replicas-are-not-synchronized.md).|Warnung|Verfügbarkeitsgruppe|  
|Verfügbarkeitsreplikat-Rollenstatus|[Einige Verfügbarkeitsreplikate haben keine fehlerfreie Rolle](../../../database-engine/availability-groups/windows/some-availability-replicas-do-not-have-a-healthy-role.md).|Warnung|Verfügbarkeitsgruppe|  
|Verbindungsstatus von Verfügbarkeitsreplikaten|[Einige Verfügbarkeitsreplikate sind getrennt](../../../database-engine/availability-groups/windows/some-availability-replicas-are-disconnected.md).|Warnung|Verfügbarkeitsgruppe|  
|Verfügbarkeitsreplikat-Rollenstatus|[Verfügbarkeitsreplikat hat keine fehlerfreie Rolle](../../../database-engine/availability-groups/windows/availability-replica-does-not-have-a-healthy-role.md).|Kritisch|Verfügbarkeitsreplikat|  
|Verfügbarkeitsreplikat-Verbindungsstatus|[Verfügbarkeitsreplikat wird getrennt](../../../database-engine/availability-groups/windows/availability-replica-is-disconnected.md).|Kritisch|Verfügbarkeitsreplikat|  
|Joinzustand des Verfügbarkeitsreplikats|[Verfügbarkeitsreplikat ist nicht verknüpft](../../../database-engine/availability-groups/windows/availability-replica-is-not-joined.md).|Warnung|Verfügbarkeitsreplikat|  
|Datensynchronisierungsstatus des Verfügbarkeitsreplikats|[Datensynchronisierungsstatus einer Verfügbarkeitsdatenbank ist nicht fehlerfrei](../../../database-engine/availability-groups/windows/data-synchronization-state-of-some-availability-database-is-not-healthy.md).|Warnung|Verfügbarkeitsreplikat|  
|Verfügbarkeitsdatenbank im angehaltenen Zustand|[Verfügbarkeitsdatenbank angehalten](../../../database-engine/availability-groups/windows/availability-database-is-suspended.md).|Warnung|Verfügbarkeitsdatenbank|  
|Joinzustand der Verfügbarkeitsdatenbank|[Sekundäre Datenbank ist nicht verknüpft](../../../database-engine/availability-groups/windows/secondary-database-is-not-joined.md).|Warnung|Verfügbarkeitsdatenbank|  
|Datensynchronisierungsstatus der Verfügbarkeitsdatenbank|[Datensynchronisierungsstatus der Verfügbarkeitsdatenbank ist nicht fehlerfrei](../../../database-engine/availability-groups/windows/data-synchronization-state-of-availability-database-is-not-healthy.md).|Warnung|Verfügbarkeitsdatenbank|  
  
> [!IMPORTANT]
>  **&#42;** Für Always On-Richtlinien werden die Kategorienamen als IDs verwendet. Durch die Änderung des Namens einer AlwaysOn-Kategorie wird deren Funktionalität zur Integritätsüberprüfung unterbrochen. Ändern Sie die Namen von Always On-Kategorien daher nicht.  
  
##  <a name="always-on-dashboard"></a><a name="Dashboard"></a> Always On-Dashboard  
 Das Always On-Dashboard bietet eine Übersicht über die Integrität einer Verfügbarkeitsgruppe. Das Always On-Dashboard umfasst die folgenden Funktionen:  
  
-   Ermöglicht Ihnen, Details zu einer angegebenen Verfügbarkeitsgruppe, zu deren Verfügbarkeitsreplikaten und Datenbanken leicht anzuzeigen.  
  
-   Zeigt visuelle Indikatoren für wichtige Zustände an, um Datenbankadministratoren zu helfen, schnelle Entscheidungen zum Betriebsablauf zu treffen.  
  
-   Stellt Startpunkte für Problembehandlungsszenarien bereit.  
  
-   Für jedes Betriebsproblem wird das Dialogfeld **Ergebnis der Richtlinienauswertung** mit Informationen zu bestimmten Always On-Integritätsrichtlinienverletzungen und Links zur Problemlösungshilfe aufgefüllt.  
  
-   Stellt einen Systemintegritäts-Viewer für erweiterte Ereignisse bereit, um vorherige Ereignisse für Always On-spezifische Probleme anzuzeigen.  
  
-   Wenn die Ausführung eines Failovers für die Verfügbarkeitsgruppe eine mögliche Problembehebung ist, wird ein Startpunkt für die Links[Assistent für das Failover von Verfügbarkeitsgruppen](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md)bereitgestellt. Dieser Assistent führt den Datenbankadministrator durch den manuellen Failoverprozess.  
  
##  <a name="extending-the-always-on-health-model"></a><a name="ExtendHealthModel"></a> Erweitern des Always On-Zustandsmodells  
 Die Erweiterung des [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Zustandsmodells bezieht sich darauf, dass Sie eigene benutzerdefinierte Richtlinien erstellen und diese je nach überwachtem Objekttyp bestimmten Kategorien zuweisen können.  Nachdem Sie einige Einstellungen geändert haben, wertet das Always On-Dashboard automatisch Ihre eigenen benutzerdefinierten Richtlinien sowie die vordefinierten Always On-Richtlinien aus.  
  
 Eine benutzerdefinierte Richtlinie kann beliebige der verfügbaren PBM-Facets verwenden, einschließlich der von vordefinierten Always On-Richtlinien verwendeten Facets (siehe [Vordefinierte Richtlinien und Probleme](#Always OnPBM)weiter oben in diesem Thema). Das Serverfacet stellt die folgenden Eigenschaften zum Überwachen des [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]-Zustands bereit: **IsHadrEnabled** und **HadrManagerStatus**. Das Serverfacet stellt außerdem Eigenschaften der folgenden Richtlinien zum Überwachen der WSFC-Clusterkonfiguration bereit: **ClusterQuorumType** und **ClusterQuorumState**.  
  
 Weitere Informationen finden Sie im SQL Server-Always On-Teamblog unter [The Always On Health Model Part 2 – Extending the Health Model](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-2-extending-the-health-model) (Das Always On-Zustandsmodell Teil 2 – Erweitern des Zustandsmodells).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Verwenden von AlwaysOn-Richtlinien zum Anzeigen des Zustands einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)  
  
-   [Verwenden des AlwaysOn-Dashboards &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
-   [WSFC-Notfallwiederherstellung durch erzwungenes Quorum &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)  
  
-   [Erzwingen des Starts eines Clusters ohne Quorum](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md)  
  
-   [Ausführen eines erzwungenen manuellen Failovers einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Problembehandlung bei einem fehlgeschlagenen Vorgang zum Hinzufügen einer Datei &#40;AlwaysOn-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   [The Always On Health Model Part 1 -- Health Model Architecture (Das Always On-Zustandsmodell Teil 1 – Zustandsmodellarchitektur)](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-1-health-model-architecture)  
  
-   [The Always On Health Model Part 2 -- Extending the Health Model (Das Always On-Zustandsmodell Teil 2 – Erweitern des Zustandsmodells)](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-2-extending-the-health-model)  
  
-   [Microsoft SQL Server Always On-Lösungshandbuch zu hoher Verfügbarkeit und Notfallwiederherstellung](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
## <a name="see-also"></a>Weitere Informationen  
 [Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verwaltung einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server.md)   
 [Überwachen von Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
