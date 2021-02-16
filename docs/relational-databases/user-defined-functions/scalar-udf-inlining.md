---
title: Inlining benutzerdefinierter Skalarfunktionen in Microsoft SQL Server | Microsoft-Dokumentation
description: In diesem Artikel wird das Inlining benutzerdefinierter Skalarfunktionen erläutert, um die Leistung von Abfragen zu verbessern, die benutzerdefinierte Skalarfunktionen in SQL Server (2019 und höher) aufrufen.
ms.custom: ''
ms.date: 08/04/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords: ''
ms.assetid: ''
author: s-r-k
ms.author: karam
monikerRange: = azuresqldb-current || >= sql-server-ver15
ms.openlocfilehash: 5e2dd566b0c5842636619c8331bfc88378dc4f84
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350859"
---
# <a name="scalar-udf-inlining"></a>Inlining benutzerdefinierter Skalarfunktionen

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Dieser Artikel stellt eine Einführung in das Inlining benutzerdefinierter Skalarfunktionen dar. Dabei handelt es sich um ein Feature für die [intelligente Abfrageverarbeitung](../../relational-databases/performance/intelligent-query-processing.md). Durch dieses Feature wird die Leistung von Abfragen verbessert, die benutzerdefinierte Skalarfunktionen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[sssql19](../../includes/sssql19-md.md)]) aufrufen.

## <a name="t-sql-scalar-user-defined-functions"></a>Benutzerdefinierte T-SQL-Skalarfunktionen
Benutzerdefinierte Funktionen (User-defined functions, UDF), die in [!INCLUDE[tsql](../../includes/tsql-md.md)] implementiert werden und einen einzelnen Datenwert zurückgeben, werden als benutzerdefinierte T-SQL-Skalarfunktionen bezeichnet. Benutzerdefinierte T-SQL-Funktionen sind gut geeignet, um Code in [!INCLUDE[tsql](../../includes/tsql-md.md)]-Abfragen wiederzuverwenden und Modularität zu erreichen. Einige Berechnungen (z.B. komplexe Geschäftsregeln) können einfacher in imperativer UDF-Form ausgedrückt werden. Mit benutzerdefinierten Funktionen können Sie eine komplexe Logik erstellen, ohne komplexe SQL-Abfragen schreiben zu können. Weitere Informationen zu benutzerdefinierten Funktionen (UDFs) finden Sie unter [Erstellen von benutzerdefinierten Funktionen (Datenbank-Engine)](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).

## <a name="performance-of-scalar-udfs"></a>Leistung von benutzerdefinierten Skalarfunktionen
Die Leistung von benutzerdefinierten Skalarfunktionen ist aus folgenden Gründen häufig niedrig:

- **Iterativer Aufruf:** Benutzerdefinierte Funktionen werden einmal pro qualifiziertem Tupel iterativ aufgerufen. Durch diese Funktionsaufrufe entstehen zusätzliche Kosten für den Kontextwechsel. Dies betrifft besonders benutzerdefinierte Funktionen, die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Abfragen in ihrer Definition ausführen.

- **Keine Kostenberechnung:** Während der Optimierung fallen nur Kosten für relationale Operatoren an, nicht für Skalaroperatoren. Vor der Einführung benutzerdefinierter Skalarfunktionen waren andere Skalaroperatoren in der Regel günstig, sodass keine Kostenberechnung nötig war. Es war ausreichend, geringe CPU-Kosten für einen Skalarvorgang hinzuzufügen. In einigen Szenarios sind die tatsächlichen Kosten jedoch wichtig und werden dennoch nicht ausreichend repräsentiert.

- **Interpretierte Ausführung:** Benutzerdefinierte Funktionen werden als Anweisungsbatch ausgewertet und Anweisung für Anweisung ausgeführt. Jede Anweisung wird kompiliert, und der kompilierte Plan wird zwischengespeichert. Die Zwischenspeicherung spart zwar Zeit, da Neukompilierungen vermieden werden, aber jede Anweisung wird isoliert ausgeführt. Es können keine anweisungsübergreifenden Optimierungen durchgeführt werden.

- **Serielle Ausführung:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lässt in Abfragen, die benutzerdefinierte Funktionen aufrufen, keine abfrageinterne Parallelität zu. 

