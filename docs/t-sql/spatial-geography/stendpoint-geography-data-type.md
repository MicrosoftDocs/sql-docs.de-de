---
description: STEndPoint (geography-Datentyp)
title: STEndPoint (geography-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STEndPoint (geography Data Type)
- STEndPoint_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STEndPoint method
ms.assetid: 8974cd07-8ec4-4126-8fc2-fdcf322ccedd
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 331c20cbde67920b2a2ba25d1d50f59996610626
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183925"
---
# <a name="stendpoint-geography-data-type"></a>STEndPoint (geography-Datentyp)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt den Endpunkt einer **geography** -Instanz zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
.STEndPoint ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **geography**  
  
 CLR-Rückgabetyp: **SqlGeography**  
  
 Open Geospatial Consortium (OGC)-Typ: **Point**  
  
## <a name="remarks"></a>Bemerkungen  
 STEndPoint() entspricht [STPointN](../../t-sql/spatial-geography/stpointn-geography-data-type.md)`(x.STNumPoints``())`.  
  
 Diese Methode gibt NULL zurück, wenn Sie für eine leere **geography** -Instanz aufgerufen wird.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird mit `LineString` eine `STGeomFromText()` -Instanz erstellt und `STEndPoint()` verwendet, um den Endpunkt in der Beschreibung der `LineString`-Instanz abzurufen.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STEndPoint().ToString();  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [OGC-Methoden für geography-Instanzen](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
