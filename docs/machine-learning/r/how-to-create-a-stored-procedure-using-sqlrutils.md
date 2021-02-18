---
title: Erstellen einer Funktion aus gebündeltem R-Code mithilfe des sqlrutils-Pakets
description: Verwenden Sie das R-Paket „sqlrutils“ in SQL Server, um Code in der R-Programmiersprache in einer einzigen Funktion zu bündeln, die als Argument an eine gespeicherte Prozedur übergeben werden kann.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 04bcac677a4cdbf3e96f60bb43244f2cc6a0eec7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336278"
---
# <a name="create-a-stored-procedure-using-sqlrutils"></a>Erstellen einer gespeicherten Prozedur mithilfe von sqlrutils
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

In diesem Artikel werden die Schritte beschrieben, in denen Sie R-Code so konvertieren, dass er als gespeicherte T-SQL-Prozedur ausgeführt werden kann. Zum Erzielen bestmöglicher Ergebnisse kann es erforderlich sein, dass Ihr Code ein wenig geändert wird, damit sichergestellt ist, dass alle Eingaben parametrisiert werden können.

## <a name="step-1-rewrite-r-script"></a><a name="bkmk_rewrite"></a>Schritt 1: Erneutes Generieren eines R-Skripts

Damit Sie die besten Ergebnisse erzielen, sollten Sie Ihren R-Code erneut generieren, um ihn in einer einzelnen Funktion zu kapseln.

