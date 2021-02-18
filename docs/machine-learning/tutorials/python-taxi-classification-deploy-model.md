---
title: 'Python-Tutorial: Ausführen von Vorhersagen in gespeicherten SQL-Prozeduren'
titleSuffix: SQL machine learning
description: Im fünften Teil dieser fünfteiligen Tutorialreihe werden Sie ein eingebettetes Python-Skript in gespeicherten SQL-Prozeduren mit T-SQL-Funktionen mit SQL Machine Learning operationalisieren.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 39717778ca2456d77b6ceff4ba72e8de06dac75f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339702"
---
# <a name="python-tutorial-run-predictions-using-python-embedded-in-a-stored-procedure"></a>Python-Tutorial: Ausführen von Vorhersagen mithilfe eines eingebetteten Python-Skripts in einer gespeicherten Prozedur
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Im fünften Teil dieser fünfteiligen Tutorialreihe erfahren Sie, wie Sie die Modelle *operationalisieren*, die Sie im vorherigen Teil trainiert und gespeichert haben.

In diesem Kontext bedeutet Operationalisierung, dass das Modell zur Bewertung in der Produktion bereitgestellt wird. Durch die Integration mit SQL Server gestaltet sich dieser Vorgang einfach, da Sie Python-Code in eine gespeicherte Prozedur einbetten können. Sie können die gespeicherte Prozedur in einer Anwendung aufrufen und die neuen Daten übergeben, um Vorhersagen für das Modell auf Grundlage neuer Eingaben zu erhalten.

In diesem Teil des Tutorials werden zwei Methoden für das Erstellen von Vorhersagen anhand eines Python-Modells vorgestellt: die Batchbewertung und die zeilenbasierte Bewertung.

+ **Batchbewertung:** Sie können mehrere Zeilen als Eingabedaten angeben, indem Sie eine SELECT-Abfrage als Argument an die gespeicherte Prozedur übergeben. Das Ergebnis ist eine Tabelle mit Beobachtungen, die mit den Eingabefällen übereinstimmen.
+ **Einzelbewertung:** Übergeben Sie individuelle Parameterwerte als Eingabe.  Die gespeicherte Prozedur gibt eine einzelne Zeile oder einen Wert zurück.

Der gesamte Python-Code für die Bewertung ist in der gespeicherten Prozedur enthalten.

In diesem Artikel führen Sie Folgendes durch:

> [!div class="checklist"]
> + Erstellen und Verwenden von gespeicherten Prozeduren für die Batchbewertung
> + Erstellen und Verwenden von gespeicherten Prozeduren zur Bewertung einer einzelnen Zeile

In [Teil 1](python-taxi-classification-introduction.md) haben Sie die Voraussetzungen installiert und die Beispieldatenbank wiederhergestellt.

Im [zweiten Teil](python-taxi-classification-explore-data.md) haben Sie die Beispieldaten untersucht und einige Plots generiert.

In [Teil drei](python-taxi-classification-create-features.md) haben Sie gelernt, wie Sie mithilfe einer Transact-SQL-Funktion aus Rohdaten Features erstellen. Anschließend haben Sie die Funktion aus einer gespeicherten Prozedur aufgerufen, um eine Tabelle zu erstellen, die die Funktionswerte enthält.

Im [vierten Teil](python-taxi-classification-train-model.md) haben Sie die Module geladen und die erforderlichen Funktionen aufgerufen, um das Modell mithilfe einer gespeicherten SQL Server-Prozedur zu erstellen und zu trainieren.

## <a name="batch-scoring"></a>Batchbewertung

Die ersten beiden gespeicherten Prozeduren beschreiben die grundlegende Syntax für das Umschließen eines Python-Vorhersageaufrufs in einer gespeicherten Prozedur. Für beide gespeicherten Prozeduren ist eine Datentabelle als Eingabe erforderlich.

+ Der Name des zu verwendenden Modells wird der gespeicherten Prozedur als Eingabeparameter übergeben. Die gespeicherte Prozedur lädt das serialisierte Modell aus der Datenbanktabelle `nyc_taxi_models` mithilfe der enthaltenen SELECT-Anweisung.
+ Das serialisierte Modell wird in der Python-Variable `mod` für die weitere Verarbeitung durch Python gespeichert.
+ Die neuen Fälle, die bewertet werden müssen, werden mit der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Abfrage in `@input_data_1` abgerufen. Wenn die Abfragedaten gelesen werden, werden die Zeilen im Standard-Datenrahmen, `InputDataSet`, gespeichert.
+ Beide gespeicherten Prozeduren verwenden Funktionen von `sklearn`, um eine Genauigkeitsmetrik (Area Under the Curve, AUC) zu berechnen. Genauigkeitsmetriken wie AUC können nur generiert werden, wenn Sie auch die Zielbezeichnung (die Spalte _tipped_) angeben. Vorhersagen benötigen keine Zielbezeichnung (Variable `y`), die Berechnung der Genauigkeitsmetrik jedoch schon.

  Wenn also keine Zielbezeichnungen für die zu bewertenden Daten vorhanden sind, können Sie die gespeicherte Prozedur so anpassen, dass die AUC-Berechnungen entfernt werden. Dann wird nur die Trinkgeldwahrscheinlichkeit anhand der Eigenschaften (Variable `X` in der gespeicherten Prozedur) zurückgegeben.

