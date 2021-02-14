---
description: SQL Graph-Architektur
title: SQL Graph-Architektur | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: language-reference
helpviewer_keywords:
- SQL graph
- SQL graph, architecture
ms.assetid: ''
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ab08692b1e221e589cc6283e2800a695044f79b7
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2021
ms.locfileid: "100351417"
---
# <a name="sql-graph-architecture"></a>SQL Graph-Architektur  
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi.md)]

Erfahren Sie, wie SQL Graph entworfen wird. Wenn Sie die Grundlagen kennen, wird es einfacher, andere SQL Graph-Artikel zu verstehen.
 
## <a name="sql-graph-database"></a>SQL Graph-Datenbank
Benutzer können ein Diagramm pro Datenbank erstellen. Bei einem Diagramm handelt es sich um eine Sammlung von Knoten-und Kanten Tabellen. Knoten-oder edgetabellen können unter jedem beliebigen Schema in der Datenbank erstellt werden, Sie gehören jedoch zu einem logischen Diagramm. Eine Knoten Tabelle ist eine Sammlung ähnlicher Knoten Typen. Beispielsweise enthält eine Person-Knoten Tabelle alle Person-Knoten, die zu einem Diagramm gehören. Entsprechend ist eine Edge-Tabelle eine Auflistung ähnlicher Kanten Typen. Beispielsweise enthält eine "Friends Edge"-Tabelle alle Kanten, die eine Person mit einer anderen Person verbinden. Da Knoten und Kanten in Tabellen gespeichert werden, werden die meisten Vorgänge, die für reguläre Tabellen unterstützt werden, in Knoten-oder edgetabellen unterstützt. 
 
 
![Diagramm, das die SQL Graph-Datenbankarchitektur anzeigt.](../../relational-databases/graphs/media/sql-graph-architecture.png "SQL Graph-Datenbankarchitektur")   

Abbildung 1: SQL Graph-Datenbankarchitektur
 
## <a name="node-table"></a>Knoten Tabelle
Eine Knoten Tabelle stellt eine Entität in einem Diagramm Schema dar. Jedes Mal, wenn eine Knoten Tabelle zusammen mit den benutzerdefinierten Spalten erstellt wird, wird eine implizite `$node_id` Spalte erstellt, die einen bestimmten Knoten in der Datenbank eindeutig identifiziert. Die Werte in `$node_id` werden automatisch generiert und sind eine Kombination aus `object_id` dieser Knoten Tabelle und einem intern generierten bigint-Wert. Wenn jedoch die `$node_id` Spalte ausgewählt ist, wird ein berechneter Wert in Form einer JSON-Zeichenfolge angezeigt. Außerdem `$node_id` ist eine Pseudo Spalte, die einem internen Namen mit hexadezimal Zeichenfolge zugeordnet ist. Wenn Sie `$node_id` aus der Tabelle auswählen, wird der Spaltenname als angezeigt `$node_id_<hex_string>` . Die Verwendung von Pseudo Spaltennamen in Abfragen ist die empfohlene Methode zum Abfragen der internen `$node_id` Spalte, und die Verwendung des internen Namens mit hexadezimaler Zeichenfolge sollte vermieden werden.

Es wird empfohlen, dass Benutzer zum Zeitpunkt der Erstellung der Knoten Tabelle eine Unique-Einschränkung oder einen Index für die `$node_id` Spalte erstellen, wenn Sie jedoch nicht erstellt wird, wird automatisch ein eindeutiger Standard Index erstellt, der einen eindeutigen, nicht gruppierten Index aufweist. Allerdings wird ein beliebiger Index für eine Diagramm Pseudo Spalte für die zugrunde liegenden internen Spalten erstellt. Das heißt, ein Index, der für die Spalte erstellt wird `$node_id` , wird in der internen `graph_id_<hex_string>` Spalte angezeigt.   


## <a name="edge-table"></a>Edge-Tabelle
Eine Edge-Tabelle stellt eine Beziehung in einem Diagramm dar. Kanten werden immer umgeleitet und verbinden zwei Knoten. Eine Rahmen Tabelle ermöglicht es Benutzern, m:n-Beziehungen im Diagramm zu modellieren. In einer Rahmen Tabelle sind möglicherweise benutzerdefinierte Attribute enthalten. Jedes Mal, wenn eine Edge-Tabelle zusammen mit den benutzerdefinierten Attributen erstellt wird, werden drei implizite Spalten in der Edge-Tabelle erstellt:

