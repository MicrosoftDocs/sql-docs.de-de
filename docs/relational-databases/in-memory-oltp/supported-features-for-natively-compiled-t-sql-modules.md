---
title: Features für nativ kompilierte T-SQL-Module
description: Hier erfahren Sie mehr über die T-SQL-Oberfläche und die unterstützten Features von nativ kompilierten T-SQL-Modulen, z. B. gespeicherte Prozeduren und benutzerdefinierte Skalarfunktionen.
ms.custom: seo-dt-2019
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 05515013-28b5-4ccf-9a54-ae861448945b
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 011a7d3fa710699f75afd4ca835ba1f082a956b5
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100353613"
---
# <a name="supported-features-for-natively-compiled-t-sql-modules"></a>Unterstützte Funktionen für nativ kompilierte T-SQL-Module
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


  Dieses Thema listet die T-SQL-Oberflächenbereiche und unterstützten Funktionen im Hauptteil nativ kompilierter T-SQL-Module auf, wie gespeicherte Prozeduren ([CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)), benutzerdefinierte Skalarfunktionen, Inline-Tabellenwertfunktionen und Trigger.  

 Unterstützte Funktionen in der Definition der nativen Module finden Sie unter [Unterstützte Konstrukte für systemintern kompilierte gespeicherte Prozeduren](../../relational-databases/in-memory-oltp/supported-ddl-for-natively-compiled-t-sql-modules.md).  

 Vollständige Informationen zu nicht unterstützten Konstrukten sowie Informationen zu Umgehungslösungen zu einigen der nicht unterstützten Funktionen in nativ kompilierten Modulen finden Sie unter [Migration Issues for Natively Compiled Stored Procedures](./a-guide-to-query-processing-for-memory-optimized-tables.md)(Migrationsprobleme bei nativ kompilierten gespeicherten Prozeduren). Weitere Informationen zu nicht unterstützten Funktionen finden Sie unter [Von In-Memory-OLTP nicht unterstützte Transact-SQL-Konstrukte](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md).  

##  <a name="query-surface-area-in-native-modules"></a><a name="qsancsp"></a> Abfrageoberfläche in nativen Modulen  

Die folgenden Abfragekonstrukte werden unterstützt:  

CASE-Ausdruck: CASE kann in einer beliebigen Anweisung oder Klausel verwendet werden, die einen gültigen Ausdruck zulässt.
   - **Gilt für:** [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]  
    Ab [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] werden CASE-Anweisungen für nativ kompilierte T-SQL-Module unterstützt.

SELECT-Klausel:  

-   Aliase für Spalten und Namen (entweder mithilfe von AS oder = Syntax).  

-   Skalare Unterabfragen
    - **Gilt für:** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]
      Ab [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] werden skalare Unterabfragen für nativ kompilierte T-SQL-Module unterstützt.

-   TOP*  

-   SELECT DISTINCT  
    - **Gilt für:** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]
      Ab [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] CTP 1.1 wird der DISTINCT-Operator in nativ kompilierten Modulen unterstützt.

        - DISTINCT-Aggregate werden nicht unterstützt.  

-   UNION und UNION ALL
    - **Gilt für:** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]
      Ab [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] CTP 1.1 werden die Operatoren UNION und UNION ALL in nativ kompilierten Modulen unterstützt.

-   Variablenzuweisungen  

FROM-Klausel:  

-   FROM \<memory optimized table or table variable>  

-   FROM \<natively compiled inline TVF>  

-   LEFT OUTER JOIN, RIGHT OUTER JOIN, CROSS JOIN und INNER JOIN.
    - **Gilt für:** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]
      Ab [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] werden JOINS für nativ kompilierte T-SQL-Module unterstützt.

-   Unterabfragen `[AS] table_alias`. Weitere Informationen finden Sie unter [FROM &#40;Transact-SQL &#41;](../../t-sql/queries/from-transact-sql.md). 
    - **Gilt für:** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]
      Ab [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] werden Unterabfragen für nativ kompilierte T-SQL-Module unterstützt.

WHERE-Klausel:  

-   Filterprädikat IS [NOT] NULL  

