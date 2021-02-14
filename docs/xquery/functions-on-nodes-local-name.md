---
title: local-name-Funktion (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die XQuery-Funktion local-Name () verwenden, um den lokalen Namensteil eines Knotens zurückzugeben.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:local-name function
- local-name function
ms.assetid: c901ef5d-89c5-482a-bf64-3eefbcf3098d
author: rothja
ms.author: jroth
ms.openlocfilehash: 9d336db0ac3a6f9c490e3b25a9bee98e1228594f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353295"
---
# <a name="functions-on-nodes---local-name"></a>Funktionen für Knoten – local-name
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Gibt den lokalen Teil des Namens von *$arg* als xs: String zurück, der entweder die Zeichenfolge der Länge 0 (null) ist oder die lexikalische Form eines xs: NCName-Typs aufweist. Wenn das Argument nicht bereitgestellt wird, ist der Standardwert der Kontextknoten.  
  
## <a name="syntax"></a>Syntax  
  
```  
fn:local-name() as xs:string  
fn:local-name($arg as node()?) as xs:string  
```  
  
## <a name="arguments"></a>Argumente  
 *$arg*  
 Knotenname, dessen lokaler Namensanteil abgerufen wird.  
  
## <a name="remarks"></a>Bemerkungen  
  
-   In SQL Server kann **FN: local-Name ()** ohne Argument nur im Kontext eines kontextabhängigen Prädikats verwendet werden. Insbesondere kann die Funktion nur innerhalb von Klammern (`[ ]`) verwendet werden.  
  
-   Wenn das Argument angegeben wird und wenn es die leere Sequenz ist, gibt die Funktion die Zeichenfolge mit der Länge null zurück.  
  
-   Wenn der Zielknoten keinen Namen besitzt, weil es sich um einen Dokumentknoten, einen Kommentar oder einen Textknoten handelt, gibt die Funktion die Zeichenfolge mit der Länge null zurück.  
  
## <a name="examples"></a>Beispiele  
 Dieses Thema stellt XQuery-Beispiele für XML-Instanzen bereit, die in verschiedenen Spalten vom Typ **XML** in der AdventureWorks-Datenbank gespeichert sind.  
  
### <a name="a-retrieve-local-name-of-a-specific-node"></a>A. Abrufen des lokalen Namens eines bestimmten Knotens  
 Die folgende Abfrage wird für eine nicht typisierte XML-Instanz angegeben. Der Abfrageausdruck `local-name(/ROOT[1])` ruft den lokalen Namensanteil des angegebenen Knotens ab.  
  
```  
declare @x xml  
set @x='<ROOT><a>111</a></ROOT>'  
SELECT @x.query('local-name(/ROOT[1])')  
-- result = ROOT  
```  
  
 Die folgende Abfrage wird für die Instructions-Spalte, eine typisierte xml-Spalte, der ProductModel-Tabelle angegeben. Der Ausdruck `local-name(/AWMI:root[1]/AWMI:Location[1])` gibt den lokalen Namen (`Location`) des angegebenen Knotens zurück.  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     local-name(/AWMI:root[1]/AWMI:Location[1])') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
-- result = Location  
```  
  
### <a name="b-using-local-name-without-argument-in-a-predicate"></a>B. Verwenden von local-name ohne Argument in einem Prädikat  
 Die folgende Abfrage wird für die Instructions-Spalte (typisierte **XML** -Spalte) der ProductModel-Tabelle angegeben. Der Ausdruck gibt alle untergeordneten Elemente des <`root`> Elements zurück, dessen lokaler Namensteil des QName "Location" ist. Die **local-Name ()-** Funktion wird im Prädikat angegeben, und Sie hat keine Argumente, und der Kontext Knoten wird von der Funktion verwendet.  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
  /AWMI:root//*[local-name() = "Location"]') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Die Abfrage gibt alle <> untergeordneten `Location` Elementen des <`root`> Elements zurück.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Funktionen auf Knoten](./xquery-functions-against-the-xml-data-type.md)   
 [Namespace-URI-Funktion &#40;XQuery&#41;](../xquery/functions-on-nodes-namespace-uri.md)  
  
