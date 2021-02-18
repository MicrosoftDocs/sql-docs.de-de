---
title: 'Schnellstart: Ausführen von R-Skripts'
titleSuffix: SQL machine learning
description: Führen Sie einen Satz einfacher R-Skripts mit SQL Machine Learning aus. In diesem Artikel erfahren Sie, wie Sie die gespeicherte Prozedur „sp_execute_external_script“ verwenden, um das Skript auszuführen.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: eaecfcf9cd4944d48099a5bf1582e8144c1f62e1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272991"
---
# <a name="quickstart-run-simple-r-scripts-with-sql-machine-learning"></a>Schnellstart: Ausführen einfacher R-Skripts mit SQL Machine Learning
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In diesem Schnellstart führen Sie einige einfache R-Skripts mithilfe von [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) oder in [Big Data-Clustern](../../big-data-cluster/machine-learning-services.md) aus. Sie erfahren, wie Sie die gespeicherte Prozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) verwenden, um das Skript in einer SQL Server-Instanz auszuführen.
::: moniker-end
::: moniker range="=sql-server-2017"
In diesem Schnellstart führen Sie einige einfache R-Skripts mithilfe von [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) aus. Sie erfahren, wie Sie die gespeicherte Prozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) verwenden, um das Skript in einer SQL Server-Instanz auszuführen.
::: moniker-end
::: moniker range="=sql-server-2016"
In diesem Schnellstart führen Sie einige einfache R-Skripts mithilfe von [SQL Server R Services](../r/sql-server-r-services.md) aus. Sie erfahren, wie Sie die gespeicherte Prozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) verwenden, um das Skript in einer SQL Server-Instanz auszuführen.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In diesem Schnellstart führen Sie einige einfache R-Skripts mithilfe von [Machine Learning Services in Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview) aus. Sie erfahren, wie Sie die gespeicherte Prozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) verwenden, um Skripts in Ihrer Datenbank auszuführen.
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

## <a name="run-a-simple-script"></a>Ausführen eines einfachen Skripts

Sie können ein R-Skript ausführen, indem Sie es als Argument an die gespeicherte Systemprozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) übergeben. Diese gespeicherte Systemprozedur startet die R-Runtime, übergibt Daten an R, verwaltet Sitzungen von R-Benutzern sicher und gibt alle Ergebnisse an den Client zurück.

In den folgenden Schritten führen Sie dieses R-Beispielskript aus:

```r
a <- 1
b <- 2
c <- a/b
d <- a*b
print(c(c, d))
```

1. Öffnen Sie **Azure Data Studio**, und stellen Sie eine Verbindung mit Ihrem Server her.

1. Übergeben Sie das gesamte R-Skript an die gespeicherte Prozedur `sp_execute_external_script`.

   Das Skript wird durch das `@script`-Argument übergeben. Das `@script`-Argument darf nur aus zulässigem R-Code bestehen.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

1. Das korrekte Ergebnis wird berechnet, und die R-Funktion `print` gibt das Ergebnis im **Meldungsfenster** zurück.

   Die Ausgabe könnte beispielsweise wie folgt aussehen:

    **Ergebnisse**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Ausführen eines „Hello World“-Skripts

In einem typischen Beispielskript wird nur die Zeichenfolge „Hallo Welt“ (oder „Hello World“) ausgegeben. Führen Sie den folgenden Befehl aus.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'OutputDataSet<-InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

Folgende Eingaben sind für die gespeicherte Prozedur `sp_execute_external_script` möglich:

| Eingabe | BESCHREIBUNG |
|-|-|
| @language | definiert die aufzurufende Spracherweiterung (hier R) |
| @script | definiert die Befehle, die an die R-Runtime übergeben werden Ihr gesamtes R-Skript muss als Unicode-Text in dieses Argument eingeschlossen werden. Sie können den Text auch einer Variablen des Typs **nvarchar** hinzufügen und die Variable anschließend aufrufen. |
| @input_data_1 | Die von der Abfrage zurückgegebenen Daten werden an die R-Runtime übergeben, die die Daten als Datenrahmen zurückgibt. |
|WITH RESULT SETS | Mit dieser Klausel wird das Schema der zurückgegebenen Datentabelle definiert und „Hello World“ als Spaltenname und **int** als Datentyp festgelegt. |

Der Befehl gibt folgenden Text aus:

| Hello World |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Verwenden von Eingaben und Ausgaben

Standardmäßig akzeptiert `sp_execute_external_script` ein einzelnes Dataset als Eingabe, das in der Regel in Form einer gültigen SQL-Abfrage angegeben wird. Anschließend wird ein einzelner R-Datenrahmen als Ausgabe zurückgegeben.

Verwenden Sie vorerst die standardmäßig festgelegten Eingabe- und Ausgabevariablen von `sp_execute_external_script`: **InputDataSet** und **OutputDataSet**.

