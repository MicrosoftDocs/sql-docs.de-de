---
title: 'R-Tutorial: Durchsuchen von Daten'
description: In diesem Tutorial wird demonstriert, wie statistische Zusammenfassungen mithilfe von R-Funktionen für datenbankinterne Analysen in SQL Server visualisiert und generiert werden.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 676fd125bf148a5139de1ecee7145c523ee6b4e0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272941"
---
# <a name="view-and-summarize-sql-server-data-using-r-walkthrough"></a>Anzeigen und Zusammenfassen von SQL Server-Daten mithilfe von R (exemplarische Vorgehensweise)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

Diese Lektion enthält eine Einführung in die Funktionen des **RevoScaleR**-Pakets und führt Sie durch die folgenden Aufgaben:

> [!div class="checklist"]
> * Verbindung mit SQL Server herstellen
> * Definieren Sie eine Abfrage mit den Daten, die Sie benötigen, oder geben Sie eine Tabelle oder Sicht an.
> * Definieren von einem oder mehreren Computekontexten, die beim Ausführen von R-Code verwendet werden sollen
> * Optional können Sie Transformationen definieren, die auf die Datenquelle angewendet werden, während sie von der Quelle gelesen wird.

## <a name="define-a-sql-server-compute-context"></a>Definieren eines SQL Server-Computekontexts

Führen Sie die folgenden R-Anweisungen in einer R-Umgebung auf der Clientarbeitsstation aus. In diesem Abschnitt wird davon ausgegangen, dass eine [Data Science-Arbeitsstation mit Microsoft R Client](../r/set-up-a-data-science-client.md) verwendet wird, da sie alle RevoScaleR-Pakete sowie einen einfachen Basissatz von R-Tools enthält. Beispielsweise können Sie Datei „Rgui.exe“ verwenden, um das R-Skript in diesem Abschnitt auszuführen.

1. Wenn das **RevoScaleR**-Paket noch nicht geladen ist, führen Sie die folgende R-Codezeile aus:

    ```R
    library("RevoScaleR")
    ```

     Die Anführungszeichen sind optional, werden in diesem Fall jedoch empfohlen.
     
     Wenn Sie eine Fehlermeldung erhalten, stellen Sie sicher, dass die R-Entwicklungsumgebung eine Bibliothek verwendet, die das RevoScaleR-Paket enthält. Verwenden Sie einen Befehl wie `.libPaths()` zum Anzeigen des aktuellen Bibliothekspfads.

2. Erstellen Sie die Verbindungszeichenfolge für SQL Server, und speichern Sie sie in einer *connStr*-R-Variablen.

   Sie müssen den Platzhalter „your_server_name“ in einen gültigen SQL Server-Instanznamen ändern. Für den Servernamen können Sie möglicherweise nur den Instanznamen verwenden, oder Sie müssen den Namen abhängig von Ihrem Netzwerk vollständig qualifizieren.
    
   Für die SQL Server-Authentifizierung lautet die Verbindungssyntax wie folgt:

    ```R
    connStr <- "Driver=SQL Server;Server=your_server_name;Database=nyctaxi_sample;Uid=your-sql-login;Pwd=your-login-password"
    ```

    Für die Windows-Authentifizierung lautet die Syntax etwas anders:
    
    ```R
    connStr <- "Driver=SQL Server;Server=your_server_name;Database=nyctaxi_sample;Trusted_Connection=True"
    ```

    Es wird grundsätzlich empfohlen, nach Möglichkeit die Windows-Authentifizierung zu verwenden, um das Speichern von Kennwörtern in Ihrem R-Code zu vermeiden.

