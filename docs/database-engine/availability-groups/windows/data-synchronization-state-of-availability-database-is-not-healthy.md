---
title: Datensynchronisierungsstatus der Verfügbarkeitsdatenbank ist nicht fehlerfrei
description: Bestimmen Sie mögliche Gründe für einen nicht fehlerfreien Status der Datensynchronisierung bei Datenbanken in einer Always On-Verfügbarkeitsgruppe.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: end-user-help
f1_keywords:
- sql13.swb.agdashboard.arp3datasynchealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 4fd003e7-808e-4b0e-b28a-47d9f2616f06
author: MashaMSFT
ms.author: mathoma
manager: erikre
ms.openlocfilehash: fe7678e4c7e3510321df792a1e4c6f94440da909
ms.sourcegitcommit: cc23d8646041336d119b74bf239a6ac305ff3d31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2020
ms.locfileid: "91113297"
---
# <a name="data-synchronization-state-of-availability-database-is-not-healthy-for-an-always-on-availability-group"></a>Datensynchronisierungsstatus der Verfügbarkeitsdatenbank einer Always On-Verfügbarkeitsgruppe ist nicht fehlerfrei
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Einführung  
  
|||  
|-|-|  
|**Richtlinienname**|Datensynchronisierungsstatus der Verfügbarkeitsdatenbank|  
|**Problem**|Der Datensynchronisierungsstatus der Verfügbarkeitsdatenbank ist nicht fehlerfrei.|  
|**Kategorie**|**Warning**|  
|**Facet**|Verfügbarkeitsdatenbank|  
  
## <a name="description"></a>BESCHREIBUNG  
 Diese Richtlinie führt ein Rollup des Datensynchronisierungsstatus aller Verfügbarkeitsdatenbanken (auch bekannt als "Datenbankreplikate") im Verfügbarkeitsreplikat aus. Die Richtlinie befindet sich in einem fehlerhaften Zustand, wenn ein beliebiges Datenbankreplikat nicht den erwarteten Datensynchronisierungsstatus aufweist. Die Richtlinie befindet sich andernfalls in einem ordnungsgemäßen Zustand.  
  
> [!NOTE]  
>  Für dieses Release von [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]finden Sie Informationen zu möglichen Ursachen und Lösungen im TechNet Wiki unter [Datensynchronisierungsstatus einer Verfügbarkeitsdatenbank ist nicht fehlerfrei](https://go.microsoft.com/fwlink/p/?LinkId=220858) .  
  
## <a name="possible-causes"></a>Mögliche Ursachen  
 Der Datensynchronisierungsstatus dieser Verfügbarkeitsdatenbank ist fehlerhaft. Auf einem Verfügbarkeitsreplikat für asynchrone Commits sollte sich jede Verfügbarkeitsdatenbank im Status SYNCHRONIZING befinden. Auf einem Replikat für synchrone Commits muss sich jede Verfügbarkeitsdatenbank im Status SYNCHRONIZED befinden.  
  
## <a name="possible-solution"></a>Mögliche Lösung  
 Verwenden Sie die Datenbankreplikatrichtlinie zum Suchen nach dem Datenbankreplikat mit einem fehlerhaften Datensynchronisierungsstatus, und beheben Sie das Problem für das Datenbankreplikat.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](~/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verwenden des AlwaysOn-Dashboards &#40;SQL Server Management Studio&#41;](~/database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  


