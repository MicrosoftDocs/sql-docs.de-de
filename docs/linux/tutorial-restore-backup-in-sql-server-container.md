---
title: Wiederherstellen einer SQL Server-Datenbank in Docker
description: In diesem Tutorial erfahren Sie, wie Sie eine SQL Server-Datenbanksicherung in einem Docker-Container in Linux wiederherstellen.
author: VanMSFT
ms.author: vanto
ms.date: 03/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
ms.openlocfilehash: 1bb137243e5a297baae55e72afeaca356e8f4d57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347362"
---
# <a name="restore-a-sql-server-database-in-a-linux-docker-container"></a>Wiederherstellen einer SQL Server-Datenbank in einem Docker-Container in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

In diesem Tutorial erfahren Sie, wie Sie eine SQL Server-Sicherungsdatei in ein Linux-Containerimage von SQL Server 2017 in Docker verschieben und dort wiederherstellen.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

In diesem Tutorial erfahren Sie, wie Sie eine SQL Server-Sicherungsdatei in ein Linux-Containerimage von SQL Server 2019 in Docker verschieben und dort wiederherstellen.

::: moniker-end

> [!div class="checklist"]
> * Übertragen Sie das neueste Linux-Containerimage von SQL Server per Pull, und führen Sie es aus.
> * Kopieren Sie die Datenbankdatei von Wide World Importers in den Container.
> * Stellen Sie die Datenbank im Container wieder her.
> * Führen Sie Transact-SQL-Anweisungen aus, um die Datenbank anzuzeigen und zu ändern.
> * Sichern Sie die geänderte Datenbank.

## <a name="prerequisites"></a>Voraussetzungen

