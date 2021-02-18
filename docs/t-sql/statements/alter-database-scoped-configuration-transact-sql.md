---
title: ALTER DATABASE SCOPED CONFIGURATION
description: Diese Anweisung aktiviert mehrere Einstellungen für die Datenbankkonfiguration auf der Ebene einzelner Datenbanken.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 02/08/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- ALTER_DATABASE_SCOPED_CONFIGURATION
- ALTER_DATABASE_SCOPED_CONFIGURATION_TSQL
- DATABASE_SCOPED_CONFIGURATION_TSQL
- SCOPED_CONFIGURATION_TSQL
- SCOPED_TSQL
- ALTER_DATABASE_SCOPED_TSQL
- DATABASE_SCOPED_TSQL
helpviewer_keywords:
- ALTER DATABASE SCOPED CONFIGURATION statement
- configuration [SQL Server], ALTER DATABASE SCOPED CONFIGURATION statement
ms.assetid: 63373c2f-9a0b-431b-b9d2-6fa35641571a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017 ||=azure-sqldw-latest
ms.openlocfilehash: 647e68fa167b24bc713266fd45527337bdd6b9e6
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100348539"
---
# <a name="alter-database-scoped-configuration-transact-sql"></a>ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)

[!INCLUDE[tsql-appliesto-ss2016-asdb-asdw-xxx-md.md](../../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]

Dieser Befehl aktiviert mehrere Einstellungen für die Datenbankkonfiguration auf der Ebene **einzelner Datenbanken**. 

Die folgenden Einstellungen werden, wie durch die Zeile **GILT FÜR** für jede Einstellung im Abschnitt [Argumente](#arguments) angegeben, in [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] und in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt: 

- Löschen des Prozedurcaches.
- Festlegen des MAXDOP-Parameters auf einen beliebigen Wert (1,2, ...) für die primäre Datenbank, basierend auf dem, was am besten für diese bestimmte Datenbank ist, und Festlegen eines anderen Werts (z. B. 0) für alle verwendeten sekundären Datenbanken (z. B. für Berichtsabfragen).
- Festlegen des Kardinalitätsschätzungsmodells für den Abfrageoptimierer unabhängig von der Datenbank auf den Kompatibilitätsgrad.
- Aktivieren oder Deaktivieren der Parameterermittlung auf Datenbankebene.
- Aktivieren oder Deaktivieren der Abfrageoptimierungs-Hotfixes auf Datenbankebene.
- Aktivieren oder Deaktivieren des Identitätscache auf Datenbankebene.
- Aktivieren oder Deaktivieren eines Stubs des kompilierten Plans, der bei der erstmaligen Kompilierung eines Batches im Cache gespeichert werden soll.
- Aktivieren oder Deaktivieren von Sammlungen von Ausführungsstatistiken für nativ kompilierte [!INCLUDE[tsql](../../includes/tsql-md.md)]-Module.
- Aktivieren oder Deaktivieren von „online by default“-Optionen (Standardmäßig online) für DDL-Anweisungen, die die `ONLINE =`-Syntax unterstützen.
- Aktivieren oder Deaktivieren von „resumable by default“-Optionen (Standardmäßig fortsetzbar) für DDL-Anweisungen, die die `RESUMABLE =`-Syntax unterstützen.
- Aktivieren oder Deaktivieren der Features der [intelligenten Abfrageverarbeitung](../../relational-databases/performance/intelligent-query-processing.md)
- Aktivieren oder Deaktivieren des beschleunigten Erzwingens des Plans.
- Aktivieren oder Deaktivieren der Funktion für automatisches Löschen von globalen temporären Tabellen
- Aktivieren oder Deaktivieren der [einfachen Profilerstellungsinfrastruktur für Abfragen](../../relational-databases/performance/query-profiling-infrastructure.md)
- Aktivieren oder Deaktivieren der neuen `String or binary data would be truncated`-Fehlermeldung
- Aktivieren oder Deaktivieren des letzten tatsächlichen Ausführungsplans in [sys.dm_exec_query_plan_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md)
- Geben Sie die Anzahl der Minuten an, in denen ein angehaltener fortsetzbarer Indexvorgang angehalten wird, bevor er von der [!INCLUDE[ssde_md](../../includes/ssde_md.md)] automatisch abgebrochen wird.
- Aktivieren oder Deaktivieren des Wartens auf Sperren mit niedriger Priorität für asynchrone Statistikupdates.

Diese Einstellung ist nur in [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] verfügbar.
- Festlegen des Kompatibilitätsgrads von Benutzerdatenbanken

![Linksymbol](../../database-engine/configure-windows/media/topic-link.gif "Linksymbol") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql
-- Syntax for SQL Server, Azure SQL Database and Azure SQL Managed Instance

ALTER DATABASE SCOPED CONFIGURATION
{
    { [ FOR SECONDARY] SET <set_options>}
}
| CLEAR PROCEDURE_CACHE [plan_handle]
| SET < set_options >
[;]

< set_options > ::=
{
    MAXDOP = { <value> | PRIMARY}
    | LEGACY_CARDINALITY_ESTIMATION = { ON | OFF | PRIMARY}
    | PARAMETER_SNIFFING = { ON | OFF | PRIMARY}
    | QUERY_OPTIMIZER_HOTFIXES = { ON | OFF | PRIMARY}
    | IDENTITY_CACHE = { ON | OFF }
    | INTERLEAVED_EXECUTION_TVF = { ON | OFF }
    | BATCH_MODE_MEMORY_GRANT_FEEDBACK = { ON | OFF }
    | BATCH_MODE_ADAPTIVE_JOINS = { ON | OFF }
    | TSQL_SCALAR_UDF_INLINING = { ON | OFF }
    | ELEVATE_ONLINE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }
    | ELEVATE_RESUMABLE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }
    | OPTIMIZE_FOR_AD_HOC_WORKLOADS = { ON | OFF }
    | XTP_PROCEDURE_EXECUTION_STATISTICS = { ON | OFF }
    | XTP_QUERY_EXECUTION_STATISTICS = { ON | OFF }
    | ROW_MODE_MEMORY_GRANT_FEEDBACK = { ON | OFF }
    | BATCH_MODE_ON_ROWSTORE = { ON | OFF }
    | DEFERRED_COMPILATION_TV = { ON | OFF }
    | ACCELERATED_PLAN_FORCING = { ON | OFF }
    | GLOBAL_TEMPORARY_TABLE_AUTO_DROP = { ON | OFF }
    | LIGHTWEIGHT_QUERY_PROFILING = { ON | OFF }
    | VERBOSE_TRUNCATION_WARNINGS = { ON | OFF }
    | LAST_QUERY_PLAN_STATS = { ON | OFF }
    | PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES = <time>
    | ISOLATE_SECURITY_POLICY_CARDINALITY  = { ON | OFF }
    | EXEC_QUERY_STATS_FOR_SCALAR_FUNCTIONS = { ON | OFF }
    | ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY = { ON | OFF }
}
```

> [!IMPORTANT]
> Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] haben sich einige Optionsnamen geändert:      
> -  `DISABLE_INTERLEAVED_EXECUTION_TVF` wurde in `INTERLEAVED_EXECUTION_TVF` geändert
> -  `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK` wurde in `BATCH_MODE_MEMORY_GRANT_FEEDBACK` geändert
> -  `DISABLE_BATCH_MODE_ADAPTIVE_JOINS` wurde in `BATCH_MODE_ADAPTIVE_JOINS` geändert

```syntaxsql
-- Syntax for Azure Synapse Analytics

