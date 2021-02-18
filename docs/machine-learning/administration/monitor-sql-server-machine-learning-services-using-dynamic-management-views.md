---
title: Überwachen von Skripts mithilfe von dynamischen Verwaltungssichten
description: Verwenden Sie dynamische Verwaltungssichten (DMVs) zum Überwachen der externen Skriptausführung von Python und R in SQL Server Machine Learning Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/14/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 91ae86607ee2127af21ae0d47f80580df32c93cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337101"
---
# <a name="monitor-sql-server-machine-learning-services-using-dynamic-management-views-dmvs"></a>Überwachen von SQL Server Machine Learning Services mithilfe von dynamischen Verwaltungssichten
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Verwenden Sie dynamische Verwaltungssichten (DMVs), um die externe Skriptausführung (Python und R) und Ressourcennutzung zu überwachen, Probleme zu diagnostizieren und die Leistung in SQL Server Machine Learning Services zu optimieren.

In diesem Artikel finden Sie DMVs, die spezifisch für SQL Server Machine Learning Services vorgesehen sind. Außerdem finden Sie Beispielabfragen, die Folgendes veranschaulichen:

+ Einstellungs- und Konfigurationsoptionen für maschinelles Lernen
+ Ausführung externer Python- oder R-Skripts durch aktive Sitzungen
+ Ausführungsstatistiken für die externe Runtime von Python und R
+ Leistungsindikatoren für externe Skripts
+ Arbeitsspeicherauslastung für das Betriebssystem, SQL Server und externe Ressourcenpools
+ Arbeitsspeicherkonfiguration für SQL Server und externe Ressourcenpools
+ Resource Governor-Ressourcenpools, einschließlich externer Ressourcenpools
+ Installierte Pakete für Python und R

Weitere allgemeine Informationen über DMVs finden Sie unter [Dynamische Systemverwaltungssichten](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).

> [!TIP]
> Sie können auch die benutzerdefinierten Berichte zum Überwachen von SQL Server Machine Learning Services verwenden. Weitere Informationen finden Sie unter [Überwachen von Machine Learning Services mithilfe benutzerdefinierter Berichte in Management Studio](monitor-sql-server-machine-learning-services-using-custom-reports-management-studio.md).

## <a name="dynamic-management-views"></a>Dynamische Verwaltungssichten

Die folgenden DMVs können beim Überwachen von Machine Learning-Workloads in SQL Server verwendet werden. Zum Abfragen der DMVs benötigen Sie die `VIEW SERVER STATE`-Berechtigung für die Instanz.

| Dynamische Verwaltungssicht | type | BESCHREIBUNG |
|-------------------------|------|-------------|
| [sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md) | Ausführung | Gibt eine Zeile für jedes aktive Workerkonto zurück, das ein externes Skript ausführt. |
| [sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md) | Ausführung | Gibt eine Zeile für jeden Typ von externer Skriptanforderung zurück. |
| [sys.dm_os_performance_counters](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) | Ausführung | Gibt eine Zeile pro Leistungsindikator zurück, der vom Server verwaltet wird. Wenn Sie die Suchbedingung `WHERE object_name LIKE '%External Scripts%'` verwenden, können Sie diese Information verwenden, um in Erfahrung zu bringen, wie viele Skripts ausgeführt wurden, welche Skripts mit welchem Authentifizierungsmodus ausgeführt wurden oder wie viele R- oder Python-Aufrufe insgesamt für die Instanz ausgegeben wurden. |
| [sys.dm_resource_governor_external_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md) | Resource Governor | Gibt Informationen zum aktuellen Status des externen Ressourcenpools im Resource Governor, zur aktuellen Konfiguration von Ressourcenpools und Ressourcenpoolstatistiken zurück. |
| [sys.dm_resource_governor_external_resource_pool_affinity](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md) | Resource Governor | Gibt CPU-Affinitätsinformationen zur aktuellen externen Ressourcenpoolkonfiguration im Resource Governor zurück. Gibt eine Zeile pro Zeitplanungsmodul in [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] zurück, wobei jedes Zeitplanungsmodul einem einzelnen Prozessor zugeordnet ist. Mithilfe dieser Sicht können Sie den Zustand eines Zeitplanungsmoduls überwachen oder Endlostasks identifizieren. |

