---
description: Interaktiver Microsoft-Replikationskonfliktlöser
title: Interaktiver Konfliktlöser (Merge)
describes: Describes the Interactive Conflict Resolver that can be used for merge subscriptions that are synchronized using the Windows Synchronization Manager.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.replconflictviewer.interactiveresolver.f1
ms.assetid: d3d4a480-782b-4b1d-b839-565c8cf6cb24
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 242e6847c2c00177ba9939781d56831d8cb9492a
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88482430"
---
# <a name="microsoft-replication-interactive-conflict-resolver"></a>Interaktiver Microsoft-Replikationskonfliktlöser
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Der interaktive Replikationskonfliktlöser kann für Mergeabonnements verwendet werden, die mithilfe der Synchronisierungsverwaltung von Windows synchronisiert werden. Er ermöglicht es Ihnen, das Ergebnis von Datenkonflikten anzuzeigen, zu vergleichen, zu bearbeiten und auszuwählen. Die Replikation beinhaltet auch den Konflikt-Viewer, der es Ihnen ermöglicht, die Ergebnisse von Konflikten anzuzeigen und zu ändern, nachdem für sie ein Commit ausgeführt wurde. Mithilfe des interaktiven Konfliktlösers können Sie das Ergebnis während der Synchronisierung auswählen.  
  
> [!NOTE]  
>  Konflikte im Zusammenhang mit logischen Datensätzen werden nicht im interaktiven Konfliktlöser angezeigt. Mit den gespeicherten Replikationsprozeduren können Informationen zu diesen Konflikten angezeigt werden. Weitere Informationen finden Sie unter [Anzeigen von Konfliktinformationen zu Mergeveröffentlichungen &#40;Replikationsprogrammierung mit Transact-SQL&#41;](../../relational-databases/replication/view-conflict-information-for-merge-publications.md)  
  
## <a name="options"></a>Tastatur  
 **Spaltenname**  
 Der Name aller Spalten in der Tabelle Die Daten einer oder mehrerer Spalten können Konflikte verursachen. Unabhängig davon, zwischen welchen Spalten es zu Konflikten kommt, überschreibt die gesamte gewinnende Zeile die gesamte verlierende Zeile.  
  
 **Vorgeschlagene Lösung**  
 Die vom Konfliktlöser für den Artikel vorgeschlagene Lösung.  
  
 **Publisher**  
 Der Datenwert auf dem Verleger.  
  
 **Abonnent**  
 Der Datenwert auf dem Abonnenten.  
  
 **Vorschläge akzeptieren**, **Verleger akzeptieren**und **Abonnenten akzeptieren**  
 Klicken Sie, je nachdem wer den Konflikt verloren hat, entweder auf Abonnent oder Verleger, um die anzuwendende Zeile anzunehmen. Wenn der Verleger den Konflikt verloren hat, empfangen alle anderen Abonnenten bei der nächsten Synchronisierung mit dem Verleger die gewinnende Zeile.  
  
 **Verbleibende Konflikte automatisch lösen**  
 Lösen Sie alle verbleibenden Konflikte anhand der für den Artikel vom Konfliktlöser vorgeschlagenen Lösung.  
  
 **Details dieses Konflikts zur späteren Bezugnahme protokollieren**  
 Protokollieren Sie die Details des Konflikts in Systemtabellen.  
  
## <a name="see-also"></a>Siehe auch  
 [Interaktive Konfliktlösung](../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md)   
 [Anzeigen und Lösen von Datenkonflikten für Mergeveröffentlichungen &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/view-and-resolve-data-conflicts-for-merge-publications.md)   
 [Synchronisieren eines Abonnements mithilfe der Synchronisierungsverwaltung von Windows &#40;Synchronisierungsverwaltung von Windows&#41;](../../relational-databases/replication/synchronize-a-subscription-using-windows-synchronization-manager.md)   
 [Advanced Merge Replication Conflict Detection and Resolution (Erweiterte Konflikterkennung und -lösung bei der Mergereplikation)](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)  
  
  
