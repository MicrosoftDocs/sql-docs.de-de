---
description: Graph-Verarbeitung mit SQL Server und Azure SQL-Datenbank
title: Graph-Verarbeitung
titleSuffix: SQL Server and Azure SQL Database
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: language-reference
helpviewer_keywords:
- SQL graph
- SQL graph, overview
ms.assetid: ''
author: shkale-msft
ms.author: shkale
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8c7dfeaa67e0ede78b0a891409be3211e1fa8d2e
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100351424"
---
# <a name="graph-processing-with-sql-server-and-azure-sql-database"></a>Graph-Verarbeitung mit SQL Server und Azure SQL-Datenbank
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bietet Graph-Datenbankfunktionen zum Modellieren von m:n-Beziehungen. Die Diagramm Beziehungen sind in integriert [!INCLUDE[tsql-md](../../includes/tsql-md.md)] und erhalten die Vorteile der Verwendung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als grundlegendes Datenbankverwaltungssystem.


## <a name="what-is-a-graph-database"></a>Was ist eine Graphdatenbank?  
Eine Diagrammdatenbank ist eine Sammlung von Knoten (oder Vertices) und Edges (oder Beziehungen). Ein Knoten repräsentiert eine Entität (z.B. eine Person oder eine Organisation), und eine Kante repräsentiert eine Beziehung zwischen den beiden Knoten, die durch die Kante verbunden sind (z.B. „Gefällt mir“-Markierungen oder Freunde). Sowohl Knoten als auch Kanten können Eigenschaften zugeordnet sein. Folgende Features machen eine Graphdatenbank einmalig:  
-    Kanten oder Beziehungen sind Entitäten der ersten Klasse in einer Graphdatenbank, denen Attribute oder Eigenschaften zugeordnet sein können. 
-    Eine einzelne Kante kann flexibel mehrere Knoten in einer Graphdatenbank verbinden.
-    Musterabgleiche und Navigationsabfragen über mehrere Hops lassen sich ganz einfach ausdrücken.
-    Transitive Abschlüsse und polymorphe Abfragen lassen sich ebenfalls sehr einfach ausdrücken.

## <a name="when-to-use-a-graph-database"></a>Verwendung einer Graph-Datenbank

Eine relationale Datenbank kann eine beliebige Diagramm Datenbank erreichen. Eine Graph-Datenbank erleichtert jedoch das Ausdrücken bestimmter Arten von Abfragen. Mit bestimmten Optimierungen können auch bestimmte Abfragen besser funktionieren. Die Entscheidung, ob Sie eine relationale Datenbank oder eine Graph-Datenbank auswählen, basiert auf den folgenden Faktoren:  
-    Ihre Anwendung verfügt über hierarchische Daten. Der hierarchyid-Datentyp kann verwendet werden, um Hierarchien zu implementieren. es gelten jedoch einige Einschränkungen. Beispielsweise ist es nicht möglich, mehrere übergeordnete Elemente für einen Knoten zu speichern.
-    Ihre Anwendung verfügt über komplexe m:n-Beziehungen. Wenn die Anwendung weiterentwickelt wird, werden neue Beziehungen hinzugefügt.
-    Sie müssen miteinander verbundene Daten und Beziehungen analysieren.

## <a name="graph-features-introduced-in-sssql17"></a>In eingeführte Graph-Features [!INCLUDE[sssql17](../../includes/sssql17-md.md)] 
Wir beginnen damit, SQL Server Graph-Erweiterungen hinzuzufügen, um das Speichern und Abfragen von Diagramm Daten zu vereinfachen. Die folgenden Funktionen werden in der ersten Version eingeführt. 


### <a name="create-graph-objects"></a>Erstellen von Diagramm Objekten
[!INCLUDE[tsql-md](../../includes/tsql-md.md)] Erweiterungen ermöglichen es Benutzern, Knoten-oder Edge-Tabellen zu erstellen. Sowohl Knoten als auch Kanten können Eigenschaften zugeordnet werden. Da Knoten und Kanten als Tabellen gespeichert werden, werden alle Vorgänge, die für relationale Tabellen unterstützt werden, für die Knoten-oder Kanten Tabelle unterstützt. Beispiel:  

```   
CREATE TABLE Person (ID INTEGER PRIMARY KEY, Name VARCHAR(100), Age INT) AS NODE;
CREATE TABLE friends (StartDate date) AS EDGE;
```   

