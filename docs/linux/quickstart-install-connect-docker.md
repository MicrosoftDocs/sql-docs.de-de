---
title: 'Docker: Installieren von Containern für SQL Server für Linux'
description: In diesem Schnellstart wird erläutert, wie Sie die Containerimages von SQL Server 2017 und 2019 mit Docker ausführen können. Anschließend erstellen Sie mit dem Hilfsprogramm „sqlcmd“ eine Datenbank und fragen diese ab.
ms.custom: seo-lt-2019, contperf-fy21q1
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.date: 09/07/2020
ms.topic: quickstart
ms.prod: sql
ms.technology: linux
ms.prod_service: linux
ms.assetid: 82737f18-f5d6-4dce-a255-688889fdde69
monikerRange: '>= sql-server-linux-2017 || >= sql-server-2017'
zone_pivot_groups: cs1-command-shell
ms.openlocfilehash: 9393064403fbd41255b6be0712813185e745d6c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344442"
---
# <a name="quickstart-run-sql-server-container-images-with-docker"></a>Schnellstart: Ausführen von SQL Server-Containerimages mit Docker
[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

> [!NOTE]
> Die unten gezeigten Beispiele verwenden „docker.exe“, die meisten dieser Befehle funktionieren aber auch in Verbindung mit Podman. Es bietet eine ähnliche Befehlszeilenschnittstelle wie die Docker-Container-Engine. Weitere Informationen zu Podman finden Sie [hier](http://docs.podman.io/en/latest):

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

In diesem Schnellstart wird Docker verwendet, um das SQL Server 2017-Containerimage [mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server) mithilfe von Pull zu übertragen und auszuführen. Stellen Sie anschließend eine Verbindung mit **sqlcmd** her, um Ihre erste Datenbank zu erstellen und Abfragen auszuführen.

> [!TIP]
> Wenn Sie SQL Server 2019-Container erstellen möchten, erhalten Sie in der [SQL Server 2019-Version dieses Artikels](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true) weitere Informationen.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

> [!NOTE]
> Ab dem kumulativen Update 3 von SQL Server 2019 (CU3) wird Ubuntu 18.04 unterstützt.

In diesem Schnellstart wird Docker verwendet, um das SQL Server 2019-Containerimage [mssql-server](https://hub.docker.com/r/microsoft/mssql-server) abzurufen und auszuführen. Stellen Sie anschließend eine Verbindung mit **sqlcmd** her, um Ihre erste Datenbank zu erstellen und Abfragen auszuführen.

> [!TIP]
> In dieser Schnellstartanleitung werden SQL Server 2019-Container erstellt. Wenn Sie lieber SQL Server 2017-Container erstellen möchten, erhalten Sie weitere Informationen in der [SQL Server 2017-Version dieses Artikels](quickstart-install-connect-docker.md?view=sql-server-linux-2017&preserve-view=true).

::: moniker-end

Dieses Image enthält SQL Server für Linux (basierend auf Ubuntu 18.04). Es kann unter Linux mit der Docker-Engine 1.8 und höher und in Docker für Mac bzw. Windows verwendet werden. In diesem Schnellstart liegt der Schwerpunkt auf der Verwendung von SQL Server für das **Linux**-Image. Das Windows-Image wird hier zwar nicht behandelt, aber auf der [Docker-Hubseite zu „mssql-server-windows-developer“](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/) erhalten Sie weitere Informationen.

## <a name="prerequisites"></a><a id="requirements"></a> Voraussetzungen

- Docker-Engine 1.8 und höher auf unterstütztem Linux-Betriebssystem oder Docker für Mac bzw. Windows. Weitere Informationen finden Sie unter [Install Docker (Installieren von Docker)](https://docs.docker.com/engine/installation/).
- Docker-Speichertreiber **overlay2**. Dies ist die Standardeinstellung für die meisten Benutzer. Wenn Sie feststellen, dass Sie diesen Speicheranbieter nicht verwenden und ihn ändern müssen, finden Sie weitere Informationen und Warnungen in der [Docker-Dokumentation zur Konfiguration von „overlay2“](https://docs.docker.com/storage/storagedriver/overlayfs-driver/#configure-docker-with-the-overlay-or-overlay2-storage-driver).
- Mindestens 2GB freier Speicherplatz auf dem Datenträger.
- Mindestens 2GB RAM.
- [Systemanforderungen von SQL Server unter Linux](sql-server-linux-setup.md#system)

<!--The following H2 is versioned for 2017 and 2019. Much of the content is duplicated, so
any changes to one section should be duplicated in the other-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="pull-and-run-the-2017-container-image"></a><a id="pullandrun2017"></a> Pullen und Ausführen des 2017-Containerimages

Bevor Sie die folgenden Schritte ausführen, stellen Sie sicher, dass Sie am Anfang dieses Artikels Ihre bevorzugte Shell (bash, PowerShell oder cmd) ausgewählt haben.

1. Pullen Sie das Linux-Containerimage von SQL Server 2017 aus Microsoft Container Registry.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   > [!TIP]
   > Wenn Sie SQL Server 2019-Container erstellen möchten, erhalten Sie in der [SQL Server 2019-Version dieses Artikels](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true#pullandrun2019) weitere Informationen.

   Mit dem obenstehenden Befehl wird das neueste Containerimage von SQL Server 2017 mithilfe von Pull übertragen. Wenn Sie ein bestimmtes Image übertragen möchten, fügen Sie einen Doppelpunkt und den Tagnamen hinzu (z.B. `mcr.microsoft.com/mssql/server:2017-GA-ubuntu`). Alle verfügbaren Images finden Sie auf der [Docker-Hubseite zu „mssql-server“](https://hub.docker.com/r/microsoft/mssql-server).

   ::: zone pivot="cs1-bash"
   Für die bash-Befehle in diesem Artikel wird `sudo` verwendet. Unter MacOS ist `sudo` möglicherweise nicht erforderlich. Wenn Sie `sudo` unter Linux nicht zum Ausführen von Docker verwenden möchten, können Sie eine **docker**-Gruppe konfigurieren und dieser Gruppe Benutzer hinzufügen. Weitere Informationen finden Sie unter [Post-installation steps for Linux (Schritte nach der Installation unter Linux)](https://docs.docker.com/install/linux/linux-postinstall/).

   ::: zone-end

2. Zum Ausführen des Containerimages in Docker können Sie eine Bash-Shell (Linux bzw. macOS) oder eine PowerShell-Eingabeaufforderung mit erhöhten Rechten verwenden. Nachfolgend finden Sie den dafür benötigten Befehl:

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" \
      -p 1433:1433 --name sql1 -h sql1 \
      -d \
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   
   > [!NOTE]
   > Wenn Sie PowerShell Core verwenden, ersetzen Sie die doppelten Anführungszeichen durch einfache Anführungszeichen.
   
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d `
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d `
      mcr.microsoft.com/mssql/server:2017-latest
   ```
   ::: zone-end

   > [!NOTE]
   > Das Kennwort sollte der Standardrichtlinie für SQL Server-Kennwörter entsprechen. Ist dies nicht der Fall, kann der Container SQL Server nicht einrichten und funktioniert nicht mehr. In der Standardeinstellung muss das Kennwort mindestens 8 Zeichen lang sein und Zeichen aus drei der folgenden vier Gruppen enthalten: Großbuchstaben, Kleinbuchstaben, Ziffern und Symbole. Mit dem Befehl [docker logs](https://docs.docker.com/engine/reference/commandline/logs/) können Sie das Fehlerprotokoll untersuchen.
   >
   > Standardmäßig wird dabei ein Container erstellt, der die Developer Edition von SQL Server 2017 enthält. Die Vorgehensweise zum Ausführen von Produktionseditionen in Containern weicht hiervon minimal ab. Weitere Informationen finden Sie unter [Run production container images (Ausführen von Containerimages für Produktionsumgebungen)](./sql-server-linux-docker-container-deployment.md#production).

   In der folgenden Tabelle finden Sie Beschreibungen der Parameter des vorangegangenen Beispiels für `docker run`:

   | Parameter | BESCHREIBUNG |
   |-----|-----|
   | **-e „ACCEPT_EULA = Y“** |  Legen Sie für die Variable **ACCEPT_EULA** einen beliebigen Wert fest, um Ihre Zustimmung zum [End-User Licensing Agreement (Benutzerlizenzvertrag)](https://go.microsoft.com/fwlink/?linkid=857698) zu geben. Diese Einstellung ist für das SQL Server-Image zwingend erforderlich. |
   | **-e "SA_PASSWORD=\<YourStrong@Passw0rd\>"** | Geben Sie ein starkes Kennwort ein, das aus mindestens acht Zeichen besteht und den [Kennwortanforderungen von SQL Server](../relational-databases/security/password-policy.md) entspricht. Diese Einstellung ist für das SQL Server-Image zwingend erforderlich. |
   | **-p 1433:1433** | Ordnen Sie einen TCP-Port in der Hostumgebung (erster Wert) einem TCP-Port im Container zu (zweiter Wert). In diesem Beispiel lauscht SQL Server an TCP 1433 im Container. Diese Informationen werden für Port 1433 auf dem Host offen gelegt. |
   | **--name sql1** | Geben Sie dem Container selbst einen Namen, anstatt einen willkürlich generierten zu verwenden. Wenn Sie mehrere Container ausführen, können Sie nicht denselben Namen mehrfach verwenden. |
   | **-h sql1** | Wird verwendet, um den Container Hostnamen explizit festzulegen. Wenn Sie ihn nicht angeben, wird standardmäßig die Container-ID verwendet, bei der es sich um eine zufällig generierte System-GUID handelt. |
   | **-d** | Hiermit wird der Container im Hintergrund ausgeführt (Daemon) |
   | **mcr.microsoft.com/mssql/server:2017-latest** | Das Linux-Containerimage von SQL Server 2017 |

3. Verwenden Sie den Befehl `docker ps`, um Ihre Docker-Container anzeigen zu lassen.


   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker ps -a
   ```
   ::: zone-end

   Die Ausgabe sollte dann ähnlich wie in diesem Screenshot sein:

   ![Ausgabe des Befehls „docker ps“](./media/sql-server-linux-setup-docker/docker-ps-command.png)

4. Wenn in der Spalte **STATUS** **Up** (Aktiv) eingetragen ist, wird SQL Server im Container ausgeführt und überwacht den Port, der in der Spalte **PORTS** angegeben ist. Wenn in der Spalte **STATUS** Ihres SQL Server-Containers **Exited** (Beendet) steht, lesen Sie bitte im [Abschnitt „Problembehebung“ im Konfigurationshandbuch](./sql-server-linux-docker-container-troubleshooting.md) nach.

Der `-h`-Parameter (Hostname) ändert den internen Namen des Containers in einen benutzerdefinierten Wert, wie oben erläutert. Dieser Name wird in der folgenden Transact-SQL-Abfrage zurückgegeben:

```sql
SELECT @@SERVERNAME,
    SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),
    SERVERPROPERTY('MachineName'),
    SERVERPROPERTY('ServerName')
```

Wenn Sie für `-h` und `--name` denselben Wert festlegen, kann der Zielcontainer ganz einfach ermittelt werden.

5. Ändern Sie als letzten Schritt das Systemadministratorkennwort, da das `SA_PASSWORD` in der `ps -eax`-Ausgabe sichtbar und in der Umgebungsvariablen mit dem gleichen Namen gespeichert ist. Weitere Informationen finden Sie nachfolgend.

::: moniker-end
<!--End of 2017 "Pull and run" section-->

<!--This is the 2019 version of the "Pull and run" section-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="pull-and-run-the-2019-container-image"></a><a id="pullandrun2019"></a> Pullen und Ausführen des 2019-Containerimages

Bevor Sie die folgenden Schritte ausführen, stellen Sie sicher, dass Sie am Anfang dieses Artikels Ihre bevorzugte Shell (bash, PowerShell oder cmd) ausgewählt haben.

1. Pullen Sie das Linux-Containerimage von SQL Server 2019 aus Microsoft Container Registry.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   
   > [!NOTE]
   > Wenn Sie PowerShell Core verwenden, ersetzen Sie die doppelten Anführungszeichen durch einfache Anführungszeichen.
   
   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker pull mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   > [!TIP]
   > In diesem Schnellstart wird das Docker-Image von SQL Server 2019 verwendet. Wenn Sie das Image von SQL Server 2017 ausführen möchten, lesen Sie [die Version dieses Artikels zu SQL Server 2017](quickstart-install-connect-docker.md?view=sql-server-linux-2017&preserve-view=true#pullandrun2017).

   Mit dem vorherigen Befehl rufen Sie das auf Ubuntu basierende Containerimage von SQL Server 2019 ab. Informationen dazu, wie Sie stattdessen Containerimages verwenden, die auf Red Hat basieren, finden Sie unter [Run RHEL-based container images (Ausführen von RHEL-basierten Containerimages)](./sql-server-linux-docker-container-deployment.md#rhel). Informationen zu allen verfügbaren Images finden Sie auf der [Docker-Hubseite zu „mssql-server-linux“](https://hub.docker.com/_/microsoft-mssql-server).

   ::: zone pivot="cs1-bash"
   Für die bash-Befehle in diesem Artikel wird `sudo` verwendet. Unter MacOS ist `sudo` möglicherweise nicht erforderlich. Wenn Sie `sudo` unter Linux nicht zum Ausführen von Docker verwenden möchten, können Sie eine **docker**-Gruppe konfigurieren und dieser Gruppe Benutzer hinzufügen. Weitere Informationen finden Sie unter [Post-installation steps for Linux (Schritte nach der Installation unter Linux)](https://docs.docker.com/install/linux/linux-postinstall/).
   ::: zone-end

2. Zum Ausführen des Containerimages in Docker können Sie eine Bash-Shell (Linux bzw. macOS) oder eine PowerShell-Eingabeaufforderung mit erhöhten Rechten verwenden. Nachfolgend finden Sie den dafür benötigten Befehl:

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" \
      -p 1433:1433 --name sql1 -h sql1 \
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong@Passw0rd>" `
      -p 1433:1433 --name sql1 -h sql1 `
      -d mcr.microsoft.com/mssql/server:2019-latest
   ```
   ::: zone-end

   > [!NOTE]
   > Das Kennwort sollte der Standardrichtlinie für SQL Server-Kennwörter entsprechen. Ist dies nicht der Fall, kann der Container SQL Server nicht einrichten und funktioniert nicht mehr. In der Standardeinstellung muss das Kennwort mindestens 8 Zeichen lang sein und Zeichen aus drei der folgenden vier Gruppen enthalten: Großbuchstaben, Kleinbuchstaben, Ziffern und Symbole. Mit dem Befehl [docker logs](https://docs.docker.com/engine/reference/commandline/logs/) können Sie das Fehlerprotokoll untersuchen.
   >
   > Standardmäßig wird dabei ein Container erstellt, der die Developer Edition von SQL Server 2019 enthält.

   In der folgenden Tabelle finden Sie Beschreibungen der Parameter des vorangegangenen Beispiels für `docker run`:

   | Parameter | BESCHREIBUNG |
   |-----|-----|
   | **-e „ACCEPT_EULA = Y“** |  Legen Sie für die Variable **ACCEPT_EULA** einen beliebigen Wert fest, um Ihre Zustimmung zum [End-User Licensing Agreement (Benutzerlizenzvertrag)](https://go.microsoft.com/fwlink/?LinkId=746388) zu geben. Diese Einstellung ist für das SQL Server-Image zwingend erforderlich. |
   | **-e "SA_PASSWORD=\<YourStrong@Passw0rd\>"** | Geben Sie ein starkes Kennwort ein, das aus mindestens acht Zeichen besteht und den [Kennwortanforderungen von SQL Server](../relational-databases/security/password-policy.md) entspricht. Diese Einstellung ist für das SQL Server-Image zwingend erforderlich. |
   | **-p 1433:1433** | Ordnen Sie einen TCP-Port in der Hostumgebung (erster Wert) einem TCP-Port im Container zu (zweiter Wert). In diesem Beispiel lauscht SQL Server an TCP 1433 im Container. Diese Informationen werden für Port 1433 auf dem Host offen gelegt. |
   | **--name sql1** | Geben Sie dem Container selbst einen Namen, anstatt einen willkürlich generierten zu verwenden. Wenn Sie mehrere Container ausführen, können Sie nicht denselben Namen mehrfach verwenden. |
   | **-h sql1** | Wird verwendet, um den Container Hostnamen explizit festzulegen. Wenn Sie ihn nicht angeben, wird standardmäßig die Container-ID verwendet, bei der es sich um eine zufällig generierte System-GUID handelt. |
   | **mcr.microsoft.com/mssql/server:2019-latest** | Das Ubuntu Linux-Containerimage von SQL Server 2019 |

3. Verwenden Sie den Befehl `docker ps`, um Ihre Docker-Container anzeigen zu lassen.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker ps -a
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker ps -a
   ```
   ::: zone-end

   Die Ausgabe sollte dann ähnlich wie in diesem Screenshot sein:

   ![Ausgabe des Befehls „docker ps“](./media/sql-server-linux-setup-docker/docker-ps-command.png)

4. Wenn in der Spalte **STATUS** **Up** (Aktiv) eingetragen ist, wird SQL Server im Container ausgeführt und überwacht den Port, der in der Spalte **PORTS** angegeben ist. Wenn in der Spalte **STATUS** Ihres SQL Server-Containers **Exited** (Beendet) angezeigt wird, lesen Sie [Problembehandlung von SQL Server Docker-Containern](sql-server-linux-docker-container-troubleshooting.md).

Der `-h`-Parameter (Hostname) ändert den internen Namen des Containers in einen benutzerdefinierten Wert, wie oben erläutert. Mit ihm lässt sich der interne Name eines Containers in einen benutzerdefinierten Wert ändern. Dieser Name wird in der folgenden Transact-SQL-Abfrage zurückgegeben:

```sql
SELECT @@SERVERNAME,
    SERVERPROPERTY('ComputerNamePhysicalNetBIOS'),
    SERVERPROPERTY('MachineName'),
    SERVERPROPERTY('ServerName')
```

Wenn Sie für `-h` und `--name` denselben Wert festlegen, kann der Zielcontainer ganz einfach ermittelt werden.


5. Ändern Sie als letzten Schritt das Systemadministratorkennwort, da das `SA_PASSWORD` in der `ps -eax`-Ausgabe sichtbar und in der Umgebungsvariablen mit dem gleichen Namen gespeichert ist. Weitere Informationen finden Sie nachfolgend.


::: moniker-end
<!--End of 2019 "Pull and run" section-->



## <a name="change-the-sa-password"></a><a id="sapassword"></a> Ändern des Systemadministratorkennworts

<!-- This section was pasted in from includes/sql-server-linux-change-docker-password.md, to better support zone pivots. 2019/02/11 -->

Das **SA**-Konto ist ein Systemadministrator auf der SQL Server-Instanz, der beim Setup erstellt wird. Nach dem Erstellen Ihres SQL Server-Containers wird die von Ihnen festgelegte `SA_PASSWORD` Umgebungsvariable sichtbar, wenn Sie sie in dem Container ausführen`echo $SA_PASSWORD`. Ändern Sie aus Sicherheitsgründen ihr SA-Kennwort.

1. Wählen Sie ein sicheres Kennwort für den SA-Benutzer aus.

1. Verwenden Sie `docker exec` in Transact-SQL **sqlcmd** zum Ausführen und Ändern des Kennworts. Ersetzen Sie im folgenden Beispiel das alte Kennwort, `<YourStrong!Passw0rd>`, und das neue Kennwort, `<YourNewStrong!Passw0rd>`, durch Ihre eigenen Kennwortwerte.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P "<YourStrong@Passw0rd>" \
      -Q 'ALTER LOGIN SA WITH PASSWORD="<YourNewStrong@Passw0rd>"'
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourStrong@Passw0rd>" `
      -Q "ALTER LOGIN SA WITH PASSWORD='<YourNewStrong@Passw0rd>'"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourStrong!Passw0rd>" `
      -Q "ALTER LOGIN SA WITH PASSWORD='<YourNewStrong@Passw0rd>'"
   ```
   ::: zone-end

## <a name="connect-to-sql-server"></a>Verbindung mit SQL Server herstellen

In den folgenden Schritten wird im Container das SQL Server-Befehlszeilentool [**sqlcmd**](../tools/sqlcmd-utility.md) genutzt, um eine Verbindung mit SQL Server herzustellen.

1. Verwenden Sie den Befehl `docker exec -it`, um in Ihrem laufenden Container eine interaktive Bash-Shell zu starten. Im folgenden Beispiel steht `sql1` für den Namen, den Sie bei der Erstellung des Containers mit dem Parameter `--name` angegeben haben.

   ::: zone pivot="cs1-bash"
   ```bash
   sudo docker exec -it sql1 "bash"
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   docker exec -it sql1 "bash"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   docker exec -it sql1 "bash"
   ```
   ::: zone-end

2. Stellen Sie eine lokale Verbindung mit „sqlcmd“ her. „Sqlcmd“ verwendet nicht automatisch den richtigen Pfad. Sie müssen daher selbst den vollständigen Pfand angeben.

   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "<YourNewStrong@Passw0rd>"
   ```

   > [!TIP]
   > Sie können das Kennwort in der Befehlszeile auslassen, damit Sie aufgefordert werden, es einzugeben.

3. Wenn dies erfolgreich war, sollten zu einer **sqlcmd** Eingabeaufforderung: `1>` gelangen.

## <a name="create-and-query-data"></a>Erstellen und Abfragen von Daten

Die folgenden Abschnitte führen Sie durch die Verwendung von **sqlcmd** und Transact-SQL, um eine neue Datenbank zu erstellen, Daten hinzuzufügen und eine Abfrage auszuführen.

### <a name="create-a-new-database"></a>Erstellen einer neuen Datenbank

Mit den folgenden Schritten wird eine neue Datenbank mit dem Namen `TestDB` erstellt.

1. Fügen Sie aus der **sqlcmd**-Eingabeaufforderung den folgenden Transact-SQL-Befehl zur Erstellung einer Testdatenbank ein:

   ```sql
   CREATE DATABASE TestDB
   ```

2. Schreiben Sie in der nächsten Zeile eine Abfrage, um den Namen all Ihrer Datenbanken auf Ihrem Server zurückzugeben:

   ```sql
   SELECT Name from sys.Databases
   ```

3. Die vorherigen beiden Befehle wurden nicht sofort ausgeführt. Geben Sie `GO` in einer neuen Zeile ein, um die vorherigen Befehle auszuführen:

   ```sql
   GO
   ```

### <a name="insert-data"></a>Einfügen von Daten

Erstellen Sie als Nächstes eine neue Tabelle, `Inventory`, und fügen Sie zwei neue Zeilen ein.

1. Wechseln Sie den Kontext aus der **sqlcmd**-Eingabeaufforderung zur neuen `TestDB`-Datenbank:

   ```sql
   USE TestDB
   ```

2. Erstellen Sie eine neue Tabelle mit dem Namen `Inventory`:

   ```sql
   CREATE TABLE Inventory (id INT, name NVARCHAR(50), quantity INT)
   ```

3. Fügen Sie Daten in die neue Tabelle ein:

   ```sql
   INSERT INTO Inventory VALUES (1, 'banana', 150); INSERT INTO Inventory VALUES (2, 'orange', 154);
   ```

4. Geben Sie `GO` ein, um die zuvor eingegebenen Befehle auszuführen:

   ```sql
   GO
   ```

### <a name="select-data"></a>Auswählen von Daten

Führen Sie nun eine Abfrage zum Zurückgeben von Daten aus der `Inventory`-Tabelle aus.

1. Geben Sie aus der **sqlcmd**-Eingabeaufforderung eine Abfrage ein, die Reihen aus der `Inventory`-Tabelle zurückgibt, bei denen die Menge größer als 152 ist:

   ```sql
   SELECT * FROM Inventory WHERE quantity > 152;
   ```

2. Führen Sie den folgenden Befehl aus:

   ```sql
   GO
   ```

### <a name="exit-the-sqlcmd-command-prompt"></a>Beenden der sqlcmd-Eingabeaufforderung

1. Zum Beenden der **sqlcmd**-Sitzung, geben Sie `QUIT` ein:

   ```sql
   QUIT
   ```

2. Geben Sie `exit` ein, um die interaktive Befehlszeile in Ihrem Container zu beenden. Der Container wird auch nach dem Beenden der interaktiven Bash-Shell weiter ausgeführt.

## <a name="connect-from-outside-the-container"></a><a id="connectexternal"></a> Herstellen einer Verbindung von außerhalb des Containers

Sie können auch über jedes externe Linux-, Windows- oder macOS-Tool, das SQL-Verbindungen unterstützt, eine Verbindung mit der SQL Server-Instanz auf Ihrem Docker-Computer herstellen.

Mithilfe der folgenden Schritte stellen Sie über **sqlcmd** von außerhalb Ihres Containers eine Verbindung mit SQL Server im Container her. Voraussetzung ist, dass Sie außerhalb Ihres Containers bereits SQL Server-Befehlszeilentools installiert haben. Für alle Tools gelten dieselben Prinzipien. Die Verbindung wird jedoch bei jedem Tool auf eine andere Weise hergestellt.

1. Ermitteln Sie die IP-Adresse des Computers, der Ihren Container hostet. Verwenden Sie dazu unter Linux **Ifconfig** oder **ip addr**. Verwenden Sie unter Windows **ipconfig**.

1. Installieren Sie für dieses Beispiel das Tool **sqlcmd** auf Ihrem Clientcomputer. Weitere Informationen finden Sie unter [sqlcmd Utility (Hilfsprogramm „sqlcmd“)](../tools/sqlcmd-utility.md) oder [Install sqlcmd and bcp the SQL Server command-line tools on Linux (Installieren der SQL Server-Befehlszeilentools sqlcmd und bcp unter Linux)](sql-server-linux-setup-tools.md).

1. Führen Sie „sqlcmd“ aus. Geben Sie dabei die IP-Adresse und den Port an, der dem Port 1433 Ihres Containers zugeordnet ist. In diesem Beispiel ist das derselbe Port, 1433, auf dem Hostcomputer. Wenn Sie einen anderen zugeordneten Port auf dem Hostcomputer angegeben haben, verwenden Sie diesen hier. Außerdem müssen Sie den entsprechenden eingehenden Port in der Firewall öffnen, um die Verbindung zuzulassen.

   ::: zone pivot="cs1-bash"
   ```bash
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

   ::: zone pivot="cs1-powershell"
   ```PowerShell
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

   ::: zone pivot="cs1-cmd"
   ```cmd
   sqlcmd -S <ip_address>,1433 -U SA -P "<YourNewStrong@Passw0rd>"
   ```
   ::: zone-end

1. Führen Sie Transact-SQL-Befehle aus. Wenn Sie fertig sind, geben Sie `QUIT` ein.

Für eine Verbindung mit SQL Server werden häufig auch folgende Tools verwendet:

- [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)
- [SQL Server Management Studio (SSMS) unter Windows](sql-server-linux-manage-ssms.md)
- [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md)
- [mssql-cli (Vorschauversion)](https://github.com/dbcli/mssql-cli/blob/master/doc/usage_guide.md)
- [PowerShell Core](sql-server-linux-manage-powershell-core.md)

## <a name="remove-your-container"></a>Entfernen Ihres Containers

Wenn Sie den in diesem Tutorial verwendeten SQL Server-Container entfernen möchten, führen Sie die folgenden Befehle aus:

::: zone pivot="cs1-bash"
```bash
sudo docker stop sql1
sudo docker rm sql1
```
::: zone-end

::: zone pivot="cs1-powershell"
```PowerShell
docker stop sql1
docker rm sql1
```
::: zone-end

::: zone pivot="cs1-cmd"
```cmd
docker stop sql1
docker rm sql1
```
::: zone-end

> [!WARNING]
> Wenn Sie einen Container beenden oder entfernen, werden die SQL Server-Daten dauerhaft aus diesem Container gelöscht. Wenn Sie Ihre Daten weiterhin benötigen, [erstellen Sie eine Sicherungskopie des Containers](tutorial-restore-backup-in-sql-server-container.md), oder nutzen Sie für die Containerdaten eine [Methode zur Datenpersistenz](sql-server-linux-docker-container-configure.md#persist).

## <a name="docker-demo"></a>Docker-Demo

Nachdem Sie nun das SQL Server-Containerimage für Docker getestet haben, sollten Sie auch wissen, welche Vorteile Ihnen Docker beim Entwickeln und Testen bringt. Im folgenden Video sehen Sie, wie sich Docker in einem Continuous Integration-Szenario bzw. einem Continuous Deployment-Szenario verwenden lässt.

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T152/player]

## <a name="next-steps"></a>Nächste Schritte

Ein Tutorial zum Wiederherstellen von Sicherungskopien von Datenbanken in einem Container finden Sie unter [Restore a SQL Server database in a Linux Docker container (Wiederherstellen einer SQL Server-Datenbank in einem Docker-Container unter Linux)](tutorial-restore-backup-in-sql-server-container.md). Untersuchen Sie andere Szenarien, z. B. das Ausführen [mehrerer Container](sql-server-linux-docker-container-deployment.md#multiple), [Datenpersistenz](sql-server-linux-docker-container-configure.md#persist) and [Problembehandlung](sql-server-linux-docker-container-troubleshooting.md).

Ressourcen, Feedback und Informationen über bekannte Probleme finden Sie im [GitHub-Repository „mssql-docker“](https://github.com/Microsoft/mssql-docker).
