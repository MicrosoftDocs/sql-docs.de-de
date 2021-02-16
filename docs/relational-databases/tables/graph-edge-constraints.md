---
description: Graph-Edgeeinschränkungen
title: Graph-Edgeeinschränkungen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/09/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- edge constraints [SQL Server]
- CONNECTION constraints
- edge constraints [Azure SQL Database]
- graph edge constraints
- SQL Graph
author: shkale-msft
ms.author: shkale
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 972e0a1cf391f321426398e94997740712580305
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346712"
---
# <a name="edge-constraints"></a>Edgeeinschränkungen

[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

Edgeeinschränkungen können verwendet werden, um die Datenintegrität und eine spezifische Semantik in den Edgetabellen in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Graph-Datenbank zu erzwingen.

## <a name="edge-constraints"></a><a name="Connection"></a> Edgeeinschränkungen

Im ersten Release der Graph-Features wurde für die Endpunkt des Edge durch die Edgetabellen nichts erzwungen. Anders gesagt: Ein Edge in einer Graph-Datenbank konnte unabhängig vom Knotentyp eine Verbindung von jedem Knoten mit jedem Knoten herstellen.

In diesem Release werden Edgeeinschränkungen eingeführt, mit denen Benutzer Einschränkungen zu ihren Edgetabellen hinzufügen und damit eine bestimmte Semantik erzwingen und die Datenintegrität sicherstellen können. Wenn einer Edgetabelle mit Edgeeinschränkungen ein neues Edge hinzugefügt wird, erzwingt die [!INCLUDE[ssDE](../../includes/ssde-md.md)], dass die Knoten, mit denen das Edge eine Verbindung herstellen möchte, in den richtigen Knotentabellen vorhanden sein müssen. Es wird ebenfalls sichergestellt, dass ein Knoten nicht gelöscht werden kann, wenn noch von einem Edge auf ihn verwiesen wird.

### <a name="edge-constraint-clauses"></a>Edgeeinschränkungsklauseln

Jede Edgeeinschränkung besteht aus mindestens einer Edgeeinschränkungsklausel. Eine Edgeeinschränkungsklausel ist ein Paar aus FROM- und TO-Knoten, das vom angegebenen Edge verbunden werden kann.

Ein Beispiel: Sie haben die Knoten `Product` und `Customer` in Ihrem Graph und verwenden ein `Bought`-Edge, um diese Knoten zu verbinden. Die Edgeeinschränkungsklausel gibt das FROM- und TO-Knotenpaar und die Richtung des Edge an. In diesem Fall lautet die Edgeeinschränkungsklausel „`Customer` TO `Product`“. Das bedeutet Folgendes: Das Einfügen eines `Bought`-Edge von `Customer` zu `Product` ist zulässig. Es ist aber nicht möglich, ein Edge von `Product` zu `Customer` einzufügen.

- Eine Edgeeinschränkungsklausel enthält ein Paar aus FROM- und TO-Knotentabellen, in denen die Edgeeinschränkung erzwungen wird.
- Benutzer können für jede Edgeeinschränkung mehrere Edgeeinschränkungsklauseln angeben, die als Disjunktion angewendet werden.
- Wenn in einer einzelnen Edgetabelle mehrere Edgeeinschränkungen erstellt werden, müssen Edges ALLE Einschränkungen zulassen.

### <a name="indexes-on-edge-constraints"></a>Indizes in Edgeeinschränkungen

Durch Erstellen einer Edgeeinschränkung wird nicht automatisch ein entsprechender Index in den `$from_id`- und `$to_id`-Spalten der Edgetabelle erstellt. Es empfiehlt sich, manuell einen Index in einem `$from_id`,`$to_id`-Spaltenpaar zu erstellen, wenn Sie Punktsuchabfragen oder OLTP-Workloads verarbeiten.

### <a name="on-delete-referential-actions-on-edge-constraints"></a>ON DELETE: referenzielle Aktionen bei Edgeeinschränkungen
Mit kaskadierende Aktionen für eine Edgeeinschränkung können Benutzer die von der Datenbank-Engine durchzuführenden Aktionen definieren, wenn ein Benutzer die Knoten löscht, die über den angegebenen Edge verbunden werden. Die folgenden referenziellen Aktionen können definiert werden:  
*NO ACTION*   
Die Datenbank-Engine löst einen Fehler aus, wenn Sie versuchen, einen Knoten zu löschen, der über verbundene Edges verfügt.  

*CASCADE*   
Wenn ein Knoten aus der Datenbank gelöscht wird, werden auch die verbundenen Edges gelöscht.  

## <a name="working-with-edge-constraints"></a>Arbeiten mit Edgeeinschränkungen

Sie können mit [!INCLUDE[tsql](../../includes/tsql-md.md)] eine Edgeeinschränkung in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definieren. Eine Edgeeinschränkung kann nur in einer Graph-Edgetabelle definiert werden. Zum Erstellen, Löschen oder Ändern einer Edgeeinschränkung müssen Sie über die **ALTER**-Berechtigung für die Tabelle verfügen.

### <a name="create-edge-constraints"></a>Erstellen von Edgeeinschränkungen

Die folgenden Beispiele zeigen, wie eine Edgeeinschränkung für neue oder vorhandene Tabellen erstellt wird.

#### <a name="to-create-an-edge-constraint-on-a-new-edge-table"></a>So erstellen Sie eine Edgeeinschränkung in einer neuen Edgetabelle

Das folgende Beispiel erstellt eine Edgeeinschränkung in der Edgetabelle **bought**.

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      ,CustomerName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      ,ProductName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
         ,CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product) ON DELETE NO ACTION
   )
   AS EDGE;
   ```

#### <a name="defining-referential-actions-on-a-new-edge-table"></a>Definieren von referenziellen Aktionen für eine neue Edgetabelle 

Im folgenden Beispiel wird eine Edgeeinschränkung für die **gekaufte** Edgetabelle erstellt und die referenzielle Aktion DELETE CASCADE definiert. 

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      ,CustomerName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      ,ProductName VARCHAR(100)
   )
AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
         ,CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product) ON DELETE CASCADE
   )
   AS EDGE;
   ```

