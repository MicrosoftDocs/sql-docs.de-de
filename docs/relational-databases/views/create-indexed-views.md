---
description: Erstellen von indizierten Sichten
title: Erstellen von indizierten Sichten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/19/2018
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexed views [SQL Server], creating
- clustered indexes, views
- CREATE INDEX statement
- large_value_types_out_of_row option
- indexed views [SQL Server]
- views [SQL Server], indexed views
ms.assetid: f86dd29f-52dd-44a9-91ac-1eb305c1ca8d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 148f93b43f704686b3083954cb3d7353f33a16e0
ms.sourcegitcommit: c6cc0b669b175ae290cf5b08952010661ebd03c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2021
ms.locfileid: "100530860"
---
# <a name="create-indexed-views"></a>Erstellen von indizierten Sichten

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

In diesem Artikel wird beschrieben, wie Sie Indizes für eine Sicht erstellen. Der erste Index, der für eine Sicht erstellt wird, muss ein eindeutiger gruppierter Index sein. Nachdem der eindeutige gruppierte Index erstellt wurde, können Sie weitere nicht gruppierte Indizes erstellen. Das Erstellen eines eindeutigen gruppierten Indexes für eine Sicht verbessert die Abfrageleistung, da die Sicht wie eine Tabelle mit einem gruppierten Index in der Datenbank gespeichert wird. Der Abfrageoptimierer kann indizierte Sichten verwenden, um die Abfrageausführung zu beschleunigen. Es ist nicht erforderlich, dass in der Abfrage auf die jeweilige Sicht verwiesen wird, damit der Optimierer diese Sicht als Ersatz berücksichtigt.

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen

Die folgenden Schritte sind zum Erstellen einer indizierten Sicht erforderlich und wichtig für eine erfolgreiche Implementierung der indizierten Sicht:

1. Stellen Sie sicher, dass die SET-Optionen für alle vorhandenen Tabellen korrekt sind, auf die in der Sicht verwiesen wird.
2. Stellen Sie sicher, dass die SET-Optionen für die Sitzung richtig festgelegt sind, bevor Sie Tabellen und die Sicht erstellen.
3. Stellen Sie sicher, dass die Sichtdefinition deterministisch ist.
4. Vergewissern Sie sich, dass die Basistabelle denselben Besitzer wie die Ansicht hat.
5. Erstellen Sie die Sicht mithilfe der Option `WITH SCHEMABINDING`.
6. Erstellen Sie den eindeutigen gruppierten Index für die Sicht.

> [!IMPORTANT]
> Wenn Sie DML<sup>1</sup> für eine Tabelle ausführen, auf die durch eine große Anzahl von indizierten Sichten oder durch wenige, jedoch sehr komplexe indizierte Sichten verwiesen wird, müssen diese indizierten Sichten ebenfalls aktualisiert werden. Als Folge daraus kann die DML-Abfrageleistung erheblich beeinträchtigt werden. In einigen Fällen kann kein Abfrageplan erstellt werden.
> Testen Sie Ihre DML-Abfragen in solchen Fällen, bevor Sie diese für die Produktion verwenden, analysieren Sie den Abfrageplan, und optimieren bzw. vereinfachen Sie die DML-Anweisung.
>
> <sup>1</sup> Beispielsweise UPDATE-, DELETE- oder INSERT-Vorgänge

### <a name="required-set-options-for-indexed-views"></a><a name="Restrictions"></a> Erforderliche SET-Optionen für indizierte Sichten

Das Auswerten desselben Ausdrucks kann im [!INCLUDE[ssDE](../../includes/ssde-md.md)] zu unterschiedlichen Ergebnissen führen, wenn bei der Ausführung der Abfrage unterschiedliche SET-Optionen aktiviert sind. Wenn die SET-Option `CONCAT_NULL_YIELDS_NULL` auf ON festgelegt ist, gibt beispielsweise der Ausdruck `'abc' + NULL` den Wert `NULL` zurück. Wenn die Option `CONCAT_NULL_YIELDS_NULL` allerdings auf OFF festgelegt ist, ergibt derselbe Ausdruck `'abc'`.

Um sicherzustellen, dass die Sichten ordnungsgemäß verwaltet werden können und konsistente Ergebnisse zurückgeben, sind für indizierte Sichten feste Werte für mehrere SET-Optionen erforderlich. Die SET-Optionen in der folgenden Tabelle müssen auf die in der Spalte **Erforderlicher Wert** angezeigten Werte festgelegt werden, wenn eine der folgenden Bedingungen zutrifft:

