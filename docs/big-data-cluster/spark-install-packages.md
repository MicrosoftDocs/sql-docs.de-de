---
title: Spark-Bibliotheksverwaltung
titleSuffix: SQL Server big data clusters
description: Spark-Bibliotheksverwaltung
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 01/25/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 70611d3f7d4ed6825911908942d9ed707dabbd2e
ms.sourcegitcommit: 8bdb5a51f87a6ff3b94360555973ca0cd0b6223f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2021
ms.locfileid: "100549969"
---
# <a name="spark-library-management"></a>Spark-Bibliotheksverwaltung

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Dieser Artikel enthält einen Leitfaden zum Importieren und Installieren von Paketen für eine Spark-Sitzung über Sitzungs- und Notebook-Konfigurationen.

## <a name="built-in-tools"></a>Integrierte Tools
Spark- und Hadoop-Basispakete Python 3.7 und Python 2.7 Pandas, Sklearn, Numpy und andere Datenverarbeitungspakete
R- und MRO-Pakete Sparklyr

## <a name="install-packages-from-a-maven-repository-onto-the-spark-cluster-at-runtime"></a>Installieren von Paketen aus einem Maven-Repository im Spark-Cluster zur Laufzeit
Maven-Pakete können mithilfe einer Notebook-Zellenkonfiguration zu Beginn einer Spark-Sitzung im Spark-Cluster installiert werden. Führen Sie hierzu vor Beginn einer Spark-Sitzung in Azure Data Studio folgenden Code aus:

```
%%configure -f \
{"conf": {"spark.jars.packages": "com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.1"}}
```

## <a name="install-python-packages-at-pyspark-job-submission-time"></a>Installieren von Python-Paketen beim Übermitteln von PySpark-Aufträgen
1. Geben Sie den Pfad zu einer „requirements.txt“-Datei in HDFS zur Verwendung als Referenz für zu installierende Pakete an.
```
%%configure -f \
{"conf": {
    "spark.pyspark.virtualenv.enabled" : "true",
    "spark.pyspark.virtualenv.type" : "conda",
    "spark.pyspark.virtualenv.requirements" : "requirements.txt",
    "spark.pyspark.virtualenv.bin.path" : "/opt/mls/python/bin/conda"
    }, 
"files": ["hdfs://nmnode-0/tmp/requirements.txt"]
}
```
2. Erstellen Sie eine virtuelle Umgebung wie Conda ohne „requirements.txt“-Datei, und fügen Sie Pakete dynamisch während der Spark-Sitzung hinzu.
```
%%configure -f \
{"conf": {
    'spark.pyspark.virtualenv.enabled' : 'true',
    'spark.pyspark.virtualenv.type' : 'conda',
    'spark.pyspark.virtualenv.bin.path' : '/opt/mls/python/bin/conda',
    'spark.pyspark.virtualenv.python_version': '3.6'
 }
 ```

 ```python
sc.install_packages("numpy==1.11.0")
import numpy as np
```

## <a name="import-jar-from-hdfs-for-use-at-runtime"></a>Importieren einer JAR-Datei aus HDFS zur Verwendung zur Laufzeit
Importieren Sie eine JAR-Datei zur Laufzeit über eine Azure Data Studio-Notebook-Zellenkonfiguration.

```
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

### <a name="import-jar-at-runtime-through-azure-data-studio-notebook-cell-configuration"></a>Importieren einer JAR-Datei zur Laufzeit über eine Azure Data Studio-Notebook-Zellenkonfiguration.
```
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Big Data-Clustern für SQL Server und zugehörige Szenarios finden Sie unter [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md).