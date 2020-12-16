---
title: Ändern von speicheroptimierten Tabellen | Microsoft-Dokumentation
description: Hier erfahren Sie wie die ALTER TABLE-Anweisung Schema- und Indexänderungen an speicheroptimierten Tabellen vornimmt. Außerdem erfahren Sie, wie Sie die Vorgänge ADD, DROP und ALTER in einer einzelnen Anweisung kombinieren.
ms.custom: ''
ms.date: 10/22/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 690b70b7-5be1-4014-af97-54e531997839
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: da12d83efd43c7ca6113348adee8e3bde14d9472
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465471"
---
# <a name="altering-memory-optimized-tables"></a>Ändern von speicheroptimierten Tabellen

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Änderungen des Schemas und des Indizes in speicheroptimierten Tabellen können mithilfe der Anweisung ALTER TABLE durchgeführt werden. In SQL Server 2016 und Azure SQL-Datenbank sind ALTER TABLE-Vorgänge mit speicheroptimierte Tabellen OFFLINE, d.h., dass die Tabelle während des Vorgangs nicht abgefragt werden kann. Die Datenbankanwendung kann weiterhin ausgeführt werden, und jeder Vorgang, der auf die Tabellen zugreift, wird blockiert bis der Änderungsprozess abgeschlossen ist. Es ist mögliche, mehrere ADD-, DROP- und ALTER-Vorgänge in einer einzigen ALTER TABLE-Anweisung zu kombinieren.

> [!IMPORTANT]
> Azure SQL Managed Instance unterstützt keine speicheroptimierten Tabellen in der Dienstebene „Universell“.
  
## <a name="alter-table"></a>ALTER TABLE  

Die ALTER TABLE-Syntax wird für die Änderung des Tabellenschemas sowie zum Hinzufügen, Löschen und Neuerstellen von Indizes verwendet. Indizes werden als Teil der Tabellendefinition berücksichtigt:  
  
- Die Syntax ALTER TABLE... ADD/DROP/ALTER INDEX wird nur für speicheroptimierte Tabelle unterstützt.  
  
- Wenn keine ALTER TABLE-Anweisung verwendet wird, werden die Anweisungen [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md), [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) und [PAD_INDEX](../../t-sql/statements/alter-table-index-option-transact-sql.md) nicht für Indizes auf speicheroptimierten Tabellen unterstützt.  
  
Die folgenden Änderungen werden unterstützt:  
  
- Ändern der Bucketanzahl  
  
- Hinzufügen und Entfernen eines Indexes  
  
- Ändern, Hinzufügen und Entfernen einer Spalte  
  
