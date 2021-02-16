---
title: Einige synchrone Replikate wurden nicht synchronisiert
description: Hier werden einige mögliche Ursachen und Lösungen für den Fall beschrieben, dass ein synchrones Replikat für eine Always On-Verfügbarkeitsgruppe nicht synchronisiert wird.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.agp5synchronized.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: e58ed56e-4c30-42e6-a9fc-a8c401620e02
author: cawrites
ms.author: chadam
ms.openlocfilehash: 41a4535f1acbf9d871a83011e50fea2ecd700404
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352591"
---
# <a name="some-synchronous-replicas-are-not-synchronized"></a>Einige synchrone Replikate wurden nicht synchronisiert
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Einführung  
  
|||  
|-|-|  
|**Richtlinienname**|Datensynchronisierungsstatus synchroner Replikate|  
|**Problem**|Einige synchrone Replikate wurden nicht synchronisiert.|  
|**Kategorie**|**Warning**|  
|**Facet**|Verfügbarkeitsgruppe|  
  
## <a name="description"></a>BESCHREIBUNG  
 Diese Richtlinie führt ein Rollup des Datensynchronisierungsstatus aller Verfügbarkeitsreplikate sowie eine Überprüfung auf Verfügbarkeitsreplikate durch, die sich nicht im erwarteten Synchronisierungsstatus befinden. Die Richtlinie befindet sich in einem fehlerhaften Zustand, wenn eines der asynchronen Replikate nicht den Status SYNCHRONIZING aufweist und eines der synchronen Replikate nicht den Status SYNCHRONIZED aufweist. Andernfalls befindet sich die Richtlinie in einem ordnungsgemäßen Zustand.  

## <a name="possible-causes"></a>Mögliche Ursachen  
 In dieser Verfügbarkeitsgruppe wird derzeit mindestens ein synchrones Replikat nicht synchronisiert. Der Synchronisierungsstatus des Replikats lautet entweder SYNCHRONIZING oder NOT SYNCHRONIZING.  
  
## <a name="possible-solution"></a>Mögliche Lösung  
 Verwenden Sie den Verfügbarkeitsreplikat-Richtlinienstatus, um nach dem Verfügbarkeitsreplikat mit dem fehlerhaften Synchronisierungsstatus zu suchen, und beheben Sie das Problem für das Verfügbarkeitsreplikat.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verwenden des AlwaysOn-Dashboards &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
