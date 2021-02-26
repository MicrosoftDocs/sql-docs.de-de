---
title: Konfigurieren von Bereitstellungen
titleSuffix: SQL Server big data clusters
description: Erfahren Sie, wie Sie eine Big Data-Clusterbereitstellung mit Konfigurationsdateien anpassen, die in das Verwaltungstool azdata integriert sind.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rajmera3
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ffb41a559c6f262fe6d25a50c51b6e6553a2a7b8
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343919"
---
# <a name="configure-deployment-settings-for-cluster-resources-and-services"></a>Konfigurieren von Bereitstellungseinstellungen für Clusterressourcen und -dienste

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
> [!Note]
> In Big Data-Clustern ab Version CU9 werden Funktionen für die Konfigurationsverwaltung unterstützt. Dieses Feature ermöglicht Konfigurationen nach der Bereitstellung und bietet mehr Transparenz und eine bessere Konfigurierbarkeit des Clusters. Versionen bis CU8 bieten diese Funktion nicht, und Konfigurationen können nur zum Zeitpunkt der Bereitstellung vorgenommen werden.

Ausgehend von einer Gruppe von vordefinierten und im Verwaltungstool [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] integrierten Konfigurationsprofilen können Sie die Standardeinstellungen ganz einfach an die Anforderungen Ihrer BDC-Workload anpassen. Die Struktur der Konfigurationsdateien ermöglicht es Ihnen, die Einstellungen für die jeweiligen Dienstressourcen einzeln zu aktualisieren.

In diesem 13-minütigen Video erhalten Sie einen Überblick über die Big Data-Clusterkonfiguration:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Big-Data-Cluster-Configuration/player?WT.mc_id=dataexposed-c9-niner]

> [!TIP]
> Weitere Informationen zum Bereitstellen von hoch verfügbaren Diensten finden Sie in den Artikeln zum Konfigurieren der **Hochverfügbarkeit** für unternehmenskritische Komponenten wie [SQL Server Master](deployment-high-availability.md) oder [HDFS-Namensknoten](deployment-high-availability-hdfs-spark.md).

> [!TIP]
> Informationen zu den konfigurierbaren Einstellungen finden Sie im Artikel [Eigenschaften für die Konfiguration von SQL Server Big Data-Clustern](reference-config-bdc-overview.md). Informationen zu den für die SQL Server-Masterinstanz verfügbaren Konfigurationen für Versionen bis CU8 finden Sie unter [Konfigurationseigenschaften der SQL Server-Masterinstanz (vor CU9)](reference-config-master-instance.md). Informationen zu Apache Spark- und Hadoop-Eigenschaften finden Sie unter [Konfigurationseigenschaften von Apache Spark und Apache Hadoop (HDFS)](reference-config-spark-hadoop.md).

Sie können auch Konfigurationen auf Ressourcenebene festlegen oder die Konfigurationen für alle Dienste in einer Ressource aktualisieren. Hier eine Zusammenfassung der Struktur für `bdc.json`:

```json
{
    "apiVersion": "v1",
    "metadata": {
        "kind": "BigDataCluster",
        "name": "mssql-cluster"
    },
    "spec": {
        "resources": {
            "nmnode-0": {...
            },
            "sparkhead": {...
            },
            "zookeeper": {...
            },
            "gateway": {...
            },
            "appproxy": {...
            },
            "master": {...
            },
            "compute-0": {...
            },
            "data-0": {...
            },
            "storage-0": {...
        },
        "services": {
            "sql": {
                "resources": [
                    "master",
                    "compute-0",
                    "data-0",
                    "storage-0"
                ]
            },
            "hdfs": {
                "resources": [
                    "nmnode-0",
                    "zookeeper",
                    "storage-0",
                    "sparkhead"
                ],
                "settings": {...
            },
            "spark": {
                "resources": [
                    "sparkhead",
                    "storage-0"
                ],
                "settings": {...
            }
        }
    }
}
```

