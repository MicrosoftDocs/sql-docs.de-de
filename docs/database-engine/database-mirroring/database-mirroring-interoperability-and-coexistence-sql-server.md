---
title: 'Datenbankspiegelung: Interoperabilität und Koexistenz'
description: In diesem Artikel werden die Interoperabilität und die Koexistenz der SQL Server-Datenbankspiegelung und anderer SQL Server-Features erläutert, z. B. Volltextkataloge und Datenbankmomentaufnahmen.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- high availability [SQL Server], interoperability and coexistence
- Database Engine [SQL Server], high availability
ms.assetid: 89fef397-e0cf-4e08-b598-25b8d4170523
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 5bd20c8ef36e676ab02430bc0da9d44f2de34768
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353161"
---
# <a name="database-mirroring-interoperability-and-coexistence-sql-server"></a>Datenbankspiegelung: Interoperabilität und Koexistenz (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Die Datenbankspiegelung kann mit den folgenden Funktionen oder Komponenten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verwendet werden:  
  
-   [AlwaysOn-Failoverclusterinstanzen (SQL Server-Failoverclustering)](../../database-engine/database-mirroring/database-mirroring-and-sql-server-failover-cluster-instances.md)  
  
-   [Change Data Capture (und Änderungsnachverfolgung)](../../relational-databases/track-changes/change-data-capture-and-other-sql-server-features.md)  
  
-   [Datenbankmomentaufnahmen](../../database-engine/database-mirroring/database-mirroring-and-database-snapshots-sql-server.md)  
  
-   [Volltextkataloge](../../database-engine/database-mirroring/database-mirroring-and-full-text-catalogs-sql-server.md)  
  
-   [Protokollversand](../../database-engine/database-mirroring/database-mirroring-and-log-shipping-sql-server.md)  
  
-   [Replikation](../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md)  
  
 Die Datenbankspiegelung funktioniert nicht in Verbindung mit folgenden Elementen:  
  
-   Datenbankübergreifende Transaktionen/verteilte Transaktionen  
  
     Weitere Informationen dazu, warum solche Transaktionen nicht unterstützt werden, finden Sie unter [Datenbankübergreifende Transaktionen und verteilte Transaktionen für AlwaysOn-Verfügbarkeitsgruppen oder Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).  
  
-   [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
