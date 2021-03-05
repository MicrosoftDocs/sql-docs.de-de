---
title: FROM-Klausel mit JOIN, APPLY, PIVOT (T-SQL)
description: FROM-Klausel mit JOIN, APPLY, PIVOT (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- JOIN
- FROM_TSQL
- FROM
- JOIN_TSQL
- OUTER_JOIN_TSQL
- INNER_JOIN_TSQL
- CROSS_TSQL
- CROSS_APPLY_TSQL
- APPLY_TSQL
- CROSS_JOIN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- OUTER APPLY operator
- hints [SQL Server], FROM clause
- SELECT statement [SQL Server], FROM clause
- ISO syntax
- DELETE statement [SQL Server], FROM clause
- CROSS APPLY operator
- FROM clause
- APPLY operator
- joins [SQL Server], FROM clause
- UPDATE statement [SQL Server], FROM clause
- derived tables
ms.assetid: 36b19e68-94f6-4539-aeb1-79f5312e4263
author: VanMSFT
ms.author: vanto
ms.custom: ''
ms.date: 06/01/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 03bd9ad046be57d9fedea50ae310956b26a32581
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186484"
---
# <a name="from-clause-plus-join-apply-pivot-transact-sql"></a>FROM-Klausel mit JOIN, APPLY, PIVOT (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

In Transact-SQL steht die FROM-Klausel in den folgenden Anweisungen zur Verfügung:

- [DELETE](../statements/delete-transact-sql.md)
- [UPDATE](update-transact-sql.md)
- [SELECT](select-transact-sql.md)

Die FROM-Klausel ist normalerweise in der SELECT-Anweisung erforderlich. Die Ausnahme ist, wenn keine Tabellenspalten aufgeführt sind und Literale, Variablen oder arithmetische Ausdrücke die einzigen aufgeführten Elemente sind.

Dieser Artikel behandelt auch die folgenden Schlüsselwörter, die in der FROM-Klausel verwendet werden können:

- [JOIN](../../relational-databases/performance/joins.md)
- APPLY
- [PIVOT](from-using-pivot-and-unpivot.md)

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
[ FROM { <table_source> } [ ,...n ] ]   
<table_source> ::=   
{  
    table_or_view_name [ FOR SYSTEM_TIME <system_time> ] [ AS ] table_alias ]   
        [ <tablesample_clause> ]   
        [ WITH ( < table_hint > [ [ , ]...n ] ) ]   
    | rowset_function [ [ AS ] table_alias ]   
        [ ( bulk_column_alias [ ,...n ] ) ]   
    | user_defined_function [ [ AS ] table_alias ]  
    | OPENXML <openxml_clause>   
    | derived_table [ [ AS ] table_alias ] [ ( column_alias [ ,...n ] ) ]   
    | <joined_table>   
    | <pivoted_table>   
    | <unpivoted_table>  
    | @variable [ [ AS ] table_alias ]  
    | @variable.function_call ( expression [ ,...n ] )   
        [ [ AS ] table_alias ] [ (column_alias [ ,...n ] ) ]     
}  
<tablesample_clause> ::=  
    TABLESAMPLE [SYSTEM] ( sample_number [ PERCENT | ROWS ] )   
        [ REPEATABLE ( repeat_seed ) ]   
  
<joined_table> ::=   
{  
    <table_source> <join_type> <table_source> ON <search_condition>   
    | <table_source> CROSS JOIN <table_source>   
    | left_table_source { CROSS | OUTER } APPLY right_table_source   
    | [ ( ] <joined_table> [ ) ]   
}  
<join_type> ::=   
    [ { INNER | { { LEFT | RIGHT | FULL } [ OUTER ] } } [ <join_hint> ] ]  
    JOIN  
  
<pivoted_table> ::=  
    table_source PIVOT <pivot_clause> [ [ AS ] table_alias ]  
  
<pivot_clause> ::=  
        ( aggregate_function ( value_column [ [ , ]...n ])   
        FOR pivot_column   
        IN ( <column_list> )   
    )   
  
<unpivoted_table> ::=  
    table_source UNPIVOT <unpivot_clause> [ [ AS ] table_alias ]  
  
<unpivot_clause> ::=  
    ( value_column FOR pivot_column IN ( <column_list> ) )   
  
<column_list> ::=  
    column_name [ ,...n ]   
  
<system_time> ::=  
{  
       AS OF <date_time>  
    |  FROM <start_date_time> TO <end_date_time>  
    |  BETWEEN <start_date_time> AND <end_date_time>  
    |  CONTAINED IN (<start_date_time> , <end_date_time>)   
    |  ALL  
}  
  
    <date_time>::=  
        <date_time_literal> | @date_time_variable  
  
    <start_date_time>::=  
        <date_time_literal> | @date_time_variable  
  
    <end_date_time>::=  
        <date_time_literal> | @date_time_variable  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
FROM { <table_source> [ ,...n ] }  
  
<table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [<tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
    | <joined_table>  
}  
  
<tablesample_clause> ::=
    TABLESAMPLE ( sample_number [ PERCENT ] ) -- Azure Synapse Analytics Dedicated SQL pool only  
 
<joined_table> ::=   
{  
    <table_source> <join_type> <table_source> ON search_condition   
    | <table_source> CROSS JOIN <table_source> 
    | left_table_source { CROSS | OUTER } APPLY right_table_source   
    | [ ( ] <joined_table> [ ) ]   
}  
  
<join_type> ::=   
    [ INNER ] [ <join hint> ] JOIN  
    | LEFT  [ OUTER ] JOIN  
    | RIGHT [ OUTER ] JOIN  
    | FULL  [ OUTER ] JOIN  
  
<join_hint> ::=   
    REDUCE  
    | REPLICATE  
    | REDISTRIBUTE  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
\<table_source>  
 Gibt eine Tabelle, Sicht, Tabellenvariable oder abgeleitete Tabelle als Quelle mit oder ohne Alias zum Verwenden in der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung an. In einer Anweisung können bis zu 256 Tabellenquellen verwendet werden. Allerdings variiert das Limit in Abhängigkeit vom verfügbaren Arbeitsspeicher und der Komplexität anderer Ausdrücke in der Abfrage. Einzelne Abfragen unterstützen möglicherweise nicht bis zu 256 Tabellenquellen.  
  
