---
description: Hinweise (Transact-SQL) – Abfrage
title: Abfragehinweise (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/27/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Query_Hint_TSQL
- Query_TSQL
- Query
- Query Hint
- MAX_GRANT_PERCENT
- MAX_GRANT_PERCENT_TSQL
- MIN_GRANT_PERCENT
- MIN_GRANT_PERCENT_TSQL
- EXTERNALPUSHDOWN
- EXTERNALPUSHDOWN_TSQL
- NOLOCK_TSQL
- MAXDOP_TSQL
- USE_HINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REPORT PLAN query hint
- FORCE ORDER query hint
- HASH JOIN query hint
- query hints [SQL Server]
- OPTIMIZE FOR query hint
- FORCESCAN query hint
- RECOMPILE query hint
- MAXRECURSION query hint
- MERGE JOIN query hint
- HASH GROUP query hint
- KEEP PLAN query hint
- UNION query hint
- FORCESEEK query hint
- ORDER GROUP query hint
- LOOP JOIN query hint
- KEEPFIXED PLAN query hint
- FAST query hint
- MAXDOP query hint
- PARAMETERIZATION query hint
- hints [SQL Server], query
- JOIN query hint
- USE PLAN query hint
- EXPAND VIEWS query hint
- MAX_GRANT_PERCENT query hint
- MIN_GRANT_PERCENT query hint
- EXTERNALPUSHDOWN query hint
- USE HINT query hint
- QUERY_PLAN_PROFILE query hint
ms.assetid: 66fb1520-dcdf-4aab-9ff1-7de8f79e5b2d
author: pmasl
ms.author: pelopes
ms.openlocfilehash: fd24bf1add1746687edfd86679813718050236ba
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100341592"
---
# <a name="hints-transact-sql---query"></a>Hinweise (Transact-SQL) – Abfrage
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Abfragehinweise sind Hinweise, die im Bereich einer Abfrage verwendet werden. Sie wirken sich auf alle Operatoren in der Anweisung aus. Falls UNION in der Hauptabfrage vorkommt, kann nur die letzte Abfrage, die eine UNION-Operation enthält, die OPTION-Klausel aufweisen. Abfragehinweise werden als Teil der [OPTION-Klausel](../../t-sql/queries/option-clause-transact-sql.md) angegeben. Fehler 8622 tritt auf, wenn mindestens ein Abfragehinweis dazu führt, dass der Abfrageoptimierer keinen gültigen Plan generiert.  
  
> [!CAUTION]  
> Da der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abfrageoptimierer in der Regel den besten Ausführungsplan für eine Abfrage auswählt, wird empfohlen, dass nur erfahrene Entwickler und Datenbankadministratoren Hinweise verwenden, wenn alle anderen Möglichkeiten sich als unbefriedigend erwiesen haben.  
  
**Anwendungsbereich:**  
  
[DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
[INSERT](../../t-sql/statements/insert-transact-sql.md)  
  
[SELECT](../../t-sql/queries/select-transact-sql.md)  
  
[UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
[MERGE](../../t-sql/statements/merge-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
<query_hint> ::=   
{ { HASH | ORDER } GROUP   
  | { CONCAT | HASH | MERGE } UNION   
  | { LOOP | MERGE | HASH } JOIN   
  | EXPAND VIEWS   
  | FAST <integer_value>   
  | FORCE ORDER   
  | { FORCE | DISABLE } EXTERNALPUSHDOWN
  | { FORCE | DISABLE } SCALEOUTEXECUTION
  | IGNORE_NONCLUSTERED_COLUMNSTORE_INDEX  
  | KEEP PLAN   
  | KEEPFIXED PLAN  
  | MAX_GRANT_PERCENT = <numeric_value>  
  | MIN_GRANT_PERCENT = <numeric_value>  
  | MAXDOP <integer_value>   
  | MAXRECURSION <integer_value>   
  | NO_PERFORMANCE_SPOOL   
  | OPTIMIZE FOR ( @variable_name { UNKNOWN | = <literal_constant> } [ , ...n ] )  
  | OPTIMIZE FOR UNKNOWN  
  | PARAMETERIZATION { SIMPLE | FORCED }   
  | QUERYTRACEON <integer_value>   
  | RECOMPILE  
  | ROBUST PLAN   
  | USE HINT ( <use_hint_name> [ , ...n ] )
  | USE PLAN N'<xml_plan>'  
  | TABLE HINT ( <exposed_object_name> [ , <table_hint> [ [, ]...n ] ] )  
}  
  
<table_hint> ::=  
{ NOEXPAND [ , INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> ) ]  
  | INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> )
  | FORCESEEK [ ( <index_value> ( <index_column_name> [,... ] ) ) ]  
  | FORCESCAN  
  | HOLDLOCK   
  | NOLOCK   
  | NOWAIT  
  | PAGLOCK   
  | READCOMMITTED   
  | READCOMMITTEDLOCK   
  | READPAST   
  | READUNCOMMITTED   
  | REPEATABLEREAD   
  | ROWLOCK   
  | SERIALIZABLE   
  | SNAPSHOT  
  | SPATIAL_WINDOW_MAX_CELLS = <integer_value>  
  | TABLOCK   
  | TABLOCKX   
  | UPDLOCK   
  | XLOCK  
}  

<use_hint_name> ::=
{ 'ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS'
  | 'ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES'
  | 'DISABLE_BATCH_MODE_ADAPTIVE_JOINS'
  | 'DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK'
  | 'DISABLE_DEFERRED_COMPILATION_TV'
  | 'DISABLE_INTERLEAVED_EXECUTION_TVF'
  | 'DISABLE_OPTIMIZED_NESTED_LOOP'
  | 'DISABLE_OPTIMIZER_ROWGOAL'
  | 'DISABLE_PARAMETER_SNIFFING'
  | 'DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK'
  | 'DISABLE_TSQL_SCALAR_UDF_INLINING'
  | 'DISALLOW_BATCH_MODE'
  | 'ENABLE_HIST_AMENDMENT_FOR_ASC_KEYS'
  | 'ENABLE_QUERY_OPTIMIZER_HOTFIXES'
  | 'FORCE_DEFAULT_CARDINALITY_ESTIMATION'
  | 'FORCE_LEGACY_CARDINALITY_ESTIMATION'
  | 'QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n'
  | 'QUERY_PLAN_PROFILE' 
}
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
{ HASH | ORDER } GROUP  
Gibt an, dass die in der GROUP BY- oder DISTINCT-Klausel der Abfrage beschriebenen Aggregationen Hash- oder Sortiervorgänge verwenden sollen.  
  
{ MERGE | HASH | CONCAT } UNION  
Gibt an, dass alle UNION-Vorgänge mithilfe von Merge-, Hash- oder Verkettungsvorgängen für die bei UNION vorkommenden Mengen ausgeführt werden. Wenn mehr als ein UNION-Hinweis angegeben wird, wählt der Abfrageoptimierer unter den angegebenen Hinweisen die Strategie mit dem geringsten Aufwand aus.  
  
{ LOOP | MERGE | HASH } JOIN  
Gibt an, dass alle Joinvorgänge per LOOP JOIN, MERGE JOIN oder HASH JOIN in der gesamten Abfrage ausgeführt werden. Wenn Sie mehr als einen Joinhinweis angeben, wählt der Optimierer unter den zulässigen Hinweisen die Strategie mit dem geringsten Aufwand aus.  
  
Wenn Sie in der FROM-Klausel derselben Abfrage für ein bestimmtes Tabellenpaar einen Joinhinweis angeben, hat dieser Joinhinweis Vorrang bei der Verknüpfung der beiden Tabellen. Die Abfragehinweise müssen jedoch auch berücksichtigt werden. Der Joinhinweis kann für das Tabellenpaar nur die Auswahl der zulässigen Joinmethoden für den Abfragehinweis einschränken. Weitere Informationen finden Sie unter [Joinhinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-join.md).  
  
EXPAND VIEWS  
Gibt an, dass die indizierten Sichten erweitert werden. Gibt auch an, dass der Abfrageoptimierer keine indizierte Sicht als Ersatz für einen Abfrageteil berücksichtigt. Eine Sicht wird erweitert, wenn die Sichtdefinition im Abfragetext den Sichtnamen ersetzt.  
  
Dieser Abfragehinweis lässt die direkte Verwendung von indizierten Sichten und Indizes für indizierte Sichten im Abfrageplan praktisch nicht zu.  
  
> [!NOTE]
> Die indizierte Sicht wird nicht erweitert, wenn ein direkter Verweis auf die Ansicht im SELECT-Teil der Abfrage vorhanden ist. Bei Angabe von WITH (NOEXPAND) oder WITH (NOEXPAND, INDEX( _<Indexwert>_ [ **,** _...n_ ] ) ) wird die Sicht ebenfalls nicht erweitert. Weitere Informationen zum Abfragehinweis NOEXPAND finden Sie unter [Using NOEXPAND (Verwenden von NOEXPAND)](../../t-sql/queries/hints-transact-sql-table.md#using-noexpand).  
  
Der Hinweis wirkt sich nur auf die Ansichten im SELECT-Teil von Anweisungen aus, einschließlich der Ansichten in den Anweisungen INSERT, UPDATE, MERGE und DELETE.  
  
FAST _<ganzzahliger\_Wert>_  
Gibt an, dass die Abfrage für den schnellen Abruf der ersten _<ganzzahliger\_Wert>_ Zeilen optimiert ist. Dieses Ergebnis ist eine nicht negative ganze Zahl. Nachdem die ersten _<ganzzahliger\_Wert>_ Zeilen zurückgegeben wurden, wird die Abfrage fortgesetzt und erzeugt das vollständige Resultset.  
  
FORCE ORDER  
Gibt an, dass die von der Abfragesyntax angegebene Joinreihenfolge während der Abfrageoptimierung beibehalten wird. Die Verwendung von FORCE ORDER hat keine Auswirkung auf das mögliche Rollentauschverhalten des Abfrageoptimierers.  
  
> [!NOTE]  
> In einer MERGE-Anweisung wird als Standardreihenfolge für Joins zunächst auf die Quelltabelle und dann auf die Zieltabelle zugegriffen, es sei denn, die WHEN SOURCE NOT MATCHED-Klausel wurde angegeben. Wenn Sie FORCE ORDER angeben, wird dieses Standardverhalten beibehalten.  
  
{ FORCE | DISABLE } EXTERNALPUSHDOWN  
Erzwingen oder Deaktivieren der Weitergabe der Berechnung von qualifizierenden Ausdrücken in Hadoop. Gilt nur für Abfragen mit PolyBase. Wird nicht in den Azure-Speicher weitergegeben.  

{ FORCE | DISABLE } SCALEOUTEXECUTION: Hiermit wird die Ausführung des Aufskalierens von PolyBase-Abfragen erzwungen oder deaktiviert, die externe Tabellen in SQL Server 2019-Big Data Clustern verwenden. Dieser Hinweis wird nur von einer Abfrage berücksichtigt, die die Masterinstanz eines SQL-Big Data-Clusters verwendet. Das Aufskalieren erfolgt über den Computepool des Big Data-Clusters. 

KEEP PLAN  
Zwingt den Abfrageoptimierer, den geschätzten Neukompilierungsschwellenwert für eine Abfrage zu lockern. Der geschätzte Recompile-Neukompilierungsschwellenwert startet ein automatisches Neukompilieren für die Abfrage, wenn die geschätzte Anzahl von indizierten Spaltenänderungen in einer Tabelle durch Ausführen einer der folgenden Anweisungen vorgenommen wurde:

* UPDATE
* Delete
* MERGE
* INSERT

Durch Angeben von KEEP PLAN wird sichergestellt, dass eine Abfrage nicht zu häufig erneut kompiliert wird, wenn an einer Tabelle mehrere Updates ausgeführt werden.  
  
KEEPFIXED PLAN  
Zwingt den Abfrageoptimierer, die Abfrage aufgrund von Änderungen in den Statistiken nicht erneut zu kompilieren. Durch Angabe von KEEPFIXED PLAN wird sichergestellt, dass eine Abfrage nur dann erneut kompiliert wird, wenn das Schema der zugrunde liegenden Tabellen geändert wird oder für diese Tabellen **sp_recompile** ausgeführt wird.  
  
IGNORE_NONCLUSTERED_COLUMNSTORE_INDEX       
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).  
  
Verhindert, dass die Abfrage einen nicht gruppierten speicheroptimierten Columnstore-Index verwendet. Wenn die Abfrage den Abfragehinweis enthält, der die Verwendung des Columnstore-Indexes verhindert, sowie einen Indexhinweis, der die Verwendung eines Columnstore-Index festlegt, besteht ein Konflikt zwischen den Hinweisen, und die Abfrage gibt einen Fehler zurück.  
  
MAX_GRANT_PERCENT = <numerischer_Wert>     
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  

Die maximale Größe der Speicherzuweisung in Prozent des konfigurierten Arbeitsspeicherlimits. Die Abfrage überschreitet diese Begrenzung garantiert nicht. Die tatsächliche Begrenzung kann niedriger sein, wenn die Resource Governor-Einstellung niedriger ist als die durch diesen Hinweis angegebene Begrenzung. Gültige Werte liegen in einem Bereich zwischen 0,0 und 100,0.  
  
MIN_GRANT_PERCENT = <numerischer_Wert>        
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   

Die minimale Größe der Speicherzuweisung in Prozent des konfigurierten Arbeitsspeicherlimits. Für die Abfrage wird `MAX(required memory, min grant)` garantiert,weil zumindest der für das Starten einer Abfrage benötigte Speicher erforderlich ist. Gültige Werte liegen in einem Bereich zwischen 0,0 und 100,0.  
 
MAXDOP <ganzzahliger_Wert>      
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
Überschreibt die Konfigurationsoption **max degree of parallelism** von **sp_configure**. Überschreibt auch den Resource Governor für die Abfrage, die diese Option angibt. Der MAXDOP-Abfragehinweis kann den mit sp_configure konfigurierten Wert überschreiten. Wenn MAXDOP den mit der Ressourcenkontrolle konfigurierten Wert überschreitet, verwendet [!INCLUDE[ssDE](../../includes/ssde-md.md)] den MAXDOP-Wert der Ressourcenkontrolle, wie unter [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md) beschrieben. Alle mit der Konfigurationsoption **Max. Grad an Parallelität** verwendeten semantischen Regeln gelten auch für die Verwendung des MAXDOP-Abfragehinweises. Weitere Informationen finden Sie unter [Konfigurieren der Serverkonfigurationsoption Max. Grad an Parallelität](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
  
> [!WARNING]     
> Wenn MAXDOP auf 0 (null) festgelegt wird, wählt der Server den maximalen Grad an Parallelität aus.  
  
MAXRECURSION <ganzzahliger_Wert>     
Gibt die maximale Anzahl der für diese Abfrage zulässigen Rekursionen an. _number_ ist eine nicht negative ganze Zahl zwischen 0 und 32.767. Wenn 0 angegeben wird, wird keine Beschränkung angewendet. Wenn diese Option nicht angegeben wird, beträgt das Standardlimit für den Server 100.  
  
Wenn der angegebene Wert bzw. der Standardwert für MAXRECURSION während der Ausführung der Abfrage erreicht wird, wird die Abfrage beendet und ein Fehler wird zurückgegeben.  
  
Aufgrund dieses Fehlers wird für alle Änderungen aufgrund der Anweisung ein Rollback ausgeführt. Falls es sich hierbei um eine SELECT-Anweisung handelt, können Teilergebnisse oder keine Ergebnisse zurückgegeben werden. Teilergebnisse schließen möglicherweise nicht alle Zeilen auf Rekursionsebenen ein, die über die angegebene maximale Rekursionsebene hinausgehen.  
  
Weitere Informationen finden Sie unter [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).     
  
NO_PERFORMANCE_SPOOL    
**Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
  
Verhindert, dass ein Spool-Operator zu Abfrageplänen hinzugefügt wird (mit Ausnahme der Pläne, bei denen der Spool-Operator eine gültige Update-Semantik garantieren muss). In einigen Szenarios kann der Spool-Operator die Leistung beeinträchtigen. Der Spool-Operator verwendet beispielsweise „tempdb“. Wenn in den Spool-Vorgängen viele Abfragen gleichzeitig ausgeführt werden, kann es zu einem tempdb-Konflikt kommen.  
  
OPTIMIZE FOR ( _\@Variablenname_ { UNKNOWN | = <literal_constant> }_ [ **,** ..._n_ ] )     
Weist den Abfrageoptimierer an, einen bestimmten Wert für eine lokale Variable zu verwenden, wenn die Abfrage kompiliert und optimiert wird. Dieser Wert wird nur während der Abfrageoptimierung verwendet, nicht während der Abfrageausführung.  
  
_\@variable\_name_  
Der Name einer lokalen Variablen, die in einer Abfrage verwendet wird und der ein Wert für die Verwendung mit dem OPTIMIZE FOR-Abfragehinweis zugewiesen werden kann.  
  
_UNKNOWN_  
Gibt an, dass der Abfrageoptimierer statt des Anfangswerts statistische Daten verwenden soll, um während der Abfrageoptimierung den Wert einer lokalen Variablen zu bestimmen.  
  
_literal\_constant_  
Ein Literalkonstantenwert, dem _\@variable\_name_ für die Verwendung mit dem Abfragehinweis „OPTIMIZE FOR“ zugewiesen wird. _literal\_constant_ wird nur während der Abfrageoptimierung verwendet, nicht als Wert von _\@variable\_name_ während der Abfrageausführung. _literal\_constant_ kann einen beliebigen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemdatentyp aufweisen, der als Literalkonstante dargestellt werden kann. Der Datentyp von _literal\_constant_ muss implizit in den Datentyp konvertierbar sein, auf den _\@variable\_name_ in der Abfrage verweist.  
  
OPTIMIZE FOR kann dem Erkennungsverhalten der Standardparameter des Optimierers entgegenwirken. Verwenden Sie OPTIMIZE FOR auch, wenn Sie Planhinweislisten erstellen. Weitere Informationen finden Sie unter [Erneutes Kompilieren einer gespeicherten Prozedur](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md).  
  
OPTIMIZE FOR UNKNOWN  
Weist den Abfrageoptimierer an, für alle Spaltenwerte die durchschnittliche Selektivität des Prädikats anstelle des Werts für den Runtime-Parameter zu verwenden, wenn die Abfrage kompiliert und optimiert wird.  
  
Wenn OPTIMIZE FOR @variable_name = _literal\_constant_ und OPTIMIZE FOR UNKNOWN in dem gleichen Abfragehinweis verwendet werden, verwendet der Abfrageoptimierer die _literal\_constant_, die für einen bestimmten Wert angegeben wurde. Der Abfrageoptimierer verwendet UNKNOWN für die übrigen Variablenwerte. Diese Werte werden nur während der Abfrageoptimierung verwendet, nicht während der Abfrageausführung.  

PARAMETERIZATION { SIMPLE | FORCED }     
Gibt die Parametrisierungsregeln an, die der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abfrageoptimierer bei der Kompilierung auf die Abfrage anwendet.  
  
> [!IMPORTANT]  
> Der PARAMETERIZATION-Abfragehinweis kann in einer Planhinweisliste nur angegeben werden, um die aktuelle Einstellung der Option PARAMETERIZATION database SET zu überschreiben. Er kann nicht direkt innerhalb einer Abfrage angegeben werden.    
> Weitere Informationen finden Sie unter [Angeben des Abfrageparametrisierungsverhaltens mithilfe von Planhinweislisten](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md).
  
Mit SIMPLE wird der Abfrageoptimierer angewiesen, einfache Parametrisierung auszuführen. Mit FORCED wird der Abfrageoptimierer angewiesen, eine erzwungene Parametrisierung auszuführen. Weitere Informationen finden Sie unter [„Erzwungene Parametrisierung“ im Handbuch zur Architektur der Abfrageverarbeitung](../../relational-databases/query-processing-architecture-guide.md#ForcedParam) und unter [„Einfache Parametrisierung“ im Handbuch zur Architektur der Abfrageverarbeitung](../../relational-databases/query-processing-architecture-guide.md#SimpleParam).  

QUERYTRACEON <ganzzahliger_Wert>    
Mit dieser Option können Sie ein Ablaufverfolgungsflag, das sich auf den Plan auswirkt, nur während der Kompilierung einer einzelnen Abfrage aktivieren. Wie andere Optionen auf Abfrageebene können Sie diese Option zusammen mit Planhinweislisten verwenden, um den Text einer Abfrage, der in einer beliebigen Sitzung ausgeführt wird, abzugleichen, und automatisch ein Ablaufverfolgungsflag anzuwenden, das sich auf den Plan auswirkt, wenn die Abfrage kompiliert wird. Die Option QUERYTRACEON wird nur für Ablaufverfolgungsflags des Abfrageoptimierers unterstützt. Weitere Informationen finden Sie unter [Ablaufverfolgungsflags](../database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). 

> [!NOTE]  
> Bei dieser Option wird jedoch weder ein Fehler noch eine Warnung zurückgegeben, wenn eine nicht unterstützte Ablaufverfolgungsflagnummer verwendet wird. Wenn das angegebene Ablaufverfolgungsflag keines ist, das sich auf den Ausführungsplan der Abfrage auswirkt, wird die Option einfach ignoriert.

> [!NOTE]  
> Wenn Sie mehr als ein Ablaufverfolgungsflag in einer Abfrage verwenden möchten, geben Sie einen QUERYTRACEON-Hinweis für jede Ablaufverfolgungsflagnummer an.

RECOMPILE  
Weist [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] an, einen neuen, temporären Plan für die Abfrage zu erstellen und diesen Plan sofort nach Ausführung der Abfrage zu verwerfen. Der generierte Abfrageplan ersetzt keinen im Cache gespeicherten Plan, wenn dieselbe Abfrage ohne den Hinweis RECOMPILE ausgeführt wird. Ohne die Angabe von RECOMPILE werden Abfragepläne von [!INCLUDE[ssDE](../../includes/ssde-md.md)] zwischengespeichert und wiederverwendet. Beim Kompilieren von Abfrageplänen verwendet der RECOMPILE-Abfragehinweis die aktuellen Werte aller lokalen Variablen in der Abfrage. Wenn sich die Abfrage innerhalb einer gespeicherten Prozedur befindet, werden die aktuellen Werte an beliebige Parameter übergeben.  
  
RECOMPILE ist eine nützliche Alternative zum Erstellen einer gespeicherten Prozedur. RECOMPILE verwendet die WITH RECOMPILE-Klausel, wenn nicht die gesamte gespeicherte Prozedur, sondern nur eine Teilmenge davon erneut kompiliert werden muss. Weitere Informationen finden Sie unter [Erneutes Kompilieren einer gespeicherten Prozedur](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md). RECOMPILE ist auch beim Erstellen von Planhinweislisten hilfreich.  
  
ROBUST PLAN  
Zwingt den Abfrageoptimierer zu einer Vorgehensweise, bei der der Schwerpunkt auf der maximalen potenziellen Zeilengröße liegt. Dies geht möglicherweise zu Lasten der Leistung. Bei der Verarbeitung der Abfrage müssen möglicherweise Zwischentabellen und Operatoren Zeilen speichern und verarbeiten, die größer sind als alle Eingabezeilen, wenn die Abfrage verarbeitet wird. Die Zeilen können so groß sein, dass der jeweilige Operator in einigen Fällen die Zeile nicht verarbeiten kann. Wenn Zeilen so groß sind, gibt [!INCLUDE[ssDE](../../includes/ssde-md.md)] während der Ausführung der Abfrage einen Fehler aus. Durch das Verwenden von ROBUST PLAN weisen Sie den Abfrageoptimierer an, keine Abfragepläne in Betracht zu ziehen, für die möglicherweise dieses Problem auftritt.  
  
Ist eine solche Vorgehensweise nicht möglich, gibt der Abfrageoptimierer einen Fehler zurück, statt die Fehlererkennung auf die Abfrageausführung zu verschieben. Die Zeilen können Spalten variabler Länge aufweisen. [!INCLUDE[ssDE](../../includes/ssde-md.md)] läßt die Definition von Zeilen zu, deren maximale potenzielle Größe von [!INCLUDE[ssDE](../../includes/ssde-md.md)] nicht mehr verarbeitet werden kann. Trotz der maximalen potenziellen Größe speichert eine Anwendung im Allgemeinen Zeilen, deren tatsächliche Größe innerhalb der Höchstwerte liegen, die [!INCLUDE[ssDE](../../includes/ssde-md.md)] verarbeiten kann. Wenn [!INCLUDE[ssDE](../../includes/ssde-md.md)] eine Zeile ermittelt, die zu lang ist, wird ein Ausführungsfehler zurückgegeben.  
 
<a name="use_hint"></a> USE HINT ( **'** _hint\_name_ **'** )    
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
 
Stellt eine oder mehrere zusätzliche Hinweise für den Abfrageprozessor bereit. Die zusätzlichen Hinweise werden von einem Hinweisnamen **innerhalb einfacher Anführungszeichen** angegeben.   

Die folgenden Hinweisnamen werden unterstützt:    
 
*  'ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS' <a name="use_hint_join_containment"></a>       
   Bewirkt, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unter dem [Kardinalitätsschätzungsmodell](../../relational-databases/performance/cardinality-estimation-sql-server.md) für den Abfrageoptimierer von [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] oder einer neueren Version einen Abfrageplan mithilfe der Simple-Containment-Annahme statt mit der Base-Containment-Annahme generiert. Dieser Hinweisname entspricht [Ablaufverfolgungsflag](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9476. 
*  'ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES' <a name="use_hint_correlation"></a>      
   Bewirkt, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen Plan mit minimaler Selektivität generiert, wenn AND-Prädikate für Filter geschätzt werden, die bei der Korrelation berücksichtigt werden sollen. Dieser Hinweisname entspricht [Ablaufverfolgungsflag](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4137, wenn es mit dem Kardinalitätsschätzungsmodell von [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und früheren Versionen verwendet wird, und hat ähnliche Auswirkungen, wenn das [Ablaufverfolgungsflag](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9471 mit dem Kardinalitätsschätzungsmodell von [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] oder einer neueren Version verwendet wird.
*  'DISABLE_BATCH_MODE_ADAPTIVE_JOINS'       
   Deaktiviert Adaptive Joins im Batchmodus. Weitere Informationen finden Sie unter [Adaptive Joins im Batchmodus](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-adaptive-joins).     
   **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  'DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK'       
   Deaktiviert das Feedback zur Speicherzuweisung im Batchmodus. Weitere Informationen finden Sie unter [Feedback zur Speicherzuweisung im Batchmodus](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-memory-grant-feedback).     
   **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
* 'DISABLE_DEFERRED_COMPILATION_TV'    
  Deaktiviert die verzögerte Kompilierung von Tabellenvariablen. Weitere Informationen finden Sie unter [Verzögerte Kompilierung von Tabellenvariablen](../../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation).     
  **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  'DISABLE_INTERLEAVED_EXECUTION_TVF'      
   Deaktiviert die verschachtelte Ausführung mit Tabellenwertfunktionen mit mehreren Anweisungen. Weitere Informationen finden Sie unter [Verschachtelte Ausführung mit Tabellenwertfunktionen mit mehreren Anweisungen](../../relational-databases/performance/intelligent-query-processing.md#interleaved-execution-for-mstvfs).     
   **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  'DISABLE_OPTIMIZED_NESTED_LOOP'      
   Weist den Abfrageprozessor an, bei der Generierung eines Abfrageplans keine Sortiervorgänge (Batch-Sortierung) für optimierte Joins geschachtelter Schleifen zu verwenden. Dieser Hinweisname entspricht [Ablaufverfolgungsflag](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 2340.
*  'DISABLE_OPTIMIZER_ROWGOAL' <a name="use_hint_rowgoal"></a>      
   Veranlasst SQL Server, einen Plan zu erstellen, der keine Zeilenzieländerungen mit Abfragen verwendet, die diese Schlüsselwörter enthalten: 
   
   * TOP
   * OPTION (FAST N)
   * IN
   * EXISTS
   
   Dieser Hinweisname entspricht [Ablaufverfolgungsflag](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4138.
*  'DISABLE_PARAMETER_SNIFFING'      
   Weist den Abfrageoptimierer an, die durchschnittliche Datenverteilung zu verwenden, während er eine Abfrage mit einem oder mehreren Parametern kompiliert. Diese Anweisung macht den Abfrageplan unabhängig von dem Parameterwert, der beim Kompilieren der Abfrage zuerst verwendet wurde. Dieser Hinweisname entspricht [Ablaufverfolgungsflag](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4136 oder der Einstellung `PARAMETER_SNIFFING = OFF` für die [Datenbankweit gültige Konfiguration](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).
* 'DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK'    
  Deaktiviert das Feedback zur Speicherzuweisung im Zeilenmodus. Weitere Informationen finden Sie unter [Feedback zur Speicherzuweisung im Zeilenmodus](../../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback).      
  **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].     
* 'DISABLE_TSQL_SCALAR_UDF_INLINING'    
  Deaktiviert das Inlining benutzerdefinierter Skalarfunktionen. Weitere Informationen finden Sie unter [Scalar UDF Inlining (Inlining benutzerdefinierter Skalarfunktionen)](../../relational-databases/user-defined-functions/scalar-udf-inlining.md).     
  **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]).    
* 'DISALLOW_BATCH_MODE'    
  Deaktiviert die Batchmodusausführung. Weitere Informationen finden Sie unter [Ausführungsmodi](../../relational-databases/query-processing-architecture-guide.md#execution-modes).     
  **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].     
*  'ENABLE_HIST_AMENDMENT_FOR_ASC_KEYS'      
   Aktiviert automatisch generierte Schnellstatistiken (Histogrammzusatz) für alle führenden Indexspalten, für welche die Kardinalitätsschätzung erforderlich ist. Das für die Kardinalitätsschätzung verwendete Histogramm wird zum Zeitpunkt der Abfragekompilierung angepasst, damit der tatsächliche Höchst- und Mindestwert in dieser Spalte berücksichtigt werden. Dieser Hinweisname entspricht [Ablaufverfolgungsflag](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4139. 
*  'ENABLE_QUERY_OPTIMIZER_HOTFIXES'     
   Aktiviert Hotfixes für den Abfrageoptimierer (Änderungen wurden in kumulativen Updates und Service Packs von SQL Server veröffentlicht). Dieser Hinweisname entspricht [Ablaufverfolgungsflag](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4199 oder der Einstellung `QUERY_OPTIMIZER_HOTFIXES = ON` für die [Datenbankweit gültige Konfiguration](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).
*  'FORCE_DEFAULT_CARDINALITY_ESTIMATION'      
   Zwingt den Abfrageoptimierer, das [Kardinalitätsschätzungsmodell](../../relational-databases/performance/cardinality-estimation-sql-server.md) zu verwenden, das dem aktuellen Kompatibilitätsgrad der Datenbank entspricht. Verwenden Sie diesen Hinweis, um die Einstellung `LEGACY_CARDINALITY_ESTIMATION = ON` für die [Datenbankweit gültige Konfiguration](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) zu überschreiben, oder das [Ablaufverfolgungsflag](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9481.
*  'FORCE_LEGACY_CARDINALITY_ESTIMATION' <a name="use_hint_ce70"></a>      
   Zwingt den Abfrageoptimierer, das Modell [Kardinalitätsschätzung](../../relational-databases/performance/cardinality-estimation-sql-server.md) von [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und Vorgängerversionen zu verwenden. Dieser Hinweisname entspricht [Ablaufverfolgungsflag](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9481 oder der Einstellung `LEGACY_CARDINALITY_ESTIMATION = ON` für die [Datenbankweit gültige Konfiguration](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).
*  QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n          
 Erzwingt das Verhalten des Abfrageoptimierers auf Abfrageebene. Dieses Verhalten entspricht dem, wenn die Abfrage mit dem Datenbank-Kompatibilitätsgrad _n_ kompiliert wird, wobei _n_ ein unterstützter Datenbank-Kompatibilitätsgrad ist (z. B. 100 oder 130). Unter [sys.dm_exec_valid_use_hints](../../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql.md) finden Sie eine Liste der zurzeit unterstützten Werte für _n_.      
   **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU10).    

   > [!NOTE]
   > Der Hinweis QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n überschreibt keine standardmäßigen oder älteren Einstellungen für die Kardinalitätsschätzung, wenn er durch eine datenbankweite Konfiguration, ein Ablaufverfolgungsflag oder einen anderen Abfragehinweis wie QUERYTRACEON erzwungen wurde.   
   > Dieser Hinweis betrifft nur das Verhalten des Abfrageoptimierers. Er wirkt sich nicht auf andere Features von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus, die möglicherweise vom [Datenbank-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) abhängig sind, wie z.B. die Verfügbarkeit bestimmter Datenbankfeatures.  
   > Weitere Informationen zu diesem Hinweis finden Sie unter [Developer's Choice: Hinting Query Execution model (Von Entwicklern inspiriert: Modell für die Ausführung von Hinweisabfragen)](/archive/blogs/sql_server_team/developers-choice-hinting-query-execution-model).
    
*  'QUERY_PLAN_PROFILE'      
 Aktiviert einfache Profilerstellung für die Abfrage. Wenn eine Abfrage, die diesen neuen Hinweis enthält, abgeschlossen wird, wird ein neues erweitertes Ereignis, query_plan_profile, ausgelöst. Dieses erweiterte Ereignis macht Ausführungsstatistiken und ein tatsächliches Ausführungsplan-XML verfügbar, das dem erweiterten Ereignis query_post_execution_showplan ähnelt, aber nur für Abfragen, die den neuen Hinweis enthalten.    
   **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU3 und [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU11). 

   > [!NOTE]
   > Wenn Sie das Sammeln des erweiterten Ereignisses query_post_execution_showplan aktivieren, wird dadurch jeder Abfrage, die auf dem Server ausgeführt wird, eine Standard-Profilerstellungsinfrastruktur hinzugefügt, was die Gesamtleistung des Servers beeinträchtigen kann.      
   > Wenn Sie das Sammeln des erweiterten Ereignisses *query_thread_profile* stattdessen für die Verwendung der einfachen Profilerstellungsinfrastruktur aktivieren, führt dies zu einem wesentlich geringeren Verarbeitungsaufwand, wirkt sich aber immer noch auf die Gesamtleistung des Servers aus.       
   > Wenn Sie das erweiterte Ereignis „query_plan_profile“ aktivieren, aktiviert dies die einfache Profilerstellungsinfrastruktur nur für eine Abfrage, die mit dem QUERY_PLAN_PROFILE ausgeführt wurde, und wirkt sich deshalb nicht auf andere Workloads auf dem Server aus. Verwenden Sie diesen Hinweis, um ein Profil für eine bestimmte Abfrage zu erstellen, ohne dabei andere Teile der Serverworkload zu beeinträchtigen.
   > Weitere Informationen zur einfachen Profilerstellung finden Sie unter [Profilerstellungsinfrastruktur für Abfragen](../../relational-databases/performance/query-profiling-infrastructure.md).
 
Die Liste aller unterstützten USE HINT-Namen kann über die dynamische Verwaltungsansicht [sys.dm_exec_valid_use_hints](../../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql.md) abgefragt werden.    

> [!TIP]
> Bei Hinweisnamen muss die Groß-/Kleinschreibung nicht beachtet werden.   
  
> [!IMPORTANT] 
> Einige USE HINT-Hinweise stehen möglicherweise mit Ablaufverfolgungsflags, die auf globaler Ebene oder auf Sitzungsebene aktiviert sind, oder mit Einstellungen für die datenbankweit gültige Konfiguration in Konflikt. In diesem Fall hat der Hinweis auf Abfrageebene (USE HINT) immer Vorrang. Wenn ein USE HINT-Hinweis mit einem anderen Abfragehinweis oder einem auf Abfrageebene aktivierten Ablaufverfolgungsflag (z.B. QUERYTRACEON) in Konflikt steht, generiert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beim Versuch der Abfrageausführung einen Fehler. 

<a name="use-plan"></a> USE PLAN N' _<XML-Plan>_ '  
 Zwingt den Abfrageoptimierer, einen vorhandenen Abfrageplan für eine Abfrage zu verwenden, die mit **'** _xml\_plan_ **'** angegeben wird. USE PLAN kann nicht für die Anweisungen INSERT, UPDATE, MERGE oder DELETE angegeben werden.  
  
TABLE HINT **(** _<Name_des_verfuegbar\_gemachten\_Objekts>_ [ **,** \<table_hint> [ [ **,** ]..._n_ ] ] **)** Wendet den angegebenen Tabellenhinweis auf die Tabelle oder die Sicht an, die _Name_des_verfuegbar\_gemachten\_Objekts_ entspricht. Es wird empfohlen, einen Tabellenhinweis nur im Kontext einer [Planhinweisliste](../../relational-databases/performance/plan-guides.md) als Abfragehinweis zu verwenden.  
  
 _Name_des_verfuegbar\_gemachten\_Objekts_ kann einer der folgenden Verweise sein:  
  
-   Wenn für die Tabelle oder die Ansicht in der [FROM](../../t-sql/queries/from-transact-sql.md)-Klausel der Abfrage ein Alias verwendet wird, ist _exposed\_object\_name_ der Alias.  
  
-   Wenn kein Alias verwendet wird, entspricht _exposed\_object\_name_ genau der Tabelle oder der Ansicht, auf die in der FROM-Klausel verwiesen wird. Wenn z.B. mit einem zweiteiligen Namen auf die Tabelle oder die Ansicht verwiesen wird, ist _exposed\_object\_name_ der gleiche zweiteilige Name.  
  
 Wenn Sie _exposed\_object\_name_ angeben, ohne auch einen Tabellenhinweis anzugeben, werden alle Indizes, die Sie in der Abfrage als Teil eines Tabellenhinweises für das Objekt angeben, nicht berücksichtigt. Die Abfrageoptimierer bestimmt dann, wie der Index zu verwenden ist. Sie können diese Vorgehensweise verwenden, um die Auswirkung eines INDEX-Tabellenhinweises zu eliminieren, wenn Sie die ursprüngliche Abfrage nicht ändern können. Siehe Beispiel J.  
  
**\<table_hint> ::=** { NOEXPAND [ , INDEX ( _<Indexwert>_ [ ,...n ] ) | INDEX = ( _<Indexwert>_ ) ] \| INDEX ( _<Indexwert>_ [ ,...n ] ) | INDEX = ( _<Indexwert>_ ) \| FORCESEEK [ **(** _<Indexwert>_ **(** _<Name\_der\_Indexspalte>_ [ **,** ... ] **))** ] \| FORCESCAN \| HOLDLOCK \| NOLOCK \| NOWAIT \| PAGLOCK \| READCOMMITTED \| READCOMMITTEDLOCK \| READPAST \| READUNCOMMITTED \| REPEATABLEREAD \| ROWLOCK \| SERIALIZABLE \| SNAPSHOT \| SPATIAL_WINDOW_MAX_CELLS = _<ganzzahliger\_Wert>_ \| TABLOCK \| TABLOCKX \| UPDLOCK \| XLOCK }    
Dieser Tabellenhinweis ist auf die Tabelle oder die Sicht anzuwenden, die *Name_des_verfuegbar_gemachten_Objekts* als Abfragehinweis entspricht. Eine Beschreibung dieser Hinweise finden Sie unter [Tabellenhinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Andere Tabellenhinweise als INDEX, FORCESCAN und FORCESEEK sind als Abfragehinweise nicht zulässig, es sei denn, die Abfrage enthält bereits eine WITH-Klausel, die einen Tabellenhinweis angibt. Weitere Informationen finden Sie im Abschnitt [Bemerkungen](#remarks).  
  
> [!CAUTION]
> Die Angabe von FORCESEEK mit Parametern schränkt die Anzahl von Plänen, die vom Abfrageoptimierer berücksichtigt werden können, stärker ein als die Angabe von FORCESEEK ohne Parameter. Dies kann in mehreren Fällen zu dem Fehler führen, dass der Plan nicht generiert werden kann. In zukünftigen Releases können durch interne Änderungen des Abfrageoptimierers möglicherweise mehr Pläne berücksichtigt werden.  
  
## <a name="remarks"></a>Bemerkungen  
 Abfragehinweise können nur dann in einer INSERT-Anweisung angegeben werden, wenn eine SELECT-Klausel innerhalb der Anweisung verwendet wird.  
  
 Abfragehinweise können nur in der Abfrage der obersten Ebene angegeben werden, nicht in Unterabfragen. Wenn ein Tabellenhinweis als Abfragehinweis angegeben wird, kann der Hinweis in der Abfrage der obersten Ebene oder in einer Unterabfrage angegeben werden. Der Wert für _Name_des_verfuegbar\_gemachten\_Objekts_ in der TABLE HINT-Klausel angegebene Wert muss jedoch genau mit dem Namen des verfügbar gemachten Objekts in der Abfrage oder Unterabfrage übereinstimmen.  
  
## <a name="specifying-table-hints-as-query-hints"></a>Angeben von Tabellenhinweisen als Abfragehinweise  
 Es wird empfohlen, den INDEX-, FORCESCAN- oder FORCESEEK-Tabellenhinweis nur im Zusammenhang mit einer [Planhinweisliste](../../relational-databases/performance/plan-guides.md) als Abfragehinweis zu verwenden. Planhinweislisten sind nützlich, wenn Sie die ursprüngliche Abfrage nicht ändern können, beispielsweise bei Anwendungen von Drittanbietern. Der in der Planhinweisliste angegebene Abfragehinweis wird vor dem Kompilieren und Optimieren zur Abfrage hinzugefügt. Verwenden Sie für Ad-hoc-Abfragen die TABLE HINT-Klausel nur dann, wenn Sie Planhinweislisten-Anweisungen testen. Es wird empfohlen, für alle anderen Ad-hoc-Abfragen diese Hinweise nur als Tabellenhinweise anzugeben.  
  
 Wenn die INDEX-, FORCESCAN- und FORCESEEK-Tabellenhinweise als Abfragehinweise angegeben werden, sind sie für die folgenden Objekte gültig:  
  
-   Tabellen  
-   Sichten  
-   Indizierte Sichten  
-   Allgemeine Tabellenausdrücke (Der Hinweis muss in der SELECT-Anweisung angegeben sein, mit deren Resultset der allgemeine Tabellenausdruck aufgefüllt wird.)  
-   Dynamische Verwaltungssichten (DMVs)
-   Benannte Unterabfragen  
  
Sie können INDEX-, FORCESCAN- und FORCESEEK-Tabellenhinweise als Abfragehinweise für eine Abfrage angeben, die keine vorhandenen Tabellenhinweise enthält. Sie können sie auch verwenden, um bereits vorhandene INDEX-, FORCESCAN- oder FORCESEEK-Hinweise in der Abfrage zu ersetzen. 

Andere Tabellenhinweise als INDEX, FORCESCAN und FORCESEEK sind als Abfragehinweise nicht zulässig, es sei denn, die Abfrage enthält bereits eine WITH-Klausel, die einen Tabellenhinweis angibt. In diesem Fall muss auch ein übereinstimmender Hinweis als Abfragehinweis angegeben werden. Geben Sie den übereinstimmenden Hinweis als Abfragehinweis an, indem Sie TABLE HINT in der OPTION-Klausel verwenden. Diese Spezifikation behält die Semantik der Abfrage bei. Wenn die Abfrage beispielsweise den Tabellenhinweis „NOLOCK“ enthält, muss die Klausel „OPTION“ im Parameter **\@hints** der Planhinweisliste ebenfalls den Hinweis „NOLOCK“ enthalten. Siehe Beispiel K. 
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-merge-join"></a>A. Verwenden von MERGE JOIN  
 Das folgende Beispiel gibt an, dass MERGE JOIN den JOIN-Vorgang in der Abfrage ausführt. Im Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank verwendet.  
  
```sql  
SELECT *   
FROM Sales.Customer AS c  
INNER JOIN Sales.CustomerAddress AS ca ON c.CustomerID = ca.CustomerID  
WHERE TerritoryID = 5  
OPTION (MERGE JOIN);  
GO    
```  
  
### <a name="b-using-optimize-for"></a>B. Verwenden von OPTIMIZE FOR  
 Im folgenden Beispiel wird der Abfrageoptimierer angewiesen, für `@city_name` den Wert `'Seattle'` und für `@postal_code` die durchschnittliche Selektivität des Prädikats für alle Spaltenwerte zu verwenden, wenn die Abfrage optimiert wird. Im Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank verwendet.  
  
```sql  
CREATE PROCEDURE dbo.RetrievePersonAddress
@city_name NVARCHAR(30),  
 @postal_code NVARCHAR(15)
AS
SELECT * FROM Person.Address  
WHERE City = @city_name AND PostalCode = @postal_code  
OPTION ( OPTIMIZE FOR (@city_name = 'Seattle', @postal_code UNKNOWN) );  
GO
```  
  
### <a name="c-using-maxrecursion"></a>C. Verwenden von MAXRECURSION  
 MAXRECURSION kann verwendet werden, um zu verhindern, dass ein fehlerhaft formatierter allgemeiner Tabellenausdruck in eine Endlosschleife gerät. Im folgenden Beispiel wird absichtlich eine Endlosschleife erstellt. Zudem wird der MAXRECURSION-Hinweis verwendet, um die Anzahl der Rekursionsebenen auf zwei zu begrenzen. Im Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank verwendet.  
  
```sql  
--Creates an infinite loop  
WITH cte (CustomerID, PersonID, StoreID) AS  
(  
    SELECT CustomerID, PersonID, StoreID  
    FROM Sales.Customer  
    WHERE PersonID IS NOT NULL  
  UNION ALL  
    SELECT cte.CustomerID, cte.PersonID, cte.StoreID  
    FROM cte   
    JOIN  Sales.Customer AS e   
        ON cte.PersonID = e.CustomerID  
)  
--Uses MAXRECURSION to limit the recursive levels to 2  
SELECT CustomerID, PersonID, StoreID  
FROM cte  
OPTION (MAXRECURSION 2);  
GO  
```  
  
 Sobald der Fehler im Code behoben wurde, wird MAXRECURSION nicht mehr benötigt.  
  
### <a name="d-using-merge-union"></a>D: Verwenden von MERGE UNION  
 Im folgenden Beispiel wird der MERGE UNION-Abfragehinweis verwendet. Im Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank verwendet.  
  
```sql  
SELECT *  
FROM HumanResources.Employee AS e1  
UNION  
SELECT *  
FROM HumanResources.Employee AS e2  
OPTION (MERGE UNION);  
GO  
```  
  
### <a name="e-using-hash-group-and-fast"></a>E. Verwenden von HASH GROUP und FAST  
 Im folgenden Beispiel werden der MERGE UNION- und der FAST-Abfragehinweis verwendet. Im Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank verwendet.  
  
```sql  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (HASH GROUP, FAST 10);  
GO    
```  
  
### <a name="f-using-maxdop"></a>F. Verwenden von MAXDOP  
 Im folgenden Beispiel wird der MAXDOP-Abfragehinweis verwendet. Im Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank verwendet.  
  
```sql  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (MAXDOP 2);    
GO
```  
  
### <a name="g-using-index"></a>G. Verwenden von INDEX  
 In den folgenden Beispielen wird der INDEX-Hinweis verwendet. Im ersten Beispiel wird ein einzelner Index angegeben. Im zweiten Beispiel werden mehrere Indizes für einen einzelnen Tabellenverweis angegeben. Da der INDEX-Hinweis auf eine Tabelle angewendet wird, die einen Alias verwendet, muss in beiden Beispielen in der TABLE HINT-Klausel auch der gleiche Alias wie der verfügbare Objektname angegeben werden. Im Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank verwendet.  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide1',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e, INDEX (IX_Employee_ManagerID)))';  
GO  
EXEC sp_create_plan_guide   
    @name = N'Guide2',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e, INDEX(PK_Employee_EmployeeID, IX_Employee_ManagerID)))';  
GO    
```  
  
### <a name="h-using-forceseek"></a>H. Verwenden von FORCESEEK  
 Im folgenden Beispiel wird der FORCESEEK-Tabellenhinweis verwendet. Die TABLE HINT-Klausel muss auch den gleichen zweiteiligen Namen wie der Name des exponierten Objekts angeben. Geben Sie den Namen an, wenn Sie den INDEX-Hinweis auf eine Tabelle anwenden, die einen zweiteiligen Namen verwendet. Im Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank verwendet.  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide3',   
    @stmt = N'SELECT c.LastName, c.FirstName, HumanResources.Employee.Title  
              FROM HumanResources.Employee  
              JOIN Person.Contact AS c ON HumanResources.Employee.ContactID = c.ContactID  
              WHERE HumanResources.Employee.ManagerID = 3  
              ORDER BY c.LastName, c.FirstName;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT( HumanResources.Employee, FORCESEEK))';  
GO    
```  
  
### <a name="i-using-multiple-table-hints"></a>I. Verwenden von mehreren Tabellenhinweisen  
 Im folgenden Beispiel wird der INDEX-Hinweis auf eine Tabelle angewendet, und der FORCESEEK-Hinweis wird auf eine andere Tabelle angewendet. Im Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank verwendet.  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide4',   
    @stmt = N'SELECT e.ManagerID, c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, INDEX( IX_Employee_ManagerID))   
                       , TABLE HINT (c, FORCESEEK))';  
GO  
```  
  
### <a name="j-using-table-hint-to-override-an-existing-table-hint"></a>J. Verwenden von TABLE HINT zum Überschreiben eines vorhandenen Tabellenhinweises  
Im folgenden Beispiel wird die Verwendung des TABLE HINT-Hinweises gezeigt. Sie können den Hinweis ohne Angabe eines Hinweises verwenden, um das Verhalten des INDEX-Tabellenhinweises zu überschreiben, den Sie in der FROM-Klausel der Abfrage angeben. Im Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank verwendet.  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide5',   
    @stmt = N'SELECT e.ManagerID, c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e WITH (INDEX (IX_Employee_ManagerID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e))';  
GO    
```  
  
### <a name="k-specifying-semantics-affecting-table-hints"></a>K. Angeben von Tabellenhinweisen, die die Semantik beeinflussen  
Das folgende Beispiel enthält in der Abfrage zwei Tabellenhinweise: den NOLOCK-Hinweis, der die Semantik beeinflusst, und den INDEX-Hinweis, der die Semantik nicht beeinflusst. Der NOLOCK-Hinweis wird in der OPTIONS-Klausel der Planhinweisliste angegeben, um die Semantik der Abfrage beizubehalten. Geben Sie neben dem NOLOCK-Hinweis die INDEX- und FORCESEEK-Hinweise an, und ersetzen Sie den nicht INDEX-Hinweis,der die Semantik nicht beeinflusst, in der Abfrage bei der Kompilierung und Optimierung von Anweisungen. Im Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank verwendet.  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide6',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
                   WITH (NOLOCK, INDEX (PK_Employee_EmployeeID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',  
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, INDEX(IX_Employee_ManagerID), NOLOCK, FORCESEEK))';  
GO    
```  
  
Das folgende Beispiel zeigt eine alternative Methode, um die Semantik der Abfrage beizubehalten und zuzulassen, dass der Abfrageoptimierer einen anderen als den im Tabellenhinweis angegebenen Index verwendet. Lassen Sie den Abfrageoptimierer wählen, indem Sie den NOLOCK-Hinweis in der OPTIONS-Klausel angeben. Sie geben den Hinweis an, da er die Semantik beeinflusst. Geben Sie dann das TABLE HINT-Schlüsselwort mit nur einem Tabellenverweis und ohne INDEX-Hinweis an. Im Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank verwendet.  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide7',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
                   WITH (NOLOCK, INDEX (PK_Employee_EmployeeID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',  
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, NOLOCK))';  
GO  
```  

### <a name="l-using-use-hint"></a>L. Verwenden von USE HINT  
 Im folgenden Beispiel werden der RECOMPILE- und der USE HINT-Abfragehinweis verwendet. Im Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank verwendet.  
  
```sql  
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (RECOMPILE, USE HINT ('ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES', 'DISABLE_PARAMETER_SNIFFING')); 
GO  
```  

### <a name="m-using-querytraceon-hint"></a>M. Verwenden von QUERYTRACEON HINT  
 Im folgenden Beispiel werden die QUERYTRACEON-Abfragehinweise verwendet. Im Beispiel wird die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank verwendet. Sie können mithilfe der folgenden Abfrage alle den Plan beeinflussenden Hotfixes aktivieren, die vom Ablaufverfolgungsflag 4199 für eine bestimmte Abfrage gesteuert werden:
  
```sql  
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (QUERYTRACEON 4199);
```  

 Wie in der folgenden Abfrage können Sie auch mehrere Ablaufverfolgungsflags verwenden:

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION  (QUERYTRACEON 4199, QUERYTRACEON 4137);
```

## <a name="see-also"></a>Weitere Informationen  
[Hints &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)   
[sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
[sp_control_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)  
[Ablaufverfolgungsflags](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)       
[Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)      