-   AND, BETWEEN  
-   OR, NOT, IN, EXISTS
    - **Gilt für:** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]
      Ab [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] werden die Operatoren OR, NOT, IN und EXISTS für nativ kompilierte T-SQL-Module unterstützt.


[GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md) -Klausel:

- Aggregatfunktionen AVG, COUNT, COUNT_BIG, MIN, MAX, und SUM.  

- MIN und MAX werden für die Typen nvarchar, char, varchar, varchar, vabinary und binary nicht unterstützt.  

[ORDER BY](../../t-sql/queries/select-order-by-clause-transact-sql.md) -Klausel:


- Es gibt keine Unterstützung für **DISTINCT** in der **ORDER BY** -Klausel.


- Wird mit [GROUP BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-group-by-transact-sql.md) unterstützt, wenn ein Ausdruck in der ORDER BY-Liste wörtlich in der GROUP BY-Liste angezeigt wird.
  - Beispielsweise wird GROUP BY a + b ORDER BY a + b unterstützt, aber GROUP BY a, b ORDER BY a + b nicht.  


HAVING-Klausel:

- Unterliegt den gleichen Ausdruckseinschränkungen wie die WHERE-Klausel.


#### <a name="order-by-and-top-are-supported-in-natively-compiled-modules-with-some-restrictions"></a>ORDER BY und TOP werden in nativ kompilierten Modulen mit einigen Einschränkungen unterstützt.


- Es gibt keine Unterstützung für **WITH TIES** oder **PERCENT** in der **TOP** -Klausel.


- Es gibt keine Unterstützung für **DISTINCT** in der **ORDER BY** -Klausel.  


- **TOP** in Kombination mit **ORDER BY** unterstützt höchstens den Wert 8.192 bei Verwendung einer Konstante in der **TOP** -Klausel.
  - Dieser Grenzwert kann herabgesetzt werden, wenn die Abfrage Joins oder Aggregatfunktionen enthält. (Beispielsweise liegt die Beschränkung bei einem Join mit zwei Tabellen bei 4.096 Zeilen. Bei zwei Joins mit drei Tabellen lautet der Grenzwert 2.730 Zeilen).  
  - Sie können Ergebnisse erhalten, die größer als 8.192 sind, indem Sie die Anzahl von Zeilen in einer Variablen speichern:  

```sql
DECLARE @v INT = 9000;
SELECT TOP (@v) ... FROM ... ORDER BY ...
```

Eine Konstante in der **TOP** -Klausel führt jedoch im Vergleich zur Verwendung einer Variablen zu einer besseren Leistung.  

Diese Einschränkungen für nativ kompilierte [!INCLUDE[tsql](../../includes/tsql-md.md)] gelten nicht für den interpretierten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Zugriff auf speicheroptimierte Tabellen.  


##  <a name="data-modification"></a><a name="dml"></a> Datenänderung  

Die folgenden DML-Anweisungen werden unterstützt.  

-   INSERT VALUES (eine Zeile pro Anweisung) und INSERT... SELECT  

-   UPDATE  

-   Delete  

-   WHERE wird zusammen mit UPDATE- und DELETE-Anweisungen unterstützt.  

##  <a name="control-of-flow-language"></a><a name="cof"></a> Sprachkonstrukte zur Ablaufsteuerung  
 Die folgenden Sprachkonstrukte zur Ablaufsteuerung werden unterstützt.  

-   [IF...ELSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/if-else-transact-sql.md)  

-   [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)  

