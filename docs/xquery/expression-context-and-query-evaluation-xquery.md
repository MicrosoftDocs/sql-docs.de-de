---
title: Ausdrucks Kontext und Abfrage Auswertung (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie, wie Informationen aus dem statischen und dynamischen Kontext eines XQuery-Ausdrucks verwendet werden, um ihn zu analysieren und zu evaluieren.
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
- expression context [XQuery]
- XQuery, expression context
- XQuery, query evaluation
- static context
- dynamic context [XQuery]
ms.assetid: 5059f858-086a-40d4-811e-81fedaa18b06
author: rothja
ms.author: jroth
ms.openlocfilehash: f4fd85737d1a7cdc981576f624a2a403f0eb19ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341951"
---
# <a name="expression-context-and-query-evaluation-xquery"></a>Ausdruckskontext und Ausdrucksauswertung (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Der Kontext eines Ausdrucks sind die Informationen, die zum Analysieren und Auswerten des Ausdrucks verwendet werden. Die Auswertung von XQuery erfolgt in den folgenden beiden Phasen:  
  
-   **Statischer Kontext** : Dies ist die Abfrage Kompilierungs Phase. Auf der Basis der verfügbaren Informationen werden während dieser statischen Analyse der Abfrage manchmal Fehler ausgelöst.  
  
-   **Dynamischer Kontext** : Dies ist die Ausführungsphase der Abfrage. Selbst wenn eine Abfrage keine statischen Fehler wie z. B. Fehler während der Abfragekompilierung aufweist, kann die Abfrage trotzdem während ihrer Ausführung Fehler auslösen.  
  
## <a name="static-context"></a>Statischer Kontext  
 Als Initialisierung des statischen Kontexts wird der Prozess bezeichnet, in dem alle Informationen für die statische Analyse des Ausdrucks zusammengestellt werden. Im Rahmen der Initialisierung des statischen Kontexts werden die folgenden Schritte durchgeführt:  
  
-   Die Richtlinie für **Begrenzungs Leerraum** ist auf Strip festgelegt. Daher wird der Begrenzungs Leerraum nicht von den Konstruktoren **any-Element** und- **Attribut** in der Abfrage beibehalten. Beispiel:  
  
    ```  
    declare @x xml  
    set @x=''  
    select @x.query('<a>  {"Hello"}  </a>,  
  
        <b> {"Hello2"}  </b>')  
    ```  
  
     Diese Abfrage gibt das folgende Ergebnis zurück, weil das Grenzleerzeichen beim Analysieren des XQuery-Ausdrucks entfernt wird:  
  
    ```  
    <a>Hello</a><b>Hello2</b>  
    ```  
  
-   Das Präfix und die Namespacebindung werden für folgende Namespaces initialisiert:  
  
    -   Einen Satz von vordefinierten Namespaces.  
  
    -   Alle mithilfe von WITH XMLNAMESPACES definierten Namespaces. Weitere Informationen finden Sie unter [Hinzufügen von Namespaces zu Abfragen mit WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)).  
  
    -   Alle im Abfrageprolog definierten Namespaces. Beachten Sie, dass die Namespacedeklarationen im Prolog die Namespacedeklaration in WITH XMLNAMESPACES überschreiben können. In der folgenden Abfrage deklariert XMLNAMESPACES z. b. ein Präfix (PD), das Sie an den-Namespace ( `https://someURI` ) bindet. Diese Bindung wird jedoch in der WHERE-Klausel durch den Abfrageprolog überschrieben.  
  
        ```  
        WITH XMLNAMESPACES ('https://someURI' AS pd)  
        SELECT ProductModelID, CatalogDescription.query('  
            <Product   
                ProductModelID= "{ sql:column("ProductModelID") }"   
                />  
        ') AS Result  
        FROM Production.ProductModel  
        WHERE CatalogDescription.exist('  
            declare namespace  pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
             /pd:ProductDescription[(pd:Specifications)]'  
            ) = 1  
        ```  
  
     Alle diese Namespacebindungen werden bei der Initialisierung des statischen Kontexts aufgelöst.  
  
