---
description: HAS_DBACCESS (Transact-SQL)
title: HAS_DBACCESS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- HAS_DBACCESS_TSQL
- HAS_DBACCESS
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], verifying
- permissions [SQL Server], user access status
- HAS_DBACCESS
- checking permission status
- verifying permission status
- users [SQL Server], access rights status
- testing permissions
- status information [SQL Server], user access
ms.assetid: 99b43a72-0722-4a7b-a493-bdee1c74c7b9
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7669828743c4afb2859161d651c035778c4ea07a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97422265"
---
# <a name="has_dbaccess-transact-sql"></a>HAS_DBACCESS (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Gibt Informationen über den Zugriff des Benutzers auf die angegebene Datenbank zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
HAS_DBACCESS ( 'database_name' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 '*database_name*'  
 Der Name der Datenbank, für die der Benutzer Zugriffsinformationen wünscht. *database_name* ist **sysname**  
  
## <a name="return-types"></a>Rückgabetypen  
 **int**  
  
## <a name="remarks"></a>Bemerkungen  
 HAS_DBACCESS gibt 1 zurück, wenn der Benutzer Zugriff auf die Datenbank hat, 0, wenn der Benutzer keinen Zugriff auf die Datenbank hat, und NULL, wenn der Datenbankname ungültig ist.  
  
 HAS_DBACCESS gibt 0 zurück, wenn die Datenbank offline oder fehlerverdächtig ist.  
  
 HAS_DBACCESS gibt 0 zurück, wenn sich die Datenbank im Einzelbenutzermodus befindet und die Datenbank von einem anderen Benutzer verwendet wird.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der public-Rolle.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird getestet, ob der aktuelle Benutzer Zugriff auf die `AdventureWorks2012`-Datenbank hat.  
  
```sql  
SELECT HAS_DBACCESS('AdventureWorks2012');  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Im folgenden Beispiel wird getestet, ob der aktuelle Benutzer Zugriff auf die `AdventureWorksPDW2012`-Datenbank hat.  
  
```sql  
SELECT HAS_DBACCESS('AdventureWorksPDW2012');  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md)   
 [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md)  
  
  

