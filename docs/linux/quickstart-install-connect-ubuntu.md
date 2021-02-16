---
title: 'Ubuntu: Installieren von SQL Server für Linux'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie SQL Server 2017 oder SQL Server 2019 unter Ubuntu installieren und anschließend mit sqlcmd eine Datenbank erstellen und abfragen.
author: VanMSFT
ms.author: vanto
ms.date: 07/15/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: seo-lt-2019
ms.technology: linux
ms.assetid: 31c8c92e-12fe-4728-9b95-4bc028250d85
ms.openlocfilehash: 11adcb6d14675952e19fcc37994614348fdae70c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339182"
---
# <a name="quickstart-install-sql-server-and-create-a-database-on-ubuntu"></a>Schnellstart: Installieren von SQL Server und Erstellen einer Datenbank unter Ubuntu
[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]


<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

In diesem Schnellstart installieren Sie SQL Server 2017 unter Ubuntu 16.04/18.04. Danach stellen Sie eine Verbindung mit **sqlcmd** her, um Ihre erste Datenbank zu erstellen und Abfragen auszuführen.

> [!TIP]
> Für dieses Tutorial sind Benutzereingaben und eine Internetverbindung erforderlich. Wenn Sie am unbeaufsichtigten oder am Offline-Installationsverfahren interessiert sind, finden Sie weitere Informationen im [Leitfaden für die Installation von SQL Server für Linux](sql-server-linux-setup.md). Eine Liste mit unterstützten Plattformen finden Sie in unseren [Versionshinweisen](sql-server-linux-release-notes.md).

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

In diesem Schnellstart installieren Sie SQL Server 2019 unter Ubuntu 16.04/18.04. Danach stellen Sie eine Verbindung mit **sqlcmd** her, um Ihre erste Datenbank zu erstellen und Abfragen auszuführen.

> [!TIP]
> Für dieses Tutorial sind Benutzereingaben und eine Internetverbindung erforderlich. Wenn Sie am unbeaufsichtigten oder am Offline-Installationsverfahren interessiert sind, finden Sie weitere Informationen im [Leitfaden für die Installation von SQL Server für Linux](sql-server-linux-setup.md). Eine Liste mit unterstützten Plattformen finden Sie in unseren [Versionshinweisen](sql-server-linux-release-notes-2019.md).

::: moniker-end

## <a name="prerequisites"></a>Voraussetzungen

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Sie müssen über einen Computer mit Ubuntu 16.04 oder 18.04 mit **mindestens 2 GB** Arbeitsspeicher verfügen.

Rufen Sie <http://releases.ubuntu.com/bionic/> auf, um Ubuntu 18.04 auf Ihrem eigenen Computer zu installieren. Sie können auch virtuelle Ubuntu-Computer in Azure erstellen. Weitere Informationen finden Sie unter [Erstellen und Verwalten virtueller Linux-Computer mit der Azure-Befehlszeilenschnittstelle](/azure/virtual-machines/linux/tutorial-manage-vm).

> [!NOTE]
> Zurzeit wird das [Windows-Subsystem für Linux](/windows/wsl/about) für Windows 10 als Installationsziel nicht unterstützt.

Weitere Systemanforderungen finden Sie unter [Systemanforderungen für SQL Server für Linux](sql-server-linux-setup.md#system).

> [!NOTE]
> Ubuntu 18.04 wird ab SQL Server 2017 CU20 unterstützt. Wenn Sie die Anweisungen in diesem Artikel mit Ubuntu 18.04 ausführen möchten, stellen Sie sicher, dass Sie den richtigen [Repositorypfad](sql-server-linux-change-repo.md) verwenden: `18.04` anstatt `16.04`.
>
> Wenn Sie SQL Server mit einer niedrigeren Version ausführen, ist die Konfiguration mit [Änderungen](/archive/blogs/sql_server_team/installing-sql-server-2017-for-linux-on-ubuntu-18-04-lts) möglich.

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Sie müssen über einen Computer mit Ubuntu 16.04 oder 18.04 mit **mindestens 2 GB** Arbeitsspeicher verfügen.

Rufen Sie <http://releases.ubuntu.com/bionic/> auf, um Ubuntu 18.04 auf Ihrem eigenen Computer zu installieren. Sie können auch virtuelle Ubuntu-Computer in Azure erstellen. Weitere Informationen finden Sie unter [Erstellen und Verwalten virtueller Linux-Computer mit der Azure-Befehlszeilenschnittstelle](/azure/virtual-machines/linux/tutorial-manage-vm).

> [!NOTE]
> Zurzeit wird das [Windows-Subsystem für Linux](/windows/wsl/about) für Windows 10 als Installationsziel nicht unterstützt.

Weitere Systemanforderungen finden Sie unter [Systemanforderungen für SQL Server für Linux](sql-server-linux-setup.md#system).

::: moniker-end

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="install-sql-server"></a><a id="install"></a>Installieren von SQL Server

> [!NOTE]
> Die folgenden Befehle für SQL Server 2017 zeigen auf das Ubuntu 18.04-Repository. Wenn Sie Ubuntu 16.04 verwenden, ändern Sie den Pfad unten in `/ubuntu/16.04/` anstelle von `/ubuntu/18.04/`.

Um SQL Server unter Ubuntu zu konfigurieren, führen Sie in einem Terminal die folgenden Befehle aus, die das Paket **mssql-server** installieren.

1. Importieren Sie die öffentlichen GPG-Schlüssel des Repositorys:

   ```bash
   wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Registrieren Sie das Microsoft SQL Server Ubuntu-Repository:
   
   Für Ubuntu 16.04:
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"
   ```
   
   Für Ubuntu 18.04:
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2017.list)"
   ```

   > [!TIP]
   > Wenn Sie SQL Server 2019 installieren möchten, müssen Sie stattdessen das Repository SQL Server 2019 registrieren. Verwenden Sie für Installationen von SQL Server 2019 den folgenden Befehl:
   >
   > Für Ubuntu 16.04:
   >
   > ```bash
   > sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2019.list)"
   > ```
   >
   > Für Ubuntu 18.04:
   >
   > ```bash
   > sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019.list)"
   > ```

3. Führen Sie die folgenden Befehle aus, um SQL Server zu installieren:

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server
   ```

