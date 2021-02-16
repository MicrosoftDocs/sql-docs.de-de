---
title: Dynamische Verwaltungssichten und Systemkatalogsichten für Verfügbarkeitsgruppen
description: Hier finden Sie eine Sammlung der dynamischen Verwaltungs- und Katalogsichten, mit denen Sie die Integrität von Always On-Verfügbarkeitsgruppen überwachen und diagnostizieren können.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: troubleshooting
ms.assetid: 4320a4a4-6183-462b-8bda-e7424e7cb706
author: rothja
ms.author: jroth
ms.openlocfilehash: 6d1a45cf3572c1d533714ff7ebf96fe0b4f236fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349306"
---
# <a name="dynamic-management-views-and-system-catalog-views-always-on-availability-groups"></a>Dynamische Verwaltungssichten und Systemkatalogsichten (Always On-Verfügbarkeitsgruppen)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema werden einige allgemeine Abfragen zu dynamischen Verwaltungssichten (DMV) für Always On-Verfügbarkeitsgruppen vorgestellt, mit der Sie Verfügbarkeitsgruppen überwachen und damit verbundene Probleme behandeln können.  
  
> [!TIP]  
>  Auf dem Always On-Dashboard können Sie mühelos die GUI so konfigurieren, dass eine Vielzahl der DMVs für die Verfügbarkeitsreplikate und -datenbanken angezeigt wird. Klicken Sie hierzu mit der rechten Maustaste auf die jeweilige Tabellenüberschrift, und wählen Sie die anzuzeigende oder auszublendende DMV aus.  
  
 Weitere Informationen zu den DMVs für Verfügbarkeitsgruppen finden Sie unter [Dynamische Verwaltungssichten und-Funktionen für Always On-Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md). Weitere Informationen zu den Katalogsichten für Verfügbarkeitsgruppen finden Sie unter [Katalogsichten für Always On-Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md).  
  
## <a name="check-the-wsfc-cluster-node-configuration"></a>Überprüfen der Konfiguration von WSFC-Clusterknoten  
 Die folgende Transact-SQL-Abfrage (T-SQL) ruft den Status aller Knoten im aktuellen WSFC-Cluster (Windows Server-Failoverclustering) ab.  
  
```sql  
use master  
go  
select * from sys.dm_hadr_cluster_members  
go  
```  
  
 Dieses Resultset meldet den Status der einzelnen Memberknoten des aktuellen WSFC-Clusters. Wenn das Quorum als **Knoten- und Dateifreigabemehrheit** definiert ist, wird auch die Dateifreigabe gemeldet. Sie können den Status der einzelnen Knoten einsehen, wie etwa die Abstimmungsverteilung der einzelnen Knoten (den Wert [number_of_quorum_votes](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)).  
  
## <a name="explore-the-cluster-network"></a>Untersuchen des Clusternetzwerks  
 Die folgende Abfrage ruft die Netzwerkkonfiguration des aktuellen WSFC-Clusters ab.  
  
```sql  
select * from sys.dm_hadr_cluster_networks  
```  
  
 Das Resultset enthält eine Zeile für jeden Netzwerkadapter im WSFC-Cluster. In einem Cluster mit zwei Knoten, der zwei Netzwerkadapter für jeden Knoten enthält, gibt diese Abfrage beispielsweise vier Zeilen zurück.  
  
## <a name="explore-the-availability-groups"></a>Untersuchen der Verfügbarkeitsgruppen  
 Die folgende Abfrage ruft Informationen zu einer Verfügbarkeitsgruppe ab.  
  
```sql  
select primary_replica, primary_recovery_health_desc, synchronization_health_desc from sys.dm_hadr_availability_group_states  
go  
select * from sys.availability_groups  
go  
select * from sys.availability_groups_cluster  
go  
```  
  
 Die DMVs [sys.dm_hadr_availability_group_states &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md), [sys.availability_groups &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) und [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) geben Informationen zu den Verfügbarkeitsgruppen im aktuellen WSFC-Cluster zurück. So geben etwa [sys.availability_groups &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) und [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) scheinbar identische Informationen zurück.  
  
 [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) meldet allerdings die im WSFC-Cluster gespeicherten Metadaten der Verfügbarkeitsgruppen, während [sys.availability_groups &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) Metadaten der Verfügbarkeitsgruppen meldet, die im SQL Server-Prozessadressraum zwischengespeichert werden. Darüber hinaus melden diese beiden DMVs Konfigurationsinformationen, wohingegen [sys.dm_hadr_availability_group_states &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md) den aktuellen Integritätsstatus der Verfügbarkeitsgruppen meldet.  
  
> [!IMPORTANT]  
>  Diese Nomenklatur wird mit den DMVs fortgeführt, die Verfügbarkeitsreplikate und -datenbanken dokumentieren.  
  