> [!NOTE]  
>  Durch eine hohe Anzahl an Tabellen, auf die in einer Abfrage verwiesen wird, kann möglicherweise die Abfrageleistung beeinträchtigt werden. Zusätzliche Faktoren haben außerdem Auswirkungen auf die Kompilierungs- und Optimierungszeit. Dazu gehört das Vorhandensein von Indizes und indizierten Sichten für jede \<table_source>-Instanz und die Größe von \<select_list> in der SELECT-Anweisung.  
  
 Die Reihenfolge der Tabellenquellen nach dem FROM-Schlüsselwort hat keinen Einfluss auf das Resultset. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gibt Fehler zurück, wenn doppelte Namen in der FROM-Klausel angezeigt werden.  
  
 *table_or_view_name*  
 Der Name einer Tabelle oder Sicht.  
  
 Ist die Tabelle oder Sicht in einer anderen Datenbank der gleichen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vorhanden, verwenden Sie einen vollqualifizierten Namen in der Form *database*.*schema*.*object_name*.  
  
 Ist die Tabelle oder die Sicht außerhalb der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vorhanden, verwenden Sie einen vierteiligen Namen in der Form *linked_server*.*catalog*.*schema*.*object*. Weitere Informationen finden Sie unter [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) erläutert. Ein vierteiliger Name mit der [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md)-Funktion als Serverteil des Namens kann ebenfalls zum Angeben der Remotequelltabelle verwendet werden. Wenn OPENDATASOURCE angegeben wird, gelten *database_name* und *schema_name* möglicherweise nicht für alle Datenquellen und unterliegen den Funktionen des OLE DB-Anbieters, der auf das Remoteobjekt zugreift.  
  
 [AS] *table_alias*  
 Ein Alias für *table_source*, der zur Vereinfachung oder zur Unterscheidung einer Tabelle oder Sicht in einem Selbstjoin oder einer Unterabfrage verwendet werden kann. Ein Alias ist oftmals ein verkürzter Tabellenname, der verwendet wird, um in einem Join auf bestimmte Spalten der beteiligten Tabellen zu verweisen. Falls ein Spaltenname in mehr als einer Tabelle des Joins vorkommt, muss dieser Spaltenname für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] durch einen Tabellennamen, einen Sichtnamen oder einen Alias gekennzeichnet werden. Falls ein Alias definiert ist, kann der Tabellenname nicht verwendet werden.  
  
 Wenn eine abgeleitete Tabelle, Rowsetwertfunktion, Tabellenwertfunktion oder Operatorklausel (z.B. PIVOT oder UNPIVOT) verwendet wird, ist der erforderliche *table_alias*-Ausdruck am Ende der Klausel der zurückgegebene verknüpfte Tabellenname für alle Spalten, einschließlich gruppierter Spalten.  
  
 WITH (\<table_hint>)  
 Gibt an, dass der Abfrageoptimierer eine Optimierungs- oder Sperrstrategie bei dieser Tabelle und für diese Anweisung verwendet. Weitere Informationen finden Sie unter [Tabellenhinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
*rowset_function*  
**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher und [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Gibt eine der Rowsetfunktionen (z. B. OPENROWSET) an, die ein Objekt zurückgeben, das statt eines Tabellenverweises verwendet werden kann. Weitere Informationen zur Liste mit Rowsetfunktionen finden Sie unter [Rowsetfunktionen &#40;Transact-SQL&#41;](../functions/opendatasource-transact-sql.md).  
  
 Die Verwendung der OPENROWSET- und OPENQUERY-Funktionen zum Angeben eines Remoteobjekts hängt von den Fähigkeiten des OLE DB-Anbieters ab, der auf das Objekt zugreift.  
  
 *bulk_column_alias*  
**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher und [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
 Ein optionaler Alias, der einen Spaltennamen im Resultset ersetzt. Spaltenaliase sind nur in SELECT-Anweisungen zulässig, die die OPENROWSET-Funktion mit der BULK-Option verwenden. Wenn Sie *bulk_column_alias* verwenden, geben Sie einen Alias für jede Tabellenspalte in derselben Reihenfolge wie die Spalten in der Datei an.  
  
> [!NOTE]  
>  Dieser Alias überschreibt das NAME-Attribut in den COLUMN-Elementen einer XML-Formatdatei, sofern vorhanden.  
  
 *user_defined_function*  
 Gibt eine Tabellenwertfunktion an.  
  
OPENXML \<openxml_clause>  
**Gilt für:** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher und [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Stellt eine Rowsetsicht eines XML-Dokuments bereit. Weitere Informationen finden Sie unter [OPENXML &#40;Transact-SQL&#41;](../../t-sql/functions/openxml-transact-sql.md).  
  
 *derived_table*  
 Entspricht einer Unterabfrage, die Zeilen von der Datenbank abruft. *derived_table* wird für die äußere Abfrage als Eingabe verwendet.  
  
 *derived_table* kann mithilfe des Tabellenwertkonstruktor-Features von [!INCLUDE[tsql](../../includes/tsql-md.md)] mehrere Zeilen angeben. Beispiel: `SELECT * FROM (VALUES (1, 2), (3, 4), (5, 6), (7, 8), (9, 10) ) AS MyTable(a, b);`. Weitere Informationen finden Sie unter [Tabellenwertkonstruktor &#40;Transact-SQL&#41;](../../t-sql/queries/table-value-constructor-transact-sql.md).  
  
 *column_alias*  
 Ein optionaler Alias, der einen Spaltennamen im Resultset der abgeleiteten Tabelle ersetzen soll. Geben Sie für jede Spalte in der Auswahlliste einen Spaltenalias an, und schließen Sie die gesamte Liste der Spaltenaliasnamen in Klammern ein.  
  
 *table_or_view_name* FOR SYSTEM_TIME \<system_time> Gilt für**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und später sowie [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Gibt an, dass eine bestimmte Version von Daten aus der angegebenen temporalen Tabelle und die verknüpfte Verlaufstabelle mit Systemversionsverwaltung zurückgegeben werden.  
  
### <a name="tablesample-clause"></a>TABLESAMPLE-Klausel
**Anwendungsbereich:** SQL Server und SQL-Datenbank; Gibt an, dass Beispieldaten aus der Tabelle zurückgegeben werden. Die Beispieldaten können ungefähr sein. Diese Klausel kann für eine primäre oder verknüpfte Tabelle in einer SELECT- oder UPDATE-Anweisung verwendet werden. TABLESAMPLE kann nicht für Sichten angegeben werden.  
  
> [!NOTE]  
>  Wenn Sie TABLESAMPLE für Datenbanken verwenden, die auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aktualisiert wurden, wird der Kompatibilitätsgrad der Datenbank auf mindestens 110 festgelegt, und PIVOT ist in einer rekursiven allgemeinen Tabellenausdrucksabfrage (CTE, Common Table Expression) nicht zugelassen. Weitere Informationen finden Sie unter [ALTER DATABASE-Kompatibilitätsgrad &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
 SYSTEM  
 Eine von der Implementierung abhängige Stichprobenmethode, die durch ISO-Standards angegeben ist. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist dies die einzige verfügbare Stichprobenmethode. Sie wird standardmäßig angewendet. SYSTEM wendet eine seitenbasierte Stichprobenmethode an, in der eine zufällige Gruppe von Seiten aus der Tabelle für die Stichprobe ausgewählt wird. Alle Zeilen auf diesen Seiten werden als Stichprobenteilmenge zurückgegeben.  
  
 *sample_number*  
 Ein genauer oder ungefährer konstanter numerischer Ausdruck, der den Prozentanteil oder die Anzahl der Zeilen angibt. Bei der Angabe für PERCENT wird *sample_number* implizit in einen **float**-Wert konvertiert. Anderenfalls wird das Argument in **bigint** konvertiert. PERCENT ist die Standardeinstellung.  
  
 PERCENT  
 Gibt an, dass ein *sample_number*-Prozentanteil der Zeilen einer Tabelle aus der Tabelle abgerufen werden soll. Wenn PERCENT angegeben wird, gibt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen ungefähren Wert des angegebenen Prozentanteils zurück. Wenn PERCENT angegeben wird, muss der *sample_number*-Ausdruck zu einem Wert von 0 bis 100 ausgewertet werden.  
  
 ROWS  
 Gibt an, dass eine ungefähre *sample_number*-Zeilenanzahl abgerufen wird. Wenn ROWS angegeben ist, gibt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ungefähr die angegebene Zeilenanzahl zurück. Wenn ROWS angegeben ist, muss der *sample_number*-Ausdruck zu einem ganzzahligen Wert größer als null ausgewertet werden.  
  
 REPEATABLE  
 Gibt an, dass die ausgewählten Stichprobendaten erneut zurückgegeben werden können. Bei Angabe mit demselben *repeat_seed*-Wert gibt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dieselbe Teilmenge von Zeilen zurück, wenn noch keine Änderungen an den Zeilen in der Tabelle vorgenommen wurden. Bei Angabe mit einem anderen *repeat_seed*-Wert gibt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] möglicherweise einige andere Stichprobenzeilen in der Tabelle zurück. Die folgenden an der Tabelle vorgenommenen Aktionen gelten als Änderungen: Einfügen, Aktualisieren, Löschen, neues Erstellen oder Defragmentieren von Indizes sowie Wiederherstellen oder Anfügen von Datenbanken.  
  
 *repeat_seed*  
 Ist ein konstanter ganzzahliger Ausdruck, der von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zum Generieren einer Zufallszahl verwendet wird. *repeat_seed* ist vom Datentyp **bigint**. Wenn *repeat_seed* nicht angegeben ist, weist [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen zufälligen Wert zu. Für einen bestimmten *repeat_seed*-Wert ist das Stichprobenergebnis immer gleich, wenn keine Änderungen auf die Tabelle angewendet wurden. Der *repeat_seed*-Ausdruck muss zu einem ganzzahligen Wert größer null ausgewertet werden.  
  
### <a name="tablesample-clause"></a>TABLESAMPLE-Klausel
**Gilt für:** [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]

 Gibt an, dass Beispieldaten aus der Tabelle zurückgegeben werden. Die Beispieldaten können ungefähr sein. Diese Klausel kann für eine primäre oder verknüpfte Tabelle in einer SELECT- oder UPDATE-Anweisung verwendet werden. TABLESAMPLE kann nicht für Sichten angegeben werden. 

 PERCENT  
 Gibt an, dass ein *sample_number*-Prozentanteil der Zeilen einer Tabelle aus der Tabelle abgerufen werden soll. Wenn PERCENT angegeben wird, gibt [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] einen ungefähren Wert des angegebenen Prozentanteils zurück. Wenn PERCENT angegeben wird, muss die Auswertung des *sample_number*-Ausdrucks einen Wert von 0 bis 100 ergeben.  


### <a name="joined-table"></a>Verknüpfte Tabelle 
Eine verknüpfte Tabelle ist ein Resultset, das das Produkt von zwei oder mehr Tabellen darstellt. Verwenden Sie für mehrere Joins Klammern, um die natürliche Joinreihenfolge zu ändern.  
  
### <a name="join-type"></a>Join-Typ
Gibt den Typ der Joinoperation an.  
  
 INNER  
 Gibt an, dass alle übereinstimmenden Paare von Zeilen zurückgegeben werden. Zeilen, die in den beiden Tabellen nicht übereinstimmen, werden verworfen. Wenn kein Jointyp angegeben wird, ist dies die Standardeinstellung.  
  
 FULL [OUTER]  
 Gibt an, dass eine Zeile aus der linken oder der rechten Tabelle im Resultset aufgeführt werden soll, die die Joinbedingung nicht erfüllt. Die Ausgabespalten der anderen Tabelle werden in diesem Fall auf NULL festgelegt. Dies erfolgt zusätzlich zu allen Zeilen, die von INNER JOIN zurückgegeben werden.  
  
 LEFT [OUTER]  
 Gibt an, dass alle Zeilen der linken Tabelle, die die angegebene Joinbedingung nicht erfüllen, im Resultset enthalten sind. Die Ausgabespalten der anderen Tabelle werden auf NULL gesetzt. Dies erfolgt zusätzlich zu allen Zeilen, die von INNER JOIN zurückgegeben werden.  
  
 RIGHT [OUTER]  
 Gibt an, dass alle Zeilen der rechten Tabelle, die die angegebene Joinbedingung nicht erfüllen, im Resultset enthalten sind. Die Ausgabespalten der anderen Tabelle werden auf NULL gesetzt. Dies erfolgt zusätzlich zu allen Zeilen, die von INNER JOIN zurückgegeben werden.  
  
### <a name="join-hint"></a>Jointipp  
Gibt für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDS](../../includes/sssds-md.md)] an, dass der Abfrageoptimierer von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pro Join genau einen in der FROM-Klausel angegebenen Joinhinweis oder Ausführungsalgorithmus verwendet. Weitere Informationen finden Sie unter [Joinhinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-join.md).  
  
 Bei [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] gelten diese Joinhinweise für INNER Joins in zwei verteilungsinkompatiblen Spalten. Sie können die Abfrageleistung verbessern, indem Sie die Anzahl der Datenverschiebungen bei der Verarbeitung von Abfragen einschränken. Die zulässigen Joinhinweise für [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] lauten wie folgt:  
  
 REDUCE  
 Reduziert die Anzahl der Zeilen, die für die Tabelle auf der rechten Seite des Joins verschoben werden, um zwei verteilungsinkompatible Tabellen kompatibel zu machen. Der REDUCE-Hinweis wird auch als Semijoinhinweis bezeichnet.  
  
 REPLICATE  
 Bewirkt, dass die Werte in der Verknüpfungsspalte aus der Tabelle auf der linken Seite des Joins auf allen Knoten repliziert werden. Die Tabelle auf der rechten Seite wird mit der replizierten Version dieser Spalten verknüpft.  
  
 REDISTRIBUTE  
 Erzwingt, dass zwei Datenquellen in Spalten verteilt werden, die in der JOIN-Klausel angegeben sind. Bei einer verteilten Tabelle führt [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] eine Verschiebung mit Vermischung durch. Bei einer replizierten Tabelle führt [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] eine Zuschneide-Verschiebung durch. Informationen zu diesen Verschiebungsarten finden Sie im Abschnitt "DMS Query Plan Operations" (DMS-Abfrageplanvorgänge) im Thema "Understanding Query Plans" (Informationen zu Abfrageplänen) in der [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)]. Mit diesem Hinweis kann die Leistung verbessert werden, wenn beim Abfrageplan eine Übertragungs-Verschiebung verwendet wird, um einen verteilungsinkompatiblen Join zu beseitigen.  
  
 JOIN  
 Legt fest, dass die angegebene Joinoperation mit den angegebenen Tabellenquellen oder Sichten durchgeführt werden soll.  
  
 ON \<search_condition>  
 Gibt die Bedingung an, auf der der Join basiert. Als Bedingung können beliebige Prädikate angegeben werden, obwohl häufig Spalten- und Vergleichsoperatoren verwendet werden, z. B.:  
  
```sql
SELECT p.ProductID, v.BusinessEntityID  
FROM Production.Product AS p   
JOIN Purchasing.ProductVendor AS v  
ON (p.ProductID = v.ProductID);  
  
```  
  
 Wenn in der Bedingung Spalten angegeben werden, müssen die Spalten nicht denselben Namen oder Datentyp aufweisen. Die Datentypen müssen jedoch, wenn sie nicht identisch sind, kompatibel sein oder von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implizit konvertiert werden können. Wenn die Datentypen nicht implizit konvertiert werden können, muss die Bedingung den Datentyp mithilfe der CONVERT-Funktion explizit konvertieren.  
  
 Es kann Prädikate geben, die sich auf nur eine der verknüpften Tabellen beziehen, die in der ON-Klausel angegeben sind. Derartige Prädikate können auch in der WHERE-Klausel der Abfrage stehen. Bei einem inneren Join (INNER JOIN) spielt es keine Rolle, wo diese Prädikate stehen. Bei einem äußeren Join (OUTER JOIN) können derartige Prädikate dagegen zu unterschiedlichen Ergebnissen führen. Dies liegt daran, dass die in der ON-Klausel angegebenen Prädikate vor dem Join auf die Tabelle angewendet werden, während die WHERE-Klausel semantisch auf das Ergebnis des Joins angewendet wird.  
  
 Weitere Informationen zu Suchbedingungen und Prädikaten finden Sie unter [Suchbedingung &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
 CROSS JOIN  
 Gibt das Kreuzprodukt zweier Tabellen an. Gibt dieselben Zeilen wie die frühere Joinanweisung (die nicht SQL-92-gemäß ist) ohne WHERE-Klausel zurück.  
  
 *left_table_source* { CROSS | OUTER } APPLY *right_table_source*  
 Gibt an, dass der *right_table_source*-Ausdruck des APPLY-Operators für jede Zeile des *left_table_source*-Ausdrucks ausgewertet wird. Diese Funktionalität ist hilfreich, wenn *right_table_source* eine Tabellenwertfunktion enthält, die Spaltenwerte aus *left_table_source* als eines ihrer Argumente verwendet.  
  
 Mit APPLY muss CROSS oder OUTER angegeben werden. Wenn CROSS angegeben wird, werden keine Zeilen erstellt, wenn *right_table_source* für eine bestimmte Zeile von *left_table_source* ausgewertet wird. Das zurückgegebene Resultset ist leer.  
  
 Wenn OUTER angegeben wird, wird für jede Zeile von *left_table_source* eine Zeile erstellt, selbst dann wenn *right_table_source* für diese Zeile ausgewertet wird. Das zurückgegebene Resultset ist leer.  
  
 Weitere Informationen finden Sie im Abschnitt mit Hinweisen.  
  
 *left_table_source*  
 Eine Tabellenquelle gemäß Definition im vorherigen Argument. Weitere Informationen finden Sie im Abschnitt mit Hinweisen.  
  
 *right_table_source*  
 Eine Tabellenquelle gemäß Definition im vorherigen Argument. Weitere Informationen finden Sie im Abschnitt mit Hinweisen.  
  
### <a name="pivot-clause"></a>PIVOT-Klausel

 *table_source* PIVOT \<pivot_clause>  
 Gibt an, dass *table_source* basierend auf *pivot_column* pivotiert wird. *table_source* ist eine Tabelle oder ein Tabellenausdruck. Die Ausgabe ist eine Tabelle, die alle Spalten von *table_source* enthält mit Ausnahme von *pivot_column* und *value_column*. Die Spalten von *table_source* mit Ausnahme von *pivot_column* und *value_column* werden als Gruppierungsspalten des PIVOT-Operators bezeichnet. Weitere Informationen zu PIVOT und UNPIVOT finden Sie unter [Verwenden von PIVOT und UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
 PIVOT führt einen Gruppierungsvorgang für die Eingabetabelle in Bezug auf die Gruppierungsspalten aus und gibt für jede Gruppe eine Zeile zurück. Zusätzlich enthält die Ausgabe eine Spalte für jeden Wert, der in *column_list* angegeben ist. Diese Liste wird in der *pivot_column*-Spalte der *input_table*-Tabelle angezeigt.  
  
 Weitere Informationen finden Sie im nachfolgenden Abschnitt mit Hinweisen.  
  
 *aggregate_function*  
 Eine system- oder benutzerdefinierte Aggregatfunktion, die mindestens eine Eingabe akzeptiert. Die Aggregatfunktion muss bezüglich NULL-Werten invariant sein. Eine bezüglich NULL-Werten invariante Aggregatfunktion berücksichtigt beim Auswerten des Aggregatwerts keine NULL-Werte in der Gruppe.  
  
 Die systembasierte Aggregatfunktion COUNT(*) ist nicht zulässig.  
  
 *value_column*  
 Die Wertspalte des PIVOT-Operators. Bei Verwendung mit UNPIVOT kann *value_column* nicht der Name einer vorhandenen Spalte in der *table_source*-Eingabe sein.  
  
 FOR *pivot_column*  
 Entspricht der Pivotspalte des PIVOT-Operators. *pivot_column* muss einen Typ aufweisen, der implizit oder explizit in **nvarchar()** konvertiert werden kann. Diese Spalte darf nicht vom Typ **image** oder **rowversion** sein.  
  
 Wenn UNPIVOT verwendet wird, ist *pivot_column* der Name der Ausgabespalte, zu der *table_source* eingeschränkt wird. In *table_source* darf es keine vorhandene Spalte mit diesem Namen geben.  
  
 IN (*column_list* )  
 Führt in der PIVOT-Klausel die Werte in der *pivot_column*-Spalte auf, die zu den Spaltennamen der Ausgabetabelle werden. In der Liste können keine Spaltennamen angegeben werden, die bereits in der zu pivotierenden *table_source*-Eingabe vorhanden sind.  
  
 Führt in der UNPIVOT-Klausel die Spalten in *table_source* auf, die zu einer einzelnen *pivot_column*-Spalte eingeschränkt werden.  
  
 *table_alias*  
 Entspricht dem Aliasnamen der Ausgabetabelle. *pivot_table_alias* muss angegeben werden.  
  
 UNPIVOT \<unpivot_clause>  
 Gibt an, dass die Eingabetabelle aus mehreren Spalten in *column_list* zu einer einzelnen Spalte namens *pivot_column* eingeschränkt wird. Weitere Informationen zu PIVOT und UNPIVOT finden Sie unter [Verwenden von PIVOT und UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
AS OF \<date_time>  
**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Gibt eine Tabelle mit einem einzelnen Datensatz für jede Zeile zurück, die die Werte enthält, die zum angegebenen Zeitpunkt in der Vergangenheit real (aktuell) waren. Intern wird eine Union zwischen der temporalen Tabelle und ihrer Verlaufstabelle ausgeführt, und die Ergebnisse werden so gefiltert, dass die Werte in der Zeile zurückgegeben werden, die zu dem durch den Parameter *\<date_time>* angegebenen Zeitpunkt gültig waren. Der Wert für eine Zeile ist gültig, wenn der Wert *system_start_time_column_name* kleiner als oder gleich dem Parameterwert *\<date_time>* und der Wert *system_end_time_column_name* größer als der Parameterwert *\<date_time>* ist.   
  
FROM \<start_date_time> TO \<end_date_time>
**Gilt für:** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher sowie [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]
Dieses Argument gibt eine Tabelle mit den Werten für alle Datensatzversionen zurück, die innerhalb des angegebenen Zeitbereichs aktiv waren, unabhängig davon, ob ihre Aktivität vor dem Parameterwert *\<start_date_time>* für das FROM-Argument begonnen hat oder ihre Aktivität nach dem Parameterwert *\<end_date_time>* für das TO-Argument geendet hat. Intern wird eine Union zwischen der temporalen Tabelle und ihrer Verlaufstabelle ausgeführt, und die Ergebnisse werden so gefiltert, dass die Werte für alle Zeilenversionen zurückgegeben werden, die zu irgendeinem Zeitpunkt innerhalb des angegebenen Zeitbereichs aktiv waren. Zeilen, die genau an dem durch den FROM-Endpunkt definierten unteren Grenzwert aktiv wurden, sind enthalten, und Datensätze, die genau an dem durch den TO-Endpunkt definierten oberen Grenzwert aktiv wurden, sind nicht enthalten.  
  
BETWEEN \<start_date_time> AND \<end_date_time>  
**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Dieses Argument ist identisch mit dem oben beschriebenen Argument **FROM \<start_date_time> TO \<end_date_time>** , mit dem Unterschied, dass es Zeilen enthält, die an dem durch den \<end_date_time>-Endpunkt definierten oberen Grenzwert aktiv wurden.  
  
CONTAINED IN (\<start_date_time>, \<end_date_time>)  
**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Gibt eine Tabelle mit den Werten für alle Datensatzversionen zurück, die innerhalb des von den zwei Datums-/Uhrzeitwerten für das Argument CONTAINED IN definierten Zeitbereichs geöffnet und geschlossen wurden. Zeilen, die genau beim unteren Grenzwert aktiv wurden, oder deren Aktivität genau beim oberen Grenzwert endete, sind enthalten.  
  
ALL  
Gibt eine Tabelle mit den Werten aus allen Zeilen aus der aktuellen Tabelle und aus der Verlaufstabelle zurück.  
  
## <a name="remarks"></a>Bemerkungen  
 Die FROM-Klausel unterstützt die SQL-Syntax von SQL-92 für verknüpfte und abgeleitete Tabellen. Die SQL-92-Syntax stellt die Joinoperatoren INNER, LEFT OUTER, RIGHT OUTER, FULL OUTER und CROSS zur Verfügung.  
  
 UNION und JOIN in einer FROM-Klausel werden in Sichten, abgeleiteten Tabellen und Unterabfragen unterstützt.  
  
 Ein Selbstjoin ist eine Tabelle, die mit sich selbst verknüpft ist. Vorgänge zum Einfügen und Aktualisieren, die auf einem Selbstjoin basieren, werden gemäß Reihenfolge der FROM-Klausel ausgeführt.  
  
 Da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verteilungs- und Kardinalitätsstatistiken von Verbindungsservern berücksichtigt, die Spaltenverteilungsstatistiken bereitstellen, ist der REMOTE-Joinhinweis nicht erforderlich, um eine Remotebewertung eines Joins zu erzwingen. Der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abfrageprozessor berücksichtigt Remotestatistiken und bestimmt, ob eine Remotejoinstrategie geeignet ist. Der REMOTE-Joinhinweis ist für Anbieter nützlich, die keine Spaltenverteilungsstatistiken bereitstellen.  
  
## <a name="using-apply"></a>Verwenden von APPLY  
 Der linke und der rechte Operand des APPLY-Operators sind Tabellenausdrücke. Der Hauptunterschied zwischen diesen Operanden besteht darin, dass *right_table_source* eine Tabellenwertfunktion verwenden kann, die eine Spalte aus *left_table_source* als eines der Argumente der Funktion verwendet. *left_table_source* kann Tabellenwertfunktionen einschließen. Dieser Operand kann allerdings keine Argumente enthalten, die Spalten aus *right_table_source* sind.  
  
Der APPLY-Operator funktioniert folgendermaßen, um die Tabellenquelle für die FROM-Klausel zu produzieren:  
  
1.  Wertet *right_table_source* für alle Zeilen von *left_table_source* aus, um Rowsets zu erstellen.  
  
    Die Werte in *right_table_source* hängen von *left_table_source* ab. *right_table_source* lässt sich etwa folgendermaßen darstellen: `TVF(left_table_source.row)`, wobei `TVF` eine Tabellenwertfunktion ist.  
  
2.  Kombiniert die Resultsets, die für die einzelnen Zeilen in der Auswertung von *right_table_source* mit *left_table_source* unter Ausführung eines UNION ALL-Vorgangs erstellt werden.  
  
    Die Liste der Spalten, die durch das Ergebnis des APPLY-Operators erstellt wurde, ist die Gruppe der Spalten aus *left_table_source*, die mit der Liste der Spalten aus *right_table_source* kombiniert wird.  
  
## <a name="using-pivot-and-unpivot"></a>Verwenden von PIVOT und UNPIVOT  
 Bei *pivot_column* und *value_column* handelt es sich um Gruppierungsspalten, die vom PIVOT-Operator verwendet werden. PIVOT führt dabei die folgenden Schritte aus, um das Ausgaberesultset zu erhalten:  
  
1.  Führt einen GROUP BY-Vorgang für *input_table* für die Gruppierungsspalten aus und produziert eine Ausgabespalte für jede Gruppe.  
  
     Die Gruppierungsspalten in der Ausgabespalte erhalten für diese Gruppe in *input_table* die entsprechenden Spaltenwerte.  
  
2.  Generiert folgendermaßen Werte für die Spalten in der Spaltenliste für jede Ausgabezeile:  
  
    1.  Zusätzliches Gruppieren der Zeilen für *pivot_column*, die im vorherigen Schritt in GROUP BY generiert wurden.  
  
         Auswählen einer Untergruppe für jede Ausgabespalte in *column_list*. Die Untergruppe erfüllt folgende Bedingung:  
  
         `pivot_column = CONVERT(<data type of pivot_column>, 'output_column')`  
  
    2.  *aggregate_function* wird für *value_column* bei dieser Untergruppe ausgewertet. Das Ergebnis wird als der Wert der entsprechenden *output_column*-Spalte ausgegeben. Wenn die Untergruppe leer ist, generiert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen NULL-Wert für diese *output_column*-Spalte. Wenn die Aggregatfunktion COUNT ist und die Untergruppe leer ist, wird null (0) zurückgegeben.  

> [!NOTE]
> Die Spaltenbezeichner in der `UNPIVOT`-Klausel folgen der Katalogsortierung. Bei [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] wird immer die Sortierung `SQL_Latin1_General_CP1_CI_AS` verwendet. Bei teilweise eigenständigen [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]-Datenbanken wird immer die Sortierung `Latin1_General_100_CI_AS_KS_WS_SC` verwendet. Wenn die Spalte mit anderen Spalten kombiniert wird, ist eine COLLATE-Klausel (`COLLATE DATABASE_DEFAULT`) erforderlich, um Konflikte zu vermeiden.   
  
 Weitere Informationen zu PIVOT und UNPIVOT sowie entsprechende Beispiele finden Sie unter [Verwenden von PIVOT und UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die Berechtigungen für die DELETE-, SELECT- oder UPDATE-Anweisung.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-a-simple-from-clause"></a>A. Verwenden einer einfachen FROM-Klausel  
 Im folgenden Beispiel werden die `TerritoryID`- und `Name`-Spalten aus der `SalesTerritory`-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Beispieldatenbank abgerufen.  
  
```sql    
SELECT TerritoryID, Name  
FROM Sales.SalesTerritory  
ORDER BY TerritoryID ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
TerritoryID Name                            
----------- ------------------------------  
1           Northwest                       
2           Northeast                       
3           Central                         
4           Southwest                       
5           Southeast                       
6           Canada                          
7           France                          
8           Germany                         
9           Australia                       
10          United Kingdom                  
(10 row(s) affected)  
```  
  
### <a name="b-using-the-tablock-and-holdlock-optimizer-hints"></a>B. Verwenden der TABLOCK- und HOLDLOCK-Optimierungshinweise  
 Die folgende Teiltransaktion zeigt, wie eine explizite freigegebene Tabellensperre auf die `Employee`-Tabelle angewendet und der Index gelesen wird. Die Sperre bleibt während der gesamten Transaktion bestehen.  
  
```sql    
BEGIN TRAN  
SELECT COUNT(*)   
FROM HumanResources.Employee WITH (TABLOCK, HOLDLOCK) ;  
```  
  
### <a name="c-using-the-sql-92-cross-join-syntax"></a>C. Verwenden der SQL-92-CROSS JOIN-Syntax  
 Im folgenden Beispiel wird das Kreuzprodukt der beiden Tabellen `Employee` und `Department` in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank zurückgegeben. Eine Liste aller möglichen Kombinationen der `BusinessEntityID`-Zeilen und aller `Department`-Namenszeilen wird zurückgegeben.  
  
```sql    
SELECT e.BusinessEntityID, d.Name AS Department  
FROM HumanResources.Employee AS e  
CROSS JOIN HumanResources.Department AS d  
ORDER BY e.BusinessEntityID, d.Name ;  
```  
  
### <a name="d-using-the-sql-92-full-outer-join-syntax"></a>D: Verwenden der SQL-92-FULL OUTER JOIN-Syntax  
 Im folgenden Beispiel werden der Produktname und alle zugehörigen Kaufaufträge in der `SalesOrderDetail`-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank zurückgegeben. Außerdem werden Kaufaufträge zurückgegeben, für die kein Produkt in der `Product`-Tabelle aufgeführt ist. Darüber hinaus werden Produkte mit einem anderen Kaufauftrag als dem in der `Product`-Tabelle aufgeführten Kaufauftrag zurückgegeben.  
  
```sql  
-- The OUTER keyword following the FULL keyword is optional.  
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
FULL OUTER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="e-using-the-sql-92-left-outer-join-syntax"></a>E. Verwenden der SQL-92 LEFT OUTER JOIN-Syntax  
 Im folgenden Beispiel werden zwei Tabellen über `ProductID` verknüpft und die nicht entsprechenden Zeilen aus der linken Tabelle aufbewahrt. Die `Product`-Tabelle wird mit der `SalesOrderDetail`-Tabelle über die `ProductID`-Spalten in den beiden Tabellen verglichen. Im Resultset werden alle bestellten und nicht bestellten Produkte angezeigt.  
  
```sql    
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
LEFT OUTER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="f-using-the-sql-92-inner-join-syntax"></a>F. Verwenden der SQL-92-INNER JOIN-Syntax  
 Im folgenden Beispiel werden alle Produktnamen und Verkaufsauftragsnummern zurückgegeben.  
  
```sql    
-- By default, SQL Server performs an INNER JOIN if only the JOIN   
-- keyword is specified.  
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
INNER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="g-using-the-sql-92-right-outer-join-syntax"></a>G. Verwenden der SQL-92-RIGHT OUTER JOIN-Syntax  
 Im folgenden Beispiel werden zwei Tabellen über `TerritoryID` verknüpft und die nicht entsprechenden Zeilen aus der rechten Tabelle aufbewahrt. Die `SalesTerritory`-Tabelle wird mit der `SalesPerson`-Tabelle über die `TerritoryID`-Spalte in den beiden Tabellen verglichen. Im Resultset werden alle Vertriebsmitarbeiter unabhängig davon aufgeführt, ob ihnen ein Gebiet zugewiesen ist.  
  
```sql    
SELECT st.Name AS Territory, sp.BusinessEntityID  
FROM Sales.SalesTerritory AS st   
RIGHT OUTER JOIN Sales.SalesPerson AS sp  
ON st.TerritoryID = sp.TerritoryID ;  
```  
  
### <a name="h-using-hash-and-merge-join-hints"></a>H. Verwenden von HASH- und MERGE-Joinhinweisen  
 Im folgenden Beispiel werden die drei Tabellen `Product`, `ProductVendor` und `Vendor` verknüpft, um eine Liste der Produkte und deren Anbieter zu produzieren. Der Abfrageoptimierer verknüpft `Product` und `ProductVendor` (`p` und `pv`) mit einem MERGE-Join. Das Ergebnis des MERGE-Joins von `Product` und `ProductVendor` (`p` und `pv`) wird mit der `Vendor`-Tabelle verknüpft und ergibt (`p` und `pv`) und `v`. Dabei wird ein HASH-Join angewendet.  
  
> [!IMPORTANT]  
>  Nach Angabe eines Joinhinweises muss das INNER-Schlüsselwort für jeder auszuführende innere Join explizit angegeben werden.  
  
```sql    
SELECT p.Name AS ProductName, v.Name AS VendorName  
FROM Production.Product AS p   
INNER MERGE JOIN Purchasing.ProductVendor AS pv   
ON p.ProductID = pv.ProductID  
INNER HASH JOIN Purchasing.Vendor AS v  
ON pv.BusinessEntityID = v.BusinessEntityID  
ORDER BY p.Name, v.Name ;  
```  
  
### <a name="i-using-a-derived-table"></a>I. Verwenden einer abgeleiteten Tabelle  
 Im folgenden Beispiel wird eine abgeleitete Tabelle verwendet. Mit einer `SELECT`-Anweisung nach der `FROM`-Klausel werden die Vor- und Nachnamen aller Mitarbeiter und ihre Wohnorte zurückgegeben.  
  
```sql    
SELECT RTRIM(p.FirstName) + ' ' + LTRIM(p.LastName) AS Name, d.City  
FROM Person.Person AS p  
INNER JOIN HumanResources.Employee e ON p.BusinessEntityID = e.BusinessEntityID   
INNER JOIN  
   (SELECT bea.BusinessEntityID, a.City   
    FROM Person.Address AS a  
    INNER JOIN Person.BusinessEntityAddress AS bea  
    ON a.AddressID = bea.AddressID) AS d  
ON p.BusinessEntityID = d.BusinessEntityID  
ORDER BY p.LastName, p.FirstName;  
```  
  
### <a name="j-using-tablesample-to-read-data-from-a-sample-of-rows-in-a-table"></a>J. Verwenden von TABLESAMPLE, um Daten von einem Beispiel für Zeilen in einer Tabelle zu lesen  
 Im folgenden Beispiel wird `TABLESAMPLE` in der `FROM`-Klausel verwendet, um ungefähr `10`-Prozent aller Zeilen in der `Customer`-Tabelle zurückzugeben.  
  
```sql    
SELECT *  
FROM Sales.Customer TABLESAMPLE SYSTEM (10 PERCENT) ;  
```  
  
### <a name="k-using-apply"></a>K. Verwenden von APPLY  
Im folgenden Beispiel wird vorausgesetzt, dass die folgenden Tabellen und Tabellenwertfunktionen in der Datenbank vorhanden sind:  

|Objektname|Spaltennamen|      
|---|---|   
|Departments|DeptID, DivisionID, DeptName, DeptMgrID|      
|EmpMgr|MgrID, EmpID|     
|Employees|EmpID, EmpLastName, EmpFirstName, EmpSalary|  
|GetReports(MgrID)|EmpID, EmpLastName, EmpSalary|     
  
Die Tabellenwertfunktion `GetReports` gibt eine Liste aller dem angegebenen `MgrID` direkt oder indirekt unterstellten Mitarbeiter zurück.  
  
Im Beispiel werden mit `APPLY` alle Abteilungen und alle Mitarbeiter in dieser Abteilung zurückgegeben. Wenn eine bestimmte Abteilung keine Mitarbeiter hat, werden für diese Abteilung keine Zeilen zurückgegeben.  
  
```sql
SELECT DeptID, DeptName, DeptMgrID, EmpID, EmpLastName, EmpSalary  
FROM Departments d    
CROSS APPLY dbo.GetReports(d.DeptMgrID) ;  
```  
  
Wenn die Abfrage Zeilen für diese Abteilungen ohne Mitarbeiter produzieren soll, die NULL-Werte für die Spalten `EmpID`, `EmpLastName` und `EmpSalary` produziert, verwenden Sie stattdessen `OUTER APPLY`.  
  
```sql
SELECT DeptID, DeptName, DeptMgrID, EmpID, EmpLastName, EmpSalary  
FROM Departments d   
OUTER APPLY dbo.GetReports(d.DeptMgrID) ;  
```  
  
### <a name="l-using-cross-apply"></a>L. Verwenden von CROSS APPLY  
Im folgenden Beispiel wird eine Momentaufnahme aller im Plancache gespeicherten Abfragen abgerufen, indem die dynamische Verwaltungssicht `sys.dm_exec_cached_plans` abgefragt wird, um die Planhandles aller Abfragepläne im Cache abzurufen. Dann wird der `CROSS APPLY`-Operator angegeben, um die Planhandles an `sys.dm_exec_query_plan` zu übergeben. Die XML-Showplanausgabe für jeden aktuell im Plancache gespeicherten Plan wird in der `query_plan` -Spalte der zurückgegebenen Tabelle angezeigt.  
  
```sql
USE master;  
GO  
SELECT dbid, object_id, query_plan   
FROM sys.dm_exec_cached_plans AS cp   
CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);   
GO  
```  
  
### <a name="m-using-for-system_time"></a>M. Verwenden von FOR SYSTEM_TIME  
  
**Gilt für:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher und [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Im folgenden Beispiel wird das Argument FOR SYSTEM_TIME AS OF date_time_literal_or_variable verwendet, um Tabellenzeilen zurückzugeben, die am 1. Januar 2014 aktuell waren.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME AS OF '2014-01-01'  
WHERE ManagerID = 5;
```  
  
Im folgenden Beispiel wird das Argument „FOR SYSTEM_TIME FROM date_time_literal_or_variable TO date_time_literal_or_variable“ verwendet, um alle Zeilen zurückzugeben, die in dem Zeitraum aktiv waren, für dessen Beginn der 1. Januar 2013 und für dessen Ende der 1. Januar 2014 ohne oberen Grenzwert definiert wurde.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME FROM '2013-01-01' TO '2014-01-01'  
WHERE ManagerID = 5;
```  
  
 Im folgenden Beispiel wird das Argument FOR SYSTEM_TIME BETWEEN date_time_literal_or_variable AND date_time_literal_or_variable verwendet, um alle Zeilen zurückzugeben, die in dem Zeitraum aktiv waren, für dessen Beginn der 1. Januar 2013 und für dessen Ende der 1. Januar 2014 mit oberem Grenzwert definiert wurde.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME BETWEEN '2013-01-01' AND '2014-01-01'  
WHERE ManagerID = 5;
```  
  
 Im folgenden Beispiel wird das Argument FOR SYSTEM_TIME CONTAINED IN ( date_time_literal_or_variable, date_time_literal_or_variable ) verwendet, um alle Zeilen zurückzugeben, die in dem Zeitraum geöffnet und geschlossen wurden, für dessen Beginn der 1. Januar 2013 und für dessen Ende der 1. Januar 2014 definiert wurde.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME CONTAINED IN ( '2013-01-01', '2014-01-01' )  
WHERE ManagerID = 5;
```  
  
 Im folgenden Beispiel wird anstelle eines Literals eine Variable verwendet, um die Datumsgrenzwerte für die Abfrage bereitzustellen.  
  
```sql
DECLARE @AsOfFrom datetime2 = dateadd(month,-12, sysutcdatetime());
DECLARE @AsOfTo datetime2 = dateadd(month,-6, sysutcdatetime());
  
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME FROM @AsOfFrom TO @AsOfTo  
WHERE ManagerID = 5;
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="n-using-the-inner-join-syntax"></a>N. Verwenden der INNER JOIN-Syntax  
 Im folgenden Beispiel werden die Spalten `SalesOrderNumber`, `ProductKey` und `EnglishProductName` aus den Tabellen `FactInternetSales` und `DimProduct` zurückgegeben, wobei der Joinschlüssel `ProductKey` in beiden Tabellen übereinstimmt. Die Spalten `SalesOrderNumber` und `EnglishProductName` sind nur in einer der Tabellen vorhanden. Daher muss mit diesen Spalten kein Tabellenalias angegeben werden. Diese Aliase werden lediglich aus Gründen der besseren Lesbarkeit eingefügt. Das Wort **AS** vor einem Aliasnamen ist nicht erforderlich, wird jedoch aus Gründen der besseren Lesbarkeit sowie zur Einhaltung des ANSI-Standards empfohlen.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
INNER JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 Da das Schlüsselwort `INNER` für innere Joins nicht benötigt wird, kann diese Abfrage auch wie folgt formuliert werden:  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
JOIN DimProduct AS dp  
ON dp.ProductKey = fis.ProductKey;  
```  
  
 Eine `WHERE`-Klausel kann zur Begrenzung der Ergebnisse auch mit der folgenden Abfrage verwendet werden. In diesem Beispiel werden die Ergebnisse auf `SalesOrderNumber`-Werte begrenzt, die größer als „SO5000“ sind:  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey  
WHERE fis.SalesOrderNumber > 'SO50000'  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="o-using-the-left-outer-join-and-right-outer-join-syntax"></a>O. Verwenden der LEFT OUTER JOIN- und RIGHT OUTER JOIN-Syntax  
 Im folgenden Beispiel werden die Tabellen `FactInternetSales` und `DimProduct` in den `ProductKey`-Spalten verknüpft. In der LEFT OUTER JOIN-Syntax werden die Zeilen ohne Entsprechung aus der linken Tabelle (`FactInternetSales`) beibehalten. Da die `FactInternetSales`-Tabelle nur `ProductKey`-Werte enthält, die mit der `DimProduct`-Tabelle übereinstimmen, gibt diese Abfrage dieselben Zeilen wie im Beispiel für den inneren Join weiter oben zurück.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
LEFT OUTER JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 Diese Abfrage kann auch ohne das Schlüsselwort `OUTER` formuliert werden.  
  
 In der RIGHT OUTER JOIN-Syntax werden Zeilen ohne Entsprechung aus der rechten Tabelle beibehalten. Im folgenden Beispiel werden dieselben Zeilen wie im Beispiel für den linken äußeren Join weiter oben zurückgegeben.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM DimProduct AS dp 
RIGHT OUTER JOIN FactInternetSales AS fis  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 In der folgenden Abfrage wird die `DimSalesTerritory`-Tabelle als linke Tabelle in einem linken äußeren Join verwendet. Die `SalesOrderNumber`-Werte werden aus der `FactInternetSales`-Tabelle abgerufen. Wenn für einen bestimmten `SalesTerritoryKey`-Schlüssel keine Aufträge vorliegen, gibt die Abfrage für diese Zeile für `SalesOrderNumber` NULL zurück. Diese Abfrage wird nach der `SalesOrderNumber`-Spalte sortiert, sodass NULL-Werte in dieser Spalte oben in den Ergebnissen angezeigt werden.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
LEFT OUTER JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
 Diese Abfrage kann mit einem rechten äußeren Join neu geschrieben werden, um dieselben Ergebnisse abzurufen:  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM FactInternetSales AS fis 
RIGHT OUTER JOIN DimSalesTerritory AS dst  
    ON fis.SalesTerritoryKey = dst.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="p-using-the-full-outer-join-syntax"></a>P. Verwenden der FULL OUTER JOIN-Syntax  
 Im folgenden Beispiel ist ein vollständiger äußerer Join dargestellt, der alle Zeilen aus beiden verknüpften Tabellen zurückgibt, jedoch für Werte ohne Entsprechung in der anderen Tabelle NULL zurückgibt.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
FULL OUTER JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
 Diese Abfrage kann auch ohne das Schlüsselwort `OUTER` formuliert werden.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
FULL JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="q-using-the-cross-join-syntax"></a>Q. Verwenden der CROSS JOIN-Syntax  
 Im folgenden Beispiel wird das Kreuzprodukt der Tabellen `FactInternetSales` und `DimSalesTerritory` zurückgegeben. Eine Liste aller möglichen Kombinationen von `SalesOrderNumber` und `SalesTerritoryKey` wird zurückgegeben. Beachten Sie, dass in der Cross Join-Abfrage die `ON`-Klausel fehlt.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
CROSS JOIN FactInternetSales AS fis  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="r-using-a-derived-table"></a>R. Verwenden einer abgeleiteten Tabelle  
 Im folgenden Beispiel wird eine abgeleitete Tabelle (eine `SELECT`-Anweisung nach der `FROM`-Klausel) verwendet, um die Spalten `CustomerKey` und `LastName` aller Kunden in der Tabelle `DimCustomer` zurückzugeben, bei denen die `BirthDate`-Werte nach dem 1. Januar 1970 liegen und der Nachname „Smith“ angegeben ist.  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey, LastName  
FROM  
   (SELECT * FROM DimCustomer  
    WHERE BirthDate > '01/01/1970') AS DimCustomerDerivedTable  
WHERE LastName = 'Smith'  
ORDER BY LastName;  
```  
  
### <a name="s-reduce-join-hint-example"></a>S. Beispiel für einen REDUCE-Joinhinweis  
 Im folgenden Beispiel wird der `REDUCE`-Joinhinweis verwendet, um die Verarbeitung der abgeleiteten Tabelle in der Abfrage zu ändern. Wenn in dieser Abfrage der `REDUCE`-Joinhinweis verwendet wird, wird `fis.ProductKey` projiziert, repliziert und unterscheidbar gemacht und anschließend beim Mischen von `DimProduct` in `ProductKey` mit `DimProduct` verknüpft. Die resultierende abgeleitete Tabelle wird in `fis.ProductKey` verteilt.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN SELECT SalesOrderNumber  
FROM  
   (SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
    FROM DimProduct AS dp   
      INNER REDUCE JOIN FactInternetSales AS fis   
          ON dp.ProductKey = fis.ProductKey  
   ) AS dTable  
ORDER BY SalesOrderNumber;  
```  
  
### <a name="t-replicate-join-hint-example"></a>T. Beispiel für einen REPLICATE-Joinhinweis  
 Im folgenden Beispiel wird dieselbe Abfrage wie im vorherigen Beispiel verwendet, jedoch mit der Ausnahme, dass anstelle des `REDUCE`-Joinhinweises ein `REPLICATE`-Joinhinweis verwendet wird. Wenn der `REPLICATE`-Hinweis verwendet wird, werden die Werte in der Verknüpfungsspalte `ProductKey` aus der Tabelle `FactInternetSales` auf allen Knoten repliziert. Die Tabelle `DimProduct` wird mit der replizierten Version dieser Werte verknüpft.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN SELECT SalesOrderNumber  
FROM  
   (SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
    FROM DimProduct AS dp   
      INNER REPLICATE JOIN FactInternetSales AS fis  
          ON dp.ProductKey = fis.ProductKey  
   ) AS dTable  
ORDER BY SalesOrderNumber;  
```  
  
### <a name="u-using-the-redistribute-hint-to-guarantee-a-shuffle-move-for-a-distribution-incompatible-join"></a>U. Verwenden des REDISTRIBUTE-Hinweises zur Gewährleistung einer Verschiebung mit Vermischung für einen verteilungsinkompatiblen Join  
 In der folgenden Abfrage wird der REDISTRIBUTE-Abfragehinweis in einem verteilungsinkompatiblen Join verwendet. Dadurch wird sichergestellt, dass der Abfrageoptimierer im Abfrageplan eine Verschiebung mit Vermischung verwendet. Ferner wird dadurch sichergestellt, dass im Abfrageplan keine Übertragungs-Verschiebung verwendet wird, bei der eine verteilte Tabelle in eine replizierte Tabelle verschoben wird.  
  
 Im folgenden Beispiel erzwingt der REDISTRIBUTE-Hinweis in der FactInternetSales-Tabelle eine Verschiebung mit Vermischung, weil ProductKey die Verteilungsspalte für DimProduct und nicht die Verteilungsspalte für FactInternetSales ist.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN  
SELECT dp.ProductKey, fis.SalesOrderNumber, fis.TotalProductCost  
FROM DimProduct AS dp 
INNER REDISTRIBUTE JOIN FactInternetSales AS fis  
    ON dp.ProductKey = fis.ProductKey;  
```  

### <a name="v-using-tablesample-to-read-data-from-a-sample-of-rows-in-a-table"></a>V. Verwenden von TABLESAMPLE, um Daten von einem Beispiel für Zeilen in einer Tabelle zu lesen  
 Im folgenden Beispiel wird `TABLESAMPLE` in der `FROM`-Klausel verwendet, um ungefähr `10`-Prozent aller Zeilen in der `Customer`-Tabelle zurückzugeben.  
  
```sql    
SELECT *  
FROM Sales.Customer TABLESAMPLE SYSTEM (10 PERCENT) ;
```
  
## <a name="see-also"></a>Weitere Informationen  
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [OPENQUERY &#40;Transact-SQL&#41;](../../t-sql/functions/openquery-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)
