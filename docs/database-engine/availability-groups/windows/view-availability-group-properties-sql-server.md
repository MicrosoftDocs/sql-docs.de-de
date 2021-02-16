---
title: Anzeigen von Verfügbarkeitsgruppeneigenschaften
description: 'Zeigen Sie die Eigenschaften einer Always On-Verfügbarkeitsgruppe mit SQL Server Management Studio (SSMS), Transact-SQL (T-SQL) oder SQL PowerShell an. '
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server]
ms.assetid: 61243c87-bd62-4510-863f-2a8f347caf1f
author: cawrites
ms.author: chadam
ms.openlocfilehash: d7c82b6f567872a32615a21b49a42007fbfca7f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350753"
---
# <a name="view-availability-group-properties-sql-server"></a>Anzeigen von Verfügbarkeitsgruppeneigenschaften (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie die Eigenschaften einer Verfügbarkeitsgruppe für eine AlwaysOn-Verfügbarkeitsgruppe unter Verwendung von [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../../includes/tsql-md.md)] in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]angezeigt werden.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So zeigen Sie die Eigenschaften einer Verfügbarkeitsgruppe an und ändern sie**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die das primäre Verfügbarkeitsreplikat hostet, und erweitern Sie die Serverstruktur.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Klicken Sie mit der rechten Maustaste auf die Verfügbarkeitsgruppe, deren Eigenschaften Sie anzeigen möchten, und wählen Sie den Befehl **Eigenschaften** aus.  
  
4.  Verwenden Sie im Dialogfeld **Eigenschaften der Verfügbarkeitsgruppe** die Seiten **Allgemein** und **Sicherungseinstellungen** , um Eigenschaften der ausgewählten Verfügbarkeitsgruppe anzuzeigen und in einigen Fällen zu ändern. Weitere Informationen finden Sie unter [Eigenschaften der Verfügbarkeitsgruppe: Neue Verfügbarkeitsgruppe &#40;Seite „Allgemein“&#41;](../../../database-engine/availability-groups/windows/availability-group-properties-new-availability-group-general-page.md) und [Eigenschaften der Verfügbarkeitsgruppe: Neue Verfügbarkeitsgruppe &#40;Seite „Sicherungseinstellungen&#41;](../../../database-engine/availability-groups/windows/availability-group-properties-new-availability-group-backup-preferences-page.md).  
  
     Verwenden Sie die Seite **Berechtigungen** , um die aktuellen Anmeldungen, Rollen und der Verfügbarkeitsgruppe zugeordneten expliziten Berechtigungen anzuzeigen. Weitere Informationen finden Sie unter [Permissions or Securables Page](../../../relational-databases/security/permissions-or-securables-page.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So zeigen Sie die Eigenschaften und den Status einer Verfügbarkeitsgruppe an**  
  
 Verwenden Sie zum Abfragen der Eigenschaften und des Status der Verfügbarkeitsgruppen, für die die Serverinstanz ein Verfügbarkeitsreplikat hostet, die folgenden Sichten:  
  
 [sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)  
 Gibt eine Zeile für jede Verfügbarkeitsgruppe zurück, für die die lokale Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ein Verfügbarkeitsreplikat hostet. Jede Zeile enthält eine zwischengespeicherte Kopie der Metadaten der Verfügbarkeitsgruppe.  
  
 **Spaltennamen:** group_id, name, resource_id, resource_group_id, failure_condition_level, health_check_timeout, automated_backup_preference, automated_backup_preference_desc  
  
 [sys.availability_groups_cluster](../../../relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md)  
 Gibt eine Zeile für jede Verfügbarkeitsgruppe im WSFC-Cluster zurück. Jede Zeile enthält die Verfügbarkeitsgruppenmetadaten vom WSFC-Cluster (Windows Server-Failoverclustering).  
  
 **Spaltennamen:** group_id, name, resource_id, resource_group_id, failure_condition_level, health_check_timeout, automated_backup_preference, automated_backup_preference_desc  
  
 [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md)  
 Gibt eine Zeile für jede Verfügbarkeitsgruppe zurück, die ein Verfügbarkeitsreplikat in der lokalen Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]besitzt. In jede Zeile werden die Statuswerte angezeigt, die den Zustand einer angegebenen Verfügbarkeitsgruppe definieren.  
  
 **Spaltennamen:** group_id, primary_replica, primary_recovery_health, primary_recovery_health_desc, secondary_recovery_health, secondary_recovery_health_desc, synchronization_health, synchronization_health_desc  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **So zeigen Sie Informationen zu Verfügbarkeitsgruppen an**  
  
-   [Anzeigen von Verfügbarkeitsreplikateigenschaften &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [Anzeigen von Eigenschaften des Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)  
  
-   [Always On-Richtlinien für Betriebsprobleme mit Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)  
  
-   [Verwenden des AlwaysOn-Dashboards &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
-   [Überwachen von Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
 **So konfigurieren Sie eine vorhandene Verfügbarkeitsgruppe**  
  
-   [Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Entfernen einer sekundären Replikats aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [Hinzufügen einer Datenbank zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)  
  
-   [Entfernen einer sekundären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)  
  
-   [Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Entfernen eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md)  
  
-   [Entfernen einer primären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)  
  
-   [Entfernen einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)  
  
 **So führen Sie manuell ein Failover für eine Verfügbarkeitsgruppe aus**  
  
-   [Ausführen eines geplanten manuellen Failovers einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Ausführen eines erzwungenen manuellen Failovers einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Überwachen von Verfügbarkeitsgruppen (Transact-SQL)](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Always On-Richtlinien für Betriebsprobleme mit Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)  
  
  
