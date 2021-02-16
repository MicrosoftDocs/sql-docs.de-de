---
title: Aktivieren der Verfügbarkeitsgruppen-Funktion für eine SQL Server-Instanz
description: Beschreibt, wie das Feature für Always On-Verfügbarkeitsgruppen für eine SQL Server-Instanz aktiviert wird.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], server instance
- Availability Groups [SQL Server], about
ms.assetid: fad8db32-593e-49d5-989c-39eb8399c416
author: cawrites
ms.author: chadam
ms.openlocfilehash: d40b3e89b9250b148f59ed78765fff9e2eaf47df
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349315"
---
# <a name="enable-the-always-on-availability-group-feature-for-a-sql-server-instance"></a>Aktivieren des Features für Always On-Verfügbarkeitsgruppen für eine SQL Server-Instanz
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Dieses Thema enthält Informationen zu den Anforderungen für die Konfiguration einer Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] zur Unterstützung von AlwaysOn-Verfügbarkeitsgruppen [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  Wichtige Informationen zu erforderlichen [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Komponenten und Einschränkungen für WSFC-Knoten (Windows Server-Failoverclustering) und für Instanzen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
   
##  <a name="terms-and-definitions"></a><a name="TermsAndDefinitions"></a> Begriffe und Definitionen  
 [AlwaysOn-Verfügbarkeitsgruppen](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
 Eine Lösung für hohe Verfügbarkeit und Notfallwiederherstellung, die als Ersatz für die Datenbankspiegelung auf Unternehmensebene verwendet werden kann. Eine *Verfügbarkeitsgruppe* unterstützt eine Failoverumgebung für einen diskreten Satz von Benutzerdatenbanken (als *Verfügbarkeitsdatenbanken* bezeichnet), die zusammen ein Failover ausführen.  
  
 Verfügbarkeitsreplikat  
 Eine Instanziierung einer Verfügbarkeitsgruppe, die von einer bestimmten Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] gehostet wird und eine lokale Kopie jeder Verfügbarkeitsdatenbank beibehält, die zur Verfügbarkeitsgruppe gehört. Zwei Typen von Verfügbarkeitsreplikaten sind vorhanden: ein einzelnes *primäres Replikat* und ein bis vier *sekundäre Replikate*. Die Serverinstanzen, die die Verfügbarkeitsreplikate für eine angegebene Verfügbarkeitsgruppe hosten, müssen sich auf verschiedenen Konten eines einzelnen WSFC-Clusters (Windows Server-Failoverclustering) befinden.  
  
 [Datenbank-Spiegelungsendpunkt](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
 Ein Endpunkt ist ein SQL Server-Objekt, mit dessen Hilfe SQL Server über das Netzwerk kommunizieren kann. Um an einer Datenbankspiegelung und/oder [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] teilnehmen zu können, ist für eine Serverinstanz ein spezieller, dedizierter Endpunkt erforderlich. Alle Spiegelungs- und Verfügbarkeitsgruppenverbindungen auf einer Serverinstanz verwenden denselben Datenbankspiegelungs-Endpunkt. Dieser Endpunkt ist ein auf einen bestimmten Zweck ausgerichteter Endpunkt, der ausschließlich dafür verwendet wird, diese Verbindungen von anderen Serverinstanzen zu empfangen.  
  
##  <a name="to-configure-a-server-instance-to-support-always-on-availability-groups"></a><a name="ConfigSI"></a> So konfigurieren Sie eine Serverinstanz zur Unterstützung von Always On-Verfügbarkeitsgruppen  
 Um [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]zu unterstützen, muss sich eine Serverinstanz in einem Knoten im WSFC-Failovercluster befinden, der die Verfügbarkeitsgruppe hostet. Sie muss [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -aktiviert sein und über einen Datenbankspiegelungs-Endpunkt verfügen.  
  
1.  Aktivieren Sie die Funktion Always On-Verfügbarkeitsgruppen auf jeder Serverinstanz, die an einer oder mehreren Verfügbarkeitsgruppen teilnehmen soll. Eine bestimmte Serverinstanz kann nur ein einzelnes Verfügbarkeitsreplikat für eine bestimmte Verfügbarkeitsgruppe hosten.  
  
2.  Stellen Sie sicher, dass die Serverinstanz einen Datenbankspiegelungs-Endpunkt besitzt.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **So aktivieren Sie Always On-Verfügbarkeitsgruppen**  
  
-   [Aktivieren und Deaktivieren von Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
 **So bestimmen Sie, ob ein Datenbankspiegelungs-Endpunkt vorhanden ist**  
  
-   [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
 **So erstellen Sie einen Datenbankspiegelungs-Endpunkt**  
  
-   [Erstellen eines Datenbankspiegelungs-Endpunkts für Always On-Verfügbarkeitsgruppen &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Ermöglichen des Verwendens von Zertifikaten für ausgehende Verbindungen für einen Datenbankspiegelungs-Endpunkt &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   **Blogs:**  
  
     [Always On - HADRON Learning Series: Worker Pool Usage for HADRON Enabled Databases (Always On - HADRON-Lernreihe: Nutzung des Arbeitsthreadpools für HADRON-fähige Datenbanken)](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases)  
  
     [SQL Server Always On Team Blogs: The official SQL Server Always On Team Blog (SQL Server Always On-Teamblogs: Der offizielle SQL Server Always On-Teamblog)](/archive/blogs/sqlalwayson/)  
  
     [CSS SQL Server-Technikblogs](/archive/blogs/psssql/)  
  
-   **Videos:**  
  
     [Microsoft SQL Server Codename „Denali“ Always On-Reihe, Teil 1:Introducing the Next Generation High Availability Solution (Einführung in die nächste Generation von Lösungen mit hoher Verfügbarkeit)](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)  
  
     [Microsoft SQL Server Codename „Denali“ Always On-Reihe, Teil 2: Building a Mission-Critical High Availability Solution Using Always On (Erstellen einer unternehmenswichtigen Lösung für hohe Verfügbarkeit mit Always On)](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **Whitepaper:**  
  
     [Microsoft SQL Server Always On-Lösungshandbuch zu hoher Verfügbarkeit und Notfallwiederherstellung](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Microsoft-Whitepapers für SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Whitepapers des SQL Server-Kundenberatungsteams](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Der Datenbankspiegelungs-Endpunkt &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Always On-Verfügbarkeitsgruppen: Interoperabilität (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [Failoverclustering und Always On-Verfügbarkeitsgruppen (SQL Server)](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)   
 [Windows Server-Failoverclustering &#40;WSFC&#41; mit SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)   
 [AlwaysOn-Failoverclusterinstanzen &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)  
  
