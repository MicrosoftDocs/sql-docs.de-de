---
title: 'R-Tutorial: Erstellen von Datenfeatures'
titleSuffix: SQL machine learning
description: In Teil drei dieser fünfteiligen Tutorialreihe verwenden Sie T-SQL-Funktionen zum Erstellen und Speichern von Features aus Beispieldaten mithilfe von SQL Machine Learning.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 0dfba9b2e9178d0b389d812881d27341fde7c208
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347790"
---
# <a name="r-tutorial-create-data-features"></a>R-Tutorial: Erstellen von Datenfeatures
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

In Teil drei dieser fünfteiligen Tutorialreihe erfahren Sie, wie Sie mithilfe einer [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktion Features aus Rohdaten erstellen. Anschließend rufen Sie diese Funktion aus einer gespeicherten SQL-Prozedur auf, um eine Tabelle zu erstellen, die die Funktionswerte enthält.

In diesem Artikel führen Sie Folgendes durch:

> [!div class="checklist"]
> + Ändern einer benutzerdefinierten Funktion zum Berechnen eine Fahrtstrecke
> + Speichern der Features mithilfe einer weiteren benutzerdefinierten Funktion

In [Teil 1](r-taxi-classification-introduction.md) haben Sie die Voraussetzungen installiert und die Beispieldatenbank wiederhergestellt.

In [Teil zwei](r-taxi-classification-explore-data.md) haben Sie die Beispieldaten überprüft und einige Plots generiert.

In [Teil vier](r-taxi-classification-train-model.md) laden Sie die Module und rufen die erforderlichen Funktionen auf, um das Modell mithilfe einer gespeicherten SQL Server-Prozedur zu erstellen und zu trainieren.

In Teil fünf erfahren Sie, wie Sie die Modelle [operationalisieren](r-taxi-classification-deploy-model.md) können, die Sie in Teil vier trainiert und gespeichert haben.

In Teil fünf erfahren Sie, wie Sie die Modelle [operationalisieren](./python-taxi-classification-deploy-model.md) können, die Sie in Teil vier trainiert und gespeichert haben.

## <a name="about-feature-engineering"></a>Über die Featureentwicklung

Nachdem Sie eine Zeit damit verbracht haben, Daten zu durchsuchen, haben Sie einige Einsichten in die Daten gesammelt und sind nun bereit, mit der *Featureentwicklung* fortzufahren. Das Erstellen nützlicher Features aus den Rohdaten ist ein wichtiger Schritt bei der Erstellung von Analysemodellen.

In diesem Dataset basieren die Entfernungswerte auf der vom Taxameter angezeigten Entfernung und stellen nicht zwangsläufig die geografische Distanz oder tatsächlich zurückgelegte Entfernung dar. Aus diesem Grund müssen Sie die direkte Entfernung zwischen den Abhol- und den Zielorten berechnen, indem Sie die verfügbaren Koordinaten in im Quell-Dataset NYC Taxi verwenden. Sie erreichen dies, indem Sie die [Haversine-Formel](https://en.wikipedia.org/wiki/Haversine_formula) in einer benutzerdefinierten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Funktion verwenden.

Verwenden Sie eine benutzerdefinierte T-SQL-Funktion, _fnCalculateDistance_, um die Entfernung mithilfe der Haversine-Formel zu berechnen, und verwenden Sie eine zweite benutzerdefinierte T-SQL-Funktion, _fnEngineerFeatures_, um eine Tabelle mit allen Funktionen zu erstellen.

Der Gesamtprozess sieht folgendermaßen aus:

+ Erstellen Sie die T-SQL-Funktion, die die Berechnungen durchführt.

+ Rufen Sie die Funktion auf, um die Featuredaten zu generieren.

+ Speichern Sie die Featuredaten in einer Tabelle.

## <a name="calculate-trip-distance-using-fncalculatedistance"></a>Berechnen der Fahrstrecke mithilfe von fnCalculateDistance

Die Funktion _fnCalculateDistance_ sollte heruntergeladen und bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als Teil der Vorbereitung für dieses Tutorial registriert worden sein. Überprüfen Sie kurz den Code.
  
1. Erweitern Sie in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]**Programmierbarkeit**, erweitern Sie **Funktionen** und anschließend **Skalarwertfunktionen**.   

2. Klicken Sie mit der rechten Maustaste auf _fnCalculateDistance_, und wählen Sie **Ändern** aus, um das [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skript in einem neuen Abfragefenster zu öffnen.
  
   ```sql
   CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)  
   -- User-defined function that calculates the direct distance between two geographical coordinates.  
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
  
   + Die Funktion ist eine Skalarwertfunktion, die einen einzelnen Datenwert eines vordefinierten Typs zurückgibt.
  
   + Sie übernimmt die Werte der Längen- und Breitengrade als Eingaben, die sie von den Abhol- und Zielorten der Fahrten erhält. Die Haversine-Formel wandelt Orte ins Bogenmaß um und verwendet diese Werte, um die direkte Entfernung zwischen zwei Orten in Meilen zu berechnen.

## <a name="generate-the-features-using-_fnengineerfeatures_"></a>Generieren der Features mithilfe von _fnEngineerFeatures_

Verwenden Sie eine andere Funktion, _fnEngineerFeatures_, um die berechneten Werte einer Tabelle hinzuzufügen, die zum Trainieren des Modells verwendet werden kann. Die neue Funktion ruft die zuvor erstellte T-SQL-Funktion _fnCalculateDistance_ auf, um die direkte Entfernung zwischen den Abhol- und Zielorten abzurufen. 

1. Nehmen Sie sich ein paar Minuten Zeit, um den Code für die benutzerdefinierte T-SQL-Funktion _fnEngineerFeatures_ zu überprüfen, die für Sie als Teil der Vorbereitung für diese exemplarische Vorgehensweise erstellt wurde.
  
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

   + Diese Tabellenwertfunktion akzeptiert mehrere Spalten als Eingaben und gibt eine Tabelle mit mehreren Featurespalten aus.

   + Der Zweck dieser Funktion ist die Erstellung neuer Features, die beim Erstellen eines Modells verwendet werden können.

2. Sie können sicherstellen, dass diese Funktion funktioniert, indem Sie sie zum Berechnen der geografischen Distanz für diese Fahrten verwenden, bei denen die gemessene Distanz 0 war, aber die Abhol- und Zielorte unterschiedlich waren.
  
   ```sql
       SELECT tipped, fare_amount, passenger_count,(trip_time_in_secs/60) as TripMinutes,
       trip_distance, pickup_datetime, dropoff_datetime,
       dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) AS direct_distance
       FROM nyctaxi_sample
       WHERE pickup_longitude != dropoff_longitude and pickup_latitude != dropoff_latitude and trip_distance = 0
       ORDER BY trip_time_in_secs DESC
   ```
  
   Wie Sie sehen können, entspricht die vom Taxameter angezeigte Entfernung nicht immer der geografischen Distanz. Daher ist die Featureentwicklung so wichtig. Sie können diese Datenfeatures verwenden, um ein Machine Learning-Modell mit R zu trainieren.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel führen Sie folgende Schritte aus:

> [!div class="checklist"]
> + Ändern einer benutzerdefinierten Funktion zum Berechnen einer Fahrtstrecke
> + Speichern der Features mithilfe einer weiteren benutzerdefinierten Funktion

> [!div class="nextstepaction"]
> [R-Tutorial: Trainieren und Speichern des Modells](r-taxi-classification-train-model.md)