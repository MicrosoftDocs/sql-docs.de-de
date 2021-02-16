---
description: RESTORE-Anweisungen (Transact-SQL)
title: RESTORE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RESTORE DATABASE
- RESTORE_TSQL
- RESTORE_DATABASE_TSQL
- RESTORE
- RESTORE_LOG_TSQL
- RESTORE LOG
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE DATABASE, see RESTORE statement
- full backups [SQL Server]
- RECOVERY option
- database snapshots [SQL Server], reverting to
- STOPAT syntax
- differential backups, RESTORE statement
- point in time recovery [SQL Server]
- restoring [SQL Server]
- NORECOVERY option
- online restores [SQL Server], RESTORE statement
- moving databases
- RESTORE statement, syntax
- cross-platform restores
- restoring databases [SQL Server], options
- RESTORE statement
- snapshots [SQL Server database snapshots], reverting to
- reverting database snapshots
- transaction log backups [SQL Server], RESTORE statement
- RESTORE LOG, see RESTORE statement
ms.assetid: 877ecd57-3f2e-4237-890a-08f16e944ef1
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||>=aps-pdw-2016'
ms.openlocfilehash: fe799de197de0fc18bb8750b4a6f8db2e288be32
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017470"
---
# <a name="restore-statements-transact-sql"></a>RESTORE-Anweisungen (Transact-SQL)

Stellt SQL-Datenbank-Sicherungen wieder her, die mit dem BACKUP-Befehl erstellt wurden.

Klicken Sie auf eine der folgenden Registerkarten, um Syntax, Argumente, Hinweise, Berechtigungen und Beispiele für eine bestimmte SQL-Version anzuzeigen, mit der Sie arbeiten.