|Spaltenname    |BESCHREIBUNG  |
|---   |---  |
|`$edge_id`   |Identifiziert eine bestimmte Kante in der Datenbank eindeutig. Dabei handelt es sich um eine generierte Spalte, bei der es sich um eine Kombination aus object_id der Kanten Tabelle und einem intern generierten bigint-Wert handelt. Wenn jedoch die `$edge_id` Spalte ausgewählt ist, wird ein berechneter Wert in Form einer JSON-Zeichenfolge angezeigt. `$edge_id` ist eine Pseudo Spalte, die einem internen Namen mit hexadezimal Zeichenfolge zugeordnet ist. Wenn Sie `$edge_id` aus der Tabelle auswählen, wird der Spaltenname als angezeigt `$edge_id_\<hex_string>` . Die Verwendung von Pseudo Spaltennamen in Abfragen ist die empfohlene Methode zum Abfragen der internen `$edge_id` Spalte, und die Verwendung des internen Namens mit hexadezimaler Zeichenfolge sollte vermieden werden. |
|`$from_id`   |Speichert den `$node_id` des Knotens, von dem der Edge stammt.  |
|`$to_id`   |Speichert den `$node_id` des Knotens, an dem der Edge beendet wird. |

Die Knoten, für die ein bestimmter Edge eine Verbindung herstellen kann, werden von den in die Spalten und eingefügten Daten gesteuert `$from_id` `$to_id` . In der ersten Version ist es nicht möglich, Einschränkungen für die Edge-Tabelle zu definieren, um eine Verbindung mit zwei Knoten Typen einzuschränken. Das heißt, dass eine Kante alle zwei Knoten im Diagramm unabhängig von ihren Typen verbinden kann.

Ähnlich wie in der- `$node_id` Spalte wird empfohlen, dass Benutzer zum Zeitpunkt der Erstellung der Edge-Tabelle einen eindeutigen Index oder eine Einschränkung für die `$edge_id` Spalte erstellen. Wenn jedoch keine erstellt wird, wird automatisch ein eindeutiger Standard Index für diese Spalte erstellt. Allerdings wird ein beliebiger Index für eine Diagramm Pseudo Spalte für die zugrunde liegenden internen Spalten erstellt. Das heißt, ein Index, der für die Spalte erstellt wird `$edge_id` , wird in der internen `graph_id_<hex_string>` Spalte angezeigt. Außerdem wird empfohlen, für OLTP-Szenarien, dass Benutzer einen Index `$from_id` für (, `$to_id` )-Spalten erstellen, um schnellere Suchvorgänge in der Richtung des Edge durchführen zu können.  

Abbildung 2 zeigt, wie Knoten-und Kanten Tabellen in der-Datenbank gespeichert werden. 

![Diagramm, das die Darstellung der Knoten-und Kanten Tabelle zeigt.](../../relational-databases/graphs/media/person-friends-tables.png "Knoten "Person" und "Friends Edge Tables"")   

Abbildung 2: Darstellung von Knoten-und Kanten Tabellen



## <a name="metadata"></a>Metadaten
Verwenden Sie diese metadatenansichten, um die Attribute einer Knoten-oder Kanten Tabelle anzuzeigen.
 
### <a name="systables"></a>sys.tables
Die folgenden neuen bittype-Spalten werden zu sys hinzugefügt. Spielern. Wenn `is_node` auf 1 festgelegt ist, gibt dies an, dass es sich bei der Tabelle um eine Knoten Tabelle handelt. wenn auf 1 festgelegt ist, gibt dies an, dass `is_edge` die Tabelle eine edgetabelle ist.
 
|Spaltenname |Datentyp |BESCHREIBUNG |
|--- |---|--- |
|is_node |bit |1 = Dies ist eine Knoten Tabelle. |
|is_edge |bit |1 = Dies ist eine Edge-Tabelle. |
 
### <a name="syscolumns"></a>sys.columns
Die `sys.columns` Sicht enthält zusätzliche Spalten `graph_type` und `graph_type_desc` , die den Typ der Spalte in Knoten-und Kanten Tabellen angeben.
 
|Spaltenname |Datentyp |BESCHREIBUNG |
|--- |---|--- |
|graph_type |INT |Interne Spalte mit einer Gruppe von Werten. Die Werte liegen zwischen 1-8 für Diagramm Spalten und NULL für andere.  |
|graph_type_desc |nvarchar(60)  |interne Spalte mit einer Gruppe von Werten |
 
In der folgenden Tabelle sind die gültigen Werte für die-Spalte aufgeführt. `graph_type`

|Spaltenwert  |BESCHREIBUNG  |
|---   |---   |
|1  |GRAPH_ID  |
|2  |GRAPH_ID_COMPUTED  |
|3  |GRAPH_FROM_ID  |
|4  |GRAPH_FROM_OBJ_ID  |
|5  |GRAPH_FROM_ID_COMPUTED  |
|6  |GRAPH_TO_ID  |
|7  |GRAPH_TO_OBJ_ID  |
|8  |GRAPH_TO_ID_COMPUTED  |


