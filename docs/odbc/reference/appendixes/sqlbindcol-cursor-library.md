---
description: SQLBindCol (Cursorbibliothek)
title: SQLBindCol (Cursor Bibliothek) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLAllocStmt function [ODBC], Cursor Library
ms.assetid: f4dd546a-0a6c-4397-8ee7-fafa6b9da543
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 64b51f58fd51b9bcad1abd86750b024be131a8a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202952"
---
# <a name="sqlbindcol-cursor-library"></a>SQLBindCol (Cursorbibliothek)
> [!IMPORTANT]  
>  Diese Funktion wird in einer zukünftigen Version von Windows entfernt. Vermeiden Sie die Verwendung dieses Features bei der Entwicklung neuer Anwendungen, und planen Sie das Ändern von Anwendungen, in denen diese Funktion derzeit verwendet wird Microsoft empfiehlt die Verwendung der Cursor-Funktionalität des Treibers.  
  
 In diesem Thema wird die Verwendung der **SQLBindCol** -Funktion in der Cursor Bibliothek erläutert. Allgemeine Informationen zu **SQLBindCol** finden Sie unter [SQLBindCol-Funktion](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
 Eine Anwendung ordnet einen oder mehrere Puffer zu, damit die Cursor Bibliothek das aktuelle Rowset in zurückgibt. **SQLBindCol** wird einmal oder mehrmals aufgerufen, um diese Puffer an das Resultset zu binden.  
  
 Eine Anwendung kann **SQLBindCol** aufrufen, um die Resultsetspalten erneut zu binden, nachdem Sie **SQLExtendedFetch**, **SQLFetch** oder **SQLFetchScroll** aufgerufen hat, sofern der C-Datentyp, die Spaltengröße und die Dezimalziffern der gebundenen Spalte unverändert bleiben. Die Anwendung muss den Cursor nicht schließen, um Spalten erneut an verschiedene Adressen zu binden.  
  
 Die Cursor Bibliothek unterstützt das Festlegen des SQL_ATTR_ROW_BIND_OFFSET_PTR Anweisungs Attributs für die Verwendung von Bindungs Offsets. (**SQLBindCol** muss nicht aufgerufen werden, damit diese erneute Bindung stattfindet.) Wenn die Cursor Bibliothek mit einem ODBC *3. x* -Treiber verwendet wird, wird der Bindungs Offset nicht verwendet, wenn **SQLFetch** aufgerufen wird. Der Bindungs Offset wird verwendet, wenn **SQLFetch** aufgerufen wird, wenn die Cursor Bibliothek mit einem ODBC *2. x* -Treiber verwendet wird, da **SQLFetch** dann **SQLExtendedFetch** zugeordnet wird.  
  
 Die Cursor Bibliothek unterstützt das Aufrufen von **SQLBindCol** , um die Lesezeichen Spalte zu binden.  
  
 Bei der Arbeit mit einem ODBC *2. x* -Treiber gibt die Cursor Bibliothek SQLSTATE HY090 (ungültige Zeichen folgen-oder Pufferlänge) zurück, wenn **SQLBindCol** aufgerufen wird, um die Pufferlänge für eine Lesezeichen Spalte auf einen nicht gleich 4-Wert festzulegen. Wenn Sie mit einem ODBC *3. x* -Treiber arbeiten, ermöglicht die Cursor Bibliothek die Größe des Puffers.