ALTER DATABASE SCOPED CONFIGURATION
{
    SET <set_options>
}
[;]

< set_options > ::=
{
    DW_COMPATIBILITY_LEVEL = { AUTO | 10 | 20 } 
}
```

## <a name="arguments"></a>Argumente

FOR SECONDARY

Gibt die Einstellungen für sekundäre Datenbanken an (alle sekundären Datenbanken müssen identische Werte aufweisen).

CLEAR PROCEDURE_CACHE [plan_handle]

Löscht den Prozedurcache (Plan) für die Datenbank und kann sowohl in den primären als auch den sekundären Datenbanken ausgeführt werden.

Geben Sie ein Abfrageplanhandle an, um einen einzelnen Abfrageplan aus dem Plancache zu löschen.

**GILT FÜR**: Die Angabe eines Abfrageplanhandles ist ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] verfügbar.

MAXDOP **=** {\<value> | PRIMARY } **\<value>**

Gibt die Standardeinstellung **Max. Grad an Parallelität (MAXDOP)** an, die für Anweisungen verwendet werden sollte. 0 ist der Standardwert und gibt an, dass die Serverkonfiguration stattdessen verwendet wird. Mit der MAXDOP-Einstellung im Datenbankbereich wird der **max. Grad an Parallelität** auf der Serverebene von sp_configure überschrieben (sofern sie nicht auf 0 festgelegt ist). Abfragehinweise können die MAXDOP-Einstellung im Datenbankbereich weiterhin überschreiben, damit bestimmte Abfragen optimiert werden können, für die andere Einstellungen erforderlich sind. All diese Einstellungen werden durch die MAXDOP-Einstellung für die [Arbeitsauslastungsgruppe](create-workload-group-transact-sql.md) begrenzt.

Sie können mithilfe der MAXDOP-Option die Anzahl der Prozessoren beschränken, die für die Ausführung paralleler Pläne verwendet werden. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] berücksichtigt die Ausführung paralleler Pläne für Abfragen, DDL-Indizierungsoperationen (Datendefinitionssprache, Data Definition Language, DDL), parallele Einfügevorgänge, Onlineausführung von ALTER COLUMN, parallele Sammlung von Statistiken sowie die statische und keysetgesteuerte Cursorauffüllung.

> [!NOTE]
> Der Grenzwert für **Max. Grad an Parallelität** wird [taskbezogen](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md) festgelegt. Es handelt sich nicht um einen [anforderungs](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)- oder abfragebezogenen Grenzwert. Das bedeutet, dass während einer parallelen Abfrageausführung eine einzelne Abfrage mehrere Tasks erzeugen kann, die einem [Planer](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md) zugeordnet sind. Weitere Informationen finden Sie im [Handbuch zur Thread- und Taskarchitektur](../../relational-databases/thread-and-task-architecture-guide.md).

Informationen zum Festlegen dieser Option auf Instanzebene finden Sie unter [Konfigurieren der Serverkonfigurationsoption „Max. Grad an Parallelität“](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

> [!NOTE]
> In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ist die datenbankweite Konfiguration von MAXDOP für neue Singletons und elastische Datenbanken in Pools standardmäßig auf 8 begrenzt. Der maximale Grad an Parallelität kann für jede Datenbank, wie im aktuellen Artikel beschrieben, konfiguriert werden. Empfehlungen zur optimalen Konfiguration des maximalen Grads an Parallelität finden Sie im Abschnitt [Weitere Ressourcen](#additional-resources).

> [!TIP]
> Verwenden Sie den [Abfragehinweis](../../t-sql/queries/hints-transact-sql-query.md) **MAXDOP**, um dies auf Abfrageebene zu erreichen.    
> Verwenden Sie den **MAXDOP**-Wert (maximaler Parallelitätsgrad) der [Serverkonfigurationsoption](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md), um dies auf Serverebene zu erreichen.     
> Verwenden Sie die [Resource Governor-Konfigurationsoption **MAX_DOP** für die Arbeitsauslastungsgruppe](../../t-sql/statements/create-workload-group-transact-sql.md), um dies auf Arbeitsauslastungsebene zu erreichen.    

PRIMARY

Kann nur für die sekundären Datenbanken festgelegt werden, während die betreffende Datenbank die primäre ist, und gibt an, dass diese Konfiguration für die primäre Datenbank festgelegt wird. Wenn sich die Konfiguration für die primäre Datenbank ändert, ändert sich der Wert für die sekundären Datenbanken entsprechend, ohne dass dieser Wert explizit festgelegt werden muss. **PRIMARY** ist die Standardeinstellung für die sekundären Datenbanken.

LEGACY_CARDINALITY_ESTIMATION **=** { ON | **OFF** | PRIMARY }

Damit können Sie das Kardinalitätsschätzungsmodell für den Abfrageoptimierer unabhängig vom Kompatibilitätsgrad der Datenbank in SQL Server 2012 und früheren Versionen festlegen. Die Standardeinstellung ist **OFF**. Sie legt das Kardinalitätsschätzungsmodell für den Abfrageoptimierer basierend auf dem Kompatibilitätsgrad der Datenbank fest. Das Festlegen von LEGACY_CARDINALITY_ESTIMATION auf **ON** entspricht der Aktivierung des [Ablaufverfolgungsflags 9481](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

> [!TIP]
> Fügen Sie den [Abfragehinweis](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) **QUERYTRACEON** hinzu, um dies auf Abfrageebene zu erreichen.
> Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 müssen Sie den [Abfragehinweis](../../t-sql/queries/hints-transact-sql-query.md#use_hint) **USE HINT** hinzufügen, statt das Ablaufverfolgungsflag zu verwenden, um dies auf Abfrageebene zu erreichen.

PRIMARY

Dieser Wert ist nur für sekundäre Datenbanken gültig, während die betreffende Datenbank die primäre ist, und gibt an, dass es sich bei der Einstellung des Kardinalitätsschätzungsmodells für den Abfrageoptimierer für alle sekundären Datenbanken um den für die primäre Datenbank festgelegten Wert handelt. Wenn sich die Konfiguration für die primäre Datenbank im Kardinalitätsschätzungsmodell des Abfrageoptimierers ändert, ändert sich der Wert für die sekundären Datenbanken entsprechend. **PRIMARY** ist die Standardeinstellung für die sekundären Datenbanken.

PARAMETER_SNIFFING **=** { **ON** | OFF | PRIMARY }

Aktiviert oder deaktiviert die [Parameterermittlung](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing). Der Standardwert ist ON. Das Festlegen von PARAMETER_SNIFFING auf OFF entspricht der Aktivierung des [Ablaufverfolgungsflags 4136](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

> [!TIP]
> Informationen darüber, wie Sie dies auf Abfrageebene erreichen, finden Sie unter dem [Abfragehinweis](../../t-sql/queries/hints-transact-sql-query.md) **OPTIMIZE FOR UNKNOWN**.
> Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 ist der [Abfragehinweis](../../t-sql/queries/hints-transact-sql-query.md#use_hint) **USE HINT** auch verfügbar, damit dies auf Abfrageebene erreicht werden kann.

PRIMARY

Dieser Wert ist nur für sekundäre Datenbanken gültig, während die betreffende Datenbank primär ist, und gibt an, dass es sich bei dem Wert für diese Einstellung für alle sekundären Datenbanken um den für die primäre Datenbank festgelegten Wert handelt. Wenn sich die Konfiguration für die primäre Datenbank bei der Verwendung der [Parameterermittlung](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing) ändert, ändert sich der Wert für die sekundären Datenbanken entsprechend, ohne dass dieser Wert explizit festgelegt werden muss. PRIMARY ist die Standardeinstellung für die sekundären Datenbanken.

<a name="qo_hotfixes"></a> QUERY_OPTIMIZER_HOTFIXES **=** { ON | **OFF** | PRIMARY }

Aktiviert oder deaktiviert Hotfixes für die Abfrageoptimierung unabhängig vom Kompatibilitätsgrad der Datenbank. Die Standardeinstellung ist **OFF**. Sie deaktiviert Hotfixes für Abfrageoptimierer, die nach der Einführung des höchsten verfügbaren Kompatibilitätsgrads für eine bestimmte Version (post-RTM) veröffentlicht wurden. Ein Festlegen dieser Einstellung auf **ON** entspricht der Aktivierung des [Ablaufverfolgungsflags 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

> [!TIP]
> Fügen Sie den [Abfragehinweis](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) **QUERYTRACEON** hinzu, um dies auf Abfrageebene zu erreichen.
> Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 müssen Sie den [Abfragehinweis](../../t-sql/queries/hints-transact-sql-query.md#use_hint) USE HINT hinzufügen, statt das Ablaufverfolgungsflag zu verwenden, um dies auf Abfrageebene zu erreichen.

PRIMARY

Dieser Wert ist nur für sekundäre Datenbanken gültig, während die betreffende Datenbank primär ist, und gibt an, dass es sich bei dem Wert für diese Einstellung für alle sekundären Datenbanken um den für die primäre Datenbank festgelegten Wert handelt. Wenn sich die Konfiguration für die primäre Datenbank ändert, ändert sich der Wert für die sekundären Datenbanken entsprechend, ohne dass dieser Wert explizit festgelegt werden muss. PRIMARY ist die Standardeinstellung für die sekundären Datenbanken.

IDENTITY_CACHE **=** { **ON** | OFF }

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Aktiviert oder deaktiviert den Identitätscache auf Datenbankebene. Der Standardwert ist **ON**. Identitätszwischenspeichern wird verwendet, um die Leistung von INSERT in Tabellen mit Identitätsspalten zu verbessern. Deaktiviert die Option IDENTITY_CACHE, um Lücken in einer Identitätsspalte zu vermeiden, wenn der Server unerwartet neu gestartet oder ein Failover zu einem sekundären Server ausgeführt wird. Diese Option ist mit dem vorhandenen [Ablaufverfolgungsflag 272](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) vergleichbar. Der einzige Unterschied besteht darin, dass sie auf Datenbankebene und nicht nur auf Serverebene festgelegt werden kann.

> [!NOTE]
> Diese Option kann nur für PRIMARY festgelegt werden. Weitere Informationen finden Sie unter [Identitätsspalten](create-table-transact-sql-identity-property.md).

INTERLEAVED_EXECUTION_TVF **=** { **ON** | OFF }

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Ermöglicht Ihnen das Aktivieren bzw. Deaktivieren der verschachtelten Ausführung für Tabellenwertfunktionen mit mehreren Anweisungen im Datenbank- oder Anweisungsbereich. Dabei kann ein Datenbank-Kompatibilitätsgrad von mindestens 140 beibehalten werden. Verschachtelte Funktionen stellen ein Feature der adaptiven Abfrageverarbeitung in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] dar. Weitere Informationen finden Sie unter [Intelligente Abfrageverarbeitung in SQL-Datenbanken](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Für Datenbank-Kompatibilitätsgrade von 130 oder weniger hat diese datenbankbezogene Konfiguration keine Auswirkungen.
>
> Nur in SQL Server 2017 (14.x) wurde für die Option INTERLEAVED_EXECUTION_TVF der ältere Name **DISABLE** _INTERLEAVED_EXECUTION_TVF verwendet.

BATCH_MODE_MEMORY_GRANT_FEEDBACK **=** { **ON** | OFF }

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Ermöglicht Ihnen das Aktivieren bzw. Deaktivieren von Feedback zur Speicherzuweisung im Batchmodus im Datenbankbereich. Dabei kann ein Datenbank-Kompatibilitätsgrad von mindestens 140 beibehalten werden. Das Feedback zur Speicherzuweisung im Batchmodus stellt einen Bestandteil der [intelligenten Abfrageverarbeitung](../../relational-databases/performance/intelligent-query-processing.md) dar, die in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] eingeführt wurde.

> [!NOTE]
> Für Datenbank-Kompatibilitätsgrade von 130 oder weniger hat diese datenbankbezogene Konfiguration keine Auswirkungen.

BATCH_MODE_ADAPTIVE_JOINS **=** { **ON** | OFF }

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Ermöglicht Ihnen das Aktivieren bzw. Deaktivieren von adaptiven Joins im Batchmodus im Datenbankbereich. Dabei kann ein Datenbank-Kompatibilitätsgrad von mindestens 140 beibehalten werden. Adaptive Joins im Batchmodus stellen einen Bestandteil der [intelligenten Abfrageverarbeitung](../../relational-databases/performance/intelligent-query-processing.md) dar, die in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] eingeführt wurde.

> [!NOTE]
> Für Datenbank-Kompatibilitätsgrade von 130 oder weniger hat diese datenbankbezogene Konfiguration keine Auswirkungen.

TSQL_SCALAR_UDF_INLINING **=** { **ON** | OFF }

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (Feature der Public Preview)

Ermöglicht Ihnen das Aktivieren bzw. Deaktivieren des Inlining benutzerdefinierter T-SQL-Skalarfunktionen im Datenbankbereich. Dabei kann ein Datenbank-Kompatibilitätsgrad von mindestens 150 beibehalten werden. Das Inlining von benutzerdefinierten T-SQL-Skalarfunktionen gehört zur Featurefamilie der [intelligenten Abfrageverarbeitung](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Für Datenbank-Kompatibilitätsgrade von 140 oder weniger hat diese datenbankbezogene Konfiguration keine Auswirkungen.

ELEVATE_ONLINE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Ermöglicht es Ihnen, Optionen auszuwählen, die das Modul dazu veranlassen, unterstützte Vorgänge automatisch in den Onlinezustand zu erhöhen. Der Standardwert ist OFF, was bedeutet, dass Vorgänge nicht in den Onlinezustand erhöht werden, es sei denn, dies ist in der Anweisung angegeben. [sys. database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) enthält den aktuellen Wert von ELEVATE_ONLINE. Diese Optionen gelten nur für Vorgänge, die für den Onlinezustand unterstützt werden.

FAIL_UNSUPPORTED

Dieser Wert erhöht alle unterstützten DDL-Vorgänge in ONLINE. Vorgänge, die keine Onlineausführung unterstützen, schlagen fehl und lösen eine Warnung aus.

WHEN_SUPPORTED

Dieser Wert erhöht Vorgänge, die ONLINE unterstützen. Vorgänge, die den Onlinezustand nicht unterstützen, werden offline ausgeführt.

> [!NOTE]
> Sie können die Standardeinstellung überschreiben, indem Sie eine Anweisung senden, in der die ONLINE-Option angegeben ist.

ELEVATE_RESUMABLE= { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Ermöglicht es Ihnen, Optionen auszuwählen, die das Modul dazu veranlassen, unterstützte Vorgänge automatisch in fortsetzbar zu erhöhen. Der Standardwert ist OFF, was bedeutet, dass Vorgänge nicht in fortsetzbar erhöht werden, es sei denn, dies ist in der Anweisung angegeben. [sys. database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) enthält den aktuellen Wert von ELEVATE_RESUMABLE. Diese Optionen gelten nur für Vorgänge, die für fortsetzbar unterstützt werden.

FAIL_UNSUPPORTED

Dieser Wert erhöht alle unterstützten DDL-Vorgänge in RESUMABLE. Vorgänge, die keine fortsetzbare Ausführung unterstützen, schlagen fehl und lösen eine Warnung aus.

WHEN_SUPPORTED

Dieser Wert erhöht Vorgänge, die RESUMABLE unterstützen. Vorgänge, die fortsetzbar nicht unterstützen, werden als nicht fortsetzbar ausgeführt.

> [!NOTE]
> Sie können die Standardeinstellung überschreiben, indem Sie eine Anweisung senden, in der die RESUMABLE-Option angegeben ist.

OPTIMIZE_FOR_AD_HOC_WORKLOADS **=** { ON | **OFF** }

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Aktiviert oder deaktiviert einen Stub des kompilierten Plans, der bei der erstmaligen Kompilierung eines Batches im Cache gespeichert werden soll. Der Standardwert ist OFF. Sobald die datenbankweite Konfiguration OPTIMIZE_FOR_AD_HOC_WORKLOADS für eine Datenbank aktiviert ist, wird ein Stub des kompilierten Plans zwischengespeichert, wenn ein Batch zum ersten Mal kompiliert wird. Planstubs weisen im Vergleich zur Größe des vollständigen kompilierten Plans einen niedrigeren Speicherbedarf auf. Wenn ein Batch erneut kompiliert oder ausgeführt wird, wird der Stub des kompilierten Plans entfernt und durch einen vollständigen kompilierten Plan ersetzt.

XTP_PROCEDURE_EXECUTION_STATISTICS **=** { ON | **OFF** }

**Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Aktiviert oder deaktiviert die Sammlung von Ausführungsstatistiken auf Modulebene für nativ kompilierte T-SQL-Module in der aktuellen Datenbank. Der Standardwert ist OFF. Die Ausführungsstatistiken werden in [sys.dm_exec_procedure_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md) wiedergegeben.

Ausführungsstatistiken auf Modulebene für nativ kompilierte T-SQL-Module werden gesammelt, wenn diese Option auf „ON“ festgelegt ist, oder die Sammlung von Statistiken durch [sp_xtp_control_proc_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md) aktiviert ist.

XTP_QUERY_EXECUTION_STATISTICS **=** { ON | **OFF** }

**Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Aktiviert oder deaktiviert die Sammlung von Ausführungsstatistiken auf Anweisungsebene für nativ kompilierte T-SQL-Module in der aktuellen Datenbank. Der Standardwert ist OFF. Die Ausführungsstatistik wird in [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) und im [Abfragespeicher](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md) wiedergegeben.

Ausführungsstatistiken auf Anweisungsebene für nativ kompilierte T-SQL-Module werden gesammelt, wenn diese Option auf „ON“ festgelegt ist, oder die Sammlung von Statistiken durch [sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md) aktiviert ist.

Weitere Informationen über die Leistungsüberwachung von nativ kompilierten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Modulen finden Sie unter [Monitoring Performance of Natively Compiled Stored Procedures (Überwachen der Leistung von nativ kompilierten gespeicherten Prozeduren)](../../relational-databases/in-memory-oltp/monitoring-performance-of-natively-compiled-stored-procedures.md).

ROW_MODE_MEMORY_GRANT_FEEDBACK **=** { **ON** | OFF }

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Ermöglicht Ihnen das Aktivieren bzw. Deaktivieren von Feedback zur Speicherzuweisung im Zeilenmodus im Datenbankbereich. Dabei kann ein Datenbank-Kompatibilitätsgrad von mindestens 150 beibehalten werden. Das Feedback zur Speicherzuweisung im Zeilenmodus stellt einen Bestandteil der [intelligenten Abfrageverarbeitung](../../relational-databases/performance/intelligent-query-processing.md) dar, die in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] eingeführt wurde. Der Zeilenmodus wird in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] unterstützt.

> [!NOTE]
> Für Datenbank-Kompatibilitätsgrade von 140 oder weniger hat diese datenbankbezogene Konfiguration keine Auswirkungen.

BATCH_MODE_ON_ROWSTORE **=** { **ON** | OFF }

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Ermöglicht Ihnen das Aktivieren bzw. Deaktivieren des Batchmodus bei Rowstore im Datenbankbereich. Dabei kann ein Datenbank-Kompatibilitätsgrad von mindestens 150 beibehalten werden. Der Batchmodus bei Rowstore gehört zur Funktionsfamilie für die [intelligente Abfrageverarbeitung](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Für Datenbank-Kompatibilitätsgrade von 140 oder weniger hat diese datenbankbezogene Konfiguration keine Auswirkungen.

DEFERRED_COMPILATION_TV **=** { **ON** | OFF }

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Ermöglicht Ihnen das Aktivieren bzw. Deaktivieren der verzögerten Kompilierung von Tabellenvariablen im Datenbankbereich. Dabei kann ein Datenbank-Kompatibilitätsgrad von mindestens 150 beibehalten werden. Die verzögerte Kompilierung von Tabellenvariablen gehört zur Funktionsfamilie für die [intelligente Abfrageverarbeitung](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Für Datenbank-Kompatibilitätsgrade von 140 oder weniger hat diese datenbankbezogene Konfiguration keine Auswirkungen.

ACCELERATED_PLAN_FORCING **=** { **ON** | OFF }

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Aktiviert einen optimierten Mechanismus für das Erzwingen von Abfrageplänen, der sich auf alle Formen des Erzwingens von Plänen anwenden lässt, wie etwa [Query Store Force Plan](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#Regressed), [Automatische Optimierung](../../relational-databases/automatic-tuning/automatic-tuning.md#automatic-plan-correction) oder den Abfragehinweis [USE PLAN](../../t-sql/queries/hints-transact-sql-query.md#use-plan). Der Standardwert ist ON.

> [!NOTE]
> Es ist nicht empfehlenswert, das beschleunigte Erzwingen von Plänen zu deaktivieren.

GLOBAL_TEMPORARY_TABLE_AUTO_DROP **=** { **ON** | OFF }

**Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Gestattet das Festlegen der Funktion für automatisches Löschen von [globalen temporären Tabellen](../../t-sql/statements/create-table-transact-sql.md#temporary-tables). Der Standardwert ist ON, was bedeutet, dass die globalen temporären Tabellen automatisch gelöscht werden, wenn sie von keiner Sitzung verwendet werden. Wenn sie auf OFF festgelegt ist, müssen globale temporäre Tabellen explizit mithilfe einer DROP TABLE-Anweisung gelöscht werden, oder sie werden beim Serverneustart automatisch gelöscht.

- Für [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Singletons und Pools für elastische Datenbanken kann diese Option in den einzelnen Benutzerdatenbanken des SQL-Datenbankservers festgelegt werden.
- In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und Azure SQL Managed Instance wird diese Option in `TempDB` festgelegt, und die Einstellungen der individuellen Benutzerdatenbanken haben keine Auswirkungen.

<a name="lqp"></a>

LIGHTWEIGHT_QUERY_PROFILING **=** { **ON** | OFF }

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Ermöglicht das Aktivieren oder Deaktivieren der [einfachen Profilerstellungsinfrastruktur für Abfragen](../../relational-databases/performance/query-profiling-infrastructure.md) Die LWP-Abfrageinfrastruktur (Lightweight Profiling) stellt Abfrageleistungsdaten effizienter bereit als standardmäßige Profilerstellungsmechanismen. Sie ist standardmäßig aktiviert.

<a name="verbose-truncation"></a>

VERBOSE_TRUNCATION_WARNINGS **=** { **ON** | OFF }

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Ermöglicht Ihnen das Aktivieren oder Deaktivieren der neuen `String or binary data would be truncated`-Fehlermeldung. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] führt eine neue, spezifischere Fehlermeldung (2628) für dieses Szenario ein:

`String or binary data would be truncated in table '%.*ls', column '%.*ls'. Truncated value: '%.*ls'.`

Wenn ON bei einem Datenbank-Kompatibilitätsgrad unter 150 festgelegt ist, lösen Kürzungsfehler die neue Fehlermeldung 2628 aus, um mehr Kontext bereitzustellen und die Problembehandlung zu vereinfachen.

Wenn OFF bei einem Datenbank-Kompatibilitätsgrad unter 150 festgelegt ist, lösen Kürzungsfehler die alte Fehlermeldung 8152 aus.

Bei einem Datenbank-Kompatibilitätsgrad von 140 oder niedriger ist die Fehlermeldung 2628 weiterhin eine optionale Fehlermeldung, für die das [Ablaufverfolgungsflag 460](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) aktiviert sein muss. Diese datenbankweite Konfiguration hat keine Auswirkungen.

LAST_QUERY_PLAN_STATS **=** { ON | **OFF** }

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Ermöglicht Ihnen das Aktivieren oder Deaktivieren der Collection der Abfrageplanstatistiken (entspricht einem tatsächlichen Ausführungsplan) in [sys.dm_exec_query_plan_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md).

PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES

**Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Die Option `PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES` legt fest, wie lange (in Minuten) der fortsetzbare Index angehalten wird, bevor er automatisch von der Engine abgebrochen wird.

- Der Standardwert ist auf 1 Tag (1440 Minuten) festgelegt.
- Die minimale Dauer ist auf 1 Minute gesetzt.
- Die maximale Dauer beträgt 71582 Minuten.
- Wenn der Wert auf 0 festgelegt ist, wird ein angehaltener Vorgang nie automatisch abgebrochen.

Der aktuelle Wert für diese Option wird in [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) angezeigt.

ISOLATE_SECURITY_POLICY_CARDINALITY **=** { ON | **OFF**}

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Ermöglicht es Ihnen, zu steuern, ob ein Prädikat für die [Sicherheit auf Zeilenebene](../../relational-databases/security/row-level-security.md) (Row-Level Security, RLS) die Kardinalität des Ausführungsplans für die gesamte Benutzerabfrage beeinflusst. Wenn für ISOLATE_SECURITY_POLICY_CARDINALITY ON festgelegt ist, beeinflussen RLS-Prädikate die Kardinalität von Ausführungsplänen nicht. Angenommen, es gibt eine Tabelle mit 1 Million Zeilen und ein RLS-Prädikat, das das Ergebnis für einen bestimmten Benutzer, der die Abfrage durchführt, auf 10 Zeilen beschränkt. Wenn für diese datenbankweite Konfiguration OFF festgelegt ist, schätzt dieses Prädikat die Kardinalität auf 10. Wenn für diese datenbankweite Konfiguration ON festgelegt ist, schätzt die Abfrageoptimierung die Anzahl der Zeilen auf 1 Million. Für die meisten Arbeitsauslastungen wird empfohlen, den Standardwert zu verwenden.

DW_COMPATIBILITY_LEVEL **=** {**AUTO** | 10 | 20 }

**Gilt für**: nur [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)]

Legt für [!INCLUDE[tsql](../../includes/tsql-md.md)] und Verhalten bei der Abfrageverarbeitung fest, dass sie mit der angegebenen Version der Datenbank-Engine kompatibel sein müssen. Sobald es festgelegt ist, werden beim Ausführen von Abfragen für die entsprechende Datenbank nur die kompatiblen Features ausgeführt.  Beim Erstellen einer Datenbank wird für den Kompatibilitätsgrad standardmäßig AUTO festgelegt. Der Kompatibilitätsgrad wird auch nach dem Anhalten/Fortsetzen einer Datenbank sowie Sicherungs-/Wiederherstellungsvorgängen beibehalten. 

|Kompatibilitätsgrad    |   Kommentare|  
|-----------------------|--------------|
|**AUTO**| Standard.  Der Wert wird automatisch von der Synapse Analytics-Engine aktualisiert.  Der aktuelle Wert ist 20.|
|**10**| Bei diesem Wert wird das Transact-SQL- und Abfrageverarbeitungsverhalten ausgeführt, bevor Kompatibilitätsgradunterstützung eingeführt wird.|
|**20**| Hierbei handelt es sich um den ersten Kompatibilitätsgrad mit geschlossenem Transact-SQL- und Abfrageverarbeitungsverhalten. |

EXEC_QUERY_STATS_FOR_SCALAR_FUNCTIONS **=** { **ON** | OFF }

**Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Hiermit können Sie steuern, ob Ausführungsstatistiken für benutzerdefinierte Skalarfunktionen (UDF) in der Systemsicht [sys.dm_exec_function_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-function-stats-transact-sql.md) angezeigt werden. Bei einigen intensiven Workloads, bei denen es sich um skalare, UDF-lastige Funktionen handelt, kann das Sammeln von Funktionsausführungsstatistiken zu einem spürbar erhöhten Leistungsaufwand führen. Dies kann vermieden werden, indem die Konfiguration auf Datenbankebene, `EXEC_QUERY_STATS_FOR_SCALAR_FUNCTIONS`, auf `OFF` festgelegt wird.

ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY **=** { ON | **OFF** }

**GILT FÜR**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (das Feature befindet sich in der öffentlichen Vorschau)

Wenn das asynchrone Statistikupdate aktiviert ist, bewirkt das Aktivieren dieser Konfiguration, dass der Hintergrundprozess zum Aktualisieren von Statistiken in einer Warteschlange mit niedriger Priorität auf eine Sch-M-Sperre wartet, um zu verhindern, dass andere Sitzungen in Szenarios mit hoher Parallelität blockiert werden. Weitere Informationen finden Sie unter [AUTO_UPDATE_STATISTICS_ASYNC](../../relational-databases/statistics/statistics.md#auto_update_statistics_async).

## <a name="permissions"></a><a name="Permissions"></a> Berechtigungen

`ALTER ANY DATABASE SCOPED CONFIGURATION` ist auf der Datenbank erforderlich. Diese Berechtigung kann von einem Benutzer mit `CONTROL`-Berechtigung für eine Datenbank erteilt werden.

## <a name="general-remarks"></a>Allgemeine Hinweise

Während Sie sekundäre Datenbanken so konfigurieren können, dass sie verschiedene bereichsbezogene Konfigurationseinstellungen von ihrer primären Datenbank aufweisen, wird für alle sekundären Datenbanken die gleiche Konfiguration verwendet. Für einzelne sekundäre Datenbanken können keine unterschiedlichen Einstellungen konfiguriert werden.

Durch die Ausführung dieser Anweisung wird der Prozedurcache in der aktuellen Datenbank geleert. Dies bedeutet, dass alle Abfragen erneut kompiliert werden müssen.

Bei Abfragen für dreiteilige Namen werden die Einstellungen für die aktuelle Datenbankverbindung berücksichtigt. Eine Ausnahme stellen SQL-Module dar (z. B. Prozeduren, Funktionen und Trigger), die in einem anderen Datenbankkontext kompiliert werden, weshalb die Optionen der Datenbank verwendet werden, in der sie sich befinden. Gleichermaßen wird beim asynchronen Aktualisieren von Statistiken die Einstellung „ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY“ für die Datenbank berücksichtigt, in der sich die Statistiken befinden.

Das Ereignis `ALTER_DATABASE_SCOPED_CONFIGURATION` wird als DLL-Ereignis hinzugefügt, mit dem ein DDL-Trigger ausgelöst werden kann, und ist ein untergeordnetes Ereignis der Triggergruppe `ALTER_DATABASE_EVENTS`.

Datenbankweit gültige Konfigurationseinstellungen werden zusammen mit der Datenbank übertragen, was bedeutet, dass die vorhandenen Konfigurationseinstellungen bei der Wiederherstellung oder dem Anfügen einer bestimmten Datenbank erhalten bleiben.

Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] haben sich einige Optionsnamen geändert:      
-  `DISABLE_INTERLEAVED_EXECUTION_TVF` wurde in `INTERLEAVED_EXECUTION_TVF` geändert
-  `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK` wurde in `BATCH_MODE_MEMORY_GRANT_FEEDBACK` geändert
-  `DISABLE_BATCH_MODE_ADAPTIVE_JOINS` wurde in `BATCH_MODE_ADAPTIVE_JOINS` geändert

## <a name="limitations-and-restrictions"></a>Einschränkungen

### <a name="maxdop"></a>MAXDOP

Die differenzierten Einstellungen können die globalen Einstellungen überschreiben. Zudem kann die Ressourcenkontrolle alle anderen MAXDOP-Einstellungen begrenzen. Die Logik für die MAXDOP-Einstellung lautet wie folgt:

- Der Abfragehinweis überschreibt die Einstellung `sp_configure` und die datenbankweit gültige Konfiguration. Wenn die Ressourcengruppe MAXDOP für die Arbeitsauslastungsgruppe festgelegt ist:

  - Wenn der Abfragehinweis auf „0 (null)“ festgelegt ist, wird er von der Einstellung der Ressourcenkontrolle überschrieben.

  - Wenn der Abfragehinweis nicht auf „0 (null)“ festgelegt ist, wird er von der Einstellung der Ressourcenkontrolle begrenzt.

- Die datenbankweit gültige Einstellung überschreibt die Einstellung `sp_configure` (wenn sie nicht auf „0 (null)“ festgelegt ist), sofern kein Abfragehinweis vorhanden ist und sie nicht von der Einstellung der Ressourcenkontrolle begrenzt wird.

- Die Einstellung `sp_configure` wird von der Einstellung der Ressourcenkontrolle überschrieben.

### <a name="query_optimizer_hotfixes"></a>QUERY_OPTIMIZER_HOTFIXES

Wenn der Hinweis `QUERYTRACEON` zur Aktivierung des Standardabfrageoptimierers von SQL Server 7.0 bis [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] oder der Hotfixes für den Abfrageoptimierer verwendet wird, bestünde zwischen dem Abfragehinweis und der datenbankweit gültigen Konfigurationseinstellung eine OR-Bedingung. Das heißt, wenn eines davon aktiviert ist, werden die datenbankweiten Konfigurationen angewendet.

### <a name="geo-dr"></a>Georeplikation von Datenbanken

Lesbare sekundäre Datenbanken (Always On-Verfügbarkeitsgruppen, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und georeplizierte [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]-Datenbanken) verwenden den sekundären Wert durch Überprüfung des Datenbankstatus. Obwohl es bei einer erneuten Kompilierung nicht zu einem Failover kommt und die neue primäre Datenbank eigentlich Abfragen aufweist, die Einstellungen für die sekundären Datenbanken verwenden, ist die Idee, dass die Einstellungen zwischen der primären und der sekundären Datenbank nur bei unterschiedlicher Arbeitsauslastung variieren. Daher verwenden die zwischengespeicherten Abfragen die optimalen Einstellungen, während neue Abfragen die neuen, für sie geeigneten Einstellungen auswählen.

### <a name="dacfx"></a>DacFX

Da `ALTER DATABASE SCOPED CONFIGURATION` ein neues Feature in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)]) ist, das sich auf das Datenbankschema auswirkt, können Exporte des Schemas (mit oder ohne Daten) nicht in ältere Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (z. B. [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] oder [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) importiert werden. Ein Export in ein [DACPAC](../../relational-databases/data-tier-applications/data-tier-applications.md) oder ein [BACPAC](../../relational-databases/data-tier-applications/data-tier-applications.md#bacpac) aus einer Datenbank von [!INCLUDE[ssSDS](../../includes/sssds-md.md)] oder [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)], in der dieses neue Feature verwendet wird, könnte nicht in einen Server der Vorgängerversion importiert werden.

### <a name="elevate_online"></a>ELEVATE_ONLINE

Diese Option gilt nur für DDL-Anweisungen, die `WITH (ONLINE = <syntax>)` unterstützen. XML-Indizes sind nicht betroffen.

### <a name="elevate_resumable"></a>ELEVATE_RESUMABLE

Diese Option gilt nur für DDL-Anweisungen, die `WITH (RESUMABLE = <syntax>)` unterstützen. XML-Indizes sind nicht betroffen.

## <a name="metadata"></a>Metadaten

Die Systemansicht [sys.database_scoped_configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) enthält Informationen zu bereichsbezogenen Konfigurationen innerhalb einer Datenbank. Datenbankbezogene Konfigurationsoptionen werden nur in sys.database_scoped_configurations angezeigt, da sie serverweite Standardeinstellungen überschreiben. In der Systemansicht [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) werden nur serverweite Einstellungen angezeigt.

## <a name="examples"></a>Beispiele

Folgende Beispiele veranschaulichen die Verwendung von ALTER DATABASE SCOPED CONFIGURATION

### <a name="a-grant-permission"></a>A. Berechtigung erteilen

In diesem Beispiel wird dem Benutzer Joe die Berechtigung erteilt, die zum Ausführen von ALTER DATABASE SCOPED CONFIGURATION erforderlich ist.

```sql
GRANT ALTER ANY DATABASE SCOPED CONFIGURATION to [Joe] ;
```

### <a name="b-set-maxdop"></a>B. Festlegen von MAXDOP

In diesem Beispiel wird in einem Georeplikationsszenario bei einer primären Datenbank MAXDOP = 1 und bei einer sekundären Datenbank MAXDOP = 4 festgelegt.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET MAXDOP = 1 ;
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP = 4 ;
```

