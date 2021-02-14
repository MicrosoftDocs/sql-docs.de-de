---
description: SQLGetFunctions-Funktion
title: SQLGetFunctions-Funktion | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetFunctions
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLGetFunctions
helpviewer_keywords:
- SQLGetFunctions function [ODBC]
ms.assetid: 0451d2f9-0f4f-46ba-b252-670956a52183
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: edc7b7875e3ee3f7512ee19a62d68a50a1617436
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081361"
---
# <a name="sqlgetfunctions-function"></a>SQLGetFunctions-Funktion
**Konformitäts**  
 Eingeführte Version: ODBC 1,0 Standards Compliance: ISO 92  
  
 **Zusammenfassung**  
 **SQLGetFunctions** gibt Informationen darüber zurück, ob ein Treiber eine bestimmte ODBC-Funktion unterstützt. Diese Funktion wird im Treiber-Manager implementiert. Sie kann auch in Treibern implementiert werden. Wenn ein Treiber **SQLGetFunctions** implementiert, ruft der Treiber-Manager die Funktion im Treiber auf. Andernfalls wird die Funktion selbst ausgeführt.  
  
## <a name="syntax"></a>Syntax  
  
```cpp  
  
SQLRETURN SQLGetFunctions(  
     SQLHDBC           ConnectionHandle,  
     SQLUSMALLINT      FunctionId,  
     SQLUSMALLINT *    SupportedPtr);  
```  
  
## <a name="arguments"></a>Argumente  
 *ConnectionHandle*  
 [Eingabe] Verbindungshandle.  
  
 *FunctionId*  
 Der Ein **#define** Wert, der die ODBC-Funktion angibt, die von Interesse ist. **SQL_API_ODBC3_ALL_FUNCTIONS orSQL_API_ALL_FUNCTIONS**. **SQL_API_ODBC3_ALL_FUNCTIONS** wird von einer ODBC 3 *. x* -Anwendung verwendet, um die Unterstützung von ODBC 3 *. x* -Funktionen und früheren Funktionen zu ermitteln. **SQL_API_ALL_FUNCTIONS** wird von einer ODBC 2 *. x* -Anwendung verwendet, um die Unterstützung von ODBC 2 *. x* -Funktionen und früheren Funktionen zu ermitteln.  
  
 Eine Liste der **#define** Werte, die ODBC-Funktionen identifizieren, finden Sie in den Tabellen in "comments".  
  
 *Supportedptr*  
 Ausgeben  Wenn *FunctionID* eine einzelne ODBC-Funktion identifiziert, verweist *supportedptr* auf einen einzelnen sqlusmallint-Wert, der SQL_TRUE wird, wenn die angegebene Funktion vom Treiber unterstützt wird, und SQL_FALSE, wenn dies nicht unterstützt wird.  
  
 Wenn *FunctionID* SQL_API_ODBC3_ALL_FUNCTIONS ist, verweist *supportedptr* auf ein SQLSMALLINT-Array mit einer Reihe von Elementen, die SQL_API_ODBC3_ALL_FUNCTIONS_SIZE entsprechen. Dieses Array wird vom Treiber-Manager als 4.000-Bit-Bitmap behandelt und kann verwendet werden, um zu bestimmen, ob eine ODBC 3 *. x* -Funktion oder eine frühere Funktion unterstützt wird. Das SQL_FUNC_EXISTS-Makro wird aufgerufen, um die Funktions Unterstützung zu bestimmen. (Siehe "Kommentare") Eine ODBC 3 *. x* -Anwendung kann **SQLGetFunctions** mit SQL_API_ODBC3_ALL_FUNCTIONS für einen ODBC 3 *. x* -oder ODBC 2 *. x* -Treiber aufrufen.  
  
 Wenn *FunctionID* SQL_API_ALL_FUNCTIONS ist, verweist *supportedptr* auf ein sqlusmallint-Array mit 100 Elementen. Das Array wird durch **#define** Werte indiziert, die von *FunctionID* zum Identifizieren der einzelnen ODBC-Funktionen verwendet werden. Einige Elemente des Arrays werden nicht verwendet und für die zukünftige Verwendung reserviert. Ein-Element wird SQL_TRUE, wenn es eine vom Treiber unterstützte ODBC 2 *. x* -oder frühere Funktion identifiziert. Es ist SQL_FALSE, wenn eine ODBC-Funktion identifiziert wird, die nicht vom Treiber unterstützt wird, oder eine ODBC-Funktion nicht identifiziert.  
  
 Die in **supportedptr* zurückgegebenen Arrays verwenden eine Null basierte Indizierung.  
  
