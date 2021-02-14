---
description: sys.dm_tran_version_store_space_usage (Transact-SQL)
title: sys.dm_tran_version_store_space_usage (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_version_store_space_usage_TSQL
- sys.dm_tran_version_store_space_usage
- dm_tran_version_store_space_usage
- dm_tran_version_store_space_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_version_store_space_usage dynamic management view
ms.assetid: 7ab44517-0351-4f91-bdd9-7cf940f03c51
author: savjani
ms.author: pariks
manager: ajayj
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: adea8474945b0f4e6ebd317f44a9b6367181ebad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079231"
---
# <a name="sysdm_tran_version_store_space_usage-transact-sql"></a>sys.dm_tran_version_store_space_usage (Transact-SQL)
[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]

Gibt eine Tabelle zurück, die den gesamten Speicherplatz in tempdb anzeigt, der von Versionsspeicher Datensätzen für jede Datenbank verwendet wird. die Ausführung **sys.dm_tran_version_store_space_usage** ist effizient und nicht aufwendig, da Sie nicht durch einzelne Versionsspeicher Datensätze navigiert und den aggregierten Versionsspeicher Bereich zurückgibt, der in tempdb pro Datenbank verbraucht ist.
  
Jeder Versionsdaten Satz wird als Binärdaten und einige Überwachungs-oder Statusinformationen gespeichert. Ähnlich wie Datensätze in Datenbanktabellen werden die Versionsspeicherdatensätze in 8192 Bytes umfassenden Seiten gespeichert. Falls ein Datensatz größer ist als 8192 Bytes, wird er in zwei unterschiedliche Datensätze geteilt.  
  
Da der Versionsdatensatz als Binärdaten gespeichert wird, treten keine Probleme mit unterschiedlichen Sortierungen aus unterschiedlichen Datenbanken auf. Verwenden Sie **sys.dm_tran_version_store_space_usage** zum Überwachen und Planen der tempdb-Größe basierend auf der Versionsspeicher-Speicherplatz Verwendung von Datenbanken in einer SQL Server Instanz.
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Datenbank-ID der Datenbank.|  
|**reserved_page_count**|**bigint**|Gesamtanzahl der Seiten, die in tempdb für Versionsspeicher Datensätze der Datenbank reserviert sind.|  
|**reserved_space_kb**|**bigint**|Gesamter verwendeter Speicherplatz in Kilobyte in tempdb für Versionsspeicher Datensätze der Datenbank.|  
  
## <a name="permissions"></a>Berechtigungen  
In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   

## <a name="examples"></a>Beispiele  
Die folgende Abfrage kann verwendet werden, um den in tempdb verbrauchten Speicherplatz im Versionsspeicher der einzelnen Datenbanken einer-Instanz zu ermitteln [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . 
  
```sql  
SELECT 
  DB_NAME(database_id) as 'Database Name',
  reserved_page_count,
  reserved_space_kb 
FROM sys.dm_tran_version_store_space_usage;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Database Name            reserved_page_count reserved_space_kb  
------------------------ -------------------- -----------  
msdb                      0                    0             
AdventureWorks2016        10                   80             
AdventureWorks2016DW      0                    0             
WideWorldImporters        20                   160             
```
 
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungssichten und Funktionen in Verbindung mit Transaktionen &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
