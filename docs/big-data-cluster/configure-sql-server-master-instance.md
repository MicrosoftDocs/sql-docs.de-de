---
title: Konfigurieren der SQL Server-Masterinstanz
titleSuffix: Configure SQL Server master instance of Big Data Cluster
description: Konfigurieren Sie die Masterinstanz eines SQL Server-Big Data-Clusters.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 11/04/2019
ms.topic: overview
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: de9bba220f985522926d610b36418d9607c8a568
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039056"
---
# <a name="configure-master-instance-of-big-data-clusters-2019"></a>Konfigurieren der Masterinstanz von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Konfigurieren Sie die Masterinstanz von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].

Die Serverkonfigurationseinstellungen der SQL Server-Masterinstanz können nicht zum Zeitpunkt der Bereitstellung konfiguriert werden. In diesem Artikel wird eine vorübergehende Problemumgehung beschrieben, mit der Einstellungen wie die SQL Server-Edition, das Aktivieren oder Deaktivieren des SQL Server-Agents, das Aktivieren spezifischer Ablaufverfolgungsflags oder das Aktivieren oder Deaktivieren von Kundenfeedback konfiguriert werden können.

Führen Sie die folgenden Schritte aus, um diese Einstellungen zu konfigurieren:

1. Erstellen Sie eine benutzerdefinierte `mssql-custom.conf`-Datei, die die gewünschten Einstellungen enthält. Im folgenden Beispiel werden der SQL Server-Agent, die Telemetrie und das Ablaufverfolgungsflag 1204 aktiviert sowie eine PID für die Enterprise Edition festgelegt:

   ```
   [sqlagent]
   enabled=true
   
   [telemetry]
   customerfeedback=true
   userRequestedLocalAuditDirectory = /tmp/audit

   [DEFAULT]
   pid = Enterprise

   [traceflag]
   traceflag0 = 1204
   ```

1. Kopieren Sie die `mssql-custom.conf`-Datei in `/var/opt/mssql` im `mssql-server`-Container, der sich im `master-0`-Pod befindet. Ersetzen Sie `<namespaceName>` durch den Namen des Big Data-Clusters.

   ```bash
   kubectl cp mssql-custom.conf master-0:/var/opt/mssql/mssql-custom.conf -c mssql-server -n <namespaceName>
   ```

1. Starten Sie die SQL Server-Instanz neu.  Ersetzen Sie `<namespaceName>` durch den Namen des Big Data-Clusters.

   ```bash
   kubectl exec -it master-0  -c mssql-server -n <namespaceName> -- /bin/bash
   supervisorctl restart mssql-server
   exit
   ```

> [!IMPORTANT]
> Wenn die SQL Server-Masterinstanz eine Verfügbarkeitsgruppenkonfiguration aufweist, kopieren Sie die `mssql-custom.conf`-Datei in alle `master`-Pods. Beachten Sie, dass jeder Neustart ein Failover auslöst. Sie müssen also sicherstellen, dass Sie diese Aktivität während Downtime durchführen.

## <a name="known-limitations"></a>Bekannte Einschränkungen

- Für die oben genannten Schritte sind Administratorberechtigungen für den Kubernetes-Cluster erforderlich.
- Sie können die Serversortierung für die SQL Server-Masterinstanz nach der Bereitstellung des Big Data-Clusters nicht ändern.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Bereitstellung von Big Data-Clustern für SQL Server finden Sie unter [Erste Schritte mit Big Data-Clustern für SQL Server](deploy-get-started.md).