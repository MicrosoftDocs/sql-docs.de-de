---
title: Floor-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die XQuery Floor ()-Funktion, die die größte Zahl ohne Bruch Teil zurückgibt, die nicht größer ist als der Wert des Arguments.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- floor function [XQuery]
- fn:floor function
ms.assetid: 4ace57dd-b66e-4b60-a2b9-a1b0f1a0831d
author: rothja
ms.author: jroth
ms.openlocfilehash: 206bacf618d38c2dbbe593355d48ed66f7a9e67f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341547"
---
# <a name="numeric-values-functions---floor"></a>Funktionen für numerische Werte – floor
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Gibt die größte Zahl ohne Bruchanteil zurück, die nicht größer als der Wert ihres Arguments ist. Wenn das Argument eine leere Sequenz ist, wird die leere Sequenz zurückgegeben.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:floor ($arg as numeric?) as numeric?  
```  
  
## <a name="arguments"></a>Argumente  
 *$arg*  
 Anzahl, auf die die Funktion angewendet wird.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn der Typ des *$arg* einer der drei numerischen Basis Typen ( **xs: float**, **xs: Double** oder **xs: Decimal**) ist, ist der Rückgabetyp identisch mit dem *$arg* Typ. Wenn der Typ *$arg* ein Typ ist, der von einem der numerischen Typen abgeleitet ist, ist der Rückgabetyp der numerische Basistyp.  
  
 Wenn die Eingabe für die FN: Floor-, FN: ceiling-oder fn: Round-Funktionen **xdt: untypedAtomic**, nicht typisierte Daten ist, wird Sie implizit in **xs: Double** umgewandelt. Alle anderen Typen führen zum Generieren eines statischen Fehlers.  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der AdventureWorks-Beispieldatenbank gespeichert sind.  
  
 Sie können das Arbeitsbeispiel in der [ceiling-Funktion (XQuery)](../xquery/numeric-values-functions-ceiling.md) für die XQuery-Funktion **Floor ()** verwenden. Sie müssen lediglich die **Ceiling ()** -Funktion in der Abfrage durch die **Floor ()** -Funktion ersetzen.  
  
## <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Die **Floor ()** -Funktion ordnet alle ganzzahligen Werte xs: Decimal zu.  
  
## <a name="see-also"></a>Weitere Informationen  
 [CEILING-Funktion &#40;XQuery-&#41;](../xquery/numeric-values-functions-ceiling.md)   
 [Round-Funktion &#40;XQuery-&#41;](../xquery/numeric-values-functions-round.md)   
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