Informationen zum Überwachen von [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]-Instanzen finden Sie unter [Katalogsichten](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md) und [Dynamische Verwaltungssichten in Verbindung mit dem Resource Governor](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md).

## <a name="settings-and-configuration"></a>Einstellungen und Konfiguration

Zeigen Sie die Installationseinstellungen und Konfigurationsoptionen von Machine Learning Services an.

![Ausgabe der Abfrage der Einstellungen und Konfiguration](media/dmv-settings-and-configuration.png "Ausgabe der Abfrage der Einstellungen und Konfiguration")

Führen Sie die folgende Abfrage aus, um diese Ausgabe zu erhalten. Weitere Informationen zu verwendeten Sichten und Funktionen finden Sie unter [sys.dm_server_registry](../../relational-databases/system-dynamic-management-views/sys-dm-server-registry-transact-sql.md), [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) und [SERVERPROPERTY](../../t-sql/functions/serverproperty-transact-sql.md).

```sql
SELECT CAST(SERVERPROPERTY('IsAdvancedAnalyticsInstalled') AS INT) AS IsMLServicesInstalled
    , CAST(value_in_use AS INT) AS ExternalScriptsEnabled
    , COALESCE(SIGN(SUSER_ID(CONCAT (
                    CAST(SERVERPROPERTY('MachineName') AS NVARCHAR(128))
                    , '\SQLRUserGroup'
                    , CAST(serverproperty('InstanceName') AS NVARCHAR(128))
                    ))), 0) AS ImpliedAuthenticationEnabled
    , COALESCE((
            SELECT CAST(r.value_data AS INT)
            FROM sys.dm_server_registry AS r
            WHERE r.registry_key LIKE 'HKLM\Software\Microsoft\Microsoft SQL Server\%\SuperSocketNetLib\Tcp'
            AND r.value_name = 'Enabled'
            ), - 1) AS IsTcpEnabled
FROM sys.configurations
WHERE name = 'external scripts enabled';
```

Die Abfrage gibt die folgenden Spalten zurück:

| Column                       | BESCHREIBUNG |
|------------------------------|-------------|
| IsMLServicesInstalled        | Gibt „1“ zurück, wenn SQL Server Machine Learning Services für die Instanz installiert ist. Andernfalls wird „0“ (null) zurückgegeben. |
| ExternalScriptsEnabled       | Gibt „1“ zurück, wenn externe Skripts für die Instanz aktiviert sind. Andernfalls wird „0“ (null) zurückgegeben. |
| ImpliedAuthenticationEnabled | Gibt „1“ zurück, wenn die implizite Authentifizierung aktiviert ist. Andernfalls wird „0“ (null) zurückgegeben. Die Konfiguration die für implizite Authentifizierung wird überprüft, indem verifiziert wird, ob eine Anmeldung für SQLRUserGroup existiert. |
| IsTcpEnabled                 | Gibt „1“ zurück, wenn das TCP/IP-Protokoll für die Instanz aktiviert ist. Andernfalls wird „0“ (null) zurückgegeben. Weitere Informationen finden Sie unter [Standardnetzwerkkonfiguration von SQL Server](../../database-engine/configure-windows/default-sql-server-network-protocol-configuration.md). |

## <a name="active-sessions"></a>Aktive Sitzungen

Zeigen Sie die aktiven Sitzungen an, die externe Skripts ausführen.

![Ausgabe der Abfrage aktiver Einstellungen](media/dmv-active-sessions.png "Ausgabe der Abfrage aktiver Einstellungen")

