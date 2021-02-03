---
description: STCurveToLine (geography-Datentyp)
title: STCurveToLine (geography-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STCurveToLine_TSQL
- STCurveToLine
dev_langs:
- TSQL
helpviewer_keywords:
- STCurveToLine method (geography)
ms.assetid: 2f863a85-6168-465a-b32f-bb5e3de58dee
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: d73859f9036fc22935e5cb59632d5ba189e417f2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204663"
---
# <a name="stcurvetoline-geography-data-type"></a>STCurveToLine (geography-Datentyp)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt eine polygonale Näherung einer Instanz von **geography** mit Kreisbogensegmenten zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
.STCurveToLine()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **geography**  
  
 CLR-Rückgabetyp: **SqlGeography**  
  
## <a name="remarks"></a>Bemerkungen  
 Gibt eine **LineString** -Instanz für eine **CircularString** - oder **CompoundCurve** -Instanz zurück.  
  
 Gibt eine **Polygon** -Instanz für eine **CurvePolygon** -Instanz zurück.  
  
 Gibt eine Kopie von **geography** -Instanzen zurück, die keine **CircularString**, **CompoundCurve**-Instanz und keine **CurvePolygon** -Instanz enthalten.  
  
 Im Gegensatz zur SQL MM-Spezifikation werden bei dieser Methode keine Werte der Z-Koordinate zur Berechnung der polygonalen Näherung verwendet. In der aufrufenden **geography**-Instanz enthaltene Werte der Z-Koordinate werden ignoriert.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird eine `LineString` -Instanz zurückgegeben, die eine polygonale Näherung einer `CircularString` -Instanz ist:  
  
```
 DECLARE @g1 geography = 'CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
 DECLARE @g2 geography;  
 SET @g2 = @g1.STCurveToLine();  
 SELECT @g1.STNumPoints() AS G1, @g2.STNumPoints() AS G2;
 ```  
  
## <a name="see-also"></a>Siehe auch  
 [STLength &#40;geography-Datentyp&#41;](../../t-sql/spatial-geography/stlength-geography-data-type.md)   
 [STNumPoints &#40;geography-Datentyp&#41;](../../t-sql/spatial-geography/stnumpoints-geography-data-type.md)   
 [Übersicht über räumliche Datentypen](../../relational-databases/spatial/spatial-data-types-overview.md)  
  
  
