---
description: sqlsrv_field_metadata
title: sqlsrv_field_metadata | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_field_metadata
apitype: NA
helpviewer_keywords:
- API Reference, sqlsrv_field_metadata
- sqlsrv_field_metadata
ms.assetid: c02f6942-0484-4567-a78e-fe8aa2053536
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 82e1ad1147867abd9308dc89517855df830a38e6
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837153"
---
# <a name="sqlsrv_field_metadata"></a>sqlsrv_field_metadata
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Ruft Metadaten für die Felder einer vorbereiteten Anweisung ab. Informationen zum Vorbereiten einer Anweisung finden Sie unter [sqlsrv_query](../../connect/php/sqlsrv-query.md) oder [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md). Beachten Sie, dass **sqlsrv_field_metadata** sowohl vor als auch nach der Ausführung jeder Anweisung aufgerufen werden kann.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sqlsrv_field_metadata( resource $stmt)  
```  
  
#### <a name="parameters"></a>Parameter  
*$stmt*: Eine Anweisungsressource für die Feldmetadaten gewünscht werden.  
  
## <a name="return-value"></a>Rückgabewert  
Ein **Array** von Arrays oder **false**. Das Array besteht aus einem Array für jedes Feld im Resultset. Jedes Teilarray hat Schlüssel, wie in der unten stehenden Tabelle beschrieben. Falls beim Abrufen der Feldmetadaten ein Fehler auftritt, wird **false** zurückgegeben.  
  
|Key|BESCHREIBUNG|  
|-------|---------------|  
|Name|Name der Spalte, der das Feld entspricht|  
|Typ|Numerischer Wert, der einem SQL-Typ entspricht|  
|Size|Anzahl der Zeichen für die Felder eines bestimmten Zeichentyps (char(n), varchar(n), nchar(n), nvarchar(n), XML) Anzahl der Bytes für Felder vom Typ „binär“ (binary(n), varbinary(n), UDT). **NULL** für andere SQL Server-Datentypen.|  
|Genauigkeit|Die Genauigkeit für die Typen mit variabler Genauigkeit (real, numeric, decimal, datetime2, datetimeoffset und time). **NULL** für andere SQL Server-Datentypen.|  
|Skalieren|Die Skalierung für die Typen mit variabler Skalierung (numeric, decimal, datetime2, datetimeoffset und time). **NULL** für andere SQL Server-Datentypen.|  
|Nullable|Ein Aufzählungswert, der anzeigt, ob die Spalte NULL-Werte zulässt (**SQLSRV_NULLABLE_YES**) oder nicht (**SQLSRV_NULLABLE_NO**), oder ob diese Eigenschaft nicht bekannt ist (**SQLSRV_NULLABLE_UNKNOWN**).|  
  
Die folgende Tabelle enthält mehr Informationen zu den Schlüsseln für jedes Teilarray (weiter Informationen zu diesen Typen finden Sie in der Dokumentation zu SQL Server):   
  
|SQL Server 2008-Datentyp|Typ|Min/Max Genauigkeit|Min/Max Skalierung|Size|  
|-----------------------------|--------|----------------------|------------------|--------|  
|BIGINT|SQL_BIGINT (-5)|||8|  
|BINARY|SQL_BINARY (-2)|||0 < *n* < 8000 <sup>1</sup>|  
|bit|SQL_BIT (-7)||||  
|char|SQL_CHAR (1)|||0 < *n* < 8000 <sup>1</sup>|  
|date|SQL_TYPE_DATE (91)|10/10|0/0||  
|datetime|SQL_TYPE_TIMESTAMP (93)|23/23|3/3||  
|datetime2|SQL_TYPE_TIMESTAMP (93)|19/27|0/7||  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET (-155)|26/34|0/7||  
|Decimal|SQL_DECIMAL (3)|1/38|0/Genauigkeitswert||  
|float|SQL_FLOAT (6)|4/8|||  
|image|SQL_LONGVARBINARY (-4)|||2 GB|  
|INT|SQL_INTEGER (4)||||  
|money|SQL_DECIMAL (3)|19/19|4/4||  
|NCHAR|SQL_WCHAR (-8)|||0 < *n* < 4000 <sup>1</sup>|  
|ntext|SQL_WLONGVARCHAR (-10)|||1 GB|  
|NUMERIC|SQL_NUMERIC (2)|1/38|0/Genauigkeitswert||  
|NVARCHAR|SQL_WVARCHAR (-9)|||0 < *n* < 4000 <sup>1</sup>|  
|real|SQL_REAL (7)|4/4|||  
|smalldatetime|SQL_TYPE_TIMESTAMP (93)|16/16|0/0||  
|SMALLINT|SQL_SMALLINT (5)|||2 Bytes|  
|Smallmoney|SQL_DECIMAL (3)|10/10|4/4||  
|sql_variant|SQL_SS_VARIANT (-150)|||-Variable|  
|Text|SQL_LONGVARCHAR (-1)|||2 GB|  
|time|SQL_SS_TIME2 (-154)|8/16|0/7||  
|timestamp|SQL_BINARY (-2)|||8 Bytes|  
|TINYINT|SQL_TINYINT (-6)|||1 Byte|  
|udt|SQL_SS_UDT (-151)|||-Variable|  
|UNIQUEIDENTIFIER|SQL_GUID (-11)|||16|  
|varbinary|SQL_VARBINARY (-3)|||0 < *n* < 8000 <sup>1</sup>|  
|varchar|SQL_VARCHAR (12)|||0 < *n* < 8000 <sup>1</sup>|  
|Xml|SQL_SS_XML (-152)|||0|  
  
(1) Die Null (0) gibt an, das die Maximalgröße zulässig ist.  
  
Ein Schlüssel der NULL-Werte zulässt, kann entweder „Ja“ oder „Nein“ sein.  
  
## <a name="example"></a>Beispiel  
Das folgende Beispiel erstellt eine Anweisungsressource, ruft  die Feldmetadaten ab und stellt sie dar. Das Beispiel setzt voraus, dass SQL Server und die [AdventureWorks-Datenbank](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) auf dem lokalen Computer installiert sind. Wenn das Beispiel über die Befehlszeile ausgeführt wird, werden alle Ausgaben in die Konsole geschrieben.  
  
```
<?php
/* Connect to the local server using Windows Authentication and
specify the AdventureWorks database as the database in use. */
$serverName = "(local)";
$connectionInfo = array("Database"=>"AdventureWorks");
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
    echo "Could not connect.\n";
    die( print_r( sqlsrv_errors(), true));
}

