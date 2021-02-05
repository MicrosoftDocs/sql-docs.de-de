---
title: PDO::quote
description: API-Referenz für die PDO::quote-Funktion im Microsoft PDO_SQLSRV-Treiber für PHP für SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ab9ddc48-42f8-4edf-aa8b-b0fc66706161
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9b9e74b05b48c4e3c225ec37a07408d2ca5c889b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206374"
---
# <a name="pdoquote"></a>PDO::quote
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Verarbeitet eine Zeichenfolge für die Verwendung in einer Abfrage, indem die Eingabezeichenfolge gemäß der zugrunde liegenden SQL Server-Datenbank in Anführungszeichen gesetzt wird. PDO::quote umgeht Sonderzeichen in der Eingabezeichenfolge unter Verwendung eines für den SQL Server geeigneten Zitierstils.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
string PDO::quote( $string[, $parameter_type ] )  
```  
  
#### <a name="parameters"></a>Parameter  
$*string*: Die zu zitierende Zeichenfolge  
  
$*parameter_type*: Ein optionales Symbol (ganze Zahl), das den Datentyp angibt  Der Standardwert ist PDO::PARAM_STR.  

In PHP 7.2 wurden neue PDO-Konstanten eingeführt, für die Unterstützung der [Bindung von Unicode- und Nicht-Unicode-Zeichenfolgen](https://wiki.php.net/rfc/extended-string-types-for-pdo). Unicode-Zeichenfolgen können in Anführungszeichen stehen, mit einem „N“ als Präfix, d. h. N'string' anstatt 'string'.

1. PDO::PARAM_STR_NATL: Ein neuer Typ für Unicode-Zeichenfolgen. Dieser wird für bitweise-OR-Operationen gegen PDO::PARAM_STR verwendet.
1. PDO::PARAM_STR_CHAR: Ein neuer Typ für Nicht-Unicode-Zeichenfolgen. Dieser wird für bitweise-OR-Operationen gegen PDO::PARAM_STR verwendet.
1. PDO::ATTR_DEFAULT_STR_PARAM: festgelegt auf entweder PDO::PARAM_STR_NATL oder PDO::PARAM_STR_CHAR, um anzugeben, welcher Wert standardmäßig für bitweise-OR-Operationen gegen PDO:PARAM_STR verwendet werden soll.

Ab Version 5.8.0 stehen Ihnen für PDO::quote folgende Konstanten zur Verfügung.
  
## <a name="return-value"></a>Rückgabewert  
Eine Zeichenfolge in Anführungszeichen, die an eine SQL-Anweisung oder bei einem Fehler an false übergeben werden kann.  
  
## <a name="remarks"></a>Bemerkungen  
Unterstützung für PDO wurde in Version 2.0 von [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]hinzugefügt.  
  
## <a name="string-escape-example"></a>Beispiel für ein Escape von Zeichenfolgen  
  
```  
<?php  
$database = "test";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$param = 'a \' g';  
$param2 = $conn->quote( $param );  
  
$query = "INSERT INTO Table1 VALUES( ?, '1' )";  
$stmt = $conn->prepare( $query );  
$stmt->execute(array($param));  
  
$query = "INSERT INTO Table1 VALUES( ?, ? )";  
$stmt = $conn->prepare( $query );  
$stmt->execute(array($param, $param2));  
?>  
```  
  
## <a name="pdo-quote-example"></a>PDO-Angebotsbeispiel  

Das folgende Skript enthält einige Beispiele für die Auswirkung erweiterter Zeichenfolgetypen auf PDO::quote() in PHP 7.2 und höher.

```
<?php
$database = "test";
$server = "(local)";
$db = new PDO("sqlsrv:server=$server; Database=$database", "", "");

$db->quote('über', PDO::PARAM_STR | PDO::PARAM_STR_NATL); // N'über'
$db->quote('foo'); // 'foo'

$db->setAttribute(PDO::ATTR_DEFAULT_STR_PARAM, PDO::PARAM_STR_NATL);
$db->quote('über'); // N'über'
$db->quote('foo', PDO::PARAM_STR | PDO::PARAM_STR_CHAR); // 'foo'
?>
```
  
## <a name="see-also"></a>Weitere Informationen  
[PDO-Klasse](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
