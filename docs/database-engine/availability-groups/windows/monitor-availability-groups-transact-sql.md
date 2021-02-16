---
title: Überwachen von Verfügbarkeitsgruppen mit Transact-SQL (T-SQL)
description: Verwenden Sie SELECT-Anweisungen von Transact-SQL, um AlwaysOn-Verfügbarkeitsgruppen und ihre Replikate und Datenbanken zu überwachen.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- dynamic management views [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], listeners
- Availability Groups [SQL Server], databases
- catalog views [SQL Server], AlwaysOn Availability Groups
ms.assetid: 881a34de-8461-4811-8c62-322bf7226bed
author: cawrites
ms.author: chadam
ms.openlocfilehash: d2bccd9808ad397d7cc83285577aea3483b253c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352659"
---
# <a name="monitor-availability-groups-transact-sql"></a>Überwachen von Verfügbarkeitsgruppen (Transact-SQL)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Zum Überwachen von Verfügbarkeitsgruppen und -replikaten und den zugeordneten Datenbanken mit [!INCLUDE[tsql](../../../includes/tsql-md.md)]stellt [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] einen Satz von Katalogsichten und dynamischen Verwaltungssichten sowie Servereigenschaften bereit. Mit [!INCLUDE[tsql](../../../includes/tsql-md.md)] SELECT-Anweisungen können Sie mithilfe der Sichten Verfügbarkeitsgruppen und ihre Replikate und Datenbanken überwachen. Die für eine bestimmte Verfügbarkeitsgruppe zurückgegebenen Informationen hängen davon ab, ob Sie mit der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] verbunden sind, die das primäre Replikat oder ein sekundäres Replikat hostet.  
  
