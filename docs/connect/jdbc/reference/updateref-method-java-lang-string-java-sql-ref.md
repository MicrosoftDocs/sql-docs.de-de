---
description: updateRef-Methode (java.lang.String, java.sql.Ref)
title: updateRef-Methode (java.lang.String, java.sql.Ref) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerResultSet.updateRef (java.lang.String, java.sql.Ref)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7740d17d-282f-4f1d-91f9-c68a873b069a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4f8878ac687c0357f2bf0a4512c1a9c1c52dc444
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88484930"
---
# <a name="updateref-method-javalangstring-javasqlref"></a>updateRef-Methode (java.lang.String, java.sql.Ref)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aktualisiert die angegebene Spalte unter Berücksichtigung des Spaltennamens mit einem java.sql.Ref-Wert.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public void updateRef(java.lang.String columnName,  
                      java.sql.Ref x)  
```  
  
#### <a name="parameters"></a>Parameter  
 *columnName*  
  
 Eine **Zeichenfolge**, die den Spaltennamen enthält.  
  
 *x*  
  
 Ein Ref-Objekt  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese updateRef-Methode wird von der updateRef-Methode in der java.sql.ResultSet-Schnittstelle angegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [updateRef Method &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateref-method-sqlserverresultset.md)   
 [SQLServerResultSet-Elemente](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet-Klasse](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
