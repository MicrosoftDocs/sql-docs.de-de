---
description: Verwenden des Visual FoxPro-ODBC-Treibers mit Ihrer C-oder Visual C++-Anwendung
title: Verwenden des Visual FoxPro-ODBC-Treibers mit C-oder Visual C++-Anwendung | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- FoxPro ODBC driver [ODBC], C or C++ applications
- C++ applications [ODBC]
- Visual FoxPro ODBC driver [ODBC], C or C++ applications
- Visual FoxPro data [ODBC], C or C++ applications
- C applications [ODBC]
ms.assetid: beb11a68-849e-4fe0-b217-d3722b1b1389
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0aa73fdf7cf224dc1c7bff6ad162c5bb18e1d6b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070385"
---
# <a name="use-the-visual-foxpro-odbc-driver-with-your-c-or-visual-c-application"></a>Verwenden des Visual FoxPro-ODBC-Treibers mit Ihrer C-oder Visual C++-Anwendung
Die C-oder C++-Anwendung kommuniziert mit den Visual FoxPro-Daten, indem eine [SQLExecute](../../odbc/microsoft/sqlexecute-visual-foxpro-odbc-driver.md) -oder [SQLExecDirect](../../odbc/microsoft/sqlexecdirect-visual-foxpro-odbc-driver.md) -Anweisung an Visual FoxPro gesendet wird. Diese Anweisung kann Folgendes enthalten:  
  
-   SQL-Anweisungen, die in der Visual FoxPro-Sprache, z. b. dem [DROP TABLE](../../odbc/microsoft/drop-table-command.md) -Befehl, einheitlich  
  
-   [Unterstützte ODBC-SQL-Grammatik](../../odbc/microsoft/supported-odbc-sql-grammar-visual-foxpro-odbc-driver.md).  
  
-   Nicht-SQL Visual FoxPro-Sprache, z. b. [unterstützte Set-Befehle](../../odbc/microsoft/supported-set-commands-visual-foxpro-odbc-driver.md).  
  
 Weitere Informationen zu SQL Native in Visual FoxPro finden Sie in der Dokumentation zu Visual FoxPro.  
  
## <a name="example-using-the-visual-foxpro-odbc-driver-with-your-c-or-c-application"></a>Beispiel: Verwenden des Visual FoxPro-ODBC-Treibers mit Ihrer C-oder C++-Anwendung  
 Im folgenden Beispiel wird die ODBC-C-API zum Abrufen von Daten verwendet, die im Feld last_name in der Employee-Tabelle der Microsoft® Visual FoxPro-Beispieldatenbank namens Tastrade gespeichert sind. Diese Datenbank wird mit Visual FoxPro bereitgestellt und wird standardmäßig an folgendem Speicherort installiert:  
  
 `c:\vfp\samples\mainsamp\data\tastrade.dbc`  
  
 Im Beispiel wird jeweils ein Nachname angezeigt, sodass Sie im Meldungs Feld auf OK klicken können, um den nächsten Nachnamen anzuzeigen. Es wird davon ausgegangen, dass eine Datenquelle mit dem Namen Tastrade für die Verwendung der Datenbank "Tastrade. DBC" eingerichtet wurde.  
  
> [!NOTE]  
>  Die Fehlerüberprüfung sollte für alle ODBC-API-Aufrufe ausgeführt werden. in diesem Beispiel wird die Fehlerüberprüfung aus Gründen der Übersichtlichkeit ausgeschlossen.  
  
