---
description: getBinaryStream-Methode (long, long)
title: Methode „getBinaryStream(long, long)“ | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 30bc8882-04b4-4efd-95e4-7d3a2a8c1d47
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: da3fcf0ab4e97c19458981feea8f7e2af4c5a133
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168232"
---
# <a name="getbinarystream-method-long-long"></a>getBinaryStream-Methode (long, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Gibt unter Verwendung der angegebenen Startposition und Länge ein Eingabedatenstrom-Objekt mit einem BLOB-Teilwert zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public java.io.InputStream getBinaryStream(long pos, long length)  
```  
  
#### <a name="parameters"></a>Parameter  
 *pos*  
  
 Das Offset zum ersten Byte des abzurufenden Teilwerts.  
  
 *length*  
  
 Die Länge in Bytes des abzurufenden Teilwerts.  
  
## <a name="return-value"></a>Rückgabewert  
 Ein Eingabedatenstrom mit den BLOB-Daten.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getBinaryStream-Methode wird von der getBinaryStream-Methode in der java.sql.Blob-Schnittstelle angegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerBlob-Methoden](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [SQLServerBlob-Elemente](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [SQLServerBlob-Klasse](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
