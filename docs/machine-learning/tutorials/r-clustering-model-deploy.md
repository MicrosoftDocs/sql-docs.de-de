---
title: 'Tutorial: Bereitstellen eines Clusteringmodells in R'
titleSuffix: SQL machine learning
description: In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie ein Clustermodell in R mit SQL Machine Learning bereit.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: b1dc019023546a4d1912480bfe452b3067f36bf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339624"
---
# <a name="tutorial-deploy-a-clustering-model-in-r-with-sql-machine-learning"></a>Tutorial: Bereitstellen eines Clusteringmodells in R mit SQL Machine Learning
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie mithilfe von SQL Server Machine Learning Services oder in Big Data-Clustern ein in R entwickeltes Clustermodell in einer Datenbank bereit.
::: moniker-end
::: moniker range="=sql-server-2017"
In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie mithilfe von SQL Server Machine Learning Services ein in R entwickeltes Clustermodell in einer Datenbank bereit.
::: moniker-end
::: moniker range="=sql-server-2016"
In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie mithilfe von SQL Server R Services ein in R entwickeltes Clustermodell in einer Datenbank bereit.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
In Teil 4 dieser vierteiligen Tutorialreihe stellen Sie mithilfe von Machine Learning Services in Azure SQL Managed Instance ein in R entwickeltes Clustermodell in einer Datenbank bereit.
::: moniker-end

Da sich immer neue Kunden registrieren, müssen Sie das R-Skript von jeder App aus aufrufen können, um das Clustering regelmäßig durchführen zu können. Hierzu können Sie das R-Skript in einer Datenbank bereitstellen, indem Sie das R-Skript in einer gespeicherten SQL-Prozedur platzieren. Da das Modell in der Datenbank ausgeführt wird, kann es problemlos mit Daten trainiert werden, die in der Datenbank gespeichert sind.

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer gespeicherten Prozedur zum Generieren des Modells
> * Durchführen des Clustering
> * Verwenden der Clusteringinformationen

In [Teil 1](r-clustering-model-introduction.md) haben Sie die Voraussetzungen installiert und die Beispieldatenbank wiederhergestellt.

In [Teil 2](r-clustering-model-prepare-data.md) haben Sie gelernt, wie Sie die Daten aus einer Datenbank für das Clustering vorbereiten.

In [Teil 3](r-clustering-model-build.md) haben Sie gelernt, wie Sie ein K-Means-Clustermodell in R erstellen und trainieren.

## <a name="prerequisites"></a>Voraussetzungen

* In Teil 4 dieser Tutorialreihe wird vorausgesetzt, dass Sie die Voraussetzungen für [**Teil 1**](r-clustering-model-introduction.md) erfüllt und die Schritte in [**Teil 2**](r-clustering-model-build.md) und [**Teil 3**](r-clustering-model-build.md) durchgeführt haben.

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Erstellen einer gespeicherten Prozedur zum Generieren des Modells

Führen Sie das folgende T-SQL-Skript aus, um die gespeicherte Prozedur zu erstellen. Mit dem Verfahren werden die in Teil 2 und Teil 3 dieser Tutorialreihe entwickelten Schritte neu erstellt:

* Klassifizieren von Kunden basierend auf deren Käufe und Rückgaben
* Generieren von vier Clustern von Kunden mithilfe des K-Means-Algorithmus

Die Prozedur speichert die daraus resultierenden Kundenclusterzuordnungen in der Datenbanktabelle **customer_return_clusters**.

