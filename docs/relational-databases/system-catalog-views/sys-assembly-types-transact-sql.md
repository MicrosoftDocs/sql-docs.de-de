---
description: sys.assembly_types (Transact-SQL)
title: sys.assembly_types (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- assembly_types
- sys.assembly_types
- sys.assembly_types_TSQL
- assembly_types_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.assembly_types catalog view
ms.assetid: 35f0384f-7a6d-41b1-9461-f1406d68f317
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ef2bb6c85593377e6239a1bff502da7d1f3a79f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197852"
---
# <a name="sysassembly_types-transact-sql"></a>sys.assembly_types (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Enthält eine Zeile für jeden benutzerdefinierten Datentyp, der durch eine CLR-Assembly definiert ist. Die folgenden **sys.assembly_types** werden in der Liste der geerbten Spalten (siehe [sys. types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)) nach **rule_object_id** angezeigt.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**assembly_id**|**int**|ID der Assembly, aus der dieser Typ erstellt wurde.|  
|**assembly_class**|**sysname**|Name der Klasse innerhalb der Assembly, mit der dieser Typ definiert wird.|  
|**is_binary_ordered**|**bit**|Die Bytesortierung dieses Typs entspricht der Sortierung mithilfe von Vergleichsoperatoren für den Typ.|  
|**is_fixed_length**|**bit**|Die Länge des Typs ist immer mit max_length identisch.|  
|**prog_id**|**nvarchar(40)**|ProgID des Typs gemäß Einbindung für COM.|  
|**assembly_qualified_name**|**nvarchar(4000)**|Der vollqualifizierte Name der Assembly. Der Name hat ein Format, das an Type.GetType() übergeben werden kann.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Skalare Typenkatalog Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/scalar-types-catalog-views-transact-sql.md)  
  
  
