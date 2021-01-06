---
title: Systemobjektreferenz für die Datenbankspiegelung | Microsoft-Dokumentation
description: 'Weitere Informationen zu Systemobjekten für die Datenbankspiegelung: Systemkatalogsichten, dynamische Systemverwaltungssichten und Systemtabellen.'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 3e5bc078ee4ee8817f8884d5c8e6f638ce432e3e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643711"
---
# <a name="database-mirroring-system-object-reference"></a>Systemobjektreferenz für die Datenbankspiegelung
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="system-catalog-views"></a>Systemkatalogsichten

| Systemkatalogsicht | BESCHREIBUNG|
| :------ | :----------------------------- |
| [sys.database_mirroring_witnesses](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   | Enthält eine Zeile für jede Zeugenrolle, die ein Server in einer Datenbankspiegelungspartnerschaft spielt. |
| &nbsp; | &nbsp; |

## <a name="system-dynamic-management-views"></a>Dynamische Systemverwaltungssichten

| Dynamische Systemverwaltungssicht | BESCHREIBUNG|
| :------ | :----------------------------- |
| [sys.dm_db_mirroring_auto_page_repair](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-auto-page-repair.md)   | Gibt eine Zeile für jede automatische Seitenreparatur für jede gespiegelte Datenbank der Serverinstanz zurück.  |
| [sys.dm_db_mirroring_connections](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md)    | Gibt für jede für die Datenbankspiegelung hergestellte Verbindung eine Zeile zurück. |
| &nbsp; | &nbsp; |

## <a name="system-tables"></a>Systemtabellen

| Systemtabelle | BESCHREIBUNG|
| :------ | :----------------------------- |
| [sysdbmaintplan_databases](../../relational-databases/system-tables/sysdbmaintplan-databases-transact-sql.md)   | Gibt Informationen zu Wartungsplänen für die Datenbankspiegelung zurück. |
| [sysdbmaintplan_history](../../relational-databases/system-tables/sysdbmaintplan-history-transact-sql.md)    | Gibt Informationen zum Verlauf von Wartungsplänen für die Datenbankspiegelung zurück. |
| [sysdbmaintplan_jobs](../../relational-databases/system-tables/sysdbmaintplan-jobs-transact-sql.md)    |Gibt Informationen zu Wartungsplanaufträgen für die Datenbankspiegelung zurück.  |
| [sysdbmaintplans](../../relational-databases/system-tables/sysdbmaintplans-transact-sql.md)    | Gibt Informationen zu Datenbankspiegelungsplänen zurück.  |
| &nbsp; | &nbsp; |


## <a name="see-also"></a>Weitere Informationen  
 [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   

  
  
