---
title: Bereitstellen von und Herstellen einer Verbindung mit SQL Server Docker-Containern
description: Erfahren Sie, wie SQL Server in Docker-Containern bereitgestellt werden kann, und erfahren Sie mehr über verschiedene Tools zum Herstellen einer Verbindung mit SQL Server innerhalb und außerhalb des Containers.
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 82737f18-f5d6-4dce-a255-688889fdde69
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: f789dd2b80515116e7bf0a12e26e6e8fe6c45b03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273101"
---
# <a name="deploy-and-connect-to-sql-server-docker-containers"></a>Bereitstellen von und Herstellen einer Verbindung mit SQL Server Docker-Containern

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel werden das Bereitstellen von und das Herstellen einer Verbindung mit SQL Server Docker-Containern beschrieben.

Informationen zu weiteren Bereitstellungsszenarios finden Sie unter:

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Linux](../linux/sql-server-linux-setup.md)
- [Kubernetes: Big Data-Cluster](../big-data-cluster/deploy-get-started.md)

> [!NOTE]
> In diesem Artikel liegt der Fokus auf dem Image „mssql-sever-linux“. Das Windows-Image wird hier zwar nicht behandelt, aber auf der [Docker Hub-Seite zu „mssql-server-windows“](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/) erhalten Sie weitere Informationen.

> [!IMPORTANT]
> Bevor Sie sich für die Ausführung eines SQL Server-Containers für Anwendungsfälle in der Produktion entscheiden, lesen Sie unsere [Richtlinie zur Unterstützung von SQL Server-Containern](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server), um sicherzustellen, dass Sie mit einer unterstützten Konfiguration arbeiten.

Dieses 6-minütige Video enthält eine Einführung in die Ausführung von SQL Server in Containern:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2019-in-Containers/player?WT.mc_id=dataexposed-c9-niner]

## <a name="pull-and-run-the-container-image"></a>Übertragen mithilfe von Pull und Ausführen von Containerimages

Wenn Sie Docker-Containerimages für SQL Server 2017 und SQL Server 2019 herunterladen und ausführen möchten, sollten Sie zunächst die im folgenden Schnellstart beschriebenen Voraussetzungen schaffen und die Schritte ausführen:

- [Ausführen des SQL Server 2017-Containerimages mit Docker](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true)
- [Ausführen des SQL Server 2019-Containerimages mit Docker](quickstart-install-connect-docker.md?view=sql-server-ver15&preserve-view=true)

In diesem Artikel zur Konfiguration werden zusätzliche Anwendungsfälle beschrieben.

## <a name="connect-and-query"></a>Herstellen einer Verbindung und Ausführen von Abfragen

Sie können von innerhalb oder außerhalb des Containers eine Verbindung mit einer SQL Server-Instanz herstellen, die in einem Container ausgeführt wird, und Abfragen an diese senden. In den folgenden Abschnitten werden beide Szenarios erläutert.

### <a name="tools-outside-the-container"></a>Tools außerhalb des Containers

Sie können über jedes externe Linux-, Windows- oder macOS-Tool, das SQL-Verbindungen unterstützt, eine Verbindung mit der SQL Server-Instanz auf Ihrem Docker-Computer herstellen. Folgende Tools werden häufig verwendet:

- [Azure Data Studio](../azure-data-studio/quickstart-sql-server.md)
- [sqlcmd](sql-server-linux-setup-tools.md)
- [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)
- [SQL Server Management Studio (SSMS) unter Windows](sql-server-linux-manage-ssms.md)

