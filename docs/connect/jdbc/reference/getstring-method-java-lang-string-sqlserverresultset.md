---
description: getString-Methode (java.lang.String) (SQLServerResultSet)
title: getString-Methode (java.lang.String) (SQLServerResultSet) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getString (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8a98c8a8-61d0-40c9-9335-25a87b732dc3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3abe4f260c8e2fffb8fa32460d9e233ea65cdfbf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174938"
---
# <a name="getstring-method-javalangstring-sqlserverresultset"></a>getString-Methode (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft den Wert des angegebenen Spaltenamens in der aktuellen Zeile des [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekts als **String**-Objekt in der Programmiersprache Java ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public java.lang.String getString(java.lang.String columnName)  
```  
  
#### <a name="parameters"></a>Parameter  
 *columnName*  
  
 Eine **Zeichenfolge**, die den Spaltennamen enthält.  
  
## <a name="return-value"></a>Rückgabewert  
 Ein **String-Wert**.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getString-Methode wird von der getString-Methode in der java.sql.ResultSet-Schnittstelle angegeben.  
  
 Alle Spalten in SQL Server können als Zeichenfolge zurückgegeben werden. D.h., es kann eine **String-Darstellung** aller zahlen- und zeichenfolgenbasierter Typen und eine hexadezimale String-Darstellung binärer Spalten wie binary, varbinary, varbinary(max), image, timestamp oder uniqueidentifier zurückgegeben werden.  
  
 Den Speicherort berücksichtigende Typen wie "money", "smallmoney", "datetime", "smalldatetime", "float", "real", "decimal" oder "numeric" geben für den zugrundeliegenden Wert des Typs das kanonische toString()-Format zurück.  
  
 Benutzerdefinierte Typen werden als hexadezimale **Zeichenfolgenwerte** zurückgegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [getString-Methode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getstring-method-sqlserverresultset.md)   
 [SQLServerResultSet-Elemente](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet-Klasse](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
