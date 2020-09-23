---
description: sql_variant (Transact-SQL)
title: sql_variant (Transact-SQL)
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- sql_variant
- sql_variant_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sql_variant comparisons
- storing data [SQL Server], sql_variant
- sql_variant data type
- storage [SQL Server], sql_variant
ms.assetid: 01229779-8bc1-4c7d-890a-8246d4899250
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: d6d5bac616d1c83cda53a055b00951cced2de19f
ms.sourcegitcommit: cc23d8646041336d119b74bf239a6ac305ff3d31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2020
ms.locfileid: "91111216"
---
# <a name="sql_variant-transact-sql"></a>sql_variant (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Ein Datentyp, der Werte verschiedener von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützter Datentypen speichert.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
sql_variant  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Bemerkungen  
**sql_variant** kann in Spalten, Parametern, Variablen und Rückgabewerten von benutzerdefinierten Funktionen verwendet werden. **sql_variant** ermöglicht, dass diese Datenbankobjekte auch Werte mit anderen Datentypen unterstützen.
  
Eine Spalte vom Datentyp **sql_variant** enthält möglicherweise Zeilen verschiedener Datentypen. Beispielsweise können in einer als **sql_variant** definierten Spalte die Werte **int**, **binary** und **char** gespeichert sein.
  
**sql_variant** kann eine maximale Länge von 8016 Bytes besitzen. Dies schließt sowohl die Basistypinformationen als auch den Basistypwert ein. Die maximale Länge des Basistypwerts ist 8.000 Byte.
  
Ein **sql_variant**-Datentyp muss zuerst in den Wert seines Basisdatentyps umgewandelt werden, bevor er in Operationen, beispielsweise Addition oder Subtraktion, verwendet werden kann.
  
**sql_variant** kann ein Standardwert zugewiesen werden. Dieser Datentyp kann auch NULL als zugrunde liegenden Wert haben, den NULL-Werten ist jedoch kein Basistyp zugeordnet. Außerdem kann **sql_variant** nicht der Basistyp für einen andren **sql_variant**-Datentyp sein.
  
Eindeutige Schlüssel sowie Primär- oder Fremdschlüssel können Spalten vom Datentyp **sql_variant** enthalten. Die Gesamtlänge der Datenwerte, aus denen ein Schlüssel für eine vorhandene Zeile besteht, sollte jedoch die maximale Länge eines Index nicht überschreiten. Diese beträgt 900 Bytes.
  
Eine Tabelle kann über eine beliebige Anzahl von **sql_variant**-Spalten verfügen.
  
**sql_variant** kann nicht in CONTAINSTABLE und FREETEXTTABLE verwendet werden.
  
ODBC unterstützt **sql_variant** nicht vollständig. Deshalb werden Abfragen von **sql_variant**-Spalten bei Verwendung von Microsoft OLE DB-Anbietern für ODBC (MSDASQL) als Binärdaten zurückgegeben. Eine **sql_variant**-Spalte mit den Zeichenfolgendaten „PS2091“ wird als 0x505332303931 zurückgegeben.
  
## <a name="comparing-sql_variant-values"></a>Vergleichen von sql_variant-Werten  
Der **sql_variant**-Datentyp steht im oberen Bereich in der Hierarchieliste der Datentypen für die Konvertierung. Für **sql_variant**-Vergleiche wird die Reihenfolge der Datentyphierarchie von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Datentypfamilien unterteilt.
  
|Datentyphierarchie|Datentypfamilie|  
|---|---|
|**sql_variant**|sql_variant |  
|**datetime2**|Datum und Uhrzeit|  
|**datetimeoffset**|Datum und Uhrzeit|  
|**datetime**|Datum und Uhrzeit|  
|**smalldatetime**|Datum und Uhrzeit|  
|**date**|Datum und Uhrzeit|  
|**time**|Datum und Uhrzeit|  
|**float**|Ungefährer numerischer Wert|  
|**real**|Ungefährer numerischer Wert|  
|**decimal**|Genauer numerischer Wert|  
|**money**|Genauer numerischer Wert|  
|**smallmoney**|Genauer numerischer Wert|  
|**bigint**|Genauer numerischer Wert|  
|**int**|Genauer numerischer Wert|  
|**smallint**|Genauer numerischer Wert|  
|**tinyint**|Genauer numerischer Wert|  
|**bit**|Genauer numerischer Wert|  
|**nvarchar**|Unicode|  
|**nchar**|Unicode|  
|**varchar**|Unicode|  
|**char**|Unicode|  
|**varbinary**|Binary|  
|**binary**|Binary|  
|**uniqueidentifier**|Uniqueidentifier |  
  