```  
// FoxPro_ODBC_Driver_with_C.cpp  
// compile with: odbc32.lib user32.lib /c  
#include <windows.h>  
#include <sql.h>  
#include <sqlext.h>  
#include <stdio.h>  
#include <mbstring.h>  
  
#define MAX_DATA 100  
#define MYSQLSUCCESS(rc) ((rc==SQL_SUCCESS)||(rc==SQL_SUCCESS_WITH_INFO))  
  
class direxec {  
   RETCODE rc;        // ODBC return code  
   HENV henv;         // Environment     
   HDBC hdbc;         // Connection handle  
   HSTMT hstmt;       // Statement handle  
   unsigned char szData[MAX_DATA];   // Returned data storage  
   SDWORD cbData;     // Output length of data  
   unsigned char chr_ds_name[SQL_MAX_DSN_LENGTH];   // Data source name  
  
public:  
   direxec();           // Constructor  
   void sqlconn();      // Allocate env, stat, and conn  
   void sqlexec(unsigned char *);   // Execute SQL statement  
   void sqldisconn();   // Free pointers to env, stat, conn, and disconnect  
   void error_out();    // Displays errors  
};  
  
// Constructor initializes the string chr_ds_name with the data source name.  
direxec::direxec() {  
   _mbscpy_s(chr_ds_name, (const unsigned char *)"tastrade");  
}  
  
// Allocate environment handle, allocate connection handle,  
// connect to data source, and allocate statement handle.  
void direxec::sqlconn() {  
   SQLAllocEnv(&henv);  
   SQLAllocConnect(henv, &hdbc);  
   rc=SQLConnect(hdbc, chr_ds_name, SQL_NTS, NULL, 0, NULL, 0);  
  
   // Deallocate handles, display error message, and exit.  
   if (!MYSQLSUCCESS(rc)) {  
      SQLFreeEnv(henv);  
      SQLFreeConnect(hdbc);  
      error_out();  
      exit(-1);  
   }  
  
   rc = SQLAllocStmt(hdbc, &hstmt);  
}  
  
// Execute SQL command with SQLExecDirect() ODBC API.  
void direxec::sqlexec(unsigned char * cmdstr) {  
   rc = SQLExecDirect(hstmt, cmdstr, SQL_NTS);  
   if (!MYSQLSUCCESS(rc)) {   // Error  
      error_out();  
      // Deallocate handles and disconnect.  
      SQLFreeStmt(hstmt, SQL_DROP);  
      SQLDisconnect(hdbc);  
      SQLFreeConnect(hdbc);  
      SQLFreeEnv(henv);  
      exit(-1);  
   }  
   else {  
      for (rc = SQLFetch(hstmt) ; rc == SQL_SUCCESS; rc=SQLFetch(hstmt)) {  
         SQLGetData(hstmt, 1, SQL_C_CHAR, szData, sizeof(szData), &cbData);  
         // In this example, the data is returned in a messagebox for  
         // simplicity. However, normally the SQLBindCol() ODBC API could  
         // be called to bind individual data rows and assign for a rowset.  
         MessageBox(NULL, (const char *)szData, "ODBC", MB_OK);  
      }  
   }  
}  
  
// Free the statement handle, disconnect, free the connection handle, and  
// free the environment handle.  
void direxec::sqldisconn() {  
   SQLFreeStmt(hstmt, SQL_DROP);  
   SQLDisconnect(hdbc);  
   SQLFreeConnect(hdbc);  
   SQLFreeEnv(henv);  
}  
  
// Display error message in a message box that has an OK button.  
void direxec::error_out() {  
   unsigned char szSQLSTATE[10];  
   SDWORD nErr;  
   unsigned char msg[SQL_MAX_MESSAGE_LENGTH + 1];  
   SWORD cbmsg;  
  
   while(SQLError(0, 0, hstmt, szSQLSTATE, &nErr, msg, sizeof(msg), &cbmsg) == SQL_SUCCESS) {  
      sprintf_s((char *)szData, MAX_DATA, "Error:\nSQLSTATE=%s, Native error=%ld, msg='%s'",   
         szSQLSTATE, nErr, msg);  
  
      MessageBox(NULL, (const char *)szData, "ODBC Error", MB_OK);  
   }  
}  
  
int main (){  
   // Declare an instance of the direxec object.  
   direxec x;  
  
   // Allocate handles and connect.  
   x.sqlconn();  
  
   // Execute SQL command "SELECT last_name FROM employee".  
   x.sqlexec((UCHAR FAR *)"SELECT last_name FROM employee");  
  
   // Free handles, and disconnect.  
   x.sqldisconn();  
  
   // Return success code; example executed successfully.  
   return (TRUE);  
}  
```
