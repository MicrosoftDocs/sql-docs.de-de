---
title: 'Python-Tutorial: Trainieren des Modells'
titleSuffix: SQL machine learning
description: Im dritten Teil dieser vierteiligen Tutorialreihe trainieren Sie ein lineares Regressionsmodell in Python, um mit SQL Machine Learning vorherzusagen, wie viele Ski verliehen werden.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: cb31c5486668daf64b86df50ecb493a778a9b61a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351458"
---
# <a name="python-tutorial-train-a-linear-regression-model-with-sql-machine-learning"></a>Python-Tutorial: Trainieren eines linearen Regressionsmodells mit SQL Machine Learning
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In Teil 3 dieser vierteiligen Tutorialreihe trainieren Sie ein lineares Regressionsmodell in Python. Im nächsten Teil dieser Reihe stellen Sie dann dieses Modell in einer SQL Server-Datenbank mit Machine Learning Services oder in Big Data-Clustern bereit.
::: moniker-end
::: moniker range="=sql-server-2017"
In Teil 3 dieser vierteiligen Tutorialreihe trainieren Sie ein lineares Regressionsmodell in Python. Im nächsten Teil dieser Reihe stellen Sie dann dieses Modell in einer SQL Server-Datenbank-Instanz mit Machine Learning Services bereit.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In Teil 3 dieser vierteiligen Tutorialreihe trainieren Sie ein lineares Regressionsmodell in Python. Im nächsten Teil dieser Reihe stellen Sie dieses Modell in einer Instanz von Azure SQL Managed Instance mit Machine Learning Services bereit.
::: moniker-end

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Trainieren eines linearen Regressionsmodells
> * Treffen von Vorhersagen mit dem linearen Regressionsmodell

In [Teil 1](python-ski-rental-linear-regression.md) dieser Tutorialreihe haben Sie gelernt, wie Sie die Beispieldatenbank wiederherstellen.

In [Teil 2](python-ski-rental-linear-regression-prepare-data.md) haben Sie erfahren, wie Sie die Daten aus einer Datenbank in einen Python-Datenrahmen laden und in Python vorbereiten.

In [Teil 4](python-ski-rental-linear-regression-deploy-model.md) haben Sie gelernt, wie Sie das Modell in einer Datenbank speichern und gespeicherte Prozeduren aus den Python-Skripts erstellen, die Sie in Teil 2 und 3 entwickelt haben. Die gespeicherten Prozeduren werden auf dem Server ausgeführt, um Vorhersagen basierend auf neuen Daten treffen zu können.

## <a name="prerequisites"></a>Voraussetzungen

* In diesem Teil der Tutorialreihe wird davon ausgegangen, dass Sie [Teil 1](python-ski-rental-linear-regression.md) und die erforderlichen Voraussetzungen abgeschlossen haben.

## <a name="train-the-model"></a>Trainieren des Modells

Für die Vorhersage müssen Sie eine Funktion (ein Modell) finden, die die Abhängigkeit zwischen den Variablen in unserem Dataset am besten beschreibt. Dies wird als Training des Modells bezeichnet. Das Trainingsdataset ist eine Teilmenge des gesamten Datensets aus dem Pandas-Datenrahmen **df**, den Sie im zweiten Teil dieser Reihe erstellt haben.

Dann trainieren Sie das Modell **lin_model** mit einem linearen Regressionsalgorithmus.

```python
# Store the variable we'll be predicting on.
target = "Rentalcount"

# Generate the training set.  Set random_state to be able to replicate results.
train = df.sample(frac=0.8, random_state=1)

# Select anything not in the training set and put it in the testing set.
test = df.loc[~df.index.isin(train.index)]

# Print the shapes of both sets.
print("Training set shape:", train.shape)
print("Testing set shape:", test.shape)

# Initialize the model class.
lin_model = LinearRegression()

# Fit the model to the training data.
lin_model.fit(train[columns], train[target])
```

Das Ergebnis sollte etwa folgendermaßen aussehen:

```results
Training set shape: (362, 7)
Testing set shape: (91, 7)
```

## <a name="make-predictions"></a>Erstellen von Vorhersagen

Verwenden Sie eine Vorhersagefunktion, um die Verleihzahlen mithilfe des Modells **lin_model** vorherzusagen.

```python
# Generate our predictions for the test set.
lin_predictions = lin_model.predict(test[columns])
print("Predictions:", lin_predictions)
# Compute error between our test predictions and the actual values.
lin_mse = mean_squared_error(lin_predictions, test[target])
print("Computed error:", lin_mse)
```

Das Ergebnis sollte etwa folgendermaßen aussehen:

```results
Predictions: [ 40.  38. 240.  39. 514.  48. 297.  25. 507.  24.  30.  54.  40.  26.
  30.  34.  42. 390. 336.  37.  22.  35.  55. 350. 252. 370. 499.  48.
  37. 494.  46.  25. 312. 390.  35.  35. 421.  39. 176.  21.  33. 452.
  34.  28.  37. 260.  49. 577. 312.  24.  24. 390.  34.  64.  26.  32.
  33. 358. 348.  25.  35.  48.  39.  44.  58.  24. 350. 651.  38. 468.
  26.  42. 310. 709. 155.  26. 648. 617.  26. 846. 729.  44. 432.  25.
  39.  28. 325.  46.  36.  50.  63.]
Computed error: 2.9960763804270902e-27
```

## <a name="next-steps"></a>Nächste Schritte

In Teil 3 dieser Tutorialreihe haben Sie die folgenden Schritte ausgeführt:

* Trainieren eines linearen Regressionsmodells
* Treffen von Vorhersagen mit dem linearen Regressionsmodell

Fahren Sie mit Teil 4 dieser Tutorialreihe fort, um das von Ihnen erstellte Machine Learning-Modell einzusetzen:

> [!div class="nextstepaction"]
> [Python-Tutorial: Bereitstellen eines Machine Learning-Modells](python-ski-rental-linear-regression-deploy-model.md)
