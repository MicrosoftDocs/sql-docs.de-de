---
description: SQLNativeSql (Cursorbibliothek)
title: SQLNativeSql (Cursor Bibliothek) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLNativeSql function [ODBC], Cursor Library
ms.assetid: c4459092-1177-4b2a-b7f5-e0083d3bf2b2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 06e5d6ceffad6025c2184f60ab98457756c36901
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202696"
---
# <a name="sqlnativesql-cursor-library"></a>SQLNativeSql (Cursorbibliothek)
> [!IMPORTANT]  
>  Diese Funktion wird in einer zukünftigen Version von Windows entfernt. Vermeiden Sie die Verwendung dieses Features bei der Entwicklung neuer Anwendungen, und planen Sie das Ändern von Anwendungen, in denen diese Funktion derzeit verwendet wird Microsoft empfiehlt die Verwendung der Cursor-Funktionalität des Treibers.  
  
 In diesem Thema wird die Verwendung der **SQLNativeSql** -Funktion in der Cursor Bibliothek erläutert. Allgemeine Informationen zu **SQLNativeSql** finden Sie unter [SQLNativeSql-Funktion](../../../odbc/reference/syntax/sqlnativesql-function.md).  
  
 Wenn der Treiber diese Funktion unterstützt, ruft die Cursor Bibliothek **SQLNativeSql** im Treiber auf und übergibt sie an die SQL-Anweisung. Für positioniertes Update, positioniertes löschen und **Select for Update** -Anweisungen wird die Anweisung von der Cursor Bibliothek geändert, bevor Sie an den Treiber übergeben wird.  
  
> [!NOTE]  
>  Die Cursor Bibliothek gibt nicht korrekt SQLSTATE 34000 (Ungültiger Cursor Name) zurück, wenn der Cursor Name in einer positionierten Update-oder DELETE-Anweisung ungültig ist, die im *instatementtext* -Argument von **SQLNativeSql** weitergegeben wird. **SQLNativeSql** ist nicht für die Rückgabe von Syntax Fehlern vorgesehen, die nur bei der Anweisungs Vorbereitung oder-Ausführung zurückgegeben werden.