Weitere Informationen zu Syntaxkonventionen finden Sie unter [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Managed Instance](restore-statements-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

Mit diesem Befehl können Sie das folgende Wiederherstellungsszenario durchführen:

- Wiederherstellen einer ganzen Datenbank aus einer vollständigen Datenbanksicherung (vollständige Wiederherstellung).
- Wiederherstellen eines Teils einer Datenbank (teilweise Wiederherstellung).
- Wiederherstellen bestimmter Dateien oder Dateigruppen einer Datenbank (Dateiwiederherstellung).
- Wiederherstellen bestimmter Seiten einer Datenbank (Seitenwiederherstellung).
- Wiederherstellen eines Transaktionsprotokolls in einer Datenbank (Transaktionsprotokollwiederherstellung).
- Wiederherstellen einer Datenbank auf den Zeitpunkt, der über eine Datenbankmomentaufnahme erfasst wurde.

Weitere Informationen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Wiederherstellungsszenarien finden Sie unter [Übersicht über Wiederherstellungsvorgänge](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md). Weitere Informationen und Argumentbeschreibungen finden Sie unter [RESTORE-Argumente](../../t-sql/statements/restore-statements-arguments-transact-sql.md). Berücksichtigen Sie beim Wiederherstellen einer Datenbank aus einer anderen Instanz die Informationen unter [Verwalten von Metadaten beim Bereitstellen einer Datenbank auf einer anderen Serverinstanz (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).

> [!NOTE]
> Weitere Informationen zur Wiederherstellung von Microsoft Azure Blob Storage finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit Microsoft Azure Blob Storage](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

## <a name="syntax"></a>Syntax

```syntaxsql
--To Restore an Entire Database from a Full database backup (a Complete Restore):
RESTORE DATABASE { database_name | @database_name_var }
 [ FROM <backup_device> [ ,...n ] ]
 [ WITH
   {
    [ RECOVERY | NORECOVERY | STANDBY =
        {standby_file_name | @standby_file_name_var }
       ]
   | ,  <general_WITH_options> [ ,...n ]
   | , <replication_WITH_option>
   | , <change_data_capture_WITH_option>
   | , <FILESTREAM_WITH_option>
   | , <service_broker_WITH options>
   | , \<point_in_time_WITH_options-RESTORE_DATABASE>
   } [ ,...n ]
 ]
[;]

--To perform the first step of the initial restore sequence of a piecemeal restore:
RESTORE DATABASE { database_name | @database_name_var }
   <files_or_filegroups> [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
      PARTIAL, NORECOVERY
      [  , <general_WITH_options> [ ,...n ]
       | , \<point_in_time_WITH_options-RESTORE_DATABASE>
      ] [ ,...n ]
[;]  
  
--To Restore Specific Files or Filegroups:
RESTORE DATABASE { database_name | @database_name_var }
   <file_or_filegroup> [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
   {
      [ RECOVERY | NORECOVERY ]
      [ , <general_WITH_options> [ ,...n ] ]
   } [ ,...n ]
[;]  
  
--To Restore Specific Pages:
RESTORE DATABASE { database_name | @database_name_var }
   PAGE = 'file:page [ ,...n ]'
 [ , <file_or_filegroups> ] [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
       NORECOVERY
      [ , <general_WITH_options> [ ,...n ] ]
[;]

--To Restore a Transaction Log:
RESTORE LOG { database_name | @database_name_var }
 [ <file_or_filegroup_or_pages> [ ,...n ] ]
 [ FROM <backup_device> [ ,...n ] ]
 [ WITH
   {
     [ RECOVERY | NORECOVERY | STANDBY =
        {standby_file_name | @standby_file_name_var }
       ]
    | , <general_WITH_options> [ ,...n ]
    | , <replication_WITH_option>
    | , \<point_in_time_WITH_options-RESTORE_LOG>
   } [ ,...n ]
 ]
[;]

--To Revert a Database to a Database Snapshot:
RESTORE DATABASE { database_name | @database_name_var }
FROM DATABASE_SNAPSHOT = database_snapshot_name

<backup_device>::=
{
   { logical_backup_device_name |
      @logical_backup_device_name_var }
 | { DISK
     | TAPE
     | URL
   } = { 'physical_backup_device_name' |
      @physical_backup_device_name_var }
}
Note: URL is the format used to specify the location and the file name for the Microsoft Azure Blob. Although Microsoft Azure storage is a service, the implementation is similar to disk and tape to allow for a consistent and seamless restore experience for all the three devices.
<files_or_filegroups>::=
{
   FILE = { logical_file_name_in_backup | @logical_file_name_in_backup_var }
 | FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }
 | READ_WRITE_FILEGROUPS
}

<general_WITH_options> [ ,...n ]::=
--Restore Operation Options
   MOVE 'logical_file_name_in_backup' TO 'operating_system_file_name'
          [ ,...n ]
 | REPLACE
 | RESTART
 | RESTRICTED_USER | CREDENTIAL

--Backup Set Options
 | FILE = { backup_set_file_number | @backup_set_file_number }
 | PASSWORD = { password | @password_variable }

--Media Set Options
 | MEDIANAME = { media_name | @media_name_variable }
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }
 | BLOCKSIZE = { blocksize | @blocksize_variable }

--Data Transfer Options
 | BUFFERCOUNT = { buffercount | @buffercount_variable }
 | MAXTRANSFERSIZE = { maxtransfersize | @maxtransfersize_variable }

--Error Management Options
 | { CHECKSUM | NO_CHECKSUM }
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }

--Monitoring Options
 | STATS [ = percentage ]

--Tape Options.
 | { REWIND | NOREWIND }
 | { UNLOAD | NOUNLOAD }

<replication_WITH_option>::=
 | KEEP_REPLICATION

<change_data_capture_WITH_option>::=
 | KEEP_CDC

<FILESTREAM_WITH_option>::=
 | FILESTREAM ( DIRECTORY_NAME = directory_name )

<service_broker_WITH_options>::=
 | ENABLE_BROKER
 | ERROR_BROKER_CONVERSATIONS
 | NEW_BROKER

\<point_in_time_WITH_options-RESTORE_DATABASE>::=
 | {
   STOPAT = { 'datetime'| @datetime_var }
 | STOPATMARK = 'lsn:lsn_number'
                 [ AFTER 'datetime']
 | STOPBEFOREMARK = 'lsn:lsn_number'
                 [ AFTER 'datetime']
   }

\<point_in_time_WITH_options-RESTORE_LOG>::=
 | {
   STOPAT = { 'datetime'| @datetime_var }
 | STOPATMARK = { 'mark_name' | 'lsn:lsn_number' }
                 [ AFTER 'datetime']
 | STOPBEFOREMARK = { 'mark_name' | 'lsn:lsn_number' }
                 [ AFTER 'datetime']
   }
```

## <a name="arguments"></a>Argumente

Argumentbeschreibungen finden Sie unter [RESTORE-Argumente](../../t-sql/statements/restore-statements-arguments-transact-sql.md).

## <a name="about-restore-scenarios"></a>Info zu Wiederherstellungsszenarien

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt eine Vielzahl von Wiederherstellungsszenarien:

- Vollständige Datenbankwiederherstellung

  Stellt die gesamte Datenbank wieder her. Dabei wird mit einer vollständigen Datenbanksicherung begonnen, darauf folgt gegebenenfalls eine differenzielle Datenbanksicherung (sowie Protokollsicherungen). Weitere Informationen finden Sie unter [Vollständige Datenbankwiederherstellungen – Einfaches Wiederherstellungsmodell](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md) oder [Vollständige Datenbankwiederherstellungen – Vollständiges Wiederherstellungsmodell](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).

- Dateiwiederherstellung

  Stellt eine Datei oder Dateigruppe in einer Datenbank mit mehreren Dateigruppen wieder her. Beachten Sie, dass die Datei im Rahmen des Modells der einfachen Wiederherstellung zu einer schreibgeschützten Dateigruppe gehören muss. Nach einer vollständigen Dateiwiederherstellung kann eine differenzielle Dateisicherung wiederhergestellt werden. Weitere Informationen finden Sie unter [Dateiwiederherstellungen – Vollständiges Wiederherstellungsmodell](../../relational-databases/backup-restore/file-restores-full-recovery-model.md) und [Dateiwiederherstellungen – Einfaches Wiederherstellungsmodell](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md).

- Seitenwiederherstellung

  Stellt einzelne Seiten wieder her. Die Seitenwiederherstellung ist nur bei den Modellen der vollständigen und der massenprotokollierten Wiederherstellung verfügbar. Weitere Informationen finden Sie unter [Wiederherstellung von Seiten – SQL Server](../../relational-databases/backup-restore/restore-pages-sql-server.md).

- Schrittweise Wiederherstellung

  Stellt die Datenbank schrittweise wieder her, wobei mit der primären Dateigruppe und einer oder mehreren sekundären Dateigruppen begonnen wird. Eine schrittweise Wiederherstellung beginnt mit RESTORE DATABASE, wobei die Option PARTIAL verwendet wird und eine oder mehrere sekundäre Dateigruppen für die Wiederherstellung angegeben werden. Weitere Informationen finden Sie unter [Schrittweise Wiederherstellungen – SQL Server](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).

- Nur Wiederherstellung

  Stellt Daten wieder her, die bereits konsistent mit der Datenbank sind und nur zur Verfügung gestellt werden müssen. Weitere Informationen finden Sie unter [Wiederherstellen einer Datenbank ohne Wiederherstellung von Daten](../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md).

- Wiederherstellung des Transaktionsprotokolls

  Beim Modell der vollen oder massenprotokollierten Wiederherstellung ist die Wiederherstellung von Protokollsicherungen erforderlich, um den gewünschten Wiederherstellungspunkt zu erreichen. Weitere Informationen zu Wiederherstellungsprotokollsicherungen finden Sie unter [Anwenden von Transaktionsprotokollsicherungen – SQL Server](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).

- Vorbereiten einer Verfügbarkeitsdatenbank für eine Always On-Verfügbarkeitsgruppe

  Weitere Informationen finden Sie unter [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe – SQL Server](../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).

- Vorbereiten einer Spiegeldatenbank für die Datenbankspiegelung

  Weitere Informationen finden Sie unter [Vorbereiten einer Spiegeldatenbank auf die Spiegelung – SQL Server](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).

- Onlinewiederherstellung

  > [!NOTE]
  > Die Onlinewiederherstellung ist nur in der Enterprise Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zulässig.

Wenn die Onlinewiederherstellung unterstützt wird und die Datenbank online ist, sind Datei- und Seitenwiederherstellungen automatisch Onlinewiederherstellungen. Dies gilt auch für Wiederherstellungen einer sekundären Dateigruppe nach der Anfangsphase einer schrittweisen Wiederherstellung.

  > [!NOTE]
  > Bei Onlinewiederherstellungen können [verzögerte Transaktionen](../../relational-databases/backup-restore/deferred-transactions-sql-server.md) auftreten.

Weitere Informationen finden Sie unter [Onlinewiederherstellung](../../relational-databases/backup-restore/online-restore-sql-server.md).

## <a name="additional-considerations-about-restore-options"></a>Weitere Überlegungen zu RESTORE-Optionen

### <a name="discontinued-restore-keywords"></a>Nicht mehr unterstützte RESTORE-Schlüsselwörter

Die folgenden Schlüsselwörter werden in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] nicht mehr unterstützt:

|Nicht mehr unterstütztes Schlüsselwort|Ersetzt durch...|Beispiel für Ersetzungsschlüsselwort|
|--------------------------|------------------|------------------------------------|
|LOAD|RESTORE|`RESTORE DATABASE`|
|TRANSACTION|PROTOKOLL|`RESTORE LOG`|
|DBO_ONLY|RESTRICTED_USER|`RESTORE DATABASE ... WITH RESTRICTED_USER`|

### <a name="restore-log"></a>RESTORE LOG

RESTORE LOG kann eine Dateiliste einschließen, um das Erstellen von Dateien bei einem Rollforward zu ermöglichen. Diese wird verwendet, wenn die Protokollsicherung Protokolleinträge enthält, die erstellt wurden, als der Datenbank eine Datei hinzugefügt wurde.

> [!NOTE]
> Bei einer Datenbank, die das Modell der vollen oder massenprotokollierten Wiederherstellung verwendet, ist in den meisten Fällen die Sicherung des Protokollfragments erforderlich, bevor die Datenbank wiederhergestellt wird. Wenn eine Datenbank ohne vorherige Sicherung des Protokollfragments wiederhergestellt wird, tritt ein Fehler auf. Dies gilt nicht, wenn die RESTORE DATABASE-Anweisung die WITH REPLACE- oder die WITH STOPAT-Klausel enthält, in der eine Zeit oder Transaktion nach dem Ende der Datensicherung angegeben sein muss. Weitere Informationen zu Sicherungen des Protokollfragments finden Sie unter [Protokollfragmentsicherungen](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).

### <a name="comparison-of-recovery-and-norecovery"></a>Vergleich zwischen RECOVERY und NORECOVERY
Ein Rollback wird von der RESTORE-Anweisung über die Optionen [ RECOVERY | NORECOVERY ] gesteuert:

- NORECOVERY gibt an, dass kein Rollback erfolgt. Damit kann das Rollforward die nächste Anweisung in der Sequenz fortsetzen.

  In diesem Fall können über die Wiederherstellungssequenz andere Sicherungen wiederhergestellt werden, und für diese kann ein Rollforward ausgeführt werden.

- RECOVERY (die Standardeinstellung) gibt an, dass das Rollback erst dann ausgeführt werden kann, wenn das Rollforward für die aktuelle Sicherung abgeschlossen ist.

  Das Wiederherstellen der Datenbank erfordert, dass die gesamte Gruppe der wiederhergestellten Daten (*Rollforwardgruppe*) mit der Datenbank konsistent ist. Wenn das Rollforward für die Rollforwardgruppe nicht weit genug ausgeführt wurde, um die Konsistenz mit der Datenbank herzustellen, und wenn RECOVERY angegeben ist, dann gibt [!INCLUDE[ssDE](../../includes/ssde-md.md)] einen Fehler aus. Informationen zum Wiederherstellungsprozess finden Sie unter [ (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery).

## <a name="compatibility-support"></a>Kompatibilitätsunterstützung
Sicherungen der Datenbanken **master**, **model** und **msdb**, die mit einer früheren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellt wurden, können von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] nicht wiederhergestellt werden.

> [!NOTE]
> Es kann keine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sicherung zu einer früheren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wiederhergestellt werden, die älter als die Version ist, von der die Sicherung erstellt wurde.

Jede Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet im Vergleich zu früheren Versionen jedoch einen anderen Standardpfad. Daher muss zum Wiederherstellen einer Datenbank, die am Standardort für Sicherungen in früheren Versionen erstellt wurde, die MOVE-Option verwendet werden. Informationen zum neuen Standardpfad finden Sie unter [Dateispeicherorte für Standard- und benannte Instanzen von SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md).

Nachdem Sie eine Datenbank einer früheren Version in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]wiederhergestellt haben, wird die Datenbank automatisch aktualisiert. In der Regel ist die Datenbank sofort verfügbar. Wenn eine [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]-Datenbank aber Volltextindizes aufweist, werden diese beim Upgrade entweder importiert, zurückgesetzt oder neu erstellt, je nach der Einstellung der Servereigenschaft **upgrade_option**. Wenn die Upgradeoption auf „Importieren“ (**upgrade_option** = 2) oder „Neu erstellen“ (**upgrade_option** = 0) festgelegt ist, sind die Volltextindizes während des Upgrades nicht verfügbar. Je nach Menge der indizierten Daten kann der Importvorgang mehrere Stunden dauern; die Neuerstellung sogar bis zu zehnmal länger. Wenn die Upgradeoption auf Importieren festgelegt ist und kein Volltextkatalog verfügbar ist, werden die zugehörigen Volltextindizes neu erstellt. Verwenden Sie **sp_fulltext_service** , um die Einstellung der Servereigenschaft [upgrade_option](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)zu ändern.