Zum Aktualisieren von Konfigurationen auf Ressourcenebene, wie z. B. Instanzen in einem Pool, aktualisieren Sie die Ressourcenspezifikation. Um beispielsweise die Anzahl von Instanzen im Computepool zu aktualisieren, ändern Sie den folgenden Abschnitt in der Konfigurationsdatei `bdc.json`:

```json
"resources": {
    ...
    "compute-0": {
        "metadata": {
            "kind": "Pool",
            "name": "default"
        },
        "spec": {
            "type": "Compute",
            "replicas": 4
        }
    }
    ...
}
``` 

Gleiches gilt für das Ändern der Einstellungen eines einzelnen Dienstes innerhalb einer bestimmten Ressource. Wenn Sie beispielsweise die Spark-Speichereinstellungen nur für die Spark-Komponente im Speicherpool ändern möchten, aktualisieren Sie die Ressource `storage-0` mit einem `settings`-Abschnitt für den `spark`-Dienst in der Konfigurationsdatei `bdc.json`.

```json
"resources":{
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
                    "spark-defaults-conf.spark.driver.memory": "2g",
                    "spark-defaults-conf.spark.driver.cores": "1",
                    "spark-defaults-conf.spark.executor.instances": "3",
                    "spark-defaults-conf.spark.executor.memory": "1536m",
                    "spark-defaults-conf.spark.executor.cores": "1",
                    "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
                    "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
                    "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
                    "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
                    "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
                }
            }
        }
    }
    ...
}
```

Wenn Sie dieselben Konfigurationen für einen Dienst anwenden möchten, dem mehreren Ressourcen zugeordnet sind, aktualisieren Sie die entsprechenden `settings` im Abschnitt `services`. Wenn Sie beispielsweise sowohl für den Speicherpool als auch für Spark-Pools die gleichen Einstellungen für Spark festlegen möchten, aktualisieren Sie den Abschnitt `settings` im Dienstabschnitt `spark` in der Konfigurationsdatei `bdc.json`.

```json
"services": {
    ...
    "spark": {
        "resources": [
            "sparkhead",
            "storage-0"
        ],
        "settings": {
            "spark-defaults-conf.spark.driver.memory": "2g",
            "spark-defaults-conf.spark.driver.cores": "1",
            "spark-defaults-conf.spark.executor.instances": "3",
            "spark-defaults-conf.spark.executor.memory": "1536m",
            "spark-defaults-conf.spark.executor.cores": "1",
            "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
            "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
            "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
            "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
            "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
        }
    }
    ...
}
```

