---
title: 'Schnellstart: Python-Datenstrukturen'
titleSuffix: SQL machine learning
description: In diesem Schnellstart erfahren Sie, wie Sie mit SQL Machine Learning in Python mit Datenstrukturen und Datenobjekten arbeiten.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 3d24323019904ea144fa0029b8bfd5065f4a4a30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351518"
---
# <a name="quickstart-data-structures-and-objects-using-python-with-sql-machine-learning"></a>Schnellstart: Datenstrukturen und -objekte in Python mit SQL Machine Learning
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

In diesem Schnellstart erfahren Sie, wie Sie mit Python Datenstrukturen und Datentypen in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md), [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview) oder [SQL Server Big Data-Cluster](../../big-data-cluster/machine-learning-services.md) verwenden. Sie erhalten Informationen zum Verschieben von Daten zwischen Python und SQL Server und zu Fehlern, die in diesem Zusammenhang häufig auftreten.

SQL Machine Learning ist vom Python-Paket **pandas** abhängig, das hervorragend für die Arbeit mit Tabellendaten geeignet ist. Sie können allerdings nicht einen Skalar von Python an Ihre Datenbank übergeben und erwarten, dass alles *einfach funktioniert*. Im Rahmen dieses Schnellstarts überprüfen Sie einige grundlegende Datenstrukturdefinitionen. Dies soll Sie auf weitere Probleme vorbereiten, die bei der Übergabe von Tabellendaten zwischen Python und der Datenbank möglicherweise auftreten.

Sie sollten sich im Vorfeld über die folgenden Konzepte informieren:

- Ein Datenrahmen ist eine Tabelle mit _mehreren_ Spalten.
- Eine einzelne Spalte eines Datenrahmens ist ein listenähnliches Objekt, das als Reihe bezeichnet wird.
- Ein einzelner Wert eines Datenrahmens wird als Zelle bezeichnet, und der Zugriff erfolgt über den Index.

Wie würden Sie das einzelne Ergebnis einer Berechnung als Datenrahmen verfügbar machen, wenn für data.frame ein Tabellenformat verlangt wird? Eine Möglichkeit ist die Darstellung des einzelnen Skalarwerts als Reihe, die ganz einfach in einen Datenrahmen konvertiert werden kann.

> [!NOTE]
> Beim Zurückgeben von Datumsangaben verwendet Python in SQL DATETIME mit dem eingeschränkten Datumsbereich von 1753-01-01 (-53690) bis 9999-12-31 (2958463).

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser Schnellstartanleitung benötigen Sie folgende Voraussetzungen.

