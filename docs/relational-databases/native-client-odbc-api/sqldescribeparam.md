---
description: SQLDescribeParam
title: SQLDescribeParam | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLDescribeParam function
ms.assetid: 396e74b1-5d08-46dc-b404-2ef2003e4689
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5352afc5b711cd0158a7119da367fe5659b78989
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465323"
---
# <a name="sqldescribeparam"></a>SQLDescribeParam
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Um die Parameter einer SQL-Anweisung zu beschreiben, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellt und führt der Native Client ODBC-Treiber eine SELECT-Anweisung aus, [!INCLUDE[tsql](../../includes/tsql-md.md)] Wenn SQLDescribeParam für ein vorbereitetes ODBC-Anweisungs Handle aufgerufen wird. Die Metadaten des Resultsets bestimmen die Eigenschaften der Parameter in der vorbereiteten Anweisung. SQLDescribeParam kann jeden Fehlercode zurückgeben, der von SQLExecute oder SQLExecDirect zurückgegeben werden kann.  
  
 Verbesserungen in der Datenbank-Engine, beginnend mit " [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SQLDescribeParam", können genauere Beschreibungen der erwarteten Ergebnisse erhalten. Diese präziseren Ergebnisse können sich von den Werten unterscheiden, die von SQLDescribeParam in früheren Versionen von zurückgegeben wurden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Weitere Informationen finden Sie unter [Metadatenermittlung](../../relational-databases/native-client/features/metadata-discovery.md).  
  
 Außerdem [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] gibt *ParameterSizePtr* , das in neu ist, nun einen Wert zurück, der der Definition der Spalte oder des Ausdrucks der entsprechenden Parameter Markierung entsprechend der Definition in der [ODBC-Spezifikation](../../odbc/reference/appendixes/column-size.md)entspricht. In früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client konnte *ParameterSizePtr* der entsprechende Wert **SQL_DESC_OCTET_LENGTH** für den-Typ oder ein irrelevante Spaltengrößen Wert sein, der für SQLBindParameter für einen Typ bereitgestellt wurde. der Wert von, der ignoriert werden soll (z. b.**SQL_INTEGER**).  
  
 Der Treiber unterstützt das Aufrufen von SQLDescribeParam in folgenden Situationen nicht:  
  
-   Nach SQLExecDirect für alle [!INCLUDE[tsql](../../includes/tsql-md.md)] Update-oder DELETE-Anweisungen, die die from-Klausel enthalten.  
  
