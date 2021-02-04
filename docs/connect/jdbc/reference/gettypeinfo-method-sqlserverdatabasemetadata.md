---
description: getTypeInfo-Methode (SQLServerDatabaseMetaData)
title: getTypeInfo-Methode (SQLServerDatabaseMetaData) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getTypeInfo
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 23208f01-c1bf-4235-b29c-9051d3df59a3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43145d029b4095ab05cd47526deface99dc81eb0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99155652"
---
# <a name="gettypeinfo-method-sqlserverdatabasemetadata"></a>getTypeInfo-Methode (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ruft eine Beschreibung aller standardmäßigen SQL-Typen ab, die von der aktuellen Datenbank unterstützt werden.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
public java.sql.ResultSet getTypeInfo()  
```  
  
## <a name="return-value"></a>Rückgabewert  
 Ein [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)-Objekt.  
  
## <a name="exceptions"></a>Ausnahmen  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Bemerkungen  
 Diese getTypeInfo-Methode wird von der getTypeInfo-Methode in der java.sql.DatabaseMetaData-Schnittstelle angegeben.  
  
 Das von der getTypeInfo-Methode zurückgegebene Resultset enthält folgende Informationen:  
  
|Name|Typ|BESCHREIBUNG|  
|----------|----------|-----------------|  
|TYPE_NAME|**String**|Der Name des Datentyps.|  
|DATA_TYPE|**short**|Der SQL-Datentyp aus "java.sql.Types".|  
|PRECISION|**int**|Die Gesamtanzahl von signifikanten Stellen.|  
|LITERAL_PREFIX|**String**|Die Zeichen, die einer Konstante vorangestellt werden.|  
|LITERAL_SUFFIX|**String**|Die Zeichen, die eine Konstante beenden.|  
|CREATE_PARAMS|**String**|Die Beschreibung der Erstellungsparameter für den Datentyp.|  
|NULLABLE|**short**|Gibt an, ob die Spalte einen NULL-Wert enthalten kann. Es kann sich um einen der folgenden Werte handeln:<br /><br /> typeNoNulls (0)<br /><br /> typeNullable (1)<br /><br /> typeNullableUnknown (2)|  
|CASE_SENSITIVE|**boolean**|Gibt an, ob bei dem Datentyp die Groß-/Kleinschreibung berücksichtigt wird. **TRUE**, wenn die Groß-/Kleinschreibung vom Typ berücksichtigt wird, andernfalls **FALSE**.|  
|DURCHSUCHBAR|**short**|Gibt an, ob die Spalte in einer SQL-Klausel vom Typ "WHERE" verwendet werden kann. Es kann sich um einen der folgenden Werte handeln:<br /><br /> typePredNone (0)<br /><br /> typePredChar (1)<br /><br /> typePredBasic (2)<br /><br /> typeSeachable (3)|  
|UNSIGNED_ATTRIBUTE|**boolean**|Gibt das Vorzeichen des Datentyps an. **TRUE**, wenn der Typ kein Vorzeichen besitzt, andernfalls **FALSE**.|  
|FIXED_PREC_SCALE|**boolean**|Gibt an, dass es sich bei dem Datentyp um eine Währung handeln kann. **TRUE**, wenn es sich bei dem Datentyp um eine Währung handelt, andernfalls **FALSE**.|  
|AUTO_INCREMENT|**boolean**|Gibt an, dass der Datentyp automatisch inkrementiert werden kann. **TRUE**, wenn der Typ automatisch inkrementiert werden kann, andernfalls **FALSE**.|  
|LOCAL_TYPE_NAME|**String**|Der lokalisierte Name des Datentyps.|  
|MINIMUM_SCALE|**short**|Die maximale Anzahl von Stellen rechts des Dezimalzeichens.|  
|MAXIMUM_SCALE|**short**|Die Mindestanzahl von Stellen rechts des Dezimalzeichens.|  
|SQL_DATA_TYPE|**int**|Wird vom JDBC-Treiber nicht unterstützt.|  
|SQL_DATETIME_SUB|**int**|Wird vom JDBC-Treiber nicht unterstützt.|  
|NUM_PREC_RADIX|**int**|Die Anzahl von Bits oder Stellen zum Berechnen der höchsten Zahl, die eine Spalte enthalten kann.|  
|INTERVAL_PRECISION|**smallint**|Die Genauigkeit für anführenden Intervallwert.|  
|USERTYPE|**smallint**|Der **usertype**-Wert aus der Tabelle **systypes**. Weitere Informationen finden Sie in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Onlinedokumentation.|  
  
> [!NOTE]  
>  Weitere Informationen zu den Daten, die von der getTypeInfo-Methode zurückgegeben werden, finden Sie in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Onlinedokumentation unter „sp_datatype_info (Transact-SQL)“.  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird veranschaulicht, wie mithilfe der getTypeInfo-Methode Informationen zu den Datentypen zurückgegeben werden können, die in einer Datenbank der Version [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] oder höher verwendet werden.  
  
```  
public static void executeGetTypeInfo(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getTypeInfo();  
      ResultSetMetaData rsmd = rs.getMetaData();  
  
      // Display the result set data.  
      int cols = rsmd.getColumnCount();  
      while(rs.next()) {  
         for (int i = 1; i <= cols; i++) {  
            System.out.println(rs.getString(i));  
         }  
      }  
      rs.close();  
   }   
  
   catch (Exception e) {  
      e.printStackTrace();  
   }  
}  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLServerDatabaseMetaData-Methoden](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData-Elemente](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData-Klasse](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
