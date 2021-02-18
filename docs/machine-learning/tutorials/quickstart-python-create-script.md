---
title: 'Schnellstart: Ausführen von Python-Skripts'
titleSuffix: SQL machine learning
description: Ausführen einer Reihe einfacher Python-Skripts mit Machine Learning Services mit SQL Server, Big Data-Clustern oder Azure SQL Managed Instance-Instanzen. In diesem Artikel erfahren Sie, wie Sie die gespeicherte Prozedur „sp_execute_external_script“ verwenden, um das Skript auszuführen.
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: contperf-fy21q1
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: ddee800cdba3a90414b0ee9d3eb9638b9a343e67
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351537"
---
# <a name="quickstart-run-simple-python-scripts-with-sql-machine-learning"></a>Schnellstart: Ausführen einfacher Python-Skripts mit SQL Machine Learning
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

In diesem Schnellstart führen Sie eine Reihe einfacher Python-Skripts mit [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md), [Azure SQL Managed Instance Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview) oder [SQL Server Big Data-Cluster](../../big-data-cluster/machine-learning-services.md) aus. Sie erfahren, wie Sie die gespeicherte Prozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) verwenden, um das Skript in einer SQL Server-Instanz auszuführen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser Schnellstartanleitung benötigen Sie folgende Voraussetzungen.

- Eine SQL-Datenbank auf einer der folgenden Plattformen:
  - [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Informationen zur Installation finden Sie im [Windows-](../install/sql-machine-learning-services-windows-install.md) oder im [Linux-Installationsleitfaden](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json).
  - Big Data-Cluster für SQL Server. Weitere Informationen finden Sie unter [Aktivieren von Machine Learning Services auf SQL Server-Big Data-Clustern](../../big-data-cluster/machine-learning-services.md).
  - Machine Learning Services in Azure SQL Managed Instance. In der Übersicht [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) finden Sie weitere Informationen.

- Ein Tool zum Ausführen von SQL-Abfragen, die Python-Skripts enthalten. In dieser Schnellstartanleitung wird [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) verwendet.

## <a name="run-a-simple-script"></a>Ausführen eines einfachen Skripts

Sie können ein Python-Skript ausführen, indem Sie es als Argument an die gespeicherte Systemprozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) übergeben. Diese gespeicherte Systemprozedur startet die Python-Runtime im Kontext von SQL Machine Learning, übergibt Daten an Python, verwaltet Python-Sitzungen von Benutzern sicher und gibt alle Ergebnisse an den Client zurück.

In den folgenden Schritten führen Sie dieses Python-Beispielskript in Ihrer Datenbank aus:

```python
a = 1
b = 2
c = a/b
d = a*b
print(c, d)
```

1. Öffnen Sie ein neues Abfragefenster in **Azure Data Studio**, das mit Ihrer SQL -Instanz verbunden ist.

