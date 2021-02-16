---
description: OPENJSON (Transact-SQL)
title: OPENJSON (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENJSON
- OPENJSON_TSQL
helpviewer_keywords:
- OPENJSON rowset function
- JSON, importing
- JSON, converting from
ms.assetid: 233d0877-046b-4dcc-b5da-adeb22f78531
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: b29315567520dff0d4700aeb476279bf6cfe24b7
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100340644"
---
# <a name="openjson-transact-sql"></a>OPENJSON (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

**OPENJSON** ist eine Tabellenwertfunktion, die JSON-Text analysiert und Objekte und Eigenschaften aus der JSON-Eingabe als Zeilen und Spalten zurückgibt. Das heißt, dass **OPENJSON** eine Rowsetansicht eines JSON-Dokuments bereitstellt. Sie können die Spalten im Rowset und die JSON-Eigenschaftspfade zum Auffüllen der Spalten angeben. Da **OPENJSON** einen Satz von Zeilen zurückgibt, können Sie **OPENJSON** in der `FROM`-Klausel einer [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung nutzen, genauso wie Sie jede Tabelle, Ansicht oder Tabellenwertfunktion verwenden können.  
  
Verwenden Sie **OPENJSON**, um die JSON-Daten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu importieren, oder um die JSON-Daten für eine Anwendung oder einen Dienst, die JSON nicht nutzen können, in ein relationales Format zu konvertieren.  
  
> [!NOTE]  
>  Die **OPENJSON**-Funktion steht nur für den Kompatibilitätsgrad 130 oder höher zur Verfügung. Wenn der Kompatibilitätsgrad Ihrer Datenbank kleiner als 130 ist, kann SQL Server die **OPENJSON**-Funktion nicht finden und ausführen. Andere JSON-Funktionen sind für alle Kompatibilitätsgrade verfügbar.
> 
> Sie können den Kompatibilitätsgrad in der `sys.databases`-Ansicht oder in den Datenbankeigenschaften überprüfen. Sie können den Kompatibilitätsgrad einer Datenbank mithilfe des folgenden Befehls ändern:  
> 
> `ALTER DATABASE DatabaseName SET COMPATIBILITY_LEVEL = 130`
>
> Kompatibilitätsgrad 120 kann auch in einer neuen Azure SQL-Datenbank die Standardeinstellung sein.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink")[Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
OPENJSON( jsonExpression [ , path ] )  [ <with_clause> ]

<with_clause> ::= WITH ( { colName type [ column_path ] [ AS JSON ] } [ ,...n ] )
```  

Die Tabellenwertfunktion **OPENJSON** analysiert *jsonExpression*, was als erstes Argument bereitgestellt wird, und gibt eine oder mehrere Zeilen mit Daten aus den JSON-Objekten im Ausdruck zurück. *jsonExpression* kann geschachtelte, untergeordnete Objekte enthalten. Wenn Sie ein untergeordnetes Objekts innerhalb eines *jsonExpression*-Ausdrucks analysieren möchten, können Sie einen **path**-Parameter für das untergeordnete JSON-Objekt angeben.

### <a name="openjson"></a>openjson

![Syntax für OPENJSON TVF](../../relational-databases/json/media/openjson-syntax.png "OPENJSON-Syntax")  

Die Tabellenwertfunktion **OPENJSON** gibt standardmäßig drei Spalten zurück, die den Schlüsselnamen, den Wert und den Typ jedes {Schlüssel-Wert}-Paars im *jsonExpression* enthalten. Als Alternative können Sie das Schema des Ergebnissets, das **OPENJSON** zurückgibt, explizit angeben, indem Sie die *with_clause* bereitstellen.
  
### <a name="with_clause"></a>with_clause
  
![Syntax für die WITH-Klausel in OPENJSON TVF](../../relational-databases/json/media/openjson-shema-syntax.png "OPENJSON WITH-Syntax")

*with_clause* enthält eine Liste von Spalten mit Typen, die **OPENJSON** zurückgibt. Standardmäßig ordnet **OPENJSON** Schlüssel im *jsonExpression* den Spaltennamen in *with_clause* zu (in diesem Fall setzen Zuordnungen von Schlüsseln voraus, dass die Groß-/Kleinschreibung beachtet wird). Wenn ein Spaltenname und ein Schlüsselname nicht übereinstimmen, können Sie einen optionalen *column_path* bereitstellen. Dabei handelt es sich um einen [JSON-Pfadausdruck](../../relational-databases/json/json-path-expressions-sql-server.md), der auf einen Schlüssel innerhalb von *jsonExpression* verweist. 

## <a name="arguments"></a>Argumente

### <a name="jsonexpression"></a>*jsonExpression*

Ein Unicode-Zeichenausdruck, der JSON-Text enthält.  
  
OPENJSON führt eine Iteration durch die Elemente eines Arrays oder die Eigenschaften des Objekts im JSON-Ausdruck durch, und gibt eine Zeile für jedes Element oder jede Eigenschaft zurück. Das folgende Beispiel gibt jede Eigenschaft des Objekts, das als *jsonExpression* bereitgestellt wird, zurück:  

```sql
DECLARE @json NVARCHAR(2048) = N'{
   "String_value": "John",
   "DoublePrecisionFloatingPoint_value": 45,
   "DoublePrecisionFloatingPoint_value": 2.3456,
   "BooleanTrue_value": true,
   "BooleanFalse_value": false,
   "Null_value": null,
   "Array_value": ["a","r","r","a","y"],
   "Object_value": {"obj":"ect"}
}';

SELECT * FROM OpenJson(@json);
```

**Ergebnisse:**

| Schlüssel                                | value                 | type |
| :--                                | :----                 | :--- |
| String_value                       | John                  | 1 |
| DoublePrecisionFloatingPoint_value | 45                    | 2 |
| DoublePrecisionFloatingPoint_value | 2.3456                | 2 |
| BooleanTrue_value                  | true                  | 3 |
| BooleanFalse_value                 | false                 | 3 |
| Null_value                         | NULL                  | 0 |
| Array_value                        | ["a","r","r","a","y"] | 4 |
| Object_value                       | {"obj":"ect"}         | 5 |
| &nbsp; | &nbsp; | &nbsp; |

- DoublePrecisionFloatingPoint_value entspricht IEEE-754.

### <a name="path"></a>*path*

Ist ein optionaler JSON-Pfadausdruck, der auf ein Objekt oder ein Array in *jsonExpression* verweist. **OPENJSON** sucht im JSON-Text an der angegebenen Position und analysiert nur das referenzierte Fragment. Weitere Informationen finden Sie unter [JSON-Pfadausdrücke &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).

In [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] und [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)] können Sie eine Variable als Wert von *path* bereitstellen.
  
Das folgende Beispiel gibt ein geschachteltes Objekt durch Angabe des *path* zurück:  

```sql  
DECLARE @json NVARCHAR(4000) = N'{  
      "path": {  
            "to":{  
                 "sub-object":["en-GB", "en-UK","de-AT","es-AR","sr-Cyrl"]  
                 }  
              }  
 }';

