---
title: Installieren von SQL Server-Befehlszeilentools unter Linux
titleSuffix: SQL Server
description: Erfahren Sie, wie Sie die SQL Server-Befehlszeilentools, Microsoft-ODBC-Treiber sowie deren Abhängigkeiten unter Linux installieren.
author: VanMSFT
ms.author: vanto
ms.date: 06/30/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: sqlfreshmay19
ms.technology: linux
ms.assetid: eff8e226-185f-46d4-a3e3-e18b7a439e63
ms.openlocfilehash: 36508869b30688e7a87035b50a7a06ca1e2a083a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346344"
---
# <a name="install-sqlcmd-and-bcp-the-sql-server-command-line-tools-on-linux"></a>Installieren der SQL Server-Befehlszeilentools sqlcmd und bcp unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Über die folgenden Schritte werden die Befehlszeilentools, die Microsoft ODBC-Treiber und deren Abhängigkeiten installiert. Das Paket **mssql-tools** enthält Folgendes:

- **sqlcmd**: Abfragehilfsprogramm für die Befehlszeile
- **bcp**: Hilfsprogramm für den Massenimport/-export

Installieren Sie die Tools für Ihre Plattform:

- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)
- [macOS](#macos)
- [Docker](#docker)

In diesem Artikel wird beschrieben, wie die Befehlszeilentools installiert werden. Beispiele zur Verwendung von **sqlcmd** oder **bcp** finden Sie unter den [Links](#next-steps) am Ende dieses Artikels.

## <a name="a-idrhelinstall-tools-on-rhel-8"></a><a id="RHEL"><a/>Installieren von Tools unter RHEL 8

Führen Sie zum Installieren von **mssql-tools** unter Red Hat Enterprise Linux die folgenden Schritte aus. 

1. Wechseln Sie in den Superuser-Modus.

   ```bash
   sudo su
   ```

1. Laden Sie die Konfigurationsdatei für das Microsoft Red Hat-Repository herunter.

   ```bash
   curl https://packages.microsoft.com/config/rhel/8/prod.repo > /etc/yum.repos.d/msprod.repo
   ```

1. Beenden Sie den Superuser-Modus.

   ```bash
   exit
   ```

1. Wenn Sie eine frühere Version von **mssql-tools** installiert hatten, entfernen Sie alle älteren unixODBC-Pakete.

   ```bash
   sudo yum remove mssql-tools unixODBC-utf16-devel
   ```

1. Führen Sie die folgenden Befehle aus, um **mssql-tools** mit dem unixODBC-Entwicklerpaket zu installieren.

   ```bash
   sudo yum install mssql-tools unixODBC-devel
   ```

   > [!Note] 
   > Führen Sie die folgenden Befehle aus, um **mssql-tools** auf die neueste Version zu aktualisieren:
   >    ```bash
   >   sudo yum check-update
   >   sudo yum update mssql-tools
   >   ```

1. **Optional**: Fügen Sie in einer Bash-Shell `/opt/mssql-tools/bin/` zu Ihrer **PATH**-Umgebungsvariablen hinzu.

   Ändern Sie **PATH** in der Datei **~/.bash_profile** mit dem folgenden Befehl, um **sqlcmd/bcp** von der Bash-Shell aus für Anmeldesitzungen zugänglich zu machen:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   Ändern Sie **PATH** in der Datei **~/.bashrc** mit dem folgenden Befehl, um **sqlcmd/bcp** von der Bash-Shell aus für interaktive Sitzungen oder Sitzungen ohne Anmeldung zugänglich zu machen:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-ubuntu-1604"></a><a id="ubuntu"></a>Installieren von Tools unter Ubuntu 16.04

Führen Sie zum Installieren von **mssql-tools** unter Ubuntu die folgenden Schritte aus.

> [!NOTE]
> Ubuntu 18.04 wird ab SQL Server 2019 CU3 unterstützt. Wenn Sie Ubuntu 18.04 verwenden, ändern Sie den Pfad des Repositorys von `/ubuntu/16.04` in `/ubuntu/18.04`.

1. Importieren Sie die GPG-Schlüssel des öffentlichen Repositorys.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Registrieren Sie das Microsoft Ubuntu-Repository.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

1. Aktualisieren Sie die Liste mit Quellen, und führen Sie den Installationsbefehl mit dem unixODBC-Entwicklerpaket aus.

   ```bash
   sudo apt-get update 
   sudo apt-get install mssql-tools unixodbc-dev
   ```

   > [!Note] 
   > Führen Sie die folgenden Befehle aus, um **mssql-tools** auf die neueste Version zu aktualisieren:
   >    ```bash
   >   sudo apt-get update 
   >   sudo apt-get install mssql-tools 
   >   ```

1. **Optional**: Fügen Sie in einer Bash-Shell `/opt/mssql-tools/bin/` zu Ihrer **PATH**-Umgebungsvariablen hinzu.

   Ändern Sie **PATH** in der Datei **~/.bash_profile** mit dem folgenden Befehl, um **sqlcmd/bcp** von der Bash-Shell aus für Anmeldesitzungen zugänglich zu machen:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   Ändern Sie **PATH** in der Datei **~/.bashrc** mit dem folgenden Befehl, um **sqlcmd/bcp** von der Bash-Shell aus für interaktive Sitzungen oder Sitzungen ohne Anmeldung zugänglich zu machen:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-sles-12"></a><a id="SLES"></a>Installieren von Tools unter SLES 12

Führen Sie zum Installieren von **mssql-tools** unter SUSE Linux Enterprise Server die folgenden Schritte aus. 

1. Fügen Sie das Microsoft SQL Server-Repository zu Zypper hinzu.

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo 
   sudo zypper --gpg-auto-import-keys refresh
   ```

1. Installieren Sie **mssql-tools** mit dem unixODBC-Entwicklerpaket.

   ```bash
   sudo zypper install mssql-tools unixODBC-devel
   ```

   > [!Note] 
   > Führen Sie die folgenden Befehle aus, um **mssql-tools** auf die neueste Version zu aktualisieren:
   >    ```bash
   >   sudo zypper refresh
   >   sudo zypper update mssql-tools
   >   ```

1. **Optional**: Fügen Sie in einer Bash-Shell `/opt/mssql-tools/bin/` zu Ihrer **PATH**-Umgebungsvariablen hinzu.

   Ändern Sie **PATH** in der Datei **~/.bash_profile** mit dem folgenden Befehl, um **sqlcmd/bcp** von der Bash-Shell aus für Anmeldesitzungen zugänglich zu machen:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   Ändern Sie **PATH** in der Datei **~/.bashrc** mit dem folgenden Befehl, um **sqlcmd/bcp** von der Bash-Shell aus für interaktive Sitzungen oder Sitzungen ohne Anmeldung zugänglich zu machen:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-macos"></a><a id="macos"></a> Installieren von Tools unter macOS

Eine Vorschauversion von **sqlcmd** und **bcp** ist nun unter macOS verfügbar. Weitere Informationen finden Sie in der [Ankündigung](https://blogs.technet.microsoft.com/dataplatforminsider/2017/05/16/sql-server-command-line-tools-for-macos-released/).

*Installieren Sie [Homebrew](https://brew.sh), falls Sie es noch nicht installiert haben:*

- `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

Verwenden Sie zum Installieren der Tools für Mac El Capitan und Sierra die folgenden Befehle:

```bash
# brew untap microsoft/mssql-preview if you installed the preview version 
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install mssql-tools
#for silent install: 
#HOMEBREW_NO_ENV_FILTERING=1 ACCEPT_EULA=y brew install mssql-tools
```

## <a name="docker"></a><a id="docker"></a> Docker

Wenn Sie [SQL Server in einem Docker-Container ausführen](quickstart-install-connect-docker.md), sind die SQL Server-Befehlszeilentools bereits im Linux-Containerimage von SQL Server enthalten. Wenn Sie mithilfe einer interaktiven Bash-Shell mit einem ausgeführten Container verbunden sind, können Sie die Tools lokal ausführen.

## <a name="offline-installation"></a>Offlineinstallation

[!INCLUDE[SQL Server Linux offline package installation](../includes/sql-server-linux-offline-package-install-intro.md)]

1. Suchen und kopieren Sie zunächst das Paket **mssql-tools** für Ihre Linux-Verteilung:

   | Linux-Verteilung | Speicherort des Pakets **mssql-tools** |
   |---|---|
   | Red Hat | [https://packages.microsoft.com/rhel/7.3/prod](https://packages.microsoft.com/rhel/7.3/prod) |
   | SLES | [https://packages.microsoft.com/sles/12/prod](https://packages.microsoft.com/sles/12/prod)|
   | Ubuntu 16.04 | [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/mssql-tools](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/mssql-tools) |

1. Suchen und kopieren Sie auch das Paket **msodbcsql**, bei dem es sich um eine Abhängigkeit handelt. Das Paket **msodbcsql** verfügt ebenfalls über eine Abhängigkeit von **unixODBC-devel** (Red Hat und SLES) oder **unixodbc-dev** (Ubuntu). Die Speicherorte der **msodbcsql**-Pakete sind in der folgenden Tabelle aufgeführt:

   | Linux-Verteilung | Speicherort von ODBC-Paketen |
   |---|---|
   | Red Hat | [https://packages.microsoft.com/rhel/8/prod](https://packages.microsoft.com/rhel/8/prod) |
   | SLES | [https://packages.microsoft.com/sles/12/prod](https://packages.microsoft.com/sles/12/prod)|
   | Ubuntu 16.04 | [**msodbcsql**](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql)<br/>[**unixodbc-dev**](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/u/unixodbc/) |

1. **Verschieben Sie die heruntergeladenen Pakete auf Ihren Linux-Computer**. Wenn Sie einen anderen Computer zum Herunterladen der Pakete verwendet haben, stellt der Befehl **scp** eine Möglichkeit dar, die Pakete auf Ihren Linux-Computer zu verschieben.

1. **Installieren der Pakete**: Installieren Sie die Pakete **mssql-tools** und **msodbc**. Wenn Abhängigkeitsfehler auftreten, ignorieren Sie diese bis zum nächsten Schritt.

    | Plattform | Befehle zur Paketinstallation |
    |-----|-----|
    | Red Hat | `sudo yum localinstall msodbcsql-<version>.rpm`<br/>`sudo yum localinstall mssql-tools-<version>.rpm` |
    | SLES | `sudo zypper install msodbcsql-<version>.rpm`<br/>`sudo zypper install mssql-tools-<version>.rpm` |
    | Ubuntu | `sudo dpkg -i msodbcsql_<version>.deb`<br/>`sudo dpkg -i mssql-tools_<version>.deb` |

1. **Lösen Sie fehlende Abhängigkeiten auf**: Zu diesem Zeitpunkt fehlen möglicherweise Abhängigkeiten. Falls nicht, können Sie diesen Schritt überspringen. In einigen Fällen müssen Sie diese Abhängigkeiten manuell suchen und installieren.

    Bei RPM-Paketen können Sie die erforderlichen Abhängigkeiten mithilfe der folgenden Befehle überprüfen:

    ```bash
    rpm -qpR msodbcsql-<version>.rpm
    rpm -qpR mssql-tools-<version>.rpm
    ```

    Wenn Sie bei Debian-Paketen Zugriff auf genehmigte Repositorys mit diesen Abhängigkeiten haben, ist die einfachste Lösung die Verwendung des Befehls **apt-get**:

    ```bash
    sudo apt-get -f install
    ```

    > [!NOTE]
    > Mit diesem Befehl wird auch die Installation der SQL Server-Pakete abgeschlossen.

    Wenn das bei Ihren Debian-Paketen nicht funktioniert, können Sie die erforderlichen Abhängigkeiten mithilfe der folgenden Befehle überprüfen:

    ```bash
    dpkg -I msodbcsql_<version>_amd64.deb | grep "Depends:"
    dpkg -I mssql-tools_<version>_amd64.deb | grep "Depends:"
    ```

## <a name="next-steps"></a>Nächste Schritte

Ein Beispiel für die Verwendung von **sqlcmd** zum Herstellen einer Verbindung mit SQL Server und zum Erstellen einer Datenbank finden Sie in einem der folgenden Schnellstarts:

- [Installation unter Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Installation unter SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Installation unter Ubuntu](quickstart-install-connect-ubuntu.md)
- [Ausführung in Docker](quickstart-install-connect-ubuntu.md)

Ein Beispiel für die Verwendung von **bcp** für den Massenimport und -export von Daten finden Sie unter [Bulk copy data to SQL Server on Linux (Massenkopieren von Daten auf SQL Server für Linux)](sql-server-linux-migrate-bcp.md).