-   [RETURN &#40;Transact-SQL&#41;](../../t-sql/language-elements/return-transact-sql.md)  

-   [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md) kann alle [unterstützten Datentypen für In-Memory OLTP](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md) sowie speicheroptimierte Tabellentypen verwenden. Variablen können als NULL oder NOT NULL deklariert werden.  

-   [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  

-   [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)  

    - Sie können einen einzelnen TRY/CATCH-Block für ein ganzes nativ kompiliertes T-SQL-Modul verwenden, um die optimale Leistung zu erzielen.  

-   [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)  

-   BEGIN ATOMIC (auf der äußeren Ebene der gespeicherten Prozedur). Weitere Informationen finden Sie unter [ATOMIC-Blöcke](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).  

##  <a name="supported-operators"></a><a name="so"></a> Unterstützte Operatoren  
 Die folgenden Operatoren werden unterstützt.  

-   [Vergleichsoperatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md) (z. B. >, \<, >=, und <=)  

-   Unäre Operatoren (+, -)  

-   Binäre Operatoren (*, /, +, -, % (Modulo)).  

    - Der Plusoperator (+) wird in Zahlen und Zeichenfolgen unterstützt.  

-   Logische Operatoren (AND, OR, NOT).  

-   Bitweise Operatoren ~, &, |, und ^  

-   APPLY-Operator
    - **Gilt für:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]  
      Ab [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] wird der APPLY-Operator in nativ kompilierten Modulen unterstützt.

##  <a name="built-in-functions-in-natively-compiled-modules"></a><a name="bfncsp"></a> Integrierte Funktionen in nativ kompilierten Modulen  
 Die folgenden Funktionen werden in Einschränkungen in speicheroptimierten Tabellen und in nativ kompilierten T-SQL-Modulen unterstützt.  

-   [Mathematische Funktionen &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md) (alle)  

-   Datumsfunktionen: CURRENT_TIMESTAMP, DATEADD, DATEDIFF, DATEFROMPARTS, DATEPART, DATETIME2FROMPARTS, DATETIMEFROMPARTS, DAY, EOMONTH, GETDATE, GETUTCDATE, MONTH, SMALLDATETIMEFROMPARTS, SYSDATETIME, SYSUTCDATETIME und YEAR.  

-   Zeichenfolgenfunktionen: LEN, LTRIM, RTRIM und SUBSTRING.  
    - **Gilt für:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]  
      Ab [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] werden die folgenden integrierten Funktionen ebenfalls unterstützt: TRIM, TRANSLATE und CONCAT_WS.  

-   Identitätsfunktionen: SCOPE_IDENTITY  

-   NULL-Funktionen: ISNULL  

-   Uniqueidentifier-Funktionen: NEWID und NEWSEQUENTIALID  

-   JSON-Funktionen  
    - **Gilt für:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]  
      Ab [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] werden die JSON-Funktionen in nativ kompilierten Modulen unterstützt.

-   Fehlerfunktionen: ERROR_LINE, ERROR_MESSAGE, ERROR_NUMBER, ERROR_PROCEDURE, ERROR_SEVERITY und ERROR_STATE  

-   Systemfunktionen: @@rowcount. Durch Anweisungen in nativ kompilierten gespeicherten Prozeduren wird @@rowcount aktualisiert, und Sie können @@rowcount in einer nativ kompilierten gespeicherten Prozedur verwenden, um die Anzahl der Zeilen zu bestimmen, die von der letzten Anweisung betroffen sind, die innerhalb der nativ kompilierten gespeicherten Prozedur ausgeführt wurde. Allerdings wird @@rowcount am Anfang und am Ende der Ausführung einer nativ kompilierten gespeicherten Prozedur auf 0 zurückgesetzt.  

-   Sicherheitsfunktionen: IS_MEMBER({'Gruppe' | 'Rolle'}), IS_ROLEMEMBER ('Rolle' [, 'database_principal']), IS_SRVROLEMEMBER ('Rolle' [, 'Anmeldename']), ORIGINAL_LOGIN(), SESSION_USER, CURRENT_USER, SUSER_ID(['Anmeldename']), SUSER_SID(['Anmeldename'] [, Param2]), SUSER_SNAME([server_user_sid]), SYSTEM_USER, SUSER_NAME, USER, USER_ID(['Benutzer']), USER_NAME([id]), CONTEXT_INFO().

-   Die Ausführungen nativer Module können geschachtelt werden.

##  <a name="auditing"></a><a name="auditing"></a> Überwachen  
 Überwachung auf Prozedurebene wird für systemintern kompilierte gespeicherte Prozeduren unterstützt.  

 Weitere Informationen zur Überwachung finden Sie unter [Erstellen einer Serverüberwachung und Datenbanküberwachungsspezifikation](../../relational-databases/security/auditing/create-a-server-audit-and-database-audit-specification.md).  

##  <a name="table-and-query-hints"></a><a name="tqh"></a> Tabellen- und Abfragehinweise  
 Folgende werden unterstützt:  

