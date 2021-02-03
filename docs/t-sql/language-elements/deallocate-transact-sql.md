---
description: DEALLOCATE (Transact-SQL)
title: DEALLOCATE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DEALLOCATE
- DEALLOCATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- locking [SQL Server], cursors
- DEALLOCATE statement
- deallocations [SQL Server]
- deleting cursor references
- removing cursor references
ms.assetid: c75cf73d-0268-4c57-973d-b8a84ff801fa
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2b6ad1892bec2cb627e4565c798506bcea91b530
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208773"
---
# <a name="deallocate-transact-sql"></a>DEALLOCATE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Entfernt einen Cursorverweis. Nachdem die Zuordnung des letzten Cursorverweises aufgehoben wurde, werden alle Datenstrukturen, die den Cursor bilden, von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] freigegeben.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
DEALLOCATE { { [ GLOBAL ] cursor_name } | @cursor_variable_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *cursor_name*  
 Der Name eines bereits deklarierten Cursors. Falls sowohl ein lokaler als auch ein globaler Cursor namens *cursor_name* vorhanden ist, bezieht sich *cursor_name* nur dann auf den globalen Cursor, wenn `GLOBAL` angegeben ist. Wird `GLOBAL` nicht angegeben, bezieht es sich auf den lokalen Cursor.  
  
 @*cursor_variable_name*  
 Der Name einer **cursor**-Variablen. @*cursor_variable_name* muss vom Typ **cursor** sein.  
  
## <a name="remarks"></a>Hinweise  
Anweisungen, die auf Cursor angewendet werden, verwenden entweder einen Cursornamen oder eine Cursorvariable, um auf den Cursor zu verweisen. `DEALLOCATE` entfernt die Zuordnung zwischen einem Cursor und dem Cursornamen oder der Cursorvariablen. Ist dies der letzte Name bzw. die letzte Variable, die auf den Cursor verweist, wird die Zuordnung des Cursors aufgehoben, und alle vom Cursor verwendeten Ressourcen werden freigegeben. Durch `DEALLOCATE` werden alle Scrollsperren freigegeben, die zum Schützen der Isolierung der Abrufvorgänge verwendet werden. Transaktionssperren, mit denen vom Cursor vorgenommene Updates, einschließlich positionierter Updates, geschützt werden, bleiben bis zum Ende der Transaktion wirksam.  
  
Die `DECLARE CURSOR`-Anweisung ordnet einem Cursor einen Cursornamen zu.  
  
```sql  
DECLARE abc SCROLL CURSOR FOR  
SELECT * FROM Person.Person;  
```  
  
Nachdem dem Cursor ein Cursorname zugeordnet wurde, kann der Name so lange nicht für einen anderen Cursor des gleichen Bereiches (global oder lokal) verwendet werden, bis die Zuordnung des Cursors aufgehoben wurde.  
  
 Eine Cursorvariable kann mithilfe einer der beiden folgenden Methoden einem Cursor zugeordnet werden:  
  
-   Über den Namen mithilfe einer `SET`-Anweisung, in der ein Cursor auf eine Cursorvariable festgelegt wird.  
  
    ```sql  
    DECLARE @MyCrsrRef CURSOR;  
    SET @MyCrsrRef = abc;  
    ```  
  
-   Ein Cursor kann auch erstellt und einer Variablen zugeordnet werden, ohne dass ein Cursorname definiert ist.  
  
    ```sql  
    DECLARE @MyCursor CURSOR;  
    SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Person.Person;  
    ```  
  
 Eine `DEALLOCATE <@cursor_variable_name>`-Anweisung entfernt nur den Verweis der benannten Variablen auf den Cursor. Die Zuordnung der Variablen wird erst dann aufgehoben, wenn sie am Ende des Batches, der gespeicherten Prozedur oder des Triggers den Gültigkeitsbereich verlässt. Nach einer `DEALLOCATE <@cursor_variable_name>`-Anweisung kann die Variable mithilfe einer SET-Anweisung einem anderen Cursor zugeordnet werden.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
DECLARE @MyCursor CURSOR;  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Sales.SalesPerson;  
  
DEALLOCATE @MyCursor;  
  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Sales.SalesTerritory;  
GO  
```  
  
Eine Zuordnung für eine Cursorvariable muss nicht explizit aufgehoben werden. Die Zuordnung der Variablen wird implizit aufgehoben, wenn sie den Bereich verlässt.  
  
## <a name="permissions"></a>Berechtigungen  
 `DEALLOCATE`-Berechtigungen erhalten standardmäßig alle gültigen Benutzer.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Skript wird gezeigt, wie Cursor erhalten bleiben, bis die Zuordnung des letzten Namens oder der Variablen, die auf die Cursor verweist, aufgehoben wurde.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create and open a global named cursor that  
-- is visible outside the batch.  
DECLARE abc CURSOR GLOBAL SCROLL FOR  
    SELECT * FROM Sales.SalesPerson;  
OPEN abc;  
GO  
-- Reference the named cursor with a cursor variable.  
DECLARE @MyCrsrRef1 CURSOR;  
SET @MyCrsrRef1 = abc;  
-- Now deallocate the cursor reference.  
DEALLOCATE @MyCrsrRef1;  
-- Cursor abc still exists.  
FETCH NEXT FROM abc;  
GO  
-- Reference the named cursor again.  
DECLARE @MyCrsrRef2 CURSOR;  
SET @MyCrsrRef2 = abc;  
-- Now deallocate cursor name abc.  
DEALLOCATE abc;  
-- Cursor still exists, referenced by @MyCrsrRef2.  
FETCH NEXT FROM @MyCrsrRef2;  
-- Cursor finally is deallocated when last referencing  
-- variable goes out of scope at the end of the batch.  
GO  
-- Create an unnamed cursor.  
DECLARE @MyCursor CURSOR;  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
SELECT * FROM Sales.SalesTerritory;  
-- The following statement deallocates the cursor  
-- because no other variables reference it.  
DEALLOCATE @MyCursor;  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [Cursor](../../relational-databases/cursors.md)   
 [DEKLARIEREN SIE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)   
 [OPEN &#40;Transact-SQL&#41;](../../t-sql/language-elements/open-transact-sql.md)  
  
  
