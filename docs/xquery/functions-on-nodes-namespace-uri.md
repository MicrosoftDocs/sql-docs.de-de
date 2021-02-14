---
title: Namespace-URI-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Namespace-URI-Funktion in einer XQuery verwenden, um den Namespace-URI eines angegebenen QName zurückzugeben.
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
- fn:namespace-uri function
- namespace-uri function
ms.assetid: 9b48d216-26c8-431d-9ab4-20ab187917f4
author: rothja
ms.author: jroth
ms.openlocfilehash: 4a9734dd4efd2f4dcedd539218a0a7d989211749
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353285"
---
# <a name="functions-on-nodes---namespace-uri"></a>Funktionen für Knoten – namespace-uri
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Gibt den Namespace-URI des in *$arg* angegebenen QName als xs: String zurück.  
  
## <a name="syntax"></a>Syntax  
  
```  
fn:namespace-uri() as xs:string  
fn:namespace-uri($arg as node()?) as xs:string  
```  
  
## <a name="arguments"></a>Argumente  
 *$arg*  
 Knotenname, dessen Namespace-URI-Teil abgerufen wird.  
  
## <a name="remarks"></a>Bemerkungen  
  
-   Wird das Argument nicht angegeben, wird standardmäßig der Kontextknoten verwendet.  
  
-   In SQL Server kann **FN: Namespace-URI ()** ohne Argument nur im Kontext eines kontextabhängigen Prädikats verwendet werden. Insbesondere kann die Funktion nur innerhalb von Klammern ([ ]) verwendet werden.  
  
-   Wenn *$arg* die leere Sequenz ist, wird die Zeichenfolge der Länge 0 (null) zurückgegeben.  
  
-   Wenn *$arg* ein Element-oder Attribut Knoten ist, dessen erweiterter QName sich nicht in einem Namespace befindet, gibt die Funktion die Zeichenfolge mit der Länge 0 (null) zurück.  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der AdventureWorks-Datenbank gespeichert sind.  
  
### <a name="a-retrieve-namespace-uri-of-a-specific-node"></a>A. Abrufen eines Namespace-URI eines bestimmten Knotens  
 Die folgende Abfrage wird für eine nicht typisierte XML-Instanz angegeben. Der Abfrageausdruck `namespace-uri(/ROOT[1])` ruft den Namespace-URI-Teil des angegebenen Knotens ab.  
  
```  
set @x='<ROOT><a>111</a></ROOT>'  
SELECT @x.query('namespace-uri(/ROOT[1])')  
```  
  
 Da der angegebene QName nicht über den URI-Teil des Namespace verfügt, sondern lediglich über den lokalen Teil des Namens, ist das Ergebnis eine leere Zeichenfolge.  
  
 Die folgende Abfrage wird für die Anweisungen typisierte **XML** -Spalte angegeben. Der Ausdruck `namespace-uri(/AWMI:root[1]/AWMI:Location[1])` gibt den Namespace-URI des ersten <>- `Location` Elements zurück, das dem <`root`> Element untergeordnet ist.  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     namespace-uri(/AWMI:root[1]/AWMI:Location[1])') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Dies ist das Ergebnis:  
  
```  
https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions  
```  
  
### <a name="b-using-namespace-uri-without-argument-in-a-predicate"></a>B. Verwenden von namespace-uri() ohne Argument in einem Prädikat  
 Die folgende Abfrage wird für die CatalogDescription-Spalte vom Typ XML angegeben. Der Ausdruck gibt alle Elementknoten zurück, deren Namespace-URI `https://www.adventure-works.com/schemas/OtherFeatures` ist. Die Namespace-**URI ()** -Funktion wird ohne Argument angegeben und verwendet den Kontext Knoten.  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
   /p1:ProductDescription//*[namespace-uri() = "https://www.adventure-works.com/schemas/OtherFeatures"]  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Dies ist ein Teilergebnis:  
  
```  
<p1:wheel xmlns:p1="https://www.adventure-works.com/schemas/OtherFeatures">High performance wheels.</p1:wheel>  
<p2:saddle xmlns:p2="https://www.adventure-works.com/schemas/OtherFeatures">  
  <p3:i xmlns:p3="http://www.w3.org/1999/xhtml">Anatomic design</p3:i> and made from durable leather for a full-day of riding in comfort.</p2:saddle>  
...  
```  
  
 Sie können den Namespace-URI in der vorherigen Abfrage zu `https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain` ändern. Sie erhalten dann alle untergeordneten Elementknoten des <>- `ProductDescription` Elements, dessen Namespace-URI-Teil des erweiterten QName ist `https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain` .  
  
### <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Die **Namespace-URI ()-** Funktion gibt Instanzen vom Typ xs: String anstelle von xs: anyURI zurück.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Funktionen auf Knoten](./xquery-functions-against-the-xml-data-type.md)   
 [local-name-Funktion &#40;XQuery-&#41;](../xquery/functions-on-nodes-local-name.md)  
  
