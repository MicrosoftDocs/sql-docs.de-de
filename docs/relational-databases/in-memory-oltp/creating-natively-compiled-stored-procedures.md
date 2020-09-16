---
title: Erstellen systemintern kompilierter gespeicherter Prozeduren | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die Transact-SQL-Features, die ausschließlich für nativ kompilierte gespeicherte Prozeduren unterstützt werden. Außerdem erfahren Sie, wie Sie nativ kompilierte gespeicherte Prozeduren in SQL Server erstellen.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: e6b34010-cf62-4f65-bbdf-117f291cde7b
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fd6c808ecc53838feb9466283c83919231ce67ad
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89537585"
---
# <a name="creating-natively-compiled-stored-procedures"></a>Erstellen systemintern kompilierter gespeicherter Prozeduren
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Von systemintern kompilierten gespeicherten Prozeduren wird nicht die vollständige [!INCLUDE[tsql](../../includes/tsql-md.md)] -Programmier- und -Abfrageoberfläche implementiert. Es gibt bestimmte [!INCLUDE[tsql](../../includes/tsql-md.md)] -Konstrukte, die innerhalb systemintern kompilierter gespeicherter Prozeduren nicht verwendet werden können. Weitere Informationen finden Sie unter [Unterstützte Funktionen für nativ kompilierte T-SQL-Module](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).  
  
Die folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionenwerden nur für nativ kompilierte gespeicherte Prozeduren unterstützt:  
  
-   ATOMIC-Blöcke. Weitere Informationen finden Sie unter [ATOMIC-Blöcke](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).  
  
-   `NOT NULL`-Einschränkungen für Parameter und Variablen. Sie können als **NOT NULL** deklarierten Parametern oder Variablen keine **NULL**-Werte zuweisen. Weitere Informationen finden Sie unter [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md).  
  
    -   `CREATE PROCEDURE dbo.myproc (@myVarchar VARCHAR(32) NOT NULL) AS (...)`  
  
    -   `DECLARE @myVarchar VARCHAR(32) NOT NULL = "Hello"; -- Must initialize to a value.`  
  
    -   `SET @myVarchar = NULL; -- Compiles, but fails during run time.`  
  
-   Schemabindung von systemintern kompilierten gespeicherten Prozeduren.  
  
Systemintern kompilierte gespeicherte Prozeduren werden mithilfe von [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md) erstellt. Das folgende Beispiel zeigt eine speicheroptimierte Tabelle und eine systemintern kompilierte gespeicherte Prozedur, die zum Einfügen von Zeilen in die Tabelle verwendet wird.  
  
```sql  
CREATE TABLE [dbo].[T2] (  
  [c1] [int] NOT NULL, 
  [c2] [datetime] NOT NULL,
  [c3] nvarchar(5) NOT NULL, 
  CONSTRAINT [PK_T1] PRIMARY KEY NONCLUSTERED ([c1])  
  ) WITH ( MEMORY_OPTIMIZED = ON , DURABILITY = SCHEMA_AND_DATA )  
GO  
  
CREATE PROCEDURE [dbo].[usp_2] (@c1 int, @c3 nvarchar(5)) 
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS BEGIN ATOMIC WITH  
(  
 TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english'  
)  
  DECLARE @c2 datetime = GETDATE();  
  INSERT INTO [dbo].[T2] (c1, c2, c3) values (@c1, @c2, @c3);  
END  
GO  
```  
 
Im Codebeispiel ist an **NATIVE_COMPILATION** erkennbar, dass diese gespeicherte [!INCLUDE[tsql](../../includes/tsql-md.md)] -Prozedur eine systemintern kompilierte gespeicherte Prozedur ist. Die folgenden Optionen sind erforderlich:  
  
|Option|BESCHREIBUNG|  
|------------|-----------------|  
|**SCHEMABINDING**|Eine systemintern kompilierte gespeicherte Prozedur muss an das Schema der Objekte gebunden werden, auf die sie verweist. Dies bedeutet, dass Tabellen, auf die von der Prozedur verwiesen wird, nicht gelöscht werden können. Die Tabellen, auf die in der Prozedur verwiesen wird, müssen den Schemanamen enthalten, und Platzhalterzeichen (\*) sind in Abfragen nicht zulässig (also ohne `SELECT * from...`). **SCHEMABINDING** wird nur in dieser Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]für systemintern kompilierte gespeicherte Prozeduren unterstützt.|  
|**BEGIN ATOMIC**|Der Text einer systemintern kompilierten gespeicherten Prozedur muss genau ein ATOMIC-Block sein. ATOMIC-Blöcke gewährleisten die unteilbare Ausführung der gespeicherten Prozedur. Wenn die Prozedur außerhalb des Kontexts einer aktiven Transaktion aufgerufen wird, wird eine neue Transaktion gestartet, für die am Ende des ATOMIC-Blocks ein Commit ausgeführt wird. ATOMIC-Blöcke in systemintern kompilierten gespeicherten Prozeduren weisen zwei erforderliche Optionen auf:<br /><br /> **TRANSACTION ISOLATION LEVEL**. Informationen zu unterstützten Isolationsstufen finden Sie unter [Transaktionsisolationsstufen für speicheroptimierte Tabellen](https://msdn.microsoft.com/library/8a6a82bf-273c-40ab-a101-46bd3615db8a) .<br /><br /> **LANGUAGE**. Die Sprache der gespeicherten Prozedur muss auf eine der verfügbaren Sprachen bzw. einen der verfügbaren Sprachenaliase festgelegt werden.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Nativ kompilierte gespeicherte Prozeduren](../../relational-databases/in-memory-oltp/natively-compiled-stored-procedures.md)  
  
  
