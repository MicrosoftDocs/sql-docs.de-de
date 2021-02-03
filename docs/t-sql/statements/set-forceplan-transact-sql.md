---
description: SET FORCEPLAN (Transact-SQL)
title: SET FORCEPLAN (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_FORCEPLAN_TSQL
- SET FORCEPLAN
- FORCEPLAN
- FORCEPLAN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- joins [SQL Server], overriding query optimizer process
- FORCEPLAN option
- SET FORCEPLAN statement
- query optimizer [SQL Server], optimizing process
- overriding query optimizer process
ms.assetid: b6c0b08f-2060-4696-9e12-50cb7e674321
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c60afa83af727a1f6c5090554f87aff11bbf49c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206995"
---
# <a name="set-forceplan-transact-sql"></a>SET FORCEPLAN (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Wenn FORCEPLAN auf ON festgelegt wird, verarbeitet der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Abfrageoptimierer einen Join in der Reihenfolge, in der die Tabellen in der FROM-Klausel einer Abfrage aufgeführt sind. Darüber hinaus wird durch Festlegen von FORCEPLAN auf ON die Verwendung eines Nested Loops-Joins erzwungen, sofern nicht andere Jointypen zum Erstellen eines Plans für die Abfrage erforderlich sind oder durch Join- bzw. Abfragehinweise angefordert werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
SET FORCEPLAN { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Bemerkungen
 SET FORCEPLAN überschreibt im Wesentlichen die Logik, die der Abfrageoptimierer beim Verarbeiten einer [!INCLUDE[tsql](../../includes/tsql-md.md)]-SELECT-Anweisung verwendet hat. Die SELECT-Anweisung gibt unabhängig von dieser Einstellung dieselben Daten zurück. Der einzige Unterschied besteht darin, wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Tabellen bei der Ausführung der Abfrage verarbeitet.  
  
 Auch Hinweise für den Abfrageoptimierer können in Abfragen verwendet werden, um das Verarbeiten der SELECT-Anweisung durch [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu beeinflussen.  
  
 SET FORCEPLAN wird zur Ausführungszeit und nicht zur Analysezeit angewendet.  
  
## <a name="permissions"></a>Berechtigungen  
 Die SET FORCEPLAN-Berechtigungen erhalten standardmäßig alle Benutzer.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden vier Tabellen verknüpft. Die `SHOWPLAN_TEXT`-Einstellung ist aktiviert. Daher gibt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Informationen dazu zurück, wie die Abfrage anders verarbeitet wird, sobald die `SET FORCE_PLAN`-Einstellung aktiviert wird.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Make sure FORCEPLAN is set to OFF.  
SET SHOWPLAN_TEXT OFF;  
GO  
SET FORCEPLAN OFF;  
GO  
SET SHOWPLAN_TEXT ON;  
GO  
-- Example where the query plan is not forced.  
SELECT p.LastName, p.FirstName, v.Name  
FROM Person.Person AS p  
   INNER JOIN HumanResources.Employee AS e  
   ON e.BusinessEntityID = p.BusinessEntityID  
   INNER JOIN Purchasing.PurchaseOrderHeader AS poh  
   ON e.BusinessEntityID = poh.EmployeeID  
   INNER JOIN Purchasing.Vendor AS v  
   ON poh.VendorID = v.BusinessEntityID;  
GO  
-- SET FORCEPLAN to ON.  
SET SHOWPLAN_TEXT OFF;  
GO  
SET FORCEPLAN ON;  
GO  
SET SHOWPLAN_TEXT ON;  
GO  
-- Reexecute inner join to see the effect of SET FORCEPLAN ON.  
SELECT p.LastName, p.FirstName, v.Name  
FROM Person.Person AS p  
   INNER JOIN HumanResources.Employee AS e   
   ON e.BusinessEntityID = p.BusinessEntityID  
   INNER JOIN Purchasing.PurchaseOrderHeader AS poh  
   ON e.BusinessEntityID = poh.EmployeeID  
   INNER JOIN Purchasing.Vendor AS v  
   ON poh.VendorID = v.BusinessEntityID;  
GO  
SET SHOWPLAN_TEXT OFF;  
GO  
SET FORCEPLAN OFF;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [SET-Anweisungen (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_ALL &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-all-transact-sql.md)   
 [SET SHOWPLAN_TEXT &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-text-transact-sql.md)  
  
  
