---
description: getTimestamp-Methode (java.lang.String) (SQLServerResultSet)
title: getTimestamp-Methode (java.lang.String) (SQLServerResultSet) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerResultSet.getTimestamp (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8b3c3938-e057-4919-9e9f-01eb8a4ad937
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cd7357be271e3a2af58fd8b3dbe5d654930d22ed
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88434062"
---
# <a name="gettimestamp-method-javalangstring-sqlserverresultset"></a>getTimestamp-Methode (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft den Wert des angegebenen Spaltennamens in der aktuellen Zeile dieses [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekts als java.sql.Timestamp-Objekt in der Programmiersprache Java ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public java.sql.Timestamp getTimestamp(java.lang.String columnName)  
```  
  
#### <a name="parameters"></a>Parameter  
 *columnName*  
  
 Eine **Zeichenfolge**, die den Spaltennamen enthält.  
  
## <a name="return-value"></a>Rückgabewert  
 Ein Timestamp-Objekt  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getTimestamp-Methode wird von der getTimestamp-Methode in der java.sql.ResultSet-Schnittstelle angegeben.  
  
 Von dieser Methode werden nur Werte aus datetime- und smalldatetime-Spalten von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] zurückgegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [getTimestamp-Methode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/gettimestamp-method-sqlserverresultset.md)   
 [SQLServerResultSet-Elemente](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet-Klasse](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
