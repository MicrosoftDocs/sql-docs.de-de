---
description: getString-Methode (java.lang.String) (SQLServerResultSet)
title: getString-Methode (java.lang.String) (SQLServerResultSet) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerResultSet.getString (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8a98c8a8-61d0-40c9-9335-25a87b732dc3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2abe464ba8c800dff256ebe2dfcec0d6e787c5dd
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88434352"
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
  
  
