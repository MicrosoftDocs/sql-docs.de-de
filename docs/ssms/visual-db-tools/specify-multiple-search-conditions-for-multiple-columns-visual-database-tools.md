---
description: Angeben mehrerer Suchbedingungen für mehrere Spalten (Visual Database Tools)
title: Angeben mehrerer Suchbedingungen für mehrere Spalten
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
ms.assetid: 06617729-0d0b-4da2-9890-b7e2f5cdbc7b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 4831d45d1e258855a70e8171a0d07c46a8a56d41
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350432"
---
# <a name="specify-multiple-search-conditions-for-multiple-columns-visual-database-tools"></a>Angeben mehrerer Suchbedingungen für mehrere Spalten (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Sie können den Bereich der Abfrage erweitern oder einschränken, indem Sie verschiedene Spalten in die Suchbedingung aufnehmen. Auf diese Weise können Sie z. B. folgende Vorgänge durchführen:  
  
-   Sie können nach Mitarbeitern suchen, die entweder seit mehr als fünf Jahren in der Firma arbeiten oder bestimmte Tätigkeiten ausführen.  
  
-   Sie können nach einem Buch suchen, das von einem bestimmten Herausgeber veröffentlicht wurde und gleichzeitig ein Kochbuch ist.  
  
Für einer Abfrage, die in zwei oder mehr Spalten nach Werten sucht, wird eine OR-Bedingung verwendet. Demgegenüber wird zur Erstellung einer Abfrage, die alle Bedingungen in zwei oder mehr Spalten erfüllen muss, eine AND-Bedingung eingesetzt.  
  
## <a name="specifying-an-or-condition"></a>Angeben einer OR-Bedingung  
Wenn Sie mehrere mit OR verknüpfte Bedingungen erstellen möchten, setzen Sie jede Bedingung in eine andere Spalte des Kriterienbereichs.  
  
#### <a name="to-specify-an-or-condition-for-two-different-columns"></a>So geben Sie eine OR-Bedingung für zwei verschiedene Spalten an  
  
1.  Fügen Sie dem [Kriterienbereich](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md)die Spalten hinzu, die durchsucht werden sollen.  
  
2.  Geben Sie in der Spalte **Filter** für die erste zu durchsuchende Spalte die erste Bedingung an.  
  
3.  Geben Sie in der Spalte **Oder** für die zweite zu durchsuchende Datenspalte die zweite Bedingung an, und lassen Sie die Spalte **Filter** leer.  
  
    Der Abfrage- und Sicht-Designer erstellt eine WHERE-Klausel mit einer OR-Bedingung, z. B.:  
  
    ```  
    SELECT job_lvl, hire_date  
    FROM employee  
    WHERE (job_lvl >= 200) OR   
      (hire_date < '01/01/1998')  
    ```  
  
4.  Wiederholen Sie die Schritte 2 und 3 für jede weitere Bedingung, die hinzugefügt werden soll. Verwenden Sie für jede neue Bedingung eine neue Spalte **Oder...** .  
  
## <a name="specifying-an-and-condition"></a>Angeben einer AND-Bedingung  
Um verschiedene Datenspalten nach Bedingungen zu durchsuchen, die mit AND verknüpft sind, setzen Sie alle Bedingungen in die Datenblattspalte **Filter** .  
  
#### <a name="to-specify-an-and-condition-for-two-different-columns"></a>So geben Sie eine AND-Bedingung für zwei verschiedene Spalten an  
  
1.  Fügen Sie dem [Kriterienbereich](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md)die Spalten hinzu, die durchsucht werden sollen.  
  
2.  Geben Sie in der Spalte **Filter** für die erste zu durchsuchende Datenspalte die erste Bedingung an.  
  
3.  Geben Sie in der Spalte **Filter** für die zweite Datenspalte die zweite Bedingung an.  
  
    Der Abfrage- und Sicht-Designer erstellt eine WHERE-Klausel mit einer AND-Bedingung, z. B.:  
  
    ```  
    SELECT pub_id, title  
    FROM titles  
    WHERE (pub_id = '0877') AND (title LIKE '%Cook%')  
    ```  
  
4.  Wiederholen Sie die Schritte 2 und 3 für jede weitere Bedingung, die hinzugefügt werden soll.  
  
## <a name="see-also"></a>Siehe auch  
[Kombinieren von Bedingungen, wenn AND Vorrang hat](../../ssms/visual-db-tools/combine-conditions-when-and-has-precedence-visual-database-tools.md)  
[Kombinieren von Bedingungen, wenn OR Vorrang hat](../../ssms/visual-db-tools/combine-conditions-when-or-has-precedence-visual-database-tools.md)  
[Konventionen für das Kombinieren von Suchbedingungen im Kriterienbereich](../../ssms/visual-db-tools/conventions-combine-search-conditions-in-criteria-pane-visual-db-tools.md)  
[Angeben von Suchkriterien](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  
