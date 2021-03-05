---
description: RENAME (Transact-SQL)
title: RENAME (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/21/2019
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
ms.assetid: 0907cfd9-33a6-4fa6-91da-7d6679fee878
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 70c72d0d9b884431a314290de4c4514c720e85ee
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186480"
---
# <a name="rename-transact-sql"></a>RENAME (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

Benennt eine vom Benutzer erstellte Tabelle in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] um. Benennt eine vom Benutzer erstellte Tabelle, eine Spalte in einer vom Benutzer erstellten Tabelle oder eine Datenbank in [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] um.

> [!NOTE]
> Verwenden Sie [ALTER DATABASE ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true), um eine Datenbank in [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] umzubenennen. Um eine Datenbank in Azure SQL-Datenbank umzubenennen, verwenden Sie die Anweisung [ALTER DATABASE (Azure SQL-Datenbank)](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true). Verwenden Sie die gespeicherte Prozedur [sp_renamedb](../../relational-databases/system-stored-procedures/sp-renamedb-transact-sql.md), um eine Datenbank in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] umzubenennen.

## <a name="syntax"></a>Syntax

```syntaxsql
-- Syntax for Azure Synapse Analytics

-- Rename a table.
RENAME OBJECT [::] [ [ database_name . [schema_name ] ] . ] | [schema_name . ] ] table_name TO new_table_name
[;]

```

```syntaxsql
-- Syntax for Analytics Platform System

-- Rename a table
RENAME OBJECT [::] [ [ database_name . [ schema_name ] . ] | [ schema_name . ] ] table_name TO new_table_name
[;]

-- Rename a database
RENAME DATABASE [::] database_name TO new_database_name
[;]

-- Rename a column 
RENAME OBJECT [::] [ [ database_name . [schema_name ] ] . ] | [schema_name . ] ] table_name COLUMN column_name TO new_column_name [;]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argumente

RENAME OBJECT [::] [ [*database_name* . [ *schema_name* ] . ] | [ *schema_name* . ] ]*table_name* TO *new_table_name*
**GILT FÜR:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Ändern Sie den Namen einer vom Benutzer erstellten Tabelle. Geben Sie die Tabelle an, die mit einem ein-, zwei-, oder dreiteiligen Namen umbenannt werden soll. Geben Sie die neue Tabelle *new_table_name* als einteiligen Namen an.

RENAME DATABASE [::] [ *Datenbankname* TO *neuer_Datenbankname*
**GILT FÜR:** [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Ändern Sie den Namen einer benutzerdefinierten Datenbank von *database_name* in *new_database_name*. Sie können bei der Umbenennung der Datenbank keine der folgenden [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]reservierten Datenbanknamen verwenden:

- master
- model
- msdb
- tempdb
- pdwtempdb1
- pdwtempdb2
- DWConfiguration
- DWDiagnostics
- DWQueue


RENAME OBJECT [::] [ [*database_name* . [ *schema_name* ] . ] | [ *schema_name* . ] ]*table_name* COLUMN *column_name* TO *new_column_name*
**GILT FÜR:** [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Ändert den Namen einer Spalte in einer Tabelle. 

## <a name="permissions"></a>Berechtigungen

Zur Ausführung dieses Befehls benötigen Sie folgende Berechtigung:

- **ALTER**-Berechtigung für die Tabelle

## <a name="limitations-and-restrictions"></a>Einschränkungen

### <a name="cannot-rename-an-external-table-indexes-or-views"></a>Externe Tabellen, Indizes oder Sichten können nicht umbenannt werden

Sie können keine externen Tabellen, Indizes oder Sichten umbenennen. Anstatt sie umzubenennen können Sie die externe Tabelle, den Index oder die Sicht löschen und dann mit dem neuen Namen erneut erstellen.

### <a name="cannot-rename-a-table-in-use"></a>Verwendete Tabelle kann nicht umbenannt werden

Sie können keine Tabellen oder Datenbanken umbenennen, während diese verwendet werden. Das Umbenennen einer Tabelle erfordert eine exklusive Sperre für die Tabelle. Wenn die Tabelle verwendet wird, müssen Sie möglicherweise die Sitzungen beenden, die die Tabelle verwenden. Zum Beenden einer Sitzung können Sie den KILL-Befehl nutzen. Verwenden Sie den KILL-Befehl mit Bedacht, da für nicht per Commit festgeschriebene Arbeit ein Rollback ausgeführt wird, wenn eine Sitzung beendet wird. Sitzungen in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] wird das Präfix „SID“ vorangestellt. Geben Sie 'SID' und die Sitzungsnummer ein, wenn Sie den KILL-Befehl aufrufen. Dieses Beispiel zeigt eine Liste der aktiven oder im Leerlauf befindlichen Sitzungen an und beendet dann die Sitzung „SID1234“.

### <a name="rename-column-restrictions"></a>Einschränkungen für das Umbenennen von Spalten

Eine Spalte, die für die Tabellenverteilung verwendet wird, kann nicht umbenannt werden. Es können auch keine Spalten in einer externen oder temporären Tabelle umbenannt werden. 

### <a name="views-are-not-updated"></a>Sichten werden nicht aktualisiert

Wenn Sie eine Datenbank umbenennen, werden alle Sichten ungültig, die den vorherigen Datenbanknamen verwenden. Dieses Verhalten gilt für Sichten innerhalb und außerhalb der Datenbank. Wenn z.B. die Sales-Datenbank umbenannt wird, wird eine Sicht ungültig, die `SELECT * FROM Sales.dbo.table1` enthält. Sie können dieses Problem beheben, indem Sie entweder keine dreiteiligen Namen in Sichten verwenden oder die Sichten aktualisieren, damit diese auf den neuen Datenbanknamen verweisen.

Beim Umbenennen einer Tabelle werden Sichten nicht aktualisiert, damit diese auf den neuen Tabellennamen verweisen. Jede Sicht (innerhalb oder außerhalb der Datenbank), die auf den vorherigen Tabellennamen verweist, wird ungültig. Zum Beheben dieses Problems können Sie alle Sichten aktualisieren, damit sie auf den neuen Tabellennamen verweisen.

Beim Umbenennen einer Spalte werden Sichten nicht auf den neuen Spaltennamen aktualisiert. Sichten zeigen so lange den alten Spaltennamen an, bis eine ALTER VIEW-Anweisung ausgeführt wird. In bestimmten Fällen können Sichten ungültig werden und müssen gelöscht und neu erstellt werden.

## <a name="locking"></a>Sperren

Das Umbenennen einer Tabelle erfordert eine gemeinsame Sperre für das DATABASE-Objekt und das SCHEMA-Objekt sowie eine exklusive Sperre für die Tabelle.

## <a name="examples"></a>Beispiele

### <a name="a-rename-a-database"></a>A. Umbenennen einer Datenbank

**GILT NUR FÜR:** [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

In diesem Beispiel wird die benutzerdefinierte Datenbank AdWorks in AdWorks2 umbenannt.

```sql
-- Rename the user defined database AdWorks
RENAME DATABASE AdWorks to AdWorks2;

