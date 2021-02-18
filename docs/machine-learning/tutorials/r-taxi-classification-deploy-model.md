---
title: 'R-Tutorial: Ausführen von Vorhersagen in gespeicherten SQL-Prozeduren'
titleSuffix: SQL machine learning
description: Im fünften Teil dieser fünfteiligen Tutorialreihe werden Sie ein eingebettetes R-Skript in gespeicherten SQL-Prozeduren mit T-SQL-Funktionen mit SQL Machine Learning operationalisieren.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: a3c46e719f4889ee53c98e671bd9569193b86e2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349058"
---
# <a name="r-tutorial-run-predictions-in-sql-stored-procedures"></a>R-Tutorial: Ausführen von Vorhersagen in gespeicherten SQL-Prozeduren
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Im fünften Teil dieser fünfteiligen Tutorialreihe erfahren Sie, wie Sie das Modell *operationalisieren*, das Sie im vorhergehenden Teil trainiert und gespeichert haben, indem Sie das Modell zur Vorhersage potenzieller Ergebnisse verwenden. Das Modell wird in einer gespeicherten Prozedur umschlossen, die direkt von anderen Anwendungen aufgerufen werden kann.

In diesem Artikel werden zwei Möglichkeiten zur Durchführung der Bewertung aufgezeigt:

+ **Batchbewertungsmodell:** Verwenden Sie eine SELECT-Abfrage als Eingabe für die gespeicherte Prozedur. Die gespeicherte Prozedur gibt eine Tabelle mit Beobachtungen zurück, die mit den Eingabefällen übereinstimmen.

+ **Einzelbewertungsmodus**: Übergeben Sie einen Satz von einzelnen Parameterwerten als Eingabe.  Die gespeicherte Prozedur gibt eine einzelne Zeile oder einen Wert zurück.

In diesem Artikel führen Sie Folgendes durch:

> [!div class="checklist"]
> + Erstellen und Verwenden von gespeicherten Prozeduren für die Batchbewertung
> + Erstellen und Verwenden von gespeicherten Prozeduren zur Bewertung einer einzelnen Zeile

In [Teil 1](r-taxi-classification-introduction.md) haben Sie die Voraussetzungen installiert und die Beispieldatenbank wiederhergestellt.

In [Teil zwei](r-taxi-classification-explore-data.md) haben Sie die Beispieldaten überprüft und einige Plots generiert.

In [Teil drei](r-taxi-classification-create-features.md) haben Sie gelernt, wie Sie mithilfe einer Transact-SQL-Funktion aus Rohdaten Features erstellen. Anschließend haben Sie die Funktion aus einer gespeicherten Prozedur aufgerufen, um eine Tabelle zu erstellen, die die Funktionswerte enthält.

In [Teil vier](r-taxi-classification-train-model.md) haben Sie die Module geladen und die erforderlichen Funktionen aufgerufen, um das Modell mithilfe einer gespeicherten SQL Server-Prozedur zu erstellen und zu trainieren.

## <a name="basic-scoring"></a>Grundlegende Bewertung

Die gespeicherte Prozedur **RPredict** beschreibt die grundlegende Syntax für das Umschließen eines `PREDICT`-Aufrufs in einer gespeicherten Prozedur.

```sql
CREATE PROCEDURE [dbo].[RPredict] (@model varchar(250), @inquery nvarchar(max))
AS 
BEGIN 

DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);  
EXEC sp_execute_external_script @language = N'R',
  @script = N' 
    mod <- unserialize(as.raw(model));
    print(summary(mod))
    OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
    str(OutputDataSet)
    print(OutputDataSet)
    ',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max)',
  @model = @lmodel2 
  WITH RESULT SETS (("Score" float));
END
GO
```

+ Die SELECT-Anweisung ruft das serialisierte Modell aus der Datenbank ab und speichert das Modell in der R-Variable `mod` zur weiteren Verarbeitung mit R.

+ Die neuen Fälle für die Bewertung werden aus der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Abfrage abgerufen, die in `@inquery` festgelegt ist, dem ersten Parameter für die gespeicherte Prozedur. Wenn die Abfragedaten gelesen werden, werden die Zeilen im Standard-Datenrahmen, `InputDataSet`, gespeichert. Dieser Datenrahmen wird an die [PREDICT](../../t-sql/queries/predict-transact-sql.md)-Funktion übergeben, die die Bewertungen generiert.
  
  `OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));`
  
  Da ein data.frame eine einzelne Zeile enthalten kann, können Sie den gleichen Code für die Batchbewertung und die Einzelbewertung verwenden.
  
