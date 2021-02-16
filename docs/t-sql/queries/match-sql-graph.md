---
description: MATCH (Transact-SQL)
title: MATCH (SQL-Graph) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- MATCH
- MATCH_TSQL
- SHORTEST_PATH
dev_langs:
- TSQL
helpviewer_keywords:
- MATCH statement [SQL Server], SQL graph
- SQL graph, MATCH statement
- Shortest Path, shortest_path
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d55511e11750d29184124aa25b00a952eaf20ae7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270517"
---
# <a name="match-transact-sql"></a>MATCH (Transact-SQL)
[!INCLUDE[SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

  Gibt eine Suchbedingung für einen Graph an. MATCH kann in der SELECT-Anweisung nur mit dem Diagrammknoten und Edgetabellen als Teil der WHERE-Klausel verwendet werden. 
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
MATCH (<graph_search_pattern>)

<graph_search_pattern>::=
  {  
      <simple_match_pattern> 
    | <arbitrary_length_match_pattern>  
    | <arbitrary_length_match_last_node_predicate> 
  }

<simple_match_pattern>::=
  {
      LAST_NODE(<node_alias>) | <node_alias>   { 
          { <-( <edge_alias> )- } 
        | { -( <edge_alias> )-> }
        <node_alias> | LAST_NODE(<node_alias>)
        } 
  }
  [ { AND } { ( <simple_match_pattern> ) } ]
  [ ,...n ]

<node_alias> ::=
  node_table_name | node_table_alias 

<edge_alias> ::=
  edge_table_name | edge_table_alias


<arbitrary_length_match_pattern>  ::=
  { 
    SHORTEST_PATH( 
      <arbitrary_length_pattern> 
      [ { AND } { <arbitrary_length_pattern> } ] 
      [ ,…n] 
    )
  } 

<arbitrary_length_match_last_node_predicate> ::=
  {  LAST_NODE( <node_alias> ) = LAST_NODE( <node_alias> ) }


<arbitrary_length_pattern> ::=
    {  LAST_NODE( <node_alias> )   | <node_alias>
     ( <edge_first_al_pattern> [<edge_first_al_pattern>…,n] )
     <al_pattern_quantifier> 
  }
    |  ( {<node_first_al_pattern> [<node_first_al_pattern> …,n] )
        <al_pattern_quantifier> 
        LAST_NODE( <node_alias> ) | <node_alias> 
 }
    
<edge_first_al_pattern> ::=
  { (  
        { -( <edge_alias> )->   } 
      | { <-( <edge_alias> )- } 
      <node_alias>
      ) 
  } 

<node_first_al_pattern> ::=
  { ( 
      <node_alias> 
        { <-( <edge_alias> )- } 
      | { -( <edge_alias> )-> }
      ) 
  } 


<al_pattern_quantifier> ::=
  {
        +
      | { 1 , n }
  }

n -  positive integer only.
 
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*graph_search_pattern*  
Gibt das Suchmuster oder den Pfad an, das bzw. der im Graph durchlaufen werden soll. Dieses Muster verwenden eine ASCII-Syntax, um den Pfad im Graph zu durchlaufen. Dieses Muster läuft in der Richtung des angegebenen Pfeils über einen Edge von einem Knoten zum anderen. Edge-Namen und -Aliase werden in Klammern bereitgestellt. Namen oder Aliase von Knoten werden an den beiden Enden des Pfeils angezeigt. Der Pfeil kann im Muster in beide Richtungen zeigen.

*node_alias*  
Name oder Alias einer Knotentabelle, die in der FROM-Klausel angegeben ist.

*edge_alias*  
Name oder Alias einer Edgetabelle, die in der FROM-Klausel angegeben ist.

*SHORTEST_PATH*   
Die Funktion wird verwendet, um den kürzesten Weg zwischen zwei gegebenen Knoten in einem Graph oder zwischen einem bestimmten Knoten und allen anderen Knoten in einem Graph zu finden. Als Eingabe dient ein beliebiges Längenmuster, das in einem Graph wiederholt durchsucht wird. 

*arbitrary_length_match_pattern*  
Gibt die Knoten und Edges an, die wiederholt durchlaufen werden müssen, bis der gewünschte Knoten erreicht ist oder bis die im Muster angegebene maximale Anzahl von Iterationen erreicht ist. 

*al_pattern_quantifier*   
Das beliebige Längenmuster verwendet Quantifizierer für reguläre Ausdrücke, um die Anzahl der Wiederholungen eines bestimmten Suchmusters festzulegen. Die unterstützten Suchmusterquantifizierer sind:   
* **+** : Das Muster 1 oder mehrmals wiederholen. Beenden, sobald der kürzeste Pfad gefunden wird.    
* **{1,n}** : Das Muster 1 bis „n“ Male wiederholen. Beenden, sobald der kürzeste Pfad gefunden wird.     

## <a name="remarks"></a>Bemerkungen  
Die Knotennamen können in MATCH wiederholt werden.  D.h., ein Knoten kann beliebig oft in der gleichen Abfrage durchlaufen werden.  
Der Name von einem Edge kann nicht in MATCH wiederholt werden.  
Ein Edge kann in beide Richtungen zeigen, muss jedoch über eine explizite Richtung verfügen.  
Die Operatoren OR und NOT werden nicht im Muster von MATCH unterstützt. MATCH kann mithilfe von AND in der WHERE-Klausel mit anderen Ausdrücken kombiniert werden. Allerdings wird das Verbinden mit Ausdrücken mithilfe von OR oder NOT nicht unterstützt. 

## <a name="examples"></a>Beispiele  
### <a name="a--find-a-friend"></a>A.  Finden von Freunden 
 Im folgenden Beispiel werden eine Knotentabelle für Personen und eine Edgetabelle für Freunde erstellt, Daten eingefügt und dann MATCH verwendet, um die Freunde von Alice (eine Person im Graph) zu finden.

 ```sql
 -- Create person node table
 CREATE TABLE dbo.Person (ID INTEGER PRIMARY KEY, name VARCHAR(50)) AS NODE;
 CREATE TABLE dbo.friend (start_date DATE) AS EDGE;

 -- Insert into node table
 INSERT INTO dbo.Person VALUES (1, 'Alice');
 INSERT INTO dbo.Person VALUES (2,'John');
 INSERT INTO dbo.Person VALUES (3, 'Jacob');

-- Insert into edge table
INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'Alice'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'John'), '9/15/2011');

INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'Alice'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'Jacob'), '10/15/2011');

INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'John'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'Jacob'), '10/15/2012');

-- use MATCH in SELECT to find friends of Alice
SELECT Person2.name AS FriendName
FROM Person Person1, friend, Person Person2
WHERE MATCH(Person1-(friend)->Person2)
AND Person1.name = 'Alice';
 ```

 ### <a name="b--find-friend-of-a-friend"></a>B.  Finden von Freundne von Freunden
 Im folgenden Beispiel wird versucht, einen Freund des Freundes von Alice zu finden. 

 ```sql
SELECT Person3.name AS FriendName 
FROM Person Person1, friend, Person Person2, friend friend2, Person Person3
WHERE MATCH(Person1-(friend)->Person2-(friend2)->Person3)
AND Person1.name = 'Alice';
 ```

### <a name="c--more-match-patterns"></a>C.  Weitere `MATCH`-Muster
 Im Folgenden werden einige weitere Möglichkeiten dargestellt, durch die ein Muster in MATCH angegeben werden kann.

 ```sql
 -- Find a friend
    SELECT Person2.name AS FriendName
    FROM Person Person1, friend, Person Person2
    WHERE MATCH(Person1-(friend)->Person2);
    
-- The pattern can also be expressed as below

    SELECT Person2.name AS FriendName
    FROM Person Person1, friend, Person Person2 
    WHERE MATCH(Person2<-(friend)-Person1);


-- Find 2 people who are both friends with same person
    SELECT Person1.name AS Friend1, Person2.name AS Friend2
    FROM Person Person1, friend friend1, Person Person2, 
         friend friend2, Person Person0
    WHERE MATCH(Person1-(friend1)->Person0<-(friend2)-Person2);
    
-- this pattern can also be expressed as below

    SELECT Person1.name AS Friend1, Person2.name AS Friend2
    FROM Person Person1, friend friend1, Person Person2, 
         friend friend2, Person Person0
    WHERE MATCH(Person1-(friend1)->Person0 AND Person2-(friend2)->Person0);
 ```
 

## <a name="see-also"></a>Weitere Informationen  
 [CREATE TABLE &#40;SQL-Graph&#41;](../../t-sql/statements/create-table-sql-graph.md)   
 [INSERT (SQL-Graph)](../../t-sql/statements/insert-sql-graph.md)]  
 [Graph Processing with SQL Server 2017 (Graph-Verarbeitung mit SQL Server-2017)](../../relational-databases/graphs/sql-graph-overview.md)  
