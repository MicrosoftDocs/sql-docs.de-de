---
description: ISDATE (Transact-SQL)
title: ISDATE (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ISDATETIME
- ISDATE_TSQL
- ISDATE
- ISDATETIME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], ISDATE
- validate dates times [SQL Server]
- formats [SQL Server], time
- dates [SQL Server], validate
- verify dates times [SQL Server]
- functions [SQL Server], time
- formats [SQL Server], dates
- functions [SQL Server], date and time
- time [SQL Server], functions
- time [SQL Server], validate
- ISDATE function [SQL Server]
ms.assetid: 8e2c9ee7-388a-432f-b2c9-7b398f26bf85
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7d43b4ab5bbd5c029022fe7a73bf7b55848f0464
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350398"
---
# <a name="isdate-transact-sql"></a>ISDATE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt 1 zurück, falls der *Ausdruck* ein gültiger **date**-, **time**- oder **datetime**-Wert ist, andernfalls 0 (null).  
  
 ISDATE gibt 0 (null) zurück, wenn der *Ausdruck* ein **datetime2**-Wert ist.  
  
 Eine Übersicht über alle Datums- und Uhrzeitdatentypen und zugehörige Funktionen für [!INCLUDE[tsql](../../includes/tsql-md.md)] finden Sie unter [Date and Time Data Types and Functions &#40;Transact-SQL&#41; (Datums- und Uhrzeitdatentypen und zugehörige Funktionen)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md). Beachten Sie, dass der Bereich für Datums-/Uhrzeitdaten zwischen 1753-01-01 und 9999-12-31 liegt, während der Bereich für Datumsdaten zwischen 0001-01-01 und 9999-12-31 liegt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
ISDATE ( expression )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *expression*  
 Entspricht einer Zeichenfolge oder einem [Ausdruck](../../t-sql/language-elements/expressions-transact-sql.md), der in eine Zeichenfolge konvertiert werden kann. Der Ausdruck muss weniger als 4000 Zeichen umfassen. Datums- und Uhrzeitdatentypen, mit Ausnahme von datetime und smalldatetime, sind nicht als Argument für ISDATE zugelassen.  
  
## <a name="return-type"></a>Rückgabetyp  
 **int**  
  
## <a name="remarks"></a>Bemerkungen  
 ISDATE ist nur deterministisch bei Verwendung mit der [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md)-Funktion, wenn der style-Parameter von CONVERT angegeben wird und „style“ nicht den Wert 0, 100, 9 oder 109 aufweist.  
  
 Der Rückgabewert von ISDATE hängt von den Einstellungen ab, die durch [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md), [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) und die Option [Konfigurieren der Serverkonfigurationsoption Standardsprache](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) festgelegt werden.  
  
## <a name="isdate-expression-formats"></a>Formate von ISDATE-Ausdrücken  
 Beispiele für gültige Formate, für die ISDATE den Wert 1 zurückgibt, finden Sie im Abschnitt „Unterstützte Formate der Zeichenfolgenliterale für datetime“ in den Themen [datetime](../../t-sql/data-types/datetime-transact-sql.md) und [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md). Zusätzliche Beispiele finden Sie auch in der Spalte Eingabe/Ausgabe des Abschnitts „Argumente“ unter [CAST und CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
 In der folgenden Tabelle werden ungültige Formate für Eingabeausdrücke zusammengefasst, die den Wert 0 oder einen Fehler zurückgeben.  
  
|ISDATE-Ausdruck|ISDATE-Rückgabewert|  
|-----------------------|-------------------------|  
|NULL|0|  
|Werte der in [Datentypen](../../t-sql/data-types/data-types-transact-sql.md) aufgeführten Datentypen in jeder beliebigen Datentypkategorie außer Zeichenfolgen, Unicode-Zeichenfolgen oder Datum und Zeit.|0|  
|Die Werte der Datentypen **text**, **ntext**, or **image**.|0|  
|Beliebiger Wert mit mehr als drei Dezimalstellen (.0000 bis .0000000... n) für die Genauigkeit bei Sekundenangaben. ISDATE gibt 0 zurück, wenn der *Ausdruck* ein **datetime2**-Wert ist, gibt jedoch 1 zurück, wenn der *Ausdruck* ein gültiger **datetime**-Wert ist.|0|  
|Beliebiger Wert, der ein gültiges Datum mit einem ungültigen Wert kombiniert, z. B. 1995-10-1a.|0|  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-isdate-to-test-for-a-valid-datetime-expression"></a>A. Verwenden von ISDATE, um auf einen gültigen datetime-Ausdruck zu testen  
 Im folgenden Beispiel wird gezeigt, wie `ISDATE` verwendet wird, um zu testen, ob eine Zeichenfolge ein gültiger **datetime**-Wert ist.  
  
```sql  
IF ISDATE('2009-05-12 10:19:41.177') = 1  
    PRINT 'VALID'  
ELSE  
    PRINT 'INVALID';  
```  
  
### <a name="b-showing-the-effects-of-the-set-dateformat-and-set-language-settings-on-return-values"></a>B. Anzeigen der Auswirkungen der SET DATEFORMAT-Einstellung und der SET LANGUAGE-Einstellung auf Rückgabewerte  
 Die folgenden Anweisungen zeigen die Werte, die als Ergebnis der Einstellungen von `SET DATEFORMAT` und `SET LANGUAGE` zurückgegeben werden.  
  
```sql  
/* Use these sessions settings. */  
SET LANGUAGE us_english;  
SET DATEFORMAT mdy;  
/* Expression in mdy dateformat */  
SELECT ISDATE('04/15/2008'); --Returns 1.  
/* Expression in mdy dateformat */  
SELECT ISDATE('04-15-2008'); --Returns 1.   
/* Expression in mdy dateformat */  
SELECT ISDATE('04.15.2008'); --Returns 1.   
/* Expression in myd  dateformat */  
SELECT ISDATE('04/2008/15'); --Returns 1.  
  
SET DATEFORMAT mdy;  
SELECT ISDATE('15/04/2008'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('15/2008/04'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('2008/15/04'); --Returns 0.  
SET DATEFORMAT mdy;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
SET DATEFORMAT dmy;  
SELECT ISDATE('15/04/2008'); --Returns 1.  
SET DATEFORMAT dym;  
SELECT ISDATE('15/2008/04'); --Returns 1.  
SET DATEFORMAT ydm;  
SELECT ISDATE('2008/15/04'); --Returns 1.  
SET DATEFORMAT ymd;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
SET LANGUAGE English;  
SELECT ISDATE('15/04/2008'); --Returns 0.  
SET LANGUAGE Hungarian;  
SELECT ISDATE('15/2008/04'); --Returns 0.  
SET LANGUAGE Swedish;  
SELECT ISDATE('2008/15/04'); --Returns 0.  
SET LANGUAGE Italian;  
SELECT ISDATE('2008/04/15'); --Returns 1.  
  
/* Return to these sessions settings. */  
SET LANGUAGE us_english;  
SET DATEFORMAT mdy;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-isdate-to-test-for-a-valid-datetime-expression"></a>C. Verwenden von ISDATE, um auf einen gültigen datetime-Ausdruck zu testen  
 Im folgenden Beispiel wird gezeigt, wie `ISDATE` verwendet wird, um zu testen, ob eine Zeichenfolge ein gültiger **datetime**-Wert ist.  
  
```sql  
IF ISDATE('2009-05-12 10:19:41.177') = 1  
    SELECT 'VALID';  
ELSE  
    SELECT 'INVALID';  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