### <a name="predicttipscikitpy"></a>PredictTipSciKitPy

Führen Sie folgende T-SQL-Anweisungen aus, um die gespeicherten Prozeduren zu erstellen. Für diese gespeicherte Prozedur ist ein Modell auf Grundlage des Pakets scikit-learn erforderlich, da spezifische Funktionen dieses Pakets verwendet werden.

Der Datenrahmen mit den Eingaben wird an die `predict_proba`-Funktion des logistischen Regressionsmodells (`mod`) übergeben. Die `predict_proba`-Funktion (`probArray = mod.predict_proba(X)`) gibt einen **Gleitkommawert** zurück, der die Wahrscheinlichkeit angibt, dass ein Trinkgeld (beliebiger Höhe) gegeben wird.

```sql
DROP PROCEDURE IF EXISTS PredictTipSciKitPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSciKitPy] (@model varchar(50), @inquery nvarchar(max))
AS
BEGIN
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from sklearn import metrics

mod = pickle.loads(lmodel2)
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
y = numpy.ravel(InputDataSet[["tipped"]])

probArray = mod.predict_proba(X)
probList = []
for i in range(len(probArray)):
  probList.append((probArray[i])[1])

probArray = numpy.asarray(probList)
fpr, tpr, thresholds = metrics.roc_curve(y, probArray)
aucResult = metrics.auc(fpr, tpr)
print ("AUC on testing data is: " + str(aucResult))

OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',  
  @input_data_1 = @inquery,
  @input_data_1_name = N'InputDataSet',
  @params = N'@lmodel2 varbinary(max)',
  @lmodel2 = @lmodel2
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="predicttiprxpy"></a>PredictTipRxPy

Diese gespeicherte Prozedur verwendet die gleichen Eingaben und erstellt die gleichen Bewertungen wie die zuvor gespeicherte Prozedur, verwendet jedoch die Funktionen des in SQL Server Machine Learning Services enthaltenen Pakets **revoscalepy**.

```sql
DROP PROCEDURE IF EXISTS PredictTipRxPy;
GO

CREATE PROCEDURE [dbo].[PredictTipRxPy] (@model varchar(50), @inquery nvarchar(max))
AS
BEGIN
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from sklearn import metrics
from revoscalepy.functions.RxPredict import rx_predict;

mod = pickle.loads(lmodel2)
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
y = numpy.ravel(InputDataSet[["tipped"]])

probArray = rx_predict(mod, X)
probList = probArray["tipped_Pred"].values 

probArray = numpy.asarray(probList)
fpr, tpr, thresholds = metrics.roc_curve(y, probArray)
aucResult = metrics.auc(fpr, tpr)
print ("AUC on testing data is: " + str(aucResult))

OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @input_data_1_name = N'InputDataSet',
  @params = N'@lmodel2 varbinary(max)',
  @lmodel2 = @lmodel2
