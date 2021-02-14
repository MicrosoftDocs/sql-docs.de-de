---
title: Quantifizierte Ausdrücke (XQuery) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie quantifizierte Ausdrücke in XQuery verwenden, um eine existentielle oder universelle Quantifizierung auf einen Ausdruck über eine oder mehrere Sequenzen anzuwenden.
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
- universal quantifiers
- effective Boolean value [XQuery]
- quantified expressions [XQuery]
- XQuery, quantified expressions
- every expression [XQuery]
- existential quantifiers [XQuery]
- some expression [XQuery]
- EBV
- expressions [XQuery], quantifiers
ms.assetid: a3a75a6c-8f67-4923-8406-1ada546c817f
author: rothja
ms.author: jroth
ms.openlocfilehash: e673895553b6a3207d0ef86337c47e2f6def2bfe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339912"
---
# <a name="quantified-expressions-xquery"></a>Quantifizierte Ausdrücke (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Die Semantik für auf zwei Sequenzen angewendete boolesche Operatoren unterscheidet sich bei existenziellen und universellen Quantifizierern, wie in der folgenden Tabelle dargestellt.  
  
 *Existenzielle Quantifizierer*  
 Wenn in zwei Sequenzen ein beliebiges Element der ersten Sequenz infolge des verwendeten Vergleichsoperators mit einem Element in der zweiten Sequenz übereinstimmt, ist der zurückgegebene Wert True.  
  
 *Universelle Quantifizierer*  
 Wenn in zwei Sequenzen jedes Element der ersten Sequenz mit einem Element in der zweiten Sequenz übereinstimmt, ist der zurückgegebene Wert True.  
  
 XQuery unterstützt quantifizierte Ausdrücke in der folgenden Form:  
  
```  
( some | every ) <variable> in <Expression> (,...) satisfies <Expression>  
```  
  
 Sie können diese Ausdrücke in einer Abfrage verwenden, um entweder die existenzielle oder die universelle Quantifizierung über eine oder mehrere Sequenzen auf einen Ausdruck anzuwenden. In [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] muss der Ausdruck in der `satisfies`-Klausel entweder eine Knotensequenz oder eine leere Sequenz oder einen booleschen Wert ergeben. Der effektive boolesche Wert des Ergebnisses dieses Ausdrucks wird dann in der Quantifizierung verwendet. Die existentielle Quantifizierung, die **einige** verwendet, gibt true zurück, wenn mindestens einer der vom Quantifizierer gebundenen Werte im Ausdruck "Erfüllung" ein "true"-Ergebnis aufweist. Die universelle Quantifizierung, die **alle** verwendet, muss für alle vom Quantifizierer gebundenen Werte den Wert true aufweisen.  
  
 Die folgende Abfrage prüft z \<Location> . b. jedes Element, um zu sehen, ob es über ein LocationID-Attribut verfügt.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        if (every $WC in //AWMI:root/AWMI:Location   
            satisfies $WC/@LocationID)  
        then  
             <Result>All work centers have workcenterLocation ID</Result>  
         else  
             <Result>Not all work centers have workcenterLocation ID</Result>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Da LocationID ein erforderliches Attribut des- \<Location> Elements ist, erhalten Sie das erwartete Ergebnis:  
  
```  
<Result>All work centers have Location ID</Result>   
```  
  
 Anstatt die [Query ()-Methode](../t-sql/xml/query-method-xml-data-type.md)zu verwenden, können Sie die [value ()-Methode](../t-sql/xml/value-method-xml-data-type.md) verwenden, um das Ergebnis an die relationale Welt zurückzugeben, wie in der folgenden Abfrage gezeigt. Die Abfrage gibt True zurück, wenn alle Arbeitsplatzstandorte LocationID-Attribute besitzen. Anderenfalls gibt die Abfrage False zurück.  
  
```  
SELECT Instructions.value('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        every $WC in  //AWMI:root/AWMI:Location   
            satisfies $WC/@LocationID',   
  'nvarchar(10)') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Die folgende Abfrage überprüft, ob eine der Produktabbildungen im Kleinformat vorhanden ist. In den XML-Produktkatalogdaten sind für jedes Produkt Abbildungen aus verschiedenen Winkeln und in verschiedenen Größen gespeichert. Angenommen, Sie möchten sicherstellen, dass alle XML-Produktkatalogdaten mindestens eine Abbildung im Kleinformat enthalten. Dies erreichen Sie mit der folgenden Abfrage:  
  
```  
SELECT ProductModelID, CatalogDescription.value('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     some $F in /PD:ProductDescription/PD:Picture  
        satisfies $F/PD:Size="small"', 'nvarchar(20)') as SmallPicturesStored  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Dies ist ein Teilergebnis:  
  
```  
ProductModelID SmallPicturesStored   
-------------- --------------------  
19             true        
```  
  
## <a name="implementation-limitations"></a>Implementierungseinschränkungen  
 Die folgenden Einschränkungen sind zu beachten:  
  
-   Die Typassertion wird als Teil der Bindung der Variablen in quantifizierten Ausdrücken nicht unterstützt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Ausdrücke](../xquery/xquery-expressions.md)  
  
  
