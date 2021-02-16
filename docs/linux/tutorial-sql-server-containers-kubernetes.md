---
title: Bereitstellen eines SQL Server-Containers mit Azure Kubernetes Service (AKS)
description: In diesem Tutorial wird gezeigt, wie Sie eine SQL Server-Hochverfügbarkeitslösung mit Kubernetes in Azure Kubernetes Service bereitstellen.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 09/01/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 53d63320bc72b0c5f0e1891b67fd41b81d46458e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337293"
---
# <a name="deploy-a-sql-server-container-in-kubernetes-with-azure-kubernetes-services-aks"></a>Bereitstellen eines SQL Server-Containers in Kubernetes mit Azure Kubernetes Service

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Erfahren Sie, wie Sie eine SQL Server-Instanz für Kubernetes in AKS (Azure Kubernetes Service) mit persistenten Speicher für Hochverfügbarkeit konfigurieren. Die Lösung stellt Resilienz bereit. Wenn die SQL Server-Instanz fehlschlägt, erstellt Kubernetes sie automatisch in einem neuen Pod neu. Kubernetes bietet auch Resilienz für Knotenausfälle.

In diesem Tutorial wird veranschaulicht, wie eine hochverfügbare SQL Server-Instanz in einem Container in AKS konfiguriert wird.

> [!div class="checklist"]
> * Erstellen eines Systemadministratorkennworts
> * Erstellen des Speichers
> * Erstellen der Bereitstellung
> * Herstellen einer Verbindung mit SSMS (SQL Server Management Studio)
> * Überprüfen von Fehlern und der Wiederherstellung

## <a name="ha-solution-on-kubernetes-running-in-azure-kubernetes-service"></a>Hochverfügbarkeitslösung in Kubernetes in Azure Kubernetes Service

