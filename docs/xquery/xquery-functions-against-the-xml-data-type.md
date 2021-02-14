---
title: XQuery-Funktionen für den XML-Datentyp | Microsoft-Dokumentation
description: Erfahren Sie mehr über die XQuery-Funktionen, die für die Verwendung mit dem XML-Datentyp unterstützt werden.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, functions
- xml data type [SQL Server], XQuery
- functions [SQL Server], XQuery
ms.assetid: 8df0877d-a03f-4ca9-b84e-908c4bb42b5e
author: rothja
ms.author: jroth
ms.openlocfilehash: 82f3eae7098b3ac5e86355aed1e13bdd5c137015
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352811"
---
# <a name="xquery-functions-against-the-xml-data-type"></a>XQuery-Funktionen für den xml-Datentyp
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  In diesem Thema und seinen Unterthemen werden die Funktionen beschrieben, die Sie verwenden können, wenn Sie XQuery für den **XML** -Datentyp angeben. Informationen zu den W3C-Spezifikationen finden Sie unter [http://www.w3.org/TR/2004/WD-xpath-functions-20040723](https://go.microsoft.com/fwlink/?LinkId=4873) .  
  
 Die XQuery-Funktionen gehören zum- http://www.w3.org/2004/07/xpath-functions Namespace. Die W3C-Spezifikationen verwenden das "fn:"-Namespacepräfix, um diese Funktionen zu beschreiben. Sie brauchen das "fn:"-Namespacepräfix nicht explizit anzugeben, wenn Sie die Funktionen verwenden. Daher, sowie auch zur besseren Lesbarkeit, werden die Namespacepräfixe in dieser Dokumentation im Allgemeinen weggelassen.  
  
 In der folgenden Tabelle sind die XQuery-Funktionen aufgelistet, die für den **XML**-Datentyp unterstützt werden.  
  
|Category|Funktionsname|  
|--------------|-------------------|  
|[Funktionen für numerische Werte]()|[Grenze](../xquery/numeric-values-functions-ceiling.md)|  
||[floor](../xquery/numeric-values-functions-floor.md)|  
||[umgekehrt](../xquery/numeric-values-functions-round.md)|  
|[XQuery-Funktionen für Zeichenfolgenwerte]()|[concat](../xquery/functions-on-string-values-concat.md)|  
||[contains](../xquery/functions-on-string-values-contains.md)|  
||[substring](../xquery/functions-on-string-values-substring.md)|  
||[&#40;XQuery-&#41;für Kleinbuchstaben ](../xquery/functions-on-string-values-lower-case.md)|  
||[Zeichen folgen Länge](../xquery/functions-on-string-values-string-length.md)|  
||[&#40;XQuery-&#41;für Großbuchstaben ](../xquery/functions-on-string-values-upper-case.md)|  
|Funktionen für boolesche Werte|[not](../xquery/functions-on-boolean-values-not-function.md)|  
|[Funktionen auf Knoten]()|[Zahl](../xquery/functions-on-nodes-number.md)|  
||[local-name-Funktion (XQuery)](../xquery/functions-on-nodes-local-name.md)|  
||[namespace-uri-Funktion (XQuery)](../xquery/functions-on-nodes-namespace-uri.md)|  
|[Kontextfunktionen]()|[last](../xquery/context-functions-last-xquery.md)|  
||[position](../xquery/context-functions-position-xquery.md)|  
|[Funktionen für Sequenzen]()|[empty](../xquery/functions-on-sequences-empty.md)|  
||[distinct-values](../xquery/functions-on-sequences-distinct-values.md)|  
||[id-Funktion (XQuery)](../xquery/functions-on-sequences-id.md)|  
|[Aggregatfunktionen &#40;XQuery&#41;]()|[count](../xquery/aggregate-functions-count.md)|  
||[AVG](../xquery/aggregate-functions-avg.md)|  
||[min](../xquery/aggregate-functions-min.md)|  
||[max](../xquery/aggregate-functions-max.md)|  
||[sum](../xquery/aggregate-functions-sum.md)|  
|[Konstruktorfunktionen &#40;XQuery-&#41;](../xquery/constructor-functions-xquery.md)|[Konstruktorfunktionen](../xquery/constructor-functions-xquery.md)|  
|[Data Accessor-Funktionen](../xquery/data-accessor-functions.md)|[string](../xquery/data-accessor-functions-string-xquery.md)|  
||[data](../xquery/data-accessor-functions-data-xquery.md)|  
|[Boolesche Konstruktorfunktionen &#40;XQuery-&#41;]()|[true-Funktion (XQuery)](../xquery/boolean-constructor-functions-true-xquery.md)|  
||[false-Funktion (XQuery)](../xquery/boolean-constructor-functions-false-xquery.md)|  
|[Funktionen im Zusammenhang mit QNames &#40;XQuery-&#41;](./functions-related-to-qnames-expanded-qname.md)|[expanded-QName (XQuery)](../xquery/functions-related-to-qnames-expanded-qname.md)|  
||[local-name-from-QName (XQuery)](../xquery/functions-related-to-qnames-local-name-from-qname.md)|  
||[namespace-uri-from-QName (XQuery)](../xquery/functions-related-to-qnames-namespace-uri-from-qname.md)|  
|[XQuery-Erweiterungsfunktionen in SQL Server](./xquery-extension-functions-sql-column.md)|[SQL: column ()-Funktion (XQuery)](../xquery/xquery-extension-functions-sql-column.md)|  
||[sql:variable()-Funktion (XQuery)](../xquery/xquery-extension-functions-sql-variable.md)|  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Datentypmethoden](../t-sql/xml/xml-data-type-methods.md)   
 [XQuery-Sprachreferenz &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [XML-Daten &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)  
  
