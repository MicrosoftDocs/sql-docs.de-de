---
title: Bereitstellungen für Azure Red Hat OpenShift mithilfe eines Python-Skripts
titleSuffix: SQL Server Big Data Clusters
description: In diesem Artikel erfahren Sie, wie Sie mithilfe eines Bereitstellungsskripts Big Data-Cluster in SQL Server für Azure Red Hat OpenShift (ARO) bereitstellen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d53950d277b245b405420af2c28fa7666ba653dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044120"
---
# <a name="use-a-python-script-to-deploy-a-sql-server-big-data-cluster-on-azure-red-hat-openshift-aro"></a>Verwenden eines Python-Skripts zum Bereitstellen eines Big Data-Clusters in SQL Server für Azure Red Hat OpenShift (ARO)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Tutorial verwenden Sie ein Python-Beispielskript, um [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] in [Azure Red Hat OpenShift (ARO)](/azure/virtual-machines/linux/openshift-get-started) bereitzustellen. Diese Bereitstellungsoption wird ab dem kumulativen Update 5 von SQL Server 2019 unterstützt.

> [!TIP]
> ARO ist nur eine Option, mit der Kubernetes für Ihre Big Data-Cluster gehostet werden kann. Informationen zu anderen Bereitstellungsoptionen und zum Anpassen dieser Optionen finden Sie unter [Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Kubernetes](deployment-guidance.md).


