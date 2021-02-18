---
title: 'R-Tutorial: Bereitstellen eines Modells'
description: Erfahren Sie, wie Sie R-Modelle in einer Produktionsumgebung bereitstellen können, indem Sie ein trainiertes Modell aus einer gespeicherten Prozedur abrufen.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 1830c441e51465f9c7cf7eab7ca503eadd677caf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272971"
---
# <a name="deploy-the-r-model-and-use-it-in-sql-server-walkthrough"></a>Bereitstellen des R-Modells und Verwendung in SQL Server (exemplarische Vorgehensweise)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

In dieser Lektion erfahren Sie, wie Sie R-Modelle in einer Produktionsumgebung bereitstellen können, indem Sie ein trainiertes Modell aus einer gespeicherten Prozedur abrufen. Sie können die gespeicherte Prozedur aus R oder jeder beliebigen Anwendungsprogrammiersprache aufrufen, die [!INCLUDE[tsql](../../includes/tsql-md.md)] (z. B. C#, Java und Python) unterstützt, um das Modell zum Treffen von Vorhersagen für neue Beobachtungen zu verwenden.

In diesem Artikel werden die häufigsten Möglichkeiten vorgestellt, ein Modell bei der Bewertung zu verwenden:

> [!div class="checklist"]
> * Im **Batchbewertungsmodus** werden mehrere Vorhersagen generiert.
> * Im **Einzelbewertungsmodus** wird jeweils nur eine Vorhersage generiert.

## <a name="batch-scoring"></a>Batchbewertung

Erstellen Sie die gespeicherte Prozedur *PredictTipBatchMode*, die mehrere Vorhersagen generiert und eine SQL-Abfrage oder Tabelle als Eingabe übergibt. Eine Tabelle mit Ergebnissen wird zurückgegeben. Diese können Sie direkt in eine Tabelle einfügen oder in eine Datei schreiben.

- Ruft einen Satz von Eingabedaten als SQL-Abfrage ab
- Ruft das trainierte logistische Regressionsmodell auf, das Sie in der vorherigen Lektion gespeichert haben
- Sagt die Wahrscheinlichkeit voraus, mit der der Fahrer ein Trinkgeld bekommt

1. Öffnen Sie in Management Studio ein neues Abfragefenster, und führen Sie folgendes T-SQL-Skript aus, um die gespeicherte Prozedur „PredictTipBatchMode“ zu erstellen.
  
    ```sql
    USE [NYCTaxi_Sample]
    GO

    SET ANSI_NULLS ON
    GO
    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'PredictTipBatchMode')
    DROP PROCEDURE v
    GO

    CREATE PROCEDURE [dbo].[PredictTipBatchMode] @input nvarchar(max)
    AS
    BEGIN
      DECLARE @lmodel2 varbinary(max) = (SELECT TOP 1 model  FROM nyc_taxi_models);
      EXEC sp_execute_external_script @language = N'R',
         @script = N'
           mod <- unserialize(as.raw(model));
           print(summary(mod))
           OutputDataSet<-rxPredict(modelObject = mod,
             data = InputDataSet,
             outData = NULL,
             predVarNames = "Score", type = "response",
             writeModelVars = FALSE, overwrite = TRUE);
           str(OutputDataSet)
           print(OutputDataSet)',
      @input_data_1 = @input,
      @params = N'@model varbinary(max)',
      @model = @lmodel2
      WITH RESULT SETS ((Score float));
    END
    ```

    + Verwenden Sie eine SELECT-Anweisung, um das gespeicherte Modell aus einer SQL-Tabelle abzurufen. Das Modell wird aus der Tabelle, die in der SQL-Variable _\@lmodel2_ gespeichert ist, als **varbinary(max)** abgerufen, und als Parameter *mod* an die gespeicherte Systemprozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) übergeben.

    + Die Eingabedaten für die Bewertung werden als SQL-Abfrage bewertet und als Zeichenfolge in der SQL-Variable _\@input_ gespeichert. Wenn Daten aus der Datenbank abgerufen werden, werden sie in einem Datenrahmen namens *InputDataSet* gespeichert. Dieser Name ist die Standardbezeichnung für Eingabedaten in die Prozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Sie können bei Bedarf einen anderen Variablennamen über den Parameter _\@input_data_1_name_ festlegen.

    + Die gespeicherte Prozedur ruft die rxPredict-Funktion aus der **RevoScaleR**-Bibliothek auf, um die Bewertung zu generieren.

    + Der Rückgabewert *Score* entspricht der Wahrscheinlichkeit, dass der Fahrer gemäß dem Modell ein Trinkgeld bekommt. Optional können Sie einfach einen Filter auf die zurückgegebenen Werten anwenden, um die Rückgabewerte in Gruppen wie „Trinkgeld“ oder „Kein Trinkgeld“ zu kategorisieren.  Eine Wahrscheinlichkeit von weniger als 0,5 würde beispielsweise bedeuten, dass wahrscheinlich kein Trinkgeld gegeben wird.
  
