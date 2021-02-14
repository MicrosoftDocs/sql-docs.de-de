---
title: Max-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die XQuery Max ()-Funktion, die ein Element in einer Sequenz zurückgibt, dessen Wert größer ist als der Wert aller anderen.
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
- max function [XQuery]
- fn:max function
ms.assetid: 5ee625c0-044a-4cda-b210-02b64e619d65
author: rothja
ms.author: jroth
ms.openlocfilehash: b52ae499cf09e8fcd024f592ae639d0f9a7c1c5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341962"
---
# <a name="aggregate-functions---max"></a>Aggregatfunktionen – max
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Gibt aus einer Sequenz atomarer Werte zurück, *$arg*, das ein Element, dessen Wert größer ist als der aller anderen.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:max($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>Argumente  
 *$arg*  
 Sequenz der atomaren Werte, aus denen der Maximalwert zurückgegeben wird.  
  
## <a name="remarks"></a>Bemerkungen  
 Alle Typen von atomisierten Werten, die an **Max ()** übermittelt werden, müssen Untertypen desselben Basistyps sein. Zulässige Basis Typen sind Typen, die den **gt** -Vorgang unterstützen. Diese Typen sind z. B. die drei integrierten numerischen Basistypen, die date/time-Basistypen, xs:string, xs:boolean und xdt:untypedAtomic. Werte des Typs xdt:untypedAtomic werden in xs:double umgewandelt. Wenn eine Mischung dieser Typen vorliegt oder andere Werte anderer Typen übermittelt werden, wird ein statischer Fehler ausgelöst.  
  
 Das Ergebnis von **Max ()** empfängt den Basistyp der bestandenen Typen, z. b. xs: Double im Fall von xdt: untypedAtomic. Wenn die Eingabe statisch leer ist, wird Empty impliziert und ein statischer Fehler ausgelöst.  
  
 Die **Max ()** -Funktion gibt den einen Wert in der Sequenz zurück, der größer als alle anderen in der Eingabe Sequenz ist. Für xs:string-Werte wird die Unicode-Codepunkt-Standardsortierung verwendet. Wenn ein xdt: untypedAtomic-Wert nicht in xs: Double umgewandelt werden kann, wird der Wert in der Eingabe Sequenz ignoriert, *$arg*. Wenn die Eingabe eine dynamisch berechnete leere Sequenz ist, wird die leere Sequenz zurückgegeben.  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der-Datenbank gespeichert sind [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] .  
  
### <a name="a-using-the-max-xquery-function-to-find-work-center-locations-in-the-manufacturing-process-that-have-the-most-labor-hours"></a>A. Ermitteln der Arbeitsplatzstandorte im Herstellungsprozess mit den meisten Arbeitsstunden unter Verwendung der () XQuery-Funktion  
 Die in der [Min-Funktion (XQuery)](../xquery/aggregate-functions-min.md) bereitgestellte Abfrage kann so umgeschrieben werden, dass die **Max ()** -Funktion verwendet wird.  
  
## <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Die **Max (**)-Funktion ordnet alle ganzzahligen Werte xs: Decimal zu.  
  
-   Die **Max ()** -Funktion wird für Werte des Typs xs: Duration nicht unterstützt.  
  
-   Sequenzen, die Typen über Basistypbegrenzungen hinweg mischen, werden nicht unterstützt.  
  
-   Die Option syntactic, die eine Sortierung bereitstellt, wird nicht unterstützt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
