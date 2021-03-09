---
description: sys.columns (Transact-SQL)
title: sys. Columns (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.columns_TSQL
- sys.columns
- columns_TSQL
- columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.columns catalog view
ms.assetid: 323ac9ea-fc52-4b8c-8a7e-e0e44f8ed86c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: babedc8a8f4711776e3bc014ee83b22130bc6554
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464748"
---
# <a name="syscolumns-transact-sql"></a>sys.columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile für jede Spalte eines Objekts zurück, das Spalten besitzt, z. B. Sichten oder Tabellen. Im Folgenden finden Sie eine Liste der Objekttypen mit Spalten:  
  
-   Assembly-Tabellenwertfunktionen (FT)  
  
-   Inline-SQL-Tabellenwertfunktionen (IF)  
  
-   Interne Tabellen (IT)  
  
-   Systemtabellen (S)  
  
-   SQL-Tabellenwertfunktionen (TF)  
  
-   Benutzertabellen (U)  
  
-   Sichten (V)  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Die ID des Objekts, zu dem diese Spalte gehört.|  
|name|**sysname**|Name der Spalte. Ist eindeutig innerhalb des Objekts.|  
|column_id|**int**|ID der Spalte. Ist eindeutig innerhalb des Objekts.<br /><br /> Spalten-IDs sind möglicherweise nicht sequenziell.|  
|system_type_id|**tinyint**|ID des System Typs der Spalte.|  
|user_type_id|**int**|Die ID des vom Benutzer definierten Typs der Spalte.<br /><br /> Stellen Sie einen Join mit der [sys.types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) -Katalogsicht für diese Spalte her, um den Namen des Typs zurückzugeben.|  
|max_length|**smallint**|Maximale Länge (in Byte) für die Spalte.<br /><br /> -1 = der Spaltendatentyp ist **varchar (max)**, **nvarchar (max)**, **varbinary (max)** oder **XML**.<br /><br /> Bei **Text** Spalten ist der max_length Wert 16 oder der Wert, der durch sp_tableoption ' Text in row ' festgelegt wird.|  
|precision (Genauigkeit)|**tinyint**|Die Genauigkeit der Spalte, wenn sie auf numerischen Werten basiert; andernfalls 0.|  
|Skalierung|**tinyint**|Die Skalierung der Spalte, wenn sie auf numerischen Werten basiert; andernfalls 0.|  
|collation_name|**sysname**|Name der Sortierung der Spalte, wenn diese zeichenbasiert ist, andernfalls NULL.|  
|is_nullable|**bit**|1 = Spalte lässt NULL-Werte zu.|  
|is_ansi_padded|**bit**|1 = Spalte verwendet ANSI_PADDING ON-Verhalten, wenn es sich um Zeichen- oder Binärdaten bzw. Daten vom Typ Variant handelt.<br /><br /> 0 = Bei der Spalte handelt es sich um Zeichen- oder Binärdaten bzw. Daten vom Typ Variant.|  
|is_rowguidcol|**bit**|1 = Spalte ist eine deklarierte ROWGUIDCOL.|  
|is_identity|**bit**|1 = Die Spalte verfügt über Identitätswerte.|  
|is_computed|**bit**|1 = Spalte ist eine berechnete Spalte.|  
|is_filestream|**bit**|1 = Spalte ist eine FILESTREAM-Spalte.|  
|is_replicated|**bit**|1 = Spalte wird repliziert.|  
|is_non_sql_subscribed|**bit**|1 = Die Spalte hat einen Nicht-SQL Server-Abonnenten.|  
|is_merge_published|**bit**|1 = Spalte verwendet die Mergeveröffentlichung.|  
|is_dts_replicated|**bit**|1 = Die Spalte wird mithilfe von [!INCLUDE[ssIS](../../includes/ssis-md.md)] repliziert.|  
|is_xml_document|**bit**|1 = Der Inhalt ist ein vollständiges XML-Dokument.<br /><br /> 0 = der Inhalt ist ein Dokument Fragment, oder der Spaltendatentyp ist nicht **XML**.|  
|xml_collection_id|**int**|Ungleich 0 (null), wenn der Datentyp der Spalte **XML** ist und die XML-Daten eingegeben werden. Der Wert entspricht der ID der Auflistung, die den prüfenden XML-Schemanamespace der Spalte enthält.<br /><br /> 0 = Keine XML-Schemaauflistung|  
|default_object_id|**int**|ID des Standard Objekts, unabhängig davon, ob es sich um ein eigenständiges Objekt [sys.sp_bindefault](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)oder eine Inline-Standard Einschränkung auf Spaltenebene handelt. Die parent_object_id-Spalte eines DEFAULT-Inlineobjekts ist ein Verweis auf die Tabelle selbst.<br /><br /> 0 = Kein Standard.|  
|rule_object_id|**int**|ID der eigenständigen Regel, die mithilfe von sys.sp_bindrule gebunden wird.<br /><br /> 0 = Keine eigenständige Regel. Informationen zu Check-Einschränkungen auf Spaltenebene finden Sie unter [sys.check_constraints &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md).|  
|is_sparse|**bit**|1 = Spalte ist eine Sparsespalte. Weitere Informationen finden Sie unter [Verwenden von Spalten mit geringer Dichte](../../relational-databases/tables/use-sparse-columns.md).|  
|is_column_set|**bit**|1 = Spalte ist ein Spaltensatz. Weitere Informationen finden Sie unter [Verwenden von Spalten mit geringer Dichte](../../relational-databases/tables/use-sparse-columns.md).|  
|generated_always_type|**tinyint**|**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Gibt an, wann der Spaltenwert generiert wird (für Spalten in Systemtabellen immer 0):<br /><br /> 0 = NOT_APPLICABLE<br /><br /> 1 = AS_ROW_START<br /><br /> 2 = AS_ROW_END<br /><br /> Weitere Informationen finden Sie unter [Temporale Tabellen &#40;relationale Datenbanken&#41;](../../relational-databases/tables/temporal-tables.md).|  
|generated_always_type_desc|**nvarchar(60)**|**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Textbeschreibung des `generated_always_type` Werts (für Spalten in Systemtabellen immer NOT_APPLICABLE) <br /><br /> NOT_APPLICABLE<br /><br /> AS_ROW_START<br /><br /> AS_ROW_END|  
|encryption_type|**int**|**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Verschlüsselungstyp:<br /><br /> 1 = deterministische Verschlüsselung<br /><br /> 2 = zufällige Verschlüsselung|  
|encryption_type_desc|**nvarchar (64)**|**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Beschreibung des Verschlüsselungs Typs:<br /><br /> Zufällige<br /><br /> DETERMINISTIC|  
|encryption_algorithm_name|**sysname**|**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Der Name des Verschlüsselungsalgorithmus.<br /><br /> Nur AEAD_AES_256_CBC_HMAC_SHA_512 wird unterstützt.|  
|column_encryption_key_id|**int**|**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> ID des Cek.|  
|column_encryption_key_database_name|**sysname**|**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher, [!INCLUDE[ssSDW_md](../../includes/sssds-md.md)].<br /><br /> Der Name der Datenbank, in der der Spalten Verschlüsselungsschlüssel vorhanden ist, wenn er sich von der Datenbank der Spalte unterscheidet. NULL, wenn der Schlüssel in der Datenbank vorhanden ist, in der sich die Spalte befindet.|  
|is_hidden|**bit**|**Gilt für:** [!INCLUDE[ssCurrentLong](../../includes/sscurrent-md.md)] und höher, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Gibt an, ob die Spalte ausgeblendet ist:<br /><br /> 0 = reguläre, nicht verborgene, sichtbare Spalte<br /><br /> 1 = verborgene Spalte|  
|is_masked|**bit**|**Gilt für:** [!INCLUDE[ssCurrentLong](../../includes/sscurrent-md.md)] und höher, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].<br /><br /> Gibt an, ob die Spalte durch eine dynamische Daten Maskierung maskiert wird:<br /><br /> 0 = reguläre, nicht maskierte Spalte<br /><br /> 1 = Spalte ist maskiert|  


 
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [System Sichten &#40;Transact-SQL-&#41;](../../t-sql/language-reference.md)   
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Abfragen der SQL Server System Katalog-FAQ](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [sys.ALL_COLUMNS &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.system_columns &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-system-columns-transact-sql.md)  
  
