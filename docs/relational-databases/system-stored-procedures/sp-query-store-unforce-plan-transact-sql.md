---
description: sp_query_store_unforce_plan (Transact-SQL)
title: sp_query_store_unforce_plan (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SP_QUERY_STORE_UNFORCE_PLAN_TSQL
- SP_QUERY_STORE_UNFORCE_PLAN
- SYS.SP_QUERY_STORE_UNFORCE_PLAN
- SYS.SP_QUERY_STORE_UNFORCE_PLAN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_query_store_unforce_plan
- sp_query_store_unforce_plan
ms.assetid: a52f91d0-ff1e-46ad-ba36-b32d9623c9ab
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a53c054c80a4e7fc05b50ee9b4d9310dbeb75ab0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185722"
---
# <a name="sp_query_store_unforce_plan-transact-sql"></a>sp_query_store_unforce_plan (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Ermöglicht das Deaktivieren eines zuvor erzwungenen Plans für eine bestimmte Abfrage.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
sp_query_store_unforce_plan [ @query_id = ] query_id , [ @plan_id = ] plan_id [;]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @query_id = ] query_id` Die ID der Abfrage. *query_id* ist vom Datentyp **bigint** und hat keinen Standardwert.  
  
`[ @plan_id = ] plan_id` Die ID des Abfrage Plans, der nicht mehr erzwungen wird. *plan_id* ist vom Datentyp **bigint** und hat keinen Standardwert.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="remarks"></a>Bemerkungen  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die **Alter** -Berechtigung für die Datenbank.
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden Informationen zu den Abfragen im Abfrage Speicher zurückgegeben.  
  
```sql  
SELECT Txt.query_text_id, Txt.query_sql_text, Pl.plan_id, Qry.*  
FROM sys.query_store_plan AS Pl  
JOIN sys.query_store_query AS Qry  
    ON Pl.query_id = Qry.query_id  
JOIN sys.query_store_query_text AS Txt  
    ON Qry.query_text_id = Txt.query_text_id ;  
```  
  
 Nachdem Sie die query_id und plan_id identifiziert haben, deren erzwingen Sie erzwingen möchten, verwenden Sie das folgende Beispiel, um den Plan zu erzwingen.  
  
```sql  
EXEC sp_query_store_unforce_plan 3, 3;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sp_query_store_force_plan &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)   
 [sp_query_store_remove_plan &#40;TransT-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-plan-transct-sql.md)   
 [sp_query_store_remove_query &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-query-transact-sql.md)   
 [sp_query_store_reset_exec_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-query-store-reset-exec-stats-transact-sql.md)   
 [sp_query_store_flush_db &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-query-store-flush-db-transact-sql.md)   
 [Abfragespeicher Katalog Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)   
 [Überwachen der Leistung mithilfe des Abfragespeicher](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     
 [Bewährte Methoden für den Abfragespeicher](../../relational-databases/performance/best-practice-with-the-query-store.md#CheckForced)     
  
