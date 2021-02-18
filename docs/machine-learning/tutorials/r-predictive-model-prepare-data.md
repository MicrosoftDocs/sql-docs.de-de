---
title: 'Tutorial: Vorbereiten von Daten zum Trainieren eines Vorhersagemodells in R'
titleSuffix: SQL machine learning
description: Im zweiten Teil dieser vierteiligen Tutorialreihe bereiten Sie die Daten für das Training eines Vorhersagemodells in R mit SQL Machine Learning vor.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e1929f0db9fb1d70fc8c370297d2d6f1c4e74513
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348081"
---
# <a name="tutorial-prepare-data-to-train-a-predictive-model-in-r-with-sql-machine-learning"></a>Tutorial: Vorbereiten von Daten für das Training eines Vorhersagemodells in R mit SQL Machine Learning
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In Teil 2 dieser vierteiligen Tutorialreihe bereiten Sie Daten aus einer Datenbank mithilfe von R vor. Später in dieser Reihe verwenden Sie diese Daten zum Trainieren und Bereitstellen eines Vorhersagemodells in R mit SQL Server Machine Learning Services oder in Big Data-Clustern.
::: moniker-end
::: moniker range="=sql-server-2017"
In Teil 2 dieser vierteiligen Tutorialreihe bereiten Sie Daten aus einer Datenbank mithilfe von R vor. Später in dieser Reihe verwenden Sie diese Daten zum Trainieren und Bereitstellen eines Vorhersagemodells in R mit SQL Server Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
In Teil 2 dieser vierteiligen Tutorialreihe bereiten Sie Daten aus einer Datenbank mithilfe von R vor. Später in dieser Reihe verwenden Sie diese Daten zum Trainieren und Bereitstellen eines Vorhersagemodells in R mit SQL Server R Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In Teil 2 dieser vierteiligen Tutorialreihe bereiten Sie Daten aus einer Datenbank mithilfe von R vor. Später in dieser Reihe verwenden Sie diese Daten zum Trainieren und Bereitstellen eines Vorhersagemodells in R mit Machine Learning Services für Azure SQL Managed Instance.
::: moniker-end

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Wiederherstellen einer Beispieldatenbank in einer Datenbank
> * Laden der Daten aus der Datenbank in einen R-Datenrahmen
> * Vorbereiten der Daten in R durch Identifizieren einiger Spalten als Kategorien

In [Teil 1](r-predictive-model-introduction.md) dieser Tutorialreihe haben Sie gelernt, wie Sie die Beispieldatenbank wiederherstellen.

In [Teil 3](r-predictive-model-train.md) erfahren Sie, wie Sie ein Machine Learning-Modell in R trainieren.

In [Teil 4](r-predictive-model-deploy.md) haben Sie gelernt, wie Sie das Modell in einer Datenbank speichern und gespeicherte Prozeduren aus den R-Skripts erstellen, die Sie in Teil 2 und 3 entwickelt haben. Die gespeicherten Prozeduren werden auf dem Server ausgeführt, um Vorhersagen basierend auf neuen Daten treffen zu können.

## <a name="prerequisites"></a>Voraussetzungen

In Teil zwei dieser Reihe von Tutorials wird angenommen, dass Sie [**Teil eins**](r-predictive-model-introduction.md) und seine Voraussetzungen abgeschlossen haben.

## <a name="load-the-data-into-a-data-frame"></a>Laden der Daten in einem neuen Datenrahmen

Zur Verwendung der Daten in R laden Sie sie aus der Datenbank in einen Datenrahmen (`rentaldata`).

Erstellen Sie eine neue RScript-Datei in RStudio, und führen Sie das folgende Skript aus. Ersetzen Sie **ServerName** durch Ihre eigenen Verbindungsinformationen.

```r
#Define the connection string to connect to the TutorialDB database
connStr <- "Driver=SQL Server;Server=ServerName;Database=TutorialDB;uid=Username;pwd=Password"


#Get the data from the table
library(RODBC)

ch <- odbcDriverConnect(connStr)

#Import the data from the table
rentaldata <- sqlFetch(ch, "dbo.rental_data")

#Take a look at the structure of the data and the top rows
head(rentaldata)
str(rentaldata)
```

Das Ergebnis sollte etwa folgendermaßen aussehen:

```results
   Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
1  2014    1     20      445         2        1      0
2  2014    2     13       40         5        0      0
3  2013    3     10      456         1        0      0
4  2014    3     31       38         2        0      0
5  2014    4     24       23         5        0      0
6  2015    2     11       42         4        0      0
'data.frame':       453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : num  2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : int  1 0 0 0 0 0 0 0 0 0 ...
$ Snow       : num  0 0 0 0 0 0 0 0 0 0 ...
```

## <a name="prepare-the-data"></a>Vorbereiten der Daten

In dieser Beispieldatenbank wurde der größte Teil der Vorbereitung bereits erledigt, aber einen Vorbereitungsschritt müssen Sie hier ausführen.
Verwenden Sie das folgende R-Skript, um drei Spalten als *Kategorien* zu identifizieren, indem Sie den Datentyp in *factor* ändern.



```r
#Changing the three factor columns to factor types
rentaldata$Holiday <- factor(rentaldata$Holiday);
rentaldata$Snow    <- factor(rentaldata$Snow);
rentaldata$WeekDay <- factor(rentaldata$WeekDay);



#Visualize the dataset after the change
str(rentaldata);
```

Das Ergebnis sollte etwa folgendermaßen aussehen:

```results
data.frame':      453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : Factor w/ 7 levels "1","2","3","4",..: 2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : Factor w/ 2 levels "0","1": 2 1 1 1 1 1 1 1 1 1 ...
$ Snow       : Factor w/ 2 levels "0","1": 1 1 1 1 1 1 1 1 1 1 ...
```

Die Daten sind jetzt für das Training vorbereitet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie nicht mit diesem Tutorial fortfahren möchten, löschen Sie die Datenbank „TutorialDB“.

## <a name="next-steps"></a>Nächste Schritte

In Teil 2 dieser Tutorialreihe haben Sie Folgendes gelernt:

* Laden der Beispieldaten in einen R-Datenframe
* Vorbereiten der Daten in R durch Identifizieren einiger Spalten als Kategorien

Fahren Sie mit Teil 3 dieser Tutorialreihe fort, um ein Machine Learning-Modell zu erstellen, das Daten aus der Datenbank „TutorialDB“ verwendet:

> [!div class="nextstepaction"]
> [Erstellen eines Vorhersagemodells in R mit SQL Machine Learning](r-predictive-model-train.md)