Im folgenden Beispiel wird **sqlcmd** verwendet, um eine Verbindung mit einer SQL Server-Instanz herzustellen, die in einem Docker-Container ausgeführt wird. Die IP-Adresse in der Verbindungszeichenfolge ist die IP-Adresse des Hostcomputers, auf dem der Container ausgeführt wird.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4 -U SA -P "<YourPassword>"
```
::: zone-end

Wenn Sie eine Hostport zugeordnet haben, der nicht dem Standardport **1433** entspricht, fügen Sie diesen Port zur Verbindungszeichenfolge hinzu. Wenn Sie beispielsweise `-p 1400:1433` in Ihrem `docker run`-Befehl angegeben haben, stellen Sie die Verbindung her, indem Sie explizit Port 1400 angeben.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4,1400 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4,1400 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4,1400 -U SA -P "<YourPassword>"
```
::: zone-end

### <a name="tools-inside-the-container"></a>Tools innerhalb des Containers

Ab SQL Server 2017 sind die [SQL Server-Befehlszeilentools](sql-server-linux-setup-tools.md) im Containerimage enthalten. Wenn Sie diese mit einer interaktiven Befehlszeile an das Image anfügen, können Sie die Tools lokal ausführen.

1. Verwenden Sie den Befehl `docker exec -it`, um in Ihrem laufenden Container eine interaktive Bash-Shell zu starten. Im folgenden Beispiel entspricht `e69e056c702d` der Container-ID.

    ```bash
    docker exec -it e69e056c702d "bash"
    ```

    > [!TIP]
    > Sie müssen nicht immer die gesamte Container-ID angeben. Sie müssen nur ausreichend Zeichen angeben, um diese eindeutig zu identifizieren. In diesem Beispiel reicht es ggf. aus, `e6` oder `e69` anstelle der vollständigen ID zu verwenden. Führen Sie den Befehl `docker ps -a` aus, um die Container-ID zu ermitteln.

2. Stellen Sie eine lokale Verbindung mit „sqlcmd“ her. „Sqlcmd“ verwendet nicht automatisch den richtigen Pfad. Sie müssen daher selbst den vollständigen Pfand angeben.

    ```bash
    /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '<YourPassword>'
    ```

3. Wenn Sie mit sqlcmd fertig sind, geben Sie `exit` ein.

4. Wenn Sie mit der interaktiven Eingabeaufforderung fertig sind, geben Sie `exit` ein. Der Container wird auch nach dem Beenden der interaktiven Bash-Shell weiter ausgeführt.

## <a name="check-the-container-version"></a><a id="version"></a> Überprüfen der Containerversion

Führen Sie folgenden Befehl aus, um die SQL Server-Version in einem ausgeführten Docker-Container zu ermitteln. Ersetzen Sie `<Container ID or name>` durch die Zielcontainer-ID oder den Namen. Ersetzen Sie `<YourStrong!Passw0rd>` durch das SQL Server-Kennwort für die SA-Anmeldung.

::: zone pivot="cs1-bash"
```bash
sudo docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd \
-S localhost -U SA -P '<YourStrong!Passw0rd>' \
-Q 'SELECT @@VERSION'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd `
-S localhost -U SA -P "<YourStrong!Passw0rd>" `
-Q "SELECT @@VERSION"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker exec -it <Container ID or name> /opt/mssql-tools/bin/sqlcmd ^
-S localhost -U SA -P "<YourStrong!Passw0rd>" ^
-Q "SELECT @@VERSION"
```
::: zone-end

Sie können auch die SQL Server-Version und die Buildnummer für ein Docker-Zielcontainerimage ermitteln. Der folgende Befehl zeigt die SQL Server-Version und die Buildinformationen für das Image **mcr.microsoft.com/mssql/server:2017-latest** an. Hierfür wird ein neuer Container mit der Umgebungsvariable **PAL_PROGRAM_INFO=1** ausgeführt. Der daraus resultierende Container wird sofort beendet und durch den Befehl `docker rm` entfernt.

::: zone pivot="cs1-bash"
```bash
sudo docker run -e PAL_PROGRAM_INFO=1 --name sqlver \
-ti mcr.microsoft.com/mssql/server:2019-latest && \
sudo docker rm sqlver
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e PAL_PROGRAM_INFO=1 --name sqlver `
-ti mcr.microsoft.com/mssql/server:2019-latest; `
docker rm sqlver
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e PAL_PROGRAM_INFO=1 --name sqlver ^
-ti mcr.microsoft.com/mssql/server:2019-latest && ^
docker rm sqlver
```
::: zone-end

