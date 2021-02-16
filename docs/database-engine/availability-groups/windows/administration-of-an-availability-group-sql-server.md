---
title: Verwaltung einer Verfügbarkeitsgruppe (Inhaltsverzeichnis)
description: Eine Referenzverzeichnis mit Links zu grundlegenden Informationen zu Verwaltungsoptionen für Always On-Verfügbarkeitsgruppen wie der Änderung von Eigenschaften, dem Hinzufügen oder Entfernen von Replikaten, dem Hinzufügen oder Entfernen von Datenbanken, dem Ausführen von Failovers oder dem Konfigurieren des Listeners.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
helpviewer_keywords:
- Availability Groups [SQL Server], managing
ms.assetid: 0b7542fa-235e-413d-81bf-3eff9ee07480
author: cawrites
ms.author: chadam
ms.openlocfilehash: 27cce2f1be58688e3cf4156206650931c060d01a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343907"
---
# <a name="administration-of-an-availability-group"></a>Verwalten einer Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
 Die Verwaltung einer vorhandenen Always On-Verfügbarkeitsgruppe in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] schließt folgende Tasks ein:  
  
-   Ändern der Eigenschaften eines vorhandenen Verfügbarkeitsreplikats, z. B. Ändern des Clientverbindungszugriffs (zum Konfigurieren lesbarer sekundärer Replikate), des Failovermodus, Verfügbarkeitsmodus oder der Timeouteinstellung für die Sitzung    
-   Hinzufügen oder Entfernen sekundärer Replikate    
-   Hinzufügen oder Entfernen einer Datenbank    
-   Anhalten oder Fortsetzen einer Datenbank   
-   Ausführen eines geplanten manuellen Failovers ( *manuelles Failover*) oder eines erzwungenen manuellen Failovers ( *erzwungenes Failover*)    
-   Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners    
-   Verwalten [lesbarer sekundärer Replikate](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md) für eine angegebene Verfügbarkeitsgruppe. Dies umfasst die Konfiguration eines oder mehrerer Replikate für den schreibgeschütztem Zugriff unter der sekundären Rolle sowie die Konfiguration von schreibgeschütztem Routing.    
-   Verwalten von [Sicherungen auf sekundären Replikaten](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md) für eine angegebene Verfügbarkeitsgruppe. Dies umfasst die Konfiguration des bevorzugten Orts für die Ausführung von Sicherungsaufträgen und die anschließende Skripterstellung für Sicherungsaufträge, um die Sicherungseinstellung zu implementieren. Sie müssen Sicherungsauftragsskripts für jede Datenbank in der Verfügbarkeitsgruppe auf jeder Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] erstellen, die ein Verfügbarkeitsreplikat hostet.    
-   Löschen einer Verfügbarkeitsgruppe    
-   Clusterübergreifende Migration von Always On-Verfügbarkeitsgruppen für Betriebssystemupgrade  
  
## <a name="configure-an-existing-availability-group"></a>Konfigurieren einer vorhandenen Verfügbarkeitsgruppe
  
-   [Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Entfernen einer sekundären Replikats aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [Hinzufügen einer Datenbank zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)    
-   [Entfernen einer sekundären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
-   [Entfernen einer primären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)  
  
-   [Konfigurieren der flexiblen Failoverrichtlinie zum Steuern der Bedingungen für ein automatisches Failover &#40;AlwaysOn-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
 ## <a name="manage-an-availability-group"></a>Verwalten einer Verfügbarkeitsgruppe  
  
-   [Konfigurieren der Sicherung auf Verfügbarkeitsreplikaten &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [Ausführen eines geplanten manuellen Failovers einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Ausführen eines erzwungenen manuellen Failovers einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Entfernen einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)  
  
 ## <a name="manage-an-availability-replica"></a>Verwalten eines Verfügbarkeitsreplikats  
  
-   [Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Entfernen einer sekundären Replikats aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [Ändern des Verfügbarkeitsmodus eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Ändern des Failovermodus eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Konfigurieren der Sicherung auf Verfügbarkeitsreplikaten &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [Konfigurieren des schreibgeschützten Zugriffs auf ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Konfigurieren des schreibgeschützten Routing für eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Ändern des Sitzungstimeouts für ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
## <a name="manage-an-availability-database"></a>Verwalten einer Verfügbarkeitsdatenbank  
  
-   [Hinzufügen einer Datenbank zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)  
  
-   [Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Entfernen einer primären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)  
  
-   [Entfernen einer sekundären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
-   [Anhalten einer Verfügbarkeitsdatenbank &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/suspend-an-availability-database-sql-server.md)  
  
-   [Fortsetzen einer Verfügbarkeitsdatenbank &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)  
  
## <a name="monitor-an-availability-group"></a>Überwachen einer Verfügbarkeitsgruppe
  
-   [Überwachen von Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
 ## <a name="support-migrating-availability-groups-to-a-new-wsfc-cluster-cross-cluster-migration"></a>Unterstützen des Migrierens von Verfügbarkeitsgruppen zu einem neuen WSFC-Cluster (clusterübergreifende Migration)
  
-   [Ändern des HADR-Clusterkontexts der Serverinstanz &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)  
  
-   [Offlineschalten einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/take-an-availability-group-offline-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   **Blogs:**  
  
     [SQL Server Always On Team Blogs: The official SQL Server Always On Team Blog (SQL Server Always On-Teamblog: Der offizielle SQL Server Always On-Teamblog)](/archive/blogs/sqlalwayson/)    
[CSS SQL Server-Technikblogs](/archive/blogs/psssql/)  
  
-   **Videos:**  
  
     [Microsoft SQL Server Code-Named „Denali“ Always On Series, Part 1: Introducing the Next Generation High Availability Solution (Microsoft SQL Server, Codename „Denali“ – Always On-Reihe, Teil 1: Einführung in die nächste Generation von Lösungen mit Hochverfügbarkeit)](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)    
     [Microsoft SQL Server Code-Named „Denali“ Always On Series, Part 2: Building a Mission-Critical High Availability Solution Using Always On (Microsoft SQL Server, Codename „Denali“ – Always On-Reihe, Teil 2: Erstellen einer unternehmenskritischen Lösung mit Hochverfügbarkeit mit Always On)](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **Whitepaper:**  
  
     [Microsoft White Papers for SQL Server 2012 (Microsoft-Whitepapers zu SQL Server 2012)](https://msdn.microsoft.com/library/hh403491.aspx)    
     [Whitepapers des SQL Server-Kundenberatungsteams](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Konfiguration einer Serverinstanz für Always On-Verfügbarkeitsgruppen (SQL Server)](../../../database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md)   
 [Erstellung und Konfiguration von Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Aktive sekundäre Replikate: Lesbare sekundäre Replikate &#40;Always On-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Aktive sekundäre Replikate: Sicherung auf sekundären Replikaten &#40;Always On-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)   
 [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Always On Policies for Operational Issues with Always On Availability Groups (SQL Server) (Always On-Richtlinien für Betriebsprobleme mit Always On-Verfügbarkeitsgruppen (SQL Server))](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)   
 [Überwachen von Verfügbarkeitsgruppen (SQL Server)](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [Always On-Verfügbarkeitsgruppen: Interoperabilität &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [Übersicht über Transact-SQL-Anweisungen für Always On-Verfügbarkeitsgruppen (SQL Server)](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md)   
 [Übersicht der PowerShell-Cmdlets für Always On-Verfügbarkeitsgruppen &#40;SQLServer&#41;](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md)  
  