Sie können einen beliebigen JSON-Format-Editor verwenden (z. B. VS Code), um die Konfigurationsdateien für Ihre Clusterbereitstellung anzupassen. Verwenden Sie den Befehl `azdata bdc config`, um für diese Bearbeitungen Skripts zu erstellen und sie zu automatisieren. In diesem Artikel wird erläutert, wie Sie Big-Data-Clusterbereitstellungen konfigurieren, indem Sie Bereitstellungskonfigurationsdateien ändern. Er enthält Beispiele zum Ändern der Konfiguration für verschiedene Szenarios. Weitere Informationen zur Verwendung von Konfigurationsdateien in Bereitstellungen finden Sie im [Leitfaden zur Bereitstellung](deployment-guidance.md#configfile).

## <a name="prerequisites"></a>Voraussetzungen

- [Installieren Sie azdata](../azdata/install/deploy-install-azdata.md).

- Bei jedem der Beispiele in diesem Abschnitt wird davon ausgegangen, dass Sie eine Kopie einer der Standardkonfigurationen erstellt haben. Weitere Informationen finden Sie unter [Create a custom configuration (Erstellen einer benutzerdefinierten Konfiguration)](deployment-guidance.md#customconfig). Mit dem folgenden Befehl wird z. B. ein Verzeichnis namens `custom-bdc` erstellt, das die beiden JSON-Bereitstellungskonfigurationsdateien `bdc.json` und `control.json` enthält, basierend auf der Standardkonfiguration für `aks-dev-test`:

   ```bash
   azdata bdc config init --source aks-dev-test --target custom-bdc
   ```

## <a name="change-default-docker-registry-repository-and-images-tag"></a><a id="docker"></a> Ändern von Standardregistrierung, -repository und -imagetag in Docker

Die integrierten Konfigurationsdateien, insbesondere „control.json“, enthalten einen `docker`-Abschnitt, in dem Containerregistrierung, -repository und -imagestag bereits eingetragen sind. Standardmäßig befinden sich die für Big Data-Cluster erforderlichen Images in der Microsoft Container Registry (`mcr.microsoft.com`) im `mssql/bdc`-Repository:

```json
{
    "apiVersion": "v1",
    "metadata": {
        "kind": "Cluster",
        "name": "mssql-cluster"
    },
    "spec": {
        "docker": {
            "registry": "mcr.microsoft.com",
            "repository": "mssql/bdc",
            "imageTag": "2019-GDR1-ubuntu-16.04",
            "imagePullPolicy": "Always"
        },
        ...
    }
}
```

Vor der Bereitstellung können Sie die `docker`-Einstellungen anpassen, indem Sie entweder die Konfigurationsdatei `control.json` direkt bearbeiten oder `azdata bdc config`-Befehle verwenden. Mit den folgenden Befehlen wird beispielsweise eine `custom-bdc`-control.json-Konfigurationsdatei mit einem anderen `<registry>`, `<repository>` und `<image_tag>` aktualisiert:

```bash
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.registry=<registry>"
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.repository=<repository>"
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.imageTag=<image_tag>"
```

> [!TIP]
> Es hat sich bewährt, anstelle des `latest`-Imagetags ein versionsspezifisches Imagetag zu verwenden, da ansonsten Versionskonflikte auftreten können, die Probleme hinsichtlich der Clusterintegrität zur Folge haben.

> [!TIP]
> Die Bereitstellung von Big Data-Clustern muss Zugriff auf die Containerregistrierung und das Containerrepository haben, aus dem Containerimages gepullt werden. Wenn Ihre Umgebung keinen Zugriff auf die standardmäßige Microsoft Container Registry hat, können Sie eine Offlineinstallation durchführen, bei der die erforderlichen Images zuerst in einem privaten Docker-Repository abgelegt werden. Weitere Informationen zu Offlineinstallationen finden Sie unter [Durchführen einer Offlinebereitstellung von Big Data-Clustern für SQL Server](deploy-offline.md). Sie müssen die [Umgebungsvariablen](deployment-guidance.md#env) `DOCKER_USERNAME` und `DOCKER_PASSWORD` festlegen, bevor Sie die Bereitstellung veröffentlichen, damit der Bereitstellungsworkflow Zugriff auf das private Repository hat, aus dem Images gepullt werden.

## <a name="change-cluster-name"></a><a id="clustername"></a> Ändern von Clusternnamen

Der Clustername entspricht dem Namen des Big-Data-Clusters und dem Kubernetes-Namespace, der bei der Bereitstellung erstellt wird. Er wird im folgenden Teil der Bereitstellungskonfigurationsdatei `bdc.json` angegeben:

```json
"metadata": {
    "kind": "BigDataCluster",
    "name": "mssql-cluster"
},
```

Der folgende Befehl sendet ein Schlüssel-Wert-Paar an den `--json-values`-Parameter, um den Big Data-Clusternamen in `test-cluster` zu ändern:

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "metadata.name=test-cluster"
```

> [!IMPORTANT]
> Der Name Ihres Big-Data-Clusters darf nur alphanumerische Zeichen (Kleinbuchstaben) und keine Leerzeichen enthalten. Alle Kubernetes-Artefakte (Container, Pods, zustandsbehaftete Sets, Dienste) für den Cluster werden in einem Namespace mit dem gleichen Namen wie der angegebene Clustername erstellt.

## <a name="update-endpoint-ports"></a><a id="ports"></a> Aktualisieren von Endpunktports

Endpunkte werden für den Controller in der Datei `control.json` und für das Gateway und die SQL Server-Masterinstanz in den entsprechenden Abschnitten in der Datei `bdc.json` definiert. Der folgende Teil der Konfigurationsdatei `control.json` zeigt die Endpunktdefinitionen für den Controller an:

```json
{
  "endpoints": [
    {
      "name": "Controller",
      "serviceType": "LoadBalancer",
      "port": 30080
    },
    {
      "name": "ServiceProxy",
      "serviceType": "LoadBalancer",
      "port": 30777
    }
  ]
}
```

Im folgenden Beispiel wird das Inline-JSON-Format verwendet, um den Port für den `controller`-Endpunkt zu ändern:

```bash
azdata bdc config replace --config-file custom-bdc/control.json --json-values "$.spec.endpoints[?(@.name==""Controller"")].port=30000"
```

## <a name="configure-scale"></a><a id="replicas"></a> Konfigurieren der Staffelung

Die Konfigurationen der einzelnen Ressourcen, wie etwa des Speicherpools, werden in der Konfigurationsdatei `bdc.json` definiert. Der folgende Teil der Datei `bdc.json` zeigt die Definition einer `storage-0`-Ressource:

```json
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
                "spark-defaults-conf.spark.driver.memory": "2g",
                "spark-defaults-conf.spark.driver.cores": "1",
                "spark-defaults-conf.spark.executor.instances": "3",
                "spark-defaults-conf.spark.executor.memory": "1536m",
                "spark-defaults-conf.spark.executor.cores": "1",
                "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
                "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
                "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
                "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
                "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
            }
        }
    }
}
```

Sie können die Anzahl der Instanzen in einem Speicher-, Compute- und/oder Datenpool konfigurieren, indem Sie den `replicas`-Wert für die einzelnen Pools ändern. Im folgenden Beispiel wird das Inline-JSON-Format verwendet, um diese Werte für die Speicher-, Compute- und Datenpools in `10`, `4` bzw. `4` zu ändern:

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.storage-0.spec.replicas=10"
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.compute-0.spec.replicas=4"
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.data-0.spec.replicas=4"
```

