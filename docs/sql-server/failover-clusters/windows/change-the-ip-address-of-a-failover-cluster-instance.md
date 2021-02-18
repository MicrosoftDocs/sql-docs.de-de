---
title: Ändern der IP-Adresse einer Failoverclusterinstanz
description: Erfahren Sie, wie Sie die IP-Adresse einer SQL Server-Failoverclusterinstanz mit dem Failovercluster-Manager ändern können.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- modifying IP addresses
- failover clustering [SQL Server], IP addresses
- IP addresses [SQL Server]
- clusters [SQL Server], IP addresses
ms.assetid: b685f400-cbfe-4c5d-a070-227a1123dae4
author: cawrites
ms.author: chadam
ms.openlocfilehash: c9c13206c93ca9a726b067091c130e39826571f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353944"
---
# <a name="change-the-ip-address-of-a-failover-cluster-instance"></a>Ändern der IP-Adresse einer Failoverclusterinstanz
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie die IP-Adressressource in einer Always On-Failoverclusterinstanz (FCI) mithilfe des Failovercluster-Manager-Snap-Ins geändert wird. Das Failovercluster-Manager-Snap-In ist die Clusterverwaltungsanwendung für den WSFC (Windows Server Failover Clustering)-Dienst.  
  
-   **Vorbereitungen:**  [Sicherheit](#Security)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
 Bevor Sie beginnen, lesen Sie sich das folgende Thema in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Onlinedokumentation durch: [Vor dem Installieren des Failoverclusterings](../../../sql-server/failover-clusters/install/before-installing-failover-clustering.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Zum Warten oder Aktualisieren einer FCI müssen Sie ein lokaler Administrator sein und über die Berechtigung verfügen, sich bei allen Knoten der FCI als Dienst anzumelden.  
  
##  <a name="using-the-failover-cluster-manager-snap-in"></a><a name="WSFC"></a> Verwenden des Failovercluster-Manager-Snap-Ins  
 **So ändern Sie die IP-Adressressource für eine FCI**  
  
1.  Öffnen Sie des Failovercluster-Manager-Snap-In.  
  
2.  Erweitern Sie den Knoten **Dienste und Anwendungen** im linken Bereich, und klicken Sie auf die FCI.  
  
3.  Klicken Sie im rechten Bereich unter der Kategorie **Servername** mit der rechten Maustaste auf die SQL Server-Instanz, und wählen Sie die Option **Eigenschaften** , um das Dialogfeld **Eigenschaften** zu öffnen.  
  
4.  Ändern Sie auf der Registerkarte **Allgemein** die IP-Adressressource.  
  
5.  Klicken Sie auf **OK** , um das Dialogfeld zu schließen.  
  
6.  Klicken Sie im rechten Fensterbereich mit der rechten Maustaste auf „SQL IP Address1(Name der Failoverclusterinstanz)“, und wählen Sie **Offline schalten** aus. SQL IP Address1(failover cluster instance name), SQL Network Name(failover cluster instance name) und die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Statusänderung von Online in Ausstehende Offlineschaltung und anschließend Offline werden angezeigt.  
  
7.  Klicken Sie im rechten Fensterbereich mit der rechten Maustaste auf [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], und klicken Sie dann auf **Online schalten**. SQL IP Address1(failover cluster instance name), SQL Network Name(failover cluster instance name) und die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Statusänderung von Offline in Ausstehende Onlineschaltung und anschließend Online werden angezeigt.  
  
8.  Schließen Sie das Failovercluster-Manager-Snap-In.  
  
  
