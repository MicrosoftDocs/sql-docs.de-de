---
description: SQLPoolConnect-Funktion
title: Sqlpoolconnect-Funktion | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLPoolConnect function [ODBC]
ms.assetid: 41322737-890d-4a81-aed2-06cc3d546962
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8317e159a5a438bb22e8077f3a6635a79f9baefd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204561"
---
# <a name="sqlpoolconnect-function"></a>SQLPoolConnect-Funktion
**Konformitäts**  
 Eingeführte Version: ODBC 3,8 Standards Compliance: ODBC  
  
 **Zusammenfassung**  
 **Sqlpoolconnect** wird verwendet, um eine neue Verbindung zu erstellen, wenn keine Verbindung im Pool wieder verwendet werden kann.  
  
## <a name="syntax"></a>Syntax  
  
```cpp
  
SQLRETURN  SQLPoolConnect(  
                SQLHDBC              hDbc,  
                SQLHDBC_INFO_TOKEN   hDbcInfoToken,  
                WCHAR *              wszOutConnectString,  
                SQLSMALLINT          cchConnectStringBuffer,  
                SQLSMALLINT *        cchConnectStringLen );  
```  
  
## <a name="arguments"></a>Argumente  
 *hdbc*  
 Der Das Verbindungs Handle.  
  
 *hdbcinfotoken*  
 Der Das Tokenhandle für die neue Anwendungs Verbindungsanforderung.  
  
 *wszoutconnectstring*  
 Ausgeben Zeiger auf einen Puffer für die abgeschlossene Verbindungs Zeichenfolge. Bei erfolgreicher Verbindung mit der Ziel Datenquelle enthält dieser Puffer die abgeschlossene Verbindungs Zeichenfolge. Anwendungen sollten für diesen Puffer mindestens 1.024 Zeichen zuordnen.  
  
 Wenn *wszoutconnectstring* gleich NULL ist, gibt *cchconnectstringlen* weiterhin die Gesamtzahl der Zeichen zurück (ausgenommen des NULL-Beendigungs Zeichens für Zeichendaten), das im Puffer zurückgegeben werden kann, auf den von *wszoutconnectstring* gezeigt wird.  
  
 *cchconnectstringbuffer*  
 Der Länge des **wszoutconnectstring* -Puffers in Zeichen.  
  
 *cchconnectstringlen*  
 Ausgeben Ein Zeiger auf einen Puffer, in dem die Gesamtzahl der Zeichen (mit Ausnahme des NULL-Beendigungs Zeichens) zurückgegeben werden soll, die in \* *wszoutconnectstring* zurückgegeben werden soll. Wenn die Anzahl der zurück zugebende Zeichen größer oder gleich *cchconnectstringbuffer* ist, wird die abgeschlossene Verbindungs Zeichenfolge in \* *wszoutconnectstring* auf *cchconnectstringbuffer* abzüglich der Länge eines NULL-Beendigungs Zeichens gekürzt.  
  
## <a name="returns"></a>Gibt zurück  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR oder SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnose  
 Vergleichbar mit [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) für jeden Eingabevalidierungsfehler, mit dem Unterschied, dass der Treiber-Manager den **Handlertyp** SQL_HANDLE_DBC_INFO_TOKEN und ein **handle** von *hdbcinfotoken* verwendet.  
  
## <a name="remarks"></a>Bemerkungen  
 Der Treiber-Manager garantiert, dass das übergeordnete HENV-Handle von *hdbc* und *hdbcinfotoken* identisch sind.  
  
 Anders als bei [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)gibt es kein *DriverCompletion* -Argument, um Benutzer zur Eingabe von Verbindungsinformationen aufzufordern. Ein Aufforderungs Dialogfeld ist im Pooling-Szenario nicht zulässig.  
  
 Anwendungen sollten diese Funktion nicht direkt aufzurufen. Ein ODBC-Treiber, der Treiber fähiges Verbindungspooling unterstützt, muss diese Funktion implementieren.  
  
 Wenn ein Treiber SQL_ERROR oder SQL_INVALID_HANDLE zurückgibt, gibt der Treiber-Manager den Fehler an die Anwendung zurück (in [SQLCONNECT](../../../odbc/reference/syntax/sqlconnect-function.md) oder [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Wenn ein Treiber SQL_SUCCESS_WITH_INFO zurückgibt, erhält der Treiber-Manager die Diagnoseinformationen von *hdbcinfotoken* und gibt SQL_SUCCESS_WITH_INFO in [SQLCONNECT](../../../odbc/reference/syntax/sqlconnect-function.md) und [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)an die Anwendung zurück.  
  
 Wenn eine Anwendung [SQLCONNECT](../../../odbc/reference/syntax/sqlconnect-function.md)verwendet, ist *wszoutconnectstring* ein NULL-Puffer (die letzten drei Parameter werden alle auf NULL, 0 und NULL festgelegt). Andernfalls muss der Treiber die Ausgabe Verbindungs Zeichenfolge zurückgeben, die an den [SQLDriverConnect-Funktions](../../../odbc/reference/syntax/sqldriverconnect-function.md) Aufrufder Anwendung zurückgegeben wird.  
  
 Fügen Sie sqlspi. h für die ODBC-Treiberentwicklung ein.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Entwickeln eines ODBC-Treibers](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Treiber fähiges Verbindungs Pooling](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Entwickeln von Verbindungspool-Unterstützung in einem ODBC-Treiber](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
