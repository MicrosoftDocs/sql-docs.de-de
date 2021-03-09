---
description: sys.types (Transact-SQL)
title: sys. types (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- types
- types_TSQL
- sys.types_TSQL
- sys.types
dev_langs:
- TSQL
helpviewer_keywords:
- sys.types catalog view
- table-valued parameters,sys.types
ms.assetid: a5dbc842-71a0-4f62-b5e0-f560a99b7f8c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6018562852e20bc30d1ea2c9c9f22022e96693ab
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464069"
---
# <a name="systypes-transact-sql"></a>sys.types (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Enthält eine Zeile für jeden Systemtyp und jeden benutzerdefinierten Typ.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Der Name des Typs. Ist innerhalb des Schemas eindeutig.|  
|**system_type_id**|**tinyint**|Die ID des internen Systemtyps des Typs|  
|**user_type_id**|**int**|Die ID des Typs. Ist in der Datenbank eindeutig. Für System Datentypen **user_type_id**  =  **system_type_id**.|  
|**schema_id**|**int**|Die ID des Schemas, zu dem der Typ gehört.|  
|**principal_id**|**int**|Die ID des einzelnen Besitzers, falls sie sich vom Schemabesitzer unterscheidet. Standardmäßig gehören Objekte mit Schemabereich dem Schemabesitzer. Mit der ALTER AUTHORIZATION-Anweisung kann jedoch ein anderer Besitzer angegeben werden.<br /><br /> Hat den Wert NULL, falls kein alternativer individueller Besitzer angegeben ist.|  
|**max_length**|**smallint**|Maximale Länge (in Bytes) für den Typ.<br /><br /> -1 = der Spaltendatentyp ist **varchar (max)**, **nvarchar (max)**, **varbinary (max)** oder **XML**.<br /><br /> Bei **Text** Spalten ist der **max_length** Wert 16.|  
|**precision**|**tinyint**|Die maximale Genauigkeit des Typs, wenn es sich um einen zahlenbasierten Typ handelt; andernfalls 0.|  
|**scale**|**tinyint**|Die maximalen Dezimalstellen des Typs, wenn es sich um einen zahlenbasierten Typ handelt; andernfalls 0.|  
|**collation_name**|**sysname**|Der Name der Sortierung des Typs, wenn es sich um einen Zeichen basierten Typ handelt. Weitere Weise, NULL.|  
|**is_nullable**|**bit**|Der Typ lässt NULL-Werte zu.|  
|**is_user_defined**|**bit**|1 = Benutzerdefinierter Typ.<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemdatentyp.|  
|**is_assembly_type**|**bit**|1 = Die Implementierung des Typs wird in einer CLR-Assembly definiert.<br /><br /> 0 = Der Typ basiert auf einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemdatentyp.|  
|**default_object_id**|**int**|ID des eigenständigen Standard-, der mithilfe von [sp_bindefault](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)an den Typ gebunden wird.<br /><br /> 0 = Kein Standard vorhanden.|  
|**rule_object_id**|**int**|ID der eigenständigen Regel, die mithilfe von [sp_bindrule](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)an den Typ gebunden wird.<br /><br /> 0 = Keine Regel vorhanden.|  
|**is_table_type**|**bit**|Gibt an, dass der Typ eine Tabelle ist.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Skalare Typenkatalog Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/scalar-types-catalog-views-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [FAQ: Abfragen des SQL Server-Systemkatalogs](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
