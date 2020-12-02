---
description: STBuffer (geography-Datentyp)
title: STBuffer (geography-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STBuffer (geography Data Type)
- STBuffer_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STBuffer (geography Data Type)
ms.assetid: cb4deab8-642b-44d9-b3d9-85114d64021e
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 1d39e58c6dd4fa648d8d4118414925777eb3535b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "92038315"
---
# <a name="stbuffer-geography-data-type"></a>STBuffer (geography-Datentyp)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Gibt ein geography-Objekt zurück, dass die Vereinigung aller Punkte darstellt, deren Abstand zu einer Instanz von **geography** kleiner oder gleich einem angegebenen Wert ist.  
  
 Diese geography -Datentypmethode unterstützt Instanzen von **FullGlobe** oder räumliche Instanzen, die größer als eine Hemisphäre sind.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
.STBuffer ( distance )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *distance*  
 Ein Wert vom Typ **float** (**double** in .NET-Framework), der den Abstand zu der **geography**-Instanz angibt, um die der Puffer berechnet werden soll.  
  
 Der maximale Abstand des Puffers darf 0,999 × *π* × minorAxis × minorAxis / majorAxis (~0,999 × 1/2 Erdumfang) oder den vollständigen Erdumfang nicht überschreiten.  
  
## <a name="return-types"></a>Rückgabetypen  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rückgabetyp: **geography**  
  
 CLR-Rückgabetyp: **SqlGeography**  
  
## <a name="remarks"></a>Bemerkungen  
 STBuffer() berechnet einen Puffer auf die gleiche Weise wie [BufferWithTolerance](../../t-sql/spatial-geography/bufferwithtolerance-geography-data-type.md), wobei *Toleranz* = abs(distance) \* 0,001 und *relativ* = **false** ist.  
  
 Ein negativer Puffer entfernt alle Punkte innerhalb des angegebenen Abstands von der Begrenzung der **geography**-Instanz.  
  
 `STBuffer()` gibt in bestimmten Fällen eine Instanz von **FullGlobe** zurück. Beispielsweise gibt `STBuffer()` eine **FullGlobe**-Instanz zurück, wenn der Pufferabstand größer als der Abstand vom Äquator zu den Polen ist. Ein Puffer kann nicht über die vollständige Kugel hinausgehen.  
  
 Diese Methode löst eine **ArgumentException** in **FullGlobe**-Instanzen aus, bei denen der Abstand des Puffers die folgende Einschränkung überschreitet:  
  
 0,999 × *π* × minorAxis × minorAxis / majorAxis (~0,999 × 1/2 Erdumfang)  
  
 Die Begrenzung des maximal möglichen Abstands ermöglicht das Erstellen von äußerst flexiblen Puffern.  
  
 Die Abweichung zwischen dem theoretischen und dem berechneten Puffer beträgt max(tolerance, extents * 1,E-7), wobei tolerance = distance \* 0,001 ist. Weitere Informationen zu Erweiterungen finden Sie unter [geography-Datentyp-Methodenverweis](./stequals-geography-data-type.md).  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird eine `LineString``geography`-Instanz erstellt. Anschließend wird `STBuffer()` verwendet, um den Bereich innerhalb von 1 Meter Umkreis um die Instanz zurückzugeben.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STBuffer(1).ToString();  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [BufferWithTolerance &#40;geography-Datentyp&#41;](../../t-sql/spatial-geography/bufferwithtolerance-geography-data-type.md)   
 [OGC-Methoden für geography-Instanzen](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
