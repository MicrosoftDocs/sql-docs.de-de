---
title: Konfigurieren von Kubernetes mit kubeadm
titleSuffix: SQL Server Big Data Clusters
description: Erfahren Sie, wie Sie Kubernetes auf mehreren Ubuntu 16.04- oder 18.04-Computern (physisch oder virtuell) für SQL Server 2019-Big Data-Cluster-Bereitstellungen konfigurieren.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8c189f3a77f68dc212ead363b935016931b1dd6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038877"
---
# <a name="configure-kubernetes-on-multiple-machines-for-sql-server-big-data-cluster-deployments"></a>Konfigurieren von Kubernetes auf mehreren Computern für SQL Server-Big Data-Cluster-Bereitstellungen

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Dieser Artikel enthält ein Beispiel für die Verwendung von **kubeadm** zum Konfigurieren von Kubernetes auf mehreren Computern für [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]-Bereitstellungen. In diesem Beispiel sind mehrere Ubuntu 16.04- oder 18.04 LTS-Computer (physisch oder virtuell) das Ziel. Für Bereitstellungen auf einer anderen Linux-Plattform müssen Sie einige der Befehle so ändern, dass Sie Ihrem System entsprechen.  

> [!TIP] 
> Beispielskripts zum Konfigurieren von Kubernetes finden Sie unter [Create a Kubernetes cluster using Kubeadm on Ubuntu 16.04 LTS or 18.04 LTS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm) (Erstellen eines Kubernetes-Clusters mit Kubeadm auf Ubuntu 16.04 LTS oder 18.04 LTS).
In [diesem ](deployment-script-single-node-kubeadm.md) Thema finden Sie auch ein Beispielskript, das eine kubeadm-Bereitstellung eines einzelnen Knotens auf einem virtuellen Computer automatisiert und dann eine Standardkonfiguration des Big Data-Clusters darauf bereitstellt.

## <a name="prerequisites"></a>Voraussetzungen

- Mindestens 3 physische Linux-Computer oder virtuelle Computer
- Empfohlene Konfiguration pro Computer:
   - 8 CPUs
   - 64GB Arbeitsspeicher
   - 100GB Speicher
 
> [!Important] 
> Stellen Sie vor der Bereitstellung eines Big Data Clusters sicher, dass die Uhren auf allen für die Bereitstellung verwendeten Kubernetes-Knoten synchronisiert werden. Der Big Data-Cluster verfügt über integrierte Integritätseigenschaften für verschiedene zeitempfindliche Dienste. Zudem können zeitliche Abweichungen zu einer falschen Statusangabe führen.

## <a name="prepare-the-machines"></a>Vorbereiten der Computer

Auf jedem Computer müssen mehrere Voraussetzungen erfüllt sein. Führen Sie in einem Bash-Terminal auf jedem Computer die folgenden Befehle aus:

1. Fügen Sie den aktuellen Computer der `/etc/hosts`-Datei hinzu:

   ```bash
   echo $(hostname -i) $(hostname) | sudo tee -a /etc/hosts
   ```

1. Deaktivieren Sie den Austausch auf allen Geräten.

   ```bash
   sudo sed -i "/ swap / s/^/#/" /etc/fstab
   sudo swapoff -a
   ```

1. Importieren Sie die Schlüssel, und registrieren Sie das Repository für Kubernetes.

   ```bash
   sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
   echo 'deb http://apt.kubernetes.io/ kubernetes-xenial main' | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
   ```

1. Konfigurieren Sie die Docker- und Kubernetes-Voraussetzungen auf dem Computer.

   ```bash
   KUBE_DPKG_VERSION=1.15.0-00 #or your other target K8s version, which should be at least 1.13.
   sudo apt-get update && \
   sudo apt-get install -y ebtables ethtool && \
   sudo apt-get install -y docker.io && \
   sudo apt-get install -y apt-transport-https && \
   sudo apt-get install -y kubelet=$KUBE_DPKG_VERSION kubeadm=$KUBE_DPKG_VERSION kubectl=$KUBE_DPKG_VERSION && \
   curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get | bash
   ```
 
1. Legen Sie `net.bridge.bridge-nf-call-iptables=1` fest. Unter Ubuntu 18.04 aktivieren die folgenden Befehle zuerst `br_netfilter`.

   ```bash
   . /etc/os-release
   if [ "$VERSION_CODENAME" == "bionic" ]; then sudo modprobe br_netfilter; fi
   sudo sysctl net.bridge.bridge-nf-call-iptables=1
   ```

## <a name="configure-the-kubernetes-master"></a>Konfigurieren des Kubernetes-Masters

Nachdem Sie die vorherigen Befehle auf den einzelnen Computern ausgeführt haben, wählen Sie einen der Computer als Ihren Kubernetes-Master aus. Führen Sie dann die folgenden Befehle auf diesem Computer aus.

1. Erstellen Sie zunächst mit dem folgenden Befehl eine rbac.yaml-Datei in Ihrem aktuellen Verzeichnis. 

   ```bash
   cat <<EOF > rbac.yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: default-rbac
   subjects:
   - kind: ServiceAccount
     name: default
     namespace: default
   roleRef:
     kind: ClusterRole
     name: cluster-admin
     apiGroup: rbac.authorization.k8s.io
   EOF
   ```

1. Initialisieren Sie den Kubernetes-Master auf diesem Computer. Im folgenden Beispielskript wird die Kubernetes-Version `1.15.0` festgelegt. Welche Version Sie verwenden, hängt von Ihrem Kubernetes-Cluster ab.

   ```bash
   KUBE_VERSION=1.15.0
   sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --kubernetes-version=$KUBE_VERSION
   ```

   Die erfolgreiche Initialisierung des Kubernetes-Masters sollte ausgegeben werden.

1. Beachten Sie den `kubeadm join`-Befehl, den Sie auf den anderen Servern für den Beitritt zum Kubernetes-Cluster verwenden müssen. Kopieren Sie diesen zur späteren Verwendung.

   ![kubeadm join](./media/deploy-with-kubeadm/kubeadm-join.png)

1. Richten Sie eine Kubernetes-Konfigurationsdatei für Ihr Basisverzeichnis ein.

   ```bash
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```

1. Konfigurieren Sie den Cluster und das Kubernetes-Dashboard.

   ```bash
   kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
   helm init
   kubectl apply -f rbac.yaml
   kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
   kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
   ```

## <a name="configure-the-kubernetes-agents"></a>Konfigurieren der Kubernetes-Agents

Die anderen Computer fungieren im Cluster als Kubernetes-Agents. 

Führen Sie auf allen anderen Computern den Befehl `kubeadm join` aus, den Sie im vorherigen Abschnitt kopiert haben.

![kubeadm join Agents](./media/deploy-with-kubeadm/kubeadm-join-agents.png)

## <a name="view-the-cluster-status"></a>Anzeigen des Clusterstatus

Überprüfen Sie die Verbindung mit Ihrem Cluster mithilfe des Befehls [kubectl get](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands), um eine Liste der Clusterknoten zurückzugeben.

```bash
kubectl get nodes
```

## <a name="next-steps"></a>Nächste Schritte

Mit den Schritten in diesem Artikel wurde ein Kubernetes-Cluster auf mehreren Ubuntu-Computern konfiguriert. Der nächste Schritt besteht darin, SQL Server 2019-Big Data-Cluster bereitzustellen. Anweisungen finden Sie im folgenden Artikel:

[Bereitstellungsübersicht](deployment-guidance.md#deploy)
