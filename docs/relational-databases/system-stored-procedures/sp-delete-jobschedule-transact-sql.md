---
description: sp_delete_jobschedule (Transact-SQL)
title: sp_delete_jobschedule (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_jobschedule
- sp_delete_jobschedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_jobschedule
ms.assetid: 82fbb48b-603a-4016-a7fb-1ce17fb76919
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c06030d2cc646dd269a0b3cb6bf564ce9248c9de
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195522"
---
# <a name="sp_delete_jobschedule-transact-sql"></a>sp_delete_jobschedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Löscht einen Zeitplan für einen Auftrag.  
  
 **sp_delete_jobschedule** wird nur aus Gründen der Abwärtskompatibilität bereitgestellt.  
  
  
## <a name="remarks"></a>Bemerkungen  
 Auftragszeitpläne können jetzt unabhängig von Aufträgen verwaltet werden. Verwenden Sie **sp_detach_schedule**, um einen Zeitplan von einem Auftrag zu entfernen. Verwenden Sie **sp_delete_schedule**, um einen Zeitplan zu löschen.  
  
> **Hinweis: sp_delete_jobschedule** unterstützt keine Zeitpläne, die an mehrere Aufträge angefügt sind. Wenn ein vorhandenes Skript **sp_delete_jobschedule** aufruft, um einen Zeitplan zu entfernen, der an mehrere Aufträge angefügt ist, gibt die Prozedur einen Fehler zurück.  
  
## <a name="permissions"></a>Berechtigungen  
 Standardmäßig können nur Mitglieder der festen Serverrolle **sysadmin** diese gespeicherte Prozedur ausführen. Andere Benutzer müssen Mitglieder der festen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Datenbankrollen in der **msdb** -Datenbank sein:  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Weitere Informationen zu den Berechtigungen dieser Rollen finden Sie unter [Feste Datenbankrollen des SQL Server-Agents](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Mitglieder der **sysadmin** -Rolle können jeden Auftragszeitplan löschen. Benutzer, die nicht Mitglied der **sysadmin** -Rolle sind, können nur Aufträge löschen, deren Besitzer sie sind.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_delete_schedule &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_detach_schedule &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-detach-schedule-transact-sql.md)   
 [Anzeigen oder Ändern von Aufträgen](../../ssms/agent/view-or-modify-jobs.md)   
 [sp_add_schedule &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_help_jobschedule &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-help-jobschedule-transact-sql.md)   
 [sp_update_jobschedule &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-update-jobschedule-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
