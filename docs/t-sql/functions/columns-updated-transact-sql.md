---
description: COLUMNS_UPDATED (Transact-SQL)
title: COLUMNS_UPDATED (Transact-SQL)
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COLUMNS_UPDATED_TSQL
- COLUMNS_UPDATED
dev_langs:
- TSQL
helpviewer_keywords:
- COLUMNS_UPDATED function
- testing columns
- column testing [SQL Server]
- updated columns
ms.assetid: 765fde44-1f95-4015-80a4-45388f18a42c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 42f23dce6b51f573c2bdf07c0a4d2f25ab088fa3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352409"
---
# <a name="columns_updated-transact-sql"></a>COLUMNS_UPDATED (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Diese Funktion gibt ein **varbinary**-Bitmuster zurück, das die eingefügten oder aktualisierten Spalten der Tabelle oder Ansicht anzeigt. `COLUMNS_UPDATED` wird im Text eines INSERT- oder UPDATE-Triggers von [!INCLUDE[tsql](../../includes/tsql-md.md)] verwendet, um zu testen, ob der Trigger bestimmte Aktionen ausführen soll.
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
COLUMNS_UPDATED ( )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
**varbinary**
  
## <a name="remarks"></a>Bemerkungen  
`COLUMNS_UPDATED` testet, ob UPDATE- oder INSERT-Aktionen für mehrere Spalten ausgeführt wurden. Verwenden Sie [UPDATE()](../../t-sql/functions/update-trigger-functions-transact-sql.md), um zu testen, ob UPDATE- oder INSERT-Aktionen für eine einzelne Spalte ausgeführt wurden.
  
`COLUMNS_UPDATED` gibt mindestens ein Byte in der Reihenfolge von links nach rechts zurück. Das äußere rechte Bit stellt in jedem Byte das niederwertigste Bit dar. Das äußere rechte Bit im Byte ganz links stellt die erste Tabellenspalte in der Tabelle dar. Das nächste Bit links davon stellt die zweite Spalte dar usw. `COLUMNS_UPDATED` gibt mehrere Bytes zurück, falls die Tabelle, für die der Trigger erstellt wird, mehr als acht Spalten enthält, wobei das niederwertigste Byte ganz links steht. `COLUMNS_UPDATED` gibt für alle Spalten in INSERT-Aktionen TRUE zurück, weil in die Spalten entweder explizite oder implizite Werte (NULL) eingefügt wurden.
  
