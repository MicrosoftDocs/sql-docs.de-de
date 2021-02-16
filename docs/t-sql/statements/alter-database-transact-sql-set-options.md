---
title: ALTER DATABASE SET-Optionen (Transact-SQL) | Microsoft-Dokumentation
description: Informationen zum Festlegen von Datenbankoptionen wie automatische Optimierung, Verschlüsselung und Abfragespeicher in SQL Server und Azure SQL-Datenbank
ms.custom: ''
ms.date: 09/04/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- online database state [SQL Server]
- database options [SQL Server]
- emergency database state [SQL Server]
- databases [SQL Server], options
- read-only databases
- recovery models [SQL Server], switching
- ALTER DATABASE statement, SET options
- offline database state [SQL Server]
- snapshot isolation framework option
- checksums [SQL Server]
- Automatic tuning
- " Data Retention Policy"
- query plan regression correction
- auto_create_statistics
- auto_update_statistics
- Query Store options
ms.assetid: f76fbd84-df59-4404-806b-8ecb4497c9cc
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current
ms.openlocfilehash: 37ea2f4896d68f4ae5a47b14025d54a344febdc0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016315"
---
# <a name="alter-database-set-options-transact-sql"></a>ALTER DATABASE SET-Optionen (Transact-SQL)

Legt Datenbankoptionen in Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] fest. Informationen zu anderen ALTER DATABASE-Optionen finden Sie unter [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md).

Klicken Sie auf eine der folgenden Registerkarten, um Syntax, Argumente, Hinweise, Berechtigungen und Beispiele für eine bestimmte SQL-Version anzuzeigen, mit der Sie arbeiten.

