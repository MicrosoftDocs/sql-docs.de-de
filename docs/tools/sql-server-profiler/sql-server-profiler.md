---
title: SQL Server Profiler
titleSuffix: SQL Server Profiler
description: Testen Sie die SQL Server Profiler-Features. Dieses Tool hilft Ihnen beim Beheben von Problemen, sodass Sie Ablaufverfolgungen erstellen und Ablaufverfolgungsergebnisse analysieren und wiedergeben können.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: profiler
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 05/01/2020
ms.openlocfilehash: 6208c880dd234a0e6a62f61e6f6ae87567213c12
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354457"
---
# <a name="sql-server-profiler"></a>SQL Server Profiler

[!INCLUDE[sql-asdbmi](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ist eine Benutzeroberfläche zum Erstellen und Verwalten von Ablaufverfolgungen sowie zum Analysieren und Wiedergeben ihrer Ergebnisse. Ereignisse werden in einer Ablaufverfolgungsdatei gespeichert, die später analysiert oder beim Diagnostizieren eines Problems zur Wiedergabe einer bestimmten Schrittabfolge verwendet werden kann.

> [!IMPORTANT]
> Die SQL-Ablaufverfolgung und [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] sind veraltet. Der *Microsoft.SqlServer.Management.Trace*-Namespace, der die Objekte für die Microsoft SQL Server-Ablaufverfolgung und -Wiedergabe enthält, ist ebenfalls veraltet.
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]
> Verwenden Sie stattdessen erweiterte Ereignisse. Weitere Informationen zu [erweiterten Ereignissen](../../relational-databases/extended-events/extended-events.md) finden Sie unter [Schnellstart: Erweiterte Ereignisse in SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md) und [Verwenden des SSMS XEvent Profilers](../../relational-databases/extended-events/use-the-ssms-xe-profiler.md).

> [!NOTE]
> [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] wird für Analysis Services-Workloads unterstützt.

> [!NOTE]
> Wenn Sie versuchen, eine Verbindung mit einer Azure SQL-Datenbank über den SQL Server Profiler herzustellen, wird fälschlicherweise eine irreführende Fehlermeldung wie folgt ausgelöst:
>
> - Zum Ausführen einer Ablaufverfolgung für SQL Server müssen Sie ein Mitglied einer festen sysadmin-Serverrolle sein oder über die ALTER TRACE-Berechtigung verfügen.
>
> Die Nachricht sollte erläutern, dass Azure SQL-Datenbank nicht vom SQL Server Profiler unterstützt wird.

## <a name="where-is-the-profiler"></a>Wo befindet sich der Profiler?

Sie können den Profiler auf verschiedene Arten in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] starten. [Weitere Informationen hierzu finden Sie im verlinkten Artikel.](start-sql-server-profiler.md)

## <a name="capture-and-replay-trace-data"></a>Aufzeichnen und Wiedergeben von Ablaufverfolgungsdaten

In der folgenden Tabelle werden die Funktionen angezeigt, deren Verwendung wir in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] empfehlen, um Ablaufverfolgungsdaten zu erfassen und wiederzugeben.

||||
|-|-|-|
|**Feature\Zielarbeitsauslastung**|**Relationale Engine**|**Analysis Services**|  
|**Ablaufverfolgungssammlung**|[Erweiterte Ereignisse](../../relational-databases/extended-events/extended-events.md), grafische Benutzeroberfläche in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]|  
|**Ablaufverfolgungswiedergabe**|[Distributed Replay](../distributed-replay/sql-server-distributed-replay.md)|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]|

## <a name="use-sql-server-profiler"></a>Verwenden des SQL Server Profilers

Microsoft [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ist eine grafische Benutzeroberfläche für die SQL-Ablaufverfolgung zur Überwachung einer Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] oder Analysis Services. Daten über die einzelnen Ereignisse können aufgezeichnet und in einer Datei oder Tabelle zur späteren Analyse gespeichert werden. Beispielsweise können Sie eine Produktionsumgebung überwachen und feststellen, welche gespeicherten Prozeduren langsam ablaufen und dadurch die Leistung beeinträchtigen. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] wird beispielsweise für folgende Aktivitäten verwendet:

- Schrittweises Untersuchen problematischer Abfragen, um die Ursache des Problems zu ermitteln.

- Suchen und Diagnostizieren von Abfragen, die sehr langsam ausgeführt werden.

- Erfassen der [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen, die zu einem Problem geführt haben. Mithilfe der gespeicherten Ablaufverfolgung kann das Problem auf einem Testserver repliziert werden, um es dort zu diagnostizieren.

- Überwachen der Leistung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zur Optimierung der Arbeitsauslastung. Informationen zum Optimieren des physischen Datenbankentwurfs für Datenbankarbeitslasten finden Sie unter [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md).

- Korrelieren von Leistungsindikatoren zur Diagnose von Problemen.

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] unterstützt auch die Überwachung von Aktionen, die für Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt werden. Durch Überwachungen werden sicherheitsbezogene Aktionen aufgezeichnet, die später von einem Sicherheitsadministrator überprüft werden können.

## <a name="sql-server-profiler-concepts"></a>SQL Server Profiler-Konzepte

Um [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]verwenden zu können, müssen Sie die Begriffe verstehen, mit denen die Funktionsweise des Tools beschrieben wird.

> [!NOTE]
> Die SQL-Ablaufverfolgung zu verstehen, hilft ungemein bei der Arbeit mit dem [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Weitere Informationen finden Sie unter [SQL Trace](../../relational-databases/sql-trace/sql-trace.md).

### <a name="event"></a>Ereignis

Ein Ereignis ist eine Aktion, die innerhalb einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]generiert wird. Im Folgenden sind Beispiele dafür aufgeführt:  
  
- Anmeldeverbindungen, fehlgeschlagene und getrennte Verbindungen.    
- [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen `SELECT`, `INSERT`, `UPDATE` und `DELETE`
- Der Status von RPC-Batches (Remote Procedure Call, Remoteprozeduraufruf).  
- Der Start oder das Ende einer gespeicherten Prozedur.  
- Der Start oder das Ende von Anweisungen innerhalb gespeicherter Prozeduren.  
- Der Start oder das Ende eines SQL-Batches.  
- Ein Fehler, der in das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Fehlerprotokoll geschrieben wird.  
- Eine Sperre, die für ein Datenbankobjekt eingerichtet oder aufgehoben wird.  
- Ein geöffneter Cursor.  
- Überprüfungen der Sicherheitsberechtigungen.  
  
Die gesamten von einem Ereignis generierten Daten werden in der Ablaufverfolgung in einer einzelnen Zeile angezeigt. Diese Zeile wird von Datenspalten unterbrochen, die Details zu den Ereignissen enthält.  

### <a name="eventclass"></a>EventClass

Eine Ereignisklasse ist ein Ereignistyp, dessen Ablauf verfolgt werden kann. Die Ereignisklasse enthält alle Daten, die von einem Ereignis berichtet werden können. Im Folgenden werden Beispiele für Ereignisklassen aufgeführt:

- **SQL:BatchCompleted**
- **Audit Login**
- **Audit Logout**
- **Lock: Acquired**
- **Lock: Released**

### <a name="eventcategory"></a>EventCategory

Eine Ereigniskategorie bestimmt, wie Ereignisse innerhalb von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]zusammengefasst werden. So sind beispielsweise alle Sperrereignisklassen in der Ereigniskategorie **Sperren** zusammengefasst. Ereigniskategorien sind jedoch nur in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]vorhanden. Dieser Begriff drückt nicht aus, wie Engine-Ereignisse gruppiert werden.

### <a name="datacolumn"></a>DataColumn

Eine Datenspalte ist ein Attribut einer Ereignisklasse, die in der Ablaufverfolgung erfasst ist. Da von der Ereignisklasse der Typ der Daten festgelegt wird, die gesammelt werden können, gelten nicht alle Datenspalten für alle Ereignisklassen. In einer Ablaufverfolgung, in der die Ereignisklasse **Lock: Acquired** erfasst wird, enthält die Datenspalte **BinaryData** beispielsweise den Wert der gesperrten Seiten-ID oder der Zeile, aber die Datenspalte **Integer Data** enthält keine Werte, weil sie nicht auf die erfasste Ereignisklasse anwendbar ist.

