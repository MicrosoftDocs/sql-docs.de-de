---
description: sys.query_context_settings (Transact-SQL)
title: sys.query_context_settings (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/29/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- QUERY_CONTEXT_SETTINGS_TSQL
- SYS.QUERY_CONTEXT_SETTINGS_TSQL
- SYS.QUERY_CONTEXT_SETTINGS
- QUERY_CONTEXT_SETTINGS
dev_langs:
- TSQL
helpviewer_keywords:
- sys.query_context_settings catalog view
ms.assetid: 3c1887df-6bd8-491e-82fc-d25ad9589faf
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 63410c44833d6ce22ac3e1ddc2772f4edb7c5ac4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337018"
---
# <a name="sysquery_context_settings-transact-sql"></a>sys.query_context_settings (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Enthält Informationen über die Semantik, die die einer Abfrage zugeordneten Kontext Einstellungen beeinflusst. In stehen eine Reihe von Kontext Einstellungen zur Verfügung [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , die die Abfrage Semantik beeinflussen (das richtige Ergebnis der Abfrage wird definiert). Derselbe Abfragetext, der unter verschiedenen Einstellungen kompiliert wird, kann zu unterschiedlichen Ergebnissen führen (abhängig von den zugrunde liegenden Daten).  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**context_settings_id**|**bigint**|Der Primärschlüssel. Dieser Wert wird in Showplan XML for queries verfügbar gemacht.|  
|**set_options**|**varbinary(8)**|Bitmaske, die den Zustand mehrerer SET-Optionen reflektiert. Weitere Informationen finden Sie unter [sys.dm_exec_plan_attributes &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md).|  
|**language_id**|**smallint**|Die ID der Sprache. Weitere Informationen finden Sie unter [sys.sysSprachen &#40;Transact-SQL-&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md).|  
|**date_format**|**smallint**|Das Datumsformat, Weitere Informationen finden Sie unter [SET DATEFORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/set-dateformat-transact-sql.md).|  
|**date_first**|**tinyint**|Der erste Wert für Date. Weitere Informationen finden Sie unter [SET DATEFIRST &#40;Transact-SQL&#41;](../../t-sql/statements/set-datefirst-transact-sql.md).|  
|**status**|**varbinary (2)**|Bitmasken Feld, das den Typ der Abfrage oder des Kontexts angibt, in der die Abfrage ausgeführt wurde. <br />Der Spaltenwert kann eine Kombination mehrerer Flags sein (ausgedrückt als hexadezimal):<br /><br /> 0x0-reguläre Abfrage (keine spezifischen Flags)<br /><br /> 0x1-Abfrage, die über eine der gespeicherten Prozeduren der Cursor-APIs ausgeführt wurde<br /><br /> 0x2-Abfrage für Benachrichtigung<br /><br /> 0x4-interne Abfrage<br /><br /> 0x8-automatische parametrisierte Abfrage ohne universelle Parametrisierung<br /><br /> 0x10-Cursor Abfrage zum Abrufen der Aktualisierung<br /><br /> 0x20-Abfrage, die in Cursor Aktualisierungs Anforderungen verwendet wird<br /><br /> 0x40-ursprüngliches Resultset wird zurückgegeben, wenn ein Cursor geöffnet wird (Automatisches Abrufen von Cursor)<br /><br /> 0x80-verschlüsselte Abfrage<br /><br /> 0x100-Abfrage im Kontext des Sicherheits Prädikats auf Zeilenebene|  
|**required_cursor_options**|**int**|Vom Benutzer angegebene Cursoroptionen, z. B. der Cursortyp.|  
|**acceptable_cursor_options**|**int**|Cursoroptionen, in die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine implizite Konvertierung vorgenommen werden kann, um die Ausführung der Anweisung zu unterstützen.|  
|**merge_action_type**|**smallint**|Der Typ des Triggerausführungsplans, der als Ergebnis einer **Merge** -Anweisung verwendet wird.<br /><br /> 0 gibt einen nicht-Triggerplan, einen Triggerplan an, der nicht als Ergebnis einer **Merge** -Anweisung ausgeführt wird, oder einen Triggerplan, der als Ergebnis einer **Merge** -Anweisung ausgeführt wird, die nur eine **Delete** -Aktion angibt.<br /><br /> 1 gibt einen **Insert** -Triggerplan an, der als Ergebnis einer **Merge** -Anweisung ausgeführt wird.<br /><br /> 2 gibt einen **Update** -Triggerplan an, der als Ergebnis einer **Merge** -Anweisung ausgeführt wird.<br /><br /> 3 gibt einen **Delete** -Triggerplan an, der als Ergebnis einer **Merge** -Anweisung ausgeführt wird, die eine entsprechende **Insert** -oder **Update** -Aktion enthält.<br /><br /> <br /><br /> Bei geschachtelte-Triggern, die von kaskadierenden Aktionen ausgeführt werden, ist dieser Wert die Aktion der **Merge** -Anweisung, die die Cascade ausgelöst hat.|  
|**default_schema_id**|**int**|ID des Standard Schemas, das verwendet wird, um Namen aufzulösen, die nicht voll qualifiziert sind.|  
|**is_replication_specific**|**bit**|Wird für die Replikation verwendet.|  
|**is_contained**|**varbinary (1)**|1 gibt eine eigenständige Datenbank an.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die **View Database State** -Berechtigung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.database_query_store_options &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)   
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Überwachen der Leistung mit dem Abfragespeicher](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Gespeicherte Prozeduren für den Abfragespeicher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)  
  
  
