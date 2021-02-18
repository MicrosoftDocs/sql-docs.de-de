---
description: Iris-Demodaten für Python- und R-Tutorials für SQL Machine Learning
title: Iris-Demodataset für Tutorials
titleSuffix: SQL machine learning
Description: Erstellen Sie eine Datenbank, die das Iris-Dataset und Vorhersagemodelle enthält. Dieses Dataset wird in R- und Python-Tutorials für SQL Machine Learning verwendet.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 05/26/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 25cc9c7c93786dcc0d88e8bcd152196c17bf91c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347317"
---
# <a name="iris-demo-data-for-python-and-r-tutorials-with-sql-machine-learning"></a>Iris-Demodaten für Python- und R-Tutorials für SQL Machine Learning
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

In dieser Übung erstellen Sie eine Datenbank zum Speichern von Daten aus dem [Iris-Dataset](https://en.wikipedia.org/wiki/Iris_flower_data_set) sowie zum Speichern von Modellen, die auf diesen Daten basieren. Die Iris-Daten sind sowohl in den R- als auch in den Python-Distributionen enthalten und werden in Tutorials für SQL Machine Learning verwendet.

Für diese Übung benötigen Sie [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) oder ein anderes Tool zum Ausführen von T-SQL-Abfragen.

Dieses Dataset wird in folgenden Tutorials und Schnellstarts verwendet:

+ [Schnellstart: Erstellen und Bewerten eines Vorhersagemodells in Python mit SQL Server Machine Learning Services](quickstart-python-train-score-model.md)

## <a name="create-the-database"></a>Erstellen der Datenbank

1. Starten Sie SQL Server Management Studio, und öffnen Sie ein neues **Abfragefenster**.  

2. Erstellen Sie für dieses Projekt eine neue Datenbank, und ändern Sie den Kontext für Ihr **Abfragefenster** so, dass die neue Datenbank verwendet wird.

    ```sql
    CREATE DATABASE irissql
    GO
    USE irissql
    GO
    ```

3. Fügen Sie leere Tabellen hinzu: eine zum Speichern der Daten und eine zum Speichern der trainierten Modelle. Die Tabelle **iris_models** wird zum Speichern von serialisierten Modellen verwendet, die in anderen Übungen generiert werden.

    Mit dem folgenden Code wird die Tabelle für die Trainingsdaten erstellt.

    ```sql
    DROP TABLE IF EXISTS iris_data;
    GO
    CREATE TABLE iris_data (
      id INT NOT NULL IDENTITY PRIMARY KEY
      , "Sepal.Length" FLOAT NOT NULL, "Sepal.Width" FLOAT NOT NULL
      , "Petal.Length" FLOAT NOT NULL, "Petal.Width" FLOAT NOT NULL
      , "Species" VARCHAR(100) NOT NULL, "SpeciesId" INT NOT NULL
    );
    ```

4. Führen Sie den folgenden Code aus, um die zum Speichern des trainierten Modells verwendete Tabelle zu erstellen. Python- oder R-Modelle können erst in SQL Server gespeichert werden, nachdem sie serialisiert und dann in einer Spalte vom Typ **varbinary(max)** gespeichert wurden.

    ```sql
    DROP TABLE IF EXISTS iris_models;
    GO

    CREATE TABLE iris_models (
      model_name VARCHAR(50) NOT NULL DEFAULT('default model') PRIMARY KEY,
      model VARBINARY(MAX) NOT NULL
    );
    GO
    ```

    Neben dem Modellinhalt sollten Sie auch Spalten für andere nützliche Metadaten wie für den Namen des Modells, das Trainingsdatum, den Quellalgorithmus, die Quellparameter, Quelldaten usw. hinzufügen. Vorerst halten wir es einfach und verwenden nur den Modellnamen.

## <a name="populate-the-table"></a>Auffüllen der Tabelle

Rufen Sie integrierte Iris-Daten von R oder Python ab. Laden Sie die Daten mit Python oder R in einen Datenrahmen, und fügen Sie diesen in eine Tabelle in der Datenbank ein. Für das Verschieben von Trainingsdaten aus einer externen Sitzung in eine Tabelle sind mehrere Schritte erforderlich:

+ Entwerfen Sie eine gespeicherte Prozedur, mit der die gewünschten Daten abgerufen werden.
+ Führen Sie die gespeicherte Prozedur aus, um die Daten letztlich abzurufen.
+ Erstellen Sie eine INSERT-Anweisung, um anzugeben, in welchem Verzeichnis die abgerufenen Daten gespeichert werden sollen.

1. Erstellen Sie bei Systemen mit Python-Integration die folgende gespeicherte Prozedur, mit deren Hilfe die Daten mittels Python-Code geladen werden.

    ```sql
    CREATE PROCEDURE get_iris_dataset
    AS
    BEGIN
    EXEC sp_execute_external_script @language = N'Python', 
    @script = N'
    from sklearn import datasets
    iris = datasets.load_iris()
    iris_data = pandas.DataFrame(iris.data)
    iris_data["Species"] = pandas.Categorical.from_codes(iris.target, iris.target_names)
    iris_data["SpeciesId"] = iris.target
    ', 
    @input_data_1 = N'', 
    @output_data_1_name = N'iris_data'
    WITH RESULT SETS (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100) not null, "SpeciesId" int not null));
    END;
    GO
    ```

    Beim Ausführen dieses Codes wird die Meldung angezeigt, dass die Befehle erfolgreich ausgeführt wurden. Das bedeutet, dass die gespeicherte Prozedur gemäß Ihren Angaben erstellt wurde.

2. Bei Systemen mit R-Integration erstellen Sie eine Prozedur, die anstelle von Python-Code R-Code verwendet.

    ```sql
    CREATE PROCEDURE get_iris_dataset
    AS
    BEGIN
    EXEC sp_execute_external_script @language = N'R', 
    @script = N'
    library(RevoScaleR)
    data(iris)
    iris$SpeciesID <- c(unclass(iris$Species))
    iris_data <- iris
    ', 
    @input_data_1 = N'', 
    @output_data_1_name = N'iris_data'
    WITH RESULT SETS (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100) not null, "SpeciesId" int not null));
    END;
    GO
    ```

3. Um die Tabelle letztlich aufzufüllen, führen Sie die gespeicherte Prozedur aus und geben Sie die Tabelle an, in der die Daten gespeichert werden sollen. Beim Ausführen der gespeicherten Prozedur wird der Python-Code bzw. der R-Code ausgeführt, mit dem das integrierte Iris-Dataset geladen wird und die Daten in die Tabelle **iris_data** einfügt werden.

    ```sql
    INSERT INTO iris_data ("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species", "SpeciesId")
    EXEC dbo.get_iris_dataset;
    ```

    Wenn Sie mit T-SQL nicht vertraut sind, sei Ihnen gesagt, dass mit der INSERT-Anweisung nur neue Daten hinzugefügt werden. Es wird nicht geprüft, ob Daten vorhanden sind, und die Tabelle wird weder gelöscht noch neu erstellt. Um zu vermeiden, dass sich in der Tabelle mehrere Kopien derselben Daten befinden, können Sie zuerst die folgende Anweisung ausführen: `TRUNCATE TABLE iris_data`. Mit der T-SQL-Anweisung [TRUNCATE TABLE](../../t-sql/statements/truncate-table-transact-sql.md) werden vorhandene Daten gelöscht, die Struktur der Tabelle bleibt jedoch erhalten.

## <a name="query-the-data"></a>Abfragen der Daten

Führen Sie eine Abfrage aus, um zu prüfen, ob die Daten hochgeladen wurden.

1. Klicken Sie in Objekt-Explorer unter „Datenbanken“ mit der rechten Maustaste auf die Datenbank **irissql**, und starten Sie eine neue Abfrage.

2. Führen Sie einige einfache Abfragen aus:

    ```sql
    SELECT TOP(10) * FROM iris_data;
    SELECT COUNT(*) FROM iris_data;
    ```

## <a name="next-steps"></a>Nächste Schritte

Im Rahmen des folgenden Schnellstarts erstellen Sie ein Machine Learning-Modell und speichern es in einer Tabelle. Anschließend generieren Sie mithilfe des Modells vorhergesagte Ergebnisse.

+ [Schnellstart: Erstellen und Bewerten eines Vorhersagemodells in Python mit SQL Server Machine Learning Services](quickstart-python-train-score-model.md)