---
title: SSMS-Abfrage-Editor
description: SSMS-Abfrage-Editor (SQL Server Management Studio)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.query.advanced.f1
- sql13.swb.query.ansi.f1
- sql13.swb.query.general.f1
- sql13.swb.query.grid.f1
- sql13.swb.sqleditors.multiserverresultssettings
- sql13.swb.tsqlquery.f1
- sql13.swb.tsqlresults.f1
dev_langs:
- TSQL
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], editor
- SQL Server Management Studio [SQL Server], Database Engine Query Editor
- SQL Server Management Studio [SQL Server], templates
- SQL Server Management Studio [SQL Server], query editor
- Query Editor [SQL Server Management Studio]
- Query Editor [Database Engine]
- Query Editor [Database Engine], Features
- Query Editor [Database Engine], Toolbar
- Query Editor [SQL Server Management Studio], full screen mode
- Query Editor [Database Engine], templates
- Query Editor [SQL Server Management Studio], about Query Editor
- Transact-SQL Editor See Query Editor [Database Engine]
- Database Engine Query Editor See Query Editor [Database Engine]
- Code Editor [SQL Server Management Studio], about Query Editor
- editors [SQL Server Management Studio], Database Engine Query Editor
- full screen mode [SQL Server Management Studio]
- writing scripts
- modifying scripts
- writing queries
- scripts [SQL Server], SQL Server Management Studio
- queries [SQL Server], SQL Server Management Studio
ms.assetid: 05cfae9b-96d5-4a35-a098-0bc3a548bcfc
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019, contperf-fy21q1
ms.date: 08/28/2020
ms.openlocfilehash: 6ce879af451abb20e108e20b36fbe129024f4806
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355162"
---
# <a name="sql-server-management-studio-ssms-query-editor"></a>SSMS-Abfrage-Editor (SQL Server Management Studio)

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

In diesem Artikel werden die Features und Funktionen des Abfrage-Editors in SQL Server Management Studio (SSMS) erläutert.

