---
title: 'Gewusst wie: Senden und Abrufen von UTF-8-Daten mithilfe der integrierten UTF-8-Unterstützung'
description: In diesem Artikel erfahren Sie, wie Sie UTF-8-codierte Daten mithilfe der in die PHP-Treiber integrierten UTF-8-Unterstützung senden und abrufen.
ms.custom: ''
ms.date: 03/23/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- retrieving data, UTF-8 encoded data
- converting data types
- updating data
ms.assetid: 366c57cf-352f-4202-8074-6ddce44880d1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d62ab2a7715360b8cceeecccada01717d87471c0
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91726811"
---
# <a name="how-to-send-and-retrieve-utf-8-data-using-built-in-utf-8-support"></a>Gewusst wie: Senden und Abrufen von UTF-8-Daten mithilfe der integrierten UTF-8-Unterstützung
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Wenn Sie den PDO_SQLSRV-Treiber verwenden, können Sie die Codierung mit dem Attribut „PDO::SQLSRV_ATTR_ENCODING“ festlegen. Weitere Informationen finden Sie unter [Konstanten &#40;Microsoft-Treiber für PHP für SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).  
  
Im weiteren Verlauf dieses Themas wird die Codierung mit dem SQLSRV-Treiber erläutert.  
  
Gehen Sie zum Senden/Abrufen von UTF-8-codierten Daten an den/vom Server wie folgt vor:  
  
1.  Stellen Sie sicher, dass die Quell- oder Ziel-Spalte den Typ **nchar** oder **nvarchar**aufweist.  
  
2.  Geben Sie im Parameterarray `SQLSRV_PHPTYPE_STRING('UTF-8')` als PHP-Typ an. Oder legen Sie `"CharacterSet" => "UTF-8"` als eine Verbindungsoption fest.  
  
    Wenn Sie einen Zeichensatz als Teil der Verbindungsoptionen angeben, geht der Treiber davon aus, dass auch die anderen Optionszeichenfolgen der Verbindung den gleichen Zeichensatz verwenden. Auch beim Servernamen und den Abfragezeichenfolgen geht er vom selben Zeichensatz aus.  
  
Sie können UTF-8 oder „SQLSRV_ENC_CHAR“ an **CharacterSet** übergeben, aber nicht „SQLSRV_ENC_BINARY“. Die Standardcodierung ist „SQLSRV_ENC_CHAR“.  
  
## <a name="example"></a>Beispiel  
Im folgenden Beispiel wird veranschaulicht, wie Sie bei der Herstellung der Verbindung, durch die Festlegung auf den UTF-8 Zeichensatz, das Senden und Empfangen von UTF-8-codierten Daten ermöglichen. Das Beispiel aktualisiert die Spalte „Kommentare“ der Tabelle „Produktion.ProduktReview“ auf eine angegebene Review-ID. Das Beispiel ruft auch die neuen, aktualisierten Daten auf und zeigt sie an. Beachten Sie, dass die Spalte „Comments“ den Typ **nvarchar(3850)** aufweist. Beachten Sie, dass Daten vor dem Senden an den Server mit der PHP-Funktion **utf8_encode** in UTF-8-Codierung konvertiert werden. Dies erfolgt nur zu Demonstrationszwecken. In einem realen Anwendungsszenario würden Sie mit UTF-8-codierten Daten beginnen.  
  
Das Beispiel setzt voraus, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und die [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)-Datenbank auf dem lokalen Computer installiert sind. Wenn das Beispiel über den Browser ausgeführt wird, werden alle Ausgaben im Browser geschrieben.  
  
```  
<?php  
  
// Connect to the local server using Windows Authentication and  
// specify the AdventureWorks database as the database in use.   
//   
$serverName = "MyServer";  
$connectionInfo = array( "Database"=>"AdventureWorks", "CharacterSet" => "UTF-8");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
  
if ( $conn === false ) {  
   echo "Could not connect.<br>";  
   die( print_r( sqlsrv_errors(), true));  
}  
  
// Set up the Transact-SQL query.  
//   
$tsql1 = "UPDATE Production.ProductReview  
          SET Comments = ?  
          WHERE ProductReviewID = ?";  
  
// Set the parameter values and put them in an array. Note that  
// $comments is converted to UTF-8 encoding with the PHP function  
// utf8_encode to simulate an application that uses UTF-8 encoded data.   
//   
$reviewID = 3;  
$comments = utf8_encode("testing 1, 2, 3, 4.  Testing.");  
$params1 = array(  
                  array( $comments, null ),  
                  array( $reviewID, null )  
                );  
  
// Execute the query.  
//   
$stmt1 = sqlsrv_query($conn, $tsql1, $params1);  
  
if ( $stmt1 === false ) {  
   echo "Error in statement execution.<br>";  
   die( print_r( sqlsrv_errors(), true));  
}  
else {  
   echo "The update was successfully executed.<br>";  
}  
  
// Retrieve the newly updated data.  
//   
$tsql2 = "SELECT Comments   
          FROM Production.ProductReview   
          WHERE ProductReviewID = ?";  
  
// Set up the parameter array.  
//   
$params2 = array($reviewID);  
  
// Execute the query.  
//   
$stmt2 = sqlsrv_query($conn, $tsql2, $params2);  
if ( $stmt2 === false ) {  
   echo "Error in statement execution.<br>";  
   die( print_r( sqlsrv_errors(), true));  
}  
  
// Retrieve and display the data.   
//   
if ( sqlsrv_fetch($stmt2) ) {  
   echo "Comments: ";  
   $data = sqlsrv_get_field( $stmt2, 0 );  
   echo $data."<br>";  
}  
else {  
   echo "Error in fetching data.<br>";  
   die( print_r( sqlsrv_errors(), true));  
}  
  
// Free statement and connection resources.  
//   
sqlsrv_free_stmt( $stmt1 );  
sqlsrv_free_stmt( $stmt2 );  
sqlsrv_close( $conn);  
?>  
```  
  
Informationen zum Speichern von Unicode-Daten finden Sie unter [Arbeiten mit Unicode-Daten](/previous-versions/sql/sql-server-2008-r2/ms175180(v=sql.105)).  
  
## <a name="example"></a>Beispiel  
Das folgende Beispiel ähnelt dem ersten Beispiel, aber statt der Festlegung des UTF-8-Zeichensatzes für die Verbindung, veranschaulicht dieses Beispiel die Festlegung des UTF-8-Zeichensatzes für die Spalte.  
  
```  
<?php  
  
// Connect to the local server using Windows Authentication and  
// specify the AdventureWorks database as the database in use.   
//   
$serverName = "MyServer";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
  
if ( $conn === false ) {  
   echo "Could not connect.<br>";  
   die( print_r( sqlsrv_errors(), true));  
}  
  
// Set up the Transact-SQL query.  
//   
$tsql1 = "UPDATE Production.ProductReview  
          SET Comments = ?  
          WHERE ProductReviewID = ?";  
  
// Set the parameter values and put them in an array. Note that  
// $comments is converted to UTF-8 encoding with the PHP function  
// utf8_encode to simulate an application that uses UTF-8 encoded data.   
//   
$reviewID = 3;  
$comments = utf8_encode("testing");  
$params1 = array(  
                  array($comments,  
                        SQLSRV_PARAM_IN,  
                        SQLSRV_PHPTYPE_STRING('UTF-8')  
                  ),  
                  array($reviewID)  
                );  
  
// Execute the query.  
//   
$stmt1 = sqlsrv_query($conn, $tsql1, $params1);  
  
if ( $stmt1 === false ) {  
   echo "Error in statement execution.<br>";  
   die( print_r( sqlsrv_errors(), true));  
}  
else {  
   echo "The update was successfully executed.<br>";  
}  
  
// Retrieve the newly updated data.  
//   
$tsql2 = "SELECT Comments   
          FROM Production.ProductReview   
          WHERE ProductReviewID = ?";  
  
// Set up the parameter array.  
//   
$params2 = array($reviewID);  
  
// Execute the query.  
//   
$stmt2 = sqlsrv_query($conn, $tsql2, $params2);  
if ( $stmt2 === false ) {  
   echo "Error in statement execution.<br>";  
   die( print_r( sqlsrv_errors(), true));  
}  
  
// Retrieve and display the data.   
//   
if ( sqlsrv_fetch($stmt2) ) {  
   echo "Comments: ";  
   $data = sqlsrv_get_field($stmt2,   
                            0,   
                            SQLSRV_PHPTYPE_STRING('UTF-8')  
                           );  
   echo $data."<br>";  
}  
else {  
   echo "Error in fetching data.<br>";  
   die( print_r( sqlsrv_errors(), true));  
}  
  
// Free statement and connection resources.  
//   
sqlsrv_free_stmt( $stmt1 );  
sqlsrv_free_stmt( $stmt2 );  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[Abrufen von Daten](../../connect/php/retrieving-data.md)

[Senden und Abrufen von ASCII-Daten in Linux und macOS](../../connect/php/how-to-send-and-retrieve-ascii-data-in-linux-mac.md)

[Aktualisieren von Daten &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/updating-data-microsoft-drivers-for-php-for-sql-server.md)

[API-Referenz für den SQLSRV-Treiber](../../connect/php/sqlsrv-driver-api-reference.md)

[Konstanten &#40;Microsoft-Treiber für PHP für SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)

[Beispielanwendung &#40;SQLSRV-Treiber&#41;](../../connect/php/example-application-sqlsrv-driver.md)  