## <a name="automatic-inlining-of-scalar-udfs"></a>Automatisches Inlining von benutzerdefinierrten Skalarfunktionen
Mit dem Feature für das Inlining benutzerdefinierter Skalarfunktionen soll die Leistung von Abfragen verbessert werden, die benutzerdefinierte T-SQL-Skalarfunktionen ausführen, bei denen die Ausführung der benutzerdefinierten Funktion den entscheidenden Engpass darstellt.

Mit diesem neuen Feature werden benutzerdefinierte Skalarfunktionen automatisch in Skalarausdrücke oder skalare Unterabfragen transformiert, die in der aufrufenden Abfrage den UDF-Operator ersetzen. Diese Ausdrücke und Unterabfragen werden anschließend optimiert. Dadurch enthält der Abfrageplan keinen UDF-Operator mehr. Seine Auswirkungen werden im Plan überwacht, z.B. Ansichten oder Inline-Tabellenwertfunktionen.

### <a name="example-1---single-statement-scalar-udf"></a>1\. Beispiel: Benutzerdefinierte Skalarfunktion mit einer einzelnen Anweisung
Sehen Sie sich die folgende Abfrage an:

```sql
SELECT L_SHIPDATE, O_SHIPPRIORITY, SUM (L_EXTENDEDPRICE *(1 - L_DISCOUNT)) 
FROM LINEITEM
INNER JOIN ORDERS
  ON O_ORDERKEY = L_ORDERKEY 
GROUP BY L_SHIPDATE, O_SHIPPRIORITY ORDER BY L_SHIPDATE;
```

Diese Abfrage berechnet die Summe der reduzierten Preise für Einzelposten und gruppiert die Ergebnisse nach Versanddatum und Versandpriorität. Der Ausdruck `L_EXTENDEDPRICE *(1 - L_DISCOUNT)` enthält die Formel für den reduzierten Preis eines bestimmten Einzelpostens. Solche Formeln können in Funktionen extrahiert werden, um Modularität und Wiederverwendbarkeit zu erreichen.

```sql
CREATE FUNCTION dbo.discount_price(@price DECIMAL(12,2), @discount DECIMAL(12,2)) 
RETURNS DECIMAL (12,2) AS
BEGIN
  RETURN @price * (1 - @discount);
END
```

Die Abfrage kann nun geändert werden, um diese benutzerdefinierte Funktion aufzurufen.

```sql
SELECT L_SHIPDATE, O_SHIPPRIORITY, SUM (dbo.discount_price(L_EXTENDEDPRICE, L_DISCOUNT)) 
FROM LINEITEM
INNER JOIN ORDERS
  ON O_ORDERKEY = L_ORDERKEY 
GROUP BY L_SHIPDATE, O_SHIPPRIORITY ORDER BY L_SHIPDATE
```

Wie zuvor beschrieben ist die Leistung der Abfrage mit der benutzerdefinierten Funktion niedrig. Durch das Inlining der benutzerdefinierten Skalarfunktion wird der Skalarausdruck im Textkörper der benutzerdefinierten Funktion direkt in der Abfrage ersetzt. Die Ergebnisse dieser Abfrage werden in folgender Tabelle aufgeführt:

| Abfrage: | Abfrage ohne benutzerdefinierte Funktion | Abfrage mit benutzerdefinierter Funktion (ohne Inlining) | Abfrage mit Inlining benutzerdefinierter Skalarfunktionen | 
| --- | --- | --- | --- |
| Ausführungszeit: | 1,6 Sekunden | 29 Minuten, 11 Sekunden | 1,6 Sekunden |

Diese Zahlen basieren auf einer CCI-Datenbank mit 10 GB (mit dem TPC-H-Schema), die auf einem Computer mit Dualprozessor (12 Kerne), 96 GB RAM und SSD-Sicherung ausgeführt wird. Dabei wurden die Kompilier- und die Ausführungszeit mit einem kalten Prozedurcache und Pufferpool eingeschlossen. Die Standardkonfiguration wurde verwendet, und es wurden keine weiteren Indizes erstellt.