Führen Sie die folgende Abfrage aus, um diese Ausgabe zu erhalten. Weitere Informationen zu den verwendeten DMVs finden Sie unter [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md), [sys.dm_external_script_requests](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) und [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md).

```sql
SELECT r.session_id, r.blocking_session_id, r.status, DB_NAME(s.database_id) AS database_name
    , s.login_name, r.wait_time, r.wait_type, r.last_wait_type, r.total_elapsed_time, r.cpu_time
    , r.reads, r.logical_reads, r.writes, er.language, er.degree_of_parallelism, er.external_user_name
FROM sys.dm_exec_requests AS r
INNER JOIN sys.dm_external_script_requests AS er
ON r.external_script_request_id = er.external_script_request_id
INNER JOIN sys.dm_exec_sessions AS s
ON s.session_id = r.session_id;
```

Die Abfrage gibt die folgenden Spalten zurück:

| Column                | BESCHREIBUNG |
|-----------------------|-------------|
| session_id            | Identifiziert die einer aktiven primären Verbindung zugeordnete Sitzung. |
| blocking_session_id   | ID der Sitzung, die die Anforderung blockiert. Wenn diese Spalte den Wert NULL aufweist, wird die Anforderung nicht blockiert, oder die Sitzungsinformationen der blockierenden Sitzung sind nicht verfügbar (bzw. können nicht identifiziert werden). |
| status                | Status der Anforderung. |
| database_name         | Name der aktuellen Datenbank für jede Sitzung. |
| login_name            | SQL Server-Anmeldename, unter dem die Sitzung gegenwärtig ausgeführt wird. |
| wait_time             | Wenn die Anforderung zurzeit blockiert wird, gibt diese Spalte die Dauer des aktuellen Wartevorgangs in Millisekunden an. Lässt keine NULL-Werte zu. |
| wait_type             | Wenn die Anforderung zurzeit blockiert wird, gibt diese Spalte den Wartetyp zurück. Informationen zu Wartetypen finden Sie unter [sys.dm_os_wait_stats](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md). |
| last_wait_type        | Wenn diese Anforderung zuvor bereits blockiert war, gibt diese Spalte den Typ des letzten Wartevorgangs zurück. |
| total_elapsed_time    | Gesamtzeit seit dem Eintreffen der Anforderung (in Millisekunden). |
| cpu_time              | Von der Anforderung beanspruchte CPU-Zeit (in Millisekunden). |
| Lesevorgänge                 | Anzahl der von dieser Anforderung ausgeführten Lesevorgänge. |
| logical_reads         | Anzahl der von dieser Anforderung ausgeführten logischen Lesevorgänge. |
| Schreibvorgänge                | Anzahl der von dieser Anforderung ausgeführten Schreibvorgänge. |
| language              | Schlüsselwort, das einer unterstützten Skriptsprache entspricht. |
| degree_of_parallelism | Zahl, die die Anzahl von parallelen Prozessen angibt, die erstellt wurden. Dieser Wert kann sich von der Anzahl von parallelen Prozessen unterscheiden, die angefordert wurden. |
| external_user_name    | Das Windows-Workerkonto, unter dem das Skript ausgeführt wurde. |

## <a name="execution-statistics"></a>Ausführungsstatistiken

Zeigen Sie die Ausführungsstatistiken für die externe Runtime von Python und R an. Derzeit sind nur Statistiken der Paketfunktionen RevoScaleR, revoscalepy und MicrosoftML verfügbar.

![Ausgabe der Abfrage der Ausführungsstatistiken](media/dmv-execution-statistics.png "Ausgabe der Abfrage der Ausführungsstatistiken")

Führen Sie die folgende Abfrage aus, um diese Ausgabe zu erhalten. Weitere Informationen zu den verwendeten DMVs finden Sie unter [sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md). Die Abfrage gibt nur Funktionen zurück, die mehr als einmal ausgeführt wurden.

```sql
SELECT language, counter_name, counter_value
FROM sys.dm_external_script_execution_stats
WHERE counter_value > 0
ORDER BY language, counter_name;
```

