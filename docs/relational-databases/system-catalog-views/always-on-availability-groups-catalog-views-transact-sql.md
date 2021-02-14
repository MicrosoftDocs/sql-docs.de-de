---
description: Katalog Sichten für Always on-Verfügbarkeits Gruppen (Transact-SQL)
title: Katalog Sichten für Always on-Verfügbarkeits Gruppen (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- catalog views [SQL Server], AlwaysOn Availability Groups
- YY
ms.assetid: ff53e873-8ff6-4628-af84-4ec52fa4951c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d341425b680fc6baa233a20d2be57988b2c9b85d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068190"
---
# <a name="always-on-availability-groups-catalog-views-transact-sql"></a>Katalog Sichten für Always on-Verfügbarkeits Gruppen (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Dieser Abschnitt enthält Katalogsichten und Funktionen, die mit [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]in Verbindung stehen.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  

:::row:::
    :::column:::
        [sys.availability_databases_cluster](../../relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md)

        [sys.availability_group_listener_ip_addresses](../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)

        [sys.availability_group_listeners](../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)

        [sys.availability_groups](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.availability_groups_cluster](../../relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md)

        [sys.availability_read_only_routing_lists](../../relational-databases/system-catalog-views/sys-availability-read-only-routing-lists-transact-sql.md)

        [sys.availability_replicas](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)
    :::column-end:::
:::row-end:::
  
> [!NOTE]  
> Informationen zu verbundenen Verfügbarkeits Datenbanken finden Sie in den **replica_id** -und **group_database_id** Spalten in [sys.-Datenbanken (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys. Datenbanken (Transact-SQL)](sys-databases-transact-sql.md)   
 [sys.database_mirroring_endpoints (Transact-SQL)](sys-database-mirroring-endpoints-transact-sql.md)   
 [Always On-Verfügbarkeitsgruppen (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Überwachen von Verfügbarkeits Gruppen (Transact-SQL);](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Dynamische Verwaltungs Sichten und-Funktionen für Always on Verfügbarkeits Gruppen (Transact-SQL)](../system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)  
  
  
