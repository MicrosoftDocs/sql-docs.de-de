---
description: sys.index_columns (Transact-SQL)
title: sys.index_columns (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.index_columns
- sys.index_columns_TSQL
- index_columns
- index_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.index_columns catalog view
ms.assetid: 211471aa-558a-475c-9b94-5913c143ed12
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9d0f9ed720df11627d59ecb773f294e12ec98346
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465059"
---
# <a name="sysindex_columns-transact-sql"></a>sys.index_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Enthält eine Zeile pro Spalte, die Teil eines **sys. Indexes** -Indexes oder einer ungeordneten Tabelle (Heap) ist.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID des Objekts, für das der Index definiert wird|  
|**index_id**|**int**|ID des Indexes, in dem die Spalte definiert wird|  
|**index_column_id**|**int**|Die ID der Indexspalte. **index_column_id** ist nur innerhalb **index_id** eindeutig.|  
|**column_id**|**int**|ID der Spalte in **object_id**.<br /><br /> 0 = Zeilenbezeichner (RID, Row Identifier) in einem nicht gruppierten Index.<br /><br /> **column_id** ist nur innerhalb von **object_id** eindeutig.|  
|**key_ordinal**|**tinyint**|Ordinalzahl (auf 1 basierend) innerhalb einer Gruppe von Schlüsselspalten.<br /><br /> 0 = Keine Schlüsselspalte oder ein XML-Index, columnstore-Index oder räumlicher Index.<br /><br /> Hinweis: ein XML-Index oder räumlicher Index kann kein Schlüssel sein, da die zugrunde liegenden Spalten nicht vergleichbar sind. Dies bedeutet, dass ihre Werte nicht geordnet werden können.|  
|**partition_ordinal**|**tinyint**|Ordinalzahl (1-basiert) innerhalb einer Gruppe von Partitionierungsspalten. Ein gruppierter columnstore-Index kann maximal 1 Partitionierungsspalte aufweisen.<br /><br /> 0 = Keine Partitionierungsspalte.|  
|**is_descending_key**|**bit**|1 = Indexschlüsselspalte hat eine absteigende Sortierreihenfolge.<br /><br /> 0 = Indexschlüsselspalte hat eine aufsteigende Sortierreihenfolge, oder die Spalte ist Teil eines columnstore-Indexes oder Hashindexes.|  
|**is_included_column**|**bit**|1 = Spalte ist eine Nichtschlüsselspalte, die dem Index mit der CREATE INDEX INCLUDE-Klausel hinzugefügt wurde, oder die Spalte ist Teil eines columnstore-Indexes.<br /><br /> 0 = Spalte ist keine eingeschlossene Spalte.<br /><br /> Spalten, die implizit hinzugefügt werden, da Sie Teil des Clustering-Schlüssels sind, sind nicht in **sys.index_columns** aufgeführt.<br /><br /> Spalten, die implizit hinzugefügt wurden, da sie eine Partitionierungsspalte sind, werden als 0 zurückgegeben.| 
|**column_store_order_ordinal**</br> Gilt für: Azure Synapse Analytics (Vorschau)|**tinyint**|Ordinalzahl (1-basiert) innerhalb des Satzes von Order-Spalten in einem geordneten gruppierten columnstore--Index.|
  
## <a name="permissions"></a>Berechtigungen

 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Beispiele

 Im folgenden Beispiel werden alle Indizes und Indexspalten für die Tabelle `Production.BillOfMaterials` zurückgegeben.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT i.name AS index_name  
    ,COL_NAME(ic.object_id,ic.column_id) AS column_name  
    ,ic.index_column_id  
    ,ic.key_ordinal  
,ic.is_included_column  
FROM sys.indexes AS i  
INNER JOIN sys.index_columns AS ic
    ON i.object_id = ic.object_id AND i.index_id = ic.index_id  
WHERE i.object_id = OBJECT_ID('Production.BillOfMaterials');  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
  
index_name                                                 column_name        index_column_id key_ordinal is_included_column  
---------------------------------------------------------- -----------------  --------------- ----------- -------------  
AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate ProductAssemblyID  1               1           0  
AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate ComponentID        2               2           0  
AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate StartDate          3               3           0  
PK_BillOfMaterials_BillOfMaterialsID                       BillOfMaterialsID  1               1           0  
IX_BillOfMaterials_UnitMeasureCode                         UnitMeasureCode    1               1           0  
  
(5 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [FAQ: Abfragen des SQL Server-Systemkatalogs](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
