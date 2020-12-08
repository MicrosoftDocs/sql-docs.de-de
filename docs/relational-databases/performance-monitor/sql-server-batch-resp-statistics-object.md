---
title: SQL Server, Statistiken zu Batchantworten (Objekt) | Microsoft-Dokumentation
description: Hier lernen Sie das Leistungsobjekt „SQLServer:Statistiken zu Batchantworten“ kennen, das Leistungsindikatoren zum Nachverfolgen von SQL Server-Batchantwortzeiten bereitstellt.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Batch Resp Statistics
ms.assetid: a58e8733-6a8d-4b47-b5cb-042e813d808a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: acd7dc7b853c600b743d9791a3bade6ce8970a41
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505870"
---
# <a name="sql-server-batch-resp-statistics-object"></a>SQL Server, Statistiken zu Batchantworten (Objekt)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Das Leistungsobjekt **SQLServer:Statistiken zu Batchantworten** stellt Leistungsindikatoren zum Nachverfolgen von SQL Server-Batchantwortzeiten bereit.

In der folgenden Tabelle werden die SQL Server-Leistungsobjekte für **Statistiken zu Batchantworten** beschrieben.


|**SQL Server – Statistiken zu Batchantworten (Leistungsindikatoren)**|BESCHREIBUNG|  
|-------------|-----------------|  
|**Batches >=000000ms & \<000001ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 0 ms, aber kleiner als 1 ms.|
|**Batches >=000001ms & \<000002ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 1 ms, aber kleiner als 2 ms.|
|**Batches >=000002ms & \<000005ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 2 ms, aber kleiner als 5 ms.|
|**Batches >=000005ms & \<000010ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 5 ms, aber kleiner als 10 ms.|
|**Batches >=000010ms & \<000020ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 10 ms, aber kleiner als 20 ms.|
|**Batches >=000020ms & \<000050ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 20 ms, aber kleiner als 50 ms.|
|**Batches >=000050ms & \<000100ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 50 ms, aber kleiner als 100 ms.|
|**Batches >=000100ms & \<000200ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 100 ms, aber kleiner als 200 ms.|
|**Batches >=000200ms & \<000500ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 200 ms, aber kleiner als 500 ms.|
|**Batches >=000500ms & \<001000ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 500 ms, aber kleiner als 1.000 ms.|
|**Batches >=001000ms & \<002000ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 1.000 ms, aber kleiner als 2.000 ms.|
|**Batches >=002000ms & \<005000ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 2.000 ms, aber kleiner als 5.000 ms.|
|**Batches >=005000ms & \<010000ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 5.000 ms, aber kleiner als 10.000 ms.|
|**Batches >=010000ms & \<020000ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 10.000 ms, aber kleiner als 20.000 ms.|
|**Batches >=020000ms & \<050000ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 20.000 ms, aber kleiner als 50.000 ms.|
|**Batches >=050000ms & \<100000ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 50.000 ms, aber kleiner als 100.000 ms.| 
|**Batches >= 100000 ms**|Die Anzahl der SQL-Batches mit einer Antwortzeit größer oder gleich 100.000 ms.| 

Jeder Leistungsindikator in dem Objekt enthält die folgenden Instanzen:  
  
|Element|BESCHREIBUNG|  
|----------|-----------------|  
|**CPU-Zeit: Anforderungen**|Die Anzahl der auf der CPU-Zeit basierenden Anforderungen|  
|**CPU-Zeit: Gesamt(ms)**|Die von der CPU für den Batch erforderliche Gesamtzeit.|  
|**Verstrichene Zeit: Anforderungen**|Die Anzahl der Anforderungen basierend auf der verstrichenen Zeit|  
|**Verstrichene Zeit: Gesamt(ms)**|Die für den Batch verstrichene Zeit.|  

## <a name="see-also"></a>Weitere Informationen
[SQL Server, Plancache-Objekt](../../relational-databases/performance-monitor/sql-server-plan-cache-object.md)  
[Überwachen der Ressourcenverwendung (Systemmonitor)](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
