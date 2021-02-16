---
title: Manuelles Ausführen eines Failovers für FCI – SQL Server für Linux
description: Erfahren Sie, wie Sie manuell ein Failover für eine Failoverclusterinstanz von SQL Server für Linux ausführen. Die Informationen gelten speziell für Red Hat Linux Enterprise, Ubuntu und Suse Linux Enterprise Server.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 12/06/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 93bff6d536a6c1c5ad4eb8d8805fefb706377594
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346290"
---
# <a name="operate-failover-cluster-instance---sql-server-on-linux"></a>Ausführen einer Failoverclusterinstanz – SQL Server unter Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Artikel wird erläutert, wie Sie eine SQL Server-Failoverclusterinstanz (FCI) unter Linux ausführen. Wenn Sie noch keine SQL Server-FCI unter Linux erstellt haben, lesen Sie [Konfigurieren einer Failoverclusterinstanz – SQL Server für Linux](sql-server-linux-shared-disk-cluster-configure.md). 

## <a name="failover"></a>Failover

Failover für FCIs ähnelt einem Windows Server-Failovercluster (WSFC). Tritt auf dem Clusterknoten, der als Host für die FCI fungiert, ein Fehler auf, sollte die FCI automatisch einen Failover zu einem anderen Knoten ausführen. Anders als bei einem WSFC gibt es keine Möglichkeit, bevorzugte Besitzer festzulegen. Daher wählt Pacemaker den Knoten aus, der als neuer Host für die FCI fungieren soll.

Gelegentlich kann es vorkommen, dass Sie für die FCI manuell ein Failover zu einem anderen Knoten ausführen möchten. Die Vorgehensweise ist nicht mit FCIs in einem WSFC identisch. In einem WSFC führen Sie ein Failover für Ressourcen auf Rollenebene aus. In Pacemaker wählen Sie eine zu verschiebende Ressource aus, und unter der Voraussetzung, dass alle Einschränkungen korrekt sind, wird auch alles andere verschoben. 

Die Art und Weise, wie ein Failover ausgeführt wird, hängt von der Linux-Distribution ab. Gehen Sie entsprechend den die Anweisungen für Ihre Linux-Distribution vor.

- [RHEL oder Ubuntu](#manual-failover-rhel-or-ubuntu)
- [SLES](#manual-failover-sles)

## <a name="manual-failover-rhel-or-ubuntu"></a>Manuelles Failover (RHEL oder Ubuntu)

Führen Sie für ein manuelles Failover auf einem RHEL- (Red Hat Enterprise Linux) oder Ubuntu-Server die folgenden Schritte aus.
1.  Führen Sie den folgenden Befehl aus: 

   ```bash
   sudo pcs resource move <FCIResourceName> <NewHostNode> 
   ```

   \<FCIResourceName> ist der Pacemaker-Ressourcenname für die SQL Server-FCI.

   \<NewHostNode> ist der Name des Clusterknotens, der als Host für die FCI fungieren soll. 

   Sie erhalten keine Bestätigung.

2.  Während eines manuellen Failovers erstellt Pacemaker eine Speicherorteinschränkung für die Ressource, die zur manuellen Verschiebung ausgewählt wurde. Führen Sie `sudo pcs constraint` aus, um diese Einschränkung anzuzeigen.

3.  Nach Abschluss des Failovers entfernen Sie die Einschränkung, indem Sie `sudo pcs resource clear <FCIResourceName>` ausführen. 

\<FCIResourceName> ist der Pacemaker-Ressourcenname für die FCI. 

## <a name="manual-failover-sles"></a>Manuelles Failover (SLES)


Verwenden Sie in SuSE Linux Enterprise Server (SLES) den `migrate`-Befehl, um ein manuelles Failover zu einer SQL Server-FCI auszuführen. Beispiel:

```bash
crm resource migrate <FCIResourceName> <NewHostNode>
```

\<FCIResourceName> ist der Ressourcenname für die Failoverclusterinstanz. 

\<NewHostNode> ist der Name des neuen Zielhosts. 


<!---

|Distribution |Topic 
|----- |-----
|**Red Hat Enterprise Linux with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)<br/>[Operate](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
|**SUSE Linux Enterprise Server with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-sles-configure.md)

--->

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren einer Failoverclusterinstanz – SQL Server für Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