3. Definieren Sie Variablen für die Erstellung eines neuen *Computekontexts*. Nachdem Sie das Computecontextobjekt erstellt haben, können Sie es verwenden, um R-Code auf der SQL Server-Instanz auszuführen.

    ```R
    sqlShareDir <- paste("C:\\AllShare\\",Sys.getenv("USERNAME"),sep="")
    sqlWait <- TRUE
    sqlConsoleOutput <- FALSE
    ```

    - R nutzt ein temporäres Verzeichnis, das bei der Serialisierung von R-Objekten zwischen Ihrer Arbeitsstation und dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Computer verwendet wird. Sie können das lokale Verzeichnis angeben, das als *SqlShareDir* verwendet wird, oder den Standardnamen übernehmen.
  
    - Verwenden Sie *sqlWait*, um anzugeben, ob R auf Ergebnisse vom Server warten soll.  Eine Erläuterung zu wartenden und nicht wartenden Aufträgen finden Sie unter [Verteiltes und paralleles Computing mit RevoScaleR in Microsoft R](/r-server/r/how-to-revoscaler-distributed-computing).
  
    - Verwenden Sie das Argument *sqlConsoleOutput*, um anzugeben, dass Sie die Ausgabe der R-Konsole nicht anzeigen möchten.

4. Rufen Sie den [RxInSqlServer](/r-server/r-reference/revoscaler/rxinsqlserver)-Konstruktor auf, um das Computekontextobjekt mit den bereits definierten Variablen und Verbindungszeichenfolgen zu erstellen, und speichern Sie das neue Objekt in der R-Variablen *sqlcc*.
  
    ```R
    sqlcc <- RxInSqlServer(connectionString = connStr, shareDir = sqlShareDir, wait = sqlWait, consoleOutput = sqlConsoleOutput)
    ```

5. Der Computekontext ist standardmäßig lokal. Daher müssen Sie den *aktiven* Computekontext explizit festlegen.

    ```R
    rxSetComputeContext(sqlcc)
    ```

    + [rxSetComputeContext](/machine-learning-server/r-reference/revoscaler/rxsetcomputecontext) gibt den zuvor aktiven Computekontext im Hintergrund zurück, sodass Sie ihn verwenden können.
    + [rxGetComputeContext](/machine-learning-server/r-reference/revoscaler/rxsetcomputecontext) gibt den aktiven Computekontext zurück.
    
    Beachten Sie, dass das Festlegen des Computekontexts nur Vorgänge betrifft, die Funktionen im **RevoScaleR**-Paket nutzen. Der Computekontext wirkt sich nicht darauf aus, wie Open-Source-R-Vorgänge erfolgen.

## <a name="create-a-data-source-using-rxsqlserver"></a>Erstellen einer Datenquelle mithilfe von RxSqlServer

Wenn Sie Microsoft-R-Bibliotheken wie RevoScaleR und MicrosoftML verwenden, ist eine *Datenquelle* ein Objekt, das Sie mithilfe von RevoScaleR-Funktionen erstellen. Das Datenquellenobjekt gibt verschiedene Daten an, die Sie für eine Aufgabe verwenden möchten, z. B. zum Trainieren eines Modells oder Extrahieren von Features. Sie können Daten aus einer Vielzahl von Quellen, einschließlich SQL Server, erhalten. Eine Liste der derzeit unterstützten Quellen finden Sie unter [RxDataSource](/r-server/r-reference/revoscaler/rxdatasource).

Zuvor haben Sie eine Verbindungszeichenfolge definiert und diese Informationen in einer R-Variablen gespeichert. Sie können diese Verbindungsinformationen wiederverwenden, um die Daten anzugeben, die Sie erhalten möchten.

1. Speichern Sie eine SQL-Abfrage als Zeichenfolgenvariable. Die Abfrage definiert die Daten zum Trainieren des Modells.

    ```R
    sampleDataQuery <- "SELECT TOP 1000 tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime, pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude FROM nyctaxi_sample"
    ```

    Wir haben hier eine TOP-Klausel verwendet, um den Vorgang zu beschleunigen, die tatsächlich von der Abfrage zurückgegebenen Zeilen können je nach Reihenfolge jedoch variieren. Daher können sich auch Ihre Zusammenfassungsergebnisse von den unten aufgeführten unterscheiden. Sie können die TOP-Klausel entfernen.

