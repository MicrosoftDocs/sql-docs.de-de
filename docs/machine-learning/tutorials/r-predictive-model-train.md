---
title: 'Tutorial: Trainieren und Vergleichen von Vorhersagemodellen in R'
titleSuffix: SQL machine learning
description: In Teil 3 dieser vierteiligen Tutorialreihe entwickeln Sie zwei Vorhersagemodelle in R mit SQL Machine Learning und wählen dann das genaueste Modell aus.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 0ae6a293ab1e7e888a83ceb3216e8ef74aa9f95a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348071"
---
# <a name="tutorial-create-a-predictive-model-in-r-with-sql-machine-learning"></a>Tutorial: Erstellen eines Vorhersagemodells in R mit SQL Machine Learning
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In Teil 3 dieser vierteiligen Tutorialreihe trainieren Sie ein Vorhersagemodell in R. Im nächsten Teil dieser Reihe stellen Sie dann dieses Modell in einer SQL Server-Datenbank mit Machine Learning Services oder in Big Data-Clustern bereit.
::: moniker-end
::: moniker range="=sql-server-2017"
In Teil 3 dieser vierteiligen Tutorialreihe trainieren Sie ein Vorhersagemodell in R. Im nächsten Teil dieser Reihe stellen Sie dann dieses Modell in einer SQL Server-Datenbank mit Machine Learning Services bereit.
::: moniker-end
::: moniker range="=sql-server-2016"
In Teil 3 dieser vierteiligen Tutorialreihe trainieren Sie ein Vorhersagemodell in R. Im nächsten Teil dieser Reihe stellen Sie dann dieses Modell in einer Datenbank mit SQL Server R Services bereit.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In Teil 3 dieser vierteiligen Tutorialreihe trainieren Sie ein Vorhersagemodell in R. Im nächsten Teil dieser Reihe stellen Sie dann dieses Modell in einer Azure SQL Managed Instance-Datenbank mit Machine Learning Services bereit.
::: moniker-end

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Trainieren von zwei Machine Learning-Modellen
> * Erstellen von Vorhersagen aus beiden Modellen
> * Vergleichen der Ergebnisse, um das genaueste Modell zu wählen

In [Teil 1](r-predictive-model-introduction.md) dieser Tutorialreihe haben Sie gelernt, wie Sie die Beispieldatenbank wiederherstellen.

In [Teil 2](r-predictive-model-prepare-data.md) haben Sie erfahren, wie Sie die Daten aus einer Datenbank in einen Python-Datenframe laden und in R vorbereiten.

In [Teil 4](r-predictive-model-deploy.md) haben Sie gelernt, wie Sie das Modell in einer Datenbank speichern und gespeicherte Prozeduren aus den Python-Skripts erstellen, die Sie in Teil 2 und 3 entwickelt haben. Die gespeicherten Prozeduren werden auf dem Server ausgeführt, um Vorhersagen basierend auf neuen Daten treffen zu können.

## <a name="prerequisites"></a>Voraussetzungen

In Teil 3 dieser Tutorialreihe wird vorausgesetzt, dass Sie die Voraussetzungen für [**Teil 1**](r-predictive-model-introduction.md) erfüllt und die Schritte in [**Teil 2**](r-predictive-model-prepare-data.md) durchgeführt haben.

## <a name="train-two-models"></a>Trainieren von zwei Modellen

Um das beste Modell für die Skiverleihdaten zu finden, erstellen Sie zwei verschiedene Modelle (lineare Regression und Entscheidungsstruktur) und stellen fest, welches genauere Vorhersagen liefert. Sie werden den Datenrahmen `rentaldata` verwenden, den Sie in Teil eins dieser Reihe erstellt haben.

