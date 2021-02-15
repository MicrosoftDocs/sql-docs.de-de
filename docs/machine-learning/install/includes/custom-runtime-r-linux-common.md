---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 07bd68eaee05893174813ed43dc5358104016e4b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072778"
---
## <a name="custom-installation-of-r"></a>Benutzerdefinierte Installation von R

> [!NOTE]
> Wenn Sie R am Standardspeicherort von **/usr/lib/R** installiert haben, können Sie diesen Abschnitt überspringen und mit dem Abschnitt [Installieren des Rcpp-Pakets](#install-rcpp-package-linux) fortfahren.

### <a name="update-the-environment-variables"></a>Aktualisieren der Umgebungsvariablen

Bearbeiten Sie zunächst den Dienst **mssql-launchpadd**, um der Datei `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` die **R_HOME**-Umgebungsvariable hinzuzufügen.

1. Öffnen Sie die Datei mit systemctl.

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. Fügen Sie den folgenden Text in die Datei `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` ein, die geöffnet wird. Legen Sie den Wert **R_HOME** auf den benutzerdefinierten R-Installationspfad fest.

    ```text
    [Service]
    Environment="R_HOME=/path/to/installation/of/R"
    ```

1. Speichern und schließen Sie die Datei.

Stellen Sie als nächstes sicher, dass `libR.so` geladen werden kann.

1. Erstellen Sie eine Datei „custom-r.conf“ in **/etc/ld.so.conf.d**.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-r.conf
    ```

1. Fügen Sie in der Datei, die geöffnet wird, den Pfad zu **libR.so** aus der benutzerdefinierten R-Installation hinzu.

    ```
    /path/to/installation/of/R/lib
    ```

1. Speichern Sie die neue Datei, und schließen Sie den Editor.

1. Führen Sie `ldconfig` aus, und vergewissern Sie sich, dass **libR.so** geladen werden kann, indem Sie den folgenden Befehl ausführen und überprüfen, ob alle abhängigen Bibliotheken gefunden werden.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/R/lib/libR.so
    ```

### <a name="grant-access-to-the-custom-r-installation-folder"></a>Gewähren des Zugriffs auf den benutzerdefinierten R-Installationsordner

Legen Sie die `datadirectories`-Option im Erweiterbarkeitsabschnitt der Datei `/var/opt/mssql/mssql.conf` auf die benutzerdefinierte R-Installation fest.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/R
```

### <a name="restart-mssql-launchpadd-service"></a>Neustarten des mssql-launchpadd-Diensts

Führen Sie den folgenden Befehl aus, um **mssql-launchpadd** neu zu starten:

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="install-rcpp-package-linux"></a>

## <a name="install-rcpp-package"></a>Installieren des Rcpp-Pakets

Führen Sie diese Schritte aus, um das **Rcpp**-Paket zu installieren.

1. Starten Sie R von einer Shell:

    ```bash
    sudo ${R_HOME}/bin/R
    ```

1. Führen Sie das folgende Skript aus, um das Rcpp-Paket im Ordner „${R_HOME}\library“ zu installieren.

  ```R
  install.packages("Rcpp", lib = "${R_HOME}/library");
  ```

## <a name="register-language-extension"></a>Registrieren der Spracherweiterung

Führen Sie die folgenden Schritte aus, um die R-Spracherweiterung herunterzuladen und zu registrieren, die für die benutzerdefinierte R-Runtime verwendet wird.

1. Laden Sie die Datei **R-lang-extension-linux.zip** aus dem [GitHub-Repository für die SQL Server-Spracherweiterungen](https://github.com/microsoft/sql-server-language-extensions/releases) herunter.

    Alternativ können Sie auch die Debugversion (**R-lang-extension-linux-debug.zip**) in einer Entwicklungs- oder Testumgebung verwenden. Die Debugversion bietet ausführliche Protokollierungsinformationen, um eventuelle Fehler zu untersuchen, und wird nicht für Produktionsumgebungen empfohlen.

1. Verwenden Sie [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md), um eine Verbindung mit Ihrer SQL Server-Instanz herzustellen, und führen Sie den folgenden T-SQL-Befehl aus, um die R-Spracherweiterung mit [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md) zu registrieren. 

    Ändern Sie den Pfad in dieser Anweisung, um den Speicherort der heruntergeladenen ZIP-Spracherweiterungsdatei (**R-lang-extension-linux.zip**) anzugeben.

    ```sql
    CREATE EXTERNAL LANGUAGE [myR]
    FROM (CONTENT = N'/path/to/R-lang-extension-linux-release.zip', FILE_NAME = 'libRExtension.so.1.0');
    GO
    ```

    Führen Sie die Anweisung für jede Datenbank aus, in der Sie die R-Spracherweiterung verwenden möchten.

    > [!NOTE]
    > **R** ist ein reserviertes Wort und kann nicht als Name für einen neuen externen Sprachnamen verwendet werden. Verwenden Sie stattdessen einen anderen Namen. Die obige Anweisung verwendet z. B. **myR**.
