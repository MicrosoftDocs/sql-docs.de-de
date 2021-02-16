---
title: Erste Schritte mit den Leistungsfeatures von SQL Server für Linux
description: Dieser Artikel bietet eine Einführung in die SQL Server-Leistungsfeatures für Linux-Benutzer, die mit SQL Server noch nicht vertraut sind. Viele dieser Beispiele funktionieren auf allen Plattformen, in diesem Artikel geht es aber um Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 60036d26-4797-4872-9a9e-3552841c61be
ms.openlocfilehash: 24794910e53f730b16975714c514eb717b41ce49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350965"
---
# <a name="walkthrough-for-the-performance-features-of-sql-server-on-linux"></a>Exemplarische Vorgehensweise für die Leistungsfeatures von SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Wenn Sie Linux-Benutzer sind, der noch nicht mit SQL Server vertraut ist, lernen Sie mit den folgenden Aufgaben einige der Leistungsfeatures kennen. Diese Features gelten nicht ausschließlich und speziell nur für Linux, aber mit diesem Artikel können Sie sich einen Überblick über die Bereiche verschaffen, die Sie näher untersuchen sollten. In jedem Beispiel wird ein Link zu der ausführlichen Dokumentation für diesen Bereich bereitgestellt.

> [!NOTE]
> In den folgenden Beispielen wird die Beispieldatenbank Adventure Works verwendet. Anweisungen zum Abrufen und Installieren dieser Beispieldatenbank finden Sie unter [Migrieren einer SQL Server-Datenbank-Instanz von Windows zu Linux mit Sicherung und Wiederherstellung](sql-server-linux-migrate-restore-database.md).

## <a name="create-a-columnstore-index"></a>Erstellen eines Columnstore-Indexes
Ein Columnstore-Index ist eine Technologie zum Speichern und Abrufen von großen Datenspeichern in einem spaltenbasierten Datenformat, das als „Columnstore“ bezeichnet wird.  

1. Fügen Sie der Tabelle „SalesOrderDetail“ einen Columnstore-Index hinzu, indem Sie die folgenden Transact-SQL-Befehle ausführen:

   ```sql
   CREATE NONCLUSTERED COLUMNSTORE INDEX [IX_SalesOrderDetail_ColumnStore]
      ON Sales.SalesOrderDetail
      (UnitPrice, OrderQty, ProductID)
   GO
   ```

2. Führen Sie die folgende Abfrage aus, die den Columnstore-Index zum Überprüfen der Tabelle verwendet:

   ```sql
   SELECT ProductID, SUM(UnitPrice) SumUnitPrice, AVG(UnitPrice) AvgUnitPrice,
      SUM(OrderQty) SumOrderQty, AVG(OrderQty) AvgOrderQty
   FROM Sales.SalesOrderDetail
      GROUP BY ProductID
      ORDER BY ProductID
   ```

3. Überprüfen Sie, ob der Columnstore-Index verwendet wurde, indem Sie die object_id für den Columnstore-Index suchen und sicherstellen, dass sie in den Nutzungsstatistiken für die Tabelle „SalesOrderDetail“ angezeigt wird:

   ```sql
   SELECT * FROM sys.indexes WHERE name = 'IX_SalesOrderDetail_ColumnStore'
   GO

   SELECT * 
   FROM sys.dm_db_index_usage_stats
      WHERE database_id = DB_ID('AdventureWorks')
      AND object_id = OBJECT_ID('AdventureWorks.Sales.SalesOrderDetail');
   ```
   
## <a name="use-in-memory-oltp"></a>Verwenden von In-Memory-OLTP
SQL Server bietet In-Memory-OLTP-Funktionen, mit denen sich die Leistung von Anwendungssystemen erheblich verbessern lässt.  Dieser Abschnitt des Evaluierungsleitfadens führt Sie durch die Schritte zum Erstellen einer speicheroptimierten Tabelle, die im Arbeitsspeicher gespeichert ist, und einer nativ kompilierten gespeicherten Prozedur, die auf die Tabelle zugreifen kann, ohne kompiliert oder interpretiert werden zu müssen.

