---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 9f58ddf6b58e32d40b2962b47255aba2e553acca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072953"
---
## <a name="prerequisites"></a>Voraussetzungen

Vor der Installation einer benutzerdefinierten Python-Laufzeit müssen Sie Folgendes installieren:

+ Wenn Sie eine vorhandene SQL Server-Instanz verwenden, installieren Sie das [kumulative Update 3 oder höher](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) für SQL Server 2019.

## <a name="install-language-extensions"></a>Installieren von Spracherweiterungen

> [!NOTE]
> Wenn Sie [Machine Learning Services](../../sql-server-machine-learning-services.md) unter SQL Server 2019 installiert haben, sind die Spracherweiterungen bereits installiert, und Sie können diesen Schritt überspringen.

Führen Sie die folgenden Schritte aus, um [SQL Server-Spracherweiterungen](../../../language-extensions/language-extensions-overview.md) zu installieren, die für die benutzerdefinierte Python-Runtime verwendet werden.

1. Starten Sie den Setup-Assistenten für SQL Server 2019.
  
1. Klicken Sie auf der Registerkarte **Installation** auf **Neue eigenständige SQL Server-Installation oder Hinzufügen von Funktionen zu einer vorhandenen Installation**.

1. Wählen Sie diese Optionen auf der Seite **Funktionsauswahl** aus:
  
    + **Datenbank-Engine-Dienste**
  
        Sie müssen eine Instanz der Datenbank-Engine installieren, um Spracherweiterungen mit SQL Server verwenden zu können. Sie können entweder eine neue oder eine vorhandene Instanz verwenden.
  
    + **Machine Learning-Dienste und -Spracherweiterungen**

        Wählen Sie **Machine Learning-Dienste und -Spracherweiterungen** aus. Wählen Sie nicht Python aus, da Sie die benutzerdefinierte Python-Runtime später installieren werden.

        :::image type="content" source="../media/2019-setup-language-extensions.png" alt-text="Setup für die SQL Server 2019-Spracherweiterungen":::

1. Stellen Sie auf der Seite **Installationsbereit** sicher, dass die folgenden Auswahlmöglichkeiten aktiviert sind, und klicken Sie auf **Installieren**.
  
    + -Datenbank-Engine-Dienste
    + Machine Learning-Dienste und -Spracherweiterungen

1. Starten Sie nach Abschluss des Setups den Computer neu, wenn Sie dazu aufgefordert werden.

> [!IMPORTANT]
> Wenn Sie eine neue Instanz von SQL Server 2019 mit Spracherweiterungen installieren, installieren Sie das [kumulative Update (CU) 3 oder höher](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md), bevor Sie mit dem nächsten Schritt fortfahren.

## <a name="install-python"></a>Installieren von Python

Die für die benutzerdefinierte Python-Runtime verwendete Python-Spracherweiterung unterstützt derzeit nur Python 3.7. Wenn Sie eine andere Version von Python verwenden möchten, folgen Sie den Anweisungen im [GitHub-Repository für die Python-Spracherweiterung](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python), um die Erweiterung zu ändern und neu zu erstellen.