Weitere Informationen zu Syntaxkonventionen finden Sie unter [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

> [!NOTE]
> Das Festlegen einiger Optionen mit ALTER DATABASE erfordert möglicherweise exklusiven Datenbankzugriff. Wenn die ALTER DATABASE-Anweisung nicht rechtzeitig vollständig ausgeführt wird, überprüfen Sie, ob die ALTER DATABASE-Sitzung von anderen Sitzungen in der Datenbank blockiert wird.

## <a name="select-a-product"></a>Auswählen eines Produkts

Klicken Sie in der folgenden Zeile auf den Namen des Produkts, das Sie am meisten interessiert. Dadurch werden Ihnen auf dieser Webseite unterschiedliche Inhalte angezeigt, die zu dem von Ihnen ausgewählten Produkt passen.

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL-Datenbank](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

Datenbankspiegelung, [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] und Kompatibilitätsgrade sind zwar `SET`-Optionen, werden aufgrund ihres Umfangs jedoch in separaten Artikeln beschrieben. Weitere Informationen finden Sie unter [ALTER DATABASE-Datenbankspiegelung](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md), [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md) und [ALTER DATABASE-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

Datenbankweit gültige Konfigurationen werden zum Festlegen mehrerer Datenbankkonfigurationen auf der Ebene einzelner Datenbanken verwendet. Weitere Informationen finden Sie unter [ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).

> [!NOTE]
> Viele DATABASE SET-Optionen können mithilfe von [SET-Anweisungen](../../t-sql/statements/set-statements-transact-sql.md) für die aktuelle Sitzung konfiguriert werden; häufig werden sie bei der Verbindung von Anwendungen konfiguriert. Die **ALTER DATABASE SET**-Werte werden durch SET-Optionen auf Sitzungsebene überschrieben. Die in den folgenden Abschnitten beschriebenen Datenbankoptionen entsprechen Werten, die Sie für Sitzungen festlegen können, von denen explizit keine weiteren Werte für SET-Optionen bereitgestellt werden.

## <a name="syntax"></a>Syntax

```syntaxsql
ALTER DATABASE { database_name | CURRENT }
SET
{
    <option_spec> [ ,...n ] [ WITH <termination> ]
}

<option_spec> ::=
{
    <accelerated_database_recovery>
  | <auto_option>
  | <automatic_tuning_option>
  | <change_tracking_option>
  | <containment_option>
  | <cursor_option>
  | <database_mirroring_option>
  | <date_correlation_optimization_option>
  | <db_encryption_option>
  | <db_state_option>
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <external_access_option>
  | FILESTREAM ( <FILESTREAM_option> )
  | <HADR_options>
  | <mixed_page_allocation_option>
  | <parameterization_option>
  | <query_store_options>
  | <recovery_option>
  | <remote_data_archive_option>
  | <service_broker_option>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
  | <data_retention_policy>
}
;

<accelerated_database_recovery> ::=
{
    ACCELERATED_DATABASE_RECOVERY = { ON | OFF }
     [ ( PERSISTENT_VERSION_STORE_FILEGROUP = { filegroup name } ) ];
}

<auto_option> ::=
{
    AUTO_CLOSE { ON | OFF }
  | AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
   {
       = OFF
     | = ON [ ( <change_tracking_option_list > [,...n] ) ]
     | ( <change_tracking_option_list> [,...n] )
   }
}

<change_tracking_option_list> ::=
{
   AUTO_CLEANUP = { ON | OFF }
 | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
}

<containment_option> ::=
   CONTAINMENT = { NONE | PARTIAL }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
  | CURSOR_DEFAULT { LOCAL | GLOBAL }
}

<database_mirroring_option>
  ALTER DATABASE Database Mirroring

<date_correlation_optimization_option> ::=
    DATE_CORRELATION_OPTIMIZATION { ON | OFF }
  
<db_encryption_option> ::=
    ENCRYPTION { ON | OFF | SUSPEND | RESUME }

<db_state_option> ::=
    { ONLINE | OFFLINE | EMERGENCY }

<db_update_option> ::=
    { READ_ONLY | READ_WRITE }

<db_user_access_option> ::=
    { SINGLE_USER | RESTRICTED_USER | MULTI_USER }

<delayed_durability_option> ::=
    DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<external_access_option> ::=
{
    DB_CHAINING { ON | OFF }
  | TRUSTWORTHY { ON | OFF }
  | DEFAULT_FULLTEXT_LANGUAGE = { <lcid> | <language name> | <language alias> }
  | DEFAULT_LANGUAGE = { <lcid> | <language name> | <language alias> }
  | NESTED_TRIGGERS = { OFF | ON }
  | TRANSFORM_NOISE_WORDS = { OFF | ON }
  | TWO_DIGIT_YEAR_CUTOFF = { 1753, ..., 2049, ..., 9999 }
}

<FILESTREAM_option> ::=
{
    NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL
  | DIRECTORY_NAME = <directory_name>
}

<HADR_options> ::=
    ALTER DATABASE SET HADR

<mixed_page_allocation_option> ::=
    MIXED_PAGE_ALLOCATION { OFF | ON }

<parameterization_option> ::=
    PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
    QUERY_STORE
    {
          = OFF [ ( FORCED ) ] 
        | = ON [ ( <query_store_option_list> [,...n] ) ]
        | ( < query_store_option_list> [,...n] )
        | CLEAR [ ALL ]
    }
}

<query_store_option_list> ::=
{
      OPERATION_MODE = { READ_WRITE | READ_ONLY }
    | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
    | DATA_FLUSH_INTERVAL_SECONDS = number
    | MAX_STORAGE_SIZE_MB = number
    | INTERVAL_LENGTH_MINUTES = number
    | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
    | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
    | MAX_PLANS_PER_QUERY = number
    | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
    | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
      STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<recovery_option> ::=
{
    RECOVERY { FULL | BULK_LOGGED | SIMPLE }
  | TORN_PAGE_DETECTION { ON | OFF }
  | PAGE_VERIFY { CHECKSUM | TORN_PAGE_DETECTION | NONE }
}

<remote_data_archive_option> ::=
{
    REMOTE_DATA_ARCHIVE =
    {
        ON ( SERVER = <server_name> ,
             { 
                  CREDENTIAL = <db_scoped_credential_name>
                  | FEDERATED_SERVICE_ACCOUNT = ON | OFF
             }
        )
        | OFF
    }
}

<service_broker_option> ::=
{
    ENABLE_BROKER
  | DISABLE_BROKER
  | NEW_BROKER
  | ERROR_BROKER_CONVERSATIONS
  | HONOR_BROKER_PRIORITY { ON | OFF}
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT { ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = { ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<target_recovery_time_option> ::=
    TARGET_RECOVERY_TIME = target_recovery_time { SECONDS | MINUTES }

<termination>::=
{
    ROLLBACK AFTER number [ SECONDS ]
  | ROLLBACK IMMEDIATE
  | NO_WAIT
}

<temporal_history_retention> ::=
    TEMPORAL_HISTORY_RETENTION { ON | OFF }

<data_retention_policy> ::=
    DATA_RETENTION { ON | OFF }

```

## <a name="arguments"></a>Argumente

*database_name*     
Der Name der Datenbank, die geändert werden soll.

CURRENT     
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Führt die Aktion in der aktuellen Datenbank aus. `CURRENT` wird nicht in allen Kontexten für alle Optionen unterstützt. Wenn `CURRENT` einen Fehler verursacht, geben Sie den Datenbanknamen an.

**\<accelerated_database_recovery> ::=**      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Aktiviert die [schnellere Datenbankwiederherstellung](../../relational-databases/accelerated-database-recovery-management.md) (Accelerated Database Recovery, ADR) für jede Datenbank. Die Standardeinstellung für ADR in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ist „OFF“. Mit dieser Syntax verfügen Sie über die Option, eine bestimmte Dateigruppe für die Daten des persistenten Versionsspeichers (PVS) festzulegen. Wenn keine Dateigruppe angegeben wird, wird der PVS in der PRIMARY-Dateigruppe gespeichert. Beispiele und weitere Informationen finden Sie unter [Schnellere Datenbankwiederherstellung](../../relational-databases/accelerated-database-recovery-management.md).

**\<auto_option> ::=**      
Steuert automatische Optionen.

<a name="auto_close"></a> AUTO_CLOSE { ON | **OFF** }     
EIN     
Die Datenbank wird ordnungsgemäß heruntergefahren, und ihre Ressourcen werden freigegeben, nachdem der letzte Benutzer die Anwendung beendet hat.

Die Datenbank wird automatisch wieder geöffnet, wenn ein Benutzer versucht, die Datenbank erneut zu verwenden. Dieses Verhalten (erneutes Öffnen) tritt auf, wenn ein Benutzer eine `USE database_name`-Anweisung ausgibt. Die Datenbank kann mit auf ON festgelegtem AUTO_CLOSE ordnungsgemäß heruntergefahren werden. Ist dies der Fall, wird die Datenbank erst dann wieder geöffnet, wenn ein Benutzer versucht, die Datenbank beim nächsten Neustart von [!INCLUDE[ssDE](../../includes/ssde-md.md)] zu verwenden.

OFF     
Die Datenbank bleibt nach dem Beenden der Verwendung durch den letzten Benutzer geöffnet.

Die Option AUTO_CLOSE ist sehr nützlich für Desktopdatenbanken, da mit ihrer Hilfe Datenbankdateien wie reguläre Dateien verwaltet werden können. Sie können verschoben, zur Sicherung kopiert oder sogar per E-Mail an andere Benutzer gesendet werden. AUTO_CLOSE ist ein asynchroner Prozess. Das wiederholte Öffnen und Schließen der Datenbank beeinträchtigt nicht die Leistung.

> [!NOTE]
> Die AUTO_CLOSE-Option ist nicht in einer eigenständigen Datenbank oder [!INCLUDE[ssSDS](../../includes/sssds-md.md)] verfügbar.
> Sie können den Status dieser Option mithilfe der Spalte `is_auto_close_on` in der Katalogsicht [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) oder der `IsAutoClose`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion ermitteln.
>
> Ist AUTO_CLOSE auf ON festgelegt, geben einige Spalten in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht sowie die [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion den Wert NULL zurück, da die Datenbank nicht für den Abruf der Daten verfügbar ist. Führen Sie eine USE-Anweisung zum Öffnen der Datenbank aus, um dieses Problem zu beheben.
>
> Für die Datenbankspiegelung muss AUTO_CLOSE auf OFF festgelegt sein.

Wenn die Datenbank auf `AUTOCLOSE = ON` festgelegt ist, wird mit einem Vorgang, mit dem das automatische Beenden der Datenbank initiiert wird, der Plancache für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gelöscht. Durch das Löschen des Plancaches wird eine Neukompilierung aller nachfolgenden Ausführungspläne verursacht, und möglicherweise entsteht plötzlich eine temporäre Verringerung der Abfrageleistung. Ab [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 enthält das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll für jeden geleerten Cachespeicher im Plancache die folgende Meldung: `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`. Diese Meldung wird alle fünf Minuten protokolliert, solange der Cache innerhalb dieses Zeitintervalls geleert wird.

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { **ON** | OFF }     
EIN     
Der Abfrageoptimierer erstellt nach Bedarf Statistiken für einzelne Spalten in Abfrageprädikaten, um Abfragepläne und die Abfrageleistung zu verbessern. Diese Statistiken für einzelne Spalten werden erstellt, wenn der Abfrageoptimierer Abfragen kompiliert. Die Statistiken für einzelne Spalten werden nur für Spalten erstellt, die noch nicht der ersten Spalte eines vorhandenen Statistikobjekts entsprechen.

Die Standardeinstellung ist ON. Für die meisten Datenbanken empfiehlt sich die Verwendung der Standardeinstellung.

OFF     
Der Abfrageoptimierer erstellt beim Kompilieren von Abfragen keine Statistiken für einzelne Spalten in Abfrageprädikaten. Das Festlegen dieser Option auf OFF kann zu suboptimalen Abfrageplänen und einer beeinträchtigten Abfrageleistung führen.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_auto_create_stats_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAutoCreateStatistics`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

Weitere Informationen finden Sie im Abschnitt „Verwenden der datenbankweiten Statistikoptionen“ unter [Statistiken](../../relational-databases/statistics/statistics.md).

INCREMENTAL = ON | **OFF**     
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Legen Sie AUTO_CREATE_STATISTICS auf ON und INCREMENTAL auf ON fest. Dadurch werden automatisch erstellte Statistiken als inkrementell festgelegt, wenn inkrementelle Statistiken unterstützt werden. Der Standardwert ist OFF. Weitere Informationen finden Sie unter [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).

<a name="auto_shrink"></a> AUTO_SHRINK { ON | **OFF** }     
EIN     
Die Datenbankdateien sind Kandidaten für das periodische Verkleinern.

Sowohl Daten- als auch Protokolldateien können automatisch verkleinert werden. AUTO_SHRINK reduziert die Größe des Transaktionsprotokolls nur, wenn Sie die Datenbank auf das SIMPLE-Wiederherstellungsmodell festlegen oder das Protokoll sichern. Wenn Sie AUTO_SHRINK auf OFF festlegen, werden die Datenbankdateien während der periodisch ausgeführten Überprüfung auf nicht verwendeten Speicherplatz nicht automatisch verkleinert.

Die Option AUTO_SHRINK verkleinert eine Datei, wenn mehr als 25 Prozent dieser aus nicht verwendetem Speicherplatz bestehen. Die Datei wird auf eine von zwei Größen verkleinert (je nachdem, welche größer ist):

- die Größe, bei der 25 Prozent der Datei aus nicht verwendetem Speicherplatz bestehen
- die Größe der Datei, als sie erstellt wurde

Eine schreibgeschützte Datenbank kann nicht verkleinert werden.

OFF     
Die Datenbankdateien werden bei periodischen Prüfungen auf nicht verwendeten Speicherplatz nicht automatisch verkleinert.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_auto_shrink_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAutoShrink`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

> [!NOTE]
> Die AUTO_SHRINK-Option ist in einer eigenständigen Datenbank nicht verfügbar.

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { **ON** | OFF }     
EIN     
Gibt an, dass der Abfrageoptimierer Statistiken aktualisiert, wenn sie von einer Abfrage verwendet werden und veraltet sein könnten. Statistiken sind veraltet, wenn die Datenverteilung in der Tabelle oder indizierten Sicht durch die Vorgänge INSERT, UPDATE, DELETE oder MERGE geändert wurde. Der Abfrageoptimierer stellt fest, wann Statistiken veraltet sein könnten, indem er die Anzahl von Datenänderungen seit der letzten Statistikaktualisierung ermittelt und sie mit einem Schwellenwert vergleicht. Der Schwellenwert basiert auf der Anzahl von Zeilen in der Tabelle oder indizierten Sicht.

Bevor der Abfrageoptimierer eine Abfrage kompiliert und einen zwischengespeicherten Abfrageplan ausführt, sucht er nach veralteten Statistiken. Der Abfrageoptimierer ermittelt anhand der Spalten, Tabellen und indizierten Sichten im Abfrageprädikat, welche Statistiken veraltet sein könnten. Der Abfrageoptimierer ermittelt diese Informationen, bevor er eine Abfrage kompiliert. Vor dem Ausführen eines zwischengespeicherten Abfrageplans stellt die [!INCLUDE[ssDE](../../includes/ssde-md.md)] sicher, dass der Abfrageplan auf aktuelle Statistiken verweist.

Die AUTO_UPDATE_STATISTICS-Option gilt für Statistikobjekte, die für Indizes, einzelne Spalten in Abfrageprädikaten und mit der CREATE STATISTICS-Anweisung generierte Statistiken erstellt wurden. Diese Option gilt auch für gefilterte Statistiken.

Der Standardwert ist ON. Für die meisten Datenbanken empfiehlt sich die Verwendung der Standardeinstellung.

Verwenden Sie die AUTO_UPDATE_STATISTICS_ASYNC-Option, um anzugeben, ob die Statistiken synchron oder asynchron aktualisiert werden.

OFF     
Gibt an, dass der Abfrageoptimierer Statistiken nicht aktualisiert, wenn sie von einer Abfrage verwendet werden. Der Abfrageoptimierer aktualisiert Statistiken auch nicht, wenn sie veraltet sein könnten. Das Festlegen dieser Option auf OFF kann zu suboptimalen Abfrageplänen und einer beeinträchtigten Abfrageleistung führen.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_auto_update_stats_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAutoUpdateStatistics`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

Weitere Informationen finden Sie im Abschnitt „Verwenden der datenbankweiten Statistikoptionen“ unter [Statistiken](../../relational-databases/statistics/statistics.md).

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | **OFF** }     
EIN     
Gibt an, dass Statistikupdates für die AUTO_UPDATE_STATISTICS-Option asynchron sind. Der Abfrageoptimierer wartet nicht, bis Statistikaktualisierungen abgeschlossen sind, bevor Abfragen kompiliert werden.

Das Festlegen dieser Option auf ON hat nur dann Auswirkungen, wenn AUTO_UPDATE_STATISTICS auf ON festgelegt ist.

Die Standardeinstellung der Option AUTO_UPDATE_STATISTICS_ASYNC ist „OFF“, und der Abfrageoptimierer aktualisiert Statistiken synchron.

OFF     
Gibt an, dass Statistikupdates für die AUTO_UPDATE_STATISTICS-Option synchron sind. Der Abfrageoptimierer wartet, bis Statistikupdates abgeschlossen sind, bevor Abfragen kompiliert werden.

> [!NOTE]
> Das Festlegen dieser Option auf OFF hat nur dann Auswirkungen, wenn AUTO_UPDATE_STATISTICS auf ON festgelegt ist.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_auto_update_stats_async_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen.

Weitere Informationen dazu, wann synchrone bzw. asynchrone Statistikupdates verwendet werden sollten, finden Sie im Abschnitt „Statistikoptionen“ unter [Statistiken](../../relational-databases/statistics/statistics.md#statistics-options).

<a name="auto_tuning"></a> **\<automatic_tuning_option> ::=**      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)])

Aktiviert bzw. deaktiviert die Option `FORCE_LAST_GOOD_PLAN` für die [automatische Optimierung](../../relational-databases/automatic-tuning/automatic-tuning.md).

FORCE_LAST_GOOD_PLAN = { ON | **OFF** }     
EIN     
Die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] erzwingt automatisch den neusten bekannten, geeigneten Plan bei [!INCLUDE[tsql-md](../../includes/tsql-md.md)]-Abfragen, bei denen neue Abfragepläne negative Auswirkungen auf die Leistung haben. Die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] überwacht regelmäßig die Abfrageleistung der [!INCLUDE[tsql-md](../../includes/tsql-md.md)]-Abfrage mit dem erzwungenen Plan.

Wenn die Leistung verbessert wurde, verwendet die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] weiterhin den neusten bekannten, geeigneten Plan. Wenn die Leistung nicht verbessert wurde, erstellt die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] einen neuen Abfrageplan. Die Anweisung schlägt fehl, wenn der Abfragespeicher nicht aktiviert ist oder sich nicht im *Lese-/Schreibmodus* befindet.

OFF     
Die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] meldet potentielle Einbußen im Hinblick auf die Abfrageleistung, die von Änderungen des Abfrageplans in der [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)-Sicht hervorgerufen werden könnten. Diese Empfehlungen werden allerdings nicht automatisch angewendet. Benutzer können aktive Empfehlungen überwachen und ermittelte Probleme beheben, indem sie die in der Sicht aufgeführten [!INCLUDE[tsql-md](../../includes/tsql-md.md)]-Skripts anwenden. Der Standardwert ist OFF.

**\<change_tracking_option> ::=**      
**Gilt für** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDSFull](../../includes/sssds-md.md)]

Steuert Änderungsnachverfolgungsoptionen. Sie können die Änderungsnachverfolgung aktivieren, Optionen festlegen, Optionen ändern und die Änderungsnachverfolgung deaktivieren. Beispiele hierzu finden Sie im Abschnitt „Beispiele“ weiter unten in diesem Artikel.

EIN     
Aktiviert die Änderungsnachverfolgung für die Datenbank. Wenn die Änderungsnachverfolgung aktiviert wird, können auch die AUTO CLEANUP-Option und die CHANGE RETENTION-Option festgelegt werden.

AUTO_CLEANUP = { ON | **OFF** }     
EIN     
Die Änderungsnachverfolgungsdaten werden nach der angegebenen Beibehaltungsdauer automatisch entfernt.

OFF     
Die Änderungsnachverfolgungsdaten werden nicht automatisch aus der Datenbank entfernt.

CHANGE_RETENTION = *retention_period* { **DAYS** | HOURS | MINUTES }     
Gibt die Mindestdauer für die Beibehaltung von Änderungsnachverfolgungsdaten in der Datenbank an. Die Daten werden nur dann entfernt, wenn der Wert für AUTO_CLEANUP ON lautet.

*retention_period* ist ein Integer, der die numerische Komponente der Vermerkdauer angibt.

Die Standardbeibehaltungsdauer beträgt **2 Tage**. Die Mindestbeibehaltungsdauer ist 1 Minute. Der Standardtyp für die Beibehaltung ist **DAYS**.

OFF     
Deaktiviert die Änderungsnachverfolgung für die Datenbank. Deaktivieren Sie erst die Änderungsnachverfolgung für alle Tabellen, bevor Sie sie für die Datenbank deaktivieren.

**\<containment_option> ::=**      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Steuert die Einschlussoptionen für Datenbanken.

CONTAINMENT = { **NONE** | PARTIAL}     
Keine     
Die Datenbank ist keine eigenständige Datenbank.

PARTIAL     
Die Datenbank ist eine eigenständige Datenbank. Wenn für die Datenbank die Replikation, das Aufzeichnen oder das Nachverfolgen von Änderungsdaten aktiviert ist, tritt beim Festlegen des Datenbankeinschlusses auf einen partiellen Einschluss ein Fehler auf. Die Fehlerüberprüfung wird nach einem Fehler beendet. Weitere Informationen zu eigenständigen Datenbanken finden Sie unter [Eigenständige Datenbanken](../../relational-databases/databases/contained-databases.md).

**\<cursor_option> ::=**     

Steuert Cursoroptionen.

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
EIN     
Alle beim Commit oder Rollback einer Transaktion geöffneten Cursor werden geschlossen.

OFF     
Cursor bleiben beim Commit einer Transaktion geöffnet. Beim Rollback einer Transaktion werden alle Cursor geschlossen, sofern sie nicht als INSENSITIVE oder STATIC definiert sind.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für CURSOR_CLOSE_ON_COMMIT. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die CURSOR_CLOSE_ON_COMMIT für die Sitzung auf OFF festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).

Sie können den Status dieser Option mithilfe der Spalte `is_cursor_close_on_commit_on` in der Katalogsicht [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) oder der `IsCloseCursorsOnCommitEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion ermitteln.

CURSOR_DEFAULT { LOCAL | GLOBAL }     
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Steuert, ob der Cursorbereich LOCAL oder GLOBAL verwendet.

LOCAL     
Wenn Sie LOCAL angeben und keinen Cursor als GLOBAL definieren, wenn Sie den Cursor erstellen, ist der Gültigkeitsbereich des Cursors lokal. Insbesondere ist der Gültigkeitsbereich des Cursors für den Batch, die gespeicherte Prozedur oder den Trigger lokal, in dem bzw. der Sie ihn erstellt haben. Der Cursorname ist nur innerhalb dieses Bereichs gültig.

Auf den Cursor kann durch lokale Cursorvariablen im Batch, in der gespeicherten Prozedur, im Trigger oder im OUTPUT-Parameter einer gespeicherten Prozedur verwiesen werden. Die Zuordnung des Cursors wird implizit aufgehoben, wenn der Batch, die gespeicherte Prozedur oder der Trigger beendet wird. Die Zuordnung des Cursors wird aufgehoben, außer er wurde zurück in einen OUTPUT-Parameter übergeben. Der Cursor könnte zurück in einen OUTPUT-Parameter übergeben werden. Wenn die Rückgabe des Cursors auf diese Weise erfolgt, wird die Zuordnung des Cursors aufgehoben, wenn die Zuordnung der letzten auf ihn verweisenden Variablen aufgehoben wird, oder wenn der Cursor den Gültigkeitsbereich verlässt.

GLOBAL     
Wenn GLOBAL angegeben wurde und beim Erstellen kein Cursor als LOCAL definiert wird, ist der Bereich des Cursors global für die Verbindung. Auf den Cursornamen kann in jeder gespeicherten Prozedur und in jedem Batch verwiesen werden, die bzw. der von der Verbindung ausgeführt wird.

Die Zuordnung des Cursors wird implizit nur aufgehoben, wenn die Verbindung getrennt wird. Weitere Informationen finden Sie unter [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md).

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_local_cursor_default` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsLocalCursorsDefault`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

**\<data_retention_policy> ::=**

**Gilt für**: *nur* Azure SQL Edge

DATA_RETENTION { ON | OFF }   
EIN    
Aktiviert das auf der Datenaufbewahrungsrichtlinie basierende Bereinigen einer Datenbank.

OFF   
Deaktiviert das auf der Datenaufbewahrungsrichtlinie basierende Bereinigen einer Datenbank.

**\<database_mirroring>**     
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Die Argumentbeschreibungen finden Sie unter [ALTER DATABASE-Datenbankspiegelung](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md).

**\<date_correlation_optimization_option> ::=**      
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Steuert die Option DATE_CORRELATION_OPTIMIZATION.

DATE_CORRELATION_OPTIMIZATION { ON | **OFF** }     
EIN     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwaltet die Korrelationsstatistiken, wenn zwei beliebige Tabellen in der Datenbank durch eine FOREIGN KEY-Einschränkung verknüpft sind, und die Tabellen über **datetime**-Spalten verfügen.

OFF     
Es werden keine Korrelationsstatistiken verwaltet.

Wenn DATE_CORRELATION_OPTIMIZATION auf ON festgelegt werden soll, darf keine aktive Verbindung mit der Datenbank bestehen, außer der Verbindung, über die die ALTER DATABASE-Anweisung ausgeführt wird. Anschließend werden mehrere Verbindungen unterstützt.

Die aktuelle Einstellung dieser Option kann mithilfe der Spalte `is_date_correlation_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht ermittelt werden.

**\<db_encryption_option> ::=**      
Steuert den Status der Datenbankverschlüsselung.

ENCRYPTION {ON \| **OFF** \| SUSPEND \| RESUME}     
EIN     
Legt fest, dass die Datenbank verschlüsselt wird.

OFF     
Legt fest, dass die Datenbank nicht verschlüsselt wird.

SUSPEND     
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])     

Kann verwendet werden, um den Verschlüsselungsscan anzuhalten, nachdem TDE aktiviert oder deaktiviert wurde oder der Verschlüsselungsschlüssel geändert wurde.

RESUME     
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Kann verwendet werden, um einen zuvor angehaltenen Verschlüsselungsscan fortzusetzen.

Weitere Informationen finden Sie unter [Transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md) und [Transparent Data Encryption in Azure SQL-Datenbank](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

Wenn die Verschlüsselung auf Datenbankebene aktiviert wird, werden alle Dateigruppen verschlüsselt. Alle neuen Dateigruppen erben die verschlüsselte Eigenschaft. Wenn Dateigruppen in der Datenbank als READ ONLY festgelegt sind, tritt beim Datenbankverschlüsselungsvorgang ein Fehler auf.

Sie können den sowohl den Verschlüsselungsstatus der Datenbank als auch den Status des Verschlüsselungsscans mithilfe der dynamischen Verwaltungssicht [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) anzeigen.

**\<db_state_option> ::=**      
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Steuert den Status der Datenbank.

OFFLINE     
Die Datenbank ist geschlossen, ordnungsgemäß heruntergefahren und als offline gekennzeichnet. Die Datenbank kann nicht geändert werden, während sie als offline gekennzeichnet ist.

ONLINE     
Die Datenbank ist geöffnet und kann verwendet werden.

EMERGENCY     
Die Datenbank ist als READ_ONLY markiert, die Protokollierung deaktiviert und der Zugriff auf Mitglieder der festen Serverrolle sysadmin beschränkt. Der Status EMERGENCY wird hauptsächlich zu Problembehandlungszwecken verwendet. Beispielsweise kann für eine Datenbank, die wegen einer beschädigten Protokolldatei als fehlerverdächtig gekennzeichnet ist, der Status EMERGENCY festgelegt werden. Durch diese Einstellung wird u. U. für den Systemadministrator der schreibgeschützte Zugriff auf die Datenbank aktiviert. Nur Mitglieder der festen Serverrolle sysadmin können für eine Datenbank den Status NOTFALL festlegen.

Erfordert die `ALTER DATABASE`-Berechtigung für die Betreffdatenbank, um eine Datenbank in den Status OFFLINE oder EMERGENCY zu ändern, und die `ALTER ANY DATABASE`-Berechtigung auf Serverebene, um eine Datenbank von OFFLINE in ONLINE zu verschieben.

Sie können den Status dieser Option ermitteln, indem Sie die Spalten `state` und `state_desc` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `Status`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen. Weitere Informationen finden Sie unter [Database States](../../relational-databases/databases/database-states.md).

Für eine Datenbank, die als RESTORING gekennzeichnet ist, kann nicht OFFLINE, ONLINE oder EMERGENCY festgelegt werden. Eine Datenbank kann den Status RESTORING aufweisen, während ein Wiederherstellungsvorgang aktiv ist oder wenn ein Wiederherstellungsvorgang einer Datenbank oder Protokolldatei aufgrund einer beschädigten Sicherungsdatei fehlschlägt.

**\<db_update_option> ::=**

Steuert, ob Updates für die Datenbank zugelassen sind.

READ_ONLY     
Benutzer können Daten aus der Datenbank lesen, aber nicht ändern.

> [!NOTE]
> Um die Abfrageleistung zu verbessern, sollten Sie vor dem Festlegen einer Datenbank auf READ_ONLY die Statistiken aktualisieren. Wenn weitere Statistiken benötigt werden, nachdem eine Datenbank auf READ_ONLY festgelegt wurde, erstellt das [!INCLUDE[ssDE](../../includes/ssde-md.md)] Statistiken in tempdb. Weitere Informationen zu Statistiken für eine schreibgeschützte Datenbank finden Sie unter [Statistiken](../../relational-databases/statistics/statistics.md).

READ_WRITE     
Die Datenbank ist für Lese- und Schreibvorgänge verfügbar.

Sie müssen über exklusiven Zugriff auf die Datenbank verfügen, um diesen Status zu ändern. Weitere Informationen finden Sie unter der SINGLE_USER-Klausel.

> [!NOTE]
> Bei Verbunddatenbanken in [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] ist `SET { READ_ONLY | READ_WRITE }` deaktiviert.

**\<db_user_access_option> ::=**      
Steuert den Benutzerzugriff auf die Datenbank.

SINGLE_USER     
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Gibt an, dass jeweils nur ein Benutzer auf die Datenbank zugreifen kann. Wenn Sie SINGLE_USER angeben und sich andere Benutzer mit der Datenbank verbinden, wird die ALTER DATABASE-Anweisung blockiert, bis alle Benutzer die Verbindung mit der angegebenen Datenbank trennen. Informationen zur Außerkraftsetzung dieses Verhaltens finden Sie im Abschnitt zur WITH \<termination>-Klausel.

Die Datenbank verbleibt im SINGLE_USER-Modus, selbst wenn sich der Benutzer abmeldet, der die Option festgelegt hat. Dadurch kann ein anderer Benutzer (aber nur einer) eine Verbindung mit der Datenbank herstellen.

Bevor Sie die Datenbank auf SINGLE_USER festlegen, müssen Sie überprüfen, ob die Option AUTO_UPDATE_STATISTICS_ASYNC auf OFF festgelegt ist. Wenn diese Option auf ON festgelegt ist, stellt der Hintergrundthread, der zum Aktualisieren von Statistiken verwendet wird, eine Verbindung mit der Datenbank her, und Sie können im Einzelbenutzermodus nicht auf die Datenbank zugreifen. Fragen Sie zum Anzeigen des Status dieser Option die `is_auto_update_stats_async_on`-Spalte in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht ab. Wenn die Option auf ON festgelegt wird, sollten Sie folgende Tasks ausführen:

1. Legen Sie AUTO_UPDATE_STATISTICS_ASYNC auf OFF fest.

2. Führen Sie eine Überprüfung auf aktive asynchrone Statistikaufträge aus, indem Sie die dynamische Verwaltungssicht [sys.dm_exec_background_job_queue](../../relational-databases/system-dynamic-management-views/sys-dm-exec-background-job-queue-transact-sql.md) abfragen.

Wenn aktive Aufträge vorhanden sind, warten Sie, bis die Aufträge abgeschlossen sind, oder beenden Sie sie manuell mithilfe von [KILL STATS JOB](../../t-sql/language-elements/kill-stats-job-transact-sql.md).

RESTRICTED_USER     
Ermöglicht nur Mitgliedern der festen Datenbankrolle `db_owner` und der festen Serverrollen `dbcreator` und `sysadmin` eine Verbindung mit der Datenbank. RESTRICTED_USER beschränkt nicht deren Anzahl. Trennen Sie alle Verbindungen mit der Datenbank, indem Sie den durch die Beendigungsklausel der ALTER DATABASE-Anweisung angegebenen Zeitraum verwenden. Sobald die Datenbank in den Status RESTRICTED_USER gewechselt hat, werden Verbindungsversuche von nicht qualifizierten Benutzern abgelehnt.

MULTI_USER     
Alle Benutzer, die über die entsprechenden Berechtigungen für die Verbindung mit der Datenbank verfügen, sind zugelassen.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `user_access` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `UserAccess`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

**\<delayed_durability_option> ::=**      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)])

Steuert, ob für Transaktionen ein Commit mit vollständiger oder verzögerter Dauerhaftigkeit ausgeführt wird.

DISABLED     
Alle Transaktionen nach `SET DISABLED` sind vollständig dauerhaft. Alle Dauerhaftigkeitsoptionen, die in einem Atomic-Block oder einer Commitanweisung festgelegt sind, werden ignoriert.

ALLOWED     
Alle Transaktionen nach `SET ALLOWED` sind abhängig von der im atomischen Block oder der Commitanweisung festgelegten Dauerhaftigkeitsoption entweder vollständig dauerhaft oder verzögert dauerhaft.

FORCED     
Alle Transaktionen nach `SET FORCED` sind verzögert dauerhaft. Alle Dauerhaftigkeitsoptionen, die in einem Atomic-Block oder einer Commitanweisung festgelegt sind, werden ignoriert.

**\<external_access_option> ::=**      
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Steuert, ob externe Ressourcen, z. B. Objekte aus einer anderen Datenbank, auf die Datenbank zugreifen können.

DB_CHAINING { ON | **OFF** }     
EIN     
Die Datenbank kann Quelle oder Ziel einer datenbankübergreifenden Besitzverkettung sein.

OFF     
Die Datenbank kann nicht an der datenbankübergreifenden Besitzverkettung teilnehmen.

> [!IMPORTANT]
> Die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erkennt diese Einstellung, wenn die Datenbankübergreifende Besitzverkettung-Serveroption deaktiviert (0 bzw. OFF) ist. Wenn für Datenbankübergreifende Besitzverkettung der Wert 1 (ON) festgelegt ist, können alle Benutzerdatenbanken unabhängig vom Wert dieser Option Teile von datenbankübergreifenden Besitzketten sein. Diese Option wird mit [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) festgelegt.

Für das Festlegen dieser Option ist die `CONTROL SERVER`-Berechtigung für die Datenbank erforderlich.

Die Option DB_CHAINING kann nicht für master-, model- und tempdb-Systemdatenbanken festgelegt werden.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_db_chaining_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen.

TRUSTWORTHY { ON | **OFF** }     
EIN     
Datenbankmodule (z. B. benutzerdefinierte Funktionen oder gespeicherte Prozeduren), die einen Identitätswechselkontext verwenden, können auf Ressourcen außerhalb der Datenbank zugreifen.

OFF     
Datenbankmodule in einem Identitätswechselkontext können nicht auf Ressourcen außerhalb der Datenbank zugreifen.

TRUSTWORTHY wird auf OFF festgelegt, wenn die Datenbank angefügt wird.

Standardmäßig ist TRUSTWORTHY für alle Systemdatenbanken mit Ausnahme der msdb-Datenbank auf OFF festgelegt. Der Wert kann für die „model“- und „tempdb“-Datenbanken nicht geändert werden. Für die master-Datenbank sollten Sie die Option TRUSTWORTHY niemals auf ON festlegen.

Für das Festlegen dieser Option ist die `CONTROL SERVER`-Berechtigung für die Datenbank erforderlich.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_trustworthy_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen.

DEFAULT_FULLTEXT_LANGUAGE     
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Gibt den Standardsprachenwert für volltextindizierte Spalten an.

> [!IMPORTANT]
> Diese Option ist nur zulässig, wenn CONTAINMENT auf PARTIAL festgelegt wurde. Wenn CONTAINMENT auf NONE festgelegt wird, treten Fehler auf.

DEFAULT_LANGUAGE     
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Gibt die Standardsprache für alle neu erstellten Benutzernamen an. Die Sprache kann durch Bereitstellung der lokalen ID (lcid), des Sprachennamens oder des Sprachenalias angegeben werden. Eine Liste mit zulässigen Sprachennamen und -aliasen finden Sie unter [sys.syslanguages](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md). Diese Option ist nur zulässig, wenn CONTAINMENT auf PARTIAL festgelegt wurde. Wenn CONTAINMENT auf NONE festgelegt wird, treten Fehler auf.

NESTED_TRIGGERS     
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Gibt an, ob ein AFTER-Trigger kaskadiert werden kann, d. h., ob er eine Aktion ausführen kann, durch die ein anderer Trigger initiiert wird, der einen weiteren Trigger initiiert usw. Diese Option ist nur zulässig, wenn CONTAINMENT auf PARTIAL festgelegt wurde. Wenn CONTAINMENT auf NONE festgelegt wird, treten Fehler auf.

TRANSFORM_NOISE_WORDS     
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Wird zum Unterdrücken einer Fehlermeldung verwendet, wenn Füllwörter oder Stoppwörter bewirken, dass eine boolesche Operation für eine Volltextabfrage einen Fehler erzeugt. Diese Option ist nur zulässig, wenn CONTAINMENT auf PARTIAL festgelegt wurde. Wenn CONTAINMENT auf NONE festgelegt wird, treten Fehler auf.

TWO_DIGIT_YEAR_CUTOFF     
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Gibt eine ganze Zahl zwischen 1753 und 9999 an, die das Umstellungsjahr für das Interpretieren zweistelliger Jahre als vierstellige Jahre darstellt. Diese Option ist nur zulässig, wenn CONTAINMENT auf PARTIAL festgelegt wurde. Wenn CONTAINMENT auf NONE festgelegt wird, treten Fehler auf.

**\<FILESTREAM_option> ::=**      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Steuert die Einstellungen für FileTables.

NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL }     
OFF     
Nicht transaktionaler Zugriff auf FileTable-Daten ist deaktiviert.

READ_ONLY     
FILESTREAM-Daten in FileTables in dieser Datenbank können von nicht transaktionalen Prozessen gelesen werden.

FULL     
Aktiviert vollständigen, nicht transaktionalen Zugriff auf FILESTREAM-Daten in FileTables.

DIRECTORY_NAME = *\<directory_name>*      
Ein Windows-kompatibler Verzeichnisname. Dieser Name sollte für alle Verzeichnisnamen auf Datenbankebene in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz eindeutig sein. Bei Eindeutigkeitsvergleichen wird unabhängig von den Sortiereinstellungen die Groß-/Kleinschreibung nicht beachtet. Diese Option muss vor dem Erstellen einer FileTable in dieser Datenbank festgelegt werden.

**\<HADR_options> ::=**      
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Siehe [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md).

**\<mixed_page_allocation_option> ::=**      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])

Steuert, ob die Datenbank die ersten Seiten mit einem gemischten Block für die ersten acht Seiten einer Tabelle oder eines Index erstellen kann.

MIXED_PAGE_ALLOCATION { OFF | **ON** }     
OFF     
Die Datenbank erstellt die ersten Seiten immer mit gleichartigen Blöcken. OFF ist der Standardwert.

EIN     
Die Datenbank erstellt die ersten Seiten immer mit gemischten Blöcken.

Diese Einstellung ist für alle Systemdatenbanken auf ON festgelegt. **tempdb** ist die einzige Systemdatenbank, die die OFF-Einstellung unterstützt.

**\<PARAMETERIZATION_option> ::=**      
Steuert die Parametrisierungsoption. Weitere Informationen zur Parametrisierung finden Sie im [Handbuch zur Architektur der Abfrageverarbeitung](../../relational-databases/query-processing-architecture-guide.md#SimpleParam).

PARAMETERIZATION { **SIMPLE** | FORCED }     
SIMPLE     
Abfragen werden basierend auf dem Standardverhalten der Datenbank parametrisiert.

FORCED     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] parametrisiert alle Abfragen in der Datenbank.

Die aktuelle Einstellung dieser Option kann mithilfe der Spalte `is_parameterization_forced column` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht ermittelt werden.

<a name="query-store"></a> **\<query_store_options> ::=**      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])

ON | **OFF** [ ( FORCED )  ] | CLEAR [ ALL ]     
Überprüft, ob der Abfragespeicher in dieser Datenbank aktiviert ist, und steuert außerdem das Entfernen des Inhalts des Abfragespeichers. Weitere Informationen finden Sie unter [Verwendungsszenarios für den Abfragespeicher](../../relational-databases/performance/query-store-usage-scenarios.md).

EIN     
Aktiviert den Abfragespeicher.

OFF [ ( FORCED ) ]      
Deaktiviert den Abfragespeicher. OFF ist der Standardwert. FORCED ist optional. FORCED bricht alle Hintergrundaufgaben, die gerade im Abfragespeicher ausgeführt werden, ab und überspringt die synchrone Leerung, wenn der Abfragespeicher deaktiviert wird. Bewirkt, dass der Abfragespeicher so schnell wie möglich heruntergefahren wird. FORCED gilt für [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU14, [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU21, [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU6 und höhere Builds.

> [!NOTE]  
> In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] mit einer Einzeldatenbank oder einem Pool für elastische Datenbanken kann der Abfragespeicher nicht deaktiviert werden. Beim Ausführen von `ALTER DATABASE [database] SET QUERY_STORE = OFF` wird die Warnung `'QUERY_STORE=OFF' is not supported in this version of SQL Server.` zurückgegeben. 

CLEAR [ ALL ]     
Entfernt abfragebezogene Daten aus dem Abfragespeicher. ALL ist optional. ALL entfernt abfragebezogene Daten und Metadaten aus dem Abfragespeicher.

OPERATION_MODE { READ_ONLY | READ_WRITE }     
Beschreibt den Betriebsmodus des Abfragespeichers.

READ_WRITE     
In diesem Modus sammelt und speichert der Abfragespeicher Angaben zum Abfrageplan und statistische Informationen zur Laufzeitausführung.

READ_ONLY     
Im diesem Modus können Informationen aus dem Abfragespeicher gelesen werden, es werden jedoch keine neuen Informationen hinzugefügt. Wenn der maximal ausgegebene Speicherplatz des Abfragespeichers ausgelastet ist, wird der Betriebsmodus in READ_ONLY geändert.

CLEANUP_POLICY     
Beschreibt die Datenaufbewahrungsrichtlinie des Abfragespeichers. STALE_QUERY_THRESHOLD_DAYS bestimmt die Anzahl an Tagen, für die die Informationen für eine Abfrage im Abfragespeicher aufbewahrt werden. STALE_QUERY_THRESHOLD_DAYS weist den Typ **bigint** auf. Der Standardwert ist 30.

DATA_FLUSH_INTERVAL_SECONDS     
Bestimmt die Häufigkeit, mit der in den Abfragespeicher geschriebene Daten auf Datenträger gespeichert werden. Um die Leistung zu optimieren, werden durch den Abfragespeicher gesammelte Daten asynchron auf den Datenträger geschrieben. Die Häufigkeit, mit der diese asynchrone Übertragung stattfindet, wird mit dem Argument DATA_FLUSH_INTERVAL_SECONDS konfiguriert. DATA_FLUSH_INTERVAL_SECONDS weist den Typ **bigint** auf. Der Standardwert ist **900** (15 Minuten).

MAX_STORAGE_SIZE_MB     
Bestimmt den Speicherplatz, der vom Abfragespeicher belegt wird. MAX_SIZE_MB weist den Typ **bigint** auf. Der Standardwert ist **100 MB** für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] durch [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]). Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] beträgt der Standardwert **1 GB**.

> [!NOTE]
> Die Begrenzung `MAX_STORAGE_SIZE_MB` wird nicht streng erzwungen. Die Speichergröße wird nur überprüft, wenn der Abfragespeicher Daten auf einen Datenträger schreibt. Dieses Intervall wird durch die Option `DATA_FLUSH_INTERVAL_SECONDS` oder die Option **Datenleerungsintervall** im Dialogfeld des [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]-Abfragespeichers festgelegt. Der Standardwert des Intervalls liegt bei 900 Sekunden (oder 15 Minuten).
> Wenn der Abfragespeicher die Begrenzung `MAX_STORAGE_SIZE_MB` zwischen Speichergrößenüberprüfungen überschritten hat, wechselt er in den schreibgeschützten Modus. Bei Aktivierung von `SIZE_BASED_CLEANUP_MODE` wird auch der Bereinigungsmechanismus zum Erzwingen der Begrenzung `MAX_STORAGE_SIZE_MB` ausgelöst.
> Sobald ausreichend Speicherplatz befreit wurde, wird der Abfragespeichermodus automatisch in den Lese-/Schreibmodus gewechselt.

> [!IMPORTANT]
> Wenn Sie glauben, dass für Ihre Arbeitsauslastungserfassung mehr als 10 GB Speicherplatz benötigt werden, sollten Sie vermutlich die Arbeitsauslastung überdenken und optimieren, um Abfragepläne wiederzuverwenden (z. B. mithilfe von [erzwungener Parametrisierung](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)), oder die Abfragespeicherkonfigurationen anpassen.    
> Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] und in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] können Sie für zusätzliche Kontrolle über die Abfrageerfassungsrichtlinie `QUERY_CAPTURE_MODE` auf CUSTOM festlegen.

INTERVAL_LENGTH_MINUTES     
Bestimmt das Zeitintervall, mit dem statistische Daten zur Laufzeitausführung im Abfragespeicher aggregiert werden. Um die Speicherverwendung zu optimieren, werden die statistischen Daten zur Laufzeitausführung im Speicher für Laufzeitstatistiken über ein festes Zeitfenster aggregiert. Dieses feste Zeitfenster wird mit dem Argument INTERVAL_LENGTH_MINUTES konfiguriert. INTERVAL_LENGTH_MINUTES weist den Typ **bigint** auf. Der Standardwert lautet **60**.

SIZE_BASED_CLEANUP_MODE { **AUTO** | OFF }     
Steuert, ob die Bereinigung automatisch aktiviert wird, wenn sich die Gesamtmenge der Daten der maximalen Größe nähert.

AUTO     
Eine auf der Größe basierende Bereinigung wird automatisch aktiviert, wenn die Größe auf dem Datenträger 90 % von **MAX_STORAGE_SIZE_MB** erreicht. Ein auf der Größe basierendes Cleanup entfernt die am wenigsten aufwendigen und die ältesten Abfragen. Bei ungefähr 80 % von **MAX_STORAGE_SIZE_MB** wird dieser Vorgang angehalten. Dies ist der Standardkonfigurationswert.

OFF     
Eine auf der Größe basierte Bereinigung wird nicht automatisch aktiviert.

SIZE_BASED_CLEANUP_MODE ist vom Typ **nvarchar**.

QUERY_CAPTURE_MODE {ALL \| AUTO \| CUSTOM \| NONE}     
Bestimmt den zum aktuellen Zeitpunkt aktiven Abfrageerfassungsmodus. Für jeden Modus sind bestimmte Abfrageerfassungsrichtlinien definiert.

> [!NOTE]
> Cursor, Abfragen in gespeicherten Prozeduren und nativ kompilierte Abfragen werden immer erfasst, wenn der Abfrageerfassungsmodus auf ALL, AUTO oder CUSTOM festgelegt ist.

ALL     
Erfasst alle Abfragen. **ALL** ist der Standardkonfigurationswert für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] bis [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]).

AUTO     
Relevante Abfragen werden anhand der Ausführungsanzahl und des Ressourcenverbrauchs erfasst. Dies ist der Standardkonfigurationswert für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Keine     
Es werden keine weiteren neuen Abfragen erfasst. Der Abfragespeicher sammelt weiterhin Statistiken zur Kompilierung und Runtime für Abfragen, die bereits erfasst wurden. Verwenden Sie diese Konfiguration mit Bedacht, da dadurch möglicherweise wichtige Abfragen verloren gehen.

CUSTOM     
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Ermöglicht die Kontrolle über die QUERY_CAPTURE_POLICY-Optionen.

QUERY_CAPTURE_MODE ist vom Typ **nvarchar**. 

max_plans_per_query     
Definiert die maximale Anzahl von Plänen, die für jede Abfrage beibehalten werden. MAX_PLANS_PER_QUERY ist vom Typ **int**. Der Standardwert ist **200**.

WAIT_STATS_CAPTURE_MODE { **ON** | OFF }     
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]))

Steuert, ob Wartestatistiken pro Abfrage erfasst werden

EIN    
Wartestatistikinformationen pro Abfrage werden erfasst. Dies ist der Standardkonfigurationswert.

OFF    
Wartestatistikinformationen pro Abfrage werden nicht erfasst.

**\<query_capture_policy_option_list> :: =**      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Steuert die Optionen für die Erfassungsrichtlinie des Abfragespeichers. Mit Ausnahme von STALE_CAPTURE_POLICY_THRESHOLD definieren diese Optionen die OR-Bedingungen, die auftreten müssen, damit Abfragen im definierten STALE_CAPTURE_POLICY_THRESHOLD-Wert erfasst werden.

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
Definiert den Zeitraum des Auswertungsintervalls um zu bestimmen, ob eine Abfrage erfasst werden sollte. Der Standardwert ist 1 Tag. Sie können Werte zwischen 1 Stunde und sieben Tagen festlegen. *number* ist vom Datentyp **int**.

EXECUTION_COUNT     
Definiert die Häufigkeit, mit der eine Abfrage im Auswertungszeitraum ausgeführt wird. Der Standardwert ist 30, was bedeutet, dass für den Stale Capture Policy Threshold-Standardwert eine Abfrage mindestens 30 Mal an einem Tag ausgeführt werden muss, um im Abfragespeicher persistent gespeichert zu werden. EXECUTION_COUNT ist vom Typ **int**.

TOTAL_COMPILE_CPU_TIME_MS     
Definiert die gesamte verstrichene Kompilierungs-CPU-Zeit, die eine Abfrage über den Auswertungszeitraum in Anspruch nimmt. Der Standardwert ist 1000, was bedeutet, dass für den Stale Capture Policy Threshold-Standardwert eine Abfrage insgesamt mindestens eine Sekunde CPU-Zeit während der Abfragekompilierung an einem Tag verbraucht haben muss, um im Abfragespeicher persistent gespeichert zu werden. TOTAL_COMPILE_CPU_TIME_MS ist vom Typ **int**.

TOTAL_EXECUTION_CPU_TIME_MS     
Definiert die gesamte verstrichene Ausführungs-CPU-Zeit, die eine Abfrage über den Auswertungszeitraum in Anspruch nimmt. Der Standardwert ist 100, was bedeutet, dass für den Stale Capture Policy Threshold-Standardwert eine Abfrage insgesamt mindestens 100 ms CPU-Zeit während der Ausführung an einem Tag verbraucht haben muss, um im Abfragespeicher persistent gespeichert zu werden. TOTAL_EXECUTION_CPU_TIME_MS ist vom Typ **int**.

**\<recovery_option> ::=**      
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Steuert Datenbankwiederherstellungsoptionen und Datenträger-E/A-Fehlerprüfung.

FULL     
Stellt nach einem Medienausfall mithilfe von Transaktionsprotokollsicherungen eine vollständige Wiederherstellung bereit. Falls eine Datendatei beschädigt ist, kann die Medienwiederherstellung alle Transaktionen wiederherstellen, für die ein Commit ausgeführt wurde. Weitere Informationen finden Sie unter [Wiederherstellungsmodelle](../../relational-databases/backup-restore/recovery-models-sql-server.md).

BULK_LOGGED     
Stellt nach einem Medienausfall Wiederherstellung bereit. Kombiniert hierzu die beste Leistung mit der geringsten Verwendung von Protokollspeicher für bestimmte umfangreiche Vorgänge oder Massenvorgänge. Informationen zu Vorgängen, die minimal protokolliert werden können, finden Sie unter [Das Transaktionsprotokoll](../../relational-databases/logs/the-transaction-log-sql-server.md). Bei dem BULK_LOGGED-Wiederherstellungsmodell ist die Protokollierung für diese Vorgänge minimal. Weitere Informationen finden Sie unter [Wiederherstellungsmodelle](../../relational-databases/backup-restore/recovery-models-sql-server.md).

SIMPLE     
Es wird eine einfache Sicherungsstrategie bereitgestellt, die minimalen Protokollspeicherplatz verwendet. Protokollspeicherplatz kann automatisch erneut verwendet werden, wenn er für die Wiederherstellung nach einem Serverfehler nicht mehr benötigt wird. Weitere Informationen finden Sie unter [Wiederherstellungsmodelle](../../relational-databases/backup-restore/recovery-models-sql-server.md).

> [!IMPORTANT]
> Das Modell der einfachen Wiederherstellung ist einfacher zu verwalten als die anderen beiden Modelle, jedoch auf Kosten eines höheren Datenverlustes, falls eine Datendatei beschädigt ist. Alle Änderungen, die nach der neuesten Datenbank- oder differenziellen Datenbanksicherung durchgeführt wurden, gehen verloren und müssen manuell erneut eingegeben werden.

Das standardmäßige Wiederherstellungsmodell wird durch das Wiederherstellungsmodell der **model**-Datenbank bestimmt. Weitere Informationen zum Auswählen des geeigneten Wiederherstellungsmodells finden Sie unter [Wiederherstellungsmodelle](../../relational-databases/backup-restore/recovery-models-sql-server.md).

Sie können den Status dieser Option ermitteln, indem Sie die Spalten **recovery_model** und **recovery_modl_desc** in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `Recovery`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

TORN_PAGE_DETECTION { ON | **OFF** }     
EIN     
Unvollständige Seiten können vom [!INCLUDE[ssDE](../../includes/ssde-md.md)] erkannt werden.

OFF     
Unvollständige Seiten können nicht vom [!INCLUDE[ssDE](../../includes/ssde-md.md)] erkannt werden.

> [!IMPORTANT]
> Die TORN_PAGE_DETECTION ON | OFF-Syntaxstruktur wird in zukünftigen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entfernt. Vermeiden Sie das Verwenden dieser Syntaxstruktur bei neuen Entwicklungsarbeiten, und planen Sie die Änderung von Anwendungen, die diese Syntaxstruktur zurzeit verwenden. Verwenden Sie stattdessen die Option PAGE_VERIFY.

<a name="page_verify"></a> PAGE_VERIFY { **CHECKSUM** | TORN_PAGE_DETECTION | NONE }     
Entdeckt Datenbankseiten, die durch Datenträger-E/A-Pfadfehler beschädigt wurden. Datenträger-E/A-Pfadfehler können die Ursache von Datenbankbeschädigungen sein. Diese Fehler werden am häufigsten durch Stromausfälle oder Datenträger-Hardwarefehler verursacht, die beim Schreiben der Seite auf den Datenträger auftreten.

CHECKSUM     
Berechnet eine Prüfsumme für den Inhalt der gesamten Seite und speichert den Wert im Seitenkopf, wenn eine Seite auf den Datenträger geschrieben wird. Wenn die Seite vom Datenträger gelesen wird, wird die Prüfsumme erneut berechnet und mit dem im Seitenkopf gespeicherten Prüfsummenwert verglichen. Stimmen die Werte nicht überein, wird Fehlermeldung 824 (Hinweis auf einen Prüfsummenfehler) an das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll und an das Windows-Ereignisprotokoll gemeldet. Ein Prüfsummenfehler weist auf ein Problem mit dem E/A-Pfad hin. Um die eigentliche Ursache zu ermitteln, müssen die Hardware, die Firmwaretreiber, das BIOS, die Filtertreiber (z. B. Antivirussoftware) und andere Komponenten des E/A-Pfads untersucht werden.

TORN_PAGE_DETECTION     
Speichert ein bestimmtes 2-Bit-Muster für jeden 512-Byte-Sektor in der 8-KB-Datenbankseite und wird im Kopf der Datenbankseite gespeichert, wenn die Seite auf den Datenträger geschrieben wird. Wenn die Seite vom Datenträger gelesen wird, werden die im Seitenkopf gespeicherten zerrissenen Bits mit den tatsächlichen Seitensektorinformationen verglichen.

Nicht übereinstimmende Werte weisen darauf hin, dass nur ein Teil der Seite auf den Datenträger geschrieben wurde. In dieser Situation wird Fehlermeldung 824 (Hinweis auf einen Fehler durch eine zerrissene Seite) an das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll und an das Windows-Ereignisprotokoll gemeldet. Zerrissene Seiten werden im Allgemeinen bei der Datenbankwiederherstellung entdeckt, wenn es sich tatsächlich um einen unvollständigen Schreibvorgang für eine Seite handelt. Allerdings können auch andere E/A-Pfadfehler jederzeit eine zerrissene Seite verursachen.

Keine     
Schreibvorgänge auf Datenbankseiten generieren keinen CHECKSUM- oder TORN_PAGE_DETECTION-Wert. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] überprüft während eines Lesevorgangs selbst dann keine Prüfsummen oder zerrissenen Seiten, wenn ein CHECKSUM- oder TORN_PAGE_DETECTION-Wert im Seitenkopf vorhanden ist.

Beachten Sie beim Verwenden der PAGE_VERIFY-Option die folgenden wichtigen Punkte:

- Der Standardwert ist **CHECKSUM**.
- Wenn eine Benutzer- oder Systemdatenbank auf [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] oder eine höhere Version aktualisiert wird, bleibt der PAGE_VERIFY-Wert (NONE oder TORN_PAGE_DETECTION) unverändert. Sie sollten den Wert in CHECKSUM ändern.

    > [!NOTE]
    > In früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist die Datenbankoption PAGE_VERIFY für die TempDB-Datenbank auf NONE festgelegt und kann nicht geändert werden. Ab [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ist der Standardwert für die Datenbank tempdb für neue Installationen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CHECKSUM. Bei dem Upgrade einer Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bleibt der Standardwert NONE. Die Option kann geändert werden. Sie sollten CHECKSUM für die tempdb-Datenbank verwenden.

- TORN_PAGE_DETECTION verwendet zwar weniger Ressourcen, bietet jedoch einen minimalen Teil des Schutzes von CHECKSUM.
- PAGE_VERIFY kann festgelegt werden, ohne die Datenbank offline zu schalten, zu sperren oder die Parallelität der Datenbank anderweitig zu beeinträchtigen.
- CHECKSUM und TORN_PAGE_DETECTION schließen sich gegenseitig aus. Beide Optionen können nicht gleichzeitig aktiviert werden.

Bei Entdecken einer zerrissenen Seite oder eines Prüfsummenfehlers können Sie eine Wiederherstellung ausführen, indem Sie die Daten wiederherstellen oder den Index u. U. neu erstellen, wenn der Fehler auf Indexseiten beschränkt ist. Führen Sie DBCC CHECKDB aus, um bei einem Prüfsummenfehler den Typ der betroffenen Datenbankseite(n) zu bestimmen. Weitere Informationen zu RESTORE-Optionen finden Sie unter [RESTORE-Argumente](../../t-sql/statements/restore-statements-arguments-transact-sql.md). Auch wenn das Datenbeschädigungsproblem durch das Wiederherstellen der Daten behoben wird, sollte die eigentliche Ursache, wie z. B. ein Datenträger-Hardwarefehler, diagnostiziert und baldmöglichst behoben werden, um wiederholte Fehler zu vermeiden.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wiederholt Lesevorgänge, die wegen eines Prüfsummenfehlers, einer zerrissenen Seite oder eines anderen E/A-Fehlers fehlschlagen, vier Mal. Ist der Lesevorgang bei einem dieser Wiederholungsversuche erfolgreich, wird eine Meldung in das Fehlerprotokoll geschrieben. Der Befehl, der den Lesevorgang ausgelöst hat, wird fortgesetzt. Schlagen alle Wiederholungsversuche fehl, schlägt der Befehl mit Fehlermeldung 824 fehl.

Weitere Informationen zu den Fehlermeldungen 823, 824 und 825 finden Sie unter:

- [How to troubleshoot a Msg 823 error in SQL Server (in englischer Sprache)](https://support.microsoft.com/help/2015755)
- [How to troubleshoot Msg 824 in SQL Server (in englischer Sprache)](https://support.microsoft.com/help/2015756)
- [How to troubleshoot Msg – read retr ](https://support.microsoft.com/help/2015757) (in englischer Sprache).

Die aktuelle Einstellung dieser Option kann mithilfe der `page_verify_option`-Spalte in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht oder der `IsTornPageDetectionEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion ermittelt werden.

**\<remote_data_archive_option> ::=**      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])

Aktiviert bzw. Deaktiviert Stretch Database für die Datenbank. Weitere Informationen finden Sie unter [Stretch Database](../../sql-server/stretch-database/stretch-database.md).

REMOTE_DATA_ARCHIVE = {ON (SERVER = \<server_name> , {CREDENTIAL = \<db_scoped_credential_name> \| FEDERATED_SERVICE_ACCOUNT = ON \| OFF})\| **OFF**     
EIN     
Aktiviert Stretch Database für die Datenbank. Weitere Informationen, einschließlich zusätzlicher Voraussetzungen, finden Sie unter [Aktivieren von Stretch Database für eine Datenbank](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md).

Erfordert die `db_owner`-Berechtigung, um Stretch Database für eine Tabelle zu aktivieren. Erfordert die `db_owner`- und `CONTROL DATABASE`-Berechtigungen, um Stretch Database für eine Datenbank zu aktivieren.

SERVER = \<server_name>     
Gibt die Adresse des Azure-Servers an. Fügen Sie den `.database.windows.net`-Anteil des Namens ein. Beispiel: `MyStretchDatabaseServer.database.windows.net`.

CREDENTIAL = \<db_scoped_credential_name>     
Gibt die datenbankbezogenen Anmeldeinformationen an, die die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet, um eine Verbindung mit dem Azure-Server herzustellen. Vergewissern Sie sich, dass die Anmeldinformationen bestehen, bevor Sie den Befehl ausführen. Weitere Informationen finden Sie unter [CREATE DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).

FEDERATED_SERVICE_ACCOUNT = { ON | OFF }     
Sie können ein Verbunddienstkonto verwenden, damit SQL Server lokal mit dem Azure-Remoteserver kommunizieren kann, wenn sämtliche der folgenden Bedingungen zutreffen.

- Das Dienstkonto, unter dem die SQL Server-Instanz ausgeführt wird, ist ein Domänenkonto.
- Das Domänenkonto gehört zu einer Domäne, deren Active Directory mit Azure Active Directory verbunden ist.
- Der Azure-Remoteserver wird konfiguriert, um die Azure Active Directory-Authentifizierung zu unterstützen.
- Das Dienstkonto, unter dem die SQL Server-Instanz ausgeführt wird, muss auf dem Azure-Remoteserver als ein `dbmanager`- oder `sysadmin`-Konto konfiguriert worden sein.

Wenn Sie angeben, dass das Konto des Verbunddiensts ON ist, können Sie nicht auch noch das CREDENTIAL-Argument angeben. Wenn Sie Off angeben, geben Sie gleichzeitig auch das CREDENTIAL-Argument an.

OFF     
Deaktiviert Stretch Database für die Datenbank. Weitere Informationen finden Sie unter [Deaktivieren von Stretch Database und Zurückholen von Remotedaten](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

Sie können Stretch Database für eine Datenbank erst deaktivieren, wenn diese keine Tabellen mehr enthält, die für Stretch Database aktiviert sind. Nachdem Sie Stretch Database deaktiviert haben, wird die Datenmigration beendet. Außerdem enthalten Abfrageergebnisse keine Ergebnisse mehr aus Remotetabellen.

Auch wenn Stretch deaktiviert wird, wird die Remotedatenbank nicht entfernt. Das Löschen der Remotedatenbank muss über das Azure-Portal erfolgen.

**\<service_broker_option> ::=**      
**Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Steuert die folgenden [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Optionen: Aktivieren oder Deaktivieren der Nachrichtenübermittlung, Festlegen eines neuen [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Bezeichners oder Festlegen der Konversationsprioritäten auf ON oder OFF.

ENABLE_BROKER     
Gibt an, dass [!INCLUDE[ssSB](../../includes/sssb-md.md)] für die angegebene Datenbank aktiviert ist. Die Nachrichtenübermittlung ist gestartet, und das is_broker_enabled-Flag ist in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht auf TRUE festgelegt. Die Datenbank behält den vorhandenen [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Bezeichner bei. Service Broker kann nicht aktiviert werden, während die Datenbank der Prinzipal in einer Datenbank-Spiegelungskonfiguration ist.

> [!NOTE]
> ENABLE_BROKER benötigt eine exklusive Datenbanksperre. Wenn Ressourcen in der Datenbank durch andere Sitzungen gesperrt wurden, wartet ENABLE_BROKER, bis die anderen Sitzungen ihre Sperren freigeben. Um [!INCLUDE[ssSB](../../includes/sssb-md.md)] in einer Benutzerdatenbank zu aktivieren, stellen Sie sicher, dass keine anderen Sitzungen auf die Datenbank zugreifen, bevor Sie die Anweisung ALTER DATABASE SET ENABLE_BROKER ausführen. Setzen Sie zum Beispiel die Datenbank in den Einzelbenutzermodus. Um [!INCLUDE[ssSB](../../includes/sssb-md.md)] in der msdb-Datenbank zu aktivieren, beenden Sie zunächst den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agenten, sodass [!INCLUDE[ssSB](../../includes/sssb-md.md)] die erforderliche Sperre abrufen kann.

DISABLE_BROKER     
Gibt an, dass [!INCLUDE[ssSB](../../includes/sssb-md.md)] für die angegebene Datenbank deaktiviert ist. Die Nachrichtenübermittlung ist angehalten, und das is_broker_enabled-Flag ist in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht auf FALSE festgelegt. Die Datenbank behält den vorhandenen [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Bezeichner bei.

NEW_BROKER     
Gibt an, dass die Datenbank einen neuen Broker-Bezeichner erhalten sollte. Die Datenbank fungiert als neuer Service Broker. Somit werden alle bestehenden Konversationen in der Datenbank sofort entfernt, ohne Nachrichten über das Beenden des Dialogs zu erstellen. Jede Route, die auf den alten [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Bezeichner verweist, muss mit dem neuen Bezeichner neu erstellt werden.

ERROR_BROKER_CONVERSATIONS     
Gibt an, dass die [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Nachrichtenübermittlung aktiviert ist. Mit dieser Einstellung wird der vorhandene [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Bezeichner für die Datenbank beibehalten. [!INCLUDE[ssSB](../../includes/sssb-md.md)] beendet alle Konversationen in der Datenbank mit einem Fehler. Diese Einstellung ermöglicht Anwendungen, reguläre Cleanups für bestehende Konversationen ausführen.

HONOR_BROKER_PRIORITY {ON | OFF}     
EIN     
Bei Sendevorgängen werden die den Konversationen zugewiesenen Prioritätsstufen berücksichtigt. Nachrichten aus Konversationen mit hohen Prioritätsstufen werden in der Regel vor Nachrichten aus Konversationen mit niedrigen Prioritätsstufen gesendet.

OFF     
Sendevorgänge werden so ausgeführt, als ob alle Konversationen die Standardprioritätsstufe haben.

Änderungen an der HONOR_BROKER_PRIORITY-Option treten bei neuen Dialogfeldern oder Dialogfeldern, in denen keine Nachrichten darauf warten, gesendet zu werden, sofort in Kraft. Dialogfelder mit Nachrichten, die gesendet werden sollen, wenn ALTER DATABASE ausgeführt wird, übernehmen die neue Einstellung erst, nachdem einige Nachrichten für das Dialogfeld gesendet wurden. Es kann unterschiedlich lange dauern, bis in allen Dialogfeldern die neue Einstellung verwendet wird.

Die aktuelle Einstellung dieser Eigenschaft wird in der `is_broker_priority_honored`-Spalte der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht angezeigt.

**\<snapshot_option> ::=**      
Berechnet die Isolationsstufe für die Transaktionen.

ALLOW_SNAPSHOT_ISOLATION { ON | **OFF** }     
EIN     
Aktiviert die Momentaufnahmeoption auf Datenbankebene. Wenn die Option aktiviert ist, beginnen DML-Anweisungen mit der Generierung von Zeilenversionen, auch wenn keine Transaktion die Momentaufnahmeisolation verwendet. Sobald diese Option aktiviert ist, können Transaktionen die SNAPSHOT-Transaktionsisolationsstufe angeben. Wenn eine Transaktion auf der SNAPSHOT-Isolationsebene ausgeführt wird, sehen alle Anweisungen eine Momentaufnahme der Daten, wie sie beim Start der Transaktion vorlagen. Greift eine Transaktion, die auf der SNAPSHOT-Isolationsstufe ausgeführt wird, auf Daten in mehreren Datenbanken zu, muss entweder in allen Datenbanken ALLOW_SNAPSHOT_ISOLATION auf ON festgelegt sein oder jede Anweisung in der Transaktion muss Sperrhinweise für alle Verweise in einer FROM-Klausel verwenden, die auf eine Tabelle in einer Datenbank verweisen, bei der ALLOW_SNAPSHOT_ISOLATION auf OFF festgelegt ist.

OFF     
Deaktiviert die Momentaufnahmeoption auf Datenbankebene. Transaktionen können die SNAPSHOT-Isolationsstufe für Transaktionen nicht angeben.

Wenn Sie ALLOW_SNAPSHOT_ISOLATION auf einen neuen Status festlegen (von ON zu OFF oder von OFF zu ON), gibt ALTER DATABASE die Kontrolle erst dann an den Aufrufer zurück, wenn ein Commit aller bestehenden Transaktionen in der Datenbank ausgeführt wurde. Hat die Datenbank bereits den in der ALTER DATABASE-Anweisung angegebenen Status, wird die Kontrolle direkt an den Aufrufer zurückgegeben. Wenn keine schnelle Rückgabe durch die ALTER DATABASE-Anweisung erfolgt, verwenden Sie [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md), um zu bestimmen, ob lang andauernde Transaktionen vorhanden sind. Wird die ALTER DATABASE-Anweisung abgebrochen, bleibt die Datenbank in dem Status, in dem sie sich vor dem Start von ALTER DATABASE befand. In der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht wird der Status von Isolationstransaktionen von Momentaufnahmen in der Datenbank angegeben. Ist **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON, wird ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF für 6 Sekunden angehalten und der Vorgang anschließend wiederholt.

Sie können den Status von ALLOW_SNAPSHOT_ISOLATION nicht ändern, wenn die Datenbank OFFLINE ist.

Wenn Sie ALLOW_SNAPSHOT_ISOLATION in einer READ_ONLY-Datenbank festlegen, wird die Einstellung gespeichert, wenn die Datenbank später auf READ_WRITE festgelegt wird.

Sie können die ALLOW_SNAPSHOT_ISOLATION-Einstellungen für die Datenbanken master, model, msdb und tempdb ändern. Wenn Sie die Einstellung für tempdb ändern, wird die Einstellung jedes Mal beibehalten, wenn die Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] beendet und neu gestartet wird. Wenn Sie die Einstellung für model ändern, wird die Einstellung zur Standardeinstellung für alle neu erstellten Datenbanken, mit Ausnahme von tempdb.

Die Option ist für die master- und msdb-Datenbanken standardmäßig auf ON eingestellt.

Die aktuelle Einstellung dieser Option kann mithilfe der Spalte `snapshot_isolation_state` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht ermittelt werden.

READ_COMMITTED_SNAPSHOT { ON | OFF }     
EIN     
Aktiviert die Option READ COMMITTED-Snapshot auf Datenbankebene. Wenn die Option aktiviert ist, beginnen DML-Anweisungen mit der Generierung von Zeilenversionen, auch wenn keine Transaktion die Momentaufnahmeisolation verwendet. Sobald diese Option aktiviert ist, verwenden Transaktionen, die die Read Committed-Isolationsstufe angeben, anstelle von Sperren die Zeilenversionsverwaltung. Wenn eine Transaktion auf der READ COMMITTED-Isolationsstufe ausgeführt wird, sehen alle Anweisungen eine Momentaufnahme der Daten, wie sie beim Start der Anweisung vorlagen.

OFF     
Deaktiviert die Option READ COMMITTED-Snapshot auf Datenbankebene. Transaktionen, die die READ COMMITTED-Isolationsstufe angeben, verwenden Sperren.

Wenn READ_COMMITTED_SNAPSHOT auf ON oder OFF festgelegt werden soll, dürfen außer der Verbindung, die den ALTER DATABASE-Befehl ausführt, keine aktiven Verbindungen zur Datenbank bestehen. Die Datenbank muss sich jedoch nicht im Einzelbenutzermodus befinden. Sie können den Status dieser Option nicht ändern, wenn die Datenbank OFFLINE ist.

Wenn Sie READ_COMMITTED_SNAPSHOT in einer READ_ONLY-Datenbank festlegen, wird die Einstellung beibehalten, wenn die Datenbank später auf READ_WRITE festgelegt wird.

READ_COMMITTED_SNAPSHOT kann für die Systemdatenbanken master, tempdb oder msdb nicht auf ON festgelegt werden. Wenn Sie die Einstellung für model ändern, wird die Einstellung zur Standardeinstellung für alle neu erstellten Datenbanken, mit Ausnahme von tempdb.

Die aktuelle Einstellung dieser Option kann mithilfe der Spalte `is_read_committed_snapshot_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht ermittelt werden.

> [!WARNING]
> Wenn eine Tabelle mit **DURABILITY = SCHEMA_ONLY** erstellt wird und **READ_COMMITTED_SNAPSHOT** anschließend mithilfe von **ALTER DATABASE** geändert wird, gehen Daten in der Tabelle verloren.

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | **OFF** }     
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)])

EIN     
Wenn die Isolationsstufe für Transaktionen auf eine niedrigere Isolationsstufe als SNAPSHOT festgelegt wird, werden alle interpretierten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Vorgänge für speicheroptimierte Tabelle unter der Isolationsstufe SNAPSHOT ausgeführt. Beispiele für Isolationsstufen, die niedriger als SNAPSHOT sind, sind READ COMMITTED oder READ UNCOMMITTED. Diese Vorgänge erfolgen ungeachtet des Umstands, ob die Transaktionsisolationsstufe explizit auf der Sitzungsebene festgelegt ist, oder ob implizit die Standardeinstellung verwendet wird.

OFF     
Erhöht nicht die Isolationsstufe für Transaktionen für interpretierte [!INCLUDE[tsql](../../includes/tsql-md.md)]-Vorgänge für speicheroptimierte Tabellen.

Sie können den Status von MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT nicht ändern, wenn die Datenbank OFFLINE ist.

Die Standardoption ist OFF.

Die aktuelle Einstellung dieser Option kann mithilfe der Spalte `is_memory_optimized_elevate_to_snapshot_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht ermittelt werden.

**\<sql_option> ::=**      
Steuert die ANSI-Kompatibilitätsoptionen auf der Datenbankebene.

ANSI_NULL_DEFAULT { ON | **OFF** }     
Legt den Standardwert (NULL oder NOT NULL) einer Spalte oder [CLR user-defined type](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) fest, für den die NULL-Zulässigkeit nicht explizit in den CREATE TABLE- oder ALTER TABLE-Anweisungen festgelegt wurde. Spalten, die mit Einschränkungen definiert werden, folgen den Einschränkungsregeln, egal wie diese Einstellung lautet.

EIN     
Der Standardwert für eine nicht definierte Spalte ist NULL.

OFF     
Der Standardwert für eine nicht definierte Spalte ist NOT NULL.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für ANSI_NULL_DEFAULT. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die ANSI_NULL_DEFAULT für die Sitzung auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).

Für die ANSI-Kompatibilität wird durch Festlegen der Datenbankoption ANSI_NULL_DEFAULT auf ON der Datenbankstandardwert auf NULL geändert.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_ansi_null_default_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAnsiNullDefault`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

ANSI_NULLS { ON | **OFF** }     
EIN     
Alle Vergleiche mit einem Nullwert ergeben UNKNOWN.

OFF     
Vergleiche von Nicht-UNICODE-Werten mit einem Nullwert ergeben TRUE, wenn beide Werte NULL sind.

> [!IMPORTANT]
> In einer späteren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird ANSI_NULLS immer auf ON festgelegt, und jede Anwendung, für die die Option explizit auf OFF festgelegt wird, löst einen Fehler aus. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für ANSI_NULLS. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die ANSI_NULLS für die Sitzung auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md).

> [!IMPORTANT]
> SET ANSI_NULLS muss ebenfalls auf ON festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_ansi_nulls_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAnsiNullsEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

ANSI_PADDING { ON | **OFF** }     
EIN     
Zeichenfolgen werden vor der Konvertierung auf dieselbe Länge aufgefüllt. Sie werden auch vor dem Einfügen in einen **varchar**- oder **nvarchar**-Datentyp durch Einfügen von Leerstellen auf dieselbe Länge gebracht.

OFF     
Fügt nachfolgende Leerräume in Zeichenwerte in **varchar** oder **nvarchar**-Spalten ein. Belässt außerdem nachfolgende Nullen in Binärwerten, die in **varbinary**-Spalten eingefügt werden. Werte werden nicht bis zur Spaltenlänge aufgefüllt.

Ist OFF festgelegt, wirkt sich diese Einstellung nur auf die Definition neuer Spalten aus.

> [!IMPORTANT]
> In einer späteren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird ANSI_PADDING immer auf ON festgelegt, und jede Anwendung, für die die Option explizit auf OFF festgelegt ist, löst einen Fehler aus. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Es wird empfohlen, für ANSI_PADDING stets den Wert ON festzulegen. ANSI_PADDING muss beim Erstellen oder Bearbeiten von Indizes für berechnete Spalten oder indizierte Sichten auf ON festgelegt sein.

**char(_n_)** - und **binary(_n_)** -Spalten, die NULL-Werte zulassen, werden bis zur Spaltenlänge aufgefüllt, wenn ANSI_PADDING auf ON festgelegt ist. Ist ANSI_PADDING hingegen auf OFF festgelegt, werden nachfolgende Leerzeichen und Nullen abgeschnitten. **char(_n_)** - und **binary(_n_)** -Spalten, die keine NULL-Werte zulassen, werden immer bis zur Spaltenlänge aufgefüllt.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für ANSI_PADDING. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die ANSI_PADDING für die Sitzung auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md).

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_ansi_padding_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAnsiPaddingEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

ANSI_WARNINGS { ON | **OFF** }     
EIN     
Fehler und Warnungen werden ausgegeben, wenn z.B. ein Fehler wegen „Division durch Null“ auftritt. Fehler oder Warnungen werden ebenfalls ausgegeben, wenn Nullwerte in Aggregatfunktionen auftreten.

OFF     
Bei Bedingungen wie einer Division durch Null werden keine Warnungen ausgegeben, und Nullwerte werden zurückgegeben.

> [!IMPORTANT]
> SET ANSI_WARNINGS muss auf ON festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für ANSI_WARNINGS. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die ANSI_WARNINGS für die Sitzung auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md).

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_ansi_warnings_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAnsiWarningsEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

ARITHABORT { ON | OFF }     
EIN     
Eine Abfrage wird beendet, wenn während der Abfrage ein Überlauffehler oder ein Fehler aufgrund einer Division durch Null auftritt.

OFF     
Eine Warnmeldung wird angezeigt, wenn einer dieser Fehler auftritt. Die Verarbeitung der Abfrage, des Batches oder der Transaktion wird fortgesetzt, als wäre kein Fehler aufgetreten, selbst wenn eine Warnung angezeigt wird.

> [!IMPORTANT]
> SET ARITHABORT muss auf ON festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_arithabort_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsArithmeticAbortEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }     

Weitere Informationen finden Sie unter [ALTER DATABASE-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

CONCAT_NULL_YIELDS_NULL { ON | **OFF** }     
EIN     
Das Ergebnis einer Verkettungsoperation ist NULL, wenn einer der Operanden NULL ist. Wenn z. B. die Zeichenfolge „This is“ und NULL verkettet werden und NULL den NULL-Wert anstelle des „This is“-Werts zurückgibt.

OFF     
Der Nullwert wird als leere Zeichenfolge behandelt.

> [WICHTIG] CONCAT_NULL_YIELDS_NULL muss auf ON festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.
>
> In zukünftigen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird CONCAT_NULL_YIELDS_NULL immer auf ON festgelegt, und jede Anwendung, die für die Option explizit OFF festlegt, löst einen Fehler aus. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für CONCAT_NULL_YIELDS_NULL. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die CONCAT_NULL_YIELDS_NULL für die Sitzung auf ON festgelegt wird, wenn sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_concat_null_yields_null_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsNullConcat`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

QUOTED_IDENTIFIER { ON | OFF }     
EIN     
Doppelte Anführungszeichen können nur zum Einschließen von Begrenzungsbezeichnern verwendet werden.

Alle Zeichenfolgen, die durch doppelte Anführungszeichen begrenzt werden, werden als Objektbezeichner interpretiert. Bezeichner in Anführungszeichen müssen nicht den [!INCLUDE[tsql](../../includes/tsql-md.md)]-Regeln für Bezeichner entsprechen. Sie können Schlüsselwörter darstellen und Zeichen einschließen, die in [!INCLUDE[tsql](../../includes/tsql-md.md)]-Bezeichnern nicht zulässig sind. Ein einfaches Anführungszeichen (’), das zur Literalzeichenfolge selbst gehört, kann durch doppelte Anführungszeichen (’’) dargestellt werden.

OFF     
Bezeichner dürfen nicht in Anführungszeichen eingeschlossen werden und müssen allen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Regeln für Bezeichner entsprechen. Literale können in einfache oder doppelte Anführungszeichen eingeschlossen werden.

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist es zudem möglich, Bezeichner durch eckige Klammern ([ ]) zu begrenzen. Bezeichner in eckigen Klammern können immer verwendet werden, egal wie die Einstellung für QUOTED_IDENTIFIER lautet. Weitere Informationen finden Sie unter [Datenbankbezeichner](../../relational-databases/databases/database-identifiers.md).

Beim Erstellen einer Tabelle wird die Option QUOTED IDENTIFIER immer als ON in den Metadaten der Tabelle gespeichert. Die Option wird gespeichert, selbst wenn die Option beim Erstellen der Tabelle auf OFF festgelegt ist.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für QUOTED_IDENTIFIER. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die QUOTED_IDENTIFIER auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_quoted_identifier_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsQuotedIdentifiersEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

NUMERIC_ROUNDABORT { ON | OFF }     
EIN     
Es wird ein Fehler generiert, wenn ein Genauigkeitsverlust in einem Ausdruck auftritt.

OFF     
Bei einem Genauigkeitsverlust wird keine Fehlermeldung generiert, und das Ergebnis wird auf die Genauigkeit der Spalte oder Variablen gerundet, die das Ergebnis speichert.

> [!IMPORTANT]
> NUMERIC_ROUNDABORT muss auf OFF festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_numeric_roundabort_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsNumericRoundAbortEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

RECURSIVE_TRIGGERS { ON | OFF }     
EIN     
Das rekursive Auslösen von AFTER-Triggern ist zugelassen.

OFF     
Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_recursive_triggers_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsRecursiveTriggersEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

> [!NOTE]
> Nur die direkte Rekursion wird verhindert, wenn RECURSIVE_TRIGGERS auf OFF festgelegt ist. Sie müssen auch die Geschachtelte Trigger-Serveroption auf 0 festlegen, um die indirekte Rekursion zu deaktivieren.

Sie können den Status dieser Option mithilfe der Spalte `is_recursive_triggers_on` in der Katalogsicht [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) oder der `IsRecursiveTriggersEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion ermitteln.

**\<target_recovery_time_option> ::=**      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Gibt die Frequenz indirekter Prüfpunkte auf Basis einzelner Datenbanken an. Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] beträgt der Standardwert für neue Datenbanken, der darauf hindeutet, dass Datenbanken indirekte Prüfpunkte verwendet werden, **eine Minute**. Der Standard für ältere Versionen ist 0 (null) und gibt an, dass die Datenbank automatische Prüfpunkte verwendet, deren Frequenz von der Einstellung für das Wiederherstellungsintervall der Serverinstanz abhängt. Für [!INCLUDE[msCoName](../../includes/msconame-md.md)] ist für die meisten Systeme eine Minute empfohlen.

TARGET_RECOVERY_TIME **=** *target_recovery_time* { SECONDS | MINUTES }     
*target_recovery_time*     
Gibt die maximale Grenze für die Zeit an, die für die Wiederherstellung der angegebenen Datenbank im Fall eines Fehlers aufgewendet wird. *target_recovery_time* ist vom Typ **int**.

SECONDS     
Gibt an, dass *target_recovery_time* die Anzahl von Sekunden darstellt.

MINUTES     
Gibt an, dass *target_recovery_time* die Anzahl von Minuten darstellt.

Weitere Informationen zu indirekten Prüfpunkten finden Sie unter [Datenbankprüfpunkte](../../relational-databases/logs/database-checkpoints-sql-server.md).

**WITH \<termination> ::=**      
Gibt an, wann beim Übergang der Datenbank von einem Status in einen anderen für unvollständige Transaktionen ein Rollback ausgeführt werden soll. Wird die Beendigungsklausel ausgelassen, wartet die ALTER DATABASE-Anweisung auf unbestimmte Zeit, wenn keine Sperre für die Datenbank besteht. Es kann nur eine Beendigungsklausel angegeben werden, und diese steht hinter den SET-Klauseln.

> [!NOTE]
> Nicht alle Datenbankoptionen verwenden die WITH \<termination>-Klausel. Weitere Informationen finden Sie in der Tabelle unter [Festlegen von Optionen](#SettingOptions) im Abschnitt „Hinweise“ in diesem Artikel.

ROLLBACK AFTER *number* [SECONDS] | ROLLBACK IMMEDIATE     
Gibt an, ob ein Rollback sofort oder nach Ablauf der angegebenen Sekundenzahl ausgeführt werden soll. *number* ist vom Datentyp **int**.

NO_WAIT     
Gibt an, dass die Anforderung fehlschlägt, wenn diese Änderung des Datenbankstatus oder der Datenbankoption nicht sofort vollständig vorgenommen werden kann. Der sofortige Abschluss des Vorgangs bedeutet, dass nicht darauf gewartet wird, dass Transaktionen eigenständig einen Commit oder Rollback ausführen.

## <a name="setting-options"></a><a name="SettingOptions"></a> Festlegen von Optionen
Verwenden Sie die [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht oder [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md), um die aktuellen Einstellungen für Datenbankoptionen abzurufen.

Wenn Sie eine Datenbankoption festlegen, tritt die neue Einstellung sofort in Kraft.

Sie können die Standardwerte einer Datenbankoption für alle neu erstellten Datenbanken ändern. Hierzu ändern Sie die entsprechende Datenbankoption in der Modelldatenbank.

Nicht alle Datenbankoptionen verwenden die WITH \<termination>-Klausel oder können zusammen mit anderen Optionen festgelegt werden. In der folgenden Tabelle sind die Optionen und ihr Options- und Beendigungsstatus aufgeführt.

|Optionskategorie|Kann mit anderen Optionen angegeben werden|Kann WITH \<termination>-Klausel verwenden|
|----------------------|-----------------------------------------|---------------------------------------------|
|\<db_state_option>|Ja|Ja|
|\<db_user_access_option>|Ja|Ja|
|\<db_update_option>|Ja|Ja|
|\<delayed_durability_option>|Ja|Ja|
|\<external_access_option>|Ja|Nein|
|\<cursor_option>|Ja|Nein|
|\<auto_option>|Ja|Nein|
|\<sql_option>|Ja|Nein|
|\<recovery_option>|Ja|Nein|
|\<target_recovery_time_option>|Nein|Ja|
|\<database_mirroring_option>|Nein|Nein|
|ALLOW_SNAPSHOT_ISOLATION|Nein|Nein|
|READ_COMMITTED_SNAPSHOT|Nein|Ja|
|MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT|Ja|Ja|
|\<service_broker_option>|Ja|Nein|
|DATE_CORRELATION_OPTIMIZATION|Ja|Ja|
|\<parameterization_option>|Ja|Ja|
|\<change_tracking_option>|Ja|Ja|
|\<db_encryption_option>|Ja|Nein|

Der Plancache für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird gelöscht, indem eine der folgenden Optionen festgelegt wird:

:::row:::
    :::column:::
        OFFLINE
    :::column-end:::
    :::column:::
        ONLINE
    :::column-end:::
    :::column:::
        MODIFY_NAME
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        COLLATE
    :::column-end:::
    :::column:::
        READ_ONLY
    :::column-end:::
    :::column:::
        READ_WRITE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        MODIFY FILEGROUP DEFAULT
    :::column-end:::
    :::column:::
        MODIFY FILEGROUP READ_WRITE
    :::column-end:::
    :::column:::
        MODIFY FILEGROUP READ_ONLY
    :::column-end:::
:::row-end:::

Der Plancache wird auch in den folgenden Szenarios geleert.

- Die AUTO_CLOSE-Datenbankoption ist für eine Datenbank auf ON festgelegt. Wenn die Datenbank von keiner Benutzerverbindung verwendet wird bzw. keine Benutzerverbindung darauf verweist, versucht der Hintergrundtask, die Datenbank automatisch zu schließen und herunterzufahren.
- Sie führen mehrere Abfragen für eine Datenbank aus, die über Standardoptionen verfügt. Anschließend wird die Datenbank gelöscht.
- Eine Datenbank-Momentaufnahme für eine Quelldatenbank wird gelöscht.
- Sie erstellen das Transaktionsprotokoll für eine Datenbank erfolgreich neu.
- Sie stellen eine Datenbanksicherung wieder her.
- Sie trennen eine Datenbank.

Durch das Löschen des Plancaches wird eine Neukompilierung aller nachfolgenden Ausführungspläne verursacht, und möglicherweise entsteht plötzlich eine temporäre Verringerung der Abfrageleistung. Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll enthält für jeden geleerten Cachespeicher im Plancache folgende Infonachricht: `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`. Diese Meldung wird alle fünf Minuten protokolliert, solange der Cache innerhalb dieses Zeitintervalls geleert wird.

## <a name="examples"></a>Beispiele

### <a name="a-setting-options-on-a-database"></a>A. Festlegen von Optionen für eine Datenbank

Im folgenden Beispiel werden die Optionen für das Wiederherstellungsmodell und die Datenseitenüberprüfung für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Beispieldatenbank festgelegt.

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RECOVERY FULL PAGE_VERIFY CHECKSUM;
GO

```

### <a name="b-setting-the-database-to-read_only"></a>B. Festlegen der Datenbank auf READ_ONLY

Für die Änderung des Status einer Datenbank oder Dateigruppe in READ_ONLY oder READ_WRITE wird der exklusive Zugriff auf die Datenbank benötigt. Im folgenden Beispiel wird die Datenbank auf den `SINGLE_USER`-Modus festgelegt, um exklusiven Zugriff zu erhalten. Anschließend wird in dem Beispiel der Status der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank auf `READ_ONLY` festgelegt und der Zugriff auf die Datenbank an alle Benutzer zurückgegeben.

> [!NOTE]
> In diesem Beispiel wird die Beendigungsoption `WITH ROLLBACK IMMEDIATE` in der ersten `ALTER DATABASE`-Anweisung verwendet. Für alle unvollständigen Transaktionen wird ein Rollback ausgeführt, und alle anderen Verbindungen zur [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank werden sofort getrennt.

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET SINGLE_USER
WITH ROLLBACK IMMEDIATE;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO
```

### <a name="c-enabling-snapshot-isolation-on-a-database"></a>C. Aktivieren der Momentaufnahmeisolation für eine Datenbank

Im folgenden Beispiel wird die Option für das Momentaufnahmeisolations-Framework für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank aktiviert.

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO

```

Das Resultset zeigt, dass das Framework für die Momentaufnahmeisolation aktiviert ist.

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[database_name] |1| EIN |

### <a name="d-enabling-modifying-and-disabling-change-tracking"></a>D: Aktivieren, Ändern und Deaktivieren der Änderungsnachverfolgung

Im folgenden Beispiel wird die Änderungsnachverfolgung für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank aktiviert und die Aufbewahrungsdauer auf `2` Tage festgelegt.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

Das folgende Beispiel veranschaulicht, wie die Beibehaltungsdauer in `3` Tage geändert wird.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

Das folgende Beispiel veranschaulicht, wie die Änderungsnachverfolgung für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank deaktiviert wird.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="e-enabling-the-query-store"></a>E. Aktivieren des Abfragespeichers

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])

Im folgenden Beispiel werden der Abfragespeicher aktiviert und dessen Parameter konfiguriert.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="f-enabling-the-query-store-with-wait-statistics"></a>F. Aktivieren des Abfragespeichers mit Wartestatistiken

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])

Im folgenden Beispiel werden der Abfragespeicher aktiviert und dessen Parameter konfiguriert.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="g-enabling-the-query-store-with-custom-capture-policy-options"></a>G. Aktivieren des Abfragespeichers mit benutzerdefinierten Erfassungsrichtlinienoptionen

**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Im folgenden Beispiel werden der Abfragespeicher aktiviert und dessen Parameter konfiguriert.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="see-also"></a>Weitere Informationen

- [ALTER DATABASE-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [ALTER DATABASE-Datenbankspiegelung](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)
- [Statistik](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [Aktivieren und Deaktivieren der Änderungsnachverfolgung](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Bewährte Methoden für den Abfragespeicher](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL-Datenbank \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL-Datenbank

Kompatibilitätsgrade sind `SET`-Optionen, die jedoch unter [ALTER DATABASE-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) beschrieben werden.

> [!NOTE]
> Viele DATABASE SET-Optionen können mithilfe von [SET-Anweisungen](../../t-sql/statements/set-statements-transact-sql.md) konfiguriert werden; häufig werden sie bei der Verbindung von Anwendungen konfiguriert. Die **ALTER DATABASE SET**-Werte werden durch SET-Optionen auf Sitzungsebene überschrieben. Die unten beschriebenen Datenbankoptionen entsprechen Werten, die für Sitzungen festgelegt werden können, von denen explizit keine weiteren Werte für SET-Optionen bereitgestellt werden.

## <a name="syntax"></a>Syntax

```syntaxsql
ALTER DATABASE { database_name | Current }
SET
{
    <option_spec> [ ,...n ] [ WITH <termination> ]
}
;

<option_spec> ::=
{
    <auto_option>
  | <automatic_tuning_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
}
;

<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING = { AUTO | INHERIT | CUSTOM }
  | AUTOMATIC_TUNING ( CREATE_INDEX = { DEFAULT | ON | OFF } )
  | AUTOMATIC_TUNING ( DROP_INDEX = { DEFAULT | ON | OFF } )
  | AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { DEFAULT | ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
    {
        = OFF
      | = ON [ ( <change_tracking_option_list > [,...n] ) ]
      | ( <change_tracking_option_list> [,...n] )
    }
}

<change_tracking_option_list> ::=
   {
       AUTO_CLEANUP = { ON | OFF }
     | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
   }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
}

<db_encryption_option> ::=
  ENCRYPTION { ON | OFF }

<db_update_option> ::=
  { READ_ONLY | READ_WRITE }

<db_user_access_option> ::=
  { RESTRICTED_USER | MULTI_USER }

<delayed_durability_option> ::= DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<parameterization_option> ::=
  PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
  QUERY_STORE
  {
      = OFF
    | = ON [ ( <query_store_option_list> [,... n] ) ]
    | ( < query_store_option_list> [,... n] )
    | CLEAR [ ALL ]
  }
}

<query_store_option_list> ::=
{
  OPERATION_MODE = { READ_WRITE | READ_ONLY }
  | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
  | DATA_FLUSH_INTERVAL_SECONDS = number
  | MAX_STORAGE_SIZE_MB = number
  | INTERVAL_LENGTH_MINUTES = number
  | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
  | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
  | MAX_PLANS_PER_QUERY = number
  | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
  | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
    STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT {ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT {ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<termination>::=
{
    ROLLBACK AFTER integer [ SECONDS ]
  | ROLLBACK IMMEDIATE
  | NO_WAIT
}

<temporal_history_retention>::=TEMPORAL_HISTORY_RETENTION { ON | OFF }
```

## <a name="arguments"></a>Argumente

*database_name*     
Der Name der Datenbank, die geändert werden soll.

CURRENT     
`CURRENT` führt die Aktion in der aktuellen Datenbank aus. `CURRENT` wird nicht in allen Kontexten für alle Optionen unterstützt. Wenn `CURRENT` einen Fehler verursacht, geben Sie den Datenbanknamen an.

**\<auto_option> ::=**      
Steuert automatische Optionen.

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { ON | OFF }     
EIN     
Der Abfrageoptimierer erstellt nach Bedarf Statistiken für einzelne Spalten in Abfrageprädikaten, um Abfragepläne sowie die Abfrageleistung zu verbessern. Diese Statistiken für einzelne Spalten werden erstellt, wenn der Abfrageoptimierer Abfragen kompiliert. Die Statistiken für einzelne Spalten werden nur für Spalten erstellt, die noch nicht der ersten Spalte eines vorhandenen Statistikobjekts entsprechen.

Der Standardwert ist ON. Für die meisten Datenbanken empfiehlt sich die Verwendung der Standardeinstellung.

OFF     
Der Abfrageoptimierer erstellt beim Kompilieren von Abfragen keine Statistiken für einzelne Spalten in Abfrageprädikaten. Das Festlegen dieser Option auf OFF kann zu suboptimalen Abfrageplänen und einer beeinträchtigten Abfrageleistung führen.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_auto_create_stats_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAutoCreateStatistics`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

Weitere Informationen finden Sie im Abschnitt „Statistikoptionen“ unter [Statistiken](../../relational-databases/statistics/statistics.md#statistics-options).

INCREMENTAL = ON | **OFF**     
Legen Sie AUTO_CREATE_STATISTICS auf ON und INCREMENTAL auf ON fest. Diese Einstellung erstellt automatisch Statistiken als inkrementell, wann immer inkrementelle Statistiken unterstützt werden. Der Standardwert ist OFF. Weitere Informationen finden Sie unter [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).

<a name="auto_shrink"></a> AUTO_SHRINK { ON | **OFF** }     
EIN     
Die Datenbankdateien sind Kandidaten für das periodische Verkleinern.

Sowohl Daten- als auch Protokolldateien können automatisch verkleinert werden. AUTO_SHRINK reduziert die Größe des Transaktionsprotokolls nur, wenn Sie die Datenbank auf das SIMPLE-Wiederherstellungsmodell festlegen oder das Protokoll sichern. Ist diese Option auf OFF festgelegt, werden die Datenbankdateien während der periodisch ausgeführten Überprüfung auf nicht verwendeten Speicherplatz nicht automatisch verkleinert.

Durch die Option AUTO_SHRINK werden Dateien dann verkleinert, wenn mehr als 25 Prozent der Datei aus nicht verwendetem Speicherplatz bestehen. Die Option bewirkt, dass die Datei, auf eine von zwei Größen verkleinert wird. Sie wird auf den jeweils größeren Wert verkleinert:

- die Größe, bei der 25 Prozent der Datei aus nicht verwendetem Speicherplatz bestehen
- die Größe der Datei, als sie erstellt wurde

Eine schreibgeschützte Datenbank kann nicht verkleinert werden.

OFF     
Die Datenbankdateien werden bei periodischen Prüfungen auf nicht verwendeten Speicherplatz nicht automatisch verkleinert.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_auto_shrink_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAutoShrink`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

> [!NOTE]
> Die AUTO_SHRINK-Option ist in einer eigenständigen Datenbank nicht verfügbar.

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { **ON** | OFF }     
EIN     
Gibt an, dass der Abfrageoptimierer Statistiken aktualisiert, wenn sie von einer Abfrage verwendet werden und veraltet sein könnten. Statistiken sind veraltet, wenn die Datenverteilung in der Tabelle oder indizierten Sicht durch die Vorgänge INSERT, UPDATE, DELETE oder MERGE geändert wurde. Der Abfrageoptimierer stellt fest, wann Statistiken veraltet sein könnten, indem er die Anzahl von Datenänderungen seit der letzten Statistikaktualisierung ermittelt und sie mit einem Schwellenwert vergleicht. Der Schwellenwert basiert auf der Anzahl von Zeilen in der Tabelle oder indizierten Sicht.

Bevor der Abfrageoptimierer eine Abfrage kompiliert und einen zwischengespeicherten Abfrageplan ausführt, sucht er nach veralteten Statistiken. Der Abfrageoptimierer ermittelt anhand der Spalten, Tabellen und indizierten Sichten im Abfrageprädikat, welche Statistiken veraltet sein könnten. Der Abfrageoptimierer ermittelt diese Informationen, bevor er eine Abfrage kompiliert. Vor dem Ausführen eines zwischengespeicherten Abfrageplans stellt die [!INCLUDE[ssDE](../../includes/ssde-md.md)] sicher, dass der Abfrageplan auf aktuelle Statistiken verweist.

Die AUTO_UPDATE_STATISTICS-Option gilt für Statistikobjekte, die für Indizes, einzelne Spalten in Abfrageprädikaten und mit der CREATE STATISTICS-Anweisung generierte Statistiken erstellt wurden. Diese Option gilt auch für gefilterte Statistiken.

Der Standardwert ist ON. Für die meisten Datenbanken empfiehlt sich die Verwendung der Standardeinstellung.

Verwenden Sie die AUTO_UPDATE_STATISTICS_ASYNC-Option, um anzugeben, ob die Statistiken synchron oder asynchron aktualisiert werden.

OFF     
Gibt an, dass der Abfrageoptimierer Statistiken nicht aktualisiert, wenn sie von einer Abfrage verwendet werden. Der Abfrageoptimierer aktualisiert Statistiken auch nicht, wenn sie veraltet sein könnten. Das Festlegen dieser Option auf OFF kann zu suboptimalen Abfrageplänen und einer beeinträchtigten Abfrageleistung führen.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_auto_update_stats_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAutoUpdateStatistics`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

Weitere Informationen finden Sie im Abschnitt „Statistikoptionen“ unter [Statistiken](../../relational-databases/statistics/statistics.md#statistics-options).

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | **OFF** }     
EIN     
Gibt an, dass Statistikupdates für die AUTO_UPDATE_STATISTICS-Option asynchron sind. Der Abfrageoptimierer wartet nicht, bis Statistikaktualisierungen abgeschlossen sind, bevor Abfragen kompiliert werden.

Das Festlegen dieser Option auf ON hat nur dann Auswirkungen, wenn AUTO_UPDATE_STATISTICS auf ON festgelegt ist.

Die AUTO_UPDATE_STATISTICS_ASYNC-Option ist standardmäßig auf OFF festgelegt, sodass der Abfrageoptimierer Statistiken synchron aktualisiert.

OFF     
Gibt an, dass Statistikupdates für die AUTO_UPDATE_STATISTICS-Option synchron sind. Der Abfrageoptimierer wartet, bis Statistikupdates abgeschlossen sind, bevor Abfragen kompiliert werden.

Das Festlegen dieser Option auf OFF hat nur dann Auswirkungen, wenn AUTO_UPDATE_STATISTICS auf ON festgelegt ist.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_auto_update_stats_async_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen.

Weitere Informationen dazu, wann synchrone bzw. asynchrone Statistikupdates verwendet werden sollten, finden Sie im Abschnitt „Statistikoptionen“ unter [Statistiken](../../relational-databases/statistics/statistics.md#statistics-options).

<a name="auto_tuning"></a> **\<automatic_tuning_option> ::=**      
**Gilt für:** [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]

Steuert automatische Optionen für die [automatische Optimierung](../../relational-databases/automatic-tuning/automatic-tuning.md).

AUTOMATIC_TUNING = { AUTO | INHERIT | CUSTOM }     
AUTO     
Durch Festlegen des Werts AUTOMATIC_TUNING auf AUTO werden die Azure-Konfigurationsstandards für die automatische Optimierung festgelegt.

INHERIT     
Durch das Verwenden des Werts INHERIT wird die Standardkonfiguration vom übergeordneten Server geerbt. Dies ist besonders nützlich, wenn Sie die Konfiguration für die automatische Optimierung auf einem übergeordneten Server anpassen möchten und alle Datenbanken auf dem Server diese benutzerdefinierten Einstellung erben sollen. Beachten Sie, dass die drei Optimierungsoptionen FORCE_LAST_GOOD_PLAN, CREATE_INDEX und DROP_INDEX auf diesen Datenbanken auf DEFAULT festgelegt sein müssen, damit die Vererbung funktioniert.

CUSTOM     
Durch das Verwenden des CUSTOM-Werts müssen Sie alle in den Datenbanken verfügbaren Optionen für die automatische Optimierung benutzerdefiniert konfigurieren.

Aktiviert oder deaktiviert die automatische Indexverwaltungsoption `CREATE_INDEX` der [automatischen Optimierung](../../relational-databases/automatic-tuning/automatic-tuning.md).

CREATE_INDEX = { DEFAULT | ON | OFF }     
DEFAULT     
Standardeinstellungen werden vom Server geerbt. In diesem Fall werden die Optionen für das Aktivieren oder Deaktivieren der einzelnen Features für die automatische Optimierung auf Serverebene definiert.

EIN     
Wenn diese Option aktiviert ist, werden fehlende Indizes automatisch für eine Datenbank generiert. Nach der Indexerstellung wird überprüft, ob sich die Leistung der Workload verbessert hat. Wenn ein erstellter Index die Workloadleistung nicht mehr verbessert, wird er automatisch zurückgesetzt. Automatische erstellte Indizes werden als systemgenerierte Indizes gekennzeichnet.

OFF     
Fehlende Indizes werden nicht automatisch in der Datenbank generiert.

Aktiviert oder deaktiviert die automatische Indexverwaltungsoption `DROP_INDEX` der [automatischen Optimierung](../../relational-databases/automatic-tuning/automatic-tuning.md).

DROP_INDEX = { DEFAULT | ON | OFF }     
DEFAULT     
Standardeinstellungen werden vom Server geerbt. In diesem Fall werden die Optionen für das Aktivieren oder Deaktivieren der einzelnen Features für die automatische Optimierung auf Serverebene definiert.

EIN     
Indizes, die doppelt vorhanden sind oder nicht mehr für die Workloadleistung benötigt werden, werden automatisch gelöscht.

OFF     
Fehlende Indizes werden nicht automatisch für die Datenbank gelöscht.

Aktiviert oder deaktiviert die automatische Plankorrekturoption `FORCE_LAST_GOOD_PLAN` der [automatischen Optimierung](../../relational-databases/automatic-tuning/automatic-tuning.md).

FORCE_LAST_GOOD_PLAN = { DEFAULT | ON | OFF }     
DEFAULT     
Standardeinstellungen werden vom Server geerbt. In diesem Fall werden die Optionen für das Aktivieren oder Deaktivieren der einzelnen Features für die automatische Optimierung auf Serverebene definiert.

EIN     
Die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] erzwingt automatisch den neusten bekannten, geeigneten Plan bei [!INCLUDE[tsql-md](../../includes/tsql-md.md)]-Abfragen, bei denen neue Abfragepläne negative Auswirkungen auf die Leistung haben. Die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] überwacht regelmäßig die Abfrageleistung der [!INCLUDE[tsql-md](../../includes/tsql-md.md)]-Abfrage mit dem erzwungenen Plan. Wenn die Leistung verbessert wurde, verwendet die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] weiterhin den neusten bekannten, geeigneten Plan. Wenn die Leistung nicht verbessert wurde, erstellt die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] einen neuen Abfrageplan. Bei der Anweisung tritt ein Fehler auf, wenn der Abfragespeicher nicht aktiviert ist oder sich nicht im *Lese-/Schreibmodus* befindet.

OFF     
Die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] meldet potentielle Einbußen im Hinblick auf die Abfrageleistung, die von Änderungen des Abfrageplans in der [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)-Sicht hervorgerufen werden könnten. Diese Empfehlungen werden allerdings nicht automatisch angewendet. Benutzer können aktive Empfehlungen überwachen und ermittelte Probleme beheben, indem sie die in der Sicht aufgeführten [!INCLUDE[tsql-md](../../includes/tsql-md.md)]-Skripts anwenden. Dies ist der Standardwert.

**\<change_tracking_option> ::=**      
Steuert Änderungsnachverfolgungsoptionen. Sie können die Änderungsnachverfolgung aktivieren, Optionen festlegen, Optionen ändern und die Änderungsnachverfolgung deaktivieren. Beispiele hierzu finden Sie im Abschnitt „Beispiele“ weiter unten in diesem Artikel.

EIN     
Aktiviert die Änderungsnachverfolgung für die Datenbank. Wenn die Änderungsnachverfolgung aktiviert wird, können auch die AUTO CLEANUP-Option und die CHANGE RETENTION-Option festgelegt werden.

AUTO_CLEANUP = { ON | OFF }     
EIN     
Die Änderungsnachverfolgungsdaten werden nach der angegebenen Beibehaltungsdauer automatisch entfernt.

OFF     
Die Änderungsnachverfolgungsdaten werden nicht aus der Datenbank entfernt.

CHANGE_RETENTION = *retention_period* { **DAYS** | HOURS | MINUTES }     
Gibt die Mindestdauer für die Beibehaltung von Änderungsnachverfolgungsdaten in der Datenbank an. Die Daten werden nur dann entfernt, wenn der Wert für AUTO_CLEANUP ON lautet.

*retention_period* ist ein Integer, der die numerische Komponente der Vermerkdauer angibt.

Die Standardbeibehaltungsdauer beträgt **2 Tage**. Die Mindestbeibehaltungsdauer ist 1 Minute. Der Standardtyp für die Beibehaltung ist **DAYS**.

OFF     
Deaktiviert die Änderungsnachverfolgung für die Datenbank. Deaktivieren Sie erst die Änderungsnachverfolgung für alle Tabellen, bevor Sie sie für die Datenbank deaktivieren.

**\<cursor_option> ::=**      
Steuert Cursoroptionen.

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
EIN     
Alle beim Commit oder Rollback einer Transaktion geöffneten Cursor werden geschlossen.

OFF     
Cursor bleiben beim Commit einer Transaktion geöffnet. Beim Rollback einer Transaktion werden alle Cursor geschlossen, sofern sie nicht als INSENSITIVE oder STATIC definiert sind.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für CURSOR_CLOSE_ON_COMMIT. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die CURSOR_CLOSE_ON_COMMIT für die Sitzung auf OFF festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).

Sie können den Status dieser Option mithilfe der Spalte `is_cursor_close_on_commit_on` in der Katalogsicht [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) oder der `IsCloseCursorsOnCommitEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion ermitteln. Die Zuordnung des Cursors wird implizit nur aufgehoben, wenn die Verbindung getrennt wird. Weitere Informationen finden Sie unter [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md).

**\<db_encryption_option> ::=**      
Steuert den Status der Datenbankverschlüsselung.

ENCRYPTION { ON | OFF }     
Legt fest, ob die Datenbank verschlüsselt (ON) oder nicht verschlüsselt (OFF) werden soll. Weitere Informationen finden Sie unter [Transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md) und [Transparent Data Encryption in Azure SQL-Datenbank](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

Wenn die Verschlüsselung auf Datenbankebene aktiviert wird, werden alle Dateigruppen verschlüsselt. Alle neuen Dateigruppen erben die verschlüsselte Eigenschaft. Wenn Dateigruppen in der Datenbank als READ ONLY festgelegt sind, tritt beim Datenbankverschlüsselungsvorgang ein Fehler auf.

Der Verschlüsselungsstatus der Datenbank wird mit der dynamischen Verwaltungssicht [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) angezeigt.

**\<db_update_option> ::=**      
Steuert, ob Updates für die Datenbank zugelassen sind.

READ_ONLY     
Benutzer können Daten aus der Datenbank lesen, aber nicht ändern.

> [!NOTE]
> Um die Abfrageleistung zu verbessern, sollten Sie vor dem Festlegen einer Datenbank auf READ_ONLY die Statistiken aktualisieren. Wenn weitere Statistiken benötigt werden, nachdem eine Datenbank auf READ_ONLY festgelegt wurde, erstellt das [!INCLUDE[ssDE](../../includes/ssde-md.md)] Statistiken in tempdb. Weitere Informationen zu Statistiken für eine schreibgeschützte Datenbank finden Sie unter [Statistiken](../../relational-databases/statistics/statistics.md).

READ_WRITE     
Die Datenbank ist für Lese- und Schreibvorgänge verfügbar.

Sie müssen über exklusiven Zugriff auf die Datenbank verfügen, um diesen Status zu ändern. Weitere Informationen finden Sie unter der SINGLE_USER-Klausel.

> [!NOTE]
> Bei Verbunddatenbanken in [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] ist `SET { READ_ONLY | READ_WRITE }` deaktiviert.

**\<db_user_access_option> ::=**      
Steuert den Benutzerzugriff auf die Datenbank.

RESTRICTED_USER     
Ermöglicht nur Mitgliedern der festen Datenbankrolle `db_owner` und der festen Serverrollen `dbcreator` und `sysadmin` eine Verbindung mit der Datenbank, begrenzt jedoch nicht deren Anzahl. Alle Verbindungen zur Datenbank werden in dem durch die Beendigungsklausel der ALTER DATABASE-Anweisung angegebenen Zeitraum getrennt. Sobald die Datenbank in den Status RESTRICTED_USER gewechselt hat, werden Verbindungsversuche von nicht qualifizierten Benutzern abgelehnt. **RESTRICTED_USER** kann nicht mit SQL Managed Instance geändert werden.

MULTI_USER     
Alle Benutzer, die über die entsprechenden Berechtigungen für die Verbindung mit der Datenbank verfügen, sind zugelassen.

Sie können den Status dieser Option mithilfe der Spalte `user_access` in der Katalogsicht [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) oder der `UserAccess`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion ermitteln.

**\<delayed_durability_option> ::=**      
Steuert, ob für Transaktionen ein Commit mit vollständiger oder verzögerter Dauerhaftigkeit ausgeführt wird.

DISABLED     
Alle Transaktionen nach `SET DISABLED` sind vollständig dauerhaft. Alle Dauerhaftigkeitsoptionen, die in einem Atomic-Block oder einer Commitanweisung festgelegt sind, werden ignoriert.

ALLOWED     
Alle Transaktionen nach `SET ALLOWED` sind abhängig von der im atomischen Block oder der Commitanweisung festgelegten Dauerhaftigkeitsoption entweder vollständig dauerhaft oder verzögert dauerhaft.

FORCED     
Alle Transaktionen nach `SET FORCED` sind verzögert dauerhaft. Alle Dauerhaftigkeitsoptionen, die in einem Atomic-Block oder einer Commitanweisung festgelegt sind, werden ignoriert.

**\<PARAMETERIZATION_option> ::=**      
Steuert die Parametrisierungsoption.

PARAMETERIZATION { **SIMPLE** | FORCED }     
SIMPLE     
Abfragen werden basierend auf dem Standardverhalten der Datenbank parametrisiert.

FORCED     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] parametrisiert alle Abfragen in der Datenbank.

Die aktuelle Einstellung dieser Option kann mithilfe der Spalte `is_parameterization_forced` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht ermittelt werden.

<a name="query-store"></a> **\<query_store_options> ::=**

ON | OFF | CLEAR [ ALL ]     
Überprüft, ob der Abfragespeicher in dieser Datenbank aktiviert ist, und steuert außerdem das Entfernen des Inhalts des Abfragespeichers.

EIN     
Aktiviert den Abfragespeicher. ON ist der Standardwert.

OFF     
Deaktiviert den Abfragespeicher. 

> [!NOTE]  
> In [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] mit einer Einzeldatenbank oder einem Pool für elastische Datenbanken kann der Abfragespeicher nicht deaktiviert werden. Beim Ausführen von `ALTER DATABASE [database] SET QUERY_STORE = OFF` wird die Warnung `'QUERY_STORE=OFF' is not supported in this version of SQL Server.` zurückgegeben. 

CLEAR     
Entfernt den Inhalt des Abfragespeichers.

OPERATION_MODE     
Beschreibt den Betriebsmodus des Abfragespeichers. Gültige Werte sind READ_ONLY und READ_WRITE. Im Modus READ_WRITE sammelt und speichert der Abfragespeicher Angaben zum Abfrageplan und statistische Informationen zur Laufzeitausführung. Im Modus READ_ONLY können Informationen aus dem Abfragespeicher gelesen werden, es werden jedoch keine neuen Informationen hinzugefügt. Wenn die maximale Speicherplatzbelegung des Abfragespeichers ausgelastet ist, wird der Betriebsmodus in READ_ONLY geändert.

CLEANUP_POLICY     
Beschreibt die Datenaufbewahrungsrichtlinie des Abfragespeichers. STALE_QUERY_THRESHOLD_DAYS bestimmt die Anzahl an Tagen, für die die Informationen für eine Abfrage im Abfragespeicher aufbewahrt werden. STALE_QUERY_THRESHOLD_DAYS weist den Typ **bigint** auf. Der Standardwert ist 30. Für die Basic Edition von [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] ist der Standardwert **7** Tage.

DATA_FLUSH_INTERVAL_SECONDS     
Bestimmt die Häufigkeit, mit der in den Abfragespeicher geschriebene Daten auf Datenträger gespeichert werden. Um die Leistung zu optimieren, werden durch den Abfragespeicher gesammelte Daten asynchron auf den Datenträger geschrieben. Die Häufigkeit, mit der diese asynchrone Übertragung stattfindet, wird mit dem Argument DATA_FLUSH_INTERVAL_SECONDS konfiguriert. DATA_FLUSH_INTERVAL_SECONDS weist den Typ **bigint** auf. Der Standardwert ist **900** (15 Minuten).

MAX_STORAGE_SIZE_MB     
Bestimmt den Speicherplatz, der vom Abfragespeicher belegt wird. MAX_SIZE_MB weist den Typ **bigint** auf. Der Standardwert für die Premium Edition von [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] liegt bei **1 GB**, für die Basic Edition von [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] bei **10 MB**.

> [!NOTE]
> Die Grenze für die Einstellung `MAX_STORAGE_SIZE_MB` ist bei [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 10.240 MB. 

> [!NOTE]
> Die Begrenzung `MAX_STORAGE_SIZE_MB` wird nicht streng erzwungen. Die Speichergröße wird nur überprüft, wenn der Abfragespeicher Daten auf einen Datenträger schreibt. Dieses Intervall wird durch die Option `DATA_FLUSH_INTERVAL_SECONDS` oder die Option **Datenleerungsintervall** im Dialogfeld des [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]-Abfragespeichers festgelegt. Der Standardwert des Intervalls liegt bei 900 Sekunden (oder 15 Minuten).
> Wenn der Abfragespeicher die Begrenzung `MAX_STORAGE_SIZE_MB` zwischen Speichergrößenüberprüfungen überschritten hat, wechselt er in den schreibgeschützten Modus. Bei Aktivierung von `SIZE_BASED_CLEANUP_MODE` wird auch der Bereinigungsmechanismus zum Erzwingen der Begrenzung `MAX_STORAGE_SIZE_MB` ausgelöst.
> Sobald ausreichend Speicherplatz befreit wurde, wird der Abfragespeichermodus automatisch in den Lese-/Schreibmodus gewechselt.

> [!IMPORTANT]
> Wenn Sie glauben, dass für Ihre Arbeitsauslastungserfassung mehr als 10 GB Speicherplatz benötigt werden, sollten Sie vermutlich die Arbeitsauslastung überdenken und optimieren, um Abfragepläne wiederzuverwenden (z. B. mithilfe von [erzwungener Parametrisierung](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)), oder die Abfragespeicherkonfigurationen anpassen.    
> Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] und in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] können Sie für zusätzliche Kontrolle über die Abfrageerfassungsrichtlinie `QUERY_CAPTURE_MODE` auf CUSTOM festlegen.

INTERVAL_LENGTH_MINUTES     
Bestimmt das Zeitintervall, mit dem statistische Daten zur Laufzeitausführung im Abfragespeicher aggregiert werden. Um die Speicherverwendung zu optimieren, werden die statistischen Daten zur Laufzeitausführung im Speicher für Laufzeitstatistiken über ein festes Zeitfenster aggregiert. Dieses feste Zeitfenster wird mit dem Argument INTERVAL_LENGTH_MINUTES konfiguriert. INTERVAL_LENGTH_MINUTES weist den Typ **bigint** auf. Der Standardwert lautet **60**.

SIZE_BASED_CLEANUP_MODE     
Steuert, ob die Bereinigung automatisch aktiviert wird, wenn sich die Gesamtmenge der Daten der maximalen Größe nähert.

OFF     
Eine auf der Größe basierte Bereinigung wird nicht automatisch aktiviert.

AUTO     
Eine auf der Größe basierende Bereinigung wird automatisch aktiviert, wenn die Größe auf dem Datenträger 90 Prozent von **max_storage_size_mb** übersteigt. Ein auf der Größe basierendes Cleanup entfernt die am wenigsten aufwendigen und die ältesten Abfragen. Bei ungefähr 80 Prozent von **max_storage_size_mb** wird dieser Vorgang angehalten. Dies ist der Standardkonfigurationswert.

SIZE_BASED_CLEANUP_MODE ist vom Typ **nvarchar**.

QUERY_CAPTURE_MODE {ALL \| AUTO \| CUSTOM \| NONE}     
Bestimmt den zum aktuellen Zeitpunkt aktiven Abfrageerfassungsmodus. Für jeden Modus sind bestimmte Abfrageerfassungsrichtlinien definiert.   

> [!NOTE]
> Cursor, Abfragen in gespeicherten Prozeduren und nativ kompilierte Abfragen werden immer erfasst, wenn der Abfrageerfassungsmodus auf ALL, AUTO oder CUSTOM festgelegt ist.

ALL     
Erfasst alle Abfragen.

AUTO     
Relevante Abfragen werden anhand der Ausführungsanzahl und des Ressourcenverbrauchs erfasst. Dies ist der Standardkonfigurationswert für [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Keine     
Es werden keine weiteren neuen Abfragen erfasst. Der Abfragespeicher sammelt weiterhin Statistiken zur Kompilierung und Runtime für Abfragen, die bereits erfasst wurden. Verwenden Sie diese Konfiguration mit Bedacht, da dadurch möglicherweise wichtige Abfragen verloren gehen.

CUSTOM     
Ermöglicht die Kontrolle über die QUERY_CAPTURE_POLICY-Optionen.

QUERY_CAPTURE_MODE ist vom Typ **nvarchar**.

max_plans_per_query     
Definiert die maximale Anzahl von Plänen, die für jede Abfrage beibehalten werden. MAX_PLANS_PER_QUERY ist vom Typ **int**. Der Standardwert ist **200**.

WAIT_STATS_CAPTURE_MODE { **ON** | OFF }     
Steuert, ob Wartestatistiken pro Abfrage erfasst werden

EIN    
Wartestatistikinformationen pro Abfrage werden erfasst. Dies ist der Standardkonfigurationswert.

OFF    
Wartestatistikinformationen pro Abfrage werden nicht erfasst.

**\<query_capture_policy_option_list> :: =**      
Steuert die Optionen für die Erfassungsrichtlinie des Abfragespeichers. Mit Ausnahme von STALE_CAPTURE_POLICY_THRESHOLD definieren diese Optionen die OR-Bedingungen, die auftreten müssen, damit Abfragen im definierten STALE_CAPTURE_POLICY_THRESHOLD-Wert erfasst werden.

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
Definiert den Zeitraum des Auswertungsintervalls um zu bestimmen, ob eine Abfrage erfasst werden sollte. Der Standardwert ist 1 Tag. Sie können Werte zwischen 1 Stunde und sieben Tagen festlegen. *number* ist vom Datentyp **int**.

EXECUTION_COUNT     
Definiert die Häufigkeit, mit der eine Abfrage im Auswertungszeitraum ausgeführt wird. Der Standardwert ist 30, was bedeutet, dass für den Stale Capture Policy Threshold-Standardwert eine Abfrage mindestens 30 Mal an einem Tag ausgeführt werden muss, um im Abfragespeicher persistent gespeichert zu werden. EXECUTION_COUNT ist vom Typ **int**.

TOTAL_COMPILE_CPU_TIME_MS     
Definiert die gesamte verstrichene Kompilierungs-CPU-Zeit, die eine Abfrage über den Auswertungszeitraum in Anspruch nimmt. Der Standardwert ist 1000, was bedeutet, dass für den Stale Capture Policy Threshold-Standardwert eine Abfrage insgesamt mindestens eine Sekunde CPU-Zeit während der Abfragekompilierung an einem Tag verbraucht haben muss, um im Abfragespeicher persistent gespeichert zu werden. TOTAL_COMPILE_CPU_TIME_MS ist vom Typ **int**.

TOTAL_EXECUTION_CPU_TIME_MS     
Definiert die gesamte verstrichene Ausführungs-CPU-Zeit, die eine Abfrage über den Auswertungszeitraum in Anspruch nimmt. Der Standardwert ist 100, was bedeutet, dass für den Stale Capture Policy Threshold-Standardwert eine Abfrage insgesamt mindestens 100 ms CPU-Zeit während der Ausführung an einem Tag verbraucht haben muss, um im Abfragespeicher persistent gespeichert zu werden. TOTAL_EXECUTION_CPU_TIME_MS ist vom Typ **int**.

**\<snapshot_option> ::=**      
Bestimmt die Isolationsstufe für Transaktionen.

ALLOW_SNAPSHOT_ISOLATION { ON | **OFF** }     
EIN     
Aktiviert die Momentaufnahmeoption auf Datenbankebene. Wenn die Option aktiviert ist, beginnen DML-Anweisungen mit der Generierung von Zeilenversionen, auch wenn keine Transaktion die Momentaufnahmeisolation verwendet. Sobald diese Option aktiviert ist, können Transaktionen die SNAPSHOT-Transaktionsisolationsstufe angeben. Wenn eine Transaktion auf der SNAPSHOT-Isolationsebene ausgeführt wird, sehen alle Anweisungen eine Momentaufnahme der Daten, wie sie beim Start der Transaktion vorlagen. Greift eine Transaktion, die auf der SNAPSHOT-Isolationsstufe ausgeführt wird, auf Daten in mehreren Datenbanken zu, muss entweder in allen Datenbanken ALLOW_SNAPSHOT_ISOLATION auf ON festgelegt sein oder jede Anweisung in der Transaktion muss Sperrhinweise für alle Verweise in einer FROM-Klausel verwenden, die auf eine Tabelle in einer Datenbank verweisen, bei der ALLOW_SNAPSHOT_ISOLATION auf OFF festgelegt ist.

OFF     
Deaktiviert die Momentaufnahmeoption auf Datenbankebene. Transaktionen können die SNAPSHOT-Isolationsstufe für Transaktionen nicht angeben.

Wenn Sie ALLOW_SNAPSHOT_ISOLATION auf einen neuen Status festlegen (von ON zu OFF oder von OFF zu ON), gibt ALTER DATABASE die Kontrolle erst dann an den Aufrufer zurück, wenn ein Commit aller bestehenden Transaktionen in der Datenbank ausgeführt wurde. Hat die Datenbank bereits den in der ALTER DATABASE-Anweisung angegebenen Status, wird die Kontrolle direkt an den Aufrufer zurückgegeben. Wenn keine schnelle Rückgabe durch die ALTER DATABASE-Anweisung erfolgt, verwenden Sie [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md), um zu bestimmen, ob lang andauernde Transaktionen vorhanden sind. Wird die ALTER DATABASE-Anweisung abgebrochen, bleibt die Datenbank in dem Status, in dem sie sich vor dem Start von ALTER DATABASE befand. In der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht wird der Status von Isolationstransaktionen von Momentaufnahmen in der Datenbank angegeben. Ist **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON, wird ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF für 6 Sekunden angehalten und der Vorgang anschließend wiederholt.

Sie können den Status von ALLOW_SNAPSHOT_ISOLATION nicht ändern, wenn die Datenbank OFFLINE ist.

Wenn Sie ALLOW_SNAPSHOT_ISOLATION in einer READ_ONLY-Datenbank festlegen, wird die Einstellung gespeichert, wenn die Datenbank später auf READ_WRITE festgelegt wird.

Die aktuelle Einstellung dieser Option kann mithilfe der Spalte `snapshot_isolation_state` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht ermittelt werden.

READ_COMMITTED_SNAPSHOT { ON | OFF }     
EIN     
Aktiviert die Option READ COMMITTED-Snapshot auf Datenbankebene. Wenn die Option aktiviert ist, beginnen DML-Anweisungen mit der Generierung von Zeilenversionen, auch wenn keine Transaktion die Momentaufnahmeisolation verwendet. Sobald diese Option aktiviert ist, verwenden Transaktionen, die die READ COMMITTED-Isolationsstufe angeben, anstelle von Sperren die Zeilenversionsverwaltung. Wenn eine Transaktion auf der READ COMMITTED-Isolationsstufe ausgeführt wird, sehen alle Anweisungen eine Momentaufnahme der Daten, wie sie beim Start der Anweisung vorlagen.

OFF     
Deaktiviert die Option READ COMMITTED-Snapshot auf Datenbankebene. Transaktionen, die die READ COMMITTED-Isolationsstufe angeben, verwenden Sperren.

Wenn READ_COMMITTED_SNAPSHOT auf ON oder OFF festgelegt werden soll, dürfen außer der Verbindung, die den ALTER DATABASE-Befehl ausführt, keine aktiven Verbindungen zur Datenbank bestehen. Die Datenbank muss sich jedoch nicht im Einzelbenutzermodus befinden. Sie können den Status dieser Option nicht ändern, wenn die Datenbank OFFLINE ist.

Wenn Sie READ_COMMITTED_SNAPSHOT in einer READ_ONLY-Datenbank festlegen, wird die Einstellung beibehalten, wenn die Datenbank später auf READ_WRITE festgelegt wird.

READ_COMMITTED_SNAPSHOT kann für die Systemdatenbanken master, tempdb oder msdb nicht auf ON festgelegt werden. Wenn Sie die Einstellung für model ändern, wird die Einstellung zur Standardeinstellung für alle neu erstellten Datenbanken, mit Ausnahme von tempdb.

Die aktuelle Einstellung dieser Option kann mithilfe der Spalte `is_read_committed_snapshot_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht ermittelt werden.

> [!WARNING]
> Wenn eine Tabelle mit `DURABILITY = SCHEMA_ONLY` erstellt wird und **READ_COMMITTED_SNAPSHOT** anschließend mithilfe von `ALTER DATABASE` geändert wird, gehen die in der Tabelle enthaltenen Daten verloren.

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | **OFF** }     
EIN     
Wenn die Isolationsstufe für Transaktionen auf eine niedrigere Isolationsstufe als SNAPSHOT festgelegt wird, werden alle interpretierten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Vorgänge für speicheroptimierte Tabelle unter der Isolationsstufe SNAPSHOT ausgeführt. Beispiele für Isolationsstufen, die niedriger als SNAPSHOT sind, sind READ COMMITTED oder READ UNCOMMITTED. Diese Vorgänge erfolgen ungeachtet des Umstands, ob die Transaktionsisolationsstufe explizit auf der Sitzungsebene festgelegt ist, oder ob implizit die Standardeinstellung verwendet wird.

OFF     
Erhöht nicht die Isolationsstufe für Transaktionen für interpretierte [!INCLUDE[tsql](../../includes/tsql-md.md)]-Vorgänge für speicheroptimierte Tabellen.

Sie können den Status von MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT nicht ändern, wenn die Datenbank OFFLINE ist.

Der Standardwert ist OFF.

Die aktuelle Einstellung dieser Option kann mithilfe der Spalte `is_memory_optimized_elevate_to_snapshot_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht ermittelt werden.

**\<sql_option> ::=**      
Steuert die ANSI-Kompatibilitätsoptionen auf der Datenbankebene.

ANSI_NULL_DEFAULT { ON | **OFF** }     
Legt den Standardwert (NULL oder NOT NULL) einer Spalte oder [CLR user-defined type](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) fest, für den die NULL-Zulässigkeit nicht explizit in den CREATE TABLE- oder ALTER TABLE-Anweisungen festgelegt wurde. Spalten, die mit Einschränkungen definiert werden, folgen den Einschränkungsregeln, egal wie diese Einstellung lautet.

EIN     
Der Standardwert ist NULL.

OFF     
Der Standardwert ist NOT NULL.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für ANSI_NULL_DEFAULT. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die ANSI_NULL_DEFAULT für die Sitzung auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).

Für die ANSI-Kompatibilität wird durch Festlegen der Datenbankoption ANSI_NULL_DEFAULT auf ON der Datenbankstandardwert auf NULL geändert.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_ansi_null_default_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAnsiNullDefault`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

ANSI_NULLS { ON | **OFF** }     
EIN     
Alle Vergleiche mit einem Nullwert ergeben UNKNOWN.

OFF     
Vergleiche von Nicht-UNICODE-Werten mit einem Nullwert ergeben TRUE, wenn beide Werte NULL sind.

> [!IMPORTANT]
> In einer späteren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird ANSI_NULLS immer auf ON festgelegt, und jede Anwendung, für die die Option explizit auf OFF festgelegt wird, löst einen Fehler aus. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für ANSI_NULLS. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die ANSI_NULLS für die Sitzung auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md).

> [!NOTE]
> SET ANSI_NULLS muss ebenfalls auf ON festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_ansi_nulls_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAnsiNullsEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

ANSI_PADDING { ON | **OFF** }     
EIN     
Zeichenfolgen werden vor der Konvertierung auf dieselbe Länge aufgefüllt. Sie werden auch vor dem Einfügen in einen **varchar**- oder **nvarchar**-Datentyp durch Einfügen von Leerstellen auf dieselbe Länge gebracht.

OFF     
Fügt nachfolgende Leerräume in Zeichenwerte in **varchar** oder **nvarchar**-Spalten ein. Belässt außerdem nachfolgende Nullen in Binärwerten, die in **varbinary**-Spalten eingefügt werden. Werte werden nicht bis zur Spaltenlänge aufgefüllt.

Ist OFF festgelegt, wirkt sich diese Einstellung nur auf die Definition neuer Spalten aus.

> [!IMPORTANT]
> In einer späteren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird ANSI_PADDING immer auf ON festgelegt, und jede Anwendung, für die die Option explizit auf OFF festgelegt ist, löst einen Fehler aus. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Es wird empfohlen, für ANSI_PADDING stets den Wert ON festzulegen. ANSI_PADDING muss beim Erstellen oder Bearbeiten von Indizes für berechnete Spalten oder indizierte Sichten auf ON festgelegt sein.

**char(_n_)** - und **binary(_n_)** -Spalten, die NULL-Werte zulassen, werden bis zur Spaltenlänge aufgefüllt, wenn ANSI_PADDING auf ON festgelegt ist. Ist ANSI_PADDING hingegen auf OFF festgelegt, werden nachfolgende Leerzeichen und Nullen abgeschnitten. **char(_n_)** - und **binary(_n_)** -Spalten, die keine NULL-Werte zulassen, werden immer bis zur Spaltenlänge aufgefüllt.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für ANSI_PADDING. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die ANSI_PADDING für die Sitzung auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md).

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_ansi_padding_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAnsiPaddingEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

ANSI_WARNINGS { ON | **OFF** }     
EIN     
Fehler und Warnungen werden ausgegeben, wenn z.B. ein Fehler wegen „Division durch Null“ auftritt. Fehler oder Warnungen werden ebenfalls ausgegeben, wenn Nullwerte in Aggregatfunktionen auftreten.

OFF     
Bei Bedingungen wie einer Division durch Null werden keine Warnungen ausgegeben, und Nullwerte werden zurückgegeben.

> [!NOTE]
> SET ANSI_WARNINGS muss auf ON festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für ANSI_WARNINGS. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die ANSI_WARNINGS für die Sitzung auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md).

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_ansi_warnings_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAnsiWarningsEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

ARITHABORT { ON | OFF }     
EIN     
Eine Abfrage wird beendet, wenn während der Abfrage ein Überlauffehler oder ein Fehler aufgrund einer Division durch Null auftritt.

OFF     
Eine Warnmeldung wird angezeigt, wenn einer dieser Fehler auftritt. Die Verarbeitung der Abfrage, des Batches oder der Transaktion wird fortgesetzt, als wäre kein Fehler aufgetreten, selbst wenn eine Warnung angezeigt wird.

> [!NOTE]
> SET ARITHABORT muss auf ON festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_arithabort_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsArithmeticAbortEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

COMPATIBILITY_LEVEL = {150 \| 140 \| 130 \| 120 \| 110 \| 100}     
Weitere Informationen finden Sie unter [ALTER DATABASE-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

CONCAT_NULL_YIELDS_NULL { ON | **OFF** }     
EIN     
Das Ergebnis einer Verkettungsoperation ist NULL, wenn einer der Operanden NULL ist. Wenn z. B. die Zeichenfolge "This is" und NULL verkettet wird, ist das Ergebnis NULL statt "This is".

OFF     
Der Nullwert wird als leere Zeichenfolge behandelt.

> [!NOTE]
> CONCAT_NULL_YIELDS_NULL muss auf ON festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.
>
> In einer späteren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird CONCAT_NULL_YIELDS_NULL immer auf ON festgelegt, und jede Anwendung, für die die Option explizit auf OFF festgelegt wurde, löst einen Fehler aus. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für CONCAT_NULL_YIELDS_NULL. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die CONCAT_NULL_YIELDS_NULL für die Sitzung auf ON festgelegt wird, wenn sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_concat_null_yields_null_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsNullConcat`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

QUOTED_IDENTIFIER { ON | OFF }     
EIN     
Doppelte Anführungszeichen können nur zum Einschließen von Begrenzungsbezeichnern verwendet werden.

Alle Zeichenfolgen, die durch doppelte Anführungszeichen begrenzt werden, werden als Objektbezeichner interpretiert. Bezeichner in Anführungszeichen müssen nicht den [!INCLUDE[tsql](../../includes/tsql-md.md)]-Regeln für Bezeichner entsprechen. Sie können Schlüsselwörter darstellen und Zeichen einschließen, die in [!INCLUDE[tsql](../../includes/tsql-md.md)]-Bezeichnern nicht zulässig sind. Ein einfaches Anführungszeichen (’), das zur Literalzeichenfolge selbst gehört, kann durch doppelte Anführungszeichen (’’) dargestellt werden.

OFF     
Bezeichner dürfen nicht in Anführungszeichen eingeschlossen werden und müssen allen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Regeln für Bezeichner entsprechen. Literale können in einfache oder doppelte Anführungszeichen eingeschlossen werden.

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist es zudem möglich, Bezeichner durch eckige Klammern ([ ]) zu begrenzen. Bezeichner in eckigen Klammern können immer verwendet werden, egal wie die Einstellung für QUOTED_IDENTIFIER lautet. Weitere Informationen finden Sie unter [Datenbankbezeichner](../../relational-databases/databases/database-identifiers.md).

Beim Erstellen einer Tabelle wird die Option QUOTED IDENTIFIER immer als ON in den Metadaten der Tabelle gespeichert. Die Option wird gespeichert, selbst wenn die Option beim Erstellen der Tabelle auf OFF festgelegt ist.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für QUOTED_IDENTIFIER. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die QUOTED_IDENTIFIER auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_quoted_identifier_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsQuotedIdentifiersEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

NUMERIC_ROUNDABORT { ON | OFF }     
EIN     
Es wird ein Fehler generiert, wenn ein Genauigkeitsverlust in einem Ausdruck auftritt.

OFF     
Bei einem Genauigkeitsverlust wird keine Fehlermeldung generiert, und das Ergebnis wird auf die Genauigkeit der Spalte oder Variablen gerundet, die das Ergebnis speichert.

> [!IMPORTANT]
> NUMERIC_ROUNDABORT muss auf OFF festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_numeric_roundabort_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsNumericRoundAbortEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

RECURSIVE_TRIGGERS { ON | OFF }     
EIN     
Das rekursive Auslösen von AFTER-Triggern ist zugelassen.

OFF     
Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_recursive_triggers_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsRecursiveTriggersEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

> [!NOTE]
> Nur die direkte Rekursion wird verhindert, wenn RECURSIVE_TRIGGERS auf OFF festgelegt ist. Sie müssen auch die Geschachtelte Trigger-Serveroption auf 0 festlegen, um die indirekte Rekursion zu deaktivieren.

Sie können den Status dieser Option mithilfe der Spalte `is_recursive_triggers_on` in der Katalogsicht [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) oder der `IsRecursiveTriggersEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion ermitteln.

**\<target_recovery_time_option> ::=**      
Gibt die Frequenz indirekter Prüfpunkte auf Basis einzelner Datenbanken an. Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] beträgt der Standardwert für neue Datenbanken, der darauf hindeutet, dass Datenbanken indirekte Prüfpunkte verwendet werden, eine Minute. Der Standard für ältere Versionen ist 0 (null) und gibt an, dass die Datenbank automatische Prüfpunkte verwendet, deren Frequenz von der Einstellung für das Wiederherstellungsintervall der Serverinstanz abhängt. Für [!INCLUDE[msCoName](../../includes/msconame-md.md)] ist für die meisten Systeme eine Minute empfohlen.

TARGET_RECOVERY_TIME **=** target_recovery_time { SECONDS | MINUTES }     
*target_recovery_time*     
Gibt die maximale Grenze für die Zeit an, die für die Wiederherstellung der angegebenen Datenbank im Fall eines Fehlers aufgewendet wird.

SECONDS     
Gibt an, dass *target_recovery_time* die Anzahl von Sekunden darstellt.

MINUTES     
Gibt an, dass *target_recovery_time* die Anzahl von Minuten darstellt.

Weitere Informationen zu indirekten Prüfpunkten finden Sie unter [Datenbankprüfpunkte](../../relational-databases/logs/database-checkpoints-sql-server.md).

**WITH \<termination> ::=**      
Gibt an, wann beim Übergang der Datenbank von einem Status in einen anderen für unvollständige Transaktionen ein Rollback ausgeführt werden soll. Wird die Beendigungsklausel ausgelassen, wartet die ALTER DATABASE-Anweisung auf unbestimmte Zeit, wenn keine Sperre für die Datenbank besteht. Es kann nur eine Beendigungsklausel angegeben werden, und diese steht hinter den SET-Klauseln.

> [!NOTE]
> Nicht alle Datenbankoptionen verwenden die WITH \<termination>-Klausel. Weitere Informationen finden Sie in der Tabelle unter [Festlegen von Optionen](#SettingOptions) im Abschnitt „Hinweise“ in diesem Artikel.

ROLLBACK AFTER *integer* [SECONDS] | ROLLBACK IMMEDIATE     
Gibt an, ob ein Rollback sofort oder nach Ablauf der angegebenen Sekundenzahl ausgeführt werden soll.

NO_WAIT     
Gibt an, dass die Anforderung fehlschlägt, wenn diese Änderung des Datenbankstatus oder der Datenbankoption nicht sofort vollständig vorgenommen werden kann. Der sofortige Abschluss des Vorgangs bedeutet, dass nicht darauf gewartet wird, dass Transaktionen eigenständig einen Commit oder Rollback ausführen.

## <a name="setting-options"></a><a name="SettingOptions"></a> Festlegen von Optionen

Verwenden Sie die [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht oder [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md), um die aktuellen Einstellungen für Datenbankoptionen abzurufen.

Wenn Sie eine Datenbankoption festlegen, tritt die neue Einstellung sofort in Kraft.

Sie können die Standardwerte einer Datenbankoption für alle neu erstellten Datenbanken ändern. Hierzu ändern Sie die entsprechende Datenbankoption in der Modelldatenbank.

Nicht alle Datenbankoptionen verwenden die WITH \<termination>-Klausel oder können zusammen mit anderen Optionen festgelegt werden. In der folgenden Tabelle sind die Optionen und ihr Options- und Beendigungsstatus aufgeführt.

|Optionskategorie|Kann mit anderen Optionen angegeben werden|Kann WITH \<termination>-Klausel verwenden|
|----------------------|-----------------------------------------|---------------------------------------------|
|\<auto_option>|Ja|Nein|
|\<change_tracking_option>|Ja|Ja|
|\<cursor_option>|Ja|Nein|
|\<db_encryption_option>|Ja|Nein|
|\<db_update_option>|Ja|Ja|
|\<db_user_access_option>|Ja|Ja|
|\<delayed_durability_option>|Ja|Ja|
|\<parameterization_option>|Ja|Ja|
|ALLOW_SNAPSHOT_ISOLATION|Nein|Nein|
|READ_COMMITTED_SNAPSHOT|Nein|Ja|
|MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT|Ja|Ja|
|DATE_CORRELATION_OPTIMIZATION|Ja|Ja|
|\<sql_option>|Ja|Nein|
|\<target_recovery_time_option>|Nein|Ja|

## <a name="examples"></a>Beispiele

### <a name="a-setting-the-database-to-read_only"></a>A. Festlegen der Datenbank auf READ_ONLY
Für die Änderung des Status einer Datenbank oder Dateigruppe in READ_ONLY oder READ_WRITE wird der exklusive Zugriff auf die Datenbank benötigt. Im folgenden Beispiel wird die Datenbank in den `RESTRICTED_USER`-Modus gesetzt, um eingeschränkten Zugriff zu erhalten. Anschließend wird in dem Beispiel der Status der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank auf `READ_ONLY` festgelegt und der Zugriff auf die Datenbank an alle Benutzer zurückgegeben.

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RESTRICTED_USER;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO

```

### <a name="b-enabling-snapshot-isolation-on-a-database"></a>B. Aktivieren der Momentaufnahmeisolation für eine Datenbank
Im folgenden Beispiel wird die Option für das Momentaufnahmeisolations-Framework für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank aktiviert.

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO
```

Das Resultset zeigt, dass das Framework für die Momentaufnahmeisolation aktiviert ist.

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[database_name] |1| EIN |

### <a name="c-enabling-modifying-and-disabling-change-tracking"></a>C. Aktivieren, Ändern und Deaktivieren der Änderungsnachverfolgung
Im folgenden Beispiel wird die Änderungsnachverfolgung für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank aktiviert und die Aufbewahrungsdauer auf `2` Tage festgelegt.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

Das folgende Beispiel veranschaulicht, wie die Beibehaltungsdauer in drei Tage geändert wird.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

Das folgende Beispiel veranschaulicht, wie die Änderungsnachverfolgung für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank deaktiviert wird.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="d-enabling-the-query-store"></a>D: Aktivieren des Abfragespeichers
Im folgenden Beispiel werden der Abfragespeicher aktiviert und Parameter des Abfragespeichers konfiguriert.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="e-enabling-the-query-store-with-wait-statistics"></a>E. Aktivieren des Abfragespeichers mit Wartestatistiken
Im folgenden Beispiel werden der Abfragespeicher aktiviert und dessen Parameter konfiguriert.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="f-enabling-the-query-store-with-custom-capture-policy-options"></a>F. Aktivieren des Abfragespeichers mit benutzerdefinierten Erfassungsrichtlinienoptionen
Im folgenden Beispiel werden der Abfragespeicher aktiviert und dessen Parameter konfiguriert.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="see-also"></a>Weitere Informationen

- [ALTER DATABASE-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [ALTER DATABASE-Datenbankspiegelung](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [Statistik](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [Aktivieren und Deaktivieren der Änderungsnachverfolgung](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Bewährte Methoden für den Abfragespeicher](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Verwaltete Azure SQL-Instanz

Kompatibilitätsgrade sind `SET`-Optionen, die jedoch unter [ALTER DATABASE-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) beschrieben werden.

> [!NOTE]
> Viele DATABASE SET-Optionen können mithilfe von [SET-Anweisungen](../../t-sql/statements/set-statements-transact-sql.md) konfiguriert werden; häufig werden sie bei der Verbindung von Anwendungen konfiguriert. Die **ALTER DATABASE SET**-Werte werden durch SET-Optionen auf Sitzungsebene überschrieben. Die unten beschriebenen Datenbankoptionen entsprechen Werten, die für Sitzungen festgelegt werden können, von denen explizit keine weiteren Werte für SET-Optionen bereitgestellt werden.

## <a name="syntax"></a>Syntax

```syntaxsql
ALTER DATABASE { database_name | Current }
SET
{
    <optionspec> [ ,...n ]
}
;

<optionspec> ::=
{
    <auto_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
}
;
<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
    {
       = OFF
     | = ON [ ( <change_tracking_option_list > [,...n] ) ]
     | ( <change_tracking_option_list> [,...n] )
    }
}

<change_tracking_option_list> ::=
   {
       AUTO_CLEANUP = { ON | OFF }
     | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
   }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
}

<db_encryption_option> ::=
  ENCRYPTION { ON | OFF }

<delayed_durability_option> ::=DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<parameterization_option> ::=
  PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
  QUERY_STORE
  {
    = OFF
    | = ON [ ( <query_store_option_list> [,... n] ) ]
    | ( < query_store_option_list> [,... n] )
    | CLEAR [ ALL ]
  }
}

<query_store_option_list> ::=
{
  OPERATION_MODE = { READ_WRITE | READ_ONLY }
  | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
  | DATA_FLUSH_INTERVAL_SECONDS = number
  | MAX_STORAGE_SIZE_MB = number
  | INTERVAL_LENGTH_MINUTES = number
  | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
  | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
  | MAX_PLANS_PER_QUERY = number
  | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
  | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
    STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT {ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT {ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<temporal_history_retention>::= TEMPORAL_HISTORY_RETENTION { ON | OFF }
```

## <a name="arguments"></a>Argumente

*database_name*     
Der Name der Datenbank, die geändert werden soll.

CURRENT     
`CURRENT` führt die Aktion in der aktuellen Datenbank aus. `CURRENT` wird nicht in allen Kontexten für alle Optionen unterstützt. Wenn `CURRENT` einen Fehler verursacht, geben Sie den Datenbanknamen an.

**\<auto_option> ::=**      
Steuert automatische Optionen.

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { **ON** | OFF }     
EIN     
Der Abfrageoptimierer erstellt nach Bedarf Statistiken für einzelne Spalten in Abfrageprädikaten, um Abfragepläne sowie die Abfrageleistung zu verbessern. Diese Statistiken für einzelne Spalten werden erstellt, wenn der Abfrageoptimierer Abfragen kompiliert. Die Statistiken für einzelne Spalten werden nur für Spalten erstellt, die noch nicht der ersten Spalte eines vorhandenen Statistikobjekts entsprechen.

Der Standardwert ist ON. Für die meisten Datenbanken empfiehlt sich die Verwendung der Standardeinstellung.

OFF     
Der Abfrageoptimierer erstellt beim Kompilieren von Abfragen keine Statistiken für einzelne Spalten in Abfrageprädikaten. Das Festlegen dieser Option auf OFF kann zu suboptimalen Abfrageplänen und einer beeinträchtigten Abfrageleistung führen.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_auto_create_stats_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAutoCreateStatistics`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

Weitere Informationen finden Sie im Abschnitt „Statistikoptionen“ unter [Statistiken](../../relational-databases/statistics/statistics.md#statistics-options).

INCREMENTAL = ON | **OFF**     
Legen Sie AUTO_CREATE_STATISTICS auf ON und INCREMENTAL auf ON fest. Diese Einstellung erstellt automatisch Statistiken als inkrementell, wann immer inkrementelle Statistiken unterstützt werden. Der Standardwert ist OFF. Weitere Informationen finden Sie unter [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).

<a name="auto_shrink"></a> AUTO_SHRINK { ON | **OFF** }     
EIN     
Die Datenbankdateien sind Kandidaten für das periodische Verkleinern.

Sowohl Daten- als auch Protokolldateien können automatisch verkleinert werden. AUTO_SHRINK reduziert die Größe des Transaktionsprotokolls nur, wenn Sie die Datenbank auf das SIMPLE-Wiederherstellungsmodell festlegen oder das Protokoll sichern. Ist diese Option auf OFF festgelegt, werden die Datenbankdateien während der periodisch ausgeführten Überprüfung auf nicht verwendeten Speicherplatz nicht automatisch verkleinert.

Durch die Option AUTO_SHRINK werden Dateien dann verkleinert, wenn mehr als 25 Prozent der Datei aus nicht verwendetem Speicherplatz bestehen. Die Option bewirkt, dass die Datei, auf eine von zwei Größen verkleinert wird. Sie wird auf den jeweils größeren Wert verkleinert:

- die Größe, bei der 25 Prozent der Datei aus nicht verwendetem Speicherplatz bestehen
- die Größe der Datei, als sie erstellt wurde

Eine schreibgeschützte Datenbank kann nicht verkleinert werden.

OFF     
Die Datenbankdateien werden bei periodischen Prüfungen auf nicht verwendeten Speicherplatz nicht automatisch verkleinert.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_auto_shrink_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAutoShrink`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

> [!NOTE]
> Die AUTO_SHRINK-Option ist in einer eigenständigen Datenbank nicht verfügbar.

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { **ON** | OFF }     
EIN     
Gibt an, dass der Abfrageoptimierer Statistiken aktualisiert, wenn sie von einer Abfrage verwendet werden und veraltet sein könnten. Statistiken sind veraltet, wenn die Datenverteilung in der Tabelle oder indizierten Sicht durch die Vorgänge INSERT, UPDATE, DELETE oder MERGE geändert wurde. Der Abfrageoptimierer stellt fest, wann Statistiken veraltet sein könnten, indem er die Anzahl von Datenänderungen seit der letzten Statistikaktualisierung ermittelt und sie mit einem Schwellenwert vergleicht. Der Schwellenwert basiert auf der Anzahl von Zeilen in der Tabelle oder indizierten Sicht.

Bevor der Abfrageoptimierer eine Abfrage kompiliert und einen zwischengespeicherten Abfrageplan ausführt, sucht er nach veralteten Statistiken. Der Abfrageoptimierer ermittelt anhand der Spalten, Tabellen und indizierten Sichten im Abfrageprädikat, welche Statistiken veraltet sein könnten. Der Abfrageoptimierer ermittelt diese Informationen, bevor er eine Abfrage kompiliert. Vor dem Ausführen eines zwischengespeicherten Abfrageplans stellt die [!INCLUDE[ssDE](../../includes/ssde-md.md)] sicher, dass der Abfrageplan auf aktuelle Statistiken verweist.

Die AUTO_UPDATE_STATISTICS-Option gilt für Statistikobjekte, die für Indizes, einzelne Spalten in Abfrageprädikaten und mit der CREATE STATISTICS-Anweisung generierte Statistiken erstellt wurden. Diese Option gilt auch für gefilterte Statistiken.

Der Standardwert ist ON. Für die meisten Datenbanken empfiehlt sich die Verwendung der Standardeinstellung.

Verwenden Sie die AUTO_UPDATE_STATISTICS_ASYNC-Option, um anzugeben, ob die Statistiken synchron oder asynchron aktualisiert werden.

OFF     
Gibt an, dass der Abfrageoptimierer Statistiken nicht aktualisiert, wenn sie von einer Abfrage verwendet werden. Der Abfrageoptimierer aktualisiert Statistiken auch nicht, wenn sie veraltet sein könnten. Das Festlegen dieser Option auf OFF kann zu suboptimalen Abfrageplänen und einer beeinträchtigten Abfrageleistung führen.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte „is_auto_update_stats_on“ in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAutoUpdateStatistics`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

Weitere Informationen finden Sie im Abschnitt „Verwenden der datenbankweiten Statistikoptionen“ unter [Statistiken](../../relational-databases/statistics/statistics.md).

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | **OFF** }     
EIN     
Gibt an, dass Statistikupdates für die AUTO_UPDATE_STATISTICS-Option asynchron sind. Der Abfrageoptimierer wartet nicht, bis Statistikaktualisierungen abgeschlossen sind, bevor Abfragen kompiliert werden.

Das Festlegen dieser Option auf ON hat nur dann Auswirkungen, wenn AUTO_UPDATE_STATISTICS auf ON festgelegt ist.

Die AUTO_UPDATE_STATISTICS_ASYNC-Option ist standardmäßig auf OFF festgelegt, sodass der Abfrageoptimierer Statistiken synchron aktualisiert.

OFF     
Gibt an, dass Statistikupdates für die AUTO_UPDATE_STATISTICS-Option synchron sind. Der Abfrageoptimierer wartet, bis Statistikupdates abgeschlossen sind, bevor Abfragen kompiliert werden.

Das Festlegen dieser Option auf OFF hat nur dann Auswirkungen, wenn AUTO_UPDATE_STATISTICS auf ON festgelegt ist.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte „is_auto_update_stats_async_on“ in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen.

Weitere Informationen dazu, wann synchrone bzw. asynchrone Statistikupdates verwendet werden sollten, finden Sie im Abschnitt „Verwenden der datenbankweiten Statistikoptionen“ unter [Statistiken](../../relational-databases/statistics/statistics.md).

<a name="auto_tuning"></a> **\<automatic_tuning_option> ::=**      
**Gilt für:** [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]

Aktiviert bzw. deaktiviert die Option `FORCE_LAST_GOOD_PLAN` für die [automatische Optimierung](../../relational-databases/automatic-tuning/automatic-tuning.md).

FORCE_LAST_GOOD_PLAN = { ON | **OFF** }     
EIN     
Die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] erzwingt automatisch den neusten bekannten, geeigneten Plan bei [!INCLUDE[tsql-md](../../includes/tsql-md.md)]-Abfragen, bei denen neue Abfragepläne negative Auswirkungen auf die Leistung haben. Die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] überwacht regelmäßig die Abfrageleistung der [!INCLUDE[tsql-md](../../includes/tsql-md.md)]-Abfrage mit dem erzwungenen Plan. Wenn die Leistung verbessert wurde, verwendet die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] weiterhin den neusten bekannten, geeigneten Plan. Wenn die Leistung nicht verbessert wurde, erstellt die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] einen neuen Abfrageplan. Bei der Anweisung tritt ein Fehler auf, wenn der Abfragespeicher nicht aktiviert ist oder sich nicht im *Lese-/Schreibmodus* befindet.

OFF     
Die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] meldet potentielle Einbußen im Hinblick auf die Abfrageleistung, die von Änderungen des Abfrageplans in der [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)-Sicht hervorgerufen werden könnten. Diese Empfehlungen werden allerdings nicht automatisch angewendet. Benutzer können aktive Empfehlungen überwachen und ermittelte Probleme beheben, indem sie die in der Sicht aufgeführten [!INCLUDE[tsql-md](../../includes/tsql-md.md)]-Skripts anwenden. Dies ist der Standardwert.

**\<change_tracking_option> ::=**      
Steuert Änderungsnachverfolgungsoptionen. Sie können die Änderungsnachverfolgung aktivieren, Optionen festlegen, Optionen ändern und die Änderungsnachverfolgung deaktivieren. Beispiele hierzu finden Sie im Abschnitt „Beispiele“ weiter unten in diesem Artikel.

ON Aktiviert die Änderungsnachverfolgung für die Datenbank. Wenn die Änderungsnachverfolgung aktiviert wird, können auch die AUTO CLEANUP-Option und die CHANGE RETENTION-Option festgelegt werden.

AUTO_CLEANUP = { ON | OFF }     
EIN     
Die Änderungsnachverfolgungsdaten werden nach der angegebenen Beibehaltungsdauer automatisch entfernt.

OFF     
Die Änderungsnachverfolgungsdaten werden nicht aus der Datenbank entfernt.

CHANGE_RETENTION = *retention_period* { **DAYS** | HOURS | MINUTES }     
Gibt die Mindestdauer für die Beibehaltung von Änderungsnachverfolgungsdaten in der Datenbank an. Die Daten werden nur dann entfernt, wenn der Wert für AUTO_CLEANUP ON lautet.

*retention_period* ist ein Integer, der die numerische Komponente der Vermerkdauer angibt.

Die Standardbeibehaltungsdauer beträgt **2 Tage**. Die Mindestbeibehaltungsdauer ist 1 Minute. Der Standardtyp für die Beibehaltung ist **DAYS**.

OFF     
Deaktiviert die Änderungsnachverfolgung für die Datenbank. Deaktivieren Sie erst die Änderungsnachverfolgung für alle Tabellen, bevor Sie sie für die Datenbank deaktivieren.

**\<cursor_option> ::=**      
Steuert Cursoroptionen.

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
EIN     
Alle beim Commit oder Rollback einer Transaktion geöffneten Cursor werden geschlossen.

OFF     
Cursor bleiben beim Commit einer Transaktion geöffnet. Beim Rollback einer Transaktion werden alle Cursor geschlossen, sofern sie nicht als INSENSITIVE oder STATIC definiert sind.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für CURSOR_CLOSE_ON_COMMIT. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die CURSOR_CLOSE_ON_COMMIT für die Sitzung auf OFF festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).

Sie können den Status dieser Option mithilfe der Spalte „is_cursor_close_on_commit_on“ in der Katalogsicht [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) oder der „IsCloseCursorsOnCommitEnabled“-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion ermitteln. Die Zuordnung des Cursors wird implizit nur aufgehoben, wenn die Verbindung getrennt wird. Weitere Informationen finden Sie unter [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md).

**\<db_encryption_option> ::=**      
Steuert den Status der Datenbankverschlüsselung.

ENCRYPTION { ON | **OFF** }     
Legt fest, ob die Datenbank verschlüsselt (ON) oder nicht verschlüsselt (OFF) werden soll. Weitere Informationen finden Sie unter [Transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md) und [Transparent Data Encryption in Azure SQL-Datenbank](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

Wenn die Verschlüsselung auf Datenbankebene aktiviert wird, werden alle Dateigruppen verschlüsselt. Alle neuen Dateigruppen erben die verschlüsselte Eigenschaft. Wenn Dateigruppen in der Datenbank als READ ONLY festgelegt sind, tritt beim Datenbankverschlüsselungsvorgang ein Fehler auf.

Der Verschlüsselungsstatus der Datenbank wird mit der dynamischen Verwaltungssicht [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md) angezeigt.

**\<db_update_option> ::=**      
Steuert, ob Updates für die Datenbank zugelassen sind.

READ_ONLY     
Benutzer können Daten aus der Datenbank lesen, aber nicht ändern.

> [!NOTE]
> Um die Abfrageleistung zu verbessern, sollten Sie vor dem Festlegen einer Datenbank auf READ_ONLY die Statistiken aktualisieren. Wenn weitere Statistiken benötigt werden, nachdem eine Datenbank auf READ_ONLY festgelegt wurde, erstellt das [!INCLUDE[ssDE](../../includes/ssde-md.md)] Statistiken in tempdb. Weitere Informationen zu Statistiken für eine schreibgeschützte Datenbank finden Sie unter [Statistiken](../../relational-databases/statistics/statistics.md).

READ_WRITE     
Die Datenbank ist für Lese- und Schreibvorgänge verfügbar.

Sie müssen über exklusiven Zugriff auf die Datenbank verfügen, um diesen Status zu ändern.

**\<db_user_access_option> ::=**      
Steuert den Benutzerzugriff auf die Datenbank.

RESTRICTED_USER     
Ermöglicht nur Mitgliedern der festen Datenbankrolle `db_owner` und der festen Serverrollen `dbcreator` und `sysadmin` eine Verbindung mit der Datenbank, begrenzt jedoch nicht deren Anzahl. Alle Verbindungen zur Datenbank werden in dem durch die Beendigungsklausel der ALTER DATABASE-Anweisung angegebenen Zeitraum getrennt. Sobald die Datenbank in den Status RESTRICTED_USER gewechselt hat, werden Verbindungsversuche von nicht qualifizierten Benutzern abgelehnt. **RESTRICTED_USER** kann nicht mit SQL Managed Instance geändert werden.

MULTI_USER     
Alle Benutzer, die über die entsprechenden Berechtigungen für die Verbindung mit der Datenbank verfügen, sind zugelassen.

Sie können den Status dieser Option mithilfe der Spalte „user_access“ in der Katalogsicht [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) oder der `UserAccess`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion ermitteln.

**\<delayed_durability_option> ::=**      
Steuert, ob für Transaktionen ein Commit mit vollständiger oder verzögerter Dauerhaftigkeit ausgeführt wird.

DISABLED     
Alle Transaktionen nach `SET DISABLED` sind vollständig dauerhaft. Alle Dauerhaftigkeitsoptionen, die in einem Atomic-Block oder einer Commitanweisung festgelegt sind, werden ignoriert.

ALLOWED     
Alle Transaktionen nach `SET ALLOWED` sind abhängig von der im atomischen Block oder der Commitanweisung festgelegten Dauerhaftigkeitsoption entweder vollständig dauerhaft oder verzögert dauerhaft.

FORCED     
Alle Transaktionen nach `SET FORCED` sind verzögert dauerhaft. Alle Dauerhaftigkeitsoptionen, die in einem Atomic-Block oder einer Commitanweisung festgelegt sind, werden ignoriert.

**\<PARAMETERIZATION_option> ::=**      
Steuert die Parametrisierungsoption.

PARAMETERIZATION { **SIMPLE** | FORCED }     
SIMPLE     
Abfragen werden basierend auf dem Standardverhalten der Datenbank parametrisiert.

FORCED     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] parametrisiert alle Abfragen in der Datenbank.

Die aktuelle Einstellung dieser Option kann mithilfe der Spalte `is_parameterization_forced` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht ermittelt werden.

<a name="query-store"></a> **\<query_store_options> ::=**

ON | OFF | CLEAR [ ALL ]     
Überprüft, ob der Abfragespeicher in dieser Datenbank aktiviert ist, und steuert außerdem das Entfernen des Inhalts des Abfragespeichers.

EIN     
Aktiviert den Abfragespeicher.

OFF     
Deaktiviert den Abfragespeicher. Dies ist der Standardwert.

CLEAR     
Entfernt den Inhalt des Abfragespeichers.

OPERATION_MODE     
Beschreibt den Betriebsmodus des Abfragespeichers. Gültige Werte sind READ_ONLY und READ_WRITE. Im Modus READ_WRITE sammelt und speichert der Abfragespeicher Angaben zum Abfrageplan und statistische Informationen zur Laufzeitausführung. Im Modus READ_ONLY können Informationen aus dem Abfragespeicher gelesen werden, es werden jedoch keine neuen Informationen hinzugefügt. Wenn die maximale Speicherplatzbelegung des Abfragespeichers ausgelastet ist, wird der Betriebsmodus in READ_ONLY geändert.

CLEANUP_POLICY     
Beschreibt die Datenaufbewahrungsrichtlinie des Abfragespeichers. STALE_QUERY_THRESHOLD_DAYS bestimmt die Anzahl an Tagen, für die die Informationen für eine Abfrage im Abfragespeicher aufbewahrt werden. STALE_QUERY_THRESHOLD_DAYS weist den Typ **bigint** auf. Der Standardwert ist 30. Für die Basic Edition von [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] ist der Standardwert **7** Tage.

DATA_FLUSH_INTERVAL_SECONDS     
Bestimmt die Häufigkeit, mit der in den Abfragespeicher geschriebene Daten auf Datenträger gespeichert werden. Um die Leistung zu optimieren, werden durch den Abfragespeicher gesammelte Daten asynchron auf den Datenträger geschrieben. Die Häufigkeit, mit der diese asynchrone Übertragung stattfindet, wird mit dem Argument DATA_FLUSH_INTERVAL_SECONDS konfiguriert. DATA_FLUSH_INTERVAL_SECONDS weist den Typ **bigint** auf. Der Standardwert ist **900** (15 Minuten).

MAX_STORAGE_SIZE_MB     
Bestimmt den Speicherplatz, der vom Abfragespeicher belegt wird. MAX_SIZE_MB weist den Typ **bigint** auf. Der Standardwert ist **100 MB**.

> [!NOTE]
> Die Begrenzung `MAX_STORAGE_SIZE_MB` wird nicht streng erzwungen. Die Speichergröße wird nur überprüft, wenn der Abfragespeicher Daten auf einen Datenträger schreibt. Dieses Intervall wird durch die Option `DATA_FLUSH_INTERVAL_SECONDS` oder die Option **Datenleerungsintervall** im Dialogfeld des [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]-Abfragespeichers festgelegt. Der Standardwert des Intervalls liegt bei 900 Sekunden (oder 15 Minuten).
> Wenn der Abfragespeicher die Begrenzung `MAX_STORAGE_SIZE_MB` zwischen Speichergrößenüberprüfungen überschritten hat, wechselt er in den schreibgeschützten Modus. Bei Aktivierung von `SIZE_BASED_CLEANUP_MODE` wird auch der Bereinigungsmechanismus zum Erzwingen der Begrenzung `MAX_STORAGE_SIZE_MB` ausgelöst.
> Sobald ausreichend Speicherplatz befreit wurde, wird der Abfragespeichermodus automatisch in den Lese-/Schreibmodus gewechselt.

> [!IMPORTANT]
> Wenn Sie glauben, dass für Ihre Arbeitsauslastungserfassung mehr als 10 GB Speicherplatz benötigt werden, sollten Sie vermutlich die Arbeitsauslastung überdenken und optimieren, um Abfragepläne wiederzuverwenden (z. B. mithilfe von [erzwungener Parametrisierung](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)), oder die Abfragespeicherkonfigurationen anpassen.    
> Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] und in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] können Sie für zusätzliche Kontrolle über die Abfrageerfassungsrichtlinie `QUERY_CAPTURE_MODE` auf CUSTOM festlegen.

