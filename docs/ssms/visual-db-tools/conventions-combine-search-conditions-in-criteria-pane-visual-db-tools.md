---
description: Konventionen für das Kombinieren von Suchbedingungen im Kriterienbereich (Visual Database Tools)
title: Konventionen für das Kombinieren von Suchbedingungen im Kriterienbereich
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- search conditions [SQL Server], combining
- precedence [SQL Server], Criteria pane
- search criteria [SQL Server], combining conditions
- multiple OR clauses
- combining search conditions
- OR operator
- AND, Criteria pane
- multiple AND clauses
ms.assetid: d4859be5-ff5b-48b2-a101-ad40c6dbcc68
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: fe9c75b0b173bf2f72437233d2c06305b5041132
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355207"
---
# <a name="conventions-for-combining-search-conditions-in-the-criteria-pane-visual-database-tools"></a>Konventionen für das Kombinieren von Suchbedingungen im Kriterienbereich (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Abfragen können eine beliebige Anzahl von Suchbedingungen enthalten, die mit beliebig vielen AND-Operatoren und OR-Operatoren verknüpft werden können. Eine Abfrage mit mehreren AND-Klauseln und OR-Klauseln kann sehr komplex sein. Es ist daher hilfreich zu wissen, wie eine Abfrage bei der Ausführung interpretiert wird und wie sie im [Kriterienbereich](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md) und im [SQL-Bereich](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md) dargestellt wird.  
  
> [!NOTE]  
> Ausführliche Informationen zu Suchbedingungen, die nur einen AND- oder OR-Operator enthalten, finden Sie unter [Angeben mehrerer Suchbedingungen für eine Spalte &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-multiple-search-conditions-for-one-column-visual-database-tools.md) und [Angeben mehrerer Suchbedingungen für mehrere Spalten &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-multiple-search-conditions-for-multiple-columns-visual-database-tools.md).  
  
Nachstehend erhalten Sie Informationen zu den folgenden Themen:  
  
-   Der Vorrang von AND und OR in Abfragen mit beiden Operatoren.  
  
-   Die logische Beziehung der Bedingungen in AND-Klauseln und OR-Klauseln.  
  
-   Die Darstellung von Abfragen, die AND und OR enthalten, durch den Abfrage- und Sicht-Designer im Kriterienbereich.  
  
In den Beispielen in den folgenden Abschnitten wird eine `employee` -Tabelle verwendet, die die Spalten `hire_date`, `job_lvl`und `status`enthält. In den Beispielen wird angenommen, dass Sie Informationen suchen, beispielsweise die Betriebszugehörigkeit eines Mitarbeiters (Einstellungsdatum), die Art der Tätigkeit des Mitarbeiters (Stellenbeschreibung) und den Mitarbeiterstatus (z. B. Ruhestand).  
  
## <a name="precedence-of-and-and-or"></a>Vorrang von AND und OR  
Bei der Ausführung einer Abfrage werden zuerst die mit AND verknüpften Klauseln und anschließend die mit OR verknüpften Klauseln ausgewertet.  
  
> [!NOTE]  
> Der NOT-Operator hat gegenüber AND und OR Vorrang.  
  
Sie können eine dem folgenden Beispiel entsprechende WHERE-Klausel erstellen, um beispielsweise nach Mitarbeitern zu suchen, die seit mindestens fünf Jahren in Tätigkeiten einer unteren Stufe in der Firma beschäftigt sind, oder die unabhängig vom Einstellungsdatum in einer Tätigkeit einer mittleren Stufe angestellt sind:  
  
```  
WHERE   
   hire_date < '01/01/95' AND   
   job_lvl = 100 OR  
   job_lvl = 200  
```  
  
Zum Überschreiben des Standardvorrangs von AND gegenüber OR können Sie Klammern um bestimmte Bedingungen im SQL-Bereich setzen. Bedingungen in Klammern werden immer zuerst ausgewertet. Sie können eine dem folgenden Beispiel entsprechende WHERE-Klausel erstellen, um beispielsweise nach allen Mitarbeitern zu suchen, die länger als fünf Jahre in einer Tätigkeit der unteren oder mittleren Stufe in der Firma arbeiten:  
  
