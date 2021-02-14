---
title: SequenceType-Ausdrücke (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die XQuery SequenceType-Ausdrucks Instanz von, und wandeln Sie Sie in um.
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
- SequenceType expressions
- instance of operator [XQuery]
- expressions [XQuery], SequenceType
- cast as operator
ms.assetid: ad3573da-d820-4d1c-81c4-a83c4640ce22
author: rothja
ms.author: jroth
ms.openlocfilehash: 0627d892733f84cb4a8d1b5cf80ad65d9c09f824
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335846"
---
# <a name="sequencetype-expressions-xquery"></a>SequenceType-Ausdrücke (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  In XQuery ist jeder Wert eine Sequenz. Der Typ des Werts wird als Sequenztyp bezeichnet. Der Sequenztyp kann in einer **Instanz des** XQuery-Ausdrucks verwendet werden. Die in der XQuery-Spezifikation beschriebene SequenceType-Syntax wird verwendet, wenn in einem XQuery-Ausdruck auf einen Typ verwiesen werden muss.  
  
 Der atomarische Typname kann auch im **Cast als** XQuery-Ausdruck verwendet werden. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] werden die **Instanz von** , die **als** XQuery-Ausdrücke in sequencetypes umgewandelt wird, teilweise unterstützt.  
  
## <a name="instance-of-operator"></a>instance of-Operator  
 Die **Instanz des** -Operators kann verwendet werden, um den dynamischen oder den Lauf Zeittyp des Werts des angegebenen Ausdrucks zu bestimmen. Beispiel:  
  
