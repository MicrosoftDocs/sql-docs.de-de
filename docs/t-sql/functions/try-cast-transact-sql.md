---
description: TRY_CAST (Transact-SQL)
title: TRY_CAST (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- TRY_CAST_TSQL
- TRY_CAST
dev_langs:
- TSQL
helpviewer_keywords:
- TRY_CAST function
ms.assetid: ea3a16de-995b-415c-b5f0-9355cf7bb401
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||>= sql-server-2016 ||>= sql-server-linux-2017||= sqlallproducts-allversions||>= aps-pdw-2016||= azure-sqldw-latest
ms.openlocfilehash: 02ec3dd7e7047411901dcaad4b76056781a9384c
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "91379515"
---
# <a name="try_cast-transact-sql"></a>TRY_CAST (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt einen in den angegebenen Datentyp umgewandelten Wert zurück, wenn die Umwandlung erfolgreich ist. Andernfalls wird NULL zurückgegeben.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
TRY_CAST ( expression AS data_type [ ( length ) ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *expression*  
 Der umzuwandelnde Wert. Beliebiger gültiger Ausdruck.  
  
 *data_type*  
 Der Datentyp, in den *expression* umgewandelt werden soll.  
  
 *length*  
 Eine optionale ganze Zahl, die die Länge des Zieldatentyps angibt.  
  
 Der Bereich zulässiger Werte wird durch den Wert von *data_type* bestimmt.  
  
## <a name="return-types"></a>Rückgabetypen  
 Gibt einen in den angegebenen Datentyp umgewandelten Wert zurück, wenn die Umwandlung erfolgreich ist. Andernfalls wird NULL zurückgegeben.  
  
## <a name="remarks"></a>Bemerkungen  
 **TRY_CAST** versucht, den übergebenen Wert in den angegebenen *data_type* zu konvertieren. Wenn die Umwandlung erfolgreich ist, gibt **TRY_CAST** den Wert als angegebenen *data_type* zurück. Bei einem Fehler wird NULL zurückgegeben. Wenn Sie jedoch eine Konvertierung anfordern, die explizit nicht zulässig ist, verursacht **TRY_CAST** einen Fehler.  
  
 **TRY_CAST** ist kein neues reserviertes Schlüsselwort und in allen Kompatibilitätsgraden verfügbar. **TRY_CAST** verfügt beim Herstellen einer Verbindung mit Remoteservern über die gleiche Semantik wie **TRY_CONVERT** .  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-try_cast-returns-null"></a>A. TRY_CAST gibt NULL zurück.  
 Im folgenden Beispiel wird veranschaulicht, dass TRY_CAST NULL zurückgibt, wenn die Umwandlung fehlerhaft ist.  
  
```sql  
SELECT   
    CASE WHEN TRY_CAST('test' AS float) IS NULL   
    THEN 'Cast failed'  
    ELSE 'Cast succeeded'  
END AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
------------  
Cast failed  
  
(1 row(s) affected)  
```  
  
 Im folgenden Beispiel wird veranschaulicht, dass der Ausdruck das erwartete Format aufweisen muss.  
  
```sql  
SET DATEFORMAT dmy;  
SELECT TRY_CAST('12/31/2010' AS datetime2) AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------  
NULL  
  
(1 row(s) affected)  
```  
  
### <a name="b-try_cast-fails-with-an-error"></a>B. Bei TRY_CAST tritt ein Fehler auf.  
 Im folgenden Beispiel wird veranschaulicht, dass TRY_CAST einen Fehler zurückgibt, wenn die Umwandlung explizit nicht zulässig ist.  
  
```sql  
SELECT TRY_CAST(4 AS xml) AS Result;  
GO  
```  
  
 Das Ergebnis dieser Anweisung ist ein Fehler, da eine ganze Zahl nicht in einen XML-Datentyp umgewandelt werden kann.  
  
```  
Explicit conversion from data type int to xml is not allowed.  
```  
  
### <a name="c-try_cast-succeeds"></a>C. TRY_CAST ist erfolgreich.  
 In diesem Beispiel wird veranschaulicht, dass der Ausdruck das erwartete Format aufweisen muss.  
  
```sql
SET DATEFORMAT mdy;  
SELECT TRY_CAST('12/31/2010' AS datetime2) AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------------------  
2010-12-31 00:00:00.0000000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [TRY_CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/try-convert-transact-sql.md)   
 [CAST und CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  
