---
description: sp_data_source_objects (Transact-SQL)
title: sp_data_source_objects | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/14/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sp_data_source_objects
helpviewer_keywords:
- PolyBase
ms.assetid: 48066431-fed2-4a8a-85af-ac704689e183
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7f3aa43750cebfbbac77e3807845f1ed154af1ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067215"
---
# <a name="sp_data_source_objects-transact-sql"></a>sp_data_source_objects (Transact-SQL)

[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

Gibt eine Liste von Tabellen Objekten zurück, die für die Virtualisierung verfügbar sind.

> [!NOTE]
> Dieses Verfahren wird in [SQL 2019 CU5](../../big-data-cluster/release-notes-big-data-cluster.md#cu5)eingeführt.

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

## <a name="syntax"></a>Syntax  

```syntaxsql
sp_data_source_objects  
         [ @data_source = ] 'data_source'
     [ , [ @object_root_name = ] 'object_root_name' ]
     [ , [ @max_search_depth = ] max_search_depth ]
     [ , [ @search_options = ] 'search_options' ]
[ ; ]
```  
  
## <a name="arguments"></a>Argumente  

`[ @data_source = ] 'data_source'`   
Der Name der externen Datenquelle, von der die Metadaten abgeleitet werden sollen. `@data_source` ist `sysname`  

`[ @object_root_name = ] 'object_root_name'`   
Dieser Parameter ist der Stamm des Namens der zu suchenden Objekte. `@object_root_name` ist `nvarchar(max)` vom Typ. der Standardwert ist `NULL` .

Dieser Rückruf gibt nur externe Objekte zurück, die mit dem für festgelegten Wert beginnen `@object_root_name` .

Wenn eine ODBC-Datenquelle eine Verbindung mit einem Management System für relationale Datenbanken (RDBMS) herstellt, das dreiteilige Namen verwendet, `@object_root_name` darf keinen partiellen Datenbanknamen enthalten. In diesen Fällen sollte der-Parameter `@object_root_name` alle drei Teile enthalten, wobei der dritte Teil der zu durchsuchende Objektname ist.
> [!CAUTION]
> Aufgrund von Unterschieden zwischen externen Daten Plattformen geben einige Plattformen keine Ergebnisse zurück, wenn der Standardwert von `NULL` angegeben wird. Einige behandeln `NULL` den Mangel an Filtern. Oracle rdmb gibt beispielsweise keine Ergebnisse zurück, wenn `NULL` für bereitgestellt wird `@object_root_name` .

`[ @max_search_depth = ] max_search_depth`   
Dieser Wert gibt die maximale Tiefe (in Teilen) an, die über der liegt `@object_root_name` , die durchsucht werden soll. `@max_search_depth` ist `int` vom Typ. der Standardwert ist 1.

Beispielsweise würde ein `@max_search_depth` von 1 mit einem, `@object_root_name` der der Name einer SQL Server Datenbank ist, in der Datenbank enthaltene Schemata zurückgeben.

Eine `@max_search_depth` von gibt `NULL` Informationen darüber zurück `@object_root_name` , ob Sie vorhanden und nicht leer ist, im Fall eines Katalogs oder Schemas.

`[ @search_options = ] 'search_options'`   
Der-Parameter ist vom Datentyp `search_options` nvarchar (max) und hat den Standardwert `NULL` .

Dieser Parameter wird nicht verwendet, kann aber in Zukunft implementiert werden.

## <a name="result-sets"></a>Resultsets

| Spaltenname | Datentyp | BESCHREIBUNG |
|--|--|--|
| Object_Type | nvarchar(200) | Der Typ des Objekts (Beispiel: Tabelle oder Datenbank). |
| OBJECT_NAME | nvarchar(max) | Der voll qualifizierte Name des-Objekts. Mit Escapezeichen mit Escapezeichen versehen. |
| OBJECT_LEAF_NAME | nvarchar(max) | Der nicht qualifizierte Objektname. |
| TABLE_LOCATION | nvarchar(max) | Eine gültige Zeichenfolge für den Tabellen Speicherort, die für eine CREATE externe TABLE-Anweisung verwendet werden kann. Ist, `NULL` Wenn es nicht anwendbar ist. |
  
## <a name="permissions"></a>Berechtigungen

Erfordert eine ALTER ANY EXTERNAL DATA SOURCE-Berechtigung.  

## <a name="remarks"></a>Bemerkungen  

Auf der SQL Server Instanz muss das  [polybase](../../relational-databases/polybase/polybase-guide.md) -Feature installiert sein.

Diese gespeicherte Prozedur unterstützt Connectors für:

- SQL Server
- Oracle
- Teradata
- MongoDB
- CosmosDB

Die gespeicherte Prozedur unterstützt keine generischen ODBC-DTA-quellconnectors.

Das Konzept von Empty im Vergleich zu nicht leeren bezieht sich auf das Verhalten des ODBC-Treibers und der [ `SQLTables` Funktion](../native-client-odbc-api/sqltables.md). Nicht leer gibt an, dass ein Objekt Tabellen und keine Zeilen enthält. Ein leeres Schema enthält z. b. keine Tabellen in SQL Server. Eine leere Datenbank enthält keine Tabellen in Teradata.

Objekttypen werden vom ODBC-Treiber der externen Datenquelle bestimmt. Jede externe Datenquelle bestimmt, was als Tabelle qualifiziert ist. Dies kann Datenbankobjekte wie Funktionen in Teradata oder Synonyme in Oracle einschließen. Polybase kann keine Verbindung mit einigen ODBC-Objekten als externe Tabellen herstellen und verfügt daher nicht über einen Wert in der Spalte TABLE_LOCATION. Obwohl das vorhanden sein eines dieser Objekte möglicherweise eine Datenbank oder ein Schema nicht leer ist.

Verwenden `sp_data_source_objects` [`sp_data_source_table_columns`](sp-data-source-table-columns.md) Sie und, um externe Objekte zu ermitteln. Diese gespeicherten System Prozeduren geben das Schema der Tabellen zurück, die für die Virtualisierung verfügbar sind. Azure Data Studio verwendet diese beiden gespeicherten Prozeduren, um die [Datenvirtualisierung](../../azure-data-studio/extensions/data-virtualization-extension.md)zu unterstützen. Verwenden Sie [sp_data_source_table_columns](sp-data-source-table-columns.md) , um externe Tabellen Schemas zu ermitteln, die in SQL Server Datentypen dargestellt werden.

### <a name="data-source-type-specific-remarks"></a>Spezifische Hinweise zum Daten Quellentyp

* Teradata

   Teradata-System Sichten verwenden keine Sicherheit auf Zeilenebene (Row-Level Security, RLS), sodass Benutzer das vorhanden sein von Tabellen sehen können, die nicht abgefragt werden können.

* MongoDB

   In einigen früheren Versionen von MongoDB ist die Möglichkeit eingeschränkt, alle Datenbanken für Administratoren wie Benutzer aufzulisten. Benutzer ohne diese Berechtigung können Authentifizierungsfehler erhalten, wenn Sie versuchen, diese Prozedur mit einem NULL-Wert auszuführen `@object_root_name` .

## <a name="examples"></a>Beispiele  

### <a name="sql-server"></a>SQL Server

Im folgenden Beispiel werden alle Datenbanken, Schemata und Tabellen/Sichten zurückgegeben.

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 3;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "database" | database | NULL |
| SCHEMA | "Database". dbo | dbo | NULL |
| TABLE | "Database". dbo "." End | Kunde | [Datenbank]. [dbo]. End |
| TABLE | "Database". dbo "." Position | item | [Datenbank]. [dbo]. Position |
| TABLE | "Database". dbo "." nationalen | nationalen | [Datenbank]. [dbo]. nationalen |

Im folgenden Beispiel werden alle Datenbanken zurückgegeben.

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "UserDatabase" | UserDatabase | NULL |
| DATABASE | „master“ | master | NULL |
| DATABASE | msdb | msdb | NULL |
| DATABASE | tempdb | tempdb | NULL |
| DATABASE | "database" | database | NULL |

Im folgenden Beispiel werden alle Schemata in einer Datenbank zurückgegeben.

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[database]'; 
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| SCHEMA | "Database". dbo | dbo | NULL |
| SCHEMA | "Database". INFORMATION_SCHEMA " | INFORMATION_SCHEMA | NULL |
| SCHEMA | "Database". einsetzt | sys | NULL |

Im folgenden Beispiel werden alle Tabellen im Schema zurückgegeben. 

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[database].[dbo]'; 
EXEC sp_data_source_objects @data_source, @object_root_name;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| TABLE | "Database". dbo "." End | Kunde | [Datenbank]. [dbo]. End |
| TABLE | "Database". dbo "." Position | item | [Datenbank]. [dbo]. Position |
| TABLE | "Database". dbo "." nationalen | nationalen | [Datenbank]. [dbo]. nationalen |
| TABLE | "Database". dbo "." Vergabe | Aufträge | [Datenbank]. [dbo]. Vergabe |
| TABLE | "Database". dbo "." Ostteil | -Komponente | [Datenbank]. [dbo]. Ostteil |

### <a name="oracle"></a>Oracle

Im folgenden Beispiel werden die kompletten Schemata und Tabellen, Funktionen, Sichten usw. zurückgegeben.

```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName'; 
DECLARE @object_root_name NVARCHAR(MAX) = N'[OracleObjectRoot]'; 
DECLARE @max_search_depth INT = 2; 
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| VIEW | "Sys". " ALL_SQLSET_STATEMENTS " | ALL_SQLSET_STATEMENTS | [Oracleobjectroot]. [Sys]. [ALL_SQLSET_STATEMENTS] |
| SYSTEM TABLE | "Sys". " Bootstrap $ " | Bootstrap $ | [Oracleobjectroot]. [Sys]. [Bootstrap $] |
| SYNONYM | "Public". ALL_ALL_TABLES " | ALL_ALL_TABLES | NULL |
| SCHEMA | "database" | database | NULL |
| TABLE | "Database". End | Kunde | [Oracleobjectroot]. [Datenbank]. End |

### <a name="teradata"></a>Teradata

Im folgenden Beispiel werden alle Datenbanken und Tabellen, Funktionen, Sichten usw. zurückgegeben.

```SQL
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 2;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |  
|--|--|--|--|
| FUNCTION | "Syslib". Extractroles " | Extractroles | NULL |  
| SYSTEM TABLE | "DBC". " Udtcast " | Udtcast | [DBC]. [Udtcast] |  
| TYPE | "Sysudtlib". " Basi | XML | NULL |  
| DATABASE | "database" | database | NULL |
| TABLE | "Database". End | Kunde | [Datenbank]. End |  

### <a name="mongo-db"></a>MongoDB

Im folgenden Beispiel werden alle Datenbanken und Tabellen zurückgegeben.

```SQL
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @object_root_name NVARCHAR(MAX) = NULL;
DECLARE @max_search_depth INT = 2;
EXEC sp_data_source_objects @data_source, @object_root_name, @max_search_depth;
```

| Object_Type | OBJECT_NAME | OBJECT_LEAF_NAME | TABLE_LOCATION |
|--|--|--|--|
| DATABASE | "database" | database | NULL |
| TABLE | "Database". End | Kunde | [Datenbank]. End |
| TABLE | "Database". Position | item | [Datenbank]. Position |
| TABLE | "Database". nationalen | nationalen | [Datenbank]. nationalen |
| TABLE | "Database". Vergabe | Aufträge | [Datenbank]. Vergabe |

## <a name="see-also"></a>Weitere Informationen

- [sp_data_source_columns](./sp-data-source-table-columns.md)   
- [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)
- [Datenvirtualisierungserweiterung für Azure Data Studio](../../azure-data-studio/extensions/data-virtualization-extension.md)   
- [Erste Schritte mit PolyBase](../polybase/polybase-guide.md)   
- [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)