-   Für eine ODBC- oder [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung, die einen Parameter in einer HAVING-Klausel enthält oder die mit dem Ergebnis einer SUM-Funktion verglichen wird.  
  
-   Für eine ODBC- oder [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung abhängig von einer Unterabfrage, die Parameter enthält.  
  
-   Für ODBC SQL-Anweisungen, die Parametermarkierungen in beiden Ausdrücken eines Vergleichs oder quantifizierten Prädikats enthalten.  
  
-   Für Abfragen, bei denen einer der Parameter ein Parameter für eine Funktion ist.  
  
-   Wenn im Befehl Kommentare (/* \* /) vorhanden sind [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 Beim Verarbeiten eines Batches von- [!INCLUDE[tsql](../../includes/tsql-md.md)] Anweisungen unterstützt der Treiber auch das Aufrufen von SQLDescribeParam für Parameter Markierungen in-Anweisungen nach der ersten Anweisung im Batch nicht.  
  
 Beim Beschreiben der Parameter vorbereiteter gespeicherter Prozeduren verwendet SQLDescribeParam die gespeicherte System Prozedur [sp_sproc_columns](../../relational-databases/system-stored-procedures/sp-sproc-columns-transact-sql.md) , um Parameter Eigenschaften abzurufen. sp_sproc_columns können Daten für gespeicherte Prozeduren innerhalb der aktuellen Benutzerdatenbank melden. Durch das Vorbereiten eines voll qualifizierten Namens einer gespeicherten Prozedur kann SQLDescribeParam Daten Bank übergreifend ausgeführt werden. So kann z. b. die gespeicherte System Prozedur [sp_who](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md) in einer beliebigen Datenbank wie folgt vorbereitet und ausgeführt werden:  
  
```  
SQLPrepare(hstmt, "{call sp_who(?)}", SQL_NTS);  
```  
  
 Das Ausführen von SQLDescribeParam nach erfolgreicher Vorbereitung gibt einen leeren Zeilen Satz zurück, wenn eine Verbindung mit einer Datenbank, aber mit dem **Master** besteht. Derselbe wie folgt vorbereitete-Befehl bewirkt, dass SQLDescribeParam unabhängig von der aktuellen Benutzerdatenbank erfolgreich ausgeführt wird:  
  
```  
SQLPrepare(hstmt, "{call master..sp_who(?)}", SQL_NTS);  
```  
  
 Bei Datentypen mit umfangreichen Werten ist der in *DataTypePtr* zurückgegebene Wert SQL_VARCHAR, SQL_VARBINARY oder SQL_NVARCHAR. Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client-ODBC-Treiber legt *ParameterSizePtr* auf 0 fest, um anzugeben, dass die Größe des Datentyp Parameters für große Werte unbegrenzt ist. Tatsächliche Größen Werte werden für **varchar** -Standardparameter zurückgegeben.  
  
> [!NOTE]  
>  Wenn der Parameter bereits mit einer Maximalgröße für den SQL_VARCHAR-, SQL_VARBINARY- oder SQL_WVARCHAR-Parameter gebunden wurde, wird die gebundene Größe des Parameters und nicht "unbegrenzt" zurückgegeben.  
  
 Um einen Eingabeparameter mit "unbegrenzter" Größe zu binden, muss Data-at-Execution verwendet werden. Es ist nicht möglich, einen "Unlimited"-Ausgabeparameter zu binden (es gibt keine Methode zum Streamen von Daten aus einem Output-Parameter, wie z. b. [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) für Resultsets).  
  
 Für Ausgabeparameter muss ein Puffer gebunden werden und wenn der Wert zu umfangreich ist, wird der Puffer aufgefüllt und eine SQL_SUCCESS_WITH_INFO-Meldung wird zusammen mit einer Warnung "Zeichenfolgendaten werden rechts abgeschnitten" zurückgegeben. Die abgeschnittenen Daten werden anschließend verworfen.  
  
## <a name="sqldescribeparam-and-table-valued-parameters"></a>SQLDescribeParam und Tabellenwertparameter  
 Eine Anwendung kann Tabellenwert Parameter-Informationen für eine vorbereitete Anweisung mit SQLDescribeParam abrufen. Weitere Informationen finden Sie unter [Tabellenwert Parameter-Metadaten für vorbereitete Anweisungen](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-metadata-for-prepared-statements.md).  
  
 Weitere Informationen zu Tabellenwert Parametern im Allgemeinen finden Sie unter [Tabellenwert Parameter &#40;ODBC-&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqldescribeparam-support-for-enhanced-date-and-time-features"></a>SQLDescribeParam-Unterstützung für erweiterte Funktionen für Datum und Uhrzeit  
 Die für Datums-/Uhrzeittypen zurückgegebenen Werte lauten wie folgt:  
  
| Attribut | *DataTypePtr* | *ParameterSizePtr* | *Decimaldigitsptr* |  
| --------- | ------------- | ------------------ | ------------------ |  
|datetime|SQL_TYPE_TIMESTAMP|23|3|  
|smalldatetime|SQL_TYPE_TIMESTAMP|16|0|  
|date|SQL_TYPE_DATE|10|0|  
|time|SQL_SS_TIME2|8, 10..16|0..7|  
|datetime2|SQL_TYPE_TIMESTAMP|19, 21..27|0..7|  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET|26, 28..34|0..7|  
  
 Weitere Informationen finden Sie unter [Verbesserungen bei Datum und Uhrzeit &#40;ODBC-&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqldescribeparam-support-for-large-clr-udts"></a>SQLDescribeParam-Unterstützung für große CLR-UDTs  
 **SQLDescribeParam** unterstützt große benutzerdefinierte CLR-Typen (UDTs). Weitere Informationen finden Sie unter [große CLR-User-Defined Typen &#40;ODBC-&#41;](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLDescribeParam-Funktion](../../odbc/reference/syntax/sqldescribeparam-function.md)   
 [ODBC API Implementation Details](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
