---
title: 'Sichern und Wiederherstellen: Systemdatenbanken'
description: SQL Server stellt Systemdatenbanken bereit, die wichtig für den Betrieb einer Serverinstanz sind. Einige Systemdatenbanken müssen nach jedem wichtigen Update gesichert werden.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- system databases [SQL Server], backing up and restoring
- restoring system databases [SQL Server]
- backing up [SQL Server], system databases
- database backups [SQL Server], system databases
- servers [SQL Server], backup
ms.assetid: aef0c4fa-ba67-413d-9359-1a67682fdaab
author: cawrites
ms.author: chadam
ms.openlocfilehash: c37eb7eb796e4ce8caed41dcdc55cd147f5916ea
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130554"
---
# <a name="backup--restore-system-databases-sql-server"></a>Sichern und Wiederherstellen: Systemdatenbanken (SQL Server)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet eine Reihe von Datenbanken auf Systemebene – die *Systemdatenbanken*, die für den Betrieb einer Serverinstanz von entscheidender Bedeutung sind. Einige der Systemdatenbanken müssen nach jedem wichtigen Update gesichert werden. Zu den Systemdatenbanken, die immer gesichert werden müssen, zählen **msdb**, **master** und **model**. Wenn eine Datenbank die Replikation auf der Serverinstanz verwendet, ist eine **distribution** -Systemdatenbank vorhanden, die Sie ebenfalls sichern müssen. Mit Sicherungen dieser Systemdatenbanken können Sie das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -System bei einem Systemfehler wiederherstellen, beispielsweise beim einem Ausfall einer Festplatte.  
  
 In der folgenden Tabelle werden alle Systemdatenbanken zusammengefasst.  
  
