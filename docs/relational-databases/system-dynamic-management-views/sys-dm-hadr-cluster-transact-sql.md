---
description: sys.dm_hadr_cluster (Transact-SQL)
title: sys.dm_hadr_cluster (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/31/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_hadr_cluster
- dm_hadr_cluster_HADR
- sys.dm_hadr_cluster_TSQL
- dm_hadr_cluster
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_cluster catalog view
- Availability Groups [SQL Server], WSFC clusters
ms.assetid: 13ce70e4-9d43-4a80-a826-099e6213bf85
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c8a4a8a968432fa37489f579d36c53c32b5d31db
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339435"
---
# <a name="sysdm_hadr_cluster-transact-sql"></a>sys.dm_hadr_cluster (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Wenn der WSFC-Knoten (Windows Server-Failoverclustering), der eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -fähige Instanz von [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] hostet, über ein WSFC-Quorum verfügt, gibt **sys.dm_hadr_cluster** eine Zeile zurück, die den Clusternamen und Informationen zum Quorum verfügbar macht. Wenn der WSFC-Knoten nicht über ein Quorum verfügt, wird keine Zeile zurückgegeben.  
 > [!TIP]
 > Ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] unterstützt diese dynamische Verwaltungs Sicht Always on-Failoverclusterinstanzen zusätzlich zu Always on Verfügbarkeits Gruppen.

|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**cluster_name**|**nvarchar(128)**|Der Name des WSFC-Clusters, der die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -fähigen Instanzen von [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]hostet.|  
|**quorum_type**|**tinyint**|Der Typ des Quorums, der von diesem WSFC-Cluster verwendet wird. Folgende Werte sind möglich:<br /><br /> 0 = Knotenmehrheit Diese Quorumkonfiguration kann Fehler bei der Hälfte der Knoten (aufgerundet) minus einem tolerieren. In einem Cluster mit sieben Knoten kann diese Quorumkonfiguration z. B. drei Knotenfehler tolerieren.<br /><br /> 1 = Knoten- und Datenträgermehrheit Wenn der Datenträgerzeuge online bleibt, kann diese Quorumkonfiguration Fehler bei der Hälfte der Knoten (aufgerundet) tolerieren. So kann ein Cluster mit sechs Knoten, in dem der Datenträgerzeuge online ist, beispielsweise drei Knotenfehler tolerieren. Wenn der Datenträgerzeuge offline geht oder ein Datenträgerzeugenfehler auftritt, kann diese Quorumkonfiguration Fehler bei der Hälfte der Knoten (aufgerundet) minus einem Knoten tolerieren. Ein Cluster mit sechs Knoten, in dem ein Datenträgerzeugenfehler aufgetreten ist, kann beispielsweise zwei (3 minus 1 = 2) Knotenfehler tolerieren.<br /><br /> 2 = Knoten- und Dateifreigabemehrheit Diese Quorumkonfiguration funktioniert auf eine ähnliche Weise wie die Knoten- und Datenträgermehrheit, verwendet aber einen Dateifreigabenzeugen statt eines Datenträgerzeugen.<br /><br /> 3 = Keine Mehrheit: Nur Datenträger Wenn der Quorumdatenträger online ist, kann diese Quorumkonfiguration Fehler bei allen Knoten außer einem tolerieren.<br /><br /> 4 = unbekanntes Quorum. Unbekanntes Quorum für den Cluster.<br /><br /> 5 = cloudzeuge. Der Cluster nutzt Microsoft Azure für die Quorum-Schiedsgerichtsbarkeit. Wenn der cloudzeuge verfügbar ist, kann der Cluster den Ausfall der Hälfte der Knoten (aufgerundet) aufrechterhalten.|  
|**quorum_type_desc**|**varchar(50)**|Beschreibung von **quorum_type**. Folgende Werte sind möglich:<br /><br /> NODE_MAJORITY<br /><br /> NODE_AND_DISK_MAJORITY<br /><br /> NODE_AND_FILE_SHARE_MAJORITY<br /><br /> NO_MAJORITY:_DISK_ONLY <br /><br /> UNKNOWN_QUORUM <br /><br /> CLOUD_WITNESS|  
|**quorum_state**|**tinyint**|Status des WSFC-Quorums. Folgende Werte sind möglich:<br /><br /> 0 = Unbekannter Quorumstatus<br /><br /> 1 = Normales Quorum<br /><br /> 2 = Erzwungenes Quorum|  
|**quorum_state_desc**|**varchar(50)**|Beschreibung von **quorum_state**. Folgende Werte sind möglich:<br /><br /> UNKNOWN_QUORUM_STATE<br /><br /> NORMAL_QUORUM<br /><br /> FORCED_QUORUM|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Always on Verfügbarkeits gruppendynamische Verwaltungs Sichten und Funktionen &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Katalogsichten Always On-Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Überwachen von Verfügbarkeits Gruppen &#40;Transact-SQL-&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [sys.dm_hadr_cluster_members &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)  
  
  
