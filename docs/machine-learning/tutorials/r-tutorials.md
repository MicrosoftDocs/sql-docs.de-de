---
title: R-Tutorials
titleSuffix: SQL machine learning
description: In diesem Artikel werden die R-Tutorials für SQL Machine Learning aufgeführt. Erfahren Sie, wie Sie Skripts ausführen und Machine Learning-Modelle erstellen.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 8b463ded43f7906627e08d86eb1809b169e6125d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346241"
---
# <a name="r-tutorials-for-sql-machine-learning"></a>R-Turorials für SQL Machine Learning
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In diesem Artikel werden die R-Tutorials und -Schnellstarts für [Machine Learning Services in SQL Server](../sql-server-machine-learning-services.md) und in [Big Data-Clustern](../../big-data-cluster/machine-learning-services.md) beschrieben.
::: moniker-end
::: moniker range="=sql-server-2017"
In diesem Artikel werden die R-Tutorials und -Schnellstarts für [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) beschrieben.
::: moniker-end
::: moniker range="=sql-server-2016"
In diesem Artikel werden die R-Tutorials und -Schnellstarts für [SQL Server 2016 R Services](../r/sql-server-r-services.md) beschrieben.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In diesem Artikel werden die Python-Tutorials und -Schnellstarts für [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) beschrieben.
::: moniker-end

<a name="bkmk_sqltutorials"></a>

## <a name="r-tutorials"></a>R-Tutorials

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
| Lernprogramm | BESCHREIBUNG |
|------|-------------|
| [Vorhersagen für einen Skiverleih mit Entscheidungsstruktur](r-predictive-model-introduction.md) | Verwenden Sie R und ein Entscheidungsstrukturmodell, um die Anzahl zukünftiger Skivermietungen vorherzusagen. Bereiten Sie mithilfe von Notebooks in Azure Data Studio Daten vor und trainieren Sie das Modell, und verwenden Sie T-SQL für die Modellimplementierung. |
| [Kategorisierung von Kunden mit K-Means-Clustering](r-clustering-model-introduction.md) | Verwenden Sie R, um ein K-Means-Clusteringmodell zum Kategorisieren von Kunden zu entwickeln und bereitzustellen. Bereiten Sie mithilfe von Notebooks in Azure Data Studio Daten vor und trainieren Sie das Modell, und verwenden Sie T-SQL für die Modellimplementierung. |
| [Datenbankinterne R-Analysen für Data Scientists](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md) | Dieses Tutorial richtet sich an R-Entwickler ohne Vorkenntnisse in SQL-Machine-Learning und erläutert, wie allgemeine Data-Science-Aufgaben in SQL ausgeführt werden. Zudem erfahren Sie, wie Sie Daten laden und visualisieren, ein Modell trainieren sowie in einer Datenbank speichern und das Modell für Predictive Analytics verwenden. |
| [Datenbankinterne R-Analysen für SQL-Entwickler](../tutorials/r-taxi-classification-introduction.md) | In diesem Tutorial erfahren Sie, wie Sie nur mithilfe von SQL-Tools eine vollständige R-Lösung erstellen. Konzentriert sich auf das Verschieben einer Lösung in die Produktionsumgebung. Zudem wird erläutert, wie Sie R-Code in einer gespeicherten Prozedur umschließen, ein R-Modell in einer Datenbank speichern und zu Vorhersagezwecken parametrisierte Aufrufe des R-Modells durchführen. |
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
| Lernprogramm | BESCHREIBUNG |
|------|-------------|
| [Vorhersagen für einen Skiverleih mit Entscheidungsstruktur](r-predictive-model-introduction.md) | Verwenden Sie R und ein Entscheidungsstrukturmodell, um die Anzahl zukünftiger Skivermietungen vorherzusagen. Bereiten Sie mithilfe von Notebooks in Azure Data Studio Daten vor und trainieren Sie das Modell, und verwenden Sie T-SQL für die Modellimplementierung. |
| [Kategorisierung von Kunden mit K-Means-Clustering](r-clustering-model-introduction.md) | Verwenden Sie R, um ein K-Means-Clusteringmodell zum Kategorisieren von Kunden zu entwickeln und bereitzustellen. Bereiten Sie mithilfe von Notebooks in Azure Data Studio Daten vor und trainieren Sie das Modell, und verwenden Sie T-SQL für die Modellimplementierung. |
::: moniker-end

## <a name="r-quickstarts"></a>R-Schnellstarts

Wenn SQL Machine Learning noch Neuland für Sie ist, können Sie auch die R-Schnellstarts ausprobieren.

| Schnellstart | BESCHREIBUNG |
|-|-|
| [Ausführen einfacher R-Skripts mit SQL Server Machine Learning Services](quickstart-r-create-script.md) | Erfahren Sie mehr über die Grundlagen zum Aufrufen von R in T-SQL mithilfe von [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). |
| [Datenstrukturen und -objekte, die R verwenden](quickstart-r-data-types-and-objects.md) | Dieser Schnellstart zeigt, wie mithilfe von R Datenstrukturen verarbeitet werden. |
| [Erstellen und Bewerten eines Vorhersagemodells in R](quickstart-r-data-types-and-objects.md) | Hier sehen Sie, wie ein R-Modell erstellt, trainiert und verwendet wird, um Vorhersagen aus neuen Daten zu treffen. |

## <a name="next-steps"></a>Nächste Schritte

+ [Python-Erweiterung in SQL Server](../concepts/extension-r.md)
