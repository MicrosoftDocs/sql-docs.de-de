---
title: Installieren von R-Paketen mit sqlmlutils
description: Hier erfahren Sie, wie Sie mit sqlmlutils neue R-Pakete in einer Instanz von SQL Server Machine Learning Services installieren können.
ms.prod: sql
ms.technology: machine-learning
ms.date: 12/15/2020
ms.topic: how-to
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 489bea391897c0924690719343bdf5febc37f35f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350682"
---
# <a name="install-r-packages-with-sqlmlutils"></a>Installieren von R-Paketen mit sqlmlutils

[!INCLUDE [SQL Server 2019 SQL MI](../../includes/applies-to-version/sqlserver2019-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In diesem Artikel wird beschrieben, wie Sie Funktionen im [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils)-Paket verwenden, um R-Pakete in einer Instanz von [Machine Learning Services in SQL Server](../sql-server-machine-learning-services.md) oder in [Big Data-Cluster](../../big-data-cluster/machine-learning-services.md) zu installieren. Die von Ihnen installierten Pakete können verwendet werden, um R-Skripts innerhalb einer Datenbank mithilfe der T-SQL-Anweisung [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) auszuführen.

> [!NOTE]
> Das Paket **sqlmlutils**, das in diesem Artikel beschrieben wird, wird zum Hinzufügen von R-Paketen zu SQL Server 2019 oder höher verwendet. Informationen für SQL Server 2017 und früher finden Sie unter [Installieren von Paketen mit R-Tools](./install-r-packages-standard-tools.md?view=sql-server-2017&preserve-view=true).
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
In diesem Artikel wird beschrieben, wie Sie Funktionen im [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils)-Paket verwenden, um R-Pakete in einer Instanz von [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) zu installieren. Die von Ihnen installierten Pakete können verwendet werden, um R-Skripts innerhalb einer Datenbank mithilfe der T-SQL-Anweisung [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) auszuführen.
::: moniker-end

## <a name="prerequisites"></a>Voraussetzungen

- Installieren Sie [R](https://www.r-project.org) und [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) auf dem Clientcomputer, den Sie mit SQL Server verbinden. Zum Ausführen von Skripts können Sie jede beliebige R-IDE verwenden. In diesem Artikel wird jedoch davon ausgegangen, dass Sie RStudio nutzen.

  Die Version von R auf dem Clientcomputer muss mit der Version von R auf dem Server übereinstimmen, und die Pakete, die Sie installieren, müssen mit Ihrer R-Version kompatibel sein.
  Informationen zu der in jeder SQL Server-Version enthaltenen R-Version finden Sie unter [Python- und R-Pakete](../sql-server-machine-learning-services.md#versions).
  
  Verwenden Sie den folgenden T-SQL-Befehl, um die Version von R auf einer bestimmten SQL Server-Instanz zu überprüfen.

  ```sql
  EXECUTE sp_execute_external_script @language = N'R'
   , @script = N'print(R.version)'
  ```

- Installieren Sie [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) auf dem Clientcomputer, den Sie mit SQL Server verbinden. Sie können auch andere Datenbankverwaltungs- oder Abfragetools verwenden. In diesem Artikel wird jedoch davon ausgegangen, dass Sie Azure Data Studio nutzen.

### <a name="other-considerations"></a>Weitere Überlegungen

- Die Paketinstallation ist für die SQL-Instanz, die Datenbank und den Benutzer spezifisch, die Sie in den Verbindungsinformationen für **sqlmlutils** angeben. Wenn Sie das Paket in mehreren SQL-Instanzen oder Datenbanken oder für verschiedene Benutzer verwenden möchten, müssen Sie das Paket für jede bzw. jeden einzeln installieren. Eine Ausnahme liegt vor, wenn das Paket von einem Mitglied von `dbo` installiert wird, denn dann ist das Paket *öffentlich* und für alle Benutzer freigegeben. Wenn ein Benutzer eine neuere Version eines öffentlichen Pakets installiert, wirkt sich dies nicht auf das öffentliche Paket aus, aber der betreffende Benutzer hat Zugriff auf die neuere Version.

- Ein R-Skript, das in SQL Server ausgeführt wird, kann nur Pakete verwenden, die in der Standardinstanzbibliothek installiert sind. SQL Server kann auch dann keine Pakete aus externen Bibliotheken laden, wenn sich die entsprechenden Bibliotheken auf demselben Computer befinden. Das gilt auch für R-Bibliotheken, die in anderen Microsoft-Produkten installiert sind.

- In einer gesicherten SQL Server-Umgebung sollten Sie Folgendes vermeiden:
  - Pakete, für die Netzwerkzugriff erforderlich ist
  - Pakete, für die erhöhte Zugriffsrechte für das Dateisystem erforderlich sind
  - Pakete, die für die Webentwicklung oder andere Aufgaben verwendet werden und nicht von der Ausführung in SQL Server profitieren

## <a name="install-sqlmlutils-on-the-client-computer"></a>Installieren von sqlmlutils auf dem Clientcomputer

Zur Verwendung von **sqlmlutils** müssen Sie dies zunächst auf dem Clientcomputer installieren, den Sie mit SQL Server verbinden.

Das **sqlmlutils**-Paket ist vom **odbc**-Paket abhängig und das **odbc**-Paket wiederum von einigen anderen Paketen. Mithilfe der folgenden Verfahren werden alle Pakete in der richtigen Reihenfolge installiert.

### <a name="install-sqlmlutils-online"></a>Onlineinstallation von sqlmlutils

Wenn der Clientcomputer Internetzugriff hat, können Sie **sqlmlutils** und die abhängigen Pakete online herunterladen und installieren.

1. Laden Sie die aktuelle **sqlmlutils**-Datei (`.zip` für Windows, `.tar.gz` für Linux) über die Seite https://github.com/Microsoft/sqlmlutils/tree/master/R/dist auf den Clientcomputer herunter. Erweitern Sie die Datei nicht.

1. Öffnen Sie eine **Eingabeaufforderung**, und führen Sie die folgenden Befehle aus, um die Pakete **odbc** und **sqlmlutils** zu installieren. Ersetzen Sie den Pfad zu der **sqlmlutils**-Datei, die Sie heruntergeladen haben. Das **odbc**-Paket können Sie online finden und installieren.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```console
   R.exe -e "install.packages('odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.zip
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```console
   R.exe -e "install.packages('odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.tar.gz
   ```
   ::: moniker-end

### <a name="install-sqlmlutils-offline"></a>Offlineinstallation von sqlmlutils

Wenn der Clientcomputer über keine Internetverbindung verfügt, müssen Sie die Pakete **odbc** und **sqlmlutils** vorab auf einem Computer mit Internetzugang herunterladen. Anschließend können Sie die Dateien in einen Ordner auf dem Clientcomputer kopieren und die Pakete offline installieren.

Das **odbc**-Paket verfügt über zahlreiche abhängige Pakete, sodass sich das Identifizieren aller Abhängigkeiten für ein Paket schwierig gestaltet. Daher sollten Sie [**miniCRAN**](https://andrie.github.io/miniCRAN/) verwenden, um einen lokalen Repositoryordner für das Paket zu erstellen, das alle abhängigen Pakete enthält.
Weitere Informationen finden Sie im Artikel zum Thema [Erstellen eines lokalen R-Paketrepositorys mit miniCRAN](create-a-local-package-repository-using-minicran.md).

Das **sqlmlutils**-Paket besteht aus einer einzelnen Datei, die Sie auf den Clientcomputer kopieren und dort installieren können.

Auf einem Computer mit Internetzugriff:

1. Installieren Sie **miniCRAN**. Weitere Informationen finden Sie unter [Installieren von miniCRAN](create-a-local-package-repository-using-minicran.md#install-minicran).

1. Führen Sie in RStudio das folgende R-Skript aus, um ein lokales Repository für das Paket **odbc** zu erstellen. In diesem Beispiel wird davon ausgegangen, dass das Repository im Ordner `odbc` erstellt wird.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "odbc"
   pkgs_needed <- "odbc"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.5");
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "odbc"
   pkgs_needed <- "odbc"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "source", Rversion = "3.5");
   ```
   ::: moniker-end

   Geben Sie für den `Rversion`-Wert die unter SQL Server installierte Version von R an. Um zu ermitteln, welche Version installiert ist, verwenden Sie den folgenden T-SQL-Befehl.

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(R.version)'
   ```

1. Laden Sie die aktuelle **sqlmlutils**-Datei (`.zip` für Windows, `.tar.gz` für Linux) über die Seite [https://github.com/Microsoft/sqlmlutils/tree/master/R/dist](https://github.com/Microsoft/sqlmlutils/tree/master/R/dist) herunter. Erweitern Sie die Datei nicht.

1. Kopieren Sie den gesamten **odbc**-Repositoryordner und die **sqlmlutils**-Datei auf den Clientcomputer.

Führen Sie auf dem Clientcomputer, den Sie für die Verbindung mit SQL Server verwenden, folgende Schritte durch:

1. Öffnen Sie eine Eingabeaufforderung.

1. Führen Sie die folgenden Befehle aus, um **odbc** und anschließend **sqlmlutils** zu installieren. Ersetzen Sie die vollständigen Pfade zum **odbc**-Repositoryordner und zur **sqlmlutils**-Datei, die Sie auf diesen Computer kopiert haben.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```console
   R.exe -e "install.packages('odbc', repos='odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.zip
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```console
   R.exe -e "install.packages('odbc', repos='odbc')"
   R.exe CMD INSTALL sqlmlutils_1.0.0.tar.gz
   ```
   ::: moniker-end

## <a name="add-an-r-package-on-sql-server"></a>Hinzufügen eines R-Pakets in SQL Server

Im folgenden Beispiel fügen Sie in SQL Server das Paket [**glue**](https://cran.r-project.org/web/packages/glue/) hinzu.

### <a name="add-the-package-online"></a>Hinzufügen des Pakets über eine Internetverbindung

Wenn der Clientcomputer, mit dem Sie sich mit SQL Server verbinden, über einen Internetzugang verfügt, können Sie mit **sqlmlutils** das Paket **glue** und alle Abhängigkeiten über das Internet suchen. Anschließend können Sie das Paket per Remotezugriff in einer SQL Server-Instanz installieren.

1. Öffnen Sie auf dem Clientcomputer RStudio, und erstellen Sie eine neue **R-Skriptdatei**.

1. Verwenden Sie das folgende R-Skript zum Installieren des **glue**-Pakets mithilfe von **sqlmlutils**. Ersetzen Sie die SQL Server-Datenbankverbindungsinformationen durch Ihre eigenen.

   ```R
   library(sqlmlutils)
   connection <- connectionInfo(
     server   = "server",
     database = "database",
     uid      = "username",
     pwd      = "password")

   sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
   ```

   > [!TIP]
   > Für **scope** kann **PUBLIC** oder **PRIVATE** angegeben werden. Ein öffentlicher Bereich (PUBLIC) ist für den Datenbankadministrator zum Installieren von Paketen nützlich, die von allen Benutzern verwendet werden. Pakete in einem privaten Bereich (PRIVATE) sind nur für den Benutzer verfügbar, der sie installiert. Wenn Sie keinen Bereich definieren, wird die Standardeinstellung **PRIVATE** verwendet.

### <a name="add-the-package-offline"></a>Hinzufügen des Pakets ohne Internetverbindung

Wenn der Clientcomputer keine Internetverbindung hat, können Sie das **glue**-Paket mit **miniCRAN** und einem Computer mit Internetzugang herunterladen. Anschließend kopieren Sie das Paket auf den Clientcomputer, auf dem Sie das Paket offline installieren können.
Informationen zum Installieren von **miniCRAN** finden Sie unter [Installieren von miniCRAN](create-a-local-package-repository-using-minicran.md#install-minicran).

Auf einem Computer mit Internetzugriff:

1. Führen Sie das folgende R-Skript aus, um ein lokales Repository für **glue** zu erstellen. In diesem Beispiel wird der Repositoryordner im Verzeichnis `c:\downloads\glue` erstellt.

   ::: moniker range=">=sql-server-ver15||=azuresqldb-mi-current"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/glue"
   pkgs_needed <- "glue"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.5");
   ```
   ::: moniker-end

   ::: moniker range=">=sql-server-linux-ver15"
   ```R
   library("miniCRAN")
   CRAN_mirror <- c(CRAN = "https://cran.microsoft.com")
   local_repo <- "c:/downloads/glue"
   pkgs_needed <- "glue"
   pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);

   makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "source", Rversion = "3.5");
   ```
   ::: moniker-end

   Geben Sie für den `Rversion`-Wert die unter SQL Server installierte Version von R an. Um zu ermitteln, welche Version installiert ist, verwenden Sie den folgenden T-SQL-Befehl.

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(R.version)'
   ```

1. Kopieren Sie den gesamten **glue**-Repositoryordner (`c:\downloads\glue`) auf den Clientcomputer. Kopieren Sie ihn beispielsweise in den Ordner `c:\temp\packages\glue`.

Auf dem Clientcomputer:

1. Öffnen Sie RStudio, und erstellen Sie eine neue **R-Skriptdatei**.

1. Verwenden Sie das folgende R-Skript zum Installieren des **glue**-Pakets mithilfe von **sqlmlutils**. Ersetzen Sie die eigenen Verbindungsdaten der SQL Server-Datenbank. (Wenn Sie keine Windows-Authentifizierung verwenden, fügen Sie die Parameter `uid` und `pwd` hinzu.)

   ```R
   library(sqlmlutils)
   connection <- connectionInfo(
     server= "yourserver",
     database = "yourdatabase")
   localRepo = "c:/temp/packages/glue"

   sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC", repos=paste0("file:///",localRepo))
   ```

   > [!TIP]
   > Für **scope** kann **PUBLIC** oder **PRIVATE** angegeben werden. Ein öffentlicher Bereich (PUBLIC) ist für den Datenbankadministrator zum Installieren von Paketen nützlich, die von allen Benutzern verwendet werden. Pakete in einem privaten Bereich (PRIVATE) sind nur für den Benutzer verfügbar, der sie installiert. Wenn Sie keinen Bereich definieren, wird die Standardeinstellung **PRIVATE** verwendet.

## <a name="use-the-package"></a>Verwenden des Pakets

Nachdem Sie das **glue**-Paket installiert haben, können Sie es in einem R-Skript in SQL Server mit dem T-SQL-Befehl **sp_execute_external_script** verwenden.

1. Öffnen Sie Azure Data Studio, und stellen Sie eine Verbindung mit Ihrer SQL Server-Datenbank her.

1. Führen Sie den folgenden Befehl aus:

   ```sql
   EXECUTE sp_execute_external_script @language = N'R'
       , @script = N'
   library(glue)

   name <- "Fred"
   birthday <- as.Date("2020-06-14")
   text <- glue(''My name is {name} '',
   ''and my birthday is {format(birthday, "%A, %B %d, %Y")}.'')

   print(text)
         ';
   ```

    **Ergebnisse**

    ```text
    My name is Fred and my birthday is Sunday, June 14, 2020.
    ```

## <a name="remove-the-package"></a>Entfernen des Programms

Wenn Sie das **glue**-Paket entfernen möchten, führen Sie das folgende R-Skript aus. Verwenden Sie die zuvor definierte **Verbindungsvariable**.

```R
sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC")
```

## <a name="more-sqlmlutils-functions"></a>Weitere sqlmlutils-Funktionen

Das **sqlmlutils**-Paket enthält zahlreiche Funktionen zum Verwalten von R-Paketen sowie zum Erstellen, Verwalten und Ausführen von gespeicherten Prozeduren und Abfragen in einer SQL Server-Instanz. Weitere Informationen finden Sie in der [README-Datei für das R-Paket sqlmlutils](https://github.com/microsoft/sqlmlutils/tree/master/R).

Weitere Informationen zu **sqlmlutils**-Funktionen erhalten Sie über die R-Funktion **help** oder den **?** -Operator. zusammenfassen. Beispiel:

```R
library(sqlmlutils)
help("sql_install.packages")
```

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu installierten R-Paketen finden Sie unter [Abrufen von Paketinformationen für R](r-package-information.md).
- Unterstützung bei der Verwendung von R-Paketen finden Sie unter [Tipps für die Verwendung von R-Paketen](tips-for-using-r-packages.md).
- Informationen zum Installieren von Python-Paketen finden Sie unter [Installieren von Python-Paketen mit pip](install-additional-python-packages-on-sql-server.md).
- Weitere Informationen zu SQL Server Machine Learning Services finden Sie unter [Was ist SQL Server Machine Learning Services (Python und R)?](../sql-server-machine-learning-services.md)