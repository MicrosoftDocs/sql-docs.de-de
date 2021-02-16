---
title: Anzeigen der Eigenschaften eines Verfügbarkeitsgruppenlisteners
description: Hier wird beschrieben, wie Sie die Eigenschaften eines Always On-Verfügbarkeitsgruppenlisteners in SQL Server mit SQL Server Management Studio, Transact-SQL oder PowerShell anzeigen.
ms.custom: seo-lt-2019
ms.date: 07/11/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygrouplistenerproperties.general.f1
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
ms.assetid: aca0d016-3228-40b8-bdc3-285ed6d9b280
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1f4f4439f8fc931e024ab53cabd92ab5d3d174c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349238"
---
# <a name="view-availability-group-listener-properties-sql-server"></a>Anzeigen von Eigenschaften des Verfügbarkeitsgruppenlisteners (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie die Eigenschaften eines Always On *-Verfügbarkeitsgruppenlisteners* mit [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../../includes/tsql-md.md)] in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]angezeigt werden.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So zeigen Sie die Eigenschaften des Listeners an**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit einer Serverinstanz her, die die Verfügbarkeitsreplikate der Verfügbarkeitsgruppe hostet, deren Listener Sie anzeigen möchten. Klicken Sie auf den Servernamen, um die Serverstruktur zu erweitern.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Erweitern Sie den Knoten der Verfügbarkeitsgruppe, und erweitern Sie den Knoten **Verfügbarkeitsgruppenlistener** .  
  
4.  Klicken Sie mit der rechten Maustaste auf den Listener, den Sie anzeigen möchten, und klicken Sie auf **Eigenschaften** .  
  
5.  Das Dialogfeld **Eigenschaften des Verfügbarkeitsgruppenlisteners** wird geöffnet. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Eigenschaften des Verfügbarkeitsgruppenlisteners (Dialogfeld)](#AgListenerPropertiesDialog).  
  
###  <a name="availability-group-listener-properties-dialog-box"></a><a name="AgListenerPropertiesDialog"></a> Eigenschaften des Verfügbarkeitsgruppenlisteners (Dialogfeld)  
 **DNS-Name des Listeners**  
 Der Netzwerkname des Verfügbarkeitsgruppenlisteners.  
  
 **Port**  
 Der von diesem Listener verwendete TCP-Port.  
  
> [!NOTE]  
>  Wenn Sie mit dem primären Replikat verbunden sind, können Sie in diesem Feld die Portnummer des Listeners ändern. Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.  
  
 **Netzwerkmodus**  
 Gibt das vom Listener verwendete TCP-Protokoll an. Folgende Werte sind möglich:  
  
 **DHCP**  
 Der Listener verwendet eine dynamische IP-Adresse, die von einem Server mit dem Dynamic Host Configuration-Protokoll (DHCP) zugewiesen wird.  
  
 **Statische IP**  
 Der Listener verwendet mindestens eine statische IP-Adresse. Ein Verfügbarkeitsgruppenlistener muss mithilfe von statischen IP-Adressen auf die verschiedenen Subnetze zugreifen.  
  
 Im Raster werden alle Subnetze, auf die der Listener lauscht, und die den einzelnen Subnetzen zugeordneten IP-Adressen angezeigt.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So zeigen Sie die Eigenschaften des Listeners an**  
  
 Um die Verfügbarkeitsgruppenlistener zu überwachen, verwenden Sie die folgenden Sichten:  
  
 [sys.availability_group_listener_ip_addresses](../../../relational-databases/system-catalog-views/sys-availability-group-listener-ip-addresses-transact-sql.md)  
 Gibt eine Zeile für jede konforme virtuelle IP-Adresse zurück, die derzeit für einen Verfügbarkeitsgruppenlistener online ist.  
  
 **Spaltennamen:** listener_id, ip_address, ip_subnet_mask, is_dhcp, network_subnet_ip, network_subnet_prefix_length, network_subnet_ipv4_mask, state, state_desc  
  
 [sys.availability_group_listeners](../../../relational-databases/system-catalog-views/sys-availability-group-listeners-transact-sql.md)  
 Gibt für eine angegebene Verfügbarkeitsgruppe entweder 0 Zeilen zurück, um anzugeben, dass der Verfügbarkeitsgruppe kein Netzwerkname zugeordnet ist, oder eine Zeile für jede Verfügbarkeitsgruppen-Listenerkonfiguration im WSFC-Cluster.  
  
 **Spaltennamen:** group_id, listener_id, dns_name, port, is_conformant, ip_configuration_string_from_cluster  
  
 [sys.dm_tcp_listener_states](../../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md)  
 Gibt eine Zeile zurück, die Informationen zum dynamischen Status für jeden TCP-Listener enthält.  
  
 **Spaltennamen:** listener_id, ip_address, is_ipv4, port, type, type_desc, state, state_desc, start_time  
  
> [!NOTE]  
>  Weitere Informationen zum Verwenden von [!INCLUDE[tsql](../../../includes/tsql-md.md)] zum Überwachen der [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Umgebung finden Sie unter [Überwachen von Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)angezeigt werden.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Entfernen eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Überwachen von Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  