SELECT [key], value
FROM OPENJSON(@json,'$.path.to."sub-object"')
```  
  
 **Ergebnisse**  
  
|Schlüssel|Wert|  
|---------|-----------|  
|0|en-GB|  
|1|en-UK|  
|2|de-AT|  
|3|es-AR|  
|4|sr-Cyrl|  

Wenn **OPENJSON** ein JSON-Array analysiert, gibt die Funktion die Indizes der Elemente im JSON-Text als Schlüssel zurück.

Beim Vergleich, der zur Zuordnung von Pfadschritten zu den Eigenschaften des JSON-Ausdrucks verwendet wird, wird die Groß-/Kleinschreibung beachtet und die Sortierung nicht (d.h. ein BIN2-Vergleich). 

### <a name="with_clause"></a>*with_clause*

Definiert das Ausgabeschema explizit, das die **OPENJSON**-Funktion zurückgibt. Die optionale *with_clause* kann die folgenden Elemente enthalten:

*colName* ist der Name für die Ausgabespalte.  
  
**OPENJSON** verwendet den Namen der Spalte standardmäßig, um eine Eigenschaft im JSON-Text zuzuordnen. Wenn Sie beispielsweise die Spalte *Name* im Schema angeben, versucht OPENJSON, diese Spalte mit der Eigenschaft „Name“ im JSON-Text zu füllen. Sie können diese Standardzuordnung überschreiben, indem Sie das *column_path*-Argument verwenden.  
  
*type*  
Ist der Datentyp für die Ausgabespalte.  

> [!NOTE]
> Wenn Sie auch die **AS JSON**-Option verwenden, muss die Spalte *Typ*`NVARCHAR(MAX)` sein.
  
*column_path*  
Ist der JSON-Pfad, der die zurückzugebende Eigenschaft in der angegebenen Spalte angibt. Weitere Informationen finden Sie in der Beschreibung der *path*-Parameter weiter oben in diesem Thema.  
  
Verwenden Sie *column_path* zum Überschreiben von Standardzuordnungsregeln, wenn der Name einer Ausgabespalte nicht mit dem Namen der Eigenschaft übereinstimmt.  
  
Beim Vergleich, der zur Zuordnung von Pfadschritten zu den Eigenschaften des JSON-Ausdrucks verwendet wird, wird die Groß-/Kleinschreibung beachtet und die Sortierung nicht (d.h. ein BIN2-Vergleich).  
  
Weitere Informationen zu Pfaden finden Sie unter [JSON-Pfadausdrücke &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
*AS JSON*  
Verwenden Sie die **AS JSON**-Option in einer Spaltendefinition, um anzugeben, dass die Eigenschaft, auf die verwiesen wird, ein inneres JSON-Objekt oder -Array enthält. Bei Angabe der **AS JSON**-Option muss der Typ der Spalte NVARCHAR(MAX) sein.

- Wenn Sie keine **AS JSON**-Option für eine Spalte angeben, gibt die Funktion einen Skalarwert (z.B. int, string, TRUE, FALSE) aus der angegebenen JSON-Eigenschaft auf dem angegebenen Pfad zurück. Wenn der Pfad ein Objekt oder ein Array darstellt, und die Eigenschaft nicht unter dem angegebenen Pfad gefunden werden kann, gibt die Funktion NULL im Lax-Modus oder einen Fehler im Strict-Modus zurück. Dieses Verhalten ist vergleichbar mit dem Verhalten der **JSON_VALUE**-Funktion.  
  
- Wenn Sie eine **AS JSON**-Option für eine Spalte angeben, gibt die Funktion ein JSON-Fragment aus der angegebenen JSON-Eigenschaft auf dem angegebenen Pfad zurück. Wenn der Pfad einen Skalarwert darstellt, und die Eigenschaft nicht unter dem angegebenen Pfad gefunden werden kann, gibt die Funktion NULL im Lax-Modus oder ein Fehler im Strict-Modus zurück. Dieses Verhalten ist vergleichbar mit dem Verhalten der **JSON_QUERY**-Funktion.  
  
> [!NOTE]  
> Wenn Sie ein geschachteltes JSON-Fragment aus einer JSON-Eigenschaft zurückgegeben möchten, müssen Sie die **AS JSON**-Flag angeben. Wenn die Eigenschaft ohne diese Option nicht gefunden werden kann, gibt OPENJSON einen NULL-Wert anstelle des referenzierten JSON-Objekts oder -Arrays oder einen Laufzeitfehler im Strict-Modus zurück.  
  
Die folgende Abfrage gibt beispielsweise die Elemente eines Arrays zurück und formatiert sie:  
  
```sql  
DECLARE @json NVARCHAR(MAX) = N'[  
  {  
    "Order": {  
      "Number":"SO43659",  
      "Date":"2011-05-31T00:00:00"  
    },  
    "AccountNumber":"AW29825",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":1  
    }  
  },  
  {  
    "Order": {  
      "Number":"SO43661",  
      "Date":"2011-06-01T00:00:00"  
    },  
    "AccountNumber":"AW73565",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":3  
    }  
  }
]'  
   
