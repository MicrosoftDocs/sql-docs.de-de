---
description: getNString-Methode (java.lang.String) (SQLServerResultSet)
title: getNString-Methode (java.lang.String) (SQLServerResultSet) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 546d77e2-723a-42ac-ba3f-fabf2395d376
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cdfa86e289bfe8a35657650aa12e2398edad6a8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162605"
---
# <a name="getnstring-method-javalangstring-sqlserverresultset"></a>getNString-Methode (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft den Wert der angegebenen Spalte in der aktuellen Zeile dieses [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekts als java.lang.String-Objekt ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public java.lang.String getNString(java.lang.String columnLabel)  
```  
  
#### <a name="parameters"></a>Parameter  
 *columnLabel*  
  
 Eine Zeichenfolge, die die Spaltenbezeichnung enthält.  
  
## <a name="return-value"></a>Rückgabewert  
 Ein String-Objekt  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getNString-Methode wird von der getNString-Methode in der java.sql.SQLServerResultSet-Schnittstelle angegeben.  
  
 Mit dieser Methode kann der Wert einer Spalte vom Typ **nvarchar**, **nchar**, **nvarchar(max)**, **ntext** oder **xml** in der aktuellen Zeile dieses [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekts abgerufen werden. Beim Versuch, mit dieser Methode Werte anderer Datentypen abzurufen, wird eine Ausnahme ausgelöst.  
  
## <a name="see-also"></a>Weitere Informationen  
 [getNString-Methode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getnstring-method-sqlserverresultset.md)   
 [SQLServerResultSet-Elemente](../../../connect/jdbc/reference/sqlserverresultset-members.md)  
  
  
