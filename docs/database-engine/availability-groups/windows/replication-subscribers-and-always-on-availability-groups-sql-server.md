---
title: Replikationsabonnenten und Verfügbarkeitsgruppen (SQL Server)
description: Hier erfahren Sie, was geschieht, wenn eine AlwaysOn-Verfügbarkeitsgruppe einen Failover in SQL Server ausführt, die eine Datenbank enthält, bei der es sich um einem Replikationsabonnenten handelt.
ms.custom: seo-lt-2019
ms.date: 08/08/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- failover subscribers with AlwaysOn
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 0995f269-0580-43ed-b8bf-02b9ad2d7ee6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 310438f8a5f8bec6fa0b2b3ef5b17eb1b097f423
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344592"
---
# <a name="replication-subscribers-and-always-on-availability-groups-sql-server"></a>Replikationsabonnenten und Always On-Verfügbarkeitsgruppen (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Wenn für eine Always On-Verfügbarkeitsgruppe mit einer Datenbank, die einen Replikationsabonnenten darstellt, ein Failover ausgeführt wird, tritt u.U. ein Fehler beim Replikationsabonnement auf. Für Transaktionsreplikations-Pushabonnenten setzt der Verteilungs-Agent das Replizieren nach einem Failover automatisch fort, wenn das Abonnement mit dem Namen des Verfügbarkeitsgruppenlisteners erstellt wurde. Für Transaktionsreplikations-Pullabonnenten setzt der Verteilungs-Agent das Replizieren nach einem Failover automatisch fort, wenn das Abonnement mit dem Namen des Verfügbarkeitsgruppenlisteners erstellt wurde und wenn der ursprüngliche Abonnentenserver aktiv ist und ausgeführt wird. Ursache hierfür ist, dass die Verteilungs-Agent-Aufträge nur auf dem ursprünglichen Abonnenten (primäres Replikat der Verfügbarkeitsgruppe) erstellt werden. Bei Mergeabonnenten muss der Abonnent vom Replikationsadministrator manuell neu konfiguriert werden, indem er das Abonnement neu erstellt.  
  
## <a name="what-is-supported"></a>Unterstützte Vorgänge  
 Die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Replikation unterstützt das automatische Failover des Verlegers und das automatische Failover transaktionale Abonnenten. Mergeabonnenten können Teil einer Verfügbarkeitsgruppe sein, jedoch sind manuelle Aktionen erforderlich, um den neuen Abonnenten nach einem Failover zu konfigurieren. Verfügbarkeitsgruppen können nicht mit WebSync- und SQL Server Compact-Szenarios kombiniert werden.  
  
## <a name="how-to-create-transactional-subscription-in-an-always-on-environment"></a>Erstellen eines Transaktionsabonnements in einer Always On-Umgebung  
 Führen Sie bei der Transaktionsreplikation folgende Schritte aus, um die Verfügbarkeitsgruppe eines Abonnenten zu konfigurieren und ein Failover auszuführen:  
  
1.  Bevor Sie das Abonnement erstellen, fügen Sie der entsprechenden Always On-Verfügbarkeitsgruppe die Abonnentendatenbank hinzu.  
  
2.  Fügen Sie den Verfügbarkeitsgruppenlistener des Abonnenten allen Knoten der Verfügbarkeitsgruppe als Verbindungsserver hinzu. Dadurch werden alle potenziellen Failoverpartner informiert, dass der Listener vorhanden und für Verbindungen verfügbar ist.  
  
3.  Erstellen Sie das Abonnement mit dem Namen des Verfügbarkeitsgruppenlisteners des Abonnenten mithilfe des Skripts im Abschnitt **Erstellen eines Pushabonnements für eine Transaktionsreplikation** unten. Der Listenername bleibt nach einem Failover weiterhin gültig, während der tatsächliche Servername des Abonnenten von dem Knoten abhängig ist, der zum neuen primären Replikat wird.  
  
    > [!NOTE]  
    >  Das Abonnement muss mithilfe eines [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Skripts erstellt werden, nicht mit [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].  
  
4.  So erstellen Sie ein Pullabonnement:  
  
    1.  Erstellen Sie das Abonnement mit dem Namen des Verfügbarkeitsgruppenlisteners des Abonnenten mithilfe des Beispielskripts im Abschnitt **Erstellen eines Pullabonnements für eine Transaktionsreplikation** unten. 
   
    2.  Erstellen Sie nach einem Failover den Verteilungs-Agent-Auftrag mithilfe der gespeicherten Prozedur **sp_addpullsubscription_agent** auf dem neuen primären Replikat. 
  
 Wenn Sie ein Pullabonnement mit der Abonnementdatenbank in einer Verfügbarkeitsgruppe erstellen, sollten Sie nach jedem Failover den Verteilungs-Agent-Auftrag auf dem alten primären Replikat deaktivieren und den Auftrag auf dem neuen primären Replikat aktivieren.  
  
## <a name="creating-a-transactional-replication-push-subscription"></a>Erstellen eines Pushabonnements für eine Transaktionsreplikation  
  
```  
-- commands to execute at the publisher, in the publisher database:  
use [<publisher database name>]  
EXEC sp_addsubscription @publication = N'<publication name>',   
       @subscriber = N'<availability group listener name>',   
       @destination_db = N'<subscriber database name>',   
       @subscription_type = N'Push',   
       @sync_type = N'automatic', @article = N'all', @update_mode = N'read only', @subscriber_type = 0;  
GO  
  
EXEC sp_addpushsubscription_agent @publication = N'<publication name>',   
       @subscriber = N'<availability group listener name>',   
       @subscriber_db = N'<subscriber database name>',   
       @job_login = null, @job_password = null, @subscriber_security_mode = 1;  
GO  
```  

## <a name="creating-a-transactional-replication-pull-subscription"></a>Erstellen eines Pullabonnements für eine Transaktionsreplikation  
  
```  
-- commands to execute at the subscriber, in the subscriber database:  
use [<subscriber database name>]  
EXEC sp_addpullsubscription @publisher= N'<publisher name>',
        @publisher_db= N'<publisher database name>',
        @publication= N'<publication name>',
        @subscription_type = N'pull';
Go

EXEC sp_addpullsubscription_agent 
        @publisher =  N'<publisher name>', 
        @subscriber = N'<availability group listener name>',
        @publisher_db= N'<publisher database name>',
        @publication= N'<publication name>' ;
        @job_login = null, @job_password = null, @subscriber_security_mode = 1;  
GO
```  
  
## <a name="to-resume-the-merge-agents-after-the-availability-group-of-the-subscriber-fails-over"></a>So setzen Sie Merge-Agents nach einem Verfügbarkeitsgruppenfailover des Abonnenten fort  
 Bei einer Mergereplikation muss der Abonnent mithilfe folgender Schritte von einem Replikationsadministrator manuell neu konfiguriert werden:  
  
1.  Führen Sie **sp_subscription_cleanup** aus, um das alte Abonnement für den Abonnenten zu entfernen. Führen Sie diese Aktion für das neue primäre Replikat aus (das zuvor das sekundäre Replikat war).  
  
2.  Erstellen Sie das Abonnement neu, indem Sie ein neues Abonnement auf Grundlage einer neuen Momentaufnahme erstellen.  
  
> [!NOTE]  
>  Das aktuelle Verfahren ist für Mergereplikationsabonnenten nicht geeignet, die Mergereplikation erfolgt jedoch hauptsächlich für nicht verbundene Benutzer (Desktops, Laptops, Mobiltelefone), die keine Always On-Verfügbarkeitsgruppen auf dem Abonnenten nutzen.  
  
  
