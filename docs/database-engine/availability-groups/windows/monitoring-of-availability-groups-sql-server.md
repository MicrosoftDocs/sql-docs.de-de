---
title: Tools zum Überwachen von Verfügbarkeitsgruppen
description: 'In diesem Artikel wird auf die verschiedenen Tools zum Überwachen der Leistung und Integrität von Always On-Verfügbarkeitsgruppen verwiesen. '
ms.custom: seodec18
ms.date: 06/08/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], troubleshooting
ms.assetid: 1d5e3291-0d0a-45a1-88e5-1fc242d17210
author: cawrites
ms.author: chadam
ms.openlocfilehash: 075f4a957764038b56d61b7be64fd31acb9e13c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337956"
---
# <a name="tools-to-monitor-always-on-availability-groups"></a>Tools zum Überwachen von Always On-Verfügbarkeitsgruppen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Sie können zum Überwachen der Eigenschaften und des Status einer Always On-Verfügbarkeitsgruppe die folgenden Tools verwenden:  
  
|Tool|Kurzbeschreibung|Links|  
|----------|-----------------------|-----------|  
|Systemcenter-Überwachungspaket für SQL Server|Das Überwachungspaket für SQL Server (SQLMP) ist die empfohlene Lösung, mit der IT-Administratoren Verfügbarkeitsgruppen sowie Verfügbarkeitsreplikate und Verfügbarkeitsdatenbanken überwachen. Die auf [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] abgestimmten Überwachungsfunktionen umfassen:<br /><br /> Automatische Erkennung von Verfügbarkeitsgruppen, Verfügbarkeitsreplikaten und Verfügbarkeitsdatenbank unter Hunderten von Computern. Dies ermöglicht Ihnen die einfache Nachverfolgung des [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Inventars.<br /><br /> Umfassende Warnungs- und Tickererstellungsfunktionen durch den System Center Operations Manager (SCOM). Diese Funktionen liefern detaillierte Informationen zur schnelleren Behebung eines Problems.<br /><br /> Eine benutzerdefinierte Erweiterung der Always On-Systemüberwachung unter Verwendung der richtlinienbasierten Verwaltung.<br /><br /> Integritätsrollups von Verfügbarkeitsdatenbanken auf Verfügbarkeitsreplikate.<br /><br /> Benutzerdefinierte Aufgaben zur Verwaltung von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] über die System Center Operations Manager-Konsole.|Hier können Sie das Überwachungspaket (SQLServerMP.msi) und das *SQL Server Management Pack-Handbuch für System Center Operations Manager* (SQLServerMPGuide.doc) herunterladen:<br /><br /> [Systemcenter-Überwachungspaket für SQL Server](https://www.microsoft.com/download/details.aspx?displaylang=en&id=10631)|  
|[!INCLUDE[tsql](../../../includes/tsql-md.md)]|[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Katalog und dynamische Verwaltungssichten stellen zahlreiche Informationen zu den Verfügbarkeitsgruppen und deren Replikaten, Datenbanken, Listenern und zur WSFC-Clusterumgebung bereit.|[Überwachen von Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)|  
|[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]|Der Bereich **Details zum Objekt-Explorer** zeigt grundlegende Informationen zu den Verfügbarkeitsgruppen an, die auf der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] gehostet werden, mit der eine Verbindung besteht.<br /><br /> **\*\* Tipp \*\*** Verwenden Sie diesen Bereich, um mehrere Verfügbarkeitsgruppen, Replikate oder Datenbanken auszuwählen und routinemäßige administrative Aufgaben auf den ausgewählten Objekten auszuführen, beispielsweise das Entfernen von mehreren Verfügbarkeitsreplikaten oder Datenbanken aus einer Verfügbarkeitsgruppe.|[Verwenden der Details zum Objekt-Explorer zum Überwachen von Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md)|  
|[!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]|**Eigenschaftendialogfelder** ermöglichen es, die Eigenschaften von Verfügbarkeitsgruppen, Replikaten oder Listenern anzuzeigen und, in einigen Fällen, deren Werte zu ändern.|-   [Anzeigen von Verfügbarkeitsgruppeneigenschaften &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)<br />-   [Anzeigen von Verfügbarkeitsreplikateigenschaften &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)<br />-   [Anzeigen von Eigenschaften des Verfügbarkeitsgruppenlisteners &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-listener-properties-sql-server.md)|  
|Systemmonitor|Das Leistungsobjekt **SQL Server:Verfügbarkeitsreplikat** enthält Leistungsindikatoren, die Informationen zu Verfügbarkeitsreplikaten bereitstellen.|[SQL Server, Verfügbarkeitsreplikat](../../../relational-databases/performance-monitor/sql-server-availability-replica.md)|  
|Systemmonitor|Das Leistungsobjekt **SQL Server:Datenbankreplikat** beinhaltet Leistungsindikatoren, die Informationen zu den sekundären Verfügbarkeitsdatenbanken eines bestimmten sekundären Replikats bereitstellen.<br /><br /> Das **SQLServer:Databases** -Objekt in SQL Server enthält u. a. Leistungsindikatoren zum Überwachen von Transaktionsprotokollaktivitäten. Die folgenden Indikatoren sind besonders für die Überwachung der Transaktionsprotokollaktivität von Verfügbarkeitsdatenbanken relevant: **Schreibdauer für Protokollleerungen (ms)** , **Protokollleerungen/Sekunde**, **Protokollpool-Cachefehlversuche/Sekunde**, **Protokollpool-Lesevorgänge auf dem Datenträger/Sekunde** und **Protokollpoolanforderungen/Sekunde**.|[SQL Server, Datenbankreplikat](../../../relational-databases/performance-monitor/sql-server-database-replica.md) und [SQL Server, Datenbank-Objekt](../../../relational-databases/performance-monitor/sql-server-databases-object.md)|  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   **Blogs:**  
  
     [The Always On Health Model Part 1 -- Health Model Architecture (Das Always On-Zustandsmodell Teil 1 – Zustandsmodellarchitektur)](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-1-health-model-architecture)  
  
     [The Always On Health Model Part 2 -- Extending the Health Model (Das Always On-Zustandsmodell Teil 2 – Erweitern des Zustandsmodells)](/archive/blogs/sqlalwayson/the-alwayson-health-model-part-2-extending-the-health-model)  
  
     [Monitoring Always On Health with PowerShell - Part 1: Basic Cmdlet Overview (Überwachen des Always On-Zustands mit PowerShell - Teil 1: Übersicht über grundlegende Cmdlets)](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-1-basic-cmdlet-overview)  
  
     [Monitoring Always On Health with PowerShell - Part 2: Advanced Cmdlet Usage (Überwachen des Always On-Zustands mit PowerShell - Teil 2: Verwendung erweiterter Cmdlets)](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-2-advanced-cmdlet-usage)  
  
     [Monitoring Always On Health with PowerShell - Part 3 : A Simple Monitoring Application (Überwachen des Always On-Zustands mit PowerShell - Teil 3: Eine einfache Überwachungsanwendung)](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-3-a-simple-monitoring-application)  
  
     [Monitoring Always On Health with PowerShell - Part 4 : Integration with SQL Server Agent (Überwachen des Always On-Zustands mit PowerShell - Teil 4: Integration in SQL Server-Agent)](/archive/blogs/sqlalwayson/monitoring-alwayson-health-with-powershell-part-4-integration-with-sql-server-agent)  
  
     [SQL Server Always On Team Blogs: The official SQL Server Always On Team Blog (SQL Server Always On-Teamblogs: Der offizielle SQL Server Always On-Teamblog)](/archive/blogs/sqlalwayson/)  
  
     [CSS SQL Server-Technikblogs](/archive/blogs/psssql/)  
  
-   **Whitepaper:**  
  
     [Microsoft-Whitepapers für SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Whitepapers des SQL Server-Kundenberatungsteams](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten Always On-Verfügbarkeitsgruppen &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Dynamische Verwaltungssichten und -funktionen für Always On-Verfügbarkeitsgruppen (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Flexible Failoverrichtlinie für automatisches Failover einer Verfügbarkeitsgruppe (SQL Server)](./configure-flexible-automatic-failover-policy.md)   
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Automatische Seitenreparatur (Verfügbarkeitsgruppen: Datenbankspiegelung)](../../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)   
 [Verwenden des AlwaysOn-Dashboards &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
