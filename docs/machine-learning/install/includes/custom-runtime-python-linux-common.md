---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 1303df98c60212c13a233d1e386c60109308e981
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072917"
---
## <a name="custom-installation-of-python"></a>Benutzerdefinierte Installation von Python

> [!NOTE]
> Wenn Sie Python 3.7 am Standardspeicherort von `/usr/lib/python3.7` installiert haben, können Sie diesen Abschnitt überspringen und mit dem Abschnitt [Registrieren der Spracherweiterung](#register-language-extension-linux) fortfahren.

Wenn Sie Ihre eigene Version von Python 3.7 erstellt haben, verwenden Sie die folgenden Befehle, um SQL Server über Ihre benutzerdefinierte Installation zu informieren.

### <a name="add-environment-variable"></a>Hinzufügen einer Umgebungsvariablen

Bearbeiten Sie zunächst den Dienst **mssql-launchpadd**, um der Datei `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` die **PYTHONHOME**-Umgebungsvariable hinzuzufügen.

1. Öffnen Sie die Datei mit systemctl.

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. Fügen Sie den folgenden Text in die Datei `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` ein, die geöffnet wird. Legen Sie den Wert von **PYTHONHOME** auf den benutzerdefinierten Python-Installationspfad fest.

    ```
    [Service]
    Environment="PYTHONHOME=/path/to/installation/of/python3.7"
    ```

1. Speichern Sie die Datei, und schließen Sie den Editor.

Stellen Sie als nächstes sicher, dass `libpython3.7m.so.1.0` geladen werden kann.

1. Erstellen Sie eine benutzerdefinierte Datei „python.conf“ in `/etc/ld.so.conf.d`.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-python.conf
    ```

1. Fügen Sie in der Datei, die geöffnet wird, den Pfad zu **libpython3.7m.so.1.0** aus der benutzerdefinierten Python-Installation hinzu.

    ```
    /path/to/installation/of/python3.7/lib
    ```

1. Speichern Sie die neue Datei, und schließen Sie den Editor.

1. Führen Sie `ldconfig` aus, und vergewissern Sie sich, dass `libpython3.7m.so.1.0` geladen werden kann, indem Sie den folgenden Befehl ausführen und überprüfen, ob die abhängigen Bibliotheken gefunden werden.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/python3.7/lib/libpython3.7m.so.1.0
    ```

### <a name="grant-access-to-python-folder"></a>Gewähren des Zugriffs auf den Python-Ordner

Legen Sie die `datadirectories`-Option im Erweiterbarkeitsabschnitt der Datei `/var/opt/mssql/mssql.conf` auf die benutzerdefinierte Python-Installation fest.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/python3.7
```

### <a name="restart-mssql-launchpadd"></a>Neustart von mssql-launchpadd

Führen Sie den folgenden Befehl aus, um **mssql-launchpadd** neu zu starten:

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="register-language-extension-linux"></a>

## <a name="register-language-extension"></a>Registrieren der Spracherweiterung

Führen Sie die folgenden Schritte aus, um die Python-Spracherweiterung herunterzuladen und zu registrieren, die für die benutzerdefinierte Python-Runtime verwendet wird.

1. Laden Sie die Datei **python-lang-extension-linux-release.zip** aus dem [GitHub-Repository für die SQL Server-Spracherweiterungen](https://github.com/microsoft/sql-server-language-extensions/releases) herunter.

    Alternativ können Sie auch die Debugversion (**python-lang-extension-linux-debug.zip**) in einer Entwicklungs- oder Testumgebung verwenden. Die Debugversion bietet ausführliche Protokollierungsinformationen, um eventuelle Fehler zu untersuchen, und wird nicht für Produktionsumgebungen empfohlen.

1. Verwenden Sie [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md), um eine Verbindung mit Ihrer SQL Server-Instanz herzustellen, und führen Sie den folgenden T-SQL-Befehl aus, um die Python-Spracherweiterung mit [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md) zu registrieren. 

    Ändern Sie den Pfad in dieser Anweisung, um den Speicherort der heruntergeladenen ZIP-Spracherweiterungsdatei (**python-lang-extension-linux-release.zi**) anzugeben.

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-linux-release.zip', FILE_NAME = 'libPythonExtension.so.1.1');
    GO
    ```

    Führen Sie die Anweisung für jede Datenbank aus, in der Sie die Python-Spracherweiterung verwenden möchten.

    > [!NOTE]
    > **Python** ist ein reserviertes Wort und kann nicht als Name für einen neuen externen Sprachnamen verwendet werden. Verwenden Sie stattdessen einen anderen Namen. Die obige Anweisung verwendet z. B. **myPython**.
