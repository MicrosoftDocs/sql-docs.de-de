---
title: Überwachen eines Clusters mit Jupyter-Notebooks und Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Überwachen eines Clusters mit Jupyter-Notebooks und Azure Data Studio in einem SQL Server 2019 Big Data-Cluster.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bc370b42f247c4b8e57c5628feaae7385ba1502d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039373"
---
# <a name="monitoring-cluster-with-notebooks"></a>Überwachen eines Clusters mit Notebooks

Diese Seite ist ein Index der Notebooks für SQL Server-Big Data-Cluster. Dieses ausführbaren Notebooks (.ipynb) sind für SQL Server 2019 entwickelt, um bei der Überwachung von Big Data-Clustern zu helfen.

Jedes Notebook überprüft seine eigenen Abhängigkeiten. Die Ausführung von **run all cells** (Alle Zellen ausführen) wird entweder erfolgreich abgeschlossen oder löst eine Ausnahme aus, die einen Hinweis mit einem Link zu einem anderen Notebook enthält, um die fehlende Abhängigkeit aufzulösen. Folgen Sie dem Link im Hinweis zu dem folgenden Notebook, klicken Sie auf **run all cells**, und wenn die Aktion erfolgreich ist, kehren Sie zum ursprünglichen Notebook zurück, und führen Sie **run all cells** aus.

Wenn alle Abhängigkeiten installiert wurden, **run all cells** aber fehlschlägt, analysiert jedes Notebook die Ergebnisse und erzeugt, wo möglich, einen Hinweis mit Link zu einem weiteren Notebook, um bei der weitergehenden Behandlung des Problems zu helfen.


## <a name="monitoring-kubernetes"></a>Überwachen von Kubernetes

Dieser Abschnitt enthält mehrere Notebooks, mit denen Informationen und Statusangaben zu einem Big-Data-Cluster für SQL Server mithilfe der `azdata`-Befehlszeilenschnittstelle (CLI) abgerufen werden können.

|Name |Beschreibung |
|---|---|---|---|
|TSG006: Systempodstatus abrufen|Zeigen Sie den Status aller Systempods an. |
|TSG007: BDC-Podstatus abrufen|Zeigen Sie den Status von Pods für Big-Data-Cluster an.|
|TSG008: Versionsinformationen abrufen (Kubernetes)|Rufen Sie die Kubernetes-Clusterinformationen ab.|
|TSG009: Knoten abrufen (Kubernetes)|Rufen Sie die Kubernetes-Kontexte ab. |
|TSG010: Konfigurationskontexte abrufen|Verwenden Sie eine DMV-Abfrage, um weitere Informationen zum internen Fehler des Abfrageprozessors zu erhalten.|
|TSG015: BDC-Dienste anzeigen (Kubernetes)|Rufen Sie den Dienststatus eines BD-Clusters ab, der im Kubernetes-Cluster bereitgestellt wird. |
|TSG016: BDC-Pods beschreiben|Rufen Sie den Pods-Status eines BD-Clusters ab, der im Kubernetes-Cluster bereitgestellt wird. |
|TSG020: Knoten beschreiben (Kubernetes)|Rufen Sie die Knoteninformationen eines BD-Clusters über die kubectl-Befehlszeilenschnittstelle ab. |
|TSG021: Clusterinformationen abrufen (Kubernetes)|Rufen Sie die Kubernetes-Clusterinformationen ab. |
|TSG022 – Externe IP-Adresse für kubeadm-Host abrufen|Rufen Sie die externe IP-Adresse des Hosts von kubeadm ab. |
|TSG023: Alle BDC-Objekte abrufen (Kubernetes)|Rufen Sie eine Zusammenfassung aller Kubernetes-Ressourcen für den Systemnamespace und den Big-Data-Clusternamespace ab. |
|TSG042: Knotenname und externe Einbindungen (Mounts) für Daten- und Protokolle-PVCs abrufen|Rufen Sie den Knotennamen des hostenden Pods zusammen mit den externen Einbindungen für Daten und Protokolle ab. |
|TSG063: Speicherklassen abrufen (Kubernetes)|Verwenden Sie dieses Notebook, um Kubernetes-Speicherklassen abzurufen. |
|TSG064: BDC-Ansprüche auf persistente Volumes abrufen|Zeigen Sie die Ansprüche auf persistente Volumes (Persistent Volume Claims, PVCs) für den Big-Data-Cluster an. |
|TSG065: BDC-Geheimnisse abrufen (Kubernetes)|Zeigen Sie die Big Data-Clustergeheimnisse an. |
|TSG066: BDC-Ereignisse abrufen (Kubernetes)|Zeigen Sie die Big Data-Clusterereignisse an.|
|TSG072: Persistente Volumes abrufen (Kubernetes)|Zeigt das persistente Volume (PV) für den Kubernetes-Cluster an. Persistente Volumes sind Objekte ohne Namespaces. |
|TSG081: Namespaces abrufen (Kubernetes)|Rufen Sie die Kubernetes-Namespaces ab. |
|TSG089: Nicht aktive BDC-Pods beschreiben|Zeigen Sie nicht aktive BDC-Pods für den Kubernetes-Cluster an. |
|TSG097: BDC-StatefulSets abrufen (Kubernetes)|Zeigen Sie BDC-StatefulSets für den Kubernetes-Cluster an. |
|TSG098: BDC-ReplicaSets abrufen (Kubernetes)|Zeigen Sie BDC-ReplicaSets für den Kubernetes-Cluster an. |
|TSG099: BDC-DaemonSets abrufen (Kubernetes)|Zeigen Sie BDC-DaemonSets für den Kubernetes-Cluster an. |


