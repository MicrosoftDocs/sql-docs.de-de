---
description: sp_password (Transact-SQL)
title: sp_password (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_password
- sp_password_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_password
ms.assetid: 0ecbec81-e637-44a9-a61e-11bf060ef084
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f154661d7cd03edfde83dd7774685e73e79aeb69
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195429"
---
# <a name="sp_password-transact-sql"></a>sp_password (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fügt ein Kennwort für einen Anmelde Namen hinzu oder ändert es [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen [Alter Login](../../t-sql/statements/alter-login-transact-sql.md) .  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_password [ [ @old = ] 'old_password' , ]  
     { [ @new =] 'new_password' }  
     [ , [ @loginame = ] 'login' ]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @old = ] 'old_password'` Das alte Kennwort. *OLD_PASSWORD* ist vom **Datentyp vom Datentyp sysname** und hat den Standardwert NULL.  
  
`[ @new = ] 'new_password'` Das neue Kennwort. *new_password* ist vom **Datentyp vom Datentyp sysname** und hat keinen Standardwert. *OLD_PASSWORD* muss angegeben werden, wenn benannte Parameter nicht verwendet werden.  
  
> [!IMPORTANT]  
>  Verwenden Sie kein NULL-Kennwort. Verwenden Sie ein sicheres Kennwort. Weitere Informationen finden Sie unter [Strong Passwords](../../relational-databases/security/strong-passwords.md).  
  
`[ @loginame = ] 'login'` Der Name der Anmeldung, die von der Kenn Wort Änderung betroffen ist. *login* ist vom Datentyp **sysname** und hat den Standardwert NULL. der *Anmelde* Name muss bereits vorhanden sein und kann nur von Mitgliedern der festen Server Rollen **sysadmin** oder **securityadmin** angegeben werden.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_password** ruft Alter Login auf. Diese Anweisung unterstützt weitere Optionen. Weitere Informationen zum Ändern von Kenn Wörtern finden Sie unter [Alter Login &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md).  
  
 **sp_password** kann nicht innerhalb einer benutzerdefinierten Transaktion ausgeführt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER ANY LOGIN-Berechtigung. Darüber hinaus wird die CONTROL SERVER-Berechtigung zum Zurücksetzen eines Kennworts ohne Bereitstellen des alten Kennworts benötigt; diese ist auch erforderlich, wenn der zu ändernde Anmeldename über die CONTROL SERVER-Berechtigung verfügt.  
  
 Ein Prinzipal kann sein eigenes Kennwort ändern.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-changing-the-password-of-a-login-without-knowing-the-old-password"></a>A. Ändern des Kennwortes einer Anmeldung ohne Kenntnis des alten Kennworts  
 Das folgende Beispiel veranschaulicht, wie mit `ALTER LOGIN` das Kennwort für den Anmeldenamen `Victoria` in `B3r1000d#2-36` geändert wird. Dies ist die bevorzugte Methode. Der Benutzer, der diesen Befehl ausführt, benötigt die CONTROL SERVER-Berechtigung.  
  
```  
ALTER LOGIN Victoria WITH PASSWORD = 'B3r1000d#2-36';  
GO  
```  
  
### <a name="b-changing-a-password"></a>B. Ändern eines Kennwortes  
 Das folgende Beispiel veranschaulicht, wie mit `ALTER LOGIN` das Kennwort für den Anmeldenamen `Victoria` von `B3r1000d#2-36` in `V1cteAmanti55imE` geändert wird. Dies ist die bevorzugte Methode. Der Benutzer `Victoria` kann diesen Befehl ohne zusätzliche Berechtigungen ausführen. Andere Benutzer benötigen die ALTER ANY LOGIN-Berechtigung.  
  
```  
ALTER LOGIN Victoria WITH   
     PASSWORD = 'V1cteAmanti55imE'   
     OLD_PASSWORD = 'B3r1000d#2-36';  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Security Stored Procedures &#40;Transact-SQL&#41; (Gespeicherte Sicherheitsprozeduren (Transact-SQL))](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_revokelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
