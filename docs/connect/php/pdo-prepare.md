---
title: PDO::prepare
description: API-Referenz für die PDO::prepare-Funktion im Microsoft PDO_SQLSRV-Treiber für PHP für SQL Server.
ms.custom: ''
ms.date: 01/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: a8b16fdc-c748-49be-acf2-a6ac7432d16b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e5e8a42aa2728ea8136c2889c18f26bafeea36ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039110"
---
# <a name="pdoprepare"></a>PDO::prepare
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Bereitet eine Anweisung für die Ausführung vor.

## <a name="syntax"></a>Syntax

```
PDOStatement PDO::prepare ( $statement [, array(key_pair)] )
```

#### <a name="parameters"></a>Parameter
$*statement*: Eine Zeichenfolge, die die SQL-Anweisung enthält

*key_pair:* Ein Array, das einen Attributnamen und einen Wert enthält. Weitere Informationen finden Sie im Abschnitt Hinweise.

## <a name="return-value"></a>Rückgabewert
Bei Erfolg wird ein PDOStatement-Objekt zurückgegeben. Bei einem Fehler wird entweder ein PDOException-Objekt oder „false“ zurückgegeben, abhängig vom Wert für `PDO::ATTR_ERRMODE`.

## <a name="remarks"></a>Bemerkungen
[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] wertet keine vorbereiteten Anweisungen vor der Ausführung aus.

In der folgenden Tabelle sind die möglichen Werte für *key_pair* aufgelistet.

