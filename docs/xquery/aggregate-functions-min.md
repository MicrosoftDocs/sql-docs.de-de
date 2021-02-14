---
title: Min-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die XQuery min ()-Funktion, die ein Element in einer Sequenz zurückgibt, dessen Wert kleiner ist als der aller anderen.
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
- fn:min function
- min function [XQuery]
ms.assetid: db0b7d94-3fa6-488f-96d6-6a9a7d6eda23
author: rothja
ms.author: jroth
ms.openlocfilehash: 19c4204e84759bab022d0f0765b9d6154d23d003
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341945"
---
# <a name="aggregate-functions---min"></a>Aggregatfunktionen – min
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Gibt aus einer Sequenz atomarer Werte zurück, *$arg*, das ein Element, dessen Wert kleiner ist als der aller anderen.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:min($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>Argumente  
 *$arg*  
 Sequenz der Elemente, aus denen der Mindestwert zurückgegeben wird.  
  
## <a name="remarks"></a>Bemerkungen  
 Alle Typen von atomisierten Werten, die an **Min ()** übermittelt werden, müssen Untertypen desselben Basistyps sein. Zulässige Basis Typen sind Typen, die den **gt** -Vorgang unterstützen. Diese Typen sind z. B. die drei integrierten numerischen Basistypen, die date/time-Basistypen, xs:string, xs:boolean und xdt:untypedAtomic. Werte des Typs xdt:untypedAtomic werden in xs:double umgewandelt. Wenn eine Mischung dieser Typen vorliegt oder andere Werte anderer Typen übermittelt werden, wird ein statischer Fehler ausgelöst.  
  
 Das Ergebnis von **Min ()** empfängt den Basistyp der bestandenen Typen, z. b. xs: Double im Fall von xdt: untypedAtomic. Wenn die Eingabe statisch leer ist, wird dies angegeben und ein statischer Fehler zurückgegeben.  
  
 Die **Min ()** -Funktion gibt den einen Wert in der Sequenz zurück, der kleiner als alle anderen in der Eingabe Sequenz ist. Für xs:string-Werte wird die Unicode-Codepunkt-Standardsortierung verwendet. Wenn ein xdt: untypedAtomic-Wert nicht in xs: Double umgewandelt werden kann, wird der Wert in der Eingabe Sequenz ignoriert, *$arg*. Wenn die Eingabe eine dynamisch berechnete leere Sequenz ist, wird die leere Sequenz zurückgegeben.  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der AdventureWorks-Datenbank gespeichert sind.  
  
### <a name="a-using-the-min-xquery-function-to-find-the-work-center-location-that-has-the-fewest-labor-hours"></a>A. Verwenden der min()-Funktion von XQuery zum Bestimmen der Arbeitsplatzstandorte, die die wenigsten Arbeitsstunden aufweisen  
 Die folgende Abfrage ruft alle Arbeitsplatzstandorte im Fertigungsprozess für das Produktmodell (ProductModelID=7) ab, die die wenigsten Arbeitsstunden aufweisen. Im Allgemeinen wird, wie im folgenden Beispiel gezeigt, ein einziger Arbeitsplatzstandort zurückgegeben. Wenn mehrere Standorte eine gleiche Anzahl von Mindestarbeitsstunden aufweisen, werden alle diese Standorte zurückgegeben.  
  
```  
select ProductModelID, Name, Instructions.query('  
  declare namespace AWMI=  
    "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
  for   $Location in /AWMI:root/AWMI:Location  
  where $Location/@LaborHours =  
          min( /AWMI:root/AWMI:Location/@LaborHours )  
return  
  <Location WCID=     "{ $Location/@LocationID }"   
              LaborHrs= "{ $Location/@LaborHours }" />  
  ') as Result   
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Das **Namespace** -Schlüsselwort im XQuery-Prolog definiert ein Namespace Präfix. Dieses Präfix wird anschließend im XQuery-Abfragetext verwendet.  
  
 Der XQuery-Text erstellt den XML-Code, der über ein \<Location> -Element mit wcid-und **laborstd** -Attributen verfügt.  
  
-   Die Abfrage ruft außerdem die ProductModelID- und Namenswerte ab.  
  
 Dies ist das Ergebnis:  
  
```  
ProductModelID   Name              Result  
---------------  ----------------  ---------------------------------  
7                HL Touring Frame  <Location WCID="45" LaborHrs="0.5"/>   
```  
  
## <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Die **Min ()** -Funktion ordnet alle ganzzahligen Werte xs: Decimal zu.  
  
-   Die **Min ()** -Funktion für Werte des Typs xs: Duration wird nicht unterstützt.  
  
-   Sequenzen, die Typen über Basistypbegrenzungen hinweg mischen, werden nicht unterstützt.  
  
-   Die Option syntactic, die eine Sortierung bereitstellt, wird nicht unterstützt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
