---
title: PDOStatement::getColumnMeta
description: API-Referenz für die PDOStatement::getColumnMeta-Funktion im Microsoft PDO_SQLSRV-Treiber für PHP für SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: c92a21cc-8e53-43d0-a4bf-542c77c100c9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b76e7c6201226c13ae057e8ac182b7ab0a9c6b13
ms.sourcegitcommit: 7eb80038c86acfef1d8e7bfd5f4e30e94aed3a75
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92082019"
---
# <a name="pdostatementgetcolumnmeta"></a>PDOStatement::getColumnMeta
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Ruft die Metadaten für eine Spalte ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
array PDOStatement::getColumnMeta ( $column );  
```  
  
#### <a name="parameters"></a>Parameter  
*$conn* (ganze Zahl): Die nullbasierte Nummer der Spalte, deren Metadaten Sie abrufen möchten.  
  
## <a name="return-value"></a>Rückgabewert  
Ein assoziatives Array (Schlüssel und Wert), das die Metadaten für die Spalte enthält. Im Abschnitt „Anmerkungen“ finden Sie eine Beschreibung für die Felder im Array.  
  
## <a name="remarks"></a>Hinweise  
Die folgende Tabelle beschreibt die Felder im durch „getColumnMeta“ zurückgegebenen Array.  
  
|NAME|WERTE|  
|--------|----------|  
|native_type|Gibt den PHP-Typ der Spalte an Immer Zeichenfolge.|  
|driver:decl_type|Gibt den SQL-Typ an, der verwendet wird, um den Spaltenwert in der Datenbank darzustellen Falls die Spalte im Resultset das Ergebnis einer Funktion ist, wird dieser Wert nicht von PDOStatement::getColumnMeta zurückgegeben.|  
|flags|Gibt die Flags (Kennzeichnungen), die für diese Spalte eingerichtet wurden, an Immer 0.|  
|name|Gibt den Namen der Spalte in der Datenbank an|  
|table|Gibt den Namen derjenigen Tabelle in der Datenbank an, welche die Spalte enthält Immer leer.|  
|Länge|Gibt die Länge der Spalte an|  
|precision (Genauigkeit)|Gibt die numerische Genauigkeit dieser Spalte an|  
|pdo_type|Gibt den Typ dieser Spalte an, wie durch die PDO::PARAM_* Konstanten widergespiegelt. Immer PDO::PARAM_STR (2).|  
  
Unterstützung für PDO wurde in Version 2.0 von [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]hinzugefügt.  
  
## <a name="example"></a>Beispiel  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$stmt = $conn->query("select * from Person.ContactType");  
$metadata = $stmt->getColumnMeta(2);  
var_dump($metadata);  
  
print $metadata['sqlsrv:decl_type'] . "\n";  
print $metadata['native_type'] . "\n";  
print $metadata['name'];  
?>  
```  
  
## <a name="sensitivity-data-classification-metadata"></a>Klassifizierungsmetadaten zu Vertraulichkeitsdaten

Ab Version 5.8.0 ist ein neues Anweisungsattribut `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` verfügbar, damit Benutzer in Microsoft SQL Server 2019 mithilfe von `PDOStatement::getColumnMeta` auf die [Klassifizierungsmetadaten zu Vertraulichkeitsdaten](../../relational-databases/security/sql-data-discovery-and-classification.md?tabs=t-sql#subheading-4) zugreifen können. Hierfür wird der Microsoft ODBC Driver 17.4.2 oder höher benötigt.

Beachten Sie, dass das Attribut `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` standardmäßig auf `false` festgelegt ist. Wird es jedoch auf `true` festgelegt, wird das zuvor erwähnte Arrayfeld (`flags`) mit den Klassifizierungsmetadaten zu Vertraulichkeitsdaten befüllt, falls vorhanden. 

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

Wenn Sie auf die Metadaten zugreifen möchten, verwenden Sie wie im folgenden Codeausschnitt gezeigt `PDOStatement::getColumnMeta`, nachdem Sie `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` auf TRUE festgelegt haben:

```
$options = array(PDO::SQLSRV_ATTR_DATA_CLASSIFICATION => true);
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = $conn->prepare($tsql, $options);
$stmt->execute();
$numCol = $stmt->columnCount();

for ($i = 0; $i < $numCol; $i++) {
    $metadata = $stmt->getColumnMeta($i);
    $jstr = json_encode($metadata);
    echo $jstr . PHP_EOL;
}
```

Die Ausgabe von Metadaten für alle Spalten sieht folgendermaßen aus:

```
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"int identity","native_type":"string","table":"","pdo_type":2,"name":"PatientId","len":10,"precision":0}
{"flags":{"Data Classification":[{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""}}]},"sqlsrv:decl_type":"char","native_type":"string","table":"","pdo_type":2,"name":"SSN","len":11,"precision":0}
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"FirstName","len":50,"precision":0}
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"LastName","len":50,"precision":0}
{"flags":{"Data Classification":[{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""}}]},"sqlsrv:decl_type":"date","native_type":"string","table":"","pdo_type":2,"name":"BirthDate","len":10,"precision":0}
```

Wenn Sie den obigen Codeausschnitt ändern, indem Sie `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` auf `false` festlegen (Standard), wird wie folgt dargestellt für das Feld `flags` wie zuvor immer der Wert `0` verwendet:

```
{"flags":0,"sqlsrv:decl_type":"int identity","native_type":"string","table":"","pdo_type":2,"name":"PatientId","len":10,"precision":0}
{"flags":0,"sqlsrv:decl_type":"char","native_type":"string","table":"","pdo_type":2,"name":"SSN","len":11,"precision":0}
{"flags":0,"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"FirstName","len":50,"precision":0}
{"flags":0,"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"LastName","len":50,"precision":0}
{"flags":0,"sqlsrv:decl_type":"date","native_type":"string","table":"","pdo_type":2,"name":"BirthDate","len":10,"precision":0}
```

      
## <a name="see-also"></a>Weitere Informationen  
[PDOStatement-Klasse](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