### <a name="example-2---multi-statement-scalar-udf"></a>2\. Beispiel: Benutzerdefinierte Skalarfunktion mit einer mehreren Anweisungen
Für benutzerdefinierte Skalarfunktionen, die über mehrere T-SQL-Anweisungen implementiert werden, z.B. Variablenzuweisungen und bedingte Verzweigungen, kann ebenfalls ein Inlining durchgeführt werden. Sehen Sie sich folgende benutzerdefinierte Skalarfunktion an, die die Servicekategorie für einen bestimmten Kunden bestimmt, wenn ein benutzerdefinierter Schlüssel vorhanden ist. Die Kategorie wird erreicht, indem zuerst der Gesamtpreis aller Bestellungen des Kunden mithilfe einer SQL-Abfrage berechnet wird. Anschließend wird eine `IF (...) ELSE`-Logik verwendet, um die Kategorie auf Grundlage des Gesamtpreises zu bestimmen.

```sql
CREATE OR ALTER FUNCTION dbo.customer_category(@ckey INT) 
RETURNS CHAR(10) AS
BEGIN
  DECLARE @total_price DECIMAL(18,2);
  DECLARE @category CHAR(10);

  SELECT @total_price = SUM(O_TOTALPRICE) FROM ORDERS WHERE O_CUSTKEY = @ckey;

  IF @total_price < 500000
    SET @category = 'REGULAR';
  ELSE IF @total_price < 1000000
    SET @category = 'GOLD';
  ELSE 
    SET @category = 'PLATINUM';

  RETURN @category;
END
```

Sehen Sie sich nun eine Abfrage an, die diese benutzerdefinierte Funktion aufruft.

```sql
SELECT C_NAME, dbo.customer_category(C_CUSTKEY) FROM CUSTOMER;
```

Der Ausführungsplan für diese Abfrage in [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] (Kompatibilitätsgrad 140 und früher) sieht folgendermaßen aus:

![Abfrageplan ohne Inlining](./media/query-plan-without-udf-inlining.png)

Der Plan zeigt, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in diesem Fall eine einfache Strategie übernimmt: Für jedes Tupel in der `CUSTOMER`-Tabelle werden die benutzerdefinierte Funktion aufgerufen und die Ergebnisse ausgegeben. Diese Strategie ist jedoch zu einfach und nicht effizient. Durch Inlining werden solche benutzerdefinierten Funktionen in gleichwertige skalare Unterabfragen transformiert, die die benutzerdefinierte Funktion in der aufrufenden Abfrage ersetzen.

Der Plan mit Inlining der benutzerdefinierten Funktion sieht für die gleiche Abfrage folgendermaßen aus.

![Abfrageplan mit Inlining](./media/query-plan-with-udf-inlining.png)

Wie zuvor erwähnt enthält der Abfrageplan keinen UDF-Operator mehr. Seine Auswirkungen sind nun im Plan ersichtlich, z.B. Ansichten oder Inline-Tabellenwertfunktionen. Hier sind die wichtigsten Beobachtungen, die aus dem obigen Plan resultieren:

-  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hat den impliziten Join zwischen `CUSTOMER` und `ORDERS` abgeleitet und diesen über einen Joinoperator in einen expliziten transformiert.
-  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hat außerdem die implizite `GROUP BY O_CUSTKEY on ORDERS`-Anweisung abgeleitet und „IndexSpool + StreamAggregate“ verwendet, um diese zu implementieren.
-  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet nun einen Parallelismus für alle Operatoren.

Je nach Komplexität der Logik in der benutzerdefinierten Funktion kann der resultierende Abfrageplan größer und komplexer werden. Wie Sie sehen können, handelt es sich bei den Vorgängen in der benutzerdefinierten Funktion nicht mehr um einen undurchsichtigen Behälter. Deshalb kann der Abfrageoptimierer diese Vorgänge optimieren und deren Kosten berücksichtigen. Da die benutzerdefinierte Funktion sich nicht mehr im Plan befindet, wird der iterative Aufruf derselben durch einen Plan ersetzt, der den Aufwand, der durch Funktionsaufrufe entsteht, vollständig vermeidet.

## <a name="inlineable-scalar-udfs-requirements"></a>Anforderungen für inlinefähige benutzerdefinierte Skalarfunktionen
<a name="requirements"></a> Für eine benutzerdefinierte T-SQL-Skalarfunktion kann ein Inlining durchgeführt werden, wenn alle der folgenden Bedingungen erfüllt sind:

