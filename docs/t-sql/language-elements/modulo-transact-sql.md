---
description: '% (Modulo) (Transact-SQL)'
title: '% (Modulo) (Transact-SQL) | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- modulo
- modulus
- '% (Modulo)'
- '% (Modulus)'
- MOD_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- '% (modulo operator)'
- '% (modulus operator)'
- remainder of division operation
- modulo operator (%)
- modulus operator (%)
ms.assetid: f93c662e-f405-486e-bf23-a2d03907b5bd
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1c523954ddcf3b326823f01c81bf89a8e56f4342
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351640"
---
# <a name="-modulus-transact-sql"></a>% (Modulo) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt den Rest der Division einer Zahl durch eine andere zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
dividend % divisor  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *dividend*  
 Der zu dividierende numerische Ausdruck. *dividend* muss ein gültiger [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md) eines Datentyps aus den Datentypkategorien für Ganzzahlen und Währungen oder vom Datentyp **numeric** sein.  
  
 *divisor*  
 Der numerische Ausdruck, durch den der Dividend geteilt werden soll. *divisor* muss ein gültiger Ausdruck eines Datentyps aus den Datentypkategorien für Ganzzahlen und Währungen oder vom Datentyp **numeric** sein.  
  
## <a name="result-types"></a>Ergebnistypen  
 Die Ergebnistypen werden von den Datentypen der beiden Argumente bestimmt.  
  
## <a name="remarks"></a>Bemerkungen  
 Sie können den arithmetischen Operator Modulo mit jeder Kombination aus Spaltennamen, numerischen Konstanten oder einem gültigen Ausdruck aus den Datentypkategorien für Ganzzahlen oder Währungen oder dem Datentyp **numeric** in der SELECT-Liste der SELECT-Anweisung verwenden.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-simple-example"></a>A. Einfaches Beispiel  
 Im folgenden Beispiel wird die Zahl 38 durch 5 geteilt. Das Ergebnis 7 ist der ganzzahlige Anteil am Ergebnis, woraus sich ein Rest (Modulo) von 3 ergibt.  
  
```sql  
SELECT 38 / 5 AS Integer, 38 % 5 AS Remainder;
```  
  
### <a name="b-example-using-columns-in-a-table"></a>B. Beispiel für Spalten in einer Tabelle  
 Das folgende Beispiel gibt die Product ID, den Einzelpreis des Produkts und den Modulo (Rest) aus der Division des Preises jedes Produkts, konvertiert in eine ganze Zahl, durch die Anzahl der bestellten Produkte zurück.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT TOP(100)ProductID, UnitPrice, OrderQty,  
   CAST((UnitPrice) AS INT) % OrderQty AS Modulo  
FROM Sales.SalesOrderDetail;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-simple-example"></a>C. Einfaches Beispiel  
 Das folgende Beispiel zeigt die Ergebnisse für den `%`-Operator nach der Division von 3 durch 2.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT TOP(1) 3%2 FROM dimEmployee;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------   
1         
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integrierte Funktionen &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [&#40;Modulozuweisung&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/modulo-equals-transact-sql.md)   
 [Verbundoperatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