/* Prepare the statement. */
$tsql = "SELECT ReviewerName, Comments FROM Production.ProductReview";
$stmt = sqlsrv_prepare( $conn, $tsql);
  
/* Get and display field metadata. */
foreach( sqlsrv_field_metadata( $stmt) as $fieldMetadata) {
    foreach( $fieldMetadata as $name => $value) {
        echo "$name: $value\n";
    }  
    echo "\n";
}  
  
/* Note: sqlsrv_field_metadata can be called on any statement
resource, pre- or post-execution. */
  
/* Free statement and connection resources. */
sqlsrv_free_stmt($stmt);
sqlsrv_close($conn);
?>
```

## <a name="sensitivity-data-classification-metadata"></a>Klassifizierungsmetadaten zu Vertraulichkeitsdaten

Ab Version 5.8.0 ist die neue Option `DataClassification` verfügbar, damit Benutzer in Microsoft SQL Server 2019 mithilfe von `sqlsrv_field_metadata` auf die [Klassifizierungsmetadaten zu Vertraulichkeitsdaten](../../relational-databases/security/sql-data-discovery-and-classification.md) zugreifen können. Hierfür wird der Microsoft ODBC Driver 17.4.2 oder höher benötigt. 

Die Option `DataClassification` entspricht standardmäßig `false`. Wenn sie jedoch auf `true` festgelegt wird, wird das von `sqlsrv_field_metadata` zurückgegebene Array mit den Klassifizierungsmetadaten zu Vertraulichkeitsdaten aufgefüllt, falls vorhanden. 

Hier als Beispiel eine Patiententabelle:

```
CREATE TABLE Patients 
      [PatientId] int identity,
      [SSN] char(11),
      [FirstName] nvarchar(50),
      [LastName] nvarchar(50),
      [BirthDate] date)
