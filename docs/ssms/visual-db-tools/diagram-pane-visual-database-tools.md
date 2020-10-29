---
description: Diagrammbereich (Visual Database Tools)
title: Diagrammbereich
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Query Designer [SQL Server], Diagram pane
- View Designer, Diagram pane
- joins [SQL Server], Query and View Designer
- Diagram pane [Visual Database Tools]
ms.assetid: 399dfc7b-e2e7-47d3-bd11-163cbe0ce13c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: eaba2d8acd2a0443bfe2ed97fc53b50fcb667629
ms.sourcegitcommit: fb8724fb99c46ecf3a6d7b02a743af9b590402f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2020
ms.locfileid: "92439384"
---
# <a name="diagram-pane-visual-database-tools"></a>Diagrammbereich (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Der Diagrammbereich liefert eine grafische Darstellung der Tabellen bzw. Tabellenwertobjekte, die Sie aus der Datenverbindung ausgewählt haben. Weiterhin werden alle Joinbeziehungen zwischen ihnen angezeigt.  
  
Im Diagrammbereich können Sie folgende Aktionen ausführen:  
  
-   Tabellen und Tabellenwertobjekte hinzufügen oder entfernen und Datenspalten für die Ausgabe festlegen  
  
-   Joins zwischen Tabellen und Tabellenwertobjekten erstellen oder ändern  
  
Wenn Sie Änderungen im Diagrammbereich vornehmen, werden die Werte im Kriterienbereich und im SQL-Bereich entsprechend aktualisiert. Wenn Sie beispielsweise im Diagrammbereich eine Spalte für die Ausgabe im Fenster einer Tabelle oder eines Tabellenwertobjekts auswählen, fügt der Abfrage- und Sicht-Designer die Datenspalte im Kriterienbereich hinzu und ergänzt die SQL-Anweisung im SQL-Bereich.  
  
Jede Tabelle und jedes Tabellenwertobjekt wird im Diagrammbereich in einem separaten Fenster angezeigt. Das Symbol in der Titelleiste jedes Rechtecks gibt den dargestellten Objekttyp wieder, wie in der folgenden Tabelle beschrieben.  
  
## <a name="options"></a>Optionen  
**Tabellen**  
Listet die Tabellen auf, die dem Diagrammbereich hinzugefügt werden können. Um eine Tabelle hinzuzufügen, wählen Sie sie aus, und klicken Sie auf **Hinzufügen** . Um mehrere Tabellen gleichzeitig hinzuzufügen, wählen Sie sie aus, und klicken Sie auf **Hinzufügen** .  
  
**Ansichten**  
Listet die Sichten auf, die dem Diagrammbereich hinzugefügt werden können. Um eine Sicht hinzuzufügen, wählen Sie sie aus, und klicken Sie auf **Hinzufügen** . Um mehrere Sichten gleichzeitig hinzuzufügen, wählen Sie sie aus, und klicken Sie auf **Hinzufügen** .  
  
**Funktionen**  
Listet die benutzerdefinierten Funktionen auf, die dem Diagrammbereich hinzugefügt werden können. Um eine Funktion hinzuzufügen, wählen Sie sie aus, und klicken Sie auf **Hinzufügen** . Um mehrere Funktionen gleichzeitig hinzuzufügen, wählen Sie sie aus, und klicken Sie auf **Hinzufügen** .  
  
**Lokale Tabellen**  
Listet die von Abfragen erstellten Tabellen auf, nicht die zur Datenbank gehörigen.  
  
**Synonyme**  
Listet die Synonyme auf, die dem Diagrammbereich hinzugefügt werden können. Um ein Synonym hinzuzufügen, wählen Sie es aus, und klicken Sie auf **Hinzufügen** . Um mehrere Synonyme gleichzeitig hinzuzufügen, wählen Sie sie aus, und klicken Sie auf **Hinzufügen** .  
  
|Symbol|Objekttyp|  
|--------|---------------|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbi1.gif":::|Tabelle|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbi2.gif":::|Abfrage oder Sicht|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbi3.gif":::|Verknüpfte Tabelle|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dvudficon.gif":::|Benutzerdefinierte Funktion|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbi5.gif":::|Verknüpfte Sicht|  
  
Jedes Rechteck zeigt die Datenspalten für die Tabelle bzw. das Tabellenwertobjekt an. Neben den Spaltennamen finden Sie Kontrollkästchen und Symbole, die Informationen zum Verwenden der Spalten in der Abfrage liefern. In QuickInfos erhalten Sie u. a. Informationen zum Datentyp und zur Größe der Spalten.  
  
In der folgenden Tabelle werden die verwendeten Kontrollkästchen und Symbole für die einzelnen Tabellen bzw. Tabellenwertobjekte erläutert.  
  
