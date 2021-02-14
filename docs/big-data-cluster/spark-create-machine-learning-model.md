---
title: 'Erstellen und Exportieren von Machine Learning-Modellen von Spark: MLeap'
titleSuffix: SQL Server Big Data Clusters
description: Verwenden Sie PySpark, um Machine Learning-Modelle mit Spark in Big Data-Clustern auf SQL Server zu trainieren und zu erstellen. Exportieren Sie das Modell mit MLeap, und bewerten Sie es dann mit Java in SQL Server.
author: RogPodge
ms.author: roliu
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 4b403644de65f866d4bafb191356fd08212c706f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043600"
---
# <a name="create-export-and-score-spark-machine-learning-models-on-big-data-clusters-2019"></a>Erstellen, Exportieren und Bewerten von Machine Learning-Modellen in Spark auf [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

Im folgenden Beispiel wird veranschaulicht, wie Sie ein Modell mit [Spark ML](https://spark.apache.org/docs/latest/ml-guide.html) erstellen, es nach [MLeap](http://mleap-docs.combust.ml/) exportieren und in SQL Server mit der [Java-Spracherweiterung](../language-extensions/language-extensions-overview.md) bewerten. Dies erfolgt im Kontext eines Big Data-Clusters in SQL Server 2019.

Im folgenden Diagramm werden die in diesem Beispiel durchgeführten Arbeitsschritte veranschaulicht:

![Trainieren des Bewertungsexports mit Spark](./media/spark-create-machine-learning-model/train-score-export-with-spark.png)

## <a name="prerequisites"></a>Voraussetzungen

Alle Dateien für dieses Beispiel befinden sich unter [https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark/sparkml](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark/sparkml).

Zum Ausführen des Beispiels müssen die folgenden Voraussetzungen ebenfalls erfüllt sein:

- Ein [Big Data-Cluster für SQL Server](deploy-get-started.md)

- [Big-Data-Tools](deploy-big-data-tools.md)
   - **kubectl**
   - **curl**
   - **Azure Data Studio**

## <a name="model-training-with-spark-ml"></a>Modelltraining mit Spark ML

In diesem Beispiel werden Erhebungsdaten (**AdultCensusIncome.csv**) zum Erstellen eines Pipelinemodells in Spark ML verwendet.

1. Verwenden Sie die Datei [mleap_sql_test/setup.sh](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/setup.sh), um das Dataset aus dem Internet herunterzuladen und es auf dem HDFS im Big Data-Cluster Ihrer SQL Server-Instanz abzulegen. Dadurch kann von Spark darauf zugegriffen werden.

1. Laden Sie anschließend das Beispielnotebook [train_score_export_ml_models_with_spark.ipynb](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/train_score_export_ml_models_with_spark.ipynb) herunter. Führen Sie an einer PowerShell- oder Bash-Befehlszeile den folgenden Befehl aus, um das Beispielnotebook herunterzuladen:

   ```PowerShell
   curl -o mssql_spark_connector.ipynb "https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/spark/sparkml/train_score_export_ml_models_with_spark.ipynb"
   ```

   Dieses Notebook enthält Zellen mit den erforderlichen Befehlen für diesen Beispielabschnitt.

1. Öffnen Sie das Notebook in Azure Data Studio, und führen Sie jeden Codeblock aus. Weitere Informationen zum Arbeiten mit Notebooks finden Sie unter [Verwenden von Notebooks mit SQL Server](../azure-data-studio/notebooks/notebooks-guidance.md).

Die Daten werden zuerst in Spark eingelesen und in Trainings-und Testdatasets aufgeteilt. Anschließend trainiert der Code ein Pipelinemodell mit den Trainingsdaten. Abschließend wird das Modell in ein MLeap-Bundle exportiert.

> [!TIP]
> Sie können den Python-Code, der diesen Schritten zugeordnet ist, auch außerhalb des Notebooks in der Datei [mleap_sql_test/mleap_pyspark.py](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/mleap_pyspark.py) überprüfen oder ausführen.

## <a name="model-scoring-with-sql-server"></a>Modellbewertung mit SQL Server

Da sich das Spark ML-Pipelinemodell nun in einem allgemeinen [MLeap-Bundle](http://mleap-docs.combust.ml/core-concepts/mleap-bundles.html)-Serialisierungsformat befindet, können Sie das Modell in Java ohne die Verwendung von Spark bewerten.

In diesem Beispiel wird die [Java-Spracherweiterung](../language-extensions/language-extensions-overview.md) in SQL Server verwendet. Um das Modell in SQL Server bewerten zu können, müssen Sie zunächst eine Java-Anwendung erstellen, die das Modell in Java laden und bewerten kann. Den Beispielcode für diese Java-Anwendung finden Sie im [Ordner „mssql-mleap-app“](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mssql-mleap-app).

Nach dem Erstellen des Beispiels können Sie Transact-SQL verwenden, um die Java-Anwendung aufzurufen und das Modell mit einer Datenbanktabelle zu bewerten. Dies wird in der Quelldatei [mleap_sql_test/mleap_sql_tests.py](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/mleap_sql_tests.py) angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Big Data-Cluster finden Sie unter [Vorgehensweise: Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] auf Kubernetes](deployment-guidance.md).