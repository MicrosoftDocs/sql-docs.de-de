---
description: OPEN (Transact-SQL)
title: OPEN (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPEN_TSQL
- OPEN
dev_langs:
- TSQL
helpviewer_keywords:
- opening cursors
- cursors [SQL Server], opening
- populating cursors [SQL Server]
- OPEN statement
- Transact-SQL cursors, opening
ms.assetid: fd1c5e3b-6045-4a42-b646-3fca76e874c1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 23d3a4c306d61f2fdc7f71102a3a786dd9856941
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191925"
---
# <a name="open-transact-sql"></a>OPEN (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Öffnet einen [!INCLUDE[tsql](../../includes/tsql-md.md)]-Servercursor und füllt den Cursor auf, indem die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung ausgeführt wird, die in der DECLARE - oder SET-*cursor_variable*-Anwendung angegeben ist.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
OPEN { { [ GLOBAL ] cursor_name } | cursor_variable_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 GLOBAL  
 Gibt an, dass *cursor_name* auf einen globalen Cursor verweist.  
  
 *cursor_name*  
 Der Name eines deklarierten Cursors. Falls sowohl ein lokaler als auch ein globaler Cursor namens *cursor_name* vorhanden ist, bezieht sich *cursor_name* nur dann auf den globalen Cursor, wenn GLOBAL angegeben ist. Andernfalls bezieht sich *cursor_name* auf den lokalen Cursor.  
  
 *cursor_variable_name*  
 Der Name einer Cursorvariablen, die auf einen Cursor verweist.  
  
## <a name="remarks"></a>Hinweise  
 Falls der Cursor mit der Option INSENSITIVE oder STATIC deklariert wird, erstellt OPEN eine temporäre Tabelle für das Resultset. OPEN schlägt fehl, wenn die Größe einer Zeile im Resultset die Maximalgröße für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Tabellen überschreitet. Falls der Cursor mit der Option KEYSET deklariert wird, erstellt OPEN eine temporäre Tabelle für das Keyset. Die temporären Tabellen werden in tempdb gespeichert.  
  
 Nachdem ein Cursor geöffnet wurde, kann mit der @@CURSOR_ROWS-Funktion die Anzahl der Zeilen im letzten geöffneten Cursor abgerufen werden, die der Bedingung entsprechen.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt das asynchrone Generieren von keysetgesteuerten oder statischen [!INCLUDE[tsql](../../includes/tsql-md.md)] -Cursorn nicht. [!INCLUDE[tsql](../../includes/tsql-md.md)] -Cursorvorgänge, z. B. OPEN oder FETCH, sind in Batches enthalten. Daher ist das asynchrone Generieren von [!INCLUDE[tsql](../../includes/tsql-md.md)] -Cursorn nicht erforderlich. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt weiterhin asynchrone keysetgesteuerte oder statische AP -Servercursor (Application Programming Interface), wobei Cursoroperationen des Typs „Open“ mit niedriger Latenz ein Problem darstellen. Dies ist auf Clientroundtrips zurückzuführen, die für jeden Cursorvorgang ausgeführt werden.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird ein Cursor geöffnet, und es werden alle Zeilen abgerufen.  
  
```sql  
DECLARE Employee_Cursor CURSOR FOR  
SELECT LastName, FirstName  
FROM AdventureWorks2012.HumanResources.vEmployee  
WHERE LastName like 'B%';  
  
OPEN Employee_Cursor;  
  
FETCH NEXT FROM Employee_Cursor;  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    FETCH NEXT FROM Employee_Cursor  
END;  
  
CLOSE Employee_Cursor;  
DEALLOCATE Employee_Cursor;  
```  
  
## <a name="see-also"></a>Siehe auch  
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [@@CURSOR_ROWS &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-rows-transact-sql.md)   
 [DEALLOCATE &#40;Transact-SQL&#41;](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [DECLARE CURSOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)  
  
  
