---
description: MERGE (Transact-SQL)
title: MERGE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/20/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- MERGE
- MERGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- updating data [SQL Server]
- modifying data [SQL Server], MERGE statement
- MERGE statement [SQL Server]
- adding data
- DML [SQL Server], MERGE statement
- table modifications [SQL Server], MERGE statement
- data manipulation language [SQL Server], MERGE statement
- inserting data
ms.assetid: c17996d6-56a6-482f-80d8-086a3423eecc
author: XiaoyuMSFT
ms.author: XiaoyuL
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017 ||  azure-sqldw-latest
ms.openlocfilehash: 6bb1014c22353826b6e4429726d4d28549cc274a
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489334"
---
# <a name="merge-transact-sql"></a>MERGE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb-asa.md)]

Führt Einfüge-, Aktualisierungs- oder Löschvorgänge in einer Zieltabelle anhand der Ergebnisse eines Joins mit einer Quelltabelle aus. Synchronisieren Sie z.B. zwei Tabellen, indem Sie Zeilen in einer Tabelle anhand von Unterschieden, die in der anderen Tabelle gefunden wurden, einfügen, aktualisieren oder löschen. 

> [!NOTE]
> MERGE befindet sich derzeit in der Vorschau für Azure Synapse Analytics.
  
**Leistungstipp:** Das für die MERGE-Anweisung beschriebene bedingte Verhalten funktioniert am besten, wenn die beiden Tabellen eine komplexe Mischung aus übereinstimmenden Eigenschaften aufweisen. Beispielsweise das Einfügen einer Zeile, wenn sie nicht vorhanden ist, oder das Aktualisieren der Zeile, wenn sie übereinstimmt. Wenn Sie eine Tabelle einfach nur basierend auf den Zeilen einer anderen Tabelle aktualisieren, verbessern Sie mit den grundlegenden INSERT-, UPDATE- und DELETE-Anweisungen Leistung und Skalierbarkeit. Beispiel:  
  
```sql
INSERT tbl_A (col, col2)  
SELECT col, col2
FROM tbl_B
WHERE NOT EXISTS (SELECT col FROM tbl_A A2 WHERE A2.col = tbl_B.col);  
```  
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql

-- SQL Server and Azure SQL Database
[ WITH <common_table_expression> [,...n] ]  
MERGE
    [ TOP ( expression ) [ PERCENT ] ]
    [ INTO ] <target_table> [ WITH ( <merge_hint> ) ] [ [ AS ] table_alias ]  
    USING <table_source> [ [ AS ] table_alias ]
    ON <merge_search_condition>  
    [ WHEN MATCHED [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]  
    [ WHEN NOT MATCHED [ BY TARGET ] [ AND <clause_search_condition> ]  
        THEN <merge_not_matched> ]  
    [ WHEN NOT MATCHED BY SOURCE [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]  
    [ <output_clause> ]  
    [ OPTION ( <query_hint> [ ,...n ] ) ]
;  
  
<target_table> ::=  
{
    [ database_name . schema_name . | schema_name . ]  
  target_table  
}  
  
<merge_hint>::=  
{  
    { [ <table_hint_limited> [ ,...n ] ]  
    [ [ , ] INDEX ( index_val [ ,...n ] ) ] }  
}  

<merge_search_condition> ::=  
    <search_condition>  
  
<merge_matched>::=  
    { UPDATE SET <set_clause> | DELETE }  
  
<merge_not_matched>::=  
{  
    INSERT [ ( column_list ) ]
        { VALUES ( values_list )  
        | DEFAULT VALUES }  
}  
  
<clause_search_condition> ::=  
    <search_condition> 
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

```syntaxsql
-- MERGE (Preview) for Azure Synapse Analytics 
[ WITH <common_table_expression> [,...n] ]  
MERGE
    [ INTO ] <target_table> [ [ AS ] table_alias ]  
    USING <table_source> [ [ AS ] table_alias ]
    ON <merge_search_condition>  
    [ WHEN MATCHED [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]  
    [ WHEN NOT MATCHED [ BY TARGET ] [ AND <clause_search_condition> ]  
        THEN <merge_not_matched> ]  
    [ WHEN NOT MATCHED BY SOURCE [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]
    [ OPTION ( <query_hint> [ ,...n ] ) ]
;  -- The semi-colon is required, or the query will return a syntax error. 
```

## <a name="arguments"></a>Argumente

WITH \<common_table_expression>  
Gibt den temporären Resultset- oder Sichtnamen an, der auch als allgemeiner Tabellenausdruck bezeichnet wird und innerhalb der MERGE-Anweisung definiert ist. Das Resultset wird aus einer einfachen Abfrage abgeleitet. Die MERGE-Anweisung verweist auf dieses Resultset. Weitere Informationen finden Sie unter [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
TOP ( *expression* ) [ PERCENT ]  
Gibt die Anzahl oder den Prozentsatz der betroffenen Zeilen an. *expression* kann eine Anzahl oder ein Prozentsatz der Zeilen sein. Die Zeilen, auf die im TOP-Ausdruck verwiesen wird, sind nicht auf bestimmte Weise angeordnet. Weitere Informationen finden Sie unter [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
Die TOP-Klausel wird angewendet, nachdem die gesamte Quelltabelle und die gesamte Zieltabelle verknüpft und die nicht für eine der Aktionen INSERT, UPDATE oder DELETE in Frage kommenden verknüpften Zeilen gelöscht wurden. Die TOP-Klausel reduziert die Anzahl der verknüpften Zeilen mit dem angegebenen Wert noch weiter. Die INSERT-, UPDATE- oder DELETE-Aktionen gelten in ungeordneter Weise für die verbliebenen verknüpften Zeilen. Dies bedeutet, dass für die Verteilung der Zeilen auf die in den WHEN-Klauseln definierten Aktionen keine bestimmte Reihenfolge gilt. Angeben von TOP (10) betrifft z.B. 10 Zeilen. Von diesen Zeilen können 7 aktualisiert und 3 eingefügt werden, oder 1 Zeile kann gelöscht, 5 können aktualisiert und 4 eingefügt werden usw.  
  
Da die MERGE-Anweisung einen vollständigen Tabellenscan der Quell- und der Zieltabelle ausführt, kann die E/A-Leistung beeinträchtigt werden, wenn mit der TOP-Klausel eine große Tabelle durch Erstellen mehrerer Batches geändert wird. In diesem Szenario muss unbedingt sichergestellt werden, dass alle aufeinanderfolgenden Batches auf neue Zeilen ausgerichtet sind.  
  
*database_name*  
Der Name der Datenbank, in der sich *target_table* befindet.  
  
*schema_name*  
Der Namen des Schemas, zu dem die Tabelle *target_table* gehört.  
  
*target_table*  
Hiermit wird die Tabelle oder die Ansicht angegeben, mit der die Datenzeilen aus \<table_source> basierend auf \<clause_search_condition> abgeglichen werden. *target_table* ist das Ziel aller Einfüge-, Update- oder Löschvorgänge, die durch die WHEN-Klauseln der MERGE-Anweisung angegeben werden.  
  
Wenn *target_table* eine Sicht ist, müssen alle Aktionen für die Tabelle die Bedingungen zum Aktualisieren von Sichten erfüllen. Weitere Informationen finden Sie unter [Modify Data Through a View](../../relational-databases/views/modify-data-through-a-view.md) (Ändern von Daten über eine Sicht).  
  
*target_table* darf keine Remotetabelle sein. Für *target_table* dürfen keine Regeln definiert sein.  
  
[ AS ] *table_alias*  
Dies ist ein alternativer Name zum Verweisen auf eine Tabelle für *target_table*.  
  
USING \<table_source>  
Hiermit wird die Datenquelle angegeben, die basierend auf \<merge_search condition> mit den Datenzeilen in *target_table* abgeglichen wird. Das Ergebnis dieser Zuordnung legt die Aktionen fest, die von den WHEN-Klauseln der MERGE-Anweisung ausgeführt werden. \<table_source> kann eine Remotetabelle oder eine abgeleitete Tabelle sein, die auf Remotetabellen zugreift.
  
\<table_source> kann eine abgeleitete Tabelle sein, die mit dem [Tabellenwertkonstruktor](../../t-sql/queries/table-value-constructor-transact-sql.md) von [!INCLUDE[tsql](../../includes/tsql-md.md)] eine Tabelle durch Angeben mehrerer Zeilen erstellt.  
  
 [ AS ] *table_alias*  
Dies ist ein alternativer Name zum Verweisen auf eine Tabelle für table_source.   
  
Weitere Informationen zur Syntax und zu den Argumenten dieser Klausel finden Sie unter [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md).  
  
ON \<merge_search_condition>  
Hiermit werden die Bedingungen angegeben, unter denen \<table_source> mit *target_table* verknüpft wird, um Übereinstimmungen zu ermitteln.
  
> [!CAUTION]  
> Es ist wichtig, dass nur die Spalten aus der Zieltabelle angegeben werden, die für Abgleichszwecke verwendet werden. Geben Sie also Spalten aus der Zieltabelle an, die mit der entsprechenden Spalte der Quelltabelle abgeglichen werden. Versuchen Sie nicht, die Abfrageleistung zu optimieren, indem Sie Zeilen in der Zieltabelle in der ON-Klausel herausfiltern, beispielsweise durch Angabe von `AND NOT target_table.column_x = value`. Dadurch kann es zu unerwarteten und falschen Ergebnissen kommen.  
  
WHEN MATCHED THEN \<merge_matched>  
Hiermit wird angegeben, dass alle Zeilen von *target_table, die mit den von \<table_source> ON \<merge_search_condition> zurückgegebenen Zeilen übereinstimmen und alle zusätzlichen Suchbedingungen erfüllen, gemäß der \<merge_matched>-Klausel aktualisiert oder gelöscht werden.  
  
Die MERGE-Anweisung kann höchstens über zwei WHEN MATCHED-Klauseln verfügen. Wenn zwei Klauseln angegeben werden, muss die erste Klausel von einer AND \<search_condition>-Klausel begleitet werden. Für jede gegebene Zeile wird die zweite WHEN MATCHED-Klausel nur angewendet, wenn die erste nicht angewendet wurde. Wenn zwei WHEN MATCHED-Klauseln vorhanden sind, muss die eine eine UPDATE-Aktion und die andere eine DELETE-Aktion angeben. Wenn UPDATE in der \<merge_matched>-Klausel angegeben wird und mehr als eine Zeile aus \<table_source> basierend auf \<merge_search_condition> mit einer Zeile in *target_table* übereinstimmt, gibt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen Fehler zurück. Die MERGE-Anweisung kann dieselbe Zeile nicht mehrmals aktualisieren oder dieselbe Zeile aktualisieren und löschen.  
  
WHEN NOT MATCHED [ BY TARGET ] THEN \<merge_not_matched>  
Hiermit wird angegeben, dass für jede Zeile, die von \<table_source> ON \<merge_search_condition> zurückgegeben wird und nicht mit einer Zeile in *target_table* übereinstimmt, aber eine zusätzliche Suchbedingung erfüllt (falls vorhanden), eine Zeile in *target_table* eingefügt wird. Die einzufügenden Werte werden durch die \<merge_not_matched>-Klausel angegeben. Die MERGE-Anweisung kann nur über eine WHEN NOT MATCHED [ BY TARGET ]-Klausel verfügen.

WHEN NOT MATCHED BY SOURCE THEN \<merge_matched>  
Hiermit wird angegeben, dass alle Zeilen von *target_table, die nicht mit den von \<table_source> ON \<merge_search_condition> zurückgegebenen Zeilen übereinstimmen und alle zusätzlichen Suchbedingungen erfüllen, gemäß der \<merge_matched>-Klausel aktualisiert oder gelöscht werden.  
  
Die MERGE-Anweisung kann höchstens über zwei WHEN NOT MATCHED BY SOURCE-Klauseln verfügen. Wenn zwei Klauseln angegeben werden, muss die erste Klausel von einer AND \<clause_search_condition>-Klausel begleitet werden. Für jede gegebene Zeile wird die zweite WHEN NOT MATCHED BY SOURCE-Klausel nur angewendet, wenn die erste nicht angewendet wurde. Wenn zwei WHEN NOT MATCHED BY SOURCE-Klauseln vorhanden sind, muss die eine eine UPDATE-Aktion und die andere eine DELETE-Aktion angeben. In \<clause_search_condition> kann nur auf Spalten aus der Zieltabelle verwiesen werden.  
  
Wenn von \<table_source> keine Zeilen zurückgegeben werden, kann auf Spalten in der Quelltabelle nicht zugegriffen werden. Wenn die in der \<merge_matched>-Klausel angegebene Update- oder Löschaktion auf Spalten in der Quelltabelle verweist, wird der Fehler 207 (Ungültiger Spaltenname) zurückgegeben. Die Klausel `WHEN NOT MATCHED BY SOURCE THEN UPDATE SET TargetTable.Col1 = SourceTable.Col1` kann beispielsweise dazu führen, dass die Anweisung fehlschlägt, da der Zugriff auf `Col1` in der Quelltabelle nicht möglich ist.  
  
AND \<clause_search_condition>  
Gibt jede gültige Suchbedingung an. Weitere Informationen finden Sie unter [Suchbedingung &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
\<table_hint_limited>  
Gibt mindestens einen Tabellenhinweis an, der für jeden durch die MERGE-Anweisung ausgeführten Einfüge-, Update- oder Löschvorgang auf die Zieltabelle angewendet wird. Das WITH-Schlüsselwort und die Klammern sind erforderlich.  
  
NOLOCK und READUNCOMMITTED sind nicht zulässig. Weitere Informationen zu Tabellenhinweisen finden Sie unter [Tabellenhinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
Das Angeben eines TABLOCK-Hinweises für eine Tabelle, die das Ziel einer INSERT-Anweisung ist, hat dieselbe Wirkung wie das Angeben eines TABLOCKX-Hinweises. Auf die Tabelle wird eine exklusive Sperre angewendet. Wenn FORCESEEK angegeben wird, wird der Hinweis auf die implizite Instanz der Zieltabelle angewendet, die mit der Quelltabelle verknüpft ist.  
  
> [!CAUTION]  
> Wenn READPAST mit WHEN NOT MATCHED [ BY TARGET ] THEN INSERT angegeben wird, kann dies zu INSERT-Operationen führen, die gegen UNIQUE-Beschränkungen verstoßen.  
  
INDEX ( index_val [ ,...n ] )  
Gibt den Namen oder die ID eines oder mehrerer Indizes in der Zieltabelle zum Ausführen eines impliziten Joins mit der Quelltabelle an. Weitere Informationen finden Sie unter [Tabellenhinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
\<output_clause>  
Gibt ohne bestimmte Reihenfolge eine Zeile für jede Zeile in *target_table* zurück, die aktualisiert, eingefügt oder gelöscht wird. **$action** kann in der OUTPUT-Klausel angegeben werden. **$action** ist eine Spalte vom Typ **nvarchar(10)** , die für jede Zeile einen von drei Werten zurückgibt: „INSERT“, „UPDATE“ oder „DELETE“, je nach der für diese Zeile ausgeführten Aktion. Weitere Informationen zu den Argumenten und dem Verhalten dieser Klausel finden Sie unter [OUTPUT-Klausel (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md).  
  
OPTION ( \<query_hint> [ ,...n ] )  
Gibt an, dass zum Anpassen der Art und Weise, wie die Anweisung durch die Datenbank-Engine verarbeitet wird, Hinweise des Abfrageoptimierers verwendet werden. Weitere Informationen finden Sie unter [Abfragehinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
\<merge_matched>  
Hiermit wird die Update- oder Löschaktion angegeben, die auf alle Zeilen von *target_table* angewendet wird, die nicht mit den von \<table_source> ON \<merge_search_condition> zurückgegebenen Zeilen übereinstimmen, und die alle zusätzlichen Suchbedingungen erfüllen.  
  
UPDATE SET \<set_clause>  
Gibt die Liste der Spalten- oder Variablennamen an, die in der Zieltabelle aktualisiert werden sollen, sowie die Werte, mit denen das Update vorgenommen werden soll.  
  
Weitere Informationen zu den Argumenten dieser Klausel finden Sie unter [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md). Eine Variable auf denselben Wert festzulegen wie eine Spalte wird nicht unterstützt.  
  
Delete  
Gibt an, dass die Zeilen, die mit Zeilen in *target_table* übereinstimmen, gelöscht werden.  
  
\<merge_not_matched>  
Gibt die Werte an, die in die Zieltabelle eingefügt werden sollen.  
  
(*column_list*)  
Eine Liste mit einer oder mehreren Spalten der Zieltabelle, in die Daten eingefügt werden sollen. Spalten müssen als einteiliger Name angegeben werden. Andernfalls schlägt die MERGE-Anweisung fehl. *column_list* muss in Klammern eingeschlossen und durch ein Trennzeichen getrennt werden.  
  
VALUES ( *values_list*)  
Eine durch Trennzeichen getrennte Liste mit Konstanten, Variablen oder Ausdrücken, die Werte zum Einfügen in die Zieltabelle zurückgeben. Ausdrücke dürfen keine EXECUTE-Anweisung enthalten.  
  
DEFAULT VALUES  
Erzwingt, dass die eingefügte Zeile den für jede Spalte definierten Standardwert enthält.  
  
Weitere Informationen zu dieser Klausel finden Sie unter [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md).  
  
\<search_condition>  
Hiermit werden die Suchbedingungen zum Angeben von \<merge_search_condition> oder \<clause_search_condition> angegeben. Weitere Informationen zu den Argumenten für diese Klausel finden Sie unter [Suchbedingung &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  

\<graph search pattern>  
Gibt das Graph-Vergleichsmuster an. Weitere Informationen zu den Argumenten für diese Klausel finden Sie unter [MATCH &#40;Transact-SQL&#41;](../../t-sql/queries/match-sql-graph.md).
  
## <a name="remarks"></a>Bemerkungen
>[!NOTE]
> In Azure Synapse Analytics weist der MERGE-Befehl (Vorschau) die folgenden Unterschiede im Vergleich mit SQL-Server und Azure SQL-Datenbank auf.  
> - Ein MERGE-Update ist als Paar aus Delete und Insert implementiert. Die betroffene Zeilenanzahl für ein MERGE-Update schließt die gelöschten und eingefügten Zeilen ein. 

> - Während der Vorschau wird MERGE…WHEN NOT MATCHED INSERT nicht für Tabellen mit IDENTITY-Spalten unterstützt.  

> - Die Unterstützung für Tabellen mit verschiedenen Verteilungstypen ist in dieser Tabelle beschrieben:

>|MERGE-KLAUSEL in Azure Synapse Analytics|Unterstützte ZIEL-Verteilungstabelle| Unterstützte QUELL-Verteilungstabelle|Comment|  
>|-----------------|---------------|-----------------|-----------|  
>|**WHEN MATCHED**| Alle Verteilungstypen |Alle Verteilungstypen||  
>|**NOT MATCHED BY TARGET**|HASH |Alle Verteilungstypen|Verwenden Sie UPDATE/DELETE FROM…JOIN, um zwei Tabellen zu synchronisieren. |
>|**NOT MATCHED BY SOURCE**|Alle Verteilungstypen|Alle Verteilungstypen|||  

Mindestens eine der drei MATCHED-Klauseln muss angegeben werden, dies kann jedoch in beliebiger Reihenfolge erfolgen. Eine Variable in derselben MATCHED-Klausel kann nicht mehr als einmal aktualisiert werden.  
  
Jede Einfüge-, Update- oder Löschaktion, die in der Zieltabelle durch die MERGE-Anweisung angegeben wird, ist durch alle für die Tabelle definierten Beschränkungen eingeschränkt, einschließlich aller kaskadierenden referenziellen Integritätsbeschränkungen. Wenn IGNORE_DUP_KEY für alle eindeutigen Indizes in der Zieltabelle auf ON festgelegt ist, ignoriert MERGE diese Einstellung.  
  
Die MERGE-Anweisung erfordert ein Semikolon (;) als Abschlusszeichen für die Anweisung. Wenn eine MERGE-Anweisung ohne das Abschlusszeichen ausgeführt wird, wird der Fehler 10713 generiert.  
  
Bei Verwendung nach MERGE gibt [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md) die Gesamtanzahl der eingefügten, aktualisierten und gelöschten Zeilen an den Client zurück.  
  
MERGE ist ein vollständig reserviertes Schlüsselwort, wenn der Kompatibilitätsgrad der Datenbank auf 100 oder höher festgelegt ist. Die MERGE-Anweisung ist bei einem Kompatibilitätsgrad von sowohl 90 als auch 100 verfügbar. Bei einem Kompatibilitätsgrad von 90 ist das Schlüsselwort allerdings nicht vollständig reserviert.  
  
Verwenden Sie die **MERGE**-Anweisung nicht zusammen mit dem Replikationstyp „Verzögertes Update über eine Warteschlange“. **MERGE** und der Trigger für verzögerte Updates über eine Warteschlange sind nicht kompatibel. Ersetzen Sie die **MERGE**-Anweisung durch eine INSERT- oder UPDATE-Anweisung.  

## <a name="trigger-implementation"></a>Triggerimplementierung

Für jeden Einfüge-, Update- oder Löschvorgang, der in der MERGE-Anweisung angegeben ist, löst [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alle entsprechenden AFTER-Trigger aus, die in der Zieltabelle definiert sind, gewährleistet jedoch nicht, für welche Aktion Trigger zuerst oder zuletzt ausgelöst werden. Trigger, die für dieselbe Aktion definiert sind, halten sich an die von Ihnen angegebene Reihenfolge. Weitere Informationen zum Festlegen der Reihenfolge beim Auslösen von Triggern finden Sie unter [Angeben des ersten und des letzten Triggers](../../relational-databases/triggers/specify-first-and-last-triggers.md).  
  
Wenn in der Zieltabelle ein aktivierter INSTEAD OF-Trigger für einen Einfüge-, Update- oder Löschvorgang definiert ist, der durch eine MERGE-Anweisung ausgeführt wird, muss sie einen aktivierten INSTEAD OF-Trigger für alle in der MERGE-Anweisung angegebenen Aktionen enthalten.  
  
Wenn für *target_table* ein INSTEAD OF UPDATE-Trigger oder INSTEAD OF DELETE-Trigger definiert ist, werden die Update- oder Löschvorgänge nicht ausgeführt. Stattdessen werden die Trigger ausgelöst, und die **inserted**- und **deleted**-Tabelle werden entsprechend aufgefüllt.  
  
Wenn für *target_table* der INSTEAD OF INSERT-Trigger definiert ist, wird der Einfügevorgang nicht ausgeführt. Stattdessen wird die Tabelle entsprechend aufgefüllt.  
  
## <a name="permissions"></a>Berechtigungen

Erfordert die SELECT-Berechtigung für die Quelltabelle und die INSERT-, UPDATE- oder DELETE-Berechtigung für die Zieltabelle. Weitere Informationen finden Sie im Abschnitt „Berechtigungen“ in den Artikeln zu [SELECT](../../t-sql/queries/select-transact-sql.md), [INSERT](../../t-sql/statements/insert-transact-sql.md), [UPDATE](../../t-sql/queries/update-transact-sql.md) und [DELETE](../../t-sql/statements/delete-transact-sql.md).  
  
## <a name="optimizing-merge-statement-performance"></a>Optimieren der Leistung bei MERGE-Anweisungen

Mit der MERGE-Anweisung können Sie die einzelnen DML-Anweisungen durch eine einzelne Anweisung ersetzen. Auf diese Weise können Sie die Abfrageleistung verbessern, da die Vorgänge innerhalb einer einzelnen Anweisung ausgeführt und so die Anzahl der Verarbeitungsvorgänge für die Daten in der Quell- und Zieltabelle minimiert werden. Leistungssteigerungen sind jedoch von richtigen Indizes, Joins und anderen Fragen abhängig.

### <a name="index-best-practices"></a>Bewährte Methoden zum Indizieren

Zur Leistungsverbesserung der MERGE-Anweisung werden die folgenden Indexrichtlinien empfohlen:

- Erstellen Sie einen eindeutigen und umfassenden Index für die Joinspalten der Quelltabelle.
- Erstellen Sie für die Joinspalten in der Zieltabelle einen eindeutigen gruppierten Index.

Mit diesen Indizes wird sichergestellt, dass die Joinschlüssel eindeutig und die Daten in den Tabellen sortiert sind. Die Abfrageleistung wird verbessert, da der Abfrageoptimierer keine zusätzliche Validierung ausführen muss, um doppelte Zeilen zu suchen und zu aktualisieren, und zusätzliche Sortiervorgänge nicht erforderlich sind.

### <a name="join-best-practices"></a>Bewährte Methoden für JOIN

Zur Leistungsverbesserung der MERGE-Anweisung und Sicherstellung richtiger Ergebnisse werden die folgenden Joinrichtlinien empfohlen:

- Geben Sie in der ON <merge_search_condition>-Klausel nur Suchbedingungen an, die die Kriterien für den Vergleich von Daten in den Quell- und Zieltabellen bestimmen. Geben Sie also nur Spalten aus der Zieltabelle an, die mit den entsprechenden Spalten der Quelltabelle verglichen werden. 
- Fügen Sie keine Vergleiche mit anderen Werten, z. B. einer Konstante, ein.

Zum Filtern von Zeilen aus der Quell- oder Zieltabelle verwenden Sie eine der folgenden Methoden:

- Geben Sie die Suchbedingung für die Zeilenfilterung in der entsprechenden WHEN-Klausel an. Beispiel: WHEN NOT MATCHED AND S.EmployeeName LIKE 'S%' THEN INSERT....
- Definieren Sie in der Quelle oder im Ziel eine Sicht, die die gefilterten Zeilen zurückgibt, und verweisen Sie auf die Sicht als Quell- oder Zieltabelle. Wenn die Sicht für die Zieltabelle definiert ist, müssen alle Aktionen für die Tabelle die Bedingungen zum Aktualisieren von Sichten erfüllen. Weitere Informationen zum Aktualisieren von Daten mithilfe von Sichten finden Sie unter „Ändern von Daten über eine Sicht“.
- Mit der `WITH <common table expression>`-Klausel können Sie Zeilen aus den Quell- oder Zieltabellen filtern. Diese Methode ähnelt dem Angeben zusätzlicher Suchkriterien in der ON-Klausel und kann zu falschen Ergebnissen führen. Es wird empfohlen, die Verwendung dieser Methode zu vermeiden oder diese vor der Implementierung gründlich zu testen.

Der Joinvorgang in der MERGE-Anweisung wird auf dieselbe Weise optimiert wie ein Join in einer SELECT-Anweisung. Das heißt, beim Verarbeiten von Joins durch SQL Server wählt der Abfrageoptimierer (aus verschiedenen Möglichkeiten) die effizienteste Methode aus. Wenn Quelle und Ziel von ähnlicher Größe sind und die zuvor beschriebenen Indizierungsrichtlinien auf die Quell- und Zieltabellen angewendet werden, bildet ein Merge Join-Operator den effizientesten Abfrageplan. Das liegt daran, dass beide Tabellen einmal durchsucht werden und anschließend keine Notwendigkeit vorliegt, die Daten zu sortieren. Wenn die Quelltabelle kleiner als die Zieltabelle ist, ist ein Nested Loops-Operator vorzuziehen.

Sie können die Verwendung einer bestimmten Join erzwingen, indem Sie in der MERGE-Anweisung die `OPTION (<query_hint>)`-Klausel angeben. Es wird empfohlen, als Abfragehinweis für MERGE-Anweisungen nicht den Hashjoin zu verwenden, da von diesem Jointyp keine Indizes verwendet werden.

### <a name="parameterization-best-practices"></a>Bewährte Methoden für die Parametrisierung

Wenn eine der SELECT-, INSERT-, UPDATE- oder DELETE-Anweisungen ohne Parameter ausgeführt wird, kann der SQL Server-Abfrageoptimierer die Anweisung intern parametrisieren. Dies bedeutet, dass alle eventuell in der Abfrage enthaltenen Literalwerte durch Parameter ersetzt werden. Beispielsweise kann die Anweisung „INSERT dbo.MyTable (Col1, Col2) VALUES (1, 10)“ intern als „INSERT dbo.MyTable (Col1, Col2) VALUES (@p1, @p2)“ implementiert werden. Dieser als einfache Parametrisierung bezeichnete Vorgang erhöht die Wahrscheinlichkeit, dass die relationale Engine neue SQL-Anweisungen vorhandenen, zuvor kompilierten Ausführungsplänen zuordnet. Möglicherweise wird die Abfrageleistung verbessert, da die Häufigkeit der Abfragekompilierungen und Neukompilierungen reduziert wird. Die einfache Parametrisierung wird vom Abfrageoptimierer nicht auf MERGE-Anweisungen angewendet. Daher ist die Leistung bei der Ausführung von MERGE-Anweisungen mit Literalwerten nicht so hoch wie bei einzelnen INSERT-, UPDATE- oder DELETE-Anweisungen, da bei jeder Ausführung der MERGE-Anweisung ein neuer Plan kompiliert wird.

Um die Abfrageleistung zu verbessern, werden die folgenden Parametrisierungsrichtlinien empfohlen:

- Parametrisieren Sie alle Literalwerte in der `ON <merge_search_condition>`-Klausel sowie in den `WHEN`-Klauseln der MERGE-Anweisung. Beispielsweise können Sie die MERGE-Anweisung in eine gespeicherte Prozedur integrieren und dabei die Literalwerte durch die entsprechenden Eingabeparameter ersetzen.
- Wenn Sie die Anweisung nicht parametrisieren können, erstellen Sie eine Planhinweisliste vom Typ `TEMPLATE`, und geben Sie in der Planhinweisliste den Abfragehinweis `PARAMETERIZATION FORCED` an.
- Wenn MERGE-Anweisungen für die Datenbank häufig ausgeführt werden, empfiehlt es sich möglicherweise, die PARAMETERIZATION-Option für die Datenbank auf FORCED festzulegen. Legen Sie diese Option mit Bedacht fest. Die `PARAMETERIZATION`-Option ist eine Einstellung auf Datenbankebene und wirkt sich auf die Verarbeitung aller Abfragen für die Datenbank aus.

### <a name="top-clause-best-practices"></a>Bewährte Methoden für die TOP-Klausel

In der MERGE-Anweisung gibt die TOP-Klausel die Anzahl oder den Prozentsatz der Zeilen an, auf die sich das Verknüpfen der Quelltabelle mit der Zieltabelle auswirkt, nachdem Zeilen, auf die keine INSERT-, UPDATE- oder DELETE-Aktion angewendet wird, entfernt wurden. Die TOP-Klausel verringert zudem die Anzahl der verknüpften Zeilen auf den angegebenen Wert, und die INSERT-, UPDATE- oder DELETE-Aktionen werden ungeordnet auf die verbliebenen verknüpften Zeilen angewendet. Dies bedeutet, dass für die Verteilung der Zeilen auf die in den WHEN-Klauseln definierten Aktionen keine bestimmte Reihenfolge gilt. Wenn beispielsweise TOP (10) angegeben wird, sind 10 Zeilen betroffen. Von diesen Zeilen können sieben aktualisiert und drei eingefügt werden, oder eine Zeile kann gelöscht, fünf können aktualisiert und vier eingefügt werden usw.

Häufig wird die TOP-Klausel zum batchweisen Ausführen von DML (Data Manipulation Language)-Vorgängen für eine umfangreiche Tabelle verwendet. Wenn Sie die TOP-Klausel zu diesem Zweck in der MERGE-Anweisung verwenden, müssen Sie sich der folgenden Auswirkungen bewusst sein.

- Die E/A-Leistung ist möglicherweise betroffen.

  Die MERGE-Anweisung führt einen vollständigen Tabellenscan der Quell- und der Zieltabelle aus. Durch Aufteilung des Vorgangs in Batches wird die Anzahl der Schreibvorgänge pro Batch reduziert. Für jeden Batch wird jedoch ein vollständiger Tabellenscan der Quell- und der Zieltabelle ausgeführt. Die resultierende Leseaktivität wirkt sich möglicherweise auf die Leistung der Abfrage aus.

- Es können falsche Ergebnisse auftreten.

  Es sollte unbedingt sichergestellt werden, dass alle aufeinander folgenden Batches neuen Zeilen zugeordnet sind, andernfalls kann ein unerwünschtes Verhalten auftreten, z. B. das Einfügen doppelter Zeilen in die Zieltabelle. Dies kann der Fall sein, wenn die Quelltabelle eine Zeile enthält, die nicht im Zielbatch, aber in der Zieltabelle insgesamt enthalten war.

- So stellen Sie die Richtigkeit der Ergebnisse sicher

  - Bestimmen Sie mithilfe der ON-Klausel die Quellzeilen, die sich auf vorhandene Zielzeilen auswirken bzw. tatsächlich neu sind.
  - Bestimmen Sie mithilfe einer zusätzlichen Bedingung in der WHEN MATCHED-Klausel, ob die Zielzeile bereits in einem früheren Batch aktualisiert wurde.

Da die TOP-Klausel erst nach diesen Klauseln angewendet wird, wird bei jeder Ausführung eine Zeile, die tatsächlich keine Entsprechung besitzt, eingefügt, oder es wird eine vorhandene Zeile aktualisiert.

### <a name="bulk-load-best-practices"></a>Bewährte Methoden zum Massenladen

Die MERGE-Anweisung kann zum effizienten Massenladen von Daten aus einer Quelldatendatei in eine Zieltabelle verwendet werden, indem die `OPENROWSET(BULK…)`-Klausel als Tabellenquelle angegeben wird. Dadurch wird die gesamte Datei als einzelner Batch verarbeitet.

Zur Leistungsverbesserung des Massenladevorgangs werden die folgenden Richtlinien empfohlen:

- Erstellen Sie für die Joinspalten in der Zieltabelle einen gruppierten Index.
- Geben Sie mithilfe des ORDER-Hinweises und des UNIQUE-Hinweises in der `OPENROWSET(BULK…)`-Klausel an, wie die Quelldatendatei sortiert ist.

  Standardmäßig geht der Massenvorgang davon aus, dass die Datendatei nicht sortiert ist. Daher ist es wichtig, dass die Quelldaten anhand des gruppierten Indexes für die Zieltabelle sortiert sind und die Reihenfolge mit dem ORDER-Hinweis angegeben wird, sodass der Abfrageoptimierer einen effizienteren Abfrageplan generieren kann. Hinweise werden zur Laufzeit validiert. Wenn der Datenstrom mit den angegebenen Hinweisen nicht übereinstimmt, wird ein Fehler ausgelöst.

Mit diesen Richtlinien wird sichergestellt, dass die Joinschlüssel eindeutig sind und die Sortierreihenfolge der Daten in der Quelldatei mit der Zieltabelle übereinstimmt. Die Abfrageleistung wird verbessert, da zusätzliche Sortiervorgänge nicht erforderlich sind und keine nicht benötigten Datenkopien angefordert werden.

### <a name="measuring-and-diagnosing-merge-performance"></a>Messen und Diagnostizieren der MERGE-Leistung

Die folgenden Funktionen stehen Ihnen zur Verfügung, um die Leistung von MERGE-Anweisungen zu messen und zu diagnostizieren.

- Geben Sie mithilfe des merge stmt-Indikators in der dynamischen Verwaltungssicht [sys.dm_exec_query_optimizer_info](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-optimizer-info-transact-sql.md) die Anzahl von Abfrageoptimierungen für MERGE-Anweisungen zurück.
- Geben Sie mithilfe des merge_action_type-Attributs in der dynamischen Verwaltungssicht [sys.dm_exec_plan_attributes](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md) den Typ des Triggerausführungsplans zurück, der als Ergebnis einer MERGE-Anweisung verwendet wird.
- Erfassen Sie mit der SQL-Ablaufverfolgung Problembehandlungsdaten für die MERGE-Anweisung auf dieselbe Weise wie für andere DML (Data Manipulation Language)-Anweisungen. Weitere Informationen finden Sie unter [SQL Trace](../../relational-databases/sql-trace/sql-trace.md).

## <a name="examples"></a>Beispiele  

### <a name="a-using-merge-to-do-insert-and-update-operations-on-a-table-in-a-single-statement"></a>A. Verwenden von MERGE zum Ausführen von INSERT- und UPDATE-Vorgängen für eine Tabelle in einer einzelnen Anweisung

Ein häufiges Szenario ist die Aktualisierung einer oder mehrerer Spalten in einer Tabelle, wenn eine übereinstimmende Zeile vorhanden ist. Anderenfalls, wenn keine übereinstimmende Zeile vorhanden ist, das Einfügen der Daten als neue Zeile. In jedem Szenario übergeben Sie normalerweise Parameter an eine gespeicherte Prozedur, die die entsprechende UPDATE-Anweisung und INSERT-Anweisung enthält. Mit der MERGE-Anweisung können Sie beide Tasks in einer einzelnen Anweisung ausführen. Im folgenden Beispiel wird eine gespeicherte Prozedur in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank dargestellt, die sowohl eine INSERT-Anweisung als auch eine UPDATE-Anweisung enthält. Anschließend wird die Prozedur so geändert, dass sie die entsprechenden Vorgänge mit einer einzelnen MERGE-Anweisung ausführt.  
  
```sql  
CREATE PROCEDURE dbo.InsertUnitMeasure  
    @UnitMeasureCode nchar(3),  
    @Name nvarchar(25)  
AS
BEGIN  
    SET NOCOUNT ON;  
-- Update the row if it exists.
    UPDATE Production.UnitMeasure  
SET Name = @Name  
WHERE UnitMeasureCode = @UnitMeasureCode  
-- Insert the row if the UPDATE statement failed.  
IF (@@ROWCOUNT = 0 )  
BEGIN  
    INSERT INTO Production.UnitMeasure (UnitMeasureCode, Name)  
    VALUES (@UnitMeasureCode, @Name)  
END  
END;  
GO  
-- Test the procedure and return the results.  
EXEC InsertUnitMeasure @UnitMeasureCode = 'ABC', @Name = 'Test Value';  
SELECT UnitMeasureCode, Name FROM Production.UnitMeasure  
WHERE UnitMeasureCode = 'ABC';  
GO  
  
-- Rewrite the procedure to perform the same operations using the
-- MERGE statement.  
-- Create a temporary table to hold the updated or inserted values
-- from the OUTPUT clause.  
CREATE TABLE #MyTempTable  
    (ExistingCode nchar(3),  
     ExistingName nvarchar(50),  
     ExistingDate datetime,  
     ActionTaken nvarchar(10),  
     NewCode nchar(3),  
     NewName nvarchar(50),  
     NewDate datetime  
    );  
GO  
ALTER PROCEDURE dbo.InsertUnitMeasure  
    @UnitMeasureCode nchar(3),  
    @Name nvarchar(25)  
AS
BEGIN  
    SET NOCOUNT ON;  
  
    MERGE Production.UnitMeasure AS target  
    USING (SELECT @UnitMeasureCode, @Name) AS source (UnitMeasureCode, Name)  
    ON (target.UnitMeasureCode = source.UnitMeasureCode)  
    WHEN MATCHED THEN
        UPDATE SET Name = source.Name  
    WHEN NOT MATCHED THEN  
        INSERT (UnitMeasureCode, Name)  
        VALUES (source.UnitMeasureCode, source.Name)  
    OUTPUT deleted.*, $action, inserted.* INTO #MyTempTable;  
END;  
GO  
-- Test the procedure and return the results.  
EXEC InsertUnitMeasure @UnitMeasureCode = 'ABC', @Name = 'New Test Value';  
EXEC InsertUnitMeasure @UnitMeasureCode = 'XYZ', @Name = 'Test Value';  
EXEC InsertUnitMeasure @UnitMeasureCode = 'ABC', @Name = 'Another Test Value';  
  
SELECT * FROM #MyTempTable;  
-- Cleanup
DELETE FROM Production.UnitMeasure WHERE UnitMeasureCode IN ('ABC','XYZ');  
DROP TABLE #MyTempTable;  
GO  
```  
  
### <a name="b-using-merge-to-do-update-and-delete-operations-on-a-table-in-a-single-statement"></a>B. Verwenden von MERGE zum Ausführen von UPDATE- und DELETE-Operationen für eine Tabelle in einer einzelnen Anweisung

Im folgenden Beispiel wird die Tabelle `ProductInventory` in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Beispieldatenbank täglich mit MERGE aktualisiert. Dies erfolgt auf der Grundlage der in der Tabelle `SalesOrderDetail` verarbeiteten Bestellungen. Die `Quantity`-Spalte der `ProductInventory`-Tabelle wird aktualisiert, indem die Anzahl der täglich aufgegebenen Bestellungen für die einzelnen Produkte in der `SalesOrderDetail`-Tabelle subtrahiert wird. Wenn die Anzahl der Bestellungen für ein Produkt dazu führt, dass der Produktbestand auf oder unter 0 (null) fällt, wird die Zeile für dieses Produkt aus der `ProductInventory`-Tabelle gelöscht.  
  
```sql  
CREATE PROCEDURE Production.usp_UpdateInventory  
    @OrderDate datetime  
AS  
MERGE Production.ProductInventory AS target  
USING (SELECT ProductID, SUM(OrderQty) FROM Sales.SalesOrderDetail AS sod  
    JOIN Sales.SalesOrderHeader AS soh  
    ON sod.SalesOrderID = soh.SalesOrderID  
    AND soh.OrderDate = @OrderDate  
    GROUP BY ProductID) AS source (ProductID, OrderQty)  
ON (target.ProductID = source.ProductID)  
WHEN MATCHED AND target.Quantity - source.OrderQty <= 0  
    THEN DELETE  
WHEN MATCHED
    THEN UPDATE SET target.Quantity = target.Quantity - source.OrderQty,
                    target.ModifiedDate = GETDATE()  
OUTPUT $action, Inserted.ProductID, Inserted.Quantity,
    Inserted.ModifiedDate, Deleted.ProductID,  
    Deleted.Quantity, Deleted.ModifiedDate;  
GO  
  
EXECUTE Production.usp_UpdateInventory '20030501'  
```  
  
### <a name="c-using-merge-to-do-update-and-insert-operations-on-a-target-table-by-using-a-derived-source-table"></a>C. Verwenden von MERGE zum Ausführen von UPDATE- und INSERT-Vorgängen für eine Zieltabelle unter Verwendung einer abgeleiteten Quelltabelle

Im folgenden Beispiel wird die `SalesReason`-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank durch das Aktualisieren oder Einfügen von Zeilen mithilfe von MERGE geändert. Wenn der Wert von `NewName` in der Quelltabelle einem Wert in der `Name`-Spalte der Zieltabelle entspricht (`SalesReason`), wird die `ReasonType`-Spalte in der Zieltabelle aktualisiert. Wenn der Wert von `NewName` jedoch nicht übereinstimmt, wird die Quellzeile in die Zieltabelle eingefügt. Die Quelltabelle ist eine abgeleitete Tabelle, die mithilfe des [!INCLUDE[tsql](../../includes/tsql-md.md)]-Tabellenwertkonstruktors mehrere Zeilen für die Quelltabelle angibt. Weitere Informationen zum Verwenden des Tabellenwertkonstruktors in einer abgeleiteten Tabelle finden Sie unter [Table Value Constructor &#40;Transact-SQL&#41; (Tabellenwertkonstruktor &#40;Transact-SQL&#41;)](../../t-sql/queries/table-value-constructor-transact-sql.md). Außerdem zeigt das Beispiel, wie die Ergebnisse der OUTPUT-Klausel in einer Tabellenvariablen gespeichert werden. Und dann fassen Sie die Ergebnisse der MERGE-Anweisung zusammen, indem Sie einen einfachen SELECT-Vorgang ausführen, der die Anzahl der eingefügten und aktualisierten Zeilen zurückgibt.  
  
```sql  
-- Create a temporary table variable to hold the output actions.  
DECLARE @SummaryOfChanges TABLE(Change VARCHAR(20));  
  
MERGE INTO Sales.SalesReason AS Target  
USING (VALUES ('Recommendation','Other'), ('Review', 'Marketing'),
              ('Internet', 'Promotion'))  
       AS Source (NewName, NewReasonType)  
ON Target.Name = Source.NewName  
WHEN MATCHED THEN  
UPDATE SET ReasonType = Source.NewReasonType  
WHEN NOT MATCHED BY TARGET THEN  
INSERT (Name, ReasonType) VALUES (NewName, NewReasonType)  
OUTPUT $action INTO @SummaryOfChanges;  
  
-- Query the results of the table variable.  
SELECT Change, COUNT(*) AS CountPerChange  
FROM @SummaryOfChanges  
GROUP BY Change;  
```  
  
### <a name="d-inserting-the-results-of-the-merge-statement-into-another-table"></a>D: Einfügen der Ergebnisse der MERGE-Anweisung in eine andere Tabelle

Im folgenden Beispiel werden aus der OUTPUT-Klausel einer MERGE-Anweisung zurückgegebene Daten erfasst und in eine andere Tabelle eingefügt. Die MERGE-Anweisung aktualisiert die `Quantity`-Spalte der `ProductInventory`-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank täglich auf der Grundlage der Bestellungen, die in der `SalesOrderDetail`-Tabelle verarbeitet werden. In diesem Beispiel werden die aktualisierten Zeilen erfasst und in eine andere Tabelle eingefügt, in der Bestandsänderungen nachverfolgt werden.  
  
```sql  
CREATE TABLE Production.UpdatedInventory  
    (ProductID INT NOT NULL, LocationID int, NewQty int, PreviousQty int,  
     CONSTRAINT PK_Inventory PRIMARY KEY CLUSTERED (ProductID, LocationID));  
GO  
INSERT INTO Production.UpdatedInventory  
SELECT ProductID, LocationID, NewQty, PreviousQty
FROM  
(    MERGE Production.ProductInventory AS pi  
     USING (SELECT ProductID, SUM(OrderQty)
            FROM Sales.SalesOrderDetail AS sod  
            JOIN Sales.SalesOrderHeader AS soh  
            ON sod.SalesOrderID = soh.SalesOrderID  
            AND soh.OrderDate BETWEEN '20030701' AND '20030731'  
            GROUP BY ProductID) AS src (ProductID, OrderQty)  
     ON pi.ProductID = src.ProductID  
    WHEN MATCHED AND pi.Quantity - src.OrderQty >= 0
        THEN UPDATE SET pi.Quantity = pi.Quantity - src.OrderQty  
    WHEN MATCHED AND pi.Quantity - src.OrderQty <= 0
        THEN DELETE  
    OUTPUT $action, Inserted.ProductID, Inserted.LocationID,
        Inserted.Quantity AS NewQty, Deleted.Quantity AS PreviousQty)  
 AS Changes (Action, ProductID, LocationID, NewQty, PreviousQty)
 WHERE Action = 'UPDATE';  
GO  
```  

### <a name="e-using-merge-to-do-insert-or-update-on-a-target-edge-table-in-a-graph-database"></a>E. Verwenden von MERGE zum Ausführen von INSERT oder UPDATE auf eine Edge-Zieltabelle in einer Graphdatenbank

In diesem Beispiel erstellen Sie Knotentabellen `Person` und `City` und eine Edgetabelle `livesIn`. Sie verwenden die MERGE-Anweisung auf dem `livesIn`-Edge und fügen eine neue Zeile ein, wenn der Edge zwischen `Person` und `City` noch nicht vorhanden ist. Wenn der Edge bereits vorhanden ist, aktualisieren Sie nur das StreetAddress-Attribut auf dem `livesIn`-Edge.

```sql
-- CREATE node and edge tables
CREATE TABLE Person
    (
        ID INTEGER PRIMARY KEY,
        PersonName VARCHAR(100)
    )
AS NODE
GO

CREATE TABLE City
    (
        ID INTEGER PRIMARY KEY,
        CityName VARCHAR(100),
        StateName VARCHAR(100)
    )
AS NODE
GO

CREATE TABLE livesIn
    (
        StreetAddress VARCHAR(100)
    )
AS EDGE
GO

-- INSERT some test data into node and edge tables
INSERT INTO Person VALUES (1, 'Ron'), (2, 'David'), (3, 'Nancy')
GO

INSERT INTO City VALUES (1, 'Redmond', 'Washington'), (2, 'Seattle', 'Washington')
GO

INSERT livesIn SELECT P.$node_id, C.$node_id, c
FROM Person P, City C, (values (1,1, '123 Avenue'), (2,2,'Main Street')) v(a,b,c)
WHERE P.id = a AND C.id = b
GO

-- Use MERGE to update/insert edge data
CREATE OR ALTER PROCEDURE mergeEdge
    @PersonId integer,
    @CityId integer,
    @StreetAddress varchar(100)
AS
BEGIN
    MERGE livesIn
        USING ((SELECT @PersonId, @CityId, @StreetAddress) AS T (PersonId, CityId, StreetAddress)
                JOIN Person ON T.PersonId = Person.ID
                JOIN City ON T.CityId = City.ID)
        ON MATCH (Person-(livesIn)->City)
    WHEN MATCHED THEN
        UPDATE SET StreetAddress = @StreetAddress
    WHEN NOT MATCHED THEN
        INSERT ($from_id, $to_id, StreetAddress)
        VALUES (Person.$node_id, City.$node_id, @StreetAddress) ;
END
GO

-- Following will insert a new edge in the livesIn edge table
EXEC mergeEdge 3, 2, '4444th Avenue'
GO

-- Following will update the StreetAddress on the edge that connects Ron to Redmond
EXEC mergeEdge 1, 1, '321 Avenue'
GO

-- Verify that all the address were added/updated correctly
SELECT PersonName, CityName, StreetAddress
FROM Person , City , livesIn
WHERE MATCH(Person-(livesIn)->city)
GO
```
  
## <a name="see-also"></a>Weitere Informationen

- [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)
- [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
- [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)
- [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)
- [OUTPUT Clause &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md)
- [MERGE in Integration Services-Paketen](../../integration-services/control-flow/merge-in-integration-services-packages.md)
- [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)
- [Table Value Constructor &#40;Transact-SQL&#41; (Tabellenwertkonstruktor &#40;Transact-SQL&#41;)](../../t-sql/queries/table-value-constructor-transact-sql.md)  
