---
title: Position-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Position der XQuery-Funktion (), die einen ganzzahligen Wert zurückgibt, der die Position eines Kontext Elements innerhalb einer Sequenz von Elementen angibt.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- position function
- fn:position function
ms.assetid: f1bab9e4-1715-4c06-9cb0-06c7e0c9c97f
author: rothja
ms.author: jroth
ms.openlocfilehash: dd2be5cf246bd625e63c31975effa59fb1b919d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345035"
---
# <a name="context-functions---position-xquery"></a>Kontextfunktionen – position (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Gibt einen ganzzahligen Wert zurück, der die Position des Kontextelements in der Sequenz der Elemente angibt, die derzeit verarbeitet werden.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:position() as xs:integer  
```  
  
## <a name="remarks"></a>Bemerkungen  
 In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] kann **FN: Position ()** nur im Kontext eines kontextabhängigen Prädikats verwendet werden. Die Funktion kann insbesondere nur innerhalb von eckigen Klammern ([ ]) verwendet werden. Das Vergleichen mit dieser Funktion verringert nicht die Kardinalität während des statischen Typrückschlusses.  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der-Datenbank gespeichert sind [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] .  
  
### <a name="a-using-the-position-xquery-function-to-retrieve-the-first-two-product-features"></a>A. Abrufen der ersten beiden Produktfunktionen mit der XQuery-Funktion position()  
 Die folgende Abfrage ruft die ersten beiden Features, die ersten beiden untergeordneten Elemente des <`Features`>-Element, aus der Produktmodell-Katalogbeschreibung ab. Wenn weitere Funktionen vorhanden sind, wird dem Ergebnis ein <>-Element hinzugefügt `there-is-more/` .  
  
```  
SELECT CatalogDescription.query('  
     declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <Product>   
          { /pd:ProductDescription/@ProductModelID }  
          { /pd:ProductDescription/@ProductModelName }   
          {  
            for $f in /pd:ProductDescription/pd:Features/*[position()<=2]  
            return  
            $f   
          }  
          {  
            if (count(/pd:ProductDescription/pd:Features/*) > 2)  
            then <there-is-more/>  
            else ()  
          }   
     </Product>          
') as x  
FROM Production.ProductModel  
WHERE CatalogDescription is not null  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Das **Namespace** -Schlüsselwort im [XQuery-Prolog](../xquery/modules-and-prologs-xquery-prolog.md) definiert ein Namespace Präfix, das im Abfragetext verwendet wird.  
  
-   Der Abfragetext erstellt XML, das über ein \<Product> -Element mit dem **ProductModelID** -Attribut und dem **ProductModelName** -Attribut verfügt und die Produktfunktionen als untergeordnete Elemente zurückgibt.  
  
-   Die **Position ()** -Funktion wird im Prädikat verwendet, um die Position des untergeordneten \<Features> Elements im Kontext zu ermitteln. Wenn es sich dabei um die erste oder zweite Funktion handelt, wird diese zurückgegeben.  
  
-   Die if-Anweisung fügt \<there-is-more/> dem Ergebnis ein-Element hinzu, wenn mehr als zwei Funktionen im Produktkatalog vorhanden sind.  
  
-   Da nicht für alle Produktmodelle Katalogbeschreibungen in der Tabelle gespeichert sind, wird die WHERE-Klausel zum Verwerfen von Zeilen verwendet, in denen CatalogDescriptions den Wert NULL besitzt.  
  
 Dies ist ein Teilergebnis:  
  
```  
<Product ProductModelID="19" ProductModelName="Mountain 100">  
  <p1:Warranty xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p1:WarrantyPeriod>3 year</p1:WarrantyPeriod>  
    <p1:Description>parts and labor</p1:Description>  
  </p1:Warranty>  
  <p2:Maintenance xmlns:p2="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <p2:NoOfYears>10</p2:NoOfYears>  
    <p2:Description>maintenance contact available through your dealer or  
                    any AdventureWorks retail store.</p2:Description>  
  </p2:Maintenance>  
  <there-is-more/>  
</Product>   
...  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
