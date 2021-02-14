---
description: sys.availability_group_listeners (Transact-SQL)
title: sys.availability_group_listeners (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- availability_group_listeners_TSQL
- sys.availability_group_listeners
- sys.availability_group_listeners_TSQL
- availability_group_listeners
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_group_listeners catalog view
- Availability Groups [SQL Server], listeners
ms.assetid: b5e7d1fb-3ffb-4767-8135-604c575016b1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fbc07c0323f8efb1032555ba83d7758ef99bc6d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338931"
---
# <a name="sysavailability_group_listeners-transact-sql"></a>sys.availability_group_listeners (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Für jede Always On-Verfügbarkeitsgruppe gilt: Entweder werden null Zeilen zurückgegeben, um anzuzeigen, dass der Verfügbarkeitsgruppe kein Netzwerkname zugeordnet ist, oder für jede Verfügbarkeitsgruppen-Listenerkonfiguration im WSFC-Cluster (Windows Server-Failoverclustering) wird eine Zeile zurückgegeben. In dieser Sicht wird die von Cluster erfasste Echtzeitkonfiguration angezeigt.  
  
> [!NOTE]  
>  In dieser Katalogsicht werden nicht die Details einer IP-Konfiguration beschrieben, die im WSFC-Cluster definiert wurde.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Verfügbarkeits Gruppen-ID (**group_id**) aus [sys.availability_groups](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md).|  
|**listener_id**|**nvarchar (36)**|Die GUID der Clusterressourcen-ID.|  
|**dns_name**|**nvarchar (63)**|Der konfigurierte Netzwerkname (Hostname) des Verfügbarkeitsgruppenlisteners.|  
|**port**|**int**|Die für den Verfügbarkeitsgruppenlistener konfigurierte TCP-Portnummer.<br /><br /> NULL = Der Listener wurde außerhalb von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] konfiguriert, und die Portnummer des Listeners wurde der Verfügbarkeitsgruppe nicht hinzugefügt. Zum Hinzufügen des Ports gefällt Ihnen die Option MODIFY Listener der [Alter Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) - [!INCLUDE[tsql](../../includes/tsql-md.md)] Anweisung.|  
|**is_conformant**|**bit**|Gibt an, ob diese IP-Konfiguration konform ist. Folgende Werte sind möglich:<br /><br /> 1 = Der Listener ist konform. Zwischen den IP-Adressen (Internet Protocol) sind nur "or"-Beziehungen vorhanden. Die *Konformität* umfasst jede IP-Konfiguration, die von der [Create Availability Group](../../t-sql/statements/create-availability-group-transact-sql.md) -Anweisung erstellt wurde [!INCLUDE[tsql](../../includes/tsql-md.md)] . Zudem gilt eine IP-Konfiguration als konform, die zwar außerhalb von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellt wurde (z. B. mit dem WSFC-Failovercluster-Manager), aber mit der TSQL-Anweisung ALTER AVAILABILITY GROUP geändert werden kann.<br /><br /> 0 = Der Listener ist nicht konform. In der Regel weist dies auf eine IP-Adresse hin, die nicht mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Befehlen konfiguriert werden konnte und stattdessen direkt im WSFC-Cluster definiert wurde.|  
|**ip_configuration_string_from_cluster**|**nvarchar(max)**|Die Cluster-IP-Konfigurationszeichenfolgen für diesen Listener, falls vorhanden. NULL = Der Listener hat keine virtuellen IP-Adressen. Beispiel:<br /><br /> IPv4-Adresse: `65.55.39.10`.<br /><br /> IPv6-Adresse: `2001::4898:23:1002:20f:1fff:feff:b3a3`|  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Always on Verfügbarkeits gruppendynamische Verwaltungs Sichten und Funktionen &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Katalogsichten Always On-Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Überwachen von Verfügbarkeits Gruppen &#40;Transact-SQL-&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
