---
title: 'Python-Tutorial: Vorbereiten von Daten'
titleSuffix: SQL machine learning
description: Im zweiten Teil dieser vierteiligen Tutorialreihe verwenden Sie Python, um Daten für die Vorhersage von Skiverleihen mit SQL Machine Learning vorzubereiten.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/26/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: b1cce65d71e686876370b6a978d660d0f75a1d66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351476"
---
# <a name="python-tutorial-prepare-data-to-train-a-linear-regression-model-with-sql-machine-learning"></a>Python-Tutorial: Vorbereiten von Daten für das Training eines linearen Regressionsmodells mit SQL Machine Learning
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Im zweiten Teil dieser vierteiligen Tutorialreihe bereiten Sie Daten aus einer Datenbank mithilfe von Python vor. Diese Daten verwenden Sie in einem späteren Teil dieser Reihe zum Trainieren und Bereitstellen eines linearen Regressionsmodells in Python mit SQL Server Machine Learning Services oder in Big Data-Clustern.
::: moniker-end
::: moniker range="=sql-server-2017"
Im zweiten Teil dieser vierteiligen Tutorialreihe bereiten Sie Daten aus einer Datenbank mithilfe von Python vor. Diese Daten verwenden Sie in einem späteren Teil dieser Reihe zum Trainieren und Bereitstellen eines linearen Regressionsmodells in Python mit SQL Server-Machine Learning Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Im zweiten Teil dieser vierteiligen Tutorialreihe bereiten Sie Daten aus einer Datenbank mithilfe von Python vor. Diese Daten verwenden Sie in einem späteren Teil dieser Reihe zum Trainieren und Bereitstellen eines linearen Regressionsmodells in Python mit Machine Learning Services in Azure SQL Managed Instance.
::: moniker-end

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Laden der Daten aus der Datenbank in einen **pandas**-Datenrahmen
> * Vorbereiten der Daten in Python durch Entfernen einiger Spalten

In [Teil 1](python-ski-rental-linear-regression.md) dieser Tutorialreihe haben Sie gelernt, wie Sie die Beispieldatenbank wiederherstellen.

In [Teil 3](python-ski-rental-linear-regression-train-model.md) trainieren Sie ein lineares Regressionsmodell für Machine Learning in Python.

In [Teil 4](python-ski-rental-linear-regression-deploy-model.md) haben Sie gelernt, wie Sie das Modell in einer Datenbank speichern und gespeicherte Prozeduren aus den Python-Skripts erstellen, die Sie in Teil 2 und 3 entwickelt haben. Die gespeicherten Prozeduren werden auf dem Server ausgeführt, um Vorhersagen basierend auf neuen Daten treffen zu können.

## <a name="prerequisites"></a>Voraussetzungen

* In diesem Teil der Tutorialreihe wird davon ausgegangen, dass Sie [Teil 1](python-ski-rental-linear-regression.md) und die erforderlichen Voraussetzungen abgeschlossen haben.

## <a name="explore-and-prepare-the-data"></a>Durchsuchen und Vorbereiten der Daten

Laden Sie die Daten aus der Datenbank in einen Pandas-Datenrahmen, um sie in Python verwenden zu können.

Erstellen Sie in Azure Data Studio ein neues Python-Notebook, und führen Sie das unten stehende Skript aus. 

Importieren Sie mit dem folgenden Python-Skript das Dataset aus der Tabelle **dbo.rental_data** in Ihrer Datenbank in den Pandas-Datenrahmen **df**:

Ersetzen Sie bei Bedarf die Verbindungsdetails in der Verbindungszeichenfolge.

```python
import pyodbc
import pandas
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error

# Connection string to your SQL Server instance
conn_str = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server}; SERVER=<server>; DATABASE=TutorialDB;UID=<username>;PWD=<password>)

query_str = 'SELECT Year, Month, Day, Rentalcount, Weekday, Holiday, Snow FROM dbo.rental_data'

df = pandas.read_sql(sql=query_str, con=conn_str)

print("Data frame:", df)

# Get all the columns from the dataframe.
columns = df.columns.tolist()

# Filter the columns to remove ones we don't want to use in the training
columns = [c for c in columns if c not in ["Year"]]
```

Das Ergebnis sollte etwa folgendermaßen aussehen:

```results
Data frame:      Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
0    2014      1   20          445        2        1     0
1    2014      2   13           40        5        0     0
2    2013      3   10          456        1        0     0
3    2014      3   31           38        2        0     0
4    2014      4   24           23        5        0     0
..    ...    ...  ...          ...      ...      ...   ...
448  2013      2   19           57        3        0     1
449  2015      3   18           26        4        0     0
450  2015      3   24           29        3        0     1
451  2014      3   26           50        4        0     1
452  2015     12    6          377        1        0     1

[453 rows x 7 columns]
```

## <a name="next-steps"></a>Nächste Schritte

In Teil 2 dieser Tutorialreihe haben Sie die folgenden Schritte ausgeführt:

* Laden der Daten aus der Datenbank in einen **pandas**-Datenrahmen
* Vorbereiten der Daten in Python durch Entfernen einiger Spalten

Fahren Sie mit Teil 3 dieser Tutorialreihe fort, um ein Machine Learning-Modell zu trainieren, das Daten aus der Datenbank „TutorialDB“ verwendet:

> [!div class="nextstepaction"]
> [Python-Tutorial: Trainieren eines linearen Regressionsmodells](python-ski-rental-linear-regression-train-model.md)