|Key|BESCHREIBUNG|
|-------|---------------|
|PDO::ATTR_CURSOR|Definiert das Cursorverhalten. Der Standardwert ist `PDO::CURSOR_FWDONLY`, ein nicht scrollbarer Vorwärtscursor. `PDO::CURSOR_SCROLL` ist ein scrollbarer Cursor.<br /><br />Beispiel: `array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY )`.<br /><br />Bei Einstellung auf `PDO::CURSOR_SCROLL` können Sie dann mit `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE` den Typ des scrollbaren Cursors einstellen, der unten beschrieben wird.<br /><br />Weitere Informationen zu Ergebnismengen und Cursorn im PDO_SQLSRV-Treiber finden Sie unter [Cursortypen &#40;PDO_SQLSRV-Treiber&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).|
|PDO::ATTR_EMULATE_PREPARES|Dieses Attribut lautet standardmäßig „false“, kann aber durch `PDO::ATTR_EMULATE_PREPARES => true` geändert werden. Details und ein Beispiel finden Sie unter [Emulate_Prepare](#emulate-prepare).|
|PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE|Gibt den Typ des scrollbaren Cursors an. Gilt nur, wenn `PDO::ATTR_CURSOR` auf `PDO::CURSOR_SCROLL` gesetzt ist. Unten finden Sie die Werte, die dieses Attribut annehmen kann.|
|PDO::SQLSRV_ATTR_DECIMAL_PLACES|Gibt die Anzahl der Dezimalstellen beim Formatieren abgerufener Geldwerte (vom Typ „money“) an. Diese Option funktioniert nur, wenn `PDO::SQLSRV_ATTR_FORMAT_DECIMALS` auf „true“ festgelegt ist. Weitere Informationen finden Sie unter [Formatieren von Dezimalzeichenfolgen und Geldwerten (PDO_SQLSRV-Treiber)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md).|
|PDO::SQLSRV_ATTR_DIRECT_QUERY|Wird hierfür der Wert „true“ festgelegt, werden Abfragen direkt ausgeführt. Der Wert „false“ sorgt dafür, dass vorbereitete Anweisungen ausgeführt werden. Weitere Informationen zu `PDO::SQLSRV_ATTR_DIRECT_QUERY` finden Sie unter [Direkte Anweisungsausführung und vorbereitete Anweisungsausführung im PDO_SQLSRV-Treiber](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md).|
|PDO::SQLSRV_ATTR_ENCODING|PDO::SQLSRV_ENCODING_UTF8 (Standard)<br /><br />PDO::SQLSRV_ENCODING_SYSTEM<br /><br />PDO::SQLSRV_ENCODING_BINARY|
|PDO::SQLSRV_ATTR_FETCHES_DATETIME_TYPE|Gibt an, ob Datums- und Uhrzeittypen als [PHP-DateTime-Objekte](http://php.net/manual/en/class.datetime.php) abgerufen werden sollen. Weitere Informationen finden Sie unter [Vorgehensweise: Abrufen von Datums- und Uhrzeittypen als PHP-datetime-Objekte mit dem PDO_SQLSRV-Treiber](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE|Verarbeitet die Abrufe numerischer Werte aus Spalten mit numerischen SQL-Typen. Weitere Informationen finden Sie unter [PDO::setAttribute](../../connect/php/pdo-setattribute.md).|
|PDO::SQLSRV_ATTR_FORMAT_DECIMALS|Gibt an, ob Dezimalzeichenfolgen gegebenenfalls führende Nullen hinzugefügt werden sollen. Bei Festlegung erlaubt diese Option der Option `PDO::SQLSRV_ATTR_DECIMAL_PLACES` das Formatieren von Datentypen vom Typ „money“. Weitere Informationen finden Sie unter [Formatieren von Dezimalzeichenfolgen und Geldwerten (PDO_SQLSRV-Treiber)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md).| 
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|Weitere Informationen finden Sie unter [PDO::setAttribute](../../connect/php/pdo-setattribute.md).|

Bei Verwendung von `PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL` können Sie mit `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE` den Typ des Cursors festlegen. Übergeben Sie beispielsweise folgendes Array an PDO::prepare, um einen dynamischen Cursor festzulegen:
```
array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL, PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE => PDO::SQLSRV_CURSOR_DYNAMIC));
```
In der folgenden Tabelle sind die möglichen Werte für `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE` aufgeführt. Weitere Informationen zu scrollbaren Cursorn finden Sie unter [Cursortypen &#40;PDO_SQLSRV-Treiber&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).

|Wert|BESCHREIBUNG|
|---------|---------------|
|PDO::SQLSRV_CURSOR_BUFFERED|Erstellt einen clientseitigen (gepufferten) statischen Cursor, der das Resultset auf dem Clientcomputer in den Arbeitsspeicher puffert.|
|PDO::SQLSRV_CURSOR_DYNAMIC|Erstellt einen serverseitigen (gepufferten) dynamischen Cursor, der es Ihnen erlaubt, auf Zeilen in beliebiger Reihenfolge zuzugreifen und Änderungen in der Datenbank widerspiegelt.|
|PDO::SQLSRV_CURSOR_KEYSET|Erstellt einen serverseitigen Keysetcursor. Ein Keysetcursor aktualisiert nach dem Löschen einer Zeile aus einer Tabelle nicht die Zeilenanzahl (eine gelöschte Zeile wird ohne Werte zurückgegeben).|
|PDO::SQLSRV_CURSOR_STATIC|Erstellt einen serverseitigen statischen Cursor, der es Ihnen zwar erlaubt, auf Zeilen in beliebiger Reihenfolge zuzugreifen, aber die Änderungen in der Datenbank nicht widerspiegelt.<br /><br />`PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL` impliziert `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE => PDO::SQLSRV_CURSOR_STATIC`.|

Sie können ein PDOStatement-Objekt schließen, indem Sie `unset` aufrufen:
```
unset($stmt);
```

## <a name="forward-only-example"></a>Beispiel für nur Vorwärtscursor
In diesem Beispiel wird erläutert, wie Sie PDO::prepare mit Parametermarkern und einem Vorwärtscursor verwenden.

```
<?php
$database = "Test";
$server = "(local)";
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");

$col1 = 'a';
$col2 = 'b';

$query = "insert into Table1(col1, col2) values(?, ?)";
$stmt = $conn->prepare( $query, array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY, PDO::SQLSRV_ATTR_QUERY_TIMEOUT => 1  ) );
$stmt->execute( array( $col1, $col2 ) );
print $stmt->rowCount();
echo "\n";

$query = "insert into Table1(col1, col2) values(:col1, :col2)";
$stmt = $conn->prepare( $query, array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY, PDO::SQLSRV_ATTR_QUERY_TIMEOUT => 1  ) );
$stmt->execute( array( ':col1' => $col1, ':col2' => $col2 ) );
print $stmt->rowCount();

unset($stmt);
?>
```

## <a name="static-cursor-example"></a>Beispiel für statischen Cursor
Dieses Beispiel zeigt die Verwendung von PDO::prepare mit einem serverseitigen statischen Cursor. Ein Beispiel für einen clientseitigen Cursor finden Sie unter [Cursortypen &#40;PDO_SQLSRV-Treiber&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).

```
<?php
$database = "AdventureWorks";
$server = "(local)";
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");

$query = "select * from Person.ContactType";
$stmt = $conn->prepare( $query, array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL));
$stmt->execute();

echo "\n";

while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){
   print "$row[Name]\n";
}
echo "\n..\n";

$row = $stmt->fetch( PDO::FETCH_BOTH, PDO::FETCH_ORI_FIRST );
print_r($row);

$row = $stmt->fetch( PDO::FETCH_ASSOC, PDO::FETCH_ORI_REL, 1 );
print "$row[Name]\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_NEXT );
print "$row[1]\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_PRIOR );
print "$row[1]..\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_ABS, 0 );
print_r($row);

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_LAST );
print_r($row);
?>
```

## <a name="targeted-example"></a>Gezieltes Beispiel
Die folgenden zwei Ausschnitte zeigen, wie Sie PDO::prepare mit Daten für die Spalten CHAR/VARCHAR verwenden. Der Benutzer kann die Option `PDO::SQLSRV_ENCODING_SYSTEM` zur Vermeidung von impliziten Konvertierungen verwenden, da die Standardcodierung für PDO::prepare UTF-8 ist.

**Option 1:**
```
$options = array(PDO::SQLSRV_ATTR_ENCODING => PDO::SQLSRV_ENCODING_SYSTEM);
$statement = $pdo->prepare(
  'SELECT *
   FROM myTable
   WHERE myVarcharColumn = :myVarcharValue',
  $options
);

$statement->bindValue(':myVarcharValue', 'my data', PDO::PARAM_STR);
```

**Option 2:**
```
$statement = $pdo->prepare(
  'SELECT *
   FROM myTable
   WHERE myVarcharColumn = :myVarcharValue'
);
$p = 'my data';
$statement->bindParam(':myVarcharValue', $p, PDO::PARAM_STR, 0, PDO::SQLSRV_ENCODING_SYSTEM);
```

<a name="emulate-prepare" />

## <a name="prepare-example"></a>Prepare-Beispiel

Dieses Beispiel zeigt, wie PDO::prepare verwendet wird, wenn `PDO::ATTR_EMULATE_PREPARES` auf „true“ gesetzt ist.

```
<?php
$serverName = "yourservername";
$username = "yourusername";
$password = "yourpassword";
$database = "tempdb";
$conn = new PDO("sqlsrv:server = $serverName; Database = $database", $username, $password);

$pdo_options = array();
$pdo_options[PDO::ATTR_EMULATE_PREPARES] = true;
$pdo_options[PDO::SQLSRV_ATTR_ENCODING] = PDO::SQLSRV_ENCODING_UTF8;

$stmt = $conn->prepare("CREATE TABLE TEST([id] [int] IDENTITY(1,1) NOT NULL,
                                          [name] nvarchar(max))",
                                          $pdo_options);
$stmt->execute();

$prefix = '가각';
$name = '가각ácasa';
$name2 = '가각sample2';

$stmt = $conn->prepare("INSERT INTO TEST(name) VALUES(:p0)", $pdo_options);
$stmt->execute(['p0' => $name]);
unset($stmt);

$stmt = $conn->prepare("SELECT * FROM TEST WHERE NAME LIKE :p0", $pdo_options);
$stmt->execute(['p0' => "$prefix%"]);
foreach ($stmt as $row) {
    echo "\n" . 'FOUND: ' . $row['name'];
}

unset($stmt);
unset($conn);
?>
```

Der PDO_SQLSRV-Treiber ersetzt intern alle Platzhalter durch die Parameter, die durch [PDOStatement::bindParam()](../../connect/php/pdostatement-bindparam.md) gebunden sind. Daher wird eine SQL-Abfragezeichenfolge ohne Platzhalter an den Server gesendet. Sehen Sie sich folgendes Beispiel an:

```
$statement = $PDO->prepare("INSERT into Customers (CustomerName, ContactName) VALUES (:cus_name, :con_name)");
$statement->bindParam(:cus_name, "Cardinal");
$statement->bindParam(:con_name, "Tom B. Erichsen");
$statement->execute();
```

Wenn `PDO::ATTR_EMULATE_PREPARES` auf „false“ gesetzt ist (der Standardfall), werden diese Daten an die Datenbank gesendet:

```
"INSERT into Customers (CustomerName, ContactName) VALUES (:cus_name, :con_name)"
Information on :cus_name parameter
Information on :con_name parameter
```

Die Abfrage wird mit einer parametrisierten Abfragefunktion für Bindungsparameter ausgeführt. Wenn jedoch `PDO::ATTR_EMULATE_PREPARES` auf „true“ gesetzt ist, sieht die an den Server gesendete Abfrage in etwa so aus:

```
"INSERT into Customers (CustomerName, ContactName) VALUES ('Cardinal', 'Tom B. Erichsen')"
```

Durch das Festlegen von `PDO::ATTR_EMULATE_PREPARES` auf „true“ können einige Einschränkungen in SQL Server umgangen werden. Beispielsweise werden in einigen Transact-SQL-Klauseln von SQL Server keine benannten oder positionellen Parameter unterstützt. Darüber hinaus begrenzt SQL Server die Bindung auf höchstens 2.100 Parameter.

> [!NOTE]
> Wenn EMULATE_PREPARES auf „true“ gesetzt ist, kommt der Sicherheitsgewinn durch parametrisierte Anfragen nicht zum Tragen. Ihre Anwendung sollte deshalb sicherstellen, dass die an den bzw. die Parameter gebunden Daten keinen bösartigen Transact-SQL-Code enthalten.

### <a name="encoding"></a>Codieren

Wenn der Benutzer Parameter mit unterschiedlichen Codierungen (z. B. UTF-8 oder binär) binden möchte, sollte er die Codierung im PHP-Skript eindeutig angeben.

Der PDO_SQLSRV-Treiber überprüft zunächst die in `PDO::bindParam()` angegebene Codierung (z. B. `$statement->bindParam(:cus_name, "Cardinal", PDO::PARAM_STR, 10, PDO::SQLSRV_ENCODING_UTF8)`).

Wenn sie nicht gefunden wird, prüft der Treiber, ob eine Codierung in `PDO::prepare()` oder `PDOStatement::setAttribute()` festgelegt ist. Andernfalls verwendet der Treiber die in `PDO::__construct()` oder `PDO::setAttribute()` angegebene Codierung.

Zusätzlich dazu kann der Benutzer ab Version 5.8.0 [die erweiterten Zeichenfolgentypen aus PHP 7.2](https://wiki.php.net/rfc/extended-string-types-for-pdo) verwenden, wenn er PDO::prepare nutzt und `PDO::ATTR_EMULATE_PREPARES` auf TRUE festgelegt ist. So stellt er sicher, dass das `N`-Präfix verwendet wird. Die nachfolgenden Ausschnitte zeigen verschiedene Alternativen.

> [!NOTE]
> Standardmäßig ist EMULATE_PREPARES auf FALSE festgelegt. In diesem Fall werden die erweiterten PDO-Zeichenfolgenkonstanten ignoriert.

**Verwenden der Treiberoption PDO::SQLSRV_ENCODING_UTF8 bei der Bindung**

```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->bindParam(':value', $p, PDO::PARAM_STR, 0, PDO::SQLSRV_ENCODING_UTF8);
$stmt->execute();
```

**Verwenden des Attributs PDO::SQLSRV_ATTR_ENCODING**

```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true, PDO::SQLSRV_ATTR_ENCODING => PDO::SQLSRV_ENCODING_UTF8);
$stmt = $conn->prepare($sql, $options);
$stmt->execute([':value' => $p]);
```

**Verwenden der PDO-Konstante PDO::PARAM_STR_NATL**
```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->bindParam(':value', $p, PDO::PARAM_STR | PDO::PARAM_STR_NATL);
$stmt->execute();
```

**Festlegen des Parametertyps der Standardzeichenfolge PDO::PARAM_STR_NATL**
```
$conn->setAttribute(PDO::ATTR_DEFAULT_STR_PARAM, PDO::PARAM_STR_NATL);
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->execute([':value' => $p]);
```

### <a name="limitations"></a>Einschränkungen

Wie Sie sehen können, erfolgt die Bindung intern durch den Treiber. Eine gültige Abfrage wird zur Ausführung ohne Parameter an den Server gesendet. Verglichen mit dem Normalfall ergeben sich einige Einschränkungen, wenn die parametrisierte Abfragefunktion nicht verwendet wird.

- Sie funktioniert nicht für Parameter, die als `PDO::PARAM_INPUT_OUTPUT` gebunden sind.
    - Wenn der Benutzer `PDO::PARAM_INPUT_OUTPUT` in `PDO::bindParam()` angibt, wird eine PDO-Ausnahme ausgelöst.
- Sie funktioniert nicht für Parameter, die als Ausgabeparameter gebunden sind.
    - Wenn der Benutzer eine vorbereitete Anweisung mit Platzhaltern erstellt, die für Ausgabeparameter bestimmt sind (d. h. ein Gleichheitszeichen unmittelbar nach einem Platzhalter, wie `SELECT ? = COUNT(*) FROM Table1`), wird eine PDO-Ausnahme ausgelöst.
    - Wenn eine vorbereitete Anweisung eine gespeicherte Prozedur mit einem Platzhalter als Argument für einen Ausgabeparameter aufruft, wird keine Ausnahme ausgelöst, da der Treiber den Ausgabeparameter nicht erkennen kann. Die Variable, die der Benutzer für den Ausgabeparameter bereitstellt, bleibt jedoch unverändert.
- Doppelte Platzhalter für einen binär kodierten Parameter funktionieren nicht.

## <a name="see-also"></a>Weitere Informationen
[PDO-Klasse](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)