In den vorherigen Befehlen werden Versionsinformationen angezeigt, die der folgenden Ausgabe ähneln:

```output
sqlservr
  Version 15.0.4063.15
  Build ID 8a3bb4cca325e1d0b3071b3a193f6a1d74b440fbd95d2fb18881651a5b9ec8e8
  Build Type release
  Git Version 0335c462
  Built at Fri Aug 28 04:50:27 GMT 2020

PAL
  Build ID cc5ceea1b3d294f7d0166f99932f98c7eacfaaa81bcd7cf23c6a89f785829b63
  Build Type release
  Git Version ae9d66dff
  Built at Fri Aug 28 04:46:48 GMT 2020

Packages
  system.security                         6.2.9200.10,unset,
  system.certificates                     6.2.9200.10,unset,
  secforwarderxplat                       15.0.4063.15
  sqlservr                                15.0.4063.15
  system.common                           10.0.17134.1246.202005133
  system.netfx                            4.7.2.461814
  system                                  6.2.9200.10,unset,
  sqlagent                                15.0.4063.15
```

## <a name="run-a-specific-sql-server-container-image"></a><a id="tags"></a> Ausführen eines bestimmten SQL Server-Containerimages

> [!NOTE]
> Ab dem kumulativen Update 3 von SQL Server 2019 (CU3) wird Ubuntu 18.04 unterstützt. Sie können eine Liste aller verfügbaren Tags für mssql/server unter <https://mcr.microsoft.com/v2/mssql/server/tags/list> abrufen.

In manchen Fällen möchten Sie nicht das neueste SQL Server-Containerimage verwenden. Mithilfe der folgenden Schritte können Sie ein bestimmtes SQL Server-Containerimage ausführen:

1. Ermitteln Sie das **Docker-Tag** für das Release, das Sie verwenden möchten. Die verfügbaren Tags finden Sie auf der [Docker Hub-Seite zu „mssql-server-linux“](https://hub.docker.com/_/microsoft-mssql-server).

2. Laden Sie das SQL Server-Containerimage mit diesem Tag herunter. Wenn Sie z. B. das 2019-CU7-ubuntu-18.04-Image herunterladen möchten, ersetzen Sie `<image_tag>` im folgenden Befehl durch `2019-CU7-ubuntu-18.04`.

   ```bash
   docker pull mcr.microsoft.com/mssql/server:<image_tag>
   ```

3. Wenn Sie einen neuen Container mit diesem Image ausführen möchten, geben Sie den Tagnamen im Befehl `docker run` an. Ersetzen Sie im folgenden Befehl `<image_tag>` durch die Version, die Sie ausführen möchten.

   ::: zone pivot="cs1-bash"
   ```bash
   docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:<image_tag>
   ```
   ::: zone-end

Diese Schritte können auch zum Herabstufen eines vorhandenen Containers verwendet werden. Sie können beispielsweise einen Rollback oder ein Downgrade für einen Container ausführen, um Probleme zu beheben oder Tests durchzuführen. Zum Herabstufen eines ausgeführten Containers müssen Sie eine Persistenzmethode für den Datenordner verwenden. Führen Sie die Schritte durch, die im [Abschnitt zu Upgrades](#upgrade) beschrieben werden, aber geben Sie den Tagnamen der älteren Version an, wenn Sie den neuen Container ausführen.

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="run-rhel-based-container-images"></a><a id="rhel"></a> Ausführen von RHEL-basierten Containerimages

Die Dokumentation für SQL Server-Containerimages für Linux bezieht sich auf Ubuntu-basierte Container. Ab SQL Server 2019 können Sie auch RHEL-basierte Container (Red Hat Enterprise Linux) verwenden. Ein Beispiel für das Image für RHEL sieht wie folgt aus: **mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8**.

Der folgende Befehl ruft beispielsweise das kumulative Update 1 für SQL Server 2019 ab, für das RHEL 8 verwendet wird:

::: zone pivot="cs1-bash"
```bash
sudo docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker pull mcr.microsoft.com/mssql/rhel/server:2019-CU1-rhel-8
```
::: zone-end

::: moniker-end

## <a name="run-production-container-images"></a><a id="production"></a> Ausführen von Produktionscontainerimages

Im [Schnellstart](quickstart-install-connect-docker.md) im vorherigen Abschnitt wird die kostenlose Developer Edition von SQL Server über Docker Hub ausgeführt. Die meisten Angaben gelten weiterhin, wenn Sie Produktionscontainerimages wie für die Enterprise-, Standard- und Web-Edition ausführen möchten. Es gibt jedoch einige Unterschiede, die im Folgenden erläutert werden.

- Sie können SQL Server nur in einer Produktionsumgebung verwenden, wenn Sie über eine gültige Lizenz verfügen. Sie können eine [kostenlose SQL Server Express-Lizenz](https://go.microsoft.com/fwlink/?linkid=857693) für die Produktion erwerben. Lizenzen für SQL Server Standard und SQL Server Enterprise sind über die [Microsoft-Volumenlizenzierung](https://www.microsoft.com/licensing/default.aspx) verfügbar.

- Das Developer-Containerimage kann so konfiguriert werden, dass auch die Produktionseditionen ausgeführt werden können. Führen Sie Folgendes durch, um Produktionseditionen auszuführen:

Machen Sie sich im [Schnellstart](quickstart-install-connect-docker.md) mit den Voraussetzungen vertraut, und führen Sie die dort aufgeführten Befehle aus. Sie müssen Ihre Produktionsedition mit der Umgebungsvariable **MSSQL_PID** angeben. Im folgenden Beispiel wird die Ausführung des neuesten SQL Server 2017-Containerimages für die Enterprise Edition veranschaulicht:

::: zone pivot="cs1-bash"
```bash
docker run --name sqlenterprise \
-e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
-e 'MSSQL_PID=Enterprise' -p 1433:1433 \
-d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run --name sqlenterprise `
-e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-e "MSSQL_PID=Enterprise" -p 1433:1433 `
-d "mcr.microsoft.com/mssql/server:2019-latest"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run --name sqlenterprise `
-e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" ^
-e "MSSQL_PID=Enterprise" -p 1433:1433 ^
-d "mcr.microsoft.com/mssql/server:2019-latest"
```
::: zone-end

> [!IMPORTANT]
> Indem Sie den Wert **Y** an die Umgebungsvariable **ACCEPT_EULA** und einen Editionswert an **MSSQL_PID** übergeben, bestätigen Sie, dass Sie eine gültige und vorhandene Lizenz für die Edition und Version von SQL Server besitzen, die Sie verwenden möchten. Außerdem stimmen Sie den Nutzungsbedingungen für Ihre SQL Server-Lizenz zu, die für die Verwendung von SQL Server-Software gilt, die in einem Docker-Containerimage ausgeführt wird.

> [!NOTE]
> Eine vollständige Liste der möglichen Werte für **MSSQL_PID** finden Sie unter [Konfigurieren von SQL Server-Einstellungen mit Umgebungsvariablen unter Linux](sql-server-linux-configure-environment-variables.md).

## <a name="run-multiple-sql-server-containers"></a><a id="multiple"></a>Ausführen mehrerer SQL Server-Container

Docker ermöglicht die Ausführung mehrerer SQL Server-Container auf demselben Hostcomputer. Verwenden Sie diesen Ansatz, wenn mehrere SQL Server-Instanzen auf demselben Host benötigt werden. Jeder Container muss sich an einem anderen Port verfügbar machen.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Im folgenden Beispiel werden zwei SQL Server 2017-Container erstellt und den Ports **1401** und **1402** auf dem Hostcomputer zugeordnet.

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2017-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Im folgenden Beispiel werden zwei SQL Server 2019-Container erstellt und den Ports **1401** und **1402** auf dem Hostcomputer zugeordnet.

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1401:1433 -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1402:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

Nun gibt es zwei Instanzen von SQL Server, die in separaten Containern ausgeführt werden. Clients können eine Verbindung mit der jeweiligen SQL Server-Instanz herstellen, indem sie die IP-Adresse des Docker-Hosts und die Portnummer für den Container verwenden.

::: zone pivot="cs1-bash"
```bash
sqlcmd -S 10.3.2.4,1401 -U SA -P '<YourPassword>'
sqlcmd -S 10.3.2.4,1402 -U SA -P '<YourPassword>'
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
sqlcmd -S 10.3.2.4,1402 -U SA -P "<YourPassword>"
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sqlcmd -S 10.3.2.4,1401 -U SA -P "<YourPassword>"
sqlcmd -S 10.3.2.4,1402 -U SA -P "<YourPassword>"
```
::: zone-end

## <a name="upgrade-sql-server-in-containers"></a><a id="upgrade"></a> Aktualisieren von SQL Server in Containern

Wenn Sie das Containerimage mit Docker aktualisieren möchten, müssen Sie zunächst das Tag für das Release des Upgrades ermitteln. Rufen Sie diese Version mit dem Befehl `docker pull` aus der Registrierung ab:

```command
docker pull mcr.microsoft.com/mssql/server:<image_tag>
```

Dadurch wird das SQL Server-Image für neu erstellte Container aktualisiert. SQL Server wird jedoch nicht in ausgeführten Containern aktualisiert. Zu diesem Zweck müssen Sie einen neuen Container mit dem neuesten SQL Server-Containerimage erstellen und die Daten in diesen neuen Container migrieren.

1. Stellen Sie sicher, dass Sie eine der [Datenpersistenzmethoden](sql-server-linux-docker-container-configure.md#persist) für Ihren vorhandenen SQL Server-Container verwenden. So können Sie einen neuen Container mit den gleichen Daten starten.

1. Halten Sie den SQL Server-Container mit dem Befehl `docker stop` an.

1. Erstellen Sie mit `docker run` einen neuen SQL Server-Container, und geben Sie entweder ein zugeordnetes Hostverzeichnis oder einen Datenvolumecontainer an. Verwenden Sie das jeweilige Tag für Ihr SQL Server-Upgrade. Der neue Container verwendet nun eine neue Version von SQL Server mit Ihren vorhandenen SQL Server-Daten.

   > [!IMPORTANT]
   > Upgrades werden derzeit nur zwischen RC1, RC2 und der allgemein verfügbaren Version unterstützt.

1. Überprüfen Sie Ihre Datenbanken und Daten im neuen Container.

1. Entfernen Sie den alten Container optional mit `docker rm`.

## <a name="next-steps"></a>Nächste Schritte

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Der [Schnellstart](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true) erleichtert Ihnen den Einstieg in die Verwendung von SQL Server 2017-Containerimages in Docker.

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Der [Schnellstart](quickstart-install-connect-docker.md) erleichtert Ihnen den Einstieg in die Verwendung von SQL Server 2019-Containerimages in Docker.

::: moniker-end

- [Verweisen auf zusätzliche Konfiguration und Anpassung für Docker-Container](sql-server-linux-docker-container-configure.md)

- Ressourcen, Feedback und Informationen zu bekannten Problemen finden Sie im [GitHub-Repository „mssql-docker“](https://github.com/Microsoft/mssql-docker).

- [Problembehandlung von SQL Server Docker-Containern](sql-server-linux-docker-container-troubleshooting.md)

- [Hochverfügbarkeit für SQL Server-Container](sql-server-linux-container-ha-overview.md)

- [Sichern von SQL Server Docker-Containern](sql-server-linux-docker-container-security.md)
