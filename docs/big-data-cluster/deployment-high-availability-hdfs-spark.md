---
title: Bereitstellen von HDFS oder Spark mit Hochverfügbarkeit
titleSuffix: SQL Server Big Data Clusters
description: Erfahren Sie, wie Sie Big Data-Cluster in SQL Server mit Hochverfügbarkeit bereitstellen.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 01/07/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 4d7d8bd2251a646b5c419b80b2067f8a25fa4d7c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046937"
---
# <a name="deploy-hdfs-name-node-and-shared-spark-services-in-a-highly-available-configuration"></a>Bereitstellen des HDFS-Namenknotens und gemeinsamer Spark-Dienste in einer Hochverfügbarkeitskonfiguration

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Zusätzlich zur Bereitstellung der SQL Server-Masterinstanz in einer Hochverfügbarkeitskonfiguration mithilfe von Verfügbarkeitsgruppen können Sie auch andere erfolgskritische Dienste im Big Data-Cluster bereitstellen, um eine höhere Zuverlässigkeit sicherzustellen. Sie können `HDFS name node` und die gemeinsamen Spark-Dienste konfigurieren, die unter `sparkhead` mit einem zusätzlichen Replikat gruppiert sind. In diesem Fall wird `Zookeeper` ebenfalls im Big Data-Cluster bereitgestellt, um als Clusterkoordinator und als Metadatenspeicher für folgende Dienste zu dienen: 

- HDFS-Namenknoten
- Livy und Yarn Resource Manager. 

Beim Spark-Verlauf, dem Auftragsverlauf und dem Hive-Metadatendienst handelt es sich um zustandslose Dienste. Zookeeper ist nicht daran beteiligt, die Dienstintegrität für diese Komponenten sicherzustellen. 

Das Bereitstellen mehrerer Replikate für diese Dienste führt zu einer verbesserten Skalierbarkeit und Zuverlässigkeit sowie einem besseren Lastenausgleich der Workloads zwischen den verfügbaren Replikaten.

> [!NOTE]
> Die folgenden Dienste werden im `sparkhead`-Pod als Container bereitgestellt: 
> - Livy
> - Yarn Resource Manager
> - Spark-Verlauf
> - Auftragsverlauf
> - Hive-Metadatendienst  
>

In der folgenden Abbildung wird eine Spark-Hochverfügbarkeitsbereitstellung in einem Big Data-Cluster von SQL Server dargestellt:

:::image type="content" source="media/deployment-high-availability-hdfs-spark/spark-ha.png" alt-text="spark-ha-bdc":::

In der folgenden Abbildung wird eine HDFS-Hochverfügbarkeitsbereitstellung in einem Big Data-Cluster von SQL Server dargestellt:

:::image type="content" source="media/deployment-high-availability-hdfs-spark/hdfs-ha.png" alt-text="hdfs-ha-bdc":::

## <a name="deploy"></a>Bereitstellen

Wenn entweder der Namenknoten oder der sparkhead-Pod mit zwei Replikaten konfiguriert wurde, müssen Sie auch die Zookeeper-Ressource mit drei Replikaten konfigurieren. In einer Hochverfügbarkeitskonfiguration für den HDFS-Namenknoten werden die zwei Replikate von zwei Pods gehostet. Die Pods sind `nmnode-0` und `nmnode-1`. Es handelt sich um eine Aktiv-/Passiv-Konfiguration. Es ist immer nur ein Namenknoten aktiv. Der andere befindet sich im Standby-Modus und wird infolge eines Failoverereignisses aktiv. 

Sie können entweder die integrierten `aks-dev-test-ha`- oder `kubeadm-prod`-Konfigurationsprofile verwenden, um die Big Data-Clusterbereitstellung anzupassen. Diese Profile enthalten die Einstellungen, die für Ressourcen erforderlich sind, für die Sie zusätzliche Hochverfügbarkeit konfigurieren können. Im Folgenden finden Sie beispielsweise einen Abschnitt in der Konfigurationsdatei `bdc.json`, der für die Bereitstellung des HDFS-Namenknoten, der Zookeeper- und der gemeinsamen Spark-Ressourcen (`sparkhead`) mit Hochverfügbarkeit relevant ist.  

```json
{
  ...
    "nmnode-0": {
        "spec": {
            "replicas": 2
        }
    },
    "sparkhead": {
        "spec": {
            "replicas": 2
        }
    },
    "zookeeper": {
        "spec": {
            "replicas": 3
        }
    },
  ...
}
```

Als bewährte Methode müssen Sie in einer Produktionsbereitstellung auch die HDFS-Blockreplikation auf 3 festlegen. Diese Einstellung ist bereits in den Profilen `aks-dev-test-ha` und `kubeadm-prod` angegeben. Siehe folgenden Abschnitt der Konfigurationsdatei `bdc.json`:

```json
{
  ...
  "hdfs": {
      "resources": [
          "nmnode-0",
          "zookeeper",
          "storage-0",
          "sparkhead"
      ],
      "settings": {
          "hdfs-site.dfs.replication": "3"
      }
  },
  ...
}
```

## <a name="known-limitations"></a>Bekannte Einschränkungen

Zu den bekannten Problemen und Einschränkungen bei der Konfiguration von Hochverfügbarkeit für die Hadoop-Dienste in Big Data-Clustern von SQL Server gehören folgende:

- Alle Konfigurationen müssen zum Zeitpunkt der Bereitstellung von Big Data-Cluster angegeben sein. Mit dem CU1-Release von SQL Server 2019 können Sie die Hochverfügbarkeitskonfiguration nach der Bereitstellung nicht mehr aktivieren.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Verwenden von Konfigurationsdateien in Big Data-Clusterbereitstellungen finden Sie unter [Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md#configfile).
- Weitere Informationen zu Hochverfügbarkeitsoptionen in Big Data-Clustern der SQL Server-Masterinstanz finden Sie im Artikel [Bereitstellen der SQL Server-Masterinstanz mit Hochverfügbarkeit](deployment-high-availability.md).
