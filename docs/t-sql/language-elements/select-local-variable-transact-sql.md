---
description: SELECT @local_variable (Transact-SQL)
title: SELECT @local_variable (Transact-SQL)
ms.custom: ''
ms.date: 09/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- variable_TSQL
- '@loca_variable'
- '@local'
- variable
- '@loca_variable_TSQL'
- '@local_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- variables [SQL Server], assigning
- SELECT statement [SQL Server], @local_variable
- '@local_variable'
- local variables [SQL Server]
ms.assetid: 8e1a9387-2c5d-4e51-a1fd-a2a95f026d6f
author: cawrites
ms.author: chadam
monikerRange: = azuresqldb-current ||>= sql-server-2016 ||= azure-sqldw-latest||>= sql-server-linux-2017
ms.openlocfilehash: 3b1545d50733a5139c0a4d6ef22f22b719691fc5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100328"
---
# <a name="select-local_variable-transact-sql"></a>SELECT @local_variable (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Legt eine lokale Variable auf den Wert eines Ausdrucks fest.  
  
 Zum Zuweisen von Variablen empfiehlt es sich, dass Sie [SET@local_variable](../../t-sql/language-elements/set-local-variable-transact-sql.md) anstelle von SELECT @*local_variable* verwenden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
SELECT { @local_variable { = | += | -= | *= | /= | %= | &= | ^= | |= } expression } 
    [ ,...n ] [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

@*local_variable*  
 Dies ist eine deklarierte Variable, der ein Wert zugewiesen wird.  
  
{= \| += \| -= \| \*= \| /= \| %= \| &= \| ^= \| \|= }  
Den Wert auf der rechten Seite der Variablen auf der linken Seite zuweisen.  
  
Verbundzuweisungsoperator:  

| Operator | action |  
| -------- | ------ |  
| = | Weist der Variable den darauf folgenden Ausdruck zu. |  
| += | Addition und Zuweisung |  
| -= | Subtraktion und Zuweisung |  
| \*= | Multiplikation und Zuweisung |  
| /= | Division und Zuweisung |  
| %= | Modulo und Zuweisung |  
| &= | Bitweises UND und Zuweisung |  
| ^= | Bitweises XOR und Zuweisung |  
| \|= | Bitweises OR und Zuweisung |  

*expression*  
Ein beliebiger gültiger [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md). Er enthält eine skalare Unterabfrage.  

## <a name="remarks"></a>Bemerkungen

SELECT @*local_variable* dient in der Regel dazu, einen einzelnen Wert in die Variable zurückzugeben. Wenn es sich bei *expression* jedoch um den Namen einer Spalte handelt, können auch mehrere Werte zurückgegeben werden. Falls die SELECT-Anweisung mehr als einen Wert zurückgibt, wird der Variablen der zuletzt zurückgegebene Wert zugewiesen.  

Wenn die SELECT-Anweisung keine Zeilen zurückgibt, behält die Variable ihren derzeitigen Wert bei. Ist *expression* eine skalare Unterabfrage, die keinen Wert zurückgibt, wird die Variable auf NULL festgelegt.  

Eine SELECT-Anweisung kann mehrere lokale Variablen initialisieren.  

> [!NOTE]
> Eine SELECT-Anweisung, die eine Variablenzuweisung enthält, kann nicht zugleich zur Durchführung der normalen Abrufvorgänge für Resultsets verwendet werden.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-use-select-local_variable-to-return-a-single-value"></a>A. Verwenden von SELECT @local_variable zum Zurückgeben eines einzelnen Wertes  
 Das folgende Beispiel weist der Variable `@var1` den Wert `Generic Name` zu. Die Abfrage der Tabelle `Store` gibt keine Zeilen zurück, da der für `CustomerID` angegebene Wert nicht in der Tabelle enthalten ist. Die Variable behält den Wert `Generic Name` bei.  
  
```sql  
-- Uses AdventureWorks    
  
DECLARE @var1 VARCHAR(30);         
SELECT @var1 = 'Generic Name';         
SELECT @var1 = Name         
FROM Sales.Store         
WHERE CustomerID = 1000 ;        
SELECT @var1 AS 'Company Name';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Company Name  
 ------------------------------  
 Generic Name  
 ```  
  
### <a name="b-use-select-local_variable-to-return-null"></a>B. Verwenden von SELECT @local_variable zum Zurückgeben von NULL  
 Das folgende Beispiel weist der Variable `@var1` mithilfe einer Unterabfrage einen Wert zu. Da der für `CustomerID` angeforderte Wert nicht vorhanden ist, gibt die Unterabfrage keinen Wert zurück, und die Variable wird auf `NULL` gesetzt.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @var1 VARCHAR(30)   
SELECT @var1 = 'Generic Name'   
SELECT @var1 = (SELECT Name   
FROM Sales.Store   
WHERE CustomerID = 1000)   
SELECT @var1 AS 'Company Name' ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Company Name  
----------------------------  
NULL  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [DEKLARIEREN SIE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Verbundoperatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  