Alle Variablen, die in der Funktion verwendet werden, sollten innerhalb der Funktion oder als Eingabeparameter definiert sein. Siehe [Beispielcode](#samples) in diesem Artikel.

Auch, da die Eingabeparameter der R-Funktion die Eingabeparameter der gespeicherten SQL-Prozedur werden, müssen Sie sicherstellen, dass Ihre Eingaben und Ausgaben den folgenden Typanforderungen entsprechen:

### <a name="inputs"></a>Eingaben

In den Eingabeparametern darf es höchstens einen Datenrahmen geben.

Der Objekte in dem Datenrahmen müssen, so wie alle anderen Eingabeparameter der Funktion, die folgenden R-Datentypen haben:
- POSIXct
- NUMERIC
- character
- integer
- Logisch
- raw

Ist ein Eingabetyp keiner der hier genannten Typen, muss es serialisiert und als *raw* an die Funktion übergeben werden. In diesem Fall muss die Funktion auch Code enthalten, in dem die Eingabe deserialisiert wird.

### <a name="outputs"></a>Ausgaben

Die Ausgabe der Funktion kann eines der folgenden Objekte sein:

- Ein Datenrahmen, der die unterstützten Datentypen enthält. Jedes Objekt im Datenrahmen muss einen der unterstützten Datentypen haben.
- Eine benannte Liste, die höchstens einen Datenrahmen enthält. Jedes Element der Liste muss einen der unterstützten Datentypen haben.
- NULL, wenn die Funktion kein Ergebnis zurückgibt.

## <a name="step-2-generate-required-objects"></a>Schritt 2: Generieren der benötigten Objekte

Nachdem Ihr R-Code bereinigt wurde und als Einzelfunktion aufgerufen werden kann, verwenden Sie die Funktionen im **sqlrutils**-Paket, um die Ein- und Ausgaben so vorzubereiten, dass sie an den Konstruktor übergeben werden können, der die gespeicherte Prozedur tatsächlich erstellt.

**sqlrutils** stellt Funktionen bereit, die das Schema und den Typ der Eingabedaten definieren, und legt das Schema und den Typ der Ausgabedaten fest. Das Paket umfasst zudem Funktionen, die R-Objekte in den erforderlichen Ausgabetyp konvertieren können. Abhängig von den im Code verwendeten Datentypen können Sie mehrere Funktionsaufrufe ausführen, um die erforderlichen Objekte zu erstellen.

### <a name="inputs"></a>Eingaben

Wenn Ihre Funktion Eingaben übernimmt, rufen Sie für jede Eingabe die folgenden Funktionen auf:

- `setInputData`, wenn die Eingabe ein Datenrahmen ist.
- `setInputParameter` für alle anderen Eingabetypen.

Wenn Sie die einzelnen Funktionsaufrufe ausführen, wird jeweils ein R-Objekt erstellt, das Sie später als Argument an `StoredProcedure` übergeben, um die komplette gespeicherte Prozedur zu erstellen.

### <a name="outputs"></a>Ausgaben

**sqlrutils** bietet mehrere Funktionen zum Konvertieren von R-Objekten wie Listen in den von SQL Server benötigten Datenrahmen (data.frame).
Gibt Ihre Funktion direkt einen Datenrahmen aus, ohne diesen erst in eine Liste einzuschließen, können Sie diesen Schritt überspringen.
Sie können die Konvertierung in diesem Schritt auch überspringen, wenn Ihre Funktion NULL zurückgibt.

Beim Konvertieren einer Liste oder beim Abrufen eines bestimmten Elements aus einer Liste stehen Ihnen diese Funktionen zur Verfügung:

- `setOutputData`, wenn die aus der Liste abzurufende Variable ein Datenrahmen ist.
- `setOutputParameter` für alle anderen Elemente der Liste.

Wenn Sie die einzelnen Funktionsaufrufe ausführen, wird jeweils ein R-Objekt erstellt, das Sie später als Argument an `StoredProcedure` übergeben, um die komplette gespeicherte Prozedur zu erstellen.

## <a name="step-3-generate-the-stored-procedure"></a>Schritt 3: Generieren der gespeicherten Prozedur

Sobald alle Eingabe- und Ausgabeparameter bereit sind, führen Sie einen Aufruf des `StoredProcedure`-Konstruktors aus.

**Verwendung**

`StoredProcedure (func, spName, ..., filePath = NULL ,dbName = NULL, connectionString = NULL, batchSeparator = "GO")`

Angenommen, Sie möchten eine gespeicherte Prozedur namens **sp_rsample** mit diesen Parametern erstellen:

- Verwendet eine vorhandene Funktion **foosql**. Die Funktion basierte auf vorhandenem Code in der R-Funktion **foo**, aber Sie haben die Funktion entsprechend der Anforderungen umgeschrieben, wie in [diesem Abschnitt](#bkmk_rewrite) beschrieben, und die aktualisierte Funktion als **foosql** bezeichnet.
- Verwendet den Datenrahmen **queryinput** als Eingabe.
- Generiert als Ausgabe eine Datenrahmen mit dem R-Variablennamen **SQLOutput**.
- Sie möchten den T-SQL-Code als Datei im Ordner `C:\Temp` erstellen, damit Sie ihn später mit SQL Server Management Studio ausführen können.

```R
StoredProcedure (foosql, sp_rsample, queryinput, sqloutput, filePath = "C:\\Temp")
```

> [!NOTE]
> Da Sie die Datei in das Dateisystem schreiben, können Sie die Argumente, die die Datenbankverbindung definieren, weglassen.

Die Ausgabe der Funktion ist eine gespeicherte T-SQL-Prozedur, die auf einer Instanz von SQL Server 2016 (erfordert R Services) oder SQL Server 2017 (erfordert Machine Learning Services mit R) ausgeführt werden kann. 

Weitere Beispiele finden Sie in der Pakethilfe, wenn Sie in einer R-Umgebung `help(StoredProcedure)` aufrufen.

## <a name="step-4-register-and-run-the-stored-procedure"></a>Schritt 4. Registrieren und Ausführen der gespeicherten Prozedur

Es gibt zwei Möglichkeiten, wie Sie die gespeicherte Prozedur ausführen können:

- Mit T-SQL von jedem Client aus, der Verbindungen zur SQL Server 2016- bzw. SQL Server 2017-Instanz unterstützt.
- Über eine R-Umgebung.

Bei beiden Methoden muss die gespeicherte Prozedur in der Datenbank registriert sein, in der Sie diese verwenden möchten.

### <a name="register-the-stored-procedure"></a>Registrieren der gespeicherten Prozedur

Sie können die gespeicherte Prozedur mit R registrieren oder die CREATE PROCEDURE-Anweisung in T-SQL ausführen.

- Verwenden von T-SQL:  Wenn Sie mit T-SQL besser vertraut sind, öffnen Sie SQL Server Management Studio (oder einen anderen Client, der SQL-DDL-Befehle ausführen kann) und führen Sie die CREATE PROCEDURE-Anweisung mit dem von der `StoredProcedure`-Funktion vorbereiteten Code aus.
- Verwenden von R: Solange Sie sich noch in Ihrer R-Umgebung befinden, können Sie die `registerStoredProcedure`-Funktion in **sqlrutils** verwenden, um die gespeicherte Prozedur bei der Datenbank zu registrieren.

  Beispielsweise können Sie die gespeicherte Prozedur **sp_rsample** in der in *sqlConnStr* definierten Instanz und Datenbank registrieren, indem Sie diesen R-Aufruf ausführen:

  ```R
  registerStoredProcedure(sp_rsample, sqlConnStr)
  ```


> [!IMPORTANT]
> Unabhängig davon, ob Sie R oder SQL verwenden, müssen Sie die Anweisung mit einem Konto ausführen, das über Berechtigungen zum Erstellen neuer Datenbankobjekte verfügt.

### <a name="run-using-sql"></a>Ausführen mit SQL

Nachdem die gespeicherte Prozedur erstellt wurde, öffnen Sie eine Verbindung zur SQL-Datenbank-Instanz. Verwenden Sie hierzu einen beliebigen Client, der T-SQL unterstützt, und übergeben Sie Werte für alle von der gespeicherten Prozedur benötigten Parameter.

### <a name="run-using-r"></a>Ausführen mit R

Eine zusätzliche Vorbereitung ist erforderlich, wenn Sie die gespeicherte Prozedur aus dem R-Code und nicht aus der SQL Server-Instanz ausführen möchten. Wenn die gespeicherte Prozedur Eingabe zum Beispiel Eingabewerte erfordert, müssen Sie diese Eingabeparameter festlegen, bevor die Funktion ausgeführt werden kann. Anschließend können diese Objekte an die gespeicherte Prozedur in Ihrem R-Code übergeben werden.

Das Aufrufen der vorbereiteten gespeicherten SQL-Prozedur läuft insgesamt wie folgt ab:

1. Rufen Sie `getInputParameters` auf, um eine Liste der Eingabeparameterobjekte abzurufen.
2. Definieren Sie eine Abfrage ( `$query` ) für jeden Parameter, oder legen Sie einen Wert ( `$value` ) für jeden Parameter fest.
3. Verwenden Sie `executeStoredProcedure` , um die gespeicherte Prozedur aus der R-Entwicklungsumgebung auszuführen. Übergeben Sie dazu die Liste der Eingabeparameterobjekte, die Sie festgelegt haben.

## <a name="example"></a><a name = "samples"></a>Beispiel

In diesem Beispiel werden die Vorher-Nachher-Versionen eines R-Skripts gezeigt, das Daten aus einer SQL Server-Datenbank abruft, einige Transformationen an den Daten durchführt und sie in einer anderen Datenbank speichert.

Dieses einfache Beispiel soll lediglich veranschaulichen, wie Sie Ihren R-Code neu anordnen könnten, um die Konvertierung in eine gespeicherte Prozedur zu vereinfachen.

### <a name="before-code-preparation"></a>Vor der Vorbereitung des Codes


```R
sqlConnFrom <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=AirlineSrc;Trusted_Connection=Yes;"
  
sqlConnTo <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=AirlineTest;Trusted_Connection=Yes;"
  
sqlQueryAirline <- "SELECT TOP 10000 ArrDelay, CRSDepTime, DayOfWeek FROM [AirlineDemoSmall]"
  
dsSqlFrom <- RxSqlServerData(sqlQuery = sqlQueryAirline, connectionString = sqlConnFrom)
  
dsSqlTo <- RxSqlServerData(table = "cleanData", connectionString = sqlConnTo)
  
xFunc <- function(data) {
    data$CRSDepHour <- as.integer(trunc(data$CRSDepTime))
    return(data)
    }
  
xVars <- c("CRSDepTime")
  
sqlCompute <- RxInSqlServer(numTasks = 4, connectionString = sqlConnTo)
  
rxOpen(dsSqlFrom)
rxOpen(dsSqlTo)
  
if (rxSqlServerTableExists("cleanData", connectionString = sqlConnTo))   {
    rxSqlServerDropTable("cleanData")}
  
rxDataStep(inData = dsSqlFrom, 
     outFile = dsSqlTo,
     transformFunc = xFunc,
     transformVars = xVars,
     overwrite = TRUE)
```

> [!NOTE]
> 
> Wenn Sie eine ODBC-Verbindung verwenden, statt die *RxSqlServerData*-Funktion aufzurufen, müssen Sie die Verbindung mit *rxOpen* öffnen, bevor Sie Vorgänge in der Datenbank ausführen können.


### <a name="after-code-preparation"></a>Nach der Vorbereitung des Codes

In der aktualisierten Version ist der Funktionsname in der ersten Zeile definiert. Der gesamte weitere Code aus der ursprünglichen R-Lösung wird Teil dieser Funktion.

```R
myetl1function <- function() { 
   sqlConnFrom <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=Airline01;Trusted_Connection=Yes;"
   sqlConnTo <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer02;Database=Airline02;Trusted_Connection=Yes;"
    
   sqlQueryAirline <- "SELECT TOP 10000 ArrDelay, CRSDepTime, DayOfWeek FROM [AirlineDemoSmall]"

   dsSqlFrom <- RxSqlServerData(sqlQuery = sqlQueryAirline, connectionString = sqlConnFrom)
  
   dsSqlTo <- RxSqlServerData(table = "cleanData", connectionString = sqlConnTo)
  
   xFunc <- function(data) {
     data$CRSDepHour <- as.integer(trunc(data$CRSDepTime))
     return(data)}
  
   xVars <- c("CRSDepTime")
  
   sqlCompute <- RxInSqlServer(numTasks = 4, connectionString = sqlConnTo)
  
   if (rxSqlServerTableExists("cleanData", connectionString = sqlConnTo)) {rxSqlServerDropTable("cleanData")}
  
   rxDataStep(inData = dsSqlFrom, 
        outFile = dsSqlTo,
        transformFunc = xFunc,
        transformVars = xVars,
        overwrite = TRUE)
   return(NULL)
}
```

> [!NOTE]
> 
> Obwohl Sie die ODBC-Verbindung nicht explizit in Ihrem Code öffnen müssen, ist weiterhin eine ODBC-Verbindung erforderlich, um **sqlrutils** zu verwenden.

## <a name="see-also"></a>Weitere Informationen

[sqlrutils (SQL)](ref-r-sqlrutils.md)


