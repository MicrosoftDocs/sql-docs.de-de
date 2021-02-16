---
title: Ausführen eines geplanten manuellen Failovers einer Verfügbarkeitsgruppe
description: In diesem Artikel wird beschrieben, wie Sie ein geplantes manuelles Failover einer Always On-Verfügbarkeitsgruppe durchführen können.
ms.custom: seodec18
ms.date: 10/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.manualfailover.f1
helpviewer_keywords:
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 419f655d-3f9a-4e7d-90b9-f0bab47b3178
author: cawrites
ms.author: chadam
ms.openlocfilehash: e5efd8802ea57a6575f9e960b44ffffd9a32f83e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350783"
---
# <a name="perform-a-planned-manual-failover-of-an-always-on-availability-group-sql-server"></a>Ausführen eines geplanten manuellen Failovers einer Always On-Verfügbarkeitsgruppe (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
In diesem Thema wird beschrieben, wie ein manuelles Failover ohne Datenverlust (ein *geplantes manuelles Failover*) in einer AlwaysOn-Verfügbarkeitsgruppe mit [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]oder PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]ausgeführt wird. Eine Verfügbarkeitsgruppe führt auf der Ebene eines Verfügbarkeitsreplikats ein Failover aus. Bei einem geplanten manuellen Failover wird – wie bei jedem Failover in einer AlwaysOn-Verfügbarkeitsgruppe – ein sekundäres Replikat in die primäre Rolle überführt. Parallel dazu wird das bisherige primäre Replikat in die sekundäre Rolle überführt.  
  
Ein geplantes manuelles Failover wird nur unterstützt, wenn das primäre Replikat und das sekundäre Zielreplikat im Modus für synchrone Commits ausgeführt werden und aktuell synchronisiert sind. Bei einem geplanten manuellen Failover bleiben sämtliche Daten in den sekundären Datenbanken, die im sekundären Zielreplikat mit der Verfügbarkeitsgruppe verknüpft sind, erhalten. Nachdem das frühere primäre Replikat in die sekundäre Rolle übergegangen ist, werden seine Datenbanken zu sekundären Datenbanken. Anschließend beginnt Ihre Synchronisierung mit den neuen primären Datenbanken. Nach dem Übergang in den Status SYNCHRONIZED kann das neue sekundäre Replikat als Ziel eines künftigen geplanten manuellen Failovers dienen.  
  
