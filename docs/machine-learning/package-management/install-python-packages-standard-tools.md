---
title: Installieren von Paketen mit Python-Tools
description: Erfahren Sie, wie Sie mit Python-Standardtools neue Python-Pakete in einer Instanz von SQL Server Machine Learning Services installieren.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/21/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2017
ms.openlocfilehash: 37d7f95f4140e0cab778ac4864757fbe75b1ff0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082631"
---
# <a name="install-packages-with-python-tools-on-sql-server"></a>Installieren von Paketen mit Python-Tools in SQL Server
[!INCLUDE [SQL Server 2017 only](../../includes/applies-to-version/sqlserver2017-only.md)]

In diesem Artikel erfahren Sie, wie Sie mit Python-Standardtools neue Python-Pakete in einer Instanz von SQL Server Machine Learning Services installieren. Im Allgemeinen ähnelt der Prozess zum Installieren neuer Pakete dem in einer Python-Standardumgebung. Einige zusätzliche Schritte sind jedoch erforderlich, wenn der Server keine Internetverbindung hat.

Weitere Informationen zu dem Paketspeicherort und Installationspfaden finden Sie unter [Abrufen von Paketinformationen für Python](python-package-information.md).

## <a name="prerequisites"></a>Voraussetzungen

+ [SQL Server Machine Learning Services](../install/sql-machine-learning-services-windows-install.md) muss mit der Python-Sprachoption installiert sein.

### <a name="other-considerations"></a>Weitere Überlegungen

+ Die Pakete müssen Python 3.5-kompatibel sein und unter Windows ausgeführt werden.

