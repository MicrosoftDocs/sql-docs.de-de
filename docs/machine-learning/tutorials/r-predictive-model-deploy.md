---
title: 'Tutorial: Bereitstellen eines Vorhersagemodells in R'
titleSuffix: SQL machine learning
description: In Teil 4 dieses vierteiligen Tutorials stellen Sie ein Vorhersagemodell in R mit SQL Machine Learning bereit.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e0f264f0996a3597085c8e5103eed9ebc0ee2c27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348056"
---
# <a name="tutorial-deploy-a-predictive-model-in-r-with-sql-machine-learning"></a>Tutorial: Bereitstellen eines Vorhersagemodells in R mit SQL Machine Learning
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie ein in R entwickeltes Machine Learning-Modell in SQL Server Machine Learning Services oder Big Data-Cluster bereit.
::: moniker-end
::: moniker range="=sql-server-2017"
In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie mithilfe von Machine Learning Services ein in R entwickeltes Machine Learning-Modell in SQL Server bereit.
::: moniker-end
::: moniker range="=sql-server-2016"
In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie mithilfe von SQL Server R Services ein in R entwickeltes Machine Learning-Modell in SQL Server bereit.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie mithilfe von Machine Learning Services ein in R entwickeltes Machine Learning-Modell in Azure SQL Managed Instance bereit.
::: moniker-end

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer gespeicherten Prozedur zum Generieren des Machine Learning-Modells
> * Speichern des Modells in einer Datenbanktabelle
> * Erstellen einer gespeicherten Prozedur für Vorhersagen mithilfe des Modells
> * Ausführen des Modells mit neuen Daten

In [Teil 1](r-predictive-model-introduction.md) dieser Tutorialreihe haben Sie gelernt, wie Sie die Beispieldatenbank wiederherstellen.

In [Teil 2](r-predictive-model-prepare-data.md) haben Sie gelernt, wie Sie eine Beispieldatenbank importieren und anschließend die Daten vorbereiten, die zum Trainieren eines Vorhersagemodells in R verwendet werden sollen.

In [Teil 3](r-predictive-model-train.md) haben Sie erfahren, wie Sie mehrere Machine Learning-Modelle in R erstellen und trainieren und dann das genaueste auswählen.

## <a name="prerequisites"></a>Voraussetzungen

In Teil 4 dieses Tutorials wird vorausgesetzt, dass Sie die Voraussetzungen für [**Teil 1**](r-predictive-model-introduction.md) erfüllt und die Schritte in [**Teil 2**](r-predictive-model-prepare-data.md) und [**Teil 3**](r-predictive-model-train.md) durchgeführt haben.

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Erstellen einer gespeicherten Prozedur zum Generieren des Modells

In Teil 3 dieser Tutorialreihe haben Sie entschieden, dass ein Entscheidungsstrukturmodell (dtree) die genauesten Ergebnisse liefert. Erstellen Sie jetzt mithilfe der entwickelten R-Skripts eine gespeicherte Prozedur (`generate_rental_model`), die das dtree-Modell mithilfe von rpart aus dem R-Paket trainiert und generiert.

Führen Sie in Azure Data Studio die folgenden Befehle aus.

```sql
USE [TutorialDB]
DROP PROCEDURE IF EXISTS generate_rental_model;
GO
CREATE PROCEDURE generate_rental_model (@trained_model VARBINARY(max) OUTPUT)
AS
BEGIN
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
rental_train_data$Month   <- factor(rental_train_data$Month);
rental_train_data$Day     <- factor(rental_train_data$Day);
rental_train_data$Holiday <- factor(rental_train_data$Holiday);
rental_train_data$Snow    <- factor(rental_train_data$Snow);
rental_train_data$WeekDay <- factor(rental_train_data$WeekDay);

#Create a dtree model and train it using the training data set
library(rpart);
model_dtree <- rpart(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = rental_train_data);
#Serialize the model before saving it to the database table
trained_model <- as.raw(serialize(model_dtree, connection=NULL));
'
        , @input_data_1 = N'
            SELECT RentalCount
                 , Year
                 , Month
                 , Day
                 , WeekDay
                 , Snow
                 , Holiday
            FROM dbo.rental_data
            WHERE Year < 2015
            '
        , @input_data_1_name = N'rental_train_data'
        , @params = N'@trained_model varbinary(max) OUTPUT'
        , @trained_model = @trained_model OUTPUT;
END;
GO
```

## <a name="store-the-model-in-a-database-table"></a>Speichern des Modells in einer Datenbanktabelle

