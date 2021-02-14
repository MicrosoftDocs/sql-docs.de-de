---
description: sys.internal_partitions (Transact-SQL)
title: sys.internal_partitions (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0262df2b-5ba7-4715-b17b-3d9ce470a38e
author: ronortloff
ms.author: rortloff
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 46eb4bf2855cf6f5f8121d1480776c83703a5fcc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341357"
---
# <a name="sysinternal_partitions-transact-sql"></a>sys.internal_partitions (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Gibt eine Zeile für jedes Rowset zurück, das interne Daten für columnstore--Indizes für Datenträger basierte Tabellen nachverfolgt. Diese Rowsets sind für columnstore--Indizes intern und Nachverfolgen gelöschter Zeilen, Zeilen Gruppen Zuordnungen und Delta Speicher-Zeilen Gruppen. Sie verfolgen Daten für jede Tabellen Partition nach. jede Tabelle verfügt über mindestens eine Partition. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellt jedes Mal, wenn der columnstore--Index neu erstellt wird, die Rowsets neu.   
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|partition_id|**bigint**|Partitions-ID für diese Partition. Sie ist innerhalb einer Datenbank eindeutig.|  
|object_id|**int**|Objekt-ID für die Tabelle, die die Partition enthält.|  
|index_id|**int**|Index-ID für den columnstore--Index, der in der Tabelle definiert ist.<br /><br /> 1 = gruppierter columnstore--Index<br /><br /> 2 = nicht gruppierter columnstore--Index|  
|partition_number|**int**|Die Partitionsnummer.<br /><br /> 1 = erste Partition einer partitionierten Tabelle oder die einzelne Partition einer nicht partitionierten Tabelle.<br /><br /> 2 = zweite Partition, usw.|  
|internal_object_type|**tinyint**|Rowsetobjekte, die interne Daten für den columnstore--Index nachverfolgen.<br /><br /> 2 = COLUMN_STORE_DELETE_BITMAP<br /><br /> 3 = COLUMN_STORE_DELTA_STORE<br /><br /> 4 = COLUMN_STORE_DELETE_BUFFER<br /><br /> 5 = COLUMN_STORE_MAPPING_INDEX|  
|internal_object_type_desc|**nvarchar(60)**|COLUMN_STORE_DELETE_BITMAP: mit diesem Bitmapindex werden Zeilen nachverfolgt, die im columnstore als gelöscht markiert sind. Die Bitmap gilt für jede Zeilen Gruppe, da Partitionen Zeilen in mehreren Zeilen Gruppen aufweisen können. Die Zeilen sind immer noch physisch vorhanden und nehmen im columnstore Speicherplatz in Anspruch.<br /><br /> COLUMN_STORE_DELTA_STORE: speichert Gruppen von Zeilen, die als Zeilen Gruppen bezeichnet werden und nicht in Spalten Speicher komprimiert wurden. Jede Tabellen Partition kann über NULL oder mehr Delta Store-Zeilen Gruppen verfügen.<br /><br /> COLUMN_STORE_DELETE_BUFFER-für die Beibehaltung von Lösch Vorgängen in aktualisierbaren nicht gruppierten columnstore--Indizes. Wenn eine Abfrage eine Zeile aus der zugrunde liegenden rowstore-Tabelle löscht, verfolgt der DELETE-Puffer den Löschvorgang aus dem columnstore. Wenn die Anzahl der gelöschten Zeilen den Wert 1048576 überschreitet, werden Sie wieder mit dem Thread Bitmap by background tupelverschiebungsthread oder einem expliziten reorganisierungs Befehl zusammengeführt.  Zu einem beliebigen Zeitpunkt stellt die Gesamtmenge der DELETE-Bitmap und des Lösch Puffers alle gelöschten Zeilen dar.<br /><br /> COLUMN_STORE_MAPPING_INDEX nur verwendet, wenn der gruppierte columnstore--Index einen sekundären, nicht gruppierten Index aufweist. Dadurch werden nicht gruppierte Index Schlüssel der richtigen Zeilen Gruppen-und Zeilen-ID im columnstore zugeordnet. Sie speichert nur Schlüssel für Zeilen, die in eine andere Zeilen Gruppe verschoben werden. Dies tritt auf, wenn eine Delta-Zeilen Gruppe in den columnstore komprimiert wird und ein Merge-Vorgang Zeilen aus zwei verschiedenen Zeilen Gruppen zusammenführt.|  
|Row_group_id|**int**|ID für die Delta Store-Zeilen Gruppe. Jede Tabellen Partition kann über NULL oder mehr Delta Store-Zeilen Gruppen verfügen.|  
|hobt_id|**bigint**|ID des internen Rowsetobjekt (-Objekt). Dies ist ein guter Schlüssel für den Beitritt zu anderen DMVs, um weitere Informationen zu den physischen Merkmalen des internen Rowsets zu erhalten.|  
|rows|**bigint**|Die ungefähre Anzahl der Zeilen in dieser Partition.|  
|data_compression|**tinyint**|Der Komprimierungs Status für das Rowset:<br /><br /> 0 = NONE<br /><br /> 1 = ROW<br /><br /> 2 = PAGE|  
|data_compression_desc|**nvarchar(60)**|Der Status der Komprimierung für jede Partition. Mögliche Werte für rowstore-Tabellen sind NONE, ROW und PAGE. Mögliche Werte für columnstore-Tabellen sind COLUMNSTORE und COLUMNSTORE_ARCHIVE.|  
|optimize_for_sequential_key|**bit**|1 = Partition verfügt über die aktivierte INSERT-Optimierung der letzten Seite.<br><br>0 = Standardwert. Bei der Partition wurde die INSERT-Optimierung der letzten Seite deaktiviert.|
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Mitgliedschaft in der Rolle `public`. Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="general-remarks"></a>Allgemeine Hinweise  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellt bei jedem Erstellen oder Neuerstellen eines columnstore--Indexes neue interne columnstore--Indizes neu.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-view-all-of-the-internal-rowsets-for-a-table"></a>A. Alle internen Rowsets für eine Tabelle anzeigen  
 In diesem Beispiel werden alle internen columnstore--Rowsets für eine Tabelle zurückgegeben. Sie können auch den hobt_id verwenden, um weitere Informationen zum jeweiligen Rowset zu erhalten.  
  
```sql  
SELECT i.object_id, i.index_id, i.name, p.hobt_id, p.internal_object_type_id, p.internal_object_type_desc  
FROM sys.internal_partitions AS p  
JOIN sys.indexes AS i  
on i.object_id = p.object_id  
WHERE p.object_id = OBJECT_ID ( '<table name' ) ;  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [FAQ: Abfragen des SQL Server-Systemkatalogs](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
