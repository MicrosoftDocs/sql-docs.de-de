---
description: PARAMETERS (Transact-SQL)
title: Parameter (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- PARAMETERS_TSQL
- PARAMETERS
dev_langs:
- TSQL
helpviewer_keywords:
- PARAMETERS view
- INFORMATION_SCHEMA.PARAMETERS view
ms.assetid: 06ded0ca-7d21-4400-864a-b801e855b257
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9a59a1e60637a9d1166be81a50e945c3f9c53e36
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89550165"
---
# <a name="parameters-transact-sql"></a>PARAMETERS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine Zeile für jeden Parameter einer benutzerdefinierten Funktion oder gespeicherten Prozedur zurück, auf die der aktuelle Benutzer in der aktuellen Datenbank zugreifen kann. Für Funktionen gibt diese Sicht auch eine Zeile mit Informationen zum Rückgabewert zurück.  
  
 Zum Abrufen von Informationen aus diesen Sichten geben Sie den voll qualifizierten Namen **INFORMATION_SCHEMA an.** _view_name_.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**SPECIFIC_CATALOG**|**nvarchar (** 128 **)**|Katalogname der Routine, für die dies ein Parameter ist|  
|**SPECIFIC_SCHEMA**|**nvarchar (** 128 **)**|Schemaname der Routine, für die dies ein Parameter ist<br /><br /> Wichtig verwenden Sie keine INFORMATION_SCHEMA Sichten, um das Schema eines Objekts zu bestimmen. <strong> \* \* \* \* </strong> INFORMATION_SCHEMA Sichten stellen nur eine Teilmenge der Metadaten eines Objekts dar. Die einzig zuverlässige Möglichkeit zum Finden des Schemas eines Objekts besteht darin, die sys.objects-Katalogsicht abzufragen.|  
|**SPECIFIC_NAME**|**nvarchar (** 128 **)**|Name der Routine, für die dies ein Parameter ist|  
|**ORDINAL_POSITION**|**int**|Die Ordnungsposition des Parameters, beginnend bei 1. Für den Rückgabewerts einer Funktion ist dies 0.|  
|**PARAMETER_MODE**|**nvarchar (** 10 **)**|Gibt IN zurück, wenn es ein Eingabeparameter ist, OUT, wenn es ein Ausgabeparameter ist, und INOUT, wenn es ein Eingabe/Ausgabeparameter ist.|  
|**IS_RESULT**|**nvarchar (** 10 **)**|Gibt YES zurück, wenn das Ergebnis auf einer Routine beruht, die eine Funktion ist. Andernfalls wird NO zurückgegeben.|  
|**AS_LOCATOR**|**nvarchar (** 10 **)**|Gibt YES zurück, wenn der Parameter als Lokator deklariert wurde. Andernfalls wird NO zurückgegeben.|  
|**PARAMETER_NAME**|**nvarchar (** 128 **)**|Der Name des Parameters. NULL, wenn er dem Rückgabewert einer Funktion entspricht.|  
|**DATA_TYPE**|**nvarchar (** 128 **)**|Vom System bereitgestellter Datentyp|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|Maximale Länge in Zeichen für binary-Datentypen oder Zeichendatentypen<br /><br /> -1 für **XML** -Daten und Daten mit umfangreichen Werten. Andernfalls wird NULL zurückgegeben.|  
|**CHARACTER_OCTET_LENGTH**|**int**|Maximale Länge in Bytes für binary-Datentypen oder Zeichendatentypen<br /><br /> -1 für **XML** -Daten und Daten mit umfangreichen Werten. Andernfalls wird NULL zurückgegeben.|  
|**COLLATION_CATALOG**|**nvarchar (** 128 **)**|Gibt immer NULL zurück.|  
|**COLLATION_SCHEMA**|**nvarchar (** 128 **)**|Gibt immer NULL zurück.|  
|**COLLATION_NAME**|**nvarchar (** 128 **)**|Name der Sortierung des Parameters. Wenn es sich nicht um einen der Zeichentypen handelt, wird NULL zurückgegeben.|  
|**CHARACTER_SET_CATALOG**|**nvarchar (** 128 **)**|Katalogname des Zeichensatzes des Parameters. Wenn es sich nicht um einen der Zeichentypen handelt, wird NULL zurückgegeben.|  
|**CHARACTER_SET_SCHEMA**|**nvarchar (** 128 **)**|Gibt immer NULL zurück.|  
|**CHARACTER_SET_NAME**|**nvarchar (** 128 **)**|Name des Zeichensatzes des Parameters. Wenn es sich nicht um einen der Zeichentypen handelt, wird NULL zurückgegeben.|  
|**NUMERIC_PRECISION**|**tinyint**|Genauigkeit für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|Basis der Genauigkeit für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|**NUMERIC_SCALE**|**tinyint**|Anzahl der Dezimalstellen für Spalten mit ungefähren numerischen Daten, exakten numerischen Daten, ganzzahligen Daten oder Währungsdaten. Andernfalls wird NULL zurückgegeben.|  
|**DATETIME_PRECISION**|**smallint**|Genauigkeit in Sekundenbruchteilen, wenn der Parametertyp **DateTime** oder **smalldatetime**ist. Andernfalls wird NULL zurückgegeben.|  
|**INTERVAL_TYPE**|**nvarchar (** 30 **)**|NULL. Für zukünftige Verwendung reserviert.|  
|**INTERVAL_PRECISION**|**smallint**|NULL. Für zukünftige Verwendung reserviert.|  
|**USER_DEFINED_TYPE_CATALOG**|**nvarchar (** 128 **)**|NULL. Für zukünftige Verwendung reserviert.|  
|**USER_DEFINED_TYPE_SCHEMA**|**nvarchar (** 128 **)**|NULL. Für zukünftige Verwendung reserviert.|  
|**USER_DEFINED_TYPE_NAME**|**nvarchar (** 128 **)**|NULL. Für zukünftige Verwendung reserviert.|  
|**SCOPE_CATALOG**|**nvarchar (** 128 **)**|NULL. Für zukünftige Verwendung reserviert.|  
|**SCOPE_SCHEMA**|**nvarchar (** 128 **)**|NULL. Für zukünftige Verwendung reserviert.|  
|**SCOPE_NAME**|**nvarchar (** 128 **)**|NULL. Für zukünftige Verwendung reserviert.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [System Sichten &#40;Transact-SQL-&#41;](https://msdn.microsoft.com/library/35a6161d-7f43-4e00-bcd3-3091f2015e90)   
 [Informations Schema Sichten &#40;Transact-SQL-&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)  
  
  
