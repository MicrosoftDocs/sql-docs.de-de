---
title: Abrufen von Paketinformationen für R
description: Erfahren Sie, wie Sie Informationen zum Installieren von R-Paketen in SQL Server Machine Learning Services und SQL Server R Services abrufen.
ms.custom: ''
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/27/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 3669b46fa88e8d59fbb3eeb5ff91bc8bb9542ce7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352536"
---
# <a name="get-r-package-information"></a>Abrufen von Paketinformationen für R

[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In diesem Artikel erfahren Sie, wie Sie Informationen zu installierten R-Paketen in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) und in [Big Data-Clustern](../../big-data-cluster/machine-learning-services.md) abrufen. Dabei wird anhand von R-Beispielskripts veranschaulicht, wie Sie Paketinformationen, z. B. Installationspfad und Version, auflisten können.
::: moniker-end
::: moniker range="<=sql-server-2017"
In diesem Artikel erfahren Sie, wie Sie Informationen zu installierten R-Paketen in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) abrufen. Dabei wird anhand von R-Beispielskripts veranschaulicht, wie Sie Paketinformationen, z. B. Installationspfad und Version, auflisten können.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In diesem Artikel erfahren Sie, wie Sie Informationen zu installierten R-Paketen in [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) abrufen. Dabei wird anhand von R-Beispielskripts veranschaulicht, wie Sie Paketinformationen, z. B. Installationspfad und Version, auflisten können.
::: moniker-end

## <a name="default-r-library-location"></a>Standardspeicherort der R-Bibliothek

Wenn Sie Machine Learning-Funktionen mit SQL Server installieren, wird eine einzelne Paketbibliothek auf Instanzebene für jede von Ihnen installierte Sprache erstellt. Unter Windows ist die Instanzbibliothek ein gesicherter Ordner, der bei SQL Server registriert ist.

Alle Skripts oder Codes, die datenbankintern in SQL Server ausgeführt werden, müssen Funktionen aus der Instanzbibliothek laden. SQL Server kann nicht auf Pakete zugreifen, die in anderen Bibliotheken installiert sind. Dies gilt auch für Remoteclients: Jedes R-Skript, das im Servercomputekontext ausgeführt wird, kann nur Pakete verwenden, die in der Instanzbibliothek installiert sind.
Zum Schutz der Serverressourcen kann die Standardinstanzbibliothek nur von einem Computeradministrator geändert werden.

::: moniker range="=sql-server-2016"
Der Standardpfad der Binärdateien für R lautet:

`C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\library`

Dabei wird von der standardmäßigen SQL-Instanz, MSSQLSERVER, ausgegangen. Wenn SQL Server als benutzerdefinierte benannte Instanz installiert wird, wird stattdessen der angegebene Name verwendet.
::: moniker-end

::: moniker range="=sql-server-2017"
Der Standardpfad der Binärdateien für R lautet:

`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library`

Dabei wird von der standardmäßigen SQL-Instanz, MSSQLSERVER, ausgegangen. Wenn SQL Server als benutzerdefinierte benannte Instanz installiert wird, wird stattdessen der angegebene Name verwendet.
::: moniker-end

::: moniker range=">=sql-server-ver15"
Der Standardpfad der Binärdateien für R lautet:

`C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\library`

Dabei wird von der standardmäßigen SQL-Instanz, MSSQLSERVER, ausgegangen. Wenn SQL Server als benutzerdefinierte benannte Instanz installiert wird, wird stattdessen der angegebene Name verwendet.
::: moniker-end

Sie können die folgende Anweisung ausführen, um die Standardbibliothek des R-Pakets für die aktuelle Instanz zu überprüfen:

```sql
EXECUTE sp_execute_external_script  
  @language = N'R',
  @script = N'OutputDataSet <- data.frame(.libPaths());'
WITH RESULT SETS (([DefaultLibraryName] VARCHAR(MAX) NOT NULL));
GO
```

## <a name="default-microsoft-r-packages"></a>Microsoft R-Standardpakete

::: moniker range="=sql-server-2016"

Die folgenden Microsoft R-Pakete werden mit SQL Server R Services installiert.

|Pakete | Version | BESCHREIBUNG |
|---------|---------|-------------|
| [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler)  | 8.0.3 | Wird für Remotecomputekontexte, Streaming, parallele Ausführung von RX-Funktionen für Datenimport und Transformation, Modellierung, Visualisierung und Analyse verwendet. |
| [sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils) | 1.0.0 | Wird zum Einschließen von R-Skripts in gespeicherte Prozeduren verwendet. |

::: moniker-end

::: moniker range="=sql-server-2017"

Wenn Sie bei der Installation die Microsoft R-Funktion auswählen, werden die folgenden R-Pakete mit SQL Server Machine Learning Services installiert.

|Pakete | Version | BESCHREIBUNG |
|---------|---------|-------------|
| [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler)  | 9.2 | Wird für Remotecomputekontexte, Streaming, parallele Ausführung von RX-Funktionen für Datenimport und Transformation, Modellierung, Visualisierung und Analyse verwendet. |
| [sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils) | 1.0.0 | Wird zum Einschließen von R-Skripts in gespeicherte Prozeduren verwendet. |
| [MicrosoftML](/r-server/r-reference/microsoftml/microsoftml-package)| 1.4.0 | Fügt Machine Learning-Algorithmen in R hinzu. | 
| [olapR](/machine-learning-server/r-reference/olapr/olapr) | 1.0.0 | Wird zum Schreiben von MDX-Anweisungen in R verwendet. |

::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"

Wenn Sie bei der Installation die Microsoft R-Funktion auswählen, werden die folgenden R-Pakete mit SQL Server Machine Learning Services installiert.

