---
description: isWrapperFor-Methode (SQLServerCallableStatement)
title: isWrapperFor-Methode (SQLServerCallableStatement) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 71156863-3588-453e-b5a5-0573b2c1bebf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 06840f9562cf0828d6a1524ee15791333bc98beb
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88433342"
---
# <a name="iswrapperfor-method-sqlservercallablestatement"></a>isWrapperFor-Methode (SQLServerCallableStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Gibt an, ob es sich bei diesem Anweisungsobjekt um einen Wrapper für die angegebene Schnittstelle handelt.  
  
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
 Die [isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlservercallablestatement.md)-Methode und die [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md)-Methode werden von der java.sql.Wrapper-Schnittstelle definiert, die in JDBC 4.0 eingeführt wird.  
  
 Wird von dieser Methode **true** zurückgegeben, kann [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md) erfolgreich mit dem gleichen Argument aufgerufen werden.  
  
 Weitere Informationen finden Sie unter [Wrapper und Schnittstellen](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [unwrap-Methode &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md)   
 [SQLServerCallableStatement-Elemente](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [SQLServerCallableStatement-Klasse](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