|Systemdatenbank|BESCHREIBUNG|Sicherungen erforderlich?|Wiederherstellungsmodell|Kommentare|  
|---------------------|-----------------|---------------------------|--------------------|--------------|  
|[master](../../relational-databases/databases/master-database.md)|In dieser Datenbank werden alle Informationen auf Systemebene für ein S [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -System aufgezeichnet.|Ja|Einfach|Sichern Sie die **master** -Datenbank so oft wie für Ihre Unternehmensanforderungen erforderlich, um die Daten ausreichend zu schützen. Wir empfehlen einen regelmäßigen Sicherungszeitplan, den Sie durch eine zusätzliche Sicherung nach umfangreicheren Updates ergänzen können.|  
|[model](../../relational-databases/databases/model-database.md)|Die Vorlage für alle Datenbanken, die für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]erstellt werden.|Ja|Vom Benutzer konfigurierbar*|Sichern Sie **model** nur, wenn dies für Ihre Unternehmensanforderungen erforderlich ist, beispielsweise unmittelbar nach dem Anpassen der entsprechenden Datenbankoptionen.<br /><br /> **Bewährte Methode:** Es wird empfohlen, dass Sie nach Bedarf ausschließlich vollständige Datenbanksicherungen von **model** erstellen. Da **model** klein ist und sich nur selten ändert, ist die Sicherung des Protokolls nicht notwendig.|  
|[msdb](../../relational-databases/databases/msdb-database.md)|Die Datenbank, die vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent zum Planen von Warnungen und Aufträgen sowie und zum Aufzeichnen von Operatoren verwendet wird. **msdb** enthält außerdem Verlaufstabellen, z. B. zu Sicherung und Wiederherstellung.|Ja|Einfach (Standard)|Sichern Sie die **msdb** -Datenbank bei jeder Aktualisierung.|  
|[Resource](../../relational-databases/databases/resource-database.md) (RDB)|Eine schreibgeschützte Datenbank mit Kopien aller Systemobjekte, die mitgeliefert werden mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Nein|-|Die **Resource** -Datenbank befindet sich in der Datei mssqlsystemresource.mdf, die ausschließlich Code enthält. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann die **Resource** -Datenbank daher nicht sichern.<br /><br /> Hinweis: Sie können eine dateibasierte oder eine datenträgerbasierte Sicherung der Datei „mssqlsystemresource.mdf“ ausführen, indem Sie die Datei als Binärdatei (EXE-Datei) statt als Datenbankdatei behandeln. Sie können diese Sicherungen allerdings nicht mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Wiederherstellung wiederherstellen. Die Wiederherstellung einer Sicherungskopie von mssqlsystemresource.mdf kann nur manuell erfolgen. Achten Sie darauf, die aktuelle **Resource** -Datenbank nicht durch eine veraltete oder potenziell unsichere Version zu überschreiben.|  
|[tempdb](../../relational-databases/databases/tempdb-database.md)|Ein Arbeitsbereich zum Speichern temporärer Resultsets oder Zwischenresultsets. Diese Datenbank wird jedes Mal neu erstellt, wenn eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gestartet wird. Wenn die Serverinstanz heruntergefahren wird, werden alle in **tempdb** enthaltenen Daten dauerhaft gelöscht.|Nein|Einfach|Die **tempdb** -Systemdatenbank kann nicht gesichert werden.|  
|[Verteilung konfigurieren](../../relational-databases/replication/configure-distribution.md)|Diese Datenbank ist nur vorhanden, wenn der Server als Replikationsverteiler konfiguriert wurde. In dieser Datenbank werden Metadaten und Verlaufsdaten für alle Replikationstypen sowie Transaktionen für die Transaktionsreplikation gespeichert.|Ja|Einfach|Informationen, wann die **distribution** -Datenbank gesichert werden sollte, finden Sie unter [Sichern und Wiederherstellen von replizierten Datenbanken](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md).|  
  
 *Informationen zum aktuellen Wiederherstellungsmodell der Datenbank finden Sie unter [Anzeigen oder Ändern des Wiederherstellungsmodells einer Datenbank &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md) oder [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
## <a name="limitations-on-restoring-system-databases"></a>Einschränkungen beim Wiederherstellen von Systemdatenbanken  
  
-   Systemdatenbanken können nur aus Sicherungen wiederhergestellt werden, die für die derzeit von der Serverinstanz aufgeführte Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellt wurden. Wenn Sie beispielsweise eine Systemdatenbank auf einer Serverinstanz wiederherstellen möchten, die mit [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 ausgeführt wird, müssen Sie eine Datenbanksicherung verwenden, die erstellt wurde, nachdem die Serverinstanz auf [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 aktualisiert wurde.  
  
-   Die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muss ausgeführt werden, um eine Datenbank wiederherstellen zu können. Zum Starten einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muss der Zugriff auf die **master** -Datenbank möglich sein, und die Datenbank muss zumindest teilweise verwendet werden können. Wenn **master** nicht mehr verwendet werden kann, gibt es mehrere Methoden, um sie in einen verwendbaren Zustand zurückzuversetzen:  
  
    -   Wiederherstellen der **master** -Datenbank von einer aktuellen Datenbanksicherung.  
  
         Wenn Sie die Serverinstanz starten können, sollten Sie in der Lage sein, **master** aus einer vollständigen Datenbanksicherung wiederherzustellen.  
  
    -   Vollständige Neuerstellung der **master** -Datenbank.  
  
         Falls ernsthafte Schäden an der **master** -Datenbank das Starten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verhindern, müssen Sie die **master**-Datenbank neu erstellen. Weitere Informationen finden Sie unter [Neuerstellen von Systemdatenbanken](../../relational-databases/databases/rebuild-system-databases.md).  
  
        > [!IMPORTANT]  
        >  Beim Neuerstellen der **master** -Datenbank werden alle Systemdatenbanken neu erstellt.  
  
-   Probleme beim Wiederherstellen der Modelldatenbank können es in einigen Fällen erforderlich machen, dass die Systemdatenbanken neu erstellt oder die MDF- und LDF-Dateien für die Modelldatenbank ersetzt werden. Weitere Informationen finden Sie unter [Neuerstellen von Systemdatenbanken](../../relational-databases/databases/rebuild-system-databases.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Erstellen einer vollständigen Datenbanksicherung &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Vollständige Datenbankwiederherstellungen &#40;einfaches Wiederherstellungsmodell&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
  
-   [Wiederherstellen der master-Datenbank &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-the-master-database-transact-sql.md)  
  
-   [Anzeigen oder Ändern des Wiederherstellungsmodells einer Datenbank &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md)  
  
-   [Verschieben von Systemdatenbanken](../../relational-databases/databases/move-system-databases.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verteilungsdatenbank](../../relational-databases/replication/distribution-database.md)   
 [master-Datenbank](../../relational-databases/databases/master-database.md)   
 [msdb-Datenbank](../../relational-databases/databases/msdb-database.md)   
 [model-Datenbank](../../relational-databases/databases/model-database.md)   
 [Ressourcendatenbank](../../relational-databases/databases/resource-database.md)   
 [tempdb-Datenbank](../../relational-databases/databases/tempdb-database.md)  
  
  
