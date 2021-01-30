---
description: SQLSetDriverConnectInfo-Funktion
title: Sqlsetdriverconnectinfo-Funktion | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetDriverConnectInfo function [ODBC]
ms.assetid: bfd4dfc2-fbca-4ef3-81e5-2706f2389256
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: de0a21b8fa3e620f4a6884311c10c93c36518849
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174684"
---
# <a name="sqlsetdriverconnectinfo-function"></a>SQLSetDriverConnectInfo-Funktion
**Konformitäts**  
 Eingeführte Version: ODBC 3,81 Standards Compliance: ODBC  
  
 **Zusammenfassung**  
 **Sqlsetdriverconnectinfo** wird verwendet, um die Verbindungs Zeichenfolge in das Verbindungs Informations Token für den **SQLDriverConnect** -Befehl einer Anwendung festzulegen.  
  
## <a name="syntax"></a>Syntax  
  
```cpp
  
SQLRETURN SQLSetDriverConnectInfo(  
                SQLHDBC_INFO_TOKEN   hDbcInfoToken,  
                WCHAR *              InConnectionString,  
                SQLSMALLINT          StringLength1 );  
```  
  
## <a name="arguments"></a>Argumente  
 *Tokenhandle*  
 Der Tokenhandle.  
  
 *InConnectionString*  
 Der Eine vollständige Verbindungs Zeichenfolge (siehe die Syntax in "comments" in [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)), eine partielle Verbindungs Zeichenfolge oder eine leere Zeichenfolge.  
  
 *StringLength1*  
 Der Länge von **InConnectionString*, in Zeichen, wenn die Zeichenfolge Unicode ist, oder bytes, wenn die Zeichenfolge ANSI oder DBCS ist.  
  
## <a name="returns"></a>Gibt zurück  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR oder SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnose  
 Identisch mit [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) im Zusammenhang mit jedem Eingabevalidierungsfehler, mit dem Unterschied, dass der Treiber-Manager den SQL_HANDLE_DBC_INFO_TOKEN **und ein** **handle** von *hdbcinfotoken* verwendet.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn ein Treiber SQL_ERROR oder SQL_INVALID_HANDLE zurückgibt, gibt der Treiber-Manager den Fehler an die Anwendung zurück (in [SQLCONNECT](../../../odbc/reference/syntax/sqlconnect-function.md) oder [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Wenn ein Treiber SQL_SUCCESS_WITH_INFO zurückgibt, erhält der Treiber-Manager die Diagnoseinformationen von *hdbcinfotoken* und gibt SQL_SUCCESS_WITH_INFO in [SQLCONNECT](../../../odbc/reference/syntax/sqlconnect-function.md) und [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)an die Anwendung zurück.  
  
 Anwendungen sollten diese Funktion nicht direkt aufzurufen. Ein ODBC-Treiber, der Treiber fähiges Verbindungspooling unterstützt, muss diese Funktion implementieren.  
  
 Fügen Sie sqlspi. h für die ODBC-Treiberentwicklung ein.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Entwickeln eines ODBC-Treibers](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Treiber fähiges Verbindungs Pooling](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Entwickeln von Verbindungspool-Unterstützung in einem ODBC-Treiber](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
