---
description: STIsRing (geometry-Datentyp)
title: STIsRing (geometry-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STIsRing_TSQL
- STIsRing (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STIsRing (geometry Data Type)
ms.assetid: ea0063be-1c74-4cc4-ac6f-b65321ddfa54
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: b0f0cb68a9923491e64fa63f52b6d481abb73c14
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205007"
---
# <a name="stisring-geometry-data-type"></a>STIsRing (geometry-Datentyp)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Gibt 1 zurück, wenn eine **geometry** -Instanz die folgenden Anforderungen erfüllt:
-   Sie ist eine **LineString** -Instanz.  
-   Sie ist geschlossen.  
-   Sie ist einfach.  
-   Gibt 0 zurück, wenn die **LineString** -Instanz die Anforderungen nicht erfüllt.  

 Damit eine **geometry**-Instanz als geschlossen und einfach betrachtet werden kann, müssen sowohl [STIsClosed()](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md) als auch [STIsSimple()](../../t-sql/spatial-geometry/stissimple-geometry-data-type.md) 1 zurückgeben, wenn sie für die Instanz aufgerufen werden. Sie können den Instanztyp einer **geometry** mithilfe von [STGeometryType()](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md) ermitteln.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
.STIsRing ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **bit**  
  
 CLR-Rückgabetyp: **SqlBoolean**  
  
## <a name="remarks"></a>Hinweise  
 Diese Methode gibt NULL zurück, wenn die Instanz kein **LineString** ist.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird eine `LineString` -Instanz erstellt und `STIsRing()` verwendet, um zu überprüfen, ob die Instanz ein Ring ist.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0, 0 0)', 0);  
SELECT @g.STIsRing();  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [STIsClosed &#40;geometry-Datentyp&#41;](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md)   
 [STGeometryType &#40;geometry-Datentyp&#41;](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)   
 [STIsSimple &#40;geometry-Datentyp&#41;](../../t-sql/spatial-geometry/stissimple-geometry-data-type.md)   
 [OGC-Methoden für geometry-Instanzen](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