> [!NOTE]
> Für Compute-und Datenpools können jeweils maximal `8` Instanzen überprüft werden. Diese Beschränkung wird zum Zeitpunkt der Bereitstellung nicht erzwungen. Es wird jedoch abgeraten, in Produktionsbereitstellungen eine höhere Staffelung zu konfigurieren.

## <a name="configure-storage"></a><a id="storage"></a> Konfigurieren des Speichers

Sie können auch die Speicherklasse und die Merkmale ändern, die für die einzelnen Pools verwendet werden. Im folgenden Beispiel wird dem Speicher- und dem Datenpool eine benutzerdefinierte Speicherklasse zugewiesen, und die Größe des Anspruchs auf persistente Volumes zum Speichern von Daten wird für HDFS (Speicherpool) auf 500 GB und für den Master- und Datenpool auf 100 GB aktualisiert. 

> [!TIP]
> Weitere Informationen zur Speicherkonfiguration finden Sie unter [Datenpersistenz mit Big-Data-Cluster für SQL Server in Kubernetes](concept-data-persistence.md).

Erstellen Sie zuerst die Datei „patch.json“ wie unten beschrieben, um die *Speichereinstellungen* anzupassen.

```json
{
        "patch": [
                {
                        "op": "add",
                        "path": "spec.resources.storage-0.spec.storage",
                        "value": {
                                "data": {
                                        "size": "500Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                },
        {
                        "op": "add",
                        "path": "spec.resources.master.spec.storage",
                        "value": {
                                "data": {
                                        "size": "100Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                },
                {
                        "op": "add",
                        "path": "spec.resources.data-0.spec.storage",
                        "value": {
                                "data": {
                                        "size": "100Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                }
        ]
}

```

Danach können Sie mit dem Befehl `azdata bdc config patch` die Konfigurationsdatei `bdc.json` aktualisieren.
```bash
azdata bdc config patch --config-file custom-bdc/bdc.json --patch ./patch.json
```

