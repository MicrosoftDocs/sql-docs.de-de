---
description: sys.tables (Transact-SQL)
title: sys. Tables (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- tables_TSQL
- sys.tables_TSQL
- sys.tables
- tables
dev_langs:
- TSQL
helpviewer_keywords:
- sys.tables catalog view
ms.assetid: 8c42eba1-c19f-4045-ac82-b97a5e994090
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: df49c9e0c270629fdf00a9ac4ab19804e6a3d9c0
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100352922"
---
# <a name="systables-transact-sql"></a>sys.tables (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile für jede Benutzertabelle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zurück.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|\<inherited columns>||Eine Liste der Spalten, die diese Sicht erbt, finden Sie unter [sys. Objects &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|lob_data_space_id|**int**|Ein Wert ungleich 0 (null) ist die ID des Datenbereichs (Dateigruppe oder Partitionsschema), der die LOB-Binärdaten (Large Object) für diese Tabelle enthält. Beispiele für LOB-Datentypen sind **varbinary (max)**, **varchar (max)**, **geography** oder **XML**.<br /><br /> 0 = Die Tabelle enthält keine LOB-Daten.|  
|filestream_data_space_id|**int**|Die ID des Datenbereichs für eine FILESTREAM-Dateigruppe oder ein Partitionsschema, das aus FILESTREAM-Dateigruppen besteht.<br /><br /> Um den Namen einer FILESTREAM-Datei Gruppe zu melden, führen Sie die Abfrage aus `SELECT FILEGROUP_NAME (filestream_data_space_id) FROM sys.tables` .<br /><br /> sys.tables kann zu den folgenden Sichten über filestream_data_space_id = data_space_id verknüpft werden.<br /><br /> -sys. File Groups<br /><br /> -sys.partition_schemes<br /><br /> -sys. Indexes<br /><br /> -sys.allocation_units<br /><br /> -sys.fulltext_catalogs<br /><br /> -sys.data_spaces<br /><br /> -sys.destination_data_spaces<br /><br /> -sys.master_files<br /><br /> -sys.database_files<br /><br /> -Backup File Group (Join on filegroup_id)|  
|max_column_id_used|**int**|Höchste Spalten-ID, die von dieser Tabelle je verwendet wurde|  
|lock_on_bulk_load|**bit**|Die Tabelle wird bei Massenladevorgängen gesperrt. Weitere Informationen finden Sie unter [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|  
|uses_ansi_nulls|**bit**|Beim Erstellen der Tabelle war die Datenbankoption SET ANSI_NULLS auf ON festgelegt.|  
|is_replicated|**bit**|1 = Die Tabelle wird mithilfe der Momentaufnahme- oder der Transaktionsreplikation veröffentlicht.|  
|has_replication_filter|**bit**|1 = Die Tabelle hat einen Replikationsfilter.|  
|is_merge_published|**bit**|1 = Die Tabelle wird mithilfe der Mergereplikation veröffentlicht.|  
|is_sync_tran_subscribed|**bit**|1 = Die Tabelle wird mithilfe eines Abonnements mit sofortigem Update abonniert.|  
|has_unchecked_assembly_data|**bit**|1 = Die Tabelle enthält persistente Daten, die von einer Assembly abhängen, deren Definition bei der letzten ALTER ASSEMBLY-Anweisung geändert wurde. Der Wert wird nach der nächsten erfolgreichen Ausführung von DBCC CHECKDB oder DBCC CHECKTABLE auf 0 zurückgesetzt.|  
|text_in_row_limit|**int**|Die zulässige Höchstzahl der Bytes für Text in Zeilen.<br /><br /> 0 = die Option Text in row ist nicht festgelegt. Weitere Informationen finden Sie unter [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|  
|large_value_types_out_of_row|**bit**|1 = Umfangreiche Werttypen werden außerhalb der Zeile gespeichert. Weitere Informationen finden Sie unter [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md).|  
|is_tracked_by_cdc|**bit**|1 = Tabelle wird für Change Data Capture aktiviert. Weitere Informationen finden Sie unter [sys.sp_cdc_enable_table &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md).|  
|lock_escalation|**tinyint**|Der Wert der LOCK_ESCALATION-Option für die Tabelle:<br /><br /> 0 = TABLE<br /><br /> 1 = DISABLE<br /><br /> 2 = AUTO|  
|lock_escalation_desc|**nvarchar(60)**|Eine Textbeschreibung der lock_escalation-Option für die Tabelle. Mögliche Werte sind TABLE, AUTO und DISABLE.|  
|is_filetable|**bit**|**Gilt für:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher und [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> 1 = Tabelle ist eine FileTable.<br /><br /> Weitere Informationen zu FileTables finden Sie unter [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md).|  
|durability|**tinyint**|**Gilt für:** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher und [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Folgende Werte sind möglich:<br /><br /> 0 = SCHEMA_AND_DATA<br /><br /> 1 = SCHEMA_ONLY<br /><br /> 0 ist der Standardwert.|  
|durability_desc|**nvarchar(60)**|**Gilt für:** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher und [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Folgende Werte sind möglich:<br /><br /> SCHEMA_ONLY<br /><br /> SCHEMA_AND_DATA<br /><br /> Der Wert von SCHEMA_AND_DATA gibt an, dass die Tabelle eine dauerhafte speicherinterne Tabelle ist. SCHEMA_AND_DATA ist der Standardwert für Speicher optimierte Tabellen. Der Wert von SCHEMA_ONLY gibt an, dass die Tabellendaten nicht nach dem Neustart der Datenbank mit speicheroptimierten Objekten beibehalten werden.|  
|is_memory_optimized|**bit**|**Gilt für:** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und höher und [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Folgende Werte sind möglich:<br /><br /> 0 = nicht speicheroptimiert.<br /><br /> 1 = ist speicheroptimiert.<br /><br /> Der Standardwert ist 0 (null).<br /><br /> Speicher optimierte Tabellen sind in-Memory-Benutzer Tabellen, deren Schema auf dem Datenträger gespeichert wird, ähnlich wie andere Benutzer Tabellen. Auf Speicher optimierte Tabellen kann von System intern kompilierten gespeicherten Prozeduren aus zugegriffen werden.|  
|temporal_type|**tinyint**|**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Der numerische Wert, der den Typ der Tabelle darstellt:<br /><br /> 0 = NON_TEMPORAL_TABLE<br /><br /> 1 = HISTORY_TABLE<br /><br /> 2 = SYSTEM_VERSIONED_TEMPORAL_TABLE|  
|temporal_type_desc|**nvarchar(60)**|**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Die Textbeschreibung des Tabellentyps:<br /><br /> NON_TEMPORAL_TABLE<br /><br /> HISTORY_TABLE<br /><br /> SYSTEM_VERSIONED_TEMPORAL_TABLE|  
|history_table_id|**int**|**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].<br /><br /> Wenn temporal_type in (2, 4) object_id der Tabelle zurückgibt, in der Verlaufs Daten verwaltet werden, andernfalls wird NULL zurückgegeben.|  
|is_remote_data_archive_enabled|**bit**|**Gilt für**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]<br /><br /> Gibt an, ob die Tabelle Stretch-aktiviert ist.<br /><br /> 0 = die Tabelle ist nicht Stretch-aktiviert.<br /><br /> 1 = die Tabelle ist Stretch-aktiviert.<br /><br /> Weitere Informationen finden Sie unter [Stretch Database](../../sql-server/stretch-database/stretch-database.md).|  
|is_external|**bit**|**Gilt für**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher, [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)] und [!INCLUDE[sssdwfull](../../includes/sssdwfull-md.md)] .<br /><br /> Gibt an, dass die Tabelle eine externe Tabelle ist.<br /><br /> 0 = die Tabelle ist keine externe Tabelle.<br /><br /> 1 = die Tabelle ist eine externe Tabelle.| 
|history_retention_period|**int**|**Gilt für**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>Der numerische Wert, der die Dauer der Beibehaltungs Dauer für den temporären Verlauf in Einheiten darstellt, die mit history_retention_period_unit angegeben werden |  
|history_retention_period_unit|**int**|**Gilt für**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>Der numerische Wert, der den Typ der Einheit für die Temporale Verlaufs Beibehaltungs Dauer <br /><br />-1: unendlich <br /><br />3: Tag <br /><br />4: Woche <br /><br />5: Monat <br /><br />6: Jahr |  
|history_retention_period_unit_desc|**nvarchar (10)**|**Gilt für**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>Die Textbeschreibung des Typs der Einheit für die Beibehaltungs Dauer des temporalen Verlaufs. <br /><br />INFINITE <br /><br />DAY <br /><br />WEEK <br /><br />MONTH <br /><br />YEAR |  
|is_node|**bit**|**Gilt für**: [!INCLUDE[sssql17-md.md](../../includes/sssql17-md.md)] und [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>1 = Dies ist eine Diagramm Knoten Tabelle. <br /><br />0 = Dies ist keine Diagramm Knoten Tabelle. |  
|is_edge|**bit**|**Gilt für**: [!INCLUDE[sssql17-md.md](../../includes/sssql17-md.md)] und [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]. <br/><br/>1 = Dies ist eine Diagramm Kanten Tabelle. <br /><br />0 = Dies ist keine Diagramm Kanten Tabelle. |  

## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden alle Benutzertabellen zurückgegeben, die keinen Primärschlüssel aufweisen.  
  
```  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,name AS table_name   
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'TableHasPrimaryKey') = 0  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
Das folgende Beispiel zeigt, wie verwandte Temporale Daten verfügbar gemacht werden können.  
   
**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].
  
```  
SELECT T1.object_id, T1.name as TemporalTableName, SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,  
T2.name as HistoryTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,  
T1.temporal_type_desc  
FROM sys.tables T1  
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id  
ORDER BY T1.temporal_type desc  
```  

Im folgenden Beispiel wird gezeigt, wie Informationen zur temporalen Verlaufs Aufbewahrung verfügbar gemacht werden können.  

**Gilt für**: [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)].  
  
```  
SELECT DB.is_temporal_history_retention_enabled, SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema, 
T1.name as TemporalTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema, T2.name as HistoryTableName,
T1.history_retention_period, T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases where name = DB_NAME()) DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2 
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [DBCC CHECKDB &#40;Transact-SQL-&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)   
 [DBCC CHECKTABLE &#40;Transact-SQL-&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)   
 [Abfragen der SQL Server System Katalog-FAQ](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [In-Memory-OLTP &#40;Arbeitsspeicheroptimierung&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