SELECT *
FROM OPENJSON ( @json )  
WITH (   
              Number   VARCHAR(200)   '$.Order.Number',  
              Date     DATETIME       '$.Order.Date',  
              Customer VARCHAR(200)   '$.AccountNumber',  
              Quantity INT            '$.Item.Quantity',  
              [Order]  NVARCHAR(MAX)  AS JSON  
 )
```  
  
**Ergebnisse**
  
|Number|Date|Kunde|Menge|Order|  
|------------|----------|--------------|--------------|-----------|  
|SO43659|2011-05-31T00:00:00|AW29825|1|{"Number":"SO43659","Date":"2011-05-31T00:00:00"}|  
|SO43661|2011-06-01T00:00:00|AW73565|3|{"Number":"SO43661","Date":"2011-06-01T00:00:00"}|  
  
## <a name="return-value"></a>Rückgabewert
Die Spalten, die die OPENJSON-Funktion zurückgibt, hängen von der WITH-Option ab.  
  
1. Wenn Sie OPENJSON mit dem Standardschema aufrufen, und kein explizites Schema in der WITH-Klausel angeben, gibt die Funktion eine Tabelle mit den folgenden Spalten zurück:  
    1.  **Schlüssel**. Ein nvarchar(4000)-Wert, der den Namen der angegebenen Eigenschaft oder den Index des Elements im angegebenen Array enthält. Die Schlüsselspalte verfügt über eine BIN2-Sortierung.  
    2.  **Wert**. Ein nvarchar(max)-Wert, der den Wert der Eigenschaft enthält. Die Wertspalte erbt die Sortierung aus *jsonExpression*.
    3.  **Art:** Ein int-Wert, der den Typ des Werts enthält. Die **Typ**-Spalte wird nur zurückgegeben, wenn Sie OPENJSON mit dem Standardschema verwenden. Die Typspalte besitzt einen der folgenden Werte:  
  
        |Wert der Typspalte|JSON-Datentyp|  
        |------------------------------|--------------------|  
        |0|NULL|  
        |1|Zeichenfolge|  
        |2|Zahl|  
        |3|TRUE/FALSE|  
        |4|array|  
        |5|Objekt (object)|  
  
     Es werden nur Eigenschaften der ersten Ebene zurückgegeben. Die Anweisung schlägt fehl, wenn der JSON-Text nicht ordnungsgemäß formatiert ist.  

2. Wenn Sie OPENJSON aufrufen und ein explizites Schema in der WITH-Klausel angeben, gibt die Funktion eine Tabelle mit dem Schema zurück, das Sie in der WITH-Klausel definiert haben.

> [!NOTE]  
> Die Spalten **Key** (Schlüssel), **Value** (Wert) und **Type** (Typ) werden nur zurückgegeben, wenn Sie OPENJSON mit dem Standardschema verwenden. Sie sind mit einem expliziten Schema nicht verfügbar.

## <a name="remarks"></a>Bemerkungen  

*json_path*, das im zweiten Argument von **OPENJSON** oder in der *with_clause* verwendet wird, kann mit einem **lax**- oder **strict**-Schlüsselwort beginnen.

- Im **Lax**-Modus löst **OPENJSON** keinen Fehler aus, wenn das Objekt oder der Wert für den angegebenen Pfad nicht gefunden werden können. Wenn der Pfad nicht gefunden werden kann, gibt **OPENJSON** ein leeres Resultset oder einen NULL-Wert zurück.
- In **strict**-Modus gibt **OPENJSON** einen Fehler zurück, wenn der Pfad nicht gefunden werden kann.

Einige der Beispiele auf dieser Seite geben den Path-Modus explizit an, Lax oder Strict. Der Path-Modus ist optional. Wenn Sie nicht explizit einen Path-Modus angeben, ist der Lax-Modus die Standardeinstellung. Weitere Informationen zu Path-Modi und -Ausdrücken finden Sie unter [JSON-Pfadausdrücke &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).    

Spaltennamen in *with_clause* werden Schlüsseln im JSON-Text zugeordnet. Wenn Sie den Spaltennamen `[Address.Country]` angeben, wird er dem `Address.Country`-Schlüssel zugeordnet. Wenn Sie auf einen geschachtelten Schlüssel `Country` innerhalb des `Address`-Objekts verweisen möchten, müssen Sie den Pfad `$.Address.Country` in der Pfadspalte angeben.

*json_path* kann Schlüssel mit alphanumerischen Zeichen enthalten. Setzen Sie den Schlüsselnamen in *json_path* in doppelte Anführungszeichen, wenn Sie über Sonderzeichen in den Schlüsseln verfügen. Im folgenden JSON-Text ist `$."my key $1".regularKey."key with . dot"` beispielsweise dem Wert 1 zugeordnet:

```json
{
  "my key $1": {
    "regularKey":{
      "key with . dot": 1
    }
  }
}
```  

## <a name="examples"></a>Beispiele  
  
### <a name="example-1---convert-a-json-array-to-a-temporary-table"></a>Beispiel 1: Konvertieren eines JSON-Arrays in eine temporäre Tabelle

Das folgende Beispiel enthält eine Liste von Bezeichnern als JSON-Array der Zahlen. Die Abfrage konvertiert das JSON-Array in eine Tabelle von Bezeichnern und filtert alle Produkte mit den angegebenen IDs.  
  
```sql  
DECLARE @pSearchOptions NVARCHAR(4000) = N'[1,2,3,4]'

