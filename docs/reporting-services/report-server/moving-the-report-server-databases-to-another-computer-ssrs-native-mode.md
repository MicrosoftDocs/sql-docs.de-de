---
title: Verschieben von Berichtsserver-Datenbanken auf einen anderen Computer (einheitlicher Modus) | Microsoft-Dokumentation
description: Sie können die Berichtsserver-Datenbanken, die in einer Installation von SQL Server-Datenbank-Engine verwendet werden, in eine Instanz auf einem anderen Computer verschieben.
ms.date: 12/16/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.assetid: 44a9854d-e333-44f6-bdc7-8837b9f34416
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 58d7a46df8eb6580961d54e02f3ee6b0c57ee32f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059325"
---
# <a name="moving-report-server-databases-to-another-computer-ssrs-native-mode"></a>Verschieben von Berichtsserver-Datenbanken auf einen anderen Computer (einheitlicher SSRS-Modus)

  Sie können die Berichtsserver-Datenbanken, die in einer Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-[!INCLUDE[ssDE](../../includes/ssde-md.md)] verwendet werden, in eine Instanz auf einem anderen Computer verschieben. Die Datenbanken reportserver und reportservertempdb müssen gemeinsam verschoben bzw. kopiert werden. Für eine Installation von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] sind beide Datenbanken erforderlich. Die reportservertempdb-Datenbank muss namentlich der zu verschiebenden primären reportserver-Datenbank entsprechen.  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]: einheitlicher Modus  
  
 Das Verschieben einer Datenbank hat keine Auswirkungen auf geplante Vorgänge, die aktuell für Berichtsserverelemente definiert sind.  
  
-   Zeitpläne werden beim ersten Neustart des Berichtsserverdiensts neu erstellt.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Aufträge, die verwendet werden, um einen Zeitplan auszulösen, werden auf der neuen Datenbankinstanz neu erstellt. Sie müssen die Aufträge nicht auf den neuen Computer verschieben, Sie möchten jedoch möglicherweise nicht mehr benötigte Aufträge auf dem Computer löschen.  
  
-   Abonnements, zwischengespeicherte Berichte und Momentaufnahmen bleiben in der verschobenen Datenbank erhalten. Wenn eine Momentaufnahme nach der Verschiebung der Datenbank keine aktualisierten Daten bezieht, löschen Sie die Momentaufnahmeoptionen, und klicken Sie auf **Anwenden**, um die vorgenommenen Änderungen zu speichern. Erstellen Sie den Zeitplan neu, und klicken Sie nochmals auf **Anwenden**, um die vorgenommenen Änderungen wieder zu speichern.  
  
-   Temporäre Berichts- und Benutzersitzungsdaten, die in reportservertempdb gespeichert sind, bleiben beim Verschieben dieser Datenbank persistent.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bietet verschiedene Vorgehensweisen zum Verschieben von Datenbanken (einschließlich Sichern und Wiederherstellen, Anfügen und Trennen sowie Kopieren). Nicht alle Vorgehensweisen sind zum Verschieben einer vorhandenen Datenbank in eine neue Serverinstanz geeignet. Die zum Verschieben der Berichtsserver-Datenbank zu verwendende Vorgehensweise ist je nach Verfügbarkeitsanforderungen Ihres Systems unterschiedlich. Die einfachste Möglichkeit zum Verschieben der Berichtsserver-Datenbanken besteht darin, sie anzufügen und zu trennen. Bei dieser Vorgehensweise muss der Berichtsserver jedoch offline geschaltet werden, während Sie die Datenbank trennen. Sicherungen und Wiederherstellungen sind jedoch besser geeignet, wenn Sie Störungen des Diensts vermeiden möchten. Sie müssen für diese Vorgänge jedoch [!INCLUDE[tsql](../../includes/tsql-md.md)] -Befehle ausführen. Das Kopieren der Datenbank wird nicht empfohlen (insbesondere nicht mithilfe des Assistenten zum Kopieren von Datenbanken), da hierbei Berechtigungseinstellungen in der Datenbank verloren gehen.  
  
