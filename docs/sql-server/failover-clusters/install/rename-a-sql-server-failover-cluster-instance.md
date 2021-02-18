---
title: Umbenennen einer Failoverclusterinstanz
description: In diesem Artikel wird beschrieben, wie Sie eine SQL Server-Instanz umbenennen, die Teil eines Failoverclusters ist. Dieser Vorgang weicht vom Umbenennen einer eigenständigen Instanz ab.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], virtual servers
- renaming virtual servers
- virtual servers [SQL Server], failover clustering
- failover clustering [SQL Server], virtual servers
ms.assetid: 2a49d417-25fb-4760-8ae5-5871bfb1e6f3
author: cawrites
ms.author: chadam
ms.openlocfilehash: 42fd6dbb5053993d5ad6c2859460e164f476bac1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354025"
---
# <a name="rename-a-sql-server-failover-cluster-instance"></a>Umbenennen einer SQL Server-Failoverclusterinstanz
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Wenn eine [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz Teil eines Failoverclusters ist, unterscheidet sich der Vorgang des Umbenennens des virtuellen Servers vom Umbenennen einer eigenständigen Instanz. Weitere Informationen finden Sie unter [Umbenennen eines Computers, der eine eigenständige Instanz von SQL Server hostet](../../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md).  
  
 Der Name des virtuellen Servers ist immer mit dem SQL-Netzwerknamen (dem Netzwerknamen des virtuellen Servers mit SQL Server) identisch. Sie können zwar den Namen des virtuellen Servers ändern, nicht jedoch den Instanznamen. Sie können z. B. einen virtuellen Server namens VS1\instance1 in einen anderen Namen ändern, z. B. in SQL35\instance1, der Instanzanteil des Namens, instance1, bleibt jedoch unverändert.  
  
 Bevor Sie den Umbenennungsvorgang beginnen, überprüfen Sie die nachfolgenden Elemente.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] unterstützt nicht das Umbenennen von Servern, die an der Replikation beteiligt sind. Eine Ausnahme stellt die Verwendung von Protokollversand mit Replikation dar. Der sekundäre Server beim Protokollversand kann umbenannt werden, wenn der primäre Server dauerhaft verloren ist. Weitere Informationen finden Sie unter [Protokollversand und Replikation &#40;SQL Server&#41;](../../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md).  
  
-   Wenn Sie einen virtuellen Server umbenennen, der für die Verwendung von Datenbankspiegelung konfiguriert ist, müssen Sie die Datenbankspiegelung vor dem Umbenennungsvorgang deaktivieren und die Datenbankspiegelung mit dem neuen Namen des virtuellen Servers anschließend neu einrichten. Die Metadaten für die Datenbankspiegelung werden nicht automatisch aktualisiert, um den neuen Namen des virtuellen Servers widerzuspiegeln.  
  
### <a name="to-rename-a-virtual-server"></a>So benennen Sie einen virtuellen Server um:  
  
1.  Ändern Sie mithilfe der Clusterverwaltung den SQL-Netzwerknamen in den neuen Namen.  
  
2.  Schalten Sie die Netzwerknamenressource offline. Durch diesen Vorgang werden die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Ressource und andere abhängige Ressourcen ebenfalls offline geschaltet.  
  
3.  Schalten Sie die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Ressource erneut online.  
  
## <a name="verify-the-renaming-operation"></a>Überprüfen des Umbenennungsvorgangs  
 Nachdem ein virtueller Server umbenannt wurde, müssen alle Verbindungen, die den alten Namen verwendet haben, nun Verbindungen mithilfe des neuen Namens herstellen.  
  
 Um zu überprüfen, ob der Umbenennungsvorgang abgeschlossen wurde, wählen Sie Informationen aus **@@servername** oder **sys.servers** aus. Die **@@servername** -Funktion gibt den neuen Namen des virtuellen Servers zurück, und die **sys.servers**-Tabelle zeigt den neuen Namen des virtuellen Servers an. Um zu überprüfen, ob der Failoverprozess ordnungsgemäß mit dem neuen Namen arbeitet, sollte der Benutzer außerdem versuchen, ein Failover der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Ressource auf die anderen Knoten auszuführen.  
  
 Für Verbindungen von einem beliebigen Knoten im Cluster kann der neue Name fast sofort verwendet werden. Für Verbindungen, die den neuen Namen von einem Clientcomputer aus verwenden, kann der neue Name jedoch erst zum Herstellen einer Verbindung zum Server verwendet werden, nachdem der neue Name für den betreffenden Clientcomputer sichtbar ist. Die Zeitspanne, die zum Weitergeben des neuen Namens über ein Netzwerk benötigt wird, kann abhängig von der Netzwerkkonfiguration zwischen einigen Sekunden bis hin zu 3 bis 5 Minuten betragen; zusätzliche Zeit ist möglicherweise erforderlich, bis der alte Name des virtuellen Servers nicht mehr im Netzwerk sichtbar ist.  
  
 Um die Verzögerung der Netzwerkweitergabe des Umbenennungsvorgangs eines virtuellen Servers zu minimieren, führen Sie die folgenden Schritte aus:  
  
#### <a name="to-minimize-network-propagation-delay"></a>So minimieren Sie die Verzögerung der Netzwerkweitergabe:  
  
1.  Geben Sie an einer Eingabeaufforderung auf dem Serverknoten die folgenden Befehle aus:  
  
    ```  
    ipconfig /flushdns  
    ipconfig /registerdns  
    nbtstat -RR  
    ```  
  
## <a name="additional-considerations-after-the-renaming-operation"></a>Weitere Überlegungen nach dem Umbenennungsvorgang  
 Nachdem der Netzwerkname des Failoverclusters geändert wurde, müssen die folgenden Anweisungen überprüft und ausgeführt werden, damit alle Szenarien in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent und [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]funktionieren.  
  
 **[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Agent-Dienst:** Überprüfen Sie die unten genannten zusätzlichen Aktionen für den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Agent-Dienst, und führen Sie diese aus:  
  
-   Korrigieren Sie die Registrierungseinstellungen, wenn SQL Agent für die Ereignisweiterleitung konfiguriert ist. Weitere Informationen finden Sie unter [Bestimmen eines Ereignisweiterleitungsservers &#40;SQL Server Management Studio&#41;](../../../ssms/agent/designate-an-events-forwarding-server-sql-server-management-studio.md).  
  
-   Korrigieren Sie die Instanznamen von Masterserver (MSX) und Zielservern (TSX), wenn der Netzwerkname von Computern/Cluster umbenannt wird. Weitere Informationen finden Sie in den folgenden Themen:  
  
    -   [Vollziehen des Austritts mehrerer Zielserver aus einem Masterserver](../../../ssms/agent/defect-multiple-target-servers-from-a-master-server.md)  
  
    -   [Erstellen einer Multiserverumgebung](../../../ssms/agent/create-a-multiserver-environment.md)  
  
-   Konfigurieren Sie den Protokollversand neu, damit der aktualisierte Servername für die Sicherungs- und Wiederherstellungsprotokolle verwendet wird. Weitere Informationen finden Sie in den folgenden Themen:  
  
    -   [Konfigurieren des Protokollversands &#40;SQL Server&#41;](../../../database-engine/log-shipping/configure-log-shipping-sql-server.md)  
  
    -   [Entfernen des Protokollversands &#40;SQL Server&#41;](../../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   Aktualisieren Sie die Auftragsschritte, die vom Servernamen abhängen. Weitere Informationen finden Sie unter [Manage Job Steps](../../../ssms/agent/manage-job-steps.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Umbenennen eines Computers, der eine eigenständige Instanz von SQL Server hostet](../../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)  
  
