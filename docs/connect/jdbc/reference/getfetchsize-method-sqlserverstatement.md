---
description: getFetchSize-Methode (SQLServerStatement)
title: Methode „getFetchSize“ (SQLServerResultSet) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getFetchSize
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8115ca58-8ae9-46ce-8515-7905d7bb25fe
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 216ed0d1989a1646add0910fab4f40a929ecd0ae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163037"
---
# <a name="getfetchsize-method-sqlserverstatement"></a>getFetchSize-Methode (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft die Anzahl von Resultsetzeilen ab, bei der es sich um die standardmäßige Abrufgröße für Resultsetobjekte handelt, die auf der Grundlage dieses [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)-Objekts generiert werden.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public final int getFetchSize()  
```  
  
## <a name="return-value"></a>Rückgabewert  
 Ein Wert vom Typ **int** zum Angeben der Abrufgröße, die von der [setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md)-Methode angegeben wird.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getFetchSize-Methode wird von der getFetchSize-Methode in der java.sql.Statement-Schnittstelle angegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerStatement-Elemente](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement-Klasse](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
