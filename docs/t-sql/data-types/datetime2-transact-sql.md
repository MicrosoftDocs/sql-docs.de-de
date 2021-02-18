---
description: datetime2 (Transact-SQL)
title: datetime2 (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- datetime2
- datetime2_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- dates [SQL Server], data types
- date and time [SQL Server], datetime2
- data types [SQL Server], date and time
- datetime2 data type [SQL Server]
ms.assetid: 868017f3-214f-43ef-8536-cc1632a2288f
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0bdacc09933666e1deb2d238f867441a8cbc8f6c
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525186"
---
# <a name="datetime2-transact-sql"></a>datetime2 (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Definiert ein Datum, das mit einer Uhrzeit kombiniert ist und auf dem 24-Stunden-Format basiert. **datetime2** kann als eine Erweiterung des bestehenden **datetime**-Typs angesehen werden, der einen größeren Datumsbereich, eine größere Standardgenauigkeit bei Sekundenbruchteilen und eine optionale vom Benutzer angegebene Genauigkeit besitzt.
  
## <a name="datetime2-description"></a>datetime2-Beschreibung
  
|Eigenschaft|Wert|  
|--------------|-----------|  
|Syntax|**datetime2** [ (*Genauigkeit in Sekundenbruchteilen*) ]|  
|Verwendung|DECLARE \@MyDatetime2 **datetime2(7)**<br /><br /> CREATE TABLE Table1 ( Column1 **datetime2(7)** )|  
|Standardmäßiges Format der Zeichenfolgenliterale<br /><br /> (wird zum Zweck der Clientkompatibilität verwendet)|YYYY-MM-DD hh:mm:ss [.Sekundenbruchteile]<br /><br /> Weitere Informationen finden Sie im nachfolgenden Abschnitt „Abwärtskompatibilität für Downlevelclients“.|  
|Datumsbereich|0001-01-01 bis 9999-12-31<br /><br /> 1. Januar 1 n. Chr. bis 31. Dezember 9999 n. Chr.|  
|Uhrzeitbereich|00:00:00 bis 23:59:590,9999999|  
|Zeitzonenoffsetbereich|Keine|  
|Elementbereiche|Bei YYYY handelt es sich um eine vierstellige Zahl im Bereich von 0001 bis 9999, die ein Jahr darstellt.<br /><br /> Bei MM handelt es sich um eine zweistellige Zahl im Bereich von 01 bis 12, die im angegebenen Jahr einen Monat darstellt.<br /><br /> Bei DD handelt es sich um eine zweistellige Zahl im Bereich von 01 bis 31, die im angegebenen Monat einen Tag darstellt.<br /><br /> Bei hh handelt es sich um eine zweistellige Zahl im Bereich von 00 bis 23, die die Stunde darstellt.<br /><br /> Bei mm handelt es sich um eine zweistellige Zahl im Bereich von 00 bis 59, die die Minute darstellt.<br /><br /> Bei ss handelt es sich um eine zweistellige Zahl im Bereich von 00 bis 59, die die Sekunde darstellt.<br /><br /> Bei n* handelt es sich um eine null- bis siebenstellige Zahl von 0 bis 9999999, die die Sekundenbruchteile darstellt. In Informatica werden die Sekundenbruchteile abgeschnitten, wenn n > 3.|  
|Zeichenlänge|Mindestens 19 Positionen (YYYY-MM-DD hh:mm:ss) bis maximal 27 Positionen (YYYY-MM-DD hh:mm:ss .0000000)|  
|Genauigkeit, Dezimalstellen|0 bis 7 Stellen mit einer Genauigkeit von 100 ns. Die Standardgenauigkeit beträgt 7 Stellen.|  
|Speichergröße <sup>1</sup>|6 Byte für Genauigkeiten unter 3.<br/>7 Byte für Genauigkeiten von 3 und 4.<br/>Alle anderen Genauigkeiten erfordern 8 Byte.<sup>2</sup>|  
|Genauigkeit|100 Nanosekunden|  
|Standardwert|1900-01-01 00:00:00|  
|Kalender|Gregorianisch|  
|Benutzerdefinierte Genauigkeit in Sekundenbruchteilen|Ja|  
|Beachtung und Beibehaltung des Zeitzonenoffsets|Nein|  
|Beachtung der Sommerzeit|Nein|  

<sup>1</sup> Bereitgestellte Werte gelten für den nicht komprimierten Rowstore-Index. Bei der Verwendung der [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md) oder von [Columnstore-Indizes](../../relational-databases/indexes/columnstore-indexes-overview.md) kann sich die Speichergröße der Genauigkeiten ändern. Außerdem kann sich die Speichergröße auf dem Datenträger von der im Arbeitsspeicher unterscheiden. So benötigen **datetime2**-Werte immer 8 Byte Arbeitsspeicher, wenn der Batchmodus verwendet wird.

<sup>2</sup> Wenn ein **datetime2**-Wert in einen **varbinary**-Wert umgewandelt wird, wird dem **varbinary**-Wert ein zusätzliches Byte hinzugefügt, um die Genauigkeit zu speichern.

Metadaten von Datentypen finden Sie unter [sys.systypes &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-systypes-transact-sql.md) oder [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md). Für einige Datums- und Uhrzeitdatentypen sind die Genauigkeit und die Dezimalstellenanzahl variabel. Informationen zum Abrufen der Präzision und Skalierung für eine Spalte finden Sie unter [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md), [COL_LENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/col-length-transact-sql.md) oder [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md).
  
## <a name="supported-string-literal-formats-for-datetime2"></a>Unterstützte Formate der Zeichenfolgenliterale für datetime2
In den folgenden Tabellen werden die unterstützten ISO 8601- und ODBC-Formate für Zeichenfolgenliterale für **datetime2** aufgelistet. Informationen zu alphabetischen, numerischen und unstrukturierten Formaten sowie zu Zeitformaten für die Datums- und Uhrzeitteile von **datetime2** finden Sie unter [date &#40;Transact-SQL&#41;](../../t-sql/data-types/date-transact-sql.md) und [time &#40;Transact-SQL&#41;](../../t-sql/data-types/time-transact-sql.md).
  
|ISO 8601|Beschreibungen|  
|---|---|
|YYYY-MM-DDThh:mm:ss[.nnnnnnn]<br /><br /> YYYY-MM-DDThh:mm:ss[.nnnnnnn]|Dieses Format wird nicht von den SET LANGUAGE- und SET DATEFORMAT-Gebietsschemaeinstellungen für Sitzungen beeinflusst. Das **T**, die Doppelpunkte (:) und der Punkt (.) werden in das Zeichenfolgenliteral eingeschlossen, z.B. '2007-05-02T19:58:47.1234567'.|  
  
|ODBC|BESCHREIBUNG|  
|---|---|
|{ ts 'yyyy-mm-dd hh:mm:ss[.Sekundenbruchteile]' }|ODBC-API-spezifisch:<br /><br /> Für die Anzahl der Stellen rechts vom Dezimaltrennzeichen, die den Sekundenbruchteil darstellen, kann 0 bis 7 (100 Nanosekunden) angegeben werden.|  
  
## <a name="ansi-and-iso-8601-compliance"></a>Kompatibilität mit ANSI und ISO 8601  
Die Kompatibilität mit ANSI und ISO 8601 von [date](../../t-sql/data-types/date-transact-sql.md) und [time](../../t-sql/data-types/time-transact-sql.md) gilt auch für **datetime2**.
  
##  <a name="backward-compatibility-for-down-level-clients"></a>Abwärtskompatibilität für Downlevelclients  
Einige Downlevelclients unterstützen nicht die Datentypen **time**, **date**, **datetime2** und **datetimeoffset**. In der folgenden Tabelle wird die Typzuordnung zwischen einer Instanz höherer Ebene in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und Downlevelclients gezeigt.
  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datentyp|Standardmäßiges Format des an Downlevelclients übergebenen Zeichenfolgenliterals|ODBC früherer Versionen|OLEDB früherer Versionen|JDBC früherer Versionen|SQLCLIENT früherer Versionen|  
| --- | --- | --- | --- | --- | --- |
|**time**|hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR oder SQL_VARCHAR|DBTYPE_WSTR oder DBTYPE_STR|Java.sql.String|Zeichenfolge oder SqString|  
|**date**|JJJJ-MM-TT|SQL_WVARCHAR oder SQL_VARCHAR|DBTYPE_WSTR oder DBTYPE_STR|Java.sql.String|Zeichenfolge oder SqString|  
|**datetime2**|YYYY-MM-DD hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR oder SQL_VARCHAR|DBTYPE_WSTR oder DBTYPE_STR|Java.sql.String|Zeichenfolge oder SqString|  
|**datetimeoffset**|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR oder SQL_VARCHAR|DBTYPE_WSTR oder DBTYPE_STR|Java.sql.String|Zeichenfolge oder SqString|  
  
## <a name="converting-date-and-time-data"></a>Konvertieren von Datums- und Uhrzeitdaten
Beim Konvertieren in date- und time-Datentypen lehnt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alle Werte ab, die nicht als Datum oder Uhrzeit erkannt werden. Informationen zur Verwendung der CAST-Funktion und der CONVERT-Funktion mit Datums- und Uhrzeitdaten finden Sie unter [CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).
  
### <a name="converting-other-date-and-time-types-to-the-datetime2-data-type"></a>Konvertieren anderer Datums- und Uhrzeittypen in datetime2-Datentyp
Der folgende Abschnitt veranschaulicht die Abläufe bei der Konvertierung von anderen Datums- und Uhrzeitdatentypen in den Datentyp **datetime2**.  
  
Beim Konvertieren in **date** werden das Jahr, der Monat und der Tag kopiert.  Die Zeitkomponente wird auf 00:00:00.0000000 festgelegt.  Der folgende Code zeigt die Ergebnisse der Konvertierung eines `date`-Werts in einen `datetime2`-Wert.  
  
```sql
DECLARE @date date = '12-21-16';
DECLARE @datetime2 datetime2 = @date;

SELECT @datetime2 AS '@datetime2', @date AS '@date';
  
--Result  
--@datetime2                  @date
----------------------------- ----------
--2016-12-21 00:00:00.0000000 2016-12-21
```  
  
Beim Konvertieren von **time(n)** wird die Zeitkomponente kopiert, und die Datumskomponente wird auf „1900-01-01“ festgelegt. Das folgende Beispiel zeigt die Ergebnisse der Konvertierung eines `time(7)`-Werts in einen `datetime2`-Wert.  
  
```sql
DECLARE @time time(7) = '12:10:16.1234567';
DECLARE @datetime2 datetime2 = @time;

SELECT @datetime2 AS '@datetime2', @time AS '@time';
  
--Result  
--@datetime2                  @time
----------------------------- ----------------
--1900-01-01 12:10:16.1234567 12:10:16.1234567
```  
  
Wenn die Konvertierung von **samlldatetime** erfolgt, werden die Stunden und Minuten kopiert. Die Sekunden und die Sekundenbruchteile werden auf 0 festgelegt. Der folgende Code zeigt die Ergebnisse der Konvertierung eines `smalldatetime`-Werts in einen `datetime2`-Wert.  
  
```sql
DECLARE @smalldatetime smalldatetime = '12-01-16 12:32';
DECLARE @datetime2 datetime2 = @smalldatetime;

SELECT @datetime2 AS '@datetime2', @smalldatetime AS '@smalldatetime'; 
  
--Result  
--@datetime2                  @smalldatetime
----------------------------- -----------------------
--2016-12-01 12:32:00.0000000 2016-12-01 12:32:00 
```  
  
Wenn die Konvertierung von **datetimeoffset(n)** erfolgt, werden die Datums- und Uhrzeitkomponenten kopiert. Die Zeitzone wird abgeschnitten. Das folgende Beispiel zeigt die Ergebnisse der Konvertierung eines `datetimeoffset(7)`-Werts in einen `datetime2`-Wert.  
  
```sql
DECLARE @datetimeoffset datetimeoffset(7) = '2016-10-23 12:45:37.1234567 +10:0';
DECLARE @datetime2 datetime2 = @datetimeoffset;

SELECT @datetime2 AS '@datetime2', @datetimeoffset AS '@datetimeoffset'; 
  
--Result  
--@datetime2                  @datetimeoffset
----------------------------- ----------------------------------
--2016-10-23 12:45:37.1234567 2016-10-23 12:45:37.1234567 +10:00
```  

Wenn die Konvertierung von **datetime** erfolgt, werden Datum und Uhrzeit kopiert. Die Genauigkeit der Sekundenbruchteile wird auf 7 Dezimalstellen erweitert. Das folgende Beispiel zeigt die Ergebnisse der Konvertierung eines `datetime`-Werts in einen `datetime2`-Wert.

```sql
DECLARE @datetime datetime = '2016-10-23 12:45:37.333';
DECLARE @datetime2 datetime2 = @datetime;

SELECT @datetime2 AS '@datetime2', @datetime AS '@datetime';
   
--Result  
--@datetime2                  @datetime
------------------------- ---------------------------
--2016-10-23 12:45:37.3333333 2016-10-23 12:45:37.333
```  

> [!NOTE]
> Unter dem Datenbankkompatibilitätsgrad 130 ergibt sich bei einer impliziten Konvertierung aus dem Datentyp „datetime“ in den Datentyp „datetime2“ eine verbesserte Genauigkeit, indem die Bruchteile von Millisekunden berücksichtigt werden, wodurch sich unterschiedliche konvertierte Werte ergeben, wie es im Beispiel oben zu sehen ist. Verwenden Sie explizite Umwandlung in den Datentyp „datetime2“, wenn ein Vergleich so gestaltet ist, dass zwischen den Datentypen „datetime“ und „datetime2“ verglichen wird. Weitere Informationen finden Sie im folgenden [Microsoft-Support-Artikel](https://support.microsoft.com/help/4010261).

### <a name="converting-string-literals-to-datetime2"></a>Konvertieren von Zeichenfolgenliteralen in datetime2  
Konvertierungen von Zeichenfolgenliteralen in Datums- und Zeitwerte sind erlaubt, wenn alle Teile der Zeichenfolge in gültigen Formaten vorliegen. Andernfalls wird ein Laufzeitfehler ausgelöst. Wird bei impliziten oder expliziten Konvertierungen von Datums- und Zeitwerten in Zeichenfolgenliterale kein Stil angegeben, wird das Standardformat der aktuellen Sitzung verwendet. In der folgenden Tabelle werden die Regeln zum Konvertieren eines Zeichenfolgenliterals in den **datetime2**-Datentyp dargestellt.
  
|Eingabezeichenfolgenliteral|**datetime2(n)**|  
|---|---|
|ODBC DATE|Dem **datetime**-Datentyp werden ODBC-Zeichenfolgenliterale zugeordnet. Jede Zuweisungsoperation von ODBC DATETIME-Literalen zu **datetime2**-Typen bewirkt eine in den Konvertierungsregeln definierte implizite Konvertierung zwischen **datetime** und diesen Typen.|  
|ODBC TIME|Siehe vorherige ODBC DATE-Regel.|  
|ODBC DATETIME|Siehe vorherige ODBC DATE-Regel.|  
|Nur DATE|Der TIME-Teil wird standardmäßig auf 00:00:00 festgelegt.|  
|Nur TIME|Der DATE-Teil wird standardmäßig auf 1900-1-1 festgelegt.|  
|Nur TIMEZONE|Standardwerte werden festgelegt.|  
|DATE + TIME|Trivial|  
|DATE + TIMEZONE|Nicht zulässig.|  
|TIME + TIMEZONE|Der DATE-Teil wird standardmäßig auf 1900-1-1 festgelegt. TIMEZONE-Eingabe wird ignoriert.|  
|DATE + TIME + TIMEZONE|Die lokale DATETIME wird verwendet.|  
  
## <a name="examples"></a>Beispiele  
Im folgenden Beispiel werden die Ergebnisse der Umwandlung von einer Zeichenfolge in alle **date**- und **time**-Datentypen verglichen.
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29. 1234567 +12:15' AS time(7)) AS 'time'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS date) AS 'date'   
    ,CAST('2007-05-08 12:35:29.123' AS smalldatetime) AS   
        'smalldatetime'   
    ,CAST('2007-05-08 12:35:29.123' AS datetime) AS 'datetime'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS datetime2(7)) AS   
        'datetime2'  
    ,CAST('2007-05-08 12:35:29.1234567 +12:15' AS datetimeoffset(7)) AS   
        'datetimeoffset';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|Datentyp|Output|  
|---|---|
|**time**|12:35:29.1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29.1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>Weitere Informationen
[CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
