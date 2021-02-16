---
title: Überwachen der Leistung mit dem Abfragespeicher | Microsoft -Dokumentation
description: Der SQL Server-Abfragespeicher bietet Ihnen Erkenntnisse über die Auswahl und Leistung eines Abfrageplans. Der Abfragespeicher erfasst einen Verlauf der Abfragen, Pläne und Laufzeitstatistiken.
ms.custom: ''
ms.date: 04/09/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store
- Query Store, described
ms.assetid: e06344a4-22a5-4c67-b6c6-a7060deb5de6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: c8a836f1a8daf2b4b6a9d5b76156191799013484
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341710"
---
# <a name="monitoring-performance-by-using-the-query-store"></a>Leistungsüberwachung mit dem Abfragespeicher

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Abfragespeicher bietet Ihnen Einblick in die Auswahl und die Leistung eines Abfrageplans. Er vereinfacht das Beheben von Leistungsproblemen, indem er das schnelle Auffinden von Leistungsabweichungen durch Änderungen an Abfrageplänen ermöglicht. Der Abfragespeicher erfasst automatisch einen Verlauf der Abfragen, Pläne und Laufzeitstatistiken und bewahrt diese zur Überprüfung auf. Es unterteilt die Daten nach Zeitfenstern, sodass Sie Verwendungsmuster für Datenbanken erkennen können und verstehen, wann Abfrageplanänderungen auf dem Server aufgetreten sind. Sie können den Abfragespeicher mit der Option [ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md) konfigurieren.