Für **sql_variant**-Vergleiche gelten die folgenden Regeln:
-   Wenn **sql_variant**-Werte unterschiedlicher Basisdatentypen verglichen werden und die Basisdatentypen verschiedenen Datentypfamilien angehören, wird der Wert als der höhere eingestuft, dessen Datentypfamilie sich weiter oben in der Hierarchieliste befindet.  
-   Wenn **sql_variant**-Werte unterschiedlicher Basisdatentypen verglichen werden und die Basisdatentypen derselben Datentypfamilie angehören, wird der Wert, dessen Basisdatentyp sich weiter unten in der Hierarchieliste befindet, implizit in den anderen Datentyp konvertiert, und dann wird der Vergleich durchgeführt.  
-   Wenn **sql_variant**-Werte der Datentypen **char**, **varchar**, **nchar** oder **nvarchar** verglichen werden, stützt sich der Vergleich zunächst auf folgende Kriterien: LCID, LCID-Version, Vergleichsflags und Sortier-ID. Diese Kriterien werden als ganzzahlige Werte und in der genannten Reihenfolge verglichen. Sind alle diese Kriterien gleich, werden die tatsächlichen Zeichenfolgenwerte entsprechend der Sortierreihenfolge verglichen.  
  
## <a name="converting-sql_variant-data"></a>Konvertieren von sql_variant-Daten  
Bei der Verarbeitung von **sql_variant**-Datentypen unterstützt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die impliziten Konvertierungen von Objekten mit anderen Datentypen in den Typ **sql_variant**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt jedoch keine impliziten Konvertierungen von **sql_variant**-Daten in ein Objekt mit einem anderen Datentyp.
  
## <a name="restrictions"></a>Beschränkungen

Im Folgenden werden die Typen von Werten aufgelistet, die nicht mithilfe von **sql_variant** gespeichert werden können:

- **datetimeoffset**<sup>1</sup>
- **geography**
- **geometry**
- **hierarchyid**
- **image**
- **ntext**
- **nvarchar(max)**
- **rowversion** (**timestamp**)
- **text**
- **varchar(max)**
- **varbinary(max)**
- **sql_variant**
- Benutzerdefinierte Typen
- **xml**

<sup>1</sup> Bei SQL Server 2012 und höher ist **datetimeoffset** nicht eingeschränkt.

## <a name="examples"></a>Beispiele  

### <a name="a-using-a-sql_variant-in-a-table"></a>A. Verwenden von „sql_variant“ in einer Tabelle  
 Im folgenden Beispiel wird eine Tabelle mit einem sql_variant-Datentyp erstellt. Dann wird im Beispiel `SQL_VARIANT_PROPERTY`-Informationen über den `colA`-Wert `46279.1` abgerufen, wobei `colB` =`1689` und `tableA` über `colA` vom Typ `sql_variant` und `colB` verfügt.  
  
```sql    
CREATE TABLE tableA(colA sql_variant, colB INT)  
INSERT INTO tableA values ( CAST(46279.1 as decimal(8,2)), 1689)  
SELECT   SQL_VARIANT_PROPERTY(colA,'BaseType') AS 'Base Type',  
         SQL_VARIANT_PROPERTY(colA,'Precision') AS 'Precision',  
         SQL_VARIANT_PROPERTY(colA,'Scale') AS 'Scale'  
FROM      tableA  
WHERE     colB = 1689  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]Beachten Sie, dass jeder dieser drei Werte vom Datentyp **sql_variant** ist.  
  
```  
Base Type    Precision    Scale  
---------    ---------    -----  
decimal      8           2  
  
(1 row(s) affected)  
```  
  
### <a name="b-using-a-sql_variant-as-a-variable"></a>B. Verwenden von „sql_variant“ als Variable   
 Das folgende Beispiel erstellt eine Variable unter Verwendung des sql_variant-Datentyps und ruft dann `SQL_VARIANT_PROPERTY`-Informationen zu einer Variable mit dem Namen @v1 ab.  
  
```sql    
DECLARE @v1 sql_variant;  
SET @v1 = 'ABC';  
SELECT @v1;  
SELECT SQL_VARIANT_PROPERTY(@v1, 'BaseType');  
SELECT SQL_VARIANT_PROPERTY(@v1, 'MaxLength');  
```    


## <a name="see-also"></a>Weitere Informationen
[CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[SQL_VARIANT_PROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sql-variant-property-transact-sql.md)
  
  