## <a name="explore-the-availability-replicas"></a>Untersuchen der Verfügbarkeitsreplikate  
 Die folgende Abfrage ruft Informationen zu den Verfügbarkeitsreplikaten ab, die in Ihren Verfügbarkeitsgruppen definiert sind.  
  
```sql  
select replica_id, role_desc, connected_state_desc, synchronization_health_desc from sys.dm_hadr_availability_replica_states  
go  
select replica_server_name, replica_id, availability_mode_desc, endpoint_url from sys.availability_replicas  
go  
select replica_server_name, join_state_desc from sys.dm_hadr_availability_replica_cluster_states  
go  
```  
  
 Ähnlich wie bei den DMVs der Verfügbarkeitsgruppen sind drei DMVs vorhanden, die Informationen zu Verfügbarkeitsreplikaten melden. [sys.dm_hadr_availability_replica_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md) meldet Zustandsinformationen zu den Verfügbarkeitsreplikaten, die lokal in SQL Server zwischengespeichert werden, während [sys.dm_hadr_availability_replica_cluster_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md) Zustandsinformationen zu den Verfügbarkeitsreplikaten aus dem WSFC-Cluster meldet. Zu guter Letzt meldet [availability_replicas](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md) Konfigurationsdaten zu den Verfügbarkeitsreplikaten, die lokal in SQL Server zwischengespeichert werden.  
  
## <a name="explore-availability-replica-health"></a>Untersuchen der Integrität von Verfügbarkeitsreplikaten  
 Die folgende Abfrage ruft aktuelle Zustandsinformationen zu den Verfügbarkeitsreplikaten ab.  
  
```sql  
select replica_id, role_desc, recovery_health_desc, synchronization_health_desc from sys.dm_hadr_availability_replica_states  
go  
```  
  
 Vergleichen Sie die Abfrageergebnisse für das primäre Replikat mit denen für das sekundäre Replikat, und beachten Sie, dass für das sekundäre Replikat nur Integritätsinformationen für dieses Replikat gemeldet werden, nicht für andere Replikate in der Verfügbarkeitsgruppe.  
  
## <a name="explore-the-availability-databases"></a>Untersuchen der Verfügbarkeitsdatenbanken  
 Die folgende Abfrage ruft Informationen zu den Verfügbarkeitsreplikaten ab, die in Ihrer Verfügbarkeitsgruppe definiert sind. Sie können die Änderung in den Abfrageergebnissen sehen, bevor und nachdem Sie die Datenverschiebung in einer Verfügbarkeitsdatenbank angehalten haben.  
  
```sql
select * from sys.availability_databases_cluster  
go  
select group_database_id, database_name, is_failover_ready  from sys.dm_hadr_database_replica_cluster_states  
go  
select database_id, synchronization_state_desc, synchronization_health_desc, last_hardened_lsn, redo_queue_size, log_send_queue_size from sys.dm_hadr_database_replica_states  
go  
```  
  
 Auch hier melden drei DMVs für Always On-Verfügbarkeitsgruppen Informationen zu Verfügbarkeitsdatenbanken. [sys.availability_databases_cluster](~/relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md) meldet Konfigurationsinformationen zu Verfügbarkeitsdatenbanken aus dem WSFC-Cluster. [sys.dm_hadr_database_replica_cluster_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md) meldet Zustandsinformationen zu den Datenbankreplikaten, die lokal in SQL Server zwischengespeichert werden. Es enthält wichtige Zustandsinformationen wie etwa die Failoverbereitschaft des Verfügbarkeitsreplikats. Zu guter Letzt meldet ein sehr ausführliches Resultset, [sys.dm_hadr_database_replica_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md), Identitäts- und Statusinformationen zu jeder Verfügbarkeitsdatenbank wie LSN-Statusinformationen für die Protokolle der primären und sekundären Datenbankreplikate.  
  
## <a name="explore-availability-database-health"></a>Untersuchen der Integrität von Verfügbarkeitsdatenbanken  
 Die folgende Abfrage ruft Informationen zur Integrität der einzelnen Verfügbarkeitsdatenbanken für die Replikate ab. Sie können die Änderung in den Abfrageergebnissen sehen, bevor und nachdem Sie die Datenverschiebung in einer Verfügbarkeitsdatenbank angehalten haben.  
  
```sql  
select dc.database_name, dr.database_id, dr.synchronization_state_desc,   
dr.suspend_reason_desc, dr.synchronization_health_desc  
from sys.dm_hadr_database_replica_states dr  join sys.availability_databases_cluster dc  
on dr.group_database_id=dc.group_database_id   
where is_local=1  
go  
```  
  
  