`sys.columns` speichert außerdem Informationen über implizite Spalten, die in Knoten-oder edgetabellen erstellt wurden. Die folgenden Informationen können aus sys. Columns abgerufen werden, aber Benutzer können diese Spalten nicht aus einer Knoten-oder Kanten Tabelle auswählen. 

Implizite Spalten in einer Knoten Tabelle

|Spaltenname    |Datentyp  |is_hidden  |Kommentar  |
|---  |---|---|---  |
|graph_id_\<hex_string> |bigint |1  |interne `graph_id` Spalte  |
|$Node _id_\<hex_string> |NVARCHAR   |0  |Externe Knoten `node_id` Spalte  |

Implizite Spalten in einer Edge-Tabelle

|Spaltenname    |Datentyp  |is_hidden  |Kommentar  |
|---  |---|---|---  |
|graph_id_\<hex_string> |bigint |1  |interne `graph_id` Spalte  |
|$Edge _id_\<hex_string> |NVARCHAR   |0  |externe `edge_id` Spalte  |
|from_obj_id_\<hex_string>  |INT    |1  |intern von Knoten `object_id`  |
|from_id_\<hex_string>  |bigint |1  |Intern von Knoten `graph_id`  |
|$from _id_\<hex_string> |NVARCHAR   |0  |extern von Knoten `node_id`  |
|to_obj_id_\<hex_string>    |INT    |1  |intern zu Knoten `object_id`  |
|to_id_\<hex_string>    |bigint |1  |Intern zu Knoten `graph_id`  |
|$to _id_\<hex_string>   |NVARCHAR   |0  |extern zu Knoten `node_id`  |
 
### <a name="system-functions"></a>Systemfunktionen
Die folgenden integrierten Funktionen werden hinzugefügt. Diese können Benutzer dabei unterstützen, Informationen aus den generierten Spalten zu extrahieren. Beachten Sie, dass diese Methoden die Eingabe des Benutzers nicht validieren. Wenn der Benutzer einen ungültigen angibt, `sys.node_id` extrahiert die Methode den entsprechenden Teil und gibt ihn zurück. OBJECT_ID_FROM_NODE_ID nimmt z. b. einen `$node_id` als Eingabe an und gibt den object_id der Tabelle zurück, zu der dieser Knoten gehört. 
 
|Integriert   |BESCHREIBUNG  |
|---  |---  |
|OBJECT_ID_FROM_NODE_ID |Extrahieren des object_id aus einem `node_id`  |
|GRAPH_ID_FROM_NODE_ID  |Extrahieren des graph_id aus einem `node_id`  |
|NODE_ID_FROM_PARTS |Erstellen eines node_id aus einem `object_id` und einem `graph_id`  |
|OBJECT_ID_FROM_EDGE_ID |Extrahieren `object_id` aus `edge_id`  |
|GRAPH_ID_FROM_EDGE_ID  |Identität extrahieren aus `edge_id`  |
|EDGE_ID_FROM_PARTS |Konstrukt `edge_id` aus `object_id` und Identität  |



## <a name="transact-sql-reference"></a>Transact-SQL-Referenz 
Erfahren Sie mehr über die [!INCLUDE[tsql-md](../../includes/tsql-md.md)] in SQL Server und Azure SQL-Datenbank eingeführten Erweiterungen, die das Erstellen und Abfragen von Diagramm Objekten ermöglichen. Die Abfrage Spracherweiterungen helfen dabei, das Diagramm mithilfe der ASCII-Grafik Syntax abzufragen und zu durchlaufen.
 
### <a name="data-definition-language-ddl-statements"></a>Anweisungen der Data Definition Language (DDL)