In diesem Beispiel wird in einem Georeplikationsszenario der Wert für MAXDOP bei einer sekundären Datenbank auf den gleichen Wert wie für die zugehörige primäre Datenbank festgelegt.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP = PRIMARY ;
```

### <a name="c-set-legacy_cardinality_estimation"></a>C. Festlegen von LEGACY_CARDINALITY_ESTIMATION

In diesem Beispiel wird LEGACY_CARDINALITY_ESTIMATION in einem Georeplikationsszenario bei einer sekundären Datenbank auf ON festgelegt.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET LEGACY_CARDINALITY_ESTIMATION = ON ;
```

In diesem Beispiel wird der Wert für LEGACY_CARDINALITY_ESTIMATION in einem Georeplikationsszenario bei einer sekundären Datenbank auf den gleichen Wert wie für die zugehörige primäre Datenbank festgelegt.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET LEGACY_CARDINALITY_ESTIMATION = PRIMARY ;
```

### <a name="d-set-parameter_sniffing"></a>D: Festlegen von PARAMETER_SNIFFING

In diesem Beispiel wird PARAMETER_SNIFFING in einem Georeplikationsszenario bei einer primären Datenbank auf OFF festgelegt.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET PARAMETER_SNIFFING = OFF ;
```

In diesem Beispiel wird PARAMETER_SNIFFING für eine sekundäre Datenbank in einem Georeplikationsszenario auf OFF festgelegt.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET PARAMETER_SNIFFING = OFF ;
```

In diesem Beispiel wird der Wert für PARAMETER_SNIFFING in einem Georeplikationsszenario bei einer sekundären Datenbank auf den gleichen Wert festgelegt wie für die zugehörige primäre Datenbank.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET PARAMETER_SNIFFING = PRIMARY ;
```

