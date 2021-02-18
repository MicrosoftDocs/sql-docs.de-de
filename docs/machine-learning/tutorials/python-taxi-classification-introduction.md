---
title: 'Python-Tutorial: Vorhersagen von Taxi-Fahrpreisen in New York City mit binärer Klassifizierung'
titleSuffix: SQL machine learning
description: Erfahren Sie, wie Sie Python-Code in gespeicherte Prozeduren von SQL Server und in T-SQL-Funktionen mit SQL Machine Learning einbetten, um Taxi-Fahrpreise in New York City mithilfe von binärer Klassifizierung vorherzusagen.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/28/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 103eaec3991e9c4f00f819ca2811f395bfbeb2b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339669"
---
# <a name="python-tutorial-predict-nyc-taxi-fares-with-binary-classification"></a>Python-Tutorial: Vorhersagen von Taxi-Fahrpreisen in New York City mit binärer Klassifizierung
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In dieser fünfteiligen Tutorialreihe für SQL-Programmierer erfahren Sie mehr über die Python-Integration in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) oder auf [Big Data-Clustern](../../big-data-cluster/machine-learning-services.md).
::: moniker-end

::: moniker range="=sql-server-2017"
In dieser fünfteiligen Tutorialreihe für SQL-Programmierer erfahren Sie mehr über die Python-Integration in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).
::: moniker-end

::: moniker range=">=azuresqldb-mi-current"
In dieser fünfteiligen Tutorialreihe für SQL-Programmierer erfahren Sie mehr über die Python-Integration in [Machine Learning Services in Azure SQL Managed Instance ](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

In diesem Tutorial erstellen Sie eine Python-basierte Machine Learning-Lösung mithilfe einer Beispieldatenbank auf SQL Server und stellen sie bereit. Sie verwenden dazu T-SQL, Azure Data Studio oder SQL Server Management Studio und eine Datenbankinstanz mit maschinellem Lernen mit SQL und Python-Sprachunterstützung.

Diese Tutorialreihe bietet eine Einführung in Python-Funktionen, die in einem Workflow für Datenmodellierung verwendet werden. Dies beinhaltet unter anderem das Durchsuchen von Daten, das Entwickeln und Trainieren eines binären Klassifizierungsmodells sowie die Modellimplementierung. Sie verwenden dazu Beispieldaten der New York City Taxi and Limousine Commission. Mit dem Modell, das Sie erstellen, soll vorhergesagt werden, ob eine Fahrt (ausgehend von der Tageszeit, der zurückgelegten Strecke und der Abholadresse) mit der Gabe von Trinkgeld endet.

Im ersten Teil dieser Reihe installieren Sie die erforderlichen Komponenten und stellen die Beispieldatenbank wieder her. Im zweiten und dritten Teil entwickeln Sie einige Python-Skripts zur Vorbereitung Ihrer Daten und zum Trainieren eines Machine Learning-Modells. In Teil vier und fünf führen Sie diese Python-Skripts dann in der Datenbank mithilfe von gespeicherten T-SQL-Prozeduren aus.

In diesem Artikel führen Sie Folgendes durch:

> [!div class="checklist"]
> + Installieren der erforderlichen Komponenten
> + Wiederherstellen der Beispieldatenbank

In [Teil zwei](python-taxi-classification-explore-data.md) untersuchen Sie die Beispieldaten und generieren einige Plots.

In [Teil drei](python-taxi-classification-create-features.md) erfahren Sie, wie Sie mithilfe einer Transact-SQL-Funktion aus Rohdaten Features erstellen. Sie rufen anschließend die Funktion aus einer gespeicherten Prozedur auf, um eine Tabelle zu erstellen, die die Funktionswerte enthält.

In [Teil vier](python-taxi-classification-train-model.md) laden Sie die Module und rufen die erforderlichen Funktionen auf, um das Modell mithilfe einer gespeicherten SQL Server-Prozedur zu erstellen und zu trainieren.

In Teil fünf erfahren Sie, wie Sie die Modelle [operationalisieren](python-taxi-classification-deploy-model.md) können, die Sie in Teil vier trainiert und gespeichert haben.

> [!NOTE]
> Dieses Tutorial ist sowohl in R als auch in Python verfügbar. Informationen zur R-Version finden Sie unter [R-Tutorial: Vorhersagen von Taxi-Fahrpreisen in New York City mit binärer Klassifizierung](r-taxi-classification-introduction.md).

## <a name="prerequisites"></a>Voraussetzungen

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
+ Installieren Sie [SQL Server Machine Learning Services mit Python](../install/sql-machine-learning-services-windows-install.md#verify-installation)
::: moniker-end

+ [Erteilen Sie Berechtigungen zum Ausführen von Python-Skripts](../security/user-permission.md)

+ Stellen Sie die [Demodatenbank für Taxifahrten in New York City](demo-data-nyctaxi-in-sql.md) wieder her.

Alle Aufgaben können mithilfe von gespeicherten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Prozeduren in Azure Data Studio oder [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ausgeführt werden.

Für diese Tutorialreihe sollten Sie sich mit grundlegenden Datenbankvorgängen auskennen, wie z. B. dem Erstellen von Datenbanken und Tabellen, dem Importieren von Daten und dem Schreiben von SQL-Abfragen. Kenntnisse im Umgang mit Python werden nicht vorausgesetzt, und der gesamte Python-Code wird bereitgestellt.

## <a name="background-for-sql-developers"></a>Hintergrund für SQL-Entwickler

Der Erstellungsprozess einer Machine Learning-Lösung ist komplex und kann den Einsatz mehrerer Tools sowie die phasenübergreifende Koordinierung von Experten bei folgenden Schritten erfordern:

+ Abrufen und Bereinigen von Daten
+ Untersuchen der Daten und Entwickeln von Modellierungsfunktionen
+ Trainieren und Optimieren des Modells
+ Bereitstellen in der Produktion

Die Entwicklung und das Testen des eigentlichen Codes werden am besten in einer dedizierten Entwicklungsumgebung durchgeführt. Nachdem das Skript vollständig getestet wurde, können Sie es jedoch problemlos in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe von gespeicherten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Prozeduren in der vertrauten Umgebung von Azure Data Studio oder [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] bereitstellen. Das Umbrechen von externem Code in gespeicherten Prozeduren ist der primäre Mechanismus zum Operationalisieren von Code in SQL Server.

Nach dem Speichern des Modells in der Datenbank für Vorhersagen können Sie es aus [!INCLUDE[tsql](../../includes/tsql-md.md)] mithilfe von gespeicherten Prozeduren aufrufen.

Egal, ob Sie ein SQL-Programmierer sind, der noch nicht mit Python vertraut ist, oder ein Python-Entwickler, für den SQL neu ist: In dieser fünfteiligen Tutorialreihe wird ein typischer Workflow für die Durchführung von datenbankinternen Analysen mit Python und SQL Server vorgestellt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel führen Sie folgende Schritte aus:

> [!div class="checklist"]
> + Installieren der Voraussetzungen
> + Wiederherstellen der Beispieldatenbank

> [!div class="nextstepaction"]
> [Python-Tutorial: Untersuchen und Visualisieren von Daten](python-taxi-classification-explore-data.md)