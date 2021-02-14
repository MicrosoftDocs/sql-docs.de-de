---
title: XML-Konstruktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie XML-Strukturen in einer XQuery mithilfe der direkten und berechneten Konstruktoren erstellen.
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
- white space [XQuery]
- computed constructor
- construct XML structures [XQuery]
- constructors [XQuery]
- prolog
- direct constructor [SQL Server]
- XML [SQL Server], construction
- XQuery, XML construction
ms.assetid: a6330b74-4e52-42a4-91ca-3f440b3223cf
author: rothja
ms.author: jroth
ms.openlocfilehash: 1c171dab70a909edb197f5a498d2207bcb2e555b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352327"
---
# <a name="xml-construction-xquery"></a>XML-Konstruktion (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  In XQuery können Sie die **direkten** und **berechneten** Konstruktoren verwenden, um XML-Strukturen innerhalb einer Abfrage zu erstellen.  
  
> [!NOTE]  
>  Es gibt keinen Unterschied zwischen dem **direkten** und dem **berechneten** Konstruktoren.  
  
## <a name="using-direct-constructors"></a>Verwenden von direkten Konstruktoren  
 Wenn Sie direkte Konstruktoren verwenden, geben Sie beim Konstruieren des XML-Codes eine XML-ähnliche Syntax an. Die folgenden Beispiele veranschaulichen die XML-Konstruktion mit direkten Konstruktoren.  
  
### <a name="constructing-elements"></a>Konstruktion von Elementen  
 Sie können Elemente konstruieren, indem Sie die XML-Schreibweise verwenden. Im folgenden Beispiel wird der direkte elementkonstruktorausdruck verwendet, und es wird ein- \<ProductModel> Element erstellt. Das konstruierte Element besitzt drei untergeordnete Elemente.  
  
-   Einen Textknoten.  
  
-   Zwei Elementknoten, \<Summary> und \<Features> .  
  
    -   Das- \<Summary> Element verfügt über ein untergeordnetes Textknoten Element, dessen Wert "Some Description" ist.  
  
    -   Das \<Features> -Element verfügt über die drei untergeordneten Knoten ' Element ',, \<Color> \<Weight> und \<Warranty> . Jeder dieser Knoten besitzt einen weiteren untergeordneten Textknoten mit den Werten Red, 25 und 2 years parts and labor.  
  
```sql
declare @x xml;  
set @x='';  
select @x.query('<ProductModel ProductModelID="111">;  
This is product model catalog description.  
<Summary>Some description</Summary>  
<Features>  
  <Color>Red</Color>  
  <Weight>25</Weight>  
  <Warranty>2 years parts and labor</Warranty>  
</Features></ProductModel>')  
  
```  
  
 Dies ist das XML-Ergebnis:  
  
```xml
<ProductModel ProductModelID="111">  
  This is product model catalog description.  
  <Summary>Some description</Summary>  
  <Features>  
    <Color>Red</Color>  
    <Weight>25</Weight>  
    <Warranty>2 years parts and labor</Warranty>  
  </Features>  
</ProductModel>  
```  
  
 Wie in diesem Beispiel gezeigt, erweist sich das Konstruieren von Elementen aus konstanten Ausdrücken zwar als nützlich; die wahre Stärke dieser Funktion der XQuery-Sprache liegt jedoch in der Möglichkeit, XML-Code zu konstruieren, mit dem Daten dynamisch aus einer Datenbank extrahiert werden können. Verwenden Sie zum Angeben von Abfrageausdrücken geschweifte Klammern. Im XML-Ergebnis wird der Ausdruck dann durch seinen Wert ersetzt. Mit der folgenden Abfrage wird beispielsweise ein <`NewRoot`>-Element mit einem untergeordneten-Element (<`e`>) erstellt. Der Wert der Element <`e`> wird berechnet, indem ein Pfad Ausdruck in geschweiften Klammern ("{...}") angegeben wird.  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