Die Abfrage gibt die folgenden Spalten zurück:

| Column        | Beschreibung |
|---------------|-------------|
| language      | Name der registrierten externen Skriptsprache. |
| counter_name  | Name einer registrierten externen Skriptfunktion. |
| counter_value | Gesamtanzahl der Instanzen, die von der registrierten externen Skriptfunktion auf dem Server aufgerufen wurden. Dieser Wert ist kumulativ und beginnt mit dem Zeitpunkt, zu dem das Feature auf der Instanz installiert wurde. Der Wert kann nicht zurückgesetzt werden. |

## <a name="performance-counters"></a>Leistungsindikatoren

Zeigen Sie die Leistungsindikatoren für die Ausführung externer Skripts an.

![Ausgabe der Abfrage der Leistungsindikatoren](media/dmv-performance-counters.png "Ausgabe der Abfrage der Leistungsindikatoren")

Führen Sie die folgende Abfrage aus, um diese Ausgabe zu erhalten. Weitere Informationen zu den verwendeten DMVs finden Sie unter [sys.dm_os_performance_counters](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md).

```sql
SELECT counter_name, cntr_value
FROM sys.dm_os_performance_counters 
WHERE object_name LIKE '%External Scripts%'
```

**sys.dm_os_performance_counters** gibt die folgenden Leistungsindikatoren für externe Skripts aus:

| Leistungsindikator                   | BESCHREIBUNG |
|---------------------------|-------------|
| Total Executions          | Gibt die Anzahl der R-Prozesse an, die durch lokale oder Remoteaufrufe gestartet wurden. |
| Parallel Executions       | Gibt an, wie oft ein Skript die _\@parallel_-Spezifikation enthielt, und dass [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] einen parallelen Abfrageplan generieren und verwenden konnte. |
| Streaming Executions      | Gibt die Anzahl der Aufrufe des Streamingfeatures an. |
| SQL CC Executions         | Gibt die Anzahl der ausgeführten externen R-Skripts an, in denen der Aufruf remote instanziiert wurde und SQL Server als Computekontext verwendet wurde. |
| Implied Auth. Anmeldungen      | Gibt an, wie oft ein ODBC-Loopback-Aufruf mit implizierter Authentifizierung abgeschlossen wurde, d. h. dass [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] den Aufruf im Auftrag des Benutzers ausgeführt und die Skriptanforderung gesendet hat. |
| Total Execution Time (ms) | Gibt an, wie viel Zeit zwischen Aufruf und Abschluss des Aufrufs vergangen ist. |
| Execution Errors          | Gibt an, wie oft Skripts Fehler gemeldet haben. Diese Zahl umfasst keine R- oder Python-Fehler. |

## <a name="memory-usage"></a>Speicherauslastung

Zeigen Sie Informationen zum vom Betriebssystem, von SQL Server und von den externen Pools verwendeten Arbeitsspeicher an.

![Ausgabe der Abfrage der Arbeitsspeicherauslastung](media/dmv-memory-usage.png "Ausgabe der Abfrage der Arbeitsspeicherauslastung")

Führen Sie die folgende Abfrage aus, um diese Ausgabe zu erhalten. Weitere Informationen zu den verwendeten DMVs finden Sie unter [sys.dm_resource_governor_external_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md) und [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md).

```sql
SELECT physical_memory_kb, committed_kb
    , (SELECT SUM(peak_memory_kb)
        FROM sys.dm_resource_governor_external_resource_pools AS ep
        ) AS external_pool_peak_memory_kb
FROM sys.dm_os_sys_info;
```

Die Abfrage gibt die folgenden Spalten zurück:

| Column                       | BESCHREIBUNG                                                                                                           |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| physical_memory_kb           | Gibt die Gesamtmenge des physischen Speichers auf dem Computer an.                                                                   |
| committed_kb                 | Gibt den im Arbeitsspeicher-Manager zugesicherten Speicher in Kilobyte (KB) an. Reservierter Arbeitsspeicher im Speicher-Manager ist nicht eingeschlossen. |
| external_pool_peak_memory_kb | Gibt die Summe des maximalen verwendeten Arbeitsspeichers für alle externen Ressourcenpools in Kilobyte an.                          |

