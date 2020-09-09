---
description: sys.plan_guides (Transact-SQL)
title: sys. plan_guides (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.planguides_TSQL
- plan_guides
- sys.planguides
- plan_guides_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.plan_guides catalog view
ms.assetid: 3dde0397-ef6f-4b3f-8250-3f25584eb62b
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e8aacbeebf50eae1a6e20d35262dbdc054f1429d
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89550480"
---
# <a name="sysplan_guides-transact-sql"></a>sys.plan_guides (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Enthält eine Zeile für jede Planhinweisliste in der Datenbank.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**plan_guide_id**|**int**|Eindeutiger Bezeichner für die Planhinweisliste in der Datenbank.|  
|**name**|**sysname**|Name der Planhinweisliste.|  
|**create_date**|**datetime**|Datum und Uhrzeit der Erstellung der Planhinweisliste.|  
|**modify_date**|**Datetime**|Datum, an dem die Planhinweisliste zuletzt geändert wurde.|  
|**is_disabled**|**bit**|1 = Planhinweisliste ist deaktiviert.<br /><br /> 0 = Planhinweisliste ist aktiviert.|  
|**query_text**|**nvarchar(max)**|Text der Abfrage, anhand derer die Planhinweisliste erstellt wird.|  
|**scope_type**|**tinyint**|Identifiziert den Bereich der Planhinweisliste.<br /><br /> 1 = OBJECT<br /><br /> 2 = SQL<br /><br /> 3 = TEMPLATE|  
|**scope_type_desc**|**nvarchar(60)**|Beschreibung des Bereichs der Planhinweisliste.<br /><br /> OBJECT<br /><br /> SQL<br /><br /> TEMPLATE|  
|**scope_object_id**|**Int**|object_id des Objekts, das den Bereich der Planhinweisliste definiert, wenn der Bereich OBJECT ist.<br /><br /> NULL, wenn der Bereich der Planhinweisliste nicht OBJECT ist.|  
|**scope_batch**|**nvarchar(max)**|Batch Text, wenn **scope_type** SQL ist.<br /><br /> NULL, wenn der Batchtyp nicht SQL ist.<br /><br /> Wenn NULL und **scope_type** SQL ist, gilt der Wert von **query_text** .|  
|**parameters**|**nvarchar(max)**|Die Zeichenfolge zur Definition der Liste der Parameter, die mit der Planhinweisliste verknüpft sind.<br /><br /> NULL = Mit der Planhinweisliste ist keine Parameterliste verknüpft.|  
|**entgegen**|**nvarchar(max)**|Die Hinweise der OPTION-Klausel, die mit der Planhinweisliste verknüpft sind.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
  
