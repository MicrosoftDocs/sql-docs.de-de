---
title: PDO::beginTransaction
description: API-Referenz für die PDO::beginTransaction-Funktion im Microsoft PDO_SQLSRV-Treiber für PHP für SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 4d5db438-9df7-4d22-9907-3ddc63bd2220
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2aff13f864bd22ab9cc2407e7a990c6114a6e108
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202083"
---
# <a name="pdobegintransaction"></a>PDO::beginTransaction
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Schaltet den Autocommit-Modus aus und beginnt eine Transaktion.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
bool PDO::beginTransaction();  
```  
  
## <a name="return-value"></a>Rückgabewert  
„true“, bei erfolgreichem Aufruf der Methode; andernfalls „false“.  
  
## <a name="remarks"></a>Bemerkungen  
Die Transaktion, die mit PDO::beginTransaction gestartet wurde, endet, wenn [PDO::commit](../../connect/php/pdo-commit.md) oder [PDO::rollback](../../connect/php/pdo-rollback.md) aufgerufen wird.  
  
PDO::beginTransaction ist nicht betroffen von dem (und hat keinen Einfluss auf den) Wert von PDO::ATTR_AUTOCOMMIT.  
  
Sie dürfen PDO::beginTransaction nicht aufrufen, solange die vorherige PDO::beginTransaction noch nicht mit PDO::rollback oder PDO::commit beendet wurde.  
  
Die Verbindung kehrt zum Autocommit-Modus zurück, falls diese Methode fehlschlägt.  
  
Unterstützung für PDO wurde in Version 2.0 von [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]hinzugefügt.  
  
## <a name="example"></a>Beispiel  
Im folgenden Beispiel wird eine Datenbank namens „Test“ und eine Tabelle namens „Tabelle1“ verwendet. Es startet eine Transaktion und gibt die Befehle aus, um zwei Zeilen hinzuzufügen und dann eine Zeile zu löschen. Diese Befehle werden an die Datenbank geschickt und die Transaktion wird explizit mit `PDO::commit`beendet.  
  
```  
<?php  
   $conn = new PDO( "sqlsrv:server=(local); Database = Test", "", "");  
   $conn->beginTransaction();  
   $ret = $conn->exec("insert into Table1(col1, col2) values('a', 'b') ");  
   $ret = $conn->exec("insert into Table1(col1, col2) values('a', 'c') ");  
   $ret = $conn->exec("delete from Table1 where col1 = 'a' and col2 = 'b'");  
   $conn->commit();  
   // $conn->rollback();  
   echo $ret;  
?>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[PDO-Klasse](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
