---
title: Dateimomentaufnahme-Sicherungen für Datenbankdateien in Azure | Microsoft-Dokumentation
description: SQL Server-Dateimomentaufnahme-Sicherungen verwenden Azure-Momentaufnahmen, um schnelle Sicherungen und schnellere Wiederherstellungen für mithilfe von Azure Blob Storage gespeicherte Datenbankdateien bereitzustellen.
ms.custom: ''
ms.date: 05/23/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 17a81fcd-8dbd-458d-a9c7-2b5209062f45
author: cawrites
ms.author: chadam
ms.openlocfilehash: 64eced0d45854db8d970c9cfbd15132ef00f070a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345573"
---
# <a name="file-snapshot-backups-for-database-files-in-azure"></a>Dateimomentaufnahme-Sicherungen für Datenbankdateien in Azure
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Dateimomentaufnahme-Sicherungen verwenden Azure-Momentaufnahmen, um nahezu sofortige Sicherungen und schnellere Wiederherstellungen für Datenbankdateien mithilfe von Azure Blob Storage zu nutzen. Sicherungs- und Wiederherstellungsrichtlinien lassen sich dank dieser Funktion vereinfachen. Eine Livedemo finden Sie unter [Demo of Point in Time Restore](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo)(in englischer Sprache). Weitere Informationen zum Speichern von Datenbankdateien mithilfe von Azure Blob Storage finden Sie unter [SQL Server-Datendateien in Microsoft Azure](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md).  
  
 ![Architekturdiagramm für die Momentaufnahmesicherung](../../relational-databases/backup-restore/media/snapshotbackups.PNG "Architekturdiagramm für die Momentaufnahmesicherung")  
  
 **Download**  
  
-   Navigieren Sie zum Herunterladen von [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]zum  **[Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)** .  
  
