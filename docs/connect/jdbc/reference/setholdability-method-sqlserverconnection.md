---
description: setHoldability-Methode (SQLServerConnection)
title: setHoldability-Methode (SQLServerConnection) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerConnection.setHoldability
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 552eebd0-4c38-43f0-961f-35244f99109b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7111094bf78e1fa1408ce163d764e52f30274c33
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88431792"
---
# <a name="setholdability-method-sqlserverconnection"></a>setHoldability-Methode (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ändert die Haltbarkeit von [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekten, die unter Verwendung dieses [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md)-Objekts erstellt werden, zur angegebenen Haltbarkeit.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public void setHoldability(int nNewHold)  
```  
  
#### <a name="parameters"></a>Parameter  
 *nNewHold*  
  
 Ein Wert vom Typ **int** mit einer der folgenden Haltbarkeitsstufen:  
  
 HOLD_CURSORS_OVER_COMMIT  
  
 CLOSE_CURSORS_AT_COMMIT  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese setHoldability-Methode wird von der setHoldability-Methode in der java.sql.Connection-Schnittstelle angegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerConnection-Elemente](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [SQLServerConnection-Klasse](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
