---
title: Neuigkeiten zu SQL Server 2019 | Microsoft-Dokumentation
description: Erfahren Sie mehr über die neuen Features von SQL Server 2019 (15.x), die Ihnen eine Auswahl aus Entwicklungssprachen, Datentypen, Umgebungen und Betriebssystemen bieten.
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: article
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: aadb59645e474557a92e3e87a0cd9c0c50df0b46
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271747"
---
# <a name="whats-new-in-sql-server-2019"></a>Neues in [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] ist eine Erweiterung der SQL Server-Plattform basierend auf früheren Releases, die Ihnen eine große Auswahl an Entwicklungssprachen, Datentypen und Betriebssystemen sowie die Arbeit mit einer lokalen Umgebung oder einer Cloud-Umgebung bietet.

In diesem Artikel werden die neuen Features und Verbesserungen für [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] beschrieben.

Weitere Informationen und bekannte Probleme finden Sie in den [[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]-Versionshinweisen](sql-server-version-15-release-notes.md).

Für optimale Erfahrungen mit [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] verwenden Sie die [neuesten Tools](../azure-data-studio/download-azure-data-studio.md).

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] führt [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] für [!INCLUDE[sql-server](../includes/ssnoversion-md.md)] ein. Außerdem werden zusätzliche Funktionen und Verbesserungen für die SQL Server-Datenbank-Engine, SQL Server Analysis Services, SQL Server Machine Learning Services, SQL Server für Linux und SQL Server Master Data Services bereitgestellt.

Im folgenden Video erhalten Sie eine 13-minütige Einführung in SQL Server 2019:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introducing-SQL-Server-2019/player?WT.mc_id=dataexposed-c9-niner]


Die folgenden Abschnitte bieten eine Übersicht über diese Features.

## <a name="data-virtualization-and-big-data-clusters-2019"></a>Datenvirtualisierung und [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]

Unternehmen verfügen heutzutage oft über umfangreiche Datensammlungen bestehend aus Datasets, deren Umfang stetig wächst und die in Datenquellensilos über das gesamte Unternehmen hinweg gehostet werden. Mit [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] erhalten Sie nahezu in Echtzeit Erkenntnisse aus all Ihren Daten. Dies erfolgt über eine vollständige Umgebung für die Arbeit mit umfangreichen Datasets, einschließlich maschinellen Lernens und künstlicher Intelligenz.

| Neue Funktion oder Update | Details |
|:---|:---|
| Skalierbare Big Data-Lösung | [Bereitstellen skalierbarer Cluster](../big-data-cluster/deploy-get-started.md) aus SQL Server-, Spark- und HDFS-Containern, die auf Kubernetes ausgeführt werden <br/><br/> Lesen, Schreiben und Verarbeiten von Big Data von Transact-SQL oder Spark<br/><br/> Einfaches Kombinieren und Analysieren hochwertiger relationaler Daten mit hohen Volumen von Big Data<br/><br/>Abfragen externer Datenquellen<br/><br/>Speichern von Big Data in von SQL Server verwaltetem HDFS<br/><br/>Abfragen von Daten aus mehreren externen Datenquellen über den Cluster<br/><br/> Verwenden der Daten für künstliche Intelligenz, maschinelles Lernen und andere Analyseaufgaben<br/><br/> [Bereitstellen und Ausführen von Anwendungen](../big-data-cluster/concept-application-deployment.md) in [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] <br/><br/> Die SQL Server-Masterinstanz stellt Hochverfügbarkeit und Notfallwiederherstellung für alle Datenbanken mithilfe von Always On-Verfügbarkeitsgruppen bereit.<br/>|
|Datenvirtualisierung mit PolyBase | Abfragen von Daten aus externen Datenquellen in SQL Server, Oracle, Teradata, MongoDB und ODBC mit externen Tabellen, jetzt mit [Unterstützung für UTF-8-Codierung](../relational-databases/collations/collation-and-unicode-support.md). Weitere Informationen finden Sie unter [Erläuterungen zu PolyBase](../relational-databases/polybase/polybase-guide.md).|
| &nbsp; | &nbsp; |

Weitere Informationen finden Sie unter [Erläuterungen zu SQL Server [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)]](../big-data-cluster/big-data-cluster-overview.md).

## <a name="intelligent-database"></a>Intelligente Datenbank
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] baut auf Innovationen in früheren Versionen auf, um eine sofort einsatzbereite branchenführende Leistung bereitzustellen. Von der [intelligenten Abfrageverarbeitung](../relational-databases/performance/intelligent-query-processing.md) bis hin zur Unterstützung persistenter Speichergeräte – die Funktionen der intelligenten Datenbank von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] verbessern die Leistung und Skalierbarkeit aller Ihrer Datenbankworkloads ohne Änderungen an Ihrer Anwendung oder Ihrem Datenbankentwurf.

