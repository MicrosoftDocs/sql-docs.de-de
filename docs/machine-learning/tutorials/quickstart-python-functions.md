---
title: 'Schnellstart: Python-Funktionen'
titleSuffix: SQL machine learning
description: In diesem Schnellstart wird beschrieben, wie Sie mathematische Python-Funktionen und Hilfsfunktionen mit SQL-Machine Learning verwenden.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 5871307e3bdd5be271547624089c9c47d2932739
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351443"
---
# <a name="quickstart-python-functions-with-sql-machine-learning"></a>Schnellstart: Python-Funktionen mit SQL-Machine Learning
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

In diesem Schnellstart erfahren Sie, wie Sie mathematische Python-Funktionen und Python-Hilfsfunktionen mit [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md), [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview) oder [SQL Server Big Data-Cluster](../../big-data-cluster/machine-learning-services.md) verwenden. Die Implementierung von statistischen Funktionen mit T-SQL ist oft kompliziert, kann aber in Python mit nur wenigen Codezeilen durchgeführt werden.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser Schnellstartanleitung benötigen Sie folgende Voraussetzungen.

- Eine SQL-Datenbank auf einer der folgenden Plattformen:
  - [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Informationen zur Installation finden Sie im [Windows-](../install/sql-machine-learning-services-windows-install.md) oder im [Linux-Installationsleitfaden](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Big Data-Cluster für SQL Server. Weitere Informationen finden Sie unter [Aktivieren von Machine Learning Services auf SQL Server-Big Data-Clustern](../../big-data-cluster/machine-learning-services.md).
  - Machine Learning Services in Azure SQL Managed Instance. In der Übersicht [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) finden Sie weitere Informationen.

- Ein Tool zum Ausführen von SQL-Abfragen, die Python-Skripts enthalten. In dieser Schnellstartanleitung wird [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) verwendet.

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>Erstellen einer gespeicherten Prozedur zum Generieren von Zufallszahlen

Der Einfachheit halber verwenden wir das Python-Paket `numpy`, das standardmäßig installiert und geladen wird. Das Paket enthält Hunderte von Funktionen für allgemeine statistische Aufgaben, darunter die `random.normal`-Funktion, die eine bestimmte Anzahl von Zufallszahlen mithilfe der normalen Verteilung bei angegebener Standardabweichung und Mittelwert generiert.

Der folgende Python-Code gibt beispielsweise 100 Zahlen mit einem Mittelwert von 50 bei einer Standardabweichung von 3 zurück.

```Python
numpy.random.normal(size=100, loc=50, scale=3)
```

Fügen Sie die Python-Funktion in den Python-Skriptparameter `sp_execute_external_script` ein, um diese Python-Codezeile über T-SQL aufzurufen. Die Ausgabe erwarten einen Datenrahmen, verwenden sie also `pandas`, um ihn zu konvertieren.

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import numpy
import pandas
OutputDataSet = pandas.DataFrame(numpy.random.normal(size=100, loc=50, scale=3));
'
    , @input_data_1 = N'   ;'
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

Wie gehen Sie vor, wenn Sie das Erstellen eines anderen Satzes von Zufallszahlen vereinfachen möchten? Sie definieren eine gespeicherte Prozedur, die Argumente vom Benutzer abruft, und übergeben diese Argumente dann als Variablen an das Python-Skript.

```sql
CREATE PROCEDURE MyPyNorm (
      @param1 INT
    , @param2 INT
    , @param3 INT
    )
AS
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import numpy
import pandas
OutputDataSet = pandas.DataFrame(numpy.random.normal(size=mynumbers, loc=mymean, scale=mysd));
'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

- Die erste Zeile definiert alle SQL-Eingabeparameter, die beim Ausführen der gespeicherten Prozedur erforderlich sind.

- Die Zeile, die mit `@params` beginnt, definiert alle vom Python-Code verwendeten Variablen und die entsprechenden SQL-Datentypen.

- Die unmittelbar folgenden Zeilen ordnen die SQL-Parameternamen den entsprechenden Python-Variablennamen zu.

Nun haben Sie die Python-Funktion in eine gespeicherte Prozedur eingeschlossen und können sie wie folgt ganz einfach aufrufen und ihr andere Werte übergeben:

```sql
EXECUTE MyPyNorm @param1 = 100,@param2 = 50, @param3 = 3
```

## <a name="use-python-utility-functions-for-troubleshooting"></a>Verwenden von Python-Hilfsfunktionen für die Problembehandlung

Python-Pakete bieten eine Vielzahl verschiedener Hilfsfunktionen zur Untersuchung der aktuellen Python-Umgebung. Diese Funktionen können sich als nützlich erweisen, wenn Sie Diskrepanzen bei der Leistung Ihres Python-Codes in SQL Server und externen Umgebungen feststellen.

Beispielsweise können Sie Funktionen für die Systemzeitsteuerung im `time`-Paket verwenden, um den Zeitaufwand von Python-Prozessen zu erfassen und Leistungsprobleme zu analysieren.

```sql
EXECUTE sp_execute_external_script
      @language = N'Python'
    , @script = N'
import time
start_time = time.time()

# Run Python processes

elapsed_time = time.time() - start_time
'
    , @input_data_1 = N' ;';
```

## <a name="next-steps"></a>Nächste Schritte

Orientieren Sie sich an dem folgenden Schnellstart, um ein Machine Learning-Modell mithilfe von Python mit SQL-Machine Learning zu erstellen:

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen und Bewerten eines Vorhersagemodells in Python](quickstart-python-train-score-model.md)