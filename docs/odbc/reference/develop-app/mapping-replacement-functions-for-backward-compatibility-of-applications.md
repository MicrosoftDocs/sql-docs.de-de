---
description: Zuordnen von Ersatzfunktionen für die Abwärtskompatibilität von Anwendungen
title: Zuordnung von Ersetzungs Funktionen für die Kompatibilität von apps (ODBC) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- mapping replacement functions [ODBC]
- upgrading applications [ODBC], mapping replacement functions
- compatibility [ODBC], mapping replacement functions
- ODBC drivers [ODBC], backward compatibility
- functions [ODBC], mapping replacement functions
- application upgrades [ODBC], mapping replacement functions
- backward compatibility [ODBC], mapping replacement functions
ms.assetid: f5e6d9da-76ef-42cb-b3f5-f640857df732
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e4c31d523a9fb50dd45bf8ab56c551d3830b0861
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082161"
---
# <a name="mapping-replacement-functions-for-backward-compatibility-of-applications"></a>Zuordnen von Ersatzfunktionen für die Abwärtskompatibilität von Anwendungen
Eine ODBC *3. x* -Anwendung, die den ODBC *3. x* -Treiber-Manager verwendet, funktioniert mit einem ODBC *2. x* -Treiber, solange keine neuen Features verwendet werden. Duplizierte Funktionen und Verhaltensänderungen wirken sich jedoch darauf aus, wie die ODBC *3. x* -Anwendung auf einem ODBC *2. x* -Treiber funktioniert. Bei der Arbeit mit einem ODBC *2. x* -Treiber ordnet der Treiber-Manager die folgenden ODBC *3. x* -Funktionen, die eine oder mehrere ODBC *2. x* -Funktionen ersetzt haben, in die entsprechenden ODBC *2. x* -Funktionen zu.  
  
|ODBC *3. x* -Funktion|ODBC *2. x* -Funktion|  
|-------------------------|-------------------------|  
|**SQLAllocHandle**|**Sqlzugecenv**, **sqlverbincconnect** oder **sqlverbincstmt**|  
|**SQLBulkOperations**|**SQLSetPos**|  
|**SQLColAttribute**|**SQLColAttributes**|  
|**SQLEndTran**|**SQLtransact**|  
|**SQLFetch**|**SQLExtendedFetch**|  
|**SQLFetchScroll**|**SQLExtendedFetch**|  
|**SQLFreeHandle**|**Sqlfreedenv**, **sqlfreeconnetct** oder **SQLFreeStmt**|  
|**SQLGetConnectAttr**|**SQLGetConnectOption**|  
|**SQLGetDiagRec**|**SQLError**|  
|**'SQLGetStmtAttr'**|**SQLGetStmtOption**[1]|  
|**SQLSetConnectAttr**|**SQLSetConnectOption**|  
|**SQLSetStmtAttr**|**SQLSetStmtOption**[1]|  
  
 [1] andere Aktionen können auch ausgeführt werden, abhängig vom angeforderten Attribut.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
 Der Treiber-Manager ordnet dies je nach Bedarf **sqlzugcenv**, **sqlzugcconnect** oder **sqlzugcstmt** zu. Der folgende Befehl von **sqlzugechandle**:  
  
```  
SQLAllocHandle(HandleType, InputHandle, OutputHandlePtr);  
```  
  
 führt dazu, dass der Treiber-Manager die folgende Zuordnung durchführt (konzeptionelle, keine Fehlerüberprüfung):  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV: return (SQLAllocEnv(OutputHandlePtr));  
   case SQL_HANDLE_DBC: return (SQLAllocConnect (InputHandle, OutputHandlePtr));  
   case SQL_HANDLE_STMT: return (SQLAllocStmt (InputHandle, OutputHandlePtr));  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlbulkoperations"></a>SQLBulkOperations  
 Der Treiber-Manager ordnet dies **SQLSetPos** zu. Der folgende **SQLBulkOperations**-Befehl:  
  
```  
SQLBulkOperations(hstmt, Operation);  
```  
  
 führt zu den folgenden Schritten:  
  
1.  Wenn das Vorgangs Argument SQL_ADD ist, ruft der Treiber-Manager **SQLSetPos** wie folgt auf:  
  
    ```  
    SQLSetPos (hstmt, 0, SQL_ADD, SQL_LOCK_NO_CHANGE);  
    ```  
  
2.  Wenn das Vorgangs Argument nicht SQL_ADD ist, gibt der Treiber SQLSTATE HY092 (Ungültiger Attribut-/Optionsbezeichner) zurück.  
  
3.  Wenn die Anwendung versucht, die SQL_ATTR_ROW_STATUS_PTR zwischen Aufrufen von **SQLFetch** oder **SQLFetchScroll** und **SQLBulkOperations** zu ändern, gibt der Treiber-Manager SQLSTATE HY011 zurück (das Attribut kann jetzt nicht festgelegt werden).  
  
