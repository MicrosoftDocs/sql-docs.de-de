---
description: sqlsrv_get_field
title: sqlsrv_get_field | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_get_field
apitype: NA
helpviewer_keywords:
- sqlsrv_get_field
- API Reference, sqlsrv_get_field
- retrieving data, as a single field
ms.assetid: fa17cc56-fb38-433b-a40d-65642f04dc23
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3fd330fdfc43ae11f4958418afc1504a0e44502f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154061"
---
# <a name="sqlsrv_get_field"></a>sqlsrv_get_field
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Ruft Daten vom angegebenen Feld der aktuellen Zeile ab. Auf die Felddaten muss gemäß der Reihenfolge zugegriffen werden. So kann beispielsweise nicht auf Daten aus dem ersten Feld zugegriffen werden nachdem auf Daten vom zweiten Feld zugegriffen wurde.  
  
## <a name="syntax"></a>Syntax  
  
```  
sqlsrv_get_field( resource $stmt, int $fieldIndex [, int $getAsType])  
```  
  
#### <a name="parameters"></a>Parameter  
*$stmt:* Hierbei handelt es sich um eine Anweisungsressource, die einer ausgeführten Anweisung entspricht.  
  
*$fieldIndex*: Der Index des abzurufenden Felds. Indizes beginnen mit 0.  
  
*$getAsType* [OPTIONAL]: Eine **SQLSRV**-Konstante (**SQLSRV_PHPTYPE_&#x2a;**), die den PHP-Datentyp für die zurückgegebenen Daten bestimmt. Informationen zu den unterstützten Datentypen finden Sie unter [Konstanten &#40;Microsoft-Treiber für PHP für SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md). Wenn kein Rückgabetyp spezifiziert ist, wird ein PHP-Typ zurückgegeben. Informationen zu PHP-Standardtypen finden Sie unter [Default PHP Data Types](../../connect/php/default-php-data-types.md). Informationen zum Spezifizieren von PHP-Datentypen finden Sie unter [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md).  
  
## <a name="return-value"></a>Rückgabewert  
Die Felddaten. Sie können den PHP-Datentyp der zurückgegebenen Daten festlegen, indem Sie den *$getAsType* -Parameter verwenden. Falls kein Rückgabedatentyp festgelegt ist, wird der PHP-Standarddatentyp zurückgegeben. Informationen zu PHP-Standardtypen finden Sie unter [Default PHP Data Types](../../connect/php/default-php-data-types.md). Informationen zum Spezifizieren von PHP-Datentypen finden Sie unter [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md).  
  
## <a name="remarks"></a>Bemerkungen  
Die Kombination von **sqlsrv_fetch** und **sqlsrv_get_field** bewirkt, dass auf Daten nur vorangehend zugegriffen werden kann.  
  
Die Kombination **sqlsrv_fetch**/**sqlsrv_get_field** lädt nur ein Feld einer Zeile eines Resultsets in den Skriptspeicher und erlaubt die Festlegung von PHP-Rückgabetypen. (Informationen zum Festlegen des PHP-Rückgabetyps finden Sie unter [Vorgehensweise: Festlegen von PHP-Datentypen](../../connect/php/how-to-specify-php-data-types.md).) Diese Kombination von Funktionen macht es möglich Daten als Stream abzurufen. Informationen zum Abrufen von Daten als Stream finden Sie unter [Abrufen von Daten als Stream mit dem SQLSRV-Treiber](../../connect/php/retrieving-data-as-a-stream-using-the-sqlsrv-driver.md).  
  
## <a name="example"></a>Beispiel  
Das folgende Beispiel ruft eine Datenzeile ab, die eine Produktprüfung enthält, sowie den Namen des Prüfers. Zum Abrufen von Daten aus dem Resultset wird **sqlsrv_get_field** verwendet. Das Beispiel setzt voraus, dass SQL Server und die [AdventureWorks-Datenbank](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) auf dem lokalen Computer installiert sind. Wenn das Beispiel über die Befehlszeile ausgeführt wird, werden alle Ausgaben in die Konsole geschrieben.  
  
```  
<?php  
/*Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Set up and execute the query. Note that both ReviewerName and  
Comments are of the SQL Server nvarchar type. */  
$tsql = "SELECT ReviewerName, Comments   
         FROM Production.ProductReview  
         WHERE ProductReviewID=1";  
$stmt = sqlsrv_query( $conn, $tsql);  
if( $stmt === false )  
{  
     echo "Error in statement preparation/execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Make the first row of the result set available for reading. */  
if( sqlsrv_fetch( $stmt ) === false )  
{  
     echo "Error in retrieving row.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Note: Fields must be accessed in order.  
Get the first field of the row. Note that no return type is  
specified. Data will be returned as a string, the default for  
a field of type nvarchar.*/  
$name = sqlsrv_get_field( $stmt, 0);  
echo "$name: ";  
  
/*Get the second field of the row as a stream.  
Because the default return type for a nvarchar field is a  
string, the return type must be specified as a stream. */  
$stream = sqlsrv_get_field( $stmt, 1,   
                            SQLSRV_PHPTYPE_STREAM( SQLSRV_ENC_CHAR));  
while( !feof( $stream))  
{   
    $str = fread( $stream, 10000);  
    echo $str;  
}  
  
/* Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[API-Referenz für den SQLSRV-Treiber](../../connect/php/sqlsrv-driver-api-reference.md)  

[Abrufen von Daten](../../connect/php/retrieving-data.md)  

[Informationen zu den Codebeispielen in der Dokumentation](../../connect/php/about-code-examples-in-the-documentation.md)  
  
