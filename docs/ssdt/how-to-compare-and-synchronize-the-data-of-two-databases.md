---
title: Vergleichen und Synchronisieren von Daten aus zwei Datenbanken
description: In diesem Artikel wird erläutert, wie Sie die Daten aus zwei verschiedenen Datenbanken vergleichen und synchronisieren. Zudem erfahren Sie, wie Sie den Vergleich einrichten, die Unterschiede anzeigen und das Ziel aktualisieren.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.datacompare.connection.datasources.f1
- sql.data.tools.datacompare.watermark.f1
- sql.data.tools.datacompare.connection.objectselection.f1
ms.assetid: 2148e517-ed42-41c6-b753-1ac625f594c8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 73dcdd57b018f980291eceefa1be37d1b51df13a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018440"
---
# <a name="how-to-compare-and-synchronize-the-data-of-two-databases"></a>Gewusst wie: Vergleichen und Synchronisieren von Daten aus zwei Datenbanken

Sie können die Daten aus zwei Datenbanken vergleichen. Die zu vergleichenden Datenbanken werden als *Quelle* und *Ziel* bezeichnet.  
  
> [!NOTE]  
> *Datenbankprojekte* sowie DACPAC- oder BACPAC-Pakete können nicht als Quelle oder Ziel für einen Datenvergleich verwendet werden.  
  
Im Zuge des Datenvergleichs wird ein Skript in der *Datenbearbeitungssprache (Data Manipulation Language, DML)* generiert, mit dessen Hilfe Sie die unterschiedlichen Datenbanken mittels Aktualisierung einiger oder sämtlicher Daten in der Zieldatenbank synchronisieren können. Die Ergebnisse eines abgeschlossenen Datenvergleichs erscheinen im Datenvergleichsfenster von Visual Studio.  
  
Nach Abschluss des Vergleichs haben Sie folgende Möglichkeiten:  
  
