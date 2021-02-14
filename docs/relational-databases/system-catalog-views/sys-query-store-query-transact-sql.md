---
description: sys.query_store_query (Transact-SQL)
title: sys.query_store_query (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- QUERY_STORE_QUERY
- SYS.QUERY_STORE_QUERY_TSQL
- SYS.QUERY_STORE_QUERY
- QUERY_STORE_QUERY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- query_store_query catalog view
- sys.query_store_query catalog view
ms.assetid: bdee149e-7556-4fc3-8242-925dd4b7b6ac
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 92634254e0931b86eeb5c30cfec5fdaa013699a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347995"
---
# <a name="sysquery_store_query-transact-sql"></a>sys.query_store_query (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Enthält Informationen über die Abfrage und die zugeordnete allgemeine allgemeine Lauf Zeit Ausführungs Statistik.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**query_id**|**bigint**|Der Primärschlüssel.|  
|**query_text_id**|**bigint**|Fremdschlüssel. Joins mit [sys.query_store_query_text &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)|  
|**context_settings_id**|**bigint**|Fremdschlüssel. Joins mit [sys.query_context_settings &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md).<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**object_id**|**bigint**|ID des Datenbankobjekts, zu dem die Abfrage gehört (gespeicherte Prozedur, Auslösung, CLR-UDF/UDAgg usw.). 0, wenn die Abfrage nicht als Teil eines Datenbankobjekts (Ad-hoc-Abfrage) ausgeführt wird.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**batch_sql_handle**|**varbinary(64)**|ID des Anweisungs Batches, zu dem die Abfrage gehört. Wird nur aufgefüllt, wenn die Abfrage auf temporäre Tabellen oder Tabellen Variablen verweist.<br/>**Hinweis:** Azure Synapse Analytics gibt immer *null* zurück.|  
|**query_hash**|**Binär (8)**|MD5-Hash der einzelnen Abfrage, basierend auf der logischen Abfrage Struktur. Enthält Optimierungs Hinweise.|  
|**is_internal_query**|**bit**|Die Abfrage wurde intern generiert.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**query_parameterization_type**|**tinyint**|Art der Parametrisierung:<br /><br /> 0 – Keine<br /><br /> 1-Benutzer<br /><br /> 2: einfach<br /><br /> 3-erzwungene<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**query_parameterization_type_desc**|**nvarchar(60)**|Die Textbeschreibung für den parameterisierungstyp.<br/>**Hinweis:** Azure Synapse Analytics gibt immer *keine* zurück.|  
|**initial_compile_start_time**|**datetimeoffset**|Startzeit der Kompilierung.|  
|**last_compile_start_time**|**datetimeoffset**|Startzeit der Kompilierung.|  
|**last_execution_time**|**datetimeoffset**|Die letzte Ausführungszeit bezieht sich auf die letzte Endzeit der Abfrage/des Plans.|  
|**last_compile_batch_sql_handle**|**varbinary(64)**|Handle des letzten SQL-Batches, in dem die Abfrage zuletzt verwendet wurde. Sie kann als Eingabe für [sys.dm_exec_sql_text &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) bereitgestellt werden, um den vollständigen Text des Batches zu erhalten.<br/>**Hinweis:** Azure Synapse Analytics gibt immer *null* zurück.|  
|**last_compile_batch_offset_start**|**bigint**|Informationen, die sys.dm_exec_sql_text zusammen mit last_compile_batch_sql_handle bereitgestellt werden können.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|
|**last_compile_batch_offset_end**|**bigint**|Informationen, die sys.dm_exec_sql_text zusammen mit last_compile_batch_sql_handle bereitgestellt werden können.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**count_compiles**|**bigint**|Kompilierungs Statistik.<br/>**Hinweis:** Azure Synapse Analytics gibt immer eins (1) zurück.|  
|**avg_compile_duration**|**float**|Kompilierungs Statistiken in Mikrosekunden.|  
|**last_compile_duration**|**bigint**|Kompilierungs Statistiken in Mikrosekunden.|  
|**avg_bind_duration**|**float**|Bindungs Statistiken in Mikrosekunden.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**last_bind_duration**|**bigint**|Bindungs Statistiken.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**avg_bind_cpu_time**|**float**|Bindungs Statistiken.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**last_bind_cpu_time**|**bigint**|Bindungs Statistiken.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|  
|**avg_optimize_duration**|**float**|Optimierungs Statistik in Mikrosekunden.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|
|**last_optimize_duration**|**bigint**|Optimierungs Statistik.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|
|**avg_optimize_cpu_time**|**float**|Optimierungs Statistik in Mikrosekunden.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|
|**last_optimize_cpu_time**|**bigint**|Optimierungs Statistik.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|
|**avg_compile_memory_kb**|**float**|Arbeitsspeicher Statistiken kompilieren.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|
|**last_compile_memory_kb**|**bigint**|Arbeitsspeicher Statistiken kompilieren.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|
|**max_compile_memory_kb**|**bigint**|Arbeitsspeicher Statistiken kompilieren.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|
|**is_clouddb_internal_query**|**bit**|Immer 0 in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] der lokalen Umgebung.<br/>**Hinweis:** Azure Synapse Analytics gibt immer 0 (null) zurück.|
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die **View Database State** -Berechtigung.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.database_query_store_options &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.query_context_settings &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query_text &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_runtime_stats_interval &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Überwachen der Leistung mit dem Abfragespeicher](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Gespeicherte Prozeduren für den Abfragespeicher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)  
  
  
