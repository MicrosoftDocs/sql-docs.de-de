---
title: 'R-Tutorial: Feature Engineering'
description: In diesem Tutorial wird erklärt, wie Sie Datenfeatures mithilfe von SQL Server-Funktionen für datenbankinterne Analysen erstellen.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 502600980a3cb1f80eaeca64284f0a189cfcb405
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272981"
---
# <a name="create-data-features-using-r-and-sql-server-walkthrough"></a>Erstellen von Datenfeatures mithilfe von R und SQL Server (Vorgehensweise)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

Data Engineering ist ein wichtiger Teil des maschinellen Lernens. Häufig müssen Daten transformiert werden, bevor Sie sie für die Vorhersagemodellierung verwenden können. Wenn die Daten nicht über die Features verfügen, die Sie benötigen, können Sie diese aus vorhandenen Werten erstellen.

Sie möchten für diesen Modellierungstask möglicherweise lieber die Entfernung in Meilen zwischen zwei Orten haben anstatt die ungefähren Werte für den Breiten- und Längengrad des Abhol- und Zielorts. Zum Erstellen dieses Features berechnen Sie die direkte lineare Entfernung zwischen zwei Punkten mit der [Haversine-Formel](https://en.wikipedia.org/wiki/Haversine_formula).

In diesem Schritt erlernen Sie zwei unterschiedliche Methoden zum Erstellen eines Features aus Daten:

> [!div class="checklist"]
> * Mithilfe einer benutzerdefinierten R-Funktion
> * Mithilfe einer benutzerdefinierten T-SQL-Funktion in [!INCLUDE[tsql](../../includes/tsql-md.md)]

Das Ziel ist es, eine neue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datengruppe zu erstellen, die die ursprünglichen Spalten sowie das neue numerische Feature *direct_distance* enthält.

## <a name="prerequisites"></a>Voraussetzungen

Bei diesem Schritt wird eine laufende R-Sitzung basierend auf den vorherigen Schritten in dieser Vorgehensweise angenommen. Es werden die Verbindungszeichenfolgen und Datenquellenobjekte verwendet, die in diesen Schritten erstellt wurden. Die folgenden Tools und Pakete werden zum Ausführen des Skripts verwendet.

+ Rgui.exe zum Ausführen von R-Befehlen
+ Management Studio zum Ausführen von T-SQL

## <a name="featurization-using-r"></a>Featurebereitstellung mit R

Die Sprache R ist bekannt für ihre großen und vielseitigen statistischen Bibliotheken, doch Sie müssen ggf. weiterhin benutzerdefinierte Datentransformationen erstellen.

Zunächst gehen wir auf die für R-Benutzer bekannte Weise vor: Wir rufen die Daten auf den Laptop ab und führen dann die benutzerdefinierte R-Funktion *ComputeDist* aus, die die Luftlinie zwischen zwei Punkten berechnet und durch Breiten- und Längengradwerte angibt.

1. Beachten Sie, dass das zuvor erstellte Datenquellenobjekt nur die obersten 1000 Zeilen abruft. Definieren wir also eine Abfrage, mit der alle Daten abgerufen werden.

    ```R
    bigQuery <- "SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,  pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude FROM nyctaxi_sample";
    ```

2. Erstellen Sie mithilfe der Abfrage ein neues Datenquellenobjekt.

    ```R
    featureDataSource <- RxSqlServerData(sqlQuery = bigQuery,colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric", dropoff_longitude = "numeric", dropoff_latitude = "numeric", passenger_count  = "numeric", trip_distance  = "numeric", trip_time_in_secs  = "numeric", direct_distance  = "numeric"), connectionString = connStr);
    ```

    - [RxSqlServerData](/r-server/r-reference/revoscaler/rxsqlserverdata) kann entweder eine aus einer gültigen SELECT-Abfrage bestehenden Abfrage verwenden, die als Argument des _sqlQuery_-Parameters bereitgestellt wird, oder den Namen eines Tabellenobjekts, bereitgestellt als _table_-Parameter.
    
    - Wenn Sie Stichproben von Daten aus einer Tabelle durchführen möchten, müssen Sie den _sqlQuery_-Parameter verwenden, mithilfe der T-SQL-Klausel TABLESAMPLE Stichprobenparameter definieren und das _rowBuffering_-Argument auf FALSE festlegen.

3. Führen Sie den folgenden Code aus, um die benutzerdefinierte R-Funktion zu erstellen. ComputeDist verwendet zwei Paare von Breiten- und Längengradwerten, berechnet die Luftlinie zwischen diesen und gibt das Ergebnis in Meilen zurück.

    ```R
    env <- new.env();
    env$ComputeDist <- function(pickup_long, pickup_lat, dropoff_long, dropoff_lat){
      R <- 6371/1.609344 #radius in mile
      delta_lat <- dropoff_lat - pickup_lat
      delta_long <- dropoff_long - pickup_long
      degrees_to_radians = pi/180.0
      a1 <- sin(delta_lat/2*degrees_to_radians)
      a2 <- as.numeric(a1)^2
      a3 <- cos(pickup_lat*degrees_to_radians)
      a4 <- cos(dropoff_lat*degrees_to_radians)
      a5 <- sin(delta_long/2*degrees_to_radians)
      a6 <- as.numeric(a5)^2
      a <- a2+a3*a4*a6
      c <- 2*atan2(sqrt(a),sqrt(1-a))
      d <- R*c
      return (d)
    }
    ```
  
    + Die erste Zeile definiert eine neue Umgebung. In R kann eine Umgebung verwendet werden, um Namespaces z.B. in Paketen einzuschließen. Sie können die `search()` -Funktion verwenden, um die Umgebungen in Ihrem Arbeitsbereich anzuzeigen. Um die Objekte in einer bestimmten Umgebung anzuzeigen, geben Sie `ls(<envname>)`ein.
    + Die Zeilen, die mit `$env.ComputeDist` beginnen, enthalten den Code, der die Haversine-Formel definiert, die die *Großkreisentfernung* zwischen zwei Punkten auf einer Kugel berechnet.

4. Wenn Sie die Funktion definiert haben, wenden Sie sie auf die Daten an, um eine neue Featurespalte, *direct_distance*, zu erstellen. Ändern Sie jedoch vor dem Ausführen der Transformation den Computekontext in „local“ (lokal).

    ```R
    rxSetComputeContext("local");
    ```

5. Rufen Sie die Funktion [rxDataStep](/r-server/r-reference/revoscaler/rxdatastep) auf, um die Featureentwicklungsdaten abzurufen, und wenden Sie die Funktion `env$ComputeDist` auf die Daten im Arbeitsspeicher an.

    ```R
    start.time <- proc.time();
  
    changed_ds <- rxDataStep(inData = featureDataSource,
    transforms = list(direct_distance=ComputeDist(pickup_longitude,pickup_latitude, dropoff_longitude, dropoff_latitude),
    tipped = "tipped", fare_amount = "fare_amount", passenger_count = "passenger_count",
    trip_time_in_secs = "trip_time_in_secs",  trip_distance="trip_distance",
    pickup_datetime = "pickup_datetime",  dropoff_datetime = "dropoff_datetime"),
    transformEnvir = env,
    rowsPerRead=500,
    reportProgress = 3);
  
    used.time <- proc.time() - start.time;
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate features.", sep=""));
    ```

    + Die „rxDataStep“-Funktion unterstützt verschiedene Methoden zum Ändern von Daten an Ort und Stelle. Weitere Informationen finden Sie im Artikel [Transformieren und Unterteilen von Daten in Microsft R](/r-server/r/how-to-revoscaler-data-transform)
    
    Hinsichtlich „rxDataStep“ sollten Sie jedoch einige Punkte beachten: 
    
    In anderen Datenquellen können Sie die Argumente *varsToKeep* und *varsToDrop* verwenden, jedoch werden diese in SQL Server-Datenquellen nicht unterstützt. Daher haben wir in diesem Beispiel das Argument _transforms_ verwendet, um sowohl die Pass-Through-Spalten als auch die transformierten Spalten anzugeben. Außerdem kann das _inData_-Argument bei der Ausführung in einem SQL Server-Computekontext nur eine SQL Server-Datenquelle verwenden.

    Der vorangehende Code kann auch eine Warnmeldung ausgeben, wenn er für größere Datasets ausgeführt wird. Wenn die Anzahl der Zeilen mal die Anzahl der erstellten Spalten einen festgelegten Wert überschreitet (der Standardwert ist 3.000.000), gibt „rxDataStep“ eine Warnung aus und die Anzahl der Zeilen im zurückgegebenen Datenrahmen wird abgeschnitten. Sie können das _maxRowsByCols_-Argument in der „rxDataStep“-Funktion ändern, um die Warnung zu entfernen. Wenn _maxRowsByCols_ jedoch zu groß ist, können beim Laden des Datenrahmens in den Arbeitsspeicher Probleme auftreten.

7. Optional können Sie [rxGetVarInfo](/r-server/r-reference/revoscaler/rxgetvarinfo) aufrufen, um das Schema der transformierten Datenquelle zu überprüfen.

    ```R
    rxGetVarInfo(data = changed_ds);
    ```

## <a name="featurization-using-transact-sql"></a>Featurebereitstellung mit Transact-SQL

In dieser Übung lernen Sie, wie Sie dieselbe Aufgabe mithilfe von SQL-Funktionen anstelle von benutzerdefinierten R-Funktionen ausführen. 

Wechseln Sie zu [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) oder zu einem anderen Abfrage-Editor, um das T-SQL-Skript auszuführen.

1. Verwenden Sie eine SQL-Funktion mit dem Namen *fnCalculateDistance*. Die Funktion sollte bereits in der Datenbank „NYCTaxi_Sample“ vorhanden sein. Überprüfen Sie im Objekt-Explorer, ob die Funktion vorhanden ist, indem Sie zum folgenden Pfad navigieren: Datenbanken > NYCTaxi_Sample > Programmierbarkeit > Funktionen > Skalarwertfunktionen > dbo.fnCalculateDistance.

    Falls die Funktion nicht vorhanden ist, generieren Sie diese mithilfe von SQL Server Management Studio in der Datenbank „NYCTaxi_Sample“.

    ```sql
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)
    -- User-defined function calculates the direct distance between two geographical coordinates.
    RETURNS decimal(28, 10)
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
    ```

2. Führen Sie in Management Studio in einem neuen Abfragefenster die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung von einer beliebigen Anwendung aus, die [!INCLUDE[tsql](../../includes/tsql-md.md)] unterstützt, um zu sehen, wie die Funktion arbeitet.

    ```sql
    USE nyctaxi_sample
    GO

    SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) as direct_distance, pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude 
    FROM nyctaxi_sample
    ```
3. Sie können eine **INTO**-Klausel hinzufügen, die den Tabellennamen angibt, um Werte direkt in eine neue Tabelle (die Sie vorher erstellen müssen) einzufügen.

    ```sql
    USE nyctaxi_sample
    GO

    SELECT tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance, pickup_datetime, dropoff_datetime,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) as direct_distance, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude
    INTO NewFeatureTable
    FROM nyctaxi_sample
    ```

4. Sie können die SQL-Funktion auch über R-Code aufrufen. Wechseln Sie wieder zurück zu Rgui, und speichern Sie die SQL-Featurebereitstellungsabfrage in einer R-Variablen.

    ```R
    featureEngineeringQuery = "SELECT tipped, fare_amount, passenger_count,
        trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,
        dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance,
        pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude
        FROM nyctaxi_sample
        tablesample (1 percent) repeatable (98052)"
    ```
  
    > [!TIP]
    > Diese Abfrage wurde so geändert, dass sie ein kleineres Datenbeispiel abruft, damit diese exemplarische Vorgehensweise schneller durchgearbeitet werden kann. Sie können die TABLESAMPLE-Klausel entfernen, wenn Sie alle Daten abrufen möchten. Je nach Umgebung kann das vollständige Dataset möglicherweise nicht in R geladen werden, und es wird ein Fehler angezeigt.
  
5. Verwenden Sie die folgenden Codezeilen, um die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktion aus Ihrer R-Umgebung aufzurufen und sie auf die Daten anzuwenden, die in *featureEngineeringQuery* definiert sind.
  
    ```R
    featureDataSource = RxSqlServerData(sqlQuery = featureEngineeringQuery,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
        dropoff_longitude = "numeric", dropoff_latitude = "numeric",
        passenger_count  = "numeric", trip_distance  = "numeric",
        trip_time_in_secs  = "numeric", direct_distance  = "numeric"),
      connectionString = connStr)
    ```
  
6.  Nachdem das neue Feature erstellt wurde, rufen Sie **rxGetVarsInfo** auf, um eine Zusammenfassung der Daten in der Featuretabelle zu erstellen.
  
    ```R
    rxGetVarInfo(data = featureDataSource)
    ```

    **Ergebnisse**

    ```R
    Var 1: tipped, Type: integer
    Var 2: fare_amount, Type: numeric
    Var 3: passenger_count, Type: numeric
    Var 4: trip_time_in_secs, Type: numeric
    Var 5: trip_distance, Type: numeric
    Var 6: pickup_datetime, Type: character
    Var 7: dropoff_datetime, Type: character
    Var 8: direct_distance, Type: numeric
    Var 9: pickup_latitude, Type: numeric
    Var 10: pickup_longitude, Type: numeric
    Var 11: dropoff_latitude, Type: numeric
    Var 12: dropoff_longitude, Type: numeric
    ```

    > [!NOTE]
    > In einigen Fällen wird Ihnen möglicherweise eine Fehlermeldung ähnlich der folgenden angezeigt: *The EXECUTE permission was denied on the object 'fnCalculateDistance'* (Die EXECUTE-Berechtigung wurde für das Objekt „fnCalculateDistance“ abgelehnt.) Stellen Sie in diesem Fall sicher, dass Sie über die verwendeten Anmeldeinformationen über die Berechtigungen zum Ausführen von Skripts und zum Erstellen von Objekten für die Datenbank und nicht nur für die Instanz verfügen.
    > Überprüfen Sie das Schema des Objekts „fnCalculateDistance“. Wenn das Objekt vom Datenbankbesitzer erstellt wurde und Ihre Anmeldeinformationen der Rolle „db_datareader“ zugeordnet sind, müssen Sie diesen explizit Berechtigungen zum Ausführen des Skripts erteilen.

## <a name="comparing-r-functions-and-sql-functions"></a>Vergleich von R- und SQL-Funktionen

Erinnern Sie sich an diesen Codeausschnitt, der zum Stoppen des R-Codes verwendet wurde?

```R
start.time <- proc.time()
<your code here>
used.time <- proc.time() - start.time
print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate features.", sep=""))
```

Sie können versuchen, diesen mit dem benutzerdefinierten SQL-Funktionsbeispiel zu verwenden, um festzustellen, wie lange die Datentransformation beim Aufrufen einer SQL-Funktion dauert. Versuchen Sie zudem, mit „rxSetComputeContext“ Computekontexte zu wechseln, und vergleichen Sie die Zeiten.

Ihre Zeiten können abhängig von Ihrer Netzwerkgeschwindigkeit und Ihrer Hardwarekonfiguration stark variieren. In den getesteten Konfigurationen war der Ansatz mit der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktion schneller als die Verwendung einer benutzerdefinierten R-Funktion. Daher haben wir in den nachfolgenden Schritten die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktion für diese Berechnungen verwendet.

> [!TIP]
> Die Featureentwicklung mit [!INCLUDE[tsql](../../includes/tsql-md.md)] ist sehr oft schneller als mit R. Zum Beispiel enthält T-SQL Windowing- und Rangfolgefunktionen, die auf allgemeine Data Science-Berechnungen wie rollierende gleitende Durchschnitte und *n*-Kacheln angewendet werden können. Wählen Sie je nach Daten und Aufgabe die effizienteste Methode.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen eines R-Modells und Speichern in SQL](walkthrough-build-and-save-the-model.md)