> [!NOTE]
> Eine auf `kubeadm-dev-test` basierende Konfigurationsdatei besitzt keine Speicherdefinition für die einzelnen Pools, aber Sie können sie mit dem obigen Prozess bei Bedarf hinzufügen.

## <a name="configure-storage-pool-without-spark"></a><a id="sparkstorage"></a> Konfigurieren von Speicherpools ohne Spark

Sie können die Speicherpools auch so konfigurieren, dass sie ohne Spark ausgeführt werden, und einen separaten Spark-Pool erstellen. Diese Konfiguration ermöglicht es Ihnen, Spark-Computeleistung unabhängig vom Speicher zu skalieren. Informationen zum Konfigurieren des Spark-Pools finden Sie im Abschnitt [Erstellen eines Spark-Pools](#sparkpool) in diesem Artikel.

> [!NOTE]
> Das Bereitstellen eines Big Data Clusters ohne Spark wird nicht unterstützt. Daher müssen Sie entweder `includeSpark` auf `true` festlegen, oder Sie müssen einen separaten [Spark-Pool](#sparkpool) mit mindestens einer Instanz erstellen. Sie können auch festlegen, dass Spark beides im Speicherpool ausführt (`includeSpark` ist `true`), und einen separaten Spark-Pool einrichten.

Standardmäßig ist die Einstellung `includeSpark` für die Speicherpoolressource auf „true“ festgelegt. Daher müssen Sie das Feld `includeSpark` der Speicherkonfiguration bearbeiten, um Änderungen vorzunehmen. Der folgende Befehl zeigt, wie dieser Wert mithilfe von Inline-JSON bearbeitet wird.

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.storage-0.spec.settings.spark.includeSpark=false"
```

## <a name="create-a-spark-pool"></a><a id="sparkpool"></a> Erstellen eines Spark-Pools

Sie können einen Spark-Pool zusätzlich oder anstelle von Spark-Instanzen erstellen, die im Speicherpool ausgeführt werden. Das folgende Beispiel zeigt, wie Sie einen Spark-Pool mit zwei Instanzen erstellen, indem Sie die Konfigurationsdatei `bdc.json` patchen. 

Erstellen Sie zunächst wie folgt eine `spark-pool-patch.json`-Datei:

```json
{
    "patch": [
        {
            "op": "add",
            "path": "spec.resources.spark-0",
            "value": {
                "metadata": {
                    "kind": "Pool",
                    "name": "default"
                },
                "spec": {
                    "type": "Spark",
                    "replicas": 2
                }
            }
        },
        {
            "op": "add",
            "path": "spec.services.spark.resources/-",
            "value": "spark-0"
        },
        {
            "op": "add",
            "path": "spec.services.hdfs.resources/-",
            "value": "spark-0"
        }
    ]
}
```

Führen Sie anschließend den Befehl `azdata bdc config patch` aus:

```bash
azdata bdc config patch -c custom-bdc/bdc.json -p spark-pool-patch.json
```

## <a name="configure-pod-placement-using-kubernetes-labels"></a><a id="podplacement"></a> Konfigurieren von Pod-Platzierungen mit Kubernetes-Bezeichnungen

Sie können Pod-Platzierungen auf Kubernetes-Knoten steuern, die über bestimmten Ressourcen verfügen, um verschiedene Arten von Arbeitsauslastungsanforderungen zu erfüllen. Mithilfe von Kubernetes-Bezeichnungen können Sie anpassen, welche Knoten in Ihrem Kubernetes-Cluster für die Bereitstellung von Big Data-Clusterressourcen verwendet werden. Darüber hinaus können Sie auch einschränken, welche Knoten für bestimmte Ressourcen verwendet werden.
Beispielsweise können Sie sicherstellen, dass die Speicherpoolressourcenpods auf Knoten mit mehr Speicherplatz platziert werden, während SQL Server-Masterinstanzen in Knoten platziert werden, die über höhere CPU- und Arbeitsspeicherressourcen verfügen. In diesem Fall erstellen Sie zuerst einen heterogenen Kubernetes-Cluster mit unterschiedlichen Hardwaretypen. Anschließend nehmen Sie die entsprechende [Zuweisung von Knotenbezeichnungen](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) vor. Bei der Bereitstellung von Big Data-Clustern können Sie die gleichen Bezeichnungen auf Clusterebene angeben, um festzulegen, welche Knoten mithilfe des Attributs `clusterLabel` in der Datei `control.json` für Big Data-Cluster verwendet werden. Dann werden für die Platzierung auf Poolebene andere Bezeichnungen verwendet. Diese Bezeichnungen können mithilfe des Attributs `nodeLabel` in den Konfigurationsdateien für die Big Data-Clusterbereitstellung angegeben werden. Kubernetes weist die Pods auf Knoten zu, die den angegebenen Bezeichnungen entsprechen. Im Kubernetes-Cluster müssen den Knoten die Bezeichnungsschlüssel `mssql-cluster` (zum Angeben, welche Knoten für Big Data-Cluster verwendet werden) und `mssql-resource` (zum Angeben, auf welchen Knoten die Pods für die verschiedenen Ressourcen jeweils platziert werden) hinzugefügt werden. Als Werte für diese Bezeichnungen können Sie beliebige Zeichenfolgen auswählen.

> [!NOTE]
> Aufgrund der Art der Pods, die Metriken auf Knotenebene sammeln, werden `metricsdc`-Pods auf allen Knoten mit der Bezeichnung `mssql-cluster` bereitgestellt und `mssql-resource` wird auf diese Pods nicht angewendet.

Im folgenden Beispiel wird gezeigt, wie eine benutzerdefinierte Konfigurationsdatei so bearbeitet wird, dass sie anschließend eine `bdc`-Knotenbezeichnung für den gesamten Big Data-Cluster, eine `bdc-master`-Bezeichnung zum Platzieren von SQL Server Master-Instanzpods auf einem bestimmten Knoten, `bdc-storage-pool` für Speicherpoolressourcen, `bdc-compute-pool` für Computepool- und Datenpoolpods und `bdc-shared` für die restlichen Ressourcen enthält. 

Bezeichnen Sie zunächst die Kubernetes-Knoten:

```bash
kubectl label node <kubernetesNodeName1> mssql-cluster=bdc mssql-resource=bdc-shared --overwrite=true
kubectl label node <kubernetesNodeName2> mssql-cluster=bdc mssql-resource=bdc-master --overwrite=true
kubectl label node <kubernetesNodeName3> mssql-cluster=bdc mssql-resource=bdc-compute-pool --overwrite=true
kubectl label node <kubernetesNodeName4> mssql-cluster=bdc mssql-resource=bdc-compute-pool --overwrite=true
kubectl label node <kubernetesNodeName5> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName6> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName7> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName8> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
```

Aktualisieren Sie dann die Konfigurationsdateien der Clusterbereitstellung so, dass diese anschließend die Bezeichnungswerte enthalten. In diesem Beispiel wird davon ausgegangen, dass Sie Konfigurationsdateien in einem `custom-bdc`-Profil anpassen. In den integrierten Konfigurationen sind standardmäßig keine `nodeLabel`- und `clusterLabel`-Schlüssel vorhanden. Daher müssen Sie entweder eine benutzerdefinierte Konfigurationsdatei manuell bearbeiten oder die `azdata bdc config add`-Befehle verwenden, um die erforderlichen Änderungen vorzunehmen.

```bash
azdata bdc config add -c custom-bdc/control.json -j "$.spec.clusterLabel=bdc"
azdata bdc config add -c custom-bdc/control.json -j "$.spec.nodeLabel=bdc-shared"

azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.master.spec.nodeLabel=bdc-master"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.compute-0.spec.nodeLabel=bdc-compute-pool"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.data-0.spec.nodeLabel=bdc-compute-pool"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.storage-0.spec.nodeLabel=bdc-storage-pool"

# below can be omitted in which case we will take the node label default from the control.json
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.nmnode-0.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.sparkhead.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.zookeeper.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.gateway.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.appproxy.spec.nodeLabel=bdc-shared"
```
>[!NOTE]
> Es gilt als bewährte Methode, zu vermeiden, dem Kubernetes-Master eine der oben erwähnten BDC-Rollen zu geben. Wenn Sie diese Rollen trotzdem dem Kubernetes-Masterknoten zuweisen möchten, müssen Sie [dessen Taint ``master:NoSchedule`` entfernen.](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) Beachten Sie, dass der Masterknoten dadurch überladen werden könnte und seine Fähigkeit einbüßen könnte, seine Kubernetes-Verwaltungsaufgaben für größere Cluster durchzuführen. Es ist normal, dass einige Pods für den Master einer Bereitstellung geplant sind. Sie tolerieren bereits den Taint ``master:NoSchedule`` und werden hauptsächlich dazu verwendet, bei der Verwaltung des Clusters zu helfen. 

## <a name="other-customizations-using-json-patch-files"></a><a id="jsonpatch"></a> Weitere Anpassungen mithilfe von JSON-Patchdateien

JSON-Patchdateien konfigurieren mehrere Einstellungen gleichzeitig. Weitere Informationen zu JSON-Patches finden Sie unter [JSON-Patches in Python](https://github.com/stefankoegl/python-json-patch) und [JSONPath Online Evaluator](https://jsonpath.com/).

Mit den folgenden `patch.json`-Dateien werden die folgenden Änderungen durchgeführt:

- Aktualisierung des Ports eines einzelnen Endpunkts in der Datei `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "$.spec.endpoints[?(@.name=='Controller')].port",
      "value": 30000
    }
  ]
}
```

- Aktualisierung aller Endpunkte (`port` und `serviceType`) in der Datei `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.endpoints",
      "value": [
        {
          "serviceType": "LoadBalancer",
          "port": 30001,
          "name": "Controller"
        },
        {
          "serviceType": "LoadBalancer",
          "port": 30778,
          "name": "ServiceProxy"
        }
      ]
    }
  ]
}
```

- Aktualisierung der Controllerspeichereinstellungen in der Datei `control.json`. Diese Einstellungen gelten für alle Clusterkomponenten, es sei denn, sie werden auf Poolebene überschrieben.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.storage",
      "value": {
        "data": {
          "className": "managed-premium",
          "accessMode": "ReadWriteOnce",
          "size": "100Gi"
        },
        "logs": {
          "className": "managed-premium",
          "accessMode": "ReadWriteOnce",
          "size": "32Gi"
        }
      }
    }
  ]
}
```