+ Die Python-Paketbibliothek befindet sich im Ordner „Programme“ Ihrer SQL Server-Instanz, und für die Installation in diesem Ordner sind standardmäßig Administratorberechtigungen erforderlich. Weitere Informationen finden Sie unter [Speicherort der Paketbibliothek](../package-management/python-package-information.md#default-python-library-location).

+ Die Paketinstallation erfolgt pro Instanz. Wenn Sie über mehrere Instanzen von Machine Learning Services verfügen, müssen Sie das Paket zu jeder hinzufügen.

+ Datenbankserver werden häufig abgeriegelt. In vielen Fällen wird der Internetzugriff vollständig blockiert. Bei Paketen mit einer langen Liste von Abhängigkeiten müssen Sie diese Abhängigkeiten im Voraus bestimmen und bereit sein, jede einzelne manuell zu installieren.

+ Bevor Sie ein Paket hinzufügen, sollten Sie überprüfen, ob das Paket für die SQL Server-Umgebung geeignet ist.

  + Es wird empfohlen, Python innerhalb der Datenbank für Tasks zu verwenden, die von einer engen Integration in die Datenbank-Engine profitieren, z. B. maschinelles Lernen, anstatt von Tasks, die einfache Datenbankabfragen ausführen.

  + Wenn Sie Pakete hinzufügen, deren Ausführung auf dem Server sehr rechenintensiv ist, wird die Leistung beeinträchtigt.

  + In einer gesicherten SQL Server-Umgebung sollten Sie Folgendes vermeiden:
    + Pakete, für die Netzwerkzugriff erforderlich ist
    + Pakete, für die erhöhte Zugriffsrechte für das Dateisystem erforderlich sind
    + Pakete, die für die Webentwicklung oder andere Aufgaben verwendet werden und nicht von der Ausführung in SQL Server profitieren

## <a name="add-a-python-package-on-sql-server"></a>Hinzufügen eines Python-Pakets in SQL Server

Um ein neues Python-Paket zu installieren, das in einem Skript für SQL Server verwendet werden kann, installieren Sie das Paket in der Instanz von Machine Learning Services. Wenn Sie über mehrere Instanzen von Machine Learning Services verfügen, müssen Sie das Paket zu jeder hinzufügen.

Das in den folgenden Beispielen installierte Paket ist [CNTK](/cognitive-toolkit/), ein Framework von Microsoft für Deep Learning, welches das Anpassen, Trainieren und gemeinsame Nutzen verschiedener Arten neuronaler Netze unterstützt.

### <a name="for-offline-install-download-the-python-package"></a>Herunterladen des Python-Pakets für eine Offline-Installation

Wenn Sie Python-Pakete auf einem Server ohne Internetzugriff installieren, müssen Sie die WHL-Datei zunächst auf einem Computer mit Internetzugriff herunterladen und dann die Datei auf den Server kopieren.

Beispielsweise können Sie auf einem mit dem Internet verbundenen Computer die Datei `cntk-2.1-cp35-cp35m-win_amd64.whl` von der Website [https://cntk.ai/PythonWheel/CPU-Only](https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl) herunterladen und dann die Datei in einen lokalen Ordner auf dem Computer mit SQL Server kopieren.

> [!IMPORTANT]
> Laden Sie unbedingt die Windows-Version des Pakets herunter. Wenn die Datei mit „.gz“ endet, handelt es sich wahrscheinlich nicht um die richtige Version.

Weitere Informationen zum Herunterladen des CNTK-Frameworks für mehrere Plattformen und für mehrere Versionen von Python finden Sie unter [Einrichten von CNTK auf Ihrem Computer](/cognitive-toolkit/Setup-CNTK-on-your-machine).

### <a name="locate-the-python-library"></a>Auffinden der Python-Bibliothek

Ermitteln Sie den von SQL Server verwendeten Standardspeicherort der Python-Bibliothek. Wenn Sie mehrere Instanzen installiert haben, wechseln Sie zum Ordner `PYTHON_SERVICES` für die Instanz, der Sie das Paket hinzufügen möchten.

Wenn beispielsweise Machine Learning Services mit Standardeinstellungen installiert wurde und maschinelles Lernen in der Standardinstanz aktiviert ist, lautet der Pfad:

```console
cd "C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES"
```

> [!TIP]
> Für künftiges Debuggen und Testen sollten Sie ggf. eine Python-Umgebung speziell für die Instanzbibliothek einrichten.

### <a name="install-the-package-using-pip"></a>Installieren des Pakets mit pip

Verwenden Sie das Installationsprogramm **pip**, um neue Pakete zu installieren. `pip.exe` finden Sie im Unterordner `Scripts` des Ordners `PYTHON_SERVICES`. Das SQL Server-Setup fügt den Unterordner `Scripts` nicht dem Systempfad hinzu. Daher müssen Sie den vollständigen Pfad angeben, oder Sie können unter Windows den Ordner „Scripts“ zur Variablen PATH hinzufügen.

> [!NOTE]
> Wenn Sie Visual Studio 2017 oder Visual Studio 2015 mit den Python-Erweiterungen verwenden, können Sie `pip install` im Fenster **Python-Umgebungen** ausführen. Klicken Sie auf **Pakete**, und geben Sie im Textfeld den Namen oder Speicherort des zu installierenden Pakets an. Sie müssen nicht `pip install` eingeben, da dies automatisch für Sie eingegeben wird.

+ Wenn der Computer Internetzugriff hat, geben Sie den Namen des Pakets an:

  ```console
  scripts\pip.exe install cntk
  ```
  Sie können auch die URL eines bestimmten Pakets und einer bestimmten Version angeben wie z. B:

  ```console
  scripts\pip.exe install https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl
  ```

+ Wenn der Computer keinen Internetzugriff hat, geben Sie die WHL-Datei an, die Sie zuvor heruntergeladen haben. Beispiel:

  ```console
  scripts\pip.exe install C:\Downloads\cntk-2.1-cp35-cp35m-win_amd64.whl
  ```

Möglicherweise werden Sie aufgefordert, Berechtigungen zu erhöhen, um die Installation abzuschließen.
Im Verlauf der Installation können Sie im Eingabeaufforderungsfenster Statusmeldungen sehen.

### <a name="load-the-package-or-its-functions-as-part-of-your-script"></a>Laden des Pakets oder seiner Funktionen als Teil Ihres Skripts

Nach Abschluss der Installation können Sie sofort mit der Verwendung des Pakets in Python-Skripts in SQL Server beginnen.

Um Funktionen aus dem Paket in Ihrem Skript zu verwenden, fügen Sie in den Anfangszeilen des Skripts die Standardanweisung `import <package_name>` ein:

```sql
EXECUTE sp_execute_external_script 
  @language = N'Python', 
  @script = N'
import cntk
# Python statements ...
'
```

## <a name="see-also"></a>Weitere Informationen

+ [Abrufen von Paketinformationen für Python](python-package-information.md)
+ [Python-Tutorials für SQL Server Machine Learning Services](../tutorials/python-tutorials.md)
+ [Python-API für CNTK](https://cntk.ai/pythondocs/tutorials.html).