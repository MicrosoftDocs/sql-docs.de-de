---
description: LAST_VALUE (Transact-SQL)
title: LAST_VALUE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/22/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LAST_VALUE
- LAST_VALUE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- analytic functions, LAST_VALUE
- LAST_VALUE function
ms.assetid: fd833e34-8092-42b7-80fc-95ca6b0eab6b
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8121b8cb50ff42114196973af17efa42992e16b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343571"
---
# <a name="last_value-transact-sql"></a>LAST_VALUE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Diese Funktion gibt den letzten Wert in einer geordneten Wertemenge zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  

```syntaxsql 
LAST_VALUE ( [ scalar_expression ] )  [ IGNORE NULLS | RESPECT NULLS ]
    OVER ( [ partition_by_clause ] order_by_clause rows_range_clause )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *scalar_expression*  
 Der zurückzugebende Wert. *scalar_expression* kann eine Spalte, eine Unterabfrage oder ein anderer Ausdruck sein, durch die sich ein einzelner Wert ergibt. Andere analytische Funktionen sind nicht zulässig.  
  
 [ IGNORE NULLS | RESPECT NULLS ]     
 **Gilt für**: Azure SQL Edge

 IGNORE NULLS: Hier werden NULL-Werte im Dataset ignoriert, wenn der letzte Wert einer Partition berechnet wird.     
 RESPECT NULLS: Hier werden NULL-Werte im Dataset berücksichtigt, wenn der letzte Wert einer Partition berechnet wird.     
 
  Weitere Informationen finden Sie unter [Eingeben fehlender Werte](/azure/azure-sql-edge/imputing-missing-values/).
  
 OVER **(** [ *partition_by_clause* ] *order_by_clause* [ *rows_range_clause* ] **)**  
 *partition_by_clause* unterteilt das von der FROM-Klausel erzeugte Resultset in Partitionen, auf die die Funktion angewendet wird. Wird dies nicht angegeben, verarbeitet die Funktion alle Zeilen des Abfrageresultsets als einzelne Gruppe.  
  
 *order_by_clause* bestimmt die Reihenfolge der Daten, bevor die Funktion angewendet wird. *order_by_clause* ist erforderlich. *rows_range_clause* schränkt die Zeilen innerhalb der Partition weiter ein, indem Ausgangs- und Endpunkte angegeben werden. Weitere Informationen finden Sie unter [OVER-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Rückgabetypen  
 Entspricht dem Typ von *scalar_expression*.  
  
## <a name="general-remarks"></a>Allgemeine Hinweise  
 LAST_VALUE ist nicht deterministisch. Weitere Informationen finden Sie unter [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-last_value-over-partitions"></a>A. Partitionsweise Verwendung von LAST_VALUE  
 Im folgenden Beispiel wird das Einstellungsdatum des letzten Mitarbeiters in jeder Abteilung für das angegebene Gehalt ("Rate") zurückgegeben. Die PARTITION BY-Klausel partitioniert die Mitarbeiter nach Abteilung, und die LAST_VALUE-Funktion wird auf jede Partition separat angewendet. Die in der OVER-Klausel angegebene ORDER BY-Klausel bestimmt die logische Reihenfolge, in der die LAST_VALUE-Funktion auf die Zeilen in jeder Partition angewendet wird.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Department, LastName, Rate, HireDate,   
    LAST_VALUE(HireDate) OVER (PARTITION BY Department ORDER BY Rate) AS LastValue  
FROM HumanResources.vEmployeeDepartmentHistory AS edh  
INNER JOIN HumanResources.EmployeePayHistory AS eph    
    ON eph.BusinessEntityID = edh.BusinessEntityID  
INNER JOIN HumanResources.Employee AS e  
    ON e.BusinessEntityID = edh.BusinessEntityID  
WHERE Department IN (N'Information Services',N'Document Control');   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
Department                  LastName                Rate         HireDate     LastValue  
--------------------------- ----------------------- ------------ ----------   ----------  
Document Control            Chai                    10.25        2003-02-23   2003-03-13  
Document Control            Berge                   10.25        2003-03-13   2003-03-13  
Document Control            Norred                  16.8269      2003-04-07   2003-01-17  
Document Control            Kharatishvili           16.8269      2003-01-17   2003-01-17  
Document Control            Arifin                  17.7885      2003-02-05   2003-02-05  
Information Services        Berg                    27.4038      2003-03-20   2003-01-24  
Information Services        Meyyappan               27.4038      2003-03-07   2003-01-24  
Information Services        Bacon                   27.4038      2003-02-12   2003-01-24  
Information Services        Bueno                   27.4038      2003-01-24   2003-01-24  
Information Services        Sharma                  32.4519      2003-01-05   2003-03-27  
Information Services        Connelly                32.4519      2003-03-27   2003-03-27  
Information Services        Ajenstat                38.4615      2003-02-18   2003-02-23  
Information Services        Wilson                  38.4615      2003-02-23   2003-02-23  
Information Services        Conroy                  39.6635      2003-03-08   2003-03-08  
Information Services        Trenary                 50.4808      2003-01-12   2003-01-12  
  
```  
  
