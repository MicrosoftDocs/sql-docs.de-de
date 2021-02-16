---
title: Initialisieren einer Verfügbarkeitsgruppe mithilfe von automatischem Seeding
description: Erstellen Sie mithilfe von automatischem Seeding automatisch sekundäre Replikate für jede Datenbank in einer Always On-Verfügbarkeitsgruppe, ohne diese manuell sichern und wiederherstellen zu müssen.
ms.custom: seo-lt-2019
ms.date: 03/26/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 67c6a601-677a-402b-b3d1-8c65494e9e96
author: cawrites
ms.author: chadam
ms.openlocfilehash: 13af5b044bfc7f3242e5e69e297b1fe0a9371ac1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340927"
---
# <a name="use-automatic-seeding-to-initialize-an-always-on-availability-group"></a>Verwenden von automatischem Seeding zum Initialisieren einer Always On-Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Mit SQL Server 2016 wurde das automatische Seeding von Verfügbarkeitsgruppen eingeführt. Beim Erstellen einer Verfügbarkeitsgruppe mit automatischem Seeding erstellt SQL Server automatisch die sekundären Replikate für jede Datenbank in der Gruppe. Sie müssen sekundäre Replikate nicht mehr manuell sichern und wiederherstellen. Erstellen Sie zum Aktivieren des automatischen Seedings die Verfügbarkeitsgruppe mit T-SQL, oder verwenden Sie die neueste Version von SQL Server Management Studio.

Weitere Hintergrundinformationen finden Sie unter [Automatisches Seeding für sekundäre Replikate](automatic-seeding-secondary-replicas.md).
 
## <a name="prerequisites"></a>Voraussetzungen

