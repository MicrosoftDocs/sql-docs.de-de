---
title: supportsANSI92IntermediateSQL-Methode (SQLServerDatabaseMetaData) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsANSI92IntermediateSQL
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 4d6e8301-0633-4565-91c6-a80910954461
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: de0bec278a0f8715cdce092afe5da174e00bceff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160963"
---
# <a name="supportsansi92intermediatesql-method-sqlserverdatabasemetadata"></a>supportsANSI92IntermediateSQL-Methode (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft ab, ob von dieser Datenbank die ANSI92-SQL-Zwischengrammatik unterstützt wird.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public boolean supportsANSI92IntermediateSQL()  
```  
  
## <a name="return-value"></a>Rückgabewert  
 **"true"** unterstützt. Andernfalls lautet der Wert **false**.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese supportsANSI92IntermediateSQL-Methode wird von der supportsANSI92IntermediateSQL-Methode in der java.sql.DatabaseMetaData-Schnittstelle angegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerDatabaseMetaData-Methoden](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData-Elemente](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData-Klasse](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
