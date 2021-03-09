---
title: Leitfaden für die Installation von SQL Server unter Linux
titleSuffix: SQL Server
description: Installieren, aktualisieren und deinstallieren Sie SQL Server unter Linux. In diesem Artikel erhalten Sie Informationen zu Online-, Offline- und unbeaufsichtigten Szenarios.
author: VanMSFT
ms.author: vanto
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: sqlfreshmay19
ms.technology: linux
ms.assetid: 565156c3-7256-4e63-aaf0-884522ef2a52
ms.openlocfilehash: 01417fa9f7f790229e073964133396f87f144d45
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465159"
---
# <a name="installation-guidance-for-sql-server-on-linux"></a>Leitfaden für die Installation von SQL Server unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel erfahren Sie, wie Sie SQL Server 2017 und SQL Server 2019 unter Linux installieren, aktualisieren und deinstallieren.

Informationen zu weiteren Bereitstellungsszenarios finden Sie unter:

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Docker-Container](./sql-server-linux-docker-container-deployment.md)
- [Kubernetes: Big Data-Cluster](../big-data-cluster/deploy-get-started.md)

> [!TIP]
> Dieser Leitfaden umfasst mehrere Bereitstellungsszenarios. Eine ausführliche Installationsanleitung mit allen erforderlichen Schritten finden Sie in den jeweiligen Schnellstarts:
> - [RHEL-Schnellstart](quickstart-install-connect-red-hat.md)
> - [SLES-Schnellstart](quickstart-install-connect-suse.md)
> - [Ubuntu-Schnellstart](quickstart-install-connect-ubuntu.md)
> - [Docker-Schnellstart](quickstart-install-connect-docker.md)

Antworten auf häufig gestellte Fragen finden Sie unter [Häufig gestellte Fragen zu SQL Server für Linux](../linux/sql-server-linux-faq.yml).

## <a name="supported-platforms"></a><a id="supportedplatforms"></a> Unterstützte Plattformen

SQL Server wird unter Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) und Ubuntu unterstützt. Es wird auch als Docker-Image unterstützt, das in der Docker-Engine unter Linux oder in Docker für Windows/Mac ausgeführt werden kann.

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