INTERVAL_LENGTH_MINUTES     
Bestimmt das Zeitintervall, mit dem statistische Daten zur Laufzeitausführung im Abfragespeicher aggregiert werden. Um die Speicherverwendung zu optimieren, werden die statistischen Daten zur Laufzeitausführung im Speicher für Laufzeitstatistiken über ein festes Zeitfenster aggregiert. Dieses feste Zeitfenster wird mit dem Argument INTERVAL_LENGTH_MINUTES konfiguriert. INTERVAL_LENGTH_MINUTES weist den Typ **bigint** auf. Der Standardwert lautet **60**.

SIZE_BASED_CLEANUP_MODE     
Steuert, ob die Bereinigung automatisch aktiviert wird, wenn sich die Gesamtmenge der Daten der maximalen Größe nähert.

OFF     
Eine auf der Größe basierte Bereinigung wird nicht automatisch aktiviert.

AUTO     
Eine auf der Größe basierende Bereinigung wird automatisch aktiviert, wenn die Größe auf dem Datenträger 90 Prozent von **max_storage_size_mb** übersteigt. Ein auf der Größe basierendes Cleanup entfernt die am wenigsten aufwendigen und die ältesten Abfragen. Bei ungefähr 80 Prozent von **max_storage_size_mb** wird dieser Vorgang angehalten. Dies ist der Standardkonfigurationswert.

