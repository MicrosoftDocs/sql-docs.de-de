---
description: STIsValid (geometry-Datentyp)
title: STIsValid (geometry-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STIsValid (geometry Data Type)
- STIsValid_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STIsValid (geometry Data Type)
ms.assetid: 6da39bea-0f67-4660-98fc-d7214f9b2138
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 13abb6eb4e7c28289760eb7a1440c0ae99cca046
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206204"
---
# <a name="stisvalid-geometry-data-type"></a>STIsValid (geometry-Datentyp)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Gibt true zurück, wenn eine **geometry** -Instanz basierend auf ihrem Open Geospatial-Konsortium (OGC)-Typ wohlgeformt ist. Gibt false zurück, wenn eine **geometry** -Instanz nicht wohlgeformt ist.
  
## <a name="syntax"></a>Syntax  
  
```  
  
.STIsValid ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **bit**  
  
 CLR-Rückgabetyp: **SqlBoolean**  
  
## <a name="remarks"></a>Hinweise  
 Der OGC-Typ einer **geometry**-Instanz kann durch einen Aufruf von [STGeometryType()](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md) bestimmt werden.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erzeugt nur gültige **geometry**-Instanzen, erlaubt aber die Speicherung und den Abruf ungültiger Instanzen. Eine gültige Instanz, die die gleiche Punktmenge wie eine ungültige Instanz darstellt, kann mithilfe der `MakeValid()` -Methode abgerufen werden.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird eine `geometry` -Instanz erstellt und `STIsValid()` verwendet, um zu überprüfen, ob die Instanz gültig ist.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);  
SELECT @g.STIsValid();  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [STGeometryType &#40;geometry-Datentyp&#41;](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)   
 [MakeValid &#40;geometry-Datentyp&#41;](../../t-sql/spatial-geometry/makevalid-geometry-data-type.md)   
 [OGC-Methoden für geometry-Instanzen](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

