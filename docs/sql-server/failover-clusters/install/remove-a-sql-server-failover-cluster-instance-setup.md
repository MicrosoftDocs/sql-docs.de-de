---
title: Entfernen einer Failoverclusterinstanz
description: Verwenden Sie dieses Verfahren, um eine Always On-Failoverclusterinstanz zu deinstallieren. Dieser Artikel gibt Ihnen wichtige Hinweise, bevor Sie fortfahren.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], removing failover cluster instance
- failover clustering [SQL Server], removing failover cluster instance
- uninstalling failover cluster instances
- removing failover cluster instances
ms.assetid: bf63353b-69cf-4c5c-98ea-7b151e36537f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2a095fdf70e8190073079944fbbcfa57e8e0ecac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354072"
---
# <a name="remove-a-failover-cluster-instance-setup"></a>Entfernen einer Failoverclusterinstanz (Setup)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Verwenden Sie dieses Verfahren, um eine Always On-[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Failoverclusterinstanz zu deinstallieren.  
  
> [!IMPORTANT]  
>  Um einen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Failovercluster zu aktualisieren oder zu entfernen, müssen Sie ein lokaler Administrator mit der Berechtigung zum Anmelden als Dienst auf allen Knoten des Windows Server-Failoverclusters sein.  
  
 **Voraussetzungen**  
  
 Berücksichtigen Sie die folgenden wichtigen Punkte, bevor Sie eine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Failoverclusterinstanz deinstallieren:  
  
-   Wenn [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client versehentlich deinstalliert wurde, können die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Ressourcen nicht gestartet werden. Um [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client zu installieren, führen Sie das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setupprogramm aus, um die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Voraussetzungen zu installieren.  
  
-   Wenn Sie einen Failovercluster deinstallieren, der über mehr als eine SQL IP-Clusterressource verfügt, müssen Sie die zusätzlichen SQL IP-Ressourcen mithilfe des Failovercluster-Managers oder von PowerShell entfernen.  
  
 Informationen zur Syntax für die Eingabeaufforderung finden Sie unter [Installieren von SQL Server 2016 von der Eingabeaufforderung](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).  
  
### <a name="to-uninstall-a-ssnoversion-failover-cluster-instance"></a>So deinstallieren Sie eine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Failoverclusterinstanz
  
1.  Zur Deinstallation eines [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failovercluster verwenden Sie die vom [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setup bereitgestellte Funktion Knoten entfernen, um jeden Knoten einzeln zu entfernen. Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen von Knoten in einem Always On-Failovercluster &#40;Setup&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Lesen und Anzeigen der Setupprotokolldateien von SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
