---
description: + (Addition) (Transact-SQL)
title: + (Addition) (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- add
- +
- +_TSQL
- + (Add)
dev_langs:
- TSQL
helpviewer_keywords:
- addition (+)
- adding numbers
- + (add)
- plus sign (+)
- add operator (+)
ms.assetid: 4ba8baac-5f07-432c-87c5-d23e7011da55
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7cb79a2abca58ebd00207b020ceae63d44a6d132
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352391"
---
# <a name="-addition-transact-sql"></a>+ (Addition) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Addition zweier Zahlen. Mit diesem arithmetischen Operator für die Addition kann auch eine Anzahl von Tagen zu einem Datum addiert werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
expression + expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *expression*  
 Bezeichnet jeden gültigen [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md) eines beliebigen Datentyps aus der numerischen Kategorie, mit Ausnahme des **bit**-Datentyps. Der Operator kann mit den Datentypen **date**, **time**, **datetime2** oder **datetimeoffset** verwendet werden.  
  
## <a name="result-types"></a>Ergebnistypen  
 Gibt einen Wert vom Datentyp des Arguments zurück, das in der Rangfolge höher steht. Weitere Informationen finden Sie unter [Rangfolge der Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-the-addition-operator-to-calculate-the-total-number-of-hours-away-from-work-for-each-employee"></a>A. Berechnen der Gesamtanzahl der Stunden für jeden Mitarbeiter, die nicht am Arbeitsplatz verbracht worden sind.  
 Im folgenden Beispiel wird die Gesamtanzahl von Stunden, die ein Mitarbeiter nicht an seinem Arbeitsplatz verbringt, durch Addieren der Urlaubs- und Krankheitstage (in Stunden) ermittelt.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, VacationHours, SickLeaveHours,   
    VacationHours + SickLeaveHours AS 'Total Hours Away'  
FROM HumanResources.Employee AS e  
    JOIN Person.Person AS p ON e.BusinessEntityID = p.BusinessEntityID  
ORDER BY 'Total Hours Away' ASC;  
GO  
```  
  
### <a name="b-using-the-addition-operator-to-add-days-to-date-and-time-values"></a>B. Verwenden des Additionsoperators, um Tage zu Datums- und Zeitwerten zu addieren  
 Im folgenden Beispiel wird eine Anzahl von Tagen zu einem `datetime`-Datum addiert.  
  
```sql
SET NOCOUNT ON  
DECLARE @startdate DATETIME, @adddays INT;  
SET @startdate = 'January 10, 1900 12:00 AM';  
SET @adddays = 5;  
SET NOCOUNT OFF;  
SELECT @startdate + 1.25 AS 'Start Date',   
   @startdate + @adddays AS 'Add Date';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Start Date                  Add Date
--------------------------- ---------------------------
1900-01-11 06:00:00.000     1900-01-15 00:00:00.000
  
(1 row(s) affected)
 ```  
  
### <a name="c-adding-character-and-integer-data-types"></a>C. Addieren bei Zeichen- und ganzzahligen Datentypen  
 Im folgenden Beispiel werden Werte eines **int**-Datentyps und eines Zeichendatentyps addiert, indem der Zeichendatentyp in **int** konvertiert wird. Wenn eine **char**-Zeichenfolge ein ungültiges Zeichen enthält, gibt [!INCLUDE[tsql](../../includes/tsql-md.md)] einen Fehler zurück.  
  
```sql  
DECLARE @addvalue INT;  
SET @addvalue = 15;  
SELECT '125127' + @addvalue;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
-----------------------
125142
  
(1 row(s) affected)
 ```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-the-addition-operator-to-calculate-the-total-number-of-hours-away-from-work-for-each-employee"></a>D: Verwenden des Additionsoperators zur Berechnung der Gesamtstunden, die ein Mitarbeiter nicht an seinem Arbeitsplatz verbringt.  
 Im folgenden Beispiel wird die Gesamtanzahl von Stunden, die ein Mitarbeiter nicht an seinem Arbeitsplatz verbringt, durch Addieren der Urlaubs- und Krankheitstage (in Stunden) ermittelt und in aufsteigender Reihenfolge sortiert.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, VacationHours, SickLeaveHours,   
    VacationHours + SickLeaveHours AS TotalHoursAway  
FROM DimEmployee  
ORDER BY TotalHoursAway ASC;  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Verbundoperatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)   
 [+= &#40;Additionszuweisung&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/add-equals-transact-sql.md)   
 [CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Datentypkonvertierung &#40;Datenbank-Engine&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Integrierte Funktionen &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  
  
  


