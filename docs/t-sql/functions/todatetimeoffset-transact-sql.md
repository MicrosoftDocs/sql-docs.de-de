---
description: TODATETIMEOFFSET (Transact-SQL)
title: TODATETIMEOFFSET (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TO_DATETIMEOFFSET_TSQL
- SWITCH_TZ_TSQL
- SWITCH_TZ
- TO_DATETIMEOFFSET
dev_langs:
- TSQL
helpviewer_keywords:
- date and time [SQL Server], TODATETIMEOFFSET
- TODATETIMEOFFSET function
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: b5fafc08-efd4-4a3b-a0b3-068981a0a685
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 66b3b48adf60f4898a72e3476aa66530728c9a24
ms.sourcegitcommit: 98acedd435aecfda1b3c4c23d3f0c3c1a12682a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102532362"
---
# <a name="todatetimeoffset-transact-sql"></a>TODATETIMEOFFSET (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt einen **datetimeoffset**-Wert zurück, der von einem **datetime2**-Ausdruck übersetzt wird.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
TODATETIMEOFFSET ( datetime_expression , timezoneoffset_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *datetime_expression*  
 Ein [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md), der zu einem [datetime2](../../t-sql/data-types/datetime2-transact-sql.md)-Wert aufgelöst wird.  
  
> [!NOTE]  
>  Ein Ausdruck kann nicht vom Typ **text**, **ntext** oder **image** sein, da diese Typen nicht implizit in **varchar** oder **nvarchar** konvertiert werden können.  
  
 *timezoneoffset_expression*  
 Ein Ausdruck, der den Zeitzonenoffset in Minuten (bei einer ganzen Zahl), z.B. -120, oder in Stunden und Minuten (bei einer Zeichenfolge), z.B. „+13:00“ darstellt. Der Bereich liegt zwischen +14 und -14 (in Stunden). Für die timezoneoffset_expression-Angabe wird der Ausdruck in Ortszeit interpretiert.  

  
> [!NOTE]  
>  Wenn der Ausdruck eine Zeichenfolge ist, muss er folgendes Format aufweisen: {+ | -} TZH:THM.  
  
## <a name="return-type"></a>Rückgabetyp  
 **datetimeoffset**. Die Genauigkeit der Bruchteile entspricht der des *datetime*-Arguments.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-changing-the-time-zone-offset-of-the-current-date-and-time"></a>A. Ändern des Zeitzonenoffsets für das aktuelle Datum und die aktuelle Uhrzeit  
 Im folgenden Beispiel ändert sich der Zeitzonenoffset des aktuellen Datums und der aktuellen Uhrzeit zur Zeitzone `-07:00`.  
  
```sql  
DECLARE @todaysDateTime DATETIME2;  
SET @todaysDateTime = GETDATE();  
SELECT TODATETIMEOFFSET (@todaysDateTime, '-07:00');  
-- RETURNS 2019-04-22 16:23:51.7666667 -07:00  
```  
  
### <a name="b-changing-the-time-zone-offset-in-minutes"></a>B. Ändern des Zeitzonenoffsets zu Minuten  
 Im folgenden Beispiel ändert sich die aktuelle Zeitzone zu `-120` Minuten.  
  
```sql  
SELECT TODATETIMEOFFSET(SYSDATETIME(), -120)
-- RETURNS: 2019-04-22 11:39:21.6986813 -02:00  
```  
  
### <a name="c-adding-a-13-hour-time-zone-offset"></a>C. Hinzufügen eines 13-Stunden-Zeitzonenoffsets  
 Im folgenden Beispiel wird ein 13-Stunden-Zeitzonenoffset einem Datum und einer Uhrzeit hinzugefügt.  
  
```sql  
SELECT TODATETIMEOFFSET(SYSDATETIME(), '+13:00')
-- RETURNS: 2019-04-22 11:39:29.0339301 +13:00
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Date and Time Data Types and Functions &#40;Transact-SQL&#41; (Datums- und Uhrzeitdatentypen und zugehörige Funktionen &#40;Transact-SQL&#41;)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)   
 [AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)  
  
  