SIZE_BASED_CLEANUP_MODE ist vom Typ **nvarchar**.

QUERY_CAPTURE_MODE {ALL \| AUTO \| CUSTOM \| NONE}     
Bestimmt den zum aktuellen Zeitpunkt aktiven Abfrageerfassungsmodus.

ALL     
Alle Abfragen werden erfasst.

AUTO     
Relevante Abfragen werden anhand der Ausführungsanzahl und des Ressourcenverbrauchs erfasst. Dies ist der Standardkonfigurationswert für [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Keine     
Es werden keine weiteren neuen Abfragen erfasst. Der Abfragespeicher sammelt weiterhin Statistiken zur Kompilierung und Runtime für Abfragen, die bereits erfasst wurden. Verwenden Sie diese Konfiguration mit Bedacht, da dadurch möglicherweise wichtige Abfragen verloren gehen.

QUERY_CAPTURE_MODE ist vom Typ **nvarchar**.

max_plans_per_query     
Eine ganze Zahl, die die maximale Anzahl von Plänen darstellt, die für jede Abfrage beibehalten werden. MAX_PLANS_PER_QUERY ist vom Typ **int**. Der Standardwert ist **200**.

WAIT_STATS_CAPTURE_MODE { **ON** | OFF }     
Steuert, ob Wartestatistiken pro Abfrage erfasst werden

EIN    
Wartestatistikinformationen pro Abfrage werden erfasst. Dies ist der Standardkonfigurationswert.

OFF    
Wartestatistikinformationen pro Abfrage werden nicht erfasst.

**\<query_capture_policy_option_list> :: =**      
Steuert die Optionen für die Erfassungsrichtlinie des Abfragespeichers. Mit Ausnahme von STALE_CAPTURE_POLICY_THRESHOLD definieren diese Optionen die OR-Bedingungen, die auftreten müssen, damit Abfragen im definierten STALE_CAPTURE_POLICY_THRESHOLD-Wert erfasst werden.

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
Definiert den Zeitraum des Auswertungsintervalls um zu bestimmen, ob eine Abfrage erfasst werden sollte. Der Standardwert ist 1 Tag. Sie können Werte zwischen 1 Stunde und sieben Tagen festlegen. *number* ist vom Datentyp **int**.

EXECUTION_COUNT     
Definiert die Häufigkeit, mit der eine Abfrage im Auswertungszeitraum ausgeführt wird. Der Standardwert ist 30, was bedeutet, dass für den Stale Capture Policy Threshold-Standardwert eine Abfrage mindestens 30 Mal an einem Tag ausgeführt werden muss, um im Abfragespeicher persistent gespeichert zu werden. EXECUTION_COUNT ist vom Typ **int**.

TOTAL_COMPILE_CPU_TIME_MS     
Definiert die gesamte verstrichene Kompilierungs-CPU-Zeit, die eine Abfrage über den Auswertungszeitraum in Anspruch nimmt. Der Standardwert ist 1000, was bedeutet, dass für den Stale Capture Policy Threshold-Standardwert eine Abfrage insgesamt mindestens eine Sekunde CPU-Zeit während der Abfragekompilierung an einem Tag verbraucht haben muss, um im Abfragespeicher persistent gespeichert zu werden. TOTAL_COMPILE_CPU_TIME_MS ist vom Typ **int**.

TOTAL_EXECUTION_CPU_TIME_MS     
Definiert die gesamte verstrichene Ausführungs-CPU-Zeit, die eine Abfrage über den Auswertungszeitraum in Anspruch nimmt. Der Standardwert ist 100, was bedeutet, dass für den Stale Capture Policy Threshold-Standardwert eine Abfrage insgesamt mindestens 100 ms CPU-Zeit während der Ausführung an einem Tag verbraucht haben muss, um im Abfragespeicher persistent gespeichert zu werden. TOTAL_EXECUTION_CPU_TIME_MS ist vom Typ **int**.

**\<snapshot_option> ::=**

Bestimmt die Isolationsstufe für Transaktionen.

ALLOW_SNAPSHOT_ISOLATION { ON| OFF }     
EIN     
Aktiviert die Momentaufnahmeoption auf Datenbankebene. Wenn die Option aktiviert ist, beginnen DML-Anweisungen mit der Generierung von Zeilenversionen, auch wenn keine Transaktion die Momentaufnahmeisolation verwendet. Nachdem diese Option aktiviert wurde, können Transaktionen die SNAPSHOT-Transaktionsisolationsstufe angeben. Wenn eine Transaktion auf der SNAPSHOT-Isolationsebene ausgeführt wird, sehen alle Anweisungen eine Momentaufnahme der Daten, wie sie beim Start der Transaktion vorlagen. Greift eine Transaktion, die auf der SNAPSHOT-Isolationsstufe ausgeführt wird, auf Daten in mehreren Datenbanken zu, muss entweder in allen Datenbanken ALLOW_SNAPSHOT_ISOLATION auf ON festgelegt sein oder jede Anweisung in der Transaktion muss Sperrhinweise für alle Verweise in einer FROM-Klausel verwenden, die auf eine Tabelle in einer Datenbank verweisen, bei der ALLOW_SNAPSHOT_ISOLATION auf OFF festgelegt ist.

OFF     
Deaktiviert die Momentaufnahmeoption auf Datenbankebene. Transaktionen können die SNAPSHOT-Isolationsstufe für Transaktionen nicht angeben.

Wenn Sie ALLOW_SNAPSHOT_ISOLATION auf einen neuen Status festlegen (von ON zu OFF oder von OFF zu ON), gibt ALTER DATABASE die Kontrolle erst dann an den Aufrufer zurück, wenn ein Commit aller bestehenden Transaktionen in der Datenbank ausgeführt wurde. Hat die Datenbank bereits den in der ALTER DATABASE-Anweisung angegebenen Status, wird die Kontrolle direkt an den Aufrufer zurückgegeben. Wenn keine schnelle Rückgabe durch die ALTER DATABASE-Anweisung erfolgt, verwenden Sie [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md), um zu bestimmen, ob lang andauernde Transaktionen vorhanden sind. Wird die ALTER DATABASE-Anweisung abgebrochen, bleibt die Datenbank in dem Status, in dem sie sich vor dem Start von ALTER DATABASE befand. In der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht wird der Status von Isolationstransaktionen von Momentaufnahmen in der Datenbank angegeben. Ist **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON, wird ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF für 6 Sekunden angehalten und der Vorgang anschließend wiederholt.

Sie können den Status von ALLOW_SNAPSHOT_ISOLATION nicht ändern, wenn die Datenbank OFFLINE ist.

Wenn Sie ALLOW_SNAPSHOT_ISOLATION in einer READ_ONLY-Datenbank festlegen, wird die Einstellung gespeichert, wenn die Datenbank später auf READ_WRITE festgelegt wird.

Sie können die ALLOW_SNAPSHOT_ISOLATION-Einstellungen für die Datenbanken master, model, msdb und tempdb ändern. Wenn Sie die Einstellung für tempdb ändern, wird die Einstellung jedes Mal beibehalten, wenn die Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] beendet und neu gestartet wird. Wenn Sie die Einstellung für model ändern, wird die Einstellung zur Standardeinstellung für alle neu erstellten Datenbanken, mit Ausnahme von tempdb.

