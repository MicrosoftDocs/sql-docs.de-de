---
title: Typumwandlungs Regeln in XQuery | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Regeln, die angewendet werden, wenn Sie in XQuery explizit oder implizit von einem Datentyp in einen anderen Datentyp umwandeln.
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
- XQuery, type casting
- casting rules [XML in SQL Server]
- explicit casting [SQL Server]
- type casting rules [XML in SQL Server]
- cast as operator
- implicit casting
ms.assetid: f2e91306-2b1b-4e1c-b6d8-a34fb9980057
author: rothja
ms.author: jroth
ms.openlocfilehash: 0cd4a68b21ae23b60c2bea5618308c0d13300cab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353276"
---
# <a name="type-casting-rules-in-xquery"></a>Typumwandlungsregeln in XQuery
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Das folgende Spezifikationsdiagramm für Funktionen und Operatoren von W3C XQuery 1.0 und XPath 2.0 zeigt die integrierten Datentypen. Dies umfasst die integrierten Grundtypen und abgeleiteten Typen.  
  
 ![XQuery 1.0-Typhierarchie](../xquery/media/xquery-typing-rules.gif "XQuery 1.0-Typhierarchie")  
  
 In diesem Thema werden die Regeln für die Typumwandlung beschrieben, die beim Umwandeln von einem Typ in einen anderen mithilfe der folgenden Methoden angewendet werden:  
  
-   Explizite Umwandlung, die Sie mit **cast as** oder den typkonstruktorfunktionen ausführen (z. b `xs:integer("5")` .).  
  
-   Implizite Umwandlung, die während der Typhöherstufung auftritt  
  
## <a name="explicit-casting"></a>Explizite Umwandlung  
 Die folgende Tabelle erläutert die zulässige Typumwandlung zwischen den integrierten Grundtypen.  
  
 ![Umwandlungsregeln für XQuery](../xquery/media/casting-builtin-types.gif "Umwandlungsregeln für XQuery")  
  
-   Ein integrierter Grundtyp kann basierend auf den in der Tabelle aufgeführten Regeln in einen anderen integrierten Grundtyp umgewandelt werden.  
  
-   Ein Grundtyp kann in jeden beliebigen Typ umgewandelt werden, der von diesem betreffenden Grundtyp abgeleitet ist. Sie können z. b. von **xs: Decimal** in **xs: Integer** oder von **xs: Decimal** in **xs: Long** umwandeln.  
  
-   Ein abgeleiteter Typ kann in einen beliebigen Typ umgewandelt werden, der sein Vorgänger in der Typhierarchie ist (bis hinauf zu seinem integrierten Grundtyp). Sie können z. b. eine Umwandlung von **xs: Token** in **xs: normalizedString** oder **xs: String** durchsetzen.  
  
-   Ein abgeleiteter Typ kann in einen Grundtyp umgewandelt werden, wenn sein primitiver Vorgänger in den Zieltyp umgewandelt werden kann. Beispielsweise können Sie **xs: Integer**, einen abgeleiteten Typ, in einen primitiven **xs: String**-Typ umwandeln, da der primitive Vorgänger von **xs: Decimal**, **xs: Integer** in **xs: String** umgewandelt werden kann.  
  
-   Ein abgeleiteter Typ kann in einen anderen abgeleiteten Typ umgewandelt werden, wenn der primitive Vorgänger des Quelltyps in den primitiven Vorgänger des Zieltyps umgewandelt werden kann. Sie können z. b. von **xs: Integer** in **xs: Token** umwandeln, da Sie von **xs: Decimal** in **xs: String** umwandeln können.  
  
-   Die Regeln zum Umwandeln benutzerdefinierter Typen in integrierte Typen sind die gleichen wie für die integrierten Datentypen. Beispielsweise können Sie einen **myInteger** -Typ definieren, der vom **xs: Integer** -Typ abgeleitet wird. Anschließend kann **myInteger** in **xs: Token** umgewandelt werden, da **xs: Decimal** in **xs: String** umgewandelt werden kann.  
  
 Die folgenden Datentypumwandlungsarten werden nicht unterstützt:  
  
