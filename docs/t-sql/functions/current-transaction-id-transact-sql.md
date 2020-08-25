---
description: CURRENT_TRANSACTION_ID (Transact-SQL)
title: CURRENT_TRANSACTION_ID (Transact-SQL)
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CURRENT_TRANSACTION_ID
- CURRENT_TRANSACTION_ID_TSQL
- sys.CURRENT_TRANSACTION_ID
- sys.CURRENT_TRANSACTION_ID_TSQL
helpviewer_keywords:
- CURRENT_TRANSACTION_ID function
ms.assetid: 82cd9f92-d935-45a0-a433-620d6e15b467
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 80ef65ccfbd26b74c31ed36adc95730315d2943a
ms.sourcegitcommit: 331b8495e4ab37266945c81ff5b93d250bdaa6da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88645964"
---
# <a name="current_transaction_id-transact-sql"></a>CURRENT_TRANSACTION_ID (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Diese Funktion gibt die Transaktions-ID der aktuellen Transaktion in der aktuellen Sitzung zurück.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CURRENT_TRANSACTION_ID( )  
  
```  

## <a name="return-types"></a>Rückgabetypen

**bigint**
  
## <a name="return-value"></a>Rückgabewert  
Die Transaktions-ID der aktuellen Transaktion in der aktuellen Sitzung, die aus [sys.dm_tran_current_transaction &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-current-transaction-transact-sql.md) entnommen wurde.
  
## <a name="permissions"></a>Berechtigungen  
Jeder Benutzer kann die Transaktions-ID der aktuellen Sitzung zurückgeben.
  
## <a name="examples"></a>Beispiele  
In diesem Beispiel wird die Transaktions-ID der aktuellen Sitzung zurückgegeben:
  
```sql
SELECT CURRENT_TRANSACTION_ID();  
```  
  
## <a name="see-also"></a>Weitere Informationen
[sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)  
[SESSION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md)  
[Sicherheit auf Zeilenebene](../../relational-databases/security/row-level-security.md)  
[CONTEXT_INFO  &#40;Transact-SQL&#41;](../../t-sql/functions/context-info-transact-sql.md)  
[SET CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/statements/set-context-info-transact-sql.md)
  
  
