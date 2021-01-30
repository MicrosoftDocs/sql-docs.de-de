---
description: sp_grant_login_to_proxy (Transact-SQL)
title: sp_grant_login_to_proxy (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_grant_login_to_proxy
- sp_grant_login_to_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_grant_login_to_proxy
ms.assetid: 90e1a6d5-a692-4462-a163-4b0709d83150
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: c59f8b19e63f491757386ba5562ea9e0106e92a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159809"
---
# <a name="sp_grant_login_to_proxy-transact-sql"></a>sp_grant_login_to_proxy (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gewährt über einen Sicherheitsprinzipal den Zugriff auf einen Proxy.  

  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
sp_grant_login_to_proxy   
     { [ @login_name = ] 'login_name'   
     | [ @fixed_server_role = ] 'fixed_server_role'   
     | [ @msdb_role = ] 'msdb_role' } ,   
     { [ @proxy_id = ] id | [ @proxy_name = ] 'proxy_name' }  
```  
  
## <a name="arguments"></a>Argumente  
`[ @login_name = ] 'login_name'` Der Anmelde Name, dem der Zugriff gewährt werden soll. *login_name* ist vom Datentyp **nvarchar(256)** und hat den Standardwert NULL. Eine der **\@ login_name**, **\@ fixed_server_role** oder **\@ msdb_role** muss angegeben werden, oder die gespeicherte Prozedur schlägt fehl.  
  
`[ @fixed_server_role = ] 'fixed_server_role'` Die fixierte Server Rolle, für die der Zugriff gewährt werden soll. *fixed_server_role* ist vom Datentyp **nvarchar(256)** und hat den Standardwert NULL. Eine der **\@ login_name**, **\@ fixed_server_role** oder **\@ msdb_role** muss angegeben werden, oder die gespeicherte Prozedur schlägt fehl.  
  
`[ @msdb_role = ] 'msdb_role'` Die Daten Bank Rolle in der **msdb** -Datenbank, für die der Zugriff gewährt werden soll. *msdb_role* ist vom Datentyp **nvarchar(256)** und hat den Standardwert NULL. Eine der **\@ login_name**, **\@ fixed_server_role** oder **\@ msdb_role** muss angegeben werden, oder die gespeicherte Prozedur schlägt fehl.  
  
`[ @proxy_id = ] id` Der Bezeichner für den Proxy, für den der Zugriff gewährt werden soll. *id* ist vom Datentyp **int** und hat den Standardwert NULL. Eine **\@ proxy_id** oder **\@ proxy_name** muss angegeben werden, oder die gespeicherte Prozedur schlägt fehl.  
  
`[ @proxy_name = ] 'proxy_name'` Der Name des Proxys, für den der Zugriff gewährt werden soll. *proxy_name* ist vom Datentyp **nvarchar(256)** und hat den Standardwert NULL. Eine **\@ proxy_id** oder **\@ proxy_name** muss angegeben werden, oder die gespeicherte Prozedur schlägt fehl.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_grant_login_to_proxy** muss von der **msdb** -Datenbank aus ausgeführt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Serverrolle **sysadmin** können **sp_grant_login_to_proxy** ausführen.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird dem Anmeldenamen `adventure-works\terrid` die Verwendung des Proxys `Catalog application proxy`ermöglicht.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_grant_login_to_proxy  
    @login_name = N'adventure-works\terrid',  
    @proxy_name = N'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_add_proxy &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
