---
title: Anzeigen von Verfügbarkeitsreplikateigenschaften
description: Hier finden Sie Anweisungen zum Anzeigen der Eigenschaften eines Verfügbarkeitsgruppenreplikats in SQL Server Management Studio (SSMS), Transact-SQL (T-SQL) oder SQL PowerShell.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- ', policies'
ms.assetid: 14fed3c4-8ecc-4e1c-931d-a7ec1e9f9e90
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7cbe19e53cd263d4831c9afe2656a2627e5139f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353260"
---
# <a name="view-availability-replica-properties-sql-server"></a>Anzeigen von Verfügbarkeitsreplikateigenschaften (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie die Eigenschaften eines Verfügbarkeitsreplikats für eine Always On-Verfügbarkeitsgruppe unter Verwendung von [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../../includes/tsql-md.md)] in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]angezeigt werden.  
  
 
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So zeigen Sie die Eigenschaften eines Verfügbarkeitsreplikats an und ändern sie**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die das primäre Verfügbarkeitsreplikat hostet, und erweitern Sie die Serverstruktur.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Erweitern Sie die Verfügbarkeitsgruppe, zu der das Verfügbarkeitsreplikat gehört, und erweitern Sie den Knoten **Verfügbarkeitsreplikate** .  
  
4.  Klicken Sie mit der rechten Maustaste auf das Verfügbarkeitsreplikat, dessen Eigenschaften Sie anzeigen möchten, und wählen Sie den Befehl **Eigenschaften** aus.  
  
5.  Verwenden Sie im Dialogfeld **Eigenschaften des Verfügbarkeitsreplikats** die Seite **Allgemein** , um die Eigenschaften dieses Replikats anzuzeigen. Wenn eine Verbindung mit dem primären Replikat hergestellt wurde, können Sie die folgenden Eigenschaften ändern: Verfügbarkeitsmodus, Failovermodus, Verbindungszugriff für die primäre Rolle, Lesezugriff für die sekundäre Rolle (lesbares sekundäres Replikat) und Wert des Sitzungstimeouts. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Eigenschaften des Verfügbarkeitsreplikats &#40;Seite Allgemein&#41;](../../../database-engine/availability-groups/windows/availability-replica-properties-general-page.md).  

   [!NOTE]
   >Wenn der Clustertyp „NONE“ ist, können Sie den Failovermodus nicht ändern.
  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So zeigen Sie die Eigenschaften und den Status von Verfügbarkeitsreplikaten an**  
  
 Um die Eigenschaften und den Status von Verfügbarkeitsreplikaten anzuzeigen, verwenden Sie die folgenden Sichten und die folgende Systemfunktion:  
  
 [sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)  
 Gibt eine Zeile für jedes Verfügbarkeitsreplikat in jeder Verfügbarkeitsgruppe zurück, für die die lokale Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ein Verfügbarkeitsreplikat hostet.  
  
 **Spaltennamen:** replica_id, group_id, replica_metadata_id, replica_server_name, owner_sid, endpoint_url, availability_mode, availability_mode_desc, failover_mode, failover_mode_desc, session_timeout, primary_role_allow_connections, primary_role_allow_connections_desc, secondary_role_allow_connections, secondary_role_allow_connections_desc, create_date, modify_date, backup_priority, read_only_routing_url  
  
 [sys.availability_read_only_routing_lists](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
 Gibt eine Zeile für die schreibgeschützte Routingliste aller Verfügbarkeitsreplikate zurück, die zu einer Always On-Verfügbarkeitsgruppe im WSFC-Failovercluster gehören.  
  
 **Spaltennamen:** replica_id, routing_priority, read_only_replica_id  
  
 [sys.dm_hadr_availability_replica_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)  
 Gibt eine Zeile für jedes Verfügbarkeitsreplikat (unabhängig vom Joinzustand) der Always On-Verfügbarkeitsgruppen im WSFC-Cluster (Windows Server Failover Clustering) zurück.  
  
 **Spaltennamen:** group_name, replica_server_name, node_name  
  
 [sys.dm_hadr_availability_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
 Gibt eine Zeile für jedes Replikat (unabhängig vom Joinzustand) aller Always On-Verfügbarkeitsgruppen (unabhängig von Replikatspeicherort) im WSFC-Cluster (Windows Server-Failoverclustering) zurück.  
  
 **Spaltennamen:** replica_id, replica_server_name, group_id, join_state, join_state_desc  
  
 [sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)  
 Gibt eine Zeile mit dem Status jedes lokalen Verfügbarkeitsreplikats und eine Zeile für jedes Remoteverfügbarkeitsreplikat in derselben Verfügbarkeitsgruppe zurück.  
  
 **Spaltennamen:** replica_id, group_id, is_local, role, role_desc, operational_state, operational_state_desc, connected_state, connected_state_desc, recovery_health, recovery_health_desc, synchronization_health, synchronization_health_desc, last_connect_error_number, last_connect_error_description und last_connect_error_timestamp  
  
 [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
 Bestimmt, ob das aktuelle Replikat das bevorzugte Sicherungsreplikat ist. Gibt 1 zurück, wenn die Datenbank auf der aktuellen Serverinstanz das bevorzugte Replikat ist. Andernfalls wird 0 zurückgegeben.  
  
> [!NOTE]  
>  Weitere Informationen zu Leistungsindikatoren für Verfügbarkeitsreplikate (das **SQLServer:Verfügbarkeitsreplikat**  -Leistungsobjekt) finden Sie unter [SQL Server, Verfügbarkeitsreplikat](../../../relational-databases/performance-monitor/sql-server-availability-replica.md).  
  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **So zeigen Sie Informationen zu Verfügbarkeitsgruppen an**  
  
-   [Anzeigen von Verfügbarkeitsgruppeneigenschaften &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)  
  
-   [Anzeigen von Eigenschaften des Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)  
  
-   [Always On-Richtlinien für Betriebsprobleme mit Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)  
  
-   [Verwenden des AlwaysOn-Dashboards &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
-   [Überwachen von Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
 **So verwalten Sie Verfügbarkeitsreplikate**  
  
-   [Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Konfigurieren des schreibgeschützten Zugriffs auf ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Ändern des Verfügbarkeitsmodus eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Ändern des Failovermodus eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Ändern des Sitzungstimeouts für ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
-   [Entfernen einer sekundären Replikats aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
 **So verwalten Sie eine Verfügbarkeitsdatenbank**  
  
-   [Hinzufügen einer Datenbank zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)  
  
-   [Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Anhalten einer Verfügbarkeitsdatenbank &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/suspend-an-availability-database-sql-server.md)  
  
-   [Fortsetzen einer Verfügbarkeitsdatenbank &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)  
  
-   [Entfernen einer sekundären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
-   [Entfernen einer primären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)  
  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Überwachen von Verfügbarkeitsgruppen (Transact-SQL)](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Always On Policies for Operational Issues with Always On Availability Groups (SQL Server) (Always On-Richtlinien für Betriebsprobleme mit Always On-Verfügbarkeitsgruppen (SQL Server))](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)   
 [Verwaltung einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server.md)  
  
  
