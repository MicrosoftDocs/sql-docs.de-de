---
description: setMaxRows-Methode (SQLServerStatement)
title: setMaxRows-Methode (SQLServerStatement) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerStatement.setMaxRows
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: cccc0667-589b-4655-8ea8-14ae8b2eb9dc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 544eb34b40bc81afbed34809f3348c5649c3d0be
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88431762"
---
# <a name="setmaxrows-method-sqlserverstatement"></a>setMaxRows-Methode (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Legt den Grenzwert für die maximale Anzahl von Zeilen, die ein beliebiges [SQLServerResultSet-Objekt](../../../connect/jdbc/reference/sqlserverresultset-class.md) enthalten kann, auf die angegebene Anzahl fest.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public final void setMaxRows(int max)  
```  
  
#### <a name="parameters"></a>Parameter  
 *max*  
  
 Ein Wert vom Typ **int** zum Angeben der maximalen Zeilenanzahl oder 0 (null), wenn kein Grenzwert vorhanden ist.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese setMaxRows-Methode wird von der setMaxRows-Methode in der java.sql.Statement-Schnittstelle angegeben.  
  
 Diese setMaxRows-Methode hat keine Auswirkungen auf dynamische scrollbare Cursor. Von der Anwendung sollte die Anzahl von Zeilen, die von potenziell umfangreichen Resultsets zurückgegeben wird, mithilfe der SQL-Syntax "SELECT TOP N" eingeschränkt werden.  
  
 Wenn die setMaxRows-Methode aufgerufen wird, wird von [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] beim Ausführen der Anwendungsabfrage die SQL-Anweisung SET ROWCOUNT ausgeführt. Dadurch wird die maximale Anzahl von Zeilen beschränkt, die von Anweisungen vom Typ [!INCLUDE[tsql](../../../includes/tsql-md.md)] betroffen sind, die von dieser Abfrage ausgeführt werden (und nicht nur die Anzahl von Zeilen, die von dieser Abfrage zurückgegeben werden). Wenn von der Anwendung lediglich ein Grenzwert für das oberste [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekt festgelegt werden muss, muss in der Abfrage anstelle der setMaxRows-Methode die SQL-Syntax SELECT TOP N verwendet werden.  
  
 Weitere Informationen zur SQL-Anweisung SET ROWCOUNT finden Sie in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Onlinedokumentation unter [SET ROWCOUNT (Transact-SQL)](https://go.microsoft.com/fwlink/?LinkId=139522).  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerStatement-Elemente](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement-Klasse](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
