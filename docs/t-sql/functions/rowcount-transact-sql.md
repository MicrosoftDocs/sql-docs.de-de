---
description: '&#x40;&#x40;ROWCOUNT (Transact-SQL)'
title: '@@ROWCOUNT (Transact-SQL) | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 08/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@ROWCOUNT_TSQL'
- '@@ROWCOUNT'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@ROWCOUNT function'
- number of rows affected by statement
- row affected by statements [SQL Server]
- statements [SQL Server], last statement
- counting rows
ms.assetid: 97a47998-81d9-4331-a244-9eb8b6fe4a56
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 6eca1ac99c93c8faa062b46b09fe3715663a5c87
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159717"
---
# <a name="x40x40rowcount-transact-sql"></a>&#x40;&#x40;ROWCOUNT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Gibt die Anzahl der Zeilen zurück, auf die sich die letzte Anweisung ausgewirkt hat. Beträgt die Anzahl der Zeilen mehr als 2 Milliarden, verwenden Sie [ROWCOUNT_BIG](../../t-sql/functions/rowcount-big-transact-sql.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
@@ROWCOUNT  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Rückgabetypen
 **int**  
  
## <a name="remarks"></a>Bemerkungen  
 [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen können den Wert in @@ROWCOUNT auf folgende Weise festlegen:  
  
-   @@ROWCOUNT wird auf die Anzahl der betroffenen oder gelesenen Zeilen festgelegt. Zeilen werden möglicherweise an den Client gesendet.  
  
-   @@ROWCOUNT wird aus der vorherigen Anweisungsausführung übernommen.  
  
-   @@ROWCOUNT wird auf 0 (null) zurückgesetzt, jedoch ohne den Wert an den Client zurückzugeben.  
  
 Anweisungen, die eine einfache Zuweisung vornehmen, legen den Wert für @@ROWCOUNT stets auf 1 fest. Es werden keine Zeilen an den Client gesendet. Beispiele für diese Anweisungen: SET @*local_variable*, RETURN, READTEXT und SELECT ohne Abfrageanweisungen wie SELECT GETDATE() oder SELECT **'** _Generic Text_*_'_*.  
  
 Anweisungen, die eine Zuweisung in einer Abfrage vornehmen oder RETURN in einer Abfrage verwenden, legen den Wert für @@ROWCOUNT auf die Anzahl der von der Abfrage betroffenen oder gelesenen Zeilen fest, z. B.: SELECT @*local_variable* = c1 FROM t1.  
  
 DML-Anweisungen (Data Manipulation Language = Datenbearbeitungssprache) legen den Wert für @@ROWCOUNT auf die Anzahl der von der Abfrage betroffenen Zeilen fest und geben diesen Wert an den Client zurück. Die DML-Anweisungen senden möglicherweise keine Zeilen an den Client.  
  
 DECLARE CURSOR und FETCH legen den Wert für @@ROWCOUNT auf 1 fest.  
  
 EXECUTE-Anweisungen behalten die vorherige @@ROWCOUNT-Einstellung bei.  
  
 Mit Anweisungen wie USE, SET \<option>, DEALLOCATE CURSOR, CLOSE CURSOR, PRINT, RAISERROR, BEGIN TRANSACTION oder COMMIT TRANSACTION wird der ROWCOUNT-Wert auf 0 (null) zurückgesetzt.  
  
 Nativ kompilierte gespeicherte Prozeduren behalten die vorherige @@ROWCOUNT-Einstellung bei. [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen in nativ kompilierten gespeicherten Prozeduren legen die @@ROWCOUNT-Einstellung nicht fest. Weitere Informationen finden Sie unter [Nativ kompilierte gespeicherte Prozeduren](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md).  
  
## <a name="examples"></a>Beispiele  
 Das folgende Beispiel führt eine `UPDATE`-Anweisung aus und erkennt mithilfe von `@@ROWCOUNT`, ob Zeilen geändert wurden.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE HumanResources.Employee   
SET JobTitle = N'Executive'  
WHERE NationalIDNumber = 123456789  
IF @@ROWCOUNT = 0  
PRINT 'Warning: No rows were updated';  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Systemfunktionen &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md)  
  
