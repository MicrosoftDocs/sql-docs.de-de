---
description: storesMixedCaseIdentifiers-Methode (SQLServerDatabaseMetaData)
title: Methode „storesMixedCaseIdentifiers“ (SQLServerDatabaseMetaData) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerDatabaseMetaData.storesMixedCaseIdentifiers
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: a91e5cd6-22b1-464e-aeec-665590737a74
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ed07af3477a591a1049f4157c06520deee51ec61
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88450422"
---
# <a name="storesmixedcaseidentifiers-method-sqlserverdatabasemetadata"></a>storesMixedCaseIdentifiers-Methode (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft ab, ob bei SQL-Bezeichnern mit gemischter Groß-/Kleinschreibung, die nicht in Anführungszeichen gesetzt sind, die Groß-/Kleinschreibung nicht berücksichtigt wird und die Bezeichner mit gemischter Groß-/Kleinschreibung gespeichert werden.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public boolean storesMixedCaseIdentifiers()  
```  
  
## <a name="return-value"></a>Rückgabewert  
 Der Wert **TRUE** wird zurückgegeben, wenn die Bezeichner mit gemischter Groß-/Kleinschreibung gespeichert werden. Andernfalls lautet der Wert **false**.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese storesMixedCaseIdentifiers-Methode wird von der storesMixedCaseIdentifiers-Methode im der java.sql.ServerDatabaseMetaData-Schnittstelle angegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerDatabaseMetaData-Methoden](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData-Elemente](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData-Klasse](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
