---
description: Parse (geography-Datentyp)
title: Parse (geography-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- Parse method
- Parse (geography Data Type)
ms.assetid: 21c402fa-fd0f-4d09-a097-49cee0316d4e
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 9ba373a279750e7399881edcc072dc3a086ec5e2
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "88360006"
---
# <a name="parse-geography-data-type"></a>Parse (geography-Datentyp)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Gibt eine **geography** -Instanz aus einer Open Geospatial-Konsortium (OGC) Well-Known Text (WKT)-Darstellung zurück. Parse() entspricht [STGeomFromText](../../t-sql/spatial-geography/stgeomfromtext-geography-data-type.md), setzt aber eine SRID (Spatial Reference ID) mit dem Wert 4326 als Parameter voraus. Die Eingabe kann optional Z- (Höhe) und M-Werte (Measure) tragen.
  
Diese **geography** -Datentypmethode unterstützt Instanzen von **FullGlobe** oder räumliche Instanzen, die größer als eine Hemisphäre sind.
  
## <a name="syntax"></a>Syntax  
  
```  
  
Parse ( 'geography_tagged_text' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *geography_tagged_text*  
 Die WKT-Darstellung der Instanz von **geography** , die zurückgegeben werden soll. *geography_tagged_text* ist ein **nvarchar** -Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **geography**  
  
 CLR-Rückgabetyp: **SqlGeography**  
  
## <a name="remarks"></a>Bemerkungen  
 Der OGC-Typ der **geography** -Instanz, die von `Parse()` zurückgegeben wird, wird auf die entsprechende WKT-Eingabe festgelegt.  
  
 Die Zeichenfolge 'Null' wird als eine NULL- **geography** -Instanz interpretiert.  
  
 Diese Methode löst **ArgumentException** aus, wenn die Eingabe eine gegenüberliegende Kante enthält.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird `Parse()` verwendet, um eine `geography`-Instanz zu erstellen.  
  
```  
DECLARE @g geography;   
SET @g = geography::Parse('LINESTRING(-122.360 47.656, -122.343 47.656)');  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erweiterte statische geography-Methoden](../../t-sql/spatial-geography/extended-static-geography-methods.md)   
 [STGeomFromText &#40;geography-Datentyp&#41;](../../t-sql/spatial-geography/stgeomfromtext-geography-data-type.md)  
  
  
