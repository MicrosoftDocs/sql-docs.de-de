---
description: sys.sql_modules (Transact-SQL)
title: sys.sql_modules (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/06/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sql_modules_TSQL
- sql_modules
- sql_modules_TSQL
- sys.sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_modules catalog view
ms.assetid: 23d3ccd2-f356-4d89-a2cd-bee381243f99
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d456a51ca01f38968bf04d3144331fc4b1b2f933
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340729"
---
# <a name="syssql_modules-transact-sql"></a>sys.sql_modules (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile für jedes Objekt zurück, bei dem es sich um ein von der SQL-Sprache definiertes Modul in handelt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , einschließlich einer nativ kompilierten benutzerdefinierten Skalarfunktion Objekten des Typs P, RF, V, TR, FN, IF, TF und R ist ein SQL-Modul zugeordnet. Eigenständige Standards, Objekte des Typs D, haben ebenfalls eine SQL-Moduldefinition in dieser Sicht. Eine Beschreibung dieser Typen finden Sie unter der **Type** -Spalte in der [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) -Katalog Sicht.  
  
 Weitere Informationen dazu finden Sie unter [Benutzerdefinierte Skalarfunktionen für In-Memory-OLTP](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
|Spaltenname|Datentyp|Beschreibung|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Die Objekt-ID des enthaltenen Objekts. Ist innerhalb einer Datenbank eindeutig.|  
|**Definition**|**nvarchar(max)**|Der SQL-Text, der dieses Modul definiert. Dieser Wert kann auch mit der [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) integrierten Funktion abgerufen werden.<br /><br /> NULL = Verschlüsselt.|  
|**uses_ansi_nulls**|**bit**|Das Modul wurde mit SET ANSI_NULLS ON erstellt.<br /><br /> Ist immer = 0 für Regeln und Standardwerte.|  
|**uses_quoted_identifier**|**bit**|Das Modul wurde mit SET QUOTED_IDENTIFIER ON erstellt.|  
|**is_schema_bound**|**bit**|Das Modul wurde mit der Option SCHEMABINDING erstellt.<br /><br /> Enthält immer den Wert 1 für systemintern kompilierte gespeicherte Prozeduren.|  
|**uses_database_collation**|**bit**|1 = Die richtige Auswertung der schemagebundenen Moduldefinition ist abhängig von der Standardsortierung der Datenbank; andernfalls ist der Wert 0. Eine solche Abhängigkeit verhindert die Änderung der Standardsortierung der Datenbank.|  
|**is_recompiled**|**bit**|Die Prozedur wurde mit der Option WITH RECOMPILE erstellt.|  
|**null_on_null_input**|**bit**|Das Modul wurde so deklariert, dass auf eine NULL-Eingabe eine NULL-Ausgabe folgt.|  
|**execute_as_principal_id**|**Int**|Die ID des Datenbankprinzipals EXECUTE AS.<br /><br /> NULL als Standardwert oder bei Verwendung von EXECUTE AS CALLER.<br /><br /> ID des angegebenen Prinzipals, wenn EXECUTE als Self oder EXECUTE AS ausgeführt wird \<principal> .<br /><br /> -2 = EXECUTE AS OWNER.|  
|**uses_native_compilation**|**bit**|**Gilt für**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] bis [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].<br /><br /> 0 = nicht systemintern kompiliert<br /><br /> 1 = systemintern kompiliert<br /><br /> Der Standardwert ist 0.|  
|**is_inlineable**|**bit**|**Gilt für**:  [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] und höher.<br/><br />Gibt an, ob das Modul Inline fähig ist oder nicht. Die Inline barkeit basiert auf den [hier](../user-defined-functions/scalar-udf-inlining.md#inlineable-scalar-udfs-requirements)angegebenen Bedingungen.<br /><br /> 0 = nicht Inline fähig<br /><br /> 1 = ist Inline fähig. <br /><br /> Bei skalaren UDFs ist der Wert 1, wenn die UDF offline ist, andernfalls 0. Sie enthält immer den Wert 1 für Inline-TVFs und 0 für alle anderen Modultypen.<br />|  
|**inline_type**|**bit**|**Gilt für**:  [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] und höher.<br /><br />Gibt an, ob Inlining für das Modul aktuell aktiviert ist. <br /><br />0 = Inlining ist ausgeschaltet<br /><br /> 1 = Inlining ist eingeschaltet.<br /><br /> Bei skalaren UDFs ist der Wert 1, wenn Inlining aktiviert ist (explizit oder implizit). Der Wert ist für Inline-TVFs immer 1 und 0 für andere Modultypen.<br />|  

  
## <a name="remarks"></a>Bemerkungen  
 Der SQL-Ausdruck für eine DEFAULT-Einschränkung, Objekt vom Typ D, befindet sich in der [sys.default_constraints](../../relational-databases/system-catalog-views/sys-default-constraints-transact-sql.md) -Katalog Sicht. Der SQL-Ausdruck für eine Check-Einschränkung, Objekt vom Typ C, finden Sie in der [sys.check_constraints](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md) -Katalog Sicht.  
  
 Diese Informationen werden auch in [sys.dm_db_uncontained_entities &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md)beschrieben.  
  
## <a name="permissions"></a>Berechtigungen  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden der Name, der Typ und die Definition der einzelnen Module in der aktuellen Datenbank zurückgegeben.  
  
```  
SELECT sm.object_id, OBJECT_NAME(sm.object_id) AS object_name, o.type, o.type_desc, sm.definition  
FROM sys.sql_modules AS sm  
JOIN sys.objects AS o ON sm.object_id = o.object_id  
ORDER BY o.type;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Katalogsichten &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Katalogsichten für Objekte &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Abfragen der SQL Server System Katalog-FAQ](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [In-Memory-OLTP &#40;Arbeitsspeicheroptimierung&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