SELECT @x.query('<NewRoot><e> { /root } </e></NewRoot>');  
```  
  
 Die geschweiften Klammern fungieren als Token, die einen Kontextwechsel bewirken, sodass die Abfrage von der XML-Konstruktion zur Abfrageauswertung wechselt. In diesem Fall wird der XQuery-Pfadausdruck innerhalb der geschweiften Klammern (`/root`) ausgewertet und durch sein Ergebnis ersetzt.  
  
 Dies ist das Ergebnis:  
  
```xml
<NewRoot>  
  <e>  
    <root>5</root>  
  </e>  
</NewRoot>  
```  
  
 Die folgende Abfrage ist der vorherigen ähnlich, Der Ausdruck in den geschweiften Klammern gibt jedoch die **Data ()** -Funktion an, um den atomaren Wert des <`root`>-Elements abzurufen und dem konstruierten Element <> zuweist `e` .  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
DECLARE @y xml;  
SET @y = (SELECT @x.query('  
                           <NewRoot>  
                             <e> { data(/root) } </e>  
                           </NewRoot>' ));  
SELECT @y;  
```  
  
 Dies ist das Ergebnis:  
  
```xml
<NewRoot>  
  <e>5</e>  
</NewRoot>  
```  
  
 Wenn die geschweiften Klammern Teil Ihres Texts sein sollen, d. h. nicht als Kontextwechseltoken fungieren sollen, können Sie als Escapezeichen "}}" und "{{" verwenden, wie im folgenden Beispiel gezeigt:  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
