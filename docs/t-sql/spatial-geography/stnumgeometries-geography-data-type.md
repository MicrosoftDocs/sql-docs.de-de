---
description: STNumGeometries (geography-Datentyp)
title: STNumGeometries (geography-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STNumGeometries (geography Data Type)
- STNumGeometries_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STNumGeometries method
ms.assetid: 6ae7fac2-62f1-420f-9fc9-a09606be9605
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 21dfb949f48d8717062cc2df6abb7ac3d30c7d23
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202116"
---
# <a name="stnumgeometries-geography-data-type"></a>STNumGeometries (geography-Datentyp)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt die Anzahl der **geometries** -Instanzen zurück, aus denen die **geography** -Instanz besteht.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
.STNumGeometries ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **int**  
  
 CLR-Rückgabetyp: **SqlInt32**  
  
## <a name="remarks"></a>Hinweise  
 Diese Methode gibt 1 zurück, wenn die **geography** -Instanz keine **MultiPoint**-, **MultiLineString**-, **MultiPolygon**- oder **GeometryCollection** -Instanz ist. Sie gibt 0 zurück, wenn die **geography** -Instanz leer ist.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird eine `MultiPoint` -Instanz erstellt und `STNumGeometries()` verwendet, um festzustellen, wieviele **geometries** die Instanz enthält.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('MULTIPOINT((-122.360 47.656), (-122.343 47.656))', 4326);  
SELECT @g.STNumGeometries();  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [OGC-Methoden für geography-Instanzen](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
