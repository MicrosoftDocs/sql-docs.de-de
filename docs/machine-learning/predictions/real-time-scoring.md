---
title: Echtzeitbewertung mit sp_rxPredict
description: Erfahren Sie, wie Sie mit der gespeicherten Systemprozedur „sp_rxPredict“ in SQL Server eine Echtzeitbewertung für Hochleistungsvorhersagen oder Bewertungen bei der Vorhersage von Workloads durchführen können.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/31/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 89e7a3e15f389de8fc696247197c9f678955ed73
ms.sourcegitcommit: 5f658b286f56001b055a8898d97e74906516dc99
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90009346"
---
# <a name="real-time-scoring-with-sp_rxpredict-in-sql-server"></a>Echtzeitbewertung mit sp_rxPredict in SQL Server
[!INCLUDE[sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

Erfahren Sie, wie Sie mit der gespeicherten Systemprozedur [sp_rxPredict](../../relational-databases/system-stored-procedures/sp-rxpredict-transact-sql.md) in SQL Server eine Echtzeitbewertung für Hochleistungsvorhersagen oder Bewertungen bei der Vorhersage von Workloads durchführen können.

Die Echtzeitbewertung mit `sp_rxPredict` ist sprachunabhängig und wird ohne Abhängigkeiten von den R- oder Python-Runtimes in [Machine Learning Services](../sql-server-machine-learning-services.md) oder [R Services](../r/sql-server-r-services.md) ausgeführt. Bei einem Modell, das mithilfe von Microsoft-Funktionen erstellt und trainiert und anschließend in SQL Server in ein Binärformat serialisiert wurde, können Sie die Echtzeitbewertung verwenden, um vorhergesagte Ergebnisse für neue Dateneingaben auf SQL Server-Instanzen zu generieren, auf denen das R- oder Python-Add-On nicht installiert ist.

## <a name="how-real-time-scoring-works"></a>Funktionsweise der Echtzeitbewertung

Die Echtzeitbewertung wird für bestimmte Modelltypen unterstützt, die auf Funktionen in [RevoScaleR](../r/ref-r-revoscaler.md) oder [MicrosoftML](../r/ref-r-microsoftml.md) in R oder [Revoscalepy](../python/ref-py-revoscalepy.md) oder [Microsoftml](../python/ref-py-microsoftml.md) in Python basieren. Es werden dabei native C++-Bibliotheken verwendet, um Bewertungen basierend auf Benutzereingaben zu generieren, die einem Machine Learning-Modell bereitgestellt werden, das in einem speziellen Binärformat gespeichert ist.

Da ein trainiertes Modell zur Bewertung verwendet werden kann, ohne eine externe Sprachlaufzeit in [Machine Learning Services](../sql-server-machine-learning-services.md) oder [R Services](../r/sql-server-r-services.md) aufrufen zu müssen, verringert sich der Aufwand für mehrere Prozesse. 

Die Echtzeitbewertung ist ein mehrstufiger Prozess:

1. Die gespeicherte Prozedur, mit der die Bewertung durchgeführt wird, muss für jede Datenbank einzeln aktiviert werden.
2. Sie laden das vortrainierte Modell im Binärformat.
3. Sie stellen neue Eingabedaten, die bewertet werden sollen, entweder in tabellarischer Form oder in Form einzelner Zeilen als Eingabe für das Modell bereit.
4. Zum Generieren von Bewertungen rufen Sie die gespeicherte Prozedur [sp_rxPredict](../../relational-databases/system-stored-procedures/sp-rxpredict-transact-sql.md) auf.

## <a name="prerequisites"></a>Voraussetzungen

+ [Aktivieren Sie die CLR-Integration in SQL Server.](../../relational-databases/clr-integration/clr-integration-enabling.md)

+ [Aktivieren Sie die Echtzeitbewertung](#bkmk_enableRtScoring).

+ Das Modell muss vorab mit einem der unterstützten **rx**-Algorithmen trainiert werden. Für R wird die Echtzeitbewertung mit `sp_rxPredict` mit [RevoScaleR- und MicrosoftML-unterstützten Algorithmen](#bkmk_rt_supported_algos) durchgeführt. Informationen zu Python finden Sie unter [revoscalepy- and microsoftml-unterstützte Algorithmen](#bkmk_py_supported_algos).

+ Serialisieren Sie das Modell mithilfe von [rxSerialize](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxserializemodel) für R und [rx_serialize_model](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-serialize-model) für Python. Diese Serialisierungsfunktionen wurden zur Unterstützung einer schnellen Bewertung optimiert.

+ Speichern Sie das Modell auf der Datenbank-Engine-Instanz, von der Sie es aufrufen möchten. Diese Instanz muss nicht über die R- oder Python-Laufzeiterweiterung verfügen.

> [!Note]
> Die Echtzeitbewertung ist derzeit für schnelle Vorhersagen für kleinere Datasets optimiert, die von einigen wenigen Zeilen bis hin zu Hunderttausenden von Zeilen reichen. Bei großen Datasets kann die Verwendung von [rxPredict](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict) schneller sein.

<a name ="bkmk_enableRtScoring"></a> 

## <a name="enable-real-time-scoring"></a>Aktivieren der Echtzeitbewertung

Sie müssen diese Funktion für jede Datenbank aktivieren, die Sie für die Bewertung verwenden wollen. Der Serveradministrator sollte das Befehlszeilen-Hilfsprogramm RegisterRExt.exe ausführen, das im RevoScaleR-Paket enthalten ist.

> [!NOTE]
> Damit die Echtzeitbewertung funktioniert, muss die SQL CLR-Funktionalität auf der Instanz aktiviert werden. Außerdem muss die Datenbank als vertrauenswürdig gekennzeichnet sein. Wenn Sie das Skript ausführen, werden diese Aktionen für Sie durchgeführt. Beachten Sie jedoch die zusätzlichen Sicherheitsaspekte, bevor Sie dies tun!

1. Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, und navigieren Sie zu dem Ordner, in dem sich RegisterRExt.exe befindet. Bei einer Standardinstallation kann der folgende Pfad verwendet werden:

    `<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\`

2. Führen Sie den folgenden Befehl aus, und setzen Sie dabei den Namen Ihrer Instanz und die Zieldatenbank ein, in der Sie die erweiterten gespeicherten Prozeduren aktivieren möchten:

    `RegisterRExt.exe /installRts [/instance:name] /database:databasename`

    Geben Sie beispielsweise Folgendes ein, um die erweiterte gespeicherte Prozedur der CLRPredict-Datenbank auf der Standardinstanz hinzuzufügen:

    `RegisterRExt.exe /installRts /database:CLRPRedict`

    Der Instanzname ist optional, wenn sich die Datenbank auf der Standardinstanz befindet. Wenn Sie eine benannte Instanz verwenden, müssen Sie den Instanznamen angeben.

3. RegisterRExt.exe erstellt die folgenden Objekte:

    + Vertrauenswürdige Assemblys.
    + Die gespeicherte Prozedur `sp_rxPredict`.
    + Eine neue Datenbankrolle `rxpredict_users`. Der Datenbankadministrator kann diese Rolle zum Erteilen von Berechtigungen für Benutzer verwenden, die die Echtzeitbewertungsfunktionalität nutzen.

4. Fügen Sie alle Benutzer, die `sp_rxPredict` ausführen müssen, der neuen Rolle hinzu.

> [!NOTE]
>
> In SQL Server 2017 und höher sind zusätzliche Sicherheitsmaßnahmen vorhanden, um Probleme bei der CLR-Integration zu vermeiden. Diese Maßnahmen bewirken zusätzliche Einschränkungen bei der Verwendung dieser gespeicherten Prozedur.

## <a name="disable-real-time-scoring"></a>Deaktivieren der Echtzeitbewertung

Zum Deaktivieren der Echtzeitbewertungsfunktionalität öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, und führen Sie den folgenden Befehl aus: `RegisterRExt.exe /uninstallrts /database:<database_name> [/instance:name]`

<a name="bkmk_py_supported_algos"></a>

## <a name="supported-algorithms"></a>Unterstützte Algorithmen

### <a name="python-algorithms-using-real-time-scoring"></a>Python-Algorithmen mit Echtzeitbewertung

+ revoscalepy-Modelle

  + [rx_lin_mod](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-lin-mod) \*
  + [rx_logit](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-logit) \*
  + [rx_btrees](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-btrees) \*
  + [rx_dtree](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-dtree) \*
  + [rx_dforest](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-dforest) \*
  
  Mit \* markierte Modelle unterstützen außerdem die native Bewertung mit der PREDICT-Funktion.

+ microsoftml-Modelle

  + [rx_fast_trees](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/rx-fast-trees)
  + [rx_fast_forest](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/rx-fast-forest)
  + [rx_logistic_regression](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/rx-logistic-regression)
  + [rx_oneclass_svm](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/rx-oneclass-svm)
  + [rx_neural_net](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/rx-neural-network)
  + [rx_fast_linear](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/rx-fast-linear)

+ Von microsoftml bereitgestellte Transformationen

  + [featurize_text](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/featurize-text)
  + [concat](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/concat)
  + [categorical](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/categorical)
  + [categorical_hash](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/categorical-hash)

<a name="bkmk_rt_supported_algos"></a>

### <a name="r-algorithms-using-real-time-scoring"></a>R-Algorithmen mit Echtzeitbewertung

+ RevoScaleR-Modelle

  + [rxLinMod](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxlinmod) \*
  + [rxLogit](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxlogit) \*
  + [rxBTrees](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxbtrees) \*
  + [rxDtree](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdtree) \*
  + [rxdForest](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdforest) \*
  
  Mit \* markierte Modelle unterstützen außerdem die native Bewertung mit der PREDICT-Funktion.

+ MicrosoftML-Modelle

  + [rxFastTrees](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [rxFastForest](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxfastforest)
  + [rxLogisticRegression](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxlogisticregression)
  + [rxOneClassSvm](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxoneclasssvm)
  + [rxNeuralNet](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxneuralnet)
  + [rxFastLinear](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxfastlinear)

+ Von MicrosoftML bereitgestellte Transformationen

  + [featurizeText](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [concat](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/concat)
  + [categorical](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/categorical)
  + [categoricalHash](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/categoricalHash)
  + [selectFeatures](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/selectFeatures)

### <a name="unsupported-model-types"></a>Nicht unterstützte Modelltypen

Die Echtzeitbewertung verwendet keinen Interpreter. Daher wird jede Funktionalität, die möglicherweise einen Interpreter erfordert, während des Bewertungsschritts nicht unterstützt.  Dazu kann Folgendes gehören:

+ Modelle, die den Algorithmus `rxGlm` oder `rxNaiveBayes` verwenden, werden nicht unterstützt.

+ Modelle, die eine Transformationsfunktion oder eine Formel mit einer Transformation enthalten (z.B. `A ~ log(B`), werden bei der Echtzeitbewertung nicht unterstützt. Wenn Sie ein Modell dieses Typs verwenden möchten, wird empfohlen, die Transformation für Eingabedaten durchzuführen, bevor die Daten an die Echtzeitbewertung übergeben werden.

## <a name="example"></a>Beispiel

In diesem Abschnitt werden die Schritte beschrieben, die zum Vorbereiten und Speichern eines Modells für die **Echtzeitvorhersage** erforderlich sind. Außerdem ist ein Beispiel in R für das Aufrufen der Funktion aus T-SQL enthalten.

### <a name="step-1-prepare-and-save-the-model"></a>Schritt 1: Vorbereiten und Speichern des Modells

Das für sp\_rxPredict erforderliche Binärformat entspricht dem Format, das für die Verwendung der PREDICT-Funktion erforderlich ist. Fügen Sie daher in Ihrem R-Code einen Aufruf von [rxSerializeModel](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxserializemodel) ein, und stellen Sie sicher, dass `realtimeScoringOnly = TRUE` wie im folgenden Beispiel angegeben ist:

```R
model <- rxSerializeModel(model.name, realtimeScoringOnly = TRUE)
```

### <a name="step-2-call-sp_rxpredict"></a>Schritt 2: Aufrufen von sp_rxPredict

Sie rufen `sp_rxPredict` wie jede andere gespeicherte Prozedur auf. In der aktuellen Version benötigt die gespeicherte Prozedur nur zwei Parameter: _\@model_ für das Modell im Binärformat und _\@inputData_ für die bei der Bewertung zu verwendenden Daten, die als gültige SQL-Abfrage definiert sind.

Da das Binärformat mit dem von der PREDICT-Funktion verwendeten Format identisch ist, können Sie die Modelle und die Datentabelle aus dem vorherigen Beispiel verwenden.

```sql
DECLARE @irismodel varbinary(max)
SELECT @irismodel = [native_model_object] from [ml_models]
WHERE model_name = 'iris.dtree' 
AND model_version = 'v1''

EXEC sp_rxPredict
@model = @irismodel,
@inputData = N'SELECT * FROM iris_rx_data'
```

> [!NOTE]
> Beim Aufruf von `sp_rxPredict` tritt ein Fehler auf, wenn die Eingabedaten für die Bewertung keine Spalten enthalten, die den Anforderungen des Modells entsprechen. Derzeit werden nur die folgenden .NET-Datentypen unterstützt: double, float, short, ushort, long, ulong und string.
>
> Daher müssen Sie möglicherweise nicht unterstützte Typen in ihren Eingabedaten herausfiltern, bevor Sie diese für die Echtzeitbewertung verwenden.
>
> Informationen zu entsprechenden SQL-Typen finden Sie unter [SQL-CLR-Typzuordnung](/dotnet/framework/data/adonet/sql/linq/sql-clr-type-mapping) oder [Zuordnen von CLR-Parameterdaten](https://docs.microsoft.com/sql/relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data).

## <a name="next-steps"></a>Nächste Schritte

+ [Native Bewertung mithilfe der PREDICT-T-SQL-Funktion mit SQL Machine Learning](native-scoring-predict-transact-sql.md)
+ [sp_rxPredict](../../relational-databases/system-stored-procedures/sp-rxpredict-transact-sql.md)
+ [SQL Machine Learning](../index.yml)
