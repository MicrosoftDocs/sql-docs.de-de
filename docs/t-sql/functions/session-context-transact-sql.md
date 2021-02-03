---
description: SESSION_CONTEXT (Transact-SQL)
title: SESSION_CONTEXT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SESSION_CONTEXT
- sys.SESSION_CONTEXT
- SESSION_CONTEXT_TSQL
- sys.SESSION_CONTEXT_TSQL
helpviewer_keywords:
- SESSION_CONTEXT function
ms.assetid: b6bdbc54-331a-43cc-ab3d-3872d6a12100
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e2d7985bcfacc880a5c1561faa160fb4ec7aee42
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212221"
---
# <a name="session_context-transact-sql"></a>SESSION_CONTEXT (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Gibt den Wert des angegebenen Schlüssels im aktuellen Sitzungskontext zurück. Der Wert wird mithilfe der [sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)-Prozedur festgelegt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
SESSION_CONTEXT(N'key')  
```  
  
## <a name="arguments"></a>Argumente
 'key'  
 Der Schlüssel (vom Typ „sysname“) des Werts, der abgerufen wird.  
  
## <a name="return-type"></a>Rückgabetyp  
 **sql_variant**  
  
## <a name="return-value"></a>Rückgabewert  
 Der Wert, der dem angegebenen Schlüssel im Sitzungskontext zugeordnet ist oder NULL, wenn für diesen Schlüssel kein Wert festgelegt wurde.  
  
## <a name="permissions"></a>Berechtigungen  
 Jeder Benutzer kann den Sitzungskontext für seine Sitzung lesen.  
  
## <a name="remarks"></a>Bemerkungen  
 Das MARS-Verhalten von SESSION_CONTEXT ist dem von CONTEXT_INFO ähnlich. Wenn ein MARS-Batch ein Schlüssel-Wert-Paar festlegt, wird der neue Wert nicht in anderen MARS-Batches auf derselben Verbindung zurückgegeben, es sei denn, sie beginnen nach dem Batch, der den neuen Wert festgelegt hat. Wenn MARS-Batches auf einer Verbindung aktiv sind, können Werte nicht als „read_only“ festgelegt werden. Dadurch werden Racebedingungen und nicht deterministische Funktionen darüber vermieden, welcher Wert „gewinnt“.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden, einfachen Beispiel wird der Sitzungkontextwert für den Schlüssel `user_id` auf 4 festgelegt und der Wert dann mithilfe der **SESSION_CONTEXT**-Funktion abgerufen.  
  
```sql  
EXEC sp_set_session_context 'user_id', 4;  
SELECT SESSION_CONTEXT(N'user_id');  
```  
  
## <a name="see-also"></a>Siehe auch  
 [sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)   
 [CURRENT_TRANSACTION_ID &#40;Transact-SQL&#41;](../../t-sql/functions/current-transaction-id-transact-sql.md)   
 [Sicherheit auf Zeilenebene](../../relational-databases/security/row-level-security.md)   
 [CONTEXT_INFO  &#40;Transact-SQL&#41;](../../t-sql/functions/context-info-transact-sql.md)   
 [SET CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/statements/set-context-info-transact-sql.md)  
  
  