2.  Sie können die gespeicherte Prozedur im Batchmodus abrufen, indem Sie die erforderliche Abfrage als Eingabe für die gespeicherte Prozedur definieren. Im Folgenden sehen Sie die SQL-Abfrage, die Sie zur Überprüfung in SSMS ausführen können.

    ```sql
    SELECT TOP 10
      a.passenger_count AS passenger_count,
      a.trip_time_in_secs AS trip_time_in_secs,
      a.trip_distance AS trip_distance,
      a.dropoff_datetime AS dropoff_datetime,
      dbo.fnCalculateDistance( pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS direct_distance
      FROM 
        (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude 
        FROM nyctaxi_sample)a 
      LEFT OUTER JOIN
      ( SELECT medallion, hack_license, pickup_datetime
      FROM nyctaxi_sample  tablesample (1 percent) repeatable (98052)  )b
      ON a.medallion=b.medallion
      AND a.hack_license=b.hack_license
      AND a.pickup_datetime=b.pickup_datetime
      WHERE b.medallion is null
    ```

3. Mit diesem R-Code können Sie die Eingabezeichenfolge aus der SQL-Abfrage erstellen:

    ```R
    input <- "N'SELECT TOP 10 a.passenger_count AS passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS direct_distance FROM (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample)a LEFT OUTER JOIN ( SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample  tablesample (1 percent) repeatable (98052)  )b ON a.medallion=b.medallion AND a.hack_license=b.hack_license AND  a.pickup_datetime=b.pickup_datetime WHERE b.medallion is null'";
    q <- paste("EXEC PredictTipBatchMode @input = ", input, sep="");
    ```

4. Sie können die gespeicherte Prozedur mit R ausführen, indem Sie die **sqlQuery**-Methode des **RODBC**-Pakets aufrufen und die zuvor definierte SQL-Verbindung `conn` verwenden:

    ```R
    sqlQuery (conn, q);
    ```

    Wenn ein ODBC-Fehler angezeigt wird, sollten Sie überprüfen, ob Syntaxfehler vorliegen und die richtige Anzahl von Anführungszeichen vorhanden ist. 
    
    Wenn ein Berechtigungsfehler angezeigt wird, sollten Sie sicherstellen, dass Sie für die Ausführung der gespeicherten Prozedur berechtigt sind.

## <a name="single-row-scoring"></a>Einzelzeilenbewertung

Im Einzelbewertungsmodus wird jeweils nur eine Vorhersage gleichzeitig generiert. Dafür werden einzelne Werte als Eingabe an die gespeicherte Prozedur übergeben. Diese Werte entsprechen den Eigenschaften des Modells, anhand derer das Modell eine Vorhersage erstellt oder andere Ergebnisse wie einen Wahrscheinlichkeitswert generiert. Sie können diesen Wert dann der Anwendung oder dem Benutzer zurückgeben.

Wenn Sie das Modell für eine Vorhersage auf Zeilenbasis aufrufen, übergeben Sie die Werte, die den Eigenschaften der einzelnen Fälle entsprechen. Die gespeicherte Prozedur gibt dann eine einzelne Vorhersage oder Wahrscheinlichkeit zurück. 

Dieser Ansatz wird in der gespeicherten Prozedur *PredictTipSingleMode* veranschaulicht. Diese akzeptiert mehrere Parameter als Eingabe, die Eigenschaftenwerte darstellen (z. B. die Anzahl der Fahrgäste oder die Fahrtstrecke). Diese Eigenschaften werden dann mit dem gespeicherten R-Modell bewertet. Zuletzt wird die Wahrscheinlichkeit ausgegeben, mit der der Fahrer ein Trinkgeld bekommt.