* Docker-Engine 1.8 und höher auf unterstütztem Linux-Betriebssystem oder Docker für Mac bzw. Windows. Weitere Informationen finden Sie unter [Install Docker (Installieren von Docker)](https://docs.docker.com/engine/installation/).
* Mindestens 2 GB freier Speicherplatz auf dem Datenträger
* Mindestens 2 GB RAM
* [Systemanforderungen von SQL Server unter Linux](sql-server-linux-setup.md#system)

## <a name="pull-and-run-the-container-image"></a>Übertragen mithilfe von Pull und Ausführen von Containerimages

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Öffnen Sie unter Linux/Mac ein Bash-Terminal oder unter Windows eine PowerShell-Sitzung mit erhöhten Rechten.

1. Übertragen Sie das Linux-Containerimage von SQL Server 2017 mithilfe von Pull aus dem Docker-Hub.

   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2017-latest
   ```

   > [!TIP]
   > Die in diesem Tutorial verwendeten Beispiele für Docker-Befehle werden jeweils für die Bash-Shell (Linux/Mac) und für PowerShell (Windows) aufgeführt.

1. Verwenden Sie zum Ausführen des Containerimages mit Docker den folgenden Befehl:

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql1' -p 1401:1433 \
      -v sql1data:/var/opt/mssql \
      -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql1" -p 1401:1433 `
      -v sql1data:/var/opt/mssql `
      -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   Mit diesem Befehl wird ein Container von SQL Server 2017 erstellt, der standardmäßig die Developer Edition enthält. Der SQL Server-Port **1433** wird auf dem Host als Port **1401** bereitgestellt. Mit dem optionalen Parameter `-v sql1data:/var/opt/mssql` wird ein Datenvolumecontainer namens **sql1ddata** erstellt. Dieser wird verwendet, um die von SQL Server erstellten Daten permanent zu speichern.

   > [!IMPORTANT]
   > In diesem Beispiel wird ein Datenvolumecontainer innerhalb von Docker verwendet. Wenn Sie stattdessen die Zuordnung eines Hostverzeichnisses gewählt haben, beachten Sie, dass es für diesen Ansatz Einschränkungen in Docker für Mac und Windows gibt. Weitere Informationen finden Sie unter [Konfigurieren von SQL Server-Containerimages in Docker](./sql-server-linux-docker-container-configure.md#persist).

1. Verwenden Sie den Befehl `docker ps`, um Ihre Docker-Container anzeigen zu lassen.

   ```bash
   sudo docker ps -a
   ```

   ```PowerShell
   docker ps -a
   ```

1. Wenn in der Spalte **STATUS** **Up** (Aktiv) eingetragen ist, wird SQL Server im Container ausgeführt und überwacht den Port, der in der Spalte **PORTS** angegeben ist. Wenn in der Spalte **STATUS** Ihres SQL Server-Containers **Exited** (Beendet) steht, lesen Sie bitte im [Abschnitt „Problembehebung“ im Konfigurationshandbuch](./sql-server-linux-docker-container-troubleshooting.md) nach.

  ```bash
  $ sudo docker ps -a

  CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
  941e1bdf8e1d        mcr.microsoft.com/mssql/server/mssql-server-linux   "/bin/sh -c /opt/m..."   About an hour ago   Up About an hour    0.0.0.0:1401->1433/tcp   sql1
  ```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Öffnen Sie unter Linux/Mac ein Bash-Terminal oder unter Windows eine PowerShell-Sitzung mit erhöhten Rechten.

1. Übertragen Sie das Linux-Containerimage von SQL Server 2019 mithilfe von Pull aus dem Docker Hub.

   ```bash
   sudo docker pull mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   > [!TIP]
   > Die in diesem Tutorial verwendeten Beispiele für Docker-Befehle werden jeweils für die Bash-Shell (Linux/Mac) und für PowerShell (Windows) aufgeführt.

1. Verwenden Sie zum Ausführen des Containerimages mit Docker den folgenden Befehl:

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql1' -p 1401:1433 \
      -v sql1data:/var/opt/mssql \
      -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql1" -p 1401:1433 `
      -v sql1data:/var/opt/mssql `
      -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
   ```

   Mit diesem Befehl wird ein Container von SQL Server 2019 erstellt, der standardmäßig die Developer-Edition enthält. Der SQL Server-Port **1433** wird auf dem Host als Port **1401** bereitgestellt. Mit dem optionalen Parameter `-v sql1data:/var/opt/mssql` wird ein Datenvolumecontainer namens **sql1ddata** erstellt. Dieser wird verwendet, um die von SQL Server erstellten Daten permanent zu speichern.

1. Verwenden Sie den Befehl `docker ps`, um Ihre Docker-Container anzeigen zu lassen.

   ```bash
   sudo docker ps -a
   ```

   ```PowerShell
   docker ps -a
   ```

1. Wenn in der Spalte **STATUS** **Up** (Aktiv) eingetragen ist, wird SQL Server im Container ausgeführt und überwacht den Port, der in der Spalte **PORTS** angegeben ist. Wenn in der Spalte **STATUS** Ihres SQL Server-Containers **Exited** (Beendet) steht, lesen Sie bitte im [Abschnitt „Problembehebung“ im Konfigurationshandbuch](./sql-server-linux-docker-container-troubleshooting.md) nach.

   ```bash
   $ sudo docker ps -a

   CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
   941e1bdf8e1d        mcr.microsoft.com/mssql/server/mssql-server-linux   "/bin/sh -c /opt/m..."   About an hour ago   Up About an hour    0.0.0.0:1401->1433/tcp   sql1
   ```

::: moniker-end

## <a name="change-the-sa-password"></a>Ändern des Systemadministratorkennworts

[!INCLUDE [Change docker password](../includes/sql-server-linux-change-docker-password.md)]

## <a name="copy-a-backup-file-into-the-container"></a>Kopieren einer Sicherungsdatei in den Container

In diesem Tutorial wird die [Beispieldatenbank von Wide World Importers](../samples/wide-world-importers-what-is.md) verwendet. Führen Sie die folgenden Schritte aus, um die Sicherungsdatei der Datenbank von Wide World Importers in Ihren SQL Server-Container herunterzuladen und zu kopieren:

1. Erstellen Sie zunächst mit dem Befehl **docker exec** einen Sicherungsordner. Mit dem folgenden Befehl wird im SQL Server-Container ein Verzeichnis vom Typ **/var/opt/mssql/backup** erstellt:

   ```bash
   sudo docker exec -it sql1 mkdir /var/opt/mssql/backup
   ```

   ```PowerShell
   docker exec -it sql1 mkdir /var/opt/mssql/backup
   ```

1. Laden Sie nun die Datei [WideWorldImporters-Full.bak](https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0) auf Ihren Hostcomputer herunter. Mit den folgenden Befehlen wechseln Sie zum Basis-/Benutzerverzeichnis und laden die Sicherungsdatei als **wwi.bak** herunter.

   ```bash
   cd ~
   curl -L -o wwi.bak 'https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Full.bak'
   ```

   ```PowerShell
   curl -OutFile "wwi.bak" "https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Full.bak"
   ```

1. Kopieren Sie mit dem Befehl **docker cp** die Sicherungsdatei in den Container im Verzeichnis **/var/opt/mssql/backup**.

   ```bash
   sudo docker cp wwi.bak sql1:/var/opt/mssql/backup
   ```

   ```PowerShell
   docker cp wwi.bak sql1:/var/opt/mssql/backup
   ```

## <a name="restore-the-database"></a>Wiederherstellen der Datenbank

Die Sicherungsdatei befindet sich nun im Container. Bevor Sie die Sicherung wiederherstellen können, müssen Sie die logischen Dateinamen und Dateitypen in der Sicherung kennen. Mit den folgenden Transact-SQL-Befehlen überprüfen Sie die Sicherung und führen die Wiederherstellung aus, indem Sie den Befehl **sqlcmd** im Container verwenden.

> [!TIP]
> In diesem Tutorial wird der Befehl **sqlcmd** im Container verwendet, da er dieses vorinstallierte Tool bereits enthält. Sie können Transact-SQL-Anweisungen jedoch auch außerhalb des Containers mit anderen Clienttools wie [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md) oder [SQL Server Management Studio](sql-server-linux-manage-ssms.md) ausführen. Verwenden Sie zum Herstellen einer Verbindung den Hostport, der Port 1433 im Container zugeordnet wurde. In diesem Beispiel ist dies **localhost, 1401** auf dem Hostcomputer und **Host_IP_Address,1401** für den Remotezugriff.

1. Führen Sie im Container den Befehl **sqlcmd** aus, um die logischen Dateinamen und Pfade in der Sicherung aufzulisten. Dies geschieht über die Transact-SQL-Anweisung **RESTORE FILELISTONLY**.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost \
      -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'RESTORE FILELISTONLY FROM DISK = "/var/opt/mssql/backup/wwi.bak"' \
      | tr -s ' ' | cut -d ' ' -f 1-2
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost `
      -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backup/wwi.bak'"
   ```

   Die Ausgabe sollte etwa folgendermaßen aussehen:

   ```
   LogicalName   PhysicalName
   ------------------------------------------
   WWI_Primary   D:\Data\WideWorldImporters.mdf
   WWI_UserData   D:\Data\WideWorldImporters_UserData.ndf
   WWI_Log   E:\Log\WideWorldImporters.ldf
   WWI_InMemory_Data_1   D:\Data\WideWorldImporters_InMemory_Data_1
   ```

1. Rufen Sie den Befehl **RESTORE DATABASE** auf, um die Datenbank im Container wiederherzustellen. Geben Sie für jede der Dateien aus dem vorherigen Schritt einen neuen Pfad an.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'RESTORE DATABASE WideWorldImporters FROM DISK = "/var/opt/mssql/backup/wwi.bak" WITH MOVE "WWI_Primary" TO "/var/opt/mssql/data/WideWorldImporters.mdf", MOVE "WWI_UserData" TO "/var/opt/mssql/data/WideWorldImporters_userdata.ndf", MOVE "WWI_Log" TO "/var/opt/mssql/data/WideWorldImporters.ldf", MOVE "WWI_InMemory_Data_1" TO "/var/opt/mssql/data/WideWorldImporters_InMemory_Data_1"'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "RESTORE DATABASE WideWorldImporters FROM DISK = '/var/opt/mssql/backup/wwi.bak' WITH MOVE 'WWI_Primary' TO '/var/opt/mssql/data/WideWorldImporters.mdf', MOVE 'WWI_UserData' TO '/var/opt/mssql/data/WideWorldImporters_userdata.ndf', MOVE 'WWI_Log' TO '/var/opt/mssql/data/WideWorldImporters.ldf', MOVE 'WWI_InMemory_Data_1' TO '/var/opt/mssql/data/WideWorldImporters_InMemory_Data_1'"
   ```

   Die Ausgabe sollte etwa folgendermaßen aussehen:

   ```
   Processed 1464 pages for database 'WideWorldImporters', file 'WWI_Primary' on file 1.
   Processed 53096 pages for database 'WideWorldImporters', file 'WWI_UserData' on file 1.
   Processed 33 pages for database 'WideWorldImporters', file 'WWI_Log' on file 1.
   Processed 3862 pages for database 'WideWorldImporters', file 'WWI_InMemory_Data_1' on file 1.
   Converting database 'WideWorldImporters' from version 852 to the current version 869.
   Database 'WideWorldImporters' running the upgrade step from version 852 to version 853.
   Database 'WideWorldImporters' running the upgrade step from version 853 to version 854.
   Database 'WideWorldImporters' running the upgrade step from version 854 to version 855.
   Database 'WideWorldImporters' running the upgrade step from version 855 to version 856.
   Database 'WideWorldImporters' running the upgrade step from version 856 to version 857.
   Database 'WideWorldImporters' running the upgrade step from version 857 to version 858.
   Database 'WideWorldImporters' running the upgrade step from version 858 to version 859.
   Database 'WideWorldImporters' running the upgrade step from version 859 to version 860.
   Database 'WideWorldImporters' running the upgrade step from version 860 to version 861.
   Database 'WideWorldImporters' running the upgrade step from version 861 to version 862.
   Database 'WideWorldImporters' running the upgrade step from version 862 to version 863.
   Database 'WideWorldImporters' running the upgrade step from version 863 to version 864.
   Database 'WideWorldImporters' running the upgrade step from version 864 to version 865.
   Database 'WideWorldImporters' running the upgrade step from version 865 to version 866.
   Database 'WideWorldImporters' running the upgrade step from version 866 to version 867.
   Database 'WideWorldImporters' running the upgrade step from version 867 to version 868.
   Database 'WideWorldImporters' running the upgrade step from version 868 to version 869.
   RESTORE DATABASE successfully processed 58455 pages in 18.069 seconds (25.273 MB/sec).
   ```

## <a name="verify-the-restored-database"></a>Überprüfen der wiederhergestellten Datenbank

Führen Sie die folgende Abfrage aus, um eine Liste der Datenbanknamen in Ihrem Container anzuzeigen:

```bash
sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
   -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
   -Q 'SELECT Name FROM sys.Databases'
```

```PowerShell
docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
   -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
   -Q "SELECT Name FROM sys.Databases"
```

Die Liste der Datenbanken sollte den Eintrag **WideWorldImporters** enthalten.

## <a name="make-a-change"></a>Vornehmen einer Änderung

Führen Sie die folgenden Schritte aus, um in der Datenbank eine Änderung vorzunehmen:

1. Führen Sie eine Abfrage aus, um die obersten zehn Elemente der Tabelle **Warehouse.StockItems** anzuzeigen.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT TOP 10 StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems ORDER BY StockItemID'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT TOP 10 StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems ORDER BY StockItemID"
   ```

   Es sollte eine Liste mit den Bezeichnern und Namen der Elemente angezeigt werden:

   ```
   StockItemID StockItemName
   ----------- -----------------
             1 USB missile launcher (Green)
             2 USB rocket launcher (Gray)
             3 Office cube periscope (Black)
             4 USB food flash drive - sushi roll
             5 USB food flash drive - hamburger
             6 USB food flash drive - hot dog
             7 USB food flash drive - pizza slice
             8 USB food flash drive - dim sum 10 drive variety pack
             9 USB food flash drive - banana
            10 USB food flash drive - chocolate bar
   ```

1. Aktualisieren Sie mit der folgenden **UPDATE**-Anweisung die Beschreibung des ersten Elements:

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'UPDATE WideWorldImporters.Warehouse.StockItems SET StockItemName="USB missile launcher (Dark Green)" WHERE StockItemID=1; SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "UPDATE WideWorldImporters.Warehouse.StockItems SET StockItemName='USB missile launcher (Dark Green)' WHERE StockItemID=1; SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   Die Ausgabe sollte etwa folgendermaßen aussehen:

   ```
   (1 rows affected)
   StockItemID StockItemName
   ----------- ------------------------------------
             1 USB missile launcher (Dark Green)
   ```

## <a name="create-a-new-backup"></a>Erstellen einer neuen Sicherung

Nachdem Sie Ihre Datenbank in einem Container wiederhergestellt haben, können Sie auch regelmäßige Datenbanksicherungen im ausgeführten Container erstellen. Die dafür erforderlichen Schritte ähneln den vorherigen, jedoch in umgekehrter Reihenfolge.

1. Erstellen Sie mit dem Transact-SQL-Befehl **BACKUP DATABASE** eine Datenbanksicherung im Container. In diesem Tutorial wird im zuvor erstellten Verzeichnis **/var/opt/mssql/backup** eine neue Sicherungsdatei namens **wwi_2.bak** erstellt.

   ```bash
   sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q "BACKUP DATABASE [WideWorldImporters] TO DISK = N'/var/opt/mssql/backup/wwi_2.bak' WITH NOFORMAT, NOINIT, NAME = 'WideWorldImporters-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
   ```

   ```PowerShell
   docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "BACKUP DATABASE [WideWorldImporters] TO DISK = N'/var/opt/mssql/backup/wwi_2.bak' WITH NOFORMAT, NOINIT, NAME = 'WideWorldImporters-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
   ```

   Die Ausgabe sollte etwa folgendermaßen aussehen:

   ```
   10 percent processed.
   20 percent processed.
   30 percent processed.
   40 percent processed.
   50 percent processed.
   60 percent processed.
   70 percent processed.
   Processed 1200 pages for database 'WideWorldImporters', file 'WWI_Primary' on file 1.
   Processed 53096 pages for database 'WideWorldImporters', file 'WWI_UserData' on file 1.
   80 percent processed.
   Processed 3865 pages for database 'WideWorldImporters', file 'WWI_InMemory_Data_1' on file 1.
   Processed 938 pages for database 'WideWorldImporters', file 'WWI_Log' on file 1.
   100 percent processed.
   BACKUP DATABASE successfully processed 59099 pages in 25.056 seconds (18.427 MB/sec).
   ```

1. Kopieren Sie nun die Sicherungsdatei aus dem Container auf den Hostcomputer.

   ```bash
   cd ~
   sudo docker cp sql1:/var/opt/mssql/backup/wwi_2.bak wwi_2.bak
   ls -l wwi*
   ```

   ```PowerShell
   cd ~
   docker cp sql1:/var/opt/mssql/backup/wwi_2.bak wwi_2.bak
   ls wwi*
   ```

## <a name="use-the-persisted-data"></a>Verwenden der persistenten Daten

Zum Schutz Ihrer Daten können Sie neben Datenbanksicherungen auch Datenvolumecontainer verwenden. Zu Beginn dieses Tutorials haben Sie mit dem Parameter `-v sql1data:/var/opt/mssql` den Container **sql1** erstellt. Im Datenvolumecontainer **sql1data** werden die Daten aus **/var/opt/mssql** auch dann weiterhin gespeichert, wenn der Container gelöscht wird. Führen Sie die folgenden Schritte aus, um den Container **sql1** vollständig zu entfernen und anschließend einen neuen Container namens **sql2** mit den persistenten Daten zu erstellen.

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

1. Beenden Sie den Container **sql1**.

   ```bash
   sudo docker stop sql1
   ```

   ```PowerShell
   docker stop sql1
   ```

1. Entfernen Sie den Container. Durch diesen Vorgang werden der zuvor erstellte Datenvolumecontainer **sql1data** und die darin enthaltenen persistenten Daten nicht gelöscht.

   ```bash
   sudo docker rm sql1
   ```

   ```PowerShell
   docker rm sql1
   ```

1. Erstellen Sie einen neuen Container namens **sql2**, und verwenden Sie den Datenvolumecontainer **sql1data** erneut.

   ```bash
   sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
      --name 'sql2' -e 'MSSQL_PID=Developer' -p 1401:1433 \
      -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

   ```PowerShell
   docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
      --name "sql2" -e "MSSQL_PID=Developer" -p 1401:1433 `
      -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2017-latest
   ```

1. Die Datenbank von Wide World Importers befindet sich nun im neuen Container. Führen Sie eine Abfrage aus, um die vorherige Änderung zu überprüfen.

   ```bash
   sudo docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   > [!NOTE]
   > Das SA-Kennwort ist nicht mit dem Kennwort identisch, das Sie für den Container **sql2** angegeben haben (`MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>`). Alle SQL Server-Daten aus **sql1** wurden wiederhergestellt, einschließlich des zuvor in diesem Tutorial geänderten Kennworts. Einige Optionen wie diese werden aufgrund der Wiederherstellung der Daten in „/var/opt/mssql“ ignoriert. Daher lautet das Kennwort wie hier gezeigt `<YourNewStrong!Passw0rd>`.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

1. Beenden Sie den Container **sql1**.

   ```bash
   sudo docker stop sql1
   ```

   ```PowerShell
   docker stop sql1
   ```

1. Entfernen Sie den Container. Durch diesen Vorgang werden der zuvor erstellte Datenvolumecontainer **sql1data** und die darin enthaltenen persistenten Daten nicht gelöscht.

   ```bash
   sudo docker rm sql1
   ```

   ```PowerShell
   docker rm sql1
   ```

1. Erstellen Sie einen neuen Container namens **sql2**, und verwenden Sie den Datenvolumecontainer **sql1data** erneut.

    ```bash
    sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
       --name 'sql2' -e 'MSSQL_PID=Developer' -p 1401:1433 \
       -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
    ```

    ```PowerShell
    docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
       --name "sql2" -e "MSSQL_PID=Developer" -p 1401:1433 `
       -v sql1data:/var/opt/mssql -d mcr.microsoft.com/mssql/server:2019-CU3-ubuntu-18.04
    ```

1. Die Datenbank von Wide World Importers befindet sich nun im neuen Container. Führen Sie eine Abfrage aus, um die vorherige Änderung zu überprüfen.

   ```bash
   sudo docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd \
      -S localhost -U SA -P '<YourNewStrong!Passw0rd>' \
      -Q 'SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1'
   ```

   ```PowerShell
   docker exec -it sql2 /opt/mssql-tools/bin/sqlcmd `
      -S localhost -U SA -P "<YourNewStrong!Passw0rd>" `
      -Q "SELECT StockItemID, StockItemName FROM WideWorldImporters.Warehouse.StockItems WHERE StockItemID=1"
   ```

   > [!NOTE]
   > Das SA-Kennwort ist nicht mit dem Kennwort identisch, das Sie für den Container **sql2** angegeben haben (`MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>`). Alle SQL Server-Daten aus **sql1** wurden wiederhergestellt, einschließlich des zuvor in diesem Tutorial geänderten Kennworts. Einige Optionen wie diese werden aufgrund der Wiederherstellung der Daten in „/var/opt/mssql“ ignoriert. Daher lautet das Kennwort wie hier gezeigt `<YourNewStrong!Passw0rd>`.

::: moniker-end

## <a name="next-steps"></a>Nächste Schritte

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

In diesem Tutorial haben Sie erfahren, wie Sie eine Datenbank unter Windows sichern und auf einen Linux-Server mit SQL Server 2017 verschieben. Sie haben Folgendes gelernt:

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

In diesem Tutorial haben Sie gelernt, wie Sie eine Datenbank unter Windows sichern und auf einen Linux-Server mit SQL Server 2019 verschieben. Sie haben Folgendes gelernt:

::: moniker-end

> [!div class="checklist"]
> * Erstellen von Linux-Containerimages in SQL Server
> * Kopieren von SQL Server-Datenbanksicherungen in einen Container
> * Ausführen von Transact-SQL-Anweisungen im Container mit **sqlcmd**
> * Erstellen und Extrahieren von Sicherungsdateien aus einem Container
> * Permanentes Speichern von SQL Server-Daten mithilfe von Datenvolumecontainern in Docker

Erfahren Sie nun mehr zu weiteren Szenarios für Docker-Konfigurationen und -Problembehandlung:

> [!div class="nextstepaction"]
>[Configuration guide for SQL Server 2017 on Docker (Konfigurationsleitfaden für SQL Server 2017 in Docker)](./sql-server-linux-docker-container-deployment.md)