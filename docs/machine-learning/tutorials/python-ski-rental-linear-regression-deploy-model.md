---
title: 'Python-Tutorial: Bereitstellen des Modells'
titleSuffix: SQL machine learning
description: In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie ein in Python entwickeltes Modell mithilfe von SQL Machine Learning in einer Datenbank-Instanz bereit, um Vorhersagen für einen Skiverleih zu treffen.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/06/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 4df492591109bef94802e1bf09a162eda09563c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351506"
---
# <a name="python-tutorial-deploy-a-linear-regression-model-with-sql-machine-learning"></a>Python-Tutorial: Bereitstellen eines linearen Regressionsmodells mit SQL Machine Learning
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie ein in Python entwickeltes lineares Regressionsmodell mithilfe von Machine Learning Services in einer SQL Server-Datenbank-Instanz oder in Big Data-Clustern bereit.
::: moniker-end
::: moniker range="=sql-server-2017"
In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie ein in Python entwickeltes lineares Regressionsmodell mithilfe von Machine Learning Services in einer SQL Server-Datenbank-Instanz bereit.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie ein in Python entwickeltes lineares Regressionsmodell mithilfe von Machine Learning Services in einer Azure SQL Managed Instance-Datenbank bereit.
::: moniker-end

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer gespeicherten Prozedur zum Generieren des Machine Learning-Modells
> * Speichern des Modells in einer Datenbanktabelle
> * Erstellen einer gespeicherten Prozedur für Vorhersagen mithilfe des Modells
> * Ausführen des Modells mit neuen Daten

In [Teil 1](python-ski-rental-linear-regression.md) dieser Tutorialreihe haben Sie gelernt, wie Sie die Beispieldatenbank wiederherstellen.

In [Teil 2](python-ski-rental-linear-regression-prepare-data.md) haben Sie erfahren, wie Sie die Daten aus einer Datenbank in einen Python-Datenrahmen laden und in Python vorbereiten.

In [Teil 3](python-ski-rental-linear-regression-train-model.md) haben Sie gelernt, wie Sie ein lineares Regressionsmodell für Machine Learning in Python trainieren.

## <a name="prerequisites"></a>Voraussetzungen

* In diesem Teil der Tutorialreihe wird davon ausgegangen, dass Sie [Teil 1](python-ski-rental-linear-regression.md) und die erforderlichen Voraussetzungen abgeschlossen haben.

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Erstellen einer gespeicherten Prozedur zum Generieren des Modells

Erstellen Sie nun mit den von Ihnen entwickelten Python-Skripts eine gespeicherte Prozedur (**generate_rental_py_model**), die das lineare Regressionsmodell mit LinearRegression aus scikit-learn trainiert und generiert.

Führen Sie die folgende T-SQL-Anweisung in Azure Data Studio aus, um die gespeicherte Prozedur zum Trainieren des Modells zu erstellen.

```sql
-- Stored procedure that trains and generates a Python model using the rental_data and a linear regression algorithm
DROP PROCEDURE IF EXISTS generate_rental_py_model;
go
CREATE PROCEDURE generate_rental_py_model (@trained_model varbinary(max) OUTPUT)
AS
BEGIN
    EXECUTE sp_execute_external_script
      @language = N'Python'
    , @script = N'
from sklearn.linear_model import LinearRegression
import pickle

df = rental_train_data

# Get all the columns from the dataframe.
columns = df.columns.tolist()

# Store the variable well be predicting on.
target = "RentalCount"

# Initialize the model class.
lin_model = LinearRegression()

# Fit the model to the training data.
lin_model.fit(df[columns], df[target])

# Before saving the model to the DB table, convert it to a binary object
trained_model = pickle.dumps(lin_model)'

, @input_data_1 = N'select "RentalCount", "Year", "Month", "Day", "WeekDay", "Snow", "Holiday" from dbo.rental_data where Year < 2015'
, @input_data_1_name = N'rental_train_data'
, @params = N'@trained_model varbinary(max) OUTPUT'
, @trained_model = @trained_model OUTPUT;
END;
GO
```

## <a name="store-the-model-in-a-database-table"></a>Speichern des Modells in einer Datenbanktabelle

Erstellen Sie eine Tabelle in der TutorialDB-Datenbank, und speichern Sie das Modell dann in der Tabelle.

1. Führen Sie die folgende T-SQL-Anweisung in Azure Data Studio aus, um eine Tabelle namens **dbo.rental_py_models** zu erstellen, in der das Modell gespeichert wird.

   ```sql
   USE TutorialDB;
   DROP TABLE IF EXISTS dbo.rental_py_models;
   GO
   CREATE TABLE dbo.rental_py_models (
       model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY,
       model VARBINARY(MAX) NOT NULL
   );
   GO
   ```

