---
description: execute-Methode ()
title: Methode „execute()“ | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.execute ()
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: fa96d0f8-101b-422f-a767-405be9a5f74f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8a5339925a0325590594f5e62003c1dba935440d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165936"
---
# <a name="execute-method-"></a>execute-Methode ()
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Führt die SQL-Anweisung in diesem [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)-Objekt aus. Hierbei kann es sich um eine beliebige Art von SQL-Anweisung handeln.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public boolean execute()  
```  
  
## <a name="return-value"></a>Rückgabewert  
 Der Wert lautet **TRUE**, wenn die Anweisung ein Resultset zurückgibt. Der Wert lautet **FALSE**, wenn die Anweisung einen aktualisierten Zählerwert oder kein Ergebnis zurückgibt.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese execute-Methode wird von der execute-Methode in der java.sql.PreparedStatement-Schnittstelle angegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [execute-Methode &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/execute-method-sqlserverpreparedstatement.md)   
 [SQLServerPreparedStatement-Elemente](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [SQLServerPreparedStatement-Klasse](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
