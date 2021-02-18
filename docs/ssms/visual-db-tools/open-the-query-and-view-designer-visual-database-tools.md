---
title: Öffnen des Abfrage- und Sicht-Designers
description: Öffnen des Abfrage- und Sicht-Designers (Visual Database Tools)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- opening View Designer
- View Designer, opening
- Query Designer [SQL Server], opening
- opening Query Designer
ms.assetid: b473f258-d53c-41c0-9ad9-528a2ff141f4
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.openlocfilehash: 23cbda571b0112018c1da7165bb19e5f67e6bd4e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344089"
---
# <a name="open-the-query-and-view-designer-visual-database-tools"></a>Öffnen des Abfrage- und Sicht-Designers (Visual Database Tools)

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/applies-to-version/sql-asdb.md)]

Der Abfrage- und Sicht-Designer wird automatisch geöffnet, wenn Sie die Definition einer Sicht öffnen, die Ergebnisse für eine Abfrage oder Sicht anzeigen bzw. eine Abfrage erstellen oder öffnen. Er besteht aus vier separaten Bereichen:  
  
-   Der Diagrammbereich liefert eine grafische Darstellung der Tabellen bzw. Tabellenwertobjekte, die Sie aus der Datenverbindung ausgewählt haben. Weiterhin werden alle Joinbeziehungen zwischen ihnen angezeigt.  
  
-   Im Kriterienbereich können Sie Abfrageoptionen angeben, d.h. welche Datenspalten angezeigt oder wie die Ergebnisse sortiert werden und welche Zeilen ausgewählt werden sollen. Hierzu geben Sie die gewünschten Optionen in einem Datenblatt ähnlich einer Kalkulationstabelle ein.  
  
-   Sie können den SQL-Bereich verwenden, um eine eigene SQL-Anweisung zu erstellen. Sie können auch den Kriterienbereich und den Diagrammbereich verwenden, um die Anweisung zu erstellen, wobei dadurch die SQL-Anweisungen im SQL-Bereich erstellt werden. Beim Erstellen einer Abfrage wird der SQL-Bereich automatisch aktualisiert und zur besseren Lesbarkeit neu formatiert.  
  
-   Im Ergebnisbereich werden die Ergebnisse der zuletzt ausgeführten SELECT-Abfrage angezeigt. (Die Ergebnisse anderer Abfragetypen werden in Meldungsfeldern angezeigt.)  
  
-   Diese Bereiche sind sowohl beim Verwenden von Abfragen als auch beim Verwenden von Sichten nützlich.  
  
-   Beim Öffnen einer Sicht oder Abfrage werden gleichzeitig einige oder alle Bereiche geöffnet. Welche Bereiche geöffnet werden, hängt von den Einstellungen im Dialogfeld **Optionen** und von dem Datenbankmanagementsystem ab, mit dem Sie verbunden sind. Standardmäßig werden alle vier Bereiche geöffnet.  
  
### <a name="to-open-the-query-and-view-designer-for-a-view"></a>So öffnen Sie den Abfrage- und Sicht-Designer für eine Sicht  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Sicht, die Sie öffnen möchten, und klicken Sie dann auf **Entwerfen** oder **Sicht öffnen**.  
  
    Wenn Sie **Entwerfen** auswählen, werden die Bereiche des Abfrage- und Sicht-Designers entsprechend den Einstellungen im Dialogfeld **Optionen** geöffnet. Wenn Sie **Sicht öffnen** auswählen, wird standardmäßig nur der Ergebnisbereich geöffnet.  
  
### <a name="to-open-the-query-and-view-designer-for-an-existing-query"></a>So öffnen Sie den Abfrage- und Sicht-Designer für eine vorhandene Abfrage  
  
1.  Erweitern Sie im Projektmappen-Explorer den Ordner **Abfragen** .  
  
2.  Doppelklicken Sie auf die Abfrage, die Sie öffnen möchten.  
  
3.  Markieren Sie die Abfrageanweisung(en), klicken Sie mit der rechten Maustaste auf den markierten Bereich, und klicken Sie auf **Abfrage in Editor entwerfen**.  
  
## <a name="see-also"></a>Weitere Informationen  
[Themen zur Vorgehensweise: Entwerfen von Abfragen und Sichten &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
[Tools im Abfrage- und Sicht-Designer &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md)  
  
