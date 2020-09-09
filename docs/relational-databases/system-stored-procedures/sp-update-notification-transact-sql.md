---
description: sp_update_notification (Transact-SQL)
title: sp_update_notification (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_update_notification_TSQL
- sp_update_notification
dev_langs:
- TSQL
helpviewer_keywords:
- sp_updatenotification
ms.assetid: 3e1c3d40-8c24-46ce-a68e-ce6c6a237fda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 103e697c8aad38fa19769358372b31d89d3b5978
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89549528"
---
# <a name="sp_update_notification-transact-sql"></a>sp_update_notification (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aktualisiert die Benachrichtigungsmethode für eine Warnbenachrichtigung.  

  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_update_notification  
          [@alert_name =] 'alert' ,  
     [@operator_name =] 'operator' ,  
     [@notification_method =] notification  
```  
  
## <a name="arguments"></a>Argumente  
`[ @alert_name = ] 'alert'` Der Name der dieser Benachrichtigung zugeordneten Warnung. *Alert* ist vom **Datentyp vom Datentyp sysname**und hat keinen Standardwert.  
  
`[ @operator_name = ] 'operator'` Der Operator, der benachrichtigt wird, wenn die Warnung auftritt. *Operator* ist vom **Datentyp vom Datentyp sysname**und hat keinen Standardwert.  
  
`[ @notification_method = ] notification` Die Methode, mit der der Operator benachrichtigt wird. die *Benachrichtigung*ist vom Datentyp **tinyint**und hat keinen Standardwert. die folgenden Werte sind möglich:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**1**|E-Mail|  
|**2**|Pager|  
|**4**|**NET SEND**|  
|**7**|Alle Methoden|  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Hinweise  
 **sp_update_notification** müssen von der **msdb** -Datenbank aus ausgeführt werden.  
  
 Sie können eine Benachrichtigung für einen Operator aktualisieren, der nicht über die erforderlichen Adressinformationen verfügt. verwenden Sie dazu den angegebenen *notification_method*. Wenn beim Senden einer E-Mail- oder Pagerbenachrichtigung ein Fehler auftritt, wird der Fehler im Fehlerprotokoll des Microsoft SQL Server-Agents angezeigt.  
  
## <a name="permissions"></a>Berechtigungen  
 Zum Ausführen dieser gespeicherten Prozedur muss den Benutzern die festen Server Rolle **sysadmin** erteilt werden.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die Benachrichtigungs Methode für Benachrichtigungen geändert, die an `François Ajenstat` für die Warnung gesendet werden `Test Alert` .  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_notification  
   @alert_name = N'Test Alert',  
   @operator_name = N'François Ajenstat',  
   @notification_method = 7;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_add_notification &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)   
 [sp_delete_notification &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-delete-notification-transact-sql.md)   
 [sp_help_notification &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-help-notification-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
