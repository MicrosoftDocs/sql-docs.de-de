---
title: PDO::errorInfo
description: API-Referenz für die PDO::errorInfo-Funktion im Microsoft PDO_SQLSRV-Treiber für PHP für SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 9d5481d5-13bc-4388-b3aa-78676c0fc709
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 62ad5c26342ce03d3098d5a835776d193df4b88c
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837231"
---
# <a name="pdoerrorinfo"></a>PDO::errorInfo
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Ruft erweiterte Fehlerinformationen des zuletzt ausgeführten Vorgangs des Datenbankhandles ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
array PDO::errorInfo();  
```  
  
## <a name="return-value"></a>Rückgabewert  
Ein Array mit Fehlerinformationen über den zuletzt ausgeführten Vorgang des Datenbankhandles. Dieses Array besteht aus den folgenden Feldern:  
  
-   Der SQLSTATE-Fehlercode  
  
-   Der treiberspezifische Fehlercode  
  
-   Die treiberspezifische Fehlermeldung  
  
Wenn kein Fehler vorliegt oder wenn der SQLSTATE nicht festgelegt ist, sind die treiberspezifischen Felder NULL.  
  
## <a name="remarks"></a>Bemerkungen  
PDO::errorInfo ruft nur die Fehlerinformationen für Vorgänge ab, die direkt in der Datenbank ausgeführt werden. Verwenden Sie PDOStatement::errorInfo, wenn eine PDOStatement-Instanz mit PDO::prepare oder PDO::query erstellt wird.  
  
Unterstützung für PDO wurde in Version 2.0 von [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]hinzugefügt.  
  
## <a name="example"></a>Beispiel  
In diesem Beispiel ist der Name der Spalte falsch geschrieben (`Cityx` anstelle von `City`) und verursacht einen Fehler, der dann gemeldet wird.  
  
```php
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks ", "");  
$query = "SELECT * FROM Person.Address where Cityx = 'Essen'";  
  
$conn->query($query);  
print $conn->errorCode();  
echo "\n";  
print_r ($conn->errorInfo());  
?>  
```  

## <a name="additional-odbc-messages"></a>Weitere ODBC-Meldungen

Wenn eine Ausnahme auftritt, gibt der ODBC-Treiber möglicherweise mehr als einen Fehler zurück, um Sie bei der Problemdiagnose zu unterstützen. PDO::errorInfo zeigt jedoch immer den ersten Fehler an. Als Reaktion auf diesen [Fehlerbericht](https://bugs.php.net/bug.php?id=78196) wurden [PDO::errorInfo](https://www.php.net/manual/en/pdo.errorinfo.php) und [PDOStatement::errorInfo](https://www.php.net/manual/en/pdostatement.errorinfo.php) aktualisiert, sodass Treiber nun angewiesen werden, *mindestens* die folgenden drei Felder zu zeigen:
```
0   SQLSTATE error code (a five characters alphanumeric identifier defined in the ANSI SQL standard).
1   Driver specific error code.
2   Driver specific error message.
```

Ab Version 5.9.0 besteht das Standardverhalten von PDO::errorInfo darin, weitere ODBC-Fehlermeldungen anzuzeigen, sofern verfügbar. Beispiel:

```php
<?php  
try {
    $conn = new PDO("sqlsrv:server=$server;", $uid, $pwd);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    $stmt = $conn->prepare("SET NOCOUNT ON; USE $database; SELECT 1/0 AS col1");
    $stmt->execute();
} catch (PDOException $e) {
    var_dump($e->errorInfo);
}
?>  
```  

Durch die Ausführung des obigen Skripts sollte eine Ausnahme ausgelöst worden sein und die Ausgabe wie folgt aussehen:

```
array(6) {
  [0]=>
  string(5) "01000"
  [1]=>
  int(5701)
  [2]=>
  string(91) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Changed database context to 'tempdb'."
  [3]=>
  string(5) "22012"
  [4]=>
  int(8134)
  [5]=>
  string(87) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Divide by zero error encountered."
}
```

Wenn der Benutzer die vorherige Methode bevorzugt, kann die aktuelle mithilfe der neuen Konfigurationsoption `pdo_sqlsrv.report_additional_errors` deaktiviert werden. Fügen Sie einfach die folgende Zeile am Anfang eines beliebigen PHP-Skripts hinzu:

```
ini_set('pdo_sqlsrv.report_additional_errors', 0);
```

In diesem Fall werden die Fehlerinformationen wie folgt angezeigt, wenn Sie das gleiche Beispielskript ausführen:

```
array(3) {
  [0]=>
  string(5) "01000"
  [1]=>
  int(5701)
  [2]=>
  string(91) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Changed database context to 'tempdb'."
}
```

Bei Bedarf kann der Benutzer die folgende Zeile in der Datei „php.ini“ hinzufügen, um dieses Feature in allen PHP-Skripts zu deaktivieren:

```
pdo_sqlsrv.report_additional_errors = 0
```

## <a name="warnings-and-errors"></a>Warnungen und Fehler

Seit Release 5.9.0 werden ODBC-Warnungen nicht mehr als Fehler protokolliert. Das heißt, das [Fehlercodes](../../odbc/reference/appendixes/appendix-a-odbc-error-codes.md) mit dem Präfix „01“ als Warnungen protokolliert werden. Anders ausgedrückt: Wenn der Benutzer ausschließlich Fehler protokollieren möchte, aktualisieren Sie die Datei „php.ini“ wie folgt:

```
[pdo_sqlsrv]  
pdo_sqlsrv.log_severity = 1
```

In diesem Fall enthält die Protokolldatei keine Warnmeldung(en). Überprüfen Sie, wie die [Protokollierung](./logging-activity.md#logging-activity-using-the-pdo_sqlsrv-driver) für pdo_sqlsrv-Benutzer funktioniert.

## <a name="see-also"></a>Weitere Informationen  
[PDO-Klasse](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  

[PDOStatement::errorInfo](../../connect/php/pdostatement-errorinfo.md)