Wird eine Datenbank zum ersten Mal an eine neue Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]angefügt oder wiederhergestellt, ist noch keine Kopie des Datenbank-Hauptschlüssels (verschlüsselt vom Diensthauptschlüssel) auf dem Server gespeichert. Der Datenbank-Hauptschlüssel (Database Master Key, DMK) muss mithilfe der Anweisung **OPEN MASTER KEY** entschlüsselt werden. Nachdem der Datenbank-Hauptschlüssel entschlüsselt wurde, können Sie für die Zukunft die automatische Entschlüsselung aktivieren, indem Sie die Anweisung **ALTER MASTER KEY REGENERATE** verwenden. Auf diese Weise können Sie eine Kopie des mit dem Diensthauptschlüssel (Service Master Key, SMK) verschlüsselten Datenbank-Hauptschlüssels für den Server bereitstellen. Wenn eine Datenbank von einer früheren Version aktualisiert wurde, sollte der DMK neu generiert werden, damit er den neueren AES-Algorithmus verwendet. Weitere Informationen zum Neugenerieren des DMK finden Sie unter [ALTER MASTER KEY](../../t-sql/statements/alter-master-key-transact-sql.md). Die zum Neugenerieren des DMK zum Upgrade auf AES erforderliche Zeit hängt von der Anzahl der Objekte ab, die durch den DMK geschützt werden. Der DMK muss nur einmal auf AES aktualisiert und neu generiert werden. Dies hat keine Auswirkungen auf zukünftige Neugenerierungen im Rahmen einer Schlüsselrotationsstrategie.

## <a name="general-remarks"></a>Allgemeine Hinweise
Wenn bei einer Offlinewiederherstellung die angegebene Datenbank verwendet wird, zwingt RESTORE die Benutzer nach einer kurzen Verzögerung zum Beenden der Datenbankverwendung. Bei der Onlinewiederherstellung einer nicht primären Dateigruppe kann die Datenbank weiter verwendet werden, es sei denn, die zu wiederherstellende Dateigruppe wird offline geschaltet. Alle in der angegebenen Datenbank vorhandenen Daten werden durch die wiederhergestellten Daten ersetzt.

Wiederherstellungsvorgänge über Plattformen hinweg, sogar zwischen unterschiedlichen Prozessortypen, können ausgeführt werden, solange die Sortierung der Datenbank vom Betriebssystem unterstützt wird.

RESTORE kann nach einem Fehler neu gestartet werden. Außerdem können Sie RESTORE anweisen, den Vorgang auch bei Fehlern fortzusetzen und so viele Daten wie möglich wiederherzustellen (siehe die Option `CONTINUE_AFTER_ERROR`).

RESTORE ist nicht in einer expliziten oder implizierten Transaktion zulässig.

Für das Wiederherstellen einer beschädigten **master**-Datenbank ist eine spezielle Vorgehensweise erforderlich. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von Systemdatenbanken](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md).

Durch das Wiederherstellen einer Datenbank wird der Plancache für die wiederherzustellende Datenbank gelöscht. Durch das Löschen des Plancaches wird eine Neukompilierung aller nachfolgenden Ausführungspläne verursacht, und möglicherweise entsteht plötzlich eine temporäre Verringerung der Abfrageleistung. 

Um eine Verfügbarkeitsdatenbank wiederherzustellen, stellen Sie zuerst die Datenbank mit der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wieder her, und fügen Sie dann die Datenbank der Verfügbarkeitsgruppe hinzu.

## <a name="interoperability"></a>Interoperabilität

### <a name="database-settings-and-restoring"></a>Datenbankeinstellungen und -wiederherstellung

Bei einer Wiederherstellung werden die meisten Datenbankoptionen, die mit ALTER DATABASE festgelegt werden können, auf die Werte zurückgesetzt, die am Ende der Sicherung gültig waren.

Durch die Verwendung der Option WITH RESTRICTED_USER wird dieses Verhalten jedoch für die Einstellung der Benutzerzugriffsoption überschrieben. Diese Einstellung wird immer im Anschluss an eine RESTORE-Anweisung festgelegt, die die Option WITH RESTRICTED_USER einschließt.

### <a name="restoring-an-encrypted-database"></a>Wiederherstellen einer verschlüsselten Datenbank

Um eine verschlüsselte Datenbank wiederherstellen zu können, muss das Zertifikat oder der asymmetrische Schlüssel verfügbar sein, das oder der zum Verschlüsseln der Datenbank verwendet wurde. Ohne das Zertifikat oder den asymmetrischen Schlüssel kann die Datenbank nicht wiederhergestellt werden. Darum muss das Zertifikat, das zur Verschlüsselung des Verschlüsselungsschlüssels für die Datenbank verwendet wurde, so lange beibehalten werden, wie die Sicherung benötigt wird. Weitere Informationen finden Sie unter [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

### <a name="restoring-a-database-enabled-for-vardecimal-storage"></a>Wiederherstellen einer für vardecimal-Speicher aktivierten Datenbank

Sicherungs- und Wiederherstellungsvorgänge können mit dem Speicherformat **vardecimal** ordnungsgemäß ausgeführt werden. Weitere Informationen zum Speicherformat **vardecimal** finden Sie unter [sp_db_vardecimal_storage_format](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md).

### <a name="restore-full-text-data"></a>Wiederherstellen von Volltextdaten

Bei einer vollständigen Wiederherstellung werden Volltextdaten zusammen mit anderen Datenbankdaten wiederhergestellt. Beim Verwenden der normalen Syntax `RESTORE DATABASE database_name FROM backup_device` erfolgt die Wiederherstellung der Volltextdateien bei der Wiederherstellung der Datenbankdateien.

Die RESTORE-Anweisung kann auch für folgende Wiederherstellungen verwendet werden: Wiederherstellungen auf alternativen Speicherorten, differenzielle Wiederherstellungen, Datei- und Dateigruppenwiederherstellungen sowie differenzielle Datei- und Dateigruppenwiederherstellungen von Volltextdaten. Darüber hinaus können mit RESTORE Volltextdateien ohne oder mit Datenbankdaten wiederhergestellt werden.

> [!NOTE]
> Aus [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] importierte Volltextkataloge werden immer noch als Datenbankdateien behandelt. Für diese findet weiterhin die [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]-Prozedur zur Sicherung von Volltextkatalogen Anwendung. Das Anhalten und Fortsetzen des Sicherungsvorgangs ist jedoch nicht mehr erforderlich. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von Volltextkatalogen](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md).

### [!INCLUDE [ssbigdataclusters-ss-nover](../../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE [big-data-clusters-master-instance-ha-endpoint-requirement](../../includes/big-data-clusters-master-instance-ha-endpoint-requirement.md)]

## <a name="metadata"></a>Metadaten

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] enthält Tabellen mit Sicherungs- und Wiederherstellungsverlauf, in denen die Sicherungs- und Wiederherstellungsaktivität für jede Serverinstanz nachverfolgt wird. Beim Ausführen einer Wiederherstellung werden auch die Tabellen mit Sicherungsverläufen geändert. Informationen zu diesen Tabellen finden Sie unter [Sicherungsverlauf und Headerinformationen](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md).

## <a name="replace-option-impact"></a><a name="REPLACEoption"></a> Auswirkung der REPLACE-Option
REPLACE sollte selten und nur nach sorgfältiger Überlegung verwendet werden. Die Wiederherstellung verhindert normalerweise, dass eine Datenbank versehentlich durch eine andere Datenbank überschrieben wird. Wenn die in einer RESTORE-Anweisung angegebene Datenbank bereits auf dem aktuellen Server vorhanden ist und sich die angegebene GUID der Datenbankfamilie von der im Sicherungssatz aufgezeichneten GUID der Datenbankfamilie unterscheidet, wird die Datenbank nicht wiederhergestellt. Dies ist ein wichtiges Sicherheitselement.

Die Option REPLACE überschreibt verschiedene wichtige Sicherheitsprüfungen, die die Wiederherstellung normalerweise ausführt. Folgende Prüfungen werden überschrieben:

- Wiederherstellung über eine vorhandene Datenbank mit einer von einer anderen Datenbank erstellten Sicherung.

  Mit der Option REPLACE ermöglicht die Wiederherstellung das Überschreiben einer vorhandenen Datenbank mit einer beliebigen im Sicherungssatz enthaltenen Datenbank, selbst wenn der angegebene Datenbankname sich von dem im Sicherungssatz aufgezeichneten Datenbanknamen unterscheidet. Dies kann zur Folge haben, dass eine Datenbank versehentlich durch eine andere Datenbank überschrieben wird.
  
- Wiederherstellung über eine Datenbank mithilfe des vollständigen oder massenprotokollierten Wiederherstellungsmodells, wobei keine Sicherung des Protokollfragments erstellt wurde und die Option STOPAT nicht verwendet wird.

  Mit der Option REPLACE können Sie Arbeit verlieren, für die ein Commit ausgeführt wurde, da das zuletzt geschriebene Protokoll nicht gesichert wurde.

