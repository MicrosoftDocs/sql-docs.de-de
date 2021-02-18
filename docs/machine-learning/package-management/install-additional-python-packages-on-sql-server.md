---
title: Installieren von Python-Paketen mit sqlmlutils
description: Erfahren Sie, wie Sie mit Python-pip neue Python-Pakete auf einer Instanz von SQL Server Machine Learning Services installieren.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/26/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 9468ec6b2a196e2fa75935fb603385faf9f2a958
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273041"
---
# <a name="install-python-packages-with-sqlmlutils"></a>Installieren von Python-Paketen mit sqlmlutils

[!INCLUDE [SQL Server 2019 SQL MI](../../includes/applies-to-version/sqlserver2019-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In diesem Artikel wird beschrieben, wie Sie Funktionen im [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils)-Paket verwenden, um neue Python-Pakete in einer Instanz von [Machine Learning Services in SQL Server](../sql-server-machine-learning-services.md) oder [Big Data-Cluster](../../big-data-cluster/machine-learning-services.md) zu installieren. Die von Ihnen installierten Pakete können verwendet werden, um Python-Skripts innerhalb einer Datenbank mithilfe der T-SQL-Anweisung [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) auszuführen.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
In diesem Artikel wird beschrieben, wie Sie Funktionen im [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils)-Paket verwenden, um neue Python-Pakete in einer Instanz von [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) zu installieren. Die von Ihnen installierten Pakete können verwendet werden, um Python-Skripts innerhalb einer Datenbank mithilfe der T-SQL-Anweisung [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) auszuführen.
::: moniker-end

Weitere Informationen zu dem Paketspeicherort und Installationspfaden finden Sie unter [Abrufen von Paketinformationen für Python](../package-management/python-package-information.md).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Das Paket **sqlmlutils**, das in diesem Artikel beschrieben wird, wird zum Hinzufügen von Python-Paketen zu SQL Server 2019 oder höher verwendet. Informationen zu SQL Server 2017 und früher finden Sie unter [Installieren von Paketen mit Python-Tools](./install-python-packages-standard-tools.md?view=sql-server-2017&preserve-view=true).
::: moniker-end

## <a name="prerequisites"></a>Voraussetzungen

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
+ [SQL Server Machine Learning Services](../install/sql-machine-learning-services-windows-install.md) muss mit der Python-Sprachoption installiert sein.
::: moniker-end

+ Installieren Sie [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) auf dem Clientcomputer, den Sie mit SQL Server verbinden. Sie können auch andere Datenbankverwaltungs- oder Abfragetools verwenden. In diesem Artikel wird jedoch davon ausgegangen, dass Sie Azure Data Studio nutzen.

+ Installieren Sie den Python-Kernel in Azure Data Studio. Sie können Python auch über die Befehlszeile installieren und verwenden. Außerdem können Sie eine alternative Python-Entwicklungsumgebung wie [Visual Studio Code](https://code.visualstudio.com/download) mit der [Python-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-python.python) verwenden.

  Die Version von Python auf dem Clientcomputer muss mit der Python-Version auf dem Server übereinstimmen. Darüber hinaus müssen die Pakete, die Sie installieren, mit Ihrer Python-Version konform sein.
  Informationen zu der in jeder SQL Server-Version enthaltenen Python-Version finden Sie unter [Python- und R-Versionen](../sql-server-machine-learning-services.md#versions).
  
  Verwenden Sie den folgenden T-SQL-Befehl, um die Version von Python einer bestimmten SQL Server-Instanz zu überprüfen.

  ```sql
  EXECUTE sp_execute_external_script
    @language = N'Python',
    @script = N'
  import sys
  print(sys.version)
  '
  ```

### <a name="other-considerations"></a>Weitere Überlegungen

+ Die Python-Paketbibliothek befindet sich im Ordner „Programme“ Ihrer SQL Server-Instanz, und für die Installation in diesem Ordner sind standardmäßig Administratorberechtigungen erforderlich. Weitere Informationen finden Sie unter [Speicherort der Paketbibliothek](../package-management/python-package-information.md#default-python-library-location).

+ Die Paketinstallation ist für die SQL-Instanz, die Datenbank und den Benutzer spezifisch, die Sie in den Verbindungsinformationen für **sqlmlutils** angeben. Wenn Sie das Paket in mehreren SQL-Instanzen oder Datenbanken oder für verschiedene Benutzer verwenden möchten, müssen Sie das Paket für jede bzw. jeden einzeln installieren. Eine Ausnahme liegt vor, wenn das Paket von einem Mitglied von `dbo` installiert wird, denn dann ist das Paket *öffentlich* und für alle Benutzer freigegeben. Wenn ein Benutzer eine neuere Version eines öffentlichen Pakets installiert, wirkt sich dies nicht auf das öffentliche Paket aus, aber der betreffende Benutzer hat Zugriff auf die neuere Version.

+ Bevor Sie ein Paket hinzufügen, sollten Sie überprüfen, ob das Paket für die SQL Server-Umgebung geeignet ist.

  + Es wird empfohlen, Python innerhalb der Datenbank für Tasks zu verwenden, die von einer engen Integration in die Datenbank-Engine profitieren, z. B. maschinelles Lernen, anstatt von Tasks, die einfache Datenbankabfragen ausführen.

  + Wenn Sie Pakete hinzufügen, deren Ausführung auf dem Server sehr rechenintensiv ist, wird die Leistung beeinträchtigt.

  + In einer gesicherten SQL Server-Umgebung sollten Sie Folgendes vermeiden:
    + Pakete, für die Netzwerkzugriff erforderlich ist
    + Pakete, für die erhöhte Zugriffsrechte für das Dateisystem erforderlich sind
    + Pakete, die für die Webentwicklung oder andere Aufgaben verwendet werden und nicht von der Ausführung in SQL Server profitieren

  + Das Python-Paket **tensorflow** kann nicht mithilfe von sqlmlutils installiert werden. Weitere Informationen und eine Problemumgehung finden Sie unter [Bekannte Probleme in SQL Server-Machine Learning Services](../troubleshooting/known-issues-for-sql-server-machine-learning-services.md#9-cannot-install-tensorflow-package-using-sqlmlutils).

## <a name="install-sqlmlutils-on-the-client-computer"></a>Installieren von sqlmlutils auf dem Clientcomputer

Zur Verwendung von **sqlmlutils** müssen Sie dies zunächst auf dem Clientcomputer installieren, den Sie mit SQL Server verbinden.

### <a name="in-azure-data-studio"></a>In Azure Data Studio

Wenn Sie **sqlmlutils** in Azure Data Studio verwenden, können Sie es mithilfe des Paket-Manager-Features in einem Python-Kernelnotebook installieren.

1. Klicken Sie in einem [Python-Kernelnotebook in Azure Data Studio](../../azure-data-studio/notebooks/notebooks-python-kernel.md) auf **Pakete verwalten**.
1. Klicken Sie auf **Neues hinzufügen**.
1. Geben Sie „sqlmlutils“ in das Feld **Search Pip packages** (pip-Pakete suchen) ein, und klicken Sie auf **Suchen**.
1. Wählen Sie die **Paketversion** aus, die Sie installieren möchten (neue Version wird empfohlen).
1. Klicken Sie auf **Installieren** und dann auf **Schließen**.

### <a name="from-python-command-line"></a>Über die Python-Befehlszeile

Wenn Sie **sqlmlutils** über eine Python-Eingabeaufforderung oder IDE verwenden, können Sie sqlmlutils mit einem einfachen **pip**-Befehl installieren:

```console
pip install sqlmlutils
```

Sie können **sqlmlutils** auch aus einer ZIP-Datei installieren:

1. Vergewissern Sie sich, dass Sie **pip** installiert haben. Weitere Informationen finden Sie unter [pip-Installation](https://pip.pypa.io/en/stable/installing/).
1. Laden Sie die neueste **sqlmlutils**-ZIP-Datei von https://github.com/Microsoft/sqlmlutils/tree/master/Python/dist auf den Clientcomputer herunter. Entpacken Sie die Datei jedoch nicht.
1. Öffnen Sie eine **Eingabeaufforderung**, und führen Sie die folgenden Befehle aus, um das Paket **sqlmlutils** zu installieren. Ersetzen Sie den vollständigen Pfad zur **sqlmlutils**-ZIP-Datei, die Sie heruntergeladen haben. In diesem Beispiel wird davon ausgegangen, dass dies die heruntergeladene Datei ist: `c:\temp\sqlmlutils-1.0.0.zip`.
   ```console
   pip install --upgrade --upgrade-strategy only-if-needed c:\temp\sqlmlutils-1.0.0.zip
   ```

## <a name="add-a-python-package-on-sql-server"></a>Hinzufügen eines Python-Pakets in SQL Server

Mithilfe von **sqlmlutils** können Sie Python-Pakete zu einer SQL-Instanz hinzufügen. Anschließend können Sie diese Pakete in Ihrem Python-Code verwenden, der in der SQL-Instanz ausgeführt wird. **sqlmlutils** verwendet [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md), um das Paket und jede seiner Abhängigkeiten zu installieren.

Im folgenden Beispiel fügen Sie in SQL Server das Paket [text-tools](https://pypi.org/project/text-tools/) hinzu.

### <a name="add-the-package-online"></a>Hinzufügen des Pakets über eine Internetverbindung

Wenn der Clientcomputer, mit dem Sie sich mit SQL Server verbinden, über einen Internetzugang verfügt, können Sie mit **sqlmlutils** das Paket **text-tools** und alle Abhängigkeiten über das Internet suchen. Anschließend können Sie das Paket per Remotezugriff in einer SQL Server-Instanz installieren.

::: moniker range=">=sql-server-ver15"

1. Öffnen Sie auf dem Clientcomputer **Python** oder eine Python-Umgebung.

1. Verwenden Sie die folgenden Befehle, um das Paket **text-tools** zu installieren. Ersetzen Sie die eigenen Verbindungsdaten der SQL Server-Datenbank. (Wenn Sie eine Windows-Authentifizierung verwenden, müssen Sie die Parameter `uid` und `pwd` nicht hinzufügen.)

::: moniker-end

::: moniker range=">=sql-server-linux-ver15||=azuresqldb-mi-current"

1. Öffnen Sie auf dem Clientcomputer **Python** oder eine Python-Umgebung.

1. Verwenden Sie die folgenden Befehle, um das Paket **text-tools** zu installieren. Ersetzen Sie die SQL Server-Datenbankverbindungsinformationen durch Ihre eigenen.

::: moniker-end

   ```python
   import sqlmlutils
   connection = sqlmlutils.ConnectionInfo(server="server", database="database", uid="username", pwd="password")
   sqlmlutils.SQLPackageManager(connection).install("text-tools")
   ```

### <a name="add-the-package-offline"></a>Hinzufügen des Pakets ohne Internetverbindung

Wenn der Clientcomputer, mit dem Sie sich mit SQL Server verbinden, über keine Internetverbindung verfügt, können Sie mit **pip** auf einem Computer mit Internetzugang das Paket und alle abhängigen Pakete in einen lokalen Ordner herunterladen. Anschließend kopieren Sie den Ordner auf den Clientcomputer, auf dem Sie das Paket offline installieren können.

#### <a name="on-a-computer-with-internet-access"></a>Auf einem Computer mit Internetzugriff

1. Öffnen Sie eine **Eingabeaufforderung**, und führen Sie den folgenden Befehl aus, um einen lokalen Ordner zu erstellen, der das Paket **text-tools** enthält. In diesem Beispiel wird der Ordner `c:\temp\text-tools` erstellt.

   ```console
   pip download text-tools -d c:\temp\text-tools
   ```

1. Kopieren Sie den `text-tools`-Ordner auf den Clientcomputer. Im folgenden Beispiel wird davon ausgegangen, dass Sie ihn nach `c:\temp\packages\text-tools` kopiert haben.

#### <a name="on-the-client-computer"></a>Auf dem Clientcomputer

Verwenden Sie **sqlmlutils**, um jedes Paket (WHL-Datei) aus dem lokalen Ordner zu installieren, der von **pip** erstellt wurde. Es spielt keine Rolle, in welcher Reihenfolge Sie die Pakete installieren.

In diesem Beispiel weist **text-tools** keine Abhängigkeiten auf, sodass es nur eine Datei aus dem Ordner `text-tools` gibt, die Sie installieren können. Im Gegensatz dazu hat ein Paket wie **scikit-plot** elf Abhängigkeiten, sodass Sie zwölf Dateien im Ordner finden würden (das Paket **scikit-plot** und die elf abhängigen Pakete), die Sie jeweils installieren würden.

::: moniker range=">=sql-server-ver15"

Führen Sie folgendes Python-Skript aus. Ersetzen Sie den tatsächlichen Dateipfad und Paketnamen sowie die eigenen Verbindungsdaten der SQL Server-Datenbank. (Wenn Sie eine Windows-Authentifizierung verwenden, müssen Sie die Parameter `uid` und `pwd` nicht hinzufügen.) Wiederholen Sie die `sqlmlutils.SQLPackageManager`-Anweisung für jede Paketdatei im Ordner.

::: moniker-end

::: moniker range=">=sql-server-linux-ver15||=azuresqldb-mi-current"

Führen Sie folgendes Python-Skript aus. Ersetzen Sie den tatsächlichen Dateipfad und -namen des Pakets sowie die Verbindungsinformationen Ihrer eigenen SQL Server-Datenbank. Wiederholen Sie die `sqlmlutils.SQLPackageManager`-Anweisung für jede Paketdatei im Ordner.

::: moniker-end

```python
import sqlmlutils
connection = sqlmlutils.ConnectionInfo(server="yourserver", database="yourdatabase", uid="username", pwd="password"))
sqlmlutils.SQLPackageManager(connection).install("text_tools-1.0.0-py3-none-any.whl")
```

## <a name="use-the-package"></a>Verwenden des Pakets

Sie können das Paket jetzt in einem Python-Skript in SQL Server verwenden. Beispiel:

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
from text_tools.finders import find_best_string
corpus = "Lorem Ipsum text"
query = "Ipsum"
first_match = find_best_string(query, corpus)
print(first_match)
  '
```

## <a name="remove-the-package-from-sql-server"></a>Entfernen eines Pakets aus SQL Server

Wenn Sie das Paket **text-tools** entfernen möchten, verwenden Sie den folgenden Python-Befehl auf dem Clientcomputer und die Verbindungsvariablen, die Sie zuvor definiert haben.

```python
sqlmlutils.SQLPackageManager(connection).uninstall("text-tools")
```

## <a name="more-sqlmlutils-functions"></a>Weitere sqlmlutils-Funktionen

Das **sqlmlutils**-Paket enthält zahlreiche Funktionen zum Verwalten von Python-Paketen sowie zum Erstellen, Verwalten und Ausführen gespeicherter Prozeduren und Abfragen in einer SQL Server-Instanz. Ausführliche Informationen finden Sie in der [README-Datei für das sqlmlutils-Paket für Python](https://github.com/microsoft/sqlmlutils/tree/master/Python).

Weitere Informationen zu **sqlmlutils**-Funktionen erhalten Sie über die Python-Funktion **help**. Beispiel:

```Python
import sqlmlutils
help(SQLPackageManager.install)
```

## <a name="next-steps"></a>Nächste Schritte

+ Informationen zu Python-Paketen, die in SQL Server Machine Learning Services installiert sind, finden Sie unter [Abrufen von Paketinformationen für Python](../package-management/python-package-information.md).

+ Informationen zum Installieren von R-Paketen in SQL Server Machine Learning Services finden Sie unter [Installieren neuer R-Pakete auf SQL Server](install-additional-r-packages-on-sql-server.md).