Erstellen Sie eine Tabelle in der TutorialDB-Datenbank, und speichern Sie das Modell dann in der Tabelle.

1. Erstellen Sie eine Tabelle (`rental_models`) zum Speichern des Modells.

    ```sql
    USE TutorialDB;
    DROP TABLE IF EXISTS rental_models;
    GO
    CREATE TABLE rental_models (
          model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(MAX) NOT NULL
        );
    GO
    ```

1. Speichern Sie das Modell als binäres Objekt in der Tabelle unter dem Modellnamen „DTree“.

    ```sql
    -- Save model to table
    TRUNCATE TABLE rental_models;
    
    DECLARE @model VARBINARY(MAX);
    
    EXECUTE generate_rental_model @model OUTPUT;
    
    INSERT INTO rental_models (
          model_name
        , model
        )
    VALUES (
         'DTree'
        , @model
        );
    
    SELECT *
    FROM rental_models;
    ```

## <a name="create-a-stored-procedure-that-makes-predictions"></a>Erstellen einer gespeicherten Prozedur für Vorhersagen

Erstellen Sie eine gespeicherte Prozedur (`predict_rentalcount_new`), die mithilfe des trainierten Modells und eines Satzes neuer Daten Vorhersagen macht.

```sql
-- Stored procedure that takes model name and new data as input parameters and predicts the rental count for the new data
USE [TutorialDB]
DROP PROCEDURE IF EXISTS predict_rentalcount_new;
GO
CREATE PROCEDURE predict_rentalcount_new (
      @model_name VARCHAR(100)
    , @input_query NVARCHAR(MAX)
    )
AS
BEGIN
    DECLARE @model VARBINARY(MAX) = (
            SELECT model
            FROM rental_models
            WHERE model_name = @model_name
            );

    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
#Convert types to factors
rentals$Month   <- factor(rentals$Month);
rentals$Day     <- factor(rentals$Day);
rentals$Holiday <- factor(rentals$Holiday);
rentals$Snow    <- factor(rentals$Snow);
rentals$WeekDay <- factor(rentals$WeekDay);

#Before using the model to predict, we need to unserialize it
rental_model <- unserialize(model);

#Call prediction function
rental_predictions <- predict(rental_model, rentals);
rental_predictions <- data.frame(rental_predictions);
'
        , @input_data_1 = @input_query
        , @input_data_1_name = N'rentals'
        , @output_data_1_name = N'rental_predictions'
        , @params = N'@model varbinary(max)'
        , @model = @model
    WITH RESULT SETS(("RentalCount_Predicted" FLOAT));
END;
GO
```

## <a name="execute-the-model-with-new-data"></a>Ausführen des Modells mit neuen Daten

Jetzt können Sie die gespeicherte Prozedur `predict_rentalcount_new` verwenden, um die Anzahl der Vermietungen aus neuen Daten vorherzusagen.

```sql
-- Use the predict_rentalcount_new stored procedure with the model name and a set of features to predict the rental count
EXECUTE dbo.predict_rentalcount_new @model_name = 'DTree'
    , @input_query = '
        SELECT CONVERT(INT,  3) AS Month
             , CONVERT(INT, 24) AS Day
             , CONVERT(INT,  4) AS WeekDay
             , CONVERT(INT,  1) AS Snow
             , CONVERT(INT,  1) AS Holiday
        ';
GO
```

Das Ergebnis sollte in etwa wie hier dargestellt aussehen.

```results
RentalCount_Predicted
332.571428571429
```

Sie haben erfolgreich ein Modell erstellt, trainiert und in einer Datenbank bereitgestellt. Anschließend haben Sie dieses Modell in einer gespeicherten Prozedur verwendet, um Werte basierend auf neuen Daten vorherzusagen.


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Verwendung der TutorialDB-Datenbank abgeschlossen haben, löschen Sie sie von Ihrem Server.

## <a name="next-steps"></a>Nächste Schritte

In Teil 4 dieser Tutorialreihe haben Sie Folgendes gelernt:

* Erstellen einer gespeicherten Prozedur zum Generieren des Machine Learning-Modells
* Speichern des Modells in einer Datenbanktabelle
* Erstellen einer gespeicherten Prozedur für Vorhersagen mithilfe des Modells
* Ausführen des Modells mit neuen Daten

Weitere Informationen zur Verwendung von R in Machine Learning Services finden Sie unter:

* [Ausführen einfacher R-Skripts mit SQL Server Machine Learning Services](quickstart-r-create-script.md)
* [R-Datenstrukturen, -typen und -objekte](quickstart-r-data-types-and-objects.md)
* [R-Funktionen](quickstart-r-functions.md)
