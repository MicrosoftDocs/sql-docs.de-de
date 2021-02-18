---
title: Von IntelliSense unterstützte Transact-SQL-Syntax
description: In diesem Artikel erfahren Sie, welche Transact-SQL-Anweisungen und Syntaxelemente von Microsoft SQL Server Management Studio – Transact-SQL IntelliSense in SQL Server 2019 (15.x) unterstützt werden.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- Transact-SQL IntelliSense
- IntelliSense [SQL Server], Transact-SQL syntax
ms.assetid: 194e8f4f-fd7e-4f32-a169-f23531128004
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/16/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 38c0ce9079dddb67e274fccb071aabbb778fb050
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352908"
---
# <a name="transact-sql-syntax-supported-by-intellisense"></a>Von IntelliSense unterstützte Transact-SQL-Syntax
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Dieses Thema beschreibt die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen und Syntaxelemente, die von IntelliSense in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]unterstützt werden.  
  
## <a name="statements-supported-by-intellisense"></a>Von IntelliSense unterstützte Anweisungen  
 In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]unterstützt IntelliSense nur die am häufigsten verwendeten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen. Einige allgemeine [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor-Bedingungen könnten verhindern, dass IntelliSense funktioniert. Weitere Informationen finden Sie unter [Problembehandlung von IntelliSense &#40;SQL Server Management Studio&#41;](./troubleshooting-intellisense.md).  
  
> [!NOTE]  
>  IntelliSense ist nicht für verschlüsselte Datenbankobjekte verfügbar, z. B. verschlüsselte gespeicherte Prozeduren oder benutzerdefinierte Funktionen. Für Parameter von erweiterten gespeicherten Prozeduren und benutzerdefinierten Typen für die CLR-Integration stehen weder Parameterhilfe noch QuickInfo zur Verfügung.  
  
### <a name="select-statement"></a>SELECT-Anweisung  
 Der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor stellt IntelliSense-Unterstützung für die folgenden Syntaxelemente in der SELECT-Anweisung bereit:  
  
:::row:::
    :::column:::
        SELECT
    :::column-end:::
    :::column:::
        WHERE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        FROM
    :::column-end:::
    :::column:::
        ORDER BY
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        HAVING
    :::column-end:::
    :::column:::
        UNION
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        FOR
    :::column-end:::
    :::column:::
        GROUP BY
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        TOP
    :::column-end:::
    :::column:::
        OPTION (Hinweis)
    :::column-end:::
:::row-end:::

### <a name="additional-transact-sql-statements-that-are-supported"></a>Weitere Transact-SQL-Anweisungen, die unterstützt werden  
 Der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor stellt zudem IntelliSense-Unterstützung für die in der folgenden Tabellen aufgeführten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen bereit.  
  
|Transact-SQL-Anweisung|Unterstützte Syntax|Ausnahmen|  
|-----------------------------|----------------------|----------------|  
|[INSERT](../../t-sql/statements/insert-transact-sql.md)|Die gesamte Syntax, mit Ausnahme der *execute_statement* -Klausel.|Keine|  
|[UPDATE](../../t-sql/queries/update-transact-sql.md)|Gesamte Syntax.|Keine|  
|[DELETE](../../t-sql/statements/delete-transact-sql.md)|Gesamte Syntax.|Keine|  
|[DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)|Gesamte Syntax.|Keine|  
|[SET@local_variable](../../t-sql/language-elements/set-local-variable-transact-sql.md)|Gesamte Syntax.|Keine|  
|[EXECUTE](../../t-sql/language-elements/execute-transact-sql.md)|Ausführung von benutzerdefinierten gespeicherten Prozeduren, gespeicherten Systemprozeduren, benutzerdefinierten Funktionen und Systemfunktionen.|Keine|  
|[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)|Gesamte Syntax.|Keine|  
|[CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)|Gesamte Syntax.|Keine|  
|[CREATE PROCEDURE](../../t-sql/statements/create-procedure-transact-sql.md)|Gesamte Syntax.|Es gibt keine IntelliSense-Unterstützung für die EXTERNAL NAME-Klausel.<br /><br /> In der AS-Klausel unterstützt IntelliSense nur die Anweisungen und die Syntaxelemente, die in diesem Thema aufgeführt werden.|  
|[ALTER PROCEDURE](../../t-sql/statements/alter-procedure-transact-sql.md)|Gesamte Syntax|Es gibt keine IntelliSense-Unterstützung für die EXTERNAL NAME-Klausel.<br /><br /> In der AS-Klausel unterstützt IntelliSense nur die Anweisungen und die Syntaxelemente, die in diesem Thema aufgeführt werden.|  
|[USE](../../t-sql/language-elements/use-transact-sql.md)|Gesamte Syntax.|Keine|  
  
## <a name="intellisense-in-supported-statements"></a>IntelliSense in unterstützten Anweisungen  
 IntelliSense im [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor unterstützt die folgenden Syntaxelemente, wenn sie in einer der unterstützten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen verwendet werden:  
  
-   Alle Jointypen, einschließlich APPLY.  
  
-   PIVOT und UNPIVOT.  
  
-   Verweise auf die folgenden Datenbankobjekte:  
  
    -   Datenbanken und Schemas  
  
    -   Tabellen, Sichten, Tabellenwertfunktionen und Tabellenausdrücke  
  
    -   Spalten  
  
    -   Prozeduren und Prozedurparameter  
  
    -   Skalare Funktionen und Skalarausdrücke  
  
    -   Lokale Variablen  
  
    -   Allgemeine Tabellenausdrücke (CTE)  
  
-   Datenbankobjekte, auf die nur in der CREATE-Anweisung oder der ALTER-Anweisung im Skript oder im Batch verwiesen wird, die aber nicht in der Datenbank vorhanden sind, da das Skript bzw. der Batch noch nicht ausgeführt wurde. Nachfolgend sind diese Objekte aufgeführt:  
  
    -   Tabellen und Prozeduren, die in einer CREATE TABLE-Anweisung oder CREATE PROCEDURE-Anweisung im Skript oder Batch angegeben wurden.  
  
    -   Änderungen an Tabellen und Prozeduren, die in einer ALTER TABLE-Anweisung oder ALTER PROCEDURE-Anweisung im Skript oder Batch angegeben wurden.  
  
    > [!NOTE]  
    >  IntelliSense ist nicht verfügbar für die Spalten einer CREATE VIEW-Anweisung, solange die CREATE VIEW-Anweisung nicht ausgeführt wurde.  
  
 IntelliSense wird für die zuvor genannten Elemente nicht bereitgestellt, wenn sie in anderen [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen verwendet werden. Es ist z. B. IntelliSense-Unterstützung für Spaltennamen verfügbar, die in einer SELECT-Anweisung verwendet werden, nicht jedoch für Spaltennamen, die in der CREATE FUNCTION-Anweisung verwendet werden.  
  
## <a name="examples"></a>Beispiele  
 In einem [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skript oder -Batch unterstützt IntelliSense im [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor nur die Anweisungen und Syntaxelemente, die in diesem Thema aufgeführt werden. Die folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)] -Codebeispiele zeigen, welche Anweisungen und Syntaxelemente von IntelliSense unterstützt werden. Beispielsweise ist im folgenden Batch IntelliSense verfügbar für die `SELECT` -Anweisung, wenn diese eigencodiert ist, aber nicht, wenn `SELECT` in einer `CREATE FUNCTION` -Anweisung enthalten ist.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT Name  
FROM Production.Product  
WHERE Name LIKE N'Road-250%' and Color = N'Red';  
GO  
CREATE FUNCTION Production.ufn_Red250 ()  
RETURNS TABLE  
AS  
RETURN   
(  
    SELECT Name  
    FROM AdventureWorks2012.Production.Product  
    WHERE Name LIKE N'Road-250%'  
      AND Color = N'Red'  
);GO  
```  
  
 Diese Funktionalität gilt auch für die Sätze von [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen in der AS-Klausel einer CREATE PROCEDURE-Anweisung oder ALTER PROCEDURE-Anweisung.  
  
 In einem [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skript oder -Batch unterstützt IntelliSense Objekte, die in einer CREATE-Anweisung oder ALTER-Anweisung angegeben sind. Allerdings sind diese Objekte nicht in der Datenbank vorhanden, da die Anweisungen nicht ausgeführt wurden. Sie können z. B. den folgenden Code im Abfrage-Editor eingeben:  
  
```  
USE MyTestDB;  
GO  
CREATE TABLE MyTable  
    (PrimaryKeyCol   INT PRIMARY KEY,  
    FirstNameCol      NVARCHAR(50),  
   LastNameCol       NVARCHAR(50));  
GO  
SELECT   
```  
  
 Wenn Sie `SELECT`eingegeben haben, listet IntelliSense **PrimaryKeyCol**, **FirstNameCol** und **LastNameCol** als mögliche Elemente in der Auswahlliste auf, auch wenn das Skript nicht ausgeführt wurde und `MyTable` noch nicht in `MyTestDB`vorhanden ist.  
  
