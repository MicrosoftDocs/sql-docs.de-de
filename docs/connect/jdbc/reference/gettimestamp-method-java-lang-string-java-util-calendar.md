---
description: getTimestamp-Methode (java.lang.String, java.util.Calendar)
title: getTimestamp-Methode (java.lang.String, java.util.Calendar) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getTimestamp (java.lang.String,java.util.Calendar)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 770668d9-2e52-4ff0-be2f-ebf78fd41644
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f27425d3f3aa7f85ac9707d56235e157f2901ed3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162060"
---
# <a name="gettimestamp-method-javalangstring-javautilcalendar"></a>getTimestamp-Methode (java.lang.String, java.util.Calendar)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft unter Verwendung eines Calendar-Objekts den Wert des angegebenen Parameters als java.sql.Timestamp-Objekt in Java ab (unter Berücksichtigung des vorhandenen Parameternamens).  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public java.sql.Timestamp getTimestamp(java.lang.String name,  
                                       java.util.Calendar cal)  
```  
  
#### <a name="parameters"></a>Parameter  
 *name*  
  
 Ein **String-Objekt**, das den Parameternamen enthält.  
  
 *cal*  
  
 Ein Calendar-Objekt  
  
## <a name="return-value"></a>Rückgabewert  
 Ein Timestamp-Objekt  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getTimestamp-Methode wird von der getTimestamp-Methode in der java.sql.CallableStatement-Schnittstelle angegeben.  
  
 Diese Methode gibt nur Werte aus den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Spalten **datetime** und **smalldatetime** zurück.  
  
## <a name="see-also"></a>Weitere Informationen  
 [getTimestamp-Methode &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/gettimestamp-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement-Elemente](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement-Klasse](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
