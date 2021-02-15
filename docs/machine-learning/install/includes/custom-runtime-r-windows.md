---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 2a156ab122f0eadce71da9f4fcaf9e99584c8e32
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072767"
---
## <a name="prerequisites"></a>Voraussetzungen

Vor der Installation einer benutzerdefinierten R-Laufzeit müssen Sie Folgendes installieren:

+ Wenn Sie eine vorhandene SQL Server-Instanz verwenden, installieren Sie das [kumulative Update 3 oder höher](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) für SQL Server 2019.

## <a name="install-language-extensions"></a>Installieren von Spracherweiterungen

> [!NOTE]
> Wenn Sie [Machine Learning Services](../../sql-server-machine-learning-services.md) unter SQL Server 2019 installiert haben, sind die Spracherweiterungen bereits installiert, und Sie können diesen Schritt überspringen.

Führen Sie die folgenden Schritte aus, um [SQL Server-Spracherweiterungen](../../../language-extensions/language-extensions-overview.md) zu installieren, die für die benutzerdefinierte R-Runtime verwendet werden.

1. Starten Sie den Setup-Assistenten für SQL Server 2019.
  
1. Klicken Sie auf der Registerkarte **Installation** auf **Neue eigenständige SQL Server-Installation oder Hinzufügen von Funktionen zu einer vorhandenen Installation**.

1. Wählen Sie diese Optionen auf der Seite **Funktionsauswahl** aus:
  
    + **Datenbank-Engine-Dienste**
  
        Sie müssen eine Instanz der Datenbank-Engine installieren, um Spracherweiterungen mit SQL Server verwenden zu können. Sie können entweder eine neue oder eine vorhandene Instanz verwenden.
  
    + **Machine Learning-Dienste und -Spracherweiterungen**

        Wählen Sie **Machine Learning-Dienste und -Spracherweiterungen** aus. Wählen Sie nicht R aus, da Sie die benutzerdefinierte R-Runtime später installieren werden.

        :::image type="content" source="../media/2019-setup-language-extensions.png" alt-text="Setup für die SQL Server 2019-Spracherweiterungen":::

1. Stellen Sie auf der Seite **Installationsbereit** sicher, dass die folgenden Auswahlmöglichkeiten aktiviert sind, und klicken Sie auf **Installieren**.
  
    + -Datenbank-Engine-Dienste
    + Machine Learning-Dienste und -Spracherweiterungen

1. Starten Sie nach Abschluss des Setups den Computer neu, wenn Sie dazu aufgefordert werden.

> [!IMPORTANT]
> Wenn Sie eine neue Instanz von SQL Server 2019 mit Spracherweiterungen installieren, installieren Sie das [kumulative Update (CU) 3 oder höher](../../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md), bevor Sie mit dem nächsten Schritt fortfahren.

## <a name="install-r"></a>Installieren von R

Laden Sie die Version von R herunter, die Sie als benutzerdefinierte Runtime verwenden möchten, und installieren Sie sie. Die R-Version 3.3 oder höher wird unterstützt.

