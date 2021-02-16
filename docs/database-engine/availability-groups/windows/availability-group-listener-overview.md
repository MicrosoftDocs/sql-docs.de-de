---
title: Was ist ein Verfügbarkeitsgruppenlistener?
description: 'Hier finden Sie einen Überblick über den Always On-Verfügbarkeitsgruppenlistener und darüber, wie er Datenverkehr automatisch zu einem gewünschten Server weiterleiten kann. '
ms.custom: seodec18
ms.date: 02/27/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], listeners
- read-only routing
- read-intent connections [AlwaysOn Availability Groups]
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], read-only routing
- Availability Groups [SQL Server], client connectivity
ms.assetid: 76fb3eca-6b08-4610-8d79-64019dd56c44
author: cawrites
ms.author: chadam
ms.openlocfilehash: e96b57c13cccf07424ce43aab74fde8b4fa43d08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340854"
---
# <a name="what-is-an-availability-group-listener"></a>Was ist ein Verfügbarkeitsgruppenlistener?  
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Ein Verfügbarkeitsgruppenlistener ist der Name eines virtuellen Netzwerks (Virtual Network Name, VNN), mit dem Clients eine Verbindung herstellen können, um auf eine Datenbank in einem primären oder sekundären Replikat einer Always On-Verfügbarkeitsgruppe zuzugreifen. Ein Listener ermöglicht es einem Client, eine Verbindung zu einem Replikat herzustellen, ohne dabei den Name der physischen SQL Server-Instanz kennen zu müssen. Da der Listener Datenverkehr weiterleitet, muss die Clientverbindungszeichenfolge nach einem Failover nicht angepasst werden. 

Ein Verfügbarkeitsgruppenlistener besteht aus einem DNS (Domain Name System)-Listenernamen, einer Listenerportbezeichnung und mindestens einer IP-Adresse. Nur das TCP-Protokoll wird von Verfügbarkeitsgruppenlistenern unterstützt.  Der DNS-Name des Listeners muss in der Domäne und NetBIOS eindeutig sein.  Wenn Sie einen neuen Listener erstellen, wird er zu einer Ressource in einem Cluster mit einem zugeordneten virtuellen Netzwerknamen (VNN), einer virtuellen IP (VIP) und Verfügbarkeitsgruppenabhängigkeit. Ein Client verwendet DNS, um den VNN in mehrere IP-Adressen aufzulösen, und versucht dann, eine Verbindung mit jeder einzelnen Adresse herzustellen, bis eine Verbindungsanforderung erfolgreich ist oder ein Timeout eintritt.  
  
Wenn für mindestens ein [lesbares sekundäres Replikat](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md) das schreibgeschützte Routing konfiguriert ist, werden Clientverbindungen zum Listener für beabsichtigte Lesevorgänge automatisch an ein lesbares sekundäres Replikat umgeleitet. 
  