> [!WARNING]
> Persistente Volumes, die mit der integrierten Speicherklasse *managed-premium* erstellt wurden, verfügen über eine *Delete*-Rückforderungsrichtlinie. Wenn Sie also den SQL Server-Big Data-Cluster löschen, werden sowohl die persistenten Volumeansprüche als auch die persistenten Volumes gelöscht. Sie sollten mithilfe des Provisioners „azure-disk“ benutzerdefinierte Speicherklassen mit einer *Retain*-Rückforderungsrichtlinie erstellen, wie unter [Speicherklassen](/azure/aks/concepts-storage/#storage-classes) beschrieben. Im folgenden Skript wird die Speicherklasse *managed-premium* verwendet. Ausführlichere Informationen finden Sie unter [Datenpersistenz](concept-data-persistence.md).

Die hier verwendete Standardbereitstellung für Big-Data-Cluster besteht aus einer SQL-Masterinstanz, einer Computepoolinstanz, zwei Datenpoolinstanzen und zwei Speicherpoolinstanzen. Daten werden dauerhaft mit persistenten Kubernetes-Volumes gespeichert, die die Standardspeicherklassen von ARO verwenden. Die Standardkonfiguration, die in diesem Tutorial verwendet wird, eignet sich für Entwicklungs- und Testumgebungen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement.
- [oc](https://docs.openshift.com/container-platform/4.4/cli_reference/openshift_cli/getting-started-cli.html)
- [Python-Mindestversion 3.0](https://www.python.org/downloads)
- [`az` CLI](/cli/azure/install-azure-cli/)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)
- **Azure Data Studio**

## <a name="log-in-to-your-azure-account"></a>Anmelden bei Ihrem Azure-Konto

Das Skript verwendet die Azure CLI, um die Erstellung eines ARO-Clusters zu automatisieren. Sie müssen sich mindestens einmal bei Ihrem Azure-Konto mit der Azure CLI anmelden, bevor Sie das Skript ausführen. Geben Sie über eine Eingabeaufforderung den folgenden Befehl ein:

```azurecli
az login
```

## <a name="instructions"></a>Instructions

1. Laden Sie sowohl das Python-Skript `deploy-sql-big-data-aro.py` als auch die YAML-Datei `bdc-scc.yaml` herunter.

   >Diese Dateien finden Sie in diesem Artikel unter:
   - [`deploy-sql-big-data-aro.py`](#deploy-sql-big-data-aropy)
   - [`bdc-scc.yaml`](#bdc-sccyaml)

1. Führen Sie das Skript folgendermaßen aus:

```console
python deploy-sql-big-data-aro.py
```

Geben Sie bei Aufforderung Ihre Angabe für die Abonnement-ID in Azure und die Azure-Ressourcengruppe ein, in der die Ressourcen erstellt werden sollen. Optional können Sie auch eine Eingabe für andere Konfigurationen machen oder die angegebenen Standardwerte übernehmen. Beispiel:

- `azure_region`
- `vm_size` für OpenShift-Workerknoten. Eine optimale Servicequalität können Sie beim Überprüfen grundlegender Szenarios erzielen, wenn Sie mindestens acht vCPUs und 64 GB Arbeitsspeicher für alle Workerknoten des Clusters verwenden. Das Skript verwendet standardmäßig `Standard_D8s_v3` und drei Workerknoten. Für eine Standardgrößenkonfiguration für Big Data-Cluster werden außerdem 24 Datenträger für Ansprüche auf persistente Volumes für alle Komponenten verwendet.
- „network configuration“ für OpenShift-Clusterbereitstellungen. Weitere Informationen zu den einzelnen Parametern finden Sie im Artikel zur [ARO-Bereitstellung](\azure\openshift\tutorial-create-cluster).
- `cluster_name`: Dieser Wert wird sowohl für ARO-Cluster als auch für Big Data-Cluster in SQL Server verwendet, die zusätzlich zu ARO erstellt werden. Beachten Sie, dass es sich beim Namen des Big Data-Clusters in SQL Server um einen Kubernetes-Namespace handelt.
- `username `: Hierbei handelt es sich um den Benutzername für die während der Bereitstellung für das Administratorkonto des Controllers, das Konto der SQL Server-Masterinstanz und das Gateway bereitgestellten Konten. Beachten Sie, dass das `sa`-Konto in SQL Server für Sie automatisch deaktiviert wird, da dies als Best Practice gilt.
- `password`: Derselbe Wert wird für alle Konten genutzt.

Der Big Data-Cluster in SQL Server wird nun für ARO bereitgestellt. Nun können Sie Azure Data Studio verwenden, um eine Verbindung mit dem Cluster herzustellen. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einem Big-Data-Cluster für SQL Server mithilfe von Azure Data Studio](connect-to-big-data-cluster.md).

## <a name="clean-up"></a>Bereinigung

Wenn Sie [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] in Azure testen, sollten Sie abschließend den ARO-Cluster löschen, um unerwartete Gebühren zu vermeiden. Entfernen Sie den Cluster nicht, wenn Sie ihn weiterhin verwenden möchten.

> [!WARNING]
> Wenn Sie die folgenden Schritte ausführen, werden der ARO-Cluster und die Big Data-Cluster für SQL Server gelöscht. Wenn Sie über Datenbanken oder HDFS-Daten verfügen, die Sie beibehalten möchten, müssen Sie diese Daten sichern, bevor Sie den Cluster löschen.

Führen Sie den folgenden Azure CLI-Befehl aus, um den Big Data-Cluster und den ARO-Dienst in Azure zu entfernen. Ersetzen Sie dabei `<resource group name>` durch die **Azure-Ressourcengruppe**, die Sie im Bereitstellungsskript angegeben haben:

```azurecli
az group delete -n <resource group name>
```

## `deploy-sql-big-data-aro.py` 

Das Skript in diesem Abschnitt stellt den Big Data-Cluster in SQL Server für Azure Red Hat OpenShift bereit. Kopieren Sie das Skript in Ihre Arbeitsstation, und speichern Sie es als `deploy-sql-big-data-aro.py`, bevor Sie mit der Bereitstellung beginnen.

```python
#
# Prerequisites: 
# 
# Azure CLI (https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), Azure Data CLI (`azdata`) (https://docs.microsoft.com/en-us/sql/big-data-cluster/deploy-install-azdata), oc CLI (https://www.openshift.com/blog/installing-oc-tools-windows)
#
# Run `az login` at least once BEFORE running this script
#

from subprocess import check_output, CalledProcessError, STDOUT, Popen, PIPE, getoutput
from time import sleep
import os
import getpass
import json

def executeCmd (cmd):
    if os.name=="nt":
        process = Popen(cmd.split(),stdin=PIPE, shell=True)
    else:
        process = Popen(cmd.split(),stdin=PIPE)
    stdout, stderr = process.communicate()
    if (stderr is not None):
        raise Exception(stderr)

#
# MUST INPUT THESE VALUES!!!!!
#
SUBSCRIPTION_ID = input("Provide your Azure subscription ID:").strip()
GROUP_NAME = input("Provide Azure resource group name to be created:").strip()
#
# This password will be use for Controller user, Gateway user and SQL Server Master SA accounts
AZDATA_USERNAME=input("Provide username to be used for Controller, SQL Server and Gateway endpoints - Press ENTER for using  `admin`:").strip() or "admin"
AZDATA_PASSWORD = getpass.getpass("Provide password to be used for Controller user, Gateway user and SQL Server Master accounts:")
#
# Optionally change these configuration settings
#
AZURE_REGION=input("Provide Azure region - Press ENTER for using `westus2`:").strip() or "westus2"
# MASTER_VM_SIZE=input("Provide VM size for master nodes for the ARO cluster - Press ENTER for using  `Standard_D2s_v3`:").strip() or "Standard_D2s_v3"
WORKER_VM_SIZE=input("Provide VM size for the worker nodes for the ARO cluster - Press ENTER for using  `Standard_D8s_v3`:").strip() or "Standard_D8s_v3"
OC_NODE_COUNT=input("Provide number of worker nodes for ARO cluster - Press ENTER for using  `3`:").strip() or "3"
VNET_NAME=input("Provide name of Virtual Network for ARO cluster - Press ENTER for using  `aro-vnet`:").strip() or "aro-vnet"
VNET_ADDRESS_SPACE=input("Provide Virtual Network Address Space for ARO cluster - Press ENTER for using  `10.0.0.0/16`:").strip() or "10.0.0.0/16"
MASTER_SUBNET_NAME=input("Provide Master Subnet Name for ARO cluster - Press ENTER for using  `master-subnet`:").strip() or "master-subnet"
MASTER_SUBNET_IP_RANGE=input("Provide address range of Master Subnet for ARO cluster - Press ENTER for using  `10.0.0.0/23`:").strip() or "10.0.0.0/23"
WORKER_SUBNET_NAME=input("Provide Worker Subnet Name for ARO cluster - Press ENTER for using  `worker-subnet`:").strip() or "worker-subnet"
WORKER_SUBNET_IP_RANGE=input("Provide address range of Worker Subnet for ARO cluster - Press ENTER for using  `10.0.2.0/23`:").strip() or "10.0.2.0/23"
#
# This is both Kubernetes cluster name and SQL Big Data cluster name
CLUSTER_NAME=input("Provide name of OpenShift cluster and SQL big data cluster - Press ENTER for using  `sqlbigdata`:").strip() or "sqlbigdata"
#
# Deploy the ARO cluster
#  
print ("Set azure context to subscription: "+SUBSCRIPTION_ID)
command = "az account set -s "+ SUBSCRIPTION_ID
executeCmd (command)
print ("Creating azure resource group: "+GROUP_NAME)
command="az group create --name "+GROUP_NAME+" --location "+AZURE_REGION
executeCmd (command)
command = "az network vnet create --resource-group "+GROUP_NAME+" --name "+VNET_NAME+" --address-prefixes "+VNET_ADDRESS_SPACE
print("Creating Virtual Network: "+VNET_NAME)
executeCmd(command)
command = "az network vnet subnet create --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --name "+MASTER_SUBNET_NAME+" --address-prefixes "+MASTER_SUBNET_IP_RANGE+" --service-endpoints Microsoft.ContainerRegistry"
print("Creating Master Subnet: "+MASTER_SUBNET_NAME)
executeCmd(command)
command = "az network vnet subnet create --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --name "+WORKER_SUBNET_NAME+" --address-prefixes "+WORKER_SUBNET_IP_RANGE+" --service-endpoints Microsoft.ContainerRegistry"
print("Creating Worker Subnet: "+WORKER_SUBNET_NAME)
executeCmd(command)
command = "az network vnet subnet update --name "+MASTER_SUBNET_NAME+" --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --disable-private-link-service-network-policies true"
print("Updating Master Subnet by disabling Private Link Policies")
executeCmd(command)
command = "az aro create --resource-group "+GROUP_NAME+" --name "+CLUSTER_NAME+" --vnet "+VNET_NAME+" --master-subnet "+MASTER_SUBNET_NAME+" --worker-subnet "+WORKER_SUBNET_NAME+" --worker-count "+OC_NODE_COUNT+" --worker-vm-size "+WORKER_VM_SIZE +" --only-show-errors"
print("Creating OpenShift cluster: "+CLUSTER_NAME)
executeCmd (command)
#
# Login to oc console
#
command = "az aro list-credentials --name "+CLUSTER_NAME+" --resource-group "+GROUP_NAME +" --only-show-errors"
output=json.loads(getoutput(command))
OC_CLUSTER_USERNAME = str(output['kubeadminUsername'])
OC_CLUSTER_PASSWORD = str(output['kubeadminPassword'])
command = "az aro show --name "+CLUSTER_NAME+" --resource-group "+GROUP_NAME +" --only-show-errors"
output=json.loads(getoutput(command))
APISERVER = str(output['apiserverProfile']['url'])
command = "oc login "+ APISERVER+ " -u " + OC_CLUSTER_USERNAME + " -p "+ OC_CLUSTER_PASSWORD
executeCmd (command)
#
# Setup pre-requisites for deploying BDC on OpenShift
#
#
#Creating new project/namespace
command = "oc new-project "+ CLUSTER_NAME
executeCmd (command)
#
# create custom SCC for BDC
command = "oc apply -f bdc-scc.yaml"
executeCmd (command)
#
#Adding the custom scc to BDC namespace
command = "oc adm policy add-scc-to-group bdc-scc system:serviceaccounts:" + CLUSTER_NAME
executeCmd (command)
#
# Deploy big data cluster
#
print ('Set environment variables for credentials')
os.environ['AZDATA_PASSWORD'] = AZDATA_PASSWORD
os.environ['AZDATA_USERNAME'] = AZDATA_USERNAME
os.environ['ACCEPT_EULA']="Yes"
#
#Creating azdata configuration with aro-dev-test profile
command = "azdata bdc config init --source aro-dev-test --target custom --force"
executeCmd (command)
command="azdata bdc config replace -c custom/bdc.json -j ""metadata.name=" + CLUSTER_NAME + ""
executeCmd (command)
#
# Create BDC
command = "azdata bdc create --config-profile custom --accept-eula yes"
executeCmd(command)
#login into BDC cluster and list endpoints
command="azdata login -n " + CLUSTER_NAME
executeCmd (command)
print("")
print("SQL Server big data cluster endpoints: ")
command="azdata bdc endpoint list -o table"
executeCmd(command)
```

## `bdc-scc.yaml`

Das folgende YAML-Manifest definiert eine benutzerdefinierte Einschränkung für Sicherheitskontexte für die Big Data-Cluster-Bereitstellung. Kopieren Sie es in dasselbe Verzeichnis wie `deploy-sql-big-data-aro.py`.

```yaml
allowHostDirVolumePlugin: false
allowHostIPC: false
allowHostNetwork: false
allowHostPID: false
allowHostPorts: false
allowPrivilegeEscalation: true
allowPrivilegedContainer: false
allowedCapabilities:
  - SETUID
  - SETGID
  - CHOWN
  - SYS_PTRACE
apiVersion: security.openshift.io/v1
defaultAddCapabilities: null
fsGroup:
  type: RunAsAny
kind: SecurityContextConstraints
metadata:
  annotations:
    kubernetes.io/description: SQL Server BDC custom scc is based on 'nonroot' scc plus additional capabilities required by BDC.
  generation: 2
  name: bdc-scc
readOnlyRootFilesystem: false
requiredDropCapabilities:
  - KILL
  - MKNOD
runAsUser:
  type: MustRunAsNonRoot
seLinuxContext:
  type: MustRunAs
supplementalGroups:
  type: RunAsAny
volumes:
  - configMap
  - downwardAPI
  - emptyDir
  - persistentVolumeClaim
  - projected
  - secret
```

## <a name="next-steps"></a>Nächste Schritte

Durch das Bereitstellungsskript wurden Azure Kubernetes Service konfiguriert und ein Big-Data-Cluster für SQL Server 2019 bereitgestellt. Sie können zukünftige Bereitstellungen auch mithilfe manueller Installationen anpassen. Weitere Informationen darüber, wie Sie Big Data-Cluster bereitstellen und Bereitstellungen anpassen, finden Sie unter [Bereitstellen von ](deployment-guidance.md) in Kubernetes[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].

Da der Big-Data-Cluster für SQL Server bereitgestellt wurde, können Sie nun Beispieldaten auf diesen hochladen und sich die zugehörigen Tutorials ansehen:

> [!div class="nextstepaction"]
> [Tutorial: Hochladen von Beispieldaten auf einen Big-Data-Cluster für SQL Server 2019](tutorial-load-sample-data.md)