- Die benutzerdefinierte Funktion wurde mit folgenden Konstrukten geschrieben:
    - `DECLARE`, `SET`: Variablendeklaration und -zuweisungen
    - `SELECT`: SQL-Abfrage mit einer bzw. mehreren Variablenzuweisungen<sup>1</sup>.
    - `IF`/`ELSE`: Verzweigung mit beliebigen Schachtelungsebenen
    - `RETURN`: eine oder mehrere RETURN-Anweisungen
    - `UDF`: Geschachtelte bzw. rekursive Funktionsaufrufe<sup>2</sup>.
    - Sonstige: relationale Vorgänge wie `EXISTS`, `ISNULL`
- Die benutzerdefinierte Funktion ruft keine intrinsische Funktion auf, die zeitabhängig ist (wie `GETDATE()`) oder Nebeneffekte<sup>3</sup> hat (wie `NEWSEQUENTIALID()`).
- Die benutzerdefinierte Funktion verwendet die `EXECUTE AS CALLER`-Klausel (Standardverhalten, wenn die `EXECUTE AS`-Klausel nicht angegeben wurde).
- Die benutzerdefinierte Funktion verweist nicht auf Tabellenvariablen oder Tabellenwertparameter.
- Die Abfrage, die eine benutzerdefinierte Skalarfunktion aufruft, verweist in der `GROUP BY`-Klausel nicht auf einen Aufruf einer benutzerdefinierten Skalarfunktion.
- Die Abfrage, die eine benutzerdefinierte Skalarfunktion in der Auswahlliste mit der `DISTINCT`-Klausel aufruft, verfügt nicht über eine `ORDER BY`-Klausel.
- Die benutzerdefinierte Funktion wird in der `ORDER BY`-Klausel nicht verwendet.
- Die benutzerdefinierte Funktion wird nicht nativ kompiliert (Interop wird unterstützt).
- Die benutzerdefinierte Funktion wird nicht in einer berechneten Spalte oder in der Definition einer CHECK-Einschränkung verwendet.
- Die benutzerdefinierte Funktion verweist nicht auf benutzerdefinierte Typen.
- Zur benutzerdefinierten Funktion werden keine Signaturen hinzugefügt.
- Bei der benutzerdefinierten Funktion handelt es sich nicht um eine Partitionsfunktion.
- Die benutzerdefinierte Funktion enthält keine Verweise auf allgemeine Tabellenausdrücke (Common Table Expressions, CTEs).
- Die benutzerdefinierte Funktion enthält keine Verweise auf intrinsische Funktionen, die die Ergebnisse ändern können, wenn ein Inlinevorgang ausgeführt wird (z. B. `@@ROWCOUNT`) <sup>4</sup>.
- Die benutzerdefinierte Funktion enthält keine Aggregatfunktionen, die als Parameter an eine Skalar-UDF übergeben werden <sup>4</sup>.
- Die benutzerdefinierte Funktion verweist nicht auf integrierte Sichten (z. B. `OBJECT_ID`) <sup>4</sup>.
- Die benutzerdefinierte Funktion verweist nicht auf XML-Methoden <sup>5</sup>.
- Die benutzerdefinierte Funktion enthält keine SELECT-Abfrage mit `ORDER BY` ohne `TOP 1`-Klausel <sup>5</sup>.
- Die benutzerdefinierte Funktion enthält keine SELECT-Abfrage, die eine Zuweisung in Verbindung mit der `ORDER BY`-Klausel durchführt (z. B. `SELECT @x = @x + 1 FROM table1 ORDER BY col1`) <sup>5</sup>.
- Die benutzerdefinierte Funktion enthält keine mehreren RETURN-Anweisungen <sup>6</sup>.
- Die benutzerdefinierte Funktion wird nicht aus einer RETURN-Anweisung aufgerufen <sup>6</sup>.
- Die benutzerdefinierte Funktion verweist nicht auf die `STRING_AGG`-Funktion <sup>6</sup>. 
- Die benutzerdefinierte Funktion verweist nicht auf Remotetabellen <sup>7</sup>.
- Die Abfrage, die die benutzerdefinierte Funktion aufruft, verwendet `GROUPING SETS`, `CUBE` oder `ROLLUP` nicht <sup>7</sup>.
- Die Abfrage, die die benutzerdefinierte Funktion aufruft, enthält keine Variable, die als Zuweisungsparameter für die benutzerdefinierte Funktion verwendet wird (z. B. `SELECT @y = 2` und `@x = UDF(@y)`) <sup>7</sup>.

