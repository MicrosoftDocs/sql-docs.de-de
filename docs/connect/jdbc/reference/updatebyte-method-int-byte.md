---
description: updateByte-Methode (int, byte)
title: updateByte(int, byte)-Methode | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerResultSet.updateByte (int, byte)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: e635d789-9218-488e-a213-2e3e09635acc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 00a7eea10e4ded2bcac76b0825f6ba33722d5795
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88462245"
---
# <a name="updatebyte-method-int-byte"></a>updateByte-Methode (int, byte)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Aktualisiert die angegebene Spalte mit einem Wert vom Typ **byte** unter Berücksichtigung des Spaltenindexes.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public void updateByte(int index,  
                       byte x)  
```  
  
#### <a name="parameters"></a>Parameter  
 *Index*  
  
 Ein **ganzzahliger** Wert, der den Spaltenindex angibt.  
  
 *x*  
  
 Ein Wert vom Typ **byte**.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese updateByte-Methode wird von der updateByte-Methode in der java.sql.ResultSet-Schnittstelle angegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [updateByte-Methode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatebyte-method-sqlserverresultset.md)   
 [SQLServerResultSet-Elemente](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet-Klasse](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
