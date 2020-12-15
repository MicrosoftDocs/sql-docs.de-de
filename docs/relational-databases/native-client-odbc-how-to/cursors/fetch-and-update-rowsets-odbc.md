---
description: Abfragen und Aktualisieren von Rowsets (ODBC)
title: Abrufen und Aktualisieren von Rowsets (ODBC) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- rowsets [ODBC]
ms.assetid: cf0eb3b4-8b72-49fc-a845-95edc360cf93
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b5eaa9a150589cbdf41a41d3528b2b49b00cd7c9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467761"
---
# <a name="fetch-and-update-rowsets-odbc"></a>Abfragen und Aktualisieren von Rowsets (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-fetch-and-update-rowsets"></a>So können Sie Rowsets abfragen und aktualisieren  
  
1.  Optional können Sie [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) mit SQL_ROW_ARRAY_SIZE aufrufen, um die Anzahl der Zeilen (R) im Rowset zu ändern.  
  
2.  Rufen Sie [SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md) oder [SQLFetchScroll](../../../relational-databases/native-client-odbc-api/sqlfetchscroll.md) auf, um ein Rowset abzurufen.  
  
3.  Bei der Verwendung von gebundenen Spalten verwenden Sie die Datenwerte und Datenlängen, die nun in den Puffern mit gebundenen Spalten für das Rowset verfügbar sind.  
  
     Bei der Verwendung von ungebundenen Spalten rufen Sie für jede Zeile [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md) mit SQL_POSITION auf, um die Cursorposition festzulegen. Gehen Sie anschließend bei jeder ungebundenen Spalte wie folgt vor:  
  
    -   Rufen Sie [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) einmal oder mehrere Male auf, um die Daten für ungebundene Spalten nach der letzten gebundenen Spalte des Rowsets abzurufen. [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) muss in zunehmender Spaltenzahlfolge aufgerufen werden.  
  
    -   Rufen Sie [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) mehrere Male auf, um Daten aus einer text- oder image-Spalte abzurufen.  
  
4.  Richten Sie alle Data-at-Execution-text- oder Data-at-Execution-image-Spalten ein.  
  
5.  Rufen Sie [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md) oder [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md) auf, um die Cursorposition festzulegen und Zeilen im Rowset zu aktualisieren, zu löschen oder hinzuzufügen.  
  
     Data-at-Execution-text- oder Data-at-Execution-image-Spalten, die zum Aktualisieren oder Hinzufügen verwendet werden, müssen verarbeitet werden.  
  
6.  Sie können eine positionierte UPDATE- oder DELETE-Anweisung ausführen und dabei den Cursornamen angeben (über [SQLGetCursorName](../../../relational-databases/native-client-odbc-api/sqlgetcursorname.md)abrufbar) und für dieselbe Verbindung ein anderes Anweisungshandle verwenden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gewusst-wie-Themen zur Verwendung von Cursorn &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-how-to/cursors/using-cursors-how-to-topics-odbc.md)  
  
