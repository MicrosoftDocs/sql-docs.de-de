---
title: Python-Tutorials
titleSuffix: SQL machine learning
description: In diesem Artikel werden die Python-Tutorials für SQL Machine Learning aufgeführt. Erfahren Sie, wie Sie Skripts ausführen und Machine Learning-Modelle erstellen.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 4b8e997b6990d4d81e9667a41cf359208ca0f31e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341476"
---
# <a name="python-tutorials-for-sql-machine-learning"></a>Python-Turorials für SQL Machine Learning
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In diesem Artikel werden die Python-Tutorials und -Schnellstarts für [Machine Learning Services in SQL Server](../sql-server-machine-learning-services.md) und in [Big Data-Clustern](../../big-data-cluster/machine-learning-services.md) beschrieben.
::: moniker-end
::: moniker range="=sql-server-2017"
In diesem Artikel werden die Python-Tutorials und -Schnellstarts für [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) beschrieben.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In diesem Artikel werden die Python-Tutorials und -Schnellstarts für [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) beschrieben.
::: moniker-end

<a name="bkmk_pythontutorials"></a>

## <a name="python-tutorials"></a>Python-Tutorials

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
| Lernprogramm | BESCHREIBUNG |
|-|-|
| [Vorhersagen für einen Skiverleih mit linearer Regression](python-ski-rental-linear-regression.md) | Verwenden Sie Python und die lineare Regression, um die Anzahl der Skiausleihen vorherzusagen. Bereiten Sie mithilfe von Notebooks in Azure Data Studio Daten vor und trainieren Sie das Modell, und verwenden Sie T-SQL für die Modellimplementierung. |
| [Kategorisierung von Kunden mit K-Means-Clustering](python-clustering-model.md) | Verwenden Sie Python, um ein K-Means-Clusteringmodell zum Kategorisieren von Kunden zu entwickeln und bereitzustellen. Bereiten Sie mithilfe von Notebooks in Azure Data Studio Daten vor und trainieren Sie das Modell, und verwenden Sie T-SQL für die Modellimplementierung. |
| [Erstellen eines Modells mithilfe von revoscalepy](use-python-revoscalepy-to-create-model.md) | Dieses Tutorial veranschaulicht, wie Sie Code von einem Python-Remoteclient ausführen, indem Sie SQL Server als Computekontext verwenden. Es wird ein Modell mithilfe von **rxLinMod** aus der **revoscalepy**-Bibliothek erstellt. |
| [Python-Datenanalysen für SQL-Entwickler](python-taxi-classification-introduction.md) | In dieser umfassenden exemplarischen Vorgehensweise wird veranschaulicht, wie Sie eine komplette Python-Lösung mithilfe von T-SQL erstellen. |
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
| Lernprogramm | BESCHREIBUNG |
|-|-|
| [Vorhersagen für einen Skiverleih mit linearer Regression](python-ski-rental-linear-regression.md) | Verwenden Sie Python und die lineare Regression, um die Anzahl der Skiausleihen vorherzusagen. Bereiten Sie mithilfe von Notebooks in Azure Data Studio Daten vor und trainieren Sie das Modell, und verwenden Sie T-SQL für die Modellimplementierung. |
| [Kategorisierung von Kunden mit K-Means-Clustering](python-clustering-model.md) | Verwenden Sie Python, um ein K-Means-Clusteringmodell zum Kategorisieren von Kunden zu entwickeln und bereitzustellen. Bereiten Sie mithilfe von Notebooks in Azure Data Studio Daten vor und trainieren Sie das Modell, und verwenden Sie T-SQL für die Modellimplementierung. |
::: moniker-end

## <a name="python-quickstarts"></a>Python-Schnellstarts

Wenn SQL Machine Learning noch Neuland für Sie ist, können Sie auch die Python-Schnellstarts ausprobieren.

| Schnellstart | BESCHREIBUNG |
|-|-|
| [Ausführen einfacher Python-Skripts mit SQL Server Machine Learning Services](quickstart-python-create-script.md) | Erfahren Sie mehr über die Grundlagen zum Abrufen von Python in T-SQL mithilfe von [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). |
| [Datenstrukturen und -objekte, die Python verwenden](quickstart-python-data-structures.md) | Dieser Schnellstart zeigt, wie mit den Python-Pandas-Paketen in SQL Datenstrukturen verarbeitet werden. |
| [Erstellen und Bewerten eines Vorhersagemodells in Python](quickstart-python-train-score-model.md) | Hier sehen Sie, wie ein Python-Modell erstellt, trainiert und verwendet wird, um Vorhersagen aus neuen Daten zu treffen. |

## <a name="next-steps"></a>Nächste Schritte

+ [Python-Erweiterung in SQL Server](../concepts/extension-python.md)
