---
description: sys.all_parameters (Transact-SQL)
title: sys.ALL_PARAMETERS (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- all_parameters_TSQL
- sys.all_parameters
- all_parameters
- sys.all_parameters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.all_parameters catalog view
ms.assetid: eecbb68e-9b4c-4243-94e2-8096a9cc7892
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 49b8b9d54a8ff076b8f7bcc72aebe61d41c71f84
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465038"
---
# <a name="sysall_parameters-transact-sql"></a>sys.all_parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Zeigt die Vereinigungsmenge aller Parameter an, die zu einem benutzerdefinierten Objekt oder einem Systemobjekt gehören.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Die ID des Objekts, zu dem dieser Parameter gehört.|  
|**name**|**sysname**|Name des Parameters. Ist eindeutig innerhalb des Objekts. Wenn es sich bei dem Objekt um eine skalare Funktion handelt, ist der Parametername eine leere Zeichenfolge in der Zeile, die den zurückgegebenen Wert darstellt.|  
|**parameter_id**|**int**|ID des Parameters. Ist eindeutig innerhalb des Objekts. Handelt es sich bei dem Objekt um eine Skalarfunktion, stellt **parameter_id** = 0 den Rückgabewert dar.|  
|**system_type_id**|**tinyint**|ID des Systemtyps des Parameters.|  
|**user_type_id**|**int**|Die ID des vom Benutzer definierten Typs des Parameters.<br /><br /> Stellen Sie einen Join mit der [sys.types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) -Katalogsicht für diese Spalte her, um den Namen des Typs zurückzugeben.|  
|**max_length**|**smallint**|Die maximale Länge des Parameters (in Byte).<br /><br /> -1 = der Spaltendatentyp ist **varchar (max)**, **nvarchar (max)**, **varbinary (max)** oder **XML**.|  
|**precision**|**tinyint**|Genauigkeit des Parameters, wenn dieser auf Nummern basiert; andernfalls 0.|  
|**scale**|**tinyint**|Skalierung des Parameters, wenn dieser auf Nummern basiert; andernfalls 0.|  
|**is_output**|**bit**|1 = Parameter ist ein Ausgabeparameter (oder Rückgabeparameter), andernfalls 0|  
|**is_cursor_ref**|**bit**|1 = Parameter ist ein Cursorverweisparameter.|  
|**has_default_value**|**bit**|1 = Parameter hat einen Standardwert.<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwaltet nur Standardwerte für CLR-Objekte in dieser Katalog Sicht. Daher weist diese Spalte für-Objekte immer den Wert 0 auf [!INCLUDE[tsql](../../includes/tsql-md.md)] . Wenn Sie den Standardwert eines Parameters in einem [!INCLUDE[tsql](../../includes/tsql-md.md)] -Objekt anzeigen möchten, fragen Sie die **definition** -Spalte der [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) -Katalogsicht ab, oder verwenden Sie die [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) -Systemfunktion.|  
|**is_xml_document**|**bit**|1 = Der Inhalt ist ein vollständiges XML-Dokument.<br /><br /> 0 = Inhalt ist ein Dokumentfragment, oder der Datentyp der Spalte ist nicht **xml**.|  
|**default_value**|**sql_variant**|Wenn **has_default_value** 1 ist, ist der Wert dieser Spalte der Standardwert für den-Parameter. andernfalls NULL.|  
|**xml_collection_id**|**int**|Die ID der XML-Schemaauflistung, die zur Überprüfung des Parameters verwendet wird.<br /><br /> Ungleich 0 (null), wenn der Datentyp des Parameters **XML** ist und die XML-Daten eingegeben werden.<br /><br /> 0 = Es ist keine XML-Schemaauflistung vorhanden, oder der Parameter ist nicht XML.|  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Abfragen der SQL Server System Katalog-FAQ](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [sys. Parameters &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)   
 [sys.system_parameters &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-system-parameters-transact-sql.md)  
  
  
