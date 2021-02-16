---
description: DROP USER (Transact-SQL)
title: DROP USER (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP_USER_TSQL
- DROP USER
dev_langs:
- TSQL
helpviewer_keywords:
- dropping users
- DROP USER statement
- deleting users
- database user removal [SQL Server]
- removing users
- users [SQL Server], removing
ms.assetid: d6e0e21a-7568-4321-b6d6-bcfba183a719
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 85ca6cde1db75564dccea10bd4ebd58e174aef79
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354548"
---
# <a name="drop-user-transact-sql"></a>DROP USER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Entfernt einen Benutzer aus der aktuellen Datenbank.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database  
  
DROP USER [ IF EXISTS ] user_name  
```  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP USER user_name  
```  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *IF EXISTS*  
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] bis zur [aktuellen Version](/troubleshoot/sql/general/determine-version-edition-update-level), [!INCLUDE[sssds](../../includes/sssds-md.md)])  
  
 Entfernt den Benutzer nur, wenn dieser bereits vorhanden ist.  
  
 *user_name*  
 Gibt den Namen an, mit dem der Benutzer innerhalb dieser Datenbank identifiziert wird.  
  
## <a name="remarks"></a>Bemerkungen  
 Benutzer, die Besitzer sicherungsfähiger Elemente sind, können nicht aus der Datenbank gelöscht werden. Vor dem Löschen eines Datenbankbenutzers, der sicherungsfähige Elemente besitzt, müssen Sie zuerst den Besitz dieser sicherungsfähigen Elemente löschen oder übertragen.  
  
 Der guest-Benutzer kann nicht gelöscht, aber deaktiviert werden. Zu diesem Zweck heben Sie die CONNECT-Berechtigung auf, indem Sie REVOKE CONNECT FROM GUEST in einer beliebigen Datenbank außer master oder tempdb ausführen.  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER ANY USER-Berechtigung in der Datenbank.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der Datenbankbenutzer `AbolrousHazem` aus der `AdventureWorks2012`-Datenbank entfernt.  
  
```sql  
DROP USER AbolrousHazem;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [ALTER USER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-user-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)