### <a name="template"></a>Vorlage

Eine Vorlage definiert die Standardkonfiguration für eine Ablaufverfolgung. Dazu gehören insbesondere die Ereignisklassen, die mithilfe von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]überwacht werden sollen. Sie können beispielsweise eine Vorlage erstellen, mit der die zu verwendenden Ereignisse, Datenspalten und Filter angegeben werden. Eine Vorlage wird nicht ausgeführt, sondern als Datei mit der Erweiterung „.tdf“ gespeichert. Nach dem Speichern steuert die Vorlage die Ablaufverfolgungsdaten, die beim Starten einer auf der Vorlage basierenden Ablaufverfolgung erfasst werden.

### <a name="trace"></a>Trace

Eine Ablaufverfolgung zeichnet Daten auf der Grundlage der ausgewählten Ereignisklassen, Datenspalten und Filter auf. Sie können beispielsweise eine Ablaufverfolgung erstellen, um Ausnahmefehler aufzuzeichnen. Wählen Sie dazu die **Exception** -Ereignisklasse und die Datenspalten **Fehler**, **Status** und **Schweregrad** aus. Daten aus diesen drei Spalten müssen gesammelt werden, damit die Ablaufverfolgung sinnvolle Daten als Ergebnis liefert. Sie können dann eine solcherart konfigurierte Ablaufverfolgung ausführen und Daten aufgrund aller **Exception** -Ereignisse sammeln, die auf dem Server auftreten. Ablaufverfolgungsdaten können gespeichert oder sofort für Analysezwecke verwendet werden. Ablaufverfolgungen können zu einem späteren Zeitpunkt wiedergegeben werden, obwohl bestimmte Ereignisse wie **Exception** -Ereignisse nie wiedergegeben werden. Sie können die Ablaufverfolgung als Vorlage speichern, um in Zukunft ähnliche Ablaufverfolgungen zu erstellen.  

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bietet zwei Möglichkeiten für die Ablaufverfolgung einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] und gespeicherte Systemprozeduren.  

### <a name="filter"></a>Filtern

Beim Erstellen einer Ablaufverfolgung oder Vorlage können Sie Kriterien definieren, um die von dem Ereignis gesammelten Daten zu filtern. Damit Ablaufverfolgungen nicht zu groß werden, können Sie sie filtern, sodass nur eine Teilmenge der Ereignisdaten gesammelt wird. Sie können beispielsweise die Microsoft Windows-Benutzernamen in der Ablaufverfolgung auf bestimmte Benutzer beschränken, wodurch die Ausgabedatenmenge kleiner wird.  

Ist kein Filter eingerichtet, werden alle Ereignisse der ausgewählten Ereignisklassen in der Ablaufverfolgungsausgabe zurückgegeben.

## <a name="sql-server-profiler-tasks"></a>SQL Server Profiler-Tasks

