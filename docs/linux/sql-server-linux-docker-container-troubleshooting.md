---
title: Problembehandlung von SQL Server Docker-Containern
description: Untersuchen Sie die verschiedenen Problembehandlungstechniken, die Sie verwenden können, um häufige Fehler zu beheben, die bei der Verwendung von Linux Docker-Containern mit SQL Server-Images auftreten.
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017'
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: d01a4b20b12dfdf47e813fa51d5862e5ce8f0cb6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273061"
---
# <a name="troubleshooting-sql-server-docker-containers"></a>Problembehandlung von SQL Server Docker-Containern

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel werden häufige Fehler erläutert, die beim Bereitstellen und Verwenden von SQL Server Docker-Containern auftreten können, sowie Problembehandlungstechniken zum Beheben des jeweiligen Problems.

## <a name="docker-command-errors"></a>Fehler bei Docker-Befehlen

Wenn Fehler bei `docker`-Befehlen auftreten, sollten Sie sicherstellen, dass der Docker-Dienst ausgeführt wird und versuchen, diesen mit erhöhten Berechtigungen auszuführen.

Unter Linux kann beispielsweise folgender Fehler auftreten, wenn Sie `docker`-Befehle ausführen:

```output
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

Wenn dieser Fehler unter Linux auftritt, versuchen Sie, diese Befehle mit `sudo` auszuführen. Wenn dieser Versuch fehlschlägt, sollten Sie überprüfen, ob der Docker-Dienst ausgeführt wird und diesen ggf. starten.

```bash
sudo systemctl status docker
sudo systemctl start docker
```

Vergewissern Sie sich unter Windows, dass Sie PowerShell oder die Eingabeaufforderung als Administrator starten.

## <a name="sql-server-container-startup-errors"></a>Startfehler bei SQL Server-Containern

Wenn der SQL Server-Container nicht ausgeführt werden kann, versuchen Sie Folgendes:

- Wenn Sie einen Fehler wie `failed to create endpoint CONTAINER_NAME on network bridge. Error starting proxy: listen tcp 0.0.0.0:1433 bind: address already in use.`erhalten, versuchen Sie, den Containerport 1433 einem bereits verwendeten Port zuzuordnen. Dieser Fall kann eintreten, wenn Sie SQL Server lokal auf dem Hostcomputer ausführen. Dieser Fall kann außerdem eintreten, wenn Sie zwei SQL Server-Container starten und versuchen, diese demselben Hostport zuzuordnen. Verwenden Sie in diesem Fall den Parameter `-p`, um den Containerport 1433 einem anderen Hostport zuzuordnen. Beispiel: 

    <!--SQL Server 2017 on Linux -->
    ::: moniker range="= sql-server-linux-2017 || = sql-server-2017"
    
    ::: zone pivot="cs1-bash"
    ```bash
    docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-powershell"
    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-cmd"
    ```cmd
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2017-latest`.
    ```
    ::: zone-end
    
    ::: moniker-end
    
    <!--SQL Server 2019 on Linux-->
    ::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15"
    
    ::: zone pivot="cs1-bash"
    ```bash
    docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-powershell"
    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: zone pivot="cs1-cmd"
    ```cmd
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1400:1433 -d mcr.microsoft.com/mssql/server:2019-latest`.
    ```
    ::: zone-end
    
    ::: moniker-end

- Wenn Sie beim Versuch, einen Container zu starten, einen Fehler wie `Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.30tdout=1&tail=all: dial unix /var/run/docker.sock: connect: permission denied` erhalten, fügen Sie den Benutzer der Docker-Gruppe in Ubuntu hinzu. Melden Sie sich ab und wieder an, da sich diese Änderung auf neue Sitzungen auswirkt. 

   ```bash
    usermod -aG docker $USER
   ```

- Überprüfen Sie, ob Fehlermeldungen in Bezug auf den Container vorhanden sind.

   ```bash
   docker logs e69e056c702d
   ```

- Stellen Sie sicher, dass die Mindestanforderungen für den Arbeitsspeicher und den Datenträger erfüllt werden, die im Abschnitt [Voraussetzungen](quickstart-install-connect-docker.md#requirements) des Schnellstartartikels aufgelistet werden.

- Wenn Sie Containerverwaltungssoftware verwenden, müssen Sie sicherstellen, dass diese die Ausführung von Containerprozessen als Root unterstützt. Der Prozess „sqlservr“ im Container wird als Root ausgeführt.

- Wenn Ihr SQL Server Docker-Container sofort nach dem Start beendet wird, überprüfen Sie die Docker-Protokolle. Wenn Sie PowerShell unter Windows mit dem Befehl `docker run` verwenden, verwenden Sie doppelte Anführungszeichen anstelle von einfachen Anführungszeichen. Verwenden Sie mit PowerShell Core einfache Anführungszeichen.

- Überprüfen Sie die [Setup- und Fehlerprotokolle für SQL Server](#errorlogs).

## <a name="enable-dump-captures"></a>Aktivieren der Sicherungserfassung

Wenn der SQL Server-Prozess innerhalb des Containers fehlschlägt, sollten Sie einen neuen Container erstellen, bei dem **SYS_PTRACE** aktiviert ist. Dadurch kann die Linux-Funktion zur Nachverfolgung eines Prozesses genutzt werden, die für das Erstellen einer Sicherungsdatei für eine Ausnahme erforderlich ist. Die Sicherungsdatei kann vom Support genutzt werden, um das Problem zu beheben. Die Funktion wird mit folgendem docker run-Befehl aktiviert.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -e 'MSSQL_PID=Developer' --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -e 'MSSQL_PID=Developer' --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -e "MSSQL_PID=Developer" --cap-add SYS_PTRACE -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="sql-server-connection-failures"></a>SQL Server-Verbindungsfehler

Wenn Sie keine Verbindung mit der SQL Server-Instanz herstellen können, die in Ihrem Container ausgeführt wird, versuchen Sie Folgendes:

- Stellen Sie sicher, dass Ihr SQL Server-Container ausgeführt wird, indem Sie die Spalte **STATUS** der Ausgabe von `docker ps -a` überprüfen. Starten Sie ihn andernfalls mit `docker start <Container ID>`.

- Wenn Sie den Container nicht dem Standardhostport 1433 zugeordnet haben, müssen Sie den Port in der Verbindungszeichenfolge angeben. Die Portzuordnung ist in der Spalte **PORTS** der Ausgabe von `docker ps -a` ersichtlich. Folgender Befehl verbindet sqlcmd beispielsweise mit einem Container, der Port 1401 lauscht:

    ::: zone pivot="cs1-bash"
    ```bash
    sqlcmd -S 10.3.2.4,1401 -U SA -P '<YourPassword>'
    ```
    ::: zone-end

    ::: zone pivot="cs1-powershell"
    ```PowerShell
    sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
    ```
    ::: zone-end

    ::: zone pivot="cs1-cmd"
    ```cmd
    sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
    ```
    ::: zone-end

- Wenn Sie `docker run` mit einem vorhandenen zugeordneten Datenvolume oder Datenvolumecontainer verwendet haben, ignoriert SQL Server den Wert von `SA_PASSWORD`. Stattdessen wird das vorkonfigurierte SA-Benutzerkennwort aus den SQL Server-Daten im Datenvolume oder Datenvolumecontainer verwendet. Vergewissern Sie sich, dass Sie das SA-Kennwort verwenden, das den Daten zugeordnet ist, an die Sie anfügen.

- Überprüfen Sie die [Setup- und Fehlerprotokolle für SQL Server](#errorlogs).

## <a name="sql-server-availability-groups"></a>SQL Server-Verfügbarkeitsgruppen

Wenn Sie Docker mit SQL Server-Verfügbarkeitsgruppen verwenden, gibt es zwei zusätzliche Voraussetzungen.

- Ordnen Sie den Port zu, der für die Replikatkommunikation verwendet wird (standardmäßig 5022). Geben Sie in Ihrem `docker run`-Befehl beispielsweise `-p 5022:5022` an.

- Legen Sie den Containerhostnamen mit dem Parameter `-h YOURHOSTNAME` des Befehls `docker run` explizit fest. Dieser Hostname wird verwendet, wenn Sie die Verfügbarkeitsgruppe konfigurieren. Wenn Sie ihn nicht mit `-h` angeben, wird standardmäßig die **Container-ID** verwendet.

## <a name="sql-server-setup-and-error-logs"></a><a id="errorlogs"></a> Setup- und Fehlerprotokolle für SQL Server

Sie finden Sie Setup- und Fehlerprotokolle für SQL Server unter **/var/opt/mssql/log**. Wenn der Container nicht ausgeführt wird, starten Sie diesen zunächst. Verwenden Sie dann eine interaktive Eingabeaufforderung, um die Protokolle zu überprüfen. Die Container-ID können Sie durch Ausführen des Befehls `docker ps` abrufen.

```bash
docker start <ContainerID>
docker exec -it <ContainerID> "bash"
```

Führen Sie folgende Befehle über die Bash-Sitzung in Ihrem Container aus:

```bash
cd /var/opt/mssql/log
cat setup*.log
cat errorlog
```

> [!TIP]
> Wenn Sie bei der Erstellung des Containers ein Hostverzeichnis in **/var/opt/mssql** eingebunden haben, können Sie stattdessen im Unterverzeichnis **log** des auf dem Host zugeordneten Pfads suchen.

## <a name="execute-commands-in-a-container"></a>Ausführen von Befehlen in einem Container

Wenn Sie bereits einen Container ausführen, können Sie über ein Hostterminal Befehle innerhalb des Containers ausführen.

So rufen Sie die Container-ID ab:

```bash
docker ps -a
```

So starten Sie ein Bash-Terminal in der Containerausführung:

```bash
docker exec -it <Container ID> /bin/bash
```

Nun können Sie Befehle wie über das Terminal innerhalb des Containers ausführen. Wenn Sie fertig sind, geben Sie `exit` ein. Dadurch wird die interaktive Befehlssitzung beendet, aber der Container wird weiter ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Der [Schnellstart](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true) erleichtert Ihnen den Einstieg in die Verwendung von SQL Server 2017-Containerimages in Docker.

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Der [Schnellstart](quickstart-install-connect-docker.md) erleichtert Ihnen den Einstieg in die Verwendung von SQL Server 2019-Containerimages in Docker.

::: moniker-end

- [Bereitstellen von und Herstellen einer Verbindung mit SQL Server Docker-Containern](sql-server-linux-docker-container-deployment.md)

- [Verweisen auf zusätzliche Konfiguration und Anpassung für Docker-Container](sql-server-linux-docker-container-configure.md)

- [Sichern von SQL Server Docker-Containern](sql-server-linux-docker-container-security.md)
