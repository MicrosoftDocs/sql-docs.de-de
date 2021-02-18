---
title: 'Verfügbarkeitsgruppe: Voraussetzungen, Einschränkungen und Empfehlungen'
description: Beschreibung der Voraussetzungen, Einschränkungen und Empfehlungen zur Bereitstellung von Always On-Verfügbarkeitsgruppen auf SQL Server.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], server instance
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], WSFC clusters
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], prerequisites and restrictions
- Availability Groups [SQL Server], Failover Cluster Instances
- Availability Groups [SQL Server], databases
- Availability Groups [SQL Server]
ms.assetid: edbab896-42bb-4d17-8d75-e92ca11f7abb
author: cawrites
ms.author: chadam
ms.openlocfilehash: 571cf683c8d905cfb499624cbd05dc40efe94b86
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100344639"
---
# <a name="prerequisites-restrictions-and-recommendations-for-always-on-availability-groups"></a>Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  In diesem Artikel werden Überlegungen zur Bereitstellung von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] beschrieben, einschließlich Voraussetzungen, Einschränkungen und Empfehlungen für Hostcomputer, Windows Server-Failovercluster (WSFC), Serverinstanzen und Verfügbarkeitsgruppen. Für alle Komponenten sind Überlegungen zur Sicherheit und ggf. erforderliche Berechtigungen angegeben.  
  
> [!IMPORTANT]  
>  Vor der Bereitstellung von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]wird empfohlen, dieses Thema vollständig zu lesen.  
    
##  <a name="net-hotfixes-that-support-availability-groups"></a><a name="DotNetHotfixes"></a> .NET-Hotfixes mit Unterstützung für Verfügbarkeitsgruppen  
 Abhängig von den [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]-Komponenten und -Features, die Sie mit [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]verwenden, müssen Sie möglicherweise zusätzliche in der folgenden Tabelle angegebene .NET-Hotfixes installieren. Die Hotfixes können in beliebiger Reihenfolge installiert werden.  
  
