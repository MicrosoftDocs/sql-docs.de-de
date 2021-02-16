---
description: Reparieren von Fehlern bei einer SQL Server-Installation
title: Reparieren von Fehlern bei einer SQL Server-Installation | Microsoft-Dokumentation
deescription: This article describes the scenarios where you can try a repair operation to fix failed SQL Server installation.
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 90c11b28-892b-44d6-978e-0eee48c75b7d
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 4edcf1335066d29c3f9a471417862f7f3967a92e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351596"
---
# <a name="repair-a-failed-sql-server-installation"></a>Reparieren von Fehlern bei einer SQL Server-Installation

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Der Reparaturvorgang kann in den folgenden Szenarien verwendet werden:  
  
- Reparieren einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , die beschädigt ist, nachdem sie erfolgreich installiert wurde. 
  
- Reparieren einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , falls der Upgradevorgang abgebrochen wird oder ein Fehler auftritt, nachdem der Instanzname der neu aktualisierten Instanz zugeordnet wird. 
  
    - Wenn im Zusammenfassungsprotokoll die folgende Meldung angezeigt wird, können Sie die fehlerhafte Upgradeinstanz reparieren:  
  
         "Fehler beim Upgrade von[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ." Untersuchen Sie die Fehlerursache, beheben Sie das Problem, und reparieren Sie die Installation, um den Vorgang fortzusetzen."  
  
    - Wenn im Zusammenfassungsprotokoll die folgende Meldung angezeigt wird, müssen Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]deinstallieren und neu installieren. Sie können die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz dann nicht reparieren. 
  
         "Fehler beim Upgrade von[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ." Untersuchen Sie die Fehlerursache, und beheben Sie das Problem, um den Vorgang fortzusetzen."  
  
 Beim Reparieren einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]wird Folgendes durchgeführt:  
  
- Alle fehlenden oder beschädigten Dateien werden ersetzt. 
  
- Alle fehlenden oder beschädigten Registrierungsschlüssel werden ersetzt. 
  
- Alle fehlenden oder ungültigen Konfigurationswerte werden auf die Standardwerte festgelegt. 
  
 Bevor Sie den Vorgang fortsetzen, sollten Sie für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Failovercluster die folgenden wichtigen Informationen überprüfen:  
  
- Die Reparatur muss auf jedem Clusterknoten einzeln ausgeführt werden. 
  
- Verwenden Sie zum Reparieren eines Failoverclusterknotens nach Fehlern bei einem Vorbereitungsvorgang die Option **Knoten entfernen** , und führen Sie anschließend den Vorbereitungsschritt erneut aus. Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen von Knoten in einem SQL Server-Failovercluster &#40;Setup&#41;](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md). 
  
## <a name="repair-a-failed-installation-of-ssnoversion-from-the-installation-center"></a>Reparieren einer fehlerhaften Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] über das Installationscenter 
  
1. Starten Sie auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installationsmedium das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setupprogramm (setup.exe). 
  
2. Nachdem die erforderlichen Komponenten installiert wurden und die Systemüberprüfung durchgeführt wurde, zeigt das Setupprogramm das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installationscenter an. 
  
3. Klicken Sie im linken Navigationsbereich auf **Wartung** , und klicken Sie dann auf **Reparieren** , um den Reparaturvorgang zu starten. 
  
   >[!TIP]  
   > Wenn das Installationscenter über das Startmenü gestartet wurde, müssen Sie zu dieser Zeit den Speicherort der Installationsmedien angeben. 
  
4. Es werden Unterstützungsregeln für Setup und Dateiroutinen ausgeführt, um sicherzustellen, dass die erforderlichen Komponenten auf dem System installiert sind und dass der Computer den Setupüberprüfungsregeln entspricht. Klicken Sie zum Fortsetzen auf **OK** oder auf **Installieren** . 
  
5. Wählen Sie auf der Seite Instanz auswählen die zu reparierende Instanz aus, und klicken Sie dann zum Fortsetzen auf **Weiter** . 
  
6. Die Reparaturregeln werden ausgeführt, um den Vorgang zu überprüfen. Klicken Sie auf **Weiter**, um den Vorgang fortzusetzen. 
  
7. Wenn die Seite Bereit zum Reparieren angezeigt wird, kann der Vorgang fortgesetzt werden. Klicken Sie zum Fortsetzen auf **Reparieren**. 
  
8. Auf der Seite Reparaturstatus wird der Status des Reparaturvorgangs angezeigt. Wenn die Seite Abgeschlossen angezeigt wird, wurde der Vorgang abgeschlossen. 
  
### <a name="to-repair-a-failed-installation-of-ssnoversion-using-command-prompt"></a>So reparieren Sie eine fehlerhafte Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] über die Eingabeaufforderung  
  
1. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus:  
  
    ```  
    Setup.exe /q /ACTION=Repair /INSTANCENAME=instancename  
    ```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Lesen und Anzeigen der Setupprotokolldateien von SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)   
 [Artikel zu Vorgehensweisen für die Installation](/previous-versions/sql/)  
  
