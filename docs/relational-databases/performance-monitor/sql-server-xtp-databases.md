---
title: SQL Server XTP-Datenbanken | Microsoft-Dokumentation
description: Informationen zum Leistungsobjekt „SQL Server XTP-Datenbanken“, das spezifische In-Memory-OLTP-Datenbank-Indikatoren bereitstellt.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQL Server 2016 XTP Databases
ms.assetid: 488ff55e-173f-43f6-9bdb-67b35e7cebfe
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9b4e75e2033e0e73c592eb0e565038d514f73229
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505504"
---
# <a name="sql-server-xtp-databases"></a>SQL Server XTP-Datenbanken
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Das Leistungsobjekt **SQL Server XTP-Datenbanken** spezifische In-Memory-OLTP-Datenbanken bereit.

> [!NOTE]
>  Die SQL Server XTP-Datenbanken-Indikatoren sind aktuell in „sys.dm_os_performance_counters“ nicht sichtbar.  Die Indikatoren können im [System-Monitor](../../relational-databases/performance/start-system-monitor-windows.md)angezeigt werden.

Diese Tabelle beschreibt die **SQL Server XTP-Datenbanken** -Leistungsindikatoren.

|Leistungsindikator|BESCHREIBUNG| 
|-------------|-----------------|  
|**Durchschnittliches Transaktionssegment – großer Datenumfang**|Mittlere Größe von Transaktionssegmenten bei großer Datennutzlast. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**Durchschnittliche Transaktionssegmentgröße**|Mittlere Größe von Transaktionssegmenten der Nutzlast. Wenn dieser Wert gegen null tendiert, werden mehr Seiten aus der Back-End-Zuweisung zugewiesen. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**Thread leeren – 256K Warteschlangentiefe**|Tiefe der Threadbereinigungs-Warteschlange für E/A-Anforderungen mit 256K.|
|**Thread leeren – 4K Warteschlangentiefe**|Tiefe der Threadbereinigungs-Warteschlange für E/A-Anforderungen mit 4K.|
|**Thread leeren – 64K Warteschlangentiefe**|Tiefe der Threadbereinigungs-Warteschlange für E/A-Anforderungen mit 64K.|
|**Unveränderliche E/A-Threads/s leeren (256K)**|Die Anzahl der 256K-E/A-Anforderungen während der Verarbeitung der Seitenlöschung oberhalb der Sperrgrenze, die daher nicht ausgegeben werden können.|
|**Unveränderliche E/A-Threads/s leeren (4K)**|Die Anzahl der 4K-E/A-Anforderungen während der Verarbeitung der Seitenlöschung oberhalb der Sperrgrenze, die daher nicht ausgegeben werden können.|
|**Unveränderliche E/A-Threads/s leeren (64K)**|Die Anzahl der 64K-E/A-Anforderungen während der Verarbeitung der Seitenlöschung oberhalb der Sperrgrenze, die daher nicht ausgegeben werden können.|
|**IoPagePool256K kostenlose Listenanzahl**|Anzahl der Seiten in der Freiliste im 256K-E/A-Seitenpool. Wenn dieser Wert gegen null tendiert, werden mehr Seiten aus der Back-End-Zuweisung zugewiesen. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**IoPagePool256K insgesamt zugeordnet**|Gesamtzahl der dem 256K E/A-Seitenpool von der Back-End-Zuweisung zugewiesenen und gehaltenen Seiten. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**IoPagePool4K kostenlose Listenanzahl**|Anzahl der Seiten in der Freiliste im 4K-E/A-Seitenpool. Wenn dieser Wert gegen null tendiert, werden mehr Seiten aus der Back-End-Zuweisung zugewiesen. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**IoPagePool4K insgesamt zugeordnet**|Gesamtzahl der dem 4K E/A-Seitenpool von der Back-End-Zuweisung zugewiesenen und gehaltenen Seiten. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**IoPagePool64K kostenlose Listenanzahl**|Anzahl der Seiten in der Freiliste im 64K-E/A-Seitenpool. Wenn dieser Wert gegen null tendiert, werden mehr Seiten aus der Back-End-Zuweisung zugewiesen. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**IoPagePool64K insgesamt zugeordnet**|Gesamtzahl der dem 64K E/A-Seitenpool von der Back-End-Zuweisung zugewiesenen und gehaltenen Seiten. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**MtLog 256K-Erweiterungsanzahl**|Anzahl der Erweiterungen eines 256K-MtLogs. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**MtLog 256K IOs ausstehend**|Die Anzahl der ausstehenden 256K E/A-Anforderungen, die von MtLog ausgegeben wurden.|
|**MtLog 256K Seitenfüllung %/Seite geleert**|Durchschnittlicher Füllprozentsatz jeder gelöschten 256K-MtLog-Seite. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**MtLog 256K geschriebene Bytes/s**|Schreiben von Bytes pro Sekunde auf 256K MtLog-Objekte. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**MtLog 4K-Erweiterungsanzahl**|Anzahl der Erweiterungen eines 4K-MtLogs. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**MtLog 4K IOs ausstehend**|Die Anzahl der ausstehenden 4K E/A-Anforderungen, die von MtLog ausgegeben wurden.|
|**MtLog 4K Seitenfüllung %/Seite geleert**|Durchschnittlicher Füllprozentsatz jeder gelöschten 4K-MtLog-Seite. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**MtLog 4K geschriebene Bytes/s**|Geschriebene Bytes pro Sekunde auf 4K MtLog-Objekte. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**MtLog 64K Anzahl erweitern**|Anzahl der Erweiterungen eines 64K-MtLogs. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**MtLog 64K IOs ausstehend**|Die Anzahl der ausstehenden 64K E/A-Anforderungen, die von MtLog ausgegeben wurden.|
|**MtLog 64K Seitenfüllung %/Seite geleert**|Durchschnittlicher Füllprozentsatz jeder gelöschten 64K-MtLog-Seite. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**MtLog 64K geschriebene Bytes/s**|Geschriebene Bytes pro Sekunde auf 64K MtLog-Objekte. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**Mergeanzahl**|Anzahl der aktiven Merges.|
|**Mergeanzahl/s**|Die Anzahl der pro Sekunde erstellten Merges (im Mittel).|
|**Anzahl Serialisierungen**|Die Anzahl der aktiven Serialisierungen.|
|**Anzahl Serialisierungen/s**|Die Anzahl der pro Sekunde erstellten Serialisierungen (im Mittel)).|
|**Seitenanzahl für Endsegmentcache**|Anzahl der im Endsegmentcache zugeordneten Seiten. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|
|**Spitzenwert der Seitenanzahl im Endsegmentcache**|Größte Anzahl Seiten, die im Endsegmentcache zugewiesen wurden. Dieser Leistungsindikator befindet sich auf einer sehr niedrigen Ebene und dient nicht der Verwendung durch Kunden.|


## <a name="see-also"></a>Weitere Informationen  
[Leistungsindikatoren für SQL Server XTP &#40;In-Memory-OLTP&#41;](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)
