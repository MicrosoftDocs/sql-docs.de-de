---
title: Offlinebereitstellung
titleSuffix: SQL Server big data clusters
description: Erfahren Sie, wie Sie eine Offlinebereitstellung eines Big Data-Clusters in SQL Server 2019 durchführen und Containerimages in ein privates Repository laden.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9cbe53718818116ae9a0c435a1cf985c132f759b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047276"
---
# <a name="perform-an-offline-deployment-of-a-sql-server-big-data-cluster"></a>Durchführen einer Offlinebereitstellung von Big Data-Clustern für SQL Server

In diesem Artikel wird beschrieben, wie Sie eine Offlinebereitstellung von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] durchführen. Big Data-Cluster müssen Zugriff auf ein Docker-Repository haben, aus dem Containerimages gepullt werden. Bei einer Offlineinstallation werden die erforderlichen Images in einem privaten Docker-Repository abgelegt. Dieses private Repository wird dann als Imagequelle für eine neue Bereitstellung verwendet.

## <a name="prerequisites"></a>Voraussetzungen

- Docker-Engine 1.8 und höher auf unterstütztem Linux-Betriebssystem oder Docker für Mac bzw. Windows. Weitere Informationen finden Sie unter [Install Docker (Installieren von Docker)](https://docs.docker.com/engine/installation/).

## <a name="load-images-into-a-private-repository"></a>Laden von Images in ein privates Repository

In den folgenden Schritten wird beschrieben, wie Sie die Containerimages des Big Data-Clusters aus dem Microsoft-Repository abrufen und anschließend in Ihr privates Repository pushen.

> [!TIP]
> Die folgenden Schritte erläutern den Prozess. Um die Aufgabe zu vereinfachen, können Sie jedoch das [automatisierte Skript](#automated) verwenden, anstatt diese Befehle manuell auszuführen.

1. Pullen Sie die Big Data-Cluster-Containerimages, indem Sie den folgenden Befehl wiederholen. Ersetzen Sie `<SOURCE_IMAGE_NAME>` durch den jeweiligen [Imagenamen](#images). Ersetzen Sie `<SOURCE_DOCKER_TAG>` durch das Tag für das Release des Big Data-Clusters, z. B. **2019-GDR1-ubuntu-16.04**.  

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/bdc/<SOURCE_IMAGE_NAME>:<SOURCE_DOCKER_TAG>
   ```

1. Melden Sie sich bei der privaten Docker-Zielregistrierung an.

   ```PowerShell
   docker login <TARGET_DOCKER_REGISTRY> -u <TARGET_DOCKER_USERNAME> -p <TARGET_DOCKER_PASSWORD>
   ```

1. Markieren Sie die lokalen Images jeweils mit dem folgenden Befehl:

   ```PowerShell
   docker tag mcr.microsoft.com/mssql/bdc/<SOURCE_IMAGE_NAME>:<SOURCE_DOCKER_TAG> <TARGET_DOCKER_REGISTRY>/<TARGET_DOCKER_REPOSITORY>/<SOURCE_IMAGE_NAME>:<TARGET_DOCKER_TAG>
   ```

1. Pushen Sie die lokalen Images in das private Docker-Repository:

   ```PowerShell
   docker push <TARGET_DOCKER_REGISTRY>/<TARGET_DOCKER_REPOSITORY>/<SOURCE_IMAGE_NAME>:<TARGET_DOCKER_TAG>
   ```
 
> [!WARNING]
> Ändern Sie Big Data-Cluster-Images nicht mehr, sobald sie in Ihr privates Repository gepusht wurden. Die Durchführung einer Bereitstellung mit bearbeiteten Images würde zu einem nicht unterstützten Big Data-Cluster-Setup führen.


### <a name="big-data-cluster-container-images"></a><a id="images"></a> Big Data-Cluster-Containerimages

Die folgenden Big Data-Cluster-Containerimages sind für eine Offlineinstallation erforderlich:
- **mssql-app-service-proxy**
- **mssql-control-watchdog**
- **mssql-controller**
- **mssql-dns**
- **mssql-hadoop**
- **mssql-mleap-serving-runtime**
- **mssql-mlserver-py-runtime**
- **mssql-mlserver-r-runtime**
- **mssql-monitor-collectd**
- **mssql-monitor-elasticsearch**
- **mssql-monitor-fluentbit**
- **mssql-monitor-grafana**
- **mssql-monitor-influxdb**
- **mssql-monitor-kibana**
- **mssql-monitor-telegraf**
- **mssql-security-knox**
- **mssql-security-support**
- **mssql-server-controller**
- **mssql-server-data**
- **mssql-ha-operator**
- **mssql-ha-supervisor**
- **mssql-service-proxy**
- **mssql-ssis-app-runtime**


## <a name="automated-script"></a><a id="automated"></a> Automatisiertes Skript

Sie können ein automatisiertes Python-Skript verwenden, das automatisch alle benötigten Containerimages pullt und sie in ein privates Repository überträgt.

> [!NOTE]
> Python ist eine Voraussetzung für die Verwendung des Skripts. Weitere Informationen über die Installation von Python finden Sie in der [Python-Dokumentation](https://wiki.python.org/moin/BeginnersGuide/Download).

1. Laden Sie das Skript per Bash oder PowerShell mit **curl** herunter:

   ```PowerShell
   curl -o push-bdc-images-to-custom-private-repo.py "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/offline/push-bdc-images-to-custom-private-repo.py"
   ```

1. Führen Sie das Skript mit einem der folgenden Befehle aus:

   **Windows:**

   ```PowerShell
   python push-bdc-images-to-custom-private-repo.py
   ```

   **Linux:**

   ```bash
   sudo python push-bdc-images-to-custom-private-repo.py
   ```

1. Befolgen Sie die Anweisungen zum Eingeben des Microsoft-Repositorys und Ihrer privaten Repository-Informationen. Nach Abschluss des Skripts sollten sich alle erforderlichen Images in Ihrem privaten Repository befinden.

1. Befolgen Sie [diese Anweisungen](deployment-custom-configuration.md#docker), um zu erfahren, wie Sie die Konfigurationsdatei für die Bereitstellung von `control.json` anpassen können, um Ihre Containerregistrierung und Ihr Repository zu nutzen. Beachten Sie, dass Sie die Umgebungsvariablen `DOCKER_USERNAME` und `DOCKER_PASSWORD` vor der Bereitstellung festlegen müssen, damit Sie auf das private Repository zugreifen können.

## <a name="install-tools-offline"></a>Offlineinstallation von Tools

Bereitstellungen von Big Data-Clustern erfordern mehrere Tools, einschließlich **Python**, [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]und **kubectl**. Führen Sie die folgenden Schritte aus, um diese Tools auf einem Offlineserver zu installieren.

### <a name="install-python-offline"></a><a id="python"></a> Offlineinstallation von Python

1. Laden Sie auf einem Computer mit Internetzugriff eine der folgenden komprimierten Dateien herunter, die Python enthalten:

   | Betriebssystem | Download |
   |---|---|
   | Windows | [https://go.microsoft.com/fwlink/?linkid=2074021](https://go.microsoft.com/fwlink/?linkid=2074021) |
   | Linux   | [https://go.microsoft.com/fwlink/?linkid=2065975](https://go.microsoft.com/fwlink/?linkid=2065975) |
   | OSX     | [https://go.microsoft.com/fwlink/?linkid=2065976](https://go.microsoft.com/fwlink/?linkid=2065976) |

1. Kopieren Sie die komprimierte Datei auf den Zielcomputer, und extrahieren Sie sie in einen Ordner Ihrer Wahl.

1. Führen Sie `installLocalPythonPackages.bat` nur unter Windows in diesem Ordner aus, und übergeben Sie den vollständigen Pfad zu demselben Ordner als Parameter.

   ```PowerShell
   installLocalPythonPackages.bat "C:\python-3.6.6-win-x64-0.0.1-offline\0.0.1"
   ```

### <a name="install-azdata-offline"></a><a id="azdata"></a> Offlineinstallation von azdata

1. Führen Sie auf einem Computer mit Internetzugriff und [Python](https://wiki.python.org/moin/BeginnersGuide/Download) den folgenden Befehl aus, um alle [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]-Pakete in den aktuellen Ordner herunterzuladen.

   ```PowerShell
   pip download -r https://aka.ms/azdata
   ```

1. Kopieren Sie die heruntergeladenen Pakete und die Datei `requirements.txt` auf den Zielcomputer.

1. Führen Sie den folgenden Befehl auf dem Zielcomputer aus, und geben Sie dabei den Ordner an, in den Sie die vorherigen Dateien kopiert haben.

   ```PowerShell
   pip install --no-index --find-links <path-to-packages> -r <path-to-requirements.txt>
   ```

### <a name="install-kubectl-offline"></a><a id="kubectl"></a> Offlineinstallation von kubectl

Führen Sie die folgenden Schritte aus, um **kubectl** auf einem Offlinecomputer zu installieren.

1. Verwenden Sie **curl**, um **kubectl** in einen Ordner Ihrer Wahl herunterzuladen. Weitere Informationen finden Sie unter [Install kubectl binary using curl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-curl) (Installieren von kubectl mit curl).

1. Kopieren Sie den Ordner auf den Zielcomputer.

## <a name="deploy-from-private-repository"></a>Bereitstellen aus privatem Repository

Verwenden Sie zum Bereitstellen über das private Repository die im [Bereitstellungshandbuch](deployment-guidance.md) beschriebenen Schritte, und verwenden Sie unbedingt eine benutzerdefinierte Bereitstellungskonfigurationsdatei, die die Informationen zu Ihrem privaten Docker-Repository enthält. Die folgenden [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]-Befehle veranschaulichen, wie die Docker-Einstellungen in einer benutzerdefinierten Bereitstellungskonfigurationsdatei namens `control.json` geändert werden:

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.repository=<your-docker-repository>"
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.registry=<your-docker-registry>"
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.imageTag=<your-docker-image-tag>"
```

In der Bereitstellung werden Sie aufgefordert, den Docker-Benutzernamen und das Kennwort einzugeben. Alternativ können Sie sie in den Umgebungsvariablen `DOCKER_USERNAME` und `DOCKER_PASSWORD` angeben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Big Data-Cluster-Bereitstellungen finden Sie unter [Vorgehensweise: Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md).
