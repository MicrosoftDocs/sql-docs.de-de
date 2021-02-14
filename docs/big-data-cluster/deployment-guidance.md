---
title: Hinweise zur Bereitstellung
titleSuffix: SQL Server Big Data Clusters
description: Erfahren Sie, wie Sie Big Data-Cluster für SQL Server in Kubernetes bereitstellen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d97c5e9b8c508387298cf00a8fbbe96ded1a2eea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047150"
---
# <a name="how-to-deploy-big-data-clusters-2019-on-kubernetes"></a>Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Ein Big Data-Cluster für SQL Server wird als Docker-Container auf einem Kubernetes-Cluster bereitgestellt. Im Folgenden finden Sie eine Übersicht über die Einrichtungs-und Konfigurationsschritte:

- Einrichten eines Kubernetes-Clusters auf einer einzelnen VM, auf einem VM-Cluster oder in Azure Kubernetes Service (AKS), Red Hat OpenShift oder Azure Red Hat OpenShift (ARO)
- Installieren des Clusterkonfigurationstools [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] auf dem Clientcomputer
- Bereitstellen eines Big-Data-Clusters für SQL Server auf einem Kubernetes-Cluster

## <a name="supported-platforms"></a>Unterstützte Plattformen

Eine vollständige Liste der verschiedenen Kubernetes-Plattformen, auf denen SQL Server Big Data-Cluster bereitgestellt werden kann, finden Sie unter [Unterstützte Plattformen](release-notes-big-data-cluster.md#supported-platforms).

### <a name="sql-server-editions"></a>SQL Server-Editionen

|Edition|Notizen|
|---------|---------|
|Enterprise<br/>Standard<br/>Entwickler| Die Edition von Big Data-Clustern wird von der Edition der SQL Server-Masterinstanz bestimmt. Zum Zeitpunkt der Bereitstellung wird standardmäßig die Developer Edition bereitgestellt. Sie können die Edition nach der Bereitstellung ändern. Informationen dazu finden Sie unter [Konfigurieren der SQL Server-Masterinstanz](./configure-sql-server-master-instance.md). |

## <a name="kubernetes"></a><a id="prereqs"></a> Kubernetes

### <a name="kubernetes-cluster-setup"></a><a id="kubernetes"></a> Einrichten des Kubernetes-Clusters

Wenn Sie bereits über einen Kubernetes-Cluster verfügen, der die oben genannten Anforderungen erfüllt, können Sie direkt mit dem [Bereitstellungsschritt](#deploy) fortfahren. In diesem Abschnitt werden grundlegende Kubernetes-Kenntnisse vorausgesetzt.  Ausführliche Informationen finden Sie in der [Kubernetes-Dokumentation](https://kubernetes.io/docs/home).

Sie können Kubernetes auf eine der folgenden Arten bereitstellen:

| Bereitstellung von Kubernetes auf bzw. in: | BESCHREIBUNG | Link |
|---|---|---|
| **Azure Kubernetes Service (AKS)** | Ein Managed Kubernetes-Containerdienst in Azure. | [Anweisungen](deploy-on-aks.md) |
| **Einzelne oder mehrere Computer (`kubeadm`)** | Ein Kubernetes-Cluster, der auf physischen oder virtuellen Computern mithilfe von `kubeadm` bereitgestellt wird. | [Anweisungen](deploy-with-kubeadm.md) |
|**Azure Red Hat OpenShift** | Ein verwaltetes Angebot von OpenShift, das in Azure ausgeführt wird | [Anweisungen](deploy-openshift.md)|
|**Red Hat OpenShift**|Eine Kubernetes-Anwendungsplattform auf Unternehmensniveau mit Hybrid Cloud| [Anweisungen](deploy-openshift.md)|

> [!TIP]
> Sie können auch ein Skript erstellen, mit dem die Bereitstellung von AKS und eines Big-Data-Clusters in einem Schritt ausgeführt wird. Weitere Informationen finden Sie im Artikel zur Verwendung eines [Python-Skripts](quickstart-big-data-cluster-deploy.md) oder im Artikel zur Nutzung eines [Notebooks in Azure Data Studio](notebooks-deploy.md).

### <a name="verify-kubernetes-configuration"></a>Überprüfen der Kubernetes-Konfiguration

Führen Sie den Befehl `kubectl` aus, um sich die Clusterkonfiguration anzeigen zu lassen. Stellen Sie sicher, dass kubectl auf den richtigen Clusterkontext verweist.

```bash
kubectl config view
```

> [!Important] 
> Stellen Sie sicher, dass bei der Bereitstellung in einem Kubernetes-Cluster mit mehreren Knoten, den Sie mithilfe von `kubeadm` gestartet haben, die Uhren auf allen für die Bereitstellung verwendeten Kubernetes-Knoten synchronisiert werden. Der Big Data-Cluster verfügt über integrierte Integritätseigenschaften für verschiedene zeitempfindliche Dienste. Zudem können zeitliche Abweichungen zu einer falschen Statusangabe führen.

Nachdem Sie den Kubernetes-Cluster konfiguriert haben, können Sie mit der Bereitstellung eines neuen Big-Data-Clusters für SQL Server fortfahren. Wenn Sie ein Upgrade von einem früheren Release durchführen, finden Sie weitere Informationen unter [Durchführen eines Upgrades für [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-upgrade.md).

## <a name="ensure-you-have-storage-configured"></a>Sicherstellung von konfiguriertem Speicher

Für die meisten Big Data-Cluster-Bereitstellungen ist persistenter Speicher erforderlich. Derzeit müssen Sie sicherstellen, dass Sie vor der Bereitstellung des Big Data-Clusters ein Plan für die Bereitstellung des persistenten Speichers im Kubernetes-Cluster haben.

Bei einer Bereitstellung in AKS müssen Sie keinen Speicher einrichten. AKS bietet integrierte Speicherklassen mit dynamischer Bereitstellung. Sie können die Speicherklasse (`default` oder `managed-premium`) in der Bereitstellungskonfigurationsdatei anpassen. Für die integrierten Profile wird eine `default`-Speicherklasse verwendet. Wenn Sie eine Bereitstellung auf einem Kubernetes-Cluster durchführen, der mit `kubeadm` bereitgestellt wurde, müssen Sie sicherstellen, dass ausreichend Speicher für einen Cluster des gewünschten Umfangs verfügbar und konfiguriert ist. Wenn Sie die Nutzung Ihres Speichers konfigurieren möchten, sollten Sie das tun, bevor Sie fortfahren. Weitere Informationen finden Sie unter [Datenpersistenz mit SQL Server-Big Data-Clustern in Kubernetes](concept-data-persistence.md).

## <a name="install-sql-server-2019-big-data-tools"></a>Installieren von Big Data-Tools für SQL Server 2019

Bevor Sie einen Big-Data-Cluster für SQL Server 2019 bereitstellen können, müssen Sie zuerst die folgenden [Big-Data-Tools installieren](deploy-big-data-tools.md):

- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]
- `kubectl`
- Azure Data Studio
- [Datenvirtualisierungserweiterung](../azure-data-studio/extensions/data-virtualization-extension.md) für Azure Data Studio


## <a name="deployment-overview"></a><a id="deploy"></a> Übersicht über die Bereitstellung

Die meisten Einstellungen für Big-Data-Cluster werden in einer JSON-Konfigurationsdatei für Bereitstellungen definiert. Sie können ein Standardbereitstellungsprofil für AKS und Kubernetes-Cluster, die mit `kubeadm` erstellt wurden, verwenden. Alternativ können Sie auch eine eigene Konfigurationsdatei für Bereitstellungen während der Einrichtung anpassen. Aus Sicherheitsgründen werden Authentifizierungseinstellungen mithilfe von Umgebungsvariablen übermittelt.

In den folgenden Abschnitten finden Sie weitere Informationen darüber, wie Sie Bereitstellungen von Big-Data-Clustern konfigurieren. Außerdem lernen Sie Beispiele für übliche Anpassungen kennen. Sie können jederzeit die benutzerdefinierte Konfigurationsdatei für Bereitstellungen mit einem Editor wie beispielsweise VS Code bearbeiten.

## <a name="default-configurations"></a><a id="configfile"></a> Standardkonfigurationen

Die Bereitstellungsoptionen für Big-Data-Cluster werden in JSON-Konfigurationsdateien definiert. Sie können mit der Anpassung der Clusterbereitstellung über die integrierten Bereitstellungsprofile beginnen, die in [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]verfügbar sind. 

> [!NOTE]
> Die Containerimages, die für die Bereitstellung im Big Data-Cluster erforderlich sind, werden im `mssql/bdc`-Repository auf Microsoft Container Registry (`mcr.microsoft.com`) gehostet. Diese Einstellungen sind standardmäßig bereits in der `control.json`-Konfigurationsdatei in jedem der Bereitstellungsprofile enthalten, die in [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] enthalten sind. Außerdem ist das Containerimagetag für jedes Release ebenfalls bereits in der gleichen Konfigurationsdatei voreingestellt. Wenn Sie die Containerimages in Ihrer eigenen privaten Containerregistrierung abrufen und/oder die Einstellungen für die Containerregistrierung oder das Containerrepository ändern müssen, befolgen Sie die Anweisungen im Artikel [Offlineinstallation](deploy-offline.md).

Führen Sie diesen Befehl aus, um herauszufinden, welche Vorlagen verfügbar sind:

```
azdata bdc config list -o table 
```

Die folgenden Vorlagen sind ab SQL Server 2019 CU5 verfügbar: 

| Bereitstellungsprofil | Kubernetes-Umgebung |
|---|---|
| `aks-dev-test` | Wird für die Bereitstellung von Big Data-Clustern für SQL Server unter Azure Kubernetes Service (AKS) verwendet.|
| `aks-dev-test-ha` | Wird für die Bereitstellung von Big Data-Clustern für SQL Server unter Azure Kubernetes Service (AKS) verwendet. Unternehmenskritische Dienste wie SQL Server Master und HDFS-Namenknoten sind für Hochverfügbarkeit konfiguriert.|
| `aro-dev-test`|Dieses Profil wird für die Bereitstellung von SQL Server-Big Data-Clustern in Azure Red Hat OpenShift für Entwicklungs- und Testzwecke verwendet. <br/><br/>Es wurde in SQL Server 2019 CU 5 eingeführt.|
| `aro-dev-test-ha`|Dieses Profil wird für die Bereitstellung von SQL Server-Big Data-Clustern mit Hochverfügbarkeit in einem Red Hat OpenShift-Cluster für Entwicklungs- und Testzwecke verwendet. <br/><br/>Es wurde in SQL Server 2019 CU 5 eingeführt.|
| `kubeadm-dev-test` | Wird für die Bereitstellung von Big Data-Clustern für SQL Server auf einem mit kubeadm erstellten Kubernetes-Cluster verwendet, unter Verwendung von einem oder mehreren physischen oder virtuellen Computern.|
| `kubeadm-prod`| Wird für die Bereitstellung von Big Data-Clustern für SQL Server auf einem mit kubeadm erstellten Kubernetes-Cluster verwendet, unter Verwendung von einem oder mehreren physischen oder virtuellen Computern. Diese Vorlage kann für die Integration von Big Data-Clusterdiensten mit Active Directory verwendet werden. Unternehmenskritische Dienste wie die SQL Server-Masterinstanz und HDFS-Namenknoten werden in einer Hochverfügbarkeitskonfiguration bereitgestellt.  |
| `openshift-dev-test`|Dieses Profil wird für die Bereitstellung von SQL Server-Big Data-Clustern in einem Red Hat OpenShift-Cluster für Entwicklungs- und Testzwecke verwendet. <br/><br/>Es wurde in SQL Server 2019 CU 5 eingeführt.|
| `openshift-prod`|Dieses Profil wird für die Bereitstellung von SQL Server-Big Data-Clustern mit Hochverfügbarkeit in einem Red Hat OpenShift-Cluster verwendet. <br/><br/>Es wurde in SQL Server 2019 CU 5 eingeführt.|

Sie können einen Big Data-Cluster bereitstellen, indem Sie `azdata bdc create` ausführen. Dadurch werden Sie aufgefordert, eine der Standardkonfigurationen auszuwählen. Anschließend werden Sie durch die Bereitstellungsschritte geführt.

Wenn Sie zum ersten Mal [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] ausführen, müssen Sie auch `--accept-eula=yes` angeben, um die Lizenzbedingungen zu akzeptieren.

```bash
azdata bdc create --accept-eula=yes
```

In diesem Szenario werden Sie aufgefordert, Informationen wie Kennwörter anzugeben, die nicht Teil der Standardkonfiguration sind. 

> [!IMPORTANT]
> Der Standardname für den Big Data-Cluster ist `mssql-cluster`. Sie benötigen diesen, um `kubectl`-Befehle ausführen zu können, mit denen der Kubernetes-Namespace mithilfe des `-n`-Parameters angeben wird.

## <a name="custom-configurations"></a><a id="customconfig"></a> Benutzerdefinierte Konfigurationen

Es ist auch möglich, die Bereitstellung an die Workloads anzupassen, die Sie ausführen möchten. Die Größe (Anzahl von Replikaten) und Speichereinstellungen für Big Data-Cluster-Dienste können nach der Bereitstellung nicht mehr geändert werden. Daher müssen Sie die Bereitstellungskonfiguration sorgfältig planen, um Kapazitätsprobleme zu vermeiden. Führen Sie die folgenden Schritte aus, um die Bereitstellung anzupassen:

1. Beginnen Sie mit einem Standardbereitstellungsprofil, das für Ihre Kubernetes-Umgebung geeignet ist. Sie können sich alle Profile mit dem Befehl `azdata bdc config list` anzeigen lassen:

   ```bash
   azdata bdc config list
   ```

1. Erstellen Sie eine Kopie des Bereitstellungsprofils mit dem Befehl `azdata bdc config init`, um die Bereitstellung anzupassen. Mit dem folgenden Befehl wird beispielsweise eine Kopie der `aks-dev-test`-Konfigurationsdateien für Bereitstellungen in einem Zielverzeichnis namens `custom` erstellt:

   ```bash
   azdata bdc config init --source aks-dev-test --target custom
   ```

   >[!TIP]
   >Mit `--target` wird ein Verzeichnis angegeben, das in Abhängigkeit des `--source`-Parameters die Konfigurationsdateien `bdc.json` und `control.json` enthält.

1. Sie können die Einstellungen in Ihrem Bereitstellungsprofil anpassen, indem Sie die Konfigurationsdatei für Bereitstellungen in einem Tool bearbeiten. Dieses muss sich zur Bearbeitung von JSON-Dateien eignen. Für diese Aufgabe können Sie beispielweise VS Code verwenden. Zur skriptbasierten Automatisierung können Sie auch das benutzerdefinierte Bereitstellungsprofil mit dem Befehl `azdata bdc config` bearbeiten. Mit dem folgenden Befehl wird z. B. ein benutzerdefiniertes Bereitstellungsprofil angepasst. Dabei wird der Standardname des bereitgestellten Clusters (`mssql-cluster`) in `test-cluster` geändert:  

   ```bash
   azdata bdc config replace --config-file custom/bdc.json --json-values "metadata.name=test-cluster"
   ```

   > [!TIP]
   > Sie können den Clusternamen auch zum Zeitpunkt der Bereitstellung übergeben, indem Sie den Parameter *--name* für den Befehl *azdata create bdc* verwenden. Die Befehlsparameter haben Vorrang vor den Werten in den Konfigurationsdateien.
   >
   > Ein nützliches Tool zum Ermitteln von JSON-Pfaden ist [JSONPath Online Evaluator](https://jsonpath.com/).
   >
   Sie können darin nicht nur Schlüssel-Wert-Paare, sondern auch Inline-JSON-Werte angeben und JSON-Patchdateien übergeben. Weitere Informationen finden Sie unter [Konfigurieren von Bereitstellungseinstellungen für Big-Data-Cluster](deployment-custom-configuration.md).

1. Übergeben Sie dem Befehl `azdata bdc create` die benutzerdefinierte Konfigurationsdatei. Beachten Sie, dass Sie die erforderlichen [Umgebungsvariablen](#env) festlegen müssen. Andernfalls werden Sie vom Terminal aufgefordert, die entsprechenden Werte einzugeben:

   ```bash
   azdata bdc create --config-profile custom --accept-eula yes
   ```

> Weitere Informationen zur Struktur einer Konfigurationsdatei für Bereitstellungen finden Sie in der zugehörigen [Referenzdokumentation](reference-deployment-config.md). Weitere Konfigurationsbeispiele finden Sie unter [Konfigurieren von Bereitstellungseinstellungen für Big-Data-Cluster](deployment-custom-configuration.md).

## <a name="environment-variables"></a><a id="env"></a> Umgebungsvariablen

Die folgenden Umgebungsvariablen werden für Sicherheitseinstellungen verwendet, die nicht in einer Konfigurationsdatei für Bereitstellungen gespeichert werden. Beachten Sie, dass Docker-Einstellungen mit Ausnahme der Anmeldeinformationen in der Konfigurationsdatei festgelegt werden können.

| Umgebungsvariable | Anforderung |BESCHREIBUNG |
|---|---|---|
| `AZDATA_USERNAME` | Erforderlich |Der Benutzername für den Big Data-Clusteradministrator für SQL Server. In der SQL Server-Masterinstanz wird eine SysAdmin-Anmeldung mit dem gleichen Namen erstellt. Als bewährte Sicherheitsmaßnahme wird das `sa`-Konto deaktiviert. <br/><br/>[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]|
| `AZDATA_PASSWORD` | Erforderlich |Das Kennwort für die oben erstellten Benutzerkonten. Bei Clustern, die vor SQL Server 2019 CU5 bereitgestellt wurden, wird dasselbe Kennwort für den Benutzer `root` verwendet, um das Knox Gateway und HDFS zu sichern. |
| `ACCEPT_EULA`| Erforderlich für die erste Verwendung von [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]| Legen Sie diese Einstellung auf „Ja“ fest. Wenn dieser Wert als Umgebungsvariable festgelegt ist, werden die Lizenzbedingungen für SQL Server und [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] akzeptiert. Wenn er nicht als Umgebungsvariable festgelegt ist, können Sie `--accept-eula=yes` angeben, wenn Sie den Befehl [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] zum ersten Mal verwenden.|
| `DOCKER_USERNAME` | Optional | Der Benutzername, mit dem auf Containerimages zugegriffen wird, wenn diese in einem privaten Repository gespeichert sind. Weitere Informationen darüber, wie Sie ein privates Docker-Repository zur Bereitstellung von Big-Data-Clustern nutzen, finden Sie im Artikel [Offlinebereitstellungen](deploy-offline.md).|
| `DOCKER_PASSWORD` | Optional |Das Kennwort, mit dem auf das oben erwähnte private Repository zugegriffen wird. |

Sie müssen diese Umgebungsvariablen festlegen, bevor Sie `azdata bdc create` aufrufen. Wenn eine Variable nicht festgelegt ist, werden Sie aufgefordert, diese anzugeben.

Im folgenden Beispiel wird gezeigt, wie Sie die Umgebungsvariablen für Linux (Bash) und Windows (PowerShell) festlegen:

```bash
export AZDATA_USERNAME=admin
export AZDATA_PASSWORD=<password>
export ACCEPT_EULA=yes
```

```PowerShell
SET AZDATA_USERNAME=admin
SET AZDATA_PASSWORD=<password>
```

> [!NOTE]
> Bei Clustern, die vor SQL Server 2019 CU5 bereitgestellt wurden, müssen Sie den Benutzer `root` für das Knox Gateway mit dem obigen Kennwort verwenden. `root` ist der einzige Benutzer, der in dieser Standardauthentifizierung (Benutzername/Kennwort) unterstützt wird.
> [!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]
> Verwenden Sie dieselben Werte wie die [Umgebungsvariablen](#env) AZDATA_USERNAME und AZDATA_PASSWORD, um über die Standardauthentifizierung eine Verbindung mit SQL Server herzustellen. 

Nachdem Sie die Umgebungsvariablen festgelegt haben, müssen Sie `azdata bdc create` ausführen, um die Bereitstellung auszulösen. Im folgenden Beispiel wird das oben erstellte Clusterkonfigurationsprofil verwendet:

```bash
azdata bdc create --config-profile custom --accept-eula yes
```

Beachten Sie die folgenden Richtlinien:

- Setzen Sie das Kennwort immer in doppelte Anführungszeichen, wenn es Sonderzeichen enthält. Sie können für `AZDATA_PASSWORD` eine beliebige Zeichenfolge festlegen. Das Kennwort muss jedoch ausreichend komplex sein und darf nicht die Zeichen `!`, `&` oder `'` enthalten. Doppelte Anführungszeichen können nur in Bash-Befehlen als Trennzeichen verwendet werden.
- Die `AZDATA_USERNAME`-Anmeldung ist ein Systemadministrator auf der SQL Server-Masterinstanz und wird bei der Einrichtung erstellt. Nach dem Erstellen Ihres SQL Server-Containers wird die von Ihnen festgelegte `AZDATA_PASSWORD` Umgebungsvariable sichtbar, wenn Sie sie in dem Container ausführen`echo $AZDATA_PASSWORD`. Es wird empfohlen, dass Sie das Kennwort aus Sicherheitsgründen ändern.

## <a name="unattended-install"></a><a id="unattended"></a>Unbeaufsichtigtes Installieren

Bei einer unbeaufsichtigten Bereitstellung müssen Sie alle erforderlichen Umgebungsvariablen festlegen, eine Konfigurationsdatei verwenden und den Befehl `azdata bdc create` mit dem `--accept-eula yes`-Parameter aufrufen. Die Beispiele im vorherigen Abschnitt veranschaulichen die Syntax für eine unbeaufsichtigte Installation.

## <a name="monitor-the-deployment"></a><a id="monitor"></a> Überwachen der Bereitstellung

Während des Clusterbootstraps wird der Bereitstellungsstatus im Befehlsfenster des Clients zurückgegeben. Im Verlauf des Bereitstellungsprozesses sollten mehrere Wartemeldungen für den Controllerpod angezeigt werden:

```output
Waiting for cluster controller to start.
```

Nach 15 bis 30 Minuten sollten Sie benachrichtigt werden, dass der Controllerpod ausgeführt wird:

```output
Cluster controller endpoint is available at 11.111.111.11:30080.
Cluster control plane is ready.
```

> [!IMPORTANT]
> Die vollständige Bereitstellung kann einige Zeit in Anspruch nehmen, da die Containerimages für die Komponenten des Big-Data-Clusters heruntergeladen werden müssen. Der Vorgang sollte jedoch nicht mehrere Stunden dauern. Wenn Probleme bei der Bereitstellung auftreten, finden Sie weitere Informationen unter [Überwachung und Problembehandlung: [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](cluster-troubleshooting-commands.md).

Nachdem die Bereitstellung erfolgreich abgeschlossen wurde, wird die folgende Meldung ausgegeben:

```output
Cluster deployed successfully.
```

> [!TIP]
> Der Standardname für den bereitgestellten Big-Data-Cluster ist `mssql-cluster`, falls er nicht durch eine benutzerdefinierte Konfiguration geändert wird.

## <a name="retrieve-endpoints"></a><a id="endpoints"></a> Abrufen von Endpunkten

Nachdem das Bereitstellungsskript erfolgreich ausgeführt wurde, können Sie die Adressen der externen Endpunkte für den Big Data-Cluster mithilfe der folgenden Schritte abrufen.

1. Suchen Sie nach der Bereitstellung die IP-Adresse des Controllerendpunkts in der Standardausgabe. Alternativ können Sie sich auch die Ausgabe des folgenden `kubectl`-Befehls ansehen und die IP-Adresse unter „EXTERNAL-IP“ ablesen:

   ```bash
   kubectl get svc controller-svc-external -n <your-big-data-cluster-name>
   ```

   > [!TIP]
   > Wenn Sie den Standardnamen während der Bereitstellung nicht geändert haben, verwenden Sie im vorherigen Befehl `-n mssql-cluster`. `mssql-cluster` ist der Standardname für den Big Data-Cluster.

1. Melden Sie sich beim Big-Data-Cluster mit [azdata login](../azdata/reference/reference-azdata.md) an. Legen Sie den Parameter `--endpoint` auf die externe IP-Adresse des Controllerendpunkts fest.

   ```bash
   azdata login --endpoint https://<ip-address-of-controller-svc-external>:30080 --username <user-name>
   ```

   Geben Sie den Benutzernamen und das Kennwort an (AZDATA_USERNAME und AZDATA_PASSWORD), die Sie während der Bereitstellung für den Big Data-Clusteradministrator konfiguriert haben.

   > [!TIP]
   > Wenn Sie der Kubernetes-Clusteradministrator sind und Zugriff auf die Clusterkonfigurationsdatei (Kube-Konfigurationsdatei) haben, können Sie den aktuellen Kontext so konfigurieren, dass er auf den Kubernetes-Zielcluster verweist. In diesem Fall können Sie sich mit `azdata login -n <namespaceName>` anmelden, wobei `namespace` der Big Data-Clustername ist. Sie werden zur Eingabe von Anmeldeinformationen aufgefordert, wenn diese im Anmeldebefehl nicht angegeben sind.
   
1. Führen Sie [azdata bdc endpoint list](../azdata/reference/reference-azdata-bdc-endpoint.md) aus, um eine Liste mit Beschreibungen jedes Endpunkts sowie deren entsprechende IP-Adressen und Portwerte abzurufen. 

   ```bash
   azdata bdc endpoint list -o table
   ```

   Die folgende Liste zeigt eine Beispielausgabe dieses Befehls an:

   ```output
   Description                                             Endpoint                                                   Ip              Name               Port    Protocol
   ------------------------------------------------------  ---------------------------------------------------------  --------------  -----------------  ------  ----------
   Gateway to access HDFS files, Spark                     https://11.111.111.111:30443                               11.111.111.111  gateway            30443   https
   Spark Jobs Management and Monitoring Dashboard          https://11.111.111.111:30443/gateway/default/sparkhistory  11.111.111.111  spark-history      30443   https
   Spark Diagnostics and Monitoring Dashboard              https://11.111.111.111:30443/gateway/default/yarn          11.111.111.111  yarn-ui            30443   https
   Application Proxy                                       https://11.111.111.111:30778                               11.111.111.111  app-proxy          30778   https
   Management Proxy                                        https://11.111.111.111:30777                               11.111.111.111  mgmtproxy          30777   https
   Log Search Dashboard                                    https://11.111.111.111:30777/kibana                        11.111.111.111  logsui             30777   https
   Metrics Dashboard                                       https://11.111.111.111:30777/grafana                       11.111.111.111  metricsui          30777   https
   Cluster Management Service                              https://11.111.111.111:30080                               11.111.111.111  controller         30080   https
   SQL Server Master Instance Front-End                    11.111.111.111,31433                                       11.111.111.111  sql-server-master  31433   tcp
   HDFS File System Proxy                                  https://11.111.111.111:30443/gateway/default/webhdfs/v1    11.111.111.111  webhdfs            30443   https
   Proxy for running Spark statements, jobs, applications  https://11.111.111.111:30443/gateway/default/livy/v1       11.111.111.111  livy               30443   https
   ```

Sie können auch alle Dienstendpunkte, die für den Cluster bereitgestellt wurden, durch die Ausführung des folgenden `kubectl`-Befehls abrufen:

```bash
kubectl get svc -n <your-big-data-cluster-name>
```

## <a name="verify-the-cluster-status"></a><a id="status"></a> Überprüfen des Clusterstatus

Nach der Bereitstellung können Sie den Status des Clusters mit dem Befehl [azdata bdc status show](../azdata/reference/reference-azdata-bdc-status.md) überprüfen.

```bash
azdata bdc status show
```

> [!TIP]
> Zum Ausführen der Statusbefehle müssen Sie sich zunächst mit dem Befehl `azdata login` anmelden, der im vorangegangenen Abschnitt zu Endpunkten gezeigt wurde.

Im Folgenden wird eine Beispielausgabe dieses Befehls angezeigt:

```output
Bdc: ready                                                                                                                                                                                                          Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Services: ready                                                                                                                                                                                                     Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Servicename    State    Healthstatus    Details

 sql            ready    healthy         -
 hdfs           ready    healthy         -
 spark          ready    healthy         -
 control        ready    healthy         -
 gateway        ready    healthy         -
 app            ready    healthy         -


 Sql Services: ready                                                                                                                                                                                                 Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 master          ready    healthy         StatefulSet master is healthy
 compute-0       ready    healthy         StatefulSet compute-0 is healthy
 data-0          ready    healthy         StatefulSet data-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy


 Hdfs Services: ready                                                                                                                                                                                                Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 nmnode-0        ready    healthy         StatefulSet nmnode-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy
 sparkhead       ready    healthy         StatefulSet sparkhead is healthy


 Spark Services: ready                                                                                                                                                                                               Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 sparkhead       ready    healthy         StatefulSet sparkhead is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy


 Control Services: ready                                                                                                                                                                                             Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 controldb       ready    healthy         -
 control         ready    healthy         -
 metricsdc       ready    healthy         DaemonSet metricsdc is healthy
 metricsui       ready    healthy         ReplicaSet metricsui is healthy
 metricsdb       ready    healthy         StatefulSet metricsdb is healthy
 logsui          ready    healthy         ReplicaSet logsui is healthy
 logsdb          ready    healthy         StatefulSet logsdb is healthy
 mgmtproxy       ready    healthy         ReplicaSet mgmtproxy is healthy


 Gateway Services: ready                                                                                                                                                                                             Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 gateway         ready    healthy         StatefulSet gateway is healthy


 App Services: ready                                                                                                                                                                                                 Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 appproxy        ready    healthy         ReplicaSet appproxy is healthy
```

Mit den folgenden Befehlen können Sie sich zusätzliche Statusinformationen anzeigen lassen:

- [azdata bdc control status show](../azdata/reference/reference-azdata-bdc-control-status.md) gibt den Integritätsstatus für alle Komponenten zurück, die dem Steuerungsverwaltungsdienst zugeordnet sind.
```
azdata bdc control status show
```
Beispielausgabe:
```output
Control: ready                                                                                                                                                                                                      Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Resources: ready                                                                                                                                                                                                    Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 controldb       ready    healthy         -
 control         ready    healthy         -
 metricsdc       ready    healthy         DaemonSet metricsdc is healthy
 metricsui       ready    healthy         ReplicaSet metricsui is healthy
 metricsdb       ready    healthy         StatefulSet metricsdb is healthy
 logsui          ready    healthy         ReplicaSet logsui is healthy
 logsdb          ready    healthy         StatefulSet logsdb is healthy
 mgmtproxy       ready    healthy         ReplicaSet mgmtproxy is healthy
```

- `azdata bdc sql status show` gibt den Integritätsstatus für alle Ressourcen mit einem SQL Server-Dienst zurück.
```
azdata bdc sql status show
```
Beispielausgabe:
```output
Sql: ready                                                                                                                                                                                                          Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Resources: ready                                                                                                                                                                                                    Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 master          ready    healthy         StatefulSet master is healthy
 compute-0       ready    healthy         StatefulSet compute-0 is healthy
 data-0          ready    healthy         StatefulSet data-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy
```

> [!IMPORTANT]
> Wenn Sie den `--all`-Paramenter verwenden, enthalten die Ausgaben dieser Befehle URLs zu Kibana-und Grafana-Dashboards. Dort ist eine ausführlichere Analyse möglich.

Zusätzlich zu [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] können Sie auch Azure Data Studio verwenden, um Endpunkt- und Statusinformationen zu ermitteln. Weitere Informationen darüber, wie Sie mit [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] und Azure Data Studio den Clusterstatus abrufen können, finden Sie unter [Anzeigen des Status eines Big Data-Clusters](view-cluster-status.md).

## <a name="connect-to-the-cluster"></a><a id="connect"></a> Herstellen einer Verbindung mit dem Cluster

Weitere Informationen darüber, wie Sie eine Verbindung mit dem Big-Data-Cluster herstellen, finden Sie unter [Herstellen einer Verbindung mit einem Big-Data-Cluster für SQL Server über Azure Data Studio](connect-to-big-data-cluster.md).

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln finden Sie weitere Informationen zur Bereitstellung von Big-Data-Clustern:

- [Konfigurieren von Bereitstellungseinstellungen für Big-Data-Cluster](deployment-custom-configuration.md)
- [Durchführen einer Offlinebereitstellung von Big-Data-Clustern für SQL Server](deploy-offline.md)
- [Workshop: Microsoft [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]-Architektur](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)