<sup>1</sup> Für `SELECT` mit einer Variablenakkumulation/-aggregation (z. B. `SELECT @val += col1 FROM table1`) wird kein Inlining unterstützt.

<sup>2</sup> Für rekursive benutzerdefinierte Funktionen wird das Inlining nur bis zu einem bestimmten Grad durchgeführt.

<sup>3</sup> Intrinsische Funktionen, deren Ergebnisse von der aktuellen Systemzeit abhängen, sind zeitabhängig. Ein Beispiel für eine Funktion mit Nebeneffekten ist eine intrinsische Funktion, die einen internen globalen Status aktualisieren kann. Solche Funktionen geben bei jedem Aufruf unterschiedliche Ergebnisse auf Grundlage des internen Status zurück.

<sup>4</sup> Einschränkung hinzugefügt in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU2

<sup>5</sup> Einschränkung hinzugefügt in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU4

<sup>6</sup> Einschränkung hinzugefügt in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU5

<sup>7</sup> Einschränkung hinzugefügt in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU6

> [!NOTE]
> Informationen zu den neuesten T-SQL-Inliningfixes für Skalar-UDFs und Änderungen für Inliningberechtigungsszenarios finden Sie im Knowledge Base-Artikel [FIX: Inliningprobleme bei benutzerdefinierten Skalarfunktionen in SQL Server 2019](https://support.microsoft.com/help/4538581).

### <a name="checking-whether-or-not-a-udf-can-be-inlined"></a>So überprüfen Sie, ob ein Inlining einer benutzerdefinierten Funktion möglich ist
Für jede benutzerdefinierte T-SQL-Skalarfunktion enthält die Katalogansicht [sys.sql_modules](../system-catalog-views/sys-sql-modules-transact-sql.md) eine Eigenschaft namens `is_inlineable`, die angibt, ob für eine benutzerdefinierte Funktion ein Inlining möglich ist. 

> [!NOTE]
> Die `is_inlineable`-Eigenschaft wird von den Konstrukten abgeleitet, die in der Definition der benutzerdefinierten Skalarfunktion gefunden werden. Es wird nicht überprüft, ob für die benutzerdefinierte Funktion zum Zeitpunkt der Kompilierung tatsächlich ein Inlining möglich ist. Weitere Informationen finden Sie im Abschnitt zu den [Bedingungen für Inlining](#requirements).

Der Wert 1 gibt an, dass ein Inlining möglich ist, während der Wert 0 angibt, dass kein Inlining möglich ist. Diese Eigenschaft enthält für alle Inline-Tabellenwertfunktionen den Wert 1. Für alle anderen Module ist der Wert 0.

Wenn ein Inlining für eine benutzerdefinierte Skalarfunktion möglich ist, bedeutet das nicht, dass immer ein Inlining durchgeführt wird. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entscheidet (pro Abfrage und pro benutzerdefinierter Funktion), ob für eine benutzerdefinierte Funktion ein Inlining durchgeführt wird. Einige Beispiele, in denen für eine benutzerdefinierte Funktion möglicherweise kein Inlining durchgeführt werden kann, finden Sie im Folgenden:

-  Wenn die Definition der benutzerdefinierten Funktion beispielsweise zu Tausenden Codezeilen führt, führt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für diese möglicherweise kein Inlining durch. 
-  Wenn eine benutzerdefinierte Funktion in einer `GROUP BY`-Klausel aufgerufen wird, wird kein Inlining durchgeführt. Diese Entscheidung erfolgt, wenn die Abfrage, die auf eine benutzerdefinierte Skalarfunktion verweist, kompiliert wird.
-  Die benutzerdefinierte Funktion ist mit einem Zertifikat signiert. Signaturen können nach dem Erstellen einer benutzerdefinierte Funktion hinzugefügt und gelöscht werden können. Deshalb wird bei der Kompilierung der Abfrage, die auf eine benutzerdefinierte Skalarfunktion verweist, entschieden, ob für diese ein Inlining durchgeführt werden soll oder nicht. Systemfunktionen sind z. B. in der Regel mit einem Zertifikat signiert. Sie können [sys.crypt_properties](../../relational-databases/system-catalog-views/sys-crypt-properties-transact-sql.md) verwenden, um zu ermitteln, welche Objekte signiert sind. 

   ```sql
   SELECT * 
   FROM sys.crypt_properties AS cp
   INNER JOIN sys.objects AS o ON cp.major_id = o.object_id;
   ```

### <a name="checking-whether-inlining-has-happened-or-not"></a>So überprüfen Sie, ob ein Inlining durchgeführt wurde
Wenn alle Bedingungen erfüllt sind und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ein Inlining durchführt, wird die benutzerdefinierte Funktion in einen relationalen Ausdruck transformiert. Über den Abfrageplan können Sie einfach feststellen, ob ein Inlining durchgeführt wurde:

- Die XML des Plans enthält keinen `<UserDefinedFunction>`-XML-Knoten für eine benutzerdefinierte Funktion, für die erfolgreich ein Inlining durchführt wurde. 
- Bestimmte erweiterte Events (XEvents) werden ausgegeben.

## <a name="enabling-scalar-udf-inlining"></a>Aktivieren des Inlinings benutzerdefinierter Skalarfunktionen
Sie können Workloads automatisch für das Inlining benutzerdefinierter Skalarfunktionen zulassen, indem Sie den Kompatibilitätsgrad 150 für die Datenbank aktivieren. Diesen können Sie mit [!INCLUDE[tsql](../../includes/tsql-md.md)] festlegen. Beispiel:  

```sql
ALTER DATABASE [WideWorldImportersDW] SET COMPATIBILITY_LEVEL = 150;
```

Es müssen keine weiteren Änderungen vorgenommen, damit dieses Feature für benutzerdefinierte Funktionen oder Abfragen verwendet werden kann.

## <a name="disabling-scalar-udf-inlining-without-changing-the-compatibility-level"></a>Deaktivieren des Inlinings von benutzerdefinierten Skalarfunktionen ohne Änderung des Kompatibilitätsgrads
Das Inlining von benutzerdefinierten Skalarfunktionen kann im Datenbank- oder Anweisungs-, oder UDF-Bereich deaktiviert werden, während der Datenbankkompatibilitätsgrad weiterhin bei 150 und höher bleibt. Führen Sie folgende Anweisung im Kontext einer Datenbank aus, um das Inlining für benutzerdefinierte Skalarfunktionen im Datenbankbereich zu deaktivieren: 

```sql
ALTER DATABASE SCOPED CONFIGURATION SET TSQL_SCALAR_UDF_INLINING = OFF;
```

Führen Sie folgende Anweisung im Kontext einer Datenbank aus, um das Inlining für benutzerdefinierte Skalarfunktionen für eine Datenbank wieder zu aktivieren:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET TSQL_SCALAR_UDF_INLINING = ON;
```

Wenn die Option auf ON festgelegt ist, wird die Einstellung in [`sys.database_scoped_configurations`](../system-catalog-views/sys-database-scoped-configurations-transact-sql.md) als „aktiviert“ angezeigt. Sie können das Inlining benutzerdefinierter Skalarfunktionen für eine bestimmte Abfrage auch deaktivieren, indem Sie `DISABLE_TSQL_SCALAR_UDF_INLINING` als `USE HINT`-Abfragehinweis festlegen. Beispiel:

```sql
SELECT L_SHIPDATE, O_SHIPPRIORITY, SUM (dbo.discount_price(L_EXTENDEDPRICE, L_DISCOUNT)) 
FROM LINEITEM
INNER JOIN ORDERS
  ON O_ORDERKEY = L_ORDERKEY 
GROUP BY L_SHIPDATE, O_SHIPPRIORITY ORDER BY L_SHIPDATE
OPTION (USE HINT('DISABLE_TSQL_SCALAR_UDF_INLINING'));
```

> [!TIP]
> Ein `USE HINT`-Abfragehinweis hat Vorrang vor einer datenbankweit gültigen Konfiguration oder einer Einstellung des Kompatibilitätsgrads.

Das Inlining benutzerdefinierter Skalarfunktionen kann auch für eine bestimmte benutzerdefinierte Funktion deaktiviert werden, indem Sie die INLINE-Klausel in der `CREATE FUNCTION`- oder `ALTER FUNCTION`-Anweisung verwenden.
Beispiel:

```sql
CREATE OR ALTER FUNCTION dbo.discount_price(@price DECIMAL(12,2), @discount DECIMAL(12,2))
RETURNS DECIMAL (12,2)
WITH INLINE = OFF
AS
BEGIN
    RETURN @price * (1 - @discount);
END;
```

Sobald die oben genannte Anweisung ausgeführt wurde, wird für diese benutzerdefinierte Funktion kein Inlining durchgeführt, wenn sie von einer Abfrage aufgerufen wird. Führen Sie folgende Anweisung aus, um das Inlining für diese benutzerdefinierte Funktion wieder zu aktivieren:

```sql
CREATE OR ALTER FUNCTION dbo.discount_price(@price DECIMAL(12,2), @discount DECIMAL(12,2))
RETURNS DECIMAL (12,2)
WITH INLINE = ON
AS
BEGIN
    RETURN @price * (1 - @discount);
END
```

> [!NOTE]
> Die `INLINE`-Klausel ist nicht verbindlich. Wenn die `INLINE`-Klausel nicht festgelegt ist, wird sie automatisch auf `ON`/`OFF` festgelegt, je nachdem, ob ein Inlining für die benutzerdefinierte Funktion durchgeführt werden kann. Wenn `INLINE = ON` festgelegt ist, aber für die benutzerdefinierte Funktion kein Inlining durchgeführt werden kann, wird ein Fehler ausgegeben.

## <a name="important-notes"></a>Wichtige Hinweise
Wie in diesem Artikel beschrieben wurde, wird beim Inlining einer benutzerdefinierten Skalarfunktion eine Abfrage mit benutzerdefinierten Skalarfunktionen in eine Abfrage mit einer gleichwertigen skalaren Unterabfrage transformiert. Durch diese Transformation kann es in folgenden Szenarios zu Unterschieden bei der Verhaltensweise kommen:

1. Das Inlining führt zu einem anderen Abfragehash für den gleichen Abfragetext.
1. Bestimmte Warnungen in Anweisungen in der benutzerdefinierten Funktion (z.B. die Division durch 0), die zuvor ausgeblendet waren, werden durch das Inlining nun möglicherweise angezeigt.
1. Joinhinweise auf Abfrageebene sind möglicherweise nicht mehr gültig, da durch das Inlining neue Joins hinzugefügt werden. Sie müssen stattdessen lokale Joinhinweise verwenden.
1. Ansichten, die auf benutzerdefinierte Inlineskalarfunktionen verweisen, können nicht indiziert werden. Wenn Sie einen Index in einer entsprechenden Ansicht erstellen müssen, sollten Sie das Inlining für die benutzerdefinierten Funktionen deaktivieren, auf die verwiesen wird.
1. Durch das Inlining benutzerdefinierter Funktionen kann das Verhalten der [dynamischen Datenmaskierung](../security/dynamic-data-masking.md) sich ändern. In bestimmten Situationen (je nach Logik in der benutzerdefinierten Funktion) in Bezug auf das Maskieren von Ausgabespalten einen konservativeren Ansatz dar. In Szenarios, bei denen es sich bei den Spalten, auf die in einer benutzerdefinierten Funktion verwiesen wird, nicht um Ausgabespalten handelt, werden diese nicht maskiert. 
1. Wenn eine benutzerdefinierte Funktion auf integrierte Funktionen wie `SCOPE_IDENTITY()`, `@@ROWCOUNT` oder `@@ERROR` verweist, ändert sich der Wert, der von der integrierten Funktion zurückgegeben wird, durch das Inlining. Diese Änderung im Verhalten geht darauf zurück, dass das Inlining den Bereich der Anweisungen in der benutzerdefinierten Funktion ändert. Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU2 wird das Inlining blockiert, wenn die benutzerdefinierte Funktion auf bestimmte intrinsische Funktionen verweist (z. B. `@@ROWCOUNT`).

## <a name="see-also"></a>Weitere Informationen
[Erstellen von benutzerdefinierten Funktionen (Datenbank-Engine)](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)   
[Leistungscenter für SQL Server-Datenbankmodul und Azure SQL-Datenbank](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md)     
[Leitfaden zur Architektur der Abfrageverarbeitung](../../relational-databases/query-processing-architecture-guide.md)     
[Referenz zu logischen und physischen Showplanoperatoren](../../relational-databases/showplan-logical-and-physical-operators-reference.md)     
[Joins](../../relational-databases/performance/joins.md)     
[Demo zur intelligenten Abfrageverarbeitung](https://aka.ms/IQPDemos)     
[FIX: Inliningprobleme bei benutzerdefinierten Skalarfunktionen in SQL Server 2019](https://support.microsoft.com/help/4538581)     

