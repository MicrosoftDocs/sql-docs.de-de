---
title: Abrufen gespeicherter Prozeduren (ODBC) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Datenquelle mithilfe des ODBC-Administrators, Programm gesteuert oder mithilfe einer Datei hinzufügen, bevor Sie ODBC-Anwendungen mit SQL Server 2005 oder höher verwenden.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- stored procedures [ODBC], calling
ms.assetid: 31176be8-d40e-4f93-8d44-a46e804a3e2d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8f93ad9f3229597fea17d53747dcd97d22172d6c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469511"
---
# <a name="running-stored-procedures---call-stored-procedures"></a>Ausführen gespeicherter Prozeduren: Aufrufen gespeicherter Prozeduren
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-ODBC-Treiber unterstützt ausführende gespeicherte Prozeduren als remote gespeicherte Prozeduren. Das Ausführen einer gespeicherten Prozedur als remote gespeicherte Prozedur ermöglicht dem Treiber und dem Server das Optimieren der Leistung der Prozedurausführung.  
  
  Wenn eine SQL-Anweisung eine gespeicherte Prozedur mit der ODBC CALL-Escape-Klausel aufruft, sendet der Microsoft® SQL Server™-Treiber die Prozedur mit dem RPC (Remote Stored Procedure Call)-Mechanismus an SQL Server. RPC-Anforderungen umgehen größtenteils das Analysieren der Anwendungen und die Parameterverarbeitung in SQL Server und sind schneller als die Transact-SQL EXECUTE-Anweisung.  
  
 Eine Beispielanwendung, die diese Funktion veranschaulicht, finden Sie unter [Verarbeiten von Rückgabe Codes und Ausgabeparametern &#40;ODBC-&#41;](../../relational-databases/native-client-odbc-how-to/running-stored-procedures-process-return-codes-and-output-parameters.md).  
  
### <a name="to-run-a-procedure-as-an-rpc"></a>So führen Sie eine Prozedur als RPC aus  
  
1.  Erstellen Sie eine SQL-Anweisung, die die ODBC-Escapesequenz verwendet. Die Anweisung verwendet Parametermarkierungen für jeden Eingabe-, Eingabe/Ausgabe- und Ausgabeparameter sowie für den Prozedurrückgabewert (falls zutreffend):  
  
    ```  
    {? = CALL procname (?,?)}  
    ```  
  
2.  Rufen Sie [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md) für jeden Eingabe-, Eingabe/Ausgabe- und Ausgabeparameter sowie für den Prozedurrückgabewert (sofern vorhanden) auf.  
  
3.  Führen Sie die Anweisung mit [SQLExecDirect](../../odbc/reference/syntax/sqlexecdirect-function.md)aus.  
  
> [!NOTE]  
>  Wenn eine Anwendung eine Prozedur mit der Transact-SQL EXECUTE-Syntax (statt mit der ODBC CALL-Escapesequenz) übermittelt, gibt der SQL Server ODBC-Treiber den Prozeduraufruf an SQL Server als SQL-Anweisung statt als RPC weiter. Darüber hinaus werden Ausgabeparameter nicht zurückgegeben, wenn die Transact SQL EXECUTE-Anweisung verwendet wird.  
  
## <a name="see-also"></a>Weitere Informationen  
  [Batch Verarbeitung von gespeicherten Prozedur aufrufen](../../relational-databases/native-client-odbc-stored-procedures/batching-stored-procedure-calls.md)   
 [Gespeicherte Prozeduren](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)   
 [Aufrufen einer gespeicherten Prozedur](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md)   
 [Vorgehensweisen](../../relational-databases/native-client-odbc-queries/executing-statements/procedures.md)  
  
