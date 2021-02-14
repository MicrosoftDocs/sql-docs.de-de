---
title: XQuery-Operatoren für den XML-Datentyp | Microsoft-Dokumentation
description: Erfahren Sie mehr über die XQuery-Operatoren, die für den XML-Datentyp verwendet werden können.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, operators
- operators [XQuery]
- xml data type [SQL Server], XQuery
ms.assetid: 39ca3d2e-e928-4333-872b-75c4ccde8e79
author: rothja
ms.author: jroth
ms.openlocfilehash: c340049fd9655febfe3974a895b9f93e8cf1e2fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352364"
---
# <a name="xquery-operators-against-the-xml-data-type"></a>XQuery-Operatoren für den xml-Datentyp
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  XQuery unterstützt die folgenden Operatoren:  
  
-   Numerische Operatoren (+, -, *, div, mod)  
  
-   Operatoren für Wertvergleiche (eq, ne, lt, gt, le, ge)  
  
-   Operatoren für allgemeine Vergleiche (=,! =, \<, > , \<=, > =)  
  
 Weitere Informationen zu diesen Operatoren finden Sie unter [Vergleichsausdrücke &#40;XQuery&#41;](../xquery/comparison-expressions-xquery.md)  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-general-operators"></a>A. Verwenden von allgemeinen Operatoren  
 Die folgende Abfrage zeigt die Verwendung allgemeiner Operatoren, die für Sequenzen gelten, sowie das Vergleichen von Sequenzen. Die Abfrage ruft eine Sequenz von Telefonnummern für jeden Kunden aus der **AdditionalContactInfo** -Spalte der **Contact** -Tabelle ab. Diese Sequenz wird dann mit der Sequenz von zwei Rufnummern ("111-111-1111", "222-2222") verglichen.  
  
 Die Abfrage verwendet den **=** Vergleichs Operator. Jeder Knoten in der Sequenz auf der rechten Seite des **=** Operators wird mit jedem Knoten in der Sequenz auf der linken Seite verglichen. Wenn die Knoten Stimmen, ist der Knoten Vergleich " **true**". Der Wert wird anschließend in einen int-Wert konvertiert und mit 1 verglichen; die Abfrage gibt dann die Kunden-ID zurück.  
  
```sql
WITH XMLNAMESPACES (  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo' AS ACI,  
'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS ACT)  
SELECT ContactID   
FROM   Person.Contact  
WHERE  AdditionalContactInfo.value('  
      //ACI:AdditionalContactInfo//ACT:telephoneNumber/ACT:number =   
          ("111-111-1111", "222-2222")',  
      'bit')= cast(1 as bit)  
```  
  
 Es gibt eine weitere Möglichkeit, zu beobachten, wie die vorherige Abfrage funktioniert: jeder Wert der Telefonnummer, der aus der **AdditionalContactInfo** -Spalte abgerufen wurde, wird mit dem Satz von zwei Telefonnummern verglichen. Wenn der Wert im Satz enthalten ist, wird der betreffende Kunde im Ergebnis zurückgegeben.  
  
### <a name="b-using-a-numeric-operator"></a>B. Verwenden eines numerischen Operators  
 Der +-Operator in dieser Abfrage ist ein Wertoperator, weil er sich auf ein einzelnes Element bezieht. Der Wert 1 wird z. B. zu einer Chargengröße addiert, die von der Abfrage zurückgegeben wird:  
  
```sql
SELECT ProductModelID, Instructions.query('  
     declare namespace   
 AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
     for $i in (/AWMI:root/AWMI:Location)[1]  
     return   
       <Location LocationID="{ ($i/@LocationID) }"  
                   LotSize  = "{  number($i/@LotSize) }"  
                   LotSize2 = "{ number($i/@LotSize) + 1 }"  
                   LotSize3 = "{ number($i/@LotSize) + 2 }" >  
  
       </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
### <a name="c-using-a-value-operator"></a>C. Verwenden eines Wertoperators  
 Die folgende Abfrage ruft die <`Picture`> Elemente für ein Produktmodell ab, wobei die Bildgröße "Small" ist:  
  
```sql
SELECT CatalogDescription.query('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     for $P in /PD:ProductDescription/PD:Picture[PD:Size eq "small"]  
     return  
           $P  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Da beide Operanden für den **EQ** -Operator atomarische Werte sind, wird der Value-Operator in der Abfrage verwendet. Sie können dieselbe Abfrage mit dem allgemeinen Vergleichs Operator ( **=** ) schreiben.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Funktionen für den XML-Datentyp](../xquery/xquery-functions-against-the-xml-data-type.md)   
 [XML-Daten &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [XQuery-Sprachreferenz &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)  
  
  
