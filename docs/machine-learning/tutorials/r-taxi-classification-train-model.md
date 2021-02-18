---
title: 'R-Tutorial: Trainieren und Speichern des Modells'
titleSuffix: SQL machine learning
description: Im vierten Teil dieser fünfteiligen Tutorialreihe trainieren und speichern Sie ein Modell in R mithilfe von Transact-SQL in SQL Server mit SQL Machine Learning.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 3b5606b9f14006632bfa2782d1d547b4565e36ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350929"
---
# <a name="r-tutorial-train-and-save-model"></a>R-Tutorial: Trainieren und Speichern des Modells
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

In Teil 4 dieser auf fünf Teilen bestehenden Tutorialreihe erfahren Sie, wie Sie ein Machine Learning-Modell mit R trainieren. Sie trainieren das Modell mit den Datenfunktionen, die Sie im vorherigen Teil erstellt haben, und speichern das trainierte Modell dann in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Tabelle. In diesem Fall sind die R-Pakete bereits mit [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] installiert, sodass alles über SQL ausgeführt werden kann.

In diesem Artikel führen Sie Folgendes durch:

> [!div class="checklist"]
> + Erstellen und Trainieren eines Modells mithilfe einer gespeicherten SQL-Prozedur
> + Speichern des trainierten Modells in einer SQL-Tabelle

In [Teil 1](r-taxi-classification-introduction.md) haben Sie die Voraussetzungen installiert und die Beispieldatenbank wiederhergestellt.

In [Teil zwei](r-taxi-classification-explore-data.md) haben Sie die Beispieldaten überprüft und einige Plots generiert.

In [Teil drei](r-taxi-classification-create-features.md) haben Sie gelernt, wie Sie mithilfe einer Transact-SQL-Funktion aus Rohdaten Features erstellen. Anschließend haben Sie die Funktion aus einer gespeicherten Prozedur aufgerufen, um eine Tabelle zu erstellen, die die Funktionswerte enthält.

In Teil fünf erfahren Sie, wie Sie die Modelle [operationalisieren](r-taxi-classification-deploy-model.md) können, die Sie in Teil vier trainiert und gespeichert haben.

## <a name="create-the-stored-procedure"></a>Erstellen der gespeicherten Prozedur

Wenn Sie R über T-SQL aufrufen, verwenden Sie die im System gespeicherte Prozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Für Prozesse, die Sie häufig wiederholen, wie z. B. das erneute Trainieren eines Modells, ist es jedoch einfacher, den Aufruf von `sp_execute_external_script` in einer anderen gespeicherten Prozedur zu kapseln.

1. Öffnen Sie in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ein neues Fenster **Abfrage**.

2. Führen Sie die folgende Anweisung aus, um die gespeicherte Prozedur **RTrainLogitModel** zu erstellen. Diese gespeicherte Prozedur definiert die Eingabedaten und verwendet **glm**, um ein logistisches Regressionsmodell zu erstellen.

   ```sql
   CREATE PROCEDURE [dbo].[RTrainLogitModel] (@trained_model varbinary(max) OUTPUT)
   
   AS
   BEGIN
     DECLARE @inquery nvarchar(max) = N'
       select tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance,
       pickup_datetime, dropoff_datetime,
       dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
       from nyctaxi_sample
       tablesample (70 percent) repeatable (98052)
   '
   
     EXEC sp_execute_external_script @language = N'R',
                                     @script = N'
   ## Create model
   logitObj <- glm(tipped ~ passenger_count + trip_distance + trip_time_in_secs + direct_distance, data = InputDataSet, family = binomial)
   summary(logitObj)
   
   ## Serialize model 
   trained_model <- as.raw(serialize(logitObj, NULL));
   ',
     @input_data_1 = @inquery,
     @params = N'@trained_model varbinary(max) OUTPUT',
     @trained_model = @trained_model OUTPUT; 
   END
   GO
   ```

   + Um sicherzustellen, dass einige Daten zum Testen des Modells übrig bleiben, werden 70 % der Daten aus der Datentabelle „Taxi“ zu Trainingszwecken nach dem Zufallsprinzip ausgewählt.

   + Die SELECT-Abfrage verwendet die benutzerdefinierte Skalarfunktion *fnCalculateDistance* zum Berechnen der direkten Entfernung zwischen den Abhol- und Zielorten. Die Ergebnisse der Abfrage werden in der Standardeingabevariable von R, `InputDataset`, gespeichert.
  
   + In dem R-Skript wird die R-Funktion **glm** aufgerufen, um das logistische Regressionsmodell zu erstellen.
  
     Die binäre Variable _tipped_ dient als die Spalte *label* oder „outcome“, und das Modell wird mithilfe folgender Funktionsspalten angepasst:  _passenger_count_, _trip_distance_, _trip_time_in_secs_ und _direct_distance_.
  
   + Das trainierte Modell, das in der R-Variablen `logitObj` gespeichert ist, wird serialisiert und als Ausgabeparameter zurückgegeben.

## <a name="train-and-deploy-the-r-model-using-the-stored-procedure"></a>Trainieren und Bereitstellen des R-Modells mithilfe der gespeicherten Prozedur

Da die gespeicherte Prozedur schon eine Definition der Eingabedaten enthält, müssen Sie keine Eingabeabfrage bereitstellen.

1. Um das R-Modell zu trainieren und bereitzustellen, rufen Sie die gespeicherte Prozedur auf und fügen sie in die Datenbanktabelle _nyc_taxi_models_ ein, sodass Sie sie für zukünftige Vorhersagen verwenden können:

   ```sql
   DECLARE @model VARBINARY(MAX);
   EXEC RTrainLogitModel @model OUTPUT;
   INSERT INTO nyc_taxi_models (name, model) VALUES('RTrainLogit_model', @model);
   ```

2. Nachrichten, die an den **stdout**-Stream von R weitergeleitet werden würden, werden im Fenster [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]Meldungen **von** angezeigt: 

   "STDOUT message(s) from external script: Gelesene Zeilen: 1193025, Total Rows Processed: 1193025, Total Chunk Time: 0.093 seconds"

3. Wenn die Anweisung abgeschlossen ist, öffnen Sie die Tabelle *nyc_taxi_models*. Die Verarbeitung der Daten und die Anpassung des Modells können eine Weile dauern.

   Sie können sehen, dass eine neue Zeile hinzugefügt wurde, die das serialisierte Modell in der Spalte _model_ und den Modellnamen **RTrainLogit_model** in der Spalte _name_ enthält.

   ```text
   model                        name
   ---------------------------- ------------------
   0x580A00000002000302020....  RTrainLogit_model
   ```

Im nächsten Teil dieses Tutorials werden Sie mithilfe des trainierten Modells Vorhersagen erstellen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel führen Sie folgende Schritte aus:

> [!div class="checklist"]
> + Erstellen und Trainieren eines Modells mithilfe einer gespeicherten SQL-Prozedur
> + Speichern des trainierten Modells in einer SQL-Tabelle

> [!div class="nextstepaction"]
> [R-Tutorial: Ausführen von Vorhersagen in gespeicherten SQL-Prozeduren](r-taxi-classification-deploy-model.md)
