---
description: BufferWithCurves (geometry-Datentyp)
title: BufferWithCurves (geometry-Datentyp) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- BufferWithCurves method (geometry)
ms.assetid: 8ffaba3f-d2dd-4e57-9f41-3ced9f14b600
author: MladjoA
ms.author: mlandzic
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 18691c1a3e02d93d7c98a6b290672af5c77f15c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347537"
---
# <a name="bufferwithcurves-geometry-data-type"></a>BufferWithCurves (geometry-Datentyp)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Gibt eine Instanz von **geometry** zurück, die die Menge aller Punkte darstellt, deren Abstand von der aufrufenden Instanz von **geometry** kleiner oder gleich dem Wert des *distance*-Parameters ist.  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
.BufferWithCurves ( distance )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente  
 *distance*  
 Ein **float**-Wert, der den maximalen Abstand der Punkte von der **geometry**-Instanz angibt, die den Puffer bilden.  
  
## <a name="return-types"></a>Rückgabetypen
SQL Server-Rückgabetyp: **geometry**  
  
 CLR-Rückgabetyp: **SqlGeometry**  
  
## <a name="exceptions"></a>Ausnahmen  
 Die folgenden Kriterien lösen eine **ArgumentException** aus.  
  
-   Es wird kein Parameter an die Methode übergeben, beispielsweise `@g.BufferWithCurves()`  
  
-   Es wird ein nicht numerischer Parameter an die Methode übergeben, beispielsweise `@g.BufferWithCurves('a')`  
  
-   **NULL** wird an die Methode übergeben, beispielsweise `@g.BufferWithCurves(NULL)`  
  
## <a name="remarks"></a>Bemerkungen  
 Die folgende Abbildung zeigt ein Beispiel für eine geometry-Instanz an, die von dieser Methode zurückgegeben wurde.  
  
 ![Diagramm: Beispiel für eine von dieser Methode zurückgegebene Geometrieinstanz](../../t-sql/spatial-geometry/media/bufferedcurve.gif)
  
 Die folgende Tabelle zeigt die Ergebnisse an, die für verschiedene Entfernungswerte zurückgegeben wurden.  
  
|distance-Wert|Type-Dimensionen|Räumlicher Rückgabetyp|  
|--------------------|---------------------|---------------------------|  
|distance < 0|Null oder eins|Leere Instanz von **GeometryCollection**|  
|distance < 0|Zwei oder mehr|Eine Instanz von **CurvePolygon** oder **GeometryCollection** mit einem negativen Puffer. **Hinweis:** Ein negativer Puffer erstellt möglicherweise eine leere Instanz von **GeometryCollection**.|  
|distance = 0|Alle Dimensionen|Kopie der aufrufenden Instanz von **geometry**|  
|distance > 0|Alle Dimensionen|Instanz von **CurvePolygon** oder **GeometryCollection**|  
  
> [!NOTE]  
>  Da *distance* ein **float**-Wert ist, kann ein sehr kleiner Wert in den Berechnungen mit 0 gleichgesetzt werden. In diesem Fall wird eine Kopie der aufrufenden Instanz von **geometry** zurückgegeben. Informationen hierzu finden Sie unter [float und real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md).  
  
 Ein negativer Puffer entfernt alle Punkte innerhalb des gegebenen Abstands der Begrenzung der geometry-Instanz. In der folgenden Abbildung wird ein negativer Puffer als heller schattierter Bereich des Kreises angezeigt. Die gepunktete Linie ist die Grenze des ursprünglichen Polygons, und die durchgezogenen Linie ist die Grenze des resultierenden Polygons.  
  
 Wenn ein **string**-Parameter an die Methode übergeben wird, erfolgt eine Konvertierung in einen **float**-Wert, oder eine `ArgumentException` wird ausgelöst.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-calling-bufferwithcurves-with-a-parameter-value--0-on-one-dimensional-geometry-instance"></a>A. Aufrufen von BufferWithCurves() mit einem Parameterwert < 0 für eine eindimensionale geometry-Instanz  
 Im folgenden Beispiel wird eine leere Instanz von `GeometryCollection` zurückgegeben:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(-1).ToString(); 
 ```
  
### <a name="b-calling-bufferwithcurves-with-a-parameter-value--0-on-a-two-dimensional-geometry-instance"></a>B. Aufrufen von BufferWithCurves() mit einem Parameterwert < 0 für eine zweidimensionale geometry-Instanz  
 Im folgenden Beispiel wird eine Instanz von `CurvePolygon` mit einem negativen Puffer zurückgegeben:  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.BufferWithCurves(-1).ToString()
 ```  
  
