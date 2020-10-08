---
description: Verwenden von räumlichen Datentypen
title: Verwenden von räumlichen Datentypen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 02b545ec1d33d17674266d58a2a120f07af423ad
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91727527"
---
# <a name="using-spatial-datatypes"></a>Verwenden von räumlichen Datentypen

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Räumliche Datentypen („Geometry“ and „Geography“) werden ab der Vorschauversion 6.5.0 des JDBC-Treibers unterstützt. Räumliche Datentypen werden bei gespeicherten Prozeduren, Tabellenwertparametern, BulkCopy-Vorgängen und Always Encrypted aktuell nicht unterstützt. Diese Seite zeigt verschiedene Anwendungsfälle der Geometry- und Geography-Datentypen mit dem JDBC-Treiber. Eine Übersicht über räumliche Datentypen finden Sie unter [Übersicht über räumliche Datentypen](../../relational-databases/spatial/spatial-data-types-overview.md).

## <a name="creating-a-geometry--geography-object"></a>Erstellen eines Geometry-/Geography-Objekts

Es gibt zwei primäre Methoden zum Erstellen eines Geometry-/Geography-Objekts: entweder eine Konvertierung aus dem WKT (Well-Known Text)-Format oder aus einem internen SQL Server-Format (CLR).

### <a name="creating-from-wkt"></a>Erstellen eines Objekts aus dem WKT-Format

```java
String geoWKT = "LINESTRING(1 0, 0 1, -1 0)";
Geometry geomWKT = Geometry.STGeomFromText(geoWKT, 0);
Geography geogWKT = Geography.STGeomFromText(geoWKT, 4326);
```

Durch diesen Vorgang wird ein Geometry-Objekt LINESTRING mit SRID 0 (Spatial Reference System Identifier) und ein Geography-Objekt mit SRID 4326 erstellt.

### <a name="creating-from-clr"></a>Erstellen eines Objekts aus der CLR

```java
byte[] geomCLR = Hex.decodeHex("00000000010403000000000000000000F03F00000000000000000000000000000000000000000000F03F000000000000F0BF000000000000000001000000010000000001000000FFFFFFFF0000000002".toCharArray());
byte[] geogCLR = Hex.decodeHex("E61000000104030000000000000000000000000000000000F03F000000000000F03F00000000000000000000000000000000000000000000F0BF01000000010000000001000000FFFFFFFF0000000002".toCharArray());

Geometry geomWKT = Geometry.deserialize(geomCLR);
Geography geogWKT = Geography.deserialize(geogCLR);
```

Durch diesen Vorgang werden ein Geometry- und ein Geography-Objekt erstellt, die den oben erstellten Objekten aus dem WKT-Format entsprechen.

## <a name="working-with-a-geometry--geography-object"></a>Arbeiten mit Geometry-/Geography-Objekten

Gehen wir davon aus, dass ein Benutzer über eine SQL Server-Tabelle wie die unten gezeigte verfügt:

```sql
CREATE TABLE sampleTable (c1 geometry)  
```

Nachfolgend ist ein Beispielskript zum Einfügen eines Geometry-Werts gezeigt:

```java
String geoWKT = "LINESTRING(1 0, 0 1, -1 0)";
Geometry geomWKT = Geometry.STGeomFromText(geoWKT, 0);
SQLServerPreparedStatement pstmt = (SQLServerPreparedStatement) connection.prepareStatement("insert into sampleTable values (?)");
pstmt.setGeometry(1, geomWKT);  
pstmt.execute();
```

Derselbe Vorgang kann für das entsprechende Geography-Objekt durchgeführt werden. Dabei wird eine Geography-Spalte und die **setGeography()**-Methode verwendet.

So lesen Sie eine Geometry-/Geography-Spalte:

```java
try(SQLServerResultSet rs = (SQLServerResultSet)stmt.executeQuery("select * from geomTable")) {
    while(rs.next()){
        rs.getGeometry(1);
    }
}
```

Derselbe Vorgang kann für das entsprechende Geography-Objekt durchgeführt werden. Dabei wird eine Geography-Spalte und die **getGeography()**-Methode verwendet.

## <a name="newly-introduced-apis"></a>Neu eingeführte APIs

Dies sind die neuen öffentlichen APIs, die mit dieser Erweiterung in den Klassen **SQLServerPreparedStatement**, **SQLServerResultSet**, **Geometry** und **Geography** eingeführt wurden.

### <a name="sqlserverpreparedstatement"></a>SQLServerPreparedStatement