-   Die Typumwandlung in oder aus Listentypen ist nicht zulässig. Dies schließt sowohl benutzerdefinierte Listen Typen als auch integrierte Listen Typen wie **xs: IDREFS**, **xs: Entities** und **xs: NMTOKENS** ein.  
  
-   Das Umwandeln in oder aus **xs: QName** wird nicht unterstützt.  
  
-   **xs: Notation** und die vollständig geordneten Untertypen von Duration, **xdt: yearMonthDuration** und **xdt: dayTimeDuration**, werden nicht unterstützt. Daher ist auch die Typumwandlung in oder aus diese(n) Datentypen nicht zulässig.  
  
 Die folgenden Beispiele veranschaulichen die explizite Typumwandlung.  
  
### <a name="example-a"></a>Beispiel A  
 Das folgende Beispiel fragt eine Variable vom Typ xml ab. Die Abfrage gibt eine Sequenz eines Werts vom simple-Typ zurück, der als xs:string typisiert ist.  
  
```  
declare @x xml  
set @x = '<e>1</e><e>2</e>'  
select @x.query('/e[1] cast as xs:string?')  
go  
```  
  
### <a name="example-b"></a>Beispiel B  
 Das folgende Beispiel fragt eine typisierte xml-Variable ab. Das Beispiel erstellt zuerst eine XML-Schemaauflistung. Anschließend wird die XML-Schemaauflistung zum Erstellen einer typisierten xml-Variablen verwendet. Das Schema stellt die Typisierungsinformationen für die XML-Instanz bereit, die der Variablen zugeordnet ist. Anschließend werden Abfragen für die Variable angegeben.  
  
```  
create xml schema collection myCollection as N'  
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">  
      <xs:element name="root">  
            <xs:complexType>  
                  <xs:sequence>  
                        <xs:element name="A" type="xs:string"/>  
                        <xs:element name="B" type="xs:string"/>  
                        <xs:element name="C" type="xs:string"/>  
                  </xs:sequence>  
            </xs:complexType>  
      </xs:element>  
</xs:schema>'  
go  
```  
  
 Die folgende Abfrage gibt einen statischen Fehler zurück, da Sie nicht wissen, wie viele <`root`> Elemente der obersten Ebene in der Dokument Instanz enthalten sind.  
  
```  
declare @x xml(myCollection)  
set @x = '<root><A>1</A><B>2</B><C>3</C></root>  
          <root><A>4</A><B>5</B><C>6</baz></C>'  
select @x.query('/root/A cast as xs:string?')  
go  
```  
  
 Durch Angeben eines Singleton-<`root`>-Elements im Ausdruck ist die Abfrage erfolgreich. Die Abfrage gibt eine Sequenz eines Werts vom simple-Typ zurück, der als xs:string typisiert ist.  
  
```  
declare @x xml(myCollection)  
set @x = '<root><A>1</A><B>2</B><C>3</C></root>  
              <root><A>4</A><B>5</B><C>6</C></root>'  
select @x.query('/root[1]/A cast as xs:string?')  
go  
```  
  
 Im folgenden Beispiel enthält die Variable vom Typ xml ein document-Schlüsselwort, das die XML-Schemaauflistung angibt. Dies zeigt an, dass es sich bei der XML-Instanz um ein Dokument handeln muss, das ein einziges Element der obersten Ebene besitzt. Wenn Sie zwei <`root`> Elemente in der XML-Instanz erstellen, wird ein Fehler zurückgegeben.  
  
```  
declare @x xml(document myCollection)  
set @x = '<root><A>1</A><B>2</B><C>3</C></root>  
              <root><A>4</A><B>5</B><C>6</C></root>'  
go  
```  
  
 Sie können die Instanz so ändern, dass nur ein Element der obersten Ebene eingeschlossen wird. Die Abfrage funktioniert dann. Die Abfrage gibt erneut eine Sequenz eines Werts vom simple-Typ zurück, der als xs:string typisiert ist.  
  
```  
declare @x xml(document myCollection)  
set @x = '<root><A>1</A><B>2</B><C>3</C></root>'  
select @x.query('/root/A cast as xs:string?')  
go  
```  
  
