---
description: "'SQLFreeStmt'"
title: SQLFreeStmt | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/23/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLFreeStmt function
ms.assetid: d9666d0b-3446-480e-bf1a-10b01213e411
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2a131178ecef5a9a0e04eb1b0564f89e6788d745
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91811094"
---
# <a name="sqlfreestmt"></a>'SQLFreeStmt'
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Meist   
      **SQLFreeStmt** wird für ODBC 3.0 und höher nicht empfohlen. Wenn die Anwendung jedoch die-Anweisung wieder verwenden muss, sollte **SQLFreeStmt** weiterhin mit den Optionen **SQL_RESET_PARAMS** und **SQL_UNBIND** ) verwendet werden. Sie können auch [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md), [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md), [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md), [SQLSetDescField](../../relational-databases/native-client-odbc-api/sqlsetdescfield.md)und [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) verwenden, um die Funktion von **SQLFreeStmt** zu ersetzen oder zu duplizieren. Sie sollten Sie stattdessen verwenden.  
  
 Im Allgemeinen ist es effizienter,-Anweisungen wiederzuverwenden, als Sie zu löschen und neue zuzuweisen. In einigen Situationen, wie z. b. der Wiederverwendung von-Anweisungen, muss jedoch auch SQLFreeStmt verwendet werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLFreeStmt-Funktion](../../odbc/reference/syntax/sqlfreestmt-function.md)   
 [ODBC API Implementation Details](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
