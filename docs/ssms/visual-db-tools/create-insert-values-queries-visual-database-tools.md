---
description: Erstellen von Abfragen zum Einfügen von Werten (Visual Database Tools)
title: Erstellen von Abfragen zum Einfügen von Werten
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- inserting values
- queries [SQL Server], types
- adding values
- row additions [SQL Server], Insert Values query
- inserting rows
- Insert Values query
- adding rows
- table values [SQL Server]
ms.assetid: 2d4b2f6d-cc09-434b-8a0e-ccce40628064
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 6315535cce06d228ccc8232dbd887a9bcac384bc
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88468452"
---
# <a name="create-insert-values-queries-visual-database-tools"></a>Erstellen von Abfragen zum Einfügen von Werten (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Mit einer Abfrage zum Einfügen von Werten können Sie in der aktuellen Tabelle eine neue Zeile erstellen. Beim Erstellen einer Abfrage zum Einfügen von Werten müssen folgende Angaben gemacht werden:  
  
-   Die Datenbanktabelle, der die Zeile hinzugefügt werden soll  
  
-   Die Spalten, deren Inhalt Sie hinzufügen möchten  
  
-   Der in die einzelnen Spalten einzufügende Wert oder Ausdruck  
  
Die folgende Abfrage fügt beispielsweise eine Zeile in die Tabelle `titles` ein, die Werte für den Titel, den Typ, den Herausgeber und den Preis angibt:  
  
```  
INSERT INTO titles  
         (title_id, title, type, pub_id, price)  
VALUES   ('BU9876', 'Creating Web Pages', 'business', '1389', '29.99')  
```  
  
Beim Erstellen einer Abfrage zum Einfügen von Werten ändert sich der Kriterienbereich entsprechend und zeigt die beiden Optionen an, die zum Einfügen einer neuen Zeile verfügbar sind: den Spaltennamen und den einzufügenden Wert.  
  
> [!CAUTION]  
> Eine ausgeführte Abfrage zum Einfügen von Werten kann nicht mehr rückgängig gemacht werden. Erstellen Sie vorsichtshalber vor Ausführung der Abfrage eine Sicherungskopie der Daten.  
  
### <a name="to-create-an-insert-values-query"></a>So erstellen Sie eine Abfrage zum Einfügen von Werten  
  
1.  Fügen Sie dem Diagrammbereich die zu aktualisierende Tabelle hinzu.  
  
2.  Zeigen Sie im Menü **Abfrage-Designer** auf **Typ ändern**, und klicken Sie dann auf **Werte einfügen**.  
  
    > [!NOTE]  
    > Wenn beim Starten der Abfrage zum Einfügen von Werten mehrere Tabellen im Diagrammbereich angezeigt werden, zeigt der Abfrage- und Sicht-Designer das [Dialogfeld „Zieltabelle für eingefügte Ergebnisse auswählen“](../../ssms/visual-db-tools/choose-target-table-for-insert-values-dialog-box-visual-database-tools.md) an, in dem Sie zur Eingabe des Namens der zu aktualisierenden Tabelle aufgefordert werden.  
  
3.  Aktivieren Sie im Diagrammbereich das Kontrollkästchen für die Spalten, für die Sie neue Werte eingeben wollen. Diese Spalten werden im Kriterienbereich angezeigt. Spalten werden nur aktualisiert, wenn sie der Abfrage hinzugefügt werden.  
  
4.  Geben Sie im Kriterienbereich in der Spalte **Neuer Wert** den neuen Wert für die Spalte ein. Sie können Literalwerte, Spaltennamen oder Ausdrücke eingeben. Der Wert muss dem Datentyp der zu aktualisierenden Spalte entsprechen (oder mit diesem kompatibel sein).  
  
    > [!CAUTION]  
    > Der Abfrage- und Sicht-Designer kann nicht überprüfen, ob die Länge eines Werts die zulässige Länge der Spalte überschreitet. Bei Eingabe eines zu langen Werts kann dieser ohne vorherige Warnung verkürzt werden. Wenn beispielsweise die Spalte `name` eine Länge von 20 Zeichen aufweist, Sie aber einen aus 25 Zeichen bestehenden einzufügenden Wert angeben, werden die letzten 5 Zeichen möglicherweise abgeschnitten.  
  
Beim Ausführen einer Abfrage zum Einfügen von Werten werden keine Ergebnisse im [Ergebnisbereich](../../ssms/visual-db-tools/results-pane-visual-database-tools.md)angezeigt. Stattdessen wird eine Meldung mit der Anzahl der geänderten Zeilen ausgegeben.  
  
## <a name="see-also"></a>Weitere Informationen  
[Unterstützte Abfragetypen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/supported-query-types-visual-database-tools.md)  
[Themen zur Vorgehensweise: Entwerfen von Abfragen und Sichten &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
[Ausführen grundlegender Vorgänge mit Abfragen &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/perform-basic-operations-with-queries-visual-database-tools.md)  
  
