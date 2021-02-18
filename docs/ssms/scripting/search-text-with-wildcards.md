---
title: Suchen von Text mit Platzhaltern
description: Hier erfahren Sie, wie Sie Platzhalter im Feld „Suchen nach“ des Dialogfelds „Suchen und ersetzen“ verwenden, um ein Muster anzugeben, das abgeglichen werden soll.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- vswildcardsbuilder
helpviewer_keywords:
- searches [SQL Server Management Studio], wildcards
- Query Editor [SQL Server Management Studio], wildcard searches
- wildcard options [SQL Server Management Studio]
ms.assetid: 449600f8-cc87-4b3f-878a-59c158a88a40
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 670b276a54796862b68b7aa49aef5145c508b317
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344165"
---
# <a name="search-text-with-wildcards"></a>Suchen von Text mit Platzhaltern
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Mit den folgenden Ausdrücken lassen sich Zeichen oder Ziffern im Feld **Suchen nach** im Dialogfeld **Suchen und Ersetzen** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ersetzen.  
  
#### <a name="to-search-using-wildcards"></a>So suchen Sie mit Platzhaltern  
  
1.  Um die Verwendung von Platzhaltern im Feld **Suchen nach** bei der Schnellsuche, **In Dateien suchen**, **Schnellersetzung** oder **In Dateien ersetzen** zu aktivieren, wählen Sie unter **Suchoptionen** nacheinander **Verwenden** und dann **Platzhalter** aus.  
  
2.  Die dreieckige Schaltfläche für die **Verweisliste** neben dem Feld **Suchen nach** ist jetzt aktiviert. Klicken Sie auf diese Schaltfläche, um eine Liste der verfügbaren Platzhalter anzuzeigen. Wenn Sie ein Element aus der Liste für die **Verweisliste** auswählen, wird es in die **Suchen nach** -Zeichenfolge eingefügt.  
  
 Die folgende Tabelle enthält eine Beschreibung der Platzhalter, die unter **Verweisliste** verfügbar sind.  
  
|Ausdruck|Syntax|BESCHREIBUNG|  
|----------------|------------|-----------------|  
|Ein einzelnes Zeichen|?|Entspricht einem beliebigen einzelnen Zeichen.|  
|Eine einzelne Ziffer|#|Entspricht einer beliebigen einzelnen Ziffer. Beispiel: 7# entspricht Zahlen, die aus einer 7 bestehen, gefolgt von einer anderen Zahl, wie 71, nicht aber 17.|  
|Zeichen außerhalb des Zeichensatzes|[! ]|Entspricht einem einzelnen Zeichen, das nicht im Zeichensatz angegeben ist.|  
|Ein oder mehrere Zeichen|*|Entspricht einem oder mehreren Zeichen. Beispiel: new* entspricht einem beliebigen Text mit der Buchstabenfolge "new", z. B. newfile.txt.|  
|Zeichensatz|[ ]|Entspricht einem einzelnen Zeichen, das im Zeichensatz angegeben ist.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Suchen und Ersetzen](./search-and-replace.md)   
 [Suchen von Text mit regulären Ausdrücken](./search-text-with-regular-expressions.md)