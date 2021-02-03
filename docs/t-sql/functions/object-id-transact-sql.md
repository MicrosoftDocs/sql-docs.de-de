---
description: OBJECT_ID (Transact-SQL)
title: OBJECT_ID (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OBJECT_ID
- OBJECT_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], IDs
- identification numbers [SQL Server], database objects
- checking object exists
- IDs [SQL Server], database objects
- OBJECT_ID function
- database objects [SQL Server], IDs
- displaying object IDs
- viewing object IDs
- verifying object exists
ms.assetid: f89286db-440f-4218-a828-30881ce3077a
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0d3e0cac16d1a3d7f180117dc29b20fea8e53276
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189916"
---
# <a name="object_id-transact-sql"></a>OBJECT_ID (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt die Datenbankobjekt-ID eines Objekts mit Schemabereich zurück.  
  
> [!IMPORTANT]  
>  Objekte, die keine Schemas als Bereiche besitzen, z. B. DDL-Trigger, können nicht mit OBJECT_ID abgefragt werden. Rufen Sie für Objekte, die nicht in der [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)-Katalogsicht gefunden werden, die Objekt-IDs ab, indem Sie die entsprechende Katalogsicht abfragen. Verwenden Sie z.B. `SELECT OBJECT_ID FROM sys.triggers WHERE name = 'DatabaseTriggerLog``'`, um die Objekt-ID eines DDL-Triggers zurückzugeben.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
OBJECT_ID ( '[ database_name . [ schema_name ] . | schema_name . ]   
  object_name' [ ,'object_type' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 **'** *object_name* **'**  
 Das zu verwendende Objekt. *object_name* entspricht entweder dem Typ **varchar** oder **nvarchar**. Wenn *object_name* dem Typ **varchar** entspricht, wird eine implizite Konvertierung in **nvarchar** vorgenommen. Die Angabe des Datenbank- und des Schemanamens ist optional.  
  
 **'** *object_type* **'**  
 Der Objekttyp mit Schemabereich. *object_type* entspricht entweder dem Typ **varchar** oder **nvarchar**. Wenn *object_type* dem Typ **varchar** entspricht, wird eine implizite Konvertierung in **nvarchar** vorgenommen. Eine Liste von Objekttypen finden Sie in der Spalte **Typ** unter [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).  
  
## <a name="return-types"></a>Rückgabetypen  
 **int**  
  
## <a name="exceptions"></a>Ausnahmen  
 Für einen räumlichen Index gibt OBJECT_ID den Wert NULL zurück.  
  
 Gibt bei einem Fehler NULL zurück.  
  
 Ein Benutzer kann nur die Metadaten sicherungsfähiger Elemente anzeigen, bei denen der Benutzer entweder der Besitzer ist oder für die dem Benutzer eine Berechtigung erteilt wurde. Dies bedeutet, dass integrierte Funktionen (z. B. OBJECT_ID), die Metadaten ausgeben, möglicherweise NULL zurückgeben, wenn dem Benutzer für das Objekt keine Berechtigung erteilt wurde. Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn der Parameter für eine Systemfunktion optional ist, wird von der aktuellen Datenbank, dem aktuellen Hostcomputer, dem aktuellen Serverbenutzer oder dem aktuellen Datenbankbenutzer ausgegangen. Auf integrierte Funktionen müssen immer runde Klammern folgen.  
  
 Wenn der Name einer temporären Tabelle angegeben wird, muss der Datenbankname vor diesem angegeben werden, es sei denn, die aktuelle Datenbank ist **tempdb**. Beispiel: `SELECT OBJECT_ID('tempdb..#mytemptable')`.  
  
 Systemfunktionen können in der SELECT-Liste, in einer WHERE-Klausel und überall dort verwendet werden, wo ein Ausdruck zulässig ist. Weitere Informationen finden Sie unter [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md) und [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md).  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-returning-the-object-id-for-a-specified-object"></a>A. Zurückgeben der Objekt-ID für ein angegebenes Objekt  
 Das folgende Beispiel gibt die Objekt-ID für die `Production.WorkOrder`-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank zurück.  
  
```sql  
USE master;  
GO  
SELECT OBJECT_ID(N'AdventureWorks2012.Production.WorkOrder') AS 'Object ID';  
GO  
```  
  
### <a name="b-verifying-that-an-object-exists"></a>B. Überprüfen, ob ein Objekt vorhanden ist  
 Das folgende Beispiel überprüft das Vorhandensein einer angegebenen Tabelle, indem überprüft wird, ob die Tabelle eine Objekt-ID besitzt. Wenn die Tabelle vorhanden ist, wird sie gelöscht. Ist die Tabelle nicht vorhanden, wird die `DROP TABLE`-Anweisung nicht ausgeführt.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'dbo.AWBuildVersion', N'U') IS NOT NULL  
DROP TABLE dbo.AWBuildVersion;  
GO  
```  
  
### <a name="c-using-object_id-to-specify-the-value-of-a-system-function-parameter"></a>C. Angeben des Werts eines Systemfunktionsparameters mithilfe von OBJECT_ID  
 Im folgenden Beispiel werden Informationen zu allen Indizes und Partitionen der `Person.Address`-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank zurückgegeben. Dazu wird die [sys.dm_db_index_operational_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)-Funktion verwendet.  
 
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
> [!IMPORTANT]  
>  Wenn Sie die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen DB_ID und OBJECT_ID zum Zurückgeben eines Parameterwerts verwenden, sollten Sie immer sicherstellen, dass eine gültige ID zurückgegeben wird. Wenn der Datenbank- oder Objektname nicht gefunden werden kann, wenn sie z. B. nicht vorhanden oder fehlerhaft geschrieben sind, geben beide Funktionen NULL zurück. Die **sys.dm_db_index_operational_stats**-Funktion interpretiert NULL als Platzhalterwert, der alle Datenbanken oder alle Objekte angibt. Da dies ein versehentlicher Vorgang sein kann, veranschaulichen die Beispiele in diesem Abschnitt, wie Sie auf sichere Weise Datenbank- und Objekt-IDs bestimmen.
  
```sql  
DECLARE @db_id INT;  
DECLARE @object_id INT;  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');  
IF @db_id IS NULL   
  BEGIN;  
    PRINT N'Invalid database';  
  END;  
ELSE IF @object_id IS NULL  
  BEGIN;  
    PRINT N'Invalid object';  
  END;  
ELSE  
  BEGIN;  
    SELECT * FROM sys.dm_db_index_operational_stats(@db_id, @object_id, NULL, NULL);  
  END;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-returning-the-object-id-for-a-specified-object"></a>D: Zurückgeben der Objekt-ID für ein angegebenes Objekt  
 Das folgende Beispiel gibt die Objekt-ID für die `FactFinance`-Tabelle in der [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)]-Datenbank zurück.  
  
```sql  
SELECT OBJECT_ID('AdventureWorksPDW2012.dbo.FactFinance') AS 'Object ID';  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Metadata Functions &#40;Transact-SQL&#41; (Metadatenfunktionen &#40;Transact-SQL&#41;)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)   
 [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md)   
 [OBJECT_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/object-name-transact-sql.md)  
  
  

