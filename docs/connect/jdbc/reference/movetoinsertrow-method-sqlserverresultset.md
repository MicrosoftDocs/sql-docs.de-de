---
description: moveToInsertRow-Methode (SQLServerResultSet)
title: moveToInsertRow-Methode (SQLServerResultSet) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.moveToInsertRow
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f3c54bfe-d5b7-4f6e-ae6c-3e8954e5b1c9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 431e9dbc8570d9505b9a2bfd1b8922229b22e3e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177039"
---
# <a name="movetoinsertrow-method-sqlserverresultset"></a>moveToInsertRow-Methode (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Versetzt den Cursor in die Einfügezeile.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public void moveToInsertRow()  
```  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese moveToInsertRow-Methode wird von der moveToInsertRow-Methode in der java.sql.ResultSet-Schnittstelle angegeben.  
  
 Die aktuelle Cursorposition wird gespeichert und der Cursor in die Einfügezeile versetzt. Die Einfügezeile ist eine spezielle Zeile, die einem aktualisierbaren Resultset zugewiesen ist. Sie ist eigentlich ein Puffer, in dem eine neue Zeile durch Aufrufen der Aktualisierungsmethoden vor dem Hinzufügen der Zeile zum Resultset erstellt wird.  
  
 Nur die Methoden für Aktualisierer, Getter und [insertRow](../../../connect/jdbc/reference/insertrow-method-sqlserverresultset.md) können aufgerufen werden, wenn sich der Cursor in der Einfügezeile befindet. Bei jedem Aufruf dieser Methode und vor dem Aufruf von insertRow muss allen Spalten in einem Resultset ein Wert zugewiesen werden. Bevor eine Getter-Methode für einen Spaltenwert aufgerufen werden kann, muss eine Aktualisierungsmethode aufgerufen werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerResultSet-Elemente](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet-Klasse](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