## <a name="monitor-big-data-cluster-bdc"></a>Überwachen eines Big Data-Clusters (BDC)

Diese Abschnitt enthält mehrere Notebooks, mit denen Informationen und Statusangaben zu einem Kubernetes-Cluster abgerufen werden können, der einen Big-Data-Cluster (BDC) für SQL Server hostet.

|Name |Beschreibung |
|---|---|---|---|
|TSG003 – BDC-Spark-Sitzungen anzeigen|Zeigen Sie BDC-Spark-Sitzungen an. |
|TSG004: BDC-Apps anzeigen|Zeigen Sie die Apps an, die in einem BD-Cluster ausgeführt werden.|
|TSG012: BDC-Status anzeigen|Rufen Sie den aktuellen Status von verschiedenen Komponenten eines BD-Clusters ab.|
|TSG013: Dateiliste im Speicherpool anzeigen (HDFS)|Rufen Sie die Dateiliste im Speicherpool ab (HDFS). |
|TSG014 – BDC-Endpunkte anzeigen|Rufen Sie alle verfügbaren Endpunkte für einen BD-Cluster ab.|
|TSG017: BDC-Konfiguration anzeigen|Rufen Sie die BDC-Konfiguration ab. |
|TSG033 – BDC-SQL-Status anzeigen|Rufen Sie den SQL Server-Status eines BD-Clusters ab, der im Kubernetes-Cluster bereitgestellt wird. |
|TSG049: Status des BDC-Controllers anzeigen|Rufen Sie den Controllerstatus eines BD-Clusters ab, der im Kubernetes-Cluster bereitgestellt wird. |
|TSG068: BDC-HDFS-Status anzeigen|Rufen Sie den HDFS-Status eines BD-Clusters ab, der im Kubernetes-Cluster bereitgestellt wird. |
|TSG069: Gatewaystatus des Big Data-Clusters anzeigen|Rufen Sie den BDC-Gatewaystatus eines BD-Clusters ab, der im Kubernetes-Cluster bereitgestellt wird. |
|TSG070 – SQL-Masterpool abfragen| Führen Sie eine SQL-Abfrage für eine Masterinstanz aus. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
