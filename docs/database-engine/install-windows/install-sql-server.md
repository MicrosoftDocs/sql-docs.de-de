---
title: Installationsleitfaden für SQL Server
description: Hier finden Sie einen Index der Inhalte, die Sie beim Installieren von SQL Server und zugehörigen Komponenten mit Optionen wie dem Installations-Assistenten, der Eingabeaufforderung oder Sysprep unterstützen.
ms.custom: ''
ms.date: 11/14/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- AdventureWorks sample database
- installing SQL Server, preparing to install
- installation [SQL Server]
ms.assetid: 0300e777-d56b-4d10-9c33-c9ebd2489ee5
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8f123235f263abd05878e7be422c13fcf84825e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353088"
---
# <a name="sql-server-installation-guide"></a>Installationsleitfaden für SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Dieser Artikel ist ein Inhaltsverzeichnis für Anleitungen zum Installieren von SQL Server unter Windows.

Informationen zu weiteren Bereitstellungsszenarios finden Sie unter:

- [Linux](../../linux/sql-server-linux-setup.md)
- [Docker-Container](../../linux/sql-server-linux-docker-container-deployment.md)
- [Kubernetes: Big Data-Cluster](../../big-data-cluster/deploy-get-started.md)

Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] ist [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] nur noch als 64-Bit-Anwendung verfügbar. Hier erhalten Sie wichtige Informationen zum Erhalt und zur Installation von SQL Server.

## <a name="getting-started"></a>Erste Schritte
  
