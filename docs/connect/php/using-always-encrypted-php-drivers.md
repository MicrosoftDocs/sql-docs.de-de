---
title: Verwenden von Always Encrypted mit den PHP-Treibern für SQL Server
description: Hier erfahren Sie, wie Sie Always Encrypted mit den PHP-Treibern für SQL Server verwenden, um vertrauliche Daten in Ihrer Anwendung zu schützen.
ms.date: 12/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: ''
ms.author: v-daenge
author: David-Engel
manager: v-mabarw
ms.openlocfilehash: 4c16930d75d191c83f3f6eead028bc9913556e3c
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837137"
---
# <a name="using-always-encrypted-with-the-php-drivers-for-sql-server"></a>Verwenden von Always Encrypted mit den PHP-Treibern für SQL Server
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

## <a name="applicable-to"></a>Anwendbar auf
 -   Microsoft-Treiber 5.2 für PHP für SQL Server
 
## <a name="introduction"></a>Einführung

Dieser Artikel enthält Informationen zum Entwickeln von PHP-Anwendungen mithilfe von [Always Encrypted (Datenbank-Engine)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) und der [Microsoft-Treiber für PHP für SQL Server](../../connect/php/Microsoft-php-driver-for-sql-server.md).

Always Encrypted ermöglicht Clientanwendungen das Verschlüsseln von vertraulichen Daten in einer Weise, dass weder die Daten noch die Verschlüsselungsschlüssel zu irgendeinem Zeitpunkt gegenüber SQL Server oder Azure SQL-Datenbank offengelegt werden. Ein Treiber, bei dem Always Encrypted aktiviert ist, z. B. ODBC Driver for SQL Server, ver- und entschlüsselt sensible Daten in der Clientanwendung. Der Treiber ermittelt automatisch, welche Abfrageparameter vertraulichen Datenbankspalten (mit Always Encrypted geschützt) entsprechen. Die Werte dieser Parameter werden dann vor der Übergabe an SQL Server oder Azure SQL-Datenbank verschlüsselt. Auf ähnliche Weise entschlüsselt der Treiber die Daten transparent, die von verschlüsselten Datenbankspalten in Abfrageergebnissen empfangen werden. Weitere Informationen finden Sie unter [Immer verschlüsselt (Datenbank-Engine)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Die PHP-Treiber für SQL Server verwenden den ODBC-Treiber für SQL Server, um sensible Daten zu verschlüsseln.

## <a name="prerequisites"></a>Voraussetzungen

 -   Konfigurieren Sie Always Encrypted in Ihrer Datenbank. Diese Konfiguration umfasst die Bereitstellung von Always Encrypted-Schlüsseln und die Einrichtung der Verschlüsselung für ausgewählte Datenbankspalten. Wenn Sie nicht bereits über eine Datenbank verfügen, für die Always Encrypted konfiguriert ist, befolgen Sie die Anweisungen in [Erste Schritte mit Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted). Insbesondere sollte Ihre Datenbank die Metadatendefinitionen für einen Spaltenhauptschlüssel (Column Master Key, CMK), einen Spaltenverschlüsselungsschlüssel (Column Encryption Key, CEK) und eine Tabelle mit mindestens einer Spalte enthalten, die mit diesem CEK verschlüsselt ist.
 -   Stellen Sie sicher, dass der ODBC Driver for SQL Server Version 17 oder höher auf dem Entwicklungscomputer installiert ist. Weitere Informationen finden Sie unter [Microsoft ODBC Driver for SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md).

## <a name="enabling-always-encrypted-in-a-php-application"></a>Aktivieren von Always Encrypted in einer PHP-Anwendung

Die einfachste Möglichkeit zum Aktivieren der Verschlüsselung von Parametern, die auf verschlüsselte Spalten ausgerichtet sind, und der Entschlüsselung von Abfrageergebnissen besteht im Festlegen des Werts für die Verbindungszeichenfolge `ColumnEncryption` auf `Enabled`. Im Folgenden finden Sie Beispiele für das Aktivieren von Always Encrypted in den SQLSRV- und PDO_SQLSRV-Treibern:

SQLSRV:
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled");
$conn = sqlsrv_connect($server, $connectionInfo);
```

PDO_SQLSRV:
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```

Das Aktivieren von Always Encrypted für eine erfolgreiche Verschlüsselung oder Entschlüsselung ist nicht ausreichend. Sie müssen außerdem Folgendes sicherstellen:
 -   Die Anwendung verfügt über die Datenbankberechtigungen VIEW ANY COLUMN MASTER KEY DEFINITION und VIEW ANY COLUMN ENCRYPTION KEY DEFINITION, die für den Zugriff auf die Metadaten in der Datenbank über Always Encrypted-Schlüssel erforderlich sind. Einzelheiten hierzu finden Sie unter [Datenbankberechtigungen](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).
 -   Die Anwendung muss auf den CMK zugreifen können, der die CEKs für die abgefragten verschlüsselten Spalten schützt. Diese Anforderung ist abhängig vom Keystore-Anbieter, der den CMK speichert. Weitere Informationen finden Sie im Abschnitt [Arbeiten mit Spaltenhauptschlüsselspeichern](#working-with-column-master-key-stores).

## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Abrufen und Ändern von Daten in verschlüsselten Spalten

Nachdem Sie Always Encrypted für eine Verbindung aktiviert haben, können Sie zum Abrufen oder Ändern von Daten in verschlüsselten Datenbankspalten Standard-SQLSRV-APIs (siehe [API-Referenz für den SQLSRV-Treiber](../../connect/php/sqlsrv-driver-api-reference.md)) oder PDO_SQLSRV-APIs (siehe [Referenz zum Treiber PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md)) verwenden. Wenn Ihre Anwendung über die erforderlichen Datenberechtigungen verfügt und auf den Spaltenhauptschlüssel zugreifen kann, verschlüsselt der Treiber alle Abfrageparameter für verschlüsselte Spalten und entschlüsselt Daten, die aus verschlüsselten Spalten abgefragt wurden. Dieser Vorgang erfolgt in der Anwendung transparent, als ob die Spalten nicht verschlüsselt wären.

Wenn Always Encrypted nicht aktiviert ist, tritt bei Abfragen mit Parametern, die auf verschlüsselte Spalten ausgerichtet sind, ein Fehler auf. Daten können weiterhin aus verschlüsselten Spalten abgerufen werden, solange die Abfrage keine Parameter für verschlüsselte Spalten enthält. Der Treiber versucht jedoch nicht, die Daten zu entschlüsseln. Daher erhält die Anwendung die verschlüsselten Binärdaten (als Bytearrays).

Die folgende Tabelle fasst das Verhalten von Abfragen in Abhängigkeit davon zusammen, ob Always Encrypted aktiviert ist:

|Abfragemerkmal|Always Encrypted ist aktiviert und die Anwendung kann auf die Schlüssel und Schlüsselmetadaten zugreifen.|Always Encrypted ist aktiviert und die Anwendung kann nicht auf die Schlüssel oder Schlüsselmetadaten zugreifen.|Always Encrypted ist deaktiviert|
|---|---|---|---|
|Parameter für verschlüsselte Spalten|Parameterwerte werden transparent verschlüsselt.|Fehler|Fehler|
|Daten von verschlüsselten Spalten ohne Parameter abrufen, die auf verschlüsselte Spalten ausgerichtet sind.|Ergebnisse von verschlüsselten Spalten werden transparent entschlüsselt. Die Anwendung erhält Klartextspaltenwerte. |Fehler|Ergebnisse von verschlüsselten Spalten werden nicht entschlüsselt. Die Anwendung erhält verschlüsselte Werte als Bytearrays.|
 
Die folgenden Beispiele veranschaulichen das Abrufen und Ändern von Daten in verschlüsselten Spalten. Dabei wird eine Tabelle mit dem folgenden Schema angenommen: Die Spalten „SSN“ und „BirthDate“ werden verschlüsselt.
```
CREATE TABLE [dbo].[Patients](
 [PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY])
 GO
```

### <a name="data-insertion-example"></a>Beispiel für das Einfügen von Daten

In den folgenden Beispielen wird veranschaulicht, wie die SQLSRV- und PDO_SQLSRV-Treiber verwendet werden, um eine Zeile in die Tabelle „Patient“ einzufügen. Beachten Sie folgende Punkte:
 -   Es erfolgt keine spezielle Verschlüsselung im Beispielcode. Der Treiber erkennt und verschlüsselt automatisch die Werte der Parameter „SSN“ und „BirthDate“ für verschlüsselte Spalten. Durch diesen Mechanismus wird die Verschlüsselung für die Anwendung transparent.
 -   Die in die Datenbankspalten eingefügten Werte, einschließlich der verschlüsselten Spalten, werden als gebundene Parameter übergeben. Während die Verwendung von Parametern optional ist, wenn Werte an nicht verschlüsselte Spalten gesendet werden (obwohl es dringend empfohlen wird, da es dabei hilft, eine Einschleusung von SQL-Befehlen zu verhindern), ist sie für Werte erforderlich, die auf verschlüsselte Spalten ausgerichtet sind. Werden die in die Spalten „SSN“ oder „BirthDate“ eingefügten Werte als Literale übergeben, die in die Abfrageanweisung eingebettet sind, tritt ein Fehler auf, da der Treiber nicht versucht, Literale in Abfragen zu verschlüsseln oder anderweitig zu verarbeiten. Daher würde der Server sie zurückweisen, da sie mit den verschlüsselten Spalten inkompatibel sind.
 -   Beim Einfügen von Werten mithilfe von Bindungsparametern muss ein SQL-Typ, der mit dem Datentyp der Zielspalte identisch ist, oder dessen Konvertierung in den Datentyp der Zielspalte unterstützt wird, an die Datenbank übergeben werden. Der Grund für diese Anforderung ist, dass Always Encrypted wenige Typkonvertierungen unterstützt (weitere Informationen finden Sie unter [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)). Die beiden PHP-Treiber SQLSRV und PDO_SQLSRV verfügen jeweils über einen Mechanismus, mit dessen Hilfe der Benutzer den SQL-Typ des Werts ermitteln kann. Daher muss der Benutzer den SQL-Typ nicht explizit angeben.
  -   Für den SQLSRV-Treiber hat der Benutzer zwei Optionen:
   -   Verlassen Sie sich auf den PHP-Treiber, um den richtigen SQL-Typ zu ermitteln und festzulegen. In diesem Fall muss der Benutzer `sqlsrv_prepare` und `sqlsrv_execute` verwenden, um eine parametrisierte Abfrage auszuführen.
   -   Legen Sie den SQL-Typ explizit fest.
  -   Der PDO_SQLSRV-Treiber bietet nicht die Option, den SQL-Typ eines Parameters explizit festzulegen. Der PDO_SQLSRV-Treiber hilft dem Benutzer automatisch beim Ermitteln des SQL-Typs, wenn er einen Parameter bindet.
 -   Die Ermittlung des SQL-Typs durch die Treiber unterliegt einigen Einschränkungen:
  -   SQLSRV-Treiber:
   -   Wenn der Benutzer möchte, dass der Treiber die SQL-Typen für die verschlüsselten Spalten bestimmt, muss der Benutzer `sqlsrv_prepare` und `sqlsrv_execute` verwenden.
   -   Wenn `sqlsrv_query` bevorzugt wird, ist der Benutzer für die Angabe der SQL-Typen für alle Parameter verantwortlich. Der angegebene SQL-Typ muss die Zeichenfolgenlänge für Zeichenfolgentypen sowie die Skalierung und Genauigkeit für Dezimaltypen enthalten.
  -   PDO_SQLSRV-Treiber:
   -   Das Anweisungsattribut `PDO::SQLSRV_ATTR_DIRECT_QUERY` wird in einer parametrisierten Abfrage nicht unterstützt.
   -   Das Anweisungsattribut `PDO::ATTR_EMULATE_PREPARES` wird in einer parametrisierten Abfrage nicht unterstützt.
   
SQLSRV-Treiber und [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Abel;
$birthDate = "1996-10-19";
$params = array($ssn, $firstName, $lastName, $birthDate);
// during sqlsrv_prepare, the driver determines the SQL types for each parameter and pass them to SQL server
$stmt = sqlsrv_prepare($conn, $query, $params);
sqlsrv_execute($stmt);
```

SQLSRV-Treiber und [sqlsrv_query](../../connect/php/sqlsrv-query.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Abel";
$birthDate = "1996-10-19";
// need to provide the SQL types for ALL parameters  
// note the SQL types (including the string length) have to be the same at the column definition
$params = array(array(&$ssn, null, null, SQLSRV_SQLTYPE_CHAR(11)),
                array(&$firstName, null, null, SQLSRV_SQLTYPE_NVARCHAR(50)),
                array(&$lastName, null, null, SQLSRV_SQLTYPE_NVARCHAR(50)),
                array(&$birthDate, null, null, SQLSRV_SQLTYPE_DATE));
sqlsrv_query($conn, $query, $params);
```

PDO_SQLSRV-Treiber und [PDO::prepare](../../connect/php/pdo-prepare.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Able";
$birthDate = "1996-10-19";
// during PDO::prepare, the driver determines the SQL types for each parameter and pass them to SQL server
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $ssn);
$stmt->bindParam(2, $firstName);
$stmt->bindParam(3, $lastName);
$stmt->bindParam(4, $birthDate);
$stmt->execute();
```

### <a name="plaintext-data-retrieval-example"></a>Beispiel für das Abrufen von Klartextdaten

Im folgenden Beispiel wird das Filtern von Daten auf Basis verschlüsselter Werte und das Abrufen von Klartextdaten aus verschlüsselten Spalten mit dem SQLSRV- und PDO_SQLSRV-Treiber veranschaulicht. Beachten Sie folgende Punkte:
 -   Der in der WHERE-Klausel zum Filtern der Spalte „SSN“ verwendete Wert muss mit dem bind-Parameter übergeben werden, damit ihn der Treiber vor dem Senden an die Datenbank transparent verschlüsseln kann.
 -   Beim Ausführen einer Abfrage mit gebundenen Parametern ermitteln die PHP-Treiber automatisch den SQL-Typ für den Benutzer, es sei denn, der Benutzer gibt den SQL-Typ explizit an, wenn der SQLSRV-Treiber verwendet wird.
 -   Alle Werte werden vom Programm als Klartext ausgegeben, da der Treiber die aus den Spalten „SSN“ und „BirthDate“ abgerufenen Daten transparent entschlüsselt.
 
Hinweis: Abfragen können Übereinstimmungsvergleiche für verschlüsselte Spalten nur bei deterministischer Verschlüsselung ausführen. Weitere Informationen finden Sie im Abschnitt [Auswählen der deterministischen oder zufälligen Verschlüsselung](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

SQLSRV:
```
// since SSN is an encrypted column, need to pass the value in the WHERE clause through bind parameter
$query = "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = ?";
$ssn = "795-73-9838";
$stmt = sqlsrv_prepare($conn, $query, array(&$ssn));
// during sqlsrv_execute, the driver encrypts the ssn value and passes it to the database
sqlsrv_execute($stmt);
// fetch like usual
$row = sqlsrv_fetch_array($stmt);
```

PDO_SQLSRV:
```
// since SSN is an encrypted column, need to pass the value in the WHERE clause through bind parameter
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = ?";
$ssn = "795-73-9838";
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $ssn);
// during PDOStatement::execute, the driver encrypts the ssn value and passes it to the database
$stmt->execute();
// fetch like usual
$row = $stmt->fetch();
```

### <a name="ciphertext-data-retrieval-example"></a>Beispiel für das Abrufen von Chiffretextdaten

Wenn Always Encrypted nicht aktiviert ist, können Abfragen weiterhin Daten aus verschlüsselten Spalten abrufen, so lange die Abfrage keine Parameter für verschlüsselte Spalten enthält.

In den folgenden Beispielen wird das Abrufen von binär verschlüsselten Daten aus verschlüsselten Spalten mit dem SQLSRV- und PDO_SQLSRV-Treiber veranschaulicht. Beachten Sie folgende Punkte:
 -   Da Always Encrypted in der Verbindungszeichenfolge nicht aktiviert ist, gibt die Abfrage verschlüsselte Werte von „SSN“ und „BirthDate“ als Bytearrays zurück (das Programm konvertiert die Werte in Zeichenfolgen).
 -   Eine Abfrage, die Daten aus verschlüsselten Spalten mit deaktiviertem Always Encrypted abruft, kann Parameter aufweisen, so lange keiner der Parameter auf eine verschlüsselte Spalte ausgerichtet ist. In der folgenden Abfrage wird nach der Spalte „LastName“ gefiltert, die in der Datenbank nicht verschlüsselt ist. Wenn in der Abfrage nach „SSN“ oder „BirthDate“ gefiltert wird, würde ein Fehler auftreten.
 
SQLSRV:
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName] = ?";
$lastName = "Abel";
$stmt = sqlsrv_prepare($conn, $query, array(&$lastName));
sqlsrv_execute($stmt);
$row = sqlsrv_fetch_array($stmt);
```

PDO_SQLSRV:
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName] = ?";
$lastName = "Abel";
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $lastName);
$stmt->execute();
$row = $stmt->fetch();
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Vermeiden allgemeiner Probleme beim Abfragen von verschlüsselter Spalten

