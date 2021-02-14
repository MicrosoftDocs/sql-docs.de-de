---
title: Verwenden von sparklyr von RStudio
titleSuffix: SQL Server big data clusters
description: Erfahren Sie, wie Sie sparklyr in einem Big Data-Cluster in SQL Server verwenden, um über die R-Schnittstelle eine Verbindung mit Spark herzustellen.
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 8a15336a22f564dd22870de8843732cbb583d2f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043532"
---
# <a name="use-sparklyr-in-sql-server-big-data-cluster"></a>Verwenden von sparklyr in SQL Server-Big Data-Clustern

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

sparklyr stellt eine R-Schnittstelle für Apache Spark bereit. sparklyr ist eine beliebte Möglichkeit für R-Entwickler, Spark zu verwenden. Dieser Artikel beschreibt die Verwendung von sparklyr in [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] mit RStudio.

## <a name="prerequisites"></a>Voraussetzungen

- [Bereitgestellter SQL Server 2019-Big Data-Cluster](quickstart-big-data-cluster-deploy.md)

### <a name="install-rstudio-desktop"></a>Installieren von RStudio Desktop

Installieren und konfigurieren Sie **RStudio Desktop** mithilfe der folgenden Schritte:

1. Wenn Sie auf einem Windows-Client arbeiten, [laden Sie R 3.4.4 herunter, und führen Sie die Anwendung aus](https://cran.rstudio.com/bin/windows/base/old/3.4.4).

1. [Laden Sie RStudio Desktop herunter, und führen Sie die Anwendung aus](https://www.rstudio.com/products/rstudio/download/).

1. Wenn die Installation abgeschlossen ist, führen Sie die folgenden Befehle in RStudio Desktop aus, um die erforderlichen Pakete zu installieren:

   ```RStudioDesktop
   install.packages("DBI", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   install.packages("dplyr", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   install.packages("sparklyr", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   ```

## <a name="connect-to-spark-in-a-big-data-cluster"></a>Herstellen einer Verbindung mit Spark in einem Big Data-Cluster

Sie können sparklyr verwenden, um über Livy und das HDFS/Spark-Gateway eine Verbindung von einem Client mit dem Big Data-Cluster herzustellen. 

Erstellen Sie in RStudio ein R-Skript, und stellen Sie eine Verbindung mit Spark her, wie im folgenden Beispiel gezeigt:

> [!TIP]
> Verwenden Sie für die Werte `<AZDATA_USERNAME>` und `<AZDATA_PASSWORD>` den Benutzernamen und das Kennwort, das Sie bei der Bereitstellung Ihres Big Data-Clusters festgelegt haben.

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

Informationen zu den Werten für `<IP>` und `<PORT>` finden Sie in der Dokumentation zum [Herstellen einer Verbindung mit einem Big Data-Cluster](connect-to-big-data-cluster.md).

```r
library(sparklyr)
library(dplyr)
library(DBI)

#Specify the Knox username and password
config <- livy_config(user = "<AZDATA_USERNAME>", password = "<AZDATA_PASSWORD>")

httr::set_config(httr::config(ssl_verifypeer = 0L, ssl_verifyhost = 0L))

sc <- spark_connect(master = "https://<IP>:<PORT>/gateway/default/livy/v1",
                    method = "livy",
                    config = config)
```

## <a name="run-sparklyr-queries"></a>Ausführen von sparklyr-Abfragen

Nach dem Herstellen der Verbindung mit Spark können Sie sparklyr ausführen. Das folgende Beispiel führt über sparklyr eine Abfrage des iris-Datasets aus:

```r
iris_tbl <- copy_to(sc, iris)

iris_count <- dbGetQuery(sc, "SELECT COUNT(*) FROM iris")

iris_count
```

## <a name="distributed-r-computations"></a>Verteilte R-Berechnungen

Ein Feature von sparklyr ist die Möglichkeit, [spark_apply](https://spark.rstudio.com/guides/distributed-r/#apply-an-r-function-to-a-spark-object) zum [Verteilen von R-Berechnungen](https://spark.rstudio.com/guides/distributed-r/) zu verwenden.

Da Big Data-Cluster Livy-Verbindungen verwenden, müssen Sie `packages = FALSE` im Aufruf von **spark_apply** festlegen. Weitere Informationen finden Sie im [Abschnitt zu Livy](https://spark.rstudio.com/guides/distributed-r/#livy) in der sparklyr-Dokumentation zu verteilten R-Berechnungen. Mit dieser Einstellung können Sie in dem an **spark_apply** übergebenen R-Code nur die R-Pakete verwenden, die bereits in Ihrem Spark-Cluster installiert sind. Das folgende Beispiel veranschaulicht diese Funktion:

```r
iris_tbl %>% spark_apply(function(e) nrow(e), names = "nrow", group_by = "Species", packages = FALSE)
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Big Data-Clustern finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