|Abhängige Funktion|Hotfix|Link|  
|-----------------------|------------|----------|  
|[!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]|Der Hotfix für .NET 3.5 SP1 fügt dem SQL-Client Unterstützung für die Always On-Features „Read-intent“, „readonly“ und „multisubnetfailover“ hinzu. Der Hotfix muss auf allen [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] -Berichtsservern installiert werden.|KB 2654347: [Hotfix für .NET 3.5 SP1 zum Hinzufügen von Unterstützung für Always On-Features](https://go.microsoft.com/fwlink/?LinkId=242896)|  
  

###  <a name="checklist-requirements-windows-system"></a><a name="SystemRequirements"></a> Prüfliste: Anforderungen (Windows-System)  
 Zur Unterstützung der Funktion [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] muss gewährleistet sein, dass jeder Computer, der an mindestens einer Verfügbarkeitsgruppe teilnehmen soll, die folgenden wesentlichen Anforderungen erfüllt:  
  
|Anforderung|Link|  
|-----------------|----------|  
|Stellen Sie sicher, dass es sich bei diesem System nicht um einen Domänencontroller handelt.|Verfügbarkeitsgruppen werden nicht auf Domänencontrollern unterstützt.|  
|Stellen Sie sicher, dass auf jedem Computer Windows Server 2012 oder höhere Versionen ausgeführt werden.|[Hardware- und Softwareanforderungen für die Installation von SQL Server 2016](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)|  
|Stellen Sie sicher, das jeder Computer ein Knoten in WSFC ist.|[Windows Server-Failoverclustering &#40;WSFC&#41; mit SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)|  
|Stellen Sie sicher, dass WSFC ausreichend Knoten enthält, um die Verfügbarkeitsgruppenkonfigurationen zu unterstützen.|Ein Clusterknoten kann ein Replikat für eine Verfügbarkeitsgruppe hosten. Es können nicht zwei Replikate aus der gleichen Verfügbarkeitsgruppe auf ein und demselben Knoten gehostet werden. Der Clusterknoten kann zu mehreren Verfügbarkeitsgruppen gehören und ein Replikat jeder Gruppe hosten. <br /><br /> Fragen Sie die Datenbankadministratoren, wie viele Clusterknoten erforderlich sind, um die Verfügbarkeitsreplikate der geplanten Verfügbarkeitsgruppen zu unterstützen.<br /><br /> [Übersicht über Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).|  

  
> [!IMPORTANT]  
>  Stellen Sie zudem sicher, dass Ihre Umgebung ordnungsgemäß zum Herstellen einer Verbindung mit einer Verfügbarkeitsgruppe konfiguriert wird. Weitere Informationen finden Sie unter [Always On-Clientkonnektivität &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
##  <a name="recommendations-for-computers-that-host-availability-replicas-windows-system"></a><a name="ComputerRecommendations"></a> Empfehlungen für Computer, die Verfügbarkeitsreplikate (Windows-System) hosten  
  
-   **Vergleichbare Systeme:**  Für eine bestimmte Verfügbarkeitsgruppe sollten alle Verfügbarkeitsreplikate auf vergleichbaren Systemen ausgeführt werden, die identische Arbeitslasten bewältigen können.  
  
-   **Dedizierte Netzwerkadapter:**  Für eine optimale Leistung sollten Sie einen dedizierten Netzwerkadapter (NIC, Network Interface Card, Netzwerkschnittstellenkarte) für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] verwenden.  
  
-   **Genügend freien Speicherplatz:**  Jeder Computer, auf dem eine Serverinstanz ein Verfügbarkeitsreplikat hostet, muss über ausreichend Speicherplatz für alle Datenbanken in der Verfügbarkeitsgruppe verfügen. Bedenken Sie, dass sekundäre Datenbanken in gleichem Maße zunehmen wie ihre entsprechenden primären Datenbanken.  
  
###  <a name="permissions-windows-system"></a><a name="PermissionsWindows"></a> Berechtigungen (Windows-System)  
 Zur Verwaltung eines WSFC muss der Benutzer Systemadministrator auf jedem Clusterknoten sein.  
  
 Weitere Informationen über das Konto zum Verwalten des Clusters finden Sie unter [Appendix A: Failover Cluster Requirements (Anhang A: Failoverclusteranforderungen)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197454(v=ws.10)).  
  
###  <a name="related-tasks-windows-system"></a><a name="RelatedTasksWindows"></a> Verwandte Aufgaben (Windows-System)  
  
|Aufgabe|Link|  
|----------|----------|  
|Legen Sie den HostRecordTTL-Wert fest.|[Ändern des HostRecordTTL (Verwenden von Windows PowerShell)](#ChangeHostRecordTTLps)|  
  
####  <a name="change-the-hostrecordttl-using-windows-powershell"></a><a name="ChangeHostRecordTTLps"></a> Ändern des HostRecordTTL (Verwenden von Windows PowerShell)  
  
1.  Öffnen Sie das PowerShell-Fenster über **Als Administrator ausführen**.  
  
2.  Importieren Sie das FailoverClusters-Modul.  
  
3.  Verwenden Sie das **Get-ClusterResource** -Cmdlet, um die Netzwerknamenressource anzuzeigen, und verwenden Sie das **Set-ClusterParameter** -Cmdlet, um den **HostRecordTTL** -Wert wie folgt festzulegen:  
  
     Get-ClusterResource " *\<NetworkResourceName>* " | Set-ClusterParameter HostRecordTTL *\<TimeInSeconds>*  
  
     Im folgenden PowerShell-Beispiel wird der HostRecordTTL für eine Netzwerknamenressource mit dem Namen `SQL Network Name (SQL35)` auf 300 Sekunden festgelegt.  
  
    ```powershell
    Import-Module FailoverClusters  
  
    $nameResource = "SQL Network Name (SQL35)"  
    Get-ClusterResource $nameResource | Set-ClusterParameter ClusterParameter HostRecordTTL 300  
    ```  
  
    > [!TIP]  
    >  Bei jedem Öffnen eines neuen PowerShell-Fensters müssen Sie das **FailoverClusters** -Modul importieren.  
  
##### <a name="related-content-powershell"></a>Verwandte Inhalte (PowerShell)  
  
-   [Clustering and High-Availability](https://techcommunity.microsoft.com/t5/failover-clustering/bg-p/FailoverClustering) (Clustering und hohe Verfügbarkeit) (Failoverclustering und Netzwerklastenausgleichs-Teamblog)  
  
-   [Erste Schritte mit Windows PowerShell auf einem Failovercluster](https://technet.microsoft.com/library/ee619762\(WS.10\).aspx)  
  
-   [Clusterressourcenbefehle und entsprechende Windows PowerShell-Cmdlets](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee619744(v=ws.10)#BKMK_resource)  
  
###  <a name="related-content-windows-system"></a><a name="RelatedContentWS"></a> Verwandte Inhalte (Windows-System)  
  
-   [Konfigurieren von DNS-Einstellungen in einem Failovercluster für mehrere Standorte](https://technet.microsoft.com/library/dd197562\(WS.10\).aspx)  
  
-   [DNS-Registrierung mit Netzwerknamenressource](https://techcommunity.microsoft.com/t5/failover-clustering/dns-registration-with-the-network-name-resource/ba-p/371482)  
  

##  <a name="sql-server-instance-prerequisites-and-restrictions"></a><a name="ServerInstance"></a> Voraussetzungen und Einschränkungen für SQL Server-Instanzen  
 Jede Verfügbarkeitsgruppe erfordert einen Satz Failoverpartner, die als *Verfügbarkeitsreplikate* bezeichnet und von Instanzen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]gehostet werden. Bei einer angegebenen Serverinstanz kann es sich um eine *eigenständige Instanz* oder eine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]*Failovercluster-Instanz* (FCI) handeln.  
  
 **In diesem Abschnitt:**  
  
-   [Prüfliste: Voraussetzungen](#PrerequisitesSI)  
  
-   [Threadverwendung durch Verfügbarkeitsgruppen](#ThreadUsage)  
  
-   [Berechtigungen](#PermissionsSI)  
  
-   [Verwandte Aufgaben](#RelatedTasksSI)  
  
-   [Verwandte Inhalte](#RelatedContentSI)  
  
###  <a name="checklist-prerequisites-server-instance"></a><a name="PrerequisitesSI"></a> Prüfliste: Voraussetzungen (Serverinstanz)  
  
|Voraussetzung|Links|  
|------------------|-----------|  
|Dieser Hostcomputer muss ein WSFC-Knoten sein. Die Instanzen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , die Verfügbarkeitsreplikate für eine angegebene Verfügbarkeitsgruppe hosten, befinden sich jeweils in einem separaten Knoten des Clusters. Eine Verfügbarkeitsgruppe kann sich während der Migration zu einem anderen Cluster vorübergehend über zwei Cluster erstrecken. SQL Server 2016 führt verteilte Verfügbarkeitsgruppen ein. In einer verteilten Verfügbarkeitsgruppe befinden sich zwei Verfügbarkeitsgruppen auf verschiedenen Clustern.|[Windows Server-Failoverclustering &#40;WSFC&#41; mit SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)<br /><br /> [Failoverclustering und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)<br/> <br/> [Verteilte Verfügbarkeitsgruppen (Always On-Verfügbarkeitsgruppen)](./distributed-availability-groups.md)|  
|Wenn eine Verfügbarkeitsgruppe mit Kerberos verwendet werden soll:<br /><br /> Alle Serverinstanzen, die ein Verfügbarkeitsreplikat für die Verfügbarkeitsgruppe hosten, müssen das gleiche SQL Server-Dienstkonto verwenden.<br /><br /> Der Domänenadministrator muss manuell einen Dienstprinzipalnamen (SPN) für Active Directory auf dem SQL Server-Dienstkonto beim virtuellen Netzwerknamen (VNN) des Verfügbarkeitsgruppenlisteners registrieren. Wenn der SPN auf keinem SQL Server-Dienstkonto registriert wird, treten bei der Authentifizierung Fehler auf.<br /><br /> <br /><br /> <b>\*\* Wichtig \*\*</b> : Wenn Sie das SQL Server-Dienstkonto ändern, muss der Domänenadministrator den SPN erneut manuell registrieren.|[Registrieren eines Dienstprinzipalnamens für Kerberos-Verbindungen](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)<br /><br /> **Kurze Erklärung:**<br /><br /> Kerberos und SPNs erzwingen die gegenseitige Authentifizierung. Dem Windows-Konto, das die SQL Server-Dienste startet, wird der SPN zugeordnet. Wenn die Registrierung des SPNs nicht richtig erfolgt oder dabei ein Fehler aufgetreten ist, kann die Windows-Sicherheitsschicht nicht das Konto bestimmen, das dem Dienstprinzipalname zugewiesen ist. Das bedeutet, die Kerberos-Authentifizierung kann nicht verwendet werden.<br /><br /> <br /><br /> Hinweis: Bei NTLM gibt es diese Anforderung nicht.|  
|Wenn Sie planen, eine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failoverclusterinstanz (FCI) zu verwenden, um ein Verfügbarkeitsreplikat zu hosten, muss gewährleistet sein, dass Sie die FCI-Einschränkungen verstehen und dass die FCI-Anforderungen erfüllt werden.|[Voraussetzungen und Anforderungen für das Hosten eines Verfügbarkeitsreplikats mithilfe einer SQL Server-Failoverclusterinstanz (FCI)](#FciArLimitations) (weiter unten in diesem Artikel)|  
|Auf jeder Serverinstanz muss die gleiche Version von SQL Server ausgeführt werden, um an einer Always On-Verfügbarkeitsgruppe teilzunehmen.|Editionen und unterstützte Funktionen von [SQL 2014](/previous-versions/sql/2014/getting-started/features-supported-by-the-editions-of-sql-server-2014?view=sql-server-2014&preserve-view=true), [SQL 2016](../../../sql-server/editions-and-components-of-sql-server-2016.md?view=sql-server-2016&preserve-view=true), [SQL 2017](../../../sql-server/editions-and-components-of-sql-server-2017.md?view=sql-server-2017&preserve-view=true).|  
|Alle Serverinstanzen, die Verfügbarkeitsreplikate für eine Verfügbarkeitsgruppe hosten, müssen die gleiche [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Sortierung verwenden.|[Festlegen oder Ändern der Serversortierung](../../../relational-databases/collations/set-or-change-the-server-collation.md)|  
|Aktivieren Sie die Funktion [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] auf jeder Serverinstanz, die ein Verfügbarkeitsreplikat für jede Verfügbarkeitsgruppe hostet. Auf einem angegebenen Computer können Sie so viele Serverinstanzen für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] aktivieren, wie Ihre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Installation unterstützt.|[Aktivieren und Deaktivieren von Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)<br /><br /> <br /><br /> <b>\*\*Wichtig\*\*</b>: Wenn Sie einen WSFC löschen und neu erstellen, müssen Sie die Funktion [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] auf jeder Serverinstanz, die auf dem ursprünglichen Cluster für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] aktiviert war, deaktivieren und erneut aktivieren.|  
|Jede Serverinstanz erfordert einen Datenbankspiegelungs-Endpunkt. Beachten Sie, dass dieser Endpunkt von allen Verfügbarkeitsreplikaten, Datenbank-Spiegelungspartnern und Zeugen auf der Serverinstanz gemeinsam verwendet wird.<br /><br /> Wenn eine Serverinstanz, die Sie zum Hosten eines Verfügbarkeitsreplikats auswählen, unter einem Domänenbenutzerkonto ausgeführt wird und noch keinen Datenbankspiegelungs-Endpunkt aufweist, kann der [Assistent für neue Verfügbarkeitsgruppen](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md) (oder [Assistent zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)) den Endpunkt erstellen und dem Dienstkonto der Serverinstanz die CONNECT-Berechtigung erteilen. Wenn der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienst jedoch als integriertes Konto, z. B. Lokales System, Lokaler Dienst oder Netzwerkdienst, oder als Nichtdomänenkonto ausgeführt wird, müssen Sie Zertifikate zur Endpunktauthentifizierung verwenden, und der Assistent kann keinen Datenbankspiegelungs-Endpunkt auf der Serverinstanz erstellen. In diesem Fall empfiehlt es sich, dass Sie die Datenbankspiegelungs-Endpunkte manuell erstellen, bevor Sie den Assistenten starten.<br /><br /> <br /><br /> <b>\*\* Sicherheitshinweis \*\*</b> : Die Transportsicherheit für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] entspricht derjenigen der Datenbankspiegelung.|[Der Datenbankspiegelungs-Endpunkt &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)<br /><br /> [Transportsicherheit für Datenbankspiegelung und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)|  
|Bevor Datenbanken, die FILESTREAM verwenden, zu einer Verfügbarkeitsgruppe hinzugefügt werden, stellen Sie sicher, dass FILESTREAM auf jeder Serverinstanz, die ein Verfügbarkeitsreplikat für die Verfügbarkeitsgruppe hostet, aktiviert worden ist.|[Aktivieren und Konfigurieren von FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)|  
|Bevor eigenständige Datenbanken einer Verfügbarkeitsgruppe hinzugefügt werden, muss gewährleistet sein, dass die Serveroption **eigenständige Datenbankauthentifizierung** auf jeder Serverinstanz, die ein Verfügbarkeitsreplikat für die Verfügbarkeitsgruppe hostet, auf **1** festgelegt wurde.|[Contained Database Authentication (Serverkonfigurationsoption)](../../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md)<br /><br /> [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)|  
  
###  <a name="thread-usage-by-availability-groups"></a><a name="ThreadUsage"></a> Threadverwendung durch Verfügbarkeitsgruppen  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] stellt die folgenden Anforderungen an Arbeitsthreads:  
  
-   Auf einer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz im Leerlauf verwendet [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 0 Threads.  
  
-   Die maximale Anzahl der von Verfügbarkeitsgruppen verwendeten Threads entspricht der Einstellung, die als maximale Anzahl von Serverthreads ('**maximale Anzahl von Workerthreads**') minus 40 konfiguriert wurde.  
  
-   Die auf einer bestimmten Serverinstanz gehosteten Verfügbarkeitsreplikate verwenden einen gemeinsamen Threadpool.  
  
     Threads werden bedarfsgesteuert wie folgt freigegeben:  
  
    -   In der Regel gibt es 3 bis 10 freigegebene Threads, diese Zahl kann sich jedoch abhängig von der Arbeitsauslastung des primären Replikats erhöhen.  
  
    -   Wenn ein bestimmter Thread eine Zeit lang im Leerlauf ist, wird er wieder im allgemeinen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Threadpool freigegeben. Normalerweise wird ein inaktiver Thread nach ~ 15 Sekunden Inaktivität freigegeben. Abhängig von der letzten Aktivität kann ein Thread jedoch länger im Leerlauf gehalten werden.  

    -   Eine SQL Server-Instanz verwendet bis zu 100 Threads für parallele Wiederholung für sekundäre Replikate. Für jede Datenbank wird bis zur Hälfte der Gesamtzahl von CPU-Kernen, jedoch nicht mehr als 16 Threads pro Datenbank verwendet. Überschreitet die Gesamtzahl von erforderlichen Threads für eine einzelne Instanz den Wert 100, verwendet SQL Server einen einzelnes Wiederholungsthread für jede verbleibende Datenbank. Serielle Wiederholungsthreads werden nach ca. 15 Sekunden Inaktivität freigegeben. 
     
-   Darüber hinaus verwenden Verfügbarkeitsgruppen nicht freigegebene Threads wie folgt:  
  
    -   Jedes primäre Replikat verwendet einen Protokollaufzeichnungsthread für jede primäre Datenbank. Außerdem verwendet es einen Protokollsendethread für jede sekundäre Datenbank. Protokollsendethreads werden nach ~ 15 Sekunden Inaktivität freigegeben.    
  
    -   Von einer Sicherung auf einem sekundären Replikat wird ein Thread auf dem primären Replikat für die Dauer des Sicherungsvorgangs beibehalten. 

-  In SQL Server 2019 wurde eine parallele Rollforwardphase für arbeitsspeicheroptimierte Datenbanken in Verfügbarkeitsgruppen eingeführt. In SQL Server 2016 und 2017 verwenden datenträgerbasierte Tabellen keine parallele Rollforwardphase, wenn eine Datenbank in einer Verfügbarkeitsgruppe ebenfalls arbeitsspeicheroptimiert ist. 
  
 Weitere Informationen finden Sie unter [Always On – HADRON Learning Series: Worker Pool Usage for HADRON Enabled Databases (Always On – HADRON-Lernreihe: Nutzung des Arbeitspools für HADRON-fähige Datenbanken)](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases) (ein CSS-[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Engineer-Blogbeitrag).  
  
###  <a name="permissions-server-instance"></a><a name="PermissionsSI"></a> Berechtigungen (Serverinstanz)  
  
|Aufgabe|Erforderliche Berechtigungen|  
|----------|--------------------------|  
|Erstellen des Endpunktes für die Datenbankspiegelung|Erfordert die CREATE ENDPOINT-Berechtigung oder die Mitgliedschaft in der festen Serverrolle **sysadmin** .  Erfordert zudem die CONTROL ON ENDPOINT-Berechtigung. Weitere Informationen finden Sie unter [GRANT (Endpunktberechtigungen) &#40;Transact-SQL&#41;](../../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).|  
|Aktivieren von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]|Erfordert auf dem lokalen Computer die Mitgliedschaft in der Gruppe **Administrator** und Vollzugriff auf den WSFC.|  
  
###  <a name="related-tasks-server-instance"></a><a name="RelatedTasksSI"></a> Verwandte Aufgaben (Serverinstanz)  
  
|Aufgabe|Artikel|  
|----------|-----------|  
|Bestimmen, ob ein Datenbankspiegelungs-Endpunkt vorhanden ist|[sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)|  
|Erstellen des Datenbankspiegelungs-Endpunkts (falls noch nicht vorhanden)|[Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)<br /><br /> [Verwenden von Zertifikaten für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)<br /><br /> [Erstellen eines Datenbankspiegelungs-Endpunkts für Always On-Verfügbarkeitsgruppen &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)|  
|Aktivieren von Verfügbarkeitsgruppen|[Aktivieren und Deaktivieren von Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)|  
  
###  <a name="related-content-server-instance"></a><a name="RelatedContentSI"></a> Verwandte Inhalte (Serverinstanz)  
  
-   [Always On – HADRON Learning Series: Worker Pool Usage for HADRON Enabled Databases (Always On – HADRON-Lernreihe: Nutzung des Arbeitspools für HADRON-fähige Datenbanken)](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
##  <a name="network-connectivity-recommendations"></a><a name="NetworkConnect"></a> Empfehlungen zur Netzwerkkonnektivität  
 Es wird dringend empfohlen, für die Kommunikation zwischen WSFC-Knoten die gleichen Netzwerkverbindungen zu verwenden wie für die Kommunikation zwischen Verfügbarkeitsreplikaten.  Bei Verwendung separater Netzwerkverbindungen kann ein unerwartetes Verhalten auftreten, wenn einige Verbindungen (wenn auch nur vorübergehend) ausfallen.  
  
 Damit eine Verfügbarkeitsgruppe automatisches Failover unterstützt, muss das sekundäre Replikat, das dem automatischen Failoverpartner entspricht, beispielsweise den Status SYNCHRONIZED aufweisen. Wenn bei der Netzwerkverbindung mit dem sekundären Replikat (wenn auch nur vorübergehend) ein Fehler auftritt, wechselt das Replikat in den Status UNSYNCHRONIZED und wird erst nach Wiederherstellen der Verbindung erneut synchronisiert. Wenn der WSFC ein automatisches Failover anfordert, während das sekundäre Replikat nicht synchronisiert ist, findet kein automatisches Failover statt.  
  
##  <a name="client-connectivity-support"></a><a name="ClientConnSupport"></a> Unterstützung für Clientkonnektivität  
 Weitere Informationen zur [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Unterstützung für Clientkonnektivität finden Sie unter [Always On-Clientkonnektivität &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
##  <a name="prerequisites-and-restrictions-for-using-a-sql-server-failover-cluster-instance-fci-to-host-an-availability-replica"></a><a name="FciArLimitations"></a> Voraussetzungen und Einschränkungen zum Hosten eines Verfügbarkeitsreplikats mithilfe einer SQL Server-Failoverclusterinstanz (FCI)  
 **In diesem Abschnitt:**  
  
-   [Einschränkungen](#RestrictionsFCI)  
  
-   [Prüfliste: Voraussetzungen](#PrerequisitesFCI)  
  
-   [Verwandte Aufgaben](#RelatedTasksFCIs)  
  
-   [Verwandte Inhalte](#RelatedContentFCIs)  
  
###  <a name="restrictions-fcis"></a><a name="RestrictionsFCI"></a> Einschränkungen (FCIs)  
  
> [!NOTE]  
> Failoverclusterinstanzen unterstützen freigegebene Clustervolumes (Cluster Shared Volumes, CSVs). Weitere Informationen zu CSVs finden Sie unter [Grundlegendes zu freigegebenen Clustervolumes in einem Failovercluster](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759255(v=ws.11)).  
  
-   **Ein FCI-Clusterknoten kann nur ein Replikat für eine bestimmte Verfügbarkeitsgruppe hosten:**  Wenn Sie ein Verfügbarkeitsreplikat in eine FCI hinzufügen, können die WSFC-Knoten, die mögliche FCI-Besitzer sind, kein anderes Replikat für dieselbe Verfügbarkeitsgruppe hosten.  Um potenzielle Konflikte zu vermeiden, empfiehlt es sich, mögliche Besitzer für die Failoverclusterinstanz zu konfigurieren. Damit kann verhindert werden, dass ein einzelner WSFC versucht, zwei Verfügbarkeitsreplikat für dieselbe Verfügbarkeitsgruppe zu hosten.
  
     Darüber hinaus muss jedes weitere Replikat von einer SQL Server 2016-Instanz gehostet werden, die sich auf einem anderen Clusterknoten im selben WSFC befindet. Die einzige Ausnahme besteht darin, dass sich eine Verfügbarkeitsgruppe während der Migration zu einem anderen Cluster vorübergehend auf zwei Cluster erstrecken kann. 

  >[!WARNING]
  > Die Verwendung des Failovercluster-Managers zum Verschieben einer *Failoverclusterinstanz*, die eine Verfügbarkeitsgruppe hostet, auf einen Knoten, der *bereits* ein Replikat derselben Verfügbarkeitsgruppe hostet, kann zum Verlust des Verfügbarkeitsgruppenreplikats führen, sodass dieses auf dem Zielknoten nicht online geschaltet werden kann. Ein einzelner Knoten eines Failoverclusters kann nicht mehr als ein Replikat für dieselbe Verfügbarkeitsgruppe hosten. Weitere Informationen dazu, wie eine solche Situation eintritt und wie sie gelöst werden kann, finden Sie im Blog [Replica unexpectedly dropped in availability group](/archive/blogs/alwaysonpro/issue-replica-unexpectedly-dropped-in-availability-group) (Replikat in Verfügbarkeitsgruppe unerwartet gelöscht). 

  
-   **FCIs unterstützen kein automatisches Failover durch Verfügbarkeitsgruppen:**  FCIs unterstützen kein automatisches Always On-Failover durch Verfügbarkeitsgruppen. Daher können die Verfügbarkeitsreplikate, die von einer FCI gehostet werden, ausschließlich für manuelle Failovers konfiguriert werden.  
  
-   **Ändern des FCI-Netzwerknamens:**  Falls Sie den Netzwerknamen einer FCI ändern müssen, die ein Verfügbarkeitsreplikat hostet, müssen Sie das Replikat aus seiner Verfügbarkeitsgruppe entfernen und das Replikat dann wieder der Verfügbarkeitsgruppe hinzufügen. Sie können das primäre Replikat nicht entfernen. Wenn Sie daher eine FCI umbenennen, die das primäre Replikat hostet, sollten Sie ein Failover zu einem sekundären Replikat ausführen und dann das vorherige primäre Replikat entfernen und wieder hinzufügen. Beachten Sie, dass durch Umbenennen einer FCI möglicherweise die URL ihres Datenbankspiegelungs-Endpunkts geändert wird. Stellen Sie beim Hinzufügen des Replikats sicher, dass Sie die aktuelle Endpunkt-URL angeben.  
  
###  <a name="checklist-prerequisites-fcis"></a><a name="PrerequisitesFCI"></a> Prüfliste: Voraussetzungen (FCIs)  
  
|Voraussetzung|Link|  
|------------------|----------|  
|Stellen Sie sicher, dass jede SQL Server-Failoverclusterinstanz (FCI) den erforderlichen gemeinsam verwendeten Speicher laut Standardinstallation der SQL Server-Failoverclusterinstanz besitzt.||  
  
###  <a name="related-tasks-fcis"></a><a name="RelatedTasksFCIs"></a> Verwandte Aufgaben (FCIs)  
  
|Aufgabe|Artikel|  
|----------|-----------|  
|Installieren eines SQL Server-Failoverclusters|[Erstellen eines neuen SQL Server-Failoverclusters &#40;Setup&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)|  
|Direktes Upgrade des vorhandenen SQL Server-Failoverclusters|[Aktualisieren einer SQL Server-Failoverclusterinstanz &#40;Setup&#41;](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)|  
|Beibehalten des vorhandenen SQL Server-Failoverclusters|[Hinzufügen oder Entfernen von Knoten in einem SQL Server-Failovercluster &#40;Setup&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)|  
  
###  <a name="related-content-fcis"></a><a name="RelatedContentFCIs"></a> Verwandte Inhalte (FCIs)  
  
-   [Failoverclustering und Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)  
  
-   [Always On-Architekturhandbuch: Erstellen einer Lösung für Hochverfügbarkeit und Notfallwiederherstellung unter Verwendung von Failoverclusterinstanzen und Verfügbarkeitsgruppen](/previous-versions/sql/sql-server-2012/jj215886(v=msdn.10))  
  
##  <a name="availability-group-prerequisites-and-restrictions"></a><a name="PrerequisitesForAGs"></a> Voraussetzungen und Einschränkungen für Verfügbarkeitsdatenbanken  
 **In diesem Abschnitt:**  
  
-   [Einschränkungen](#RestrictionsAG)  
  
-   [Anforderungen](#RequirementsAG)  
  
-   [Security](#SecurityAG)  
  
-   [Verwandte Aufgaben](#RelatedTasksAGs)  
  
###  <a name="restrictions-availability-groups"></a><a name="RestrictionsAG"></a> Einschränkungen (Verfügbarkeitsgruppen)  
  
-   **Verfügbarkeitsreplikate müssen auf verschiedenen Knoten eines WSFC gehostet werden:**  Für eine bestimmte Verfügbarkeitsgruppe müssen Verfügbarkeitsreplikate von Serverinstanzen gehostet werden, die in anderen Knoten desselben WSFC ausgeführt werden. Die einzige Ausnahme besteht darin, dass sich eine Verfügbarkeitsgruppe während der Migration zu einem anderen Cluster vorübergehend auf zwei Cluster erstrecken kann.  
  
    > [!NOTE]  
    >  Virtuelle Computer können auf demselben physischen Computer jeweils ein Verfügbarkeitsreplikat für dieselbe Verfügbarkeitsgruppe hosten, da jeder virtuelle Computer als separater Computer fungiert.  
  
-   **Eindeutiger Name der Verfügbarkeitsgruppe:**  Jeder Verfügbarkeitsgruppenname muss auf dem WSFC eindeutig sein. Die maximale Länge eines Verfügbarkeitsgruppennamens beträgt 128 Zeichen.  
  
-   **Verfügbarkeitsreplikate:**  Jede Verfügbarkeitsgruppe unterstützt ein primäres Replikat und bis zu acht sekundäre Replikate. Alle Replikate können im Modus für asynchrone Commits ausgeführt werden. Alternativ können bis zu fünf Replikate im Modus für synchrone Commits ausgeführt werden (ein primäres Replikat mit zwei synchronen sekundären Replikaten).  
  
-   **Maximale Anzahl von Verfügbarkeitsgruppen und Verfügbarkeitsdatenbanken pro Computer:** Die tatsächliche Anzahl der auf einem Computer (virtuell oder physisch) ausführbaren Datenbanken und Verfügbarkeitsgruppen richtet sich nach der Hardware und Workload, es gibt jedoch keine maximale Vorgabe. Microsoft hat bis zu 10 Verfügbarkeitsgruppen und 100 Datenbanken pro physischem Computer getestet, aber dies stellt keinen bindenden Grenzwert dar. Abhängig von der Hardwarespezifikation auf dem Server und dem Workload können Sie eine höhere Anzahl von Datenbanken und Verfügbarkeitsgruppen auf einer Instanz von SQL Server platzieren. Anzeichen für eine Systemüberlastung könnten u.a. zu wenige Arbeitsthreads, langsame Antwortzeiten für Verfügbarkeitsgruppen-Systemsichten und DMVs und/oder Systemspeicherabbilder bei angehaltenem Verteiler sein. Es wird empfohlen, die Umgebung unter produktionsähnlichen Bedingungen eingehend zu testen, um zu gewährleisten, dass das System maximale Arbeitsauslastungen im Rahmen Ihrer Anwendungs-SLAs bewältigen kann. Im Hinblick auf SLAs sollten sowohl die Auslastung unter Fehlerbedingungen als auch die erwarteten Antwortzeiten abgewogen werden.  
  
-   **Verwenden Sie den Failovercluster-Manager nicht, um Verfügbarkeitsgruppen zu bearbeiten**. Der Status einer SQL Server-Failoverclusterinstanz (FCI) wird von SQL Server und dem Windows-Failovercluster (WSFC) gemeinsam verwendet, wobei SQL Server ausführlichere Zustandsinformationen zu den Instanzen verwaltet als für den Cluster von Interesse sind. Das Verwaltungsmodell sieht so aus, dass SQL Server die Transaktionen steuern muss und dafür zuständig ist, die Sicht des Status des Clusters synchron mit der Sicht des Status von SQL Server zu halten. Wenn der Status des Clusters außerhalb von SQL Server geändert wird, kann die Synchronisierung des Status zwischen WSFC und SQL Server verloren gehen, was zu nicht vorhersehbarem Verhalten führen kann.
  
     Beispiel:  
  
    -   Ändern Sie keine Verfügbarkeitsgruppeneigenschaften, z. B. die möglichen Besitzer.  
  
    -   Verwenden Sie den Failovercluster-Manager nicht, um Failover für Verfügbarkeitsgruppen auszuführen. Sie müssen [!INCLUDE[tsql](../../../includes/tsql-md.md)] oder [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]verwenden.  
  
###  <a name="prerequisites-availability-groups"></a><a name="RequirementsAG"></a> Voraussetzungen (Verfügbarkeitsgruppen)  
 Beim Erstellen oder Neukonfigurieren einer Verfügbarkeitsgruppenkonfiguration müssen Sie folgende Anforderungen einhalten.  
  
|Voraussetzung|BESCHREIBUNG|  
|------------------|-----------------|  
|Wenn Sie planen, eine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failoverclusterinstanz (FCI) zu verwenden, um ein Verfügbarkeitsreplikat zu hosten, muss gewährleistet sein, dass Sie die FCI-Einschränkungen verstehen und dass die FCI-Anforderungen erfüllt werden.|[Voraussetzungen und Einschränkungen für das Hosten eines Verfügbarkeitsreplikats mithilfe einer SQL Server-Failoverclusterinstanz (FCI)](#FciArLimitations) (weiter oben in diesem Artikel)|  
  
###  <a name="security-availability-groups"></a><a name="SecurityAG"></a> Sicherheit (Verfügbarkeitsgruppen)  
  
-   Die Sicherheit wird vom WSFC vererbt. WSFC bieten zwei Sicherheitsebenen mit der Granularität eines kompletten Clusters:  
  
    -   Schreibgeschützter Zugriff  
  
    -   Vollzugriff  
  
         [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] benötigt Vollzugriff. Durch Aktivieren von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] auf einer Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] wird der Vollzugriff auf den Cluster erteilt (über Dienst-SID).  
  
         Sie können im Cluster-Manager die Sicherheit für eine Serverinstanz nicht direkt hinzufügen oder entfernen. Um Clustersicherheitssitzungen zu verwalten, verwenden Sie den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Konfigurations-Manager oder die WMI-Entsprechung von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Jede Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] muss über Berechtigungen zum Zugreifen auf die Registrierung, den Cluster usw. verfügen.  
  
-   Es wird empfohlen, dass Sie eine Verschlüsselung für Verbindungen zwischen Serverinstanzen verwenden, die [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Verfügbarkeitsreplikate hosten.  
  
#### <a name="permissions-availability-groups"></a>Berechtigungen (Verfügbarkeitsgruppen)  
  
|Aufgabe|Erforderliche Berechtigungen|  
|----------|--------------------------|  
|Erstellen einer Verfügbarkeitsgruppe|Erfordert die Mitgliedschaft in der festen Serverrolle **sysadmin** und die CREATE AVAILABILITY GROUP-Serverberechtigung, ALTER ANY AVAILABILITY GROUP-Berechtigung oder CONTROL SERVER-Berechtigung.|  
|Ändern einer Verfügbarkeitsgruppe|Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung.<br /><br /> Außerdem erfordert das Verknüpfen einer Datenbank mit einer Verfügbarkeitsgruppe die Mitgliedschaft in der festen **db_owner** -Datenbankrolle.|  
|Löschen einer Verfügbarkeitsgruppe|Erfordert die ALTER AVAILABILITY GROUP-Berechtigung für die Verfügbarkeitsgruppe, die CONTROL AVAILABILITY GROUP-Berechtigung, die ALTER ANY AVAILABILITY GROUP-Berechtigung oder die CONTROL SERVER-Berechtigung. Um eine Verfügbarkeitsgruppe zu löschen, die nicht am lokalen Replikatspeicherort gehostet wird, benötigen Sie die CONTROL SERVER-Berechtigung oder die CONTROL-Berechtigung für diese Verfügbarkeitsgruppe.|  
  
###  <a name="related-tasks-availability-groups"></a><a name="RelatedTasksAGs"></a> Verwandte Aufgaben (Verfügbarkeitsgruppen)  
  
|Aufgabe|Artikel|  
|----------|-----------|  
|Erstellen einer Verfügbarkeitsgruppe|[Verwenden der Verfügbarkeitsgruppe (Assistent für neue Verfügbarkeitsgruppen)](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)<br /><br /> [Erstellen einer Verfügbarkeitsgruppe (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)<br /><br /> [Erstellen einer Verfügbarkeitsgruppe (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)<br /><br /> [Angeben der Endpunkt-URL beim Hinzufügen oder Ändern eines Verfügbarkeitsreplikats &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)|  
|Ändern der Anzahl der Verfügbarkeitsreplikate|[Hinzufügen eines sekundären Replikats zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)<br /><br /> [Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)<br /><br /> [Entfernen einer sekundären Replikats aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)|  
|Erstellen eines Verfügbarkeitsgruppenlisteners|[Erstellen oder Konfigurieren eines Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)|  
|Löschen einer Verfügbarkeitsgruppe|[Entfernen einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)|  
  
##  <a name="availability-database-prerequisites-and-restrictions"></a><a name="PrerequisitesForDbs"></a> Voraussetzungen und Einschränkungen für Verfügbarkeitsdatenbanken  
 Damit einer Verfügbarkeitsgruppe eine Datenbank hinzugefügt werden kann, muss sie folgenden Voraussetzungen und Einschränkungen entsprechen:  
  
 **In diesem Abschnitt:**  
  
-   [Anforderungen](#RequirementsDb)  
  
-   [Einschränkungen](#RestrictionsDb)  
  
-   [Empfehlungen für Computer, die Verfügbarkeitsreplikate (Windows-System) hosten](#TDEdbs)  
  
-   [Berechtigungen](#PermissionsDbs)  
  
-   [Verwandte Aufgaben](#RelatedTasksADb)  
  
###  <a name="checklist-requirements-availability-databases"></a><a name="RequirementsDb"></a> Prüfliste: Anforderungen (Verfügbarkeitsdatenbanken)  
 Damit eine Datenbank einer Verfügbarkeitsgruppe hinzugefügt zu werden, müssen folgende Bedingungen für die Datenbank zutreffen:  
  
|Requirements (Anforderungen)|Link|  
|------------------|----------|  
|Die Datenbank muss eine Benutzerdatenbank sein. Systemdatenbanken können nicht zu einer Verfügbarkeitsgruppe gehören.||  
|Die Datenbank muss sich auf der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , auf der Sie die Verfügbarkeitsgruppe erstellen, und die Serverinstanz muss darauf zugreifen können.||  
|Die Datenbank muss eine Datenbank mit Lese-/Schreibzugriff sein. Schreibgeschützte Datenbanken können nicht zu einer Verfügbarkeitsgruppe hinzugefügt werden.|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**is_read_only** = 0)|  
|Die Datenbank muss eine Mehrbenutzerdatenbank sein.|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**user_access** = 0)|  
|Verwenden Sie nicht AUTO_CLOSE.|[sys.Databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**is_auto_close_on** = 0)|  
|Verwenden Sie das vollständige Wiederherstellungsmodell (auch bekannt als der vollständige Wiederherstellungsmodus).|[sys.Databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**recovery_model** = 1)|  
|Die Datenbank muss mindestens über eine vollständige Datenbanksicherung verfügen.<br /><br /> Hinweis: Eine vollständige Sicherung ist erforderlich, um die volle Protokollkette für eine vollständige Wiederherstellung auszulösen, nachdem eine Datenbank auf den vollständigen Wiederherstellungsmodus festgelegt wurde.|[Erstellen einer vollständigen Datenbanksicherung &#40;SQL Server&#41;](../../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)|  
|Sie darf zu keiner vorhandenen Verfügbarkeitsgruppe gehören.|[sys.Databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**group_database_id** = NULL)|  
|Die Datenbank darf nicht für die Datenbankspiegelung konfiguriert sein.|[sys.database_mirroring](../../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md) (Wenn die Datenbank nicht an der Spiegelung beteiligt ist, haben alle Spalten mit dem Präfix „mirroring_“ den Wert NULL.)|  
|Bevor Sie eine Datenbank, die FILESTREAM verwendet, zu einer Verfügbarkeitsgruppe hinzufügen, muss gewährleistet sein, dass FILESTREAM auf jeder Serverinstanz aktiviert ist, die ein Verfügbarkeitsreplikat für die Verfügbarkeitsgruppe hostet oder hosten wird.|[Aktivieren und Konfigurieren von FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)|  
|Vor dem Hinzufügen einer enthaltenen Datenbank zu einer Verfügbarkeitsgruppe muss gewährleistet sein, dass die Serveroption **eigenständige Datenbankauthentifizierung** auf jeder Serverinstanz, die ein Verfügbarkeitsreplikat für die Verfügbarkeitsgruppe hostet oder hosten wird, auf **1** gesetzt ist.|[Contained Database Authentication (Serverkonfigurationsoption)](../../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md)<br /><br /> [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)|  
  
> [!NOTE]  
>  [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] funktioniert mit jedem unterstützten Datenbank-Kompatibilitätsgrad.  
  
###  <a name="restrictions-availability-databases"></a><a name="RestrictionsDb"></a> Einschränkungen (Verfügbarkeitsdatenbanken)  
  
-   Falls sich der Dateipfad (einschließlich des Laufwerkbuchstabens) einer sekundären Datenbank vom Pfad der entsprechenden primären Datenbank unterscheidet, gelten folgende Einschränkungen.  
  
    -   **[!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)]/[!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)]:**  Die Option **Vollständig** wird nicht unterstützt (auf der Seite [Anfängliche Datensynchronisierung auswählen](../../../database-engine/availability-groups/windows/select-initial-data-synchronization-page-always-on-availability-group-wizards.md)).  
  
    -   **RESTORE WITH MOVE:**  Zum Erstellen der sekundären Datenbanken müssen die Datenbankdateien auf jeder Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], die ein sekundäres Replikat hostet, auf RESTORED WITH MOVE festgelegt sein.  
  
    -   **Auswirkungen auf add-file-Vorgänge:**  Ein späterer add-file-Vorgang auf dem primären Replikat schlägt auf den sekundären Datenbanken möglicherweise fehl. Dieser Fehler kann bewirken, dass die sekundären Datenbanken angehalten werden. Dies bewirkt dann, dass die sekundären Replikate den Status NOT SYNCHRONIZING erhalten.  
  
        > [!NOTE]  
        >  Informationen zum Reagieren auf einen Dateihinzufügungsvorgang, bei dem ein Fehler aufgetreten ist, finden Sie unter [Problembehandlung bei einem fehlgeschlagenen Vorgang zum Hinzufügen einer Datei &#40;Always On-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md).  
  
-   Sie können keine Datenbank löschen, die aktuell einer Verfügbarkeitsgruppe angehört.  
  
###  <a name="follow-up-for-tde-protected-databases"></a><a name="TDEdbs"></a> Nachverfolgung für TDE-geschützte Datenbanken  
 Wenn Sie die transparente Datenverschlüsselung (TDE) verwenden, muss das Zertifikat oder der asymmetrische Schlüssel zum Erstellen und Entschlüsseln weiterer Schlüssel auf jeder Serverinstanz, die ein Verfügbarkeitsreplikat für die Verfügbarkeitsgruppe hostet, identisch sein. Weitere Informationen finden Sie unter [Verschieben einer TDE-geschützten Datenbank auf einen anderen SQL-Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md).  
  
###  <a name="permissions-availability-databases"></a><a name="PermissionsDbs"></a> Berechtigungen (Verfügbarkeitsdatenbanken)  
 Erfordert die ALTER-Berechtigung für die Datenbank.  
  
###  <a name="related-tasks-availability-databases"></a><a name="RelatedTasksADb"></a> Verwandte Aufgaben (Verfügbarkeitsdatenbanken)  
  
|Aufgabe|Artikel|  
|----------|-----------|  
|Vorbereiten einer sekundären Datenbank (manuell)|[Manuelles Vorbereiten einer sekundären Datenbank auf eine Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)|  
|Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe (manuell)|[Verknüpfen einer sekundären Datenbank mit einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)|  
|Ändern der Anzahl der Verfügbarkeitsdatenbanken|[Hinzufügen einer Datenbank zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)<br /><br /> [Entfernen einer sekundären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)<br /><br /> [Entfernen einer primären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)|  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   [Microsoft SQL Server Always On-Lösungshandbuch zu hoher Verfügbarkeit und Notfallwiederherstellung](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
-   [SQL Server Always On Team Blog: The official SQL Server Always On Team Blog (SQL Server Always On-Teamblog: Der offizielle SQL Server Always On-Teamblog)](/archive/blogs/sqlalwayson/)  
  
-   [Always On – HADRON Learning Series: Worker Pool Usage for HADRON Enabled Databases (Always On – HADRON-Lernreihe: Nutzung des Arbeitspools für HADRON-fähige Datenbanken)](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Failoverclustering und Always On-Verfügbarkeitsgruppen (SQL Server)](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)   
 [AlwaysOn-Clientkonnektivität &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)  
  
    
  
--------------------------------------------------