## <a name="implicit-casting"></a>Implizite Typumwandlung  
 Implizite Typumwandlung ist nur für numerische Datentypen und nicht typisierte atomare Datentypen zulässig. Beispielsweise gibt die folgende **Min ()** -Funktion den minimalen der beiden Werte zurück:  
  
```  
min(xs:integer("1"), xs:double("1.1"))  
```  
  
 In diesem Beispiel sind die beiden Werte, die an die XQuery **Min ()** -Funktion übermittelt werden, von unterschiedlichen Typen. Daher wird eine implizite Konvertierung durchgeführt, bei der der **ganzzahlige** Typ zu **Double** herauf gestuft wird und die beiden **doppelten** Werte verglichen werden.  
  
 Die Typhöherstufung, die in diesem Beispiel gezeigt wurde, unterliegt den folgenden Regeln:  
  
-   Ein integrierter abgeleiteter numerischer Datentyp kann auf seinen Basistyp heraufgestuft werden. Beispielsweise kann **Integer** zu **Decimal** herauf gestuft werden.  
  
-   Ein **Dezimal** Trennzeichen kann auf **float** herauf gestuft werden, und ein **float** -Wert kann auf **Double** herauf gestuft werden.  
  
 Da implizite Typumwandlung nur für numerische Datentypen zulässig ist, sind die folgenden Vorgänge nicht zulässig.  
  
-   Die implizite Typumwandlung für Zeichenfolgen-Datentypen ist nicht zulässig. Wenn z. b. zwei **Zeichen** folgen Typen erwartet werden und Sie eine **Zeichenfolge** und ein **Token** übergeben, erfolgt keine implizite Umwandlung, und es wird ein Fehler zurückgegeben.  
  
-   Die implizite Typumwandlung aus numerischen Datentypen in Zeichenfolgen-Datentypen ist nicht zulässig. Wenn Sie z. B. einen Wert vom Typ integer an eine Funktion übergeben, die einen Parameter vom string-Typ erwartet, tritt keine implizite Typumwandlung auf, und es wird ein Fehler zurückgegeben.  
  
## <a name="casting-values"></a>Typumwandlung von Werten  
 Wenn eine Umwandlung von einem Datentyp in einen anderen erfolgt, werden die Istwerte aus dem Wertebereich des Quelltyps in den Wertebereich des Zieltyps transformiert. Die Typumwandlung aus einem xs:decimal- in einen xs:double-Typ transformiert z. B. den decimal-Wert in einen double-Wert.  
  
 Im Folgenden finden Sie einige der Transformationsregeln.  
  
##### <a name="casting-a-value-from-a-string-or-untypedatomic-type"></a>Umwandeln eines Werts aus einem string- oder untypedAtomic-Typ  
 Der Wert, der in einen string- oder untypedAtomic-Typ umgewandelt wird, wird auf die gleiche Weise wie beim Überprüfen des Werts basierend auf den Regeln des Zieltyps transformiert. Dies schließt ggf. Muster- und Leerzeichenverarbeitungsregeln ein. Die folgende Umwandlung ist z. B. erfolgreich und generiert einen double-Wert (1.1e0):  
  
 `xs:double("1.1")`  
  
 Bei der Umwandlung in binäre Typen wie z. B. xs:base64Binary oder xs:hexBinary aus einem string- oder untypedAtomic-Typ müssen die Eingabewerte base64- bzw. hexadezimal codiert sein.  
  