|Aufgabe   |Verwandter Artikel  |Hinweise
|---  |---  |---  |
|CREATE TABLE |[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-sql-graph.md)|`CREATE TABLE` wird jetzt erweitert, um das Erstellen einer Tabelle als Knoten oder als Edge zu unterstützen. Beachten Sie, dass eine Edge-Tabelle über benutzerdefinierte Attribute verfügen kann.  |
|ALTER TABLE    |[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)|Knoten-und edgetabellen können mithilfe von auf dieselbe Weise wie eine relationale Tabelle geändert werden `ALTER TABLE` . Benutzer können benutzerdefinierte Spalten, Indizes oder Einschränkungen hinzufügen oder ändern. Das ändern interner Diagramm Spalten, wie z `$node_id` `$edge_id` . b. oder, führt jedoch zu einem Fehler.  |
|CREATE INDEX   |[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  |Benutzer können Indizes für Pseudo Spalten und benutzerdefinierte Spalten in Knoten-und Edge-Tabellen erstellen. Alle Index Typen werden unterstützt, einschließlich gruppierter und nicht gruppierter columnstore--Indizes.  |
|Edge-Einschränkungen erstellen    |[Edge-Einschränkungen &#40;Transact-SQL-&#41;](../../relational-databases/tables/graph-edge-constraints.md)  |Benutzer können jetzt Edge-Einschränkungen für Edge-Tabellen erstellen, um eine bestimmte Semantik zu erzwingen und die Datenintegrität beizubehalten.  |
|DROP TABLE |[DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)  |Knoten-und Kanten Tabellen können auf die gleiche Weise wie eine relationale Tabelle mithilfe von gelöscht werden `DROP TABLE` . Allerdings gibt es in dieser Version keine Einschränkungen, um sicherzustellen, dass keine Ränder auf einen gelöschten Knoten verweisen und das überlappende Löschen von Kanten beim Löschen einer Knoten-oder Knoten Tabelle nicht unterstützt wird. Es wird empfohlen, dass beim Löschen einer Knoten Tabelle alle Kanten, die mit den Knoten in dieser Knoten Tabelle verbunden sind, manuell abgelegt werden, um die Integrität des Diagramms aufrechtzuerhalten.  |


### <a name="data-manipulation-language-dml-statements"></a>Anweisungen der Data Manipulation Language (DML)

|Aufgabe   |Verwandter Artikel  |Hinweise
|---  |---  |---  |
|INSERT |[INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-sql-graph.md)|Das Einfügen in eine Knoten Tabelle unterscheidet sich nicht vom Einfügen in eine relationale Tabelle. Die Werte für die `$node_id` Spalte werden automatisch generiert. Der Versuch, einen Wert in die `$node_id` Spalte oder einzufügen, `$edge_id` führt zu einem Fehler. Benutzer müssen `$from_id` `$to_id` beim Einfügen in eine Rahmen Tabelle Werte für die Spalten und angeben. `$from_id` und `$to_id` sind die `$node_id` Werte der Knoten, die von einem bestimmten Edge verbunden werden.  |
|Delete | [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)|Daten aus Knoten-oder edgetabellen können auf die gleiche Weise wie aus relationalen Tabellen gelöscht werden. Allerdings gibt es in dieser Version keine Einschränkungen, um sicherzustellen, dass keine Ränder auf einen gelöschten Knoten und das überlappende Löschen von Rändern zeigen, wenn ein Knoten nicht mehr unterstützt wird. Es wird empfohlen, dass jedes Mal, wenn ein Knoten gelöscht wird, alle Verbindungs Ränder mit diesem Knoten gelöscht werden, um die Integrität des Diagramms aufrechtzuerhalten.  |
|UPDATE |[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)  |Werte in benutzerdefinierten Spalten können mithilfe der Update-Anweisung aktualisiert werden. Das Aktualisieren der internen Diagramm Spalten `$node_id` , `$edge_id` , `$from_id` und `$to_id` ist nicht zulässig.  |
|MERGE |[MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)  |`MERGE` die-Anweisung wird für eine Knoten-oder Kanten Tabelle unterstützt.  |


### <a name="query-statements"></a>Abfrage Anweisungen

|Aufgabe   |Verwandter Artikel  |Notizen
|---  |---  |---  |
|SELECT |[SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)|Knoten und Kanten werden intern als Tabellen gespeichert. Daher werden die meisten Vorgänge, die für eine Tabelle in SQL Server oder Azure SQL-Datenbank unterstützt werden, in den Knoten-und Edge-Tabellen unterstützt.  |
|MATCH  | [Vergleichen &#40;Transact-SQL-&#41;](../../t-sql/queries/match-sql-graph.md)|Die integrierte Übereinstimmung wird eingeführt, um Muster Vergleiche und durchlaufen des Diagramms zu unterstützen.  |



## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme  
In dieser Version gibt es bestimmte Einschränkungen für Knoten-und Kanten Tabellen:
* Lokale oder globale temporäre Tabellen können keine Knoten-oder edgetabellen sein.
* Tabellentypen und Tabellen Variablen können nicht als Knoten-oder Kanten Tabelle deklariert werden. 
* Knoten-und Kanten Tabellen können nicht als Temporale Tabellen mit System Versionsverwaltung erstellt werden.   
* Knoten-und Kanten Tabellen können keine Speicher optimierten Tabellen sein.  
* Benutzer können die `$from_id` -und- `$to_id` Spalten eines Edge nicht mithilfe der Update-Anweisung aktualisieren. Zum Aktualisieren der Knoten, die von Edge verbunden werden, müssen Benutzer den neuen Edge einfügen, der auf neue Knoten zeigt, und die vorherige löschen.
* Datenbankübergreifende Abfragen für Graph-Objekte werden nicht unterstützt. 


## <a name="next-steps"></a>Nächste Schritte
Informationen zu den ersten Schritten mit der neuen Syntax finden Sie unter [SQL Graph Database-Sample](./sql-graph-sample.md)
 

