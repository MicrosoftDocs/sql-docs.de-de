---
title: 'Python-Tutorial: Bereitstellen eines Clustermodells'
titleSuffix: SQL machine learning
description: In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie ein Clustermodell in Python mit SQL Machine Learning bereit.
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/21/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 32b009f8155000dfc44f714cfa6f67fc8333813e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349053"
---
# <a name="python-tutorial-deploy-a-model-to-categorize-customers-with-sql-machine-learning"></a>Python-Tutorial: Bereitstellen eines Modells zum Kategorisieren von Kunden mithilfe von SQL Machine Learning
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie mithilfe von SQL Server Machine Learning Services oder in Big Data-Cluster ein in Python entwickeltes Clustermodell in einer Datenbank bereit.
::: moniker-end
::: moniker range="=sql-server-2017"
In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie mithilfe von SQL Server Machine Learning Services ein in Python entwickeltes Clustermodell in einer Datenbank bereit.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie mithilfe von Machine Learning Services in Azure SQL Managed Instance ein in Python entwickeltes Clustermodell in einer Datenbank bereit.
::: moniker-end

Da sich immer neue Kunden registrieren, müssen Sie das Python-Skript von jeder App aus aufrufen können, um das Clustering regelmäßig durchführen zu können. Hierzu können Sie das Python-Skript in einer Datenbank bereitstellen, indem Sie das Python-Skript in einer gespeicherten SQL-Prozedur platzieren. Da das Modell in der Datenbank ausgeführt wird, kann es problemlos mit Daten trainiert werden, die in der Datenbank gespeichert sind.

In diesem Abschnitt verschieben Sie den soeben geschriebenen Python-Code auf den Server und stellen Clustering bereit.

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer gespeicherten Prozedur zum Generieren des Modells
> * Durchführen des Clustering auf dem Server
> * Verwenden der Clusteringinformationen

In [Teil 1](python-clustering-model.md) haben Sie die Voraussetzungen installiert und die Beispieldatenbank wiederhergestellt.

In [Teil 2](python-clustering-model-prepare-data.md) haben Sie gelernt, wie Sie die Daten aus einer Datenbank für das Clustering vorbereiten.

In [Teil 3](python-clustering-model-build.md) haben Sie gelernt, wie Sie ein K-Means-Clustermodell in Python erstellen und trainieren.

## <a name="prerequisites"></a>Voraussetzungen

* In Teil 4 dieser Tutorialreihe wird vorausgesetzt, dass Sie die Voraussetzungen für [**Teil 1**](python-clustering-model.md) erfüllt und die Schritte in [**Teil 2**](python-clustering-model-prepare-data.md) und [**Teil 3**](python-clustering-model-build.md) durchgeführt haben.

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Erstellen einer gespeicherten Prozedur zum Generieren des Modells

Führen Sie das folgende T-SQL-Skript aus, um die gespeicherte Prozedur zu erstellen. Mit dem Verfahren werden die in Teil 1 und Teil 2 dieser Tutorialreihe entwickelten Schritte neu erstellt:

* Klassifizieren von Kunden basierend auf deren Käufe und Rückgaben
* Generieren von vier Clustern von Kunden mithilfe des K-Means-Algorithmus