## <a name="memory-configuration"></a>Konfiguration des Arbeitsspeichers

Zeigen Sie Informationen zur maximalen Arbeitsspeicherkonfiguration von SQL Server und externen Ressourcenpools in Prozent an. Wenn SQL Server mit dem Standardwert `max server memory (MB)` ausgeführt wird, wird dieser als 100 % des Betriebssystemspeichers betrachtet.

![Ausgabe der Abfrage der Arbeitsspeicherkonfiguration](media/dmv-memory-configuration.png "Ausgabe der Abfrage der Arbeitsspeicherkonfiguration")

Führen Sie die folgende Abfrage aus, um diese Ausgabe zu erhalten. Weitere Informationen zu den verwendeten Sichten finden Sie unter [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) und [sys.dm_resource_governor_external_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md).

```sql
SELECT 'SQL Server' AS name
    , CASE CAST(c.value AS BIGINT)
        WHEN 2147483647 THEN 100
        ELSE (SELECT CAST(c.value AS BIGINT) / (physical_memory_kb / 1024.0) * 100 FROM sys.dm_os_sys_info)
        END AS max_memory_percent
FROM sys.configurations AS c
WHERE c.name LIKE 'max server memory (MB)'
UNION ALL
SELECT CONCAT ('External Pool - ', ep.name) AS pool_name, ep.max_memory_percent
FROM sys.dm_resource_governor_external_resource_pools AS ep;
```

Die Abfrage gibt die folgenden Spalten zurück:

| Column             | Beschreibung |
|--------------------|-------------|
| name               | Gibt den Namen des externen Ressourcenpools oder der SQL Server-Instanz an. |
| max_memory_percent | Gibt den maximalen Arbeitsspeicher an, den die SQL Server-Instanz oder der externe Ressourcenpool beanspruchen kann. |

## <a name="resource-pools"></a>Ressourcenpools

Im [SQL Server-Resource Governor](../../relational-databases/resource-governor/resource-governor.md) stellt ein [Ressourcenpool](../../relational-databases/resource-governor/resource-governor-resource-pool.md) eine Teilmenge der physischen Ressourcen einer Instanz dar. Sie können Grenzwerte für die CPU, physische E/A und den Arbeitsspeicher festlegen, die eingehende Anwendungsanforderungen, einschließlich der Ausführung externer Skripts, innerhalb des Ressourcenpools nutzen können. Zeigen Sie die für SQL Server und externe Skripts verwendeten Ressourcenpools an.

![Ausgabe der Ressourcenpoolabfrage](media/dmv-resource-pools.png "Ausgabe der Ressourcenpoolabfrage")

Führen Sie die folgende Abfrage aus, um diese Ausgabe zu erhalten. Weitere Informationen zu den verwendeten DMVs finden Sie unter [sys.dm_resource_governor_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md) und [sys.dm_resource_governor_external_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md).

```sql
SELECT CONCAT ('SQL Server - ', p.name) AS pool_name
    , p.total_cpu_usage_ms, p.read_io_completed_total, p.write_io_completed_total
FROM sys.dm_resource_governor_resource_pools AS p
UNION ALL
SELECT CONCAT ('External Pool - ', ep.name) AS pool_name
    , ep.total_cpu_user_ms, ep.read_io_count, ep.write_io_count
FROM sys.dm_resource_governor_external_resource_pools AS ep;
```

Die Abfrage gibt die folgenden Spalten zurück:

