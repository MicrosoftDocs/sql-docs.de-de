---
title: 'Bereitstellen in Active Directory unter Azure Kubernetes Services: Tutorial'
titleSuffix: SQL Server Big Data Cluster
description: Hier erfahren Sie, wie Sie SQL Server-Big Data-Cluster im AD-Modus in Azure Kubernetes Services (AKS) bereitstellen.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 11/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 760fc7070f0673f61a28cd126be0c6542ac4ee93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039862"
---
# <a name="tutorial-deploy-sql-server-big-data-clusters-in-ad-mode-on-azure-kubernetes-services-aks"></a>Tutorial: Bereitstellen von SQL Server-Big Data-Clustern im AD-Modus in Azure Kubernetes Services

In diesem Artikel wird erläutert, wie Sie einen Big Data-Cluster (BDC) für SQL Server im Active Directory-Authentifizierungsmodus mithilfe einer Referenzarchitektur bereitstellen. Die Referenzarchitektur erweitert Ihre lokale Active Directory Domain Services-Instanz (AD DS) auf Azure. Sie können sie mithilfe von [Azure-Bausteinen](https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks) über das [Azure Architecture Center](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-extend-domain) bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

Vor dem Bereitstellen eines SQL Server-Big Data Clusters müssen die folgenden Voraussetzungen erfüllt sein:

* Greifen Sie auf eine Azure-VM für die Verwaltung zu. Diese VM benötigt Zugriff auf das virtuelle Azure-Netzwerk (VNET), auf dem Sie den BDC bereitstellen. Dieses muss sich entweder im gleichen virtuellen Netzwerk oder auf einem [VNET mit Peering](/azure/virtual-network/virtual-network-manage-peering) befinden.
* [Installieren Sie die Big Data Tools](deploy-big-data-tools.md) auf der Verwaltungs-VM.
* Bereiten Sie die Bereitstellung des Clusters im [AD-Authentifizierungsmodus](active-directory-prerequisites.md) auf Ihrem lokalen AD-Controller vor.

## <a name="create-aks-subnet"></a>Erstellen eines AKS-Subnetzes

1. Festlegen von Umgebungsvariablen

   ```console
   export REGION_NAME=< your Azure Region >
   export RESOURCE_GROUP=<your resource group >
   export SUBNET_NAME=aks-subnet
   export VNet_NAME= adds-vnet
   export AKS_NAME= <your aks cluster name>
   ```

1. Erstellen des AKS-Subnetzes

   ```console
   SUBNET_ID=$(az network vnet subnet show \
    --resource-group $RESOURCE_GROUP \
    --vnet-name $VNet_NAME \
    --name $SUBNET_NAME \
    --query id -o tsv)
   ```

Auf dem folgenden Screenshot sehen Sie einen Plan der Subnetze, die sich im VNET in der Architektur befinden.

:::image type="content" source="media/active-directory-deployment-aks/ad-in-aks-diagram.png" alt-text="AKS-Cluster mit AD und SQL Server-Big Data-Cluster":::

## <a name="create-an-aks-private-cluster"></a>Erstellen eines privaten AKS-Clusters

Sie können den folgenden Befehl verwenden, um einen privaten AKS-Cluster bereitzustellen. Wenn Sie keinen privaten Cluster benötigen, entfernen Sie den Parameter `--enable-private-cluster` aus dem Befehl. Weitere Informationen zu anderen Voraussetzungen finden Sie unter [Bereitstellen eines Azure Kubernetes Service-Clusters (AKS)](/azure/aks/tutorial-kubernetes-deploy-cluster).

```azurecli
az aks create \
    --resource-group $RESOURCE_GROUP \
    --name $AKS_NAME \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id $SUBNET_ID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.3.0.10 \
    --service-cidr 10.3.0.0/24 \
    --node-vm-size Standard_D13_v2 \
    --node-count 2 \
    --generate-ssh-keys
```

[Stellen Sie eine Verbindung mit dem AKS-Cluster her](/azure/aks/tutorial-kubernetes-deploy-cluster#connect-to-cluster-using-kubectl), nachdem Sie ihn bereitgestellt haben.

## <a name="verify-reverse-dns-entry-for-domain-controller"></a>Überprüfen der Reverse-DNS-Einträge für den Domänencontroller

Vergewissern Sie sich vor dem Starten der BDC-Bereitstellung im AD-Modus im AKS-Cluster, dass der Domänencontroller selbst sowohl über einen **A-Eintrag** als auch über einen **PTR-Eintrag** (Reverse-DNS-Eintrag) verfügt, der beim DNS-Server registriert ist.

Führen Sie den Befehl `nslookup` oder [das PowerShell-Skript](troubleshoot-ad-reverse-lookup-zone.md) aus, um zu überprüfen, ob ein Reverse-DNS-Eintrag (PTR-Eintrag) konfiguriert ist.

## <a name="create-bdc-deployment-profile"></a>Erstellen eines BDC-Bereitstellungsprofils

Über den folgenden Befehl können Sie ein Bereitstellungsprofil erstellen:

```console
azdata bdc config init --source kubeadm-prod  --target bdc-ad-aks
```

Die folgenden Befehle werden verwendet, um Parameter für ein Bereitstellungsprofil festzulegen.

### <a name="controljson"></a>control.json

```console
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.storage.data.className=default"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.storage.logs.className=default"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[1].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[0].dnsName=controller.contoso.com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[1].dnsName=proxys.contoso.com"

# security settings 
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.dnsIpAddresses=[\"192.168.0.4\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.domainControllerFullyQualifiedDns=[\"ad1.contoso.com\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.domainDnsName=contoso.com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.clusterUsers=[\"bdcusersgroup\"]"
```

### <a name="bdcjson"></a>bdc.json

```console
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].dnsName=master.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[1].dnsName=mastersec.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[1].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].dnsName=gateway.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].dnsName=approxy.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].serviceType=NodePort"
```

## <a name="initiate-deployment"></a>Initiieren einer Bereitstellung

Über den folgenden Befehl können Sie eine BDC-Bereitstellung initiieren:

```console
azdata bdc create --config-profile bdc-ad-aks --accept-eula yes
```

## <a name="next-steps"></a>Nächste Schritte

[Herstellen einer Verbindung mit einem Big-Data-Cluster für SQL Server mit Azure Data Studio](connect-to-big-data-cluster.md)

[Tutorial: Laden von Beispieldaten in einen Big Data-Cluster für SQL Server](tutorial-load-sample-data.md)