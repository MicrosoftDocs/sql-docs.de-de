---
title: Abrufen von Paketinformationen für Python
description: Erfahren Sie, wie Sie Informationen zu installierten Python-Paketen, einschließlich Versionsnummern und Installationsspeicherorten, in SQL Server Machine Learning Services erhalten.
ms.custom: ''
ms.prod: sql
ms.technology: machine-learning
ms.date: 06/03/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 29ed535b25d364ffa552aba20b10b446e0561cf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352521"
---
# <a name="get-python-package-information"></a>Abrufen von Paketinformationen für Python

[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In diesem Artikel erfahren Sie, wie Sie in [Machine Learning Services in SQL Server](../sql-server-machine-learning-services.md) und auf [Big Data-Clustern](../../big-data-cluster/machine-learning-services.md) Informationen zu installierten Python-Paketen abrufen, einschließlich der Versionen und dem Speicherort der Installation. Dabei wird anhand von Python-Beispielskripts veranschaulicht, wie Sie Paketinformationen, z. B. Installationspfad und Version, auflisten können.
::: moniker-end
::: moniker range="=sql-server-2017"
In diesem Artikel erfahren Sie, wie Sie Informationen zu installierten Python-Paketen, einschließlich Versionsnummern und Installationsspeicherorten, in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) erhalten. Dabei wird anhand von Python-Beispielskripts veranschaulicht, wie Sie Paketinformationen, z. B. Installationspfad und Version, auflisten können.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In diesem Artikel erfahren Sie, wie Sie Informationen zu installierten Python-Paketen, einschließlich Versionsnummern und Installationsspeicherorten, in [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview) erhalten. Dabei wird anhand von Python-Beispielskripts veranschaulicht, wie Sie Paketinformationen, z. B. Installationspfad und Version, auflisten können.
::: moniker-end

## <a name="default-python-library-location"></a>Standardspeicherort der Python-Bibliothek

Wenn Sie Machine Learning-Funktionen mit SQL Server installieren, wird eine einzelne Paketbibliothek auf Instanzebene für jede von Ihnen installierte Sprache erstellt. Die Instanzbibliothek ist ein gesicherter Ordner, der bei SQL Server registriert ist.

Alle Skripts oder Codes, die datenbankintern in SQL Server ausgeführt werden, müssen Funktionen aus der Instanzbibliothek laden. SQL Server kann nicht auf Pakete zugreifen, die in anderen Bibliotheken installiert sind. Dies gilt auch für Remoteclients: Jeder Python-Code, der im Servercomputekontext ausgeführt wird, kann nur Pakete verwenden, die in der Instanzbibliothek installiert sind.
Zum Schutz der Serverressourcen kann die Standardinstanzbibliothek nur von einem Computeradministrator geändert werden.

::: moniker range="=sql-server-2017"
Der Standardpfad der Binärdateien für Python lautet:

`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES`

Dabei wird von der standardmäßigen SQL-Instanz, MSSQLSERVER, ausgegangen. Wenn SQL Server als benutzerdefinierte benannte Instanz installiert wird, wird stattdessen der angegebene Name verwendet.
::: moniker-end

::: moniker range=">=sql-server-ver15"
Der Standardpfad der Binärdateien für Python lautet:

`C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\PYTHON_SERVICES`

Dabei wird von der standardmäßigen SQL-Instanz, MSSQLSERVER, ausgegangen. Wenn SQL Server als benutzerdefinierte benannte Instanz installiert wird, wird stattdessen der angegebene Name verwendet.
::: moniker-end

Aktivieren Sie externe Skripts, indem Sie die folgenden SQL-Befehle ausführen:

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH override;
```

::: moniker range="=azuresqldb-mi-current"
> [!IMPORTANT]
> In Azure SQL Managed Instance löst die Ausführung der Befehle sp_configure und RECONFIGURE einen Neustart von SQL Server aus, damit die RG-Einstellungen übernommen werden können. Dies kann zu einer Nichtverfügbarkeit von wenigen Sekunden führen.
::: moniker-end

Sie können die folgende SQL-Anweisung ausführen, um die Standardbibliothek für die aktuelle Instanz zu überprüfen. In diesem Beispiel wird die Liste der in der Python-Variablen `sys.path` enthaltenen Ordner zurückgegeben. Die Liste enthält das aktuelle Verzeichnis und den Pfad der Standardbibliothek.

```sql
EXECUTE sp_execute_external_script
  @language =N'Python',
  @script=N'import sys; print("\n".join(sys.path))'