-   Wenn eine typisierte **XML** -Spalte oder-Variable abgefragt wird, werden die Komponenten der XML-Schema Auflistung, die der Spalte oder Variablen zugeordnet ist, in den statischen Kontext importiert. Weitere Informationen finden Sie unter [Vergleichen von typisiertem XML mit nicht typisiertem XML](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
-   Für jeden atomaren Typ in den importierten Schemas wird im statischen Kontext auch eine Umwandlungsfunktion verfügbar gemacht. Dies wird im folgenden Beispiel illustriert. In diesem Beispiel wird eine Abfrage für eine typisierte **XML** -Variable angegeben. Die dieser Variablen zugeordnete XML-Schemaauflistung definiert den atomaren Typ myType. Entsprechend diesem Typ ist die Typumwandlungs Funktion **MyType ()** während der statischen Analyse verfügbar. Der Abfrageausdruck (`ns:myType(0)`) gibt einen Wert von myType zurück.  
  
    ```  
    -- DROP XML SCHEMA COLLECTION SC  
    -- go  
    CREATE XML SCHEMA COLLECTION SC AS '<schema xmlns="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="myNS" xmlns:ns="myNS"  
    xmlns:s="https://schemas.microsoft.com/sqlserver/2004/sqltypes">  
          <import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
          <simpleType name="myType">  
                <restriction base="int">  
                 <enumeration value="0" />  
                  <enumeration value="1"/>  
                </restriction>  
          </simpleType>  
          <element name="root" type="ns:myType"/>  
    </schema>'  
    go  
  
    DECLARE @var XML(SC)  
    SET @var = '<root xmlns="myNS">0</root>'  
    -- specify myType() casting function in the query  
    SELECT @var.query('declare namespace ns="myNS"; ns:myType(0)')  
    ```  
  
     Im folgenden Beispiel wird die Umwandlungs Funktion für den integrierten XML-Typ **int** im Ausdruck angegeben.  
  
    ```  
    declare @x xml  
    set @x = ''  
    select @x.query('xs:int(5)')  
    go  
    ```  
  
 Nachdem der statische Kontext initialisiert wurde, wird der Abfrageausdruck analysiert (kompiliert). Die statische Analyse umfasst die folgenden Schritte:  
  
1.  Analysieren der Abfrage.  
  
2.  Auflösen der im Ausdruck angegebenen Funktions- und Typnamen.  
  
3.  Statisches Typisieren der Abfrage. Damit wird sichergestellt, dass die Abfrage typsicher ist. Beispielsweise gibt die folgende Abfrage einen statischen Fehler zurück, da der **+** Operator numerische primitive Typargumente erfordert:  
  
    ```  
    declare @x xml  
    set @x=''  
    SELECT @x.query('"x" + 4')  
    ```  
  
     Im folgenden Beispiel erfordert der **value ()** -Operator einen Singleton. Wie im XML-Schema angegeben, können mehrere Elemente vorhanden sein \<Elem> . Bei der statischen Analyse des Ausdrucks wird ermittelt, dass der Ausdruck nicht typsicher ist, und es wird ein statischer Fehler zurückgegeben. Um den Fehler aufzulösen, muss der Ausdruck so umgeschrieben werden, dass er explizit einen Singleton-Wert angibt (`data(/x:Elem)[1]`).  
  
    ```  
    DROP XML SCHEMA COLLECTION SC  
    go  
    CREATE XML SCHEMA COLLECTION SC AS '<schema xmlns="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="myNS" xmlns:ns="myNS"  
    xmlns:s="https://schemas.microsoft.com/sqlserver/2004/sqltypes">  
          <import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
          <element name="Elem" type="string"/>  
    </schema>'  
    go  
  
    declare @x xml (SC)  
    set @x='<Elem xmlns="myNS">test</Elem><Elem xmlns="myNS">test2</Elem>'  
    SELECT @x.value('declare namespace x="myNS"; data(/x:Elem)[1]','varchar(20)')  
    ```  
  
     Weitere Informationen finden Sie unter [XQuery und statische Typisierung](../xquery/xquery-and-static-typing.md).  
  
### <a name="implementation-restrictions"></a>Einschränkungen für die Implementierung  
 Im Zusammenhang mit dem statischen Kontext gelten die folgenden Einschränkungen:  
  
-   Der XPath-Kompatibilitätsmodus wird nicht unterstützt.  
  
-   Für die XML-Konstruktion wird nur der strip-Konstruktionsmodus unterstützt. Dies ist die Standardeinstellung. Daher ist der Typ des konstruierten Element Knotens **xdt: nicht typisierter** Typ, und die Attribute weisen den **xdt: untypedAtomic** -Typ auf.  
  
-   Es wird nur der der ordered-Sortiermodus strip XML unterstützt.  
  
-   Es wird nur die strip-XML-Leerzeichenrichtlinie unterstützt.  
  
-   Die Basis-URI-Funktionalität wird nicht unterstützt.  
  
-   **FN: doc ()** wird nicht unterstützt.  
  
-   **FN: Collection ()** wird nicht unterstützt.  
  
-   Es wird kein statischer Flagger für XQuery bereitgestellt.  
  
-   Die Sortierung, die dem **XML** -Datentyp zugeordnet ist, wird verwendet. Diese Sortierreihenfolge ist immer auf die Unicode-Codepunktsortierreichenfolge festgelegt.  
  
## <a name="dynamic-context"></a>Dynamischer Kontext  
 Unter dem dynamischen Kontext werden die Informationen verstanden, die zum Zeitpunkt der Ausführung des Ausdrucks verfügbar sein müssen. Zusätzlich zum statischen Kontext werden die folgenden Informationen als Bestandteil des dynamischen Kontexts initialisiert:  
  
-   Der Schwerpunkt des Ausdrucks, z. B. das Kontext-Item, die Kontextposition und die Kontextgröße, werden entsprechend dem folgenden Beispiel initialisiert. Beachten Sie, dass alle diese Werte von der [Nodes ()-Methode](../t-sql/xml/nodes-method-xml-data-type.md)überschrieben werden können.  
  
    -   Der **XML** -Datentyp legt das Kontext Element, den Knoten, der verarbeitet wird, auf den Dokument Knoten fest.  
  
    -   Die Kontextposition, also die Position des Kontext-Items im Verhältnis zu den gerade verarbeiteten Knoten, wird auf 1 festgelegt.  
  
    -   Die Kontextgröße, also die Anzahl der Items in der Sequenz, die gerade verarbeitet wird, wird zunächst auf 1 festgelegt, weil es immer einen Dokumentknoten gibt.  
  
### <a name="implementation-restrictions"></a>Einschränkungen für die Implementierung  
 Im Zusammenhang mit dem dynamischen Kontext gelten die folgenden Einschränkungen:  
  
-   Die **aktuellen Datums-und Uhrzeit** Kontextfunktionen, **FN: Current-Date**, **FN: Current-Time** und **FN: Current-DateTime**, werden nicht unterstützt.  
  
-   Die **implizite Zeitzone** ist auf "UTC + 0" gesetzt und kann nicht geändert werden.  
  
-   Die **FN: doc ()** -Funktion wird nicht unterstützt. Alle Abfragen werden für Spalten oder Variablen vom Typ **XML** ausgeführt.  
  
-   Die **FN: Collection ()** -Funktion wird nicht unterstützt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Grundlagen](../xquery/xquery-basics.md)   
 [Vergleichen von typisiertem XML mit nicht typisiertem XML](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [XML-Schemaauflistungen &#40;SQL Server&#41;](../relational-databases/xml/xml-schema-collections-sql-server.md)  
  
  
