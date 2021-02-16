---
title: Keine fehlerfreie Rolle von Replikaten für eine Verfügbarkeitsgruppe
description: Identifizieren Sie mögliche Ursachen dafür, warum ein Verfügbarkeitsreplikat keine fehlerfreie Rolle innerhalb einer Always On-Verfügbarkeitsgruppe hat.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: troubleshooting
f1_keywords:
- sql13.swb.agdashboard.arp1rolehealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: ebb2c9f4-2097-4688-b4fb-8f0571047317
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1f74f004d65f4fb5191ab562142204898fc3453d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343854"
---
# <a name="availability-replica-does-not-have-a-healthy-role-for-an-always-on-availability-group"></a>Verfügbarkeitsreplikat hat keine fehlerfreie Rolle für eine Always On-Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Einführung  
  
|||  
|-|-|  
|**Richtlinienname**|Verfügbarkeitsreplikat-Rollenstatus|  
|**Problem**|Das Verfügbarkeitsreplikat hat keine fehlerfreie Rolle.|  
|**Kategorie**|**Critical** (Kritisch)|  
|**Facet**|Verfügbarkeitsreplikat|  
  
## <a name="description"></a>BESCHREIBUNG  
 Diese Richtlinie überprüft den Status der Verfügbarkeitsreplikatrolle. Die Richtlinie befindet sich in einem fehlerhaften Zustand, wenn die Rolle des Verfügbarkeitsreplikats weder primär noch sekundär ist. Die Richtlinie befindet sich andernfalls in einem ordnungsgemäßen Zustand.  
  
## <a name="possible-causes"></a>Mögliche Ursachen  
 Die Rolle dieses Verfügbarkeitsreplikats ist fehlerhaft. Das Replikat hat nicht entweder die primäre oder sekundäre Rolle inne.  
  
## <a name="possible-solution-information_still_to_come"></a>Mögliche Lösung: Informationen_werden_noch_bereitgestellt  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verwenden des AlwaysOn-Dashboards &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
