---
title: Apache Spark und Apache Hadoop
titleSuffix: Configure Apache Spark and Apache Hadoop in Big Data Clusters
description: SQL Server-Big Data-Cluster sind mit Spark- und HDFS-Lösungen kompatibel. Hier erfahren Sie, wie Sie diese konfigurieren können.
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.date: 08/04/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3c018c3882f5a74bb55a3b4cb5fc23881b0daec8
ms.sourcegitcommit: 765262cdc6352a5325148afc22fa4f1499fe1aa3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514857"
---
# <a name="configure-apache-spark-and-apache-hadoop-in-big-data-clusters"></a>Konfigurieren von Apache Spark und Apache Hadoop in Big Data-Clustern

Zum Konfigurieren von Apache Spark und Apache Hadoop in Big Data-Clustern müssen Sie das Clusterprofil zum Zeitpunkt der Bereitstellung ändern.

Ein Big Data-Cluster verfügt über vier Konfigurationskategorien: 

- `sql` 
- `hdfs` 
- `spark` 
- `gateway` 

`sql`, `hdfs`, `spark` und `sql` sind Dienste. Diese Dienste werden der jeweils gleichen benannten Konfigurationskategorie zugeordnet. Alle Gatewaykonfigurationen werden in Kategorie `gateway` eingeordnet. 

Sämtliche Konfigurationen im Dienst `hdfs` zählen beispielsweise zu Kategorie `hdfs`. Beachten Sie, dass alle Konfigurationen für Hadoop (core-site), HDFS und Zookeeper zur Kategorie `hdfs` zählen und alle Konfigurationen für Livy, Spark, Yarn und Hive Metastore zur Kategorie `spark`. 

Unter [Unterstützte Konfigurationen](reference-config-spark-hadoop.md) sind die Eigenschaften von Apache Spark und Apache Hadoop aufgeführt, die Sie beim Bereitstellen eines Big Data-Clusters in SQL Server konfigurieren können.

In den folgenden Abschnitten sind die Eigenschaften aufgeführt, die Sie in einem Cluster **nicht** ändern können:

- [Nicht unterstützte `spark`-Konfigurationen](reference-config-spark-hadoop.md#unsupported-spark-configurations)
- [Nicht unterstützte `hdfs`-Konfigurationen](reference-config-spark-hadoop.md#unsupported-hdfs-configurations)
- [Nicht unterstützte `gateway`-Konfigurationen](reference-config-spark-hadoop.md#unsupported-gateway-configurations)


## <a name="configurations-via-cluster-profile"></a>Konfigurationen über das Clusterprofil

Im Clusterprofil gibt es Ressourcen und Dienste. Zum Zeitpunkt der Bereitstellung können Konfigurationen auf zwei Arten angegeben werden: 

* Auf Ressourcenebene: 

   In den folgenden Beispielen werden die Patchdateien für das Profil aufgeführt: 

   ```json
   { 
         "op": "add", 
         "path": "spec.resources.zookeeper.spec.settings", 
         "value": { 
           "hdfs": { 
             "zoo-cfg.syncLimit": "6" 
           } 
         } 
   }
   ```

   Oder: 

   ```json
   { 
         "op": "add", 
         "path": "spec.resources.gateway.spec.settings", 
         "value": { 
           "gateway": { 
             "gateway-site.gateway.httpclient.socketTimeout": "95s" 
           } 
         } 
   } 
   ```

* Auf Dienstebene. Weisen Sie einem Dienst mehrere Ressourcen zu, und geben Sie die Konfigurationen für den Dienst an.

Im Folgenden finden Sie ein Beispiel für die Patchdatei für das Profil zum Festlegen der HDFS-Blockgröße: 

   ```json
   { 
         "op": "add", 
         "path": "spec.services.hdfs.settings", 
         "value": { 
           "hdfs-site.dfs.block.size": "268435456" 
        } 
   } 
   ```

Der Dienst `hdfs` wird wie folgt definiert:

```json
{ 
  "spec": { 
   "services": { 
     "hdfs": { 
        "resources": [ 
          "nmnode-0", 
          "zookeeper", 
          "storage-0", 
          "sparkhead" 
        ], 
        "settings":{ 
          "hdfs-site.dfs.block.size": "268435456" 
        } 
      } 
    } 
  } 
} 
```
 
> [!NOTE]
> Konfigurationen auf Ressourcenebene überschreiben Konfigurationen auf Dienstebene. Eine Ressource kann mehreren Diensten zugewiesen werden.

## <a name="enable-spark-in-the-storage-pool"></a>Aktivieren von Spark im Speicherpool
Zusätzlich zu den unterstützten Apache-Konfigurationen bieten wir auch die Möglichkeit an, zu konfigurieren, ob Spark-Aufträge im Speicherpool ausgeführt werden können oder nicht. Der boolesche Wert `includeSpark` befindet sich in der `bdc.json`-Konfigurationsdatei unter `spec.resources.storage-0.spec.settings.spark`.

Ein Beispiel für eine Speicherpooldefinition in der Datei „bdc.json“ sieht wie folgt aus:
```json
...
"storage-0": {
                "metadata": {
                    "kind": "Pool",
                    "name": "default"
                },
                "spec": {
                    "type": "Storage",
                    "replicas": 2,
                    "settings": {
                        "spark": {
                            "includeSpark": "true"
                        }
                    }
                }
            }
```


## <a name="limitations"></a>Einschränkungen

Konfigurationen können nur auf Kategorieebene festgelegt werden. Sie können das gemeinsame Präfix nicht im Clusterprofil extrahieren, um mehrere Konfigurationen mit derselben Unterkategorie festzulegen. 

```json
{ 
      "op": "add", 
      "path": "spec.services.hdfs.settings.core-site.hadoop", 
      "value": { 
        “proxyuser.xyz.users”: “*”, 
        “proxyuser.abc.users”: “*” 
     } 
} 
```

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurationseigenschaften von Apache Spark und Apache Hadoop (HDFS)](reference-config-spark-hadoop.md)
- [Referenz zu [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/reference/reference-azdata.md)
- [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