|Pakete | Version | BESCHREIBUNG |
|---------|---------|-------------|
| [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler)  | 9.4.7 | Wird für Remotecomputekontexte, Streaming, parallele Ausführung von RX-Funktionen für Datenimport und Transformation, Modellierung, Visualisierung und Analyse verwendet. |
| [sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils) | 1.0.0 | Wird zum Einschließen von R-Skripts in gespeicherte Prozeduren verwendet. |
| [MicrosoftML](/r-server/r-reference/microsoftml/microsoftml-package)| 9.4.7 | Fügt Machine Learning-Algorithmen in R hinzu. |
| [olapR](/machine-learning-server/r-reference/olapr/olapr) | 1.0.0 | Wird zum Schreiben von MDX-Anweisungen in R verwendet. |

::: moniker-end

### <a name="component-upgrades"></a>Komponentenupgrades

R-Pakete werden standardmäßig durch Service Packs und kumulative Updates aktualisiert. Zusätzliche Pakete und vollständige Versionsupgrades von R-Kernkomponenten sind nur durch Produktupgrades oder durch Binden des R-Supports an Microsoft Machine Learning Server möglich.

::: moniker range="=sql-server-2016"
Außerdem können Sie MicrosoftML- und olapR-Pakete über ein Komponentenupgrade zu einer SQL Server-Instanz hinzufügen.
::: moniker-end

Weitere Informationen finden Sie unter [Upgrade R and Python components in SQL Server (Upgrade von R- und Python-Komponenten in SQL Server)](../install/upgrade-r-and-python.md).

## <a name="default-open-source-r-packages"></a>Open-Source-Standardpakete für R

Die R-Unterstützung umfasst Open-Source-Funktionen, sodass Sie grundlegende R-Funktionen aufrufen und zusätzliche Open-Source- und Drittanbieterpakete installieren können. Die Unterstützung der Programmiersprache R schließt Kernfunktionen **base**, **stats**, **utils** und andere ein. Eine Basisinstallation von R umfasst auch zahlreiche Beispieldatasets und standardmäßige R-Tools wie **RGui** (ein einfacher interaktiver Editor) und **RTerm** (eine R-Eingabeaufforderung).

Als Open-Source-Distribution von R ist [Microsoft R Open (MRO)](https://mran.microsoft.com/open) in der Installation enthalten. MRO ergänzt die R-Basisinstallation, indem dadurch zusätzliche Open-Source-Pakete wie die [Intel Math Kernel Library](https://en.wikipedia.org/wiki/Math_Kernel_Library) eingebunden werden.

Informationen zu der in jeder SQL Server-Version enthaltenen R-Version finden Sie unter [Python- und R-Pakete](../sql-server-machine-learning-services.md#versions).

> [!IMPORTANT]
> Überschreiben Sie die vom SQL Server-Setup installierte Version von R niemals manuell mit neueren, online verfügbaren Versionen. R-Pakete von Microsoft basieren auf bestimmten Versionen von R. Die Änderung Ihrer Installation könnte diese instabil machen.

## <a name="list-all-installed-r-packages"></a>Auflisten aller installierten R-Pakete

Im folgenden Beispiel wird die R-Funktion `installed.packages()` in einer gespeicherten Prozedur [!INCLUDE[tsql](../../includes/tsql-md.md)] verwendet, um eine Liste von R-Paketen anzuzeigen, die in der Bibliothek „R_SERVICES“ für die aktuelle SQL-Instanz installiert wurden. Dieses Skript gibt die Felder mit dem Paketnamen und der Version in der Datei „DESCRIPTION“ zurück.

```sql
EXECUTE sp_execute_external_script
  @language=N'R',
@script = N'str(OutputDataSet);
packagematrix <- installed.packages();
Name <- packagematrix[,1];
Version <- packagematrix[,3];
OutputDataSet <- data.frame(Name, Version);',
@input_data_1 = N'
  '
WITH RESULT SETS ((PackageName nvarchar(250), PackageVersion nvarchar(max) ))
```

Weitere Informationen zu den optionalen und Standardfeldern für das R-Paket im Feld „DESCRIPTION“ finden Sie unter [https://cran.r-project.org](https://cran.r-project.org/doc/manuals/R-exts.html#The-DESCRIPTION-file).

## <a name="find-a-single-r-package"></a>Suchen eines einzelnen R-Pakets

Wenn Sie ein R-Paket installiert haben und sicherstellen möchten, dass es für eine bestimmte SQL Server-Instanz verfügbar ist, können Sie eine gespeicherte Prozedur ausführen, um das Paket zu laden und Meldungen zurückzugeben.

Mit der folgenden Anweisung wird zum Beispiel das Paket [glue](https://cran.r-project.org/web/packages/glue/) gesucht und, falls verfügbar, geladen.
Wenn das Paket nicht gefunden oder geladen werden kann, erhalten Sie eine Fehlermeldung.

```sql
EXECUTE sp_execute_external_script  
  @language =N'R',
  @script=N'
require("glue")
'
```

Weitere Informationen über das Paket finden Sie in der `packageDescription`.
Die folgende Anweisung gibt Informationen zum **MicrosoftML**-Paket zurück.

```sql
EXECUTE sp_execute_external_script
  @language = N'R',
  @script = N'
print(packageDescription("MicrosoftML"))
'
```

## <a name="next-steps"></a>Nächste Schritte

::: moniker range="<=sql-server-2017"
+ [Installieren von Paketen mit R-Tools](install-r-packages-standard-tools.md)
::: moniker-end
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
+ [Installieren von neuen R-Paketen mit sqlmlutils](install-additional-r-packages-on-sql-server.md)
::: moniker-end