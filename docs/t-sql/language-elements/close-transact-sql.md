---
description: CLOSE (Transact-SQL)
title: CLOSE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CLOSE_TSQL
- CLOSE
dev_langs:
- TSQL
helpviewer_keywords:
- closing cursors
- cursors [SQL Server], closing
- CLOSE statement
ms.assetid: 21546874-97e3-4b93-970f-87c27f6b78c7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8622cba1a9dcf8e39b23d2f7c00554e0cd3c466c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208796"
---
# <a name="close-transact-sql"></a>CLOSE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Schließt einen geöffneten Cursor, indem die Zuordnung des aktuellen Resultsets zum Cursor aufgehoben wird und alle Cursorsperren für die Zeilen, auf die der Cursor positioniert ist, freigegeben werden. `CLOSE` sorgt dafür, dass die Daten für ein erneutes Öffnen verfügbar sind, jedoch sind das Abrufen und positionierte Aktualisieren von Daten erst dann zulässig, wenn der Cursor erneut geöffnet wird. CLOSE muss bei einem geöffneten Cursor ausgeführt werden. Wurde ein Cursor lediglich deklariert oder bereits geschlossen, darf `CLOSE` nicht angewendet werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
CLOSE { { [ GLOBAL ] cursor_name } | cursor_variable_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 GLOBAL  
 Gibt an, dass *cursor_name* auf einen globalen Cursor verweist.  
  
 *cursor_name*  
 Der Name eines geöffneten Cursors. Falls sowohl ein lokaler als auch ein globaler Cursor namens *cursor_name* vorhanden ist, bezieht sich *cursor_name* nur dann auf den globalen Cursor, wenn GLOBAL angegeben ist. Andernfalls bezieht sich *cursor_name* auf den lokalen Cursor.  
  
 *cursor_variable_name*  
 Der Name einer Cursorvariablen, die mit einem geöffneten Cursor verknüpft ist.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird die richtige Platzierung der `CLOSE`-Anweisung in einem cursorbasierten Prozess gezeigt.  
  
```sql  
DECLARE Employee_Cursor CURSOR FOR  
SELECT EmployeeID, Title FROM AdventureWorks2012.HumanResources.Employee;  
OPEN Employee_Cursor;  
FETCH NEXT FROM Employee_Cursor;  
WHILE @@FETCH_STATUS = 0  
   BEGIN  
      FETCH NEXT FROM Employee_Cursor;  
   END;  
CLOSE Employee_Cursor;  
DEALLOCATE Employee_Cursor;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Cursor](../../relational-databases/cursors.md)   
 [Cursors &#40;Transact-SQL&#41;](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DEALLOCATE &#40;Transact-SQL&#41;](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)   
 [OPEN &#40;Transact-SQL&#41;](../../t-sql/language-elements/open-transact-sql.md)  
  
  