```sql
USE [tpcxbb_1gb]
GO

CREATE procedure [dbo].[py_generate_customer_return_clusters]
AS

BEGIN
    DECLARE

-- Input query to generate the purchase history & return metrics
     @input_query NVARCHAR(MAX) = N'
SELECT
  ss_customer_sk AS customer,
  CAST( (ROUND(COALESCE(returns_count / NULLIF(1.0*orders_count, 0), 0), 7) ) AS FLOAT) AS orderRatio,
  CAST( (ROUND(COALESCE(returns_items / NULLIF(1.0*orders_items, 0), 0), 7) ) AS FLOAT) AS itemsRatio,
  CAST( (ROUND(COALESCE(returns_money / NULLIF(1.0*orders_money, 0), 0), 7) ) AS FLOAT) AS monetaryRatio,
  CAST( (COALESCE(returns_count, 0)) AS FLOAT) AS frequency
FROM
  (
    SELECT
      ss_customer_sk,
      -- return order ratio
      COUNT(distinct(ss_ticket_number)) AS orders_count,
      -- return ss_item_sk ratio
      COUNT(ss_item_sk) AS orders_items,
      -- return monetary amount ratio
      SUM( ss_net_paid ) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
  ) orders
  LEFT OUTER JOIN
  (
    SELECT
      sr_customer_sk,
      -- return order ratio
      count(distinct(sr_ticket_number)) as returns_count,
      -- return ss_item_sk ratio
      COUNT(sr_item_sk) as returns_items,
      -- return monetary amount ratio
      SUM( sr_return_amt ) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
  ) returned ON ss_customer_sk=sr_customer_sk
 '

EXEC sp_execute_external_script
      @language = N'Python'
    , @script = N'

import pandas as pd
from sklearn.cluster import KMeans

#get data from input query
customer_data = my_input_data

#We concluded in step 2 in the tutorial that 4 would be a good number of clusters
n_clusters = 4

#Perform clustering
est = KMeans(n_clusters=n_clusters, random_state=111).fit(customer_data[["orderRatio","itemsRatio","monetaryRatio","frequency"]])
clusters = est.labels_
customer_data["cluster"] = clusters

OutputDataSet = customer_data
'
    , @input_data_1 = @input_query
    , @input_data_1_name = N'my_input_data'
             with result sets (("Customer" int, "orderRatio" float,"itemsRatio" float,"monetaryRatio" float,"frequency" float,"cluster" float));
END;
GO
```

## <a name="perform-clustering"></a>Durchführen des Clustering

Da Sie nun die gespeicherte Prozedur erstellt haben, führen Sie das folgende Skript aus, um mithilfe der Prozedur das Clustering auszuführen.

```sql
--Create a table to store the predictions in

DROP TABLE IF EXISTS [dbo].[py_customer_clusters];
GO

CREATE TABLE [dbo].[py_customer_clusters] (
    [Customer] [bigint] NULL
  , [OrderRatio] [float] NULL
  , [itemsRatio] [float] NULL
  , [monetaryRatio] [float] NULL
  , [frequency] [float] NULL
  , [cluster] [int] NULL
  ,
    ) ON [PRIMARY]
GO

--Execute the clustering and insert results into table
INSERT INTO py_customer_clusters
EXEC [dbo].[py_generate_customer_return_clusters];

-- Select contents of the table to verify it works
SELECT * FROM py_customer_clusters;
```

## <a name="use-the-clustering-information"></a>Verwenden der Clusteringinformationen

Da Sie die Clusteringprozedur in der Datenbank gespeichert haben, können Sie das Clustering effizient für Kundendaten ausführen, die in derselben Datenbank gespeichert sind. Sie können die Prozedur nach jeder Aktualisierung der Kundendaten ausführen und die aktualisierten Clusteringinformationen verwenden.

Angenommen, Sie möchten eine Werbe-E-Mail an Kunden in Cluster 0 senden (inaktive Gruppe; Informationen zu den vier Clustern in [Teil 3](python-clustering-model-build.md#analyze-the-results) dieses Tutorials). Der folgende Code wählt die E-Mail-Adressen der Kunden in Cluster 0 aus.

```sql
USE [tpcxbb_1gb]
--Get email addresses of customers in cluster 0 for a promotion campaign
SELECT customer.[c_email_address], customer.c_customer_sk
  FROM dbo.customer
  JOIN
  [dbo].[py_customer_clusters] as c
  ON c.Customer = customer.c_customer_sk
  WHERE c.cluster = 0
```

Sie können den Wert **c.cluster** ändern, um E-Mail-Adressen für Kunden in anderen Clustern zurückzugeben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie dieses Tutorial abgeschlossen haben, können Sie die Datenbank „tpcxbb_1gb“ löschen.

## <a name="next-steps"></a>Nächste Schritte

In Teil 4 dieser Tutorialreihe haben Sie die folgenden Schritte ausgeführt:

* Erstellen einer gespeicherten Prozedur zum Generieren des Modells
* Durchführen des Clustering auf dem Server
* Verwenden der Clusteringinformationen

Weitere Informationen zur Verwendung von Python in SQL Machine Learning finden Sie unter:

* [Schnellstart: Erstellen und Ausführen einfacher Python-Skripts](quickstart-python-create-script.md)
* [Weitere Python-Turorials für SQL Machine Learning](python-tutorials.md)
* [Installieren von Python-Paketen mit sqlmlutils](../package-management/install-additional-python-packages-on-sql-server.md)