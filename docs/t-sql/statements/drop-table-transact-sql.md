---
description: DROP TABLE (Transact-SQL)
title: DROP TABLE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_TABLE_TSQL
- DROP TABLE
dev_langs:
- TSQL
helpviewer_keywords:
- removing indexes
- table removal [SQL Server]
- deleting indexes
- dropping data
- deleting tables
- dropping indexes
- removing constraints
- removing permissions
- DROP TABLE statement
- removing tables
- deleting triggers
- removing data
- dropping tables
- deleting permissions
- dropping triggers
- deleting constraints
- removing triggers
- deleting data
- dropping constraints
- dropping permissions
ms.assetid: 0b6f2b6f-3aa3-4767-943f-43df3c3c5cfd
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ab3e6728560c563b5b3ecf4de03fd9359070f19e
ms.sourcegitcommit: c74bb5944994e34b102615b592fdaabe54713047
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90989882"
---
# <a name="drop-table-transact-sql"></a>DROP TABLE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Entfernt eine oder mehrere Tabellendefinitionen sowie alle Daten, Indizes, Trigger, Einschränkungen und Berechtigungen für diese Tabellen. Jede Sicht oder gespeicherte Prozedur, die auf die gelöschte Tabelle verweist, muss explizit mithilfe einer [DROP VIEW](../../t-sql/statements/drop-view-transact-sql.md)- oder [DROP PROCEDURE](../../t-sql/statements/drop-procedure-transact-sql.md)-Anweisung gelöscht werden. Verwenden Sie [sys.dm_sql_referencing_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md), um über die Abhängigkeiten von einer Tabelle zu berichten.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
DROP TABLE [ IF EXISTS ] { database_name.schema_name.table_name | schema_name.table_name | table_name } [ ,...n ]  
[ ; ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *database_name*  
 Name der Datenbank, in der die Tabelle erstellt wurde.  
  
 Die Azure SQL-Datenbank unterstützt das aus drei Teilen bestehende Format „database_name.[schema_name].object_name“, wenn „database_name“ die aktuelle Datenbank bzw. „database_name tempdb“ ist und „object_name“ mit „#“ beginnt. Die Azure SQL-Datenbank unterstützt keine vierteiligen Namen.  
  
 *IF EXISTS*  
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] bis zur [aktuellen Version](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Löscht die Tabelle nur, wenn diese bereits vorhanden ist.  
  
 *schema_name*  
 Der Name des Schemas, zu dem die Tabelle gehört.  
  
 *table_name*  
 Name der Tabelle, die entfernt werden soll  
  
## <a name="remarks"></a>Bemerkungen  
 Die DROP TABLE-Anweisung kann nicht zum Löschen einer Tabelle verwendet werden, auf die mit einer FOREIGN KEY-Einschränkung verwiesen wird. Die verweisende FOREIGN KEY-Einschränkung oder die verweisende Tabelle muss zuerst gelöscht werden. Wenn sowohl die verweisende Tabelle als auch die Tabelle mit dem Primärschlüssel in derselben DROP TABLE-Anweisung gelöscht werden, muss die verweisende Tabelle zuerst aufgelistet werden.  
  
 Mehrere Tabellen können in jeder beliebigen Datenbank gelöscht werden. Verweist eine zu löschende Tabelle auf den Primärschlüssel einer anderen, ebenfalls zu löschenden Tabelle, muss die verweisende Tabelle mit dem Fremdschlüssel vor der Tabelle mit dem Primärschlüssel, auf den verwiesen wird, aufgelistet werden.  
  
 Wird eine Tabelle gelöscht, werden alle Bindungen von Regeln und Standardwerten zur Tabelle entfernt, und alle der Tabelle zugeordneten Einschränkungen und Trigger werden automatisch gelöscht. Wenn Sie die Tabelle neu erstellen, müssen Sie auch die entsprechenden Regeln und Standardwerte neu binden, die Trigger neu erstellen und alle erforderlichen Einschränkungen hinzufügen.  
  
 Wenn Sie alle Zeilen einer Tabelle löschen (DELETE *Tabellenname*) oder die TRUNCATE TABLE-Anweisung verwenden, ist die Tabelle so lange vorhanden, bis sie gelöscht wird.  
  
 Umfangreiche Tabellen und Indizes mit mehr als 128 Blöcken werden in zwei getrennten Phasen gelöscht: in der logischen und in der physischen Phase. In der logischen Phase werden die von der Tabelle verwendeten vorhandenen Zuordnungseinheiten für die Aufhebung der Zuordnungen markiert und bis zum Commit der Transaktion gesperrt. In der physischen Phase werden die für die Zuordnungsaufhebung markierten IAM-Seiten in Batches physisch gelöscht.  
  
 Wenn Sie eine Tabelle löschen, die eine VARBINARY(MAX)-Spalte mit dem FILESTREAM-Attribut enthält, werden alle im Dateisystem gespeicherten Daten nicht entfernt.  
  
> [!IMPORTANT]  
>  DROP TABLE und CREATE TABLE dürfen nicht in der gleichen Tabelle im gleichen Batch ausgeführt werden. Andernfalls tritt möglicherweise ein unerwarteter Fehler auf.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die ALTER-Berechtigung für das Schema, zu dem die Tabelle gehört, die CONTROL-Berechtigung für die Tabelle oder die Mitgliedschaft in der festen Datenbankrolle **db_ddladmin** .  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-dropping-a-table-in-the-current-database"></a>A. Ablegen einer Tabelle in der aktuellen Datenbank  
 Im folgenden Beispiel werden die `ProductVendor1`-Tabelle, ihre Daten und ihre Indizes aus der aktuellen Datenbank entfernt.  
  
```sql  
DROP TABLE ProductVendor1 ;  
```  
  
### <a name="b-dropping-a-table-in-another-database"></a>B. Ablegen einer Tabelle in einer anderen Datenbank  
 Im folgenden Beispiel wird die `SalesPerson2`-Tabelle in der [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank gelöscht. Das Beispiel kann aus jeder Datenbank auf der Serverinstanz heraus ausgeführt werden.  
  
```sql  
DROP TABLE AdventureWorks2012.dbo.SalesPerson2 ;  
```  
  
### <a name="c-dropping-a-temporary-table"></a>C. Ablegen einer temporären Tabelle  
 Im folgenden Beispiel wird eine temporäre Tabelle erstellt, überprüft, ob sie vorhanden ist, die Tabelle gelöscht und erneut überprüft, ob sie vorhanden ist. In diesem Beispiel wird die **IF EXISTS**-Syntax, die verfügbar ist, wenn mit [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] begonnen wird, nicht verwendet.  
  
```sql  
CREATE TABLE #temptable (col1 INT);  
GO  
INSERT INTO #temptable  
VALUES (10);  
GO  
SELECT * FROM #temptable;  
GO  
IF OBJECT_ID(N'tempdb..#temptable', N'U') IS NOT NULL   
DROP TABLE #temptable;  
GO  
--Test the drop.  
SELECT * FROM #temptable;  
```  
  
### <a name="d-dropping-a-table-using-if-exists"></a>D: Löschen einer Tabelle mit IF EXISTS  
  
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] bis zur [aktuellen Version](https://go.microsoft.com/fwlink/p/?LinkId=299658)).  
  
 Im folgenden Beispiel wird eine Tabelle mit dem Namen „T1“ erstellt. Die zweite Anweisung legt dann die Tabelle ab. Die dritte Anweisung führt keine Aktion aus, da die Tabelle bereits gelöscht wurde, führt allerdings auch nicht zu einer Fehlermeldung.  
  
```sql  
CREATE TABLE T1 (Col1 INT);  
GO  
DROP TABLE IF EXISTS T1;  
GO  
DROP TABLE IF EXISTS T1;  
```  
  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [TRUNCATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/truncate-table-transact-sql.md)   
 [DROP VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/drop-view-transact-sql.md)   
 [DROP PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-procedure-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
