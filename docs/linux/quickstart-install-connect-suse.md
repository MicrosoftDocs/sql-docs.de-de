---
title: 'SUSE: Installieren von SQL Server für Linux'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie SQL Server 2017 oder SQL Server 2019 unter SUSE Linux Enterprise Server installieren und anschließend mit sqlcmd eine Datenbank erstellen und abfragen.
author: VanMSFT
ms.author: vanto
ms.date: 04/10/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 31ddfb80-f75c-4f51-8540-de6213cb68b8
ms.openlocfilehash: 7aadd757e9e825ace17b69d28f2ab8ecc78123da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340405"
---
# <a name="quickstart-install-sql-server-and-create-a-database-on-suse-linux-enterprise-server"></a>Schnellstart: Installieren von SQL Server und Erstellen einer Datenbank unter SUSE Linux Enterprise Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

In diesem Schnellstart installieren Sie SQL Server 2017 oder SQL Server 2019 unter SUSE Linux Enterprise Server v12 SP2 (SLES). Danach stellen Sie eine Verbindung mit **sqlcmd** her, um Ihre erste Datenbank zu erstellen und Abfragen auszuführen.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

In diesem Schnellstart installieren Sie SQL Server 2019 unter SUSE Linux Enterprise Server v12 (SLES). Danach stellen Sie eine Verbindung mit **sqlcmd** her, um Ihre erste Datenbank zu erstellen und Abfragen auszuführen.

> [!IMPORTANT]
> SQL Server 2019 wird unter SUSE Enterprise Linux Server v12 SP2, SP3 ,SP4 oder SP5 unterstützt.

::: moniker-end