In diesem Artikel erhalten Sie einen Überblick über ein Verfügbarkeitsgruppenlistener. Sie haben auch die Möglichkeit, [den Listener zu konfigurieren](create-or-configure-an-availability-group-listener-sql-server.md). Dann erfahren Sie, wie Sie [eine Verbindung zum Listener herstellen](listeners-client-connectivity-application-failover.md).
  
  
##  <a name="listener-parameters"></a><a name="AGlConfig"></a> Listenerparameter  

 Ein Verfügbarkeitsgruppenlistener verwendet Folgendes:
  
 **Ein eindeutiger DNS-Name**  
 Dieser wird auch als virtueller Netzwerkname (Virtual Network Name, VNN) bezeichnet. Es gelten die Active Directory-Benennungsregeln für DNS-Hostnamen. Weitere Informationen finden Sie im KB-Artikel [Namenskonventionen in Active Directory für Computer, Domänen, Standorte und Organisationseinheiten](https://support.microsoft.com/kb/909264) .  
  
**Mindestens eine virtuelle IP-Adresse (Virtual IP address, VIP)**  
 VIPs werden für mindestens ein Subnetz konfiguriert, in das ein Failover der Verfügbarkeitsgruppe erfolgen kann.  
  
**IP-Adresskonfiguration**  
 Für einen gegebenen Listener der Verfügbarkeitsgruppe verwendet die IP-Adresse entweder das Dynamic Host Configuration Protocol (DHCP) oder mindestens eine statische IP-Adresse. Die Verwendung von DHCP kann zu Verbindungsverzögerungen während des Failovers führen. Die Verwendung in Produktionsumgebungen empfiehlt sich also nicht. Verfügbarkeitsgruppen, die sich über mehrere Subnetze erstrecken oder hybride Netzwerkkonfigurationen verwenden, müssen eine statische IP-Adresse verwenden. 
 
  
##  <a name="listener-port"></a><a name="SelectListenerPort"></a> Listenerport 
 Wenn Sie einen Verfügbarkeitsgruppenlistener konfigurieren, müssen Sie einen Port festlegen.  Sie können den Standardport auf 1433 konfigurieren, um Clientverbindungszeichenfolgen einfach zu gestalten. Wenn Sie 1433 verwenden, müssen Sie keine Portnummer in einer Verbindungszeichenfolge festlegen. Da jeder Verfügbarkeitsgruppenlistener einen separaten virtuellen Netzwerknamen besitzt, kann außerdem jeder für einen WSFC-Cluster konfigurierte Verfügbarkeitsgruppenlistener für den Verweis auf Port 1433 konfiguriert werden.  
  
 Sie können auch einen nicht standardmäßigen Listenerport festlegen. Dies bedeutet jedoch, dass Sie auch in der Verbindungszeichenfolge immer dann explizit einen Zielport angeben müssen, wenn Sie eine Verbindung mit dem Verfügbarkeitsgruppenlistener herstellen.  Darüber hinaus müssen Sie die Berechtigung für die Firewall für den nicht standardmäßigen Port öffnen.  
  
 Wenn Sie den Standardport 1433 für die virtuellen Netzwerknamen der Verfügbarkeitsgruppenlistener verwenden, müssen Sie dennoch sicherstellen, dass keine anderen Dienste im Clusterknoten diesen Port verwenden. Andernfalls tritt ein Portkonflikt auf.  
  
 Wenn eine der Instanzen von SQL Server bereits über den Instanzlistener TCP-Port 1433 überwacht und keine anderen Dienste (einschließlich weitere Instanzen von SQL Server) auf dem Computer Port 1433 überwachen, wird kein Portkonflikt mit dem Verfügbarkeitsgruppenlistener verursacht.  Das liegt daran, dass der Verfügbarkeitsgruppenlistener den selben TCP-Port im gleichen Dienstprozess freigeben kann.  Mehrere Instanzen von SQL Server (parallel) sollten jedoch nicht für die Überwachung desselben Ports konfiguriert werden.  
  
  
##  <a name="behavior-of-client-connections-on-failover"></a><a name="CCBehaviorOnFailover"></a> Verhalten von Clientverbindungen beim Failover  

 Wenn ein Verfügbarkeitsgruppenfailover auftritt, werden vorhandene persistente Verbindungen zur Verfügbarkeitsgruppe beendet. Der Client muss eine neue Verbindung herstellen, um weiterhin dieselbe primäre Datenbank oder schreibgeschützte sekundäre Datenbank zu verwenden.  Während eines Failovers auf der Serverseite tritt bei der Verbindung zur Verfügbarkeitsgruppe möglicherweise ein Fehler auf, und die Clientanwendung wird gezwungen, erneut eine Verbindung herzustellen, bis die primäre Datenbank wieder vollständig online geschaltet ist.  
  
 Wenn die Verfügbarkeitsgruppe während des Verbindungsversuchs einer Clientanwendung, jedoch vor dem Verbindungstimeout online geschaltet wird, stellt der Clienttreiber möglicherweise während einer der internen Wiederholungsversuche erfolgreich eine Verbindung her. In diesem Fall wird kein Fehler an die Anwendung ausgegeben.  


## <a name="next-steps"></a>Nächste Schritte

Da Sie nun damit vertraut sind, wie ein Verfügbarkeitsgruppenlistener funktioniert, [erstellen Sie Ihren Listener](create-or-configure-an-availability-group-listener-sql-server.md), und konfigurieren Sie Ihre Anwendung dann so, dass eine [Verbindung zum Listener](listeners-client-connectivity-application-failover.md) hergestellt wird. Sie können auch verschiedene [Überwachungsstrategien für Verfügbarkeitsgruppen](monitoring-of-availability-groups-sql-server.md) überprüfen, mit denen Sie die Integrität Ihrer Verfügbarkeitsgruppe gewährleisten können. 

Weitere Informationen finden Sie im Abschnitt „Verfügbarkeitsmodi“ des Themas [Übersicht über Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md). 
  

  
  