WITH RESULT SETS ((Score float));
END
GO
```

## <a name="run-batch-scoring-using-a-select-query"></a>Ausführen der Batchbewertung mithilfe einer SELECT-Abfrage

Für die gespeicherte Prozedur **PredictTipSciKitPy** und **PredictTipRxPy** sind zwei Eingabeparameter erforderlich:

+ die Abfrage, die die Daten für die Bewertung abruft
+ der Name des trainierten Modells

Indem Sie diese Argumente an die gespeicherte Prozedur übergeben, können Sie ein bestimmtes Modell auswählen oder die Daten für die Bewertung ändern.

1. Wenn Sie das Modell **scikit-learn** für die Bewertung verwenden möchten, rufen Sie die gespeicherte Prozedur **PredictTipSciKitPy** auf, und übergeben Sie den Modellnamen und die Abfragezeichenfolge als Eingabe.

   ```sql
   DECLARE @query_string nvarchar(max) -- Specify input query
     SET @query_string='
     select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance,
     dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
     from nyctaxi_sample_testing'
   EXEC [dbo].[PredictTipSciKitPy] 'SciKit_model', @query_string;
   ```

   Die gespeicherte Prozedur gibt die vorhergesagten Wahrscheinlichkeiten für jede Fahrt zurück, die mit der Eingabeabfrage übergeben wurde. 

   Wenn Sie SSMS (SQL Server Management Studio) für die Ausführung von Abfragen verwenden, werden die Wahrscheinlichkeiten als Tabelle im Bereich **Ergebnisse** angezeigt. Im Bereich **Meldungen** wird die Genauigkeitsmetrik (AUC) mit einem Wert von etwa 0,56 angezeigt.

2. Wenn Sie das Modell **revoscalepy** für die Bewertung verwenden möchten, rufen Sie die gespeicherte Prozedur **PredictTipRxPy** auf, und übergeben den Modellnamen und die Abfragezeichenfolge als Eingabe.

   ```sql
   DECLARE @query_string nvarchar(max) -- Specify input query
     SET @query_string='
     select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance,
     dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
     from nyctaxi_sample_testing'
   EXEC [dbo].[PredictTipRxPy] 'revoscalepy_model', @query_string;
   ```

## <a name="single-row-scoring"></a>Zeilenbasierte Bewertung

Es kann vorkommen, dass Sie anstelle einer Batchbewertung nur einen einzelnen Fall zur Bewertung übergeben müssen, sodass Werte von einer Anwendung abgerufen und ein einzelnes anhand dieser Werte berechnetes Ergebnis zurückgegeben wird. Beispielsweise könnten Sie ein Excel-Arbeitsblatt, eine Webanwendung oder einen Bericht einrichten, um die gespeicherte Prozedur aufzurufen und Eingaben zu übergeben, die von Benutzern getätigt oder ausgewählt wurden.

In diesem Abschnitt erfahren Sie, wie Sie einzelne Vorhersagen durch Abrufen von zwei gespeicherten Prozeduren erstellen können.

+ [PredictTipSingleModeSciKitPy](#predicttipsinglemodescikitpy) wurde für die zeilenbasierte Bewertung mithilfe des Modells scikit-learn entworfen.
+ [PredictTipSingleModeRxPy](#predicttipsinglemoderxpy) wurde für die zeilenbasierte Bewertung mithilfe des Modells revoscalepy entworfen.
+ Wenn Sie noch kein Modell trainiert haben, sollten Sie zum [fünften Teil](python-taxi-classification-train-model.md) zurückkehren.

Beide Modelle akzeptieren einzelne Werte als Eingabe, z. B. die Anzahl der Fahrgäste oder die Fahrtstrecke. `fnEngineerFeatures` wird als Tabellenwertfunktion verwendet, um die Werte für Längen- und Breitengrad aus den Eingaben in eine neue Eigenschaft zu konvertieren: die direkte Entfernung. Im [vierten Teil](python-taxi-classification-create-features.md) wird diese Tabellenwertfunktion näher erläutert.

Beide gespeicherten Prozeduren erstellen eine Bewertung anhand des Python-Modells.

> [!NOTE]
>
> Es ist wichtig, alle Eingabeeigenschaften anzugeben, die das Python-Modell benötigt, wenn Sie die gespeicherte Prozedur von einer externen Anwendung aus aufrufen. Sie können Fehler vermeiden, indem Sie die Eingabedaten in einen Python-Datentyp umwandeln oder konvertieren. Zudem können Sie den Datentyp und die Datenlänge überprüfen.

### <a name="predicttipsinglemodescikitpy"></a>PredictTipSingleModeSciKitPy

Sehen Sie sich kurz den Code der gespeicherten Prozedur an, die die Bewertung mithilfe des Modells **scikit-learn** durchführt.

```sql
DROP PROCEDURE IF EXISTS PredictTipSingleModeSciKitPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSingleModeSciKitPy] (@model varchar(50), @passenger_count int = 0,
  @trip_distance float = 0,
  @trip_time_in_secs int = 0,
  @pickup_latitude float = 0,
  @pickup_longitude float = 0,
  @dropoff_latitude float = 0,
  @dropoff_longitude float = 0)
AS
BEGIN
  DECLARE @inquery nvarchar(max) = N'
  SELECT * FROM [dbo].[fnEngineerFeatures]( 
    @passenger_count,
    @trip_distance,
    @trip_time_in_secs,
    @pickup_latitude,
    @pickup_longitude,
    @dropoff_latitude,
    @dropoff_longitude)
    '
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;

# Load model and unserialize
mod = pickle.loads(model)

# Get features for scoring from input data
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]

# Score data to get tip prediction probability as a list (of float)
probList = []
probList.append((mod.predict_proba(X)[0])[1])

