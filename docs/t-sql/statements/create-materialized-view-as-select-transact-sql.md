---
description: CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL)
title: CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: data-warehouse
ms.topic: reference
f1_keywords:
- CREATE VIEW
- VIEW_TSQL
- VIEW
- CREATE_VIEW_TSQL
- SCHEMABINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table creation [SQL Server], CREATE VIEW
- views [SQL Server], creating
- CREATE VIEW statement
- updatable partitioned views
- tables [SQL Server], virtual
- updatable views
- modifying partitioned views
- virtual tables [SQL Server]
- number of columns per view
- partitioned views [SQL Server], creating
- WITH ENCRYPTION clause
- WITH CHECK OPTION clause
- partitioned views [SQL Server], modifying
- partitioned views [SQL Server], replication
- distributed partitioned views [SQL Server]
- views [SQL Server], indexed views
- maximum number of columns per view
ms.assetid: aecc2f73-2ab5-4db9-b1e6-2f9e3c601fb9
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 60f460c06c89d1d9b01ed5f19f705cec745dce09
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247358"
---
# <a name="create-materialized-view-as-select-transact-sql"></a>CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL)  

[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

In diesem Artikel wird die T-SQL-Anweisung CREATE MATERIALIZED VIEW AS SELECT in [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] für Entwicklungslösungen erläutert. Der Artikel enthält auch Codebeispiele.

In einer materialisierten Sicht werden die von der Sichtdefinitionsabfrage zurückgegebenen Daten beibehalten, und die Sicht wird automatisch aktualisiert, wenn Daten in den zugrunde liegenden Tabellen geändert werden.   Sie verbessert die Leistung komplexer Abfragen (in der Regel Abfragen mit Joins und Aggregationen) und bietet einfache Wartungsvorgänge.   Dank der Funktion zum automatischen Abgleich ihres Ausführungsplans muss eine materialisierte Sicht nicht in der Abfrage referenziert werden, damit der Optimierer die Sicht für Ersetzungen berücksichtigt.  Dadurch können Dateningenieure materialisierte Sichten als Mechanismus zur Verbesserung der Beantwortungszeit für Abfragen implementieren, ohne die Abfragen ändern zu müssen.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CREATE MATERIALIZED VIEW [ schema_name. ] materialized_view_name
    WITH (  
      <distribution_option>
    )
    AS <select_statement>
[;]

<distribution_option> ::=
    {  
        DISTRIBUTION = HASH ( distribution_column_name )  
      | DISTRIBUTION = ROUND_ROBIN  
    }

<select_statement> ::=
    SELECT select_criteria

```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Argumente

*schema_name*     
 Ist der Name des Schemas, zu dem die Sicht gehört.  
  
*materialized_view_name*   
Der Name der Sicht. Sichtnamen müssen den Regeln für Bezeichner entsprechen. Das Angeben des Sichtbesitzernamens ist optional.  

*distribution_option*     
Es werden nur HASH- und ROUND_ROBIN-Distributionen unterstützt.

*select_statement*   
Die SELECT-Liste in der Definition der materialisierten Sicht muss mindestens eines von diesen zwei Kriterien erfüllen:
- Die SELECT-Liste enthält eine Aggregatfunktion.
- GROUP BY wird in der Definition der materialisierten Sicht verwendet, und alle Spalten in GROUP BY sind in der SELECT-Liste enthalten.  In der GROUP BY-Klausel können bis zu 32 Spalten verwendet werden.

Aggregatfunktionen sind in der SELECT-Liste der Definition der materialisierten Sicht erforderlich.  Zu den unterstützten Aggregationen gehören MAX, MIN, AVG, COUNT, COUNT_BIG, SUM, VAR, STDEV.

Wenn MIN/MAX-Aggregate in der SELECT-Liste der Definition der materialisierten Sicht verwendet werden, gelten die folgenden Anforderungen:
 
- FOR_APPEND ist erforderlich.  Beispiel:
  ```sql 
  CREATE MATERIALIZED VIEW mv_test2  
  WITH (distribution = hash(i_category_id), FOR_APPEND)  
  AS
  SELECT MAX(i.i_rec_start_date) as max_i_rec_start_date, MIN(i.i_rec_end_date) as min_i_rec_end_date, i.i_item_sk, i.i_item_id, i.i_category_id
  FROM syntheticworkload.item i  
  GROUP BY i.i_item_sk, i.i_item_id, i.i_category_id
  ```

- Die materialisierte Sicht wird bei einem UPDATE oder DELETE in den referenzierten Basistabellen deaktiviert.  Diese Einschränkung gilt nicht für INSERT-Anweisungen.  Führen Sie ALTER MATERIALIZED VIEW mit REBUILD aus, um die materialisierte Sicht wieder zu aktivieren.
  
## <a name="remarks"></a>Bemerkungen

Eine materialisierte Sicht in Azure Data Warehouse ähnelt einer indizierten Sicht in SQL Server.Es gelten fast die gleichen Einschränkungen wie für eine indizierte Sicht (Details siehe [Erstellen indizierter Sichten](../../relational-databases/views/create-indexed-views.md)) – abgesehen davon, dass eine materialisierte Sicht Aggregatfunktionen unterstützt.   

>[!Note]
>Obwohl CREATE MATERIALIZED VIEW die Anweisungen COUNT, DISTINCT, COUNT(DISTINCT-Ausdruck) oder COUNT_BIG (DISTINCT-Ausdruck) nicht unterstützt, können SELECT-Abfragen mit diesen Funktionen dennoch von materialisierten Sichten in Form einer schnelleren Leistung profitieren, da das Optimierungstool für Synapse SQL diese Aggregationen in der Benutzerabfrage automatisch neu schreiben kann, um für Übereinstimmung mit vorhandenen materialisierten Sichten zu sorgen.  Details finden Sie im Abschnitt mit Beispielen dieses Artikels. 

APPROX_COUNT_DISTINCT wird in CREATE MATERIALIZED VIEW AS SELECT nicht unterstützt.

Eine materialisierte Sicht unterstützt nur CLUSTERED COLUMNSTORE INDEX. 

Eine materialisierte Sicht kann nicht auf andere Sichten verweisen.  

Für Tabellen mit dynamischer Datenmaskierung (DDM) kann keine materialisierte Sicht erstellt werden, selbst wenn die DDM-Spalte nicht Teil der materialisierten Sicht ist.  Wenn eine Tabellenspalte Teil einer aktiven oder deaktivierten materialisierten Sicht ist, kann bei dieser Spalte keine DDM hinzugefügt werden.  

Für Tabellen mit aktivierter Sicherheit auf Zeilenebene kann keine materialisierte Sicht erstellt werden.

Materialisierte Sichten können für partitionierte Tabellen erstellt werden.  SPLIT/MERGE in Bezug auf Partitionen wird bei Basistabellen für materialisierte Sichten unterstützt. SWITCH in Bezug auf Partitionen wird nicht unterstützt.  
 
ALTER TABLE SWITCH wird nicht für Tabellen unterstützt, die in materialisierten Sichten referenziert werden. Deaktivieren oder löschen Sie die materialisierte Sicht, bevor Sie ALTER TABLE SWITCH verwenden. In den folgenden Szenarien erfordert das Erstellen der materialisierten Sicht das Hinzufügen neuer Spalten zur materialisierten Sicht:

|Szenario|Der materialisierten Sicht neu hinzuzufügende Spalten|Comment|  
|-----------------|---------------|-----------------|
|COUNT_BIG() fehlt in der SELECT-Liste der Definition einer materialisierten Sicht.| COUNT_BIG (*) |Wird beim Erstellen der materialisierten Sicht automatisch hinzugefügt.  Es ist keine Benutzeraktion erforderlich.|
|SUM(a) wird von Benutzern in der SELECT-Liste der Definition einer materialisierten Sicht angegeben, und „a“ ist ein Ausdruck, der Nullwerte zulässt. |COUNT_BIG (a) |Benutzer müssen der Definition der materialisierten Sicht den Ausdruck „a“ manuell hinzufügen.|
|AVG(a) wird von Benutzern in der SELECT-Liste der Definition einer materialisierten Sicht angegeben, wobei „a“ ein Ausdruck ist.|SUM(a), COUNT_BIG(a)|Wird beim Erstellen der materialisierten Sicht automatisch hinzugefügt.  Es ist keine Benutzeraktion erforderlich.|
|STDEV(a) wird von Benutzern in der SELECT-Liste der Definition einer materialisierten Sicht angegeben, wobei „a“ ein Ausdruck ist.|SUM(a), COUNT_BIG(a), SUM(square(a))|Wird beim Erstellen der materialisierten Sicht automatisch hinzugefügt.  Es ist keine Benutzeraktion erforderlich. |
| | | |

Nach ihrer Erstellung werden materialisierte Sichten in SQL Server Management Studio im Ordner „Views“ (Sichten) der [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]-Instanz angezeigt.

Benutzer können [SP_SPACEUSED](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) und [DBCC PDW_SHOWSPACEUSED](../database-console-commands/dbcc-pdw-showspaceused-transact-sql.md?view=azure-sqldw-latest&preserve-view=true) ausführen, um den von einer materialisierten Sicht verbrauchten Speicherplatz zu ermitteln.  

Eine materialisierte Sicht kann über DROP VIEW gelöscht werden.  Sie können eine materialisierte Sicht mit ALTER MATERIALIZED VIEW deaktivieren oder neu erstellen.   

Der EXPLAIN-Plan und der grafische Plan zur geschätzten Ausführung in SQL Server Management Studio können zeigen, ob eine materialisierte Sicht vom Abfrageoptimierer bei der Abfrageausführung berücksichtigt wird.

Um herauszufinden, ob eine SQL-Anweisung von einer neuen materialisierten Sicht profitieren kann, führen Sie den `EXPLAIN`-Befehl mit `WITH_RECOMMENDATIONS` aus.  Ausführliche Informationen finden Sie unter [EXPLAIN (Transact-SQL)](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true).

## <a name="permissions"></a>Berechtigungen

Erfordert entweder die Berechtigungen REFERENCES und CREATE VIEW oder die Berechtigung CONTROL in dem Schema, in dem die Sicht erstellt wird. 

## <a name="example"></a>Beispiel
A. Dieses Beispiel zeigt, wie das Optimierungstool für Synapse SQL materialisierte Sichten verwendet, um eine Abfrage mit verbesserter Leistung auszuführen, selbst wenn die Abfrage Funktionen verwendet, die in CREATE MATERIALIZED VIEW nicht unterstützt werden, z. B. COUNT(DISTINCT-Ausdruck). Abfragen, deren Durchführung bisher mehrere Sekunden dauerte, werden nun im Bruchteil einer Sekunde abgeschlossen, ohne dass Änderungen der Benutzerabfrage erfolgen.   

``` sql 

-- Create a table with ~536 million rows
create table t(a int not null, b int not null, c int not null) with (distribution=hash(a), clustered columnstore index);

insert into t values(1,1,1);

declare @p int =1;
while (@P < 30)
    begin
    insert into t select a+1,b+2,c+3 from t;  
    select @p +=1;
end

-- A SELECT query with COUNT_BIG (DISTINCT expression) took multiple seconds to complete and it reads data directly from the base table a. 
select a, count_big(distinct b) from t group by a;

-- Create two materialized views, not using COUNT_BIG(DISTINCT expression).
create materialized view V1 with(distribution=hash(a)) as select a, b from dbo.t group by a, b;

-- Clear all cache.

DBCC DROPCLEANBUFFERS;
DBCC freeproccache;

-- Check the estimated execution plan in SQL Server Management Studio.  It shows the SELECT query is first step (GET operator) is to read data from the materialized view V1, not from base table a.
select a, count_big(distinct b) from t group by a;

-- Now execute this SELECT query.  This time it took sub-second to complete because Synapse SQL engine automatically matches the query with materialized view V1 and uses it for faster query execution.  There was no change in the user query.

DECLARE @timerstart datetime2, @timerend datetime2;
SET @timerstart = sysdatetime();

select a, count_big(distinct b) from t group by a;

SET @timerend = sysdatetime()
select DATEDIFF(ms,@timerstart,@timerend);

```

B. In diesem Beispiel erstellt User_B eine materialisierte Sicht für die Tabellen T1 und T2.  Die Sicht und die beiden Tabellen sind im Besitz eines anderen Benutzers, User_A.

```sql

-- Create the users 
CREATE USER User_A WITHOUT LOGIN ;  
CREATE USER User_B WITHOUT LOGIN ;  
GO
CREATE SCHEMA User_A authorization User_A;
GO

-- User_A creates two tables

GRANT CREATE TABLE to User_A;
GO
EXECUTE AS USER = 'User_A';  
SELECT USER_NAME();  
Go
CREATE TABLE [User_A].[T1]
(
    [vendorID] [varchar](255) Not NULL,
    [totalAmount] [float] Not NULL,
    [puYear] [int] NULL
)
GO
CREATE TABLE [User_A].[T2]
(
    [vendorID] [varchar](255) Not NULL,
    [totalAmount] [float] Not NULL,
    [puYear] [int] NULL
)
GO
REVERT;

-- Grant User_B the required permissions to create a materialized view for User_A on T1 and T2 owned by User_A
GRANT CREATE VIEW to User_B;
GRANT Control ON SCHEMA::User_A to User_B;
GRANT REFERENCES ON OBJECT::User_A.T1 to User_B;
GRANT REFERENCES ON OBJECT::User_A.T2 to User_B;

-- User_B creates a materialized view.  Both the view and the base tables are owned by User_A.
EXECUTE AS USER = 'User_B';  
SELECT USER_NAME(); 
GO

CREATE materialized VIEW [User_A].MV_CreatedBy_UserB with(distribution=round_robin) 
as 
        select A.vendorID, sum(A.totalamount) as S, Count_Big(*) as T 
        from [User_A].[T1] A
        inner join [User_A].[T2] B
        on A.vendorID = B.vendorID
        group by A.vendorID ;
GO
revert;
```

## <a name="see-also"></a>Weitere Informationen

[Leistungsoptimierung durch materialisierte Sicht](/azure/sql-data-warehouse/performance-tuning-materialized-views)   
[ALTER MATERIALIZED VIEW &#40;Transact-SQL&#41;](./alter-materialized-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)      
[DROP VIEW](./drop-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)  
[EXPLAIN &#40;Transact-SQL&#41;](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-column-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-mappings-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD &#40;Transact-SQL&#41;](../database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]- und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]-Katalogsichten](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[Systemansichten werden in Azure [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] unterstützt](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[In Azure [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] unterstützte T-SQL-Anweisungen](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)
