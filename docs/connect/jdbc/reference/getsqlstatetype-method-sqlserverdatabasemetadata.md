---
description: getSQLStateType-Methode (SQLServerDatabaseMetaData)
title: getSQLStateType-Methode (SQLServerDatabaseMetaData) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getSQLStateType
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ee4d6751-68a3-4d04-831c-e6d704c59e63
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 33b0553b18dc09306e7d84effa30b81b20b84768
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162273"
---
# <a name="getsqlstatetype-method-sqlserverdatabasemetadata"></a>getSQLStateType-Methode (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Gibt an, ob "SQLSTATE" (zurückgegeben von der SQLException.getSQLState-Methode) den Wert "X/Open" (jetzt "Open Group"), "SQL CLI", "SQL99" (JDBC 3.0) oder "SQL:2003" (JDBC 4.0) besitzt.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public int getSQLStateType()  
```  
  
## <a name="return-value"></a>Rückgabewert  
 Ein Wert vom Typ **int** zum Angeben des SQLSTATE-Typs. Mögliche Werte:  
  
-   Java Runtime Environment-Version 5.0 (JRE): Wenn die Verbindungseigenschaft **xopenstates** auf **TRUE** festgelegt ist, gibt diese Methode DatabaseMetaData.sqlStateXOpen zurück. Andernfalls DatabaseMetaData.sqlStateSQL99.  
  
-   Java Runtime Environment-Version 6.0 (JRE): Wenn die Verbindungseigenschaft **xopenstates** auf **TRUE** festgelegt ist, gibt diese Methode DatabaseMetaData.sqlStateXOpen zurück. Andernfalls DatabaseMetaData.sqlStateSQL.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getSQLStateType-Methode wird von der getSQLStateType-Methode in der java.sql.DatabaseMetaData-Schnittstelle angegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerDatabaseMetaData-Methoden](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData-Elemente](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData-Klasse](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