| Column                   | BESCHREIBUNG  |
|--------------------------|--------------|
| pool_name                | Name des Ressourcenpools. Den Namen von SQL Server-Ressourcenpools wird `SQL Server` und den Namen von externen Ressourcenpools wird `External Pool` vorangestellt. |
| total_cpu_usage_hours    | Gibt die kumulative CPU-Auslastung in Millisekunden seit der letzten Zurücksetzung der Resource Governor-Statistiken an. |
| read_io_completed_total  | Die Gesamtanzahl der E/A-Lesevorgänge, die seit der letzten Zurücksetzung der Resource Governor-Statistiken abgeschlossen wurden.              |
| write_io_completed_total | Die Gesamtanzahl der E/A-Schreibvorgänge, die seit der letzten Zurücksetzung der Resource Governor-Statistiken abgeschlossen wurden.             |

## <a name="installed-packages"></a>Installierte Pakete

Sie können die in SQL Server Machine Learning Services installierten R- und Python-Pakete anzeigen, indem Sie ein R- oder Python-Skript ausführen, das diese ausgibt.

### <a name="installed-packages-for-r"></a>Installierte R-Pakete

Zeigen Sie die in SQL Server Machine Learning Services installierten R-Pakete an.

![Ausgabe der Abfrage der installierten R-Pakete](media/dmv-installed-packages-r.png "Ausgabe der Abfrage der installierten R-Pakete")

Führen Sie die folgende Abfrage aus, um diese Ausgabe zu erhalten. Die Abfrage nutzt ein R-Skript, um die auf der SQL Server-Instanz installierten R-Pakete zu ermitteln.

```sql
EXECUTE sp_execute_external_script @language = N'R'
, @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((Package NVARCHAR(255), Version NVARCHAR(100), Depends NVARCHAR(4000)
    , License NVARCHAR(1000), LibPath NVARCHAR(2000)));
```

Die folgenden Spalten werden zurückgegeben:

| Column  | BESCHREIBUNG                                                 |
|---------|-------------------------------------------------------------|
| Paket | Gibt den Namen des installierten Pakets an.                              |
| Version | Gibt die Paketversion an.                                     |
| Depends (Abhängig) | Führt die Pakete auf, von denen das installierte Paket abhängt. |
| Lizenz | Gibt die Lizenz für das installierte Paket an.                          |
| LibPath | Gibt das Verzeichnis an, in dem Sie das Paket finden können.                   |

### <a name="installed-packages-for-python"></a>Installierte Python-Pakete

Zeigen Sie die in SQL Server Machine Learning Services installierten Python-Pakete an.

![Ausgabe der Abfrage der installierten Python-Pakete](media/dmv-installed-packages-python.png "Ausgabe der Abfrage der installierten Python-Pakete")

Führen Sie die folgende Abfrage aus, um diese Ausgabe zu erhalten. Die Abfrage nutzt ein Python-Skript, um die auf der SQL Server-Instanz installierten Python-Pakete zu ermitteln.

```sql
EXECUTE sp_execute_external_script @language = N'Python'
, @script = N'
import pkg_resources
import pandas
OutputDataSet = pandas.DataFrame(sorted([(i.key, i.version, i.location) for i in pkg_resources.working_set]))'
WITH result sets((Package NVARCHAR(128), Version NVARCHAR(128), Location NVARCHAR(1000)));
```

Die folgenden Spalten werden zurückgegeben:

| Column   | BESCHREIBUNG                               |
|----------|-------------------------------------------|
| Paket  | Gibt den Namen des installierten Pakets an.            |
| Version  | Gibt die Paketversion an.                   |
| Position | Gibt das Verzeichnis an, in dem Sie das Paket finden können. |

## <a name="next-steps"></a>Nächste Schritte

+ [Erweiterte Ereignisse für Machine Learning Services](extended-events.md)
+ [Dynamische Verwaltungssichten in Verbindung mit dem Resource Governor](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)
+ [Dynamische Systemverwaltungssichten](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
+ [Überwachen von Machine Learning Services mithilfe benutzerdefinierter Berichte in Management Studio](monitor-sql-server-machine-learning-services-using-custom-reports-management-studio.md)
