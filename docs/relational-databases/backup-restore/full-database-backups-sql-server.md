---
title: Vollständige Datenbanksicherungen (SQL Server) | Microsoft-Dokumentation
description: In SQL Server wird mit einer vollständigen Datenbanksicherung die gesamte Datenbank gesichert. Vollständige Datenbanksicherungen stellen die Datenbank bei Abschluss der Sicherung dar.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full backups [SQL Server]
- backups [SQL Server], database
- backing up databases [SQL Server], full backups
- estimating database backup size
- backing up [SQL Server], size of backup
- database backups [SQL Server], full backups
- size [SQL Server], backups
- database backups [SQL Server], about backing up databases
ms.assetid: 4d933d19-8d21-4aa1-8153-d230cb3a3f99
author: cawrites
ms.author: chadam
ms.openlocfilehash: 96a78b5613f73f2030891985cd22c117feb99d53
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2020
ms.locfileid: "96126953"
---
# <a name="full-database-backups-sql-server"></a>Vollständige Datenbanksicherungen (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Mit einer vollständigen Datenbanksicherung wird die gesamte Datenbank gesichert. Dazu gehört ein Teil des Transaktionsprotokolls, damit nach der Wiederherstellung einer vollständigen Datenbanksicherung die vollständige Datenbank wiederhergestellt werden kann. Vollständige Datenbanksicherungen stellen die Datenbank bei Abschluss der Sicherung dar.  
  
> [!TIP]  
>  Wenn eine Datenbank größer wird, ist zum Abschließen von vollständigen Datenbanksicherungen jedoch mehr Zeit und mehr Speicherplatz erforderlich. Deshalb können Sie bei einer großen Datenbank eine vollständige Datenbanksicherung durch mehrere *differenzielle Datenbanksicherungen* ergänzen. Weitere Informationen finden Sie unter [Differenzielle Sicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md).  
  