- Hinzufügen und Entfernen einer Einschränkung  
  
 Weitere Informationen zu ALTER TABLE-Funktionen und die vollständige Syntax finden Sie unter [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
## <a name="schema-bound-dependency"></a>Schemagebundene Abhängigkeit

 Systemintern kompilierte gespeicherte Prozeduren müssen schemagebunden sein, d. h., sie haben eine schemagebundene Abhängigkeit zu den speicheroptimierten Tabellen, auf die sie zugreifen, und zu den Spalten, auf die sie verweisen. Eine schemagebundene Abhängigkeit ist eine Beziehung zwischen zwei Entitäten, mit der verhindert wird, dass die Entität, auf die verwiesen wird, gelöscht oder in einer inkompatiblen Art geändert wird, solange die verweisende Entität vorhanden ist.  
  
 Wird beispielsweise in einer schemagebundenen systemintern kompilierten gespeicherten Prozedur auf die Spalte *c1* aus der Tabelle *mytable* verwiesen, kann die Spalte *c1* nicht gelöscht werden. Entsprechend kann, wenn es eine solche Prozedur mit einer INSERT-Anweisung ohne Spaltenliste gibt (z. B. `INSERT INTO dbo.mytable VALUES (...)`) keine Spalte aus der Tabelle gelöscht werden.  

## <a name="logging-of-alter-table-on-memory-optimized-tables"></a>Protokollieren von ALTER TABLE bei speicheroptimierten Tabellen

Bei einer speicheroptimierten Tabelle werden die meisten ALTER TABLE-Szenarios nun parallel ausgeführt und tragen zu einer Optimierung der Schreibvorgänge in das Transaktionsprotokoll bei. Zur Optimierung werden nur die Metadatenänderungen in das Transaktionsprotokoll geschrieben. Die folgenden ALTER TABLE-Vorgänge werden als Singlethread ausgeführt und sind nicht protokolloptimiert.

Die Singlethreadvorgänge schreiben in diesem Fall den gesamten Inhalt der geänderten Tabelle in das Transaktionsprotokoll. Im Folgenden finden Sie eine Liste der Singlethread-Vorgänge:

- Ändern oder Hinzufügen einer Spalte, um einen LOB-Typ (Large Object) zu verwenden: nvarchar(max), varchar(max) oder varbinary(max).

- Hinzufügen oder Löschen eines Columnstore-Indexes.

- Fast jeder Vorgang, der sich auf eine [zeilenüberragende Spalte (off-row column)](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md)auswirkt.

  - Sorgen Sie dafür, dass eine Spalte in der Zeile aus der Zeile verschoben wird.
  - Sorgen Sie dafür, dass eine Spalte außerhalb der Zeile in die Zeile verschoben wird.
  - Erstellen Sie eine neue Spalte außerhalb der Zeile.
  - *Ausnahme:* Ein Verlängern einer bereits zeilenüberragenden Spalte wird in der optimierten Weise protokolliert.
  
## <a name="examples"></a>Beispiele

Im folgenden Beispiel ändert sich die Bucketanzahl eines vorhandenen Hashindexes. Dadurch wird der Hashindex mit der neuen Bucketanzahl neu erstellt, während andere Eigenschaften des Hashindexes unverändert bleiben.  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem
       ALTER INDEX imPK_SalesOrderDetail_SalesOrderID_SalesOrderDetailID  
              REBUILD WITH (BUCKET_COUNT=67108864);  
GO
```

Im folgenden Beispiel wird eine Spalte mit der Einschränkung NOT NULL und einer DEFAULT-Definition hinzugefügt, und WITH VALUES wird verwendet, um Werte für jede vorhandene Zeile der Tabelle bereitzustellen. Ohne WITH VALUES hat jede Zeile in der neuen Spalte den Wert NULL.  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD Comment NVARCHAR(100) NOT NULL DEFAULT N'' WITH VALUES;  
GO
```

Im folgenden Beispiel wird einer vorhandenen Spalte eine Primärschlüsseleinschränkung hinzugefügt.  

```sql
CREATE TABLE dbo.UserSession (
   SessionId int not null,
   UserId int not null,
   CreatedDate datetime2 not null,
   ShoppingCartId int,
   index ix_UserId nonclustered hash (UserId) with (bucket_count=400000)
)
WITH (MEMORY_OPTIMIZED=ON, DURABILITY=SCHEMA_ONLY) ;  
GO  
  
ALTER TABLE dbo.UserSession  
       ADD CONSTRAINT PK_UserSession PRIMARY KEY NONCLUSTERED (SessionId);  
GO
```

Im folgenden Beispiel wird ein Index entfernt.  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       DROP INDEX ix_ModifiedDate;  
GO
```  

Im folgenden Beispiel wird ein Index hinzugefügt.  

```sql  
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD INDEX ix_ModifiedDate (ModifiedDate);  
GO  
```  

Im folgenden Beispiel werden mehrere Spalten hinzugefügt, mit einem Index und Einschränkungen.  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD    CustomerID int NOT NULL DEFAULT -1 WITH VALUES,  
              ShipMethodID int NOT NULL DEFAULT -1 WITH VALUES,  
              INDEX ix_Customer (CustomerID);  
GO  
```

<a name="logging-of-alter-table-on-memory-optimized-tables-124"></a>

## <a name="see-also"></a>Weitere Informationen  

[Speicheroptimierte Tabellen](./sample-database-for-in-memory-oltp.md)