In SQL Server 2016 erfordert automatisches Seeding, dass der Pfad für Daten- und Protokolldateien für jede SQL Server-Instanz der Verfügbarkeitsgruppe identisch ist. In SQL Server 2017 können Sie unterschiedliche Pfade verwenden, Microsoft empfiehlt jedoch, dieselben Pfade zu verwenden, wenn alle Replikate auf derselben Plattform (z.B. Windows oder Linux) gehostet werden. Plattformübergreifende Verfügbarkeitsgruppen besitzen unterschiedliche Pfade für die Replikate. Weitere Informationen finden Sie unter [Datenträgerlayout](automatic-seeding-secondary-replicas.md#disklayout).

Verfügbarkeitsgruppenseeding kommuniziert über den Datenbankspiegelungs-Endpunkt. Öffnen Sie Eingangsfirewallregeln für den Port für den Spiegelungsendpunkt auf jedem Server.

Datenbanken in einer Verfügbarkeitsgruppe müssen sich im vollständigen Wiederherstellungsmodell befinden. Die Datenbank muss über eine aktuelle vollständige Sicherung und eine aktuelle Transaktionsprotokollsicherung verfügen. Diese Sicherungsdateien werden nicht für das automatische Seeding verwendet, aber sie sind erforderlich, bevor die Datenbank einer Verfügbarkeitsgruppe hinzugefügt wird. 
 
## <a name="create-availability-group-with-automatic-seeding"></a>Erstellen einer Verfügbarkeitsgruppe mit automatischem Seeding

Zum Erstellen einer Verfügbarkeitsgruppe mit automatischem Seeding legen Sie `SEEDING_MODE=AUTOMATIC`fest. 

Im folgenden Beispiel wird eine Verfügbarkeitsgruppe auf einem Windows Server-Failovercluster mit zwei Knoten erstellt. Aktualisieren Sie vor dem Ausführen der Skripts die Werte für Ihre Umgebung.

1. Erstellen Sie die Endpunkte. Jeder Server benötigt einen Endpunkt. Das folgende Skript erstellt einen Endpunkt, der TCP-Port 5022 für den Listener verwendet. Legen Sie `<endpoint_name>` und `LISTENER_PORT` entsprechend Ihrer Umgebung fest, und führen Sie das Skript auf beiden Servern aus:

    ```sql
    CREATE ENDPOINT [<endpoint_name>] 
        STATE=STARTED
        AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)
        FOR DATA_MIRRORING (
            ROLE = ALL, 
            AUTHENTICATION = WINDOWS NEGOTIATE, 
            ENCRYPTION = REQUIRED ALGORITHM AES
            )
    GO
    ```

1. Erstellen Sie die Verfügbarkeitsgruppe. Das folgende Skript erstellt die Verfügbarkeitsgruppe. Aktualisieren Sie die Werte für den Gruppennamen, die Servernamen und die Domänennamen in spitzen Klammern `<>`, und führen Sie das Skript auf der primären Instanz von SQL Server aus.  

    ```sql
    CREATE AVAILABILITY GROUP [<availability_group_name>]
        FOR DATABASE db1
        REPLICA ON'<*primary_server*>'
        WITH (ENDPOINT_URL = N'TCP://<primary_server>.<fully_qualified_domain_name>:5022', 
            FAILOVER_MODE = AUTOMATIC, 
            AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
            BACKUP_PRIORITY = 50, 
            SECONDARY_ROLE(ALLOW_CONNECTIONS = NO), 
            SEEDING_MODE = AUTOMATIC),
        N'<secondary_server>' WITH (ENDPOINT_URL = N'TCP://<secondary_server>.<fully_qualified_domain_name>:5022', 
            FAILOVER_MODE = AUTOMATIC, 
            AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
            BACKUP_PRIORITY = 50, 
            SECONDARY_ROLE(ALLOW_CONNECTIONS = NO), 
            SEEDING_MODE = AUTOMATIC);
    GO
    ``` 

1. Verknüpfen Sie die sekundäre Serverinstanz mit der Verfügbarkeitsgruppe, und gewähren Sie der Verfügbarkeitsgruppe Berechtigungen zum Erstellen von Datenbanken. Aktualisieren Sie das folgende Skript, ersetzen Sie die Werte für Ihre Umgebung in spitzen Klammern `<>`, und führen Sie dieses auf der sekundären Replikatinstanz von SQL Server aus: 
 
    ```sql
    ALTER AVAILABILITY GROUP [<availability_group_name>] JOIN
    GO  
    ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
    GO
    ```

SQL Server erstellt automatisch das Datenbankreplikat auf dem sekundären Server. Wenn die Datenbank groß ist, kann die Synchronisierung der Datenbank einige Zeit dauern. Wenn sich eine Datenbank in einer Verfügbarkeitsgruppe befindet, die für das automatische Seeding konfiguriert ist, können Sie zum Überwachen des Seedingstatus die Systemsicht `sys.dm_hadr_automatic_seeding` abfragen. Die folgende Abfrage gibt eine Zeile für jede Datenbank in einer Verfügbarkeitsgruppe zurück, die für das automatische Seeding konfiguriert ist.

```sql 
SELECT start_time,
    ag.name,
    db.database_name,
    current_state,
    performed_seeding,
    failure_state,
    failure_state_desc
FROM sys.dm_hadr_automatic_seeding autos 
    JOIN sys.availability_databases_cluster db 
        ON autos.ag_db_id = db.group_database_id
    JOIN sys.availability_groups ag 
        ON autos.ag_id = ag.group_id
```

## <a name="prevent-automatic-seeding-after-an-availability-group"></a>Verhindern des automatischen Seedings für eine Verfügbarkeitsgruppe

Um vorübergehend zu verhindern, dass das primäre Replikat für weitere Datenbanken ein Seeding in das sekundäre Replikat ausführt, können Sie der Verfügbarkeitsgruppe die Berechtigung zum Erstellen von Datenbanken verweigern. Führen Sie die folgende Abfrage für die Instanz aus, die das sekundäre Replikat hostet, um der Verfügbarkeitsgruppe die Berechtigung zum Erstellen von Replikatdatenbanken zu verweigern.

```sql
ALTER AVAILABILITY GROUP [<availability_group_name>] 
    DENY CREATE ANY DATABASE
GO
```


## <a name="enable-automatic-seeding-on-an-existing-availability-group"></a>Aktivieren des automatischen Seedings für eine vorhandene Verfügbarkeitsgruppe

Sie können das automatische Seeding für eine vorhandene Datenbank festlegen. Mit dem folgenden Befehl wird eine Verfügbarkeitsgruppe so geändert, dass das automatische Seeding verwendet wird. Führen Sie den folgenden Befehl auf dem primären Replikat aus.

```sql
ALTER AVAILABILITY GROUP [<availability_group_name>] 
    MODIFY REPLICA ON '<secondary_node>' 
    WITH (SEEDING_MODE = AUTOMATIC)
GO
```

Der vorherige Befehl zwingt eine Datenbank bei Bedarf dazu, das Seeding erneut zu starten. Wenn beim Seeding z.B. ein Fehler auftritt, weil auf dem sekundären Replikat zu wenig Speicherplatz verfügbar ist, fügen Sie freien Speicherplatz hinzu, und starten Sie das Seeding durch Ausführen von `ALTER AVAILABILITY GROUP ... WITH (SEEDING_MODE=AUTOMATIC)` erneut.

## <a name="stop-automatic-seeding"></a>Beenden des automatischen Seedings

Um das automatische Seeding für eine Verfügbarkeitsgruppe zu beenden, führen Sie das folgende Skript auf dem primären Replikat aus:

```sql
ALTER AVAILABILITY GROUP [<availability_group_name>] 
    MODIFY REPLICA ON '<secondary_node>'   
    WITH (SEEDING_MODE = MANUAL)
GO
```

Durch das vorherige Skript werden alle Replikate, für die das Seeding derzeit aktiviert ist, abgebrochen, und es wird verhindert, dass SQL Server automatisch ein Replikat in dieser Verfügbarkeitsgruppe initialisiert. Die Synchronisierung für Replikate, die bereits initialisiert wurden, wird hierdurch nicht beendet. 


## <a name="monitor-automatic-seeding-availability-group"></a>Überwachen des automatischen Seedings einer Verfügbarkeitsgruppe

### <a name="use-system-dynamic-management-views-to-monitor-seeding"></a>Verwenden dynamischer Verwaltungssichten (DMVs) für das System zum Überwachen des Seedings

Die folgenden Systemsichten zeigen den Status des automatischen Seedings in SQL Server.

**sys.dm_hadr_automatic_seeding** 

Fragen Sie auf dem primären Replikat `sys.dm_hadr_automatic_seeding` ab, um den Status des automatischen Seedingprozesses zu überprüfen. Die Sicht gibt eine Zeile für jeden Seedingprozess zurück. Beispiel:

```sql
SELECT start_time, 
    completion_time
    is_source,
    current_state,
    failure_state,
    failure_state_desc
FROM sys.dm_hadr_automatic_seeding
```
 
**sys.dm_hadr_physical_seeding_stats** 

Fragen Sie auf dem primären Replikat die DMV `sys.dm_hadr_physical_seeding_stats` ab, um die physischen Statistiken für jeden aktuell ausgeführten Seedingprozess anzuzeigen. Die folgende Abfrage gibt Zeilen zurück, wenn das Seeding ausgeführt wird:

```sql
SELECT * FROM sys.dm_hadr_physical_seeding_stats;
```

Die zwei Spalten *total_disk_io_wait_time_ms* und *total_network_wait_time_ms* können zum Ermitteln von Leistungsengpässen im automatischen Seedingprozess verwendet werden. Die zwei Spalten sind auch in dem erweiterten Ereignis *hadr_physical_seeding_progress* vorhanden.

**total_disk_io_wait_time_ms** stellt die Zeit dar, die der Sicherungs- bzw. Wiederherstellungsthread auf den Datenträger wartet. Dieser Wert ist von Beginn des Seedingvorgangs an kumulativ. Wenn die Datenträger nicht für das Lesen oder Schreiben des Sicherungsdatenstroms bereit sind, geht der Sicherungs- bzw. Wiederherstellungsthread in einen Ruhezustand über und reaktiviert sich jede Sekunde, um zu überprüfen, ob der Datenträger bereit ist.
        
**total_network_wait_time_ms** wird für das primäre und sekundäre Replikat anders interpretiert. Auf dem primären Replikat stellt dieser Indikator die Flusssteuerungsdauer des Netzwerks dar. Auf dem sekundären Replikat stellt er die Zeit dar, die der Wiederherstellungsthread auf eine verfügbare Nachricht zum Schreiben auf den Datenträger wartet.

### <a name="diagnose-database-initialization-using-automatic-seeding-in-the-error-log"></a>Diagnostizieren der Datenbankinitialisierung mithilfe von automatischem Seeding im Fehlerprotokoll

Wenn Sie eine Datenbank zu einer Verfügbarkeitsgruppe hinzufügen, die für das automatische Seeding konfiguriert ist, führt SQL Server eine VDI-Sicherung über den Endpunkt der Verfügbarkeitsgruppe aus. Überprüfen Sie das SQL Server-Fehlerprotokoll auf Informationen darüber, wann die Sicherung abgeschlossen und die sekundäre Datenbank synchronisiert wurde.

### <a name="diagnose-database-level-health-with-extended-events"></a>Diagnostizieren der Integrität auf Datenbankebene mit erweiterten Ereignissen

Das automatische Seeding verfügt über neue erweiterte Ereignisse zum Nachverfolgen von Statistiken zu Statusänderungen, Fehlern und Leistung während der Initialisierung. 

Dieses Skript erstellt z. B. eine Sitzung für erweiterte Ereignisse, die Ereignisse im Zusammenhang mit automatischem Seeding erfasst: 

```sql
CREATE EVENT SESSION [AlwaysOn_autoseed] ON SERVER 
    ADD EVENT sqlserver.hadr_automatic_seeding_state_transition,
    ADD EVENT sqlserver.hadr_automatic_seeding_timeout,
    ADD EVENT sqlserver.hadr_db_manager_seeding_request_msg,
    ADD EVENT sqlserver.hadr_physical_seeding_backup_state_change,
    ADD EVENT sqlserver.hadr_physical_seeding_failure,
    ADD EVENT sqlserver.hadr_physical_seeding_forwarder_state_change,
    ADD EVENT sqlserver.hadr_physical_seeding_forwarder_target_state_change,
    ADD EVENT sqlserver.hadr_physical_seeding_progress,
    ADD EVENT sqlserver.hadr_physical_seeding_restore_state_change,
    ADD EVENT sqlserver.hadr_physical_seeding_submit_callback
    ADD TARGET package0.event_file(
        SET filename=N'autoseed.xel',
            max_file_size=(5),
            max_rollover_files=(4)
        )
WITH (
    MAX_MEMORY=4096 KB,
    EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
    MAX_DISPATCH_LATENCY=30 SECONDS,
    MAX_EVENT_SIZE=0 KB,
    MEMORY_PARTITION_MODE=NONE,
    TRACK_CAUSALITY=OFF,
    STARTUP_STATE=ON
    )
GO 

ALTER EVENT SESSION AlwaysOn_autoseed ON SERVER STATE=START
GO 
```


Die folgende Tabelle enthält die erweiterten Ereignisse die sich auf automatisches Seeding beziehen: 

| Name | BESCHREIBUNG|
|------------ |---------------| 
|hadr_db_manager_seeding_request_msg |  Seedinganforderungsnachricht.
|hadr_physical_seeding_backup_state_change |    Statusänderung auf der Sicherungsseite für das physische Seeding.
|hadr_physical_seeding_restore_state_change |Statusänderung auf der Wiederherstellungsseite für das physische Seeding.
|hadr_physical_seeding_forwarder_state_change | Statusänderung auf der Weiterleitungsseite für das physische Seeding.
|hadr_physical_seeding_forwarder_target_state_change |  Statusänderung auf der Zielseite der Weiterleitung für das physische Seeding.
|hadr_physical_seeding_submit_callback  |Senderückrufereignis für das physische Seeding.
|hadr_physical_seeding_failure  |Fehlerereignis für das physische Seeding.
|hadr_physical_seeding_progress |   Statusereignis für das physische Seeding.
|hadr_physical_seeding_schedule_long_task_failure   |Fehlerereignis für langen Task im Zeitplan für das physische Seeding.
|hadr_automatic_seeding_start   |Tritt beim Senden eines automatischen Seedingvorgangs auf.
|hadr_automatic_seeding_state_transition    |Tritt beim Statuswechsel eines automatischen Seedingvorgangs auf.
|hadr_automatic_seeding_success |Tritt beim Erfolg eines automatischen Seedingvorgangs auf.
|hadr_automatic_seeding_failure |Tritt bei einem Fehler eines automatischen Seedingvorgangs auf.
|hadr_automatic_seeding_timeout |Tritt beim Timeout eines automatischen Seedingvorgangs auf.

### <a name="other-troubleshooting-considerations"></a>Weitere Überlegungen zur Problembehandlung

**Überwachen des automatischen Seedings**

Fragen Sie `sys.dm_hadr_physical_seeding_stats` nach aktuell ausgeführten automatischen Seedingprozessen ab. Die Sicht gibt eine Zeile für jede Datenbank zurück. Beispiel:

```sql
SELECT local_database_name, 
    role_desc, 
    internal_state_desc, 
    transfer_rate_bytes_per_second, 
    transferred_size_bytes, 
    database_size_bytes, 
    start_time_utc, 
    end_time_utc, estimate_time_complete_utc, 
    total_disk_io_wait_time_ms, 
    total_network_wait_time_ms, 
    is_compression_enabled 
FROM sys.dm_hadr_physical_seeding_stats
```

**Problembehandlung: Warum wird eine Datenbank in einer Verfügbarkeitsgruppe, die für das automatische Seeding konfiguriert ist, nicht angezeigt?**


Wenn eine Datenbank nicht als Teil einer Verfügbarkeitsgruppe mit aktiviertem automatischen Seeding angezeigt wird, ist beim automatischen Seeding wahrscheinlich ein Fehler aufgetreten. Dadurch wird verhindert, dass die Datenbank zur Verfügbarkeitsgruppe auf dem primären oder sekundären Replikat hinzugefügt wird. Fragen Sie `sys.dm_hadr_automatic_seeding` auf den primären und sekundären Replikaten ab. Führen Sie z. B. die folgende Abfrage aus, um einen Fehlerzustand des automatischen Seedings zu identifizieren.

```sql
SELECT start_time, 
    completion_time, 
    is_source, 
    current_state, 
    failure_state, 
    failure_state_desc, 
    error_code 
FROM sys.dm_hadr_automatic_seeding
```

## <a name="automatic-seeding-and-performance-considerations"></a>Automatisches Seeding und Leistungsüberlegungen

SQL Server verwendet eine feste Anzahl von Threads für das automatische Seeding. In der primären Instanz verwendet SQL Server einen Thread pro LUN zum Lesen von Änderungen. In der sekundären Instanz verwendet SQL Server einen Thread pro LUN zum Initialisieren der Datenbank.

Legen Sie das Ablaufverfolgungsflag 9567 auf dem primären Replikat fest, um die Komprimierung des Datenstroms während des automatischen Seedings zu aktivieren. Dadurch lässt sich die Übertragungsdauer des automatischen Seedings deutlich verringern, allerdings erhöht sich die CPU-Nutzung. Weitere Informationen finden Sie unter [Optimieren der Komprimierung für die Verfügbarkeitsgruppe](../../../database-engine/availability-groups/windows/tune-compression-for-availability-group.md). 


## <a name="when-not-to-use-automatic-seeding"></a>Wann Sie das automatische Seeding nicht verwenden sollten

In einigen Szenarien ist das automatische Seeding möglicherweise keine Optimale Lösung zum Initialisieren eines sekundären Replikats. Während des automatischen Seedings führt SQL Server für die Initialisierung eine Sicherung über das Netzwerk aus. Dieser Prozess kann langsam sein, wenn die Datenbanken sehr groß sind oder das sekundäre Replikat remote gespeichert wird. Das Transaktionsprotokoll für diese Datenbanken kann während der Sicherung abgeschnitten werden, daher kann ein längerer Initialisierungsprozess für eine ausgelastete Datenbank zu einer erheblichen Wachstumsrate des Transaktionsprotokolls führen.
Vor dem Hinzufügen einer Datenbank zu einer Verfügbarkeitsgruppe mit automatischem Seeding sollten Sie daher Datenbankgröße, Last und Standortabstand zwischen den Replikaten untersuchen.

## <a name="resources"></a>Ressourcen

[CREATE AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/create-availability-group-transact-sql.md)

[Handbuch zur Problembehandlung und Überwachung von AlwaysOn-Verfügbarkeitsgruppen](/previous-versions/sql/sql-server-guides/dn135328(v=sql.110))