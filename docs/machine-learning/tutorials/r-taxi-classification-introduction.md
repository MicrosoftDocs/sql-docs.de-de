---
title: 'R-Tutorial: Vorhersagen von Taxi-Fahrpreisen in New York City mit binärer Klassifizierung'
titleSuffix: SQL machine learning
description: In dieser fünfteiligen Tutorialreihe lernen Sie, wie Sie R-Code in gespeicherte Prozeduren von SQL Server und in T-SQL-Funktionen mit SQL Machine Learning einbetten, um Taxi-Fahrpreise in New York City mithilfe von binärer Klassifizierung vorherzusagen.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 5cae9925ab4c805028fcd73c47b41d829f6101ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354291"
---
# <a name="r-tutorial-predict-nyc-taxi-fares-with-binary-classification"></a>R-Tutorial: Vorhersagen von Taxi-Fahrpreisen in New York City mit binärer Klassifizierung
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In dieser fünfteiligen Tutorialreihe für SQL-Programmierer erfahren Sie mehr über die R-Integration in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) oder auf [Big Data-Clustern](../../big-data-cluster/machine-learning-services.md).
::: moniker-end

::: moniker range="=sql-server-2017"
In dieser fünfteiligen Tutorialreihe für SQL-Programmierer erfahren Sie mehr über die R-Integration in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).
::: moniker-end

::: moniker range="=sql-server-2016"
In dieser fünfteiligen Tutorialreihe für SQL-Programmierer erfahren Sie mehr über die R-Integration in [SQL Server 2016 R Services](../sql-server-machine-learning-services.md).
::: moniker-end

