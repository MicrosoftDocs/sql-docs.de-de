---
description: DROP ENDPOINT (Transact-SQL)
title: DROP ENDPOINT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_ENDPOINT_TSQL
- DROP ENDPOINT
dev_langs:
- TSQL
helpviewer_keywords:
- removing endpoints
- endpoints [SQL Server], removing
- deleting endpoints
- DROP ENDPOINT statement
- dropping endpoints
ms.assetid: 6aca7412-66a5-4fa4-86b2-061512ff2080
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a33cf6c43f2f01e3a897a0491954b98e5739db6e
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89539826"
---
# <a name="drop-endpoint-transact-sql"></a>DROP ENDPOINT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Löscht einen vorhandenen Endpunkt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
DROP ENDPOINT endPointName  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *endPointName*  
 Der Name des Endpunkts, der entfernt werden soll.  
  
## <a name="remarks"></a>Bemerkungen  
 ENDPOINT DDL-Anweisungen können nicht innerhalb einer Benutzertransaktion ausgeführt werden.  
  
## <a name="permissions"></a>Berechtigungen  
 Der Benutzer muss ein Mitglied der festen Serverrolle **sysadmin** sein, er muss der Besitzer des Endpunkts sein, oder ihm muss die CONTROL-Berechtigung für den Endpunkt erteilt worden sein.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird ein zuvor erstellter Endpunkt namens `sql_endpoint` entfernt.  
  
```  
DROP ENDPOINT sql_endpoint;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
