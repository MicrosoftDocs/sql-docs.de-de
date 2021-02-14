---
description: Verwalten der schnelleren Datenbankwiederherstellung
title: Verwalten der beschleunigten Datenbankwiederherstellung | Microsoft-Dokumentation
ms.date: 02/02/2021
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- accelerated database recovery [SQL Server], recovery-only
- database recovery [SQL Server]
author: mashamsft
ms.author: mathoma
ms.reviewer: kfarlee
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: ca11bbae7f1bcc86c0891cc22d8a7a02b26fd46e
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495778"
---
# <a name="manage-accelerated-database-recovery"></a>Verwalten der schnelleren Datenbankwiederherstellung

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver2019.md)]

## <a name="enabling-and-controlling-adr"></a>Aktivieren und Steuern von ADR

Die schnellere Datenbankwiederherstellung (Accelerated Database Recovery, ADR) ist in [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] standardmäßig deaktiviert und kann mithilfe der DDL-Syntax gesteuert werden:
```sql
ALTER DATABASE [DB] SET ACCELERATED_DATABASE_RECOVERY = {ON | OFF}
[(PERSISTENT_VERSION_STORE_FILEGROUP = { filegroup name }) ];

```

Verwenden Sie diese Syntax, um zu steuern, ob die Funktion aktiviert oder deaktiviert ist, und legen Sie eine bestimmte Dateigruppe für die Daten des *persistenten Versionsspeichers* (Persistent Version Store, PVS) fest. Wenn keine Dateigruppe angegeben wird, wird der PVS in der PRIMARY-Dateigruppe gespeichert.

## <a name="managing-the-persistent-version-store-filegroup"></a>Verwalten der Dateigruppe des persistenten Versionsspeichers
Die ADR-Funktion basiert auf der Versionierung von Änderungen, wobei verschiedene Versionen eines Datenelements im PVS gespeichert sind.
Es gibt Überlegungen hinsichtlich der Suche nach dem Speicherort des PVS und der Verwaltung der Größe der Daten im PVS.

### <a name="to-enable-adr-without-specifying-a-filegroup"></a>Aktivieren von ADR ohne Angabe einer Dateigruppe

```sql
ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = ON;
GO
```

Wenn keine PVS-Dateigruppe angegeben wird, werden die PVS-Daten in der `PRIMARY`-Dateigruppe gespeichert.

### <a name="to-enable-adr-and-specify-that-the-pvs-should-be-stored-in-the-versionstorefg-filegroup"></a>So aktivieren Sie ADR und geben als Speicherort für den PVS die [VersionStoreFG]-Dateigruppe an

Erstellen Sie vor dem Ausführen dieses Skripts die Dateigruppe.

```sql
ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = ON
(PERSISTENT_VERSION_STORE_FILEGROUP = [VersionStoreFG])
```

### <a name="to-disable-the-adr-feature"></a>So deaktivieren Sie die ADR-Funktion

```sql
ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = OFF;
GO
```

Auch nach dem Aktivieren der ADR-Funktion werden im persistenten Versionsspeicher Versionen gespeichert, die vom System für die logische Wiederherstellung weiterhin benötigt werden.

### <a name="change-the-location-of-the-pvs-to-a-different-filegroup"></a>Ändern des Speicherorts des PVS in eine andere Dateigruppe

Es kann aus verschiedenen Gründen notwendig sein, den Speicherort des PVS in eine andere Dateigruppe zu ändern. Dies ist dann zum Beispiel der Fall, wenn der PVS mehr Speicherplatz benötigt oder eine schnellere Speicherung erforderlich ist.

Der Speicherort des PVS wird in drei Schritten geändert.

1. Deaktivieren Sie die ADR-Funktion.

   ```sql
   ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = OFF;
   GO
   ```

2. Warten Sie, bis alle im PVS gespeicherten Versionen freigegeben werden können.

   Um ADR mit einem neuen Speicherort für den persistenten Versionsspeicher aktivieren zu können, müssen Sie zunächst sicherstellen, dass alle Versionsinformationen aus dem vorherigen PVS-Speicherort gelöscht wurden. Um dieses Cleanup zu erzwingen, führen Sie den folgenden Befehl aus:

   ```sql
   EXEC sys.sp_persistent_version_cleanup [database name]
   ```

   Die gespeicherte Prozedur `sys.sp_persistent_version_cleanup` erfolgt synchron. Dies bedeutet, dass sie erst vollständig ausgeführt wird, bis alle Versionsinformationen im aktuellen PVS gelöscht wurden.  Nach Durchführen des Cleanups können Sie überprüfen, ob die Versionsinformationen tatsächlich entfernt wurden. Führen Sie dazu eine `sys.dm_persistent_version_store_stats`-Abfrage für DMV durch, und untersuchen Sie den `persistent_version_store_size_kb`-Wert.

   ```sql
   SELECT DB_Name(database_id), persistent_version_store_size_kb 
   FROM sys.dm_tran_persistent_version_store_stats where database_id = [MyDatabaseID]
   ```

   Wenn der „persistent_version_store_size_kb“-Wert 0 ist, können Sie die ADR-Funktion noch mal aktivieren und den PVS so konfigurieren, dass er sich in der neuen Dateigruppe befinden.

