---
title: Was ist SQL Server Machine Learning Services (Python und R)?
titleSuffix: ''
description: Machine Learning Services ist ein Feature in SQL Server, das die Möglichkeit bietet, Python- und R-Skripts mit relationalen Daten auszuführen. Sie können Open-Source-Pakete und -Frameworks und die Microsoft Python- und R-Pakete für Predictive Analytics und Machine Learning verwenden. Die Skripts werden in der Datenbank ausgeführt, ohne dass Daten aus SQL Server oder über das Netzwerk verschoben werden. In diesem Artikel werden die Grundlagen von SQL Server Machine Learning Services sowie die ersten Schritte damit erläutert.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/10/2020
ms.topic: overview
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 23206301c399449042380976e985ec3981933caa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340311"
---
# <a name="what-is-sql-server-machine-learning-services-with-python-and-r"></a>Wie funktioniert SQL Server Machine Learning Services mit Python und R?
[!INCLUDE [SQL Server 2017 SQL MI](../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Machine Learning Services ist ein Feature in SQL Server, das die Möglichkeit bietet, Python- und R-Skripts mit relationalen Daten auszuführen. Sie können Open-Source-Pakete und -Frameworks und die [Microsoft Python- und R-Pakete](#packages) für Predictive Analytics und Machine Learning verwenden. Die Skripts werden in der Datenbank ausgeführt, ohne dass Daten aus SQL Server oder über das Netzwerk verschoben werden. In diesem Artikel werden die Grundlagen von SQL Server Machine Learning Services sowie die ersten Schritte damit erläutert.

Informationen zu Machine Learning auf anderen SQL-Plattformen finden Sie in der [SQL Machine Learning-Dokumentation](index.yml).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Informationen zum Ausführen von Java in SQL Server finden Sie in der [Dokumentation zur Java-Spracherweiterung](../language-extensions/java-overview.md).
::: moniker-end

## <a name="execute-python-and-r-scripts-in-sql-server"></a>Ausführen von Python- und R-Skripts in SQL Server

Mit SQL Server Machine Learning Services können Sie Python- und R-Skripts in einer Datenbank ausführen. Sie können das Feature verwenden, um Daten vorzubereiten und zu bereinigen, Features zu entwickeln und Machine Learning-Modelle in einer Datenbank zu trainieren, auszuwerten und bereitzustellen. Mit dem Feature können Sie Skripts ausführen, in denen sich die Daten befinden. Die Übertragung der Daten über das Netzwerk auf einen anderen Server entfällt.

Sie können Python- und R-Skripts auf einer SQL Server-Instanz mit der gespeicherten Prozedur [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) ausführen.

In Machine Learning Services sind Basisverteilungen von R und Python enthalten. Neben den Microsoft-Paketen können Sie Open-Source-Pakete und -Frameworks wie PyTorch, TensorFlow und scikit-learn installieren und verwenden.

Zum Ausführen von Python- und R-Skripts in SQL Server wird von Machine Learning Services ein Erweiterbarkeitsframework verwendet. Weitere Informationen zur Funktionsweise finden Sie unter:

+ [Erweiterbarkeitsframework](concepts/extensibility-framework.md)
+ [Python-Erweiterung](concepts/extension-python.md)
+ [R-Erweiterung](concepts/extension-r.md)

## <a name="get-started-with-machine-learning-services"></a>Erste Schritte mit Machine Learning Service

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
1. [Installieren Sie SQL Server Machine Learning Services unter Windows](install/sql-machine-learning-services-windows-install.md) oder [unter Linux](../linux/sql-server-linux-setup-machine-learning.md?toc=/sql/machine-learning/toc.json). Sie können ferner [Machine Learning Services für Big Data-Cluster](../big-data-cluster/machine-learning-services.md) und die [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) verwenden.

1. Konfigurieren Sie Ihre Entwicklungstools. Sie können [Ausführen von Python- und R-Skripts in Azure Data Studio-Notebooks](install/sql-machine-learning-azure-data-studio.md) verwenden. Sie können auch T-SQL in [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md) ausführen.

1. Schreiben Sie Ihr erstes Python- oder R-Skript.

   + [Python-Turorials für SQL Machine Learning](tutorials/python-tutorials.md)
   + [R-Turorials für SQL Machine Learning](tutorials/r-tutorials.md)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
+ Schreiben Sie Ihr erstes Python- oder R-Skript.

   + [Python-Turorials für SQL Machine Learning](tutorials/python-tutorials.md)
   + [R-Turorials für SQL Machine Learning](tutorials/r-tutorials.md)
::: moniker-end

::: moniker range="=sql-server-2017"
1. [Installieren von SQL Server Machine Learning Services (Python und R) unter Windows](install/sql-machine-learning-services-windows-install.md)

1. Konfigurieren Sie Ihre Entwicklungstools. Sie können [Ausführen von Python- und R-Skripts in Azure Data Studio-Notebooks](install/sql-machine-learning-azure-data-studio.md) verwenden. Sie können T-SQL auch in [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md) verwenden.

1. Schreiben Sie Ihr erstes Python- oder R-Skript.

   + [Python-Turorials für SQL Machine Learning](tutorials/python-tutorials.md)
   + [R-Turorials für SQL Machine Learning](tutorials/r-tutorials.md)
::: moniker-end

<a name="versions"></a>

## <a name="python-and-r-versions"></a>Python- und R-Versionen

Im Folgenden werden die Versionen von Python und R aufgeführt, die in Machine Learning Services enthalten sind.

| SQL Server-Version | Python-Version | R-Version |
|-|-|-|
| SQL Server 2017 | 3.5.2 | 3.3.3 |
| SQL Server 2019 | 3.7.3 | 3.5.2 |

Die R-Version in SQL Server 2016 finden Sie in [„Was ist SQL Server 2016 R Services?“ im Abschnitt „R-Version“](r/sql-server-r-services.md?view=sql-server-2016&preserve-view=true#version).

<a name="packages"></a>

## <a name="python-and-r-packages"></a>Python- und R-Pakete

Neben den Enterprise-Paketen von Microsoft können Sie auch Open-Source-Pakete und -Frameworks verwenden. Die gängigsten Open-Source-Pakete für Python und R sind in Machine Learning Services bereits vorinstalliert. Zudem sind die folgenden Python- und R-Pakete von Microsoft bereits enthalten:

| Sprache | Paket | BESCHREIBUNG |
|-|-|-|
| Python | [revoscalepy](python/ref-py-revoscalepy.md) | Das primäre Paket mit skalierbaren Python-Funktionen zum Transformieren und Bearbeiten von Daten sowie zum Erstellen von statistischen Übersichten, Visualisierungen und vielen anderen Modellierungsformen. Zudem enthält das Paket zur Parallelverarbeitung Funktionen zum automatischen Verteilen von Workloads auf verfügbare Kerne. |
| Python | [microsoftml](python/ref-py-microsoftml.md) | Stellt Machine Learning-Algorithmen zur Erstellung von benutzerdefinierten Modellen für die Text-, Bild- und Stimmungsanalyse bereit. | 
| R | [RevoScaleR](r/ref-r-revoscaler.md) | Das primäre Paket mit skalierbaren R-Funktionen zum Transformieren und Bearbeiten von Daten sowie zum Erstellen von statistischen Übersichten, Visualisierungen und vielen anderen Modellierungsformen. Zudem enthält das Paket zur Parallelverarbeitung Funktionen zum automatischen Verteilen von Workloads auf verfügbare Kerne. |
| R | [MicrosoftML (R)](r/ref-r-microsoftml.md) | Stellt Machine Learning-Algorithmen zur Erstellung von benutzerdefinierten Modellen für die Text-, Bild- und Stimmungsanalyse bereit. |
| R | [olapR](r/ref-r-olapr.md) | R-Funktionen, die für MDX-Abfragen eines SQL Server Analysis Services-OLAP-Cube verwendet werden können. |
| R | [sqlrutils](r/ref-r-sqlrutils.md) | Ein Mechanismus zur Verwendung von R-Skripts in gespeicherten T-SQL-Prozeduren sowie zum Registrieren dieser gespeicherten Prozeduren bei einer Datenbank und zum Ausführen der gespeicherten Prozeduren über eine [R-Entwicklungsumgebung](r/set-up-a-data-science-client.md). |
| R | [Microsoft R Open](https://mran.microsoft.com/rro) | Bei Microsoft R Open (MRO) handelt es sich um die erweiterte Verteilung von R von Microsoft. Diese umfassende Open-Source-Plattform wird für statistische Analysen und Data Science verwendet. Sie basiert auf R und ist vollständig kompatibel mit R. Zudem beinhaltet sie Funktionen für eine verbesserte Leistung und Reproduzierbarkeit. |

Weitere Informationen zu den in Machine Learning Services installierten Paketen sowie zur Installation anderer Pakete finden Sie unter:

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
+ [Abrufen von Paketinformationen für Python](package-management/python-package-information.md)
+ [Installieren von Python-Paketen mit sqlmlutils](package-management/install-additional-python-packages-on-sql-server.md)
+ [Abrufen von R-Paketinformationen](package-management/r-package-information.md)
+ [Installieren von neuen R-Paketen mit sqlmlutils](package-management/install-additional-r-packages-on-sql-server.md)
::: moniker-end
::: moniker range="=sql-server-2017"
+ [Abrufen von Paketinformationen für Python](package-management/python-package-information.md)
+ [Installieren von Paketen mit Python-Tools in SQL Server](package-management/install-python-packages-standard-tools.md)
+ [Abrufen von R-Paketinformationen](package-management/r-package-information.md)
+ [Verwenden Sie T-SQL (CREATE EXTERNAL LIBRARY) zum Installieren von R-Paketen in SQL Server](package-management/install-r-packages-with-tsql.md).
::: moniker-end

## <a name="next-steps"></a>Nächste Schritte

+ [Installieren von SQL Server Machine Learning Services unter Windows](install/sql-machine-learning-services-windows-install.md) oder [unter Linux](../linux/sql-server-linux-setup-machine-learning.md?toc=/sql/machine-learning/toc.json)
+ [Python-Turorials für SQL Machine Learning](tutorials/python-tutorials.md)
+ [R-Turorials für SQL Machine Learning](tutorials/r-tutorials.md)