|Methode|BESCHREIBUNG|
|:------|:----------|
|void setGeometry(int n, Geometry x)| Legt den angegebenen Parameter auf das angegebene microsoft.sql.Geometry-Klassenobjekt fest.
|void setGeography(int n, Geography x)| Legt den angegebenen Parameter auf das angegebene microsoft.sql.Geography-Klassenobjekt fest.

### <a name="sqlserverresultset"></a>SQLServerResultSet

|Methode|BESCHREIBUNG|
|:------|:----------|
|Geometry getGeometry(int colunIndex)| Gibt den Wert der angegebenen Spalte in der aktuellen Zeile dieses ResultSet-Objekts als com.microsoft.sqlserver.jdbc.Geometry-Objekt in der Programmiersprache Java zurück.
|Geometry getGeometry(String columnName)| Gibt den Wert der angegebenen Spalte in der aktuellen Zeile dieses ResultSet-Objekts als com.microsoft.sqlserver.jdbc.Geometry-Objekt in der Programmiersprache Java zurück.
|Geography getGeography(int colunIndex)| Gibt den Wert der angegebenen Spalte in der aktuellen Zeile dieses ResultSet-Objekts als com.microsoft.sqlserver.jdbc.Geography-Objekt in der Programmiersprache Java zurück.
|Geography getGeography(String columnName)| Gibt den Wert der angegebenen Spalte in der aktuellen Zeile dieses ResultSet-Objekts als com.microsoft.sqlserver.jdbc.Geography-Objekt in der Programmiersprache Java zurück.

### <a name="geometry"></a>Geometrie

|Methode|BESCHREIBUNG|
|:------|:----------|
|Geometry STGeomFromText(String wkt, int SRID)| Konstruktor für eine Geometry-Instanz aus einer WKB-Darstellung (Well-Known Binary) von OGC (Open Geospatial Consortium), die um alle von der Instanz getragenen Z- und M-Werte (Höhe/Measure) erweitert wurde.
|Geometry STGeomFromWKB(byte[] wkb)| Konstruktor für eine Geometry-Instanz aus einer Darstellung des Typs „Open Geospatial-Konsortium (OGC) Well-Known Binary (WKB)“. Hinweis: Diese Methode verwendet derzeit das interne SQL Server-Format (CLR) zum Erstellen einer Geometry-Instanz. Dies ist ein bekanntes Problem im Treiber und soll dahingehend geändert werden, dass stattdessen WKB-Daten akzeptiert werden. Für vorhandene Benutzer, die diese Methode bereits verwenden, sollten Sie in Erwägung ziehen, stattdessen zu deserialize(byte) zu wechseln.
|Geometries deserialize(byte[] clr)| Konstruktor für eine Geometry-Instanz aus einem internen SQL Server-Format für räumliche Daten.
|Geometry parse(String wkt)| Konstruktor für eine Geometry-Instanz aus einer WKB-Darstellung (Well-Known Binary) von OGC (Open Geospatial Consortium). Der SRID (Spatial Reference Identifier) ist standardmäßig auf 0 festgelegt.
|Geometry point(double x, double y, int SRID)| Konstruktor für eine Geometry-Instanz, die mit ihren X- und Y-Werten sowie mit einem Spatial Reference Identifier eine Point-Instanz darstellt.
|String STAsText()| Gibt die WKB-Darstellung (Well-Known Binary) von OGC (Open Geospatial Consortium) einer Geometry-Instanz zurück. Dieser Text enthält keine Z (Höhe)- oder M (Measure)-Werte, die von der Instanz getragen werden.
|byte[] STAsBinary()| Gibt die Darstellung einer Geometry-Instanz im internen SQL Server-Format (CLR) zurück. Dieser Wert enthält keine Z- oder M-Werte, die von der Instanz getragen werden.
|byte[] serialize()| Gibt die Bytes zurück, die ein internes SQL Server-Format vom Geometry-Typ darstellen.
|boolean hasM()| Wird zurückgegeben, wenn das Objekt einen M-Wert (Maßeinheit) enthält.
|boolean hasZ()| Wird zurückgegeben, wenn das Objekt einen Z-Wert (Höhe) enthält.
|Double getX()| Gibt den Wert der X-Koordinate zurück.
|Double getY()| Gibt den Wert der Y-Koordinate zurück.
|Double getM()| Gibt den M-Wert (Maßeinheit) des Objekts zurück.
|Double getZ()| Gibt den Z-Wert (Höhe) des Objekts zurück.
|int getSrid()| Gibt den SRID-Wert (Spatial Reference Identifier) zurück.
|boolean isNull()| Wird zurückgegeben, wenn das Geometry-Objekt NULL ist.
|int STNumPoints()| Gibt die Anzahl von Punkten im Geometry-Objekt zurück.
|String STGeometryType()| Gibt den durch eine geometry-Instanz dargestellten Open Geospatial Consortium (OGC)-Typnamen zurück.
|String asTextZM()| Gibt die WKT-Darstellung (Well-Known Text) des Geometry-Objekts zurück.
|String toString()| Gibt die Zeichenfolgendarstellung des Geometry-Objekts zurück.

