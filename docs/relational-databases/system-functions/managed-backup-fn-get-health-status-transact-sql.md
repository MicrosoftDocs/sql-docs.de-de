---
description: managed_backup managed_backup.fn_get_health_status (Transact-SQL)
title: managed_backup managed_backup.fn_get_health_status (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_get_health_status_TSQL
- smart_admin.fn_get_health_status_TSQL
- smart_admin.fn_get_health_status
- fn_get_health_status
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_get_health_status
- fn_get_health_status
ms.assetid: b376711d-444a-4b5e-b483-8df323b4e31f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b139114cb7be4094c652f1fbf69313d4f7f1f2db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207400"
---
# <a name="managed_backupfn_get_health_status-transact-sql"></a>managed_backup managed_backup.fn_get_health_status (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Gibt eine Tabelle mit keiner, einer oder mehreren Zeilen der aggregierten Anzahl der Fehler zurück, die durch erweiterte Ereignisse während eines bestimmten Zeitraums gemeldet wurden.  
  
 Die-Funktion wird verwendet, um den Integritäts Status von Diensten unter Smart admin zu melden.  Wird zurzeit [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] unter dem intelligenten Administrator unterstützt. Daher beziehen sich die zurückgegebenen Fehler auf [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```sql  
managed_backup.fn_get_health_status([@begin_time = ] 'time_1' , [ @end_time = ] 'time_2')  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a>Argumente  
 [@begin_time]  
 Der Beginn des Zeitraums, von dem an die aggregierte Anzahl von Fehlern berechnet wird.  Der @begin_time Parameter ist "DateTime". Der Standardwert ist NULL. Wenn der Wert NULL ist, verarbeitet die Funktion die Ereignisse, die bereits 30 Minuten vor der aktuellen Zeit gemeldet wurden.  
  
 [ @end_time]  
 Das Ende des Zeitraums, von dem an die aggregierte Anzahl von Fehlern berechnet wird. Der- @end_time  Parameter ist vom Datentyp DateTime und hat den Standardwert NULL. Wenn der Wert NULL ist, verarbeitet die Funktion erweiterte Ereignisse bis zur aktuellen Zeit.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|number_of_storage_connectivity_errors|INT|Anzahl der Verbindungsfehler, wenn das Programm eine Verbindung mit dem Azure Storage-Konto herstellt.|  
|number_of_sql_errors|INT|Die Anzahl der Fehler, die zurückgegeben werden, wenn das Programm eine Verbindung mit der SQL Server Engine herstellt.|  
|number_of_invalid_credential_errors|INT|Die Anzahl der Fehler, die zurückgegeben werden, wenn das Programm versucht, sich mit den SQL-Anmeldeinformationen zu authentifizieren.|  
|number_of_other_errors|INT|Die Anzahl der Fehler aus anderen Kategorien außer Konnektivität, SQL oder Anmeldeinformationen.|  
|number_of_corrupted_or_deleted_backups|INT|Die Anzahl der gelöschten oder beschädigte Sicherungsdateien.|  
|number_of_backup_loops|INT|Die Anzahl der Scans, die der Sicherungs-Agent für alle mit [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] konfigurierten Datenbanken ausführt sind.|  
|number_of_retention_loops|INT|Die Anzahl der Datenbankscans, die zur Ermittlung der festgelegten Beibehaltungsdauer ausgeführt werden.|  
  
## <a name="best-practices"></a>Empfehlungen  
 Anhand dieser aggregierten Anzahl kann die Systemintegrität überwacht werden. Wenn die Spalte number_ of_retention_loops nach 30 Minuten beispielsweise 0 ist, dauert die Überwachung der Beibehaltungsdauer entweder sehr lange oder funktioniert nicht ordnungsgemäß. Spalten mit Werten ungleich 0 können auf Probleme hindeuten. Sie sollten die Protokolle der erweiterten Ereignisse prüfen, um das Problem einzugrenzen. Verwenden Sie alternativ die gespeicherte Prozedur **managed_backup. sp_get_backup_diagnostics** , um eine Liste der erweiterten Ereignisse zum Ermitteln der Details des Fehlers zu erhalten.  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Erfordert **Select** -Berechtigungen für die Funktion.  
  
## <a name="examples"></a>Beispiele  
  
-   Das folgende Beispiel gibt die aggregierte Fehleranzahl für die letzten 30 Minuten seit der Ausführung zurück.  
  
    ```  
    SELECT *  
    FROM managed_backup.fn_get_health_status(NULL, NULL)  
  
    ```  
  
-   Im folgenden Beispiel wird die aggregierte Fehleranzahl für die aktuelle Woche zurückgegeben:  
  
    ```  
    Use msdb  
    Go  
    DECLARE @startofweek datetime  
    DECLARE @endofweek datetime  
    SET @startofweek = DATEADD(Day, 1-DATEPART(WEEKDAY, CURRENT_TIMESTAMP), CURRENT_TIMESTAMP)   
    SET @endofweek = DATEADD(Day, 7-DATEPART(WEEKDAY, CURRENT_TIMESTAMP), CURRENT_TIMESTAMP)  
    SELECT *  
    FROM managed_backup.fn_get_health_status(@startofweek, @endofweek)  
  
    ```  
  
  
