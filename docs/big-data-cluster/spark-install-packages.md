---
title: Spark-Bibliotheksverwaltung
titleSuffix: SQL Server big data clusters
description: Spark-Bibliotheksverwaltung
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/25/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bed687cb003bfc7748aefa3c98ae5e19089f9685
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837057"
---
# <a name="spark-library-management"></a>Spark-Bibliotheksverwaltung

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Dieser Artikel enthält einen Leitfaden zum Importieren und Installieren von Paketen für eine Spark-Sitzung über Sitzungs- und Notebook-Konfigurationen.

## <a name="built-in-tools"></a>Integrierte Tools

Basispakete für Scala Spark (Scala 2.11) und Hadoop. 

PySpark (Python 3.7). Pandas, Sklearn, Numpy und andere Pakete für Datenverarbeitung und maschinelles Lernen.

MRO 3.5.2-Pakete. Sparklyr und SparkR für R Spark-Workloads.

## <a name="install-packages-from-a-maven-repository-onto-the-spark-cluster-at-runtime"></a>Installieren von Paketen aus einem Maven-Repository im Spark-Cluster zur Laufzeit

Maven-Pakete können mithilfe einer Notebook-Zellenkonfiguration zu Beginn einer Spark-Sitzung im Spark-Cluster installiert werden. Führen Sie hierzu vor Beginn einer Spark-Sitzung in Azure Data Studio folgenden Code aus:

```python
%%configure -f \
{"conf": {"spark.jars.packages": "com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.1"}}
```

## <a name="install-python-packages-at-pyspark-at-runtime"></a>Installieren von Python-Paketen mit PySpark zur Laufzeit

Die Paketverwaltung auf Sitzungs- und Auftragsebene garantiert Konsistenz und Isolierung der Bibliothek. Die Konfiguration ist eine Spark-Standardbibliothekskonfiguration, die auf Livy-Sitzungen angewendet werden kann. __azdata spark__ unterstützt diese Konfigurationen. Die folgenden Beispiele sind als __Azure Data Studio-Notebooks__ konfigurierte Zellen dargestellt, die nach dem Anfügen an einen Cluster mit dem PySpark-Kernel ausgeführt werden müssen.

Wenn die Konfiguration __"spark.pyspark.virtualenv.enabled" : "true"__ nicht festgelegt ist, verwendet die Sitzung den Python-Standard des Clusters und die installierten Bibliotheken.

### <a name="sessionjob-configuration-with-requirementstxt"></a>Sitzungs-/Auftragskonfiguration mithilfe von „requirements.txt“

Geben Sie den Pfad zur Datei „requirements.txt“ in HDFS als Referenz für zu installierende Pakete an.

```python
%%configure -f \
{
    "conf": {
        "spark.pyspark.virtualenv.enabled" : "true",
        "spark.pyspark.virtualenv.python_version": "3.7",
        "spark.pyspark.virtualenv.requirements" : "hdfs://user/project-A/requirements.txt"
    }
}
```

### <a name="sessionjob-configuration-with-different-python-versions"></a>Sitzungs-/Auftragskonfiguration mit unterschiedlichen Python-Versionen

Erstellen Sie eine virtuelle Umgebung wie Conda ohne „requirements.txt“-Datei, und fügen Sie Pakete dynamisch während der Spark-Sitzung hinzu.

```python
%%configure -f \
{
    "conf": {
        "spark.pyspark.virtualenv.enabled" : "true",
        "spark.pyspark.virtualenv.python_version": "3.6"
    }
}
```

### <a name="library-installation"></a>Bibliotheksinstallation

Führen Sie den Befehl __sc.install_packages__ aus, um Bibliotheken dynamisch in Ihrer Sitzung zu installieren. Die Bibliotheken werden in den Treiber und auf allen Executorknoten installiert.

 ```python
sc.install_packages("numpy==1.11.0")
import numpy as np
```

Es ist auch möglich, mehrere Bibliotheken mit demselben Befehl über ein Array zu installieren.

 ```python
sc.install_packages(["numpy==1.11.0", "xgboost"])
import numpy as np
import xgboost as xgb
```

## <a name="import-jar-from-hdfs-for-use-at-runtime"></a>Importieren einer JAR-Datei aus HDFS zur Verwendung zur Laufzeit
Importieren Sie eine JAR-Datei zur Laufzeit über eine Azure Data Studio-Notebook-Zellenkonfiguration.

```python
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

### <a name="import-jar-at-runtime-through-azure-data-studio-notebook-cell-configuration"></a>Importieren einer JAR-Datei zur Laufzeit über eine Azure Data Studio-Notebook-Zellenkonfiguration.

```python
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Big Data-Clustern für SQL Server und zugehörige Szenarios finden Sie unter [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md).