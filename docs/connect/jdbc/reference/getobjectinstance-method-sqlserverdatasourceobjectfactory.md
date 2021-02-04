---
description: getObjectInstance-Methode (SQLServerDataSourceObjectFactory)
title: getObjectInstance-Methode (SQLServerDataSourceObjectFactory) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSourceObjectFactory.getObjectInstance
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 0a1503e2-e991-4d70-a223-087fc63baf73
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 015c197f80839a175697c5db8fbaf5b6eb42edf6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175295"
---
# <a name="getobjectinstance-method-sqlserverdatasourceobjectfactory"></a>getObjectInstance-Methode (SQLServerDataSourceObjectFactory)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft eine Instanz des angegebenen Datenquellenobjekts ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public java.lang.Object getObjectInstance(java.lang.Object ref,  
                                          javax.naming.Name name,  
                                          javax.naming.Context c,  
                                          java.util.Hashtable h)  
```  
  
#### <a name="parameters"></a>Parameter  
 *ref*  
  
 Ein **Object**-Wert.  
  
 *name*  
  
 Der Name des Objekts.  
  
 *c*  
  
 Der Kontext relativ zum angegebenen Namen.  
  
 *h*  
  
 Die Umgebung, die beim Erstellen des Objekts verwendet wird.  
  
## <a name="return-value"></a>Rückgabewert  
 Ein **Object**-Wert.  
  
## <a name="exceptions"></a>Ausnahmen  
 java.sql.SQLException  
  
## <a name="remarks"></a>Bemerkungen  
 Die getObjectInstance-Methode wird von der getObjectInstance-Methode in der javax.naming.spi.ObjectFactory-Schnittstelle angegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerDataSourceObjectFactory-Methoden](../../../connect/jdbc/reference/sqlserverdatasourceobjectfactory-methods.md)   
 [SQLServerDataSourceObjectFactory-Elemente](../../../connect/jdbc/reference/sqlserverdatasourceobjectfactory-members.md)   
 [SQLServerDataSourceObjectFactory-Klasse](../../../connect/jdbc/reference/sqlserverdatasourceobjectfactory-class.md)  
  
  
