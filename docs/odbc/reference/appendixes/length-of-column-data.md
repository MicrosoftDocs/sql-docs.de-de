---
description: Länge von Spaltendaten
title: Länge der Spaltendaten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- column data [ODBC]
- length of column data [ODBC]
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- cache [ODBC]
ms.assetid: c762c881-ebe0-4eac-84d5-f30281fc3eca
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: be0ad52f040c195d4e8eeab9d307aa8f92bcf62d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184406"
---
# <a name="length-of-column-data"></a>Länge von Spaltendaten
> [!IMPORTANT]  
>  Diese Funktion wird in einer zukünftigen Version von Windows entfernt. Vermeiden Sie die Verwendung dieses Features bei der Entwicklung neuer Anwendungen, und planen Sie das Ändern von Anwendungen, in denen diese Funktion derzeit verwendet wird Microsoft empfiehlt die Verwendung der Cursor-Funktionalität des Treibers.  
  
 Die Cursor Bibliothek erstellt einen Puffer im Cache für jeden length/Indicator-Puffer, der an das Resultset mit **SQLBindCol** gebunden ist. Die Werte in diesen Puffern werden verwendet, um eine **Where** -Klausel zu erstellen, wenn Sie positionierte UPDATE-oder DELETE-Anweisungen emuliert. Diese Puffer werden von den rowsetpuffern aktualisiert, wenn Daten aus der Datenquelle abgerufen und positionierte UPDATE-Anweisungen ausgeführt werden.  
  
 Wenn der C-Typ eines Daten Puffers SQL_C_CHAR oder SQL_C_BINARY ist und der Wert für die Länge/Anzeige SQL_NTS ist, wird die Zeichen folgen Länge der Daten in den Längen-/Indikatorpuffer eingefügt.  
  
> [!NOTE]  
>  Die Cursor Bibliothek aktualisiert Ihren Cache für eine Spalte nicht, wenn **StrLen_or_IndPtr* im entsprechenden rowsetpuffer SQL_DATA_AT_EXEC oder das Ergebnis des SQL_LEN_DATA_AT_EXEC Makros ist.