- Die Sicht und nachfolgende Indizes für die Sicht werden erstellt.
- Die Basistabellen, auf die beim Erstellen der Ansicht in dieser verwiesen wird.
- Für eine Tabelle, die Teil der indizierten Sicht ist, wird ein Einfüge-, Update- oder Löschvorgang durchgeführt. Dazu gehören Vorgänge wie Massenkopieren, Replikation und verteilte Abfragen.
- Die indizierte Sicht wird vom Abfrageoptimierer verwendet, um den Abfrageplan zu erstellen.

|SET-Optionen|Erforderlicher Wert|Standardserverwert|Standard<br /><br /> OLE DB- und ODBC-Wert|Standard<br /><br /> DB-Library-Wert|
|-----------------|--------------------|--------------------------|---------------------------------------|-----------------------------------|
|ANSI_NULLS|EIN|EIN|EIN|OFF|
|ANSI_PADDING|EIN|EIN|EIN|OFF|
|ANSI_WARNINGS<sup>1</sup>|EIN|EIN|EIN|OFF|
|ARITHABORT|EIN|EIN|OFF|OFF|
|CONCAT_NULL_YIELDS_NULL|EIN|EIN|EIN|OFF|
|NUMERIC_ROUNDABORT|OFF|OFF|OFF|OFF|
|QUOTED_IDENTIFIER|EIN|EIN|EIN|OFF|
|&nbsp;|&nbsp;|&nbsp;|&nbsp;|&nbsp;|

<sup>1</sup> Durch das Festlegen von `ANSI_WARNINGS` auf ON wird `ARITHABORT` implizit auf ON festgelegt.

Wenn Sie eine OLE DB- oder ODBC-Serververbindung verwenden, müssen Sie nur den Wert der `ARITHABORT`-Einstellung ändern. Alle DB-Library-Werte müssen entweder auf Serverebene mithilfe von **sp_configure** oder über die Anwendung mithilfe des SET-Befehls ordnungsgemäß festgelegt werden.

> [!IMPORTANT]
> Es wird dringend empfohlen, die Benutzeroption `ARITHABORT` serverweit auf ON festzulegen, sobald in einer Datenbank auf dem Server die erste indizierte Sicht oder der erste Index für eine berechnete Spalte erstellt wird.

### <a name="deterministic-views"></a>Deterministische Sichten

Die Definition einer indizierten Sicht muss deterministisch sein. Eine Sicht ist deterministisch, wenn alle Ausdrücke in der Auswahlliste sowie die `WHERE`-Klausel und die `GROUP BY`-Klausel deterministisch sind. Deterministische Ausdrücke geben stets dasselbe Ergebnis zurück, wenn sie mit einer bestimmten Gruppe von Eingabewerten ausgewertet werden. Nur deterministische Funktionen können Teil von deterministischen Ausdrücken sein. Beispielsweise ist die `DATEADD`-Funktion deterministisch, weil sie für eine bestimmte Gruppe von Argumentwerten immer das gleiche Ergebnis für die drei Parameter zurückgibt. `GETDATE` ist nicht deterministisch, da diese Funktion immer mit dem gleichen Argument aufgerufen wird, der zurückgegebene Wert jedoch bei jeder Ausführung unterschiedlich ist.

Um zu bestimmen, ob eine Sichtspalte deterministisch ist, verwenden Sie die **IsDeterministic** -Eigenschaft der [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md) -Funktion. Mithilfe der **IsPrecise**-Eigenschaft der `COLUMNPROPERTY`-Funktion können Sie bestimmen, ob eine deterministische Spalte in einer Sicht mit Schemabindung präzise ist. `COLUMNPROPERTY` gibt den Wert 1 für TRUE, den Wert 0 für FALSE und NULL für ungültige Eingaben zurück. Dies bedeutet, dass die Spalte nicht deterministisch oder nicht präzise ist.

Auch wenn ein Ausdruck deterministisch ist, kann das exakte Ergebnis von der Prozessorarchitektur oder der Version des Microcodes abhängen, wenn dieser Ausdruck float-Ausdrücke enthält. Um die Datenintegrität sicherzustellen, können solche Ausdrücke nur als Nichtschlüsselspalten von indizierten Sichten verwendet werden. Deterministische Ausdrücke, die keine float-Ausdrücke enthalten, werden als präzise bezeichnet. Nur präzise deterministische Ausdrücke können in indizierten Sichten Teile von Schlüsselspalten und `WHERE`- oder `GROUP BY`-Klauseln sein.

### <a name="additional-requirements"></a>Zusätzliche Anforderungen

