---
description: Einschränkungen der SELECT-Anweisung
title: Einschränkungen für die SELECT-Anweisung | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, SELECT statement limitations
- SELECT statement limitations [ODBC]
ms.assetid: c6b05955-f8fd-4706-a1a7-a8dbd74870c2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5bfbdbf4396b51624173e676f4b0c9bea1f4cf87
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208678"
---
# <a name="select-statement-limitations"></a>Einschränkungen der SELECT-Anweisung
Eine Aggregat Funktions Spalte kann nicht mit einer nicht aggregierten Spalte in einer SELECT-Anweisung gemischt werden.  
  
 Die SELECT-Liste einer SELECT-Anweisung mit einer Group By-Klausel kann nur Ausdrücke aus der Group By-Klausel oder Set-Funktionen enthalten.  
  
 Die Verwendung eines Sternchen (zum Auswählen aller Spalten) in einer SELECT-Anweisung, die eine Group By-Klausel enthält, wird nicht unterstützt. Die Namen der auszuwählenden Spalten müssen angegeben werden.  
  
 Die Verwendung eines vertikalen Balkens in einer SELECT-Anweisung wird nicht unterstützt. Verwenden Sie einen Parameter in der SELECT-Anweisung, wenn Sie auf einen Datenwert verweisen müssen, der einen senkrechten Strich enthält.  
  
 Wenn ein Spaltenalias in einer SELECT-Anweisung verwendet wird, muss das Wort "as" dem Alias vorangestellt sein. Beispiel: "SELECT Col1 as a from b". Ohne das "as" gibt die Anweisung einen Fehler zurück.  
  
 Wenn ein falscher Spaltenname in eine SELECT-Anweisung eingegeben wird, wird der SQLSTATE 07001-Fehler "falsche Anzahl von Parametern" anstelle eines SQLSTATE-S0022-Fehlers zurückgegeben, "Spalte nicht gefunden".  
  
 Wenn der Microsoft Excel-Treiber verwendet wird und eine leere Zeichenfolge in eine Spalte eingefügt wird, wird die leere Zeichenfolge in einen NULL-Wert konvertiert. eine gesuchte SELECT-Anweisung, die mit einer leeren Zeichenfolge in der WHERE-Klausel ausgeführt wird, ist für diese Spalte nicht erfolgreich.