- Aktualisierung des Speicherklassennamens in der Datei `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.storage.data.className",
      "value": "managed-premium"
    }
  ]
}
```

- Aktualisierung der Poolspeichereinstellungen für den Speicherpool in der Datei `bdc.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.resources.storage-0.spec",
      "value": {
        "type": "Storage",
        "replicas": 2,
        "storage": {
          "data": {
            "size": "100Gi",
            "className": "myStorageClass",
            "accessMode": "ReadWriteOnce"
          },
          "logs": {
            "size": "32Gi",
            "className": "myStorageClass",
            "accessMode": "ReadWriteOnce"
          }
        }
      }
    }
  ]
}
```

- Aktualisierung der Spark-Einstellungen für den Speicherpool in der Datei `bdc.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.services.spark.settings",
      "value": {
            "spark-defaults-conf.spark.driver.memory": "2g",
            "spark-defaults-conf.spark.driver.cores": "1",
            "spark-defaults-conf.spark.executor.instances": "3",
            "spark-defaults-conf.spark.executor.memory": "1536m",
            "spark-defaults-conf.spark.executor.cores": "1",
            "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
            "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
            "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
            "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
            "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
      }
    }
  ]
}
```

> [!TIP]
> Weitere Informationen zur Struktur und zu den Optionen zum Ändern einer Bereitstellungskonfigurationsdatei finden Sie unter [Referenz zur Bereitstellungskonfigurationsdatei für Big-Data-Cluster](reference-deployment-config.md).

