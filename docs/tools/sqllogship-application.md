---
title: Anwendung sqllogship
description: Von der Anwendung „sqllogship“ werden ein Sicherungs-, Kopier- oder Wiederherstellungsvorgang und Bereinigungstasks für eine Protokollversandkonfiguration auf einer SQL Server-Datenbank ausgeführt.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- sqllogship
ms.assetid: 8ae70041-f3d9-46e4-8fa8-31088572a9f8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 349b8f5bd757e623b79b043d14ce9de91c34e942
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341987"
---
# <a name="sqllogship-application"></a>Anwendung sqllogship
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  Von der Anwendung **sqllogship** werden ein Sicherungs-, Kopier- oder Wiederherstellungsvorgang und zugeordnete Cleanuptasks für eine Protokollversandkonfiguration ausgeführt. Der Vorgang wird in einer bestimmten Instanz von [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] für eine bestimmte Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") Informationen zu den Syntaxkonventionen finden Sie unter [Referenz zum Eingabeaufforderungs-Hilfsprogramm &#40;Datenbank-Engine&#41;](../tools/command-prompt-utility-reference-database-engine.md).  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sqllogship -server instance_name { -backup primary_id | -copy secondary_id | -restore secondary_id } [ -verboselevel level ] [ -logintimeout timeout_value ] [ -querytimeout timeout_value ]  
```  
  
## <a name="arguments"></a>Argumente  
 **-server** _instance_name_  
 Gibt die Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] an, in der der Vorgang ausgeführt wird. Die anzugebende Serverinstanz hängt vom angegebenen Protokollversandvorgang ab. Für **-backup** muss *instance_name* der Name des primären Servers in einer Protokollversandkonfiguration sein. Für **-copy** oder **-restore** muss *instance_name* der Name eines sekundären Servers in einer Protokollversandkonfiguration sein.  
  
 **-backup** _primary_id_  
 Führt einen Sicherungsvorgang für die primäre Datenbank aus, deren primäre ID durch *primary_id* angegeben ist. Sie können diese ID abrufen, indem Sie sie in der [log_shipping_primary_databases](../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md) -Systemtabelle auswählen oder die gespeicherte Prozedur [sp_help_log_shipping_primary_database](../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md) verwenden.  
  
 Bei diesem Sicherungsvorgang wird die Protokollsicherung im Sicherungsverzeichnis erstellt. Für die veralteten Sicherungsdateien wird dann auf der Grundlage der Beibehaltungsdauer der Datei von der Anwendung **sqllogship** ein Cleanup ausgeführt. Anschließend wird der Verlauf für den Sicherungsvorgang von der Anwendung auf dem primären Server und dem Überwachungsserver protokolliert. Abschließend wird von der Anwendung die Prozedur [sp_cleanup_log_shipping_history](../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)ausgeführt, sodass auf der Grundlage der Beibehaltungsdauer veraltete Verlaufsinformationen gelöscht werden.  
  
 **-copy** _secondary_id_  
 Führt einen Kopiervorgang aus, um Sicherungen vom angegebenen sekundären Server für die sekundäre Datenbank bzw. sekundären Datenbanken zu kopieren, deren sekundäre ID durch *secondary_id* angegeben ist. Sie können diese ID abrufen, indem Sie sie aus der [log_shipping_secondary](../relational-databases/system-tables/log-shipping-secondary-transact-sql.md) -Systemtabelle auswählen oder die gespeicherte Prozedur [sp_help_log_shipping_secondary_database](../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md) verwenden.  
  
 Bei dem Vorgang werden die Sicherungsdateien aus dem Sicherungsverzeichnis in das Zielverzeichnis kopiert. Dann wird der Verlauf für den Kopiervorgang von der Anwendung **sqllogship** auf dem sekundären Server und dem Überwachungsserver protokolliert.  
  
 **-restore** _secondary_id_  
 Führt auf dem angegebenen sekundären Server einen Wiederherstellungsvorgang für die sekundäre Datenbank bzw. sekundären Datenbanken aus, deren sekundäre ID durch *secondary_id* angegeben ist. Sie können diese ID mithilfe der gespeicherten Prozedur **sp_help_log_shipping_secondary_database** abrufen.  
  
 Alle Sicherungsdateien im Zielverzeichnis, die nach dem letzten Wiederherstellungspunkt erstellt wurden, werden in der sekundären Datenbank bzw. in den sekundären Datenbanken wiederhergestellt. Für die veralteten Sicherungsdateien wird dann auf der Grundlage der Beibehaltungsdauer der Datei von der Anwendung **sqllogship** ein Cleanup ausgeführt. Anschließend wird der Verlauf für den Wiederherstellungsvorgang von der Anwendung auf dem sekundären Server und dem Überwachungsserver protokolliert. Abschließend wird von der Anwendung die Prozedur **sp_cleanup_log_shipping_history** ausgeführt, sodass auf der Grundlage der Beibehaltungsdauer veraltete Verlaufsinformationen gelöscht werden.  
  
 **-verboselevel** _level_  
 Gibt die Ebene der dem Protokollversandverlauf hinzugefügten Meldungen an. *level* entspricht einer der folgenden ganzen Zahlen:  
  
|Ebene|Beschreibung|  
|-----------|-----------------|  
|0|Keine Ausgabe von Ablaufverfolgungs- oder Debugmeldungen|  
|1|Ausgabe von Fehlerbehandlungsmeldungen|  
|2|Ausgabe von Warnungen und Fehlerbehandlungsmeldungen|  
|**3**|Ausgabe von Informationsmeldungen, Warnungen und Fehlerbehandlungsmeldungen. Dies ist der Standardwert.|  
|4|Ausgabe aller Debug- und Ablaufverfolgungsmeldungen|  
  
 **-logintimeout** _timeout_value_  
 Gibt die für das Anmelden an der Serverinstanz vorgesehene Zeit an, bevor für den Versuch ein Timeout auftritt. Der Standardwert ist 15 Sekunden. *timeout_value* hat den Typ **int** _._  
  
 **-querytimeout** _timeout_value_  
 Gibt die für das Starten des angegebenen Vorgangs vorgesehene Zeit an, bevor für den Versuch ein Timeout auftritt. Der Standard ist kein Timeout. *timeout_value* hat den Typ **int** _._  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie zum Sichern, Kopieren und Wiederherstellen nach Möglichkeit die Sicherungs-, Kopier- und Wiederherstellungsaufträge. Rufen Sie die gespeicherte Prozedur [sp_start_job](../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md) auf, um diese Aufträge aus einem Batchvorgang oder einer anderen Anwendung zu starten.  
  
 Der von **sqllogship** erstellte Protokollversandverlauf wird in den vom Protokollversand-Sicherungsauftrag, -Kopierauftrag und -Wiederherstellungsauftrag erstellten Verlauf eingefügt. Wenn Sie **sqllogship** wiederholt zum Ausführen von Sicherungs-, Kopier- und Wiederherstellungsvorgängen für eine Protokollversandkonfiguration verwenden möchten, sollten Sie den entsprechenden Protokollversandauftrag bzw. die entsprechenden Protokollversandaufträge deaktivieren. Weitere Informationen finden sie unter [Disable or Enable a Job](../ssms/agent/disable-or-enable-a-job.md).  
  
 Die **sqllogship** -Anwendung (SqlLogShip.exe) ist im Verzeichnis „x:\Programme\Microsoft SQL Server\130\Tools\Binn“ installiert.  
  
## <a name="permissions"></a>Berechtigungen  
 Für **sqllogship** wird die Windows-Authentifizierung verwendet. Für das Windows-Authentifizierungskonto zum Ausführen des Befehls sind ein Windows-Verzeichniszugriff und [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Berechtigungen erforderlich. Die Anforderung hängt davon ab, ob der **sqllogship** -Befehl die Option **-backup**, **-copy** oder **-restore** festlegt.  
  
|Option|Verzeichniszugriff|Berechtigungen|  
|------------|----------------------|-----------------|  
|**-backup**|Erfordert Lese-/Schreibzugriff auf das Sicherungsverzeichnis.|Erfordert dieselben Berechtigungen wie die BACKUP-Anweisung. Weitere Informationen finden Sie unter [BACKUP &#40;Transact-SQL&#41;](../t-sql/statements/backup-transact-sql.md).|  
|**-copy**|Erfordert Lesezugriff auf das Sicherungsverzeichnis und Schreibzugriff auf das Kopieverzeichnis.|Erfordert dieselben Berechtigungen wie die gespeicherte Prozedur [sp_help_log_shipping_secondary_database](../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md) .|  
|**-restore**|Erfordert Lese-/Schreibzugriff auf das Kopieverzeichnis.|Erfordert dieselben Berechtigungen wie die RESTORE-Anweisung. Weitere Informationen finden Sie unter [RESTORE &#40;Transact-SQL&#41;](../t-sql/statements/restore-statements-transact-sql.md)nicht wiederhergestellt werden.|  
  
> [!NOTE]  
>  Um die Pfade der Sicherungs- und Kopierverzeichnisse zu ermitteln, können Sie die gespeicherte Prozedur **sp_help_log_shipping_secondary_database** ausführen oder die **log_shipping_secondary-Tabelle** in **msdb** anzeigen. Der Pfad des Sicherungsverzeichnisses und des Zielverzeichnisses befindet sich in der **backup_source_directory** -Spalte bzw. in der **backup_destination_directory** -Spalte.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Informationen zum Protokollversand &#40;SQL Server&#41;](../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [log_shipping_primary_databases &#40;Transact-SQL&#41;](../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md)   
 [log_shipping_secondary &#40;Transact-SQL&#41;](../relational-databases/system-tables/log-shipping-secondary-transact-sql.md)   
 [sp_cleanup_log_shipping_history &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-cleanup-log-shipping-history-transact-sql.md)   
 [sp_help_log_shipping_primary_database &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)   
 [sp_help_log_shipping_secondary_database &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)   
 [sp_start_job &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)  
  
  
