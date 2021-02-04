---
description: getFunctions-Methode (SQLServerDatabaseMetaData)
title: getFunctions-Methode (SQLServerDatabaseMetaData) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 44335cbd-c84d-4ef3-a6a1-fca7eb7ec768
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8ede94f4024fc5c99dc120d4ad4137d8fe9df716
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162982"
---
# <a name="getfunctions-method-sqlserverdatabasemetadata"></a>getFunctions-Methode (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft eine Beschreibung der System- und Benutzerfunktionen ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public ResultSet getFunctions(java.lang.String catalog,  
                       java.lang.String schemaPattern,  
                       java.lang.String functionNamePattern)  
```  
  
#### <a name="parameters"></a>Parameter  
 *catalog*  
  
 Der Name eines Katalogs in der Datenbank. Bei einer leeren Zeichenfolge ("") enthält das Ergebnis die Funktionen ohne einen Katalog. Bei **NULL** wird der Katalogname nicht für die Suche verwendet.  
  
 *schemaPattern*  
  
 Der Name eines Schemas. Bei einer leeren Zeichenfolge ("") enthält das Ergebnis die Funktionen ohne ein Schema. Bei **NULL** wird der Schemaname nicht für die Suche verwendet.  
  
 *functionNamePattern*  
  
 Der Name einer Funktion.  
  
## <a name="return-value"></a>Rückgabewert  
 Ein [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekt.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getFunctions-Methode wird von der getFunctions-Methode in der java.sql.DatabaseMetaData-Schnittstelle angegeben.  
  
 Von dieser Methode werden nur die System- und Benutzerfunktionen zurückgegeben, die dem angegebenen Schema- und Funktionsnamen entsprechen.  
  
> [!IMPORTANT]  
>  Das zurückgegebene Resultset kann Funktionen enthalten, zu deren Ausführung der aufrufende Benutzer nicht berechtigt ist.  
  
 Jede Funktionsbeschreibung umfasst die folgenden Spalten:  
  
|Name|Typ|BESCHREIBUNG|  
|----------|----------|-----------------|  
|FUNCTION_CAT|**String**|Der Name der Datenbank, in der die Funktion gespeichert ist.|  
|FUNCTION_SCHEM|**String**|Der Name des Schemas, in dem die Funktion gespeichert ist.|  
|FUNCTION_NAME|**String**|Der Name der Funktion.|  
|NUM_INPUT_PARAMS|**int**|Reserviert für zukünftige Verwendung. Gibt derzeit den Wert "-1" zurück.|  
|NUM_OUTPUT_PARAMS|**int**|Reserviert für zukünftige Verwendung. Gibt derzeit den Wert "-1" zurück.|  
|NUM_RESULT_SETS|**int**|Reserviert für zukünftige Verwendung. Gibt derzeit den Wert "-1" zurück.|  
|ANMERKUNGEN|**String**|Kommentare zur Funktion.|  
|FUNCTION_TYPE|**short**|Der Typ der Funktion. Es kann sich um einen der folgenden Werte handeln:<br /><br /> SQL_PT_UNKNOWN (0)<br /><br /> SQL_PT_PROCEDURE (1)<br /><br /> SQL_PT_FUNCTION (2)|  
  
 Alle Beschreibungen im zurückgegebenen Resultset sind nach "FUNCTION_CAT", "FUNCTION_SCHEM", "FUNCTION_NAME" und "SPECIFIC_NAME" sortiert.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerDatabaseMetaData-Elemente](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData-Klasse](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