> [!IMPORTANT]  
>  Bei einer Datenbanksicherung ist TRUSTWORTHY auf OFF festgelegt. Weitere Informationen zum Festlegen von TRUSTWORTHY auf ON finden Sie unter [ALTER DATABASE SET-Optionen &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 **In diesem Thema:**  
  
-   [Datenbanksicherungen im einfachen Wiederherstellungsmodell](#DbBuRMs)  
  
-   [Datenbanksicherungen im vollständigen Wiederherstellungsmodell](#DbBuRMf)  
  
-   [Verwenden einer vollständigen Datenbanksicherung zum Wiederherstellen der Datenbank](#RestoreDbBu)  
  
-   [Verwandte Aufgaben](#RelatedTasks)  
  
##  <a name="database-backups-under-the-simple-recovery-model"></a><a name="DbBuRMs"></a> Datenbanksicherungen im einfachen Wiederherstellungsmodell  
 Im einfachen Wiederherstellungsmodell besteht nach jeder Sicherung die Gefahr, dass in der Datenbank Datenverluste auftreten, wenn ein Notfall auftritt. Die Gefahr des Datenverlusts steigt mit jedem Update bis zur nächsten Sicherung, wenn die Gefahr auf null zurückgeht und ein neuer Zyklus der Datenverlustgefahr beginnt. Die Gefahr des Datenverlusts steigt in der Zeit zwischen den Sicherungen. In der folgenden Abbildung wird die Gefahr des Datenverlusts für eine Sicherungsstrategie veranschaulicht, in der nur vollständige Datenbanksicherungen verwendet werden.  
  
 ![Datenverlust zwischen Datenbanksicherungen](../../relational-databases/backup-restore/media/bnr-rmsimple-1-fulldb-backups.gif "Datenverlust zwischen Datenbanksicherungen")  
  
### <a name="example--tsql"></a>Beispiel ([!INCLUDE[tsql](../../includes/tsql-md.md)])  
 Im folgenden Beispiel wird veranschaulicht, wie eine vollständige Datenbanksicherung erstellt wird, indem WITH FORMAT zum Überschreiben aller vorhandenen Sicherungen und zum Erstellen eines neuen Mediensatzes verwendet wird.  
  
```  
-- Back up the AdventureWorks2012 database to new media set.  
BACKUP DATABASE AdventureWorks2012  
    TO DISK = 'Z:\SQLServerBackups\AdventureWorksSimpleRM.bak'   
    WITH FORMAT;  
GO  
```  
  
##  <a name="database-backups-under-the-full-recovery-model"></a><a name="DbBuRMf"></a> Datenbanksicherungen im vollständigen Wiederherstellungsmodell  
 Bei Datenbanken, für die die vollständige und massenprotokollierte Wiederherstellung verwendet wird, sind Datensicherungen erforderlich, aber nicht ausreichend. Transaktionsprotokollsicherungen sind ebenfalls erforderlich. In der folgenden Abbildung wird die einfachste Sicherungsstrategie veranschaulicht, die im vollständigen Wiederherstellungsmodell möglich ist.  
  
 ![Reihen vollständiger Datenbanksicherungen und Protokollsicherungen](../../relational-databases/backup-restore/media/bnr-rmfull-1-fulldb-log-backups.gif "Reihen vollständiger Datenbanksicherungen und Protokollsicherungen")  
  
 Informationen zum Verwenden von Transaktionsprotokollsicherungen finden Sie unter [Transaktionsprotokollsicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md).  
  
### <a name="example--tsql"></a>Beispiel ([!INCLUDE[tsql](../../includes/tsql-md.md)])  
 Im folgenden Beispiel wird veranschaulicht, wie eine vollständige Datenbanksicherung erstellt wird, indem WITH FORMAT zum Überschreiben aller vorhandenen Sicherungen und zum Erstellen eines neuen Mediensatzes verwendet wird. Anschließend wird das Transaktionsprotokoll im Beispiel gesichert. In einer realen Situation müssten Sie mehrere reguläre Protokollsicherungen ausführen. Für dieses Beispiel muss die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Beispieldatenbank auf die Verwendung des vollständigen Wiederherstellungsmodells festgelegt sein.  
  
```  
USE master;  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
GO  
-- Back up the AdventureWorks2012 database to new media set (backup set 1).  
BACKUP DATABASE AdventureWorks2012  
  TO DISK = 'Z:\SQLServerBackups\AdventureWorks2012FullRM.bak'   
  WITH FORMAT;  
GO  
--Create a routine log backup (backup set 2).  
BACKUP LOG AdventureWorks2012 TO DISK = 'Z:\SQLServerBackups\AdventureWorks2012FullRM.bak';  
GO  
```  
  
##  <a name="use-a-full-database-backup-to-restore-the-database"></a><a name="RestoreDbBu"></a> Verwenden einer vollständigen Datenbanksicherung zum Wiederherstellen der Datenbank  
 Sie können eine gesamte Datenbank in einem Schritt erneut erstellen, indem Sie die Datenbank aus einer vollständigen Datenbanksicherung an einem beliebigen Speicherort wiederherstellen. In der Sicherung ist ein ausreichender Bestandteil des Transaktionsprotokolls enthalten, sodass Sie die Datenbank bis zu dem Zeitpunkt wiederherstellen können, zu dem die Sicherung abgeschlossen war. Die wiederhergestellte Datenbank entspricht dem Zustand der ursprünglichen Datenbank beim Abschluss der Datenbanksicherung, abzüglich aller Transaktionen, für die kein Commit ausgeführt wurde. Bei Verwendung des vollständigen Wiederherstellungsmodells sollten dann alle nachfolgenden Transaktionsprotokollsicherungen wiederhergestellt werden. Wenn die Datenbank wiederhergestellt wurde, wird für Transaktionen ohne Commit ein Rollback ausgeführt.  
  
 Weitere Informationen finden Sie unter [Vollständige Datenbankwiederherstellungen &#40;einfaches Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md) oder [Vollständige Datenbankwiederherstellungen &#40;vollständiges Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **So erstellen Sie eine vollständige Datenbanksicherung**  
  
-   [Erstellen einer vollständigen Datenbanksicherung &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Backup.SqlBackup%2A> (SMO)  
  
 **So planen Sie Sicherungsaufträge**  
  
 [Verwenden des Wartungsplanungs-Assistenten](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Sichern und Wiederherstellen von SQL Server-Datenbanken](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Übersicht über Sicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Sichern und Wiederherstellen von Analysis Services-Datenbanken](/analysis-services/multidimensional-models/backup-and-restore-of-analysis-services-databases)  
  
