---
description: getBytes-Methode (int) (SQLServerResultSet)
title: getBytes-Methode (int) (SQLServerResultSet) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getBytes (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 1385d7d4-9288-4cbd-8606-4b919e9b07b2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c33a1580e6474d85b72e4bff6d33475accfaba09
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168180"
---
# <a name="getbytes-method-int-sqlserverresultset"></a>getBytes-Methode (int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft den Wert des angegebenen Spaltenindexes in der aktuellen Zeile dieses [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekts als Array vom Typ **byte** in der Programmiersprache Java ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public byte[] getBytes(int columnIndex)  
```  
  
#### <a name="parameters"></a>Parameter  
 *columnIndex*  
  
 Ein **ganzzahliger** Wert, der den Spaltenindex angibt.  
  
## <a name="return-value"></a>Rückgabewert  
 Ein Array von **byte**-Werten  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getBytes-Methode wird von der getBytes-Methode in der java.sql.ResultSet-Schnittstelle angegeben.  
  
 Von dieser Methode wird das Abrufen aller Spalten als unaufbereitete Bytes vom Server unterstützt. Sie gibt ein Bytearray direkt vom Server in einem Format wieder, das auf dem Server gespeichert ist.  
  
 In einer früheren Version von [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] konnten Sie mithilfe von „SQLServerResultSet.getBytes“ Werte zwischen Bytearrays und dem [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Datentyp **date**, **time**, **datetime2** oder **datetimeoffset** konvertieren. Nun wird durch Verwendung der Methode mit diesen Datentypen eine Ausnahme ausgelöst, die angibt, dass die Konvertierung nicht unterstützt wird.  
  
## <a name="see-also"></a>Weitere Informationen  
 [getBytes-Methode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getbytes-method-sqlserverresultset.md)   
 [SQLServerResultSet-Elemente](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet-Klasse](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
