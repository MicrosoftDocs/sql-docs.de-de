---
title: SQL Graph-Daten Bank Beispiel | Microsoft-Dokumentation
description: Ein kurzes Beispiel, das Sie bei den ersten Schritten mit der neuen Syntax unterstützt, die in SQL Graph-Datenbank eingeführt wurde.
ms.date: 04/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: ''
ms.topic: language-reference
helpviewer_keywords:
- SQL graph
- SQL graph, tsql reference
ms.assetid: ''
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3ec041d799407434cb7fa374c6808ef3113e3153
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100351430"
---
# <a name="create-a-graph-database-and-run-some-pattern-matching-queries-using-t-sql"></a>Erstellen einer Graph-Datenbank und Ausführen einiger Muster Vergleichs Abfragen mit T-SQL

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi.md)]

Dieses Beispiel stellt ein [!INCLUDE[tsql-md](../../includes/tsql-md.md)] Skript zum Erstellen einer Graph-Datenbank mit Knoten und Kanten bereit und verwendet dann die neue Match-Klausel, um einige Muster zu vergleichen und das Diagramm zu durchlaufen. Dieses Beispielskript funktioniert sowohl für Azure SQL-Datenbank als auch für [!INCLUDE[sssql17](../../includes/sssql17-md.md)]  

## <a name="sample-schema"></a>Beispiel Schema

Dieses Beispiel erstellt ein Diagramm Schema, wie in Abbildung 1 gezeigt, für ein hypothetisches soziales Netzwerk mit Personen-, Restaurant-und Stadt Knoten. Diese Knoten sind über Freunde, likes, livesin und locatcher-Kanten miteinander verbunden.

![Das Diagramm zeigt ein Beispiel Schema mit Restaurant-, Stadt-, Personen Knoten und livesin, locatcher, gefällt Edge.](../../relational-databases/graphs/media/person-cities-restaurants-tables.png "SQL Graph-Daten Bank Beispiel")  
Abbildung 1: Beispiel Schema mit "Restaurant", "City", "Person Nodes" und "livesin", "loingedin", gefällt Kanten

## <a name="sample-script"></a>Beispielskript