### <a name="intelligent-query-processing"></a>Intelligente Abfrageverarbeitung
Dank der [intelligenten Abfrageverarbeitung](../relational-databases/performance/intelligent-query-processing.md) wissen Sie, dass kritische parallele Workloads bei der Skalierung verbessert werden. Gleichzeitig bleiben sie anpassungsfähig an die sich ständig verändernde Welt der Daten. Die intelligente Abfrageverarbeitung ist standardmäßig für die neueste [Datenbank-Kompatibilitätsgrad](../t-sql/statements/alter-database-transact-sql-compatibility-level.md#differences-between-compatibility-level-140-and-level-150)-Einstellung verfügbar und bietet eine Breitenwirkung, die die Leistung bestehender Workloads mit minimalem Implementierungsaufwand verbessert.

|Neue Funktion oder Update | Details |
|:---|:---|
|Feedback zur Speicherzuweisung im Zeilenmodus |Erweitert die Feedbackfunktion zur Speicherzuweisung im Batchmodus, indem die Größe der Speicherzuweisung sowohl für Batch- als auch für Zeilenmodusoperatoren angepasst wird. Durch diese Anpassung können übermäßige Zuweisungen, die zu Verschwendung von Speicherplatz und geringerer Parallelität führen, automatisch korrigiert werden. Außerdem können damit nicht ausreichende Speicherzuweisungen korrigiert werden, die zu teuren Überläufen auf den Datenträger führen. Siehe [Feedback zur Speicherzuweisung im Zeilenmodus](../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback). |
|Batchmodus bei Rowstore | Ermöglicht die Ausführung im Batchmodus, ohne dass Columnstore-Indizes erforderlich sind. Die Ausführung im Batchmodus führt zu einer effizienteren CPU-Nutzung für Analysearbeitsauslastungen, aber bis [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] wurde sie nur verwendet, wenn eine Abfrage Vorgänge mit Columnstore-Indizes enthielt. Einige Anwendungen verwenden jedoch möglicherweise Features, die keine Columnstore-Indizes unterstützen und den Batchmodus deshalb nicht nutzen können. Ab [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] wird der Batchmodus für berechtigte Analysearbeitsauslastungen aktiviert, deren Abfragen Vorgänge mit beliebigen Indextypen (Rowstore oder Columnstore) umfassen. Siehe [Batchmodus bei Rowstore](../relational-databases/performance/intelligent-query-processing.md#batch-mode-on-rowstore). |
|Inlining benutzerdefinierter Skalarfunktionen|Wandelt benutzerdefinierte Skalarfunktionen (UDF) automatisch in relationale Ausdrücke um und bettet sie in die aufrufende SQL-Abfrage ein. Diese Transformation verbessert die Leistung von Workloads, die skalare UDFs nutzen. Siehe [Inlining benutzerdefinierter Skalarfunktionen](../relational-databases/performance/intelligent-query-processing.md#scalar-udf-inlining).|
|Verzögerte Kompilierung von Tabellenvariablen|Verbessert die Qualität des Abfrageplans und die Gesamtleistung für Abfragen mit Verweisen auf Tabellenvariablen. Während der Optimierung und der ersten Kompilierung propagiert diese Funktion Kardinalitätsschätzungen, die auf tatsächlichen Tabellenvariablen-Zeilenzahlen basieren. Diese genauen Zeilenzahlinformationen werden für die Optimierung der nachgelagerten Planvorgänge verwendet. Siehe [Verzögerte Kompilierung von Tabellenvariablen](../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation). |
|Geschätzte Abfrageverarbeitung mit `APPROX_COUNT_DISTINCT` |Für Szenarios, in denen keine absolute Genauigkeit notwendig, die Reaktionsfähigkeit aber entscheidend ist, führt `APPROX_COUNT_DISTINCT` Aggregationen über große Datasets hinweg mit weniger Ressourcen durch als `COUNT(DISTINCT())`, um die Parallelität zu verbessern. Siehe [Geschätzte Abfrageverarbeitung](../relational-databases/performance/intelligent-query-processing.md#approximate-query-processing).|
| &nbsp; | &nbsp; |


### <a name="in-memory-database"></a>In-Memory Database
[In-Memory Database](../relational-databases/in-memory-database.md)-Technologien von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nutzen Hardwareinnovationen, um eine beispiellose Leistung und Skalierbarkeit zu erzielen. [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] baut auf früheren Innovationen in diesem Bereich auf, wie z. B. die In-Memory-Onlinetransaktionsverarbeitung (OLTP), um ein neues Maß an Skalierbarkeit für all Ihre Datenbankworkloads zu ermöglichen.  

|Neue Funktion oder Update | Details |
|:---|:---|
|Hybrider Pufferpool| Neues Feature von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], mit dem bei Bedarf direkt auf Datenbankseiten in Datenbankdateien zugegriffen werden, die sich auf einem Gerät mit beständigem Arbeitsspeicher (PMEM) befinden. Siehe [Hybrider Pufferpool](../database-engine/configure-windows/hybrid-buffer-pool.md).|
|Speicheroptimierte TempDB-Metadaten| Mit [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] wird eine neue Funktion eingeführt, die Teil der [In-Memory Database](../relational-databases/in-memory-database.md)-Featurefamilie ist. Hierbei handelt es sich um speicheroptimierte TempDB-Metadaten, durch die dieser Engpass effektiv behoben wird und sich eine neue Ebene der Skalierbarkeit für TempDB-intensive Workloads ergibt. In [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] können die Systemtabellen, die an der Verwaltung von Metadaten temporärer Tabellen beteiligt sind, in nicht dauerhafte speicheroptimierte Tabellen ohne Latches verschoben werden. Informationen finden Sie unter [Speicheroptimierte TempDB-Metadaten](../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata).|
| Unterstützung von In-Memory-OLTP für Datenbank-Momentaufnahmen | [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] führt Unterstützung für das Erstellen von [Datenbank-Momentaufnahmen](../relational-databases/databases/database-snapshots-sql-server.md) von Datenbanken mit speicheroptimierten Dateigruppen ein. |
| &nbsp; | &nbsp; |

### <a name="intelligent-performance"></a>Intelligente Leistung
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] baut auf Innovationen früherer Versionen für intelligente Datenbanken auf, um sicherzustellen, dass die [Ausführung noch schneller](/archive/blogs/bobsql/) erfolgt. Diese Verbesserungen helfen, bekannte Ressourcenengpässe zu überwinden, und bieten Optionen für das Konfigurieren Ihres Datenbankservers, um eine vorhersehbare Leistung für alle Ihre Workloads zu gewährleisten.

|Neue Funktion oder Update | Details |
|:---|:---|
|`OPTIMIZE_FOR_SEQUENTIAL_KEY`|Diese Option aktiviert eine Optimierung in der [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], die den Durchsatz für Einfügevorgänge mit hoher Parallelität in den Index verbessert. Diese Option ist für Indizes vorgesehen, die anfällig für Konflikte durch Einfügevorgänge auf der letzten Seite sind. Dieser Fall tritt häufig bei Indizes mit fortlaufendem Schlüssel auf. Dazu zählen beispielsweise Identitätsspalten, Sequenzspalten oder Spalten für Datum und Uhrzeit. Siehe [CREATE INDEX](../t-sql/statements/create-index-transact-sql.md#sequential-keys).|
|Erzwingen von schnellen Vorwärts- und statischen Cursorn | Stellt durch den Abfragespeicherplan erzwungene Unterstützung für schnelle Vorwärtscursor und statische Cursor bereit. Siehe [Erzwingen eines Plans für schnelle Vorwärts- und statische Cursor](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#ctp23).|
|Ressourcengovernance| Der Datentyp des konfigurierbaren Werts für die Option `REQUEST_MAX_MEMORY_GRANT_PERCENT` von `CREATE WORKLOAD GROUP` und `ALTER WORKLOAD GROUP` wurde geändert und entspricht nun nicht mehr einem Integer, sondern einer Gleitkommazahl. Dadurch können Speichergrößen präziser festgelegt werden. Weitere Informationen finden Sie unter [ALTER WORKLOAD GROUP](../t-sql/statements/alter-workload-group-transact-sql.md) und [CREATE WORKLOAD GROUP](../t-sql/statements/create-workload-group-transact-sql.md).|
|Weniger Neukompilierungen für Workloads| Verbessert die Leistung bei der Verwendung temporärer Tabellen über mehrere Bereiche hinweg, indem unnötige Neukompilierungen reduziert werden. Siehe [Weniger Neukompilierungen für Workloads](../relational-databases/tables/tables.md#ctp23). |
|Skalierbarkeit indirekter Prüfpunkte |Siehe [Verbesserte Skalierbarkeit indirekter Prüfpunkte](../relational-databases/logs/database-checkpoints-sql-server.md#ctp23).|
|Parallele PFS-Aktualisierungen|[PFS-Seiten](https://techcommunity.microsoft.com/t5/SQL-Server/Under-the-covers-GAM-SGAM-and-PFS-pages/ba-p/383125) (Page Free Space-Seiten) sind spezielle Seiten innerhalb einer Datenbankdatei, die SQL Server verwendet, um beim Zuweisen von Speicherplatz für ein Objekt freien Speicherplatz zu ermitteln. Latchkonflikte für PFS-Seiten stehen häufig in Zusammenhang mit [TempDB](https://support.microsoft.com/help/2154845/recommendations-to-reduce-allocation-contention-in-sql-server-tempdb-d), können aber auch für Benutzerdatenbanken auftreten, wenn viele parallele Threads zur Objektzuweisung vorliegen. Diese Verbesserung ändert die Verwaltung paralleler Vorgänge bei PFS-Aktualisierungen, sodass die Aktualisierung nicht mit einem exklusiven Latch, sondern über einen gemeinsamen Latch erfolgen kann. Dieses Verhalten ist ab [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] in allen Datenbanken (TempDB eingeschlossen) standardmäßig aktiviert.|
|Workermigration von Schedulern |Mithilfe der Workermigration kann ein Scheduler im Leerlauf einen Worker der ausführbaren Warteschlange eines anderen Schedulers auf demselben NUMA-Knoten migrieren und sofort die Aufgabe des migrierten Workers fortsetzen. Diese Erweiterung sorgt für eine ausgeglichenere CPU-Auslastung in Situationen, in denen Aufgaben mit langer Ausführungszeit demselben Scheduler zugewiesen sind. Weitere Informationen finden Sie unter [Intelligente Leistung von SQL Server 2019 – Workermigration](https://techcommunity.microsoft.com/t5/SQL-Server/SQL-Server-2019-Intelligent-Performance-Worker-Migration/ba-p/939610). |
| &nbsp; | &nbsp; |

### <a name="monitoring"></a>Überwachung
Durch eine verbesserte Überwachung stehen Informationen zur Leistung einzelner Datenbankworkloads in dem Moment zur Verfügung, in dem diese benötigt werden.

|Neue Funktion oder Update | Details |
|:---|:---|
|`WAIT_ON_SYNC_STATISTICS_REFRESH` | Ein neuer Wartetyp in der dynamischen `sys.dm_os_wait_stats`-Verwaltungssicht. Er zeigt die kumulierte Zeit auf Instanzebene für synchrone Statistikaktualisierungen an. Siehe [`sys.dm_os_wait_stats`](../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|
|Angepasste Erfassungsrichtlinie für den Abfragespeicher | Bei Aktivierung dieser Richtlinie stehen unter einer neuen Einstellung für die Erfassungsrichtlinie des Abfragespeichers zusätzliche Abfragespeicherkonfigurationen zur Verfügung, um die Datensammlung auf einem bestimmten Server zu optimieren. Siehe [ALTER DATABASE SET-Optionen](../t-sql/statements/alter-database-transact-sql-set-options.md).|
|`LIGHTWEIGHT_QUERY_PROFILING`| Eine neue datenbankweite Konfiguration. Siehe [`LIGHTWEIGHT_QUERY_PROFILING`](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#lqp). |
|`sys.dm_exec_requests`-Spalten-`command` | Zeigt `SELECT (STATMAN)` an, wenn ein `SELECT`-Objekt darauf wartet, dass ein synchroner Statistikupdatevorgang abgeschlossen wird, bevor die Abfrage weiter ausgeführt wird. Siehe [`sys.dm_exec_requests`](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|
|`sys.dm_exec_query_plan_stats` | Eine neue dynamische Verwaltungsfunktion (DMF), die das Äquivalent des letzten bekannten tatsächlichen Ausführungsplans für alle Abfragen zurückgibt. Siehe [sys.dm_exec_query_plan_stats](../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md).|
|`LAST_QUERY_PLAN_STATS` | Eine neue datenbankweit gültige Konfiguration, die `sys.dm_exec_query_plan_stats` ermöglicht. Finden Sie unter [ALTER ausgelegte DATENBANKKONFIGURATION](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).|
|`query_post_execution_plan_profile` | Ein erweitertes Ereignis, das das Äquivalent eines tatsächlichen, auf einfacher Profilerstellung basierenden Ausführungsplans erfasst – im Gegensatz zu `query_post_execution_showplan`, das die Standardprofilerstellung verwendet. Siehe [Profilerstellungsinfrastruktur für Abfragen](../relational-databases/performance/query-profiling-infrastructure.md).|
|`sys.dm_db_page_info(database_id, file_id, page_id, mode)` | Eine neue DMF, die Informationen über eine Seite in einer Datenbank zurückgibt. Weitere Informationen finden Sie unter [sys.dm_db_page_info (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md).|
| &nbsp; | &nbsp; |

## <a name="developer-experience"></a>Entwicklerumgebung
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] bietet weiterhin eine erstklassige Entwicklerumgebung mit Verbesserungen bei Graph- und räumlichen Datentypen, UTF-8-Unterstützung und einem neuen Erweiterbarkeitsframework, mit dem Entwickler die Sprache ihrer Wahl verwenden können, um Einblicke in alle ihre Daten zu erhalten.

### <a name="graph"></a>Graph

|Neue Funktion oder Update | Details |
|:---|:---|
|Kaskadierende Löschaktionen für Edgeeinschränkung | Sie können nun kaskadierende Löschaktionen für eine Edgeeinschränkung in einer Graphdatenbank definieren. Siehe [Edgeeinschränkungen](../relational-databases/tables/graph-edge-constraints.md). |
|Neue Graphfunktion: `SHORTEST_PATH` | Sie können nun `SHORTEST_PATH` in `MATCH` verwenden, um den kürzesten Pfad zwischen zwei Knoten in einem Graph zu ermitteln oder beliebige Längentraversierungen durchzuführen.|
|Partitionierte Tabellen und Indizes| Graphtabellen unterstützen jetzt die Tabellen- und Indexpartitionierung. |
|Verwenden von abgeleiteten Tabellen- oder Ansichtsaliasen in Graphabgleichsabfragen |Siehe [MATCH-Graphabfrage](../t-sql/queries/match-sql-graph.md). |
| &nbsp; | &nbsp; |

### <a name="unicode-support"></a>Unicode-Unterstützung
Unterstützt in verschiedenen Ländern und Regionen tätige Unternehmen, in denen zwingend globale mehrsprachige Datenbankanwendungen und -dienste bereitgestellt werden müssen, um die Kundenanforderungen zu erfüllen und spezifische Marktvorschriften einzuhalten. 

|Neue Funktion oder Update | Details |
|:---|:---|
|Unterstützung der UTF-8-Zeichencodierung |Unterstützt UTF-8 für die Import- und Exportcodierung sowie für die Sortierung von Zeichenfolgendaten auf Datenbank- oder Spaltenebene. Die Unterstützung wird auch für externe PolyBase-Tabellen und für Always Encrypted (sofern nicht mit Enclaves verwendet) bereitgestellt. Siehe [Sortierung und Unicode-Unterstützung](../relational-databases/collations/collation-and-unicode-support.md).|
| &nbsp; | &nbsp; |

### <a name="language-extensions"></a>Spracherweiterungen

|Neue Funktion oder Update | Details |
|:---|:---|
|Neues Java-Sprach-SDK | Vereinfacht die Entwicklung von Java-Programmen, die von SQL Server ausgeführt werden können. Siehe [Microsoft-SDK für Java-Erweiterung für SQL Server](../language-extensions/how-to/extensibility-sdk-java-sql-server.md). |
|Java-Sprach-SDK ist Open Source |Das [Microsoft-SDK für Java-Erweiterung für Microsoft SQL Server](../language-extensions/how-to/extensibility-sdk-java-sql-server.md) ist jetzt Open Source und [auf GitHub verfügbar](https://github.com/microsoft/sql-server-language-extensions).|
|Unterstützung für Java-Datentypen|Informationen finden Sie unter [Java-Datentypen](../language-extensions/how-to/java-to-sql-data-types.md).|
|Neue standardmäßige Java Runtime | SQL Server umfasst jetzt Zulu Embedded von Azul System für Java-Unterstützung im gesamten Produkt. Siehe [Kostenlose Java-Unterstützung in SQL Server 2019 jetzt verfügbar](https://cloudblogs.microsoft.com/sqlserver/2019/07/24/free-supported-java-in-sql-server-2019-is-now-available/). |
|SQL Server-Spracherweiterungen| Ausführung von externem Code mit dem Erweiterungsframework. Siehe [SQL Server-Spracherweiterungen](../language-extensions/language-extensions-overview.md).
|Registrieren externer Sprachen|`CREATE EXTERNAL LANGUAGE`, eine neue DDL (Data Definition Language, Datendefinitionssprache), registriert externe Sprachen wie beispielsweise Java in SQL Server. Informationen finden Sie unter [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md). |
| &nbsp; | &nbsp; |

### <a name="spatial"></a>Spatial

|Neue Funktion oder Update | Details |
|:---|:---|
| Neue SRIDs (Spatial Reference Identifiers) |Das [australische GDA2020](http://www.ga.gov.au/scientific-topics/positioning-navigation/geodesy/datums-projections/gda2020) stellt robustere und genauere Datumsangaben bereit, die stärker an GPS-Systemen ausgerichtet sind. Die neuen SRIDs sind:<ul><li>7843: geografische 2D-Darstellung</li><li>7844: geografische 3D-Darstellung</li></ul> Die Ansicht [sys.spatial_reference_systems](../relational-databases/system-catalog-views/sys-spatial-reference-systems-transact-sql.md) enthält Definitionen neuer SRIDs. |
| &nbsp; | &nbsp; |

### <a name="error-messages"></a>Fehlermeldungen
Wenn ein ETL-Prozess (Extrahieren, Transformieren und Laden) fehlschlägt, weil die Quelle und das Ziel nicht über übereinstimmende Datentypen und/oder -längen verfügen, ist die Problembehandlung bisher sehr zeitaufwändig, insbesondere bei großen Datasets. [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] ermöglicht schnellere Einblicke in Fehler durch abgeschnittene Daten.

|Neue Funktion oder Update | Details |
|:---|:---|
|Kürzungswarnungen | Fehlermeldungen bei abgeschnittenen Daten enthalten standardmäßig die Tabellen- und Spaltennamen sowie den abgeschnittenen Wert. Siehe [VERBOSE_TRUNCATION_WARNINGS](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#verbose-truncation).|
| &nbsp; | &nbsp; |

## <a name="mission-critical-security"></a>Unternehmenskritische Sicherheit
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] bietet eine Sicherheitsarchitektur, die es Datenbankadministratoren und -entwicklern ermöglicht, sichere Datenbankanwendungen zu erstellen und Bedrohungen zu bekämpfen. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] wurde mit jeder neuen Version durch die Einführung neuer Features und Funktionen verbessert, und dies gilt auch für [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].

|Neue Funktion oder Update | Details |
|:---|:---|
|Always Encrypted mit Secure Enclaves|Erweitert auf Always Encrypted mit direkter Verschlüsselung und umfangreichen Berechnungen, indem es Berechnungen von Klartextdaten innerhalb einer serverseitigen sicheren Enclave-Instanz ermöglicht. Die direkte Verschlüsselung verbessert die Leistung und die Zuverlässigkeit kryptografischer Vorgänge (Verschlüsseln von Spalten, Rotieren von Verschlüsselungsschlüsseln für Spalten usw.), da dadurch das Verschieben von Daten aus der Datenbank vermieden wird.<br><br> Durch die Unterstützung für umfangreiche Berechnungen (Musterabgleich und Vergleichsvorgänge) wird Always Encrypted für weitaus mehr Szenarios und Anwendungen entsperrt, die einen sensiblen Datenschutz erfordern, während gleichzeitig umfangreichere Funktionen in Transact-SQL-Abfragen erforderlich ist. Weitere Informationen finden Sie unter [Always Encrypted mit Secure Enclaves](../relational-databases/security/encryption/always-encrypted-enclaves.md).|
|Zertifikatverwaltung im SQL Server-Konfigurations-Manager|Im SQL Server-Konfigurations-Manager können Sie nun Zertifikate verwalten, d. h., Sie können sich z. B. Zertifikate ansehen und sie bereitstellen. Siehe [Zertifikatverwaltung (SQL Server-Konfigurations-Manager)](../database-engine/configure-windows/manage-certificates.md).|
|Datenermittlung und -klassifizierung|Dank der Datenermittlung und -klassifizierung können Sie Spalten in Benutzertabellen klassifizieren und mit Bezeichnungen versehen. Die Klassifizierung vertraulicher Daten (geschäftliche, finanzielle, gesundheitliche, personenbezogene Informationen, usw.) kann beim Informationsschutz in einem Unternehmen eine entscheidende Rolle spielen. Sie kann für Folgendes als Infrastruktur gelten:<ul><li>Unterstützung beim Einhalten von Datenschutzstandards und gesetzlich geregelten Complianceanforderungen</li><li>Verschiedene Sicherheitsszenarios, z. B. Überwachung und Benachrichtigung bei anomalem Zugriff auf vertrauliche Daten</li><li>Vereinfachung des Vorgangs der Ermittlung, wo sich vertrauliche Daten im Unternehmen befinden, damit Administratoren die richtigen Schritte zum Schutz der Datenbank durchführen können</li></ul>|
|SQL Server Audit|Die [Überwachung](../relational-databases/security/auditing/sql-server-audit-database-engine.md) wurde zudem dahingehend verbessert, dass der Überwachungsprotokolleintrag jetzt das neu Feld `data_sensitivity_information` enthält, das die Vertraulichkeitsklassifizierungen (Bezeichnungen) der tatsächlichen Daten protokolliert, die von der Abfrage zurückgegeben wurden. Detaillierte Informationen und Beispiele finden Sie unter [`ADD SENSITIVITY CLASSIFICATION`](../t-sql/statements/add-sensitivity-classification-transact-sql.md).|
| &nbsp; | &nbsp; |

## <a name="high-availability"></a>Hochverfügbarkeit
Eine allgemeine Aufgabe, die jeder berücksichtigen sollte, der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] bereitstellt, ist das Sicherstellen der Verfügbarkeit aller unternehmenskritischen [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Instanzen und der darin enthaltenen Datenbanken, wann immer das Unternehmen oder die Benutzer diese benötigen. Die Verfügbarkeit ist eine wichtige Säule der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Plattform, und [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] führt viele neue Features und Verbesserungen ein, mit denen Unternehmen sicherstellen können, dass ihre Datenbankumgebungen hochverfügbar sind.

### <a name="availability-groups"></a>Verfügbarkeitsgruppen

|Neue Funktion oder Update | Details |
|:---|:---|
|Bis zu fünf synchrone Replikate|In [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] wird die maximale Anzahl der synchronen Replikate von ehemals 3 in [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] auf 5 erhöht. Sie können diese Gruppe aus fünf Replikaten für das automatische Failover in der Gruppe konfigurieren. Es gibt ein primäres Replikat sowie vier synchrone sekundäre Replikate.|
|Umleiten der Verbindung vom sekundären zum primären Replikat| Durch dieses Feature können Clientanwendungsverbindungen zum primären Replikat weitergeleitet werden, unabhängig davon, ob der Zielserver in der Verbindungszeichenfolge angegeben ist. Weitere Informationen finden Sie unter [Umleitung von Lese-/Schreibverbindungen vom sekundären zum primären Replikat (Always On-Verfügbarkeitsgruppen)](../database-engine/availability-groups/windows/secondary-replica-connection-redirection-always-on-availability-groups.md).|
|HADR-Vorteile| Jeder Software Assurance-Kunde von SQL Server kann für jede SQL Server-Version, die noch von Microsoft unterstützt wird, drei erweiterte Vorteile nutzen. Einzelheiten dazu finden Sie unter [unsere Ankündigung](https://cloudblogs.microsoft.com/sqlserver/2019/10/30/new-high-availability-and-disaster-recovery-benefits-for-sql-server/).
| &nbsp; | &nbsp; |

### <a name="recovery"></a>Wiederherstellung

|Neue Funktion oder Update | Details |
|:---|:---|
|Verbesserte Wiederherstellung von Datenbanken | Verkürzen Sie mit der verbesserten Wiederherstellung von Datenbanken (ADR, Accelerated Database Recovery) die Zeitdauer für die Wiederherstellung nach einem Neustart oder einem langwierigen Transaktionsrollback. Siehe [Verbesserte Wiederherstellung von Datenbanken](../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#adr).|
| &nbsp; | &nbsp; |

### <a name="resumable-operations"></a>Fortsetzbare Vorgänge

|Neue Funktion oder Update | Details |
|:---|:---|
|Erstellen und Neuerstellen eines gruppierten Columnstore-Onlineindexes | Siehe [Ausführen von Onlineindexvorgängen](../relational-databases/indexes/perform-index-operations-online.md). |
|Erstellen eines fortsetzbaren Rowstore-Onlineindexes | Siehe [Ausführen von Onlineindexvorgängen](../relational-databases/indexes/perform-index-operations-online.md). |
|Anhalten und Fortsetzen der anfänglichen Überprüfung für Transparent Data Encryption (TDE)|Siehe [TDE-Überprüfung (Transparent Data Encryption): Anhalten und Fortsetzen](../relational-databases/security/encryption/transparent-data-encryption.md#scan-suspend-resume).|
| &nbsp; | &nbsp; |

## <a name="platform-choice"></a>Plattformauswahl
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] baut auf den Innovationen auf, die in [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] eingeführt wurden, sodass Sie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf Ihrer bevorzugten Plattform mit mehr Funktionen und Sicherheit als je zuvor ausführen können.

### <a name="linux"></a><a id="sql-server-on-linux"></a>Linux

| Neue Funktion oder Update | Details |
|:-----|:-----|
|Replikationsunterstützung | Siehe [SQL Server-Replikation unter Linux](../linux/sql-server-linux-replication.md). |
|Unterstützung für den Microsoft Distributed Transaction Coordinator (MSDTC) | Siehe [Konfigurieren von Microsoft Distributed Transaction Coordinator (MS DTC) unter Linux](../linux/sql-server-linux-configure-msdtc.md). |
|OpenLDAP-Unterstützung für AD-Drittanbieter | Siehe [Tutorial: Verwenden der Active Directory-Authentifizierung für SQL Server für Linux](../linux/sql-server-linux-active-directory-authentication.md). |
|Machine Learning Services unter Linux | Siehe [Installieren von SQL Server Machine Learning Services (Python und R) unter Linux](../linux/sql-server-linux-setup-machine-learning.md). |
|TempDB-Verbesserungen | Standardmäßig erstellt eine neue Installation von SQL Server für Linux mehrere TempDB-Datendateien, deren Anzahl sich nach der Anzahl der logischen Kerne richtet (bis zu acht Datendateien). Dies gilt nicht für direkte Upgrades der Neben- oder Hauptversion. Jede TempDB-Datei ist 8 MB groß und wird automatisch um 64 MB vergrößert. Dieses Verhalten ähnelt dem der SQL Server-Standardinstallation unter Windows. |
|PolyBase unter Linux | Siehe [Installieren von PolyBase unter Linux](../relational-databases/polybase/polybase-linux-setup.md) für Nicht-Hadoop-Connectors.<br/><br/>Siehe [Typzuordnung mit PolyBase](../relational-databases/polybase/polybase-type-mapping.md). |
| Unterstützung von Change Data Capture (CDC) | Change Data Capture (CDC) wird jetzt unter Linux für [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] unterstützt. |
| &nbsp; | &nbsp; |

### <a name="containers"></a>Container
Die einfachste Möglichkeit, um die Arbeit mit [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] aufzunehmen, ist die Verwendung von Containern. [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] baut auf den Innovationen auf, die in früheren Versionen eingeführt wurden, damit Sie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Container auf sicherere Weise und mit mehr Funktionen auf neuen Plattformen bereitstellen können.

|Neue Funktion oder Update | Details |
|:---|:---|
| Microsoft Container Registry | [Microsoft Container Registry](https://azure.microsoft.com/blog/microsoft-syndicates-container-catalog/) ersetzt nun Docker Hub für neue offizielle Microsoft-Containerimages, einschließlich [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]. |
| Container ohne Root-Berechtigung | Mit [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] wird die Möglichkeit eingeführt, sicherere Container zu erstellen, indem der [!INCLUDE[sql-server](../includes/ssnoversion-md.md)]-Prozess standardmäßig als Nicht-Root-Benutzer gestartet wird. Siehe [Erstellen und Ausführen von SQL Server-Containern als Nicht-Root-Benutzer](../linux/sql-server-linux-docker-container-deployment.md). |
| Red Hat-zertifizierte Containerimages | Ab [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] können Sie SQL Server-Container auf Red Hat Enterprise Linux ausführen. |
| Unterstützung für PolyBase und Machine Learning| [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] bietet neue Möglichkeiten, um mit SQL Server-Containern wie Machine Learning Services und PolyBase zu arbeiten. Sehen Sie sich einige Beispiele unter [SQL Server im GitHub-Repository des Containers](https://github.com/microsoft/mssql-docker/tree/master/linux/preview/examples) an. |
| &nbsp; | &nbsp; |

## <a name="setup-options"></a>Setupoptionen

|Neue Funktion oder Update | Details |
|:---|:---| 
|Neue Optionen für die Arbeitsspeichereinrichtung | Legt die Serverkonfigurationseinstellungen *Min. Serverarbeitsspeicher (MB)* und *Max. Serverarbeitsspeicher (MB)* während der Installation fest. Weitere Informationen finden Sie unter [Konfiguration der Datenbank-Engine – Seite „Arbeitsspeicher“](./install/instance-configuration.md#memory) und unter den Parametern `USESQLRECOMMENDEDMEMORYLIMITS`, `SQLMINMEMORY` und `SQLMAXMEMORY` im Artikel [Installieren von SQL Server von der Eingabeaufforderung](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#Install). Der vorgeschlagene Wert entspricht den Richtlinien für die Arbeitsspeicherkonfiguration in [Konfigurationsoptionen für den Serverarbeitsspeicher](../database-engine/configure-windows/server-memory-server-configuration-options.md#manually).| 
|Neue Optionen für die Nebenläufigkeitseinrichtung | Legt die Serverkonfigurationsoption *Max. Grad an Parallelität* während der Installation fest. Siehe [Konfiguration der Datenbank-Engine – Seite „MaxDOP“](./install/instance-configuration.md#maxdop) und den Abschnitt zum `SQLMAXDOP`-Parameter in [Installieren von SQL Server von der Eingabeaufforderung](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#Install). Der Standardwert entspricht den Richtlinien für die maximale Nebenläufigkeit in [Konfigurieren der Serverkonfigurationsoption „Max. Grad an Parallelität“](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines).| 
|Setupwarnung bei Produktschlüssel für Sever-/CAL-Lizenz|Wenn ein Produktschlüssel für eine Server-/Cal-Lizenz eingegeben wird und die Maschine mehr als 20 physische (oder 40 logische Kerne bei aktiviertem Hyper-Threading) hat, wird während des Setupvorgangs eine Warnung angezeigt. Benutzer haben nun die Wahl. Sie können wie bisher die Beschränkung zur Kenntnis nehmen und mit dem Setupvorgang fortfahren, oder sie geben einen Lizenzschlüssel ein, der die maximal mögliche Prozessorzahl des Systems unterstützt.|
| &nbsp; | &nbsp; |

## <a name="sql-server-machine-learning-services"></a><a id="ml"></a>SQL Server-Machine Learning-Dienste

|Neue Funktion oder Update | Details |
|:---|:---|
|Partitionsbasierte Modellierung | Sie können externe Skripts Ihrer Daten partitionsbasiert verarbeiten, indem Sie die neuen Parameter verwenden, die zu `sp_execute_external_script` hinzugefügt wurden. Diese Funktion unterstützt anstelle eines großen Modells das Trainieren von einer Vielzahl kleiner Modelle (ein Modell pro Datenpartition). Siehe [Erstellen von partitionsbasierten Modellen](../machine-learning/tutorials/r-tutorial-create-models-per-partition.md).|
|Windows Server-Failovercluster| Sie können Hochverfügbarkeit für Machine Learning Services in einem Windows Server-Failovercluster konfigurieren.|
| &nbsp; | &nbsp; |

## <a name="sql-server-analysis-services"></a>SQL Server Analysis Services

Mit dieser Version werden neue Features und Verbesserungen für Leistung, Ressourcenkontrolle und Clientunterstützung eingeführt.

| Neue Funktion oder Update | Details |
|:---|:---|
|Berechnungsgruppen in tabellarischen Modellen| Berechnungsgruppen können die Anzahl der redundanten Measures deutlich reduzieren, indem sie gängige Measure-Ausdrücke als *Berechnungselemente* gruppieren. Weitere Informationen finden Sie unter [Berechnungsgruppen in einem tabellarischen Modell](/analysis-services/tabular-models/calculation-groups). |
|Abfrageüberlappung| Die Abfrageüberlappung ist eine Systemkonfiguration im tabellarischen Modus, mit der die Antwortzeiten für Benutzerabfragen in Szenarios mit hoher Parallelität verbessert werden können. Weitere Informationen finden Sie unter [Abfrageüberlappung](/analysis-services/tabular-models/query-interleaving). |
|M:n-Beziehungen in tabellarischen Modellen| Ermöglicht m:n-Beziehungen zwischen Tabellen, in denen beide Spalten nicht eindeutig sind. Weitere Informationen finden Sie unter [Beziehungen in tabellarischen Modellen](/analysis-services/tabular-models/relationships-ssas-tabular).|
|Eigenschafteneinstellungen für die Ressourcenkontrolle| Diese Version enthält neue Speichereinstellungen: Memory\QueryMemoryLimit, DbpropMsmdRequestMemoryLimit und OLAP\Query\RowsetSerializationLimit für die Ressourcenkontrolle. Weitere Informationen finden Sie unter [Arbeitsspeicher-Eigenschaften](/analysis-services/server-properties/memory-properties).|
|Governanceeinstellung für Power BI-Cacheaktualisierungen | In dieser Version wird die Eigenschaft ClientCacheRefreshPolicy eingeführt, die das Zwischenspeichern von Dashboardkacheldaten und Berichtsdaten für das anfängliche Laden von Live Connect-Berichten durch den Power BI-Dienst überschreibt. Weitere Informationen finden Sie unter [Allgemeine Eigenschaften](/analysis-services/server-properties/general-properties). |
| Online anfügen  | Onlineanfügen kann für die Synchronisierung schreibgeschützter Replikate in horizontal skalierten Umgebungen für lokale Abfragen verwendet werden. Weitere Informationen finden Sie unter [Online anfügen](/analysis-services/what-s-new-in-sql-server-analysis-services#online-attach). |
| &nbsp; | &nbsp; |

## <a name="sql-server-integration-services"></a>SQL Server Integration Services

In dieser Version werden neue Funktionen eingeführt, um Dateivorgänge zu verbessern.

| Neue Funktion oder Update | Details |
|:---|:---|
|Flexibler Dateitask |Führen Sie Dateivorgänge im lokalen Dateisystem, Azure Blob Storage und Azure Data Lake Storage Gen2 durch. Weitere Informationen finden Sie unter [Flexibler Dateitask](../integration-services/control-flow/flexible-file-task.md).|
|Flexible Dateiquelle und flexibles Dateiziel |Lesen und Schreiben Sie Daten für Azure Blob Storage und Azure Data Lake Storage Gen2. Weitere Informationen finden Sie unter [Flexible Dateiquelle](../integration-services/data-flow/flexible-file-source.md) und [Flexibles Dateiziel](../integration-services/data-flow/flexible-file-destination.md). |

## <a name="sql-server-master-data-services"></a>SQL Server [!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)]

| Neue Funktion oder Update | Details |
|:---|:---|
|Unterstützung für Azure SQL Managed Instance-Datenbanken| Hosten von [!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)] auf Azure SQL Managed Instance. Informationen finden Sie unter [[!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)]-Installation und -Konfiguration](../master-data-services/master-data-services-installation-and-configuration.md#SetUpWeb).|
|Neue HTML-Steuerelemente| HTML-Steuerelemente ersetzen alle früheren Silverlight-Komponenten. Die Silverlight-Abhängigkeit wurde aufgehoben.|
| &nbsp; | &nbsp; |

## <a name="sql-server-reporting-services"></a>SQL Server Reporting Services

Diese Version von SQL Server Reporting Services bietet Unterstützung für verwaltete Azure SQL-Instanzen, Power BI Premium-Datasets, verbesserte Barrierefreiheit, Azure Active Directory-Anwendungsproxy und Transparent Data Encryption. Außerdem geht sie mit einem Update für Microsoft Berichts-Generator einher. Für ausführliche Informationen siehe [Neues in SQL Server Reporting Services (SSRS)](../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md).

## <a name="see-also"></a>Weitere Informationen

- [PowerShell-Modul von `SqlServer`](https://www.powershellgallery.com/packages/Sqlserver)
- [SQL Server PowerShell-Dokumentation](../powershell/sql-server-powershell.md)

## <a name="next-steps"></a>Nächste Schritte

- [SQL Server-Workshops](https://aka.ms/sqlworkshops)
- [Versionshinweise zu [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]](sql-server-version-15-release-notes.md)
- [Microsoft [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]: Technical white paper (technisches Whitepaper)](https://aka.ms/sql2019whitepaper)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
