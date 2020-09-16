---
description: getSchemas-Methode (String, String)
title: getSchemas-Methode (String, String) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 672171ac-976f-4605-9bee-2a5e141d92cb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a3b421ae475d6c161396380073d1d8336b7f1b26
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88434642"
---
# <a name="getschemas-method-string-string"></a>getSchemas-Methode (String, String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft die in der aktuellen Datenbank verfügbaren Schemanamen unter Verwendung des angegebenen Katalognamens und des Schemanamens ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public ResultSet getSchemas(java.lang.String catalog,  
                       java.lang.String schemaPattern)  
```  
  
#### <a name="parameters"></a>Parameter  
 *catalog*  
  
 Der Name eines Katalogs in der Datenbank. Bei einer leeren Zeichenfolge ("") enthält das Ergebnis die Schemas ohne einen Katalog. Bei **NULL** wird der Katalogname nicht für die Suche verwendet.  
  
 *schemaPattern*  
  
 Der Name eines Schemas. Bei **NULL** wird der Schemaname nicht für die Suche verwendet.  
  
## <a name="return-value"></a>Rückgabewert  
 Ein [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekt.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getSchemas-Methode wird von der getSchemas-Methode in der java.sql.DatabaseMetaData-Schnittstelle angegeben.  
  
 Das von der getSchemas-Methode zurückgegebene Resultset enthält folgende Informationen:  
  
|Name|Typ|BESCHREIBUNG|  
|----------|----------|-----------------|  
|TABLE_SCHEM|**String**|Der Name des Schemas.|  
|TABLE_CATALOG|**String**|Der Katalogname für das Schema.|  
  
 Die Ergebnisse werden nach „TABLE_CATALOG“ und anschließend nach „TABLE_SCHEM“ sortiert. In jeder Zeile bildet "TABLE_SCHEM" die erste Spalte und "TABLE_CATALOG" die zweite Spalte.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerDatabaseMetaData-Methoden](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData-Elemente](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData-Klasse](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
