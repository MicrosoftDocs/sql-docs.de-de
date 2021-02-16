---
title: Erstellen und Analysieren von „CLUSTER.LOG“ für Verfügbarkeitsgruppen
description: In diesem Artikel finden Sie Informationen zum Generieren und Analysieren des Clusterprotokolls für eine AlwaysOn-Verfügbarkeitsgruppe.
ms.custom: seo-lt-2019
ms.date: 06/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 01a9e3c1-2a5f-4b98-a424-0ffc15d312cf
author: rothja
ms.author: jroth
ms.openlocfilehash: c674f16110b6c1161e543d7e6871fddceb02f6bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343754"
---
# <a name="generate-and-analyze-the-clusterlog-for-an-always-on-availability-group"></a>Erstellen und Analysieren der Protokolldatei „CLUSTER.LOG“ für eine Always On-Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Als Failoverclusterressource stehen externe Interaktionen zwischen SQL Server, dem WSFC-Cluster (Windows Server-Failovercluster) und der SQL Server-Ressourcen-DLL (hadrres.dll) zur Verfügung, die innerhalb von SQL Server nicht überwacht werden können. Das WSFC-Protokoll, CLUSTER.LOG, kann Probleme im WSFC-Cluster oder in der SQL Server-Ressourcen-DLL diagnostizieren. 
  
## <a name="generate-cluster-log"></a>Generieren einer Clusterprotokolldatei  
 Sie können die Clusterprotokolldateien auf zwei Arten generieren:  
  
1.  Verwenden Sie den Befehl `cluster /log /g` an der Eingabeaufforderung. Mit diesem Befehl werden auf jedem WSFC-Knoten die Clusterprotokolldateien im Verzeichnis „\windows\cluster\reports“ generiert. Der Vorteil dieser Methode besteht darin, dass Sie mithilfe der Option `/level` die Detailebene in den generierten Protokollen angeben können. Der Nachteil ist, dass Sie kein Zielverzeichnis für die generierten Clusterprotokolldateien festlegen können. Weitere Informationen finden Sie unter [How to create the cluster.log in Windows Server 2008 Failover Clustering](https://techcommunity.microsoft.com/t5/failover-clustering/how-to-create-the-cluster-log-in-windows-server-2008-failover/ba-p/371283) (Gewusst wie: Erstellen von „cluster.log“ im Windows Server 2008-Failoverclustering).  
  
2.  Verwenden Sie das PowerShell-Cmdlet [Get-ClusterLog](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10)). Der Vorteil dieser Methode ist, dass Sie das Clusterprotokolldatei von allen Knoten in einem Zielverzeichnis auf dem Knoten generieren können, auf dem Sie das Cmdlet ausführen. Der Nachteil ist, dass Sie die Detailebene in den generierten Protokollen nicht festlegen können.  
  
 Die folgenden PowerShell-Befehle generieren von allen Clusterknoten der letzten 15 Minuten Clusterprotokolldateien und platzieren diese im aktuellen Verzeichnis. Führen Sie die Befehle in einem PowerShell-Fenster mit Administratorrechten aus.  
  
```powershell  
Import-Module FailoverClusters   
Get-ClusterLog -TimeSpan 15 -Destination .  
```  
  
## <a name="always-on-log-verbosity"></a>Ausführlichkeit von Always On-Protokollen  
 Sie können den Ausführlichkeitsgrad der Protokolle in CLUSTER.LOG für eine Verfügbarkeitsgruppe erhöhen. Um den Ausführlichkeitsgrad zu ändern, führen Sie die folgenden Schritte aus:  
  
1.  Öffnen Sie im Menü **Start** den **Failovercluster-Manager**.  
  
2.  Erweitern Sie Ihren Cluster und den Knoten **Dienste und Anwendungen**, und klicken Sie dann auf den Namen der Verfügbarkeitsgruppe.  
  
3.  Klicken Sie im Detailbereich mit der rechten Maustaste auf die Verfügbarkeitsgruppenressource, und klicken Sie auf **Eigenschaften**.  
  
4.  Klicken Sie auf die Registerkarte **Eigenschaften**.  
  
5.  Ändern Sie die Eigenschaft **VerboseLogging**. Standardmäßig wird **VerboseLogging**, das Informationen, Warnungen und Fehler meldet, auf `0` festgelegt. **VerboseLogging** kann von `0` in `2` geändert werden.  
  
6.  Klicken Sie auf **OK**.  
  
7.  Klicken Sie mit der rechten Maustaste erneut auf die Verfügbarkeitsgruppenressource, und klicken Sie auf **Diese Ressource offline schalten**.  
  
8.  Klicken Sie mit der rechten Maustaste erneut auf die Verfügbarkeitsgruppenressource, und klicken Sie auf **Diese Ressource online schalten**.  
  
## <a name="availability-group-resource-events"></a>Ereignisse der Verfügbarkeitsgruppenressource  
 Die folgende Tabelle zeigt die verschiedenen Arten von Ereignissen in CLUSTER.LOG, die die Verfügbarkeitsgruppenressource betreffen. Weitere Informationen zum Ressourcenhosting-Subsystem (RHS) und dem Ressourcensteuerungsmonitor (RCM) in WSFC finden Sie unter [Resource Hosting Subsystem (RHS) In Windows Server 2008 Failover Clusters](/archive/blogs/askcore/resource-hosting-subsystem-rhs-in-windows-server-2008-failover-clusters) (Ressourcenhosting-Subsystem (RHS) In Windows Server 2008-Failoverclustern).  
  