Kubernetes 1.6 und höher unterstützt [Speicherklassen](https://kubernetes.io/docs/concepts/storage/storage-classes/), [PersistentVolumeClaims](https://kubernetes.io/docs/concepts/storage/storage-classes/#persistentvolumeclaims) und den [Volumetyp Azure-Datenträger](https://github.com/kubernetes/examples/tree/master/staging/volumes/azure_disk). Sie können Ihre SQL Server-Instanzen nativ in Kubernetes erstellen und verwalten. Das in diesem Artikel enthaltene Beispiel veranschaulicht die Erstellung einer [Bereitstellung](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/), um eine Hochverfügbarkeitskonfiguration ähnlich einer Failoverclusterinstanz auf einem freigegebenen Datenträger zu erstellen. In dieser Konfiguration spielt Kubernetes die Rolle des Clusterorchestrators. Wenn eine SQL Server-Instanz in einem Container fehlschlägt, startet der Orchestrator eine neues Instanz des Containers, der denselben persistenten Speicher anfügt.

![SQL Server-Container in einem Kubernetes-Cluster](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql.png)

Im obigen Diagramm ist `mssql-server` ein Container in einem [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/). Kubernetes orchestriert die Ressourcen im Cluster. Eine [Replikatgruppe](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) stellt sicher, dass der Pod nach dem Ausfall eines Knotens automatisch wiederhergestellt wird. Anwendungen stellen eine Verbindung zum Dienst her. In diesem Fall stellt der Dienst einen Lastenausgleich dar, der eine IP-Adresse hostet, die sich nach dem Ausfall von `mssql-server` nicht ändert.

Im folgenden Diagramm ist der `mssql-server`-Container fehlgeschlagen. Als Orchestrator garantiert Kubernetes die richtige Anzahl fehlerfreier Instanzen in der Replikatgruppe und startet gemäß der Konfiguration einen neuen Container. Der Orchestrator startet einen neuen Pod auf demselben Knoten, und `mssql-server` stellt erneut eine Verbindung mit demselben persistenten Speicher her. Der Dienst stellt eine Verbindung zur neu erstellen `mssql-server`-Instanz her.

![SQL Server-Podfehler für Kubernetes-Cluster](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql-after-pod-fail.png)

Im folgenden Diagramm ist der Knoten fehlgeschlagen, der den `mssql-server`-Container hostet. Der Orchestrator startet den neuen Pod auf einem anderen Knoten, und `mssql-server` stellt erneut eine Verbindung mit demselben persistenten Speicher her. Der Dienst stellt eine Verbindung zur neu erstellen `mssql-server`-Instanz her.

![SQL Server-Podwiederherstellung im Kubernetes-Cluster](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql-after-node-fail.png)

## <a name="prerequisites"></a>Voraussetzungen

* **Kubernetes-Cluster**
   - Für das Tutorial ist ein Kubernetes-Cluster erforderlich. In den Schritten wird [kubectl](https://kubernetes.io/docs/user-guide/kubectl/) zum Verwalten des Clusters verwendet. 

   - Unter [Bereitstellen eines Azure Kubernetes Service-Clusters (AKS)](/azure/aks/tutorial-kubernetes-deploy-cluster) erfahren Sie, wie Sie einen Kubernetes-Cluster mit einem einzelnen Knoten mithilfe von `kubectl` in AKS erstellen und eine Verbindung mit diesem herstellen. 

   >[!NOTE]
   >Ein Kubernetes-Cluster benötigt mehrere Knoten zum Schutz vor Knotenausfällen.

* **Azure CLI 2.0.23**
   - Die Anweisungen in diesem Tutorial wurden für die Azure CLI 2.0.23 überprüft.

## <a name="create-an-sa-password"></a>Erstellen eines Systemadministratorkennworts

Erstellen Sie ein Systemadministratorkennwort im Kubernetes-Cluster. Kubernetes kann vertrauliche Konfigurationsinformationen wie Kennwörter als [Geheimnisse](https://kubernetes.io/docs/concepts/configuration/secret/) verwalten.

Mit dem folgenden Befehl wird ein Kennwort für das Systemadministratorkonto erstellt:

   ```console
   kubectl create secret generic mssql --from-literal=SA_PASSWORD="MyC0m9l&xP@ssw0rd"
   ```  

   Ersetzen Sie `MyC0m9l&xP@ssw0rd` durch ein komplexes Kennwort.

   Führen Sie den Befehl aus, um ein Geheimnis in Kubernetes namens `mssql` zu erstellen, das den Wert `MyC0m9l&xP@ssw0rd` für `SA_PASSWORD` enthält.


## <a name="create-storage"></a>Erstellen des Speichers

Konfigurieren Sie ein [persistentes Volume](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) und einen [Anspruch auf ein persistentes Volume](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistent-volume-claim-protection) im Kubernetes-Cluster. Führen Sie die folgenden Schritte aus: 

1. Erstellen Sie ein Manifest, um die Speicherklasse und den Anspruch auf ein persistentes Volume zu definieren.  Mit dem Manifest werden der Speicheranbieter, die Parameter und die [reclaim policy](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#reclaiming) (Rückforderungsrichtlinie) festgelegt. Der Kubernetes-Cluster verwendet dieses Manifest, um den persistenten Speicher zu erstellen. 

   Im folgenden YAML-Beispiel werden eine Speicherklasse und ein Anspruch auf persistente Volumes definiert. Der Speicherklassenanbieter lautet `azure-disk`, weil sich dieser Kubernetes-Cluster in Azure befindet. Der Speicherkontotyp lautet `Standard_LRS`. Der Anspruch auf persistente Volumes heißt `mssql-data`. Die Metadaten des Anspruchs auf persistente Volumes enthalten eine Anmerkung, durch die sie auf die Speicherklasse zurückgeführt werden können. 

   ```yaml
   kind: StorageClass
   apiVersion: storage.k8s.io/v1
   metadata:
        name: azure-disk
   provisioner: kubernetes.io/azure-disk
   parameters:
     storageaccounttype: Standard_LRS
     kind: Managed
   ---
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: mssql-data
     annotations:
       volume.beta.kubernetes.io/storage-class: azure-disk
   spec:
     accessModes:
     - ReadWriteOnce
     resources:
       requests:
         storage: 8Gi
   ```

   Speichern Sie die Datei (z. B. **pvc.yaml**).

1. Erstellen Sie einen Anspruch auf ein permanentes Volume in Kubernetes.

   ```console
   kubectl apply -f <Path to pvc.yaml file>
   ```

   `<Path to pvc.yaml file>` entspricht dem Speicherort, an dem Sie die Datei gespeichert haben.

   Das persistente Volume wird automatisch als Azure-Speicherkonto erstellt und an den Anspruch auf ein persistentes Volume gebunden. 

    ![Screenshot: Befehl für Anspruch auf persistentes Volume](media/tutorial-sql-server-containers-kubernetes/02_pvc_cmd.png)

1. Überprüfen Sie den Anspruch auf persistente Volumes.

   ```console
   kubectl describe pvc <PersistentVolumeClaim>
   ```

   `<PersistentVolumeClaim>` ist der Name des Anspruchs auf persistente Volumes.

   Im vorherigen Schritt wurde der Anspruch auf persistente Volumes `mssql-data` genannt. Führen Sie den folgenden Befehl aus, um die Metadaten über den Anspruch auf permanente Volumes anzuzeigen:

   ```console
   kubectl describe pvc mssql-data
   ```

   Die zurückgegebenen Metadaten enthalten einen Wert namens `Volume`. Dieser Wert wird dem Namen des Blobs zugeordnet.

   ![Screenshot: zurückgegebene Metadaten, einschließlich Volume](media/tutorial-sql-server-containers-kubernetes/describe-volume.png)

   Der Wert für Volume entspricht einem Teil des Namens des Blobs in der folgenden Abbildung aus dem Azure-Portal: 

   ![Screenshot: Name des Blobs im Azure-Portal](media/tutorial-sql-server-containers-kubernetes/describe-volume-portal.png)

1. Überprüfen Sie das persistente Volume.

   ```console
   kubectl describe pv
   ```

   `kubectl` gibt Metadaten zum persistenten Volume zurück, die automatisch generiert und an den Anspruch für das persistente Volume gebunden wurden. 

## <a name="create-the-deployment"></a>Erstellen der Bereitstellung

In diesem Beispiel wird der Container, der die SQL Server-Instanz hostet, als Kubernetes-Bereitstellungsobjekt bezeichnet. Die Bereitstellung erstellt eine Replikatgruppe. Die Replikatgruppe erstellt einen Pod. 

In diesem Schritt erstellen Sie ein Manifest, um den Container zu beschreiben, der auf dem Docker-Image der SQL Server-Instanz [mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server) basiert. Das Manifest referenziert den `mssql-server`-Anspruch auf das persistente Volume und das `mssql`-Geheimnis, das Sie bereits auf den Kubernetes-Cluster angewendet haben. Das Manifest beschreibt außerdem einen [Dienst](https://kubernetes.io/docs/concepts/services-networking/service/). Bei diesem Dienst handelt es sich um einen Lastenausgleich. Der Lastenausgleich garantiert, dass die IP-Adresse beibehalten wird, nachdem die SQL Server-Instanz wiederhergestellt wurde. 

1. Erstellen Sie ein Manifest (eine YAML-Datei), um die Bereitstellung zu beschreiben. Im folgenden Beispiel wird eine Bereitstellung beschrieben, die einen Container enthält, der auf dem SQL Server-Containerimage basiert.

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: mssql-deployment
   spec:
     replicas: 1
     selector:
        matchLabels:
          app: mssql
     template:
       metadata:
         labels:
           app: mssql
       spec:
         terminationGracePeriodSeconds: 30
         hostname: mssqlinst
         securityContext:
           fsGroup: 10001
         containers:
         - name: mssql
           image: mcr.microsoft.com/mssql/server:2019-latest
           ports:
           - containerPort: 1433
           env:
           - name: MSSQL_PID
             value: "Developer"
           - name: ACCEPT_EULA
             value: "Y"
           - name: SA_PASSWORD
             valueFrom:
               secretKeyRef:
                 name: mssql
                 key: SA_PASSWORD 
           volumeMounts:
           - name: mssqldb
             mountPath: /var/opt/mssql
         volumes:
         - name: mssqldb
           persistentVolumeClaim:
             claimName: mssql-data
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: mssql-deployment
   spec:
     selector:
       app: mssql
     ports:
       - protocol: TCP
         port: 1433
         targetPort: 1433
     type: LoadBalancer
   ```

   Kopieren Sie den obigen Code in eine neue Datei namens `sqldeployment.yaml`. Ändern Sie die folgenden Werte: 

   * MSSQL_PID `value: "Developer"`: Hiermit wird festgelegt, dass der Container die SQL Server Developer-Edition ausführt. Die Developer-Edition ist nicht für Produktionsdaten lizenziert. Wenn die Bereitstellung für die Produktion vorgesehen ist, legen Sie die entsprechende Edition fest (`Enterprise`, `Standard` oder `Express`). 

      >[!NOTE]
      >Weitere Informationen finden Sie unter [Lizenzierung von SQL Server](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

   * `persistentVolumeClaim`: Dieser Wert erfordert einen Eintrag für `claimName:`, der den verwendeten Namen dem Anspruch für das persistente Volume zuordnet. In diesem Tutorial wird `mssql-data` verwendet. 

   * `name: SA_PASSWORD`: Konfiguriert das Containerimage, um das Systemadministratorkennwort wie in diesem Abschnitt beschrieben festzulegen.

     ```yaml
     valueFrom:
       secretKeyRef:
         name: mssql
         key: SA_PASSWORD
     ```

    Wenn Kubernetes den Container bereitstellt, verweist auf das Geheimnis namens `mssql`, um den Wert für das Kennwort abzurufen.

   * `securityContext` : Ein securityContext definiert Berechtigungs- und Zugriffssteuerungseinstellungen für einen Pod oder Container.In diesem Fall wird er auf Podebene festgelegt, sodass alle Container (in diesem Fall nur ein Container) diesem Sicherheitskontext entsprechen. Im Sicherheitskontext definieren wir fsGroup mit dem Wert 10001 (das ist die GID für mssql group), d. h. alle Prozesse des Containers sind auch Teil der ergänzenden Gruppen-ID 10001(mssql). Der Besitzer des Volumes „/var/opt/mssql“ und aller Dateien, die in diesem Volume erstellt werden, ist die Gruppen-ID 10001(mssql group).

   >[!NOTE]
   >Mithilfe des Diensttyps `LoadBalancer` wird der Remotezugriff auf die SQL Server-Instanz (über das Internet) über Port 1433 ermöglicht.

   Speichern Sie die Datei (z. B. **sqldeployment.yaml**).

1. Erstellen Sie die Bereitstellung.

   ```console
   kubectl apply -f <Path to sqldeployment.yaml file>
   ```

   `<Path to sqldeployment.yaml file>` entspricht dem Speicherort, an dem Sie die Datei gespeichert haben.

   ![Screenshot: Bereitstellungsbefehl](media/tutorial-sql-server-containers-kubernetes/04_deploy_cmd.png)

   Die Bereitstellung und der Dienst werden erstellt. Die SQL Server-Instanz befindet sich in einem Container, der mit dem persistenten Speicher verbunden ist.

   Geben Sie `kubectl get pod` ein, um den Status des Pods anzuzeigen.

   ![Screenshot: Befehl „get pod“](media/tutorial-sql-server-containers-kubernetes/05_get_pod_cmd.png)

   In der vorangehenden Abbildung weist der Pod den Status `Running` auf. Dieser Status gibt an, dass der Container bereit ist. Dieser Vorgang kann einige Minuten dauern.

   >[!NOTE]
   >Nachdem die Bereitstellung erstellt wurde, kann es einige Minuten dauern, bis der Pod sichtbar ist. Die Verzögerung liegt daran, dass der Cluster das Image [mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server) per Pull aus dem Docker-Hub abruft. Nachdem das Image zum ersten Mal per Pull abgerufen wurde, können nachfolgende Bereitstellungen schneller sein, wenn die Bereitstellung einen Knoten nutzt, auf dem bereits ein Image zwischengespeichert wurde. 

1. Überprüfen Sie, ob die Dienste ausgeführt werden. Führen Sie den folgenden Befehl aus:

   ```console
   kubectl get services 
   ```

   Dieser Befehl gibt alle Dienste, die ausgeführt werden, und die internen sowie externen IP-Adressen der Dienste zurück. Beachten Sie die externe IP-Adresse für den `mssql-deployment`-Dienst. Verwenden Sie diese IP-Adresse, um eine Verbindung mit SQL Server herzustellen. 

   ![Screenshot: Befehl „get service“](media/tutorial-sql-server-containers-kubernetes/06_get_service_cmd.png)

   Führen Sie den folgenden Befehl aus, um weitere Informationen zum Status der Objekte im Kubernetes-Cluster zu erhalten:

   ```azurecli
   az aks browse --resource-group <MyResourceGroup> --name <MyKubernetesClustername>
   ```

1. Sie können auch überprüfen, ob der Container als Nicht-Root ausgeführt wird, indem Sie den folgenden Befehl ausführen:

    ```console
    kubectl.exe exec <name of SQL POD> -it -- /bin/bash 
    ```

    Wenn Sie dann „whoami“ ausführen, sollte der Benutzername als „mssql“ angezeigt werden. Also als Nicht-Root-Benutzer.

    ```console
    whoami
    ```

## <a name="connect-to-the-sql-server-instance"></a>Herstellen einer Verbindung mit der SQL Server-Instanz

Wenn Sie den Container gemäß der Beschreibung konfiguriert haben, können Sie eine Verbindung mit einer Anwendung außerhalb des virtuellen Azure-Netzwerks herstellen. Verwenden Sie das `sa`-Konto und die externe IP-Adresse für den Dienst. Verwenden Sie das Kennwort, das Sie als Kubernetes-Geheimnis konfiguriert haben. 

Sie können die folgenden Anwendungen verwenden, um eine Verbindung mit der SQL Server-Instanz herzustellen. 

* [SSMS](./sql-server-linux-manage-ssms.md)

* [SSDT](./sql-server-linux-develop-use-ssdt.md)

* sqlcmd

   Führen Sie den folgenden Befehl aus, um eine Verbindung mit `sqlcmd` herzustellen:

   ```cmd
   sqlcmd -S <External IP Address> -U sa -P "MyC0m9l&xP@ssw0rd"
   ```

   Ersetzen Sie die folgenden Werte:

  * Ersetzen Sie `<External IP Address>` durch die IP-Adresse für den `mssql-deployment`-Dienst 
  * Ersetzen Sie `MyC0m9l&xP@ssw0rd` durch Ihr Kennwort

## <a name="verify-failure-and-recovery"></a>Überprüfen von Fehlern und der Wiederherstellung

Sie können den Pod löschen, um die Fehler und Wiederherstellung zu überprüfen. Führen Sie die folgenden Schritte aus:

1. Listen Sie den Pod auf, der SQL Server ausführt.

   ```console
   kubectl get pods
   ```

   Notieren Sie sich den Namen des Pods, der SQL Server ausführt.

1. Löschen Sie den Pod.

   ```console
   kubectl delete pod mssql-deployment-0
   ```

   `mssql-deployment-0` ist der Wert, der im vorherigen Schritt als Name des Pods zurückgegeben wurde. 

Kubernetes erstellt den Pod automatisch neu, um die SQL Server-Instanz wiederherzustellen und eine Verbindung mit dem persistenten Speicher herzustellen. Verwenden Sie `kubectl get pods`, um zu überprüfen, dass ein neuer Pod bereitgestellt wurde. Verwenden Sie `kubectl get services`, um zu überprüfen, ob die IP-Adresse für den neuen Container gleich ist. 

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie Sie SQL Server-Container in einen Kubernetes-Cluster für Hochverfügbarkeit bereitstellen. 

> [!div class="checklist"]
> * Erstellen eines Systemadministratorkennworts
> * Erstellen des Speichers
> * Erstellen der Bereitstellung
> * Herstellen einer Verbindung mit SSMS (SQL Server Management Studio)
> * Überprüfen von Fehlern und der Wiederherstellung

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
>[Einführung in Kubernetes](/azure/aks/intro-kubernetes)