1. Aktivieren von ADR nach Festlegen des neuen PVS-Speicherorts

   ```sql
   ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = ON
   (PERSISTENT_VERSION_STORE_FILEGROUP = [VersionStoreFG])
   ```

## <a name="troubleshooting"></a>Problembehandlung

> [!NOTE]
> Dieser Abschnitt bezieht sich auch auf Azure SQL-Datenbank.

Führen Sie eine `sys.dm_tran_persistent_version_store_stats`-Abfrage durch, um die PVS-Größen zu überprüfen.

Überprüfen Sie die `% of DB`-Größe. Beachten Sie auch den Unterschied zur typischen Größe.

Der PVS gilt als groß, wenn er deutlich größer als die Baseline ist oder annähernd 50 % der Größe der Datenbank entspricht. 

1. Rufen Sie `oldest_active_transaction_id` ab, und überprüfen Sie, ob diese Transaktion für einen sehr langen Zeitraum aktiv war, indem Sie eine `sys.dm_tran_database_transactions`-Abfrage basierend auf der Transaktions-ID durchführen.

   Aktive Transaktionen verhindern das Cleanup des PVS.

2. Wenn die Datenbank Teil einer Verfügbarkeitsgruppe ist, überprüfen Sie die `secondary_low_water_mark`. Diese ist identisch mit dem `low_water_mark_for_ghosts`, das von `sys.dm_hadr_database_replica_states` gemeldet wird. Führen Sie die `sys.dm_hadr_database_replica_states`-Abfrage durch, um festzustellen, ob eines der Replikate diesen Wert zurückhält, da dadurch auch das PVS-Cleanup verhindert wird.
3. Überprüfen Sie `min_transaction_timestamp` (oder `online_index_min_transaction_timestamp`, wenn der Online-PVS den Vorgang aufhält), und überprüfen Sie auf dieser Grundlage `sys.dm_tran_active_snapshot_database_transactions` für die Spalte `transaction_sequence_num`, um die Sitzung mit der alten Momentaufnahmetransaktion zu suchen, die das PVS-Cleanup aufhält.
4. Wenn nichts davon zutrifft, bedeutet dies, dass das Cleanup von abgebrochenen Transaktionen aufgehalten wird. Überprüfen Sie ein letztes Mal `aborted_version_cleaner_last_start_time` und `aborted_version_cleaner_last_end_time`, um festzustellen, ob das Cleanup abgebrochener Transaktionen durchgeführt wurde. Die `oldest_aborted_transaction_id` sollte nach Durchführen des Cleanups abgebrochener Transaktionen nach oben verschoben werden.
5. Wenn die abgebrochene Transaktion zuletzt nicht erfolgreich abgeschlossen wurde, überprüfen Sie das Fehlerprotokoll auf `VersionCleaner`-Probleme.

Verwenden Sie die folgende Beispielabfrage als Hilfe zur Problembehandlung:

```sql
SELECT pvss.persistent_version_store_size_kb / 1024. / 1024 AS persistent_version_store_size_gb,
       pvss.online_index_version_store_size_kb / 1024. / 1024 AS online_index_version_store_size_gb,
       pvss.current_aborted_transaction_count,
       pvss.aborted_version_cleaner_start_time,
       pvss.aborted_version_cleaner_end_time,
       dt.database_transaction_begin_time AS oldest_transaction_begin_time,
       asdt.session_id AS active_transaction_session_id,
       asdt.elapsed_time_seconds AS active_transaction_elapsed_time_seconds
FROM sys.dm_tran_persistent_version_store_stats AS pvss
LEFT JOIN sys.dm_tran_database_transactions AS dt
ON pvss.oldest_active_transaction_id = dt.transaction_id
   AND
   pvss.database_id = dt.database_id
LEFT JOIN sys.dm_tran_active_snapshot_database_transactions AS asdt
ON pvss.min_transaction_timestamp = asdt.transaction_sequence_num
   OR
   pvss.online_index_min_transaction_timestamp = asdt.transaction_sequence_num
WHERE pvss.database_id = DB_ID();
```