Informationen zum Betrieb des Abfragespeichers in Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)] finden Sie unter [Verwenden des Abfragespeichers in Azure SQL-Datenbank](best-practice-with-the-query-store.md#Insight).

> [!IMPORTANT]
> Wenn Sie den Abfragespeicher für Erkenntnisse zu Just-In-Time-Arbeitsauslastungen in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] verwenden, planen Sie baldmöglichst die Installation der Fixes zur Leistungsskalierbarkeit in [KB 4340759](https://support.microsoft.com/help/4340759) ein.

## <a name="enabling-the-query-store"></a><a name="Enabling"></a> Aktivieren des Abfragespeichers

 Standardmäßig ist der Abfragespeicher für neue SQL Server- und Azure Synapse Analytics-Datenbanken nicht aktiviert. Für neue Azure SQL-Datenbanken ist er hingegen standardmäßig aktiviert.

### <a name="use-the-query-store-page-in-ssmanstudiofull"></a>Verwenden der Seite „Abfragespeicher“ in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]

1. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf eine Datenbank und anschließend auf **Eigenschaften**.

   > [!NOTE]
   > Erfordert mindestens Version 16 von [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].

2. Wählen Sie im Dialogfeld **Datenbankeigenschaften** die Seite **Abfragespeicher** aus.

3. Wählen Sie im Feld **Betriebsmodus (angefordert)** die Option **Lesen und schreiben** aus.

### <a name="use-transact-sql-statements"></a>Verwenden von Transact-SQL-Anweisungen

Verwenden Sie die Anweisung **ALTER DATABASE**, um den Abfragespeicher für eine bestimmte Datenbank zu aktivieren. Beispiel:

```sql
SET QUERY_STORE = ON (OPERATION_MODE = READ_WRITE);
```

Weitere Syntaxoptionen im Zusammenhang mit dem Abfragespeicher finden Sie unter [ALTER DATABASE SET-Optionen &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).

> [!NOTE]
> Der Abfragespeicher kann für die **master**- oder **tempdb**-Datenbank nicht aktiviert werden.

> [!IMPORTANT]
> Informationen zum Aktivieren des Abfragespeichers und dazu, wie Sie ihn an Ihre Arbeitsauslastung angepasst halten, finden Sie unter [Bewährte Methoden für den Abfragespeicher](../../relational-databases/performance/best-practice-with-the-query-store.md#Configure).

## <a name="information-in-the-query-store"></a><a name="About"></a> Informationen im Abfragespeicher

Die Ausführungspläne für eine bestimmte Abfrage in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verändern sich i. Allg. im Laufe der Zeit aufgrund unterschiedlicher Ursachen wie z.B. statischer Änderungen, Schemaänderungen, des Erstellens/Löschens von Indizes usw. Der Prozedurcache (in dem zwischengespeicherte Abfragepläne gespeichert werden) speichert nur den letzten Ausführungsplan. Pläne werden auch bei Speicherplatzknappheit aus dem Plancache entfernt. Aus diesem Grund kann die Problembehandlung bei einer Regression der Abfrageleistung schwierig und zeitaufwendig sein.

Da der Abfragespeicher mehrere Ausführungspläne pro Abfrage beibehält, kann er über Richtlinien den Abfrageprozessor anweisen, für eine Abfrage einen bestimmten Ausführungsplan zu verwenden. Dies wird als Planerzwingung bezeichnet. Das Erzwingen eines Plans im Abfragespeicher erfolgt ähnlich wie beim Abfragehinweis [USE PLAN](../../t-sql/queries/hints-transact-sql-query.md) , es erfordert jedoch keine Änderung an Benutzeranwendungen. Durch das Erzwingen eines Plans können Sie eine Regression der Abfrageleistung aufgrund einer Änderung des Plans in sehr kurzer Zeit beheben.

> [!NOTE]
> Der Abfragespeicher sammelt Pläne für DML-Anweisungen wie SELECT, INSERT, UPDATE, DELETE, MERGE und BULK INSERT.
>
> Der Abfragespeicher sammelt standardmäßig keine Daten für systemintern kompilierte gespeicherte Prozeduren. Verwenden Sie [sys.sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md), um die Datensammlung für systemintern kompilierte gespeicherte Prozeduren zu aktivieren.

Durch **Wartestatistiken** erhalten Sie weitere Informationen, die Ihnen bei der Problembehandlung der Leistung in [!INCLUDE[ssde_md](../../includes/ssde_md.md)] helfen können. Lange Zeit waren Wartestatistiken nur auf Instanzebene verfügbar, wodurch es schwierig war, sie einer bestimmten Abfrage zuzuordnen. Ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] bietet der Abfragespeicher eine Dimension, die Wartestatistiken nachverfolgt. Im folgenden Beispiel wird der Abfragespeicher zum Sammeln von Wartestatistiken aktiviert.

```sql
SET QUERY_STORE = ON ( WAIT_STATS_CAPTURE_MODE = ON );
```

Häufige Szenarios für die Verwendung des Abfragespeichers:

- Schnelles Auffinden und Beheben von Regressionen der Planleistung durch Erzwingung des vorherigen Abfrageplans Korrigieren von Abfragen, die in der Vergangenheit aufgrund von Änderungen am Ausführungsplan die Leistung vermindert haben
- Bestimmen der Ausführungshäufigkeit einer Abfrage in einem festgelegten Zeitraum mit Unterstützung eines DBAs bei der Behandlung von Leistungsproblemen mit Ressourcen
- Identifizieren der häufigsten *n* Abfragen (nach Ausführungszeit, Speicherverbrauch usw.) in den letzten *x* Stunden.
- Überwachen des Verlaufs von Abfrageplänen für eine bestimmte Abfrage
- Analysieren der Verwendungsmuster einer Ressource (CPU, E/A und Arbeitsspeicher) für eine bestimmte Datenbank
- Identifizieren Sie Top-N-Abfragen, die auf den Ressourcen warten.
- Erhalten Sie Einblick in die Wartedetails einer bestimmten Abfrage oder eines bestimmten Plans.

Der Abfragespeicher enthält drei Speicher:

- einen **Planspeicher**, der die Informationen zum Ausführungsplan speichert
- einen **Speicher für Laufzeitstatistiken**, der die Informationen zum Ausführungsstatistiken speichert
- einen **Speicher für Wartestatistiken**, der die Informationen zum Wartestatistiken speichert

Die Anzahl der eindeutigen Pläne, die für eine Abfrage gespeichert werden können, wird durch die Konfigurationsoption **max_plans_per_query** begrenzt. Zum Verbessern der Leistung werden diese Informationen asynchron in die Speicher geschrieben. Um die Speicherverwendung zu minimieren, werden die statistischen Daten zur Laufzeitausführung im Speicher für Laufzeitstatistiken über ein festes Zeitintervall aggregiert. Die Informationen in diesen Speichern können durch Abfragen der Katalogsichten für den Abfragespeicher angezeigt werden.

Die folgende Abfrage gibt Informationen über Abfragen und Pläne im Abfragespeicher zurück.

```sql
SELECT Txt.query_text_id, Txt.query_sql_text, Pl.plan_id, Qry.*
FROM sys.query_store_plan AS Pl
INNER JOIN sys.query_store_query AS Qry
    ON Pl.query_id = Qry.query_id
INNER JOIN sys.query_store_query_text AS Txt
    ON Qry.query_text_id = Txt.query_text_id ;
```

## <a name="use-the-regressed-queries-feature"></a><a name="Regressed"></a> Verwenden der Funktion „Rückläufige Abfragen“

Aktualisieren Sie nach der Aktivierung des Abfragespeichers den Datenbankbereich im Objekt-Explorer-Bereich, um den Abschnitt **Abfragespeicher** hinzuzufügen.

![SQL Server 2016: Abfragespeicherstruktur im Objekt-Explorer in SSMS](../../relational-databases/performance/media/objectexplorerquerystore.PNG "SQL Server 2016: Abfragespeicherstruktur im Objekt-Explorer in SSMS") ![SQL Server 2017: Abfragespeicherstruktur im Objekt-Explorer in SSMS](../../relational-databases/performance/media/objectexplorerquerystore_sql17.PNG "SQL Server 2017: Abfragespeicherstruktur im Objekt-Explorer in SSMS")

Wählen Sie **Zurückgestellte Abfragen** aus, um den Bereich **Zurückgestellte Abfragen** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]zu öffnen. Im Bereich „Regressed Queries“ werden die Abfragen und Pläne im Abfragespeicher angezeigt. Verwenden Sie die Dropdownfelder im oberen Bereich, um Abfragen anhand verschiedener Kriterien zu filtern: **Dauer (ms)** (Standard), CPU-Zeit (ms), Logische Lesevorgänge (KB), Logische Schreibvorgänge (KB), Physische Lesevorgänge (KB), CLR-Zeit (ms), DOP, Arbeitsspeicherverbrauch (KB), Zeilenanzahl, Verwendeter Protokollspeicher (KB), Verwendeter temporärer DB-Speicher (KB) und Wartezeit (ms).

