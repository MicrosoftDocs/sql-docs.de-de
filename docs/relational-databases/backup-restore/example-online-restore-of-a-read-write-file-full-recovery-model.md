---
title: 'Onlinewiederherstellung: Datei mit Lese-/Schreibzugriff (vollständiges Wiederherstellungsmodell)'
description: In diesem Beispiel wird eine Datei mit Lese-/Schreibzugriff für eine Datenbank in SQL Server online wiederhergestellt, wobei das vollständige Wiederherstellungsmodell mit mehreren Dateigruppen verwendet wird.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full recovery model [SQL Server], RESTORE example
- online restores [SQL Server], full recovery model
- restore sequences [SQL Server], online
ms.assetid: 0dbeda81-1464-44ba-9011-914900096368
author: cawrites
ms.author: chadam
ms.openlocfilehash: ce68e817070765a6f84a12c518e71de221734b34
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2020
ms.locfileid: "96126993"
---
# <a name="example-online-restore-of-a-read-write-file-full-recovery-model"></a>Beispiel: Onlinewiederherstellung einer Datei mit Lese-/Schreibzugriff (vollständiges Wiederherstellungsmodell)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Dieses Thema ist nur für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbanken mit mehreren Dateien oder Dateigruppen im vollständigen Wiederherstellungsmodell relevant.  
  
 In diesem Beispiel enthält die Datenbank `adb`, für die das vollständige Wiederherstellungsmodell verwendet wird, drei Dateigruppen. Die Dateigruppe `A` weist Lese-/Schreibzugriff auf, die Dateigruppen `B` und `C` sind schreibgeschützt. Zu Beginn sind alle Dateigruppen online.  
  
 Datei `a1` in Dateigruppe `A` ist allem Anschein nach beschädigt, darum beschließt der Datenbankadministrator, die Datei wiederherzustellen; die Datenbank bleibt dabei online.  
  
> [!NOTE]  
>  Bei Verwendung des einfachen Wiederherstellungsmodells ist die Onlinewiederherstellung von Daten mit Lese- und Schreibzugriff nicht zulässig.  
  
## <a name="restore-sequences"></a>Wiederherstellen von Sequenzen  
  
> [!NOTE]  
>  Die Syntax für eine Onlinewiederherstellungssequenz ist dieselbe wie bei einer Offlinewiederherstellungssequenz.  
  
1.  Onlinewiederherstellung von Datei `a1`.  
  
    ```  
    RESTORE DATABASE adb FILE='a1' FROM backup   
    WITH NORECOVERY;  
    ```  
  
     Datei a1 befindet sich jetzt im Wiederherstellungsstatus, und Dateigruppe A ist offline.  
  
2.  Nach der Wiederherstellung der Datei erstellt der Datenbankadministrator eine neue Protokollsicherung, um sicherzustellen, dass der Status der Datenbank zu dem Zeitpunkt erfasst wird, zu dem die Datei offline geschaltet wurde.  
  
    ```  
    BACKUP LOG adb TO log_backup3;   
    ```  
  
3.  Onlinewiederherstellung von Protokollsicherungen.  
  
     Der Administrator stellt alle seit der wiederhergestellten Dateisicherung erstellten Protokollsicherungen bis zur letzten Protokollsicherung (der in Schritt 2 erstellten Sicherung *log_backup3*) wieder her. Nach dem Wiederherstellen der letzten Protokollsicherung wird die Datenbank wiederhergestellt.  
  
    ```  
    RESTORE LOG adb FROM log_backup1 WITH NORECOVERY;  
    RESTORE LOG adb FROM log_backup2 WITH NORECOVERY;  
    RESTORE LOG adb FROM log_backup3 WITH NORECOVERY;  
    RESTORE DATABASE adb WITH RECOVERY;  
    ```  
  
     Die Datei `a1` ist jetzt online.  
  
## <a name="additional-examples"></a>Zusätzliche Beispiele  
  
-   [Beispiel: Schrittweise Wiederherstellung einer Datenbank &#40;Einfaches Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [Beispiel: Schrittweise Wiederherstellung nur bestimmter Dateigruppen &#40;einfaches Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
-   [Beispiel: Onlinewiederherstellung einer schreibgeschützten Datei &#40;einfaches Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-simple-recovery-model.md)  
  
-   [Beispiel: Schrittweise Wiederherstellung einer Datenbank &#40;Vollständiges Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-full-recovery-model.md)  
  
-   [Beispiel: Schrittweise Wiederherstellung nur bestimmter Dateigruppen &#40;vollständiges Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
-   [Beispiel: Onlinewiederherstellung einer schreibgeschützten Datei &#40;vollständiges Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Onlinewiederherstellungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [Schrittweise Wiederherstellungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Übersicht über Wiederherstellungsvorgänge &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [Anwenden von Transaktionsprotokollsicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
