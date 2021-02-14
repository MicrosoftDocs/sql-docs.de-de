---
title: Sum-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die XQuery-Funktion "Sum ()", die die Summe einer Sequenz von Zahlen zurückgibt.
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
- sum function [XQuery]
- fn:sum function
ms.assetid: 12288f37-b54c-4237-b75e-eedc5fe8f96d
author: rothja
ms.author: jroth
ms.openlocfilehash: e9d68fba9560ef4b0fe99f1263e8d65c57a7c6a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341932"
---
# <a name="aggregate-functions---sum"></a>Aggregatfunktionen – sum
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Gibt die Summe einer Sequenz von Zahlen zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
fn:sum($arg as xdt:anyAtomicType*) as xdt:anyAtomicType  
```  
  
## <a name="arguments"></a>Argumente  
 *$arg*  
 Sequenz aus atomaren Werten, deren Summe berechnet werden soll.  
  
## <a name="remarks"></a>Bemerkungen  
 Alle Typen von atomisierten Werten, die an **Sum ()** übermittelt werden, müssen Untertypen desselben Basistyps sein. Zu den akzeptierten Basistypen zählen die drei integrierten numerischen Basistypen oder xdt:untypedAtomic. Werte des Typs xdt:untypedAtomic werden in xs:double umgewandelt. Wenn eine Mischung dieser Typen vorliegt oder andere Werte anderer Typen übermittelt werden, wird ein statischer Fehler ausgelöst.  
  
 Das Ergebnis von **Sum ()** empfängt den Basistyp der im Fall von xdt: untypedAtomic eingegebenen Typen, wie z. b. xs: Double, auch dann, wenn es sich bei der Eingabe optional um die leere Sequenz handelt. Wenn die Eingabe statisch leer ist, ist das Ergebnis 0 mit dem statischen und dynamischen Typ xs:integer.  
  
 Die **Sum ()** -Funktion gibt die Summe der numerischen Werte zurück. Wenn ein xdt: untypedAtomic-Wert nicht in xs: Double umgewandelt werden kann, wird der Wert in der Eingabe Sequenz ignoriert, *$arg*. Wenn die Eingabe eine dynamisch berechnete leere Sequenz ist, wird der Wert 0 des verwendeten Basistyps zurückgegeben.  
  
 Die Funktion gibt einen Laufzeitfehler zurück, wenn ein Ausnahmefehler wegen Überlauf oder Verstoß gegen den Gültigkeitsbereich auftritt.  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der-Datenbank gespeichert sind [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] .  
  
### <a name="a-using-the-sum-xquery-function-to-find-the-total-combined-number-of-labor-hours-for-all-work-center-locations-in-the-manufacturing-process"></a>A. Ermitteln der kombinierten Gesamtanzahl der Arbeitsstunden für alle Arbeitsplatzstandorte im Produktionsprozess mit der XQuery-Funktion sum()  
 Die folgende Abfrage sucht nach der Gesamtanzahl der Arbeitsstunden für alle Arbeitsplatzstandorte im Produktionsprozess aller Produktmodelle, für die Produktionsanweisungen gespeichert sind.  
  
```  
SELECT Instructions.query('         
   declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";         
  <ProductModel PMID= "{ sql:column("Production.ProductModel.ProductModelID") }"         
  ProductModelName = "{ sql:column("Production.ProductModel.Name") }" >         
   <TotalLaborHrs>         
     { sum(//AWMI:Location/@LaborHours) }         
   </TotalLaborHrs>         
 </ProductModel>         
    ') as Result         
FROM Production.ProductModel         
WHERE Instructions is not NULL         
```  
  
 Dies ist das Teilergebnis.  
  
```  
<ProductModel PMID="7" ProductModelName="HL Touring Frame">  
   <TotalLaborHrs>12.75</TotalLaborHrs>  
</ProductModel>  
<ProductModel PMID="10" ProductModelName="LL Touring Frame">  
  <TotalLaborHrs>13</TotalLaborHrs>  
</ProductModel>  
...  
```  
  
 Die Abfrage kann auch so geschrieben werden, dass das Resultset nicht als XML-Code zurückgegeben wird, sondern dass relationale Ergebnisse generiert werden, wie das in der folgenden Abfrage gezeigt wird:  
  
```  
SELECT ProductModelID,         
        Name,         
        Instructions.value('declare namespace   
      AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";         
    sum(//AWMI:Location/@LaborHours)', 'float') as TotalLaborHours         
FROM Production.ProductModel         
WHERE Instructions is not NULL          
```  
  
 Dies ist ein Teilergebnis:  
  
```  
ProductModelID Name                 TotalLaborHours         
-------------- -------------------------------------------------  
7              HL Touring Frame           12.75                   
10             LL Touring Frame           13                      
43             Touring Rear Wheel         3                       
...  
```  
  
### <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Nur die einzige Argument Version von **Sum ()** wird unterstützt.  
  
-   Wenn die Eingabe eine dynamisch berechnete leere Sequenz ist, wird statt des Typs xs:integer der Wert 0 des verwendeten Basistyps zurückgegeben.  
  
-   Die **Sum ()** -Funktion ordnet alle ganzzahligen Werte xs: Decimal zu.  
  
-   Die **Sum ()** -Funktion wird für Werte des Typs xs: Duration nicht unterstützt.  
  
-   Sequenzen, die Typen über Basistypbegrenzungen hinweg mischen, werden nicht unterstützt.  
  
-   Die Summe ((xs: Double ("inf"), xs: Double ("-inf")) löst einen Domänen Fehler aus.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
