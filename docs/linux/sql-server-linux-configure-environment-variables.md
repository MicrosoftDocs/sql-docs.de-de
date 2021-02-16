---
title: Konfigurieren von Umgebungsvariablen für SQL Server für Linux
description: In diesem Artikel wird beschrieben, wie Sie Umgebungsvariablen verwenden, um bestimmte SQL Server 2017-Einstellungen unter Linux zu konfigurieren.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: fb9455970c58daa65f8068792a4edc8ae7e20f51
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343169"
---
# <a name="configure-sql-server-settings-with-environment-variables-on-linux"></a>Konfigurieren von SQL Server-Einstellungen mit Umgebungsvariablen unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Sie können mehrere verschiedene Umgebungsvariablen verwenden, um SQL Server 2017 unter Linux zu konfigurieren. Diese Variablen werden in zwei Szenarien verwendet:

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Sie können mehrere verschiedene Umgebungsvariablen verwenden, um SQL Server 2019 unter Linux zu konfigurieren. Diese Variablen werden in zwei Szenarien verwendet:

::: moniker-end

- Zum Konfigurieren der ersten Einrichtung mit dem `mssql-conf setup`-Befehl.
- Zum Konfigurieren eines neuen [SQL Server-Containers in Docker](quickstart-install-connect-docker.md).

> [!TIP]
> Wenn Sie SQL Server nach diesen Setupszenarien konfigurieren müssen, nutzen Sie die Informationen in [Konfigurieren von SQL Server für Linux mit dem mssql-conf-Tool](sql-server-linux-configure-mssql-conf.md).

## <a name="environment-variables"></a>Umgebungsvariablen

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

