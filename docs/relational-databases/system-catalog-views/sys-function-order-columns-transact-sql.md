---
description: sys.function_order_columns (Transact-SQL)
title: sys. function_order_columns (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- function_order_columns
- sys.function_order_columns_TSQL
- function_order_columns_TSQL
- sys.function_order_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.function_order_columns catalog view
ms.assetid: 29287973-3125-4d35-8ca9-92cb45828854
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5c7ac555630f4472e5cf58ad9017cdb03643ae2b
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89546793"
---
# <a name="sysfunction_order_columns-transact-sql"></a>sys.function_order_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile pro Spalte zurück, die Teil eines **ORDER** -Ausdrucks einer CLR-Tabellenwertfunktion (Common Language Runtime) ist.  

  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID des Objekts (CLR-Tabellenwertfunktion), das die Reihenfolge definiert.|  
|**order_column_id**|**int**|Die ID der Sortierspalte. **order_column_id** ist nur innerhalb von **object_id**eindeutig.<br /><br /> **order_column_id** stellt die Position dieser Spalte in der Sortierung dar.|  
|**column_id**|**int**|ID der Spalte in **object_id**.<br /><br /> **column_id** ist nur innerhalb von **object_id**eindeutig.|  
|**is_descending**|**bit**|1 = Die Sortierspalte hat eine absteigende Sortierreihenfolge.<br /><br /> 0 = Die Sortierspalte hat eine aufsteigende Sortierreihenfolge.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