```

 Beim Umbenennen einer Tabelle werden alle Objekte und Eigenschaften aktualisiert, die der Tabelle zugeordnet sind, damit diese auf den neuen Tabellennamen verweisen. Es werden z.B. Tabellendefinitionen, Indizes, Einschränkungen und Berechtigungen aktualisiert. Sichten werden nicht aktualisiert.

### <a name="b-rename-a-table"></a>B. Umbenennen einer Tabelle

**GILT FÜR:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

In diesem Beispiel wird die Tabelle Customer in Customer1 umbenannt.

```sql
-- Rename the customer table
RENAME OBJECT Customer TO Customer1;

RENAME OBJECT mydb.dbo.Customer TO Customer1;
```

Beim Umbenennen einer Tabelle werden alle Objekte und Eigenschaften aktualisiert, die der Tabelle zugeordnet sind, damit diese auf den neuen Tabellennamen verweisen. Es werden z.B. Tabellendefinitionen, Indizes, Einschränkungen und Berechtigungen aktualisiert. Sichten werden nicht aktualisiert.

### <a name="c-move-a-table-to-a-different-schema"></a>C. Verschieben einer Tabelle in ein anderes Schema

**GILT FÜR:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

Wenn Sie beabsichtigen, das Objekt in ein anderes Schema zu verschieben, verwenden Sie [ALTER SCHEMA](../../t-sql/statements/alter-schema-transact-sql.md). Durch die folgende Anweisung wird z.B. das Tabellenelement aus dem product-Schema in das dbo-Schema verschoben.

```sql
ALTER SCHEMA dbo TRANSFER OBJECT::product.item;
```

### <a name="d-terminate-sessions-before-renaming-a-table"></a>D: Beenden von Sitzungen vor dem Umbenennen einer Tabelle

**GILT FÜR:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

Eine Tabelle kann nicht umbenannt werden, während sie verwendet wird. Das Umbenennen einer Tabelle erfordert eine exklusive Sperre für die Tabelle. Wenn die Tabelle verwendet wird, müssen Sie möglicherweise die Sitzung beenden, die die Tabelle verwendet. Zum Beenden einer Sitzung können Sie den KILL-Befehl nutzen. Verwenden Sie den KILL-Befehl mit Bedacht, da für nicht per Commit festgeschriebene Arbeit ein Rollback ausgeführt wird, wenn eine Sitzung beendet wird. Sitzungen in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] wird das Präfix „SID“ vorangestellt. Sie müssen beim Aufruf des KILL-Befehls 'SID' und die Sitzungsnummer eingeben. Dieses Beispiel zeigt eine Liste der aktiven oder im Leerlauf befindlichen Sitzungen an und beendet dann die Sitzung „SID1234“.

```sql
-- View a list of the current sessions
SELECT session_id, login_name, status
FROM sys.dm_pdw_exec_sessions
WHERE status='Active' OR status='Idle';

-- Terminate a session using the session_id.
KILL 'SID1234';
```

### <a name="e-rename-a-column"></a>E. Umbenennen einer Spalte 

**GILT FÜR:** [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

In diesem Beispiel wird die Spalte „FName“ der Tabelle „Customer“ in „FirstName“ umbenannt.

```sql
-- Rename the Fname column of the customer table
RENAME OBJECT::Customer COLUMN FName TO FirstName;

RENAME OBJECT mydb.dbo.Customer COLUMN FName TO FirstName;
```
