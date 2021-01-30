---
description: SQLGetPoolID-Funktion
title: Sqlgetpoolid-Funktion | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetPoolID function [ODBC]
ms.assetid: 95a8666a-ad68-4d89-bf65-f2cc797f8820
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e7bd06a289eab6499995e40a3c6ad9ea56c747e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180920"
---
# <a name="sqlgetpoolid-function"></a>SQLGetPoolID-Funktion
**Konformitäts**  
 Eingeführte Version: ODBC 3,81 Standards Compliance: ODBC  
  
 **Zusammenfassung**  
 **Sqlgetpoolid** Ruft die Pool-ID ab.  
  
## <a name="syntax"></a>Syntax  
  
```cpp
  
SQLRETURN  SQLGetPoolID (  
                SQLHDBC_INFO_TOKEN    hDbcInfoToken,  
                POOLID *              pPoolID );  
```  
  
## <a name="arguments"></a>Argumente  
 *hdbcinfotoken*  
 Der Tokenhandle, das alle Verbindungsinformationen enthält.  
  
 *ppoolid*  
 Ausgeben Die Pool-ID, die verwendet wird, um eine Gruppe von Verbindungen zu identifizieren, die austauschbar verwendet werden können (möglicherweise eine zusätzliche zurück Setzung erforderlich).  
  
## <a name="returns"></a>Gibt zurück  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR oder SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnose  
 Wenn **sqlgetpoolid** SQL_ERROR oder SQL_SUCCESS_WITH_INFO zurückgibt, verwendet der Treiber-Manager den **Handlertyp** SQL_HANDLE_DBC_INFO_TOKEN und einen **handle** von *hdbcinfotoken*.  
  
## <a name="remarks"></a>Bemerkungen  
 **Sqlgetpoolid** wird zum Abrufen der Pool-ID anhand eines Satzes von Verbindungsinformationen verwendet (von **sqlsetconnectattrfordbcinfo**, **sqlsetdriverconnectinfo** und **sqlsetconnectinfo**). Diese Pool-ID wird verwendet, um eine Gruppe von Verbindungen zu identifizieren, die austauschbar verwendet werden können (möglicherweise eine zusätzliche zurück Setzung erforderlich). Die Pool-ID wird verwendet, um den Verbindungspool für diese Gruppe von Verbindungen zu identifizieren.  
  
 Wenn ein Treiber SQL_ERROR oder SQL_INVALID_HANDLE zurückgibt, gibt der Treiber-Manager den Fehler an die Anwendung zurück (in [SQLCONNECT](../../../odbc/reference/syntax/sqlconnect-function.md) oder [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Wenn ein Treiber SQL_SUCCESS_WITH_INFO zurückgibt, erhält der Treiber-Manager die Diagnoseinformationen von *hdbcinfotoken* und gibt SQL_SUCCESS_WITH_INFO in [SQLCONNECT](../../../odbc/reference/syntax/sqlconnect-function.md) und [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)an die Anwendung zurück.  
  
 Anwendungen sollten diese Funktion nicht direkt aufzurufen. Ein ODBC-Treiber, der Treiber fähiges Verbindungspooling unterstützt, muss diese Funktion implementieren.  
  
 Fügen Sie sqlspi. h für die ODBC-Treiberentwicklung ein.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Entwickeln eines ODBC-Treibers](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Treiber fähiges Verbindungs Pooling](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Entwickeln von Verbindungspool-Unterstützung in einem ODBC-Treiber](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