Zusätzlich zu den Anforderungen bzgl. SET-Optionen und deterministischen Funktionen müssen die folgenden Anforderungen erfüllt werden:

- Der Benutzer, der die `CREATE INDEX`-Anweisung ausführt, muss der Besitzer der Sicht sein.
- Wenn Sie den Index erstellen, muss die Option `IGNORE_DUP_KEY` auf OFF (Standardeinstellung) festgelegt sein.
- Auf Tabellen muss in der Sichtdefinition mit dem zweiteiligen Namen _Schema_ **.** _Tabellenname_ verwiesen werden.
- Benutzerdefinierte Funktionen, auf die in der Sicht verwiesen wird, müssen mit der Option `WITH SCHEMABINDING` erstellt werden.
- Auf benutzerdefinierte Funktionen, auf die in der Sicht verwiesen wird, muss mit zweiteiligen Namen verwiesen werden: _\<schema\>_ **.** _\<function\>_ .
- Die Datenzugriffseigenschaft einer benutzerdefinierten Funktion muss `NO SQL` lauten, und die Eigenschaft für den externen Zugriff muss `NO` lauten.
- CLR-Funktionen (Common Language Runtime) können in der SELECT-Liste der Sicht angezeigt werden, können aber nicht Teil der Definition des gruppierten Indexschlüssels sein. CLR-Funktionen können nicht in der WHERE-Klausel der Sicht oder in der ON-Klausel einer JOIN-Operation in der Sicht auftreten.
- Für CLR-Funktionen und -Methoden der CLR-benutzerdefinierten Typen, die in der Sichtdefinition verwendet werden, müssen die in der folgenden Tabelle dargestellten Eigenschaften festgelegt werden.

   |Eigenschaft|Hinweis|
   |--------------|----------|
   |DETERMINISTIC = TRUE|Muss explizit als ein Attribut der Microsoft .NET Framework-Methode deklariert werden.|
   |PRECISE = TRUE|Muss explizit als ein Attribut der .NET Framework-Methode deklariert werden.|
   |DATA ACCESS = NO SQL|Wird durch Festlegen des DataAccess-Attributs auf DataAccessKind.None und des SystemDataAccess-Attributs auf SystemDataAccessKind.None bestimmt.|
   |EXTERNAL ACCESS = NO|Diese Eigenschaft ist für CLR-Routinen standardmäßig auf NO festgelegt.|
   |&nbsp;|&nbsp;|

- Die Sicht muss mithilfe der Option `WITH SCHEMABINDING` erstellt werden.
- Die Sicht darf nur auf Basistabellen in derselben Datenbank wie die Sicht verweisen. Die Sicht kann nicht auf andere Sichten verweisen.

- Wenn `GROUP BY` vorhanden ist, muss die VIEW-Definition `COUNT_BIG(*)` enthalten, während `HAVING` nicht enthalten sein darf. Diese `GROUP BY`-Einschränkungen gelten nur für die indizierte Sichtdefinition. Im Ausführungsplan einer Abfrage kann eine indizierte Sicht auch dann verwendet werden, wenn sie diese `GROUP BY`-Einschränkungen nicht erfüllt.
- Wenn die Sichtdefinition eine `GROUP BY`-Klausel enthält, kann der Schlüssel des eindeutigen gruppierten Indexes nur auf die Spalten verweisen, die in der `GROUP BY`-Klausel angegeben werden.

