---
title: Unbeaufsichtigte Installation für SQL Server unter SUSE Linux Enterprise Server
titleSuffix: SQL Server
description: Verwenden Sie ein Bash-Beispielskript, um SQL Server 2017 ohne interaktive Eingaben unter SUSE Linux Enterprise Server (SLES) v12 SP2 zu installieren.
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 410a4c1fcb378f4d93730721ecfc0416564794bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339138"
---
# <a name="sample-unattended-sql-server-installation-script-for-suse-linux-enterprise-server"></a>Beispiel: Skript für eine unbeaufsichtigte SQL Server-Installation für SUSE Linux Enterprise Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Dieses Bash-Beispielskript installiert SQL Server 2017 unter SUSE Linux Enterprise Server (SLES) v12 SP2 ohne interaktive Eingabe. Es stellt Beispiele für die Installation von Datenbank-Engine, SQL Server-Befehlszeilentools und SQL Server-Agent bereit und führt Schritte nach der Installation aus. Optional können Sie die Volltextsuche installieren und einen Administratorbenutzer erstellen.

> [!TIP]
> Wenn Sie kein Skript für die unbeaufsichtigte Installation benötigen, lässt sich SQL Server am schnellsten installieren, indem Sie die [Schnellstartanleitung für SLES](quickstart-install-connect-suse.md) befolgen. Weitere Setupinformationen finden Sie im [Leitfaden für die Installation von SQL Server unter Linux](sql-server-linux-setup.md).

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen mindestens 2 GB Arbeitsspeicher, um SQL Server für Linux auszuführen.
- Das Dateisystem muss **XFS** oder **EXT4** sein. Andere Dateisysteme wie **BTRFS** werden nicht unterstützt.
- Weitere Systemanforderungen finden Sie unter [Systemanforderungen für SQL Server für Linux](sql-server-linux-setup.md#system).

> [!IMPORTANT]
> SQL Server 2017 erfordert das Paket „libsss_nss_idmap0“, das nicht von den standardmäßigen SLES-Repositorys bereitgestellt wird. Sie können es aus dem SLES v12 SP2 SDK installieren.

## <a name="sample-script"></a>Beispielskript

```bash
#!/bin/bash -e

# Use the following variables to control your install:

# Password for the SA user (required)
MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>'

# Product ID of the version of SQL server you're installing
# Must be evaluation, developer, express, web, standard, enterprise, or your 25 digit product key
# Defaults to developer
MSSQL_PID='evaluation'

# Install SQL Server Agent (recommended)
SQL_INSTALL_AGENT='y'

# Install SQL Server Full Text Search (optional)
# SQL_INSTALL_FULLTEXT='y'

# Create an additional user with sysadmin privileges (optional)
# SQL_INSTALL_USER='<Username>'
# SQL_INSTALL_USER_PASSWORD='<YourStrong!Passw0rd>'

if [ -z $MSSQL_SA_PASSWORD ]
then
  echo Environment variable MSSQL_SA_PASSWORD must be set for unattended install
  exit 1
fi

echo Adding Microsoft repositories...
sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo
sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo 
sudo zypper --gpg-auto-import-keys refresh

#Add the SLES v12 SP2 SDK to obtain libsss_nss_idmap0
sudo SUSEConnect -p sle-sdk/12.2/x86_64

echo Installing SQL Server...
sudo zypper install -y mssql-server

echo Running mssql-conf setup...
sudo MSSQL_SA_PASSWORD=$MSSQL_SA_PASSWORD \
     MSSQL_PID=$MSSQL_PID \
     /opt/mssql/bin/mssql-conf -n setup accept-eula

echo Installing mssql-tools and unixODBC developer...
sudo ACCEPT_EULA=Y zypper install -y mssql-tools unixODBC-devel

# Add SQL Server tools to the path by default:
echo Adding SQL Server tools to your path...
echo PATH="$PATH:/opt/mssql-tools/bin" >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc

# Optional SQL Server Agent installation:
if [ ! -z $SQL_INSTALL_AGENT ]
then
  echo Installing SQL Server Agent...
  sudo zypper install -y mssql-server-agent
fi

# Optional SQL Server Full Text Search installation:
if [ ! -z $SQL_INSTALL_FULLTEXT ]
then
    echo Installing SQL Server Full-Text Search...
    sudo zypper install -y mssql-server-fts
fi

# Configure firewall to allow TCP port 1433:
echo Configuring SuSEfirewall2 to allow traffic on port 1433...
sudo SuSEfirewall2 open INT TCP 1433
sudo SuSEfirewall2 stop
sudo SuSEfirewall2 start

# Example of setting post-installation configuration options
# Set trace flags 1204 and 1222 for deadlock tracing:
# echo Setting trace flags...
# sudo /opt/mssql/bin/mssql-conf traceflag 1204 1222 on

# Restart SQL Server after making configuration changes:
echo Restarting SQL Server...
sudo systemctl restart mssql-server

# Connect to server and get the version:
counter=1
errstatus=1
while [ $counter -le 5 ] && [ $errstatus = 1 ]
do
  echo Waiting for SQL Server to start...
  sleep 5s
  /opt/mssql-tools/bin/sqlcmd \
    -S localhost \
    -U SA \
    -P $MSSQL_SA_PASSWORD \
    -Q "SELECT @@VERSION" 2>/dev/null
  errstatus=$?
  ((counter++))
done

# Display error if connection failed:
if [ $errstatus = 1 ]
then
  echo Cannot connect to SQL Server, installation aborted
  exit $errstatus
fi

# Optional new user creation:
if [ ! -z $SQL_INSTALL_USER ] && [ ! -z $SQL_INSTALL_USER_PASSWORD ]
then
  echo Creating user $SQL_INSTALL_USER
  /opt/mssql-tools/bin/sqlcmd \
    -S localhost \
    -U SA \
    -P $MSSQL_SA_PASSWORD \
    -Q "CREATE LOGIN [$SQL_INSTALL_USER] WITH PASSWORD=N'$SQL_INSTALL_USER_PASSWORD', DEFAULT_DATABASE=[master], CHECK_EXPIRATION=ON, CHECK_POLICY=ON; ALTER SERVER ROLE [sysadmin] ADD MEMBER [$SQL_INSTALL_USER]"
fi

echo Done!
```

### <a name="running-the-script"></a>Ausführen des Skripts

So führen Sie das Skript aus

1. Fügen Sie das Beispiel in Ihren bevorzugten Text-Editor ein, und speichern Sie es unter einem einprägsamen Namen wie z.B. `install_sql.sh`.

1. Passen Sie `MSSQL_SA_PASSWORD`, `MSSQL_PID` sowie alle weiteren Variablen an, die Sie ändern möchten.

1. Markieren Sie das Skript als ausführbar.

   ```bash
   chmod +x install_sql.sh
   ```

1. Führen Sie das Skript aus.

   ```bash
   ./install_sql.sh
   ```

### <a name="understanding-the-script"></a>Grundlegendes zum Skript
Als Erstes legt das Bash-Skript einige Variablen fest. Dabei kann es sich wie im Beispiel um Skriptvariablen oder um Umgebungsvariablen handeln. Die Variable `MSSQL_SA_PASSWORD` ist für die SQL Server-Installation **erforderlich**, die anderen sind benutzerdefinierte Variablen, die für das Skript erstellt wurden. Das Beispielskript führt folgende Schritte aus:

1. Es importiert die öffentlichen Microsoft-GPG-Schlüssel.

1. Es registriert die Microsoft-Repositorys für SQL Server und die Befehlszeilentools.

1. Es aktualisiert die lokalen Repositorys.

1. Installieren von SQL Server

1. Es konfiguriert SQL Server mit dem ```MSSQL_SA_PASSWORD``` und akzeptiert automatisch den Endbenutzer-Lizenzvertrag.

1. Es akzeptiert automatisch den Endbenutzer-Lizenzvertrag für die SQL Server-Befehlszeilentools, installiert sie und installiert das unixodbc-dev-Paket.

1. Es fügt die SQL Server-Befehlszeilentools zum Pfad hinzu, um die Verwendung zu vereinfachen.

1. Es installiert den SQL Server-Agent, wenn die Skriptvariable ```SQL_INSTALL_AGENT``` festgelegt ist. Standardmäßig ist diese aktiviert.

1. Es installiert optional die SQL Server-Volltextsuche, wenn die Variable ```SQL_INSTALL_FULLTEXT``` festgelegt ist.

1. Es entsperrt Port 1433 für TCP in der Systemfirewall – dies ist erforderlich, um aus einem anderen System eine Verbindung mit SQL Server herzustellen.

1. Es legt optional Ablaufverfolgungsflags für die Nachverfolgung von Deadlocks fest (erfordert das Aufheben der Auskommentierung dieser Zeilen).

1. SQL Server ist jetzt installiert. Um es in Betriebsbereitschaft zu versetzen, startet das Skript den Prozess neu.

1. Das Skript überprüft, ob SQL Server ordnungsgemäß installiert ist und blendet dabei Fehlermeldungen aus.

1. Es erstellt einen neuen Serveradministratorbenutzer, wenn ```SQL_INSTALL_USER``` und ```SQL_INSTALL_USER_PASSWORD``` festgelegt sind.

## <a name="next-steps"></a>Nächste Schritte

Vereinfachen Sie mehrere unbeaufsichtigte Installationen, und erstellen Sie ein eigenständiges Bash-Skript, das die richtigen Umgebungsvariablen festlegt. Sie können alle im Beispielskript verwendeten Variablen entfernen und sie in ein separates Bash-Skript einfügen.

```bash
#!/bin/bash
export MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>'
export MSSQL_PID='evaluation'
export SQL_INSTALL_AGENT='y'
export SQL_INSTALL_USER='<Username>'
export SQL_INSTALL_USER_PASSWORD='<YourStrong!Passw0rd>'
export SQL_INSTALL_AGENT='y'
```

Führen Sie das Bash-Skript dann folgendermaßen aus:
```bash
. ./my_script_name.sh
```

Weitere Informationen zu SQL Server für Linux finden Sie unter [Übersicht über SQL Server für Linux](sql-server-linux-overview.md).
