---
description: STGeomCollFromWKB (geography-Datentyp)
title: STGeomCollFromWKB (geography-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STGeomCollFromWKB (geography Data Type)
- STGeomCollFromWKB_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STMGeomCollFromWKB method
ms.assetid: bbed028c-9cd6-4236-b5e5-8e914a21f2e4
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: d6e669f286277f3c8aaff4e3f659d2995a18abe3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205025"
---
# <a name="stgeomcollfromwkb-geography-data-type"></a>STGeomCollFromWKB (geography-Datentyp)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Gibt eine **GeometryCollection**-Instanz von einer Open Geospatial Consortium (OGC) Well-Known Binary (WKB)-Darstellung zurück.
  
## <a name="syntax"></a>Syntax  
  
```  
  
STGeomCollFromWKB ( 'WKB_geometrycollection' , SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *WKB_geometrycollection*  
 Die WKB-Darstellung der Instanz von **GeometryCollection** , die zurückgegeben werden soll. *WKB_geometrycollection* ist ein **varbinary(max)** -Ausdruck.  
  
 *SRID*  
 Ein **int** -Ausdruck, der den SRID (Spatial Reference ID) der **GeometryCollection** -Instanz darstellt, die Sie zurückgeben möchten.  
  
## <a name="return-types"></a>Rückgabetypen  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **geography**  
  
 CLR-Rückgabetyp: **SqlGeography**  
  
## <a name="remarks"></a>Bemerkungen  
 Der OGC-Typ der von STGeomCollFromWKB() zurückgegebenen **geography**-Instanz wird, abhängig von der entsprechenden WKB-Eingabe, auf **GeometryCollection**, **MultiPolygon**, **MultiLineString** oder **MultiPoint** festgelegt.  
  
 Diese Methode löst eine **FormatException** -Ausnahme aus, wenn die Eingabe nicht korrekt formatiert ist.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird `STGeomCollFromWKB()` verwendet, um eine `geography`-Instanz zu erstellen.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomCollFromWKB(0x01070000000200000001010000007593180456965EC017D9CEF753D34740010200000002000000D7A3703D0A975EC08716D9CEF7D34740CBA145B6F3955EC08716D9CEF7D34740, 4326);  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Statische geography-Methoden des OGC](../../t-sql/spatial-geography/ogc-static-geography-methods.md)  
  
  