-   INDEX-, FORCESCAN- und FORCESEEK-Hinweise, entweder in der Tabellenhinweissyntax oder in der [OPTION-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md) der Abfrage. Weitere Informationen finden Sie unter [Tabellenhinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  

-   FORCE ORDER  

-   LOOP JOIN-Hinweis  

-   OPTIMIZE FOR  

 Weitere Informationen finden Sie unter [Abfragehinweise &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  

##  <a name="limitations-on-sorting"></a><a name="los"></a> Einschränkungen bei der Sortierung  
 Sie können mehr als 8.000 Zeilen in einer Abfrage sortieren, die [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md) und eine [ORDER BY-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md) verwendet. Ohne die [ORDER BY-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md) kann [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md) eine Sortierung von bis zu 8.000 Zeilen durchführen (weniger Zeilen, falls es Verknüpfungen gibt).  

 Wenn die Abfrage jeweils den Operator [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md) und eine [ORDER BY-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md) verwendet, können Sie bis zu 8192 Zeilen für den TOP-Operator angeben. Wenn Sie mehr als 8192 Zeilen angeben, wird die folgende Fehlermeldung angezeigt: **Nachricht 41398, Ebene 16, Zustand 1, Prozedur *\<procedureName>* , Zeile *\<lineNumber>* der TOP-Operator kann maximal 8192 Zeilen zurückgeben; *\<number>* wurde angefordert.**  

 Wenn keine TOP-Klausel vorhanden ist, kann eine beliebige Anzahl von Zeilen mit ORDER BY sortiert werden.  

 Wenn keine ORDER BY-Klausel verwendet wird, können Sie jeden ganzzahligen Wert mit dem TOP-Operator verwenden.  

 Beispiel mit TOP N = 8192: Wird kompiliert  

```sql  
CREATE PROCEDURE testTop  
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION  
  AS  
  BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
    SELECT TOP 8192 ShoppingCartId, CreatedDate, TotalPrice FROM dbo.ShoppingCart  
    ORDER BY ShoppingCartId DESC  
  END;  
GO  
```

 Beispiel mit TOP N > 8192: Kann nicht kompiliert werden.  

```sql  
CREATE PROCEDURE testTop  
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION  
  AS  
  BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
    SELECT TOP 8193 ShoppingCartId, CreatedDate, TotalPrice FROM dbo.ShoppingCart  
    ORDER BY ShoppingCartId DESC  
  END;  
GO  
```

 Die Einschränkung auf 8192 Zeilen gilt nur für `TOP N` , wobei `N` wie in den Beispielen oben eine Konstante ist.  Wenn `N` größer als 8192 sein muss, können Sie den Wert einer Variablen zuweisen und die Variable mit `TOP`verwenden.  

 Beispiel mit einer Variablen: Wird kompiliert  

```sql  
CREATE PROCEDURE testTop  
WITH EXECUTE AS OWNER, SCHEMABINDING, NATIVE_COMPILATION  
  AS  
  BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
    DECLARE @v int = 8193   
    SELECT TOP (@v) ShoppingCartId, CreatedDate, TotalPrice FROM dbo.ShoppingCart  
    ORDER BY ShoppingCartId DESC  
  END;  
GO  
```

 **Einschränkungen für die zurückgegebenen Zeilen:** Es gibt zwei Fälle, in denen die Anzahl von Zeilen, die vom TOP-Operator zurückgegeben werden kann, verringert wird:  

-   Verwenden von JOINs in der Abfrage  Die Auswirkungen von JOINs auf die Einschränkung sind vom Abfrageplan abhängig.  

-   Verwenden von Aggregatfunktionen oder Verweisen auf Aggregatfunktionen in der ORDER BY-Klausel  

 Die Formel zum Berechnen eines im ungünstigsten Fall unterstützten Maximalwerts für N in TOP N lautet wie folgt: `N = floor ( 65536 / number_of_tables * 8 + total_size+of+aggs )`.  

## <a name="see-also"></a>Weitere Informationen  
 [Nativ kompilierte gespeicherte Prozeduren](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [Migrationsprobleme bei nativ kompilierten gespeicherten Prozeduren](./a-guide-to-query-processing-for-memory-optimized-tables.md)