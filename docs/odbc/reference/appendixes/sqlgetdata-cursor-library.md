---
description: SQLGetData (Cursorbibliothek)
title: SQLGetData (Cursor Bibliothek) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetData function [ODBC], Cursor Library
ms.assetid: ff40c9c0-b847-4426-a099-1bff47e6e872
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1e0cb6a73fb294e8b47ab82c988f8d07a4a220df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202792"
---
# <a name="sqlgetdata-cursor-library"></a>SQLGetData (Cursorbibliothek)
> [!IMPORTANT]  
>  Diese Funktion wird in einer zukünftigen Version von Windows entfernt. Vermeiden Sie die Verwendung dieses Features bei der Entwicklung neuer Anwendungen, und planen Sie das Ändern von Anwendungen, in denen diese Funktion derzeit verwendet wird Microsoft empfiehlt die Verwendung der Cursor-Funktionalität des Treibers.  
  
 In diesem Thema wird die Verwendung der **SQLGetData** -Funktion in der Cursor Bibliothek erläutert. Allgemeine Informationen zu **SQLGetData** finden Sie unter [SQLGetData-Funktion](../../../odbc/reference/syntax/sqlgetdata-function.md).  
  
 Die Cursor Bibliothek implementiert **SQLGetData** , indem zuerst eine **Select** -Anweisung mit einer **Where** -Klausel erstellt wird, die die im Cache gespeicherten Werte für jede gebundene Spalte in der aktuellen Zeile auflistet. Anschließend wird die **Select** -Anweisung ausgeführt, um die Zeile erneut auszuwählen, und **SQLGetData** wird im Treiber aufgerufen, um die Daten aus der Datenquelle abzurufen (im Gegensatz zum Cache).  
  
> [!CAUTION]  
>  Die **Where** -Klausel, die von der Cursor Bibliothek zum Identifizieren der aktuellen Zeile erstellt wurde, kann keine Zeilen identifizieren, eine andere Zeile identifizieren oder mehr als eine Zeile identifizieren. Weitere Informationen finden Sie unter [Erstellen von durchsuchten Anweisungen](../../../odbc/reference/appendixes/constructing-searched-statements.md).  
  
 Wenn das SQL_ATTR_USE_BOOKMARKS-Anweisungs Attribut auf SQL_UB_VARIABLE festgelegt ist, kann **SQLGetData** für Spalte 0 aufgerufen werden, um Lesezeichen Daten zurückzugeben.  
  
 Aufrufe von **SQLGetData** unterliegen den folgenden Einschränkungen:  
  
-   **SQLGetData** kann nicht für Vorwärts Cursor aufgerufen werden.  
  
-   **SQLGetData** kann nur aufgerufen werden, wenn die folgenden Bedingungen erfüllt sind: eine **Select** -Anweisung hat das Resultset generiert. die **Select** -Anweisung enthielt keine Join-, **Union** -oder **Group by** -Klausel. und alle Spalten, die einen Alias oder Ausdruck in der Auswahlliste verwendet haben, wurden nicht an **SQLBindCol** gebunden.  
  
-   Wenn der Treiber nur eine aktive Anweisung unterstützt, ruft die Cursor Bibliothek den Rest des Resultsets ab, bevor die **Select** -Anweisung ausgeführt und **SQLGetData** aufgerufen wird.
