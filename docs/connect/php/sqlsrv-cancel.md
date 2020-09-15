---
description: sqlsrv_cancel
title: sqlsrv_cancel | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- sqlsrv_cancel
apitype: NA
helpviewer_keywords:
- sqlsrv_cancel
- API Reference, sqlsrv_cancel
ms.assetid: 75798c9b-f711-445d-9b8f-ba4d405ca50a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 463ccadd953bd628f7f7fc0f5a99f414fe2f8ed1
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88414236"
---
# <a name="sqlsrv_cancel"></a>sqlsrv_cancel
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Bricht eine Anweisung ab. Dies bedeutet, dass alle ausstehenden Ergebnisse für die Anweisung verworfen werden. Nachdem diese Funktion aufgerufen wurde, kann die Anweisung erneut ausgeführt werden, wenn sie mit [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md) vorbereitet wurde. Das Aufrufen dieser Funktion ist nicht erforderlich, wenn alle Ergebnisse, die der Anweisung zugeordnet sind, verwendet wurden.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sqlsrv_cancel( resource $stmt)  
```  
  
#### <a name="parameters"></a>Parameter  
*$stmt*: Die Anweisung, die abgebrochen werden soll.  
  
## <a name="return-value"></a>Rückgabewert  
Ein boolescher Wert: **true** , wenn der Vorgang erfolgreich war. Andernfalls lautet der Wert **false**.  
  
## <a name="example"></a>Beispiel  
Das folgende Beispiel nimmt die [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)-Datenbank als Ziel zur Ausführung einer Abfrage. Es werden dann Ergebnisse verarbeitet und gezählt, bis die Variable *$salesTotal* einen angegebenen Betrag erreicht. Die verbleibenden Abfrageergebnisse werden anschließend verworfen. Das Beispiel setzt voraus, dass SQL Server und die AdventureWorks-Datenbank auf dem lokalen Computer installiert sind. Wenn das Beispiel über die Befehlszeile ausgeführt wird, werden alle Ausgaben in die Konsole geschrieben.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and   
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Prepare and execute the query. */  
$tsql = "SELECT OrderQty, UnitPrice FROM Sales.SalesOrderDetail";  
$stmt = sqlsrv_prepare( $conn, $tsql);  
if( $stmt === false )  
{  
     echo "Error in statement preparation.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
if( sqlsrv_execute( $stmt ) === false)  
{  
     echo "Error in statement execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Initialize tracking variables. */  
$salesTotal = 0;  
$count = 0;  
  
/* Count and display the number of sales that produce revenue  
of $100,000. */  
while( ($row = sqlsrv_fetch_array( $stmt)) && $salesTotal <=100000)  
{  
     $qty = $row[0];  
     $price = $row[1];  
     $salesTotal += ( $price * $qty);  
     $count++;  
}  
echo "$count sales accounted for the first $$salesTotal in revenue.\n";  
  
/* Cancel the pending results. The statement can be reused. */  
sqlsrv_cancel( $stmt);  
?>  
```  
  
## <a name="comments"></a>Kommentare  
Eine Anweisung, die mit der Kombination von [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md) und [sqlsrv_execute](../../connect/php/sqlsrv-execute.md) vorbereitet und ausgeführt wird, kann nach dem Aufruf von **sqlsrv_cancel** erneut mit **sqlsrv_execute** ausgeführt werden. Eine Anweisung, die mit [sqlsrv_query](../../connect/php/sqlsrv-query.md) ausgeführt wird, kann nach dem Aufruf von **sqlsrv_cancel** nicht erneut ausgeführt werden.  
  
## <a name="see-also"></a>Weitere Informationen  
[API-Referenz für den SQLSRV-Treiber](../../connect/php/sqlsrv-driver-api-reference.md)

[Verbinden mit dem Server](../../connect/php/connecting-to-the-server.md)

[Abrufen von Daten](../../connect/php/retrieving-data.md)

[Informationen zu den Codebeispielen in der Dokumentation](../../connect/php/about-code-examples-in-the-documentation.md)

[sqlsrv_free_stmt](../../connect/php/sqlsrv-free-stmt.md)

  
