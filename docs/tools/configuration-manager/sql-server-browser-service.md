---
title: SQL Server Browser Service
description: Hier erfahren Sie, wie Sie SQL Server-Browser verwenden, einen Dienst, der auf Anforderungen für SQL Server-Ressourcen lauscht und Informationen über installierte SQL Server-Instanzen bereitstellt.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
f1_keywords:
- sql13.swb.browseservers.local.f1
- sql13.swb.browseservers.network.f1
helpviewer_keywords:
- services [SQL Server], security
- SQL Browser service (See SQL Server Browser Service)
- Browser Service
- SQL Server Browser service
ms.assetid: 3cc00d3a-487c-4cd9-a155-655f02485fa0
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a4c58894434c503f92c8bcf4df4f74cf9fa2c688
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345188"
---
# <a name="sql-server-browser-service"></a>SQL Server Browser Service
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Browserprogramm wird als Windows-Dienst ausgeführt. Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Browser lauscht auf eingehende Anforderungen für [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ressourcen und stellt Informationen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanzen zur Verfügung, die auf dem Computer installiert sind. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser unterstützt folgende Aktionen:  
  
-   Durchsuchen einer Liste verfügbarer Server  
  
-   Herstellen einer Verbindung mit der richtigen Serverinstanz  
  
-   Herstellen einer Verbindung mit den Endpunkten einer dedizierten Administratorverbindung (DAC, dedicated administrator connection)  
  
 Der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -, [!INCLUDE[ssAS](../../includes/ssas-md.md)]- und der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browserdienst (sqlbrowser) stellt für jede Instanz den Instanzennamen und die Versionsnummer bereit. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser wird mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser kann während des Setups oder mithilfe des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Managers konfiguriert werden. In den folgenden Situationen wird der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browserdienst standardmäßig gestartet:  
  
-   Beim Ausführen eines Upgrades für eine Installation.  
  
-   Beim Installieren in einem Cluster.  
  
-   Beim Installieren einer benannten [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Instanz, die alle Instanzen von SQL Server Express einschließt.  
  
-   Beim Installieren einer benannten Instanz von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
## <a name="background"></a>Hintergrund  
 Vor [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]konnte lediglich eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Computer installiert werden. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lauschte auf eingehende Anforderungen an Port 1433, der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] durch die Internet Assigned Numbers Authority (IANA) zugewiesen wurde. Da ein Port nur von einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz verwendet werden kann, wurde mit Einführung der Unterstützung mehrerer [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] -Instanzen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Resolution Protocol (SSRP) zum Lauschen an UDP-Port 1434 entwickelt. Dieser Listenerdienst reagierte auf Clientanforderungen mit den Namen der installierten Instanzen und den von der Instanz verwendeten Ports bzw. Named Pipes. Aufgrund der begrenzten Funktionsweise des SSRP-Systems wurde der [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] -Browserdienst in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als Ersatz für SSRP eingeführt.  
  
## <a name="how-sql-server-browser-works"></a>Funktionsweise von SQL Server-Browser  
 Wenn die Protokolle TCP/IP oder VIA für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aktiviert sind und eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]gestartet wird, wird dem Server ein TCP/IP-Port zugewiesen. Wenn das Named Pipes-Protokoll aktiviert ist, lauscht [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an einer speziell benannten Pipe. Dieser Port oder "Pipe" wird von der betreffenden Instanz zum Datenaustausch mit Clientanwendungen verwendet. Bei der Installation werden der TCP-Port 1433 und die Pipe `\sql\query` der Standardinstanz zugewiesen, sie können jedoch zu einem späteren Zeitpunkt vom Serveradministrator mit dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Manager geändert werden. Da ein Port oder eine Pipe von nur jeweils einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden kann, werden den benannten Instanzen einschließlich [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]unterschiedliche Portnummern und Pipenamen zugewiesen. Wenn diese Funktion aktiviert ist, werden die beiden benannten Instanzen und [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] standardmäßig für die Verwendung dynamischer Ports konfiguriert, sodass beim Starten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ein verfügbarer Port zugewiesen wird. Bei Bedarf kann einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ein bestimmter Port zugewiesen werden. Beim Verbindungsaufbau können Clients einen bestimmten Port angeben. Wenn der Port jedoch dynamisch zugewiesen wird, kann sich die Portnummer bei jedem Neustart von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ändern, sodass die richtige Portnummer dem Client unbekannt bleibt.  
  
 Beim Starten beansprucht der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser UDP-Port 1434. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser liest die Registrierung, identifiziert alle Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf dem Computer und notiert die verwendeten Ports und Named Pipes. Wenn ein Server über zwei oder mehr Netzwerkkarten verfügt, gibt der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser den ersten gefundenen aktivierten Port für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]zurück. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser unterstützt ipv6 und ipv4.  
  
 Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Clients [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Ressourcen anfordern, sendet die Clientnetzwerkbibliothek über den Port 1434 eine UDP-Nachricht an den Server. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser antwortet mit dem TCP/IP-Port oder der Named Pipe der angeforderten Instanz. Anschließend wird die Verbindung durch die Netzwerkbibliothek der Clientanwendung vollständig abgeschlossen, indem über den Port oder die Named Pipe der gewünschten Instanz eine Anforderung an den Server gesendet wird.  
  
 Weitere Informationen zum Starten und Beenden des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browserdiensts finden Sie unter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Onlinedokumentation.  
  
## <a name="using-sql-server-browser"></a>Verwenden von SQL Server-Browser  
 Wenn der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browserdienst nicht ausgeführt wird, können Sie dennoch eine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen, wenn Sie die richtige Anschlussnummer bzw. die richtig benannte Pipe angeben. Sie können beispielsweise über TCP/IP eine Verbindung mit der Standardinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen, wenn sie an Port 1433 ausgeführt wird.  
  
 Wird der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browserdienst jedoch nicht ausgeführt, sind die folgenden Verbindungen nicht funktionsfähig:  
  
-   Ein Verbindungsversuch einer beliebigen Komponente mit einer benannten Instanz ohne Angabe aller Parameter (z. B. des TCP/IP-Ports oder der Named Pipe).  
  
-   Das Generieren oder Übergeben von Server- oder Instanzeninformationen, die später von anderen Komponenten für erneute Verbindungen verwendet werden können.  
  
-   Verbindung mit einer benannten Instanz ohne Angabe der Portnummer oder der Pipe.  
  
-   Eine DAC mit einer benannten Instanz oder der Standardinstanz, wenn der TCP/IP-Port 1433 nicht verwendet wird.  
  
-   Der OLAP-Redirectordienst.  
  
-   Aufzählen von Servern in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], Enterprise Manager oder Query Analyzer.  
  
 Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in einem Client-zu-Server-Szenario verwenden (z. B. wenn die Anwendung über ein Netzwerk auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zugreift) und Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browserdienst anhalten oder deaktivieren, müssen Sie jeder Instanz eine bestimmte Portnummer zuweisen und den Code der Clientanwendung so schreiben, dass immer dieser Port verwendet wird. Dieser Ansatz birgt folgende Probleme:  
  
