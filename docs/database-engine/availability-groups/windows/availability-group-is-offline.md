---
title: Verfügbarkeitsgruppe ist offline
description: Hier erfahren Sie, wie Sie die Zeiten und möglichen Gründe dafür identifizieren, wieso eine AlwaysOn-Verfügbarkeitsgruppe offline geschaltet wird.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.agdashboard.agp2online.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 093c5208-bf7a-49f4-a546-72b48197cadf
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8a1de6e359fbb74b3d53a4c86851e9c3fdc4570c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340885"
---
# <a name="always-on-availability-group-is-offline"></a>Always On-Verfügbarkeitsgruppe ist offline
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Einführung  
  
|||  
|-|-|  
|**Richtlinienname**|Onlinezustand der Verfügbarkeitsgruppe|  
|**Problem**|Die Verfügbarkeitsgruppe ist offline.|  
|**Kategorie**|**Critical** (Kritisch)|  
|**Facet**|Verfügbarkeitsgruppe|  
  
## <a name="description"></a>BESCHREIBUNG  
 Diese Richtlinie überprüft den Online- oder Offlinestatus der Verfügbarkeitsgruppe. Die Richtlinie befindet sich in einem fehlerhaften Zustand, und eine Warnung wird ausgelöst, wenn die Clusterressource der Verfügbarkeitsgruppe offline ist oder wenn die Verfügbarkeitsgruppe nicht über ein primäres Replikat verfügt.  
  
 Der Zustand der Richtlinie ist fehlerfrei, wenn die Clusterressource der Verfügbarkeitsgruppe online ist und die Verfügbarkeitsgruppe über ein primäres Replikat verfügt.
  
## <a name="possible-causes"></a>Mögliche Ursachen  
 Dieses Problem kann von einem Fehler in der Serverinstanz verursacht werden, die das primäre Replikat hostet, oder dadurch, dass die Windows Server-Failovercluster (WSFC)-Verfügbarkeitsgruppenressource offline geht. Im Folgenden sind mögliche Ursachen für den Offlinezustand der Verfügbarkeitsgruppe aufgeführt:  
  
-   Die Verfügbarkeitsgruppe ist nicht für den automatischen Failovermodus konfiguriert. Das primäre Replikat ist nicht mehr verfügbar, und alle Replikate in der Verfügbarkeitsgruppe nehmen den Status RESOLVING an.  
  
    -   Der Instanzdienst des primären Replikats ist ausgefallen oder reagiert nicht.  
  
    -   Für die Verfügbarkeitsgruppe besteht ein Problem bei der Verbindung zum Cluster.  
  
-   Die Verfügbarkeitsgruppe wurde mit automatischem Failovermodus konfiguriert und nicht erfolgreich abgeschlossen.  
  
    -   Während des automatischen Failovers tritt für die primäre Bereitschaftsüberprüfung auf dem Zielreplikat ein Fehler auf, und es ist kein Replikat verfügbar, das zum neuen primären Replikat werden kann.  
  
-   Die Verfügbarkeitsgruppenressource im Cluster ist offline.  
  
    -   In einer abhängigen Clusterressource tritt ein schwerwiegendes Problem auf, und die Ressource wird in den Offlinezustand versetzt. Die Verfügbarkeitsgruppenressource ist ebenfalls offline, bis die abhängige Ressource online geschaltet wird.  
  
    -   Durch ein schwerwiegendes Problem im Cluster wird die Verfügbarkeitsgruppenressource deaktiviert.  
  
-   Für die Verfügbarkeitsgruppe wird ein automatisches, manuelles oder erzwungenes Failover durchgeführt.  
  
## <a name="possible-solutions"></a>Mögliche Lösungen  
 Für dieses Problem gibt es die folgenden möglichen Lösungen:  
  
-   Wenn die SQL Server-Instanz des primären Replikats ausgefallen ist, starten Sie den Server neu, und überprüfen Sie dann, ob die Verfügbarkeitsgruppe wieder einen ordnungsgemäßen Zustand erreicht.  
  
-   Falls anscheinend für das automatische Failover ein Fehler aufgetreten ist, überprüfen Sie, ob die Datenbanken auf dem Replikat mit dem zuvor bekannten primären Replikat synchronisiert werden, und führen Sie dann das Failover auf das primäre Replikat durch. Falls die Datenbanken nicht synchronisiert werden, wählen Sie ein Replikat mit einem minimalen Datenverlust aus und stellen dann den Failovermodus wieder her.  
  
-   Falls die Ressource im Cluster offline ist, während die Instanzen von SQL Server anscheinend fehlerfrei sind, verwenden Sie den Failovercluster-Manager, um den Clusterzustand oder andere Clusterprobleme auf dem Server zu überprüfen. Sie können mit dem Failovercluster-Manager auch versuchen, die Verfügbarkeitsgruppenressource wieder in den Onlinezustand zu versetzen.  
  
-   Falls gerade ein Failover durchgeführt wird, warten Sie, bis das Failover abgeschlossen ist.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verwenden des AlwaysOn-Dashboards &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
