---
description: Tabellenwertkonstruktor (Transact-SQL)
title: Tabellenwertkonstruktor (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- inserting multiple rows
- row value expression
- row constructor [SQL Server]
- table value constructor [SQL Server]
ms.assetid: e57cd31d-140e-422f-8178-2761c27b9deb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 32074a8c0229ace53d636f8ef9bb31b8caf729b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200440"
---
# <a name="table-value-constructor-transact-sql"></a>Tabellenwertkonstruktor (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt eine Gruppe mit Zeilenwertausdrücken an, die in einer Tabelle erstellt werden sollen. Der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Tabellenwertkonstruktor ermöglicht das Angeben mehrerer Datenzeilen in nur einer DML-Anweisung. Der Tabellenwertkonstruktor kann entweder als die VALUES-Klausel einer INSERT ... VALUES-Anweisung oder als eine abgeleitete Tabelle in der USING-Klausel der MERGE-Anweisung oder der FROM-Klausel angegeben werden.
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
VALUES ( <row value expression list> ) [ ,...n ]   
  
<row value expression list> ::=  
    {<row value expression> } [ ,...n ]  
  
<row value expression> ::=  
    { DEFAULT | NULL | expression }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 WERTE  
 Bietet eine Einführung in die Zeilenwertausdruckslisten. Die einzelnen Listen müssen in Klammern gesetzt und durch ein Trennzeichen getrennt werden.  
  
 Die Anzahl der in jeder Liste angegebenen Werte muss übereinstimmen, und die Werte müssen in der gleichen Reihenfolge wie die Spalten in der Tabelle vorliegen. Für jede Spalte in der Tabelle muss ein Wert angegeben werden, oder in der Spaltenliste müssen explizit die Spalten für jeden eingehenden Wert angegeben werden.  
  
 DEFAULT  
 Erzwingt, dass [!INCLUDE[ssDE](../../includes/ssde-md.md)] den für eine Spalte definierten Standardwert einfügt. Wenn für die Spalte kein Standardwert vorhanden ist und die Spalte NULL-Werte zulässt, wird NULL eingefügt. DEFAULT ist für eine Identitätsspalte nicht zulässig. Wenn DEFAULT in einem Tabellenwertkonstruktor angegeben wird, ist diese Variable nur in einer INSERT-Anweisung zulässig.  
  
 *expression*  
 Eine Konstante, eine Variable oder ein Ausdruck. Der Ausdruck darf keine EXECUTE-Anweisung enthalten.  
  
## <a name="limitations-and-restrictions"></a>Einschränkungen  
 Bei Verwendung als abgeleitete Tabelle gibt es keine Beschränkung für die Anzahl der Zeilen.  
 
 Bei Verwendung als VALUES-Klausel einer INSERT ... VALUES-Anweisung gilt ein Grenzwert von 1.000 Zeilen. Fehler 10738 wird zurückgegeben, wenn die Anzahl der Zeilen in diesem Fall das Maximum übersteigt. Um mehr als 1.000 Zeilen einzufügen, verwenden Sie eine der folgenden Methoden:  
  
- Erstellen mehrerer INSERT-Anweisungen  
  
- Verwenden einer abgeleiteten Tabelle  
  
- Massenimport der Daten mithilfe des [**bcp**-Hilfsprogramms](../../tools/bcp-utility.md), der [Klasse „SqlBulkCopy“](/dotnet/api/system.data.sqlclient.sqlbulkcopy) von .NET, [OPENROWSET (BULK ...)](../../t-sql/functions/openrowset-transact-sql.md) oder der [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md)-Anweisung  
  
 Nur einzelne Skalarwerte sind als Zeilenwertausdruck zulässig. Eine Unterabfrage, die mehrere Spalten umfasst, ist nicht als Zeilenwertausdruck zulässig. Der folgende Code verursacht z. B. einen Syntaxfehler, weil die dritte Zeilenwertausdrucksliste eine Unterabfrage mit mehreren Spalten enthält.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE dbo.MyProducts (Name VARCHAR(50), ListPrice MONEY);  
GO  
-- This statement fails because the third values list contains multiple columns in the subquery.  
INSERT INTO dbo.MyProducts (Name, ListPrice)  
VALUES ('Helmet', 25.50),  
       ('Wheel', 30.00),  
       (SELECT Name, ListPrice FROM Production.Product WHERE ProductID = 720);  
GO  
```  
  
 Die Anweisung kann jedoch umgeschrieben werden, indem jede Spalte getrennt in der Unterabfrage angegeben wird. Im folgenden Beispiel werden drei Zeilen erfolgreich in die `MyProducts`-Tabelle eingefügt.  
  
```sql
INSERT INTO dbo.MyProducts (Name, ListPrice)  
VALUES ('Helmet', 25.50),  
       ('Wheel', 30.00),  
       ((SELECT Name FROM Production.Product WHERE ProductID = 720),  
        (SELECT ListPrice FROM Production.Product WHERE ProductID = 720));  
GO  
```  
  
## <a name="data-types"></a>Datentypen  
 Die in einer aus mehreren Zeilen bestehenden INSERT-Anweisung angegebenen Werte befolgen die Datentypkonvertierungseigenschaften der UNION ALL-Syntax. Dies führt zur impliziten Konvertierung nicht übereinstimmender Typen in den Typ, der in der [Rangfolge](../../t-sql/data-types/data-type-precedence-transact-sql.md) höher steht. Wenn es sich bei der Konvertierung nicht um eine unterstützte implizite Konvertierung handelt, gibt das System einen Fehler zurück. In der folgenden Anweisung werden beispielsweise ein ganzzahliger Wert und ein Zeichenwert in eine Spalte vom Typ **char** eingefügt.  
  
```sql
CREATE TABLE dbo.t (a INT, b CHAR);  
GO  
INSERT INTO dbo.t VALUES (1,'a'), (2, 1);  
GO  
```  
  
 Bei Ausführung der INSERT-Anweisung versucht [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], "a" in eine ganze Zahl zu konvertieren, da die Rangfolge der Datentypen angibt, dass eine ganze Zahl einen höherrangigen Typ hat als ein Zeichen. Die Konvertierung schlägt fehl, und es wird ein Fehler zurückgegeben. Sie können den Fehler vermeiden, indem Sie Werte nach Bedarf explizit konvertieren. Die vorherige Anweisung kann beispielsweise folgendermaßen geschrieben werden.  
  
```sql
INSERT INTO dbo.t VALUES (1,'a'), (2, CONVERT(CHAR,1));  
```  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-inserting-multiple-rows-of-data"></a>A. Einfügen mehrerer Datenzeilen  
 Im folgenden Beispiel wird die `dbo.Departments`-Tabelle erstellt; anschließend werden unter Verwendung des Tabellenwertkonstruktors fünf Zeilen in die Tabelle eingefügt. Da Werte für alle Spalten bereitgestellt werden und in der Reihenfolge der Spalten in der Tabelle aufgelistet sind, müssen die Spaltennamen nicht in der Spaltenliste angegeben werden.  
  
```sql
USE AdventureWorks2012;  
GO  
INSERT INTO Production.UnitMeasure  
VALUES (N'FT2', N'Square Feet ', '20080923'), (N'Y', N'Yards', '20080923'),
       (N'Y3', N'Cubic Yards', '20080923');  
GO  
```  
  
### <a name="b-inserting-multiple-rows-with-default-and-null-values"></a>B. Einfügen mehrerer Zeilen mit DEFAULT- und NULL-Werten  
 Im folgenden Beispiel wird das Angeben von DEFAULT und NULL gezeigt, wenn mithilfe des Tabellenwertkonstruktors Zeilen in eine Tabelle eingefügt werden.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE Sales.MySalesReason(  
SalesReasonID int IDENTITY(1,1) NOT NULL,  
Name dbo.Name NULL ,  
ReasonType dbo.Name NOT NULL DEFAULT 'Not Applicable' );  
GO  
INSERT INTO Sales.MySalesReason   
VALUES ('Recommendation','Other'), ('Advertisement', DEFAULT), (NULL, 'Promotion');  
  
SELECT * FROM Sales.MySalesReason;  
```  
  
### <a name="c-specifying-multiple-values-as-a-derived-table-in-a-from-clause"></a>C. Angeben mehrerer Werte als abgeleitete Tabelle in einer FROM-Klausel  
 In den folgenden Beispielen dient der Tabellenwertkonstruktor zur Angabe mehrerer Werte in der FROM-Klausel einer SELECT-Anweisung.  
  
```sql
SELECT a, b FROM (VALUES (1, 2), (3, 4), (5, 6), (7, 8), (9, 10) ) AS MyTable(a, b);  
GO  
-- Used in an inner join to specify values to return.  
SELECT ProductID, a.Name, Color  
FROM Production.Product AS a  
INNER JOIN (VALUES ('Blade'), ('Crown Race'), ('AWC Logo Cap')) AS b(Name)   
ON a.Name = b.Name;  
```  
  
### <a name="d-specifying-multiple-values-as-a-derived-source-table-in-a-merge-statement"></a>D: Angeben mehrerer Werte als abgeleitete Quelltabelle in einer MERGE-Anweisung  
 Im folgenden Beispiel wird die `SalesReason`-Tabelle durch das Aktualisieren oder Einfügen von Zeilen mithilfe von MERGE geändert. Wenn der Wert von `NewName` in der Quelltabelle einem Wert in der `Name`-Spalte der Zieltabelle entspricht (`SalesReason`), wird die `ReasonType`-Spalte in der Zieltabelle aktualisiert. Wenn der Wert von `NewName` jedoch nicht übereinstimmt, wird die Quellzeile in die Zieltabelle eingefügt. Die Quelltabelle ist eine abgeleitete Tabelle, die mithilfe des [!INCLUDE[tsql](../../includes/tsql-md.md)]-Tabellenwertkonstruktors mehrere Zeilen für die Quelltabelle angibt.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Create a temporary table variable to hold the output actions.  
DECLARE @SummaryOfChanges TABLE(Change VARCHAR(20));  
  
MERGE INTO Sales.SalesReason AS Target  
USING (VALUES ('Recommendation','Other'), ('Review', 'Marketing'), ('Internet', 'Promotion'))  
       AS Source (NewName, NewReasonType)  
ON Target.Name = Source.NewName  
WHEN MATCHED THEN  
UPDATE SET ReasonType = Source.NewReasonType  
WHEN NOT MATCHED BY TARGET THEN  
INSERT (Name, ReasonType) VALUES (NewName, NewReasonType)  
OUTPUT $action INTO @SummaryOfChanges;  
  
-- Query the results of the table variable.  
SELECT Change, COUNT(*) AS CountPerChange  
FROM @SummaryOfChanges  
GROUP BY Change;  
```  
  
### <a name="e-inserting-more-than-1000-rows"></a>E. Einfügen von mehr als 1.000 Zeilen
  Im folgenden Beispiel wird die Verwendung des Tabellenwertkonstruktors als abgeleitete Tabelle veranschaulicht. Dies ermöglicht das Einfügen von mehr als 1.000 Zeilen aus einem einzelnen Tabellenwertkonstruktor.
  
```sql
CREATE TABLE dbo.Test ([Value] INT);  
  
INSERT INTO dbo.Test ([Value])  
  SELECT drvd.[NewVal]
  FROM   (VALUES (0), (1), (2), (3), ..., (5000)) drvd([NewVal]);
```  


## <a name="see-also"></a>Weitere Informationen  
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)  
  
  
