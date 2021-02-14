---
description: SHORTEST_PATH (Transact-SQL)
title: Kürzeste Pfade (SQL-Graph) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: language-reference
f1_keywords:
- SHORTEST PATH
- SHORTEST_PATH
dev_langs:
- TSQL
helpviewer_keywords:
- MATCH statement [SQL Server], SQL graph
- SQL graph, MATCH statement
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-ver15||=azuresqldb-mi-current
ms.openlocfilehash: 9e5a6f6506e4ce40b3b89f32dd58329eb4667760
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100351397"
---
# <a name="shortest_path-transact-sql"></a>SHORTEST_PATH (Transact-SQL)

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi.md)]

  Gibt eine Such Bedingung für ein Diagramm an, das rekursiv oder wiederholt durchsucht wird. SHORTEST_PATH können in der SELECT-Anweisung innerhalb der Übereinstimmung mit Diagramm Knoten und Edge-Tabellen verwendet werden. 
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="shortest-path"></a>Kürzeste Pfad
Mit der SHORTEST_PATH-Funktion können Sie Folgendes finden:    
* Ein kürzeste Pfad zwischen zwei angegebenen Knoten/Entitäten
* Kürzeste Pfade der einzelnen Quelle.
* Der kürzeste Pfad von mehreren Quellknoten zu mehreren Zielknoten.

Er nimmt ein beliebiges Längen Muster als Eingabe an und gibt einen kürzesten Pfad zurück, der zwischen zwei Knoten besteht. Diese Funktion kann nur innerhalb von Match verwendet werden. Die-Funktion gibt nur einen kürzesten Pfad zwischen zwei angegebenen Knoten zurück. Wenn es vorhanden ist, zwei oder mehr kürzeste Pfade derselben Länge zwischen einem beliebigen Paar von Quell-und Zielknoten, gibt die Funktion nur einen Pfad zurück, der bei der Durchquerung zuerst gefunden wurde. Beachten Sie, dass ein beliebiges Längen Muster nur innerhalb einer SHORTEST_PATH Funktion angegeben werden kann. 

Die Syntax finden Sie unter [Match (SQL Graph)](../../t-sql/queries/match-sql-graph.md) . 

## <a name="for-path"></a>für Pfad
FOR PATH muss mit einem beliebigen Knoten-oder Kanten Tabellennamen in der from-Klausel verwendet werden, die an einem beliebigen Längen Muster beteiligt ist. Für path weist die Engine an, dass die Knoten-oder Kanten Tabelle eine geordnete Auflistung zurückgibt, die die Liste der Knoten oder Kanten darstellt, die entlang des durchsuchten Pfades gefunden werden. Die Attribute aus diesen Tabellen können nicht direkt in die SELECT-Klausel projiziert werden. Zum Projizieren von Attributen aus diesen Tabellen müssen Graph-Pfad Aggregatfunktionen verwendet werden.  

## <a name="arbitrary-length-pattern"></a>Beliebiges Längen Muster
Dieses Muster umfasst die Knoten und Kanten, die wiederholt durchlaufen werden müssen, bis der gewünschte Knoten erreicht wird, oder bis die maximale Anzahl von Iterationen, die im Muster angegeben sind, erreicht wird. Jedes Mal, wenn die Abfrage ausgeführt wird, ist das Ergebnis der Ausführung dieses Musters eine geordnete Auflistung der Knoten und Kanten, die entlang des Pfads vom Startknoten zum Endknoten durchlaufen werden. Dabei handelt es sich um ein Stil Syntax Muster für reguläre Ausdrücke, und die folgenden beiden Muster quantifiziererer werden unterstützt:

* **"+"**: Wiederholen Sie das Muster mindestens 1 mal. Beenden, sobald der kürzeste Pfad gefunden wird.
* **{1,n}** : Das Muster 1 bis „n“ Male wiederholen. Beenden, sobald eine kürzeste gefunden wurde.

## <a name="last_node"></a>LAST_NODE
Die LAST_NODE ()-Funktion ermöglicht das Verketten von zwei beliebigen Längen Durchlauf Mustern. Sie kann in Szenarien verwendet werden, in denen Folgendes gilt:    
* In einer Abfrage werden mehr als ein kürzeste Pfad Muster verwendet, und ein Muster beginnt mit dem letzten Knoten des vorherigen Musters.
* Zwei kürzeste Pfad Muster werden mit demselben LAST_NODE () zusammengeführt.

