---
description: MultiPoint
title: MultiPoint | Microsoft-Dokumentation
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- MultiPoint geometry subtype [SQL Server]
ms.assetid: 2aaab211-3aba-4dbd-90b7-095d997b1f62
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2a64b2d0e640a2c16fc04491e9db307fca82633a
ms.sourcegitcommit: a5398f107599102af7c8cda815d8e5e9a367ce7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "92006288"
---
# <a name="multipoint"></a>MultiPoint
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
   Ein **MultiPoint** ist eine Auflistung von null oder mehr Punkten. Die Begrenzung einer **MultiPoint** -Instanz ist leer.  
  
## <a name="examples"></a>Beispiele  

### <a name="example-a"></a>Beispiel A.
Im folgenden Beispiel wird eine `geometry MultiPoint` -Instanz mit dem SRID 23 und zwei Punkten erstellt: ein Punkt mit den Koordinaten (2, 3), ein Punkt mit den Koordinaten (7, 8) und ein Z-Wert von 9,5.  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('MULTIPOINT((2 3), (7 8 9.5))', 23);  
```  
  
### <a name="example-b"></a>Beispiel B. 
Im folgenden Beispiel wird die `MultiPoint`-Instanz mit `STMPointFromText()` ausgedrückt.  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STMPointFromText('MULTIPOINT((2 3), (7 8 9.5))', 23);  
```  
  
### <a name="example-c"></a>Beispiel C.
Im folgenden Beispiel wird mit der Methode `STGeometryN()` eine Beschreibung des ersten Punkts der Auflistung abgerufen.  
  
```sql  
SELECT @g.STGeometryN(1).STAsText();  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Punkt](../../relational-databases/spatial/point.md)   
 [Räumliche Daten &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