SELECT *
FROM products
INNER JOIN OPENJSON(@pSearchOptions) AS productTypes
 ON product.productTypeID = productTypes.value
```  
  
Diese Abfrage entspricht dem folgenden Beispiel. Im folgenden Beispiel müssen Sie jedoch Zahlen in die Abfrage einbetten, anstatt sie als Parameter zu übergeben.  
  
```sql  
SELECT *
FROM products
WHERE product.productTypeID IN (1,2,3,4)
```  
  
### <a name="example-2---merge-properties-from-two-json-objects"></a>Beispiel 2: Zusammenführen von Eigenschaften aus zwei JSON-Objekten

Das folgende Beispiel wählt eine Vereinigung aller Eigenschaften aus zwei JSON-Objekten aus. Die beiden Objekte verfügen über eine doppelte *name*-Eigenschaft. Im Beispiel wird der Schlüsselwert verwendet, um die doppelte Zeile aus den Ergebnissen auszuschließen.  
  
```sql  
DECLARE @json1 NVARCHAR(MAX),@json2 NVARCHAR(MAX)

SET @json1=N'{"name": "John", "surname":"Doe"}'

SET @json2=N'{"name": "John", "age":45}'

SELECT *
FROM OPENJSON(@json1)
UNION ALL
SELECT *
FROM OPENJSON(@json2)
WHERE [key] NOT IN (SELECT [key] FROM OPENJSON(@json1))
```  
  
### <a name="example-3---join-rows-with-json-data-stored-in-table-cells-using-cross-apply"></a>Beispiel 3: Verknüpfen von Zeilen und JSON-Daten, die in Tabellenzellen gespeichert sind, mithilfe von CROSS APPLY

Im folgenden Beispiel besitzt die `SalesOrderHeader`-Tabelle eine `SalesReason`-Textspalte, die ein `SalesOrderReasons`-Array im JSON-Format enthält. Die `SalesOrderReasons`-Objekte enthalten Eigenschaften wie *Qualität* und *Hersteller*. Das Beispiel erstellt einen Bericht, der jede Zeile der Bestellung und die zugehörigen Verkaufsgründe verknüpft. Der OPENJSON-Operator erweitert das JSON-Array von Verkaufsgründen, als ob die Gründe in einer separaten untergeordneten Tabelle gespeichert wären. Der CROSS APPLY-Operator verknüpft dann jede Verkaufszeile der Bestellung mit den von der OPENJSON-Tabellenwertfunktion zurückgegebenen Zeilen.  
  
```sql  
SELECT SalesOrderID,OrderDate,value AS Reason
FROM Sales.SalesOrderHeader
CROSS APPLY OPENJSON(SalesReasons)
```  
  
> [!TIP] 
> Wenn Sie in den einzelnen Feldern gespeicherte JSON-Arrays erweitern müssen und sie mit ihren übergeordneten Zeilen verknüpfen, verwenden Sie in der Regel den CROSS APPLY-Operator. [!INCLUDE[tsql](../../includes/tsql-md.md)]. Weitere Informationen zu CROSS APPLY finden Sie unter [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md).  
  
Dieselbe Abfrage kann umgeschrieben werden, indem Sie `OPENJSON` mit einem explizit definierten Schema der zurückzugebenden Zeilen verwenden:  
  
```sql  
SELECT SalesOrderID, OrderDate, value AS Reason  
FROM Sales.SalesOrderHeader  
     CROSS APPLY OPENJSON (SalesReasons) WITH (value NVARCHAR(100) '$')
