---
description: ALTER SERVER ROLE (Transact-SQL)
title: ALTER SERVER ROLE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_SERVER_ROLE_TSQL
- ALTER SERVER ROLE
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER ROLE, ALTER
- ALTER SERVER ROLE statement
ms.assetid: 7a4db7bb-c442-4e12-9a8a-114da5bc7710
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c79b8ad73bc939354d3b9078c9486531ec8b01ec
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126244"
---
# <a name="alter-server-role-transact-sql"></a>ALTER SERVER ROLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md)]

Ändert die Mitgliedschaft einer Serverrolle oder ändert Namen einer benutzerdefinierten Serverrolle. Feste Serverrollen können nicht umbenannt werden.  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server  
  
ALTER SERVER ROLE server_role_name   
{  
    [ ADD MEMBER server_principal ]  
  | [ DROP MEMBER server_principal ]  
  | [ WITH NAME = new_server_role_name ]  
} [ ; ]  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
ALTER SERVER ROLE  server_role_name  ADD MEMBER login;  
  
ALTER SERVER ROLE  server_role_name  DROP MEMBER login;  
```  
  
## <a name="arguments"></a>Argumente  
*server_role_name*  
Der Name der zu ändernden Serverrolle.  
  
ADD MEMBER *server_principal*  
Fügt der Serverrolle den angegebenen Serverprinzipal hinzu. *server_principal* kann eine Anmeldung oder eine benutzerdefinierte Serverrolle sein. *server_principal* darf keine feste Serverrolle, Datenbankrolle oder sa sein.  
  
DROP MEMBER *server_principal*  
Entfernt den angegebenen Serverprinzipal aus der Serverrolle. *server_principal* kann eine Anmeldung oder eine benutzerdefinierte Serverrolle sein. *server_principal* darf keine feste Serverrolle, Datenbankrolle oder sa sein.  
  
WITH NAME **=** _new_server_role_name_  
Gibt den neuen Namen der benutzerdefinierten Serverrolle an. Dieser Name darf nicht bereits auf dem Server vorhanden sein.  
  
## <a name="remarks"></a>Bemerkungen  
Durch das Ändern des Namens einer benutzerdefinierten Serverrolle werden die ID-Nummer, der Besitzer oder Berechtigungen der Rolle nicht geändert.  
  
`ALTER SERVER ROLE` ändert die Rollenmitgliedschaft durch Ersetzen von sp_addsrvrolemember und sp_dropsrvrolemember. Diese gespeicherten Prozeduren sind veraltet.  
  
Sie können Serverrollen anzeigen, indem Sie die `sys.server_role_members`- Katalogsicht und die `sys.server_principals`-Katalogsicht abfragen.  
  
Mit [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md) können Sie den Benutzer einer benutzerdefinierten Serverrolle ändern.  
  
## <a name="permissions"></a>Berechtigungen  
Erfordert eine `ALTER ANY SERVER ROLE`-Berechtigung für den Server, um den Namen einer benutzerdefinierten Serverrolle zu ändern.  
  
**Feste Serverrollen**  
  
Wenn Sie einer festen Serverrolle ein Mitglied hinzufügen möchten, müssen Sie Mitglied dieser festen Serverrolle oder Mitglied der festen Serverrolle `sysadmin` sein.  
  
> [!NOTE]  
>  Die Berechtigungen `CONTROL SERVER` und `ALTER ANY SERVER ROLE` sind nicht ausreichend, um `ALTER SERVER ROLE` für eine feste Serverrolle auszuführen. Außerdem kann die `ALTER`-Berechtigung nicht an eine feste Serverrolle vergeben werden.  
  
**Benutzerdefinierte Serverrollen**  
  
Wenn Sie einer benutzerdefinierten Serverrolle ein Mitglied hinzufügen möchten, müssen Sie Mitglied der festen Serverrolle `sysadmin` sein oder über die Berechtigung `CONTROL SERVER` oder `ALTER ANY SERVER ROLE` verfügen. Andernfalls müssen Sie über die `ALTER`-Berechtigung für diese Rolle verfügen.  
  
> [!NOTE]  
>  Anders als bei festen Serverrollen verfügen Mitglieder einer benutzerdefinierten Serverrolle nicht unbedingt über die Berechtigung, dieser Rolle Mitglieder hinzuzufügen.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-changing-the-name-of-a-server-role"></a>A. Ändern des Namens einer Serverrolle  
Im folgenden Beispiel wird die Serverrolle `Product` erstellt, und anschließend wird der Name der Serverrolle in `Production` geändert.  
  
```sql
CREATE SERVER ROLE Product ;  
ALTER SERVER ROLE Product WITH NAME = Production ;  
GO  
```  
  
### <a name="b-adding-a-domain-account-to-a-server-role"></a>B. Hinzufügen eines Domänenkontos zu einer Serverrolle  
Im folgenden Beispiel wird der benutzerdefinierten Serverrolle `Production` das Domänenkonto `adventure-works\roberto0` hinzugefügt.  
  
```sql
ALTER SERVER ROLE Production ADD MEMBER [adventure-works\roberto0] ;  
```  
  
### <a name="c-adding-a-sql-server-login-to-a-server-role"></a>C. Hinzufügen einer SQL Server-Anmeldung zu einer Serverrolle  
Im folgenden Beispiel wird die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldung `Ted` der festen Serverrolle `diskadmin` hinzugefügt.  
  
```sql
ALTER SERVER ROLE diskadmin ADD MEMBER Ted ;  
GO  
```  
  
### <a name="d-removing-a-domain-account-from-a-server-role"></a>D: Entfernen eines Domänenkontos aus einer Serverrolle  
Im folgenden Beispiel wird das Domänenkonto `adventure-works\roberto0` aus der benutzerdefinierten Serverrolle `Production` entfernt.  
  
```sql
ALTER SERVER ROLE Production DROP MEMBER [adventure-works\roberto0] ;  
```  
  
### <a name="e-removing-a-sql-server-login-from-a-server-role"></a>E. Entfernen einer SQL Server-Anmeldung aus einer Serverrolle  
Im folgenden Beispiel wird die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldung `Ted` aus der festen Serverrolle `diskadmin` entfernt.  
  
```sql
ALTER SERVER ROLE Production DROP MEMBER Ted ;  
GO  
```  
  
### <a name="f-granting-a-login-the-permission-to-add-logins-to-a-user-defined-server-role"></a>F. Gewähren der Berechtigung zum Hinzufügen von Anmeldungen zu einer benutzerdefinierten Serverrolle für eine Anmeldung  
Im folgenden Beispiel wird `Ted` die Berechtigung erteilt, der benutzerdefinierten Serverrolle `Production` weitere Anmeldungen hinzuzufügen.  
  
```sql
GRANT ALTER ON SERVER ROLE::Production TO Ted ;  
GO  
```  
  
### <a name="g-to-view-role-membership"></a>G. Anzeigen der Rollenmitgliedschaft  
Um die Rollenmitgliedschaft anzuzeigen, verwenden Sie die Seite **Serverrolle (Mitglieder)** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], oder führen Sie die folgende Abfrage aus:  
  
```sql
SELECT SRM.role_principal_id, SP.name AS Role_Name,   
SRM.member_principal_id, SP2.name  AS Member_Name  
FROM sys.server_role_members AS SRM  
JOIN sys.server_principals AS SP  
    ON SRM.Role_principal_id = SP.principal_id  
JOIN sys.server_principals AS SP2   
    ON SRM.member_principal_id = SP2.principal_id  
ORDER BY  SP.name,  SP2.name  
```  
  
## <a name="examples-sspdw"></a>Beispiele: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="h-basic-syntax"></a>H. Allgemeine Syntax  
Im folgenden Beispiel wird die Anmeldung `Anna` der Serverrolle `LargeRC` hinzugefügt.  
  
```sql
ALTER SERVER ROLE LargeRC ADD MEMBER Anna;  
```  
  
### <a name="i-remove-a-login-from-a-resource-class"></a>I. Entfernen einer Anmeldung aus einer Ressourcenklasse  
Im folgenden Beispiel wird Annas Mitgliedschaft aus der Serverrolle `LargeRC` gelöscht.  
  
```sql
ALTER SERVER ROLE LargeRC DROP MEMBER Anna;  
```  
  
## <a name="see-also"></a>Siehe auch  
[CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-role-transact-sql.md)   
[DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md)   
[CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
[ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
[DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
[Security Stored Procedures &#40;Transact-SQL&#41; (Gespeicherte Sicherheitsprozeduren (Transact-SQL))](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
[Sicherheitsfunktionen &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
[Prinzipale &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
[sys.server_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)   
[sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  