1. Laden Sie [Python 3.7](https://www.python.org/downloads/windows/) für Windows herunter, und führen Sie das Setup auf dem Server aus.

1. Wählen Sie **Add Python 3.7 to PATH** (Python 3.7 zu PATH hinzufügen) und dann **Installation anpassen** aus.

    :::image type="content" source="../media/python-install-add-to-path.png" alt-text="Python 3.7-Installation: Python 3.7 zu PATH hinzufügen":::

1. Behalten Sie unter **Optionale Features** die Standardeinstellungen bei, und wählen Sie **Weiter** aus.

1. Wählen Sie **Für alle Benutzer installieren** aus, und merken Sie sich den Installationsort.

    :::image type="content" source="../media/python-install-for-all-users.png" alt-text="Python 3.7-Installation: Für alle Benutzer installieren":::

1. Wählen Sie **Installieren** aus.

## <a name="install-pandas"></a>Installieren von Pandas

Installieren Sie das [Pandas](https://pandas.pydata.org/)-Paket für Python über eine Eingabeaufforderung mit *erhöhten Rechten* (Als Administrator ausführen):

```bash
python.exe -m pip install pandas
```

## <a name="grant-access-to-python-folder"></a>Gewähren des Zugriffs auf den Python-Ordner

Führen Sie die folgenden **icacls**-Befehle in einer neuen Eingabeaufforderung mit *erhöhten Rechten* aus, um Zugriff auf den **SQL Server-Launchpad-Dienst** und SID **S-1-15-2-1** (**ALL_APPLICATION_PACKAGES**) **LESE- und AUSFÜHRUNGSZUGRIFF** auf dem Python-Installationsspeicherort zu gewähren.

In den folgenden Beispielen wird der Python-Installationsspeicherort als `C:\Program Files\Python37`verwendet. Wenn sich Ihr Speicherort unterscheidet, dann ändern Sie ihn im Befehl.

1. Erteilen Sie Berechtigungen für **Benutzername des SQL Server-Launchpad Diensts**.

    ```cmd
    icacls "C:\Program Files\Python37" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    Bei einer benannter Instanz ist der Befehl `icacls "C:\Program Files\Python37" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T` für eine Instanz namens **SQL01**.

2. Erteilen Sie die Berechtigung für **SID S-1-15-2-1**.

    ```cmd
    icacls "C:\Program Files\Python37" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    Der vorangehende Befehl erteilt Berechtigungen für die Computer-SID **S-1-15-2-1**, was **ALLEN ANWENDUNGSPAKETEN** auf einer englischen Version von Windows entspricht. Alternativ können Sie `icacls "C:\Program Files\Python37" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` in einer englischen Version von Windows verwenden.

## <a name="restart-sql-server-launchpad"></a>Neustart von SQL Server-Launchpad

Führen Sie die folgenden Schritte aus, um den SQL Server-Launchpad-Dienst neu zu starten.

1. Öffnen Sie den [SQL Server-Konfigurations-Manager](../../../relational-databases/sql-server-configuration-manager.md).

1. Klicken Sie unter **SQL Server-Dienste** mit der rechten Maustaste auf **SQL Server-Launchpad (MSSQLSERVER)** , und wählen Sie **Neu starten** aus. Wenn Sie eine benannte Instanz verwenden, wird der Instanzname anstelle von **(MSSQLSERVER)** angezeigt.

## <a name="register-language-extension"></a>Registrieren der Spracherweiterung

Führen Sie die folgenden Schritte aus, um die Python-Spracherweiterung herunterzuladen und zu registrieren, die für die benutzerdefinierte Python-Runtime verwendet wird.

1. Laden Sie die Datei **python-lang-extension-windows-release.zip** aus dem [GitHub-Repository für die SQL Server-Spracherweiterungen](https://github.com/microsoft/sql-server-language-extensions/releases) herunter.

    Alternativ können Sie auch die Debugversion (**python-lang-extension-windows-debug.zip**) in einer Entwicklungs- oder Testumgebung verwenden. Die Debugversion bietet ausführliche Protokollierungsinformationen, um eventuelle Fehler zu untersuchen, und wird nicht für Produktionsumgebungen empfohlen.

1. Verwenden Sie [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md), um eine Verbindung mit Ihrer SQL Server-Instanz herzustellen, und führen Sie den folgenden T-SQL-Befehl aus, um die Python-Spracherweiterung mit [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md) zu registrieren.

    Ändern Sie den Pfad in dieser Anweisung, um den Speicherort der heruntergeladenen ZIP-Spracherweiterungsdatei (**python-lang-extension-windows**) und den Speicherort Ihrer Python-Installation (`C:\\Program Files\\Python3.7`) anzugeben.

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'C:\path\to\python-lang-extension-windows-release.zip', 
        FILE_NAME = 'pythonextension.dll', 
        ENVIRONMENT_VARIABLES = N'{"PYTHONHOME": "C:\\Program Files\\Python3.7"}');
    GO
    ```

    Führen Sie die Anweisung für jede Datenbank aus, in der Sie die Python-Spracherweiterung verwenden möchten.

    > [!NOTE]
    > **Python** ist ein reserviertes Wort und kann nicht als Name für einen neuen externen Sprachnamen verwendet werden. Verwenden Sie stattdessen einen anderen Namen. Die obige Anweisung verwendet z. B. **myPython**.
