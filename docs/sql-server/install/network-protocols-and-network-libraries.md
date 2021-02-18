---
title: Netzwerkprotokolle und Netzwerkbibliotheken | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie ein Server so konfiguriert werden kann, dass er mehrere Netzwerkprotokolle überwacht. Außerdem erfahren Sie, wie Sie die Konfiguration mithilfe des SQL Server-Konfigurations-Managers ändern können.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- protocols [SQL Server]
- configuration options [SQL Server], protocols
- network libraries [SQL Server]
- protocols [SQL Server], about network protocols
- pipes [SQL Server]
- network protocols [SQL Server]
- default SQL Server configurations
- library [SQL Server]
- network protocols [SQL Server], about network protocols
- configuration options [SQL Server], libraries
ms.assetid: 8cd437f6-9af1-44ce-9cb0-4d10c83da9ce
author: cawrites
ms.author: chadam
ms.openlocfilehash: 81bf039f0a63aaad7a2214c48bb5948618e7ce7e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336158"
---
# <a name="network-protocols-and-network-libraries"></a>Netzwerkprotokolle und Netzwerkbibliotheken
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Ein Server kann gleichzeitig auf mehreren Netzwerkprotokollen lauschen bzw. diese überwachen. Jedes Protokoll muss jedoch konfiguriert sein. Wenn ein bestimmtes Protokoll nicht konfiguriert ist, kann der Server auf diesem Protokoll nicht lauschen. Nach der Installation können diese Protokollkonfigurationen mithilfe des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Managers geändert werden.  
  
## <a name="default-sql-server-network-configuration"></a>Standardnetzwerkkonfiguration von SQL Server  
 Eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Standardinstanz wird konfiguriert für TCP/IP, Port 1433 und die Named Pipe „ \\\\.\pipe\sql\query“. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden für dynamische TCP-Ports konfiguriert, wobei eine Portnummer vom Betriebssystem zugewiesen wird.  
  
 Wenn Sie keine dynamischen Portadressen verwenden können (beispielsweise wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Verbindungen über einen Firewallserver weitergeleitet werden, der für die Verwendung bestimmter Portadressen konfiguriert ist). Wählen Sie eine nicht zugewiesene Portnummer aus. Portnummernzuweisungen werden von der Internet Assigned Numbers Authority verwaltet und sind unter [https://www.iana.org](https://go.microsoft.com/fwlink/?LinkId=48844) aufgelistet.  
  
 Zur Verbesserung der Sicherheit wird die Netzwerkkonnektivität bei der Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht vollständig aktiviert. Um Netzwerkprotokolle nach Abschluss des Setups zu aktivieren, zu deaktivieren und zu konfigurieren, verwenden Sie den Bereich für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Netzwerkkonfiguration des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Managers.  
  
## <a name="server-message-block-protocol"></a>SMB (Server Message-Block)-Protokoll  
 Bei Servern im Umkreisnetzwerk sollten alle nicht benötigten Protokolle deaktiviert sein, einschließlich des SMB (Server Message Block). Webserver und DNS-Server (Domain Name System) benötigen SMB nicht. Dieses Protokoll sollte deaktiviert sein, um der Gefahr der Benutzerenumeration vorzubeugen.  
  
> [!WARNING]
>  Durch das Deaktivieren von Server Message Block wird der Zugriff auf die Remotedateifreigabe durch [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder durch den Windows-Clusterdienst blockiert. Deaktivieren Sie SMB nicht, wenn Sie einen der folgenden Schritte ausführen möchten:  
> 
>  -   Verwenden des Mehrheitsquorummodus für Windows-Clusterknoten und Dateifreigaben  
> -   Angeben einer SMB-Dateifreigabe als Datenverzeichnis während der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installation  
> -   Erstellen einer Datenbankdatei auf einer SMB-Dateifreigabe  
  
#### <a name="to-disable-smb"></a>So deaktivieren Sie SMB  
  
1.  Zeigen Sie im Menü **Start** auf **Einstellungen**, und klicken Sie dann auf **Netzwerk- und DFÜ-Verbindungen**.  
  
     Klicken Sie mit der rechten Maustaste auf die Internetverbindung, und klicken Sie dann auf **Eigenschaften**.  
  
2.  Aktivieren Sie das Kontrollkästchen **Client für Microsoft-Netzwerke** , und klicken Sie dann auf **Deinstallieren**.  
  
3.  Befolgen Sie die Schritte zur Deinstallation.  
  
4.  Aktivieren Sie das Kontrollkästchen **Datei- und Druckerfreigabe für Microsoft-Netzwerke**, und klicken Sie dann auf **Deinstallieren**.  
  
5.  Befolgen Sie die Schritte zur Deinstallation.  
  
#### <a name="to-disable-smb-on-servers-accessible-from-the-internet"></a>So deaktivieren Sie SMB auf Servern, auf die vom Internet aus zugegriffen werden kann  
  
-   Deaktivieren Sie in den Eigenschaften von LAN-Verbindung im Dialogfeld **Eigenschaften von Internetprotokoll (TCP/IP)** die Kontrollkästchen **Datei- und Druckerfreigabe für Microsoft-Netzwerke** und **Client für Microsoft-Netzwerke**.  
  
## <a name="endpoints"></a>Endpunkte  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird ein neues Konzept für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Verbindungen eingeführt: Die Verbindung wird auf dem Server durch einen [!INCLUDE[tsql](../../includes/tsql-md.md)]*Endpunkt*. Für die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Endpunkte können Berechtigungen erteilt, aufgehoben oder verweigert werden. Standardmäßig sind alle Benutzer berechtigt, auf einen Endpunkt zuzugreifen, sofern die betreffende Berechtigung nicht durch ein Mitglied der sysadmin-Gruppe oder den Besitzer des Endpunkts verweigert oder aufgehoben wird. Im Rahmen der GRANT-, REVOKE- und DENY ENDPOINT-Syntax wird eine Endpunkt-ID verwendet, die der Administrator aus der Katalogsicht des Endpunkts abrufen muss.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup erstellt [!INCLUDE[tsql](../../includes/tsql-md.md)] -Endpunkte für alle unterstützten Netzwerkprotokolle und für die dedizierte Administratorverbindung.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] -Setup werden die folgenden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Endpunkte erstellt:  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] Local Machine  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] Named Pipes  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] Default TCP  
  
 Weitere Informationen über Endpunkte finden Sie unter [Konfigurieren der Datenbank-Engine zum Überwachen mehrerer TCP-Ports](../../database-engine/configure-windows/configure-the-database-engine-to-listen-on-multiple-tcp-ports.md) und [Endpunkte-Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md).  
  
 Weitere Informationen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Netzwerkkonfigurationen finden Sie in den folgenden Artikeln der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Onlinedokumentation:  
  
-   [Server-Netzwerkkonfiguration](../../database-engine/configure-windows/server-network-configuration.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Oberflächenkonfiguration](../../relational-databases/security/surface-area-configuration.md)   
 [Überlegungen zur Sicherheit bei SQL Server-Installationen](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)   
 [Planen einer SQL Server-Installation](../../sql-server/install/planning-a-sql-server-installation.md)  
  
  