> [!IMPORTANT]  
>  Die in diesem Artikel vorgestellten Schritte sind geeignet, wenn das Verschieben der Berichtsserver-Datenbank die einzige Änderung ist, die Sie an der vorhandenen Installation vornehmen. Wenn eine vollständige [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Installation migriert wird (d.h. wenn die Datenbank verschoben und die Identität des Berichtsserver-Windows-Diensts, der die Datenbank verwendet, geändert wird), müssen die Verbindung neu konfiguriert und der Verschlüsselungsschlüssel zurückgesetzt werden.  
  
## <a name="detaching-and-attaching-the-report-server-databases"></a>Trennen und Anfügen der Berichtsserver-Datenbanken  
 Wenn Sie den Berichtsserver offline schalten können, können Sie die Datenbanken trennen, um sie in die gewünschte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz zu verschieben. Bei dieser Vorgehensweise bleiben die Berechtigungen in den Datenbanken erhalten. Wenn Sie eine SQL Server-Datenbank verwenden, müssen Sie diese in eine andere SQL Server-Instanz verschieben. Nachdem Sie die Datenbanken verschoben haben, müssen Sie die Verbindung des Berichtsservers mit der Berichtsserver-Datenbank erneut konfigurieren. Wenn Sie eine Bereitstellung für dezentrales Skalieren ausführen, müssen Sie die Verbindung der Berichtsserver-Datenbank für jeden Berichtsserver in der Bereitstellung erneut konfigurieren.  
  
 Führen Sie folgende Schritte aus, um die Datenbanken zu verschieben:  
  
1.  Sichern Sie die Verschlüsselungsschlüssel für die zu verschiebende Berichtsserver-Datenbank. Mit dem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurationstool können Sie die Schlüssel sichern.  
  
2.  Beenden Sie den Berichtsserverdienst. Mit dem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurationstool können Sie den Dienst beenden.  
  
3.  Starten Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], und stellen Sie eine Verbindung mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz her, die als Host für die Berichtsserver-Datenbanken fungiert.  
  
4.  Klicken Sie mit der rechten Maustaste auf die Berichtsserver-Datenbank, zeigen Sie auf „Tasks“, und klicken Sie auf **Trennen**. Wiederholen Sie diesen Schritt für die temporäre Berichtsserver-Datenbank.  
  
5.  Kopieren oder verschieben Sie die MDF- und LDF-Dateien in den Datenordner der gewünschten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz. Da Sie zwei Datenbanken verschieben, müssen Sie sicherstellen, dass Sie alle vier Dateien verschieben bzw. kopieren.  
  
6.  Öffnen Sie in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]eine Verbindung mit der neuen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz, die als Host für die Berichtsserver-Datenbanken fungiert.  
  
7.  Klicken Sie mit der rechten Maustaste auf den Knoten „Datenbanken“, und klicken Sie anschließend auf **Anfügen**.  
  
8.  Klicken Sie auf **Hinzufügen** , um die anzufügenden MDF- und LDF-Dateien der Berichtsserver-Datenbank auszuwählen. Wiederholen Sie diesen Schritt für die temporäre Berichtsserver-Datenbank.  
  
9. Überprüfen Sie nach dem Anfügen der Datenbanken, ob es sich bei **RSExecRole** um eine Datenbankrolle in der Berichtsserver-Datenbank und in der temporären Datenbank handelt. **RSExecRole** muss über Berechtigungen zum Auswählen, Einfügen, Aktualisieren, Löschen und Verweisen auf die Berichtsserver-Datenbanktabellen und über Berechtigungen für die gespeicherten Prozeduren verfügen. Weitere Informationen finden Sie unter [Erstellen der Rolle RSExecRole](../../reporting-services/security/create-the-rsexecrole.md).  
  
10. Starten Sie das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurationstool, und stellen Sie eine Verbindung mit dem Berichtsserver her.  
  
11. Wählen Sie auf der Datenbankseite die neue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz aus, und klicken Sie dann auf **Verbinden**.  
  
12. Wählen Sie die soeben verschobene Berichtsserver-Datenbank aus, und klicken Sie anschließend auf **Anwenden**.  
  
13. Klicken Sie auf der Seite Verschlüsselungsschlüssel auf Wiederherstellen. Gibt die Datei an, die eine Sicherungskopie der Schlüssel und des Kennworts zum Entsperren der Datei enthält.  
  
14. Starten Sie den Berichtsserverdienst neu.  
  