1. Erstellen Sie eine kleine Tabelle mit Testdaten.

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    
    INSERT INTO RTestData
    VALUES (1);
    
    INSERT INTO RTestData
    VALUES (10);
    
    INSERT INTO RTestData
    VALUES (100);
    GO
    ```

1. Verwenden Sie die `SELECT`-Anweisung zum Abfragen der Tabelle.
  
    ```sql
    SELECT *
    FROM RTestData
    ```

    **Ergebnisse**

    ![Inhalt der RTestData-Tabelle](./media/select-rtestdata.png)

1. Führen Sie folgendes R-Skript aus: Es ruft die Daten mithilfe der `SELECT`-Anweisung aus der Tabelle ab, übergibt sie über die R-Runtime und gibt sie anschließend als Datenrahmen zurück. Die Klausel `WITH RESULT SETS` definiert das Schema der zurückgegeben Datentabelle für SQL und fügt den Spaltennamen *NewColName* hinzu.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Ergebnisse**

    ![Ausgabe des R-Skripts, das Daten aus einer Tabelle zurückgibt](./media/r-output-rtestdata.png)

1. Ändern Sie nun die Namen der Eingabe- und Ausgabevariablen. Standardmäßig werden die Eingabe- und Ausgabevariablen **InputDataSet** und **OutputDataSet** verwendet. In diesem Skript werden die Namen in **SQL_in** und **SQL_out** geändert:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N' SQL_out <- SQL_in;'
        , @input_data_1 = N' SELECT 12 as Col;'
        , @input_data_1_name = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    Beachten Sie, dass R Groß- und Kleinschreibung beachtet. Die im R-Skript verwendeten Eingabe- und Ausgabevariablen (**SQL_out** und **SQL_in**) müssen mit den mit `@input_data_1_name` und `@output_data_1_name` definierten Namen (Groß-/Kleinschreibung beachten) identisch sein.

   > [!TIP]
   > Nur ein Eingabedataset kann als Parameter übergeben werden, und Sie können nur ein Dataset zurückgeben. Sie können aber andere Datasets in Ihrem R-Code aufrufen und zusätzlich zum Dataset Ausgaben anderer Typen zurückgeben. Außerdem können Sie auch allen Parametern das Schlüsselwort OUTPUT hinzufügen, damit es zusammen mit den Ergebnissen zurückgegeben wird.

1. Sie können über das R-Skript auch Werte ohne Eingabedaten generieren, indem kein Wert für `@input_data_1` festgelegt wird.

   Das folgende Skript gibt den Text „hello“ und „world“ aus.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    mytextvariable <- c("hello", " ", "world");
    OutputDataSet <- as.data.frame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Ergebnisse**

    ![Abfrageergebnisse mit @script als Eingabe](./media/r-data-generated-output.png)

## <a name="check-r-version"></a>Überprüfen der R-Version

Wenn Sie ermitteln möchten, welche Version von R installiert ist, führen Sie das folgende Skript aus.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(version)';
GO
```

Die R-Funktion `print` gibt die Version im **Meldungsfenster** zurück. In der folgenden Beispielausgabe sehen Sie, dass in diesem Fall Version 3.4.4 von R installiert ist.

**Ergebnisse**

```text
STDOUT message(s) from external script:
                   _
platform       x86_64-w64-mingw32
arch           x86_64
os             mingw32
system         x86_64, mingw32
status
major          3
minor          4.4
year           2018
month          03
day            15
svn rev        74408
language       R
version.string R version 3.4.4 (2018-03-15)
nickname       Someone to Lean On
```

## <a name="list-r-packages"></a>Auflisten von R-Paketen
::: moniker range=">=sql-server-2017"
Microsoft bietet eine Reihe von R-Paketen, die mit Machine Learning Services vorinstalliert werden.
::: moniker-end
::: moniker range="=sql-server-2016"
Microsoft bietet eine Reihe von R-Paketen, die mit R Services vorinstalliert werden.
::: moniker-end

Führen Sie folgendes Skript aus, um eine Liste der installierten R-Pakete (einschließlich Version, Abhängigkeiten, Lizenz und Bibliothekspfad) anzuzeigen:

```SQL
EXEC sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            , LibPath NVARCHAR(2000)
            ));
```

Die Ausgabe stammt von `installed.packages()` in R und wird als Resultset zurückgegeben.

**Ergebnisse**

![Installierte Pakete in R](./media/rsql-installed-packages.png) 

## <a name="next-steps"></a>Nächste Schritte

Befolgen Sie diesen Schnellstart, um die Verwendung von Datenstrukturen zu erlernen, wenn R mit SQL Machine Learning verwendet wird:

> [!div class="nextstepaction"]
> [Arbeiten mit Datentypen und -objekten mithilfe von R mit SQL Machine Learning](quickstart-r-data-types-and-objects.md)