2. Übergeben Sie die Abfragedefinition als Argument an die [RxSqlServerData](/r-server/r-reference/revoscaler/rxsqlserverdata)-Funktion.

    ```R
    inDataSource <- RxSqlServerData(
      sqlQuery = sampleDataQuery,
      connectionString = connStr,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
      dropoff_longitude = "numeric", dropoff_latitude = "numeric"),
      rowsPerRead=500
      )
    ```
    
    + Das Argument  *colClasses* gibt die zu verwendenden Spaltentypen an, wenn die Daten zwischen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und R verschoben werden. Das ist wichtig, da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] andere und mehr Datentypen als R verwendet. Weitere Informationen finden Sie unter [R-Bibliotheken und -Datentypen](../r/r-libraries-and-data-types.md).
  
    + Das Argument *rowsPerRead* ist wichtig für das Speichermanagement und effiziente Berechnungen.  Die meisten analytischen Funktionen in[!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] verarbeiten Daten in Segmenten und sammeln Zwischenergebnisse an, die die endgültigen Berechnungen zurückgeben, nachdem alle Daten gelesen wurden.  Durch Hinzufügen des *rowsPerRead*-Parameters können Sie steuern, wie viele Datenzeilen in jedes Segment für die Verarbeitung eingelesen werden.  Wenn der Wert dieses Parameters zu groß ist, könnte der Datenzugriff möglicherweise langsam sein, da Sie nicht über genügen Speicher verfügen, um einen solch großen Datenblock effizient zu verarbeiten.  Auf einigen Systemen kann sich die Leistung auch verringern, wenn *rowsPerRead* auf einen zu kleinen Wert festgelegt wird.

3. Zu diesem Zeitpunkt haben Sie das *inDataSource*-Objekt zwar erstellt, es enthält allerdings keine Daten. Die Daten werden erst aus der SQL-Abfrage in die lokale Umgebung übertragen, nachdem Sie eine Funktion wie [rxImport](/r-server/r-reference/revoscaler/rxdatastep) oder [rxSummary](/r-server/r-reference/revoscaler/rxsummary) ausgeführt haben.

    Da Sie jedoch inzwischen die Datenobjekte definiert haben, können Sie sie als Argument für andere Funktionen verwenden.

## <a name="use-the-sql-server-data-in-r-summaries"></a>Verwenden der SQL Server-Daten in R-Zusammenfassungen

In diesem Abschnitt testen Sie einige der Funktionen, die in [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] bereitgestellt sind und Remotecomputekontexte unterstützen. Durch Anwenden von R-Funktionen auf die Datenquelle können Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Daten untersuchen, zusammenfassen und in einem Diagramm darstellen.

1. Rufen Sie die Funktion [rxGetVarInfo](/r-server/r-reference/revoscaler/rxgetvarinfo) auf, um eine Liste der Variablen und der dazugehörigen Datentypen in der Datenquelle zu erhalten.

    **rxGetVarInfo** ist eine praktische Funktion, die mit jedem beliebigen Datenrahmen oder unterschiedliche Daten in einem Remote-Datenobjekt verwendet werden kann, um Informationen wie die Maximal- und Minimalwerte, den Datentyp sowie die Anzahl der Ebenen in Faktorspalten abzurufen.
    
    Ziehen Sie es in Erwägung, diese Funktion nach jeder Art von Dateneingabe, Funktionstransformation oder Featureentwicklung auszuführen. So können Sie sicherstellen, dass es sich bei allen Funktionen, die Sie in Ihrem Modell verwenden möchten, um den erwarteten Datentyp handelt und Fehler vermieden werden.
  
    ```R
    rxGetVarInfo(data = inDataSource)
    ```

    **Ergebnisse**
    
    ```R
    Var 1: tipped, Type: integer
    Var 2: fare_amount, Type: numeric
    Var 3: passenger_count, Type: integer
    Var 4: trip_time_in_secs, Type: numeric, Storage: int64
    Var 5: trip_distance, Type: numeric
    Var 6: pickup_datetime, Type: character
    Var 7: dropoff_datetime, Type: character
    Var 8: pickup_longitude, Type: numeric
    Var 9: pickup_latitude, Type: numeric
    Var 10: dropoff_longitude, Type: numeric
    ```