## <a name="backing-up-and-restoring-the-report-server-databases"></a>Sichern und Wiederherstellen der Berichtsserver-Datenbanken  
 Wenn Sie den Berichtsserver nicht offline schalten können, können Sie die Berichtsserver-Datenbanken durch Sichern und Wiederherstellen verschieben. Sie müssen [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen zum Sichern und Wiederherstellen verwenden. Nach dem Wiederherstellen der Datenbanken müssen Sie den Berichtsserver so konfigurieren, dass die Datenbank in der neuen Serverinstanz verwendet wird. Weitere Informationen finden Sie in den Anweisungen am Ende dieses Themas.  
  
### <a name="using-backup-and-copy_only-to-backup-the-report-server-databases"></a>Verwenden von BACKUP und COPY_ONLY zum Sichern der Berichtsserver-Datenbanken  
 Legen Sie beim Sichern der Datenbanken das COPY_ONLY-Argument fest. Achten Sie unbedingt darauf, dass Sie beide Datenbanken und Protokolldateien sichern.  
  
```  
-- To permit log backups, before the full database backup, alter the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE ReportServer  
   SET RECOVERY FULL  
  
-- If the ReportServerData device does not exist yet, create it.   
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerData',   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\BACKUP\ReportServerData.bak'  
  
-- Create a logical backup device, ReportServerLog.  
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerLog',   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\BACKUP\ReportServerLog.bak'  
  
-- Back up the full ReportServer database.  
BACKUP DATABASE ReportServer  
   TO ReportServerData  
   WITH COPY_ONLY  
  
-- Back up the ReportServer log.  
BACKUP LOG ReportServer  
   TO ReportServerLog  
   WITH COPY_ONLY  
  
-- To permit log backups, before the full database backup, alter the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE ReportServerTempdb  
   SET RECOVERY FULL  
  
-- If the ReportServerTempDBData device does not exist yet, create it.   
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerTempDBData',   
'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\BACKUP\ReportServerTempDBData.bak'  
  
-- Create a logical backup device, ReportServerTempDBLog.  
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerTempDBLog',   
'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\BACKUP\ReportServerTempDBLog.bak'  
  
-- Back up the full ReportServerTempDB database.  
BACKUP DATABASE ReportServerTempDB  
   TO ReportServerTempDBData  
   WITH COPY_ONLY  
  
-- Back up the ReportServerTempDB log.  
BACKUP LOG ReportServerTempDB  
   TO ReportServerTempDBLog  
   WITH COPY_ONLY  
```  
  
### <a name="using-restore-and-move-to-relocate-the-report-server-databases"></a>Verwenden von RESTORE und MOVE zum Verschieben der Berichtsserver-Datenbanken  
 Stellen Sie beim Wiederherstellen der Datenbanken sicher, dass Sie das MOVE-Argument einschließen, damit Sie einen Pfad angeben können. Mithilfe des NORECOVERY-Arguments wird die erste Wiederherstellung ausgeführt. Dadurch verbleibt die Datenbank im RESTORING-Status, und Sie haben Zeit, die Protokollsicherungen zu überprüfen und zu bestimmen, welche Sicherung wiederhergestellt werden soll. Im letzten Schritt wird der RESTORE-Vorgang mit dem RECOVERY-Argument wiederholt.  
  
 Das MOVE-Argument verwendet den logischen Namen der Datendatei. Führen Sie die folgende Anweisung aus, um den logischen Namen zu ermitteln: `RESTORE FILELISTONLY FROM DISK='C:\ReportServerData.bak';`  
  
 Im folgenden Beispiel ist das FILE-Argument enthalten, sodass Sie die Dateiposition der wiederherzustellenden Protokolldatei angeben können. Führen Sie die folgende Anweisung aus, um die Dateiposition zu ermitteln: `RESTORE HEADERONLY FROM DISK='C:\ReportServerData.bak';`  
  
 Beim Wiederherstellen der Datenbank- und Protokolldateien sollten Sie jeden RESTORE-Vorgang separat ausführen.  
  
```  
-- Restore the report server database and move to new instance folder   
RESTORE DATABASE ReportServer  
   FROM DISK='C:\ReportServerData.bak'  
   WITH NORECOVERY,   
      MOVE 'ReportServer' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer.mdf',   
      MOVE 'ReportServer_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer_Log.ldf';  
GO  
  
-- Restore the report server log file to new instance folder   
RESTORE LOG ReportServer  
   FROM DISK='C:\ReportServerData.bak'  
   WITH NORECOVERY, FILE=2  
      MOVE 'ReportServer' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer.mdf',   
      MOVE 'ReportServer_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer_Log.ldf';  
GO  
  
-- Restore and move the report server temporary database  
RESTORE DATABASE ReportServerTempdb  
   FROM DISK='C:\ReportServerTempDBData.bak'  
   WITH NORECOVERY,   
      MOVE 'ReportServerTempDB' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServerTempDB.mdf',   
      MOVE 'ReportServerTempDB_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\REportServerTempDB_Log.ldf';  
GO  
  
-- Restore the temporary database log file to new instance folder   
RESTORE LOG ReportServerTempdb  
   FROM DISK='C:\ReportServerTempDBData.bak'  
   WITH NORECOVERY, FILE=2  
      MOVE 'ReportServerTempDB' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServerTempDB.mdf',   
      MOVE 'ReportServerTempDB_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\REportServerTempDB_Log.ldf';  
GO  
  
-- Perform final restore  
RESTORE DATABASE ReportServer  
   WITH RECOVERY  
GO  
  
-- Perform final restore  
RESTORE DATABASE ReportServerTempDB  
   WITH RECOVERY  
GO  
```  
  
### <a name="how-to-configure-the-report-server-database-connection"></a>Konfigurieren der Berichtsserver-Datenbankverbindung  
  
1.  Starten Sie den [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurations-Manager, und stellen Sie eine Verbindung mit dem Berichtsserver her.  
  
2.  Doppelklicken Sie auf der Seite Datenbank auf **Datenbank ändern**. Klicken Sie auf **Weiter**.  
  
3.  Sie können auch auf **Wählen Sie eine vorhandene Berichtsserver-Datenbank aus** klicken. Klicken Sie auf **Weiter**.  
  
4.  Wählen Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , die nun als Host für die Berichtsserver-Datenbank fungiert, und klicken Sie auf **Verbindung testen**. Klicken Sie auf **Weiter**.  
  
5.  Wählen Sie unter "Datenbankname" die gewünschte Berichtsserver-Datenbank aus. Klicken Sie auf **Weiter**.  
  
6.  Geben Sie unter Anmeldeinformationen die Anmeldeinformationen an, die der Berichtsserver zur Herstellung der Verbindung mit der Berichtsserver-Datenbank verwendet. Klicken Sie auf **Weiter**.  
  
7.  Klicken Sie auf **Weiter** und anschließend auf **Fertig stellen**.  
  
> [!NOTE]  
>  Für eine [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Installation muss die [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] -Instanz die **RSExecRole** -Rolle beinhalten. Wenn Sie die Berichtsserver-Datenbankverbindung über das [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Konfigurationstool festlegen, werden Rollen erstellt und Anmelderegistrierungen sowie Rollenzuweisungen vorgenommen. Wenn Sie zum Konfigurieren der Verbindung alternative Vorgehensweisen verwenden (insbesondere das Befehlszeilen-Hilfsprogramm rsconfig.exe), ist der Berichtsserver nicht betriebsbereit. Sie müssen möglicherweise WMI-Code schreiben, um den Berichtsserver verfügbar zu machen. Weitere Informationen finden Sie unter [Zugreifen auf den Reporting Services-WMI-Anbieter](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md).  

## <a name="next-steps"></a>Nächste Schritte

[Erstellen der Rolle RSExecRole](../../reporting-services/security/create-the-rsexecrole.md)   
[Starten und Beenden des Berichtsserverdiensts](../../reporting-services/report-server/start-and-stop-the-report-server-service.md)   
[Konfigurieren einer Verbindung mit der Berichtsserver-Datenbank](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)   
[Konfigurieren des unbeaufsichtigten Ausführungskontos](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)   
[Berichtsserver-Konfigurations-Manager](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
[rsconfig-Hilfsprogramm](../../reporting-services/tools/rsconfig-utility-ssrs.md)   
[Konfigurieren und Verwalten von Verschlüsselungsschlüsseln](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)   
[Berichtsserver-Datenbank](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
