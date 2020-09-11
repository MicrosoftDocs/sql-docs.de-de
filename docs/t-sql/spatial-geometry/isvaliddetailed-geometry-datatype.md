---
description: IsValidDetailed (geometry-Datentyp)
title: IsValidDetailed (geometry DataType) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- IsValidDetailed geometry
ms.assetid: 5a31e88a-ad7b-4ef7-b773-e2571f1cb3aa
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: f57155c8724d8cb27aaf06eaf491e11298ad61c2
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88497037"
---
# <a name="isvaliddetailed-geometry-datatype"></a>IsValidDetailed (geometry-Datentyp)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Gibt eine Meldung zurück, die Informationen zur Identifizierung von Problemen mit einem räumlichen Objekt enthält, das nicht gültig ist. Wenn das Objekt nicht gültig ist, wird nur der erste Fehler zurückgegeben. Wenn das Objekt gültig ist, wird der Wert 24400 zurückgegeben.
  
## <a name="syntax"></a>Syntax  
  
```  
  
.IsValidDetailed()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **nvarchar(max)**  
  
 CLR-Rückgabetyp: **string**  
  
## <a name="remarks"></a>Hinweise  
 Die folgende Tabelle enthält mögliche Rückgabewerte:  
  
|Rückgabewert|BESCHREIBUNG|  
|------------------|-----------------|  
|24400|Gültig|  
|24401|Ungültig, die Ursache ist unbekannt.|  
|24402|Ungültig, weil der Punkt {0} ein isolierter Punkt ist, der für diesen Objekttyp ungültig ist.|  
|24403|Ungültig, weil sich ein Polygonkantenpaar überlappt.|  
|24404|Ungültig, weil eine Überschneidung von Polygonring {0} mit sich selbst oder einem anderen Ring vorliegt.|  
|24405|Ungültig, weil eine Überschneidung eines Polygonrings mit sich selbst oder einem anderen Ring vorliegt.|  
|24406|Ungültig, weil die Kurve {0} zu einem Punkt degeneriert wird.|  
|24407|Ungültig, weil der Polygonring {0} auf eine Linie an Punkt {1} reduziert wird.|  
|24408|Ungültig, weil der Polygonring {0} nicht geschlossen ist.|  
|24409|Ungültig, weil sich ein Teil des Polygonrings {0} im Inneren eines Polygons befindet.|  
|24410|Ungültig, weil der Ring {0} der erste Ring in einem Polygon, aber nicht dessen äußerer Ring ist.|  
|24411|Ungültig, weil der Ring {0} außerhalb des äußeren Rings {1} des zugehörigen Polygons liegt.|  
|24412|Ungültig, weil das Innere eines Polygons mit den Ringen {0} und {1} unverbunden ist.|  
|24413|Ungültig aufgrund von zwei überlappenden Kanten in Kurve {0}.|  
|24414|Ungültig, weil sich eine Kante der Kurve {0} und eine Kante der Kurve {1} überschneiden.|  
|24415|Ungültig, weil ein Polygon über eine ungültige Ringstruktur verfügt.|  
|24416|Ungültig, weil die Kante, die an Punkt {1} beginnt, in Kurve {0} entweder eine Linie oder ein degenerierter Bogen mit entgegengesetzten Endpunkten ist.|  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel für ein ungültiges räumliches Objekt wird veranschaulicht, wie sich die **IsValidDetailed()**-Methoden verhalten.  
  
```sql  
DECLARE @p GEOMETRY = 'Polygon((2 2, 4 4, 4 2, 2 4, 2 2))'  
SELECT @p.IsValidDetailed()  
--Returns: 24404: Not valid because polygon ring (1) intersects itself or some other ring.  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erweiterte Methoden für geometry-Instanzen](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
  
  

