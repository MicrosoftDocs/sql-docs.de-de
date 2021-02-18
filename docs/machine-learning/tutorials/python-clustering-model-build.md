---
title: 'Python-Tutorial: Erstellen eines Clustermodells'
titleSuffix: SQL machine learning
description: Im dritten Teil dieser vierteiligen Tutorialreihe erstellen Sie ein K-Means-Modell, um das Clustering in Python mit SQL Machine Learning durchzuführen.
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/21/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: a397e23c9e2bc4e297efe7d85a1822a3056daf0a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349075"
---
# <a name="python-tutorial-build-a-model-to-categorize-customers-with-sql-machine-learning"></a>Python-Tutorial: Erstellen eines Modells zum Kategorisieren von Kunden mithilfe von SQL Machine Learning
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Im dritten Teil dieser vierteiligen Tutorialreihe erstellen Sie ein K-Means-Modell in Python, um das Clustering durchzuführen. Im nächsten Teil dieser Reihe stellen Sie dann dieses Modell in einer Datenbank mit SQL Server Machine Learning Services oder in Big Data-Clustern bereit.
::: moniker-end
::: moniker range="=sql-server-2017"
Im dritten Teil dieser vierteiligen Tutorialreihe erstellen Sie ein K-Means-Modell in Python, um das Clustering durchzuführen. Im nächsten Teil dieser Reihe stellen Sie dann dieses Modell in einer Datenbank mit SQL Server Machine Learning Services bereit.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Im dritten Teil dieser vierteiligen Tutorialreihe erstellen Sie ein K-Means-Modell in Python, um das Clustering durchzuführen. Im nächsten Teil dieser Reihe stellen Sie dieses Modell in einer Datenbank mit Machine Learning Services für Azure SQL Managed Instance bereit.
::: moniker-end

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Definieren der Anzahl der Cluster für einen K-Means-Algorithmus
> * Durchführen des Clustering
> * Analysieren der Ergebnisse

In [Teil 1](python-clustering-model.md) haben Sie die Voraussetzungen installiert und die Beispieldatenbank wiederhergestellt.

In [Teil 2](python-clustering-model-prepare-data.md) haben Sie gelernt, wie Sie die Daten aus einer Datenbank für das Clustering vorbereiten.

In [Teil 4](python-clustering-model-deploy.md) erfahren Sie, wie Sie eine gespeicherte Prozedur in einer Datenbank erstellen, die Clustering auf der Grundlage neuer Daten in Python durchführen kann.

## <a name="prerequisites"></a>Voraussetzungen

* In Teil 3 dieses Tutorials wird vorausgesetzt, dass Sie die Voraussetzungen für [**Teil 1**](python-clustering-model.md) erfüllt und die Schritte in [**Teil 2**](python-clustering-model-prepare-data.md) durchgeführt haben.

## <a name="define-the-number-of-clusters"></a>Definieren der Anzahl der Cluster