```  
  
Expression instance of SequenceType[Occurrence indicator]  
```  
  
 Beachten Sie, dass der- `instance of` Operator, der die `Occurrence indicator` Kardinalität, die Anzahl der Elemente in der resultierenden Sequenz angibt. Wenn dieser Operator nicht angegeben wird, wird eine Kardinalität von 1 verwendet. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] wird nur der Indikator "Fragezeichen (**?)** vorkommen" unterstützt. Der **?**-Quantifizierer der Vorkommen Indikator gibt an, dass `Expression` null oder ein Element zurückgeben kann. Wenn **?** der Vorkommen Indikator wird angegeben. `instance of` gibt true zurück, wenn der `Expression` Typ mit dem angegebenen übereinstimmt `SequenceType` , unabhängig davon, ob `Expression` einen Singleton oder eine leere Sequenz zurückgibt.  
  
 Wenn **?** der Vorkommen Indikator ist nicht angegeben, `sequence of` gibt nur dann true zurück, wenn der `Expression` Typ mit dem angegebenen übereinstimmt `Type` und `Expression` ein Singleton zurückgibt.  
  
 **Hinweis** Das Pluszeichen ( **+** ) und das Sternchen (**&#42;**) werden in nicht unterstützt [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 In den folgenden Beispielen wird die Verwendung der **Instanz des** XQuery-Operators veranschaulicht.  
  
### <a name="example-a"></a>Beispiel A  
 Im folgenden Beispiel wird eine Variable vom Typ **XML** erstellt und eine Abfrage dafür angegeben. Der Abfrageausdruck gibt einen `instance of`-Operator an, um zu bestimmen, ob der dynamische Typ des von dem ersten Operanden zurückgegebenen Werts mit dem im zweiten Operanden angegebenen Typ übereinstimmt.  
  
 Die folgende Abfrage gibt true zurück, da der 125-Wert eine Instanz des angegebenen Typs ( **xs: Integer**) ist:  
  
```  
declare @x xml  
set @x=''  
select @x.query('125 instance of xs:integer')  
go  
```  
  
 Die folgende Abfrage gibt True zurück, da der von dem Ausdruck (/a[1]) zurückgegebene Wert im ersten Operanden ein Element ist:  
  
```  
declare @x xml  
set @x='<a>1</a>'  
select @x.query('/a[1] instance of element()')  
go  
```  
  
 Entsprechend gibt `instance of` in der folgenden Abfrage True zurück, da der Werttyp des Ausdrucks im ersten Ausdruck ein Attribut ist:  
  
```  
declare @x xml  
set @x='<a attr1="x">1</a>'  
select @x.query('/a[1]/@attr1 instance of attribute()')  
go  
```  
  
 Im folgenden Beispiel gibt der Ausdruck `data(/a[1]` einen atomaren Wert mit der Typisierung xdt:untypedAtomic zurück. Folglich gibt `instance of` TRUE zurück.  
  
```  
declare @x xml  
set @x='<a>1</a>'  
select @x.query('data(/a[1]) instance of xdt:untypedAtomic')  
go  
```  
  
 In der folgenden Abfrage gibt der Ausdruck `data(/a[1]/@attrA` einen nicht typisierten atomaren Wert zurück. Folglich gibt `instance of` TRUE zurück.  
  
```  
declare @x xml  
set @x='<a attrA="X">1</a>'  
select @x.query('data(/a[1]/@attrA) instance of xdt:untypedAtomic')  
go  
```  
  
### <a name="example-b"></a>Beispiel B  
 In diesem Beispiel fragen Sie eine typisierte XML-Spalte in der AdventureWorks-Beispieldatenbank ab. Die der abzufragenden Spalte zugeordnete XML-Schemaauflistung stellt die Typisierungsinformationen bereit.  
  
 Im Ausdruck gibt **Data ()** den typisierten Wert des ProductModelID-Attributs zurück, dessen Typ xs: String gemäß dem Schema ist, das der Spalte zugeordnet ist. Folglich gibt `instance of` TRUE zurück.  
  
```  
SELECT CatalogDescription.query('  
   declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
   data(/PD:ProductDescription[1]/@ProductModelID) instance of xs:string  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Weitere Informationen finden Sie unter [Vergleichen von typisiertem XML mit nicht typisiertem XML](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
 Der folgende Abfrage useteboolean- `instance of` Ausdruck bestimmt, ob das LocationID-Attribut vom Typ xs: Integer ist:  
  
```  
SELECT Instructions.query('  
   declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   /AWMI:root[1]/AWMI:Location[1]/@LocationID instance of attribute(LocationID,xs:integer)  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Die folgende Abfrage wird für die typisierte CatalogDescription-Spalte vom Typ XML angegeben. Die dieser Spalte zugeordnete XML-Schemaauflistung stellt die Typisierungsinformationen bereit.  
  
 Die Abfrage verwendet den `element(ElementName, ElementType?)`-Test im `instance of`-Ausdruck, um zu überprüfen, ob `/PD:ProductDescription[1]` einen Elementknoten eines bestimmten Namens und Typs zurückgibt.  
  
```  
SELECT CatalogDescription.query('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     /PD:ProductDescription[1] instance of element(PD:ProductDescription, PD:ProductDescription?)  
    ') as Result  
FROM  Production.ProductModel  
where ProductModelID=19  
```  
  
 Diese Abfrage gibt True zurück.  
  
### <a name="example-c"></a>Beispiel C  
 Beim Verwenden von UNION-Datentypen in einem `instance of`-Ausdruck gilt in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] die folgende Einschränkung: speziell wenn ein Element oder Attribut vom Typ UNION ist, kann `instance of` den genauen Typ möglicherweise nicht bestimmen. Folglich gibt eine Abfrage False zurück, es sei denn, der in SequenceType verwendete atomare Typ ist das höchste übergeordnete Element des aktuellen Typs des Ausdrucks in der simpleType-Hierarchie. Mit anderen Worten müssen die in SequenceType angegebenen atomaren Typen dem Typ anySimpleType direkt untergeordnet sein. Weitere Informationen zur Typhierarchie finden Sie unter [Typumwandlungs Regeln in XQuery](../xquery/type-casting-rules-in-xquery.md).  
  
 Das nächste Abfragebeispiel führt folgende Vorgänge aus:  
  
-   Erstellen einer XML-Schemaauflistung und Definieren eines UNION-Typs wie integer oder string.  
  
-   Deklarieren Sie eine typisierte **XML** -Variable mithilfe der XML-Schema Auflistung.  
  
-   Zuordnen einer XML-Beispielinstanz zu der Variablen.  
  
-   Abfragen der Variablen, um das Verhalten von `instance of` mit einem UNION-Typ zu veranschaulichen.  
  
 Im Folgenden wird die Abfrage aufgeführt:  
  
```  
CREATE XML SCHEMA COLLECTION MyTestSchema AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema" targetNamespace="http://ns" xmlns:ns="http://ns">  
<simpleType name="MyUnionType">  
<union memberTypes="integer string"/>  
</simpleType>  
<element name="TestElement" type="ns:MyUnionType"/>  
</schema>'  
Go  
```  
  
 Die folgende Abfrage gibt False zurück, da der in dem `instance of`-Ausdruck angegebene Sequenztyp nicht das höchste übergeordnete Element des aktuellen Typs des angegebenen Ausdrucks ist. Das heißt, der Wert der <`TestElement`> ist ein ganzzahliger Typ. das höchste übergeordnete Element ist xs:decimal. Es ist jedoch nicht als der zweite Operand des `instance of`-Operators angegeben.  
  
```  
SET QUOTED_IDENTIFIER ON  
DECLARE @var XML(MyTestSchema)  
  
SET @var = '<TestElement xmlns="http://ns">123</TestElement>'  
  
SELECT @var.query('declare namespace ns="http://ns"   
   data(/ns:TestElement[1]) instance of xs:integer')  
go  
```  
  
 Nachdem das höchste übergeordnete Element von xs:integer xs:decimal ist, würde die Abfrage True zurückgeben, wenn Sie sie ändern und xs:decimal als SequenceType angeben.  
  
```  
SET QUOTED_IDENTIFIER ON  
DECLARE @var XML(MyTestSchema)  
SET @var = '<TestElement xmlns="http://ns">123</TestElement>'  
SELECT @var.query('declare namespace ns="http://ns"     
   data(/ns:TestElement[1]) instance of xs:decimal')  
go  
```  
  
### <a name="example-d"></a>Beispiel D  
 In diesem Beispiel erstellen Sie zuerst eine XML-Schema Auflistung und verwenden Sie, um eine **XML** -Variable einzugeben. Die typisierte **XML** -Variable wird dann abgefragt, um die Funktionalität zu veranschaulichen `instance of` .  
  
 Die folgende XML-Schema Auflistung definiert einen einfachen Typ, MyType und ein Element, <`root`> vom Typ MyType:  
  
```  
drop xml schema collection SC  
go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema" targetNamespace="myNS" xmlns:ns="myNS"  
xmlns:s="https://schemas.microsoft.com/sqlserver/2004/sqltypes">  
      <import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
      <simpleType name="myType">  
           <restriction base="s:varchar">  
                  <maxLength value="20"/>  
            </restriction>  
      </simpleType>  
      <element name="root" type="ns:myType"/>  
</schema>'  
Go  
```  
  
 Erstellen Sie jetzt eine typisierte **XML** -Variable und Fragen Sie Sie ab:  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xmlns="myNS">My data</root>'  
SELECT @var.query('declare namespace sqltypes = "https://schemas.microsoft.com/sqlserver/2004/sqltypes";  
declare namespace ns="myNS";   
   data(/ns:root[1]) instance of ns:myType')  
go  
```  
  
 Nachdem der myType-Typ durch Einschränkung aus einem im sqltpypes-Schema definierten varchar-Typ abgeleitet ist, gibt `instance of` ebenfalls True zurück.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xmlns="myNS">My data</root>'  
SELECT @var.query('declare namespace sqltypes = "https://schemas.microsoft.com/sqlserver/2004/sqltypes";  
declare namespace ns="myNS";   
data(/ns:root[1]) instance of sqltypes:varchar?')  
go  
```  
  
### <a name="example-e"></a>Beispiel E  
 Im folgenden Beispiel ruft der Ausdruck einen der Werte des IDREFS-Attributs auf und verwendet `instance of`, um zu bestimmen, ob der Wert vom Typ IDREF ist. Das Beispiel führt die folgenden Aktionen aus:  
  
-   Erstellt eine XML-Schema Auflistung, in der das <`Customer`>-Element ein **OrderList** -Attribut vom Typ IDREFS aufweist und das <`Order`>-Element über ein OrderID-Attribut vom Typ " **OrderID** " verfügt.  
  
-   Erstellt eine typisierte **XML** -Variable und weist ihr eine XML-Beispiel Instanz zu.  
  
-   Gibt eine Abfrage für die Variable an. Der Abfrage Ausdruck ruft den ersten Bestellungs-ID-Wert aus dem OrderList idrer Type-Attribut der ersten <`Customer`> ab. Der abgerufene Wert ist vom Typ IDREF. Folglich gibt `instance of` True zurück.  
  
```  
create xml schema collection SC as  
'<schema xmlns="http://www.w3.org/2001/XMLSchema" xmlns:Customers="Customers" targetNamespace="Customers">  
            <element name="Customers" type="Customers:CustomersType"/>  
            <complexType name="CustomersType">  
                        <sequence>  
                            <element name="Customer" type="Customers:CustomerType" minOccurs="0" maxOccurs="unbounded" />  
                        </sequence>  
            </complexType>  
             <complexType name="OrderType">  
                <sequence minOccurs="0" maxOccurs="unbounded">  
                            <choice>  
                                <element name="OrderValue" type="integer" minOccurs="0" maxOccurs="unbounded"/>  
                            </choice>  
                </sequence>                                             
                <attribute name="OrderID" type="ID" />  
            </complexType>  
  
            <complexType name="CustomerType">  
                <sequence minOccurs="0" maxOccurs="unbounded">  
                            <choice>  
                                <element name="spouse" type="string" minOccurs="0" maxOccurs="unbounded"/>  
                                <element name="Order" type="Customers:OrderType" minOccurs="0" maxOccurs="unbounded"/>  
                            </choice>  
                </sequence>                                             
                <attribute name="CustomerID" type="string" />  
                <attribute name="OrderList" type="IDREFS" />  
            </complexType>  
 </schema>'  
go  
declare @x xml(SC)  
set @x='<CustOrders:Customers xmlns:CustOrders="Customers">  
                <Customer CustomerID="C1" OrderList="OrderA OrderB"  >  
                              <spouse>Jenny</spouse>  
                                <Order OrderID="OrderA"><OrderValue>11</OrderValue></Order>  
                                <Order OrderID="OrderB"><OrderValue>22</OrderValue></Order>  
  
                </Customer>  
                <Customer CustomerID="C2" OrderList="OrderC OrderD" >  
                                <spouse>John</spouse>  
                                <Order OrderID="OrderC"><OrderValue>33</OrderValue></Order>  
                                <Order OrderID="OrderD"><OrderValue>44</OrderValue></Order>  
  
                        </Customer>  
                <Customer CustomerID="C3"  OrderList="OrderE OrderF" >  
                                <spouse>Jane</spouse>  
                                <Order OrderID="OrderE"><OrderValue>55</OrderValue></Order>  
                                <Order OrderID="OrderF"><OrderValue>55</OrderValue></Order>  
                </Customer>  
                <Customer CustomerID="C4"  OrderList="OrderG"  >  
                                <spouse>Tim</spouse>  
                                <Order OrderID="OrderG"><OrderValue>66</OrderValue></Order>  
                        </Customer>  
                <Customer CustomerID="C5"  >  
                </Customer>  
                <Customer CustomerID="C6" >  
                </Customer>  
                <Customer CustomerID="C7"  >  
                </Customer>  
</CustOrders:Customers>'  
  
select @x.query(' declare namespace CustOrders="Customers";   
 data(CustOrders:Customers/Customer[1]/@OrderList)[1] instance of xs:IDREF ? ') as XML_result  
```  
  
### <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Die Sequenz Typen **Schema-Element ()** und **Schema-Attribute ()** werden für den Vergleich mit dem-Operator nicht unterstützt `instance of` .  
  
-   Vollständige Sequenzen wie z. B. `(1,2) instance of xs:integer*` werden nicht unterstützt.  
  
-   Wenn Sie eine Form des Element Sequenz Typs **()** verwenden, der einen Typnamen angibt, z. b `element(ElementName, TypeName)` ., muss der Typ mit einem Fragezeichen (?) qualifiziert werden. `element(Title, xs:string?)` gibt beispielsweise an, dass für das Element NULL-Werte zulässig sind. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützt keine Lauf Zeit Erkennung der **xsi: Nil** -Eigenschaft mithilfe von `instance of` .  
  
-   Wenn der Wert in `Expression` aus einem als UNION typisierten Element oder Attribut stammt, identifiziert [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nur den Grundtyp, nicht aber den Typ, aus dem der Typ des Werts abgeleitet wurde. Wenn <> beispielsweise `e1` als statischer Typ (xs: integer | xs: String) definiert ist, wird false zurückgegeben.  
  
    ```  
    data(<e1>123</e1>) instance of xs:integer  
    ```  
  
     Jedoch gibt `data(<e1>123</e1>) instance of xs:decimal` True zurück.  
  
-   Für die Sequenz Typen **processing-instruction ()** und **Document-Node ()** sind nur Formulare ohne Argumente zulässig. Beispielsweise `processing-instruction()` ist zulässig, jedoch `processing-instruction('abc')` nicht zulässig.  
  
## <a name="cast-as-operator"></a>cast as-Operator  
 Der **cast as** -Ausdruck kann verwendet werden, um einen Wert in einen bestimmten Datentyp zu konvertieren. Beispiel:  
  
```  
  
Expression cast as  AtomicType?  
```  
  
 In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ist das Fragezeichen (?) nach `AtomicType` erforderlich. Wie in der folgenden Abfrage gezeigt, konvertiert z. b. `"2" cast as xs:integer?` den Zeichen folgen Wert in eine ganze Zahl:  
  
```  
declare @x xml  
set @x=''  
select @x.query('"2" cast as xs:integer?')  
```  
  
 In der folgenden Abfrage gibt **Data ()** den typisierten Wert des ProductModelID-Attributs zurück, einen Zeichen Folgentyp. Der- `cast as` Operator konvertiert den Wert in xs: Integer.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD)  
SELECT CatalogDescription.query('  
   data(/PD:ProductDescription[1]/@ProductModelID) cast as xs:integer?  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Die explizite Verwendung von **Daten ()** ist in dieser Abfrage nicht erforderlich. Der `cast as`-Ausdruck führt die implizite Atomisierung des Eingabeausdrucks aus.  
  
### <a name="constructor-functions"></a>Konstruktorfunktionen  
 Sie können die Konstruktorfunktionen für atomare Typen verwenden. Anstatt den-Operator zu verwenden, `cast as` `"2" cast as xs:integer?` können Sie beispielsweise die **xs: Integer ()** -Konstruktorfunktion verwenden, wie im folgenden Beispiel gezeigt:  
  
```  
declare @x xml  
set @x=''  
select @x.query('xs:integer("2")')  
```  
  
 Das folgende Beispiel gibt einen Datumswert vom Typ xs:date gleich 2000-01-01Z zurück.  
  
```  
declare @x xml  
set @x=''  
select @x.query('xs:date("2000-01-01Z")')  
```  
  
 Sie können Konstruktoren auch für benutzerdefinierte atomare Typen verwenden. Wenn z. b. die XML-Schema Auflistung, die dem XML-Datentyp zugeordnet ist, einen einfachen Typ definiert, kann ein **MyType ()** -Konstruktor verwendet werden, um einen Wert dieses Typs zurückzugeben.  
  
#### <a name="implementation-limitations"></a>Implementierungseinschränkungen  
  
-   Die XQuery-Ausdrücke **typeswitch**, **castable** und **Treat** werden nicht unterstützt.  
  
-   Umwandlung **als** erfordert ein Fragezeichen (?) nach dem Atomic-Typ.  
  
-   **xs: QName** wird nicht als Typ für die Umwandlung unterstützt. Verwenden Sie stattdessen " **expanded-QName** ".  
  
-   **xs: Date**, **xs: Time** und **xs: DateTime** erfordern eine Zeitzone, die durch a Z angegeben wird.  
  
     Die folgende Abfrage schlägt fehl, da die Zeitzone nicht angegeben ist.  
  
    ```  
    DECLARE @var XML  
    SET @var = ''  
    SELECT @var.query(' <a>{xs:date("2002-05-25")}</a>')  
    go  
    ```  
  
     Wenn Sie dem Wert ein Z zum Anzeigen der Zeitzone hinzufügen, kann die Abfrage ordnungsgemäß ausgeführt werden.  
  
    ```  
    DECLARE @var XML  
    SET @var = ''  
    SELECT @var.query(' <a>{xs:date("2002-05-25Z")}</a>')  
    go  
    ```  
  
     Dies ist das Ergebnis:  
  
    ```  
    <a>2002-05-25Z</a>  
    ```  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Ausdrücke](../xquery/xquery-expressions.md)   
 [Geben Sie System &#40;XQuery ein&#41;](../xquery/type-system-xquery.md)  
  
  