```

Weitere Informationen zur Variablen `sys.path`, und wie sie zum Festlegen des Suchpfads für Module des Interpreters verwendet wird, finden Sie im Abschnitt zum [Suchpfad für das Modul](https://docs.python.org/2/tutorial/modules.html#the-module-search-path).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
> [!NOTE]
> Versuchen Sie nicht, Python-Pakete direkt in der SQL-Paketbibliothek mithilfe von **pip** oder ähnlichen Methoden zu installieren. Verwenden Sie stattdessen **sqlmlutils**, um Pakete in einer SQL-Instanz zu installieren. Weitere Informationen finden Sie unter [Installieren von Python-Paketen mit sqlmlutils](install-additional-python-packages-on-sql-server.md).
::: moniker-end

## <a name="default-microsoft-python-packages"></a>Microsoft Python-Standardpakete

Wenn Sie bei der Installation die Microsoft Python-Funktion auswählen, werden die folgenden Python-Pakete mit SQL Server Machine Learning Services installiert.

| Pakete | Version |  BESCHREIBUNG |
| ---------|---------|--------------|
| [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) | 9.4.7 | Wird für Remotecomputekontexte, Streaming, parallele Ausführung von RX-Funktionen für Datenimport und Transformation, Modellierung, Visualisierung und Analyse verwendet. |
| [microsoftml](/machine-learning-server/python-reference/microsoftml/microsoftml-package) | 9.4.7 | Fügt Machine Learning-Algorithmen in Python hinzu. |

Informationen zu der enthaltenen Python-Version finden Sie unter [Python- und R-Pakete](../sql-server-machine-learning-services.md#versions).

### <a name="component-upgrades"></a>Komponentenupgrades

Python-Pakete werden standardmäßig durch Service Packs und kumulative Updates aktualisiert. Zusätzliche Pakete und vollständige Versionsupgrades von Python-Kernkomponenten sind nur durch Produktupgrades oder durch Binden des Python-Supports an Microsoft Machine Learning Server möglich.

Weitere Informationen finden Sie unter [Upgrade R and Python components in SQL Server (Upgrade von R- und Python-Komponenten in SQL Server)](../install/upgrade-r-and-python.md).

## <a name="default-open-source-python-packages"></a>Open-Source-Standardpakete für Python

Wenn Sie während der Installation als Programmiersprache „Python“ auswählen, wird die Anaconda 4.2-Distribution (über Python 3.5) installiert. Zusätzlich zu den Python-Codebibliotheken umfasst die Standardinstallation Beispieldaten, Komponententests und Beispielskripts.

> [!IMPORTANT]
> Überschreiben Sie die vom SQL Server-Setup installierte Version von Python niemals manuell mit neueren, online verfügbaren Versionen. Python-Pakete von Microsoft basieren auf bestimmten Versionen von Anaconda. Die Änderung Ihrer Installation könnte diese instabil machen.

## <a name="list-all-installed-python-packages"></a>Auflisten aller installierten Python-Pakete

Das folgende Beispielskript zeigt eine Liste aller in der SQL Server-Instanz installierten Python-Pakete an.

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import pkg_resources
import pandas
OutputDataSet = pandas.DataFrame(sorted([(i.key, i.version) for i in pkg_resources.working_set]))'
WITH result sets((Package NVARCHAR(128), Version NVARCHAR(128)));
```

## <a name="find-a-single-python-package"></a>Suchen eines einzelnen Python-Pakets

Wenn Sie ein Python-Paket installiert haben und sicherstellen möchten, dass es für eine bestimmte SQL Server-Instanz verfügbar ist, können Sie eine gespeicherte Prozedur ausführen, um nach dem Paket zu suchen und Meldungen zurückzugeben.

Beispielsweise kann mit dem folgenden Code nach dem `scikit-learn`-Paket gesucht werden.
Wenn das Paket gefunden wird, gibt der Code die Paketversion aus.

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import pkg_resources
pkg_name = "scikit-learn"
try:
    version = pkg_resources.get_distribution(pkg_name).version
    print("Package " + pkg_name + " is version " + version)
except:
    print("Package " + pkg_name + " not found")
'
```

Ergebnis:

```text
STDOUT message(s) from external script: Package scikit-learn is version 0.20.2
```

<a name="bkmk_SQLPythonVersion"></a>
## <a name="view-the-version-of-python"></a>Anzeigen der Python-Version

Im folgenden Beispielcode wird die Version der in der SQL Server-Instanz installierten Python-Version zurückgegeben.

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
import sys
print(sys.version)
'
```

## <a name="next-steps"></a>Nächste Schritte

::: moniker range="=sql-server-2017"
+ [Installieren von Paketen mit Python-Tools](install-python-packages-standard-tools.md)
::: moniker-end
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
+ [Installieren neuer Python-Pakete mit sqlmlutils](install-additional-r-packages-on-sql-server.md)
::: moniker-end