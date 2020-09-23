---
title: Wiederherstellen nach einem Fehler der Failoverclusterinstanz
description: In diesem Artikel erfahren Sie, wie Sie nach einem Failover einer Failoverclusterinstanz über das Failovercluster-Manager-Snap-In eine Wiederherstellung durchführen, nachdem in SQL Server ein Failover aufgetreten ist.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], recovery from failure
- failover clustering [SQL Server], recovery from failure
- hardware failures [SQL Server]
- recovering failover cluster failures [SQL Server]
ms.assetid: 3d151d0c-e841-4325-8606-c094de37d7d1
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 2f9f6dc96c4cc8cf218416b81a429159ad82b558
ms.sourcegitcommit: cc23d8646041336d119b74bf239a6ac305ff3d31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2020
ms.locfileid: "91115766"
---
# <a name="recover-from-failover-cluster-instance-failure"></a>Wiederherstellen nach einem Fehler der Failoverclusterinstanz
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie eine Wiederherstellung nach Clusterfehlern mithilfe des Failovercluster-Manager-Snap-Ins ausgeführt wird, nachdem in [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]ein Failover aufgetreten ist. Das Failovercluster-Manager-Snap-In ist die Clusterverwaltungsanwendung für den WSFC (Windows Server Failover Clustering)-Dienst.  
  
-   [Wiederherstellen nach einem irreparablen Fehler](#Scenario1)  
  
-   [Wiederherstellen nach einem Softwarefehler](#Scenario2)  
  
##  <a name="recover-from-an-irreparable-failure"></a><a name="Scenario1"></a> Wiederherstellen nach einem irreparablen Fehler  
 Führen Sie zur Wiederherstellung nach einem irreparablen Fehler folgende Schritte aus. Der Hardwarefehler kann z. B. durch einen Fehler eines Datenträgercontrollers oder des Betriebssystems verursacht werden. In diesem Fall wird der Fehler durch einen Hardwarefehler in Knoten 1 eines Clusters mit zwei Knoten verursacht.  
  
1.  Nach dem Fehler bei Knoten 1 führt die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -FCI ein Failover auf Knoten 2 aus.  
  
2.  Entfernen Sie Knoten 1 aus der FCI. Öffnen Sie hierzu in Knoten 2 das Failovercluster-Manager-Snap-In, klicken Sie mit der rechten Maustaste auf Knoten 1, klicken Sie auf **Verschiebeaktionen**und anschließend auf **Knoten entfernen**.  
  
3.  Überprüfen Sie, ob Knoten 1 aus der Clusterdefinition entfernt wurde.  
  
4.  Installieren Sie neue Hardware, um die fehlerhafte Hardware in Knoten 1 zu ersetzen.  
  
5.  Fügen Sie mithilfe des Failovercluster-Manager-Snap-Ins Knoten 1 zum vorhandenen Cluster hinzu. Weitere Informationen finden Sie unter [Vor dem Installieren des Failoverclusterings](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md).  
  
6.  Stellen Sie sicher, dass die Administratorkonten für alle Clusterknoten identisch sind.  
  
7.  Führen Sie das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Setup aus, um der FCI Knoten 1 hinzuzufügen. Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen von Knoten in einem SQL Server-Failovercluster &#40;Setup&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
##  <a name="recover-from-a-reparable-failure"></a><a name="Scenario2"></a> Wiederherstellen nach einem behebbaren Fehler  
 Führen Sie zur Wiederherstellung nach einem behebbaren Fehler die folgenden Schritte aus. In diesem Fall wird der Fehler dadurch verursacht, dass Knoten 1 ausgefallen oder offline, aber nicht unwiderruflich fehlerhaft ist. Die Ursache könnte beispielsweise ein Betriebssystem- oder Hardwarefehler oder ein Fehler in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz selbst sein.  
  
1.  Nach dem Fehler bei Knoten 1 führt die FCI ein Failover auf Knoten 2 aus.  
  
2.  Lösen Sie das Problem bei Knoten 1.  
  
3.  Stellen Sie sicher, dass alle Knoten online sind und der WSFC-Dienst aktiviert ist.  
  
4.  Führen Sie für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ein Failover zum wiederhergestellten Knoten aus.  
  
  