4.  Wenn das Vorgangs Argument SQL_ADD ist, muss die Anwendung **SQLBindCol** aufrufen, um die einzufügenden Daten zu binden. **SQLSetDescField** oder **SQLSetDescRec** kann nicht aufgerufen werden, um die einzufügenden Daten zu binden.  
  
5.  Wenn das Vorgangs Argument SQL_ADD ist und die Anzahl der einzufügenden Zeilen nicht mit der aktuellen Rowsetgröße identisch ist, muss **SQLSetStmtAttr** aufgerufen werden, um das SQL_ATTR_ROW_ARRAY_SIZE Statement-Attribut auf die Anzahl der einzufügenden Zeilen vor dem Aufrufen von **SQLBulkOperations** festzulegen. Um zur vorherigen Rowsetgröße zurückzukehren, muss die Anwendung das SQL_ATTR_ROW_ARRAY_SIZE Anweisungs Attribut festlegen, bevor **SQLFetch**, **SQLFetchScroll** oder **SQLSetPos** aufgerufen wird.  
  
## <a name="sqlcolattribute"></a>SQLColAttribute  
 Der Treiber-Manager ordnet dies **SQLColAttribute** zu. Der folgende **SQLColAttribute**-Befehl:  
  
```  
SQLColAttribute(StatementHandle, ColumnNumber, FieldIdentifier, CharacterAttributePtr, BufferLength, StringLengthPtr, NumericAttributePtr);  
```  
  
 führt zu den folgenden Schritten:  
  
1.  Wenn *fieldidentifier* eine der folgenden ist:  
  
     SQL_DESC_PRECISION, SQL_DESC_SCALE, SQL_DESC_LENGTH, SQL_DESC_OCTET_LENGTH, SQL_DESC_UNNAMED, SQL_DESC_BASE_COLUMN_NAME, SQL_DESC_LITERAL_PREFIX, SQL_DESC_LITERAL_SUFFIX oder SQL_DESC_LOCAL_TYPE_NAME  
  
     der Treiber-Manager gibt SQL_ERROR mit SQLSTATE HY091 (Ungültiger Deskriptorfeldbezeichner) zurück. Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
2.  Der Treiber-Manager ordnet SQL_COLUMN_COUNT, SQL_COLUMN_NAME oder SQL_COLUMN_NULLABLE SQL_DESC_COUNT, SQL_DESC_NAME bzw. SQL_DESC_NULLABLE zu. (Ein ODBC *2. x* -Treiber muss nur SQL_COLUMN_COUNT, SQL_COLUMN_NAME und SQL_COLUMN_NULLABLE unterstützen, nicht SQL_DESC_COUNT, SQL_DESC_NAME und SQL_DESC_NULLABLE.) Der SQLColAttribute-Befehl wird zugeordnet zu:  
  
    ```  
    SQLColAttributes(StatementHandle, ColumnNumber, FieldIdentifier, CharacterAttributePtr, BufferLength, StringLengthPtr, NumericAttributePtr);  
    ```  
  
3.  Alle anderen *fieldidentifier* -Werte werden an den Treiber übergeben, wobei **SQLColAttribute** wie zuvor gezeigt **SQLColAttribute** zugeordnet ist.  
  
4.  Wenn *BufferLength* kleiner als 0 ist, gibt der Treiber-Manager SQL_ERROR mit SQLSTATE HY090 (ungültige Zeichen folgen-oder Pufferlänge) zurück. Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
5.  Wenn *fieldidentifier* SQL_DESC_CONCISE_TYPE und der zurückgegebene Typ ein präziser DateTime-Datentyp ist, ordnet der Treiber-Manager die Rückgabewerte für Datums-, Uhrzeit-und Zeitstempel Codes zu.  
  
6.  Der Treiber-Manager führt die erforderlichen Überprüfungen durch, um festzustellen, ob SQLSTATE HY010 (Funktions Sequenz Fehler) ausgelöst werden muss. Wenn dies der Fall ist, gibt der Treiber-Manager SQL_ERROR und SQLSTATE HY010 (Funktions Sequenz Fehler) zurück. Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
## <a name="sqlendtran"></a>SQLEndTran  
 Der Treiber-Manager ordnet dies **SQLTransact** zu. Der folgende **SQLEndTran**-aufrufsbefehl:  
  
