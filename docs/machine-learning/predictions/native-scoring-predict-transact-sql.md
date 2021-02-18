---
title: Native Bewertung mit der T-SQL-Funktion PREDICT
titleSuffix: SQL machine learning
description: Hier erfahren Sie, wie Sie die native Bewertung mit der T-SQL-Funktion PREDICT verwenden, um Vorhersagewerte für neue Dateneingaben nahezu in Echtzeit zu generieren.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest'
ms.openlocfilehash: 74fc66dd6ed235d7508f774e7397c9b4d55e80a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352538"
---
# <a name="native-scoring-using-the-predict-t-sql-function-with-sql-machine-learning"></a>Native Bewertung mithilfe der PREDICT-T-SQL-Funktion mit SQL Machine Learning

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Hier erfahren Sie, wie Sie die native Bewertung mit der [T-SQL-Funktion PREDICT](../../t-sql/queries/predict-transact-sql.md) verwenden, um Vorhersagewerte für neue Dateneingaben nahezu in Echtzeit zu generieren. Für die native Bewertung ist ein bereits trainiertes Modell erforderlich.

Die `PREDICT`-Funktion verwendet die nativen Funktionen der C++-Erweiterung in [SQL Machine Learning](../index.yml). Diese Methodik bietet die schnellste Verarbeitungsgeschwindigkeit von Vorhersage- und Prognoseworkloads und unterstützt Modelle im [ONNX](https://onnx.ai/get-started.html)-Format (Open Neural Network Exchange) oder Modelle, die mithilfe der [RevoScaleR](../r/ref-r-revoscaler.md)- und [revoscalepy](../python/ref-py-revoscalepy.md)-Pakete trainiert werden.

## <a name="how-native-scoring-works"></a>Funktionsweise der nativen Bewertung

Die native Bewertung verwendet Bibliotheken, die Modelle im ONNX-Format oder einem vordefinierten Binärformat lesen und Ergebnisse für neue Dateneingaben generieren können, die Sie bereitstellen. Da das Modell trainiert, bereitgestellt und gespeichert wird, kann es für die Bewertung verwendet werden, ohne den R- oder Python-Interpreter aufrufen zu müssen. Das bedeutet, dass der Aufwand für mehrere Prozessinteraktionen verringert wird, was wesentlich schnellere Vorhersagen ermöglicht.

Rufen Sie zur Verwendung der nativen Bewertung die T-SQL-Funktion `PREDICT` auf, und übergeben Sie die folgenden erforderlichen Eingaben:

+ ein kompatibles Modell, das auf einem unterstützten Modell und Algorithmus basiert
+ Eingabedaten, die in der Regel als SQL-Abfrage definiert sind

Die Funktion gibt Vorhersagen für die Eingabedaten zurück, zusammen mit allen Spalten der Quelldaten, die Sie durchlaufen möchten.

## <a name="prerequisites"></a>Voraussetzungen

`PREDICT` ist verfügbar in den folgenden Versionen:

+ alle Editionen von SQL Server 2017 oder höher unter Windows und Linux
+ Verwaltete Azure SQL-Instanz
+ Azure SQL-Datenbank
+ Azure SQL Edge
+ Azure Synapse Analytics

Die Funktion ist standardmäßig aktiviert. Sie müssen R bzw. Python nicht installieren oder zusätzliche Features aktivieren.

## <a name="supported-models"></a>Unterstützte Modelle

Welche Modellformate von der `PREDICT`-Funktion unterstützt werden, hängt von der SQL-Plattform ab, auf der Sie die native Bewertung durchführen. In der folgenden Tabelle sehen Sie, welche Modellformate auf welcher Plattform unterstützt werden.

| Plattform | ONNX-Modellformat | RevoScale-Modellformat |
|-|-|-|
| SQL Server | Nein | Ja |
| Verwaltete Azure SQL-Instanz | Ja | Ja |
| Azure SQL-Datenbank | Nein | Ja |
| Azure SQL Edge | Ja | Nein |
| Azure Synapse Analytics | Ja | Nein |

::: moniker range="=azuresqldb-mi-current||=azure-sqldw-latest"
### <a name="onnx-models"></a>ONNX-Modelle

Das Modell muss im [ONNX-Modellformat (Open Neural Network Exchange)](https://onnx.ai/get-started.html) vorliegen.
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current||=azuresqldb-current"
### <a name="revoscale-models"></a>RevoScale-Modelle

Das Modell muss vorab mithilfe des [RevoScaleR](../r/ref-r-revoscaler.md)- oder [revoscalepy](../python/ref-py-revoscalepy.md)-Pakets mit einem der unterstützten **rx**-Algorithmen trainiert werden, die unten aufgeführt sind.

Serialisieren Sie das Modell mithilfe von [rxSerialize](/machine-learning-server/r-reference/revoscaler/rxserializemodel) für R und [rx_serialize_model](/machine-learning-server/python-reference/revoscalepy/rx-serialize-model) für Python. Diese Serialisierungsfunktionen wurden zur Unterstützung einer schnellen Bewertung optimiert.

<a name="bkmk_native_supported_algos"></a> 

#### <a name="supported-revoscale-algorithms"></a>Unterstützte RevoScale-Algorithmen

Die folgenden Algorithmen werden in revoscalepy und RevoScaleR unterstützt.

+ revoscalepy-Algorithmen

  + [rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod)
  + [rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit) 
  + [rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees) 
  + [rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree) 
  + [rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest) 

+ RevoScaleR-Algorithmen

  + [rxLinMod](/r-server/r-reference/revoscaler/rxlinmod)
  + [rxLogit](/r-server/r-reference/revoscaler/rxlogit)
  + [rxBTrees](/r-server/r-reference/revoscaler/rxbtrees)
  + [rxDtree](/r-server/r-reference/revoscaler/rxdtree)
  + [rxDForest](/r-server/r-reference/revoscaler/rxdforest)

Wenn Sie einen Algorithmus aus MicrosoftML oder microsoftml verwenden müssen, verwenden Sie [Echtzeitbewertung mit sp_rxPredict](../predictions/real-time-scoring.md).

Die folgenden Modelltypen werden nicht unterstützt:

+ Modelle, die andere Transformationen enthalten
+ Modelle, die die `rxGlm`- oder `rxNaiveBayes`-Algorithmen in Entsprechungen von RevoScaleR oder revoscalepy verwenden
+ PMML-Modelle
+ Mit anderen Open-Source- oder Drittanbieterbibliotheken erstellte Modelle
::: moniker-end

## <a name="examples"></a>Beispiele
::: moniker range="=azuresqldb-mi-current||=azure-sqldw-latest"
### <a name="predict-with-an-onnx-model"></a>PREDICT mit einem ONNX-Modell

In diesem Beispiel wird gezeigt, wie ein in der `dbo.models`-Tabelle gespeichertes ONNX-Modell für die native Bewertung verwendet wird.

```sql
DECLARE @model VARBINARY(max) = (
        SELECT DATA
        FROM dbo.models
        WHERE id = 1
        );

WITH predict_input
AS (
    SELECT TOP (1000) [id]
        , CRIM
        , ZN
        , INDUS
        , CHAS
        , NOX
        , RM
        , AGE
        , DIS
        , RAD
        , TAX
        , PTRATIO
        , B
        , LSTAT
    FROM [dbo].[features]
    )
SELECT predict_input.id
    , p.variable1 AS MEDV
FROM PREDICT(MODEL = @model, DATA = predict_input, RUNTIME=ONNX) WITH (variable1 FLOAT) AS p;
```

> [!NOTE]
> Da die von **PREDICT** zurückgegebenen Spalten und Werte je nach Modelltyp variieren können, müssen Sie das Schema der zurückgegebenen Daten mithilfe einer **WITH**-Klausel definieren.
::: moniker-end

::: moniker range=">=sql-server-2017||=azuresqldb-mi-current||>=sql-server-linux-2017"
### <a name="predict-with-revoscale-model"></a>PREDICT mit einem RevoScale-Modell

In diesem Beispiel erstellen Sie mithilfe von **RevoScaleR** in R ein Modell und rufen dann die T-SQL-Funktion für Vorhersagen in Echtzeit auf.

#### <a name="step-1-prepare-and-save-the-model"></a>Schritt 1: Vorbereiten und Speichern des Modells

Führen Sie den folgenden Code aus, um die Beispieldatenbank und die erforderlichen Tabellen zu erstellen.

```sql
CREATE DATABASE NativeScoringTest;
GO
USE NativeScoringTest;
GO
DROP TABLE IF EXISTS iris_rx_data;
GO
CREATE TABLE iris_rx_data (
    "Sepal.Length" float not null, "Sepal.Width" float not null
  , "Petal.Length" float not null, "Petal.Width" float not null
  , "Species" varchar(100) null
);
GO
```

Verwenden Sie die folgende Anweisung, um die Datentabelle mit Daten aus dem **Iris**-Dataset aufzufüllen.

```sql
INSERT INTO iris_rx_data ("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width" , "Species")
EXECUTE sp_execute_external_script
  @language = N'R'
  , @script = N'iris_data <- iris;'
  , @input_data_1 = N''
  , @output_data_1_name = N'iris_data';
GO
```

Erstellen Sie nun eine Tabelle zum Speichern von Modellen.

```sql
DROP TABLE IF EXISTS ml_models;
GO
CREATE TABLE ml_models ( model_name nvarchar(100) not null primary key
  , model_version nvarchar(100) not null
  , native_model_object varbinary(max) not null);
GO
```

Mit dem folgenden Code wird ein Modell erstellt, das auf dem **Iris**-Dataset basiert und in der Tabelle mit dem Namen **Modelle** gespeichert wird.

```sql
DECLARE @model varbinary(max);
EXECUTE sp_execute_external_script
  @language = N'R'
  , @script = N'
    iris.sub <- c(sample(1:50, 25), sample(51:100, 25), sample(101:150, 25))
    iris.dtree <- rxDTree(Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width, data = iris[iris.sub, ])
    model <- rxSerializeModel(iris.dtree, realtimeScoringOnly = TRUE)
    '
  , @params = N'@model varbinary(max) OUTPUT'
  , @model = @model OUTPUT
  INSERT [dbo].[ml_models]([model_name], [model_version], [native_model_object])
  VALUES('iris.dtree','v1', @model) ;
```

> [!NOTE]
> Stellen Sie sicher, dass Sie die [rxSerializeModel](/machine-learning-server/r-reference/revoscaler/rxserializemodel)-Funktion von RevoScaleR verwenden, um das Modell zu speichern. Die standardmäßige R-Funktion `serialize` kann das erforderliche Format nicht generieren.

Sie können eine Anweisung wie die folgende ausführen, um das gespeicherte Modell im Binärformat anzuzeigen:

```sql
SELECT *, datalength(native_model_object)/1024. as model_size_kb
FROM ml_models;
```

#### <a name="step-2-run-predict-on-the-model"></a>Schritt 2: Ausführen von PREDICT für das Modell

Die folgende einfache PREDICT-Anweisung ruft eine Klassifizierung aus dem Entscheidungsstrukturmodell mithilfe der Funktion für die **native Bewertung** ab. Die Irisarten werden anhand der von Ihnen angegebenen Attribute, der Länge und Breite der Blütenblätter, vorhergesagt.

```sql
DECLARE @model varbinary(max) = (
  SELECT native_model_object
  FROM ml_models
  WHERE model_name = 'iris.dtree'
  AND model_version = 'v1');
SELECT d.*, p.*
  FROM PREDICT(MODEL = @model, DATA = dbo.iris_rx_data as d)
  WITH(setosa_Pred float, versicolor_Pred float, virginica_Pred float) as p;
go
```

Wenn ein Fehler wie „Fehler bei der Ausführung der Funktion PREDICT. Modell ist beschädigt oder ungültig.“ erhalten, bedeutet das normalerweise, dass die Abfrage kein Modell zurückgegeben hat. Überprüfen Sie, ob Sie den Modellnamen ordnungsgemäß eingegeben haben oder ob die Tabelle des Modells leer ist.

> [!NOTE]
> Da die von **PREDICT** zurückgegebenen Spalten und Werte je nach Modelltyp variieren können, müssen Sie das Schema der zurückgegebenen Daten mithilfe einer **WITH**-Klausel definieren.
::: moniker-end

## <a name="next-steps"></a>Nächste Schritte

+ [T-SQL-Funktion PREDICT](../../t-sql/queries/predict-transact-sql.md)
+ [Machine-Learning-Dokumentation für SQL](../index.yml)
+ [Machine Learning und KI mit ONNX in SQL Edge (Vorschau)](/azure/azure-sql-edge/onnx-overview)
+ [Bereitstellen eines ONNX-Modells und Treffen von Vorhersagen damit in Azure SQL Edge (Vorschau)](/azure/azure-sql-edge/deploy-onnx)