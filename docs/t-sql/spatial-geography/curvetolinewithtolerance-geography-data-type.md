---
description: CurveToLineWithTolerance (geography-Datentyp)
title: CurveToLineWithTolerance (geography-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CurveToLineWithTolerance_TSQL
- CurveToLineWithTolerance
dev_langs:
- TSQL
helpviewer_keywords:
- CurveToLineWithTolerance method (geography)
ms.assetid: 74369c76-2cf6-42ae-b9cc-e7a051db2767
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: f19dbc100f507d10a3c1c382735608b307a40ffd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210606"
---
# <a name="curvetolinewithtolerance-geography-data-type"></a>CurveToLineWithTolerance (geography-Datentyp)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Gibt eine polygonale Näherung einer Instanz von **geography** mit Kreisbogensegmenten zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
.CurveToLineWithTolerance( tolerance, relative )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
_tolerance_  
Ein **double** -Ausdruck, der die maximalen Fehler zwischen dem ursprünglichen Kreisbogensegment und der linearen Näherung definiert.  
  
_relative_  
Ein **bool** -Ausdruck, der angibt, ob ein relatives Maximum für die Abweichung verwendet werden soll. Wenn der relativer Wert auf „false“ (0) festgelegt wird, wird ein absolutes Maximum für die Abweichung verwendet, die eine lineare Näherung aufweisen kann. Wenn der relative Wert auf „true“ (1) festgelegt wird, wird die Toleranz als Produkt des tolerance-Parameters und des Durchmessers des Begrenzungsrahmens für das räumliche Objekt berechnet.  
  
## <a name="return-types"></a>Rückgabetypen  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **geography**  
  
CLR-Rückgabetyp: **SqlGeography**  
  
## <a name="exceptions"></a>Ausnahmen  
Festlegen der Toleranz < = 0 löst eine **ArgumentOutOfRange**-Ausnahme aus.  
  
## <a name="remarks"></a>Hinweise  
Mit dieser Methode kann ein Umfang für die Fehlertoleranz in der resultierenden **LineString** angegeben werden.  
  
Die **CurveToLineWithTolerance** -Methode gibt eine **LineString** -Instanz für eine **CircularString** oder eine **CompoundCurve** -Instanz und eine **Polygon** -Instanz für eine **CurvePolygon** -Instanz zurück.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-different-tolerance-values-on-a-circularstring-instance"></a>A. Verwenden verschiedener Toleranzwerte in einer CircularString-Instanz  
Im folgenden Beispiel wird gezeigt, wie sich das Festlegen der Toleranz auf eine Instanz von `LineString`auswirkt, die von einer Instanz von `CircularString` zurückgegeben wird:  
  
```
DECLARE @g geography;  
SET @g = geography::Parse('CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)');  
SELECT @g.CurveToLineWithTolerance(0.1,0).STNumPoints(), @g.CurveToLineWithTolerance(0.01, 0).STNumPoints();
```  
  
### <a name="b-using-the-method-on-a-multilinestring-instance-containing-one-linestring"></a>B. Verwenden der Methode in einer MultiLineString-Instanz mit einem LineString  
Im folgenden Beispiel wird die Rückgabe einer Instanz von `MultiLineString` gezeigt, die nur eine `LineString` -Instanz enthält:  
  
```
DECLARE @g geography;  
SET @g = geography::Parse('MULTILINESTRING((-122.358 47.653, -122.348 47.649))');  
SELECT @g.CurveToLineWithTolerance(0.1,0).ToString();
```  
  
### <a name="c-using-the-method-on-a-multilinestring-instance-containing-multiple-linestrings"></a>C. Verwenden der Methode in einer MultiLineString-Instanz mit mehreren LineStrings  
Im folgenden Beispiel wird die Rückgabe einer Instanz von `MultiLineString` gezeigt, die mehrere `LineString` -Instanzen enthält:  
  
```
DECLARE @g geography;  
SET @g = geography::Parse('MULTILINESTRING((-122.358 47.653, -122.348 47.649),(-123.358 47.653, -123.348 47.649))');  
SELECT @g.CurveToLineWithTolerance(0.1,0).ToString();
```  
  
### <a name="d-setting-relative-to-true-for-an-invoking-curvepolygon-instance"></a>D: Festlegen des relativen Werts für eine aufrufende CurvePolygon-Instanz auf true  
Im folgenden Beispiel wird eine Instanz von `CurvePolygon` verwendet, um `CurveToLineWithTolerance()` mit *relative* und dem Wert true aufzurufen:  
  
```
DECLARE @g geography = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658), (-122.348 47.658, -122.358 47.658, -122.358 47.653)))';  
SELECT @g.CurveToLineWithTolerance(.5,1).ToString();
```  
  
## <a name="see-also"></a>Weitere Informationen  
[Erweiterte Methoden für geography-Instanzen](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
