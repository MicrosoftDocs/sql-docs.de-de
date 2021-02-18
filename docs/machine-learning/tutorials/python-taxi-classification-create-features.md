---
title: 'Python-Tutorial: Erstellen von Datenfeatures'
titleSuffix: SQL machine learning
description: In Teil drei dieser fünfteiligen Tutorialreihe fügen Sie gespeicherten Prozeduren Berechnungen zur Verwendung in Python Machine Learning-Modellen mit SQL Machine Learning hinzu.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 630a5db712e3bd6e288c0ac12ca767a07c3ac4ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339716"
---
# <a name="python-tutorial-create-data-features-using-t-sql"></a>Python-Tutorial: Erstellen von Datenfeatures mit T-SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

In Teil drei dieser fünfteiligen Tutorialreihe erfahren Sie, wie Sie mithilfe einer [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktion Features aus Rohdaten erstellen. Anschließend rufen Sie diese Funktion aus einer gespeicherten SQL-Prozedur auf, um eine Tabelle zu erstellen, die die Funktionswerte enthält.

Der Vorgang des *Feature Engineerings*, der Erstellung von Features aus Rohdaten, kann ein entscheidender Schritt in der Modellierung der erweiterten Analyse sein.

In diesem Artikel führen Sie Folgendes durch:

> [!div class="checklist"]
> + Ändern einer benutzerdefinierten Funktion zum Berechnen eine Fahrtstrecke
> + Speichern der Features mithilfe einer weiteren benutzerdefinierten Funktion

In [Teil 1](python-taxi-classification-introduction.md) haben Sie die Voraussetzungen installiert und die Beispieldatenbank wiederhergestellt.

In [Teil zwei](python-taxi-classification-explore-data.md) haben Sie die Beispieldaten untersucht und einige Plots generiert.

In [Teil vier](python-taxi-classification-train-model.md) laden Sie die Module und rufen die erforderlichen Funktionen auf, um das Modell mithilfe einer gespeicherten SQL Server-Prozedur zu erstellen und zu trainieren.

In Teil fünf erfahren Sie, wie Sie die Modelle [operationalisieren](python-taxi-classification-deploy-model.md) können, die Sie in Teil vier trainiert und gespeichert haben.

## <a name="define-the-function"></a>Definieren der Funktion

Die in den ursprünglichen Daten angezeigten Entfernungswerte basieren auf der vom Taxameter angezeigten Entfernung und stellen in der Regel keine geografische Distanz oder zurückgelegte Entfernung dar. Aus diesem Grund müssen Sie die direkte Entfernung zwischen den Abhol- und den Zielorten berechnen, indem Sie die verfügbaren Koordinaten in im Quell-Dataset NYC Taxi verwenden. Sie erreichen dies, indem Sie die [Haversine-Formel](https://en.wikipedia.org/wiki/Haversine_formula) in einer benutzerdefinierten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Funktion verwenden.

Verwenden Sie eine benutzerdefinierte T-SQL-Funktion, _fnCalculateDistance_, um die Entfernung mithilfe der Haversine-Formel zu berechnen, und verwenden Sie eine zweite benutzerdefinierte T-SQL-Funktion, _fnEngineerFeatures_, um eine Tabelle mit allen Funktionen zu erstellen.

### <a name="calculate-trip-distance-using-fncalculatedistance"></a>Berechnen der Fahrstrecke mithilfe von fnCalculateDistance

1. Die Funktion _fnCalculateDistance_ ist in der Beispieldatenbank enthalten. Überprüfen Sie kurz den Code.
  
2. Erweitern Sie in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]**Programmierbarkeit**, erweitern Sie **Funktionen** und anschließend **Skalarwertfunktionen**.
   Klicken Sie mit der rechten Maustaste auf _fnCalculateDistance_, und wählen Sie **Ändern** aus, um das [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skript in einem neuen Abfragefenster zu öffnen.
  
   ```sql
   CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)
   -- User-defined function that calculates the direct distance between two geographical coordinates
   RETURNS float
   AS
   BEGIN
     DECLARE @distance decimal(28, 10)
     -- Convert to radians
     SET @Lat1 = @Lat1 / 57.2958
     SET @Long1 = @Long1 / 57.2958
     SET @Lat2 = @Lat2 / 57.2958
     SET @Long2 = @Long2 / 57.2958
     -- Calculate distance
     SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
     --Convert to miles
     IF @distance <> 0
     BEGIN
       SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
     END
     RETURN @distance
   END
   GO
   ```

**Hinweise:**

+ Die Funktion ist eine Skalarwertfunktion, die einen einzelnen Datenwert eines vordefinierten Typs zurückgibt.
+ Sie übernimmt die Werte der Längen- und Breitengrade als Eingaben, die sie von den Abhol- und Zielorten der Fahrten erhält. Die Haversine-Formel wandelt Orte ins Bogenmaß um und verwendet diese Werte, um die direkte Entfernung zwischen zwei Orten in Meilen zu berechnen.

Verwenden Sie eine andere Funktion, _fnEngineerFeatures_, um die berechneten Werte einer Tabelle hinzuzufügen, die zum Trainieren des Modells verwendet werden kann.

### <a name="save-the-features-using-_fnengineerfeatures_"></a>Speichern der Features mithilfe von _fnEngineerFeatures_

1. Nehmen Sie sich eine Minute Zeit, um den Code für die benutzerdefinierte T-SQL Funktion _fnEngineerFeatures_ zu überprüfen, die in der Beispieldatenbank enthalten ist.
  
   Diese Funktion ist eine Tabellenwertfunktion, die mehrere Spalten als Eingaben annimmt und eine Tabelle mit mehreren Funktionsspalten ausgibt.  Der Zweck dieser Funktion ist die Erstellung einer Funktionsgruppe zum Verwenden beim Erstellen eines Modells. Die Funktion _fnEngineerFeatures_ ruft die zuvor erstellte T-SQL-Funktion _fnCalculateDistance_ auf, um die direkte Entfernung zwischen den Abhol- und Zielorten abzurufen.
  
   ```sql
   CREATE FUNCTION [dbo].[fnEngineerFeatures] (
   @passenger_count int = 0,
   @trip_distance float = 0,
   @trip_time_in_secs int = 0,
   @pickup_latitude float = 0,
   @pickup_longitude float = 0,
   @dropoff_latitude float = 0,
   @dropoff_longitude float = 0)
   RETURNS TABLE
   AS
     RETURN
     (
     -- Add the SELECT statement with parameter references here
     SELECT
       @passenger_count AS passenger_count,
       @trip_distance AS trip_distance,
       @trip_time_in_secs AS trip_time_in_secs,
       [dbo].[fnCalculateDistance](@pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude) AS direct_distance
     )
   GO
   ```
  
2. Um sicherzustellen, dass diese Funktion funktioniert, können Sie sie zum Berechnen der geografischen Distanz für diese Fahrten verwenden, bei denen die gemessene Distanz 0 war, aber die Abhol- und Zielorte unterschiedlich waren.
  
   ```sql
       SELECT tipped, fare_amount, passenger_count,(trip_time_in_secs/60) as TripMinutes,
       trip_distance, pickup_datetime, dropoff_datetime,
       dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) AS direct_distance
       FROM nyctaxi_sample
       WHERE pickup_longitude != dropoff_longitude and pickup_latitude != dropoff_latitude and trip_distance = 0
       ORDER BY trip_time_in_secs DESC
   ```
  
   Wie Sie sehen können, entspricht die vom Taxameter angezeigte Entfernung nicht immer der geografischen Distanz. Daher ist die Featureentwicklung wichtig.

Im nächsten Teil erfahren Sie, wie Sie diese Datenfeatures zum Erstellen und Trainieren eines Machine Learning-Modells mit Python verwenden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel führen Sie folgende Schritte aus:

> [!div class="checklist"]
> + Ändern einer benutzerdefinierten Funktion zum Berechnen einer Fahrtstrecke
> + Speichern der Features mithilfe einer weiteren benutzerdefinierten Funktion

> [!div class="nextstepaction"]
> [Python-Tutorial: Trainieren und Speichern eines Python-Modells mit T-SQL](python-taxi-classification-train-model.md)