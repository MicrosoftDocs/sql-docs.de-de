---
title: Resilienz von Verbindungen im Leerlauf
description: Erfahren Sie, was Resilienz von Verbindungen im Leerlauf ist, und wie sie sich in den Microsoft-Treibern für PHP für SQL Server verhält.
ms.date: 03/04/2021
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 07223e87c82a64fe17cd1b2969b5c93227c70af2
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247486"
---
# <a name="idle-connection-resiliency"></a>Resilienz von Verbindungen im Leerlauf
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Die [Verbindungsresilienz](../odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) beschreibt das Prinzip, dass eine unterbrochene Leerlaufverbindung mit bestimmten Einschränkungen wiederhergestellt werden kann. Wenn eine Verbindung mit Microsoft SQL Server unterbrochen wird, ermöglicht die Verbindungsresilienz es dem Client, automatisch eine Wiederherstellung der Verbindung zu versuchen. Die Verbindungsresilienz ist eine Eigenschaft der Datenquelle und wird nur von SQL Server 2014 und höher und Azure SQL-Datenbank unterstützt.

Die Verbindungsresilienz ist mit zwei Verbindungsschlüsselwörtern implementiert, die zu Verbindungszeichenfolgen hinzugefügt werden können: **ConnectRetryCount** und **ConnectRetryInterval**.

|Schlüsselwort|Werte|Standard|BESCHREIBUNG|
|-|-|-|-|
|**ConnectRetryCount**| Ganze Zahl von 0 bis 255|1|Die maximale Anzahl von Versuchen zum Wiederherstellen einer unterbrochenen Verbindung, bevor aufgegeben wird. Standardmäßig wird nur ein Versuch unternommen, eine Verbindung nach einer Unterbrechung wiederherzustellen. Der Wert 0 bedeutet, dass kein Wiederherstellungsversuch unternommen wird.|
|**ConnectRetryInterval**| Ganze Zahl von 1 bis 60|10| Die Zeit in Sekunden zwischen den Versuchen zum Wiederherstellen einer Verbindung. Die Anwendung versucht sofort nach dem Erkennen einer unterbrochenen Verbindung, diese wiederherzustellen, und wartet dann die mit **ConnectRetryInterval** angegebene Anzahl von Sekunden, bevor sie es erneut versucht. Dieses Schlüsselwort wird ignoriert, wenn **ConnectRetryCount** gleich 0 ist.

Wenn das Produkt aus **ConnectRetryCount** und **ConnectRetryInterval** größer ist als **LoginTimeout**, versucht der Client nach dem Erreichen von **LoginTimeout** nicht mehr, die Verbindung wiederherzustellen. Andernfalls wird die erneute Verbindung weiter versucht, bis **ConnectRetryCount** erreicht ist.

#### <a name="remarks"></a>Bemerkungen

Die Verbindungsresilienz gilt auch, wenn eine Verbindung sich im Leerlauf befindet. Fehler während der Ausführung einer Transaktion lösen beispielsweise keine erneuten Verbindungsversuche aus, sondern führen erwartungsgemäß zu einem Abbruch. In folgenden Situationen, die als nicht wiederherstellbare Sitzungszustände bezeichnet werden, werden keine erneuten Verbindungsversuche ausgelöst:

* Temporäre Tabellen
* Globale und lokale Cursor
* Transaktionskontext und Transaktionssperren auf Sitzungsebene
* Anwendungssperren
* EXECUTE AS/REVERT-Sicherheitskontext
* OLE-Automatisierungshandles
* Vorbereitete XML-Handles
* Ablaufverfolgungsflags

## <a name="example"></a>Beispiel

Der folgende Code stellt eine Verbindung mit einer Datenbank her und führt eine Abfrage aus. Die Verbindung wird durch Beendigung der Sitzung unterbrochen, und es wird versucht, über die unterbrochene Verbindung eine neue Abfrage zu senden. In diesem Beispiel wird die [AdventureWorks](/previous-versions/sql/sql-server-2008/ms124501(v=sql.100))-Beispieldatenbank verwendet.

In diesem Beispiel geben wir einen gepufferten Cursor an, bevor die Verbindung unterbrochen wird. Ohne die Angabe eines gepufferten Cursors würde die Verbindung nicht wiederhergestellt, weil ein aktiver serverseitiger Cursor vorhanden wäre und sich daher die Verbindung im Moment der Unterbrechung nicht im Leerlauf befände. In diesem Fall könnten wir allerdings „sqlsrv_free_stmt()“ aufrufen, bevor die Verbindung unterbrochen wird, um den Cursor freizugeben. Dann würde die Verbindung erfolgreich wiederhergestellt.

```php
<?php
// This function breaks the connection by determining its session ID and killing it.
// A separate connection is used to break the main connection because a session
// cannot kill itself. The sleep() function ensures enough time has passed for KILL
// to finish ending the session.
function BreakConnection( $conn, $conn_break )
{
    $stmt1 = sqlsrv_query( $conn, "SELECT @@SPID" );
    if ( sqlsrv_fetch( $stmt1 ) )
    {
        $spid=sqlsrv_get_field( $stmt1, 0 );
    }

    $stmt2 = sqlsrv_prepare( $conn_break, "KILL ".$spid );
    sqlsrv_execute( $stmt2 );
    sleep(1);
}

// Connect to the local server using Windows authentication and specify
// AdventureWorks as the database in use. Specify values for
// ConnectRetryCount and ConnectRetryInterval as well.
$databaseName = 'AdventureWorks2014';
$serverName = '(local)';
$connectionInfo = array( "Database"=>$databaseName, "ConnectRetryCount"=>10, "ConnectRetryInterval"=>10 );

$conn = sqlsrv_connect( $serverName, $connectionInfo );
if( $conn === false)  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}

// A separate connection that will be used to break the main connection $conn
$conn_break = sqlsrv_connect( $serverName, array( "Database"=>$databaseName) );

// Create a statement to retrieve the contents of a table
$stmt1 = sqlsrv_query( $conn, "SELECT * FROM HumanResources.Employee",
                       array(), array( "Scrollable"=>"buffered" ) );
if( $stmt1 === false )
{
     echo "Error in statement 1.\n";
     die( print_r( sqlsrv_errors(), true ));
}
else
{
    echo "Statement 1 successful.\n";
    $rowcount = sqlsrv_num_rows( $stmt1 );
    echo $rowcount." rows in result set.\n";
}

// Now break the connection $conn
BreakConnection( $conn, $conn_break );

// Create another statement. The connection will be reestablished.
$stmt2 = sqlsrv_query( $conn, "SELECT * FROM HumanResources.Department",
                       array(), array( "Scrollable"=>"buffered" ) );
if( $stmt2 === false )
{
     echo "Error in statement 2.\n";
     die( print_r( sqlsrv_errors(), true ));
}
else
{
    echo "Statement 2 successful.\n";
    $rowcount = sqlsrv_num_rows( $stmt2 );
    echo $rowcount." rows in result set.\n";
}

sqlsrv_close( $conn );
sqlsrv_close( $conn_break );
?>
```
Erwartete Ausgabe:
```
Statement 1 successful.
290 rows in result set.
Statement 2 successful.
16 rows in result set.
```

## <a name="see-also"></a>Weitere Informationen
[Verbindungsresilienz im Windows ODBC-Treiber](../odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)
