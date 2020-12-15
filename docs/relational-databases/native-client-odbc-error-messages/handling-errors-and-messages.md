---
title: Behandeln von Fehlern und Meldungen | Microsoft-Dokumentation
description: Erfahren Sie, welche Diagnoseinformationen zurückgegeben werden, wenn eine Anwendung eine ODBC-Funktion aufruft, einschließlich Erfolg oder Fehler und ausführliche Informationen.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC error handling, about error handling
- errors [ODBC]
- SQL Server Native Client ODBC driver, errors
- messages [ODBC], about messages
- ODBC error handling
- SQL_INVALID_HANDLE return code
- errors [ODBC], about error handling
- messages [ODBC]
ms.assetid: 74ea9630-e482-4a46-bb45-f5234f079b48
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 367142f4a605f95c81cbac34d4696a77f7a693ce
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438478"
---
# <a name="handling-errors-and-messages"></a>Behandlung von Fehlern und Meldungen
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Wenn eine Anwendung eine ODBC-Funktion aufruft, führt der Treiber die Funktion aus und gibt Diagnoseinformationen zurück: Ein Rückgabecode gibt das Ergebnis einer ODBC-Funktion zurück (Erfolg oder Fehlschlagen), und Diagnosedatensätze liefern detaillierte Informationen über die Funktion. Diagnosedatensätze enthalten einen Headerdatensatz und Statusdatensätze. Auch wenn die Funktion erfolgreich ausgeführt wurde, wird zumindest ein Diagnosedatensatz, der Headerdatensatz, zurückgegeben.  
  
 Die Diagnoseinformationen dienen während der Entwicklung zur Erfassung von Programmierfehlern wie ungültigen Handles und Syntaxfehlern in hartcodierten SQL-Anweisungen. Darüber hinaus dienen sie zur Laufzeit dazu, Laufzeitfehler und Warnungen zu erfassen, beispielsweise das Abschneiden von Daten, Regelverstöße und Syntaxfehler in benutzerdefinierten SQL-Anweisungen. Die Programmlogik basiert im Allgemeinen auf Rückgabecodes.  
  
 Nachdem eine Anwendung z. b. **SQLFetch** aufgerufen hat, um die Zeilen in einem Resultset abzurufen, gibt der Rückgabecode an, ob das Ende des Resultsets erreicht wurde (SQL_NO_DATA), ob Informationsmeldungen zurückgegeben wurden (SQL_SUCCESS_WITH_INFO) oder ob ein Fehler aufgetreten ist (SQL_ERROR).  
  
 Wenn der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client-ODBC-Treiber etwas anderes als SQL_SUCCESS zurückgibt, kann die Anwendung **SQLGetDiagRec** aufrufen, um Informations-oder Fehlermeldungen abzurufen. Verwenden Sie **SQLGetDiagRec** , um den Nachrichten Satz nach oben oder unten zu scrollen, wenn mehr als eine Nachricht vorhanden ist.  
  
 Der Rückgabecode SQL_INVALID_HANDLE gibt immer einen Programmierfehler an und sollte zur Laufzeit nie auftreten. Alle anderen Rückgabecodes stellen Laufzeitinformationen bereit, wenngleich SQL_ERROR einen Programmierfehler angeben kann.  
  
 Die ursprüngliche [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native API, DB-Library für C, ermöglicht es einer Anwendung, Rückruf Fehlerbehandlung und Nachrichten Behandlung zu installieren, die Fehler oder Meldungen zurückgeben. Einige [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen, wie PRINT, RAISERROR, DBCC und SET, geben ihre Ergebnisse an die Meldungshandlerfunktion der DB-Library anstatt an ein Resultset zurück. Jedoch verfügt die ODBC-API über keine solche Rückruffähigkeit. Wenn der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client-ODBC-Treiber die von zurückgegebenen Nachrichten erkennt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , wird der ODBC-Rückgabecode auf SQL_SUCCESS_WITH_INFO oder SQL_ERROR festgelegt, und die Nachricht wird als mindestens ein Diagnosedaten Satz zurückgegeben. Daher muss eine ODBC-Anwendung diese Rückgabecodes sorgfältig testen und **SQLGetDiagRec** aufrufen, um Nachrichten Daten abzurufen.  
  
 Informationen zur Ablaufverfolgung von Fehlern finden Sie unter [Data Access Tracing (Ablaufverfolgung für den Datenzugriff)](/previous-versions/sql/sql-server-2008/cc765421(v=sql.100)). Informationen zu Verbesserungen der in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] hinzugefügten Fehlerablaufverfolgung finden Sie unter [Zugreifen auf Diagnoseinformationen im Protokoll der erweiterten Ereignisse](../../relational-databases/native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md).  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
-   [Verarbeiten von Anweisungen, die Meldungen generieren](../../relational-databases/native-client-odbc-error-messages/processing-statements-that-generate-messages.md)  
  
-   [Diagnosedatensätze und -felder](../../relational-databases/native-client-odbc-error-messages/diagnostic-records-and-fields.md)  
  
-   [Systemeigene Fehlernummern](../../relational-databases/native-client-odbc-error-messages/native-error-numbers.md)  
  
-   [SQLSTATE &#40;ODBC-Fehler Codes&#41;](../../relational-databases/native-client-odbc-error-messages/sqlstate-odbc-error-codes.md)  
  
-   [Fehlermeldungen](../../relational-databases/native-client-odbc-error-messages/error-messages.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  