```r
#First, split the dataset into two different sets:
# one for training the model and the other for validating it
train_data = rentaldata[rentaldata$Year < 2015,];
test_data  = rentaldata[rentaldata$Year == 2015,];


#Use the RentalCount column to check the quality of the prediction against actual values
actual_counts <- test_data$RentalCount;

#Model 1: Use lm to create a linear regression model, trained with the training data set
model_lm <- lm(RentalCount ~  Month + Day + WeekDay + Snow + Holiday, data = train_data);

#Model 2: Use rpart to create a decision tree model, trained with the training data set
library(rpart);
model_rpart  <- rpart(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = train_data);
```

## <a name="make-predictions-from-both-models"></a>Erstellen von Vorhersagen aus beiden Modellen

Verwenden Sie eine Vorhersagefunktion, um die Vermietungszahlen mithilfe jedes der trainierten Modelle vorherzusagen.

```r
#Use both models to make predictions using the test data set.
predict_lm <- predict(model_lm, test_data)
predict_lm <- data.frame(RentalCount_Pred = predict_lm, RentalCount = test_data$RentalCount, 
                         Year = test_data$Year, Month = test_data$Month,
                         Day = test_data$Day, Weekday = test_data$WeekDay,
                         Snow = test_data$Snow, Holiday = test_data$Holiday)

predict_rpart  <- predict(model_rpart,  test_data)
predict_rpart <- data.frame(RentalCount_Pred = predict_rpart, RentalCount = test_data$RentalCount, 
                         Year = test_data$Year, Month = test_data$Month,
                         Day = test_data$Day, Weekday = test_data$WeekDay,
                         Snow = test_data$Snow, Holiday = test_data$Holiday)

#To verify it worked, look at the top rows of the two prediction data sets.
head(predict_lm);
head(predict_rpart);
```

```results
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1         27.45858          42       2     11     4      0       0
2        387.29344         360       3     29     1      0       0
3         16.37349          20       4     22     4      0       0
4         31.07058          42       3      6     6      0       0
5        463.97263         405       2     28     7      1       0
6        102.21695          38       1     12     2      1       0
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1          40.0000          42       2     11     4      0       0
2         332.5714         360       3     29     1      0       0
3          27.7500          20       4     22     4      0       0
4          34.2500          42       3      6     6      0       0
5         645.7059         405       2     28     7      1       0
6          40.0000          38       1     12     2      1       0
```

## <a name="compare-the-results"></a>Vergleichen der Ergebnisse

Jetzt möchten Sie bestimmen, welches der Modelle die besten Vorhersagen ergibt. Eine schnelle und bequeme Möglichkeit dazu bietet eine einfache Zeichenfunktion, um den Unterschied zwischen den Ist-Werten in Ihren Trainingsdaten und den vorhergesagten Daten darzustellen.

```r
#Use the plotting functionality in R to visualize the results from the predictions
par(mfrow = c(1, 1));
plot(predict_lm$RentalCount_Pred - predict_lm$RentalCount, main = "Difference between actual and predicted. lm")
plot(predict_rpart$RentalCount_Pred  - predict_rpart$RentalCount,  main = "Difference between actual and predicted. rpart")
```

![Vergleich der beiden Modelle](./media/compare-models.png)

Offenbar ist das Entscheidungsstruktur-Modell das genauere der beiden Modelle.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie nicht mit diesem Tutorial fortfahren möchten, löschen Sie die Datenbank „TutorialDB“.

## <a name="next-steps"></a>Nächste Schritte

In Teil 3 dieser Tutorialreihe haben Sie Folgendes gelernt:

* Trainieren von zwei Machine Learning-Modellen
* Erstellen von Vorhersagen aus beiden Modellen
* Vergleichen der Ergebnisse, um das genaueste Modell zu wählen

Fahren Sie mit Teil 4 dieser Tutorialreihe fort, um das von Ihnen erstellte Machine Learning-Modell einzusetzen:

> [!div class="nextstepaction"]
> [Bereitstellen eines Vorhersagemodells in R mit SQL Machine Learning](r-predictive-model-deploy.md)
