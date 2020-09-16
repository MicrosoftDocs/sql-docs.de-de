---
description: Parameterabfragen (Visual Database Tools)
title: Parameterabfragen
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- parameter queries [SQL Server]
ms.assetid: 4897c41a-324a-47b8-a30b-cbc9e9e19a8b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 264992e76d738b16c2a520342547889de9ba1a0a
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88468368"
---
# <a name="parameter-queries-visual-database-tools"></a>Parameterabfragen (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Für bestimmte Aufgaben werden Abfragen benötigt, die bei sich wiederholenden Aufrufen die Übergabe eines wechselnden Werts ermöglichen. Angenommen, Sie führen des Öfteren eine Abfrage aus, mit der alle `title_ids` eines Autors abgerufen werden. In diesem Fall führen Sie für jede Anforderung dieselbe Abfrage aus, wobei jedoch die ID bzw. der Name des Autors jeweils unterschiedlich ist.  
  
Zum Erstellen einer Abfrage, die bei jedem Aufruf mit unterschiedlichen Werten ausgeführt werden kann, verwenden Sie Parameter innerhalb der Abfrage. Ein Parameter ist ein Platzhalter für einen Wert, der erst beim Ausführen der Abfrage festgelegt wird. Eine SQL-Anweisung mit einem Parameter könnte folgendermaßen lauten, wobei "?" als Parameter für die ID des Autors dient:  
  
```  
SELECT title_id  
FROM titleauthor  
WHERE (au_id = ?)  
```  
  
## <a name="where-you-can-use-parameters"></a>Verwendungsmöglichkeit von Parametern  
Sie können Parameter als Platzhalter für Literalwerte verwenden, d.h. sowohl für Textwerte als auch für numerische Werte. Sehr häufig werden Parameter als Platzhalter in Suchbedingungen für einzelne Zeilen oder Zeilengruppen verwendet (also in den WHERE- oder HAVING-Klauseln einer SQL-Anweisung).  
  
Sie können Parameter als Platzhalter in Ausdrücken verwenden. Angenommen, Sie möchten Rabattpreise berechnen und bei jedem Ausführen der Abfrage einen anderen Rabattwert angeben können. Hierzu lässt sich folgender Ausdruck verwenden:  
  
```  
(price * ?)  
```  
  
## <a name="specifying-unnamed-and-named-parameters"></a>Angeben unbenannter und benannter Parameter  
Sie können zwei Parametertypen angeben: unbenannte und benannte Parameter. Ein unbenannter Parameter wird durch ein Fragezeichen (?) bezeichnet, das Sie in einer Abfrage an der Position einfügen, an der Sie einen Literalwert abfragen oder einsetzen möchten. Wenn Sie z. B. einen unbenannten Parameter für die Suche nach einer Autoren-ID in der Tabelle `titleauthor` verwenden, ergibt sich folgende Anweisung im [SQL-Bereich](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md) :  
  
```  
SELECT title_id  
FROM titleauthor  
WHERE (au_id = ?)  
```  
  
Wenn Sie die Abfrage im [Abfrage- und Sicht-Designer](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md)ausführen, wird das [Dialogfeld „Abfrageparameter“](../../ssms/visual-db-tools/query-parameters-dialog-box-visual-database-tools.md) mit „?“ als Name des Parameters angezeigt.  
  
Sie können einem Parameter auch einen Namen zuweisen. Benannte Parameter sind dann hilfreich, wenn in einer Abfrage mehrere Parameter enthalten sind. Wenn Sie z. B. benannte Parameter für die Suche nach dem Vor- und Nachnamen eines Autors in der Tabelle `authors` verwenden, ergibt sich folgende Anweisung im SQL-Bereich:  
  
```  
SELECT au_id  
FROM authors  
WHERE au_fname = %first name% AND  
      au_lname = %last name%  
```  
  
> [!TIP]  
> Definieren Sie Präfix- und Suffixzeichen, bevor Sie eine Abfrage mit benannten Parametern erstellen.  
  
Wenn Sie die Abfrage im Abfrage- und Sicht-Designer ausführen, wird das [Dialogfeld „Abfrageparameter“](../../ssms/visual-db-tools/query-parameters-dialog-box-visual-database-tools.md) mit einer Liste benannter Parameter angezeigt.  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen von Abfragen mit Parametern &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-parameters-visual-database-tools.md)  
[Unterstützte Abfragetypen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/supported-query-types-visual-database-tools.md)  
[Themen zur Vorgehensweise: Entwerfen von Abfragen und Sichten &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
