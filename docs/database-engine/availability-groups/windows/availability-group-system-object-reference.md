---
title: Systemobjektreferenz für Verfügbarkeitsgruppen
description: Dies ist eine Referenz zu verschiedenen Systemobjekten, die bei der Arbeit mit Always On-Verfügbarkeitsgruppen verwendet werden können.
ms.custom: seo-lt-2019
ms.date: 04/03/2010
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: reference
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: f712ec1fe11f5c975695f15e2d6d6bf4efdf4af5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341534"
---
# <a name="always-on-availability-group-system-object-reference"></a>Systemobjektreferenz für Always On-Verfügbarkeitsgruppen

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]
    
Dieses Thema dient als Referenzseite für die verschiedenen Systemobjekte, die bei der Arbeit mit Always On-Verfügbarkeitsgruppen verwendet werden können. 

## <a name="system-catalog-views"></a>Systemkatalogsichten

| Systemkatalogsicht | BESCHREIBUNG|
| :------ | :----------------------------- |
| [sys.availability_databases_cluster](../../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)   | Enthält eine Zeile für jede Verfügbarkeitsdatenbank in der SQL Server-Instanz, die für jede Always On-Verfügbarkeitsgruppe im WSFC-Cluster (Windows Server-Failoverclustering) ein Verfügbarkeitsreplikat hostet, unabhängig davon, ob die lokale Kopie der Datenbank bereits mit der Verfügbarkeitsgruppe verknüpft wurde. |
| [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  | Gibt eine Zeile für jede IP-Adresse zurück, die einem beliebigen Always On-Verfügbarkeitsgruppenlistener im WSFC-Cluster (Windows Server-Failoverclustering) zugeordnet ist. |
| [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)    | Für jede Always On-Verfügbarkeitsgruppe gilt: Entweder werden null Zeilen zurückgegeben, um anzuzeigen, dass der Verfügbarkeitsgruppe kein Netzwerkname zugeordnet ist, oder für jede Verfügbarkeitsgruppen-Listenerkonfiguration im WSFC-Cluster (Windows Server-Failoverclustering) wird eine Zeile zurückgegeben.  |
| [sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   | Gibt eine Zeile für jede Verfügbarkeitsgruppe zurück, für die die lokale SQL Server-Instanz ein Verfügbarkeitsreplikat hostet. Jede Zeile enthält eine zwischengespeicherte Kopie der Metadaten der Verfügbarkeitsgruppe. |
| [sys.availability_groups_cluster](../../../relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md)    | Gibt eine Zeile für jede Always On-Verfügbarkeitsgruppe im WSFC-Cluster (Windows Server-Failoverclustering) zurück. Jede Zeile enthält die Metadaten der Verfügbarkeitsgruppe aus dem WSFC-Cluster. |
| [sys.availability_read_only_routing_lists](../../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)    | Gibt eine Zeile für die schreibgeschützte Routingliste aller Verfügbarkeitsreplikate zurück, die zu einer Always On-Verfügbarkeitsgruppe im WSFC-Failovercluster gehören. |
| [sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)    | Gibt eine Zeile für jedes Verfügbarkeitsreplikat zurück, das zu einer Always On-Verfügbarkeitsgruppe im WSFC-Failovercluster gehört. |
| &nbsp; | &nbsp; |

## <a name="system-dynamic-management-views"></a>Dynamische Systemverwaltungssichten


| Dynamische Systemverwaltungssicht | BESCHREIBUNG|
| :------ | :----------------------------- |
| [sys.dm_hadr_auto_page_repair](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-auto-page-repair-transact-sql.md)   | Gibt eine Zeile für jede versuchte automatische Seitenreparatur in einer beliebigen Verfügbarkeitsdatenbank auf einem Verfügbarkeitsreplikat zurück, das von der Serverinstanz für eine beliebige Verfügbarkeitsgruppe gehostet wird.  |
| [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)    | Gibt eine Zeile für jede Always On-Verfügbarkeitsgruppe zurück, die ein Verfügbarkeitsreplikat in der lokalen SQL Server-Instanz besitzt. In jede Zeile werden die Statuswerte angezeigt, die den Zustand einer angegebenen Verfügbarkeitsgruppe definieren. |
| [sys.dm_hadr_availability_replica_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-nodes-transact-sql.md)     | Gibt eine Zeile für jedes Verfügbarkeitsreplikat (unabhängig vom Joinzustand) der Always On-Verfügbarkeitsgruppen im WSFC-Cluster (Windows Server Failover Clustering) zurück. |
| [sys.dm_hadr_availability_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md)     | Gibt eine Zeile für jedes Always On-Verfügbarkeitsreplikat (unabhängig vom Joinzustand) aller Always On-Verfügbarkeitsgruppen (unabhängig vom Replikatspeicherort) im WSFC-Cluster (Windows Server Failover Clustering) zurück. |
| [sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)    | Gibt eine Zeile für jedes lokale Replikat und eine Zeile für jedes Remotereplikat zurück, das sich in derselben Always On-Verfügbarkeitsgruppe wie ein lokales Replikat befindet. Jede Zeile enthält Informationen zum Zustand eines angegebenen Replikats. |
| [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md)    | Gibt eine Zeile zurück, die den Clusternamen und Informationen zum Quorum verfügbar macht. |
| [sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)    | Gibt eine Zeile für jedes Element, das an der Quorumbildung beteiligt ist, und den Zustand jedes Elements zurück. |
| [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)    | Gibt eine Zeile für jedes WSFC-Clusterelement zurück, das an der Subnetzkonfiguration einer Verfügbarkeitsgruppe beteiligt ist.  |
| [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md)     | Gibt eine Zeile mit Informationen zurück, die Aufschluss über die Integrität der Verfügbarkeitsdatenbanken in den Always On-Verfügbarkeitsgruppen in jeder Always On-Verfügbarkeitsgruppe im WSFC-Cluster (Windows Server Failover Clustering) geben sollen.  |
| [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)    | Gibt eine Zeile für jede Datenbank zurück, die an einer Always On-Verfügbarkeitsgruppe teilnimmt, für die die lokale SQL Server-Instanz ein Verfügbarkeitsreplikat hostet. |
| [sys.dm_hadr_instance_node_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-instance-node-map-transact-sql.md)    | Gibt für jede SQL Server-Instanz, die ein Verfügbarkeitsreplikat hostet, das mit seiner Always On-Verfügbarkeitsgruppe verknüpft ist, den Namen des WSFC-Knotens (Windows Server-Failovercluster) zurück, der die Serverinstanz hostet. |
| [sys.dm_hadr_name_id_map](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-name-id-map-transact-sql.md)    | Zeigt die Zuordnung von AlwaysOn-Verfügbarkeitsgruppen an, die die aktuelle Instanz von SQL Server mit drei eindeutigen IDs verknüpft hat: eine Verfügbarkeitsgruppen-ID, eine WSFC-Ressourcen-ID und eine WSFC-Gruppen-ID. |
| [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)    | Gibt eine Zeile zurück, die Informationen zum dynamischen Status für jeden TCP-Listener enthält. |
| &nbsp; | &nbsp; |

## <a name="system-functions"></a>Systemfunktionen


| Systemfunktion | BESCHREIBUNG|
| :------ | :----------------------------- |
| [sys.fn_hadr_is_primary_replica](../../../relational-databases/system-functions/sys-fn-hadr-is-primary-replica-transact-sql.md)  | Dient zum Ermitteln, ob das aktuelle Replikat das primäre Replikat ist. |
| [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)    | Dient zum Ermitteln, ob das aktuelle Replikat das bevorzugte Sicherungsreplikat ist. |
| [sys.fn_hadr_distributed_ag_replica](../../../relational-databases/system-functions/sys-fn-hadr-distributed-ag-replica-transact-sql.md)    | Wird verwendet, um ein Replikat in einer verteilten Verfügbarkeitsgruppe der lokalen Verfügbarkeitsgruppe zuzuordnen. |
| &nbsp; | &nbsp; |


  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht zu AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   

  