|Bezeichner|`Source`|Beispiel aus CLUSTER.LOG|  
|----------------|------------|------------------------------|  
|Nachrichten mit dem Präfix `[RES]` und `[hadrag]`|hadrres.dll (Always On-Ressourcen-DLL)|00002cc4.00001264::2011/08/05-13:47:42.543 INFO [RES] SQL Server Availability Group \<ag>: `[hadrag]` Offline request. (00002cc4.00001264::2011/08/05-13:47:42.543 INFO [RES] SQL Server-Verfügbarkeitsgruppe \<ag>ag>:`[hadrag]` Offlineanforderung.)<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.558 ERR [RES] SQL Server Availability Group \<ag>: `[hadrag]` Lease Thread terminated (00002cc4.00003384::2011/08/05-13:47:42.558 ERR [RES] SQL Server-Verfügbarkeitsgruppe \<ag>ag>:`[hadrag]` Leasethread beendet)<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.605 INFO [RES] SQL Server Availability Group \<ag>: `[hadrag]` Free SQL statement (00002cc4.00003384::2011/08/05-13:47:42.605 INFO [RES] SQL Server-Verfügbarkeitsgruppe \<ag>ag>: `[hadrag]`Freie SQL-Anweisung)<br /><br /> 00002cc4.00003384::2011/08/05-13:47:42.902 INFO [RES] SQL Server Availability Group \<ag>: `[hadrag]` Disconnect from SQL Server (00002cc4.00003384::2011/08/05-13:47:42.902 INFO [RES] SQL Server-Verfügbarkeitsgruppe \<ag>ag>: `[hadrag]` Verbindung mit SQL Server trennen)|  
|Nachrichten mit dem Präfix `[RHS]`|RHS.EXE (Ressourcenhosting-Subsystem, Hostprozess von „hadrres.dll“)|00000c40.00000a34::2011/08/10-18:42:29.498 INFO  [RHS] Resource ag has come offline. RHS is about to report resource status to RCM.|  
|Nachrichten mit dem Präfix `[RCM]`|Ressourcensteuerungsmonitor (Clusterdienst)|000011d0.00000f80::2011/08/05-13:47:42.480 INFO [RCM] rcm::RcmGroup::Move: Bringing group 'ag' offline first... (000011d0.00000f80::2011/08/05-13:47:42.480 INFO [RCM] rcm::RcmGroup::Move: Zuerst wird die Gruppe 'ag' offline geschaltet.<br /><br /> 000011d0.00000f80::2011/08/05-13:47:42.496 INFO  [RCM] TransitionToState(ag) Online-->OfflineCallIssued.|  
|RcmApi/ClusAPI|Ein API-Aufruf, der hauptsächlich bedeutet, dass SQL Server die Aktion anfordert|000011d0.00000f80::2011/08/05-13:47:42.465 INFO  [RCM] rcm::RcmApi::MoveGroup: (ag, 2)|  
  
## <a name="debug-always-on-resource-dll-in-isolation"></a>Debuggen einer isolierten Always On-Ressourcen-DLL  
 Zum Debuggen empfiehlt es sich, Ihren Cluster für die Ausführung der Always On-Ressourcen-DLL (hadrres.dll) isoliert von anderen Ressourcen-DLLs zu konfigurieren. Standardmäßig führt der WSFC-Cluster alle Ressourcen-DLLs in einer einzigen Instanz von „rhs.exe“ aus. Dies führt dazu, dass alle Ressourcen innerhalb des Clusters gemeinsam dieselbe Instanz von „rhs.exe“ verwenden. Wenn Sie versuchen, „hadrres.dll“ mit einem Debugger zu debuggen, der an einem Haltepunkt anhält, kann dies dazu führen, dass andere Ressourcen, die gemeinsam die Instanz von „rhs.exe“ verwenden, ebenfalls angehalten werden. Auch wenn Sie mehrere Verfügbarkeitsgruppen im selben Cluster ausführen, kann durch diese Konfiguration zum Debuggen einer Verfügbarkeitsgruppe bewirkt werden, dass alle Verfügbarkeitsgruppen angehalten werden, wenn Sie an einem Haltepunkt anhalten.  
  
 Um eine Verfügbarkeitsgruppe von den anderen Clusterressourcen-DLLs wie anderen Verfügbarkeitsgruppen zu isolieren, gehen Sie wie folgt vor, um „hadrres.dll“ in einem separaten „rhs.exe“-Prozess auszuführen:  
  
1.  Öffnen Sie den **Registrierungs-Editor**, und navigieren Sie zum folgenden Registrierungsschlüssel: HKEY_LOCAL_MACHINE\Cluster\Resources. Dieser Schlüssel enthält die Schlüssel für alle Ressourcen, jeweils mit einer anderen GUID.  
  
2.  Suchen Sie den Ressourcenschlüssel mit einem **Name**-Wert, der dem Namen Ihrer Verfügbarkeitsgruppe entspricht.  
  
3.  Ändern Sie den Wert **SeparateMonitor** in **1**.  
  
4.  Starten Sie den Clusterdienst für Ihre Verfügbarkeitsgruppe im WSFC-Cluster neu.  
  
