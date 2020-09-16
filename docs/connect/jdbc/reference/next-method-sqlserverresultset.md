---
description: next-Methode (SQLServerResultSet)
title: next-Methode (SQLServerResultSet) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerResultSet.next
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 60248447-6908-4036-a779-a501453cd553
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 48e6d1fcfd0a7e2e2778ed6b2f109e66024df7b9
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88433182"
---
# <a name="next-method-sqlserverresultset"></a>next-Methode (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Versetzt den Cursor von seiner aktuellen Position aus um eine Zeile nach unten.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public boolean next()  
```  
  
## <a name="return-value"></a>Rückgabewert  
 Der Wert **TRUE** wird zurückgegeben, wenn die neue aktuelle Zeile gültig ist. Wenn keine weiteren zu verarbeitenden Zeilen vorhanden sind, wird der Wert **FALSE** zurückgegeben.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese next-Methode wird von der next-Methode in der java.sql.ResultSet-Schnittstelle angegeben.  
  
 Ein Resultsetcursor wird anfänglich vor dieser Zeile positioniert. Beim ersten Aufruf der next-Methode wird die erste Zeile zur aktuellen Zeile, beim zweiten Aufruf wird die zweite Zeile zur aktuellen Zeile usw.  
  
 Wenn für die aktuelle Zeile ein Eingabedatenstrom geöffnet ist, wird dieser durch einen Aufruf der next-Methode implizit geschlossen. Beim Lesen einer neuen Zeile wird eine Warnkette für das [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekt gelöscht.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerResultSet-Elemente](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet-Klasse](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
