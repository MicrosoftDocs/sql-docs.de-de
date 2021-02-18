---
description: Darstellungsweise von Joins im Abfrage- und Sicht-Designer (Visual Database Tools)
title: Darstellungsweise von Joins im Abfrage- und Ansicht-Designer
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL pane [Visual Database Tools]
- joins [SQL Server], Query and View Designer
- Diagram pane [Visual Database Tools]
ms.assetid: 20a99dcb-83bd-4aa6-9139-92e2e5ba4887
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 11b6399adb28a81f5364e6073a6fb0fde757ffe7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338055"
---
# <a name="how-the-query-and-view-designer-represents-joins-visual-database-tools"></a>Darstellungsweise von Joins im Abfrage- und Sicht-Designer (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 Bei verknüpften Tabellen stellt der [Abfrage- und Sicht-Designer](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) die Verknüpfung im [Diagrammbereich](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md) grafisch und im [SQL-Bereich](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md) mithilfe von SQL-Syntax dar.  
  
## <a name="diagram-pane"></a>Diagrammbereich  
Im Diagrammbereich wird im Abfrage- und Sicht-Designer eine Joinlinie zwischen den verknüpften Datenspalten an. Der Abfrage- und Sicht-Designer zeigt eine Joinlinie für jede Joinbedingung an. Die folgende Abbildung zeigt eine Joinlinie zwischen zwei verknüpften Tabellen:  
  
![Joinlinie zeigt Beziehung zwischen zwei Tabellen](../../ssms/visual-db-tools/media/dv3wbig.gif "Joinlinie zeigt Beziehung zwischen zwei Tabellen")  
  
Wenn Tabellen durch mehrere Joinbedingungen miteinander verknüpft sind, zeigt der Abfrage- und Sicht-Designer wie im folgenden Beispiel mehrere Joinlinien an:  
  
![Mit mehr als einer Verknüpfungsbedingung verknüpfte Tabellen](../../ssms/visual-db-tools/media/dv3w9n1.gif "Mit mehr als einer Verknüpfungsbedingung verknüpfte Tabellen")  
  
Wenn die verknüpften Datenspalten nicht angezeigt werden (z. B., weil das die Tabelle oder das Objekt mit Tabellenstruktur darstellende Rechteck minimiert ist oder der Join einen Ausdruck beinhaltet), setzt der Abfrage- und Sicht-Designer die Joinlinie in die Titelleiste des Rechtecks, das die Tabelle oder das Objekt mit Tabellenstruktur darstellt.  
  
Die Form des Symbols in der Mitte der Joinlinie zeigt an, wie die Tabellen oder Objekte mit Tabellenstruktur verknüpft sind. Wenn die Joinklausel einen anderen Operator als „gleich“ (=) verwendet, wird der Operator im Symbol der Joinlinie angezeigt. In der folgenden Tabelle werden die in der Joinlinie angezeigten Symbole aufgelistet.  
  
|**Joinliniensymbol**|**Beschreibung**|  
|----------------------|-------------------|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbih.gif":::|Innerer Join (erstellt mit einem Gleichheitszeichen).|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbii.gif":::|Innerer Join mit dem Operator "größer als".|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbij.gif":::|Äußerer Join, bei dem sämtliche Zeilen aus der links angezeigten Tabelle aufgenommen werden, auch wenn keine Übereinstimmungen in der verknüpften Tabelle vorliegen.|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbik.gif":::|Äußerer Join, bei dem sämtliche Zeilen aus der rechts angezeigten Tabelle aufgenommen werden, auch wenn keine Übereinstimmungen in der verknüpften Tabelle vorliegen.|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbil.gif":::|Ein vollständiger äußerer Join, bei der alle Zeilen aus beiden Tabellen aufgenommen werden, auch wenn keine Übereinstimmungen in der verknüpften Tabelle vorliegen.|  
  
Die Symbole an den Enden der Joinlinie zeigen den Jointyp an. In der folgenden Tabelle werden die Jointypen und die an den Enden der Joinslinien verwendeten Symbole aufgelistet.  
  
|**Symbole an den Enden der Joinlinien**|**Jointyp**|  
|---------------------------------|--------------------|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbim.gif":::|1:1-Join|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbin.gif":::|1:n-Join|  
|:::image type="icon" source="../../ssms/visual-db-tools/media/dv3wbio.gif":::|Der Abfrage- und Sicht-Designer konnte den Joinstyp nicht ermitteln. Dies tritt häufig auf, wenn Sie einen Join manuell erstellt haben.|  
  
## <a name="sql-pane"></a>SQL-Bereich  
Ein Join kann in einer SQL-Anweisung auf unterschiedliche Weise ausgedrückt werden. Die genaue Syntax ergibt sich aus der verwendeten Datenbank und daraus, wie Sie den Join definiert haben.  
  
Folgende Syntaxoptionen werden beim Verknüpfen von Tabellen angewendet:  
  
-   **JOIN-Qualifizierer in der FROM-Klausel**.   Die Schlüsselwörter INNER und OUTER geben den Jointyp an. Diese Syntax entspricht dem Standard bei ANSI 92 SQL.  
  
    Wenn Sie z. B. die Tabellen `publishers` und `pub_info` über die Spalte `pub_id` der beiden Tabellen verknüpfen, kann dies mit folgender SQL-Anweisung ausgedrückt werden:  
  
    ```  
    SELECT *  
    FROM publishers INNER JOIN pub_info ON  
       publishers.pub_id = pub_info.pub_id  
    ```  
  
    Wenn Sie einen äußeren Join erstellen, wird LEFT OUTER oder RIGHT OUTER statt INNER verwendet.  
  
-   **WHERE-Klausel zum Vergleich der Spalten in beiden Tabellen**.   Eine WHERE-Klausel wird angezeigt, wenn die Datenbank die JOIN-Syntax nicht unterstützt (oder wenn Sie sie selbst eingegeben haben). Wenn der Join über die WHERE-Klausel erstellt wird, werden beide Tabellennamen in der FROM-Klausel angegeben.  
  
    Die folgende Anweisung verknüpft z. B. die Tabellen `publishers` und `pub_info` .  
  
    ```  
    SELECT *  
    FROM publishers, pub_info  
    WHERE publishers.pub_id = pub_info.pub_id  
    ```  
  
## <a name="see-also"></a>Siehe auch  
[Erstellen von Abfragen mit Joins &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-joins-visual-database-tools.md)  
[Verknüpfen (Dialogfeld) &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/join-dialog-box-visual-database-tools.md)  
  
