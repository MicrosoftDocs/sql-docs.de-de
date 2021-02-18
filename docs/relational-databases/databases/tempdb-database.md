---
title: tempdb-Datenbank | Microsoft-Dokumentation
description: Dieses Thema bietet detaillierte Informationen zur Konfiguration und Verwendung der tempdb-Datenbank in SQL Server und Azure SQL-Datenbank.
ms.custom: P360
ms.date: 09/16/2020
ms.prod: sql
ms.prod_service: database-engine
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- temporary tables [SQL Server], tempdb database
- tempdb database [SQL Server], about tempdb
- temporary stored procedures [SQL Server]
- tempdb database [SQL Server]
ms.assetid: ce4053fb-e37a-4851-b711-8e504059a780
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0fa90cc172c07b7642ca937271fe8c6709b65ede
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489374"
---
# <a name="tempdb-database"></a>tempdb-Datenbank

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Die Systemdatenbank `tempdb` ist eine globale Ressource und steht allen Benutzern zur Verfügung, die mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank verbunden sind. `tempdb` enthält Folgendes:  
  
- Temporäre *Benutzerobjekte*, die explizit erstellt werden. Hierzu gehören globale oder lokale temporäre Tabellen und Indizes, temporäre gespeicherte Prozeduren, Tabellenvariablen, in Tabellenwertfunktionen zurückgegebene Tabellen und Cursor.  
- *Interne Objekte*, die von der Datenbank-Engine erstellt werden. Dazu gehören:
  - Arbeitstabellen, in denen direkte Ergebnisse für Spools, Cursor, Sortierungen und temporäre große Objektspeicher (LOB) gespeichert werden.
  - Arbeitsdateien für Hashjoin- oder Hashaggregatvorgänge.
  - Zwischenergebnisse von Sortierungen bei Vorgängen wie z. B. dem Erstellen oder Neuerstellen von Indizes (wenn `SORT_IN_TEMPDB` angegeben ist) oder bei bestimmten `GROUP BY`-, `ORDER BY`- oder `UNION`-Abfragen.

  Jedes interne Objekt verwendet mindestens neun Seiten: eine IAM-Seite (Index Allocation Map) und eine achtseitige Erweiterung. Weitere Informationen zu Seiten und Erweiterungen finden Sie unter [Seiten und Blöcke](../../relational-databases/pages-and-extents-architecture-guide.md#pages-and-extents).

  > [!IMPORTANT]
  > Azure SQL-Datenbank-Singletons und Pools für elastische Datenbanken unterstützen globale temporäre Tabellen und globale temporär gespeicherte Prozeduren, die in `tempdb` gespeichert werden und für die Datenbankebene gelten. 
  >
  > Globale temporäre Tabellen und globale temporäre gespeicherte Prozeduren sind für alle Benutzersitzungen innerhalb derselben SQL-Datenbank freigegeben. Benutzersitzungen aus anderen SQL-Datenbanken können nicht auf globale temporäre Tabellen zugreifen. Weitere Informationen finden Sie unter [Database scoped global temporary tables (Azure SQL Database) (Globale temporäre Tabellen auf Datenbankebene (Azure SQL-Datenbank))](../../t-sql/statements/create-table-transact-sql.md#database-scoped-global-temporary-tables-azure-sql-database). [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) unterstützt dieselben temporären Objekte wie SQL Server.
  >
  > Für Azure SQL-Datenbank-Singletons und -Pools für elastische Datenbanken sind nur die master- und die `tempdb`-Datenbank gültig. Weitere Informationen finden Sie unter [Was ist ein Server in Azure SQL-Datenbank und Azure Synapse Analytics?](/azure/sql-database/sql-database-servers-databases#what-is-an-azure-sql-database-server). Details zu `tempdb` im Kontext von Azure SQL-Datenbank-Singletons und -Pools für elastische Datenbanken finden Sie unter [tempdb-Datenbank in SQL-Datenbank](#tempdb-database-in-sql-database). 
  >
  > Für Azure SQL Managed Instance gelten alle Systemdatenbanken.

- *Versionsspeicher*. Dies sind Sammlungen von Datenseiten, in denen die Datenzeilen zur Unterstützung von Features für die Zeilenversionsverwaltung gespeichert werden. Es gibt zwei Speichertypen: einen allgemeinen Versionsspeicher und einen Versionsspeicher für die Online-Indexerstellung. Die Versionsspeicher beinhalten Folgendes:
  - Zeilenversionen, die von Datenänderungstransaktionen in einer Datenbank generiert werden, die `READ COMMITTED` durch Isolation der Zeilenversionsverwaltung oder durch Transaktionen der Momentaufnahmeisolation verwendet.  
  - Zeilenversionen, die von Datenänderungstransaktionen für Features wie z. B. die folgenden generiert werden: Online-Indexvorgänge, mehrere aktive Resultsets (MARS) und `AFTER`-Trigger.  
  
Vorgänge in `tempdb` werden minimal protokolliert, sodass ein Rollback für Transaktionen ausgeführt werden kann. `tempdb` wird bei jedem Start von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] neu erstellt, sodass das System immer mit einer bereinigten Kopie der Datenbank startet. Temporäre Tabellen und gespeicherte Prozeduren werden beim Trennen der Verbindung automatisch gelöscht; es sind keine Verbindungen aktiv, wenn das System heruntergefahren wird. 

Zwischen einzelnen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sitzungen wird also niemals etwas in `tempdb` gespeichert. Sicherungs- und Wiederherstellungsvorgänge sind für `tempdb` nicht zulässig.  

## <a name="physical-properties-of-tempdb-in-sql-server"></a>Physische Eigenschaften von tempdb in SQL Server

In der folgenden Tabelle sind die anfänglichen Konfigurationswerte der Daten- und Protokolldateien von `tempdb` in SQL Server aufgelistet. Diese Werte basieren auf den Standardwerten für die `model`-Datenbank. Die Größe dieser Dateien kann sich in den verschiedenen Editionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]geringfügig unterscheiden.  
  
|Datei|Logischer Name (logical name)|Physischer Name (physical name)|Ursprüngliche Größe|Dateivergrößerung (file growth)|  
|----------|------------------|-------------------|------------------|-----------------|  
|Primäre Daten|tempdev|tempdb.mdf|8 Megabytes|Automatische Vergrößerung um 64 MB, bis der Speicherplatz auf dem Datenträger erschöpft ist|  
|Sekundäre Datendateien|temp#|tempdb_mssql_#.ndf|8 Megabytes|Automatische Vergrößerung um 64 MB, bis der Speicherplatz auf dem Datenträger erschöpft ist|  
|Log|templog|templog.ldf|8 Megabytes|Automatische Vergrößerung um 64 MB, bis der Maximalwert von 2 TB erreicht wird|  
  
Die Anzahl von sekundären Datendateien richtet sich nach der Anzahl der (logischen) Prozessoren auf dem Computer. Als allgemeine Regel gilt: Verwenden Sie die Anzahl von Datendateien, die der Anzahl von logischen Prozessoren entspricht, falls die Anzahl von logischen Prozessoren acht oder weniger beträgt. Wenn mehr als acht logische Prozessoren vorhanden sind, verwenden Sie acht Datendateien. Sollte weiterhin ein Konflikt bestehen, erhöhen Sie die Anzahl von Datendateien um ein Vielfaches von vier, bis der Konflikt auf ein akzeptables Ausmaß reduziert ist. Alternativ dazu können Sie auch die Arbeitsauslastung oder den Code ändern.

> [!NOTE]
> Der Standardwert für die Anzahl der Datendateien basiert auf den allgemeinen Richtlinien in [KB 2154845](https://support.microsoft.com/kb/2154845/).  

> [!NOTE]
> Fragen Sie die Sicht `tempdb.sys.database_files` ab, um die aktuelle Größe und die Vergrößerungsparameter von `tempdb` zu überprüfen.
  
### <a name="moving-the-tempdb-data-and-log-files-in-sql-server"></a>Verschieben der tempdb-Daten- und -Protokolldateien in SQL Server

Informationen zum Verschieben der Daten- und Protokolldateien von `tempdb` finden Sie unter [Verschieben von Systemdatenbanken](../../relational-databases/databases/move-system-databases.md).  
  
### <a name="database-options-for-tempdb-in-sql-server"></a>Datenbankoptionen für tempdb in SQL Server

In der folgenden Tabelle werden die Standardwerte für alle einzelnen Datenbankoptionen der Datenbank `tempdb` aufgeführt und, ob die Option geändert werden kann. Zum Anzeigen der aktuellen Einstellungen dieser Optionen verwenden Sie die Katalogsicht [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) .  
  
|Datenbankoption|Standardwert|Kann geändert werden.|  
|---------------------|-------------------|---------------------|  
|ALLOW_SNAPSHOT_ISOLATION|OFF|Ja|  
|ANSI_NULL_DEFAULT|OFF|Ja|  
|ANSI_NULLS|OFF|Ja|  
|ANSI_PADDING|OFF|Ja|  
|ANSI_WARNINGS|OFF|Ja|  
|ARITHABORT|OFF|Ja|  
|AUTO_CLOSE|OFF|Nein|  
|AUTO_CREATE_STATISTICS|EIN|Ja|  
|AUTO_SHRINK|OFF|Nein|  
|AUTO_UPDATE_STATISTICS|EIN|Ja|  
|AUTO_UPDATE_STATISTICS_ASYNC|OFF|Ja|  
|CHANGE_TRACKING|OFF|Nein|  
|CONCAT_NULL_YIELDS_NULL|OFF|Ja|  
|CURSOR_CLOSE_ON_COMMIT|OFF|Ja|  
|CURSOR_DEFAULT|GLOBAL|Ja|  
|Datenbankverfügbarkeitsoptionen|ONLINE<br /><br /> MULTI_USER<br /><br /> READ_WRITE|Nein<br /><br /> Nein<br /><br /> Nein|  
|DATE_CORRELATION_OPTIMIZATION|OFF|Ja|  
|DB_CHAINING|EIN|Nein|  
|ENCRYPTION|OFF|Nein|  
|MIXED_PAGE_ALLOCATION|OFF|Nein|  
|NUMERIC_ROUNDABORT|OFF|Ja|  
|PAGE_VERIFY|CHECKSUM für neue Installationen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> NONE für Upgrades von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Ja|  
|PARAMETERIZATION|SIMPLE|Ja|  
|QUOTED_IDENTIFIER|OFF|Ja|  
|READ_COMMITTED_SNAPSHOT|OFF|Nein|  
|RECOVERY|SIMPLE|Nein|  
|RECURSIVE_TRIGGERS|OFF|Ja|  
|Service Broker-Optionen|ENABLE_BROKER|Ja|  
|TRUSTWORTHY|OFF|Nein|  
  
Eine Beschreibung dieser Datenbankoptionen finden Sie unter [ALTER DATABASE SET-Optionen (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
## <a name="tempdb-database-in-sql-database"></a>tempdb-Datenbank in SQL-Datenbank

### <a name="tempdb-sizes-for-dtu-based-service-tiers"></a>Tempdb-Größen für DTU-basierte Diensttarife

<!-- tempdb being larger for Basic and 50 eDTU pools than for 100-400 eDTU pools reflects actual config (historical reasons) --> 

|Servicelevelziel|Maximale Dateigröße (GB) in `tempdb`|Anzahl von Datendateien in `tempdb`|Maximale Datengröße (GB) in `tempdb`|
|---|---:|---:|---:|
|Basic|13,9|1|13,9|
|S0|13,9|1|13,9|
|S1|13,9|1|13,9|
|S2|13,9|1|13,9|
|S3|32|1|32
|S4|32|2|64|
|S6|32|3|96|
|S7|32|6|192|
|S9|32|12|384|
|S12|32|12|384|
|P1|13,9|12|166,7|
|P2|13,9|12|166,7|
|P4|13,9|12|166,7|
|P6|13,9|12|166,7|
|P11|13,9|12|166,7|
|P15|13,9|12|166,7|
|Elastischer Basic-Pool (alle DTU-Konfigurationen)|13,9|12|166,7|
|Elastischer Standard-Pool (50 eDTU)|13,9|12|166,7|
|Elastischer Standard-Pool (100 eDTU)|32|1|32|
|Elastischer Standard-Pool (200 eDTU)|32|2|64|
|Elastischer Standard-Pool (300 eDTU)|32|3|96|
|Elastischer Standard-Pool (400 eDTU)|32|3|96|
|Elastischer Standard-Pool (800 eDTU)|32|6|192|
|Elastischer Standard-Pool (1.200 eDTU)|32|10|320|
|Elastischer Standard-Pool (1.600-3.000 eDTU)|32|12|384|
|Elastischer Premium-Pool (alle DTU-Konfigurationen)|13,9|12|166,7|
||||

### <a name="tempdb-sizes-for-vcore-based-service-tiers"></a>Tempdb-Größen für auf virtuellen Kern basierende Diensttarife

Informationen finden Sie unter [Ressourcenlimits beim auf virtuellen Kernen basierenden Kaufmodell](/azure/sql-database/sql-database-vcore-resource-limits).

## <a name="restrictions"></a>Beschränkungen

Die folgenden Vorgänge können in der `tempdb`-Datenbank nicht ausgeführt werden:  
  
- Hinzufügen von Dateigruppen.
- Sichern und Wiederherstellen der Datenbank.
- Ändern der Sortierung. Die Standardsortierung entspricht der Serversortierung.
- Ändern des Datenbankbesitzers `tempdb` befindet sich im Besitz von *sa*.
- Erstellen einer Datenbankmomentaufnahme.
- Löschen der Datenbank.
- Löschen des *guest* -Benutzers aus der Datenbank.
- Aktivieren von Change Data Capture.
- Teilnehmen an der Datenbankspiegelung.
- Entfernen der primären Dateigruppe, der primären Datendatei oder der Protokolldatei.
- Umbenennen der Datenbank oder der primären Dateigruppe.
- Ausführen von `DBCC CHECKALLOC`.
- Ausführen von `DBCC CHECKCATALOG`.
- Festlegen der Datenbank auf `OFFLINE`.
- Festlegen der Datenbank oder primären Dateigruppe auf `READ_ONLY`.
  
## <a name="permissions"></a>Berechtigungen

Jeder Benutzer kann temporäre Objekte in `tempdb` erstellen. Benutzer haben nur Zugriff auf ihre eigenen Objekte, es sei denn, ihnen wurden zusätzliche Berechtigungen zugewiesen. Es ist möglich, die Berechtigung zum Herstellen einer Verbindung mit `tempdb` zu widerrufen, um einen Benutzer an der Verwendung von `tempdb` zu hindern. Dies wird jedoch nicht empfohlen, da die Verwendung von `tempdb` für einige Routinevorgänge erforderlich ist.  

## <a name="optimizing-tempdb-performance-in-sql-server"></a>Optimieren der Leistung von tempdb in SQL Server
Die Größe und die physische Platzierung der `tempdb`-Datenbank kann sich auf die Leistung eines Systems auswirken. Ein Beispiel: Wenn eine zu geringe Größe für `tempdb` definiert wurde, muss bei jedem Neustart der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz möglicherweise ein Teil der Systemverarbeitungslast dafür aufgewendet werden, die `tempdb`-Datenbank automatisch auf den Umfang zu vergrößern, der zum Unterstützen der anfallenden Arbeitsauslastung erforderlich ist.

Verwenden Sie nach Möglichkeit die [schnelle Dateiinitialisierung](../../relational-databases/databases/database-instant-file-initialization.md), um die Leistung von Vergrößerungsvorgängen für Datendateien zu verbessern.

Weisen Sie allen `tempdb`-Dateien im Voraus Speicherplatz zu, indem Sie die Dateigröße auf einen Wert festlegen, der hoch genug ist, um der üblichen Arbeitsauslastung in der Umgebung gerecht zu werden. Durch die Vorabzuordnung wird verhindert, dass `tempdb` zu häufig vergrößert und die Leistung dadurch beeinträchtigt wird. Für die `tempdb`-Datenbank sollte die automatische Vergrößerung festgelegt werden, um den Speicherplatz für nicht geplante Ausnahmen zu erhöhen.

Datendateien sollten in jeder [Dateigruppe](../../relational-databases/databases/database-files-and-filegroups.md#filegroups) die gleiche Größe aufweisen, da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen Algorithmus für das proportionale Auffüllen verwendet, in dem Zuteilungen in Dateien mit mehr freiem Platz bevorzugt werden. Ein Aufteilen von `tempdb` in mehrere Datendateien gleicher Größe bietet einen hohen Grad an paralleler Effizienz in Vorgängen, in denen `tempdb` verwendet wird.

Legen Sie das Inkrement für die Dateivergrößerung auf eine sinnvolle Größe fest, damit die Vergrößerung der `tempdb`-Datenbankdateien nicht zu gering ausfällt. Wenn die Vergrößerung im Vergleich zur Menge der in `tempdb` geschriebenen Daten zu gering ist, muss `tempdb` möglicherweise ständig vergrößert werden. Dies beeinträchtigt die Leistung.

Verwenden Sie die folgende Abfrage, um die aktuelle Größe und die Vergrößerungsparameter von `tempdb` zu überprüfen:

```sql
 SELECT name AS FileName,
    size*1.0/128 AS FileSizeInMB,
    CASE max_size
        WHEN 0 THEN 'Autogrowth is off.'
        WHEN -1 THEN 'Autogrowth is on.'
        ELSE 'Log file grows to a maximum size of 2 TB.'
    END,
    growth AS 'GrowthValue',
    'GrowthIncrement' =
        CASE
            WHEN growth = 0 THEN 'Size is fixed.'
            WHEN growth > 0 AND is_percent_growth = 0
                THEN 'Growth value is in 8-KB pages.'
            ELSE 'Growth value is a percentage.'
        END
FROM tempdb.sys.database_files;
GO
```

Platzieren Sie die `tempdb`-Datenbank auf einem schnellen E/A-Subsystem. Verwenden Sie Datenträgerstriping, wenn viele Datenträger direkt angeschlossen sind. Einzelne oder Gruppen von `tempdb`-Datendateien müssen nicht unbedingt auf verschiedenen Datenträgern oder Spindeln gespeichert sein, es sei denn, Sie stellen außerdem E/A-Engpässe fest.

Platzieren Sie die `tempdb`-Datenbank nicht auf denselben Datenträgern, die auch von Benutzerdatenbanken genutzt werden.

## <a name="performance-improvements-in-tempdb-for-sql-server"></a>Leistungsverbesserungen in tempdb für SQL Server
Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] wird die Leistung von `tempdb` auf folgende Weise weiter optimiert:  
  
- Temporäre Tabellen und Tabellenvariablen werden zwischengespeichert. Das Zwischenspeichern ermöglicht eine sehr schnelle Ausführung der Vorgänge zum Löschen und Erstellen der temporären Objekte. Zudem reduziert es Konflikte bei Seitenzuordnung und Metadaten.  
- Das Latchprotokoll für Zuordnungsseiten wurde verbessert, um die Anzahl der verwendeten `UP`-Latches (Updatelatches) zu verringern.  
- Der Protokollierungsaufwand für `tempdb` wurde verringert, um die E/A-Bandbreite des Datenträgers für die `tempdb`-Protokolldatei zu reduzieren.  
- Das Setup fügt während der Installation einer neuen Instanz mehrere `tempdb`-Datendateien hinzu. Für diese Aufgabe können Sie die neue Eingabesteuerung der Benutzeroberfläche im Abschnitt **Datenbank-Engine-Konfiguration** und den Befehlszeilenparameter `/SQLTEMPDBFILECOUNT` verwenden. Standardmäßig fügt das Setup die Anzahl von `tempdb`-Datendateien hinzu, die der Anzahl von logischen Prozessoren entspricht, höchstens jedoch acht.  
- Wenn mehrere `tempdb`-Datendateien vorhanden sind, werden alle Dateien je nach Wachstumseinstellungen automatisch gleichzeitig und um denselben Wert vergrößert. [Ablaufverfolgungsflag 1117](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) ist nicht mehr erforderlich.  
- Alle Zuordnungen in `tempdb` verwenden einheitliche Erweiterungen. [Ablaufverfolgungsflag 1118](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) ist nicht mehr erforderlich.  
- Für die primäre Dateigruppe ist die Eigenschaft `AUTOGROW_ALL_FILES` aktiviert. Diese kann nicht geändert werden.

Weitere Informationen zu Leistungsverbesserungen in `tempdb` finden Sie im Blogbeitrag [TEMPDB - Files and Trace Flags and Updates, Oh My!](/archive/blogs/sql_server_team/tempdb-files-and-trace-flags-and-updates-oh-my) (TEMPDB – Dateien und Ablaufverfolgungsflags und Updates, o je!).

## <a name="memory-optimized-tempdb-metadata"></a>Speicheroptimierte tempdb-Metadaten
Bislang stellten Metadatenkonflikte in `tempdb` einen Engpass für die Skalierbarkeit vieler Workloads dar, die in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wurden. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] führt ein neues Feature ein, das zur [In-Memory Database](../in-memory-database.md)-Featurefamilie gehört: speicheroptimierte tempdb-Metadaten. 

Dieses Feature beseitigt diesen Engpass und ermöglicht ein neues Maß an Skalierbarkeit für tempdb-intensive Arbeitsauslastungen. In [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] können die Systemtabellen, die an der Verwaltung von Metadaten temporärer Tabellen beteiligt sind, in nicht dauerhafte speicheroptimierte Tabellen ohne Latches verschoben werden.

In diesem siebenminütigen Video erhalten Sie einen Überblick darüber, wann und wie speicheroptimierte tempdb-Metadaten verwendet werden sollten:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/How-and-When-To-Memory-Optimized-TempDB-Metadata/player?WT.mc_id=dataexposed-c9-niner]


### <a name="configuring-and-using-memory-optimized-tempdb-metadata"></a>Konfigurieren und Verwenden von speicheroptimierten tempdb-Metadaten

Mit dem folgenden Skript können Sie dieses neue Feature aktivieren:

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON;
```

Damit diese Konfigurationsänderung wirksam wird, muss der Dienst neu gestartet werden.

Mit dem folgenden T-SQL-Befehl können Sie überprüfen, ob `tempdb` speicheroptimiert ist:

```sql
SELECT SERVERPROPERTY('IsTempdbMetadataMemoryOptimized');
```

Wenn nach dem Aktivieren von speicheroptimierten `tempdb`-Metadaten der Server aus irgendeinem Grund nicht startet, können Sie das Feature umgehen, indem Sie die SQL Server-Instanz über die Startoption **-f** in der [Minimalkonfiguration](../../database-engine/configure-windows/start-sql-server-with-minimal-configuration.md) starten. Dann können Sie das Feature deaktivieren und SQL Server im normalen Modus neu starten.

Zum Schutz des Servers vor potenziellen Bedingungen mit nicht genügendem Arbeitsspeicher können Sie `tempdb` an einen [Ressourcenpool](../in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md) binden. Dies erfolgt über den Befehl [`ALTER SERVER`](../../t-sql/statements/alter-server-configuration-transact-sql.md) anstelle der Schritte, die Sie normalerweise befolgen, um einen Ressourcenpool an eine Datenbank zu binden.

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON (RESOURCE_POOL = 'pool_name');
```

Diese Änderung erfordert außerdem einen Neustart, um wirksam zu werden, selbst dann, wenn die speicheroptimierten tempdb-Metadaten bereits aktiviert sind.

### <a name="memory-optimized-tempdb-limitations"></a>Einschränkungen von speicheroptimierten tempdb-Metadaten

- Das Ein- und Ausschalten dieser Funktion ist nicht dynamisch. Aufgrund der systeminternen Änderungen, die an der Struktur von `tempdb` vorgenommen werden müssen, ist ein Neustart erforderlich, um das Feature zu aktivieren oder zu deaktivieren.

- Eine einzelne Transaktion darf nicht auf speicheroptimierte Tabellen in mehreren Datenbanken zugreifen. Bei Transaktionen, an denen eine speicheroptimierte Tabelle in einer Benutzerdatenbank beteiligt ist, ist ein Zugriff auf `tempdb`-Systemsichten nicht innerhalb derselben Transaktion möglich. Wenn Sie versuchen, in derselben Transaktion auf `tempdb`-Systemsichten zuzugreifen, wird die folgende Fehlermeldung angezeigt:
    
  ```
  A user transaction that accesses memory optimized tables or natively compiled modules cannot access more than one user database or databases model and msdb, and it cannot write to master.
  ```
    
  Beispiel:
    
  ```sql
  BEGIN TRAN;
  
  SELECT *
  FROM tempdb.sys.tables;  -----> Creates a user in-memory OLTP transaction in tempdb
  
  INSERT INTO <user database>.<schema>.<mem-optimized table>
  VALUES (1); ----> Tries to create a user in-memory OLTP transaction in the user database but will fail
  
  COMMIT TRAN;
  ```
    
- Abfragen in speicheroptimierten Tabellen unterstützen keine Sperr- und Isolationshinweise, daher werden diese Hinweise bei Abfragen in speicheroptimierten `tempdb`-Katalogsichten nicht berücksichtigt. Ebenso wie bei anderen Systemkatalogsichten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erfolgen alle Transaktionen für Systemsichten in `READ COMMITTED`-Isolation (bzw. in diesem Fall in `READ COMMITTED SNAPSHOT`-Isolation).

- [Columnstore-Indizes](../indexes/columnstore-indexes-overview.md) können in temporären Tabellen nicht erstellt werden, wenn speicheroptimierte `tempdb`-Metadaten aktiviert sind.

- Aufgrund der Einschränkung für Columnstore-Indizes wird die Verwendung der gespeicherten Systemprozedur `sp_estimate_data_compression_savings` mit dem Datenkomprimierungsparameter `COLUMNSTORE` oder `COLUMNSTORE_ARCHIVE` nicht unterstützt, wenn speicheroptimierte `tempdb`-Metadaten aktiviert sind.

> [!NOTE] 
> Diese Einschränkungen kommen nur beim Verweisen auf `tempdb`-Systemsichten zum Tragen. Sie können bei Bedarf eine temporäre Tabelle in derselben Transaktion erstellen, wenn Sie auf eine speicheroptimierte Tabelle in einer Benutzerdatenbank zugreifen.

## <a name="capacity-planning-for-tempdb-in-sql-server"></a>Kapazitätsplanung für tempdb in SQL Server
Das Festlegen der angemessenen Größe von `tempdb` in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Produktionsumgebung hängt von vielen Faktoren ab. Wie bereits erläutert, gehören die vorhandene Arbeitsauslastung und die verwendeten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Features zu diesen Faktoren. Es wird empfohlen, die vorhandene Workload durch Ausführen folgender Aufgaben in einer SQL Server-Testumgebung zu analysieren:

- Legen Sie die automatische Vergrößerung für `tempdb` fest.
- Führen Sie einzelne Abfragen oder Ablaufverfolgungsdateien für die Arbeitsauslastung aus, und überwachen Sie die Speicherplatzbelegung von `tempdb`.
- Führen Sie Indexverwaltungsvorgänge aus – z. B. die Neuerstellung von Indizes –, und überwachen Sie den `tempdb`-Speicherplatz.
- Verwenden Sie die Werte der Speicherplatzbelegung aus den vorherigen Schritten, um die gesamte Arbeitsauslastung zu prognostizieren. Passen Sie diesen Wert an die veranschlagten gleichzeitigen Aktivitäten an, und legen Sie dann die Größe von `tempdb` entsprechend fest.

## <a name="monitoring-tempdb-use"></a>Überwachen der tempdb-Nutzung
Unzureichender Speicherplatz in `tempdb` kann erhebliche Unterbrechungen in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Produktionsumgebung verursachen. Dieses Problem kann auch dazu führen, dass Anwendungen Vorgänge nicht abschließen können. Mit der dynamischen Verwaltungssicht [sys.dm_db_file_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md) können Sie den in den `tempdb`-Dateien verwendeten Speicherplatz überwachen:

```sql
 -- Determining the amount of free space in tempdb
SELECT SUM(unallocated_extent_page_count) AS [free pages],
  (SUM(unallocated_extent_page_count)*1.0/128) AS [free space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by the version store
SELECT SUM(version_store_reserved_page_count) AS [version store pages used],
  (SUM(version_store_reserved_page_count)*1.0/128) AS [version store space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by internal objects
SELECT SUM(internal_object_reserved_page_count) AS [internal object pages used],
  (SUM(internal_object_reserved_page_count)*1.0/128) AS [internal object space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by user objects
SELECT SUM(user_object_reserved_page_count) AS [user object pages used],
  (SUM(user_object_reserved_page_count)*1.0/128) AS [user object space in MB]
FROM sys.dm_db_file_space_usage;
 ```

Darüber hinaus können Sie die dynamischen Verwaltungssichten [sys.dm_db_session_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md) und [sys.dm_db_task_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md) verwenden, um die Seitenzuordnung und die Zuordnungsaufhebung in `tempdb` auf der Sitzungs- oder Taskebene zu überwachen. Mit diesen Sichten können Sie umfangreiche Abfragen, temporäre Tabellen oder Tabellenvariablen identifizieren, die sehr viel Speicherplatz in `tempdb` belegen. Sie können auch verschiedene Leistungsindikatoren verwenden, um den in `tempdb` verfügbaren freien Speicherplatz sowie die Ressourcen zu überwachen, die `tempdb` verwenden.

```sql
-- Obtaining the space consumed by internal objects in all currently running tasks in each session
SELECT session_id,
  SUM(internal_objects_alloc_page_count) AS task_internal_objects_alloc_page_count,
  SUM(internal_objects_dealloc_page_count) AS task_internal_objects_dealloc_page_count
FROM sys.dm_db_task_space_usage
GROUP BY session_id;

-- Obtaining the space consumed by internal objects in the current session for both running and completed tasks
SELECT R2.session_id,
  R1.internal_objects_alloc_page_count
  + SUM(R2.internal_objects_alloc_page_count) AS session_internal_objects_alloc_page_count,
  R1.internal_objects_dealloc_page_count
  + SUM(R2.internal_objects_dealloc_page_count) AS session_internal_objects_dealloc_page_count
FROM sys.dm_db_session_space_usage AS R1
INNER JOIN sys.dm_db_task_space_usage AS R2 ON R1.session_id = R2.session_id
GROUP BY R2.session_id, R1.internal_objects_alloc_page_count,
  R1.internal_objects_dealloc_page_count;;
```

## <a name="related-content"></a>Verwandte Inhalte
[SORT_IN_TEMPDB-Option für Indizes](../../relational-databases/indexes/sort-in-TempDB-option-for-indexes.md)    
[Systemdatenbanken](../../relational-databases/databases/system-databases.md)    
[sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)    
[sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)    
[Verschieben von Datenbankdateien](../../relational-databases/databases/move-database-files.md)    
