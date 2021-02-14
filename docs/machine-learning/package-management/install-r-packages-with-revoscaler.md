---
title: Verwenden von RevoScaleR zum Installieren von R-Paketen
description: Erfahren Sie, wie RevoScaleR-Funktionen zur Installation von R-Paketen in SQL Server mit Machine Learning Services oder R-Services verwendet werden.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 99750753f7b74070ecb3bc8217d72b91255a0962
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082611"
---
# <a name="use-revoscaler-to-install-r-packages"></a>Verwenden von RevoScaleR zum Installieren von R-Paketen

[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

In diesem Artikel wird beschrieben, wie [RevoScaleR](../r/ref-r-revoscaler.md)-Funktionen (ab Version 9.0.1) zur Installation von R-Paketen in SQL Server mit Machine Learning Services oder R-Services verwendet werden. Die RevoScaleR-Funktionen können von remote arbeitenden Nichtadministratoren verwendet werden, um ohne direkten Zugriff auf den Server Pakete in SQL Server zu installieren.

::: moniker range="=sql-server-2016"
> [!NOTE]
> SQL Server R Services-Kunden müssen ein [Komponentenupgrade](../install/upgrade-r-and-python.md) durchführen, um die RevoScaleR-Paketverwaltungsfunktionen nutzen zu können. Anweisungen zum Abrufen der Paketversion und des Inhalts finden Sie unter [Abrufen von R-Paketinformationen](../package-management/r-package-information.md).
::: moniker-end

## <a name="revoscaler-functions-for-package-management"></a>RevoScaleR-Funktionen für die Paketverwaltung

In der folgenden Tabelle werden die Funktionen beschrieben, die für die Installation und Verwaltung von R-Paketen verwendet werden.

| Funktion | BESCHREIBUNG |
|----------|-------------|
| [rxSqlLibPaths](/machine-learning-server/r-reference/revoscaler/rxsqllibpaths) | Bestimmt den Pfad der Instanzbibliothek in der Remote-Instanz von SQL Server. |
| [rxFindPackage](/machine-learning-server/r-reference/revoscaler/rxfindpackage) | Ruft den Pfad für ein oder mehrere Pakete in der Remote-Instanz von SQL Server ab. |
| [rxInstallPackages](/machine-learning-server/r-reference/revoscaler/rxinstallpackages) | Rufen Sie diese Funktion auf einem R-Remoteclient auf, um Pakete in einem SQL Server-Computekontext zu installieren, entweder über ein angegebenes Repository oder durch Lesen lokal gespeicherter ZIP-Pakete. Diese Funktion prüft Abhängigkeiten und stellt sicher, dass alle zugehörigen Pakete in SQL Server installiert werden können, genau wie bei der Installation von R-Paketen im lokalen Computekontext. Um diese Option verwenden zu können, müssen Sie die Paketverwaltung auf dem Server und in der Datenbank aktiviert haben. Sowohl Client- als auch Serverumgebung müssen die gleiche Version von RevoScaleR aufweisen. |
| [rxInstalledPackages](/machine-learning-server/r-reference/revoscaler/rxinstalledpackages) | Ruft eine Liste der im angegebenen Computekontext installierten Pakete ab. |
| [rxSyncPackages](/machine-learning-server/r-reference/revoscaler/rxsyncpackages) | Kopiert Informationen zu einer Paketbibliothek zwischen dem Dateisystem und der Datenbank für den angegebenen Computekontext. |
| [rxRemovePackages](/machine-learning-server/r-reference/revoscaler/rxremovepackages) | Entfernt Pakete aus einem angegebenen Computekontext. Die Funktion berechnet außerdem Abhängigkeiten und stellt sicher, dass Pakete, die nicht mehr von anderen Paketen in SQL Server verwendet werden, entfernt werden, um Ressourcen freizugeben. |

## <a name="prerequisites"></a>Voraussetzungen

+ Remoteverwaltung in SQL Server ist aktiviert. Weitere Informationen finden Sie unter [Aktivieren der Remoteverwaltung für R-Pakete in SQL Server](r-package-how-to-enable-or-disable.md).

+ RevoScaleR-Versionen sind in Client- und Serverumgebung gleich. Weitere Informationen finden Sie unter [Abrufen von Paketinformationen für R](../package-management/r-package-information.md).

+ Sie haben die Berechtigung, sich mit dem Server und einer Datenbank zu verbinden und R-Befehle auszuführen. Sie müssen Mitglied einer Datenbankrolle sein, die erlaubt, Pakete in der angegebenen Instanz und Datenbank zu installieren.

  + Pakete im **freigegebenen Bereich** können von Benutzern installiert werden, die der Rolle `rpkgs-shared` in einer bestimmten Datenbank angehören. Alle Benutzer mit dieser Rolle können freigegebene Pakete deinstallieren.

  + Pakete im **privaten Bereich** können von jedem Benutzer installiert werden, der der Rolle `rpkgs-private` in einer Datenbank angehört. Benutzer können jedoch nur Ihre eigenen Pakete sehen und deinstallieren.

  + Datenbankbesitzer können mit freigegebenen oder privaten Paketen arbeiten.

## <a name="client-connections"></a>Clientverbindungen

Eine Clientarbeitsstation kann [Microsoft R Client](/machine-learning-server/r-client/install-on-windows) oder [Microsoft Machine Learning Server](/machine-learning-server/install/machine-learning-server-windows-install) (Datenwissenschaftler verwenden oft die kostenlose Entwickler-Edition) im gleichen Netzwerk sein.

Wenn Sie Paketverwaltungsfunktionen auf einem R-Remoteclient aufrufen, müssen Sie zunächst mit der Funktion [RxInSqlServer](/machine-learning-server/r-reference/revoscaler/rxinsqlserver) ein Computekontextobjekt erstellen. Danach übergeben Sie für jede Paketverwaltungsfunktion, die Sie verwenden, den Computekontext als Argument.

Die Benutzeridentität wird in der Regel beim Festlegen des Computekontexts angegeben. Wenn Sie beim Erstellen des Computekontexts keinen Benutzernamen und kein Kennwort angeben, wird die Identität des Benutzers verwendet, der den R-Code ausführt.

1. Definieren Sie über eine R-Befehlszeile eine Verbindungszeichenfolge zur Instanz und Datenbank.
2. Verwenden Sie den Konstruktor [RxInSqlServer](/machine-learning-server/r-reference/revoscaler/rxinsqlserver), um einen SQL Server-Computekontext unter Verwendung der Verbindungszeichenfolge zu definieren.

    ```R
    sqlcc <- RxInSqlServer(connectionString = myConnString, shareDir = sqlShareDir, wait = sqlWait, consoleOutput = sqlConsoleOutput)
    ```
3. Erstellen Sie eine Liste der Pakete, die Sie installieren möchten, und speichern Sie die Liste in einer Zeichenfolgenvariablen.

    ```R
    packageList <- c("e1071", "mice")
    ```

4. Rufen Sie [rxInstallPackages](/machine-learning-server/r-reference/revoscaler/rxinstallpackages) auf, und übergeben Sie den Computekontext und die Zeichenfolgenvariable, die die Paketnamen enthält.

    ```R
    rxInstallPackages(pkgs = packageList, verbose = TRUE, computeContext = sqlcc)
    ```

    Wenn abhängige Pakete erforderlich sind, werden sie ebenfalls installiert, vorausgesetzt, auf dem Client ist eine Internetverbindung verfügbar.
    
    Pakete werden unter Verwendung der Anmeldeinformationen des Benutzers installiert, der die Verbindung herstellt, und zwar im Standardgeltungsbereich dieses Benutzers.

## <a name="call-package-management-functions-in-stored-procedures"></a>Aufrufen von Paketverwaltungsfunktionen in gespeicherten Prozeduren

Sie können Paketverwaltungsfunktionen in `sp_execute_external_script` ausführen. Dabei wird die Funktion im Sicherheitskontext des Aufrufers der gespeicherten Prozedur ausgeführt.

## <a name="examples"></a>Beispiele

Dieser Abschnitt enthält Beispiele für die Verwendung dieser Funktionen auf einem Remoteclient, wenn eine Verbindung mit einer SQL Server-Instanz oder -Datenbank als Computekontext hergestellt wird.

Für alle Beispiele müssen Sie entweder eine Verbindungszeichenfolge oder einen Computekontext angeben, für den eine Verbindungszeichenfolge erforderlich ist. Dieses Beispiel bietet eine Möglichkeit, einen Computekontext für SQL Server zu erstellen:

```R
instance_name <- "computer-name/instance-name";
database_name <- "TestDB";
sqlWait= TRUE;
sqlConsoleOutput <- TRUE;
connString <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
sqlcc <- RxInSqlServer(connectionString = connString, wait = sqlWait, consoleOutput = sqlConsoleOutput, numTasks = 4);
```

Je nach Standort des Servers und Sicherheitsmodell müssen Sie möglicherweise eine Domänen- und Subnetzspezifikation in der Verbindungszeichenfolge angeben oder eine SQL Server-Anmeldung verwenden. Beispiel:

```R
connStr <- "Driver=SQL Server;Server=myserver.financeweb.contoso.com;Database=Finance;Uid=RUser1;Pwd=RUserPassword"
```

### <a name="get-package-path-on-a-remote-sql-server-compute-context"></a>Abrufen des Paketpfads für einen SQL Server-Remotecomputekontext

Dieses Beispiel ruft den Pfad des Pakets **RevoScaleR** im Computekontext ab, `sqlcc`.

```R
sqlPackagePaths <- rxFindPackage(package = "RevoScaleR", computeContext = sqlcc)
print(sqlPackagePaths)
```

**Ergebnisse**

"C:/Program Files/Microsoft SQL Server/MSSQL14.MSSQLSERVER/R_SERVICES/library/RevoScaleR"

> [!TIP]
> Wenn Sie die Option zur Anzeige der SQL-Konsolenausgabe aktiviert haben, erhalten Sie möglicherweise Statusmeldungen von der Funktion, die der `print`-Anweisung vorausgeht. Nachdem Sie Ihren Code getestet haben, legen Sie `consoleOutput` im Konstruktor des Computekontexts auf FALSE fest, um Meldungen zu vermeiden.

### <a name="get-locations-for-multiple-packages"></a>Abrufen von Speicherorten für mehrere Pakete

Das folgende Beispiel ruft die Pfade für die Pakete **RevoScaleR** und **lattice** im Computekontext ab, `sqlcc`. Um Informationen zu mehreren Paketen abzurufen, wird ein Zeichenfolgenvektor übergeben, der die Paketnamen enthält.

```R
packagePaths <- rxFindPackage(package = c("RevoScaleR", "lattice"), computeContext = sqlcc)
print(packagePaths)
```

### <a name="get-package-versions-on-a-remote-compute-context"></a>Abrufen der Paketversionen für einen Remotecomputekontext

Führen Sie diesen Befehl über eine R-Konsole aus, um die Buildnummer und die Versionsnummern für die im Computekontext installierten Pakete zu erhalten, *sqlServer*.

```R
sqlPackages <- rxInstalledPackages(fields = c("Package", "Version", "Built"), computeContext = sqlServer)
```

### <a name="install-a-package-on-sql-server"></a>Installieren eines Pakets in SQL Server

In diesem Beispiel wird das Paket **forecast** mit seinen Abhängigkeiten in den Computekontext installiert.

  ```R
  pkgs <- c("forecast")
  rxInstallPackages(pkgs = pkgs, verbose = TRUE, scope = "private", computeContext = sqlcc)
  ```

### <a name="remove-a-package-from-sql-server"></a>Entfernen eines Pakets aus SQL Server

In diesem Beispiel wird das Paket **forecast** mit seinen Abhängigkeiten aus dem Computekontext entfernt.

  ```R
  pkgs <- c("forecast")
  rxRemovePackages(pkgs = pkgs, verbose = TRUE, scope = "private", computeContext = sqlcc)
  ```

### <a name="synchronize-packages-between-database-and-file-system"></a>Synchronisieren von Paketen zwischen Datenbank und Dateisystem

Das folgende Beispiel prüft die Datenbank **TestDB** und ermittelt, ob alle Pakete im Dateisystem installiert sind. Wenn einige Pakete fehlen, werden sie im Dateisystem installiert.

```R
# Instantiate the compute context
connectionString <- "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;"
computeContext <- RxInSqlServer(connectionString = connectionString )

# Synchronize the packages in the file system for all scopes and users
rxSyncPackages(computeContext=computeContext, verbose=TRUE)
```

Die Paketsynchronisierung funktioniert datenbank- und benutzerbezogen. Weitere Informationen finden Sie unter [R-Paketsynchronisierung für SQL Server](package-install-uninstall-and-sync.md).

### <a name="use-a-stored-procedure-to-list-packages-in-sql-server"></a>Verwenden einer gespeicherten Prozedur zum Auflisten von Paketen in SQL Server

Führen Sie diesen Befehl in Management Studio oder einem anderen Tool mit Unterstützung von T-SQL aus, um eine Liste der in der aktuellen Instanz installierten Pakete unter Verwendung von `rxInstalledPackages` in einer gespeicherten Prozedur abzurufen.

```sql
EXEC sp_execute_external_script 
  @language=N'R', 
  @script=N'
    myPackages <- rxInstalledPackages();
    OutputDataSet <- as.data.frame(myPackages);
    '
```

Die `rxSqlLibPaths`-Funktion kann verwendet werden, um die aktive Bibliothek zu ermitteln, die von SQL Server Machine Learning Services verwendet wird. Dieses Skript kann nur den Bibliothekspfad für den aktuellen Server zurückgeben. 

```sql
declare @instance_name nvarchar(100) = @@SERVERNAME, @database_name nvarchar(128) = db_name();
exec sp_execute_external_script 
  @language = N'R',
  @script = N'
    connStr <- paste("Driver=SQL Server;Server=", instance_name, ";Database=", database_name, ";Trusted_Connection=true;", sep="");
    .libPaths(rxSqlLibPaths(connStr));
    print(.libPaths());
  ', 
  @input_data_1 = N'', 
  @params = N'@instance_name nvarchar(100), @database_name nvarchar(128)',
  @instance_name = @instance_name, 
  @database_name = @database_name;
```

## <a name="see-also"></a>Weitere Informationen

+ [Remoteverwaltung für R-Pakete aktivieren](r-package-how-to-enable-or-disable.md)
+ [Synchronisieren von R-Paketen](package-install-uninstall-and-sync.md)
+ [Tipps für die Verwendung von R-Paketen](tips-for-using-r-packages.md)
+ [Abrufen von R-Paketinformationen](../package-management/r-package-information.md)