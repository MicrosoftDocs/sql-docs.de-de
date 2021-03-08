---
title: Bereitstellen in Active Directory unter Azure Kubernetes Services
titleSuffix: SQL Server Big Data Cluster
description: Informationen für Konzepte und zur Planung für die Bereitstellung von SQL Server-Big Data-Clustern im AD-Modus in Azure Kubernetes Service
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 11/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a0407589eefab4d5e997d582fee900a20dfcb0a3
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837011"
---
# <a name="deploy-sql-server-big-data-clusters-in-ad-mode-on-azure-kubernetes-services-aks"></a>Bereitstellen von SQL Server-Big Data-Clustern im AD-Modus in Azure Kubernetes Services

SQL Server-Big Data-Cluster unterstützen den Bereitstellungsmodus von [Active Directory (AD)](./active-directory-prerequisites.md) für die **Identitäts- und Zugriffsverwaltung** (Identity & Access Management, IAM). IAM für **Azure Kubernetes Service (AKS)** hat bisher eine Herausforderung dargestellt, da häufig verwendete Protokolle wie OAuth 2.0 und OpenID Connect, die von Microsoft Identity Platform unterstützt werden, nicht von SQL Server unterstützt werden.  

In diesem Artikel wird erläutert, wie Sie einen Big Data-Cluster (BDC) im AD-Modus für Bereitstellungen in [AKS](/azure/aks/intro-kubernetes) bereitstellen. 

## <a name="architecture-topologies"></a>Architekturtopologien

**Active Directory Domain Services (AD DS)** wird auf Azure-VMs [auf dieselbe Weise](/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm) ausgeführt wie auf lokalen Instanzen.  Nachdem Sie die neuen Domänencontroller in Azure heraufgestuft haben, legen Sie die primären und sekundären DNS-Server für das virtuelle Netzwerk fest, und Stufen Sie alle lokalen DNS-Server auf die tertiäre oder eine niedrigere Stufe herab. Die AD-Authentifizierung ermöglicht es in Domänen eingebundenen Clients unter [Linux](../linux/sql-server-linux-active-directory-auth-overview.md), sich mit ihren Domänenanmeldeinformationen und dem Kerberos-Protokoll in SQL Server zu authentifizieren.

Es gibt mehrere Möglichkeiten, einen BDC im AD-Modus in AKS bereitzustellen.  In diesem Artikel werden zwei Methoden vorgestellt, die einfacher zu implementieren sind und mit bestehenden Unternehmensarchitekturen integriert werden können:

* **Erweitern Sie Ihre lokale Active Directory-Domäne auf Azure.** Diese Methode [ermöglicht es Ihrer Active Directory-Umgebung](/azure/architecture/reference-architectures/identity/adds-extend-domain), verteilte Authentifizierungsdienste mithilfe von AD DS in Azure bereitzustellen. Sie replizieren Ihre lokale AD DS-Instanz, um die Latenz zu verringern, die durch das Senden von Authentifizierungsanforderungen aus der Cloud an die lokale AD DS-Instanz verursacht wird. Sie können diese Lösung beispielsweise verwenden, wenn Ihre Anwendung teilweise lokal und teilweise in Azure gehostet wird und Ihre Authentifizierungsanforderungen zwischen beiden Instanzen hin- und hergesendet werden müssen.

   Weitere detaillierte Informationen zum Bereitstellen dieser Lösung finden Sie [in dieser Referenzarchitektur](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-extend-domain).

* **Erweitern Sie Ihre Ressourcengesamtstruktur für AD DS auf Azure.** In dieser Architektur erstellen Sie eine neue Domäne in Azure, die von der lokalen AD-Gesamtstruktur als vertrauenswürdig eingestuft wird. Diese Architektur zeigt eine [unidirektionale Vertrauensstellung zwischen der Domäne in Azure und der lokalen Gesamtstruktur](/azure/architecture/reference-architectures/identity/adds-forest).

   Diese Vertrauensstellung ermöglicht lokalen Benutzern den Zugriff auf Ressourcen in der Domäne in Azure. Weitere detaillierte Informationen zum Bereitstellen dieser Lösung finden Sie [in dieser Referenzarchitektur](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-forest).

Die oben beschriebenen Referenzarchitekturen ermöglichen Ihnen das Erstellen einer Zielzone, in der alle Ressourcen erst von Grund auf neu bereitgestellt werden müssen oder in der je nach vorhandener Architektur eine andere Problemumgehung angewendet werden muss. Zusätzlich zu diesen Referenzarchitekturen sollten Sie BDC in einem AKS-Cluster in einem separaten Subnetz bereitstellen, das in Ihrem Ziel-VNET oder in einem VNET mit Peering verbleibt.

