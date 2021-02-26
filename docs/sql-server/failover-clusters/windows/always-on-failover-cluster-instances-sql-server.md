---
title: Always On-Failoverclusterinstanzen
description: In diesem Artikel erfahren Sie, wie Always-On-Failoverclusterinstanzen durch Redundanz auf Ebene der Serverinstanz in SQL Server lokal eine Hochverfügbarkeit bereitstellen können.
ms.custom: seo-lt-2019
ms.date: 01/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: conceptual
helpviewer_keywords:
- clustering [SQL Server]
- high availability [SQL Server], failover clustering
- virtual servers [SQL Server], about virtual servers
- clusters [SQL Server]
- servers [SQL Server], failover clustering
- resource groups [SQL Server]
- availability [SQL Server]
- failover clustering [SQL Server]
- AlwaysOn [SQL Server], see failover clustering [SQL Server]
ms.assetid: 86a15b33-4d03-4549-8ea2-b45e4f1baad7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2f87ae98424eef3f6aeadca1f11da6d9d1f2b403
ms.sourcegitcommit: 8bdb5a51f87a6ff3b94360555973ca0cd0b6223f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2021
ms.locfileid: "100549386"
---
# <a name="always-on-failover-cluster-instances-sql-server"></a>AlwaysOn-Failoverclusterinstanzen (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Als Teil des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Always On-Angebots nutzen Always On-Failoverclusterinstanzen die Funktionalität des Windows Server-Failoverclustering (WSFC), um durch Redundanz auf Serverinstanzebene (eine *Failoverclusterinstanz* [FCI]) lokale Hochverfügbarkeit zu bieten. Eine FCI ist eine einzelne Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Diese ist auf Windows Server-Failoverclustering-Knoten (WSFC) und möglicherweise auf mehreren Subnetzen installiert. In einem Netzwerk wird eine FCI als eine Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] angezeigt, die auf einem einzelnen Computer ausgeführt wird. Die FCI bietet jedoch die Möglichkeit zur Failoverbereitstellung von einem WSFC-Knoten zu einem anderen, wenn der aktuelle Knoten nicht verfügbar ist.  
  
 Eine FCI kann [Availability Groups (Verfügbarkeitsgruppen)](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md) nutzen, um die Remotewiederherstellung im Notfall auf Datenbankebene bereitzustellen. Weitere Informationen finden Sie unter [Failover Clustering and Always On Availability Groups &#40;SQL Server&#41; (Failoverclustering und Always On-Verfügbarkeitsgruppen (SQL Server))](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md).  
 
 > [!NOTE]  
 > Windows Server 2016 Datacenter Edition führt die Unterstützung für direkte Speicherplätze (Storage Spaces Direct, S2D) ein. SQL Server-Failoverclusterinstanzen unterstützen S2D für Clusterspeicherressourcen. Weitere Informationen finden Sie unter [Direkte Speicherplätze in Windows Server 2016](/windows-server/storage/storage-spaces/storage-spaces-direct-overview).
 > 
 >Failoverclusterinstanzen unterstützen außerdem freigegebene Clustervolumes (Cluster Shared Volumes, CSVs). Weitere Informationen finden Sie unter [Grundlegendes zu freigegebenen Clustervolumes in einem Failovercluster](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759255(v=ws.11)). 
   
 **In diesem Thema:**  
  
-   [Vorteile](#Benefits)  
  
-   [Empfehlungen](#Recommendations)  
  
-   [Failoverclusterinstanz-Übersicht](#Overview)  
  
-   [Elemente einer Failoverclusterinstanz](#FCIelements)  
  
-   [Konzepte und Tasks des SQL Server-Failovers](#ConceptsAndTasks)  
  
-   [Verwandte Themen](#RelatedTopics)  
  
##  <a name="benefits-of-a-failover-cluster-instance"></a><a name="Benefits"></a> Vorteile einer Failoverclusterinstanz  
 Wenn bei einem Server Hardware- oder Softwarefehler auftreten, kommt es bei den mit dem Server verbundenen Anwendungen oder Clients zu Ausfallzeiten. Wenn eine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz konfiguriert wird, um eine FCI (statt einer eigenständigen Instanz) zu sein, wird die Hochverfügbarkeit dieser [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz vom Vorhandensein redundanter Knoten in der FCI geschützt. Nur jeweils einer der Knoten in der FCI kann die WSFC-Ressourcengruppe besitzen. Bei einem Fehler (Hardwarefehler, Betriebssystemfehler, Anwendungs- oder Dienstfehler) oder einem geplanten Upgrade wird der Ressourcengruppenbesitz zu einem anderen WSFC-Knoten verschoben. Dieser Prozess ist für den Client oder die Anwendung transparent, der bzw. die eine Verbindung mit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] herstellt. Dadurch werden die Ausfallzeiten der Anwendung oder des Clients bei einem Fehler minimiert. Die folgende Liste enthält einige wichtige Vorteile, die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Failoverclusterinstanzen bieten:  
  
-   Schutz auf Instanzebene durch Redundanz  
  
-   Automatisches Failover im Fall eines Fehlers (Hardwarefehler, Betriebssystemfehler, Anwendungs- oder Dienstfehler)  
  
    > [!IMPORTANT]  
    >  In einer Verfügbarkeitsgruppe wird ein automatisches Failover von einer FCI zu anderen Knoten innerhalb der Verfügbarkeitsgruppe nicht unterstützt. Dies bedeutet, dass FCIs und eigenständige Knoten nicht miteinander innerhalb einer Verfügbarkeitsgruppe verbunden werden sollten, wenn ein automatisches Failover eine wichtige Komponente Ihrer Hochverfügbarkeitslösung darstellt. Diese Kopplung kann jedoch für Ihre *Notfallwiederherstellungslösung* vorgenommen werden.  
  
-   Unterstützung für ein umfangreiches Array von Speicherlösungen, einschließlich WSFC-Clusterdatenträger (iSCSI, Fiber-Channel usw.) und SMB-Dateifreigaben (Server Message Block).  
  
-   Notfallwiederherstellungslösung mit Multisubnetz-FCI oder durch Ausführung einer FCI-gehosteten Datenbank innerhalb einer Verfügbarkeitsgruppe Mit der neuen Multisubnetzunterstützung in [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]ist für ein Multisubnetz-FCI nicht länger eine VLAN-Verbindung erforderlich, wodurch die Verwaltbarkeit und die Sicherheit der Multisubnetz-FCI verbessert werden.  
  
-   Kein erneutes Konfigurieren von Anwendungen und Clients während Failover ausgeführt werden  
  
-   Flexible Failoverrichtlinie für präzise Triggerereignisse für automatische Failover  
  
-   Zuverlässige Failover durch regelmäßige und ausführliche Zustandserkennung mithilfe von dedizierten und permanenten Verbindungen  
  
-   Konfigurierbarkeit und Voraussagbarkeit der Failoverzeit durch indirekte Hintergrundprüfpunkte  
  
-   Eingeschränkte Ressourcenauslastung während Failover ausgeführt werden  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
 Es wird empfohlen, in einer Produktionsumgebung statische IP-Adressen zusammen mit der virtuellen IP-Adresse einer Failoverclusterinstanz zu verwenden.  Von der Verwendung von DHCP in einer Produktionsumgebung wird abgeraten. Wenn es zu einer Ausfallzeit kommt und das DHCP-IP-Leasing abläuft, ist für die erneute Registrierung der dem DNS-Namen zugeordneten neuen DHCP-IP-Adresse zusätzlich Zeit erforderlich.  
  
##  <a name="failover-cluster-instance-overview"></a><a name="Overview"></a> Failoverclusterinstanz-Übersicht  
 Eine FCI wird in einer WSFC-Ressourcengruppe mit einem oder mehreren WSFC-Knoten ausgeführt. Wenn die FCI gestartet wird, nimmt einer der Knoten den Besitz der Ressourcengruppe an und schaltet seine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz online. Zu den Ressourcen, die dieser Knoten besitzt, gehören:  
  
-   Netzwerkname  
  
-   IP-Adresse  
  
-   Freigegebene Datenträger  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Datenbank-Engine-Dienste  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Agent-Dienst  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Analysis Services-Dienst, sofern installiert  
  
-   Eine Dateifreigaberessource, wenn die FILESTREAM-Funktion installiert ist  
  
 Nur der Ressourcengruppenbesitzer (und kein anderer Knoten in der FCI) kann jederzeit die jeweiligen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienste in der Ressourcengruppe ausführen. Wenn ein Failover auftritt, und zwar unabhängig davon, ob es ein automatisches Failover oder ein geplantes Failover ist, treten folgende Ereignisse ein:  
  
1.  Außer wenn eine Hardware- oder ein Systemfehler auftritt, werden alle modifizierten Seiten im Puffercache auf den Datenträger geschrieben.  
  
2.  Alle jeweiligen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienste in der Ressourcengruppe werden im aktiven Knoten beendet.  
  
3.  Der Ressourcengruppenbesitz wird auf einen anderen Knoten in der FCI übertragen.  
  
4.  Der neue Ressourcengruppenbesitzer startet seine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienste.  
  
5.  Verbindungsanforderungen für Clientanwendungen werden automatisch an den neuen aktiven Knoten mit dem gleichen virtuellen Netzwerknamen (VNN) übertragen  
  
 Die FCI ist online, solange sein zugrunde liegender WSFC-Cluster in gutem Quorumzustand (die Mehrheit der Quorum-WSFC-Knoten ist als automatische Failoverziele verfügbar) ist. Wenn der WSFC-Cluster sein Quorum verliert, und zwar unabhängig davon, ob durch einen Hardware-, Software-, Netzwerkfehler oder durch eine nicht ordnungsgemäße Quorumkonfiguration, wird der gesamte WSFC-Cluster zusammen mit der FCI in den Offlinemodus versetzt. Ein manueller Eingriff ist dann in diesem ungeplanten Failoverszenario erforderlich, um in den verbleibenden verfügbaren Knoten das Quorum wiederherzustellen, damit der WSFC-Cluster und die FCI wieder in den Onlinemodus versetzt werden können. Weitere Informationen finden Sie unter [WSFC-Quorummodi und Abstimmungskonfiguration &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  
  
### <a name="predictable-failover-time"></a>Vorhersagbare Failoverzeit  
 Je nachdem, wann Ihre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz zuletzt einen Prüfpunktvorgang ausgeführt hat, kann sich eine beträchtliche Menge an modifizierten Seiten im Puffercache befinden. Folglich dauern Failover so lange, wie das Schreiben der verbleibenden modifizierte Seiten auf den Datenträger dauert. Dadurch kann es zu einer langen und nicht vorhersagbaren Failoverzeit kommen. Ab [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]kann die FCI die Menge an modifizierten Seiten, die im Puffercache behalten wurde, mithilfe von indirekten Prüfpunkten einschränken. Obwohl dadurch zusätzliche Ressource unter normaler Arbeitsauslastung belegt werden, wird die Failoverzeit vorhersagbarer und besser konfigurierbar. Dies ist sehr nützlich, wenn in der Vereinbarung zum Servicelevel in Ihrer Organisation die Wiederherstellungszeit-Zielsetzung (Recovery Time Objective, RTO) für Ihre Hochverfügbarkeitslösung angegeben wird. Weitere Informationen zu indirekten Prüfpunkten finden Sie unter [Indirect Checkpoints](../../../relational-databases/logs/database-checkpoints-sql-server.md#IndirectChkpt).  
  
### <a name="reliable-health-monitoring-and-flexible-failover-policy"></a>Zuverlässige Systemüberwachung und flexible Failoverrichtlinie  
 Nachdem die FCI erfolgreich gestartet wurde, überwacht der WSFC-Dienst sowohl den Zustand des zugrunde liegenden WSFC-Clusters als auch den Zustand der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz. Ab [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]wird die aktive [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz für die ausführliche Komponentendiagnose durch eine gespeicherte Systemprozedur mithilfe einer dedizierten Verbindung vom WSFC-Dienst abgerufen. Die Implikation hiervon erfolgt dreifach:  
  
-   Die dedizierte Verbindung zur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz macht es möglich, jederzeit die Komponentendiagnose zuverlässig abzurufen, und zwar sogar bei starker Auslastung der FCI. Dadurch wird es möglich, zwischen einem stark ausgelasteten System und einem System, das tatsächlich einen fehlerhaften Zustand aufweist, zu unterscheiden. Dadurch lassen sich Probleme wie falsche Failover verhindern.  
  
-   Die ausführliche Komponentendiagnose macht es möglich, eine flexiblere Failoverrichtlinie zu konfigurieren, wodurch Sie auswählen können, welche Fehlerbedingungen Failover auslösen bzw. welche sie nicht auslösen.  
  
-   Mit der ausführlichen Komponentendiagnose wird auch rückwirkend eine bessere Problembehandlung automatischer Failover ermöglicht. Die Diagnoseinformationen werden in Protokolldateien gespeichert, die den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Fehlerprotokollen zugeordnet werden. Sie können sie mit dem Protokolldatei-Viewer laden, um die Komponentenstatus zu überprüfen, die zu einem Failover führen, damit Sie bestimmen können, wodurch das Failover verursacht wurde.  
  
 Weitere Informationen finden Sie unter [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md).  
  
##  <a name="elements-of-a-failover-cluster-instance"></a><a name="FCIelements"></a> Elemente einer Failoverclusterinstanz  
 Eine FCI besteht aus einem Satz physischer Server (Knoten), die über eine ähnliche Hardwarekonfiguration sowie über eine identische Softwarekonfiguration verfügen, einschließlich Betriebssystemversion und Patchebene sowie [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Version, -Patchebene, -Komponenten und -Instanzname. Identische Softwarekonfiguration ist notwendig, um sicherzustellen, dass die FCI vollständig funktional sein kann, da es zwischen den Knoten Failover ausführt.  
  
 WSFC-Ressourcengruppe  
 Eine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -FCI wird in einer WSFC-Ressourcengruppe ausgeführt. Jeder Knoten in der Ressourcengruppe behält eine synchronisierte Kopie der Konfigurationseinstellungen und Prüfpunkt-Registrierungsschlüssel bei, um die vollständige Funktionalität der FCI nach einem Failover sicherzustellen. Zudem besitzt nur einer der Knoten im Cluster die Ressourcengruppe zu einer bestimmten Zeit (der aktive Knoten). Der WSFC-Dienst verwaltet den Servercluster, Quorumkonfiguration, Failoverrichtlinie und Failovervorgänge sowie die VNN und virtuelle IP-Adressen für die FCI. Bei einem Fehler (Hardwarefehler, Betriebssystemfehler, Anwendungs- oder Dienstfehler) oder einem geplanten Upgrade wird der Ressourcengruppenbesitz zu einem anderen Knoten in der FCI verschoben. Die Anzahl der in der WSFC-Ressourcengruppe unterstützten Knoten hängt von der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Edition ab. Der gleiche WSFC-Cluster kann abhängig von der Hardwarekapazität, z. B. CPUs, Arbeitsspeicher und Anzahl von Datenträgern, zudem mehrere FCIs (mehrere Ressourcengruppen) ausführen.  
  
 SQL Server-Binärdateien  
 Die Produktbinärdateien werden lokal auf jedem Knoten der FCI installiert. Dieser Prozess ähnelt eigenständigen Installationen von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Während des Starts werden die Dienste jedoch nicht automatisch gestartet, sondern durch den WSFC verwaltet.  
  
 Storage  
 Im Gegensatz zur Verfügbarkeitsgruppe muss eine FCI freigegebenen Speicher zwischen allen Knoten der FCI für Datenbank und Protokolle verwenden. Der freigegebene Speicher kann die Form von WSFC-Clusterdatenträgern, direkten Speicherplätzen (Storage Spaces Direct, S2D), Datenträgern auf einem SAN oder Dateifreigaben auf einem SMB aufweisen. Auf diese Weise verfügen alle Knoten in der FCI immer dann über die gleiche Sicht der Instanzdaten, wenn ein Failover auftritt. Dies bedeutet jedoch, dass der freigegebene Speicher das Potenzial hat, die einzelne Fehlerquelle zu sein. Die FCI hängt zudem von der zugrunde liegenden Speicherlösung ab, um Datenschutz sicherzustellen.  
  
 Netzwerkname  
 Der VNN für die FCI stellt einen einheitlichen Verbindungspunkt für die FCI bereit. Dadurch können Anwendungen eine Verbindung zum VNN herstellen, ohne dass sie den derzeit aktiven Knoten kennen müssen. Wenn ein Failover auftritt, wird der VNN für den neuen aktiven Knoten registriert, nachdem dieser gestartet wurde. Dieser Prozess ist für den Client oder die Anwendung transparent, der bzw. die eine Verbindung mit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] herstellt. Dadurch werden die Ausfallzeiten der Anwendung oder des Clients bei einem Fehler minimiert.  
  
 Virtuelle IP-Adressen  
 Im Fall einer Multisubnetz-FCI wird jedem Subnetz eine virtuelle IP-Adresse in der FCI zugewiesen. Während eines Failovers wird der VNN auf dem DNS-Server aktualisiert, um auf die virtuelle IP-Adresse für das jeweilige Subnetz zu verweisen. Anwendungen und Clients können dann eine Verbindung mit der FCI herstellen, die den gleichen VNN nach einem Multisubnetzfailover verwendet.  
  
##  <a name="sql-server-failover-concepts-and-tasks"></a><a name="ConceptsAndTasks"></a> Konzepte und Tasks des SQL Server-Failovers  
  
|Konzepte und Tasks|Thema|  
|------------------------|-----------|  
|Beschreibt den Fehlererkennungsmechanismus und die flexible Failoverrichtlinie.|[Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)|  
|Beschreibt Konzepte hinsichtlich FCI-Verwaltung und -Wartung.|[Verwaltung und Wartung von Failoverclusterinstanzen](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)|  
|Beschreibt die Konfiguration und Konzepte von Multisubnetzen.|[SQL Server-Multisubnetzclustering &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md)|  
  
##  <a name="related-topics"></a><a name="RelatedTopics"></a> Verwandte Themen  
  
|**Beschreibungen der Themen**|**Thema**|  
|----------------------------|---------------|  
|Beschreibt die Installation eines neuen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -FCIs.|[Erstellen eines neuen SQL Server-Failoverclusters &#40;Setup&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)|  
|Beschreibt die Aktualisierung eines [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] -Failoverclusters.|[Upgrade einer SQL Server-Failoverclusterinstanz](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md).|  
|Beschreibt Konzepte des Windows-Failoverclustering und stellt Links zu Tasks für Windows-Failoverclustering bereit.|[Windows Server-Failovercluster mit SQL Server](windows-server-failover-clustering-wsfc-with-sql-server.md)|
|Beschreibt die Unterschiede der Konzepte zwischen Knoten in einer FCI und Replikaten innerhalb einer Verfügbarkeitsgruppe. Zudem werden Überlegungen zum Hosten mithilfe einer FCI für eine Verfügbarkeitsgruppe eines Replikats dargelegt.|[Failoverclustering und Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)|  
  