::: moniker range=">=azuresqldb-mi-current"
In dieser fünfteiligen Tutorialreihe für SQL-Programmierer erfahren Sie mehr über die R-Integration in [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

In diesem Tutorial erstellen Sie eine R-basierte Machine Learning-Lösung mithilfe einer Beispieldatenbank auf SQL Server und stellen sie bereit. Sie verwenden dazu T-SQL, Azure Data Studio oder SQL Server Management Studio und eine Instanz der Datenbankengine mit maschinellem Lernen mit SQL und R-Sprachunterstützung.

Diese Tutorialreihe bietet eine Einführung in R-Funktionen, die in einem Workflow für Datenmodellierung verwendet werden. Dies beinhaltet unter anderem das Durchsuchen von Daten, das Entwickeln und Trainieren eines binären Klassifizierungsmodells sowie die Modellimplementierung. Sie verwenden dazu Beispieldaten der New York City Taxi and Limousine Commission. Mit dem Modell, das Sie erstellen, soll vorhergesagt werden, ob eine Fahrt (ausgehend von der Tageszeit, der zurückgelegten Strecke und der Abholadresse) mit der Gabe von Trinkgeld endet.

Im ersten Teil dieser Reihe installieren Sie die erforderlichen Komponenten und stellen die Beispieldatenbank wieder her. Im zweiten und dritten Teil entwickeln Sie einige R-Skripts zur Vorbereitung Ihrer Daten und zum Trainieren eines Machine Learning-Modells. In Teil vier und fünf führen Sie diese R-Skripts dann in der Datenbank mithilfe von gespeicherten T-SQL-Prozeduren aus.

In diesem Artikel führen Sie Folgendes durch:

> [!div class="checklist"]
> + Installieren der erforderlichen Komponenten
> + Wiederherstellen der Beispieldatenbank

In [Teil zwei](r-taxi-classification-explore-data.md) untersuchen Sie die Beispieldaten und generieren einige Plots.

In [Teil drei](r-taxi-classification-create-features.md) erfahren Sie, wie Sie mithilfe einer Transact-SQL-Funktion aus Rohdaten Features erstellen. Sie rufen anschließend die Funktion aus einer gespeicherten Prozedur auf, um eine Tabelle zu erstellen, die die Funktionswerte enthält.

In [Teil vier](r-taxi-classification-train-model.md) laden Sie die Module und rufen die erforderlichen Funktionen auf, um das Modell mithilfe einer gespeicherten SQL Server-Prozedur zu erstellen und zu trainieren.

In Teil fünf erfahren Sie, wie Sie die Modelle [operationalisieren](r-taxi-classification-deploy-model.md) können, die Sie in Teil vier trainiert und gespeichert haben.

> [!NOTE]
> Dieses Tutorial ist sowohl in R als auch in Python verfügbar. Die Python-Version finden Sie unter [Python-Tutorial: Vorhersagen von Taxi-Fahrpreisen in New York City mit binärer Klassifizierung](r-taxi-classification-introduction.md).

## <a name="prerequisites"></a>Voraussetzungen

::: moniker range="=sql-server-2016"
+ Installieren Sie [SQL Server 2016 R Services](../install/sql-r-services-windows-install.md#verify-installation)
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
+ Installieren Sie [SQL Server Machine Learning Services mit aktiviertem R](../install/sql-machine-learning-services-windows-install.md#verify-installation)
::: moniker-end

+ Installieren Sie [R-Bibliotheken](../package-management/r-package-information.md)

+ [Erteilen Sie Berechtigungen zum Ausführen von R-Skripts](../security/user-permission.md)

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
+ Ab SQL Server 2019 erfordert der Isolationsmechanismus, dass Sie für das Verzeichnis, in dem die Plotdatei gespeichert ist, entsprechende Berechtigungen erteilen. Informationen zum Festlegen dieser Berechtigungen finden Sie im [Abschnitt zu Dateiberechtigungen unter „SQL Server 2019 unter Windows: Isolationsänderungen für Machine Learning Services“](../install/sql-server-machine-learning-services-2019.md#file-permissions).
::: moniker-end

+ Stellen Sie die [Demodatenbank für Taxifahrten in New York City](demo-data-nyctaxi-in-sql.md) wieder her.

Alle Aufgaben können mithilfe von gespeicherten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Prozeduren in Azure Data Studio oder [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ausgeführt werden.

Für dieses Tutorial sollten Sie sich mit grundlegenden Datenbankvorgängen auskennen, wie z. B. dem Erstellen von Datenbanken und Tabellen, dem Importieren von Daten und dem Schreiben von SQL-Abfragen. Kenntnisse im Umgang mit R sind nicht erforderlich, und jeglicher R-Code wird bereitgestellt.

## <a name="background-for-sql-developers"></a>Hintergrund für SQL-Entwickler

Der Erstellungsprozess einer Machine Learning-Lösung ist komplex und kann den Einsatz mehrerer Tools sowie die phasenübergreifende Koordinierung von Experten bei folgenden Schritten erfordern:

+ Abrufen und Bereinigen von Daten
+ Untersuchen der Daten und Entwickeln von Modellierungsfunktionen
+ Trainieren und Optimieren des Modells
+ Bereitstellen in der Produktion

Die Entwicklung und das Testen des eigentlichen Codes werden am besten in einer dedizierten R-Entwicklungsumgebung durchgeführt. Nachdem das Skript vollständig getestet wurde, können Sie es jedoch problemlos in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe von gespeicherten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Prozeduren in der vertrauten Umgebung von Azure Data Studio oder [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] bereitstellen. Das Umbrechen von externem Code in gespeicherten Prozeduren ist der primäre Mechanismus zum Operationalisieren von Code in SQL Server.

Nach dem Speichern des Modells in der Datenbank für Vorhersagen können Sie es aus [!INCLUDE[tsql](../../includes/tsql-md.md)] mithilfe von gespeicherten Prozeduren aufrufen.

Egal, ob Sie ein SQL-Programmierer sind, der noch nicht mit R vertraut ist, oder ein R-Entwickler, für den SQL neu ist: In dieser fünfteiligen Tutorialreihe wird ein typischer Workflow für die Durchführung von datenbankinternen Analysen mit R und SQL Server vorgestellt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel führen Sie folgende Schritte aus:

> [!div class="checklist"]
> + Installieren der Voraussetzungen
> + Wiederherstellen der Beispieldatenbank

> [!div class="nextstepaction"]
> [R-Tutorial: Untersuchen und Visualisieren von Daten](r-taxi-classification-explore-data.md)