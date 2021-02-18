---
description: Angeben mehrerer Suchbedingungen für eine Spalte (Visual Database Tools)
title: Angeben mehrerer Suchbedingungen für eine Spalte
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- search criteria [SQL Server], multiple conditions
- multiple search conditions
- search conditions [SQL Server], multiple
- OR operator
- AND, Criteria pane
ms.assetid: 2c006e36-56b1-4992-89b4-c6c0b19808f3
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: ddb90eb0beaabac86e8d422fac24fa3cf8c66168
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346069"
---
# <a name="specify-multiple-search-conditions-for-one-column-visual-database-tools"></a>Angeben mehrerer Suchbedingungen für eine Spalte (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
In manchen Fällen kann es sinnvoll sein, mehrere Suchkriterien auf dieselbe Datenspalte anzuwenden. Auf diese Weise können Sie z. B. folgende Vorgänge durchführen:  
  
-   Sie können in der Tabelle `employee` nach mehreren verschiedenen Namen oder nach Mitarbeitern in verschiedenen Gehaltsgruppen suchen. Diese Art von Suche erfordert die Verwendung einer OR-Bedingung.  
  
-   Sie können nach einem Buchtitel suchen, der mit dem Wort "Der" beginnt und das Wort "Koch" enthält. Diese Art von Suche erfordert die Verwendung einer AND-Bedingung.  
  
> [!NOTE]  
> Die Angaben zu diesem Thema beziehen sich auf Suchbedingungen in WHERE- und HAVING-Klauseln einer Abfrage. Die Beispiele behandeln vorrangig die Erstellung von WHERE-Klauseln, aber die Regeln sind auf beide Arten von Suchbedingungen anwendbar.  
  
Zum Suchen nach verschiedenen Werten in derselben Datenspalte wird eine OR-Bedingung eingesetzt. Für die Suche nach Werten, die mehrere Bedingungen erfüllen, wird eine AND-Bedingung festgelegt.  
  
## <a name="specifying-an-or-condition"></a>Angeben einer OR-Bedingung  
Mit einer OR-Bedingung können Sie mehrere verschiedene Werte angeben, nach denen in einer Spalte gesucht werden soll. Diese Möglichkeit erweitert den Bereich der Suche, und es werden unter Umständen mehr Zeilen als bei der Angabe eines einzelnen Werts zurückgegeben.  
  
> [!TIP]  
> Wenn Sie nach mehreren Werten in derselben Spalte suchen, können Sie häufig auch den Operator IN verwenden.  
  
#### <a name="to-specify-an-or-condition"></a>So geben Sie eine OR-Bedingung an  
  
1.  Fügen Sie dem [Kriterienbereich](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md)die Spalte für die Suche hinzu.  
  
2.  Geben Sie in der Spalte **Filter** für die soeben hinzugefügte Datenspalte die erste Bedingung an.  
  
3.  Geben Sie in der Spalte **Oder...** für dieselbe Datenspalte die zweite Bedingung an.  
  
Der Abfrage- und Sicht-Designer erstellt eine WHERE-Klausel mit einer OR-Bedingung, z. B.:  
  
```  
SELECT fname, lname  
FROM employees  
WHERE (salary < 30000) OR (salary > 100000)  
```  
  
## <a name="specifying-an-and-condition"></a>Angeben einer AND-Bedingung  
Mit einer AND-Bedingung können Sie angeben, dass die Werte in einer Spalte zwei oder mehr Bedingungen erfüllen müssen, damit die entsprechende Zeile in das Resultset aufgenommen wird. Diese Möglichkeit schränkt den Bereich der Suche ein, sodass normalerweise weniger Zeilen als bei der Suche nach einem einzigen Wert zurückgegeben werden.  
  
> [!TIP]  
> Wenn Sie nach einem Wertebereich suchen, können Sie den Operator BETWEEN verwenden, anstatt zwei Bedingungen mit AND zu verknüpfen.  
  
#### <a name="to-specify-an-and-condition"></a>So geben Sie eine AND-Bedingung an  
  
1.  Fügen Sie dem Kriterienbereich die Spalte für die Suche hinzu.  
  
2.  Geben Sie in der Spalte **Filter** für die soeben hinzugefügte Datenspalte die erste Bedingung an.  
  
3.  Fügen Sie dem Kriterienbereich noch einmal dieselbe Datenspalte hinzu, und platzieren Sie diese in einer leeren Zeile des Datenblatts.  
  
4.  Geben Sie in der Spalte **Filter** für die zweite Instanz der Datenspalte die zweite Bedingung an.  
  
Der Abfrage-Designer erstellt eine WHERE-Klausel mit einer AND-Bedingung, z. B.:  
  
```  
SELECT title_id, title  
FROM titles  
WHERE (title LIKE '%Cook%') AND   
  (title LIKE '%Recipe%')  
```  
  
## <a name="see-also"></a>Weitere Informationen  
[Konventionen für das Kombinieren von Suchbedingungen im Kriterienbereich &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/conventions-combine-search-conditions-in-criteria-pane-visual-db-tools.md)  
[Angeben von Suchkriterien &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  
