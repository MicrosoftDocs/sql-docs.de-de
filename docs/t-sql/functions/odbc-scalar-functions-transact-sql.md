---
description: ODBC-Skalarfunktionen (Transact-SQL)
title: ODBC-Skalarfunktionen (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- CURDATE ODBC function
- DAYOFMONTH ODBC function
- WEEK ODBC function
- functions, ODBC CONCAT
- SECOND ODBC function
- functions, ODBC CURRENT_TIME
- functions, ODBC HOUR
- functions, ODBC QUARTER
- TRUNCATE ODBC function
- functions, ODBC SECOND
- DAYOFWEEK ODBC function
- CURRENT_DATE ODBC function
- DAYNAME ODBC function
- CURTIME ODBC function
- functions, ODBC CURRENT_DATE
- MINUTE ODBC function
- functions, ODBC BIT_LENGTH
- functions, ODBC OCTET_LENGTH
- CONCAT ODBC function
- MONTHNAME ODBC function
- functions, ODBC DAYOFYEAR
- OCTET_LENGTH ODBC function
- functions [SQL Server], ODBC scalar functions
- BIT_LENGTH ODBC function
- functions, ODBC DAYOFMONTH
- CURRENT_TIME ODBC function
- functions, ODBC CURDATE
- functions, ODBC CURTIME
- functions, ODBC TRUNCATE
- functions, ODBC MONTHNAME
- functions, ODBC DAYNAME
- ODBC scalar functions
- functions, ODBC MINUTE
- functions, ODBC DAYOFWEEK
- QUARTER ODBC function
- DAYOFYEAR ODBC function
- functions, ODBC WEEK
- HOUR ODBC function
ms.assetid: a0df1ac2-6699-4ac0-8f79-f362f23496f1
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fb9cc0e305334fa15bdbdb8b61eafc82f8360c3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340663"
---
# <a name="odbc-scalar-functions-transact-sql"></a>ODBC-Skalarfunktionen (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Sie können [ODBC-Skalarfunktionen](../../odbc/reference/appendixes/appendix-e-scalar-functions.md) in [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen verwenden. Diese Anweisungen werden von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interpretiert. Sie können in gespeicherten Prozeduren und benutzerdefinierten Funktionen verwendet werden. Hierzu zählen Zeichenfolgen-, Uhrzeit-, Datums-, Intervall- und Systemfunktionen sowie numerische Funktionen.  
  
## <a name="usage"></a>Verwendung  
 ```syntaxsql
 SELECT {fn <function_name> [ (<argument>,....n) ] }
 ```
  
## <a name="functions"></a>Functions  
 In den folgenden Tabellen werden ODBC-Skalarfunktionen aufgelistet, die nicht in [!INCLUDE[tsql](../../includes/tsql-md.md)] dupliziert werden.  
  
### <a name="string-functions"></a>String-Funktionen  
  
|Funktion|BESCHREIBUNG|  
|--------------|-----------------|  
|BIT_LENGTH( string_exp ) (ODBC 3.0)|Gibt die Länge des Zeichenfolgenausdrucks in Bits zurück.<br /><br /> Gibt die interne Größe des angegebenen Datentyps zurück, ohne „string_exp“ in „string“ zu konvertieren.|  
|CONCAT( string_exp1,string_exp2) (ODBC 1.0)|Gibt eine Zeichenfolge zurück, die das Ergebnis der Verkettung von string_exp2 und string_exp1 darstellt. Die Ergebniszeichenfolge hängt vom DBMS ab. Wenn die durch string_exp1 dargestellte Spalte z. B. einen NULL-Wert enthält, wird DB2 NULL zurückgeben. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hingegen gibt eine Zeichenfolge ungleich NULL zurückgeben.|  
|OCTET_LENGTH( string_exp ) (ODBC 3.0)|Gibt die Länge des Zeichenfolgenausdrucks in Bytes zurück. Das Ergebnis ist die kleinste ganze Zahl, die nicht kleiner ist als die Anzahl der Bits dividiert durch 8.<br /><br /> Gibt die interne Größe des angegebenen Datentyps zurück, ohne „string_exp“ in „string“ zu konvertieren.|  
  
### <a name="numeric-function"></a>Numerische Funktion  
  
|Funktion|BESCHREIBUNG|  
|--------------|-----------------|  
|TRUNCATE( numeric_exp, integer_exp) (ODBC 2.0)|Gibt numeric_exp abgeschnitten auf integer_exp-Positionen rechts vom Dezimaltrennzeichen zurück. Wenn „integer_exp“ negativ ist, wird „numeric_exp“ auf die Positionen „&#124;integer_exp&#124;“ links vom Dezimaltrennzeichen abgeschnitten.|  
  
### <a name="time-date-and-interval-functions"></a>Uhrzeit-, Datums- und Intervallfunktionen  
  
|Funktion|BESCHREIBUNG|  
|--------------|-----------------|  
|CURRENT_DATE( ) (ODBC 3.0)|Gibt das aktuelle Datum zurück.|  
|CURDATE( ) (ODBC 3.0)|Gibt das aktuelle Datum zurück.|  
|CURRENT_TIME`[( time-precision )]` (ODBC 3.0)|Gibt die aktuelle lokale Zeit zurück. Das time-precision-Argument bestimmt die Genauigkeit des zurückgegebenen Werts bezüglich der Sekundenangaben.|  
|CURTIME() (ODBC 3.0)|Gibt die aktuelle lokale Zeit zurück.|  
|DAYNAME( date_exp ) (ODBC 2.0)|Gibt eine Zeichenfolge zurück, die den für die Datenquelle spezifischen Namen des Tages aus „date_exp“ enthält. Der Name entspricht beispielsweise „Sunday through Saturday“ oder „Sun. bis Sa. für eine Datenquelle, die Englisch als Sprache verwendet. Für eine Datenquelle, die Deutsch als Sprache verwendet, entspricht der Name beispielsweise „Sonntag through Samstag“.|
|DAYOFMONTH( date_exp ) (ODBC 1.0)|Gibt den Tag des Monats basierend auf dem Monatsfeld in „date_exp“ als ganze Zahl zurück. Der Rückgabewert liegt im Bereich zwischen 1 und 31.|  
|DAYOFWEEK( date_exp ) (ODBC 1.0)|Gibt den Tag der Woche basierend auf dem Wochenfeld in „date_exp“ als ganze Zahl zurück. Der Rückgabewert liegt im Bereich zwischen 1 und 7, wobei 1 für den Sonntag steht.|  
|HOUR( time_exp ) (ODBC 1.0)|Gibt die Stunde basierend auf dem Stundenfeld in „time_exp“ als ganze Zahl im Bereich zwischen 0 und 23 zurück.|  
|MINUTE( time_exp ) (ODBC 1.0)|Gibt die Minute basierend auf dem Minutenfeld in „time_exp“ als ganze Zahl im Bereich zwischen 0 und 59 zurück.|  
|SECOND( time_exp ) (ODBC 1.0)|Gibt die Sekunde basierend auf dem Sekundenfeld in „time_exp“ als ganze Zahl im Bereich zwischen 0 und 59 zurück.|  
|MONTHNAME( date_exp ) (ODBC 2.0)|Gibt eine Zeichenfolge zurück, die den für die Datenquelle spezifischen Namen des Monats aus „date_exp“ enthält. Der Name entspricht beispielsweise „January through December“ oder „Jan. through Dec.“ für eine Datenquelle, die Englisch als Sprache verwendet. Für eine Datenquelle, die Deutsch als Sprache verwendet, entspricht der Name beispielsweise „Januar through Dezember“.|  
|QUARTER( date_exp ) (ODBC 1.0)|Gibt das Quartal in date_exp als ganze Zahl im Bereich von 1 bis 4 zurück, wobei 1 den Zeitraum vom 1. Januar bis 31. März darstellt.|  
|WEEK( date_exp ) (ODBC 1.0)|Gibt die Kalenderwoche des Jahres basierend auf dem Wochenfeld in „date_exp“ als ganze Zahl im Bereich zwischen 1 und 53 zurück.|  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-using-an-odbc-function-in-a-stored-procedure"></a>A. Verwenden einer ODBC-Funktion in einer gespeicherten Prozedur  
 Im folgenden Beispiel wird eine ODBC-Funktion in einer gespeicherten Prozedur verwendet:  
  

```sql 
CREATE PROCEDURE dbo.ODBCprocedure  
(  
    @string_exp NVARCHAR(4000)  
)  
AS  
SELECT {fn OCTET_LENGTH( @string_exp )};  
```  
  
### <a name="b-using-an-odbc-function-in-a-user-defined-function"></a>B. Verwenden einer ODBC-Funktion in einer benutzerdefinierten Funktion  
 Im folgenden Beispiel wird eine ODBC-Funktion in einer benutzerdefinierten Funktion verwendet:  
  
```sql  
CREATE FUNCTION dbo.ODBCudf  
(  
    @string_exp NVARCHAR(4000)  
)  
RETURNS INT  
AS  
BEGIN  
DECLARE @len INT  
SET @len = (SELECT {fn OCTET_LENGTH( @string_exp )})  
RETURN(@len)  
END ;  
  
SELECT dbo.ODBCudf('Returns the length.');  
--Returns 38  
```  
  
### <a name="c-using-an-odbc-functions-in-select-statements"></a>C. Verwenden von ODBC-Funktionen in SELECT-Anweisungen  
 In den folgenden SELECT-Anweisungen werden ODBC-Funktionen verwendet:  
  
```sql 
DECLARE @string_exp NVARCHAR(4000) = 'Returns the length.';  
SELECT {fn BIT_LENGTH( @string_exp )};  
-- Returns 304  
SELECT {fn OCTET_LENGTH( @string_exp )};  
-- Returns 38  
  
SELECT {fn CONCAT( 'CONCAT ','returns a character string')};  
-- Returns CONCAT returns a character string  
SELECT {fn TRUNCATE( 100.123456, 4)};  
-- Returns 100.123400  
SELECT {fn CURRENT_DATE( )};  
-- Returns 2007-04-20  
SELECT {fn CURRENT_TIME(6)};  
-- Returns 10:27:11.973000  
  
DECLARE @date_exp NVARCHAR(30) = '2007-04-21 01:01:01.1234567';  
SELECT {fn DAYNAME( @date_exp )};  
-- Returns Saturday  
SELECT {fn DAYOFMONTH( @date_exp )};  
-- Returns 21  
SELECT {fn DAYOFWEEK( @date_exp )};  
-- Returns 7  
SELECT {fn HOUR( @date_exp)};  
-- Returns 1   
SELECT {fn MINUTE( @date_exp )};  
-- Returns 1  
SELECT {fn SECOND( @date_exp )};  
-- Returns 1  
SELECT {fn MONTHNAME( @date_exp )};  
-- Returns April  
SELECT {fn QUARTER( @date_exp )};  
-- Returns 2  
SELECT {fn WEEK( @date_exp )};  
-- Returns 16  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-an-odbc-function-in-a-stored-procedure"></a>D: Verwenden einer ODBC-Funktion in einer gespeicherten Prozedur  
 Im folgenden Beispiel wird eine ODBC-Funktion in einer gespeicherten Prozedur verwendet:  
  
```sql  
CREATE PROCEDURE dbo.ODBCprocedure  
(  
    @string_exp NVARCHAR(4000)  
)  
AS  
SELECT {fn BIT_LENGTH( @string_exp )};  
```  
  
### <a name="e-using-an-odbc-function-in-a-user-defined-function"></a>E. Verwenden einer ODBC-Funktion in einer benutzerdefinierten Funktion  
 Im folgenden Beispiel wird eine ODBC-Funktion in einer benutzerdefinierten Funktion verwendet:  
  
```sql  
CREATE FUNCTION dbo.ODBCudf  
(  
    @string_exp NVARCHAR(4000)  
)  
RETURNS INT  
AS  
BEGIN  
DECLARE @len INT  
SET @len = (SELECT {fn BIT_LENGTH( @string_exp )})  
RETURN(@len)  
END ;  
  
SELECT dbo.ODBCudf('Returns the length in bits.');  
--Returns 432  
```  
  
### <a name="f-using-an-odbc-functions-in-select-statements"></a>F. Verwenden von ODBC-Funktionen in SELECT-Anweisungen  
 In den folgenden SELECT-Anweisungen werden ODBC-Funktionen verwendet:  
  
```sql  
DECLARE @string_exp NVARCHAR(4000) = 'Returns the length.';  
SELECT {fn BIT_LENGTH( @string_exp )};  
-- Returns 304  
  
SELECT {fn CONCAT( 'CONCAT ','returns a character string')};  
-- Returns CONCAT returns a character string  
SELECT {fn CURRENT_DATE( )};  
-- Returns today's date  
SELECT {fn CURRENT_TIME(6)};  
-- Returns the time  
  
DECLARE @date_exp NVARCHAR(30) = '2007-04-21 01:01:01.1234567';  
SELECT {fn DAYNAME( @date_exp )};  
-- Returns Saturday  
SELECT {fn DAYOFMONTH( @date_exp )};  
-- Returns 21  
SELECT {fn DAYOFWEEK( @date_exp )};  
-- Returns 7  
SELECT {fn HOUR( @date_exp)};  
-- Returns 1   
SELECT {fn MINUTE( @date_exp )};  
-- Returns 1  
SELECT {fn SECOND( @date_exp )};  
-- Returns 1  
SELECT {fn MONTHNAME( @date_exp )};  
-- Returns April  
SELECT {fn QUARTER( @date_exp )};  
-- Returns 2  
SELECT {fn WEEK( @date_exp )};  
-- Returns 16  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integrierte Funktionen &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)