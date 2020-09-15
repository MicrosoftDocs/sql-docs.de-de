---
description: Hinzufügen neuer Zeilen im Ergebnisbereich (Visual Database Tools)
title: Hinzufügen von Zeilen im Ergebnisbereich
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- View Designer, Results pane
- inserting rows
- Query Designer [SQL Server], Results pane
- Results pane
- adding rows
- row additions [SQL Server], Results pane
ms.assetid: 59891c84-3f54-4ab9-8b86-72c59627b480
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.openlocfilehash: c9e976858478d896a1057cc561a2e61a540d0cfe
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88370236"
---
# <a name="add-new-rows-in-the-results-pane-visual-database-tools"></a>Hinzufügen neuer Zeilen im Ergebnisbereich (Visual Database Tools)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Sie können neue Daten hinzufügen, indem Sie sie eintippen oder aus einem anderen Programm, z. B. Editor oder Excel, einfügen. Eine einzufügende Zeile muss genau dieselbe Anzahl und denselben Typ von Spalten wie die Tabelle aufweisen, in die Sie die Zeile einfügen. Sie können mehrere Zeilen gleichzeitig in den Ergebnisbereich einfügen.  
  
Informationen zum Eingeben von Daten finden Sie unter [Regeln zum Aktualisieren von Ergebnissen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/rules-for-updating-results-visual-database-tools.md).  
  
### <a name="to-add-a-new-data-row"></a>So fügen Sie eine neue Datenzeile ein  
  
1.  Wechseln Sie in den unteren Teil des Ergebnisbereichs, in dem eine leere Zeile für das Hinzufügen einer neuen Datenzeile verfügbar ist.  
  
    Die Anfangswerte aller Spalten sind *NULL*.  
  
    > [!TIP]  
    > Um sofort zur ersten Leerzeile zu wechseln, benutzen Sie die Navigationsleiste am unteren Rand des Ergebnisbereichs.  
  
2.  Wenn Sie Zeilen aus der Zwischenablage einfügen, markieren Sie die neue Zeile, indem Sie auf die Schaltfläche auf der linken Seite klicken.  
  
    > [!NOTE]  
    > Wenn eine oder mehrere der eingefügten Zeilen nicht in die Datenbank eingetragen werden können, wird eine Meldung ausgegeben, die darüber Auskunft gibt, welche Zeile(n) nicht eingetragen werden konnte(n).  
  
3.  Geben Sie die Daten für die neue Zeile ein. Beim Einfügen aus der Zwischenablage wählen Sie im Menü **Bearbeiten** die Option **Einfügen** aus.  
  
4.  Bewegen Sie den Zeiger aus der Zeile heraus, damit sie in die Datenbank eingetragen wird.  
  
Falls beim Speichern der Zeile ein Fehler auftritt, zeigt der Abfrage- und Sicht-Designer eine Meldung an und setzt den Zeiger in die Zeile, die Sie in Bearbeitung hatten. Sie können anschließend folgende Aktionen durchführen:  
  
-   Auflösen des Fehlers durch weitere Bearbeitungen der Zeile.  
  
-   Abbrechen der Bearbeitung durch Drücken von ESC. Wenn Sie sich beim Drücken von ESC in einer geänderten Zelle befinden, werden die Änderungen an dieser Zelle abgebrochen. Befinden Sie sich beim Drücken von ESC in einer nicht geänderten Zelle, werden die Änderungen für die gesamte Zeile verworfen.  
  
## <a name="see-also"></a>Weitere Informationen  
[Verwenden von Daten im Ergebnisbereich &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/work-with-data-in-the-results-pane-visual-database-tools.md)  
[Ausführen grundlegender Vorgänge mit Abfragen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/perform-basic-operations-with-queries-visual-database-tools.md)  
  
