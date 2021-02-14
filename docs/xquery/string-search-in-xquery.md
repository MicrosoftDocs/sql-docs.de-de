---
title: Zeichen folgen Suche in XQuery | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Text in XML-Dokumenten durchsuchen, indem Sie ein Beispiel für die Zeichen folgen Suche in XQuery anzeigen.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- strings [SQL Server], search
- XML [SQL Server], searching text
- searches [SQL Server], XML documents
- XQuery, string search
ms.assetid: edc62024-4c4c-4970-b5fa-2e54a5aca631
author: rothja
ms.author: jroth
ms.openlocfilehash: 2064840d8e5418466c976b3c3d4b2613f1be0708
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335898"
---
# <a name="string-search-in-xquery"></a>Zeichenfolgensuche in XQuery
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  In diesem Thema werden Beispielabfragen bereitgestellt, die zeigen, wie in XML-Dokumenten nach Text gesucht werden kann.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-find-feature-descriptions-that-contain-the-word-maintenance-in-the-product-catalog"></a>A. Suchen von Funktionsbeschreibungen mit dem Wort "maintenance" im Produktkatalog  
  
```  
SELECT CatalogDescription.query('  
     declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
    for $f in /p1:ProductDescription/p1:Features/*  
     where contains(string($f), "maintenance")  
     return  
           $f ') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 In der vorherigen Abfrage `where` filtert der im FLOWR-Ausdruck das Ergebnis des `for` Ausdrucks und gibt nur die Elemente zurück, die die **enthält ()** -Bedingung erfüllen.  
  
 Dies ist das Ergebnis:  
  
```  
<p1:Maintenance     
      xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
 <p1:NoOfYears>10</p1:NoOfYears>  
 <p1:Description>maintenance contact available through your   
               dealer or any AdventureWorks retail store.</p1:Description>  
</p1:Maintenance>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Daten &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [XQuery-Sprachreferenz &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)  
  
  
