---
description: STDifference (geography-Datentyp)
title: STDifference (geography-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STDifference_TSQL
- STDifference (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STDifference (geography Data Type)
ms.assetid: 1cde5054-b91a-41bb-812a-08c9308738af
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: a6ac9219a2450835acb3a6495aef4c5db4304048
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192213"
---
# <a name="stdifference-geography-data-type"></a>STDifference (geography-Datentyp)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt ein Objekt zurück, das die Punktmenge einer **geography** -Instanz darstellt, die sich außerhalb einer anderen **geography** -Instanz befindet.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
.STDifference ( other_geography )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *other_geography*  
 Eine andere **geography**-Instanz, die angibt, welche Punkte aus der Instanz zu entfernen sind, in der STDifference() aufgerufen wird.  
  
## <a name="return-types"></a>Rückgabetypen  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **geography**  
  
 CLR-Rückgabetyp: **SqlGeography**  
  
## <a name="exceptions"></a>Ausnahmen  
 Diese Methode löst eine **ArgumentException** aus, wenn die Instanz eine gegenüberliegende Kante enthält.  
  
## <a name="remarks"></a>Bemerkungen  
 Diese Methode gibt immer NULL zurück, wenn die SRIDs (Spatial Reference IDs) der **geography** -Instanzen nicht übereinstimmen.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wurden die Ergebnisse, die auf dem Server zurückgegeben werden können, um **FullGlobe**-Instanzen erweitert. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt räumliche Instanzen, die größer als eine Hemisphäre sind. Im Ergebnis können nur dann Kreisbogensegmente enthalten sein, wenn die Eingabeinstanzen auch Kreisbogensegmente enthalten. Diese Methode ist nicht exakt.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-computing-the-difference-between-two-geography-instances"></a>A. Berechnen des Unterschiedes zwischen zwei geography-Instanzen  
 Im folgenden Beispiel wird `STDifference()` zum Berechnen der Differenz zwischen zwei **geography** -Instanzen erweitert.  
  
```  
DECLARE @g geography;  
DECLARE @h geography;  
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);  
SET @h = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STDifference(@h).ToString();  
```  
  
### <a name="b-using-a-fullglobe-with-stdifference"></a>B. Verwenden eines FullGlobe mit STDifference()  
 Im folgenden Beispiel wird eine Instanz von `FullGlobe` verwendet. Das erste Ergebnis ist eine leere `GeometryCollection` , und das zweite Ergebnis ist eine `Polygon` -Instanz. `STDifference()` gibt eine leere `GeometryCollection` zurück, wenn der Parameter eine `FullGlobe`-Instanz ist. Alle Punkte in einer aufrufenden Instanz von `geography` sind in einer Instanz von `FullGlobe` enthalten.  
  
```
 DECLARE @g geography = 'POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 DECLARE @h geography = 'FULLGLOBE';  
 SELECT @g.STDifference(@h).ToString(),  
 @h.STDifference(@g).ToString();
 ```  
  
## <a name="see-also"></a>Weitere Informationen  
 [OGC-Methoden für geography-Instanzen](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
