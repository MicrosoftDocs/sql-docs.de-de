---
description: 'Logische Funktionen: IIF (Transact-SQL)'
title: IIF (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IIF_TSQL
- IIF
dev_langs:
- TSQL
helpviewer_keywords:
- IIF function
ms.assetid: e3ccf8ed-1cec-43ac-90b7-d8597c24b050
author: cawrites
ms.author: chadam
ms.openlocfilehash: 81b2e383d44386e14e6cca59ebf3973bc2dfc20c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208814"
---
# <a name="logical-functions---iif-transact-sql"></a>Logische Funktionen: IIF (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Gibt einen von zwei Werten zurück, abhängig davon, ob der boolesche Ausdruck "true" oder "false" in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ergibt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
IIF( boolean_expression, true_value, false_value )
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *boolean_expression*  
 Ein gültiger boolescher Ausdruck.  
  
 Wenn dieses Argument kein boolescher Ausdruck ist, wird ein Syntaxfehler ausgelöst.  
  
 *true_value*  
 Der zurückzugebende Wert, wenn *boolean_expression* den Wert TRUE ergibt.  
  
 *false_value*  
 Der zurückzugebende Wert, wenn *boolean_expression* den Wert FALSE ergibt.  
  
## <a name="return-types"></a>Rückgabetypen  
 Gibt den Datentyp mit dem höchsten Rang unter den Typen in *true_value* und in *false_value* zurück. Weitere Informationen finden Sie unter [Rangfolge der Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="remarks"></a>Bemerkungen  
 IIF ist eine schnelle Möglichkeit zum Schreiben eines CASE-Ausdrucks. Hierdurch wird der als erstes Argument übergebenen booleschen Ausdruck ausgewertet und eines der beiden anderen Argumente auf Grundlage des Ergebnisses der Auswertung zurückgegeben. Das heißt, dass bei einem zutreffenden (TRUE) booleschen Ausdruck *true_value* und bei einem unzutreffenden oder unbekannten (FALSE oder unbekannt) booleschen Ausdruck *false_value* zurückgegeben wird. *true_value* und *false_value* können einen beliebigen Typ aufweisen. Die gleichen Regeln, die für den CASE-Ausdruck für boolesche Ausdrücke, NULL-Behandlung und Rückgabetypen gelten, sind auch für IIF gültig. Weitere Informationen finden Sie unter [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md).  
  
 Die Tatsache, dass IIF in CASE übersetzt wird, wirkt sich auch auf andere Aspekte des Verhaltens dieser Funktion aus. Da CASE-Ausdrücke nur bis zur Ebene 10 geschachtelt werden können, können auch IIF-Anweisungen nur bis zu einer maximalen Ebene von 10 geschachtelt werden. Außerdem wird IIF remote an andere Server als semantisch gleichwertiger CASE-Ausdruck übergeben, einschließlich aller Verhaltensweisen eines remote ausgeführten CASE-Ausdrucks.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-simple-iif-example"></a>A. Einfaches Beispiel für IIF  
  
```sql  
DECLARE @a INT = 45, @b INT = 40;
SELECT [Result] = IIF( @a > @b, 'TRUE', 'FALSE' );
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------  
TRUE  
```  
  
### <a name="b-iif-with-null-constants"></a>B. IIF mit NULL-Konstanten  
  
```sql 
SELECT [Result] = IIF( 45 > 30, NULL, NULL );
```  
  
 Das Ergebnis dieser Anweisung ist ein Fehler.  
  
### <a name="c-iif-with-null-parameters"></a>C. IIF mit NULL-Parametern  
  
```sql  
DECLARE @P INT = NULL, @S INT = NULL;  
SELECT [Result] = IIF( 45 > 30, @P, @S );
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------  
NULL  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [CHOOSE &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-choose-transact-sql.md)  
  
  