```  
  
In diesem Beispiel verweist der `$`-Pfad auf jedes Element im Array. Wenn der zurückgegebene Wert explizit umgewandelt werden soll, können Sie diese Art der Abfrage verwenden.  
  
### <a name="example-4---combine-relational-rows-and-json-elements-with-cross-apply"></a>Beispiel 4: Kombinieren der relationalen Zeilen und JSON-Elemente mit CROSS APPLY

Die folgende Abfrage kombiniert relationale Zeilen und JSON-Elemente zu den in der folgenden Tabelle dargestellten Ergebnissen.  
  
```sql  
SELECT store.title, location.street, location.lat, location.long  
FROM store  
CROSS APPLY OPENJSON(store.jsonCol, 'lax $.location')   
     WITH (street VARCHAR(500) ,  postcode VARCHAR(500) '$.postcode' ,  
     lon int '$.geo.longitude', lat int '$.geo.latitude')  
     AS location
```  
  
**Ergebnisse**
  
|title|street|postcode|lon|lat|  
|-----------|------------|--------------|---------|---------|  
|Whole Food Markets|17991 Redmond Way|WA 98052|47.666124|-122.10155|  
|Sears|148th Ave NE|WA 98052|47.63024|-122.141246,17|  
  
### <a name="example-5---import-json-data-into-sql-server"></a>Beispiel 5: Importieren von JSON-Daten in SQL Server

Im folgenden Beispiel wird ein komplettes JSON-Objekt in eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Tabelle geladen.  
  
```sql  
DECLARE @json NVARCHAR(max)  = N'{  
  "id" : 2,  
  "firstName": "John",  
  "lastName": "Smith",  
  "isAlive": true,  
  "age": 25,  
  "dateOfBirth": "2015-03-25T12:00:00",  
  "spouse": null  
  }';  
   
  INSERT INTO Person  
  SELECT *   
  FROM OPENJSON(@json)  
  WITH (id INT,  
        firstName NVARCHAR(50), lastName NVARCHAR(50),   
        isAlive BIT, age INT,  
        dateOfBirth DATETIME, spouse NVARCHAR(50))