1. Führen Sie die folgende Transact-SQL-Anweisung aus, um die gespeicherte Prozedur zu erstellen.

    ```sql
    USE [NYCTaxi_Sample]
    GO

    SET ANSI_NULLS ON
    GO
    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'PredictTipSingleMode')
    DROP PROCEDURE v
    GO

    CREATE PROCEDURE [dbo].[PredictTipSingleMode] @passenger_count int = 0,
    @trip_distance float = 0,
    @trip_time_in_secs int = 0,
    @pickup_latitude float = 0,
    @pickup_longitude float = 0,
    @dropoff_latitude float = 0,
    @dropoff_longitude float = 0
    AS
    BEGIN
      DECLARE @inquery nvarchar(max) = N'
        SELECT * FROM [dbo].[fnEngineerFeatures](@passenger_count, @trip_distance, @trip_time_in_secs, @pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude)'
      DECLARE @lmodel2 varbinary(max) = (SELECT TOP 1 model FROM nyc_taxi_models);

      EXEC sp_execute_external_script @language = N'R',  @script = N'
            mod <- unserialize(as.raw(model));
            print(summary(mod))
            OutputDataSet<-rxPredict(
              modelObject = mod,
              data = InputDataSet,
              outData = NULL,
              predVarNames = "Score",
              type = "response",
              writeModelVars = FALSE,
              overwrite = TRUE);
            str(OutputDataSet)
            print(OutputDataSet)
            ',
      @input_data_1 = @inquery,
      @params = N'
      -- passthrough columns
      @model varbinary(max) ,
      @passenger_count int ,
      @trip_distance float ,
      @trip_time_in_secs int ,
      @pickup_latitude float ,
      @pickup_longitude float ,
      @dropoff_latitude float ,
      @dropoff_longitude float',
      -- mapped variables
      @model = @lmodel2 ,
      @passenger_count =@passenger_count ,
      @trip_distance=@trip_distance ,
      @trip_time_in_secs=@trip_time_in_secs ,
      @pickup_latitude=@pickup_latitude ,
      @pickup_longitude=@pickup_longitude ,
      @dropoff_latitude=@dropoff_latitude ,
      @dropoff_longitude=@dropoff_longitude
      WITH RESULT SETS ((Score float));
    END
    ```

2. In SQL Server Management Studio können Sie über [!INCLUDE[tsql](../../includes/tsql-md.md)] **EXEC** (oder **EXECUTE**) die gespeicherte Prozedur aufrufen und die erforderlichen Eingaben an diese übergeben. Führen Sie diese Anweisung in Management Studio aus:

    ```sql
    EXEC [dbo].[PredictTipSingleMode] 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
    ```

    Die hier übergebenen Werte stehen für die Variablen _passenger\_count_, _trip_distance_, _trip\_time\_in\_secs_, _pickup\_latitude_, _pickup\_longitude_, _dropoff\_latitude_ und _dropoff\_longitude_.

3. Definieren Sie einfach eine R-Variable, die den gesamten Aufruf der gespeicherten Prozedur enthält, um diesen gleichen Aufruf von R-Code ausführen zu können.

    ```R
    q2 = "EXEC PredictTipSingleMode 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303 ";
    ```

    Die hier übergebenen Werte stehen für die Variablen _passenger\_count_, _trip\_distance_, _trip\_time\_in\_secs_, _pickup\_latitude_, _pickup\_longitude_, _dropoff\_latitude_ und _dropoff\_longitude_.

4. Rufen Sie `sqlQuery` über das **RODBC**-Paket auf, und übergeben Sie die Verbindungszeichenfolge und die Zeichenfolgenvariable mit dem Aufruf der gespeicherten Prozedur.

    ```R
    # predict with stored procedure in single mode
    sqlQuery (conn, q2);
    ```

    >[!TIP]
    > R Tools für Visual Studio (RTVS) ist eng mit SQL Server und R verzahnt. Im folgenden Artikel finden Sie weitere Beispiele für die Verwendung von RODBC mit einer SQL Server-Verbindung: [Arbeiten mit SQL Server und R](/visualstudio/rtvs/sql-server)

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun gelernt haben, mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Daten zu arbeiten und trainierte R-Modelle dauerhaft in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu speichern, sollten Sie problemlos neue Modelle auf Grundlage dieses Datasets erstellen können. Sie können beispielsweise versuchen, folgende Modelle zu erstellen:

+ Ein Regressionsmodell, das die Höhe des Trinkgelds vorhersagt
+ Ein mehrklassiges Klassifizierungsmodell, das vorhersagt, ob das Trinkgeld hoch, nicht so hoch oder gering ausfallen wird

Sie können sich auch folgende zusätzlichen Beispiele und Ressourcen ansehen:

+ [Szenarien für Data Science und Lösungsvorlagen](data-science-scenarios-and-solution-templates.md)
+ [Datenbankinterne Advanced Analytics](r-taxi-classification-introduction.md)
+ [Anleitungen für Microsoft Machine Learning Server](/machine-learning-server/r/how-to-introduction)
+ [Zusätzliche Ressourcen für Machine Learning Server](/machine-learning-server/resources-more)