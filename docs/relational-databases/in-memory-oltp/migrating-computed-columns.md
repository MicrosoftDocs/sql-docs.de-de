---
title: Migrieren von berechneten Spalten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie berechnete Spalten in speicheroptimierten Tabellen simulieren. Bewerten Sie, ob nach der Migration eine Funktion für berechnete Spalten erforderlich ist.
ms.custom: ''
ms.date: 12/17/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 64a9eade-22c3-4a9d-ab50-956219e08df1
author: MightyPen
ms.author: genemi
monikerRange: =sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 1ff95c2fad217e0592e82c6e7bf034813931e758
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91863832"
---
# <a name="migrating-computed-columns"></a>Migrieren von berechneten Spalten

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Berechnete Spalten werden in speicheroptimierten Tabellen nicht unterstützt. Sie können jedoch eine berechnete Spalte simulieren.

Ab [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] werden in speicheroptimierten Tabellen und Indizes berechnete Spalten unterstützt.

Sie sollten berücksichtigen, dass berechnete Spalten beibehalten werden müssen, wenn Sie datenträgerbasierte Tabellen zu speicheroptimierten Tabellen migrieren. Aufgrund der unterschiedlichen Leistungsmerkmale speicheroptimierter Tabellen und systemintern kompilierter gespeicherter Prozeduren ist u. U. keine Persistenz erforderlich.  
  
## <a name="non-persisted-computed-columns"></a>Nicht permanent berechnete Spalten  
 Um die Auswirkungen einer nicht persistiert berechneten Spalte zu simulieren, erstellen Sie eine Sicht für die speicheroptimierte Tabelle. Fügen Sie in der SELECT-Anweisung, durch die die Sicht definiert wird, die Definition der berechneten Spalte in die Sicht ein. Außer in einer systemintern kompilierten gespeicherten Prozedur sollten Abfragen, die Werte aus der berechneten Spalte verwenden, aus der Sicht lesen. Innerhalb der systemintern kompilierten gespeicherten Prozeduren sollten Sie eine SELECT-, UPDATE- oder DELETE-Anweisung gemäß der Definition der berechneten Spalte aktualisieren.  
  
```sql  
-- Schema for the table dbo.OrderDetails:  
-- OrderId int not null primary key,  
-- ProductId int not null,  
-- SalePrice money not null,  
-- Quantity int not null,  
-- Total money not null  
--  
-- Total is computed as SalePrice * Quantity and is not persisted.  
CREATE VIEW dbo.v_order_details AS  
   SELECT  
      OrderId,  
      ProductId,  
      SalePrice,  
      Quantity,  
      Quantity * SalePrice AS Total  
   FROM dbo.order_details  
```  
  
## <a name="persisted-computed-columns"></a>Permanent berechnete Spalten  
 Um die Auswirkungen einer persistiert berechneten Spalte zu simulieren, erstellen Sie eine gespeicherte Prozedur zum Einfügen in die Tabelle und eine weitere gespeicherte Prozedur zum Aktualisieren der Tabelle. Wenn Sie die Tabelle einfügen oder aktualisieren, rufen Sie diese gespeicherten Prozeduren auf, um diese Tasks auszuführen. Innerhalb der gespeicherten Prozeduren berechnen Sie den Wert für das berechnete Feld gemäß den Eingaben, ähnlich der Definition der berechneten Spalte in der ursprünglichen datenträgerbasierten Tabelle. Anschließend können Sie die Tabelle nach Bedarf in der gespeicherten Prozedur einfügen oder aktualisieren.  
  
```sql  
-- Schema for the table dbo.OrderDetails:  
-- OrderId int not null primary key,  
-- ProductId int not null,  
-- SalePrice money not null,  
-- Quantity int not null,  
-- Total money not null  
--  
-- Total is computed as SalePrice * Quantity and is persisted.  
-- we need to create insert and update procedures to calculate Total.  

CREATE PROCEDURE sp_insert_order_details   
@OrderId int, @ProductId int, @SalePrice money, @Quantity int  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH (LANGUAGE = N'english', TRANSACTION ISOLATION LEVEL = SNAPSHOT)  

-- compute the value here.   
-- this stored procedure works with single rows only.  
-- for bulk inserts, accept a table-valued parameter into the stored procedure  
-- and use an INSERT INTO SELECT statement.  

DECLARE @total money = @SalePrice * @Quantity  
INSERT INTO dbo.OrderDetails (OrderId, ProductId, SalePrice, Quantity, Total)  
VALUES (@OrderId, @ProductId, @SalePrice, @Quantity, @total)  
END  
GO  
  
CREATE PROCEDURE sp_update_order_details_by_id  
@OrderId int, @ProductId int, @SalePrice money, @Quantity int  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH (LANGUAGE = N'english', TRANSACTION ISOLATION LEVEL = SNAPSHOT)  

-- compute the value here.   
-- this stored procedure works with single rows only.  

DECLARE @total money = @SalePrice * @Quantity  
UPDATE dbo.OrderDetails   
SET ProductId = @ProductId, SalePrice = @SalePrice, Quantity = @Quantity, Total = @total  
WHERE OrderId = @OrderId  
END  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Migrieren zu In-Memory OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
