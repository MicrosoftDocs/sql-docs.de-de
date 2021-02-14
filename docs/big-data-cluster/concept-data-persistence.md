---
title: Datenpersistenz in Kubernetes
titleSuffix: SQL Server big data clusters
description: Hier erfahren Sie, wie persistente Volumes ein Plug-In-Modell für Speicher in Kubernetes bereitstellen. Außerdem erfahren Sie, wie Datenpersistenz in einem Big Data-Cluster in SQL Server 2019 funktioniert.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a2836a6966727c0c2d27b363048f2e1dcd6afa49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052098"
---
# <a name="data-persistence-with-sql-server-big-data-cluster-in-kubernetes"></a>Datenpersistenz mit SQL Server-Big Data-Clustern in Kubernetes

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

[Persistente Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) stellen ein Plug-In-Modell für Speicher in Kubernetes bereit. Dabei wird die Bereitstellung des Speichers von seiner Nutzung abstrahiert. Daher können Sie Ihren eigenen hochverfügbaren Speicher verwenden und in den SQL Server-Big-Data-Cluster einbinden. Dadurch erhalten Sie die vollständige Kontrolle über die Art des Speichers sowie über die erforderliche Verfügbarkeit und Leistung. Kubernetes unterstützt [verschiedene Arten von Speicherlösungen](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner), einschließlich Azure-Datenträger und -Dateien, Network File System (NFS) und lokalen Speicher.

## <a name="configure-persistent-volumes"></a>Konfigurieren persistenter Volumes