Die Option hat für die Datenbanken master und msdb die Standardeinstellung ON.

Die aktuelle Einstellung dieser Option kann mithilfe der Spalte `snapshot_isolation_state` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht ermittelt werden.

READ_COMMITTED_SNAPSHOT { ON | **OFF** }     
EIN     
Aktiviert die Option READ_COMMITTED_SNAPSHOT auf Datenbankebene. Wenn die Option aktiviert ist, beginnen DML-Anweisungen mit der Generierung von Zeilenversionen, auch wenn keine Transaktion die Momentaufnahmeisolation verwendet. Nachdem diese Option aktiviert wurde, verwenden Transaktionen, die die READ COMMITTED-Isolationsstufe angeben, anstelle von Sperren die Zeilenversionsverwaltung. Wenn eine Transaktion auf der READ COMMITTED-Isolationsstufe ausgeführt wird, sehen alle Anweisungen eine Momentaufnahme der Daten, wie sie beim Start der Anweisung vorlagen.

OFF     
Deaktiviert die Option READ_COMMITTED_SNAPSHOT auf Datenbankebene. Transaktionen, die die READ COMMITTED-Isolationsstufe angeben, verwenden Sperren.

Wenn READ_COMMITTED_SNAPSHOT auf ON oder OFF festgelegt werden soll, dürfen außer der Verbindung, die den ALTER DATABASE-Befehl ausführt, keine aktiven Verbindungen zur Datenbank bestehen. Die Datenbank muss sich jedoch nicht im Einzelbenutzermodus befinden. Sie können den Status dieser Option nicht ändern, wenn die Datenbank OFFLINE ist.