- Die SELECT-Anweisung in der Sichtdefinition darf die folgenden Transact-SQL-Elemente nicht enthalten:

   | Transact-SQL-Elemente | (Fortsetzung) | (Fortsetzung) |
   | --------------------- | ----------- | ----------- |
   |`COUNT`|ROWSET-Funktionen (`OPENDATASOURCE`, `OPENQUERY`, `OPENROWSET` und `OPENXML`)|`OUTER`-Joins (`LEFT`, `RIGHT` oder `FULL`)|
   |Abgeleitete Tabelle (durch Angabe einer `SELECT`-Anweisung in der `FROM`-Klausel definiert)|Selbstjoins|Angeben von Spalten mithilfe von `SELECT *` oder `SELECT <table_name>.*`|
   |`DISTINCT`|`STDEV`, `STDEVP`, `VAR`, `VARP` oder `AVG`|Allgemeine Tabellenausdrücke (CTE, Common Table Expression)|
   |**float**<sup>1</sup>-, **text**-, **ntext**-, **image**-, **XML**- oder **filestream**-Spalten|Unterabfrage|Die `OVER`-Klausel, die Fensterrangfunktionen oder Fensteraggregatfunktionen enthält|
   |Volltextprädikate (`CONTAINS`, `FREETEXT`)|Eine `SUM`-Funktion, die auf einen Ausdruck verweist, der NULL-Werte zulässt|`ORDER BY`|
   |CLR-benutzerdefinierte Aggregatfunktion|`TOP`|`CUBE`-, `ROLLUP`- oder `GROUPING SETS`-Operatoren|
   |`MIN`, `MAX`|`UNION`-, `EXCEPT`- oder `INTERSECT`-Operatoren|`TABLESAMPLE`|
   |Tabellenvariablen|`OUTER APPLY` oder `CROSS APPLY`|`PIVOT`, `UNPIVOT`|
   |Spaltensätze mit geringer Dichte|Inline-Tabellenwertfunktionen (TVF) oder Tabellenwertfunktionen mit mehreren Anweisungen (MSTVF)|`OFFSET`|
   |`CHECKSUM_AGG`|||

   <sup>1</sup> Die indizierte Sicht kann Spalten mit dem Datentyp **float** enthalten. Allerdings dürfen solche Spalten nicht im Schlüssel des gruppierten Indexes enthalten sein.

   > [!IMPORTANT]
   > Indizierte Sichten, die temporale Abfragen (Abfragen, die die `FOR SYSTEM_TIME`-Klausel verwenden) überlagern, werden nicht unterstützt.

### <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen

Wenn Sie in indizierten Sichten auf **datetime** - und **smalldatetime** -Zeichenfolgenliterale verweisen, wird empfohlen, das Literal mithilfe eines deterministischen Datenformats explizit in den gewünschten Datentyp zu konvertieren. Eine Liste der deterministischen Datenformatstile finden Sie unter [CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md). Weitere Informationen zu deterministischen und nicht deterministischen Ausdrücken finden Sie im Abschnitt [Weitere Überlegungen](#nondeterministic) auf dieser Seite.

Wenn Sie DML (z.B. `UPDATE`, `DELETE` oder `INSERT`) für eine Tabelle ausführen, auf die durch eine große Anzahl von indizierten Sichten oder durch wenige, jedoch sehr komplexe indizierte Sichten verwiesen wird, müssen diese indizierten Sichten während der DML-Ausführung ebenfalls aktualisiert werden. Als Folge daraus kann die DML-Abfrageleistung erheblich beeinträchtigt werden. In einigen Fällen kann kein Abfrageplan erstellt werden. Testen Sie Ihre DML-Abfragen in solchen Fällen, bevor Sie diese für die Produktion verwenden, analysieren Sie den Abfrageplan, und optimieren bzw. vereinfachen Sie die DML-Anweisung.

### <a name="considerations"></a><a name="Considerations"></a> Weitere Überlegungen

Die Einstellung der Option **large_value_types_out_of_row** der Spalten in einer indizierten Sicht wird von der Einstellung für die entsprechende Spalte in der Basistabelle vererbt. Dieser Wert wird mithilfe von [sp_tableoption](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)festgelegt. Die Standardeinstellung für Spalten, die auf Grundlage von Ausdrücken erstellt werden, ist 0. Das bedeutet, dass umfangreiche Werte innerhalb der Zeile gespeichert werden.

Indizierte Sichten können für eine partitionierte Tabelle erstellt werden und selbst partitioniert werden.

Wenn Sie verhindern möchten, dass [!INCLUDE[ssDE](../../includes/ssde-md.md)] indizierte Sichten verwendet, schließen Sie den `OPTION (EXPAND VIEWS)`-Hinweis in die Abfrage ein. Außerdem kann der Optimierer die Indizes für die Sichten nicht verwenden, wenn eine der aufgeführten Optionen falsch festgelegt ist. Weitere Informationen zum `OPTION (EXPAND VIEWS)`-Hinweis finden Sie unter [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md).

Wenn eine Sicht gelöscht wird, werden alle Indizes für diese Sicht gelöscht. Wird der gruppierte Index einer Sicht gelöscht, werden alle nicht gruppierten Indizes und alle automatisch erstellten Statistiken der Sicht gelöscht. Vom Benutzer erstellte Statistiken zur Sicht werden beibehalten. Nicht gruppierte Indizes können einzeln gelöscht werden. Durch das Löschen des gruppierten Index der Sicht wird das gespeicherte Resultset entfernt, und der Optimierer verarbeitet die Sicht von nun an wieder wie eine Standardsicht.

Indizes für Tabellen und Sichten können deaktiviert werden. Wenn ein gruppierter Index für eine Tabelle deaktiviert wird, werden Indizes für die den Tabellen zugeordneten Sichten auch deaktiviert.

<a name="nondeterministic"></a> Ausdrücke, die eine implizite Konvertierung von Zeichenfolgen in **datetime** oder **smalldatetime** umfassen, werden als nicht deterministisch angesehen. Weitere Informationen finden Sie unter [Nicht deterministische Konvertierung von Datumsliteralzeichenfolgen in DATE-Werte](../../t-sql/data-types/nondeterministic-convert-date-literals.md).

### <a name="security"></a><a name="Security"></a> Sicherheit

#### <a name="permissions"></a><a name="Permissions"></a> Berechtigungen

Zum Erstellen der Sicht muss ein Benutzer die **CREATE VIEW**-Berechtigung in der Datenbank und die **ALTER**-Berechtigung in dem Schema verwenden, in dem die Sicht erstellt wird. Wenn sich die Basistabelle in einem anderen Schema befindet, ist mindestens die **REFERENCES**-Berechtigung für die Tabelle erforderlich. Wenn sich der Benutzer, der den Index erstellt, von den Benutzern unterscheidet, die die Sicht erstellt haben, ist allein für die Indexerstellung die **ALTER**-Berechtigung für die Sicht erforderlich (durch ALTER für das Schema abgedeckt).

    > [!NOTE]  
    > Indexes can only be created on views which have the same owner as the referenced table or tables. This is also called an intact **ownership-chain** between the view and the table(s). Typically, when table and view reside within the same schema, the same schema-owner applies to all objects within the schema. But it is possible that individual objects have different explicit owners. The column **principal_id** in sys.tables contains a value if the owner is different from the schema-owner.


## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL

### <a name="to-create-an-indexed-view"></a>So erstellen Sie eine indizierte Sicht

Im folgenden Beispiel werden eine Sicht und ein Index für diese Sicht erstellt. Dies beinhaltet zwei Abfragen, in denen die indizierte Sicht in der AdventureWorks-Datenbank verwendet wird.

```sql
--Set the options to support indexed views.
SET NUMERIC_ROUNDABORT OFF;
SET ANSI_PADDING, ANSI_WARNINGS, CONCAT_NULL_YIELDS_NULL, ARITHABORT,
   QUOTED_IDENTIFIER, ANSI_NULLS ON;
--Create view with schemabinding.
IF OBJECT_ID ('Sales.vOrders', 'view') IS NOT NULL
   DROP VIEW Sales.vOrders ;
GO
CREATE VIEW Sales.vOrders
   WITH SCHEMABINDING
   AS  
      SELECT SUM(UnitPrice*OrderQty*(1.00-UnitPriceDiscount)) AS Revenue,
         OrderDate, ProductID, COUNT_BIG(*) AS COUNT
      FROM Sales.SalesOrderDetail AS od, Sales.SalesOrderHeader AS o
      WHERE od.SalesOrderID = o.SalesOrderID
      GROUP BY OrderDate, ProductID;
GO
--Create an index on the view.
CREATE UNIQUE CLUSTERED INDEX IDX_V1
   ON Sales.vOrders (OrderDate, ProductID);
GO
--This query can use the indexed view even though the view is
--not specified in the FROM clause.
SELECT SUM(UnitPrice*OrderQty*(1.00-UnitPriceDiscount)) AS Rev,
   OrderDate, ProductID
FROM Sales.SalesOrderDetail AS od
JOIN Sales.SalesOrderHeader AS o
   ON od.SalesOrderID=o.SalesOrderID
      AND ProductID BETWEEN 700 and 800
      AND OrderDate >= CONVERT(datetime,'05/01/2002',101)
   GROUP BY OrderDate, ProductID
   ORDER BY Rev DESC;
GO
--This query can use the above indexed view.
SELECT OrderDate, SUM(UnitPrice*OrderQty*(1.00-UnitPriceDiscount)) AS Rev
FROM Sales.SalesOrderDetail AS od
JOIN Sales.SalesOrderHeader AS o
   ON od.SalesOrderID=o.SalesOrderID
      AND DATEPART(mm,OrderDate)= 3
      AND DATEPART(yy,OrderDate) = 2002
    GROUP BY OrderDate
    ORDER BY OrderDate ASC;
```

Weitere Informationen finden Sie unter [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).

## <a name="see-also"></a>Weitere Informationen

- [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)
- [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md)
- [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md)
- [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md)
- [SET ARITHABORT](../../t-sql/statements/set-arithabort-transact-sql.md)
- [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)
- [SET NUMERIC_ROUNDABORT](../../t-sql/statements/set-numeric-roundabort-transact-sql.md)
- [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md)  