| Plattform | Unterstützte Version(en) | Herunterladen
|-----|-----|-----
| **Red Hat Enterprise Linux** | 7.3, 7.4, 7.5, 7.6, 8  | [RHEL 7.6 herunterladen](https://access.redhat.com/products/red-hat-enterprise-linux/evaluation)
| **SUSE Linux Enterprise Server** | v12 SP2 | [SLES v12 SP2 herunterladen](https://www.suse.com/products/server)
| **Ubuntu** | 16.04 | [Ubuntu 16.04 herunterladen](http://releases.ubuntu.com/xenial/)
| **Docker-Engine** | ab 1.8 | [Docker herunterladen](https://www.docker.com/get-started)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

[!INCLUDE [linux-supported-platfoms-2019](../includes/linux-supported-platfoms-2019.md)]

::: moniker-end

Microsoft unterstützt auch die Bereitstellung und Verwaltung von SQL Server-Containern mit OpenShift und Kubernetes.

> [!NOTE]
> SQL Server wird unter Linux für die oben aufgeführten Verteilungen getestet und unterstützt. Wenn Sie SQL Server unter einem nicht unterstützten Betriebssystem installieren möchten, finden Sie Informationen zu den Auswirkungen auf die Unterstützung unter [Technical support policy for Microsoft SQL Server (Richtlinie für den technischen Support für Microsoft SQL Server)](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server) im Abschnitt **Supportrichtlinie**.

## <a name="system-requirements"></a><a id="system"></a> Systemanforderungen

Für SQL Server gelten die folgenden Systemanforderungen unter Linux:

|||
|-----|-----|
| **Memory** | 2 GB |
| **Dateisystem** | **XFS** oder **EXT4** (andere Dateisysteme wie z. B. **BTRFS** werden nicht unterstützt) |
| **Speicherplatz** | 6 GB |
| **Prozessorgeschwindigkeit** | 2 GHz |
| **Prozessorkerne** | 2 Kerne |
| **Prozessortyp** | Nur x64-kompatibel |

Wenn Sie **NFS-Remotefreigaben (Network File System)** in der Produktion verwenden, beachten Sie die folgenden Anforderungen für die Unterstützung:

- Verwenden Sie NFS in der Version **4.2 oder höher**. Ältere NFS-Versionen unterstützen nicht die erforderlichen Features, wie das Erstellen von Fallocate- oder Sparsedateien, die häufig in modernen Dateisystemen enthalten sind.
- Verwenden Sie nur die Verzeichnisse vom Typ **/var/opt/mssql** in der NFS-Bereitstellung. Andere Dateien, wie z. B. die SQL Server-Systembinärdateien, werden nicht unterstützt.
- Stellen Sie sicher, dass NFS-Clients beim Einbinden der Remotefreigabe die Option „NOLOCK“ verwenden.

## <a name="configure-source-repositories"></a><a id="repositories"></a> Konfigurieren von Quellrepositorys

Beim Installieren oder Aktualisieren von SQL Server erhalten Sie die aktuelle SQL Server-Version aus Ihrem konfigurierten Microsoft-Repository. In den Schnellstarts wird das Update des **CU**-Repositorys für SQL Server verwendet. Sie können stattdessen jedoch ein **GDR**-Repository konfigurieren. Weitere Informationen zu Repositorys und deren Konfiguration finden Sie unter [Configure repositories for SQL Server on Linux (Konfigurieren von Repository für SQL Server unter Linux)](sql-server-linux-change-repo.md).

## <a name="install-sql-server"></a><a id="platforms"></a> Installieren von SQL Server

Sie können SQL Server 2017 oder SQL Server 2019 unter Linux über die Befehlszeile installieren. Eine ausführliche Anleitung mit den einzelnen Schritten finden Sie jeweils unter den folgenden Schnellstarts:

| Plattform | Installationsschnellstarts |
|---|---|
| Red Hat Enterprise Linux (RHEL) | [2017](quickstart-install-connect-red-hat.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-red-hat.md?view=sql-server-linux-ver15&preserve-view=true) |
| SUSE Linux Enterprise Server (SLES) | [2017](quickstart-install-connect-suse.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-suse.md?view=sql-server-linux-ver15&preserve-view=true) |
| Ubuntu | [2017](quickstart-install-connect-ubuntu.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-ubuntu.md?view=sql-server-linux-ver15&preserve-view=true) |
| Docker | [2017](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true) |

Sie können SQL Server für Linux auch auf einem virtuellen Azure-Computer ausführen. Weitere Informationen finden Sie unter [Bereitstellen eines virtuellen SQL Server-Computers über das Azure-Portal](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json).

Nach der Installation sollten Sie zusätzliche Konfigurationsänderungen vornehmen, um eine optimale Leistung zu erzielen. Weitere Informationen finden Sie unter [Bewährte Methoden für die Leistung und Konfigurationsrichtlinien für SQL Server für Linux](sql-server-linux-performance-best-practices.md).

## <a name="update-or-upgrade-sql-server"></a><a id="upgrade"></a> Update oder Upgrade von SQL Server

Wenn Sie das Paket **mssql-server** auf die neueste Version aktualisieren möchten, verwenden Sie je nach Plattform einen der folgenden Befehle:

| Plattform | Befehl(e) für das Paketupdate |
|-----|-----|
| RHEL | `sudo yum update mssql-server` |
| SLES | `sudo zypper update mssql-server` |
| Ubuntu | `sudo apt-get update`<br/>`sudo apt-get install mssql-server` |

Mit diesen Befehlen laden Sie das neueste Paket herunter, und die Binärdateien unter `/opt/mssql/` werden ersetzt. Benutzergenerierte Datenbanken und Systemdatenbanken sind von diesem Vorgang nicht betroffen.

Zum Upgraden von SQL Server [ändern Sie zunächst Ihr konfiguriertes Repository](sql-server-linux-change-repo.md) in die gewünschte SQL Server-Version. Verwenden Sie dann denselben **update**-Befehl, um Ihre Version von SQL Server upzugraden. Dies ist nur möglich, wenn der Upgradepfad zwischen den Repositorys unterstützt wird.

## <a name="rollback-sql-server"></a><a id="rollback"></a> Zurücksetzen von SQL Server

Wenn Sie SQL Server auf eine frühere Version zurücksetzen oder herabstufen möchten, führen Sie die folgenden Schritte aus:

1. Ermitteln Sie die Versionsnummer des SQL Server-Pakets, auf das Sie herabstufen möchten. Eine Liste der Paketnummern finden Sie in den [Versionshinweisen](../linux/sql-server-linux-release-notes.md).

1. Führen Sie ein Downgrade auf eine frühere Version von SQL Server aus. Ersetzen Sie `<version_number>` in den folgenden Befehlen durch die in Schritt 1 ermittelte Versionsnummer von SQL Server.

   | Plattform | Befehl(e) für das Paketupdate |
   |-----|-----|
   | RHEL | `sudo yum downgrade mssql-server-<version_number>.x86_64` |
   | SLES | `sudo zypper install --oldpackage mssql-server=<version_number>` |
   | Ubuntu | `sudo apt-get install mssql-server=<version_number>`<br/>`sudo systemctl start mssql-server` |

> [!NOTE]
> Sie können nur auf eine Version innerhalb derselben Hauptversion, z. B. SQL Server 2019, herabstufen.

## <a name="check-installed-sql-server-version"></a><a id="versioncheck"></a> Überprüfen der installierten Version von SQL Server

Gehen Sie zum Überprüfen Ihrer aktuellen Version und Edition von SQL Server unter Linux folgendermaßen vor:

1. Installieren Sie die [SQL Server-Befehlszeilentools](sql-server-linux-setup-tools.md), falls diese noch nicht installiert sind.

1. Führen Sie mit **sqlcmd** einen Transact-SQL-Befehl aus, der Ihre Version und Edition von SQL Server anzeigt.

   ```bash
   sqlcmd -S localhost -U SA -Q 'select @@VERSION'
   ```

## <a name="uninstall-sql-server"></a><a id="uninstall"></a> Deinstallieren von SQL Server

Wenn Sie das Paket **mssql-server** unter Linux entfernen möchten, verwenden Sie je nach Plattform einen der folgenden Befehle:

| Plattform | Befehl(e) zum Entfernen eines Pakets |
|-----|-----|
| RHEL | `sudo yum remove mssql-server` |
| SLES | `sudo zypper remove mssql-server` |
| Ubuntu | `sudo apt-get remove mssql-server` |

Durch Entfernen des Pakets werden die generierten Datenbankdateien nicht automatisch gelöscht. Wenn Sie die Datenbankdateien löschen möchten, verwenden Sie den folgenden Befehl:

```bash
sudo rm -rf /var/opt/mssql/
```

## <a name="unattended-install"></a><a id="unattended"></a>Unbeaufsichtigtes Installieren

Gehen Sie für eine unbeaufsichtigte Installation folgendermaßen vor:

- Führen Sie die ersten Schritte der [Schnellstarts](#platforms) aus, mit denen die Repositorys registriert und SQL Server installiert wird.
- Legen Sie beim Ausführen von `mssql-conf setup`[Umgebungsvariablen](sql-server-linux-configure-environment-variables.md) fest, und verwenden Sie die Option `-n` (keine Aufforderung).

Im folgenden Beispiel wird die Developer Edition von SQL Server mit der Umgebungsvariablen **MSSQL_PID** konfiguriert. Zudem werden die Lizenzbedingungen akzeptiert (**ACCEPT_EULA**) und das SA-Benutzerkennwort festgelegt (**MSSQL_SA_PASSWORD**). Mit dem Parameter `-n` wird eine unangekündigte Installation durchgeführt, bei der die Konfigurationswerte aus den Umgebungsvariablen abgerufen werden.

```bash
sudo MSSQL_PID=Developer ACCEPT_EULA=Y MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' /opt/mssql/bin/mssql-conf -n setup
```

Sie können auch ein Skript für weitere Aktionen erstellen. So können beispielsweise weitere SQL Server-Pakete installiert werden.

Ein ausführliches Beispielskript finden Sie in den folgenden Beispielen:

- [Red Hat unattended installation script (Skript für die unbeaufsichtigte Installation von Red Hat)](sample-unattended-install-redhat.md)
- [SUSE unattended installation script (Skript für die unbeaufsichtigte Installation von SUSE)](sample-unattended-install-suse.md)
- [Ubuntu unattended installation script (Skript für die unbeaufsichtigte Installation von Ubuntu)](sample-unattended-install-ubuntu.md)

## <a name="offline-install"></a><a id="offline"></a> Offlineinstallation

Wenn Ihr Linux-Computer keinen Zugriff auf die Onlinerepositorys hat, die in den [Schnellstarts](#platforms) verwendet werden, können Sie die Paketdateien direkt herunterladen. Diese Pakete befinden sich im Microsoft-Repository [https://packages.microsoft.com](https://packages.microsoft.com).

> [!TIP]
> Wenn Sie die Installation mithilfe der Schritte in den Schnellstarts erfolgreich abgeschlossen haben, müssen Sie das oder die SQL Server-Pakete nicht herunterladen oder manuell installieren. Dieser Abschnitt betrifft nur das Offlineszenario.

1. **Laden Sie das Paket für die Datenbank-Engine für Ihre Plattform herunter**: Downloadlinks für die Pakete finden Sie in den [Versionshinweisen](../linux/sql-server-linux-release-notes.md) im Abschnitt zu den Paketdetails.

1. **Verschieben Sie das heruntergeladene Paket auf Ihren Linux-Computer**: Wenn Sie einen anderen Computer zum Herunterladen der Pakete verwendet haben, stellt der Befehl **scp** eine Möglichkeit dar, die Pakete auf Ihren Linux-Computer zu verschieben.

1. **Installieren Sie das Paket für die Datenbank-Engine**: Verwenden Sie je nach Plattform einen der folgenden Befehle. Ersetzen Sie den Dateinamen des Pakets in diesem Beispiel durch den genauen Namen des Pakets, das Sie heruntergeladen haben.

   | Plattform | Befehl zur Paketinstallation |
   |-----|-----|
   | RHEL | `sudo yum localinstall mssql-server_versionnumber.x86_64.rpm` |
   | SLES | `sudo zypper install mssql-server_versionnumber.x86_64.rpm` |
   | Ubuntu | `sudo dpkg -i mssql-server_versionnumber_amd64.deb` |

    > [!NOTE]
    > Sie können zur Installation der RPM-Pakete (RHEL und SLES) auch den Befehl `rpm -ivh` verwenden. Mit den Befehlen in der vorherigen Tabelle werden jedoch automatisch Abhängigkeiten installiert, sofern sie in genehmigten Repositorys verfügbar sind.

1. **Lösen Sie fehlende Abhängigkeiten auf**: Zu diesem Zeitpunkt fehlen möglicherweise Abhängigkeiten. Falls nicht, können Sie diesen Schritt überspringen. Wenn Sie unter Ubuntu Zugriff auf genehmigte Repositorys mit diesen Abhängigkeiten haben, ist die einfachste Lösung die Verwendung des Befehls `apt-get -f install`. Mit diesem Befehl wird auch die Installation von SQL Server abgeschlossen. Verwenden Sie zum manuellen Überprüfen von Abhängigkeiten je nach Plattform einen der folgenden Befehle:

   | Plattform | Befehl zum Auflisten von Abhängigkeiten |
   |-----|-----|
   | RHEL | `rpm -qpR mssql-server_versionnumber.x86_64.rpm` |
   | SLES | `rpm -qpR mssql-server_versionnumber.x86_64.rpm` |
   | Ubuntu | `dpkg -I mssql-server_versionnumber_amd64.deb` |

   Wenn Sie die fehlenden Abhängigkeiten aufgelöst haben, versuchen Sie erneut, das Paket „mssql-server“ zu installieren.

1. **Schließen Sie das Setup von SQL Server ab**: Verwenden Sie dazu den Befehl **mssql-conf**.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

## <a name="licensing-and-pricing"></a>Lizenzierung und Preise

Die SQL Server-Lizenzierung für Linux und Windows ist identisch. Weitere Informationen zur Lizenzierung und den Preisen von SQL Server finden Sie unter [Lizenzierung von SQL Server](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

## <a name="optional-sql-server-features"></a>Optionale SQL Server-Funktionen

Nach der Installation können Sie optionale SQL Server-Funktionen installieren oder aktivieren, wie etwa:

- [SQL Server-Befehlszeilentools](sql-server-linux-setup-tools.md)
- [SQL Server-Agent](sql-server-linux-setup-sql-agent.md)
- [SQL Server-Volltextsuche](sql-server-linux-setup-full-text-search.md)
- [Machine Learning Services (R, Python)](sql-server-linux-setup-machine-learning.md)
- [SQL Server Integration Services](sql-server-linux-setup-ssis.md)

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]

> [!TIP]
> Antworten auf häufig gestellte Fragen finden Sie unter [Häufig gestellte Fragen zu SQL Server für Linux](sql-server-linux-faq.yml).
