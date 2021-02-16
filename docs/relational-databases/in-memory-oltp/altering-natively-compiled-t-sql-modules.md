---
title: Ändern von systemintern kompilierten T-SQL-Modulen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie ALTER-Vorgänge für nativ kompilierte gespeicherte Prozeduren und nativ kompilierte Transact-SQL-Module in SQL Server und Azure SQL-Datenbank ausführen.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 010318a0-6807-47c3-8ecc-bb7cb60513f0
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b1427f359d0b6019f76432a4a65ac7b954aacb51
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351302"
---
# <a name="altering-natively-compiled-t-sql-modules"></a>Ändern von nativ kompilierten T-SQL-Modulen
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und höher) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] können Sie `ALTER`-Vorgänge auf nativ kompilierte gespeicherte Prozeduren und andere nativ kompilierte [!INCLUDE[tsql](../../includes/tsql-md.md)]-Module wie skalare benutzerdefinierte Funktionen und Trigger mit der `ALTER`-Anweisung ausführen.  
  
Bei der Ausführung von `ALTER` für ein nativ kompiliertes [!INCLUDE[tsql](../../includes/tsql-md.md)]-Modul wird das Modul mit einer neuen Definition neu kompiliert. Während der Neukompilierung steht die alte Version des Moduls nach wie vor noch für die Ausführung zur Verfügung. Nach Abschluss der Kompilierung werden die Modulausführungen entladen und die neue Version des Moduls wird installiert. Bei der Änderung eines nativ kompilierten [!INCLUDE[tsql](../../includes/tsql-md.md)]-Moduls können die folgenden Optionen geändert werden.  
  
-   Parameter  
-   EXECUTE AS  
-   TRANSACTION ISOLATION LEVEL  
-   LANGUAGE  
-   DATEFIRST  
-   DATEFORMAT  
-   DELAYED_DURABILITY  
  
> [!NOTE]  
> Nativ kompilierte [!INCLUDE[tsql](../../includes/tsql-md.md)]-Module können nicht in nicht-nativ kompilierte Module konvertiert werden. Nicht-nativ kompilierte T-SQL-Module können nicht in nativ kompilierte Module konvertiert werden.  
  
Weitere Informationen zu `ALTER PROCEDURE`-Funktionen und deren Syntax finden Sie unter [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md).  
  
Sie können [sp_recompile](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md) für ein nativ kompiliertes [!INCLUDE[tsql](../../includes/tsql-md.md)]-Modul ausführen, wodurch das Modul bei der nächsten Ausführung erneut kompiliert.  
  
## <a name="example"></a>Beispiel  
Das folgende Beispiel zeigt die Erstellung einer speicheroptimierten Tabelle (T1) und einer systemintern kompilierten gespeicherten Prozedur (ups_1), die alle Spalten von Tabelle T1 auswählt. Anschließend wird usp_1 geändert, um die Klausel `EXECUTE AS` zu entfernen, `LANGUAGE` zu ändern und nur eine Spalte (C1) von T1 auszuwählen.  
  
```sql  
CREATE TABLE [dbo].[T1] (  
  [c1] [int] NOT NULL,  
  [c2] [float] NOT NULL,  
  CONSTRAINT [PK_T1] PRIMARY KEY NONCLUSTERED ([c1])  
  ) WITH ( MEMORY_OPTIMIZED = ON , DURABILITY = SCHEMA_AND_DATA )  
GO  
  
CREATE PROCEDURE [dbo].[usp_1]  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH  
(  
 TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english'  
)  
   SELECT c1, c2 FROM dbo.T1  
END  
GO  
  
ALTER PROCEDURE [dbo].[usp_1]  
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS BEGIN ATOMIC WITH  
(  
 TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'Dutch'  
)  
   SELECT c1 FROM dbo.T1  
END  
GO    
```   
  
## <a name="see-also"></a>Weitere Informationen  
 [Nativ kompilierte gespeicherte Prozeduren](./a-guide-to-query-processing-for-memory-optimized-tables.md)