### <a name="e-set-query_optimizer_hotfixes"></a>E. Festlegen von QUERY_OPTIMIZER_HOTFIXES

Legt QUERY_OPTIMIZER_HOTFIXES in einem Georeplikationsszenario bei einer primären Datenbank auf ON fest.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET QUERY_OPTIMIZER_HOTFIXES = ON ;
```

### <a name="f-clear-procedure-cache"></a>F. Leeren des Prozedurcache

In diesem Beispiel wird der Prozedurcache geleert (dies ist nur bei einer primären Datenbank möglich).

```sql
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
```

### <a name="g-set-identity_cache"></a>G. Festlegen von IDENTITY_CACHE

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

In diesem Beispiel wird der Identitätscache deaktiviert.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET IDENTITY_CACHE = OFF ;
```

### <a name="h-set-optimize_for_ad_hoc_workloads"></a>H. Festlegen von OPTIMIZE_FOR_AD_HOC_WORKLOADS

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

In diesem Beispiel wird ein Stub des kompilierten Plans aktiviert, der bei der erstmaligen Kompilierung eines Batches im Cache gespeichert werden soll.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET OPTIMIZE_FOR_AD_HOC_WORKLOADS = ON;
```

### <a name="i-set-elevate_online"></a>I. Festlegen von ELEVATE_ONLINE

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

In diesem Beispiel wird ELEVATE_ONLINE auf FAIL_UNSUPPORTED festgelegt.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ELEVATE_ONLINE = FAIL_UNSUPPORTED ;
```

