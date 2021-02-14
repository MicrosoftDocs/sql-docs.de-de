---
title: Abrufen von Datums- und Uhrzeittypen als Zeichenfolgen mit dem SQLSRV-Treiber
description: Erfahren Sie, wie Sie mit dem SQLSRV-Treiber für PHP für SQL Server Datum- und Uhrzeittypen als Zeichenfolgen abrufen können.
ms.custom: ''
ms.date: 02/11/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- date and time types, retrieving as strings
ms.assetid: 58a974ea-4daf-4e3b-98ed-9731b9c9250f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 23366d72375c530facb79f343e2622c9955263b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045310"
---
# <a name="how-to-retrieve-date-and-time-types-as-strings-using-the-sqlsrv-driver"></a>Gewusst wie: Abrufen von Datums- und Uhrzeittypen als Zeichenfolgen mit dem SQLSRV-Treiber
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Wenn Sie den SQLSRV-Treiber für die [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] verwenden, können Sie Datums- und Uhrzeittypen (**smalldatetime**, **datetime**, **date**, **time**, **datetime2** und **datetimeoffset**) als Zeichenfolgen abrufen, indem Sie in der Verbindungszeichenfolge oder auf Anweisungsebene die folgenden Optionen festlegen:

```
'ReturnDatesAsStrings'=>true
```

Der Standardwert ist **false**, was bedeutet, dass die Typen **smalldatetime**, **datetime**, **date**, **time**, **datetime2** und **datetimeoffset** als [PHP DateTime](http://php.net/manual/en/class.datetime.php)-Objekte zurückgegeben werden. Wird diese Option auf Anweisungsebene festgelegt, werden die Einstellungen auf Verbindungsebene überschrieben.

Der PDO_SQLSRV-Treiber gibt Datums-und Uhrzeittypen standardmäßig als Zeichenfolgen zurück. Wenn Sie möchten, dass sie als PHP-DateTime-Objekte abgerufen werden, lesen Sie den Artikel [Vorgehensweise: Abrufen von Datums- und Uhrzeittypen als PHP-DateTime-Objekte mit dem PDO_SQLSRV-Treiber](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md)

## <a name="example-1"></a>Beispiel 1
Das folgende Beispiel zeigt die Syntax, welche festlegt, dass die Datums- und Uhrzeittypen als Zeichenfolgen abgerufen werden.

```php
<?php
$serverName = "MyServer";
$connectionInfo = array("Database"=>"AdventureWorks", 'ReturnDatesAsStrings'=> true);
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
   echo "Could not connect.\n";
   die(print_r(sqlsrv_errors(), true));
}

sqlsrv_close($conn);
?>
```

## <a name="example-2"></a>Beispiel 2
Das folgende Beispiel zeigt, dass Sie verschiedene Datumsangaben als Zeichenfolge abrufen können, sofern Sie beim Abruf UTF-8 spezifizieren. Dies gilt auch, wenn die Verbindung mittels `"ReturnDatesAsStrings" => false` erstellt wurde.

```php
<?php
$serverName = "MyServer";
$connectionInfo = array("Database"=>"AdventureWorks", "ReturnDatesAsStrings" => false);
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
   echo "Could not connect.\n";
   die(print_r(sqlsrv_errors(), true));
}

$tsql = "SELECT VersionDate FROM AWBuildVersion";

$stmt = sqlsrv_query($conn, $tsql);

if ($stmt === false) {
   echo "Error in statement preparation/execution.\n";
   die(print_r(sqlsrv_errors(), true));
}

sqlsrv_fetch($stmt);

// retrieve date as string
$date = sqlsrv_get_field($stmt, 0, SQLSRV_PHPTYPE_STRING("UTF-8"));

if ($date === false) {
   die(print_r(sqlsrv_errors(), true));
}

echo $date;

sqlsrv_close($conn);
?>
```

## <a name="example-3"></a>Beispiel 3
Das folgende Beispiel zeigt, wie Datumsangaben als Zeichenfolgen abgerufen werden können, indem UTF-8 und `"ReturnDatesAsStrings" => true` in der Verbindungszeichenfolge spezifiziert werden.

```php
<?php
$serverName = "MyServer";
$connectionInfo = array("Database"=>"AdventureWorks", 'ReturnDatesAsStrings'=> true, "CharacterSet" => 'utf-8');
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
   echo "Could not connect.\n";
   die(print_r(sqlsrv_errors(), true));
}

$tsql = "SELECT VersionDate FROM AWBuildVersion";

$stmt = sqlsrv_query($conn, $tsql);

if ($stmt === false) {
   echo "Error in statement preparation/execution.\n";
   die(print_r(sqlsrv_errors(), true));
}

sqlsrv_fetch($stmt);

// retrieve date as string
$date = sqlsrv_get_field($stmt, 0);

if ($date === false) {
   die(print_r(sqlsrv_errors(), true));
}

echo $date;
sqlsrv_close($conn);
?>
```

## <a name="example-4"></a>Beispiel 4
Das folgende Beispiel zeigt, wie das Datum als PHP-Typ abgerufen wird. `'ReturnDatesAsStrings'=> false` ist standardmäßig aktiviert.

```php
<?php
$serverName = "MyServer";
$connectionInfo = array("Database"=>"AdventureWorks");
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
   echo "Could not connect.\n";
   die(print_r(sqlsrv_errors(), true));
}

$tsql = "SELECT VersionDate FROM AWBuildVersion";

$stmt = sqlsrv_query($conn, $tsql);

if ($stmt === false) {
   echo "Error in statement preparation/execution.\n";
   die(print_r(sqlsrv_errors(), true));
}

sqlsrv_fetch($stmt);

// retrieve date as a DateTime object, then convert to string using PHP's date_format function
$date = sqlsrv_get_field($stmt, 0);

if ($date === false) {
   die(print_r(sqlsrv_errors(), true));
}

$date_string = date_format($date, 'jS, F Y');
echo "Date = $date_string\n";

sqlsrv_close($conn);
?>
```

## <a name="example-5"></a>Beispiel 5
Die Option ReturnDatesAsStrings auf Anweisungsebene überschreibt die entsprechende Verbindungsoption.

```php
<?php
$serverName = 'MyServer';
$connectionInfo = array('Database' => 'MyDatabase', 'ReturnDatesAsStrings' => false);
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
   echo "Could not connect.\n";
   die(print_r(sqlsrv_errors(), true));
}

$tableName = 'MyTable';
$options = array('ReturnDatesAsStrings' => true);
$query = "SELECT DateTimeCol FROM $tableName";
$stmt = sqlsrv_prepare($conn, $query, array(), $options);
if ($stmt === false) {
   echo "Error in statement preparation/execution.\n";
   die(print_r(sqlsrv_errors(), true));
}
sqlsrv_execute($stmt);

// Expect the fetched value to be a string
$field = sqlsrv_get_field($stmt, 0);
echo $field . PHP_EOL;

sqlsrv_close($conn);
?>
```

## <a name="see-also"></a>Weitere Informationen
[Abrufen von Daten](../../connect/php/retrieving-data.md)

[Abrufen von Datums- und Uhrzeittypen als PHP-datetime-Objekte mit dem PDO_SSQLSRV-Treiber](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md)