## <a name="returns"></a>Gibt zurück  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR oder SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnose  
 Wenn **SQLGetFunctions** SQL_ERROR oder SQL_SUCCESS_WITH_INFO zurückgibt, kann ein zugeordneter SQLSTATE-Wert durch Aufrufen von **SQLGetDiagRec** mit dem *Handlertyp* SQL_HANDLE_DBC und einem *handle* von *connectionHandle* abgerufen werden. In der folgenden Tabelle sind die SQLSTATE-Werte aufgelistet, die von **SQLGetFunctions** häufig zurückgegeben werden, und die einzelnen Werte werden im Kontext dieser Funktion erläutert. die Notation "(DM)" geht vor den Beschreibungen von Sqlstates vor, die vom Treiber-Manager zurückgegeben werden. Der Rückgabecode, der den einzelnen SQLSTATE-Werten zugeordnet ist, ist SQL_ERROR, sofern nichts anderes angegeben ist.  
  
|SQLSTATE|Fehler|BESCHREIBUNG|  
|--------|-----|-----------|  
|01000|Allgemeine Warnung|Treiber spezifische Informations Meldung. (Die Funktion gibt SQL_SUCCESS_WITH_INFO zurück.)|  
|08S01|Kommunikations Verbindungsfehler|Die Kommunikationsverbindung zwischen dem Treiber und der Datenquelle, mit der der Treiber verbunden war, ist fehlgeschlagen, bevor die Funktion die Verarbeitung abgeschlossen hat.|  
|HY000|Allgemeiner Fehler|Es ist ein Fehler aufgetreten, bei dem kein spezifischer SQLSTATE vorhanden war und für den kein Implementierungs spezifischer SQLSTATE definiert wurde. Die von **SQLGetDiagRec** im *\* MessageText* -Puffer zurückgegebene Fehlermeldung beschreibt den Fehler und die Ursache.|  
|HY001|Fehler bei der Speicher Belegung|Der Treiber konnte keinen Arbeitsspeicher zuweisen, der zur Unterstützung der Ausführung oder Beendigung der Funktion erforderlich ist.|  
|HY010|Funktions Sequenz Fehler|(DM) **SQLGetFunctions** wurde vor **SQLCONNECT**, **sqlbrowseconnetct** oder **SQLDriverConnect** aufgerufen.<br /><br /> (DM) **sqlbrowseconnetct** wurde für *connectionHandle* aufgerufen und SQL_NEED_DATA zurückgegeben. Diese Funktion wurde aufgerufen, bevor **sqlbrowseconnct** SQL_SUCCESS_WITH_INFO oder SQL_SUCCESS zurückgegeben hat.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** oder **SQLMoreResults** wurde für *connectionHandle* aufgerufen und SQL_PARAM_DATA_AVAILABLE zurückgegeben. Diese Funktion wurde aufgerufen, bevor Daten für alle gestreuten Parameter abgerufen wurden.|  
|HY013|Speicher Verwaltungsfehler|Der Funktions Aufrufwert konnte nicht verarbeitet werden, da auf die zugrunde liegenden Speicher Objekte nicht zugegriffen werden konnte, möglicherweise aufgrund von wenig Arbeitsspeicher.|  
|HY095|Funktionstyp außerhalb des gültigen Bereichs|(DM) Es wurde ein ungültiger *FunctionID* -Wert angegeben.|  
|HY117|Die Verbindung wurde aufgrund eines unbekannten Transaktions Zustands angehalten. Nur Disconnect-und Read-Only-Funktionen sind zulässig.|(DM) Weitere Informationen zum angehaltenen Status finden Sie unter [SQLEndTran Function](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Verbindungs Timeout abgelaufen|Der Verbindungs Timeout Zeitraum ist abgelaufen, bevor die Datenquelle auf die Anforderung geantwortet hat. Der Timeout Zeitraum für die Verbindung wird über **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT festgelegt.|  
  
## <a name="comments"></a>Kommentare  
 **SQLGetFunctions** gibt immer zurück, dass " **SQLGetFunctions**", " **SQLDataSources**" und " **SQLDrivers** " unterstützt werden. Dies geschieht, da diese Funktionen im Treiber-Manager implementiert werden. Der Treiber-Manager ordnet eine ANSI-Funktion der entsprechenden Unicode-Funktion zu, wenn die Unicode-Funktion vorhanden ist, und ordnet eine Unicode-Funktion der entsprechenden ANSI-Funktion zu, wenn die ANSI-Funktion vorhanden ist. Informationen dazu, wie Anwendungen **SQLGetFunctions** verwenden, finden Sie unter Schnittstellen Übereinstimmungs [Ebenen](../../../odbc/reference/develop-app/interface-conformance-levels.md).  
  
 Im folgenden finden Sie eine Liste gültiger Werte für *FunctionID* für Funktionen, die der ISO 92-Standards-Kompatibilitäts Ebene entsprechen:  
  
|FunctionId-Wert|FunctionId-Wert|  
|----------|----------|  
|SQL_API_SQLALLOCHANDLE|SQL_API_SQLGETDESCFIELD|  
|SQL_API_SQLBINDCOL|SQL_API_SQLGETDESCREC|  
|SQL_API_SQLCANCEL|SQL_API_SQLGETDIAGFIELD|  
|SQL_API_SQLCLOSECURSOR|SQL_API_SQLGETDIAGREC|  
|SQL_API_SQLCOLATTRIBUTE|SQL_API_SQLGETENVATTR|  
|SQL_API_SQLCONNECT|SQL_API_SQLGETFUNCTIONS|  
|SQL_API_SQLCOPYDESC|SQL_API_SQLGETINFO|  
|SQL_API_SQLDATASOURCES|SQL_API_SQLGETSTMTATTR|  
|SQL_API_SQLDESCRIBECOL|SQL_API_SQLGETTYPEINFO|  
|SQL_API_SQLDISCONNECT|SQL_API_SQLNUMRESULTCOLS|  
|SQL_API_SQLDRIVERS|SQL_API_SQLPARAMDATA|  
|SQL_API_SQLENDTRAN|SQL_API_SQLPREPARE|  
|SQL_API_SQLEXECDIRECT|SQL_API_SQLPUTDATA|  
|SQL_API_SQLEXECUTE|SQL_API_SQLROWCOUNT|  
|SQL_API_SQLFETCH|SQL_API_SQLSETCONNECTATTR|  
|SQL_API_SQLFETCHSCROLL|SQL_API_SQLSETCURSORNAME|  
|SQL_API_SQLFREEHANDLE|SQL_API_SQLSETDESCFIELD|  
|SQL_API_SQLFREESTMT|SQL_API_SQLSETDESCREC|  
|SQL_API_SQLGETCONNECTATTR|SQL_API_SQLSETENVATTR|  
|SQL_API_SQLGETCURSORNAME|SQL_API_SQLSETSTMTATTR|  
|SQL_API_SQLGETDATA| |  
  
 Im folgenden finden Sie eine Liste gültiger Werte für *FunctionID* für Funktionen, die der Standard Kompatibilitäts Stufe "Open Group" entsprechen:  
  
|FunctionId-Wert|FunctionId-Wert|  
|-|-|  
|SQL_API_SQLCOLUMNS|SQL_API_SQLSTATISTICS|  
|SQL_API_SQLSPECIALCOLUMNS|SQL_API_SQLTABLES|  
  
 Im folgenden finden Sie eine Liste gültiger Werte für *FunctionID* für Funktionen, die der ODBC-Standards-Kompatibilitäts Ebene entsprechen.  
  
|FunctionId-Wert|FunctionId-Wert|  
|-|-|  
|SQL_API_SQLBINDPARAMETER|SQL_API_SQLNATIVESQL|  
|SQL_API_SQLBROWSECONNECT|SQL_API_SQLNUMPARAMS|  
|SQL_API_SQLBULKOPERATIONS [1]|SQL_API_SQLPRIMARYKEYS|  
|SQL_API_SQLCOLUMNPRIVILEGES|SQL_API_SQLPROCEDURECOLUMNS|  
|SQL_API_SQLDESCRIBEPARAM|SQL_API_SQLPROCEDURES|  
|SQL_API_SQLDRIVERCONNECT|SQL_API_SQLSETPOS|  
|SQL_API_SQLFOREIGNKEYS|SQL_API_SQLTABLEPRIVILEGES|  
|SQL_API_SQLMORERESULTS| |  
  
 [1] bei der Verwendung eines ODBC 2 *. x* -Treibers wird **SQLBulkOperations** nur als unterstützt zurückgegeben, wenn beide der folgenden Werte zutreffen: der ODBC 2 *. x* -Treiber unterstützt **SQLSetPos**, und der Informationstyp SQL_POS_OPERATIONS gibt das SQL_POS_ADD Bit zurück, das als festgelegt ist.  
  
 Im folgenden finden Sie eine Liste gültiger Werte für *FunctionID* für Funktionen, die in ODBC 3,8 oder höher eingeführt wurden:  
  
|FunctionId-Wert|  
|-|  
|SQL_API_SQLCANCELHANDLE [2]|  
  
 [2]   **sqlcancelhandle** wird nur dann als unterstützt zurückgegeben, wenn der Treiber sowohl **SQLCancel** als auch **sqlcancelhandle** unterstützt. Wenn **SQLCancel** unterstützt wird, aber **sqlcancelhandle** nicht ist, kann die Anwendung immer noch **sqlcancelhandle** für ein Anweisungs Handle aufzurufen, da Sie **SQLCancel** zugeordnet wird.  
  
## <a name="sql_func_exists-macro"></a>SQL_FUNC_EXISTS-Makro  
 Das SQL_FUNC_EXISTS-Makro (*supportedptr*, *FunctionID*) wird verwendet, um die Unterstützung von ODBC 3 *. x* -Funktionen oder früheren Funktionen zu ermitteln, nachdem **SQLGetFunctions** mit dem *FunctionID* -Argument SQL_API_ODBC3_ALL_FUNCTIONS aufgerufen wurde. Die Anwendung ruft SQL_FUNC_EXISTS auf, wobei das *supportedptr* -Argument auf " *supportedptr* " festgelegt ist, das in *SQLGetFunctions* weitergegeben wurde, und mit dem *FunctionID* -Argument, das für die-Funktion auf die **#define** SQL_FUNC_EXISTS gibt SQL_TRUE zurück, wenn die Funktion unterstützt wird, und SQL_FALSE andernfalls.  
  
> [!NOTE]
>  Bei der Arbeit mit einem ODBC 2 *. x* -Treiber gibt der ODBC 3 *. x* -Treiber-Manager SQL_TRUE für **sqlzuordchandle** und **SQLFreeHandle** zurück, da **sqlzuordchandle** **sqlzuzuordcenv** zugeordnet ist. **sqlzuordcconnect**, oder **sqlzucstmt** und, da **SQLFreeHandle** **sqlfreeeinv**, **sqlfreeconnetct** oder **SQLFreeStmt** zugeordnet ist. **Sqlzuordchandle** oder **SQLFreeHandle** mit einem " *Lenker* Argument"-Argument von "SQL_HANDLE_DESC" wird jedoch nicht unterstützt, obwohl SQL_TRUE für die Funktionen zurückgegeben wird, da es in diesem Fall keine ODBC 2 *. x* -Funktion gibt, die zugeordnet werden kann.  
  
## <a name="code-example"></a>Codebeispiel  
 In den folgenden drei Beispielen wird gezeigt, wie eine Anwendung **SQLGetFunctions** verwendet, um zu bestimmen, ob ein Treiber **SQLTables**, **SQLColumns** und **SQLStatistics** unterstützt. Wenn der Treiber diese Funktionen nicht unterstützt, trennt die Anwendung die Verbindung mit dem Treiber. Im ersten Beispiel wird **SQLGetFunctions** einmal für jede Funktion aufgerufen.  
  
```cpp  
SQLUSMALLINT TablesExists, ColumnsExists, StatisticsExists;  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
  
retcodeTables = SQLGetFunctions(hdbc, SQL_API_SQLTABLES, &TablesExists);  
retcodeColumns = SQLGetFunctions(hdbc, SQL_API_SQLCOLUMNS, &ColumnsExists);  
retcodeStatistics = SQLGetFunctions(hdbc, SQL_API_SQLSTATISTICS, &StatisticsExists);  
  
// SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver.  
if (retcodeTables == SQL_SUCCESS && TablesExists == SQL_TRUE &&   
retcodeColumns == SQL_SUCCESS && ColumnsExists == SQL_TRUE &&   
retcodeStatistics == SQL_SUCCESS && StatisticsExists == SQL_TRUE)   
{  
  
   // Continue with application  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
 Im zweiten Beispiel ruft eine ODBC 3. x-Anwendung **SQLGetFunctions** auf und übergibt ihr ein Array, in dem **SQLGetFunctions** Informationen zu allen ODBC 3. x-Funktionen und früheren Funktionen zurückgibt.  
  
```cpp  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
SQLUSMALLINT fExists[SQL_API_ODBC3_ALL_FUNCTIONS_SIZE];  
  
retcode = SQLGetFunctions(hdbc, SQL_API_ODBC3_ALL_FUNCTIONS, fExists);  
  
// SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver.  
if (reccode == SQL_SUCCESS &&   
SQL_FUNC_EXISTS(fExists, SQL_API_SQLTABLES) == SQL_TRUE &&  
   SQL_FUNC_EXISTS(fExists, SQL_API_SQLCOLUMNS) == SQL_TRUE &&  
   SQL_FUNC_EXISTS(fExists, SQL_API_SQLSTATISTICS) == SQL_TRUE)   
{  
  
   // Continue with application  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
 Das dritte Beispiel ist eine ODBC 2. x-Anwendung, die **SQLGetFunctions** aufruft und ihr ein Array von 100 Elementen übergibt, in der **SQLGetFunctions** Informationen zu allen ODBC 2. x-Funktionen und früheren Funktionen zurückgibt.  
  
```cpp  
#define FUNCTIONS 100  
  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
SQLUSMALLINT fExists[FUNCTIONS];  
  
retcode = SQLGetFunctions(hdbc, SQL_API_ALL_FUNCTIONS, fExists);  
  
/* SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver. */  
if (retcode == SQL_SUCCESS &&   
fExists[SQL_API_SQLTABLES] == SQL_TRUE &&  
   fExists[SQL_API_SQLCOLUMNS] == SQL_TRUE &&  
   fExists[SQL_API_SQLSTATISTICS] == SQL_TRUE)   
{  
  
   /* Continue with application */  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
## <a name="related-functions"></a>Verwandte Funktionen  
  
|Informationen über|Finden Sie unter|  
|---------------------------|---------|  
|Zurückgeben der Einstellung eines Verbindungs Attributs|[SQLGetConnectAttr-Funktion](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Zurückgeben von Informationen zu einem Treiber oder einer Datenquelle|[SQLGetInfo-Funktion](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
|Zurückgeben der Einstellung eines Anweisungs Attributs|[SQLGetStmtAttr-Funktion](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
  
## <a name="see-also"></a>Weitere Informationen  
 [ODBC-API-Referenz](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC-Headerdateien](../../../odbc/reference/install/odbc-header-files.md)
