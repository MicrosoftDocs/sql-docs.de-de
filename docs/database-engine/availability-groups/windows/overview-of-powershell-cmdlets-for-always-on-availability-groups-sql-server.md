---
title: PowerShell-Cmdlets für Verfügbarkeitsgruppen
description: 'Eine Referenz für die unterschiedlichen PowerShell-Cmdlets, mit denen Sie Always On-Verfügbarkeitsgruppen verwalten können. '
ms.custom: seo-lt-2019
ms.date: 08/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], PowerShell cmdlets
- Availability Groups [SQL Server], about
- PowerShell [SQL Server], cmdlets
ms.assetid: b3fef0d5-b6d7-4386-a0f0-d06c165ad4de
author: cawrites
ms.author: chadam
ms.openlocfilehash: 122a4133de7965b193ee2ca2708da9b48a5c1239
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337904"
---
# <a name="overview-of-powershell-cmdlets-for-always-on-availability-groups"></a>Übersicht über PowerShell-Cmdlets für Always On-Verfügbarkeitsgruppen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] PowerShell ist eine speziell für die Systemverwaltung entwickelte taskbasierte Befehlszeilenshell und Skriptsprache. [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] stellt in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] einen Satz von PowerShell-Cmdlets bereit, mit denen Sie Verfügbarkeitsgruppen, Verfügbarkeitsreplikate und Verfügbarkeitsdatenbanken bereitstellen, verwalten und überwachen können.  
  
> [!NOTE]  
>  Ein PowerShell-Cmdlet kann ausgeführt werden, indem eine Aktion erfolgreich initiiert wird. Dies zeigt nicht an, dass die vorgesehene Arbeit, z. B. das Failover einer Verfügbarkeitsgruppe, ausgeführt wurde. Wenn Sie Skripts für eine Aktionsfolge erstellen, müssen Sie möglicherweise den Status von Aktionen überprüfen und auf deren Ausführung warten.  
  
