---
description: CREATE ROLE (Transact-SQL)
title: CREATE ROLE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE ROLE
- DATABASE ROLE
- ROLE_TSQL
- DATABASE_ROLE_TSQL
- CREATE_ROLE_TSQL
- CREATE DATABASE ROLE
- ROLE
- CREATE_DATABASE_ROLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database roles [SQL Server], creating
- CREATE DATABASE ROLE statement
- roles [SQL Server], creating
- CREATE ROLE statement
ms.assetid: b0cd54ad-e81d-4d71-acec-8a6d7261ca08
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 63f7f7157afe656f38160b37f99059e47aab9484
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192707"
---
# <a name="create-role-transact-sql"></a>CREATE ROLE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Erstellt eine neue Datenbankrolle in der aktuellen Datenbank.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
CREATE ROLE role_name [ AUTHORIZATION owner_name ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *role_name*  
 Der Name der zu erstellenden Rolle.  
  
 AUTHORIZATION *owner_name*  
 Der Datenbankbenutzer oder die Datenbankrolle, der bzw. die die neue Rolle besitzen soll. Wenn kein Benutzer angegeben wird, besitzt der Benutzer, der CREATE ROLE ausführt, diese Rolle. Der Besitzer der Rolle oder ein beliebiges Mitglied einer besitzenden Rolle kann der Rolle Mitglieder hinzufügen oder Mitglieder aus dieser entfernen.
  
## <a name="remarks"></a>Bemerkungen  
 Rollen sind auf Datenbankebene sicherungsfähige Elemente. Nachdem Sie eine Rolle erstellt haben, konfigurieren Sie die Berechtigungen der Rolle auf Datenbankebene mithilfe von GRANT, DENY und REVOKE. Um einer Datenbankrolle Elemente hinzuzufügen, verwenden Sie [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md). Weitere Informationen finden Sie unter [Rollen auf Datenbankebene](../../relational-databases/security/authentication-access/database-level-roles.md).  
  
 Datenbankrollen werden in den Katalogsichten sys.database_role_members und sys.database_principals angezeigt.  
  
 Informationen zum Entwerfen eines Berechtigungssystems finden Sie unter [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die **CREATE ROLE**-Berechtigung für die Datenbank oder die Mitgliedschaft in der festen Datenbankrolle **db_securityadmin**. Wenn Sie die Option **AUTHORIZATION** verwenden, sind auch die folgenden Berechtigungen erforderlich:  
  
-   Um einer Rolle einen anderen Benutzer als Besitzer zuzuweisen, ist die IMPERSONATE-Berechtigung für diesen Benutzer erforderlich.  
  
-   Um einer Rolle eine andere Rolle als Besitzer zuzuweisen, ist die Mitgliedschaft in der Empfängerrolle oder die ALTER-Berechtigung für diese Rolle erforderlich.  
  
-   Um einer Rolle eine Anwendungsrolle als Besitzer zuzuweisen, ist die ALTER-Berechtigung für diese Anwendungsrolle erforderlich.  
  
## <a name="examples"></a>Beispiele  
In den folgenden Beispielen wird die Datenbank AdventureWorks verwendet.   

### <a name="a-creating-a-database-role-that-is-owned-by-a-database-user"></a>A. Erstellen einer Datenbankrolle, deren Besitzer ein Datenbankbenutzer ist  
 Im folgenden Beispiel wird die `buyers`-Datenbankrolle erstellt, deren Besitzer der Benutzer `BenMiller` ist.  
  
```sql  
CREATE ROLE buyers AUTHORIZATION BenMiller;  
GO  
```  
  
### <a name="b-creating-a-database-role-that-is-owned-by-a-fixed-database-role"></a>B. Erstellen einer Datenbankrolle, deren Besitzer eine feste Datenbankrolle ist  
 Im folgenden Beispiel wird die `auditors`-Datenbankrolle erstellt, deren Besitzer die feste Datenbankrolle `db_securityadmin` ist.  
  
```sql  
CREATE ROLE auditors AUTHORIZATION db_securityadmin;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Prinzipale &#40;Datenbank-Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
 [DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [Erste Schritte mit Berechtigungen für die Datenbank-Engine](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)  
  
  