![Diagramm, in dem die Knoten und Kanten angezeigt werden, werden als Tabellen gespeichert.](../../relational-databases/graphs/media/person-friends-tables.png "Knoten "Person" und "Friends Edge Tables"")  
Knoten und Kanten werden als Tabellen gespeichert.  

### <a name="query-language-extensions"></a>Abfrage Spracherweiterungen  
`MATCH`Die neue Klausel wird eingeführt, um Musterabgleich und Multihop-Navigation durch das Diagramm zu unterstützen. Die `MATCH` -Funktion verwendet die ASCII-Art-Format Syntax für den Musterabgleich. Beispiel:  

```   
-- Find friends of John
SELECT Person2.Name 
FROM Person Person1, Friends, Person Person2
WHERE MATCH(Person1-(Friends)->Person2)
AND Person1.Name = 'John';
```   
 
### <a name="fully-integrated-in-ssnoversion-engine"></a>Vollständig in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Engine integriert 
Graph-Erweiterungen sind vollständig in die- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Engine integriert. Verwenden Sie die gleiche Speicher-Engine, Metadaten, den Abfrage Prozessor usw., um Diagramm Daten zu speichern und abzufragen. Abfragen über Diagramm-und relationale Daten in einer einzelnen Abfrage. Kombinieren von Diagramm Funktionen mit anderen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Technologien wie z. b. columnstore, ha, R Services usw. Die SQL Graph-Datenbank unterstützt auch alle in verfügbaren Sicherheits-und Kompatibilitäts Features [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
 
### <a name="tooling-and-ecosystem"></a>Tools und Ökosystem

Profitieren Sie von vorhandenen Tools und Ökosystem, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angeboten werden. Tools wie sichern und wiederherstellen, importieren und exportieren, bcp funktionieren einfach sofort. Andere Tools oder Dienste wie SSIS, SSRS oder Power BI funktionieren mit Diagramm Tabellen, genau so, wie Sie mit relationalen Tabellen funktionieren.

## <a name="edge-constraints"></a>Edgeeinschränkungen
Eine Edge-Einschränkung wird für eine Diagramm Rahmen Tabelle definiert und ist ein paar von Knoten Tabellen, für die ein bestimmter Edge-Typ eine Verbindung herstellen kann. Dies ermöglicht Benutzern eine bessere Kontrolle über das Diagramm Schema. Mithilfe von Edge-Einschränkungen können Benutzer den Knotentyp einschränken, mit dem eine bestimmte Kante eine Verbindung herstellen darf. 

Weitere Informationen zum Erstellen und Verwenden von Edge-Einschränkungen finden Sie unter [Edge-Einschränkungen](../../relational-databases/tables/graph-edge-constraints.md) .

## <a name="merge-dml"></a>DML zusammenführen 
Die [Merge](../../t-sql/statements/merge-transact-sql.md) -Anweisung führt INSERT-, Update-oder DELETE-Vorgänge für eine Ziel Tabelle basierend auf den Ergebnissen einer Verknüpfung mit einer Quell Tabelle aus. Beispielsweise können Sie zwei Tabellen synchronisieren, indem Sie Zeilen in einer Ziel Tabelle basierend auf den Unterschieden zwischen der Ziel Tabelle und der Quell Tabelle einfügen, aktualisieren oder löschen. Die Verwendung von Match-Prädikaten in einer MERGE-Anweisung wird jetzt in Azure SQL-Datenbank und SQL Server vNext unterstützt. Das heißt, es ist nun möglich, die aktuellen Diagramm Daten (Knoten-oder edgetabellen) mit neuen Daten zusammenzuführen, wobei die Übereinstimmungs Prädikate verwendet werden, um Diagramm Beziehungen in einer einzelnen Anweisung anstelle von separaten INSERT-, Update-und DELETE-Anweisungen anzugeben.

Weitere Informationen zur Verwendung von Match in Merge DML finden Sie unter Merge- [Anweisung](../../t-sql/statements/merge-transact-sql.md) .

## <a name="shortest-path"></a>Kürzeste Pfad
Die [SHORTEST_PATH](./sql-graph-shortest-path.md) -Funktion findet den kürzesten Pfad zwischen zwei beliebigen Knoten in einem Diagramm oder beginnend mit einem bestimmten Knoten mit allen anderen Knoten im Diagramm. Der kürzeste Pfad kann auch verwendet werden, um einen transitiven Abschluss zu finden, oder für beliebige Längen Traversale im Diagramm. 

 ## <a name="next-steps"></a>Nächste Schritte  
Lesen der [SQL Graph-Datenbankarchitektur](./sql-graph-architecture.md)
   

