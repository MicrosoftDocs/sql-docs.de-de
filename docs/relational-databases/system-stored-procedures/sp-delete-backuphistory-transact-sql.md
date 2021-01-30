---
description: sp_delete_backuphistory (Transact-SQL)
title: sp_delete_backuphistory (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_backuphistory
- sp_delete_backuphistory_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_backuphistory
ms.assetid: bdb56834-616e-47e4-b942-e895d2325e97
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ac5426940ed3fd2a94c055968a22ec1000e4d564
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195547"
---
# <a name="sp_delete_backuphistory-transact-sql"></a>sp_delete_backuphistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Reduziert die Größe der Sicherungs- und Wiederherstellungsverlaufstabellen, indem die Einträge für Sicherungssätze gelöscht werden, die älter sind als das angegebene Datum. Der Sicherungs-und Wiederherstellungs Verlaufs Tabelle werden nach jedem Sicherungs-oder Wiederherstellungs Vorgang zusätzliche Zeilen hinzugefügt. aus diesem Grund wird empfohlen, **sp_delete_backuphistory** regelmäßig auszuführen.  
  
> [!NOTE]  
>  Die Sicherungs-und Wiederherstellungs Verlaufs Tabellen befinden sich in der **msdb** -Datenbank.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_delete_backuphistory [ @oldest_date = ] 'oldest_date'   
```  
  
## <a name="arguments"></a>Argumente  
`[ @oldest_date = ] 'oldest\_date'` Ist das älteste Datum, das in den Sicherungs-und Wiederherstellungs Verlaufs Tabellen beibehalten wird. *oldest_date* ist vom **Datentyp DateTime** und hat keinen Standardwert.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="result-sets"></a>Resultsets  
 Keine  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_delete_backuphistory** muss von der **msdb** -Datenbank aus ausgeführt werden und wirkt sich auf die folgenden Tabellen aus:  
  
-   [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md)  
  
-   [backupfilegroup](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)  
  
-   [backupmediafamily](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)  
  
-   [backupmediaset](../../relational-databases/system-tables/backupmediaset-transact-sql.md)  
  
-   [backupset](../../relational-databases/system-tables/backupset-transact-sql.md)  
  
-   [restorefile](../../relational-databases/system-tables/restorefile-transact-sql.md)  
  
-   [restorefilegroup](../../relational-databases/system-tables/restorefilegroup-transact-sql.md)  
  
-   [restorehistory](../../relational-databases/system-tables/restorehistory-transact-sql.md)  
  
 Die physischen Sicherungsdateien werden beibehalten, auch wenn der gesamte Verlauf gelöscht wird.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der festen Server Rolle **sysadmin** , aber Berechtigungen können anderen Benutzern erteilt werden.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden alle Einträge in den Sicherungs- und Wiederherstellungsverlaufstabellen gelöscht, die weiter zurückliegen als der 14. Januar 2010, 00:00:00 Uhr.  
  
```  
USE msdb;  
GO  
EXEC sp_delete_backuphistory @oldest_date = '01/14/2010';  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_delete_database_backuphistory &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-delete-database-backuphistory-transact-sql.md)   
 [Sicherungsverlauf und Headerinformationen &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  
