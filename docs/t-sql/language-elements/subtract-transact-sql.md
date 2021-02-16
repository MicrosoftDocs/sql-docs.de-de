---
description: '- (Subtraktion) (Transact-SQL)'
title: '- (Subtraktion) (Transact-SQL) | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- subtract
- '-'
- -_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- '- (subtract)'
- subtract operator (-)
- minus operator (-)
- subtracting numbers
ms.assetid: db23145f-f17d-4b90-be09-28a881cacd1a
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9e791bacd7c59d0160c13f819218ff5b9b49b5a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341601"
---
# <a name="--subtraction-transact-sql"></a>- (Subtraktion) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Subtrahiert zwei Zahlen (ein arithmetischer Subtraktionsoperator). Kann auch eine Zahl, in Tagen, von einem Datum subtrahieren.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
expression - expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *expression*  
 Ein beliebiger gültiger [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md) eines der Datentypen der numerischen Datentypkategorie, mit Ausnahme des **bit**-Datentyps. Der Operator kann mit den Datentypen **date**, **time**, **datetime2** oder **datetimeoffset** verwendet werden.  
  
## <a name="result-types"></a>Ergebnistypen  
 Gibt einen Wert vom Datentyp des Arguments zurück, das in der Rangfolge höher steht. Weitere Informationen finden Sie unter [Rangfolge der Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-subtraction-in-a-select-statement"></a>A. Verwenden der Subtraktion in einer SELECT-Anweisung  
 Im folgenden Beispiel wird der Unterschied im Steuersatz zwischen dem Staat bzw. der Provinz mit dem höchsten Steuersatz und dem Staat oder der Provinz mit dem niedrigsten Steuersatz berechnet.  
  
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
```sql  
-- Uses AdventureWorks  
  
SELECT MAX(TaxRate) - MIN(TaxRate) AS 'Tax Rate Difference'  
FROM Sales.SalesTaxRate  
WHERE StateProvinceID IS NOT NULL;  
GO  
```  
  
 Sie können die Reihenfolge der Ausführung ändern, indem Sie Klammern verwenden. Berechnungen innerhalb von Klammern werden zuerst ausgewertet. Bei geschachtelten Klammerausdrücken werden die innersten Bestandteile zuerst berechnet.  
  
### <a name="b-using-date-subtraction"></a>B. Verwenden der Subtraktion von Datumsangaben  
 Im folgenden Beispiel wird eine Anzahl von Tagen von einem `datetime`-Datum abgezogen.  
  
 Gilt für: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @altstartdate DATETIME;  
SET @altstartdate = CONVERT(DATETIME, ''January 10, 1900 3:00 AM', 101);  
SELECT @altstartdate - 1.5 AS 'Subtract Date';  
```  
  
 Im Folgenden wird das Resultset aufgeführt:  
  
 ```
 Subtract Date  
 -----------------------  
 1900-01-08 15:00:00.000  

 (1 row(s) affected)
 ```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-subtraction-in-a-select-statement"></a>C. Verwenden der Subtraktion in einer SELECT-Anweisung  
 Im folgenden Beispiel wird der Unterschied im Basistarif zwischen dem Mitarbeiter mit dem höchsten Tarif und dem Mitarbeiter mit dem niedrigsten Tarif aus der `dimEmployee`-Tabelle berechnet.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT MAX(BaseRate) - MIN(BaseRate) AS BaseRateDifference  
FROM DimEmployee;  
```  
  
## <a name="see-also"></a>Siehe auch  
 [-= &#40;Subtraktionszuweisung&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/subtract-equals-transact-sql.md)   
 [Verbundoperatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)  
 [Arithmetische Operatoren &#40;Transact-SQL&#41;](../../t-sql/language-elements/arithmetic-operators-transact-sql.md)   
 [- &#40;Negative&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/unary-operators-negative.md)   
 [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Ausdrücke &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Integrierte Funktionen &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
  
  