Wählen Sie einen Plan aus, um die grafische Darstellung des Abfrageplans anzuzeigen. Schaltflächen stehen zur Verfügung, um die Quellabfrage anzuzeigen, einen Abfrageplan zu erzwingen bzw. seine Erzwingung aufzuheben, zwischen Raster- und Diagrammformaten umzuschalten, ausgewählte Pläne zu vergleichen (wenn mehrere Pläne ausgewählt sind) und die Anzeige zu aktualisieren.

![SQL Server 2016: Zurückgestellte Abfragen im Objekt-Explorer in SSMS](../../relational-databases/performance/media/objectexplorerregressedqueries.PNG "SQL Server 2016: Zurückgestellte Abfragen im Objekt-Explorer in SSMS")

Wählen Sie eine Abfrage und einen Plan aus, und klicken Sie anschließend auf **Plan erzwingen**, um einen Plan zu erzwingen. Sie können nur Pläne erzwingen, die mit dem Abfrageplanfeature gespeichert wurden und sich noch im Abfrageplancache befinden.

## <a name="finding-waiting-queries"></a><a name="Waiting"></a> Suchen von Wartestatistiken zu Abfragen

Ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] sind im Abfragespeicher Wartestatistiken pro Abfrage im Zeitverlauf verfügbar.

Im Abfragespeicher werden Wartetypen in **Wartekategorien** zusammengefasst. Die Zuordnung von Wartekategorien zu Wartetypen finden Sie unter [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md#wait-categories-mapping-table).

Wählen Sie **Abfragewartestatistiken** aus, um den Bereich **Abfragewartestatistiken** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v18 oder höher zu öffnen. Der Bereich „Abfragewartestatistiken“ zeigt ein Balkendiagramm mit den wichtigsten Wartekategorien im Abfragespeicher an. Verwenden Sie die Dropdownliste im oberen Bereich, um ein Aggregatkriterium für die Wartezeit auszuwählen: avg, max, min, std dev oder **total** (Standard).

![SQL Server 2017: Statistik der Abfragewartezeit im Objekt-Explorer in SSMS](../../relational-databases/performance/media/query-store-waits.PNG "SQL Server 2017: Statistik der Abfragewartezeit im Objekt-Explorer in SSMS")

Wählen Sie eine Wartekategorie aus, indem Sie auf die Leiste klicken. Eine Detailansicht der ausgewählten Wartekategorie wird angezeigt. Dieses neue Balkendiagramm enthält die Abfragen, die zu dieser Wartekategorie beigetragen haben.

![SQL Server 2017: Detailansicht der Statistik der Abfragewartezeit im Objekt-Explorer in SSMS](../../relational-databases/performance/media/query-store-waits-detail.PNG "SQL Server 2017: Detailansicht der Statistik der Abfragewartezeit im Objekt-Explorer in SSMS")

Verwenden Sie die Dropdownfelder im oberen Bereich, um Abfragen nach verschiedenen Wartezeitkriterien für die ausgewählte Wartekategorie zu filtern: avg, max, min, std dev oder **total** (Standard). Wählen Sie einen Plan aus, um die grafische Darstellung des Abfrageplans anzuzeigen. Über verschiedene Schaltflächen können Sie die Quellabfrage anzeigen, einen Abfrageplan erzwingen und die Erzwingung wieder aufheben und die Ansicht aktualisieren.

**Wartekategorien** fassen mehrere Wartetypen in Buckets zusammen, die sich in ihrer Art ähneln. Verschiedene Wartekategorien erfordern verschiedene Analysen zur Problembehebung. Wartetypen aus der gleichen Kategorien führen jedoch zu sehr ähnlichen Problembehebungsvorgängen. Wenn nun die betroffenen Abfrage in den Wartezuständen bereitgestellt wird, kann ein Großteil der Überprüfungen erfolgreich abgeschlossen werden.

Im folgenden finden Sie einige Beispiele, wie Sie ausführlicheren Einblick in Ihre Workload erhalten, bevor oder nachdem Wartekategorien im Abfragespeicher eingefügt wurden:

|Frühere Erfahrung|Neue Erfahrung|Aktion|
|-|-|-|
|Lange Wartezustände von RESOURCE_SEMAPHORE pro Datenbank|Lange Speicherwartezustände im Abfragespeicher für bestimmte Abfragen|Suchen Sie die im Abfragespeicher die speicherintensivsten Abfragen. Diese Abfragen verzögern wahrscheinlich zusätzlich den Fortschritt der betroffen Abfragen. Ziehen Sie in Betracht, den Abfragehinweis „MAX_GRANT_PERCENT“ für diese Abfragen oder für die betroffene Abfrage zu verwenden.|
|Lange Wartezustände von LCK_M_X pro Datenbank|Lange Sperrwartezustände im Abfragespeicher für bestimmte Abfragen|Überprüfen Sie die Abfragetexte der betroffenen Abfragen, und identifizieren Sie die Zielentitäten. Suchen Sie im Abfragespeicher nach anderen Abfragen, die die gleiche Entität modifizieren und die häufig ausgeführt werden bzw. oder eine lange Dauer haben. Nachdem Sie diese Abfragen ermittelt haben, ändern Sie ggf. die Anwendungslogik, um die Parallelität zu verbessern, oder verwenden Sie eine weniger restriktive Isolationsstufe.|
|Lange Wartezustände von PAGEIOLATCH_SH pro Datenbank|Lange Wartezustände der Puffer-E/A im Abfragespeicher für bestimmte Abfragen|Suchen Sie die Abfragen mit einer hohen Anzahl an physischen Lesevorgängen im Abfragespeicher. Wenn Sie mit den Abfragen mit langen E/A-Wartezuständen übereinstimmen, denken Sie darüber nach, einen Index auf der zugrunde liegenden Entität einzufügen, damit Suchvorgänge statt Scanvorgängen durchgeführt werden und damit der E/A-Aufwand der Abfragen gesenkt wird.|
|Lange Wartezustände von SOS_SCHEDULER_YIELD pro Datenbank|Lange CPU-Wartezustände im Abfragespeicher für bestimmte Abfragen|Machen Sie die Abfragen im Abfragespeicher ausfindig, die am meisten CPU nutzen. Bestimmen Sie dann, welche dieser Abfragen sowohl eine hohe CPU-Auslastung als auch lange CPU-Wartezustände für die betroffenen Abfragen aufweisen. Konzentrieren Sie sich darauf, diese Abfragen zu optimieren: möglicherweise gibt es eine Planregression, oder es fehlt ein Index.|

## <a name="configuration-options"></a><a name="Options"></a> Konfigurationsoptionen

Weitere Informationen zu den verfügbaren Konfigurationsoptionen für Abfragespeicherparameter finden Sie unter [ALTER DATABASE SET-Optionen (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md#query-store).

Fragen Sie die Sicht **sys.database_query_store_options** ab, um die aktuellen Optionen des Abfragespeichers zu ermitteln. Weitere Informationen zu den Werten finden Sie unter [sys.database_query_store_options](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md).

Beispiele für das Festlegen der Konfigurationsoptionen mit [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen finden Sie unter [Optionsverwaltung](#OptionMgmt).

## <a name="related-views-functions-and-procedures"></a><a name="Related"></a> Zugehörige Sichten, Funktionen und Prozeduren

Überprüfen und verwalten Sie den Abfragespeicher mit [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] oder mithilfe der folgenden Sichten und Prozeduren.

### <a name="query-store-functions"></a>Funktionen des Abfragespeichers

Funktionen unterstützen Sie bei den Vorgängen des Abfragespeichers.

:::row:::
    :::column:::
        [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="query-store-catalog-views"></a>Katalogsichten des Abfragespeichers

Katalogsichten stellen Informationen über den Abfragespeicher bereit.

:::row:::
    :::column:::
        [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)
    :::column-end:::
:::row-end:::


### <a name="query-store-stored-procedures"></a>Gespeicherte Prozeduren für den Abfragespeicher

Gespeicherte Prozeduren ermöglichen das Konfigurieren des Abfragespeichers.

:::row:::
    :::column:::
        [sp_query_store_flush_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-flush-db-transact-sql.md)
    :::column-end:::
    :::column:::
        [sp_query_store_reset_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-reset-exec-stats-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sp_query_store_force_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)
    :::column-end:::
    :::column:::
        [sp_query_store_unforce_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sp_query_store_remove_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-plan-transct-sql.md)
    :::column-end:::
    :::column:::
        [sp_query_store_remove_query &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-query-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        sp_query_store_consistency_check &#40;Transact-SQL&#41;<sup>1</sup>
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

<sup>1</sup> In extremen Szenarien kann der Abfragespeicher aufgrund interner Fehler in den Zustand FEHLER geraten. Falls dies eintritt, kann der Abfragespeicher ab SQL Server 2017 (14.x) wiederhergestellt werden, indem in der betroffenen Datenbank die gespeicherte Prozedur sp_query_store_consistency_check ausgeführt wird. Weitere Einzelheiten finden Sie unter [sys.database_query_store_options](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md) in der Beschreibung der Spalte actual_state_desc.

## <a name="key-usage-scenarios"></a><a name="Scenarios"></a> Wichtige Verwendungsszenarien

### <a name="option-management"></a><a name="OptionMgmt"></a> Optionsverwaltung

Dieser Abschnitt enthält einige Richtlinien zum Verwalten des Abfragespeichers selbst.

**Ist der Abfragespeicher derzeit aktiv?**

Der Abfragespeicher speichert seine Daten in der Benutzerdatenbank und besitzt aus diesem Grund eine Größenbegrenzung (konfiguriert mit `MAX_STORAGE_SIZE_MB`). Wenn die Daten im Abfragespeicher dieses Limit erreichen, ändert sich der Status automatisch vom Lese-/ Schreibzugriff in den schreibgeschützten Modus, und es werden keine neuen Daten mehr erfasst.

Fragen Sie [sys.database_query_store_options](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md) ab, um zu ermitteln, ob der Abfragespeicher zurzeit aktiv ist und Laufzeitstatistiken erfasst.

```sql
SELECT actual_state, actual_state_desc, readonly_reason,
    current_storage_size_mb, max_storage_size_mb
FROM sys.database_query_store_options;
```

Der Status des Abfragespeichers wird durch die „actual_state“-Spalte bestimmt. Wenn er vom gewünschten Status abweicht, können Sie über die `readonly_reason`-Spalte weitere Informationen erhalten.
Wenn der Abfragespeicher das Kontingent überschreitet, wird das Feature in den Modus „read_only“ versetzt.

**Abrufen von Optionen zum Abfragespeicher**

Führen Sie zum Abrufen detaillierter Informationen zum Status des Abfragespeichers Folgendes in einer Benutzerdatenbank aus.

```sql
SELECT * FROM sys.database_query_store_options;
```

**Festlegen des Abfragespeicherintervalls**

Sie können das Intervall zum Sammeln von Statistiken zur Abfragelaufzeit  überschreiben (der Standardwert ist 60 Minuten).

```sql
ALTER DATABASE <database_name>
SET QUERY_STORE (INTERVAL_LENGTH_MINUTES = 15);
```

> [!NOTE]
> Ganzzahlige Werte sind für den Typ `INTERVAL_LENGTH_MINUTES` nicht zulässig. Verwenden Sie einen der folgenden Werte: 1, 5, 10, 15, 30, 60 oder 1440 Minuten.

Der neue Wert für das Intervall wird über die Sicht **sys.database_query_store_options** offen gelegt.

**Die Speicherplatzverwendung des Abfragespeichers**

So überprüfen Sie die aktuelle Größe des Abfragespeichers und begrenzen die folgende Anweisung in der Benutzerdatenbank.

```sql
SELECT current_storage_size_mb, max_storage_size_mb
FROM sys.database_query_store_options;
```

Mit der folgenden Anweisung können Sie den Speicher erweitern, wenn der Abfragespeicher voll ist.

```sql
ALTER DATABASE <database_name>
SET QUERY_STORE (MAX_STORAGE_SIZE_MB = <new_size>);
```

**Festlegen von Optionen zum Abfragespeicher**

Sie können mehrere Optionen zum Abfragespeicher gleichzeitig mit einer einzigen ALTER DATABASE-Anweisung festlegen.

```sql
ALTER DATABASE <database name>
SET QUERY_STORE (
    OPERATION_MODE = READ_WRITE,
    CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30),
    DATA_FLUSH_INTERVAL_SECONDS = 3000,
    MAX_STORAGE_SIZE_MB = 500,
    INTERVAL_LENGTH_MINUTES = 15,
    SIZE_BASED_CLEANUP_MODE = AUTO,
    QUERY_CAPTURE_MODE = AUTO,
    MAX_PLANS_PER_QUERY = 1000,
    WAIT_STATS_CAPTURE_MODE = ON
);
```

Eine vollständige Liste der Konfigurationsoptionen finden Sie unter [ALTER DATABASE SET-Optionen (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).

**Bereinigen des Speicherplatzes**

Die internen Tabellen des Abfragespeichers werden beim Erstellen der Datenbank in der PRIMÄREN Dateigruppe erstellt. Diese Konfiguration kann später nicht mehr geändert werden. Wenn nicht mehr genügend Speicherplatz vorhanden ist, sollten Sie mithilfe der folgenden Anweisung ältere Daten aus dem Abfragespeicher löschen.

```sql
ALTER DATABASE <db_name> SET QUERY_STORE CLEAR;
```

Sie können auch nur Ad-hoc-Abfragedaten löschen, da diese evtl. für die Abfrageoptimierung und Plananalyse weniger wichtig sind, aber trotzdem viel Platz einnehmen.

**Löschen von Ad-hoc-Abfragen**

Dadurch werden Ad-hoc- und interne Abfragen alle 3 Minuten aus dem Abfragespeicher gelöscht, damit der Abfragespeicher über ausreichend Speicherplatz verfügt und keine Abfragen entfernt, die unbedingt nachverfolgt werden müssen.

```sql
SET NOCOUNT ON
-- This purges adhoc and internal queries from the query store every 3 minutes so that the
-- query store does not run out of space and remove queries we really need to track
DECLARE @command varchar(1000)

SELECT @command = 'IF ''?'' NOT IN(''master'', ''model'', ''msdb'', ''tempdb'') BEGIN USE ?
EXEC(''
DECLARE @id int
DECLARE adhoc_queries_cursor CURSOR
FOR
SELECT q.query_id
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
ON q.query_text_id = qt.query_text_id
JOIN sys.query_store_plan AS p
ON p.query_id = q.query_id
JOIN sys.query_store_runtime_stats AS rs
ON rs.plan_id = p.plan_id
WHERE q.is_internal_query = 1 ' -- is it an internal query then we dont care to keep track of it

' OR q.object_id = 0' -- if it does not have a valid object_id then it is an adhoc query and we dont care about keeping track of it
' GROUP BY q.query_id
HAVING MAX(rs.last_execution_time) < DATEADD (minute, -5, GETUTCDATE()) ' -- if it has been more than 5 minutes since the adhoc query ran
' ORDER BY q.query_id ;
OPEN adhoc_queries_cursor ;
FETCH NEXT FROM adhoc_queries_cursor INTO @id;
WHILE @@fetch_status = 0
BEGIN
EXEC sp_query_store_remove_query @id
FETCH NEXT FROM adhoc_queries_cursor INTO @id
END
CLOSE adhoc_queries_cursor ;
DEALLOCATE adhoc_queries_cursor;
'') END' ;
EXEC sp_MSforeachdb @command
```

Sie können eine eigene Prozedur mit abweichender Logik für das Bereinigen von Daten definieren, die Sie nicht mehr benötigen.

Im Beispiel oben wird die erweiterte gespeicherte Prozedur **sp_query_store_remove_query** verwendet, um nicht benötigte Daten zu entfernen. Sie können auch Folgendes verwenden:

- **sp_query_store_reset_exec_stats**, um Laufzeitstatistiken für einen bestimmten Plan zu löschen.
- **sp_query_store_remove_plan**, um einen einzelnen Plan zu entfernen.

### <a name="performance-auditing-and-troubleshooting"></a><a name="Peformance"></a> Leistungsüberwachung und Problembehandlung

Der Abfragespeicher protokolliert den Verlauf der Kompilierungs- und Laufzeitmetriken während der Abfrageausführungen, sodass Sie Fragen zu Ihrer Arbeitsauslastung stellen können.

**Welche waren die letzten *n* Abfragen, die für die Datenbank ausgeführt wurden?**

```sql
SELECT TOP 10 qt.query_sql_text, q.query_id,
    qt.query_text_id, p.plan_id, rs.last_execution_time
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_runtime_stats AS rs
    ON p.plan_id = rs.plan_id
ORDER BY rs.last_execution_time DESC;
```

**Wie oft wurde jede Abfrage ausgeführt?**

```sql
SELECT q.query_id, qt.query_text_id, qt.query_sql_text,
    SUM(rs.count_executions) AS total_execution_count
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_runtime_stats AS rs
    ON p.plan_id = rs.plan_id
GROUP BY q.query_id, qt.query_text_id, qt.query_sql_text
ORDER BY total_execution_count DESC;
```

**Wie viele Abfragen weisen die längste durchschnittliche Ausführungszeit innerhalb der letzten Stunde auf?**

```sql
SELECT TOP 10 rs.avg_duration, qt.query_sql_text, q.query_id,
    qt.query_text_id, p.plan_id, GETUTCDATE() AS CurrentUTCTime,
    rs.last_execution_time
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_runtime_stats AS rs
    ON p.plan_id = rs.plan_id
WHERE rs.last_execution_time > DATEADD(hour, -1, GETUTCDATE())
ORDER BY rs.avg_duration DESC;
```

**Wie viele Abfragen weisen die umfangreichsten durchschnittlichen physischen E/A-Lesevorgänge in den letzten 24 Stunden auf (mit der entsprechenden durchschnittlichen Zeilen- und Ausführungsanzahl)?**

```sql
SELECT TOP 10 rs.avg_physical_io_reads, qt.query_sql_text,
    q.query_id, qt.query_text_id, p.plan_id, rs.runtime_stats_id,
    rsi.start_time, rsi.end_time, rs.avg_rowcount, rs.count_executions
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_runtime_stats AS rs
    ON p.plan_id = rs.plan_id
JOIN sys.query_store_runtime_stats_interval AS rsi
    ON rsi.runtime_stats_interval_id = rs.runtime_stats_interval_id
WHERE rsi.start_time >= DATEADD(hour, -24, GETUTCDATE())
ORDER BY rs.avg_physical_io_reads DESC;
```

**Gab es Abfragen mit mehreren Plänen?** Diese Abfragen sind besonders interessant, da sie bei Änderungen der Planauswahl zu einer Regression führen können. Die folgende Abfrage erkennt diese Abfragen sowie alle Pläne:

```sql
WITH Query_MultPlans
AS
(
SELECT COUNT(*) AS cnt, q.query_id
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON p.query_id = q.query_id
GROUP BY q.query_id
HAVING COUNT(distinct plan_id) > 1
)

SELECT q.query_id, object_name(object_id) AS ContainingObject,
    query_sql_text, plan_id, p.query_plan AS plan_xml,
    p.last_compile_start_time, p.last_execution_time
FROM Query_MultPlans AS qm
JOIN sys.query_store_query AS q
    ON qm.query_id = q.query_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_query_text qt
    ON qt.query_text_id = q.query_text_id
ORDER BY query_id, plan_id;
```

 **Gab es Abfragen, bei denen kürzlich eine Leistungsregression stattgefunden hat (im Vergleich zu einem anderen Zeitpunkt)?** Das folgende Abfragebeispiel gibt alle Abfragen zurück, für die sich die Ausführungszeit in den letzten 48 Stunden aufgrund einer Änderung der Planauswahl verdoppelt hat. Die Abfrage vergleicht alle Intervalle der Laufzeitstatistiken miteinander.

```sql
SELECT
    qt.query_sql_text,
    q.query_id,
    qt.query_text_id,
    rs1.runtime_stats_id AS runtime_stats_id_1,
    rsi1.start_time AS interval_1,
    p1.plan_id AS plan_1,
    rs1.avg_duration AS avg_duration_1,
    rs2.avg_duration AS avg_duration_2,
    p2.plan_id AS plan_2,
    rsi2.start_time AS interval_2,
    rs2.runtime_stats_id AS runtime_stats_id_2
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p1
    ON q.query_id = p1.query_id
JOIN sys.query_store_runtime_stats AS rs1
    ON p1.plan_id = rs1.plan_id
JOIN sys.query_store_runtime_stats_interval AS rsi1
    ON rsi1.runtime_stats_interval_id = rs1.runtime_stats_interval_id
JOIN sys.query_store_plan AS p2
    ON q.query_id = p2.query_id
JOIN sys.query_store_runtime_stats AS rs2
    ON p2.plan_id = rs2.plan_id
JOIN sys.query_store_runtime_stats_interval AS rsi2
    ON rsi2.runtime_stats_interval_id = rs2.runtime_stats_interval_id
WHERE rsi1.start_time > DATEADD(hour, -48, GETUTCDATE())
    AND rsi2.start_time > rsi1.start_time
    AND p1.plan_id <> p2.plan_id
    AND rs2.avg_duration > 2*rs1.avg_duration
ORDER BY q.query_id, rsi1.start_time, rsi2.start_time;
```

Wenn Sie alle Leistungsregressionen (nicht nur die im Zusammenhang mit einer Änderung der Planauswahl) anzeigen möchten, entfernen Sie einfach die Bedingung `AND p1.plan_id <> p2.plan_id` aus der vorherigen Abfrage.

**Abfragen mit der längsten Wartezeit?**
Die Abfrage gibt die 10 Abfragen zurück, die am längsten warten.

```sql
SELECT TOP 10
    qt.query_text_id,
    q.query_id,
    p.plan_id,
    sum(total_query_wait_time_ms) AS sum_total_wait_ms
FROM sys.query_store_wait_stats ws
JOIN sys.query_store_plan p ON ws.plan_id = p.plan_id
JOIN sys.query_store_query q ON p.query_id = q.query_id
JOIN sys.query_store_query_text qt ON q.query_text_id = qt.query_text_id
GROUP BY qt.query_text_id, q.query_id, p.plan_id
ORDER BY sum_total_wait_ms DESC
```

**Gab es Abfragen, bei denen kürzlich eine Leistungsregression stattgefunden hat (Vergleich jüngerer und älterer Ausführungen)?** Die nächste Abfrage vergleicht die Abfrageausführung basierend auf den Zeiträumen der Ausführung. In diesem speziellen Beispiel vergleicht die Abfrage die Ausführung in jüngster Zeit (1 Stunde) mit einem älteren Zeitraum (letzter Tag) und identifiziert Ausführungen, die zu `additional_duration_workload`geführt haben. Diese Metrik wird als Differenz zwischen aktueller durchschnittlicher Ausführung und älterer durchschnittlicher Ausführung multipliziert mit der Anzahl der letzten Ausführungen berechnet. Sie stellt damit tatsächlich dar, wie viel zusätzliche Zeit die letzten Ausführungen im Vergleich zu älteren benötigt haben:

```sql
--- "Recent" workload - last 1 hour
DECLARE @recent_start_time datetimeoffset;
DECLARE @recent_end_time datetimeoffset;
SET @recent_start_time = DATEADD(hour, -1, SYSUTCDATETIME());
SET @recent_end_time = SYSUTCDATETIME();

--- "History" workload
DECLARE @history_start_time datetimeoffset;
DECLARE @history_end_time datetimeoffset;
SET @history_start_time = DATEADD(hour, -24, SYSUTCDATETIME());
SET @history_end_time = SYSUTCDATETIME();

WITH
hist AS
(
    SELECT
        p.query_id query_id,
        ROUND(ROUND(CONVERT(FLOAT, SUM(rs.avg_duration * rs.count_executions)) * 0.001, 2), 2) AS total_duration,
        SUM(rs.count_executions) AS count_executions,
        COUNT(distinct p.plan_id) AS num_plans
     FROM sys.query_store_runtime_stats AS rs
        JOIN sys.query_store_plan AS p ON p.plan_id = rs.plan_id
    WHERE (rs.first_execution_time >= @history_start_time
               AND rs.last_execution_time < @history_end_time)
        OR (rs.first_execution_time <= @history_start_time
               AND rs.last_execution_time > @history_start_time)
        OR (rs.first_execution_time <= @history_end_time
               AND rs.last_execution_time > @history_end_time)
    GROUP BY p.query_id
),
recent AS
(
    SELECT
        p.query_id query_id,
        ROUND(ROUND(CONVERT(FLOAT, SUM(rs.avg_duration * rs.count_executions)) * 0.001, 2), 2) AS total_duration,
        SUM(rs.count_executions) AS count_executions,
        COUNT(distinct p.plan_id) AS num_plans
    FROM sys.query_store_runtime_stats AS rs
        JOIN sys.query_store_plan AS p ON p.plan_id = rs.plan_id
    WHERE  (rs.first_execution_time >= @recent_start_time
               AND rs.last_execution_time < @recent_end_time)
        OR (rs.first_execution_time <= @recent_start_time
               AND rs.last_execution_time > @recent_start_time)
        OR (rs.first_execution_time <= @recent_end_time
               AND rs.last_execution_time > @recent_end_time)
    GROUP BY p.query_id
)
SELECT
    results.query_id AS query_id,
    results.query_text AS query_text,
    results.additional_duration_workload AS additional_duration_workload,
    results.total_duration_recent AS total_duration_recent,
    results.total_duration_hist AS total_duration_hist,
    ISNULL(results.count_executions_recent, 0) AS count_executions_recent,
    ISNULL(results.count_executions_hist, 0) AS count_executions_hist
FROM
(
    SELECT
        hist.query_id AS query_id,
        qt.query_sql_text AS query_text,
        ROUND(CONVERT(float, recent.total_duration/
                   recent.count_executions-hist.total_duration/hist.count_executions)
               *(recent.count_executions), 2) AS additional_duration_workload,
        ROUND(recent.total_duration, 2) AS total_duration_recent,
        ROUND(hist.total_duration, 2) AS total_duration_hist,
        recent.count_executions AS count_executions_recent,
        hist.count_executions AS count_executions_hist
    FROM hist
        JOIN recent
            ON hist.query_id = recent.query_id
        JOIN sys.query_store_query AS q
            ON q.query_id = hist.query_id
        JOIN sys.query_store_query_text AS qt
            ON q.query_text_id = qt.query_text_id
) AS results
WHERE additional_duration_workload > 0
ORDER BY additional_duration_workload DESC
OPTION (MERGE JOIN);
```

### <a name="maintaining-query-performance-stability"></a><a name="Stability"></a> Erhalten einer stabilen Abfrageleistung

Möglicherweise ist Ihnen bereits aufgefallen, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verschiedene Pläne für mehrmals ausgeführte Abfragen verwendet, was zu unterschiedlicher Ressourcenverwendung und Dauer führt. Mit dem Abfragespeicher können Sie erkennen, wenn die Abfrageleistung abfällt, und den optimalen Plan innerhalb des gewünschten Zeitraums bestimmen. Anschließend können Sie diesen optimalen Plan für zukünftige Abfrageausführungen erzwingen.

Sie können auch abweichende Abfrageleistungen für eine Abfrage mit Parametern ermitteln (entweder automatisch oder manuell parametrisiert). Sie können unter den verschiedenen Plänen den Plan identifizieren, der schnell und ausreichend geeignet für alle oder die meisten Parameterwerte ist, und diesen anschließend erzwingen. So erhalten Sie eine vorhersagbare Leistung für eine größere Anzahl von Benutzerszenarien.

### <a name="force-a-plan-for-a-query-apply-forcing-policy"></a>Erzwingen eines Plans für eine Abfrage (Anwenden einer Durchsetzungsrichtlinie)

Wenn ein Plan für eine bestimmte Abfrage erzwungen wird, versucht [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], den Plan im Optimierer zu erzwingen. Wenn das Erzwingen des Plans fehlschlägt, wird ein XEvent ausgelöst, und der Optimierer wird angewiesen, die Optimierung auf die übliche Weise durchzuführen.

```sql
EXEC sp_query_store_force_plan @query_id = 48, @plan_id = 49;
```

Durch die Verwendung von **sp_query_store_force_plan** können Sie ausschließlich solche Pläne erzwingen, die vom Abfragespeicher als Plan für diese Abfrage aufgezeichnet wurden. Es stehen für eine Abfrage also nur Pläne zur Verfügung, die bereits zum Ausführen dieser Abfrage verwendet wurden, während der Abfragespeicher aktiv war.

#### <a name="a-namectp23-plan-forcing-support-for-fast-forward-and-static-cursors"></a><a name="ctp23"><a/> Erzwingen eines Plans für schnelle Vorwärts- und statische Cursor

Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] und Azure SQL-Datenbank (alle Bereitstellungsmodelle) unterstützt der Abfragespeicher die Möglichkeit, Abfrageausführungspläne für schnelle Vorwärtscursor und statische Cursor ([!INCLUDE[tsql](../../includes/tsql-md.md)] und API) zu erzwingen. Das Erzwingen wird durch `sp_query_store_force_plan` oder [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]-Abfragespeicherberichte unterstützt.

### <a name="remove-plan-forcing-for-a-query"></a>Aufheben der Erzwingung eines Plans für eine Abfrage

Wenn Sie wieder den Abfrageoptimierer von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwenden möchten, um den optimalen Abfrageplan zu berechnen, heben Sie mit **sp_query_store_unforce_plan** das Erzwingen des für die Abfrage ausgewählten Plans auf.

```sql
EXEC sp_query_store_unforce_plan @query_id = 48, @plan_id = 49;
```

## <a name="see-also"></a>Weitere Informationen

- [Bewährte Methoden für den Abfragespeicher](../../relational-databases/performance/best-practice-with-the-query-store.md)
- [Verwenden des Abfragespeichers mit In-Memory-OLTP](../../relational-databases/performance/using-the-query-store-with-in-memory-oltp.md)
- [Query Store Usage Scenarios (Verwendungsszenarien für den Abfragespeicher)](../../relational-databases/performance/query-store-usage-scenarios.md)
- [So werden Daten im Abfragespeicher gesammelt](../../relational-databases/performance/how-query-store-collects-data.md)
- [Gespeicherte Prozeduren für den Abfragespeicher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)
- [Katalogsichten des Abfragespeichers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)
- [Überwachen und Optimieren der Leistung](../../relational-databases/performance/monitor-and-tune-for-performance.md)
- [Tools für die Leistungsüberwachung und -optimierung](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)
- [Öffnen des Aktivitätsmonitors &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)
- [Live-Abfragestatistik](../../relational-databases/performance/live-query-statistics.md)
- [Aktivitätsmonitor](../../relational-databases/performance-monitor/activity-monitor.md)
- [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)