Die folgende Abbildung zeigt eine typische Architektur:

:::image type="content" source="media/active-directory-deployment-aks/ad-in-aks-diagram.png" alt-text="AKS-Cluster mit AD und SQL Server-Big Data-Cluster":::

## <a name="recommendations"></a>Empfehlungen

Die folgenden Empfehlungen gelten für die meisten BDC-Bereitstellungen im AD-Modus in AKS. In jeder Komponente werden die verfügbaren Optionen erwähnt, um eine bessere Integration in die Unternehmensarchitektur zu ermöglichen.

### <a name="networking-recommendations"></a>Netzwerkempfehlungen

Einige wichtige Komponenten können verwendet werden, um Ihre lokale Umgebung mit Azure zu verknüpfen:

* **Azure VPN Gateway**: Ein VPN-Gateway ist eine spezielle Art von Gateway für virtuelle Netzwerke, das verwendet wird, um verschlüsselten Datenverkehr zwischen einem virtuellen Azure-Netzwerk und einem lokalen Standort über das öffentliche Internet zu senden. Sie verwenden sowohl das Azure Virtual Network-Gateway als auch das lokale Gateway für virtuelle Netzwerke. Informationen dazu, wie Sie diese konfigurieren, finden Sie unter [Was ist VPN Gateway?](/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* **Azure ExpressRoute:** ExpressRoute-Verbindungen werden nicht über das öffentliche Internet hergestellt und bieten mehr Sicherheit, Zuverlässigkeit und höhere Geschwindigkeit bei weniger Latenz als herkömmliche Verbindungen über das Internet. Die von Ihnen ausgewählte Konnektivitätsoption wirkt sich je nach SKUs auf die Latenz, Leistung und SLA-Ebene Ihrer Lösung aus. Detaillierte Informationen finden Sie unter [Informationen zu ExpressRoute-Gateways für virtuelle Netzwerke](/azure/expressroute/expressroute-about-virtual-network-gateways).

Die meisten Kunden verwenden eine Jumpbox oder [Azure Bastion](/azure/bastion/bastion-overview), um auf andere Azure-Infrastrukturen zuzugreifen. Mit **Azure Private Link** können Sie über einen privaten Endpunkt in Ihrem virtuellen Netzwerk sicher auf Azure-PaaS-Dienste zugreifen, z. B. AKS in diesem Szenario und andere in Azure gehostete Dienste. Der Datenverkehr zwischen Ihrem virtuellen Netzwerk und dem Dienst wird über das Microsoft-Backbonenetzwerk übertragen und dadurch vom öffentlichen Internet isoliert. Sie können auch Ihren eigenen Private Link-Dienst in Ihrem virtuellen Netzwerk erstellen und Ihren Kunden privat zur Verfügung stellen.

### <a name="active-directory-and-azure-recommendation"></a>Empfehlung für Active Directory und Azure

Lokale AD DS-Instanzen speichern Informationen zu Benutzerkonten und ermöglichen anderen autorisierten Benutzern in demselben Netzwerk den Zugriff auf diese Informationen durch das Authentifizieren von Identitäten, die Benutzern, Computern, Anwendungen oder anderen Ressourcen in einer Sicherheitsgrenze zugeordnet sind. In den meisten Hybridszenarios wird die Benutzerauthentifizierung über eine VPN Gateway- oder eine ExpressRoute-Verbindung mit der lokalen AD DS-Umgebung ausgeführt.  

Bei einer BDC-Bereitstellung im AD-Modus müssen die folgenden Voraussetzungen erfüllt sein, damit die [lokale AD-Instanz mit Azure integriert werden kann](/azure/architecture/reference-architectures/identity/):

* Ein [AD-Konto verfügt über bestimmte Berechtigungen](active-directory-prerequisites.md) zum Erstellen von Benutzern, Gruppen und Computerkonten in der bereitgestellten Organisationseinheit (OE) in Ihrer lokalen AD-Instanz.
* Ein DNS-Server zum [Auflösen von internem DNS](active-directory-dns-reconciliation.md). Es müssen sowohl **A-Einträge (Forwardlookup)** als auch **PTR-Einträge (Reverselookup)** auf dem DNS-Server mit Namen in dieser Domäne enthalten sein. Geben Sie die Domäneneinstellungen für das DNS im BDC-Bereitstellungsprofil an.  

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Bereitstellen von SQL Server-Big Data-Clustern im AD-Modus in Azure Kubernetes Services (AKS)](active-directory-deployment-aks-tutorial.md)