> [!NOTE]  
>  Eine Liste von Themen in der [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] -Onlinedokumentation, die beschreiben, wie Cmdlets zum Ausführen von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Aufgaben verwendet werden, finden Sie im Abschnitt "Verwandte Aufgaben" von [Übersicht über Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
##  <a name="configuring-a-server-instance-for-always-on-availability-groups"></a><a name="ConfiguringServerInstance"></a> Configuring a Server Instance for Always On Availability Groups  
  
|Cmdlets|BESCHREIBUNG|Unterstützt auf|  
|-------------|-----------------|------------------|
|[**Disable-SqlAlwaysOn**](/powershell/module/sqlserver/disable-sqlalwayson)|Deaktiviert die [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Funktion auf einer Serverinstanz.|Die Serverinstanz, die vom Parameter **Path**, **InputObject** oder **Name** angegeben wird. (Muss eine Edition von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sein, die [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]unterstützt.)|  
|[**Enable-SqlAlwaysOn**](/powershell/module/sqlserver/enable-sqlalwayson)|Aktiviert [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] auf einer Instanz von [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] , die die [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Funktion unterstützt. Informationen zur Unterstützung für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).|Eine beliebige Edition von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , die [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]unterstützt.|  
|[**New-SqlHadrEndPoint**](/powershell/module/sqlserver/new-sqlhadrendpoint)|Erstellt einen neuen Datenbankspiegelungs-Endpunkt auf einer Serverinstanz. Dieser Endpunkt ist zur Datenverschiebung zwischen primären und sekundären Datenbanken erforderlich.|Eine beliebige Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]|  
|[**Set-SqlHadrEndpoint**](/powershell/module/sqlserver/set-sqlhadrendpoint)|Ändert die Eigenschaften eines vorhandenen Datenbankspiegelungs-Endpunkts, z. B. Namens-, Status- oder Authentifizierungseigenschaften.|Eine Serverinstanz, die [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] unterstützt und keinen Datenbankspiegelungs-Endpunkt aufweist|  

  
##  <a name="backing-up-and-restoring-databases-and-transaction-logs"></a><a name="BnRcmdlets"></a> Backing Up and Restoring Databases and Transaction Logs  
  
|Cmdlets|BESCHREIBUNG|Unterstützt auf|  
|-------------|-----------------|------------------|  
|[**Backup-SqlDatabase**](/powershell/module/sqlserver/backup-sqldatabase)|Erstellt eine Daten- oder Protokollsicherung.|Eine beliebige Onlinedatenbank (für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]eine Datenbank auf der Serverinstanz, die das primäre Replikat hostet)|  
|[**Restore-SqlDatabase**](/powershell/module/sqlserver/restore-sqldatabase)|Stellt eine Sicherung wieder her.|Eine beliebige Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]eine Serverinstanz, die ein sekundäres Replikat hostet)<br /><br />

  >[!Important]
  >Beim Vorbereiten einer sekundären Datenbank müssen Sie den **-NoRecovery**-Parameter in jedem **Restore-SqlDatabase**-Befehl verwenden. 
  
 Informationen zur Verwendung dieser Cmdlets zum Vorbereiten einer sekundären Datenbank finden Sie unter [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
##  <a name="creating-and-managing-an-availability-group"></a><a name="DeployManageAGs"></a> Creating and Managing an Availability Group  
  
|Cmdlets|BESCHREIBUNG|Unterstützt auf|  
|-------------|-----------------|------------------|  
|[**New-SqlAvailabilityGroup**](/powershell/module/sqlserver/new-sqlavailabilitygroup)|Erstellt eine neue Verfügbarkeitsgruppe.|Serverinstanz zum Hosten des primären Replikats|  
|[**Remove-SqlAvailabilityGroup**](/powershell/module/sqlserver/remove-sqlavailabilitygroup)|Löscht eine Verfügbarkeitsgruppe.|HADR-fähige Serverinstanz|  
|[**Set-SqlAvailabilityGroup**](/powershell/module/sqlserver/set-sqlavailabilitygroup)|Legt die Eigenschaften einer Verfügbarkeitsgruppe fest; schaltet eine Verfügbarkeitsgruppe online/offline|Serverinstanz, die das primäre Replikat hostet|  
|[**Switch-SqlAvailabilityGroup**](/powershell/module/sqlserver/switch-sqlavailabilitygroup)|Initiiert einen der folgenden Failovertypen:<br /><br /> Ein erzwungenes Failover einer Verfügbarkeitsgruppe (mit möglichem Datenverlust).<br /><br /> Ein manuelles Failover einer Verfügbarkeitsgruppe.|Serverinstanz, die das sekundäre Zielreplikat hostet|  
  
##  <a name="creating-and-managing-an-availability-group-listener"></a><a name="AGlisteners"></a> Creating and Managing an Availability Group Listener  
  
|Cmdlet|BESCHREIBUNG|Unterstützt auf|  
|------------|-----------------|------------------|  
|[**New-SqlAvailabilityGroupListener**](/powershell/module/sqlserver/new-sqlavailabilitygrouplistener)|Erstellt einen neuen Verfügbarkeitsgruppenlistener und fügt ihn einer vorhandenen Verfügbarkeitsgruppe hinzu.|Serverinstanz, die das primäre Replikat hostet|  
|[**Set-SqlAvailabilityGroupListener**](/powershell/module/sqlserver/set-sqlavailabilitygrouplistener)|Ändert die Porteinstellung eines vorhandenen Verfügbarkeitsgruppenlisteners.|Serverinstanz, die das primäre Replikat hostet|  
|[**Add-SqlAvailabilityGroupListenerStaticIp**](/powershell/module/sqlserver/add-sqlavailabilitygrouplistenerstaticip)|Fügt der vorhandenen Konfiguration eines Verfügbarkeitsgruppenlisteners eine statische IP-Adresse hinzu. Die IP-Adresse kann eine IPv4-Adresse mit Subnetz oder eine IPv6-Adresse sein.|Serverinstanz, die das primäre Replikat hostet|  
  
##  <a name="creating-and-managing-an-availability-replica"></a><a name="DeployManageARs"></a> Creating and Managing an Availability Replica  
  
|Cmdlets|BESCHREIBUNG|Unterstützt auf|  
|-------------|-----------------|------------------|  
|[**New-SqlAvailabilityReplica**](/powershell/module/sqlserver/new-sqlavailabilityreplica)|Erstellt eine neue Verfügbarkeitsgruppe. Sie können mithilfe des **-AsTemplate** -Parameters für jedes neue Verfügbarkeitsreplikat ein Verfügbarkeitsreplikatobjekt im Arbeitsspeicher erstellen.|Serverinstanz, die das primäre Replikat hostet|  
|[**Join-SqlAvailabilityGroup**](/powershell/module/sqlserver/join-sqlavailabilitygroup)|Verknüpft ein sekundäres Replikat mit der Verfügbarkeitsgruppe.|Serverinstanz, die ein sekundäres Replikat hostet|  
|[**Remove-SqlAvailabilityReplica**](/powershell/module/sqlserver/remove-sqlavailabilityreplica)|Lösch Sie ein Verfügbarkeitsreplikat.|Serverinstanz, die das primäre Replikat hostet|  
|[**Set-SqlAvailabilityReplica**](/powershell/module/sqlserver/set-sqlavailabilityreplica)|Legt die Eigenschaften eines Verfügbarkeitsreplikats fest.|Serverinstanz, die das primäre Replikat hostet|  
  
##  <a name="adding-and-managing-an-availability-database"></a><a name="DeployManageDbs"></a> Adding and Managing an Availability Database  
  
|Cmdlets|BESCHREIBUNG|Unterstützt auf|  
|-------------|-----------------|------------------|  
|[**Add-SqlAvailabilityDatabase**](/powershell/module/sqlserver/add-sqlavailabilitydatabase)|Fügt auf dem primären Replikat einer Verfügbarkeitsgruppe eine Datenbank hinzu.<br /><br /> Verknüpft auf einem sekundären Replikat eine sekundäre Datenbank mit einer Verfügbarkeitsgruppe.|Eine beliebige Serverinstanz, die ein Verfügbarkeitsreplikat hostet (Verhalten unterscheidet sich für primäre und sekundäre Replikate)|  
|[**Remove-SqlAvailabilityDatabase**](/powershell/module/sqlserver/remove-sqlavailabilitydatabase)|Entfernt auf dem primären Replikat die Datenbank aus der Verfügbarkeitsgruppe.<br /><br /> Entfernt auf einem sekundären Replikat die lokale sekundäre Datenbank aus dem lokalen sekundären Replikat.|Eine beliebige Serverinstanz, die ein Verfügbarkeitsreplikat hostet (Verhalten unterscheidet sich für primäre und sekundäre Replikate)|  
|[**Resume-SqlAvailabilityDatabase**](/powershell/module/sqlserver/resume-sqlavailabilitydatabase)|Setzt die Datenverschiebung für eine angehaltene Verfügbarkeitsdatenbank fort.|Die Serverinstanz, auf der die Datenbank angehalten wurde.|  
|[**Suspend-SqlAvailabilityDatabase**](/powershell/module/sqlserver/suspend-sqlavailabilitydatabase)|Hält die Datenverschiebung für eine Verfügbarkeitsdatenbank an.|Eine beliebige Serverinstanz, die ein Verfügbarkeitsreplikat hostet.|  
  
##  <a name="monitoring-availability-group-health"></a><a name="MonitorTblshtAGs"></a> Monitoring Availability Group Health  
 Mit den folgenden [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Cmdlets können Sie die Integrität einer Verfügbarkeitsgruppe und ihrer Replikate und Datenbanken überwachen.  
  
> [!IMPORTANT]  
>  Sie müssen über CONNECT-, VIEW SERVER STATE- und VIEW ANY DEFINITION-Berechtigungen verfügen, um diese Cmdlets auszuführen.  
  
|Cmdlet|BESCHREIBUNG|Unterstützt auf|  
|------------|-----------------|------------------|  
|[**Test-SqlAvailabilityGroup**](/powershell/module/sqlserver/test-sqlavailabilitygroup)|Bewertet die Integrität einer Verfügbarkeitsgruppe durch die Auswertung der Richtlinien der richtlinienbasierten SQL Server-Verwaltung.|Eine beliebige Serverinstanz, die ein Verfügbarkeitsreplikat hostet.*|  
|[**Test-SqlAvailabilityReplica**](/powershell/module/sqlserver/test-sqlavailabilityreplica)|Bewertet die Integrität von Verfügbarkeitsreplikaten durch die Auswertung der Richtlinien der richtlinienbasierten SQL Server-Verwaltung.|Eine beliebige Serverinstanz, die ein Verfügbarkeitsreplikat hostet.*|  
|[**Test-SqlDatabaseReplicaState**](/powershell/module/sqlserver/test-sqldatabasereplicastate)|Bewertet die Integrität einer Verfügbarkeitsdatenbank für alle hinzugefügten Verfügbarkeitsreplikate durch die Auswertung der Richtlinien der richtlinienbasierten SQL Server-Verwaltung.|Eine beliebige Serverinstanz, die ein Verfügbarkeitsreplikat hostet.*|  
  
 *Verwenden Sie zum Anzeigen von Informationen zu allen Verfügbarkeitsreplikaten in einer Verfügbarkeitsgruppe die Serverinstanz, die das primäre Replikat hostet.  
  
 Weitere Informationen finden Sie unter [Verwenden von Always On-Richtlinien zum Anzeigen des Zustands einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
