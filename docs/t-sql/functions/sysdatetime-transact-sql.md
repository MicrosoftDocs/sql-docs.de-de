---
description: SYSDATETIME (Transact-SQL)
title: SYSDATETIME (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SYSDATETIME_TSQL
- SYSDATETIME
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], SYSDATETIME
- current date and time [SQL Server]
- functions [SQL Server], time
- system date and time [SQL Server]
- system time [SQL Server]
- SYSDATETIME function [SQL Server]
- functions [SQL Server], date and time
- time [SQL Server], functions
- dates [SQL Server], current date and time
- dates [SQL Server], system date and time
- time [SQL Server], system
ms.assetid: cba4999e-a9d4-4742-abc9-4a4f109206b6
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 95a36c889059488d20e5cfdf3f1c954d6c403d87
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88459626"
---
# <a name="sysdatetime-transact-sql"></a>SYSDATETIME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt einen **datetime2(7)**-Wert zurück, der das Datum und die Uhrzeit des Computers enthält, auf dem die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird.  
  
> [!NOTE]  
>  SYSDATETIME und SYSUTCDATETIME weisen eine höhere Genauigkeit bezüglich der Bruchteile von Sekunden auf als GETDATE und GETUTCDATE. SYSDATETIMEOFFSET schließt den Zeitzonenoffset des Systems ein. SYSDATETIME, SYSUTCDATETIME und SYSDATETIMEOFFSET können einer Variablen eines beliebigen Datums- und Zeittyps zugewiesen werden.  
  
Azure SQL-Datenbank (außer Azure SQL Managed Instance) und Azure Synapse Analytics verwenden UTC. Verwenden Sie [AT TIME ZONE](../../t-sql/queries/at-time-zone-transact-sql.md) in Azure SQL-Datenbank oder Azure Synapse Analytics, wenn Sie Datums- und Uhrzeitinformationen in einer anderen Zeitzone als UTC interpretieren müssen.

 Eine Übersicht über alle Datums- und Uhrzeitdatentypen und zugehörige Funktionen für [!INCLUDE[tsql](../../includes/tsql-md.md)] finden Sie unter [Date and Time Data Types and Functions &#40;Transact-SQL&#41; (Datums- und Uhrzeitdatentypen und zugehörige Funktionen)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
SYSDATETIME ( )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>Rückgabetyp  
 **datetime2(7)**  
  
## <a name="remarks"></a>Bemerkungen  
 [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen können auf eine beliebige Position von SYSDATETIME verweisen, die auf einen **datetime2(7)**-Ausdruck verweisen kann.  
  
 SYSDATETIME ist eine nicht deterministische Funktion. Sichten und Ausdrücke, die in einer Spalte auf diese Funktion verweisen, können nicht indiziert werden.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ruft die Datums- und Zeitwerte mithilfe der GetSystemTimeAsFileTime()-Windows-API ab. Die Genauigkeit hängt von der Computerhardware und der Windows-Version ab, unter der die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird. Die Genauigkeit dieser API liegt bei 100 Nanosekunden. Die Genauigkeit kann mithilfe der GetSystemTimeAdjustment()-Windows-API festgestellt werden.  
  
## <a name="examples"></a>Beispiele  
 In den folgenden Beispielen werden die sechs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Systemfunktionen verwendet, die das aktuelle Datum und die aktuelle Uhrzeit wiedergeben, um das Datum, die Uhrzeit oder beides zurückzugeben. Die Werte werden der Reihe nach zurückgegeben und können sich daher in Bruchteilen von Sekunden unterscheiden.  
  
### <a name="a-getting-the-current-system-date-and-time"></a>A. Abrufen des aktuellen Systemdatums und der aktuellen Systemzeit  
  
```  
SELECT SYSDATETIME()  
    ,SYSDATETIMEOFFSET()  
    ,SYSUTCDATETIME()  
    ,CURRENT_TIMESTAMP  
    ,GETDATE()  
    ,GETUTCDATE();  
/* Returned:  
SYSDATETIME()      2007-04-30 13:10:02.0474381  
SYSDATETIMEOFFSET()2007-04-30 13:10:02.0474381 -07:00  
SYSUTCDATETIME()   2007-04-30 20:10:02.0474381  
CURRENT_TIMESTAMP  2007-04-30 13:10:02.047  
GETDATE()          2007-04-30 13:10:02.047  
GETUTCDATE()       2007-04-30 20:10:02.047  
*/
```    
  
### <a name="b-getting-the-current-system-date"></a>B. Abrufen des aktuellen Systemdatums  
  
```  
SELECT CONVERT (date, SYSDATETIME())  
    ,CONVERT (date, SYSDATETIMEOFFSET())  
    ,CONVERT (date, SYSUTCDATETIME())  
    ,CONVERT (date, CURRENT_TIMESTAMP)  
    ,CONVERT (date, GETDATE())  
    ,CONVERT (date, GETUTCDATE());  
  
/* All returned 2007-04-30 */  
```  
  
### <a name="c-getting-the-current-system-time"></a>C. Abrufen der aktuellen Systemzeit  
  
```  
SELECT CONVERT (time, SYSDATETIME())  
    ,CONVERT (time, SYSDATETIMEOFFSET())  
    ,CONVERT (time, SYSUTCDATETIME())  
    ,CONVERT (time, CURRENT_TIMESTAMP)  
    ,CONVERT (time, GETDATE())  
    ,CONVERT (time, GETUTCDATE());  
  
/* Returned  
SYSDATETIME()      13:18:45.3490361  
SYSDATETIMEOFFSET()13:18:45.3490361  
SYSUTCDATETIME()   20:18:45.3490361  
CURRENT_TIMESTAMP  13:18:45.3470000  
GETDATE()          13:18:45.3470000  
GETUTCDATE()       20:18:45.3470000  
*/  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-getting-the-current-system-date-and-time"></a>D: Abrufen des aktuellen Systemdatums und der aktuellen Systemzeit  
  
```  
SELECT SYSDATETIME();  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
--------------------------  
7/20/2013 2:49:59 PM
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Date and Time Data Types and Functions &#40;Transact-SQL&#41; (Datums- und Uhrzeitdatentypen und zugehörige Funktionen (Transact-SQL))](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)  
  
  