### <a name="configure-database-for-in-memory-oltp"></a>Konfigurieren einer Datenbank für In-Memory-OLTP
1. Es wird empfohlen, zur Verwendung von In-Memory-OLTP einen Kompatibilitätsgrad von mindestens 130 für die Datenbank festzulegen.  Verwenden Sie die folgende Abfrage, um den aktuellen Kompatibilitätsgrad von AdventureWorks zu überprüfen:  

   ```sql
   USE AdventureWorks
   GO
   SELECT d.compatibility_level
   FROM sys.databases as d
     WHERE d.name = Db_Name();
   GO
   ```
   
   Aktualisieren Sie den Grad ggf. auf 130:

   ```sql
   ALTER DATABASE CURRENT
   SET COMPATIBILITY_LEVEL = 130;
   GO
   ```

2. Wenn sowohl eine datenträgerbasierte als auch eine speicheroptimierte Tabelle an einer Transaktion beteiligt ist, muss der speicheroptimierte Teil der Transaktion unbedingt auf der Transaktionsisolationsstufe SNAPSHOT ausgeführt werden.  Um diese Stufe für speicheroptimierte Tabellen in einer containerübergreifenden Transaktion zuverlässig zu erzwingen, führen Sie folgenden Code aus:

   ```sql
   ALTER DATABASE CURRENT SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON
   GO
   ```

3. Bevor Sie eine speicheroptimierte Tabelle erstellen können, müssen Sie zuerst eine speicheroptimierte FILEGROUP und einen Container für Datendateien erstellen:

   ```sql
   ALTER DATABASE AdventureWorks ADD FILEGROUP AdventureWorks_mod CONTAINS memory_optimized_data
   GO  
   ALTER DATABASE AdventureWorks ADD FILE (NAME='AdventureWorks_mod', FILENAME='/var/opt/mssql/data/AdventureWorks_mod') TO FILEGROUP AdventureWorks_mod
   GO
   ```

### <a name="create-a-memory-optimized-table"></a>Erstellen einer speicheroptimierten Tabelle
Der primäre Speicher für speicheroptimierte Tabellen ist der Hauptspeicher, daher müssen Daten im Gegensatz zu datenträgerbasierten Tabellen nicht von Datenträgern in Arbeitsspeicherpuffer eingelesen werden.  Verwenden Sie zum Erstellen einer speicheroptimierten Tabelle die Klausel MEMORY_OPTIMIZED = ON.

1. Führen Sie die folgende Abfrage aus, um die speicheroptimierte Tabelle „dbo.ShoppingCart“ zu erstellen.  Standardmäßig werden die Daten aus Gründen der Dauerhaftigkeit auf einem Datenträger gespeichert (beachten Sie, dass DURABILITY auch so festgelegt werden kann, dass nur das Schema dauerhaft gespeichert wird). 

   ```sql
   CREATE TABLE dbo.ShoppingCart ( 
   ShoppingCartId INT IDENTITY(1,1) PRIMARY KEY NONCLUSTERED,
   UserId INT NOT NULL INDEX ix_UserId NONCLUSTERED HASH WITH (BUCKET_COUNT=1000000), 
   CreatedDate DATETIME2 NOT NULL, 
   TotalPrice MONEY
   ) WITH (MEMORY_OPTIMIZED=ON) 
   GO
   ```

2. Fügen Sie einige Datensätze in die Tabelle ein:

   ```sql
   INSERT dbo.ShoppingCart VALUES (8798, SYSDATETIME(), NULL) 
   INSERT dbo.ShoppingCart VALUES (23, SYSDATETIME(), 45.4) 
   INSERT dbo.ShoppingCart VALUES (80, SYSDATETIME(), NULL) 
   INSERT dbo.ShoppingCart VALUES (342, SYSDATETIME(), 65.4) 
   ```

