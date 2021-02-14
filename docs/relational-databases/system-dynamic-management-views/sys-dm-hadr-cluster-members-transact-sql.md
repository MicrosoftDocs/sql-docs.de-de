---
description: sys.dm_hadr_cluster_members (Transact-SQL)
title: sys.dm_hadr_cluster_members (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/31/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_hadr_cluster_members_TSQL
- sys.dm_hadr_cluster_members
- dm_hadr_cluster_members_TSQL
- dm_hadr_cluster_members
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_cluster_members catalog view
ms.assetid: feb20b3a-8835-41d3-9a1c-91d3117bc170
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eb47ace512b9db4983a9b61ccbdb6db490545937
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354176"
---
# <a name="sysdm_hadr_cluster_members-transact-sql"></a>sys.dm_hadr_cluster_members (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Wenn der WSFC-Knoten, der eine lokale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -fähige Instanz von [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] hostet, über ein WSFC-Quorum verfügt, wird eine Zeile für jedes Element, aus denen das Quorum besteht, einschließlich Elementstatus zurückgegeben. Dazu gehören alle Knoten in dem Cluster (zurückgegeben von der Funktion **Clusterenum** mit dem Typ CLUSTER_ENUM_NODE) sowie die Datenträger- bzw. Dateifreigabenzeugen. Die für ein bestimmtes Element zurückgegebene Zeile enthält Informationen zum Status dieses Elements. Beispiel: Für einen Cluster mit fünf Knoten mit Mehrheitsknotenquorum, in dem ein Knoten heruntergefahren ist, stellt **sys.dm_hadr_cluster_members** den Status des heruntergefahrenen Knotens als "NODE_DOWN" dar, wenn [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] sys.dm_hadr_cluster_members **von einer Serverinstanz aus abgefragt wird, die** -fähig ist und sich in einem Knoten mit Quorum befindet.  
  
 Wenn der WSFC-Knoten nicht über ein Quorum verfügt, werden keine Zeilen zurückgegeben.  
  
 Mithilfe dieser dynamischen Verwaltungssicht können Sie beispielsweise folgende Fragen beantworten:  
  
-   Welche Knoten werden derzeit auf dem WSFC-Cluster ausgeführt?  
  
-   Wie viele weitere Fehler kann der WSFC-Cluster tolerieren, bevor er sein Quorum in einem Szenario mit einem Mehrheitsknoten verliert?  

 > [!TIP]
 > Ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] unterstützt diese dynamische Verwaltungs Sicht Always on-Failoverclusterinstanzen zusätzlich zu Always on Verfügbarkeits Gruppen.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**member_name**|**nvarchar(128)**|Der Elementname, der ein Computername, ein Laufwerkbuchstabe oder ein Dateifreigabepfad sein kann.|  
|**member_type**|**tinyint**|Der Typ des Elements. Folgende Werte sind möglich:<br /><br /> 0 = WSFC-Knoten<br /><br /> 1 = Datenträgerzeuge<br /><br /> 2 = Dateifreigabenzeuge<br /><br /> 3 = cloudzeuge|  
|**member_type_desc**|**nvarchar(50)**|Beschreibung von **member_type**. Folgende Werte sind möglich:<br /><br /> CLUSTER_NODE<br /><br /> DISK_WITNESS<br /><br /> FILE_SHARE_WITNESS<br /><br /> CLOUD_WITNESS|  
|**member_state**|**tinyint**|Der Status des Elements. Folgende Werte sind möglich:<br /><br /> 0 = Offline<br /><br /> 1 = Online|  
|**member_state_desc**|**nvarchar(60)**|Beschreibung von **member_state**. Folgende Werte sind möglich:<br /><br /> UP<br /><br /> DOWN|  
|**number_of_quorum_votes**|**tinyint**|Anzahl von Quorumabstimmungen, die im Besitz dieses Quorumelements sind. Wenn keine Mehrheit vorliegt: Nur Datenträger-Quorumstypen. Dieser Wert wird standardmäßig auf "0" festgelegt. Für andere Quorumstypen wird dieser Wert standardmäßig auf "1" festgelegt.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="examples"></a>Beispiele  
  
## <a name="see-also"></a>Weitere Informationen  
 [Always on Verfügbarkeits gruppendynamische Verwaltungs Sichten und Funktionen &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Katalogsichten Always On-Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Überwachen von Verfügbarkeits Gruppen &#40;Transact-SQL-&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
