---
description: getSQLXML-Methode (int) (SQLServerResultSet)
title: getSQLXML-Methode (int) (SQLServerResultSet) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: faa35676-573d-48d5-afd9-850134735728
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1657d552aa2fa54a1b06d06af069878852499044
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174993"
---
# <a name="getsqlxml-method-int-sqlserverresultset"></a>getSQLXML-Methode (int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft den Wert einer angegebenen Spalte in der aktuellen Zeile dieses [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekts als SQLXML-Objekt ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public final java.sql.SQLXML getSQLXML(int columnIndex)  
```  
  
#### <a name="parameters"></a>Parameter  
 *columnIndex*  
  
 Ein **ganzzahliger** Wert, der den Spaltenindex angibt.  
  
## <a name="return-value"></a>Rückgabewert  
 ASQLXMLobject.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getSQLXML-Methode wird von der getSQLXML-Methode in der java.sql.ResultSet-Schnittstelle angegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [getSQLXML-Methode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getsqlxml-method-sqlserverresultset.md)   
 [SQLServerResultSet-Elemente](../../../connect/jdbc/reference/sqlserverresultset-members.md)  
  
  
