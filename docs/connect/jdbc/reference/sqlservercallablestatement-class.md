---
description: SQLServerCallableStatement-Klasse
title: Klasse „SQLServerCallableStatement“ | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 30710a63-c05d-47d9-9cf9-c087a1c76373
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 60faed31bc161bda250a4981d98406f81815a1ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178418"
---
# <a name="sqlservercallablestatement-class"></a>SQLServerCallableStatement-Klasse
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Mit dieser Klasse kann der gespeicherte Prozedurname angegeben werden, der mit Eingabe- und Ausgabeparametern aufgerufen wird. Mit dieser Klasse können Sie außerdem den Wert für den Rückgabestatus mit der `? = call( ?, ..)`-Syntax abrufen.  
  
 **Paket:** com.microsoft.sqlserver.jdbc  
  
 **Implementiert** [ISQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
 **Erweitert** [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public final class SQLServerCallableStatement  
```  
  
## <a name="remarks"></a>Bemerkungen  
 Mithilfe von SQLServerCallableStatement kann der gespeicherte Prozedurname angegeben werden, der mit Eingabe- und Ausgabeparametern aufgerufen wird. Mit der Klasse „SQLServerCallableStatement“ können Sie außerdem den Wert für den Rückgabestatus mit der `? = call( ?, ..)`-Syntax abrufen.  
  
 Diese Klasse unterstützt das Entpacken in die SQLServerCallableStatement-Klasse, die ISQLServerCallableStatement-Schnittstelle, die java.sql.CallableStatement-Schnittstelle und die von SQLServerPreparedStatement für das Entpacken unterstützten Klassen und Schnittstellen. Weitere Informationen finden Sie unter [Wrapper und Schnittstellen](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
 Wenn eine der SQLServerCallableStatement-Set-Methoden für einen Typ aufgerufen wird und der betreffende Typ mit dem von [registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) angegebenen Typ in Konflikt steht, wird der von der letzten SQLServerCallableStatement-Set-Methode angegebene Typ verwendet. Dies kann jedoch zu Konvertierungsfehlern aufgrund von inkompatiblen Datentypen führen. Wird keine SQLServerCallableStatement-Set-Methode aufgerufen, wird der Typ verwendet, der mit dem ersten [registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md)-Aufruf angegeben wird.  
  
 Der JDBC-Treiber 3.0 für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] folgt der Empfehlung für JDBC 4.0, die besagt, dass ein Resultset und die Anzahl von Updates abgerufen werden müssen, bevor OUT-Parameter abgerufen werden können. Sollten OUT-Parameter vor Abschluss der Verarbeitung des Resultsets und der Updatezählungen abgerufen werden, gehen alle noch nicht verarbeiteten Resultsets und Updatezählungen verloren.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerCallableStatement-Elemente](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [API-Referenz für den JDBC-Treiber](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
