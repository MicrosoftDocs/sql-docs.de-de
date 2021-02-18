---
title: 'Tutorial: Vorbereiten von Daten zum Ausführen von Clustering in R'
titleSuffix: SQL machine learning
description: Im zweiten Teil dieser vierteiligen Tutorialreihe bereiten Sie die Daten aus einer Datenbank für das Clustering in R mit SQL Machine Learning vor.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 358f34e281c5c5b1a750135117886247c5e504c0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348065"
---
# <a name="tutorial-prepare-data-to-perform-clustering-in-r-with-sql-machine-learning"></a>Tutorial: Vorbereiten von Daten für das Clustering in R mit SQL Machine Learning
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Im zweiten Teil dieser vierteiligen Tutorialreihe bereiten Sie die Daten aus einer Datenbank für das Clustering in R mit SQL Server Machine Learning Services oder in Big Data-Clustern vor.
::: moniker-end
::: moniker range="=sql-server-2017"
Im zweiten Teil dieser vierteiligen Tutorialreihe bereiten Sie die Daten aus einer Datenbank für das Clustering in R mit SQL Server Machine Learning Services vor.
::: moniker-end
::: moniker range="=sql-server-2016"
Im zweiten Teil dieser vierteiligen Tutorialreihe bereiten Sie die Daten aus einer Datenbank für das Clustering in R mit SQL Server 2016 R Services vor.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Im zweiten Teil dieser vierteiligen Tutorialreihe bereiten Sie die Daten aus einer Datenbank für das Clustering in R mit Machine Learning Services in Azure SQL Managed Instance vor.
::: moniker-end

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Trennen von Kunden anhand verschiedener Dimensionen mithilfe von R
> * Laden der Daten aus der Datenbank in einen R-Datenrahmen

In [Teil 1](r-clustering-model-introduction.md) haben Sie die Voraussetzungen installiert und die Beispieldatenbank wiederhergestellt.

In [Teil 3](r-clustering-model-build.md) erfahren Sie, wie Sie ein K-Means-Clustermodell in R erstellen und trainieren.

In [Teil 4](r-clustering-model-deploy.md) erfahren Sie, wie Sie eine gespeicherte Prozedur in einer Datenbank erstellen, die Clustering auf der Grundlage neuer Daten in R durchführen kann.

## <a name="prerequisites"></a>Voraussetzungen

* Im zweiten Teil dieses Tutorials wird davon ausgegangen, dass Sie [**Teil 1**](r-clustering-model-introduction.md) abgeschlossen haben.

## <a name="separate-customers"></a>Aufteilen von Kunden

Erstellen Sie eine neue RScript-Datei in RStudio, und führen Sie das folgende Skript aus.
In der SQL-Abfrage trennen Sie Kunden anhand der folgenden Dimensionen:

* **orderRatio** = Retourenquote für Bestellungen (Gesamtzahl der teilweise oder vollständig zurückgegebenen Bestellungen im Vergleich zur Gesamtzahl der Bestellungen)
* **itemsRatio** = Retourenquote für Artikel (Gesamtzahl der zurückgegebenen Artikel im Vergleich zur Anzahl der gekauften Artikel)
* **monetaryRatio** = Rückzahlungsquote (Monetärer Gesamtbetrag der zurückgegebenen Artikel im Vergleich zum erworbenen Betrag)
* **frequency** = Häufigkeit der Retouren

Ersetzen Sie in der **connStr**-Funktion ersetzen Sie **ServerName** durch Ihre eigenen Verbindungsinformationen.

```r
# Define the connection string to connect to the tpcxbb_1gb database

connStr <- "Driver=SQL Server;Server=ServerName;Database=tpcxbb_1gb;uid=Username;pwd=Password"

#Define the query to select data
input_query <- "
SELECT ss_customer_sk AS customer
    ,round(CASE 
            WHEN (
                       (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio
    ,round(CASE 
            WHEN (
                     (orders_items = 0)
                  OR (returns_items IS NULL)
                  OR (orders_items IS NULL)
                  OR ((returns_items / orders_items) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio
    ,round(CASE 
            WHEN (
                     (orders_money = 0)
                  OR (returns_money IS NULL)
                  OR (orders_money IS NULL)
                  OR ((returns_money / orders_money) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio
    ,round(CASE 
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
    ) returned ON ss_customer_sk = sr_customer_sk";
```

## <a name="load-the-data-into-a-data-frame"></a>Laden der Daten in einem neuen Datenrahmen

Verwenden Sie nun das folgende Skript, um die Ergebnisse der Abfrage an einen R-Datenrahmen zurückzugeben.

```r
# Query using input_query and get the results back
# to data frame customer_data

library(RODBC)

ch <- odbcDriverConnect(connStr)

customer_data <- sqlQuery(ch, input_query)

# Take a look at the data just loaded
head(customer_data, n = 5);
```

Das Ergebnis sollte etwa folgendermaßen aussehen:

```results
  customer orderRatio itemsRatio monetaryRatio frequency
1    29727          0          0      0.000000         0
2    26429          0          0      0.041979         1
3    60053          0          0      0.065762         3
4    97643          0          0      0.037034         3
5    32549          0          0      0.031281         4
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie nicht mit diesem Tutorial fortfahren möchten, löschen Sie die Datenbank „tpcxbb_1gb“.

## <a name="next-steps"></a>Nächste Schritte

In Teil 2 dieser Tutorialreihe haben Sie Folgendes gelernt:

* Trennen von Kunden anhand verschiedener Dimensionen mithilfe von R
* Laden der Daten aus der Datenbank in einen R-Datenrahmen

Befolgen Sie Teil 3 dieser Tutorialreihe, um ein Machine Learning-Modell zu erstellen, das diese Kundendaten verwendet:

> [!div class="nextstepaction"]
> [Erstellen eines Vorhersagemodells in R mit SQL Machine Learning](r-clustering-model-build.md)
