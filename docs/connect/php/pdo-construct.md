---
title: PDO::__construct
description: API-Referenz für die PDO::__construct-Funktion im Microsoft PDO_SQLSRV-Treiber für PHP für SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 3ee53aff-6fe4-44cd-a15b-51770c98c712
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9a815f72ef466442c601d0720243f9476a0cfc95
ms.sourcegitcommit: 7eb80038c86acfef1d8e7bfd5f4e30e94aed3a75
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92081669"
---
# <a name="pdo__construct"></a>PDO::__construct
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Stellt eine Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank her.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
PDO::__construct($dsn [,$username [,$password [,$driver_options ]]] )  
```  
  
#### <a name="parameters"></a>Parameter  
*$dsn*: Dies ist eine Zeichenfolge mit dem Präfixnamen (immer `sqlsrv`), einem Doppelpunkt und dem Server-Schlüsselwort. Beispiel: `"sqlsrv:server=(local)"`. Sie können optional auch andere Schlüsselwörter angeben. Unter [Connection Options](../../connect/php/connection-options.md) finden Sie eine Beschreibung des Server-Schlüsselworts und anderer Verbindungsschlüsselwörter. Die gesamte *$dsn* wird in Anführungszeichen gesetzt, daher sollten die einzelnen Verbindungsschlüsselwörter nicht in separate Anführungszeichen gesetzt werden.  
  
*$username*: Optional. Eine Zeichenfolge, die den Namen des Benutzers enthält. Um eine Verbindung mithilfe der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung herzustellen, geben Sie die Anmelde-ID an. Um eine Verbindung mithilfe der Windows-Authentifizierung herzustellen, geben Sie `""`an.  
  
*$password*: Optional. Eine Zeichenfolge, die das Kennwort des Benutzers enthält. Um eine Verbindung mithilfe der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung herzustellen, geben Sie das Kennwort an. Um eine Verbindung mithilfe der Windows-Authentifizierung herzustellen, geben Sie `""`an.  
  
*$driver_options*: Optional. Sie können Attribute des PDO-Treiber-Managers und bestimmte Attribute des [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]-Treibers angeben, z.B. PDO::SQLSRV_ATTR_ENCODING, PDO::SQLSRV_ATTR_DIRECT_QUERY. Ein ungültiges Attribut generiert keine Ausnahme. Ungültige Attribute generieren Ausnahmen, wenn sie mit [PDO::setAttribute](../../connect/php/pdo-setattribute.md)spezifiziert werden.  
  
## <a name="return-value"></a>Rückgabewert  
Gibt ein PDO-Objekt zurück. Wenn der Fehler auftritt, wird ein PDOException-Objekt zurückgegeben.  
  
## <a name="exceptions"></a>Ausnahmen  
PDOException  
  
## <a name="remarks"></a>Bemerkungen  
Sie können ein Verbindungsobjekt schließen, indem Sie die Instanz auf NULL setzen.  
  
Nachdem eine Verbindung hergestellt wurde, wird PDO::errorCode 01000 anstelle von 00000 angezeigt.  
  
Wenn PDO::__construct aus irgendeinem Grund fehlschlägt, wird eine Ausnahme ausgelöst, selbst wenn PDO::ATTR_ERRMODE auf PDO::ERRMODE_SILENT festgelegt ist.  
  
Unterstützung für PDO wurde in Version 2.0 von [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]hinzugefügt.  
  
## <a name="example-with-database"></a>Beispiel mit Datenbank  
Dieses Beispiel zeigt, wie Sie eine Verbindung mit einem Server mithilfe der Windows-Authentifizierung herstellen und eine Datenbank angeben.  
  
```  
<?php  
   $c = new PDO( "sqlsrv:Server=(local) ; Database = AdventureWorks ", "", "", array(PDO::SQLSRV_ATTR_DIRECT_QUERY => true));   
  
   $query = 'SELECT * FROM Person.ContactType';   
   $stmt = $c->query( $query );   
   while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ) {   
      print_r( $row );   
   }  
   $c = null;   
?>  
```  
  
## <a name="example-without-database"></a>Beispiel ohne Datenbank  
Dieses Beispiel zeigt, wie Sie eine Verbindung mit einem Server herstellen und später die Datenbank festlegen.  
  
```  
<?php  
   $c = new PDO( "sqlsrv:server=(local)");  
  
   $c->exec( "USE AdventureWorks");  
   $query = 'SELECT * FROM Person.ContactType';  
   $stmt = $c->query( $query );  
   while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){  
      print_r( $row );  
   }  
   $c = null;  
?>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[PDO-Klasse](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
