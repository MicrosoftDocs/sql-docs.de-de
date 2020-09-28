---
title: Abrufen von Zeichendaten als Datenstrom mit dem SQLSRV-Treiber
description: In diesem Thema wird beschrieben, wie Zeichendaten als Datenstrom abgerufen werden können, wenn der Microsoft SQLSRV-Treiber für PHP für SQL Server verwendet wird.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- retrieving data, as a character stream
- streaming data
ms.assetid: 3c0dbca4-abfc-4449-b133-66c819681840
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f0665f02e81c09a7ac753da738efa6cd92cc62c3
ms.sourcegitcommit: d1051f05a7db81ec62d9785bb6af572408f3d4e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88680600"
---
# <a name="how-to-retrieve-character-data-as-a-stream-using-the-sqlsrv-driver"></a>Vorgehensweise: Abrufen von Zeichendaten als Stream mit dem SQLSRV-Treiber
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Das Abrufen von Daten als Stream ist nur im SQLSRV-Treiber von [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] und nicht im PDO_SQLSRV-Treiber verfügbar.  
  
Der SQLSRV-Treiber nutzt PHP-Streams zum Abrufen großer Datenmengen vom Server. Im Beispiel in diesem Thema wird veranschaulicht, wie Zeichendaten als Stream abgerufen werden.  
  
## <a name="example"></a>Beispiel  
Im folgenden Beispiel wird eine einzelne Zeile von der *Production.ProductReview* -Tabelle der AdventureWorks-Datenbank abgerufen. Das *Comments*-Feld der zurückgegebenen Zeile wird als Stream abgerufen und mithilfe der PHP-Funktion [fpassthru](https://php.net/manual/function.fpassthru.php) angezeigt.  
  
Das Abrufen von Daten als Stream erfolgt mithilfe von [sqlsrv_fetch](../../connect/php/sqlsrv-fetch.md) und [sqlsrv_get_field](../../connect/php/sqlsrv-get-field.md) als Zeichenstream spezifizierten Rückgabetyp. Der Rückgabetyp wird unter Verwendung der Konstanten **SQLSRV_PHPTYPE_STREAM** angegeben. Informationen zu **sqlsrv**-Konstanten finden Sie unter [Konstanten &#40;Microsoft-Treiber für PHP für SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).  
  
Das Beispiel setzt voraus, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und die [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)-Datenbank auf dem lokalen Computer installiert sind. Wenn das Beispiel über die Befehlszeile ausgeführt wird, werden alle Ausgaben in die Konsole geschrieben.  
  
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
  
/* Set up the Transact-SQL query. */  
$tsql = "SELECT ReviewerName,   
               CONVERT(varchar(32), ReviewDate, 107) AS [ReviewDate],  
               Rating,   
               Comments   
         FROM Production.ProductReview   
         WHERE ProductReviewID = ? ";  
  
/* Set the parameter value. */  
$productReviewID = 1;  
$params = array( $productReviewID);  
  
/* Execute the query. */  
$stmt = sqlsrv_query($conn, $tsql, $params);  
if( $stmt === false )  
{  
     echo "Error in statement execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Retrieve and display the data. The first three fields are retrieved  
as strings and the fourth as a stream with character encoding. */  
if(sqlsrv_fetch( $stmt ) === false )  
{  
     echo "Error in retrieving row.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
echo "Name: ".sqlsrv_get_field( $stmt, 0 )."\n";  
echo "Date: ".sqlsrv_get_field( $stmt, 1 )."\n";  
echo "Rating: ".sqlsrv_get_field( $stmt, 2 )."\n";  
echo "Comments: ";  
$comments = sqlsrv_get_field( $stmt, 3,   
                             SQLSRV_PHPTYPE_STREAM(SQLSRV_ENC_CHAR));  
fpassthru($comments);  
  
/* Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
Die ersten drei Felder werden entsprechend des jeweiligen PHP-Standardtyps zurückgegeben, da für sie kein PHP-Rückgabetyp angegeben ist. Informationen zu PHP-Datentypen finden Sie unter [Default PHP Data Types](../../connect/php/default-php-data-types.md). Weitere Informationen zum Angeben von PHP-Rückgabetypen finden Sie unter [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[Abrufen von Daten](../../connect/php/retrieving-data.md)

[Abrufen von Daten als Stream mit dem SQLSRV-Treiber](../../connect/php/retrieving-data-as-a-stream-using-the-sqlsrv-driver.md)

[Informationen zu den Codebeispielen in der Dokumentation](../../connect/php/about-code-examples-in-the-documentation.md)  
  
