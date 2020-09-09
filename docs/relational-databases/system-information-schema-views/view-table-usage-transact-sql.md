---
description: VIEW_TABLE_USAGE (Transact-SQL)
title: View_Table_Usage (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- VIEW_TABLE_USAGE_TSQL
- VIEW_TABLE_USAGE
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.VIEW_TABLE_USAGE view
- VIEW_TABLE_USAGE view
ms.assetid: 0aeefb3f-02ef-457e-8c42-84ddb26f1c88
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 29543e27bbe08efa2a20ce0499bc2d2119bc91d1
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89548489"
---
# <a name="view_table_usage-transact-sql"></a>VIEW_TABLE_USAGE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile für jede Tabelle in der aktuellen Datenbank zurück, die in einer Sicht verwendet wird. Diese Informationsschemasicht gibt Informationen zu den Objekten zurück, für die der aktuelle Benutzer Berechtigungen besitzt.  
  
 Zum Abrufen von Informationen aus diesen Sichten geben Sie den voll qualifizierten Namen **INFORMATION_SCHEMA an.** _view_name_.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**VIEW_CATALOG**|**nvarchar (** 128 **)**|Sichtqualifizierer|  
|**VIEW_SCHEMA**|**nvarchar (** 128 **)**|Der Name des Schemas, das die Sicht enthält.<br /><br /> **&#42;&#42; wichtig &#42;&#42;**  nur eine zuverlässige Möglichkeit, das Schema eines-Objekts zu finden, besteht darin, die sys. Objects-Katalog Sicht abzufragen.|  
|**VIEW_NAME**|**sysname**|Ansichtsname.|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Tabellenqualifizierer|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Name des Schemas, das die Basistabelle enthält<br /><br /> **&#42;&#42; wichtig &#42;&#42;**  nur eine zuverlässige Möglichkeit, das Schema eines-Objekts zu finden, besteht darin, die sys. Objects-Katalog Sicht abzufragen.|  
|**TABLE_NAME**|**sysname**|Basistabelle der Sicht|  
  
## <a name="see-also"></a>Weitere Informationen  
 [System Sichten &#40;Transact-SQL-&#41;](https://msdn.microsoft.com/library/35a6161d-7f43-4e00-bcd3-3091f2015e90)   
 [Informations Schema Sichten &#40;Transact-SQL-&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys. sql_dependencies &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-sql-dependencies-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-views-transact-sql.md)  
  
  
