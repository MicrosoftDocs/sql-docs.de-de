---
title: CREATE DATABASE (Transact-SQL) | Microsoft-Dokumentation
description: Erstellen von Datenbanksyntax für SQL Server, Azure SQL-Datenbank, Azure Synapse Analytics und Analytics Platform System
ms.custom: references_regions
ms.date: 12/11/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASE_TSQL
- DATABASE
- CONTAINMENT_TSQL
- CREATE DATABASE
- CREATE_DATABASE_TSQL
- CONTAINS_FILESTREAM_TSQL
- CONTAINS FILESTREAM
- CONTAINMENT
dev_langs:
- TSQL
helpviewer_keywords:
- snapshots [SQL Server database snapshots], creating
- databases [SQL Server], creating
- model database [SQL Server], database creation
- mounted drives [SQL Server]
- CREATE DATABASE
- CREATE DATABASE statement
- file creation [SQL Server]
- creating databases
- containment
- filegroups [SQL Server], database creation
- database creation [SQL Server], CREATE DATABASE statement
- moving databases
- attaching databases [SQL Server], CREATE DATABASE...FOR ATTACH
ms.assetid: 29ddac46-7a0f-4151-bd94-75c1908c89f8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-current||=azuresqldb-mi-current||=azure-sqldw-latest||>=aps-pdw-2016'
ms.openlocfilehash: 509e4fc7445371c57d0fed7208d556e53aa18fe8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063035"
---
# <a name="create-database"></a>CREATE DATABASE

Erstellt eine neue Datenbank.

Klicken Sie auf eine der folgenden Registerkarten, um Syntax, Argumente, Hinweise, Berechtigungen und Beispiele für eine bestimmte SQL-Version anzuzeigen, mit der Sie arbeiten.

Weitere Informationen zu Syntaxkonventionen finden Sie unter [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL-Datenbank](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="overview"></a>Übersicht

In SQL Server erstellt diese Anweisung eine neue Datenbank sowie die verwendeten Dateien und deren Dateigruppen. Mit ihr lassen sich auch Datenbankmomentaufnahmen erstellen oder Datenbankdateien anfügen, um eine Datenbank aus den getrennten Dateien einer anderen Datenbank zu erstellen.

## <a name="syntax"></a>Syntax

Erstellen einer Datenbank

```syntaxsql
CREATE DATABASE database_name
[ CONTAINMENT = { NONE | PARTIAL } ]
[ ON
      [ PRIMARY ] <filespec> [ ,...n ]
      [ , <filegroup> [ ,...n ] ]
      [ LOG ON <filespec> [ ,...n ] ]
]
[ COLLATE collation_name ]
[ WITH <option> [,...n ] ]
[;]

<option> ::=
{
      FILESTREAM ( <filestream_option> [,...n ] )
    | DEFAULT_FULLTEXT_LANGUAGE = { lcid | language_name | language_alias }
    | DEFAULT_LANGUAGE = { lcid | language_name | language_alias }
    | NESTED_TRIGGERS = { OFF | ON }
    | TRANSFORM_NOISE_WORDS = { OFF | ON}
    | TWO_DIGIT_YEAR_CUTOFF = <two_digit_year_cutoff>
    | DB_CHAINING { OFF | ON }
    | TRUSTWORTHY { OFF | ON }
    | PERSISTENT_LOG_BUFFER=ON ( DIRECTORY_NAME='<Filepath to folder on DAX formatted volume>' )
}

<filestream_option> ::=
{
      NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL }
    | DIRECTORY_NAME = 'directory_name'
}

<filespec> ::=
{
(
    NAME = logical_file_name ,
    FILENAME = { 'os_file_name' | 'filestream_path' }
    [ , SIZE = size [ KB | MB | GB | TB ] ]
    [ , MAXSIZE = { max_size [ KB | MB | GB | TB ] | UNLIMITED } ]
    [ , FILEGROWTH = growth_increment [ KB | MB | GB | TB | % ] ]
)
}

<filegroup> ::=
{
FILEGROUP filegroup name [ [ CONTAINS FILESTREAM ] [ DEFAULT ] | CONTAINS MEMORY_OPTIMIZED_DATA ]
    <filespec> [ ,...n ]
}
```

Anfügen einer Datenbank

```syntaxsql
CREATE DATABASE database_name
    ON <filespec> [ ,...n ]
    FOR { { ATTACH [ WITH <attach_database_option> [ , ...n ] ] }
        | ATTACH_REBUILD_LOG }
[;]

<attach_database_option> ::=
{
      <service_broker_option>
    | RESTRICTED_USER
    | FILESTREAM ( DIRECTORY_NAME = { 'directory_name' | NULL } )
}

<service_broker_option> ::=
{
    ENABLE_BROKER
  | NEW_BROKER
  | ERROR_BROKER_CONVERSATIONS
}
```

Erstellen einer Datenbankmomentaufnahme

```syntaxsql
CREATE DATABASE database_snapshot_name
    ON
    (
        NAME = logical_file_name,
        FILENAME = 'os_file_name'
    ) [ ,...n ]
    AS SNAPSHOT OF
[;]
```

## <a name="arguments"></a>Argumente

*database_name* Der Name der neuen Datenbank. Datenbanknamen müssen innerhalb einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eindeutig sein und den Regeln für [Bezeichnern](../../relational-databases/databases/database-identifiers.md) entsprechen.

*database_name* darf maximal 128 Zeichen lang sein, wenn kein logischer Name für die Protokolldatei angegeben wurde. Wenn kein logischer Name angegeben ist, generiert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Werte *logical_file_name* und *os_file_name* für das Protokoll durch Anfügen eines Suffixes an *database_name*. Dadurch wird *database_name* auf 123 Zeichen beschränkt, sodass der generierte logische Protokolldateiname nicht länger als 128 Zeichen ist.

Wenn der Datendateiname nicht angegeben ist, verwendet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]*database_name* sowohl als *logical_file_name* als auch als *os_file_name*. Der Standardpfad wird aus der Registrierung abgerufen. Der Standardpfad kann über die **Servereigenschaften (Seite Datenbankeinstellungen)** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] geändert werden. Zum Ändern des Standardpfads muss [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] neu gestartet werden.

CONTAINMENT = { NONE | PARTIAL }

**Gilt für**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher

Gibt den Kapselungsstatus der Datenbank an. NONE = nicht eigenständige Datenbank. PARTIAL = teilweise eigenständige Datenbank.

ON Gibt an, dass die zum Speichern der Datenabschnitte der Datenbank (Datendateien) verwendeten Datenträgerdateien explizit definiert sind. ON ist erforderlich, wenn darauf eine Liste von durch Trennzeichen voneinander getrennter \<filespec>-Elemente folgt, die die Datendateien für die primäre Dateigruppe definieren. Auf die Liste der Dateien in der primären Dateigruppe kann eine optionale Liste durch Trennzeichen voneinander getrennter \<filegroup>-Elemente folgen, die Benutzerdateigruppen und deren Dateien definieren.

PRIMARY gibt an, dass die zugeordnete \<filespec>-Liste die Primärdatei definiert. Die erste Datei, die im \<filespec>-Eintrag in der primären Dateigruppe angegeben ist, wird zur primären Datei. Eine Datenbank kann nur eine primäre Datei haben. Weitere Informationen finden Sie unter [Datenbankdateien und Dateigruppen](../../relational-databases/databases/database-files-and-filegroups.md).

Ist PRIMARY nicht angegeben, wird die erste in der CREATE DATABASE-Anweisung aufgeführte Datei die primäre Datei.

LOG ON Gibt an, dass die zum Speichern des Datenbankprotokolls verwendeten Datenträgerdateien (Protokolldateien) explizit definiert sind. Nach LOG ON folgt eine Liste durch Trennzeichen voneinander getrennter \<filespec>-Elemente, die die Protokolldateien definieren. Wenn LOG ON nicht angegeben ist, wird automatisch eine Protokolldatei erstellt, deren Größe 25 Prozent der Gesamtgröße aller Datendateien für die Datenbank beträgt, oder 512 KB, je nachdem, welcher Wert größer ist. Diese Datei wird am Standard-Protokolldateispeicherort eingefügt. Informationen zu diesem Speicherort finden Sie unter [Anzeigen oder Ändern der Standardspeicherorte für Daten- und Protokolldateien – SSMS](../../database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files.md).

LOG ON kann nicht in einer Datenbankmomentaufnahme angegeben werden.

COLLATE *collation_name* Gibt die Standardsortierung für die Datenbank an. Als Sortierungsname kann entweder der Name einer Windows-Sortierreihenfolge oder ein SQL-Sortierungsname verwendet werden. Wenn keine Sortierung angegeben ist, wird der Datenbank die Standardsortierung der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zugewiesen. In einer Datenbankmomentaufnahme kann kein Sortierungsname angegeben werden.