1. Speichern Sie das Modell als Binärobjekt mit dem Modellnamen **linear_model** in der Tabelle.

   ```sql
   DECLARE @model VARBINARY(MAX);
   EXECUTE generate_rental_py_model @model OUTPUT;
   
   INSERT INTO rental_py_models (model_name, model) VALUES('linear_model', @model);
   ```

## <a name="create-a-stored-procedure-that-makes-predictions"></a>Erstellen einer gespeicherten Prozedur für Vorhersagen

1. Erstellen Sie eine gespeicherte Prozedur **py_predict_rentalcount**, die Vorhersagen mit dem trainierten Modell und einem Satz neuer Daten bereitstellt. Führen Sie die folgende T-SQL-Anweisung in Azure Data Studio aus.

   ```sql
   DROP PROCEDURE IF EXISTS py_predict_rentalcount;
   GO
   CREATE PROCEDURE py_predict_rentalcount (@model varchar(100))
   AS
   BEGIN
       DECLARE @py_model varbinary(max) = (select model from rental_py_models where model_name = @model);
   
       EXECUTE sp_execute_external_script
                   @language = N'Python',
                   @script = N'
   
   # Import the scikit-learn function to compute error.
   from sklearn.metrics import mean_squared_error
   import pickle
   import pandas
   
   rental_model = pickle.loads(py_model)
   
   df = rental_score_data
   
   # Get all the columns from the dataframe.
   columns = df.columns.tolist()
   
   # Variable you will be predicting on.
   target = "RentalCount"
   
   # Generate the predictions for the test set.
   lin_predictions = rental_model.predict(df[columns])
   print(lin_predictions)
   
   # Compute error between the test predictions and the actual values.
   lin_mse = mean_squared_error(lin_predictions, df[target])
   #print(lin_mse)
   
   predictions_df = pandas.DataFrame(lin_predictions)
   
   OutputDataSet = pandas.concat([predictions_df, df["RentalCount"], df["Month"], df["Day"], df["WeekDay"], df["Snow"], df["Holiday"], df["Year"]], axis=1)
   '
   , @input_data_1 = N'Select "RentalCount", "Year" ,"Month", "Day", "WeekDay", "Snow", "Holiday"  from rental_data where Year = 2015'
   , @input_data_1_name = N'rental_score_data'
   , @params = N'@py_model varbinary(max)'
   , @py_model = @py_model
   with result sets (("RentalCount_Predicted" float, "RentalCount" float, "Month" float,"Day" float,"WeekDay" float,"Snow" float,"Holiday" float, "Year" float));
   
   END;
   GO
    ```

1. Erstellen Sie eine Tabelle zum Speichern der Vorhersagen.

   ```sql
   DROP TABLE IF EXISTS [dbo].[py_rental_predictions];
   GO
   
   CREATE TABLE [dbo].[py_rental_predictions](
    [RentalCount_Predicted] [int] NULL,
    [RentalCount_Actual] [int] NULL,
    [Month] [int] NULL,
    [Day] [int] NULL,
    [WeekDay] [int] NULL,
    [Snow] [int] NULL,
    [Holiday] [int] NULL,
    [Year] [int] NULL
   ) ON [PRIMARY]
   GO
   ```

1. Ausführen der gespeicherten Prozedur zum Vorhersagen von Verleihzahlen für einen Skiverleih

   ```sql
   --Insert the results of the predictions for test set into a table
   INSERT INTO py_rental_predictions
   EXEC py_predict_rentalcount 'linear_model';
   
   -- Select contents of the table
   SELECT * FROM py_rental_predictions;
   ```

   Das Ergebnis sollte etwa folgendermaßen aussehen:

   :::image type="content" source="media/python-tutorial-prediction-results.png" alt-text="Vorhersageergebnisse aus gespeicherter Prozedur":::

Sie haben erfolgreich ein Modell erstellt, trainiert und bereitgestellt. Anschließend haben Sie dieses Modell in einer gespeicherten Prozedur verwendet, um Werte basierend auf neuen Daten vorherzusagen.

## <a name="next-steps"></a>Nächste Schritte

In Teil 4 dieser Tutorialreihe haben Sie die folgenden Schritte ausgeführt:

* Erstellen einer gespeicherten Prozedur zum Generieren des Machine Learning-Modells
* Speichern des Modells in einer Datenbanktabelle
* Erstellen einer gespeicherten Prozedur für Vorhersagen mithilfe des Modells
* Ausführen des Modells mit neuen Daten

Weitere Informationen zur Verwendung von Python mit SQL Machine Learning finden Sie unter:

+ [Python-Tutorials](python-tutorials.md)