> [!TIP]  
>  Viele dieser Sichten können mithilfe ihre ID-Spalten verknüpft werden, um Informationen aus mehreren Sichten in einer einzelnen Abfrage zurückzugeben.  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Katalogsichten erfordern die VIEW ANY DEFINITION-Berechtigung für die Serverinstanz. [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Verwaltungssichten erfordern die VIEW SERVER STATE-Berechtigung für den Server.  
  
##  <a name="monitoring-the-always-on-availability-groups-feature-on-a-server-instance"></a><a name="AoAgFeatureOnSI"></a> Überwachen der Funktion AlwaysOn-Verfügbarkeitsgruppen auf einer Serverinstanz  
 Verwenden Sie zum Überwachen der [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Funktion auf einer Serverinstanz folgende integrierte Funktion:  
  
 [SERVERPROPERTY](../../../t-sql/functions/serverproperty-transact-sql.md) -Funktion  
 Gibt Server-Eigenschaftsinformationen dazu zurück, ob [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] aktiviert ist, und falls ja, ob es auf der Serverinstanz gestartet wurde.  
  
 **Spaltennamen:** IsHadrEnabled, HadrManagerStatus  
  
##  <a name="monitoring-availability-groups-on-the-wsfc-cluster"></a><a name="WSFC"></a> Überwachen von Verfügbarkeitsgruppen auf dem WSFC-Cluster  
 Verwenden Sie zum Überwachen des WSFC-Clusters (Windows Server-Failoverclustering), der eine für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]aktivierte lokale Serverinstanz hostet, die folgenden Sichten:  
  
 [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)  
 Wenn der WSFC-Knoten (Windows Server-Failoverclustering), der eine Instanz von SQL Server hostet, für die [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] aktiviert ist, ein WSFC-Quorum hat, gibt **sys.dm_hadr_cluster** eine Zeile zurück, die den Clusternamen und Informationen zum Quorum verfügbar macht. Wenn der WSFC-Knoten nicht über ein Quorum verfügt, werden keine Zeilen zurückgegeben.  
  
 **Spaltennamen:** cluster_name, quorum_type, quorum_type_desc, quorum_state, quorum_state_desc  
  
 [sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)  
 Wenn der WSFC-Knoten, der die lokale AlwaysOn-fähige Instanz von SQL Server hostet, über ein WSFC-Quorum verfügt, wird eine Zeile für jedes Element, aus denen das Quorum besteht, einschließlich Elementstatus, zurückgegeben.  
  
 **Spaltennamen:** member_name, member_type, member_type_desc, member_state, member_state_desc, number_of_quorum_votes  
  
 [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)  
 Gibt eine Zeile für jedes Element zurück, das an der Subnetzkonfiguration einer Verfügbarkeitsgruppe beteiligt ist. Sie können diese dynamische Verwaltungssicht verwenden, um die virtuelle IP-Adresse des Netzwerks zu überprüfen, die für jedes Verfügbarkeitsreplikat konfiguriert ist.  
  
 **Spaltennamen:** member_name, network_subnet_ip, network_subnet_ipv4_mask, network_subnet_prefix_length, is_public, is_ipv4  
  
 **Primärschlüssel:** member_name + network_subnet_IP + network_subnet_prefix_length  
  
 [sys.dm_hadr_instance_node_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)  
 Jede Instanz von SQL Server, auf der ein Verfügbarkeitsreplikat gehostet wird, das mit seiner AlwaysOn-Verfügbarkeitsgruppe verknüpft ist, gibt den Namen des WSFC-Knotens (Windows Server Failover Clustering) zurück, auf dem die Serverinstanz gehostet wird. Die dynamische Verwaltungssicht dient für Folgendes:  
  
-   Diese dynamische Verwaltungssicht ist nützlich für das Erkennen einer Verfügbarkeitsgruppe mit mehreren Verfügbarkeitsreplikaten, die im selben WSFC-Knoten gehostet werden, der eine nicht unterstützte Konfiguration ist. Letztere könnte nach einem FCI-Failover auftreten, wenn die Verfügbarkeitsgruppe falsch konfiguriert wird.  
  
-   Wenn mehrere SQL Server-Instanzen auf dem gleichen WSFC-Knoten gehostet werden, verwendet die Ressourcen-DLL diese dynamische Verwaltungssicht, um die Instanz von SQL Server zu bestimmen, um damit eine Verbindung herzustellen.  
  
 **Spaltennamen:** ag_resource_id, instance_name, node_name  
  
 [sys.dm_hadr_name_id_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)  
 Zeigt die Zuordnung von AlwaysOn-Verfügbarkeitsgruppen an, die die aktuelle Instanz von SQL Server mit drei eindeutigen IDs verknüpft hat: eine Verfügbarkeitsgruppen-ID, eine WSFC-Ressourcen-ID und eine WSFC-Gruppen-ID. Der Zweck dieser Zuordnung ist, das Szenario zu behandeln, in dem die WSFC-Ressource/Gruppe umbenannt wird.  
  
 **Spaltennamen:** ag_name, ag_id, ag_resource_id, ag_group_id  
  
> [!NOTE]  
>  Siehe auch **sys.dm_hadr_availability_replica_cluster_nodes** und **sys.dm_hadr_availability_replica_cluster_states** im Abschnitt [Überwachen von Verfügbarkeitsreplikaten](#AvReplicas) sowie **sys.availability_databases_cluster** und **sys.dm_hadr_database_replica_cluster_states** im Abschnitt [Überwachen von Verfügbarkeitsdatenbanken](#AvDbs) weiter unten in diesem Thema.  
  
 Informationen zu WSFC-Cluster und [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] finden Sie unter [Windows Server-Failoverclustering &#40;WSFC&#41; mit SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md) und [Failoverclustering und AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md).  
  
##  <a name="monitoring-availability-groups"></a><a name="AvGroups"></a> Überwachen von Verfügbarkeitsgruppen  
 Verwenden Sie zum Überwachen der Verfügbarkeitsgruppen, für die die Serverinstanz ein Verfügbarkeitsreplikat hostet, die folgenden Sichten:  
  
 [sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)  
 Gibt eine Zeile für jede Verfügbarkeitsgruppe zurück, für die die lokale Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ein Verfügbarkeitsreplikat hostet. Jede Zeile enthält eine zwischengespeicherte Kopie der Metadaten der Verfügbarkeitsgruppe.  
  
 **Spaltennamen:** group_id, name, resource_id, resource_group_id, failure_condition_level, health_check_timeout, automated_backup_preference, automated_backup_preference_desc  
  
 [sys.availability_groups_cluster](../../../relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md)  
 Gibt eine Zeile für jede Verfügbarkeitsgruppe im WSFC-Cluster zurück. Jede Zeile enthält die Verfügbarkeitsgruppenmetadaten vom WSFC-Cluster (Windows Server-Failoverclustering).  
  
 **Spaltennamen:** group_id, name, resource_id, resource_group_id, failure_condition_level, health_check_timeout, automated_backup_preference, automated_backup_preference_desc  
  
 [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)  
 Gibt eine Zeile für jede Verfügbarkeitsgruppe zurück, die ein Verfügbarkeitsreplikat in der lokalen Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]besitzt. In jede Zeile werden die Statuswerte angezeigt, die den Zustand einer angegebenen Verfügbarkeitsgruppe definieren.  
  
 **Spaltennamen:** group_id, primary_replica, primary_recovery_health, primary_recovery_health_desc, secondary_recovery_health, secondary_recovery_health_desc, synchronization_health, synchronization_health_desc  
  
##  <a name="monitoring-availability-replicas"></a><a name="AvReplicas"></a> Überwachen von Verfügbarkeitsreplikaten  
 Verwenden Sie zum Überwachen von Verfügbarkeitsreplikaten die folgenden Sichten und Systemfunktion:  
  
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
 Bestimmt, ob das aktuelle Replikat das bevorzugte Sicherungsreplikat ist.  
  
> [!NOTE]  
>  Weitere Informationen zu Leistungsindikatoren für Verfügbarkeitsreplikate (das **SQLServer:Verfügbarkeitsreplikat**  -Leistungsobjekt) finden Sie unter [SQL Server, Verfügbarkeitsreplikat](../../../relational-databases/performance-monitor/sql-server-availability-replica.md).  
  
##  <a name="monitoring-availability-databases"></a><a name="AvDbs"></a> Überwachen von Verfügbarkeitsdatenbanken  
 Verwenden Sie zum Überwachen von Verfügbarkeitsdatenbanken die folgenden Sichten:  
  
 [sys.availability_databases_cluster](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)  
 Enthält eine Zeile für jede Datenbank in der Instanz von SQL Server, die Teil aller AlwaysOn-Verfügbarkeitsgruppen im Cluster ist, unabhängig davon, ob die lokale Kopie der Datenbank bereits mit der Verfügbarkeitsgruppe verknüpft wurde.  
  
> [!NOTE]  
>  Wenn eine Datenbank einer Verfügbarkeitsgruppe hinzugefügt wird, wird die primäre Datenbank automatisch mit der Gruppe verknüpft. Sekundäre Datenbanken müssen auf jedem sekundären Replikat vorbereitet werden, bevor sie mit der Verfügbarkeitsgruppe verknüpft werden können.  
  
 **Spaltennamen:** group_id, group_database_id, database_name  
  
 [sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
 Enthält eine Zeile für jede Datenbank in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz. Wenn eine Datenbank zu einem Verfügbarkeitsreplikat gehört, zeigt die Zeile für diese Datenbank die GUID des Replikats und den eindeutigen Bezeichner der Datenbank innerhalb der Verfügbarkeitsgruppe an.  
  
 **[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Spaltennamen:** replica_id, group_database_id  
  
 [sys.dm_hadr_auto_page_repair](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)  
 Gibt eine Zeile für jede versuchte automatische Seitenreparatur in einer beliebigen Verfügbarkeitsdatenbank auf einem Verfügbarkeitsreplikat zurück, das von der Serverinstanz für eine beliebige Verfügbarkeitsgruppe gehostet wird. Diese Sicht enthält Zeilen für die letzte automatische Seitenreparatur einer bestimmten primären oder sekundären Datenbank. Pro Datenbank können maximal 100 Zeilen angezeigt werden. Sobald das Maximum in der Datenbank erreicht ist, ersetzt die Zeile bei der nächsten automatischen Seitenreparatur einen der bereits vorhandenen Einträge.  
  
 **Spaltennamen:** database_id, file_id, page_id, error_type, page_status, modification_time  
  
 [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
 Gibt eine Zeile für jede Datenbank zurück, die an einer Verfügbarkeitsgruppe teilnimmt, für die die lokale Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ein Verfügbarkeitsreplikat hostet.  
  
 **Spaltennamen:** database_id, group_id, replica_id, group_database_id, is_local, synchronization_state, synchronization_state_desc, is_commit_participant, synchronization_health, synchronization_health_desc, database_state, database_state_desc, is_suspended, suspend_reason, suspend_reason_desc, recovery_lsn, truncation_lsn, last_sent_lsn, last_sent_time, last_received_lsn, last_received_time, last_hardened_lsn, last_hardened_time, last_redone_lsn, last_redone_time, log_send_queue_size, log_send_rate, redo_queue_size, redo_rate, filestream_send_rate, end_of_log_lsn, last_commit_lsn, last_commit_time, low_water_mark_for_ghosts  
  
 [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
 Gibt eine Zeile mit Informationen zurück, die einen Einblick in den Zustand der Verfügbarkeitsdatenbanken aller Verfügbarkeitsgruppen auf dem WSFC-Cluster (Windows Server-Failoverclustering) geben. Diese dynamische Verwaltungssicht ist nützlich beim Planen oder Reagieren auf ein Failover oder zum Ermitteln des sekundären Replikats in einer Verfügbarkeitsgruppe, das die Protokollkürzung in einer bestimmten primären Datenbank aufhält.  
  
 **Spaltennamen:** replica_id, group_database_id, database_name, is_failover_ready, is_pending_secondary_suspend, is_database_joined, recovery_lsn, truncation_lsn  
  
> [!NOTE]  
>  Der primäre Replikatspeicherort ist die autoritative Quelle für eine Verfügbarkeitsgruppe.  
  
> [!NOTE]  
>  Informationen zu den [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Leistungsindikatoren für Verfügbarkeitsdatenbanken (das **SQLServer:Datenbankreplikat** -Leistungsobjekt) finden Sie unter [SQL Server, Datenbankreplikat](../../../relational-databases/performance-monitor/sql-server-database-replica.md). Verwenden Sie zum Überwachen der Transaktionsprotokollaktivität in Verfügbarkeitsdatenbanken die folgenden Indikatoren des **SQLServer:Datenbanken**-Leistungsobjekts: **Schreibzeit für Protokollleerungen (ms)** , **Protokollleerungen/Sekunde**, **Protokollpool-Cachefehler/Sekunde**, **Protokollpool-Datenträgerlesevorgänge/Sekunde** und **Protokollpoolanforderungen/Sekunde**. Weitere Informationen finden Sie unter [SQL Server, Databases Object](../../../relational-databases/performance-monitor/sql-server-databases-object.md).  
  
##  <a name="monitoring-availability-group-listeners"></a><a name="AGlisteners"></a> Überwachen von Verfügbarkeitsgruppenlistenern  
 Zum Überwachen der Verfügbarkeitsgruppenlistener auf Subnetzen des WSFC-Clusters verwenden Sie die folgenden Sichten:  
  
 [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  
 Gibt eine Zeile für jede konforme virtuelle IP-Adresse zurück, die derzeit für einen Verfügbarkeitsgruppenlistener online ist.  
  
 **Spaltennamen:** listener_id, ip_address, ip_subnet_mask, is_dhcp, network_subnet_ip, network_subnet_prefix_length, network_subnet_ipv4_mask, state, state_desc  
  
 [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)  
 Gibt für eine angegebene Verfügbarkeitsgruppe entweder 0 Zeilen zurück, um anzugeben, dass der Verfügbarkeitsgruppe kein Netzwerkname zugeordnet ist, oder eine Zeile für jede Verfügbarkeitsgruppen-Listenerkonfiguration im WSFC-Cluster.  
  
 **Spaltennamen:** group_id, listener_id, dns_name, port, is_conformant, ip_configuration_string_from_cluster  
  
 [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)  
 Gibt eine Zeile zurück, die Informationen zum dynamischen Status für jeden TCP-Listener enthält.  
  
 **Spaltennamen:** listener_id, ip_address, is_ipv4, port, type, type_desc, state, state_desc, start_time  
  
 **Primärschlüssel:** listener_id  
  
 Informationen zu Verfügbarkeitsgruppenlistenern finden Sie unter [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **Überwachungsaufgaben für AlwaysOn-Verfügbarkeitsgruppen:**  
  
-   [Verwenden der Details zum Objekt-Explorer zum Überwachen von Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md)  
  
-   [Anzeigen von Verfügbarkeitsgruppeneigenschaften &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)  
  
-   [Anzeigen von Verfügbarkeitsreplikateigenschaften &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [Anzeigen von Eigenschaften des Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)  
  
 **Referenz zum Überwachen von AlwaysOn-Verfügbarkeitsgruppen (Transact-SQL):**  
  
-   [SERVERPROPERTY &#40;Transact-SQL&#41;](../../../t-sql/functions/serverproperty-transact-sql.md)  
  
-   [sys.availability_group_listener_ip_addresses &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  
  
-   [sys.availability_group_listeners &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)  
  
-   [sys.availability_databases_cluster &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)  
  
-   [sys.availability_groups &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)  
  
-   [sys.availability_read_only_routing_lists &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)  
  
-   [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_nodes &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
  
-   [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
-   [sys.dm_hadr_auto_page_repair &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)  
  
-   [sys.dm_hadr_availability_group_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_availability_replica_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_cluster &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)  
  
-   [sys.dm_hadr_cluster_members &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)  
  
-   [sys.dm_hadr_cluster_networks &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_cluster_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)  
  
-   [sys.dm_hadr_database_replica_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)  
  
-   [sys.dm_hadr_instance_node_map &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)  
  
-   [sys.dm_hadr_name_id_map &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)  
  
-   [sys.dm_os_performance_counters &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)  
  
-   [sys.dm_tcp_listener_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)  
  
-   [sys.fn_hadr_backup_is_preferred_replica &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)  
  
 **AlwaysOn-Leistungsindikatoren:**  
  
-   [SQL Server, Verfügbarkeitsreplikat](../../../relational-databases/performance-monitor/sql-server-availability-replica.md)  
  
-   [SQL Server, Datenbankreplikat](../../../relational-databases/performance-monitor/sql-server-database-replica.md)  
  
-   [SQL Server, Databases Object](../../../relational-databases/performance-monitor/sql-server-databases-object.md)  
  
 **Richtlinienbasierte Verwaltung für AlwaysOn-Verfügbarkeitsgruppen**  
  
-   [Verwenden von AlwaysOn-Richtlinien zum Anzeigen des Zustands einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)  
  
-   [Verwenden des AlwaysOn-Dashboards &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Überwachen von Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
  
