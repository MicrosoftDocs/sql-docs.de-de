---
title: TCP/IP-Eigenschaften
description: In diesem Artikel wird erläutert, wie Sie über die Registerkarte „IP-Adressen“ im Dialogfeld für die TCP/IP-Eigenschaften die TCP/IP-Protokolloptionen für eine bestimmte IP-Adresse in SQL Server konfigurieren.
ms.custom: seo-lt-2019
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- ports [SQL Server], listening on
- listening [SQL Server], on ports
ms.assetid: 4c17ed45-9da7-4bec-bce6-970109fe7365
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 56055b7d3537413398980579342884ab6a58c142
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345084"
---
# <a name="tcpip-properties-ip-addresses-tab"></a>TCP/IP-Eigenschaften
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Verwenden Sie das Dialogfeld **TCP/IP-Eigenschaften** (Registerkarte IP-Adressen), um die TCP/IP-Protokolloptionen für eine spezielle IP-Adresse zu konfigurieren. Nur die Optionen **Dynamische TCP-Ports** und **TCP-Port** können durch Auswahl von **IPAll** für alle Adressen sofort konfiguriert werden.  
  
 Die Änderungen werden bei einem Neustart von SQL Server wirksam. Informationen über das Starten und Beenden des SQL Server-Browser-Dienstes finden Sie unter [Starten und Beenden des SQL Server-Browser-Dienstes](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
## <a name="static-vs-dynamic-ports"></a>Statische und Dynamische Ports  
 Die Standardinstanz von SQL Server lauscht für eingehende Verbindungen an Port 1433. Der Port kann aus Sicherheitsgründen oder wegen Anforderungen von Clientanwendungen geändert werden. Standardmäßig werden benannte Instanzen (einschließlich SQL Server Express) zur Überwachung von dynamischen Ports konfiguriert. Lassen Sie das Feld **Dynamische TCP-Ports** leer, und geben Sie eine verfügbare Portnummer in das Feld **TCP-Port** ein, um einen statischen Port zu konfigurieren. Weitere Informationen zum Öffnen von Ports in der Firewall finden Sie unter "Konfigurieren der Windows-Firewall für den SQL Server-Zugriff" in der Onlinedokumentation.  
  
## <a name="dynamic-ports"></a>Dynamische Ports  
 Wenn eine Instanz beim Starten von SQL Server zur Überwachung von dynamischen Ports konfiguriert ist, wird das Betriebssystem auf einen verfügbaren Port überprüft und ein Endpunkt für diesen Port geöffnet. Eingehende Verbindungen müssen diese Portnummer zum Verbinden angeben. Da sich die Portnummer bei jedem Start von SQL Server ändern kann, stellt SQL Server den SQL Server-Browser-Dienst bereit, um an den Ports zu lauschen und eingehende Verbindungen an den aktuellen Port für diese Instanz zu leiten. Das Verwenden von dynamischen Ports macht das Herstellen von Verbindungen mit SQL Server durch eine Firewall schwierig, da die Portnummer sich bei einem Neustart von SQL Server ändern kann. Dies erfordert Änderungen an den Firewalleinstellungen. Konfigurieren Sie SQL Server zum Verwenden eines statischen Ports, um Verbindungsprobleme durch eine Firewall zu vermeiden.  
  
## <a name="options"></a>Tastatur  
 **Aktiv**  
 Gibt an, dass die IP-Adresse auf dem Computer aktiviert ist. Nicht verfügbar für **IPAll**.  
  
 **Aktiviert**  
 Wenn die Eigenschaft **Auf Alle lauschen** unter **TCP/IP-Eigenschaften** (Registerkarte Protokoll) auf **Nein** festgelegt ist, gibt diese Eigenschaft an, ob SQL Server auf die IP-Adresse lauscht. Wenn die Eigenschaft **Auf Alle Lauschen** unter **TCP/IP-Eigenschaften** (Registerkarte Protokoll) auf **Ja** festgelegt ist, wird die Eigenschaft ignoriert. Nicht verfügbar für **IPAll**.  
  
 **IP-Adresse**  
 Zeigt die von dieser Verbindung verwendete IP-Adresse an oder ändert diese. Führt die von diesem Computer verwendete IP-Adresse sowie die IP-Loopbackadresse 127.0.0.1 auf. Nicht verfügbar für **IPAll**. Die IP-Adresse kann sowohl im IPv4- oder IPv6-Format vorliegen.  
  
 **Dynamische TCP-Ports**  
 Leer, wenn dynamische Ports nicht aktiviert sind. Legen Sie den Wert 0 fest, um dynamische Ports zu verwenden.  
  
 Für **IPAll** wird die Portnummer des verwendeten dynamischen Ports angezeigt.  
  
 **TCP-Port**  
 Zeigt den Port an, an dem SQL Server lauscht, oder ändert diesen. Die Standardinstanz von SQL Server lauscht standardmäßig an Port 1433.  
  
 Die Datenbank-Engine kann an mehreren Ports auf derselben IP-Adresse lauschen. Ports werden (durch Trennzeichen getrennt) im Format 1433,1500,1501 aufgelistet. Dieses Feld ist auf 2047 Zeichen begrenzt.  
  
 Zum Konfigurieren einer einzelnen IP-Adresse zum Lauschen an mehreren Ports muss der Parameter **Auf Alle Lauschen** auch auf **Nein** festgelegt sein. Diesen finden Sie im Dialogfeld **TCP/IP-Eigenschaften** auf der Registerkarte **Protokolle** . Weitere Informationen finden Sie im Abschnitt „Vorgehensweise: Konfigurieren der Datenbank-Engine zum Überwachen mehrerer TCP-Ports“ in der SQL Server-Onlinedokumentation.  
  
## <a name="adding-or-removing-ip-addresses"></a>Hinzufügen und Entfernen von IP-Adressen  
 Der SQL Server-Konfigurations-Manager zeigt die IP-Adressen an, die zum Zeitpunkt der Installation von SQL Server verfügbar waren. Die verfügbaren IP-Adressen können sich ändern, wenn Netzwerkkarten hinzugefügt oder entfernt werden, wenn dynamisch zugewiesene IP-Adressen ablaufen, wenn die Netzwerkstruktur neu konfiguriert wird oder wenn der physische Standort des Computers geändert wird, z. B. bei einem Laptop, über das von einem anderen Gebäude aus eine Verbindung mit dem Netzwerk hergestellt wird. Zum Ändern der IP-Adresse bearbeiten Sie das Feld **IP-Adresse** , und starten Sie anschließend SQL Server neu.  
  
## <a name="additional-topics-in-books-online"></a>Weitere Themen in der Onlinedokumentation  
 Suchen Sie in MSDN nach Themen wie **Konfigurieren eines Servers zur Überwachung eines bestimmten TCP-Ports (SQL Server-Konfigurations-Manager)** und **Konfigurieren der Datenbank-Engine zum Überwachen mehrerer TCP-Ports**.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Auswählen eines Netzwerkprotokolls](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)   
 [Erstellen einer gültigen Verbindungszeichenfolge mithilfe von TCP/IP](creating-a-valid-connection-string-using-tcp-ip.md)   
 [SQL Server-Browserdienst](sql-server-browser-service.md)  
  
