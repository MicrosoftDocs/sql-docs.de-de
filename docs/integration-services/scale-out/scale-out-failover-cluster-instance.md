---
title: Scale Out-Unterstützung für Hochverfügbarkeit über eine SQL Server-Failoverclusterinstanz | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie SSIS Scale Out für Hochverfügbarkeit mit einer SQL Server-Failoverclusterinstanz konfigurieren.
ms.custom: performance
ms.date: 04/10/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 444226f7e0f28a6f587a5f01b1e512f2392e9916
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351966"
---
# <a name="scale-out-support-for-high-availability-via-sql-server-failover-cluster-instance"></a>Scale Out-Unterstützung für Hochverfügbarkeit über eine SQL Server-Failoverclusterinstanz

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Führen Sie die folgenden Schritte aus, um die Hochverfügbarkeit auf Scale Out-Masterseite mithilfe einer SQL Server-Failoverclusterinstanz einzurichten:

## <a name="1-prerequisites"></a>1. Voraussetzungen
Erstellen Sie einen Windows-Failovercluster. Weitere Anweisungen finden Sie im Blogbeitrag [Installing the Failover Cluster Feature and Tools for Windows Server 2012 (Installation des Failoverclusterfeatures und des Tools für Windows Server 2012)](https://techcommunity.microsoft.com/t5/failover-clustering/installing-the-failover-cluster-feature-and-tools-in-windows/ba-p/371733). Installieren Sie die Funktion und die Tools auf allen Clusterknoten.

## <a name="2-install-sql-server-failover-cluster"></a>2. Installieren eines SQL Server-Failoverclusters
Installieren Sie einen SQL Server-Failovercluster. Weitere Informationen finden Sie unter [SQL Server-Failoverclusterinstallation](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md). Klicken Sie bei der Installation auf der Seite „Featureauswahl“ auf „Database Engine Services“ (Dienste der Datenbank-Engine). Notieren Sie sich den SQL Server-Netzwerknamen für zukünftige Konfigurationen.

![SQL-Netzwerkname](media/sql-network-name.PNG)

Fügen Sie einen Sekundärknoten zu einem vorhandenen SQL Server-Failovercluster hinzu.

## <a name="3-install-scale-out-master-on-the-primary-node"></a>3. Installieren des Scale Out-Masters auf dem Primärknoten
Installieren Sie Integration Services und den Scale Out-Master mithilfe des Setup-Assistenten für die nicht gruppierte Installation auf dem primären Knoten. 

Fügen Sie bei der Installation den SQL Server-Netzwerknamen zum allgemeinen Namen des Scale Out-Masterzertifikats hinzu.

> [!NOTE]
> Wenn Sie für die SSIS-Datenbank und den Scale Out-Master jeweils einen separaten Failover ausführen möchten, führen Sie die unter [2 beschriebenen Anweisungen aus. Installieren Sie den Scale Out-Master für die Konfiguration des Scale Out-Masters auf dem Primärknoten](scale-out-support-for-high-availability.md#2-install-scale-out-master-on-the-primary-node).

## <a name="4-install-scale-out-master-on-the-secondary-node"></a>4. Installieren des Scale Out-Masters auf dem Sekundärknoten
Führen Sie die unter [3 beschriebenen Anweisungen aus. Installieren des Scale Out-Masters auf dem Sekundärknoten](scale-out-support-for-high-availability.md#3-install-scale-out-master-on-the-secondary-node)

## <a name="5-update-the-scale-out-master-service-configuration-file"></a>5. Aktualisieren Sie die Konfigurationsdatei des Scale Out-Masterdiensts
Aktualisieren Sie die Konfigurationsdatei des Scale Out-Masterdiensts (\<drive\>:\Programme\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config) auf dem Primär- und dem Sekundärknoten. Aktualisieren Sie **SqlServerName** für die Standardinstanz auf [den SQL Server-Netzwerknamen]//[den Instanznamen] oder [den SQL Server-Netzwerknamen].

## <a name="6-add-scale-out-master-service-to-sql-server-role-in-windows-failover-cluster"></a>6. Hinzufügen des Scale Out-Masterdiensts zur SQL Server-Rolle im Windows-Failovercluster
Stellen Sie im Failovercluster-Manager eine Verbindung mit dem Cluster für Scale Out her. Wählen Sie im Explorer Rollen aus, klicken Sie mit der rechten Maustaste auf die SQL Server-Rolle, und klicken Sie anschließend auf „Ressource hinzufügen“ > „Allgemeiner Dienst“. 

![Allgemeiner Dienst](media/generic-service.PNG)

Wählen Sie im Assistenten zum Erstellen neuer Ressourcen den Scale Out-Masterdienst aus, und beenden Sie den Assistenten. 

Schalten Sie den Scale Out-Masterdienst online.

![Online schalten](media/bring-online.PNG)

> [!NOTE]
> Wenn Sie für die SSIS-Datenbank und den Scale Out-Master jeweils einen separaten Failover ausführen möchten, führen Sie die unter [7 beschriebenen Anweisungen aus. Konfigurieren der Rolle des Scale Out-Masterdiensts des Windows-Failoverclusters](scale-out-support-for-high-availability.md#7-configure-the-scale-out-master-service-role-of-the-windows-server-failover-cluster)

## <a name="7-install-scale-out-workers"></a>7. Installieren des Scale Out-Workers
Installieren Sie den Scale Out-Worker auf den Workerknoten. Geben Sie bei der Installation für den Masterendpunkt https://[Name_des_SQL_Server_Netzwerks]:[Masterport] an. 

> [!NOTE]
> Wenn Sie für die SSIS-Datenbank und den Scale Out-Master jeweils einen separaten Failover ausführen möchten, geben Sie den DNS-Hostnamen für den Scale Out-Master anstelle des SQL Server-Netzwerknamens an.

## <a name="8-install-scale-out-worker-client-certificate"></a>8. Installieren des Clientzertifikats für den Scale Out-Worker
Installieren Sie das Workerzertifikat auf allen Knoten im SQL Server-Failovercluster. Weitere Informationen finden Sie unter [Installieren des Clientzertifikats für den Scale Out-Worker](walkthrough-set-up-integration-services-scale-out.md#InstallCert).

> [!NOTE]
> Der Scale Out-Manager hat den SQL Server-Failovercluster nicht unterstützt. Wenn Sie den Scale Out-Manager verwenden, um den Scale Out-Worker hinzuzufügen, müssen Sie weiterhin das Workerzertifikat manuell auf allen Masterknoten installieren.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Artikeln:
-   [Scale Out-Master von Integration Services (SSIS)](integration-services-ssis-scale-out-master.md)
-   [Scale Out-Worker von Integration Services (SSIS)](integration-services-ssis-scale-out-worker.md)