```
-- Create a graph demo database
IF NOT EXISTS (SELECT * FROM sys.databases WHERE NAME = 'graphdemo')
    CREATE DATABASE GraphDemo;
GO

USE GraphDemo;
GO

-- Create NODE tables
CREATE TABLE Person (
  ID INTEGER PRIMARY KEY,
  name VARCHAR(100)
) AS NODE;

CREATE TABLE Restaurant (
  ID INTEGER NOT NULL,
  name VARCHAR(100),
  city VARCHAR(100)
) AS NODE;

CREATE TABLE City (
  ID INTEGER PRIMARY KEY,
  name VARCHAR(100),
  stateName VARCHAR(100)
) AS NODE;

-- Create EDGE tables. 
CREATE TABLE likes (rating INTEGER) AS EDGE;
CREATE TABLE friendOf AS EDGE;
CREATE TABLE livesIn AS EDGE;
CREATE TABLE locatedIn AS EDGE;

-- Insert data into node tables. Inserting into a node table is same as inserting into a regular table
INSERT INTO Person (Id, name)
    VALUES (1, 'John')
         , (2, 'Mary')
         , (3, 'Alice')
         , (4, 'Jacob')
         , (5, 'Julie');

INSERT INTO Restaurant (Id, name, city)
    VALUES (1, 'Taco Dell','Bellevue')
         , (2, 'Ginger and Spice','Seattle')
         , (3, 'Noodle Land', 'Redmond');

INSERT INTO City (Id, name, stateName)
    VALUES (1,'Bellevue','wa')
         , (2,'Seattle','wa')
         , (3,'Redmond','wa');

-- Insert into edge table. While inserting into an edge table,
-- you need to provide the $node_id from $from_id and $to_id columns.
/* Insert which restaurants each person likes */
INSERT INTO likes 
    VALUES ((SELECT $node_id FROM Person WHERE ID = 1), (SELECT $node_id FROM Restaurant WHERE ID = 1), 9)
         , ((SELECT $node_id FROM Person WHERE ID = 2), (SELECT $node_id FROM Restaurant WHERE ID = 2), 9)
         , ((SELECT $node_id FROM Person WHERE ID = 3), (SELECT $node_id FROM Restaurant WHERE ID = 3), 9)
         , ((SELECT $node_id FROM Person WHERE ID = 4), (SELECT $node_id FROM Restaurant WHERE ID = 3), 9)
         , ((SELECT $node_id FROM Person WHERE ID = 5), (SELECT $node_id FROM Restaurant WHERE ID = 3), 9);

/* Associate in which city live each person*/
INSERT INTO livesIn 
    VALUES ((SELECT $node_id FROM Person WHERE ID = 1), (SELECT $node_id FROM City WHERE ID = 1))
         , ((SELECT $node_id FROM Person WHERE ID = 2), (SELECT $node_id FROM City WHERE ID = 2))
         , ((SELECT $node_id FROM Person WHERE ID = 3), (SELECT $node_id FROM City WHERE ID = 3))
         , ((SELECT $node_id FROM Person WHERE ID = 4), (SELECT $node_id FROM City WHERE ID = 3))
         , ((SELECT $node_id FROM Person WHERE ID = 5), (SELECT $node_id FROM City WHERE ID = 1));

/* Insert data where the restaurants are located */
INSERT INTO locatedIn 
    VALUES ((SELECT $node_id FROM Restaurant WHERE ID = 1), (SELECT $node_id FROM City WHERE ID =1))
         , ((SELECT $node_id FROM Restaurant WHERE ID = 2), (SELECT $node_id FROM City WHERE ID =2))
         , ((SELECT $node_id FROM Restaurant WHERE ID = 3), (SELECT $node_id FROM City WHERE ID =3));

/* Insert data into the friendOf edge */
INSERT INTO friendOf 
    VALUES ((SELECT $NODE_ID FROM Person WHERE ID = 1), (SELECT $NODE_ID FROM Person WHERE ID = 2))
         , ((SELECT $NODE_ID FROM Person WHERE ID = 2), (SELECT $NODE_ID FROM Person WHERE ID = 3))
         , ((SELECT $NODE_ID FROM Person WHERE ID = 3), (SELECT $NODE_ID FROM Person WHERE ID = 1))
         , ((SELECT $NODE_ID FROM Person WHERE ID = 4), (SELECT $NODE_ID FROM Person WHERE ID = 2))
         , ((SELECT $NODE_ID FROM Person WHERE ID = 5), (SELECT $NODE_ID FROM Person WHERE ID = 4));


-- Find Restaurants that John likes
SELECT Restaurant.name
FROM Person, likes, Restaurant
WHERE MATCH (Person-(likes)->Restaurant)
AND Person.name = 'John';

-- Find Restaurants that John's friends like
SELECT Restaurant.name 
FROM Person person1, Person person2, likes, friendOf, Restaurant
WHERE MATCH(person1-(friendOf)->person2-(likes)->Restaurant)
AND person1.name='John';

-- Find people who like a restaurant in the same city they live in
SELECT Person.name
FROM Person, likes, Restaurant, livesIn, City, locatedIn
WHERE MATCH (Person-(likes)->Restaurant-(locatedIn)->City AND Person-(livesIn)->City);
```

## <a name="clean-up"></a>Bereinigen  
Bereinigen Sie das für das Beispiel erstellte Schema und die Datenbank.

```
USE graphdemo;
go

DROP TABLE IF EXISTS likes;
DROP TABLE IF EXISTS Person;
DROP TABLE IF EXISTS Restaurant;
DROP TABLE IF EXISTS City;
DROP TABLE IF EXISTS friendOf;
DROP TABLE IF EXISTS livesIn;
DROP TABLE IF EXISTS locatedIn;

USE master;
go
DROP DATABASE graphdemo;
go
```

## <a name="script-explanation"></a>Erläuterung des Skripts  
Dieses Skript verwendet die neue T-SQL-Syntax, um Knoten-und Edge-Tabellen zu erstellen. Zeigt, wie Daten mithilfe der-Anweisung in Knoten-und edgetabellen eingefügt werden `INSERT` und wie die `MATCH` -Klausel für den Musterabgleich und die Navigation verwendet wird.

|Get-Help    |Notizen
|---  |---  |
|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-sql-graph.md)  |Diagramm Knoten oder Edge-Tabelle erstellen  |
|[INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-sql-graph.md)  |Einfügen in eine Knoten-oder Kanten Tabelle  |
|[Vergleichen &#40;Transact-SQL-&#41;](../../t-sql/queries/match-sql-graph.md)  |Verwenden von Match zum Abgleichen eines Musters oder durchlaufen des Diagramms  |
