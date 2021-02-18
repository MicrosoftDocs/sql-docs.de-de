---
title: 'Installieren von Clienttools: Failovercluster'
description: Hier erfahren Sie, wie Sie Clienttools wie das SQL Server Management Studio auf einer SQL Server-Failoverinstanz installieren.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: 3c82d510-9798-46be-bebb-cac9bef56936
author: cawrites
ms.author: chadam
ms.openlocfilehash: b5147a57dd5915ef850b1a05cd6e21a78a2932f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351737"
---
# <a name="install-client-tools-on-a-sql-server-failover-cluster"></a>Installieren von Clienttools auf einem SQL Server-Failovercluster
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Clienttools wie [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] sind Funktionen, die von allen Instanzen auf einem Computer genutzt werden. Sie sind rückwärtskompatibel. Unterstützte [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Versionen können parallel installiert werden. Nur eine Version des Clienttools ist jeweils auf einem Knoten vorhanden.  
  
 Wenn die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Clienttools während des Setups auf dem ersten Knoten des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Clusters installiert werden, werden sie automatisch allen Knoten hinzugefügt, die unter Umständen später mit Knoten hinzufügen der Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] hinzugefügt werden.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Onlinedokumentation wird nicht automatisch den zusätzlichen Knoten hinzugefügt, die dem [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Cluster mit der Option Knoten hinzufügen hinzugefügt werden. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Onlinedokumentation kann manuell in den Knoten installiert werden, die eine lokale Kopie der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Onlinedokumentation aufweisen sollen.  
  
 Wenn Sie die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Clienttools nicht während der ersten Installation des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Clusters installieren, können Sie sie später wie unten beschrieben installieren.  
  
## <a name="installation-procedures"></a>Installationsprozeduren  
  
#### <a name="installing-ssnoversion-client-tools-using-the-setup-user-interface"></a>Installieren der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Clienttools mit dem Setup für die Benutzeroberfläche  
  
1.  Legen Sie das [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Installationsmedium ein. Doppelklicken Sie im Stamminstallationsordner auf Setup.exe. Wenn Sie die Installation über eine Netzwerkfreigabe vornehmen möchten, suchen Sie den Stammordner in der Freigabe, und doppelklicken Sie auf Setup.exe.  
  
2.  Klicken Sie auf der Seite **Installation** auf **Neue eigenständige[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Installation oder Hinzufügen von Funktionen zu einer vorhandenen Installation**. Klicken Sie nicht auf **Neue [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Failoverclusterinstallation**.  
  
3.  Die Systemkonfigurationsprüfung überprüft den Systemstatus des Computers, bevor Setup fortgesetzt wird.  
  
4.  Klicken Sie auf der Seite **Installationstyp** auf **Neuinstallation von [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] ausführen**.  
  
5.  Wählen Sie auf der Seite **Funktionsauswahl** die zu installierenden Tools aus, und führen Sie die restlichen Schritte des Setupvorgangs durch.  
  
#### <a name="installing-ssnoversion-client-tools-at-the-command-prompt"></a>Installieren von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Clienttools über die Eingabeaufforderung  
  
1.  Führen Sie die folgenden Befehl aus, um die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Clienttools und die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Onlinedokumentation zu installieren: Setup.exe/q/Action=Install /Features=Tools  
  
2.  Führen Sie den folgenden Befehl aus, um nur die grundlegenden [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Verwaltungstools zu installieren: Setup.exe/q/Action=Install Features=SSMS. Damit wird die [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]-Unterstützung für [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)], [!INCLUDE[ssExpress](../../../includes/ssexpress-md.md)], das Hilfsprogramm sqlcmd und den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Anbieter installiert.  
  
3.  Führen Sie den folgenden Befehl aus, um alle [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Verwaltungstools zu installieren: Setup.exe/q/Action=Install /Features=ADV_SSMS. Weitere Informationen zu den Parameterwerten für die Funktionen finden Sie unter [Installieren von SQL Server 2016 von der Eingabeaufforderung](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).  
  
### <a name="uninstalling-ssnoversion-client-tools"></a>Deinstallieren von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Clienttools  
 Sie werden in der Systemsteuerung unter Software als **[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]** angezeigt und können dort auch entfernt werden. Wenn Sie die Option Knoten entfernen zum Deinstallieren einer Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] im Failovercluster verwenden, werden die Clientkomponenten nicht gleichzeitig deinstalliert.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Lesen und Anzeigen der Setupprotokolldateien von SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
