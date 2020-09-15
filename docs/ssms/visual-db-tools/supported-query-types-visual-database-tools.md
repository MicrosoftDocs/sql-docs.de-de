---
description: Unterstützte Abfragetypen (Visual Database Tools)
title: Unterstützte Abfragetypen
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Delete query
- queries [SQL Server], types
- Update query
- Query Designer [SQL Server], query types
- Criteria pane
- Insert Values query
- Select query
- Make Table query
- Insert Results query
- Diagram pane [Visual Database Tools]
- View Designer, query types
ms.assetid: 72b9116c-c128-4078-a78d-257a2955a3f6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 3253aef57da9eb34ddd87b17aa9a0e690fd866b8
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88312357"
---
# <a name="supported-query-types-visual-database-tools"></a>Unterstützte Abfragetypen (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Sie können folgende Abfragetypen im Diagramm- oder Kriterienbereich (den grafischen Bereichen) des [Abfrage- und Sicht-Designers](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md)erstellen:  
  
-   **Auswahlabfrage** Ruft Daten aus einer oder mehreren Tabellen oder Sichten ab. Dieser Abfragetyp wird durch eine SELECT-Anweisung in SQL ausgedrückt.  
  
-   **Ergebnisse einfügen** Erstellt neue Zeilen durch Kopieren vorhandener Zeilen als neue Zeilen aus einer Tabelle in eine andere oder in dieselbe Tabelle. Dieser Abfragetyp wird durch eine INSERT INTO...SELECT-Anweisung in SQL ausgedrückt.  
  
-   **Werte einfügen** Erstellt eine neue Zeile und fügt Werte in die angegebenen Spalten ein. Dieser Abfragetyp wird durch eine INSERT INTO...VALUES-Anweisung in SQL ausgedrückt.  
  
-   **Aktualisierungsabfrage** Ändert die Werte einzelner Spalten in einer oder mehreren vorhandenen Zeilen einer Tabelle. Dieser Abfragetyp wird durch eine UPDATE…SET-Anweisung in SQL ausgedrückt.  
  
-   **Löschabfrage** Entfernt eine oder mehrere Zeilen aus einer Tabelle. Dieser Abfragetyp wird durch eine DELETE-Anweisung in SQL ausgedrückt.  
  
    > [!NOTE]  
    > Bei einer Löschabfrage werden ganze Zeilen aus der Tabelle gelöscht. Wenn Sie Werte aus einzelnen Datenspalten löschen möchten, verwenden Sie eine UPDATE-Abfrage.  
  
-   **Tabellenerstellungsabfrage** Erstellt eine neue Tabelle mit neuen Zeilen, indem die Ergebnisse einer Abfrage in die Tabelle kopiert werden. Dieser Abfragetyp wird durch eine SELECT...INTO-Anweisung in SQL ausgedrückt.  
  
Zusätzlich zu den in den grafischen Bereichen erstellten Abfragen können Sie jede SQL-Anweisung im SQL-Bereich eingeben, z. B. UNION-Abfragen.  
  
Wenn Sie mithilfe von SQL-Anweisungen Abfragen erstellen, die nicht in den grafischen Bereichen dargestellt werden können, blendet der Abfrage- und Sicht-Designer diese Bereiche ab und zeigt damit an, dass in diesen Bereichen nicht die erstellte Abfrage wiedergegeben wird. Die abgeblendeten Bereiche bleiben allerdings aktiv, und in vielen Fällen können Sie in diesen Bereichen Änderungen an der Abfrage vornehmen. Wenn sich aus den von Ihnen vorgenommenen Änderungen eine Abfrage ergibt, die in den grafischen Bereichen dargestellt werden kann, werden die Bereiche nicht länger abgeblendet angezeigt.  
  
## <a name="see-also"></a>Weitere Informationen  
[Artikel zum Entwerfen von Abfragen und Sichten](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
[Abfragetypen](../../ssms/visual-db-tools/types-of-queries-visual-database-tools.md)  
  