Verwenden Sie `azdata bdc config`-Befehle, um die Änderungen in der JSON-Patchdatei anzuwenden. Im folgenden Beispiel wird die `patch.json`-Datei auf eine `custom-bdc/bdc.json`-Zielkonfigurationsdatei für die Bereitstellung angewendet.

```bash
azdata bdc config patch --config-file custom-bdc/bdc.json --patch-file ./patch.json
```

## <a name="disable-elasticsearch-to-run-in-privileged-mode"></a>Deaktivieren der Ausführung von ElasticSearch im privilegierten Modus

Der ElasticSearch-Container wird im Big Data-Cluster standardmäßig im privilegierten Modus ausgeführt. Mit dieser Einstellung wird sichergestellt, dass der Container bei seiner Initialisierung über die Berechtigungen zum Aktualisieren einer Einstellung auf dem Host verfügt, die zum Verarbeiten einer größeren Menge an Protokollen durch ElasticSearch erforderlich sind. Weitere Informationen zu diesem Thema finden Sie in [diesem Artikel](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html). 

Um für den Container, auf dem ElasticSearch ausgeführt wird, die Ausführung im privilegierten Modus zu deaktivieren, müssen Sie den Abschnitt `settings` in der Datei `control.json` aktualisieren und den Wert von `vm.max_map_count` auf `-1` festlegen. Im Folgenden ist ein Beispiel für diesen Abschnitt dargestellt:

