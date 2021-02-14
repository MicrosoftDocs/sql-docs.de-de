---
title: Angeben der Achse in einem Pfad Ausdrucks Schritt | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen Achsen Schritt in einem XQuery-Pfad Ausdruck angeben.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- attribute axis [SQL Server]
- axis step [XQuery]
- descendant axis
- self axis
- path expressions [XQuery]
- child axis
- descendant-or-self axis
- parent axis
ms.assetid: c44fb843-0626-4496-bde0-52ca0bac0a9e
author: rothja
ms.author: jroth
ms.openlocfilehash: 2207dcce2787fb6f51668e777b01ce345d1f33ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344662"
---
# <a name="path-expressions---specifying-axis"></a>Pfadausdrücke – Angeben der Achse
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Ein Achsenschritt in einem Pfadausdruck besteht aus den folgenden Komponenten:  
  
-   Einer Achse  
  
-   Ein [Knoten Test](../xquery/path-expressions-specifying-node-test.md)  
  
-   [Null oder mehr Schrittqualifizierern (optional)](../xquery/path-expressions-specifying-predicates.md)  
  
 Weitere Informationen finden Sie unter [Path Expressions &#40;XQuery&#41;](../xquery/path-expressions-xquery.md).  
  
 Die XQuery-Implementierung in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützt folgende Achsenschritte:  
  
|Achse|Beschreibung|  
|----------|-----------------|  
|**Idee**|Gibt die untergeordneten Elemente des Kontextknotens zurück.|  
|**descendant**|Gibt alle nachfolgenden Elemente des Kontextknotens zurück.|  
|**parent**|Gibt das übergeordnete Element des Kontextknotens zurück.|  
|**attribute**|Gibt die Attribute des Kontextknotens zurück.|  
|**Selbstbedienungs**|Gibt den Kontextknoten selbst zurück.|  
|**descendant-or-self**|Gibt den Kontextknoten und alle nachfolgenden Elemente des Kontextknotens zurück.|  
  
 Alle diese Achsen, außer die über **geordnete** Achse, sind vorwärts Achsen. Die über **geordnete** Achse ist eine umgekehrte Achse, da Sie rückwärts in der Dokument Hierarchie durchsucht wird. Beispiel: Der relative Pfadausdruck `child::ProductDescription/child::Summary` enthält zwei Schritte, von denen jeder eine `child`-Achse angibt. Der erste Schritt ruft die untergeordneten- \<ProductDescription> Elemente des Kontext Knotens ab. \<ProductDescription>Der zweite Schritt ruft für jeden Elementknoten die untergeordneten \<Summary> Elemente des Elements ab.  
  
 Der relative Pfadausdruck `child::root/child::Location/attribute::LocationID` besitzt drei Schritte. Die ersten beiden Schritte geben jeweils eine `child`-Achse und der dritte die `attribute`-Achse an. Bei der Ausführung für die XML-Dokumente mit den Fertigungsanweisungen in der **Production. ProductModel** -Tabelle gibt der Ausdruck das- `LocationID` Attribut des untergeordneten \<Location> Element Knotens des- \<root> Elements zurück.  
  
## <a name="examples"></a>Beispiele  
 Die Abfrage Beispiele in diesem Thema werden für Spalten vom Typ **XML** in der **AdventureWorks** -Datenbank angegeben.  
  
### <a name="a-specifying-a-child-axis"></a>A. Angeben einer child-Achse  
 Bei einem bestimmten Produktmodell Ruft die folgende Abfrage die untergeordneten \<Features> Elementknoten des- \<ProductDescription> Element Knotens aus der in der-Tabelle gespeicherten Produktkatalog Beschreibung ab `Production.ProductModel` .  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  /child::PD:ProductDescription/child::PD:Features')  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Beachten Sie hinsichtlich der vorherigen Abfrage Folgendes:  
  
-   Die- `query()` Methode des **XML** -Datentyps gibt den Pfad Ausdruck an.  
  
-   Beide Schritte des Pfadausdrucks geben eine `child`-Achse und die Knotennamen `ProductDescription` und `Features` als Knotentests an. Weitere Informationen zu Knoten Tests finden Sie unter [Angeben eines Knoten Tests in einem Pfad Ausdrucks Schritt](../xquery/path-expressions-specifying-node-test.md).  
  
### <a name="b-specifying-descendant-and-descendant-or-self-axes"></a>B. Angeben von descendant- und descendant-or-self-Achsen  
 In diesem Beispiel werden descendant- und descendant-or-self-Achsen verwendet. Die Abfrage in diesem Beispiel wird für eine Variable vom Typ **XML** angegeben. Die XML-Instanz wird hier zur Veranschaulichung der Unterschiede in den generierten Ergebnissen vereinfacht dargestellt.  
  
```  
declare @x xml  
set @x='  
<a>  
 <b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
 </b>  
</a>'  
declare @y xml  
set @y = @x.query('  
  /child::a/child::b  
')  
select @y  
```  
  
 Im folgenden Ergebnis gibt der Ausdruck einen untergeordneten Elementknoten `<b>` eines Elementknotens `<a>` zurück.  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
```  
  
 Wenn Sie in diesem Ausdruck eine descendant-Achse für den Pfadausdruck angeben,  
  
 `/child::a/child::b/descendant::*`, Sie Fragen nach allen nachfolgenden `b` Elementen des Knotens "<> Element".  
  
 Das Sternchen (*) im Knotentest stellt den Knotennamen als Knotentest dar. Folglich bestimmt der Typ des Primärknotens der descendant-Achse (der Elementknoten) den Typ der zurückgegebenen Knoten, d. h. in diesem Fall, dass der Ausdruck alle Elementknoten zurückgibt. Es werden keine Textknoten zurückgegeben. Weitere Informationen über den primären Knotentyp und seine Beziehung mit dem Knoten Test finden Sie unter [Angeben eines Knoten Tests in einem Schritt eines Pfad Ausdrucks](../xquery/path-expressions-specifying-node-test.md) .  
  
 Die Elementknoten <`c`> und <`d`> werden zurückgegeben, wie im folgenden Ergebnis gezeigt:  
  
```  
<c>text2  
     <d>text3</d>  
</c>  
<d>text3</d>  
```  
  
 Wenn Sie anstelle der Nachfolger Achse eine nach-oben-oder-Selbstachse angeben, `/child::a/child::b/descendant-or-self::*` gibt den Kontext Knoten, das Element <`b`> und dessen Nachfolger zurück.  
  
 Dies ist das Ergebnis:  
  
```  
<b>text1  
   <c>text2  
     <d>text3</d>  
   </c>  
</b>  
  
<c>text2  
     <d>text3</d>  
</c>  
  
<d>text3</d>   
```  
  
 In der folgenden Beispiel Abfrage für die **AdventureWorks** -Datenbank werden alle Nachfolger Elementknoten des <>-Element abgerufen, `Features` das dem <> Element untergeordnet ist `ProductDescription` :  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  /child::PD:ProductDescription/child::PD:Features/descendant::*  
')  
FROM  Production.ProductModel  
WHERE ProductModelID=19  
```  
  
### <a name="c-specifying-a-parent-axis"></a>C. Angeben einer übergeordneten Achse  
 Die folgende Abfrage gibt die <>-Element zurück, das `Summary` dem <`ProductDescription`> Element im in der-Tabelle gespeicherten Produktkatalog-XML-Dokument untergeordnet ist `Production.ProductModel` .  
  
 In diesem Beispiel wird die übergeordnete Achse verwendet, um zum übergeordneten Element des <`Feature`>-Elements zurückzukehren und die <>-Element abzurufen, das `Summary` dem <> Element untergeordnet ist `ProductDescription` .  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
  
/child::PD:ProductDescription/child::PD:Features/parent::PD:ProductDescription/child::PD:Summary  
')  
FROM   Production.ProductModel  
WHERE  ProductModelID=19  
  
```  
  
 In dieser Beispielabfrage verwendet der Pfadausdruck die `parent`-Achse. Sie können den Ausdruck auch ohne die parent-Achse schreiben, wie im Folgenden gezeigt:  
  
```  
/child::PD:ProductDescription[child::PD:Features]/child::PD:Summary  
```  
  
 Ein nützlicheres Beispiel für die parent-Achse wird im folgenden Beispiel bereitgestellt.  
  
 Jede Produktmodell-Katalogbeschreibung, die in der **CatalogDescription** -Spalte der **ProductModel** -Tabelle gespeichert ist, verfügt über ein `<ProductDescription>` -Element mit dem `ProductModelID` -Attribut und dem untergeordneten- `<Features>` Element, wie im folgenden Fragment dargestellt:  
  
```  
<ProductDescription ProductModelID="..." >  
  ...  
  <Features>  
    <Feature1>...</Feature1>  
    <Feature2>...</Feature2>  
   ...  
</ProductDescription>  
```  
  
 Die Abfrage legt in der FLWOR-Anweisung eine Iteratorvariable, `$f`, fest, damit die untergeordneten Elemente des `<Features>`-Element zurückgegeben werden. Weitere Informationen finden Sie unter [FLWOR-Anweisung und-Iterations &#40;XQuery&#41;](../xquery/flwor-statement-and-iteration-xquery.md). Für jedes Funktions-Element konstruiert die `return`-Klausel ein XML-Dokument in der folgenden Form:  
  
```  
<Feature ProductModelID="...">...</Feature>  
<Feature ProductModelID="...">...</Feature>  
```  
  
 Um die `ProductModelID` für jedes `<Feature`> Element hinzuzufügen, `parent` wird die Achse angegeben:  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
  for $f in /child::PD:ProductDescription/child::PD:Features/child::*  
  return  
   <Feature  
     ProductModelID="{ ($f/parent::PD:Features/parent::PD:ProductDescription/attribute::ProductModelID)[1]}" >  
          { $f }  
   </Feature>  
')  
FROM  Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Dies ist das Teilergebnis:  
  
```  
<Feature ProductModelID="19">  
  <wm:Warranty   
   xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
    <wm:Description>parts and labor</wm:Description>  
  </wm:Warranty>  
</Feature>  
<Feature ProductModelID="19">  
  <wm:Maintenance   
   xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    <wm:NoOfYears>10 years</wm:NoOfYears>  
    <wm:Description>maintenance contract available through your dealer   
                  or any AdventureWorks retail store.</wm:Description>  
  </wm:Maintenance>  
</Feature>  
<Feature ProductModelID="19">  
  <p1:wheel   
   xmlns:p1="https://www.adventure-works.com/schemas/OtherFeatures">  
      High performance wheels.  
  </p1:wheel>  
</Feature>  
```  
  
 Beachten Sie, dass dem Pfadausdruck das Prädikat `[1]` hinzugefügt wird, um sicherzustellen, dass ein Singleton-Wert zurückgegeben wird.  
  
  