```  
SQLEndTran(HandleType, Handle, CompletionType);  
```  
  
 führt dazu, dass der Treiber-Manager die folgende Zuordnung durchführt (konzeptionelle, keine Fehlerüberprüfung):  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV:return(SQLTransact(Handle, SQL_NULL_HDBC, CompletionType));  
   case SQL_HANDLE_DBC:return(SQLTransact(SQL_NULL_HENV, Handle, CompletionType);  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlfetch"></a>SQLFetch  
 Der Treiber-Manager ordnet dies **sqlextendecodfetch** mit einem *FetchOrientation* -Argument SQL_FETCH_NEXT zu. Der folgende **SQLFetch**-Befehl:  
  
```  
SQLFetch (StatementHandle);  
```  
  
 führt dazu, dass der Treiber-Manager **SQLExtendedFetch** wie folgt aufruft:  
  
```  
rc = SQLExtendedFetch(StatementHandle, FetchOrientation, FetchOffset, &RowCount, RowStatusArray);  
```  
  
 In diesem-Befehl wird das *pcrow* -Argument auf den Wert festgelegt, auf den die Anwendung das SQL_ATTR_ROWS_FETCHED_PTR-Anweisungs Attribut durch einen **SQLSetStmtAttr**-Befehl festlegt.  
  
> [!NOTE]  
>  Wenn die Anwendung **SQLSetStmtAttr** aufruft, um SQL_ATTR_ROW_STATUS_PTR so festzulegen, dass Sie auf ein Status Array verweist, speichert der Treiber-Manager den Zeiger zwischen. *Rowstatus Array* kann gleich einem NULL-Zeiger sein.  
  
 Wenn der Treiber **SQLExtendedFetch** nicht unterstützt und die Cursor Bibliothek geladen wird, verwendet der Treiber-Manager das **SQLExtendedFetch** der Cursor Bibliothek, um **SQLFetch** **SQLExtendedFetch** zuzuordnen. Wenn der Treiber **SQLExtendedFetch** nicht unterstützt und die Cursor Bibliothek nicht geladen ist, übergibt der Treiber-Manager den **SQLFetch** -Befehl an den Treiber. Wenn die Anwendung **SQLSetStmtAttr** aufruft, um SQL_ATTR_ROW_STATUS_PTR festzulegen, stellt der Treiber-Manager sicher, dass das Array aufgefüllt wird. Wenn die Anwendung **SQLSetStmtAttr** aufruft, um SQL_ATTR_ROWS_FETCHED_PTR festzulegen, legt der Treiber-Manager dieses Feld auf 1 fest.  
  
## <a name="sqlfetchscroll"></a>SQLFetchScroll  
 Der Treiber-Manager ordnet dies **sqlextendecodfetch** zu. Der folgende **SQLFetchScroll**-Befehl:  
  
```  
SQLFetchScroll(StatementHandle, FetchOrientation, FetchOffset);  
```  
  
 führt zu den folgenden Schritten:  
  
1.  Wenn die Anwendung **SQLSetStmtAttr** aufruft, um SQL_ATTR_ROW_STATUS_PTR festzulegen (wodurch das SQL_DESC_ARRAY_STATUS_PTR Feld im IRD festgelegt wird), um auf ein Status Array zu verweisen, speichert der Treiber-Manager diesen Zeiger zwischen. Dieser Zeiger ist " *rowstatus Array*;". Andernfalls lassen Sie *rowstatus Array* auf einen NULL-Zeiger fest. Wenn das *rowstatusarray* -Argument auf einen NULL-Zeiger festgelegt ist, generiert der Treiber-Manager ein Row-Status-Array.  
  
2.  Wenn *FetchOrientation* nicht eine der SQL_FETCH_NEXT, SQL_FETCH_PRIOR, SQL_FETCH_ABSOLUTE, SQL_FETCH_RELATIVE, SQL_FETCH_FIRST, SQL_FETCH_LAST oder SQL_FETCH_BOOKMARK ist, gibt der Treiber-Manager mit SQL_ERROR und SQLSTATE HY106 (fetch Type außerhalb des gültigen Bereichs) zurück. Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
3.  Schreibweise:  
  
-   Wenn *FetchOrientation* gleich SQL_FETCH_BOOKMARK ist, gilt Folgendes:  
  
    -   Wenn **SQLSetStmtAttr** zuvor aufgerufen wurde, um den Wert SQL_ATTR_FETCH_BOOKMARK_PTR festzulegen, sollte *BMK* der Wert sein, der durch Dereferenzierung des Zeiger SQL_DESC_FETCH_BOOKMARK_PTR abgerufen wird.  
  
    -   Andernfalls wird SQL_ERROR mit SQLSTATE HY111 (Ungültiger Lesezeichen Wert) zurückgegeben. Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
     Der Treiber-Manager ruft nun **SQLExtendedFetch** wie folgt auf:  
  
    ```  
    rc = SQLExtendedFetch(StatementHandle, FetchOrientation, Bmk, pcRow, RowStatusArray);  
    ```  
  
-   Andernfalls ruft der Treiber-Manager **SQLExtendedFetch** wie folgt auf:  
  
    ```  
    rc = SQLExtendedFetch(StatementHandle, FetchOrientation, FetchOffset, pcRow, RowStatusArray);  
    ```  
  
     In diesen aufrufen wird das *pcrow* -Argument auf den Wert festgelegt, den die Anwendung durch einen Aufruf von **SQLSetStmtAttr** auf das SQL_ATTR_ROWS_FETCHED_PTR Statement-Attribut festlegt.  
  
-   SQL_ATTR_ROW_ARRAY_SIZE ist SQL_ROWSET_SIZE zugeordnet.  
  
-   Wenn *RC* gleich SQL_SUCCESS oder SQL_SUCCESS_WITH_INFO ist und " *FetchOrientation* " gleich SQL_FETCH_BOOKMARK und " *FetchOffset* " nicht gleich 0 ist, sendet der Treiber-Manager eine Warnung, SQLSTATE 01s10 (der Versuch, einen Lesezeichen Offset zu erhalten, der Offset Wert wird ignoriert) und gibt SQL_SUCCESS_WITH_INFO zurück.  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
 Der Treiber-Manager ordnet dies **sqlfreeerv**, **sqlfreeconnect** oder **SQLFreeStmt** zu. Der folgende **SQLFreeHandle**-Befehl:  
  
```  
SQLFreeHandle(HandleType, Handle);  
```  
  
 führt dazu, dass der Treiber-Manager die folgende Zuordnung durchführt (konzeptionelle, keine Fehlerüberprüfung):  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV: return (SQLFreeEnv(Handle));  
   case SQL_HANDLE_DBC: return (SQLFreeConnect(Handle));  
   case SQL_HANDLE_STMT: return (SQLFreeStmt(Handle, SQL_DROP));  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
 Der Treiber-Manager ordnet dies **SQLGetConnectOption** zu. Der folgende Befehl von **SQLGetConnectAttr**:  
  
```  
SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength, StringLengthPtr);  
```  
  
 führt zu den folgenden Schritten:  
  
1.  Wenn das *Attribut* kein Treiber definiertes Verbindungs-oder Anweisungs Attribut ist und kein Attribut ist, das in ODBC *2. x* definiert ist, gibt der Treiber-Manager SQL_ERROR mit SQLState HY092 (Ungültiger Attribut-/Optionsbezeichner) zurück. In diesem Abschnitt sind keine weiteren Regeln anwendbar.  
  
2.  Wenn *Attribute* gleich SQL_ATTR_AUTO_IPD oder SQL_ATTR_METADATA_ID ist, gibt der Treiber-Manager SQL_ERROR mit SQLState HY092 zurück (Ungültiger Attribut-/Optionsbezeichner).  
  
3.  Der Treiber-Manager führt die erforderlichen Überprüfungen durch, um festzustellen, ob SQLSTATE 08003 (Verbindung nicht geöffnet) oder SQLSTATE HY010 (Funktions Sequenz Fehler) ausgelöst werden muss. Wenn dies der Fall ist, gibt der Treiber-Manager SQL_ERROR zurück und gibt die entsprechende Fehlermeldung aus. Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
4.  Der Treiber-Manager ruft **SQLGetConnectOption** wie folgt auf:  
  
    ```  
    SQLGetConnectOption (ConnectionHandle, Attribute, ValuePtr);  
    ```  
  
     Beachten Sie, dass *BufferLength* und *stringlengthptr* ignoriert werden.  
  
## <a name="sqlgetdata"></a>SQLGetData  
 Wenn eine ODBC *3. x* -Anwendung, die mit einem ODBC *2. x* -Treiber arbeitet, **SQLGetData** mit dem *ColumnNumber* -Argument gleich 0 aufruft, ordnet der ODBC *3. x* -Treiber-Manager dies einem Aufruf von **SQLGetStmtOption** zu, wobei das *options* -Attribut auf SQL_GET_BOOKMARK festgelegt ist.  
  
## <a name="sqlgetstmtattr"></a>'SQLGetStmtAttr'  
 Der Treiber-Manager ordnet dies **SQLGetStmtOption** zu. Der folgende **SQLGetStmtAttr**-Befehl:  
  
```  
SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, BufferLength, StringLengthPtr);  
```  
  
 führt zu den folgenden Schritten:  
  
1.  Wenn das *Attribut* kein Treiber definiertes Verbindungs-oder Anweisungs Attribut ist und kein Attribut ist, das in ODBC *2. x* definiert ist, gibt der Treiber-Manager SQL_ERROR mit SQLState HY092 (Ungültiger Attribut-/Optionsbezeichner) zurück. In diesem Abschnitt sind keine weiteren Regeln anwendbar.  
  
2.  Wenn das *Attribut* eines der folgenden ist:  
  
     SQL_ATTR_APP_ROW_DESC  
  
     SQL_ATTR_APP_PARAM_DESC  
  
     SQL_ATTR_AUTO_IPD  
  
     SQL_ATTR_ROW_BIND_TYPE  
  
     SQL_ATTR_IMP_ROW_DESC  
  
     SQL_ATTR_IMP_PARAM_DESC  
  
     SQL_ATTR_METADATA_ID  
  
     SQL_ATTR_PARAM_BIND_TYPE  
  
     SQL_ATTR_PREDICATE_PTR  
  
     SQL_ATTR_PREDICATE_OCTET_LENGTH_PTR  
  
     SQL_ATTR_PARAM_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_OPERATION_PTR  
  
     SQL_ATTR_PARAM_OPERATION_PTR  
  
     der Treiber-Manager gibt SQL_ERROR mit SQLState HY092 zurück (Ungültiger Attribut-/Optionsbezeichner). Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
3.  Der Treiber-Manager führt die erforderlichen Überprüfungen durch, um festzustellen, ob SQLSTATE HY010 (Funktions Sequenz Fehler) ausgelöst werden muss. Wenn dies der Fall ist, gibt der Treiber-Manager SQL_ERROR und SQLSTATE HY010 (Funktions Sequenz Fehler) zurück. Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
4.  Wenn *Attribute* gleich SQL_ATTR_ROWS_FETCHED_PTR *ist, gibt* der Treiber-Manager einen Zeiger auf die interne variierungszerlegung des Treiber-Managers zurück, die er verwendet oder in einem **sqlextendebug**-aufrufsvorgang verwendet. Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
5.  Wenn *Attribute* gleich SQL_DESC_FETCH_BOOKMARK_PTR ist, gibt der Treiber-Manager den entsprechenden Zeiger zurück, den er während eines Aufrufes von **SQLSetStmtAttr** zwischengespeichert hat.  
  
6.  Wenn *Attribute* gleich SQL_ATTR_ROW_STATUS_PTR ist, gibt der Treiber-Manager den entsprechenden Zeiger zurück, den er während eines Aufrufes von **SQLSetStmtAttr** zwischengespeichert hat.  
  
7.  Der Treiber-Manager ruft **SQLGetStmtOption** wie folgt auf:  
  
    ```  
    SQLGetStmtOption (hstmt, fOption, pvParam);  
    ```  
  
     Dabei werden *hstmt*, *fOption* und *pvParam* auf die Werte von *StatementHandle*, *Attribute* bzw. *ValuePtr* festgelegt. *BufferLength* und *stringlengthptr* werden ignoriert.  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
 Der Treiber-Manager ordnet dies **SQLSetConnectOption** zu. Der folgende Befehl von **SQLSetConnectAttr**:  
  
```  
SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, StringLength);  
```  
  
 führt zu den folgenden Schritten:  
  
1.  Wenn das *Attribut* kein Treiber definiertes Verbindungs-oder Anweisungs Attribut ist und kein Attribut ist, das in ODBC *2. x* definiert ist, gibt der Treiber-Manager SQL_ERROR mit SQLState HY092 (Ungültiger Attribut-/Optionsbezeichner) zurück. In diesem Abschnitt sind keine weiteren Regeln anwendbar.  
  
2.  Wenn *Attribute* gleich SQL_ATTR_AUTO_IPD ist, gibt der Treiber-Manager SQL_ERROR mit SQLState HY092 (Ungültiger Attribut-/Optionsbezeichner) zurück.  
  
3.  Der Treiber-Manager führt die erforderlichen Überprüfungen durch, um festzustellen, ob SQLSTATE 08003 (Verbindung nicht geöffnet) oder SQLSTATE HY010 (Funktions Sequenz Fehler) ausgelöst werden muss. Wenn einer dieser Fehler ausgelöst werden muss, gibt der Treiber-Manager SQL_ERROR zurück und gibt die entsprechende Fehlermeldung aus. Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
4.  Der Treiber-Manager ruft **SQLSetConnectOption** wie folgt auf:  
  
    ```  
    SQLSetConnectOption (hdbc, fOption, vParam);  
    ```  
  
     Dabei werden *hdbc*, *fOption* und *vParam* auf die Werte von *connectionHandle*, *Attribute* bzw. *ValuePtr* festgelegt. *Stringlengthptr* wird ignoriert.  
  
> [!NOTE]  
>  Die Möglichkeit, Anweisungs Attribute auf der Verbindungs Ebene festzulegen, wurde als veraltet markiert. Anweisungs Attribute sollten nie von einer ODBC *3. x* -Anwendung auf der Verbindungs Ebene festgelegt werden.  
  
## <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
 Der Treiber-Manager ordnet dies **SQLSetStmtOption** zu. Der folgende Befehl von **SQLSetStmtAttr**:  
  
```  
SQLSetStmtAttr(StatementHandle, Attribute, ValuePtr, StringLength);  
```  
  
 führt zu den folgenden Schritten:  
  
1.  Wenn das *Attribut* kein Treiber definiertes Verbindungs-oder Anweisungs Attribut ist und kein Attribut ist, das in ODBC *2. x* definiert ist, gibt der Treiber-Manager SQL_ERROR mit SQLState HY092 (Ungültiger Attribut-/Optionsbezeichner) zurück. In diesem Abschnitt sind keine weiteren Regeln anwendbar.  
  
2.  Wenn das *Attribut* eines der folgenden ist:  
  
     SQL_ATTR_APP_ROW_DESC  
  
     SQL_ATTR_APP_PARAM_DESC  
  
     SQL_ATTR_AUTO_IPD  
  
     SQL_ATTR_ROW_BIND_TYPE  
  
     SQL_ATTR_IMP_ROW_DESC  
  
     SQL_ATTR_IMP_PARAM_DESC  
  
     SQL_ATTR_METADATA_ID  
  
     SQL_ATTR_PARAM_BIND_TYPE  
  
     SQL_ATTR_PREDICATE_PTR  
  
     SQL_ATTR_PREDICATE_OCTET_LENGTH_PTR  
  
     SQL_ATTR_PARAM_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_OPERATION_PTR  
  
     SQL_ATTR_PARAM_OPERATION_PTR  
  
     der Treiber-Manager gibt SQL_ERROR mit SQLState HY092 zurück (Ungültiger Attribut-/Optionsbezeichner). Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
3.  Der Treiber-Manager führt die erforderlichen Überprüfungen durch, um festzustellen, ob SQLSTATE HY010 (Funktions Sequenz Fehler) ausgelöst werden muss. Wenn dies der Fall ist, gibt der Treiber-Manager SQL_ERROR und SQLSTATE HY010 (Funktions Sequenz Fehler) zurück. Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
4.  Wenn *Attribute* gleich SQL_ATTR_PARAMSET_SIZE oder SQL_ATTR_PARAMS_PROCESSED_PTR sind, finden Sie weitere Informationen im Abschnitt "Zuordnungen zum Behandeln von Parameter Arrays" weiter unten in diesem Thema. Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
5.  Wenn *Attribute* gleich SQL_ATTR_ROWS_FETCHED_PTR ist, speichert der Treiber-Manager den Zeiger Wert für die spätere Verwendung mit **SQLFetchScroll** zwischen.  
  
6.  Wenn *Attribute* gleich SQL_ATTR_ROW_STATUS_PTR ist, speichert der Treiber-Manager den Zeiger Wert für die spätere Verwendung mit **SQLFetchScroll** oder **SQLSetPos** zwischen. Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
7.  Wenn *Attribute* gleich SQL_ATTR_FETCH_BOOKMARK_PTR ist, speichert der Treiber-Manager *ValuePtr* zwischen und verwendet den zwischengespeicherten Wert später in einem Befehl von **SQLFetchScroll**. Es sind keine weiteren Regeln dieses Abschnitts anwendbar.  
  
8.  Der Treiber-Manager ruft **SQLSetStmtOption** wie folgt auf:  
  
    ```  
    SQLSetStmtOption (hstmt, fOption, vParam);  
    ```  
  
     Dabei werden *hstmt*, *fOption* und *vParam* auf die Werte von *StatementHandle*, *Attribute* bzw. *ValuePtr* festgelegt. Das *StringLength* -Argument wird ignoriert.  
  
     Wenn ein ODBC *2. x* -Treiber Zeichen folgen-, Treiber spezifische Anweisungs Optionen unterstützt, sollte eine ODBC *3. x* -Anwendung **SQLSetStmtOption** aufrufen, um diese Optionen festzulegen.  
  
## <a name="mappings-for-handling-parameter-arrays"></a>Zuordnungen zum Behandeln von Parameter Arrays  
 Wenn die Anwendung aufruft:  
  
```  
SQLSetStmtAttr (StatementHandle, SQL_ATTR_PARAMSET_SIZE, Size, StringLength);  
```  
  
 der Treiber-Manager ruft auf:  
  
```  
SQLParamOptions (StatementHandle, Size, &RowCount);  
```  
  
 Der Treiber-Manager gibt später einen Zeiger auf diese Variable zurück, wenn die Anwendung **SQLGetStmtAttr** aufruft, um SQL_ATTR_PARAMS_PROCESSED_PTR abzurufen. Der Treiber-Manager kann diese interne Variable erst ändern, wenn das Anweisungs Handle an den vorbereiteten oder zugewiesenen Zustand zurückgegeben wurde.  
  
 Eine ODBC *3. x* -Anwendung kann **SQLGetStmtAttr** aufrufen, um den Wert von SQL_ATTR_PARAMS_PROCESSED_PTR zu erhalten, obwohl das SQL_DESC_ARRAY_SIZE Feld nicht explizit in der APD festgelegt wurde. Diese Situation könnte z. b. auftreten, wenn die Anwendung über eine generische Routine verfügt, die auf die aktuelle "Zeile" der zu verarbeitenden Parameter prüft, wenn **SQLExecute** SQL_NEED_DATA zurückgibt. Diese Routine wird unabhängig davon aufgerufen, ob die SQL_DESC_ARRAY_SIZE 1 oder größer als 1 ist. Um dies zu berücksichtigen, muss der Treiber-Manager diese interne Variable definieren, unabhängig davon, ob die Anwendung **SQLSetStmtAttr** aufgerufen hat, um das SQL_DESC_ARRAY_SIZE Feld in APD festzulegen. Wenn SQL_DESC_ARRAY_SIZE nicht festgelegt wurde, muss der Treiber-Manager sicherstellen, dass diese Variable vor der Rückgabe von **SQLExecDirect** oder **SQLExecute** den Wert 1 enthält.  
  
## <a name="error-handling"></a>Fehlerbehandlung  
 In ODBC *3. x* füllt das Aufrufen von **SQLFetch** oder **SQLFetchScroll** die SQL_DESC_ARRAY_STATUS_PTR im IRD auf, und das SQL_DIAG_ROW_NUMBER Feld eines angegebenen Diagnosedaten Satzes enthält die Nummer der Zeile im Rowset, für die sich dieser Datensatz bezieht. Dabei kann die Anwendung eine Fehlermeldung mit einer angegebenen Zeilen Position korrelieren.  
  
 Ein ODBC *2. x* -Treiber kann diese Funktion nicht bereitstellen. Allerdings wird die Fehler Abgrenzung mit SQLSTATE 01s01 (Fehler in Zeile) bereitgestellt. Eine ODBC *3. x* -Anwendung, die **SQLFetch** oder **SQLFetchScroll** verwendet, während ein ODBC *2. x* -Treiber verwendet wird, muss diese Tatsache beachten. Beachten Sie außerdem, dass eine solche Anwendung nicht in der Lage ist, **SQLGetDiagField** aufzurufen, damit das SQL_DIAG_ROW_NUMBER Feld überhaupt nicht abgerufen wird. Eine ODBC *3. x* -Anwendung, die mit einem ODBC *2. x* -Treiber arbeitet, kann **SQLGetDiagField** nur mit einem *DiagIdentifier* -Argument von SQL_DIAG_MESSAGE_TEXT, SQL_DIAG_NATIVE, SQL_DIAG_RETURNCODE oder SQL_DIAG_SQLSTATE aufrufen. Der Treiber-Manager von ODBC *3. x* verwaltet die Diagnosedaten Struktur bei der Arbeit mit einem ODBC *2. x* -Treiber, aber der ODBC *2. x* -Treiber gibt nur diese vier Felder zurück.  
  
 Wenn eine ODBC *2. x* -Anwendung mit einem ODBC *2. x* -Treiber arbeitet und ein Vorgang dazu führen kann, dass mehrere Fehler vom Treiber-Manager zurückgegeben werden, können vom ODBC *3. x* -Treiber-Manager andere Fehler zurückgegeben werden als der ODBC *2. x* -Treiber-Manager.  
  
## <a name="mappings-for-bookmark-operations"></a>Zuordnungen für Lesezeichen Vorgänge  
 Der Treiber-Manager für ODBC *3. x* führt die folgenden Zuordnungen aus, wenn eine ODBC *3. x* -Anwendung, die mit einem ODBC *2. x* -Treiber arbeitet, Lesezeichen Vorgänge ausführt.  
  
### <a name="sqlbindcol"></a>SQLBindCol  
 Wenn eine ODBC *3. x* -Anwendung, die mit einem ODBC *2. x* -Treiber arbeitet, **SQLBindCol** aufruft, um eine Bindung an Spalte 0 herzustellen, wobei *fctype* gleich SQL_C_VARBOOKMARK ist, überprüft der ODBC *3. x* -Treiber-Manager, ob das *BufferLength* -Argument kleiner als 4 oder größer als 4 ist Wenn das *BufferLength* -Argument gleich 4 ist, ruft der Treiber-Manager **SQLBindCol** im Treiber auf, nachdem *fctype* durch SQL_C_BOOKMARK ersetzt wurde.  
  
### <a name="sqlcolattribute"></a>SQLColAttribute  
 Wenn eine ODBC *3. x* -Anwendung, die mit einem ODBC *2. x* -Treiber arbeitet, **SQLColAttribute** aufruft, bei dem das *ColumnNumber* -Argument auf 0 festgelegt ist, gibt der Treiber-Manager die in der folgenden Tabelle aufgeführten *fieldidentifier* -Werte zurück.  
  
|*FieldIdentifier*|Wert|  
|-----------------------|-----------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQL_FALSE|  
|SQL_DESC_CASE_SENSITIVE|SQL_FALSE|  
|SQL_DESC_CATALOG_NAME|"" (leere Zeichenfolge)|  
|SQL_DESC_CONCISE_TYPE|SQL_BINARY|  
|SQL_DESC_COUNT|Der von **SQLNumResultCols** zurückgegebene Wert.|  
|SQL_DESC_DATETIME_INTERVAL_CODE|0|  
|SQL_DESC_DISPLAY_SIZE|8|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_FALSE|  
|SQL_DESC_LABEL|"" (leere Zeichenfolge)|  
|SQL_DESC_LENGTH|0|  
|SQL_DESC_LITERAL_PREFIX|"" (leere Zeichenfolge)|  
|SQL_DESC_LITERAL_SUFFIX|"" (leere Zeichenfolge)|  
|SQL_DESC_LOCAL_TYPE_NAME|"" (leere Zeichenfolge)|  
|SQL_DESC_NAME|"" (leere Zeichenfolge)|  
|SQL_DESC_NULLABLE|SQL_NO_NULLS|  
|SQL_DESC_OCTET_LENGTH|4|  
|SQL_DESC_PRECISION|4|  
|SQL_DESC_SCALE|0|  
|SQL_DESC_SCHEMA_NAME|"" (leere Zeichenfolge)|  
|SQL_DESC_SEARCHABLE|SQL_PRED_NONE|  
|SQL_DESC_TABLE_NAME|"" (leere Zeichenfolge)|  
|SQL_DESC_TYPE|SQL_BINARY|  
|SQL_DESC_TYPE_NAME|"" (leere Zeichenfolge)|  
|SQL_DESC_UNNAMED|SQL_UNNAMED|  
|SQL_DESC_UNSIGNED|SQL_FALSE|  
|SQL_DESC_UPDATEABLE|SQL_ATTR_READ_ONLY|  
  
### <a name="sqldescribecol"></a>SQLDescribeCol  
 Wenn eine ODBC *3. x* -Anwendung, die mit einem ODBC *2. x* -Treiber arbeitet, **SQLDescribeCol** aufruft, bei dem das *ColumnNumber* -Argument auf 0 festgelegt ist, gibt der Treiber-Manager die in der folgenden Tabelle aufgeführten Werte zurück.  
  
|Buffer|Wert|  
|------------|-----------|  
|ColumnName|"" (leere Zeichenfolge)|  
|* Namelengthptr|0|  
|* DataTypePtr|SQL_BINARY|  
|* ColumnSizePtr|4|  
|* Decimaldigitsptr|0|  
|* Nullableptr|SQL_NO_NULLS|  
  
### <a name="sqlgetdata"></a>SQLGetData  
 Wenn eine ODBC *3. x* -Anwendung, die mit einem ODBC *2. x* -Treiber arbeitet, den folgenden **SQLGetData** -Befehl aufruft, um ein Lesezeichen abzurufen:  
  
```  
SQLGetData(StatementHandle, 0, SQL_C_VARBOOKMARK, TargetValuePtr, BufferLength, StrLen_or_IndPtr)  
```  
  
 der-Befehl wird **SQLGetStmtOption** mit der *fOption* SQL_GET_BOOKMARK wie folgt zugeordnet:  
  
```  
SQLGetStmtOption(hstmt, SQL_GET_BOOKMARK, TargetValuePtr)  
```  
  
 Dabei werden *hstmt* und *pvParam* auf die Werte in *StatementHandle* bzw. *targetvalueptr* festgelegt. Das Lesezeichen wird in dem Puffer zurückgegeben, auf den das *pvParam* (*targetvalueptr*)-Argument zeigt. Der Wert im Puffer, auf den das *StrLen_or_IndPtr* -Argument im **SQLGetData** -Befehl verweist, ist auf 4 festgelegt.  
  
 Diese Zuordnung ist erforderlich, um den Fall zu berücksichtigen, in dem **SQLFetch** vor dem Aufruf von **SQLGetData** aufgerufen wurde und der ODBC *2. x* -Treiber **SQLExtendedFetch** nicht unterstützte. In diesem Fall würde **SQLFetch** an den ODBC *2. x* -Treiber weitergeleitet werden. in diesem Fall wird das Abrufen von Lesezeichen nicht unterstützt.  
  
 **SQLGetData** kann nicht mehrmals in einem ODBC *2. x* -Treiber aufgerufen werden, um ein Lesezeichen in Teilen abzurufen. der Aufruf von **SQLGetData** mit dem *BufferLength* -Argument, das auf einen Wert kleiner als 4 festgelegt ist, und das auf 0 festgelegte *ColumnNumber* -Argument gibt SQLSTATE HY090 (ungültige Zeichenfolge oder Pufferlänge) zurück. **SQLGetData** kann jedoch mehrmals aufgerufen werden, um das gleiche Lesezeichen abzurufen.  
  
### <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
 Wenn eine ODBC *3. x* -Anwendung, die mit einem ODBC *2. x* -Treiber arbeitet, **SQLSetStmtAttr** aufruft, um das SQL_ATTR_USE_BOOKMARKS-Attribut auf SQL_UB_VARIABLE festzulegen, legt der Treiber-Manager das-Attribut auf SQL_UB_ON im zugrunde liegenden ODBC *2. x* -Treiber fest.
