---
description: getSubString-Methode (SQLServerClob)
title: getSubString-Methode (SQLServerClob) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerClob.getSubString
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bf915590-a883-4403-befa-5b5bb42f34d8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f08b8515dd827b2202d3e373242bb292c5060f72
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162192"
---
# <a name="getsubstring-method-sqlserverclob"></a>getSubString-Methode (SQLServerClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Gibt eine Kopie der angegebenen Teilzeichenfolge im CLOB auf der Grundlage der angegebenen Startposition und der Anzahl der zu kopierenden Zeichen zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public java.lang.String getSubString(long pos,  
                                     int length)  
```  
  
#### <a name="parameters"></a>Parameter  
 *pos*  
  
 Das erste Zeichen der zu extrahierenden Teilzeichenfolge. Das erste Zeichen befindet sich an Position 1.  
  
 *length*  
  
 Die Anzahl der aufeinanderfolgenden und zu kopierenden Zeichen.  
  
## <a name="return-value"></a>Rückgabewert  
 Eine **Zeichenfolge**, bei der es sich um die angegebene Teilzeichenfolge im CLOB handelt.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getSubString-Methode wird von der getSubString-Methode in der java.sql.Clob-Schnittstelle angegeben.  
  
 Beim Versuch, null Zeichen aus einem leeren CLOB oder aus einem CLOB mit der Länge Null abzurufen, wird eine leere Zeichenfolge zurückgegeben. Beim Versuch, aus einem CLOB mit der Länge Null eine beliebige Zeichenlänge an einer beliebigen Position (und nicht von Position 1) abzurufen, wird eine Positionsausnahme ausgelöst.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerClob-Methoden](../../../connect/jdbc/reference/sqlserverclob-methods.md)   
 [SQLServerClob-Elemente](../../../connect/jdbc/reference/sqlserverclob-members.md)   
 [SQLServerClob-Klasse](../../../connect/jdbc/reference/sqlserverclob-class.md)  
  
  
