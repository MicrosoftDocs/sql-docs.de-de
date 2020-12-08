---
title: SQL Server, Abfragespeicherobjekt | Microsoft-Dokumentation
description: Erfahren Sie mehr über das Abfragespeicherobjekt, das Leistungsindikatoren bereitstellt, um die Ressourcenverwendung von SQL Server zum Speichern von Abfragetexten, Ausführungsplänen und Laufzeitstatistiken zu überwachen.
ms.custom: ''
ms.date: 03/17/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store object
- SQL Server:Query Store
ms.assetid: b4a04acd-0b66-44a5-b72d-1a45b49e13e6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 57892eac5224bb3b90f490644c3c49e1317b1a78
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505603"
---
# <a name="sql-server-query-store-object"></a>SQL Server, Abfragespeicherobjekt

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Das Abfragespeicherobjekt enthält Leistungsindikatoren zum Überwachen der Ressourcenverwendung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zum Speichern von Abfragetexten, Ausführungsplänen und Runtimestatistiken für Objekte wie z. B. gespeicherte Prozeduren, Ad-hoc- und vorbereitete [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen und Trigger.  
  
In dieser Tabelle werden die **SQLServer:Abfragespeicher**-Leistungsindikatoren beschrieben.  
  
|Leistungsindikatoren des SQL Server-Abfragespeichers|BESCHREIBUNG|  
|-------------------------------------|-----------------|  
|**CPU-Verwendung des Abfragespeichers**|Gibt die Nutzung der CPU durch den Abfragespeicher als Perzentil der Nutzung der CPU durch andere Prozesse an.|  
|**Logische Lesevorgänge des Abfragespeichers**|Gibt die Anzahl der logischen Lesevorgänge an, die durch den Abfragespeicher vorgenommen werden.|  
|**Logische Schreibvorgänge des Abfragespeichers**|Gibt an, wie viele Daten sich in der Warteschlange befinden, um aus dem Abfragespeicher geleert zu werden. Die Häufigkeit und Verzögerung beim Hinzufügen von Elementen zur Warteschlange (die Runtimestatistiken) werden durch die Einstellung für das Intervall für Datenleerung gesteuert.|  
|**Physische Lesevorgänge des Abfragespeichers**|Gibt die Anzahl der physischen Lesevorgänge an, die durch den Abfragespeicher vorgenommen werden.|  
  
 Jeder Leistungsindikator in dem Objekt enthält die folgenden Instanzen:  
  
|Abfragespeicherinstanz|BESCHREIBUNG|  
|--------------------------|-----------------|  
|**_Total**|Informationen für den Abfragespeicher für diese Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|\<database name>|Abfragespeicherinformationen für diese Datenbank.|  
  
## <a name="see-also"></a>Weitere Informationen  

- [Überwachen der Leistung mit dem Abfragespeicher](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Gespeicherte Prozeduren für den Abfragespeicher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)
- [Katalogsichten des Abfragespeichers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)
- [Überwachen der Ressourcenverwendung &#40;Systemmonitor&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
