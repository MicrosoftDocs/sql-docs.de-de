---
title: 'Direkte Anweisung: Ausführung vorbereiteter Anweisungen im PDO_SQLSRV-Treiber'
description: Erfahren Sie, wie Sie das PDO::SQLSRV_ATTR_DIRECT_QUERY-Attribut für die direkte Ausführung von Anweisungen verwenden, wenn Sie den Microsoft PDO_SQLSRV-Treiber für PHP für SQL Server einsetzen.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 05544ca6-1e07-486c-bf03-e8c2c25b3024
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4dc9177ed83e349b4cdd3d38da34285ee90a1f1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045390"
---
# <a name="direct-statement-execution-and-prepared-statement-execution-in-the-pdo_sqlsrv-driver"></a>Direkte Anweisungsausführung und vorbereitete Anweisungsausführung im PDO_SQLSRV-Treiber.
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

In diesem Artikel wird erläutert, wie das PDO::SQLSRV_ATTR_DIRECT_QUERY-Attribut verwendet wird, um die direkte Anweisungsausführung anstelle des Standardwerts, der die Ausführung der vorbereiteten Anweisung darstellt, anzugeben. Die Verwendung einer vorbereiteten Anweisung kann zu einer besseren Leistung führen, wenn die Anweisung mithilfe einer Parameterbindung mehrmals ausgeführt wird.  
  
## <a name="remarks"></a>Bemerkungen  
Wenn Sie eine [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung ohne Vorbereitung durch den Treiber direkt an den Server senden möchten, können Sie das PDO::SQLSRV_ATTR_DIRECT_QUERY-Attribut mit [PDO::setAttribute](../../connect/php/pdo-setattribute.md) (oder als Treiberoption, die an [PDO::__construct](../../connect/php/pdo-construct.md) übergeben wird) oder beim Aufruf von [PDO::prepare](../../connect/php/pdo-prepare.md) festlegen. Standardmäßig weist PDO::SQLSRV_ATTR_DIRECT_QUERY den Wert „false“ auf (Ausführung vorbereiteter Anweisungen verwenden).  
  
Wenn Sie [PDO::query](../../connect/php/pdo-query.md) verwenden, ist möglicherweise eine direkte Ausführung wünschenswert. Bevor Sie [PDO::query](../../connect/php/pdo-query.md) aufrufen, rufen Sie [PDO::setAttribute](../../connect/php/pdo-setattribute.md) auf, und legen Sie PDO::SQLSRV_ATTR_DIRECT_QUERY auf „true“ fest.  Jeder Aufruf von [PDO::query](../../connect/php/pdo-query.md) kann mit einer anderen Einstellung für PDO::SQLSRV_ATTR_DIRECT_QUERY ausgeführt werden.  
  
Wenn Sie [PDO::prepare](../../connect/php/pdo-prepare.md) und [PDOStatement::execute](../../connect/php/pdostatement-execute.md) verwenden, um eine Abfrage mithilfe von Bindungsparametern mehrmals auszuführen, optimieren vorbereitete Anweisungen die Ausführung der wiederholten Abfrage.  Rufen Sie in diesem Fall [PDO::prepare](../../connect/php/pdo-prepare.md) auf – hierbei muss PDO::SQLSRV_ATTR_DIRECT_QUERY im Parameter für das Treiberoptionsarray auf „false“ festgelegt sein. Wenn nötig, können Sie vorbereitete Anweisungen mit auf „false“ festgelegtem Attribut PDO::SQLSRV_ATTR_DIRECT_QUERY ausführen.  
  
Nach dem Aufruf von [PDO::prepare](../../connect/php/pdo-prepare.md) kann der Wert von PDO::SQLSRV_ATTR_DIRECT_QUERY beim Ausführen der vorbereiteten Abfrage nicht geändert werden.  
  
Wenn eine Abfrage den in einer vorherigen Abfrage festgelegten Kontext benötigt, führen Sie Ihre Abfragen mit auf „true“ festgelegtem Attribut PDO::SQLSRV_ATTR_DIRECT_QUERY aus. Wenn Sie beispielsweise temporäre Tabellen in Ihren Abfragen verwenden, muss PDO::SQLSRV_ATTR_DIRECT_QUERY auf „true“ festgelegt sein.  
  
Das folgende Beispiel zeigt, dass PDO::SQLSRV_ATTR_DIRECT_QUERY auf „true“ festgelegt werden muss, wenn Kontext aus einer vorherigen Abfrage erforderlich ist. Dieses Beispiel verwendet temporäre Tabellen, die für nachfolgende Anweisungen in Ihrem Programm nur verfügbar sind, wenn Abfragen direkt ausgeführt werden.  
  
> [!NOTE]
> Wenn die Abfrage darin besteht, eine gespeicherte Prozedur aufrufen, und in dieser gespeicherten Prozedur temporäre Tabellen verwendet werden, verwenden Sie stattdessen [PDO::exec](../../connect/php/pdo-exec.md).

```  
<?php  
   $conn = new PDO('sqlsrv:Server=(local)', '', '');  
   $conn->setAttribute(constant('PDO::SQLSRV_ATTR_DIRECT_QUERY'), true);  
  
   $stmt1 = $conn->query("DROP TABLE #php_test_table");  
  
   $stmt2 = $conn->query("CREATE TABLE #php_test_table ([c1_int] int, [c2_int] int)");  
  
   $v1 = 1;  
   $v2 = 2;  
  
   $stmt3 = $conn->prepare("INSERT INTO #php_test_table (c1_int, c2_int) VALUES (:var1, :var2)");  
  
   if ($stmt3) {  
      $stmt3->bindValue(1, $v1);  
      $stmt3->bindValue(2, $v2);  
  
      if ($stmt3->execute())  
         echo "Execution succeeded\n";       
      else  
         echo "Execution failed\n";  
   }  
   else  
      var_dump($conn->errorInfo());  
  
   $stmt4 = $conn->query("DROP TABLE #php_test_table");  
?>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[Programmierhandbuch für die Microsoft-Treiber für PHP für SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)
  