#### <a name="to-add-edge-constraint-to-an-existing-edge-table"></a>So fügen Sie einer vorhandenen Edgetabelle eine Edgeeinschränkung hinzu

Das folgende Beispiel verwendet ALTER TABLE, um der Edgetabelle **bought** eine Edgeeinschränkung hinzuzufügen.

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY,
      , CustomerName VARCHAR(100)
   )
   AS NODE;
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
   )
   AS EDGE;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Customer TO Product);
```  

#### <a name="create-a-new-edge-constraint-on-existing-edge-table-with-additional-edge-constraint-clauses"></a>Erstellen einer Edgeeinschränkung für eine vorhandene Edgetabelle mit zusätzlichen Edgeeinschränkungsklauseln

Das folgende Beispiel verwendet den Befehl `ALTER TABLE`, um eine neue Edgeeinschränkung mit zusätzlichen Edgeeinschränkungsklauseln in der Edgetabelle **bought** hinzuzufügen.
  
```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
   )
   AS EDGE;
-- Drop the existing edge constraint first and then create a new one.
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT;
GO
-- User ALTER TABLE to create a new edge constraint.
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Customer TO Product, Supplier TO Product);
```  

Im Beispiel oben gibt es in der Einschränkung *EC_BOUGHT1* zwei Edgeeinschränkungsklauseln: Eine stellt eine Verbindung zwischen **Customer** und **Product** her, die andere verbindet **Supplier** mit **Product**. Beide Klauseln werden in Disjunktion angewendet. Das bedeutet, dass ein Edge die Bedingung einer der beiden Klauseln erfüllen muss, um in der Edgetabelle zugelassen zu werden.

#### <a name="creating-a-new-edge-constraint-on-existing-edge-table-with-new-edge-constraint-clause"></a>Erstellen einer neuen Edgeeinschränkung für eine vorhandenen Edgetabelle mit einer neuen Edgeeinschränkungsklausel

Das folgende Beispiel verwendet den Befehl `ALTER TABLE`, um eine neue Edgeeinschränkung mit einer neuen Edgeeinschränkungsklausel für die Edgetabelle **bought** hinzuzufügen.
  
 ```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT,
         CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
   )
   AS EDGE;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT1 CONNECTION (Supplier TO Product);
 ```  

Im Beispiel oben haben wir zwei separate Edgeeinschränkungen in der Edgetabelle **bought** erstellt: *EC_BOUGHT* und *EC_BOUGHT1*. Beide Edgeeinschränkungen weisen unterschiedliche Edgeeinschränkungsklauseln auf. Wenn eine Edgetabelle über mehr als eine Edgeeinschränkung verfügt, muss ein Edge **ALLE** Edgeeinschränkungen erfüllen, um in der Tabelle zugelassen zu werden. Da kein Edge hier sowohl *EC_BOUGHT* als auch *EC_BOUGHT1* erfüllen kann, muss die Tabelle **bought** leer bleiben.

Damit Einfügungen in diese Edgetabelle funktionieren, muss entweder eine der Edgeeinschränkungen gelöscht werden, oder es müssen beide Einschränkungen gelöscht und eine neue Edgeeinschränkung erstellt werden, die beide Edgeeinschränkungsklauseln enthält.

```sql
-- Drop the existing edge constraint first and then create a new one.
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT;
GO
ALTER TABLE bought DROP CONSTRAINT EC_BOUGHT1;
GO
ALTER TABLE bought ADD CONSTRAINT EC_BOUGHT_NEW CONNECTION (Customer TO Product, Supplier TO Product);
GO
```  

Damit ein Edge in der Edgetabelle **bought** zugelassen wird, muss er eine der Edgeeinschränkungsklauseln in der Einschränkung *EC_BOUGHT_NEW* erfüllen. Daher wird jedes Edge zugelassen, das versucht, eine Verbindung zwischen den gültigen Knoten **Customer** und **Product** bzw. **Supplier** und **Product** herzustellen.

### <a name="delete-edge-constraints"></a>Löschen von Edgeeinschränkungen

Das folgende Beispiel identifiziert zuerst den Namen der Edgeeinschränkung und löscht die Einschränkung dann.  
  
```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   ) AS NODE;
GO
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product)
    )
    AS EDGE;
