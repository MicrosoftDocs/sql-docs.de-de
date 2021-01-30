---
description: sp_help_log_shipping_secondary_database (Transact-SQL)
title: sp_help_log_shipping_secondary_database (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_secondary_database
- sp_help_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_secondary_database
ms.assetid: 11ce42ca-d3f1-44c8-9cac-214ca8896b9a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a5fc81b903438d874a80399563527747ada9ef43
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199626"
---
# <a name="sp_help_log_shipping_secondary_database-transact-sql"></a>sp_help_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Diese gespeicherte Prozedur ruft die Einstellungen für mindestens eine sekundäre Datenbank ab.  
  

  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_help_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database' OR  
[ @secondary_id = ] 'secondary_id'  
```  
  
## <a name="arguments"></a>Argumente  
`[ @secondary_database = ] 'secondary_database'` Der Name der sekundären Datenbank. *secondary_database* ist vom Datentyp **sysname** und hat keinen Standardwert.  
  
`[ @secondary_id = ] 'secondary_id'` Die ID für den sekundären Server in der Protokoll Versand Konfiguration. *secondary_id* ist vom Datentyp **uniqueidentifier** und darf nicht NULL sein.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="result-sets"></a>Resultsets  
  
|Spaltenname|BESCHREIBUNG|  
|-----------------|-----------------|  
|**secondary_id**|Die ID für den sekundären Server in der Protokollversandkonfiguration.|  
|**primary_server**|Der Name der primären Instanz von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] in der Protokollversandkonfiguration.|  
|**primary_database**|Der Name der primären Datenbank in der Protokollversandkonfiguration|  
|**backup_source_directory**|Das Verzeichnis, in dem die Dateien der Transaktionsprotokollsicherung gespeichert werden.|  
|**backup_destination_directory**|Das Verzeichnis auf dem sekundären Server, in das Sicherungsdateien kopiert werden|  
|**file_retention_period**|Gibt an, wie lange (in Minuten) eine Sicherungsdatei auf dem sekundären Server aufbewahrt wird, bevor sie gelöscht wird|  
|**copy_job_id**|Die dem Kopierauftrag zugeordnete ID auf dem sekundären Server|  
|**restore_job_id**|Die dem Wiederherstellungsauftrag zugeordnete ID auf dem sekundären Server|  
|**monitor_server**|Der Name der Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] , die als Überwachungsserver in der Protokollversandkonfiguration verwendet wird.|  
|**monitor_server_security_mode**|Der Sicherheitsmodus, der zum Herstellen einer Verbindung mit dem Überwachungsserver verwendet wird.<br /><br /> 1 = [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Authentifizierung<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Authentifizierung.|  
|**secondary_database**|Der Name der sekundären Datenbank in der Protokollversandkonfiguration.|  
|**restore_delay**|Die Zeit in Minuten, die der sekundäre Server vor dem Wiederherstellen einer bestimmten Sicherungsdatei wartet. Die Standardeinstellung beträgt 0 Minuten.|  
|**restore_all**|Falls 1, stellt der sekundäre Server bei Ausführung des Wiederherstellungsauftrags alle verfügbaren Sicherungen des Transaktionsprotokolls wieder her. Andernfalls wird Sie beendet, nachdem eine Datei wieder hergestellt wurde.|  
|**restore_mode**|Der Wiederherstellungsmodus für die sekundäre Datenbank.<br /><br /> 0 = Das Protokoll wird mit NORECOVERY wiederhergestellt.<br /><br /> 1 = Das Protokoll wird mit STANDBY wiederhergestellt.|  
|**disconnect_users**|Falls 1, werden Benutzer beim Ausführen eines Wiederherstellungsvorgangs von der sekundären Datenbank getrennt. Standardwert = 0.|  
|**block_size**|Die Größe in Bytes, die als Blockgröße für das Sicherungsmedium verwendet wird.|  
|**buffer_count**|Die Gesamtanzahl der beim Sicherungs- oder Wiederherstellungsvorgang verwendeten Puffer.|  
|**max_transfer_size**|Die Größe der maximalen Eingabe- oder Ausgabeanforderung in Bytes, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an das Sicherungsmedium ausgegeben wird.|  
|**restore_threshold**|Die Anzahl der zulässigen Minuten zwischen Wiederherstellungsvorgängen, bevor eine Warnung generiert wird.|  
|**threshold_alert**|Die Warnung, die ausgelöst wird, wenn die Wiederherstellungsschwelle überschritten wird.|  
|**threshold_alert_enabled**|Legt fest, ob Warnungen für Wiederherstellungsschwellen aktiviert sind.<br /><br /> 1 = Aktiviert.<br /><br /> 0 = Deaktiviert.|  
|**last_copied_file**|Der Dateiname der letzten Sicherungsdatei, die auf den sekundären Server kopiert wurde.|  
|**last_copied_date**|Datum und Uhrzeit des letzten Kopiervorgangs auf den sekundären Server|  
|**last_copied_date_utc**|Datum und Uhrzeit des letzten Kopiervorgangs auf den sekundären Server in UTC (Coordinated Universal Time).|  
|**last_restored_file**|Der Dateiname der letzten Sicherungsdatei, die in der sekundären Datenbank wiederhergestellt wurde.|  
|**last_restored_date**|Datum und Uhrzeit des letzten Wiederherstellungsvorgangs für die sekundäre Datenbank.|  
|**last_restored_date_utc**|Datum und Uhrzeit des letzten Wiederherstellungsvorgangs auf dem sekundären Server in UTC (Coordinated Universal Time).|  
|**history_retention_period**|Die Zeitdauer in Minuten, die Verlaufsdatensätze des Protokollversands für eine bestimmte sekundäre Datenbank vor dem Löschen aufbewahrt werden.|  
|**last_restored_latency**|Der Zeitraum in Minuten zwischen dem Erstellen der Protokollsicherung auf dem primären Server und dem Wiederherstellen auf dem sekundären Server.<br /><br /> Der Anfangswert ist NULL.|  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn Sie den *secondary_database* -Parameter einschließen, enthält das Resultset Informationen zu dieser sekundären Datenbank. Wenn Sie den *secondary_id* -Parameter einschließen, enthält das Resultset Informationen zu allen sekundären Datenbanken, die mit dieser sekundären ID verknüpft sind.  
  
 **sp_help_log_shipping_secondary_database** muss von der **Master** -Datenbank auf dem sekundären Server ausgeführt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Serverrolle **sysadmin** können diese Prozedur ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_help_log_shipping_secondary_primary &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-primary-transact-sql.md)   
 [Informationen zum Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
