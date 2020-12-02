---
title: Sicherungszeitachse | Microsoft-Dokumentation
description: In SQL Server können Sie mit dem Dialogfeld „Sicherungszeitachse“ Sicherungen zum Wiederherstellen einer Datenbank auf dem Stand zu einem bestimmten Zeitpunkt suchen und angeben.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.SWB.POINTINTIMERESTORE.F1
- sql13.swb.backuptimeline.f1
helpviewer_keywords:
- Backup Timeline
ms.assetid: ae3565f2-ddb2-4469-a992-7531d4f9ebb8
author: cawrites
ms.author: chadam
ms.openlocfilehash: ea91958c9475e90e0ee71f700112b5cb42663ae4
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130507"
---
# <a name="backup-timeline"></a>Sicherungszeitachse
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Verwenden Sie das Dialogfeld **Sicherungszeitachse** , um Sicherungen zum Wiederherstellen einer Datenbank entsprechend einem bestimmten Zeitpunkt zu suchen und anzugeben. Auf das Dialogfeld **Sicherungszeitachse** greifen Sie zu, indem Sie im Bereich **Datenbank wiederherstellen (Seite „Allgemein“)** auf **Zeitachse** klicken. In diesem Dialogfeld können Sie eine Zeitachse der für die Datenbank ausgeführten Wiederherstellungsvorgänge anzeigen.  
  
 Der Datenbankwiederherstellungsberater stellt sicher, dass nur Sicherungen ausgewählt werden, die für die Wiederherstellung auf diesen Zeitpunkt benötigt werden. Die ausgewählten Sicherungen machen den empfohlenen Wiederherstellungsplan für die Wiederherstellung aus. Sie sollten nur die ausgewählten Sicherungen verwenden. Informationen zum Datenbankwiederherstellungsberater finden Sie unter [Übersicht über Wiederherstellungsvorgänge &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).  
  
## <a name="restore-to"></a>Wiederherstellen in  
 Standardmäßig ist **Letzte Sicherung** ausgewählt. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] wählt die entsprechenden Sicherungen zum Wiederherstellen der Datenbank aus und stellt die Datenbank entsprechend dem Zeitpunkt der letzten Sicherung wieder her. Klicken Sie auf **Bestimmtes Datum und bestimmte Uhrzeit** , um das Datum und die Uhrzeit manuell festzulegen (und so einen bestimmten Zeitpunkt auszuwählen).  
  
 Mit **Bestimmtes Datum und bestimmte Uhrzeit** können Sie die Wiederherstellung zu einem bestimmten ausgewählten Datum und einer bestimmten Uhrzeit beenden. Die Zeitachse zeigt die Sicherungsvorgänge in den 24 Stunden um das ausgewählte Datum und die Uhrzeit an.  
  
 **Date**  
 Wählen Sie in der Dropdownliste ein Datum aus, oder geben Sie ein Datum ein.  
  
 **Time**  
 Geben Sie ein Datum ein, oder wählen Sein ein Datum aus, um den bestimmten Zeitpunkt anzugeben, bis zu dem die Wiederherstellung durchgeführt werden soll.  
  
 **Zeitachsenintervall**  
 Zeigt die Optionen für die Intervalltypen an, die auf der Zeitachse angezeigt werden können.  
  
## <a name="timeline-and-legend"></a>Zeitachse und Legende  
 Verwenden Sie die Bildlaufleiste unter der Zeitachse, um den Cursor auf der Zeitachse vorwärts und rückwärts zu bewegen. Klicken Sie auf eine Sicherung, um die Bildlaufleiste an das Ende dieser Sicherung zu verschieben. Zeigen Sie mit der Maus auf einen Marker, um eine QuickInfo mit Informationen zum ausgewählten Sicherungssatz anzuzeigen. Die Sicherungsinformationen werden mit den folgenden Markern auf der Zeitachse angezeigt:  
  
 Größeres Dreieck  
 Stellt die für die Datenbank ausgeführten vollständigen Sicherungen dar und bezeichnet den jeweiligen Ausführungszeitpunkt der einzelnen vollständigen Sicherungen.  
  
 Kleineres Dreieck  
 Stellt die für die Datenbank ausgeführten differenziellen Sicherungen dar und bezeichnet den jeweiligen Ausführungszeitpunkt der einzelnen differenziellen Sicherungen.  
  
 Grün schattierte Bereiche  
 Stellen den Sicherungsumfang des Transaktionsprotokolls dar.  
  
 Rote Linie  
 Kann nur an Stellen auf der Zeitachse positioniert werden, für die eine Wiederherstellung möglich ist. Wenn Sie die rote Linie entlang der Zeitachse verschieben, werden die oben angezeigten Felder **Datum** und **Uhrzeit** angepasst.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbank wiederherstellen &#40;Seite „Allgemein“&#41;](../../relational-databases/backup-restore/restore-database-general-page.md)  
  
  
