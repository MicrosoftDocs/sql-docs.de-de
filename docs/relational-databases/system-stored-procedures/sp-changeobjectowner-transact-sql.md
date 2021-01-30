---
description: sp_changeobjectowner (Transact-SQL)
title: sp_changeobjectowner (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_changeobjectowner_TSQL
- sp_changeobjectowner
dev_langs:
- TSQL
helpviewer_keywords:
- sp_changeobjectowner
ms.assetid: 45b3dc1c-1cde-45b7-a248-5195c12973e9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 482fcbdb2d7c1b79eb0a8b5ed88d1295785b994b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159832"
---
# <a name="sp_changeobjectowner-transact-sql"></a>sp_changeobjectowner (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ändert den Besitzer eines Objekts in der aktuellen Datenbank.  
  
> [!IMPORTANT]
>  Diese gespeicherte Prozedur funktioniert nur mit den in verfügbaren Objekten [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] . [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen [Alter Schema](../../t-sql/statements/alter-schema-transact-sql.md) oder [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md) . **sp_changeobjectowner** ändert sowohl das Schema als auch den Besitzer. Aus Gründen der Kompatibilität mit früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ändert diese gespeicherte Prozedur nur Objektbesitzer, wenn sowohl der aktuelle Besitzer als auch der neue Besitzer Schemas besitzen, die den gleichen Namen wie die Datenbankbenutzernamen aufweisen.  
> 
> [!IMPORTANT]
>  Dieser gespeicherten Prozedur wurde eine neue Berechtigungsanforderung hinzugefügt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_changeobjectowner [ @objname = ] 'object' , [ @newowner = ] 'owner'  
```  
  
## <a name="arguments"></a>Argumente  
`[ @objname = ] 'object'` Der Name einer vorhandenen Tabelle, Sicht, benutzerdefinierten Funktion oder gespeicherten Prozedur in der aktuellen Datenbank. *Object* ist vom Datentyp **nvarchar (776)** und hat keinen Standardwert. Das *Objekt* kann mit dem Besitzer des vorhandenen Objekts im Formular _existing_owner_ qualifiziert werden **.** _Objekt_ , wenn das Schema und der zugehörige Besitzer denselben Namen haben.  
  
`[ @newowner = ] 'owner_ '` Der Name des Sicherheits Kontos, das den neuen Besitzer des Objekts sein wird. *Owner* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert. der *Besitzer* muss ein gültiger Datenbankbenutzer, eine Server Rolle, eine [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Anmeldung oder eine Windows-Gruppe mit Zugriff auf die aktuelle Datenbank sein. Wenn es sich beim neuen Besitzer um einen Windows-Benutzer oder eine Windows-Gruppe handelt, für den bzw. die kein entsprechender Datenbankprinzipal vorhanden ist, wird ein Datenbankbenutzer erstellt.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_changeobjectowner** entfernt alle vorhandenen Berechtigungen aus dem-Objekt. Sie müssen alle Berechtigungen, die Sie beibehalten möchten, nach dem Ausführen **sp_changeobjectowner** erneut anwenden. Daher wird empfohlen, vor dem Ausführen **sp_changeobjectowner** Skripts für vorhandene Berechtigungen zu erstellen. Nachdem der Besitz des Objekts geändert wurde, können Sie das Skript verwenden, um die Berechtigungen erneut zuzuweisen. Sie müssen den Objektbesitzer im Berechtigungsskript vor dem Ausführen ändern.  
  
 Wenn Sie den Besitzer eines sicherungsfähigen Elements ändern möchten, verwenden Sie ALTER AUTHORIZATION. Zum Ändern eines Schemas verwenden Sie ALTER SCHEMA.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der **db_owner** Fixed-Daten Bank Rolle oder die Mitgliedschaft in der Daten Bank Rolle **db_ddladmin** und der **db_securityadmin** Fixed-Daten Bank Rolle sowie die CONTROL-Berechtigung für das Objekt.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der Besitzer der `authors`-Tabelle zu `Corporate\GeorgeW` geändert.  
  
```  
EXEC sp_changeobjectowner 'authors', 'Corporate\GeorgeW';  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/alter-schema-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