Wenn Sie READ_COMMITTED_SNAPSHOT in einer READ_ONLY-Datenbank festlegen, wird die Einstellung beibehalten, wenn die Datenbank später auf READ_WRITE festgelegt wird.

READ_COMMITTED_SNAPSHOT kann für die Systemdatenbanken master, tempdb oder msdb nicht auf ON festgelegt werden. Wenn Sie die Einstellung für model ändern, wird die Einstellung zur Standardeinstellung für alle neu erstellten Datenbanken, mit Ausnahme von tempdb.

Die aktuelle Einstellung dieser Option kann mithilfe der Spalte `is_read_committed_snapshot_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht ermittelt werden.

> [!WARNING]
> Wenn eine Tabelle mit **DURABILITY = SCHEMA_ONLY** erstellt wird und **READ_COMMITTED_SNAPSHOT** anschließend mithilfe von **ALTER DATABASE** geändert wird, gehen Daten in der Tabelle verloren.

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | **OFF** }     
EIN     
Wenn die Isolationsstufe für Transaktionen auf eine niedrigere Isolationsstufe als SNAPSHOT festgelegt wird, werden alle interpretierten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Vorgänge für speicheroptimierte Tabelle unter der Isolationsstufe SNAPSHOT ausgeführt. Beispiele für Isolationsstufen, die niedriger als SNAPSHOT sind, sind READ COMMITTED oder READ UNCOMMITTED. Diese Vorgänge erfolgen ungeachtet des Umstands, ob die Transaktionsisolationsstufe explizit auf der Sitzungsebene festgelegt ist, oder ob implizit die Standardeinstellung verwendet wird.

OFF     
Erhöht nicht die Isolationsstufe für Transaktionen für interpretierte [!INCLUDE[tsql](../../includes/tsql-md.md)]-Vorgänge für speicheroptimierte Tabellen.

Sie können den Status von MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT nicht ändern, wenn die Datenbank OFFLINE ist.

Der Standardwert ist OFF.

Die aktuelle Einstellung dieser Option kann mithilfe der Spalte `is_memory_optimized_elevate_to_snapshot_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht ermittelt werden.

