---
title: number-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die XQuery-Funktions Nummer (), die den numerischen Wert eines angegebenen Arguments zurückgibt.
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
- number function
- fn:number function
ms.assetid: dff6d19b-765c-4df9-afff-9a0e7be9b91b
author: rothja
ms.author: jroth
ms.openlocfilehash: 6aa035be000fbbda12a7205c33925daf656c3da0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345018"
---
# <a name="functions-on-nodes---number"></a>Funktionen für Knoten – number
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Gibt den numerischen Wert des Knotens zurück, der durch *$arg* angegeben wird.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:number() as xs:double?   
fn:number($arg as node()?) as xs:double?  
```  
  
## <a name="arguments"></a>Argumente  
 *$arg*  
 Knoten, dessen Wert als Zahl zurückgegeben wird.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn *$arg* nicht angegeben wird, wird der numerische Wert des Kontext Knotens zurückgegeben, der in einen Double-Wert konvertiert wird. In SQL Server kann **FN: Number ()** ohne Argument nur im Kontext eines kontextabhängigen Prädikats verwendet werden. Insbesondere kann die Funktion nur innerhalb von Klammern ([ ]) verwendet werden. Der folgende Ausdruck gibt beispielsweise den <`ROOT`>-Element zurück.  
  
```  
declare @x xml  
set @x='<ROOT>111</ROOT>'  
select @x.query('/ROOT[number()=111]')  
```  
  
 Wenn der Wert des Knotens keine gültige lexikalische Darstellung eines numerischen einfachen Typs ist, wie in **XML Schema Part 2: Datatypes, W3C-Empfehlung** definiert, gibt die Funktion eine leere Sequenz zurück. NaN wird nicht unterstützt.  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der AdventureWorks-Datenbank gespeichert sind.  
  
### <a name="a-using-the-number-xquery-function-to-retrieve-the-numeric-value-of-an-attribute"></a>A. Verwenden der number()-Funktion von XQuery zum Abrufen des numerischen Werts eines Attributs  
 Die folgende Abfrage ruft den numerischen Wert des LotSize-Attributs aus dem ersten Arbeitsplatzstandort im Fertigungsvorgang von Produktmodell 7 ab.  
  
```  
SELECT ProductModelID, Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     for $i in (//AWMI:root//AWMI:Location)[1]  
     return   
       <Location LocationID="{ ($i/@LocationID) }"   
                   LotSizeA="{  $i/@LotSize }"  
                   LotSizeB="{  number($i/@LotSize) }"  
                   LotSizeC="{ number($i/@LotSize) + 1 }" >  
  
       </Location>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Die **Number ()** -Funktion ist nicht erforderlich, wie von der Abfrage für das **LotSizeA** -Attribut angezeigt. Es handelt sich dabei um eine XPath 1.0-Funktion, die hauptsächlich aus Gründen der Abwärtskompatibilität enthalten ist.  
  
-   Die XQuery für **LotSizeB** gibt die number-Funktion an und ist redundant.  
  
-   Die Abfrage für die **lotgrößen** -Abfrage veranschaulicht die Verwendung eines Zahlen Werts in einer arithmetischen Operation.  
  
 Dies ist das Ergebnis:  
  
```  
ProductModelID   Result  
----------------------------------------------  
7              <Location LocationID="10"   
                         LotSizeA="100"   
                         LotSizeB="100"   
                         LotSizeC="101" />  
```  
  
### <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Die **Number ()** -Funktion akzeptiert nur Knoten. Sie nimmt keine atomaren Werte an.  
  
-   Wenn Werte nicht als Zahl zurückgegeben werden können, gibt die **Number ()** -Funktion die leere Sequenz anstelle von NaN zurück.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
