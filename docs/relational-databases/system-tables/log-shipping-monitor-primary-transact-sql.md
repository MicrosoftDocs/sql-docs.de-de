---
description: log_shipping_monitor_primary (Transact-SQL)
title: log_shipping_monitor_primary (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- log_shipping_monitor_primary
- log_shipping_monitor_primary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_monitor_primary system table
ms.assetid: 5f629a29-1a62-40e6-ae33-6f6b7dd09a36
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8511681e673429cbe45ef3885e72fb6c5e8009e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209633"
---
# <a name="log_shipping_monitor_primary-transact-sql"></a>log_shipping_monitor_primary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Speichert einen Überwachungsdatensatz pro primärer Datenbank in jeder Protokollversandkonfiguration. Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
 Die verlaufs- und überwachungsverbundenen Tabellen werden auch auf dem primären und dem sekundären Server verwendet.   
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**primary_id**|**uniqueidentifier**|Die ID der primären Datenbank für die Protokollversandkonfiguration.|  
|**primary_server**|**sysname**|Der Name der primären Instanz von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] in der Protokollversandkonfiguration.|  
|**primary_database**|**sysname**|Der Name der primären Datenbank in der Protokollversandkonfiguration|  
|**backup_threshold**|**int**|Die Anzahl von Minuten, die zwischen Sicherungsvorgängen verstreichen darf, bevor eine Warnung generiert wird.|  
|**threshold_alert**|**int**|Die Warnung, die bei Überschreiten des Sicherungsschwellenwertes ausgelöst wird.|  
|**threshold_alert_enabled**|**bit**|Bestimmt, ob Warnungen für Sicherungsschwellenwerte aktiviert sind. 1 = Aktiviert.<br /><br /> 0 = Deaktiviert.|  
|**last_backup_file**|**nvarchar (500)**|Der absolute Pfad der jüngsten Transaktionsprotokollsicherung.|  
|**last_backup_date**|**datetime**|Die Uhrzeit und das Datum der letzten Transaktionsprotokollsicherung in der primären Datenbank.|  
|**last_backup_date_utc**|**datetime**|Die Uhrzeit und das Datum der letzten Transaktionsprotokollsicherung in der primären Datenbank in UTC.|  
|**history_retention_period**|**int**|Der Zeitraum in Minuten, den Verlaufsdatensätze des Protokollversands für eine bestimmte primäre Datenbank aufbewahrt werden, ehe sie gelöscht werden.|  
  
## <a name="remarks"></a>Bemerkungen  
 Die Informationen, die sich auf den primären Server beziehen, werden nicht nur auf dem Remoteüberwachungsserver, sondern auch auf dem primären Server selbst in der **log_shipping_monitor_primary** -Tabelle gespeichert.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Informationen zum Protokollversand &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_primary_database &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)   
 [sp_change_log_shipping_primary_database &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-change-log-shipping-primary-database-transact-sql.md)   
 [sp_delete_log_shipping_primary_database &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_help_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)   
 [sp_refresh_log_shipping_monitor &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)   
 [sp_help_log_shipping_monitor_primary &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-primary-transact-sql.md)   
 [sp_delete_log_shipping_alert_job &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-alert-job-transact-sql.md)   
 [Systemtabellen &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
