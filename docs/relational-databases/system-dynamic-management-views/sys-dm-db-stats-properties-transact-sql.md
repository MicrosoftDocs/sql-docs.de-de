---
description: sys.dm_db_stats_properties (Transact-SQL)
title: sys.dm_db_stats_properties (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_stats_properties_TSQL
- sys.dm_db_stats_properties
- dm_db_stats_properties_TSQL
- dm_db_stats_properties
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_stats_properties
ms.assetid: 8a54889d-e263-4881-9fcb-b1db410a9453
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 03f712bb35f0a24ba563022ce45b01a5f1bf132c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343056"
---
# <a name="sysdm_db_stats_properties-transact-sql"></a>sys.dm_db_stats_properties (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt Statistikeigenschaften für das angegebene Datenbankobjekt (Tabelle oder indizierte Sicht) in der aktuellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank zurück. Informationen zu partitionierten Tabellen finden Sie in der ähnlichen [sys.dm_db_incremental_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md). 
 
## <a name="syntax"></a>Syntax  
  
```  
sys.dm_db_stats_properties (object_id, stats_id)  
```  
  
## <a name="arguments"></a>Argumente  
 *object_id*  
 ID des Objekts in der aktuellen Datenbank, für die Eigenschaften einer der enthaltenen Statistiken angefordert werden. *object_id* ist vom Datentyp **int**.  
  
 *stats_id*  
 ID der Statistik für die angegebene *object_id*. Die Statistik-ID kann aus der dynamischen Verwaltungssicht [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) abgerufen werden. *stats_id* ist **int**.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID des Objekts (Tabelle oder indizierte Sicht), für das die Eigenschaften des Statistikobjekts zurückgegeben werden sollen.|  
|stats_id|**int**|Die ID des Statistikobjekts. Diese ist innerhalb der Tabelle oder indizierten Sicht eindeutig. Weitere Informationen finden Sie unter [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md).|  
|last_updated|**datetime2**|Datum und Uhrzeit der letzten Aktualisierung des Statistikobjekts. Weitere Informationen finden Sie im Abschnitt [Hinweise](#Remarks) dieses Artikels.|  
|rows|**bigint**|Gesamtanzahl der Zeilen in der Tabelle oder indizierten Sicht zum Zeitpunkt der letzten Aktualisierung der Statistik. Wenn die Statistik gefiltert wird oder einem gefilterten Index entspricht, kann die Anzahl der Zeilen geringer als die Anzahl der Zeilen in der Tabelle sein.|  
|rows_sampled|**bigint**|Gesamtzahl der Zeilen, die für die statistischen Berechnungen in die Stichprobe aufgenommen wurden.|  
|steps|**int**|Anzahl der Schritte im Histogramm. Weitere Informationen finden Sie unter [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).|  
|unfiltered_rows|**bigint**|Gesamtanzahl der Zeilen in der Tabelle vor dem Anwenden des Filterausdrucks (für gefilterte Statistiken). Wenn die Statistik nicht gefiltert ist, entspricht „unfiltered_rows“ dem in der rows-Spalte zurückgegebenen Wert.|  
|modification_counter|**bigint**|Gesamtanzahl der Änderungen für die führende Statistikspalte (auf der das Histogramm basiert) seit der letzten Aktualisierung der Statistik.<br /><br /> Speicher optimierte Tabellen: das Starten [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] dieser Spalte enthält: Gesamtzahl der Änderungen für die Tabelle seit der letzten Aktualisierung der Statistik oder der Neuerstellung der Datenbank.|  
|persisted_sample_percent|**float**|Der persistierte Prozentwert für die Stichprobe wird für Aktualisierungen von Statistiken verwendet, die keinen expliziten Prozentwert für die Stichprobenentnahme angibt. Wenn der Wert 0 (null) ist, wird kein persistierter Prozentwert für diese Statistik festgelegt.<br /><br /> **Anwendungsbereich:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU4|  
  
## <a name="remarks"></a><a name="Remarks"></a> Hinweise  
 **sys.dm_db_stats_properties** gibt unter den folgenden Bedingungen ein leeres Rowset zurück:  
  
-   **object_id** oder **stats_id** ist NULL.    
-   Das angegebene Objekt wurde nicht gefunden bzw. entspricht keiner Tabelle bzw. keiner indizierten Sicht.    
-   Die angegebene Statistik-ID entspricht keiner vorhandenen Statistik für die angegebene Objekt-ID.    
-   Der aktuelle Benutzer verfügt nicht über die erforderlichen Berechtigungen zum Anzeigen des Statistikobjekts.  
  
 Dieses Verhalten ermöglicht die sichere Verwendung von **sys.dm_db_stats_properties** , wenn Cross auf Zeilen in Sichten, wie z **. b. sys. Objects** und **sys. stats**, angewendet wird.  
 
Das Aktualisierungsdatum für die Statistiken befindet sich gemeinsam mit dem [Histogramm](../../relational-databases/statistics/statistics.md#histogram) und [Dichtevektor](../../relational-databases/statistics/statistics.md#density) nicht in den Metadaten, sondern im [Statistik-Blobobjekt](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics). Wenn keine Daten zum Generieren von Statistikdaten gelesen werden, wird das Statistik-BLOB nicht erstellt, das Datum ist nicht verfügbar, und die *last_updated* Spalte ist NULL. Dies ist der Fall bei gefilterten Statistiken oder neuen und leeren Tabellen, für die das Prädikat keine Zeilen zurückgibt.
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert, dass der Benutzer über SELECT-Berechtigungen für Statistikspalten verfügt, Besitzer der Tabelle oder Mitglied der festen Serverrolle `sysadmin`, der festen Datenbankrolle `db_owner` oder der festen Datenbankrolle `db_ddladmin` ist.  
  
## <a name="examples"></a>Beispiele  

### <a name="a-simple-example"></a>A. Einfaches Beispiel
Im folgenden Beispiel wird die Statistik für die- `Person.Person` Tabelle in der AdventureWorks-Datenbank zurückgegeben.

```sql
SELECT * FROM sys.dm_db_stats_properties (object_id('Person.Person'), 1);
``` 
  
### <a name="b-returning-all-statistics-properties-for-a-table"></a>B. Zurückgeben aller Statistikeigenschaften für eine Tabelle  
 Im folgenden Beispiel werden die Eigenschaften aller Statistiken zurückgegeben, die für die TEST-Tabelle vorhanden sind.  
  
```sql  
SELECT sp.stats_id, name, filter_definition, last_updated, rows, rows_sampled, steps, unfiltered_rows, modification_counter   
FROM sys.stats AS stat   
CROSS APPLY sys.dm_db_stats_properties(stat.object_id, stat.stats_id) AS sp  
WHERE stat.object_id = object_id('TEST');  
```  
  
### <a name="c-returning-statistics-properties-for-frequently-modified-objects"></a>C. Zurückgeben von Statistikeigenschaften für häufig geänderte Objekte  
 Im folgenden Beispiel werden alle Tabellen, indizierten Sichten und Statistiken, für die die führende Spalte seit dem letzten Statistikupdate mehr als tausend Mal geändert wurde, aus der aktuellen Datenbank zurückgegeben.  
  
```sql  
SELECT obj.name, obj.object_id, stat.name, stat.stats_id, last_updated, modification_counter  
FROM sys.objects AS obj   
INNER JOIN sys.stats AS stat ON stat.object_id = obj.object_id  
CROSS APPLY sys.dm_db_stats_properties(stat.object_id, stat.stats_id) AS sp  
WHERE modification_counter > 1000;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [Objektbezogene dynamische Verwaltungs Sichten und Funktionen &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/object-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
 [sys.dm_db_incremental_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md)  
 [sys.dm_db_stats_histogram (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md) 
  