**\<sql_option> ::=**      
Steuert die ANSI-Kompatibilitätsoptionen auf der Datenbankebene.

ANSI_NULL_DEFAULT { ON | OFF }     
Legt den Standardwert (NULL oder NOT NULL) einer Spalte oder [CLR user-defined type](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) fest, für den die NULL-Zulässigkeit nicht explizit in den CREATE TABLE- oder ALTER TABLE-Anweisungen festgelegt wurde. Spalten, die mit Einschränkungen definiert werden, folgen den Einschränkungsregeln, egal wie diese Einstellung lautet.

EIN     
Der Standardwert ist NULL.

OFF     
Der Standardwert ist NOT NULL.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für ANSI_NULL_DEFAULT. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die ANSI_NULL_DEFAULT für die Sitzung auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).

Für die ANSI-Kompatibilität wird durch Festlegen der Datenbankoption ANSI_NULL_DEFAULT auf ON der Datenbankstandardwert auf NULL geändert.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_ansi_null_default_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAnsiNullDefault`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

ANSI_NULLS { ON | OFF }     
EIN     
Alle Vergleiche mit einem Nullwert ergeben UNKNOWN.

OFF     
Vergleiche von Nicht-UNICODE-Werten mit einem Nullwert ergeben TRUE, wenn beide Werte NULL sind.

> [!IMPORTANT]
> In einer späteren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird ANSI_NULLS immer auf ON festgelegt, und jede Anwendung, für die die Option explizit auf OFF festgelegt wird, löst einen Fehler aus. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden.

  Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für ANSI_NULLS. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die ANSI_NULLS für die Sitzung auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md).

> [!IMPORTANT]
> SET ANSI_NULLS muss ebenfalls auf ON festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_ansi_nulls_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAnsiNullsEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

ANSI_PADDING { ON | **OFF** }     
EIN     
Zeichenfolgen werden vor der Konvertierung auf dieselbe Länge aufgefüllt. Sie werden auch vor dem Einfügen in einen **varchar**- oder **nvarchar**-Datentyp durch Einfügen von Leerstellen auf dieselbe Länge gebracht.

OFF     
Fügt nachfolgende Leerräume in Zeichenwerte in **varchar** oder **nvarchar**-Spalten ein. Belässt außerdem nachfolgende Nullen in Binärwerten, die in **varbinary**-Spalten eingefügt werden. Werte werden nicht bis zur Spaltenlänge aufgefüllt.

Ist OFF festgelegt, wirkt sich diese Einstellung nur auf die Definition neuer Spalten aus.

> [!IMPORTANT]
> In einer späteren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird ANSI_PADDING immer auf ON festgelegt, und jede Anwendung, für die die Option explizit auf OFF festgelegt ist, löst einen Fehler aus. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Es wird empfohlen, für ANSI_PADDING stets den Wert ON festzulegen. ANSI_PADDING muss beim Erstellen oder Bearbeiten von Indizes für berechnete Spalten oder indizierte Sichten auf ON festgelegt sein.

**char(_n_)** - und **binary(_n_)** -Spalten, die NULL-Werte zulassen, werden bis zur Spaltenlänge aufgefüllt, wenn ANSI_PADDING auf ON festgelegt ist. Ist ANSI_PADDING hingegen auf OFF festgelegt, werden nachfolgende Leerzeichen und Nullen abgeschnitten. **char(_n_)** - und **binary(_n_)** -Spalten, die keine NULL-Werte zulassen, werden immer bis zur Spaltenlänge aufgefüllt.

  Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für ANSI_PADDING. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die ANSI_PADDING für die Sitzung auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md).

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_ansi_padding_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAnsiPaddingEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

ANSI_WARNINGS { ON | **OFF** }     
EIN     
Fehler und Warnungen werden ausgegeben, wenn z.B. ein Fehler wegen „Division durch Null“ auftritt. Fehler oder Warnungen werden ebenfalls ausgegeben, wenn Nullwerte in Aggregatfunktionen auftreten.

OFF     
Bei Bedingungen wie einer Division durch Null werden keine Warnungen ausgegeben, und Nullwerte werden zurückgegeben.

> [!IMPORTANT]
> SET ANSI_WARNINGS muss auf ON festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.

  Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für ANSI_WARNINGS. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die ANSI_WARNINGS für die Sitzung auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md).

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_ansi_warnings_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAnsiWarningsEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

ARITHABORT { ON | OFF }     
EIN     
Eine Abfrage wird beendet, wenn während der Abfrage ein Überlauffehler oder ein Fehler aufgrund einer Division durch Null auftritt.

OFF     
Eine Warnmeldung wird angezeigt, wenn einer dieser Fehler auftritt. Die Verarbeitung der Abfrage, des Batches oder der Transaktion wird fortgesetzt, als wäre kein Fehler aufgetreten, selbst wenn eine Warnung angezeigt wird.

> [!IMPORTANT]
> SET ARITHABORT muss auf ON festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.

  Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_arithabort_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsArithmeticAbortEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

COMPATIBILITY_LEVEL = {150 \| 140 \| 130 \| 120 \| 110 \| 100}     
Weitere Informationen finden Sie unter [ALTER DATABASE-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

CONCAT_NULL_YIELDS_NULL { ON | **OFF** }     
EIN     
Das Ergebnis einer Verkettungsoperation ist NULL, wenn einer der Operanden NULL ist. Wenn z. B. die Zeichenfolge "This is" und NULL verkettet wird, ist das Ergebnis NULL statt "This is".

OFF     
Der Nullwert wird als leere Zeichenfolge behandelt.

> [!IMPORTANT]
> CONCAT_NULL_YIELDS_NULL muss auf ON festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.
>
> In einer späteren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird CONCAT_NULL_YIELDS_NULL immer auf ON festgelegt, und jede Anwendung, für die die Option explizit auf OFF festgelegt wurde, löst einen Fehler aus. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für CONCAT_NULL_YIELDS_NULL. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die CONCAT_NULL_YIELDS_NULL für die Sitzung auf ON festgelegt wird, wenn sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_concat_null_yields_null_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsNullConcat`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