> [!Note]
> Wenn Sie erfahren möchten, wie Sie die F1-Hilfe für Transact-SQL (T-SQL) verwenden, finden Sie weitere Informationen im Abschnitt [F1-Hilfe für Transact-SQL](#transact-sql-f1-help).
>
> Wenn Sie mehr über Aufgaben erfahren möchten, die Sie mit dem Editor erledigen können, besuchen Sie den Abschnitt [Editor-Aufgaben](#editor-tasks).

Die Editoren in SSMS besitzen eine typische Architektur. Der Text-Editor implementiert die Basisfunktionalitätsstufe und kann als grundlegender Editor für Textdateien verwendet werden. Die anderen Editoren oder Abfrage-Editoren erweitern diese Funktionsbasis, indem sie einen Sprachdienst einschließen, der die Syntax von einer der in SQL Server unterstützten Sprachen definiert. Die Abfrage-Editoren implementieren darüber hinaus verschiedene Ebenen der Unterstützung für Editor-Funktionen, z. B. IntelliSense und Debugging. Die Abfrage-Editoren beinhalten den Abfrage-Editor der Datenbank-Engine zur Verwendung bei der Erstellung von Skripts mit T-SQL- und XQuery-Anweisungen, den MDX-Editor für die MDX-Sprache, den DMX-Editor für die DMX-Sprache sowie den XML/A-Editor für die XML for Analysis-Sprache.
Sie können den Abfrage-Editor verwenden, um Skripts mit Transact-SQL-Anweisungen zu erstellen und auszuführen.

![Neue Abfrage](media/database-engine-query-editor-sql-server-management-studio/new-query.png)

## <a name="sql-editor-toolbar"></a>SQL-Editor-Symbolleiste

Wenn der Abfrage-Editor geöffnet ist, wird die SQL-Editor-Symbolleiste mit den folgenden Schaltflächen angezeigt.

Sie können die SQL-Editor-Symbolleiste auch hinzufügen, indem Sie im Menü **Ansicht** nacheinander **Symbolleisten** und **SQL-Editor** auswählen. Wenn Sie die SQL-Editor-Symbolleiste hinzufügen, ohne dass ein Fenster des Abfrage-Editors geöffnet ist, steht keine der Schaltflächen zur Verfügung.

![Editor-Symbolleiste](media/database-engine-query-editor-sql-server-management-studio/editor-toolbar.png)

### <a name="connect-using-the-editor-toolbar"></a>Herstellen einer Verbindung über die Editor-Symbolleiste

Öffnet das Dialogfeld **[Verbindung mit Server herstellen](connect-to-server-database-engine.md)** . Mithilfe dieses Dialogfelds können Sie eine Verbindung mit einem Server herstellen.

Sie können auch über das [Kontextmenü](#connection-using-the-context-menu) eine Verbindung mit Ihrer Datenbank herstellen.

### <a name="change-connection-using-the-editor-toolbar"></a>Ändern einer Verbindung über die Editor-Symbolleiste

Öffnet das Dialogfeld **Verbindung mit Server herstellen** . Über dieses Dialogfeld können Sie eine [Verbindung mit einem anderen Server](f1-help-for-server-connections-sql-server-management-studio.md) herstellen.

Sie können Verbindungen auch über das [Kontextmenü](#connection-using-the-context-menu) ändern.

### <a name="available-databases-using-the-editor-toolbar"></a>Verfügbare Datenbanken bei Verwendung der Editor-Symbolleiste

Wechselt die Verbindung zu einer anderen Datenbank auf demselben Server.

### <a name="execute-using-the-editor-toolbar"></a>Ausführen über die Editor-Symbolleiste

Führt den ausgewählten Code aus bzw. den gesamten Code im Abfrage-Editor, wenn kein Code ausgewählt ist.

Sie können eine Abfrage auch durch Drücken der Taste F5 oder über das [Kontextmenü](#execute-using-the-context-menu) **ausführen**.

### <a name="cancel-executing-query-using-the-editor-toolbar"></a>Abbrechen der Abfrageausführung über die Editor-Symbolleiste

Sendet eine Abbruchsanforderung an den Server. Einige Abfragen können nicht sofort abgebrochen werden, sondern müssen auf eine geeignete Abbruchbedingung warten. Beim Abbruch von Transaktionen können Verzögerungen auftreten, während für die Transaktionen ein Rollback ausgeführt wird.

Sie können die Ausführung einer Abfrage auch mithilfe der Tastenkombination ALT+UNTBR abbrechen.

### <a name="parse-using-the-editor-toolbar"></a>Analysieren über die Editor-Symbolleiste

Überprüft die Syntax des ausgewählten Codes. Wenn kein Code ausgewählt ist, wird die gesamte Syntax des Codes im Fenster des Abfrage-Editors überprüft.

Sie können den Code im Abfrage-Editor auch prüfen, indem Sie die Tastenkombination STRG+F5 drücken.

### <a name="display-estimated-execution-plan-using-the-editor-toolbar"></a>Anzeigen des geschätzten Ausführungsplan über die Editor-Symbolleiste

Fordert einen Abfrageausführungsplan vom Abfrageprozessor an, ohne die Abfrage auszuführen. Der Plan wird im Fenster **Ausführungsplan** angezeigt. Dieser Plan verwendet Indexstatistiken, um die Anzahl der Zeilen zu schätzen, die während jedes Teils der Abfrageausführung voraussichtlich zurückgegeben werden. Der tatsächlich verwendete Abfrageplan kann sich vom geschätzten Ausführungsplan unterscheiden. Wenn die Anzahl der zurückgegebenen Zeilen von der Schätzung abweicht und der Abfrageprozessor den Plan zur effizienteren Vorgehensweise ändert, kann dies vorkommen.

Sie können einen geschätzten Ausführungsplan auch mithilfe der Tastenkombination STRG+L oder über das [Kontextmenü](#display-estimated-execution-plan-using-the-context-menu) anzeigen.

### <a name="query-options-using-the-editor-toolbar"></a>Verwenden von Abfrageoptionen über die Editor-Symbolleiste

Öffnet das Dialogfeld **Abfrageoptionen** . Mithilfe dieses Dialogfelds konfigurieren Sie die Standardoptionen für die Abfrageausführung und die Abfrageergebnisse.

Sie können **Abfrageoptionen** auch über das [Kontextmenü](#query-options-using-the-context-menu) auswählen.

### <a name="intellisense-enabled-using-the-editor-toolbar"></a>Aktivieren von IntelliSense über die Editor-Symbolleiste

Gibt an, ob die [IntelliSense](../scripting/configure-intellisense-sql-server-management-studio.md)-Funktionalität im Abfrage-Editor der Datenbank-Engine verfügbar ist. Diese Option ist standardmäßig festgelegt.

Sie können **IntelliSense aktiviert** auch auswählen, indem Sie nacheinander die Tastenkombinationen STRG+B und STRG+I drücken oder das [Kontextmenü](#intellisense-enabled-using-the-context-menu) verwenden.

### <a name="include-actual-execution-plan-using-the-editor-toolbar"></a>Einschließen des tatsächlichen Ausführungsplans über die Editor-Symbolleiste

Führt die Abfrage aus, gibt die Abfrageergebnisse zurück und verwendet den Ausführungsplan für die Abfrage. Die Abfragen werden im Fenster **Ausführungsplan** als grafischer Abfrageplan angezeigt.

Sie können die Option **Tatsächlichen Ausführungsplan einschließen** auch mithilfe der Tastenkombination STRG+M oder über das [Kontextmenü](#include-actual-execution-plan-using-the-context-menu) auswählen.

### <a name="include-live-query-statistics-using-the-editor-toolbar"></a>Einschließen der Live-Abfragestatistik über die Editor-Symbolleiste

Bietet Echtzeiteinblicke in den Ausführungsprozess der Abfrage, während die Steuerung von einem Abfrageplanoperator zum nächsten übergeben wird.

Sie können die Option **Live-Abfragestatistik einschließen** auch über das [Kontextmenü](#include-live-query-statistics-using-the-context-menu) auswählen.

### <a name="include-client-statistics-using-the-editor-toolbar"></a>Einschließen der Clientstatistik über die Editor-Symbolleiste

Schließt das Fenster **Clientstatistiken** ein, das Statistiken zu der Abfrage, den Netzwerkpaketen und der verstrichenen Zeit für die Abfrage enthält.

Sie können die Option **Clientstatistik einschließen** auch mithilfe der Tastenkombination UMSCHALT+ALT+S oder über das [Kontextmenü](#include-client-statistics-using-the-context-menu) auswählen.

### <a name="results-to-text-using-the-editor-toolbar"></a>Zurückgeben von Ergebnissen als Text über die Editor-Symbolleiste

Gibt die Abfrageergebnisse als Text im Fenster **Ergebnisse** zurück.

Sie können Ergebnisse auch im Textformat zurückgeben, indem Sie die Tastenkombination STRG+T drücken oder das [Kontextmenü](#results-using-the-context-menu) verwenden.

### <a name="results-to-grid-using-the-editor-toolbar"></a>Zurückgeben von Ergebnissen in einem Raster über die Editor-Symbolleiste

Gibt die Abfrageergebnisse als ein oder mehrere Raster im Fenster **Ergebnisse** zurück. Diese Option ist standardmäßig aktiviert.

Sie können Ergebnisse auch in einem Raster zurückgeben, indem Sie die Tastenkombination STRG+D drücken oder das [Kontextmenü](#results-using-the-context-menu) verwenden.

### <a name="results-to-file-using-the-editor-toolbar"></a>Zurückgeben von Ergebnissen in einer Datei über die Editor-Symbolleiste

Wenn die Abfrage ausgeführt wird, wird das Dialogfeld **Ergebnisse speichern** geöffnet. Wählen Sie unter **Speichern in** den Ordner aus, in dem Sie die Datei speichern möchten. Geben Sie unter **Dateiname** den Namen der Datei ein, und klicken Sie dann auf **Speichern**, um die Abfrageergebnisse als **Berichtsdatei** mit der Dateierweiterung „.rpt“ zu speichern. Wählen Sie den Pfeil nach unten auf der Schaltfläche **Speichern** und dann die Option **Mit Codierung speichern** aus.

Sie können Ergebnisse auch im Dateiformat zurückgeben, indem Sie die Tastenkombination STRG+UMSCHALT+F drücken oder das [Kontextmenü](#results-using-the-context-menu) verwenden.

### <a name="comment-out-the-selected-lines-using-the-editor-toolbar"></a>Auskommentieren von ausgewählten Zeilen über die Editor-Symbolleiste

Markiert die aktuelle Zeile als Kommentar, indem am Zeilenanfang ein Kommentaroperator (--) hinzugefügt wird.

Sie können eine Zeile auch auskommentieren, indem Sie nacheinander die Tastenkombinationen STRG+K und STRG+C drücken.

### <a name="uncomment-the-selected-lines-using-the-editor-toolbar"></a>Aufheben der Auskommentierung von ausgewählten Zeilen über die Editor-Symbolleiste

Markiert die aktuelle Zeile als aktive Quellanweisung, indem alle Kommentaroperatoren (--) am Zeilenanfang entfernt werden.

Sie können eine Zeile auch auskommentieren, indem Sie STRG + K und dann STRG + U drücken.

### <a name="decrease-indent-using-the-editor-toolbar"></a>Verkleinern des Einzugs über die Editor-Symbolleiste

Verschiebt durch das Entfernen von Leerzeichen am Zeilenanfang den Text der Zeile nach links.

### <a name="increase-line-indent-using-the-editor-toolbar"></a>Vergrößern des Einzugs über die Editor-Symbolleiste

Verschiebt durch das Hinzufügen von Leerzeichen am Zeilenanfang den Text der Zeile nach rechts.

### <a name="specify-values-for-template-parameters-using-the-editor-toolbar"></a>Angeben von Werten für Vorlagenparameter über die Editor-Symbolleiste

Öffnet ein Dialogfeld, in dem Sie Werte für Parameter in gespeicherten Prozeduren und Funktionen festlegen können.

## <a name="context-menu"></a>Kontextmenü

Sie können auf das Kontextmenü zugreifen, indem Sie an einer beliebigen Stelle im Abfrage-Editor *mit der rechten Maustaste klicken*. Die Optionen im Kontextmenü entsprechen denen in der SQL-Editor-Symbolleiste. Über das Kontextmenü erhalten Sie dieselben Optionen wie **Verbinden** und **Ausführen**, es werden aber auch weitere Optionen wie **Codeausschnitt einfügen** oder **Umschließen mit** aufgeführt.

![Optionen](media/database-engine-query-editor-sql-server-management-studio/context-menu.png)

### <a name="insert-snippet-using-the-context-menu"></a>„Codeausschnitt einfügen“ über das Kontextmenü

Ein [T-SQL-Codeausschnitt](../scripting/add-transact-sql-snippets.md) ist eine Vorlage, die Sie beim Schreiben von neuen Transact-SQL-Anweisungen im Abfrage-Editor als Ausgangspunkt verwenden können.

### <a name="surround-with-using-the-context-menu"></a>„Umschließen mit“ über das Kontextmenü

Ein Codeausschnitt zum Umschließen ist eine Vorlage, die Sie beim Einschließen einer Reihe von Transact-SQL-Anweisungen in einem BEGIN-, IF- oder WHILE-Block als Ausgangspunkt verwenden können.

### <a name="connection-using-the-context-menu"></a>Herstellen einer Verbindung über das Kontextmenü

![Verbindungen](media/database-engine-query-editor-sql-server-management-studio/context-menu-connections.png)

Im Kontextmenü gibt es mehr **Verbindungsoptionen** als bei Symbolleistenoptionen in SSMS.

- **Verbinden**: Öffnet das Dialogfeld „Verbindung mit Server herstellen“. Mithilfe dieses Dialogfelds können Sie eine Verbindung mit einem Server herstellen.

- **Trennen**: Trennt den aktuellen Abfrage-Editor vom Server.

- **Alle Abfragen trennen**: Trennt die Verbindung sämtlicher Abfragen.

- **Verbindung ändern**: Öffnet das Dialogfeld „Verbindung mit Server herstellen“. Mithilfe dieses Dialogfelds können Sie eine Verbindung mit einem anderen Server herstellen.

### <a name="open-server-in-object-explorer-using-the-context-menu"></a>Öffnen eines Servers im Objekt-Explorer über das Kontextmenü

Der Objekt-Explorer stellt eine hierarchische Benutzeroberfläche zum Anzeigen und Verwalten der Objekte in allen Instanzen von SQL Server bereit. Der Bereich "Details zum Objekt-Explorer" stellt eine tabellarische Ansicht der Instanzobjekte sowie eine Funktion zum Suchen bestimmter Objekte zur Verfügung. Die Funktionalität des Objekt-Explorers variiert abhängig vom Servertyp, enthält aber im Allgemeinen die Entwicklungsfunktionen für Datenbanken und die Verwaltungsfunktionen für alle Servertypen.

### <a name="execute-using-the-context-menu"></a>Ausführen über das Kontextmenü

Führt den ausgewählten bzw. (wenn kein Code ausgewählt ist) den gesamten Code im Abfrage-Editor aus.

### <a name="display-estimated-execution-plan-using-the-context-menu"></a>Anzeigen des geschätzten Ausführungsplan über das Kontextmenü

Fordert einen Abfrageausführungsplan vom Abfrageprozessor an, ohne die Abfrage tatsächlich auszuführen. Der Plan wird im Fenster **Ausführungsplan** angezeigt. Dieser Plan verwendet Indexstatistiken, um die Anzahl der Zeilen zu schätzen, die während jedes Teils der Abfrageausführung voraussichtlich zurückgegeben werden. Der tatsächlich verwendete Abfrageplan kann sich vom geschätzten Ausführungsplan unterscheiden. Dieser Fall kann eintreten, wenn die Anzahl der zurückgegebenen Zeilen von der Schätzung abweicht und der Abfrageprozessor den Plan aus Effizienzgründen ändert.

### <a name="intellisense-enabled-using-the-context-menu"></a>Aktivieren von IntelliSense über das Kontextmenü

Gibt an, ob die IntelliSense-Funktionalität im Abfrage-Editor der Datenbank-Engine verfügbar ist. Diese Option ist standardmäßig festgelegt.

### <a name="trace-query-in-sql-server-profiler-using-the-context-menu"></a>Ablaufverfolgung einer Abfrage im SQL Server Profiler über das Kontextmenü

Der SQL Server Profiler ist eine Benutzeroberfläche zum Erstellen und Verwalten von Ablaufverfolgungen sowie zum Analysieren und Wiedergeben ihrer Ergebnisse. Die Ereignisse werden in einer Ablaufverfolgungsdatei gespeichert, die später analysiert oder beim Versuch, ein Problem zu diagnostizieren, zur Wiedergabe einer bestimmten Reihe von Schritten verwendet werden kann.

### <a name="analyze-query-in-database-engine-tuning-advisor-using-the-context-menu"></a>Analysieren von Abfragen im Datenbankoptimierungsratgeber über das Kontextmenü

Der Datenbankoptimierungsratgeber von Microsoft analysiert Datenbanken und bietet Empfehlungen zum Optimieren der Abfrageleistung. Mit dem Datenbankoptimierungsratgeber können Sie optimale Indizes, indizierten Sichten oder Tabellenpartitionen auswählen und erstellen, auch wenn Sie nicht über detaillierte Kenntnisse bezüglich der Datenbankstruktur oder der internen Mechanismen von SQL Server verfügen. Mit dem DTA können Sie die folgenden Aufgaben ausführen:

### <a name="design-query-in-editor-using-the-context-menu"></a>Entwerfen von Abfragen im Editor über das Kontextmenü

Der Abfrage- und Sicht-Designer wird automatisch geöffnet, wenn Sie die Definition einer Sicht öffnen, die Ergebnisse für eine Abfrage oder Sicht anzeigen bzw. eine Abfrage erstellen oder öffnen.

### <a name="include-actual-execution-plan-using-the-context-menu"></a>Einschließen des tatsächlichen Ausführungsplans über das Kontextmenü

Führt die Abfrage aus, gibt die Abfrageergebnisse zurück und verwendet den Ausführungsplan für die Abfrage. Die Abfragen werden im Fenster **Ausführungsplan** als grafischer Abfrageplan angezeigt.

### <a name="include-live-query-statistics-using-the-context-menu"></a>Einschließen der Live-Abfragestatistik über das Kontextmenü

Bietet Echtzeiteinblicke in den Ausführungsprozess der Abfrage, während die Steuerung von einem Abfrageplanoperator zum nächsten übergeben wird.

### <a name="include-client-statistics-using-the-context-menu"></a>Einschließen der Clientstatistik über das Kontextmenü

Schließt das Fenster **Clientstatistiken** ein, das Statistiken zu der Abfrage, den Netzwerkpaketen und der verstrichenen Zeit für die Abfrage enthält.

### <a name="results-using-the-context-menu"></a>Zurückgeben von Ergebnissen über das Kontextmenü

![Optionen für Ergebnisse](media/database-engine-query-editor-sql-server-management-studio/context-menu-results.png)

Sie können die gewünschten Optionen für *Ergebnisse* über das Kontextmenü auswählen.

- **Ergebnisse in Text**: Gibt die Abfrageergebnisse im Fenster **Ergebnisse** als Text zurück.

- **Ergebnisse in Raster**: Gibt die Abfrageergebnisse im Fenster **Ergebnisse** in Form eines oder mehrerer Raster zurück.

- **Ergebnisse in Datei**: Wenn die Abfrage ausgeführt wird, wird das Dialogfeld **Ergebnisse speichern** geöffnet. Wählen Sie unter **Speichern in** den Ordner aus, in dem Sie die Datei speichern möchten. Geben Sie unter **Dateiname** den Namen der Datei ein, und wählen Sie dann **Speichern** aus, um die Abfrageergebnisse als **Berichtsdatei** mit der Dateierweiterung „.rpt“ zu speichern. Wählen Sie den Pfeil nach unten auf der Schaltfläche **Speichern** und dann die Option **Mit Codierung speichern** aus.

### <a name="properties-window-using-the-context-menu"></a>Eigenschaftenfenster über das Kontextmenü

Das [Eigenschaftenfenster](../menu-help/properties-window-f1-help-management-studio.md) enthält eine Beschreibung des Zustands eines Elements in SQL Server Management Studio, z. B. eine Verbindung oder einen Showplan-Operator, sowie Informationen zu Datenbankobjekten wie Tabellen, Sichten und Designer.

Im Eigenschaftenfenster können Sie die Eigenschaften der aktuellen Verbindung anzeigen. Viele Eigenschaften im Eigenschaftenfenster sind schreibgeschützt, können aber an anderer Stelle in Management Studio geändert werden. So ist z. B. die Database-Eigenschaft einer Abfrage im Eigenschaftenfenster schreibgeschützt, kann aber auf der Symbolleiste geändert werden.

### <a name="query-options-using-the-context-menu"></a>Abfrageoptionen über das Kontextmenü

Öffnet das Dialogfeld **Abfrageoptionen** . Mithilfe dieses Dialogfelds konfigurieren Sie die Standardoptionen für Abfrageausführung und Abfrageergebnisse.

## <a name="transact-sql-f1-help"></a>F1-Hilfe für Transact-SQL

Wenn Sie bei einer Transact-SQL-Anweisung die Taste F1 drücken, stellt der Abfrage-Editor einen Link zum entsprechenden Referenzthema bereit. Markieren Sie hierzu den Namen einer Transact-SQL-Anweisung, und wählen Sie dann F1 aus. Die Suchmaschine für Hilfe sucht dann nach einem Thema mit einem F1-Hilfeattribut, das der markierten Zeichenfolge entspricht.

Wenn die Suchmaschine kein Thema mit einem F1-Hilfeschlüsselwort findet, das genau der markierten Zeichenfolge entspricht, wird dieses Thema angezeigt. In diesem Fall gibt es zwei Ansätze zum Suchen der gewünschten Hilfe:

- Kopieren Sie die markierte Zeichenfolge aus dem Editor, fügen Sie diese in der SQL Server-Onlinedokumentation in die Registerkarte "Suchen" ein, und führen Sie eine Suche aus.

- Markieren Sie nur den Teil der Transact-SQL-Anweisung, die am ehesten zu einem F1-Hilfeschlüsselwort passt, das auf ein Thema angewendet wird, und wählen Sie erneut F1 aus. Für die Such-Engine ist eine genaue Übereinstimmung zwischen der markierten Zeichenfolge und einem Thema zugewiesenen F1-Hilfeschlüsselwort erforderlich. Wenn die markierte Zeichenfolge für Ihre Umgebung eindeutige Elemente enthält, z. B. Spalten- oder Parameternamen, erhält die Suchmaschine keine Übereinstimmung. Zu den Zeichenfolgen, die markiert werden können, gehören die folgenden:

  - Der Name einer Transact-SQL-Anweisung, zum Beispiel SELECT, CREATE DATABASE oder BEGIN TRANSACTION.

  - Der Name einer integrierten Funktion, zum Beispiel SERVERPROPERTY oder @@VERSION.

  - Der Name einer im System gespeicherten Prozedurtabelle oder von Sichten, z. B. "sys.data_spaces" oder "sp_tableoption".

## <a name="editor-tasks"></a>Editor-Tasks

| Taskbeschreibung | Thema |
|------------------|-------|
| Beschreibt die verschiedenen Methoden zum Öffnen der Editoren in SSMS.| [Öffnen eines Editors](../scripting/open-an-editor-sql-server-management-studio.md) |
| Konfigurieren Sie Optionen für die verschiedenen Editoren, z. B. Zeilennummerierung und IntelliSense-Optionen. | [Konfigurieren von Editoren](../scripting/configure-editors-sql-server-management-studio.md) |
| Beschreibt, wie der Anzeigemodus, z. B. Zeilenumbruch, Aufteilen eines Fensters oder Registerkarten, verwaltet wird.| [Verwalten des Editors und des Ansichtsmodus](../scripting/manage-the-editor-and-view-mode.md) |
| Legen Sie Formatierungsoptionen fest, z. B. ausgeblendeter Text oder Einzug. | [Verwalten der Codeformatierung](../scripting/manage-code-formatting.md) |
| Navigieren Sie durch den Text in einem Editor-Fenster mithilfe von Features wie der inkrementellen Suche oder „Gehe zu“. | [Navigieren in Code und Text](../scripting/navigate-code-and-text.md) |
| Legen Sie Farbcodierungsoptionen für verschiedene Syntaxklassen fest, wodurch das Lesen komplexer Anweisungen vereinfacht wird. | [Farbcodierung im Abfrage-Editor](../scripting/color-coding-in-query-editors.md) |
| Ziehen Sie Text von einer Position in einem Skript, und legen Sie ihn an einer anderen Position ab.| [Verschieben von Text mit Drag und Drop](../scripting/drag-and-drop-text.md) |
| Beschreibt, wie Lesezeichen festgelegt werden, damit wichtige Codeteile leichter wiedergefunden werden können. | [Verwalten von Lesezeichen](../scripting/manage-bookmarks.md) |
| Beschreibt, wie Skripts oder die Ergebnisse in einem Fenster oder einem Raster gedruckt werden.| [Drucken von Code und Ergebnissen](../scripting/print-code-and-results.md) |
| Beschreibt, wie die grundlegenden Features im MDX-Abfrage-Editor angezeigt und verwendet werden. | [Erstellen von Analysis Services-Skripts](/analysis-services/instances/create-analysis-services-scripts-in-management-studio) |
| Beschreibt, wie die grundlegenden Features im DMX-Abfrage-Editor angezeigt und verwendet werden. | [Erstellen einer DMX-Abfrage](/analysis-services/data-mining/create-a-dmx-query-in-sql-server-management-studio) |
| Beschreibt, wie die grundlegenden Features im XML/A-Editor angezeigt und verwendet werden. | [XML-Editor](../scripting/xml-editor-sql-server-management-studio.md) |
| Beschreibt, wie die sqlcmd-Features im Abfrage-Editor der Datenbank-Engine verwendet werden.| [Bearbeiten von SQLCMD-Skripts](../scripting/edit-sqlcmd-scripts-with-query-editor.md) |
| Beschreibt, wie die Codeausschnitte im Abfrage-Editor der Datenbank-Engine verwendet werden. Ausschnitte sind Vorlagen für häufig verwendete Anweisungen oder Blöcke. Sie können angepasst oder erweitert werden, um sitespezifische Ausschnitte einzuschließen.| [T-SQL-Codeausschnitte](../scripting/add-transact-sql-snippets.md) |
| Beschreibt, wie der Transact\-SQL-Debugger verwendet wird, um Code in Einzelschritten auszuführen und Debuggininformationen anzuzeigen, z. B. die Werte in Variablen und Parametern.| [T-SQL-Debugger](../scripting/transact-sql-debugger.md) |

## <a name="see-also"></a>Weitere Informationen

- [Anpassen von Menüs und Tastenkombinationen](../customize-menus-and-shortcut-keys.md)
- [Tastenkombinationen für SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)