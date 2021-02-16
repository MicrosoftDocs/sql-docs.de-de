---
title: Installieren von SQL Server Integration Services unter Linux
description: In diesem Artikel wird beschrieben, wie SQL Server Integration Services (SSIS) unter Linux ausgeführt werden. Sie können SSIS unter Ubuntu 16.04 und Red Hat Enterprise Linux installieren.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 01/09/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 311b8fec7114c4d63680d131ac0988ad4f4a8b15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346361"
---
# <a name="install-sql-server-integration-services-ssis-on-linux"></a>Installieren von SQL Server Integration Services (SSIS) unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Führen Sie die Schritte in diesem Artikel aus, um SQL Server Integration Services (**mssql-server-is**) unter Linux zu installieren. Informationen zu den Features, die in diesem Release von Integration Services für Linux unterstützt werden, finden Sie in den [Versionshinweisen](sql-server-linux-release-notes.md).

SQL Server Integration Services lässt sich auf folgenden Plattformen installieren:

- [Ubuntu 16.04](#ubuntu)
- [Red Hat Enterprise Linux](#RHEL)

## <a name="install-ssis-on-ubuntu"></a><a name="ubuntu"></a> Installieren von SSIS unter Ubuntu

Wenn Sie das **mssql-server-is**-Paket unter Ubuntu installieren möchten, führen Sie die folgenden Schritte aus:

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Importieren Sie die GPG-Schlüssel des öffentlichen Repositorys.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Registrieren Sie das Ubuntu-Repository von SQL Server.

   ```bash
   sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"
   ```

1. Führen Sie die folgenden Befehle aus, um SQL Server Integration Services zu installieren.

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server-is
   ```

1. Führen Sie nach dem Installieren von Integration Services **ssis-conf** aus. Weitere Informationen finden Sie unter [Konfigurieren von SSIS unter Linux mit ssis-conf](sql-server-linux-configure-ssis.md).

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. Nachdem die Konfiguration abgeschlossen ist, legen Sie die PATH-Umgebungsvariable fest.

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Importieren Sie die GPG-Schlüssel des öffentlichen Repositorys.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Registrieren Sie das Ubuntu-Repository von SQL Server.

   ```bash
   sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2019.list)"
   ```

1. Führen Sie die folgenden Befehle aus, um SQL Server Integration Services zu installieren.

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server-is
   ```

1. Führen Sie nach dem Installieren von Integration Services **ssis-conf** aus. Weitere Informationen finden Sie unter [Konfigurieren von SSIS unter Linux mit ssis-conf](sql-server-linux-configure-ssis.md).

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. Nachdem die Konfiguration abgeschlossen ist, legen Sie die PATH-Umgebungsvariable fest.

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

### <a name="update-ssis"></a>Aktualisieren von SSIS

Wenn Sie **mssql-server-is** bereits installiert haben, können Sie es mit dem folgenden Befehl auf die neueste Version aktualisieren:

```bash
sudo apt-get install mssql-server-is
```

### <a name="remove-ssis"></a>Entfernen von SSIS

Führen Sie den folgenden Befehl aus, um **mssql-server-is** zu entfernen:

```bash
sudo apt-get remove mssql-server-is
```

## <a name="install-ssis-on-rhel"></a><a name="RHEL"></a> Installieren von SSIS unter RHEL
Wenn Sie das **mssql-server-is**-Paket unter RHEL installieren möchten, führen Sie die folgenden Schritte aus:

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Laden Sie die Konfigurationsdatei für das SQL Server-Red Hat-Repository herunter.

   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo
   ```

1. Führen Sie den folgenden Befehl aus, um SQL Server Integration Services zu installieren.

   ```bash
   sudo yum install -y mssql-server-is
   ```

1. Führen Sie nach der Installation **ssis-conf** aus. Weitere Informationen finden Sie unter [Konfigurieren von SSIS unter Linux mit ssis-conf](sql-server-linux-configure-ssis.md).

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. Nachdem die Konfiguration abgeschlossen ist, legen Sie die PATH-Umgebungsvariable fest.

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Laden Sie die Konfigurationsdatei für das SQL Server-Red Hat-Repository herunter.

   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo
   ```

1. Führen Sie den folgenden Befehl aus, um SQL Server Integration Services zu installieren.

   ```bash
   sudo yum install -y mssql-server-is
   ```

1. Führen Sie nach der Installation **ssis-conf** aus. Weitere Informationen finden Sie unter [Konfigurieren von SSIS unter Linux mit ssis-conf](sql-server-linux-configure-ssis.md).

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. Nachdem die Konfiguration abgeschlossen ist, legen Sie die PATH-Umgebungsvariable fest.

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

::: moniker-end

### <a name="update-ssis"></a>Aktualisieren von SSIS

Wenn Sie **mssql-server-is** bereits installiert haben, können Sie es mit dem folgenden Befehl auf die neueste Version aktualisieren:

```bash
sudo yum update mssql-server-is
```

### <a name="remove-ssis"></a>Entfernen von SSIS
Führen Sie den folgenden Befehl aus, um **mssql-server-is** zu entfernen:

```bash
sudo yum remove mssql-server-is
```

## <a name="unattended-installation"></a>Unbeaufsichtigte Installation

Wenn **ssis-conf setup** unbeaufsichtigt installiert werden soll, führen Sie die folgenden Schritte aus:

1. Geben Sie die Option **-n** (keine Aufforderung) an.
1. Geben Sie erforderliche Werte an, indem Sie Umgebungsvariablen festlegen.

Im folgenden Beispiel werden die folgenden Aktionen ausgeführt:

- SSIS wird installiert.
- Die Developer Edition wird durch Angabe eines Werts für die SSIS_PID-Umgebungsvariable festgelegt.
- Akzeptiert die Software-Lizenzbedingungen von Microsoft durch Angabe eines Werts für die ACCEPT_EULA-Umgebungsvariable.
- Führt eine unbeaufsichtigte Installation durch Angabe der Option **-n** (keine Aufforderung) aus.

```
sudo SSIS_PID=Developer ACCEPT_EULA=Y /opt/ssis/bin/ssis-conf -n setup 
```

### <a name="environment-variables-for-unattended-installation"></a>Umgebungsvariablen für die unbeaufsichtigte Installation

| Umgebungsvariable | BESCHREIBUNG |
|---|---|
| ACCEPT_EULA | Akzeptiert die SQL Server-Lizenzbedingungen bei Festlegung auf einen beliebigen Wert wie „Y“.|
| SSIS_PID | Legt die SQL Server-Edition oder den Product Key fest. Die folgenden Werte sind möglich:<ul><li>Auswertung</li><li>Entwickler</li><li>Express</li><li>Web</li><li>Standard</li><li>Enterprise</li><li>Ein Product Key</li></ul>Wenn Sie einen Product Key angeben, muss der er das Format *#####* - *#####* - *#####* - *#####* - *#####* haben, wobei *#* für einen Buchstaben oder eine Ziffer stehen kann.  |
| | |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Ausführen von SSIS-Paketen unter Linux finden Sie unter [Extrahieren, Transformieren und Laden von Daten für SQL Server für Linux mit SSIS](sql-server-linux-migrate-ssis.md).

Informationen zum Konfigurieren zusätzlicher SSIS-Einstellungen unter Linux finden Sie unter [Konfigurieren von SQL Server Integration Services unter Linux mit ssis-conf](sql-server-linux-configure-ssis.md).

## <a name="related-content-about-ssis-on-linux"></a>Verwandte Inhalte zu SSIS unter Linux

- [Extrahieren, Transformieren und Laden von Daten unter Linux mit SSIS](sql-server-linux-migrate-ssis.md)
- [Konfigurieren von SQL Server Integration Services unter Linux mit ssis-conf](sql-server-linux-configure-ssis.md)
- [Einschränkungen und bekannte Probleme bei SSIS unter Linux](sql-server-linux-ssis-known-issues.md)
- [Zeitliches Planen der Ausführung von SSIS-Paketen unter Linux mit Cron](sql-server-linux-schedule-ssis-packages.md)
