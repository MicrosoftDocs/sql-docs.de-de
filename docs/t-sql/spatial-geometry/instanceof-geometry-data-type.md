---
description: InstanceOf (geometry-Datentyp)
title: InstanceOf (geometry-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- InstanceOf
- InstanceOf_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- InstanceOf (geometry Data Type)
ms.assetid: fdea1248-29a4-4bab-a60d-a1b359b5e109
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 5b79e0d67f11258afb8cf7aeebbfc4d52b9f4907
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "88497056"
---
# <a name="instanceof-geometry-data-type"></a>InstanceOf (geometry-Datentyp)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Eine Methode, die die Übereinstimmung der **geometry**-Instanz mit dem angegebenen Typ überprüft. Gibt „1“ zurück, wenn der Typ einer **geometry**-Instanz dem angegebenen Typ entspricht. Diese Methode gibt auch „1“ zurück, wenn der angegebene Typ ein Vorgänger des Instanztyps ist. Andernfalls gibt diese Methode den Wert „0“ zurück.
  
## <a name="syntax"></a>Syntax  
  
```  
  
.InstanceOf (geometry_type )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*geometry_type*  
Eine **nvarchar(4000)**-Zeichenfolge, die einen von 15 Typen angibt, die in der **geometry**-Typhierarchie verfügbar gemacht werden.  
  
## <a name="return-types"></a>Rückgabetypen  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **bit**  
  
 CLR-Rückgabetyp: **SqlBoolean**  
  
## <a name="remarks"></a>Hinweise  
 Die Eingabe für die Methode muss einem der folgenden Typen entsprechen: **Geometry**, **Point**, **Curve**, **LineString**, **CircularString**, **CompoundCurve**, **Surface**, **Polygon**, **CurvePolygon**, **GeometryCollection**, **MultiSurface**, **MultiPolygon**, **MultiCurve**, **MultiLineString** oder **MultiPoint**. Diese Methode löst eine **ArgumentException** aus, wenn andere Zeichenfolgen für die Eingabe verwendet werden.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird eine `MultiPoint`-Instanz erstellt und `InstanceOf()` verwendet, um zu überprüfen, ob die Instanz eine `GeometryCollection` ist.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('MULTIPOINT(0 0, 13.5 2, 7 19)', 0);  
SELECT @g.InstanceOf('GEOMETRYCOLLECTION');  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Erweiterte Methoden für geometry-Instanzen](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
  
  