### <a name="b-using-first_value-and-last_value-in-a-computed-expression"></a>B. Verwenden von FIRST_VALUE und LAST_VALUE in einem berechneten Ausdruck  
 Im folgenden Beispiel werden die FIRST_VALUE-Funktion und die LAST_VALUE-Funktion in berechneten Ausdrücken verwendet, um den Unterschied zwischen den Verkaufszahlen für das laufende Quartal und das erste bzw. letzte Quartal des Jahres für eine bestimmte Anzahl von Mitarbeitern aufzuzeigen. Die FIRST_VALUE-Funktion gibt den Verkaufszahlenwert für das erste Quartal des Jahres zurück und subtrahiert diesen von den Verkaufszahlen für das aktuelle Quartal. Der Wert wird in der abgeleiteten Spalte mit dem Titel "DifferenceFromFirstQuarter" zurückgegeben. Für das erste Quartal eines Jahres ist der Wert der Spalte "DifferenceFromFirstQuarter" 0. Die LAST_VALUE-Funktion gibt den Verkaufszahlenwert für das letzte Quartal des Jahres zurück und subtrahiert diesen von den Verkaufszahlen für das aktuelle Quartal. Der Wert wird in der abgeleiteten Spalte mit dem Titel "DifferenceFromLastQuarter" zurückgegeben. Für das letzte Quartal eines Jahres ist der Wert der Spalte "DifferenceFromLastQuarter" 0.  
  
 Die Klausel „RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING“ muss in diesem Beispiel wie unten dargestellt für die Werte ungleich NULL in der Spalte „DifferenceFromLastQuarter“ zurückgegeben werden. Der Standardbereich lautet „RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW“. In diesem Beispiel führt die Verwendung dieses Standardbereichs (oder ausschließlich eines Bereichs, wodurch der Standardbereich verwendet wird) dazu, dass Nullen (0) in der Spalte "DifferenceFromLastQuarter" zurückgegeben werden. Weitere Informationen finden Sie unter [OVER-Klausel &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
SELECT BusinessEntityID, DATEPART(QUARTER,QuotaDate)AS Quarter, YEAR(QuotaDate) AS SalesYear,   
    SalesQuota AS QuotaThisQuarter,   
    SalesQuota - FIRST_VALUE(SalesQuota)   
        OVER (PARTITION BY BusinessEntityID, YEAR(QuotaDate)   
              ORDER BY DATEPART(QUARTER,QuotaDate) ) AS DifferenceFromFirstQuarter,   
    SalesQuota - LAST_VALUE(SalesQuota)   
        OVER (PARTITION BY BusinessEntityID, YEAR(QuotaDate)   
              ORDER BY DATEPART(QUARTER,QuotaDate)   
              RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING ) AS DifferenceFromLastQuarter   
FROM Sales.SalesPersonQuotaHistory   
WHERE YEAR(QuotaDate) > 2005   
AND BusinessEntityID BETWEEN 274 AND 275   
ORDER BY BusinessEntityID, SalesYear, Quarter;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID Quarter     SalesYear   QuotaThisQuarter      DifferenceFromFirstQuarter DifferenceFromLastQuarter  
---------------- ----------- ----------- --------------------- --------------------------- -----------------------  
274              1           2006        91000.00              0.00                        -63000.00  
274              2           2006        140000.00             49000.00                    -14000.00  
274              3           2006        70000.00              -21000.00                   -84000.00  
274              4           2006        154000.00             63000.00                    0.00  
274              1           2007        107000.00             0.00                        -9000.00  
274              2           2007        58000.00              -49000.00                   -58000.00  
274              3           2007        263000.00             156000.00                   147000.00  
274              4           2007        116000.00             9000.00                     0.00  
274              1           2008        84000.00              0.00                        -103000.00  
274              2           2008        187000.00             103000.00                   0.00  
275              1           2006        502000.00             0.00                        -822000.00  
275              2           2006        550000.00             48000.00                    -774000.00  
275              3           2006        1429000.00            927000.00                   105000.00  
275              4           2006        1324000.00            822000.00                   0.00  
275              1           2007        729000.00             0.00                        -489000.00  
275              2           2007        1194000.00            465000.00                   -24000.00  
275              3           2007        1575000.00            846000.00                   357000.00  
275              4           2007        1218000.00            489000.00                   0.00  
275              1           2008        849000.00             0.00                        -20000.00  
275              2           2008        869000.00             20000.00                    0.00  
  
(20 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Weitere Informationen  

 [First_Value &#40;Transact-SQL&#41;](first-value-transact-sql.md)  