2. Rufen Sie nun die RevoScaleR-Funktion [rxSummary](/r-server/r-reference/revoscaler/rxsummary) auf, um detailliertere Statistiken zu einzelnen Variablen zu erhalten.

    rxSummary basiert auf der R-Funktion `summary`, bietet jedoch einige zusätzliche Features und Vorteile. rxSummary funktioniert in mehreren Computekontexten und unterstützt Segmentierungen.  Sie können rxSummary auch verwenden, um Werte zu transformieren oder basierend auf den Faktorebenen zusammenzufassen.
    
    In diesem Beispiel fassen Sie den Fahrpreis basierend auf der Anzahl der Fahrgäste zusammen.
    
    ```R
    start.time <- proc.time()
    rxSummary(~fare_amount:F(passenger_count,1,6), data = inDataSource)
    used.time <- proc.time() - start.time
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds,
      Elapsed Time=", round(used.time[3],2),
      " seconds to summarize the inDataSource.", sep=""))
    ```
    + Das erste Argument für „rxSummary“ gibt die Formel oder den Begriff an, nach der oder dem zusammengefasst wird. Hier wird die Funktion `F()` verwendet, um die Werte in _passenger\_count_ vor der Zusammenfassung in Faktoren zu konvertieren. Sie müssen auch den Mindestwert (1) und den Maximalwert (6) für die Faktorvariable _passenger\_count_ angeben.
    + Wenn Sie die Statistiken, die ausgegeben werden sollen, nicht angeben, gibt rxSummary standardmäßig Mean, StDev, Min, Max und die Anzahl der gültigen und nicht vorhandenen Beobachtungen aus.
    + Dieses Beispiel enthält Code, um zu messen, wie viel Zeit zwischen dem Start der Funktion und dem Abschluss der Funktion vergangen ist, sodass Sie die Leistung vergleichen können.
  
    **Ergebnisse**

    Wenn die Funktion „rxSummary“ erfolgreich ausgeführt wird, sollten Ergebnisse wie diese angezeigt werden, gefolgt von einer Liste von Statistiken nach Kategorie. 

    ```R
    rxSummary(formula = ~fare_amount:F(passenger_count, 1,6), data = inDataSource)
    Data: inDataSource (RxSqlServerData Data Source)
    Number of valid observations: 1000
    ```

### <a name="bonus-exercise-on-big-data"></a>Bonusübung zu Big Data

Versuchen Sie, eine neue Abfragezeichenfolge mit allen Zeilen zu definieren. Es wird empfohlen, dass Sie für dieses Experiment ein neues Datenquellenobjekt einrichten. Sie können auch versuchen, den *rowsToRead*-Parameter zu ändern, um zu sehen, wie sich dies auf den Durchsatz auswirkt

```R
bigDataQuery  <- "SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime, pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude FROM nyctaxi_sample"

bigDataSource <- RxSqlServerData(
      sqlQuery = bigDataQuery,
      connectionString = connStr,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
      dropoff_longitude = "numeric", dropoff_latitude = "numeric"),
      rowsPerRead=500
      )

start.time <- proc.time()
rxSummary(~fare_amount:F(passenger_count,1,6), data = bigDataSource)
used.time <- proc.time() - start.time
print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds,
  Elapsed Time=", round(used.time[3],2),
  " seconds to summarize the inDataSource.", sep=""))
```

> [!TIP]
> Während dieses Vorgangs können Sie ein Tool wie [Prozess-Explorer](/sysinternals/downloads/process-explorer) oder SQL Profiler verwenden, um zu sehen, wie die Verbindung hergestellt und der R-Code mit SQL Server Services ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen von Graphen und Plots mit R](walkthrough-create-graphs-and-plots-using-r.md)