DECLARE @y xml;  
SET @y = (SELECT @x.query('  
<NewRoot> Hello, I can use {{ and  }} as part of my text</NewRoot>'));  
SELECT @y;  
```  
  
 Dies ist das Ergebnis:  
  
```xml
<NewRoot> Hello, I can use { and  } as part of my text</NewRoot>  
```  
  
 Die folgende Abfrage ist ein weiteres Beispiel für die Konstruktion von Elementen mithilfe des direkten Elementkonstruktors. Außerdem wird der Wert des <`FirstLocation`>-Elements durch Ausführen des Ausdrucks in den geschweiften Klammern abgerufen. Der Abfrageausdruck gibt die Fertigungsschritte am ersten Arbeitsplatzstandort aus der Instructions-Spalte der Production.ProductModel-Tabelle zurück.  
  
```sql
SELECT Instructions.query('  
    declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        <FirstLocation>  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM Production.ProductModel  
WHERE ProductModelID=7;  
```  
  
 Dies ist das Ergebnis:  
  
```xml
<FirstLocation>  
  <AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">  
      Insert <AWMI:material>aluminum sheet MS-2341</AWMI:material> into the <AWMI:tool>T-85A framing tool</AWMI:tool>.   
  </AWMI:step>  
  <AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">  
      Attach <AWMI:tool>Trim Jig TJ-26</AWMI:tool> to the upper and lower right corners of the aluminum sheet.   
  </AWMI:step>  
   ...  
</FirstLocation>  
```  
  
#### <a name="element-content-in-xml-construction"></a>Elementinhalt in der XML-Konstruktion  
 Das folgende Beispiel veranschaulicht das Verhalten von Ausdrücken beim Konstruieren von Elementinhalten mithilfe des direkten Konstruktors. Im folgenden Beispiel gibt der direkte Elementkonstruktor einen Ausdruck an. Für diesen Ausdruck wird im daraus resultierenden XML-Code ein Textknoten erstellt.  
  
```sql
declare @x xml;  
set @x='  
<root>  
  <step>This is step 1</step>  
  <step>This is step 2</step>  
  <step>This is step 3</step>  
</root>';  
select @x.query('  
<result>  
 { for $i in /root[1]/step  
    return string($i)  
 }  
</result>');  
  
```  
  
 Die aus der Auswertung des Ausdrucks resultierende Sequenz atomarer Werte wird dem Textknoten hinzugefügt, wobei die einzelnen atomaren Werte durch Leerzeichen getrennt werden, wie im Ergebnis gezeigt. Das konstruierte Element besitzt ein untergeordnetes Element. Es handelt sich um einen Textknoten, der den im Ergebnis gezeigten Wert enthält.  
  
```xml
<result>This is step 1 This is step 2 This is step 3</result>  
```  
  
 Wenn Sie anstelle eines einzigen Ausdrucks drei verschiedene Ausdrücke angeben, die drei Textknoten generieren, werden die aneinander angrenzenden Textknoten im XML-Ergebnis durch Verkettung zu einem einzigen zusammengeführt.  
  
```sql
declare @x xml;  
set @x='  
<root>  
  <step>This is step 1</step>  
  <step>This is step 2</step>  
  <step>This is step 3</step>  
</root>';  
select @x.query('  
<result>  
 { string(/root[1]/step[1]) }  
 { string(/root[1]/step[2]) }  
 { string(/root[1]/step[3]) }  
</result>');  
```  
  
 Der konstruierte Elementknoten besitzt ein untergeordnetes Element. Es handelt sich um einen Textknoten, der den im Ergebnis gezeigten Wert enthält.  
  
```xml
<result>This is step 1This is step 2This is step 3</result>  
```  
  
### <a name="constructing-attributes"></a>Konstruktion von Attributen  
 Beim Konstruieren von Elementen mithilfe des direkten Elementkonstruktors können Sie über eine XML-ähnliche Syntax Elementattribute angeben, wie im folgenden Beispiel gezeigt:  
  
```sql
declare @x xml;  
set @x='';  
select @x.query('<ProductModel ProductModelID="111">;  
This is product model catalog description.  
<Summary>Some description</Summary>  
</ProductModel>')  
```  
  
 Dies ist das XML-Ergebnis:  
  
```xml
<ProductModel ProductModelID="111">  
  This is product model catalog description.  
  <Summary>Some description</Summary>  
</ProductModel>  
```  
  
 Das erstellte Element <`ProductModel`> das ProductModelID-Attribut und diese untergeordneten Knoten enthält:  
  
-   Einen Textknoten, `This is product model catalog description.`.  
  
-   Ein Elementknoten, <`Summary`>. Dieser Knoten besitzt einen untergeordneten Textknoten, `Some description`.  
  
 Beim Konstruieren eines Attributs können Sie dessen Wert durch einen Ausdruck in geschweiften Klammern angeben. In diesem Fall wird das Ergebnis des Ausdrucks als der Attributwert zurückgegeben.  
  
 Im folgenden Beispiel ist die **Data ()** -Funktion nicht unbedingt erforderlich. Da Sie den Ausdruckswert einem Attribut zuweisen, wird **Data ()** implizit angewendet, um den typisierten Wert des angegebenen Ausdrucks abzurufen.  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
DECLARE @y xml;  
SET @y = (SELECT @x.query('<NewRoot attr="{ data(/root) }" ></NewRoot>'));  
SELECT @y;  
```  
  
 Dies ist das Ergebnis:  
  
```xml
<NewRoot attr="5" />  
```  
  
 Es folgt eine weiteres Beispiel, in dem Ausdrücke für die Konstruktion der LocationID- und SetupHrs-Attribute angegeben werden. Diese Ausdrücke werden für den XML-Code in der Instructions-Spalte ausgewertet. Der typisierte Wert des Ausdrucks wird anschließend den Attributen zugewiesen.  
  
```sql
SELECT Instructions.query('  
    declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        <FirstLocation   
         LocationID="{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
         SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7;  
```  
  
 Dies ist das Teilergebnis:  
  
```xml
<FirstLocation LocationID="10" SetupHours="0.5" >  
  <AWMI:step ...   
  </AWMI:step>  
  ...  
</FirstLocation>  
```  
  
#### <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Mehrere oder gemischte Attributausdrücke (d. h. Zeichenfolgen- und XQuery-Ausdrücke) werden nicht unterstützt. Wie in der folgenden Abfrage gezeigt, konstruieren Sie beispielsweise XML-Code, in dem `Item` eine Konstante ist, und der Wert `5` durch Auswerten eines Abfrageausdrucks erhalten wird:  
  
    ```xml
    <a attr="Item 5" />  
    ```  
  
     Die folgende Abfrage gibt einen Fehler zurück, da Sie eine Zeichenfolgenkonstante mit einem Ausdruck ({/x}) verwendet haben und dies nicht unterstützt wird:  
  
    ```sql
    DECLARE @x xml  
    SET @x ='<x>5</x>'  
    SELECT @x.query( '<a attr="Item {/x}"/>' )   
    ```  
  
     In diesem Fall haben Sie folgende Möglichkeiten:  
  
    -   Sie können den Attributwert durch Verkettung zweier atomarer Werte bilden. Diese atomaren Werte werden für den Attributwert serialisiert, wobei zwischen die beiden Werte ein Leerzeichen eingefügt wird:  
  
        ```sql
        SELECT @x.query( '<a attr="{''Item'', data(/x)}"/>' )   
        ```  
  
         Dies ist das Ergebnis:  
  
        ```xml
        <a attr="Item 5" />  
        ```  
  
    -   Verwenden Sie die [Concat-Funktion](../xquery/functions-on-string-values-concat.md) , um die zwei Zeichen folgen Argumente in den resultierenden Attribut Wert zu verketten:  
  
        ```sql
        SELECT @x.query( '<a attr="{concat(''Item'', /x[1])}"/>' )   
        ```  
  
         In diesem Fall wird zwischen den beiden Zeichenfolgenwerten kein Leerzeichen eingefügt. Wenn Sie die beiden Werte doch durch ein Leerzeichen trennen möchten, müssen Sie dies explizit angeben.  
  
         Dies ist das Ergebnis:  
  
        ```xml
        <a attr="Item5" />  
        ```  
  
-   Mehrere Ausdrücke werden als Attributwert nicht unterstützt. So gibt beispielsweise folgende Abfrage einen Fehler zurück:  
  
    ```sql
    DECLARE @x xml  
    SET @x ='<x>5</x>'  
    SELECT @x.query( '<a attr="{/x}{/x}"/>' )  
    ```  
  
-   Heterogene Sequenzen werden nicht unterstützt. Der Versuch, eine heterogene Sequenz als Attributwert zuzuweisen, gibt einen Fehler zurück, wie im folgenden Beispiel gezeigt. In diesem Beispiel wird eine heterogene Sequenz, eine Zeichenfolge "Item" und ein Element <`x`>, als Attribut Wert angegeben:  
  
    ```sql
    DECLARE @x xml  
    SET @x ='<x>5</x>'  
    select @x.query( '<a attr="{''Item'', /x }" />')  
    ```  
  
     Wenn Sie die **Data ()** -Funktion anwenden, funktioniert die Abfrage, da Sie den atomaren Wert des Ausdrucks, `/x` , der mit der Zeichenfolge verkettet wird, abruft. Es folgt eine Sequenz atomarer Werte:  
  
    ```sql
    SELECT @x.query( '<a attr="{''Item'', data(/x)}"/>' )   
    ```  
  
     Dies ist das Ergebnis:  
  
    ```xml
    <a attr="Item 5" />  
    ```  
  
-   Die Reihenfolge der Attributknoten wird während der Serialisierung erzwungen und nicht während der Überprüfung des statischen Typs. Beispielsweise ist die folgende Abfrage fehlerhaft, weil sie versucht, nach einem Nichtattributknoten einen Attributknoten hinzuzufügen.  
  
    ```sql
    select convert(xml, '').query('  
    element x { attribute att { "pass" }, element y { "Element text" }, attribute att2 { "fail" } }  
    ')  
    go  
    ```  
  
     Die obige Abfrage gibt den folgenden Fehler zurück:  
  
    ```  
    XML well-formedness check: Attribute cannot appear outside of element declaration. Rewrite your XQuery so it returns well-formed XML.  
    ```  
  
### <a name="adding-namespaces"></a>Hinzufügen von Namespaces  
 Beim Konstruieren von XML-Code mithilfe von direkten Konstruktoren können die konstruierten Element- und Attributnamen durch das Verwenden von Namespacepräfixen qualifiziert werden. Es gibt folgende Möglichkeiten, um Präfixe an Namespaces zu binden:  
  
-   Mithilfe eines Namespacedeklarationsattributs.  
  
-   Mithilfe der WITH XMLNAMESPACES-Klausel.  
  
-   Im XQuery-Prolog.  
  
#### <a name="using-a-namespace-declaration-attribute-to-add-namespaces"></a>Verwenden eines Namespacedeklarationsattributs zum Hinzufügen von Namespaces  
 Im folgenden Beispiel wird ein Namespace Deklarations Attribut bei der Erstellung des-Elements <`a`> verwendet, um einen Standard Namespace zu deklarieren. Die Erstellung des untergeordneten-Elements <`b`> die Deklaration des Standard Namespace, der im übergeordneten Element deklariert ist, nicht mehr durchführt.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
  <a xmlns="a">  
    <b xmlns=""/>  
  </a>' )   
```  
  
 Dies ist das Ergebnis:  
  
```xml
<a xmlns="a">  
  <b xmlns="" />  
</a>  
```  
  
 Sie können dem Namespace ein Präfix zuweisen. Das-Präfix wird bei der Erstellung des-Elements <`a`> angegeben.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
  <x:a xmlns:x="a">  
    <b/>  
  </x:a>' )  
```  
  
 Dies ist das Ergebnis:  
  
```xml
<x:a xmlns:x="a">  
  <b />  
</x:a>  
```  
  
 Sie können die Deklaration eines Standardnamespace in der XML-Konstruktion rückgängig machen, nicht jedoch die Deklaration eines Namespacepräfixes. Die folgende Abfrage gibt einen Fehler zurück, da Sie das Deklarieren eines Präfixes nicht durchführen können, wie in der Erstellung des Elements <`b`> angegeben.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
  <x:a xmlns:x="a">  
    <b xmlns:x=""/>  
  </x:a>' )  
```  
  
 Der neu konstruierte Namespace steht zum Verwenden innerhalb der Abfrage zur Verfügung. Die folgende Abfrage deklariert z. b. einen Namespace zum Erstellen des-Elements, <`FirstLocation`> und gibt das Präfix in den Ausdrücken für die LocationID-und SetupHrs-Attributwerte an.  
  
```sql
SELECT Instructions.query('  
        <FirstLocation xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"  
         LocationID="{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
         SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7  
```  
  
 Beachten Sie, dass das Erstellen eines neuen Namespacepräfixes in dieser Weise alle eventuell bereits vorhandenen Namespacedeklarationen für dieses Präfix überschreibt. Beispielsweise wird die-Namespace Deklaration `AWMI="https://someURI"` im Abfrage Prolog von der Namespace Deklaration im <>-Element überschrieben `FirstLocation` .  
  
```sql
SELECT Instructions.query('  
declare namespace AWMI="https://someURI";  
        <FirstLocation xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"  
         LocationID="{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
         SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7  
```  
  
#### <a name="using-a-prolog-to-add-namespaces"></a>Verwenden eines Prologs zum Hinzufügen von Namespaces  
 Dieses Beispiel veranschaulicht, wie dem konstruierten XML-Code Namespaces hinzugefügt werden können. Es wird ein Standardnamespace im Prolog der Abfrage hinzugefügt.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
           declare default element namespace "a";  
            <a><b xmlns=""/></a>' )  
```  
  
 Beachten Sie, dass `b` das Namespace Deklarations Attribut bei der Erstellung von Element <> mit einer leeren Zeichenfolge als Wert angegeben wird. Dies macht die Deklaration des Standardnamespace des übergeordneten Elements rückgängig.  
  

Dies ist das Ergebnis:  

```xml
<a xmlns="a">  
  <b xmlns="" />  
</a>  
```  
  
### <a name="xml-construction-and-white-space-handling"></a>XML-Konstruktion und Behandlung von Leerzeichen  
 Der Inhalt von Elementen in XML-Konstruktionen kann Leerzeichen enthalten. Diese Zeichen werden auf folgende Weise verarbeitet:  
  
-   Die Leerzeichen in den Namespace-URIs werden als der XSD-Typ **anyURI** behandelt. Im Einzelnen werden sie behandelt wie folgt:  
  
    -   Leerzeichen am Anfang und am Ende werden entfernt.  
  
    -   Interne Leerzeichenwerte werden zu einem einzigen Leerzeichen zusammengefasst  
  
-   Zeilenvorschubzeichen innerhalb des Attributinhalts werden durch Leerzeichen ersetzt. Alle anderen Leerzeichen bleiben unverändert.  
  
-   Leerzeichen innerhalb von Elementen werden beibehalten.  
  
 Das folgende Beispiel veranschaulicht die Behandlung von Leerzeichen beim Konstruieren von XML-Code:  
  
```sql
-- line feed is repaced by space.  
declare @x xml  
set @x=''  
select @x.query('  
  
declare namespace myNS="   https://       
 abc/  
xyz  
  
";  
<test attr="    my   
test   attr   
value    " >  
  
<a>  
  
This     is  a  
  
test  
  
</a>  
</test>  
') as XML_Result  
  
```  
  
 Dies ist das Ergebnis:  
  
```xml
-- result  
<test attr="<test attr="    my test   attr  value    "><a>  
  
This     is  a  
  
test  
  
</a></test>  
"><a>  
  
This     is  a  
  
test  
  
</a></test>  
```  
  
### <a name="other-direct-xml-constructors"></a>Andere direkte XML-Konstruktoren  
 Die Konstruktoren für Verarbeitungsanweisungen und XML-Kommentare verwenden dieselbe Syntax wie die ihnen entsprechenden XML-Konstrukte. Berechnete Konstruktoren für Textknoten werden ebenfalls unterstützt. Sie werden jedoch hauptsächlich zum Konstruieren von Textknoten in der XML-Datenbearbeitungssprache verwendet.  
  
 **Hinweis** Ein Beispiel für die Verwendung eines expliziten Textknotenkonstruktors finden Sie im speziellen Beispiel unter [Insert &#40;XML DML&#41;](../t-sql/xml/insert-xml-dml.md).  
  
 In der folgenden Abfrage enthält der konstruierte XML-Code ein Element, zwei Attribute, einen Kommentar und eine Verarbeitungsanweisung. Beachten Sie, dass ein Komma vor dem <`FirstLocation`> verwendet wird, da eine Sequenz erstellt wird.  
  
```sql
SELECT Instructions.query('  
  declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   <?myProcessingInstr abc="value" ?>,   
   <FirstLocation   
        WorkCtrID = "{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
        SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
       <!-- some comment -->  
       <?myPI some processing instructions ?>  
       { (/AWMI:root/AWMI:Location[1]/AWMI:step) }  
    </FirstLocation>   
') as Result   
FROM Production.ProductModel  
where ProductModelID=7;  
  
```  
  
 Dies ist das Teilergebnis:  
  
```xml
<?myProcessingInstr abc="value" ?>  
<FirstLocation WorkCtrID="10" SetupHrs="0.5">  
  <!-- some comment -->  
  <?myPI some processing instructions ?>  
  <AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">I  
  nsert <AWMI:material>aluminum sheet MS-2341</AWMI:material> into the <AWMI:tool>T-85A framing tool</AWMI:tool>.   
  </AWMI:step>  
    ...  
</FirstLocation>  
  
```  
  
## <a name="using-computed-constructors"></a>Verwenden von berechneten Konstruktoren  
 . In diesem Fall geben Sie die Schlüsselwörter an, die den Typ des zu konstruierenden Knotens identifizieren. Es werden nur folgende Schlüsselwörter unterstützt:  
  
-   element  
  
-   Attribut  
  
-   text  
  
 Bei Element- und Attributknoten folgen auf diese Schlüsselwörter der Knotenname sowie der Ausdruck in geschweiften Klammern, der den Inhalt dieses Knotens generiert. Im folgenden Beispiel wird dieser XML-Code konstruiert:  
  
```xml
<root>  
  <ProductModel PID="5">Some text <summary>Some Summary</summary></ProductModel>  
</root>  
```  
  
 Im Folgenden wird die Abfrage gezeigt, die die berechneten Konstruktoren zum Generieren des XML-Codes verwendet:  
  
```sql
declare @x xml  
set @x=''  
select @x.query('element root   
               {   
                  element ProductModel  
     {  
attribute PID { 5 },  
text{"Some text "},  
    element summary { "Some Summary" }  
 }  
               } ')  
  
```  
  
 Der Ausdruck, der den Knoteninhalt generiert, kann einen Abfrageausdruck angeben.  
  
```sql
declare @x xml  
set @x='<a attr="5"><b>some summary</b></a>'  
select @x.query('element root   
               {   
                  element ProductModel  
     {  
attribute PID { /a/@attr },  
text{"Some text "},  
    element summary { /a/b }  
 }  
               } ')  
```  
  
 Beachten Sie, dass die berechneten Element- und Attributkonstruktoren ermöglichen, Knotennamen zu berechnen, wie in der XQuery-Spezifikation definiert. Wenn Sie direkte Konstruktoren in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] verwenden, müssen Sie die Knotennamen, wie Element und Attribut, als konstante Literale angeben. Daher gibt es in Bezug auf Elemente und Attribute zwischen dem direkten und dem berechneten Konstruktor keinen Unterschied.  
  
 Im folgenden Beispiel wird der Inhalt für die konstruierten Knoten aus den in der Instructions-Spalte des **XML** -Datentyps in der ProductModel-Tabelle gespeicherten XML-Fertigungsanweisungen abgerufen.  
  
```sql
SELECT Instructions.query('  
  declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   element FirstLocation   
     {  
        attribute LocationID { (/AWMI:root/AWMI:Location[1]/@LocationID)[1] },  
        element   AllTheSteps { /AWMI:root/AWMI:Location[1]/AWMI:step }  
     }  
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7  
```  
  
 Dies ist das Teilergebnis:  
  
```xml
<FirstLocation LocationID="10">  
  <AllTheSteps>  
    <AWMI:step> ... </AWMI:step>  
    <AWMI:step> ... </AWMI:step>  
    ...  
  </AllTheSteps>  
</FirstLocation>    
```  
  
## <a name="additional-implementation-limitations"></a>Zusätzliche Implementierungseinschränkungen  
 Berechnete Attributkonstruktoren können nicht zum Deklarieren eines neuen Namespace verwendet werden. Außerdem werden in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] die folgenden berechneten Konstruktoren nicht unterstützt:  
  
-   Berechnete Konstruktoren für Dokumentknoten  
  
-   Berechnete Konstruktoren für Verarbeitungsanweisungen  
  
-   Berechnete Konstruktoren für Kommentare  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Ausdrücke](../xquery/xquery-expressions.md)  
  
  