```  
WHERE   
   hire_date < '01/01/95' AND   
   (job_lvl = 100 OR job_lvl = 200)  
```  
  
> [!TIP]  
> Es wird empfohlen, aus Gründen der Übersichtlichkeit beim Kombinieren von AND-Klauseln und OR-Klauseln immer Klammern zu setzen, auch wenn der Standardvorrang gilt.  
  
## <a name="how-and-works-with-multiple-or-clauses"></a>Verwenden von AND mit mehreren OR-Klauseln  
Wenn Sie wissen, wie AND-Klauseln und OR-Klauseln beim Kombinieren miteinander verknüpft werden, erleichtert dies das Erstellen und Verstehen komplexer Abfragen im Abfrage- und Sicht-Designer.  
  
Wenn Sie mehrere Bedingungen mit AND verknüpfen, gilt der erste Satz der mit AND verknüpften Bedingungen für alle Bedingungen im zweiten Satz. Das heißt, dass eine Bedingung, die durch AND mit einer anderen Bedingung verknüpft ist, an alle Bedingungen im zweiten Satz verteilt wird. Die folgende schematische Darstellung enthält eine mit einem Satz von OR-Bedingungen verknüpfte AND-Bedingung:  
  
```  
A AND (B OR C)  
```  
  
Die Darstellung ist logisch equivalent zu der folgenden schematischen Darstellung, die zeigt, wie sich die AND-Bedingung zum zweiten Satz von Bedingungen verhält:  
  
```  
(A AND B) OR (A AND C)  
```  
  
Dieses Distributivprinzip beeinflusst auch die Verwendung des Abfrage- und Sicht-Designers. Angenommen, Sie suchen alle Mitarbeiter, die der Firma seit mehr als fünf Jahren angehören und Tätigkeiten einer niedrigen oder mittleren Stufe ausführen. Geben Sie die folgende WHERE-Klausel im SQL-Bereich in die Anweisung ein:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
   (job_lvl = 100 OR job_lvl = 200)  
```  
  
Die mit AND verknüpfte Klausel gilt für beide mit OR verknüpften Klauseln. Sie können dies auch durch Wiederholen der AND-Bedingung für jede Bedingung in der OR-Klausel ausdrücken. Die folgende Anweisung ist genauer (und länger) als die vorhergehende Anweisung, ist jedoch logisch equivalent:  
  
```  
WHERE    (hire_date < '01/01/95' ) AND  
  (job_lvl = 100) OR   
  (hire_date < '01/01/95' ) AND   
  (job_lvl = 200)  
```  
  
Die Distribution von AND-Klauseln zu verknüpften OR-Klauseln kann immer angewendet werden, unabhängig von der Anzahl von einzelnen Bedingungen. Angenommen, Sie möchten alle Mitarbeiter suchen, die der Firma seit mehr als fünf Jahren angehören und die Tätigkeiten der mittleren oder höheren Stufe ausführen oder im Ruhestand sind. Die entsprechende WHERE-Klausel kann folgendermaßen lauten:  
  
```  
WHERE   
   (job_lvl = 200 OR job_lvl = 300) AND  
   (hire_date < '01/01/95' ) OR (status = 'R')  
```  
  
Nachdem die mit AND verknüpften Bedingungen verteilt wurden, ergibt sich folgende WHERE-Klausel:  
  
```  
WHERE   
   (job_lvl = 200 AND hire_date < '01/01/95' ) OR  
   (job_lvl = 200 AND status = 'R') OR  
   (job_lvl = 300 AND hire_date < '01/01/95' ) OR  
   (job_lvl = 300 AND status = 'R')  