```

Die Spalten „SSN“ und „BirthDate“ können wie unten dargestellt klassifiziert werden:

```
ADD SENSITIVITY CLASSIFICATION TO [Patients].SSN WITH (LABEL = 'Highly Confidential - secure privacy', INFORMATION_TYPE = 'Credentials')
ADD SENSITIVITY CLASSIFICATION TO [Patients].BirthDate WITH (LABEL = 'Confidential Personal Data', INFORMATION_TYPE = 'Birthdays')
```

Rufen Sie `sqlsrv_field_metadata` wie im folgenden Codeausschnitt gezeigt auf, um auf die Metadaten zuzugreifen:

```
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = sqlsrv_prepare($conn, $tsql, array(), array('DataClassification' => true));
if (sqlsrv_execute($stmt)) {
    $fieldmeta = sqlsrv_field_metadata($stmt);

    foreach ($fieldmeta as $f) {
        if (count($f['Data Classification']) > 0) {
            echo $f['Name'] . ": \n";
            print_r($f['Data Classification']); 
        }
    }
}
```

Ausgabe:

```
SSN: 
Array
(
    [0] => Array
        (
            [Label] => Array
                (
                    [name] => Highly Confidential - secure privacy
                    [id] => 
                )

            [Information Type] => Array
                (
                    [name] => Credentials
                    [id] => 
                )

        )

)
BirthDate: 
Array
(
    [0] => Array
        (
            [Label] => Array
                (
                    [name] => Confidential Personal Data
                    [id] => 
                )

            [Information Type] => Array
                (
                    [name] => Birthdays
                    [id] => 
                )

        )

)
```

Wenn Sie `sqlsrv_query` anstelle von `sqlsrv_prepare` verwenden, können Sie den obigen Codeausschnitt wie folgt anpassen:

```
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = sqlsrv_query($conn, $tsql, array(), array('DataClassification' => true));
$fieldmeta = sqlsrv_field_metadata($stmt);

foreach ($fieldmeta as $f) {
    $jstr = json_encode($f);
    echo $jstr . PHP_EOL;
}
```

Wie in der folgenden JSON-Darstellung veranschaulicht, werden die gezeigten Datenklassifizierungsmetadaten den Spalten zugeordnet:

```
{"Name":"PatientId","Type":4,"Size":null,"Precision":10,"Scale":null,"Nullable":0,"Data Classification":[]}
{"Name":"SSN","Type":1,"Size":11,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""}}]}
{"Name":"FirstName","Type":-9,"Size":50,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[]}
{"Name":"LastName","Type":-9,"Size":50,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[]}
{"Name":"BirthDate","Type":91,"Size":null,"Precision":10,"Scale":0,"Nullable":1,"Data Classification":[{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""}}]}
```

## <a name="sensitivity-rank-using-a-predefined-set-of-values"></a>Vertraulichkeitsbewertung mithilfe vordefinierter Werte

Ab 5.9.0 können PHP-Treiber bei Verwendung von ODBC-Treibern mit Version 17.4.2 und höher Klassifizierungsbewertungen abrufen. Mithilfe von [ADD SENSITIVITY CLASSIFICATION](../../t-sql/statements/add-sensitivity-classification-transact-sql.md) können Benutzer eine Bewertung definieren, um Datenspalten zu klassifizieren. 

Wenn ein Benutzer beispielsweise `NONE` und `LOW` jeweils BirthDate und SSN zuweist, sieht die JSON-Darstellung folgendermaßen aus:

```
{"0":{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""},"rank":0},"rank":0}
{"0":{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""},"rank":10},"rank":10}
```

Wie bei der [Vertraulichkeitsklassifizierung](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md) ersichtlich wird, lauten die numerischen Werte für die Bewertungen wie folgt:

```
0 for NONE
10 for LOW
20 for MEDIUM
30 for HIGH
40 for CRITICAL
```

Deshalb lauten die Klassifizierungsmetadaten folgendermaßen, wenn der Benutzer `RANK=CRITICAL` anstelle von `RANK=NONE` beim Klassifizieren der Spalte BirthDate definiert:

```
  array(7) {
    ["Name"]=>
    string(9) "BirthDate"
    ["Type"]=>
    int(91)
    ["Size"]=>
    NULL
    ["Precision"]=>
    int(10)
    ["Scale"]=>
    int(0)
    ["Nullable"]=>
    int(1)
    ["Data Classification"]=>
    array(2) {
      [0]=>
      array(3) {
        ["Label"]=>
        array(2) {
          ["name"]=>
          string(26) "Confidential Personal Data"
          ["id"]=>
          string(0) ""
        }
        ["Information Type"]=>
        array(2) {
          ["name"]=>
          string(9) "Birthdays"
          ["id"]=>
          string(0) ""
        }
        ["rank"]=>
        int(40)
      }
      ["rank"]=>
      int(40)
    }
  }
```

Unten sehen Sie die aktualisierte JSON-Darstellung:

```
{"0":{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""},"rank":40},"rank":40}
```

## <a name="see-also"></a>Weitere Informationen  
[API-Referenz für den SQLSRV-Treiber](../../connect/php/sqlsrv-driver-api-reference.md)  

[Konstanten &#40;Microsoft-Treiber für PHP für SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)  

[Informationen zu den Codebeispielen in der Dokumentation](../../connect/php/about-code-examples-in-the-documentation.md)