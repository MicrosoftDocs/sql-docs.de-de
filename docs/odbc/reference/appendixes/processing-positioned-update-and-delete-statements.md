---
description: Verarbeiten von positionierten UPDATE- und DELETE-Anweisungen
title: Verarbeiten von positionierten Update-und DELETE-Anweisungen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- positioned deletes [ODBC]
- ODBC cursor library [ODBC], statement processing
- cursor library [ODBC], positioned update or delete
- positioned updates [ODBC]
- SQL statements [ODBC], cursor library
- ODBC cursor library [ODBC], positioned update or delete
- cursor library [ODBC], statement processing
ms.assetid: 2975dd97-48e6-4d0a-a9c7-40759a7d94c8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f2d7283524a516b81b6489b543fd3169f7f9aeb4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159533"
---
# <a name="processing-positioned-update-and-delete-statements"></a>Verarbeiten von positionierten UPDATE- und DELETE-Anweisungen
> [!IMPORTANT]  
>  Diese Funktion wird in einer zukünftigen Version von Windows entfernt. Vermeiden Sie die Verwendung dieses Features bei der Entwicklung neuer Anwendungen, und planen Sie das Ändern von Anwendungen, in denen diese Funktion derzeit verwendet wird Microsoft empfiehlt die Verwendung der Cursor-Funktionalität des Treibers.  
  
 Die Cursor Bibliothek unterstützt positionierte UPDATE-und DELETE-Anweisungen durch Ersetzen der **WHERE CURRENT of** -Klausel in solchen Anweisungen durch eine **Where** -Klausel, die die im Cache gespeicherten Werte für jede gebundene Spalte auflistet. Die Cursor Bibliothek übergibt die neu erstellten **Update** -und **Delete** -Anweisungen zur Ausführung an den Treiber. Bei positionierten Update-Anweisungen aktualisiert die Cursor Bibliothek dann Ihren Cache von den Werten in den rowsetpuffern und legt den entsprechenden Wert im Zeilen Status Array auf SQL_ROW_UPDATED fest. Für positionierte DELETE-Anweisungen wird der entsprechende Wert im Zeilen Status Array auf SQL_ROW_DELETED festgelegt.  
  
> [!CAUTION]  
>  Die **Where** -Klausel, die von der Cursor Bibliothek zum Identifizieren der aktuellen Zeile erstellt wurde, kann keine Zeilen identifizieren, eine andere Zeile identifizieren oder mehr als eine Zeile identifizieren. Weitere Informationen finden Sie unter [Erstellen](../../../odbc/reference/appendixes/constructing-searched-statements.md)von Such Anweisungen weiter unten in diesem Anhang.  
  
 Positionierte UPDATE-und DELETE-Anweisungen unterliegen den folgenden Einschränkungen:  
  
-   Positionierte UPDATE-und DELETE-Anweisungen können nur in den folgenden Fällen verwendet werden: Wenn eine **Select** -Anweisung das Resultset generiert hat. Wenn die **Select** -Anweisung keine Join-, **Union** -oder **Group by** -Klausel enthielt; und wenn Spalten, die einen Alias oder Ausdruck in der Auswahlliste verwendet haben, nicht an **SQLBindCol** gebunden wurden.  
  
-   Wenn eine Anwendung eine positionierte UPDATE-oder DELETE-Anweisung vorbereitet, muss Sie nach dem Aufrufen von **SQLFetch** oder **SQLFetchScroll** aufgerufen werden. Obwohl die Cursor Bibliothek die Anweisung zur Vorbereitung an den Treiber übermittelt, schließt Sie die Anweisung und führt Sie direkt aus, wenn die Anwendung **SQLExecute** aufruft.  
  
-   Wenn der Treiber nur eine aktive Anweisung unterstützt, ruft die Cursor Bibliothek den Rest des Resultsets ab und ruft dann das aktuelle Rowset aus dem Cache ab, bevor eine positionierte UPDATE-oder DELETE-Anweisung ausgeführt wird. Wenn die Anwendung dann eine Funktion aufruft, die Metadaten in einem Resultset zurückgibt (z. b. **SQLNumResultCols** oder **SQLDescribeCol**), gibt die Cursor Bibliothek einen Fehler zurück.  
  
-   Wenn eine positionierte UPDATE-oder DELETE-Anweisung für eine Spalte einer Tabelle mit einer Zeitstempel-Spalte ausgeführt wird, die bei jeder Aktualisierung automatisch aktualisiert wird, können alle nachfolgenden positionierten Update-oder DELETE-Anweisungen nicht ausgeführt werden, wenn die Zeitstempel-Spalte gebunden ist. Dies liegt daran, dass die von der Cursor Bibliothek erstellte Update-oder DELETE-Anweisung die zu Aktualisier Ende Zeile nicht exakt identifiziert. Der Wert in der gesuchten Anweisung für die Zeitstempel-Spalte stimmt nicht mit dem automatisch aktualisierten Wert der Zeitstempel-Spalte ab.
