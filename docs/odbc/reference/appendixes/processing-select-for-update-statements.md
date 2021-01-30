---
description: Verarbeiten von SELECT FOR UPDATE-Anweisungen
title: Verarbeiten von Select for Update-Anweisungen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], statement processing
- SQL statements [ODBC], select for update statements
- select for update [ODBC]
- SQL statements [ODBC], cursor library
- cursor library [ODBC], select for update statements
- ODBC cursor library [ODBC], select for update statements
- cursor library [ODBC], statement processing
ms.assetid: 8d2e79a4-5daf-458e-a536-d8b6e588753e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 116a87e12d1f765e3e33aa5b446743acfd8d429b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189733"
---
# <a name="processing-select-for-update-statements"></a>Verarbeiten von SELECT FOR UPDATE-Anweisungen
> [!IMPORTANT]  
>  Diese Funktion wird in einer zukünftigen Version von Windows entfernt. Vermeiden Sie die Verwendung dieses Features bei der Entwicklung neuer Anwendungen, und planen Sie das Ändern von Anwendungen, in denen diese Funktion derzeit verwendet wird Microsoft empfiehlt die Verwendung der Cursor-Funktionalität des Treibers.  
  
 Zur maximalen Interoperabilität sollten Anwendungen Resultsets generieren, die mit einer positionierten Update-Anweisung aktualisiert werden, indem **Sie eine SELECT for Update** -Anweisung ausführen. Die Cursor Bibliothek erfordert dies nicht, aber Sie ist für die meisten Datenquellen erforderlich, die positionierte UPDATE-Anweisungen unterstützen.  
  
 Die Cursor Bibliothek ignoriert die Spalten in der **for Update** -Klausel einer **Select for Update** -Anweisung. Diese Klausel wird entfernt, bevor die-Anweisung an den Treiber übergeben wird. In der Cursor Bibliothek steuert das SQL_ATTR_CONCURRENCY Statement-Attribut zusammen mit den im vorherigen Abschnitt erwähnten Einschränkungen, ob die Spalten in einem Resultset aktualisiert werden können.
