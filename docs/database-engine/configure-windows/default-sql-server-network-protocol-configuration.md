---
title: Standard-Netzwerkkonfiguration von SQL Server | Microsoft-Dokumentation
description: Machen Sie sich mit den Faktoren vertraut, von denen abhängt, ob Netzwerkprotokolle während der Installation von SQL Server aktiviert oder deaktiviert werden. Hier erfahren Sie, wie Sie Protokolle nach der Installation konfigurieren.
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- protocols [SQL Server], default settings
- default protocols, after install
ms.assetid: 635ea361-a797-4971-bd05-e3415862bc5c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d5463ac163271cf14f5b52167559bc8f3f53d805
ms.sourcegitcommit: 2f868a77903c1f1c4cecf4ea1c181deee12d5b15
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2020
ms.locfileid: "91671113"
---
# <a name="default-sql-server-network-protocol-configuration"></a>Standard-Netzwerkkonfiguration von SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Um eine erhöhte Sicherheit zu gewährleisten, wird bei bestimmten Neuinstallationen von [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] die Netzwerkkonnektivität deaktiviert. Die Netzwerkkonnektivität über TCP/IP wird nicht deaktiviert, wenn Sie Enterprise Edition, Standard Edition, Evaluation Edition oder Workgroup Edition verwenden oder wenn eine vorherige Installation von [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] vorhanden ist. Für alle Installationen wird das Shared Memory-Protokoll aktiviert, um lokale Verbindungen mit dem Server zu ermöglichen. Der [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] -Browser-Dienst wird möglicherweise beendet, je nach Installationsbedingungen und -optionen.

Verwenden Sie den Knoten [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] Netzwerkkonfiguration des Konfigurations-Managers von [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , um nach der Installation die Netzwerkprotokolle zu konfigurieren. Verwenden Sie den Knoten [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] -Dienste des Konfigurations-Managers von [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , um den [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] -Browserdienst so zu konfigurieren, dass er automatisch gestartet wird. Weitere Informationen finden Sie unter [Aktivieren oder Deaktivieren eines Servernetzwerkprotokolls](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md).


## <a name="default-configuration"></a>Standardkonfiguration

In der folgenden Tabelle wird die Konfiguration nach der Installation beschrieben.

|Edition | Neuinstallation/vorherige Installation vorhanden | Shared Memory | TCP/IP | Named Pipes|
| -------- | -- | -- | -- | --  |  
|Enterprise | Neue Installation | Aktiviert | Aktiviert | Deaktiviert für Netzwerkverbindungen|
|Standard | Neue Installation | Aktiviert | Aktiviert | Deaktiviert für Netzwerkverbindungen|
|Web | Neue Installation | Aktiviert | Aktiviert | Deaktiviert für Netzwerkverbindungen|
|Entwickler | Neue Installation | Aktiviert | Disabled | Deaktiviert für Netzwerkverbindungen|
|Auswertung | Neue Installation | Aktiviert | Aktiviert | Deaktiviert für Netzwerkverbindungen|
|SQL Server Express | Neue Installation | Aktiviert | Disabled | Deaktiviert für Netzwerkverbindungen|
|Alle Editionen | Vorherige Installation ist vorhanden, wird aber nicht aktualisiert. | Wie für Neuinstallation | Wie für Neuinstallation | Wie für Neuinstallation|
|Alle Editionen | Aktualisieren | Aktiviert | Einstellungen aus früherer Installation werden beibehalten. | Einstellungen aus früherer Installation werden beibehalten.|


>[!NOTE]
> Wenn die Instanz auf einem [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]-Failovercluster ausgeführt wird, lauscht sie an allen Ports für alle IP-Adressen, die beim Ausführen des [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]-Setups für [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ausgewählt wurden.
 
>[!NOTE]
> Wenn Sie [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] mit Eingabeaufforderungsargumenten installieren, können Sie die zu aktivierenden Protokolle mithilfe des `TCPENABLED` -Parameters und des `NPENABLED` -Parameters angeben. Weitere Informationen finden Sie unter [Installieren von SQL Server über die Eingabeaufforderung](../install-windows/install-sql-server-from-the-command-prompt.md).

## <a name="creating-a-connection-string"></a>Erstellen einer Verbindungszeichenfolge

Beispiele für Verbindungszeichenfolgen finden Sie in den folgenden Themen:
* [Erstellen einer gültigen Verbindungszeichenfolge mithilfe des Shared Memory-Protokolls](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)
* [Erstellen einer gültigen Verbindungszeichenfolge mithilfe von TCP/IP](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md)



## <a name="ssnoversion_md-browser-settings"></a>[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] Browsereinstellungen

Der [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] -Browserdienst kann während der Installation so konfiguriert werden, dass er automatisch gestartet wird. Standardmäßig wird er unter folgenden Bedingungen automatisch gestartet:

* Beim Ausführen eines Upgrades für eine Installation.
* Beim gleichzeitigen Installieren mit einer anderen Instanz von [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].
* Beim Installieren in einem Cluster.
* Beim Installieren einer benannten Instanz der Datenbank-Engine, die alle Instanzen von [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] Express einschließt.
* Beim Installieren einer benannten Instanz von Analysis Services.

## <a name="see-also"></a>Weitere Informationen

[Hardware- und Softwareanforderungen für die Installation von SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)

[Oberflächenkonfiguration](../../relational-databases/security/surface-area-configuration.md)