1. Laden Sie die [R-Version 3.3 oder höher herunter](https://cran.r-project.org/bin/windows/base/).

1. Führen Sie das R-Setup aus.

1. Notieren Sie sich den Pfad, in dem R installiert ist. In diesem Artikel lautet er zum Beispiel `C:\Program Files\R\R-4.0.3`.

    :::image type="content" source="../media/r-setup-path.png" alt-text="R-Setup":::

## <a name="update-system-environment-variable"></a>Aktualisieren der Systemumgebungsvariable

Führen Sie die folgenden Schritte aus, um die **PATH**-Umgebungsvariablen des Systems zu ändern.

1. Suchen Sie im Windows-Suchfeld nach der Option **Systemumgebungsvariablen bearbeiten**, und öffnen Sie sie.

1. Wählen Sie unter **Erweitert** **Umgebungsvariablen** aus.

1. Ändern Sie die **PATH**-Umgebungsvariable des Systems.

    Wählen Sie **PATH** aus, und klicken Sie auf **Bearbeiten**.

    Wählen Sie **Neu** aus, und fügen Sie dem Pfad zum `\bin\x64`-Ordner in Ihrem R-Installationspfad hinzu. Beispiel: `C:\Program Files\R\R-4.0.3\bin\x64`.

## <a name="install-rcpp-package"></a>Installieren des Rcpp-Pakets

Führen Sie diese Schritte aus, um das **Rcpp**-Paket zu installieren.

1. Starten Sie eine Eingabeaufforderung *mit erhöhten Rechten* (Als Administrator ausführen).

1. Starten Sie R über die Eingabeaufforderung. Führen Sie `\bin\R.exe` im Ordner in Ihrem R-Installationspfad aus. Beispiel: `C:\Program Files\R\R-4.0.3\bin\R.exe`.

    ```CMD
    "C:\Program Files\R\R-4.0.3\bin\R.exe"
    ```

1. Führen Sie das folgende Skript aus, um das Rcpp-Paket im Ordner `\library` in Ihrem R-Installationspfad zu installieren. Beispiel: `C:\Program Files\R\R-4.0.3\library`.

    ```R
    install.packages("Rcpp", lib="C:\Program Files\R\R-4.0.3\library");
    ```

## <a name="grant-access-to-r-folder"></a>Gewähren des Zugriffs auf den R-Ordner

> [!NOTE]
> Wenn Sie R am Standardspeicherort `C:\Program Files\R\R-version` (zum Beispiel `C:\Program Files\R\R-4.0.3`) installiert haben, können Sie diesen Schritt überspringen.

Führen Sie die folgenden **icacls**-Befehle in einer neuen Eingabeaufforderung *mit erhöhten Rechten* aus, um **Benutzername des SQL Server-Launchpad-Diensts** und SID **S-1-15-2-1** (**ALL_APPLICATION_PACKAGES**) **LESE- und AUSFÜHRUNGSZUGRIFF** zu gewähren. Der Benutzername des Launchpad-Diensts weist die Form `NT Service\MSSQLLAUNCHPAD$INSTANCENAME` auf, wobei `INSTANCENAME` der Instanzname Ihrer SQL Server-Instanz ist.

Die Befehle gewähren rekursiv Zugriff auf alle Dateien und Ordner unter dem angegebenen Verzeichnispfad.

1. Erteilen Sie für Ihren R-Installationspfad die Berechtigungen für **Benutzername des SQL Server-Launchpad Diensts**. Beispiel: `C:\Program Files\R\R-4.0.3`.

    ```cmd
    icacls "C:\Program Files\R\R-4.0.3" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    Bei einer benannter Instanz ist der Befehl `icacls "C:\Program Files\R\R-4.0.3" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T` für eine Instanz namens **SQL01**.

2. Erteilen Sie die Berechtigungen für **SID S-1-15-2-1** für Ihren R-Installationspfad. Beispiel: `C:\Program Files\R\R-4.0.3`.

    ```cmd
    icacls "C:\Program Files\R\R-4.0.3" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    Der vorangehende Befehl erteilt Berechtigungen für die Computer-SID **S-1-15-2-1**, was **ALLEN ANWENDUNGSPAKETEN** auf einer englischen Version von Windows entspricht. Alternativ können Sie `icacls "C:\Program Files\R\R-4.0.3" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` in einer englischen Version von Windows verwenden.

## <a name="restart-sql-server-launchpad"></a>Neustart von SQL Server-Launchpad

Führen Sie die folgenden Schritte aus, um den SQL Server-Launchpad-Dienst neu zu starten.

1. Öffnen Sie den [SQL Server-Konfigurations-Manager](../../../relational-databases/sql-server-configuration-manager.md).

1. Klicken Sie unter **SQL Server-Dienste** mit der rechten Maustaste auf **SQL Server-Launchpad (MSSQLSERVER)** , und wählen Sie **Neu starten** aus. Wenn Sie eine benannte Instanz verwenden, wird der Instanzname anstelle von **(MSSQLSERVER)** angezeigt.

## <a name="register-language-extension"></a>Registrieren der Spracherweiterung

Führen Sie die folgenden Schritte aus, um die R-Spracherweiterung herunterzuladen und zu registrieren, die für die benutzerdefinierte R-Runtime verwendet wird.

1. Laden Sie die Datei **R-lang-extension-windows-release.zip** aus dem [GitHub-Repository für die SQL Server-Spracherweiterungen](https://github.com/microsoft/sql-server-language-extensions/releases) herunter.

    Alternativ können Sie auch die Debugversion (**R-lang-extension-windows-debug.zip**) in einer Entwicklungs- oder Testumgebung verwenden. Die Debugversion bietet ausführliche Protokollierungsinformationen, um eventuelle Fehler zu untersuchen, und wird nicht für Produktionsumgebungen empfohlen.

1. Verwenden Sie [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md), um eine Verbindung mit Ihrer SQL Server-Instanz herzustellen, und führen Sie den folgenden T-SQL-Befehl aus, um die R-Spracherweiterung mit [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md) zu registrieren.

    Ändern Sie den Pfad in dieser Anweisung, um den Speicherort der heruntergeladenen ZIP-Spracherweiterungsdatei (**R-lang-extension-windows**) und den Speicherort Ihrer R-Installation (`C:\\Program Files\\R\\R-4.0.3`) anzugeben.

    ```sql
    CREATE EXTERNAL LANGUAGE [myR]
    FROM (CONTENT = N'C:\path\to\R-lang-extension-windows-release.zip', 
        FILE_NAME = 'libRExtension.dll',
        ENVIRONMENT_VARIABLES = N'{"R_HOME": "C:\\Program Files\\R\\R-4.0.3"}'););
    GO
    ```

    Führen Sie die Anweisung für jede Datenbank aus, in der Sie die R-Spracherweiterung verwenden möchten.

    > [!NOTE]
    > **R** ist ein reserviertes Wort und kann nicht als Name für einen neuen externen Sprachnamen verwendet werden. Verwenden Sie stattdessen einen anderen Namen. Die obige Anweisung verwendet z. B. **myR**.
