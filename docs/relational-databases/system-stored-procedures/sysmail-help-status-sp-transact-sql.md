---
description: sysmail_help_status_sp (Transact-SQL)
title: sysmail_help_status_sp (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_status_sp
- sysmail_help_status_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_status_sp
ms.assetid: b44277c6-81e8-4b4d-85b3-a2f04d602e7a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c60340af17a1faf439519416ac862fb792115a5e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181917"
---
# <a name="sysmail_help_status_sp-transact-sql"></a>sysmail_help_status_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Zeigt den Status der Datenbank-E-Mail-Warteschlangen an. Verwenden Sie **sysmail_start_sp** , um die Datenbank-E-Mail-Warteschlangen zu starten, und **sysmail_stop_sp** , um sie anzuhalten.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sysmail_help_status_sp  
```  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="result-set"></a>Resultset  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**Status**|**nvarchar (7)**|Der Status der Datenbank-E-Mail. Mögliche Werte sind **STARTED** und **STOPPED**.|  
  
## <a name="permissions"></a>Berechtigungen  
 Standardmäßig können nur Mitglieder der festen Serverrolle **sysadmin** auf diese Prozedur zugreifen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der Status der Datenbank-E-Mail angezeigt.  
  
```  
EXECUTE msdb.dbo.sysmail_help_status_sp ;  
GO  
```  
  
 Resultset:  
  
```  
Status  
-------  
STARTED  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Externes Programm Datenbank-E-Mail](../../relational-databases/database-mail/database-mail-external-program.md)   
 [sysmail_start_sp &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sysmail-start-sp-transact-sql.md)   
 [sysmail_stop_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-stop-sp-transact-sql.md)  
  
  
