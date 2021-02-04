---
description: getBytes-Methode (SQLServerBlob)
title: getBytes-Methode (SQLServerBlob) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerBlob.getBytes
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bea1b810-b5c1-466d-bdc4-561468214632
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 32fd57b6be09a09036d3bd29c5e22ee9934e7ab8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168107"
---
# <a name="getbytes-method-sqlserverblob"></a>getBytes-Methode (SQLServerBlob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft die BLOB-Daten als Bytearray ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public byte[] getBytes(long pos,  
                       int length)  
```  
  
#### <a name="parameters"></a>Parameter  
 *pos*  
  
 Die Startposition, beginnend bei "1" (nicht "0").  
  
 *length*  
  
 Die Länge der abzurufenden Daten.  
  
## <a name="return-value"></a>Rückgabewert  
 Ein **byte**-Array mit den angeforderten Daten.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getBytes-Methode wird von der getBytes-Methode in der java.sql.Blob-Schnittstelle angegeben.  
  
 Bei einem BLOB, der NULL ist oder die Länge 0 besitzt, wird ein leeres **byte**-Array zurückgegeben (ein Bytearray der Länge „0“), wenn Sie versuchen, genau 0 Bytes an Position „1“ abzurufen.  
  
 Bei einem BLOB mit der Länge Null wird beim Versuch, eine beliebige Länge in Bytes an einer anderen Position als "1" abzurufen, eine Positionsausnahme ausgelöst.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerBlob-Methoden](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [SQLServerBlob-Elemente](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [SQLServerBlob-Klasse](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
