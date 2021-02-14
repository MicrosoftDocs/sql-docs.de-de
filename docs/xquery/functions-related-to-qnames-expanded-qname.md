---
title: Erweiterter QName (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Funktion "expanded-QName ()" verwenden, um den Namespace-URI und den lokalen Namensteil eines QName zurückzugeben.
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
- expanded-QName function
- fn:expanded-QName function
ms.assetid: b8377042-95cc-467b-9ada-fe43cebf4bc3
author: rothja
ms.author: jroth
ms.openlocfilehash: 704cac94b48fe172eaa6a972cdd6c49bad1601fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344931"
---
# <a name="functions-related-to-qnames---expanded-qname"></a>Funktionen, die sich auf QNames beziehen – expanded-QName
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Gibt einen Wert des xs: QName-Typs mit dem in der *$paramURI* angegebenen Namespace-URI und dem im *$paramLocal* angegebenen lokalen Namen zurück. Wenn *$paramURI* eine leere Zeichenfolge oder eine leere Sequenz ist, stellt Sie keinen Namespace dar.  
  
## <a name="syntax"></a>Syntax  
  
```  
fn:expanded-QName($paramURI as xs:string?, $paramLocal as xs:string?) as xs:QName?  
```  
  
## <a name="arguments"></a>Argumente  
 *$paramURI*  
 Der Namespace-URI (Universal Resource Identifier) für QName.  
  
 *$paramLocal*  
 Der lokale Teil des Namens von QName.  
  
## <a name="remarks"></a>Bemerkungen  
 Folgendes gilt für die **expanded-QName ()-** Funktion:  
  
-   Wenn sich der angegebene *$paramLocal* Wert nicht in der richtigen lexikalischen Form für den xs: NcName-Typ befindet, wird die leere Sequenz zurückgegeben und stellt einen dynamischen Fehler dar.  
  
-   Das Konvertieren des Typs xs:QName in andere Typen wird in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nicht unterstützt. Aus diesem Grund kann die **Erweiterte QName ()-** Funktion nicht in der XML-Konstruktion verwendet werden. Wenn Sie beispielsweise einen Knoten wie `<e> expanded-QName(...) </e>` konstruieren, darf der Wert nicht typisiert sein. Um dies zu erreichen, müssten Sie den von `expanded-QName()` zurückgegebenen Wert vom Typ xs:QName in xdt:untypedAtomic konvertieren. Dies wird jedoch nicht unterstützt. Eine Lösungsmöglichkeit wird nachfolgend in diesem Thema bereitgestellt.  
  
-   Sie können vorhandene Werte vom Typ QName ändern oder vergleichen. `/root[1]/e[1] eq expanded-QName("http://nsURI" "myNS")`Vergleicht z. b. den Wert des-Elements, <`e`> mit dem von der **expanded-QName ()-** Funktion zurückgegebenen QName.  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der-Datenbank gespeichert sind [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] .  
  
### <a name="a-replacing-a-qname-type-node-value"></a>A. Ersetzen eines Knotenwerts vom Typ QName  
 In diesem Beispiel wird veranschaulicht, wie Sie den Wert eines Elementknotens vom Typ QName ändern können. Das Beispiel führt die folgenden Aktionen aus:  
  
-   Erstellen einer XML-Schemaauflistung, die ein Element vom Typ QName definiert.  
  
-   Erstellt eine Tabelle mit einer Spalte vom Typ **XML** mithilfe der XML-Schema Auflistung.  
  
-   Speichern einer XML-Instanz in der Tabelle.  
  
-   Verwendet die **Modify ()** -Methode des XML-Datentyps, um den Wert des QName-typelements in der-Instanz zu ändern. Die **expanded-QName ()-** Funktion wird verwendet, um den neuen QName-Typwert zu generieren.  
  
```  
-- If XML schema collection (if exists)  
-- drop xml schema collection SC  
-- go  
-- Create XML schema collection  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
    xmlns:xs="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="QNameXSD"   
      xmlns:xqo="QNameXSD" elementFormDefault="qualified">  
      <element name="Root" type="xqo:rootType" />  
      <complexType name="rootType">  
            <sequence minOccurs="1" maxOccurs="1">  
                        <element name="ElemQN" type="xs:QName" />  
            </sequence>  
      </complexType>  
</schema>'  
go  
-- Create table.  
CREATE TABLE T( XmlCol xml(SC) )  
-- Insert sample XML instnace  
INSERT INTO T VALUES ('  
<Root xmlns="QNameXSD" xmlns:ns="https://myURI">  
      <ElemQN>ns:someName</ElemQN>  
</Root>')  
go  
-- Verify the insertion  
SELECT * from T  
go  
-- Result  
<Root xmlns="QNameXSD" xmlns:ns="https://myURI">  
  <ElemQN>ns:someName</ElemQN>  
</Root>   
```  
  
 In der folgenden Abfrage `ElemQN` wird der Wert <> Element ersetzt, indem die **Modify ()** -Methode des XML-Datentyps und der replace value of XML DML (wie gezeigt) verwendet wird.  
  
```  
-- the value.  
UPDATE T   
SET XmlCol.modify('  
  declare default element namespace "QNameXSD";   
  replace value of /Root[1]/ElemQN   
  with expanded-QName("https://myURI", "myLocalName") ')  
go  
-- Verify the result  
SELECT * from T  
go  
```  
  
 Dies ist das Ergebnis. Beachten Sie, dass das Element <`ElemQN`> vom Typ QName nun über einen neuen Wert verfügt:  
  
```  
<Root xmlns="QNameXSD" xmlns:ns="urn">  
  <ElemQN xmlns:p1="https://myURI">p1:myLocalName</ElemQN>  
</Root>  
```  
  
 Die folgenden Anweisungen entfernen die in diesem Beispiel verwendeten Objekte.  
  
```  
-- Cleanup  
DROP TABLE T  
go  
drop xml schema collection SC  
go  
```  
  
### <a name="b-dealing-with-the-limitations-when-using-the-expanded-qname-function"></a>B. Umgehend mit den Einschränkungen bei Verwendung der erweiterten QName()-Funktion  
 Die **expanded-QName-** Funktion kann in der XML-Konstruktion nicht verwendet werden. Dies wird anhand des folgenden Beispiels veranschaulicht. Im Beispiel wird zuerst ein Knoten eingefügt, um diese Einschränkung zu umgehen, und dann wird der Knoten geändert.  
  
```  
-- if exists drop the table T  
--drop table T  
-- go  
-- Create XML schema collection  
-- DROP XML SCHEMA COLLECTION SC  
-- go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" type="QName" nillable="true"/>  
</schema>'  
go  
 -- Create table T with a typed xml column (using the XML schema collection)  
CREATE TABLE T (xmlCol XML(SC))  
go  
-- Insert an XML instance.  
insert into T values ('<root xmlns:a="https://someURI">a:b</root>')  
 go  
-- Verify  
SELECT *   
FROM T  
```  
  
 Der folgende Versuch fügt ein weiteres <`root`>-Element hinzu, schlägt jedoch fehl, da die erweiterte QName ()-Funktion in der XML-Konstruktion nicht unterstützt wird.  
  
```  
update T SET xmlCol.modify('  
insert <root>{expanded-QName("http://ns","someLocalName")}</root> as last into / ')  
go  
```  
  
 Eine Lösung hierfür besteht darin, zuerst eine Instanz mit einem Wert für das <`root`> Element einzufügen und dann zu ändern. In diesem Beispiel wird ein Nil-Anfangswert verwendet, wenn das <`root`>-Element eingefügt wird. Die XML-Schema Auflistung in diesem Beispiel lässt einen Nullwert für das <`root`>-Element zu.  
  
```  
update T SET xmlCol.modify('  
insert <root xsi:nil="true"/> as last into / ')  
go  
-- now replace the nil value with another QName.  
update T SET xmlCol.modify('  
replace value of /root[last()] with expanded-QName("http://ns","someLocalName") ')  
go  
 -- verify   
SELECT * FROM T  
go  
-- result  
<root>b</root>  
```  
  
 `<root xmlns:a="https://someURI">a:b</root>`  
  
 `<root xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p1="http://ns">p1:someLocalName</root>`  
  
 Sie können den Wert für QName vergleichen, wie in der folgenden Abfrage gezeigt: Die Abfrage gibt nur die <`root`> Elemente zurück, deren Werte mit dem von der **erweiterten QName ()-** Funktion zurückgegebenen Wert des QName-Typs identisch sind.  
  
```  
SELECT xmlCol.query('  
    for $i in /root  
    return  
       if ($i eq expanded-QName("http://ns","someLocalName") ) then  
          $i  
       else  
          ()')  
FROM T  
```  
  
### <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Es gibt eine Einschränkung: die Funktion " **expanded-QName ()** " akzeptiert die leere Sequenz als zweites Argument und gibt "Empty" zurück, anstatt einen Laufzeitfehler zu erhalten, wenn das zweite Argument falsch ist.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Funktionen im Zusammenhang mit QNames &#40;XQuery-&#41;]()  
  
