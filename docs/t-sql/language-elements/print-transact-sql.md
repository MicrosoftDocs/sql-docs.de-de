---
description: PRINT (Transact-SQL)
title: PRINT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PRINT_TSQL
- PRINT
dev_langs:
- TSQL
helpviewer_keywords:
- PRINT statement
- user-defined messages [SQL Server]
- messages [SQL Server], PRINT statement
- displaying user-defined messages
- viewing user-defined messages
- conditionally returning messages [SQL Server]
ms.assetid: 32ba0729-c4b5-4cfb-a5aa-e8b9402be028
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5950ce0b347eb4c1547b8524374c8845f1935f97
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342562"
---
# <a name="print-transact-sql"></a>PRINT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Gibt eine benutzerdefinierte Meldung an den Client zurück.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
PRINT msg_str | @local_variable | string_expr  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *msg_str*  
 Eine Zeichen- oder Unicode-Zeichenfolgenkonstante. Weitere Informationen finden Sie unter [Konstanten &#40;Transact-SQL&#41;](../../t-sql/data-types/constants-transact-sql.md).  
  
 **@** *lokale_Variable*  
 Dies ist eine Variable eines beliebigen gültigen Zeichendatentyps. **@** _local\_variable_ muss auf **char**, **nchar**, **varchar** oder **nvarchar** festgelegt oder implizit in diese Datentypen konvertierbar sein.  
  
 *string_expr*  
 Ein Ausdruck, der eine Zeichenfolge zurückgibt. Er kann verkettete Literalwerte, Funktionen und Variablen enthalten. Weitere Informationen finden Sie unter [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).  
  
## <a name="remarks"></a>Hinweise  
 Eine Meldungszeichenfolge kann bis zu 8.000 Zeichen lang sein, wenn es sich um eine Nicht-Unicode-Zeichenfolge handelt, und 4.000 Zeichen, wenn es sich um eine Unicode-Zeichenfolge handelt. Längere Zeichenfolgen werden abgeschnitten. Die Datentypen **varchar(max)** und **nvarchar(max)** werden abgeschnitten und ergeben Datentypen, die nicht größer sind als **varchar(8000)** und **nvarchar(4000)**.  
  
 RAISERROR kann auch zum Zurückgeben von Meldungen verwendet werden. RAISERROR hat im Vergleich zu PRINT die folgenden Vorteile:  
  
-   RAISERROR unterstützt das Ersetzen von Argumenten in eine Fehlermeldungs-Zeichenfolge. Dabei wird ein Mechanismus verwendet, der auf der printf-Funktion der Standardbibliothek der C-Programmiersprache modelliert wurde.  
  
-   RAISERROR kann neben der Textmeldung eine eindeutige Fehlernummer, einen Schweregrad und einen Statuscode angeben.  
  
-   Mit RAISERROR lassen sich mithilfe der gespeicherten Systemprozedur sp_addmessage benutzerdefinierte Meldungen zurückgeben.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-conditionally-executing-print-if-exists"></a>A. Bedingt ausgeführte PRINT-Anweisung (IF EXISTS)  
 Das folgende Beispiel verwendet die `PRINT`-Anweisung zur bedingten Rückgabe einer Meldung.  
  
```sql  
IF @@OPTIONS & 512 <> 0  
    PRINT N'This user has SET NOCOUNT turned ON.';  
ELSE  
    PRINT N'This user has SET NOCOUNT turned OFF.';  
GO  
```  
  
### <a name="b-building-and-displaying-a-string"></a>B. Erstellung und Anzeigen einer Zeichenfolge  
 Das folgende Beispiel konvertiert die Ergebnisse der `GETDATE`-Funktion in einen `nvarchar`-Datentyp und verkettet sie mit Literaltext. Das Ergebnis der Verkettung wird von `PRINT` zurückgegeben.  
  
```sql  
-- Build the message text by concatenating  
-- strings and expressions.  
PRINT N'This message was printed on '  
    + RTRIM(CAST(GETDATE() AS NVARCHAR(30)))  
    + N'.';  
GO  
-- This example shows building the message text  
-- in a variable and then passing it to PRINT.  
-- This was required in SQL Server 7.0 or earlier.  
DECLARE @PrintMessage NVARCHAR(50);  
SET @PrintMessage = N'This message was printed on '  
    + RTRIM(CAST(GETDATE() AS NVARCHAR(30)))  
    + N'.';  
PRINT @PrintMessage;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Beispiele: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] und [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-conditionally-executing-print"></a>C. Bedingt ausgeführte PRINT-Anweisung  
 Das folgende Beispiel verwendet die `PRINT`-Anweisung zur bedingten Rückgabe einer Meldung.  
  
```sql  
IF DB_ID() = 1  
    PRINT N'The current database is master.';  
ELSE  
    PRINT N'The current database is not master.';  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [DEKLARIEREN SIE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)  
  
  