```sql
USE [tpcxbb_1gb]
DROP PROC IF EXISTS generate_customer_return_clusters;
GO
CREATE procedure [dbo].[generate_customer_return_clusters]
AS
/*
  This procedure uses R to classify customers into different groups
  based on their purchase & return history.
*/
BEGIN
    DECLARE @duration FLOAT
    , @instance_name NVARCHAR(100) = @@SERVERNAME
    , @database_name NVARCHAR(128) = db_name()
-- Input query to generate the purchase history & return metrics
    , @input_query NVARCHAR(MAX) = N'
SELECT ss_customer_sk AS customer,
    round(CASE 
            WHEN (
                    (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio,
    round(CASE 
            WHEN (
                    (orders_items = 0)
                    OR (returns_items IS NULL)
                    OR (orders_items IS NULL)
                    OR ((returns_items / orders_items) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio,
    round(CASE 
            WHEN (
                    (orders_money = 0)
                    OR (returns_money IS NULL)
                    OR (orders_money IS NULL)
                    OR ((returns_money / orders_money) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio,
    round(CASE 
            WHEN (returns_count IS NULL)
                THEN 0.0
            ELSE returns_count
            END, 0) AS frequency
FROM (
    SELECT ss_customer_sk,
        -- return order ratio
        COUNT(DISTINCT (ss_ticket_number)) AS orders_count,
        -- return ss_item_sk ratio
        COUNT(ss_item_sk) AS orders_items,
        -- return monetary amount ratio
        SUM(ss_net_paid) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
    ) orders
LEFT OUTER JOIN (
    SELECT sr_customer_sk,
        -- return order ratio
        count(DISTINCT (sr_ticket_number)) AS returns_count,
        -- return ss_item_sk ratio
        COUNT(sr_item_sk) AS returns_items,
        -- return monetary amount ratio
        SUM(sr_return_amt) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
    ) returned ON ss_customer_sk = sr_customer_sk
 '
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
# Define the connection string

connStr <- paste("Driver=SQL Server; Server=", instance_name,
                 "; Database=", database_name,
                 "; uid=Username;pwd=Password; ",
                 sep="" )

# Input customer data that needs to be classified.
# This is the result we get from the query.
library(RODBC)

ch <- odbcDriverConnect(connStr);

customer_data <- sqlQuery(ch, input_query)

sqlDrop(ch, "customer_return_clusters")

## create clustering model
clust <- kmeans(customer_data[,2:5],4)

## create clustering output for table
customer_cluster <- data.frame(cluster=clust$cluster,customer=customer_data$customer,orderRatio=customer_data$orderRatio,
            itemsRatio=customer_data$itemsRatio,monetaryRatio=customer_data$monetaryRatio,frequency=customer_data$frequency)

## write cluster output to DB table
sqlSave(ch, customer_cluster, tablename = "customer_return_clusters")

## clean up
odbcClose(ch)
'
    , @input_data_1 = N''
    , @params = N'@instance_name nvarchar(100), @database_name nvarchar(128), @input_query nvarchar(max), @duration float OUTPUT'
    , @instance_name = @instance_name
    , @database_name = @database_name
    , @input_query = @input_query
    , @duration = @duration OUTPUT;
END;

GO
```

## <a name="perform-clustering"></a>Durchführen des Clustering

Nachdem Sie die gespeicherte Prozedur erstellt haben, führen Sie das folgende Skript aus, um Clustering durchzuführen.

```sql
--Empty table of the results before running the stored procedure
TRUNCATE TABLE customer_return_clusters;

--Execute the clustering
--This will load the table customer_return_clusters with cluster mappings
EXECUTE [dbo].[generate_customer_return_clusters];
```

Stellen Sie sicher, dass die Ausführung erfolgreich war und die Liste der Kunden und deren Clusterzuordnungen vorliegt.

```sql
--Select data from table customer_return_clusters
--to verify that the clustering data was loaded
SELECT TOP (5) *
FROM customer_return_clusters;
```

```result
cluster  customer  orderRatio  itemsRatio  monetaryRatio  frequency
1        29727     0           0           0              0
4        26429     0           0           0.041979       1
2        60053     0           0           0.065762       3
2        97643     0           0           0.037034       3
2        32549     0           0           0.031281       4
```

## <a name="use-the-clustering-information"></a>Verwenden der Clusteringinformationen

Da Sie die Clusteringprozedur in der Datenbank gespeichert haben, können Sie das Clustering effizient für Kundendaten ausführen, die in derselben Datenbank gespeichert sind. Sie können die Prozedur nach jeder Aktualisierung der Kundendaten ausführen und die aktualisierten Clusteringinformationen verwenden.

Angenommen, Sie möchten eine Werbe-E-Mail an Kunden in Cluster 0 senden (inaktive Gruppe; Informationen zu den vier Clustern in [Teil 3](r-clustering-model-build.md#analyze-the-results) dieses Tutorials). Der folgende Code wählt die E-Mail-Adressen der Kunden in Cluster 0 aus.

```sql
USE [tpcxbb_1gb]
--Get email addresses of customers in cluster 0 for a promotion campaign
SELECT customer.[c_email_address], customer.c_customer_sk
  FROM dbo.customer
  JOIN
  [dbo].[customer_clusters] as c
  ON c.Customer = customer.c_customer_sk
  WHERE c.cluster = 0
```

Sie können den Wert **c.cluster** ändern, um E-Mail-Adressen für Kunden in anderen Clustern zurückzugeben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie dieses Tutorial abgeschlossen haben, können Sie die Datenbank „tpcxbb_1gb“ löschen.

## <a name="next-steps"></a>Nächste Schritte

In Teil 4 dieser Tutorialreihe haben Sie Folgendes gelernt:

* Erstellen einer gespeicherten Prozedur zum Generieren des Modells
* Durchführen des Clusterings mit SQL-Machine-Learning
* Verwenden der Clusteringinformationen

Weitere Informationen zur Verwendung von R in Machine Learning Services finden Sie unter:

* [Ausführen einfacher R-Skripts mit SQL Server Machine Learning Services](quickstart-r-create-script.md)
* [R-Datenstrukturen, -typen und -objekte](quickstart-r-data-types-and-objects.md)
* [R-Funktionen](quickstart-r-functions.md)