##### <a name="casting-a-value-to-a-string-or-untypedatomic-type"></a>Umwandeln eines Werts in einen string- oder untypedAtomic-Typ  
 Bei der Umwandlung eines string- oder untypedAtomic-Typs wird der Wert in die kanonische lexikalische Darstellung von XQuery transformiert. Dies kann insbesondere bedeuten, dass ein Wert, der während der Eingabe einem bestimmten Muster oder einer anderen Einschränkung unterworfen war, nicht gemäß dieser Einschränkung dargestellt wird.  Um Benutzer darüber zu informieren, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] werden Typen, bei denen die Typeinschränkung problematisch sein kann, durch Bereitstellen einer Warnung angezeigt, wenn diese Typen in die Schema Auflistung geladen werden.  
  
 Wenn ein Wert vom Typ xs:float oder xs:double oder einer ihrer Untertypen in einen string- oder untypedAtomic-Typ umgewandelt wird, wird der Wert in wissenschaftlicher Schreibweise dargestellt. Dies geschieht nur, wenn der Absolutwert des Werts kleiner als 1.0E-6 oder größer oder gleich 1.0E6 ist. Dies bedeutet, dass 0 in wissenschaftlicher Schreibweise zu 0.0E0 serialisiert wird.  
  
 Beispielsweise gibt `xs:string(1.11e1)` den Zeichenfolgenwert `"11.1"` zurück, während `xs:string(-0.00000000002e0)` den Zeichenfolgenwert `"-2.0E-11"` zurückgibt.  
  
 Bei der Umwandlung binärer Typen wie z. B. xs:base64Binary oder xs:hexBinary in einen string- oder untypedAtomic-Typ werden die Binärwerte in base64- bzw. hexadezimal codierter Form dargestellt.  
  
##### <a name="casting-a-value-to-a-numeric-type"></a>Umwandeln eines Werts in einen numeric-Typ  
 Wenn ein Wert eines numeric-Typs in einen Wert eines anderen numeric-Typs umgewandelt wird, wird der Wert aus einem Wertebereich dem anderen Wertebereich zugeordnet, ohne die Zeichenfolgenserialisierung zu durchlaufen. Wenn der Wert nicht der Einschränkung eines Zieltyps genügt, gelten die folgenden Regeln:  
  
-   Wenn der Quellwert bereits vom Typ numeric und der Zieltyp xs:float oder ein Untertyp davon ist, der -INF- oder INF-Werte zulässt, und die Typumwandlung des numeric-Quellwerts zu einem Überlauf führen würde, wird der Wert INF zugeordnet, wenn er positiv ist, oder -INF, wenn der Wert negativ ist. Wenn der Zieltyp keine INF- oder -INF-Werte zulässt und ein Überlauf auftreten würde, schlägt die Typumwandlung fehl. Das Ergebnis ist in dieser Version von SQL Server die leere Sequenz.  
  
-   Wenn der Quellwert bereits vom Typ numeric und der Zieltyp ein numeric-Typ ist, der 0, -0e0 oder 0e0 im zulässigen Wertebereich enthält, und die Typumwandlung des numeric-Quellwerts einen Unterlauf verursachen würde, wird der Wert auf die folgende Weise zugeordnet:  
  
    -   Der Wert wird für einen decimal-Zieltyp 0 zugeordnet.  
  
    -   Der Wert wird -0e0 zugeordnet, wenn der Wert einen negativen Unterlauf verursacht.  
  
    -   Der Wert wird 0e0 zugeordnet, wenn der Wert einen negativen Unterlauf für einen float- oder double-Zieltyp verursacht.  
  
     Die Typumwandlung schlägt fehl, wenn der Zieltyp nicht Null in seinem Wertebereich enthält; das Ergebnis ist die leere Sequenz.  
  
     Beachten Sie, dass durch die Umwandlung eines Werts in einen binären Gleitkommawert wie z. B. xs:float, xs:double oder einen der jeweiligen Untertypen die Genauigkeit beeinträchtigt werden kann.  
  
## <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Der Gleitkommawert NaN wird nicht unterstützt.  
  
-   Umwandelbare Werte werden durch die Implementierungsbeschränkungen der Zieltypen eingeschränkt. Beispielsweise ist es nicht möglich, eine Datums Zeichenfolge mit einem negativen Jahr in **xs: Date** umzuwandeln. Solche Umwandlungen führen zu einer leeren Sequenz, wenn der Wert zur Laufzeit bereitgestellt wird (statt einen Laufzeitfehler auszulösen).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Definieren der Serialisierung von XML-Daten](../relational-databases/xml/define-the-serialization-of-xml-data.md)  
  
  