Ein SQL Server-Big Data-Cluster nutzt diese persistenten Volumes durch die Verwendung von [Speicherklassen](https://kubernetes.io/docs/concepts/storage/storage-classes/). Sie können unterschiedliche Speicherklassen für unterschiedliche Arten von Speicher erstellen und diese bei der Bereitstellung des Big Data-Clusters angeben. Sie können konfigurieren, welche Speicherklasse und welche Größe für das persistente Volume auf Poolebene für welchen Zweck verwendet werden soll. Zum Erstellen von [persistenten Volumeansprüchen](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims) verwendet der SQL Server-Big Data-Cluster den angegebenen Speicherklassennamen für jede Komponente, die persistente Volumes benötigt. Anschließend werden die entsprechenden persistenten Volumes im Pod bereitgestellt. 

Im Folgenden sind einige wichtige Aspekte aufgeführt, die beim Planen der Speicherkonfiguration für einen Big Data-Cluster zu beachten sind:

- Für die erfolgreiche Bereitstellung eines Big Data-Clusters müssen Sie sicherstellen, dass die erforderliche Anzahl von persistenten Volumes verfügbar ist. Wenn Sie Ihr System in einem AKS-Cluster (Azure Kubernetes Service) bereitstellen und eine der integrierten Speicherklassen (`default` oder `managed-premium`) verwenden, unterstützen diese die dynamische Bereitstellung für die persistenten Volumes. Das bedeutet, dass Sie die persistenten Volumes nicht vorab erstellen müssen. Sie müssen jedoch sicherstellen, dass die im AKS-Cluster verfügbaren Workerknoten so viele Datenträger anfügen können, wie persistente Volumes für die Bereitstellung erforderlich sind. Abhängig von der [VM-Größe](/azure/virtual-machines/linux/sizes), die für die Workerknoten angegeben wird, kann jeder Knoten eine bestimmte Anzahl von Datenträgern anfügen. Bei einem Cluster mit Standardgröße (ohne Hochverfügbarkeit) sind mindestens 24 Datenträger erforderlich. Wenn Sie Hochverfügbarkeit aktivieren oder einen Pool zentral hochskalieren, müssen Sie sicherstellen, dass für jedes zusätzliche Replikat mindestens zwei persistente Volumes vorhanden sind. Dies gilt unabhängig von der Ressource, die Sie zentral hochskalieren.

- Wenn der Speicheranbieter für die Speicherklasse, die Sie in der Konfiguration bereitstellen, keine dynamische Bereitstellung unterstützt, müssen Sie die persistenten Volumes vorab erstellen. Die dynamische Bereitstellung wird beispielsweise nicht vom `local-storage`-Anbieter unterstützt. Weitere Informationen zu einem mit `kubeadm` bereitgestellten Kubernetes-Cluster finden Sie in diesem [Beispielskript](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu).

- Wenn Sie einen Big Data-Cluster bereitstellen, können Sie für alle Komponenten im Cluster dieselbe Speicherklasse konfigurieren. Als bewährte Methode für eine Produktionsbereitstellung benötigen verschiedene Komponenten jedoch unterschiedliche Speicherkonfigurationen, um verschiedene Workloads in Bezug auf die Größe oder den Durchsatz zu unterstützen. Sie können die im Controller angegebene Standardspeicherkonfiguration für jede SQL Server-Masterinstanz und für alle Datasets und Speicherpools überschreiben. Diese Schritte werden in diesem Artikel anhand von Beispielen veranschaulicht.

- Bei der Berechnung der Größenanforderungen für den Speicherpool müssen Sie den Replikationsfaktor berücksichtigen, mit dem HDFS konfiguriert ist.  Der Replikationsfaktor kann bei der Bereitstellung in der Konfigurationsdatei für die Clusterbereitstellung konfiguriert werden. Der Standardwert für die dev-test-Profile (d. h. `aks-dev-test` oder `kubeadm-dev-test`) ist 2. Für die Profile, die für Produktionsbereitstellungen empfohlen werden (d. h. `kubeadm-prod`) lautet der Standardwert 3. Als Best Practice empfehlen wir Ihnen, dass Sie Ihre Produktionsbereitstellung eines Big Data-Clusters mit einem Replikationsfaktor für HDFS von mindestens 3 konfigurieren. Der Wert des Replikationsfaktors beeinflusst die Anzahl an Instanzen im Speicherpool: Sie müssen mindestens so viele Speicherpoolinstanzen bereitstellen, dass sie dem Wert des Replikationsfaktors entsprechen. Zusätzlich müssen Sie die Speichergröße entsprechend anpassen. Berücksichtigen Sie dabei auch Daten, die in HDFS entsprechend dem Wert des Replikationsfaktors repliziert werden. Weitere Informationen zur Datenreplikation in HDFS finden Sie [hier](https://hadoop.apache.org/docs/r3.2.1/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Data_Replication). 

- Ab SQL Server 2019 CU1 bietet Big Data-Cluster (BDC) keine Möglichkeit, die Speicherkonfigurationseinstellung nach einer Bereitstellung zu aktualisieren. Dadurch wird verhindert, dass Sie nach einer Bereitstellung BDC-Vorgänge verwenden, um die Größe des persistenten Volumes jeder Instanz zu ändern oder einen Pool zu skalieren. Daher sollte das Speicherlayout vor der Bereitstellung eines Big Data-Clusters sorgfältig geplant werden. Allerdings können Sie die Größe eines persistenten Volumes über Kubernetes-APIs direkt erweitern. In diesem Fall werden BDC-Metadaten nicht aktualisiert, und es ergeben sich Inkonsistenzen in Bezug auf die Volumegrößen in den Metadaten des Konfigurationsclusters.

- Durch die Bereitstellung als Containeranwendungen in Kubernetes und die Verwendung von Features wie zustandsbehafteten Sätzen und permanentem Speicher stellt Kubernetes sicher, dass Pods im Falle von Integritätsproblemen neu gestartet und an denselben permanenten Speicher angefügt werden. Für den Fall, dass ein Knoten ausfällt und ein Pod auf einem anderen Knoten neu gestartet werden muss, besteht jedoch ein höheres Risiko der Nichtverfügbarkeit, wenn für den fehlerhaften Knoten lokaler Speicher verwendet wird. Um dieses Risiko zu mindern, müssen Sie entweder für zusätzliche Redundanz sorgen und [Funktionen für Hochverfügbarkeit](deployment-high-availability.md) aktivieren oder redundanten Remotespeicher verwenden. Hier finden Sie eine Übersicht über die Speicheroptionen für verschiedene Komponenten in den Big Data-Clustern.

| Ressourcen | Speichertyp für Daten | Speichertyp für Protokoll |  Hinweise |
|---|---|---|--|
| SQL Server-Masterinstanz | Lokaler Speicher (Replikate >= 3) oder redundanter Remotespeicher (Replikat = 1) | Lokaler Speicher | Die auf einem zustandsbehafteten Satz basierende Implementierung, bei der Pods auf den Knoten verbleiben, stellt sicher, dass Neustarts und vorübergehende Fehler keinen Datenverlust verursachen. |
| Computepool | Lokaler Speicher | Lokaler Speicher | Keine Benutzerdaten gespeichert |
| Datenpool | Lokaler Speicher/redundanter Remotespeicher | Lokaler Speicher | Verwenden Sie redundanten Remotespeicher, wenn der Datenpool nicht aus anderen Datenquellen neu erstellt werden kann.  |
| Speicherpool (HDFS) | Lokaler Speicher/redundanter Remotespeicher | Lokaler Speicher | HDFS-Replikation ist aktiviert. |
| Spark-Pool | Lokaler Speicher | Lokaler Speicher | Keine Benutzerdaten gespeichert |
| Steuerung (controldb, metricsdb, logsdb)| Redundanter Remotespeicher | Lokaler Speicher | Wichtig für die Verwendung von Speicherebenenredundanz für Big Data-Cluster-Metadaten. |

> [!IMPORTANT]
> Stellen Sie sicher, dass sich steuerungsbezogene Komponenten auf einem redundanten Remotespeichergerät befinden. Ein `controldb-0`-Pod hostet eine SQL Server-Instanz mit Datenbanken, in denen alle Metadaten zu den Clusterdienstzuständen, verschiedenen Einstellungen und andere relevante Informationen gespeichert werden. Wenn diese Instanz nicht verfügbar ist, treten Integritätsprobleme mit anderen abhängigen Diensten im Cluster auf.

## <a name="configure-big-data-cluster-storage-settings"></a>Konfigurieren der Speichereinstellungen für Big-Data-Cluster

Wie bei anderen Anpassungen können Sie zum Zeitpunkt der Bereitstellung in den Clusterkonfigurationsdateien für jeden Pool in der `bdc.json`-Konfigurationsdatei und für die Steuerungsdienste in der `control.json`-Datei die Speichereinstellungen angeben. Wenn in den Poolspezifikationen keine Speicherkonfigurationseinstellungen vorhanden sind, werden die Steuerungsspeichereinstellungen für alle anderen Komponenten verwendet, einschließlich SQL Server-Masterinstanz (`master`-Ressource), HDFS (`storage-0`-Ressource) und Datenpoolkomponenten. Das folgende Codebeispiel zeigt den Speicherkonfigurationsabschnitt, den Sie in die Spezifikation aufnehmen können:

```json
    "storage": 
    {
      "data": {
        "className": "default",
        "accessMode": "ReadWriteOnce",
        "size": "15Gi"
      },
      "logs": {
        "className": "default",
        "accessMode": "ReadWriteOnce",
        "size": "10Gi"
    }
```

Bei der Bereitstellung des Big Data-Clusters wird zum Speichern von Daten, Metadaten und Protokollen für verschiedene Komponenten persistenter Speicher verwendet. Sie können die Größe der als Teil der Bereitstellung erstellten persistenten Volumeansprüche anpassen. Es wird empfohlen, die Speicherklassen mit einer *Retain-* [Rückforderungsrichtlinie](https://kubernetes.io/docs/concepts/storage/storage-classes/#reclaim-policy) zu verwenden.

> [!WARNING]
> Die Ausführung ohne persistenten Speicher kann zwar in einer Testumgebung funktionieren, führt dann allerdings möglicherweise zu einem nicht funktionsfähigen Cluster. Beim Neustart eines Pods gehen Clustermetadaten und/oder Benutzerdaten dauerhaft verloren. Von dieser Konfiguration wird abgeraten.

Im Abschnitt [Konfigurieren von Speicher](#config-samples) finden Sie weitere Beispiele für die Konfiguration der Speichereinstellungen Ihrer SQL Server-Big Data-Cluster-Bereitstellung.

## <a name="aks-storage-classes"></a>Azure Kubernetes Service-Speicherklassen

Azure Kubernetes Service (AKS) verfügt über [zwei integrierte Speicherklassen](/azure/aks/azure-disks-dynamic-pv/) (`default` und `managed-premium`) und einen dynamischen Anbieter für diese Klassen. Sie können eine dieser Klassen angeben oder Ihre eigene Speicherklasse zum Bereitstellen von Big Data-Clustern mit aktiviertem persistenten Speicher erstellen. Standardmäßig sind in der integrierten Clusterkonfigurationsdatei für AKS (`aks-dev-test`) permanente Speicherkonfigurationen enthalten, die die Speicherklasse `default` verwenden.

> [!WARNING]
> Persistente Volumes, die mit den integrierten Speicherklassen`default` und `managed-premium` erstellt wurden, verfügen über eine *Delete*-Rückforderungsrichtlinie. Wenn Sie also den SQL Server-Big Data-Cluster löschen, werden sowohl die persistenten Volumeansprüche als auch die persistenten Volumes gelöscht. Sie können mithilfe des `azure-disk`-Anbieters benutzerdefinierte Speicherklassen mit einer `Retain`-Rückforderungsrichtlinie erstellen, wie [hier](/azure/aks/concepts-storage/#storage-classes) beschrieben.

## <a name="storage-classes-for-kubeadm-clusters"></a>Speicherklassen für `kubeadm`-Cluster 

Kubernetes-Cluster, die mit `kubeadm` bereitgestellt werden, verfügen über keine integrierte Speicherklasse. Sie müssen mithilfe des lokalen Speichers oder Ihres [bevorzugten Speicheranbieters](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner) eigene Speicherklassen und persistente Volumes erstellen. In diesem Fall legen Sie den Wert für `className` auf die konfigurierte Speicherklasse fest.

> [!IMPORTANT]
>  In den integrierten Bereitstellungskonfigurationsdateien für kubeadm (`kubeadm-dev-test` oder `kubeadm-prod`) ist für Daten- und Protokollspeicher kein Speicherklassenname angegeben. Vor der Bereitstellung müssen Sie die Konfigurationsdatei anpassen und den Wert für `className` festlegen. Anderenfalls tritt bei der Überprüfung vor der Bereitstellung ein Fehler auf. Die Bereitstellung umfasst außerdem einen Validierungsschritt, mit dem überprüft wird, ob die Speicherklasse vorhanden ist. Dabei wird die Existenz der erforderlichen persistenten Volumes nicht überprüft. Stellen Sie sicher, dass Sie abhängig vom Umfang Ihres Clusters ausreichend Volumes erstellen. Für die standardmäßig festgelegte minimale Clustergröße (Standardskalierung, keine Hochverfügbarkeit) müssen Sie mindestens 24 persistente Volumes erstellen. Unter [Erstellen eines Kubernetes-Clusters mithilfe von Kubeadm](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu) finden Sie ein Beispiel für das Erstellen persistenter Volumes mithilfe eines lokalen Anbieters.

## <a name="customize-storage-configurations-for-each-pool"></a>Anpassen der Speicherkonfigurationen für jeden Pool

Für alle Anpassungen müssen Sie zuerst eine Kopie der integrierten Konfigurationsdatei erstellen, die Sie verwenden möchten. Mit dem folgenden Befehl wird beispielsweise eine Kopie der `aks-dev-test`-Konfigurationsdateien für Bereitstellungen in einem Unterverzeichnis namens `custom` erstellt:

```bash
azdata bdc config init --source aks-dev-test --target custom
```

Bei diesem Vorgang werden zwei Dateien erstellt (`bdc.json` und `control.json`), die Sie entweder manuell oder mithilfe des Befehls `azdata bdc config` anpassen können. Sie können eine Kombination aus den Bibliotheken „jsonpath“ und „jsonpatch“ verwenden, um Möglichkeiten zum Bearbeiten von Konfigurationsdateien bereitzustellen.


### <a name="configure-storage-class-name-andor-claims-size"></a><a id="config-samples"></a> Konfigurieren von Speicherklassennamen und/oder der Anspruchsgröße

Die Größe der persistenten Volumeansprüche, die für alle im Cluster bereitgestellten Pods bereitgestellt werden, beträgt standardmäßig 10 GB. Sie können diesen Wert vor der Clusterbereitstellung in einer benutzerdefinierten Konfigurationsdatei anpassen und auf Ihre ausgeführten Workloads abstimmen.

Im folgenden Beispiel wird die Anspruchsgröße für persistente Volumes in der `control.json`-Datei auf 32 GB aktualisiert. Diese Einstellung wird auf alle Pools angewendet, sofern sie nicht auf Poolebene überschrieben wird.

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.storage.data.size=100Gi"
```

Im folgenden Beispiel wird gezeigt, wie die Speicherklasse für die `control.json`-Datei geändert wird:

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.storage.data.className=<yourStorageClassName>"
```

Sie können die benutzerdefinierte Konfigurationsdatei auch manuell bearbeiten oder einen JSON-Patch verwenden, mit dem die Speicherklasse für den Speicherpool geändert wird. Beispiel:

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "$.spec.resources.storage-0.spec",
      "value": {
        "type":"Storage",
        "replicas":2,
        "storage":{
            "data":{
                    "size": "100Gi",
                    "className": "myStorageClass",
                    "accessMode":"ReadWriteOnce"
                    },
            "logs":{
                    "size":"32Gi",
                    "className":"myStorageClass",
                    "accessMode":"ReadWriteOnce"
                    }
                }
            }
        }
    ]
}
```

Wenden Sie die Patchdatei an. Verwenden Sie den `azdata bdc config patch`-Befehl, um die Änderungen in der JSON-Patchdatei anzuwenden. Im folgenden Beispiel wird die `patch.json`-Datei auf die Zielkonfigurationsdatei für die Bereitstellung (`custom.json`) angewendet:

```bash
azdata bdc config patch --config-file custom/bdc.json --patch-file ./patch.json
```

## <a name="next-steps"></a>Nächste Schritte

- Eine umfassende Dokumentation zu Volumes in Kubernetes finden Sie in der [Kubernetes-Dokumentation zu Volumes](https://kubernetes.io/docs/concepts/storage/volumes/).

- Weitere Informationen zum Bereitstellen eines SQL Server-Big-Data-Clusters finden Sie unter [How to deploy SQL Server big data cluster on Kubernetes (Bereitstellen von SQL Server-Big-Data-Clustern in Kubernetes)](deployment-guidance.md).
