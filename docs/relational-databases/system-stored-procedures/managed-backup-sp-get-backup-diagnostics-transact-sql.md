---
description: managed_backup managed_backup.sp_get_backup_diagnostics (Transact-SQL)
title: managed_backup managed_backup.sp_get_backup_diagnostics (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_get_backup_diagnostics_TSQL
- sp_get_backup_diagnostics
- smart_admin.sp_get_backup_diagnostics_TSQL
- smart_admin.sp_get_backup_diagnostics
dev_langs:
- TSQL
helpviewer_keywords:
- sp_get_backup_diagnostics
- smart_admin.sp_get_backup_diagnostics
ms.assetid: 2266a233-6354-464b-91ec-824ca4eb9ceb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fbda9abd944665d3f0d52c174e0b1a5c1e488fa0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205291"
---
# <a name="managed_backupsp_get_backup_diagnostics-transact-sql"></a>managed_backup managed_backup.sp_get_backup_diagnostics (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Gibt die erweiterten Ereignisse zurück, die von Smart Admin protokolliert wurden.  
  
 Verwenden Sie diese gespeicherte Prozedur zum Überwachen von erweiterten Ereignissen, die von Smart admin protokolliert wurden. [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] Ereignisse werden in diesem System protokolliert und können mithilfe dieser gespeicherten Prozedur überprüft und überwacht werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```sql  
managed_backup.sp_get_backup_diagnostics [@xevent_channel = ] 'event type' [, [@begin_time = ] 'time1' ] [, [@end_time = ] 'time2'VARCHAR(255) = 'Xevent',@begin_time DATETIME = NULL,@end_time DATETIME = NULL  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a>Argumente  
 @xevent_channel  
 Der Typ des erweiterten Ereignisses. Bei Verwendung des Standardwerts werden alle in den letzten 30 Minuten protokollierten Ereignisse zurückgegeben. Die protokollierten Ereignisse hängen vom aktivierten Typ der erweiterten Ereignisse ab. Sie können mithilfe dieses Parameters die gespeicherte Prozedur filtern, sodass nur Ereignisse eines bestimmten Typs angezeigt werden. Sie können entweder den vollständigen Ereignis Namen oder eine Teil Zeichenfolge angeben, z. b.: **' admin'**, **' Analytic '**, **' Operational '** und **' Debug '**. Ist vom Datentyp @event_channel **varchar (255)**.  
  
 Verwenden Sie die **managed_backup. fn_get_current_xevent_settings** -Funktion, um eine Liste der derzeit aktivierten Ereignis Typen zu erhalten.  
  
 [@begin_time  
 Der Beginn des Zeitraums, für den die Ereignisse angezeigt werden sollen. Der- @begin_time Parameter ist vom Datentyp DateTime und hat den Standardwert NULL. Wenn dieser nicht angegeben ist, werden die Ereignisse der letzten 30 Minuten angezeigt.  
  
 @end_time  
 Das Ende des Zeitraums, für den die Ereignisse angezeigt werden sollen. Der @end_time Parameter ist vom DataTime und hat den Standardwert NULL.  Wenn dieser nicht angegeben ist, werden die Ereignisse bis zum aktuellen Zeitpunkt angezeigt.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
 Die gespeicherte Prozedur gibt eine Tabelle mit den folgenden Informationen zurück:  
  
| Spaltenname | Datentyp | BESCHREIBUNG |  
| ----------- | --------- | ----------- |
|event_type|Nvarchar (512)|Der Typ des erweiterten Ereignisses.|  
|Ereignis|Nvarchar (512)|Die Zusammenfassung der Ereignisprotokolle.|  
|Timestamp|timestamp|Der Zeitstempel des Ereignisses, der angibt, zu welchem Zeitpunkt das Ereignis ausgelöst wurde.|  
  
## <a name="security"></a>Sicherheit  
  
### <a name="permissions"></a>Berechtigungen  
 Erfordert **Ausführungs** Berechtigungen für die gespeicherte Prozedur. Außerdem ist die **View Server State** -Berechtigung erforderlich, da andere Systemobjekte, die diese Berechtigung erfordern, intern aufgerufen werden.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden alle für die letzten 30 Minuten protokollierten Ereignisse zurückgegeben.  
  
```  
Use msdb  
Go  
EXEC managed_backup.sp_get_backup_diagnostics  
  
```  
  
 Im folgenden Beispiel werden alle für einen bestimmten Zeitraum protokollierten Ereignisse zurückgegeben.  
  
```  
Use msdb  
Go  
EXEC managed_backup.sp_get_backup_diagnostics @xevent_channel = 'Admin',  
  @begin_time = '2013-06-01', @end_time = '2013-06-10'  
  
```  
  
 Im folgenden Beispiel werden alle für die letzten 30 Minuten protokollierten Analyseereignisse zurückgegeben.  
  
```  
Use msdb  
Go  
EXEC managed_backup.sp_get_backup_diagnostics @xevent_channel = 'Analytic'  
  
```  
  
  
