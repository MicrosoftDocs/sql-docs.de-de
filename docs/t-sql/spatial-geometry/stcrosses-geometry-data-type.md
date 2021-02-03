---
description: STCrosses (geometry-Datentyp)
title: STCrosses (geometry-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STCrosses (geometry Data Type)
- STCrosses_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STCrosses (geometry Data Type)
ms.assetid: 3e3fc065-555a-4bee-8b71-e92f3dc62a4f
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 3db656100e4d3eb53a28c0a3f3a2e5c25c6cf3be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187692"
---
# <a name="stcrosses-geometry-data-type"></a>STCrosses (geometry-Datentyp)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Gibt 1 zurück, wenn eine **geometry** -Instanz eine andere **geometry** -Instanz überkreuzt. Andernfalls wird 0 zurückgegeben.
  
## <a name="syntax"></a>Syntax  
  
```  
  
.STCrosses ( other_geometry )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *other_geometry*  
 Eine andere **geometry** -Instanz zum Vergleich mit der Instanz, in der `STCrosses()` aufgerufen wird.  
  
## <a name="return-types"></a>Rückgabetypen  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **bit**  
  
 CLR-Rückgabetyp: **SqlBoolean**  
  
## <a name="remarks"></a>Hinweise  
 Zwei **geometry** -Instanzen überkreuzen sich, wenn die beiden folgenden Bedingungen zutreffen:  
  
-   Die Schnittmenge zweier **geometry** -Instanzen ergeben eine geometry-Instanz, deren Dimensionen kleiner sind als die maximalen Dimensionen der **geometry** -Quellinstanzen.  
  
-   Der Schnittmengensatz liegt im Inneren beider **geometry** -Quellinstanzen.  
  
 Diese Methode gibt immer NULL zurück, wenn die SRIDs (Spatial Reference IDs) der **geometry** -Instanzen nicht übereinstimmen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird `STCrosses()` verwendet, um zwei `geometry` -Instanzen daraufhin zu überprüfen, ob sie sich überkreuzen.  
  
```  
DECLARE @g geometry;  
DECLARE @h geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 2, 2 0)', 0);  
SET @h = geometry::STGeomFromText('LINESTRING(0 0, 2 2)', 0);  
SELECT @g.STCrosses(@h);  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [OGC-Methoden für geometry-Instanzen](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

