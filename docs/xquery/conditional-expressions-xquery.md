---
title: Bedingte Ausdrücke (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die bedingten Ausdrücke, die von XQuery unterstützt werden.
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
- XQuery, conditional expressions
- expressions [XQuery], conditional
- effective Boolean value [XQuery]
- if-then-else statement [XQuery]
- conditional expressions [XQuery]
- EBV
ms.assetid: b280dd96-c80f-4c51-bc06-a88d42174acb
author: rothja
ms.author: jroth
ms.openlocfilehash: 5fdc8703482f570d0ecb749b471af170b62770ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349368"
---
# <a name="conditional-expressions-xquery"></a>Bedingte Ausdrücke (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  XQuery unterstützt die folgende bedingte **if-then-else-** Anweisung:  
  
```  
if (<expression1>)  
then  
  <expression2>  
else  
  <expression3>  
```  
  
 Abhängig vom effektiven booleschen Wert von `expression1` wird entweder `expression2` oder `expression3` ausgewertet. Beispiel:  
  
-   Wenn der Testausdruck `expression1` eine leere Sequenz ergibt, ist das Ergebnis "False".  
  
-   Wenn der Testausdruck `expression1` einen einfachen booleschen Wert ergibt, gilt dieser Wert als Ergebnis des Ausdrucks.  
  
-   Wenn der Testausdruck `expression1` eine Sequenz aus einem oder mehreren Knoten ergibt, ist das Ergebnis des Ausdrucks "True".  
  
-   Anderenfalls wird ein statischer Fehler ausgegeben.  
  
 Beachten Sie dabei außerdem Folgendes:  
  
-   Der Testausdruck muss in Klammern stehen.  
  
-   Der **else** -Ausdruck ist erforderlich. Falls Sie ihn nicht benötigen, können Sie " ( ) " zurückgeben, wie dies in den Beispielen der Fall ist.  
  
 Beispielsweise wird die folgende Abfrage für die Variable vom Typ **XML** angegeben. Die **if** -Bedingung testet den Wert der SQL-Variablen ( @v ) innerhalb des XQuery-Ausdrucks mithilfe der [SQL: Variable ()-Funktions](../xquery/xquery-extension-functions-sql-variable.md) Erweiterungs Funktion. Wenn der Variablen Wert "FirstName" ist, wird der <>-Element zurückgegeben `FirstName` . Andernfalls wird das <>- `LastName` Element zurückgegeben.  
  
```  
declare @x xml  
declare @v varchar(20)  
set @v='FirstName'  
set @x='  
<ROOT rootID="2">  
  <FirstName>fname</FirstName>  
  <LastName>lname</LastName>  
</ROOT>'  
SELECT @x.query('  
if ( sql:variable("@v")="FirstName" ) then  
  /ROOT/FirstName  
 else  
   /ROOT/LastName  
')  
```  
  
 Dies ist das Ergebnis:  
  
```  
<FirstName>fname</FirstName>  
```  
  
 Die folgende Abfrage ruft die beiden ersten Funktionsbeschreibungen der Produktkatalogbeschreibung eines bestimmten Produktmodells ab. Wenn im Dokument weitere Funktionen vorhanden sind, wird ein <>- `there-is-more` Element mit leerem Inhalt hinzugefügt.  
  
```  
SELECT CatalogDescription.query('  
     declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <Product>   
          { /p1:ProductDescription/@ProductModelID }  
          { /p1:ProductDescription/@ProductModelName }   
          {  
            for $f in /p1:ProductDescription/p1:Features/*[position()\<=2]  
            return  
            $f   
          }  
          {  
            if (count(/p1:ProductDescription/p1:Features/*) > 2)  
            then \<there-is-more/>  
            else ()  
          }   
     </Product>          
') as x  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 In der vorherigen Abfrage überprüft die Bedingung im **if** -Ausdruck, ob in <> mehr als zwei untergeordnete Elemente vorhanden sind `Features` . Wenn dies der Fall ist, wird als Ergebnis ein `\<there-is-more/>`-Element zurückgegeben.  
  
 Dies ist das Ergebnis:  
  
```  
<Product ProductModelID="19" ProductModelName="Mountain 100">  
  \<p1:Warranty xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    \<p1:WarrantyPeriod>3 years\</p1:WarrantyPeriod>  
    \<p1:Description>parts and labor\</p1:Description>  
  \</p1:Warranty>  
  \<p2:Maintenance xmlns:p2="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    \<p2:NoOfYears>10 years\</p2:NoOfYears>  
    \<p2:Description>maintenance contract available through your dealer or any AdventureWorks retail store.\</p2:Description>  
  \</p2:Maintenance>  
  \<there-is-more />  
</Product>  
```  
  
 In der folgenden Abfrage wird ein <`Location`> Element mit einem LocationID-Attribut zurückgegeben, wenn der Arbeitsplatz Standort die Setup Stunden nicht angibt.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        for $WC in //AWMI:root/AWMI:Location  
        return  
        if ( $WC[not(@SetupHours)] )  
        then  
          <WorkCenterLocation>  
             { $WC/@LocationID }   
          </WorkCenterLocation>  
         else  
           ()  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Dies ist das Ergebnis:  
  
```  
<WorkCenterLocation LocationID="30" />  
<WorkCenterLocation LocationID="45" />  
<WorkCenterLocation LocationID="60" />  
```  
  
 Diese Abfrage kann ohne die **if** -Klausel geschrieben werden, wie im folgenden Beispiel gezeigt:  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $WC in //AWMI:root/AWMI:Location[not(@SetupHours)]   
        return  
          <Location>  
             { $WC/@LocationID }   
          </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Ausdrücke](../xquery/xquery-expressions.md)  
  
  
