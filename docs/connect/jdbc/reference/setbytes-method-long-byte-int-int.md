---
description: setBytes-Methode (long, byte, int, int)
title: setBytes(long, byte, int, int)-Methode | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerBlob.setBytes (long.byte[], int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7def226c-b211-459e-8c1a-08592d75d4a4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7210c363f8fefca7022fa018378f5d1bceeaa375
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173777"
---
# <a name="setbytes-method-long-byte-int-int"></a>setBytes-Methode (long, byte, int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Schreibt ab der angegebenen Position, dem Offset und der Länge das gesamte Bytearray oder einen Teil des Bytearrays in das BLOB und gibt anschließend die Anzahl der geschriebenen Bytes zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public int setBytes(long pos,  
                    byte[] bytes,  
                    int offset,  
                    int len)  
```  
  
#### <a name="parameters"></a>Parameter  
 *pos*  
  
 Die Position (1-basiert) im BLOB, ab der Daten geschrieben werden.  
  
 *bytes*  
  
 Das in den BLOB zu schreibende Bytearray.  
  
 *offset*  
  
 Das Offset im Bytearray, ab dem Daten im **byte**-Array gelesen werden sollen.  
  
 *len*  
  
 Die Anzahl von Bytes, die aus dem Bytearray in das BLOB geschrieben werden sollen.  
  
## <a name="return-value"></a>Rückgabewert  
 Ein Element vom Typ **int** mit der Anzahl der geschriebenen Bytes.  
  
## <a name="exceptions"></a>Ausnahmen  
 java.sql.SQLException  
  
## <a name="remarks"></a>Bemerkungen  
 Diese setBytes-Methode wird von der setBytes-Methode in der java.sql.Blob-Schnittstelle angegeben.  
  
 Daten werden beginnend mit der angegebenen Position überschrieben, und sie können die ursprüngliche Länge des BLOB übersteigen. Durch Angeben eines Werts vom Typ Position+1 werden Bytes an die Zeichenfolge angefügt. Durch Weitergeben eines Werts vom Typ Position+2 oder größer (oder null oder weniger) wird ein Positionsfehler ausgelöst. Durch Weitergeben eines **Bytearrays** mit einer Länge von NULL wird NULL zurückgegeben, weil keine Bytes geschrieben wurden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [setBytes-Methode &#40;SQLServerBlob&#41;](../../../connect/jdbc/reference/setbytes-method-sqlserverblob.md)   
 [SQLServerBlob-Methoden](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [SQLServerBlob-Elemente](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [SQLServerBlob-Klasse](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
