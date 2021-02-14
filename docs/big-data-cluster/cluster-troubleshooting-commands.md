---
title: Problembehandlung für Kubernetes
titleSuffix: SQL Server big data clusters
description: Dieser Artikel enthält nützliche Befehle zum Überwachen und Behandeln von Problemen eines Big-Data-Clusters für SQL Server 2019.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3f0d8ba02c56ca625094829770771c053e31802e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039640"
---
# <a name="troubleshoot-big-data-clusters-2019-kubernetes"></a>Problembehandlung für [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] auf Kubernetes

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Artikel werden verschiedene nützliche Kubernetes-Befehle beschrieben, die Sie zum Überwachen und Behandeln von Problemen in [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] verwenden können. Sie erfahren zudem, wie Sie ausführliche Informationen zu einem Pod oder anderen Kubernetes-Artefakten erhalten, die sich im Big-Data-Cluster befinden. In diesem Artikel werden auch allgemeine Aufgaben behandelt, z. B. das Kopieren in oder aus einem Container, der einen der folgenden Big-Data-Clusterdienste für SQL Server ausführt.

> [!TIP]
> Für die Überwachung des Status von Big Data-Clusterkomponenten können Sie [**azdata bdc status**](deployment-guidance.md#status)-Befehle oder die integrierten [Notebooks zur Problembehandlung](notebooks-manage-bdc.md) verwenden, die mit Azure Data Studio bereitgestellt werden.

> [!TIP]
> Führen Sie die folgenden **kubectl**-Befehle auf einem Windows- (cmd oder PS) oder einem Linux-(bash)-Clientcomputer aus. Diese müssen zuvor im Cluster authentifiziert und für einen Clusterkontext ausgeführt werden. Für einen zuvor erstellten AKS-Cluster können Sie beispielsweise `az aks get-credentials --name <aks_cluster_name> --resource-group <azure_resource_group_name>` ausführen, um die Konfigurationsdatei des Kubernetes-Clusters herunterzuladen und den Clusterkontext festzulegen.

## <a name="get-status-of-pods"></a>Abrufen der Status von Pods

Mithilfe des `kubectl get pods`-Befehls können Sie die Status von Pods im Cluster entweder für alle Namespaces oder für den Namespace des Big-Data-Clusters abrufen. Die folgenden Abschnitte umfassen Beispiele für beide Möglichkeiten.

### <a name="show-status-of-all-pods-in-the-kubernetes-cluster"></a>Anzeigen der Status aller Pods im Kubernetes-Cluster

Führen Sie den folgenden Befehl aus, um alle Pods und deren Status abzurufen. Dies umfasst auch alle Pods, die Teil des Namespaces sind, in dem Big-Data-Clusterpods für SQL Server erstellt werden:

```bash
kubectl get pods --all-namespaces
```

### <a name="show-status-of-all-pods-in-the-sql-server-big-data-cluster"></a>Anzeigen aller Pods im Big-Data-Cluster für SQL Server

Verwenden Sie den `-n`-Parameter, um einen bestimmten Namespace anzugeben. Beachten Sie, dass Big-Data-Clusterpods für SQL Server in einem neuen Namespace erstellt werden, der zur Clusterbootstrapzeit basierend auf dem in der Konfigurationsdatei der Bereitstellung angegebenen Clusternamen erstellt wird. Der Standardname `mssql-cluster` wird hier verwendet.

```bash
kubectl get pods -n mssql-cluster
```

Ihnen sollte eine Ausgabe ähnlich der folgenden Liste für einen ausgeführten Big-Data-Cluster angezeigt werden:

```output
PS C:\> kubectl get pods -n mssql-cluster
NAME              READY   STATUS    RESTARTS   AGE
appproxy-f2qqt    2/2     Running   0          110m
compute-0-0       3/3     Running   0          110m
control-zlncl     4/4     Running   0          118m
data-0-0          3/3     Running   0          110m
data-0-1          3/3     Running   0          110m
gateway-0         2/2     Running   0          109m
logsdb-0          1/1     Running   0          112m
logsui-jtdnv      1/1     Running   0          112m
master-0          7/7     Running   0          110m
metricsdb-0       1/1     Running   0          112m
metricsdc-shv2f   1/1     Running   0          112m
metricsui-9bcj7   1/1     Running   0          112m
mgmtproxy-x6gcs   2/2     Running   0          112m
nmnode-0-0        1/1     Running   0          110m
storage-0-0       7/7     Running   0          110m
storage-0-1       7/7     Running   0          110m
```

> [!NOTE]
> Während der Bereitstellung werden Pods mit dem **STATUS** **ContainerCreating** immer noch angezeigt. Wenn die Bereitstellung aus irgendeinem Grund hängt, erhalten Sie dadurch möglicherweise einen Eindruck von dem bestehenden Problem. Sehen Sie sich auch die Spalte **READY** (Bereit) an. Diese zeigt an, wie viele Container im Pod gestartet wurden. Beachten Sie, dass Bereitstellungen je nach Konfiguration und Netzwerk 30 Minuten oder länger dauern können. Ein Großteil dieser Zeit wird für das Herunterladen der Containerimages für verschiedene Komponenten aufgewendet.

## <a name="get-pod-details"></a>Abrufen von Poddetails

Führen Sie den folgenden Befehl aus, um eine ausführliche Beschreibung eines bestimmten Pods in einer Ausgabe im JSON-Format zu erhalten. Sie enthält ausführliche Informationen wie den aktuellen Kubernetes-Knoten, auf dem der Pod platziert wurde, die im Pod ausgeführten Container sowie das zum Starten der Container verwendete Image. Zudem werden weitere Informationen wie Bezeichnungen, Status und persistierte Volumeansprüche angezeigt, die dem Pod zugeordnet sind.

```bash
kubectl describe pod  <pod_name> -n <namespace_name>
```

Im folgenden Beispiel werden die ausführlichen Informationen zum Pod `master-0` in einem Big-Data-Cluster mit dem Namen `mssql-cluster` angezeigt:

```bash
kubectl describe pod  master-0 -n mssql-cluster
```

Wenn ein Fehler aufgetreten ist, wird Ihnen dieser manchmal unter den aktuellen Ereignissen des Pods angezeigt.

## <a name="get-pod-logs"></a>Abrufen von Podprotokollen

Sie können die Protokolle für Container abrufen, die in einem Pod ausgeführt werden. Der folgende Befehl ruft die Protokolle für alle Container im Pod `master-0` ab und gibt sie in einer Datei mit dem Namen `master-0-pod-logs.txt` aus:

```bash
kubectl logs master-0 --all-containers=true -n mssql-cluster > master-0-pod-logs.txt
```

## <a name="get-status-of-services"></a><a id="services"></a> Abrufen des Status von Diensten

Führen Sie den folgenden Befehl aus, um ausführliche Informationen zu den Big-Data-Clusterdiensten abzurufen. Diese Informationen umfassen den Typ und die IP-Adressen, die den entsprechenden Diensten und Ports zugeordnet sind. Beachten Sie, dass Big-Data-Clusterdienste für SQL Server in einem neuen Namespace erstellt werden, der zur Clusterbootstrapzeit basierend auf dem in der Konfigurationsdatei der Bereitstellung angegebenen Clusternamen erstellt wird.

```bash
kubectl get svc -n <namespace_name>
```

Im folgenden Beispiel werden die Status der Dienste in einem Big-Data-Cluster mit dem Namen `mssql-cluster` angezeigt:

```bash
kubectl get svc -n mssql-cluster
```

Die folgenden Dienste unterstützen externe Verbindungen mit dem Big-Data-Cluster:

| Dienst | BESCHREIBUNG |
|---|---|
| **master-svc-external** | Bietet Zugriff auf die Masterinstanz.<br/>(**EXTERNAL-IP,31433** und der **SA**-Benutzer) |
| **controller-svc-external** | Unterstützt Tools und Clients, die den Cluster verwalten. |
| **gateway-svc-external** | Bietet Zugriff auf das HDFS/Spark-Gateway.<br/>(**EXTERNAL-IP** und der Benutzer `<AZDATA_USERNAME>`)<sup>1</sup>|
| **appproxy-svc-external** | Unterstützt Anwendungsbereitstellungsszenarios. |

<sup>1</sup> [!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

> [!TIP]
> Dies ist eine Möglichkeit zur Anzeige der Dienste mit **kubectl**, es ist jedoch auch möglich, den Befehl `azdata bdc endpoint list` zur Anzeige dieser Endpunkte zu verwenden. Weitere Informationen finden Sie unter [Get big data cluster endpoints (Abrufen der Endpunkte eines Big-Data-Clusters)](deployment-guidance.md#endpoints).

## <a name="get-service-details"></a>Abrufen von Dienstdetails

Führen Sie den folgenden Befehl aus, um eine ausführliche Beschreibung eines Diensts in einer Ausgabe im JSON-Format zu erhalten. Diese wird ausführliche Informationen wie Bezeichnungen, Selektoren, IP-Adressen, externe IP-Adressen (falls es sich bei dem Dienst um einen Dienst vom Typ LoadBalancer handelt), Ports usw. umfassen.

```bash
kubectl describe service <service_name> -n <namespace_name>
```

Im folgenden Beispiel werden ausführliche Informationen zum Dienst **master-svc-external** abgerufen:

```bash
kubectl describe service master-svc-external -n mssql-cluster
```

## <a name="copy-files"></a><a id="copy"></a> Kopieren von Dateien

Wenn Sie Dateien aus dem Container auf Ihren lokalen Computer kopieren müssen, verwenden Sie den Befehl `kubectl cp` mit der folgenden Syntax:

```bash
kubectl cp <pod_name>:<source_file_path> -c <container_name> -n <namespace_name> <target_local_file_path>
```

Auf ähnliche Weise können Sie `kubectl cp` verwenden, um Dateien vom lokalen Computer in einen bestimmten Container zu kopieren.

```bash
kubectl cp <source_local_file_path> <pod_name>:<target_container_path> -c <container_name>  -n <namespace_name>
```

### <a name="copy-files-from-a-container"></a><a id="copyfrom"></a> Kopieren von Dateien aus einem Container

Im folgenden Beispiel werden die SQL Server-Protokolldateien aus dem Container in den `~/temp/sqlserverlogs`-Pfad auf dem lokalen Computer (in diesem Beispiel handelt es sich bei dem lokalen Computer um einen Linux-Client) kopiert:

```bash
kubectl cp master-0:/var/opt/mssql/log -c mssql-server -n mssql-cluster ~/tmp/sqlserverlogs
```

### <a name="copy-files-into-container"></a><a id="copyinto"></a> Kopieren von Dateien in Container

Im folgenden Beispiel wird die Datei **AdventureWorks2016CTP3.bak** vom lokalen Computer in den Container der SQL Server-Masterinstanz (`mssql-server`) im Pod `master-0` kopiert. Die Datei wird in das `/tmp`-Verzeichnis in den Container kopiert. 

```bash
kubectl cp ~/Downloads/AdventureWorks2016CTP3.bak master-0:/tmp -c mssql-server -n mssql-cluster
```

## <a name="force-delete-a-pod"></a><a id="forcedelete"></a> Erzwingen des Löschens eines Pods
 
Es wird nicht empfohlen, das Löschen eines Pods zu erzwingen. Zum Testen der Verfügbarkeit, Resilienz oder Datenpersistenz können Sie mit dem Befehl `kubectl delete pods` einen Pod löschen, um einen Podfehler zu simulieren.

```bash
kubectl delete pods <pod_name> -n <namespace_name> --grace-period=0 --force
```

Im folgenden Beispiel wird der Speicherpoolpod `storage-0-0` gelöscht:

```bash
kubectl delete pods storage-0-0 -n mssql-cluster --grace-period=0 --force
```

## <a name="get-pod-ip"></a><a id="getip"></a> Abrufen einer Pod-IP-Adresse
 
Für die Problembehandlung müssen Sie möglicherweise die IP-Adresse des Knotens abrufen, auf dem der Pod derzeit ausgeführt wird. Verwenden Sie den Befehl `kubectl get pods` mit der folgenden Syntax, um die IP-Adresse abzurufen:

```bash
kubectl get pods <pod_name> -o yaml -n <namespace_name> | grep hostIP
```

Im folgenden Beispiel wird die IP-Adresse des Knotens abgerufen, auf dem der Pod `master-0` ausgeführt wird:

```bash
kubectl get pods master-0 -o yaml -n mssql-cluster | grep hostIP
```

## <a name="kubernetes-dashboard"></a>Kubernetes-Dashboard

Sie können das Kubernetes-Dashboard starten, um weitere Informationen zum Cluster zu erhalten. In den folgenden Abschnitten wird erläutert, wie das Dashboard für Kubernetes in AKS und für einen gestarteten Kubernetes-Cluster mithilfe von kubeadm gestartet wird.
 
### <a name="start-dashboard-when-cluster-is-running-in-aks"></a>Starten des Dashboards bei Ausführung des Clusters in AKS

Führen Sie Folgendes aus, um das Kubernetes-Dashboard zu starten:

```bash
az aks browse --resource-group <azure_resource_group> --name <aks_cluster_name>
```

> [!Note]
> Möglicherweise wird die folgende Fehlermeldung angezeigt: *Unable to listen on port 8001: All listeners failed to create with the following errors: Unable to create listener: Error listen tcp4 127.0.0.1:8001: >bind: Only one usage of each socket address (protocol/network address/port) is normally permitted. Unable to create listener: Error listen tcp6: address [[::1]]:8001: missing port in >address error: Unable to listen on any of the requested ports: [{8001 9090}]* (Es kann nicht an Port 8001 gelauscht werden: Bei jedem Listener ist ein Fehler aus folgenden beim Erstellen aufgetreten: Fehler beim Erstellen des Listeners: Fehler „listen tcp4 127.0.0.1:8001: >bind“: In der Regel ist nur eine Verwendung jeder Socket-Adresse (Protokoll/Netzwerkadresse/Port) zulässig. Fehler beim Erstellen des Listeners: Fehler „listen tcp6: address [[::1]]:8001: missing port in >address error“: Fehler beim Lauschen an einem der angeforderten Ports: [{8001 9090}])). Stellen Sie in diesem Fall sicher, dass Sie das Dashboard nicht bereits in einem anderen Fenster gestartet haben.

Wenn Sie das Dashboard in Ihrem Browser starten, werden Ihnen möglicherweise Berechtigungswarnungen angezeigt, da die rollenbasierte Zugriffssteuerung in AKS-Clustern standardmäßig aktiviert wird und das vom Dashboard verwendete Dienstkonto nicht über genug Berechtigungen für den Zugriff auf alle Ressourcen verfügt (z. B.: *pods is forbidden: User "system:serviceaccount:kube-system:kubernetes-dashboard" cannot list pods in the namespace "default"* (Pods nicht zulässig: Benutzer „system:serviceaccount:kube-system:kubernetes-dashboard“ kann Pods nicht im Namespace „default“ (Standard) auflisten)). Führen Sie den folgenden Befehl aus, um die erforderlichen Berechtigungen für `kubernetes-dashboard` zu erteilen, und starten Sie anschließend das Dashboard neu:

```bash
kubectl create clusterrolebinding kubernetes-dashboard -n kube-system --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

### <a name="start-dashboard-when-kubernetes-cluster-is-bootstrapped--using-kubeadm"></a>Starten des Dashboards mithilfe von kubeadm bei gestartetem Kubernetes-Cluster

Ausführliche Anweisungen zum Bereitstellen und Konfigurieren des Dashboards in Ihrem Kubernetes-Cluster finden Sie in der [Kubernetes-Dokumentation](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/). Führen Sie folgenden Befehl aus, um das Kubernetes-Dashboard zu starten:

```bash
kubectl proxy
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Big Data-Clustern finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md).