1. Übergeben Sie das gesamte Python-Skript an die gespeicherte Prozedur `sp_execute_external_script`.

   Das Skript wird durch das `@script`-Argument übergeben. Das `@script`-Argument darf nur aus zulässigem Python-Code bestehen.

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    a = 1
    b = 2
    c = a/b
    d = a*b
    print(c, d)
    '
    ```

1. Das korrekte Ergebnis wird berechnet, und die Python-Funktion `print` gibt das Ergebnis im **Meldungsfenster** zurück.

   Die Ausgabe könnte beispielsweise wie folgt aussehen:

    **Ergebnisse**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Ausführen eines „Hello World“-Skripts

In einem typischen Beispielskript wird nur die Zeichenfolge „Hallo Welt“ (oder „Hello World“) ausgegeben. Führen Sie den folgenden Befehl aus.

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'OutputDataSet = InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

Folgende Eingaben sind für die gespeicherte Prozedur `sp_execute_external_script` möglich:

| Eingabe | BESCHREIBUNG |
|-|-|
| @language | definiert die aufzurufende Spracherweiterung (hier Python) |
| @script | In diesem Argument werden die Befehle definiert, die an die Python-Runtime übergeben werden. Ihr gesamtes Python-Skript muss von diesem Argument als Unicode-Text umschlossen sein. Sie können den Text auch einer Variablen des Typs **nvarchar** hinzufügen und die Variable anschließend aufrufen. |
| @input_data_1 | Die von der Abfrage zurückgegebenen Daten werden an die Python-Runtime übergeben, die die Daten als Datenrahmen zurückgibt. |
| WITH RESULT SETS | Mit dieser Klausel wird das Schema der zurückgegebenen Datentabelle für SQL Machine Learning definiert und „Hello World“ als Spaltenname und **int** als Datentyp festgelegt. |

Der Befehl gibt folgenden Text aus:

| Hello World |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Verwenden von Eingaben und Ausgaben

Standardmäßig akzeptiert `sp_execute_external_script` ein einzelnes Dataset als Eingabe, das in der Regel in Form einer gültigen SQL-Abfrage angegeben wird. Anschließend wird ein einzelner Python-Datenrahmen als Ausgabe zurückgegeben.

Verwenden Sie vorerst die standardmäßig festgelegten Eingabe- und Ausgabevariablen von `sp_execute_external_script`: **InputDataSet** und **OutputDataSet**.

1. Erstellen Sie eine kleine Tabelle mit Testdaten.

    ```sql
    CREATE TABLE PythonTestData (col1 INT NOT NULL)
    
    INSERT INTO PythonTestData
    VALUES (1);
    
    INSERT INTO PythonTestData
    VALUES (10);
    
    INSERT INTO PythonTestData
    VALUES (100);
    GO
    ```

1. Verwenden Sie die `SELECT`-Anweisung zum Abfragen der Tabelle.
  
    ```sql
    SELECT *
    FROM PythonTestData
    ```

    **Ergebnisse**

    ![Inhalt der PythonTestData-Tabelle](./media/select-pythontestdata.png)

1. Führen Sie folgendes Python-Skript aus. Es ruft die Daten mithilfe der `SELECT`-Anweisung aus der Tabelle ab, übergibt sie über die Python-Runtime und gibt sie anschließend als Datenrahmen zurück. Die Klausel `WITH RESULT SETS` definiert das Schema der zurückgegeben Datentabelle für SQL und fügt den Spaltennamen *NewColName* hinzu.

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'OutputDataSet = InputDataSet;'
        , @input_data_1 = N'SELECT * FROM PythonTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Ergebnisse**

    ![Ausgabe des Python-Skripts, das Daten aus einer Tabelle zurückgibt](./media/python-output-pythontestdata.png)

1. Ändern Sie nun die Namen der Eingabe- und Ausgabevariablen. Standardmäßig werden die Eingabe- und Ausgabevariablen **InputDataSet** und **OutputDataSet** verwendet. In dem folgenden Skript werden die Namen in **SQL_in** und **SQL_out** geändert:

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'SQL_out = SQL_in;'
        , @input_data_1 = N'SELECT 12 as Col;'
        , @input_data_1_name  = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    Beachten Sie, dass Python Groß- und Kleinschreibung beachtet. Die im Python-Skript verwendeten Eingabe- und Ausgabevariablen (**SQL_out** und **SQL_in**) müssen mit den mit `@input_data_1_name` und `@output_data_1_name` definierten Namen (Groß-/Kleinschreibung beachten) übereinstimmen.

    > [!TIP]
    > Nur ein Eingabedataset kann als Parameter übergeben werden, und Sie können nur ein Dataset zurückgeben. Allerdings können Sie andere Datasets innerhalb Ihres Python-Codes aufrufen und Ausgaben und andere Typen zusätzlich zum Dataset zurückgeben. Außerdem können Sie auch allen Parametern das Schlüsselwort OUTPUT hinzufügen, damit es zusammen mit den Ergebnissen zurückgegeben wird.

1. Sie können über das Python-Skript auch Werte ohne Eingabedaten generieren, indem keine Eingabedaten für `@input_data_1` festgelegt werden.

    Das folgende Skript gibt den Text „hello“ und „world“ aus.

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    import pandas as pd
    mytextvariable = pandas.Series(["hello", " ", "world"]);
    OutputDataSet = pd.DataFrame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Ergebnisse**

    ![Abfrageergebnisse mit @script als Eingabe](./media/python-data-generated-output.png)

> [!NOTE]
> Python verwendet führende Leerzeichen für Gruppenanweisungen. Wenn also das eingebettete Python-Skript mehrere Zeilen wie im vorhergehenden Skript umfasst, versuchen Sie nicht, die Python-Befehle einzuziehen, um mit den SQL-Befehlen übereinstimmen zu können. Dieses Skript erzeugt beispielsweise einen Fehler:
>
> ```sql
> EXECUTE sp_execute_external_script @language = N'Python'
>       , @script = N'
>       import pandas as pd
>       mytextvariable = pandas.Series(["hello", " ", "world"]);
>       OutputDataSet = pd.DataFrame(mytextvariable);
>       '
>       , @input_data_1 = N''
> WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
> ```

## <a name="check-python-version"></a>Überprüfung der Python-Version

Wenn Sie ermitteln möchten, welche Version von Python auf Ihrem Server installiert ist, führen Sie das folgende Skript aus.

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import sys
print(sys.version)
'
GO
```

Die Python-Funktion `print` gibt die Version im **Meldungsfenster** zurück. In der folgenden Beispielausgabe sehen Sie, dass in diesem Fall Version 3.5.2 von Python installiert ist.

**Ergebnisse**

```text
STDOUT message(s) from external script:
3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]
```

## <a name="list-python-packages"></a>Auflistung der Python-Pakete

Microsoft stellt einige mit Machine Learning Services vorinstallierte Python-Pakete bereit.

Führen Sie das folgende Skript aus, um eine Liste der installierten Python-Pakete einschließlich der Version zu erhalten.

```SQL
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import pkg_resources
import pandas
dists = [str(d) for d in pkg_resources.working_set]
OutputDataSet = pandas.DataFrame(dists)
'
WITH RESULT SETS(([Package] NVARCHAR(max)))
GO
```

Die Liste stammt aus `pkg_resources.working_set` in Python und wird an SQL als Datenrahmen zurückgegeben.

**Ergebnisse**

:::image type="content" source="media/python-package-list.png" alt-text="Auflisten aller installierten Python-Pakete":::

## <a name="next-steps"></a>Nächste Schritte

Befolgen Sie diesen Schnellstart, um die Verwendung von Datenstrukturen mit Python in SQL Machine Learning zu erlernen:

> [!div class="nextstepaction"]
> [Schnellstart: Datenstrukturen und -objekte in Python](quickstart-python-data-structures.md)