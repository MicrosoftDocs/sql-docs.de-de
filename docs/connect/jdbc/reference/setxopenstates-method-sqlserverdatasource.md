---
description: setXopenStates-Methode (SQLServerDataSource)
title: Methode „setXopenStates“ (SQLServerDataSource) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setXopenStates
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9723591f-e987-426f-b70a-07f5c70dc094
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0ea389a352ec117e6cd59acf709bb6300434d5cf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178424"
---
# <a name="setxopenstates-method-sqlserverdatasource"></a>setXopenStates-Methode (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Legt einen Wert vom Typ **Boolean** fest, mit dem angegeben wird, ob das Konvertieren von SQL-Status in XOPEN-kompatible Status aktiviert ist.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public void setXopenStates(boolean xopenStates)  
```  
  
#### <a name="parameters"></a>Parameter  
 *xopenStates*  
  
 **TRUE**, wenn das Konvertieren von SQL-Status zu XOPEN-kompatiblen Status aktiviert ist. Andernfalls lautet der Wert **false**.  
  
## <a name="remarks"></a>Bemerkungen  
 Ist die xopenStates-Eigenschaft auf **true** festgelegt, werden SQL-Status von [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] in XOPEN-kompatible Status konvertiert. Durch den Standardwert **false** werden vom JDBC-Treiber SQL 99-Statuscodes zurückgegeben. Ist die xopenStates-Eigenschaft nicht festgelegt, wird von der [getXopenStates](../../../connect/jdbc/reference/getxopenstates-method-sqlserverdatasource.md)-Methode der Standardwert **false** zurückgegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerDataSource-Elemente](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource-Klasse](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