-   Sie haben ein Azure-Konto?  Wechseln Sie anschließend **[hierhin](https://azure.microsoft.com/services/virtual-machines/sql-server/)** , um einen virtuellen Computer zu starten, auf dem [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] bereits installiert ist.  
  
## <a name="using-azure-snapshots-to-back-up-database-files-stored-in-azure"></a>Verwenden von Azure-Momentaufnahmen zum Sichern von in Azure gespeicherten Datenbankdateien  
  
### <a name="what-is-a-ssnoversion-file-snapshot-backup"></a>Was ist eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dateimomentaufnahme-Sicherung?  
 Eine Dateimomentaufnahme-Sicherung besteht aus einem Satz von Azure-Momentaufnahmen der BLOBs, die die Datenbankdateien enthalten, und einer Sicherungsdatei mit Zeigern auf diese Dateimomentaufnahmen. Jede Dateimomentaufnahme wird im Container mit dem Basis-BLOB gespeichert. Sie können angeben, dass die Sicherungsdatei selbst über URL, auf Datenträger oder auf Band geschrieben wird. Sicherung über URL wird empfohlen. Weitere Informationen zur Sicherung finden Sie unter [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md) und zur Sicherung über URLs unter [SQL Server-Sicherung über URLs](../../relational-databases/backup-restore/sql-server-backup-to-url.md).  
  
 ![Architektur des Momentaufnahmefeatures](../../relational-databases/backup-restore/media/snapshotbackups-flat.png "Architektur des Momentaufnahmefeatures")  
  
 Durch das Löschen des Basis-BLOBs wird der Sicherungssatz ungültig, und Sie werden daran gehindert, ein BLOB zu löschen, das Dateimomentaufnahmen enthält (es sei denn, Sie entscheiden ausdrücklich, ein BLOB mit allen zugehörigen Dateimomentaufnahmen zu löschen). Durch Löschen einer Datenbank oder einer Datei wird zudem weder das Basis-BLOB noch eine seiner Dateimomentaufnahmen gelöscht. Darüber hinaus werden durch das Löschen der Sicherungsdatei keine Dateimomentaufnahmen aus dem Sicherungssatz gelöscht. Verwenden Sie zum Löschen eines Dateimomentaufnahme-Sicherungssatzes die gespeicherte Systemprozedur **sys.sp_delete_backup** .  
  
 **Vollständige Datenbanksicherung:** Durch Ausführen einer vollständigen Datenbanksicherung mithilfe einer Dateimomentaufnahme-Sicherung wird eine Azure-Momentaufnahme aller Daten- und Protokolldateien der Datenbank erstellt, die Transaktionsprotokoll-Sicherungskette erzeugt und der Speicherort der Dateimomentaufnahmen in die Sicherungsdatei geschrieben.  
  
 **Transaktionsprotokollsicherung:** Durch Ausführen einer Transaktionsprotokollsicherung mithilfe einer Dateimomentaufnahme-Sicherung wird eine Dateimomentaufnahme der einzelnen Datenbankdateien (nicht nur des Transaktionsprotokolls) erstellt, es werden die Informationen zum Speicherort der Dateimomentaufnahme in der Sicherungsdatei aufgezeichnet und die Transaktionsprotokolldatei wird abgeschnitten.  
  
> [!IMPORTANT]  
>  Nach der ersten vollständigen Sicherung, die zum Einrichten der Transaktionsprotokoll-Sicherungskette (die eine Dateimomentaufnahme-Sicherung sein kann) erforderlich ist, brauchen nur noch Sicherungen des Transaktionsprotokolls ausgeführt werden, da jeder Dateimomentaufnahme-Sicherungssatz des Transaktionsprotokolls die Dateimomentaufnahmen aller Datenbankdateien enthält und verwendet werden kann, um eine Datenbank- oder Protokollwiederherstellung durchzuführen. Nach der ersten vollständigen Datenbanksicherung brauchen Sie keine zusätzlichen vollständigen oder differenziellen Sicherungen mehr auszuführen, da der Azure-BLOB-Speicherdienst die Unterschiede zwischen jeder Dateimomentaufnahme und dem aktuellen Zustand des Basis-BLOBs für jede Datenbankdatei behandelt.  
  
> [!NOTE]  
>  Ein Tutorial zur Verwendung von SQL Server 2016 mit dem Microsoft Azure-BLOB-Speicherdienst finden Sie unter [Tutorial: Verwenden des Microsoft Azure BLOB-Speicherdiensts mit SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)  
  
### <a name="restore-using-file-snapshot-backups"></a>Wiederherstellung mithilfe von Dateimomentaufnahme-Sicherungen  
 Da jeder Dateimomentaufnahme-Sicherungssatz eine Dateimomentaufnahme jeder einzelnen Datenbankdatei enthält, erfordert ein Wiederherstellungsvorgang höchstens zwei aufeinander folgende Dateimomentaufnahme-Sicherungssätze. Dies gilt unabhängig davon, ob der Sicherungssatz aus einer vollständigen Sicherung oder einer Protokollsicherung stammt. Dies unterscheidet sich wesentlich vom Wiederherstellungsvorgang unter Verwendung herkömmlicher Streamingsicherungsdateien. Bei der herkömmlichen Streamingsicherung ist für den Wiederherstellungsvorgang eine vollständige Kette von Sicherungssätzen erforderlich: die vollständige Sicherung, eine differenzielle Sicherung und eine oder mehrere Transaktionsprotokollsicherungen. Der Wiederherstellungsteil Wiederherstellungsvorgangs ist identisch, unabhängig davon, ob von der Wiederherstellung ein Dateimomentaufnahme-Sicherungs- oder ein Streamingsicherungssatz verwendet wird.  
  
 **Auf den Zeitpunkt eines beliebigen Sicherungssatzes:** Um einen RESTORE DATABASE-Vorgang zum Wiederherstellen einer Datenbank auf den Zeitpunkt eines bestimmten Dateimomentaufnahme-Sicherungssatzes auszuführen, werden nur der entsprechende Sicherungssatz und die Basis-Blobs selbst benötigt. Da Sie einen Dateimomentaufnahme-Sicherungssatz des Transaktionsprotokolls verwenden können, um einen RESTORE DATABASE-Vorgang auszuführen, verwenden Sie zum Ausführen dieser Art von RESTORE DATABASE-Vorgang in der Regel einen Sicherungssatz des Transaktionsprotokolls und selten einen vollständigen Sicherungssatz der Datenbank. Ein Beispiel am Ende dieses Themas veranschaulicht dieses Verfahren.  
  
 **Auf einen bestimmten Zeitpunkt zwischen zwei Dateimomentaufnahme-Sicherungssätzen:** Um einen RESTORE DATABASE-Vorgang auszuführen, der eine Datenbank auf einen bestimmten Zeitpunkt zwischen zwei aufeinander folgenden Transaktionsprotokoll-Sicherungssätzen wiederherstellt, werden nur zwei Transaktionsprotokoll-Sicherungssätze benötigt: ein Sicherungssatz vor und ein Sicherungssatz nach dem Zeitpunkt, auf den Sie die Datenbank wiederherstellen möchten. Um dies zu erreichen, führen Sie einen RESTORE DATABASE-Vorgang WITH NORECOVERY mithilfe des Dateimomentaufnahme-Sicherungssatzes des Transaktionsprotokolls des früheren Zeitpunkts sowie einen RESTORE LOG-Vorgang WITH RECOVERY mithilfe des Dateimomentaufnahme-Sicherungssatzes des Transaktionsprotokolls des späteren Zeitpunkt aus und verwenden das STOPAT-Argument, um den Zeitpunkt anzugeben, an dem die Wiederherstellung aus der Transaktionsprotokollsicherung angehalten werden soll. Ein Beispiel am Ende dieses Themas veranschaulicht dieses Verfahren. Eine Livedemo finden Sie unter [Demo of Point in Time Restore](https://channel9.msdn.com/Blogs/Windows-Azure/File-Snapshot-Backups-Demo)(in englischer Sprache).  
  
### <a name="file-backup-set-maintenance"></a>Wartung eines Dateimomentaufnahme-Sicherungssatzes  
 **Löschen eines Dateimomentaufnahme-Sicherungssatzes:** Ein Dateimomentaufnahme-Sicherungssatz kann nicht mit dem FORMAT-Argument überschrieben werden. Das Argument FORMAT ist nicht zulässig, um zu vermeiden, dass Dateimomentaufnahmen, die mit der ursprünglichen Dateimomentaufnahme-Sicherung erstellt wurden, verwaist zurückbleiben. Verwenden Sie zum Löschen eines Dateimomentaufnahme-Sicherungssatzes die gespeicherte Systemprozedur **sys.sp_delete_backup** . Diese gespeicherte Prozedur löscht die Sicherungsdatei und die Dateimomentaufnahmen, die den Sicherungssatz bilden. Bei Verwendung einer anderen Methode zum Löschen eines Dateimomentaufnahme-Sicherungssatzes kann die Sicherungsdatei gelöscht werden, ohne dass die Dateimomentaufnahmen im Sicherungssatz gelöscht werden.  
  
 **Löschen verwaister Sicherungsdatei-Momentaufnahmen:** Möglicherweise sind verwaiste Dateimomentaufnahmen vorhanden, weil die Sicherungsdatei nicht mit der gespeicherten Systemprozedur **sys.sp_delete_backup** gelöscht wurde. Auch die Löschung einer Datenbank oder Datenbankdatei kann zu diesem Problem führen, wenn den Blobs, zu denen die Datenbank bzw. Datenbankdatei gehörte, Sicherungsdatei-Momentaufnahmen zugeordnet waren. Verwenden Sie die Systemfunktion **sys.fn_db_backup_file_snapshots** zum Auflisten aller Dateimomentaufnahmen der Datenbankdateien, um mögliche verwaiste Dateimomentaufnahmen zu ausfindig zu machen. Um die Dateimomentaufnahmen zu suchen, die Teil eines bestimmten Dateimomentaufnahme-Sicherungssatzes sind, verwenden Sie die gespeicherte Systemprozedur RESTORE FILELISTONLY. Anschließend können Sie mit der gespeicherten Systemprozedur **sys.sp_delete_backup_file_snapshot** einzelne verwaiste Sicherungsdatei-Momentaufnahmen löschen. Beispiele für die Verwendung dieser Systemfunktion und dieser gespeicherten Systemprozeduren finden Sie am Ende dieses Themas. Weitere Informationen finden Sie unter [sp_delete_backup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md), [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md), [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md) und [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).  
  
### <a name="considerations-and-limitations"></a>Überlegungen und Einschränkungen  
 **Storage Premium:** Bei Verwendung von Storage Premium gelten die folgenden Einschränkungen:  
  
-   Die Sicherungsdatei selbst kann nicht unter Verwendung von Premium-Speicher gespeichert werden.  
  
-   Das Sicherungsintervall darf nicht unter 10 Minuten liegen.  
  
-   Es können maximal 100 Momentaufnahmen gespeichert werden.  
  
-   RESTORE WITH MOVE ist erforderlich.  
  
-   Weitere Informationen zu Storage Premium finden Sie unter [Storage Premium: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](/azure/virtual-machines/disks-types).  
  
 **Ein einziges Speicherkonto:** Die Dateimomentaufnahme und die Ziel-BLOBs müssen dasselbe Speicherkonto verwenden.  
  
 **Massenprotokolliertes Wiederherstellungsmodell:** Bei Verwendung des massenprotokollierten Wiederherstellungsmodus und bei der Arbeit mit einer Sicherung des Transaktionsprotokolls mit minimal protokollierten Transaktionen können Sie keine Protokollwiederherstellung (einschließlich Zeitpunktwiederherstellung) mithilfe der Transaktionsprotokollsicherung ausführen. Führen Sie stattdessen eine Wiederherstellung der Datenbank auf den Zeitpunkt des Dateimomentaufnahme-Sicherungssatzes aus. Diese Einschränkung entspricht der Einschränkung bei Streamingsicherungen.  
  
 **Onlinewiederherstellung:** Bei Verwendung von Dateimomentaufnahme-Sicherungen können Sie keine Onlinewiederherstellung ausführen. Weitere Informationen zur Onlinewiederherstellung finden Sie unter [Onlinewiederherstellungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md).  
  
 **Abrechnung:** Bei Verwendung von SQL Server-Dateimomentaufnahme-Sicherungen fallen zusätzliche Gebühren an, wenn sich Daten ändern. Weitere Informationen finden Sie unter [Grundlegendes zur Ermittlung der Gebühren für Momentaufnahmen](/rest/api/storageservices/Understanding-How-Snapshots-Accrue-Charges).  
  
 **Archivierung:** Sie können Dateimomentaufnahme-Sicherungen in einem Blob-Speicher oder einer Streamingsicherung archivieren. Kopieren Sie zum Archivieren in einem Blob-Speicher die Momentaufnahmen im Dateimomentaufnahme-Sicherungssatz in getrennte Blobs. Zum Archivieren in eine Streamingsicherung stellen Sie die Dateimomentaufnahme-Sicherung als neue Datenbank wieder her, und führen Sie dann eine normale Streamingsicherung mit Komprimierung und/oder Verschlüsselung durch, und archivieren Sie diese für die gewünschte Dauer, unabhängig von den Basis-BLOBs.  
  
> [!IMPORTANT]  
>  Beim Verwalten mehrerer Dateimomentaufnahme-Sicherungen fällt nur ein geringer Leistungsmehraufwand an. Allerdings kann eine übermäßige Anzahl von Dateimomentaufnahme-Sicherungen die E/A-Leistung für die Datenbank beeinträchtigen. Wir empfehlen, nur die Dateimomentaufnahme-Sicherungen aufbewahren, die zur Unterstützung Ihres Ziels hinsichtlich der Wiederherstellungspunkte erforderlich sind.  
  
## <a name="backing-up-the-database-and-log-using-a-file-snapshot-backup"></a>Sichern von Datenbank und Protokoll unter Verwendung einer Dateimomentaufnahme-Sicherung  
 Im folgenden Beispiel wird die AdventureWorks2016-Beispieldatenbank mithilfe einer Dateimomentaufnahme-Sicherung über URL gesichert.  
  
```  
-- To permit log backups, before the full database backup, modify the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE AdventureWorks2016  
   SET RECOVERY FULL;  
GO  
-- Back up the full AdventureWorks2016 database.  
BACKUP DATABASE AdventureWorks2016   
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak'   
WITH FILE_SNAPSHOT;  
GO  
-- Back up the AdventureWorks2016 log using a time stamp in the backup file name.  
DECLARE @Log_Filename AS VARCHAR (300);  
SET @Log_Filename = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_Log_'+   
REPLACE (REPLACE (REPLACE (CONVERT (VARCHAR (40), GETDATE (), 120), '-','_'),':', '_'),' ', '_') + '.trn';  
BACKUP LOG AdventureWorks2016  
 TO URL = @Log_Filename WITH FILE_SNAPSHOT;  
GO  
```  
  
## <a name="restoring-from-a-ssnoversion-file-snapshot-backup"></a>Wiederherstellen aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dateimomentaufnahme-Sicherung  
 Im folgende Beispiel wird die AdventureWorks2016-Datenbank unter Verwendung eines Dateimomentaufnahme -Sicherungssatzes des Transaktionsprotokolls wiederhergestellt und ein Wiederherstellungsvorgang veranschaulicht. Beachten Sie, dass Sie eine Datenbank aus einem einzigen Dateimomentaufnahme -Sicherungssatzes des Transaktionsprotokolls wiederherstellen können.  
  
```  
RESTORE DATABASE AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_16_00_00.trn'   
WITH RECOVERY, REPLACE;  
GO  
```  
  
## <a name="restoring-from-a-ssnoversion-file-snapshot-backup-to-a-point-in-time"></a>Wiederherstellen aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dateimomentaufnahme-Sicherung auf einen bestimmten Zeitpunkt  
 Im folgende Beispiel wird die AdventureWorks2016-Datenbank unter Verwendung von zwei Dateimomentaufnahme -Sicherungssätzen des Transaktionsprotokolls in den Zustand zu einem bestimmten Zeitpunkt wiederhergestellt und ein Wiederherstellungsvorgang veranschaulicht.  
  
```  
RESTORE DATABASE AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_16_00_00.trn'   
WITH NORECOVERY,REPLACE;  
GO   
  
RESTORE LOG AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_18_00_00.trn'   
WITH RECOVERY,STOPAT = 'May 18, 2015 5:35 PM';  
GO  
```  
  
## <a name="deleting-a-database-file-snapshot-backup-set"></a>Löschen eines Dateimomentaufnahme-Sicherungssatzes der Datenbank  
 Verwenden Sie zum Löschen eines Dateimomentaufnahme-Sicherungssatzes die gespeicherte Systemprozedur **sys.sp_delete_backup** . Geben Sie den Datenbanknamen an, damit das System überprüft, ob der angegebene Dateimomentaufnahme Sicherungssatz tatsächlich eine Sicherung für die angegebene Datenbank ist. Wenn kein Datenbankname angegeben ist, wird der angegebene Sicherungssatz mit seinen Dateimomentaufnahmen ohne eine solche Überprüfung gelöscht. Weitere Informationen finden Sie unter [sp_delete_backup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md).  
  
> [!WARNING]  
>  Beim Versuch, einen Dateimomentaufnahmen-Sicherungssatz mit einer anderen Methode zu löschen, z. B. dem Microsoft Azure-Verwaltungsportal oder dem Azure-Speicher-Viewer in SQL Server Management Studio, werden die Dateimomentaufnahmen im Sicherungssatz nicht gelöscht. Durch diese Tools wird nur die Sicherungsdatei gelöscht, die die Zeiger auf die Dateimomentaufnahmen im Dateimomentaufnahme-Sicherungssatz enthält. Verwenden Sie zum Suchen der Sicherungsdatei-Momentaufnahmen, die zurückbleiben, wenn eine Sicherungsdatei nicht ordnungsgemäß gelöscht wurde, die Systemfunktion **sys.fn_db_backup_file_snapshots** und anschließend die gespeicherte Systemprozedur **sys.sp_delete_backup_file_snapshot** , um einzelne Sicherungsdatei-Momentaufnahmen zu löschen.  
  
 Das folgende Beispiel löscht den angegebenen Dateimomentaufnahme-Sicherungssatz, einschließlich der Sicherungsdatei und der Dateimomentaufnahmen, die den angegebenen Sicherungssatz bilden.  
  
```  
sys.sp_delete_backup 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak', 'adventureworks2016' ;  
GO  
  
```  
  
## <a name="viewing-database-backup-file-snapshots"></a>Anzeigen der Datenbanksicherungs-Dateimomentaufnahmen  
 Verwenden Sie zum Anzeigen der Dateimomentaufnahmen des Basis-Blobs für jede Datenbankdatei die Systemfunktion **sys.fn_db_backup_file_snapshots** . Mit dieser Systemfunktion können Sie alle Sicherungsdatei-Momentaufnahmen der einzelnen Basis-BLOBs für eine Datenbank anzeigen, die mit dem Azure-BLOB-Speicherdienst gespeichert wurden. Ein primärer Anwendungsfall für diese Funktion ist Suche nach Sicherungsdatei-Momentaufnahmen einer Datenbank, die zurückbleiben, wenn die Sicherungsdatei für einen Dateimomentaufnahme-Sicherungssatz gelöscht wurde und für die Löschung nicht die gespeicherten Systemprozedur **sys.sp_delete_backup** verwendet wurde. Verwenden Sie die gespeicherte Systemprozedur **RESTORE FILELISTONLY**  , um zu ermitteln, welche Sicherungsdatei-Momentaufnahmen Teil von intakten und welche Teil von nicht intakten Sicherungssätzen sind. Die Systemprozedur listet die Datei-Momentaufnahmen für jede Sicherungsdatei auf. Weitere Informationen finden Sie unter [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md) und [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).  
  
 Das folgende Beispiel gibt die Liste aller Sicherungsdatei-Momentaufnahmen für die angegebene Datenbank zurück.  
  
```  
--Either specify the database name or set the database context  
USE AdventureWorks2016  
select * from sys.fn_db_backup_file_snapshots (null) ;  
GO  
select * from sys.fn_db_backup_file_snapshots ('AdventureWorks2016') ;  
GO  
  
```  
  
## <a name="deleting-an-individual-database-backup-file-snapshot"></a>Löschen einer einzelnen Sicherungsdatei-Momentaufnahme einer Datenbank  
 Mit der gespeicherten Systemprozedur **sys.sp_delete_backup_file_snapshot** können Sie eine einzelne Sicherungsdatei-Momentaufnahme des Basis-Blobs einer Datenbank löschen. Eine primäre Anwendungsfall für diese gespeicherte Systemprozedur ist die Löschung verwaister Dateimomentaufnahme-Dateien, die nach dem Löschen einer Sicherungsdatei zurückbleiben, wenn für die Löschung nicht die gespeicherte Systemprozedur **sys.sp_delete_backup** verwendet wurde. Weitere Informationen finden Sie unter [sp_delete_backup_file_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md).  
  
> [!WARNING]  
>  Wird eine einzelne Dateimomentaufnahme gelöscht, die Bestandteil eines Dateimomentaufnahme-Sicherungssatzes ist, wird der Sicherungssatz ungültig.  
  
 Das folgende Beispiel löscht die angegebene Sicherungsdatei-Momentaufnahme. Die URL für die angegebene Sicherung wurde mithilfe der Systemfunktion **sys.fn_db_backup_file_snapshots** abgerufen.  
  
```  
sys.sp_delete_backup_file_snapshot N'adventureworks2016', N'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016Data.mdf?snapshot=2015-05-29T21:31:31.6502195Z';  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Tutorial: Verwenden des Microsoft Azure BLOB-Speicherdiensts mit SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)  
  
