---
description: sp_can_tlog_be_applied (Transact-SQL)
title: sp_can_tlog_be_applied (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_can_tlog_be_applied_TSQL
- sp_can_tlog_be_applied
dev_langs:
- TSQL
helpviewer_keywords:
- sp_can_tlog_be_applied
ms.assetid: 9c143b6c-27ac-4ab7-98d1-3b7b265f3963
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9521c435d17e94a034aa1723f5cd023035cd1f87
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197690"
---
# <a name="sp_can_tlog_be_applied-transact-sql"></a>sp_can_tlog_be_applied (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Überprüft, ob die Sicherung eines Transaktionsprotokolls auf eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank angewendet werden kann. **sp_can_tlog_be_applied** setzt voraus, dass sich die Datenbank im Wiederherstellungsstatus befindet.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_can_tlog_be_applied [ @backup_file_name = ] 'backup_file_name'   
        , [ @database_name = ] 'database_name'   
        , [ @result = ] result OUTPUT  
```  
  
## <a name="arguments"></a>Argumente  
`[ @backup_file_name = ] 'backup_file_name'` Der Name einer Sicherungsdatei. *backup_file_name* ist **nvarchar(128)**  
  
`[ @database_name = ] 'database_name'` Der Name der Datenbank. *database_name* ist **sysname**  
  
`[ @result = ] _result_ OUTPUT` Gibt an, ob das Transaktionsprotokoll auf die Datenbank angewendet werden kann. *result* ist **bit**  
  
 1 = Das Protokoll kann angewendet werden.  
  
 0 = Das Protokoll kann nicht angewendet werden.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Serverrolle **sysadmin** können **sp_can_tlog_be_applied** ausführen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird eine lokale Variable, `@MyBitVar`, zum Speichern des Ergebnisses deklariert.  
  
```  
USE master;  
GO  
DECLARE @MyBitVar BIT;  
EXEC sp_can_tlog_be_applied  
     @backup_file_name =   
N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\AdventureWorks2012.bak',  
     @database_name = N'AdventureWorks2012',  
     @result = @MyBitVar OUTPUT;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
