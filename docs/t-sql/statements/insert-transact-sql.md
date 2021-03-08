---
description: INSERT (Transact-SQL)
title: INSERT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- INSERT_TSQL
- INSERT
dev_langs:
- TSQL
helpviewer_keywords:
- inserting multiple rows
- user-defined types [SQL Server], inserting values
- DML [SQL Server], INSERT statement
- bulk load [SQL Server]
- row additions [SQL Server], INSERT statement
- inserting rows
- table value constructor [SQL Server]
- adding data
- INSERT statement [SQL Server], about INSERT statement
- INSERT statement [SQL Server]
- UDTs [SQL Server], inserting values
- adding rows
- INSERT INTO statement
- data manipulation language [SQL Server], INSERT statement
- inserting data
ms.assetid: 1054c76e-0fd5-4131-8c07-a6c5d024af50
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 505cda4f226945cd8788e777b8ac1ea35a3aa47e
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837723"
---
# <a name="insert-transact-sql"></a>INSERT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


Fügt einer Tabelle oder Sicht in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine oder mehrere Zeilen hinzu. Beispiele finden Sie unter [Beispiele](#InsertExamples).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  

[ WITH <common_table_expression> [ ,...n ] ]  
INSERT   
{  
        [ TOP ( expression ) [ PERCENT ] ]   
        [ INTO ]   
        { <object> | rowset_function_limited   
          [ WITH ( <Table_Hint_Limited> [ ...n ] ) ]  
        }  
    {  
        [ ( column_list ) ]   
        [ <OUTPUT Clause> ]  
        { VALUES ( { DEFAULT | NULL | expression } [ ,...n ] ) [ ,...n     ]   
        | derived_table   
        | execute_statement  
        | <dml_table_source>  
        | DEFAULT VALUES   
        }  
    }  
}  
[;]  
  
<object> ::=  
{   
    [ server_name . database_name . schema_name .   
      | database_name .[ schema_name ] .   
      | schema_name .   
    ]  
  table_or_view_name  
}  
  
<dml_table_source> ::=  
    SELECT <select_list>  
    FROM ( <dml_statement_with_output_clause> )   
      [AS] table_alias [ ( column_alias [ ,...n ] ) ]  
    [ WHERE <search_condition> ]  
        [ OPTION ( <query_hint> [ ,...n ] ) ]  
```  
  
```syntaxsql
-- External tool only syntax  

INSERT   
{  
    [BULK]  
    { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
    ( <column_definition> )  
    [ WITH (  
        [ [ , ] CHECK_CONSTRAINTS ]  
        [ [ , ] FIRE_TRIGGERS ]  
        [ [ , ] KEEP_NULLS ]  
        [ [ , ] KILOBYTES_PER_BATCH = kilobytes_per_batch ]  
        [ [ , ] ROWS_PER_BATCH = rows_per_batch ]  
        [ [ , ] ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) ]  
        [ [ , ] TABLOCK ]  
    ) ]  
}  
  
[; ] <column_definition> ::=  
 column_name <data_type>  
    [ COLLATE collation_name ]  
    [ NULL | NOT NULL ]  
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

INSERT INTO { database_name.schema_name.table_name | schema_name.table_name | table_name }
    [ ( column_name [ ,...n ] ) ]  
    {   
      VALUES ( { NULL | expression } )  
      | SELECT <select_criteria>  
    }  
    [ OPTION ( <query_option> [ ,...n ] ) ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 WITH \<common_table_expression>  
 Gibt das (auch als allgemeiner Tabellenausdruck bezeichnete) temporäre benannte Resultset an, das innerhalb des Bereichs der INSERT-Anweisung definiert ist. Das Resultset wird von einer SELECT-Anweisung abgeleitet. Weitere Informationen finden Sie unter [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 TOP (*expression*) [ PERCENT ]  
 Gibt die Anzahl oder den Prozentsatz willkürlicher Zeilen an, die eingefügt werden. *expression* kann eine Anzahl oder ein Prozentsatz der Zeilen sein. Weitere Informationen finden Sie unter [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
 INTO  
 Ein optionales Schlüsselwort, das zwischen INSERT und der Zieltabelle verwendet werden kann.  
  
 *server_name*  
 **Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Der Name des Verbindungsservers, auf dem sich die Tabelle oder Sicht befinden. Für *server_name* kann der Name eines [Verbindungsservers](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) angegeben werden, oder Sie verwenden die Funktion [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md).  
  
 Wenn *server_name* als Verbindungsserver angegeben ist, sind *database_name* und *schema_name* erforderlich. Wenn *server_name* mit OPENDATASOURCE angegeben wird, gelten *database_name* und *schema_name* möglicherweise nicht für alle Datenquellen und unterliegen den Funktionen des OLE DB-Anbieters, der auf das Remoteobjekt zugreift.  
  
 *database_name*  
 **Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Der Name der Datenbank.  
  
 *schema_name*  
 Der Name des Schemas, zu dem die Tabelle oder Sicht gehört.  
  
 *table_or_view_name*  
 Der Name der Tabelle oder Sicht, die die Daten empfangen soll.  
  
 Innerhalb ihres Bereichs kann eine [table](../../t-sql/data-types/table-transact-sql.md)-Variable als Tabellenquelle in einer INSERT-Anweisung verwendet werden.  
  
 Die Ansicht, auf die *table_or_view_name* verweist, muss aktualisierbar sein und auf genau eine Basistabelle in der FROM-Klausel der Ansicht verweisen. Beispielsweise muss eine INSERT-Anweisung für eine auf mehreren Tabellen basierende Sicht eine *column_list* verwenden, die nur auf Spalten einer einzigen Basistabelle verweist. Weitere Informationen zu aktualisierbaren Ansichten finden Sie unter [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
 *rowset_function_limited*  
 **Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Die [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md)-Funktion oder die [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md)-Funktion. Die Verwendung dieser Funktionen unterliegt den Funktionen des OLE DB-Anbieters, der auf das Remoteobjekt zugreift.  
  
 WITH ( \<table_hint_limited> [... *n* ] )  
 Gibt mindestens einen Tabellenhinweis an, der für eine Zieltabelle zulässig ist. Das WITH-Schlüsselwort und die Klammern sind erforderlich.  
  
 READPAST, NOLOCK und READUNCOMMITTED sind nicht zulässig. Weitere Informationen zu Tabellenhinweisen finden Sie unter [Tabellenhinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
> [!IMPORTANT]  
>  Die Möglichkeit, die Hinweise HOLDLOCK, SERIALIZABLE, READCOMMITTED, REPEATABLEREAD oder UPDLOCK für Tabellen anzugeben, bei denen es sich um Ziele von INSERT-Anweisungen handelt, wird in einer zukünftigen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entfernt. Diese Hinweise beeinträchtigen die Leistung von INSERT-Anweisungen nicht. Vermeiden Sie ihre Verwendung bei neuen Entwicklungsarbeiten, und planen Sie die Änderung von Anwendungen, in denen sie aktuell verwendet werden.  
  
 Das Angeben eines TABLOCK-Hinweises für eine Tabelle, die das Ziel einer INSERT-Anweisung ist, hat dieselbe Wirkung wie das Angeben eines TABLOCKX-Hinweises. Auf die Tabelle wird eine exklusive Sperre angewendet.  
  
 (*column_list*)  
 Eine Liste mit einer oder mehreren Spalten, in die Daten eingefügt werden sollen. *column_list* muss in Klammern eingeschlossen und durch ein Trennzeichen getrennt werden.  
  
 Ist eine Spalte nicht in *column_list* enthalten, muss [!INCLUDE[ssDE](../../includes/ssde-md.md)] in der Lage sein, basierend auf der Spaltendefinition einen Wert bereitzustellen. Andernfalls kann die Zeile nicht geladen werden. [!INCLUDE[ssDE](../../includes/ssde-md.md)] stellt automatisch einen Wert für die Spalte bereit, wenn für sie eine der folgenden Bedingungen erfüllt ist:  
  
-   Besitzt eine IDENTITY-Eigenschaft. Der nächste inkrementelle Identitätswert wird verwendet.  
  
-   Verfügt über einen Standardwert. Der Standardwert der Spalte wird verwendet.  
  
-   Verfügt über einen **timestamp**-Datentyp. Der aktuelle Zeitstempelwert wird verwendet.  
  
-   Lässt NULL-Werte zu. Ein NULL-Wert wird verwendet.  
  
-   Ist eine berechnete Spalte. Der berechnete Wert wird verwendet.  
  
*column_list* muss beim Einfügen von expliziten Werten in eine Identitätsspalte verwendet werden. Dabei muss die Option SET IDENTITY_INSERT für die Tabelle auf ON festgelegt sein.  
  
OUTPUT-Klausel  
 Gibt eingefügte Zeilen als Teil des Einfügevorgangs zurück. Die Ergebnisse können an die Verarbeitungsanwendung zurückgegeben bzw. zur weiteren Verarbeitung in eine Tabelle oder eine Tabellenvariable eingefügt werden.  
  
 Die OUTPUT-Klausel wird in DML-Anweisungen, die auf lokale partitionierte Sichten, verteilte partitionierte Sichten oder Remotetabellen verweisen, oder INSERT-Anweisungen, die *execute_statement* enthalten, nicht unterstützt. Die OUTPUT INTO-Klausel wird nicht in INSERT-Anweisungen unterstützt, die eine \<dml_table_source>-Klausel enthalten. Weitere Informationen zu den Argumenten und dem Verhalten dieser Klausel finden Sie unter [OUTPUT-Klausel (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md).
  
 VALUES  
 Steht vor der Liste oder den Listen der Datenwerte, die eingefügt werden sollen. Für jede Spalte in *column_list* (falls angegeben) bzw. in der Tabelle muss ein Datenwert vorhanden sein. Die Wertliste muss in Klammern stehen.  
  
 Wenn die Reihenfolge der Werte in der Werteliste nicht mit der Reihenfolge der Spalten in der Tabelle übereinstimmt oder wenn nicht für jede Spalte in der Tabelle ein Wert vorhanden ist, muss mithilfe von *column_list* explizit die Spalte angegeben werden, in der ein eingehender Wert gespeichert werden soll.  
  
 Mit dem [!INCLUDE[tsql](../../includes/tsql-md.md)]-Zeilenkonstruktor (auch Tabellenwertkonstruktor genannt) können mehrere Zeilen in einer einzelnen INSERT-Anweisung angegeben werden. Der Zeilenkonstruktor besteht aus einer einzelnen VALUES-Klausel mit mehreren Wertelisten, die in Klammern eingeschlossen und durch ein Komma getrennt sind. Weitere Informationen finden Sie unter [Tabellenwertkonstruktor &#40;Transact-SQL&#41;](../../t-sql/queries/table-value-constructor-transact-sql.md).  

> [!NOTE]  
> In Azure Synapse Analytics können Einfügewerte nur konstante Literalwerte oder Verweise auf Variablen sein. Um ein Nichtliteral einzufügen, legen Sie eine Variable auf einen nicht konstanten Wert fest, und fügen Sie die Variable ein.

 DEFAULT  
 Erzwingt, dass [!INCLUDE[ssDE](../../includes/ssde-md.md)] den für eine Spalte definierten Standardwert lädt. Wenn für die Spalte kein Standardwert vorhanden ist und die Spalte NULL-Werte zulässt, wird NULL eingefügt. Für eine Spalte, die mit dem Datentyp **timestamp** definiert ist, wird der nächste Zeitstempelwert eingefügt. DEFAULT ist für eine Identitätsspalte nicht zulässig.  
  
 *expression*  
 Eine Konstante, eine Variable oder ein Ausdruck. Der Ausdruck darf keine EXECUTE-Anweisung enthalten.  
  
 In Verweisen auf die Zeichendatentypen in Unicode **nchar**, **nvarchar** und **ntext** muss *expression* der Großbuchstabe „N“ vorangestellt werden. Wenn "N" nicht angegeben wird, konvertiert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Zeichenfolge in die Codepage, die der Standardsortierung der Datenbank oder Spalte entspricht. Alle Zeichen, die in der betreffenden Codepage nicht gefunden werden, gehen verloren.  
  
 *derived_table*  
 Eine gültige SELECT-Anweisung, die in die Tabelle zu ladende Datenzeilen zurückgibt. Die SELECT-Anweisung kann keinen allgemeinen Tabellenausdruck (Common Table Expression, CTE) enthalten.  
  
 *execute_statement*  
 Eine gültige EXECUTE-Anweisung, die Daten mithilfe von SELECT- oder READTEXT-Anweisungen zurückgibt. Weitere Informationen finden Sie unter [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md).  
  
 Die RESULT SETS-Optionen der EXECUTE-Anweisung können in einer INSERT…EXEC-Anweisung nicht angegeben werden.  
  
 Wenn *execute_statement* mit INSERT verwendet wird, muss jedes Resultset mit den Spalten in der Tabelle oder in *column_list* kompatibel sein.  
  
 *execute_statement* kann zum Ausführen gespeicherter Prozeduren auf dem gleichen Server oder auf einem Remoteserver verwendet werden. Die Prozedur auf dem Remoteserver wird ausgeführt, und die Resultsets werden an den lokalen Server gesendet. Anschließend werden sie in die Tabelle auf dem lokalen Server geladen. In einer verteilten Transaktion kann *execute_statement* nicht für einen verknüpften Loopbackserver ausgeführt werden, wenn für die Verbindung mehrere aktive Resultsets (MARS) aktiviert ist.  
  
 Wenn *execute_statement* Daten mit der READTEXT-Anweisung zurückgibt, kann jede READTEXT-Anweisung maximal 1 MB (1024 KB) an Daten zurückgeben. *execute_statement* kann auch mit erweiterten Prozeduren verwendet werden. *execute_statement* fügt die durch den Hauptthread der erweiterten Prozedur zurückgegebenen Daten ein. Die Ausgabe von anderen Threads als dem Hauptthread wird hingegen nicht eingefügt.  
  
 Sie können keinen Tabellenwertparameter als Ziel einer INSERT EXEC-Anweisung angeben; er kann jedoch als Quelle in der INSERT EXEC-Zeichenfolge oder der gespeicherten Prozedur angegeben werden. Weitere Informationen finden Sie unter [Use Table-Valued Parameters &#40;Database Engine&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
 \<dml_table_source>  
 Gibt an, dass es sich bei den in die Zieltabelle eingefügten Zeilen um die von der OUTPUT-Klausel in der INSERT-, UPDATE-, DELETE- oder MERGE-Anweisung zurückgegebenen Zeilen handelt, die optional durch eine WHERE-Klausel gefiltert werden. Wenn \<dml_table_source> angegeben ist, muss das Ziel der äußeren INSERT-Anweisung die folgenden Einschränkungen einhalten: 
  
-   Die Tabelle muss eine Basistabelle sein, keine Sicht.  
  
-   Die Tabelle darf keine Remotetabelle sein.  
  
-   Die Tabelle darf keine Definition von aktivierten Triggern besitzen.  
  
-   Die Tabelle darf an keinen Primär-/Fremdschlüsselbeziehungen teilnehmen.  
  
-   Sie darf nicht an Mergereplikationen oder aktualisierbaren Abonnements für Transaktionsreplikationen teilnehmen.  
  
 Der Kompatibilitätsgrad der Datenbank muss auf 100 oder höher festgelegt sein. Weitere Informationen finden Sie unter [OUTPUT-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).  
  
 \<select_list>  
 Eine durch Trennzeichen getrennte Liste, die angibt, welche von der OUTPUT-Klausel zurückgegebenen Spalten eingefügt werden sollen. Die Spalten in \<select_list> müssen mit den Spalten kompatibel sein, in die Werte eingefügt werden. \<select_list> kann nicht auf Aggregatfunktionen oder TEXTPTR verweisen. 
  
> [!NOTE]  
>  In der SELECT-Liste aufgeführte Variablen verweisen auf ihre ursprünglichen Werte, unabhängig von den Änderungen, die daran in \<dml_statement_with_output_clause> vorgenommen wurden.  
  
 \<dml_statement_with_output_clause>  
 Eine gültige INSERT-, UPDATE-, DELETE- oder MERGE-Anweisung, die betroffene Zeilen in einer OUTPUT-Klausel zurückgibt. Die Anweisung darf keine WITH-Klausel enthalten und sich nicht auf Remotetabellen oder partitionierte Sichten beziehen. Wenn UPDATE oder DELETE angegeben wird, darf kein cursorbasiertes UPDATE oder DELETE verwendet werden. Auf Quellzeilen darf nicht als geschachtelte DML-Anweisungen verwiesen werden.  
  
 WHERE \<search_condition>  
 Eine WHERE-Klausel, die eine gültige \<search_condition> enthält, die die von \<dml_statement_with_output_clause> zurückgegebenen Zeilen filtert. Weitere Informationen finden Sie unter [Suchbedingung &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md). Bei Verwendung in diesem Kontext darf \<search_condition> keine Unterabfragen und benutzerdefinierte Skalarfunktionen für einen Datenzugriff, Aggregatfunktionen, TEXTPTR oder Prädikate der Volltextsuche enthalten. 
  
 DEFAULT VALUES  
 **Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Erzwingt, dass die neue Zeile den für jede Spalte definierten Standardwert enthält.  
  
 BULK  
**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Wird von externen Tools verwendet, um einen Binärdatenstrom hochzuladen. Diese Option ist nicht zur Verwendung mit Tools wie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], SQLCMD, OSQL oder Anwendungsprogrammierschnittstellen für den Datenzugriff wie den nativen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Client bestimmt.  
  
 FIRE_TRIGGERS  
 **Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Gibt an, dass INSERT-Trigger, die für die Zieltabelle definiert sind, während des Binärdatenstrom-Uploads ausgeführt werden. Weitere Informationen finden Sie unter [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
 CHECK_CONSTRAINTS  
 **Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Gibt an, dass alle Einschränkungen, die für die Zieltabelle oder -sicht gelten, während des Binärdatenstrom-Uploads überprüft werden müssen. Weitere Informationen finden Sie unter [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
 KEEPNULLS  
 **Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Gibt an, dass leere Spalten während des Binärdatenstrom-Uploads einen NULL-Wert beibehalten sollen. Weitere Informationen finden Sie unter [Beibehalten von NULL-Werten oder Verwenden von Standardwerten während des Massenimports &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md).  
  
 KILOBYTES_PER_BATCH = kilobytes_per_batch  
 Gibt die ungefähre Datenmenge pro Batch in KB als *kilobytes_per_batch* an. Weitere Informationen finden Sie unter [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
 ROWS_PER_BATCH =*rows_per_batch*  
 **Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
 Gibt die ungefähre Anzahl von Datenzeilen im Binärdatenstrom an. Weitere Informationen finden Sie unter [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
> [!NOTE]
>  Ein Syntaxfehler wird ausgelöst, wenn keine Spaltenliste bereitgestellt wird.  

## <a name="remarks"></a>Bemerkungen  
Spezifische Informationen zum Einfügen von Daten in SQL-Graph-Tabellen finden Sie unter [INSERT (SQL-Graph)](../../t-sql/statements/insert-sql-graph.md). 

## <a name="best-practices"></a>Bewährte Methoden  
 Verwenden Sie die Funktion @@ROWCOUNT, um die Anzahl der eingefügten Zeilen an die Clientanwendung zurückzugeben. Weitere Informationen finden Sie unter [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md).  
  
### <a name="best-practices-for-bulk-importing-data"></a>Bewährte Methoden für den Massenimport von Daten  
  
#### <a name="using-insert-intoselect-to-bulk-import-data-with-minimal-logging-and-parallelism"></a>Verwenden von INSERT INTO…SELECT für den Massenimport von Daten mit minimaler Protokollierung und Parallelität 
Sie können `INSERT INTO <target_table> SELECT <columns> FROM <source_table>` verwenden, um eine große Anzahl von Zeilen aus einer Tabelle, z.B. einer Stagingtabelle, effizient in eine andere Tabelle mit minimaler Protokollierung zu übertragen. Die minimale Protokollierung kann die Leistung der Anweisung verbessern und die Wahrscheinlichkeit senken, dass der Vorgang den verfügbaren Transaktionsprotokoll-Speicherplatz während der Transaktion auffüllt.  
  
Bei der minimalen Protokollierung für diese Anweisung müssen die folgenden Voraussetzungen erfüllt sein:  
-   Das Wiederherstellungsmodell der Datenbank ist auf einfach oder massenprotokolliert festgelegt.  
-   Die Zieltabelle ist ein leerer oder nicht leerer Heap.  
-   Die Zieltabelle wird nicht in der Replikation verwendet.  
-   Der `TABLOCK`-Hinweis wird für die Zieltabelle angegeben.  
  
Zeilen, die infolge einer Einfügeaktion in eine MERGE-Anweisung in einen Heap eingefügt werden, können ebenfalls minimal protokolliert werden.  
  
Im Gegensatz zur `BULK INSERT`-Anweisung, die eine weniger restriktive Massenupdatesperre (BU-Sperre) enthält, weist `INSERT INTO … SELECT` mit dem `TABLOCK`-Hinweis eine exklusive Sperre (X) für die Tabelle auf. Das bedeutet, dass Sie Zeilen nicht mit mehreren parallel ausgeführten Einfügevorgängen einfügen können. 

Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und dem Datenbank-Kompatibilitätsgrad 130 kann die `INSERT INTO … SELECT`-Anweisung jedoch parallel ausgeführt werden, wenn in Heaps oder gruppierte Columnstore-Indizes (CCI) eingefügt wird. Parallele Einfügungen sind möglich, wenn der `TABLOCK`-Hinweis verwendet wird.  

Für die Parallelität der obigen Anweisung gelten die folgenden Anforderungen, die den Anforderungen für die minimale Protokollierung ähneln:  
-   Die Zieltabelle ist ein leerer oder nicht leerer Heap.  
-   Die Zieltabelle enthält einen gruppierten Columnstore-Index (CCI), aber keine nicht gruppierten Indizes.  
-   Die Zieltabelle verfügt nicht über eine Identitätsspalte, bei der IDENTITY_INSERT auf OFF festgelegt ist.  
-   Der `TABLOCK`-Hinweis wird für die Zieltabelle angegeben.

Bei Szenarien, in denen die Mindestanforderungen an die Protokollierung und die parallele Einfügung erfüllt sind, können beide Verbesserungen zusammen verwendet werden, um den maximalen Durchsatz der Datenladevorgänge sicherzustellen.

> [!NOTE]
> Einfügevorgänge in lokale temporäre Tabellen (durch das #-Präfix gekennzeichnet) und in globale temporäre Tabellen (durch das ##-Präfix gekennzeichnet) sind ebenfalls für Parallelität geeignet, wenn der TABLOCK-Hinweis verwendet wird.
  
#### <a name="using-openrowset-and-bulk-to-bulk-import-data"></a>Verwenden von OPENROWSET und BULK für den Massenimport von Daten  
 Von der OPENROWSET-Funktion können die folgenden Tabellenhinweise akzeptiert werden, die Massenladeoptimierungen mit der INSERT-Anweisung bereitstellen:  
  
-   Der `TABLOCK`-Hinweis kann die Anzahl der Protokolldatensätze für den Einfügevorgang minimieren. Das Wiederherstellungsmodell der Datenbank muss auf einfach oder massenprotokolliert festgelegt werden, und die Zieltabelle kann nicht in der Replikation verwendet werden. Weitere Informationen finden Sie unter [Voraussetzungen für die minimale Protokollierung beim Massenimport](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).  
-   Mit dem `TABLOCK`-Hinweis können parallele Einfügevorgänge ermöglicht werden. Die Zieltabelle ist ein Heap oder ein gruppierter Columnstore-Index (CCI) ohne nicht gruppierte Indizes, und für die Zieltabelle darf keine Identitätsspalte angegeben sein.  
-   Durch den `IGNORE_CONSTRAINTS`-Hinweis kann vorübergehend die FOREIGN KEY- und CHECK-Einschränkungsüberprüfung deaktiviert werden.  
-   Durch den `IGNORE_TRIGGERS`-Hinweis kann vorübergehend die Ausführung des Triggers deaktiviert werden.  
-   Der `KEEPDEFAULTS`-Hinweis ermöglicht das Einfügen eines Standardwerts für eine Tabellenspalte (falls vorhanden) anstelle von NULL, wenn der Datensatz keinen Wert für die Spalte aufweist.  
-   Der `KEEPIDENTITY`-Hinweis ermöglicht die Verwendung der Identitätswerte in der importierten Datendatei für die Identitätsspalte in der Zieltabelle.  
  
Diese Optimierungen sind mit denen vergleichbar, die mit dem `BULK INSERT`-Befehl verfügbar sind. Weitere Informationen finden Sie unter [Tabellenhinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
## <a name="data-types"></a>Datentypen  
 Beachten Sie beim Einfügen von Zeilen das folgende Datentypverhalten:  
  
-   Wird ein Wert in Spalten mit dem Datentyp **char**, **varchar** oder **varbinary** geladen, ist die Auffüllung mit Leerstellen oder das Abschneiden nachfolgender Leerstellen (Leerzeichen bei **char** und **varchar**, Nullen bei **varbinary**) abhängig von der Einstellung für SET ANSI_PADDING, die bei der Tabellenerstellung für die Spalte festgelegt wurde. Weitere Informationen finden Sie unter [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
     Die folgende Tabelle zeigt den Standardvorgang für SET ANSI_PADDING OFF.  
  
    |Datentyp|Standardvorgang|  
    |---------------|-----------------------|  
    |**char**|Füllt den Wert mit Leerzeichen auf, bis die definierte Breite der Spalte erreicht ist.|  
    |**varchar**|Löscht nachfolgende Leerzeichen bis zum ersten Zeichen, das kein Leerzeichen ist, oder alle Zeichen bis auf eines, wenn die Zeichenfolge nur aus Leerzeichen besteht.|  
    |**varbinary**|Löscht nachfolgende Nullen.|  
  
-   Wird eine leere Zeichenfolge („ “) in eine Spalte mit dem Datentyp **varchar** oder **text** geladen, wird standardmäßig eine Zeichenfolge der Länge 0 (null) eingefügt.  
  
-   Durch das Einfügen eines NULL-Werts in eine Spalte mit dem Datentyp **text** oder **image** wird kein gültiger Textzeiger erstellt, und es wird vorab auch keine 8-KB-Textseite zugeordnet.  
  
-   Mit dem Datentyp **uniqueidentifier** erstellte Spalten enthalten speziell formatierte 16-Byte-Binärwerte. Anders als bei Identitätsspalten generiert [!INCLUDE[ssDE](../../includes/ssde-md.md)] für Spalten mit dem Datentyp **uniqueidentifier** nicht automatisch Werte. Bei einem Einfügevorgang können Variablen mit einem **uniqueidentifier**-Datentyp und Zeichenfolgenkonstanten der Form *xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx* (36 Zeichen inklusive Bindestriche, wobei *x* für eine Hexadezimalziffer im Bereich 0–9 bzw. a–f steht) für **uniqueidentifier**-Spalten verwendet werden. Der Wert 6F9619FF-8B86-D011-B42D-00C04FC964FF ist z.B. ein gültiger Wert für eine **uniqueidentifier**-Variable oder -Spalte. Verwenden Sie die Funktion [NEWID()](../../t-sql/functions/newid-transact-sql.md), um eine GUID (ein eindeutiger Bezeichner) abzurufen.  
  
### <a name="inserting-values-into-user-defined-type-columns"></a>Einfügen von Werten in Spalten eines benutzerdefinierten Typs  
 Sie können Werte in Spalten eines benutzerdefinierten Typs einfügen, indem Sie eine der folgenden Methoden verwenden:  
  
-   Bereitstellen eines Werts des benutzerdefinierten Typs  
  
-   Bereitstellen eines Werts eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemdatentyps, sofern der benutzerdefinierte Typ implizite oder explizite Konvertierung von diesem Typ unterstützt. Im folgenden Beispiel wird gezeigt, wie ein Wert durch explizite Konvertierung von einer Zeichenfolge in eine Spalte des benutzerdefinierten Typs `Point` eingefügt wird.  
  
    ```sql
    INSERT INTO Cities (Location)  
    VALUES ( CONVERT(Point, '12.3:46.2') );  
    ```  
  
     Ein binärer Wert kann auch ohne explizite Konvertierung bereitgestellt werden, da alle benutzerdefinierten Typen implizit aus binären Werten konvertierbar sind.  
  
-   Aufrufen einer benutzerdefinierten Funktion, die einen Wert des benutzerdefinierten Typs zurückgibt. Das folgende Beispiel verwendet die benutzerdefinierte Funktion `CreateNewPoint()`, um einen neuen Wert des benutzerdefinierten Typs `Point` zu erstellen und den Wert in die `Cities`-Tabelle einzufügen.  
  
    ```sql
    INSERT INTO Cities (Location)  
    VALUES ( dbo.CreateNewPoint(x, y) );  
    ```  
  
## <a name="error-handling"></a>Fehlerbehandlung  
 Sie können die Fehlerbehandlung für die INSERT-Anweisung durch Angeben der Anweisung in einem TRY…CATCH-Konstrukt implementieren.  
  
 Wenn eine INSERT-Anweisung eine Einschränkung oder Regel verletzt bzw. die Anweisung einen Wert enthält, der mit dem Datentyp der Spalte nicht kompatibel ist, schlägt die Anweisung fehl, und eine Fehlermeldung wird zurückgegeben.  
  
 Wenn INSERT mehrere Zeilen mit SELECT oder EXECUTE lädt, bewirkt eine Verletzung einer Regel oder Einschränkung beim Laden der Werte, dass die Anweisung beendet und keine Zeile geladen wird.  
  
 Wenn in einer INSERT-Anweisung bei der Auswertung eines Ausdrucks ein arithmetischer Fehler (Überlauf, Division durch Null oder Domänenfehler) auftritt, behandelt [!INCLUDE[ssDE](../../includes/ssde-md.md)] den Fehler so, als wäre SET ARITHABORT auf ON festgelegt. Der Batch wird beendet, und eine Fehlermeldung wird zurückgegeben. Wenn in einer Anweisung INSERT, DELETE oder UPDATE ein arithmetischer Fehler (Überlauf, Division durch 0 (null) oder Bereichsfehler) bei der Auswertung eines Ausdrucks auftritt und SET ARITHABORT und SET ANSI_WARNINGS auf OFF festgelegt ist, fügt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] einen NULL-Wert ein oder aktualisiert ihn. Wenn die Zielspalte keine NULL-Werte zulässt, schlägt das Einfügen oder Aktualisieren fehl, und dem Benutzer wird ein Fehler angezeigt.  
  
## <a name="interoperability"></a>Interoperabilität  
 Wenn ein `INSTEAD OF`-Trigger für INSERT-Aktionen für eine Tabelle oder Sicht definiert ist, wird der Trigger anstelle der INSERT-Anweisung ausgeführt. Weitere Informationen zu `INSTEAD OF`-Triggern finden Sie unter [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
## <a name="limitations-and-restrictions"></a>Einschränkungen  
 Wenn in Remotetabellen Werte eingefügt und nicht alle Werte für alle Spalten angegeben werden, müssen Sie die Spalten identifizieren, in denen die angegebenen Werte eingefügt werden sollen.  
  
 Wenn TOP mit INSERT verwendet wird, werden die Zeilen, auf die verwiesen wird, nicht auf bestimmte Weise angeordnet, und die ORDER BY-Klausel kann in dieser Anweisung nicht direkt angegeben werden. Wenn Sie die TOP-Klausel verwenden müssen, um Zeilen in einer sinnvollen Reihenfolge einzufügen, müssen Sie sie zusammen mit einer ORDER BY-Klausel in einer untergeordneten SELECT-Anweisung verwenden. Weitere Informationen finden Sie im Abschnitt "Beispiele" in diesem Thema.
 
INSERT-Abfragen, die SELECT mit ORDER BY zum Füllen von Zeilen verwenden, stellen sicher, wie Identitätswerte berechnet werden, jedoch nicht in der Reihenfolge, in der die Zeilen eingefügt werden.

In Parallel Data Warehouse ist die ORDER BY-Klausel in VIEWS, CREATE TABLE AS SELECT, INSERT SELECT, Inlinefunktionen, abgeleiteten Tabellen, Unterabfragen und allgemeinen Tabellenausdrücken nur dann gültig, wenn auch TOP angegeben wird.
  
## <a name="logging-behavior"></a>Protokollierungsverhalten  
 Die INSERT-Anweisung wird immer vollständig protokolliert, sofern nicht die OPENROWSET-Funktion mit dem BULK-Schlüsselwort oder `INSERT INTO <target_table> SELECT <columns> FROM <source_table>` verwendet wird. Für diese Vorgänge ist eine minimale Protokollierung möglich. Weitere Informationen finden Sie im Abschnitt "Bewährte Methoden zum Massenladen von Daten" weiter oben in diesem Thema.  
  
## <a name="security"></a>Sicherheit  
 Im Verlauf der Verbindung mit einem Verbindungsserver stellt der sendende Server einen Benutzernamen und ein Kennwort bereit, um eine Verbindung mit dem empfangenden Server in dessen Auftrag aufzubauen. Damit diese Verbindung funktioniert, müssen Sie mithilfe von [sp_addlinkedsrvlogin](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md) eine Anmeldenamenzuordnung zwischen den Verbindungsservern erstellen.  
  
 Für die Verwendung von OPENROWSET(BULK…) ist es wichtig, nachvollziehen zu können, wie in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit Identitätswechseln umgegangen wird. Weitere Informationen finden Sie im Abschnitt „Überlegungen zur Sicherheit“ unter [Importieren von Massendaten mithilfe von BULK INSERT oder OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).  
  
### <a name="permissions"></a>Berechtigungen  
 Die INSERT-Berechtigung ist für die Zieltabelle erforderlich.  
  
 Standardmäßig verfügen Mitglieder der festen Serverrolle `sysadmin`, der festen Datenbankrollen `db_owner` und `db_datawriter` sowie Tabellenbesitzer über INSERT-Berechtigungen. Mitglieder der Rollen `sysadmin`, `db_owner` und `db_securityadmin` sowie der Tabellenbesitzer können Berechtigungen an andere Benutzer übertragen.  
  
 Zum Ausführen von INSERT mit der BULK-Option der OPENROWSET-Funktion müssen Sie Mitglied der festen Serverrolle `sysadmin` oder der festen Serverrolle `bulkadmin` sein.  
  
##  <a name="examples"></a><a name="InsertExamples"></a> Beispiele  
  
|Category|Funktionssyntaxelemente|  
|--------------|------------------------------|  
|[Grundlegende Syntax](#BasicSyntax)|INSERT • Tabellenwertkonstruktor|  
|[Behandeln von Spaltenwerten](#ColumnValues)|IDENTITY • NEWID • Standardwerte • Benutzerdefinierte Typen|  
|[Einfügen von Daten aus anderen Tabellen](#OtherTables)|INSERT…SELECT • INSERT…EXECUTE • WITH Allgemeiner Tabellenausdruck • TOP • OFFSET FETCH|  
|[Angeben von Zielobjekten, die keine Standardtabellen sind](#TargetObjects)|Views • Tabellenvariablen|  
|[Einfügen von Zeilen in eine Remotetabelle](#RemoteTables)|Verbindungsserver • OPENQUERY-Rowsetfunktion • OPENDATASOURCE-Rowsetfunktion|  
|[Massenladen von Daten aus Tabellen oder Datendateien](#BulkLoad)|INSERT…SELECT • OPENROWSET-Funktion|  
|[Überschreiben des Standardverhaltens des Abfrageoptimierers mithilfe von Hinweisen](#TableHints)|Tabellenhinweise|  
|[Erfassen der Ergebnisse der INSERT-Anweisung](#CaptureResults)|OUTPUT-Klausel|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Grundlegende Syntax  
 Anhand von Beispielen in diesem Abschnitt wird die grundlegende Funktion der INSERT-Anweisung mithilfe der mindestens erforderlichen Syntax veranschaulicht.  
  
#### <a name="a-inserting-a-single-row-of-data"></a>A. Einfügen einer einzelnen Datenzeile  
 Im folgenden Beispiel wird eine Zeile in die `Production.UnitMeasure`-Tabelle der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank eingefügt. Die Spalten in dieser Tabelle heißen `UnitMeasureCode`, `Name` und `ModifiedDate`. Da Werte für alle Spalten bereitgestellt werden und in der Reihenfolge der Spalten in der Tabelle aufgelistet sind, müssen die Spaltennamen nicht in der Spaltenliste angegeben werden *.*  
  
```sql
INSERT INTO Production.UnitMeasure  
VALUES (N'FT', N'Feet', '20080414');  
```  
  
#### <a name="b-inserting-multiple-rows-of-data"></a>B. Einfügen mehrerer Datenzeilen  
 Im folgenden Beispiel werden mit dem [Tabellenwertkonstruktor](../../t-sql/queries/table-value-constructor-transact-sql.md) in einer einzelnen INSERT-Anweisung drei Zeilen in die Tabelle `Production.UnitMeasure` der Datenbank [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] eingefügt. Da Werte für alle Spalten bereitgestellt werden und in der Reihenfolge der Spalten in der Tabelle aufgelistet sind, müssen die Spaltennamen nicht in der Spaltenliste angegeben werden.  
  
```sql
INSERT INTO Production.UnitMeasure  
VALUES (N'FT2', N'Square Feet ', '20080923'), (N'Y', N'Yards', '20080923')
    , (N'Y3', N'Cubic Yards', '20080923');  
```  
  
#### <a name="c-inserting-data-that-is-not-in-the-same-order-as-the-table-columns"></a>C. Einfügen von Daten, deren Reihenfolge nicht mit der Reihenfolge der Tabellenspalten übereinstimmt  
 Im folgenden Beispiel wird eine Spaltenliste verwendet, um die in jede Spalte einzufügenden Werte explizit anzugeben. Die Spaltenreihenfolge in der Tabelle `Production.UnitMeasure` der Datenbank [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] lautet `UnitMeasureCode`, `Name`, `ModifiedDate`. Die Spalten sind jedoch nicht in dieser Reihenfolge in *column_list* aufgelistet.  
  
```sql
INSERT INTO Production.UnitMeasure (Name, UnitMeasureCode,  
    ModifiedDate)  
VALUES (N'Square Yards', N'Y2', GETDATE());  
```  
  
###  <a name="handling-column-values"></a><a name="ColumnValues"></a> Behandeln von Spaltenwerten  
 In den Beispielen in diesem Abschnitt werden Methoden zum Einfügen von Werten in Spalten erläutert, die mit einer IDENTITY-Eigenschaft und einem DEFAULT-Wert oder mit Datentypen wie **uniqueidentifier** oder Spalten eines benutzerdefinierten Typs definiert werden.  
  
#### <a name="d-inserting-data-into-a-table-with-columns-that-have-default-values"></a>D: Einfügen von Daten in eine Tabelle mit Spalten, die Standardwerte enthalten  
 Im folgenden Beispiel wird das Einfügen von Zeilen in eine Tabelle mit Spalten gezeigt, die automatisch einen Wert generieren oder einen Standardwert haben. `Column_1` ist eine berechnete Spalte, die automatisch einen Wert generiert, indem eine Zeichenfolge mit dem in `column_2` eingefügten Wert verkettet wird. `Column_2` wird als Standardeinschränkung definiert. Wenn für diese Spalte kein Wert angegeben ist, wird der Standardwert verwendet. `Column_3` ist mit dem Datentyp **rowversion** definiert, der automatisch eine eindeutige, inkrementelle Binärzahl generiert. `Column_4` generiert nicht automatisch einen Wert. Wird für diese Spalte kein Wert definiert, wird NULL eingefügt. Die INSERT-Anweisungen fügen Zeilen ein, die Werte für einige (aber nicht alle) Spalten enthalten. In der letzten INSERT-Anweisung werden keine Spalten angegeben, und nur die Standardwerte werden mithilfe der DEFAULT VALUES-Klausel eingefügt.  
  
```sql
CREATE TABLE dbo.T1   
(  
    column_1 AS 'Computed column ' + column_2,   
    column_2 varchar(30)   
        CONSTRAINT default_name DEFAULT ('my column default'),  
    column_3 rowversion,  
    column_4 varchar(40) NULL  
);  
GO  
INSERT INTO dbo.T1 (column_4)   
    VALUES ('Explicit value');  
INSERT INTO dbo.T1 (column_2, column_4)   
    VALUES ('Explicit value', 'Explicit value');  
INSERT INTO dbo.T1 (column_2)   
    VALUES ('Explicit value');  
INSERT INTO T1 DEFAULT VALUES;   
GO  
SELECT column_1, column_2, column_3, column_4  
FROM dbo.T1;  
GO  
```  
  
#### <a name="e-inserting-data-into-a-table-with-an-identity-column"></a>E. Einfügen von Daten in eine Tabelle mit einer Identitätsspalte  
 Im folgenden Beispiel werden verschiedene Methoden zum Einfügen von Daten in eine Identitätsspalte gezeigt. Die ersten beiden INSERT-Anweisungen ermöglichen das Generieren von Identitätswerten für die neuen Zeilen. Die dritte INSERT-Anweisung überschreibt die IDENTITY-Eigenschaft für die Spalte mit der SET IDENTITY_INSERT-Anweisung und fügt in die Identitätsspalte einen expliziten Wert ein.  
  
```sql
CREATE TABLE dbo.T1 ( column_1 int IDENTITY, column_2 VARCHAR(30));  
GO  
INSERT T1 VALUES ('Row #1');  
INSERT T1 (column_2) VALUES ('Row #2');  
GO  
SET IDENTITY_INSERT T1 ON;  
GO  
INSERT INTO T1 (column_1,column_2)   
    VALUES (-99, 'Explicit identity value');  
GO  
SELECT column_1, column_2  
FROM T1;  
GO  
```  
  
#### <a name="f-inserting-data-into-a-uniqueidentifier-column-by-using-newid"></a>F. Einfügen von Daten in eine uniqueidentifier-Spalte mithilfe von NEWID()  
 Im folgenden Beispiel wird die Funktion [NEWID()](../../t-sql/functions/newid-transact-sql.md) verwendet, um eine GUID für `column_2` zu erhalten. Im Gegensatz zu Identitätsspalten generiert [!INCLUDE[ssDE](../../includes/ssde-md.md)] nicht automatisch Werte für Spalten mit dem [uniqueidentifier](../../t-sql/data-types/uniqueidentifier-transact-sql.md)-Datentyp, wie durch die zweite `INSERT`-Anweisung gezeigt.  
  
```sql
CREATE TABLE dbo.T1   
(  
    column_1 int IDENTITY,   
    column_2 uniqueidentifier,  
);  
GO  
INSERT INTO dbo.T1 (column_2)   
    VALUES (NEWID());  
INSERT INTO T1 DEFAULT VALUES;   
GO  
SELECT column_1, column_2  
FROM dbo.T1;  
```  
  
#### <a name="g-inserting-data-into-user-defined-type-columns"></a>G. Einfügen von Daten in Spalten eines benutzerdefinierten Typs  
 Mit den folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen werden drei Zeilen in die `PointValue`-Spalte der `Points`-Tabelle eingefügt. Für diese Spalte wird ein [CLR-benutzerdefinierter Typ](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) (UDT) verwendet. Der `Point`-Datentyp besteht aus X- und Y-Ganzzahlwerten, die als Eigenschaften des benutzerdefinierten Typs verfügbar gemacht werden. Verwenden Sie entweder die CAST- oder CONVERT-Funktion, um die durch Kommas getrennten X- und Y-Werte in den `Point`-Typ umzuwandeln. Die ersten beiden Anweisungen verwenden die CONVERT-Funktion, um einen Zeichenfolgenwert in den Typ `Point` zu konvertieren. Die dritte Anweisung verwendet die CAST-Funktion. Weitere Informationen zum Ändern von Daten finden Sie unter [Bearbeiten von UDT-Dateien](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-manipulating-udt-data.md).  
  
```sql
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '3,4'));  
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '1,5'));  
INSERT INTO dbo.Points (PointValue) VALUES (CAST ('1,99' AS Point));  
```  
  
###  <a name="inserting-data-from-other-tables"></a><a name="OtherTables"></a> Einfügen von Daten aus anderen Tabellen  
 Anhand von Beispielen in diesem Abschnitt werden Methoden zum Einfügen von Zeilen aus einer Tabelle in eine andere Tabelle gezeigt.  
  
#### <a name="h-using-the-select-and-execute-options-to-insert-data-from-other-tables"></a>H. Verwenden der SELECT- und EXECUTE-Option zum Einfügen von Daten aus anderen Tabellen  
 Im folgenden Beispiel wird gezeigt, wie Daten mithilfe von INSERT…SELECT oder INSERT…EXECUTE aus einer Tabelle in eine andere Tabelle eingefügt werden. Jede Methode basiert auf einer SELECT-Anweisung mit mehreren Tabellen, die einen Ausdruck und einen Literalwert in der Spaltenliste enthält.  
  
 Bei der ersten INSERT-Anweisung wird eine SELECT-Anweisung verwendet, um die Daten aus den Quelltabellen (`Employee`, `SalesPerson` und `Person`) in der Datenbank [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] abzuleiten und das Resultset in der Tabelle `EmployeeSales` zu speichern. Für die zweite INSERT-Anweisung wird die EXECUTE-Klausel verwendet, um eine gespeicherte Prozedur aufzurufen, die die SELECT-Anweisung enthält, wohingegen für die dritte INSERT-Anweisung mithilfe der EXECUTE-Klausel auf die SELECT-Anweisung als Literalzeichenfolge verwiesen wird.  
  
```sql
CREATE TABLE dbo.EmployeeSales  
( DataSource   varchar(20) NOT NULL,  
  BusinessEntityID   varchar(11) NOT NULL,  
  LastName     varchar(40) NOT NULL,  
  SalesDollars money NOT NULL  
);  
GO  
CREATE PROCEDURE dbo.uspGetEmployeeSales   
AS   
    SET NOCOUNT ON;  
    SELECT 'PROCEDURE', sp.BusinessEntityID, c.LastName,   
        sp.SalesYTD   
    FROM Sales.SalesPerson AS sp    
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY sp.BusinessEntityID, c.LastName;  
GO  
--INSERT...SELECT example  
INSERT INTO dbo.EmployeeSales  
    SELECT 'SELECT', sp.BusinessEntityID, c.LastName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY sp.BusinessEntityID, c.LastName;  
GO  
--INSERT...EXECUTE procedure example  
INSERT INTO dbo.EmployeeSales   
EXECUTE dbo.uspGetEmployeeSales;  
GO  
--INSERT...EXECUTE('string') example  
INSERT INTO dbo.EmployeeSales   
EXECUTE   
('  
SELECT ''EXEC STRING'', sp.BusinessEntityID, c.LastName,   
    sp.SalesYTD   
    FROM Sales.SalesPerson AS sp   
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE ''2%''  
    ORDER BY sp.BusinessEntityID, c.LastName  
');  
GO  
--Show results.  
SELECT DataSource,BusinessEntityID,LastName,SalesDollars  
FROM dbo.EmployeeSales;  
```  
  
#### <a name="i-using-with-common-table-expression-to-define-the-data-inserted"></a>I. Definieren der eingefügten Daten mithilfe von WITH (allgemeiner Tabellenausdruck)  
 Im folgenden Beispiel wird die `NewEmployee`-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank erstellt. Durch einen allgemeinen Tabellenausdruck (`EmployeeTemp`) werden die aus mindestens einer Tabelle in die `NewEmployee`-Tabelle einzufügenden Zeilen definiert. Die INSERT-Anweisung verweist auf die Spalten im allgemeinen Tabellenausdruck.  
  
```sql
CREATE TABLE HumanResources.NewEmployee  
(  
    EmployeeID int NOT NULL,  
    LastName nvarchar(50) NOT NULL,  
    FirstName nvarchar(50) NOT NULL,  
    PhoneNumber Phone NULL,  
    AddressLine1 nvarchar(60) NOT NULL,  
    City nvarchar(30) NOT NULL,  
    State nchar(3) NOT NULL,   
    PostalCode nvarchar(15) NOT NULL,  
    CurrentFlag Flag  
);  
GO  
WITH EmployeeTemp (EmpID, LastName, FirstName, Phone,   
                   Address, City, StateProvince,   
                   PostalCode, CurrentFlag)  
AS (SELECT   
       e.BusinessEntityID, c.LastName, c.FirstName, pp.PhoneNumber,  
       a.AddressLine1, a.City, sp.StateProvinceCode,   
       a.PostalCode, e.CurrentFlag  
    FROM HumanResources.Employee e  
        INNER JOIN Person.BusinessEntityAddress AS bea  
        ON e.BusinessEntityID = bea.BusinessEntityID  
        INNER JOIN Person.Address AS a  
        ON bea.AddressID = a.AddressID  
        INNER JOIN Person.PersonPhone AS pp  
        ON e.BusinessEntityID = pp.BusinessEntityID  
        INNER JOIN Person.StateProvince AS sp  
        ON a.StateProvinceID = sp.StateProvinceID  
        INNER JOIN Person.Person as c  
        ON e.BusinessEntityID = c.BusinessEntityID  
    )  
INSERT INTO HumanResources.NewEmployee   
    SELECT EmpID, LastName, FirstName, Phone,   
           Address, City, StateProvince, PostalCode, CurrentFlag  
    FROM EmployeeTemp;  
GO  
```  
  
#### <a name="j-using-top-to-limit-the-data-inserted-from-the-source-table"></a>J. Beschränken der aus der Quelltabelle eingefügten Daten mit TOP  
 Im folgenden Beispiel wird die `EmployeeSales`-Tabelle erstellt. Anschließend werden der Name und die Verkaufszahlen des laufenden Jahres für die ersten 5 zufälligen Mitarbeiter aus der `HumanResources.Employee`-Tabelle in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank eingefügt. Die INSERT-Anweisung wählt fünf beliebige Zeilen aus, die von der `SELECT`-Anweisung zurückgegeben werden. Mit der OUTPUT-Klausel werden die Zeilen angezeigt, die in die `EmployeeSales`-Tabelle eingefügt werden. Beachten Sie, dass die ersten 5 Mitarbeiter in der SELECT-Anweisung nicht mit der ORDER BY-Klausel ermittelt werden.  
  
```sql
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   nvarchar(11) NOT NULL,  
  LastName     nvarchar(20) NOT NULL,  
  FirstName    nvarchar(20) NOT NULL,  
  YearlySales  money NOT NULL  
 );  
GO  
INSERT TOP(5)INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, 
        inserted.LastName, inserted.YearlySales  
    SELECT sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
```  
  
 Wenn Sie die TOP-Klausel verwenden müssen, um Zeilen in einer sinnvollen Reihenfolge einzufügen, müssen Sie sie zusammen mit einer ORDER BY-Klausel in einer untergeordneten SELECT-Anweisung verwenden, wie im folgenden Beispiel veranschaulicht. Mit der OUTPUT-Klausel werden die Zeilen angezeigt, die in die `EmployeeSales`-Tabelle eingefügt werden. Beachten Sie, dass die ersten 5 Mitarbeiter jetzt anhand der Ergebnisse der ORDER BY-Klausel und nicht anhand zufälliger Zeilen eingefügt werden.  
  
```sql
INSERT INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, 
        inserted.LastName, inserted.YearlySales  
    SELECT TOP (5) sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
```  
  
###  <a name="specifying-target-objects-other-than-standard-tables"></a><a name="TargetObjects"></a> Angeben von Zielobjekten, die keine Standardtabellen sind  
 In den Beispielen in diesem Abschnitt wird veranschaulicht, wie Zeilen durch Angeben einer Sicht oder Tabellenvariablen eingefügt werden.  
  
#### <a name="k-inserting-data-by-specifying-a-view"></a>K. Einfügen von Daten durch Angeben einer Sicht  
 Im folgenden Beispiel wird ein Sichtname als Zielobjekt angegeben. Die neue Zeile wird jedoch in die zugrunde liegende Basistabelle eingefügt. Die Reihenfolge der Werte in der `INSERT`-Anweisung muss mit der Reihenfolge der Spalten in der Sicht übereinstimmen. Weitere Informationen finden Sie unter [Modify Data Through a View](../../relational-databases/views/modify-data-through-a-view.md) (Ändern von Daten über eine Sicht).  
  
```sql
CREATE TABLE T1 ( column_1 int, column_2 varchar(30));  
GO  
CREATE VIEW V1 AS   
SELECT column_2, column_1   
FROM T1;  
GO  
INSERT INTO V1   
    VALUES ('Row 1',1);  
GO  
SELECT column_1, column_2   
FROM T1;  
GO  
SELECT column_1, column_2  
FROM V1;  
GO  
```  
  
#### <a name="l-inserting-data-into-a-table-variable"></a>L. Einfügen von Daten in eine Tabellevariable  
 Im folgenden Beispiel wird eine Tabellenvariable als Zielobjekt in der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank angegeben.  
  
```sql
-- Create the table variable.  
DECLARE @MyTableVar table(  
    LocationID int NOT NULL,  
    CostRate smallmoney NOT NULL,  
    NewCostRate AS CostRate * 1.5,  
    ModifiedDate datetime);  
  
-- Insert values into the table variable.  
INSERT INTO @MyTableVar (LocationID, CostRate, ModifiedDate)  
    SELECT LocationID, CostRate, GETDATE() 
    FROM Production.Location  
    WHERE CostRate > 0;  
  
-- View the table variable result set.  
SELECT * FROM @MyTableVar;  
GO  
```  
  
###  <a name="inserting-rows-into-a-remote-table"></a><a name="RemoteTables"></a> Einfügen von Zeilen in eine Remotetabelle  
 In den Beispielen in diesem Abschnitt wird veranschaulicht, wie Zeilen mit einem [Verbindungsserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) oder einer [Rowsetfunktion](../functions/opendatasource-transact-sql.md) in eine Remotezieltabelle eingefügt werden, um auf die Remotetabelle zu verweisen.  
  
#### <a name="m-inserting-data-into-a-remote-table-by-using-a-linked-server"></a>M. Einfügen von Daten in eine Remotetabelle mithilfe eines Verbindungsservers  
 Im folgenden Beispiel werden Zeilen in eine Remotetabelle eingefügt. In diesem Beispiel wird zunächst mithilfe von [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) ein Link zur Remotedatenquelle erstellt. Der Name des Verbindungsservers (`MyLinkServer`) wird anschließend als Teil des vierteiligen Objektnamens in der Form *server.catalog.schema.object* angegeben.  
  
**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
```sql
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' 
-- or 'server_nameinstance_name'.  
  
EXEC sp_addlinkedserver @server = N'MyLinkServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI',   
    @datasrc = N'server_name',  
    @catalog = N'AdventureWorks2012';  
GO  
```  
  
```sql
-- Specify the remote data source in the FROM clause using a four-part name   
-- in the form linked_server.catalog.schema.object.  
  
INSERT INTO MyLinkServer.AdventureWorks2012.HumanResources.Department (Name, GroupName)  
VALUES (N'Public Relations', N'Executive General and Administration');  
GO  
```  
  
#### <a name="n-inserting-data-into-a-remote-table-by-using-the-openquery-function"></a>N. Einfügen von Daten in eine Remotetabelle mithilfe der OPENQUERY-Funktion  
 Im folgenden Beispiel wird durch Angabe der Rowsetfunktion [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) eine Zeile in eine Remotetabelle eingefügt. Der im vorherigen Beispiel erstellte Name des Verbindungsservers wird hier verwendet.  
  
**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
```sql
INSERT OPENQUERY (MyLinkServer, 
    'SELECT Name, GroupName 
     FROM AdventureWorks2012.HumanResources.Department')  
VALUES ('Environmental Impact', 'Engineering');  
GO  
```  
  
#### <a name="o-inserting-data-into-a-remote-table-by-using-the-opendatasource-function"></a>O. Einfügen von Daten in eine Remotetabelle mithilfe der OPENDATASOURCE-Funktion  
 Im folgenden Beispiel wird durch Angabe der Rowsetfunktion [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) eine Zeile in eine Remotetabelle eingefügt. Geben Sie im Format *server_name* oder *server_name\instance_name* einen gültigen Servernamen für die Datenquelle an.  
  
**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
```sql
-- Use the OPENDATASOURCE function to specify the remote data source.  
-- Specify a valid server name for Data Source using the format 
-- server_name or server_nameinstance_name.  
  
INSERT INTO OPENDATASOURCE('SQLNCLI',  
    'Data Source= <server_name>; Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Department (Name, GroupName)  
    VALUES (N'Standards and Methods', 'Quality Assurance');  
GO  
```  
  
#### <a name="p-inserting-into-an-external-table-created-using-polybase"></a>P. Einfügen in eine externe Tabelle mithilfe von PolyBase  
 Exportieren von Daten aus SQL Server in Hadoop oder Azure Storage Erstellen Sie zuerst eine externe Tabelle, die auf die Zieldatei oder das Verzeichnis verweist. Verwenden Sie dann INSERT INTO zum Exportieren von Daten aus einer lokalen SQL Server-Tabelle in eine externe Datenquelle. Die INSERT INTO-Anweisung erstellt die Zieldatei oder das Verzeichnis, falls nicht vorhanden, und die Ergebnisse der SELECT-Anweisung werden zu einem angegebenen Speicherort im angegebenen Dateiformat exportiert.  Weitere Informationen finden Sie unter [Get started with PolyBase](../../relational-databases/polybase/polybase-guide.md)(Erste Schritte mit PolyBase).  
  
**Gilt für**: [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
```sql
-- Create an external table.   
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] (  
        [FirstName] char(25) NOT NULL,   
        [LastName] char(25) NOT NULL,   
        [YearlyIncome] float NULL,   
        [MaritalStatus] char(1) NOT NULL  
)  
WITH (  
        LOCATION='/old_data/2009/customerdata.tbl',  
        DATA_SOURCE = HadoopHDP2,  
        FILE_FORMAT = TextFileFormat,  
        REJECT_TYPE = VALUE,  
        REJECT_VALUE = 0  
);  
  
-- Export data: Move old data to Hadoop while keeping 
-- it query-able via external table.  

INSERT INTO dbo.FastCustomer2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  
  
###  <a name="bulk-loading-data-from-tables-or-data-files"></a><a name="BulkLoad"></a> Massenladen von Daten aus Tabellen oder Datendateien  
 In den Beispielen in diesem Abschnitt werden zwei Methoden zum Massenladen von Daten in eine Tabelle mithilfe der INSERT-Anweisung vorgestellt.  
  
#### <a name="q-inserting-data-into-a-heap-with-minimal-logging"></a>Q. Einfügen von Daten mit minimaler Protokollierung in einen Heap  
 Im folgenden Beispiel wird eine neue Tabelle (ein Heap) erstellt, und es werden Daten aus einer anderen Tabelle in die neu erstellte Tabelle eingefügt. Dazu wird minimale Protokollierung verwendet. Im Beispiel wird davon ausgegangen, dass das Wiederherstellungsmodell der `AdventureWorks2012`-Datenbank auf FULL festgelegt wird. Zur Sicherstellung, dass die minimale Protokollierung verwendet wird, wird das Wiederherstellungsmodell der `AdventureWorks2012`-Datenbank auf BULK_LOGGED festgelegt, bevor Zeilen eingefügt und nach der INSERT INTO…-SELECT-Anweisung auf FULL zurückgesetzt werden. Außerdem wird der TABLOCK-Hinweis für die `Sales.SalesHistory`-Zieltabelle angegeben. Dadurch wird sichergestellt, dass die Anweisung minimalen Speicherplatz im Transaktionsprotokoll verwendet und effektiv ausgeführt wird.  
  
```sql
-- Create the target heap.  
CREATE TABLE Sales.SalesHistory(  
    SalesOrderID int NOT NULL,  
    SalesOrderDetailID int NOT NULL,  
    CarrierTrackingNumber nvarchar(25) NULL,  
    OrderQty smallint NOT NULL,  
    ProductID int NOT NULL,  
    SpecialOfferID int NOT NULL,  
    UnitPrice money NOT NULL,  
    UnitPriceDiscount money NOT NULL,  
    LineTotal money NOT NULL,  
    rowguid uniqueidentifier ROWGUIDCOL  NOT NULL,  
    ModifiedDate datetime NOT NULL );  
GO  
-- Temporarily set the recovery model to BULK_LOGGED.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY BULK_LOGGED;  
GO  
-- Transfer data from Sales.SalesOrderDetail to Sales.SalesHistory  
INSERT INTO Sales.SalesHistory WITH (TABLOCK)  
    (SalesOrderID,   
     SalesOrderDetailID,  
     CarrierTrackingNumber,   
     OrderQty,   
     ProductID,   
     SpecialOfferID,   
     UnitPrice,   
     UnitPriceDiscount,  
     LineTotal,   
     rowguid,   
     ModifiedDate)  
SELECT * FROM Sales.SalesOrderDetail;  
GO  
-- Reset the recovery model.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY FULL;  
GO  
```  
  
#### <a name="r-using-the-openrowset-function-with-bulk-to-bulk-load-data-into-a-table"></a>R. Verwenden der OPENROWSET-Funktion mit BULK zum Massenladen von Daten in eine Tabelle  
 Im folgenden Beispiel werden durch Angabe der OPENROWSET-Funktion Zeilen aus einer Datendatei in eine Tabelle eingefügt. Der IGNORE_TRIGGERS-Tabellenhinweis wird zur Leistungsoptimierung angegeben. Weitere Beispiele finden Sie unter [Importieren von Massendaten mithilfe von BULK INSERT oder OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).  
  
**Gilt für**:  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] und höher.  
  
```sql
INSERT INTO HumanResources.Department WITH (IGNORE_TRIGGERS) (Name, GroupName)  
SELECT b.Name, b.GroupName   
FROM OPENROWSET (  
    BULK 'C:SQLFilesDepartmentData.txt',  
    FORMATFILE = 'C:SQLFilesBulkloadFormatFile.xml',  
    ROWS_PER_BATCH = 15000)AS b ;  
```  
  
###  <a name="overriding-the-default-behavior-of-the-query-optimizer-by-using-hints"></a><a name="TableHints"></a> Überschreiben des Standardverhaltens des Abfrageoptimierers mithilfe von Hinweisen  
 In den Beispielen in diesem Abschnitt wird gezeigt, wie mit [Tabellenhinweisen](../../t-sql/queries/hints-transact-sql-table.md) beim Verarbeiten der INSERT-Anweisung zeitweise das Standardverhalten des Abfrageoptimierers überschrieben wird.  
  
> [!CAUTION]  
>  Da der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abfrageoptimierer in der Regel den optimalen Ausführungsplan für eine Abfrage auswählt, wird empfohlen, dass erfahrene Entwickler und Datenbankadministratoren Hinweise nur dann verwenden, wenn alle anderen Möglichkeiten sich als unzureichend erwiesen haben.  
  
#### <a name="s-using-the-tablock-hint-to-specify-a-locking-method"></a>S. Verwenden des TABLOCK-Hinweises zum Angeben einer Sperrmethode  
 Im folgenden Beispiel wird angegeben, dass eine exklusive Sperre (X) für die Production.Location-Tabelle eingerichtet und bis zum Ende der INSERT-Anweisung aufrechterhalten wird.  
  
**Gilt für**: [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].  
  
```sql
INSERT INTO Production.Location WITH (XLOCK)  
(Name, CostRate, Availability)  
VALUES ( N'Final Inventory', 15.00, 80.00);  
```  
  
###  <a name="capturing-the-results-of-the-insert-statement"></a><a name="CaptureResults"></a> Erfassen der Ergebnisse der INSERT-Anweisung  
 In den Beispielen in diesem Abschnitt wird gezeigt, wie mit der [OUTPUT-Klausel](../../t-sql/queries/output-clause-transact-sql.md) Informationen aus jeder von einer INSERT-Anweisung betroffenen Zeile bzw. Ausdrücke, die auf jeder Zeile basieren, zurückgegeben werden. Diese Ergebnisse können an die verarbeitende Anwendung zurückgegeben werden, die sie z. B. für Bestätigungen, Archivierungen und andere Anwendungsanforderungen verwendet.  
  
#### <a name="t-using-output-with-an-insert-statement"></a>T. Verwenden von OUTPUT mit einer INSERT-Anweisung  
 Im folgenden Beispiel wird eine Zeile in die `ScrapReason`-Tabelle eingefügt, und die `OUTPUT`-Klausel wird verwendet, um die Ergebnisse der Anweisung an die `@MyTableVar`-Tabellenvariable zurückzugeben. Da die `ScrapReasonID`-Spalte mit einer `IDENTITY`-Eigenschaft definiert ist, wird kein Wert für diese Spalte in der `INSERT`-Anweisung angegeben. Beachten Sie jedoch, dass der von [!INCLUDE[ssDE](../../includes/ssde-md.md)] für diese Spalte generierte Wert in der `OUTPUT`-Klausel in der `INSERTED.ScrapReasonID`-Spalte zurückgegeben wird.  
  
```sql
DECLARE @MyTableVar table( NewScrapReasonID smallint,  
                           Name varchar(50),  
                           ModifiedDate datetime);  
INSERT Production.ScrapReason  
    OUTPUT INSERTED.ScrapReasonID, INSERTED.Name, INSERTED.ModifiedDate  
        INTO @MyTableVar  
VALUES (N'Operator error', GETDATE());  
  
--Display the result set of the table variable.  
SELECT NewScrapReasonID, Name, ModifiedDate FROM @MyTableVar;  
--Display the result set of the table.  
SELECT ScrapReasonID, Name, ModifiedDate   
FROM Production.ScrapReason;  
```  
  
#### <a name="u-using-output-with-identity-and-computed-columns"></a>U. Verwenden von OUTPUT mit Identitätsspalten und berechneten Spalten  
 Im folgenden Beispiel wird die `EmployeeSales`-Tabelle erstellt, und es werden mehrere Zeilen mithilfe einer INSERT-Anweisung mit einer SELECT-Anweisung zum Abrufen der Daten aus den Quelltabellen in die Tabelle eingefügt. Die `EmployeeSales`-Tabelle enthält eine Identitätsspalte (`EmployeeID`) und eine berechnete Spalte (`ProjectedSales`). Da diese Werte während des Einfügevorgangs von [!INCLUDE[ssDE](../../includes/ssde-md.md)] generiert werden, kann keine dieser Spalten in `@MyTableVar` definiert werden.  
  
```sql
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   int IDENTITY (1,5)NOT NULL,  
  LastName     nvarchar(20) NOT NULL,  
  FirstName    nvarchar(20) NOT NULL,  
  CurrentSales money NOT NULL,  
  ProjectedSales AS CurrentSales * 1.10   
);  
GO  
DECLARE @MyTableVar table(  
  LastName     nvarchar(20) NOT NULL,  
  FirstName    nvarchar(20) NOT NULL,  
  CurrentSales money NOT NULL  
  );  
  
INSERT INTO dbo.EmployeeSales (LastName, FirstName, CurrentSales)  
  OUTPUT INSERTED.LastName,   
         INSERTED.FirstName,   
         INSERTED.CurrentSales  
  INTO @MyTableVar  
    SELECT c.LastName, c.FirstName, sp.SalesYTD  
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY c.LastName, c.FirstName;  
  
SELECT LastName, FirstName, CurrentSales  
FROM @MyTableVar;  
GO  
SELECT EmployeeID, LastName, FirstName, CurrentSales, ProjectedSales  
FROM dbo.EmployeeSales;  
```  
  
#### <a name="v-inserting-data-returned-from-an-output-clause"></a>V. Einfügen von Daten, die von einer OUTPUT-Klausel zurückgegeben wurden  
 Im folgenden Beispiel werden aus der OUTPUT-Klausel einer MERGE-Anweisung zurückgegebene Daten erfasst und in eine andere Tabelle eingefügt. Die MERGE-Anweisung aktualisiert die `Quantity`-Spalte der `ProductInventory`-Tabelle täglich auf der Grundlage der Bestellungen, die in der `SalesOrderDetail`-Tabelle der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]-Datenbank verarbeitet werden. Außerdem werden die Zeilen für Produkte gelöscht, deren Bestand auf 0 (null) fällt. Das Beispiel erfasst die gelöschten Zeilen und fügt sie in einer anderen Tabelle (`ZeroInventory`) ein, in der Produkte ohne Bestand gespeichert werden.  
  
```sql
--Create ZeroInventory table.  
CREATE TABLE Production.ZeroInventory (DeletedProductID int, RemovedOnDate DateTime);  
GO  
  
INSERT INTO Production.ZeroInventory (DeletedProductID, RemovedOnDate)  
SELECT ProductID, GETDATE()  
FROM  
(   MERGE Production.ProductInventory AS pi  
    USING (SELECT ProductID, SUM(OrderQty) FROM Sales.SalesOrderDetail AS sod  
           JOIN Sales.SalesOrderHeader AS soh  
           ON sod.SalesOrderID = soh.SalesOrderID  
           AND soh.OrderDate = '20070401'  
           GROUP BY ProductID) AS src (ProductID, OrderQty)  
    ON (pi.ProductID = src.ProductID)  
    WHEN MATCHED AND pi.Quantity - src.OrderQty <= 0  
        THEN DELETE  
    WHEN MATCHED  
        THEN UPDATE SET pi.Quantity = pi.Quantity - src.OrderQty  
    OUTPUT $action, deleted.ProductID) AS Changes (Action, ProductID)  
WHERE Action = 'DELETE';  
IF @@ROWCOUNT = 0  
PRINT 'Warning: No rows were inserted';  
GO  
SELECT DeletedProductID, RemovedOnDate FROM Production.ZeroInventory;  
```  

#### <a name="w-inserting-data-using-the-select-option"></a>W. Einfügen von Daten mit der SELECT-Option  
 Im folgenden Beispiel wird gezeigt, wie mehrere Datenzeilen mithilfe einer INSERT-Anweisung mit einer SELECT-Option eingefügt werden. Die erste `INSERT`-Anweisung verwendet eine `SELECT`-Anweisung direkt, um Daten aus der Quelltabelle abzurufen und dann das Resultset in der Tabelle `EmployeeTitles` zu speichern.  
  
```sql
CREATE TABLE EmployeeTitles  
( EmployeeKey   INT NOT NULL,  
  LastName     varchar(40) NOT NULL,  
  Title      varchar(50) NOT NULL  
);  
INSERT INTO EmployeeTitles  
    SELECT EmployeeKey, LastName, Title   
    FROM ssawPDW.dbo.DimEmployee  
    WHERE EndDate IS NULL;  
```  
  
#### <a name="x-specifying-a-label-with-the-insert-statement"></a>X. Angeben einer Bezeichnung mit der INSERT-Anweisung  
 Im folgenden Beispiel wird die Verwendung einer Bezeichnung mit einer INSERT-Anweisung gezeigt.  
  
```sql
-- Uses AdventureWorks  
  
INSERT INTO DimCurrency   
VALUES (500, N'C1', N'Currency1')  
OPTION ( LABEL = N'label1' );  
```  
  
#### <a name="y-using-a-label-and-a-query-hint-with-the-insert-statement"></a>Y. Verwenden einer Bezeichnung und eines Abfragehinweises mit der Anweisung INSERT  
 Diese Abfrage zeigt die grundlegende Syntax für die Verwendung einer Bezeichnung und eines Join-Abfragehinweises mit der INSERT-Anweisung. Nachdem die Abfrage an den Steuerungsknoten übermittelt wurde, wird die Hashjoinstrategie angewendet, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (wird auf den Computeknoten ausgeführt) den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abfrageplan generiert. Weitere Informationen zu Joinhinweisen und der Verwendung der OPTION-Klausel finden Sie unter [OPTION (SQL Server PDW)](../../t-sql/queries/option-clause-transact-sql.md).  
  
```sql
-- Uses AdventureWorks  
  
INSERT INTO DimCustomer (CustomerKey, CustomerAlternateKey, 
    FirstName, MiddleName, LastName )   
SELECT ProspectiveBuyerKey, ProspectAlternateKey, 
    FirstName, MiddleName, LastName  
FROM ProspectiveBuyer p JOIN DimGeography g ON p.PostalCode = g.PostalCode  
WHERE g.CountryRegionCode = 'FR'  
OPTION ( LABEL = 'Add French Prospects', HASH JOIN);  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [IDENTITY &#40;Eigenschaft&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [NEWID &#40;Transact-SQL&#41;](../../t-sql/functions/newid-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)   
 [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)   
 [OUTPUT-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md)   
 [Verwenden der Tabellen inserted und deleted](../../relational-databases/triggers/use-the-inserted-and-deleted-tables.md)  
  
