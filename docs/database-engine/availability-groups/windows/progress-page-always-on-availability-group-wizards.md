---
title: Seite „Status“ (Always On-Verfügbarkeitsgruppen-Assistenten)
description: Hier finden Sie eine Beschreibung der Optionen auf der Seite „Fortschritt“ des Assistenten für Always On-Verfügbarkeitsgruppen in SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.failoverwizard.progress.f1
- sql13.swb.adddatabasewizard.progress.f1
- sql13.swb.addreplicawizard.progress.f1
- sql13.swb.newagwizard.progress.f1
ms.assetid: bd3b0306-8384-4120-a1c9-03825f0ae26a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8656399c063af3b8f0a3a91e8ad85f6af0ef8ac5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351048"
---
# <a name="progress-page-always-on-availability-group-wizards"></a>Seite „Status“ (Always On-Verfügbarkeitsgruppen-Assistenten)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Mithilfe dieses Dialogfelds können Sie den Status eines [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Assistenten anzuzeigen, den Sie in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]ausführen. Die Statusanzeige gibt den relativen Status der Schritte an, die der Assistent ausführt.  
  
## <a name="ui-element-list"></a>Liste der Benutzeroberflächenelemente  
 **Weitere Informationen**  
 Klicken Sie auf den Pfeil nach unten, um ein Statusraster anzuzeigen, in der alle ausgeführten Schritte in der entsprechenden Reihenfolge aufgeführt sind, gefolgt vom derzeit laufenden Vorgang. Das Raster enthält die folgenden Spalten:  
  
 **Name**  
 Zeigt einen Ausdruck an, der einen bestimmten Schritt beschreibt.  
  
 **Status**  
 Gibt das Ergebnis ausgeführter Schritte und den Prozentsatz des Abschlusses des aktuellen Schritts wie folgt an:  
  
|Ergebnis|BESCHREIBUNG|  
|------------|-----------------|  
|**Fehler**|Gibt an, dass beim Vorgang für diesen Schritt ein Fehler aufgetreten ist. Klicken Sie auf den Link, um ein Meldungsdialogfeld anzuzeigen, das den Fehler beschreibt.|  
|**In Bearbeitung (** *abgeschlossener Prozentsatz* **)**|Gibt an, dass der Vorgang jetzt auftritt, und schätzt den Prozentsatz dieses Schritts, der abgeschlossen wurde.|  
|**Erfolgreich**|Gibt an, dass der Vorgang für diesen Schritt erfolgreich abgeschlossen wurde.|  
  
 **Weniger Details**  
 Klicken Sie, um das Statusraster auszublenden.  
  
 **Abbrechen**  
 Klicken Sie, um verbleibende Vorgänge zu überspringen, und beenden Sie dann den Assistenten.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Verwenden des Dialogfelds Neue Verfügbarkeitsgruppe &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Verwenden des Assistenten zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Verwenden des Assistenten zum Hinzufügen von Datenbanken zu Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
-   [Verwenden des Assistenten für Failover-Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht zu AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
