---
description: sp_dropmessage (Transact-SQL)
title: sp_dropmessage (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropmessage_TSQL
- sp_dropmessage
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropmessage
ms.assetid: 17287a15-cdde-43d1-bb18-9f920bc15db8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 41971224ec830b1fbc17fdc8f754d7ae99e8731e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197639"
---
# <a name="sp_dropmessage-transact-sql"></a>sp_dropmessage (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Löscht eine angegebene benutzerdefinierte Fehlermeldung aus einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Benutzerdefinierte Meldungen können mithilfe der **sys.messages** -Katalogsicht angezeigt werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_dropmessage [ @msgnum = ] message_number  
    [ , [ @lang = ] 'language' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @msgnum = ] message_number` Die Nachrichten-Nummer, die gelöscht werden soll. *message_number* muss eine benutzerdefinierte Nachricht mit einer Nachrichtennummer größer als 50000 sein. *message_number* ist vom Datentyp **int** und hat den Standardwert NULL.  
  
`[ @lang = ] 'language'` Die Sprache der zu löschenden Nachricht. Wenn **all** angegeben wird, werden alle Sprachversionen von *message_number* gelöscht. *language* ist vom Datentyp **sysname** und hat den Standardwert NULL.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="result-sets"></a>Resultsets  
 Keine.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in den festen Serverrollen **sysadmin** und **serveradmin** .  
  
## <a name="remarks"></a>Bemerkungen  
 Solange nicht **all** für *language* angegeben wird, müssen alle lokalisierten Versionen der Nachricht gelöscht werden, bevor die englische Version (USA) der Nachricht gelöscht werden kann.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-dropping-a-user-defined-message"></a>A. Löschen einer benutzerdefinierten Meldung  
 Im folgenden Beispiel wird eine benutzerdefinierte Meldung (Nummer `50001`) aus **sys.messages** gelöscht.  
  
```  
USE master;  
GO  
EXEC sp_dropmessage 50001;  
```  
  
### <a name="b-dropping-a-user-defined-message-that-includes-a-localized-version"></a>B. Löschen einer benutzerdefinierten Meldung mit einer lokalisierten Version  
 Im folgenden Beispiel wird eine benutzerdefinierte Meldung (Nummer `60000`) gelöscht, die eine lokalisierte Version der Meldung enthält.  
  
```  
USE master;  
GO  
  
-- Create a user-defined message in U.S. English  
EXEC sp_addmessage   
    @msgnum = 60000,  
    @severity = 16,  
    @msgtext = N'The item named %s already exists in %s.',   
    @lang = 'us_english';  
  
-- Create a localized version of the same message.  
EXEC sp_addmessage   
    @msgnum = 60000,  
    @severity = 16,  
    @msgtext = N'L''élément nommé %1! existe déjà dans %2!',  
    @lang = 'French';  
GO  
  
-- This statement will fail as long as the localized version  
-- of the message exists.  
EXEC sp_dropmessage 60000;  
GO  
  
-- This statement will drop the message.  
EXEC sp_dropmessage  
    @msgnum = 60000,  
    @lang = 'all';  
GO  
```  
  
### <a name="c-dropping-a-localized-version-of-a-user-defined-message"></a>C. Löschen der lokalisierten Version einer benutzerdefinierten Meldung  
 Im folgenden Beispiel wird eine lokalisierte Version der benutzerdefinierten Meldung (Nummer `60000`) gelöscht, ohne dass die gesamte Meldung gelöscht wird.  
  
```  
USE master;  
GO  
  
-- Create a user-defined message in U.S. English  
EXEC sp_addmessage   
    @msgnum = 60000,  
    @severity = 16,  
    @msgtext = N'The item named %s already exists in %s.',   
    @lang = 'us_english';  
  
-- Create a localized version of the same message.  
EXEC sp_addmessage   
    @msgnum = 60000,  
    @severity = 16,  
    @msgtext = N'L''élément nommé %1! existe déjà dans %2!',  
    @lang = 'French';  
GO  
-- This statement will remove only the localized version of the   
-- message.  
EXEC sp_dropmessage  
    @msgnum = 60000,  
    @lang = 'French';  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [sp_addmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sp_altermessage &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-altermessage-transact-sql.md)   
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)   
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