```json
{
    "apiVersion": "v1",
    "metadata": {...},
    "spec": {
        "docker": {...},
        "storage": {...},
        "endpoints": [...],
        "settings": {
            "ElasticSearch": {
                "vm.max_map_count": "-1"
            }
        }
    }
}
```

Sie können die Datei `control.json` manuell bearbeiten und den obigen Abschnitt zu `spec` hinzufügen. Alternativ können Sie auch eine `elasticsearch-patch.json`-Patchdatei erstellen und die Datei `control.json` mithilfe der [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] patchen:

```json
{
  "patch": [
    {
      "op": "add",
      "path": "spec.settings",
      "value": {
            "ElasticSearch": {
                "vm.max_map_count": "-1"
        }
      }
    }
  ]
}
```

Führen Sie diesen Befehl aus, um die Konfigurationsdatei zu patchen:

```bash
azdata bdc config patch --config-file custom-bdc/control.json --patch-file elasticsearch-patch.json
```

> [!IMPORTANT]
> Es wird empfohlen, die Einstellung `max_map_count` auf allen Hosts im Kubernetes-Cluster gemäß den Anweisungen in [diesem Artikel ](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html) manuell zu aktualisieren.

## <a name="turn-pods-and-nodes-metrics-collection-onoff"></a>Aktivieren und Deaktivieren der Sammlung von Pod- und Knotenmetriken

SQL Server 2019 CU5 hat zwei Funktionsparameter eingeführt, um die Sammlung von Pod- und Knotenmetriken zu steuern. Wenn Sie Ihre Kubernetes-Infrastruktur mithilfe anderer Lösungen überwachen, können Sie die integrierte Metriksammlung für Pods und Hostknoten deaktivieren, indem Sie *allowNodeMetricsCollection* und *allowPodMetricsCollection* in der Bereitstellungskonfigurationsdatei *control.json* auf *False* festlegen. Bei OpenShift-Umgebungen werden diese Einstellungen in den integrierten Bereitstellungsprofilen standardmäßig auf *False* festgelegt, da für das Sammeln von Pod- und Knotenmetriken von Berechtigungen abhängige Funktionen erforderlich sind.
Führen Sie diesen Befehl aus, um die Werte dieser Einstellungen in Ihrer benutzerdefinierten Konfigurationsdatei mithilfe der *azdata*-CLI zu aktualisieren:

```bash
 azdata bdc config replace -c custom-bdc/control.json -j "$.security.allowNodeMetricsCollection=false"
 azdata bdc config replace -c custom-bdc/control.json -j "$.security.allowPodMetricsCollection=false"
 ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von Konfigurationsdateien in Big Data-Clusterbereitstellungen finden Sie unter [Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md#configfile).