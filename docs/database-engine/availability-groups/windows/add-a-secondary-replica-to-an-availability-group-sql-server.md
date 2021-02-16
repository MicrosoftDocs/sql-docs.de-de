---
title: Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe
description: Erfahren Sie, wie ein sekundäres Replikat einer vorhandenen Always On-Verfügbarkeitsgruppe hinzugefügt wird, entweder mithilfe von Transact-SQL (T-SQL), PowerShell oder dem Assistenten für Verfügbarkeitsgruppen in SQL Server Management Studio (SSMS).
ms.custom: seodec18
ms.date: 05/18/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], configuring
ms.assetid: 6669dcce-85f9-495f-aadf-7f62cff4a9da
author: cawrites
ms.author: chadam
ms.openlocfilehash: cde589fa3ba6f1fd46dcf1b16077797cdb268dad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354395"
---
# <a name="add-a-secondary-replica-to-an-always-on-availability-group"></a>Hinzufügen eines sekundären Replikats zu einer Always On-Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie ein sekundäres Replikat einer vorhandenen AlwaysOn-Verfügbarkeitsgruppe mit [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]oder PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]hinzugefügt wird.  

  
##  <a name="prerequisites-and-restrictions"></a><a name="PrerequisitesRestrictions"></a> Voraussetzungen und Einschränkungen  
  
-   Sie müssen mit der Serverinstanz verbunden sein, die das primäre Replikat hostet.  
  
 Weitere Informationen finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)zu unterstützen.  

##  <a name="security"></a><a name="Security"></a> Sicherheit  
  
###  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.  

[!INCLUDE[Freshness](../../../includes/paragraph-content/fresh-note-steps-feedback.md)]

##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So fügen Sie ein Replikat hinzu**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die das primäre Verfügbarkeitsreplikat hostet, und erweitern Sie die Serverstruktur.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Klicken Sie mit der rechten Maustaste auf die Verfügbarkeitsgruppe, und wählen Sie einen der folgenden Befehle aus:  
  
    -   Wählen Sie zum Starten des Assistenten zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen den Befehl **Replikat hinzufügen** aus. Weitere Informationen finden Sie unter [Verwenden des Assistenten zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md).  
  
    -   Wählen Sie alternativ den Befehl **Eigenschaften** aus, um das Dialogfeld **Eigenschaften der Verfügbarkeitsgruppe** zu öffnen. Für das Hinzufügen eines Replikats in diesem Dialogfeld sind folgende Schritte erforderlich:  
  
        1.  Klicken Sie im Bereich **Verfügbarkeitsreplikate** des Dialogfelds auf die Schaltfläche **Hinzufügen** . Dadurch wird ein Replikateintrag erstellt und ausgewählt, in dem das leere Serverinstanzfeld ausgewählt ist.  
  
        2.  Geben Sie den Namen einer Serverinstanz ein, die die Voraussetzungen zum Hosten eines Verfügbarkeitsreplikats erfüllt.  
  
         Wiederholen Sie zum Hinzufügen eines weiteren Replikats die vorhergehenden Schritte. Wenn Sie alle Replikate angegeben haben, klicken Sie auf **OK** , um den Vorgang abzuschließen.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So fügen Sie ein Replikat hinzu**  
  
1.  Stellen Sie eine Verbindung mit der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] her, die das primäre Replikat hostet.  
  