4. Nachdem die Paketinstallation abgeschlossen ist, führen Sie **mssql-conf setup** aus, und befolgen Sie die Anweisungen, um das Systemadministratorkennwort festzulegen und Ihre Edition auszuwählen.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!TIP]
   > Für die folgenden SQL Server 2017-Editionen sind kostenlose Lizenzen verfügbar: Evaluation, Developer und Express.

   > [!NOTE]
   > Stellen Sie sicher, dass Sie ein sicheres Kennwort für das Systemadministratorkonto angeben (minimale Länge von 8 Zeichen, Groß-und Kleinbuchstaben, Ziffern und/oder nicht-alphanumerische Symbole).

5. Nachdem die Konfiguration abgeschlossen ist, überprüfen Sie, ob der Dienst ausgeführt wird:

   ```bash
   systemctl status mssql-server --no-pager
   ```

6. Wenn Sie eine Remoteverbindung planen, müssen Sie möglicherweise auch den SQL Server-TCP-Port (standardmäßig 1433) in Ihrer Firewall öffnen.

Jetzt wird SQL Server auf Ihrem Ubuntu-Computer ausgeführt und ist einsatzbereit!

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="install-sql-server"></a><a id="install"></a>Installieren von SQL Server

> [!NOTE]
> Die folgenden Befehle für SQL Server 2019 verweisen auf das Ubuntu 18.04-Repository. Wenn Sie Ubuntu 16.04 verwenden, ändern Sie den Pfad unten in `/ubuntu/16.04/` anstelle von `/ubuntu/18.04/`.

Um SQL Server unter Ubuntu zu konfigurieren, führen Sie in einem Terminal die folgenden Befehle aus, die das Paket **mssql-server** installieren.

1. Importieren Sie die öffentlichen GPG-Schlüssel des Repositorys:

   ```bash
   wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Registrieren Sie das Microsoft SQL Server Ubuntu-Repository für SQL Server 2019:
   
   Für Ubuntu 16.04:
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2019.list)"
   ```
   
   Für Ubuntu 18.04:
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019.list)"
   ```

3. Führen Sie die folgenden Befehle aus, um SQL Server zu installieren:

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server
   ```

4. Nachdem die Paketinstallation abgeschlossen ist, führen Sie **mssql-conf setup** aus, und befolgen Sie die Anweisungen, um das Systemadministratorkennwort festzulegen und Ihre Edition auszuwählen.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!NOTE]
   > Stellen Sie sicher, dass Sie ein sicheres Kennwort für das Systemadministratorkonto angeben (minimale Länge von 8 Zeichen, Groß-und Kleinbuchstaben, Ziffern und/oder nicht-alphanumerische Symbole).

5. Nachdem die Konfiguration abgeschlossen ist, überprüfen Sie, ob der Dienst ausgeführt wird:

   ```bash
   systemctl status mssql-server --no-pager
   ```

6. Wenn Sie eine Remoteverbindung planen, müssen Sie möglicherweise auch den SQL Server-TCP-Port (standardmäßig 1433) in Ihrer Firewall öffnen.

Jetzt wird SQL Server 2019 auf Ihrem Ubuntu-Computer ausgeführt und ist einsatzbereit.

::: moniker-end

## <a name="install-the-sql-server-command-line-tools"></a><a id="tools"></a>Installieren der SQL Server-Befehlszeilentools

Um eine Datenbank zu erstellen, müssen Sie eine Verbindung mit einem Tool herstellen, das Transact-SQL-Anweisungen auf dem SQL Server-Computer ausführen kann. Mit den folgenden Schritten installieren Sie die SQL Server-Befehlszeilentools: [sqlcmd](../tools/sqlcmd-utility.md) und [bcp](../tools/bcp-utility.md).

Führen Sie zum Installieren von **mssql-tools** unter Ubuntu die folgenden Schritte aus. 

1. Importieren Sie die GPG-Schlüssel des öffentlichen Repositorys.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Registrieren Sie das Microsoft Ubuntu-Repository.
   
   Für Ubuntu 16.04:
   
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

   Für Ubuntu 18.04:
   
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

1. Aktualisieren Sie die Liste mit Quellen, und führen Sie den Installationsbefehl mit dem unixODBC-Entwicklerpaket aus. Weitere Informationen finden Sie unter [Installieren des Microsoft-ODBC-Treibers für SQL Server (Linux)](../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

   ```bash
   sudo apt-get update 
   sudo apt-get install mssql-tools unixodbc-dev
   ```

   > [!Note] 
   > Führen Sie die folgenden Befehle aus, um **mssql-tools** auf die neueste Version zu aktualisieren:
   >
   > ```bash
   > sudo apt-get update 
   > sudo apt-get install mssql-tools 
   > ```

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

[!INCLUDE [Connect, create, and query data](../includes/sql-linux-quickstart-connect-query.md)]
