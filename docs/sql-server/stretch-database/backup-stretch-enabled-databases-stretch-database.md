---
description: Sichern von Stretch-fähigen Datenbanken (Stretch Database)
title: Sichern von Stretch-aktivierten Datenbanken
ms.date: 06/14/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, disabling
- disabling Stretch Database
ms.assetid: 18f0dff0-d8ce-4bee-a935-76ed6dfb3208
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: b407e37ec4a6c3337bd5c5bc98018c73792396dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081091"
---
# <a name="backup-stretch-enabled-databases-stretch-database"></a>Sichern von Stretch-fähigen Datenbanken (Stretch Database)
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


 Datenbanksicherungen erleichtern de Wiederherstellung nach verschiedensten Fehlern und Notfällen.  
  
 -   Daher sollten Sie die SQL Server-Datenbanken mit aktivierter Stretch-Datenbank-Funktion sichern.  
      
 -   Microsoft Azure sichert automatisch die Remotedaten, die von Stretch Database von SQL Server zu Azure migriert wurden.  

> [!TIP]
> Die Datensicherung ist nur ein Teil einer vollständigen Lösung für Hochverfügbarkeit und Geschäftskontinuität. Weitere Informationen zu Hochverfügbarkeit finden Sie unter [Lösungen für Hochverfügbarkeit](../../database-engine/sql-server-business-continuity-dr.md).
   
## <a name="back-up-your-sql-server-data"></a>Sichern Ihrer SQL Server-Daten  
  
Zum Sichern Ihrer SQL Server-Datenbanken, für die Stretch-Datenbank aktiviert wurde, können Sie weiterhin die SQL Server-Sicherungsmethoden verwenden, die Sie derzeit verwenden. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von SQL Server-Datenbanken](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md).
  
 Sicherungen von SQL Server-Datenbanken mit aktivierter Stretch-Datenbank-Funktion enthalten nur lokale Daten und nur Daten, die zu dem Zeitpunkt der Sicherung migriert werden dürfen. (Zur Migration berechtigte Daten sind Daten, die noch nicht migriert wurden. Sie werden jedoch basierend auf den Migrationseinstellungen der Tabellen zu Azure migriert.) Dies wird als **flaches** Sichern bezeichnet und berücksichtigt nicht Ihre bereits zu Azure migrierten Daten.  
  
## <a name="back-up-your-remote-azure-data"></a>Sichern Ihrer Azure-Remotedaten   
  
Microsoft Azure sichert automatisch die Remotedaten, die von Stretch Database von SQL Server zu Azure migriert wurden.    
### <a name="azure-reduces-the-risk-of-data-loss-with-automatic-backup"></a>Azure reduziert das Risiko eines Datenverlusts durch das automatische Sichern  
Der Azure-Dienst SQL Server Stretch Database schützt Ihre Remotedatenbanken mindestens alle acht Stunden durch automatische Speichermomentaufnahmen. Jede Momentaufnahme wird sieben Tage lang beibehalten, um den größtmöglichen Bereich von möglichen Wiederherstellungspunkten für Sie bereitzustellen.  
  
### <a name="azure-reduces-the-risk-of-data-loss-with-geo-redundancy"></a>Azure reduziert das Risiko eines Datenverlusts durch Georedundanz  
Datenbanksicherungen in Azure werden im georedundanten Azure-Speicher (Read-Access Geo Redundant-Speicher; RA-GRS) gespeichert und sind daher standardmäßig georedundant. Georedundanter Speicher repliziert Ihre Daten in eine sekundäre Region, die Hunderte von Meilen von der primären Region entfernt ist. In primären und sekundären Regionen werden Ihre Daten jeweils dreimal in separaten Fehler- und Upgradedomänen repliziert. Dadurch wird sichergestellt, dass Ihre Daten erhalten bleiben, sogar im Falle eines vollständigen, regionalen Stromausfalls oder eines Notfalls, der eine ganze Region unzugänglich macht.

### <a name="stretch-database-reduces-the-risk-of-data-loss-for-your-azure-data-by-retaining-migrated-rows-temporarily"></a><a name="stretchRPO"></a>Stretch Database reduziert das Risiko eines Verlusts Ihrer Azure-Daten, indem migrierte Zeilen vorübergehend beibehalten werden.
Nachdem Stretch Database relevante Zeilen aus SQL Server zu Azure migriert hat, behält sie diese Zeilen mindestens acht Stunden lang in der Stagingtabelle bei. Wenn Sie eine Sicherungskopie Ihrer Azure-Datenbank wiederherstellen, verwendet Stretch Database die in der Stagingtabelle gespeicherten Zeilen, um die SQL Server- und die Azure-Datenbanken abzustimmen.

Nach der Wiederherstellung einer Ihrer Azure-Datenbanksicherungen müssen Sie die gespeicherte Prozedur [sys.sp_rda_reauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) ausführen, um eine SQL Server-Datenbank, für die Stretch-Datenbank aktiviert wurde, wieder mit der Azure-Remotedatenbank zu verbinden. Wenn Sie **sys.sp_rda_reauthorize_db** ausführen, stimmt Stretch Database automatisch die SQL Server- und die Azure-Datenbanken ab.

Um die Anzahl von Stunden zu erhöhen, für die migrierte Daten von Stretch Database vorübergehend in der Stagingtabelle beibehalten werden, führen Sie die gespeicherte Prozedur [sys.sp_rda_set_rpo_duration](../../relational-databases/system-stored-procedures/sys-sp-rda-set-rpo-duration-transact-sql.md) aus, und geben Sie eine Stundenanzahl von über acht an. Bedenken Sie bei der Entscheidung, wie viele Daten beibehalten werden sollen, die folgenden Faktoren:
-   Die Häufigkeit der automatischen Azure-Sicherungen (mindestens alle 8 Stunden)
-   Die erforderliche Zeit, um ein Problem zu erkennen und zu entscheiden, ob eine Sicherung wiederhergestellt werden soll
-   Die Dauer des Azure-Wiederherstellungsvorgangs

> [!NOTE]
> Wird die Datenmenge erhöht, die Stretch Database vorübergehend in der Stagingtabelle beibehält, erhöht sich dadurch auch der erforderliche Speicherplatz auf dem SQL Server-Computer.

Um die Anzahl von Stunden zu überprüfen, für die Daten von Stretch Database vorübergehend in der Stagingtabelle beibehalten werden, führen Sie die gespeicherte Prozedur [sys.sp_rda_get_rpo_duration](../../relational-databases/system-stored-procedures/sys-sp-rda-get-rpo-duration-transact-sql.md) aus.

## <a name="see-also"></a>Weitere Informationen  
[Wiederherstellen von Stretch-fähigen Datenbanken](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
 [Verwalten von Stretch Database und Behandeln von Problemen](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)   
   
  
  
