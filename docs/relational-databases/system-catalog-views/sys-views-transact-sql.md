---
description: sys.views (Transact-SQL)
title: sys. views (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- views_TSQL
- views
- sys.views_TSQL
- sys.views
dev_langs:
- TSQL
helpviewer_keywords:
- sys.views catalog view
ms.assetid: f8a8ea39-5a09-4662-801e-b43519467def
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 25dcf255096fae920e3bb66a1c0cbcf8cb31f723
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482861"
---
# <a name="sysviews-transact-sql"></a>sys.views (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Enthält eine Zeile für jedes Ansichts Objekt mit **sys. Objects. Type** = V.  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||Eine Liste der Spalten, die diese Sicht erbt, finden Sie unter [sys. Objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)|  
|**is_replicated**|**bit**|1 = Sicht ist repliziert.|  
|**has_replication_filter**|**bit**|1 = Sicht verfügt über einen Replikationsfilter.|  
|**has_opaque_metadata**|**bit**|1 = VIEW_METADATA-Option wurde für die Sicht angegeben. Weitere Informationen finden Sie unter [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).|  
|**has_unchecked_assembly_data**|**bit**|1 = Sicht enthält persistente Daten, die von einer Assembly abhängen, deren Definition seit dem letzten ALTER ASSEMBLY geändert wurde. Wird nach dem nächsten erfolgreichen DBCC CHECKDB oder DBCC CHECKTABLE auf 0 zurückgesetzt.|  
|**with_check_option**|**bit**|1 = WITH CHECK OPTION wurde in der Sichtdefinition angegeben.|  
|**is_date_correlation_view**|**bit**|1 = Sicht wurde vom System automatisch zum Speichern von Korrelationsinformationen zwischen datetime-Spalten erstellt. Die Erstellung der Sicht wurde durch Festlegen von DATE_CORRELATION_OPTIMIZATION auf ON aktiviert.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [ALTER ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-assembly-transact-sql.md)   
 [DBCC CHECKDB &#40;Transact-SQL-&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)   
 [DBCC CHECKTABLE &#40;Transact-SQL-&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)   
 [FAQ: Abfragen des SQL Server-Systemkatalogs](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
