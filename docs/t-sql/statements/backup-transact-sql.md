---
description: BACKUP (Transact-SQL)
title: BACKUP (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/22/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BACKUP_TSQL
- BACKUP
- BACKUP_DATABASE_TSQL
- BACKUP_LOG_TSQL
- BACKUP LOG
- BACKUP DATABASE
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], BACKUP statement
- backing up filegroups [SQL Server]
- backup file formats [SQL Server]
- backups [SQL Server]
- database backups [SQL Server], BACKUP statement
- multifamily media sets [SQL Server]
- mirrored media sets [SQL Server]
- backing up files [SQL Server]
- backup sets [SQL Server], BACKUP statement
- backup compression [SQL Server], BACKUP statement
- BACKUP statement
- backups [SQL Server], BACKUP statement
- backup history tables [SQL Server]
- transaction log backups [SQL Server], BACKUP LOG statement
- READ_WRITE_FILEGROUPS option
- BACKUP DATABASE statement
- concurrency [SQL Server], backups
- backing up databases [SQL Server]
- passwords [SQL Server], backups
- security [SQL Server], backups
- media families [SQL Server]
- BACKUP LOG statement
- backing up transaction logs [SQL Server]
- stripe sets [SQL Server]
- cross-platform backups
ms.assetid: 89a4658a-62f1-4289-8982-f072229720a1
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||>=aps-pdw-2016'
ms.openlocfilehash: 6025ca2bec08327f668a437b5428519cf85790dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075775"
---
# <a name="backup-transact-sql"></a>BACKUP (Transact-SQL)

Eine SQL-Datenbank wird gesichert.

Klicken Sie auf eine der folgenden Registerkarten, um Syntax, Argumente, Hinweise, Berechtigungen und Beispiele für eine bestimmte SQL-Version anzuzeigen, mit der Sie arbeiten.

Weitere Informationen zu Syntaxkonventionen finden Sie unter [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Managed Instance](backup-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](backup-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

Sichert eine vollständige [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank, um eine Datenbanksicherung zu erstellen, oder eine oder mehrere Dateien oder Dateigruppen, um eine Dateisicherung (BACKUP DATABASE) zu erstellen. Sichert bei Verwendung des vollständigen oder massenprotokollierten Wiederherstellungsmodells auch das Transaktionsprotokoll der Datenbank, um eine Protokollsicherung (BACKUP LOG) zu erstellen.

## <a name="syntax"></a>Syntax

```syntaxsql
--Backing Up a Whole Database
BACKUP DATABASE { database_name | @database_name_var }
  TO <backup_device> [ ,...n ]
  [ <MIRROR TO clause> ] [ next-mirror-to ]
  [ WITH { DIFFERENTIAL
           | <general_WITH_options> [ ,...n ] } ]
[;]

--Backing Up Specific Files or Filegroups
BACKUP DATABASE { database_name | @database_name_var }
 <file_or_filegroup> [ ,...n ]
  TO <backup_device> [ ,...n ]
  [ <MIRROR TO clause> ] [ next-mirror-to ]
  [ WITH { DIFFERENTIAL | <general_WITH_options> [ ,...n ] } ]
[;]

--Creating a Partial Backup
BACKUP DATABASE { database_name | @database_name_var }
 READ_WRITE_FILEGROUPS [ , <read_only_filegroup> [ ,...n ] ]
  TO <backup_device> [ ,...n ]
  [ <MIRROR TO clause> ] [ next-mirror-to ]
  [ WITH { DIFFERENTIAL | <general_WITH_options> [ ,...n ] } ]
[;]

--Backing Up the Transaction Log (full and bulk-logged recovery models)
BACKUP LOG
  { database_name | @database_name_var }
  TO <backup_device> [ ,...n ]
  [ <MIRROR TO clause> ] [ next-mirror-to ]
  [ WITH { <general_WITH_options> | \<log-specific_optionspec> } [ ,...n ] ]
[;]
  
<backup_device>::=
 {
  { logical_device_name | @logical_device_name_var }
 | {   DISK
     | TAPE
     | URL } =
     { 'physical_device_name' | @physical_device_name_var | 'NUL' }
 }

<MIRROR TO clause>::=
 MIRROR TO <backup_device> [ ,...n ]

<file_or_filegroup>::=
 {
   FILE = { logical_file_name | @logical_file_name_var }
 | FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }
 }

<read_only_filegroup>::=
FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }

<general_WITH_options> [ ,...n ]::=
--Backup Set Options
   COPY_ONLY
 | { COMPRESSION | NO_COMPRESSION }
 | DESCRIPTION = { 'text' | @text_variable }
 | NAME = { backup_set_name | @backup_set_name_var }
 | CREDENTIAL
 | ENCRYPTION
 | FILE_SNAPSHOT
 | { EXPIREDATE = { 'date' | @date_var }
        | RETAINDAYS = { days | @days_var } }

--Media Set Options
   { NOINIT | INIT }
 | { NOSKIP | SKIP }
 | { NOFORMAT | FORMAT }
 | MEDIADESCRIPTION = { 'text' | @text_variable }
 | MEDIANAME = { media_name | @media_name_variable }
 | BLOCKSIZE = { blocksize | @blocksize_variable }

--Data Transfer Options
   BUFFERCOUNT = { buffercount | @buffercount_variable }
 | MAXTRANSFERSIZE = { maxtransfersize | @maxtransfersize_variable }

--Error Management Options
   { NO_CHECKSUM | CHECKSUM }
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }

--Compatibility Options
   RESTART

--Monitoring Options
   STATS [ = percentage ]

--Tape Options
   { REWIND | NOREWIND }
 | { UNLOAD | NOUNLOAD }

--Log-specific Options
   { NORECOVERY | STANDBY = undo_file_name }
 | NO_TRUNCATE

--Encryption Options
 ENCRYPTION (ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY } , encryptor_options ) <encryptor_options> ::=
   `SERVER CERTIFICATE` = Encryptor_Name | SERVER ASYMMETRIC KEY = Encryptor_Name
```

## <a name="arguments"></a>Argumente

DATABASE Gibt eine vollständige Datenbanksicherung an. Wird eine Liste von Dateien und Dateigruppen angegeben, werden nur diese Dateien und Dateigruppen gesichert. Während einer vollständigen oder differenziellen Datenbanksicherung werden von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auch die erforderlichen Teile des Transaktionsprotokolls gesichert, um eine konsistente Datenbank zu erzeugen, wenn die Sicherung wiederhergestellt wird.

Wenn Sie eine von BACKUP DATABASE (eine *Datensicherung*) erstellte Sicherung wiederherstellen, wird die komplette Sicherung wiederhergestellt. Nur Protokollsicherungen können bis zu einem bestimmten Zeitpunkt oder bis zu einer bestimmten Transaktion in der Sicherung wiederhergestellt werden.

> [!NOTE]
> Für die **Masterdatenbank** kann nur eine vollständige Datenbanksicherung ausgeführt werden.

PROTOKOLL

Gibt an, dass nur das Transaktionsprotokoll gesichert wird. Das Protokoll wird von der letzten erfolgreich ausgeführten Protokollsicherung bis zum aktuellen Ende des Protokolls gesichert. Bevor Sie die erste Protokollsicherung erstellen können, müssen Sie eine vollständige Sicherung erstellen.

Eine Protokollsicherung können Sie auf eine bestimmte Zeit oder Transaktion in der Sicherung wiederherstellen, indem Sie `WITH STOPAT`, `STOPATMARK` oder `STOPBEFOREMARK` in Ihrer [RESTORE LOG](../../t-sql/statements/restore-statements-transact-sql.md)-Anweisung angeben.

> [!NOTE]
> Nach einer normalen Protokollsicherung sind einige Transaktionsprotokolldatensätze inaktiv, sofern Sie nicht `WITH NO_TRUNCATE` oder `COPY_ONLY` angeben. Das Protokoll wird abgeschnitten, nachdem alle Datensätze innerhalb mindestens einer virtuellen Protokolldatei deaktiviert werden. Wenn das Protokoll nach routinemäßigen Protokollsicherungen nicht abgeschnitten wird, wird das Abschneiden des Protokolls möglicherweise verzögert. Weitere Informationen finden Sie in [Faktoren, die die Protokollkürzung verzögern können](../../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation).

{ _database\_name_ |  **@** _database\_name\_var_ } Die Datenbank, aus der das Transaktionsprotokoll, die Teildatenbank oder die vollständige Datenbank gesichert wird. Bei Angabe in Form einer Variablen ( **@** _database\_name\_var_) kann dieser Name entweder als Zeichenfolgenkonstante ( **@** _database\_name\_var_ **=** _database name_) oder als Variable eines Zeichenfolgen-Datentyps (mit Ausnahme der Datentypen **ntext** oder **text**) angegeben werden.

> [!NOTE]
> Eine Sicherung der Spiegeldatenbank in einer Datenbank-Spiegelungspartnerschaft ist nicht möglich.

\<file_or_filegroup> [ **,** ...*n* ] wird nur mit BACKUP DATABASE verwendet, gibt eine Datenbankdatei oder eine Dateigruppe in einer Datenbank an, die in einer Dateisicherung enthalten sein soll, oder gibt eine schreibgeschützte Datei oder Dateigruppe an, die in einer Teilsicherung enthalten sein soll.

FILE **=** { *logical_file_name* | **@** _logical\_file\_name\_var_ } Der logische Name einer Datei oder einer Variablen, deren Wert dem logischen Namen einer Datei entspricht, die in der Sicherung enthalten sein soll.

FILEGROUP **=** { _logical\_filegroup\_name_ |  **@** _logical\_filegroup\_name\_var_ } Der logische Name einer Dateigruppe oder einer Variablen, deren Wert dem logischen Namen einer Dateigruppe entspricht, die in der Sicherung enthalten sein soll. Beim einfachen Wiederherstellungsmodell wird die Dateigruppensicherung nur für eine schreibgeschützte Dateigruppe unterstützt.

> [!NOTE]
> Verwenden Sie Dateisicherungen dann, wenn eine Datenbanksicherung aufgrund der Datenbankgröße und aufgrund von Leistungsanforderungen nicht möglich ist. Das NUL-Gerät kann zum Testen der Leistung von Sicherungen verwendet werden, sollte aber nicht in Produktionsumgebungen eingesetzt werden.

*n* Ein Platzhalter, der anzeigt, dass mehrere Dateien und Dateigruppen in einer durch Trennzeichen getrennten Liste angegeben werden können. Für die Anzahl gibt es keine Einschränkungen.

Weitere Informationen finden Sie unter [Vollständige Dateisicherungen](../../relational-databases/backup-restore/full-file-backups-sql-server.md) und [Sichern von Dateien und Dateigruppen](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md).

READ_WRITE_FILEGROUPS [ **,** FILEGROUP = { _logical\_filegroup\_name_ |  **@** _logical\_filegroup\_name\_var_ } [ **,** ..._n_ ] ] Gibt eine Teilsicherung an. Eine Teilsicherung umfasst alle Dateien mit Lese-/Schreibzugriff in einer Datenbank: die primäre Dateigruppe und alle beliebigen sekundären Dateigruppen mit Lese-/Schreibzugriff sowie auch alle angegebenen schreibgeschützten Dateien oder Dateigruppen.

READ_WRITE_FILEGROUPS Gibt an, dass alle Dateigruppen mit Lese-/Schreibzugriff in der Teilsicherung gesichert werden sollen. Wenn die Datenbank schreibgeschützt ist, umfasst READ_WRITE_FILEGROUPS nur die primäre Dateigruppe.

> [!IMPORTANT]
> Durch das explizite Auflisten der Dateigruppen mit Lese-/Schreibzugriff mithilfe von FILEGROUP anstelle von READ_WRITE_FILEGROUPS wird eine Dateisicherung erstellt.

FILEGROUP = { *logical_filegroup_name* |  **@** _logical\_filegroup\_name\_var_ } Der logische Name einer schreibgeschützten Dateigruppe oder einer Variablen, deren Wert dem logischen Namen einer schreibgeschützten Dateigruppe entspricht, die in der Teilsicherung enthalten sein soll. Weitere Informationen finden Sie weiter oben in diesem Artikel unter „\<file_or_filegroup>“.

*n* Ein Platzhalter, der anzeigt, dass mehrere schreibgeschützte Dateigruppen in einer durch Trennzeichen getrennten Liste angegeben werden können.

Weitere Informationen zu Sicherungen des Protokollfragments finden Sie unter [Teilsicherungen](../../relational-databases/backup-restore/partial-backups-sql-server.md).

TO \<backup_device> [ **,** ...*n* ] gibt an, dass es sich bei den zugehörigen [Sicherungsmedien](../../relational-databases/backup-restore/backup-devices-sql-server.md) entweder um einen nicht gespiegelten Mediensatz oder um den ersten Spiegel innerhalb eines gespiegelten Mediensatzes handelt (für den eine oder mehrere MIRROR TO-Klauseln deklariert sind).

\<backup_device>

Gibt ein logisches oder physisches Sicherungsmedium an, das für den Sicherungsvorgang verwendet werden soll.

{*logical_device_name* \| **@** _logical\_device\_name\_var_} **Gilt für:** SQL Server: Der logische Name des Sicherungsmediums, auf dem die Datenbank gesichert wird. Der logische Name muss den Regeln für Bezeichner entsprechen. Bei Angabe in Form einer Variablen (@*logical_device_name_var*) kann der Name des Sicherungsmediums entweder als Zeichenfolgenkonstante (@_logical\_device\_name\_var_ **=** logical backup device name) oder als Variable eines Zeichenfolgen-Datentyps (mit Ausnahme der Datentypen **ntext** oder **text**) angegeben werden.

{DISK \| TAPE \| URL} **=** { **'** _physical\_device\_name_ **'** \| **@** _physical\_device\_name\_var_ \| 'NUL'} **Gilt für:** DISK, TAPE und URL gelten für SQL Server.
Gibt eine Datenträgerdatei oder ein Bandmedium oder einen Microsoft Azure-BLOB-Speicherdienst an. Das URL-Format wird zum Erstellen von Sicherungen in Microsoft Azure Storage verwendet. Weitere Informationen finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure Blob Storage Service](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md). Ein Tutorial finden Sie unter [Tutorial: SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure Blob Storage-Dienst](~/relational-databases/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service.md).

> [!NOTE]
> Das Datenträgermedium NUL verwirft alle Informationen, die es empfängt, und sollte nur zu Testzwecken verwendet werden. Es ist nicht zur Verwendung in der Produktion bestimmt.
> [!IMPORTANT]
> Ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 bis [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] können Sie nur auf ein einzelnes Gerät sichern, wenn Sie über die URL sichern. Wenn Sie eine Sicherung auf mehreren Geräten durchführen möchten, müssen Sie [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher verwenden, und Sie müssen SAS-Token (Shared Access Signature) verwenden. Beispiele für die Erstellung einer Shared Access Signature finden Sie unter [SQL Server-Sicherung über URLs](../../relational-databases/backup-restore/sql-server-backup-to-url.md) und [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell) (Vereinfachen der Erstellung von SQL-Anmeldeinformationen mit Shared Access Signature-Token in Azure Storage mit PowerShell).

**URL betrifft:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 und höher).

Das Datenträgermedium muss erst dann vorhanden sein, wenn es in einer BACKUP-Anweisung angegeben wird. Wenn das physische Medium vorhanden ist und die Option INIT in der BACKUP-Anweisung nicht angegeben ist, wird die Sicherung an das Medium angefügt.

> [!NOTE]
> Das NUL-Gerät verwirft alle Eingaben, die an diese Datei gesendet werden, aber die Sicherung kennzeichnet noch immer alle Seiten als gesichert.

Weitere Informationen finden Sie unter [Sicherungsmedien](../../relational-databases/backup-restore/backup-devices-sql-server.md).

> [!NOTE]
> Die Option TAPE wird in zukünftigen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht mehr bereitgestellt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden.

*n* Ein Platzhalter, der anzeigt, dass in einer durch Trennzeichen getrennten Liste möglicherweise bis zu 64 Sicherungsmedien angegeben werden.

MIRROR TO \<backup_device> [ **,** ...*n* ] gibt bis zu drei sekundäre Sicherungsmedien an, die die in der TO-Klausel angegebenen Sicherungsmedien spiegeln. Die MIRROR TO-Klausel muss denselben Typ und dieselbe Anzahl von Sicherungsdateien angeben wie die TO-Klausel. Die maximale Anzahl von MIRROR TO-Klauseln lautet drei.

Diese Option ist nur in der Enterprise Edition von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verfügbar.

> [!NOTE]
> Für MIRROR TO = DISK legt BACKUP automatisch die geeignete Blockgröße für Datenträgermedien auf Basis der Sektorgröße des Datenträgers fest. Wenn der MIRROR TO-Datenträger mit einer anderen Sektorgröße als der als primäres Sicherungsmedium angegebener Datenträger formatiert ist, schlägt der backup-Befehl fehl. Um Sicherungen auf Geräten zu spiegeln, die verschiedene Sektorgrößen aufweisen, muss der BLOCKSIZE-Parameter angegeben werden und sollte auf der höchsten Sektorgröße für die Zielgeräte festgelegt werden. Weitere Informationen zur Blockgröße finden Sie weiter unten in diesem Thema unter „BLOCKSIZE“.

\<backup_device> Weiter Informationen finden Sie oben in diesem Abschnitt unter „\<backup_device>“.

*n* Ein Platzhalter, der anzeigt, dass in einer durch Trennzeichen getrennten Liste möglicherweise bis zu 64 Sicherungsmedien angegeben werden. Die Anzahl von Medien in der MIRROR TO-Klausel muss der Anzahl von Medien in der TO-Klausel entsprechen.

Weitere Informationen finden Sie unter "Medienfamilien in gespiegelten Mediensätzen" im Abschnitt [Hinweise](#general-remarks) weiter unten in diesem Thema.

[ *next-mirror-to* ] Ein Platzhalter, der anzeigt, dass eine einzelne BACKUP-Anweisung zusätzlich zu der einzelnen TO-Klausel bis zu drei MIRROR TO-Klauseln enthalten kann.

### <a name="with-options"></a>WITH-Optionen

Gibt die Optionen an, die bei einem Sicherungsvorgang verwendet werden sollen.

CREDENTIAL **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 und höher).
Wird nur verwendet, wenn eine Sicherung in Microsoft Azure Blob Storage erstellt wird.

FILE_SNAPSHOT **Gilt für:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher)

