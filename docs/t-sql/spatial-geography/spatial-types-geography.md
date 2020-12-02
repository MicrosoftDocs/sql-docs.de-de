---
description: Räumliche Typen - geography
title: geography (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- geography
dev_langs:
- TSQL
helpviewer_keywords:
- geography data type [SQL Server], Transact-SQL
- spatial data types [SQL Server]
ms.assetid: d9e4952a-1841-4465-a64b-11e9288dba1d
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 46489eadd2c56fbccca62dfe415611e0f8f66a2a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "88467474"
---
# <a name="spatial-types---geography"></a>Räumliche Typen - geography
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Der Räumlichkeitsdatentyp **geography** wird in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als .NET-CLR-Datentyp (Common Language Runtime) implementiert. Dieser Typ stellt Daten in einem Erdkugel-Koordinatensystem dar. Der -Datentyp [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **geography** speichert elliptische Daten (Globusmodell) wie GPS-Koordinaten in Längen- und Breitengraden.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt einen Satz von Methoden für den räumlichen **geography**-Datentyp. Dazu gehören Methoden für **geography**, die im OGC-Standard (Open Geospatial Consortium) sowie in einer Gruppe von [!INCLUDE[msCoName](../../includes/msconame-md.md)]-Erweiterungen dieses Standards definiert sind.  
 
 Die Fehlertoleranz für die **geography**-Methoden kann bis zu 1,0e-7 * Erweiterungen groß sein. Die Erweiterungen verweisen auf den ungefähren maximalen Abstand zwischen Punkten des **geography**-Objekts.
  

## <a name="registering-the-geography-type"></a>Registrieren des geography-Datentyps  
 Der **geography** -Typ ist vordefiniert und in jeder Datenbank verfügbar. Sie können Tabellenspalten des **geography** -Typs in der gleichen Weise erstellen und **geography** -Daten in der gleichen Weise verwenden wie andere vom System bereitgestellte Typen. Kann in persistierten und nicht persistierten berechneten Spalten verwendet werden.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-showing-how-to-add-and-query-geography-data"></a>A. Darstellung des Hinzufügens und Abfragens von Geografiedaten  
 Die folgenden Beispiele zeigen, wie Geografiedaten hinzugefügt und abgefragt werden. Im ersten Beispiel wird eine Tabelle mit einer Identitätsspalte und der `geography`-Spalte `GeogCol1` erstellt. Eine dritte Spalte rendert die `geography` -Spalte als Darstellung im Open Geospatial Consortium (OGC) WKT-Format und verwendet die `STAsText()` -Methode. Dann werden zwei Zeilen eingefügt: Eine Zeile enthält eine `LineString` -Instanz des Typs `geography`und die andere eine `Polygon` -Instanz.  
  
```sql  
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
    DROP TABLE dbo.SpatialTable;  
GO  
  
CREATE TABLE SpatialTable   
    ( id int IDENTITY (1,1),  
    GeogCol1 geography,   
    GeogCol2 AS GeogCol1.STAsText() );  
GO  
  
INSERT INTO SpatialTable (GeogCol1)  
VALUES (geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656 )', 4326));  
  
INSERT INTO SpatialTable (GeogCol1)  
VALUES (geography::STGeomFromText('POLYGON((-122.358 47.653 , -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326));  
GO  
```  
  
### <a name="b-returning-the-intersection-of-two-geography-instances"></a>B. Zurückgeben der Schnittmenge von zwei geography-Instanzen  
 Im folgenden Beispiel werden mithilfe der `STIntersection()`-Methode die Punkte zurückgegeben, an denen die beiden zuvor eingegebenen `geography`-Instanzen sich schneiden.  
  
```sql  
DECLARE @geog1 geography;  
DECLARE @geog2 geography;  
DECLARE @result geography;  
  
SELECT @geog1 = GeogCol1 FROM SpatialTable WHERE id = 1;  
SELECT @geog2 = GeogCol1 FROM SpatialTable WHERE id = 2;  
SELECT @result = @geog1.STIntersection(@geog2);  
SELECT @result.STAsText();  
```  
  
### <a name="c-using-geography-in-a-computed-column"></a>C. Verwenden des geography-Typs in einer berechneten Spalte  
 Im folgenden Beispiel wird eine Tabelle mit einer persistierten berechneten Spalte mit einem **geography**-Typ erstellt.  
  
```sql  
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
    DROP TABLE dbo.SpatialTable;  
GO  
  
CREATE TABLE SpatialTable  
(  
    locationId int IDENTITY(1,1),  
    location geography,  
    deliveryArea as location.STBuffer(10) persisted  
);  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Räumliche Daten &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)   

  
  
