---
title: Sekundäre Datenbank ist nicht verknüpft | Microsoft-Dokumentation
description: Die Option „Joinzustand der Verfügbarkeitsdatenbank“ überprüft den Joinzustand der sekundären Datenbank im Rahmen der richtlinienbasierten Verwaltung für Always On-Verfügbarkeitsgruppen.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.drp2joined.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 10817e5e-75fa-42dd-baa2-359bea3ad051
author: cawrites
ms.author: chadam
ms.openlocfilehash: 39c9f6852fd1a1cc15a014b67ee693a3cb315787
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344505"
---
# <a name="secondary-database-is-not-joined"></a>Sekundäre Datenbank ist nicht verknüpft
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Einführung  
  
|||  
|-|-|  
|**Richtlinienname**|Joinzustand der Verfügbarkeitsdatenbank|  
|**Problem**|Die sekundäre Datenbank ist nicht verknüpft.|  
|**Kategorie**|**Warning**|  
|**Facet**|Verfügbarkeitsdatenbank|  
  
## <a name="description"></a>BESCHREIBUNG  
 Diese Richtlinie überprüft den Joinstatus der sekundären Datenbank (auch bekannt als "sekundäres Datenbankreplikat"). Die Richtlinie befindet sich in einem fehlerhaften Zustand, wenn das Datenbankreplikat nicht verknüpft wird. Die Richtlinie befindet sich andernfalls in einem ordnungsgemäßen Zustand.  

## <a name="possible-causes"></a>Mögliche Ursachen  
 Diese sekundäre Datenbank ist nicht mit der Verfügbarkeitsgruppe verknüpft. Die Konfiguration dieser sekundären Datenbank ist unvollständig.  
  
## <a name="possible-solution"></a>Mögliche Lösung  
 Verknüpfen Sie das sekundäre Replikat mit der Verfügbarkeitsgruppe mithilfe von Transact-SQL, PowerShell oder SQL Server Management Studio. Weitere Informationen zum Verknüpfen sekundärer Replikate mit Verfügbarkeitsgruppen finden Sie unter [Verknüpfen eines sekundären Replikats mit einer Verfügbarkeitsgruppe (SQL Server)](https://msdn.microsoft.com/library/ff878473\(SQL.110\).aspx).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Verwenden des AlwaysOn-Dashboards &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
