---
description: getMaxUserNameLength-Methode (SQLServerDatabaseMetaData)
title: getMaxUserNameLength-Methode (SQLServerDatabaseMetaData) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerDatabaseMetaData.getMaxUserNameLength
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 09ec7d40-4c4a-4d89-ba11-78e5327b5759
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: efa08594db2e634d500f78b16e25952f072332e3
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88435462"
---
# <a name="getmaxusernamelength-method-sqlserverdatabasemetadata"></a>getMaxUserNameLength-Methode (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft die maximale Anzahl von Zeichen ab, die bei dieser Datenbank für einen Benutzernamen zulässig sind.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public int getMaxUserNameLength()  
```  
  
## <a name="return-value"></a>Rückgabewert  
 Ein Wert vom Typ **int** zum Angeben der maximalen Anzahl der zulässigen Zeichen.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getMaxUserNameLength-Methode wird von der getMaxUserNameLength-Methode in der java.sql.DatabaseMetaData-Schnittstelle angegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerDatabaseMetaData-Methoden](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData-Elemente](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData-Klasse](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