### <a name="j-set-elevate_resumable"></a>J. Festlegen von ELEVATE_RESUMABLE

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

In diesem Beispiel wird ELEVATE_RESUMABLE auf WHEN_SUPPORTED festgelegt.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ELEVATE_RESUMABLE = WHEN_SUPPORTED ;
```

### <a name="k-clear-a-query-plan-from-the-plan-cache"></a>K. Löschen eines Abfrageplans aus dem Plancache

**GILT FÜR**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

In diesem Beispiel wird ein bestimmter Plan aus dem Prozedurcache gelöscht.

```sql
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE 0x06000500F443610F003B7CD12C02000001000000000000000000000000000000000000000000000000000000;
```

### <a name="l-set-paused-duration"></a>L. Festlegen der Pausendauer

**Gilt für**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

In diesem Beispiel wird die Pausendauer des fortsetzbaren Index auf 60 Minuten festgelegt.

```sql
ALTER DATABASE SCOPED CONFIGURATION
SET PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES = 60
```

## <a name="additional-resources"></a>Weitere Ressourcen

### <a name="maxdop-resources"></a>Ressourcen von MAXDOP

- [Grad der Parallelität](../../relational-databases/query-processing-architecture-guide.md#DOP)
- [Recommendations and guidelines for the "max degree of parallelism" configuration option in SQL Server (Empfehlungen und Guidelines für die Konfigurationsoption „Max. Grad an Parallelität“ in SQL Server)](https://support.microsoft.com/kb/2806535)

### <a name="legacy_cardinality_estimation-resources"></a>Ressourcen von LEGACY_CARDINALITY_ESTIMATION

- [Kardinalitätsschätzung (SQL Server)](../../relational-databases/performance/cardinality-estimation-sql-server.md)
- [Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator](/previous-versions/dn673537(v=msdn.10))

### <a name="parameter_sniffing-resources"></a>Ressourcen von PARAMETER_SNIFFING

- [Parameterermittlung](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing)
- [„I smell a parameter!“](/archive/blogs/queryoptteam/i-smell-a-parameter)

### <a name="query_optimizer_hotfixes-resources"></a>Ressourcen von QUERY_OPTIMIZER_HOTFIXES

- [Ablaufverfolgungsflags](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)
- [SQL Server query optimizer hotfix trace flag 4199 servicing model](https://support.microsoft.com/kb/974006) (Wartungsmodell für SQL Server-Hotfix für Abfrageoptimierer – Ablaufverfolgungsflag 4199)

### <a name="elevate_online-resources"></a>ELEVATE_ONLINE-Ressourcen

[Richtlinien für Onlineindexvorgänge](../../relational-databases/indexes/guidelines-for-online-index-operations.md)

### <a name="elevate_resumable-resources"></a>ELEVATE_RESUMABLE-Ressourcen

[Richtlinien für Onlineindexvorgänge](../../relational-databases/indexes/guidelines-for-online-index-operations.md)

## <a name="more-information"></a>Weitere Informationen   
 [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md)      
 [Empfehlung und Richtlinien für die Konfigurationsoption „Max. Grad an Parallelität“ in SQL Server](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines)      
 [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)    
 [Datenbank- und Dateikatalogsichten](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)    
 [Serverkonfigurationsoptionen](../../database-engine/configure-windows/server-configuration-options-sql-server.md)    
 [Funktionsweise von Onlineindexvorgängen](../../relational-databases/indexes/how-online-index-operations-work.md)    
 [Ausführen von Onlineindexvorgängen](../../relational-databases/indexes/perform-index-operations-online.md)    
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)    
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)
