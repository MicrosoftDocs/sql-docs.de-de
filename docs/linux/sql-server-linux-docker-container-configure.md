---
title: Konfigurieren und Anpassen von SQL Server Docker-Containern
description: Erfahren Sie mehr über die verschiedenen Möglichkeiten zum Anpassen von SQL Server Docker-Containern und wie Sie diese basierend auf Ihren Anforderungen konfigurieren können.
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.custom: contperf-fy21q1
ms.date: 09/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: 2fa5cc3b5fe8ecc5a2b3ed87444474f1de54b022
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273071"
---
# <a name="configure-and-customize-sql-server-docker-containers"></a>Konfigurieren und Anpassen von SQL Server Docker-Containern

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel wird erläutert, wie Sie SQL Server Docker-Container konfigurieren und anpassen können, z. B. zum persistenten Speichern Ihrer Daten, zum Verschieben von Dateien aus Containern und in Container sowie zum Festlegen von Standardeinstellungen.

## <a name="create-a-customized-container"></a><a id="customcontainer"></a> Erstellen eines benutzerdefinierten Containers

Sie können ein eigenes [Dockerfile](https://docs.docker.com/engine/reference/builder/#usage) erstellen, um einen benutzerdefinierten SQL Server-Container zu erstellen. Weitere Informationen finden Sie in [dieser Demo, in der SQL Server und eine Node-Anwendung kombiniert werden](https://github.com/twright-msft/mssql-node-docker-demo-app). Wenn Sie ein eigenes Dockerfile erstellen, sollten Sie auf den Vordergrundprozess achten, da dieser die Lebensdauer des Containers bestimmt. Wenn dieser beendet wird, wird der Container heruntergefahren. Wenn Sie beispielsweise ein Skript ausführen und SQL Server starten möchten, sollten Sie sicherstellen, dass der SQL Server-Prozess der Befehl ganz rechts ist. Alle anderen Befehle werden im Hintergrund ausgeführt. Dies wird mit dem folgenden Befehl in einem Dockerfile veranschaulicht:

```bash
/usr/src/app/do-my-sql-commands.sh & /opt/mssql/bin/sqlservr
```

Wenn Sie die Befehle im vorherigen Beispiel rückgängig gemacht haben, wird der Container heruntergefahren, wenn das Skript „do-my-sql-commands.sh“ abgeschlossen wird.

## <a name="persist-your-data"></a><a id="persist"></a> Beibehalten von Daten

Die SQL Server-Konfigurationsänderungen und -Datenbankdateien werden im Container beibehalten, auch wenn Sie den Container mit `docker stop` und `docker start` neu starten. Wenn Sie den Container jedoch mit `docker rm` entfernen, werden sämtliche Inhalte des Containers gelöscht, einschließlich SQL Server und Ihrer Datenbanken. Im folgenden Abschnitt wird erläutert, wie **Datenvolumes** verwendet werden, um Ihre Datenbankdateien beizubehalten, auch wenn die zugeordneten Container gelöscht werden.

> [!IMPORTANT]
> Für SQL Server ist es wichtig, sich mit der Datenpersistenz in Docker auseinanderzusetzen. Weitere Informationen finden Sie in der Docker-Dokumentation unter [Verwalten von Daten in Docker-Containern](https://docs.docker.com/engine/tutorials/dockervolumes/).

### <a name="mount-a-host-directory-as-data-volume"></a>Einbinden eines Hostverzeichnisses als Datenvolume

Die erste Option besteht darin, ein Verzeichnis auf dem Host als Datenvolume in Ihrem Container einzubinden. Verwenden Sie hierzu den Befehl `docker run` mit dem Flag `-v <host directory>:/var/opt/mssql`. Dadurch können die Daten zwischen den Containerausführungen wiederhergestellt werden.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v <host directory>/data:/var/opt/mssql/data -v <host directory>/log:/var/opt/mssql/log -v <host directory>/secrets:/var/opt/mssql/secrets -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

Mit dieser Methode können Sie die Dateien auch außerhalb von Docker auf dem Host freigeben und anzeigen.

> [!IMPORTANT]
> Die Hostvolumezuordnung für **Docker unter Windows** unterstützt derzeit nicht die Zuordnung des kompletten `/var/opt/mssql`-Verzeichnisses. Sie können dem Hostcomputer jedoch ein Unterverzeichnis wie `/var/opt/mssql/data` zuordnen.
>
> Die Hostvolumezuordnung für **Docker unter Mac** mit dem SQL Server für Linux-Image wird derzeit nicht unterstützt. Verwenden Sie stattdessen Datenvolumecontainer. Diese Einschränkung gilt nur für das Verzeichnis `/var/opt/mssql`. Das Lesen aus einem bereitgestellten Verzeichnis funktioniert ordnungsgemäß. Sie können beispielsweise ein Hostverzeichnis mithilfe von-v auf einem Mac einbinden und eine Sicherung aus einer BAK-Datei wiederherstellen, die sich auf dem Host befindet.

### <a name="use-data-volume-containers"></a>Verwenden von Datenvolumecontainern

Die zweite Option besteht in der Verwendung eines Datenvolumecontainers. Sie können einen Datenvolumecontainer erstellen, indem Sie mit dem Parameter `-v` einen Volumenamen anstelle eines Hostverzeichnisses angeben. Im folgenden Beispiel wird ein freigegebenes Datenvolume mit dem Namen **sqlvolume** erstellt.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" -p 1433:1433 -v sqlvolume:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

> [!NOTE]
> Dieses Verfahren zum impliziten Erstellen eines Datenvolumes im run-Befehl funktioniert nicht mit älteren Docker-Versionen. Wenden Sie in diesem Fall die expliziten Schritte an, die in der Docker-Dokumentation zum [Erstellen und Einbinden eines Datenvolumecontainers](https://docs.docker.com/engine/tutorials/dockervolumes/#creating-and-mounting-a-data-volume-container) beschrieben werden.

Auch wenn Sie diesen Container anhalten und entfernen, bleibt das Datenvolume erhalten. Sie können es mit dem Befehl `docker volume ls` anzeigen.

```command
docker volume ls
```

Wenn Sie dann einen weiteren Container mit dem gleichen Volumenamen erstellen, verwendet der neue Container die gleichen SQL Server-Daten, die auch im Volume enthalten sind.

Verwenden Sie den Befehl `docker volume rm`, um einen Datenvolumecontainer zu entfernen.

> [!WARNING]
> Wenn Sie den Datenvolumecontainer löschen, werden alle SQL Server-Daten im Container *dauerhaft* gelöscht.

### <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Zusätzlich zu diesen Containerverfahren können Sie auch SQL Server-Standardverfahren für die Sicherung und Wiederherstellung anwenden. Sie können Sicherungsdateien verwenden, um Ihre Daten zu schützen oder die Daten auf eine andere SQL Server-Instanz zu verschieben. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von SQL Server-Datenbanken unter Linux](sql-server-linux-backup-and-restore-database.md).

> [!WARNING]
> Wenn Sie Sicherungen erstellen, stellen Sie sicher, dass die Sicherungsdateien außerhalb des Containers erstellt oder kopiert werden. Andernfalls werden die Sicherungsdateien gelöscht, wenn der Container entfernt wird.

## <a name="copy-files-from-a-container"></a>Kopieren von Dateien aus einem Container

Verwenden Sie den folgenden Befehl, um eine Datei aus dem Container zu kopieren:

```command
docker cp <Container ID>:<Container path> <host path>
```

Die Container-ID können Sie durch Ausführen des Befehls `docker ps -a` abrufen.

**Beispiel:**

::: zone pivot="cs1-bash"
```bash
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog /tmp/errorlog
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog C:\Temp\errorlog
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker cp d6b75213ef80:/var/opt/mssql/log/errorlog C:\Temp\errorlog
```
::: zone-end

## <a name="copy-files-into-a-container"></a>Kopieren von Dateien in einen Container

Verwenden Sie den folgenden Befehl, um eine Datei in den Container zu kopieren:

```command
docker cp <Host path> <Container ID>:<Container path>
```

**Beispiel:**

::: zone pivot="cs1-bash"
```bash
docker cp /tmp/mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker cp C:\Temp\mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker cp C:\Temp\mydb.mdf d6b75213ef80:/var/opt/mssql/data
```
::: zone-end

## <a name="configure-the-timezone"></a><a id="tz"></a> Konfigurieren der Zeitzone

Konfigurieren Sie die Umgebungsvariable `TZ`, um SQL Server mit einer bestimmten Zeitzone in einem Linux-Container auszuführen. Führen Sie den Befehl `tzselect` über eine Linux-Bash-Eingabeaufforderung aus, um den richtigen Zeitzonenwert zu ermitteln:

```command
tzselect
```

Nachdem Sie eine Zeitzone ausgewählt haben, zeigt `tzselect` in etwa folgende Ausgabe an:

```output
The following information has been given:

        United States
        Pacific

Therefore TZ='America/Los_Angeles' will be used.
```

Sie können diese Informationen verwenden, um die gleiche Umgebungsvariable in Ihrem Linux-Container festzulegen. Im folgenden Beispiel wird veranschaulicht, wie SQL Server in einem Container in der Zeitzone `Americas/Los_Angeles` ausgeführt wird:

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
sudo docker run -e 'ACCEPT_EULA=Y' -e 'A_PASSWORD=<YourStrong!Passw0rd>' \
-p 1433:1433 --name sql1 \
-e 'TZ=America/Los_Angeles'\
-d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sudo docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-p 1433:1433 --name sql1 `
-e "TZ=America/Los_Angeles" `
-d mcr.microsoft.com/mssql/server:2017-latest 
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sudo docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
-p 1433:1433 --name sql1 ^
-e "TZ=America/Los_Angeles" ^
-d mcr.microsoft.com/mssql/server:2017-latest 
```
::: zone-end

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
   -p 1433:1433 --name sql1 \
   -e 'TZ=America/Los_Angeles'\
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
sudo docker run -e 'ACCEPT_EULA=Y' -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
   -p 1433:1433 --name sql1 `
   -e "TZ=America/Los_Angeles" `
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
sudo docker run -e 'ACCEPT_EULA=Y' -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
   -p 1433:1433 --name sql1 `
   -e "TZ=America/Los_Angeles" `
   -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="change-the-default-file-location"></a><a id="changefilelocation"></a> Ändern des Standardspeicherorts der Datei

Fügen Sie die Variable `MSSQL_DATA_DIR` hinzu, um Ihr Datenverzeichnis in Ihrem `docker run`-Befehl zu ändern, und stellen Sie dann ein Volume an dem Speicherort bereit, auf den der Benutzer Ihres Containers Zugriff hat.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=MyStrongPassword' -e 'MSSQL_DATA_DIR=/my/file/path' -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest
```
::: zone-end

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

::: zone pivot="cs1-bash"
```bash
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=MyStrongPassword' -e 'MSSQL_DATA_DIR=/my/file/path' -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker run -e 'ACCEPT_EULA=Y' -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MyStrongPassword" -e "MSSQL_DATA_DIR=/my/file/path" -v /my/host/path:/my/file/path -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```
::: zone-end

::: moniker-end

## <a name="next-steps"></a>Nächste Schritte

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

- Der [Schnellstart](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true) erleichtert Ihnen den Einstieg in die Verwendung von SQL Server 2017-Containerimages in Docker.

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

- Der [Schnellstart](quickstart-install-connect-docker.md) erleichtert Ihnen den Einstieg in die Verwendung von SQL Server 2019-Containerimages in Docker.

::: moniker-end

- [Bereitstellen von und Herstellen einer Verbindung mit SQL Server Docker-Containern](sql-server-linux-docker-container-deployment.md)

- [Problembehandlung von SQL Server Docker-Containern](sql-server-linux-docker-container-troubleshooting.md)

- [Sichern von SQL Server Docker-Containern](sql-server-linux-docker-container-security.md)
