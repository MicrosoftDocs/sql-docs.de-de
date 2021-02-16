---
title: Konfigurieren von SQL Server-Einstellungen unter Linux
description: In diesem Artikel wird beschrieben, wie Sie das mssql-conf-Tool verwenden, um SQL Server-Einstellungen unter Linux zu konfigurieren.
author: VanMSFT
ms.author: vanto
ms.date: 08/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 06798dff-65c7-43e0-9ab3-ffb23374b322
ms.openlocfilehash: 6415d91152277fd2cc6541acb24a61cfcb057081
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338349"
---
# <a name="configure-sql-server-on-linux-with-the-mssql-conf-tool"></a>Konfigurieren von SQL Server für Linux mit dem mssql-conf-Tool

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

**mssql-conf** ist ein Konfigurationsskript, das mit SQL Server 2017 für Red Hat Enterprise Linux, SUSE Linux Enterprise Server und Ubuntu installiert wird. Es ändert die [**mssql.conf-Datei**](#mssql-conf-format), in der Konfigurationswerte gespeichert sind. Sie können das **mssql-conf**-Hilfsprogramm zum Festlegen der folgenden Parameter verwenden:

|Parameter|Beschreibung|
|---|---|
| [Agent](#agent) | Aktivieren des SQL Server-Agents. |
| [Sortierung](#collation) | Festlegen einer neuen Sortierung für SQL Server für Linux. |
| [Kundenfeedback](#customerfeedback) | Festlegen, ob SQL Server Feedback an Microsoft sendet. |
| [Datenbank-E-Mail-Profil](#dbmail) | Festlegen des Standardprofils von Datenbank-E-Mails für SQL Server für Linux. |
| [Standarddatenverzeichnis](#datadir) | Festlegen eines anderen Standardverzeichnisses für neue SQL Server-Datenbank-Datendateien (MDF). |
| [Standardprotokollverzeichnis](#datadir) | Festlegen eines anderen Standardverzeichnisses für neue SQL Server-Datenbank-Protokolldateien (LDF). |
| [Standardverzeichnis für Masterdatenbank](#masterdatabasedir) | Festlegen eines anderen Standardverzeichnisses für die Masterdatenbank und für Protokolldateien.|
| [Standarddateiname für Masterdatenbank](#masterdatabasename) | Festlegen eines anderen Namens für Masterdatenbank-Dateien. |
| [Standardverzeichnis für Speicherabbilder](#dumpdir) | Festlegen eines anderen Standardverzeichnisses für neue Speicherabbilder und andere Dateien zur Problembehandlung. |
| [Standardverzeichnis für Fehlerprotokolle](#errorlogdir) | Festlegen eines anderen Standardverzeichnisses für neue SQL Server-Dateien für Fehlerprotokolle, Standardprofiler-Ablaufverfolgungen, XE-Systemintegritätssitzungen und Hekaton-XE-Sitzungen. |
| [Standardverzeichnis für Sicherungen](#backupdir) | Festlegen eines anderen Standardverzeichnisses für neue Sicherungsdateien. |
| [Typ des Speicherabbilds](#coredump) | Festlegen des Typs der Speicherabbilddateien, die erfasst werden sollen. |
| [Hochverfügbarkeit](#hadr) | Aktivieren von Verfügbarkeitsgruppen. |
| [Verzeichnis für lokale Überwachungen](#localaudit) | Festlegen eines Verzeichnisses, dem Dateien für lokale Überwachungen hinzugefügt werden sollen. |
| [Gebietsschema](#lcid) | Festlegen eines Gebietsschemas, das SQL Server verwenden soll. |
| [Arbeitsspeicherlimit](#memorylimit) | Festlegen des Arbeitsspeicherlimits für SQL Server. |
| [Netzwerkeinstellungen](#network) | Zusätzliche Netzwerkeinstellungen für SQL Server. |
| [Microsoft Distributed Transaction Coordinator](#msdtc) | Konfigurieren von MS DTC unter Linux und Durchführen einer Problembehandlung. |
| [TCP-Port](#tcpport) | Festlegen eines anderen Ports, auf dem SQL Server nach Verbindungen lauscht. |
| [TLS](#tls) | Konfigurieren der Verschlüsselung mit Transport Layer Security. |
| [Ablaufverfolgungsflags](#traceflags) | Festlegen der Ablaufverfolgungsflags, die vom Dienst verwendet werden sollen. |

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

**mssql-conf** ist ein Konfigurationsskript, das mit [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] für Red Hat Enterprise Linux, SUSE Linux Enterprise Server und Ubuntu installiert wird. Mit diesem Hilfsprogramm können Sie die folgenden Parameter festlegen:

|Parameter|Beschreibung|
|---|---|
| [Agent](#agent) | Aktivieren des SQL Server-Agents. |
| [Sortierung](#collation) | Festlegen einer neuen Sortierung für SQL Server für Linux. |
| [Kundenfeedback](#customerfeedback) | Festlegen, ob SQL Server Feedback an Microsoft sendet. |
| [Datenbank-E-Mail-Profil](#dbmail) | Festlegen des Standardprofils von Datenbank-E-Mails für SQL Server für Linux. |
| [Standarddatenverzeichnis](#datadir) | Festlegen eines anderen Standardverzeichnisses für neue SQL Server-Datenbank-Datendateien (MDF). |
| [Standardprotokollverzeichnis](#datadir) | Festlegen eines anderen Standardverzeichnisses für neue SQL Server-Datenbank-Protokolldateien (LDF). |
| [Standardverzeichnis für Masterdatenbank-Dateien](#masterdatabasedir) | Festlegen eines anderen Standardverzeichnisses für die Masterdatenbank-Dateien auf einer vorhandenen SQL-Installation.|
| [Standarddateiname für Masterdatenbank](#masterdatabasename) | Festlegen eines anderen Namens für Masterdatenbank-Dateien. |
| [Standardverzeichnis für Speicherabbilder](#dumpdir) | Festlegen eines anderen Standardverzeichnisses für neue Speicherabbilder und andere Dateien zur Problembehandlung. |
| [Standardverzeichnis für Fehlerprotokolle](#errorlogdir) | Festlegen eines anderen Standardverzeichnisses für neue SQL Server-Dateien für Fehlerprotokolle, Standardprofiler-Ablaufverfolgungen, XE-Systemintegritätssitzungen und Hekaton-XE-Sitzungen. |
| [Standardverzeichnis für Sicherungen](#backupdir) | Festlegen eines anderen Standardverzeichnisses für neue Sicherungsdateien. |
| [Typ des Speicherabbilds](#coredump) | Festlegen des Typs der Speicherabbilddateien, die erfasst werden sollen. |
| [Hochverfügbarkeit](#hadr) | Aktivieren von Verfügbarkeitsgruppen. |
| [Verzeichnis für lokale Überwachungen](#localaudit) | Festlegen eines Verzeichnisses, dem Dateien für lokale Überwachungen hinzugefügt werden sollen. |
| [Gebietsschema](#lcid) | Festlegen eines Gebietsschemas, das SQL Server verwenden soll. |
| [Arbeitsspeicherlimit](#memorylimit) | Festlegen des Arbeitsspeicherlimits für SQL Server. |
| [Microsoft Distributed Transaction Coordinator](#msdtc) | Konfigurieren von MS DTC unter Linux und Durchführen einer Problembehandlung. |
| [Lizenzbedingungen für ML Services](#mlservices-eula) | Festlegen, dass R- und Python-bezogene Lizenzbedingungen für mlservices-Pakete akzeptiert werden. Gilt nur für SQL Server 2019.|
| [Netzwerkeinstellungen](#network) | Zusätzliche Netzwerkeinstellungen für SQL Server. |
| [outboundnetworkaccess](#mlservices-outbound-access) |Aktivieren des Zugriffs auf ausgehenden Netzwerkdatenverkehr für R-, Python- und Java-Erweiterungen von [mlservices](sql-server-linux-setup-machine-learning.md).|
| [TCP-Port](#tcpport) | Festlegen eines anderen Ports, auf dem SQL Server nach Verbindungen lauscht. |
| [TLS](#tls) | Konfigurieren der Verschlüsselung mit Transport Layer Security. |
| [Ablaufverfolgungsflags](#traceflags) | Festlegen der Ablaufverfolgungsflags, die vom Dienst verwendet werden sollen. |

::: moniker-end

> [!TIP]
> Einige dieser Einstellungen können auch mit Umgebungsvariablen konfiguriert werden. Weitere Informationen finden Sie unter [Konfigurieren von SQL Server-Einstellungen mit Umgebungsvariablen](sql-server-linux-configure-environment-variables.md).

## <a name="usage-tips"></a>Verwendungstipps

* Nehmen Sie für Always On-Verfügbarkeitsgruppen und Cluster mit gemeinsam genutzten Datenträgern immer dieselben Konfigurationsänderungen an jedem Knoten vor.

* Versuchen Sie bei Clustern mit gemeinsam genutzten Datenträgern nicht, den **mssql-server**-Dienst neu zu starten, um die Änderungen zu übernehmen. SQL Server wird als Anwendung ausgeführt. Versetzen Sie die Ressource stattdessen zuerst in den Offline- und anschließend wieder in den Onlinezustand.

* In den Beispielen wird für die Ausführung von mssql-conf der vollständige Pfad **/opt/mssql/bin/mssql-conf** verwendet. Wenn Sie zu diesem Pfad navigieren, müssen Sie mssql-conf im aktuellen Verzeichnis **./mssql-conf** ausführen.

## <a name="enable-sql-server-agent"></a><a id="agent"></a> Aktivieren des SQL Server-Agents

Durch die Einstellung **sqlagent.enabled** wird der [SQL Server-Agent](sql-server-linux-run-sql-server-agent-job.md) aktiviert. Standardmäßig ist der SQL Server-Agent deaktiviert. Wenn **sqlagent.enabled** nicht in der Einstellungsdatei „mssql.conf“ vorhanden ist, wird von SQL Server intern angenommen, dass der SQL Server-Agent deaktiviert ist.

Führen Sie die folgenden Schritte aus, um diese Einstellung zu ändern:

1. Aktivieren Sie den SQL Server-Agent:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
   ```

2. Starten Sie den SQL Server-Dienst neu:

   ```bash
   sudo systemctl restart mssql-server
   ```

### <a name="set-the-default-database-mail-profile-for-sql-server-on-linux"></a><a id="dbmail"></a> Festlegen des Standardprofils von Datenbank-E-Mails für SQL Server für Linux

Mit der Einstellung **sqlpagent.databasemailprofile** können Sie das Standardprofil von Datenbank-E-Mails für E-Mail-Benachrichtigungen festlegen.

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.databasemailprofile <profile_name>
```

### <a name="sql-agent-error-logs"></a><a id="agenterrorlog"></a> SQL-Agent-Fehlerprotokolle

Mit den Einstellungen **sqlpagent.errorlogfile** und **sqlpagent.errorlogginglevel** können Sie den Protokolldateipfad und Protokolliergrad für den SQL-Agent angeben. 

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.errorfile <path>
```

SQL-Agent-Protokolliergrade sind folgende Bitmaskenwerte:

- 1 = Fehler
- 2 = Warnungen
- 4 = Info

Wenn Sie alle Grade erfassen möchten, verwenden Sie `7` als Wert.

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.errorlogginglevel <level>
```

## <a name="change-the-sql-server-collation"></a><a id="collation"></a> Ändern der SQL Server-Sortierung

Mit der Option **set-collation** kann ein anderer unterstützter Sortierungswert angegeben werden.

1. [Sichern Sie zuerst alle Benutzerdatenbanken](sql-server-linux-backup-and-restore-database.md) auf Ihrem Server.

1. Verwenden Sie dann die gespeicherte Prozedur [sp_detach_db](../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md), um die Benutzerdatenbanken zu trennen.

1. Führen Sie die Option **set-collation** aus, und folgen Sie den Anweisungen:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set-collation
   ```

1. Das mssql-conf-Hilfsprogramm versucht, den angegebenen Sortierungswert festzulegen und den Dienst neu zu starten. Wenn Fehler auftreten, wird ein Rollback ausgeführt, und die Sortierung wird auf den vorherigen Wert zurückgesetzt.

1. Stellen Sie die Sicherungen der Benutzerdatenbanken wieder her.

Führen Sie die [sys.fn_helpcollations](../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)-Funktion aus, um eine Liste der unterstützten Sortierungen anzuzeigen: `SELECT Name from sys.fn_helpcollations()`.

## <a name="configure-customer-feedback"></a><a id="customerfeedback"></a> Konfigurieren von Einstellungen für Kundenfeedback

Mit der Einstellung **telemetry.customerfeedback** können Sie festlegen, ob SQL Server Feedback an Microsoft sendet. Standardmäßig ist dieser Wert für alle Editionen auf **true** festgelegt. Führen Sie die folgenden Befehle aus, um den Wert zu ändern:

> [!IMPORTANT]
> Sie können das Kundenfeedback für kostenlose Editionen von SQL Server, Express und Developer nicht deaktivieren.

1. Führen Sie das mssql-conf-Skript als Rootbenutzer mit dem Befehl **set** für **telemetry.customerfeedback** aus. Im folgenden Beispiel wird das Kundenfeedback durch die Angabe von **false** deaktiviert.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.customerfeedback false
   ```

1. Starten Sie den SQL Server-Dienst neu:

   ```bash
   sudo systemctl restart mssql-server
   ```

Weitere Informationen finden Sie unter [Kundenfeedback für SQL Server für Linux](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md) und [SQL Server-Datenschutzbestimmungen](../sql-server/sql-server-privacy.md).

## <a name="change-the-default-data-or-log-directory-location"></a><a id="datadir"></a> Ändern des Standardspeicherorts für das Verzeichnis der Datenbank- oder Protokolldateien

Mit den Einstellungen **filelocation.defaultdatadir** und **filelocation.defaultlogdir** können Sie den Speicherort festlegen, an dem die neuen Datenbank- und Protokolldateien erstellt werden. Der Standardspeicherort ist /var/opt/mssql/data. Führen Sie die folgenden Schritte aus, um diese Einstellungen zu ändern:

1. Erstellen Sie das Zielverzeichnis für neue Datenbank- und Protokolldateien. Im folgenden Beispiel wird das Verzeichnis **/tmp/data** erstellt:

   ```bash
   sudo mkdir /tmp/data
   ```

1. Ändern Sie Besitzer und Gruppe des Verzeichnisses in den **mssql**-Benutzer:

   ```bash
   sudo chown mssql /tmp/data
   sudo chgrp mssql /tmp/data
   ```

1. Verwenden Sie mssql-conf mit dem **set**-Befehl, um das Standarddatenverzeichnis zu ändern:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultdatadir /tmp/data
   ```

1. Starten Sie den SQL Server-Dienst neu:

   ```bash
   sudo systemctl restart mssql-server
   ```

1. Nun werden alle Datenbankdateien für neu erstellte Datenbanken an diesem Speicherort gespeichert. Wenn Sie für die Protokolldateien (LDF) der neuen Datenbanken einen anderen Speicherort festlegen möchten, können Sie den folgenden „set“-Befehl nutzen:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultlogdir /tmp/log
   ```

1. Auch dieser Befehl setzt voraus, dass das Verzeichnis /tmp/log vorhanden ist und dem Benutzer sowie der Gruppe **mssql** zugeordnet ist.


## <a name="change-the-default-master-database-file-directory-location"></a><a id="masterdatabasedir"></a> Ändern des Standardspeicherorts für das Verzeichnis der Masterdatenbank-Dateien

Mit der Einstellung **filelocation.masterdatafile** und **filelocation.masterlogfile** können Sie einen anderen Speicherort festlegen, an dem die SQL Server-Engine nach Masterdatenbank-Dateien sucht. Der Standardspeicherort ist /var/opt/mssql/data.

Führen Sie die folgenden Schritte aus, um diese Einstellungen zu ändern:

1. Erstellen Sie das Zielverzeichnis für neue Fehlerprotokolldateien. Im folgenden Beispiel wird das Verzeichnis **/tmp/masterdatabasedir** erstellt:

   ```bash
   sudo mkdir /tmp/masterdatabasedir
   ```

1. Ändern Sie Besitzer und Gruppe des Verzeichnisses in den **mssql**-Benutzer:

   ```bash
   sudo chown mssql /tmp/masterdatabasedir
   sudo chgrp mssql /tmp/masterdatabasedir
   ```

1. Verwenden Sie mssql-conf mit dem **set**-Befehl, um das Standardverzeichnis der Masterdatenbank für die Masterdaten und Protokolldateien zu ändern:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterdatafile /tmp/masterdatabasedir/master.mdf
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterlogfile /tmp/masterdatabasedir/mastlog.ldf
   ```

   > [!NOTE]
   > Dadurch werden nicht nur die Masterdaten und Protokolldateien, sondern auch das Standardverzeichnis für alle anderen Systemdatenbanken verschoben.

1. Halten Sie den SQL Server-Dienst an:

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Verschieben Sie die Dateien „master.mdf“ und „masterlog.ldf“:

   ```bash
   sudo mv /var/opt/mssql/data/master.mdf /tmp/masterdatabasedir/master.mdf 
   sudo mv /var/opt/mssql/data/mastlog.ldf /tmp/masterdatabasedir/mastlog.ldf
   ```

1. Starten Sie den SQL Server-Dienst:

   ```bash
   sudo systemctl start mssql-server
   ```

   > [!NOTE]
   > Wenn SQL Server die Dateien „master.mdf“ und „mastlog.ldf“ nicht im angegebenen Verzeichnis findet, wird eine auf Vorlagen basierende Kopie der Systemdatenbanken automatisch im angegebenen Verzeichnis erstellt. Anschließend wird SQL Server gestartet. Metadaten wie Benutzerdatenbanken, Serveranmeldungen, Serverzertifikate, Verschlüsselungsschlüssel, SQL Server-Agent-Aufträge oder alte Systemadministrator-Anmeldekennwörter werden jedoch nicht in der neuen Masterdatenbank aktualisiert. Wenn Sie die vorhandenen Metadaten weiterhin verwenden möchten, müssen Sie SQL Server anhalten, die alten Dateien „master.mdf“ und „mastlog.ldf“ an den angegebenen Speicherort verschieben und SQL Server anschließend neu starten.
 
## <a name="change-the-name-of-master-database-files"></a><a id="masterdatabasename"></a> Ändern der Masterdatenbank-Dateinamen

Mit der Einstellung **filelocation.masterdatafile** und **filelocation.masterlogfile** können Sie einen anderen Speicherort festlegen, an dem die SQL Server-Engine nach Masterdatenbank-Dateien sucht. Außerdem können Sie damit den Namen der Masterdatenbank- und Protokolldateien ändern. 

Führen Sie die folgenden Schritte aus, um diese Einstellungen zu ändern:

1. Halten Sie den SQL Server-Dienst an:

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Verwenden Sie mssql-conf mit dem **set**-Befehl, um die Masterdatenbank-Namen für die Masterdaten und Protokolldateien zu ändern:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.masterdatafile /var/opt/mssql/data/masternew.mdf
   sudo /opt/mssql/bin/mssql-conf set filelocation.mastlogfile /var/opt/mssql/data/mastlognew.ldf
   ```

   > [!IMPORTANT]
   > Sie können den Namen der Masterdatenbank- und Protokolldateien erst ändern, nachdem SQL Server gestartet wurde. Vor der ersten Ausführung wird von SQL Server vorausgesetzt, dass die Dateien „master.mdf“ und „mastlog.ldf“ heißen.

1. Ändern Sie den Namen der Masterdatenbank-Daten- und Protokolldateien: 

   ```bash
   sudo mv /var/opt/mssql/data/master.mdf /var/opt/mssql/data/masternew.mdf
   sudo mv /var/opt/mssql/data/mastlog.ldf /var/opt/mssql/data/mastlognew.ldf
   ```

1. Starten Sie den SQL Server-Dienst:

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="change-the-default-dump-directory-location"></a><a id="dumpdir"></a> Ändern des Standardspeicherorts für das Speicherabbildverzeichnis

Mit der Einstellung **filelocation.defaultdumpdir** können Sie einen anderen Standardspeicherort festlegen, an dem die Speicher- und SQL-Abbilder erstellt werden, wenn es zu einem Absturz kommt. Standardmäßig werden diese Dateien im Verzeichnis /var/opt/mssql/log erstellt.

Verwenden Sie die folgenden Befehle, um einen neuen Speicherort festzulegen:

1. Erstellen Sie das Zielverzeichnis für neue Speicherabbilddateien. Im folgenden Beispiel wird das Verzeichnis **/tmp/dump** erstellt:

   ```bash
   sudo mkdir /tmp/dump
   ```

1. Ändern Sie Besitzer und Gruppe des Verzeichnisses in den **mssql**-Benutzer:

   ```bash
   sudo chown mssql /tmp/dump
   sudo chgrp mssql /tmp/dump
   ```

1. Verwenden Sie mssql-conf mit dem **set**-Befehl, um das Standarddatenverzeichnis zu ändern:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultdumpdir /tmp/dump
   ```

1. Starten Sie den SQL Server-Dienst neu:

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="change-the-default-error-log-file-directory-location"></a><a id="errorlogdir"></a> Ändern des Standardspeicherorts für das Verzeichnis der Fehlerprotokolldateien

Mit der Einstellung **filelocation.errorlogfile** können Sie einen anderen Speicherort festlegen, an dem Dateien für Fehlerprotokolle, Standardprofiler-Ablaufverfolgungen, XE-Systemintegritätssitzungen und Hekaton-XE-Sitzungen erstellt werden. Der Standardspeicherort ist /var/opt/mssql/log. Das Verzeichnis, in dem die SQL-Fehlerprotokolldatei abgelegt wird, wird zum Standardverzeichnis für andere Protokolle.

Gehen Sie wie folgt vor, um diese Einstellungen zu ändern:

1. Erstellen Sie das Zielverzeichnis für neue Fehlerprotokolldateien. Im folgenden Beispiel wird das Verzeichnis **/tmp/logs** erstellt:

   ```bash
   sudo mkdir /tmp/logs
   ```

1. Ändern Sie Besitzer und Gruppe des Verzeichnisses in den **mssql**-Benutzer:

   ```bash
   sudo chown mssql /tmp/logs
   sudo chgrp mssql /tmp/logs
   ```

1. Verwenden Sie mssql-conf mit dem **set**-Befehl, um den Standardnamen für Fehlerprotokolle zu ändern:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.errorlogfile /tmp/logs/errorlog
   ```

1. Starten Sie den SQL Server-Dienst neu:

   ```bash
   sudo systemctl restart mssql-server
   ```

Mit der Einstellung `errorlog.numerrorlogs` können Sie angeben, wie viele Fehlerprotokolle beibehalten werden sollen, bevor das Protokoll neu gestartet wird.

## <a name="change-the-default-backup-directory-location"></a><a id="backupdir"></a> Ändern des Standardspeicherorts für das Sicherungsverzeichnis

Mit der Einstellung **filelocation.defaultbackupdir** können Sie einen anderen Standardspeicherort festlegen, an dem Sicherungsdateien erstellt werden. Standardmäßig werden diese Dateien im Verzeichnis /var/opt/mssql/data erstellt.

Verwenden Sie die folgenden Befehle, um einen neuen Speicherort festzulegen:

1. Erstellen Sie das Zielverzeichnis für neue Sicherungsdateien. Im folgenden Beispiel wird das Verzeichnis **/tmp/backup** erstellt:

   ```bash
   sudo mkdir /tmp/backup
   ```

1. Ändern Sie Besitzer und Gruppe des Verzeichnisses in den **mssql**-Benutzer:

   ```bash
   sudo chown mssql /tmp/backup
   sudo chgrp mssql /tmp/backup
   ```

1. Verwenden Sie mssql-conf mit dem „set“-Befehl, um das Standardsicherungsverzeichnis zu ändern:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set filelocation.defaultbackupdir /tmp/backup
   ```

1. Starten Sie den SQL Server-Dienst neu:

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="specify-core-dump-settings"></a><a id="coredump"></a> Festlegen der Einstellungen für Kernspeicherabbilder

Wenn in einem SQL Server-Prozess eine Ausnahme auftritt, erstellt SQL Server ein Speicherabbild.

Sie können mit den Optionen **coredump.coredumptype** und **coredump.captureminiandfull** zwei Typen von Speicherabbildern festlegen, die von SQL Server erfasst werden. Diese Optionen beziehen sich auf die beiden Phasen zur Erfassung von Kernspeicherabbildern. 

Die Erfassung in der ersten Phase wird durch die Einstellung **coredump.coredumptype** gesteuert. Mit dieser wird der Typ der Speicherabbilddatei festgelegt, die bei einer Ausnahme erstellt wird. Die zweite Phase wird durch die Einstellung **coredump.captureminiandfull** aktiviert. Wenn **coredump.captureminiandfull** auf „true“ festgelegt ist, werden die durch **coredump.coredumptype** angegebene Speicherabbilddatei und ein zweites Minimalspeicherabbild erstellt. Wenn Sie **coredump.captureminiandfull** auf „false“ festlegen, wird die zweite Erfassungsphase deaktiviert.

1. Mit der Einstellung **coredump.captureminiandfull** können Sie festlegen, ob sowohl Minimalspeicherabbilder als auch vollständige Speicherabbilder erfasst werden.

    ```bash
    sudo /opt/mssql/bin/mssql-conf set coredump.captureminiandfull <true or false>
    ```

    Die Standardeinstellung ist **false**.

1. Mit der Einstellung **coredump.coredumptype** können Sie den Typ der Speicherabbilddatei angeben.

    ```bash
    sudo /opt/mssql/bin/mssql-conf set coredump.coredumptype <dump_type>
    ```

    Die Standardeinstellung ist **miniplus**.

    In der folgenden Tabelle sind die möglichen Werte für **coredump.coredumptype** aufgeführt.

    | type | BESCHREIBUNG |
    |-----|-----|
    | **mini** | „mini“ ist der kleinste Speicherabbild-Dateityp. Mithilfe von Linux-Systeminformationen werden die Threads und Module eines Prozesses ermittelt. Das Speicherabbild enthält nur die Threadstapel und Module der Hostumgebung. Es umfasst keine indirekten Speicherverweise oder globale Variablen. |
    | **miniplus** | „miniplus“ ist mit „mini“ vergleichbar, jedoch werden zusätzliche Speicherbereiche erfasst. Interne Vorgänge in SQL PAL und in der Hostumgebung werden berücksichtigt, wodurch dem Speicherabbild mehrere Speicherbereiche hinzugefügt werden. Diese betreffen:</br></br> – verschiedene globale Variablen</br> – sämtlichen Speicher über 64 TB</br> – benannte Bereiche in **/proc/$pid/maps**</br> – indirekten Arbeitsspeicher aus Threads und Stapeln</br> – Threadinformationen</br> – zugeordnete TEBs und PEBs</br> – Modulinformationen</br> – VMM und die VAD-Struktur |
    | **filtered** | Für „filtered“ wird ein subtraktionsbasierter Entwurf verwendet, bei dem sämtlicher Speicher im Prozess erfasst wird, falls er nicht explizit ausgeschlossen wird. Interne Vorgänge in SQL PAL und in der Hostumgebung werden berücksichtigt. Bestimmte Bereiche werden jedoch nicht im Speicherabbild erfasst.
    | **full** | Durch „full“ wird ein vollständiges Prozessspeicherabbild erstellt, das sämtliche Bereiche in **/proc/$pid/maps** einschließt. Dieser Vorgang wird nicht durch die Einstellung **coredump.captureminiandfull** gesteuert. |

## <a name="high-availability"></a><a id="hadr"></a> Hochverfügbarkeit

Mit der Option **hadr.hadrenabled** können Sie Verfügbarkeitsgruppen auf Ihrer SQL Server-Instanz aktivieren. Mit dem folgenden Befehl wird die Einstellung **hadr.hadrenabled** auf 1 festgelegt, wodurch Verfügbarkeitsgruppen aktiviert werden. Sie müssen SQL Server neu starten, damit die Einstellung übernommen wird.

```bash
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled  1
sudo systemctl restart mssql-server
```

Wie Sie diese Einstellung für Verfügbarkeitsgruppen nutzen, wird in den folgenden beiden Artikeln beschrieben:

- [Konfigurieren von Always On-Verfügbarkeitsgruppen für SQL Server für Linux](sql-server-linux-availability-group-configure-ha.md)
- [Konfigurieren von Leseskalierungs-Verfügbarkeitsgruppen für SQL Server für Linux](sql-server-linux-availability-group-configure-rs.md)


## <a name="set-local-audit-directory"></a><a id="localaudit"></a> Festlegen des Verzeichnisses für lokale Überwachungen

Mit der Einstellung **telemetry.userrequestedlocalauditdirectory** werden lokale Überwachungen aktiviert. Zusätzlich können Sie das Verzeichnis festlegen, in dem die Protokolle für lokale Überwachungen erstellt werden.

1. Erstellen Sie ein Zielverzeichnis für neue Protokolle von lokalen Überwachungen. Im folgenden Beispiel wird das Verzeichnis **/tmp/audit** erstellt:

   ```bash
   sudo mkdir /tmp/audit
   ```

1. Ändern Sie Besitzer und Gruppe des Verzeichnisses in den **mssql**-Benutzer:

   ```bash
   sudo chown mssql /tmp/audit
   sudo chgrp mssql /tmp/audit
   ```

1. Führen Sie das mssql-conf-Skript als Rootbenutzer mit dem **set**-Befehl für **telemetry.userrequestedlocalauditdirectory** aus:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set telemetry.userrequestedlocalauditdirectory /tmp/audit
   ```

1. Starten Sie den SQL Server-Dienst neu:

   ```bash
   sudo systemctl restart mssql-server
   ```

Weitere Informationen finden Sie unter [Kundenfeedback für SQL Server für Linux](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md).

## <a name="change-the-sql-server-locale"></a><a id="lcid"></a> Ändern des SQL Server-Gebietsschemas

Mit der Einstellung **language.lcid** können Sie ein anderes SQL Server-Gebietsschema festlegen. Dazu nutzen geben Sie einen der unterstützten Gebietsschemabezeichner (LCID) an. 

1. Im folgenden Beispiel wird Französisch (1036) als Gebietsschema festgelegt:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set language.lcid 1036
   ```

1. Starten Sie den SQL Server-Dienst neu, damit die Änderungen übernommen werden:

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="set-the-memory-limit"></a><a id="memorylimit"></a> Festlegen des Arbeitsspeicherlimits

Mit der Einstellung **memory.memorylimitmb** können Sie festlegen, wie viel verfügbarer physischer Speicher (in MB) für SQL Server bereitgestellt wird. Der Standardwert ist 80 % des physischen Speichers.

1. Führen Sie das mssql-conf-Skript als Rootbenutzer mit dem Befehl **set** für **memory.memorylimitmb** aus. Im folgenden Beispiel wird als verfügbarer Arbeitsspeicher für SQL Server 3,25 GB (3.328 MB) festgelegt.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set memory.memorylimitmb 3328
   ```

1. Starten Sie den SQL Server-Dienst neu, damit die Änderungen übernommen werden:

   ```bash
   sudo systemctl restart mssql-server
   ```

### <a name="additional-memory-settings"></a>Zusätzliche Arbeitsspeichereinstellungen

Für die Arbeitsspeichereinstellungen stehen die folgenden Optionen zur Verfügung.

|Option |BESCHREIBUNG |
|--- |--- |
| memory.disablememorypressure | Deaktivieren Sie die SQL Server-Arbeitsspeicherauslastung. Die Werte können `true` oder `false` lauten. |
| memory.memory_optimized | Aktivieren oder deaktivieren Sie arbeitsspeicheroptimierte SQL Server-Features: Aufklärung für persistente Arbeitsspeicherdateien, Arbeitsspeicherschutz. Die Werte können `true` oder `false` lauten. |

## <a name="configure-msdtc"></a><a id="msdtc"></a> Konfigurieren von MS DTC

Mit den Einstellungen **network.rpcport** und **distributedtransaction.servertcpport** können Sie den Microsoft Distributed Transaction Coordinator (MS DTC) konfigurieren. Führen Sie die folgenden Befehle aus, um diese Einstellungen zu ändern:

1. Führen Sie das mssql-conf-Skript als Rootbenutzer mit dem Befehl **set** für „network.rpcport“ aus:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.rpcport <rcp_port>
   ```

2. Legen Sie anschließend die Einstellung „distributedtransaction.servertcpport“ fest:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set distributedtransaction.servertcpport <servertcpport_port>
   ```

Zusätzlich zum Festlegen dieser Werte müssen Sie auch das Routing konfigurieren und die Firewall für Port 135 aktualisieren. Weitere Informationen hierzu finden Sie unter [Konfigurieren von MS DTC unter Linux.](sql-server-linux-configure-msdtc.md)

Für mssql-conf sind einige weitere Einstellungen verfügbar, mit denen Sie MS DTC überwachen und eine Problembehandlung durchführen können. Diese Einstellungen werden in der folgenden Tabelle kurz beschrieben. Weitere Informationen zu deren Verwendung finden Sie im Windows-Supportartikel [Aktivieren der Diagnoseablaufverfolgungen für MS DTC](https://support.microsoft.com/help/926099/how-to-enable-diagnostic-tracing-for-ms-dtc-on-a-windows-based-compute).

| mssql-conf-Einstellung | BESCHREIBUNG |
|---|---|
| distributedtransaction.allowonlysecurerpccalls | Konfigurieren von sicheren RPC-Aufrufen für verteilte Transaktionen. |
| distributedtransaction.fallbacktounsecurerpcifnecessary | Konfigurieren von sicheren RPC-Aufrufen für verteilte Transaktionen |
| distributedtransaction.maxlogsize | Dateigröße des DTC-Transaktionsprotokolls in MB. Der Standardwert ist 64 MB. |
| distributedtransaction.memorybuffersize | Puffergröße des Ringspeichers, in dem Ablaufverfolgungen gespeichert werden. Die Größe wird in MB angegeben. Der Standardwert ist 10 MB. |
| distributedtransaction.servertcpport | Port des MS DTC-RPC-Servers. |
| distributedtransaction.trace_cm | Ablaufverfolgungen im Verbindungs-Manager. |
| distributedtransaction.trace_contact | Ablaufverfolgungen für den Kontaktpool und für Kontakte. |
| distributedtransaction.trace_gateway | Ablaufverfolgungen; Gatewayquelle |
| distributedtransaction.trace_log | Protokollablaufverfolgung. |
| distributedtransaction.trace_misc | Ablaufverfolgungen, die nicht in die anderen Kategorien fallen. |
| distributedtransaction.trace_proxy | Ablaufverfolgungen, die im MS DTC-Proxy generiert werden. |
| distributedtransaction.trace_svc | Ablaufverfolgungen für den Start der EXE-Datei und des Diensts |
| distributedtransaction.trace_trace | Die eigentliche Ablaufverfolgungsinfrastruktur. |
| distributedtransaction.trace_util | Für Ablaufverfolgungen verwendete Hilfsroutinen, die von unterschiedlichen Speicherorten aus aufgerufen werden. |
| distributedtransaction.trace_xa | Ablaufverfolgungsquelle für XA-Transaktions-Manager (XATM). |
| distributedtransaction.tracefilepath | Ordner, in dem Ablaufverfolgungsdateien gespeichert werden sollen. |
| distributedtransaction.turnoffrpcsecurity | Aktivieren oder Deaktivieren der RPC-Sicherheit für verteilte Transaktionen. |

::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="accept-mlservices-eulas"></a><a id="mlservices-eula"></a> Akzeptieren der Lizenzbedingungen für ML Services

Wenn Sie der Datenbank-Engine [R- oder Python-Pakete für maschinelles Lernen](sql-server-linux-setup-machine-learning.md) hinzufügen möchten, müssen Sie die Lizenzbedingungen für die Open-Source-Verteilungen von R und Python akzeptieren. In der folgenden Tabelle werden alle verfügbaren Befehle oder Optionen aufgelistet, die sich auf die mlservices-Lizenzbedingungen beziehen. Für R und Python wird je nach Installation der gleiche Lizenzbedingungsparameter verwendet.

```bash
# For all packages: database engine and mlservices
# Setup prompts for mlservices EULAs, which you need to accept
sudo /opt/mssql/bin/mssql-conf setup

# Add R or Python to an existing installation
sudo /opt/mssql/bin/mssql-conf setup accept-eula-ml

# Alternative valid syntax
# Adds the EULA section to the INI and sets acceptulam to yes
sudo /opt/mssql/bin/mssql-conf set EULA accepteulaml Y

# Rescind EULA acceptance and removes the setting
sudo /opt/mssql/bin/mssql-conf unset EULA accepteulaml
```

In der Datei [mssql.conf](#mssql-conf-format) können Sie direkt festlegen, dass die Lizenzbedingungen akzeptiert werden:

```ini
[EULA]
accepteula = Y
accepteulaml = Y
```
:::moniker-end
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="enable-outbound-network-access"></a><a id="mlservices-outbound-access"></a> Aktivieren des Zugriffs auf ausgehenden Netzwerkdatenverkehr

In [SQL Server Machine Learning Services](sql-server-linux-setup-machine-learning.md) haben R-, Python- und Java-Erweiterungen standardmäßig keinen Zugriff auf ausgehenden Netzwerkdatenverkehr. Sie können mit mssql-conf die boolesche Eigenschaft „outboundnetworkaccess“ festlegen, um ausgehende Anforderungen zu aktivieren.

Starten Sie anschließend das SQL Server-Launchpad neu, um die aktualisierten Werte aus der INI-Datei zu lesen. Wenn eine Erweiterbarkeitseinstellung geändert wird, werden Sie in einer Neustartmeldung darauf hingewiesen.

```bash
# Adds the extensibility section and property.
# Sets "outboundnetworkaccess" to true.
# This setting is required if you want to access data or operations off the server.
sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 1

# Turns off network access but preserves the setting
sudo /opt/mssql/bin/mssql-conf set extensibility outboundnetworkaccess 0

# Removes the setting and rescinds network access
sudo /opt/mssql/bin/mssql-conf unset extensibility.outboundnetworkaccess
```

Sie können „outboundnetworkaccess“ auch direkt der Datei [mssql.conf](#mssql-conf-format) hinzufügen:

```ini
[extensibility]
outboundnetworkaccess = 1
```
:::moniker-end

## <a name="change-the-tcp-port"></a><a id="tcpport"></a> Ändern des TCP-Ports

Mit der Einstellung **network.tcpport** können Sie einen anderen TCP-Port festlegen, auf dem SQL Server nach Verbindungen lauscht. In der Standardeinstellung ist dies der Port 1433. Führen Sie die folgenden Befehle aus, um den Port zu ändern:

1. Führen Sie das mssql-conf-Skript als Rootbenutzer mit dem Befehl „set“ für „network.tcpport“ aus:

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.tcpport <new_tcp_port>
   ```

2. Starten Sie den SQL Server-Dienst neu:

   ```bash
   sudo systemctl restart mssql-server
   ```

3. Wenn Sie nun eine Verbindung mit SQL Server herstellen, müssen Sie nach dem Hostnamen oder der IP-Adresse den benutzerdefinierten Port angeben und ihn mit einem Komma abtrennen. Wenn Sie beispielsweise eine Verbindung mit sqlcmd herstellen möchten, müssen Sie den folgenden Befehl verwenden:

   ```bash
   sqlcmd -S localhost,<new_tcp_port> -U test -P test
   ```

## <a name="specify-tls-settings"></a><a id="tls"></a> Festlegen von TLS-Einstellungen

Mit den folgenden Optionen können Sie TLS für eine SQL Server-Instanz konfigurieren, die unter Linux ausgeführt wird.

|Option |BESCHREIBUNG |
|--- |--- |
|**network.forceencryption** |Wenn für die Option der Wert 1 festgelegt ist, erzwingt [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], dass alle Verbindungen verschlüsselt werden. Der Standardwert für diese Option ist 0. |
|**network.tlscert** |Der absolute Pfad zur Zertifikatdatei, die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für TLS verwendet. Beispiel:   `/etc/ssl/certs/mssql.pem`  Das mssql-Konto muss auf die Zertifikatdatei zugreifen können. Microsoft empfiehlt, den Zugriff auf die Datei mithilfe von `chown mssql:mssql <file>; chmod 400 <file>` einzuschränken. |
|**network.tlskey** |Der absolute Pfad zur Datei mit dem privaten Schlüssel, die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für TLS verwendet. Beispiel:  `/etc/ssl/private/mssql.key`  Das mssql-Konto muss auf die Zertifikatdatei zugreifen können. Microsoft empfiehlt, den Zugriff auf die Datei mithilfe von `chown mssql:mssql <file>; chmod 400 <file>` einzuschränken. |
|**network.tlsprotocols** |Eine durch Trennzeichen getrennte Liste mit TLS-Protokollen, die von SQL Server zugelassen werden. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] versucht immer, das sicherste Protokoll zu verwenden. Wenn ein Client keines der zulässigen Protokolle unterstützt, lehnt [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] den Verbindungsversuch ab.  Aus Kompatibilitätsgründen sind alle unterstützten Protokollversionen (1.2, 1.1, 1.0) standardmäßig zulässig.  Wenn die Clients TLS 1.2 unterstützen, empfiehlt Microsoft, nur diese Version zuzulassen. |
|**network.tlsciphers** |Mit dieser Einstellung können Sie festlegen, welche Verschlüsselungsverfahren von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für TLS zugelassen werden. Das Format für diese Zeichenfolge muss dem der [Liste der Verschlüsselungsverfahren für OpenSSL](https://www.openssl.org/docs/man1.0.2/apps/ciphers.html) entsprechen. In der Regel müssen Sie diese Option nicht ändern. <br /> Die folgenden Verschlüsselungsverfahren sind standardmäßig zulässig: <br /> `ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA` |
| **network.kerberoskeytabfile** |Pfad zur Kerberos-Schlüsseltabellendatei |

Ein Beispiel für die Verwendung der TLS-Einstellungen finden Sie unter [Verschlüsseln von SQL Server für Linux-Verbindungen](sql-server-linux-encrypted-connections.md).

## <a name="network-settings"></a><a id="network"></a> Netzwerkeinstellungen

Siehe [Tutorial: Verwenden der Active Directory-Authentifizierung mit SQL Server für Linux](sql-server-linux-active-directory-authentication.md), um umfassende Informationen zur Verwendung der AD-Authentifizierung mit SQL Server für Linux zu erhalten.

Die folgenden Optionen sind zusätzliche Netzwerkeinstellungen, die mit `mssql-conf` konfiguriert werden können.

|Option |BESCHREIBUNG |
|--- |--- |
| network.disablesssd | Hiermit wird die Abfrage von SSSD nach AD-Kontoinformationen deaktiviert, und es werden standardmäßig LDAP-Aufrufe verwendet. Die Werte können `true` oder `false` lauten. |
| network.enablekdcfromkrb5conf | Hiermit wird die Suche nach KDC-Informationen in „krb5.conf“ aktiviert. Die Werte können `true` oder `false` lauten. |
| network.forcesecureldap | Hiermit wird LDAPS zum Kontaktieren des Domänencontrollers erzwungen. Die Werte können `true` oder `false` lauten. |
| network.ipaddress | Die IP-Adresse für eingehende Verbindungen. |
| network.kerberoscredupdatefrequency | Die Zeit in Sekunden zwischen Überprüfungen auf Kerberos-Anmeldeinformationen, die aktualisiert werden müssen. Der Wert ist eine ganze Zahl.|
| network.privilegedadaccount | Privilegierter AD-Benutzer zur Verwendung für die AD-Authentifizierung. Der Wert lautet `<username>`. Weitere Informationen finden Sie im [Tutorial: Use Active Directory authentication with SQL Server on Linux (Tutorial: Verwenden der Azure Active Directory-Authentifizierung mit SQL Server unter Linux)](sql-server-linux-active-directory-authentication.md#spn)|
| uncmapping | Ordnet den UNC-Pfad einem lokalen Pfad zu. Beispiel: `sudo /opt/mssql/bin/mssql-conf set uncmapping //servername/sharename /tmp/folder`. |

## <a name="enabledisable-traceflags"></a><a id="traceflags"></a> Aktivieren oder Deaktivieren von Ablaufverfolgungsflags

Mit der Option **traceflag** können Sie Ablaufverfolgungsflags für den Start des SQL Server-Diensts aktivieren oder deaktivieren. Verwenden Sie die folgenden Befehle, um ein Ablaufverfolgungsflag zu aktivieren oder zu deaktivieren:

1. Aktivieren Sie mithilfe des folgenden Befehls ein Ablaufverfolgungsflag. Für das Ablaufverfolgungsflag 1234 können Sie beispielsweise wie folgt vorgehen:

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 1234 on
   ```

1. Sie können mehrere Ablaufverfolgungsflags aktivieren, indem Sie sie separat angeben:

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 2345 3456 on
   ```

1. Auf ähnliche Weise können Sie ein oder mehrere aktivierte Ablaufverfolgungsflags deaktivieren, indem Sie sie angeben und den **off**-Parameter ergänzen:

   ```bash
   sudo /opt/mssql/bin/mssql-conf traceflag 1234 2345 3456 off
   ```

1. Starten Sie den SQL Server-Dienst neu, damit die Änderungen übernommen werden:

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="remove-a-setting"></a>Löschen einer Einstellung

Sie können eine Einstellung löschen, die Sie mit `mssql-conf set` vorgenommen haben. Rufen Sie dazu **mssql-conf** mit der `unset`-Option und dem Namen der Einstellung auf. Dadurch wird die Einstellung auf den Standardwert zurückgesetzt.

1. Im folgenden Beispiel wird der Wert der Option **network.tcpport** gelöscht.

   ```bash
   sudo /opt/mssql/bin/mssql-conf unset network.tcpport
   ```

1. Starten Sie den SQL Server-Dienst neu.

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="view-current-settings"></a>Anzeigen der aktuellen Einstellungen

Sie können sich alle vorgenommen Einstellungen anzeigen lassen. Führen Sie dazu den folgenden Befehl aus, um die Inhalte der Datei **mssql.conf** auszugeben:

```bash
sudo cat /var/opt/mssql/mssql.conf
```

Für alle in dieser Datei nicht aufgeführten Einstellungen werden die Standardwerte verwendet. Der nächste Abschnitt enthält eine **mssql.conf**-Beispieldatei.


## <a name="mssqlconf-format"></a><a id="mssql-conf-format"></a> mssql.conf-Format

Die Datei **/var/opt/mssql/mssql.conf**, deren Inhalt weiter unten aufgeführt ist, enthält für jede Einstellung ein Beispiel. Sie können dieses Format verwenden, um bei Bedarf Änderungen an der Datei **mssql.conf** manuell vorzunehmen. Wenn Sie so vorgehen, müssen Sie SQL Server neu starten, damit die Änderungen übernommen werden. Wenn Sie die Datei **mssql.conf** mit Docker verwenden möchten, muss Docker [die Daten dauerhaft speichern](./sql-server-linux-docker-container-deployment.md). Fügen Sie dazu zuerst Ihrem Hostverzeichnis eine vollständige **mssql.conf**-Datei hinzu, und führen Sie dann den Container aus. Ein Beispiel hierfür finden Sie unter [Kundenfeedback](./usage-and-diagnostic-data-configuration-for-sql-server-linux.md).

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

```ini
[EULA]
accepteula = Y

[coredump]
captureminiandfull = true
coredumptype = full

[filelocation]
defaultbackupdir = /var/opt/mssql/data/
defaultdatadir = /var/opt/mssql/data/
defaultdumpdir = /var/opt/mssql/data/
defaultlogdir = /var/opt/mssql/data/

[hadr]
hadrenabled = 0

[language]
lcid = 1033

[memory]
memorylimitmb = 4096

[network]
forceencryption = 0
ipaddress = 10.192.0.0
kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
tcpport = 1401
tlscert = /etc/ssl/certs/mssql.pem
tlsciphers = ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA
tlskey = /etc/ssl/private/mssql.key
tlsprotocols = 1.2,1.1,1.0

[sqlagent]
databasemailprofile = default
errorlogfile = /var/opt/mssql/log/sqlagentlog.log
errorlogginglevel = 7

[telemetry]
customerfeedback = true
userrequestedlocalauditdirectory = /tmp/audit

[traceflag]
traceflag0 = 1204
traceflag1 = 2345
traceflag = 3456
```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

```ini
[EULA]
accepteula = Y
accepteulaml = Y

[coredump]
captureminiandfull = true
coredumptype = full

[distributedtransaction]
servertcpport = 51999

[filelocation]
defaultbackupdir = /var/opt/mssql/data/
defaultdatadir = /var/opt/mssql/data/
defaultdumpdir = /var/opt/mssql/data/
defaultlogdir = /var/opt/mssql/data/

[hadr]
hadrenabled = 0

[language]
lcid = 1033

[memory]
memorylimitmb = 4096

[network]
forceencryption = 0
ipaddress = 10.192.0.0
kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
rpcport = 13500
tcpport = 1401
tlscert = /etc/ssl/certs/mssql.pem
tlsciphers = ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA
tlskey = /etc/ssl/private/mssql.key
tlsprotocols = 1.2,1.1,1.0

[sqlagent]
databasemailprofile = default
errorlogfile = /var/opt/mssql/log/sqlagentlog.log
errorlogginglevel = 7

[telemetry]
customerfeedback = true
userrequestedlocalauditdirectory = /tmp/audit

[traceflag]
traceflag0 = 1204
traceflag1 = 2345
traceflag = 3456
```

::: moniker-end

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie einige dieser Konfigurationsänderungen mit Umgebungsvariablen vornehmen möchten, finden Sie unter [Konfigurieren von SQL Server-Einstellungen mit Umgebungsvariablen](sql-server-linux-configure-environment-variables.md) weitere Informationen.

Weitere Verwaltungstools und Szenarios finden Sie unter [Verwalten von SQL Server für Linux](sql-server-linux-management-overview.md).