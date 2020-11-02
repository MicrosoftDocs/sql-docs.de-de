---
title: Empfohlene DTA-Leistungsverbesserungen
description: Erfahren Sie wie der Datenbankoptimierungsratgeber (Database Engine Tuning Advisor) durch die Analyse der Arbeitsauslastung einer Datenbank in SQL Server eine Kombination von Rowstore- und Columnstore-Indizes empfehlen kann.
ms.custom: seo-dt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor, performance improvements
ms.assetid: 2e51ea06-81cb-4454-b111-da02808468e6
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 8c64f567d8da74a2eb15b5647a2b259ba52193b4
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92678954"
---
# <a name="performance-improvements-using-database-engine-tuning-advisor-dta-recommendations"></a>Leistungsverbesserungen mithilfe von Empfehlungen des Datenbankoptimierungsratgebers (DTA)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]


---
Die Leistung von Data Warehousing- und Analysearbeitsauslastungen kann mit **Columnstore** -Indizes enorm verbessert werden, insbesondere bei Abfragen, bei denen umfangreiche Tabellen gescannt werden müssen. **Rowstore** -Indizes (B+-Struktur) sind bei Abfragen am effektivsten, bei denen bei der Suche nach einem bestimmten Wert oder Wertebereich auf relativ kleine Datenmengen zugegriffen wird. Da Rowstore-Indizes Zeilen in sortierter Reihenfolge zurückgeben, lassen sich damit auch die Kosten der Sortierung in Abfrageausführungsplänen reduzieren. Die Auswahl der Kombination von Rowstore- und Columnstore-Indizes für die Erstellung Ihrer Datenbank hängt somit von der Workload Ihrer Anwendung ab.

Der Datenbankoptimierungsratgeber (Database Engine Tuning Advisor, DTA) kann ab SQL Server 2016 durch die Analyse der Arbeitsauslastung einer bestimmten Datenbank eine geeignete **Kombination von Rowstore- und Columnstore** -Indizes empfehlen. 

Zur Veranschaulichung, welche Vorteile die DTA-Empfehlungen für die Arbeitsauslastungsleistung mit sich bringen, haben wir mit verschiedenen echten Kundenarbeitsauslastungen experimentiert. Für jede Kundenarbeitsauslastung haben wir DTA einzelne Abfragen sowie die gesamte Arbeitsauslastung mit Abfragen analysieren lassen. Dabei haben wir drei Alternativen betrachtet:
  
  1. **Nur Columnstore:** Für alle Tabellen wurden nur Columnstore-Indizes ohne Verwendung des DTA erstellt. 
  2. **DTA (nur Rowstore):** Der DTA wurde mit der Option ausgeführt, dass nur Rowstore-Indizes empfohlen werden.
  3. **DTA (Rowstore und Columnstore):** Der DTA wurde mit der Option ausgeführt, dass sowohl Rowstore- als auch Columnstore-Indizes empfohlen werden.  
   
Anschließend haben wir die jeweils empfohlenen Indizes implementiert. Dabei wurde die durchschnittliche CPU-Zeit (in Millisekunden) über mehrere Abfrageausführungen oder über die Arbeitsauslastung hinweg protokolliert. In der folgenden Abbildung ist die CPU-Zeit in Millisekunden für Arbeitsauslastungen über zwei verschiedene Kundendatenbanken hinweg dargestellt. Beachten Sie, dass für die y-Achse (CPU-Zeit) eine logarithmische Skalierung verwendet wird.   


![Screenshot: Säulendiagramm zur DTA-Leistung (Columnstore/Rowstore)](../../relational-databases/performance/media/dta-columnstore-rowstore-performance.gif)



**Bedarf an gemischten physischen Designs:** Die erste Säulengruppe entspricht Kunde 1, Abfrage 1. DTA (Rowstore und Columnstore) empfiehlt eine Gruppe von vier Columnstore- und sechs Rowstore-Indizes, was zu einer im Vergleich zu einem reinen Columnstore-Index und DTA (nur Rowstore) eine zweieinhalb bis vier Mal geringere CPU-Zeit zur Folge hat. Dies zeigt die Vorteile von gemischten physischen Entwürfen, die aus Rowstore- und Columnstore-Indizes bestehen, *auch bei einer einzelnen Abfrage* . 

**Effizienz von Rowstore-Indexempfehlungen** : Die zweite und die dritte Säulengruppe (die Kunde 1, Abfrage 2 und Kunde 2, Abfrage 1 entsprechen) stellen Fälle dar, bei denen die Abfragen über Auswahlfilterprädikate verfügen, die von geeigneten Rowstore-Indizes profitieren. Für beide Abfragen empfiehlt DTA (nur Rowstore) und DTA (Rowstore + Columnstore) nur Rowstore-Indizes. Diese Beispiele zeigen auch, dass selbst dann, wenn DTA mit der Option ausgeführt wird, Columnstore-Indizes zu empfehlen, das kostenbasierte Konzept sicherstellt, dass ein Columnstore-Index nur dann empfohlen wird, wenn die Arbeitsauslastung tatsächlich davon profitiert.

**Effizienz von Columnstore-Indexempfehlungen** : Die vierte Säulengruppe, die Kunde 2, Abfrage 2 entspricht, stellt einen Fall dar, bei dem die Abfrage umfangreiche Tabellen scannt, die von Columnstore-Indizes profitieren würden. DTA (nur Rowstore) generiert eine Empfehlung, bei der der Wert für die CPU-Zeit höher ist, als wenn Columnstore-Indizes verwendet werden würden. DTA (Rowstore + Columnstore) empfiehlt geeignete Columnstore-Indizes und entspricht somit der Abfrageausführungsleistung der Option „Nur Columnstore“.

**Effizienz von Empfehlungen für Workloads mit mehreren Abfragen** : Die letzte Säulengruppe, die der gesamten Workload für Kunde 2 entspricht, veranschaulicht die Fähigkeit von DTA, mehrere Abfragen in der Workload analysieren und eine geeignete Kombination aus Rowstore- und Columnstore-Indizes empfehlen zu können, sodass die Ausführungskosten der gesamten Workload verbessert werden. DTA (Rowstore und Columnstore) empfiehlt vier Columnstore-Indizes und eine Reihe von Rowstore-Indizes. Dadurch wird die Leistung für die Workload im Vergleich zu der Option, mit der nur Columnstore-Indizes erstellt werden, um ein Vielfaches verbessert. Im Vergleich zu DTA (nur Rowstore) lässt sich die Leistung um das 4- bis 5-Fache verbessern.

Die Beispiele oben veranschaulichen also die Fähigkeit von DTA, die von der SQL Server-Datenbank-Engine unterstützten Rowstore- und Columnstore-Indizes optimal nutzen und eine geeignete Indexkombination empfehlen zu können, mit der sich die CPU-Zeit für die Workload deutlich verringern lässt. 

<a name="see-also"></a>Weitere Informationen
---
[Datenbankoptimierungsratgeber](../../relational-databases/performance/database-engine-tuning-advisor.md)

[Empfehlungen für Columnstore-Index im Datenbankoptimierungsratgeber (DTA)](../../relational-databases/performance/columnstore-index-recommendations-in-database-engine-tuning-advisor-dta.md)

[Beschreibung von Columnstore-Indizes](~/relational-databases/indexes/columnstore-indexes-overview.md)

[Columnstore-Indizes für Data Warehousing](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)

[CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md)

[CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)



