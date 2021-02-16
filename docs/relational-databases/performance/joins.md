---
title: Joins (SQL Server) | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Typen von Join-Vorgängen (Verknüpfungen), die SQL Server verwendet. Beim Verwenden von Join-Vorgängen unterstützt SQL Server die vertikale Tabellenpartitionierung, auch spaltenweise Speicherung genannt.
ms.custom: ''
ms.date: 07/19/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- HASH join
- NESTED LOOPS join
- MERGE join
- ADAPTIVE join
- joins [SQL Server], about joins
- join hints [SQL Server]
ms.assetid: bfc97632-c14c-4768-9dc5-a9c512f4b2bd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0716c686698745e5dae851f17844e49b5cab0872
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348023"
---
# <a name="joins-sql-server"></a>Joins (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet bei Sortier-, Schnittmengen- und Vereinigungsvorgängen sowie bei Vorgängen zum Feststellen von Unterschieden arbeitsspeicherinterne Sortierverfahren und Hashjointechniken. Wenn dieser Abfrageplan verwendet wird, unterstützt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die vertikale Tabellenpartitionierung.   

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implementiert logische Joinvorgänge, die durch die Syntax [!INCLUDE[tsql](../../includes/tsql-md.md)] festgelegt werden:
-   Innere Verknüpfung
-   Left Outer Join
-   Rechte äußere Verknüpfung
-   Vollständiger äußerer Join
-   Kreuzprodukt

> [!NOTE]
> Weitere Informationen zur Joinsyntax finden Sie unter [FROM-Klausel mit JOIN, APPLY, PIVOT (Transact-SQL)](../../t-sql/queries/from-transact-sql.md).

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet vier verschiedene physische Joinvorgänge, um logische Joinvorgänge auszuführen:    
-   Joins geschachtelter Schleifen     
-   Zusammenführungsjoins   
-   Hashjoins   
-   Adaptive Joins (beginnend mit [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])

## <a name="join-fundamentals"></a><a name="fundamentals"></a> Grundlegende Informationen zu Joins
Mithilfe von Joins können Sie Daten aus zwei oder mehr Tabellen basierend auf logischen Beziehungen zwischen den Tabellen abrufen. Joins zeigen an, wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Daten aus einer Tabelle zum Auswählen der Zeilen in einer anderen Tabelle verwenden soll.    

Eine Joinbedingung definiert die Beziehung zweier Tabellen in einer Abfrage auf folgende Art:    
-   Sie gibt die Spalte aus jeder Tabelle an, die für den Join verwendet werden soll. Eine typische Joinbedingung gibt einen Fremdschlüssel aus einer Tabelle und den zugehörigen Schlüssel in der anderen Tabelle an.    
-   Sie gibt einen logischen Operator (z. B. = oder <>) an, der zum Vergleichen von Werten aus den Spalten verwendet wird.   

Joins werden mithilfe der folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Syntax logisch ausgedrückt:
-   INNER JOIN
-   LEFT [ OUTER ] JOIN
-   RIGHT [ OUTER ] JOIN
-   FULL [ OUTER ] JOIN
-   CROSS JOIN

**Innere Joins** können in `FROM`- oder `WHERE`-Klauseln angegeben werden. **Äußere Joins** und **Kreuzprodukte** können nur in der `FROM`-Klausel angegeben werden. Die Joinbedingungen in Verbindung mit `WHERE`- und `HAVING`-Suchbedingungen steuern, welche Zeilen aus den Basistabellen ausgewählt werden, auf die in der `FROM`-Klausel verwiesen wird.    

Das Angeben der Joinbedingungen in der `FROM`-Klausel trägt dazu bei, dass diese von anderen, möglicherweise in einer `WHERE`-Klausel angegebenen Suchbedingungen getrennt werden. Dies ist die empfohlene Methode zur Angabe von Joins. Eine vereinfachte ISO-Joinsyntax für eine `FROM`-Klausel lautet:

```
FROM first_table < join_type > second_table [ ON ( join_condition ) ]
```

*join_type* gibt an, welche Art von Join ausgeführt wird: innerer Join, äußerer Join oder Cross Join. *join_condition* definiert das Prädikat, das für jedes verknüpfte Zeilenpaar ausgewertet werden soll. Dies ist ein Beispiel für die Joinspezifizierung einer `FROM`-Klausel:

```sql
FROM Purchasing.ProductVendor INNER JOIN Purchasing.Vendor
     ON ( ProductVendor.BusinessEntityID = Vendor.BusinessEntityID )
```

Dies ist ein Beispiel für eine einfache `SELECT`-Anweisung mithilfe dieses Joins:

```sql
SELECT ProductID, Purchasing.Vendor.BusinessEntityID, Name
FROM Purchasing.ProductVendor INNER JOIN Purchasing.Vendor
    ON (Purchasing.ProductVendor.BusinessEntityID = Purchasing.Vendor.BusinessEntityID)
WHERE StandardPrice > $10
  AND Name LIKE N'F%'
GO
```

