---
description: ALTER APPLICATION ROLE (Transact-SQL)
title: ALTER APPLICATION ROLE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_APPLICATION_ROLE_TSQL
- ALTER APPLICATION ROLE
dev_langs:
- TSQL
helpviewer_keywords:
- modifying application roles
- passwords [SQL Server], application roles
- ALTER APPLICATION ROLE statement
- application roles [SQL Server], modifying
ms.assetid: c6cd5d0f-18f4-49be-b161-64d9c5569086
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c20b880e586572cb7f4ac3687663e6f7c2ba012e
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88488077"
---
# <a name="alter-application-role-transact-sql"></a>ALTER APPLICATION ROLE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Ändert den Namen, das Kennwort oder das Standardschema einer Anwendungsrolle.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax
  
```syntaxsql
  
ALTER APPLICATION ROLE application_role_name
    WITH <set_item> [ ,...n ]  
  
<set_item> ::=
    NAME = new_application_role_name
    | PASSWORD = 'password'  
    | DEFAULT_SCHEMA = schema_name  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

 *application_role_name*  
 Der Name der zu ändernden Anwendungsrolle.  
  
 NAME = *new_application_role_name*  
 Gibt den neuen Namen der Anwendungsrolle an. Dieser Name darf nicht bereits als Verweis auf einen Prinzipal in der Datenbank verwendet werden.  
  
 PASSWORD ='*password*'  
 Gibt das Kennwort für die Anwendungsrolle an. *password* muss den Anforderungen der Windows-Kennwortrichtlinien des Computers entsprechen, auf dem die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird. Es sollten immer sichere Kennwörter verwendet werden.  
  
 DEFAULT_SCHEMA = *schema_name*  
 Gibt das erste Schema an, das vom Server beim Auflösen der Namen von Objekten durchsucht wird. *schema_name* kann ein Schema sein, das in der Datenbank nicht vorhanden ist.  
  
## <a name="remarks"></a>Bemerkungen

Falls der neue Anwendungsrollenname bereits in der Datenbank vorhanden ist, wird für die Anweisung ein Fehler gemeldet. Wenn der Name, das Kennwort oder das Standardschema einer Anwendungsrolle geändert wird, wird die der Rolle zugeordnete ID nicht geändert.  
  
> [!IMPORTANT]  
>  Die Richtlinie für das Ablaufen von Kennwörtern wird nicht auf Kennwörter von Anwendungsrollen angewendet. Wählen Sie deshalb unbedingt sichere Kennwörter. Anwendungen, die Anwendungsrollen aufrufen, müssen ihre Kennwörter speichern.  
  
 Anwendungsrollen werden in der sys.database_principals-Katalogsicht angezeigt.  
  
> [!CAUTION]  
>  Das Verhalten der Schemas in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] unterscheidet sich von dem in früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Code, in dem vorausgesetzt wird, dass Schemas Datenbankbenutzern entsprechen, gibt möglicherweise nicht die richtigen Ergebnisse zurück. Alte Katalogsichten, einschließlich „sysobjects“, sollten nicht in einer Datenbank verwendet werden, in der bereits irgendeine der folgenden DDL-Anweisungen verwendet wurde: CREATE SCHEMA, ALTER SCHEMA, DROP SCHEMA, CREATE USER, ALTER USER, DROP USER, CREATE ROLE, ALTER ROLE, DROP ROLE, CREATE APPROLE, ALTER APPROLE, DROP APPROLE, ALTER AUTHORIZATION. In einer Datenbank, in der jemals eine dieser Anweisungen verwendet wurde, müssen Sie die neuen Katalogsichten verwenden. In den neuen Katalogsichten wird die Trennung zwischen Prinzipalen und Schemas berücksichtigt, die in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] eingeführt wird. Weitere Informationen zu Katalogansichten finden Sie unter [Catalog Views &#40;Transact-SQL&#41; (Katalogansichten (Transact-SQL))](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER ANY APPLICATION ROLE-Berechtigung in der Datenbank. Zum Ändern des Standardschemas benötigt der Benutzer auch die ALTER-Berechtigung für die Anwendungsrolle. Eine Anwendungsrolle kann ihr eigenes Standardschema ändern, jedoch nicht den Namen oder das Kennwort.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-changing-the-name-of-application-role"></a>A. Ändern des Namens der Anwendungsrolle  
 Im folgenden Beispiel wird der Name der `weekly_receipts`-Anwendungsrolle in `receipts_ledger` geändert.  
  
```sql  
USE AdventureWorks2012;  
CREATE APPLICATION ROLE weekly_receipts   
    WITH PASSWORD = '987Gbv8$76sPYY5m23' ,   
    DEFAULT_SCHEMA = Sales;  
GO  
ALTER APPLICATION ROLE weekly_receipts   
    WITH NAME = receipts_ledger;  
GO  
```  
  
### <a name="b-changing-the-password-of-application-role"></a>B. Ändern des Kennworts der Anwendungsrolle  
 Im folgenden Beispiel wird das Kennwort der `receipts_ledger`-Anwendungsrolle geändert.  
  
```sql  
ALTER APPLICATION ROLE receipts_ledger   
    WITH PASSWORD = '897yUUbv867y$200nk2i';  
GO  
```  
  
### <a name="c-changing-the-name-password-and-default-schema"></a>C. Ändern des Namens, des Kennworts und des Standardschemas  
 Im folgenden Beispiel werden der Name, das Kennwort und das Standardschema der `receipts_ledger`-Anwendungsrolle gleichzeitig geändert.  
  
```sql  
ALTER APPLICATION ROLE receipts_ledger   
    WITH NAME = weekly_ledger,   
    PASSWORD = '897yUUbv77bsrEE00nk2i',   
    DEFAULT_SCHEMA = Production;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Anwendungsrollen](../../relational-databases/security/authentication-access/application-roles.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [DROP APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-application-role-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
