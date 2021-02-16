---
description: sys.dm_db_missing_index_columns (Transact-SQL)
title: sys.dm_db_missing_index_columns (Transact-SQL)
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_missing_index_columns_TSQL
- sys.dm_db_missing_index_columns_TSQL
- sys.dm_db_missing_index_columns
- dm_db_missing_index_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_columns dynamic management function
- missing indexes feature [SQL Server], sys.dm_db_missing_index_columns dynamic management function
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ec00a68faedb48e63988d91f6e612c62a705caf3
ms.sourcegitcommit: c6cc0b669b175ae290cf5b08952010661ebd03c3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2021
ms.locfileid: "100530852"
---
# <a name="sysdm_db_missing_index_columns-transact-sql"></a>sys.dm_db_missing_index_columns (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt Informationen zu Spalten von Datenbanktabellen zurück, für die kein Index vorhanden ist, mit Ausnahme von räumlichen Indizes. **sys.dm_db_missing_index_columns** ist eine dynamische Verwaltungsfunktion.  

## <a name="syntax"></a>Syntax  
  
```  
  
sys.dm_db_missing_index_columns(index_handle)  
```  
  
## <a name="arguments"></a>Argumente  
 *index_handle*  
 Eine ganze Zahl, die einen fehlenden Index eindeutig identifiziert. Sie kann aus den folgenden dynamischen Verwaltungsobjekten abgerufen werden:  
  
 [sys.dm_db_missing_index_details &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)  
  
 [sys.dm_db_missing_index_groups &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**column_id**|**int**|ID der Spalte.|  
|**column_name**|**sysname**|Name der Tabellenspalte.|  
|**column_usage**|**varchar (20)**|Art der Verwendung der Spalte durch die Abfrage. Die möglichen Werte und deren Beschreibungen lauten:<br /><br /> Gleichheit: die Spalte trägt zu einem Prädikat bei, das die Gleichheit in der Form ausdrückt: <br />                        *Table. Column*  =  *constant_value*<br /><br /> Ungleichheit: die Spalte trägt zu einem Prädikat bei, das Ungleichheit ausdrückt, z. b. ein Prädikat der Form: *Table. Column*  >  *constant_value*. Jeder Vergleichsoperator außer "=" drückt Ungleichheit aus.<br /><br /> INCLUDE: die Spalte wird nicht zur Auswertung eines Prädikats verwendet, sondern wird aus einem anderen Grund verwendet, z. b. zum Abdecken einer Abfrage.|  
  
## <a name="remarks"></a>Bemerkungen  
 Von **sys.dm_db_missing_index_columns** zurückgegebene Informationen werden aktualisiert, wenn eine Abfrage vom Abfrageoptimierer optimiert wird. Sie werden nicht persistent gespeichert. Informationen zu fehlenden Indizes werden nur bis zum Neustart von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aufbewahrt. Datenbankadministratoren sollten regelmäßig Sicherungskopien der Informationen zu fehlenden Indizes erstellen, wenn Sie sie nach dem Wiederverwenden des Servers beibehalten möchten.  
  
## <a name="transaction-consistency"></a>Transaktionskonsistenz  
 Wenn durch eine Transaktion eine Tabelle erstellt oder gelöscht wird, werden die Zeilen mit Informationen zu fehlenden Indizes bezüglich der gelöschten Objekte aus diesem dynamischen Verwaltungsobjekt entfernt, damit die Transaktionskonsistenz erhalten bleibt.  
  
## <a name="permissions"></a>Berechtigungen  
 Benutzern muss die VIEW SERVER STATE-Berechtigung oder eine Berechtigung, die die VIEW SERVER STATE-Berechtigung impliziert, erteilt werden, damit sie diese dynamische Verwaltungsfunktion abfragen können.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird eine Abfrage für die `Address`-Tabelle und anschließend eine Abfrage mithilfe der dynamischen Verwaltungssicht `sys.dm_db_missing_index_columns` ausgeführt, um die Tabellenspalten zurückzugeben, die keinen Index aufweisen.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT City, StateProvinceID, PostalCode  
FROM Person.Address  
WHERE StateProvinceID = 9;  
GO  
SELECT mig.*, statement AS table_name,  
    column_id, column_name, column_usage  
FROM sys.dm_db_missing_index_details AS mid  
CROSS APPLY sys.dm_db_missing_index_columns (mid.index_handle)  
INNER JOIN sys.dm_db_missing_index_groups AS mig ON mig.index_handle = mid.index_handle  
ORDER BY mig.index_group_handle, mig.index_handle, column_id;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.dm_db_missing_index_details &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)  
 [sys.dm_db_missing_index_group_stats_query &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-query-transact-sql.md)     
  