|Kontrollkästchen oder Symbol|BESCHREIBUNG|  
|-----------------------|---------------|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbi7.gif":::<br /><br />:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbi8.gif":::<br /><br />:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbi9.gif":::<br /><br />:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbia.gif":::|Gibt an, ob eine Datenspalte im Resultset der Abfrage enthalten ist oder nicht (SELECT-Abfrage) oder ob sie in einer UPDATE-, INSERT FROM-, MAKE TABLE- oder INSERT INTO-Abfrage verwendet wird. Wählen Sie die Spalte aus, um sie in die Ergebnisse aufzunehmen. Wenn **(Alle Spalten)** ausgewählt ist, werden in der Ausgabe alle Datenspalten angezeigt.<br /><br />Das mit dem Kontrollkästchen verwendete Symbol ändert sich entsprechend dem Typ der erstellten Abfrage. Beim Erstellen einer DELETE-Abfrage können Sie keine einzelnen Spalten auswählen.|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbib.gif":::<br /><br />:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbic.gif":::|Zeigt an, dass die Datenspalte zum Sortieren der Abfrageergebnisse verwendet wird (d. h. Teil einer ORDER BY-Klausel ist). Das Symbol wird als A-Z bei aufsteigender oder als Z-A bei absteigender Sortierreihenfolge dargestellt.|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbid.gif":::|Zeigt an, dass die Datenspalte in einer Aggregatabfrage zum Erstellen eines gruppierten Resultsets verwendet wird (d. h. Teil einer GROUP BY-Klausel ist).|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbie.gif":::|Zeigt an, dass die Datenspalte in eine Suchbedingung der Abfrage eingebunden ist (d. h. Teil einer WHERE- oder HAVING-Klausel ist).|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbif.gif":::|Zeigt an, dass die Inhalte der Datenspalte für die Ausgabe zusammengefasst werden (d. h. in eine SUM- oder AVG- oder eine andere Aggregatfunktion eingebunden sind).|  
  
> [!NOTE]  
> Der Abfrage- und Sicht-Designer zeigt keine Datenspalten für eine Tabelle oder ein Tabellenwertobjekt an, wenn Sie nicht über die erforderlichen Zugriffsrechte verfügen oder der ODBC-Treiber keine Informationen über die Tabelle oder das Objekt zurückgeben kann. In einem solchen Fall zeigt der Abfrage- und Sicht-Designer nur die Titelleiste für die Tabelle oder das Objekt mit Tabellenstruktur an.  
  
## <a name="joined-tables-on-the-diagram-pane"></a>Verknüpfte Tabellen im Diagrammbereich  
Wenn die Abfrage einen Join enthält, wird eine Joinlinie zwischen den verknüpften Datenspalten angezeigt. Wenn die verknüpften Datenspalten nicht angezeigt werden (z. B., weil das Fenster für die Tabelle oder das Tabellenwertobjekt minimiert ist oder der Join einen Ausdruck beinhaltet), setzt der Abfrage- und Sicht-Designer die Joinlinie in die Titelleiste des Rechtecks, das die Tabelle oder das Tabellenwertobjekt darstellt. Der Abfrage- und Sicht-Designer zeigt eine Joinlinie für jede Joinbedingung an.  
  
Die Form des Symbols in der Mitte der Joinlinie zeigt an, wie die Tabellen oder Objekte mit Tabellenstruktur verknüpft sind. Wenn die Joinklausel einen anderen Operator als "gleich" (=) verwendet, wird der Operator im Symbol der Joinlinie angezeigt. In der folgenden Tabelle werden die in Joinlinien verwendeten Symbole aufgelistet.  
  
|Joinliniensymbol|BESCHREIBUNG|  
|------------------|---------------|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbih.gif":::|Innerer Join (erstellt mit einem Gleichheitszeichen).|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbii.gif":::|Innerer Join mit dem Operator "größer als". (Der im Symbol der Joinlinie angezeigte Operator gibt den im Join verwendeten Operator wieder.)|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbij.gif":::|Äußerer Join, bei dem sämtliche Zeilen aus der links angezeigten Tabelle aufgenommen werden, auch wenn keine Übereinstimmungen in der verknüpften Tabelle vorliegen.|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbik.gif":::|Äußerer Join, bei dem sämtliche Zeilen aus der rechts angezeigten Tabelle aufgenommen werden, auch wenn keine Übereinstimmungen in der verknüpften Tabelle vorliegen.|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbil.gif":::|Ein vollständiger äußerer Join, bei dem alle Zeilen aus beiden Tabellen aufgenommen werden, auch wenn keine Übereinstimmungen in der verknüpften Tabelle vorliegen.|  
  
Symbole an den Enden der Joinlinie zeigen den Jointyp an. In der folgenden Tabelle werden die Jointypen und die an den Enden der Joinlinien verwendeten Symbole aufgelistet.  
  
|Symbole an den Enden der Joinlinien|BESCHREIBUNG|  
|-----------------------------|---------------|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbim.gif":::|1:1-Join|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbin.gif":::|1:n-Join|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbio.gif ":::|Der Abfrage- und Sicht-Designer konnte den Jointyp nicht ermitteln.|  
  
## <a name="see-also"></a>Weitere Informationen  
[Themen zur Vorgehensweise: Entwerfen von Abfragen und Sichten &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
[Kriterienbereich &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md)  
[Sortieren und Gruppieren von Abfrageergebnissen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md)  
  
