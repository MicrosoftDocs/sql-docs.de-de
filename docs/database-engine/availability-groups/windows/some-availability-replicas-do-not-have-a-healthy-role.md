---
title: Einige Verfügbarkeitsreplikate haben keine fehlerfreie Rolle | Microsoft-Dokumentation
description: Mit dem Verfügbarkeitsreplikat-Rollenstatus wird überprüft, ob Verfügbarkeitsreplikate vorhanden sind, die keine fehlerfreie Rolle aufweisen.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp6allroleshealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 7ec5b337-7201-4a66-a541-7560f8b18784
author: cawrites
ms.author: chadam
ms.openlocfilehash: ce3cb14ae7e45cfa076a10f5617525b882e3ba14
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766100"
---
# <a name="some-availability-replicas-do-not-have-a-healthy-role"></a>Einige Verfügbarkeitsreplikate haben keine fehlerfreie Rolle
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Einführung  
  
|||  
|-|-|  
|**Richtlinienname**|Verfügbarkeitsreplikat-Rollenstatus|  
|**Problem**|Einige Verfügbarkeitsreplikate haben keine fehlerfreie Rolle.|  
|**Kategorie**|**Warning**|  
|**Facet**|Verfügbarkeitsgruppe|  
  
## <a name="description"></a>BESCHREIBUNG  
 Diese Richtlinie führt ein Rollup des Verbindungsstatus aller Verfügbarkeitsreplikate aus und überprüft, ob Verfügbarkeitsreplikate vorhanden sind, die keine fehlerfreie Rolle aufweisen. Die Richtlinie befindet sich in einem fehlerhaften Zustand, wenn jedes Verfügbarkeitsreplikat weder primär noch sekundär ist. Die Richtlinie befindet sich andernfalls in einem ordnungsgemäßen Zustand.  
  
## <a name="possible-causes"></a>Mögliche Ursachen  
 In dieser Verfügbarkeitsgruppe verfügt mindestens ein Verfügbarkeitsreplikat derzeit nicht über die primäre oder sekundäre Rolle.  
  
## <a name="possible-solution"></a>Mögliche Lösung  
 Verwenden Sie den Verfügbarkeitsreplikat-Richtlinienstatus, um nach dem Verfügbarkeitsreplikat zu suchen, dessen Rolle nicht primär oder sekundär ist, und beheben Sie das Problem des Verfügbarkeitsreplikats.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verwenden des AlwaysOn-Dashboards &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
