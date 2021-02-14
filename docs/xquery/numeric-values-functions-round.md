---
title: Round-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die XQuery-Funktion Round (), die die Zahl zurückgibt, die keine Bruchteile hat, die dem angegebenen Argument am nächsten sind.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:round function
- round function [XQuery]
ms.assetid: 320b572f-bd5b-4055-95a6-dec5718c0041
author: rothja
ms.author: jroth
ms.openlocfilehash: f245b613c3e5e32fd2d4cc8eb09e969719ab1762
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347913"
---
# <a name="numeric-values-functions---round"></a>Funktionen für numerische Werte – round
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Gibt die Zahl (ohne Stellen hinter dem Dezimalpunkt) zurück, die dem Argument am nächsten kommt. Wenn es mehr als eine solche Zahl gibt, wird diejenige zurückgegeben, die am nächsten an der positiv unendlichen Zahl liegt. Beispiel:  
  
 Wenn das Argument 2,5 ist, gibt **Round ()** 3 zurück.  
  
 Wenn das Argument 2,4999 ist, gibt **Round ()** 2 zurück.  
  
 Wenn das Argument-2,5 ist, gibt **Round ()** -2 zurück.  
  
 Wenn das Argument eine leere Sequenz ist, gibt **Round ()** die leere Sequenz zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:round ( $arg as numeric?) as numeric?  
```  
  
## <a name="arguments"></a>Argumente  
 *$arg*  
 Anzahl, auf die die Funktion angewendet wird.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn der Typ des *$arg* einer der drei numerischen Basis Typen ( **xs: float**, **xs: Double** oder **xs: Decimal**) ist, ist der Rückgabetyp identisch mit dem *$arg* Typ. Wenn der Typ *$arg* ein Typ ist, der von einem der numerischen Typen abgeleitet ist, ist der Rückgabetyp der numerische Basistyp.  
  
 Wenn die Eingabe für die **FN: Floor**-, **FN: ceiling**-oder **FN: Round** -Funktionen **xdt: untypedAtomic**, nicht typisierte Daten ist, wird Sie implizit in **xs: Double** umgewandelt.  
  
 Alle anderen Typen führen zum Generieren eines statischen Fehlers.  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der AdventureWorks-Datenbank gespeichert sind.  
  
 Sie können das Arbeitsbeispiel in der [ceiling-Funktion (XQuery)](../xquery/numeric-values-functions-ceiling.md) für die XQuery-Funktion **Round ()** verwenden. Sie müssen lediglich die **Ceiling ()** -Funktion in der Abfrage durch die **Round ()** -Funktion ersetzen.  
  
## <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Die **Round ()** -Funktion ordnet ganzzahlige Werte xs: Decimal zu.  
  
-   Die **Round ()** -Funktion von xs: Double-und xs: float-Werten zwischen-0.5 E0 und-0e0 wird 0e0 anstelle von-0e0 zugeordnet.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Floor-Funktion &#40;XQuery-&#41;](../xquery/numeric-values-functions-floor.md)   
 [CEILING-Funktion &#40;XQuery-&#41;](../xquery/numeric-values-functions-ceiling.md)  
  
  
