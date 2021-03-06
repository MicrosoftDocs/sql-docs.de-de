---
title: PDOStatement::rowCount
description: API-Referenz für die PDOStatement::rowCount-Funktion im Microsoft PDO_SQLSRV-Treiber für PHP für SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 0569f26a-2376-4c20-8813-bd3c87d0ae9f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 765a3af1800e7a3a3a834130659637322a51e374
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179901"
---
# <a name="pdostatementrowcount"></a>PDOStatement::rowCount
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Gibt die Anzahl der Zeilen zurück, die durch die letzte Anweisung hinzugefügt, gelöscht oder geändert wurden  
  
## <a name="syntax"></a>Syntax  
  
```  
  
int PDOStatement::rowCount ();  
```  
  
## <a name="return-value"></a>Rückgabewert  
Die Anzahl der hinzugefügten, gelöschten oder geänderten Zeilen.  
  
## <a name="remarks"></a>Bemerkungen  
Falls die letzte SQL-Anweisung, die durch die zugeordnete PDO-Anweisung ausgeführt wurde, eine SELECT-Anweisung war, gibt ein PDO::CURSOR_FWDONLY-Cursor den Wert -1 zurück. A PDO::CURSOR_SCROLLABLE -Cursor gibt die Anzahl der Zeilen in dem Resultset zurück.  
  
Unterstützung für PDO wurde in Version 2.0 von [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]hinzugefügt.  
  
## <a name="example"></a>Beispiel  
Dieses Beispiel zeigt zwei Verwendungen von „rowCount“. Die erste gibt die Anzahl der Zeilen zurück, die der Tabelle hinzugefügt wurden. Die zweite Verwendung zeigt, dass „rowCount“ die Anzahl der Zeilen in einem Resultset ausgeben kann, wenn Sie einen bildlauffähigen Cursor spezifizieren.  
  
```  
<?php  
$database = "Test";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$col1 = 'a';  
$col2 = 'b';  
  
$query = "insert into Table2(col1, col2) values(?, ?)";  
$stmt = $conn->prepare( $query );  
$stmt->execute( array( $col1, $col2 ) );  
print $stmt->rowCount();  
  
echo "\n\n";  
  
$con = null;  
$database = "AdventureWorks";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$query = "select * from Person.ContactType";  
$stmt = $conn->prepare( $query, array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL));  
$stmt->execute();  
print $stmt->rowCount();  
?>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[PDOStatement-Klasse](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
