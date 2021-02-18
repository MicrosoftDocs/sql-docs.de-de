---
description: Wertzusammenfassung oder -aggregation für alle Zeilen in einer Tabelle (Visual Database Tools)
title: Wertzusammenfassung oder -aggregation für alle Zeilen in einer Tabelle
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- summarizing query results
- aggregate functions [SQL Server], summarizing query results
ms.assetid: f5af876e-f937-4110-ba09-07999c35a699
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 88c4808e8418c248f54c9da8966306020fbb72f0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350418"
---
# <a name="summarize-or-aggregate-values-for-all-rows-in-a-table-visual-database-tools"></a>Wertzusammenfassung oder -aggregation für alle Zeilen in einer Tabelle (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
## <a name="aggregate-function"></a>Aggregate-Funktion
Mithilfe einer Aggregatfunktion können Sie eine Zusammenfassung für alle Werte in einer Tabelle erstellen. Sie können z. B. folgende Abfrage erstellen, um den Gesamtpreis aller Bücher in der Tabelle `titles` anzeigen zu lassen:  
  
```  
SELECT SUM(price)  
FROM titles  
```  
  
Erstellen Sie mehrere Aggregationen in derselben Abfrage, indem Sie Aggregatfunktionen mit mehreren Spalten verwenden. Beispielsweise können Sie eine Abfrage erstellen, in der die Gesamtsumme der `price` -Spalte und der Durchschnittswert der `discount` -Spalte berechnet werden.  
  
Sie können eine einzige Spalte in derselben Abfrage auf unterschiedliche Weise aggregieren (z. B. Ausgabe der Gesamtsumme, der Anzahl und des Durchschnittswerts). In der folgenden Abfrage wird beispielsweise der Durchschnittswert und die Gesamtsumme für die `price` -Spalte in der Tabelle `titles` berechnet:  
  
```  
SELECT AVG(price), SUM(price)  
FROM titles  
```  
  
Wenn Sie eine Suchbedingung hinzufügen, können Sie die Teilmenge von Zeilen aggregieren, die die entsprechende Bedingung erfüllt.  

**Hinweis!** Sie können außerdem alle Zeilen in der Tabelle oder nur die Zeilen zählen, die eine bestimmte Bedingung erfüllen. Weitere Informationen finden Sie unter [Zählen der Zeilen in einer Tabelle &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/count-rows-in-a-table-visual-database-tools.md).  
  
  
Wenn Sie für alle Zeilen in einer Tabelle einen einzigen Aggregatwert erstellen, werden nur die Aggregatwerte selbst angezeigt. Wenn Sie z. B. die Gesamtsumme des Werts für die `price` -Spalte in der Tabelle `titles` berechnen, werden die einzelnen Buchtitel, Herausgebernamen usw. nicht angezeigt.  
 
 **!** Wenn Sie Teilergebnisse berechnen, d.h. Gruppen erstellen, können Sie die Spaltenwerte für jede Gruppe anzeigen. Weitere Informationen finden Sie unter [Gruppieren von Zeilen in Abfrageergebnissen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/group-rows-in-query-results-visual-database-tools.md).  

## <a name="aggregate-values-for-all-rows"></a>Aggregieren von Werten für alle Zeilen  
  
1.  Stellen Sie sicher, dass die Tabelle, die Sie aggregieren möchten, bereits im Diagrammbereich angezeigt wird.  
  
2.  Klicken Sie mit der rechten Maustaste auf den Hintergrund des Diagrammbereichs, und wählen Sie im Kontextmenü die Option **Gruppieren nach** aus. Der [Abfrage- und Sicht-Designer](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) fügt dem Datenblatt im Kriterienbereich die Spalte **Gruppieren nach** hinzu.  
  
3.  Fügen Sie dem Kriterienbereich die Spalte hinzu, die aggregiert werden soll. Vergewissern Sie sich, dass die Spalte für die Ausgabe ausgewählt ist.  
  
    Der Abfrage- und Sicht-Designer weist der Spalte, die zusammengefasst wird, automatisch einen Spaltenalias zu. Sie können diesen Alias durch einen aussagekräftigeren Alias ersetzen. Ausführliche Informationen finden Sie unter [Erstellen von Spaltenaliasen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-column-aliases-visual-database-tools.md).  
  
4.  Wählen Sie in der **Gruppieren nach**-Rasterspalte die gewünschte Aggregatfunktion aus, z. B.: **SUM**, **AVG**, **MIN**, **MAX**, **COUNT**. Wenn im Resultset nur eindeutige Zeilen aggregiert werden sollen, wählen Sie eine Aggregatfunktion mit den DISTINCT-Optionen aus, z. B. **Min Distinct**. Verwenden Sie nicht **Gruppieren nach**, **Ausdruck** oder **Wobei**, da diese Optionen bei einer Aggregation aller Zeilen nicht anwendbar sind.  
  
    Der Abfrage- und Sicht-Designer ersetzt den Spaltennamen in der Anweisung im [SQL-Bereich](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md) durch die angegebene Aggregatfunktion. Die SQL-Anweisung könnte z. B. folgendermaßen aussehen:  
  
    ```  
    SELECT SUM(price)  
    FROM titles  
    ```  
  
5.  Wenn Sie mehrere Aggregationen in der Abfrage erstellen möchten, wiederholen Sie die Schritte 3 und 4.  
  
    Wenn Sie der Liste der Abfrageergebnisse oder der ORDER BY-Liste eine weitere Spalte hinzufügen, wird vom Abfrage- und Sicht-Designer automatisch der Begriff **Gruppieren nach** in die **Gruppieren nach** Spalte des Rasters eingefügt. Wählen Sie die entsprechende Aggregatfunktion aus.  
  
6.  Fügen Sie bei Bedarf Suchbedingungen hinzu, um die Teilmenge der zusammenzufassenden Zeilen anzugeben.  
  
Bei Ausführung der Abfrage werden die angegebenen Aggregationen im Ergebnisbereich angezeigt.  
  
> [!NOTE]  
> Der Abfrage- und Sicht-Designer behält die Aggregatfunktionen so lange als Bestandteil der SQL-Anweisung im SQL-Bereich bei, bis Sie den Gruppieren nach-Modus explizit ausschalten. Wenn Sie daher den Typ der Abfrage ändern oder ändern, welche Tabellen bzw. Tabellenwertobjekte im Diagrammbereich vorhanden sind, kann die resultierende Abfrage unter Umständen ungültige Aggregatfunktionen enthalten.  
  
## <a name="see-also"></a>Weitere Informationen  
[Sortieren und Gruppieren von Abfrageergebnissen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md)  
[Zusammenfassen von Abfrageergebnissen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/summarize-query-results-visual-database-tools.md)  
  
