---
title: 'Python-Tutorial: Vorbereiten von Clusterdaten'
titleSuffix: SQL machine learning
description: Im zweiten Teil dieser vierteiligen Tutorialreihe bereiten Sie SQL Server-Daten für das Clustering in Python mit SQL Machine Learning vor.
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/21/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 7d73f57f771a52de065e0fdc8236e280d5d85ab2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339720"
---
# <a name="python-tutorial-prepare-data-to-categorize-customers-with-sql-machine-learning"></a>Python-Tutorial: Vorbereiten von Daten zum Kategorisieren von Kunden mithilfe von SQL Machine Learning
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Im zweiten Teil dieser vierteiligen Tutorialreihe stellen Sie Daten aus einer Datenbank mithilfe von Python wieder her und bereiten diese vor. Diese Daten verwenden Sie in einem späteren Teil dieser Reihe zum Trainieren und Bereitstellen eines Clustermodells in Python mit SQL Server Machine Learning Services oder in Big Data-Clustern.
::: moniker-end
::: moniker range="=sql-server-2017"
Im zweiten Teil dieser vierteiligen Tutorialreihe stellen Sie Daten aus einer Datenbank mithilfe von Python wieder her und bereiten diese vor. Diese Daten verwenden Sie in einem späteren Teil dieser Reihe zum Trainieren und Bereitstellen eines Clustermodells in Python mit SQL Server Machine Learning Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Im zweiten Teil dieser vierteiligen Tutorialreihe stellen Sie Daten aus einer Datenbank mithilfe von Python wieder her und bereiten diese vor. Diese Daten verwenden Sie in einem späteren Teil dieser Reihe zum Trainieren und Bereitstellen eines Clustermodells in Python mit Machine Learning Services in Azure SQL Managed Instance.
::: moniker-end

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Aufteilen von Kunden über verschiedene Dimensionen mit Python
> * Laden der Daten aus der Datenbank in einen Python-Datenrahmen

In [Teil 1](python-clustering-model.md) haben Sie die Voraussetzungen installiert und die Beispieldatenbank wiederhergestellt.

In [Teil 3](python-clustering-model-build.md) erfahren Sie, wie Sie ein K-Means-Clustermodell in Python erstellen und trainieren.

In [Teil 4](python-clustering-model-deploy.md) erfahren Sie, wie Sie eine gespeicherte Prozedur in einer Datenbank erstellen, die Clustering auf der Grundlage neuer Daten in Python durchführen kann.

## <a name="prerequisites"></a>Voraussetzungen

* Teil 2 dieses Tutorials setzt voraus, dass Sie die Voraussetzungen von [**Teil 1**](python-clustering-model.md) erfüllt haben.

## <a name="separate-customers"></a>Aufteilen von Kunden

Sie werden zunächst Kunden nach den folgenden Dimensionen aufteilen, um sich auf das Clustering von Kunden vorzubereiten:

* **orderRatio** = Retourenquote für Bestellungen (Gesamtzahl der teilweise oder vollständig zurückgegebenen Bestellungen im Vergleich zur Gesamtzahl der Bestellungen)
* **itemsRatio** = Retourenquote für Artikel (Gesamtzahl der zurückgegebenen Artikel im Vergleich zur Anzahl der gekauften Artikel)
* **monetaryRatio** = Rückzahlungsquote (Monetärer Gesamtbetrag der zurückgegebenen Artikel im Vergleich zum erworbenen Betrag)
* **frequency** = Häufigkeit der Retouren

Öffnen Sie ein neues Notebook in Azure Data Studio, und geben Sie das folgende Skript ein.

Ersetzen Sie bei Bedarf die Verbindungsdetails in der Verbindungszeichenfolge.

```python
# Load packages.
import pyodbc
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
from scipy.spatial import distance as sci_distance
from sklearn import cluster as sk_cluster

################################################################################################

## Connect to DB and select data

################################################################################################

# Connection string to connect to SQL Server named instance.
conn_str = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server}; SERVER=<server>; DATABASE=tpcxbb_1gb; UID=<username>; PWD=<password>')

input_query = '''SELECT
ss_customer_sk AS customer,
ROUND(COALESCE(returns_count / NULLIF(1.0*orders_count, 0), 0), 7) AS orderRatio,
ROUND(COALESCE(returns_items / NULLIF(1.0*orders_items, 0), 0), 7) AS itemsRatio,
ROUND(COALESCE(returns_money / NULLIF(1.0*orders_money, 0), 0), 7) AS monetaryRatio,
COALESCE(returns_count, 0) AS frequency
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
GROUP BY sr_customer_sk ) returned ON ss_customer_sk=sr_customer_sk'''


# Define the columns we wish to import.
column_info = {
    "customer": {"type": "integer"},
    "orderRatio": {"type": "integer"},
    "itemsRatio": {"type": "integer"},
    "frequency": {"type": "integer"}
}
```

## <a name="load-the-data-into-a-data-frame"></a>Laden der Daten in einem neuen Datenrahmen

Die Ergebnisse der Abfrage werden mit der Pandas-Funktion **read_sql** an Python zurückgegeben. Als Teil des Prozesses verwenden Sie die Spalteninformationen, die Sie im vorherigen Skript definiert haben.

```python
customer_data = pandas.read_sql(input_query, conn_str)
```

Zeigen Sie nun den Anfang des Datenrahmens an, um sicherzustellen, dass er korrekt ist.

```python
print("Data frame:", customer_data.head(n=5))
```

```results
Rows Read: 37336, Total Rows Processed: 37336, Total Chunk Time: 0.172 seconds
Data frame:     customer  orderRatio  itemsRatio  monetaryRatio  frequency
0    29727.0    0.000000    0.000000       0.000000          0
1    97643.0    0.068182    0.078176       0.037034          3
2    57247.0    0.000000    0.000000       0.000000          0
3    32549.0    0.086957    0.068657       0.031281          4
4     2040.0    0.000000    0.000000       0.000000          0
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie nicht mit diesem Tutorial fortfahren möchten, löschen Sie die Datenbank „tpcxbb_1gb“.

## <a name="next-steps"></a>Nächste Schritte

In Teil 2 dieser Tutorialreihe haben Sie die folgenden Schritte ausgeführt:

* Aufteilen von Kunden über verschiedene Dimensionen mit Python
* Laden der Daten aus der Datenbank in einen Python-Datenrahmen

Befolgen Sie Teil 3 dieser Tutorialreihe, um ein Machine Learning-Modell zu erstellen, das diese Kundendaten verwendet:

> [!div class="nextstepaction"]
> [Python-Tutorial: Erstellen eines Vorhersagemodells](python-clustering-model-build.md)