# Create output data frame
OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,
    @trip_time_in_secs int ,
    @pickup_latitude float ,
    @pickup_longitude float ,
    @dropoff_latitude float ,
    @dropoff_longitude float',
    @model = @lmodel2,
    @passenger_count =@passenger_count ,
    @trip_distance=@trip_distance,
    @trip_time_in_secs=@trip_time_in_secs,
    @pickup_latitude=@pickup_latitude,
    @pickup_longitude=@pickup_longitude,
    @dropoff_latitude=@dropoff_latitude,
    @dropoff_longitude=@dropoff_longitude
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="predicttipsinglemoderxpy"></a>PredictTipSingleModeRxPy

Die folgende gespeicherte Prozedur führt die Bewertung mithilfe des Modells **revoscalepy** durch.

```sql
DROP PROCEDURE IF EXISTS PredictTipSingleModeRxPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSingleModeRxPy] (@model varchar(50), @passenger_count int = 0,
  @trip_distance float = 0,
  @trip_time_in_secs int = 0,
  @pickup_latitude float = 0,
  @pickup_longitude float = 0,
  @dropoff_latitude float = 0,
  @dropoff_longitude float = 0)
AS
BEGIN
DECLARE @inquery nvarchar(max) = N'
  SELECT * FROM [dbo].[fnEngineerFeatures]( 
    @passenger_count,
    @trip_distance,
    @trip_time_in_secs,
    @pickup_latitude,
    @pickup_longitude,
    @dropoff_latitude,
    @dropoff_longitude)
  '
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from revoscalepy.functions.RxPredict import rx_predict;

# Load model and unserialize
mod = pickle.loads(model)

# Get features for scoring from input data
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]

# Score data to get tip prediction probability as a list (of float)

probArray = rx_predict(mod, X)

probList = []
probList = probArray["tipped_Pred"].values

# Create output data frame
OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,
    @trip_time_in_secs int ,
    @pickup_latitude float ,
    @pickup_longitude float ,
    @dropoff_latitude float ,
    @dropoff_longitude float',
    @model = @lmodel2,
    @passenger_count =@passenger_count ,
    @trip_distance=@trip_distance,
    @trip_time_in_secs=@trip_time_in_secs,
    @pickup_latitude=@pickup_latitude,
    @pickup_longitude=@pickup_longitude,
    @dropoff_latitude=@dropoff_latitude,
    @dropoff_longitude=@dropoff_longitude
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="generate-scores-from-models"></a>Generieren von Bewertungen aus Modellen

Nachdem die gespeicherte Prozedur erstellt wurde, kann eine Bewertung anhand eines vorhandenen Modells generiert werden. Öffnen Sie ein neues **Abfragefenster,** und geben oder fügen Sie Parameter für die einzelnen Eigenschaftenspalten ein. Folgende sieben Werte sind für diese Eigenschaftenspalten erforderlich:

+ *passenger_count*
+ *trip_distance*
+ *trip_time_in_secs*
+ *pickup_latitude*
+ *pickup_longitude*
+ *dropoff_latitude*
+ *dropoff_longitude*

1. Führen Sie diese Anweisung aus, um eine Vorhersage mithilfe des Modells **revoscalepy** zu generieren:
  
   ```sql
   EXEC [dbo].[PredictTipSingleModeRxPy] 'revoscalepy_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

2. Führen Sie diese Anweisung aus, um eine Bewertung mithilfe des Modells **scikit-learn** zu generieren:

   ```sql
   EXEC [dbo].[PredictTipSingleModeSciKitPy] 'SciKit_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

Die Ausgabe beider Prozeduren entspricht der Wahrscheinlichkeit, dass ein Trinkgeld für die Taxifahrt mit den angegebenen Parametern bzw. Eigenschaften gegeben wird.

## <a name="conclusions"></a>Zusammenfassung

In dieser Tutorialreihe haben Sie erfahren, wie Sie mit Python-Code arbeiten, der in gespeicherten Prozeduren eingebettet ist. Die Integration mit [!INCLUDE[tsql](../../includes/tsql-md.md)] macht es einfacher, Python-Modelle für Vorhersagen bereitzustellen und das erneute Training von Modellen im Rahmen eines Unternehmensdatenworkflows zu integrieren.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel führen Sie folgende Schritte aus:

> [!div class="checklist"]
> + Sie haben gespeicherte Prozeduren für die Batchbewertung erstellt und verwendet.
> + Sie haben gespeicherte Prozeduren zur Bewertung einer einzelnen Zeile erstellt und verwendet.

Weitere Informationen über Python finden Sie unter [Python-Erweiterung in SQL Server](../concepts/extension-python.md).