GO
-- Return the name of edge constraint.
SELECT name  
   FROM sys.edge_constraints  
   WHERE type = 'EC' AND parent_object_id = OBJECT_ID('bought');  
GO  
-- Delete the primary key constraint.  
ALTER TABLE bought
DROP CONSTRAINT EC_BOUGHT;
```  

### <a name="modify-edge-constraints"></a>Ändern von Edgeeinschränkungen

Um eine Edgeeinschränkung mit Transact-SQL ändern zu können, müssen Sie zuerst die vorhandene Edgeeinschränkung löschen und sie dann mit der neuen Definition neu erstellen.


### <a name="view-edge-constraints"></a>Anzeigen von Edgeeinschränkungen

[!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Weitere Informationen finden Sie unter [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).

Das Beispiel gibt alle Edgeeinschränkungen und ihre Eigenschaften für die Edgetabelle `bought` in der tempdb-Datenbank zurück.  

```sql
-- CREATE node and edge tables
CREATE TABLE Customer
   (
      ID INTEGER PRIMARY KEY
      , CustomerName VARCHAR(100)
   )
   AS NODE;
GO
CREATE TABLE Supplier
   (
      ID INTEGER PRIMARY KEY
      , SupplierName VARCHAR(100)
   )
   AS NODE;
   GO
CREATE TABLE Product
   (
      ID INTEGER PRIMARY KEY
      , ProductName VARCHAR(100)
   )
   AS NODE;

-- CREATE edge table with edge constraints.
CREATE TABLE bought
   (
      PurchaseCount INT
      , CONSTRAINT EC_BOUGHT CONNECTION (Customer TO Product, Supplier TO Product)
   )
   AS EDGE;

-- Query sys.edge_constraints and sys.edge_constraint_clauses to view
-- edge constraint properties.
SELECT
   EC.name AS edge_constraint_name
   , OBJECT_NAME(EC.parent_object_id) AS edge_table_name
   , OBJECT_NAME(ECC.from_object_id) AS from_node_table_name
   , OBJECT_NAME(ECC.to_object_id) AS to_node_table_name
   , is_disabled
   , is_not_trusted
FROM sys.edge_constraints EC
   INNER JOIN sys.edge_constraint_clauses ECC
   ON EC.object_id = ECC.object_id
WHERE EC.parent_object_id = object_id('bought');
```  

## <a name="related-tasks"></a>Zugehörige Aufgaben

[CREATE TABLE (SQL-Graph)](../../t-sql/statements/create-table-sql-graph.md)  
[ALTER TABLE table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)  

Weitere Informationen zur Graphtechnologie in SQL Server finden Sie unter [Graphverarbeitung mit SQL Server und Azure SQL-Datenbank](../graphs/sql-graph-overview.md).
