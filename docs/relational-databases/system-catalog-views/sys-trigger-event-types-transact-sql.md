---
description: sys.trigger_event_types (Transact-SQL)
title: sys. trigger_event_types (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- trigger_event_types_TSQL
- sys.trigger_event_types_TSQL
- sys.trigger_event_types
- trigger_event_types
dev_langs:
- TSQL
helpviewer_keywords:
- sys.trigger_event_types catalog view
ms.assetid: 054aed54-7151-4760-934a-149fa434f1ae
author: markingmyname
ms.author: maghan
ms.openlocfilehash: aaa944c06fc90a5a7b1a1c421b1c77963aa5d40e
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89544906"
---
# <a name="systrigger_event_types-transact-sql"></a>sys.trigger_event_types (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile für jedes Ereignis oder jede Ereignisgruppe zurück, für die ein Trigger ausgelöst werden kann.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**type**|**int**|Ereignistyp oder Ereignisgruppe, der bzw. die einen Trigger auslöst.|  
|**type_name**|**nvarchar (64)**|Name eines Ereignisses oder einer Ereignisgruppe. Dieser kann in der FOR-Klausel einer [CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md) -Anweisung angegeben werden.|  
|**parent_type**|**int**|Typ einer Ereignisgruppe, die dem Ereignis oder der Ereignisgruppe übergeordnet ist.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
