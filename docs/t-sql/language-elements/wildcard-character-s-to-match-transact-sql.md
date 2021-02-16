---
title: '[ ] Platzhalterzeichen für zu suchende Zeichen'
description: Verwenden eines Platzhalters, um ein oder mehrere Zeichen abzugleichen
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Match
- wildcard
- '[ ]'
- '[_]_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- wildcard characters [SQL Server]
- '[ ] (wildcard - character(s) to match)'
ms.assetid: 57817576-0bf1-49ed-b05d-fac27e8fed7a
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2c69fbf1900de88da7faf08e477eb4775187b964
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339805"
---
# <a name="--wildcard---characters-to-match-transact-sql"></a>\[ \] (Platzhalterzeichen – zu suchende(s) Zeichen) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Entspricht jedem einzelnen Zeichen im Bereich oder der Menge, der bzw. die innerhalb der Klammern `[ ]` angegeben ist. Diese Platzhalterzeichen können in Zeichenfolgenvergleichen verwendet werden, bei denen Mustervergleiche wie `LIKE` und `PATINDEX` durchgeführt werden.  

 
## <a name="examples"></a>Beispiele  
### <a name="a-simple-example"></a>A: Einfaches Beispiel   
Im folgenden Beispiel werden Namen zurückgegeben, die mit dem Buchstaben `m` beginnen. `[n-z]` legt fest, dass der zweite Buchstaben zwischen `n` und `z` liegen muss. Durch das Prozent-Platzhalterzeichen `%` wird angegeben, dass auf das zweite Zeichen entweder kein weiteres Zeichen oder beliebige Zeichen folgen können. Die Datenbanken `model` und `msdb` erfüllen diese Kriterien. Die `master`-Datenbank erfüllt diese Kriterien nicht und wird aus dem Resultset ausgeschlossen.
 
```sql
SELECT name FROM sys.databases
WHERE name LIKE 'm[n-z]%';
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  

```
name
-----
model
msdb
```   
 Möglicherweise haben Sie aber zusätzliche qualifizierende Datenbanken installiert.


### <a name="b-more-complex-example"></a>B: Komplexeres Beispiel   
 Im folgenden Beispiel wird mithilfe des []-Operators nach den IDs und Namen aller [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)]-Mitarbeiter gesucht, deren Adressen eine vierstellige Postleitzahl enthalten.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.BusinessEntityID, p.FirstName, p.LastName, a.PostalCode  
FROM HumanResources.Employee AS e  
INNER JOIN Person.Person AS p ON e.BusinessEntityID = p.BusinessEntityID  
INNER JOIN Person.BusinessEntityAddress AS ea ON e.BusinessEntityID = ea.BusinessEntityID  
INNER JOIN Person.Address AS a ON a.AddressID = ea.AddressID  
WHERE a.PostalCode LIKE '[0-9][0-9][0-9][0-9]';  
```  
  
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  
  
```  
EmployeeID      FirstName      LastName      PostalCode  
----------      ---------      ---------     ----------  
290             Lynn           Tsoflias      3000  
```  

### <a name="c-using-a-set-that-combines-ranges-and-single-characters"></a>C: Verwenden mehrerer Platzhalter, die für Bereiche und einzelne Zeichen stehen

In einer Reihe von Platzhaltern können sowohl Bereiche als auch einzelne Zeichen angegeben werden. Im folgenden Beispiel wird der []-Operator verwendet, um eine Zeichenfolge zu suchen, die mit einer Zahl oder einer Reihe von Sonderzeichen beginnt.

```sql
SELECT [object_id], OBJECT_NAME(object_id) AS [object_name], name, column_id 
FROM sys.columns 
WHERE name LIKE '[0-9!@#$.,;_]%';
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  

```
object_id     object_name                         name  column_id
---------     -----------                         ----  ---------
615673241     vSalesPersonSalesByFiscalYears      2002  5
615673241     vSalesPersonSalesByFiscalYears      2003  6
615673241     vSalesPersonSalesByFiscalYears      2004  7
1591676718    JunkTable                           _xyz  1
```
  
## <a name="see-also"></a>Weitere Informationen  
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)   
  [% &#40;Wildcard - Character&#40;s&#41; to Match&#41; &#40;Transact-SQL&#41; (% (Platzhalterzeichen – zu suchende(s) Zeichen) (Transact-SQL))](../../t-sql/language-elements/percent-character-wildcard-character-s-to-match-transact-sql.md)   
 [&#91;^&#93; &#40;Wildcard - Character&#40;s&#41; Not to Match&#41; &#40;Transact-SQL&#41; ([^] (Platzhalterzeichen – nicht zu suchende(s) Zeichen) (Transact-SQL))](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)     
 [\_ &#40;Wildcard - Match One Character&#41; &#40;Transact-SQL&#41; (_ (Platzhalterzeichen – einzelnes zu suchendes Zeichen) (Transact-SQL))](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)  
    
  
