---
description: sys.column_store_row_groups (Transact-SQL)
title: sys.column_store_row_groups (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/28/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.column_store_row_groups_TSQL
- column_store_row_groups
- sys.column_store_row_groups
- column_store_row_groups_TSQL
- deleted bitmap
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_store_row_groups catalog view
ms.assetid: 76e7fef2-d1a4-4272-a2bb-5f5dcd84aedc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b949ce0f7c32676c1d47125d411af1e22c00bb7a
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465076"
---
# <a name="syscolumn_store_row_groups-transact-sql"></a>sys.column_store_row_groups (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Stellt Informationen zu gruppierten Columnstore-Indizes auf Segmentbasis bereit, um den Administrator bei Fragen zur Systemverwaltung zu unterstützen. **sys.column_store_row_groups** verfügt über eine Spalte für die Gesamtanzahl der physisch gespeicherten Zeilen (einschließlich der als gelöscht markierten Zeilen) und eine Spalte für die Anzahl der Zeilen, die als gelöscht markiert sind. Verwenden Sie **sys.column_store_row_groups** , um zu bestimmen, welche Zeilen Gruppen einen hohen Prozentsatz gelöschter Zeilen aufweisen und neu erstellt werden sollten.  
   
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**object_id**|INT|Die ID der Tabelle, in der dieser Index definiert ist.|  
|**index_id**|INT|ID des Indexes, der diesen columnstore-Index enthält.|  
|**partition_number**|INT|ID der Tabellenpartition, die die row_group_id der Zeilengruppe enthält. Sie können partition_number verwenden, um diese DMV mit sys.partitions zu verknüpfen.|  
|**row_group_id**|INT|Die dieser Zeilengruppe zugeordnete Zeilengruppenzahl. Diese ist innerhalb der Partition eindeutig.<br /><br /> -1 = Ende einer in-Memory-Tabelle.|  
|**delta_store_hobt_id**|BIGINT|Die hobt_id für die geöffnete Zeilen Gruppe im Delta Speicher.<br /><br /> NULL, wenn sich die Zeilen Gruppe nicht im Delta Speicher befindet.<br /><br /> NULL für das Ende einer in-Memory-Tabelle.|  
|**state**|TINYINT|Die der state_description zugeordnete ID.<br /><br /> 0 = INVISIBLE<br /><br /> 1 = OPEN<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED <br /><br /> 4 = Tombstone|  
|**state_description**|nvarchar(60)|Beschreibung des persistenten Status der Zeilengruppe:<br /><br /> Unsichtbar: ein verborgenes komprimiertes Segment, das aus Daten in einem Delta Speicher erstellt wird. Lesevorgänge nutzen den Deltastore, bis das unsichtbare komprimierte Segment abgeschlossen ist. Anschließend werden das neue Segment sichtbar gemacht und der Quelldeltastore entfernt.<br /><br /> Öffnen: eine Zeilen Gruppe mit Lese-/Schreibzugriff, die neue Datensätze akzeptiert. Eine offene Zeilengruppe befindet sich weiterhin im rowstore-Format und wurde nicht in das columnstore-Format komprimiert.<br /><br /> Closed: eine Zeilen Gruppe, die ausgefüllt, aber noch nicht durch den tupelverschiebungsprozess komprimiert wurde.<br /><br /> Komprimiert: eine Zeilen Gruppe, die ausgefüllt und komprimiert wurde.|  
|**total_rows**|BIGINT|Gesamtzahl der Zeilen, die in der Zeilengruppe physisch gespeichert sind. Einige wurden u. U. gelöscht, sind aber weiterhin gespeichert. Die maximale Anzahl der Zeilen in einer Zeilengruppe beträgt 1.048.576 (hexadezimal FFFFF).|  
|**deleted_rows**|BIGINT|Gesamtzahl der Zeilen in der Zeilengruppe, die als gelöscht markiert sind. Dies ist für DELTA-Zeilengruppen immer 0.|  
|**size_in_bytes**|BIGINT|Größe aller Daten (in Bytes) in dieser Zeilengruppe (ausgenommen von Metadaten oder freigegebenen Wörterbüchern) sowohl für DELTA- als auch für COLUMNSTORE-Zeilengruppen.|  
  
## <a name="remarks"></a>Bemerkungen  
 Gibt eine Zeile für jede columnstore-Zeilengruppe für jede Tabelle zurück, die über einen gruppierten oder nicht gruppierten columnstore-Index verfügt.  
  
 Verwenden Sie **sys.column_store_row_groups** , um die Anzahl der Zeilen, die in der Zeilen Gruppe enthalten sind, und die Größe der Zeilen Gruppe zu bestimmen.  
  
 Wenn die Anzahl der gelöschten Zeilen in einer Zeilengruppe auf einen hohen Prozentsatz der Gesamtzeilen ansteigt, wird die Tabelle weniger effizient. Erstellen Sie den columnstore-Index neu, um die Tabellengröße zu verringern und die Datenträger-E/A zu reduzieren, die zum Lesen der Tabelle erforderlich ist. Verwenden Sie die **Rebuild** -Option der **Alter Index** -Anweisung, um den columnstore--Index neu zu erstellen.  
  
 Der aktualisierbare columnstore-fügt zunächst neue Daten in eine **geöffnete** Zeilen Gruppe ein, die im rowstore-Format vorliegt, und wird manchmal auch als Delta Tabelle bezeichnet.  Wenn eine offene Zeilen Gruppe voll ist, ändert sich der Status in " **geschlossen**". Eine geschlossene Zeilen Gruppe wird vom tupelverschiebungsvorgang im columnstore--Format komprimiert, und der Status ändert sich in **komprimiert**.  Die Tupelverschiebungsfunktion ist ein Hintergrundprozess, der regelmäßig aktiv wird und überprüft, ob geschlossene Zeilengruppen vorhanden sind, die in eine columnstore-Zeilengruppe komprimiert werden können.  Die Tupelverschiebungsfunktion gibt außerdem alle Zeilengruppen frei, in denen alle Zeilen gelöscht wurden. Die zugeordneten Zeilen Gruppen werden als **Tombstone** gekennzeichnet. Verwenden Sie die Option **reorganisieren** der **Alter Index** -Anweisung, um den tupelverschiebungsvorgang sofort auszuführen.  
  
 Wenn eine columnstore-Zeilengruppe aufgefüllt wurde, wird sie komprimiert und akzeptiert keine neuen Zeilen mehr. Wenn aus einer komprimierten Gruppe Zeilen gelöscht werden, verbleiben sie zwar, werden aber als gelöscht gekennzeichnet. Updates einer komprimierten Gruppe werden als Löschvorgang für die komprimierte Gruppe und als Einfügevorgang für eine offene Gruppe implementiert.  
  
## <a name="permissions"></a>Berechtigungen  
 Gibt Informationen für eine Tabelle zurück, wenn der Benutzer `VIEW DEFINITION` über die-Berechtigung für die Tabelle verfügt.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die **sys.column_store_row_groups** Tabelle mit anderen Systemtabellen verbunden, um Informationen zu bestimmten Tabellen zurückzugeben. Die berechnete `PercentFull`-Spalte ist eine Schätzung der Effizienz der Zeilengruppe. Wenn Sie Informationen zu einer einzelnen Tabelle suchen möchten, entfernen Sie die Kommentar-Bindestriche vor der **Where** -Klausel, und geben Sie einen Tabellennamen an.  
  
```sql  
SELECT i.object_id, object_name(i.object_id) AS TableName,   
i.name AS IndexName, i.index_id, i.type_desc,   
CSRowGroups.*,   
100*(total_rows - ISNULL(deleted_rows,0))/total_rows AS PercentFull    
FROM sys.indexes AS i  
JOIN sys.column_store_row_groups AS CSRowGroups  
    ON i.object_id = CSRowGroups.object_id  
AND i.index_id = CSRowGroups.index_id   
--WHERE object_name(i.object_id) = '<table_name>'   
ORDER BY object_name(i.object_id), i.name, row_group_id;  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Abfragen der SQL Server System Katalog-FAQ](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.ALL_COLUMNS &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)   
 [Beschreibung von Columnstore-Indizes](~/relational-databases/indexes/columnstore-indexes-overview.md)     
 [sys.column_store_dictionaries &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)   
 [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
  
  

