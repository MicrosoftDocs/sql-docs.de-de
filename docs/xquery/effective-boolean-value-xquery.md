---
title: Effektiver boolescher Wert (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über effektive boolesche Werte in XQuery.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- effective Boolean value [XQuery]
- Boolean values
- XQuery, effective Boolean values
- EBV
ms.assetid: 506682b1-b6c9-45e2-aa54-7abd5844c3f1
author: rothja
ms.author: jroth
ms.openlocfilehash: c9e390546feb848f965df2bfd60a2cc68c8fb3ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341554"
---
# <a name="effective-boolean-value-xquery"></a>Effektiver boolescher Wert (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Folgende Werte sind effektive boolesche Werte:  
  
-   False, wenn der Operand eine leere Sequenz oder ein boolesches False ist.  
  
-   Anderenfalls ist der Wert True.  
  
 Der effektive boolesche Wert kann für Ausdrücke berechnet werden, die einen einzelnen booleschen Wert, eine Knotensequenz oder eine leere Sequenz zurückgeben. Beachten Sie, dass der boolesche Wert implizit berechnet wird, wenn folgende Typen von Ausdrücken verarbeitet werden:  
  
-   Logische Ausdrücke  
  
-   Die [not-Funktion](../xquery/functions-on-boolean-values-not-function.md)  
  
-   Die WHERE-Klausel eines FLWOR-Ausdrucks  
  
-   [Bedingte Ausdrücke](../xquery/conditional-expressions-xquery.md)  
  
-   [Quantifizierte Ausdrücke](../xquery/quantified-expressions-xquery.md)  
  
 Das folgende Beispiel zeigt einen effektiven booleschen Wert. Wenn der **if** -Ausdruck verarbeitet wird, wird der effektive boolesche Wert der Bedingung bestimmt. Da `/a[1]` eine leere Sequenz zurückgibt, ist der effektive boolesche Wert False. Das Ergebnis wird als XML mit einem Textknoten (false) zurückgegeben.  
  
```  
value is false  
DECLARE @x XML  
SET @x = '<b/>'  
SELECT @x.query('if (/a[1]) then "true" else "false"')  
go  
```  
  
 Im folgenden Beispiel ist der effektive boolesche Wert True, weil der Ausdruck eine nicht leere Sequenz zurückgibt.  
  
```  
DECLARE @x XML  
SET @x = '<a/>'  
SELECT @x.query('if (/a[1]) then "true" else "false"')  
go  
```  
  
 Beim Abfragen von typisierten **XML** -Spalten oder-Variablen können Knoten des booleschen Typs vorhanden sein. In diesem Fall gibt die **Daten ()** einen booleschen Wert zurück. Wenn der Abfrageausdruck einen booleschen Wert zurückgibt, ist der effektive boolesche Wert True, was im nächsten Beispiel dargestellt ist. Das Beispiel zeigt außerdem Folgendes:  
  
-   Es wird eine XML-Schemaauflistung erstellt. Das Element \<b> in der Auflistung weist einen booleschen Typ auf.  
  
-   Eine typisierte **XML** -Variable wird erstellt und abgefragt.  
  
-   Der Ausdruck `data(/b[1])` gibt einen booleschen True-Wert zurück. Der effektive boolesche Wert ist deshalb in diesem Fall True.  
  
-   Der Ausdruck `data(/b[2])` gibt einen booleschen Wert false zurück. Der effektive boolesche Wert ist deshalb in diesem Fall False.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="s" type="string"/>  
      <element name="b" type="boolean"/>  
</schema>'  
go  
DECLARE @x XML(SC)  
SET @x = '<b>true</b><b>false</b>'  
SELECT @x.query('if (data(/b[1])) then "true" else "false"')  
SELECT @x.query('if (data(/b[2])) then "true" else "false"')  
go  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Grundlagen](../xquery/xquery-basics.md)   
 [FLWOR-Anweisung und Iterations &#40;XQuery&#41;](../xquery/flwor-statement-and-iteration-xquery.md)  
  
  
