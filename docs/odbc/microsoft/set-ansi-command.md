---
description: SET ANSI-Befehl
title: ANSI-Befehl festlegen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- set ANSI command [ODBC]
ms.assetid: cf9a01b2-14bf-458c-a73c-2a58ddef32d8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bb9dbd2b73b4ff0f7f75442c42de31dbe389211a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208664"
---
# <a name="set-ansi-command"></a>SET ANSI-Befehl
Bestimmt, wie Vergleiche zwischen Zeichen folgen mit unterschiedlichen Längen mit dem =-Operator in Visual FoxPro-SQL-Befehlen durchgeführt werden.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
SET ANSI ON | OFF  
```  
  
## <a name="arguments"></a>Argumente  
 EIN  
 (Standardwert für den Treiber; der Standardwert für Visual FoxPro ist off.) Füllt die kürzere Zeichenfolge mit den Leerzeichen, die erforderlich sind, damit Sie der Länge der längeren Zeichenfolge entspricht. Die beiden Zeichen folgen werden dann mit einem Zeichen für Ihre gesamte Länge verglichen. Beachten Sie diesen Vergleich:  
  
```  
'Tommy' = 'Tom'  
```  
  
 Das Ergebnis ist false (. F.), wenn SET ANSI auf ON festgelegt ist, da "Tom" beim Auffüllen zu "Tom" wird und die Zeichen folgen "Tom" und "Torsten" nicht mit Zeichen für Zeichen identisch sind.  
  
 Der = =-Operator verwendet diese Methode für Vergleiche in Visual FoxPro-SQL-Befehlen.  
  
 OFF  
 Gibt an, dass die kürzere Zeichenfolge nicht mit Leerzeichen aufgefüllt werden soll. Die beiden Zeichen folgen werden mit einem Zeichen verglichen, bis das Ende der kürzeren Zeichenfolge erreicht ist. Beachten Sie diesen Vergleich:  
  
```  
'Tommy' = 'Tom'  
```  
  
 Das Ergebnis ist true (. T.), wenn SET ANSI auf OFF festgelegt ist, da der Vergleich nach ' Tom ' beendet wird.  
  
## <a name="remarks"></a>Bemerkungen  
 SET ANSI bestimmt, ob der kürzere von zwei Zeichen folgen mit Leerzeichen aufgefüllt wird, wenn ein SQL-Zeichen folgen Vergleich durchgeführt wird. SET ANSI hat keine Auswirkung auf den = =-Operator; Wenn Sie den = =-Operator verwenden, wird die kürzere Zeichenfolge für den Vergleich immer mit Leerzeichen aufgefüllt.  
  
## <a name="string-order"></a>Zeichenfolge  
 In SQL-Befehlen ist die Reihenfolge der beiden Zeichen folgen in einem Vergleich von links nach rechts eine Zeichenfolge, die eine Zeichenfolge von einer Seite des =-oder = =-Operators auf die andere nicht beeinträchtigt. Dies wirkt sich nicht auf das Ergebnis des Vergleichs aus.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SELECT-SQL-Befehl](../../odbc/microsoft/select-sql-command.md)   
 [SET EXACT-Befehl](../../odbc/microsoft/set-exact-command.md)
