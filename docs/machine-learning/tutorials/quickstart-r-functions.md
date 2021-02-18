---
title: 'Schnellstart: R-Funktionen'
titleSuffix: SQL machine learning
description: In diesem Schnellstart wird beschrieben, wie Sie mathematische R-Funktionen und Hilfsfunktionen mit SQL Machine Learning verwenden.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 451e72262cce6befa1a0eadc149a1e0e69d6f5b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339615"
---
# <a name="quickstart-r-functions-with-sql-machine-learning"></a>Schnellstart: R-Funktionen mit SQL Machine Learning
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In diesem Schnellstart wird beschrieben, wie Sie mathematische Funktionen und Hilfsfunktionen in R mit [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) oder in [Big Data-Clustern](../../big-data-cluster/machine-learning-services.md) verwenden. Die Implementierung von statistischen Funktionen mit T-SQL ist oft kompliziert, kann aber in R mit nur wenigen Codezeilen durchgeführt werden.
::: moniker-end
::: moniker range="=sql-server-2017"
In diesem Schnellstart wird beschrieben, wie Sie mathematische R-Funktionen und Hilfsfunktionen mit [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) verwenden. Die Implementierung von statistischen Funktionen mit T-SQL ist oft kompliziert, kann aber in R mit nur wenigen Codezeilen durchgeführt werden.
::: moniker-end
::: moniker range="=sql-server-2016"
In diesem Schnellstart wird beschrieben, wie Sie mathematische R-Funktionen und Hilfsfunktionen mit [SQL Server R Services](../r/sql-server-r-services.md) verwenden. Die Implementierung von statistischen Funktionen mit T-SQL ist oft kompliziert, kann aber in R mit nur wenigen Codezeilen durchgeführt werden.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In dieser Schnellstartanleitung erfahren Sie, wie Sie Datenstrukturen und -typen mit R in [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) verwenden können. Außerdem erhalten Sie Informationen zum Verschieben von Daten zwischen R und SQL Managed Instance und zu Fehlern, die in diesem Zusammenhang häufig auftreten.
::: moniker-end

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser Schnellstartanleitung benötigen Sie folgende Voraussetzungen.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
- SQL Server Machine Learning Services. Informationen zur Installation von Machine Learning Services finden Sie im [Windows-](../install/sql-machine-learning-services-windows-install.md) oder im [Linux-Installationsleitfaden](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). Sie können auch [Machine Learning Services in Big Data-Clustern unter SQL Server aktivieren](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017"
- SQL Server Machine Learning Services. Informationen zur Installation von Machine Learning Services finden Sie im [Windows-Installationsleitfaden](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=sql-server-2016"
- SQL Server 2016 R Services. Informationen zur Installation von R Services finden Sie im [Windows-Installationsleitfaden](../install/sql-r-services-windows-install.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
- Machine Learning Services in Azure SQL Managed Instance. In der Übersicht [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) finden Sie weitere Informationen.
::: moniker-end

- Ein Tool zum Ausführen von SQL-Abfragen, die R-Skripts enthalten. In dieser Schnellstartanleitung wird [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) verwendet.

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>Erstellen einer gespeicherten Prozedur zum Generieren von Zufallszahlen

Der Einfachheit halber verwenden wir das R-Paket `stats`, das standardmäßig installiert und geladen wird. Das Paket enthält Hunderte von Funktionen für allgemeine statistische Aufgaben, darunter die `rnorm`-Funktion, die eine bestimmte Anzahl von Zufallszahlen mithilfe der normalen Verteilung bei angegebener Standardabweichung und Mittelwert generiert.

Der folgende R-Code gibt beispielsweise 100 Zahlen mit einem Mittelwert von 50 bei einer Standardabweichung von 3 zurück.

```R
as.data.frame(rnorm(100, mean = 50, sd = 3));
```

Fügen Sie die R-Funktion wie folgt in den R-Skriptparameter `sp_execute_external_script` ein, um diese R-Codezeile über T-SQL aufzurufen:

```sql
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
         OutputDataSet <- as.data.frame(rnorm(100, mean = 50, sd =3));'
    , @input_data_1 = N'   ;'
      WITH RESULT SETS (([Density] float NOT NULL));
```

Wie gehen Sie vor, wenn Sie das Erstellen eines anderen Satzes von Zufallszahlen vereinfachen möchten?

Das ist in Kombination mit T-SQL ganz einfach. Sie definieren eine gespeicherte Prozedur, die Argumente vom Benutzer abruft, und übergeben diese Argumente dann als Variablen an das R-Skript.

```sql
CREATE PROCEDURE MyRNorm (
    @param1 INT
    , @param2 INT
    , @param3 INT
    )
AS
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
         OutputDataSet <- as.data.frame(rnorm(mynumbers, mymean, mysd));'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

- Die erste Zeile definiert alle SQL-Eingabeparameter, die beim Ausführen der gespeicherten Prozedur erforderlich sind.

- In der mit `@params` beginnenden Zeile werden alle Variablen, die im R-Code verwendet werden, und die zugehörigen SQL-Datentypen definiert.

- Die unmittelbar darauf folgenden Zeilen ordnen die SQL-Parameternamen den entsprechenden R-Variablennamen zu.

Nachdem Sie die R-Funktion in eine gespeicherte Prozedur eingeschlossen haben, können die Funktion ganz einfach wie folgt aufrufen und dabei jeweils andere Werte übergeben:

```sql
EXECUTE MyRNorm @param1 = 100,@param2 = 50, @param3 = 3
```

## <a name="use-r-utility-functions-for-troubleshooting"></a>Verwenden von R-Hilfsfunktionen für die Problembehandlung

Das standardmäßig installierte Paket **utils** bietet eine Vielzahl von Hilfsfunktionen zum Untersuchen der aktuellen R-Umgebung. Diese Funktionen können sich als nützlich erweisen, wenn Sie Diskrepanzen bei der Leistung Ihres R-Codes in SQL Server und externen Umgebungen feststellen.

Sie können z. B. die Systemzeitfunktionen in R wie `system.time` und `proc.time` verwenden, um die Dauer von R-Prozessen zu erfassen und Leistungsprobleme zu analysieren. Im Tutorial [Erstellen von Datenfeatures](../tutorials/walkthrough-create-data-features.md) finden Sie ein Beispiel, bei dem Zeitsteuerungsfunktionen von R in die Lösung eingebettet sind.

```sql
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
        library(utils);
        start.time <- proc.time();
        
        # Run R processes
        
        elapsed_time <- proc.time() - start.time;'
```

Weitere nützliche Funktionen zum Verbessern der Leistung finden Sie unter [Verwenden von R-Code-Profilerstellungsfunktionen](../r/using-r-code-profiling-functions.md).

## <a name="next-steps"></a>Nächste Schritte

Befolgen Sie den folgenden Schnellstart, um ein Machine Learning-Modell mithilfe von R mit SQL Machine Learning zu erstellen:

> [!div class="nextstepaction"]
> [Erstellen und Bewerten eines Vorhersagemodells in R mit SQL Machine Learning](quickstart-r-train-score-model.md)