---
description: AND (Transact-SQL)
title: AND (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- AND_TSQL
- AND
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], TRUE
- "TRUE"
- AND, about AND operators
- AND
- combining expressions
ms.assetid: b61d7f8d-5a51-49b7-91dd-f6190a5a0fb9
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c4d9a4169bac272cf57699a1436e36b53181e390
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92196868"
---
# <a name="and-transact-sql"></a>AND (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Kombiniert zwei boolesche Ausdrücke und gibt **TRUE** zurück, wenn beide Ausdrücke **TRUE** sind. Werden in einer Anweisung mehrere logische Operatoren verwendet, werden zuerst die **AND**-Operatoren ausgewertet. Sie können die Auswertungsreihenfolge ändern, indem Sie Klammern verwenden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
boolean_expression AND boolean_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *boolean_expression*  
 Ein beliebiger gültiger [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md), der **TRUE**, **FALSE** oder **UNKNOWN** zurückgibt.  
  
## <a name="result-types"></a>Ergebnistypen  
 **Boolescher Wert**  
  
## <a name="result-value"></a>Ergebniswert  
 Gibt TRUE zurück, wenn beide Ausdrücke TRUE sind.  
  
## <a name="remarks"></a>Bemerkungen  
 Das folgende Diagramm zeigt die Ergebnisse des Vergleichs von TRUE- und FALSE-Werten mit dem AND-Operator.  
  
||true|false|UNKNOWN|  
|------|----------|-----------|-------------|  
|**TRUE**|true|false|UNKNOWN|  
|**FALSE**|FALSE|FALSE|false|  
|**UNKNOWN**|UNKNOWN|FALSE|UNKNOWN|  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-the-and-operator"></a>A. Verwenden des AND-Operators  
 Im folgenden Beispiel werden Informationen zu Mitarbeitern, die sowohl den Titel `Marketing Assistant` führen als auch mehr als `41` Resturlaubsstunden haben, ausgewählt.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT  BusinessEntityID, LoginID, JobTitle, VacationHours   
FROM HumanResources.Employee  
WHERE JobTitle = 'Marketing Assistant'  
AND VacationHours > 41 ;  
```  
  
### <a name="b-using-the-and-operator-in-an-if-statement"></a>B. Verwenden des AND-Operators in einer IF-Anweisung  
 In den folgenden Beispielen wird gezeigt, wie AND in einer IF-Anweisung verwendet wird. In der ersten Anweisung ist sowohl `1 = 1` als auch `2 = 2` zutreffend. Das Ergebnis lautet deshalb TRUE. Im zweiten Beispiel trifft das Argument `2 = 17` nicht zu. Das Ergebnis lautet daher FALSE.  
  
```sql  
IF 1 = 1 AND 2 = 2  
BEGIN  
   PRINT 'First Example is TRUE'  
END  
ELSE PRINT 'First Example is FALSE' ;  
GO  
  
IF 1 = 1 AND 2 = 17  
BEGIN  
   PRINT 'Second Example is TRUE'  
END  
ELSE PRINT 'Second Example is FALSE' ;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integrierte Funktionen &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  
