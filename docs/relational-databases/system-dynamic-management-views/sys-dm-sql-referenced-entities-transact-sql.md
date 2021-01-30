---
description: sys.dm_sql_referenced_entities (Transact-SQL)
title: sys.dm_sql_referenced_entities (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_sql_referenced_entities_TSQL
- dm_sql_referenced_entities
- sys.dm_sql_referenced_entities
- sys.dm_sql_referenced_entities_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_sql_referenced_entities dynamic management function
ms.assetid: 077111cb-b860-4d61-916f-bac5d532912f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1ed4aebeaf422b47dd94531c5d0a8a491cf9490e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203338"
---
# <a name="sysdm_sql_referenced_entities-transact-sql"></a>sys.dm_sql_referenced_entities (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Gibt eine Zeile für jede benutzerdefinierte Entität zurück, auf die nach Namen in der Definition der angegebenen verweisenden Entität in verwiesen wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Eine Abhängigkeit zwischen zwei Entitäten wird erstellt, wenn eine benutzerdefinierte Entität, die als *Referenzierte Entität* bezeichnet wird, in einem permanenten SQL-Ausdruck einer anderen benutzerdefinierten Entität, die als *verweisende Entität* bezeichnet wird, nach Namen angezeigt wird. Handelt es sich beispielsweise bei einer gespeicherten Prozedur um die angegebene verweisende Entität, gibt diese Funktion alle benutzerdefinierten Entitäten zurück, auf die die gespeicherte Prozedur verweist, z. B. Tabellen, Sichten, benutzerdefinierte Typen (UDTs) oder andere gespeicherte Prozeduren.  
  
 Verwenden Sie diese dynamische Verwaltungsfunktion, um zu folgenden Entitätstypen, auf die in der verweisenden Entität verwiesen wird, einen Bericht zu erstellen.  
  
-   Schemagebundene Entitäten  
  
-   Nicht schemagebundene Entitäten  
  
-   Datenbankübergreifende und serverübergreifende Entitäten  
  
-   Abhängigkeiten auf Spaltenebene von schemagebundenen und nicht schemagebundenen Entitäten  
  
-   Benutzerdefinierte Typen (Alias und CLR UDT)  
  
-   XML-Schemaauflistungen  
  
-   Partitionsfunktionen  

## <a name="syntax"></a>Syntax  
  
```  
sys.dm_sql_referenced_entities (  
    ' [ schema_name. ] referencing_entity_name ' ,
    ' <referencing_class> ' )  
  
<referencing_class> ::=  
{  
    OBJECT  
  | DATABASE_DDL_TRIGGER  
  | SERVER_DDL_TRIGGER  
}  
```  
  
## <a name="arguments"></a>Argumente  
 [ *schema_name*. ] *referencing_entity_name*  
 Der Name der verweisenden Entität. *schema_name* ist erforderlich, wenn die verweisende Klasse Object ist.  
  
 *schema_name. referencing_entity_name* ist vom Datentyp **nvarchar (517)**.  
  
 *<referencing_class>* :: = {Object | DATABASE_DDL_TRIGGER | SERVER_DDL_TRIGGER}  
 Klasse der angegebenen verweisenden Entität. Pro Anweisung kann nur eine Klasse angegeben werden.  
  
 *<referencing_class>* ist vom Datentyp **nvarchar (60)**.  
  
## <a name="table-returned"></a>Zurückgegebene Tabelle  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|referencing_minor_id|**int**|Die Spalten-ID, wenn es sich bei der verweisenden Entität um eine Spalte handelt. Andernfalls ist der Wert 0. Lässt keine NULL-Werte zu.|  
|referenced_server_name|**sysname**|Servername der Entität, auf die verwiesen wird.<br /><br /> Diese Spalte wird für serverübergreifende Abhängigkeiten aufgefüllt, die auf der Angabe eines gültigen vierteiligen Namens basieren. Weitere Informationen zu mehrteiligen Namen finden Sie unter [Transact-SQL-Syntax Konventionen &#40;Transact-SQL-&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).<br /><br /> NULL für nicht schemagebundene Abhängigkeiten, für die ohne Angabe eines vierteiligen Namens auf die Entität verwiesen wurde.<br /><br /> NULL für Schema gebundene Entitäten, da Sie sich in derselben Datenbank befinden müssen und deshalb nur mit einem zweiteiligen Namen (*Schema. Object*) definiert werden können.|  
|referenced_database_name|**sysname**|Datenbankname der Entität, auf die verwiesen wird.<br /><br /> Diese Spalte wird für datenbankübergreifende oder serverübergreifende Verweise aufgefüllt, die auf der Angabe eines gültigen dreiteiligen oder vierteiligen Namens basieren.<br /><br /> NULL für nicht schemagebundene Verweise, wenn auf Basis eines einteiligen oder zweiteiligen Namens angegeben.<br /><br /> NULL für Schema gebundene Entitäten, da Sie sich in derselben Datenbank befinden müssen und deshalb nur mit einem zweiteiligen Namen (*Schema. Object*) definiert werden können.|  
|referenced_schema_name|**sysname**|Schema, in das die Entität gehört, auf die verwiesen wird.<br /><br /> NULL für nicht schemagebundene Verweise, in denen ohne Angabe des Schemanamens auf die Entität verwiesen wird.<br /><br /> Niemals NULL für schemagebundene Verweise.|  
|referenced_entity_name|**sysname**|Name der Entität, auf die verwiesen wird. Lässt keine NULL-Werte zu.|  
|referenced_minor_name|**sysname**|Der Spaltenname, wenn es sich bei der Entität, auf die verwiesen wird, um eine Spalte handelt. Andernfalls ist der Wert NULL. Beispielsweise hat referenced_minor_name den Wert NULL in der Zeile, in der die Entität, auf die verwiesen wird, selbst aufgeführt wird.<br /><br /> Eine Entität, auf die verwiesen wird, ist eine Spalte, wenn diese in der verweisenden Entität namentlich identifiziert wird oder wenn die übergeordnete Entität in einer SELECT *-Anweisung verwendet wird.|  
|referenced_id|**int**|ID der Entität, auf die verwiesen wird. Wenn referenced_minor_id nicht 0 ist, ist referenced_id die Entität, in der die Spalte definiert wird.<br /><br /> Immer NULL für serverübergreifende Verweise.<br /><br /> NULL für datenbankübergreifende Verweise, wenn die ID nicht bestimmt werden kann, weil die Datenbank offline ist oder die Entität nicht gebunden werden kann.<br /><br /> NULL für Verweise innerhalb der Datenbank, wenn die ID nicht bestimmt werden kann. Für nicht Schema gebundene Verweise kann die ID nicht aufgelöst werden, wenn die Entität, auf die verwiesen wird, nicht in der Datenbank vorhanden ist oder wenn die Namensauflösung vom Aufrufer abhängig ist.  Im letzteren Fall wird is_caller_dependent auf 1 festgelegt.<br /><br /> Niemals NULL für schemagebundene Verweise.|  
|referenced_minor_id|**int**|Die Spalten-ID, wenn es sich bei der Entität, auf die verwiesen wird, um eine Spalte handelt. Andernfalls ist der Wert 0. Beispielsweise hat referenced_minor_is den Wert NULL in der Zeile, in der die Entität, auf die verwiesen wird, selbst aufgeführt wird.<br /><br /> Für nicht schemagebundene Verweise werden Spaltenabhängigkeiten nur gemeldet, wenn alle Entitäten, auf die verwiesen wird, gebunden werden können. Kann eine der Entitäten, auf die verwiesen wird, nicht gebunden werden, werden keine Abhängigkeiten auf Spaltenebene gemeldet, und referenced_minor_id wird auf 0 gesetzt. Siehe Beispiel D.|  
|referenced_class|**tinyint**|Klasse der Entität, auf die verwiesen wird.<br /><br /> 1 = Objekt oder Spalte<br /><br /> 6 = Typ<br /><br /> 10 = XML-Schemaauflistung<br /><br /> 21 = Partitionsfunktion|  
|referenced_class_desc|**nvarchar(60)**|Klassenbeschreibung der Entität, auf die verwiesen wird.<br /><br /> OBJECT_OR_COLUMN<br /><br /> TYPE<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> PARTITION_FUNCTION|  
|is_caller_dependent|**bit**|Gibt an, dass die Schemabindung für die Entität, auf die verwiesen wird, zur Laufzeit erfolgt. Deshalb hängt die Auflösung der Entitäts-ID vom Schema des Aufrufers ab. Dies ist der Fall, wenn es sich bei der Entität, auf die verwiesen wird, um eine gespeicherte Prozedur, eine erweiterte gespeicherte Prozedur oder um eine benutzerdefinierte Funktion handelt, die in einer EXECUTE-Anweisung aufgerufen wird.<br /><br /> 1 = Die Entität, auf die verwiesen wird, hängt vom Aufrufer ab und wird zur Laufzeit aufgelöst. In diesem Fall ist referenced_id gleich NULL.<br /><br /> 0 = Die Entitäts-ID, auf die verwiesen wird, ist nicht aufruferabhängig. Immer 0 für schemagebundene Verweise sowie für datenbankübergreifende und serverübergreifende Verweise, die explizit einen Schemanamen angeben. Zum Beispiel ist ein Verweis auf eine Entität im Format `EXEC MyDatabase.MySchema.MyProc` nicht aufruferabhängig. Ein Verweis im Format `EXEC MyDatabase..MyProc` ist jedoch aufruferabhängig.|  
|is_ambiguous|**bit**|Gibt an, dass der Verweis mehrdeutig ist und zur Laufzeit in eine benutzerdefinierte Funktion, in einen benutzerdefinierten Typ (User-Defined Type, UDT) oder in einen XQuery-Verweis auf eine Spalte vom Typ **XML** aufgelöst werden kann. Angenommen, die `SELECT Sales.GetOrder() FROM Sales.MySales`-Anweisung ist in einer gespeicherten Prozedur definiert. Bis zur Ausführung der gespeicherten Prozedur ist unbekannt, ob `Sales.GetOrder()` eine benutzerdefinierte Funktion im Schema `Sales` oder in der Spalte namens `Sales` vom Typ UDT mit einer Methode namens `GetOrder()` ist.<br /><br /> 1 = Verweis auf eine benutzerdefinierte Funktion oder Spalte, für die die benutzerdefinierte Typmethode (UDT) mehrdeutig ist.<br /><br /> 0 = Verweis ist eindeutig, oder die Entität kann beim Aufruf der Funktion erfolgreich gebunden werden.<br /><br /> Immer 0 für schemagebundene Verweise.|  
|is_selected|**bit**|1 = Gibt an, dass das Objekt oder die Spalte ausgewählt ist.|  
|is_updated|**bit**|1 = Gibt an, dass das Objekt oder die Spalte geändert wurde.|  
|is_select_all|**bit**|1 = Gibt an, dass das Objekt in einer SELECT *-Klausel verwendet wird (nur auf Objektebene).|  
|is_all_columns_found|**bit**|1 = Alle Spaltenabhängigkeiten für das Objekt konnten gefunden werden.<br /><br /> 0 = Spaltenabhängigkeiten für das Objekt konnten nicht gefunden werden.|
|is_insert_all|**bit**|1 = das Objekt wird in einer INSERT-Anweisung ohne Spaltenliste verwendet (nur auf Objektebene).<br /><br />Diese Spalte wurde in SQL Server 2016 hinzugefügt.|  
|is_incomplete|**bit**|1 = das Objekt oder die Spalte weist einen Bindungs Fehler auf und ist unvollständig.<br /><br />Diese Spalte wurde in SQL Server 2016 SP2 hinzugefügt.|
| &nbsp; | &nbsp; | &nbsp; |

## <a name="exceptions"></a>Ausnahmen  
 Gibt unter den folgenden Bedingungen ein leeres Resultset zurück:  
  
-   Ein Systemobjekt wird angegeben.  
  
-   Die angegebene Entität ist in der Datenbank nicht vorhanden.  
  
-   Die angegebene Entität verweist auf keine Entitäten.  
  
-   Ein ungültiger Parameter wird übergeben.  
  
 Gibt einen Fehler zurück, wenn die angegebene verweisende Entität eine nummerierte gespeicherte Prozedur ist.  
  
 Gibt Fehler 2020 zurück, wenn Spaltenabhängigkeiten nicht aufgelöst werden können. Dieser Fehler verhindert nicht, dass die Abfrage Abhängigkeiten auf Objektebene zurückgibt.  
  
## <a name="remarks"></a>Bemerkungen  
 Diese Funktion kann im Kontext jeder beliebigen Datenbank ausgeführt werden, um die Entitäten zurückzugeben, die auf einen DDL-Trigger auf Serverebene verweisen.  
  
 In der folgenden Tabelle werden die Typen von Entitäten aufgelistet, für die Abhängigkeitsinformationen erstellt und verwaltet werden. Für Regeln, Standardwerte, temporäre Tabellen, temporär gespeicherte Prozeduren oder Systemobjekte werden keine Abhängigkeitsinformationen erstellt oder verwaltet.  
  
|Entitätstyp|Verweisende Entität|Entität, auf die verwiesen wird|  
|-----------------|------------------------|-----------------------|  
|Tabelle|Ja*|Ja|  
|Sicht|Ja|Ja|  
|In [!INCLUDE[tsql](../../includes/tsql-md.md)] gespeicherte Prozedur**|Ja|Ja|  
|Gespeicherte CLR-Prozedur|Nein|Ja|  
|Benutzerdefinierte Funktion in [!INCLUDE[tsql](../../includes/tsql-md.md)]|Ja|Ja|  
|CLR-benutzerdefinierte Funktion|Nein|Ja|  
|CLR-Trigger (DML und DDL)|Nein|Nein|  
|DML-Trigger in [!INCLUDE[tsql](../../includes/tsql-md.md)]|Ja|Nein|  
|DDL-Trigger auf Datenbankebene in [!INCLUDE[tsql](../../includes/tsql-md.md)]|Ja|Nein|  
|DDL-Trigger auf Serverebene in [!INCLUDE[tsql](../../includes/tsql-md.md)]|Ja|Nein |  
|Erweiterte gespeicherte Prozeduren|Nein |Ja|  
|Warteschlange|Nein|Ja|  
|Synonym|Nein|Ja|  
|Typ (Alias und CLR-benutzerdefinierter Typ)|Nein|Ja|  
|XML-Schemaauflistung|Nein|Ja|  
|Partitionsfunktion|Nein|Ja|  
| &nbsp; | &nbsp; | &nbsp; |

 \* Eine Tabelle wird nur als verweisende Entität nachverfolgt, wenn Sie [!INCLUDE[tsql](../../includes/tsql-md.md)] auf ein Modul, einen benutzerdefinierten Typ oder eine XML-Schema Auflistung in der Definition einer berechneten Spalte, einer Check-Einschränkung oder einer DEFAULT-Einschränkung verweist.  
  
 ** Nummerierte gespeicherte Prozeduren mit einem ganzzahligen Wert größer als 1 werden weder als verweisende Entität noch als Entität, auf die verwiesen wird, aufgezeichnet.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die SELECT-Berechtigung für sys.dm_sql_referenced_entities und die VIEW DEFINITION-Berechtigung für die verweisende Entität. Standardmäßig wird die SELECT-Berechtigung der public-Rolle erteilt. Erfordert die Berechtigung VIEW DEFINITION oder ALTER DATABASE DDL TRIGGER für die Datenbank, wenn es sich bei der verweisenden Entität um einen DDL-Trigger auf Datenbankebene handelt. Erfordert die VIEW ANY DEFINITION-Berechtigung für den Server, wenn es sich bei der verweisenden Entität um einen DDL-Trigger auf Serverebene handelt.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-return-entities-that-are-referenced-by-a-database-level-ddl-trigger"></a>A. Rückgabe von Entitäten, auf die durch einen DDL-Auslösers auf Datenbankebene  
 Im folgenden Beispiel werden die Entitäten (Tabellen und Spalten) zurückgegeben, auf die vom DDL-Trigger auf Datensatzebene  `ddlDatabaseTriggerLog` verwiesen wird.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT
        referenced_schema_name,
        referenced_entity_name,
        referenced_minor_name,
        referenced_minor_id,
        referenced_class_desc
    FROM
        sys.dm_sql_referenced_entities (
            'ddlDatabaseTriggerLog',
            'DATABASE_DDL_TRIGGER')
;
GO  
```  
  
### <a name="b-return-entities-that-are-referenced-by-an-object"></a>B. Rückgabe von Entitäten, auf die von einem Objekt verwiesen wird  
 Im folgenden Beispiel werden die Entitäten zurückgegeben, auf die von der benutzerdefinierten Funktion `dbo.ufnGetContactInformation` verwiesen wird.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT
        referenced_schema_name,
        referenced_entity_name,
        referenced_minor_name,
        referenced_minor_id,
        referenced_class_desc,
        is_caller_dependent,
        is_ambiguous
    FROM
        sys.dm_sql_referenced_entities (
            'dbo.ufnGetContactInformation',
            'OBJECT')
;
GO  
```  
  
### <a name="c-return-column-dependencies"></a>C. Zurückgeben von Spalten Abhängigkeiten  
 Im folgenden Beispiel wird die `Table1`-Tabelle mit der berechneten Spalte `c`, die als Summe der Spalten `a` und `b` definiert ist, erstellt. Anschließend wird die `sys.dm_sql_referenced_entities`-Sicht aufgerufen. Die Sicht gibt zwei Zeilen zurück: eine für jede in der berechneten Spalte definierte Spalte.  
  
```sql  
CREATE TABLE dbo.Table1 (a int, b int, c AS a + b);  
GO  
SELECT
        referenced_schema_name AS schema_name,  
        referenced_entity_name AS table_name,  
        referenced_minor_name  AS referenced_column,  
        COALESCE(
            COL_NAME(OBJECT_ID(N'dbo.Table1'),
            referencing_minor_id),
            'N/A') AS referencing_column_name  
    FROM
        sys.dm_sql_referenced_entities ('dbo.Table1', 'OBJECT')
;
GO

-- Remove the table.  
DROP TABLE dbo.Table1;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```console
 schema_name table_name referenced_column referencing_column  
 ----------- ---------- ----------------- ------------------  
 dbo         Table1     a                 c  
 dbo         Table1     b                 c  
```

### <a name="d-returning-non-schema-bound-column-dependencies"></a>D: Zurückgeben von nicht schemagebundenen Spaltenabhängigkeiten  
 Im folgenden Beispiel wird die `Table1`-Tabelle gelöscht und die `Table2`-Tabelle sowie die gespeicherte Prozedur `Proc1` erstellt. Die Prozedur verweist auf die `Table2`-Tabelle und auf die nicht vorhandene `Table1`-Tabelle. Die `sys.dm_sql_referenced_entities`-Sicht wird mit der gespeicherten Prozedur ausgeführt, die als verweisende Entität angegeben ist. Das Resultset zeigt eine Zeile für `Table1` und drei Zeilen für `Table2` an. Da `Table1` nicht vorhanden ist, können die Spaltenabhängigkeiten nicht aufgelöst werden, und es wird der Fehler 2020 zurückgegeben. Die Spalte `is_all_columns_found` gibt 0 für `Table1` zurück. Damit wird angegeben, dass einige Spalten nicht ermittelt werden konnten.  
  
```sql  
DROP TABLE IF EXISTS dbo.Table1;
GO  
CREATE TABLE dbo.Table2 (c1 int, c2 int);  
GO  
CREATE PROCEDURE dbo.Proc1 AS  
    SELECT a, b, c FROM Table1;  
    SELECT c1, c2 FROM Table2;  
GO  
SELECT
        referenced_id,
        referenced_entity_name AS table_name,
        referenced_minor_name  AS referenced_column_name,
        is_all_columns_found
    FROM
        sys.dm_sql_referenced_entities ('dbo.Proc1', 'OBJECT');
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```console
 referenced_id table_name   referenced_column_name  is_all_columns_found  
 ------------- ------------ ----------------------- --------------------  
 935674381     Table2       NULL                    1  
 935674381     Table2       C1                      1  
 935674381     Table2       C2                      1  
 NULL          Table1       NULL                    0  

Msg 2020, Level 16, State 1, Line 1
The dependencies reported for entity "dbo.Proc1" might not include
  references to all columns. This is either because the entity
  references an object that does not exist or because of an error
  in one or more statements in the entity.  Before rerunning the
  query, ensure that there are no errors in the entity and that
  all objects referenced by the entity exist.
 ```
  
### <a name="e-demonstrating-dynamic-dependency-maintenance"></a>E. Veranschaulichen der dynamischen Verwaltung von Abhängigkeiten  

In diesem Beispiel wird davon ausgegangen, dass das Beispiel D ausgeführt wurde. Beispiel E zeigt, dass Abhängigkeiten dynamisch verwaltet werden. Im folgenden Beispiel werden die folgenden Schritte ausgeführt:

1. Erstellt neu `Table1` , das in Beispiel D gelöscht wurde.
2. Run `sys.dm_sql_referenced_entities` wird dann erneut ausgeführt, und die gespeicherte Prozedur wird als verweisende Entität angegeben.

Das Resultset zeigt, dass beide Tabellen und ihre entsprechenden Spalten, die in der gespeicherten Prozedur definiert sind, zurückgegeben werden. Außerdem gibt die Spalte `is_all_columns_found` für alle Objekte und Spalten 1 zurück.

```sql  
CREATE TABLE Table1 (a int, b int, c AS a + b);  
GO   
SELECT
        referenced_id,
        referenced_entity_name AS table_name,
        referenced_minor_name  AS column_name,
        is_all_columns_found
    FROM
        sys.dm_sql_referenced_entities ('dbo.Proc1', 'OBJECT');
GO  
DROP TABLE Table1, Table2;  
DROP PROC Proc1;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```console
 referenced_id table_name   referenced_column_name  is_all_columns_found  
 ------------- ------------ ----------------------- --------------------  
 935674381     Table2       NULL                    1 
 935674381     Table2       c1                      1 
 935674381     Table2       c2                      1 
 967674495     Table1       NULL                    1 
 967674495     Table1       a                       1  
 967674495     Table1       b                       1  
 967674495     Table1       c                       1  
 ```
 
### <a name="f-returning-object-or-column-usage"></a>F. Zurückgeben von Objekten und Spalten (Verwendung)  
 Im folgenden Beispiel werden die Objekte und die Spaltenabhängigkeiten der gespeicherten Prozedur `HumanResources.uspUpdateEmployeePersonalInfo` zurückgegeben. Diese Prozedur aktualisiert die Spalten `NationalIDNumber` , `BirthDate,``MaritalStatus` und `Gender` der- `Employee` Tabelle auf Grundlage eines angegebenen- `BusinessEntityID` Werts. Eine andere gespeicherte Prozedur `upsLogError` wird in einem try... Catch-Block, um Ausführungsfehler zu erfassen. Die Spalten `is_selected`, `is_updated` und `is_select_all` geben Informationen darüber zurück, wie diese Objekte und Spalten innerhalb des verweisenden Objekts verwendet werden. Die Tabelle und geänderten Spalten werden mit 1 in der Spalte "is_updated" angegeben. Die Spalte `BusinessEntityID` wird nur ausgewählt und die gespeicherte Prozedur `uspLogError` wird weder ausgewählt noch geändert.  

```sql  
USE AdventureWorks2012;
GO
SELECT
        referenced_entity_name AS table_name,
        referenced_minor_name  AS column_name,
        is_selected,  is_updated,  is_select_all
    FROM
        sys.dm_sql_referenced_entities(
            'HumanResources.uspUpdateEmployeePersonalInfo',
            'OBJECT')
;
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```console
 table_name    column_name         is_selected is_updated is_select_all  
 ------------- ------------------- ----------- ---------- -------------  
 uspLogError   NULL                0           0          0  
 Employee      NULL                0           1          0  
 Employee      BusinessEntityID    1           0          0  
 Employee      NationalIDNumber    0           1          0  
 Employee      BirthDate           0           1          0  
 Employee      MaritalStatus       0           1          0  
 Employee      Gender              0           1          0
 ```
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
  
