---
title: Hochverfügbarkeit für SQL Server-Container
description: Informieren Sie sich über die Hochverfügbarkeit für SQL Server-Container. Erfahren Sie außerdem, wie Sie einen Container mit SQL Server auf Kubernetes bereitstellen.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/09/2018
ms.topic: article
ms.prod: sql
ms.technology: linux
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: 168b1e11896ac29cf1d34037b1a28d7421fcfcc3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061455"
---
# <a name="high-availability-for-sql-server-containers"></a>Hochverfügbarkeit für SQL Server-Container

Erstellen und verwalten Sie Ihre SQL Server-Instanzen nativ in Kubernetes.

Stellen Sie SQL Server an Docker-Container bereit, die von [Kubernetes](https://kubernetes.io/) verwaltet werden. In Kubernetes kann ein Container mit einer SQL Server-Instanz automatisch wiederhergestellt werden, wenn ein Clusterknoten ausfällt.

SQL Server 2017 führt ein Docker-Image für die Bereitstellung in Kubernetes ein. Sie können das Image mit einem PersistentVolumeClaim (PVC) von Kubernetes konfigurieren. Kubernetes überwacht den SQL Server-Prozess im Container. Wenn Prozess, Pod, Container oder Knoten ausfallen, startet Kubernetes automatisch eine andere Instanz und stellt die Verbindung zum Speicher wieder her.

## <a name="container-with-sql-server-instance-on-kubernetes"></a>Container mit SQL Server-Instanz auf Kubernetes

Kubernetes 1.6 und höher unterstützt [*Speicherklassen*](https://kubernetes.io/docs/concepts/storage/storage-classes/), [*PersistentVolumeClaims*](https://kubernetes.io/docs/concepts/storage/storage-classes/#persistentvolumeclaims) und den [*Volumetyp Azure-Datenträger*](https://github.com/kubernetes/examples/tree/master/staging/volumes/azure_disk). 

In dieser Konfiguration spielt Kubernetes die Rolle des Containerorchestrators. 

![Diagramm eines Kubernetes-SQL Server-Clusters](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql.png)

Im obigen Diagramm ist `mssql-server` eine SQL Server-Instanz (Container) in einem [*Pod*](https://kubernetes.io/docs/concepts/workloads/pods/pod/). Eine [Replikatgruppe](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) stellt sicher, dass der Pod nach dem Ausfall eines Knotens automatisch wiederhergestellt wird. Anwendungen stellen eine Verbindung zum Dienst her. In diesem Fall stellt der Dienst einen Lastenausgleich dar, der eine IP-Adresse hostet, die sich nach dem Ausfall von `mssql-server` nicht ändert.

Kubernetes orchestriert die Ressourcen im Cluster. Wenn ein Knoten, der einen SQL Server-Instanzcontainer hostet, fehlschlägt, startet er einen neuen Container mit einer SQL Server-Instanz und fügt sie an denselben beständigen Speicher an.

SQL Server 2017 und höher unterstützt Container auf Kubernetes.

Informationen zum Erstellen eines Containers in Kubernetes finden Sie unter [Deploy a SQL Server container in Kubernetes (Bereitstellen eines SQL Server-Containers in Kubernetes)](tutorial-sql-server-containers-kubernetes.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Bereitstellen von SQL Server-Containern in Azure Kubernetes Service (AKS) finden Sie in folgenden Beispielen:
* [Bereitstellen von SQL Server in einem Docker-Container](./sql-server-linux-docker-container-deployment.md)
* [Bereitstellen eines SQL Server-Containers in Kubernetes](tutorial-sql-server-containers-kubernetes.md)