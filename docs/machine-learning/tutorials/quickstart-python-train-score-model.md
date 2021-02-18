---
title: 'Schnellstart: Trainieren eines Modells in Python'
titleSuffix: SQL machine learning
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie ein Vorhersagemodell mithilfe von Python erstellen und trainieren. Außerdem lernen Sie, wie Sie das Modell in einer Tabelle in Ihrer Datenbank speichern und anschließend dazu verwenden, Werte aus neuen Daten mithilfe von SQL-Machine-Learning vorherzusagen.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 85cc5fafd2efb3fba605cc80da28946bc5215941
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273001"
---
# <a name="quickstart-create-and-score-a-predictive-model-in-python-with-sql-machine-learning"></a>Schnellstart: Erstellen und Bewerten eines Vorhersagemodells in Python mit SQL-Machine-Learning
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

In dieser Schnellstartanleitung erfahren Sie, wie Sie ein Vorhersagemodell mithilfe von Python erstellen und trainieren. Sie lernen, wie Sie das Modell in Ihrer SQL Server-Instanz in einer Tabelle speichern und es anschließend verwenden, um Werte basierend auf neuen Daten mithilfe von [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md), [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview) oder [SQL Server Big Data-Cluster](../../big-data-cluster/machine-learning-services.md) vorherzusagen.

Hierzu erstellen Sie zwei gespeicherte Prozeduren, die in SQL ausgeführt werden. Die erste verwendet das klassische Irisblüten-Datenset und generiert ein Naïve Bayes-Modell, um eine Irisart anhand der Blumenmerkmale vorherzusagen. Die zweite Prozedur ist für die Bewertung vorgesehen: Sie ruft das in der ersten Prozedur generierte Modell auf, um mehrere Vorhersagen basierend auf neuen Daten auszugeben. Durch das Platzieren von Python-Code in einer gespeicherten SQL-Prozedur sind Vorgänge in SQL enthalten. Sie sind wiederverwendbar und können von anderen gespeicherten Prozeduren und Clientanwendungen aufgerufen werden.

In diesem Schnellstart lernen Sie Folgendes:

> [!div class="checklist"]
> - Einbetten von Python-Code in eine gespeicherte Prozedur
> - Übergeben von Eingaben an Ihren Code mithilfe von Eingaben in der gespeicherten Prozedur
> - Verwenden von gespeicherten Prozeduren zum Operationalisieren von Modellen

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser Schnellstartanleitung benötigen Sie folgende Voraussetzungen.