| Umgebungsvariable | BESCHREIBUNG |
|-----|-----|
| **ACCEPT_EULA** | Legen Sie für die Variable **ACCEPT_EULA** einen beliebigen Wert fest, um Ihre Zustimmung zum [End-User Licensing Agreement (Benutzerlizenzvertrag)](https://go.microsoft.com/fwlink/?LinkId=746388) zu geben. Diese Einstellung ist für das SQL Server-Image zwingend erforderlich. |
| **MSSQL_SA_PASSWORD** | Konfigurieren des Benutzerkennworts des Systemadministrators. |
| **MSSQL_PID** | Festlegen von SQL Server-Edition oder Product Key. Mögliche Werte sind: </br></br>**Auswertung**</br>**Developer**</br>**Express**</br>**Web**</br>**Standard**</br>**Enterprise**</br>**Ein Product Key**</br></br>Wenn ein Product Key angegeben wird, muss er das Format #####-#####-#####-#####-##### aufweisen, wobei „#“ eine Ziffer oder ein Buchstabe ist.|
| **MSSQL_LCID** | Legt die Sprach-ID fest, die für SQL Server verwendet werden soll. Beispiel: 1036 ist Französisch. |
| **MSSQL_COLLATION** | Legt die Standardsortierung für SQL Server fest. Dadurch wird die Standardzuordnung der Sprachen-ID (LCID) zur Sortierung überschrieben. |
| **MSSQL_MEMORY_LIMIT_MB** | Festlegen der maximalen Größe des Arbeitsspeichers (in MB), der von SQL Server verwendet werden kann. Der Standardwert ist 80% des gesamten physischen Speichers. |
| **MSSQL_TCP_PORT** | Konfigurieren des TCP-Ports, auf dem SQL Server lauscht (standardmäßig 1433). |
| **MSSQL_IP_ADDRESS** | Festlegen der IP-Adresse. Derzeit muss die IP-Adresse das IPv4-Format (0.0.0.0) aufweisen. |
| **MSSQL_BACKUP_DIR** | Festlegen des Standardspeicherorts für das Sicherungsverzeichnis. |
| **MSSQL_DATA_DIR** | Ändern des Verzeichnisses zum Erstellen der neuen SQL Server-Datenbank-Datendateien (MDF). |
| **MSSQL_LOG_DIR** | Ändern des Verzeichnisses zum Erstellen der neuen SQL Server-Datenbank-Protokolldateien (LDF). |
| **MSSQL_DUMP_DIR** | Ändern des Verzeichnisses, in dem SQL Server standardmäßig die Speicherabbilder und andere Dateien für die Problembehandlung ablegt. |
| **MSSQL_ENABLE_HADR** | Aktivieren der Verfügbarkeitsgruppe. Beispielsweise bedeutet „1“ aktiviert und „0“ deaktiviert. |
| **MSSQL_AGENT_ENABLED** | Aktivieren des SQL Server-Agents. Beispielsweise bedeutet „true“ aktiviert und „false“ deaktiviert. Standardmäßig ist der Agent deaktiviert.  |
| **MSSQL_MASTER_DATA_FILE** | Festlegen des Speicherorts der Masterdatenbankdatei. Muss bis zur ersten Ausführung von SQL Server als **master.mdf** benannt werden. |
| **MSSQL_MASTER_LOG_FILE** | Festlegen des Speicherorts der Masterdatenbank-Protokolldatei. Muss bis zur ersten Ausführung von SQL Server als **mastlog.mdf** benannt werden. |
| **MSSQL_ERROR_LOG_FILE** | Festlegen des Speicherorts der Protokolldateien. |

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

| Umgebungsvariable | BESCHREIBUNG |
|-----|-----|
| **ACCEPT_EULA** | Legen Sie für die Variable **ACCEPT_EULA** einen beliebigen Wert fest, um Ihre Zustimmung zum [End-User Licensing Agreement (Benutzerlizenzvertrag)](https://go.microsoft.com/fwlink/?LinkId=746388) zu geben. Diese Einstellung ist für das SQL Server-Image zwingend erforderlich. |
| **MSSQL_SA_PASSWORD** | Konfigurieren des Benutzerkennworts des Systemadministrators. |
| **MSSQL_PID** | Festlegen von SQL Server-Edition oder Product Key. Mögliche Werte sind: </br></br>**Auswertung**</br>**Developer**</br>**Express**</br>**Web**</br>**Standard**</br>**Enterprise**</br>**Ein Product Key**</br></br>Wenn ein Product Key angegeben wird, muss er das Format #####-#####-#####-#####-##### aufweisen, wobei „#“ eine Ziffer oder ein Buchstabe ist.|
| **MSSQL_LCID** | Legt die Sprach-ID fest, die für SQL Server verwendet werden soll. Beispiel: 1036 ist Französisch. |
| **MSSQL_COLLATION** | Legt die Standardsortierung für SQL Server fest. Dadurch wird die Standardzuordnung der Sprachen-ID (LCID) zur Sortierung überschrieben. |
| **MSSQL_MEMORY_LIMIT_MB** | Festlegen der maximalen Größe des Arbeitsspeichers (in MB), der von SQL Server verwendet werden kann. Der Standardwert ist 80% des gesamten physischen Speichers. |
| **MSSQL_TCP_PORT** | Konfigurieren des TCP-Ports, auf dem SQL Server lauscht (standardmäßig 1433). |
| **MSSQL_IP_ADDRESS** | Festlegen der IP-Adresse. Derzeit muss die IP-Adresse das IPv4-Format (0.0.0.0) aufweisen. |
| **MSSQL_BACKUP_DIR** | Festlegen des Standardspeicherorts für das Sicherungsverzeichnis. |
| **MSSQL_DATA_DIR** | Ändern des Verzeichnisses zum Erstellen der neuen SQL Server-Datenbank-Datendateien (MDF). |
| **MSSQL_LOG_DIR** | Ändern des Verzeichnisses zum Erstellen der neuen SQL Server-Datenbank-Protokolldateien (LDF). |
| **MSSQL_DUMP_DIR** | Ändern des Verzeichnisses, in dem SQL Server standardmäßig die Speicherabbilder und andere Dateien für die Problembehandlung ablegt. |
| **MSSQL_ENABLE_HADR** | Aktivieren der Verfügbarkeitsgruppe. Beispielsweise bedeutet „1“ aktiviert und „0“ deaktiviert. |
| **MSSQL_AGENT_ENABLED** | Aktivieren des SQL Server-Agents. Beispielsweise bedeutet „true“ aktiviert und „false“ deaktiviert. Standardmäßig ist der Agent deaktiviert.  |
| **MSSQL_MASTER_DATA_FILE** | Festlegen des Speicherorts der Masterdatenbankdatei. Muss bis zur ersten Ausführung von SQL Server als **master.mdf** benannt werden. |
| **MSSQL_MASTER_LOG_FILE** | Festlegen des Speicherorts der Masterdatenbank-Protokolldatei. Muss bis zur ersten Ausführung von SQL Server als **mastlog.mdf** benannt werden. |
| **MSSQL_ERROR_LOG_FILE** | Festlegen des Speicherorts der Protokolldateien. |

::: moniker-end

## <a name="use-with-initial-setup"></a>Verwendung mit der ersten Einrichtung

Dieses Beispiel führt `mssql-conf setup` mit konfigurierten Umgebungsvariablen aus. Die folgenden Umgebungsvariablen werden angegeben:

- **ACCEPT_EULA** akzeptiert die Lizenzbedingungen für Endbenutzer.
- **MSSQL_PID** gibt die frei lizenzierte Developer Edition von SQL Server für die Verwendung außerhalb von Produktionsumgebungen an.
- **MSSQL_SA_PASSWORD** legt ein sicheres Kennwort fest.
- **MSSQL_TCP_PORT** legt 1234 als TCP-Port fest, auf dem SQL Server lauscht.

```bash
sudo ACCEPT_EULA='Y' MSSQL_PID='Developer' MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' MSSQL_TCP_PORT=1234 /opt/mssql/bin/mssql-conf setup
```

## <a name="use-with-docker"></a>Verwendung mit Docker

Dieser Docker-Beispielbefehl verwendet die folgenden Umgebungsvariablen, um einen neuen SQL Server-Container zu erstellen:

- **ACCEPT_EULA** akzeptiert die Lizenzbedingungen für Endbenutzer.
- **MSSQL_PID** gibt die frei lizenzierte Developer Edition von SQL Server für die Verwendung außerhalb von Produktionsumgebungen an.
- **MSSQL_SA_PASSWORD** legt ein sicheres Kennwort fest.
- **MSSQL_TCP_PORT** legt 1234 als TCP-Port fest, auf dem SQL Server lauscht. Dies bedeutet, dass in diesem Beispiel anstelle der Zuordnung von Port 1433 (Standard) zu einem Hostport der benutzerdefinierte TCP-Port mit dem `-p 1234:1234`-Befehl zugeordnet werden muss.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Wenn Sie Docker unter Linux/macOS ausführen, verwenden Sie die folgende Syntax in einfachen Anführungszeichen:

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID='Developer' -e MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2017-latest
```

Wenn Sie Docker unter Windows ausführen, verwenden Sie die folgende Syntax in doppelten Anführungszeichen:

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID="Developer" -e MSSQL_SA_PASSWORD="<YourStrong!Passw0rd>" -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2017-latest
```

> [!NOTE]
> Die Vorgehensweise zum Ausführen von Produktionseditionen in Containern weicht hiervon minimal ab. Weitere Informationen finden Sie unter [Run production container images (Ausführen von Containerimages für Produktionsumgebungen)](./sql-server-linux-docker-container-deployment.md#production).

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Wenn Sie Docker unter Linux/macOS ausführen, verwenden Sie die folgende Syntax in einfachen Anführungszeichen:

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID='Developer' -e MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

Wenn Sie Docker unter Windows ausführen, verwenden Sie die folgende Syntax in doppelten Anführungszeichen:

```bash
docker run -e ACCEPT_EULA=Y -e MSSQL_PID="Developer" -e MSSQL_SA_PASSWORD="<YourStrong!Passw0rd>" -e MSSQL_TCP_PORT=1234 -p 1234:1234 -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

::: moniker-end

## <a name="next-steps"></a>Nächste Schritte

Weitere hier nicht aufgeführte SQL Server-Einstellungen finden Sie unter [Konfigurieren von SQL Server für Linux mit dem mssql-conf-Tool](sql-server-linux-configure-mssql-conf.md).

Weitere Informationen zum Installieren und Ausführen von SQL Server unter Linux finden Sie unter [Installationsanleitung für SQL Server unter Linux](sql-server-linux-setup.md).