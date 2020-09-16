---
description: Gruppieren von Zeilen in Abfrageergebnissen (Visual Database Tools)
title: Gruppenzeilen in Abfrageergebnissen
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- summarizing table subsets
- grouping rows
- grouping query results
ms.assetid: b07082d5-4d55-4903-9af9-4c470554c6d3
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.openlocfilehash: c607bc00528abe60ed4cb8e7d7e083f758cf9cea
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88468366"
---
# <a name="group-rows-in-query-results-visual-database-tools"></a>Gruppieren von Zeilen in Abfrageergebnissen (Visual Database Tools)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Zum Bilden von Teilergebnissen oder zum Anzeigen weiterer Kurzinformationen für Teilbereiche einer Tabelle erstellen Sie Gruppen mithilfe einer Aggregatabfrage. In jeder Gruppe werden die Daten aus allen Zeilen der Tabelle mit demselben Wert zusammengefasst.  
  
Angenommen, Sie möchten den durchschnittlichen Preis für ein Buch in der Tabelle `titles` anzeigen lassen, wobei die Ergebnisse nach Herausgeber aufgeteilt werden sollen. Dazu gruppieren Sie die Abfrage nach Herausgeber (z. B. `pub_id`). Hierfür kann folgende Abfrageausgabe formuliert werden:  
  
![Abfrageergebnisse: Durchschnittspreis gruppiert nach Verlag](../../ssms/visual-db-tools/media/dv3w9e1.gif "Abfrageergebnisse: Durchschnittspreis gruppiert nach Verlag")  
  
Beim Gruppieren von Daten können nur Daten aus Zusammenfassungen bzw. gruppierte Daten angezeigt werden, z. B.:  
  
-   Die Werte der gruppierten Spalten (die in der GROUP BY-Klausel auftreten). Im oben angeführten Beispiel ist `pub_id` die gruppierte Spalte.  
  
-   Die Werte, die von Aggregatfunktionen wie SUM( ) und AVG( ) erzeugt werden. Im oben aufgeführten Beispiel wird die zweite Spalte erstellt, indem die Funktion AVG( ) auf die Spalte `price` angewendet wird.  
  
Werte aus einzelnen Zeilen können nicht angezeigt werden. Wenn Sie z. B. nur nach Herausgeber gruppieren, können nicht gleichzeitig die einzelnen Titel in der Abfrage angezeigt werden. Wenn Sie daher dem Abfrageergebnis Spalten hinzufügen, fügt der [Abfrage- und Sicht-Designer](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) diese automatisch der GROUP BY-Klausel der Anweisung im [SQL-Bereich](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md)hinzu. Wenn eine Spalte stattdessen aggregiert werden soll, können Sie für diese Spalte eine Aggregatfunktion angeben.  
  
Wenn Sie nach mehreren Spalten gruppieren, werden von jeder Gruppe der Abfrage die Aggregatwerte für alle Gruppenspalten angezeigt.  
  
In der folgenden Abfrage für die Tabelle `titles` sind die Zeilen beispielsweise sowohl nach Herausgeber (`pub_id`) als auch nach Buchtyp (`type`) gruppiert. Die Abfrageergebnisse sind nach Herausgeber geordnet und zeigen für jeden Buchtyp des jeweiligen Herausgebers Kurzinformationen an:  
  
```  
SELECT pub_id, type, SUM(price) Total_price  
FROM titles  
GROUP BY pub_id, type  
```  
  
Die entsprechende Ausgabe könnte folgendermaßen aussehen:  
  
![Abfrageergebnisse: Preis gruppiert nach Verlag und Art](../../ssms/visual-db-tools/media/dv3w9e2.gif "Abfrageergebnisse: Preis gruppiert nach Verlag und Art")  
  
### <a name="to-group-rows"></a>So gruppieren Sie Zeilen  
  
1.  Starten Sie die Abfrage, indem Sie dem Diagrammbereich die Tabellen hinzufügen, die zusammengefasst werden sollen.  
  
2.  Klicken Sie mit der rechten Maustaste auf den Hintergrund des Diagrammbereichs, und wählen Sie im Kontextmenü die Option **Gruppe hinzufügen nach** aus. Der Abfrage- und Sicht-Designer fügt dem Datenblatt im Kriterienbereich die Spalte **Gruppieren nach** hinzu.  
  
3.  Fügen Sie dem Kriterienbereich die Spalte(n) hinzu, die gruppiert werden soll(en). Wenn die Spalte im Abfrageergebnis aufgeführt werden soll, muss die Spalte **Ausgabe** für die Ausgabe aktiviert sein.  
  
    Der Abfrage- und Sicht-Designer fügt der Anweisung im SQL-Bereich eine GROUP BY-Klausel hinzu. Die SQL-Anweisung könnte z. B. folgendermaßen aussehen:  
  
    ```  
    SELECT pub_id  
    FROM titles  
    GROUP BY pub_id  
    ```  
  
4.  Fügen Sie dem Kriterienbereich die Spalte(n) hinzu, die aggregiert werden soll(en). Vergewissern Sie sich, dass die Spalte für die Ausgabe ausgewählt ist.  
  
5.  Wählen Sie in der Datenblattzelle **Gruppieren nach** der zu aggregierenden Spalte die entsprechende Aggregatfunktion aus.  
  
    Der Abfrage- und Sicht-Designer weist der Spalte, die zusammengefasst wird, automatisch einen Spaltenalias zu. Sie können diesen automatisch generierten durch einen aussagekräftigeren Alias ersetzen. Weitere Informationen finden Sie unter [Erstellen von Spaltenaliasen (Visual Database Tools)](../../ssms/visual-db-tools/create-column-aliases-visual-database-tools.md).  
  
    ![Hinzufügen eines Spaltenalias zum Resultset der Abfrage](../../ssms/visual-db-tools/media/dv3w9e3.gif "Hinzufügen eines Spaltenalias zum Resultset der Abfrage")  
  
    Die entsprechende Anweisung im Bereich **SQL** könnte folgendermaßen aussehen:  
  
    ```  
    SELECT   pub_id, SUM(price) AS Totalprice  
    FROM     titles  
    GROUP BY pub_id  
    ```  
  
## <a name="see-also"></a>Weitere Informationen  
[Sortieren und Gruppieren von Abfrageergebnissen](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md)  
  