```  

### <a name="example-6---simple-example-with-json-content"></a>Beispiel 6: Einfaches Beispiel mit JSON-Inhalten

```sql
--simple cross apply example
DECLARE @JSON NVARCHAR(MAX) = N'[
{
"OrderNumber":"SO43659",
"OrderDate":"2011-05-31T00:00:00",
"AccountNumber":"AW29825",
"ItemPrice":2024.9940,
"ItemQuantity":1
},
{
"OrderNumber":"SO43661",
"OrderDate":"2011-06-01T00:00:00",
"AccountNumber":"AW73565",
"ItemPrice":2024.9940,
"ItemQuantity":3
}
]'

SELECT root.[key] AS [Order],TheValues.[key], TheValues.[value]
FROM OPENJSON ( @JSON ) AS root
CROSS APPLY OPENJSON ( root.value) AS TheValues
```

## <a name="see-also"></a>Weitere Informationen

- [JSON Path Expressions &#40;SQL Server&#41; (JSON-Pfadausdrücke [SQL Server])](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [Convert JSON Data to Rows and Columns with OPENJSON &#40;SQL Server&#41; (Konvertieren von JSON-Daten in Zeilen und Spalten mit OPENJSON [SQL Server])](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md)   
- [Verwenden von OPENJSON mit dem Standardschema &#40;SQL Server&#41;](../../relational-databases/json/use-openjson-with-the-default-schema-sql-server.md)   
- [Verwenden von OPENJSON mit einem expliziten Schema &#40;SQL Server&#41;](../../relational-databases/json/use-openjson-with-an-explicit-schema-sql-server.md)  
  