```  
  
## <a name="how-multiple-and-and-or-clauses-are-represented-in-the-criteria-pane"></a>Darstellung mehrerer AND-Klauseln und OR-Klauseln im Kriterienbereich  
Der Abfrage- und Sicht-Designer stellt die Suchbedingungen im [Kriterienbereich](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md)dar. Bei mehreren mit AND und OR verknüpften Klauseln kann diese Darstellung im Kriterienbereich jedoch abweichen. Außerdem kann die SQL-Anweisung von den Eingaben abweichen, wenn Sie die Abfrage im Kriterienbereich oder im [Diagrammbereich](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md)ändern.  
  
Diese Regeln legen grundsätzlich fest, wie AND- und OR-Klauseln im Kriterienbereich angezeigt werden:  
  
-   Alle mit AND verknüpften Bedingungen werden in der Datenblattspalte **Filter** oder in derselben **Oder** -Spalte angezeigt.  
  
-   Alle mit OR verknüpften Bedingungen werden in separaten **Oder** Spalten angezeigt.  
  
-   Wenn das logische Ergebnis einer Kombination von AND- und OR-Klauseln die Verteilung von AND in verschiedene OR-Klauseln ist, wird dies im Kriterienbereich entsprechend angezeigt, indem die AND-Klausel so oft wie erforderlich wiederholt wird.  
  
Sie könnten z. B. im SQL-Bereich eine Suchbedingung nach folgendem Muster erstellen. Dabei haben zwei mit AND verknüpfte Klauseln Vorrang gegenüber einer dritten mit OR verknüpften Klausel:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  (job_lvl = 100) OR   
  (status = 'R')  
```  
  
Diese WHERE-Klausel wird vom Abfrage- und Sicht-Designer folgendermaßen im Kriterienbereich dargestellt:  
  
![OR-Klauselrangfolge im Kriterienbereich](../../ssms/visual-db-tools/media/vs_criteriapane1.gif "OR-Klauselrangfolge im Kriterienbereich")  
  
Wenn die verknüpfte OR-Klausel Vorrang vor einer AND-Klausel erhält, wird die AND-Klausel für jede OR-Klausel wiederholt. Dadurch wird die AND-Klausel an jede OR-Klausel verteilt. Sie können im SQL-Bereich z. B. folgende WHERE-Klausel erstellen:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  ( (job_lvl = 100) OR   
  (status = 'R') )  
```  
  
Diese WHERE-Klausel wird vom Abfrage- und Sicht-Designer folgendermaßen im Kriterienbereich dargestellt:  
  
![Mehrere AND- und OR-Klauseln im Kriterienbereich](../../ssms/visual-db-tools/media/vs_criteriapane2.gif "Mehrere AND- und OR-Klauseln im Kriterienbereich")  
  
Wenn die verknüpften OR-Klauseln nur eine Datenspalte enthalten, kann der Abfrage- und Sicht-Designer die gesamte OR-Klausel in eine Zelle des DatenBlatts einfügen, sodass die AND-Klausel nicht ständig wiederholt werden muss. Sie können im SQL-Bereich z. B. folgende WHERE-Klausel erstellen:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  ((status = 'R') OR (status = 'A'))  
```  
  
Diese WHERE-Klausel wird vom Abfrage- und Sicht-Designer folgendermaßen im Kriterienbereich dargestellt:  
  
![Im Kriterienbereich definierte, verknüpfte OR-Klauseln](../../ssms/visual-db-tools/media/vs_criteriapane3.gif "Im Kriterienbereich definierte, verknüpfte OR-Klauseln")  
  
Wenn Sie die Abfrage ändern (z. B. durch Ändern eines der Werte im Kriterienbereich), erstellt der Abfrage- und Sicht-Designer die SQL-Anweisung im SQL-Bereich neu. Die neu erstellte SQL-Anweisung stimmt eher mit der Anzeige im Kriterienbereich als mit der ursprünglichen Anweisung überein. Wenn der Kriterienbereich z. B. verteilte AND-Klauseln enthält, wird die Anweisung im SQL-Bereich mit eindeutig verteilten AND-Klauseln erstellt. Ausführliche Informationen finden Sie weiter oben unter "Verwenden von AND mit mehreren OR-Klauseln".  
  
## <a name="see-also"></a>Siehe auch  
[Angeben von Suchkriterien &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  