In diesem Abschnitt werden die allgemeinen Fehlerkategorien bei der Abfrage verschlüsselter Spalten über PHP-Anwendungen sowie einige Grundsätze zum Vermeiden dieser Fehler.

#### <a name="unsupported-data-type-conversion-errors"></a>Konvertierungsfehler durch nicht unterstützte Datentypen

Always Encrypted unterstützt einige Konvertierungen für verschlüsselte Datentypen. Eine ausführliche Liste der unterstützten Typkonvertierungen finden Sie unter [Always Encrypted (Datenbank-Engine)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Gehen Sie wie folgt vor, um Fehler bei Konvertierung des Datentyps zu vermeiden:
 -   Wenn Sie den SQLSRV-Treiber mit `sqlsrv_prepare` verwenden und `sqlsrv_execute` für den SQL-Typ ausführen, wird die Spaltengröße zusammen mit der Anzahl der Dezimalstellen des Parameters automatisch ermittelt.
 -   Wenn Sie den PDO_SQLSRV-Treiber verwenden, um eine Abfrage auszuführen, wird auch automatisch der SQL-Typ mit der Spaltengröße und der Anzahl der Dezimalstellen des Parameters ermittelt.
 -   Wenn Sie den SQLSRV-Treiber mit `sqlsrv_query` verwenden, um eine Abfrage auszuführen:
  -   Entweder ist der SQL-Typ des Parameters identisch mit dem Typ der Zielspalte oder die Konvertierung vom SQL-Typ in den Typ der Spalte wird unterstützt.
  -   Die Genauigkeit und Dezimalstellenanzahl von Parametern, die auf Spalten der SQL Server-Datentypen `decimal` und `numeric` ausgerichtet sind, ist mit der für die Zielspalte konfigurierten Genauigkeit und Dezimalstellenanzahl identisch.
  -   Die Genauigkeit von Parametern, die auf Spalten der SQL Server-Datentypen `datetime2`, `datetimeoffset` oder `time` ausgerichtet sind, ist in Abfragen, in denen die Zielspalte geändert wird, nicht größer als die Genauigkeit für die Zielspalte.
 -   Verwenden Sie nicht die PDO_SQLSRV-Anweisungsattribute `PDO::SQLSRV_ATTR_DIRECT_QUERY` oder `PDO::ATTR_EMULATE_PREPARES` in einer parametrisierten Abfrage.
 
#### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Fehler aufgrund der Übergabe von Klartext anstelle von verschlüsselten Werten

Jeder Wert für verschlüsselte Spalten muss vor dem Senden an den Server verschlüsselt werden. Ein Versuch, einen Klartextwert einzufügen, zu ändern oder nach einem Klartextwert für eine verschlüsselte Spalte zu filtern, führt zu einem Fehler. Stellen Sie Folgendes sicher, um solche Fehler zu vermeiden:
 -   Always Encrypted ist aktiviert (legen Sie in der Verbindungszeichenfolge das `ColumnEncryption`-Schlüsselwort auf `Enabled` fest).
 -   Sie verwenden Bind-Parameter zum Senden von Daten, die auf verschlüsselte Spalten ausgerichtet sind. Das folgende Beispiel zeigt eine Abfrage, die falsch nach einem Literal bzw. einer Konstante einer verschlüsselten Spalte (SSN) filtert:
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
```

## <a name="controlling-performance-impact-of-always-encrypted"></a>Kontrollieren der Auswirkungen von Always Encrypted auf die Leistung

Da Always Encrypted eine clientseitige Verschlüsselungstechnologie ist, werden die meisten Leistungseinbußen auf der Clientseite und nicht in der Datenbank beobachtet. Neben den Kosten für Verschlüsselungs- und Entschlüsselungsvorgänge ergeben sich die folgenden anderen Quellen für Leistungseinbußen auf der Clientseite:
 -   Zusätzliche Roundtrips zur Datenbank zum Abrufen von Metadaten für Abfrageparameter.
 -   Aufrufe an einen Spaltenhauptschlüsselspeicher für den Zugriff auf einen Spaltenhauptschlüssel.
 
### <a name="round-trips-to-retrieve-metadata-for-query-parameters"></a>Roundtrips zum Abrufen von Metadaten für Abfrageparameter

Wenn Always Encrypted für eine Verbindung aktiviert ist, ruft der ODBC-Treiber standardmäßig [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) für jede parametrisierte Abfrage auf, wobei die Abfrageanweisung (ohne Parameterwerte) an SQL Server übergeben wird. Die Abfrageanweisung wird von dieser gespeicherten Prozedur analysiert, um zu ermitteln, ob Parameter verschlüsselt werden müssen. In diesem Fall werden verschlüsselungsbezogene Informationen für jeden Parameter zurückgegeben, die der Treiber dann verschlüsseln kann.

Da der Benutzer mit den PHP-Treibern einen Parameter in einer vorbereiteten Anweisung ohne Angabe des SQL-Typs binden kann, rufen beim Binden eines Parameters in einer Always Encrypted-aktivierten Verbindung die PHP-Treiber [SQLDescribeParam](../../odbc/reference/syntax/sqldescribeparam-function.md) für den Parameter auf, um SQL-Typ, Spaltengröße und Dezimalstellen abzurufen. Die Metadaten werden dann zum Aufrufen von [SQLBindParameter]( ../../odbc/reference/syntax/sqlbindparameter-function.md) verwendet. Diese zusätzlichen `SQLDescribeParam`-Aufrufe erfordern keine zusätzlichen Roundtrips zur Datenbank, da der ODBC-Treiber die Informationen bereits auf der Clientseite gespeichert hat, als `sys.sp_describe_parameter_encryption` aufgerufen wurde.

Das oben beschriebene Verhalten stellt einen hohen Grad an Transparenz für die Clientanwendung sicher. Außerdem muss der Anwendungsentwickler nicht beachten, welche Abfragen auf verschlüsselte Spalten zugreifen, so lange die Werte für verschlüsselte Spalten in Parametern an den Treiber übergeben werden.

Im Gegensatz zum ODBC Driver for SQL Server wird das Aktivieren von Always Encrypted auf der Anweisung-/Abfrageebene in den PHP-Treibern noch nicht unterstützt. 

### <a name="column-encryption-key-caching"></a>Zwischenspeicherung von Spaltenverschlüsselungsschlüsseln

Der Treiber speichert die Spaltenverschlüsselungsschlüssel (CEKs) unverschlüsselt im Zwischenspeicher, um die Anzahl der Aufrufe an einen Spaltenhauptschlüsselspeicher zum Entschlüsseln von CEKs zu verringern. Nach dem Erhalt des verschlüsselten CEK (ECEK) aus den Datenbankmetadaten versucht der Treiber zunächst, den unverschlüsselten CEK zu finden, der dem verschlüsselten Schlüsselwert im Cache entspricht. Der Treiber ruft den Schlüsselspeicher, der den CMK enthält, nur auf, wenn der entsprechende CEK im Klartext im Cache nicht gefunden werden kann.

Hinweis: Beim ODBC-Treiber für SQL Server werden die Einträge im Cache nach einem Zeitlimit von zwei Stunden entfernt. Dieses Verhalten bedeutet, dass der Treiber den Schlüsselspeicher für einen bestimmten ECEK während der Lebensdauer der Anwendung oder alle zwei Stunden (je nachdem, welches Intervall kürzer ist) nur einmal kontaktiert.

## <a name="working-with-column-master-key-stores"></a>Arbeiten mit Spaltenhauptschlüsselspeichern

Der Treiber benötigt zum Verschlüsseln oder Entschlüsseln von Daten einen CEK, der für die Zielspalte konfiguriert ist. CEKs werden in verschlüsselter Form (als ECEKs) in den Datenbankmetadaten gespeichert. Jeder CEK besitzt einen entsprechenden CMK, mit dem er verschlüsselt wurde. In den [Datenbankmetadaten](../../t-sql/statements/create-column-master-key-transact-sql.md) wird nicht der CMK selbst gespeichert, sondern nur der Name des Schlüsselspeichers sowie Informationen zur Suche nach dem CMK.

Wenn der Treiber den Klartextwert eines ECEK abrufen möchten, benötigt er zunächst die Metadaten für den CEK und den entsprechenden CMK. Mit diesen Informationen kontaktiert er den Schlüsselspeicher, der den CMK enthält, und fordert die Entschlüsselung des ECEK an. Die Kommunikation zwischen Treiber und Schlüsselspeicher erfolgt über einen Schlüsselspeicheranbieter.

Für Microsoft-Treiber 5.3.0 für PHP für SQL Server werden nur Windows-Zertifikatspeicheranbieter und Azure Key Vault unterstützt. Der andere, vom ODBC Driver unterstützte Keystore-Anbieter (Custom Keystore Provider, benutzerdefinierter Keystore-Anbieter) wird noch nicht unterstützt.

### <a name="using-the-windows-certificate-store-provider"></a>Verwenden des Windows-Zertifikatspeicheranbieters

Unter Windows enthält ODBC Driver for SQL Server einen integrierten CMK-Speicheranbieter für den Windows-Zertifikatspeicher namens `MSSQL_CERTIFICATE_STORE`. (Dieser Anbieter ist nicht unter macOS oder Linux verfügbar.) Mit diesem Anbieter wird der CMK lokal auf dem Clientcomputer gespeichert, und es ist keine weitere Konfiguration durch die Anwendung erforderlich, um ihn mit dem Treiber zu verwenden. Die Anwendung muss jedoch auf das Zertifikat und den privaten Schlüssel im Speicher zugreifen können. Weitere Informationen finden Sie unter [Create and Store Column Master Keys (Always Encrypted)](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)(Erstellen und Speichern von Spaltenhauptschlüsseln (Always Encrypted)).

### <a name="using-azure-key-vault"></a>Verwenden von Azure Key Vault

Azure Key Vault bietet eine Möglichkeit zum Speichern von Verschlüsselungsschlüsseln, Kennwörtern und anderen Geheimnissen mit Azure und kann zum Speichern von Schlüsseln für Always Encrypted verwendet werden. Der ODBC Driver for SQL Server (Version 17 und höher) enthält einen integrierten Hauptschlüssel-Speicheranbieter für Azure Key Vault. Die folgenden Verbindungsoptionen behandeln die Azure Key Vault-Konfiguration: `KeyStoreAuthentication`, `KeyStorePrincipalId` und `KeyStoreSecret`. 
 -   `KeyStoreAuthentication` kann einen von zwei möglichen Zeichenfolgenwerten annehmen: `KeyVaultPassword` und `KeyVaultClientSecret`. Diese Werte steuern, welche Art von Authentifizierungsanmeldeinformationen mit den anderen beiden Schlüsselwörtern verwendet wird.
 -   `KeyStorePrincipalId` nimmt eine Zeichenfolge an, die einen Bezeichner für das Konto darstellt, das auf die Azure Key Vault-Instanz zugreifen möchte. 
     -   Wenn `KeyStoreAuthentication` auf `KeyVaultPassword` festgelegt ist, muss `KeyStorePrincipalId` der Name eines Azure Active Directory-Benutzers sein.
     -   Wenn `KeyStoreAuthentication` auf `KeyVaultClientSecret` festgelegt ist, muss `KeyStorePrincipalId` eine Anwendungsclient-ID sein.
 -   `KeyStoreSecret` nimmt eine Zeichenfolge an, die ein Anmeldeinformationsgeheimnis darstellt. 
     -   Wenn `KeyStoreAuthentication` auf `KeyVaultPassword` festgelegt ist, muss `KeyStoreSecret` das Kennwort des Benutzers sein. 
     -   Wenn `KeyStoreAuthentication` auf `KeyVaultClientSecret` festgelegt ist, muss `KeyStoreSecret` das Anwendungsgeheimnis sein, das der Anwendungsclient-ID zugeordnet ist.

Alle drei Optionen müssen in der Verbindungszeichenfolge vorhanden sein, um Azure Key Vault zu verwenden. Außerdem muss `ColumnEncryption` auf `Enabled` festgelegt werden. Wenn `ColumnEncryption` auf `Disabled` festgelegt ist, aber die Azure Key Vault-Optionen vorhanden sind, wird das Skript zwar ohne Fehler fortgesetzt, es wird jedoch keine Verschlüsselung ausgeführt.

In den folgenden Beispielen wird gezeigt, wie Sie mithilfe von Azure Key Vault eine Verbindung mit SQL Server herstellen.

SQLSRV:

Verwenden eines Azure Active Directory-Kontos:
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled", "KeyStoreAuthentication"=>"KeyVaultPassword", "KeyStorePrincipalId"=>$AADUsername, "KeyStoreSecret"=>$AADPassword);
$conn = sqlsrv_connect($server, $connectionInfo);
```
Verwenden einer Azure-Anwendungsclient-ID und des Geheimnisses:
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled", "KeyStoreAuthentication"=>"KeyVaultClientSecret", "KeyStorePrincipalId"=>$applicationClientID, "KeyStoreSecret"=>$applicationClientSecret);
$conn = sqlsrv_connect($server, $connectionInfo);
```

PDO_SQLSRV: Verwenden eines Azure Active Directory-Kontos:
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled; KeyStoreAuthentication = KeyVaultPassword; KeyStorePrincipalId = $AADUsername; KeyStoreSecret = $AADPassword;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```
Verwenden einer Azure-Anwendungsclient-ID und des Geheimnisses:
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled; KeyStoreAuthentication = KeyVaultClientSecret; KeyStorePrincipalId = $applicationClientID; KeyStoreSecret = $applicationClientSecret;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```

## <a name="limitations-of-the-php-drivers-when-using-always-encrypted"></a>Einschränkungen des PHP-Treibers bei Verwendung von Always Encrypted

SQLSRV und PDO_SQLSRV:
 -   Linux/macOS unterstützt den Windows-Zertifikatspeicheranbieter nicht
 -   Erzwingen von Parameterverschlüsselung
 -   Aktivieren von Always Encrypted auf Anweisungsebene 
 -   Bei Verwendung der Always Encrypted-Funktion und von Nicht-UTF8-Gebietsschemata unter Linux und macOS (z. B. „en_US.ISO-8859-1“) funktioniert das Einfügen von NULL-Daten oder einer leeren Zeichenfolge in eine verschlüsselte char(n)-Spalte möglicherweise nicht, wenn die Codepage 1252 nicht auf Ihrem System installiert ist.
 
Nur SQLSRV:
 -   Verwenden von `sqlsrv_query` für das Binden von Parametern ohne Angabe des SQL-Typs
 -   Verwenden von `sqlsrv_prepare` für das Binden von Parametern in einem Batch von SQL-Anweisungen  
 
Nur PDO_SQLSRV:
 -   Anweisungsattribut `PDO::SQLSRV_ATTR_DIRECT_QUERY` wird in einer parametrisierten Abfrage angegeben
 -   Anweisungsattribut `PDO::ATTR_EMULATE_PREPARE` wird in einer parametrisierten Abfrage angegeben
 -   Binden von Parametern in einem Batch von SQL-Anweisungen
 
Die PHP-Treiber erben außerdem die Einschränkungen, die vom ODBC Driver for SQL Server und der Datenbank auferlegt werden. Weitere Informationen finden Sie unter [Einschränkungen des ODBC-Treibers bei Verwendung von Always Encrypted](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md) und [Details zur Funktion](../../relational-databases/security/encryption/always-encrypted-database-engine.md#feature-details).  
  
## <a name="see-also"></a>Weitere Informationen  
[Programmierhandbuch für den PHP-SQL-Treiber](../../connect/php/programming-guide-for-php-sql-driver.md)

[API-Referenz für den SQLSRV-Treiber](../../connect/php/sqlsrv-driver-api-reference.md)  

[API-Referenz für den PDO_SQLSRV-Treiber](../../connect/php/pdo-sqlsrv-driver-reference.md)  
  
