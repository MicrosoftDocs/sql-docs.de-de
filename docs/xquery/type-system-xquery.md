---
title: Typsystem (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie mehr über das XQuery-Typsystem, das integrierte Typen von XML-Schemas und Typen enthält, die im xpath-datatypes-Namespace definiert sind.
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sequence [XQuery]
- type system [XQuery]
- typed values [XQuery]
- XQuery, sequence
- string values [XQuery]
- XQuery, XPath data type namespace
- xdt prefix [XML in SQL Server]
- XQuery, type system
- built-in XML schema types [SQL Server]
- xs prefix [XML in SQL Server]
ms.assetid: 22d6f861-d058-47ee-b550-cbe9092dcb12
author: rothja
ms.author: jroth
ms.openlocfilehash: a92b26642f5759044233d5d63b5a432ee5fe426c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352842"
---
# <a name="type-system-xquery"></a>Typensystem (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  XQuery ist eine stark typisierte Sprache für Schema Typen und eine schwach typisierte Sprache für nicht typisierte Daten. Zu den vordefinierten Typen von XQuery zählen folgende Typen:  
  
-   Integrierte XML-Schema Typen im- **http://www.w3.org/2001/XMLSchema** Namespace.  
  
-   Typen, die im- **http://www.w3.org/2004/07/xpath-datatypes** Namespace definiert sind.  
  
 In diesem Thema wird auch Folgendes beschrieben:  
  
-   Der Unterschied zwischen dem typisierten Wert und dem Zeichenfolgenwert eines Knotens.  
  
-   Die [Daten Funktion &#40;XQuery&#41;](../xquery/data-accessor-functions-data-xquery.md) und die [Zeichen folgen Funktion &#40;XQuery&#41;](../xquery/data-accessor-functions-string-xquery.md).  
  
-   Zuordnen des von einem Ausdruck zurückgegebenen Sequenztyps  
  
## <a name="built-in-types-of-xml-schema"></a>Integrierte Typen des XML-Schemas  
 Die integrierten Typen des XML-Schemas besitzen das vordefinierte Namespacepräfix xs. Zu diesen Typen gehören **xs: Integer** und **xs: String**. Alle diese integrierten Typen werden unterstützt. Sie können diese Typen verwenden, wenn Sie eine XML-Schemaauflistung erstellen.  
  
 Beim Abfragen von typisiertem XML-Code wird der statische und dynamische Typ der Knoten durch die XML-Schemaauflistung bestimmt, die der abgefragten Spalte oder Variablen zugeordnet ist. Weitere Informationen zu statischen und dynamischen Typen finden Sie unter [Ausdrucks Kontext und Abfrage Auswertung &#40;XQuery&#41;](../xquery/expression-context-and-query-evaluation-xquery.md). Beispielsweise wird die folgende Abfrage für eine typisierte **XML** -Spalte ( `Instructions` ) angegeben. Der Ausdruck verwendet `instance of`, um zu überprüfen, ob der typisierte Wert des zurückgegebenen `LotSize`-Attributs den `xs:decimal`-Typ aufweist.  
  
```  
SELECT Instructions.query('  
   DECLARE namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   data(/AWMI:root[1]/AWMI:Location[@LocationID=10][1]/@LotSize)[1] instance of xs:decimal  
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Diese Typisierungsinformationen werden durch die XML-Schemaauflistung bereitgestellt, die der Spalte zugeordnet sind.  
  
## <a name="types-defined-in-xpath-data-types-namespace"></a>Im Namespace für XPath-Datentypen definierte Typen  
 Die im- **http://www.w3.org/2004/07/xpath-datatypes** Namespace definierten Typen verfügen über ein vordefiniertes Präfix von **xdt**. Für diese Typen gilt Folgendes:  
  
-   Sie können diese Typen nicht verwenden, wenn Sie eine XML-Schemaauflistung erstellen. Diese Typen werden im XQuery-Typsystem verwendet und für die [XQuery-und statische Typisierung](../xquery/xquery-and-static-typing.md)verwendet. Sie können eine Umwandlung in die atomaren Typen, z. b. **xdt: untypedAtomic**, in den **xdt** -Namespace ausführen.  
  
-   Beim Abfragen von nicht typisiertem XML ist der statische und dynamische Typ von Elementknoten **xdt: untypisiert**, und der Typ der Attributwerte ist **xdt: untypedAtomic**. Das Ergebnis einer **Query ()** -Methode generiert nicht typisierten XML-Code. Dies bedeutet, dass die XML-Knoten als **xdt: untypisiert** und **xdt: untypedAtomic** zurückgegeben werden.  
  
-   Die **xdt: dayTimeDuration** -und **xdt: yearMonthDuration** -Typen werden nicht unterstützt.  
  
 Im folgenden Beispiel wird die Abfrage für eine nicht typisierte XML-Variable angegeben. Der Ausdruck `data(/a[1]`) gibt eine Sequenz eines atomaren Werts zurück. Die `data()`-Funktion gibt den typisierten Wert des `<a>`-Elements zurück. Da der abgefragte XML-Code nicht typisiert ist, ist der Typ des zurückgegebenen Werts `xdt:untypedAtomic`. Deshalb gibt `instance of` den Wert True zurück.  
  
```  
DECLARE @x xml  
SET @x='<a>20</a>'  
SELECT @x.query( 'data(/a[1]) instance of xdt:untypedAtomic' )  
```  
  
 Statt den typisierten Wert abzurufen, gibt der Ausdruck (`/a[1]`) im folgenden Beispiel eine Sequenz aus einem Element (dem `<a>`-Element) zurück. Der `instance of`-Ausdruck verwendet den Elementtest, um zu überprüfen, ob der vom Ausdruck zurückgegebene Wert ein `xdt:untyped type`-Elementknoten ist.  
  
```  
DECLARE @x xml  
SET @x='<a>20</a>'  
-- Is this an element node whose name is "a" and type is xdt:untyped.  
SELECT @x.query( '/a[1] instance of element(a, xdt:untyped?)')  
-- Is this an element node of type xdt:untyped.  
SELECT @x.query( '/a[1] instance of element(*, xdt:untyped?)')  
-- Is this an element node?  
SELECT @x.query( '/a[1] instance of element()')  
```  
  
> [!NOTE]  
>  Wenn Sie eine typisierte XML-Instanz abfragen und der Abfrageausdruck schließt die übergeordnete Achse ein, sind die Informationen zum statischen Typ der resultierenden Knoten nicht weiter verfügbar. Der dynamische Typ ist jedoch weiterhin den Knoten zugeordnet.  
  
## <a name="typed-value-vs-string-value"></a>Typisierter Wert im Vergleich zum Zeichenfolgenwert  
 Jeder Knoten besitzt einen typisierten Wert und einen Zeichenfolgenwert. Für typisierte XML-Daten wird der Typ des typisierten Werts durch die XML-Schemaauflistung bereitgestellt, die der abgefragten Spalte oder Variablen zugeordnet ist. Bei nicht typisierten XML-Daten ist der Typ des typisierten Werts **xdt: untypedAtomic**.  
  
 Sie können die **Data ()** -oder **String ()** -Funktion verwenden, um den Wert eines Knotens abzurufen:  
  
-   Die [Daten Funktion &#40;XQuery&#41;](../xquery/data-accessor-functions-data-xquery.md) den typisierten Wert eines Knotens zurückgibt.  
  
-   Die [Zeichen folgen Funktion &#40;XQuery&#41;](../xquery/data-accessor-functions-string-xquery.md) gibt den Zeichen folgen Wert des Knotens zurück.  
  
 In der folgenden XML-Schema Auflistung ist das <`root`>-Element des ganzzahligen Typs definiert:  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" type="integer"/>  
</schema>'  
GO  
```  
  
 Im folgenden Beispiel ruft der Ausdruck zuerst den typisierten Wert von `/root[1]` ab und fügt ihm anschließend `3` hinzu.  
  
```  
DECLARE @x xml(SC)  
SET @x='<root>5</root>'  
SELECT @x.query('data(/root[1]) + 3')  
```  
  
 Im nächsten Beispiel schlägt der Ausdruck fehl, weil die `string(/root[1])`-Anweisung im Ausdruck einen Wert des Zeichenfolgentyps zurückgibt. Dieser Wert wird dann an einen arithmetischen Operator übergeben, der nur Werte des numerischen Typs als Operand akzeptiert.  
  
```  
-- Fails because the argument is string type (must be numeric primitive type).  
DECLARE @x xml(SC)  
SET @x='<root>5</root>'  
SELECT @x.query('string(/root[1]) + 3')  
```  
  
 Im folgenden Beispiel wird der Gesamtwert der `LaborHours`-Attribute berechnet. Die- `data()` Funktion Ruft die typisierten Werte von `LaborHours` Attributen aus allen <`Location`>-Elementen für ein Produktmodell ab. Gemäß dem XML-Schema, das der `Instruction` Spalte zugeordnet `LaborHours` ist, ist vom Typ **xs: Decimal** .  
  
```  
SELECT Instructions.query('   
DECLARE namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
             sum(data(//AWMI:Location/@LaborHours))   
') AS Result   
FROM Production.ProductModel   
WHERE ProductModelID=7  
```  
  
 Diese Abfrage gibt 12.75 als Ergebnis zurück.  
  
> [!NOTE]  
>  Die explizite Verwendung der **Data ()** -Funktion in diesem Beispiel dient nur der Veranschaulichung. Wenn Sie nicht angegeben ist, wendet **Sum ()** implizit die **Data ()** -Funktion an, um die typisierten Werte der Knoten zu extrahieren.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Vorlagen und Berechtigungen in SQL Server Profiler](../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md)   
 [XQuery-Grundlagen](../xquery/xquery-basics.md)  
  
  
