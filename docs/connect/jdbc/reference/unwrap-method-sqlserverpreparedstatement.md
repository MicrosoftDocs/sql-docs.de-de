---
description: unwrap-Methode (SQLServerPreparedStatement)
title: unwrap-Methode (SQLServerPreparedStatement) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 8e3ec950-3ac1-4c28-9e97-ddce3bd46578
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a1ac875841272b339738245c6bd7ca09cf757c5f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88458075"
---
# <a name="unwrap-method-sqlserverpreparedstatement"></a>unwrap-Methode (SQLServerPreparedStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Gibt ein Objekt zurück, das die angegebene Schnittstelle implementiert, um den Zugriff auf die [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]-spezifischen Methoden zu ermöglichen.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public <T> T unwrap(Class<T> iface)  
```  
  
#### <a name="parameters"></a>Parameter  
 *iface*  
  
 Eine Klasse vom Typ **T** zum Definieren einer Schnittstelle.  
  
## <a name="return-value"></a>Rückgabewert  
 Ein Objekt, von dem die angegebene Schnittstelle implementiert wird.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Die [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverpreparedstatement.md)-Methode wird von der java.sql.Wrapper-Schnittstelle definiert, die in den JDBC 4.0-Spezifikationen eingeführt wird.  
  
 Von den Anwendungen muss möglicherweise auf [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]-spezifische JDBC-API-Erweiterungen zugegriffen werden. Die unwrap-Methode unterstützt das Entpacken in öffentliche, von diesem Objekt erweiterte Klassen, wenn von den Klassen Herstellererweiterungen verfügbar gemacht werden.  
  
 Beim Aufrufen dieser Methode wird das Objekt in die folgenden Klassen entpackt: [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) und [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).  
  
 Beispielcode finden Sie unter [unwrap-Methode &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md).  
  
 Weitere Informationen finden Sie unter [Wrapper und Schnittstellen](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [isWrapperFor-Methode &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverpreparedstatement.md)   
 [SQLServerPreparedStatement-Elemente](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [SQLServerPreparedStatement-Klasse](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
