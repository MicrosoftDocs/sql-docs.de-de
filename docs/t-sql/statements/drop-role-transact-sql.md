---
description: DROP ROLE (Transact-SQL)
title: DROP ROLE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP ROLE
- DROP_ROLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting roles
- database roles [SQL Server], removing
- removing roles
- DROP ROLE statement
- roles [SQL Server], removing
- dropping roles
ms.assetid: 1f6f13ae-56a2-4ef1-93f5-8e6151b83e1d
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 142684a85ac44ff744c38f1e47ed80b4b61dc07b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350025"
---
# <a name="drop-role-transact-sql"></a>DROP ROLE (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Entfernt eine Rolle aus der Datenbank.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
-- Syntax for SQL Server  
  
DROP ROLE [ IF EXISTS ] role_name  
```  
  

```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

DROP ROLE role_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *IF EXISTS*  
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] bis zur [aktuellen Version](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 Löscht die Rolle nur, wenn diese bereits vorhanden ist.  
  
 *role_name*  
 Gibt die Rolle an, die aus der Datenbank gelöscht werden soll.  
  
## <a name="remarks"></a>Hinweise  
 Rollen, die sicherungsfähige Elemente besitzen, können nicht aus der Datenbank gelöscht werden. Wenn eine Datenbankrolle mit sicherungsfähigen Elementen gelöscht werden soll, müssen Sie zunächst den Besitz dieser sicherungsfähigen Elemente übertragen oder sie aus der Datenbank löschen. Rollen mit Mitgliedern können nicht aus der Datenbank gelöscht werden. Zum Löschen einer Rolle mit Mitgliedern müssen Sie zunächst die Mitglieder der Rolle entfernen.  
  
 Um Mitglieder aus einer Datenbankrolle zu entfernen, verwenden Sie [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md).  
  
 DROP ROLE kann nicht zum Löschen einer festen Datenbankrolle verwendet werden.  
  
 Informationen zur Rollenmitgliedschaft können in der sys.database_role_members-Katalogsicht angezeigt werden.  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
 Verwenden Sie [DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md), um eine Serverrolle zu entfernen.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die **ALTER ANY ROLE**-Berechtigung für die Datenbank, die **CONTROL**-Berechtigung für die Rolle oder die Mitgliedschaft in **db_securityadmin**.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die Datenbankrolle `purchasing` aus der `AdventureWorks2012`-Datenbank entfernt.  
  
```sql  
DROP ROLE purchasing;  
GO  
```  
  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
 [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
 [Prinzipale &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [Sicherheitsfunktionen &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
