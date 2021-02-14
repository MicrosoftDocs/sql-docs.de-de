---
title: Atomisierung (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über den atomisierungsprozess in XQuery, in dem die typisierten Werte eines Elements extrahiert werden.
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, atomization
- atomization [XQuery]
ms.assetid: e3d7cf2f-c6fb-43c2-8538-4470a6375af5
author: rothja
ms.author: jroth
ms.openlocfilehash: e12ac51b38073363edf7774f4ac8046c1985b38c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340529"
---
# <a name="atomization-xquery"></a>Atomisierung (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Atomisierung ist das Verfahren, durch das der typisierte Wert eines Elements extrahiert wird. Dieses Verfahren wird unter bestimmten Umständen implizit verwendet. Bestimmte XQuery-Operatoren, wie z. B. arithmetische und Vergleichsoperatoren, sind von diesem Verfahren abhängig. Wenn Sie z. b. arithmetische Operatoren direkt auf Knoten anwenden, wird der typisierte Wert eines Knotens zuerst durch den impliziten Aufruf der [Data-Funktion](../xquery/data-accessor-functions-data-xquery.md)abgerufen. Auf diese Weise wird der atomare Wert dem arithmetischen Operator als Operand übergeben.  
  
 Die folgende Abfrage gibt beispielsweise den Gesamtwert der LaborHours-Attribute zurück. In diesem Fall werden die **Daten ()** implizit auf die Attribut Knoten angewendet.  
  
```  
declare @x xml  
set @x='<ROOT><Location LID="1" SetupTime="1.1" LaborHours="3.3" />  
<Location LID="2" SetupTime="1.0" LaborHours="5" />  
<Location LID="3" SetupTime="2.1" LaborHours="4" />  
</ROOT>'  
-- data() implicitly applied to the attribute node sequence.  
SELECT @x.query('sum(/ROOT/Location/@LaborHours)')  
```  
  
 Auch wenn dies nicht erforderlich ist, können Sie die **Data ()** -Funktion explizit angeben:  
  
```  
SELECT @x.query('sum(data(ROOT/Location/@LaborHours))')  
```  
  
 Ein anderes Beispiel für die implizite Atomisierung ist das Verwenden von arithmetischen Operatoren. Der **+** Operator erfordert atomarische Werte, und **Data ()** wird implizit angewendet, um den atomaren Wert des LaborHours-Attributs abzurufen. Die Abfrage wird für die Instructions-Spalte des **XML** -Typs in der ProductModel-Tabelle angegeben. Die folgende Abfrage gibt das LaborHours-Attribut dreimal zurück. Beachten Sie in der Abfrage Folgendes:  
  
-   Bei der Konstruktion des OriginalLaborHours-Attributs wird die Atomisierung implizit auf die von (`$WC/@LaborHours`) zurückgegebene Singleton-Sequenz angewendet. Der typisierte Wert des LaborHours-Attributs wird anschließend dem OriginalLaborHours-Attribut zugewiesen.  
  
-   Bei der Konstruktion des UpdatedLaborHoursV1-Attributs macht der arithmetische Operator atomare Werte erforderlich. Daher werden die **Daten ()** implizit auf das LaborHours-Attribut angewendet, das von () zurückgegeben wird `$WC/@LaborHours` . Anschließend wird dem Attribut der atomare Wert 1 hinzugefügt. Die Erstellung des Attributs UpdatedLaborHoursV2 zeigt die explizite Anwendung der **Daten ()**, ist aber nicht erforderlich.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $WC in /AWMI:root/AWMI:Location[1]  
        return  
            <WC OriginalLaborHours = "{ $WC/@LaborHours }"  
                UpdatedLaborHoursV1 = "{ $WC/@LaborHours + 1 }"   
                UpdatedLaborHoursV2 = "{ data($WC/@LaborHours) + 1 }" >  
            </WC>') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Dies ist das Ergebnis:  
  
```  
<WC OriginalLaborHours="2.5"   
    UpdatedLaborHoursV1="3.5"   
    UpdatedLaborHoursV2="3.5" />  
```  
  
 Aus der Atomisierung ergibt sich eine Instanz eines einfachen Datentyps, ein leeres Dataset oder ein Fehler bezüglich des statischen Typs.  
  
 Die Atomisierung tritt auch in Vergleichs Ausdrucks Parametern auf, die an Funktionen, von Functions zurückgegebene Werte, **Cast ()** -Ausdrücke und in der ORDER BY-Klausel übergebenen Sortier Ausdrücken ausgegeben werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Grundlagen](../xquery/xquery-basics.md)   
 [Vergleichsausdrücke &#40;XQuery-&#41;](../xquery/comparison-expressions-xquery.md)   
 [XQuery-Funktionen für den xml-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  