+ Der von der `PREDICT`-Funktion zurückgegebene Wert ist ein **float**-Wert, der die Wahrscheinlichkeit darstellt, dass der Fahrer ein Trinkgeld in beliebiger Höhe erhält.

## <a name="batch-scoring-a-list-of-predictions"></a>Batchbewertung (Liste von Vorhersagen)

Ein häufigeres Szenario ist das Generieren von Vorhersagen für mehrere Beobachtungen im Batchmodus. In diesem Schritt sehen wir uns an, wie die Batchbewertung funktioniert.

1. Rufen Sie zunächst eine Reihe von Eingabedaten ab, mit denen Sie arbeiten werden. Diese Abfrage erstellt eine „Top 10“-Liste der Fahrten mit Reisenden sowie anderen Funktionen, die benötigt werden, um eine Vorhersage zu erstellen.
  
   ```sql
   SELECT TOP 10 a.passenger_count AS passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude,dropoff_longitude) AS direct_distance
   
   FROM (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample)a

   LEFT OUTER JOIN

   (SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample TABLESAMPLE (70 percent) REPEATABLE (98052)    )b

   ON a.medallion=b.medallion AND a.hack_license=b.hack_license 
   AND a.pickup_datetime=b.pickup_datetime
   WHERE b.medallion IS NULL
   ```

   **Bespielergebnisse**

   ```text
   passenger_count   trip_time_in_secs    trip_distance  dropoff_datetime          direct_distance
   1                 283                  0.7            2013-03-27 14:54:50.000   0.5427964547
   1                 289                  0.7            2013-02-24 12:55:29.000   0.3797099614
   1                 214                  0.7            2013-06-26 13:28:10.000   0.6970098661
   ```

2. Erstellen Sie eine gespeicherte Prozedur mit dem Namen **RPredictBatchOutput** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].

   ```sql
   CREATE PROCEDURE [dbo].[RPredictBatchOutput] (@model varchar(250), @inquery nvarchar(max))
   AS
   BEGIN
   DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);
   EXEC sp_execute_external_script 
     @language = N'R',
     @script = N'
       mod <- unserialize(as.raw(model));
       print(summary(mod))
       OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
       str(OutputDataSet)
       print(OutputDataSet)
     ',
     @input_data_1 = @inquery,
     @params = N'@model varbinary(max)',
     @model = @lmodel2
     WITH RESULT SETS ((Score float));
   END
   ```

3. Stellen Sie den Abfragetext in einer Variable bereit, und übergeben Sie diese als Parameter an die gespeicherte Prozedur:

   ```sql
   -- Define the input data
   DECLARE @query_string nvarchar(max)
   SET @query_string='SELECT TOP 10 a.passenger_count as passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude,dropoff_longitude) AS direct_distance FROM  (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample  )a   LEFT OUTER JOIN (SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample TABLESAMPLE (70 percent) REPEATABLE (98052))b ON a.medallion=b.medallion AND a.hack_license=b.hack_license AND a.pickup_datetime=b.pickup_datetime WHERE b.medallion is null'
   
   -- Call the stored procedure for scoring and pass the input data
   EXEC [dbo].[RPredictBatchOutput] @model = 'RTrainLogit_model', @inquery = @query_string;
   ```
  
Die gespeicherte Prozedur gibt eine Reihe von Werten zurück, die die Vorhersage für jede Fahrt der „Top 10-Fahrten“ darstellt. Bei den besten Fahrten handelt es sich jedoch um Fahrten von Einzelpersonen mit einer relativ kurzen Fahrtstrecke, für die der Fahrer wahrscheinlich kein Trinkgeld erhält.

> [!TIP]
> Anstatt nur die Ergebnisse mit Informationen wie „Trinkgeld/Kein Trinkgeld“ zurückzugeben, könnten Sie auch den Wahrscheinlichkeitswert für die Vorhersage zurückgeben und anschließend auf die Spalte _Score_ eine WHERE-Klausel anwenden, um die Bewertung als „Trinkgeld“ oder „Kein Trinkgeld“ zu kategorisieren und dabei einen Schwellenwert wie z. B. 0,5 oder 0,7 verwenden. Dieser Schritt ist nicht in der gespeicherten Prozedur enthalten, aber es wäre leicht, ihn zu implementieren.

## <a name="single-row-scoring-of-multiple-inputs"></a>Einzeilige Bewertung mehrerer Eingaben

Manchmal möchten Sie mehrere Eingabewerte übergeben und basierend auf diesen Werten eine einzelne Vorhersage erhalten. Beispielsweise könnten Sie ein Excel-Arbeitsblatt, eine Webanwendung oder einen Reporting Services-Bericht einrichten, um die gespeicherte Prozedur aufzurufen und Eingaben bereitzustellen, die von Benutzern dieser Anwendungen eingegeben oder ausgewählt wurden.