> [!NOTE]  
>  Wenn das sekundäre und primäre Replikat für den automatischen Failovermodus konfiguriert sind, kann das sekundäre Replikat nach der Synchronisierung auch als Ziel für ein automatisches Failover dienen. Weitere Informationen finden Sie unter [Verfügbarkeitsmodi &#40;AlwaysOn-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
   
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Voraussetzungen 

>[!IMPORTANT]
>Für das Failover einer schreibgeschützten Verfügbarkeitsgruppe ohne Cluster-Manager gibt es bestimmte Vorgehensweisen. Wenn für eine Verfügbarkeitsgruppe CLUSTER_TYPE = NONE gilt, führen Sie die Schritte unter [Ausführen eines Failovers des primären Replikats auf schreibgeschützten Verfügbarkeitsgruppen](#fail-over-the-primary-replica-on-a-read-scale-availability-group) aus.

###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen 
  
- Ein Failoverbefehl gibt einen Wert zurück, sobald das sekundäre Zielreplikat den Befehl akzeptiert hat. Die Datenbankwiederherstellung tritt jedoch asynchron auf, nachdem die Verfügbarkeitsgruppe aufgehört hat, ein Failover auszuführen. 
- Datenbankübergreifende Konsistenz zwischen Datenbanken innerhalb der Verfügbarkeitsgruppe wird beim Failover möglicherweise nicht beibehalten. 
  
    > [!NOTE] 
    >  Die Unterstützung für datenbankübergreifende und verteilte Transaktionen unterscheidet sich je nach verwendeter SQL Server- und Betriebssystemversion. Weitere Informationen finden Sie unter [Datenbankübergreifende Transaktionen und verteilte Transaktionen für Always On-Verfügbarkeitsgruppen oder Datenbankspiegelung &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md). 
  
###  <a name="prerequisites-and-restrictions"></a><a name="Prerequisites"></a> Voraussetzungen und Einschränkungen 
  
-   Sowohl das sekundäre Zielreplikat als auch das primäre Replikat müssen im Verfügbarkeitsmodus für synchrone Commits ausgeführt werden. 
-   Das sekundäre Zielreplikat muss aktuell mit dem primären Replikat synchronisiert sein. Alle sekundären Datenbanken in diesem sekundären Replikat müssen mit der Verfügbarkeitsgruppe verknüpft sein. Ebenso müssen sie mit ihren entsprechenden primären Datenbanken synchronisiert sein (d.h., die lokalen sekundären Datenbanken müssen SYNCHRONIZED sein). 
  
    > [!TIP] 
    >  Sie können die Failoverbereitschaft eines sekundären Replikats ermitteln, indem Sie die Spalte **is_failover_ready** in der dynamischen Verwaltungssicht [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md) abfragen. Alternativ können Sie sich die Spalte **Failoverbereitschaft** des [AlwaysOn-Gruppendashboards](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md) ansehen. 
-   Dieser Task wird nur für das sekundäre Zielreplikat unterstützt. Sie müssen mit der Serverinstanz verbunden sein, auf der das sekundäre Zielreplikat gehostet wird. 
  
###  <a name="security"></a><a name="Security"></a> Sicherheit 
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen 
 Für die Verfügbarkeitsgruppe ist die Berechtigung ALTER AVAILABILITY GROUP erforderlich. Die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung sind ebenfalls erforderlich. 
  
##  <a name="use-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio 
 So führen Sie manuell ein Failover für eine Verfügbarkeitsgruppe aus: 
  
1. Stellen Sie im Objekt-Explorer eine Verbindung mit einer Serverinstanz her, die ein sekundäres Replikat der Verfügbarkeitsgruppe hostet, für die ein Failover ausgeführt werden muss. Erweitern Sie die Serverstruktur. 
  
2. Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** . 
  
3. Klicken Sie mit der rechten Maustaste auf die Verfügbarkeitsgruppe, für die ein Failover ausgeführt werden soll, und wählen Sie **Failover** aus. 
  
4. Der Assistent für das Failover von Verfügbarkeitsgruppen wird gestartet. Weitere Informationen finden Sie unter [Verwenden des Assistenten für Failover-Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md). 
  
##  <a name="use-transact-sql"></a><a name="TsqlProcedure"></a> Verwendung von Transact-SQL 
 So führen Sie manuell ein Failover für eine Verfügbarkeitsgruppe aus: 
  
1. Stellen Sie eine Verbindung mit der Serverinstanz her, die das sekundäre Zielreplikat hostet. 
  
2. Verwenden Sie die [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) -Anweisung wie folgt: 
  
     ALTER AVAILABILITY GROUP *Gruppenname* FAILOVER 
  
     In der Anweisung ist *Gruppenname* der Name der Verfügbarkeitsgruppe. 
  
     Im folgenden Beispiel wird manuell ein Failover der *MyAg*-Verfügbarkeitsgruppe zum verbundenen sekundären Replikat ausgeführt: 
  
    ```  
    ALTER AVAILABILITY GROUP MyAg FAILOVER;  
    ```  
  
##  <a name="use-powershell"></a><a name="PowerShellProcedure"></a> Verwendung von PowerShell 
 So führen Sie manuell ein Failover für eine Verfügbarkeitsgruppe aus: 
  
1. Wechseln Sie (mit **cd**) in das Verzeichnis der Serverinstanz, die das sekundäre Zielreplikat hostet. 
  
2. Verwenden Sie das Cmdlet **Switch-SqlAvailabilityGroup** . 
  
    > [!NOTE] 
    >  Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help for SQL Server PowerShell](../../../powershell/sql-server-powershell.md) (Hilfe zu SQL Server-PowerShell anfordern). 
  
     Im folgenden Beispiel wird manuell ein Failover der *MyAg*-Verfügbarkeitsgruppe zum sekundären Replikat mit dem angegebenen Pfad ausgeführt: 
  
    ```  
    Switch-SqlAvailabilityGroup -Path SQLSERVER:\Sql\SecondaryServer\InstanceName\AvailabilityGroups\MyAg  
    ```  
  
    So richten Sie den SQL Server PowerShell-Anbieter ein und verwenden ihn: 
  
    -   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md) 
    -   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md) (Hilfe zu SQL Server PowerShell anfordern) 

##  <a name="follow-up-after-you-manually-fail-over-an-availability-group"></a><a name="FollowUp"></a> Nachverfolgung: Nach dem manuellen Ausführen eines Failovers für eine Verfügbarkeitsgruppe 
 Wenn Sie ein Failover außerhalb des [!INCLUDE[ssFosAuto](../../../includes/ssfosauto-md.md)] der Verfügbarkeitsgruppe ausgeführt haben, passen Sie die Quorumabstimmungen der WSFC-Knoten (Windows Server-Failoverclustering) an die Konfiguration der neuen Verfügbarkeitsgruppe an. Weitere Informationen finden Sie unter [Windows Server-Failoverclustering &#40;WSFC&#41; mit SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md). 

<a name = "ReadScaleOutOnly"><a/>

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>Ausführen eines Failovers des primären Replikats auf eine schreibgeschützte Verfügbarkeitsgruppe

[!INCLUDE[Force failover](../../../includes/ss-force-failover-read-scale-out.md)]

## <a name="see-also"></a>Weitere Informationen 

 * [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) 
 * [Failover und Failovermodi &#40;AlwaysOn-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) 
 * [Ausführen eines erzwungenen manuellen Failovers einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md) 
  