## <a name="graph-path-order"></a>Diagramm Pfad-Reihenfolge
Die Reihenfolge der Diagramm Pfade bezieht sich auf die Reihenfolge der Daten im Ausgabepfad. Die Ausgabepfad Reihenfolge beginnt immer mit dem nicht rekursiven Teil des Musters, gefolgt von den Knoten/Rändern, die im rekursiven Teil angezeigt werden. Die Reihenfolge, in der das Diagramm während der Abfrageoptimierung/-Ausführung durchlaufen wird, hat nichts mit der in der Ausgabe gedruckten Reihenfolge zu tun. Ebenso wirkt sich die Richtung des Pfeils im rekursiven Muster nicht auf die Diagramm Pfad Reihenfolge aus. 

## <a name="graph-path-aggregate-functions"></a>Diagramm Pfad-Aggregatfunktionen
Da die Knoten und Kanten, die an einem beliebigen Längen Muster beteiligt sind, eine Auflistung (der Knoten (n) und die in diesem Pfad Traversierung (n) zurückgeben) können die Benutzer die Attribute nicht direkt mithilfe der herkömmlichen TableName. AttributeName-Syntax projizieren. Für Abfragen, bei denen es erforderlich ist, Attributwerte aus den zwischen Knoten-oder edgetabellen zu projizieren, verwenden Sie im durchsuchten Pfad die folgenden Diagramm Pfad Aggregatfunktionen: STRING_AGG, LAST_VALUE, Sum, AVG, min, Max und count. Die allgemeine Syntax für die Verwendung dieser Aggregatfunktionen in der SELECT-Klausel lautet wie folgt:

```syntaxsql
<GRAPH_PATH_AGGREGATE_FUNCTION>(<expression> , <separator>)  <order_clause>

    <order_clause> ::=
        { WITHIN GROUP (GRAPH PATH) }

    <GRAPH_PATH_AGGREGATE_FUNCTION> ::=
          STRING_AGG
        | LAST_VALUE
        | SUM
        | COUNT
        | AVG
        | MIN
        | MAX

```

### <a name="string_agg"></a>STRING_AGG
Die STRING_AGG-Funktion nimmt einen Ausdruck und ein Trennzeichen als Eingabe an und gibt eine Zeichenfolge zurück. Benutzer können diese Funktion in der SELECT-Klausel verwenden, um Attribute von den zwischen Knoten oder Kanten im durchsuchten Pfad zu projizieren. 

### <a name="last_value"></a>LAST_VALUE
Um Attribute aus dem letzten Knoten des durchsuchten Pfades zu projizieren, kann LAST_VALUE Aggregatfunktion verwendet werden. Es ist ein Fehler, einen edgetabellenalias als Eingabe für diese Funktion bereitzustellen. es können nur Knoten Tabellennamen oder Aliase verwendet werden.

**Letzter Knoten**: der letzte Knoten verweist auf den Knoten, der zuletzt im durchsuchten Pfad angezeigt wird, unabhängig von der Richtung des Pfeils im Übereinstimmungs Prädikat. Beispiel: `MATCH(SHORTEST_PATH(n(-(e)->p)+) )`. Hier ist der letzte Knoten im Pfad der zuletzt besuchte P-Knoten. 

Der letzte Knoten ist hingegen der letzte n-te Knoten im Ausgabe Diagramm Pfad für dieses Muster: `MATCH(SHORTEST_PATH((n<-(e)-)+p))`    

### <a name="sum"></a>SUM
Diese Funktion gibt die Summe der angegebenen Knoten-/edgeattributwerte oder des Ausdrucks zurück, die im durchsuchten Pfad aufgetreten sind.

### <a name="count"></a>COUNT
Diese Funktion gibt die Anzahl der Werte zurück, die nicht NULL sind. Die Count-Funktion unterstützt den- \* Operator mit einem Knoten-oder edgetabellenalias. Ohne den Knoten-oder edgetabellenalias ist die Verwendung von \* mehrdeutig und führt zu einem Fehler.

```syntaxsql
{  COUNT( <expression> | <node_or_edge_alias>.* )  <order_clause>  }
```

### <a name="avg"></a>DURCHSCHN.
Gibt den Durchschnitt der angegebenen Knoten-/edgeattributwerte oder des Ausdrucks zurück, der im durchsuchten Pfad aufgetreten ist.

### <a name="min"></a>MIN
Gibt den minimalen Wert aus den angegebenen Knoten-/edgeattributwerten oder dem Ausdruck zurück, der im durchsuchten Pfad aufgetreten ist.

### <a name="max"></a>MAX
Gibt den maximalen Wert aus den angegebenen Knoten-/edgeattributwerten oder dem Ausdruck zurück, der im durchsuchten Pfad aufgetreten ist.

