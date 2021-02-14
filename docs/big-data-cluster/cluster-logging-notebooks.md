---
title: Sammeln und Analysieren von Protokollen mit Jupyter-Notebooks und Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Protokollieren eines Clusters mit Jupyter-Notebooks und Azure Data Studio in einem SQL Server 2019 Big Data-Cluster.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a8609fbe12e6218d328528dd5577bf1a40167162
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047780"
---
# <a name="gathering-and-analyzing-logs-in-the-cluster-with-notebooks"></a>Sammeln und Analysieren von Protokollen im Cluster mit Notebooks

Diese Seite ist ein Index der Notebooks für SQL Server-Big Data-Cluster. Dieses ausführbaren Notebooks (IPYNB) sind für SQL Server 2019 entwickelt, um bei der Protokollierung von Big Data-Clustern zu helfen.

Jedes Notebook überprüft seine eigenen Abhängigkeiten. Die Ausführung von **run all cells** (Alle Zellen ausführen) wird entweder erfolgreich abgeschlossen oder löst eine Ausnahme aus, die einen Hinweis mit einem Link zu einem anderen Notebook enthält, um die fehlende Abhängigkeit aufzulösen. Folgen Sie dem Link im Hinweis zu dem folgenden Notebook, klicken Sie auf **run all cells**, und wenn die Aktion erfolgreich ist, kehren Sie zum ursprünglichen Notebook zurück, und führen Sie **run all cells** aus.

Wenn alle Abhängigkeiten installiert wurden, **run all cells** aber fehlschlägt, analysiert jedes Notebook die Ergebnisse und erzeugt, wo möglich, einen Hinweis mit Link zu einem weiteren Notebook, um bei der weitergehenden Behandlung des Problems zu helfen.

## <a name="gathering-logs-from-big-data-cluster-bdc"></a>Sammeln von Protokollen von einem Big Data-Cluster (BDC)

Dieser Abschnitt enthält eine Reihe von Notebooks, mit denen es möglich ist, Protokolle aus einem SQL Server-Big Data-Cluster (BDC) abzurufen.

| Name | Beschreibung |
|--|--|
| TSG001 – azdata-Kopierprotokolle ausführen | Verwenden Sie die Befehlszeilenschnittstelle azdata, um Daten in Big-Data-Clustern zu kopieren. |
| TSG061 – Alle Containerprotokollfragmente für Pods im Namespace des Big-Data-Clusters abrufen | Rufen Sie alle Containerprotokolle für Pods aus einem BD-Cluster im Namespace ab. |
| TSG062 – Alle vorherigen Containerprotokollfragmente für Pods im Namespace des Big-Data-Clusters abrufen | Rufen Sie alle vorherigen Containerprotokolle für Pods aus einem BD-Cluster im Namespace ab. |
| TSG083 – Sicherung der kubectl-Clusterinformationen ausführen | Verwenden Sie die Befehlszeilenschnittstelle kubetl, um BD-Cluster-bezogene Informationen zu sichern. |
| TSG084 – Interner Fehler des Abfrageprozessors | Verwenden Sie eine DMV-Abfrage, um weitere Informationen zum internen Fehler des Abfrageprozessors zu erhalten. |
| TSG091 – Die azdata-CLI-Protokolle abrufen | Rufen Sie die azdata-Protokolle vom lokalen Computer ab. |



## <a name="analyse-logs-from-big-data-clusters-bdc"></a>Analysieren von Protokollen von Big Data-Clustern (BDC)

Notebooks zum Erfassen und Analysieren von Protokollen eines Big-Data-Clusters für SQL Server.  Für den Analyseprozess wird empfohlen, die folgenden Notebooks auszuführen, um bekannte Probleme aus den Protokollen abzurufen.

|Name|Beschreibung |
|---|---|
|TSG030 – errorlog-Dateien in SQL Server|Rufen Sie SQL Server-errorlog-Dateien ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen. |
|TSG031 – PolyBase-Protokolle in SQL Server|Rufen Sie SQL Server-PolyBase-Protokolle ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|
|TSG034 – Livy-Protokolle|Rufen Sie Livy-Protokolle ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|
|TSG035 – Spark-Verlaufsprotokolle|Rufen Sie Spark-Verlaufsprotokolle ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|
|TSG036 – Controllerprotokolle|Rufen Sie die letzten „n“ Stunden aus Controllerprotokollen ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|
|TSG046 – Knox-Gatewayprotokolle|Knox löst einen Fehler (Error 500) für den Client aus und entfernt Details (den Stapel), die auf die Ursache des zugrunde liegenden Problems hindeuten. Verwenden Sie deshalb dieses Notebook, um die Knox-Protokolle aus dem Cluster abzurufen. Rufen Sie Knox-Gatewayprotokolle ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|
|TSG073 – InfluxDB-Protokolle|Rufen Sie InfluxDB-Protokolle ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|
|TSG076 – Elasticsearch-Protokolle|Rufen Sie Elasticsearch-Protokolle ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|
|TSG077 – Kibana-Protokolle|Rufen Sie Kibana-Protokolle ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|
|TSG088 –Hadoop-Datenknotenprotokolle|Rufen Sie Hadoop-Datenknotenprotokolle ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|
|TSG090 – Yarn-nodemanager-Protokolle|Rufen Sie Yarn-nodemanager-Protokolle ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|
|TSG092 – Supervisord-Protokollfragment für alle Container in BDC|Rufen Sie ein Supervisord-Protokollfragment für alle Container in BDC ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|
|TSG093 – Agentenprotokollfragment für alle Container in BDC|Rufen Sie ein Agentenprotokollfragment für alle Container in BDC ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|
|TSG094 – Grafana-Protokolle|Rufen Sie Grafana-Protokolle ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|
|TSG095 – Hadoop-NameNode-Protokolle|Rufen Sie Hadoop-NameNode-Protokolle ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|
|TSG096 –Zookeeper-Protokolle|Rufen Sie Zookeeper-Protokolle ab, analysieren Sie Protokolleinträge, und erhalten Sie Vorschläge zu weiteren relevanten Problembehandlungsanleitungen.|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