- Eine SQL-Datenbank auf einer der folgenden Plattformen:
  - [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Informationen zur Installation finden Sie im [Windows-](../install/sql-machine-learning-services-windows-install.md) oder im [Linux-Installationsleitfaden](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Big Data-Cluster für SQL Server. Weitere Informationen finden Sie unter [Aktivieren von Machine Learning Services auf SQL Server-Big Data-Clustern](../../big-data-cluster/machine-learning-services.md).
  - Machine Learning Services in Azure SQL Managed Instance. In der Übersicht [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) finden Sie weitere Informationen.

- Ein Tool zum Ausführen von SQL-Abfragen, die Python-Skripts enthalten. In dieser Schnellstartanleitung wird [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) verwendet.

- Für Beispiele in dieser Übung wird das Iris-Beispieldataset verwendet. Befolgen Sie die Anweisungen in den [Iris-Demodaten](demo-data-iris-in-sql.md), um die Beispieldatenbank **irissql** zu erstellen.

## <a name="create-a-stored-procedure-that-generates-models"></a>Erstellen einer gespeicherten Prozedur zum Generieren von Modellen

In diesem Schritt erstellen Sie eine gespeicherte Prozedur, die ein Modell zur Vorhersage von Ergebnissen generiert.

1. Öffnen Sie Azure Data Studio, stellen Sie eine Verbindung mit Ihrer SQL-Instanz her, und öffnen Sie ein neues Abfragefenster.

1. Stellen Sie eine Verbindung mit der irissql-Datenbank her.

    ```sql
    USE irissql
    GO
    ```

1. Kopieren Sie den folgenden Code, um eine neue gespeicherte Prozedur zu erstellen.

   Bei der Ausführung ruft diese Prozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) auf, um eine Python-Sitzung zu starten. 

   Eingaben, die vom Python-Code benötigt werden, werden als Eingabeparameter in dieser gespeicherten Prozedur übergeben. Als Ergebnis wird ein trainiertes Modell ausgegeben, das auf der Python-Bibliothek **scikit-learn** für den Machine Learning-Algorithmus basiert.

   In diesem Code wird [**pickle**](https://docs.python.org/2/library/pickle.html) verwendet, um das Modell zu serialisieren. Das Modell wird mit den Daten aus den Spalten 0 bis 4 der Tabelle **iris_data** trainiert. 

   Die Parameter, die Sie im zweiten Teil der Prozedur sehen, formulieren Dateneingaben und Modellausgaben. Sie möchten, dass der Python-Code weitestgehend in einer gespeicherten Prozedur ausgeführt wird, die klar definierte Ein- und Ausgaben hat, die den zur Laufzeit übergebenen Ein- und Ausgaben der gespeicherten Prozeduren zugeordnet sind.

    ```sql
    CREATE PROCEDURE generate_iris_model (@trained_model VARBINARY(max) OUTPUT)
    AS
    BEGIN
        EXECUTE sp_execute_external_script @language = N'Python'
            , @script = N'
    import pickle
    from sklearn.naive_bayes import GaussianNB
    GNB = GaussianNB()
    trained_model = pickle.dumps(GNB.fit(iris_data[["Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width"]], iris_data[["SpeciesId"]].values.ravel()))
    '
            , @input_data_1 = N'select "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "SpeciesId" from iris_data'
            , @input_data_1_name = N'iris_data'
            , @params = N'@trained_model varbinary(max) OUTPUT'
            , @trained_model = @trained_model OUTPUT;
    END;
    GO
    ```

1. Prüfen Sie, ob die gespeicherte Prozedur vorhanden ist. 

   Wenn das T-SQL-Skript aus dem vorherigen Schritt fehlerfrei ausgeführt wurde, wird eine neue gespeicherte Prozedur namens **generate_iris_model** erstellt und der Datenbank **irissql** hinzugefügt. Gespeicherte Prozeduren finden Sie im **Objekt-Explorer** von Azure Data Studio unter **Programmability**.

## <a name="execute-the-procedure-to-create-and-train-models"></a>Ausführen der Prozedur zum Erstellen und Trainieren von Modellen

In diesem Schritt führen Sie den eingebetteten Code mithilfe der Prozedur aus und erstellen ein trainiertes und serialisiertes Modell als Ausgabe. 

Modelle, die zur Wiederverwendung in Ihrer Datenbank gespeichert werden, werden als Bytestream serialisiert und in einer „VARBINARY(MAX)“-Spalte einer Datenbanktabelle gespeichert. Sobald das Modell erstellt, trainiert, serialisiert und in einer Datenbank gespeichert ist, kann es durch andere Prozeduren oder durch die [PREDICT-T-SQL](../../t-sql/queries/predict-transact-sql.md)-Funktion zur Bewertung von Workloads aufgerufen werden.

1. Führen Sie das folgende Skript aus, um die Prozedur auszuführen. Die spezifische Anweisung zum Ausführen einer gespeicherten Prozedur finden Sie in der vierten Zeile: `EXECUTE`.

   Dieses spezielle Skript löscht ein bestehendes Modell mit dem gleichen Namen („Naïve Bayes“), um Platz für neue zu schaffen, die durch erneutes Ausführen derselben Prozedur erstellt wurden. Ohne das Löschen von Modellen tritt ein Fehler auf, wenn das Objekt bereits vorhanden ist. Das Modell wird in einer Tabelle **iris_models** gespeichert, die bei der Erstellung der **irissql**-Datenbank bereitgestellt wurde.

    ```sql
    DECLARE @model varbinary(max);
    DECLARE @new_model_name varchar(50)
    SET @new_model_name = 'Naive Bayes'
    EXECUTE generate_iris_model @model OUTPUT;
    DELETE iris_models WHERE model_name = @new_model_name;
    INSERT INTO iris_models (model_name, model) values(@new_model_name, @model);
    GO
    ```

1. Stellen Sie sicher, dass das Modell eingefügt wurde.

    ```sql
    SELECT * FROM dbo.iris_models
    ```

    **Ergebnisse**

    | model_name  | model |
    |---|-----------------|
    | Naive Bayes | 0x800363736B6C6561726E2E6E616976655F62617965730A... | 

## <a name="create-and-execute-a-stored-procedure-for-generating-predictions"></a>Erstellen und Ausführen einer gespeicherten Prozedur zum Generieren von Vorhersagen

Nachdem Sie nun ein Modell erstellt, trainiert und gespeichert haben, fahren Sie mit dem nächsten Schritt fort: dem Erstellen einer gespeicherten Prozedur, die Vorhersagen generiert. Hierzu rufen Sie `sp_execute_external_script` auf, um ein Python-Skript auszuführen, das das serialisierte Modell lädt und ihm neue Dateneingaben zur Bewertung gibt.

1. Führen Sie den folgenden Code aus, um die gespeicherte Prozedur zu erstellen, die die Bewertung ausführt. Zur Laufzeit lädt diese Prozedur ein Binärmodell, verwendet die Spalten `[1,2,3,4]` als Eingaben und gibt die Spalten `[0,5,6]` als Ausgabe an.

   ```sql
   CREATE PROCEDURE predict_species (@model VARCHAR(100))
   AS
   BEGIN
       DECLARE @nb_model VARBINARY(max) = (
               SELECT model
               FROM iris_models
               WHERE model_name = @model
               );
   
       EXECUTE sp_execute_external_script @language = N'Python'
           , @script = N'
   import pickle
   irismodel = pickle.loads(nb_model)
   species_pred = irismodel.predict(iris_data[["Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width"]])
   iris_data["PredictedSpecies"] = species_pred
   OutputDataSet = iris_data[["id","SpeciesId","PredictedSpecies"]] 
   print(OutputDataSet)
   '
           , @input_data_1 = N'select id, "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "SpeciesId" from iris_data'
           , @input_data_1_name = N'iris_data'
           , @params = N'@nb_model varbinary(max)'
           , @nb_model = @nb_model
       WITH RESULT SETS((
                   "id" INT
                 , "SpeciesId" INT
                 , "SpeciesId.Predicted" INT
                   ));
   END;
   GO
   ```

2. Führen Sie die gespeicherte Prozedur aus, und geben Sie den Modellnamen „Naïve Bayes“ an, damit die Prozedur das richtige Modell verwendet.

   ```sql
   EXECUTE predict_species 'Naive Bayes';
   GO
   ```

   Sobald Sie die gespeicherte Prozedur ausgeführt haben, wird ein Python-data.frame-Ergebnis zurückgegeben. Diese T-SQL-Zeile gibt das Schema für die zurückgegebenen Ergebnisse an: `WITH RESULT SETS ( ("id" int, "SpeciesId" int, "SpeciesId.Predicted" int));`. Sie können die Ergebnisse in eine neue Tabelle einfügen oder an eine Anwendung zurückgeben.

   ![Ergebnisse aus der Ausführung einer gespeicherten Prozedur](media/train-score-using-python-NB-model-results.png)

   Als Ergebnisse werden 150 Vorhersagen über Arten, die florale Merkmale als Eingabe verwenden. Bei den meisten Beobachtungen stimmen die vorhergesagten Arten mit den tatsächlichen Arten überein.

   In diesem Beispiel wurde die Verwendung des Iris-Datasets in Python sowohl für das Training als auch für die Bewertung vereinfacht. Bei einem typischen Ansatz würde eine SQL-Abfrage durchgeführt, um die neuen Daten abzurufen und diese als `InputDataSet` an Python zu übergeben.

## <a name="conclusion"></a>Zusammenfassung

In dieser Übung haben Sie gelernt, wie Sie gespeicherte Prozeduren erstellen, die für verschiedene Tasks vorgesehen sind. Dabei wurde von jeder gespeicherten Prozedur die gespeicherte Systemprozedur `sp_execute_external_script` verwendet, um einen Python-Prozess zu starten. Eingaben für den Python-Prozess werden an `sp_execute_external` als Parameter übergeben. Sowohl das Python-Skript selbst als auch Datenvariablen in einer Datenbank werden als Eingaben übergeben.

Grundsätzlich sollten Sie nur Azure Data Studio mit hoch entwickeltem Python-Code verwenden, oder aber einfachen Python-Code, der zeilenbasierte Ausgaben zurückgibt. Als Tool unterstützt Azure Data Studio Abfragesprachen wie T-SQL und gibt flache Rowsets zurück. Wenn Ihr Code eine visuelle Ausgabe wie ein Punktdiagramm oder Histogramm erzeugt, benötigen Sie ein eigenständiges Tool oder eine Endbenutzeranwendung, die das Bild außerhalb der gespeicherten Prozedur darstellen kann.

Für einige Python-Entwickler, die es gewohnt sind, allumfassende Skripts zu schreiben, die eine Reihe von Vorgängen ausführen, mag die Organisation von Tasks in separate Prozeduren unnötig erscheinen. Training und Bewertung haben jedoch unterschiedliche Anwendungsfälle. Indem Sie diese trennen, können Sie jede Task in einem anderen Zeitplan und in Bereichsberechtigungen für jeden Vorgang platzieren.

Letztendlich ist es von Vorteil, dass die Prozesse über Parameter geändert werden können. In dieser Übung wurde der Python-Code, mit dem das Modell erstellt wurde (in diesem Beispiel „Naïve Bayes“ genannt), als Eingabe für eine zweite gespeicherte Prozedur übergeben, die das Modell in einem Bewertungsprozess aufruft. Für diese Übung wird nur ein Modell verwendet, aber Sie können sich vorstellen, wie dieses Skript bei der Parametrisierung des Modells in einer Bewertungstask sinnvoller eingesetzt werden kann.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Tutorials für Python mit SQL Machine Learning finden Sie unter:

- [Python-Tutorials](python-tutorials.md)