Verwenden Sie zum Testen, ob in bestimmten Spalten Updates oder Einfügungen vorgenommen wurden, die Syntax mit einem bitweisen Operator und einer ganzzahligen Bitmaske der getesteten Spalten. Die Tabelle **t1** enthält beispielsweise die Spalten **C1**, **C2**, **C3**, **C4** und **C5**. Wenn Sie überprüfen möchten, ob die Spalten **C2**, **C3** und **C4** alle erfolgreich aktualisiert wurden (die Tabelle **t1** weist dabei einen UPDATE-Trigger auf), verwenden Sie die Syntax mit **& 14**. Geben Sie **& 2** ein, um zu testen, ob nur die Spalte **C2** aktualisiert wurde. Tatsächliche Beispiele finden Sie unter [Beispiel A](#a-using-columns_updated-to-test-the-first-eight-columns-of-a-table) und [Beispiel B](#b-using-columns_updated-to-test-more-than-eight-columns).
  
Verwenden Sie `COLUMNS_UPDATED` überall innerhalb eines INSERT- oder UPDATE-Triggers von [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
Die Spalte ORDINAL_POSITION der Ansicht INFORMATION_SCHEMA.COLUMNS ist nicht mit dem Bitmuster von Spalten kompatibel, die von `COLUMNS_UPDATED` zurückgegeben werden. Für ein mit `COLUMNS_UPDATED` kompatibles Bitmuster verweisen Sie, wie im folgenden Beispiel dargestellt, auf die `ColumnID`-Eigenschaft der `COLUMNPROPERTY`-Systemfunktion, wenn Sie die `INFORMATION_SCHEMA.COLUMNS`-Ansicht abfragen.
  
```sql
SELECT TABLE_NAME, COLUMN_NAME,  
    COLUMNPROPERTY(OBJECT_ID(TABLE_SCHEMA + '.' + TABLE_NAME),  
    COLUMN_NAME, 'ColumnID') AS COLUMN_ID  
FROM AdventureWorks2012.INFORMATION_SCHEMA.COLUMNS  
WHERE TABLE_NAME = 'Person';  
```  

Wenn ein Trigger für eine Spalte gilt, wird der Wert `COLUMNS_UPDATED` als `true` oder `1` zurückgegeben. Dies ist auch der Fall, wenn der Spaltenwert unverändert bleibt. Dies wurde mit Absicht so eingerichtet, und der Trigger sollte eine Geschäftslogik implementieren, die bestimmt, ob der Einfüge-/Update-/Löschvorgang zulässig ist oder nicht. 
  
## <a name="column-sets"></a>Spaltensätze
Wenn ein Spaltensatz für eine Tabelle definiert wird, verhält sich die Funktion `COLUMNS_UPDATED` wie folgt:
-   Wenn eine Elementspalte des Spaltensatzes explizit aktualisiert wird, werden das entsprechende Bit für diese Spalte und das Bit für den Spaltensatz auf 1 festgelegt.  
-   Wenn ein Spaltensatz explizit aktualisiert wird, werden das Bit für den Spaltensatz und die Bits für alle Spalten mit geringer Dichte in dieser Tabelle auf 1 festgelegt.  
-   Für Einfügevorgänge werden alle Bits auf 1 festgelegt.  
  
     Da bei Änderungen des Spaltensatzes die Bits aller Spalten in diesem Spaltensatz auf 1 zurückgesetzt werden, werden auch nicht geänderte Spalten in einem Spaltensatz als geändert angezeigt. Weitere Informationen zu Spaltensätzen finden Sie unter [Verwenden von Spaltensätzen](../../relational-databases/tables/use-column-sets.md).  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-columns_updated-to-test-the-first-eight-columns-of-a-table"></a>A. Verwenden von COLUMNS_UPDATED zum Testen der ersten acht Spalten einer Tabelle  
In diesem Beispiel werden zwei Tabellen erstellt: `employeeData` und `auditEmployeeData`. Die `employeeData`-Tabelle enthält vertrauliche Gehaltsinformationen und kann von Mitgliedern der Personalabteilung geändert werden. Wenn die Sozialversicherungsnummer (SSN, Social Security Number), das Jahresgehalt oder die Kontonummer eines Mitarbeiters geändert wird, wird ein Überwachungsdatensatz generiert und in die `auditEmployeeData`-Überwachungstabelle eingefügt.
  
Mit der `COLUMNS_UPDATED()`-Funktion kann auf schnelle Weise getestet werden, ob Änderungen an Spalten mit vertraulichen Informationen zu Mitarbeitern vorgenommen wurden. `COLUMNS_UPDATED()` kann jedoch nur dann auf diese Weise verwendet werden, wenn Sie Änderungen an den ersten acht Spalten in der Tabelle erkennen möchten.
  
```sql
USE AdventureWorks2012;  
GO  
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
   WHERE TABLE_NAME = 'employeeData')  
   DROP TABLE employeeData;  
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
   WHERE TABLE_NAME = 'auditEmployeeData')  
   DROP TABLE auditEmployeeData;  
GO  
CREATE TABLE dbo.employeeData (  
   emp_id INT NOT NULL PRIMARY KEY,  
   emp_bankAccountNumber CHAR (10) NOT NULL,  
   emp_salary INT NOT NULL,  
   emp_SSN CHAR (11) NOT NULL,  
   emp_lname NCHAR (32) NOT NULL,  
   emp_fname NCHAR (32) NOT NULL,  
   emp_manager INT NOT NULL  
   );  
GO  
CREATE TABLE dbo.auditEmployeeData (  
   audit_log_id uniqueidentifier DEFAULT NEWID() PRIMARY KEY,  
   audit_log_type CHAR (3) NOT NULL,  
   audit_emp_id INT NOT NULL,  
   audit_emp_bankAccountNumber CHAR (10) NULL,  
   audit_emp_salary INT NULL,  
   audit_emp_SSN CHAR (11) NULL,  
   audit_user sysname DEFAULT SUSER_SNAME(),  
   audit_changed DATETIME DEFAULT GETDATE()  
   );  
GO  
CREATE TRIGGER dbo.updEmployeeData   
ON dbo.employeeData   
AFTER UPDATE AS  
/* Check whether columns 2, 3 or 4 have been updated. If any or all  
columns 2, 3 or 4 have been changed, create an audit record.
The bitmask is: power(2, (2-1)) + power(2, (3-1)) + power(2, (4-1)) = 14.
This bitmask translates into base_10 as: 2 + 4 + 8 = 14.
To test whether all columns 2, 3, and 4 are updated, use = 14 instead of > 0  
(below). */
  
   IF (COLUMNS_UPDATED() & 14) > 0  
/* Use IF (COLUMNS_UPDATED() & 14) = 14 to see whether all columns 2, 3,   
and 4 are updated. */  
      BEGIN  
-- Audit OLD record.  
      INSERT INTO dbo.auditEmployeeData  
         (audit_log_type,  
         audit_emp_id,  
         audit_emp_bankAccountNumber,  
         audit_emp_salary,  
         audit_emp_SSN)  
         SELECT 'OLD',   
            del.emp_id,  
            del.emp_bankAccountNumber,  
            del.emp_salary,  
            del.emp_SSN  
         FROM deleted del;  
  
-- Audit NEW record.  
      INSERT INTO dbo.auditEmployeeData  
         (audit_log_type,  
         audit_emp_id,  
         audit_emp_bankAccountNumber,  
         audit_emp_salary,  
         audit_emp_SSN)  
         SELECT 'NEW',  
            ins.emp_id,  
            ins.emp_bankAccountNumber,  
            ins.emp_salary,  
            ins.emp_SSN  
         FROM inserted ins;  
   END;  
GO  
  
/* Inserting a new employee does not cause the UPDATE trigger to fire. */  
INSERT INTO employeeData  
   VALUES ( 101, 'USA-987-01', 23000, 'R-M53550M', N'Mendel', N'Roland', 32);  
GO  
  
/* Updating the employee record for employee number 101 to change the   
salary to 51000 causes the UPDATE trigger to fire and an audit trail to   
be produced. */  
  
UPDATE dbo.employeeData  
   SET emp_salary = 51000  
   WHERE emp_id = 101;  
GO  
SELECT * FROM auditEmployeeData;  
GO  
  
/* Updating the employee record for employee number 101 to change both   
the bank account number and social security number (SSN) causes the   
UPDATE trigger to fire and an audit trail to be produced. */  
  
UPDATE dbo.employeeData  
   SET emp_bankAccountNumber = '133146A0', emp_SSN = 'R-M53550M'  
   WHERE emp_id = 101;  
GO  
SELECT * FROM dbo.auditEmployeeData;  
  
GO  
```  
  
### <a name="b-using-columns_updated-to-test-more-than-eight-columns"></a>B. Verwenden von COLUMNS_UPDATED zum Testen von mehr als acht Spalten  
Wenn Sie testen möchten, ob Updates in anderen Spalten als den ersten acht Spalten in einer Tabelle ausgeführt wurden, verwenden Sie die `SUBSTRING`-Funktion, um das richtige durch `COLUMNS_UPDATED` zurückgegebene Bit zu testen. In diesem Beispiel wird getestet, ob Updates vorhanden sind, die die Spalten `3`, `5` und `9` in der Tabelle `AdventureWorks2012.Person.Person` betreffen.
  
```sql
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'Person.uContact2', N'TR') IS NOT NULL  
    DROP TRIGGER Person.uContact2;  
GO  
CREATE TRIGGER Person.uContact2 ON Person.Person  
AFTER UPDATE AS  
    IF ( (SUBSTRING(COLUMNS_UPDATED(), 1, 1) & 20 = 20)   
        AND (SUBSTRING(COLUMNS_UPDATED(), 2, 1) & 1 = 1) )   
    PRINT 'Columns 3, 5 and 9 updated';  
GO  
  
UPDATE Person.Person   
   SET NameStyle = NameStyle,  
      FirstName=FirstName,  
      EmailPromotion=EmailPromotion;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen
[Bitweise Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-operators-transact-sql.md)  
[CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)  
[UPDATE&#40;&#41; &#40;Transact-SQL&#41;](../../t-sql/functions/update-trigger-functions-transact-sql.md)
  
  