### <a name="natively-compiled-stored-procedure"></a>Nativ kompilierte gespeicherte Prozedur
SQL Server unterstützt nativ kompilierte gespeicherte Prozeduren, die auf speicheroptimierte Tabellen zugreifen. Die T-SQL-Anweisungen werden in Computercode kompiliert und als native DLLs gespeichert – so ermöglichen sie einen schnelleren Datenzugriff und eine effizientere Abfrageausführung als herkömmliches T-SQL.   Gespeicherte Prozeduren, die mit NATIVE_COMPILATION markiert sind, werden systemintern kompiliert. 

1. Führen Sie das folgende Skript aus, um eine nativ kompilierte gespeicherte Prozedur zu erstellen, die eine große Anzahl von Datensätzen in die Tabelle „ShoppingCart“ einfügt:


   ```sql
   CREATE PROCEDURE dbo.usp_InsertSampleCarts @InsertCount int 
    WITH NATIVE_COMPILATION, SCHEMABINDING AS 
   BEGIN ATOMIC 
   WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')

   DECLARE @i int = 0

   WHILE @i < @InsertCount 
    BEGIN 
     INSERT INTO dbo.ShoppingCart VALUES (1, SYSDATETIME() , NULL) 
     SET @i += 1 
    END
   END 
   ```
2. Fügen Sie 1.000.000 Zeilen ein:

   ```sql
   EXEC usp_InsertSampleCarts 1000000 
   ```

3. Überprüfen Sie, ob die Zeilen eingefügt wurden:

   ```sql
   SELECT COUNT(*) FROM dbo.ShoppingCart 
   ```

### <a name="learn-more-about-in-memory-oltp"></a>Weitere Informationen zu In-Memory-OLTP
Weitere Informationen zu In-Memory-OLTP finden Sie in den folgenden Themen:

- [Schnellstart 1: In-Memory-OLTP-Technologien für höhere Transact-SQL-Leistung](../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md)
- [Migrieren zu In-Memory OLTP](../relational-databases/in-memory-oltp/plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)
- [Schnellere temporäre Tabellen und Tabellenvariablen durch Speicheroptimierung](../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md)
- [Überwachung und Fehlerbehebung für die Arbeitsspeicherauslastung](../relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage.md)
- [In-Memory OLTP (In-Memory Optimization)](../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)

## <a name="use-query-store"></a>Verwenden des Abfragespeichers
Der Abfragespeicher sammelt detaillierte Leistungsinformationen zu Abfragen, Ausführungsplänen und Laufzeitstatistiken.

Der Abfragespeicher ist standardmäßig nicht aktiv und kann mit ALTER DATABASE aktiviert werden:

   ```sql
   ALTER DATABASE AdventureWorks SET QUERY_STORE = ON;
   ```

Führen Sie die folgende Abfrage aus, um Informationen zu Abfragen und Plänen im Abfragespeicher zurückzugeben: 

   ```sql
   SELECT Txt.query_text_id, Txt.query_sql_text, Pl.plan_id, Qry.*
   FROM sys.query_store_plan AS Pl
      JOIN sys.query_store_query AS Qry
         ON Pl.query_id = Qry.query_id
      JOIN sys.query_store_query_text AS Txt
         ON Qry.query_text_id = Txt.query_text_id ;
   ```

## <a name="query-dynamic-management-views"></a>Abfragen von dynamischen Verwaltungssichten
Dynamische Verwaltungssichten (DMVs) und geben Serverstatusinformationen zurück, mit denen Sie die Integrität einer Serverinstanz überwachen, Probleme diagnostizieren und die Leistung optimieren können.

So fragen Sie die dynamische Verwaltungssicht „dm_os_wait stats“ ab:

   ```sql
   SELECT wait_type, wait_time_ms
   FROM sys.dm_os_wait_stats;
   ```