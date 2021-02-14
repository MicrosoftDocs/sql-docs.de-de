---
title: Datenaccessorfunktionen | Microsoft-Dokumentation
description: 'Erfahren Sie, wie Sie die XQuery-Daten Accessor-Funktionen FN: data (), FN: String () und Text () verwenden.'
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
- data-accessor functions [XQuery]
ms.assetid: 31bad04f-7c74-4773-9f83-612704fdd21c
author: rothja
ms.author: jroth
ms.openlocfilehash: 53ce941c88e2ab551ac0787126e2cf981d353819
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351070"
---
# <a name="data-accessor-functions"></a>Data Accessor-Funktionen
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Die Themen in diesem Abschnitt behandeln die Datenaccessorfunktionen und stellen entsprechenden Beispielcode bereit.  
  
## <a name="understanding-fndata-fnstring-and-text"></a>Grundlegendes zu 'fn:data()', 'fn:string()' und text()  
 XQuery weist eine Funktion **FN: data ()** auf, um skalare, typisierte Werte von Knoten, einen Knoten **Testtext ()** zum Zurückgeben von Textknoten und die Funktion **FN: String ()** zu extrahieren, die den Zeichen folgen Wert eines Knotens zurückgibt. Ihre Verwendung kann verwirrend sein. Im Folgenden finden Sie Richtlinien zu ihrer ordnungsgemäßen Verwendung in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. \<age> \</age> Zum Zweck der Veranschaulichung wird die XML-Instanz 12 verwendet.  
  
-   Nicht typisiertes XML: Der Pfadausdruck /age/text() gibt den Textknoten 12 zurück. Die Funktion fn:data(/age) gibt den Zeichenfolgenwert 12 zurück, was auch für fn:string(/age) gilt.  
  
-   Typisiertes XML: der Ausdruck/age/Text () gibt einen statischen Fehler für ein beliebiges einfaches typisiertes \<age> Element zurück. Dagegen gibt fn:data(/age) die ganze Zahl 12 zurück. fn:string(/age) führt zur Zeichenfolge 12.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
-   [Zeichen folgen Funktion &#40;XQuery-&#41;](../xquery/data-accessor-functions-string-xquery.md)  
  
-   [&#40;XQuery-Daten Funktions&#41;](../xquery/data-accessor-functions-data-xquery.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Path-Ausdrücke &#40;XQuery-&#41;](../xquery/path-expressions-xquery.md)  
  
  