> [!TIP]
> Für dieses Tutorial sind Benutzereingaben und eine Internetverbindung erforderlich. Wenn Sie am [unbeaufsichtigten](sql-server-linux-setup.md#unattended) oder am [Offline](sql-server-linux-setup.md#offline)-Installationsverfahren interessiert sind, finden Sie weitere Informationen im [Leitfaden für die Installation von SQL Server für Linux](sql-server-linux-setup.md).

## <a name="prerequisites"></a>Voraussetzungen

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Sie müssen über einen SLES v12 SP2-Computer mit **mindestens 2 GB** Arbeitsspeicher verfügen. Das Dateisystem muss **XFS** oder **EXT4** sein. Andere Dateisysteme wie **BTRFS** werden nicht unterstützt.

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Sie müssen über einen Computer mit SLES v12 SP2, SP3, SP4 oder SP5 und **mindestens 2 GB** Arbeitsspeicher verfügen. Das Dateisystem muss **XFS** oder **EXT4** sein. Andere Dateisysteme wie **BTRFS** werden nicht unterstützt.

::: moniker-end

Um SUSE Linux Enterprise Server auf Ihrem eigenen Computer zu installieren, wechseln Sie zu [https://www.suse.com/products/server](https://www.suse.com/products/server). Sie können auch virtuelle SLES-Computer in Azure erstellen. Weitere Informationen finden Sie unter [Erstellen und Verwalten virtueller Linux-Computer mit der Azure-Befehlszeilenschnittstelle](/azure/virtual-machines/linux/tutorial-manage-vm). Verwenden Sie `--image SLES` im Aufruf von `az vm create`.

Wenn Sie bereits eine CTP- oder RC-Version von SQL Server installiert haben, müssen Sie zuerst das alte Repository entfernen, bevor Sie die folgenden Schritte ausführen. Weitere Informationen finden Sie unter [Konfigurieren von Linux-Repositorys für SQL Server 2017 und 2019](sql-server-linux-change-repo.md).

> [!NOTE]
> Zurzeit wird das [Windows-Subsystem für Linux](/windows/wsl/about) für Windows 10 als Installationsziel nicht unterstützt.

Weitere Systemanforderungen finden Sie unter [Systemanforderungen für SQL Server für Linux](sql-server-linux-setup.md#system).

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="install-sql-server-2017"></a><a id="install"></a>Installieren von SQL Server 2017

Führen Sie zum Konfigurieren von SQL Server 2017 unter SLES in einem Terminal die folgenden Befehle aus, um das Paket **mssql-server** zu installieren:

1. Laden Sie die Konfigurationsdatei für das SLES-Repository für Microsoft SQL Server 2017 herunter:

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo
   ```

   > [!TIP]
   > Wenn Sie SQL Server 2019 installieren möchten, müssen Sie stattdessen das Repository SQL Server 2019 registrieren. Verwenden Sie für Installationen von SQL Server 2019 den folgenden Befehl:
   >
   > ```bash
   > sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo
   > ```

2. Aktualisieren Sie Ihre Repositorys.

   ```bash
   sudo zypper --gpg-auto-import-keys refresh 
   ```
   
   Damit sichergestellt ist, dass der Microsoft-Paketsignaturschlüssel auf Ihrem System installiert ist, importieren Sie ihn bitte mithilfe des Befehls unten: 
   
   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```
   
3. Führen Sie die folgenden Befehle aus, um SQL Server zu installieren:

   ```bash
   sudo zypper install -y mssql-server
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
   systemctl status mssql-server
   ```

6. Wenn Sie eine Remoteverbindung planen, müssen Sie möglicherweise auch den SQL Server-TCP-Port (standardmäßig 1433) in Ihrer Firewall öffnen. Wenn Sie die SuSE-Firewall verwenden, müssen Sie die Konfigurationsdatei **/etc/sysconfig/SuSEfirewall2** bearbeiten. Ändern Sie den Eintrag **FW_SERVICES_EXT_TCP** so, dass die SQL Server-Portnummer enthalten ist.

   ```
   FW_SERVICES_EXT_TCP="1433"
   ```

Jetzt wird SQL Server auf Ihrem SLES-Computer ausgeführt und ist einsatzbereit!

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="install-sql-server-2019"></a><a id="install"></a>Installieren von SQL Server 2019

Führen Sie zum Konfigurieren von SQL Server 2019 unter SLES in einem Terminal die folgenden Befehle aus, um das Paket **mssql-server** zu installieren:

1. Laden Sie die Konfigurationsdatei für das SLES-Repository für Microsoft SQL Server 2019 herunter:

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo
   ```

2. Aktualisieren Sie Ihre Repositorys.

   ```bash
   sudo zypper --gpg-auto-import-keys refresh 
   ```

   Damit sichergestellt ist, dass der Microsoft-Paketsignaturschlüssel auf Ihrem System installiert ist, importieren Sie den Schlüssel mit folgendem Befehl: 

   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```
   
3. Führen Sie die folgenden Befehle aus, um SQL Server zu installieren:

   ```bash
   sudo zypper install -y mssql-server
   ```

4. Nachdem die Paketinstallation abgeschlossen ist, führen Sie **mssql-conf setup** aus, und befolgen Sie die Anweisungen, um das Systemadministratorkennwort festzulegen und Ihre Edition auszuwählen.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!NOTE]
   > Stellen Sie sicher, dass Sie ein sicheres Kennwort für das Systemadministratorkonto angeben (minimale Länge von 8 Zeichen, Groß-und Kleinbuchstaben, Ziffern und/oder nicht-alphanumerische Symbole).

5. Nachdem die Konfiguration abgeschlossen ist, überprüfen Sie, ob der Dienst ausgeführt wird:

   ```bash
   systemctl status mssql-server
   ```

6. Wenn Sie eine Remoteverbindung planen, müssen Sie möglicherweise auch den SQL Server-TCP-Port (standardmäßig 1433) in Ihrer Firewall öffnen. Wenn Sie die SuSE-Firewall verwenden, müssen Sie die Konfigurationsdatei **/etc/sysconfig/SuSEfirewall2** bearbeiten. Ändern Sie den Eintrag **FW_SERVICES_EXT_TCP** so, dass die SQL Server-Portnummer enthalten ist.

   ```
   FW_SERVICES_EXT_TCP="1433"
   ```

Jetzt wird SQL Server 2019 auf Ihrem SLES-Computer ausgeführt und ist einsatzbereit!

::: moniker-end


## <a name="install-the-sql-server-command-line-tools"></a><a id="tools"></a>Installieren der SQL Server-Befehlszeilentools

Um eine Datenbank zu erstellen, müssen Sie eine Verbindung mit einem Tool herstellen, das Transact-SQL-Anweisungen auf dem SQL Server-Computer ausführen kann. Mit den folgenden Schritten installieren Sie die SQL Server-Befehlszeilentools: [sqlcmd](../tools/sqlcmd-utility.md) und [bcp](../tools/bcp-utility.md).

1. Fügen Sie das Microsoft SQL Server-Repository zu Zypper hinzu.

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo 
   sudo zypper --gpg-auto-import-keys refresh
   ```

1. Installieren Sie **mssql-tools** mit dem unixODBC-Entwicklerpaket. Weitere Informationen finden Sie unter [Installieren des Microsoft-ODBC-Treibers für SQL Server (Linux)](../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

   ```bash
   sudo zypper install -y mssql-tools unixODBC-devel
   ```

1. Fügen Sie für einfacheres Arbeiten `/opt/mssql-tools/bin/` zu Ihrer **PATH**-Umgebungsvariablen hinzu. Dadurch können Sie die Tools ausführen, ohne den vollständigen Pfad angeben zu müssen. Führen Sie die folgenden Befehle aus, um **PATH** sowohl für Anmeldesitzungen als auch für interaktive Sitzungen oder Sitzungen ohne Anmeldung zu ändern:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

[!INCLUDE [Connect, create, and query data](../includes/sql-linux-quickstart-connect-query.md)]