2.  Fügen Sie der Verfügbarkeitsgruppe das neue sekundäre Replikat mit der ADD REPLICA ON-Klausel der ALTER AVAILABILITY GROUP-Anweisung hinzu. In einer ADD REPLICA ON-Klausel sind die ENDPOINT_URL-, AVAILABILITY_MODE- und FAILOVER_MODE-Optionen erforderlich. Die anderen Replikatoptionen (BACKUP_PRIORITY, SECONDARY_ROLE, PRIMARY_ROLE und SESSION_TIMEOUT) sind optional. Weitere Informationen finden Sie unter [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)hinzugefügt wird.  
  
     Beispielsweise wird durch die folgende [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Anweisung ein neues Replikat für eine Verfügbarkeitsgruppe namens `MyAG` auf der von `COMPUTER04`gehosteten Standardserverinstanz, deren Endpunkt-URL `TCP://COMPUTER04.Adventure-Works.com:5022'`lautet, erstellt. Das Replikat unterstützt ein manuelles Failover und den Verfügbarkeitsmodus für asynchrone Commits.  
  
    ```  
    ALTER AVAILABILITY GROUP MyAG ADD REPLICA ON 'COMPUTER04'   
       WITH (  
             ENDPOINT_URL = 'TCP://COMPUTER04.Adventure-Works.com:5022',  
             AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,  
             FAILOVER_MODE = MANUAL  
             );  
    ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> PowerShell  
 **So fügen Sie ein Replikat hinzu**  
  
1.  Wechseln Sie mit **cd** in das Verzeichnis der Serverinstanz, die das primäre Replikat hostet.  
  
2.  Verwenden Sie das **New-SqlAvailabilityReplica** -Cmdlet.  
  
     Beispielsweise wird mit dem folgenden Befehl einer vorhandenen Verfügbarkeitsgruppe namens `MyAg`ein Verfügbarkeitsreplikat hinzugefügt. Das Replikat unterstützt ein manuelles Failover und den Verfügbarkeitsmodus für asynchrone Commits. In der sekundären Rolle unterstützt dieses Replikat Verbindungen mit Lesezugriff, sodass Sie die schreibgeschützte Verarbeitung auf dieses Replikat auslagern können.  
  
    ```  
    $agPath = "SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg"  
    $endpointURL = "TCP://PrimaryServerName.domain.com:5022"  
    $failoverMode = "Manual"  
    $availabilityMode = "AsynchronousCommit"  
    $secondaryReadMode = "AllowAllConnections"  
  
    New-SqlAvailabilityReplica -Name SecondaryServer\Instance `   
    -EndpointUrl $endpointURL `   
    -FailoverMode $failoverMode `   
    -AvailabilityMode $availabilityMode `   
    -ConnectionModeInSecondaryRole $secondaryReadMode `   
    -Path $agPath  
    ```  
  
    > [!NOTE]  
    >  Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Einrichten und Verwenden des SQL Server PowerShell-Anbieters**  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-after-adding-a-secondary-replica"></a><a name="FollowUp"></a> Nachverfolgung: Nach dem Hinzufügen eines sekundären Replikats  
 Zum Hinzufügen eines Replikats für eine vorhandene Verfügbarkeitsgruppe müssen Sie die folgenden Schritte ausführen:  
  
1.  Stellen Sie eine Verbindung mit der Serverinstanz her, die das neue sekundäre Replikat hosten wird.  
  
2.  Verknüpfen Sie das neue sekundäre Replikat mit der Verfügbarkeitsgruppe. Weitere Informationen finden Sie unter [Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)mit einer Always On-Verfügbarkeitsgruppe verknüpft wird.  
  
3.  Erstellen Sie für jede Datenbank in der Verfügbarkeitsgruppe eine sekundäre Datenbank auf der Serverinstanz, die das sekundäre Replikat hostet. Weitere Informationen finden Sie unter [Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)erstellt und konfiguriert wird.  
  
4.  Verknüpfen Sie alle neuen sekundären Datenbanken mit der Verfügbarkeitsgruppe. Weitere Informationen finden Sie unter [Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)aktiviert sind, eine Always On-Verfügbarkeitsgruppe zu erstellen.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **So verwalten Sie ein Verfügbarkeitsreplikat**  
  
-   [Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Entfernen einer sekundären Replikats aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
-   [Konfigurieren des schreibgeschützten Zugriffs auf ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Ändern des Verfügbarkeitsmodus eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Ändern des Failovermodus eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Ändern des Sitzungstimeouts für ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
-   [Ändern des Sitzungstimeouts für ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Erstellung und Konfiguration von Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Verwenden des Alway On-Dashboards (SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)   
 [Überwachen von Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
