---
title: unterschiedliche Values-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Funktion mit unterschiedlichen Werten in einer XQuery verwenden, um doppelte Werte aus einer Sequenz zu entfernen.
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
- distinct-values function
- fn:distinct-values function
ms.assetid: f4c2bb53-2bec-4f1a-9c00-cf997fb7ae5b
author: rothja
ms.author: jroth
ms.openlocfilehash: b9a3561c25ca0c64e01c2603b64656320d499518
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341907"
---
# <a name="functions-on-sequences---distinct-values"></a>Funktionen für Sequenzen – distinct-values
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Entfernt doppelte Werte aus der durch *$arg* angegebenen Sequenz. Wenn *$arg* eine leere Sequenz ist, gibt die Funktion die leere Sequenz zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:distinct-values($arg as xdt:anyAtomicType*) as xdt:anyAtomicType*  
```  
  
## <a name="arguments"></a>Argumente  
 *$arg*  
 Sequenz der atomaren Werte.  
  
## <a name="remarks"></a>Bemerkungen  
 Alle Typen von atomisierten Werten, die an unter **schiedliche Werte ()** übermittelt werden, müssen Untertypen desselben Basistyps sein. Zulässige Basis Typen sind Typen, die den **EQ** -Vorgang unterstützen. Diese Typen sind z. B. die drei integrierten numerischen Basistypen, die date/time-Basistypen, xs:string, xs:boolean und xdt:untypedAtomic. Werte des Typs xdt:untypedAtomic werden in xs:string umgewandelt. Wenn eine Mischung dieser Typen vorliegt oder andere Werte anderer Typen übergeben werden, wird ein statischer Fehler ausgelöst.  
  
 Das Ergebnis von unter **schiedlichen Werten ()** empfängt den Basistyp der Übergabe Typen, z. b. xs: String im Fall von xdt: untypedAtomic, mit der ursprünglichen Kardinalität. Wenn die Eingabe statisch leer ist, wird Empty impliziert und ein statischer Fehler ausgelöst.  
  
 Werte vom Typ xs:string werden mit der Unicode-Codepunkt-Standardsortierung von XQuery verglichen.  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der AdventureWorks-Datenbank gespeichert sind.  
  
### <a name="a-using-the-distinct-values-function-to-remove-duplicate-values-from-the-sequence"></a>A. Verwenden der distinct-values()-Funktion zum Entfernen doppelter Werte aus der Sequenz  
 In diesem Beispiel wird eine XML-Instanz, die Telefonnummern enthält, einer Variablen vom Typ **XML** zugewiesen. Die für diese Variable angegebene XQuery verwendet die unter **schiedliche-Values ()-** Funktion, um eine Liste von Telefonnummern zu kompilieren, die keine Duplikate enthalten.  
  
```  
declare @x xml  
set @x = '<PhoneNumbers>  
 <Number>111-111-1111</Number>  
 <Number>111-111-1111</Number>  
 <Number>222-222-2222</Number>  
</PhoneNumbers>'  
-- 1st select  
select @x.query('  
  distinct-values( data(/PhoneNumbers/Number) )  
') as result  
```  
  
 Dies ist das Ergebnis:  
  
```  
111-111-1111 222-222-2222    
```  
  
 In der folgenden Abfrage wird eine Sequenz von Zahlen (1, 1, 2) an die unter **schiedliche-Values ()-** Funktion übermittelt. Die Funktion entfernt anschließend die doppelten Werte in der Sequenz und gibt die beiden verbleibenden Werte zurück.  
  
```  
declare @x xml  
set @x = ''  
select @x.query('  
  distinct-values((1, 1, 2))  
') as result  
```  
  
 Die Abfrage gibt 1 2 zurück.  
  
### <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Die unter **schiedliche-Values ()-** Funktion ordnet ganzzahlige Werte xs: Decimal zu.  
  
-   Die unter **schiedliche-Values ()-** Funktion unterstützt nur die zuvor erwähnten Typen und unterstützt nicht die Mischung von Basis Typen.  
  
-   Die unter **schiedliche-Values ()-** Funktion für xs: Duration-Werte wird nicht unterstützt.  
  
-   Die Option syntactic, die eine Sortierung bereitstellt, wird nicht unterstützt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