## <a name="remarks"></a>Bemerkungen  
shortest_path Funktion kann nur innerhalb von Match verwendet werden.     
LAST_NODE wird nur in shortest_path unterstützt.     
Das Auffinden von gewichtetem kürzesten Pfad wird nicht unterstützt.         
In einigen Fällen können fehlerhafte Pläne für Abfragen mit einer höheren Anzahl von Hops generiert werden, was zu höheren Abfrage Ausführungszeiten führt. Die Verwendung eines hashjoinhinweises kann hilfreich sein.    


## <a name="examples"></a>Beispiele 
Für die hier gezeigten Beispielabfragen verwenden wir die Knoten-und edgetabellen, die im [SQL Graph-Beispiel](./sql-graph-sample.md) erstellt wurden.

### <a name="a--find-shortest-path-between-2-people"></a>A.  Auffinden des kürzesten Pfads zwischen zwei Personen
 Im folgenden Beispiel finden Sie den kürzesten Pfad zwischen Jacob und Alice. Wir benötigen den Knoten "Person" und "friendof Edge", die aus dem Diagramm Beispielskript erstellt werden. 

```sql
SELECT PersonName, Friends
FROM (  
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        LAST_VALUE(Person2.name) WITHIN GROUP (GRAPH PATH) AS LastNode
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
    AND Person1.name = 'Jacob'
) AS Q
WHERE Q.LastNode = 'Alice'
```

 ### <a name="b--find-shortest-path-from-a-given-node-to-all-other-nodes-in-the-graph"></a>B.  Auffinden des kürzesten Pfads von einem bestimmten Knoten zu allen anderen Knoten im Diagramm. 
 Im folgenden Beispiel werden alle Personen gefunden, mit denen Jakob im Diagramm verbunden ist, und der kürzeste Weg von Jakob zu allen diesen Personen. 

```sql
SELECT
    Person1.name AS PersonName, 
    STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends
FROM
    Person AS Person1,
    friendOf FOR PATH AS fo,
    Person FOR PATH  AS Person2
WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
AND Person1.name = 'Jacob'
```

### <a name="c--count-the-number-of-hopslevels-traversed-to-go-from-one-person-to-another-in-the-graph"></a>C.  Zählen Sie die Anzahl der Hops/Ebenen, die durchlaufen werden, um von einer Person zu einer anderen im Diagramm zu wechseln.
 Im folgenden Beispiel wird der kürzeste Pfad zwischen Jacob und Alice gefunden, und es wird die Anzahl der Hops ausgegeben, die für den Weg von Jacob zu Alice benötigt werden. 

```sql
 SELECT PersonName, Friends, levels
FROM (  
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        LAST_VALUE(Person2.name) WITHIN GROUP (GRAPH PATH) AS LastNode,
        COUNT(Person2.name) WITHIN GROUP (GRAPH PATH) AS levels
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
    AND Person1.name = 'Jacob'
) AS Q
WHERE Q.LastNode = 'Alice'
```

### <a name="d-find-people-1-3-hops-away-from-a-given-person"></a>D: Suchen von Personen, die von einer bestimmten Person 1-3 Hops entfernt wurden
Im folgenden Beispiel wird der kürzeste Pfad zwischen Jacob und allen Personen, mit denen er verbunden ist, im Graph 1-3-hophop gefunden. 

```sql
SELECT
    Person1.name AS PersonName, 
    STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends
FROM
    Person AS Person1,
    friendOf FOR PATH AS fo,
    Person FOR PATH  AS Person2
WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2){1,3}))
AND Person1.name = 'Jacob'
```

### <a name="e-find-people-exactly-2-hops-away-from-a-given-person"></a>E. Suchen von Personen genau 2 Hops von einer bestimmten Person
Im folgenden Beispiel wird der kürzeste Pfad zwischen Jacob und Personen, die genau 2 Hops sind, im Diagramm gefunden. 

```sql
SELECT PersonName, Friends
FROM (
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        COUNT(Person2.name) WITHIN GROUP (GRAPH PATH) AS levels
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2){1,3}))
    AND Person1.name = 'Jacob'
) Q
WHERE Q.levels = 2
```

## <a name="see-also"></a>Weitere Informationen  
 [Match (SQL-Graph)](../../t-sql/queries/match-sql-graph.md)    
 [CREATE TABLE &#40;SQL-Graph&#41;](../../t-sql/statements/create-table-sql-graph.md)   
 [INSERT (SQL-Graph)](../../t-sql/statements/insert-sql-graph.md)]  
 [Graph Processing with SQL Server 2017 (Graph-Verarbeitung mit SQL Server-2017)](../../relational-databases/graphs/sql-graph-overview.md)     
 