- Eine SQL-Datenbank auf einer der folgenden Plattformen:
  - [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Informationen zur Installation finden Sie im [Windows-](../install/sql-machine-learning-services-windows-install.md) oder im [Linux-Installationsleitfaden](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Big Data-Cluster für SQL Server. Weitere Informationen finden Sie unter [Aktivieren von Machine Learning Services auf SQL Server-Big Data-Clustern](../../big-data-cluster/machine-learning-services.md).
  - Machine Learning Services in Azure SQL Managed Instance. In der Übersicht [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) finden Sie weitere Informationen.

- Ein Tool zum Ausführen von SQL-Abfragen, die Python-Skripts enthalten. In dieser Schnellstartanleitung wird [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) verwendet.

## <a name="scalar-value-as-a-series"></a>Skalarwert als Reihe

In diesem Beispiel wird einfache Mathematik verwendet und ein Skalar in eine Reihe konvertiert.

1. Für eine Reihe wird ein Index benötigt, den Sie manuell (wie hier gezeigt) oder programmgesteuert zuweisen können.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   a = 1
   b = 2
   c = a/b
   print(c)
   s = pandas.Series(c, index =["simple math example 1"])
   print(s)
   '
   ```

   Da die Reihe nicht in ein data.frame-Format konvertiert wurde, werden die Werte im Fenster „Meldungen“angezeigt – und zwar in tabellarischer Form.

   **Ergebnisse**

   ```text
   STDOUT message(s) from external script: 
   0.5
   simple math example 1    0.5
   dtype: float64
   ```

1. Sie können Reihen verlängern, indem Sie mithilfe eines Arrays neue Werte hinzufügen.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   a = 1
   b = 2
   c = a/b
   d = a*b
   s = pandas.Series([c,d])
   print(s)
   '
   ```

   Wenn Sie keinen Index angeben, wird ein Index mit Werten generiert, die mit 0 beginnen und mit der Länge des Arrays enden.

   **Ergebnisse**

   ```text
   STDOUT message(s) from external script:
   0    0.5
   1    2.0
   dtype: float64
   ```

1. Wenn Sie die Anzahl der **Indexwerte** erhöhen, aber keine neuen **Datenwerte** hinzufügen, werden die Datenwerte mehrfach verwendet, um die Reihen auszufüllen.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   a = 1
   b = 2
   c = a/b
   s = pandas.Series(c, index =["simple math example 1", "simple math example 2"])
   print(s)
   '
   ```

   **Ergebnisse**

   ```text
   STDOUT message(s) from external script:
   0.5
   simple math example 1    0.5
   simple math example 2    0.5
   dtype: float64
   ```

## <a name="convert-series-to-data-frame"></a>Konvertieren von Reihen in Datenrahmen

Auch wenn Sie die mathematischen Skalarergebnisse in ein Tabellenformat konvertiert haben, müssen Sie sie noch in ein Format konvertieren, das SQL Machine Learning verarbeiten kann.

1. Sie können Reihe in ein data.frame-Format konvertieren, indem Sie die Pandas-Methode [DataFrame](https://pandas.pydata.org/pandas-docs/stable/dsintro.html#dataframe) aufrufen.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   import pandas as pd
   a = 1
   b = 2
   c = a/b
   d = a*b
   s = pandas.Series([c,d])
   print(s)
   df = pd.DataFrame(s)
   OutputDataSet = df
   '
   WITH RESULT SETS((ResultValue FLOAT))
   ```

   Das Ergebnis wird unten angezeigt. Auch wenn Sie den Index verwenden, um bestimmte Werte aus data.frame zu erhalten, sind die Indexwerte nicht Teil der Ausgabe.

   **Ergebnisse**

   |ResultValue|
   |------|
   |0.5|
   |2|

## <a name="output-values-into-dataframe"></a>Ausgeben von Werten in data.frame

Im Folgenden sollten Sie spezifische Werte aus zwei Datenreihen der mathematischen Ergebnisse im data.frame-Format ausgeben. Die erste Datenreihe verfügt über einen Index von sequenziellen Werten, die von Python generiert werden. Die zweite Datenreihe verwendet einen beliebigen Index von Zeichenfolgenwerten.

1. Im folgenden Beispiel wird mithilfe eines ganzzahligen Index ein Wert aus der Reihe abgerufen.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   import pandas as pd
   a = 1
   b = 2
   c = a/b
   d = a*b
   s = pandas.Series([c,d])
   print(s)
   df = pd.DataFrame(s, index=[1])
   OutputDataSet = df
   '
   WITH RESULT SETS((ResultValue FLOAT))
   ```

   **Ergebnisse**

   |ResultValue|
   |------|
   |2.0|

   Wie bereits erwähnt, beginnt der automatisch generierte Index bei 0. Verwenden Sie einen Indexwert außerhalb des gültigen Bereichs, um zu prüfen, was dadurch passiert.

1. Rufen Sie nun mithilfe eines Zeichenfolgenindex einen einzelnen Wert aus dem anderen Datenrahmen ab.

   ```sql
   EXECUTE sp_execute_external_script @language = N'Python'
       , @script = N'
   import pandas as pd
   a = 1
   b = 2
   c = a/b
   s = pandas.Series(c, index =["simple math example 1", "simple math example 2"])
   print(s)
   df = pd.DataFrame(s, index=["simple math example 1"])
   OutputDataSet = df
   '
   WITH RESULT SETS((ResultValue FLOAT))
   ```

   **Ergebnisse**

   |ResultValue|
   |------|
   |0.5|

   Wenn Sie versuchen, einen numerischen Index zu verwenden, um einen Wert aus dieser Reihe abzurufen, wird ein Fehler ausgelöst.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Erstellen von erweiterten Python-Funktionen mit SQL Machine Learning finden Sie in diesem Schnellstart:

> [!div class="nextstepaction"]
> [Python-Funktionen mit SQL Machine Learning](quickstart-python-functions.md)