- Überschreiben von vorhandenen Dateien.

  Beispielsweise könnten irrtümlicherweise Dateien des falschen Typs (z. B. XLS-Dateien) oder Dateien, die von einer anderen Datenbank verwendet werden, die zurzeit nicht online ist, überschrieben werden. Willkürliche Datenverluste sind möglich, wenn vorhandene Dateien überschrieben werden, auch wenn die wiederhergestellte Datenbank vollständig ist.

## <a name="redoing-a-restore"></a>Wiederholen einer Wiederherstellung
Eine Wiederherstellung kann nicht rückgängig gemacht werden. Sie können jedoch die Auswirkungen des Datenkopiervorgangs und des Rollforwards negieren, indem Sie noch einmal beginnen und pro Datei vorgehen. Um noch einmal zu starten, stellen Sie die gewünschte Datei wieder her und führen dann erneut das Rollforward aus. Wenn Sie beispielsweise versehentlich zu viele Protokollsicherungen wiederhergestellt haben und über den beabsichtigten Endpunkt hinausgegangen sind, müssen Sie die Sequenz neu starten.

Eine Wiederherstellungssequenz kann abgebrochen und neu gestartet werden, indem der gesamte Inhalt der betroffenen Dateien wiederhergestellt wird.

## <a name="reverting-a-database-to-a-database-snapshot"></a>Wiederherstellen einer Datenbank aus einer Datenbank-Momentaufnahme
Über einen *Datenbank-Wiederherstellungsvorgang* (angegeben mithilfe der Option DATABASE_SNAPSHOT) wird eine Datenbank mit einer vollständigen Quelle in einen früheren Zustand versetzt, indem sie auf den Zeitpunkt einer Datenbankmomentaufnahme wiederhergestellt wird. Das bedeutet, dass die Quelldatenbank mit Daten von dem Zeitpunkt überschrieben wird, der in der angegebenen Datenbankmomentaufnahme erfasst ist. Derzeit darf nur die Momentaufnahme vorhanden sein, aus der Sie die Datenbank wiederherstellen. Bei diesem Wiederherstellungsvorgang wird das Protokoll neu erstellt (deshalb können Sie für eine wiederhergestellte Datenbank später kein Rollforward bis zum Punkt des Benutzerfehlers durchführen).

Der Datenverlust beschränkt sich auf Updates der Datenbank, die nach der Erstellung der Momentaufnahme vorgenommen wurden. Die Metadaten einer wiederhergestellten Datenbank sind dieselben wie die Metadaten zum Zeitpunkt der Momentaufnahmeerstellung. Durch das Wiederherstellen einer Momentaufnahme werden jedoch alle Volltextkataloge gelöscht.

Die Wiederherstellung von einer Datenbankmomentaufnahme ist nicht für die Medienwiederherstellung vorgesehen. Im Gegensatz zu einem normalen Sicherungssatz ist die Datenbankmomentaufnahme eine unvollständige Kopie der Datenbankdateien. Wenn die Datenbank oder die Datenbankmomentaufnahme beschädigt ist, ist die Wiederherstellung von einer Momentaufnahme vermutlich unmöglich. Zudem ist es eher unwahrscheinlich, dass das Problem durch eine Wiederherstellung im Falle einer Beschädigung behoben wird, selbst wenn eine Wiederherstellung möglich ist.

### <a name="restrictions-on-reverting"></a>Einschränkungen zur Wiederherstellung
Die Wiederherstellung wird unter folgenden Bedingungen nicht unterstützt:

- Die Quelldatenbank enthält schreibgeschützte oder komprimierte Dateigruppen.
- Mindestens eine Datei ist offline, die beim Erstellen der Momentaufnahme online war.
- Derzeit ist mehr als eine Momentaufnahme der Datenbank vorhanden.

Weitere Informationen finden Sie unter [Wiederherstellen einer Datenbank zu einer Datenbank-Momentaufnahme](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md).

## <a name="security"></a>Sicherheit
Bei einem Sicherungsvorgang können optional Kennwörter für einen Mediensatz, einen Sicherungssatz oder für beides angegeben werden. Wurde ein Kennwort für einen Mediensatz oder Sicherungssatz definiert, müssen die richtigen Kennwörter in der RESTORE-Anweisung angegeben werden. Über diese Kennwörter werden nicht autorisierte Wiederherstellungsoptionen und unbefugtes Anfügen von Sicherungssätzen an Medien mithilfe der Tools von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verhindert. Kennwortgeschützte Medien können nicht mit der Option FORMAT der BACKUP-Anweisung überschrieben werden.

> [!IMPORTANT]
> Dieses Kennwort bietet also nur unzureichenden Schutz. Es soll vermeiden, dass autorisierte wie nicht autorisierte Benutzer mithilfe von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Tools fehlerhafte Wiederherstellungen durchführen. Es verhindert jedoch nicht das Lesen der Sicherungsdaten mit anderen Mitteln oder das Ersetzen des Kennworts. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]Eine bewährte Methode für den Schutz von Sicherungen ist das Verwahren von Sicherungsbändern an einem sicheren Ort oder das Sichern in Datenträgerdateien, die durch eine adäquate Zugriffssteuerungsliste (ACL, Access Control List) geschützt sind. Die ACLs sollten für den Verzeichnisstamm eingerichtet werden, unter dem die Sicherungen erstellt werden.