* **Editionen und Features:** Überprüfen Sie die unterstützten Features für die verschiedenen Editionen und Versionen von SQL Server, um zu ermitteln, welche am besten zu Ihren geschäftlichen Anforderungen passt. 
    - [[!INCLUDE[ss2019](../../includes/sssql19-md.md)]](~/sql-server/editions-and-components-of-sql-server-version-15.md).  
    - [[!INCLUDE[ss2017](../../includes/sssql17-md.md)]](~/sql-server/editions-and-components-of-sql-server-2017.md).  
    - [[!INCLUDE[ss2016](../../includes/sssql16-md.md)]](~/sql-server/editions-and-components-of-sql-server-2016.md).  
    - [[!INCLUDE[ss2014](../../includes/sssql14-md.md)]](https://docs.microsoft.com/previous-versions/sql/2014/getting-started/features-supported-by-the-editions-of-sql-server-2014)

*  **Anforderungen:** Überprüfen Sie die Hardware- und Software-Installationsanforderungen für [SQL Server 2016 und 2017](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md), [SQL Server 2019](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md) oder [SQL Server für Linux](../../linux/sql-server-linux-setup.md) sowie die Systemkonfigurationsprüfungen und Sicherheitsaspekte unter [Planen einer SQL Server-Installation](../../sql-server/install/planning-a-sql-server-installation.md). 


  
* **Beispieldatenbanken und Beispielcode**: 
    * Diese sind standardmäßig nicht als Teil der SQL Server-Installation installiert, können jedoch heruntergeladen werden. 
    * Weitere Informationen zur Installation der SQL Server-Editionen außer Express Edition finden Sie unter [SQL-Beispiele](../../samples/sql-samples-where-are.md).
    

## <a name="installation-media"></a>Installationsmedien

Der Downloadspeicherort für [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ist abhängig von der Edition.

* **SQL Server Enterprise-, Standard- und Express-Editionen** werden für die Verwendung in einer Produktionsumgebung lizenziert. Wenden Sie sich für die Installation von Medien für die Enterprise- und Standard-Editionen an Ihren Softwareanbieter. Informationen zum Kauf und ein Verzeichnis mit Microsoft-Partner finden Sie auf der [Seite für Microsoft-Lizenzierungen](https://www.microsoft.com/licensing/product-licensing/sql-server).
* [Aktuelle kostenlose Version](https://www.microsoft.com/sql-server/sql-server-downloads)
* [Andere kostenlose Versionen](https://www.microsoft.com/evalcenter/evaluate-sql-server)


Weitere SQL Server-Komponenten finden Sie hier: 

* [Alle kumulativen Updates](https://sqlserverbuilds.blogspot.com/)
* [SQL Server Reporting Services](https://www.microsoft.com/download/details.aspx?id=100122). 
* [SQL Server Management Studio](https://aka.ms/ssmsfullsetup)
* [Azure Data Studio](https://go.microsoft.com/fwlink/?linkid=2109256)


## <a name="considerations"></a>Überlegungen

-   Bei der Installation tritt ein Fehler auf, wenn Sie das Setup über eine Remotedesktopverbindung mit den Medien auf einer lokalen Ressource im RDC-Client starten. Für die Remoteinstallation müssen sich die Medien auf einer Netzwerkfreigabe oder lokal auf dem physischen oder virtuellen Computer befinden. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installationsmedien befinden sich entweder auf einer Netzwerkfreigabe, einem zugeordneten Laufwerk, einem lokalen Laufwerk oder werden einem virtuellen Computer als ISO zur Verfügung gestellt.  
  
  
-   Beim[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup werden die folgenden vom Produkt benötigten Softwarekomponenten installiert:  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client    
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setuphilfsdateien  

## <a name="sql-server-installation"></a>Installation von SQL Server


|Artikel|BESCHREIBUNG|  
|-----------|-----------------|  
|[Installations-Assistent](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)|Informationen zur Installation von SQL Server mithilfe der grafischen Benutzeroberfläche des Installations-Assistenten über die Setupdatei „setup.exe“ |  
|[Eingabeaufforderung](./install-sql-server-from-the-command-prompt.md)|Beispielsyntax und Installationsparameter für die Ausführung einer SQL Server-Installation über die Befehlszeile | 
|[Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md)|Installation von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] unter Windows Server Core|  
|[Überprüfen der Parameter für die Systemkonfigurationsprüfung](../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)|Erläutert die Funktion der Systemkonfigurationsprüfung (System Configuration Checker, SCC).|   
|[Konfigurationsdatei](./install-sql-server-using-a-configuration-file.md)|Beispielsyntax und Installationsparameter zum Ausführen eines unbeaufsichtigten Setups mithilfe einer Konfigurationsdatei|  
|[SysPrep](../../database-engine/install-windows/install-sql-server-using-sysprep.md)|Beispielsyntax und Installationsparameter zum Ausführen eines unbeaufsichtigten Setups mithilfe von SysPrep|
|[Hinzufügen von Features zu einer Instanz](./add-features-to-an-instance-of-sql-server-setup.md)|Aktualisieren von Komponenten mithilfe einer vorhandenen Instanz von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|  
|[SQL Server-Failoverclusterinstallation](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)| Installieren einer SQL Server-Failoverclusterinstanz  | 
|[Korrigieren einer fehlgeschlagenen Installation von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)|Reparieren eine beschädigten [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]-Installation|  
|[Umbenennen eines Computers mit SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)|Aktualisieren der Systemmetadaten, die in „sys.servers“ gespeichert sind, nachdem der Hostname eines Computers, der eine eigenständige Instanz von SQL Server hostet, geändert wurde. |  
|[Installieren von ](../../database-engine/install-windows/install-sql-server-servicing-updates.md)-Wartungsupdates[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|Installation von Updates für [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]|  
|[Setupprotokolldateien](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)| Lesen und Anzeigen von Fehlern in Setupprotokolldateien für SQL Server |  
|[Überprüfen einer Installation](../../database-engine/install-windows/validate-a-sql-server-installation.md)|Informieren Sie sich darüber, wie Sie mithilfe des SQL-Ermittlungsberichts die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Version sowie die auf dem Computer installierten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Funktionen überprüfen.|  
  
  
## <a name="individual-component-installation"></a>Installation einzelner Komponenten 
  
|Artikel|BESCHREIBUNG|  
|-----------|-----------------|  
|[SQL Server-Datenbank-Engine](../../database-engine/install-windows/install-sql-server-database-engine.md)|Installation und Konfiguration von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|[SQL Server-Replikation](../../database-engine/install-windows/install-sql-server-replication.md)|Installation und Konfiguration der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Replikation|  
|[Distributed Replay](../../tools/distributed-replay/install-distributed-replay-overview.md)|Artikel zur Installation des Distributed Replay-Features|  
|[SQL Server-Verwaltungstools mit SSMS](../../ssms/download-sql-server-management-studio-ssms.md)|Installation und Konfiguration von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verwaltungstools|  
|[SQL Server-PowerShell](../../database-engine/install-windows/install-sql-server-powershell.md)|Wichtiges bei der Installation von PowerShell-Komponenten für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
  

## <a name="sql-server-configuration"></a>SQL Server-Konfiguration 
  
|Artikel|BESCHREIBUNG|  
|-----------|-----------------|  
|[Konfigurieren der Windows-Firewall (SQL Server)](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)|Übersicht über die Konfiguration von Firewalls und sowie Anleitungen zum Konfigurieren der Windows-Firewall für den Zugriff auf SQL Server|  
|[Konfigurieren der Windows-Firewall (SSAS)](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)|Konfiguration von Port- und Firewalleinstellungen, um den Zugriff auf [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] oder [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] für SharePoint zu ermöglichen|  
|[Konfigurieren eines mehrfach vernetzten Computers](../../sql-server/install/configure-a-multi-homed-computer-for-sql-server-access.md)|Konfiguration von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und der Windows-Firewall mit erweiterter Sicherheit, um Netzwerkverbindungen für eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in einer mehrfach vernetzten Umgebung bereitzustellen|  

 
## <a name="see-also"></a>Weitere Informationen  

[Upgrade [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../database-engine/install-windows/upgrade-sql-server.md)   
[Deinstallieren von[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../sql-server/install/uninstall-sql-server.md)   
[Installieren von SQL Server Reporting Services (SSRS)](../../reporting-services/install-windows/install-reporting-services.md)   
[Installieren von SQL Server Analysis Services (SSAS)](/analysis-services/instances/install-windows/install-analysis-services)   
[Installieren von Business Intelligence-Funktionen von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../sql-server/install/install-sql-server-business-intelligence-features.md)   
[Lösungen mit hoher Verfügbarkeit &#40;SQL Server&#41;](../sql-server-business-continuity-dr.md)