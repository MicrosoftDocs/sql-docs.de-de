---
description: sp_delete_backup_file_snapshot (Transact-SQL)
title: sp_delete_backup_file_snapshot (Transact-SQL) | Microsoft-Dokumentation
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.custom: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 5afe5530-a404-4fa5-af3c-bc7c3ca43ce6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0edcaca6a812ed5162aa94a091a5f2d27a83baab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186026"
---
# <a name="sp_delete_backup_file_snapshot-transact-sql"></a>sp_delete_backup_file_snapshot (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Löscht einen angegebenen Sicherungssnapshot aus der angegebenen Datenbank. Verwenden Sie diese gespeicherte System Prozedur in Verbindung mit der **sys.fn_db_backup_file_snapshots** -Systemfunktion, um verwaiste Sicherungs Momentaufnahmen zu identifizieren und zu löschen. Weitere Informationen finden Sie unter [Dateimomentaufnahme-Sicherungen für Datenbankdateien in Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  

  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sys.sp_delete_backup_file_snapshot  
    [ @db_name = ] N'<database_name>  
    , [ @snapshot_url = ] N'<snapshot_url>  
```  
  
## <a name="arguments"></a>Argumente  
 *[ @db_name =] database_name*  
 Der Name der Datenbank mit der zu löschenden Momentaufnahme, die als Unicode-Zeichenfolge bereitgestellt wird.  
  
 *[ @snapshot_url =] snapshot_url*  
 Die URL der zu löschenden Momentaufnahme, die als Unicode-Zeichenfolge bereitgestellt wird.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER ANY DATABASE-Berechtigung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.fn_db_backup_file_snapshots &#40;Transact-SQL-&#41;](../../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md)   
 [sp_delete_backup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md)  
  
  