> [!NOTE]
> Spezifische Informationen zur Sicherung und Wiederherstellung von SQL Server in Microsoft Azure Blob Storage finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit Microsoft Azure Blob Storage Service](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

### <a name="permissions"></a>Berechtigungen
Ist die wiederherzustellende Datenbank nicht vorhanden, muss der Benutzer über `CREATE DATABASE`-Berechtigungen verfügen, um RESTORE ausführen zu können. Ist die Datenbank vorhanden, werden RESTORE-Berechtigungen standardmäßig den Mitgliedern der festen Serverrollen `sysadmin` und `dbcreator` sowie dem Besitzer (`dbo`) der Datenbank erteilt (für die Option `FROM DATABASE_SNAPSHOT` ist die Datenbank immer vorhanden).

RESTORE-Berechtigungen werden Rollen erteilt, in denen Mitgliedsinformationen immer für den Server verfügbar sind. Da die Mitgliedschaft in einer festen Datenbankrolle nur bei unbeschädigten und zugänglichen Datenbanken geprüft werden kann (was beim Ausführen von RESTORE nicht immer der Fall ist), verfügen Mitglieder der festen Datenbankrolle `db_owner` nicht über RESTORE-Berechtigungen.

## <a name="examples"></a><a name="examples"></a> Beispiele

Bei allen Beispielen wird davon ausgegangen, dass eine vollständige Datenbanksicherung ausgeführt wurde.

Die Beispiele zu RESTORE schließen Folgendes ein:

- A. [Wiederherstellen einer vollständigen Datenbank](#restoring_full_db)
- B. [Wiederherstellung von vollständigen und von differenziellen Datenbanksicherungen](#restoring_full_n_differential_db_backups)
- C. [Wiederherstellen einer Datenbank mit der RESTART-Syntax](#restoring_db_using_RESTART)
- D: [Wiederherstellen einer Datenbank und Verschieben von Dateien](#restoring_db_n_move_files)
- E. [Kopieren einer Datenbank mithilfe von BACKUP und RESTORE](#copying_db_using_bnr)
- F. [Wiederherstellen eines bestimmten Zeitpunkts mithilfe von STOPAT](#restoring_to_pit_using_STOPAT)
- G. [Wiederherstellen eines Transaktionsprotokolls bis zu einer Markierung](#restoring_transaction_log_to_mark)
- H. [Wiederherstellen mit der TAPE-Syntax](#restoring_using_TAPE)
- I. [Wiederherstellen mithilfe der FILE- und FILEGROUP-Syntax](#restoring_using_FILE_n_FG)
- J. [Wiederherstellen aus einer Datenbank-Momentaufnahme](#reverting_from_db_snapshot)
- K. [Wiederherstellen aus dem Microsoft Azure BLOB-Speicherdienst](#Azure_Blob)

> [!NOTE]
> Weitere Beispiele finden Sie in den Artikeln zur Vorgehensweisen zur Wiederherstellung, die unter [Übersicht über Wiederherstellungsvorgänge](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md) aufgelistet sind.

### <a name="a-restoring-a-full-database"></a><a name="restoring_full_db"></a> A. Wiederherstellen einer vollständigen Datenbank

Im folgenden Beispiel wird eine vollständige Datenbanksicherung vom logischen Sicherungsmedium `AdventureWorksBackups` wiederhergestellt. Ein Beispiel für das Erstellen dieses Mediums finden Sie unter [Sicherungsmedien](../../relational-databases/backup-restore/backup-devices-sql-server.md).

```sql
RESTORE DATABASE AdventureWorks2012
  FROM AdventureWorks2012Backups;
```

> [!NOTE]
> Bei einer Datenbank, die das Modell der vollen oder massenprotokollierten Wiederherstellung verwendet, erfordert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in den meisten Fällen die Sicherung des Protokollfragments, bevor die Datenbank wiederhergestellt wird. Weitere Informationen finden Sie unter [Protokollfragmentsicherungen](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).

[&#91;Anfang der Beispiele&#93;](#examples)

### <a name="b-restoring-full-and-differential-database-backups"></a><a name="restoring_full_n_differential_db_backups"></a> B. Wiederherstellung von vollständigen und von differenziellen Datenbanksicherungen

Im folgenden Beispiel wird eine vollständige Datenbanksicherung und anschließend eine differenzielle Sicherung vom `Z:\SQLServerBackups\AdventureWorks2012.bak`-Sicherungsmedium wiederhergestellt, das beide Sicherungen enthält. Die vollständige Datenbanksicherung, die wiederhergestellt werden soll, ist der sechste Sicherungssatz auf dem Medium (`FILE = 6`). Die differenzielle Sicherung ist der neunte Sicherungssatz auf dem Medium (`FILE = 9`). Sobald die differenzielle Sicherung wiederhergestellt wird, wird die Datenbank wiederhergestellt.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak'
    WITH FILE = 6
      NORECOVERY;
RESTORE DATABASE AdventureWorks2012
    FROM DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak'
    WITH FILE = 9
      RECOVERY;
```

[&#91;Anfang der Beispiele&#93;](#examples)

### <a name="c-restoring-a-database-using-restart-syntax"></a><a name="restoring_db_using_RESTART"></a> C. Wiederherstellen einer Datenbank mit der RESTART-Syntax

Im folgenden Beispiel wird die Option `RESTART` zum Neustart eines `RESTORE`-Vorgangs verwendet, der durch einen Stromausfall des Servers unterbrochen wurde.

```sql
-- This database RESTORE halted prematurely due to power failure.
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups;
-- Here is the RESTORE RESTART operation.
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups WITH RESTART;
```

[&#91;Anfang der Beispiele&#93;](#examples)

### <a name="d-restoring-a-database-and-move-files"></a><a name="restoring_db_n_move_files"></a> D. Wiederherstellen einer Datenbank und Verschieben von Dateien

Im folgenden Beispiel wird eine vollständige Datenbank und ein Transaktionsprotokoll wiederhergestellt. Anschließend wird die wiederhergestellte Datenbank in das Verzeichnis `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data` verschoben.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups
    WITH NORECOVERY,
      MOVE 'AdventureWorks2012_Data' TO
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\NewAdvWorks.mdf',
      MOVE 'AdventureWorks2012_Log'
TO 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\NewAdvWorks.ldf';
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH RECOVERY;
```

[&#91;Anfang der Beispiele&#93;](#examples)

### <a name="e-copying-a-database-using-backup-and-restore"></a><a name="copying_db_using_bnr"></a> E. Kopieren einer Datenbank mithilfe von BACKUP und RESTORE

Im folgenden Beispiel werden die Anweisungen `BACKUP` und `RESTORE` verwendet, um eine Kopie der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank zu erstellen. Die `MOVE`-Anweisung bewirkt, dass die Daten- und die Protokolldatei an den angegebenen Speicherorten wiederhergestellt werden. Die `RESTORE FILELISTONLY`-Anweisung wird verwendet, um die Anzahl und die Namen der Dateien der Datenbank zu bestimmen, die wiederhergestellt werden. Die neue Kopie der Datenbank erhält den Namen `TestDB`. Weitere Informationen finden Sie unter [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).

```sql
BACKUP DATABASE AdventureWorks2012
    TO AdventureWorksBackups ;

RESTORE FILELISTONLY
    FROM AdventureWorksBackups ;

RESTORE DATABASE TestDB
    FROM AdventureWorksBackups
    WITH MOVE 'AdventureWorks2012_Data' TO 'C:\MySQLServer\testdb.mdf',
    MOVE 'AdventureWorks2012_Log' TO 'C:\MySQLServer\testdb.ldf';
GO
```

[&#91;Anfang der Beispiele&#93;](#examples)

### <a name="f-restoring-to-a-point-in-time-using-stopat"></a><a name="restoring_to_pit_using_STOPAT"></a> F. Wiederherstellen eines bestimmten Zeitpunkts mithilfe von STOPAT

Im folgenden Beispiel wird eine Datenbank in den am `12:00 AM` um `April 15, 2020` bestehenden Status wiederhergestellt und ein Wiederherstellungsvorgang gezeigt, der mehrere Protokollsicherungen umfasst. Auf dem Sicherungsmedium ( `AdventureWorksBackups`) ist die wiederherzustellende vollständige Datenbanksicherung der dritte Sicherungssatz (`FILE = 3`), die erste Protokollsicherung ist der vierte Sicherungssatz (`FILE = 4`), und die zweite Protokollsicherung ist der fünfte Sicherungssatz (`FILE = 5`).

```sql
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=3, NORECOVERY;
  
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=4, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';
  
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=5, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';
RESTORE DATABASE AdventureWorks2012 WITH RECOVERY;

```

[&#91;Anfang der Beispiele&#93;](#examples)

### <a name="g-restoring-the-transaction-log-to-a-mark"></a><a name="restoring_transaction_log_to_mark"></a> G. Wiederherstellen eines Transaktionsprotokolls bis zu einer Markierung

Im folgenden Beispiel wird das Transaktionsprotokoll bis zur Markierung in der markierten Transaktion mit dem Namen `ListPriceUpdate`wiederhergestellt.

```sql
USE AdventureWorks2012
GO
BEGIN TRANSACTION ListPriceUpdate
    WITH MARK 'UPDATE Product list prices';
GO

UPDATE Production.Product
    SET ListPrice = ListPrice * 1.10
    WHERE ProductNumber LIKE 'BK-%';
GO

COMMIT TRANSACTION ListPriceUpdate;
GO

-- Time passes. Regular database
-- and log backups are taken.
-- An error occurs in the database.
USE master;
GO

RESTORE DATABASE AdventureWorks2012
FROM AdventureWorksBackups
WITH FILE = 3, NORECOVERY;
GO

RESTORE LOG AdventureWorks2012
  FROM AdventureWorksBackups
    WITH FILE = 4,
    RECOVERY,
    STOPATMARK = ListPriceUpdate;
```

[&#91;Anfang der Beispiele&#93;](#examples)

### <a name="h-restoring-using-tape-syntax"></a><a name="restoring_using_TAPE"></a> H. Wiederherstellen mit der TAPE-Syntax

Im folgenden Beispiel wird eine vollständige Datenbanksicherung von einem `TAPE`-Sicherungsmedium wiederhergestellt.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM TAPE = '\\.\tape0';
```

[&#91;Anfang der Beispiele&#93;](#examples)

### <a name="i-restoring-using-file-and-filegroup-syntax"></a><a name="restoring_using_FILE_n_FG"></a> I. Wiederherstellen mithilfe der FILE- und FILEGROUP-Syntax

Im folgenden Beispiel wird eine Datenbank mit dem Namen `MyDatabase` wiederhergestellt, die über zwei Dateien, eine sekundäre Dateigruppe und ein Transaktionsprotokoll verfügt. Für die Datenbank wird das vollständige Wiederherstellungsmodell verwendet.

Die Datenbanksicherung ist der neunte Sicherungssatz im Mediensatz auf einem logischen Sicherungsmedium mit dem Namen `MyDatabaseBackups`. Anschließend werden drei Protokollsicherungen, die sich in den nächsten drei Sicherungssätzen (`10`, `11` und `12`) im Medium `MyDatabaseBackups` befinden, mit `WITH NORECOVERY` wiederhergestellt. Nach dem Wiederherstellen der letzten Protokollsicherung wird die Datenbank wiederhergestellt.

> [!NOTE]
> Die Wiederherstellung wird in einem separaten Schritt ausgeführt, um zu vermeiden, dass sie zu früh ausgeführt wird, d. h., bevor alle Protokollsicherungen wiederhergestellt wurden. Informationen zum Wiederherstellungsprozess finden Sie unter [ (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery).

Beachten Sie, dass in der `RESTORE DATABASE`-Anweisung zwei Typen von `FILE`-Optionen vorhanden sind. Mit den `FILE`-Optionen vor dem Namen des Sicherungsmediums werden die logischen Dateinamen der Datenbankdateien angegeben, die aus dem Sicherungssatz wiederhergestellt werden sollen, beispielsweise `FILE = 'MyDatabase_data_1'`. Dieser Sicherungssatz ist nicht die erste Datenbanksicherung im Mediensatz. Daher wird seine Position im Mediensatz mit der Option `FILE` in der `WITH`-Klausel angegeben: `FILE=9`.

```sql
RESTORE DATABASE MyDatabase
    FILE = 'MyDatabase_data_1',
    FILE = 'MyDatabase_data_2',
    FILEGROUP = 'new_customers'
    FROM MyDatabaseBackups
    WITH
      FILE = 9,
      NORECOVERY;
GO  
-- Restore the log backups
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 10,
      NORECOVERY;
GO
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 11,
      NORECOVERY;
GO
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 12,
      NORECOVERY;
GO
--Recover the database
RESTORE DATABASE MyDatabase WITH RECOVERY;
GO
```

[&#91;Anfang der Beispiele&#93;](#examples)

### <a name="j-reverting-from-a-database-snapshot"></a><a name="reverting_from_db_snapshot"></a> J. Wiederherstellen aus einer Datenbank-Momentaufnahme

Im folgenden Beispiel wird eine Datenbank aus einer Datenbank-Momentaufnahme wiederhergestellt. In diesem Beispiel wird davon ausgegangen, dass derzeit in der Datenbank nur eine Momentaufnahme vorhanden ist. Ein Beispiel zum Erstellen dieser Datenbankmomentaufnahme finden Sie unter [Erstellen einer Datenbankmomentaufnahme](../../relational-databases/databases/create-a-database-snapshot-transact-sql.md).

> [!NOTE]
> Durch das Wiederherstellen einer Momentaufnahme werden alle Volltextkataloge gelöscht.

```sql
USE master;
RESTORE DATABASE AdventureWorks2012 FROM DATABASE_SNAPSHOT = 'AdventureWorks_dbss1800';
GO
```

Weitere Informationen finden Sie unter [Wiederherstellen einer Datenbank zu einer Datenbank-Momentaufnahme](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md).

[&#91;Anfang der Beispiele&#93;](#examples)

### <a name="k-restoring-from-the-microsoft-azure-blob-storage-service"></a><a name="Azure_Blob"></a> K. Wiederherstellen aus dem Microsoft Azure BLOB-Speicherdienst

Die drei folgenden Beispiele umfassen die Verwendung des Microsoft Azure-Speicherdiensts. Der Speicherkontoname lautet `mystorageaccount`. Der Container für Datendateien heißt `myfirstcontainer`. Der Container für Sicherungsdateien heißt `mysecondcontainer`. Eine gespeicherte Zugriffsrichtlinie wurde mit Lese-, Schreib-, Lösch- und Auflistungsrechten für jeden Container erstellt. Die SQL Server Anmeldeinformationen wurden mit einer gespeicherten Zugriffssignatur erstellt, die der gespeicherten Zugriffsrichtlinie zugeordnet ist. Spezifische Informationen zur Sicherung und Wiederherstellung von SQL Server in Microsoft Azure Blob Storage finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit Microsoft Azure Blob Storage Service](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

**K1. Wiederherstellen einer vollständigen Datenbanksicherung aus dem Microsoft Azure-Speicherdienst**    
Eine vollständige Datenbanksicherung in `mysecondcontainer` von `Sales` wird in `myfirstcontainer` wiederhergestellt. `Sales` ist auf dem Server derzeit nicht vorhanden.

```sql
RESTORE DATABASE Sales
  FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/Sales.bak'
  WITH MOVE 'Sales_Data1' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_Data1.mdf',
  MOVE 'Sales_log' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_log.ldf',
  STATS = 10;
```

**K2. Wiederherstellen einer vollständigen Datenbanksicherung aus dem Microsoft Azure-Speicherdienst in den lokalen Speicher** Eine vollständige Datenbanksicherung in `mysecondcontainer` von `Sales` wird im lokalen Speicher wiederhergestellt. `Sales` ist auf dem Server derzeit nicht vorhanden.

```sql
RESTORE DATABASE Sales
  FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/Sales.bak'
  WITH MOVE 'Sales_Data1' to 'H:\DATA\Sales_Data1.mdf',
  MOVE 'Sales_log' to 'O:\LOG\Sales_log.ldf',
  STATS = 10;
```

**K3. Wiederherstellen einer vollständigen Datenbanksicherung aus dem lokalen Speicher in den Microsoft Azure-Speicherdienst**

```sql
RESTORE DATABASE Sales
  FROM DISK = 'E:\BAK\Sales.bak'
  WITH MOVE 'Sales_Data1' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_Data1.mdf',
  MOVE 'Sales_log' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_log.ldf',
  STATS = 10;
```

[&#91;Anfang der Beispiele&#93;](#examples)

## <a name="more-information"></a>Weitere Informationen

[Übersicht über Wiederherstellungsvorgänge (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery)     
[Sichern und Wiederherstellen von SQL Server-Datenbanken](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)    
[Sichern und Wiederherstellen von Systemdatenbanken (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)      
[Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)     
[Sichern und Wiederherstellen von Volltextkatalogen und Indizes](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)      
[Sichern und Wiederherstellen von replizierten Datenbanken](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)      
[BACKUP](../../t-sql/statements/restore-statements-transact-sql.md)      
[Mediensätze, Medienfamilien und Sicherungssätze](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)      
[RESTORE REWINDONLY](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)     
[RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)     
[RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)     
[RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)     
[Sicherungsverlauf und Headerinformationen](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)       

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](restore-statements-transact-sql.md?view=sql-server-2017&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Verwaltete Azure SQL-Instanz

Über diesen Befehl können Sie eine komplette Datenbank aus einer vollständigen Datenbanksicherung in einem Azure Blob Storage-Konto wiederherstellen (vollständige Wiederherstellung).

Andere unterstützte RESTORE-Befehle finden Sie hier:

- [RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)
- [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)
- [RESTORE LABELONLY ONLY (Transact-SQL)](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)
- [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)

> [!IMPORTANT]
> Informationen zur Wiederherstellung mit automatischen Sicherungen von SQL Managed Instance finden Sie unter [SQL-Datenbank-Wiederherstellung](/azure/sql-database/sql-database-recovery-using-backups).

## <a name="syntax"></a>Syntax

```syntaxsql
--To Restore an Entire Database from a Full database backup (a Complete Restore):
RESTORE DATABASE { database_name | @database_name_var }
 FROM URL = { 'physical_device_name' | @physical_device_name_var } [ ,...n ]
[;]

```

## <a name="arguments"></a>Argumente

DATABASE

Gibt die Zieldatenbank an.

FROM URL

Gibt ein oder mehrere Sicherungsmedien an, die mit URLs angegeben sind, die für die Wiederherstellung verwendet werden. Das URL-Format wird zur Wiederherstellung von Sicherungen aus dem Microsoft Azure-Speicherdienst verwendet.

> [!IMPORTANT]
> Wenn Sie eine Wiederherstellung von mehreren Geräten durchführen möchten, müssen Sie Shared Access Signature-Token (SAS) verwenden, wenn Sie die Wiederherstellung über eine URL durchführen. Beispiele für die Erstellung einer Shared Access Signature finden Sie unter [SQL Server-Sicherung über URLs](../../relational-databases/backup-restore/sql-server-backup-to-url.md) und [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell) (Vereinfachen der Erstellung von SQL-Anmeldeinformationen mit Shared Access Signature-Token in Azure Storage mit PowerShell).

*n* Ein Platzhalter, der anzeigt, dass in einer durch Trennzeichen getrennten Liste möglicherweise bis zu 64 Sicherungsmedien angegeben werden.

## <a name="general-remarks"></a>Allgemeine Hinweise

Als Voraussetzung müssen Sie Anmeldeinformationen mit dem Namen erstellen, der mit der URL des Blob Storage-Kontos übereinstimmt, und eine Shared Access Signature als Geheimnis verwenden. Der RESTORE-Befehl sucht anhand der Blob Storage-URL nach Anmeldeinformationen, die erforderlich sind, um das Sicherungsmedium auszulesen.

Der Wiederherstellungsvorgang erfolgt asynchron. Die Wiederherstellung wird fortgesetzt, selbst wenn die Verbindung mit dem Client unterbrochen wird. Wenn Ihre Verbindung getrennt wird, können Sie die Ansicht [sys.dm_operation_status](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md) auf den Status eines Wiederherstellungsvorgangs überprüfen (sowie für CREATE- und DROP-Datenbanken).

Die folgenden Datenbankoptionen werden festgelegt oder überschrieben und können später nicht geändert werden:

- NEW_BROKER (wenn der Broker nicht in der BAK-Datei aktiviert ist)
- ENABLE_BROKER (wenn der Broker nicht in der BAK-Datei aktiviert ist)
- AUTO_CLOSE=OFF (wenn für eine Datenbank in der BAK-Datei AUTO_CLOSE=ON festgelegt ist)
- RECOVERY FULL (wenn eine Datenbank in der BAK-Datei über den Wiederherstellungsmodus SIMPLE oder BULK_LOGGED verfügt)
- Eine arbeitsspeicheroptimierte Dateigruppe wird hinzugefügt und hat XTP aufgerufen, wenn sie nicht in der BAK-Quelldatei war. Alle vorhandenen arbeitsspeicheroptimierten Dateigruppen werden in XTP umbenannt.
- Die Optionen SINGLE_USER und RESTRICTED_USER werden in MULTI_USER konvertiert.

## <a name="limitations---sql-managed-instance"></a>Einschränkungen von SQL Managed Instance

Diese Einschränkungen gelten:

- BAK-Dateien, die mehrere Sicherungssätze enthalten, können nicht wiederhergestellt werden.
- BAK-Dateien, die mehrere Protokolldateien enthalten, können nicht wiederhergestellt werden.
- Die Wiederherstellung schlägt fehl, wenn die BAK-Datei FILESTREAM-Daten enthält.
- Sicherungen, die Datenbanken enthalten, die über aktive In-Memory-Objekte verfügen, können nicht für die Leistungsstufe „Universell“ wiederhergestellt werden.
- Sicherungen, die Datenbanken im schreibgeschützten Modus enthalten, können derzeit nicht wiederhergestellt werden. Diese Einschränkung wird bald entfernt.

Weitere Informationen finden Sie unter [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance).

## <a name="restoring-an-encrypted-database"></a>Wiederherstellen einer verschlüsselten Datenbank
Um eine verschlüsselte Datenbank wiederherstellen zu können, muss das Zertifikat oder der asymmetrische Schlüssel verfügbar sein, das oder der zum Verschlüsseln der Datenbank verwendet wurde. Ohne das Zertifikat oder den asymmetrischen Schlüssel kann die Datenbank nicht wiederhergestellt werden. Darum muss das Zertifikat, das zur Verschlüsselung des Verschlüsselungsschlüssels für die Datenbank verwendet wurde, so lange beibehalten werden, wie die Sicherung benötigt wird. Weitere Informationen finden Sie unter [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

## <a name="permissions"></a>Berechtigungen
Der Benutzer muss über `CREATE DATABASE`-Berechtigungen verfügen, um RESTORE ausführen zu können.

```sql
CREATE LOGIN mylogin WITH PASSWORD = 'Very Strong Pwd123!';
GRANT CREATE ANY DATABASE TO [mylogin];
```

RESTORE-Berechtigungen werden Rollen erteilt, in denen Mitgliedsinformationen immer für den Server verfügbar sind. Da die Mitgliedschaft in einer festen Datenbankrolle nur bei unbeschädigten und zugänglichen Datenbanken geprüft werden kann (was beim Ausführen von RESTORE nicht immer der Fall ist), verfügen Mitglieder der festen Datenbankrolle `db_owner` nicht über RESTORE-Berechtigungen.

## <a name="examples"></a><a name="examples"></a> Beispiele

In den folgenden Beispiele wird eine Kopiesicherung der Datenbank aus einer URL wiederhergestellt, und es werden Anmeldeinformationen erstellt.

### <a name="a-restore-database-from-four-backup-devices"></a><a name="restore-mi-database"></a> A. Wiederherstellen einer Datenbank aus vier Sicherungsmedien

```sql

-- Create credential
CREATE CREDENTIAL [https://mybackups.blob.core.windows.net/wide-world-importers]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
      SECRET = 'sv=2017-11-09&ss=bq&srt=sco&sp=rl&se=2022-06-19T22:41:07Z&st=2018-06-01T14:41:07Z&spr=https&sig=s7wddcf0w%3D';
GO
-- Restore database
RESTORE DATABASE WideWorldImportersStandard
FROM URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/00-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/01-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/02-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/03-WideWorldImporters-Standard.bak'
```

Der folgende Fehler wird angezeigt, wenn die Datenbank bereits vorhanden ist:

```
Msg 1801, Level 16, State 1, Line 9
Database 'WideWorldImportersStandard' already exists. Choose a different database name.
```

### <a name="b-restore-database-specified-via-variable"></a><a name="restore-mi-database-variables"></a> B. Wiederherstellen einer per Variable angegebenen Datenbank

```sql
DECLARE @db_name sysname = 'WideWorldImportersStandard';
DECLARE @url nvarchar(400) = N'https://mybackups.blob.core.windows.net/wide-world-importers/WideWorldImporters-Standard.bak';

RESTORE DATABASE @db_name
FROM URL = @url
```

### <a name="c-track-progress-of-restore-statement"></a><a name="restore-mi-database-progress"></a> C. Nachverfolgen des Fortschritts einer RESTORE-Anweisung

```sql
SELECT query = a.text, start_time, percent_complete,
    eta = dateadd(second,estimated_completion_time/1000, getdate())
FROM sys.dm_exec_requests r
    CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
WHERE r.command = 'RESTORE DATABASE'
```

> [!Note]
> Diese Ansicht zeigt wahrscheinlich zwei Wiederherstellungsanforderungen an. Die eine ist die ursprüngliche, vom Client gesendete RESTORE-Anweisung, die andere ist eine RESTORE-Hintergrundanweisung, die auch dann ausgeführt wird, wenn keine Verbindung mit dem Client hergestellt werden kann.

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](restore-statements-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](restore-statements-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Analyseplattformsystem

Wiederherstellen einer [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Benutzerdatenbank von einer Datenbanksicherung auf einer [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Appliance. Die Datenbank wird anhand einer Sicherung wiederhergestellt, die zuvor mithilfe des [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Befehls [BACKUP DATABASE – Analytics Platform System](../../t-sql/statements/backup-transact-sql.md) erstellt wurde. Mit BACKUP- und RESTORE-Vorgängen können Sie einen Notfallwiederherstellungsplan erstellen oder Datenbanken von einer Appliance zur anderen verschieben.

> [!NOTE]
> Zum Wiederherstellen der Masterdatenbank müssen die Anmeldeinformationen der Appliance wiederhergestellt werden. Verwenden Sie zum Wiederherstellen der Masterdatenbank im Tool **Configuration Manager** die Seite [Wiederherstellen der Masterdatenbank](../../relational-databases/backup-restore/restore-the-master-database-transact-sql.md). Dieser Vorgang kann von einem Administrator mit Zugriff auf den Steuerknoten ausgeführt werden. Weitere Informationen zu [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Datenbanksicherungen finden Sie unter „Sichern und Wiederherstellen“ in der [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

## <a name="syntax"></a>Syntax

```syntaxsql

-- Restore the master database
-- Use the Configuration Manager tool.

Restore a full user database backup.
RESTORE DATABASE database_name
    FROM DISK = '\\UNC_path\full_backup_directory'
[;]

--Restore a full user database backup and then a differential backup.
RESTORE DATABASE database_name
    FROM DISK = '\\UNC_path\differential_backup_directory'
    WITH [ ( ] BASE = '\\UNC_path\full_backup_directory' [ ) ]
[;]

--Restore header information for a full or differential user database backup.
RESTORE HEADERONLY
    FROM DISK = '\\UNC_path\backup_directory'
[;]
```

## <a name="arguments"></a>Argumente

RESTORE DATABASE *database_name* Gibt an, dass eine Benutzerdatenbank in einer Datenbank mit dem Namen *database_name* wiederhergestellt wird. Die wiederhergestellte Datenbank kann anders benannt werden als die Quelldatenbank, die gesichert wurde. *database_name* darf nicht bereits als Datenbank auf der Zielappliance vorhanden sein. Weitere Informationen zu zulässigen Datenbanknamen finden Sie in der [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] unter „Objektbenennungsregeln“.

Beim Wiederherstellen einer Benutzerdatenbank wird eine vollständige Datenbanksicherung wiederhergestellt. Anschließend kann optional eine differenzielle Sicherung auf der Appliance wiederhergestellt werden. Das Wiederherstellen einer Benutzerdatenbank beinhaltet das Wiederherstellen von Datenbankbenutzern und Datenbankrollen.

FROM DISK = '\\\\*UNC_path*\\*backup_directory*' Netzwerkpfad und Verzeichnis, aus denen [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] die Sicherungsdateien wiederherstellt. Ein Beispiel: FROM DISK = '\\\xxx.xxx.xxx.xxx\backups\2012\Monthly\08.2012.Mybackup‘.

*backup_directory* Gibt den Namen eines Verzeichnisses an, das die vollständige oder differenzielle Sicherung enthält. Beispielsweise kann für eine vollständige oder differenzielle Sicherung ein RESTORE HEADERONLY-Vorgang ausgeführt werden.

*full_backup_directory* Gibt den Namen eines Verzeichnisses an, das die vollständige Sicherung enthält.

*differential_backup_directory* Gibt den Namen eines Verzeichnisses an, das die differenzielle Sicherung enthält.

- Pfad und Sicherungsverzeichnis müssen bereits vorhanden sein und als vollqualifizierter UNC-Pfad (Universal Naming Convention) angegeben werden.
- Beim Pfad zum Sicherungsverzeichnis darf es sich nicht um einen lokalen Pfad handeln, und der Speicherort darf sich nicht auf einem [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Applianceknoten befinden.
- Die maximale Länge des UNC-Pfads und des Sicherungsverzeichnisnamens beträgt 200 Zeichen.
- Der Server oder Host muss als IP-Adresse angegeben werden.

RESTORE HEADERONLY Gibt an, dass nur die Headerinformationen für die Sicherung einer Benutzerdatenbank zurückgegeben werden sollen. Der Header enthält u.a. die Textbeschreibung für die Sicherung und den Sicherungsnamen. Der Sicherungsname und der Name des Verzeichnisses, in dem die Sicherungsdateien gespeichert werden, müssen nicht identisch sein.

Die RESTORE HEADERONLY-Ergebnisse entsprechen dem Muster der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-RESTORE HEADERONLY-Ergebnisse. Das Ergebnis enthält mehr als 50 Spalten, die nicht alle von [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] verwendet werden. Eine Beschreibung der Spalten der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-RESTORE HEADERONLY-Ergebnisse finden Sie unter [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md).

## <a name="permissions"></a>Berechtigungen
Erfordert die `CREATE ANY DATABASE`-Berechtigung.

Es ist ein Windows-Konto mit der Berechtigung erforderlich, auf das Sicherungsverzeichnis zuzugreifen und es zu lesen. Sie müssen außerdem den Windows-Kontonamen und das Kennwort in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] speichern.

- Überprüfen Sie mit [sys.dm_pdw_network_credentials](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md), ob die Anmeldeinformationen bereits vorhanden sind.
- Verwenden Sie [sp_pdw_add_network_credentials – [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md) zum Hinzufügen oder Aktualisieren der Anmeldeinformationen.
- Mit [sp_pdw_remove_network_credentials – [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md) können Sie Anmeldeinformationen aus [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] entfernen.

## <a name="error-handling"></a>Fehlerbehandlung

Der RESTORE DATABASE-Befehl führt unter den folgenden Bedingungen zu einem Fehler:

- Der Name der Datenbank, die wiederhergestellt werden soll, ist auf der Zielappliance bereits vorhanden. Wählen Sie daher einen eindeutigen Datenbanknamen, oder löschen Sie die vorhandene Datenbank, bevor Sie die Wiederherstellung ausführen.
- Im Sicherungsverzeichnis befinden sich mehrere ungültige Sicherungsdateien.
- Die Anmeldeberechtigungen reichen zum Wiederherstellen einer Datenbank nicht aus.
- [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] verfügt nicht über die erforderlichen Berechtigungen für den Netzwerkspeicherort, an dem die Sicherungsdateien gespeichert sind.
- Der Netzwerkspeicherort für das Sicherungsverzeichnis ist nicht vorhanden oder nicht verfügbar.
- Es ist nicht genügend Speicherplatz auf den Computeknoten oder auf dem Steuerknoten vorhanden. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] bestätigt nicht, dass auf der Appliance genügend Speicherplatz vorhanden ist, bevor das Wiederherstellen gestartet wird. Daher kann beim Ausführen der RESTORE DATABASE-Anweisung ein Fehler wegen unzureichendem Speicherplatz generiert werden. Wenn nicht genügend Speicherplatz vorhanden ist, führt [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ein Rollback für die Wiederherstellung aus.
- Die Zielappliance, auf der die Datenbank wiederhergestellt wird, verfügt über weniger Computeknoten als die Quellappliance, von der die Datenbank gesichert wurde.
- Die Wiederherstellung der Datenbank wird aus einer Transaktion heraus ausgeführt.

## <a name="general-remarks"></a>Allgemeine Hinweise

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] verfolgt, ob die Wiederherstellung der Datenbank erfolgreich ist. Vor dem Wiederherstellen einer differenziellen Datenbanksicherung wird durch [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] überprüft, ob die vollständige Datenbankwiederherstellung erfolgreich abgeschlossen wurde.

Nach der Wiederherstellung verfügt die Benutzerdatenbank über einen Datenbank-Kompatibilitätsgrad von 120. Dies gilt für alle Datenbanken, unabhängig vom ursprünglichen Kompatibilitätsgrad.

## <a name="restoring-to-an-appliance-with-a-larger-number-of-compute-nodes"></a>Wiederherstellen auf einer Appliance mit einer größeren Anzahl von Computeknoten

Führen Sie nach dem Wiederherstellen einer Datenbank von einer kleineren auf eine größere Appliance [DBCC SHRINKLOG ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) aus, da das Transaktionsprotokoll durch die Umverteilung vergrößert wird.

Beim Wiederherstellen einer Sicherung auf einer Appliance mit einer größeren Anzahl von Computeknoten wächst die Größe der zugeordneten Datenbank entsprechend der Anzahl der Computeknoten.

Beispiel: Wenn Sie eine 60-GB-Datenbank von einer 2-Knoten-Appliance (30 GB pro Knoten) auf einer 6-Knoten-Appliance wiederherstellen, erstellt [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] auf der 6-Knoten-Appliance eine 180-GB-Datenbank (6 Knoten à 30 GB). Zunächst stellt [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] die Datenbank entsprechend der Quellkonfiguration auf 2 Knoten wieder her und verteilt sie anschließend auf alle 6 Knoten.

Nach der Umverteilung enthält jeder Computeknoten weniger tatsächliche Daten und mehr freien Speicherplatz als die einzelnen Computeknoten auf der kleineren Quellappliance. Dank des zusätzlichen Speicherplatzes können Sie der Datenbank weitere Daten hinzufügen. Ist die wiederhergestellte Datenbank größer als erforderlich, können Sie mit [sp_pdw_remove_network_credentials – SQL Data Warehouse](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true) die Größe der Datenbank verringern.

## <a name="limitations-and-restrictions"></a>Einschränkungen

Für diese Einschränkungen gilt: Die Quellappliance ist die Appliance, von der aus die Sicherung der Datenbank erstellt wurde, und die Zielappliance ist die Appliance, auf der die Datenbank wiederhergestellt wird.

- Beim Wiederherstellen einer Datenbank wird nicht automatisch die Statistik neu erstellt.
- Auf der Appliance kann nur eine RESTORE DATABASE- oder BACKUP DATABASE-Anweisung gleichzeitig ausgeführt werden. Werden mehrere BACKUP- und RESTORE-Anweisungen gleichzeitig gesendet, werden sie in eine Warteschlange eingereiht und nacheinander verarbeitet.
- Eine Datenbanksicherung kann nur auf einer [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Zielappliance wiederhergestellt werden, die eine gleiche oder höhere Anzahl von Computeknoten besitzt wie die Quellappliance. Die Zielappliance darf nicht über weniger Computeknoten verfügen als die Quellappliance.
- Es ist nicht möglich, eine Sicherung, die auf einer Appliance mit SQL Server 2012 PDW-Hardware erstellt wurde, auf einer Appliance mit SQL Server 2008 R2-Hardware wiederherzustellen. Dies gilt auch, wenn die Appliance ursprünglich mit der SQL Server 2008 R2-PDW-Hardware gekauft wurde und mittlerweile mit SQL Server 2012 PDW-Software ausgeführt wird.

## <a name="locking"></a>Sperren
Sperrt exklusiv das DATABASE-Objekt.

## <a name="examples"></a>Beispiele

### <a name="a-simple-restore-examples"></a>A. Einfache Beispiele für RESTORE

Im folgenden Beispiel wird eine vollständige Sicherung auf einer `SalesInvoices2013`-Datenbank wiederhergestellt. Die Sicherungsdateien werden im Verzeichnis `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full` gespeichert. Die Datenbank „SalesInvoices2013“ darf auf der Zielappliance nicht bereits vorhanden sein. Andernfalls erzeugt dieser Befehl einen Fehler.

```sql
RESTORE DATABASE SalesInvoices2013
FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full';
```

### <a name="b-restore-a-full-and-differential-backup"></a>B. Wiederherstellen einer vollständigen und differenziellen Sicherung

Im folgenden Beispiel wird zunächst eine vollständige, anschließend eine differenzielle Sicherung auf der Datenbank „SalesInvoices2013“ wiederhergestellt.

Die vollständige Sicherung der Datenbank wird von der vollständigen Sicherung im Verzeichnis `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full` wiederhergestellt. Nach der erfolgreichen Wiederherstellung wird die differenzielle Sicherung auf der Datenbank „SalesInvoices2013“ wiederhergestellt. Die differenzielle Sicherung wird im Verzeichnis `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff` gespeichert.

```syntaxsql
RESTORE DATABASE SalesInvoices2013
    FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff'
    WITH BASE = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full'
[;]

```

### <a name="c-restoring-the-backup-header"></a>C. Wiederherstellen des Sicherungsheaders

In diesem Beispiel werden die Headerinformationen für die Datenbanksicherung `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full` wiederhergestellt. Der Befehl ergibt eine Zeile mit Informationen für die Sicherung von „Invoices2013Full“.

```syntaxsql
RESTORE HEADERONLY
    FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full'
[;]
```

Sie können die Headerinformationen verwenden, um den Inhalt einer Sicherung zu überprüfen. Sie können damit auch sicherstellen, dass die Zielappliance für die Wiederherstellung mit der Quellappliance der Sicherung kompatibel ist, bevor Sie mit dem Wiederherstellen der Sicherung beginnen.

## <a name="see-also"></a>Weitere Informationen
[BACKUP DATABASE – Analytics Platform System](../../t-sql/statements/backup-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)     

::: moniker-end