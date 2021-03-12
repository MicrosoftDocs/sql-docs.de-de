---
title: Always Encrypted mit Secure Enclaves mit den PHP-Treibern
description: Erfahren Sie, wie Sie Always Encrypted mit Secure Enclaves mit den Microsoft Treibern für PHP für SQL Server verwenden.
ms.date: 01/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: ''
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: c49d81783f82c41cd95e25b137807b006e5b2ad1
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464706"
---
# <a name="using-always-encrypted-with-secure-enclaves-with-the-php-drivers-for-sql-server"></a>Verwenden von Always Encrypted mit Secure Enclaves mit den PHP-Treibern für SQL Server

[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

## <a name="applicable-to"></a>Anwendbar auf

- Microsoft-Treiber 5.8.0 für PHP für SQL Server

## <a name="introduction"></a>Einführung

[Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md) ist die zweite Iteration des Always Encrypted-Features für SQL Server. Always Encrypted mit Secure Enclaves ermöglicht es Benutzern, umfangreiche Berechnungen mit verschlüsselten Daten durchzuführen, indem sie eine Secure Enclave erstellen. Dabei handelt es sich um einen Bereich des Speichers auf dem Server, in dem verschlüsselte Daten in einer Datenbank entschlüsselt werden, sodass Berechnungen erfolgen können. Die unterstützten Vorgänge umfassen den Vergleich und Musterabgleich mit der `LIKE`-Klausel.

## <a name="enabling-always-encrypted-with-secure-enclaves"></a>Aktivieren von Always Encrypted mit Secure Enclaves

Unterstützung für Always Encrypted mit Secure Enclaves ist mit den PHP-Treibern für SQL Server ab 5.8.0 verfügbar. Always Encrypted mit Secure Enclaves erfordert SQL Server 2019 oder höher und mindestens Version 17.4 des ODBC-Treibers. Weitere Einzelheiten zu den allgemeinen Anforderungen für Always Encrypted mit den PHP-Treibern für SQL Server finden Sie [hier](using-always-encrypted-php-drivers.md).

Always Encrypted mit Secure Enclaves gewährleistet die Sicherheit verschlüsselter Daten durch den Nachweis der Enclave, d. h. die Bestätigung der Enclave durch einen externen Nachweisdienst. Um Secure Enclaves verwenden zu können, muss das Schlüsselwort `ColumnEncryption` den Nachweistyp und das Protokoll zusammen mit den zugehörigen Nachweisdaten, getrennt durch ein Komma, identifizieren. Version 17.4 des ODBC-Treibers unterstützt nur virtualisierungsbasierte Sicherheit (VBS) und das Protokoll „Host-Überwachungsdienst“ (Host Guardian Service, HGS) für den Enclave-Typ und das Protokoll. Die zugehörigen Nachweisdaten sind die URL des Nachweisservers. Daher wird die folgende Einstellung der Verbindungszeichenfolge hinzugefügt:

```
ColumnEncryption=VBS-HGS,http://attestationserver.mydomain/Attestation
```

Wenn das Protokoll nicht stimmt, erkennt der Treiber dies nicht. Die Verbindung schlägt fehl, ohne dass ein Fehler zurückgegeben wird. Wenn nur die Nachweis-URL nicht stimmt, wird die Verbindung erfolgreich hergestellt und ein Fehler ausgelöst, sobald eine Enclave-fähige Berechnung versucht wird. Andernfalls ist das Verhalten jedoch identisch mit dem ursprünglichen Always Encrypted-Verhalten. Durch Festlegen von `ColumnEncryption` auf `enabled` wird die normale Always Encrypted-Funktionalität bereitgestellt. Der Versuch eines Enclave-fähigen Vorgangs führt jedoch zu einem Fehler.

Vollständige Einzelheiten zur Konfiguration Ihrer Umgebung zur Unterstützung von Always Encrypted mit Secure Enclaves, einschließlich Einrichtung des Host-Überwachungsdiensts und der Erstellung der erforderlichen Verschlüsselungsschlüssel, finden Sie [hier](../../relational-databases/security/encryption/configure-always-encrypted-enclaves.md).

## <a name="examples"></a>Beispiele

In den folgenden Beispielen, eines für SQLSRV und eines für PDO_SQLSRV, wird eine Tabelle mit mehreren Datentypen in Klartext erstellt. Anschließend wird sie verschlüsselt, und es erfolgen Vergleiche und Musterabgleiche. Beachten Sie Folgendes:

- Bei der Verschlüsselung einer Tabelle mit `ALTER TABLE` darf nur eine Spalte für jeden Aufruf von `ALTER TABLE` verschlüsselt werden, sodass mehrere Aufrufe erforderlich sind, um mehrere Spalten zu verschlüsseln.
- Wenn der Vergleichsschwellenwert als Parameter für den Vergleich der Typen char und nchar übergeben wird, muss die Spaltenbreite im entsprechenden `SQLSRV_SQLTYPE_*` angegeben werden. Andernfalls wird der Fehler `HY104`, `Invalid precision value` zurückgegeben.
- Für den Musterabgleich muss die Sortierung mithilfe der `COLLATE`-Klausel als `Latin1_General_BIN2` angegeben werden.
- Wenn die Musterabgleichzeichenfolge als Parameter für den Abgleich der Typen char und nchar übergeben wird, muss der an `sqlsrv_query` oder `sqlsrv_prepare` übergebene `SQLSRV_SQLTYPE_*` die Länge der abzugleichenden Zeichenfolge und nicht die Größe der Spalte angeben. Der Grund ist, dass die Typen char und nchar am Ende der Zeichenfolge Leerzeichen auffüllen. Wenn Sie z. B. die Zeichenfolge `%abc%` mit einer char(10)-Spalte abgleichen, geben Sie `SQLSRV_SQLTYPE_CHAR(5)` an. Wenn Sie stattdessen `SQLSRV_SQLTYPE_CHAR(10)` angeben, gleicht die Abfrage `%abc%     ` (mit fünf angefügten Leerzeichen) ab. Alle Daten in der Spalte mit weniger als fünf angefügten Leerzeichen stimmen nicht überein (daher würde `abcdef` nicht mit `%abc%` übereinstimmen, da vier vorhanden sind). Verwenden Sie bei Unicode-Zeichenfolgen die Funktionen `mb_strlen` oder `iconv_strlen`, um die Anzahl der Zeichen abzurufen.
- Die PDO-Schnittstelle erlaubt nicht die Angabe der Länge eines Parameters. Geben Sie stattdessen in `PDOStatement::bindParam` die Länge 0 oder `null` an. Wenn die Länge explizit auf einen anderen Wert festgelegt wird, wird der Parameter als Ausgabeparameter behandelt.
- Der Musterabgleich funktioniert in Always Encrypted nicht für Typen, die keine Zeichenfolgen sind.
- Auf eine Fehlerüberprüfung wird aus Gründen der Übersichtlichkeit verzichtet.

Die folgenden Daten gelten für beide Beispiele:

```php
<?php
// Data for testing - integer, datetime2, char, nchar, varchar, and nvarchar
// String data is random, showing that we can match or compare anything
$testValues = array(array(1, "2019-12-31 01:00:00", "abcd", "㬚㔈♠既", "abcd", "㬚㔈♠既"),
                    array(-100, "1753-01-31 14:25:25.25", "#e@?q&zy+", "ઔܛ᎓Ե⅜", "#e@?q&zy+", "ઔܛ᎓Ե⅜"),
                    array(100, "2112-03-15 23:40:10.1594", "zyxwv", "㶋㘚ᐋꗡ", "zyxwv", "㶋㘚ᐋꗡ"),
                    array(0, "8888-08-08 08:08:08.08", "7t", "㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ", "7t", "㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ"),
                    );

// Queries to create the table and insert data
$createTable = "DROP TABLE IF EXISTS $myTable; 
                CREATE TABLE $myTable (c_integer int NULL, 
                                       c_datetime2 datetime2(7) NULL, 
                                       c_char char(32) NULL, 
                                       c_nchar nchar(32) NULL, 
                                       c_varchar varchar(32) NULL, 
                                       c_nvarchar nvarchar(32) NULL);";
$insertData = "INSERT INTO $myTable (c_integer, c_datetime2, c_char, c_nchar, c_varchar, c_nvarchar) VALUES (?, ?, ?, ?, ?, ?)";

// This is the query that encrypts the table in place
$encryptQuery = " ALTER TABLE $myTable
                      ALTER COLUMN [c_integer] integer
                      ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK-enclave], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
                      WITH (ONLINE = ON);
                  ALTER TABLE $myTable
                      ALTER COLUMN [c_datetime2] datetime2(7)
                      ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK-enclave], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
                      WITH (ONLINE = ON);
                  ALTER TABLE $myTable
                      ALTER COLUMN [c_char] char(32) COLLATE Latin1_General_BIN2
                      ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK-enclave], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
                      WITH (ONLINE = ON);
                  ALTER TABLE $myTable
                      ALTER COLUMN [c_nchar] nchar(32) COLLATE Latin1_General_BIN2
                      ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK-enclave], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
                      WITH (ONLINE = ON);
                  ALTER TABLE $myTable
                      ALTER COLUMN [c_varchar] varchar(32) COLLATE Latin1_General_BIN2
                      ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK-enclave], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
                      WITH (ONLINE = ON);
                  ALTER TABLE $myTable
                      ALTER COLUMN [c_nvarchar] nvarchar(32) COLLATE Latin1_General_BIN2
                      ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK-enclave], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
                      WITH (ONLINE = ON);
                  ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;";
?>
```

### <a name="sqlsrv"></a>SQLSRV

```php
<?php
// Specify Azure Key Vault credentials using the KeyStoreAuthentication, KeyStorePrincipalId, and KeyStoreSecret keywords
// Otherwise, the local Windows Certificate Store will be used
$options = array('database'=>$myDatabase,
                 'uid'=>$myUsername,
                 'pwd'=>$myPassword,
                 'CharacterSet'=>'UTF-8',
                 'ReturnDatesAsStrings'=>true,
                 'ColumnEncryption'=>"VBS-HGS,http://myattestationserver.mydomain/Attestation",
                 );
                 
$conn = sqlsrv_connect($myServer, $options);

// Create the table and insert the test data
$stmt = sqlsrv_query($conn, $createTable);

foreach ($testValues as $values) {
    $stmt = sqlsrv_prepare($conn, $insertData, $values);
    sqlsrv_execute($stmt);
}

// Encrypt the table in place
$stmt = sqlsrv_query($conn, $encryptQuery);

// Test comparison and pattern matching on the encrypted table
echo "Test comparisons:\n";

$intThreshold = 0;
$testGreater = "SELECT c_integer FROM $myTable WHERE c_integer > ?";
$param = array($intThreshold, SQLSRV_PARAM_IN, null, SQLSRV_SQLTYPE_INT);
$stmt = sqlsrv_prepare($conn, $testGreater, array($param));
getResults($stmt);
// Expect:
// 1
// 100

$datetimeThreshold = "3000-01-01 00:00:00.0";
$testLess = "SELECT c_datetime2 FROM $myTable WHERE c_datetime2 < ?";
$param = array($datetimeThreshold, SQLSRV_PARAM_IN, null, SQLSRV_SQLTYPE_DATETIME2);
$stmt = sqlsrv_prepare($conn, $testLess, array($param));
getResults($stmt);
// Expect:
// 2019-12-31 01:00:00.0000000
// 1753-01-31 14:25:25.2500000
// 2112-03-15 23:40:10.1594000

$charThreshold = "abcd";
$ncharThreshold = "㬚㔈♠既";

$testGreaterEqual = "SELECT c_char FROM $myTable WHERE c_char >= ?";
$param = array($charThreshold, SQLSRV_PARAM_IN, null, SQLSRV_SQLTYPE_CHAR(32));
$stmt = sqlsrv_prepare($conn, $testGreaterEqual, array($param));
getResults($stmt);
// Expect:
// abcd                            
// zyxwv                           

$testLessEqual = "SELECT c_nchar FROM $myTable WHERE c_nchar <= ?";
$param = array($ncharThreshold, SQLSRV_PARAM_IN, SQLSRV_PHPTYPE_STRING('UTF-8'), SQLSRV_SQLTYPE_NCHAR(32));
$stmt = sqlsrv_prepare($conn, $testLessEqual, array($param));
getResults($stmt);
// Expect:
// 㬚㔈♠既                            
// ઔܛ᎓Ե⅜                           
// 㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ                    

$testNotGreater = "SELECT c_varchar FROM $myTable WHERE c_varchar !> ?";
$param = array($charThreshold, SQLSRV_PARAM_IN, null, SQLSRV_SQLTYPE_VARCHAR);
$stmt = sqlsrv_prepare($conn, $testNotGreater, array($param));
getResults($stmt);
// Expect:
// abcd
// #e@?q&zy+
// 7t

$testNotLess = "SELECT c_nvarchar FROM $myTable WHERE c_nvarchar !< ?";
$param = array($ncharThreshold, SQLSRV_PARAM_IN, SQLSRV_PHPTYPE_STRING('UTF-8'), SQLSRV_SQLTYPE_NVARCHAR);
$stmt = sqlsrv_prepare($conn, $testNotLess, array($param));
getResults($stmt);
// Expect:
// 㬚㔈♠既
// 㶋㘚ᐋꗡ

echo "\nTest pattern matching:\n";

$charMatch = "%zy%";
$ncharMatch = "%㔈♠既%";

$param = array($charMatch, SQLSRV_PARAM_IN, null, SQLSRV_SQLTYPE_CHAR(strlen($charMatch)));
$testCharMatch = "SELECT c_char FROM $myTable WHERE c_char LIKE ? COLLATE Latin1_General_BIN2";
$stmt = sqlsrv_prepare($conn, $testCharMatch, array($param));
getResults($stmt);
// Expect:
// #e@?q&zy+                       
// zyxwv                           

$param = array($ncharMatch, SQLSRV_PARAM_IN, SQLSRV_PHPTYPE_STRING("UTF-8"), SQLSRV_SQLTYPE_NCHAR(iconv_strlen($ncharMatch)));
$testNCharMatch = "SELECT c_nchar FROM $myTable WHERE c_nchar LIKE ? COLLATE Latin1_General_BIN2";
$stmt = sqlsrv_prepare($conn, $testNCharMatch, array($param));
getResults($stmt);
// Expect:
// 㬚㔈♠既                            
// 㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ                    

$param = array($charMatch, SQLSRV_PARAM_IN, null, SQLSRV_SQLTYPE_VARCHAR(strlen($charMatch)));
$testVarcharMatch = "SELECT c_varchar FROM $myTable WHERE c_varchar LIKE ? COLLATE Latin1_General_BIN2";
$stmt = sqlsrv_prepare($conn, $testVarcharMatch, array($param));
getResults($stmt);
// Expect:
// #e@?q&zy+
// zyxwv

$param = array($ncharMatch, SQLSRV_PARAM_IN, SQLSRV_PHPTYPE_STRING("UTF-8"), SQLSRV_SQLTYPE_NVARCHAR(iconv_strlen($ncharMatch)));
$testNVarcharMatch = "SELECT c_nvarchar FROM $myTable WHERE c_nvarchar LIKE ? COLLATE Latin1_General_BIN2";
$stmt = sqlsrv_prepare($conn, $testNVarcharMatch, array($param));
getResults($stmt);
// Expect:
// 㬚㔈♠既
// 㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ

function getResults($stmt)
{
    sqlsrv_execute($stmt);
    while ($res = sqlsrv_fetch_array($stmt, SQLSRV_FETCH_NUMERIC)) {
        print_r($res[0]);
        echo "\n";
    }
}
?>
```

### <a name="pdo_sqlsrv"></a>PDO_SQLSRV

```php
<?php
// Specify Azure Key Vault credentials using the KeyStoreAuthentication, KeyStorePrincipalId, and KeyStoreSecret keywords
// Otherwise, the local Windows Certificate Store will be used
$options = "sqlsrv:server=$myServer;database=$myDatabase;driver={ODBC Driver 17 for SQL Server};";
$options .= "ColumnEncryption=VBS-HGS,http://myattestationserver.mydomain/Attestation",

$conn = new PDO($options, $myUsername, $myPassword);

// Create the table and insert the test data
$stmt = $conn->query($createTable);

foreach ($testValues as $values) {
    $stmt = $conn->prepare($insertData);
    $stmt->execute($values);
}

// Encrypt the table in place
$stmt = $conn->query($encryptQuery);

// Test comparison and pattern matching on the encrypted table
echo "Test comparisons:\n";

$intThreshold = 0;
$testGreater = "SELECT c_integer FROM $myTable WHERE c_integer > ?";
$stmt = $conn->prepare($testGreater);
$stmt->bindParam(1, $intThreshold, PDO::PARAM_INT);
getResults($stmt);
// Expect:
// 1
// 100

$datetimeThreshold = "3000-01-01 00:00:00.0";
$testLess = "SELECT c_datetime2 FROM $myTable WHERE c_datetime2 < ?";
$stmt = $conn->prepare($testLess);
$stmt->bindParam(1, $datetimeThreshold, PDO::PARAM_STR);
getResults($stmt);
// Expect:
// 2019-12-31 01:00:00.0000000
// 1753-01-31 14:25:25.2500000
// 2112-03-15 23:40:10.1594000

$charThreshold = "abcd";
$ncharThreshold = "㬚㔈♠既";

$testGreaterEqual = "SELECT c_char FROM $myTable WHERE c_char >= ?";
$stmt = $conn->prepare($testGreaterEqual);
$stmt->bindParam(1, $charThreshold, PDO::PARAM_STR);
getResults($stmt);
// Expect:
// abcd                            
// zyxwv                           

$testLessEqual = "SELECT c_nchar FROM $myTable WHERE c_nchar <= ?";
$stmt = $conn->prepare($testLessEqual);
$stmt->bindParam(1, $ncharThreshold, PDO::PARAM_STR);
getResults($stmt);
// Expect:
// 㬚㔈♠既                            
// ઔܛ᎓Ե⅜                           
// 㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ                    

$testNotGreater = "SELECT c_varchar FROM $myTable WHERE c_varchar !> ?";
$stmt = $conn->prepare($testNotGreater);
$stmt->bindParam(1, $charThreshold, PDO::PARAM_STR);
getResults($stmt);
// Expect:
// abcd
// #e@?q&zy+
// 7t

$testNotLess = "SELECT c_nvarchar FROM $myTable WHERE c_nvarchar !< ?";
$stmt = $conn->prepare($testNotLess);
$stmt->bindParam(1, $ncharThreshold, PDO::PARAM_STR);
getResults($stmt);
// Expect:
// 㬚㔈♠既
// 㶋㘚ᐋꗡ

echo "\nTest pattern matching:\n";

$charMatch = "%zy%";
$ncharMatch = "%㔈♠既%";

$testCharMatch = "SELECT c_char FROM $myTable WHERE c_char LIKE ? COLLATE Latin1_General_BIN2";
$stmt = $conn->prepare($testCharMatch);
$stmt->bindParam(1, $charMatch, PDO::PARAM_STR);
getResults($stmt);
// Expect:
// #e@?q&zy+                       
// zyxwv                           

$testNCharMatch = "SELECT c_nchar FROM $myTable WHERE c_nchar LIKE ? COLLATE Latin1_General_BIN2";
$stmt = $conn->prepare($testNCharMatch);
$stmt->bindParam(1, $ncharMatch, PDO::PARAM_STR,null,PDO::SQLSRV_ENCODING_UTF8);
getResults($stmt);
// Expect:
// 㬚㔈♠既                            
// 㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ                    

$testVarcharMatch = "SELECT c_varchar FROM $myTable WHERE c_varchar LIKE ? COLLATE Latin1_General_BIN2";
$stmt = $conn->prepare($testVarcharMatch);
$stmt->bindParam(1, $charMatch, PDO::PARAM_STR);
getResults($stmt);
// Expect:
// #e@?q&zy+
// zyxwv

$testNVarcharMatch = "SELECT c_nvarchar FROM $myTable WHERE c_nvarchar LIKE ? COLLATE Latin1_General_BIN2";
$stmt = $conn->prepare($testNVarcharMatch);
$stmt->bindParam(1, $ncharMatch, PDO::PARAM_STR,null,PDO::SQLSRV_ENCODING_UTF8);
getResults($stmt);
// Expect:
// 㬚㔈♠既
// 㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ

function getResults($stmt)
{
    $stmt->execute();
    while($res = $stmt->fetch(PDO::FETCH_NUM)) {
        print_r($res[0]);
        echo "\n";
    }
}
?>
```

Ausgabe:

```
Test comparisons:
1
100
2019-12-31 01:00:00.0000000
1753-01-31 14:25:25.2500000
2112-03-15 23:40:10.1594000
abcd                            
zyxwv                           
㬚㔈♠既                            
ઔܛ᎓Ե⅜                           
㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ                    
abcd
#e@?q&zy+
7t
㬚㔈♠既
㶋㘚ᐋꗡ

Test pattern matching:
#e@?q&zy+                       
zyxwv                           
㬚㔈♠既                            
㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ                    
#e@?q&zy+
zyxwv
㬚㔈♠既
㛜ꆶ㕸㔈♠既ꁺꖁ㓫ޘ갧ᛄ
```

## <a name="see-also"></a>Weitere Informationen

[Programmierhandbuch für den PHP-SQL-Treiber](programming-guide-for-php-sql-driver.md)  
[API-Referenz für den SQLSRV-Treiber](sqlsrv-driver-api-reference.md)  
[API-Referenz für den PDO_SQLSRV-Treiber](pdo-sqlsrv-driver-reference.md)  
[Verwendung von Always Encrypted mit den PHP-Treibern für SQL Server](using-always-encrypted-php-drivers.md)