-   Sie können die Unterschiede zwischen den beiden Datenbanken anzeigen. Weitere Informationen finden Sie unter [Anzeigen von Datenunterschieden](#ViewDifferences).  
  
-   Sie können das Ziel ganz oder teilweise aktualisieren, sodass es der Quelle entspricht. Weitere Informationen finden Sie unter [Synchronisieren von Datenbankdaten](#Synchronize).  
  
Weitere Informationen finden Sie unter [Vergleichen und Synchronisieren von Daten in einer oder mehreren Tabellen anhand von Daten aus einer Verweisdatenbank](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
> [!NOTE]  
> Sie können auch das *Schema* von zwei Datenbanken oder von zwei Versionen der gleichen Datenbank vergleichen. Weitere Informationen finden Sie unter [Vorgehensweise: Vergleichen verschiedener Datenbankdefinitionen mithilfe des Schemavergleichs](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md).  
  
## <a name="comparing-database-data"></a><a name="CompareDatabaseData"></a>Vergleichen von Datenbankdaten  
  
#### <a name="to-compare-data-by-using-the-new-data-comparison-wizard"></a>So vergleichen Sie Daten mithilfe des Datenvergleichs-Assistenten  
  
1.  Zeigen Sie im Menü **SQL** auf **Datenvergleich**, und klicken Sie anschließend auf **Neuer Datenvergleich**.  
  
    Der Datenvergleichs-Assistent erscheint. Außerdem wird das Datenvergleichsfenster geöffnet und von Visual Studio automatisch mit einem Namen wie DataCompare1 versehen.  
  
2.  Geben Sie die Quell- und die Zieldatenbank an.  
  
    Ist die Liste **Quelldatenbank** oder die Liste **Zieldatenbank** leer, klicken Sie auf **Neue Verbindung**. Geben Sie im Dialogfeld **Verbindungseigenschaften** den Server mit der Datenbank sowie die Art der Authentifizierung an, die beim Herstellen einer Verbindung mit der Datenbank verwendet werden soll. Klicken Sie anschließend auf **OK**, um das Dialogfeld **Verbindungseigenschaften** zu schließen und zum Datenvergleichs-Assistenten zurückzukehren.  
  
    Vergewissern Sie sich auf der ersten Seite des Datenvergleichs-Assistenten, dass die Informationen zu den einzelnen Datenbanken korrekt sind, geben Sie die in die Ergebnisse einzubeziehenden Datensätze an, und klicken Sie anschließend auf **Weiter**. Die zweite Seite des Datenvergleichs-Assistenten erscheint. Hier finden Sie eine hierarchische Auflistung der Tabellen und Sichten in der Datenbank.  
  
3.  Aktivieren Sie die Kontrollkästchen der zu vergleichenden Tabellen und Sichten. Optional können Sie auch die Knoten für Datenbankobjekte erweitern und dann die Kontrollkästchen der zu vergleichenden Spalten innerhalb dieser Objekte aktivieren.  
  
    > [!NOTE]  
    > Tabellen und Sichten müssen zwei Kriterien erfüllen, damit sie in der Auflistung erscheinen. Erstens: Die Schemas der Objekte müssen in Quell- und Zieldatenbank gleich sein. Zweitens: In der Liste werden nur Tabellen und Sichten mit Primärschlüssel, eindeutigem Schlüssel, eindeutigem Index oder eindeutiger Einschränkung angezeigt. Werden diese beiden Kriterien von keiner Tabelle oder Sicht erfüllt, ist die Liste leer.  
  
4.  Sind mehrere Schlüssel vorhanden, können Sie mithilfe der Spalte **Vergleichsschlüssel** den Schlüssel angeben, auf dem der Datenvergleich basieren soll. So können Sie beispielsweise angeben, ob der Vergleich auf der Primärschlüsselspalte oder auf einer anderen Spalte mit (eindeutig identifizierbarem) Schlüssel basieren soll.  
  
5.  Klicken Sie auf **Fertig stellen**.  
  
    Der Vergleich beginnt.  
  
    > [!NOTE]  
    > Wenn Sie einen gerade ausgeführten Datenvergleich beenden möchten, öffnen Sie das Menü **SQL**, klicken Sie auf **Datenvergleich**, und klicken Sie anschließend auf **Datenvergleich beenden**.  
  
    Nach Abschluss des Vergleichs können Sie die Unterschiede zwischen den beiden Datenbanken anzeigen. Außerdem können Sie die Daten in der Zieldatenbank ganz oder teilweise aktualisieren, sodass sie den Daten in der Quelldatenbank entsprechen.  
  
#### <a name="to-compare-data-by-using-the-visual-studio-automation-model"></a>So vergleichen Sie Daten mithilfe des Visual Studio-Automatisierungsmodells  
  
1.  Zeigen Sie im Menü **Ansicht** auf **Weitere Fenster**, und klicken Sie auf **Befehlsfenster**.  
  
2.  Geben Sie im Befehlsfenster folgenden Befehl ein:  
  
    ```  
    Sql.NewDataComparison /SrcServerName sServerName /SrcDatabaseName sDatabaseName /SrcUserName sUserName /SrcPassword sPassword /SrcDisplayName sDisplayName /TargetServerName tServerName /TargetDatabaseName tDatabaseName /TargeUserName tUserName /TargetPassword tPassword /TargetDisplayName tDisplayName  
    ```  
  
    Ersetzen Sie die Platzhalter (*sServerName*, *sDatabaseName*, *sUserName*, *sPassword*, *sDisplayName*, *tServerName*, *tDatabaseName*, *tUserName*, *tPassword* und *tDisplayName*) durch die Werte für Ihre Quell- und Zieldatenbanken.  
  
    Falls Sie keine Quelle und kein Ziel angeben, wird das Dialogfeld **Neuer Datenvergleich** angezeigt. Weitere Informationen zu den Parametern für den Befehl „Sql.NewDataComparison“ finden Sie unter [Referenz der Automatisierungsbefehle für Datenbankfunktionen von Visual Studio](/previous-versions/visualstudio/visual-studio-2010/dd470565(v=vs.100)).  
  
    Die Daten in der angegebenen Quell- und Zieldatenbank werden verglichen. Die Ergebnisse erscheinen in einer Datenvergleichssitzung. Weitere Informationen zum Anzeigen von Ergebnissen oder zum Synchronisieren von Daten finden Sie unter [Anzeigen von Datenunterschieden](#ViewDifferences) bzw. unter [Synchronisieren von Datenbankdaten](#Synchronize).  
  
## <a name="viewing-data-differences"></a><a name="ViewDifferences"></a>Anzeigen von Datenunterschieden  
Nach dem Vergleich der Daten in zwei Datenbanken werden beim Datenvergleich alle verglichenen *Datenbankobjekte* sowie deren Status aufgeführt. Sie können auch Ergebnisse für die Datensätze in den einzelnen Objekten (gruppiert nach Status) anzeigen. Weitere Informationen zu den Zustandsbezeichnungen finden Sie unter [Vergleichen und Synchronisieren von Daten in einer oder mehreren Tabellen anhand von Daten aus einer Verweisdatenbank](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
Nachdem Sie sich die Unterschiede angesehen haben, können Sie einige oder alle unterschiedlichen, fehlenden oder neuen Objekte oder Datensätze des Ziels aktualisieren, sodass sie der Quelle entsprechen. Weitere Informationen finden Sie unter [Synchronisieren von Datenbankdaten](#Synchronize).  
  
#### <a name="to-view-data-differences"></a>So zeigen Sie Datenunterschiede an  
  
1.  Vergleichen Sie die Daten einer Quell- und einer Zieldatenbank. Weitere Informationen finden Sie unter [Vergleichen von Datenbankdaten](#CompareDatabaseData).  
  
2.  (Optional) Führen Sie einen oder beide der folgenden Schritte aus:  
  
    -   Standardmäßig werden die Ergebnisse für alle Objekte (unabhängig vom jeweiligen Status) angezeigt. Wenn nur Objekte mit einem bestimmten Status angezeigt werden sollen, klicken Sie auf eine Option in der Liste **Filter**.  
  
    -   Wenn Sie Ergebnisse für Datensätze innerhalb eines bestimmten Objekts anzeigen möchten, klicken Sie im Hauptergebnisbereich auf das gewünschte Objekt, und klicken Sie anschließend im Bereich mit der Datensatzansicht auf eine Registerkarte. Jede Registerkarte zeigt sämtliche Datensätze innerhalb des Objekts, die einen bestimmten Status besitzen: Unterschiedliche Datensätze, Nur in der Quelle, Nur im Ziel oder Identische Datensätze. Die Daten sind nach Datensatz und Spalte sortiert.  
  
## <a name="synchronizing-database-data"></a><a name="Synchronize"></a>Synchronisieren von Datenbankdaten  
Nach dem Vergleich der Daten in zwei Datenbanken können Sie die Datenbanken synchronisieren, indem Sie das Ziel ganz oder teilweise aktualisieren, sodass es der Quelle entspricht. Sie können Daten aus zwei Arten von Datenbankobjekten vergleichen: Tabellen und Sichten.  
  
#### <a name="to-update-target-data-by-using-the-write-updates-command"></a>So aktualisieren Sie Zieldaten mithilfe des Befehls "Updates schreiben"  
  
1.  Vergleichen Sie die Daten einer Quell- und einer Zieldatenbank. Weitere Informationen finden Sie unter [Vergleichen von Datenbankdaten](#CompareDatabaseData).  
  
    Nach Abschluss des Vergleichs werden im Datenvergleichsfenster die Ergebnisse für die verglichenen Objekte aufgeführt. In vier Spalten (Unterschiedliche Datensätze, Nur in der Quelle, Nur im Ziel und Identische Datensätze) werden Informationen zu nicht übereinstimmenden Objekten angezeigt. Die Spalten geben für jedes dieser Objekte Aufschluss darüber, wie viele nicht übereinstimmende Datensätze gefunden wurden und wie viele Datensätze bei einer Aktualisierung geändert würden. Diese beiden Zahlen sind zunächst identisch. In Schritt 4 können Sie jedoch festlegen, welche Objekte aktualisiert werden sollen.  
  
    Weitere Informationen finden Sie unter [Vergleichen und Synchronisieren von Daten in einer oder mehreren Tabellen anhand von Daten aus einer Verweisdatenbank](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
2.  Klicken Sie in der Tabelle des Datenvergleichsfensters auf eine Zeile.  
  
    Im Detailbereich werden Ergebnisse für die Datensätze des Datenbankobjekts angezeigt, auf das Sie geklickt haben. Die Datensätze sind mithilfe von Registerkarten nach Status gruppiert. Über diese Registerkarten können Sie die Daten aus der Quelle angeben, die in das Ziel übertragen werden sollen.  
  
3.  Klicken Sie im Detailbereich auf eine Registerkarte, deren Name eine Zahl enthält, bei der es sich nicht um die Zahl Null handelt.  
  
    Die Spalte **Aktualisieren** der Tabelle **Nur im Ziel** enthält Kontrollkästchen, mit deren Hilfe Sie die zu aktualisierenden Zeilen auswählen können. Standardmäßig ist dieses Kontrollkästchen aktiviert.  
  
4.  Deaktivieren Sie die Kontrollkästchen der Datensätze im Ziel, die nicht mit Daten aus der Quelle aktualisiert werden sollen.  
  
    Wenn Sie ein Kontrollkästchen deaktivieren, verringert sich dadurch die Anzahl der zu aktualisierenden Datensätze, und die Anzeige wird entsprechend angepasst. Diese Zahl erscheint in der Statuszeile des Detailbereichs sowie in der entsprechenden Spalte im Hauptergebnisbereich (siehe Schritt 1).  
  
5.  (Optional) Klicken Sie auf **Skript generieren**.  
  
    Ein Transact\-SQL-Editorfenster wird geöffnet. Es enthält das Skript in der *Datenbearbeitungssprache (Data Manipulation Language, DML)* , das zum Aktualisieren des Ziels verwendet wird.  
  
6.  Klicken Sie auf **Ziel aktualisieren**, um unterschiedliche, fehlende oder neue Datensätze zu synchronisieren.  
  
    > [!NOTE]  
    > Die Aktualisierung der Zieldatenbank kann durch Klicken auf **Schreiben auf Ziel beenden** abgebrochen werden.  
  
    Die Daten der ausgewählten Datensätze im Ziel werden mit den Daten aus den entsprechenden Datensätzen der Quelle aktualisiert.  
  
    > [!NOTE]  
    > Wenn Sie sich für die Aktualisierung indizierter Sichten entscheiden, tritt beim Vorgang **Ziel aktualisieren** unter Umständen ein Fehler auf, falls diese Aktion dazu führt, dass doppelte Schlüssel in die gleiche Tabelle eingefügt werden.  
  
#### <a name="to-update-target-data-by-using-a-transact-sql-script"></a>So aktualisieren Sie Zieldaten mithilfe eines Transact\-SQL-Skripts  
  
1.  Vergleichen Sie die Daten einer Quell- und einer Zieldatenbank. Weitere Informationen finden Sie unter [Vergleichen von Datenbankdaten](#CompareDatabaseData).  
  
    Nach Abschluss des Vergleichs werden im Datenvergleichsfenster die verglichenen Objekte aufgeführt. Weitere Informationen finden Sie unter [Vergleichen und Synchronisieren von Daten in einer oder mehreren Tabellen anhand von Daten aus einer Verweisdatenbank](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
2.  (Optional) Deaktivieren Sie im Detailbereich die Kontrollkästchen der nicht zu aktualisierenden Datensätze im Ziel, wie im vorherigen Verfahren beschrieben.  
  
3.  Klicken Sie auf **Skript generieren**.  
  
    Ein neues Fenster zeigt das Transact\-SQL-Skript zum Übertragen der erforderlichen Änderungen, mit denen dafür gesorgt wird, dass die Daten im Ziel den Daten aus der Quelle entsprechen. Das neue Fenster wird mit einer Bezeichnung wie **DataUpdate_Database_1.sql** versehen.  
  
    Dieses Skript spiegelt die im Detailbereich vorgenommenen Änderungen wider. Beispielsweise haben Sie dort vielleicht auf der Seite Nur im Ziel das Kontrollkästchen einer bestimmten Zeile für die Tabelle [dbo].[Shippers] deaktiviert. In diesem Fall wird diese Zeile durch das Skript nicht aktualisiert.  
  
4.  (Optional) Bearbeiten Sie das Skript im Fenster **DataUpdate_Database_1.sql**.  
  
5.  (Optional, aber empfohlen) Sichern Sie die Zieldatenbank.  
  
6.  Klicken Sie auf **Ausführen**, um die Zieldatenbank zu aktualisieren.  
  
    Geben Sie eine Verbindung mit der zu aktualisierenden Zieldatenbank an.  
  
    > [!IMPORTANT]  
    > Standardmäßig erfolgen die Aktualisierungen im Rahmen einer Transaktion. Im Falle eines Fehlers können Sie die gesamte Aktualisierung zurücksetzen. Diese Verhalten kann geändert werden.  
  
    Die Daten der ausgewählten Datensätze im Ziel werden mit den Daten aus den entsprechenden Datensätzen der Quelle aktualisiert.  
  
## <a name="see-also"></a>Weitere Informationen  
[Vergleichen und Synchronisieren von Daten in einer oder mehreren Tabellen anhand von Daten aus einer Verweisdatenbank](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md)  
