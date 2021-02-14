---
title: FLWOR-Anweisung und-Iterationen (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Klauseln for, Let, WHERE, Order by und Return, aus denen sich die FLOWR-Iterations Syntax in XQuery zusammensetzen lässt.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- return clause
- FLWOR statement
- effective Boolean value [XQuery]
- multiple variable binding
- order by clause [XQuery]
- for clause [XQuery]
- where clause [XQuery]
- iterations [XQuery]
- XQuery, FLWOR statement
- EBV
ms.assetid: d7cd0ec9-334a-4564-bda9-83487b6865cb
author: rothja
ms.author: jroth
ms.openlocfilehash: daca777a2994813a6df131d1fa756edf4f753f6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353315"
---
# <a name="flwor-statement-and-iteration-xquery"></a>FLWOR-Anweisung und -Iteration (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  XQuery definiert die Iterationssyntax FLWOR. FLWOR ist das Akronym für `for`, `let`, `where`, `order by` und `return`.  
  
 Eine FLWOR-Anweisung besteht aus den folgenden Teilen:  
  
-   Einer oder mehreren FOR-Klauseln, die eine oder mehrere Iteratorvariablen an Eingabesequenzen binden.  
  
     Eingabesequenzen können andere XQuery-Ausdrücke wie z. B. XPath-Ausdrücke sein. Es handelt sich entweder um Sequenzen von Knoten oder um Sequenzen atomarer Werte. Sequenzen atomarer Werte können mithilfe von Literalen oder Konstruktorfunktionen erstellt werden. Erstellte XML-Knoten sind als Eingabesequenzen in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nicht zulässig.  
  
-   Einer optionalen `let`-Klausel. Diese Klausel weist der angegebenen Variable für eine bestimmte Iteration einen Wert zu. Der zugewiesene Ausdruck kann ein XQuery-Ausdruck, z. B. ein XPath-Ausdruck sein, und entweder eine Sequenz aus Knoten oder eine Sequenz aus atomaren Werten zurückgeben. Sequenzen aus atomaren Werten können mithilfe von Literalen oder Konstruktorfunktionen erstellt werden. Erstellte XML-Knoten sind als Eingabesequenzen in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nicht zulässig.  
  
-   Einer Iteratorvariablen. Diese Variable kann eine optionale Typassertion mithilfe des `as`-Schlüsselworts besitzen.  
  
-   Einer optionalen `where`-Klausel. Diese Klausel wendet ein Filterprädikat auf die Iteration an.  
  
-   Einer optionalen `order by`-Klausel.  
  
-   Ein `return`-Ausdruck. Der Ausdruck in der `return`-Klausel erstellt das Ergebnis der FLWOR-Anweisung.  
  
 Die folgende Abfrage durchläuft z. b. die <`Step`> Elemente am ersten Produktions Speicherort und gibt den Zeichen folgen Wert der <`Step`> Knoten zurück:  
  
```sql
declare @x xml  
set @x='<ManuInstructions ProductModelID="1" ProductModelName="SomeBike" >  
<Location LocationID="L1" >  
  <Step>Manu step 1 at Loc 1</Step>  
  <Step>Manu step 2 at Loc 1</Step>  
  <Step>Manu step 3 at Loc 1</Step>  
</Location>  
<Location LocationID="L2" >  
  <Step>Manu step 1 at Loc 2</Step>  
  <Step>Manu step 2 at Loc 2</Step>  
  <Step>Manu step 3 at Loc 2</Step>  
</Location>  
</ManuInstructions>'  
SELECT @x.query('  
   for $step in /ManuInstructions/Location[1]/Step  
   return string($step)  
')  
```  
  
 Dies ist das Ergebnis:  
  
```  
Manu step 1 at Loc 1 Manu step 2 at Loc 1 Manu step 3 at Loc 1  
```  
  
 Die folgende Abfrage ähnelt der vorherigen Abfrage, wird jedoch für die Instructions-Spalte, eine typisierte xml-Spalte, der ProductModel-Tabelle angegeben. Die Abfrage durchläuft alle Fertigungsschritte, <`step`> Elemente, am ersten Arbeitsplatz Standort für ein bestimmtes Produkt.  
  
```sql
SELECT Instructions.query('  
   declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $Step in //AWMI:root/AWMI:Location[1]/AWMI:step  
      return  
           string($Step)   
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   `$Step` ist die Iteratorvariable.  
  
-   Der [Pfad Ausdruck](../xquery/path-expressions-xquery.md) `//AWMI:root/AWMI:Location[1]/AWMI:step` generiert die Eingabe Sequenz. Diese Sequenz ist die Sequenz der <untergeordneten `step` Knoten> Elements des ersten <`Location`> Element Knotens.  
  
-   Die optionale Prädikatklausel `where` wird nicht verwendet.  
  
-   Der `return` Ausdruck gibt einen Zeichen folgen Wert aus dem <`step`>-Element zurück.  
  
 Die [Zeichen folgen Funktion (XQuery)](../xquery/data-accessor-functions-string-xquery.md) wird verwendet, um den Zeichen folgen Wert des `step` Knotens <> abzurufen.  
  
 Dies ist das Teilergebnis:  
  
```  
Insert aluminum sheet MS-2341 into the T-85A framing tool.   
Attach Trim Jig TJ-26 to the upper and lower right corners of   
the aluminum sheet. ....         
```  
  
 Die folgenden Beispiele zeigen weitere zusätzliche Eingabesequenzen, die zulässig sind:  
  
```sql
declare @x xml  
set @x=''  
SELECT @x.query('  
for $a in (1, 2, 3)  
  return $a')  
-- result = 1 2 3   
  
declare @x xml  
set @x=''  
SELECT @x.query('  
for $a in   
   for $b in (1, 2, 3)  
      return $b  
return $a')  
-- result = 1 2 3  
  
declare @x xml  
set @x='<ROOT><a>111</a></ROOT>'  
SELECT @x.query('  
  for $a in (xs:string( "test"), xs:double( "12" ), data(/ROOT/a ))  
  return $a')  
-- result test 12 111  
```  
  
 In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sind heterogene Sequenzen nicht zulässig. Insbesondere sind Sequenzen, die eine Mischung aus atomaren Werten und Knoten enthalten, nicht zulässig.  
  
 Iterationen werden häufig zusammen mit der [XML-Konstruktions](../xquery/xml-construction-xquery.md) Syntax zum Transformieren von XML-Formaten verwendet, wie in der nächsten Abfrage gezeigt.  
  
 In der AdventureWorks-Beispieldatenbank verfügen die in der **instructions** -Spalte der **Production. ProductModel** -Tabelle gespeicherten Fertigungsanweisungen über die folgende Form:  
  
```xml
<Location LocationID="10" LaborHours="1.2"   
            SetupHours=".2" MachineHours=".1">  
  <step>describes 1st manu step</step>  
   <step>describes 2nd manu step</step>  
   ...  
</Location>  
...  
```  
  
 Mit der folgenden Abfrage wird ein neuer XML-Code erstellt, der über die <>-Elemente verfügt, `Location` wobei die Attribute des Arbeitsplatz Standorts als untergeordnete Elemente  
  
```xml
<Location>  
   <LocationID>10</LocationID>  
   <LaborHours>1.2</LaborHours>  
   <SetupHours>.2</SetupHours>  
   <MachineHours>.1</MachineHours>  
</Location>  
...  
```  
  
 Im Folgenden wird die Abfrage aufgeführt:  
  
```sql
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        for $WC in /AWMI:root/AWMI:Location  
        return  
          <Location>  
            <LocationID> { data($WC/@LocationID) } </LocationID>  
            <LaborHours>   { data($WC/@LaborHours) }   </LaborHours>  
            <SetupHours>   { data($WC/@SetupHours) }   </SetupHours>  
            <MachineHours> { data($WC/@MachineHours) } </MachineHours>  
          </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Die FLWOR-Anweisung ruft eine Sequenz von <`Location`> Elemente für ein bestimmtes Produkt ab.  
  
-   Die [Data-Funktion (XQuery)](../xquery/data-accessor-functions-data-xquery.md) wird verwendet, um den Wert jedes Attributs zu extrahieren, sodass Sie dem resultierenden XML-Code als Textknoten anstelle von Attributen hinzugefügt werden.  
  
-   Der Ausdruck in der RETURN-Klausel erstellt das von Ihnen gewünschte XML.  
  
 Dies ist ein Teilergebnis:  
  
```xml
<Location>  
  <LocationID>10</LocationID>  
  <LaborHours>2.5</LaborHours>  
  <SetupHours>0.5</SetupHours>  
  <MachineHours>3</MachineHours>  
</Location>  
<Location>  
   ...  
<Location>  
...  
```  
  
## <a name="using-the-let-clause"></a>Verwenden der let-Klausel  
 Mithilfe der `let`-Klausel können Sie wiederholte Ausdrücke benennen, auf die Sie durch einen Verweis auf die Variable verweisen können. Der Ausdruck, der einer `let`-Variable zugewiesen ist, wird jedes Mal in die Abfrage eingefügt wird, wenn in der Abfrage auf die Variable verwiesen wird. Das bedeutet, dass die Anweisung so oft ausgeführt wird, wie auf den Ausdruck verwiesen wird.  
  
 In der [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)]-Datenbank enthalten die Produktionsanweisungen Informationen zu den erforderlichen Tools und der Position, an der die Tools verwendet werden. In der folgenden Abfrage wird die `let`-Klausel verwendet, um die Tools, die zum Erstellen eines Produktionsmodells erforderlich sind, sowie die Positionen aufzulisten, an denen die einzelnen Tools benötigt werden.  
  
```sql
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        for $T in //AWMI:tool  
            let $L := //AWMI:Location[.//AWMI:tool[.=data($T)]]  
        return  
          <tool desc="{data($T)}" Locations="{data($L/@LocationID)}"/>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
## <a name="using-the-where-clause"></a>Verwenden der where-Klausel  
 Sie können die- `where` Klausel verwenden, um die Ergebnisse einer Iterationen zu filtern. Dies wird im folgenden Beispiel veranschaulicht.  
  
 Bei der Produktion eines Fahrrades durchläuft der Produktionsprozess eine Reihe von Arbeitsplatzstandorten. Jeder Arbeitsplatzstandort definiert eine Sequenz von Produktionsschritten. Die folgende Abfrage ruft nur die Arbeitsplatzstandorte ab, die ein Fahrradmodell fertigen und weniger als drei Produktionsschritte verwenden. Das heißt, Sie verfügen über weniger als drei <`step`> Elemente.  
  
```sql
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $WC in /AWMI:root/AWMI:Location  
      where count($WC/AWMI:step) < 3  
      return  
          <Location >  
           { $WC/@LocationID }   
          </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Beachten Sie in der vorhergehenden Abfrage Folgendes:  
  
-   Das- `where` Schlüsselwort verwendet die **count ()** -Funktion, um die Anzahl der <> untergeordneten `step` Elementen an jedem Arbeitsplatz Standort zu zählen.  
  
-   Der `return`-Ausdruck erstellt das von Ihnen gewünschte XML aus den Ergebnissen der Iteration.  
  
 Dies ist das Ergebnis:  
  
```  
<Location LocationID="30"/>   
```  
  
 Das Ergebnis des Ausdrucks in der `where`-Klausel wird mithilfe der folgenden Regeln in der angegebenen Reihenfolge in einen booleschen Wert konvertiert. Diese Regeln entsprechen den Regeln für Prädikate in path-Ausdrücken, nur sind keine ganzen Zahlen zulässig:  
  
1.  Wenn der `where`-Ausdruck eine leere Sequenz zurückgibt, ist der effektive boolesche Wert False.  
  
2.  Wenn der `where`-Ausdruck einen booleschen Wert vom simple-Typ zurückgibt, ist dieser Wert der effektive boolesche Wert.  
  
3.  Wenn der `where`-Ausdruck eine Sequenz zurückgibt, die mindestens einen Knoten enthält, ist der effektive boolesche Wert True.  
  
4.  Anderenfalls wird ein statischer Fehler ausgelöst.  
  
## <a name="multiple-variable-binding-in-flwor"></a>Binden mehrerer Variablen in FLWOR  
 Sie können einen einzelnen FLWOR-Ausdruck verwenden, um mehrere Variablen an Eingabesequenzen zu binden. Im folgenden Beispiel wird die Abfrage z. B. für eine nicht typisierte xml-Variable angegeben. Der FLOWR-Ausdruck gibt das erste <`Step`>-Element in jedem <`Location`> Element zurück.  
  
```sql
declare @x xml  
set @x='<ManuInstructions ProductModelID="1" ProductModelName="SomeBike" >  
<Location LocationID="L1" >  
  <Step>Manu step 1 at Loc 1</Step>  
  <Step>Manu step 2 at Loc 1</Step>  
  <Step>Manu step 3 at Loc 1</Step>  
</Location>  
<Location LocationID="L2" >  
  <Step>Manu step 1 at Loc 2</Step>  
  <Step>Manu step 2 at Loc 2</Step>  
  <Step>Manu step 3 at Loc 2</Step>  
</Location>  
</ManuInstructions>'  
SELECT @x.query('  
   for $Loc in /ManuInstructions/Location,  
       $FirstStep in $Loc/Step[1]  
   return   
       string($FirstStep)  
')  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Der `for` Ausdruck definiert `$Loc` -und $- `FirstStep` Variablen.  
  
-   Die `two`-Ausdrücke `/ManuInstructions/Location` und `$FirstStep in $Loc/Step[1]` sind insofern korreliert, als die Werte von `$FirstStep` von den Werten von `$Loc` abhängen.  
  
-   Der Ausdruck, der zugeordnet ist, `$Loc` generiert eine Sequenz von <`Location`> Elementen. Für jedes <`Location`>-Element wird `$FirstStep` eine Sequenz von einem <`Step`> Element, einem Singleton, generiert.  
  
-   `$Loc` wird in dem Ausdruck angegeben, der der `$FirstStep`-Variablen zugeordnet ist.  
  
 Dies ist das Ergebnis:  
  
```  
Manu step 1 at Loc 1   
Manu step 1 at Loc 2  
```  
  
 Die folgende Abfrage ist ähnlich, mit der Ausnahme, dass Sie für die Instructions-Spalte, typisierte **XML** -Spalte, der **ProductModel** -Tabelle angegeben wird. [XML-Konstruktion (XQuery)](../xquery/xml-construction-xquery.md) wird verwendet, um den gewünschten XML-Code zu generieren.  
  
```sql
SELECT Instructions.query('  
     declare default element namespace "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $WC in /root/Location,  
            $S  in $WC/step  
      return  
          <Step LocationID= "{$WC/@LocationID }" >  
            { $S/node() }  
          </Step>  
') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Beachten Sie in der vorhergehenden Abfrage Folgendes:  
  
-   Die `for`-Klausel definiert zwei Variablen, `$WC` und `$S`. Der Ausdruck, der `$WC` zugeordnet ist, generiert eine Sequenz von Arbeitsplatzstandorten bei der Fertigung eines Fahrrades eines Fahrradproduktmodells. Der path-Ausdruck, der der `$S`-Variablen zugeordnet ist, generiert eine Sequenz von Schritten für jede Arbeitsplatzstandort-Sequenz in `$WC`.  
  
-   Die Return-Anweisung konstruiert XML mit einem <`Step`>-Element, das den Fertigungsschritt und die **LocationID** als Attribut enthält.  
  
-   Der **Declare Default Element-Namespace** wird im XQuery-Prolog verwendet, sodass alle Namespace Deklarationen in der resultierenden XML-Datei im Element der obersten Ebene angezeigt werden. Auf diese Weise ist das Ergebnis besser lesbar. Weitere Informationen zu Standardnamespaces finden Sie unter [Handling Namespaces in XQuery](../xquery/handling-namespaces-in-xquery.md).  
  
 Dies ist das Teilergebnis:  
  
```xml
<Step xmlns=  
    "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"     
  LocationID="10">  
     Insert <material>aluminum sheet MS-2341</material> into the <tool>T-   
     85A framing tool</tool>.   
</Step>  
...  
<Step xmlns=  
      "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"     
    LocationID="20">  
        Assemble all frame components following blueprint   
        <blueprint>1299</blueprint>.  
</Step>  
...  
```  
  
## <a name="using-the-order-by-clause"></a>Verwenden der order by-Klausel  
 Die Sortierung in XQuery wird mithilfe der `order by`-Klausel im FLWOR-Ausdruck ausgeführt. Die an die-Klausel über gebenden Sortier Ausdrücke `order by` müssen Werte zurückgeben, deren Typen für den **gt** -Operator gültig sind. Jeder Sortierausdruck muss zu einer Singletonsequenz mit einem Element führen. Standardmäßig wird die Sortierung in aufsteigender Reihenfolge durchgeführt. Sie können optional die auf- oder absteigende Reihenfolge für jeden Sortierausdruck angeben.  
  
> [!NOTE]  
>  Sortiervergleiche für Zeichenfolgenwerte, die von der XQuery-Implementierung in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ausgeführt werden, werden immer mithilfe der binären Unicode-Codepunktsortierung ausgeführt.  
  
 Die folgende Abfrage ruft alle Rufnummern für einen bestimmten Kunden aus der AdditionalContactInfo-Spalte ab. Die Ergebnisse werden nach Rufnummer sortiert.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT AdditionalContactInfo.query('  
   declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
   declare namespace aci="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
   for $a in /aci:AdditionalContactInfo//act:telephoneNumber   
   order by $a/act:number[1] descending  
   return $a  
') As Result  
FROM Person.Person  
WHERE BusinessEntityID=291;  
```  
  
 Beachten Sie, dass der [atomisierungsprozess (XQuery)](../xquery/atomization-xquery.md) den atomaren Wert der <`number`> Elemente abruft, bevor er an übergeben wird `order by` . Sie können den Ausdruck mit der **Data ()** -Funktion schreiben, dies ist jedoch nicht erforderlich.  
  
```  
order by data($a/act:number[1]) descending  
```  
  
 Dies ist das Ergebnis:  
  
```xml
<act:telephoneNumber xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>333-333-3334</act:number>  
</act:telephoneNumber>  
<act:telephoneNumber xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  <act:number>333-333-3333</act:number>  
</act:telephoneNumber>  
```  
  
 Statt die Namespaces im Abfrageprolog zu deklarieren, können Sie diese auch mithilfe von WITH XMLNAMESPACES deklarieren.  
  
```sql
WITH XMLNAMESPACES (  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes' AS act,  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo'  AS aci)  
  
SELECT AdditionalContactInfo.query('  
   for $a in /aci:AdditionalContactInfo//act:telephoneNumber   
   order by $a/act:number[1] descending  
   return $a  
') As Result  
FROM Person.Person  
WHERE BusinessEntityID=291;  
```  
  
 Sie können auch nach Attributwert sortieren. Beispielsweise ruft die folgende Abfrage die neu erstellten <`Location`> Elemente ab, deren LocationID-und LaborHours-Attribute nach dem LaborHours-Attribut in absteigender Reihenfolge sortiert sind. Als Ergebnis dieses Vorgangs werden die Arbeitsplatzstandorte, die die größte Anzahl von Arbeitsstunden aufweisen, zuerst zurückgegeben.  
  
```sql
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $WC in /AWMI:root/AWMI:Location   
order by $WC/@LaborHours descending  
        return  
          <Location>  
             { $WC/@LocationID }   
             { $WC/@LaborHours }   
          </Location>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7;  
```  
  
 Dies ist das Ergebnis:  
  
```  
<Location LocationID="60" LaborHours="4"/>  
<Location LocationID="50" LaborHours="3"/>  
<Location LocationID="10" LaborHours="2.5"/>  
<Location LocationID="20" LaborHours="1.75"/>  
<Location LocationID="30" LaborHours="1"/>  
<Location LocationID="45" LaborHours=".5"/>  
```  
  
 In der folgenden Abfrage werden die Ergebnisse nach dem Elementnamen sortiert. Die Abfrage ruft die Spezifikationen eines bestimmten Produkts aus dem Produktkatalog ab. Die Spezifikationen sind die untergeordneten Elemente des <`Specifications`>-Elements.  
  
```sql
SELECT CatalogDescription.query('  
     declare namespace  
 pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      for $a in /pd:ProductDescription/pd:Specifications/*   
     order by local-name($a)  
      return $a  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19;  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Der `/p1:ProductDescription/p1:Specifications/*` Ausdruck gibt die untergeordneten Elemente von <`Specifications`> zurück.  
  
-   Der `order by (local-name($a))`-Ausdruck sortiert die Sequenz nach dem lokalen Namensanteil des Elementnamens.  
  
 Dies ist das Ergebnis:  
  
```xml
<Color>Available in most colors</Color>  
<Material>Almuminum Alloy</Material>  
<ProductLine>Mountain bike</ProductLine>  
<RiderExperience>Advanced to Professional riders</RiderExperience>  
<Style>Unisex</Style>    
```  
  
 Knoten, in denen der Sortierausdruck leer zurückgegeben wird, werden an den Beginn der Sequenz sortiert, wie das folgende Beispiel zeigt:  
  
```sql
declare @x xml  
set @x='<root>  
  <Person Name="A" />  
  <Person />  
  <Person Name="B" />  
</root>  
'  
select @x.query('  
  for $person in //Person  
  order by $person/@Name  
  return   $person  
')  
```  
  
 Dies ist das Ergebnis:  
  
```xml
<Person />  
<Person Name="A" />  
<Person Name="B" />  
```  
  
 Sie können mehrere Sortierkriterien angeben, wie im folgenden Beispiel gezeigt wird. Die Abfrage in diesem Beispiel sortiert <`Employee`> Elemente zuerst nach Titel und dann nach Administrator Attributwerten.  
  
```sql
declare @x xml  
set @x='<root>  
  <Employee ID="10" Title="Teacher"        Gender="M" />  
  <Employee ID="15" Title="Teacher"  Gender="F" />  
  <Employee ID="5" Title="Teacher"         Gender="M" />  
  <Employee ID="11" Title="Teacher"        Gender="F" />  
  <Employee ID="8" Title="Administrator"   Gender="M" />  
  <Employee ID="4" Title="Administrator"   Gender="F" />  
  <Employee ID="3" Title="Teacher"         Gender="F" />  
  <Employee ID="125" Title="Administrator" Gender="F" /></root>'  
SELECT @x.query('for $e in /root/Employee  
order by $e/@Title ascending, $e/@Gender descending  
  
  return  
     $e  
')  
```  
  
 Dies ist das Ergebnis:  
  
```xml
<Employee ID="8" Title="Administrator" Gender="M" />  
<Employee ID="4" Title="Administrator" Gender="F" />  
<Employee ID="125" Title="Administrator" Gender="F" />  
<Employee ID="10" Title="Teacher" Gender="M" />  
<Employee ID="5" Title="Teacher" Gender="M" />  
<Employee ID="11" Title="Teacher" Gender="F" />  
<Employee ID="15" Title="Teacher" Gender="F" />  
<Employee ID="3" Title="Teacher" Gender="F" />  
```  
  
### <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Die Sortierausdrücke müssen homogen typisiert sein. Dies wird statisch geprüft.  
  
-   Das Sortieren leerer Sequenzen kann nicht gesteuert werden.  
  
-   Die empty least-, empty greatest- und collation-Schlüsselwörter für `order by` werden nicht unterstützt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Ausdrücke](../xquery/xquery-expressions.md)  
  
  
