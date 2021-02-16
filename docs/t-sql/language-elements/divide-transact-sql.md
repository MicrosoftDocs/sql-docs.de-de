---
description: / (Division) (Transact-SQL)
title: (Division) (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- /
- /_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- / (divide)
- division [SQL Server]
- divide operator (/)
ms.assetid: 1d69893b-e5c3-441d-8dd8-0e5eb872ecfc
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 751b3ebbd2924b90b657d94ab6356214c1f5a911
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347585"
---
# <a name="-division-transact-sql"></a>/ (Division) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Division einer Zahl durch eine andere (arithmetischer Operator für die Division).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
dividend / divisor  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *dividend*  
 Der zu dividierende numerische Ausdruck. *dividend* kann ein gültiger [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md) eines Datentyps der numerischen Datentypkategorie sein. Dies gilt allerdings nicht für die Datentypen **datetime** und **smalldatetime**.  
  
 *divisor*  
 Der numerische Ausdruck, durch den der Dividend geteilt werden soll. *divisor* kann ein gültiger Ausdruck eines Datentyps der numerischen Datentypkategorie sein. Dies gilt allerdings nicht für die Datentypen **datetime** und **smalldatetime**.  
  
## <a name="result-types"></a>Ergebnistypen  
 Gibt einen Wert vom Datentyp des Arguments zurück, das in der Rangfolge höher steht. Weitere Informationen finden Sie unter [Rangfolge der Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
 Wenn ein *dividend*-Integer durch einen *divisor*-Integer geteilt wird, ist das Ergebnis ein Integer, dessen Dezimalstellen abgeschnitten werden.  
  
## <a name="remarks"></a>Bemerkungen  
 Der Ist-Wert, der vom Operator / zurückgegeben wird, ist der Quotient aus dem ersten Ausdruck geteilt durch den zweiten.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der arithmetische Operator für die Division zum Berechnen des Umsatzzieles pro Monat für die Vertriebsmitarbeiter von [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] verwendet.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT s.BusinessEntityID AS SalesPersonID, FirstName, LastName, SalesQuota, SalesQuota/12 AS 'Sales Target Per Month'  
FROM Sales.SalesPerson AS s   
JOIN HumanResources.Employee AS e   
    ON s.BusinessEntityID = e.BusinessEntityID  
JOIN Person.Person AS p   
    ON e.BusinessEntityID = p.BusinessEntityID;  
```  
  
 Dies ist ein Auszug aus dem Resultset.  
  
```  
  
SalesPersonID FirstName    LastName          SalesQuota  Sales Target Per Month  
------------- ------------ ----------------- ----------- ------------------  
274           Stephen      Jiang             NULL        NULL  
275           Michael      Blythe            300000.00   25000.00  
276           Linda        Mitchell          250000.00   20833.3333  
277           Jillian      Carson            250000.00   20833.3333  
  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Im folgenden Beispiel wird der Operator für die arithmetische Division dazu verwendet, das einfache Verhältnis von Urlaubs- und Krankheitszeit für jeden Mitarbeiter zu berechnen.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, VacationHours/SickLeaveHours AS PersonalTimeRatio  
FROM DimEmployee;  
  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Integrierte Funktionen &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [/= &#40;Division Assignment&#41; &#40;Transact-SQL&#41; (/= &#40;Divisionszuweisung&#41; &#40;Transact-SQL&#41;)](../../t-sql/language-elements/divide-equals-transact-sql.md)   
 [Verbundoperatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


