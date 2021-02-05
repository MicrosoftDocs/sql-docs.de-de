---
description: getXAConnection-Methode (java.lang.String, java.lang.String)
title: getXAConnection-Methode (java.lang.String, java.lang.String) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerXADataSource.getXAConnection (java.lang.String, java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 276e0093-3d42-4f73-acc4-2b5b98245b40
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c5c4e04d12c1646d2c5984521f26997594d25432
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177583"
---
# <a name="getxaconnection-method-javalangstring-javalangstring"></a>getXAConnection-Methode (java.lang.String, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Stellt eine physikalische Datenbankverbindung anhand des angegebenen Benutzernamens und -kennworts her.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public javax.sql.XAConnection getXAConnection(java.lang.String user,  
                                              java.lang.String password)  
```  
  
#### <a name="parameters"></a>Parameter  
 *user*  
  
 Ein **String-Objekt**, das den Benutzernamen enthält.  
  
 *password*  
  
 Eine **Zeichenfolge**, die das Kennwort enthält.  
  
## <a name="return-value"></a>Rückgabewert  
 Ein XAConnection-Objekt  
  
## <a name="exceptions"></a>Ausnahmen  
 java.sql.SQLException  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getXAConnection-Methode wird von der getXAConnection-Methode in der javax.sql.XADataSource-Schnittstelle angegeben.  
  
> [!NOTE]  
>  Diese Methode wird normalerweise von XA-Verbindungspoolimplementierungen und nicht vom regulären JDBC-Anwendungscode aufgerufen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [getXAConnection-Methode &#40;SQLServerXADataSource&#41;](../../../connect/jdbc/reference/getxaconnection-method-sqlserverxadatasource.md)   
 [SQLServerXADataSource-Methoden](../../../connect/jdbc/reference/sqlserverxadatasource-methods.md)   
 [SQLServerXADataSource-Elemente](../../../connect/jdbc/reference/sqlserverxadatasource-members.md)   
 [SQLServerXADataSource-Klasse](../../../connect/jdbc/reference/sqlserverxadatasource-class.md)  
  
  
