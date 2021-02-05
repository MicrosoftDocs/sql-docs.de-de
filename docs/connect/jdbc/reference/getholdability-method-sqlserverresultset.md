---
description: getHoldability-Methode (SQLServerResultSet)
title: getHoldability-Methode (SQLServerResultSet) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 4508d90f-c3c4-4eac-8001-fb0b93b66734
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 285e5b9547415da7fea26133f017ad07df66d92d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175694"
---
# <a name="getholdability-method-sqlserverresultset"></a>getHoldability-Methode (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft die Haltbarkeit dieses [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekts ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public int getHoldability()  
```  
  
## <a name="return-value"></a>Rückgabewert  
 Ein Wert vom Typ **int** mit einer der folgenden Haltbarkeitsstufen:  
  
 HOLD_CURSORS_OVER_COMMIT  
  
 CLOSE_CURSORS_AT_COMMIT  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getHoldability-Methode wird von der getHoldability-Methode in der java.sql.ResultSet-Schnittstelle angegeben.  
  
 Zum Festlegen der Holdability für Resultsets kann von Anwendungen die [setHoldability](../../../connect/jdbc/reference/setholdability-method-sqlserverconnection.md)-Methode der [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)-Klasse verwendet werden. Nach dem Aufrufen der [setHoldability](../../../connect/jdbc/reference/setholdability-method-sqlserverconnection.md)-Methode, dem Erstellen des Anweisungsobjekts und dessen Resultsetobjekts und dem Ausführen der Anweisung muss die Haltbarkeit von der Anwendung möglicherweise erneut geändert werden.  
  
 Wenn Servercursor mit SQL Server 2005 oder höher verbunden sind, wirkt sich das Festlegen der Haltbarkeit nur auf die Haltbarkeit neuer Resultsets aus, die erst noch für diese Verbindung erstellt werden. Bei SQL Server 2000 wirkt sich das Festlegen der Haltbarkeit jedoch sowohl auf die Haltbarkeit vorhandener als auch neuer, noch für die Verbindung zu erstellender Resultsets aus.  
  
 Wird die Haltbarkeit zurückgesetzt und die getHoldability-Methode für ein zuvor erstelltes Resultsetobjekt aufgerufen, unterscheidet sich der von dieser Methode zurückgegebene Wert möglicherweise vom Holdability-Wert, der von den folgenden Methoden zurückgegeben wird: Statement.getResultSetHoldability, Connection.getHoldability oder DatabaseMetaData.getResultSetHoldability.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerResultSet-Elemente](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet-Klasse](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
