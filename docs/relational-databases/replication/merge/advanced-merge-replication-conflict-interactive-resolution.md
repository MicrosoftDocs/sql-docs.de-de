---
description: Erweiterte Konflikte bei der Mergereplikation – Interaktive Lösung
title: Interaktive Konfliktlösung | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- interactive conflict resolution [SQL Server replication]
- interactive resolver [SQL Server replication]
- articles [SQL Server replication], conflict resolution
- conflict resolution [SQL Server replication], merge replication
ms.assetid: 172c60c7-f605-4eb5-b185-54ae9e9d3c60
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 07b8552da25b0ba9d79320c09d3fb4eb8f346532
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91867899"
---
# <a name="advanced-merge-replication-conflict---interactive-resolution"></a>Erweiterte Konflikte bei der Mergereplikation – Interaktive Lösung
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Bei der [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Replikation steht ein interaktiver Konfliktlöser zur Verfügung, mit dem Sie Konflikte bei einer bedarfsgesteuerten Synchronisierung in der Synchronisierungsverwaltung von [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows manuell lösen können. Der interaktive Konfliktlöser, der zur Laufzeit aktiviert wird, ist eine grafische Schnittstelle, mit der Daten für jede konfliktverursachende Zeile angezeigt und Optionen zum Anzeigen und Bearbeiten der Konfliktdaten sowie zum individuellen Lösen aller Konflikte bereitgestellt werden.  
  
 Der interaktive Konfliktlöser ist mit dem Konflikt-Viewer vergleichbar. Der Konflikt-Viewer zeigt jedoch die Ergebnisse von Konflikten an, die nach der Mergesynchronisierung bereits gelöst sind, während der interaktive Konfliktlöser einen Konflikt vor der Lösung anzeigt und Ihnen ermöglicht, das Ergebnis eines Konflikts während der Mergesynchronisierung zu bestimmen. Wenn ein Konflikt auftritt, sollte ein Benutzer zur Verfügung stehen, um den interaktiven Konfliktlöser zu überwachen.  
  
> [!NOTE]  
>  Für die interaktive Konfliktlösung ist die Synchronisierungsverwaltung von Windows erforderlich. Wenn die Synchronisierung nicht im Rahmen der Synchronisierungsverwaltung von Windows erfolgt (sondern als geplante Synchronisierung oder als bedarfsgesteuerte Synchronisierung in [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] oder des Replikationsmonitors), werden Konflikte ohne Benutzereingriff automatisch gemäß dem Konfliktlöser gelöst, der für den Artikel angegeben ist. Konflikte im Zusammenhang mit logischen Datensätzen werden nicht im interaktiven Konfliktlöser angezeigt. Mit den gespeicherten Replikationsprozeduren können Informationen zu diesen Konflikten angezeigt werden. Weitere Informationen finden Sie unter [Anzeigen von Konfliktinformationen zu Mergeveröffentlichungen &#40;Replikationsprogrammierung mit Transact-SQL&#41;](../view-and-resolve-data-conflicts-for-merge-publications.md)  
  
## <a name="article-resolvers-and-the-interactive-resolver"></a>Artikelkonfliktlöser und der interaktive Konfliktlöser  
 Konfliktlöser (entweder der Standardkonfliktlöser, ein Geschäftslogikhandler oder ein benutzerdefinierter Konfliktlöser) werden beim Erstellen einer Veröffentlichung bestimmten Artikeln zugewiesen und verwenden eine Gruppe von vordefinierten Regeln, um die zu verwendende Datengruppe zu bestimmen, wenn konfliktverursachende Zeilendaten eingegeben werden. Der interaktive Konfliktlöser ist kein separater Konfliktlöser mit Regeln zum Bestimmen von Konfliktgewinnern und -verlierern, sondern ein Tool, das zusammen mit dem Standardkonfliktlöser und den benutzerdefinierten Konfliktlösern verwendet wird. Der Artikelkonfliktlöser bestimmt zwar die gewinnende und die verlierende Zeile, der interaktive Konfliktlöser ermöglicht aber den Benutzereingriff, um die Ergebnisse zu akzeptieren, abzulehnen oder zu ändern.  
  
 Damit der interaktive Konfliktlöser verwendet werden kann, muss die interaktive Konfliktlösung für alle Artikel und alle Abonnements aktiviert sein, für die sie erforderlich ist. Wenn der interaktive Konfliktlöser für einen oder mehrere Artikel und Abonnements aktiviert wurde, wird er verwendet, wenn während einer Mergesynchronisierung ein Konflikt erkannt wird.  
  
 Informationen zur Verwendung des interaktiven Konfliktlösers finden Sie unter [Specify Merge Replication options (Angeben von Mergereplikationsoptionen)](../../../relational-databases/replication/merge/specify-merge-replication-properties.md) und [Synchronisieren eines Abonnements mit der Synchronisierungsverwaltung von Windows](../../../relational-databases/replication/synchronize-a-subscription-using-windows-synchronization-manager.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Advanced Merge Replication Conflict Detection and Resolution (Erweiterte Konflikterkennung und -lösung bei der Mergereplikation)](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)  
  
