---
description: VIEW_COLUMN_USAGE (Transact-SQL)
title: View_Column_Usage (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- VIEW_COLUMN_USAGE
- VIEW_COLUMN_USAGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.VIEW_COLUMN_USAGE view
- VIEW_COLUMN_USAGE view
ms.assetid: fc0b3608-a7e8-4532-8215-32235d6670f1
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3f72b7a0555615a3bb4852ef61e5bdb62f98eeca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190079"
---
# <a name="view_column_usage-transact-sql"></a>VIEW_COLUMN_USAGE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile für jede Spalte in der aktuellen Datenbank zurück, die in einer Sichtdefinition verwendet wird. Diese Informationsschemasicht gibt Informationen zu den Objekten zurück, für die der aktuelle Benutzer Berechtigungen besitzt.  
  
 Zum Abrufen von Informationen aus diesen Sichten geben Sie den voll qualifizierten Namen **INFORMATION_SCHEMA an.** _view_name_.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**VIEW_CATALOG**|**nvarchar (** 128 **)**|Sichtqualifizierer|  
|**VIEW_SCHEMA**|**nvarchar (** 128 **)**|Der Name des Schemas, das die Sicht enthält.<br /><br /> **&#42;&#42; wichtig &#42;&#42;**  nur eine zuverlässige Möglichkeit, das Schema eines-Objekts zu finden, besteht darin, die sys. Objects-Katalog Sicht abzufragen.|  
|**VIEW_NAME**|**sysname**|Ansichtsname.|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Tabellenqualifizierer|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Der Name des Schemas, das die Tabelle enthält.<br /><br /> **&#42;&#42; wichtig &#42;&#42;**  nur eine zuverlässige Möglichkeit, das Schema eines-Objekts zu finden, besteht darin, die sys. Objects-Katalog Sicht abzufragen.|  
|**TABLE_NAME**|**sysname**|Basistabelle.|  
|**COLUMN_NAME**|**sysname**|Spaltenname.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [System Sichten &#40;Transact-SQL-&#41;](../../t-sql/language-reference.md)   
 [Informations Schema Sichten &#40;Transact-SQL-&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.sql_dependencies &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-sql-dependencies-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)  
  