QUOTED_IDENTIFIER { ON | OFF }     
EIN     
Doppelte Anführungszeichen können nur zum Einschließen von Begrenzungsbezeichnern verwendet werden.

Alle Zeichenfolgen, die durch doppelte Anführungszeichen begrenzt werden, werden als Objektbezeichner interpretiert. Bezeichner in Anführungszeichen müssen nicht den [!INCLUDE[tsql](../../includes/tsql-md.md)]-Regeln für Bezeichner entsprechen. Sie können Schlüsselwörter darstellen und Zeichen einschließen, die in [!INCLUDE[tsql](../../includes/tsql-md.md)]-Bezeichnern nicht zulässig sind. Ein einfaches Anführungszeichen (’), das zur Literalzeichenfolge selbst gehört, kann durch doppelte Anführungszeichen (’’) dargestellt werden.

OFF     
Bezeichner dürfen nicht in Anführungszeichen eingeschlossen werden und müssen allen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Regeln für Bezeichner entsprechen. Literale können in einfache oder doppelte Anführungszeichen eingeschlossen werden.

  In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist es zudem möglich, Bezeichner durch eckige Klammern ([ ]) zu begrenzen. Bezeichner in eckigen Klammern können immer verwendet werden, egal wie die Einstellung für QUOTED_IDENTIFIER lautet. Weitere Informationen finden Sie unter [Datenbankbezeichner](../../relational-databases/databases/database-identifiers.md).

  Beim Erstellen einer Tabelle wird die Option QUOTED IDENTIFIER immer als ON in den Metadaten der Tabelle gespeichert. Die Option wird gespeichert, selbst wenn die Option beim Erstellen der Tabelle auf OFF festgelegt ist.

Einstellungen auf Verbindungsebene, die mithilfe der SET-Anweisung festgelegt werden, überschreiben die Standardeinstellung der Datenbank für QUOTED_IDENTIFIER. ODBC- und OLE DB-Clients geben standardmäßig eine SET-Anweisung aus, durch die QUOTED_IDENTIFIER auf ON festgelegt wird. Die Clients führen die Anweisung aus, wenn Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen. Weitere Informationen finden Sie unter [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

  Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_quoted_identifier_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsQuotedIdentifiersEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

NUMERIC_ROUNDABORT { ON | OFF }     
EIN     
Es wird ein Fehler generiert, wenn ein Genauigkeitsverlust in einem Ausdruck auftritt.

OFF     
Bei einem Genauigkeitsverlust wird keine Fehlermeldung generiert, und das Ergebnis wird auf die Genauigkeit der Spalte oder Variablen gerundet, die das Ergebnis speichert.

> [!IMPORTANT]
> NUMERIC_ROUNDABORT muss auf OFF festgelegt sein, wenn Sie Indizes auf berechneten Spalten oder indizierten Sichten erstellen oder ändern.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_numeric_roundabort_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsNumericRoundAbortEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

RECURSIVE_TRIGGERS { ON | OFF }     
EIN     
Das rekursive Auslösen von AFTER-Triggern ist zugelassen.

OFF     
Sie können den Status dieser Option ermitteln, indem Sie die Spalte `is_recursive_triggers_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsRecursiveTriggersEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

> [!NOTE]
> Nur die direkte Rekursion wird verhindert, wenn RECURSIVE_TRIGGERS auf OFF festgelegt ist. Sie müssen auch die Geschachtelte Trigger-Serveroption auf 0 festlegen, um die indirekte Rekursion zu deaktivieren.

Sie können den Status dieser Option mithilfe der Spalte `is_recursive_triggers_on` in der Katalogsicht [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) oder der `IsRecursiveTriggersEnabled`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion ermitteln.

**\<target_recovery_time_option> ::=**      
Gibt die Frequenz indirekter Prüfpunkte auf Basis einzelner Datenbanken an. Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] beträgt der Standardwert für neue Datenbanken, der darauf hindeutet, dass Datenbanken indirekte Prüfpunkte verwendet werden, **eine Minute**. Der Standard für ältere Versionen ist 0 (null) und gibt an, dass die Datenbank automatische Prüfpunkte verwendet, deren Frequenz von der Einstellung für das Wiederherstellungsintervall der Serverinstanz abhängt. Für [!INCLUDE[msCoName](../../includes/msconame-md.md)] ist für die meisten Systeme eine Minute empfohlen.

TARGET_RECOVERY_TIME **=** _target_recovery_time_ { SECONDS | MINUTES }     
*target_recovery_time*     
Gibt die maximale Grenze für die Zeit an, die für die Wiederherstellung der angegebenen Datenbank im Fall eines Fehlers aufgewendet wird.

SECONDS     
Gibt an, dass *target_recovery_time* die Anzahl von Sekunden darstellt.

MINUTES     
Gibt an, dass *target_recovery_time* die Anzahl von Minuten darstellt.

Weitere Informationen zu indirekten Prüfpunkten finden Sie unter [Datenbankprüfpunkte](../../relational-databases/logs/database-checkpoints-sql-server.md).

ROLLBACK AFTER *integer* [SECONDS] | ROLLBACK IMMEDIATE     
Gibt an, ob ein Rollback sofort oder nach Ablauf der angegebenen Sekundenzahl ausgeführt werden soll.

NO_WAIT     
Gibt an, dass die Anforderung fehlschlägt, wenn diese Änderung des Datenbankstatus oder der Datenbankoption nicht sofort vollständig vorgenommen werden kann. Der sofortige Abschluss des Vorgangs bedeutet, dass nicht darauf gewartet wird, dass Transaktionen eigenständig einen Commit oder Rollback ausführen.

## <a name="setting-options"></a><a name="SettingOptions"></a> Festlegen von Optionen
Verwenden Sie die [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht oder [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md), um die aktuellen Einstellungen für Datenbankoptionen abzurufen.

Wenn Sie eine Datenbankoption festlegen, tritt die neue Einstellung sofort in Kraft.

Sie können die Standardwerte einer Datenbankoption für alle neu erstellten Datenbanken ändern. Hierzu ändern Sie die entsprechende Datenbankoption in der Modelldatenbank.

## <a name="examples"></a>Beispiele

### <a name="a-setting-the-database-to-read_only"></a>A. Festlegen der Datenbank auf READ_ONLY
Für die Änderung des Status einer Datenbank oder Dateigruppe in READ_ONLY oder READ_WRITE wird der exklusive Zugriff auf die Datenbank benötigt. Im folgenden Beispiel wird die Datenbank in den `RESTRICTED_USER`-Modus gesetzt, um eingeschränkten Zugriff zu erhalten. Anschließend wird in dem Beispiel der Status der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank auf `READ_ONLY` festgelegt und der Zugriff auf die Datenbank an alle Benutzer zurückgegeben.

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RESTRICTED_USER;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO
```

### <a name="b-enabling-snapshot-isolation-on-a-database"></a>B. Aktivieren der Momentaufnahmeisolation für eine Datenbank
Im folgenden Beispiel wird die Option für das Momentaufnahmeisolations-Framework für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank aktiviert.

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO
```

Das Resultset zeigt, dass das Framework für die Momentaufnahmeisolation aktiviert ist.

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[database_name] |1| EIN |

### <a name="c-enabling-modifying-and-disabling-change-tracking"></a>C. Aktivieren, Ändern und Deaktivieren der Änderungsnachverfolgung
Im folgenden Beispiel wird die Änderungsnachverfolgung für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank aktiviert und die Aufbewahrungsdauer auf `2` Tage festgelegt.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

Das folgende Beispiel veranschaulicht, wie die Beibehaltungsdauer in `3` Tage geändert wird.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

Das folgende Beispiel veranschaulicht, wie die Änderungsnachverfolgung für die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank deaktiviert wird.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="d-enabling-the-query-store"></a>D: Aktivieren des Abfragespeichers
Im folgenden Beispiel werden der Abfragespeicher aktiviert und Parameter des Abfragespeichers konfiguriert.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="e-enabling-the-query-store-with-wait-statistics"></a>E. Aktivieren des Abfragespeichers mit Wartestatistiken
Im folgenden Beispiel werden der Abfragespeicher aktiviert und dessen Parameter konfiguriert.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="f-enabling-the-query-store-with-custom-capture-policy-options"></a>F. Aktivieren des Abfragespeichers mit benutzerdefinierten Erfassungsrichtlinienoptionen
Im folgenden Beispiel werden der Abfragespeicher aktiviert und dessen Parameter konfiguriert.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="see-also"></a>Weitere Informationen

- [ALTER DATABASE-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [ALTER DATABASE-Datenbankspiegelung](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [Statistik](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
- [Aktivieren und Deaktivieren der Änderungsnachverfolgung](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Bewährte Methoden für den Abfragespeicher](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>Syntax

```syntaxsql
ALTER DATABASE { database_name }
SET
{
    <optionspec> [ ,...n ]
}
;

<option_spec>::=
{
    <auto_option>
  | <db_encryption_option>
  | <query_store_options>
  | <result_set_caching>
  | <snapshot_option>
}
;

<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON }
}

<db_encryption_option> ::=
{
    ENCRYPTION { ON | OFF }
}

<query_store_option> ::=
{
    QUERY_STORE { OFF |  ON }
}

<result_set_caching_option> ::=
{
    RESULT_SET_CACHING {ON | OFF}
}

<snapshot_option> ::=
{
    READ_COMMITTED_SNAPSHOT {ON | OFF }
}

```

## <a name="arguments"></a>Argumente

*database_name*     
Der Name der Datenbank, die geändert werden soll.

**<auto_option> ::=**      
Steuert automatische Optionen.

AUTO_CREATE_STATISTICS { **ON** | OFF }     

EIN     
Der Abfrageoptimierer erstellt nach Bedarf Statistiken für einzelne Spalten in Abfrageprädikaten, um Abfragepläne sowie die Abfrageleistung zu verbessern. Diese Statistiken für einzelne Spalten werden erstellt, wenn der Abfrageoptimierer Abfragen kompiliert. Die Statistiken für einzelne Spalten werden nur für Spalten erstellt, die noch nicht der ersten Spalte eines vorhandenen Statistikobjekts entsprechen.

Der Standardwert ist ON. Für die meisten Datenbanken empfiehlt sich die Verwendung der Standardeinstellung.

OFF     
Der Abfrageoptimierer erstellt beim Kompilieren von Abfragen keine Statistiken für einzelne Spalten in Abfrageprädikaten. Das Festlegen dieser Option auf OFF kann zu suboptimalen Abfrageplänen und einer beeinträchtigten Abfrageleistung führen.

### <a name="remarks"></a>Bemerkungen
Sie müssen während der Ausführung dieses Befehls mit der Benutzerdatenbank verbunden sein.

Sie können den Status dieser Option ermitteln, indem Sie die Spalte `s_auto_create_stats_on` in der [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)-Katalogsicht untersuchen. Sie können den Status auch durch Untersuchen der `IsAutoCreateStatistics`-Eigenschaft der [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)-Funktion bestimmen.

Weitere Informationen finden Sie im Abschnitt „Verwenden der datenbankweiten Statistikoptionen“ unter „Statistiken“.

**<db_encryption_option> ::=**      
Steuert den Status der Datenbankverschlüsselung.

ENCRYPTION { ON | OFF }     

EIN     
Legt fest, dass die Datenbank verschlüsselt wird.

OFF     
Legt fest, dass die Datenbank nicht verschlüsselt wird.

Weitere Informationen finden Sie unter „Transparent Data Encryption“ und „Transparent Data Encryption in Azure SQL-Datenbank“.

Wenn die Verschlüsselung auf Datenbankebene aktiviert wird, werden alle Dateigruppen verschlüsselt. Alle neuen Dateigruppen erben die verschlüsselte Eigenschaft. Wenn Dateigruppen in der Datenbank als READ ONLY festgelegt sind, tritt beim Datenbankverschlüsselungsvorgang ein Fehler auf.

Sie können sowohl den Verschlüsselungsstatus der Datenbank als auch den Status des Verschlüsselungsscans mithilfe der dynamischen Verwaltungssicht „sys.dm_database_encryption_keys“ anzeigen.

**<query_store_option> ::=**      
Steuert, ob der Abfragespeicher in diesem Data Warehouse aktiviert ist.

QUERY_STORE { ON |  **OFF**  }     

EIN     
Aktiviert den Abfragespeicher.

OFF     
Deaktiviert den Abfragespeicher. OFF ist der Standardwert.

> [!NOTE]
> Für [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] müssen Sie `ALTER DATABASE SET QUERY_STORE` aus der Benutzerdatenbank ausführen. Ein Ausführen dieser Anweisung aus einer anderen Data Warehouse-Instanz wird nicht unterstützt.

**<result_set_caching_option> ::=**      
**Gilt für**: Azure Synapse Analytics  

Steuert, ob das Abfrageergebnis in der Datenbank zwischengespeichert wird.

RESULT_SET_CACHING { ON | OFF}     

EIN     
Gibt an, dass von dieser Datenbank zurückgegebene Abfrageresultsets in der Datenbank zwischengespeichert werden

OFF     
Gibt an, dass von dieser Datenbank zurückgegebene Abfrageresultsets nicht in der Datenbank zwischengespeichert werden

### <a name="remarks"></a>Bemerkungen

Sie müssen während der Ausführung dieses Befehls mit der `master`-Datenbank verbunden sein.  Änderungen an dieser Datenbankeinstellung werden sofort wirksam.  Speicherkosten fallen durch das Zwischenspeichern von Abfrageresultsets an. Nachdem die Zwischenspeicherung von Ergebnissen für eine Datenbank deaktiviert wurde, werden zuvor dauerhaft zwischengespeicherte Ergebnisse sofort aus dem Azure Synapse-Speicher gelöscht.

Führen Sie diesen Befehl aus, um die Cachekonfiguration der Ergebnismenge einer Datenbank zu überprüfen.  Wenn das Zwischenspeichern der Ergebnismenge aktiviert ist (ON), gibt is_result_set_caching_on 1 zurück.

```sql
SELECT name, is_result_set_caching_on FROM sys.databases
WHERE name = <'Your_Database_Name'>
```

Führen Sie diesen Befehl aus, um zu überprüfen, ob eine Abfrage mithilfe eines zwischengespeicherten Ergebnisses ausgeführt wurde.  Die Spalte „result_cache_hit“ gibt folgende Werte zurück: 1 für Cachetreffer, 0 für Cachefehler und negative Werte für Gründe, warum das Zwischenspeichern von Resultsets nicht verwendet wurde.  Weitere Informationen hierzu finden Sie unter [sys.dm_pdw_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true).  

```sql
SELECT request_id, command, result_cache_hit FROM sys.dm_pdw_exec_requests
WHERE request_id = <'Your_Query_Request_ID'>
```

> [!IMPORTANT]
> Die Vorgänge zum Erstellen des Resultsetcaches und zum Abrufen von Daten aus dem Cache erfolgen in dem Steuerknoten einer Data Warehouse-Instanz. Wenn das Zwischenspeichern von Resultsets aktiviert ist (ON), können ausgeführte Abfragen, die ein großes Resultset zurückgeben (z. B. >1 Million Zeilen), eine hohe CPU-Auslastung in dem Kontrollknoten verursachen und die gesamte Abfrageantwort auf der Instanz verlangsamen. Diese Abfragen werden häufig beim Durchsuchen von Daten oder ETL-Vorgängen verwendet. Benutzer sollten das Zwischenspeichern von Resultsets auf der Datenbank deaktivieren, bevor sie diese Abfragetypen ausführen, um den Kontrollknoten nicht zu belasten und Leistungsprobleme zu verursachen.  

Weitere Informationen zur Leistungsoptimierung mithilfe des Zwischenspeicherns von Resultsets finden Sie in den [Richtlinien zur Optimierung der Leistung](/azure/sql-data-warehouse/performance-tuning-result-set-caching).

### <a name="permissions"></a>Berechtigungen

Ein Benutzer muss entweder über die Serverebenenprinzipal-Anmeldung verfügen, die durch den Bereitstellungsprozess erstellt wurde, oder ein Mitglied der `dbmanager`-Datenbankrolle sein, um die Option RESULT_SET_CACHING festlegen zu können.  

**<snapshot_option> ::=**      
**Gilt für**: Azure Synapse Analytics

Steuert die Transaktionsisolationsstufe einer Datenbank.

READ_COMMITTED_SNAPSHOT  { ON | **OFF** }     

EIN     
Aktiviert die Option READ_COMMITTED_SNAPSHOT auf Datenbankebene.

OFF     
Deaktiviert die Option READ_COMMITTED_SNAPSHOT auf Datenbankebene.

### <a name="remarks"></a>Bemerkungen

Sie müssen während der Ausführung dieses Befehls mit der `master`-Datenbank verbunden sein. Wenn Sie für eine Benutzerdatenbank die Option READ_COMMITTED_SNAPSHOT auf ON oder OFF festlegen, werden alle aktiven Verbindungen mit dieser Datenbank getrennt. Sie sollten diese Änderung innerhalb des Datenbankwartungsfensters vornehmen oder warten, bis mit Ausnahme der Verbindung, die zur Ausführung des ALTER DATABASE-Befehls verwendet wird, keine Verbindung mit der Datenbank mehr besteht.  Die Datenbank muss sich nicht im Einzelbenutzermodus befinden. Die Einstellung READ_COMMITTED_SNAPSHOT kann nicht auf Sitzungsebene geändert werden.  Sie können mithilfe der Spalte „is_read_committed_snapshot_on“ in „sys.databases“ überprüfen, welche Einstellung für die Datenbank festgelegt ist.

Wenn für eine Datenbank READ_COMMITTED_SNAPSHOT aktiviert ist, werden Abfragen möglicherweise langsamer ausgeführt, wenn mehrere Datenversionen vorliegen und nach einer Version gesucht wird. Lange geöffnete Transaktionen können ebenfalls zu einer Vergrößerung der Datenbank führen. Dieses Problem tritt auf, wenn von diesen Transaktionen Datenänderungen vorgenommen werden, die die Versionsbereinigung blockieren.  

### <a name="permissions"></a>Berechtigungen

Ein Benutzer benötigt die ALTER-Berechtigung für eine Datenbank, um die Option READ_COMMITTED_SNAPSHOT festlegen zu können.

## <a name="examples"></a>Beispiele

### <a name="check-statistics-setting-for-a-database"></a>Überprüfen der Statistikeinstellung für eine Datenbank

```sql
SELECT name, is_auto_create_stats_on FROM sys.databases
```

### <a name="enable-query-store-for-a-database"></a>Aktivieren des Abfragespeichers für eine Datenbank

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON;
```

### <a name="enable-result-set-caching-for-a-database"></a>Aktivieren der Zwischenspeicherung von Resultsets für eine Datenbank

```sql
-- Run this command when connecting to the MASTER database

ALTER DATABASE [database_name]
SET RESULT_SET_CACHING ON;
```

### <a name="check-result-set-caching-setting-for-a-database"></a>Überprüfen der Einstellung für die Zwischenspeicherung von Resultsets für eine Datenbank

```sql
SELECT name, is_result_set_caching_on
FROM sys.databases;
```

### <a name="enable-the-read_committed_snapshot-option-for-a-database"></a>Aktivieren der Option READ_COMMITTED_SNAPSHOT für eine Datenbank

```sql
-- Run this command when connecting to the MASTER database

ALTER DATABASE MyDatabase
SET READ_COMMITTED_SNAPSHOT ON
```

## <a name="see-also"></a>Weitere Informationen

- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [Best Practices für Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-best-practices#maintain-statistics)
- [Entwerfen von Tabellen in Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-tables-overview#statistics)
- [Azure Synapse Analytics: Sprachelemente](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-language-elements)

::: moniker-end
