---
description: APPROX_COUNT_DISTINCT (Transact-SQL)
title: APPROX_COUNT_DISTINCT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- APPROX_COUNT_DISTINCT
dev_langs:
- TSQL
author: joesackmsft
ms.author: josack
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c041986f79779c5eeeeb6a1a11b7f77abfe5eebe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270655"
---
# <a name="approx_count_distinct-transact-sql"></a>APPROX_COUNT_DISTINCT (Transact-SQL)

[!INCLUDE [sqlserver2019-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2019-asdb-asdbmi-asa.md)]

Diese Funktion gibt die ungefähre Anzahl von eindeutigen Ungleich-Null-Werten in einer Gruppe zurück. 
  
![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
APPROX_COUNT_DISTINCT ( expression )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
*expression*  
Eine [expression](../../t-sql/language-elements/expressions-transact-sql.md) beliebigen Typs mit Ausnahme von **image**, **sql_variant**, **ntext** oder **text**. 

## <a name="return-types"></a>Rückgabetypen
 **bigint**  
  
## <a name="remarks"></a>Bemerkungen  
`APPROX_COUNT_DISTINCT( expression )` wertet einen Ausdruck für jede Zeile in einer Gruppe aus und gibt die geschätzte Anzahl der eindeutigen Werte in einer Gruppe zurück, die nicht NULL sind. Diese Funktion wurde entwickelt, um Aggregationen über große Datasets hinweg bereitzustellen, bei denen die Reaktionsfähigkeit wichtiger ist als die absolute Präzision.  

`APPROX_COUNT_DISTINCT` ist für den Einsatz in großen Datenszenarien konzipiert und für die folgenden Bedingungen optimiert:
- Zugriff auf Datasets, die aus Millionen von Zeilen oder mehr bestehen *und*
- Aggregation einer Spalte oder von Spalten mit vielen unterschiedlichen Werten

Die Funktionsimplementierung garantiert eine Fehlerquote von bis zu 2 % mit einer Wahrscheinlichkeit von 97 %. 

`APPROX_COUNT_DISTINCT` erfordert weniger Speicherplatz als ein vollständiger COUNT DISTINCT-Vorgang.  Angesichts des geringeren Speicherbedarfs ist es weniger wahrscheinlich, dass durch `APPROX_COUNT_DISTINCT` weniger Speicher zum Datenträger überläuft, als bei einem präzisen COUNT DISTINCT-Vorgang. Weitere Informationen zu dem Algorithmus, mit dem dies erreicht wird, finden Sie unter [HyperLogLog](https://en.wikipedia.org/wiki/HyperLogLog).

> [!NOTE]
> Bei sortierungsbezogenen Zeichenfolgen verwendet APPROX_COUNT_DISTINCT einen binären Abgleich und liefert Ergebnisse, die bei Vorhandensein von BIN-Sortierungen und nicht von BIN2 erzeugt worden wären. 
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-approx_count_distinct"></a>A. Verwenden von APPROX_COUNT_DISTINCT 
Dieses Beispiel gibt die ungefähre Anzahl verschiedener Sortierschlüssel aus der Sortiertabelle zurück.
  
```sql
SELECT APPROX_COUNT_DISTINCT(O_OrderKey) AS Approx_Distinct_OrderKey
FROM dbo.Orders;
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Approx_Distinct_OrderKey
------------------------
15164704
```
  
### <a name="b-using-approx_count_distinct-with-group-by"></a>B. Verwenden von APPROX_COUNT_DISTINCT mit GROUP BY 
Dieses Beispiel gibt die ungefähre Anzahl verschiedener Sortierschlüssel geordnet nach Sortierstatus aus der Sortiertabelle zurück. 
  
```sql
SELECT O_OrderStatus, APPROX_COUNT_DISTINCT(O_OrderKey) AS Approx_Distinct_OrderKey
FROM dbo.Orders
GROUP BY O_OrderStatus
ORDER BY O_OrderStatus; 
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
O_OrderStatus                                                    Approx_Distinct_OrderKey
---------------------------------------------------------------- ------------------------
F                                                                7397838
O                                                                7387803
P                                                                388036
```
    
## <a name="see-also"></a>Weitere Informationen
[Aggregate Functions &#40;Transact-SQL&#41; (Aggregatfunktionen (Transact-SQL))](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[COUNT &#40;Transact-SQL&#41;](../../t-sql/functions/count-transact-sql.md) 