Die `SELECT`-Anweisung gibt die Produkt- und Lieferanteninformationen für alle Kombinationen der Teile zurück, die von einer Firma geliefert werden, deren Firmenname mit dem Buchstaben F beginnt, und bei denen der Produktpreis über 10 USD liegt.   

Wenn in einer einzigen Abfrage auf mehrere Tabellen verwiesen wird, müssen alle Spaltenverweise eindeutig sein. Im vorherigen Beispiel verfügt sowohl die `ProductVendor`- als auch die `Vendor`-Tabelle über eine Spalte mit der Bezeichnung `BusinessEntityID`. Alle Spaltennamen, die in zwei oder mehr Tabellen vorkommen, auf die in der Abfrage verwiesen wird, müssen mit dem Tabellennamen gekennzeichnet werden. Alle Verweise auf die `Vendor`-Spalten im Beispiel sind gekennzeichnet.   

Wenn ein Spaltenname nicht in zwei oder mehr in der Abfrage verwendeten Tabellen vorkommt, brauchen Verweise darauf nicht mit dem Tabellennamen gekennzeichnet zu werden. Dies ist im vorherigen Beispiel dargestellt. Eine solche `SELECT`-Klausel ist manchmal schwer verständlich, weil nicht angezeigt wird, welche Spalte aus welcher Tabelle stammt. Die Lesbarkeit der Abfrage wird verbessert, indem alle Spalten mit dem entsprechenden Tabellennamen gekennzeichnet werden. Die Übersichtlichkeit kann weiterhin durch Verwenden von Tabellenaliasnamen verbessert werden, besonders, wenn die Tabellennamen mit dem Datenbank- und Besitzernamen gekennzeichnet werden müssen. Das folgende Beispiel unterscheidet sich vom vorherigen nur dadurch, dass Tabellenaliasnamen zugewiesen und die Spalten der Übersichtlichkeit halber mit Tabellenaliasnamen gekennzeichnet wurden:

```sql
SELECT pv.ProductID, v.BusinessEntityID, v.Name
FROM Purchasing.ProductVendor AS pv 
INNER JOIN Purchasing.Vendor AS v
    ON (pv.BusinessEntityID = v.BusinessEntityID)
WHERE StandardPrice > $10
    AND Name LIKE N'F%';
```

In den vorhergehenden Beispielen wurden die Joinbedingungen in der `FROM`-Klausel angegeben. Dies ist die bevorzugte Methode. Die folgende Abfrage enthält dieselbe Joinbedingung, die in der `WHERE`-Klausel angegeben wird:

```sql
SELECT pv.ProductID, v.BusinessEntityID, v.Name
FROM Purchasing.ProductVendor AS pv, Purchasing.Vendor AS v
WHERE pv.BusinessEntityID=v.BusinessEntityID
    AND StandardPrice > $10
    AND Name LIKE N'F%';
```

Die `SELECT`-Liste für einen Join kann auf alle Spalten in den verknüpften Tabellen oder auf eine beliebige Teilmenge der Spalten verweisen. Es ist nicht erforderlich, dass die `SELECT`-Liste Spalten aus allen Tabellen im Join enthält. So kann z. B. in einem Join dreier Tabellen nur eine Tabelle verwendet werden, um die anderen Tabellen zu verbinden, und in der Auswahlliste muss auf keine der Spalten aus der mittleren Tabelle verwiesen werden. Dies wird auch als **Antisemijoin** bezeichnet.  

Obwohl Joinbedingungen normalerweise Übereinstimmungsvergleiche (=) enthalten, können andere Vergleichsoperatoren oder relationale Operatoren ebenso wie andere Prädikate angegeben werden. Weitere Informationen finden Sie unter [Vergleichsoperatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md) und [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md).  