### <a name="geography"></a>Geografie

|Methode|BESCHREIBUNG|
|:------|:----------|
|Geography STGeomFromText(String wkt, int SRID)| Konstruktor für eine Geography-Instanz aus einer WKB-Darstellung (Well-Known Binary) von OGC (Open Geospatial Consortium), die um alle von der Instanz getragenen Z- und M-Werte (Höhe/Measure) erweitert wurde.
|Geography STGeomFromWKB(byte[] wkb)| Konstruktor für eine Geography-Instanz aus einer Darstellung des Typs „Open Geospatial-Konsortium (OGC) Well-Known Binary (WKB)“. Hinweis: Diese Methode verwendet derzeit das interne SQL Server-Format (CLR) zum Erstellen einer Geometry-Instanz. In Zukunft wird dieser Ansatz jedoch geändert, dass stattdessen WKB-Daten akzeptiert werden, da das SQL Server-Gegenstück zu dieser Methode (STGeomFromWKB) WKB verwendet. Für vorhandene Benutzer, die diese Methode bereits verwenden, sollten Sie in Erwägung ziehen, stattdessen zu deserialize(byte) zu wechseln.
|Geography deserialize(byte[] clr)| Konstruktor für eine Geography-Instanz aus einem internen SQL Server-Format für räumliche Daten.
|Geography parse(String wkt)| Konstruktor für eine Geography-Instanz aus WKB-Darstellung (Well-Known Binary) von OGC (Open Geospatial Consortium). Der SRID (Spatial Reference Identifier) ist standardmäßig auf 0 festgelegt.
|Geography point(double lon, double lat, int SRID)| Konstruktor für eine Geography-Instanz, die mit Ihren Längen- und Breitengraden sowie mit einem Spatial Reference Identifier eine Point-Instanz darstellt.
|String STAsText()| Gibt die WKB-Darstellung (Well-Known Binary) von OGC (Open Geospatial Consortium) einer Geography-Instanz zurück. Dieser Text enthält keine Z (Höhe)- oder M (Measure)-Werte, die von der Instanz getragen werden.
|byte[] STAsBinary())| Gibt die Darstellung einer Geography-Instanz im internen SQL Server-Format (CLR) zurück. Dieser Wert enthält keine Z- oder M-Werte, die von der Instanz getragen werden.
|byte[] serialize()| Gibt die Bytes zurück, die ein internes SQL Server-Format vom Geography-Typ darstellen.
|boolean hasM()| Wird zurückgegeben, wenn das Objekt einen M-Wert (Maßeinheit) enthält.
|boolean hasZ()| Wird zurückgegeben, wenn das Objekt einen Z-Wert (Höhe) enthält.
|Double getLatitude()| Gibt den Breitengrad zurück.
|Double getLongitude()| Gibt den Längengrad zurück.
|Double getM()| Gibt den M-Wert (Maßeinheit) des Objekts zurück.
|Double getZ()| Gibt den Z-Wert (Höhe) des Objekts zurück.
|int getSrid()| Gibt den SRID-Wert (Spatial Reference Identifier) zurück.
|boolean isNull()| Wird zurückgegeben, wenn das Geography-Objekt NULL ist.
|int STNumPoints()| Gibt die Anzahl von Punkten im Geography-Objekt zurück.
|String STGeographyType()| Gibt den durch eine geography-Instanz dargestellten Open Geospatial Consortium (OGC)-Typnamen zurück.
|String asTextZM()| Gibt die WKT-Darstellung (Well-Known Text) des Geography-Objekts zurück.
|String toString()| Gibt die Zeichenfolgendarstellung des Geography-Objekts zurück.

## <a name="limitations-of-spatial-datatypes"></a>Einschränkungen räumlicher Datentypen

1. Die räumlichen Unterdatentypen **CircularString**, **CompoundCurve**, **CurvePolygon** und **FullGlobe** werden erst ab SQL Server 2012 und höher unterstützt.

2. Always Encrypted kann nicht mit räumlichen Datentypen verwendet werden.

3. Gespeicherte Prozeduren, Tabellenwertparameter und BulkCopy-Vorgänge werden derzeit nicht mit räumlichen Datentypen unterstützt.

## <a name="see-also"></a>Weitere Informationen:

[Beispiel für räumliche Datentypen (JDBC)](../../connect/jdbc/spatial-data-types-sample.md)