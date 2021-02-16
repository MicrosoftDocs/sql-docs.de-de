---
title: Konfigurieren von Sicherungen auf sekundären Replikaten von Verfügbarkeitsgruppen
description: Informationen zum Konfigurieren von Sicherungen auf sekundären Replikaten einer Always On-Verfügbarkeitsgruppe mithilfe von Transact-SQL (T-SQL), PowerShell oder SQL Server Management Studio
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- backup priority
- backup on secondary replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], backup on secondary replicas
- active secondary replicas [SQL Server], backup on
- automated backup preference
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 74bc40bb-9f57-44e4-8988-1d69c0585eb6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7258eff52e3614b30eeb001a5f570bafd4c7f5cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348991"
---
# <a name="configure-backups-on-secondary-replicas-of-an-always-on-availability-group"></a>Konfigurieren von Sicherungen auf sekundären Replikaten von Always On-Verfügbarkeitsgruppen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie die Sicherung auf sekundären Replikaten für eine AlwaysOn-Verfügbarkeitsgruppe mit [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]oder PowerShell in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]konfiguriert wird.  
  
> [!NOTE]  
>  Eine Einführung in die Sicherung auf sekundären Replikaten finden Sie unter [Aktive sekundäre Replikate: Sicherung auf sekundären Replikaten &#40;AlwaysOn-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)konfiguriert wird.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
 Sie müssen mit der Serverinstanz verbunden sein, die das primäre Replikat hostet.  
 
   > [!NOTE]
   > Das sekundäre Replikat muss nicht lesbar sein, um Sicherungen in dieses zu laden. Sicherungen werden weiterhin im sekundären Replikat erfolgreich sein, auch wenn `Readable Secondary` auf `no` festgelegt ist. 
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
  
|Aufgabe|Berechtigungen|  
|----------|-----------------|  
|Konfigurieren der Sicherung auf sekundären Replikaten beim Erstellen einer Verfügbarkeitsgruppe|Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** und die CREATE AVAILABILITY GROUP-Serverberechtigung, ALTER ANY AVAILABILITY GROUP-Berechtigung oder CONTROL SERVER-Berechtigung.|  
|Ändern einer Verfügbarkeitsgruppe oder eines Verfügbarkeitsreplikats|Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.|  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 **So konfigurieren Sie die Sicherung auf sekundären Replikaten**  
  
1.  Stellen Sie im Objekt-Explorer eine Verbindung mit der Serverinstanz her, die das primäre Replikat hostet, und klicken Sie auf den Servernamen, um die Serverstruktur zu erweitern.  
  
2.  Erweitern Sie den Knoten **Hohe Verfügbarkeit (immer aktiviert)** und den Knoten **Verfügbarkeitsgruppen** .  
  
3.  Klicken Sie auf die Verfügbarkeitsgruppe, deren Sicherungseinstellungen Sie konfigurieren möchten, und wählen Sie den Befehl **Eigenschaften** aus.  
  
4.  Wählen Sie im Dialogfeld **Eigenschaften der Verfügbarkeitsgruppe** die Seite **Sicherungseinstellungen** aus.  
  
5.  Wählen Sie im Bereich **Wo sollten Sicherungen erfolgen?** eine der folgenden Optionen als Voreinstellung für die automatisierte Sicherung der Verfügbarkeitsgruppe aus:  
  
     **Sekundär bevorzugen**  
     Gibt an, dass Sicherungen auf einem sekundären Replikat erfolgen müssen, außer wenn es sich beim primären Replikat um das einzige Onlinereplikat handelt. In diesem Fall muss die Sicherung auf dem primären Replikat erfolgen. Dies ist die Standardoption.  
  
     **Nur sekundär**  
     Gibt an, dass Sicherungen nie auf dem primären Replikat ausgeführt werden dürfen. Wenn es sich beim primären Replikat um das einzige Onlinereplikat handelt, darf keine Sicherung erfolgen.  
  
     **Primärer Server/verwaltete Instanz**  
     Gibt an, dass die Sicherungen immer auf dem primären Replikat erfolgen müssen. Diese Option ist hilfreich, wenn Sie Sicherungsfunktionen benötigen, z. B. das Erstellen differenzieller Sicherungen, die nicht unterstützt werden, wenn die Sicherung auf einem sekundären Replikat ausgeführt wird.  
  
    > [!IMPORTANT]  
    >  Wenn Sie den Protokollversand verwenden möchten, um sekundäre Datenbanken auf eine Verfügbarkeitsgruppe vorzubereiten, legen Sie die Voreinstellung für automatisierte Sicherungen auf **Primär** fest, bis alle sekundären Datenbanken vorbereitet und mit der Verfügbarkeitsgruppe verknüpft worden sind.  
  
     **Beliebiges Replikat**  
     Gibt an, dass Sicherungsaufträge die Rolle der Verfügbarkeitsreplikate ignorieren sollen, wenn sie das Replikat zum Durchführen der Sicherungen auswählen. Sicherungsaufträge können andere Faktoren auswerten, wie z. B. die Sicherungspriorität jedes Verfügbarkeitsreplikats in Verbindung mit seinem Betriebszustand und Verbindungsstatus.  
  
    > [!IMPORTANT]  
    >  Die Voreinstellung für die automatisierte Sicherung wird nicht erzwungen. Die Interpretation dieser Voreinstellung hängt von der Logik ab, die Sie ggf. per Skript in Sicherungsaufträge für die Datenbanken in einer bestimmten Verfügbarkeitsgruppe eingefügt haben. Die Voreinstellung für die automatisierte Sicherung hat keine Auswirkungen auf Ad-hoc-Sicherungen. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Nachverfolgung: Nach dem Konfigurieren einer Sicherung auf sekundären Replikaten](#FollowUp) .  
  
6.  Verwenden Sie das Raster **Replikatssicherungsprioritäten** , um die Sicherungspriorität der Verfügbarkeitsreplikate zu ändern. Dieses Raster enthält die aktuelle Sicherungspriorität aller Serverinstanzen, die ein Replikat für die Verfügbarkeitsgruppe hosten. Es gibt folgende Rasterspalten:  
  
     **Serverinstanz**  
     Der Name der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , die das Verfügbarkeitsreplikat hostet.  
  
     **Sicherungspriorität (niedrigste = 1, höchste = 100)**  
     Gibt die Priorität für die Ausführung von Sicherungen auf diesem Replikat in Relation zu den anderen Replikaten in derselben Verfügbarkeitsgruppe an. Der Wert liegt im Bereich von 0 bis 100 und ist eine ganze Zahl. 1 gibt die niedrigste Priorität und 100 die höchste Priorität an. Wenn **BACKUP_PRIORITY** 1 ist, würde das Verfügbarkeitsreplikat nur zum Ausführen von Sicherungen ausgewählt werden, wenn gerade keine höheren Prioritätsverfügbarkeitsreplikate verfügbar sind.  
  
     **Replikat ausschließen**  
     Wählen Sie diese Option, wenn dieses Verfügbarkeitsreplikat nie zum Ausführen von Sicherungen ausgewählt werden soll. Dies ist zum Beispiel für ein Remoteverfügbarkeitsreplikat hilfreich, für das keine Failover bei Sicherungen auftreten sollen.  
  
7.  Um die Änderungen zu speichern, klicken Sie auf **OK**.  
  
 **Alternative Möglichkeiten, auf die Seite 'Sicherungseinstellungen' zuzugreifen**  
  
-   [Verwenden des Dialogfelds Neue Verfügbarkeitsgruppe &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Verwenden des Assistenten zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Verwenden des Dialogfelds Neue Verfügbarkeitsgruppe &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So konfigurieren Sie die Sicherung auf sekundären Replikaten**  
  
1.  Stellen Sie eine Verbindung mit der Serverinstanz her, die das primäre Replikat hostet.  
  
2.  Für eine neue Verfügbarkeitsgruppe verwenden Sie die Anweisung [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md). Verwenden Sie zum Ändern einer vorhandenen Verfügbarkeitsgruppe die [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md)-Anweisung.  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> PowerShell  
 **So konfigurieren Sie die Sicherung auf sekundären Replikaten**  
  
1.  Legen Sie mit (**cd**) die Serverinstanz als Standard fest, die das primäre Replikat hostet.  
  
2.  Konfigurieren Sie optional die Sicherungspriorität jedes Verfügbarkeitsreplikats, das Sie hinzufügen oder ändern. Diese Priorität wird von der Serverinstanz verwendet, von der das primäre Replikat gehostet wird, um zu entscheiden, welches Replikat eine Anforderung für die automatisierte Sicherung in einer Datenbank in der Verfügbarkeitsgruppe verarbeiten soll (das Replikat mit höchster Priorität wird ausgewählt). Die Priorität kann einer beliebigen Zahl zwischen 0 und einschließlich 100 entsprechen. Die Priorität 0 gibt an, dass das Replikat nicht für die Verarbeitung von Sicherungsanforderungen in Betracht gezogen wird.  Die Standardeinstellung ist 50.  
  
     Verwenden Sie zum Hinzufügen eines Verfügbarkeitsreplikats zu einer Verfügbarkeitsgruppe das Cmdlet **New-SqlAvailabilityReplica** . Verwenden Sie zum Ändern eines vorhandenen Verfügbarkeitsreplikats das Cmdlet **Set-SqlAvailabilityReplica** . In beiden Fällen geben Sie den **BackupPriority**_n_ -Parameter (BackupPriority) an, wobei *n* für einen Wert zwischen 0 und 100 steht.  
  
     Beispielsweise legt der folgende Befehl die Sicherungspriorität des Verfügbarkeitsreplikats `MyReplica` auf **60** fest.  
  
    ```  
    Set-SqlAvailabilityReplica -BackupPriority 60 `  
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
3.  Konfigurieren Sie optional die Voreinstellung für die automatisierte Sicherung der Verfügbarkeitsgruppe, die Sie erstellen oder ändern. Diese Voreinstellung legt fest, wie ein Sicherungsauftrag das primäre Replikat auswerten soll, wenn ausgewählt wird, wo Sicherungen ausgeführt werden sollen. Gemäß der Standardeinstellung werden sekundäre Replikate bevorzugt.  
  
     Verwenden Sie zum Erstellen einer Verfügbarkeitsgruppe das Cmdlet **New-SqlAvailabilityGroup** . Verwenden Sie zum Ändern einer vorhandenen Verfügbarkeitsgruppe das Cmdlet **Set-SqlAvailabilityGroup** . In beiden Fällen geben Sie den **AutomatedBackupPreference** -Parameter an.  
  
     Erläuterungen:  
  
     **Primärer Server/verwaltete Instanz**  
     Gibt an, dass die Sicherungen immer auf dem primären Replikat erfolgen müssen. Diese Option ist hilfreich, wenn Sie Sicherungsfunktionen benötigen, z. B. das Erstellen differenzieller Sicherungen, die nicht unterstützt werden, wenn die Sicherung auf einem sekundären Replikat ausgeführt wird.  
  
    > [!IMPORTANT]  
    >  Wenn Sie den Protokollversand verwenden möchten, um sekundäre Datenbanken auf eine Verfügbarkeitsgruppe vorzubereiten, legen Sie die Voreinstellung für automatisierte Sicherungen auf **Primär** fest, bis alle sekundären Datenbanken vorbereitet und mit der Verfügbarkeitsgruppe verknüpft worden sind.  
  
     **SecondaryOnly**  
     Gibt an, dass Sicherungen nie auf dem primären Replikat ausgeführt werden dürfen. Wenn es sich beim primären Replikat um das einzige Onlinereplikat handelt, darf keine Sicherung erfolgen.  
  
     **Sekundärer Server/verwaltete Instanz**  
     Gibt an, dass Sicherungen auf einem sekundären Replikat erfolgen müssen, außer wenn es sich beim primären Replikat um das einzige Onlinereplikat handelt. In diesem Fall muss die Sicherung auf dem primären Replikat erfolgen. Dies ist das Standardverhalten.  
  
     **None**  
     Gibt an, dass Sicherungsaufträge die Rolle der Verfügbarkeitsreplikate ignorieren sollen, wenn sie das Replikat zum Durchführen der Sicherungen auswählen. Hinweis: Sicherungsaufträge können andere Faktoren auswerten, wie z. B. die Sicherungspriorität jedes Verfügbarkeitsreplikats in Verbindung mit seinem Betriebszustand und Verbindungsstatus.  
  
    > [!IMPORTANT]  
    >  **AutomatedBackupPreference** wird nicht erzwungen. Die Interpretation dieser Voreinstellung hängt von der Logik ab, die Sie ggf. per Skript in Sicherungsaufträge für die Datenbanken in einer bestimmten Verfügbarkeitsgruppe eingefügt haben. Die Voreinstellung für die automatisierte Sicherung hat keine Auswirkungen auf Ad-hoc-Sicherungen. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Nachverfolgung: Nach dem Konfigurieren einer Sicherung auf sekundären Replikaten](#FollowUp) .  
  
     Beispielsweise legt der folgende Befehl die **AutomatedBackupPreference** -Eigenschaft der Verfügbarkeitsgruppe `MyAg` auf **SecondaryOnly** fest. Automatisierte Sicherungen der Datenbanken in dieser Verfügbarkeitsgruppe treten nie auf dem primären Replikat auf, sondern werden an das sekundäre Replikat mit der höchsten Sicherungspriorität umgeleitet.  
  
    ```  
    Set-SqlAvailabilityGroup `  
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg `  
    -AutomatedBackupPreference SecondaryOnly  
    ```  
  
> [!NOTE]  
>  Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Einrichten und Verwenden des SQL Server PowerShell-Anbieters**  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
##  <a name="follow-up-after-configuring-backup-on-secondary-replicas"></a><a name="FollowUp"></a> Nachverfolgung: Nach dem Konfigurieren einer Sicherung auf sekundären Replikaten  
 Damit die automatisierte Sicherungseinstellung für eine angegebene Verfügbarkeitsgruppe berücksichtigt wird, müssen Sie auf jeder Serverinstanz, die ein Verfügbarkeitsreplikat hostet, dessen Sicherungspriorität größer als Null (>0) ist, Sicherungsauftragsskripts für die Datenbanken in der Verfügbarkeitsgruppe erstellen. Um zu bestimmen, ob das aktuelle Replikat das bevorzugte Sicherungsreplikat ist, verwenden Sie die [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md) -Funktion in Ihrem Sicherungsskript. Wenn das Verfügbarkeitsreplikat, das von der aktuellen Serverinstanz gehostet wird, das bevorzugte Replikat für die Sicherungen ist, wird von dieser Funktion "1" zurückgegeben. Wenn dies nicht zutrifft, wird von der Funktion "0" zurückgegeben. Indem Sie auf jedem Verfügbarkeitsreplikat, von dem diese Funktion abgefragt wird, ein einfaches Skript ausführen, können Sie bestimmen, von welchem Replikat ein bestimmter Sicherungsauftrag ausgeführt werden sollte. Ein typischer Ausschnitt eines Sicherungsauftragsskripts würde z. B. so aussehen:  
  
```sql  
IF (NOT sys.fn_hadr_backup_is_preferred_replica(@DBNAME))  
BEGIN  
      Select 'This is not the preferred replica, exiting with success';  
      RETURN 0 -- This is a normal, expected condition, so the script returns success  
END  
BACKUP DATABASE @DBNAME TO DISK=<disk>  
   WITH COPY_ONLY;  
```  
  
 Indem Sie für einen Sicherungsauftrag ein Skript mit dieser Logik erstellen, können Sie die Ausführung des Auftrags auf jedem Verfügbarkeitsreplikat nach demselben Zeitplan vorsehen. Jeder dieser Aufträge greift auf die gleichen Daten zurück, um zu bestimmen, welcher Auftrag auszuführen ist. Folglich wechselt eigentlich nur einer der geplanten Aufträge in den Sicherungsstatus.  Bei einem Failover muss kein Skript oder Auftrag geändert werden. Auch bei der Neukonfiguration einer Verfügbarkeitsgruppe zum Hinzufügen eines Verfügbarkeitsreplikats ist zum Verwalten des Sicherungsauftrags nur das Kopieren oder Planen des Sicherungsauftrags erforderlich. Wenn Sie ein Verfügbarkeitsreplikat entfernen, löschen Sie einfach den Sicherungsauftrag von der Serverinstanz, die dieses Replikat gehostet hat.  
  
> [!TIP]  
>  Wenn Sie einen bestimmten Sicherungsauftrag mithilfe des [Wartungsplanungs-Assistenten](../../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)erstellen, verfügt der Auftrag automatisch über die Skripterstellungslogik, durch die die **sys.fn_hadr_backup_is_preferred_replica** -Funktion aufgerufen und überprüft wird. Der Sicherungsauftrag gibt jedoch nicht die Nachricht „Dies ist nicht das bevorzugte Replikat...“ zurück. Stellen Sie sicher, dass Sie die Aufträge für jede Verfügbarkeitsdatenbank auf jeder Serverinstanz erstellen, die ein Verfügbarkeitsreplikat für die Verfügbarkeitsgruppe hostet.  
  
##  <a name="to-obtain-information-about-backup-preference-settings"></a><a name="ForInfoAboutBuPref"></a> So rufen Sie Informationen zu Sicherungseinstellungen ab  
 Folgendes ist hilfreich zum Abrufen von Informationen, die für eine Sicherung auf sekundären Replikaten relevant sind.  
  
|Sicht|Information|Relevante Spalten|  
|----------|-----------------|----------------------|  
|[sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)|Ist das aktuelle Replikat das bevorzugte Sicherungsreplikat?|Nicht zutreffend|  
|[sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)|Voreinstellung für die automatisierte Sicherung|**automated_backup_preference**<br /><br /> **automated_backup_preference_desc**|  
|[sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)|Sicherungspriorität eines gegebenen Verfügbarkeitsreplikats|**backup_priority**|  
|[sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)|Ist das Replikat für die Serverinstanz lokal?<br /><br /> Aktuelle Rolle<br /><br /> Betriebszustand<br /><br /> Verbindungszustand<br /><br /> Synchronisierungszustand eines Verfügbarkeitsreplikats|**is_local**<br /><br /> **role**, **role_desc**<br /><br /> **operational_state**, **operational_state_desc**<br /><br /> **connected_state**, **connected_state_desc**<br /><br /> **synchronization_health**, **synchronization_health_desc**|  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   [Microsoft SQL Server Always On-Lösungshandbuch zu hoher Verfügbarkeit und Notfallwiederherstellung](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
-   [SQL Server Always On-Teamblog: Der offizielle SQL Server Always On-Teamblog](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Aktive sekundäre Replikate: Sicherung auf sekundären Replikaten (Always On-Verfügbarkeitsgruppen)](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)  
  