Mit den Klauseln FOR ATTACH und FOR ATTACH_REBUILD_LOG kann kein Sortierungsname angegeben werden. Informationen zum Ändern der Sortierung einer angefügten Datenbank finden Sie auf dieser [Microsoft-Website](https://go.microsoft.com/fwlink/?linkid=16419&kbid=325335).

Weitere Informationen zu den Namen von Windows- und SQL-Sortierungen finden Sie unter [COLLATE](~/t-sql/statements/collations.md).

> [!NOTE]
> Eigenständige Datenbanken werden anders sortiert als nicht eigenständige Datenbanken. Weitere Informationen finden Sie unter [Enthaltene Datenbanksortierungen](../../relational-databases/databases/contained-database-collations.md).

WITH \<option>
 **\<filestream_option>**

NON_TRANSACTED_ACCESS = { **OFF** | READ_ONLY | FULL } **Gilt für**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.

Gibt die Ebene des nicht transaktionalen FILESTREAM-Zugriffs auf die Datenbank an.

|Wert|BESCHREIBUNG|
|-----------|-----------------|
|OFF|Nicht transaktionaler Zugriff ist deaktiviert.|
|READONLY|FILESTREAM-Daten in dieser Datenbank können von nicht transaktionalen Prozessen gelesen werden.|
|FULL|Der vollständige nicht transaktionale Zugriff auf FILESTREAM-FileTables ist aktiviert.|

DIRECTORY_NAME = \<directory_name>
**Gilt für:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher

Ein Windows-kompatibler Verzeichnisname. Dieser Name sollte für alle Database_Directory-Namen in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz eindeutig sein. Bei Eindeutigkeitsvergleichen wird die Groß-/Kleinschreibung nicht beachtet, unabhängig von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sortiereinstellungen. Diese Option sollte vor dem Erstellen einer FileTable in dieser Datenbank festgelegt werden.

Die folgenden Optionen sind nur zulässig, wenn CONTAINMENT auf PARTIAL festgelegt wurde. Wenn CONTAINMENT auf NONE festgelegt wird, treten Fehler auf.

- **DEFAULT_FULLTEXT_LANGUAGE = \<lcid> | \<language name> | \<language alias>**

  **Gilt für**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher

  Weitere Informationen zu dieser Option finden Sie unter [Konfigurieren der Serverkonfigurationsoption Volltext-Standardsprache](../../database-engine/configure-windows/configure-the-default-full-text-language-server-configuration-option.md).

- **DEFAULT_LANGUAGE = \<lcid> | \<language name> | \<language alias>**

  **Gilt für**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher

  Weitere Informationen zu dieser Option finden Sie unter [Konfigurieren der Serverkonfigurationsoption Standardsprache](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md).

- **NESTED_TRIGGERS = { OFF | ON}**

  **Gilt für**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher

  Weitere Informationen zu dieser Option finden Sie unter [Konfigurieren der Serverkonfigurationsoption Geschachtelte Trigger](../../database-engine/configure-windows/configure-the-nested-triggers-server-configuration-option.md).

- **TRANSFORM_NOISE_WORDS = { OFF | ON}**

  **Gilt für**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher

  Weitere Informationen zu dieser Option finden Sie unter [Füllwörtertransformation (Serverkonfigurationsoption)](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md).

- **TWO_DIGIT_YEAR_CUTOFF = { 2049 | \<any year between 1753 and 9999> }**

  Vier Ziffern, die ein Jahr darstellen. Der Standardwert lautet 2049. Weitere Informationen zu dieser Option finden Sie unter [Konfigurieren der Serverkonfigurationsoption „Umstellungsjahr für Angaben mit zwei Ziffern“](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).

- **DB_CHAINING { OFF | ON }**

  Wenn ON angegeben wird, kann die Datenbank Quelle oder Ziel einer datenbankübergreifenden Besitzverkettung sein.

  Wenn OFF festgelegt ist, darf die Datenbank nicht Teil einer datenbankübergreifenden Besitzverkettung sein. Der Standardwert ist OFF.

  > [!IMPORTANT]
  > Die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erkennt diese Einstellung, wenn die Datenbankübergreifende Besitzverkettung-Serveroption deaktiviert (0 bzw. OFF) ist. Wenn für Datenbankübergreifende Besitzverkettung der Wert 1 (ON) festgelegt ist, können alle Benutzerdatenbanken unabhängig vom Wert dieser Option Teile von datenbankübergreifenden Besitzketten sein. Diese Option wird mit [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) festgelegt.

  Sie müssen Mitglied der festen Serverrolle sysadmin sein, um diese Option festlegen zu können. Die Option DB_CHAINING kann auf folgenden Systemdatenbanken nicht festgelegt werden: master, model, tempdb.

- **TRUSTWORTHY { OFF | ON }**

  Wenn ON angegeben wird, können Datenbankmodule (z. B. Sichten, benutzerdefinierte Funktionen oder gespeicherte Prozeduren), die den Identitätswechselkontext verwenden, auf Ressourcen außerhalb der Datenbank zugreifen.

  Wenn OFF angegeben wird, können Datenbankmodule in einem Identitätswechselkontext nicht auf Ressourcen außerhalb der Datenbank zugreifen. Der Standardwert ist OFF.

  TRUSTWORTHY wird auf OFF festgelegt, wenn die Datenbank angefügt wird.

  Standardmäßig ist TRUSTWORTHY für alle Systemdatenbanken mit Ausnahme der msdb-Datenbank auf OFF festgelegt. Für die model-Datenbank und für die tempdb-Datenbank kann der Wert nicht geändert werden. Für die master-Datenbank sollten Sie die Option TRUSTWORTHY niemals auf ON festlegen.

- **PERSISTENT_LOG_BUFFER=ON ( DIRECTORY_NAME='' )**

  Durch Festlegen dieser Option wird der Transaktionsprotokollpuffer auf einem Volume erstellt, das sich auf einem Laufwerk befindet, welches durch Speicherklassenspeicher (NVDIMM-N permanenter Speicher) gesichert ist – auch bekannt als persistenter Protokollpuffer. Weitere Informationen finden Sie unter [Transaction Commit latency acceleration using Storage Class Memory](/archive/blogs/sqlserverstorageengine/transaction-commit-latency-acceleration-using-storage-class-memory-in-windows-server-2016sql-server-2016-sp1) (Beschleunigung der Transaktionscommitlatenz mit Speicherklassenspeicher). **Gilt für**: [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] und neuere Versionen.

FOR ATTACH [ WITH \< attach_database_option > ] gibt an, dass die Datenbank durch [Anfügen](../../relational-databases/databases/database-detach-and-attach-sql-server.md) von vorhandenen Betriebssystemdateien erstellt wird. Es muss ein \<filespec>-Eintrag vorhanden sein, der die erste primäre Datei angibt. Darüber hinaus werden nur \<filespec>-Einträge für die Dateien benötigt, deren Pfad sich seit dem Erstellen oder letzten Anhängen der Datenbank geändert hat. Für diese Dateien muss ein \<filespec>-Eintrag angegeben werden.

Für FOR ATTACH ist Folgendes erforderlich:

- Alle Datendateien (MDF und NDF) müssen verfügbar sein.
- Wenn mehrere Protokolldateien vorhanden sind, müssen alle verfügbar sein.

Wenn eine Datenbank im Lese-/Schreibmodus eine einzige Protokolldatei hat, die derzeit nicht verfügbar ist, und wenn die Datenbank vor dem Anfügen heruntergefahren wurde und keine Benutzer oder offene Transaktionen vorhanden sind, dann wird mit FOR ATTACH automatisch die Protokolldatei neu erstellt und die primäre Datei aktualisiert. Im Gegensatz dazu kann für eine schreibgeschützte Datenbank das Protokoll nicht neu erstellt werden, da das Hochladen der primären Datei nicht möglich ist. Deshalb müssen Sie beim Anfügen einer schreibgeschützten Datenbank mit einem nicht verfügbaren Protokoll die Protokolldateien oder Dateien in der FOR ATTACH-Klausel angeben.

> [!NOTE]
> Eine Datenbank, die in einer neueren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellt wurde, kann in früheren Versionen nicht angefügt werden.

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden alle Volltextdateien, die zur angefügten Datenbank gehören, mit der Datenbank angefügt. Geben Sie den neuen Speicherort ohne Betriebssystem-Dateinamen der Volltextdatei an, um einen neuen Pfad für den Volltextkatalog anzugeben. Weitere Informationen finden Sie im Abschnitt „Beispiele“.

Wenn Sie einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz eine Datenbank anfügen, die eine FILESTREAM-Option von Verzeichnisnamen enthält, überprüft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ob der Database_Directory-Name eindeutig ist. Ist dies nicht der Fall, schlägt der Anfügevorgang mit folgendem Fehler fehl: "FILESTREAM Database_Directory name \<name>name> is not unique in this SQL Server instance." (FILESTREAM Database_Directory-Name <name> ist auf dieser SQL Server-Instanz nicht eindeutig.). Um diesen Fehler zu vermeiden, sollte der optionale Parameter *directory_name* in diesem Vorgang übergeben werden.

FOR ATTACH kann nicht in einer Datenbank-Momentaufnahme angegeben werden.

FOR ATTACH kann die RESTRICTED_USER-Option angeben. RESTRICTED_USER ermöglicht nur Mitgliedern der festen Datenbankrolle db_owner und der festen Serverrollen dbcreator und sysadmin eine Verbindung mit der Datenbank, begrenzt jedoch nicht deren Anzahl. Versuche von nicht qualifizierten Benutzern werden abgelehnt.

Wenn die Datenbank [!INCLUDE[ssSB](../../includes/sssb-md.md)] verwendet, sollten Sie WITH \<service_broker_option> in Ihrer FOR ATTACH-Klausel verwenden:

\<service_broker_option> steuert die [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Nachrichtenübermittlung und den Bezeichner [!INCLUDE[ssSB](../../includes/sssb-md.md)] für die Datenbank. [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Optionen können nur bei Verwendung der FOR ATTACH-Klausel angegeben werden.

ENABLE_BROKER Gibt an, dass [!INCLUDE[ssSB](../../includes/sssb-md.md)] für die angegebene Datenbank aktiviert ist. Das heißt, dass die Nachrichtenübermittlung gestartet und für is_broker_enabled die Einstellung TRUE in der Katalogsicht sys.databases festgelegt wird. Die Datenbank behält den vorhandenen [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Bezeichner bei.

NEW_BROKER Erstellt sowohl in sys.databases als auch in der wiederhergestellten Datenbank einen neuen Wert für service_broker_guid und beendet alle Konversationsendpunkte mit einem Cleanup. Der Broker ist aktiviert, es wird jedoch keine Meldung an die Remote-Konversationsendpunkte gesendet. Jede Route, die auf den alten [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Bezeichner verweist, muss mit dem neuen Bezeichner neu erstellt werden.

ERROR_BROKER_CONVERSATIONS Beendet alle Konversationen mit einem Fehler, der angibt, dass die Datenbank angefügt oder wiederhergestellt wird. Der Broker ist deaktiviert, bis dieser Vorgang abgeschlossen ist, und wird dann aktiviert. Die Datenbank behält den vorhandenen [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Bezeichner bei.

Berücksichtigen Sie Folgendes, wenn Sie eine replizierte Datenbank anfügen, die kopiert statt getrennt wurde:

- Wenn Sie die Datenbank an die gleiche Serverinstanz und -version wie die ursprüngliche Datenbank anfügen, sind keine weiteren Schritte erforderlich.
- Wenn Sie die Datenbank an die gleiche Serverinstanz mit einer aktualisierten Version anfügen, müssen Sie [sp_vupgrade_replication](../../relational-databases/system-stored-procedures/sp-vupgrade-replication-transact-sql.md) ausführen, um die Replikation zu aktualisieren, nachdem der Anfügevorgang abgeschlossen wurde.
- Wenn Sie die Datenbank an eine andere Serverinstanz unabhängig von der Version anfügen, müssen Sie [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) ausführen, um die Replikation zu entfernen, nachdem der Anfügevorgang abgeschlossen wurde.

> [!NOTE]
> Für das Anfügen wird das **vardecimal**-Speicherformat verwendet, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] muss jedoch mindestens auf [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] SP2 aktualisiert werden. Sie können keine Datenbank mit "vardecimal"-Speicherformat an eine frühere Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anfügen. Informationen zum **vardecimal**-Speicherformat finden Sie unter [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md).

Wird eine Datenbank zum ersten Mal an eine neue Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]angefügt oder wiederhergestellt, ist noch keine Kopie des Datenbank-Hauptschlüssels (verschlüsselt vom Diensthauptschlüssel) auf dem Server gespeichert. Der Datenbank-Hauptschlüssel (Database Master Key, DMK) muss mithilfe der Anweisung **OPEN MASTER KEY** entschlüsselt werden. Nachdem der Datenbank-Hauptschlüssel entschlüsselt wurde, können Sie für die Zukunft die automatische Entschlüsselung aktivieren, indem Sie die Anweisung **ALTER MASTER KEY REGENERATE** verwenden. Auf diese Weise können Sie eine Kopie des mit dem Diensthauptschlüssel (Service Master Key, SMK) verschlüsselten Datenbank-Hauptschlüssels für den Server bereitstellen. Wenn eine Datenbank von einer früheren Version aktualisiert wurde, sollte der DMK neu generiert werden, damit er den neueren AES-Algorithmus verwendet. Weitere Informationen zum Neugenerieren des DMK finden Sie unter [ALTER MASTER KEY](../../t-sql/statements/alter-master-key-transact-sql.md). Die zum Neugenerieren des DMK zum Upgrade auf AES erforderliche Zeit hängt von der Anzahl der Objekte ab, die durch den DMK geschützt werden. Der DMK muss nur einmal auf AES aktualisiert und neu generiert werden. Dies hat keine Auswirkungen auf zukünftige Neugenerierungen im Rahmen einer Schlüsselrotationsstrategie. Weitere Informationen zum Upgraden einer Datenbank über Anfügevorgänge finden Sie unter [Aktualisieren einer Datenbank durch Trennen und Anfügen](../../relational-databases/databases/upgrade-a-database-using-detach-and-attach-transact-sql.md).

> [!IMPORTANT]
> Sie sollten keine Datenbanken aus unbekannten oder nicht vertrauenswürdigen Quellen anfügen. Solche Datenbanken können bösartigen Code enthalten, der möglicherweise unbeabsichtigten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Code ausführt oder Fehler verursacht, indem er das Schema oder die physische Datenbankstruktur ändert. Bevor Sie eine Datenbank aus einer unbekannten oder nicht vertrauenswürdigen Quelle verwenden, führen Sie auf einem Nichtproduktionsserver [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) für die Datenbank aus. Überprüfen Sie außerdem den Code in der Datenbank, z.B. gespeicherte Prozeduren oder anderen benutzerdefinierten Code.
> [!NOTE]
> Beim Anfügen einer Datenbank haben die Optionen **TRUSTWORTHY** und **DB_CHAINING** keine Auswirkung.

FOR ATTACH_REBUILD_LOG Gibt an, dass die Datenbank durch Anfügen eines vorhandenen Satzes von Betriebssystemdateien erstellt wird. Diese Option ist auf Datenbanken mit Lese-/Schreibzugriff beschränkt. Es muss ein *\<filespec>* -Eintrag vorhanden sein, der die primäre Datei angibt. Wenn eines oder mehrere Transaktionsprotokolle fehlen, wird das Protokoll neu erstellt. ATTACH_REBUILD_LOG erstellt automatisch eine neue 1-MB-Protokolldatei. Diese Datei wird am Standard-Protokolldateispeicherort eingefügt. Informationen zu diesem Speicherort finden Sie unter [Anzeigen oder Ändern der Standardspeicherorte für Daten- und Protokolldateien – SSMS](../../database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files.md).

> [!NOTE]
> Wenn die Protokolldateien verfügbar sind, verwendet [!INCLUDE[ssDE](../../includes/ssde-md.md)] diese Dateien und erstellt nicht die Protokolldateien neu.

Für FOR ATTACH_REBUILD_LOG ist Folgendes erforderlich:

- Ein fehlerfreies Herunterfahren der Datenbank.
- Alle Datendateien (MDF und NDF) müssen verfügbar sein.

> [!IMPORTANT]
> Mit diesem Vorgang wird die Protokollsicherungskette unterbrochen. Wir empfehlen, nach Abschluss dieses Vorgangs eine vollständige Datenbanksicherung auszuführen. Weitere Informationen finden Sie unter [SICHERUNG](../../t-sql/statements/backup-transact-sql.md).

In der Regel wird FOR ATTACH_REBUILD_LOG verwendet, wenn Sie eine Datenbank mit Lese-/Schreibzugriff mit einem großen Protokoll auf einen anderen Server kopieren, auf dem die Kopie hauptsächlich oder ausschließlich für Lesevorgänge verwendet wird und deshalb weniger Speicherplatz für das Protokoll benötigt wird, als bei der ursprünglichen Datenbank.

FOR ATTACH_REBUILD_LOG kann nicht auf einer Datenbank-Momentaufnahme angegeben werden.

Weitere Informationen zum Anfügen und Trennen von Datenbanken finden Sie unter [Anfügen und Trennen von Datenbanken](../../relational-databases/databases/database-detach-and-attach-sql-server.md).

\<filespec> steuert die Dateieigenschaften.

NAME *logical_file_name* Gibt den logischen Namen der Datei an. NAME ist erforderlich, wenn FILENAME angegeben wird, dies gilt jedoch nicht, wenn eine der FOR ATTACH-Klauseln angegeben wird. Einer FILESTREAM-Dateigruppe kann der Name PRIMARY nicht zugewiesen werden.

*logical_file_name* Der logische Dateiname, der in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beim Verweis auf die Datei verwendet wird. *Logical_file_name* muss in der Datenbank eindeutig sein und den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen. Der Name kann eine Zeichen- oder Unicode-Konstante oder ein regulärer oder Begrenzungsbezeichner sein.

FILENAME { **'** _os\_file\_name_ **'** | **'** _filestream\_path_ **'** } Gibt den (physischen) Betriebssystem-Dateinamen an.

**'** *os_file_name* **'** Der Pfad und der Dateiname, die vom Betriebssystem beim Erstellen der Datei verwendet werden. Die Datei muss sich auf einem der folgenden Geräten bzw. Netzwerken befinden: auf dem lokalen Server, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert ist, in einem SAN (Storage Area Network) oder in einem Netzwerk auf iSCSI-Basis. Der angegebene Pfad muss bereits vorhanden sein, bevor die CREATE DATABASE-Anweisung ausgeführt wird. Weitere Informationen finden Sie im Abschnitt mit Hinweisen unter "Datenbankdateien und Dateigruppen".

Die Parameter SIZE, MAXSIZE und FILEGROWTH können festgelegt werden, wenn ein UNC-Pfad für die Datei angegeben wird.

Wenn sich die Datei auf einer Rawpartition befindet, darf *os_file_name* nur den Laufwerkbuchstaben einer vorhandenen Rawpartition angeben. Auf einer Rawpartition kann nur eine einzige Datendatei erstellt werden.

Datendateien sollten nicht in komprimierten Dateisystemen abgelegt werden, es sei denn, alle Dateien sind schreibgeschützte sekundäre Dateien oder die Datenbank ist schreibgeschützt. Protokolldateien sollten niemals in komprimierten Dateisystemen abgelegt werden.

**'** *filestream_pfad* **'** Bei einer FILESTREAM-Dateigruppe verweist FILENAME auf einen Pfad, unter dem FILESTREAM-Daten gespeichert werden. Der Pfad muss bis zum letzten Ordner vorhanden sein, und der letzte Ordner darf nicht vorhanden sein. Wenn Sie z. B. den Pfad C:\MyFiles\MyFilestreamData angeben, muss C:\MyFiles vor der Ausführung von ALTER DATABASE vorhanden sein, der Ordner MyFilestreamData muss jedoch noch nicht existieren.

Die Dateigruppe und die Datei (`<filespec>`) müssen in derselben Anweisung erstellt werden.

Die Eigenschaften SIZE und FILEGROWTH gelten nicht für eine FILESTREAM-Dateigruppe.

SIZE *size* Gibt die Größe der Datei an.

SIZE kann nicht angegeben werden, wenn *os_file_name* als UNC-Pfad angegeben ist. SIZE gilt nicht für eine FILESTREAM-Dateigruppe.

*size* Die Anfangsgröße der Datei.

Wenn *size* für die primäre Datei nicht angegeben wird, verwendet [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Größe der primären Datei in der Modelldatenbank. Die Standardgröße von Modellen beträgt 8 MB (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) oder 1 MB (bei früheren Versionen). Wenn eine sekundäre Datendatei oder eine Protokolldatei angegeben wird, *size* für die Datei jedoch nicht angegeben wird, legt [!INCLUDE[ssDE](../../includes/ssde-md.md)] die Größe der Datei auf 8 MB (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) oder 1 MB (für frühere Versionen) fest. Die für die primäre Datei angegebene Größe muss mindestens der Größe der primären Datei der model-Datenbank entsprechen.

Es kann das Suffix Kilobyte (KB), Megabyte (MB), Gigabyte (GB) oder Terabyte (TB) verwendet werden. Die Standardeinheit ist MB. Geben Sie eine ganze Zahl (ohne Dezimalstellen) an. *Size* ist ein ganzzahliger Wert. Verwenden Sie für Werte größer als 2.147.483.647 größere Einheiten.

MAXSIZE *max_size* Gibt die maximale Größe an, auf die die Datei vergrößert werden kann. MAXSIZE kann nicht angegeben werden, wenn *os_file_name* als UNC-Pfad angegeben wird.

*max_size* Die maximale Dateigröße. Die Suffixe KB, MB, GB und TB können verwendet werden. Die Standardeinheit ist MB. Geben Sie eine ganze Zahl (ohne Dezimalstellen) an. Wenn *max_size* nicht angegeben ist, kann die Dateigröße so lange zunehmen, bis der Speicherplatz auf dem Datenträger erschöpft ist. *Max_size* ist ein ganzzahliger Wert. Verwenden Sie für Werte größer als 2.147.483.647 größere Einheiten.

UNLIMITED Gibt an, dass die Größe der Datei so lange zunehmen kann, bis auf dem Datenträger kein Speicherplatz mehr verfügbar ist. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gilt für eine Protokolldatei, für die keine Größenbeschränkung festgelegt ist, eine Maximalgröße von 2 TB und für eine Datendatei eine Maximalgröße von 16 TB.

> [!NOTE]
> Wenn diese Option für einen FILESTREAM-Container angegeben wird, gilt keine Maximalgröße. Die Dateigröße erhöht sich so lange, bis der Datenträger voll ist.

FILEGROWTH *growth_increment* Gibt das automatische Vergrößerungsinkrement der Datei an. Die FILEGROWTH-Einstellung für eine Datei darf die MAXSIZE-Einstellung nicht überschreiten. FILEGROWTH kann nicht angegeben werden, wenn *os_file_name* als UNC-Pfad angegeben wird. FILEGROWTH gilt nicht für eine FILESTREAM-Dateigruppe.

*growth_increment* Die Menge an Speicherplatz, die der Datei hinzugefügt wird, wenn neuer Speicherplatz erforderlich wird.

Der Wert kann in MB, KB, GB, TB oder Prozent (%) angegeben werden. Bei Zahlen ohne Angabe von MB, KB oder % wird standardmäßig MB verwendet. Wenn der Wert in Prozent angegeben wird, ist die growth_increment-Größe der angegebene Prozentsatz der Dateigröße zum Zeitpunkt der Vergrößerung. Die angegebene Größe wird auf den nächsten durch 64 KB teilbaren Wert gerundet. Der Mindestwert beträgt 64 KB.

Der Wert 0 zeigt an, dass die automatische Vergrößerung deaktiviert ist und kein zusätzlicher Platz zulässig ist.

Wenn FILEGROWTH nicht angegeben ist, lauten die Standardwerte wie folgt:

|Version|Standardwerte|
|-------------|--------------------|
|Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|Daten: 64 MB, Protokolldateien: 64 MB|
|Ab [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|Daten: 1 MB, Protokolldateien: 10 %|
|Vor [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|Daten: 10 %, Protokolldateien: 10 %|

\<filegroup> steuert die Dateigruppeneigenschaften. Kann nicht in einer Datenbankmomentaufnahme angegeben werden.

FILEGROUP *filegroup_name* Der logische Name der Dateigruppe.

*filegroup_name*
*filegroup_name* muss innerhalb der Datenbank eindeutig sein und darf nicht den vom System bereitgestellten Namen PRIMARY bzw. PRIMARY_LOG tragen. Der Name kann eine Zeichen- oder Unicode-Konstante oder ein regulärer oder Begrenzungsbezeichner sein. Der Name muss den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen.

CONTAINS FILESTREAM Gibt an, dass die Dateigruppe FILESTREAM-Blobs (Binary Large Objects) im Dateisystem speichert.

CONTAINS MEMORY_OPTIMIZED_DATA

**Gilt für**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher

Gibt an, dass die Dateigruppe speicheroptimierte Daten im Dateisystem speichert. Weitere Informationen finden Sie unter [In-Memory OLTP – Arbeitsspeicheroptimierung](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md). Nur eine MEMORY_OPTIMIZED_DATA-Dateigruppe ist pro Datenbank zulässig. Codebeispiele, die Dateigruppen erstellen, um speicheroptimierte Daten zu speichern, finden Sie unter [Erstellen einer speicheroptimierten Tabelle und einer nativ kompilierten gespeicherten Prozedur](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md).

DEFAULT Gibt an, dass die benannte Dateigruppe die Standarddateigruppe in der Datenbank ist.

*database_snapshot_name* Der Name der neuen Datenbankmomentaufnahme. Die Namen von Datenbankmomentaufnahmen müssen innerhalb einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eindeutig sein und den Regeln für Bezeichner entsprechen. *database_snapshot_name* darf maximal 128 Zeichen lang sein.

ON **(** NAME **=** _logical\_file\_name_ **,** FILENAME **='** _os\_file\_name_ **')** [ **,** ... *n* ] Gibt für das Erstellen einer Datenbankmomentaufnahme eine Liste von Dateien in der Quelldatenbank an. Damit die Momentaufnahme funktionsfähig ist, müssen alle Datendateien einzeln angegeben werden. Protokolldateien sind jedoch für Datenbankmomentaufnahmen nicht zulässig. FILESTREAM-Dateigruppen werden von Datenbankmomentaufnahmen nicht unterstützt. Wenn eine FILESTREAM-Datendatei in eine CREATE DATABASE ON-Klausel eingeschlossen wird, schlägt die Anweisung fehl, und ein Fehler wird ausgelöst.

Beschreibungen von NAME und FILENAME sowie deren Werte finden Sie in den Beschreibungen der entsprechenden \<filespec>-Werte.

> [!NOTE]
> Wenn Sie eine Datenbankmomentaufnahme erstellen, sind die anderen Optionen für \<filespec> sowie das PRIMARY-Schlüsselwort nicht zulässig.

AS SNAPSHOT OF *source_database_name* Gibt an, dass die erstellte Datenbank eine Datenbankmomentaufnahme der Quelldatenbank ist, die durch *source_database_name* angegeben wird. Die Momentaufnahme- und Quelldatenbank müssen sich auf derselben Instanz befinden.

Weitere Informationen finden Sie im Abschnitt mit Hinweisen unter [Datenbank-Momentaufnahmen](#database-snapshots).

## <a name="remarks"></a>Bemerkungen

Die [Masterdatenbank](../../relational-databases/databases/master-database.md) sollte immer dann gesichert werden, wenn eine Benutzerdatenbank erstellt, geändert oder gelöscht wird.

Die `CREATE DATABASE`-Anweisung muss im Autocommitmodus (dem Standardmodus für die Transaktionsverwaltung) ausgeführt werden und ist in einer expliziten oder impliziten Transaktion nicht zugelassen.

Sie können mit einer `CREATE DATABASE`-Anweisung eine Datenbank und die Dateien erstellen, in denen die Datenbank gespeichert ist. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implimiert die CREATE DATABASE-Anweisung, indem die folgenden Schritte ausgeführt werden:

1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet eine Kopie der [Modelldatenbank](../../relational-databases/databases/model-database.md), um die Datenbank und ihre Metadaten zu initialisieren.
2. Der Datenbank wird eine Service Broker-GUID zugewiesen.
3. Dann füllt das [!INCLUDE[ssDE](../../includes/ssde-md.md)] den Rest der Datenbank mit leeren Seiten auf, mit Ausnahme der Seiten mit internen Daten, in denen aufgezeichnet ist, wie der Speicherplatz in der Datenbank verwendet wird.

Maximal 32.767 Datenbanken können auf einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]angegeben werden.

Jede Datenbank hat einen Besitzer, der besondere Aktivitäten in der Datenbank ausführen kann. Der Besitzer ist der Benutzer, der die Datenbank erstellt. Der Datenbankbesitzer kann mit [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md) geändert werden.

Einige Datenbankfeatures sind von Features oder im Dateisystem enthaltenen Features abhängig, damit eine Datenbank vollständig funktionieren kann. Einige Beispiele von Features, die von verschiedenen Dateisystemfeatures abhängig sind, umfassen:

- DBCC CHECKDB
- FileStream
- Onlinesicherungen über VSS und Dateimomentaufnahmen
- Erstellung der Datenbankmomentaufnahme
- Arbeitsspeicheroptimierte Datendateigruppe

## <a name="database-files-and-filegroups"></a>Datenbankdateien und Dateigruppen

Jede Datenbank verfügt über mindestens zwei Dateien, und zwar einer *primären Datei* und einer *Transaktionsprotokolldatei* sowie über mindestens eine Dateigruppe. Für jede Datenbank können maximal 32.767 Dateien und 32.767 Dateigruppen angegeben werden.

Wenn Sie eine Datenbank erstellen, sollten die Datendateien möglichst groß sein. Orientieren Sie sich dabei an den maximal zu erwartenden Datenmengen, die in der Datenbank gespeichert werden sollen.

Wir empfehlen, dass Sie ein Storage Area Network (SAN), ein Netzwerk auf iSCSI-Basis oder einen lokal zugeordneten Datenträger für die Speicherung Ihrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbankdateien verwenden, da bei dieser Konfiguration die Leistung und Zuverlässigkeit von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] optimiert werden.

## <a name="database-snapshots"></a>Datenbank-Momentaufnahmen

Sie können die `CREATE DATABASE`-Anweisung verwenden, um eine schreibgeschützte statische Sicht (eine *Datenbankmomentaufnahme*) der *Quelldatenbank* zu erstellen. Ein Datenbank-Momentaufnahme ist im Hinblick auf Transaktionen konsistent mit der Quelldatenbank zu dem Zeitpunkt, an dem die Momentaufnahme erstellt wurde. Für eine Quelldatenbank kann es mehrere Momentaufnahmen geben.

> [!NOTE]
> Wenn Sie eine Datenbankmomentaufnahme erstellen, kann die `CREATE DATABASE`-Anweisung nicht auf Protokolldateien, Offlinedateien, Wiederherstellungsdateien und außer Kraft gesetzte Dateien verweisen.

Wenn das Erstellen einer Datenbankmomentaufnahme fehlschlägt, wird der Snapshot fehlerverdächtig und muss gelöscht werden. Weitere Informationen finden Sie unter [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md).

Jede Momentaufnahme wird so lange persistent gespeichert, bis sie mit `DROP DATABASE` gelöscht wird.

Weitere Informationen finden Sie unter [Datenbankmomentaufnahmen](../../relational-databases/databases/database-snapshots-sql-server.md).

## <a name="database-options"></a>Datenbankoptionen

Mehrere Datenbankoptionen werden automatisch festgelegt, wenn Sie eine Datenbank erstellen. Eine Beschreibung dieser Optionen finden Sie unter [ALTER DATABASE SET-Optionen](../../t-sql/statements/alter-database-transact-sql-set-options.md).

## <a name="the-model-database-and-creating-new-databases"></a>Die model-Datenbank und das Erstellen neuer Datenbanken

Alle benutzerdefinierten Objekte in der [Modelldatenbank](../../relational-databases/databases/model-database.md) werden in alle neu erstellten Datenbanken kopiert. Sie können der model-Datenbank beliebige Objekte (z. B. Tabellen, Sichten, gespeicherte Prozeduren, Datentypen usw.) hinzufügen, die in allen neu erstellten Datenbanken enthalten sein sollen.

Wenn eine `CREATE DATABASE <database_name>`-Anweisung ohne zusätzliche Größenparameter angegeben wird, erhält die primäre Datendatei die gleiche Größe wie die primäre Datei in der Modelldatenbank.

Jede neue Datenbank erbt die Einstellungen der Datenbankoptionen von der Modelldatenbank, es sei denn, `FOR ATTACH` ist angegeben. Die Datenbankoption „auto shrink“ ist beispielsweise in der Modelldatenbank und in allen neuen, von Ihnen erstellten Datenbanken auf **TRUE** festgelegt. Wenn Sie die Optionen in der model-Datenbank ändern, werden diese neuen Einstellungen in jeder neu erstellten Datenbank verwendet. Änderungen in der model-Datenbank haben jedoch keine Auswirkungen auf vorhandene Datenbanken. Wenn FOR ATTACH in der CREATE DATABASE-Anweisung angegeben ist, erbt die neue Datenbank die Einstellungen der Datenbankoptionen der ursprünglichen Datenbank.

## <a name="viewing-database-information"></a>Anzeigen von Datenbankinformationen

Sie können Katalogsichten, Systemfunktionen und gespeicherte Systemprozeduren verwenden, um Informationen zu Datenbanken, Dateien und Dateigruppen zurückzugeben. Weitere Informationen finden Sie unter [Systemsichten](../language-reference.md).

## <a name="permissions"></a>Berechtigungen

Erfordert die Berechtigung `CREATE DATABASE`, `CREATE ANY DATABASE` oder `ALTER ANY DATABASE`.

Zur Steuerung der Datenträgernutzung einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]wird die Berechtigung zum Erstellen von Datenbanken in der Regel auf einige wenige Anmeldekonten beschränkt.

Im folgenden Beispiel wird dem Datenbankbenutzer Fay die Berechtigung zum Erstellen einer Datenbank erteilt.

```sql
USE master;
GO
GRANT CREATE DATABASE TO [Fay];
GO
```

### <a name="permissions-on-data-and-log-files"></a>Berechtigungen für Daten und Protokolldateien

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden Berechtigungen für die Daten und Protokolldateien der einzelnen Datenbanken festgelegt. Die folgenden Berechtigungen werden stets festgelegt, wenn die folgenden Vorgänge auf eine Datenbank angewendet werden:

- Angefügt
- Gesichert
- Erstellt
- Getrennt
- Ändern, um eine neue Datei hinzuzufügen
- Wiederherstellen

Durch die Berechtigungen wird verhindert, dass die Dateien versehentlich manipuliert werden, wenn sie sich in einem Verzeichnis mit offenen Berechtigungen befinden.

> [!NOTE]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)] legt keine Berechtigungen für Daten und Protokolldateien fest.

## <a name="examples"></a>Beispiele

### <a name="a-creating-a-database-without-specifying-files"></a>A. Erstellen einer Datenbank ohne Angabe von Dateien

Mit dem folgenden Beispiel werden die Datenbank `mytest` sowie eine entsprechende primäre Datei und Transaktionsprotokolldatei erstellt. Da die Anweisung keine \<filespec>-Elemente enthält, hat die primäre Datenbankdatei die Größe der primären Modelldatenbankdatei. Das Transaktionsprotokoll wird auf den größeren dieser Werte festgelegt: 512 KB oder 25 % der Größe der primären Datendatei. Da MAXSIZE nicht angegeben ist, können die Dateien so lange vergrößert werden, bis der gesamte verfügbare Speicherplatz auf dem Datenträger gefüllt ist. Dieses Beispiel zeigt auch, wie Sie die Datenbank mit dem Namen `mytest`, falls vorhanden, vor dem Erstellen der Datenbank `mytest` löschen.

```sql
USE master;
GO
IF DB_ID (N'mytest') IS NOT NULL
DROP DATABASE mytest;
GO
CREATE DATABASE mytest;
GO
-- Verify the database files and sizes
SELECT name, size, size*1.0/128 AS [Size in MBs]
FROM sys.master_files
WHERE name = N'mytest';
GO
```

### <a name="b-creating-a-database-that-specifies-the-data-and-transaction-log-files"></a>B. Erstellen einer Datenbank mit Angabe der Datendatei und der Transaktionsprotokolldatei

Im folgenden Beispiel wird die Datenbank mit dem Namen `Sales` erstellt. Da das PRIMARY-Schlüsselwort nicht verwendet wird, wird die erste Datei (`Sales_dat`) zur primären Datei. Da im SIZE-Parameter für die Datei `Sales_dat` weder MB noch KB angegeben ist, wird die Einheit MB verwendet und in Megabyte zugeordnet. Die `Sales_log` wird in Megabyte zugeordnet, weil das Suffix `MB` explizit im `SIZE` -Parameter angegeben ist.

```sql
USE master;
GO
CREATE DATABASE Sales
ON
( NAME = Sales_dat,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\saledat.mdf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 )
LOG ON
( NAME = Sales_log,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\salelog.ldf',
    SIZE = 5MB,
    MAXSIZE = 25MB,
    FILEGROWTH = 5MB ) ;
GO
```

### <a name="c-creating-a-database-by-specifying-multiple-data-and-transaction-log-files"></a>C. Erstellen einer Datenbank unter Angabe mehrerer Daten- und Transaktionsprotokolldateien

Mit dem folgenden Beispiel wir die `Archive`-Datenbank erstellt, die über drei Datendateien mit `100-MB` und zwei Transaktionsprotokolldateien mit `100-MB` verfügt. Die primäre Datei ist die erste Datei in der Liste und wird explizit mit dem `PRIMARY`-Schlüsselwort angegeben. Die Transaktionsprotokolldateien werden nach den `LOG ON`-Schlüsselwörtern angegeben. Beachten Sie die Erweiterungen, die für die Dateien in der Option `FILENAME` verwendet werden: `.mdf` wird für primäre Datendateien verwendet, `.ndf` wird für sekundäre Datendateien verwendet, und `.ldf` wird für Transaktionsprotokolldateien verwendet. In diesem Beispiel wird die Datenbank auf dem Laufwerk `D:` abgelegt, anstatt an demselben Speicherort wie die `master`-Datenbank.

```sql
USE master;
GO
CREATE DATABASE Archive
ON
PRIMARY
    (NAME = Arch1,
    FILENAME = 'D:\SalesData\archdat1.mdf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
    ( NAME = Arch2,
    FILENAME = 'D:\SalesData\archdat2.ndf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
    ( NAME = Arch3,
    FILENAME = 'D:\SalesData\archdat3.ndf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20)
LOG ON
  (NAME = Archlog1,
    FILENAME = 'D:\SalesData\archlog1.ldf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
  (NAME = Archlog2,
    FILENAME = 'D:\SalesData\archlog2.ldf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20) ;
GO
```

### <a name="d-creating-a-database-that-has-filegroups"></a>D: Erstellen einer Datenbank mit Dateigruppen

Im folgenden Beispiel wird die `Sales`-Datenbank erstellt, die über folgende Dateigruppen verfügt:

- Die primäre Dateigruppe mit den Dateien `Spri1_dat` und `Spri2_dat`. Die FILEGROWTH-Inkremente für diese Dateien werden mit `15%` angegeben.
- Eine Dateigruppe mit dem Namen `SalesGroup1` mit den Dateien `SGrp1Fi1` und `SGrp1Fi2`.
- Eine Dateigruppe mit dem Namen `SalesGroup2` mit den Dateien `SGrp2Fi1` und `SGrp2Fi2`.

In diesem Beispiel werden die Daten und Protokolldateien auf verschiedenen Datenträgern angeordnet, um die Leistung zu verbessern.

```sql
USE master;
GO
CREATE DATABASE Sales
ON PRIMARY
( NAME = SPri1_dat,
    FILENAME = 'D:\SalesData\SPri1dat.mdf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 15% ),
( NAME = SPri2_dat,
    FILENAME = 'D:\SalesData\SPri2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 15% ),
FILEGROUP SalesGroup1
( NAME = SGrp1Fi1_dat,
    FILENAME = 'D:\SalesData\SG1Fi1dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
( NAME = SGrp1Fi2_dat,
    FILENAME = 'D:\SalesData\SG1Fi2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
FILEGROUP SalesGroup2
( NAME = SGrp2Fi1_dat,
    FILENAME = 'D:\SalesData\SG2Fi1dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
( NAME = SGrp2Fi2_dat,
    FILENAME = 'D:\SalesData\SG2Fi2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 )
LOG ON
( NAME = Sales_log,
    FILENAME = 'E:\SalesLog\salelog.ldf',
    SIZE = 5MB,
    MAXSIZE = 25MB,
    FILEGROWTH = 5MB ) ;
GO
```

### <a name="e-attaching-a-database"></a>E. Anfügen einer Datenbank

Im folgenden Beispiel wird die in Beispiel D erstellte `Archive`-Datenbank gelöst und dann mithilfe der `FOR ATTACH`-Klausel angefügt. `Archive` wurde so definiert, dass mehrere Daten- und Protokolldateien vorhanden sind. Da sich jedoch der Speicherort der Dateien seit ihrem Erstellen nicht geändert hat, muss nur die primäre Datei in der `FOR ATTACH`-Klausel angegeben werden. Ab Version [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] werden alle Volltextdateien, die zur angefügten Datenbank gehören, mit der Datenbank angefügt.

```sql
USE master;
GO
sp_detach_db Archive;
GO
CREATE DATABASE Archive
  ON (FILENAME = 'D:\SalesData\archdat1.mdf')
  FOR ATTACH ;
GO
```

### <a name="f-creating-a-database-snapshot"></a>F. Erstellen einer Datenbankmomentaufnahme

Im folgenden Beispiel wird die Datenbankmomentaufnahme `sales_snapshot0600` erstellt. Da eine Datenbankmomentaufnahme schreibgeschützt ist, kann keine Protokolldatei angegeben werden. In Übereinstimmung mit der Syntax wird jede Datei in der Quelldatenbank angegeben, Dateigruppen werden nicht angegeben.

Die Quelldatenbank für dieses Beispiel ist die `Sales`-Datenbank, die in Beispiel D erstellt wurde.

```sql
USE master;
GO
CREATE DATABASE sales_snapshot0600 ON
    ( NAME = SPri1_dat, FILENAME = 'D:\SalesData\SPri1dat_0600.ss'),
    ( NAME = SPri2_dat, FILENAME = 'D:\SalesData\SPri2dt_0600.ss'),
    ( NAME = SGrp1Fi1_dat, FILENAME = 'D:\SalesData\SG1Fi1dt_0600.ss'),
    ( NAME = SGrp1Fi2_dat, FILENAME = 'D:\SalesData\SG1Fi2dt_0600.ss'),
    ( NAME = SGrp2Fi1_dat, FILENAME = 'D:\SalesData\SG2Fi1dt_0600.ss'),
    ( NAME = SGrp2Fi2_dat, FILENAME = 'D:\SalesData\SG2Fi2dt_0600.ss')
AS SNAPSHOT OF Sales ;
GO
```

### <a name="g-creating-a-database-and-specifying-a-collation-name-and-options"></a>G. Erstellen einer Datenbank, Angeben eines Sortierungsnamens und Angeben von Optionen

Im folgenden Beispiel wird die Datenbank mit dem Namen `MyOptionsTest` erstellt. Ein Sortierungsname wird angegeben, und für die Optionen `TRUSTYWORTHY` und `DB_CHAINING` wird `ON` festgelegt.

```sql
USE master;
GO
IF DB_ID (N'MyOptionsTest') IS NOT NULL
DROP DATABASE MyOptionsTest;
GO
CREATE DATABASE MyOptionsTest
COLLATE French_CI_AI
WITH TRUSTWORTHY ON, DB_CHAINING ON;
GO
--Verifying collation and option settings.
SELECT name, collation_name, is_trustworthy_on, is_db_chaining_on
FROM sys.databases
WHERE name = N'MyOptionsTest';
GO
```

### <a name="h-attaching-a-full-text-catalog-that-has-been-moved"></a>H. Anhängen eines Volltextkatalogs, der verschoben wurde

Im folgenden Beispiel wird der Volltextkatalog `AdvWksFtCat` zusammen mit den Daten und Protokolldateien von `AdventureWorks2012` angefügt. In diesem Beispiel wird der Volltextkatalog vom Standardspeicherort an den neuen Speicherort `c:\myFTCatalogs` verschoben. Die Daten- und Protokolldateien bleiben an ihrem jeweiligen Standardspeicherort.

```sql
USE master;
GO
--Detach the AdventureWorks2012 database
sp_detach_db AdventureWorks2012;
GO
-- Physically move the full text catalog to the new location.
--Attach the AdventureWorks2012 database and specify the new location of the full-text catalog.
CREATE DATABASE AdventureWorks2012 ON
    (FILENAME = 'c:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_data.mdf'),
    (FILENAME = 'c:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_log.ldf'),
    (FILENAME = 'c:\myFTCatalogs\AdvWksFtCat')
FOR ATTACH;
GO
```

### <a name="i-creating-a-database-that-specifies-a-row-filegroup-and-two-filestream-filegroups"></a>I. Erstellen einer Datenbank, die eine Zeilendateigruppe und zwei FILESTREAM-Dateigruppen angibt

Im folgenden Beispiel wird die `FileStreamDB`-Datenbank erstellt. Die Datenbank wird mit einer Zeilendateigruppe und zwei FILESTREAM-Dateigruppen erstellt. Jede Dateigruppe enthält eine Datei:

- `FileStreamDB_data` enthält Zeilendaten. Darin enthalten ist eine Datei `FileStreamDB_data.mdf` mit dem Standardpfad.
- `FileStreamPhotos` enthält FILESTREAM-Daten. Darin enthalten sind zwei FILESTREAM-Datencontainer: `FSPhotos` unter `C:\MyFSfolder\Photos` und `FSPhotos2` unter `D:\MyFSfolder\Photos`. Er ist als FILESTREAM-Standarddateigruppe gekennzeichnet.
- `FileStreamResumes` enthält FILESTREAM-Daten. Darin enthalten ist ein FILESTREAM-Datencontainer `FSResumes`, der sich unter `C:\MyFSfolder\Resumes` befindet.

```sql
USE master;
GO
-- Get the SQL Server data path.
DECLARE @data_path nvarchar(256);
SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);

 -- Execute the CREATE DATABASE statement.
EXECUTE ('CREATE DATABASE FileStreamDB
ON PRIMARY
    (
    NAME = FileStreamDB_data
    ,FILENAME = ''' + @data_path + 'FileStreamDB_data.mdf''
    ,SIZE = 10MB
    ,MAXSIZE = 50MB
    ,FILEGROWTH = 15%
    ),
FILEGROUP FileStreamPhotos CONTAINS FILESTREAM DEFAULT
    (
    NAME = FSPhotos
    ,FILENAME = ''C:\MyFSfolder\Photos''
-- SIZE and FILEGROWTH should not be specified here.
-- If they are specified an error will be raised.
, MAXSIZE = 5000 MB
    ),
    (
      NAME = FSPhotos2
      , FILENAME = ''D:\MyFSfolder\Photos''
      , MAXSIZE = 10000 MB
     ),
FILEGROUP FileStreamResumes CONTAINS FILESTREAM
    (
    NAME = FileStreamResumes
    ,FILENAME = ''C:\MyFSfolder\Resumes''
    )
LOG ON
    (
    NAME = FileStream_log
    ,FILENAME = ''' + @data_path + 'FileStreamDB_log.ldf''
    ,SIZE = 5MB
    ,MAXSIZE = 25MB
    ,FILEGROWTH = 5MB
    )'
);
GO
```

### <a name="j-creating-a-database-that-has-a-filestream-filegroup-with-multiple-files"></a>J. Erstellen einer Datenbank mit einer FILESTREAM-Dateigruppe mit mehreren Dateien

Im folgenden Beispiel wird die `BlobStore1`-Datenbank erstellt. Die Datenbank wird mit einer Zeilendateigruppe und einer FILESTREAM-Dateigruppe, `FS`, erstellt. Die FILESTREAM-Dateigruppe enthält die beiden Dateien `FS1` und `FS2`. Dann wird die Datenbank durch das Hinzufügen der dritten Datei `FS3` zur FILESTREAM-Dateigruppe geändert.

```sql
USE master;
GO

CREATE DATABASE [BlobStore1]
CONTAINMENT = NONE
ON PRIMARY
(
    NAME = N'BlobStore1',
    FILENAME = N'C:\BlobStore\BlobStore1.mdf',
    SIZE = 100MB,
    MAXSIZE = UNLIMITED,
    FILEGROWTH = 1MB
),
FILEGROUP [FS] CONTAINS FILESTREAM DEFAULT
(  
    NAME = N'FS1',
    FILENAME = N'C:\BlobStore\FS1',
    MAXSIZE = UNLIMITED
),
(
    NAME = N'FS2',
    FILENAME = N'C:\BlobStore\FS2',
    MAXSIZE = 100MB
)
LOG ON
(
    NAME = N'BlobStore1_log',
    FILENAME = N'C:\BlobStore\BlobStore1_log.ldf',
    SIZE = 100MB,
    MAXSIZE = 1GB,
    FILEGROWTH = 1MB
);
GO

ALTER DATABASE [BlobStore1]
ADD FILE
(
    NAME = N'FS3',
    FILENAME = N'C:\BlobStore\FS3',
    MAXSIZE = 100MB
)
TO FILEGROUP [FS];
GO
```

## <a name="see-also"></a>Weitere Informationen

- [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)
- [Anfügen und Trennen von Datenbanken](../../relational-databases/databases/database-detach-and-attach-sql-server.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md)
- [sp_detach_db](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)
- [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)
- [Datenbank-Momentaufnahmen](../../relational-databases/databases/database-snapshots-sql-server.md)
- [Verschieben von Datenbankdateien](../../relational-databases/databases/move-database-files.md)
- [Datenbanken](../../relational-databases/databases/databases.md)
- [Binary Large Object-Daten – Blob](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL-Datenbank \*_**
    :::column-end:::
    :::column:::
        [SQL Managed Instance](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL-Datenbank

## <a name="overview"></a>Übersicht

Diese Anweisung kann in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] mit einem Azure SQL-Server verwendet werden, um ein Singleton oder einen Pool für elastische Datenbanken zu erstellen. Mit dieser Anweisung geben Sie für die neue Datenbank den Datenbanknamen, die Sortierung, die maximale Größe, die Edition, das Dienstziel und ggf. den Pool für elastische Datenbanken an. Mit ihr lässt sich auch die Datenbank in einem Pool für elastische Datenbanken erstellen. Außerdem kann sie verwendet werden, um eine Kopie der Datenbank auf einem anderen SQL-Datenbankserver zu erstellen.

## <a name="syntax"></a>Syntax

### <a name="create-a-database"></a>Erstellen einer Datenbank

```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
{
  (<edition_options> [, ...n])
}
[ WITH <with_options> [,..n]]
[;]

<with_options> ::=
{
  CATALOG_COLLATION = { DATABASE_DEFAULT | SQL_Latin1_General_CP1_CI_AS }
  | BACKUP_STORAGE_REDUNDANCY = { 'LOCAL' | 'ZONE' | 'GEO' }
}

<edition_options> ::=
{

  MAXSIZE = { 100 MB | 500 MB | 1 ... 1024 ... 4096 GB }
  | ( EDITION = { 'Basic' | 'Standard' | 'Premium' | 'GeneralPurpose' | 'BusinessCritical' | 'Hyperscale' }
  | SERVICE_OBJECTIVE =
    { 'Basic' | 'S0' | 'S1' | 'S2' | 'S3' | 'S4'| 'S6'| 'S7'| 'S9'| 'S12'
      | 'P1' | 'P2' | 'P4'| 'P6' | 'P11' | 'P15'
      | 'GP_Gen4_1' | 'GP_Gen4_2' | 'GP_Gen4_3' | 'GP_Gen4_4' | 'GP_Gen4_5' | 'GP_Gen4_6'
      | 'GP_Gen4_7' | 'GP_Gen4_8' | 'GP_Gen4_9' | 'GP_Gen4_10' | 'GP_Gen4_16' | 'GP_Gen4_24'
      | 'GP_Gen5_2' | 'GP_Gen5_4' | 'GP_Gen5_6' | 'GP_Gen5_8' | 'GP_Gen5_10' | 'GP_Gen5_12' | 'GP_Gen5_14'
      | 'GP_Gen5_16' | 'GP_Gen5_18' | 'GP_Gen5_20' | 'GP_Gen5_24' | 'GP_Gen5_32' | 'GP_Gen5_40' | 'GP_Gen5_80'
      | 'GP_Fsv2_8' | 'GP_Fsv2_10' | 'GP_Fsv2_12' | 'GP_Fsv2_14' | 'GP_Fsv2_16' | 'GP_Fsv2_18'
      | 'GP_Fsv2_20' | 'GP_Fsv2_24' | 'GP_Fsv2_32' | 'GP_Fsv2_36' | 'GP_Fsv2_72'
      | 'GP_S_Gen5_1' | 'GP_S_Gen5_2' | 'GP_S_Gen5_4' | 'GP_S_Gen5_6' | 'GP_S_Gen5_8'
      | 'GP_S_Gen5_10' | 'GP_S_Gen5_12' | 'GP_S_Gen5_14' | 'GP_S_Gen5_16'
      | 'GP_S_Gen5_18' | 'GP_S_Gen5_20' | 'GP_S_Gen5_24' | 'GP_S_Gen5_32' | 'GP_S_Gen5_40'
      | 'BC_Gen4_1' | 'BC_Gen4_2' | 'BC_Gen4_3' | 'BC_Gen4_4' | 'BC_Gen4_5' | 'BC_Gen4_6'
      | 'BC_Gen4_7' | 'BC_Gen4_8' | 'BC_Gen4_9' | 'BC_Gen4_10' | 'BC_Gen4_16' | 'BC_Gen4_24'
      | 'BC_Gen5_2' | 'BC_Gen5_4' | 'BC_Gen5_6' | 'BC_Gen5_8' | 'BC_Gen5_10' | 'BC_Gen5_12' | 'BC_Gen5_14'
      | 'BC_Gen5_16' | 'BC_Gen5_18' | 'BC_Gen5_20' | 'BC_Gen5_24' | 'BC_Gen5_32' | 'BC_Gen5_40' | 'BC_Gen5_80'
      | 'BC_M_8' | 'BC_M_10' | 'BC_M_12' | 'BC_M_14' | 'BC_M_16' | 'BC_M_18'
      | 'BC_M_20' | 'BC_M_24' | 'BC_M_32' | 'BC_M_64' | 'BC_M_128'
      | 'HS_GEN4_1' | 'HS_GEN4_2' | 'HS_GEN4_4' | 'HS_GEN4_8' | 'HS_GEN4_16' | 'HS_GEN4_24'
      | 'HS_GEN5_2' | 'HS_GEN5_4' | 'HS_GEN5_8' | 'HS_GEN5_16' | 'HS_GEN5_24' | 'HS_GEN5_32' | 'HS_GEN5_48' | 'HS_GEN5_80'
      | { ELASTIC_POOL(name = <elastic_pool_name>) } })
}
```

### <a name="copy-a-database"></a>Kopieren einer Datenbank

```syntaxsql
CREATE DATABASE database_name
    AS COPY OF [source_server_name.] source_database_name
    [ ( SERVICE_OBJECTIVE =
      { 'Basic' |'S0' | 'S1' | 'S2' | 'S3'| 'S4'| 'S6'| 'S7'| 'S9'| 'S12'
      | 'P1' | 'P2' | 'P4'| 'P6' | 'P11' | 'P15'
      | 'GP_Gen4_1' | 'GP_Gen4_2' | 'GP_Gen4_3' | 'GP_Gen4_4' | 'GP_Gen4_5' | 'GP_Gen4_6'
      | 'GP_Gen4_7' | 'GP_Gen4_8' | 'GP_Gen4_9' | 'GP_Gen4_10' | 'GP_Gen4_16' | 'GP_Gen4_24'
      | 'GP_Gen5_2' | 'GP_Gen5_4' | 'GP_Gen5_6' | 'GP_Gen5_8' | 'GP_Gen5_10' | 'GP_Gen5_12' | 'GP_Gen5_14'
      | 'GP_Gen5_16' | 'GP_Gen5_18' | 'GP_Gen5_20' | 'GP_Gen5_24' | 'GP_Gen5_32' | 'GP_Gen5_40' | 'GP_Gen5_80'
      | 'GP_Fsv2_8' | 'GP_Fsv2_10' | 'GP_Fsv2_12' | 'GP_Fsv2_14' | 'GP_Fsv2_16' | 'GP_Fsv2_18'
      | 'GP_Fsv2_20' | 'GP_Fsv2_24' | 'GP_Fsv2_32' | 'GP_Fsv2_36' | 'GP_Fsv2_72'
      | 'GP_S_Gen5_1' | 'GP_S_Gen5_2' | 'GP_S_Gen5_4' | 'GP_S_Gen5_6' | 'GP_S_Gen5_8'
      | 'GP_S_Gen5_10' | 'GP_S_Gen5_12' | 'GP_S_Gen5_14' | 'GP_S_Gen5_16'
      | 'GP_S_Gen5_18' | 'GP_S_Gen5_20' | 'GP_S_Gen5_24' | 'GP_S_Gen5_32' | 'GP_S_Gen5_40'
      | 'BC_Gen4_1' | 'BC_Gen4_2' | 'BC_Gen4_3' | 'BC_Gen4_4' | 'BC_Gen4_5' | 'BC_Gen4_6'
      | 'BC_Gen4_7' | 'BC_Gen4_8' | 'BC_Gen4_9' | 'BC_Gen4_10' | 'BC_Gen4_16' | 'BC_Gen4_24'
      | 'BC_Gen5_2' | 'BC_Gen5_4' | 'BC_Gen5_6' | 'BC_Gen5_8' | 'BC_Gen5_10' | 'BC_Gen5_12' | 'BC_Gen5_14'
      | 'BC_Gen5_16' | 'BC_Gen5_18' | 'BC_Gen5_20' | 'BC_Gen5_24' | 'BC_Gen5_32' | 'BC_Gen5_40' | 'BC_Gen5_80'
      | 'BC_M_8' | 'BC_M_10' | 'BC_M_12' | 'BC_M_14' | 'BC_M_16' | 'BC_M_18'
      | 'BC_M_20' | 'BC_M_24' | 'BC_M_32' | 'BC_M_64' | 'BC_M_128'
      | 'HS_GEN4_1' | 'HS_GEN4_2' | 'HS_GEN4_4' | 'HS_GEN4_8' | 'HS_GEN4_16' | 'HS_GEN4_24'
      | 'HS_GEN5_2' | 'HS_GEN5_4' | 'HS_GEN5_8' | 'HS_GEN5_16' | 'HS_GEN5_24' | 'HS_GEN5_32' | 'HS_GEN5_48' | 'HS_GEN5_80'
      | { ELASTIC_POOL(name = <elastic_pool_name>) } })
   ]
[;]
```

## <a name="arguments"></a>Argumente

*database_name* Der Name der neuen Datenbank. Dieser Name muss auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eindeutig sein und den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Regeln für Bezeichner entsprechen. Weitere Informationen finden Sie unter [Bezeichner](../../relational-databases/databases/database-identifiers.md).

*Collation_name* gibt die Standardsortierung für die Datenbank an. Als Sortierungsname kann entweder der Name einer Windows-Sortierreihenfolge oder ein SQL-Sortierungsname verwendet werden. Wenn keine Angabe erfolgt, wird der Datenbank die Standardsortierung „SQL_Latin1_General_CP1_CI_AS“ zugewiesen.

Weitere Informationen zu den Windows- und SQL-Sortierungsnamen finden Sie unter [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md).

CATALOG_COLLATION gibt die Standardsortierung für den Metadatenkatalog an. *DATABASE_DEFAULT* gibt an, dass der Metadatenkatalog sortiert werden muss, der für Systemansichten und Systemtabellen verwendet wird, um der Standardsortierung der Datenbank zu entsprechen. SQL Server weist dieses Verhalten auf.

*SQL_Latin1_General_CP1_CI_AS* gibt an, dass der Metadatenkatalog, der für Systemansichten und -tabellen verwendet wird, für die feste Sortierung „SQL_Latin1_General_CP1_CI_AS“ sortiert werden muss. Sofern nichts anderes angegeben ist, entspricht dies der Standardeinstellung für Azure SQL-Datenbank.

BACKUP_STORAGE_REDUNDANCY gibt an, wie die Zeitpunktwiederherstellung und LTR-Sicherungen (Long-Term Retention, Langzeitaufbewahrung) für eine Datenbank repliziert werden. Die Geowiederherstellung und die Wiederherstellung nach regionalen Ausfällen sind nur verfügbar, wenn die Datenbank mit der Sicherungsspeicherredundanz GEO erstellt wird. Mit T-SQL erstellte Datenbanken verwenden georedundanten Sicherungsspeicher, sofern nichts anderes angegeben wird. 

> [!IMPORTANT]
> Die Option BACKUP_STORAGE_REDUNDANCY für Azure SQL-Datenbank ist derzeit nur in der Azure-Region „Brasilien, Süden“ in der Public Preview und in „Asien, Südosten“ allgemein verfügbar.  

EDITION gibt die Dienstebene der Datenbank an.

Einzel- und Pooldatenbanken Die verfügbaren Werte sind: „Basic“, „Standard“, „Premium“, „GeneralPurpose“, „BusinessCritical“ und „Hyperscale“.

MAXSIZE gibt die maximale Größe der Datenbank an. MAXSIZE muss für die angegebene EDITION (Dienstebene) gültig sein. Im Folgenden sind die unterstützten MAXSIZE-Werte und die Standardwerte (S) für die Dienstebenen aufgeführt.

> [!NOTE]
> Das Argument **MAXSIZE** gilt nicht für Einzeldatenbanken im Diensttarif „Hyperscale“. Datenbanken im Tarif „Hyperscale“ können bei Bedarf auf bis zu 100 TB skaliert werden. Der SQL-Datenbank-Dienst fügt automatisch Speicher hinzu. Sie müssen keine maximale Größe festlegen.

**DTU-Modell für einzelne und in einem Pool zusammengefasste Datenbanken auf einem SQL-Datenbankserver**

|**MAXSIZE**|**Grundlegend**|**S0-S2**|**S3-S12**|**P1-P6**| **P11-P15** |
|:---|:---|:---|:---|:---|:---|
|100 MB|√|√|√|√|√|
|500 MB|√|√|√|√|√|
|1 GB|√|√|√|√|√|
|2 GB|√ (S)|√|√|√|√|
|5 GB|–|√|√|√|√|
|10 GB|–|√|√|√|√|
|20 GB|–|√|√|√|√|
|30 GB|–|√|√|√|√|
|40 GB|–|√|√|√|√|
|50 GB|–|√|√|√|√|
|100 GB|–|√|√|√|√|
|150 GB|–|√|√|√|√|
|200 GB|–|√|√|√|√|
|250 GB|–|√ (S)|√ (S)|√|√|
|300 GB|–|–|√|√|√|
|400 GB|–|–|√|√|√|
|500 GB|–|–|√|√ (S)|√|
|750 GB|–|–|√|√|√|
|1024 GB|–|–|√|√|√ (S)|
|Von 1024 GB bis 4096 GB in Inkrementen von 256 GB* |–|NICHT ZUTREFFEND|–|Nicht zutreffend|√|√|

\* P11 und P15 ermöglichen, dass die Größe von MAXSIZE bis zu 4 TB beträgt, wobei 1024 GB die Standardgröße darstellt. P11 und P15 können bis zu 4 TB des enthaltenen Speichers ohne Aufpreis verwenden. Im Premium-Tarif ist MAXSIZE mit einer Größe von mehr als 1 TB derzeit in den folgenden Regionen verfügbar: USA, Osten 2; USA, Westen; US Gov Virginia; Europa, Westen; Deutschland, Mitte; Asien, Südosten; Japan, Osten; Australien, Osten; Kanada, Mitte und Kanada, Osten. Zusätzliche Informationen bezüglich der Ressourcenbeschränkungen für das DTU-Modell finden Sie unter [DTU-Ressourcenlimits](/azure/sql-database/sql-database-dtu-resource-limits).

Der MAXSIZE-Wert für das DTU-Modell muss – wenn angegeben – ein gültiger Wert sein, der in der Tabelle oben für die festgelegte Dienstebene angezeigt wird.

**V-Kern-Modell**

**Universell – bereitgestellte Computekapazität – Gen4 (Teil 1)**

|MAXSIZE|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6|
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|Maximale Datengröße (GB)|1024|1024|1024|1536|1536|1536|

**Universell – bereitgestellte Computekapazität – Gen4 (Teil 2)**

|MAXSIZE|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|Maximale Datengröße (GB)|1536|3072|3072|3072|4096|4096|

**Universell – bereitgestellte Computekapazität – Gen5 (Teil 1)**

|MAXSIZE|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|Maximale Datengröße (GB)|1024|1024|1024|1536|1536|1536|1536|

**Universell – bereitgestellte Computekapazität – Gen5 (Teil 2)**

|MAXSIZE|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|Maximale Datengröße (GB)|3072|3072|3072|4096|4096|4096|4096|

**Universell – bereitgestelltes Computing – Fsv2-Serie (Teil 1)**

|MAXSIZE|GP_Fsv2_8|GP_Fsv2_10|GP_Fsv2_12|GP_Fsv2_14|GP_Fsv2_16|GP_Fsv2_18|
|:----- | ------: | ------: | ------: | ------: | ------: | ------: |
|Maximale Datengröße (GB)|1024|1024|1024|1024|1536|1536|

**Universell – bereitgestelltes Computing – Fsv2-Serie (Teil 2)**

|MAXSIZE|GP_Fsv2_20|GP_Fsv2_24|GP_Fsv2_32|GP_Fsv2_36|GP_Fsv2_72|
|:----- | ------: | ------: | ------: | ------: | ------: |
|Maximale Datengröße (GB)|1536|1536|3072|3072|4096|

**Universell – serverlose Computekapazität – Gen5 (Teil 1)**

|MAXSIZE|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|GP_S_Gen5_6|GP_S_Gen5_8|
|:----- | ------: |-------: |-------: |-------: |--------: |
|Max. virtuelle Kerne|1|2|4|6|8|

**Universell – serverlose Computekapazität – Gen5 (Teil 2)**

|MAXSIZE|GP_S_Gen5_10|GP_S_Gen5_12|GP_S_Gen5_14|GP_S_Gen5_16|
|:----- | ------: |-------: |-------: |-------: |
|Max. virtuelle Kerne|10|12|14|16|

**Universell – serverlose Computekapazität – Gen5 (Teil 3)**

|MAXSIZE|GP_S_Gen5_18|GP_S_Gen5_20|GP_S_Gen5_24|GP_S_Gen5_32|GP_S_Gen5_40|
|:----- | ------: |-------: |-------: |-------: |--------: |
|Max. virtuelle Kerne|18|20|24|32|40|

**Unternehmenskritisch – bereitgestellte Computekapazität – Gen4 (Teil 1)**

|Computegröße (Dienstziel)|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--------------- | ------: |-------: |-------: |-------: |-------: |-------: |
|Maximale Datengröße (GB)|1024|1024|1024|1024|1024|1024|

**Unternehmenskritisch – bereitgestellte Computekapazität – Gen4 (Teil 2)**

|Computegröße (Dienstziel)|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--------------- | ------: |-------: |-------: |--------: |--------: |--------: |
|Maximale Datengröße (GB)|1024|1024|1024|1024|1024|1024|

**Unternehmenskritisch – bereitgestellte Computekapazität – Gen5 (Teil 1)**

|MAXSIZE|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |---------: |--------:|--------: |
|Maximale Datengröße (GB)|1024|1024|1024|1536|1536|1536|1536|

**Unternehmenskritisch – bereitgestellte Computekapazität – Gen5 (Teil 2)**

|MAXSIZE|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:----- | -------: |--------: |--------: |--------: |--------: |---------:|--------: |
|Maximale Datengröße (GB)|3072|3072|3072|4096|4096|4096|4096|

**Unternehmenskritisch – bereitgestelltes Computing – M-Serie (Teil 1)**

|MAXSIZE|BC_M_8|BC_M_10|BC_M_12|BC_M_14|BC_M_16|BC_M_18|
|:----- | -------: | -------: | -------: | -------: | -------: | -------: |
|Maximale Datengröße (GB)|512|640|768|896|1024|1152|

**Unternehmenskritisch – bereitgestelltes Computing – M-Serie (Teil 2)**

|MAXSIZE|BC_M_20|BC_M_24|BC_M_32|BC_M_64|BC_M_128|
|:----- | -------: | -------: | -------: | -------: | -------: |
|Maximale Datengröße (GB)|1280|1536|2048|4096|4096|

Wenn kein `MAXSIZE`-Wert bei Verwendung des vCore-Modells festgelegt ist, beträgt die Standardgröße 32 GB. Zusätzliche Informationen bezüglich der Ressourcenbeschränkungen für das V-Kern-Modell finden Sie unter [V-Kern-Ressourcenlimits](/azure/sql-database/sql-database-dtu-resource-limits).

Die folgenden Regeln gelten für das MAXSIZE-Argument und das EDITION-Argument:

- Wenn EDITION angegeben ist, MAXSIZE jedoch nicht, wird der Standardwert für die Edition verwendet. Wenn EDITION beispielsweise auf „Standard“ festgelegt und MAXSIZE nicht angegeben ist, wird MAXSIZE automatisch auf 250 MB festgelegt.
- Wenn weder MAXSIZE noch EDITION angegeben sind, wird EDITION auf `GeneralPurpose` und MAXSIZE auf 32 GB festgelegt.

SERVICE_OBJECTIVE

- **Bei einzelnen und in einem Pool zusammengefassten Datenbanken**

  - Gibt die Computegröße (Dienstziel) an. Als Dienstziele sind die folgenden Werte verfügbar: `S0`, `S1`, `S2`, `S3`, `S4`, `S6`, `S7`, `S9`, `S12`, `P1`, `P2`, `P4`, `P6`, `P11`, `P15`, `GP_GEN4_1`, `GP_GEN4_2`, `GP_GEN4_3`, `GP_GEN4_4`, `GP_GEN4_5`, `GP_GEN4_6`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_9`, `GP_GEN4_10`, `GP_GEN4_16`, `GP_GEN4_24`, `BC_GEN4_1`, `BC_GEN4_2`, `BC_GEN4_3`, `BC_GEN4_4`, `BC_GEN4_5`, `BC_GEN4_6`, `BC_GEN4_7`, `BC_GEN4_8`, `BC_GEN4_9`, `BC_GEN4_10`, `BC_GEN4_16`, `BC_GEN4_24`, `GP_Gen5_2`, `GP_Gen5_4`, `GP_Gen5_6`, `GP_Gen5_8`, `GP_Gen5_10`, `GP_Gen5_12`, `GP_Gen5_14`, `GP_Gen5_16`, `GP_Gen5_18`, `GP_Gen5_20`, `GP_Gen5_24`, `GP_Gen5_32`, `GP_Gen5_40`, `GP_Gen5_80`, `GP_Fsv2_8`, `GP_Fsv2_10`, `GP_Fsv2_12`, `GP_Fsv2_14`, `GP_Fsv2_16`, `GP_Fsv2_18`, `GP_Fsv2_20`, `GP_Fsv2_24`, `GP_Fsv2_32`, `GP_Fsv2_36`, `GP_Fsv2_72`, `BC_Gen5_2`, `BC_Gen5_4`, `BC_Gen5_6`, `BC_Gen5_8`, `BC_Gen5_10`, `BC_Gen5_12`, `BC_Gen5_14`, `BC_Gen5_16`, `BC_Gen5_18`, `BC_Gen5_20`, `BC_Gen5_24`, `BC_Gen5_32`,`BC_Gen5_40`, `BC_Gen5_80`, `BC_M_8`, `BC_M_10`, `BC_M_12`, `BC_M_14`, `BC_M_16`, `BC_M_18`, `BC_M_20`, `BC_M_24`, `BC_M_32`, `BC_M_64`, `BC_M_128`.

- **Für serverlose Datenbanken**
- 
  - Gibt die Computegröße (Dienstziel) an. Als Dienstziele sind die folgenden Werte verfügbar: `GP_S_Gen5_1` `GP_S_Gen5_2` `GP_S_Gen5_4` `GP_S_Gen5_6` `GP_S_Gen5_8`, `GP_S_Gen5_10`, `GP_S_Gen5_12`, `GP_S_Gen5_14`, `GP_S_Gen5_16`, `GP_S_Gen5_18`, `GP_S_Gen5_20`, `GP_S_Gen5_24`, `GP_S_Gen5_32`, `GP_S_Gen5_40`.

- **Bei einzelnen Datenbanken im Diensttarif „Hyperscale“**

  - Gibt die Computegröße (Dienstziel) an. Als Dienstziele sind die folgenden Werte verfügbar: `HS_GEN4_1` `HS_GEN4_2` `HS_GEN4_4` `HS_GEN4_8` `HS_GEN4_16`, `HS_GEN4_24`, `HS_Gen5_2`, `HS_Gen5_4`, `HS_Gen5_8`, `HS_Gen5_16`, `HS_Gen5_24`, `HS_Gen5_32`, `HS_Gen5_48`, `HS_Gen5_80`.

Dienstzielbeschreibungen und weitere Informationen zu Größe, Editionen und Dienstzielkombinationen finden Sie unter [Dienstebenen von Azure SQL-Datenbank](/azure/sql-database/sql-database-service-tiers). Wenn das angegebene SERVICE_OBJECTIVE von der EDITION nicht unterstützt wird, tritt ein Fehler auf. Zum Ändern des SERVICE_OBJECTIVE-Werts von einer Ebene in eine andere (z. B. von S1 in P1) muss auch der EDITION-Wert geändert werden. Dienstzielbeschreibungen und weitere Informationen zu Größe, Editionen und Dienstzielkombinationen finden Sie unter [Dienstebenen und Leistungsstufen von Azure SQL-Datenbank](/azure/azure-sql/database/purchasing-models), [DTU-Ressourcenlimits](/azure/sql-database/sql-database-dtu-resource-limits) und [V-Kern-Ressourcenlimits](/azure/sql-database/sql-database-dtu-resource-limits). Die Unterstützung für PRS-Dienstziele wurde entfernt. Wenn Sie Fragen haben, wenden Sie sich an den E-Mail-Alias premium-rs@microsoft.com.

ELASTIC_POOL (Name = \<elastic_pool_name>) **Gilt für:** Einzelne und in einem Pool zusammengefasste Datenbanken. Gilt nicht für Datenbanken im Diensttarif „Hyperscale“.
Legen Sie zum Erstellen einer neuen Datenbank in einem Pool für elastische Datenbanken das Schlüsselwort SERVICE_OBJECTIVE der Datenbank auf ELASTIC_POOL fest, und stellen Sie den Namen des Pools bereit. Weitere Informationen finden Sie unter [Erstellen und Verwalten eines Pools für elastische Datenbanken von SQL-Database](/azure/azure-sql/database/elastic-pool-overview).

AS COPY OF [source_server_name.]source_database_name **Gilt für:** Einzelne und in einem Pool zusammengefasste Datenbanken.
Zum Kopieren einer Datenbank auf demselben oder einem anderen [!INCLUDE[ssSDS](../../includes/sssds-md.md)]-Server.

*source_server_name*: Der Name des [!INCLUDE[ssSDS](../../includes/sssds-md.md)]-Servers, auf dem sich die Quelldatenbank befindet. Dieser Parameter ist optional, wenn sich die Quell- und die Zieldatenbank auf demselben [!INCLUDE[ssSDS](../../includes/sssds-md.md)]-Server befinden sollen.

> [!NOTE]
> Das `AS COPY OF`-Argument unterstützt nicht die vollqualifizierten eindeutigen Domänennamen. Das heißt, wenn der vollqualifizierte Domänenname des Servers `serverName.database.windows.net` ist, verwenden Sie `serverName` nur während des Datenbankkopiervorgangs.

*source_database_name*

Der Name der zu kopierenden Datenbank.

## <a name="remarks"></a>Bemerkungen

Datenbanken in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] weisen einige Standardeinstellungen auf, die beim Erstellen der Datenbank festgelegt werden. Weitere Informationen zu diesen Standardeinstellungen finden Sie in der Liste der Werte unter [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

`MAXSIZE` bietet die Möglichkeit, die Größe der Datenbank zu beschränken. Wenn die Größe der Datenbank den Wert von `MAXSIZE` erreicht, erhalten Sie den Fehlercode 40544. In diesem Fall können Sie keine Daten einfügen oder aktualisieren oder neue Objekte (wie Tabellen, gespeicherte Prozeduren. Sichten und Funktionen) erstellen. Sie können jedoch weiterhin Daten lesen und löschen, Tabellen abschneiden, Tabellen und Indizes löschen sowie Indizes neu erstellen. Anschließend können Sie `MAXSIZE` auf einen Wert aktualisieren, der größer als die aktuelle Datenbankgröße ist, oder Sie löschen einige Daten, um Speicherplatz freizugeben. Eine Verzögerung von bis zu fünfzehn Minuten ist möglich, bevor Sie neue Daten einfügen können.

Verwenden Sie [ALTER DATABASE - Azure SQL Database](../../t-sql/statements/alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true), um die Größe, Edition oder die Dienstzielwerte im Nachhinein zu ändern.

Das Argument `CATALOG_COLLATION` ist nur während der Erstellung der Datenbank verfügbar.

## <a name="database-copies"></a>Datenbankkopien

**Anwendungsbereich:** Einzelne und in einem Pool zusammengefasste Datenbanken.

Beim Kopieren einer Datenbank mit der `CREATE DATABASE`-Anweisung handelt es sich um einen asynchronen Vorgang. Deshalb muss nicht für die volle Dauer des Kopiervorgangs eine Verbindung mit dem [!INCLUDE[ssSDS](../../includes/sssds-md.md)]-Server bestehen. Die `CREATE DATABASE`-Anweisung gibt die Steuerung an den Benutzer zurück, nachdem der Eintrag in „sys.databases“ erstellt, aber bevor der Kopiervorgang der Datenbank abgeschlossen wurde. Das heißt, die `CREATE DATABASE`-Anweisung wird erfolgreich ausgeführt, während der Datenbank-Kopiervorgang noch ausgeführt wird.

- Überwachen Sie den Kopiervorgang auf einem [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]-Server: Fragen Sie die `percentage_complete`- oder `replication_state_desc`-Spalte von [dm_database_copies](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md) oder die `state`-Spalte in der Ansicht **sys.databases** ab. Die Ansicht [sys.dm_operation_status](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md) kann ebenfalls verwendet werden, da diese den Status von Datenbankvorgängen zurückgibt, z.B. den des Kopiervorgangs der Datenbank.

Sobald der Kopiervorgang erfolgreich abgeschlossen wurde, ist die Zieldatenbank im Hinblick auf Transaktionen mit der Quelldatenbank konsistent.

Die folgende Syntax und die folgenden semantischen Regeln gelten für die Verwendung des `AS COPY OF`-Arguments:

- Der Quellservername und der Servername für das Kopierziel können identisch oder unterschiedlich sein. Wenn diese identisch sind, ist dieser Parameter optional, und es wird standardmäßig der Serverkontext der aktuellen Sitzung verwendet.
- Die Namen der Quell- und der Zieldatenbank müssen angegeben werden. Diese Namen müssen eindeutig sein und den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Regeln für Bezeichner entsprechen. Weitere Informationen finden Sie unter [Bezeichner](../../relational-databases/databases/database-identifiers.md).
- Die `CREATE DATABASE`-Anweisung muss im Kontext der master-Datenbank des [!INCLUDE[ssSDS](../../includes/sssds-md.md)]-Servers ausgeführt werden, auf dem die neue Datenbank erstellt wird.
- Nachdem der Kopiervorgang abgeschlossen wurde, muss die Zieldatenbank als unabhängige Datenbank verwaltet werden. Sie können die `ALTER DATABASE`-Anweisung und die `DROP DATABASE`-Anweisung unabhängig von der Quelldatenbank für die neue Datenbank ausführen. Außerdem können Sie die neue Datenbank in eine andere neue Datenbank kopieren.
- Der Zugriff auf die Quelldatenbank ist weiterhin möglich, solange der Datenbank-Kopiervorgang ausgeführt wird.

Weitere Informationen finden Sie unter [Create a copy of an Azure SQL database using Transact-SQL (Erstellen einer Kopie einer Azure SQL-Datenbank mithilfe von Transact-SQL)](/azure/azure-sql/database/database-copy).

> [!IMPORTANT]
> Standardmäßig wird die Datenbankkopie mit der gleichen Sicherungsspeicherredundanz wie die Quelldatenbank erstellt. Das Ändern der Sicherungsspeicherredundanz während des Erstellens einer Datenbankkopie wird von T-SQL nicht unterstützt. 


## <a name="permissions"></a>Berechtigungen

Für das Erstellen einer Datenbank muss das Konto des Benutzers einem der Folgenden entsprechen:

- Dem Prinzipalkonto auf Serverebene
- Dem Azure AD-Administrator für den lokalen Azure SQL-Server
- Einem Konto, das Mitglied der `dbmanager`-Datenbankrolle ist

**Zusätzliche Anforderungen für die Verwendung der `CREATE DATABASE ... AS COPY OF`-Syntax:** Der Benutzer, der die Anweisung auf dem lokalen Server ausführt, muss mindestens auch der `db_owner` (Datenbankbesitzer) des Quellservers sein. Wenn das Konto auf der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung basiert, muss der Benutzer, der die Anweisung auf dem lokalen Server ausführt, über passende Anmeldeinformationen (mit identischem Namen und Kennwort) für den Quellserver [!INCLUDE[ssSDS](../../includes/sssds-md.md)] besitzen.

## <a name="examples"></a>Beispiele

### <a name="simple-example"></a>Einfaches Beispiel

Ein einfaches Beispiel für das Erstellen einer Datenbank.

```sql
CREATE DATABASE TestDB1;
```

### <a name="simple-example-with-edition"></a>Einfaches Beispiel mit „Edition“

Ein einfaches Beispiel zum Erstellen einer universellen Datenbank.

```sql
CREATE DATABASE TestDB2
( EDITION = 'GeneralPurpose' );
```

### <a name="example-with-additional-options"></a>Beispiel mit zusätzlichen Optionen

Ein Beispiel, bei dem mehrere Optionen verwendet werden.

```sql
CREATE DATABASE hito
COLLATE Japanese_Bushu_Kakusu_100_CS_AS_KS_WS
( MAXSIZE = 500 MB, EDITION = 'GeneralPurpose', SERVICE_OBJECTIVE = 'GP_GEN4_8' ) ;
```

### <a name="creating-a-copy"></a>Erstellen einer Kopie

Ein Beispiel, in dem die Kopie einer Datenbank erstellt wird.

**Anwendungsbereich:** Einzelne und in einem Pool zusammengefasste Datenbanken.

```sql
CREATE DATABASE escuela
AS COPY OF school;
```

### <a name="creating-a-database-in-an-elastic-pool"></a>Erstellen einer Datenbank in einem elastischen Pool

Erstellt eine neue Datenbank in einem Pool namens „S3M100“:

**Anwendungsbereich:** Einzelne und in einem Pool zusammengefasste Datenbanken.

```sql
CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL ( name = S3M100 ) ) ;
```

### <a name="creating-a-copy-of-a-database-on-another-server"></a>Erstellen einer Kopie einer Datenbank auf einem anderen Server

Im folgenden Beispiel wird eine Kopie der Datenbank „db_original“ namens „db_copy“ in die P2-Computegröße (Dienstziel) für eine einzelne Datenbank erstellt. Dies gilt unabhängig davon, ob „db_original“ sich in einem Pool für elastische Datenbanken oder in einer Computegröße (Dienstziel) für eine einzelne Datenbank befindet.

**Anwendungsbereich:** Einzelne und in einem Pool zusammengefasste Datenbanken.

```sql
CREATE DATABASE db_copy
  AS COPY OF ozabzw7545.db_original ( SERVICE_OBJECTIVE = 'P2' );
```

Im folgenden Beispiel wird eine Kopie der Datenbank „db_original“ namens „db_copy“ in einem elastischen Pool namens „ep1“ erstellt. Dies gilt unabhängig davon, ob „db_original“ sich in einem Pool für elastische Datenbanken oder in einer Computegröße (Dienstziel) für eine einzelne Datenbank befindet. Wenn es sich bei „db_original“ um einen elastischen Pool mit einem unterschiedlichen Namen handelt, wird „db_copy“ dennoch in „ep1“ erstellt.

**Anwendungsbereich:** Einzelne und in einem Pool zusammengefasste Datenbanken.

```sql
CREATE DATABASE db_copy
  AS COPY OF ozabzw7545.db_original
  (SERVICE_OBJECTIVE = ELASTIC_POOL( name = ep1 ) ) ;
```

### <a name="create-database-with-specified-catalog-collation-value"></a>Erstellen einer Datenbank mit einem angegebenen Wert für die Katalogsortierung

Im folgenden Beispiel wird die Katalogsortierung während der Erstellung der Datenbank auf DATABASE_DEFAULT festgelegt. Dadurch entspricht die Katalogsortierung der Datenbanksortierung.

```sql
CREATE DATABASE TestDB3 COLLATE Japanese_XJIS_140 (MAXSIZE = 100 MB, EDITION = 'Basic')
  WITH CATALOG_COLLATION = DATABASE_DEFAULT
```

### <a name="create-database-using-zone-redundancy-for-backups"></a>Erstellen einer Datenbank mit Zonenredundanz für Sicherungen

Im folgenden Beispiel wird die Zonenredundanz für Datenbanksicherungen festgelegt. Sicherungen für die Zeitpunktwiederherstellung und die Langzeitaufbewahrung (falls konfiguriert) verwenden die gleiche Sicherungsspeicherredundanz.

```sql
CREATE DATABASE test_zone_redundancy 
  WITH BACKUP_STORAGE_REDUNDANCY = 'ZONE';
```

## <a name="see-also"></a>Weitere Informationen

- [sys.dm_database_copies - Azure SQL-Datenbank](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)
- [ALTER DATABASE - Azure SQL-Datenbank](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Verwaltete Azure SQL-Instanz

## <a name="overview"></a>Übersicht

Diese Anweisung wird in Azure SQL Managed Instance verwendet, um eine Datenbank zu erstellen. Wenn Sie eine Datenbank in einer verwalteten Instanz erstellen, geben Sie den Datenbanknamen und die Sortierung ein.

## <a name="syntax"></a>Syntax

```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
[;]
```

> [!IMPORTANT]
> Verwenden Sie zum Hinzufügen von Dateien oder Festlegen der Kapselung für eine Datenbank in einer verwalteten Instanz die [ALTER DATABASE](alter-database-transact-sql.md?tabs=sqldbmi)-Anweisung.

## <a name="arguments"></a>Argumente

*database_name* Der Name der neuen Datenbank. Dieser Name muss in der SQL Server-Instanz eindeutig sein und den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Regeln für Bezeichner entsprechen. Weitere Informationen finden Sie unter [Bezeichner](../../relational-databases/databases/database-identifiers.md).

*Collation_name* gibt die Standardsortierung für die Datenbank an. Als Sortierungsname kann entweder der Name einer Windows-Sortierreihenfolge oder ein SQL-Sortierungsname verwendet werden. Wenn keine Angabe erfolgt, wird der Datenbank die Standardsortierung „SQL_Latin1_General_CP1_CI_AS“ zugewiesen.

Weitere Informationen zu den Windows- und SQL-Sortierungsnamen finden Sie unter [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md).

## <a name="remarks"></a>Bemerkungen

Datenbanken in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] weisen einige Standardeinstellungen auf, die beim Erstellen der Datenbank festgelegt werden. Weitere Informationen zu diesen Standardeinstellungen finden Sie in der Liste der Werte unter [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!IMPORTANT]
> Die `CREATE DATABASE`-Anweisung muss die einzige Anweisung in einem [!INCLUDE[tsql](../../includes/tsql-md.md)]-Batch sein.

Es gelten die folgenden Einschränkungen für `CREATE DATABASE`:

- Dateien und Dateigruppen können nicht definiert werden.
- `WITH`-Optionen werden nicht unterstützt.

  > [!TIP]
  > Verwenden Sie zur Umgehung dieses Problems [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true) nach `CREATE DATABASE`, um Datenbankoptionen festzulegen und Dateien hinzuzufügen.

## <a name="permissions"></a>Berechtigungen

Für das Erstellen einer Datenbank muss das Konto des Benutzers einem der Folgenden entsprechen:

- Dem Prinzipalkonto auf Serverebene
- Dem Azure AD-Administrator für den lokalen Azure SQL-Server
- Einem Konto, das Mitglied der `dbcreator`-Datenbankrolle ist

## <a name="examples"></a>Beispiele

### <a name="simple-example"></a>Einfaches Beispiel

Ein einfaches Beispiel für das Erstellen einer Datenbank.

```sql
CREATE DATABASE TestDB1;
```

## <a name="see-also"></a>Weitere Informationen

Siehe [ALTER DATABASE](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="overview"></a>Übersicht

In Azure Synapse kann diese Anweisung mit einer Azure SQL-Datenbank-Serverinstanz verwendet werden, um eine SQL Analytics-Datenbankinstanz zu erstellen. Mit dieser Anweisung geben Sie den Datenbanknamen, die Sortierung, die maximale Größe, die Edition und das Dienstziel an.

## <a name="syntax"></a>Syntax

### <a name="dedicated-sql-pool"></a>[Dedizierter SQL-Pool](#tab/sqlpool)
```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
(
    [ MAXSIZE = {
          250 | 500 | 750 | 1024 | 5120 | 10240 | 20480 | 30720
        | 40960 | 51200 | 61440 | 71680 | 81920 | 92160 | 102400
        | 153600 | 204800 | 245760
      } GB ,
    ]
    EDITION = 'datawarehouse',
    SERVICE_OBJECTIVE = {
         'DW100' | 'DW200' | 'DW300' | 'DW400' | 'DW500' | 'DW600'
        | 'DW1000' | 'DW1200' | 'DW1500' | 'DW2000' | 'DW3000' | 'DW6000'
        |'DW100c' | 'DW200c' | 'DW300c' | 'DW400c' | 'DW500c'
        | 'DW1000c' | 'DW1500c' | 'DW2000c' | 'DW2500c' | 'DW3000c' | 'DW5000c'
        | 'DW6000c' | 'DW7500c' | 'DW10000c' | 'DW15000c' | 'DW30000c'
    }
)
[;]
```
### <a name="serverless-sql-pool"></a>[Serverloser SQL-Pool](#tab/sqlod)
```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
[;] 
```
---

## <a name="arguments"></a>Argumente

*database_name* Der Name der neuen Datenbank. Dieser Name muss auf dem SQL-Server eindeutig sein, der [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]- und [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]-Datenbanken hosten kann, und muss den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Regeln für Bezeichner entsprechen. Weitere Informationen finden Sie unter [Bezeichner](../../relational-databases/databases/database-identifiers.md).

*collation_name* Gibt die Standardsortierung für die Datenbank an. Als Sortierungsname kann entweder der Name einer Windows-Sortierreihenfolge oder ein SQL-Sortierungsname verwendet werden. Wenn keine Angabe erfolgt, wird der Datenbank die Standardsortierung „SQL_Latin1_General_CP1_CI_AS“ zugewiesen.

Weitere Informationen zu den Windows- und SQL-Sortierungsnamen finden Sie unter [COLLATE (Transact-SQL)](./collations.md).

*EDITION* Gibt die Dienstebene der Datenbank an. Verwenden Sie für [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] „datawarehouse“.

*MAXSIZE* Der Standardwert ist 245.760 GB (240 TB).

**Anwendungsbereich:** Optimiert für Compute Gen1

Der Wert für die maximal zulässige Größe der Datenbank Die Datenbank kann nicht größer sein als MAXSIZE.

**Anwendungsbereich:** Optimiert für Compute Gen2

Die maximal zulässige Größe für Rowstore-Daten in der Datenbank Daten, die in Rowstore-Tabellen, dem Deltastore eines Columnstore-Index oder einem nicht gruppierten Index für einen gruppierten Columnstore-Index gespeichert sind, können MAXSIZE nicht übersteigen. Daten, die im Columnstore-Format komprimiert sind, haben kein Größenlimit und werden nicht durch MAXSIZE beschränkt.

SERVICE_OBJECTIVE: Gibt die Computegröße (Dienstziel) an. Weitere Informationen zu Dienstzielen für Azure Synapse finden Sie unter [Data Warehouse-Einheiten (DWUs)](/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu).

## <a name="general-remarks"></a>Allgemeine Hinweise

Verwenden Sie [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md), um die Datenbankeigenschaften anzuzeigen.

Verwenden Sie [ALTER DATABASE – Azure Synapse Analytics](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true), um die Maximalgröße oder die Dienstzielwerte später zu ändern.

Azure Synapse ist auf COMPATIBILITY_LEVEL 130 festgelegt und kann nicht verändert werden. Weitere Informationen finden Sie unter [Verbesserte Abfrageleistung mit Kompatibilitätsgrad 130 in Azure SQL-Datenbank](./alter-database-transact-sql-compatibility-level.md).

## <a name="permissions"></a>Berechtigungen

Erforderliche Berechtigungen:

- Im Rahmen des Bereitstellungsprozesses erstellte Prinzipalanmeldung auf Serverebene oder
- Mitgliedschaft in der `dbmanager`-Datenbankrolle

## <a name="error-handling"></a>Fehlerbehandlung

Wenn die Größe der Datenbank den Wert von MAXSIZE erreicht, erhalten Sie den Fehlercode 40544. In diesem Fall können Sie keine Daten einfügen oder aktualisieren und keine neuen Objekte (wie Tabellen, gespeicherte Prozeduren, Ansichten und Funktionen) erstellen. Sie können jedoch weiterhin Daten lesen und löschen, Tabellen abschneiden, Tabellen und Indizes löschen, sowie Indizes neu erstellen. Anschließend können Sie MAXSIZE auf einen Wert aktualisieren, der größer als die aktuelle Datenbankgröße ist, oder Sie löschen einige Daten, um Speicherplatz freizugeben. Eine Verzögerung von bis zu fünfzehn Minuten ist möglich, bevor Sie neue Daten einfügen können.

## <a name="limitations-and-restrictions"></a>Einschränkungen

Es muss eine Verbindung mit der master-Datenbank bestehen, um eine neue Datenbank zu erstellen.

Die `CREATE DATABASE`-Anweisung muss die einzige Anweisung in einem [!INCLUDE[tsql](../../includes/tsql-md.md)]-Batch sein.

Nachdem die Datenbank erstellt wurde, kann die Datenbanksortierung nicht mehr geändert werden.

## <a name="examples-sssdwfull"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]

### <a name="a-simple-example"></a>A. Einfaches Beispiel
Ein einfaches Beispiel zum Erstellen einer Data Warehouse-Datenbank. Dadurch wird die Datenbank mit der kleinsten Maximalgröße von 10240 GB, der Standardsortierung „SQL_Latin1_General_CP1_CI_AS“ und der geringsten Computeleistung von DW100 erstellt.

```sql
CREATE DATABASE TestDW
(EDITION = 'datawarehouse', SERVICE_OBJECTIVE='DW100');
```

### <a name="b-create-a-data-warehouse-database-with-all-the-options"></a>B. Erstellen Sie eine Data Warehouse-Datenbank mit allen Optionen.
Ein Beispiel zum Erstellen eines 10-TB-Data Warehouses mit allen Optionen.

```sql
CREATE DATABASE TestDW COLLATE Latin1_General_100_CI_AS_KS_WS
(MAXSIZE = 10240 GB, EDITION = 'datawarehouse', SERVICE_OBJECTIVE = 'DW1000');
```

## <a name="see-also"></a>Weitere Informationen

- [ALTER DATABASE – Azure Synapse Analytics](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
- [CREATE TABLE – Azure Synapse Analytics](../../t-sql/statements/create-table-azure-sql-data-warehouse.md)
- [DROP DATABASE - Transact-SQL](../../t-sql/statements/drop-database-transact-sql.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL-Datenbank](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics Platform<br />System (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Analyseplattformsystem

## <a name="overview"></a>Übersicht

In Analytics Platform System wird diese Anweisung verwendet, um in einer Analytics Platform eine neue Datenbank System-Appliance zu erstellen. Verwenden Sie diese Anweisung, um alle Dateien zu erstellen, die einer Appliancedatenbank zugeordnet sind, und um die Optionen für die maximale Größe und die automatische Vergrößerung der Datenbanktabellen und des Transaktionsprotokolls festzulegen.

## <a name="syntax"></a>Syntax

```syntaxsql
CREATE DATABASE database_name
WITH (
    [ AUTOGROW = ON | OFF , ]
    REPLICATED_SIZE = replicated_size [ GB ] ,
    DISTRIBUTED_SIZE = distributed_size [ GB ] ,
    LOG_SIZE = log_size [ GB ] )
[;]
```

## <a name="arguments"></a>Argumente

*database_name* Der Name der neuen Datenbank. Weitere Informationen zu zulässigen Datenbanknamen finden Sie unter „Object Naming Rules“ (Regeln für die Objektbenennung) und „Reserved Database Names“ (Reservierte Datenbanknamen) in der [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

AUTOGROW = ON | **OFF** Gibt an, ob die Parameter *replicated_size*, *distributed_size* und *log_size* für diese Datenbank automatisch je nach Bedarf über ihre angegebenen Größen hinweg vergrößert werden. Der Standardwert lautet **OFF**.

Wenn AUTOGROW auf ON festgelegt ist, vergrößern sich *replicated_size*, *distributed_size* und *log_size* nach Bedarf (nicht in Blöcken der zuerst angegebenen Größe) bei jeder Dateneingabe, jedem Update oder anderen Aktionen, die mehr Speicherplatz als den bereits zugewiesenen erfordern.

Wenn AUTOGROW auf OFF festgelegt ist, verändern sich die Größen nicht automatisch. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] gibt einen Fehler zurück, wenn eine Aktion versucht wird, die erfordert, dass sich *replicated_size*, *distributed_size* oder *log_size* über ihren angegebenen Wert hinweg vergrößern.

AUTOGROW ist für alle Größen entweder auf ON oder auf OFF festgelegt. Es ist beispielsweise nicht möglich, AUTOGROW für *log_size* auf ON und für *replicated_size* auf OFF festzulegen.

*replicated_size* [ GB ] Eine positive Zahl. Legt die Größe (eine ganze Zahl oder Dezimalzahl Gigabytes) für den gesamten Speicherplatz fest, der replizierten Tabellen und den entsprechenden Daten *auf jedem Computeknoten* zugewiesen wurde. Die minimalen und maximalen Anforderungen für *replicated_size* finden Sie unter „Minimum and Maximum Values“ (Minimale und maximale Werte) in der [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Wenn AUTOGROW auf ON festgelegt ist, können sich replizierte Tabellen über diese Begrenzung hinaus vergrößern.

Wenn AUTOGROW auf OFF festgelegt ist, wird ein Fehler zurückgegeben, wenn ein Benutzer versucht, eine neue replizierte Tabelle zu erstellen, Daten in eine bestehende replizierte Tabelle einzufügen oder eine bestehende replizierte Tabelle auf eine Weise zu aktualisieren, durch die die Größe über *replicated_size* hinweg steigen würde.

*distributed_size* [ GB ] Eine positive Zahl. Die Größe, als ganze Zahl oder Dezimalzahl Gigabytes, für den gesamten Speicherplatz, der verteilten Tabellen (und den entsprechenden Daten) *auf der gesamten Appliance* zugewiesen wurde. Die minimalen und maximalen Anforderungen für *distributed_size* finden Sie unter „Minimum and Maximum Values“ (Minimale und maximale Werte) in der [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Wenn AUTOGROW auf ON festgelegt ist, können sich verteilte Tabellen über diese Begrenzung hinaus vergrößern.

Wenn AUTOGROW auf OFF festgelegt ist, wird ein Fehler zurückgegeben, wenn ein Benutzer versucht, eine neue verteilte Tabelle zu erstellen, Daten in eine bestehende verteilte Tabelle einzufügen oder eine bestehende verteilte Tabelle auf eine Weise zu aktualisieren, durch die die Größe über *distributed_size* hinweg steigen würde.

*log_size* [ GB ] Eine positive Zahl. Die Größe (als ganze Zahl oder Dezimalzahl Gigabytes) für das Transaktionsprotokoll *auf der gesamten Appliance*.

Die minimalen und maximalen Anforderungen für *log_size* finden Sie unter „Minimum and Maximum Values“ (Minimale und maximale Werte) in der [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Wenn AUTOGROW auf ON festgelegt ist, kann sich die Protokolldatei über diese Begrenzung hinaus vergrößern. Verwenden Sie die Anweisung [DBCC SHRINKLOG (Azure Synapse Analytics)](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md), um die Protokolldateien auf deren Originalgröße zu verkleinern.

Wenn AUTOGROW auf OFF festgelegt ist, wird an den Benutzer ein Fehler zurückgegeben, wenn eine Aktion ausgeführt wird, durch die sich die Protokollgröße auf einem einzelnen Computeknoten über *log_size* hinaus steigern würde.

## <a name="permissions"></a>Berechtigungen

Erfordert die `CREATE ANY DATABASE`-Berechtigung in der Masterdatenbank oder die Mitgliedschaft in der festen Serverrolle **sysadmin**.

Im folgenden Beispiel wird dem Datenbankbenutzer Fay die Berechtigung zum Erstellen einer Datenbank erteilt.

```sql
USE master;
GO
GRANT CREATE ANY DATABASE TO [Fay];
GO
```

## <a name="general-remarks"></a>Allgemeine Hinweise

Datenbanken werden mit dem Datenbank-Kompatibilitätsgrad 120, also dem Kompatibilitätsgrad für [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], erstellt. Dadurch wird sichergestellt, dass die Datenbank alle [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]-Funktionen verwenden kann, die PDW verwendet.

## <a name="limitations-and-restrictions"></a>Einschränkungen

Die CREATE DATABASE-Anweisung ist in einer expliziten Transaktion nicht zulässig. Weitere Informationen finden Sie unter [Transact-SQL-Anweisungen](../../t-sql/statements/statements.md).

Informationen zu minimalen und maximalen Einschränkungen für Datenbanken finden Sie unter „Minimum and Maximum Values“ (Minimale und maximale Werte) in der [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Bei der Erstellung der Datenbank muss genügend freier Speicherplatz *auf jedem Computeknoten* verfügbar sein, um den kombinierten Gesamtspeicherplatz der folgenden Größen zuzuweisen:

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank mit Tabellen mit einer Größe von *replicated_table_size*.
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank mit Tabellen mit einer Größe von (*distributed_table_size* / Anzahl von Computeknoten).
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Protokolle mit einer Größe von (*log_size* / Anzahl von Computeknoten).

## <a name="locking"></a>Sperren

Führt eine gemeinsame Sperre für das DATABASE-Objekt durch.

## <a name="metadata"></a>Metadaten

Nachdem dieser Vorgang erfolgreich abgeschlossen wurde, wird für diese Datenbank in den Metadatensichten [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) und [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) ein Eintrag angezeigt.

## <a name="examples-sspdw"></a>Beispiele: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="a-basic-database-creation-examples"></a>A. Beispiele für die Erstellung einer grundlegenden Datenbank

Im folgenden Beispiel wird die Datenbank `mytest` mit einem zugewiesenen Speicherplatz von 100 GB pro Computeknoten für replizierte Tabellen, 500 GB pro Appliance für verteilte Tabellen und 100 GB pro Appliance für das Transaktionsprotokoll erstellt. In diesem Beispiel ist die Standardeinstellung für AUTOGROW auf OFF festgelegt.

```sql
CREATE DATABASE mytest
  WITH
    (REPLICATED_SIZE = 100 GB,
    DISTRIBUTED_SIZE = 500 GB,
    LOG_SIZE = 100 GB );
```

Im folgenden Beispiel wird die Datenbank `mytest` mit den gleichen Parametern wie oben erstellt, wobei AUTOGROW auf ON festgelegt ist. Dadurch kann sich die Datenbank über die angegebenen Größenparameter hinweg vergrößern.

```sql
CREATE DATABASE mytest
  WITH
    (AUTOGROW = ON,
    REPLICATED_SIZE = 100 GB,
    DISTRIBUTED_SIZE = 500 GB,
    LOG_SIZE = 100 GB);
```

### <a name="b-creating-a-database-with-partial-gigabyte-sizes"></a>B. Erstellen einer Datenbank mit partiellen Gigabytegrößen

Im folgenden Beispiel wird die Datenbank `mytest`, für die AUTOGROW auf OFF festgelegt ist, mit einem zugewiesenen Speicherplatz von 1,5 GB pro Computeknoten für replizierte Tabellen, 5,25 GB pro Appliance für verteilte Tabellen und 10 GB pro Appliance für das Transaktionsprotokoll erstellt.

```sql
CREATE DATABASE mytest
  WITH
    (REPLICATED_SIZE = 1.5 GB,
    DISTRIBUTED_SIZE = 5.25 GB,
    LOG_SIZE = 10 GB);
```

## <a name="see-also"></a>Weitere Informationen

- [ALTER DATABASE - Analytics Platform System](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)

::: moniker-end