|Taskbeschreibung|Thema|  
|----------------------|-----------|  
|Listet die vordefinierten Vorlagen, die SQL Server zum Überwachen bestimmter Ereignistypen bereitstellt, und die zum Wiedergeben von Ablaufverfolgungen erforderlichen Berechtigungen auf.|[Vorlagen und Berechtigungen in SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md)|  
|Beschreibt, wie der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler ausgeführt wird|[Erforderliche Berechtigungen zum Ausführen von SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)|  
|Beschreibt, wie eine Ablaufverfolgung erstellt wird.|[Erstellen einer Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)|  
|Beschreibt, wie Ereignisse und Datenspalten für eine Ablaufverfolgungsdatei angegeben werden.|[Angeben von Ereignissen und Datenspalten für eine Ablaufverfolgungsdatei &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/specify-events-and-data-columns-for-a-trace-file-sql-server-profiler.md)|  
|Beschreibt, wie Ablaufverfolgungsergebnisse in einer Datei gespeichert werden.|[Speichern von Ablaufverfolgungsergebnissen in einer Datei &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/save-trace-results-to-a-file-sql-server-profiler.md)|  
|Beschreibt, wie Ablaufverfolgungsergebnisse in einer Tabelle gespeichert werden.|[Speichern von Ablaufverfolgungsergebnissen in einer Tabelle &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/save-trace-results-to-a-table-sql-server-profiler.md)|  
|Beschreibt, wie Ereignisse in einer Ablaufverfolgung gefiltert werden.|[Filtern von Ereignissen in einer Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/filter-events-in-a-trace-sql-server-profiler.md)|  
|Beschreibt, wie Filterinformationen angezeigt werden.|[Anzeigen von Filterinformationen &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/view-filter-information-sql-server-profiler.md)|  
|Beschreibt, wie ein Filter geändert wird.|[Ändern eines Filters &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/modify-a-filter-sql-server-profiler.md)|  
|Beschreibt, wie eine maximale Dateigröße für eine Ablaufverfolgungsdatei ([!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]) festgelegt wird.|[Festlegen einer maximalen Dateigröße für eine Ablaufverfolgungsdatei &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-a-maximum-file-size-for-a-trace-file-sql-server-profiler.md)|  
|Beschreibt, wie eine maximale Tabellengröße für eine Ablaufverfolgungstabelle festgelegt wird.|[Festlegen der maximalen Tabellengröße für eine Ablaufverfolgungstabelle &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-a-maximum-table-size-for-a-trace-table-sql-server-profiler.md)|  
|Beschreibt, wie eine Ablaufverfolgung gestartet wird.|[Starten einer Ablaufverfolgung](../../tools/sql-server-profiler/start-a-trace.md)|  
|Beschreibt, wie eine Ablaufverfolgung nach dem Herstellen einer Verbindung mit einem Server automatisch gestartet wird.|[Automatisches Starten einer Ablaufverfolgung nach dem Herstellen einer Verbindung mit einem Server &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/start-a-trace-automatically-after-connecting-to-a-server-sql-server-profiler.md)|  
|Beschreibt, wie Ereignisse basierend auf der Ereignisstartzeit gefiltert werden.|[Filtern von Ereignissen nach Ereignisstartzeit &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/filter-events-based-on-the-event-start-time-sql-server-profiler.md)|  
|Beschreibt, wie Ereignisse basierend auf der Ereignisendzeit gefiltert werden.|[Filtern von Ereignissen anhand der Ereignisendzeit &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/filter-events-based-on-the-event-end-time-sql-server-profiler.md)|  
|Beschreibt, wie Serverprozess-IDs (SPIDs) in einer Ablaufverfolgung gefiltert werden.|[Filtern von Server-Prozess-IDs &#40;SPIDs&#41; in einer Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/filter-server-process-ids-spids-in-a-trace-sql-server-profiler.md)|  
|Beschreibt, wie eine Ablaufverfolgung angehalten wird.|[Anhalten einer Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/pause-a-trace-sql-server-profiler.md)|  
|Beschreibt, wie eine Ablaufverfolgung beendet wird.|[Beenden einer Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/stop-a-trace-sql-server-profiler.md)|  
|Beschreibt, wie eine Ablaufverfolgung ausgeführt wird, nachdem sie angehalten oder beendet wurde.|[Ausführen einer Ablaufverfolgung, nachdem sie angehalten oder beendet wurde &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/run-a-trace-after-it-has-been-paused-or-stopped-sql-server-profiler.md)|  
|Beschreibt, wie ein Ablaufverfolgungsfenster gelöscht wird.|[Löschen des Inhalts eines Ablaufverfolgungsfensters &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/clear-a-trace-window-sql-server-profiler.md)|  
|Beschreibt, wie ein Ablaufverfolgungsfenster geschlossen wird.|[Schließen eines Ablaufverfolgungsfensters &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/close-a-trace-window-sql-server-profiler.md)|  
|Beschreibt, wie Standardeinstellungen für Ablaufverfolgungsdefinitionen festgelegt werden.|[Festlegen der Standardeinstellungen für Ablaufverfolgungsdefinitionen &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-trace-definition-defaults-sql-server-profiler.md)|  
|Beschreibt, wie Standardeinstellungen für die Ablaufverfolgungsanzeige festgelegt werden.|[Festlegen der Standardeinstellungen für die Ablaufverfolgungsanzeige &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-trace-display-defaults-sql-server-profiler.md)|  
|Beschreibt, wie eine Ablaufverfolgungsdatei geöffnet wird.|[Öffnen einer Ablaufverfolgungsdatei &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md)|  
|Beschreibt, wie eine Ablaufverfolgungstabelle geöffnet wird.|[Öffnen einer Ablaufverfolgungstabelle &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)|  
|Beschreibt, wie eine Ablaufverfolgungstabelle wiedergegeben wird.|[Wiedergeben einer Ablaufverfolgungstabelle &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-a-trace-table-sql-server-profiler.md)|  
|Beschreibt, wie eine Ablaufverfolgungsdatei wiedergegeben wird.|[Wiedergeben einer Ablaufverfolgungsdatei &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-a-trace-file-sql-server-profiler.md)|  
|Beschreibt, wie jeweils ein einzelnes Ereignis wiedergegeben wird.|[Wiedergeben von jeweils einem einzelnen Ereignis &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-a-single-event-at-a-time-sql-server-profiler.md)|  
|Beschreibt die Wiedergabe bis zu einem Breakpoint.|[Wiedergeben bis zu einem Breakpoint &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-to-a-breakpoint-sql-server-profiler.md)|  
|Beschreibt die Wiedergabe bis zu einer Cursorposition.|[Wiedergeben bis zu einer Cursorposition &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-to-a-cursor-sql-server-profiler.md)|  
|Beschreibt, wie ein [!INCLUDE[tsql](../../includes/tsql-md.md)]-Skript wiedergegeben wird|[Wiedergeben eines Transact-SQL-Skripts &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/replay-a-transact-sql-script-sql-server-profiler.md)|  
|Beschreibt, wie eine Ablaufverfolgungsvorlage erstellt wird.|[Erstellen einer Ablaufverfolgungsvorlage &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-template-sql-server-profiler.md)|  
|Beschreibt, wie eine Ablaufverfolgungsvorlage geändert wird.|[Ändern einer Ablaufverfolgungsvorlage &#40;SQL Server Profiler&#41;](./modify-trace-templates.md)|  
|Beschreibt, wie globale Ablaufverfolgungsoptionen festgelegt werden.|[Festlegen globaler Ablaufverfolgungsoptionen &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/set-global-trace-options-sql-server-profiler.md)|  
|Beschreibt, wie ein Wert oder eine Datenspalte während der Ablaufverfolgung gesucht wird.|[Suchen eines Wertes oder einer Datenspalte während der Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/find-a-value-or-data-column-while-tracing-sql-server-profiler.md)|  
|Beschreibt, wie eine Vorlage aus einer ausgeführten Ablaufverfolgung abgeleitet wird.|[Ableiten einer Vorlage von einer zurzeit ausgeführten Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-running-trace-sql-server-profiler.md)|  
|Beschreibt, wie eine Vorlage aus einer Ablaufverfolgungsdatei oder Ablaufverfolgungstabelle abgeleitet wird.|[Ableiten einer Vorlage von einer Ablaufverfolgungsdatei oder Ablaufverfolgungstabelle &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-trace-file-or-trace-table-sql-server-profiler.md)|  
|Beschreibt, wie ein [!INCLUDE[tsql](../../includes/tsql-md.md)]-Skript für das Ausführen einer Ablaufverfolgung erstellt wird|[Erstellen eines Transact-SQL-Skripts zum Ausführen einer Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-transact-sql-script-for-running-a-trace-sql-server-profiler.md)|  
|Beschreibt, wie eine Ablaufverfolgungsvorlage exportiert wird.|[Exportieren einer Ablaufverfolgungsvorlage &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/export-a-trace-template-sql-server-profiler.md)|  
|Beschreibt, wie eine Ablaufverfolgungsvorlage importiert wird.|[Importieren einer Ablaufverfolgungsvorlage &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/import-a-trace-template-sql-server-profiler.md)|  
|Beschreibt, wie ein Skript aus einer Ablaufverfolgung extrahiert wird.|[Extrahieren eines Skripts aus einer Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/extract-a-script-from-a-trace-sql-server-profiler.md)|  
|Beschreibt, wie eine Ablaufverfolgung mit Windows-Leistungsprotokolldaten korreliert wird.|[Korrelieren einer Ablaufverfolgung mit Windows-Leistungsprotokolldaten &#40;SQL Server Profiler&#41;](./correlate-a-trace-with-windows-performance-log-data.md)|  
|Beschreibt, wie in einer Ablaufverfolgung angezeigte Spalten organisiert werden.|[Organisieren von in einer Ablaufverfolgung angezeigten Spalten &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/organize-columns-displayed-in-a-trace-sql-server-profiler.md)|  
|Beschreibt, wie der [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] gestartet wird|[Starten des SQL Server Profilers](../../tools/sql-server-profiler/start-sql-server-profiler.md)|  
|Beschreibt, wie Ablaufverfolgungen und Ablaufverfolgungsvorlagen gespeichert werden.|[Speichern von Ablaufverfolgungen und Ablaufverfolgungsvorlagen](../../tools/sql-server-profiler/save-traces-and-trace-templates.md)|  
|Beschreibt, wie Ablaufverfolgungsvorlagen geändert werden.|[Ändern von Ablaufverfolgungsvorlagen](../../tools/sql-server-profiler/modify-trace-templates.md)|  
|Beschreibt, wie eine Ablaufverfolgung mit Windows-Leistungsprotokolldaten korreliert wird.|[Korrelieren einer Ablaufverfolgung mit Windows-Leistungsprotokolldaten](../../tools/sql-server-profiler/correlate-a-trace-with-windows-performance-log-data.md)|  
|Beschreibt, wie Ablaufverfolgungen mit dem [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] angezeigt und analysiert werden|[Anzeigen und Analysieren von Ablaufverfolgungen mit SQL Server Profiler](../../tools/sql-server-profiler/view-and-analyze-traces-with-sql-server-profiler.md)|  
|Beschreibt, wie Deadlocks mit dem [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] analysiert werden|[Analysieren von Deadlocks mit SQL Server Profiler](../../tools/sql-server-profiler/analyze-deadlocks-with-sql-server-profiler.md)|  
|Beschreibt, wie Abfragen mit SHOWPLAN-Ergebnissen in SQL Server Profiler analysiert werden.|[Analysieren von Abfragen mit SHOWPLAN-Ergebnissen in SQL Server Profiler](../../tools/sql-server-profiler/analyze-queries-with-showplan-results-in-sql-server-profiler.md)|  
|Beschreibt, wie Ablaufverfolgungen mit dem [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] gefiltert werden|[Filtern von Ablaufverfolgungen mit SQL Server Profiler](../../tools/sql-server-profiler/filter-traces-with-sql-server-profiler.md)|  
|Beschreibt die Verwendung der Wiedergabefunktionen von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|[Wiedergeben von Ablaufverfolgungen](../../tools/sql-server-profiler/replay-traces.md)|  
|Enthält kontextbezogene Hilfethemen zum [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]|[SQL Server Profiler (F1-Hilfe)](../../tools/sql-server-profiler/sql-server-profiler-f1-help.md)|  
|Listet die von [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] zur Leistungs- und Aktivitätsüberwachung verwendeten gespeicherten Systemprozeduren auf.|[Gespeicherte Prozeduren von SQL Server Profiler &#40;SQL Server Profiler&#41;](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)|  

## <a name="see-also"></a>Weitere Informationen

- [Sperren-Ereigniskategorie](../../relational-databases/event-classes/locks-event-category.md)
- [Sessions-Ereigniskategorie](../../relational-databases/event-classes/sessions-event-category.md)
- [Gespeicherte Prozeduren (Ereigniskategorie)](../../relational-databases/event-classes/stored-procedures-event-category.md)
- [TSQL-Ereigniskategorie](../../relational-databases/event-classes/tsql-event-category.md)
- [Überwachen der Serverleistung und -aktivität](../../relational-databases/performance/server-performance-and-activity-monitoring.md)