Wird zum Erstellen einer Azure-Momentaufnahme der Datenbankdateien verwendet, wenn alle SQL Server-Datenbankdateien unter Verwendung des Azure-Blob-Speicherdiensts gespeichert werden. Weitere Informationen finden Sie unter [SQL Server-Datendateien in Microsoft Azure](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Die Momentaufnahmesicherung erstellt Azure-Momentaufnahmen der Datenbankdateien (Daten- und Protokolldateien) in einem konsistenten Zustand. Ein konsistenter Satz von Azure-Momentaufnahmen bildet zusammen eine Sicherung und wird in der Sicherungsdatei aufgezeichnet. Der einzige Unterschied zwischen `BACKUP DATABASE TO URL WITH FILE_SNAPSHOT` und `BACKUP LOG TO URL WITH FILE_SNAPSHOT` ist, dass der zweite Wert auch das Transaktionsprotokoll abschneidet, während dies beim ersten Wert nicht der Fall ist. Bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Momentaufnahmesicherung ist nur eine Transaktionsprotokollsicherung erforderlich, um eine Datenbank auf den Zeitpunkt der Transaktionsprotokollsicherung wiederherzustellen, nachdem die erste vollständige, von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für die Einrichtung der Sicherungskette vorausgesetzte Sicherung durchgeführt wurde. Darüber hinaus sind nur zwei Sicherungen des Transaktionsprotokolls erforderlich, um eine Datenbank auf einen Zeitpunkt zwischen den beiden Transaktionsprotokollsicherungen wiederherstellen.

DIFFERENTIAL

Gibt bei der Verwendung nur mit BACKUP DATABASE an, dass sich die Datenbank- oder Dateisicherung nur auf die Teile der Datenbank oder Datei beschränken soll, die seit der letzten vollständigen Sicherung geändert wurden. Eine differenzielle Sicherung benötigt in der Regel weniger Speicherplatz als eine vollständige Sicherung. Verwenden Sie diese Option, damit nicht alle seit der letzten vollständigen Sicherung ausgeführten individuellen Protokollsicherungen angewendet werden müssen.

> [!NOTE]
> Standardmäßig erstellt `BACKUP DATABASE` eine vollständige Sicherung.

Weitere Informationen finden Sie unter [Differenzielle Sicherungen](../../relational-databases/backup-restore/differential-backups-sql-server.md).

ENCRYPTION Wird verwendet, um die Verschlüsselung für eine Sicherung anzugeben. Sie können einen Verschlüsselungsalgorithmus angeben, um die Sicherung zu verschlüsseln, oder `NO_ENCRYPTION` angeben, um die Sicherung nicht zu verschlüsseln. Die Verschlüsselung wird empfohlen, um Sicherungsdateien zu sichern. Die Liste der Algorithmen, die Sie angeben können:

- `AES_128`
- `AES_192`
- `AES_256`
- `TRIPLE_DES_3KEY`
- `NO_ENCRYPTION`

Wenn Sie sich für die Verschlüsselung entscheiden, müssen Sie auch die Verschlüsselung mithilfe der Verschlüsselungsoptionen festlegen:

- `SERVER CERTIFICATE` = Encryptor_Name
- `SERVER ASYMMETRIC KEY` = Encryptor_Name

Bei `SERVER CERTIFICATE` und `SERVER ASYMMETRIC KEY` handelt es sich um ein Zertifikat und einen asymmetrischen Schlüssel, die in der `master`-Datenbank erstellt wurden. Weitere Informationen finden Sie jeweils unter [`CREATE CERTIFICATE`](../../t-sql/statements/create-certificate-transact-sql.md) und [`CREATE ASYMMETRIC KEY`](../../t-sql/statements/create-asymmetric-key-transact-sql.md).

> [!WARNING]
> Bei Verwendung der Verschlüsselung in Verbindung mit dem `FILE_SNAPSHOT`-Argument wird die Metadatendatei selbst mithilfe des angegebenen Verschlüsselungsalgorithmus verschlüsselt, und das System überprüft, ob [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) für die Datenbank durchgeführt wurde. Für die Daten selbst erfolgt keine zusätzliche Verschlüsselung. Die Sicherung schlägt fehl, wenn die Datenbank nicht verschlüsselt oder die Verschlüsselung nicht abgeschlossen wurde, bevor die Backup-Anweisung ausgegeben wurde.

**Sicherungssatzoptionen**

Diese Optionen werden für den durch diesen Sicherungsvorgang erstellten Sicherungssatz verwendet.

> [!NOTE]
> Verwenden Sie die Option `FILE = <backup_set_file_number>`, um einen Sicherungssatz für einen Wiederherstellungsvorgang anzugeben. Weitere Informationen zum Angeben eines Sicherungssatzes finden Sie in [RESTORE-Argumente](../../t-sql/statements/restore-statements-arguments-transact-sql.md) unter „Angeben eines Sicherungssatzes“.

COPY_ONLY gibt an, dass die Sicherung eine *Kopiesicherung* ist, die keine Auswirkungen auf die normale Sicherungssequenz hat. Eine Kopiesicherung wird unabhängig von den regelmäßig geplanten konventionellen Sicherungen erstellt. Eine Kopiesicherung hat keine Auswirkungen auf die allgemeinen Sicherungs- und Wiederherstellungsprozeduren für die Datenbank.

Kopiesicherungen sollten in Situationen verwendet werden, in denen eine Sicherung für einen besonderen Zweck verwendet wird, z. B. zum Sichern des Protokolls vor einer Onlinedateiwiederherstellung. In der Regel wird eine Kopieprotokollsicherung einmal verwendet und dann gelöscht.

- Bei Verwendung mit `BACKUP DATABASE`BACKUP DATABASE wird durch die Option `COPY_ONLY` eine vollständige Sicherung erstellt, die nicht als eine differenzielle Basis verwendet werden kann. Das differenzielle Bitmuster wird nicht aktualisiert, und die differenziellen Sicherungen verhalten sich so, als sei die Kopiesicherung nicht vorhanden. Nachfolgende differenzielle Sicherungen verwenden die neueste, konventionelle vollständige Sicherung als Basis.

    > [!IMPORTANT]
    > Wenn `DIFFERENTIAL` und `COPY_ONLY` zusammen verwendet werden, wird `COPY_ONLY` ignoriert, und eine differenzielle Sicherung wird erstellt.

- Bei Verwendung mit `BACKUP LOG` wird durch die Option `COPY_ONLY` eine *Kopieprotokollsicherung* erstellt, die das Transaktionsprotokoll nicht abschneidet. Die Kopieprotokollsicherung hat keine Auswirkungen auf die Protokollkette, und andere Protokollsicherungen verhalten sich so, als sei die Kopiesicherung nicht vorhanden.

Weitere Informationen finden Sie unter [Kopiesicherungen](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).

{ COMPRESSION | NO_COMPRESSION } Nur in [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] und höheren Versionen verfügbar. Gibt an, ob bei dieser Sicherung die [Sicherungskomprimierung](../../relational-databases/backup-restore/backup-compression-sql-server.md) eingesetzt wird, wodurch die Standardeinstellung auf Serverebene überschrieben wird.

Die Sicherungskomprimierung wird bei der Installation standardmäßig deaktiviert. Diese Standardeinstellung kann jedoch durch Festlegen der Serverkonfigurationsoption [backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md) geändert werden. Informationen zum Anzeigen des aktuellen Werts dieser Option finden Sie unter [Anzeigen oder Ändern von Servereigenschaften](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md).

Informationen zur Verwendung der Sicherungskomprimierung mit [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md)-fähigen Datenbanken finden Sie im Abschnitt [Hinweise](#general-remarks).

COMPRESSION Aktiviert die Sicherungskomprimierung explizit.

NO_COMPRESSION Deaktiviert die Sicherungskomprimierung explizit.

DESCRIPTION **=** { **'** _text_ **'**  |  **@** _text\_variable_ } Gibt den freien Text an, der als Beschreibung des Sicherungssatzes verwendet wird. Die Zeichenfolge kann maximal 255 Zeichen haben.

NAME **=** { *backup_set_name* |  **@** _backup\_set\_var_ } Gibt den Namen des Sicherungssatzes an. Namen können maximal 128 Zeichen haben. Wird NAME nicht angegeben, erhält der Sicherungssatz einen leeren Namen.

{ EXPIREDATE **='** _date_ **'** | RETAINDAYS **=** _days_ } Gibt an, wann der Sicherungssatz für diese Sicherung überschrieben werden kann. Wenn beide Optionen verwendet werden, hat RETAINDAYS Vorrang vor EXPIREDATE.

Wenn keine der Optionen angegeben wird, wird das Ablaufdatum durch die Konfigurationseinstellung **media retention** bestimmt. Weitere Informationen finden Sie unter [Serverkonfigurationsoptionen](../../database-engine/configure-windows/server-configuration-options-sql-server.md).

> [!IMPORTANT]
> Diese Optionen verhindern nur, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine Datei überschreibt. Bänder können mit anderen Methoden gelöscht werden, und Dateien auf einem Datenträger können mit entsprechenden Betriebssystembefehlen gelöscht werden. Weitere Informationen zur Prüfung des Ablaufdatums finden Sie unter SKIP und FORMAT in diesem Thema.

EXPIREDATE **=** { **'** _date_ **'**  |  **@** _date\_var_ } Gibt an, wann der Sicherungssatz abläuft und überschrieben werden kann. Bei Angabe in Form einer Variablen (@_date\_var_) muss das Datum dem konfigurierten **datetime**-Systemformat entsprechen und als eines der folgenden Elemente angegeben werden:

- Eine Zeichenfolgekonstante (@_date\_var_ **=** date)
- Eine Variable mit einem Zeichenfolgen-Datentyp (mit Ausnahme der Datentypen **ntext** oder **text**)
- Eine **smalldatetime**-Variable
- Eine **datetime**-Variable

Beispiel:

- `'Dec 31, 2020 11:59 PM'`
- `'1/1/2021'`

Informationen zum Angeben der **datetime**-Werte finden Sie unter [Datums- und Uhrzeittypen](../../t-sql/data-types/date-and-time-types.md).

> [!NOTE]
> Zum Ignorieren des Ablaufdatums verwenden Sie die Option `SKIP`.

RETAINDAYS **=** { *days* |  **@** _days\_var_ } Gibt die Anzahl von Tagen an, die verstreichen müssen, bevor diese Sicherungsmedien überschrieben werden können. Bei Angabe als Variable ( **@** _days\_var_) muss der Wert als Integer angegeben werden.

**Mediensatzoptionen**

Diese Optionen werden für den Mediensatz insgesamt verwendet.

{ **NOINIT** | INIT } Steuert, ob der Sicherungsvorgang an die vorhandenen Sicherungssätze auf dem Sicherungsmedium angefügt wird oder diese überschreibt. Er wird standardmäßig an den neuesten Sicherungssatz auf dem Medium (NOINIT) angefügt.

> [!NOTE]
> Informationen zu Interaktionen zwischen { **NOINIT** | INIT } und { **NOSKIP** | SKIP } finden Sie unter [Hinweise](#general-remarks) weiter unten in diesem Thema.

NOINIT Zeigt an, dass der Sicherungssatz an den angegebenen Mediensatz angehängt wird, wobei vorhandene Sicherungssätze erhalten bleiben. Wenn für den Mediensatz ein Medienkennwort definiert ist, muss dieses Kennwort angegeben werden. NOINIT ist die Standardeinstellung.

Weitere Informationen finden Sie weiter unten in diesem Thema unter [Mediensätze, Medienfamilien und Sicherungssätze](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).

INIT Gibt an, dass alle Sicherungssätze überschrieben werden sollen, während der Medienheader erhalten bleibt. Wenn INIT angegeben wird, werden alle bereits auf dem Medium vorhandenen Sicherungssätze überschrieben (wenn die Bedingungen dies zulassen). Standardmäßig prüft BACKUP die folgenden Bedingungen und überschreibt die Sicherungsmedien nicht, wenn eine der Bedingungen erfüllt wird:

- Einer der Sicherungssätze ist noch nicht abgelaufen. Weitere Informationen finden Sie unter `EXPIREDATE` und `RETAINDAYS`.
- Der möglicherweise in der BACKUP-Anweisung angegebene Name des Sicherungssatzes stimmt nicht mit dem Namen auf dem Sicherungsmedium überein. Weitere Informationen finden Sie unter der Option NAME weiter oben in diesem Abschnitt.

Verwenden Sie die Option `SKIP`, um diese Überprüfungen zu überschreiben.

Weitere Informationen finden Sie weiter unten in diesem Thema unter [Mediensätze, Medienfamilien und Sicherungssätze](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).

{ **NOSKIP** | SKIP } Steuert, ob ein Sicherungsvorgang das Ablaufdatum und die Ablaufzeit der Sicherungssätze auf dem Medium überprüft, bevor diese überschrieben werden.

> [!NOTE]
> Informationen zu Interaktionen zwischen { **NOINIT** | INIT } und { **NOSKIP** | SKIP } finden Sie unter "Hinweise" weiter unten in diesem Thema.

NOSKIP Weist die BACKUP-Anweisung an, das Ablaufdatum aller Sicherungssätze auf den Medien zu prüfen, bevor diese überschrieben werden dürfen. Dies ist das Standardverhalten.

SKIP Deaktiviert die Prüfung von Ablaufdatum und Namen der Sicherungssätze. Diese Überprüfung wird normalerweise von der BACKUP-Anweisung ausgeführt, damit ein Überschreiben von Sicherungssätzen verhindert wird. Informationen zu Interaktionen zwischen { INIT | NOINIT } und { NOSKIP | SKIP } finden Sie unter "Hinweise" weiter unten in diesem Thema.
Zum Anzeigen des Ablaufdatums und der Sicherungssätze fragen Sie die **expiration_date**-Spalte der [backupset](../../relational-databases/system-tables/backupset-transact-sql.md)-Verlaufstabelle ab.

{ **NOFORMAT** | FORMAT } Gibt an, dass der Medienheader auf alle Volumes geschrieben werden soll, die für diesen Sicherungsvorgang verwendet werden. Dabei werden vorhandene Medienheader und Sicherungssätze überschrieben.

NOFORMAT Gibt an, dass der Sicherungsvorgang die vorhandenen Medienheader und Sicherungssätze auf den Medienvolumes beibehält, die für diesen Vorgang verwendet werden. Dies ist das Standardverhalten.

FORMAT Gibt an, dass ein neuer Mediensatz erstellt werden kann. FORMAT bewirkt, dass vom Sicherungsvorgang ein neuer Medienheader auf alle Medienvolumes geschrieben wird, die für den Sicherungsvorgang verwendet werden. Der vorhandene Inhalt des Volumes wird ungültig, da alle vorhandenen Medienheader und Sicherungssätze überschrieben werden.

> [!IMPORTANT]
> Verwenden Sie `FORMAT` mit Bedacht. Die Formatierung von Volumes eines Mediensatzes führt dazu, dass der gesamte Mediensatz nicht mehr verwendet werden kann. Wird beispielsweise ein einzelnes Band initialisiert, das zu vorhandenen Stripesetmedien gehört, führt dies dazu, dass der gesamte Mediensatz unbrauchbar wird.

Durch die Angabe von FORMAT ist `SKIP` impliziert. `SKIP` muss nicht explizit angegeben werden.

MEDIADESCRIPTION **=** { *text* | **@** _text\_variable_ } Gibt die freie Textbeschreibung des Mediensatzes an, die aus maximal 255 Zeichen bestehen kann.

MEDIANAME **=** { *media_name* |  **@** _media\_name\_variable_ } Gibt den Mediennamen für den gesamten Sicherungsmediensatz an. Der Medienname darf nicht mehr als 128 Zeichen umfassen. Wird `MEDIANAME` angegeben, muss dieser Name dem vorher angegebenen Mediennamen auf den Sicherungsvolumes entsprechen. Wird er nicht angegeben, oder ist die Option SKIP festgelegt, findet keine Prüfung des Mediennamens statt.

BLOCKSIZE **=** { *blocksize* |  **@** _blocksize\_variable_ } Legt die physische Blockgröße in Bytes fest. Die unterstützten Größen sind 512, 1024, 2048, 4096, 8192, 16.384, 32.768 und 65.536 (64 KB) Bytes. Der Standardwert ist 65.536 für Bandmedien und andernfalls 512. In der Regel ist diese Option nicht erforderlich, da von BACKUP automatisch eine Blockgröße ausgewählt wird, die für das Medium geeignet ist. Mit der expliziten Angabe einer Blockgröße wird die automatische Wahl der Blockgröße überschrieben.

Geben Sie beim Erstellen einer Sicherung, die Sie auf eine CD-ROM kopieren und von dieser wiederherstellen möchten, BLOCKSIZE=2048 an.

> [!NOTE]
> Diese Option hat i. d. R. nur dann Auswirkungen auf die Leistung, wenn auf Bandmedien geschrieben wird.

**Datenübertragungsoptionen**

BUFFERCOUNT **=** { *buffercount* |  **@** _buffercount\_variable_ } Gibt die Gesamtanzahl von E/A-Puffern an, die für den Sicherungsvorgang verwendet werden sollen. Sie können eine beliebige positive ganze Zahl angeben. Eine große Pufferanzahl kann jedoch wegen eines ungeeigneten virtuellen Adressraumes im Prozess Sqlservr.exe zu Fehlern aufgrund von nicht genügend Arbeitsspeicher führen.

Der gesamte von den Puffern belegte Speicherplatz wird durch `BUFFERCOUNT * MAXTRANSFERSIZE` bestimmt.

> [!NOTE]
> Wichtige Informationen zur Verwendung der `BUFFERCOUNT`-Option finden Sie im Blogeintrag [Falsche BufferCount-Datenübertragungsoption kann OOM-Bedingung auslösen](/archive/blogs/sqlserverfaq/incorrect-buffercount-data-transfer-option-can-lead-to-oom-condition).

MAXTRANSFERSIZE **=** { *maxtransfersize* | _**@** maxtransfersize\_variable_ } Gibt die größte zu verwendende Einheit für Übertragungen zwischen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und dem Sicherungsmedium in Bytes an. Die möglichen Werte sind Vielfache von 65.536 Bytes (64 KB) bis hin zu 4.194.304 Bytes (4 MB).

> [!NOTE]
> Wenn beim Erstellen von Sicherungen mithilfe des SQL Writer-Diensts für die Datenbank [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md) konfiguriert ist oder [speicheroptimierte Dateigruppen](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md) enthält, dann sollte `MAXTRANSFERSIZE` zum Zeitpunkt der Wiederherstellung größer als oder gleich der `MAXTRANSFERSIZE` sein, die beim Erstellen der Sicherung verwendet wurde.

> [!NOTE]
> Für [Transparente Datenverschlüsselung (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md)-fähige Datenbanken mit einer einzelnen Datendatei ist der standardmäßige `MAXTRANSFERSIZE`-Wert 65536 (64 KB). Für nicht mit TDE verschlüsselte Datenbanken ist der `MAXTRANSFERSIZE`-Standardwert 1048576 (1 MB) bei Verwendung der Sicherung auf DISK und 65536 (64 KB) bei Verwendung von VDI oder TAPE.
> Weitere Informationen zur Verwendung der Sicherungskomprimierung mit TDE-verschlüsselten Datenbanken finden Sie im Abschnitt [Hinweise](#general-remarks).

**Fehlerverwaltungsoptionen**

Mit diesen Optionen können Sie bestimmen, ob Sicherungsprüfsummen für den Sicherungsvorgang aktiviert werden und ob der Vorgang beim Auftreten eines Fehlers beendet wird.

{ **NO_CHECKSUM** | CHECKSUM } Steuert, ob Sicherungsprüfsummen aktiviert sind.

NO_CHECKSUM Das Generieren von Sicherungsprüfsummen (und die Überprüfung von Seitenprüfsummen) wird explizit deaktiviert. Dies ist das Standardverhalten.

CHECKSUM Gibt an, dass der Sicherungsvorgang jede Seite auf Prüfsumme und zerrissene Seiten überprüft (wenn aktiviert und verfügbar) und eine Prüfsumme für die gesamte Sicherung generiert.

Die Verwendung von Sicherungsprüfsummen kann sich auf die Arbeitsauslastung und den Durchsatz bei der Sicherung auswirken.

Weitere Informationen finden Sie unter [Mögliche Medienfehler während der Sicherung und Wiederherstellung](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).

{ **STOP_ON_ERROR** | CONTINUE_AFTER_ERROR } Steuert, ob ein Sicherungsvorgang beendet oder fortgesetzt wird, nachdem ein Fehler bei der Seitenprüfsumme aufgetreten ist.

STOP_ON_ERROR Von BACKUP wird ein Fehler erzeugt, wenn eine Seitenprüfsumme nicht stimmt. Dies ist das Standardverhalten.

CONTINUE_AFTER_ERROR BACKUP wird auch dann fortgesetzt, wenn Fehler auftreten, z.B. ungültige Prüfsummen oder zerrissene Seiten.

Wenn Sie das Protokollfragment mithilfe der Option NO_TRUNCATE nicht sichern können, wenn die Datenbank beschädigt ist, können Sie versuchen, eine [Sicherung des Protokollfragments](../../relational-databases/backup-restore/tail-log-backups-sql-server.md) auszuführen, indem Sie CONTINUE_AFTER_ERROR anstelle von NO_TRUNCATE angeben.

Weitere Informationen finden Sie unter [Mögliche Medienfehler während der Sicherung und Wiederherstellung](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).

**Kompatibilitätsoptionen**

RESTART Hat ab [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] keine Auswirkungen. Die Option wird von der Version aus Gründen der Kompatibilität mit früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angenommen.

**Überwachungsoptionen**

STATS [ **=** _percentage_ ] Zeigt nach jedem abgeschlossenen *Prozentsatz* eine Meldung an, und wird als Statusanzeige verwendet. Wird *Prozentsatz* nicht angegeben, zeigt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] jedes Mal eine Meldung an, wenn weitere 10 Prozent des Vorgangs abgeschlossen sind.

Mit der Option STATS wird der Prozentsatz gemeldet, der beim Erreichen des Schwellenwertes für das nächste Meldungsintervall abgeschlossen ist. Bei dem angegebenen Prozentsatz handelt es sich um einen ungefähren Wert. Wird beispielsweise STATS=10 festgelegt und sind 40 Prozent des Vorgangs abgeschlossen, dann zeigt die Option u. U. 43 Prozent an. Bei größeren Sicherungssätzen stellt dies kein Problem dar, da sich der Wert für den abgeschlossenen Prozentsatz zwischen abgeschlossenen E/A-Aufrufen nur sehr langsam verändert.

**Bandoptionen**

Diese Optionen werden nur für Bandmedien verwendet. Diese Optionen werden ignoriert, wenn ein anderes Medium als ein Bandmedium verwendet wird.

{ **REWIND** | NOREWIND } REWIND

Gibt an, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das Band freigeben und zurückspulen soll. REWIND ist die Standardeinstellung.

NOREWIND

Gibt an, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das Band nach dem Sicherungsvorgang offen hält. Diese Option trägt zur Leistungsverbesserung bei, wenn mehrere Sicherungsvorgänge auf einem Band ausgeführt werden.

NOREWIND impliziert NOUNLOAD, und diese Optionen sind innerhalb einer BACKUP-Anweisung inkompatibel.

> [!NOTE]
> Wenn Sie `NOREWIND` verwenden, bleibt die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Besitzer des Bandlaufwerks, bis in einer BACKUP- oder RESTORE-Anweisung, die in demselben Prozess ausgeführt wird, die Option `REWIND` oder `UNLOAD` verwendet wird oder bis die Serverinstanz heruntergefahren wird. Ein Offenhalten des Bandes verhindert, dass andere Prozesse auf das Band zugreifen können. Informationen zum Anzeigen einer Liste offener Bänder und zum Schließen eines offenen Bands finden Sie unter [Sicherungsmedien](../../relational-databases/backup-restore/backup-devices-sql-server.md).

{ **UNLOAD** | NOUNLOAD }

> [!NOTE]
> `UNLOAD` und `NOUNLOAD` sind Sitzungseinstellungen, die für die Dauer der Sitzung oder bis zur Angabe der alternativen Einstellung persistent gespeichert wird.

UNLOAD

Gibt an, dass das Band automatisch zurückgespult und ausgeworfen wird, wenn die Sicherung vollständig ausgeführt ist. UNLOAD ist die Standardeinstellung beim Beginn einer Sitzung.

NOUNLOAD

Gibt an, dass das Band nach dem BACKUP-Vorgang im Bandlaufwerk geladen bleibt.

> [!NOTE]
> Beim Sichern auf einem Bandsicherungsmedium wirkt sich die Option `BLOCKSIZE` auf die Leistung des Sicherungsvorgangs aus. Diese Option hat i. d. R. nur dann Auswirkungen auf die Leistung, wenn auf Bandmedien geschrieben wird.

**Protokollspezifische Optionen**

Diese Optionen werden nur mit `BACKUP LOG` verwendet.

> [!NOTE]
> Wenn Sie keine Protokollsicherungen vornehmen möchten, verwenden Sie das einfache Wiederherstellungsmodell. Weitere Informationen finden Sie unter [Wiederherstellungsmodelle](../../relational-databases/backup-restore/recovery-models-sql-server.md).

{ NORECOVERY | STANDBY **=** _undo_file_name_ }

NORECOVERY

Sichert das Protokollfragment und belässt die Datenbank im RESTORING-Status. NORECOVERY ist hilfreich, wenn ein Failover zu einer sekundären Datenbank erfolgt oder wenn das Protokollfragment vor einem RESTORE-Vorgang gesichert wird.

Zum Ausführen einer Protokollsicherung, bei der die Protokollkürzung ausgelassen wird und die Datenbank automatisch den Status RESTORING erhält, verwenden Sie die Optionen `NO_TRUNCATE` und `NORECOVERY` zusammen.

STANDBY **=** _standby_file_name_

Sichert das Protokollfragment und belässt die Datenbank im schreibgeschützten Modus und im STANDBY-Status. Die STANDBY-Klausel schreibt Standbydaten (wobei ein Rollback durchgeführt wird, aber mit der Option weiterer Wiederherstellungen). Die Verwendung der Option STANDBY ist gleichbedeutend mit BACKUP LOG WITH NORECOVERY gefolgt von RESTORE WITH STANDBY.

Wenn der Standbymodus verwendet wird, ist eine Standbydatei erforderlich, die mit *standby_file_name* angegeben wird und deren Speicherort im Protokoll der Datenbank gespeichert wird. Ist die angegebene Datei bereits vorhanden, wird sie von [!INCLUDE[ssDE](../../includes/ssde-md.md)] überschrieben. Ist sie noch nicht vorhanden, wird sie von [!INCLUDE[ssDE](../../includes/ssde-md.md)] erstellt. Die Standbydatei wird Teil der Datenbank.

Diese Datei enthält die Änderungen aus dem Rollback, die rückgängig gemacht werden müssen, wenn zu einem späteren Zeitpunkt RESTORE LOG-Vorgänge angewendet werden sollen. Es muss ausreichend Speicherplatz für die Vergrößerung der Standbydatei vorhanden sein, damit diese Datei alle Seiten aus der Datenbank enthalten kann, die durch das Rollback für die Transaktionen ohne Commit geändert wurden.

NO_TRUNCATE

Gibt an, dass das Protokoll nicht abgeschnitten wird, und führt dazu, dass [!INCLUDE[ssDE](../../includes/ssde-md.md)] versucht, die Sicherung unabhängig vom Status der Datenbank durchzuführen. Daraus folgt, dass eine Sicherung, die mit `NO_TRUNCATE` erstellt wird, u.U. unvollständige Metadaten enthält. Diese Option ermöglicht es, das Protokoll auch dann zu sichern, wenn die Datenbank beschädigt ist.

Die NO_TRUNCATE-Option von BACKUP LOG ist gleichbedeutend mit der Angabe von COPY_ONLY und CONTINUE_AFTER_ERROR.

Ohne die Option `NO_TRUNCATE` muss sich die Datenbank im ONLINE-Status befinden. Wenn die Datenbank im SUSPENDED-Status ist, können Sie möglicherweise durch die Angabe von `NO_TRUNCATE` eine Sicherung erstellen. Wenn sich die Datenbank jedoch im Status OFFLINE oder EMERGENCY befindet, ist die Sicherung auch mit `NO_TRUNCATE` nicht zulässig. Weitere Informationen zu Datenbankstatus finden Sie unter [Datenbankstatus](../../relational-databases/databases/database-states.md).

## <a name="about-working-with-sql-server-backups"></a>Informationen zum Arbeiten mit SQL Server-Sicherungen

In diesem Abschnitt werden die folgenden grundlegenden Sicherungskonzepte erläutert:

[Sicherungstypen](#Backup_Types)
[Kürzung des Transaktionsprotokolls](#Tlog_Truncation)
[Formatieren von Sicherungsmedien](#Formatting_Media)
[Arbeiten mit Sicherungsmedien und Mediensätzen](#Backup_Devices_and_Media_Sets)
[Wiederherstellen von SQL Server-Sicherungen](#Restoring_Backups)

> [!NOTE]
> Eine Einführung in die Sicherung in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] finden Sie unter [Übersicht über Sicherungen](../../relational-databases/backup-restore/backup-overview-sql-server.md).

### <a name="backup-types"></a><a name="Backup_Types"></a> Sicherungstypen

Die unterstützten Sicherungstypen sind vom Wiederherstellungsmodell der Datenbank abhängig, und zwar wie folgt:

- Alle Wiederherstellungsmodelle unterstützen vollständige und differenzielle Sicherungen von Daten.

    |Umfang der Sicherung|Sicherungstypen|
    |---------------------|------------------|
    |Gesamte Datenbank|[Datenbanksicherungen](../../relational-databases/backup-restore/full-database-backups-sql-server.md) umfassen die gesamte Datenbank.<br /><br /> Optional kann jede Datenbanksicherung als Basis einer Serie von einer oder mehreren [differenziellen Datenbanksicherungen](../../relational-databases/backup-restore/differential-backups-sql-server.md) dienen.|
    |Teilweise Datenbanksicherung|[Teilsicherungen](../../relational-databases/backup-restore/partial-backups-sql-server.md) umfassen Dateigruppen mit Lese-/Schreibzugriff und möglicherweise mindestens eine schreibgeschützte Datei oder Dateigruppe.<br /><br /> Optional kann jede Teilsicherung als Basis einer Serie von einer oder mehreren [differenziellen Teilsicherungen](../../relational-databases/backup-restore/differential-backups-sql-server.md) dienen.|
    |Datei oder Dateigruppe|[Dateisicherungen](../../relational-databases/backup-restore/full-file-backups-sql-server.md) umfassen mindestens eine Datei oder Dateigruppe und sind nur für Datenbanken relevant, in denen mehrere Dateigruppen enthalten sind. Beim einfachen Wiederherstellungsmodell werden Dateisicherungen im Wesentlichen auf schreibgeschützte sekundäre Dateigruppen beschränkt.<br /> Optional kann jede Dateisicherung als Basis einer Serie von einer oder mehreren [differenziellen Dateisicherungen](../../relational-databases/backup-restore/differential-backups-sql-server.md) dienen.|

- Beim vollständigen oder massenprotokollierten Wiederherstellungsmodell enthalten die herkömmlichen Sicherungen auch sequentielle *Transaktionsprotokollsicherungen* (oder *Protokollsicherungen*), die erforderlich sind. Jede Protokollsicherung umfasst den Teil des Transaktionsprotokolls, der beim Erstellen der Sicherung aktiv war, und enthält alle Protokolldatensätze, die in einer vorherigen Protokollsicherung nicht gesichert wurden.

    Wenn Sie die Gefahr eines Datenverlusts minimieren und den damit verbundenen höheren Verwaltungsaufwand in Kauf nehmen möchten, sollten Sie häufige Protokollsicherungen planen. Wenn Sie zwischen vollständigen Sicherungen differenzielle Sicherungen planen, kann dies die Wiederherstellungszeit verkürzen, da Sie nach dem Wiederherstellen der Daten nur eine geringe Anzahl von Protokollsicherungen wiederherstellen müssen.

     Es empfiehlt sich, Protokollsicherungen auf einem anderen Volume als die Datenbanksicherungen zu speichern.

    > [!NOTE]
    > Bevor Sie die erste Protokollsicherung erstellen können, müssen Sie eine vollständige Sicherung erstellen.

- Eine *Kopiesicherung* ist eine besondere vollständige Sicherung oder Protokollsicherung, die von der normalen Sequenz konventioneller Sicherungen unabhängig ist. Verwenden Sie zum Erstellen einer Kopiesicherung die Option COPY_ONLY in der BACKUP-Anweisung. Weitere Informationen finden Sie unter [Kopiesicherungen](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).

### <a name="transaction-log-truncation"></a><a name="Tlog_Truncation"></a> Kürzung des Transaktionsprotokolls

Um das schnelle Auffüllen des Transaktionsprotokolls einer Datenbank zu vermeiden, sind routinemäßige Sicherungen von entscheidender Bedeutung. Die Protokollkürzung erfolgt beim einfachen Wiederherstellungsmodell automatisch, wenn die Datenbank gesichert wurde, und beim vollständigen Wiederherstellungsmodell, wenn das Transaktionsprotokoll gesichert wurde. Manchmal kann der Kürzungsprozess jedoch verzögert werden. Informationen zu Faktoren, die eine Protokollkürzung verzögern können, finden Sie unter [Das Transaktionsprotokoll](../../relational-databases/logs/the-transaction-log-sql-server.md).

> [!NOTE]
> Die Optionen `BACKUP LOG WITH NO_LOG` und `WITH TRUNCATE_ONLY` werden nicht mehr unterstützt. Wechseln Sie zum einfachen Wiederherstellungsmodell, wenn Sie zur Wiederherstellung das vollständige oder das massenprotokollierte Wiederherstellungsmodell verwenden und die Protokollsicherungskette aus einer Datenbank entfernen müssen. Weitere Informationen finden Sie unter [Anzeigen oder Ändern des Wiederherstellungsmodells einer Datenbank](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md).

### <a name="formatting-backup-media"></a><a name="Formatting_Media"></a>Formatieren von Sicherungsmedien

Sicherungsmedien werden durch eine BACKUP-Anweisung formatiert, und zwar nur dann, wenn eine der folgenden Bedingungen zutrifft:

- Die Option `FORMAT` wird angegeben.
- Das Medium ist leer.
- Mit dem Vorgang wird ein Anschlussband geschrieben.

### <a name="working-with-backup-devices-and-media-sets"></a><a name="Backup_Devices_and_Media_Sets"></a>Arbeiten mit Sicherungsmedien und Mediensätzen

#### <a name="backup-devices-in-a-striped-media-set-a-stripe-set"></a>Sicherungsmedien in Stripesetmedien
Bei einem *Stripeset* handelt es sich um eine Gruppe von Datenträgerdateien, für die die Daten in Blöcke aufgeteilt und in einer festen Reihenfolge verteilt werden. Die Anzahl der in einem Stripeset verwendeten Sicherungsmedien muss gleich bleiben (es sei denn, die Medien werden mit `FORMAT` neu initialisiert).

Im folgenden Beispiel wird eine Sicherung der [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)]-Datenbank in ein neues Stripesetmedium geschrieben, für das drei Datenträgerdateien verwendet werden.

```sql
BACKUP DATABASE AdventureWorks2012
TO DISK='X:\SQLServerBackups\AdventureWorks1.bak',
DISK='Y:\SQLServerBackups\AdventureWorks2.bak',
DISK='Z:\SQLServerBackups\AdventureWorks3.bak'
WITH FORMAT,
  MEDIANAME = 'AdventureWorksStripedSet0',
  MEDIADESCRIPTION = 'Striped media set for AdventureWorks2012 database';
GO
```

Nachdem ein Sicherungsmedium als Teil eines Stripesets definiert wurde, kann es nicht für eine Sicherung mit einem einzelnen Medium verwendet werden, es sei denn, FORMAT wird angegeben. Ebenso kann ein Sicherungsmedium, das Sicherungen enthält, die nicht von einem Stripeset stammen, erst dann in einem Stripeset verwendet werden, wenn FORMAT angegeben wird. Verwenden Sie FORMAT, um einen im Stripesetformat vorliegenden Sicherungssatz zu teilen.

Ist weder MEDIANAME noch MEDIADESCRIPTION angegeben, wenn ein Medienheader geschrieben wird, bleibt das entsprechende Feld im Medienheader leer.

#### <a name="working-with-a-mirrored-media-set"></a>Verwenden eines gespiegelten Mediensatzes

In der Regel sind Sicherungen ungespiegelt, und BACKUP-Sicherungen enthalten einfach eine TO-Klausel. Pro Mediensatz sind jedoch insgesamt vier Spiegel möglich. Bei einem gespiegelten Mediensatz schreibt der Sicherungsvorgang in mehrere Gruppen von Sicherungsmedien. Jede Gruppe von Sicherungsmedien umfasst einen einzelnen Spiegel im gespiegelten Mediensatz. Jede Spiegelung muss dieselbe Anzahl und denselben Typ von physischen Sicherungsmedien verwenden, die alle über dieselben Eigenschaften verfügen müssen.

Zum Sichern eines gespiegelten Mediensatzes müssen alle Spiegel vorhanden sein. Zum Sichern eines gespiegelten Mediensatzes geben Sie die `TO`-Klausel an, um den ersten Spiegel anzugeben, und geben Sie eine `MIRROR TO`-Klausel für jeden zusätzlichen Spiegel an.

Bei einem gespiegelten Mediensatz muss jede `MIRROR TO`-Klausel dieselbe Anzahl (und denselben Typ) von Medien wie die TO-Klausel aufführen. Im folgenden Beispiel wird in einen gespiegelten Mediensatz geschrieben, der zwei Spiegel enthält und drei Medien pro Spiegel verwendet:

```sql
BACKUP DATABASE AdventureWorks2012
TO DISK='X:\SQLServerBackups\AdventureWorks1a.bak',
  DISK='Y:\SQLServerBackups\AdventureWorks2a.bak',
  DISK='Z:\SQLServerBackups\AdventureWorks3a.bak'
MIRROR TO DISK='X:\SQLServerBackups\AdventureWorks1b.bak',
  DISK='Y:\SQLServerBackups\AdventureWorks2b.bak',
  DISK='Z:\SQLServerBackups\AdventureWorks3b.bak';
GO
```

> [!IMPORTANT]
> Dieses Beispiel wurde so entwickelt, dass Sie es auf Ihrem lokalen System testen können. In der Praxis würde das Sichern auf mehreren Medien desselben Laufwerks die Leistung beeinträchtigen und die Redundanz zunichte machen, für die gespiegelte Mediensätze entwickelt wurden.

##### <a name="media-families-in-mirrored-media-sets"></a>Medienfamilien in gespiegelten Mediensätzen

Jedes in der `TO`-Klausel einer BACKUP-Sicherung angegebene Sicherungsmedium entspricht einer Medienfamilie. Werden beispielsweise in den `TO`-Klauseln drei Medien aufgelistet, schreibt BACKUP Daten in drei Medienfamilien. In einem gespiegelten Mediensatz muss jeder Spiegel eine Kopie jeder Medienfamilie enthalten. Aus diesem Grund muss die Anzahl der Medien in jedem Spiegel identisch sein.

Wenn für jeden Spiegel mehrere Medien aufgeführt sind, bestimmt die Reihenfolge der Medien, welche Medienfamilie in ein bestimmtes Medium geschrieben wird. In jeder Medienliste entspricht beispielsweise das zweite Medium der zweiten Medienfamilie. Für die im obigen Beispiel genannten Medien ist die Entsprechung zwischen Medien und Medienfamilien in der folgenden Tabelle dargestellt:

|Spiegel|Medienfamilie 1|Medienfamilie 2|Medienfamilie 3|
|---------|---------|---------|---------|
|0|`Z:\AdventureWorks1a.bak`|`Z:\AdventureWorks2a.bak`|`Z:\AdventureWorks3a.bak`|
|1|`Z:\AdventureWorks1b.bak`|`Z:\AdventureWorks2b.bak`|`Z:\AdventureWorks3b.bak`|

 Eine Medienfamilie muss immer auf dem gleichen Gerät innerhalb eines bestimmten Spiegels gesichert werden. Daher müssen Sie bei Verwendung eines vorhandenen Mediensatzes jedes Mal die Medien eines Spiegels in derselben Reihenfolge wie beim Erstellen des Mediensatzes aufführen.

Weitere Informationen über gespiegelte Mediensätze finden Sie unter [Gespiegelte Sicherungsmediensätze](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md). Weitere Informationen über Mediensätze und Medienfamilien finden Sie unter [Mediensätze, Medienfamilien und Sicherungssätze](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).

### <a name="restoring-sql-server-backups"></a><a name="Restoring_Backups"></a>Wiederherstellen von SQL Server-Sicherungen

Sie können die [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)-Anweisung von [!INCLUDE[tsql](../../includes/tsql-md.md)] oder die **Restore**-Tasks von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] verwenden, um eine Datenbank wiederherzustellen und optional wieder online zu schalten, oder um eine Datei oder Dateigruppe wiederherzustellen. Weitere Informationen finden Sie unter [Übersicht über Wiederherstellungsvorgänge](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).

## <a name="additional-considerations-about-backup-options"></a><a name="Additional_Considerations"></a>Weitere Überlegungen zu BACKUP-Optionen

### <a name="interaction-of-skip-noskip-init-and-noinit"></a><a name="Interactions_SKIP_etc"></a>Interaktion von SKIP, NOSKIP, INIT und NOINIT

In dieser Tabelle werden die Interaktionen zwischen den Optionen { **NOINIT** | INIT } und { **NOSKIP** | SKIP } beschrieben.

> [!NOTE]
> Wenn das Bandmedium leer oder die Datenträger-Sicherungsdatei nicht vorhanden ist, wird bei allen nachfolgend aufgeführten Interaktionen ein Medienheader geschrieben und erst danach fortgefahren. Wenn das Medium nicht leer ist und keinen gültigen Medienheader aufweist, wird von diesen Vorgängen die Rückmeldung gegeben, dass dies kein gültiges MTF-Medium ist, und der Sicherungsvorgang wird beendet.

|Skip-Option|NOINIT|INIT|
|------|------------|----------|
|NOSKIP|Überprüft, wenn das Volume einen gültigen Medienheader enthält, ob der Medienname mit dem angegebenen `MEDIANAME` (sofern vorhanden) übereinstimmt. Wenn dies der Fall ist, wird der Sicherungssatz angefügt, wobei alle vorhandenen Sicherungssätze beibehalten werden.<br /> Enthält das Volume keinen gültigen Medienheader, tritt ein Fehler auf.|Wenn das Volume einen gültigen Medienheader enthält, werden die folgenden Überprüfungen durchgeführt:<br /><ul><li>Wenn `MEDIANAME` angegeben wurde, wird überprüft, ob der angegebene Medienname mit dem Mediennamen im Medienheader übereinstimmt.<sup>1</sup></li><li>Stellt sicher, dass auf dem Medium keine noch nicht abgelaufenen Sicherungssätze vorhanden sind. Wenn solche vorhanden sind, wird die Sicherung beendet.</li></ul><br />Wenn diese Überprüfungen erfolgreich sind, werden alle Sicherungssätze auf dem Medium überschrieben, und nur der Medienheader wird beibehalten.<br /> Wenn das Volume keinen gültigen Medienheader enthält, wird dieser mithilfe der angegebenen Optionen `MEDIANAME` und `MEDIADESCRIPTION` (sofern vorhanden) generiert.|
|SKIP|Wenn das Volume einen gültigen Medienheader enthält, wird der Sicherungssatz angefügt. Alle vorhandenen Sicherungssätze werden beibehalten.|Wenn das Volume einen gültigen<sup>2</sup> Medienheader enthält, werden alle Sicherungssätze auf dem Medium überschrieben. Nur der Medienheader wird beibehalten.<br /> Wenn das Medium leer ist, wird ein Medienheader mithilfe der Optionen `MEDIANAME` und `MEDIADESCRIPTION` (sofern vorhanden) generiert.|

<sup>1</sup> Damit der Benutzer einen Sicherungsvorgang ausführen kann, muss er den entsprechenden festen Datenbank- oder Serverrollen angehören.

<sup>2</sup> Zur Gültigkeit gehören die MTF-Versionsnummer und andere Headerinformationen. Wenn die angegebene Version nicht unterstützt wird oder einen unerwarteten Wert hat, tritt ein Fehler auf.

## <a name="compatibility"></a>Kompatibilität

> [!CAUTION]
> Sicherungen, die mit aktuelleren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellt werden, können in früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]nicht wiederhergestellt werden.

Um die Abwärtskompatibilität mit früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu gewährleisten, unterstützt BACKUP die `RESTART`-Option. RESTART hat jedoch keine Auswirkungen.

## <a name="general-remarks"></a>Allgemeine Hinweise

Datenbank- oder Protokollsicherungen können an das Ende jedes beliebigen Datenträgers oder Bandmediums angefügt werden. Damit ist es möglich, eine Datenbank und ihre Transaktionsprotokolle an einem einzelnen physischen Speicherort unterzubringen.

Die BACKUP-Anweisung ist nicht in einer expliziten oder implizierten Transaktion zulässig.

Sicherungsvorgänge über Plattformen hinweg, sogar zwischen unterschiedlichen Prozessortypen, können ausgeführt werden, solange die Sortierung der Datenbank vom Betriebssystem unterstützt wird.

Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] ermöglicht die Einstellung `MAXTRANSFERSIZE` **größer als 65.536 (64 KB)** einen optimierten Komprimierungsalgorithmus für TDE-verschlüsselte Datenbanken ([Transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md)), der eine Seite zuerst entschlüsselt, komprimiert und dann wieder verschlüsselt. Wenn `MAXTRANSFERSIZE` nicht angegeben ist, oder wenn `MAXTRANSFERSIZE = 65536` (64 KB) verwendet wird, komprimiert die Sicherungskomprimierung mit TDE-verschlüsselten Datenbanken die verschlüsselten Seiten direkt und gibt möglicherweise keine guten Komprimierungsraten aus. Weitere Informationen finden Sie unter [Backup Compression for TDE-enabled Databases (Sicherungskomprimierung für TDE-fähige Datenbanken)](/archive/blogs/sqlcat/sqlsweet16-episode-1-backup-compression-for-tde-enabled-databases).

Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU5 muss `MAXTRANSFERSIZE` nicht mehr festgelegt werden, um diesen optimierten Komprimierungsalgorithmus mit TDE zu aktivieren. Wenn der Sicherungsbefehl `WITH COMPRESSION` angegeben ist, oder wenn die Serverkonfiguration *backup compression default* auf 1 festgelegt ist, wird `MAXTRANSFERSIZE` automatisch auf 128.000 erhöht, um den optimierten Algorithmus zu aktivieren. Wenn `MAXTRANSFERSIZE` für den Sicherungsbefehl mit einem Wert > 64.000 angegeben wird, wird der angegebene Wert berücksichtigt. Anders ausgedrückt: SQL Server verringert den Wert nie automatisch, sondern erhöht ihn nur. Wenn Sie eine TDE-verschlüsselte Datenbank mit `MAXTRANSFERSIZE = 65536` sichern müssen, müssen Sie `WITH NO_COMPRESSION` angeben oder sicherstellen, dass die Serverkonfiguration *backup compression default* auf 0 festgelegt ist.

> [!NOTE]
> In einigen Fällen ist die Standardeinstellung von `MAXTRANSFERSIZE` größer als 64 KB:
>
> - Wenn die Datenbank mehrere Datendateien erstellt hat, wird `MAXTRANSFERSIZE` > 64 KB verwendet.
> - Beim Ausführen der Sicherung über URLs beträgt die Standardeinstellung `MAXTRANSFERSIZE = 1048576` (1 MB).
>
> Selbst wenn eine der folgenden Bedingungen gilt, müssen Sie explizit festlegen, dass `MAXTRANSFERSIZE` im Sicherungsbefehl größer als 64 KB ist, damit der optimierte Algorithmus für die Sicherungskomprimierung zurückgegeben wird, wenn Sie nicht [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU5 oder höher verwenden.

Standardmäßig wird bei jedem erfolgreichen Sicherungsvorgang dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Fehlerprotokoll und dem Systemereignisprotokoll ein Eintrag hinzugefügt. Wenn Sie das Protokoll regelmäßig sichern, kann die Anzahl dieser Erfolgsmeldungen schnell ansteigen, d. h., es entstehen sehr große Fehlerprotokolle, die das Suchen nach anderen Meldungen erschweren können. In solchen Fällen können Sie diese Protokolleinträge mithilfe des Ablaufverfolgungsflags 3226 unterdrücken, wenn keines der Skripts von diesen Einträgen abhängig ist. Weitere Informationen finden Sie unter [Ablaufverfolgungsflags](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

## <a name="interoperability"></a>Interoperabilität

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet einen Onlinesicherungsprozess, um das Ausführen einer Datenbanksicherung zu ermöglichen, während die Datenbank weiterhin verwendet wird. Bei einer Sicherung sind die meisten Vorgänge möglich, so sind z. B. die Anweisungen INSERT, UPDATE oder DELETE bei einem Sicherungsvorgang zulässig.

Folgende Vorgänge können nicht ausgeführt werden, während eine Datenbank oder ein Transaktionsprotokoll gesichert werden:

- Dateiverwaltungsvorgänge, wie z.B. die `ALTER DATABASE`-Anweisung mit der Option `ADD FILE` oder `REMOVE FILE`.

- Vorgänge zum Verkleinern der Datenbank oder von Dateien. Dazu gehören auch Vorgänge zum automatischen Verkleinern.

Wenn sich ein Sicherungsvorgang mit einem Dateiverwaltungs- oder Verkleinerungsvorgang überschneidet, tritt ein Konflikt auf. Unabhängig davon, welcher am Konflikt beteiligte Vorgang zuerst begonnen hat, wartet der zweite Vorgang auf das Timeout der Sperre, die vom ersten Vorgang angewendet wurde (der Timeoutzeitraum wird durch eine Timeouteinstellung für die Sitzung gesteuert). Wenn die Sperre während des Timeoutzeitraums aufgehoben wird, wird der zweite Vorgang fortgesetzt. Wenn das Timeout für die Sperre eintritt, erzeugt der zweite Vorgang einen Fehler.

## <a name="metadata"></a>Metadaten

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sind folgende Tabellen mit Sicherungsverläufen enthalten, in denen Sicherungsaktivitäten nachverfolgt werden:

- [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md)
- [backupfilegroup](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)
- [backupmediafamily](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)
- [backupmediaset](../../relational-databases/system-tables/backupmediaset-transact-sql.md)
- [backupset](../../relational-databases/system-tables/backupset-transact-sql.md)

Wird eine Wiederherstellung durchgeführt, wenn der Sicherungssatz noch nicht in der **msdb**-Datenbank erfasst ist, werden möglicherweise die Tabellen mit Sicherungsverläufen verändert.

## <a name="security"></a>Sicherheit

Ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] werden die Optionen `PASSWORD` und `MEDIAPASSWORD` nicht mehr für die Erstellung von Sicherungen verwendet. Es ist weiterhin möglich, mit Kennwörtern erstellte Sicherungen wiederherzustellen.

### <a name="permissions"></a>Berechtigungen

Mitglieder der festen Serverrolle **sysadmin** und der festen Datenbankrollen **db_owner** und **db_backupoperator** verfügen standardmäßig über BACKUP DATABASE- und BACKUP LOG-Berechtigungen.

Besitz- und Berechtigungsprobleme im Zusammenhang mit der physischen Datei des Sicherungsmediums können den Sicherungsvorgang beeinträchtigen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muss über Lese- und Schreibberechtigungen für das Medium verfügen. Das Konto, unter dem der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst ausgeführt wird, muss Schreibberechtigungen haben. Allerdings prüft die gespeicherte Prozedur [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), die den Systemtabellen einen Eintrag für ein Sicherungsmedium hinzufügt, nicht die Dateizugriffsberechtigungen. Solche Probleme mit der physischen Datei des Sicherungsmediums treten möglicherweise erst auf, wenn auf die physische Ressource zugegriffen wird, um einen Sicherungs- oder Wiederherstellungsvorgang auszuführen.

## <a name="examples"></a><a name="examples"></a> Beispiele

Dieser Abschnitt enthält folgende Beispiele:

- A. [Sichern einer vollständigen Datenbank](#backing_up_db)
- B. [Sichern der Datenbank und des Protokolls](#backing_up_db_and_log)
- C. [Erstellen einer vollständigen Dateisicherung der sekundären Dateigruppen](#full_file_backup)
- D: [Erstellen einer differenziellen Dateisicherung der sekundären Dateigruppen](#differential_file_backup)
- E. [Erstellen eines gespiegelten Mediensatzes für eine Medienfamilie und Sichern auf einen gespiegelten Mediensatz für eine Medienfamilie](#create_single_family_mirrored_media_set)
- F. [Erstellen eines gespiegelten Mediensatzes für mehrere Medienfamilien und Sichern auf einen gespiegelten Mediensatz für mehrere Medienfamilien](#create_multifamily_mirrored_media_set)
- G. [Sichern auf einen vorhandenen gespiegelten Mediensatz](#existing_mirrored_media_set)
- H. [Erstellen einer komprimierten Sicherung in einem neuen Mediensatz](#creating_compressed_backup_new_media_set)
- I. [Sichern in den Microsoft Azure BLOB-Speicherdienst](#url)
- J. [Überwachen des Fortschritts einer BACKUP-Anweisung](#backup_progress)

> [!NOTE]
> In den Themen über die Vorgehensweisen bei der Sicherung sind weitere Beispiele aufgeführt. Weitere Informationen finden Sie unter [Übersicht über Sicherungen](../../relational-databases/backup-restore/backup-overview-sql-server.md).

### <a name="a-backing-up-a-complete-database"></a><a name="backing_up_db"></a> A. Sichern einer vollständigen Datenbank

Im folgenden Beispiel wird die [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)]-Datenbank in einer Datenträgerdatei gesichert.

```sql
BACKUP DATABASE AdventureWorks2012
 TO DISK = 'Z:\SQLServerBackups\AdvWorksData.bak'
    WITH FORMAT;
GO
```

### <a name="b-backing-up-the-database-and-log"></a><a name="backing_up_db_and_log"></a> B. Sichern der Datenbank und des Protokolls

Im folgenden Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Beispieldatenbank gesichert, in der standardmäßig das einfache Wiederherstellungsmodell verwendet wird. Zum Unterstützen der Protokollsicherungen wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank für die Verwendung des vollständigen Wiederherstellungsmodells geändert.

Im Beispiel wird [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md) verwendet, um ein logisches [Sicherungsmedium](../../relational-databases/backup-restore/backup-devices-sql-server.md) zum Sichern von Daten zu erstellen, `AdvWorksData`, und ein anderes logisches Sicherungsmedium zum Sichern des Protokolls, `AdvWorksLog`, wird erstellt.

Im Beispiel wird eine vollständige Datenbanksicherung in `AdvWorksData` erstellt, und nach einer Updateaktivität wird das Protokoll in `AdvWorksLog` gesichert.

```sql
-- To permit log backups, before the full database backup, modify the database
-- to use the full recovery model.
USE master;
GO
ALTER DATABASE AdventureWorks2012
    SET RECOVERY FULL;
GO
-- Create AdvWorksData and AdvWorksLog logical backup devices.
USE master
GO
EXEC sp_addumpdevice 'disk', 'AdvWorksData',
'Z:\SQLServerBackups\AdvWorksData.bak';
GO
EXEC sp_addumpdevice 'disk', 'AdvWorksLog',
'X:\SQLServerBackups\AdvWorksLog.bak';
GO

-- Back up the full AdventureWorks2012 database.
BACKUP DATABASE AdventureWorks2012 TO AdvWorksData;
GO
-- Back up the AdventureWorks2012 log.
BACKUP LOG AdventureWorks2012
    TO AdvWorksLog;
GO
```

> [!NOTE]
> Für eine Produktionsdatenbank sollte das Protokoll regelmäßig gesichert werden. Protokollsicherungen sollten möglichst häufig ausgeführt werden, damit ein ausreichender Schutz vor Datenverlusten besteht.

### <a name="c-creating-a-full-file-backup-of-the-secondary-filegroups"></a><a name="full_file_backup"></a> C. Erstellen einer vollständigen Dateisicherung der sekundären Dateigruppen

Im folgenden Beispiel wird eine vollständige Dateisicherung von jeder Datei der beiden sekundären Dateigruppen erstellt.

```sql
--Back up the files in SalesGroup1:
BACKUP DATABASE Sales
    FILEGROUP = 'SalesGroup1',
    FILEGROUP = 'SalesGroup2'
    TO DISK = 'Z:\SQLServerBackups\SalesFiles.bck';
GO
```

### <a name="d-creating-a-differential-file-backup-of-the-secondary-filegroups"></a><a name="differential_file_backup"></a> D. Erstellen einer differenziellen Dateisicherung der sekundären Dateigruppen

Im folgenden Beispiel wird eine differenzielle Dateisicherung von jeder Datei der beiden sekundären Dateigruppen erstellt.

```sql
--Back up the files in SalesGroup1:
BACKUP DATABASE Sales
    FILEGROUP = 'SalesGroup1',
    FILEGROUP = 'SalesGroup2'
    TO DISK = 'Z:\SQLServerBackups\SalesFiles.bck'
    WITH
      DIFFERENTIAL;
GO
```

### <a name="e-creating-and-backing-up-to-a-single-family-mirrored-media-set"></a><a name="create_single_family_mirrored_media_set"></a> E. Erstellen eines gespiegelten Mediensatzes für eine Medienfamilie und Sichern auf einen gespiegelten Mediensatz für eine Medienfamilie

Im folgenden Beispiel wird ein gespiegelter Mediensatz erstellt, der eine einzelne Medienfamilie und vier Spiegel umfasst. Auf diese wird die [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)]-Datenbank gesichert.

```sql
BACKUP DATABASE AdventureWorks2012
TO TAPE = '\\.\tape0'
MIRROR TO TAPE = '\\.\tape1'
MIRROR TO TAPE = '\\.\tape2'
MIRROR TO TAPE = '\\.\tape3'
WITH
    FORMAT,
    MEDIANAME = 'AdventureWorksSet0';
```

### <a name="f-creating-and-backing-up-to-a-multifamily-mirrored-media-set"></a><a name="create_multifamily_mirrored_media_set"></a> F. Erstellen eines gespiegelten Mediensatzes für mehrere Medienfamilien und Sichern auf einen gespiegelten Mediensatz für mehrere Medienfamilien

Im folgenden Beispiel wird ein gespiegelter Mediensatz erstellt, in dem jeder Spiegel zwei Medienfamilien enthält. Im Beispiel wird dann die [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)]-Datenbank auf beide Spiegel gesichert.

```sql
BACKUP DATABASE AdventureWorks2012
TO TAPE = '\\.\tape0', TAPE = '\\.\tape1'
MIRROR TO TAPE = '\\.\tape2', TAPE = '\\.\tape3'
WITH
    FORMAT,
    MEDIANAME = 'AdventureWorksSet1';
```

### <a name="g-backing-up-to-an-existing-mirrored-media-set"></a><a name="existing_mirrored_media_set"></a> G. Sichern auf einen vorhandenen gespiegelten Mediensatz

Im folgenden Beispiel wird ein Sicherungssatz an den Mediensatz angefügt, der im vorherigen Beispiel erstellt wurde.

```sql
BACKUP LOG AdventureWorks2012
TO TAPE = '\\.\tape0', TAPE = '\\.\tape1'
MIRROR TO TAPE = '\\.\tape2', TAPE = '\\.\tape3'
WITH
    NOINIT,
    MEDIANAME = 'AdventureWorksSet1';
```

> [!NOTE]
> Die Standardeinstellung NOINIT wird hier zur Verdeutlichung gezeigt.

### <a name="h-creating-a-compressed-backup-in-a-new-media-set"></a><a name="creating_compressed_backup_new_media_set"></a> H. Erstellen einer komprimierter Sicherung in einem neuen Mediensatz

Im folgenden Beispiel wird das Medium formatiert, wobei ein neuer Mediensatz angelegt wird, und eine vollständige, komprimierte Sicherung der [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)]-Datenbank wird erstellt.

```sql
BACKUP DATABASE AdventureWorks2012 TO DISK='Z:\SQLServerBackups\AdvWorksData.bak'
WITH
    FORMAT,
    COMPRESSION;
```

### <a name="i-backing-up-to-the-microsoft-azure-blob-storage-service"></a><a name="url"></a> I. Sichern in den Microsoft Azure BLOB-Speicherdienst

In dem Beispiel wird eine vollständige Sicherung der `Sales`-Datenbank in den Microsoft Azure BLOB-Speicherdienst ausgeführt. Der Speicherkontoname lautet `mystorageaccount`. Der Container heißt `myfirstcontainer`. Eine gespeicherte Zugriffsrichtlinie wurde mit Lese-, Schreib-, Lösch- und Auflistungsrechten erstellt. Die Anmeldeinformation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`https://mystorageaccount.blob.core.windows.net/myfirstcontainer` wurde mit einer Shared Access Signature (SAS) erstellt, die der gespeicherten Zugriffsrichtlinie zugeordnet ist. Informationen zur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sicherung in Microsoft Azure Blob Storage finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit Microsoft Azure Blob Storage](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) und [SQL Server-Sicherung über URLs](../../relational-databases/backup-restore/sql-server-backup-to-url.md).

```sql
BACKUP DATABASE Sales
TO URL = 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_20160726.bak'
WITH STATS = 5;
```

### <a name="j-track-the-progress-of-backup-statement"></a><a name="backup_progress"></a> J. Überwachen des Fortschritts von BACKUP-Anweisungen

Die folgende Abfrage gibt Informationen zu den aktuell ausgeführten BACKUP-Anweisungen zurück:
```sql
SELECT query = a.text, start_time, percent_complete,
    eta = dateadd(second,estimated_completion_time/1000, getdate())
FROM sys.dm_exec_requests r
    CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
WHERE r.command LIKE 'BACKUP%'
```

## <a name="see-also"></a>Weitere Informationen

- [Sicherungsmedien](../../relational-databases/backup-restore/backup-devices-sql-server.md)
- [Mediensätze, Medienfamilien und Sicherungssätze](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)
- [Sicherungen des Protokollfragments](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)
- [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)
- [DBCC SQLPERF](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md)
- [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)
- [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)
- [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)
- [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)
- [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)
- [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_helpfile](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)
- [sp_helpfilegroup](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)
- [Serverkonfigurationsoptionen](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
- [Schrittweise Wiederherstellung von Datenbanken mit speicheroptimierten Tabellen](../../relational-databases/in-memory-oltp/piecemeal-restore-of-databases-with-memory-optimized-tables.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](backup-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](backup-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Verwaltete Azure SQL-Instanz

Dieser Dienst sichert eine SQL-Datenbank, die sich in Azure SQL Managed Instance befindet bzw. gehostet wird. SQL [Managed Instance](/azure/sql-database/sql-database-managed-instance) verfügt über automatische Sicherungen und ermöglicht es Benutzern, vollständige `COPY_ONLY`-Sicherungen von Datenbanken zu erstellen. Differenzielle Sicherungen, Protokollsicherungen und Dateimomentaufnahmesicherungen werden nicht unterstützt.

## <a name="syntax"></a>Syntax

```syntaxsql
BACKUP DATABASE { database_name | @database_name_var }
  TO URL = { 'physical_device_name' | @physical_device_name_var }[ ,...n ]
  WITH COPY_ONLY [, { <general_WITH_options> } ]
[;]

<general_WITH_options> [ ,...n ]::=

--Media Set Options
   MEDIADESCRIPTION = { 'text' | @text_variable }
 | MEDIANAME = { media_name | @media_name_variable }
 | BLOCKSIZE = { blocksize | @blocksize_variable }

--Data Transfer Options
   BUFFERCOUNT = { buffercount | @buffercount_variable }
 | MAXTRANSFERSIZE = { maxtransfersize | @maxtransfersize_variable }

--Error Management Options
   { NO_CHECKSUM | CHECKSUM }
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }

--Compatibility Options
   RESTART

--Monitoring Options
   STATS [ = percentage ]

--Encryption Options
 ENCRYPTION (ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY } , encryptor_options ) <encryptor_options> ::=
   SERVER CERTIFICATE = Encryptor_Name | SERVER ASYMMETRIC KEY = Encryptor_Name
```

## <a name="arguments"></a>Argumente

DATABASE Gibt eine vollständige Datenbanksicherung an. Während einer Datenbanksicherung sichert Azure SQL Managed Instance einen ausreichenden Anteil des Transaktionsprotokolls, um eine konsistente Datenbank zu erstellen, wenn die Sicherung wiederhergestellt wird.

> [!IMPORTANT]
> Eine Datenbanksicherung, die auf einer verwalteten Instanz erstellt wird, kann nur auf einer anderen Instanz von Azure SQL Managed Instance wiederhergestellt werden. Sie kann nicht auf einer lokalen SQL Server-Instanz wiederhergestellt werden (genauso wenig, wie eine Sicherung einer SQL Server 2016-Datenbank auf einer SQL Server 2012-Instanz wiederhergestellt werden kann).

Wenn Sie eine von BACKUP DATABASE (eine *Datensicherung*) erstellte Sicherung wiederherstellen, wird die komplette Sicherung wiederhergestellt. Informationen zum Wiederherstellen der automatischen Sicherungen von SQL Managed Instance finden Sie unter [Wiederherstellen einer Datenbank in Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance-get-started-restore).

{ *database_name* |  **@** _database\_name\_var_ } ist die Datenbank, aus der die vollständige Datenbank gesichert wird. Bei Angabe in Form einer Variablen ( **@** _database\_name\_var_) kann dieser Name entweder als Zeichenfolgenkonstante ( **@** _database\_name\_var_ **=** _database name_) oder als Variable eines Zeichenfolgen-Datentyps (mit Ausnahme der Datentypen **ntext** oder **text**) angegeben werden.

Weitere Informationen finden Sie unter [Vollständige Dateisicherungen](../../relational-databases/backup-restore/full-file-backups-sql-server.md) und [Sichern von Dateien und Dateigruppen](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md).

TO URL

Gibt die URL an, die für den Sicherungsvorgang verwendet werden soll. Das URL-Format wird zum Erstellen von Sicherungen in Microsoft Azure Storage verwendet.

> [!IMPORTANT]
> Um auf mehreren Geräten eine Sicherung durchzuführen, wenn Sie über die URL sichern, müssen Sie Shared Access Signature-Token (SAS) verwenden. Beispiele für die Erstellung einer Shared Access Signature finden Sie unter [SQL Server-Sicherung über URLs](../../relational-databases/backup-restore/sql-server-backup-to-url.md) und [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with Powershell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell) (Vereinfachen der Erstellung von SQL-Anmeldeinformationen mit Shared Access Signature-Token in Azure Storage mit PowerShell).

*n* Ein Platzhalter, der anzeigt, dass in einer durch Trennzeichen getrennten Liste möglicherweise bis zu 64 Sicherungsmedien angegeben werden.

### <a name="with-optionsspecifies-options-to-be-used-with-a-backup-operation"></a>Optionen für WITH Gibt die Optionen an, die bei einem Sicherungsvorgang verwendet werden sollen.

CREDENTIAL Wird nur verwendet, wenn eine Sicherung im Dienst Microsoft Azure Blob Storage erstellt wird.

ENCRYPTION Wird verwendet, um die Verschlüsselung für eine Sicherung anzugeben. Sie können einen Verschlüsselungsalgorithmus angeben, um die Sicherung zu verschlüsseln, oder `NO_ENCRYPTION` angeben, um die Sicherung nicht zu verschlüsseln. Die Verschlüsselung wird empfohlen, um Sicherungsdateien zu sichern. Die Liste der Algorithmen, die Sie angeben können:

- `AES_128`
- `AES_192`
- `AES_256`
- `TRIPLE_DES_3KEY`
- `NO_ENCRYPTION`

Wenn Sie sich für die Verschlüsselung entscheiden, müssen Sie auch die Verschlüsselung mithilfe der Verschlüsselungsoptionen angeben:

- `SERVER CERTIFICATE = <Encryptor_Name>`
- `SERVER ASYMMETRIC KEY = <Encryptor_Name>`

**Sicherungssatzoptionen**

COPY_ONLY gibt an, dass die Sicherung eine *Kopiesicherung* ist, die keine Auswirkungen auf die normale Sicherungssequenz hat. Eine Kopiesicherung wird unabhängig von automatischen Azure SQL-Datenbank-Sicherungen erstellt. Weitere Informationen finden Sie unter [Kopiesicherungen](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).

{ COMPRESSION | NO_COMPRESSION } Gibt an, ob für diese Sicherung eine [Sicherungskomprimierung](../../relational-databases/backup-restore/backup-compression-sql-server.md) verwendet wird, wodurch die Standardeinstellung auf Serverebene überschrieben wird.

Die Sicherungskomprimierung wird standardmäßig deaktiviert. Diese Standardeinstellung kann jedoch durch Festlegen der Serverkonfigurationsoption [backup compression default](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md) geändert werden. Informationen zum Anzeigen des aktuellen Werts dieser Option finden Sie unter [Anzeigen oder Ändern von Servereigenschaften](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md).

COMPRESSION Aktiviert die Sicherungskomprimierung explizit.

NO_COMPRESSION Deaktiviert die Sicherungskomprimierung explizit.

DESCRIPTION **=** { **'** _text_ **'**  |  **@** _text\_variable_ } Gibt den freien Text an, der als Beschreibung des Sicherungssatzes verwendet wird. Die Zeichenfolge kann maximal 255 Zeichen haben.

NAME **=** { *backup_set_name* |  **@** _backup\_set\_var_ } Gibt den Namen des Sicherungssatzes an. Namen können maximal 128 Zeichen haben. Wird NAME nicht angegeben, erhält der Sicherungssatz einen leeren Namen.

MEDIADESCRIPTION **=** { *text* | **@** _text\_variable_ } Gibt die freie Textbeschreibung des Mediensatzes an, die aus maximal 255 Zeichen bestehen kann.

MEDIANAME **=** { *media_name* |  **@** _media\_name\_variable_ } Gibt den Mediennamen für den gesamten Sicherungsmediensatz an. Der Medienname darf nicht mehr als 128 Zeichen umfassen. Wird `MEDIANAME` angegeben, muss dieser Name dem vorher angegebenen Mediennamen auf den Sicherungsvolumes entsprechen. Wird er nicht angegeben, oder ist die Option SKIP festgelegt, findet keine Prüfung des Mediennamens statt.

BLOCKSIZE **=** { *blocksize* |  **@** _blocksize\_variable_ } Legt die physische Blockgröße in Bytes fest. Die unterstützten Größen sind 512, 1024, 2048, 4096, 8192, 16.384, 32.768 und 65.536 (64 KB) Bytes. Der Standardwert ist 65.536 für Bandmedien und andernfalls 512. In der Regel ist diese Option nicht erforderlich, da von BACKUP automatisch eine Blockgröße ausgewählt wird, die für das Medium geeignet ist. Mit der expliziten Angabe einer Blockgröße wird die automatische Wahl der Blockgröße überschrieben.

**Datenübertragungsoptionen**

BUFFERCOUNT **=** { *buffercount* |  **@** _buffercount\_variable_ } Gibt die Gesamtanzahl von E/A-Puffern an, die für den Sicherungsvorgang verwendet werden sollen. Sie können eine beliebige positive ganze Zahl angeben. Eine große Pufferanzahl kann jedoch wegen eines ungeeigneten virtuellen Adressraumes im Prozess Sqlservr.exe zu Fehlern aufgrund von nicht genügend Arbeitsspeicher führen.

Der gesamte von den Puffern belegte Speicherplatz wird durch `BUFFERCOUNT * MAXTRANSFERSIZE` bestimmt.

> [!NOTE]
> Wichtige Informationen zur Verwendung der `BUFFERCOUNT`-Option finden Sie im Blogeintrag [Falsche BufferCount-Datenübertragungsoption kann OOM-Bedingung auslösen](/archive/blogs/sqlserverfaq/incorrect-buffercount-data-transfer-option-can-lead-to-oom-condition).

MAXTRANSFERSIZE **=** { *maxtransfersize* | _**@** maxtransfersize\_variable_ } Gibt die größte zu verwendende Einheit für Übertragungen zwischen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und dem Sicherungsmedium in Bytes an. Die möglichen Werte sind Vielfache von 65.536 Bytes (64 KB) bis hin zu 4.194.304 Bytes (4 MB).

> [!NOTE]
> Für [Transparente Datenverschlüsselung (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md)-fähige Datenbanken mit einer einzelnen Datendatei ist der standardmäßige `MAXTRANSFERSIZE`-Wert 65536 (64 KB). Für nicht mit TDE verschlüsselte Datenbanken ist der `MAXTRANSFERSIZE`-Standardwert 1048576 (1 MB) bei Verwendung der Sicherung auf DISK und 65536 (64 KB) bei Verwendung von VDI oder TAPE.

**Fehlerverwaltungsoptionen**

Mit diesen Optionen können Sie bestimmen, ob Sicherungsprüfsummen für den Sicherungsvorgang aktiviert werden und ob der Vorgang beim Auftreten eines Fehlers beendet wird.

{ **NO_CHECKSUM** | CHECKSUM } Steuert, ob Sicherungsprüfsummen aktiviert sind.

NO_CHECKSUM Das Generieren von Sicherungsprüfsummen (und die Überprüfung von Seitenprüfsummen) wird explizit deaktiviert. Dies ist das Standardverhalten.

CHECKSUM Gibt an, dass der Sicherungsvorgang jede Seite auf Prüfsumme und zerrissene Seiten überprüft (wenn aktiviert und verfügbar) und eine Prüfsumme für die gesamte Sicherung generiert.

Die Verwendung von Sicherungsprüfsummen kann sich auf die Arbeitsauslastung und den Durchsatz bei der Sicherung auswirken.

Weitere Informationen finden Sie unter [Mögliche Medienfehler während der Sicherung und Wiederherstellung](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).

{ **STOP_ON_ERROR** | CONTINUE_AFTER_ERROR } Steuert, ob ein Sicherungsvorgang beendet oder fortgesetzt wird, nachdem ein Fehler bei der Seitenprüfsumme aufgetreten ist.

STOP_ON_ERROR Von BACKUP wird ein Fehler erzeugt, wenn eine Seitenprüfsumme nicht stimmt. Dies ist das Standardverhalten.

CONTINUE_AFTER_ERROR BACKUP wird auch dann fortgesetzt, wenn Fehler auftreten, z.B. ungültige Prüfsummen oder zerrissene Seiten.

Wenn Sie das Protokollfragment mithilfe der Option NO_TRUNCATE nicht sichern können, wenn die Datenbank beschädigt ist, können Sie versuchen, eine [Sicherung des Protokollfragments](../../relational-databases/backup-restore/tail-log-backups-sql-server.md) auszuführen, indem Sie CONTINUE_AFTER_ERROR anstelle von NO_TRUNCATE angeben.

Weitere Informationen finden Sie unter [Mögliche Medienfehler während der Sicherung und Wiederherstellung](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).

**Kompatibilitätsoptionen**

RESTART hat keine Auswirkungen. Die Option wird von der Version aus Gründen der Kompatibilität mit früheren Versionen von SQL Server angenommen.

**Überwachungsoptionen**

STATS [ **=** _percentage_ ] Zeigt nach jedem abgeschlossenen *Prozentsatz* eine Meldung an, und wird als Statusanzeige verwendet. Wird *Prozentsatz* nicht angegeben, zeigt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] jedes Mal eine Meldung an, wenn weitere 10 Prozent des Vorgangs abgeschlossen sind.

Mit der Option STATS wird der Prozentsatz gemeldet, der beim Erreichen des Schwellenwertes für das nächste Meldungsintervall abgeschlossen ist. Bei dem angegebenen Prozentsatz handelt es sich um einen ungefähren Wert. Wird beispielsweise STATS=10 festgelegt und sind 40 Prozent des Vorgangs abgeschlossen, dann zeigt die Option u. U. 43 Prozent an. Bei größeren Sicherungssätzen stellt dies kein Problem dar, da sich der Wert für den abgeschlossenen Prozentsatz zwischen abgeschlossenen E/A-Aufrufen nur sehr langsam verändert.

## <a name="limitations-for-sql-managed-instance"></a>Einschränkungen von SQL Managed Instance

Die max. Streifengröße bei der Speicherung beträgt 195 GB (maximale Blob-Größe). Erhöhen Sie die Streifenanzahl im Backup-Befehl, um die einzelne Streifengröße zu reduzieren und innerhalb dieser Einschränkungen zu bleiben.

## <a name="security"></a>Sicherheit

### <a name="permissions"></a>Berechtigungen

Mitglieder der festen Serverrolle **sysadmin** und der festen Datenbankrollen **db_owner** und **db_backupoperator** verfügen standardmäßig über BACKUP DATABASE-Berechtigungen.

Besitz- und Berechtigungsprobleme im Zusammenhang mit der URL können den Sicherungsvorgang beeinträchtigen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muss über Lese- und Schreibberechtigungen für das Medium verfügen. Das Konto, unter dem der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst ausgeführt wird, muss Schreibberechtigungen haben.

## <a name="examples"></a><a name="examples"></a> Beispiele

In dem Beispiel wird eine COPY_ONLY-Sicherung von `Sales` in Microsoft Azure Blob Storage ausgeführt. Der Speicherkontoname lautet `mystorageaccount`. Der Container heißt `myfirstcontainer`. Eine gespeicherte Zugriffsrichtlinie wurde mit Lese-, Schreib-, Lösch- und Auflistungsrechten erstellt. Die Anmeldeinformation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`https://mystorageaccount.blob.core.windows.net/myfirstcontainer` wurde mit einer Shared Access Signature (SAS) erstellt, die der gespeicherten Zugriffsrichtlinie zugeordnet ist. Informationen zur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sicherung in Microsoft Azure Blob Storage finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit Microsoft Azure Blob Storage](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) und [SQL Server-Sicherung über URLs](../../relational-databases/backup-restore/sql-server-backup-to-url.md).

```sql
BACKUP DATABASE Sales
TO URL = 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_20160726.bak'
WITH STATS = 5, COPY_ONLY;
```

## <a name="see-also"></a>Weitere Informationen

[Datenbank wiederherstellen](restore-statements-transact-sql.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](backup-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](backup-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Analyseplattformsystem

Erstellt eine Sicherungskopie einer [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Datenbank und speichert die Sicherung nicht auf der Appliance, sondern in einem benutzerdefinierten Netzwerkpfad. Verwenden Sie diese Anweisung mit [RESTORE DATABASE - Analytics Platform System](../../t-sql/statements/restore-statements-transact-sql.md) zur Notfallwiederherstellung oder zum Kopieren einer Datenbank von einer Appliance auf eine andere.

**Bevor Sie beginnen**, lesen Sie „Erwerb und Konfiguration eines Sicherungsservers“ in der [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Es gibt zwei Arten von Sicherungen in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Eine *vollständige Datenbanksicherung* ist eine Sicherung einer kompletten [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Datenbank. Bei einer *differenziellen Datenbanksicherung* werden nur die Änderungen seit der letzten vollständigen Datenbanksicherung erfasst. Die Sicherung einer Benutzerdatenbank schließt Datenbankbenutzer und Datenbankrollen ein. Eine Sicherung der Masterdatenbank schließt Anmeldungen ein.

Weitere Informationen zu [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Datenbanksicherungen finden Sie unter „Sichern und Wiederherstellen“ in der [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

## <a name="syntax"></a>Syntax

```syntaxsql
--Create a full backup of a user database or the master database.
BACKUP DATABASE database_name
    TO DISK = '\\UNC_path\backup_directory'
    [ WITH [ ( ]<with_options> [ ,...n ][ ) ] ]
[;]

--Create a differential backup of a user database.
BACKUP DATABASE database_name
    TO DISK = '\\UNC_path\backup_directory'
    WITH [ ( ] DIFFERENTIAL
    [ , <with_options> [ ,...n ] [ ) ]
[;]

<with_options> ::=
    DESCRIPTION = 'text'
    | NAME = 'backup_name'
```

## <a name="arguments"></a>Argumente

*database_name* Der Name der Datenbank, in der eine Sicherung erstellt werden soll. Die Datenbank kann die Masterdatenbank oder eine Benutzerdatenbank sein.

TO DISK = '\\\\*UNC_path*\\*backup_directory*' Netzwerkpfad und Verzeichnis, in die [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] die Sicherungsdateien schreibt. Beispiel: '\\\xxx.xxx.xxx.xxx\backups\2012\Monthly\08.2012.Mybackup'.

- Der Pfad zum Sicherungsverzeichnisnamen muss bereits vorhanden sein und als vollqualifizierter UNC-Pfad (Universal Naming Convention) angegeben werden.
- Das Sicherungsverzeichnis *backup_directory* darf nicht vor dem Ausführen des Sicherungsbefehls vorhanden sein. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] erstellt das Sicherungsverzeichnis.
- Beim Pfad zum Sicherungsverzeichnis darf es sich nicht um einen lokalen Pfad handeln, und der Speicherort darf sich nicht auf einem [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Applianceknoten befinden.
- Die maximale Länge des UNC-Pfads und des Sicherungsverzeichnisnamens beträgt 200 Zeichen.
- Der Server oder Host muss als IP-Adresse angegeben werden. Sie können ihn nicht als Host- oder Servernamen angeben.

DESCRIPTION = **'** _Text_ **'** Gibt eine Textbeschreibung der Datensicherung an. Die maximale Länge des Texts beträgt 255 Zeichen.

Die Beschreibung wird in den Metadaten gespeichert und angezeigt, wenn der Sicherungsheader mit RESTORE HEADERONLY wiederhergestellt wird.

NAME = **'** _backup \_name_ **'** Gibt den Namen des Sicherungssatzes an. Der Sicherungsname kann vom Datenbanknamen abweichen.

- Namen können maximal 128 Zeichen haben.
- Sie können keinen Pfad einschließen.
- An der ersten Stelle muss ein Buchstabe, eine Ziffer oder ein Unterstrich (_) stehen. Zulässige Sonderzeichen sind Unterstrich (\_), Bindestrich (-) oder Leerzeichen ( ). Sicherungsnamen dürfen nicht mit einem Leerzeichen enden.
- Die Anweisung schlägt fehl, wenn *backup_name* am angegebenen Speicherort bereits vorhanden ist.

Der Name wird in den Metadaten gespeichert und angezeigt, wenn der Sicherungsheader mit RESTORE HEADERONLY wiederhergestellt wird.

DIFFERENTIAL Gibt an, dass eine differenzielle Sicherung einer Benutzerdatenbank durchgeführt werden soll. Wenn nicht angegeben, ist die Standardeinstellung eine vollständige Sicherung. Der Name der differenziellen Sicherung muss nicht mit dem Namen der vollständigen Sicherung übereinstimmen. Zum Nachverfolgen der differenziellen Sicherung und der zugehörigen vollständigen Sicherung sollten Sie den gleichen Namen verwenden und "full" oder "diff" anfügen.

Beispiel:

`BACKUP DATABASE Customer TO DISK = '\\xxx.xxx.xxx.xxx\backups\CustomerFull';`

`BACKUP DATABASE Customer TO DISK = '\\xxx.xxx.xxx.xxx\backups\CustomerDiff' WITH DIFFERENTIAL;`

## <a name="permissions"></a>Berechtigungen

Erfordert die **BACKUP DATABASE**-Berechtigung oder die Mitgliedschaft in der festen Datenbankrolle **db_backupoperator**. Die Masterdatenbank kann nur von einem normalen Benutzer gesichert werden, der der festen Datenbankrolle **Db_backupoperator** hinzugefügt wurde. Die Masterdatenbank kann nur durch **sa**, den Fabricadministrator, oder Mitglieder der festen Serverrolle **Sysadmin** gesichert werden.

Erfordert ein Windows-Konto mit der Berechtigung, auf das Sicherungsverzeichnis zuzugreifen, es zu erstellen und auf es zu schreiben. Sie müssen außerdem den Windows-Kontonamen und das Kennwort in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] speichern. Verwenden Sie die gespeicherte Prozedur [sp_pdw_add_network_credentials – [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md), um diese Netzwerkanmeldeinformationen zu [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] hinzuzufügen.

Weitere Informationen zum Verwalten von Anmeldeinformationen in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] finden Sie im Abschnitt [Sicherheit](#Security).

## <a name="error-handling"></a>Fehlerbehandlung

BACKUP DATABASE-Fehler unter den folgenden Bedingungen:

- Benutzerberechtigungen sind nicht ausreichend, um eine Sicherung auszuführen.
- [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] verfügt nicht über die erforderlichen Berechtigungen für den Netzwerkspeicherort, an dem die Sicherungsdateien gespeichert werden.
- Die Datenbank ist nicht vorhanden.
- Das Zielverzeichnis ist bereits auf der Netzwerkfreigabe vorhanden.
- Die Zielnetzwerkfreigabe ist nicht verfügbar.
- Der Speicherplatz auf der Ziel-Netzwerkfreigabe ist nicht ausreichend für die Sicherung. Der BACKUP DATABASE-Befehl bestätigt nicht, dass genügend freier Speicherplatz vorhanden ist, bevor die Sicherung initiiert wird, wodurch bei Ausführen von BACKUP DATABASE ein Fehler wegen unzureichendem Speicherplatz generiert werden kann. Wenn nicht genügend Speicherplatz vorhanden ist, führt [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ein Rollback für den Befehl BACKUP DATABASE aus. Führen Sie [DBCC SHRINKLOG ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) aus, um Ihre Datenbank zu verkleinern.
- Versuchen Sie, eine Sicherung innerhalb einer Transaktion zu starten.

::: moniker-end
::: moniker range=">=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017"
## <a name="general-remarks"></a>Allgemeine Hinweise

Verwenden Sie vor dem Ausführen einer Datenbanksicherung [DBCC SHRINKLOG ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) zum Verkleinern der Datenbank.

Eine [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Sicherung wird als Satz von mehreren Dateien im gleichen Verzeichnis gespeichert.

Eine differenzielle Sicherung nimmt normalerweise weniger Zeit in Anspruch als eine vollständige Sicherung und kann häufiger ausgeführt werden. Wenn mehrere differenzielle Sicherungen auf der gleichen vollständigen Sicherung basieren, enthält jede differenzielle Sicherung alle Änderungen aus der vorherigen differenziellen Sicherung.

Wenn Sie einen BACKUP-Befehl abbrechen, entfernt [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] das Zielverzeichnis und alle Dateien, die für die Sicherung erstellt wurden. Wenn [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] die Netzwerkkonnektivität mit der Freigabe verliert, kann das Rollback nicht abgeschlossen werden.

Vollständige und differenzielle Sicherungen werden in separaten Verzeichnissen gespeichert. Benennungskonventionen werden nicht erzwungen, um anzugeben, dass eine vollständige Sicherung und eine differenzielle Sicherung zusammengehören. Sie können dies über Ihre eigenen Benennungskonventionen nachverfolgen. Alternativ können Sie dies nachverfolgen, indem Sie mithilfe der WITH DESCRIPTION-Option eine Beschreibung hinzufügen und diese dann mithilfe der RESTORE HEADERONLY-Anweisung abrufen.

::: moniker-end
::: moniker range=">=aps-pdw-2016"
## <a name="limitations-and-restrictions"></a>Einschränkungen

Sie können keine differenzielle Sicherung der Masterdatenbank ausführen. Nur vollständige Sicherungen der Masterdatenbank werden unterstützt.

Die Sicherungsdateien werden in einem Format gespeichert, das nur geeignet ist, um die Sicherung auf einer [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Appliance unter Verwendung der [RESTORE DATABASE - Analytics Platform System](../../t-sql/statements/restore-statements-transact-sql.md)-Anweisung wiederherzustellen.

Die Sicherung mit der BACKUP DATABASE-Anweisung kann nicht verwendet werden, um Daten oder Benutzerinformationen an SMP-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbanken zu übertragen. Hierfür können Sie die Remotetabellenkopie-Features verwenden. Weitere Informationen finden Sie unter „Remotetabellenkopie“ in der [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] verwendet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Technologie zum Sichern und Wiederherstellen von Datenbanken. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sicherungsoptionen sind vorkonfiguriert, um die Komprimierung von Sicherungen zu verwenden. Sie können keine Sicherungsoptionen wie Komprimierung, Prüfsumme, Blockgröße und Pufferanzahl festlegen.

Nur eine Datenbanksicherung oder -wiederherstellung kann zu einem beliebigen Zeitpunkt auf der Appliance ausgeführt werden. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] stellt Sicherungs- oder Wiederherstellungsbefehle in die Warteschlange, bis der aktuelle Sicherungs-oder Wiederherstellungsbefehl abgeschlossen ist.

Die Zielappliance für die Wiederherstellung der Sicherung muss mindestens so viele Computeknoten besitzen wie die Quellappliance. Die Zielappliance kann über mehr Computeknoten verfügen als die Quellappliance, darf aber nicht weniger Computeknoten besitzen.

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] verfolgt Speicherort und Namen von Sicherungen nicht, da die Sicherungen nicht auf der Appliance gespeichert werden.

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] verfolgt den Erfolg oder Misserfolg von Datenbanksicherungen nicht.

Eine differenzielle Sicherung ist nur zulässig, wenn die letzte vollständige Sicherung erfolgreich abgeschlossen wurde. Nehmen wir beispielsweise an, dass Sie am Montag eine vollständige Sicherung der Sales-Datenbank erstellen und die Sicherung erfolgreich abgeschlossen wird. Am Dienstag erstellen Sie dann eine vollständige Sicherung der Sales-Datenbank, die fehlschlägt. Nach diesem Fehler können Sie dann keine auf der vollständigen Sicherung vom Montag basierende differenzielle Sicherung erstellen. Sie müssen eine vollständige Sicherung erstellen, bevor Sie eine differenzielle Sicherung erstellen.

## <a name="metadata"></a>Metadaten

Diese dynamischen Verwaltungssichten enthalten Informationen über alle Sicherungs-, Wiederherstellungs- und Ladevorgänge. Die Informationen persistieren über Systemneustarts.

- [sys.pdw_loader_backup_runs](../../relational-databases/system-catalog-views/sys-pdw-loader-backup-runs-transact-sql.md)
- [sys.pdw_loader_backup_run_details](../../relational-databases/system-catalog-views/sys-pdw-loader-backup-run-details-transact-sql.md)
- [sys.pdw_loader_run_stages](../../relational-databases/system-catalog-views/sys-pdw-loader-run-stages-transact-sql.md)

## <a name="performance"></a>Leistung

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] führt eine Sicherung aus, indem zuerst die Metadaten gesichert werden und dann eine parallele Sicherung der auf den Computeknoten gespeicherten Datenbankdaten ausgeführt wird. Daten werden direkt aus jedem Computeknoten in das Sicherungsverzeichnis kopiert. Um die bestmögliche Leistung für das Verschieben von Daten von den Computeknoten in das Sicherungsverzeichnis zu erzielen, steuert [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] die Anzahl von Computeknoten, die gleichzeitig Daten kopieren.

## <a name="locking"></a>Sperren

Das DATABASE-Objekt wird mit einer ExclusiveUpdate-Sperre gesperrt.

## <a name="security"></a><a name="Security"></a> Sicherheit

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Sicherungen werden nicht auf dem Gerät gespeichert. Daher ist Ihr IT-Team zuständig für das Verwalten aller Aspekte der Sicherheit von Sicherungen. Dazu gehört beispielsweise die Verwaltung der Sicherheit der Sicherungsdaten, der Sicherheit der Server, auf denen Sicherungen gespeichert werden, und der Sicherheit der Netzwerkinfrastruktur, die die Sicherungsserver mit der [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Appliance verbinden.

**Verwalten von Anmeldeinformationen für das Netzwerk**

Der Netzwerkzugriff auf das Sicherungsverzeichnis basiert auf der Datei für die Standardfreigabesicherheit des Betriebssystems. Vor dem Ausführen einer Sicherung müssen Sie ein Windows-Konto erstellen oder reservieren, das für die Authentifizierung von [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] beim Sicherungsverzeichnis verwendet wird. Dieses Windows-Konto benötigt die Berechtigung, auf das Sicherungsverzeichnis zuzugreifen, es zu erstellen und auf es zu schreiben.

> [!IMPORTANT]
> Um Sicherheitsrisiken in Verbindung mit Ihren Daten zu reduzieren, empfehlen wir, dass Sie ein Windows-Konto ausschließlich zum Ausführen der Sicherungs- und Wiederherstellungsvorgänge festlegen. Definieren Sie das Konto so, dass es nur Berechtigungen für den Sicherungsspeicherort besitzt.

Sie müssen den Benutzernamen und das Kennwort in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] speichern, indem Sie die gespeicherte Prozedur [sp_pdw_add_network_credentials – [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md) ausführen. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] verwendet die Windows-Anmeldeinformationsverwaltung zum Speichern und Verschlüsseln von Benutzernamen und Kennwörtern auf dem Steuerknoten und den Computeknoten. Die Anmeldeinformationen werden nicht mit dem Befehl BACKUP DATABASE gesichert.

Mit [sp_pdw_remove_network_credentials – [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md) können Sie Netzwerkanmeldeinformationen aus [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] entfernen.

Um alle in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] gespeicherten Netzwerkanmeldeinformationen aufzulisten, verwenden Sie die dynamische Verwaltungssicht [sys.dm_pdw_network_credentials](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md).

## <a name="examples"></a>Beispiele

### <a name="a-add-network-credentials-for-the-backup-location"></a>A. Fügen Sie der Netzwerkanmeldeinformationen für den Sicherungsspeicherort hinzu

Zum Erstellen einer Sicherung [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] benötigen Sie die Lese-/Schreibberechtigung für das Sicherungsverzeichnis. Im folgenden Beispiel wird gezeigt, wie die Anmeldeinformationen für einen Benutzer hinzugefügt werden. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] speichert die Anmeldeinformationen und verwendet sie für Sicherungs- und Wiederherstellungsvorgänge.

> [!IMPORTANT]
> Aus Sicherheitsgründen wird empfohlen, mindestens ein Domänenkonto ausschließlich zum Zweck der Durchführung von Sicherungen zu erstellen.

```sql
EXEC sp_pdw_add_network_credentials 'xxx.xxx.xxx.xxx', 'domain1\backupuser', '*****';
```

### <a name="b-remove-network-credentials-for-the-backup-location"></a>B. Entfernen Sie Netzwerkanmeldeinformationen für den Sicherungsspeicherort

Im folgenden Beispiel wird gezeigt, wie die Anmeldeinformationen für ein Domänenbenutzerkonto aus [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] entfernt werden.

```sql
EXEC sp_pdw_remove_network_credentials 'xxx.xxx.xxx.xxx';
```

### <a name="c-create-a-full-backup-of-a-user-database"></a>C. Erstellen Sie eine vollständige Sicherung jeder Benutzerdatenbank

Im folgenden Beispiel wird eine vollständige Sicherung der Invoices-Benutzerdatenbank erstellt. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] erstellt das Verzeichnis Invoices2013 und speichert die Sicherungsdateien im Verzeichnis \\\10.192.63.147\backups\yearly\Invoices2013Full.

```sql
BACKUP DATABASE Invoices TO DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full';
```

### <a name="d-create-a-differential-backup-of-a-user-database"></a>D: Erstellen Sie eine differenzielle Sicherung einer Benutzerdatenbank

Das folgende Beispiel erstellt eine differenzielle Sicherung, die alle Änderungen seit der letzten vollständigen Sicherung der Invoices-Datenbank enthält. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] erstellt das Verzeichnis \\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff, in dem die Dateien gespeichert werden. Die Beschreibung "Invoices 2013 differential backup" wird mit den Headerinformationen für die Sicherung gespeichert.

Die differenzielle Sicherung wird nur dann erfolgreich ausgeführt, wenn die letzte vollständige Sicherung von Rechnungen erfolgreich abgeschlossen wurde.

```sql
BACKUP DATABASE Invoices TO DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff'
    WITH DIFFERENTIAL,
    DESCRIPTION = 'Invoices 2013 differential backup';
```

### <a name="e-create-a-full-backup-of-the-master-database"></a>E. Erstellen Sie eine vollständige Sicherung der Masterdatenbank

Im folgenden Beispiel wird eine vollständige Sicherung der Masterdatenbank erstellt und im Verzeichnis \\\10.192.63.147\backups\2013\daily\20130722\master gespeichert.

```sql
BACKUP DATABASE master TO DISK = '\\xxx.xxx.xxx.xxx\backups\2013\daily\20130722\master';
```

### <a name="f-create-a-backup-of-appliance-login-information"></a>F. Erstellen Sie eine Sicherung der Appliance-Anmeldeinformationen.

Die Masterdatenbank speichert die Anmeldeinformationen für die Appliance. Um die Anmeldeinformationen für die Appliance zu sichern, müssen Sie die Masterdatenbank sichern.

Im folgenden Beispiel wird eine vollständige Sicherung der Masterdatenbank erstellt.

```sql
BACKUP DATABASE master TO DISK = '\\xxx.xxx.xxx.xxx\backups\2013\daily\20130722\master'
WITH (
    DESCRIPTION = 'Master Backup 20130722',
    NAME = 'login-backup'
)
;
```

## <a name="see-also"></a>Weitere Informationen

[RESTORE DATABASE (Parallel Data Warehouse)](../../t-sql/statements/restore-statements-transact-sql.md)

::: moniker-end