### <a name="c-calling-bufferwithcurves-with-a-parameter-value--0-that-returns-an-empty-geometrycollection"></a>C. Aufrufen von BufferWithCurves() mit einem Parameterwert < 0 zur Rückgabe einer leeren GeometryCollection  
 Im folgenden Beispiel wird gezeigt, was geschieht, wenn der *distance*-Parameter –2 entspricht:  
  
```
 DECLARE @g geometry = 'CURVEPOLYGON(COMPOUNDCURVE(CIRCULARSTRING(0 4, 4 0, 8 4), (8 4, 0 4)))'; 
 SELECT @g.BufferWithCurves(-2).ToString();
 ```  
  
 Diese **SELECT**-Anweisung gibt `GEOMETRYCOLLECTION EMPTY` zurück.  
  
### <a name="d-calling-bufferwithcurves-with-a-parameter-value--0"></a>D: Aufrufen von BufferWithCurves() mit einem Parameterwert = 0  
 Im folgenden Beispiel wird eine Kopie der aufrufenden Instanz von **geometry** zurückgegeben:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(0).ToString();
 ```  
  
### <a name="e-calling-bufferwithcurves-with-a-non-zero-parameter-value-that-is-extremely-small"></a>E. Aufrufen von BufferWithCurves() mit einem sehr kleinen Parameterwert ungleich 0  
 Im folgenden Beispiel wird ebenfalls eine Kopie der aufrufenden Instanz von **geometry** zurückgegeben:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)'; 
 DECLARE @distance float = 1e-20; 
 SELECT @g.BufferWithCurves(@distance).ToString();
 ```  
  
### <a name="f-calling-bufferwithcurves-with-a-parameter-value--0"></a>F. Aufrufen von BufferWithCurves() mit einem Parameterwert > 0  
 Im folgenden Beispiel wird eine Instanz von `CurvePolygon` zurückgegeben:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves(2).ToString();
 ```  
  
### <a name="g-passing-a-valid-string-parameter"></a>G. Übergeben eines gültigen Zeichenfolgenparameters  
 Im folgenden Beispiel wird die gleiche Instanz von `CurvePolygon` zurückgegeben, die bereits erwähnt wurde, es wird jedoch ein Zeichenfolgenparameter an die Methode übergeben:  
  
```
 DECLARE @g geometry= 'LINESTRING(3 4, 8 11)'; 
 SELECT @g.BufferWithCurves('2').ToString();
 ```  
  
### <a name="h-passing-an-invalid-string-parameter"></a>H. Übergeben eines ungültigen Zeichenfolgenparameters  
 Im folgenden Beispiel wird ein Fehler ausgelöst:  
  
```
 DECLARE @g geometry = 'LINESTRING(3 4, 8 11)' 
 SELECT @g.BufferWithCurves('a').ToString();
 ```  
  
 Beachten Sie, dass in den beiden vorangehenden Beispielen ein Zeichenfolgenliteral an die `BufferWithCurves()`-Methode übergeben wurde. Das erste Beispiel kann ordnungsgemäß ausgeführt werden, da das Zeichenfolgenliteral in einen numerischen Wert konvertiert werden kann. Im zweiten Beispiel wird allerdings eine `ArgumentException`ausgelöst.  
  
### <a name="i-calling-bufferwithcurves-on-multipoint-instance"></a>I. Aufrufen von BufferWithCurves() für eine MultiPoint-Instanz  
 Im folgenden Beispiel werden zwei Instanzen von `GeometryCollection` sowie eine Instanz von `CurvePolygon` zurückgegeben:  
  
```
 DECLARE @g geometry = 'MULTIPOINT((1 1),(1 4))'; 
 SELECT @g.BufferWithCurves(1).ToString(); 
 SELECT @g.BufferWithCurves(1.5).ToString(); 
 SELECT @g.BufferWithCurves(1.6).ToString();
 ```  
  
 Von den ersten beiden **SELECT**-Anweisungen wird eine Instanz von `GeometryCollection` zurückgegeben, da der *distance*-Parameter kleiner oder gleich 1/2 des Abstands zwischen den beiden Punkten (1 1) und (1 4) ist. Von der dritten **SELECT**-Anweisung gibt `CurvePolygon` zurückgegeben, da sich die zwischengespeicherten Instanzen der beiden Punkte (1 1) und (1 4) überschneiden.  
  
## <a name="see-also"></a>Siehe auch  
 [Erweiterte Methoden für geometry-Instanzen](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
 