Beim Verarbeiten von Joins durch [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wählt der Abfrageoptimierer aus verschiedenen Möglichkeiten die effizienteste Methode aus. Dabei werden der effizienteste physische Join, die Joinreihenfolge der Tabellen und sogar logische Joinvorgänge ausgewählt, die mit der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Syntax nicht direkt ausgedrückt werden können (z. B. **Semijoins** und **Antisemijoins**). Die physische Ausführung von verschiedenen Joins kann viele verschiedene Optimierungen verwenden, weshalb keine zuverlässige Einschätzung möglich ist. Weitere Informationen zu Semijoins und Antisemijoins finden Sie unter [Referenz zu logischen und physischen Showplanoperatoren](../../relational-databases/showplan-logical-and-physical-operators-reference.md).  

Spalten, die in einer Joinbedingung verwendet werden, müssen nicht den gleichen Namen oder Datentyp haben. Die Datentypen müssen jedoch kompatibel sein, falls sie nicht identisch sind, oder es müssen Datentypen sein, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implizit konvertieren kann. Wenn die Datentypen nicht implizit konvertiert werden können, muss die Joinbedingung den Datentyp mithilfe der `CAST`-Funktion explizit konvertieren. Weitere Informationen zur impliziten und expliziten Konvertierung finden Sie unter [Datentypkonvertierung &#40;Datenbank-Engine&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md).    

Die meisten Abfragen, die einen Join verwenden, können in eine Unterabfrage (eine in eine andere Abfrage geschachtelte Abfrage) umgeschrieben werden, und die meisten Unterabfragen können in Joins umgeschrieben werden. Weitere Informationen zu Unterabfragen finden Sie unter [Unterabfragen](../../relational-databases/performance/subqueries.md).   

> [!NOTE]
> Tabellen können nicht direkt über ntext-, text- oder image-Spalten verknüpft werden. Sie können jedoch mithilfe von `SUBSTRING` indirekt über ntext- , text- oder image-Spalten verknüpft werden.    
> `SELECT * FROM t1 JOIN t2 ON SUBSTRING(t1.textcolumn, 1, 20) = SUBSTRING(t2.textcolumn, 1, 20)` führt z.B. einen inneren Join zwischen zwei Tabellen auf den ersten 20 Zeichen jeder Textspalte in den Tabellen t1 und t2 aus.   
> Außerdem können ntext- oder text-Spalten aus zwei Tabellen verglichen werden, indem die Längen der Spalten mithilfe einer `WHERE`-Klausel wie im folgenden Beispiel verglichen werden: `WHERE DATALENGTH(p1.pr_info) = DATALENGTH(p2.pr_info)`.

## <a name="understanding-nested-loops-joins"></a><a name="nested_loops"></a> Grundlegendes zu Nested Loops-Joins
Wenn eine Joineingabe klein (weniger als 10 Zeilen) und die andere Joineingabe relativ umfangreich ist und indizierte Joinspalten aufweist, ist ein indizierter Nested Loops-Join der schnellste Joinvorgang, da sie mit dem geringsten E/A-Aufkommen und den wenigsten Vergleichen auskommen. 

Der Join geschachtelter Schleifen, auch *geschachtelte Iteration* genannt, verwendet eine Joineingabe als äußere Eingabetabelle (im grafischen Ausführungsplan als obere Eingabe dargestellt) und eine zweite Joineingabe als innere (untere) Eingabetabelle. Die äußere Schleife verarbeitet die äußere Eingabetabelle zeilenweise. Die innere Schleife wird für jede äußere Zeile ausgeführt und sucht übereinstimmende Zeilen in der inneren Eingabetabelle.   

Im einfachsten Fall wird beim Suchvorgang eine Tabelle oder ein Index vollständig gescannt. Dieser Vorgang wird als *naiver Join geschachtelter Schleifen* bezeichnet. Wird beim Suchvorgang ein Index verwendet, wird dies *indizierter Join geschachtelter Schleifen* genannt. Wird der Index als Teil des Abfrageplans erstellt (und nach Beendigung der Abfrage gelöscht), wird dies *temporärer indizierter Join geschachtelter Schleifen* genannt. Alle beschriebenen Varianten werden vom Abfrageoptimierer berücksichtigt.   

Ein Nested Loops-Join ist besonders wirksam, wenn die äußere Eingabe klein und die innere Eingabe vorindiziert und umfangreich ist. In vielen kleinen Transaktionen, die beispielsweise nur wenige Zeilen betreffen, sind indizierte Nested Loops-Joins sowohl Zusammenführungsjoins als auch Hashjoins überlegen. In umfangreichen Abfragen dagegen sind Nested Loops-Joins häufig nicht die optimale Wahl.    

Wenn das „OPTIMIZED“-Attribut eines Operators für Joins geschachtelter Schleifen auf **True** festgelegt ist, bedeutet dies, dass ein optimierter Join geschachtelter Schleifen (oder eine Sortierung im Batchmodus) verwendet wird, um E/A-Vorgänge zu minimieren, wenn die innere Tabelle groß ist, unabhängig davon, ob sie parallelisiert wird oder nicht. Das Vorhandensein dieser Optimierung in einem bestimmten Plan ist beim Analysieren eines Ausführungsplans möglicherweise nicht offensichtlich, da die Sortierung selbst ein verborgener Vorgang ist. Wenn jedoch in der Plan-XML nach dem Attribut „OPTIMIZED“ gesucht wird, deutet dies darauf hin, dass der Join geschachtelter Schleifen möglicherweise versucht, die Eingabezeilen neu anzuordnen und die E/A-Leistung zu verbessern.

## <a name="understanding-merge-joins"></a><a name="merge"></a> Grundlegendes zu Zusammenführungsjoins
Sind beide Joineingaben nicht klein, aber nach ihrer Joinspalte sortiert (was beispielsweise der Fall ist, wenn sie beim Scannen sortierter Indizes gewonnen wurden), so ist ein Zusammenführungsjoin der schnellste Joinvorgang. Sind beide Joineingaben umfangreich und etwa gleich groß, so bietet ein Zusammenführungsjoin mit vorherigem Sortiervorgang und ein Hashjoin vergleichbares Leistungsverhalten. Hashjoinvorgänge sind jedoch häufig erheblich schneller, wenn sich beide Eingaben im Umfang deutlich unterscheiden.       

Der Zusammenführungsjoin setzt voraus, dass beide Eingaben nach den Zusammenführungsspalten sortiert sind; diese werden von den Gleichheitsoperatoren des Joinprädikats (in der ON-Klausel) definiert. Der Abfrageoptimierer scannt in der Regel einen Index, falls ein Index für die geeigneten Spalten vorhanden ist, oder platziert einen Sort-Operator unter den Zusammenführungsjoin. In seltenen Fällen treten mehrere Gleichheitsklauseln auf, jedoch werden die Zusammenführungsspalten nur von einigen der verfügbaren Gleichheitsklauseln genommen.    

Da die Eingaben sortiert vorliegen, ruft der **Merge Join**-Operator von jeder Eingabe jeweils eine Zeile ab und vergleicht diese. Beispielsweise werden bei inneren Joins die Zeilen zurückgegeben, wenn sie gleich sind. Sind sie nicht gleich, wird die Zeile mit dem kleineren Wert verworfen, und von der betreffenden Eingabe wird die nächste Zeile abgerufen. Dieser Vorgang wird wiederholt, bis alle Zeilen verarbeitet wurden.    

Die Zusammenführungsjoinoperation kann eine reguläre oder eine n:n-Operation sein. Ein n:n-Zusammenführungsjoin verwendet eine temporäre Tabelle, um Zeilen zu speichern. Treten bei den Eingaben doppelte Werte auf, so muss eine Eingabe bis zum ersten Duplikat zurückgespult werden, damit alle Duplikate der anderen Eingabe verarbeitet werden können.    

Ist ein Residualprädikat vorhanden, so werten alle Zeilen, die dem Zusammenführungsprädikat genügen, das Residualprädikat aus. Nur die Zeilen werden zurückgegeben, die auch dem Residualprädikat genügen.   

Der Zusammenführungsjoin ist sehr schnell, kann aber eine teure Wahl darstellen, wenn Sortieroperationen erforderlich sind. Wenn jedoch die Datenmenge umfangreich ist und die gewünschten Daten vorsortiert aus vorhandenen B-Baum-Indizes abgerufen werden können, ist der Zusammenführungsjoin häufig der schnellste Joinalgorithmus.    

## <a name="understanding-hash-joins"></a><a name="hash"></a> Grundlegendes zu Hashjoins
Hashjoins können umfangreiche, unsortierte, nicht indizierte Eingaben effizient verarbeiten. Sie sind für Zwischenergebnisse in komplexen Abfragen aus folgenden Gründen nützlich:
-   Zwischenergebnisse sind nicht indiziert (es sei denn, sie werden explizit auf einem Datenträger gespeichert und dann indiziert) und häufig nicht für den nächste Vorgang im Abfrageplan passend sortiert.
-   Abfrageoptimierer schätzen nur die Größe von Zwischenergebnissen ab. Weil Schätzungen in komplexen Abfragen sehr ungenau sein können, müssen Algorithmen zum Verarbeiten von Zwischenergebnissen nicht nur effizient sein, sondern auch kontrolliert beendet werden können, wenn ein Zwischenergebnis viel umfangreicher als erwartet ausfällt.   

Der Hashjoin lässt Reduktionen bei der Denormalisierung zu. Die Denormalisierung wird in der Regel verwendet, um bessere Leistung durch Reduzierung der Joinvorgänge zu erreichen, und zwar trotz der Redundanzgefahr wie beispielsweise durch inkonsistente Updates. Hashjoins vermindern den Bedarf, die Denormalisierung durchzuführen. Hashjoins ermöglichen die vertikale Partitionierung (die Darstellung von Spaltengruppen aus einer einzelnen Tabelle in separate Dateien oder Indizes), wodurch sie zu einer beachtenswerten Option für den physischen Datenbankentwurf wird.     

Der Hashjoin verfügt über zwei Eingaben: die **Erstellungseingabe** und die **Untersuchungseingabe**. Der Abfrageoptimierer weist diese Rollen so zu, dass die kleinere Eingabe als Erstellungseingabe verwendet wird.    

Hashjoins werden für viele ergebnismengenorientierte Operationen verwendet: INNER JOIN (innerer Join); LEFT, RIGHT und FULL OUTER JOIN (linker, rechter und vollständiger äußerer Join); LEFT SEMI-JOIN und RIGHT SEMI-JOIN (linker und rechter Semijoin); INTERSECTION (Schnittmenge); UNION (Vereinigungsmenge) und DIFFERENCE (Restmenge). Darüber hinaus können mit einer Variante des Hashjoins Duplikate entfernt und Gruppierungen vorgenommen werden, z.B. `SUM(salary) GROUP BY department`. Diese Varianten verwenden dieselbe Eingabe als Erstellungseingabe und Untersuchungseingabe.   

In den folgenden Abschnitten werden verschiedene Typen von Hashjoins beschrieben: arbeitsspeicherinterne Hashjoins, schrittweise Hashjoins und rekursive Hashjoins.    

### <a name="in-memory-hash-join"></a><a name="inmem_hash"></a> Arbeitsspeicherinterne Hashjoins
Der Hashjoin scannt oder berechnet zuerst die gesamte Erstellungseingabe und erstellt dann eine Hashtabelle im Arbeitsspeicher. Jede Zeile wird in ein Hashbucket eingefügt, abhängig von dem für den Hashschlüssel berechneten Hashwert. Wenn die gesamte Erstellungseingabe kleiner als der verfügbare Arbeitsspeicher ist, können alle Zeilen in die Hashtabelle eingefügt werden. Auf diese Erstellungsphase folgt die Untersuchungsphase. Die gesamte Untersuchungseingabe wird zeilenweise gescannt oder berechnet, und für jede Untersuchungszeile wird der Wert des Hashschlüssels berechnet, das entsprechende Hashbucket wird gescannt, und die Übereinstimmungen werden erzeugt.    

### <a name="grace-hash-join"></a><a name="grace_hash"></a> GRACE-Hashjoins
Wenn die Erstellungseingabe nicht vollständig in den Arbeitsspeicher passt, wird der Hashjoin in mehreren Schritten durchgeführt. Dies wird als schrittweiser Hashjoin bezeichnet. Jeder Schritt besteht aus einer Erstellungsphase und einer Untersuchungsphase. Zuerst wird die gesamte Erstellungseingabe und Untersuchungseingabe verarbeitet und (durch Anwenden einer Hashfunktion auf die Hashschlüssel) in mehrere Dateien aufgeteilt. Durch Anwenden der Hashfunktion auf die Hashschlüssel wird sichergestellt, dass die zwei zu verknüpfenden Datensätze sich stets in demselben Dateipaar befinden. So wird die Aufgabe, zwei umfangreiche Eingaben zu verknüpfen, auf mehrere kleinere gleichartige Teilaufgaben reduziert. Der Hashjoin wird dann auf jedes Paar partitionierter Dateien angewendet.    

### <a name="recursive-hash-join"></a><a name="recursive_hash"></a> Rekursive Hashjoins
Wenn die Erstellungseingabe so umfangreich ist, dass Eingaben für einen standardmäßigen externen Mergeprozess mehrere Mergeebenen erfordern würden, sind mehrere Partitionierungsschritte und mehrere Partitionierungsebenen erforderlich. Sind nur einige Partitionen umfangreich, so sind zusätzliche Partitionierungsschritte nur für diese Partitionen erforderlich. Um alle Partitionierungsschritte möglichst schnell zu machen, werden umfangreiche, asynchrone E/A-Operationen verwendet, sodass bereits ein Thread mehrere Datenträger auslastet.    

> [!NOTE]
> Wenn die Erstellungseingabe nur wenig umfangreicher als der verfügbare Arbeitsspeicher ist, werden Elemente des schrittweisen und des arbeitsspeicherinternen Hashjoin in einem Schritt kombiniert, sodass ein hybrider Hashjoin entsteht.   

Bei der Optimierung kann nicht in jedem Fall ermittelt werden, welcher Hashjoin verwendet wird. Daher verwendet SQL Server zunächst einen arbeitsspeicherinternen Hashjoin und geht, abhängig von der Größe der Erstellungseingabe, sukzessive zum GRACE- und rekursiven Hashjoin über.    

Wenn der Abfrageoptimierer falsch einschätzt, welche Eingabe kleiner ist und daher als Erstellungseingabe verwendet werden müsste, werden die Rollen der Erstellungs- und der Untersuchungseingabe dynamisch vertauscht. Der Hashjoin stellt sicher, dass die kleinere Überlaufdatei als Erstellungseingabe verwendet wird. Diese Technik wird als Rollentausch bezeichnet. Ein Rollentausch tritt innerhalb des Hashjoins nach mindestens einem Überlauf auf den Datenträger auf.     

> [!NOTE]
> Der Rollentausch tritt unabhängig von Abfragehinweisen oder von der Struktur auf. Der Rollentausch wird nicht im Abfrageplan angezeigt. Wenn ein solcher Vorgang auftritt, erfolgt er transparent für den Benutzer.

### <a name="hash-bailout"></a><a name="hash_bailout"></a> Hashabbruch
Der Begriff „Hashabbruch“ wird manchmal zur Beschreibung von GRACE- oder rekursiven Hashjoins verwendet.    

> [!NOTE]
> Rekursive Hashjoins oder Hashabbrüche verursachen eine reduzierte Leistung auf dem Server. Wenn in einer Ablaufverfolgung viele Hash Warning-Ereignisse angezeigt werden, sollten Sie die Statistiken für die verknüpften Spalten aktualisieren.    

Weitere Informationen zu Hashabbrüchen finden Sie unter [Hash Warning-Ereignisklasse](../../relational-databases/event-classes/hash-warning-event-class.md).    

## <a name="understanding-adaptive-joins"></a><a name="adaptive"></a> Grundlegendes zu adaptiven Joins
Mit dem [Batchmodus](../../relational-databases/query-processing-architecture-guide.md#batch-mode-execution) für adaptive Joins können Sie wählen, ob eine Methode für [Hashjoins](#hash) oder [Joins geschachtelter Schleifen](#nested_loops) zurückgestellt wird. Die Methode wird dann erst **nach** der Überprüfung der ersten Eingabe angewendet. Der Operator für adaptive Joins definiert einen Schwellenwert, der bestimmt, wann zu einem Plan geschachtelter Schleifen gewechselt wird. Daher kann ein Abfrageplan während der Ausführung dynamisch zu einer passenderen Joinstrategie wechseln, ohne dass er erneut kompiliert werden muss. 

> [!TIP]
> Workloads mit häufiger Oszillation zwischen kleinen und großen Joineingabescans profitieren am meisten von dieser Funktion.

Die Laufzeitentscheidung basiert auf den folgenden Schritten:
-  Wenn die Anzahl der Zeilen der Buildjoineingabe so klein ist, dass ein Join geschachtelter Schleifen passender wäre als ein Hashjoin, wechselt der Plan zu einem Algorithmus geschachtelter Schleifen.
-  Wenn die Buildjoineingabe eine bestimmte Anzahl an Zeilen übersteigt, wird nicht gewechselt, und der Plan wird mit einem Hashjoin fortgesetzt.

Die folgende Abfrage veranschaulicht ein Beispiel für einen adaptiven Join:

```sql
SELECT [fo].[Order Key], [si].[Lead Time Days], [fo].[Quantity]
FROM [Fact].[Order] AS [fo]
INNER JOIN [Dimension].[Stock Item] AS [si]
       ON [fo].[Stock Item Key] = [si].[Stock Item Key]
WHERE [fo].[Quantity] = 360;
```

Die Abfrage gibt 336 Zeilen zurück. Bei Aktivierung der [Liveabfragestatistik](../../relational-databases/performance/live-query-statistics.md) wird der folgende Plan angezeigt:

![Abfrageergebnis: 336 Zeilen](../../relational-databases/performance/media/4_AQPStats336Rows.png)

Beachten Sie im Plan Folgendes:
1. Ein Columnstore-Indexscan wurde verwendet, um Zeilen für die Buildphase des Hashjoins bereitzustellen.
2. Den neuen Operator für adaptive Joins. Dieser Operator definiert einen Schwellenwert, der bestimmt, wann zu einem Plan geschachtelter Schleifen gewechselt wird. In diesem Beispiel entspricht der Schwellenwert 78 Zeilen. Alles mit &gt;= 78 Zeilen verwendet einen Hashjoin. Wenn der Schwellenwert nicht überschritten wird, wird ein Join geschachtelter Schleifen verwendet.
3. Da von der Abfrage 336 Zeilen zurückgegeben werden, wird der Schwellenwert überschritten. Deshalb stellt der zweite Branch die Überprüfungsphase eines standardmäßigen Hashjoinvorgangs dar. Beachten Sie, dass die Liveabfragestatistik Zeilen anzeigt, die die Operatoren durchlaufen: in diesem Fall „672 von 672“.
4. Beim letzten Branch handelt es sich um eine Suche im gruppierten Index, die vom Join geschachtelter Schleifen verwendet worden wäre, wenn der Schwellenwert nicht überschritten worden wäre. Beachten Sie, dass „0 von 336“ Zeilen angezeigt werden (der Branch wird nicht verwendet).

Vergleichen Sie den Plan nun mit derselben Abfrage, dieses Mal aber mit einem *Mengenwert*, der nur eine Zeile in der Tabelle hat:
 
```sql
SELECT [fo].[Order Key], [si].[Lead Time Days], [fo].[Quantity]
FROM [Fact].[Order] AS [fo]
INNER JOIN [Dimension].[Stock Item] AS [si]
       ON [fo].[Stock Item Key] = [si].[Stock Item Key]
WHERE [fo].[Quantity] = 361;
```
Die Abfrage gibt eine Zeile zurück. Bei Aktivierung der „Liveabfragestatistik“ wird der folgende Plan angezeigt:

![Abfrageergebnis eine Zeile](../../relational-databases/performance/media/5_AQPStatsOneRow.png)

Beachten Sie im Plan Folgendes:
- Bei einer zurückgegebenen Zeile durchlaufen Zeilen jetzt den Clustered Index Seek.
- Da die Buildphase des Hashjoins nicht fortgesetzt wurde, wird der zweite Branch nicht von Zeilen durchlaufen.

### <a name="adaptive-join-remarks"></a>Hinweise zu adaptiven Joins
Adaptive Joins erfordern mehr Speicherplatz als ein äquivalenter Plan eines indizierten Joins geschachtelter Schleifen. Der zusätzliche Speicherplatz wird so angefordert, als wären die geschachtelte Schleifen ein Hashjoin. Auch die Buildphase ist mit Aufwand verbunden: sowohl für einen Stop-and-Go-Vorgang als auch für einen äquivalenten Join eines Streamings geschachtelter Schleifen. Dieser zusätzliche Aufwand geht mit Flexibilität für Szenarios einher, in denen die Zeilenzahl möglicherweise in der Buildeingabe schwankt.

Adaptive Joins im Batchmodus funktionieren bei der ersten Ausführung einer Anweisung. Nach der ersten Kompilierung bleiben aufeinanderfolgende Ausführungen adaptiv, basierend auf dem Schwellenwert des kompilierten adaptiven Joins und den Laufzeitzeilen, die die Buildphase der äußeren Eingabe durchlaufen.

Wenn ein adaptiver Join zu einem Vorgang geschachtelter Schleifen wechselt, verwendet er die Zeilen, die bereits vom Hashjoinbuild gelesen wurden. Der Operator liest **nicht** erneut die Zeilen des äußeren Verweises.

### <a name="tracking-adaptive-join-activity"></a>Nachverfolgen der Aktivität adaptiver Joins
Der Operator für adaptive Joins verfügt über folgende Planoperatorattribute:

|Planattribut|BESCHREIBUNG|
|---|---|
|AdaptiveThresholdRows|Gibt den beim Wechsel von einem Hashjoin zu einem Nested Loop-Join zu verwendenden Schwellenwert an|
|EstimatedJoinType|Gibt den erwarteten Jointyp an|
|ActualJoinType|Gibt in einem Plan an, welcher Joinalgorithmus basierend auf dem Schwellenwert verwendet wurde.|

Der geschätzte Plan zeigt die Form des adaptiven Joinplans an sowie den definierten Schwellenwert für adaptive Joins und den geschätzten Jointyp.

> [!TIP]
> Der Abfragespeicher erfasst einen adaptiven Joinplan im Batchmodus und kann diesen erzwingen.

### <a name="adaptive-join-eligible-statements"></a>Zulässige Anweisungen für adaptive Joins
Ein logischer Join ist dann für adaptive Joins im Batchmodus zulässig, wenn er folgende Bedingungen erfüllt:
- Der Datenbank-Kompatibilitätsgrad ist 140 oder höher.
- Die Abfrage ist eine `SELECT`-Anweisung (Anweisungen zur Datenänderung sind aktuell nicht verfügbar).
- Der Join kann sowohl vom physischen Algorithmus eines indizierten Joins geschachtelter Schleifen als auch eines Hashjoins ausgeführt werden.
- Der Hashjoin verwendet den Batchmodus, der durch das Vorhandensein eines Columnstore-Index in der Abfrage, durch einen direkten Verweis auf eine Tabelle im Columnstore-Index im Join oder durch die Verwendung des [Batchmodus für Rowstore](./intelligent-query-processing.md#batch-mode-on-rowstore) aktiviert wird.
- Die generierten alternativen Lösungen des Joins geschachtelter Schleifen und Hashjoins sollten dasselbe erste untergeordnete Element haben (äußerer Verweis).

### <a name="adaptive-threshold-rows"></a>Adaptive Schwellenwertzeilen
Das folgende Diagramm zeigt eine beispielhafte Überschneidung zwischen den Ressourcen eines Hashjoins und den Ressourcen des alternativen Joins geschachtelter Schleifen. Am Überschneidungspunkt wird der Schwellenwert bestimmt, der wiederum den für den Joinvorgang verwendeten Algorithmus bestimmt.

![Schwellenwert des Joins](../../relational-databases/performance/media/6_AQPJoinThreshold.png)

### <a name="disabling-adaptive-joins-without-changing-the-compatibility-level"></a>Deaktivieren von adaptiven Joins ohne Änderung des Kompatibilitätsgrads
Adaptive Joins können im Datenbank- oder Anweisungsbereich deaktiviert werden, während der Datenbankkompatibilitätsgrad weiterhin bei 140 und höher bleibt.  
Führen Sie die folgende Anweisung im Kontext der betreffenden Datenbank aus, um adaptive Joins für alle Abfrageausführungen zu deaktivieren, die aus der Datenbank stammen:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_BATCH_MODE_ADAPTIVE_JOINS = ON;

-- Azure SQL Database, SQL Server 2019 and higher
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_ADAPTIVE_JOINS = OFF;
```

Ist diese Einstellung aktiviert, wird sie in [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) als aktiviert aufgeführt.
Um adaptive Joins für alle Abfrageausführungen wieder zu aktivieren, die aus der Datenbank stammen, führen Sie die folgende Anweisung im Kontext der betroffenen Datenbank aus:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_BATCH_MODE_ADAPTIVE_JOINS = OFF;

-- Azure SQL Database, SQL Server 2019 and higher
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_ADAPTIVE_JOINS = ON;
```

Durch Festlegen von `DISABLE_BATCH_MODE_ADAPTIVE_JOINS` als [USE HINT-Abfragehinweis](../../t-sql/queries/hints-transact-sql-query.md#use_hint) können adaptive Joins für eine bestimmte Abfrage auch deaktiviert werden. Beispiel:

```sql
SELECT s.CustomerID,
       s.CustomerName,
       sc.CustomerCategoryName
FROM Sales.Customers AS s
LEFT OUTER JOIN Sales.CustomerCategories AS sc
       ON s.CustomerCategoryID = sc.CustomerCategoryID
OPTION (USE HINT('DISABLE_BATCH_MODE_ADAPTIVE_JOINS')); 
```

> [!NOTE]
> Ein USE HINT-Abfragehinweis hat Vorrang vor einer datenbankweit gültigen Konfiguration oder einer Ablaufverfolgungsflageinstellung. 

## <a name="null-values-and-joins"></a><a name="nulls_joins"></a> NULL-Werte und Joins
Wenn die Spalten der zu verknüpfenden Tabellen NULL-Werte enthalten, werden diese Werte nicht als übereinstimmend angesehen. Das Vorhandensein von NULL-Werten in einer Spalte aus einer der verknüpften Tabellen kann nur mithilfe eines äußeren Joins zurückgegeben werden (wenn die `WHERE`-Klausel keine NULL-Werte ausschließt).     

Es folgen zwei Tabellen, bei denen NULL in der Spalte enthalten ist, die Bestandteil des Joins ist.     

```
table1                          table2
a           b                   c            d
-------     ------              -------      ------
      1        one                 NULL         two
   NULL      three                    4        four
      4      join4
```    

Ein Join, der die Werte in der a-Spalte mit denen der c-Spalte vergleicht, erhält keine Übereinstimmung für die Zeilen, die NULL-Werte enthalten:

```sql
SELECT *
FROM table1 t1 JOIN table2 t2
   ON t1.a = t2.c
ORDER BY t1.a;
GO
```  

Nur eine Zeile mit dem Wert 4 in der a-Spalte und der c-Spalte wird zurückgegeben:

```
a           b      c           d      
----------- ------ ----------- ------ 
4           join4  4           four   

(1 row(s) affected)
```   

Außerdem sind aus einer Basistabelle zurückgegebene NULL-Werte schwer von den von einem äußeren Join zurückgegebenen NULL-Werten zu unterscheiden. Die folgende `SELECT`-Anweisung führt z. B. einen linken äußeren Join für diese beiden Tabellen aus:   

```sql
SELECT *
FROM table1 t1 LEFT OUTER JOIN table2 t2
   ON t1.a = t2.c
ORDER BY t1.a;
GO
```   

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]   

```
a           b      c           d      
----------- ------ ----------- ------ 
NULL        three  NULL        NULL 
1           one    NULL        NULL 
4           join4  4           four   

(3 row(s) affected)
```   

In den Ergebnissen ist ein NULL-Wert in den Daten nicht ohne Weiteres von einem NULL-Wert zu unterscheiden, der einen fehlgeschlagenen Join darstellt. Wenn NULL-Werte in den zu verknüpfenden Daten enthalten sind, empfiehlt es sich, sie durch einen normalen Join aus den Ergebnissen auszuschließen.    

## <a name="see-also"></a>Weitere Informationen  
[Referenz zu logischen und physischen Showplanoperatoren](../../relational-databases/showplan-logical-and-physical-operators-reference.md)     
[Vergleichsoperatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md)    
[Datentypkonvertierung &#40;Datenbank-Engine&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)   
[Unterabfragen](../../relational-databases/performance/subqueries.md)      
[Adaptive Joins](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-adaptive-joins)    
[FROM-Klausel mit JOIN, APPLY, PIVOT (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)
