---
description: Statische Cursor
title: Statische Cursor | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], static
- static cursors [ADO]
ms.assetid: cce93ace-c4ed-4c6c-940c-28a50ff2fd12
author: rothja
ms.author: jroth
ms.openlocfilehash: a6b6ef12f2da2416e5f2c3c6e2e6c0c94e2c5327
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036980"
---
# <a name="static-cursors"></a>Statische Cursor
Der statische Cursor zeigt das Resultset immer so an, wie es beim ersten Öffnen des Cursors war. Abhängig von der Implementierung sind statische Cursor entweder schreibgeschützt oder Lese-/Schreibzugriff und bieten vorwärts und rückwärts Bildlauf. Der statische Cursor erkennt in der Regel keine Änderungen an der Mitgliedschaft, Reihenfolge oder den Werten des Resultsets, nachdem der Cursor geöffnet wurde. Statische Cursor können ihre eigenen UPDATE-, DELETE- und INSERT-Anweisungen erkennen, obwohl dies nicht erforderlich ist.  
  
 Statische Cursor erkennen nie andere Updates, Löschungen und Einfügungen. Angenommen ein statischer Cursor ruft eine Zeile ab, und eine andere Anwendung aktualisiert diese Zeile dann. Wenn die Anwendung die Zeile erneut vom statischen Cursor abruft, sind die erkannten Werte unverändert, obwohl die andere Anwendung Änderungen vorgenommen hat. Alle scrolltypen werden unterstützt, aber die Anbieter unterstützen Lesezeichen möglicherweise nicht.  
  
 Wenn Ihre Anwendung keine Datenänderungen erkennen muss und einen Bildlauf erfordert, ist der statische Cursor die beste Wahl. Verwenden Sie die " **adopconstatic cursortypeumum** ", um anzugeben, dass Sie einen statischen Cursor in ADO verwenden möchten.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Vorwärts Cursor](../../../ado/guide/data/forward-only-cursors.md)   
 [Keysetcursor](../../../ado/guide/data/keyset-cursors.md)   
 [Dynamische Cursor](../../../ado/guide/data/dynamic-cursors.md)
