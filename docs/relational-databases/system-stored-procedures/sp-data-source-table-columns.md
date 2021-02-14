---
title: sp_data_source_table_columns
description: sp_data_source_table_columns (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sp_data_source_table_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase
author: MikeRayMSFT
ms.author: mikeray
ms.custom: ''
ms.date: 11/10/2020
ms.openlocfilehash: 31b0dedb8544cc6752eac8a9934aa92d74d424ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081871"
---
# <a name="sp_data_source_table_columns-transact-sql"></a>sp_data_source_table_columns (Transact-SQL)

[!INCLUDE [sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

Gibt die Liste der Spalten in der externen Datenquellen Tabelle zurück.
  
> [!NOTE]
> Dieses Verfahren wird in [SQL 2019 CU5](../../big-data-cluster/release-notes-big-data-cluster.md#cu5)eingeführt.

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```sqlsyntax
sp_data_source_table_columns
         [ @data_source = ] 'data_source'
       , [ @table_location = ] 'table_location'
[ ; ]
```  

## <a name="arguments"></a>Argumente

`[ @data_source = ] 'data_source'`   
Der Name der externen Datenquelle, von der die Metadaten abgeleitet werden sollen. Der Typ ist `sysname` .

`[ @table_location = ] 'table_location'`   
Die Tabellen Speicherort-Zeichenfolge, die die Tabelle identifiziert. `table_location` der Typ ist `nvarchar(max)` .

## <a name="returns"></a>Gibt zurück

Die gespeicherte Prozedur gibt die folgenden Informationen zurück:

|Spaltenname |Datentyp |BESCHREIBUNG|
|---|---|---|
|NAME|nvarchar(max)|Der Name der Spalte.
|TYPE|nvarchar(200)|SQL Server Typname
|LENGTH|INT|Länge der Spalte
|PRECISION|INT|Genauigkeit der Spalte
|SCALE|INT|Skala der Spalte
|COLLATION|nvarchar(200)|SQL Server Sortierung der Spalte
|IS_NULLABLE|bit|Ist diese Spalte NULL-Werte zulässig.
|SOURCE_TYPE_NAME|nvarchar(max)|Der Back-End-spezifische Typname. Wird größtenteils zum Debuggen verwendet. Für ODBC-Quellen entspricht dies der TYPE_NAME Ergebnisspalte für SQLColumns ().
|ANMERKUNGEN|nvarchar(max)|Allgemeine Kommentare oder eine Beschreibung der Spalte. Zurzeit immer NULL.|

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

Das Konzept von Empty im Vergleich zu nicht leeren bezieht sich auf das Verhalten des ODBC-Treibers und der [ `SQLTables` Funktion](../native-client-odbc-api/sqltables.md). Nicht leer gibt an, dass ein Objekt Tabellen und keine Zeilen enthält. Ein leeres Schema enthält z. b. keine Tabellen in SQL Server. Eine leere Datenbank enthält ohne Tabellen in Teradata. die Ergebnisse sind eine SQL Server Darstellung des Back-End-Schemas, wie vom polybase-Connector für das Back-End interpretiert. Der Unterschied besteht darin, dass anstatt lediglich die Ergebnisse des ODBC-Aufrufes an das Back-End zu übergeben, die Ergebnisse auf dem Ergebnis des polybase-Typmapping-Codes basieren.

Verwenden [`sp_data_source_objects`](sp-data-source-objects.md) `sp_data_source_table_columns` Sie und, um externe Objekte zu ermitteln. Diese gespeicherten System Prozeduren geben das Schema der Tabellen zurück, die für die Virtualisierung verfügbar sind. Azure Data Studio verwendet diese beiden gespeicherten Prozeduren, um die [Datenvirtualisierung](../../azure-data-studio/extensions/data-virtualization-extension.md)zu unterstützen. Verwenden Sie `sp_data_source_table_columns` , um externe Tabellen Schemas zu ermitteln, die in SQL Server Datentypen dargestellt werden.

Aufgrund von Unterschieden zwischen Sortierungen in Hadoop-Quelldaten und unterstützten Sortierungen in SQL Server 2019 sind die empfohlenen Datentyp Längen für varchar-Datentyp Spalten in externen Tabellen möglicherweise weitaus größer als erwartet. Dies ist beabsichtigt.

## <a name="example"></a>Beispiel  

Im folgenden Beispiel werden die Tabellen Spalten für eine externe Tabelle in einer SQL Server mit dem Namen zurückgegeben `server` , die zu einem Schema namens gehört `schema` .
  
```sql
DECLARE @data_source SYSNAME = N'ExternalDataSourceName';
DECLARE @table_location NVARCHAR(400) = N'[database].[schema].[table]';
EXEC sp_data_source_table_columns @data_source, @table_location
```  
  
## <a name="see-also"></a>Weitere Informationen

- [Erste Schritte mit PolyBase](../polybase/polybase-guide.md)
- [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT (Transact-SQL)](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md)