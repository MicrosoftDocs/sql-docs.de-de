---
description: getNCharacterStream-Methode (java.lang.String) (SQLServerResultSet)
title: getNCharacterStream-Methode (java.lang.String) (SQLServerResultSet) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a117f3a3-9c25-41e1-9adb-a40e90620dd6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5db2c9a81865779fa00d3800aed64d1d7666b49f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88435342"
---
# <a name="getncharacterstream-method-javalangstring-sqlserverresultset"></a>getNCharacterStream-Methode (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft den Wert der angegebenen Spalte in der aktuellen Zeile dieses [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekts als Reader-Objekt ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public java.io.Reader getNCharacterStream(java.lang.String columnLabel)  
```  
  
#### <a name="parameters"></a>Parameter  
 *columnLabel*  
  
 Eine Zeichenfolge, die die Spaltenbezeichnung enthält.  
  
## <a name="return-value"></a>Rückgabewert  
 Ein Reader-Objekt  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getNCharacterStream-Methode wird von der getNCharacterStream-Methode in der java.sql.ResultSet-Schnittstelle angegeben.  
  
 Mit dieser Methode kann der Wert einer Spalte vom Typ **nvarchar**, **nchar**, **nvarchar(max)**, **ntext** oder **xml** in der aktuellen Zeile dieses [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekts abgerufen werden. Beim Versuch, mit dieser Methode Werte anderer Datentypen abzurufen, wird eine Ausnahme ausgelöst.  
  
## <a name="see-also"></a>Weitere Informationen  
 [getNCharacterStream-Methode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getncharacterstream-method-sqlserverresultset.md)   
 [SQLServerResultSet-Elemente](../../../connect/jdbc/reference/sqlserverresultset-members.md)  
  
  