-   Sie müssen den Code der Clientanwendung aktualisieren und überarbeiten, um sicherzustellen, dass eine Verbindung mit dem richtigen Port hergestellt wird.  
  
-   Die Ports, die Sie für die einzelnen Instanzen auswählen, können auf dem Server durch andere Dienste oder Anwendungen verwendet werden, sodass die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht verfügbar ist.  
  
## <a name="clustering"></a>Clustering  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser handelt es sich nicht um eine gruppierte Ressource, und ein Failover von einem Clusterknoten auf einen anderen wird nicht unterstützt. In einem Cluster sollte der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser deshalb auf jedem Knoten des Clusters installiert und aktiviert werden. In Clustern lauscht auf jede [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser IP_ANY.  
  
> [!NOTE]  
>  Wenn Sie bei der Überwachung von IP_ANY die Überwachung bestimmter IP-Adressen aktivieren, muss der Benutzer für jede IP-Adresse den gleichen TCP-Port konfigurieren, da der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser die erste gefundene Kombination aus IP-Adresse und Port zurückgibt.  
  
## <a name="installing-uninstalling-and-running-from-the-command-line"></a>Installieren, Deinstallieren und Ausführen über die Befehlszeile  
 Standardmäßig ist das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browserprogramm unter „C:\Programme (x86)\Microsoft SQL Server\90\Shared\sqlbrowser.exe“ installiert.  
  
 Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browserdienst wird deinstalliert, wenn die letzte Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entfernt wird.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser kann zur Problembehandlung mit dem Schalter **-c** von der Eingabeaufforderung aus gestartet werden.  
  
```  
<drive>\<path>\sqlbrowser.exe -c  
```  
  
## <a name="security"></a>Sicherheit  
  
### <a name="account-privileges"></a>Kontoberechtigungen  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser lauscht an einem UDP-Port und akzeptiert nicht authentifizierte Anforderungen mithilfe von SSRP ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Resolution Protocol). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser sollte im Sicherheitskontext eines Benutzers mit geringen Zugriffsrechten ausgeführt werden, um die Anfälligkeit gegenüber böswilligen Angriffen zu verringern. Das Anmeldekonto kann mithilfe des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Managers geändert werden. Für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser gelten die folgenden Mindestbenutzerrechte:  
  
-   Zugriff vom Netzwerk auf diesen Computer verweigern  
  
-   Lokal anmelden verweigern  
  
-   Anmelden als Batchauftrag verweigern  
  
-   Anmelden über Terminaldienste verweigern  
  
-   Anmelden als Dienst  
  
-   Lesen und Schreiben der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Registrierungsschlüssel, die sich auf die Netzwerkkommunikation beziehen (Ports und Pipes)  
  
### <a name="default-account"></a>Standardkonto  
 Bei Setup wird die Verwendung des für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Dienste ausgewählten Kontos konfiguriert. Weitere mögliche Konten sind u. a. die Folgenden:  
  
-   Beliebige **lokale** Domänenkonten  
  
-   Das Konto **Lokaler Dienst**  
  
-   Das Konto **Lokales System** (wird nicht empfohlen, da es über unnötige Berechtigungen verfügt)  
  
### <a name="hiding-sql-server"></a>Ausblenden von SQL Server  
 Bei ausgeblendeten Instanzen handelt es sich um Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , die nur Verbindungen im freigegebenen Speicherbereich unterstützen. Legen Sie für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]das Flag `HideInstance` fest, um anzugeben, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser nicht mit Informationen zu dieser Serverinstanz reagieren soll.  
  
### <a name="using-a-firewall"></a>Verwenden einer Firewall  
 Für die Kommunikation mit dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browserdienst auf einem Server, auf dem eine Firewall verwendet wird, öffnen Sie neben dem von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendeten Port (z. B. 1433) den UDP-Port 1434. Informationen zum Arbeiten mit einer Firewall finden Sie unter „Gewusst wie: Konfigurieren einer Firewall für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Zugriff“ in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Onlinedokumentation.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Netzwerkprotokolle und Netzwerkbibliotheken](../../sql-server/install/network-protocols-and-network-libraries.md)  
  
  
