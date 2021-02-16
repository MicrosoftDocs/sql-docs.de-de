---
title: Konfigurieren des SQL Server-Agent unter Linux
description: Erfahren Sie, wie Sie den SQL Server-Agent unter Linux installieren. Ab SQL Server 2017 CU4 ist der SQL Server-Agent im mssql-server-Paket enthalten.
author: VanMSFT
ms.author: vanto
ms.date: 12/05/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 77f16adc-e6cb-4a57-82f3-7b9780369868
ms.openlocfilehash: 8ca8de4b922ad2108ad40996713cf47501b29dca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346394"
---
# <a name="install-sql-server-agent-on-linux"></a>Installieren des SQL Server-Agent unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel wird beschrieben, wie der SQL Server-Agent unter Linux aktiviert oder installiert wird.

Der [SQL Server-Agent](../ssms/agent/sql-server-agent.md) führt geplante SQL Server-Aufträge aus. Ab SQL Server 2017 CU4 ist der SQL Server-Agent im **mssql-server**-Paket enthalten und standardmäßig deaktiviert. Informationen zu den Funktionen, die in dieser Version des SQL Server-Agent unterstützt werden, sowie Versionsinformationen finden Sie in den [Versionshinweisen](sql-server-linux-release-notes.md).

## <a name="instructions"></a>Instructions

Führen Sie die folgenden Schritte aus, bevor Sie den SQL Server-Agent unter Linux verwenden, um diesen zu aktivieren oder zu installieren.

1. Fügen Sie den Hostnamen (mit und ohne Domäne) in den `/etc/hosts`-Dateien hinzu. In den folgenden Zeilen sehen Sie ein Beispiel für das Format für diese Einträge:

   ```bash
   "IP Address" "hostname"
   "IP Address" "hostname.domain.com"
   ```

1. Befolgen Sie die Anweisungen in dem Abschnitt, der auf Ihrer Version von SQL Server basiert:

   | Versionen | Instructions |
   |---|---|
   | SQL Server 2017 CU4 und höher</br>SQL Server 2019 | [Aktivieren des SQL Server-Agent](#EnableAgentAfterCU4) |
   | SQL Server 2017 CU3 und niedriger | [Installieren des SQL Server-Agent](#InstallAgentBelowCU4) |

## <a name="enable-the-sql-server-agent"></a><a id="EnableAgentAfterCU4"></a>Aktivieren des SQL Server-Agent

Bei SQL Server 2019 und SQL Server 2017 CU4 und höher müssen Sie den SQL Server-Agent nur aktivieren. Sie müssen kein separates Paket installieren.

Um den SQL Server-Agent zu aktivieren, führen Sie die folgenden Schritte aus.

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
sudo systemctl restart mssql-server
```

> [!NOTE]
> Wenn Sie von 2017 CU3 oder darunter mit installiertem Agent ein Upgrade durchführen, wird der SQL Server-Agent automatisch aktiviert, und frühere Agent-Pakete werden deinstalliert.  

## <a name="install-the-sql-server-agent"></a><a name="InstallAgentBelowCU4"></a>Installieren des SQL Server-Agent

Für SQL Server 2017 CU3 und früher müssen Sie das Paket für den SQL Server-Agent installieren.

> [!NOTE]
> Die folgenden Installationsanweisungen gelten für die SQL Server-Versionen bis einschließlich 2017 CU3. Bevor Sie die den SQL Server-Agent installieren, müssen Sie zunächst [SQL Server installieren](sql-server-linux-setup.md#platforms). Dadurch werden die Schlüssel und Repositorys konfiguriert, die Sie verwenden, wenn Sie das **mssql-server-agent**-Paket installieren.

Installieren Sie den SQL Server-Agent für Ihre Plattform:
- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)

### <a name=""></a><a name="RHEL">Installation unter RHEL</a>

Führen Sie zum Installieren von **mssql-server-agent** unter Red Hat Enterprise Linux die folgenden Schritte aus. 

```bash
sudo yum install mssql-server-agent
sudo systemctl restart mssql-server
```

Wenn Sie **mssql-server-agent** bereits installiert haben, können Sie mit den folgenden Befehlen auf die neueste Version aktualisieren:

```bash
sudo yum check-update
sudo yum update mssql-server-agent
sudo systemctl restart mssql-server
```

Wenn Sie eine Offlineinstallation benötigen, suchen Sie in den [Versionshinweisen](sql-server-linux-release-notes.md) nach der Downloadmöglichkeit für das SQL Server-Agent-Paket. Führen Sie dann die im Artikel [Installieren von SQL Server](sql-server-linux-setup.md#offline) beschriebenen Schritte für die Offlineinstallation aus.

### <a name=""></a><a name="ubuntu">Installation unter Ubuntu</a>

Führen Sie zum Installieren von **mssql-server-agent** unter Ubuntu die folgenden Schritte aus. 

```bash
sudo apt-get update 
sudo apt-get install mssql-server-agent
sudo systemctl restart mssql-server
```

Wenn Sie **mssql-server-agent** bereits installiert haben, können Sie mit den folgenden Befehlen auf die neueste Version aktualisieren:

```bash
sudo apt-get update 
sudo apt-get install mssql-server-agent
sudo systemctl restart mssql-server
```

Wenn Sie eine Offlineinstallation benötigen, suchen Sie in den [Versionshinweisen](sql-server-linux-release-notes.md) nach der Downloadmöglichkeit für das SQL Server-Agent-Paket. Führen Sie dann die im Artikel [Installieren von SQL Server](sql-server-linux-setup.md#offline) beschriebenen Schritte für die Offlineinstallation aus.

### <a name=""></a><a name="SLES">Installation unter SLES</a>

Führen Sie zum Installieren von **mssql-server-agent** unter SUSE Linux Enterprise Server die folgenden Schritte aus. 

Installieren Sie **mssql-server-agent**. 

```bash
sudo zypper install mssql-server-agent
sudo systemctl restart mssql-server
```

Wenn Sie **mssql-server-agent** bereits installiert haben, können Sie mit den folgenden Befehlen auf die neueste Version aktualisieren:

```bash
sudo zypper refresh
sudo zypper update mssql-server-agent
sudo systemctl restart mssql-server
```

Wenn Sie eine Offlineinstallation benötigen, suchen Sie in den [Versionshinweisen](sql-server-linux-release-notes.md) nach der Downloadmöglichkeit für das SQL Server-Agent-Paket. Führen Sie dann die im Artikel [Installieren von SQL Server](sql-server-linux-setup.md#offline) beschriebenen Schritte für die Offlineinstallation aus.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen dazu, wie der SQL Server-Agent verwendet wird, um Aufträge zu erstellen, zu planen und auszuführen, finden Sie unter [Erstellen und Ausführen von SQL Server-Agent-Aufträgen unter Linux](sql-server-linux-run-sql-server-agent-job.md).