In diesem Abschnitt erfahren Sie, wie Sie einzelne Vorhersagen mithilfe einer gespeicherten Prozedur erstellen, bei der mehrere Eingaben wie beispielsweise die Anzahl der Fahrgäste, die Fahrtstrecke usw. berücksichtigt werden. Die gespeicherte Prozedur erstellt eine Bewertung auf Grundlage des zuvor gespeicherten R-Modells.
  
Überprüfen Sie, ob die Daten mit den Anforderungen des R-Modells übereinstimmen, wenn Sie die gespeicherte Prozedur aus einer externen Anwendung aufrufen. Sie müssen möglicherweise sicherstellen, dass die Eingabedaten in einen R-Datentyp umgewandelt oder konvertiert werden können oder den Datentyp und die Datenlänge überprüfen.

1. Erstellen Sie die gespeicherte Prozedur **RPredictSingleRow**.
  
   ```sql
   CREATE PROCEDURE [dbo].[RPredictSingleRow] @model varchar(50), @passenger_count int = 0, @trip_distance float = 0, @trip_time_in_secs int = 0, @pickup_latitude float = 0, @pickup_longitude float = 0, @dropoff_latitude float = 0, @dropoff_longitude float = 0
   AS
   BEGIN
   DECLARE @inquery nvarchar(max) = N'SELECT * FROM [dbo].[fnEngineerFeatures](@passenger_count, @trip_distance, @trip_time_in_secs,  @pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude)';
   DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);
   EXEC sp_execute_external_script  
     @language = N'R',
     @script = N'  
       mod <- unserialize(as.raw(model));  
       print(summary(mod));  
       OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
       str(OutputDataSet);
       print(OutputDataSet); 
       ',  
     @input_data_1 = @inquery,  
     @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,@trip_time_in_secs int ,  @pickup_latitude float ,@pickup_longitude float ,@dropoff_latitude float ,@dropoff_longitude float', @model = @lmodel2, @passenger_count =@passenger_count, @trip_distance=@trip_distance, @trip_time_in_secs=@trip_time_in_secs, @pickup_latitude=@pickup_latitude, @pickup_longitude=@pickup_longitude, @dropoff_latitude=@dropoff_latitude, @dropoff_longitude=@dropoff_longitude  
     WITH RESULT SETS ((Score float));  
   END
   ```

2. Probieren Sie es einfach aus, indem Sie die Werte manuell bereitstellen.
  
   Öffnen Sie ein neues **Abfrage**-Fenster, und rufen Sie die gespeicherte Prozedur auf, die Werte für jeden der Parameter bereitstellt. Die Parameter stellen Funktionsspalten dar, die vom Modell verwendet werden und erforderlich sind.

   ```sql
   EXEC [dbo].[RPredictSingleRow] @model = 'RTrainLogit_model',
   @passenger_count = 1,
   @trip_distance = 2.5,
   @trip_time_in_secs = 631,
   @pickup_latitude = 40.763958,
   @pickup_longitude = -73.973373,
   @dropoff_latitude =  40.782139,
   @dropoff_longitude = -73.977303
   ```

   Oder verwenden Sie diese kürzere Form, die für [Parameter für eine gespeicherte Prozedur](../../relational-databases/stored-procedures/specify-parameters.md) unterstützt wird:
  
   ```sql
   EXEC [dbo].[RPredictSingleRow] 'RTrainLogit_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

3. Die Ergebnisse geben an, dass die Wahrscheinlichkeit eines Trinkgelds für diese „Top 10-Fahrten“ sehr niedrig ist, da hier nur Einzelpersonen über eine relativ kurze Entfernung mitgefahren sind.

## <a name="conclusions"></a>Zusammenfassung

Nachdem Sie erfahren haben, wie Sie R-Code in gespeicherte Prozeduren einbetten, können Sie diese Vorgehensweisen erweitern, um eigene Modelle zu erstellen. Die Integration mit [!INCLUDE[tsql](../../includes/tsql-md.md)] macht es viel einfacher, R-Modelle für Vorhersagen bereitzustellen und das erneute Trainieren von Modellen im Rahmen eines Unternehmensdatenworkflows zu integrieren.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel führen Sie folgende Schritte aus:

> [!div class="checklist"]
> + Sie haben gespeicherte Prozeduren für die Batchbewertung erstellt und verwendet.
> + Sie haben gespeicherte Prozeduren zur Bewertung einer einzelnen Zeile erstellt und verwendet.

Weitere Informationen zu R finden Sie unter [R-Erweiterung in SQL Server](../concepts/extension-r.md).