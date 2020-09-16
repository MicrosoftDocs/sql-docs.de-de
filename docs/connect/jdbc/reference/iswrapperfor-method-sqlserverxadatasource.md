---
description: isWrapperFor-Methode (SQLServerXADataSource)
title: isWrapperFor-Methode (SQLServerXADataSource) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: d612461d-4c3f-46db-b968-ff4c80b2aa7c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: caab887a8e104313f78a593520ea7d735008fd1d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88433292"
---
# <a name="iswrapperfor-method-sqlserverxadatasource"></a>isWrapperFor-Methode (SQLServerXADataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Gibt an, ob es sich bei diesem Objekt um einen Wrapper für die angegebene Schnittstelle handelt.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public boolean isWrapperFor(Class iface)  
```  
  
#### <a name="parameters"></a>Parameter  
 *iface*  
  
 Dies ist eine **Klasse** zum Definieren einer Schnittstelle.  
  
## <a name="return-value"></a>Rückgabewert  
 Der Wert lautet **TRUE**, wenn dieses Objekt die Schnittstelle implementiert oder ein Objekt umschließt, das die Schnittstelle implementiert. Andernfalls lautet der Wert **false**.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Die [isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverxadatasource.md)-Methode und die [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverxadatasource.md)-Methode werden von der java.sql.Wrapper-Schnittstelle definiert, die in den JDBC 4.0-Spezifikationen eingeführt wird.  
  
 Wird von dieser Methode „true“ zurückgegeben, kann [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverxadatasource.md) erfolgreich mit dem gleichen Argument aufgerufen werden.  
  
 Weitere Informationen finden Sie unter [Wrapper und Schnittstellen](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [unwrap-Methode &#40;SQLServerXADataSource&#41;](../../../connect/jdbc/reference/unwrap-method-sqlserverxadatasource.md)   
 [SQLServerXADataSource-Methoden](../../../connect/jdbc/reference/sqlserverxadatasource-methods.md)   
 [SQLServerXADataSource-Elemente](../../../connect/jdbc/reference/sqlserverxadatasource-members.md)   
 [SQLServerXADataSource-Klasse](../../../connect/jdbc/reference/sqlserverxadatasource-class.md)  
  
  
