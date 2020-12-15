---
title: Verarbeiten von Ergebnissen (ODBC) | Microsoft-Dokumentation
description: Informationen zum Verarbeiten von Daten, die SQL Server zurückgibt, wenn eine ODBC-Anwendung eine SQL-Anweisung sendet.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- result sets [ODBC], about result sets
- SQLRowCount function
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- COMPUTE clause
- result sets [ODBC]
- COMPUTE BY clause
ms.assetid: 61a8db19-6571-47dd-84e8-fcc97cb60b45
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a7e51247bec904bbd4d735e5706814c2b60bdfed
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97437951"
---
# <a name="processing-results-odbc"></a>Verarbeiten von Ergebnissen (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Wenn eine Anwendung eine SQL-Anweisung übermittelt, gibt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alle resultierenden Daten als ein oder mehrere Resultsets zurück. Ein Resultset ist ein Satz von Zeilen und Spalten, die den Kriterien der Abfrage entsprechen. SELECT-Anweisungen, Katalogfunktionen sowie einige gespeicherte Prozeduren erzeugen Resultsets, die für eine Anwendung in der Form von tabellarischen Daten verfügbar gemacht werden. Wenn es sich bei der ausgeführten SQL-Anweisung um eine gespeicherte Prozedur, einen Batch mit mehreren Befehlen oder eine SELECT-Anweisung mit Schlüsselwörtern handelt, ergeben sich daraus mehrere zu verarbeitende Resultsets.  
  
 Auch ODBC-Katalogfunktionen können Daten abrufen. [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md) ruft z. b. Daten zu Spalten in der Datenquelle ab. Diese Resultsets können 0 (null) oder mehr Zeilen enthalten.  
  
 Andere SQL-Anweisungen, z. B. GRANT oder REVOKE, geben keine Resultsets zurück. Für diese Anweisungen ist der Rückgabecode von **SQLExecute** oder **SQLExecDirect** normalerweise der einzige Hinweis darauf, dass die Anweisung erfolgreich war.  
  
 Jede INSERT-, UPDATE- und DELETE-Anweisung gibt ein Resultset zurück, das nur die Anzahl der von der Änderung betroffenen Zeilen enthält. Diese Anzahl wird zur Verfügung gestellt, wenn die Anwendung [SQLRowCount](../../relational-databases/native-client-odbc-api/sqlrowcount.md)aufruft. ODBC 3. *x* -Anwendungen müssen entweder **SQLRowCount** aufrufen, um das Resultset abzurufen, oder [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md) , um das Resultset abzubrechen. Wenn eine Anwendung einen Batch oder eine gespeicherte Prozedur ausführt, die mehrere INSERT-, Update-oder DELETE-Anweisungen enthält, muss das Resultset aus jeder Änderungs Anweisung mithilfe von **SQLRowCount** verarbeitet oder mit **SQLMoreResults** abgebrochen werden. Die Anzahlangaben können durch eine SET NOCOUNT ON-Anweisung im Batch oder in der gespeicherten Prozedur annulliert werden.  
  
 Transact-SQL enthält die SET NOCOUNT-Anweisung. Wenn die NOCOUNT-Option auf ON festgelegt ist, gibt SQL Server nicht die Anzahl der von einer-Anweisung betroffenen Zeilen zurück, und **SQLRowCount** gibt 0 zurück. Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Version des Native Client-ODBC-Treibers stellt eine Treiber spezifische [SQLGetStmtAttr](../../relational-databases/native-client-odbc-api/sqlgetstmtattr.md) -Option (SQL_SOPT_SS_NOCOUNT_STATUS) vor, um zu melden, ob die NOCOUNT-Option aktiviert oder deaktiviert ist. Jedes Mal, wenn **SQLRowCount** 0 zurückgibt, sollte die Anwendung SQL_SOPT_SS_NOCOUNT_STATUS testen. Wenn SQL_NC_ON zurückgegeben wird, gibt der Wert 0 von **SQLRowCount** lediglich an, dass SQL Server keine Zeilen Anzahl zurückgegeben hat. Wenn SQL_NC_OFF zurückgegeben wird, bedeutet dies, dass NOCOUNT auf OFF festgelegt ist und der Wert 0 aus **SQLRowCount** angibt, dass sich die Anweisung nicht auf Zeilen ausgewirkt hat. Anwendungen sollten den Wert von **SQLRowCount** nicht anzeigen, wenn SQL_SOPT_SS_NOCOUNT_STATUS SQL_NC_OFF ist. Große Batches oder gespeicherte Prozeduren können mehrere SET NOCOUNT-Anweisungen enthalten. Daher kann der Programmierer nicht davon ausgehen, dass SQL_SOPT_SS_NOCOUNT_STATUS konstant bleibt. Die Option sollte jedes Mal getestet werden, wenn **SQLRowCount** 0 zurückgibt.  
  
 Mehrere andere Transact-SQL-Anweisungen geben ihre Daten in Meldungen statt in Resultsets zurück. Wenn der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client-ODBC-Treiber diese Nachrichten empfängt, wird SQL_SUCCESS_WITH_INFO zurückgegeben, damit die Anwendung weiß, dass Informationsmeldungen verfügbar sind. Die Anwendung kann dann **SQLGetDiagRec** aufrufen, um diese Nachrichten abzurufen. Die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen, die auf diese Weise funktionieren, sind folgende:  
  
-   DBCC  
  
-   SET SHOWPLAN (mit früheren Versionen von SQL Server verfügbar)  
  
-   SET STATISTICS  
  
-   PRINT  
  
-   RAISERROR  
  
 Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client-ODBC-Treiber gibt SQL_ERROR bei einem RAISERROR mit einem Schweregrad von 11 oder höher zurück. Ist der Schweregrad von RAISERROR 19 oder mehr, wird auch die Verbindung unterbrochen.  
  
 Zum Verarbeiten der Resultsets aus einer SQL-Anweisung geht die Anwendung folgendermaßen vor:  
  
-   Sie bestimmt die Charakteristika des Resultsets.  
  
-   Sie bindet die Spalten an Programmvariable.  
  
-   Sie ruft einen einzelnen Wert, eine ganze Zeile mit Werten oder mehrere Zeilen mit Werten ab.  
  
-   Sie überprüft, ob weitere Resultsets vorhanden sind, und wenn dies der Fall ist, ermittelt sie die Charakteristika des neuen Resultsets.  
  
 Der Prozess des Abrufens von Zeilen aus der Datenquelle und deren Rückgabe an die Anwendung wird "Fetching" (Abrufen) genannt.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
-   [Bestimmen der Eigenschaften eines Resultsets &#40;ODBC-&#41;](../../relational-databases/native-client-odbc-results/determining-the-characteristics-of-a-result-set-odbc.md)  
  
-   [Zuweisen von Speicher](../../relational-databases/native-client-odbc-results/assigning-storage.md)  
  
-   [Abrufen von Ergebnisdaten](../../relational-databases/native-client-odbc-results/fetching-result-data.md)  
  
-   [Zuordnung von Datentypen &#40;ODBC-&#41;](../../relational-databases/native-client-odbc-results/mapping-data-types-odbc.md)  
  
-   [Datentypverwendung](../../relational-databases/native-client-odbc-results/data-type-usage.md)  
  
-   [Automatische Übersetzung der Zeichendaten](../../relational-databases/native-client-odbc-results/autotranslation-of-character-data.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Native Client &#40;ODBC-&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [Themen zur Vorgehensweise bei der Verarbeitung von Ergebnissen &#40;ODBC-&#41;](../native-client-odbc-how-to/processing-results-process-results.md)  
  