Für das Clustering Ihrer Kundendaten verwenden Sie den **K-Means**-Clusteringalgorithmus. Dies ist eine der einfachsten und bekanntesten Methoden zum Gruppieren von Daten.
Weitere Informationen zum K-Means-Algorithmus finden Sie im [Umfassenden Leitfaden zu K-Means-Clusteringalgorithmus](https://www.kdnuggets.com/2019/05/guide-k-means-clustering-algorithm.html).

Der Algorithmus akzeptiert zwei Eingaben: Die Daten selbst und eine vordefinierte Zahl „*k*“, die die Anzahl der zu generierenden Cluster darstellt.
Die Ausgabe entspricht *k* Clustern mit den partitionierten Eingabedaten.

Das Ziel des K-Means-Algorithmus ist es, die Elemente in k Cluster zu gruppieren, sodass sich alle Elemente im selben Cluster ähneln und sich möglichst von den Elementen in anderen Clustern unterscheiden.

Verwenden Sie einen Plot aus der Abweichungsquadratsumme der enthaltenen Gruppen nach der Anzahl der extrahierten Cluster, um die Anzahl der zu verwendenden Cluster für den Algorithmus zu bestimmen. Die angemessene Anzahl der zu verwendenden Cluster befindet sich an der Knickstelle bzw. am „Ellenbogen“ des Plots.

```python
################################################################################################
## Determine number of clusters using the Elbow method
################################################################################################

cdata = customer_data
K = range(1, 20)
KM = (sk_cluster.KMeans(n_clusters=k).fit(cdata) for k in K)
centroids = (k.cluster_centers_ for k in KM)

D_k = (sci_distance.cdist(cdata, cent, 'euclidean') for cent in centroids)
dist = (np.min(D, axis=1) for D in D_k)
avgWithinSS = [sum(d) / cdata.shape[0] for d in dist]
plt.plot(K, avgWithinSS, 'b*-')
plt.grid(True)
plt.xlabel('Number of clusters')
plt.ylabel('Average within-cluster sum of squares')
plt.title('Elbow for KMeans clustering')
plt.show()
```

![Ellenbogengraph](./media/python-tutorial-elbow-graph.png)

Aus diesem Graph geht *k = 4* als geeigneter Wert hervor. Mit diesem *k*-Wert werden die Kunden in vier Cluster gruppiert.

## <a name="perform-clustering"></a>Durchführen des Clustering

Im folgenden Python-Skript verwenden Sie die KMeans-Funktion aus dem Paket „sklearn“.

```python
################################################################################################
## Perform clustering using Kmeans
################################################################################################

# It looks like k=4 is a good number to use based on the elbow graph.
n_clusters = 4

means_cluster = sk_cluster.KMeans(n_clusters=n_clusters, random_state=111)
columns = ["orderRatio", "itemsRatio", "monetaryRatio", "frequency"]
est = means_cluster.fit(customer_data[columns])
clusters = est.labels_
customer_data['cluster'] = clusters

# Print some data about the clusters:

# For each cluster, count the members.
for c in range(n_clusters):
    cluster_members=customer_data[customer_data['cluster'] == c][:]
    print('Cluster{}(n={}):'.format(c, len(cluster_members)))
    print('-'* 17)
print(customer_data.groupby(['cluster']).mean())
```

## <a name="analyze-the-results"></a>Analysieren der Ergebnisse

Nachdem Sie nun das Clustering mithilfe des K-Means-Algorithmus durchgeführt haben, besteht der nächste Schritt darin, das Ergebnis zu analysieren und nach handlungsrelevanten Informationen zu suchen.

Sehen Sie sich die Durchschnittswerte und ausgegebenen Clustergrößen des Clusterings aus dem vorherigen Skript an.

```results
Cluster0(n=31675):
-------------------
Cluster1(n=4989):
-------------------
Cluster2(n=1):
-------------------
Cluster3(n=671):
-------------------

         customer  orderRatio  itemsRatio  monetaryRatio  frequency
cluster
0        50854.809882    0.000000    0.000000       0.000000   0.000000
1        51332.535779    0.721604    0.453365       0.307721   1.097815
2        57044.000000    1.000000    2.000000     108.719154   1.000000
3        48516.023845    0.136277    0.078346       0.044497   4.271237
```

Die vier Clusterdurchschnittswerte werden mittels der Variablen angegeben, die Sie in [Teil 1](python-clustering-model-prepare-data.md#separate-customers) des Tutorials definiert haben:

* *orderRatio* = Retourenquote für Bestellungen (Gesamtzahl der teilweise oder vollständig zurückgegebenen Bestellungen im Vergleich zur Gesamtzahl der Bestellungen)
* *itemsRatio* = Retourenquote für Artikel (Gesamtzahl der zurückgegebenen Artikel im Vergleich zur Anzahl der gekauften Artikel)
* *monetaryRatio* = Rückzahlungsquote (Monetärer Gesamtbetrag der zurückgegebenen Artikel im Vergleich zum erworbenen Betrag)
* *frequency* = Häufigkeit der Retouren

Data Mining mit K-Means erfordert häufig weitere Analysen der Ergebnisse sowie weitere Schritte, um die einzelnen Cluster besser zu verstehen, kann jedoch gute Leads erzielen.
Im Folgenden finden Sie Beispiele für die Interpretation dieser Ergebnisse:

* Cluster 0 scheint, eine Gruppe inaktiver Kunden zu sein (alle Werte sind 0 (null)).
* Cluster 3 scheint eine Gruppe mit erhöhtem Retournierverhalten zu sein.

Cluster 0 besteht aus Kunden, die eindeutig inaktiv sind. Vielleicht können Sie Marketingmaßnahmen für diese Gruppe ergreifen, um ihr Interesse an Einkäufen zu wecken. Im nächsten Schritt fragen Sie die E-Mail-Adressen von Kunden in Cluster 0 aus der Datenbank ab, um ihnen Marketing-E-Mails zu senden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie nicht mit diesem Tutorial fortfahren möchten, löschen Sie die Datenbank „tpcxbb_1gb“.

## <a name="next-steps"></a>Nächste Schritte

In Teil 3 dieser Tutorialreihe haben Sie die folgenden Schritte ausgeführt:

* Definieren der Anzahl der Cluster für einen K-Means-Algorithmus
* Durchführen des Clustering
* Analysieren der Ergebnisse

Fahren Sie mit Teil 4 dieser Tutorialreihe fort, um das von Ihnen erstellte Machine Learning-Modell einzusetzen:

> [!div class="nextstepaction"]
> [Python-Tutorial: Bereitstellen eines Clusteringmodells](python-clustering-model-deploy.md)