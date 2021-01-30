---
description: Verwenden der ODBC-Cursorbibliothek
title: Verwenden der ODBC-Cursor Bibliothek | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], using cursor library
- cursor library [ODBC], using cursor library
ms.assetid: 9653f2f8-ccfc-4220-99ef-601dc0fa641c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1658d3c628506f1b5c53a5c9271a10f96fa2ef44
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158189"
---
# <a name="using-the-odbc-cursor-library"></a>Verwenden der ODBC-Cursorbibliothek
> [!IMPORTANT]  
>  Diese Funktion wird in einer zukünftigen Version von Windows entfernt. Vermeiden Sie die Verwendung dieses Features bei der Entwicklung neuer Anwendungen, und planen Sie das Ändern von Anwendungen, in denen diese Funktion derzeit verwendet wird Microsoft empfiehlt die Verwendung der Cursor-Funktionalität des Treibers.  
  
 Um die ODBC-Cursor Bibliothek zu verwenden, ist eine Anwendung:  
  
1.  Ruft **SQLSetConnectAttr** mit einem *Attribut* von SQL_ATTR_ODBC_CURSORS auf, um anzugeben, wie die Cursor Bibliothek mit einer bestimmten Verbindung verwendet werden soll. Die Cursor Bibliothek kann immer verwendet werden (SQL_CUR_USE_ODBC), nur verwendet, wenn der Treiber keine scrollbaren Cursor (SQL_CUR_USE_IF_NEEDED) unterstützt oder nie verwendet wird (SQL_CUR_USE_DRIVER).  
  
2.  Ruft **SQLCONNECT**, **SQLDriverConnect** oder **sqlbrowseconnetct** auf, um eine Verbindung mit der Datenquelle herzustellen.  
  
3.  Ruft **SQLSetStmtAttr** auf, um den Cursortyp (SQL_ATTR_CURSOR_TYPE), die Parallelität (SQL_ATTR_CONCURRENCY) und die Rowsetgröße (SQL_ATTR_ROW_ARRAY_SIZE) anzugeben. Die Cursor Bibliothek unterstützt Vorwärts Cursor und statische Cursor. Vorwärts Cursor müssen schreibgeschützt sein, während statische Cursor schreibgeschützt sein können oder die Steuerung der vollständigen Parallelität beim Vergleichen von Werten verwenden können.  
  
4.  Weist einen oder mehrere rowsetpuffer zu und ruft **SQLBindCol** einmal oder mehrmals auf, um diese Puffer an Resultsetspalten zu binden.  
  
5.  Generiert ein Resultset, indem eine **Select** -Anweisung oder eine-Prozedur ausgeführt oder eine Katalog Funktion aufgerufen wird. Wenn die Anwendung positionierte UPDATE-Anweisungen ausführt, sollte eine **Select for Update** -Anweisung ausgeführt werden, um das Resultset zu generieren.  
  
6.  Ruft ein oder mehrere Male **SQLFetch** oder **SQLFetchScroll** auf, um durch das Resultset zu scrollen.  
  
 Die Anwendung kann Datenwerte in den rowsetpuffern ändern. Um die rowsetpuffer mit Daten aus dem Cache der Cursor Bibliothek zu aktualisieren, ruft eine Anwendung **SQLFetchScroll** auf, wobei das *FetchOrientation* -Argument auf SQL_FETCH_RELATIVE und das *FetchOffset* -Argument auf "0" festgelegt ist.  
  
 Zum Abrufen von Daten aus einer ungebundenen Spalte Ruft die Anwendung **SQLSetPos** auf, um den Cursor in der gewünschten Zeile zu positionieren. Anschließend wird **SQLGetData** aufgerufen, um die Daten abzurufen.  
  
 Um die Anzahl der Zeilen zu bestimmen, die aus der Datenquelle abgerufen wurden, ruft die Anwendung **SQLRowCount** auf.
